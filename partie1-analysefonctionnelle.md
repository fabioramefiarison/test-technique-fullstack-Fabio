# Partie 1 : Analyse Fonctionnelle  
## Cas métier : Application de calcul automatisé pour la broderie

---

## 1. Contexte Métier

Un client souhaite développer une application permettant d'automatiser le calcul des surfaces à broder à partir d'une image.

L'utilisateur :
1. Fournit une image (PNG/JPG)
2. Définit les dimensions réelles de broderie (ex : largeur en cm)
3. Obtient :
   - La surface par couleur
   - Une estimation de quantité de fil
   - Une estimation du temps et/ou du coût

---

## 2. Illustration du Processus

### Exemple d’image à broder

![Exemple image pixelisée](https://upload.wikimedia.org/wikipedia/commons/7/70/Pixel_art_example.png)

*(Source : Wikimedia Commons – Exemple de pixel art)*

---

### Étape 1 : Analyse des couleurs (Color Clustering)

Principe : regrouper les pixels par proximité de couleur.

Illustration du regroupement de couleurs :

![Clustering KMeans](https://upload.wikimedia.org/wikipedia/commons/e/ea/K-means_convergence.gif)

*(Source : Wikimedia Commons – Visualisation K-Means)*

Objectif :
- Réduire les milliers de nuances possibles
- Ne conserver que les couleurs réellement brodables

---

### Étape 2 : Calcul des surfaces

Principe :
- Compter le nombre de pixels par couleur
- Convertir en surface réelle

Formule :

Surface réelle (cm²) =  
(nombre de pixels couleur / nombre total de pixels) × surface totale réelle

---

### Étape 3 : Estimation métier (fil / temps / coût)

Exemple de logique métier :

- 10 cm² de remplissage = 4 mètres de fil
- Vitesse machine = 800 points/minute
- 1 cm² = 400 points

Illustration d’une machine à broder :

![Machine à broder](https://upload.wikimedia.org/wikipedia/commons/4/4c/Embroidery_machine.jpg)

*(Source : Wikimedia Commons – Machine industrielle de broderie)*

---

## 3. Décomposition Fonctionnelle

### Briques principales :

### 1️⃣ Import & Prétraitement
- Upload image
- Redimensionnement
- Lissage (réduction du bruit)

### 2️⃣ Analyse de la palette
- Extraction des couleurs dominantes
- Quantification (ex: K-Means)

### 3️⃣ Calcul des surfaces
- Comptage pixels par couleur
- Conversion pixel → cm²

### 4️⃣ Moteur de calcul métier
- Application des ratios techniques
- Estimation du fil
- Estimation du temps
- Estimation du coût

### 5️⃣ Restitution
- Tableau récapitulatif :

| Couleur | Surface (cm²) | Fil estimé (m) | Temps estimé | Coût estimé |
|----------|---------------|---------------|--------------|------------|

---

## 4. Modèle de Données

| Entité | Champs | Rôle |
|--------|--------|------|
| Projet | id, nom, image_url, largeur_cm, hauteur_cm | Conteneur principal |
| Couleur | id, projet_id, code_hex | Couleur détectée |
| Surface | couleur_id, nb_pixels, surface_cm2 | Résultat calcul |
| Configuration | densite_points, vitesse_machine, cout_fil_metre | Paramètres métier |

---

## 5. Risques & Solutions

| Risque | Impact | Solution |
|--------|--------|----------|
| Dégradés trop nombreux | Explosion du nombre de couleurs | Réduction palette (K-Means) |
| Image HD trop lourde | Temps de calcul élevé | Downscale automatique |
| Bruit numérique | Surfaces imprécises | Filtre médian / flou |
| Pas d’échelle réelle | Impossible de calculer coût | Dimension obligatoire |

---

## 6. Résultat attendu

L’application doit fournir :

✔ Surface par couleur  
✔ Quantité de fil estimée  
✔ Temps machine estimé  
✔ Estimation du coût total  
