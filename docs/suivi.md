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
- Participation à une réunion avec Corélie pour qu'elle puisse m'expliquer la dernière version de l'API
- Recherche sur le modèle ETL et comment les outils OpenRefine ainsi que Apache Airflow peuvent opérer au niveau du processus

Après avoir effectuer ma rechecher j'ai pu organiser mes idées comme suit :

<div style="position: relative; width: 100%; margin: 2rem auto;">
<p style="text-align:center; font-style: italic; margin-top: -1rem;">
  Cliquer sur le schéma pour l’ouvrir en plein écran
</p>
  <a href="../pdfs/ETL_process_num1.pdf" target="_blank"
     style="position:absolute; inset:0; z-index:10;">
  </a>

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

- Participation à une réunion avec Simon pour me montrer comment effectuer un déploiement :
  <ul>
    <li>Bonne pratique : créer une branche sur GitHub pour chaque tâche</li>
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

- J'ai exploré cette semaine quels étaient les avantages et inconvénients de l'utilisation du processus ETL au sein du projet MONA
