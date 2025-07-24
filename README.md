# Cabinet d'Avocat Maître Ben Arab - Site Web Professionnel

Ce projet est un site web professionnel multilingue (français, anglais, arabe) pour Maître Najla Ben Arab, avocate indépendante basée à Tunis.

## Table des matières

1. [Structure du projet](#structure-du-projet)
2. [Fonctionnalités principales](#fonctionnalités-principales)
3. [Technologies utilisées](#technologies-utilisées)
4. [Architecture du système](#architecture-du-système)
5. [Sécurité](#sécurité)
6. [Installation et déploiement](#installation-et-déploiement)
7. [Configuration](#configuration)
8. [Administration](#administration)
9. [Maintenance](#maintenance)
10. [Multilingue](#multilingue)
11. [Personnalisation](#personnalisation)
12. [Contacts et support](#contacts-et-support)

## Structure du projet

Le site comprend les pages suivantes:

### Pages publiques
- **Accueil** (index.html, index-en.html, index-ar.html) - Présentation générale du cabinet
- **À propos** (a-propos.html, about.html, about-ar.html) - Biographie et parcours professionnel
- **Domaines d'intervention** (domaines-intervention.html, practice-areas.html, practice-areas-ar.html) - Services juridiques proposés
- **Rendez-vous** (rendez-vous.php, appointment.php, appointment-ar.php) - Formulaire de prise de rendez-vous
- **Honoraires** (honoraires.html, fees.html, fees-ar.html) - Tarifs et modalités de paiement
- **Blog** (blog.php, blog-en.php, blog-ar.php) - Articles juridiques et actualités
- **Contact** (contact.php, contact-en.php, contact-ar.php) - Formulaire de contact et coordonnées
- **Politique de confidentialité** (politique-confidentialite.html, privacy-policy.html, privacy-policy-ar.html)

### Interface d'administration
- **Tableau de bord** (admin/index.php) - Vue d'ensemble des statistiques
- **Gestion des articles** (admin/posts.php, admin/post-new.php, admin/post-edit.php) - CRUD pour les articles du blog
- **Gestion des rendez-vous** (admin/appointments.php) - Gestion des demandes de rendez-vous
- **Gestion des messages** (admin/contact-messages.php) - Gestion des messages de contact
- **Profil** (admin/profile.php) - Gestion du profil administrateur

## Fonctionnalités principales

1. **Site multilingue** - Disponible en français, anglais et arabe avec adaptation RTL pour l'arabe
2. **Système de blog** - Publication d'articles juridiques avec catégories
3. **Système de prise de rendez-vous** - Formulaire avec vérification des disponibilités en temps réel
4. **Formulaire de contact** - Pour les demandes générales
5. **Panneau d'administration** - Interface sécurisée pour gérer le contenu du site
6. **Design responsive** - Adapté à tous les appareils (mobile, tablette, desktop)
7. **Optimisation SEO** - Structure et balises optimisées pour les moteurs de recherche

## Technologies utilisées

- **Frontend**:
  - HTML5
  - CSS3
  - JavaScript
  - Bootstrap 5
  - Font Awesome (icônes)
  - Google Fonts (Playfair Display et Roboto)

- **Backend**:
  - PHP 7.4+
  - SQLite (base de données)
  - PDO (PHP Data Objects)

## Architecture du système

### Base de données
Le système utilise SQLite pour stocker les données, avec les tables suivantes:
- `users` - Administrateurs du site
- `posts` - Articles du blog
- `categories` - Catégories des articles
- `appointments` - Rendez-vous clients
- `contact_messages` - Messages de contact

### Structure des fichiers
- `/` - Fichiers principaux du site
- `/admin` - Interface d'administration
- `/css` - Feuilles de style
- `/js` - Scripts JavaScript
- `/images` - Images et médias

## Sécurité

Le système implémente plusieurs mesures de sécurité avancées pour protéger les données et garantir l'intégrité du site:

### Authentification et autorisation
- Utilisation de mots de passe hachés avec Argon2id (algorithme recommandé OWASP)
- Paramètres de hachage renforcés: memory_cost=65536, time_cost=4, threads=3
- Protection contre la force brute avec délai d'attente après échec (sleep(1))
- Sessions sécurisées avec régénération d'ID (session_regenerate_id(true))
- Expiration automatique des sessions après inactivité (2 heures configurables)
- Vérification constante de l'état d'authentification via is_logged_in()
- Redirection automatique vers la page de connexion pour les accès non autorisés

### Protection contre les attaques web courantes
- Protection CSRF (Cross-Site Request Forgery):
  - Génération de jetons aléatoires via bin2hex(random_bytes(32))
  - Vérification des jetons avec hash_equals() (comparaison à temps constant)
  - Implémentation sur tous les formulaires (login, contact, rendez-vous)
- Protection XSS (Cross-Site Scripting):
  - Échappement systématique des données utilisateur via htmlspecialchars()
  - Fonction sanitize() pour nettoyer toutes les entrées
  - Utilisation de ENT_QUOTES pour échapper les guillemets simples et doubles
- Prévention des injections SQL:
  - Utilisation exclusive de requêtes préparées PDO
  - Paramètres liés pour toutes les requêtes dynamiques
  - Configuration PDO avec PDO::ATTR_ERRMODE à PDO::ERRMODE_EXCEPTION

### Sécurité des fichiers et uploads
- Validation stricte des types de fichiers lors de l'upload
- Génération de noms de fichiers aléatoires avec uniqid() pour éviter les écrasements
- Restriction des extensions autorisées (.jpg, .jpeg, .png, .gif)
- Vérification du type MIME réel des fichiers
- Restriction des permissions sur les dossiers d'upload (0755)
- Création sécurisée des dossiers d'upload si nécessaire

### Validation et traitement des données
- Filtrage des entrées avec filter_var() et les filtres PHP appropriés:
  - FILTER_SANITIZE_EMAIL pour les adresses email
  - FILTER_SANITIZE_FULL_SPECIAL_CHARS pour le texte général
  - FILTER_VALIDATE_EMAIL pour vérifier la validité des emails
- Validation côté serveur de toutes les données de formulaire
- Validation des dates pour les rendez-vous (empêche les dates passées)
- Vérification de disponibilité en temps réel des créneaux horaires

### Gestion des erreurs et journalisation
- Messages d'erreur génériques ne révélant pas d'informations sensibles
- Journalisation des erreurs dans des fichiers séparés
- Désactivation de l'affichage des erreurs PHP en production
- Gestion des exceptions avec try/catch pour éviter les fuites d'information

### Sécurité des communications
- Support complet pour HTTPS (recommandé pour le déploiement)
- En-têtes de sécurité pour les réponses HTTP:
  - Content-Type correctement défini
  - Pas d'exposition des versions de logiciels

### Bonnes pratiques additionnelles
- Principe du moindre privilège pour les accès à la base de données
- Séparation claire entre l'interface publique et l'administration
- Pas de stockage de données sensibles en clair
- Mécanisme de verrouillage de compte après tentatives échouées (configurable)
- Système de sauvegarde automatique de la base de données

## Installation et déploiement

1. **Prérequis**:
   - Serveur web (Apache, Nginx, etc.)
   - PHP 7.4 ou supérieur
   - Extensions PHP: PDO, PDO_SQLite, GD (pour les images)

2. **Installation**:
   ```bash
   # Cloner le dépôt ou télécharger les fichiers
   git clone [url-du-repo] maitre-ben-arab
   
   # Définir les permissions appropriées
   chmod 755 -R maitre-ben-arab
   chmod 777 -R maitre-ben-arab/admin/blog.db
   chmod 777 -R maitre-ben-arab/images/blog
   ```

3. 

## Configuration

### Paramètres généraux
Modifier le fichier `admin/config.php` pour ajuster:
- Nom du site
- URL du site
- Durée des sessions
- Chemins des dossiers

### Disponibilités des rendez-vous
Les créneaux de rendez-vous sont configurés dans `admin/database.php` avec:
- Horaires standards (8h-17h en période normale)
- Horaires d'été (8h-15h en juillet et août)
- Fermeture le weekend (samedi et dimanche)


### Fonctionnalités administratives
1. **Tableau de bord**:
   - Statistiques des rendez-vous
   - Messages non lus
   - Articles récents

2. **Gestion du blog**:
   - Création, édition et suppression d'articles
   - Gestion des catégories
   - Upload d'images

3. **Gestion des rendez-vous**:
   - Liste des rendez-vous
   - Modification du statut (en attente, confirmé, annulé, terminé)
   - Filtrage par date et statut

4. **Gestion des messages**:
   - Liste des messages de contact
   - Marquage comme lu/non lu
   - Suppression des messages

5. **Profil**:
   - Modification du mot de passe administrateur

## Maintenance

### Sauvegarde
Pour sauvegarder la base de données SQLite:
```bash
cp maitre-ben-arab/admin/blog.db maitre-ben-arab/admin/blog.db.backup
```

### Mise à jour
1. Sauvegarder la base de données et les fichiers modifiés
2. Remplacer les fichiers par la nouvelle version
3. Vérifier les permissions des dossiers

## Multilingue

Le site est disponible en trois langues:
- Français (par défaut)
- Anglais (suffixe -en dans les noms de fichiers)
- Arabe (suffixe -ar dans les noms de fichiers)

La détection de langue est basée sur l'URL et les redirections sont gérées automatiquement dans les scripts de traitement.

## Personnalisation

### Couleurs
Les couleurs principales sont définies dans les fichiers CSS (`css/style.css` et `css/style-rtl.css`):

```css
:root {
    --primary-color: #2c3e50;    /* Bleu marine foncé */
    --secondary-color: #c0a080;  /* Doré / beige */
    --accent-color: #e67e22;     /* Orange */
    --light-color: #f8f9fa;      /* Gris très clair */
    --dark-color: #343a40;       /* Gris foncé */
    --text-color: #333;          /* Noir texte */
}
```

### Polices
Le site utilise deux polices principales:
- Playfair Display pour les titres
- Roboto pour le texte courant

Ces polices sont importées via Google Fonts et peuvent être modifiées dans les fichiers CSS.

## Contacts et support

Pour toute question ou assistance concernant ce site web, veuillez contacter:

- **Email**: support@votre-agence.com
- **Téléphone**: +216 XX XXX XXX

---

© 2023 Cabinet d'avocat Maître Ben Arab. Tous droits réservés. 
