<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Rapport détaillé du dossier `app/` — mona-server

Description de la structure et du le rôle de chaque sous-dossier de `app/` dans mona-server.

---

## 1. Vue d'ensemble

| Dossier       | Rôle principal                                               |
| ------------- | ------------------------------------------------------------ |
| `Console/`    | Commandes Artisan et planification (schedule)                |
| `Exceptions/` | Gestion centralisée des exceptions HTTP                      |
| `Helpers/`    | Fonctions utilitaires (fichiers, photos)                     |
| `Http/`       | Contrôleurs, middleware, validation, ressources API          |
| `Jobs/`       | Tâches asynchrones (imports, nettoyage, mise à jour JSON)    |
| `Mail/`       | Classes d'envoi d'e-mails (réinitialisation mot de passe)    |
| `Models/`     | Modèles Eloquent et logique métier des entités               |
| `Providers/`  | Enregistrement des services et configuration des routes/vues |

---

## 2. `app/Console/`

### 2.1 `Console/Kernel.php`

- Définit les **commandes** chargées depuis `Commands/`.
- **Planification** : exécution mensuelle du job `Api\All` via `job:dispatch "Api\All"` (import global des données).

### 2.2 `Console/Commands/`

Commandes Artisan personnalisées (invoquées par `php artisan <signature>`).

| Fichier                      | Rôle                                                      |
| ---------------------------- | --------------------------------------------------------- |
| `DispatchJob.php`            | Lance un job (ex. `php artisan job:dispatch NOM_DU_JOB`). |
| `RestoreDatabase.php`        | Restauration de la base de données.                       |
| `MySqlDump.php`              | Création d'un dump MySQL.                                 |
| `CleansePhotos.php`          | Nettoyage / déduplication des photos.                     |
| `UpdateArtworksJSON.php`     | Mise à jour des fichiers JSON des œuvres.                 |
| `ImportDOIlesCommand.php`    | Import des données « Îles ».                              |
| `importPlaceJsonCommand.php` | Import des lieux à partir de JSON.                        |

_Note : fichiers `.CleansePhotos.php.swo` et `.swp` = artefacts éditeur (vim), à ignorer._

---

## 3. `app/Exceptions/`

### 3.1 `Handler.php`

- Hérite de `Illuminate\Foundation\Exceptions\Handler`.
- **`report()`** : envoi des exceptions vers les logs.
- **`render()`** : transformation des exceptions en réponses HTTP.
- **`$dontFlash`** : champs non renvoyés en session en cas d'erreur (ex. `password`, `password_confirmation`).

Comportement par défaut Laravel ; pas de personnalisation forte dans ce projet.

---

## 4. `app/Helpers/`

### 4.1 `FileHelpers.php`

Classe **autoloadée** via `composer.json` (`autoload.files`). Fonctions statiques utilitaires :

| Méthode                                | Rôle                                                                          |
| -------------------------------------- | ----------------------------------------------------------------------------- |
| `fileDiff($a, $b)`                     | Compare le contenu de deux fichiers (arrêt au premier octet différent).       |
| `platformSlashes($path)`               | Remplace `/` par `DIRECTORY_SEPARATOR`.                                       |
| `databaseSlashes($path)`               | Normalise les antislashs pour la base (slash `/`).                            |
| `isAlreadyUploaded($file, $files)`     | Détecte si un fichier identique (taille + contenu) existe déjà dans la liste. |
| `getAllPhotosFiles()`                  | Liste les fichiers dans `storage/app/public/photos`.                          |
| `getAllPhotosPaths()`                  | Liste les chemins relatifs des photos (Storage).                              |
| `getSiblingsSameTime($path, $seconds)` | Photos modifiées dans une fenêtre de ±`$seconds` autour de `$path`.           |
| `getSiblings($path)`                   | Toutes les photos sauf celle en `$path`.                                      |

Utilisées notamment pour la déduplication et la gestion des uploads de photos.

---

## 5. `app/Http/`

Cœur de la couche HTTP : chaque requête passe par le **Kernel** (middleware), puis un **Controller**, avec éventuellement **Requests** (validation) et **Resources** (format de réponse).

### 5.1 `Http/Kernel.php`

- **`$middleware`** (global) : CORS, mode maintenance, TrimStrings, TrustProxies, etc.
- **`$middlewareGroups`** :
  - **`web`** : cookies, session, CSRF, vues (routes classiques).
  - **`api`** : throttle (240 req/min), bindings, **`json`** (force `Accept: application/json`).
- **`$routeMiddleware`** (alias) :
  - `auth`, `auth.basic`, `guest`, `can`, `verified`, `signed`, `throttle`, `cache.headers`
  - **`api_version`** → `ApiVersion::class` (choix de version d'API v1/v2/v3).
  - **`json`** → `Jsonify::class`.
  - **`role`** → `CheckRole::class` (contrôle rôle admin).

### 5.2 `Http/Middleware/`

| Middleware                                  | Rôle                                                                                      |
| ------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `ApiVersion.php`                            | Reçoit un paramètre (ex. `v2`) et définit `config('app.api.version')` pour la requête.    |
| `Jsonify.php`                               | Force `Accept: application/json` sur toutes les requêtes du groupe `api`.                 |
| `Cors.php`                                  | Gestion des en-têtes CORS.                                                                |
| `CheckRole.php`                             | Vérifie `$request->user()->role == $role` (ex. `role:admin`) ; sinon redirection `/home`. |
| `Authenticate.php`                          | Redirige les non-authentifiés (web ou API).                                               |
| `EncryptCookies.php`, `VerifyCsrfToken.php` | Sécurité web (cookies, CSRF).                                                             |
| `TrimStrings.php`, `TrustProxies.php`, etc. | Comportements standards Laravel.                                                          |

### 5.3 `Http/Controllers/`

#### Controllers Admin (`Admin/`)

Interface d'administration (routes sous `admin/`, middleware `auth` + `role:admin`).

| Contrôleur                                             | Rôle                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| `AdminController.php`                                  | Contrôleur générique (peu utilisé d'après la doc).           |
| `ArtworkController.php`                                | CRUD / listing des œuvres.                                   |
| `ArtistController.php`                                 | Gestion des artistes.                                        |
| `PlaceController.php`                                  | Gestion des lieux.                                           |
| `HeritageController.php`                               | Gestion du patrimoine.                                       |
| `UserController.php`                                   | Gestion des utilisateurs.                                    |
| `PhotoController.php`                                  | Gestion des photos.                                          |
| `DiscoveryController.php`                              | Gestion des « découvertes ».                                 |
| `TagPhotoController.php`, `TagDiscoveryController.php` | Association tags ↔ photos / découvertes.                     |
| `AdjustmentController.php`                             | Consultation des ajustements (historique des modifications). |
| `DeletionController.php`                               | Gestion des suppressions.                                    |
| `ChiffresClesController.php`                           | Tableau de bord / chiffres clés.                             |
| `BadgeController.php`                                  | Gestion des badges.                                          |
| `MenuController.php`                                   | Logique du menu admin.                                       |

#### Controllers API (`Api/`)

Une arborescence par **version** : V1, V2, V3.

- **V1** : routes en grande partie commentées ; `LoadJson`, `LoginController`, `RegisterController`, `UserController` (notes, commentaires, photos).
- **V2** : API principale en lecture + auth + profil utilisateur :
  - Ressources : `Artwork`, `Artist`, `Place`, `Heritage`, `Badge` (index/show).
  - `LastUpdatedArtworks`, `LastUpdatedPlaces`, `LastUpdatedHeritages`.
  - Auth : login, register, logout.
  - Sous `user` (auth requise) : show, `user/artworks`, `user/places`, `user/heritages`, updatePassword, updateUsername, updateEmail.
  - `CountController` : techniques, materials, categories, subcategories.
- **V3** : évolution de V2 avec :
  - Même structure de ressources + `LODExportController` (export LOD).
  - Documentation : `DocumentationController` → `doc/artworks`, `doc/artists`, etc.
  - Réinitialisation mot de passe : `PasswordResetController` (email, reset, setNewPassword).
  - `UserPhotoController` : `user/photo` (get).

#### Controllers Auth (`Auth/`)

Auth Laravel classique (web) : `LoginController`, `RegisterController`, `ForgotPasswordController`, `ResetPasswordController`, `VerificationController`.

### 5.4 `Http/Requests/`

Form Requests pour la **validation** des requêtes API (utilisateur authentifié).

| Request                                                                                      | Usage typique            |
| -------------------------------------------------------------------------------------------- | ------------------------ |
| `StoreArtworkUserRequest`, `UpdateArtworkUserRequest`, `ShowAndDestroyArtworkUserRequest`    | Liaison user ↔ artwork.  |
| `StorePlaceUserRequest`, `UpdatePlaceUserRequest`, `ShowAndDestroyPlaceUserRequest`          | Liaison user ↔ place.    |
| `StoreHeritageUserRequest`, `UpdateHeritageUserRequest`, `ShowAndDestroyHeritageUserRequest` | Liaison user ↔ heritage. |

### 5.5 `Http/Resources/`

Transformateurs JSON (format de réponse API par version).

- **`UserResource.php`** : commun.
- **V1** : `ArtistResource`, `ArtworkResource`, `MultiLanguageResource`.
- **V2** : `ArtistResource`, `ArtworkResource`, `BadgeResource`, `HeritageResource`, `PlaceResource`.
- **V3** : idem V2 (même noms, éventuelles différences de champs ou de structure).

Les contrôleurs API appellent ces resources pour renvoyer des réponses JSON homogènes.

---

## 6. `app/Jobs/`

Tâches asynchrones (implémentent `ShouldQueue`) : **imports de données**, **nettoyage**, **mise à jour de JSON**. Structure reflétant les **versions d'API** et les **sources de données**.

### 6.1 Jobs racine (legacy / génériques)

- `GetArtworks.php`, `ImportArtwork.php`, `ImportMural.php`, `ImportCollection.php`
- `ImportPlace.php`, `ImportRimouskiArtworks.php`

### 6.2 `Jobs/Api/`

- **`All.php`** : job « global » (appelé mensuellement par le schedule).
- **`ImportAll.php`**, **`Cleansing.php`**, **`UpdateJson.php`** : orchestration ou utilitaires.

### 6.3 `Jobs/Api/V1/`

- `ImportAll.php`
- `Artwork/ImportAll.php`, `ImportArtwork.php`, `ImportMural.php`

### 6.4 `Jobs/Api/V2/`

- **Artwork** : ImportAll, ImportMONA, ImportDonneesOuvertesMontreal, ImportDonneesOuvertesRimouski, ImportMuralesMontreal, ImportArtPublicMontreal, ImportStorageCollections.
- **Place** : ImportAll, ImportBibliBanQ, ImportLieuxCulturels.
- **Heritage** : ImportAll, ImportHeritages.
- **Badge** : ImportAll, ImportBadges.
- **Location** : ImportAll, ImportBoroughs, ImportTerritories.

### 6.5 `Jobs/Api/V3/`

Version la plus riche : **Cleansing** et **UpdateJson** par domaine.

- **Artwork** : ImportAll, Cleansing, UpdateJson, ImportMONA, ImportDoMtl, ImportDoRim, ImportDoIles, ImportArtPubMtl, ImportArtPubLaval, ImportMU, ImportMurSubMtl, ImportDoseCult, ImportUdeM.
- **Place** : ImportAll, Cleansing, UpdateJson, ImportBAnQ, ImportLieuCult, ImportEqCult, updateBibliData.
- **Heritage** : ImportAll, Cleansing, UpdateJson, ImportMONA, ImportAreas, ImportRPCQ, ImportSiLoi, ImportSiMuni, ImportSiMCC, ImportSiMuni, ImportImMCC, ImportImMuni.
- **Badge** : ImportAll, Cleansing, UpdateJson, ImportBadges.

Sources couvertes : Montréal, Rimouski, Laval, BAnQ, MU, UdeM, Données ouvertes (Mtl, Rim, Îles), RPCQ, MCC, municipal, etc.

---

## 7. `app/Mail/`

### 7.1 `ResetPassword.php`

Mailable pour l'e-mail de **réinitialisation de mot de passe** : utilise la vue `emails.reset`. Utilisé par le flux « mot de passe oublié » (web ou API selon les routes).

---

## 8. `app/Models/`

Modèles Eloquent : **tables**, **relations**, **traits**, **scopes**. Organisation par **domaine métier** en sous-dossiers.

### 8.1 Modèles « transversaux »

- **`User.php`** : utilisateur (username, email, password, role, api_token). Relations : `artworks`, `places`, `heritages` (pivot avec notes/favoris, etc.).
- **`Version.php`** : gestion de version (données ou schéma).
- **`CategoryHistory.php`** : historique de catégories.
- **`HasDuplicates.php`** : trait pour modèles avec doublons (original_id, duplicates).

### 8.2 `Models/Adjustment/`

- **`Adjustment.php`** : table des « ajustements » (qui a modifié quoi : user_id, adjustable_id, adjustable_type, before, after). Relation `morphTo` vers le modèle modifié.
- **`Adjustable.php`** : trait pour les modèles qui enregistrent leurs modifications (Artwork, Place, Heritage). Sur `updating`/`deleting`, enregistre un enregistrement dans `adjustments` avec diff avant/après.

### 8.3 `Models/Artwork/`

Domaine **œuvres d'art**.

| Modèle                                                       | Rôle                                                                                                                                                                                                              |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Artwork.php`                                                | Œuvre : titre (fr/en), source, localisation (spatial), catégorie, collection, artistes, matériaux, techniques, dimensions, etc. Utilise `SpatialTrait`, `SoftDeletes`, `Adjustable`, `Taggable`, `HasDuplicates`. |
| `Artist.php`                                                 | Artiste (nom, alias, collectif).                                                                                                                                                                                  |
| `Category.php`, `Subcategory.php`, `CategoryHistory`         | Catégories / sous-catégories.                                                                                                                                                                                     |
| `Collection.php`, `Owner.php`, `Producer.php`, `Funder.php`  | Collection, propriétaire, producteur, financeur.                                                                                                                                                                  |
| `Material.php`, `Technique.php`, `Medium.php`, `Support.php` | Matériaux, techniques, supports.                                                                                                                                                                                  |
| `Dimension.php`                                              | Dimensions.                                                                                                                                                                                                       |
| `Accessibility.php`                                          | Accessibilité.                                                                                                                                                                                                    |
| `ArtworkUser.php`                                            | Pivot user ↔ artwork (notes, favoris, etc.).                                                                                                                                                                      |

### 8.4 `Models/Place/`

- **`Place.php`** : lieu (titre, source, location spatiale, address, description, category, borough, territory). Traits : SpatialTrait, SoftDeletes, Adjustable, Taggable, HasDuplicates.
- **`Usage.php`** : type d'usage du lieu.
- **`PlaceUser.php`** : pivot user ↔ place.

### 8.5 `Models/Heritage/`

- **`Heritage.php`** : élément patrimonial (titre, source, url, location, area, dates, synthesis, description, status, etc.). Mêmes traits que Place pour spatial, soft delete, adjustments, tags, duplicates.
- **`Address.php`**, **`Subusage.php`** : adresses, sous-usages.
- **`HeritageUser.php`** : pivot user ↔ heritage.

### 8.6 `Models/Badge/`

- **`Badge.php`** : badge (ex. « Patrimoine », « Art public »).
- **`Badgeable.php`** : trait pour relation polymorphique (artwork, place, heritage peuvent avoir des badges).

### 8.7 `Models/Location/`

- **`Borough.php`** : arrondissement.
- **`Territory.php`** : territoire (ex. Montréal, Rimouski).

### 8.8 `Models/Tag/`

- **`Tag.php`** : libellé de tag.
- **`Taggable.php`** : trait pour modèles taggables (morphToMany).
- **`TagArtwork.php`, `TagPlace.php`, `TagHeritage.php`, `TagPhoto.php`** : pivots ou configurations de relation.

### 8.9 `Models/Photo/`

- **`Duplicatable.php`**, **`DuplicatePhoto.php`**, **`LostPhoto.php`** : gestion des photos et de la déduplication / perte.

---

## 9. `app/Providers/`

### 9.1 `AppServiceProvider.php`

- **`boot()`** : `Schema::defaultStringLength(191)` ; `URL::forceRootUrl(config('app.url'))` ; définition d'une Gate `viewApiDocs` (actuellement toujours true).

### 9.2 `AuthServiceProvider.php`

Politiques d'autorisation (policies) si définies.

### 9.3 `RouteServiceProvider.php`

- **`map()`** : enregistre les routes API puis web.
- **API** : pour chaque version (1, 2, 3), charge `routes/api/v{version}/api.php` avec préfixe `api/v{version}` et middlewares `api` + `api_version:v{version}`.
- **API par défaut** : préfixe `api` (sans numéro) → même fichier que la version `config('app.api_latest', 1)`.

### 9.4 `ViewServiceProvider.php`

Partage de données vers les vues ; `boot()` vide dans l'état actuel.

### 9.5 `BroadcastServiceProvider.php`, `EventServiceProvider.php`

Broadcasting et événements Laravel (non détaillés ici).

---

## 10. Synthèse des flux

1. **Requête API** : `RouteServiceProvider` choisit le fichier de routes selon l'URL (`/api`, `/api/v2`, `/api/v3`) → `Kernel` applique `api` + `api_version` → contrôleur API (V1/V2/V3) → Model + Resource → JSON.
2. **Requête Admin** : `web.php` → middleware `auth` + `role:admin` → contrôleur Admin → vues Blade/Vue.
3. **Données** : Jobs (Artisan ou schedule) exécutent les imports V2/V3, cleansing et UpdateJson ; les modèles Artwork, Place, Heritage, Badge, etc. sont alimentés et exposés via l'API.

---
