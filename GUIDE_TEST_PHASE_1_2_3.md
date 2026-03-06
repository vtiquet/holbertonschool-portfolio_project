# Guide de Test - Phases 1, 2 & 3
## Module de Prêts d'Équipements Médicaux

**Date**: 24 février 2026  
**Branche**: `DevVal`  
**Environment**: Docker Development  
**URL**: http://localhost:8000/domain/loans/dashboard/

---

## 🎯 Vue d'Ensemble des Tests

| Phase | Fonctionnalité | Statut | Tests |
|-------|---------------|--------|-------|
| **Phase 1** | Propriété `new_equipment_serial` | ✅ Implémenté | 3 tests |
| **Phase 2** | Quick Action Panel (Redesign) | ✅ Redesign complet | 10 tests |
| **Phase 3** | Chevauchement calendrier + Dashboard | ✅ Implémenté | 6 tests |

**Total**: 19 tests à exécuter

---

## � Corrections Appliquées (2 mars 2026)

### Bugs Corrigés:

1. **✅ Fonction `quickMarkAsReturned()` manquante**
   - **Problème**: Les boutons "Retourné" ne fonctionnaient pas (Quick Action Panel)
   - **Solution**: Ajout de la fonction JavaScript qui appelle `/domain/loans/loan/${loanId}/quick-mark-returned/`
   - **Fichier**: `dashboard.html` (lignes 1066-1107)

2. **✅ Bouton "Contacter" supprimé**
   - **Problème**: Bouton inutile dans "Prêts en Retard" et "Actions Rapides"
   - **Solution**: Suppression du bouton "Contacter" des deux sections
   - **Fichiers**: 
     - `dashboard.html` ligne 197 (table Prêts en Retard)
     - `dashboard.html` ligne 943 (Quick Action Panel - overdue)

3. **✅ Boutons Modal vérifiés**
   - **Problème**: "Marquer Retourné" et "Signaler Défaut" dans la modal
   - **Solution**: Les fonctions existent et appellent les bons endpoints
   - **Fichier**: `loan_modal_detail.html` (fonctions déjà présentes)
   - **Endpoints**: 
     - `/domain/loans/loan/${loanId}/quick-mark-returned/` ✅
     - `/domain/loans/loan/${loanId}/quick-report-defect/` ✅

### À Re-tester:

- [ ] **Test 2.3**: Confirmer départ d'un prêt
- [ ] **Test 2.4**: Reporter un prêt
- [ ] **Test 2.6**: Marquer équipement défectueux
- [ ] Boutons "Retourné" dans Quick Action Panel
- [ ] Boutons "Défectueux" dans Quick Action Panel
- [ ] BoutonsModal: "Marquer Retourné" et "Signaler Défaut"

---

## �📋 Prérequis

### Données de Test Requises

Pour tester efficacement, assurez-vous d'avoir:

1. **Équipements**:
   - Au moins 3 équipements du même type (ex: "Tensiomètre")
   - Au moins 1 équipement avec un numéro de série
   - Au moins 1 équipement sans numéro de série

2. **Patients**:
   - Au moins 3 patients actifs

3. **Prêts pour Phase 2**:
   - 1 prêt démarrant dans les 2 heures (statut: `reserved`)
   - 1 prêt démarrant dans 12 heures (statut: `reserved`)
   - 1 prêt se terminant dans 3 heures (statut: `active`)
   - 1 prêt se terminant dans 20 heures (statut: `active`)
   - 1 prêt en retard (date retour passée, statut: `active`)

4. **Prêts pour Phase 3**:
   - 2-3 prêts superposés sur le même créneau horaire
   - Même jour, même heure de début, équipements différents

### Commandes de Préparation

```bash
# 1. Naviguer vers le projet
cd /Users/vtiquet/Desktop/Cardiobot/CardioBot

# 2. Vérifier que les containers tournent
docker-compose ps

# 3. Accéder au shell Django pour créer des données de test
docker-compose exec web python manage.py shell

# 4. Dans le shell Python, créer des prêts de test:
from domain.loans.domain.models.loan import Loan
from domain.loans.domain.models.equipment import Equipment
from domain.patients.domain.models.patient import Patient
from datetime import datetime, timedelta
from django.utils import timezone

# Exemple: Créer un prêt démarrant dans 2 heures
patient = Patient.objects.first()
equipment = Equipment.objects.filter(status='available').first()
loan_date = timezone.now() + timedelta(hours=2)
return_date = loan_date + timedelta(hours=24)

loan = Loan.objects.create(
    patient=patient,
    equipment=equipment,
    loan_date=loan_date,
    expected_return_date=return_date,
    status='reserved'
)
print(f"✅ Prêt créé: ID={loan.id}")

# Répéter pour créer d'autres prêts avec différents timings
```

---

## 🧪 PHASE 1: Propriété `new_equipment_serial`

### Contexte
Ajout d'une propriété calculée pour afficher le numéro de série d'un équipement de remplacement lors d'un conflit.

### Fichier Modifié
- `django/domain/loans/domain/models/conflict.py` (lignes 123-141)

---

### ✅ Test 1.1: Propriété existe et retourne None par défaut

**Étapes**:
1. Accéder au shell Django:
   ```bash
   docker-compose exec web python manage.py shell
   ```

2. Exécuter dans le shell:
   ```python
   from domain.loans.domain.models.conflict import ConflictResolution
   
   # Trouver une résolution de conflit existante (ou en créer une)
   conflict = ConflictResolution.objects.first()
   
   if conflict:
       print(f"Conflit ID: {conflict.id}")
       print(f"Resolution Type: {conflict.resolution_type}")
       print(f"New Equipment ID: {conflict.new_equipment_id}")
       print(f"New Equipment Serial: {conflict.new_equipment_serial}")
   else:
       print("⚠️ Aucun conflit trouvé - créez-en un pour tester")
   ```

**Résultat Attendu**:
- ✅ La propriété `new_equipment_serial` existe
- ✅ Retourne `None` si `new_equipment_id` est `None`
- ✅ Aucune erreur AttributeError

---

### ✅ Test 1.2: Propriété retourne le serial si équipement de remplacement existe

**Étapes**:
1. Dans le shell Django:
   ```python
   from domain.loans.domain.models.conflict import ConflictResolution
   from domain.loans.domain.models.equipment import Equipment
   from domain.loans.domain.models.loan import Loan
   
   # Trouver un équipement avec serial number
   equipment = Equipment.objects.exclude(serial_number__isnull=True).exclude(serial_number='').first()
   
   if equipment:
       print(f"✅ Équipement trouvé: {equipment.name}")
       print(f"   Serial: {equipment.serial_number}")
       
       # Créer ou trouver une résolution avec cet équipement
       # (Dans un vrai test, cela serait créé via l'interface)
       conflict = ConflictResolution.objects.filter(
           new_equipment_id=equipment.id
       ).first()
       
       if conflict:
           print(f"Conflit ID: {conflict.id}")
           print(f"New Equipment Serial: {conflict.new_equipment_serial}")
           print(f"Match: {conflict.new_equipment_serial == equipment.serial_number}")
       else:
           print("⚠️ Pas de conflit avec cet équipement - test manuel requis")
   else:
       print("⚠️ Aucun équipement avec serial number - ajoutez-en un!")
   ```

**Résultat Attendu**:
- ✅ `new_equipment_serial` retourne le numéro de série de l'équipement
- ✅ La valeur correspond à `Equipment.serial_number`

---

### ✅ Test 1.3: Propriété gère les cas où l'équipement n'existe plus

**Étapes**:
1. Dans le shell Django:
   ```python
   from domain.loans.domain.models.conflict import ConflictResolution
   
   # Créer manuellement un conflit avec un ID d'équipement invalide
   # (Ceci simule un équipement supprimé)
   from domain.loans.domain.models.loan import Loan
   
   # Note: Test théorique - normalement protected par foreign key
   # Vérifier que la propriété ne crash pas si accès invalide
   
   conflict = ConflictResolution.objects.first()
   if conflict:
       # Forcer temporairement un ID invalide (dangereux - test uniquement)
       original_id = conflict.new_equipment_id
       try:
           conflict.new_equipment_id = 999999  # ID inexistant
           result = conflict.new_equipment_serial  # Devrait gérer l'erreur
           print(f"✅ Résultat: {result}")
           print("✅ Pas de crash - gestion d'erreur OK")
       except Exception as e:
           print(f"❌ Erreur: {e}")
       finally:
           conflict.new_equipment_id = original_id  # Restaurer
   ```

**Résultat Attendu**:
- ✅ Pas de crash même si l'équipement n'existe pas
- ✅ Retourne `None` ou gère l'erreur gracieusement

---

## ⚡ PHASE 2: Quick Action Panel (Redesign 24h)

### Contexte
Panneau d'actions rapides redesigné avec:
- Fenêtre de 24 heures (au lieu de 30min/2hr)
- Thème bleu (au lieu de violet)
- Bouton "Défectueux" pour retours
- Actions persistantes pour retards
- Correction erreurs SafeString

### Fichiers Modifiés
- `django/domain/loans/interfaces/web/views.py` (lignes 88-119, 1713-1950)
- `django/domain/loans/templates/loans/dashboard.html` (lignes 780-1067)
- `django/domain/loans/static/loans/css/p-loan-dashboard.css` (lignes 881-1051)
- `django/domain/loans/interfaces/web/urls.py` (ligne 56)

---

### ✅ Test 2.1: Panneau visible avec style bleu

**Étapes**:
1. Ouvrir le dashboard: http://localhost:8000/domain/loans/dashboard/
2. Vérifier la présence du Quick Action Panel en haut de page
3. Inspecter les styles CSS (DevTools)

**Résultat Attendu**:
- ✅ Panneau visible avec dégradé bleu (`#4299e1` → `#3182ce`)
- ✅ Badge avec nombre d'actions affichées
- ✅ Style cohérent avec Calendrier des Prêts
- ❌ Plus de violet (`#667eea`, `#764ba2`)

**Screenshot**: Le panneau doit avoir le même style bleu que la navigation

---

### ✅ Test 2.2: Fenêtre de 24 heures - Prêts démarrant bientôt

**Prérequis**: Créer 3 prêts:
- 1 prêt dans 1 heure (statut: `reserved`)
- 1 prêt dans 12 heures (statut: `reserved`)
- 1 prêt dans 30 heures (statut: `reserved`)

**Étapes**:
1. Rafraîchir le dashboard
2. Compter le nombre de cartes "Confirmer le départ du prêt"
3. Vérifier l'affichage du temps

**Résultat Attendu**:
- ✅ 2 prêts affichés (1h et 12h)
- ❌ Le prêt dans 30h n'apparaît PAS (hors fenêtre 24h)
- ✅ Temps affiché correctement:
  - "Dans 1 heure" ou "Dans 60 min"
  - "Dans 12 heures"
- ✅ Priorité "urgent" (rouge) pour prêt < 1h
- ✅ Priorité "warning" (orange) pour prêt > 1h

---

### ✅ Test 2.3: Confirmer départ d'un prêt (SafeString FIXED)

**Prérequis**: Un prêt avec statut `reserved` démarrant bientôt

**Étapes**:
1. Identifier un prêt dans le panneau avec bouton "Confirmer le départ"
2. Cliquer sur le bouton "Confirmer le départ"
3. Confirmer dans la popup
4. Observer le toast de succès
5. Vérifier que la page se rafraîchit

**Résultat Attendu**:
- ✅ Pas d'erreur SafeString dans la console
- ✅ Toast: "Prêt confirmé - Statut mis à jour"
- ✅ Statut du prêt passe à `active`
- ✅ Date `actual_start_date` enregistrée
- ✅ Note ajoutée: `[CONFIRMATION 2026-02-24 XX:XX] Départ confirmé par...`
- ✅ Le prêt disparaît du Quick Action Panel

**Vérification Backend**:
```bash
docker-compose exec web python manage.py shell
```
```python
from domain.loans.domain.models.loan import Loan
loan = Loan.objects.get(id=XXX)  # Remplacer XXX par l'ID
print(f"Status: {loan.status}")
print(f"Actual start: {loan.actual_start_date}")
print(f"Notes:\n{loan.notes}")
```

---

### ✅ Test 2.4: Reporter un prêt (SafeString FIXED)

**Prérequis**: Un prêt avec statut `reserved` démarrant bientôt

**Étapes**:
1. Cliquer sur "Reporter" pour un prêt
2. Entrer "60" minutes dans le prompt
3. Confirmer
4. Observer le résultat

**Résultat Attendu**:
- ✅ Pas d'erreur SafeString
- ✅ Toast: "Prêt reporté de 60 minutes"
- ✅ `loan_date` et `expected_return_date` décalées de 60 minutes
- ✅ Note ajoutée: `[REPORT 2026-02-24 XX:XX] Prêt reporté de 60 minutes par...`
- ✅ Page se rafraîchit et affiche le nouveau timing

---

### ✅ Test 2.5: Fenêtre de 24 heures - Prêts se terminant bientôt

**Prérequis**: Créer 3 prêts actifs:
- 1 prêt terminant dans 2 heures
- 1 prêt terminant dans 18 heures
- 1 prêt terminant dans 28 heures

**Étapes**:
1. Rafraîchir le dashboard
2. Compter les cartes "Préparer le retour"

**Résultat Attendu**:
- ✅ 2 prêts affichés (2h et 18h)
- ❌ Le prêt dans 28h n'apparaît PAS
- ✅ Boutons affichés:
  - "Retour OK"
  - "Défectueux" (NOUVEAU)
  - "Prolonger"

---

### ✅ Test 2.6: Marquer équipement défectueux (NOUVEAU)

**Prérequis**: Un prêt actif se terminant bientôt ou en retard

**Étapes**:
1. Cliquer sur le bouton "Défectueux" (rouge)
2. Entrer une description: "Bouton START cassé"
3. Confirmer le popup
4. Vérifier le résultat

**Résultat Attendu**:
- ✅ Prompt demande description du problème
- ✅ Toast: "Équipement marqué comme défectueux - Mis hors service"
- ✅ Statut du prêt → `returned`
- ✅ Statut de l'équipement → `defective`
- ✅ Note ajoutée:
  ```
  [DÉFECTUEUX 2026-02-24 XX:XX] Équipement marqué défectueux par...
  Raison: Bouton START cassé
  ```
- ✅ Page se rafraîchit
- ✅ L'équipement n'apparaît plus comme disponible

**Vérification Backend**:
```python
from domain.loans.domain.models.equipment import Equipment
equipment = Equipment.objects.get(id=XXX)
print(f"Status: {equipment.status}")  # Doit être 'defective'
```

---

### ✅ Test 2.7: Actions persistantes - Prêts en retard

**Prérequis**: Un prêt avec `expected_return_date` dans le passé et statut `active`

**Étapes**:
1. Ouvrir le dashboard
2. Identifier les cartes "RETARD - Action requise"
3. Attendre 2-3 minutes (ou forcer un refresh)
4. Vérifier que la carte reste visible

**Résultat Attendu**:
- ✅ Carte affichée avec priorité "urgent" (bordure rouge)
- ✅ Badge "EN RETARD" avec icône `exclamation-circle`
- ✅ Boutons disponibles:
  - "Contacter" (gris)
  - "Retourné" (cyan)
  - "Défectueux" (rouge)
- ✅ La carte reste affichée même après refresh (persistante)
- ✅ Ne disparaît QUE quand action manuelle effectuée

---

### ✅ Test 2.8: Auto-refresh toutes les 60 secondes

**Étapes**:
1. Ouvrir le dashboard
2. Noter l'heure actuelle et le contenu du panneau
3. Ouvrir la console développeur (F12)
4. Attendre 60 secondes
5. Observer les requêtes réseau

**Résultat Attendu**:
- ✅ Pas de reload complet de page
- ✅ Fonction `loadQuickActions()` appelée toutes les 60 secondes
- ✅ Contenu du panneau mis à jour dynamiquement
- ✅ Pas d'erreurs dans la console

---

### ✅ Test 2.9: Panneau caché si aucune action

**Prérequis**: Aucun prêt dans les 24 prochaines heures, aucun retard

**Étapes**:
1. Résoudre tous les prêts actifs
2. Supprimer les prêts à venir (ou les décaler > 24h)
3. Rafraîchir le dashboard

**Résultat Attendu**:
- ✅ Panneau a la classe CSS `hidden`
- ✅ Message: "Aucune action urgente pour le moment"
- ✅ Icône check-circle verte
- ✅ Le reste du dashboard reste fonctionnel

---

### ✅ Test 2.10: Gestion d'erreurs backend

**Étapes**:
1. Ouvrir DevTools → Network
2. Tenter de confirmer un prêt déjà confirmé
3. Tenter de reporter un prêt invalide (ID inexistant)

**Résultat Attendu**:
- ✅ Toast d'erreur affiché avec message clair
- ✅ Pas de crash de l'application
- ✅ Console montre le traceback (en mode DEBUG)
- ✅ Codes HTTP appropriés (400, 404, 500)

---

## 📅 PHASE 3: Chevauchement + Dashboard

### Contexte
Améliorations du calendrier et du dashboard:
- Détection chevauchement de prêts sur calendrier
- Affichage côte-à-côte avec colonnes
- Contraintes max-width sur cartes dashboard
- Masquage types d'équipements vides
- Tri par disponibilité

### Fichiers Modifiés
- `django/domain/loans/static/loans/js/loan-calendar-grid.js` (lignes 234-342)
- `django/domain/loans/static/loans/css/loan-calendar-grid.css` (lignes 667-726)
- `django/domain/loans/static/loans/css/03-generic/g-loans-unified-layout.css` (lignes 126-143)
- `django/domain/loans/static/loans/css/p-loan-dashboard.css` (lignes 415-432)
- `django/domain/loans/interfaces/web/views.py` (lignes 121-154)

---

### ✅ Test 3.1: Chevauchement - Deux prêts côte-à-côte

**Prérequis**: Créer 2 prêts qui se chevauchent:
```python
# Dans le shell Django
from domain.loans.domain.models.loan import Loan
from domain.patients.domain.models.patient import Patient
from domain.loans.domain.models.equipment import Equipment
from datetime import datetime, timedelta
from django.utils import timezone

# Même créneau, équipements différents
base_time = timezone.now().replace(hour=14, minute=0, second=0, microsecond=0)
patient1 = Patient.objects.all()[0]
patient2 = Patient.objects.all()[1]
equipment1 = Equipment.objects.filter(status='available')[0]
equipment2 = Equipment.objects.filter(status='available')[1]

loan1 = Loan.objects.create(
    patient=patient1,
    equipment=equipment1,
    loan_date=base_time,
    expected_return_date=base_time + timedelta(hours=2),
    status='reserved'
)

loan2 = Loan.objects.create(
    patient=patient2,
    equipment=equipment2,
    loan_date=base_time,
    expected_return_date=base_time + timedelta(hours=2),
    status='reserved'
)
```

**Étapes**:
1. Ouvrir le calendrier: http://localhost:8000/domain/loans/calendar/
2. Naviguer vers la date des prêts créés
3. Observer l'affichage sur la grille horaire

**Résultat Attendu**:
- ✅ Les 2 prêts apparaissent côte-à-côte
- ✅ Classe CSS `.loan-appointment--column-0` sur le premier
- ✅ Classe CSS `.loan-appointment--column-1` sur le second
- ✅ Largeur: 48% chacun (avec gap)
- ✅ Pas de chevauchement visuel
- ✅ Les deux cartes sont lisibles

---

### ✅ Test 3.2: Comportement hover - Pas d'expansion si chevauchement

**Prérequis**: Même setup que Test 3.1 (2 prêts côte-à-côte)

**Étapes**:
1. Survoler le premier prêt avec la souris
2. Observer la largeur et le z-index
3. Survoler le second prêt
4. Observer le comportement

**Résultat Attendu**:
- ✅ Au hover: légère élévation (z-index augmente)
- ❌ Pas d'expansion en largeur (resterait à 48%)
- ✅ Les autres prêts restent visibles
- ✅ Transition smooth

---

### ✅ Test 3.3: Chevauchement - Trois prêts ou plus

**Prérequis**: Créer 3 prêts au même moment

**Étapes**:
1. Créer 3 prêts identiques au Test 3.1 (même créneau)
2. Rafraîchir le calendrier
3. Observer l'affichage

**Résultat Attendu**:
- ✅ 3 prêts affichés côte-à-côte
- ✅ Classes: `.loan-appointment--column-0`, `--column-1`, `--column-2`
- ✅ Largeur: ~31% chacun
- ✅ Tous les textes restent lisibles
- ✅ Responsive: réduction progressive de la taille de police

---

### ✅ Test 3.4: Dashboard - Contraintes max-width sur cartes

**Étapes**:
1. Ouvrir le dashboard
2. Redimensionner la fenêtre à différentes largeurs:
   - Petit écran: 1024px
   - Écran moyen: 1440px
   - Grand écran: 1920px
3. Observer la largeur des cartes dashboard

**Résultat Attendu**:
- ✅ Écran < 1200px: cartes 100% width (1 par ligne)
- ✅ Écran ≥ 1200px: cartes max 600px
- ✅ Pas de cartes trop larges sur grand écran
- ✅ Grid layout: 2-3 colonnes selon l'espace disponible
- ✅ Espacement uniforme entre les cartes

**CSS à vérifier**:
```css
@media (min-width: 1200px) {
    .unified-loans-grid__card {
        max-width: 600px;
    }
}
```

---

### ✅ Test 3.5: Dashboard - Masquage types d'équipements vides

**Prérequis**:
- Avoir 2+ types d'équipements
- Au moins 1 type sans équipements disponibles

**Étapes**:
1. Ouvrir le dashboard
2. Scroller vers la section "Inventaire par type"
3. Compter les cartes affichées

**Résultat Attendu**:
- ✅ Seuls les types avec équipements (disponibles OU en prêt) sont affichés
- ❌ Les types totalement vides ne sont PAS affichés
- ✅ Message si aucun équipement: "Aucun équipement trouvé"

**Vérification code** (`views.py` ligne 140):
```python
if category_data['available_count'] == 0 and category_data['loaned_count'] == 0:
    continue  # Skip empty categories
```

---

### ✅ Test 3.6: Dashboard - Tri par disponibilité

**Prérequis**:
- Plusieurs types d'équipements avec disponibilités différentes

**Étapes**:
1. Ouvrir le dashboard
2. Observer l'ordre des cartes dans "Inventaire par type"
3. Noter la disponibilité de chaque type

**Résultat Attendu**:
- ✅ Types triés par disponibilité décroissante
- ✅ Types avec plus d'équipements disponibles en premier
- ✅ Types sans disponibilité à la fin
- ✅ Badge "X disponibles / Y total" correct pour chaque type

**Exemple attendu**:
```
1. Tensiomètre (5 disponibles / 7 total)
2. Glucomètre (3 disponibles / 4 total)
3. ECG (0 disponibles / 2 total)
```

---

## 🔍 Tests de Régression

### ✅ Régression 1: Calendrier - Prêts isolés non affectés

**Étapes**:
1. Créer un prêt isolé (pas de chevauchement)
2. Vérifier l'affichage sur le calendrier

**Résultat Attendu**:
- ✅ Prêt affiché normalement (100% width du slot)
- ✅ Pas de classe `--column-X`
- ✅ Hover fonctionne normalement
- ✅ Pas de régression visuelle

---

### ✅ Régression 2: Dashboard - Navigation et filtres

**Étapes**:
1. Tester tous les boutons du dashboard
2. Filtrer par statut (Réservé, En cours, etc.)
3. Utiliser les boutons rapides

**Résultat Attendu**:
- ✅ Tous les filtres fonctionnent
- ✅ Boutons "Créer un prêt", "Voir calendrier" OK
- ✅ Pas de régression sur fonctionnalités existantes

---

### ✅ Régression 3: Performance - Chargement du dashboard

**Étapes**:
1. Ouvrir DevTools → Network
2. Vider le cache
3. Charger le dashboard
4. Mesurer le temps de chargement

**Résultat Attendu**:
- ✅ Page charge en < 2 secondes (avec données)
- ✅ Pas de requêtes en double
- ✅ CSS et JS minifiés en production
- ✅ Pas d'erreurs 404 dans Network

---

## 📊 Résumé des Tests

### Check-list Globale

**Phase 1 - new_equipment_serial**:
- [✅] Test 1.1: Propriété existe
- [✅] Test 1.2: Retourne serial si équipement existe
- [✅] Test 1.3: Gère les cas d'erreur

**Phase 2 - Quick Action Panel**:
- [✅] Test 2.1: Style bleu visible
- [✅] Test 2.2: Fenêtre 24h départs
- [✅] Test 2.3: Confirmer départ (SafeString fix)
- [✅] Test 2.4: Reporter prêt (SafeString fix)
- [✅] Test 2.5: Fenêtre 24h retours
- [✅] Test 2.6: Marquer défectueux (NOUVEAU)
- [✅] Test 2.7: Actions persistantes retards
- [✅] Test 2.8: Auto-refresh 60s
- [✅] Test 2.9: Panneau caché si vide
- [✅] Test 2.10: Gestion erreurs

**Phase 3 - Calendrier & Dashboard**:
- [✅] Test 3.1: Deux prêts côte-à-côte
- [✅] Test 3.2: Hover sans expansion
- [✅] Test 3.3: Trois prêts ou plus
- [✅] Test 3.4: Max-width cartes dashboard
- [✅] Test 3.5: Types vides masqués
- [✅] Test 3.6: Tri par disponibilité

**Tests de Régression**:
- [ ] Régression 1: Prêts isolés OK
- [ ] Régression 2: Navigation OK
- [ ] Régression 3: Performance OK

---

## 🐛 Rapport de Bugs

Si vous rencontrez des problèmes, noter:

1. **Test échoué**: Numéro du test
2. **Comportement observé**: Ce qui se passe réellement
3. **Comportement attendu**: Ce qui devrait se passer
4. **Erreurs console**: Copier les erreurs JavaScript/Python
5. **Screenshot**: Si applicable
6. **Environnement**: Navigateur, résolution écran

### Template de Rapport

```markdown
### Bug #X - [Titre court]

**Test**: Test X.X
**Sévérité**: Critique / Majeure / Mineure
**Date**: 24/02/2026

**Description**:
[Ce qui ne fonctionne pas]

**Reproduction**:
1. Étape 1
2. Étape 2
3. Résultat observé

**Erreur Console**:
```
[Copier l'erreur ici]
```

**Screenshot**: [Image si nécessaire]

**Suggestion Fix**: [Si applicable]
```

