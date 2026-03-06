---
title: Suivi du projet
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Suivi de projet

<!-- > :bulb: Cette page documente l’évolution du projet dans le temps.
> Elle sert à rendre visibles les décisions, ajustements et apprentissages.
> Les entrées peuvent être hebdomadaires ou bi-hebdomadaires.
> N'oubliez pas d’effacer ou de mettre en commentaires les notes (`>`) avant la remise finale. -->

---

## **Semaine 1 (12–18 janvier)**

### Objectifs de la période

- Prendre connaissance du projet MONA et de son contexte
- Comprendre l’organisation de l’équipe et les outils utilisés
- S’approprier l’application et les ressources existantes

### Travail réalisé

<!-- !!! abstract "Avancement" - [x] Analyse de solutions existantes - Comparaison de trois outils similaires - [x] Prototype basse fidélité (Figma) - [ ] Validation utilisateur - Reportée à la semaine suivante -->

- Création de la page de suivi afin de consigner les rapports hebdomadaires
- Prise de connaissance de l’historique et de la mission de MONA (lecture du blogue et des rapports existants)
- Découverte et test de l’application MONA en conditions réelles (utilisation sur le terrain)
- Intégration aux outils de communication et de collaboration du projet (Groupe Element, pCloud, Dépôt Git, Figma, MarkDown, TestFlight)
- Rencontre avec Lena pour comprendre le fonctionnement général du projet et de l’équipe
- Rencontre avec Simon pour une présentation du fonctionnement du serveur
- Participation à une réunion avec l’équipe de design et découverte de nouvelles propositions de design

<!-- ### Décisions et ajustements

> À compléter uniquement si des choix structurants ont été faits
> ou si l’orientation du projet a évolué.

!!! info "Décisions" - Abandon de l’approche X jugée trop complexe - Reformulation de la problématique suite aux premières analyses

### Difficultés rencontrées

> À compléter uniquement si des obstacles ont eu un impact réel
> sur l’avancement du projet.

!!! warning "Difficultés" - Problème de configuration du plugin Mermaid - Confusion entre `mkdocs-mermaid2-plugin` (pip)
et `mermaid2` (nom du plugin) - Résolu après nettoyage et configuration correcte dans `mkdocs.yml` -->

<hr style="border: 0; height: 4px; background-color: #F7EFA2;">

## **Semaine 2 (19–25 janvier)**

### Objectifs de la période

- Effectuer l'installation des outils et logiciels nécessaires
- Comprendre l’organisation de l’équipe et les outils utilisés
- Explorer le processus ETL en quoi il pourrait nous servir au sein du projet

### Travail réalisé

<!-- !!! abstract "Avancement" - [x] Analyse de solutions existantes - Comparaison de trois outils similaires - [x] Prototype basse fidélité (Figma) - [ ] Validation utilisateur - Reportée à la semaine suivante -->

- Compléter la page de suivi pour cette semaine concernant les rapports hebdomadaires
- Participation à une réunion avec l’équipe (Weekly Meeting)
- Lecture de la documentation concernant l'architecture du projet sur le Wiki.
- Effectuer l'installation ainsi que la configuration de l'environnement de développement
- Familiarisation avec la librérie Laravel
- Participation à une réunion avec Corélie pour qu'elle puisse m'expliquer l'API **v3** (version la plus récente)
- Recherche sur le modèle ETL et comment les outils OpenRefine ainsi que Apache Airflow peuvent opérer au niveau du processus

Après avoir effectuer ma rechecher j'ai pu organiser mes idées comme suit :

<div style="position: relative; width: 100%; margin: 2rem auto;">
<p style="text-align:center; font-style: italic; margin-top: -1rem;" onclick="window.open('../pdfs/ETL_process_num1.pdf','_blank')">
    Cliquer ici pour ouvrir en plein écran
</p>
<object
        data="../pdfs/ETL_process_num1.pdf"
        type="application/pdf"
        width="100%"
        height="550">
</object>
</div>

<hr style="border: 0; height: 4px; background-color: #F7EFA2;">

## **Semaine 3 (26 janvier - 1er février)**

### Objectifs de la période

- Continuer la recherche sur le processus ETL et son intégration avec le projet MONA
- Trouver des avis sur les deux outils Apache Airflow ainsi que OpenRefine
- Planifier une rencontre avec Simon pour apprendre comment effectuer un déploiement
- Générer la clé ssh publique et la transmettre à Lena pour avoir accès au serveur

### Travail réalisé

<!-- !!! abstract "Avancement" - [x] Analyse de solutions existantes - Comparaison de trois outils similaires - [x] Prototype basse fidélité (Figma) - [ ] Validation utilisateur - Reportée à la semaine suivante -->

- Participation à une réunion avec l’équipe (Weekly Meeting)

- Génération de la clé publique SSH afin d'avoir accès au serveur :
  <ol>
    <li>Il faut d'abord s'assurer que OpenSSH soit installé (Windows 11) : <code>ssh -v</code></li>
    <li>Pour générer sa clé publique il faut taper : <code>ssh-keygen -t ed25519 -C "ton_email@example.com"</code></li>
    <li>Appuyer sur Entrée pour accepter l'emplacement par défaut</li>
    <li>Choisir une passphrase (optionnel)</li>
    <li>Pour récupérer la clé publique : <code>cat ~/.ssh/id_ed25519.pub</code></li>
  </ol>

- Participation à une réunion avec Simon pour me montrer comment effectuer un déploiement au niveau de l'environnement de staging (dev) :
  <ul>
    <li> *Bonne pratique* : créer une branche sur GitHub pour chaque tâche</li>
    <li>
      Étapes du déploiement :
      <ol>
        <li>Dans le terminal taper : <code>ssh picasso</code></li>
        <li>Introduire le mot de passe de la configuration</li>
        <li>
          Accéder au bon repo — pour l’environnement de staging :
          <code>ls docker/dev/mona-server/</code>
        </li>
        <li>Effectuer un <code>git pull</code></li>
      </ol>
    </li>
  </ul>

- J'ai exploré cette semaine quels étaient les avantages et inconvénients de l'utilisation du processus ETL au sein du projet MONA :

  <div style="position: relative; width: 100%; margin: 2rem auto;">
  <p style="text-align:center; font-style: italic; margin-top: -1rem;" onclick="window.open('../pdfs/etl_process_num2.pdf','_blank')">
    Cliquer ici pour ouvrir en plein écran
  </p>

  <object data="../pdfs/etl_process_num2.pdf" type="application/pdf" width="100%" height="550"></object>

</div>

- _Feedback lors de la réunion hebdomadaire_ : le schéma n’explique pas clairement de quelle façon les deux outils seront utilisés dans le backend pour la migration des données. Je dois donc me familiariser davantage avec le code existant et déterminer comment et où intégrer les packages du processus ETL déjà présent dans Laravel notamment ceux mentionnés dans le premier schéma, puisque l’idée d’utiliser Airflow a été écartée en raison de sa complexité

<hr style="border: 0; height: 4px; background-color: #F7EFA2;">

## **Semaine 4 (02–08 février)**

### Objectifs de la période

- IMPORTANT : Comprendre et se familiariser avec le code côté serveur
- Répondre aux questions suivantes :
  <ol>

  <li> Comment intégrer OpenRefine dans le workfow actuel ? </li>
  <li> Est-ce que le format exporté de OpenRefine correspond à nos besoins?</li>

  </ol>

### Travail réalisé

- Participation à une réunion avec l’équipe (Weekly Meeting)
- Participation à une réunion avec Christian, durant laquelle il m’a présenté une vue d’ensemble du développement mobile, la librairie utilisée (Vue.js) et le processus de release, qui était le sujet principal de notre rencontre. En résumé, une fois la release destinée aux tests effectuée, la mise en production se fait facilement, que ce soit sur l’Apple App Store (iOS) ou sur le Google Play Store (Android).
- Familiarisation et compréhension de mona-server :

**Architecture du projet**

    - app/ : Contient la logique métier de l'application (Modèles, Controllers, Services)

    - routes/ : Définit les routes API et web

    - database/ : Migrations de la base de données

    - resources/ : Vues (Blade, Vue.js) et assets (CSS, JS)

    - public/ : Point d'entrée web de l'application (fichiers publics)

    - config/ : Fichiers de configuration Laravel

    - storage/ : Fichiers générés : logs, cache, uploads

    - tests/ : Tests unitaires et fonctionnels

    - documentation/ : Documentation du projet

- **Vue d'ensemble du dossier app/**

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

**NOTE** : Plus de détails concernant les sous dossiers de app/ sont disponibles ici [Détails](details-app-folders.md)

**Architecture à plusieurs niveaux**
Le projet suit une architecture multi-niveaux :

     - Couche de présentation : Interface web (Vue.js + Blade)
     - Couche de service : API REST (Laravel) — version actuelle : v3 (Hiver 2026)
     - Couche de données : Base de données MySQL

**Fichiers importants**

      - composer.json : Dépendances PHP
      - package.json : Dépendances JavaScript
      - docker-compose.yml : Configuration Docker
      - .env : Variables d'environnement
      - artisan : CLI Laravel pour les commandes de développement

- **Packages ETL et import de données pour Laravel**

**1. Packages ETL (Extract – Transform – Load)**

**marquine/php-etl**

- **Packagist :** `marquine/php-etl`
- Très utilisé (132k+ installs). API fluide pour enchaîner extract → transform → load.
- Intégration Laravel (ServiceProvider), support SQLite, MySQL, PostgreSQL.
- Exemple : `Job::start()->extract('csv', 'file.csv')->transform('trim', ['columns' => ['name']])->load('table', 'users')`.

**LaravelPlus/etl-manifesto**

- **GitHub :** [LaravelPlus/etl-manifesto](https://github.com/laravelplus/etl-manifesto)
- ETL « Laravel-native » déclaratif : tout se définit en **YAML** (sources, transformations, destinations).
- Pas besoin d’écrire de requêtes ou de scripts d’export ; adapté si tu veux des pipelines déclaratifs.

**2. Packages d’import (proches ETL / flux de données)**

**spatie/simple-excel**

- **Packagist :** `spatie/simple-excel`
- Très répandu (millions d’installs). Lecture/écriture Excel et CSV, API simple.
- Utile pour l’« extract » et un peu de « transform » avant d’alimenter ta base (load fait à la main avec Eloquent/DB).
- Compatible Laravel 9–12, PHP 8.3+ sur les versions récentes.

  **laravel-enso/data-import**

- **GitHub :** [laravel-enso/data-import](https://github.com/laravel-enso/data-import)
- Import **XLSX** (box/spout) avec templates, validation, notifications.
- Plutôt « import structuré » que ETL complet, mais couvre bien extract + transform + validation.

  **giftonian/massive-csv-import**

- Import **CSV** avec beaucoup de lignes, basé sur les **Laravel Queues**.
- Adapté aux gros fichiers (millions de lignes), moins « ETL générique » qu’un vrai moteur ETL.

**3. Synthèse**

| Besoin                                                | Package à regarder                                             |
| ----------------------------------------------------- | -------------------------------------------------------------- |
| ETL complet (extract + transform + load) avec API PHP | **marquine/php-etl**                                           |
| ETL déclaratif en YAML, intégré Laravel               | **LaravelPlus/etl-manifesto**                                  |
| Lire/écrire Excel/CSV simplement                      | **spatie/simple-excel**                                        |
| Import XLSX avec validation et templates              | **laravel-enso/data-import**                                   |
| Gros CSV en file d’attente                            | **massive-csv-import** (ou jobs custom comme dans mona-server) |

---

Dans **mona-server**, les imports sont faits avec des **Jobs Laravel** personnalisés (sans package ETL). Si on veut introduire un ETL (sources multiples, transformations réutilisables, config déclarative), **php-etl** ou **etl-manifesto** sont les plus proches ; pour seulement lire des CSV/Excel avant de les insérer en base, **spatie/simple-excel** suffit souvent.

<hr style="border: 0; height: 4px; background-color: #F7EFA2;">

## **Semaine 5 (09–15 février)**

### Objectifs de la période

- Continuer les tâches de la semaine précédante.

### Travail réalisé

- J'ai eu une réunion avec Simon durant laquelle il m'a expliqué comment les migrations fonctionnaient et comment notre base de données est peuplée. En explorant les dossiers du **mona-server** je ne comprenais pas trop pourquoi dans le dossier **database** on avait seulement 3 fichiers de migrations et surtout comment nos données sont importer dans notre base de données. Simon m'a expliqué qu'il devrait y avoir un dossier migrations sur Github non disponible pour l'instant (qu'il va le push bientôt) et qui donc contient tout les fichiers nécessaires pour la création/modification de nos tables. Pour ce qui est de l'importation des données, pour remplir nos tables pour la 1ère fois après que le serveur soit actif en local :

       - on lance le serveur dans le fond :  `./vendor/laravel/sail/bin/sail up -d`
       - on utilise ces commandes : `./vendor/laravel/sail/bin/sail artisan migrate` ensuite `./vendor/laravel/sail/bin/sail mysql < 01-07-2024.sql`

- J'ai également installé dbeaver (n'importe quel autre logiciel fera l'affaire) pour mieux visualiser nos données car je trouvais que c'était un peu compliqué depuis le terminal. Les identifiants pour se connecter à la base de données se trouvent dans le fichier **.env** . Une fois ceci est fait on devrait voir ceci :

  ![dbeaver View](assets/dbeaver-view.png){style="display:block; margin: 0 auto; width:85%; height:100%;"}

- Une des questions posée à Simon était pourquoi ma page une fois que je lance le serveur ne charge pas le styling, et on s'est donc rendu compte que y'avait un bug quelque part.
  ![dbeaver View](assets/home-view-nonFixed.png){style="display:block; margin: 0 auto; width:85%; height:100%;"}

Simon m'a alors fait part de ces deux fichiers : **.env** et **docker-compose.yml**
En comparant mes fichiers et les siens, il n'y avait pas de différences. J'ai donc fait mes recherches et j'ai trouvé que le problème était bel et bien dans le fichier des variables d'environements **.env** :

Changer le APP_URL de : `APP_URL=http://localhost` à `APP_URL=http://localhost:8080`

Concernant le logo, j'ai trouvé que dans le code le nom de l'image utilisé est `Mona-Logo.svg` alors que dans le projet c'est `logo.svg`. J’ai donc simplement changé le nom de l’image au lieu de le modifier partout dans le code.

- J'ai lu le mémoire de Simon, et voici ce que je retiens :
- Retour sur les deux questions :
  <ol>

  <li> Comment intégrer OpenRefine dans le workfow actuel ? </li>
  <li> Est-ce que le format exporté de OpenRefine correspond à nos besoins?</li>

  </ol>

  Y'a plusieurs versions exportées par OpenRefine notamment les fichiers CSV, ou des SQL Exporter ce qui correspond à aux formats utilisés dans MOna-server, sauf que je ne pense pas qu'il nous serait d'une grande utilité vu que les corrections que l'on veut apporter au niveau de nos données, on les a à présent sous forme SQL. Conclusion, à mon avis, à part pour détecter les doublons ce logiciel ne nous servira pas à grand chose.

<hr style="border: 0; height: 4px; background-color: #F7EFA2;">

## **Semaine 6 (16–22 février)**

### Objectifs de la période

- Avoir une réunion avec Simon pour qu'il me partage le fichier de patch de corrections SQL.

### Travail réalisé

- Cette semaine je devais avoir une réunion avec Simon pour voir à quoi ressemble le patch de corrections ainsi que pour m'expliquer comment le tout fonctionne mais il ne se sentait pas bien donc il ne pouvait pas se connecter et m'envoyer le fichier, par conséquent la réunion n'a pas eu lieu. En plus, j'avais mes examens intra cette semaine aussi alors je n'ai pas pu avancé sur le projet.

<hr style="border: 0; height: 4px; background-color: #F7EFA2;">

## **Semaine 7 (23 février - 1er mars)**

### Objectifs de la période

- Comprendre le patch de corrections

### Travail réalisé

- Une fois que Simon m'a envoyé le fichier de corrections SQL, j'ai pu voir à quoi ça ressamblait. Voici ce que je retiens :

      *Contexte*

      Le document « Exemple d’un fichier de corrections » décrit le fonctionnement du pipeline ETL utilisé dans le projet MONA pour intégrer des données provenant de différentes sources culturelles (par exemple UdeM) dans la base de données finale. Le pipeline suit la structure classique **EXTRACT – TRANSFORM – LOAD (ETL)**.

      Dans un premier temps, les données sources sont importées dans des **tables temporaires**. Ensuite, des **fichiers SQL de corrections** sont appliqués afin d’aligner les données avec le schéma de la base MONA. Finalement, certaines colonnes sont copiées vers la base de données finale.

      ---

      *Extraction des données (EXTRACT)*

      Chaque source de données est décrite dans un fichier de configuration contenant plusieurs propriétés :

      * le nom de la source
      * le type de fichier (CSV, JSON, etc.)
      * l’emplacement du fichier (URL ou chemin local)
      * le schéma de la table temporaire

      Ce schéma permet de créer automatiquement une table temporaire dans laquelle les données brutes sont importées.

      ---

      *Transformation des données (TRANSFORM)*

      Les fichiers de corrections constituent l’étape principale du traitement. Chaque fichier est divisé en trois sections :

      1. **Alignement des schémas**
      2. **Réconciliation des données**
      3. **Corrections ciblées**

      *Alignement des schémas* :

      Cette étape vise à faire correspondre les colonnes des sources avec la structure de la base de données MONA. Plusieurs transformations peuvent être appliquées :

      * ajout de colonnes pour indiquer la provenance des données (ex. `source`, `source_id`)
      * conversion de formats de données (par exemple transformer une date texte en format SQL)
      * normalisation de certaines valeurs (ex. dimensions)
      * transformation des coordonnées géographiques en objets spatiaux
      * création d’un nom d’artiste à partir du prénom et du nom

      Ces transformations permettent d’obtenir une structure compatible avec la base de données cible.

      *Réconciliation des données* :

      Une fois les colonnes alignées, il est nécessaire de faire correspondre les **lignes** avec les identifiants internes de la base MONA. Cette étape consiste à attribuer des identifiants tels que :

      * l’ID MONA de l’œuvre
      * l’ID MONA de l’artiste
      * éventuellement un identifiant externe (ex. Wikidata)

      Cela permet d’éviter les doublons lorsque plusieurs sources décrivent la même œuvre.

      *Corrections ciblées* :

      Cette dernière section permet d’appliquer des corrections spécifiques identifiées par les expertes du domaine (historiennes de l’art). Par exemple, certaines catégories ou informations peuvent être corrigées manuellement pour des œuvres particulières.

      ---



      *Conclusion*

      Le fichier de corrections joue un rôle central dans l’intégration des données au sein du pipeline ETL du projet MONA. Il permet de transformer les données brutes provenant de diverses sources afin de les rendre compatibles avec le schéma de la base de données finale. Bien que cette approche soit efficace pour gérer des sources hétérogènes, elle repose fortement sur des transformations manuelles en SQL, ce qui peut poser des défis en termes de maintenance et d’évolutivité.

<hr style="border: 0; height: 4px; background-color: #F7EFA2;">

## **Semaine 8 ( 02 - 08 mars)**

### Objectifs de la période

- Apprendre à utiliser Curl et Postman
- Comprendre + apprendre à utiliser l'API
- Explorer l'interface admin
- Executer des requêtes HTTP pour

### Travail réalisé

- **Utilisation de Postman et Curl**

Postman et Curl permettent d’envoyer des requêtes HTTP vers une API et d’observer les réponses retournées par le serveur. Cela permet notamment de :

  <ol> 
   <li> vérifier que les endpoints de l’API fonctionnent correctement </li>
   <li>tester l’envoi ou la modification de données </li>
   <li>analyser les réponses retournées par le serveur</li>
   <li>déboguer certaines opérations liées à l’importation ou à la correction de données</li>
  </ol>

- **Comparaison des deux outils**

Les deux outils permettent d’effectuer les mêmes types de requêtes HTTP, mais ils sont utilisés dans des contextes différents :

Postman est plus adapté pour explorer une API et tester des requêtes de manière interactive.

Curl est plus léger et plus adapté à l’automatisation ou à l’exécution de requêtes directement dans le terminal.

-- Dans le cadre du projet MONA, selon moi Postman est particulièrement utile pour tester et valider les interactions avec l’API,
