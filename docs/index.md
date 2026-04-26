---
title: Vue d'ensemble du projet
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Vue d'ensemble du projet

!!! info "Informations générales"
**Session**: Hiver 2026  
 **Auteur(s)**: Anissa Ould ferroukh<!-- Nom de chaque membre (matricule)  -->  
 **Thème(s)**: Médiation culturelle, valorisation de l’art public, innovation culturelle et numérique<!-- Thèmes principaux abordés dans le projet  -->  
 **Superviseur(s)**: Guy Lapalme<!-- Nom du superviseur (affiliation)  -->  
 **Collaborateur(s):** Maison MONA et partenaires culturels/communautaires<!-- Nom de(s) collaborateur(s) et partenaire(s)` -->

## Description du projet

<!-- > :bulb: N'oubliez pas d'effacer ou mettre en commentaires les notes (`>`) en début de section  -->

### Contexte

<!-- > Présentez le contexte général dans lequel s’inscrit votre projet (social, organisationnel, technologique, éducatif, environnemental, etc.). -->

![Texte alternatif](assets/logo.jpg){style="display:block; margin: 0 auto; width:30%; height:40%;"}

Le projet s’inscrit dans le contexte de la valorisation du patrimoine culturel et artistique de Montréal à travers des expériences interactives en milieu urbain. La Maison MONA est un organisme à but non lucratif fondé en 2020 qui vise à rendre l’art et le patrimoine accessibles à un large public, notamment par l’entremise d’outils numériques (application mobile) et de parcours de médiation culturelle dans l’espace public.

[MONA](https://monamontreal.org/) collabore avec des établissements scolaires, des institutions culturelles et des organismes communautaires pour proposer des activités participatives qui valorisent œuvres d’art, lieux patrimoniaux et diversité culturelle.

### Problématique

<!-- > Décrivez le problème central ou la question de recherche que votre projet cherche à adresser, pourquoi s'y intéresser et les faiblesses des solutions actuelles.
> Le problème doit pouvoir être compris indépendamment de la solution envisagée. -->

MONA maintient une base de données fédérée alimentée par plusieurs sources externes (données gouvernementales, fichiers CSV, Wikidata, etc.). Ces sources évoluent dans le temps, ce qui oblige MONA à réimporter régulièrement l’ensemble des données afin de refléter les mises à jour. Cependant, la réimportation complète des données pose un problème majeur :

- Les corrections humaines (alignements, réconciliations, fusions, corrections éditoriales) que les historiennes ont ressorties et qui ont donc été appliquées vont être écrasées.

- Les modifications qui sont effectuées via l’API v4 (UPDATE, CREATE, DELETE) vont apparaître immédiatement au niveau de la base de données mais ne sont pas systématiquement enregistrées comme règles persistantes de corrections donc encore une fois les données corrigées vont être écrasées.

- Il existe donc une dissociation entre les corrections SQL historiques et les modifications faites via l’API.

Le défi principal est donc de lier les corrections et modifications faites au niveau de l'API avec ceux du processus d'importation et donc avoir un système cohérent où toutes les corrections, sont persistées et automatiquement rejouées lors des imports.

### Proposition et objectifs

<!-- > Présentez votre proposition de projet et les objectifs visés. Expliquez en quoi votre approche répond à la problématique identifiée.
> Assurez-vous d'avoir, dans la mesure du possible, des objectifs mesurables, raisonnnables dans le temps et non redondants entre eux. -->

L’objectif du projet est de renforcer l’architecture technique de MONA en assurant la cohérence entre :

- Le système d’importation

- Le système de corrections persistantes

- L’API

Les objectifs spécifiques sont :

- Concevoir un mécanisme permettant d’enregistrer les corrections effectuées via l’API v4 dans le système officiel de corrections.

- Unifier les différents canaux de correction (SQL, API) en une seule couche persistante.

- Garantir que les réimportations complètes des sources n’écrasent pas les corrections validées.

- Mettre en place un pipeline reproductible et traçable.

- Améliorer la maintenabilité et la robustesse de l’architecture des données.

### Méthodologie

<!-- > Expliquez comment vous comptez aborder le projet : démarche générale, grandes étapes prévues, itérations, types de validations envisagées. -->

La méthodologie adoptée repose sur une approche d’analyse technique et d’amélioration architecturale d’un système existant.

Les principales étapes sont :

- Analyse du fonctionnement actuel du pipeline d’importation (ETL) et du système de corrections SQL.

- Étude des nouvelles fonctionnalités CRUD/REST de l’API v4.

- Identification des incohérences entre modifications API et corrections persistantes.

- Conception d’un mécanisme unifié où toute modification sur des données fédérées devient une règle persistante.

- Implémentation d’un lien entre les opérations UPDATE de l’API et le système de corrections.

- Validation du fonctionnement lors d’un cycle complet de réimportation.

Cette démarche vise à assurer la cohérence des données dans un contexte de fédération multi-sources, tout en minimisant le travail répétitif des expertes humaines.

### Validation et Évaluation

<!-- > Indiquez comment vous évaluerez que votre solution répond aux objectifs du projet (ex. scénarios d’usage, tests, retours utilisateurs, indicateurs qualitatifs ou quantitatifs). -->

L’évaluation du travail reposera sur des critères techniques et fonctionnels permettant de vérifier que l’architecture proposée répond efficacement aux enjeux de pérennité et de cohérence des données.

Les principaux éléments de validation seront :

- **Reproductibilité des données**: Vérifier qu’après une réimportation complète des sources, les corrections enregistrées (via SQL ou via API v4) sont correctement réappliquées.

- **Intégration API ↔ Système de corrections**: Tester que toute modification effectuée via les opérations CRUD de l’API v4 (notamment UPDATE) génère automatiquement une règle persistante intégrée au pipeline ETL.

- **Non-régression des données** : S’assurer qu’aucune correction validée ne soit perdue lors des cycles successifs d’importation.

- **Cohérence des fusions et relations** : Vérifier que les opérations de fusion (merge) conservent l’intégrité des relations (pivots, clés étrangères).

- **Traçabilité des transformations** : Confirmer que les corrections sont documentées, identifiables et rejouables dans un contexte de nouvelle installation.

- **Tests techniques** : Validation par des scénarios concrets : Fusion d’entités → Réimport → Vérification

Cette approche permet d’évaluer non seulement le bon fonctionnement technique du système, mais également sa robustesse face aux mises à jour successives des sources externes.

## Équipe

<!-- > Présentez les membres de l’équipe et le rôle principal de chacun dans le projet. -->

- Anissa Ould Ferroukh Développement serveur.
- Camille Delattre Direction des opérations et coordinatrice à la structuration des données
- Camila De Oliveira Savoi Direction de la recherche
- Julie Graff Direction artistique
- Lena Krause Direction technique et fondatrice
- Alexia Pinto Ferretti Direction des publics
- Marguerite Chiarello Responsable des communications et consultante en médiation
- Corélie Godefroid Développement serveur
- Simon Janssen Responsable de l'infrastructure
- Christian Lungescu Développement mobile
- Barbara Marche Designer UI/UX
- David Valentine Consultation en sciences de l'information

## Échéancier

Le suivi complet est disponible dans la page [Suivi de projet](suivi.md).

| Activités                      | Début   | Fin     | Livrable               | Statut      |
| ------------------------------ | ------- | ------- | ---------------------- | ----------- |
| Réunion d'intégration          | 12 jan. | 12 jan. | Notes de compréhention | ✅ Terminé  |
| Analyse / Études préliminaires | 12 jan. | 23 jan. | Document d'analyse     | ✅ Terminé  |
| Développement / Contribution   | 23 jan. | 17 jan. | Modules / Ajouts       | ✅ Terminé  |
| Présentation + Rapport         | 17 avr. | 30 avr. | Présentation + Rapport | ⏳ À venir  |
