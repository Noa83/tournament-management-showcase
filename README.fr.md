# Tournament Management Platform

[🇬🇧 English](README.md) | 🇫🇷 Français

### Application de gestion et d'organisation de tournois sportifs

**PHP · Symfony · Doctrine ORM · JavaScript · Twig · Symfony Messenger · Docker**

Application web métier développée pour **Annecy Volleyball**, club français de volleyball, afin d'accompagner l'organisation et la gestion de son tournoi annuel de Pentecôte.

La plateforme couvre les besoins opérationnels d'un événement sportif : inscriptions des participants, paiements, gestion des équipes, organisation des compétitions, génération des rencontres, suivi des scores, classements et coordination des bénévoles.

**Statut : Application entièrement fonctionnelle — améliorations et nouvelles fonctionnalités prévues.**

> **Code source privé.** Ce dépôt constitue une présentation technique du projet, de ses fonctionnalités, de son architecture et des problématiques rencontrées, sans exposer son code propriétaire.

---
## Le projet en chiffres

La plateforme accompagne l'organisation d'un tournoi de volleyball de grande envergure, avec :

- **Plus de 550 équipes** à gérer.
- **Environ 1 800 joueurs** inscrits.
- **Des inscriptions complètes en moins d'une heure** après leur ouverture.

Cette forte concentration des inscriptions sur une période très courte constitue un enjeu technique majeur. L'application doit gérer de nombreuses demandes simultanées tout en assurant la **fiabilité des inscriptions, la cohérence des données et le respect des capacités d'accueil**.

Pour répondre à ces contraintes, elle intègre notamment une file d'attente, des transactions en base de données et des mécanismes de verrouillage pessimiste.

## Contexte et objectifs

L'organisation d'un tournoi de volleyball nécessite de coordonner plusieurs processus interdépendants :

- Gérer les inscriptions des participants et des équipes.
- Traiter les paiements et contrôler les capacités d'inscription.
- Organiser les catégories, les poules et les rencontres.
- Affecter les matchs aux terrains disponibles.
- Suivre les résultats et calculer les classements.
- Coordonner les bénévoles et les opérations liées à l'événement.
- Générer les documents administratifs et sportifs nécessaires.

L'application centralise ces opérations pour faciliter la gestion de l'événement par les organisateurs.

## Mon rôle et mes contributions

**Contributrice principale au développement et à l'évolution d'une application PHP/Symfony depuis 2021.**

Selon les statistiques de contribution GitHub consultées en octobre 2026, mon compte @Noa83 représente **742 des 831 commits hors fusion** enregistrés sur la branche principale, soit environ 89 % du total.

L'application a été développée de manière collaborative, avec la participation de plusieurs développeurs.

Mes interventions couvrent plusieurs domaines techniques :

- Développement back-end et implémentation de logique métier.
- Maintenance applicative et développement de fonctionnalités.
- Modélisation des données et gestion de la base de données.
- Intégration de services externes et gestion des paiements.
- Sécurité applicative et gestion des accès.
- Développement et amélioration des fonctionnalités d'administration.
- Évolution des fonctionnalités existantes et des interfaces utilisateur.

### Algorithme de génération des rencontres

L'algorithme de génération des rencontres a été principalement conçu et développé par **@L-Nicolet**, un autre contributeur au projet.

Mes interventions sur ce composant se sont limitées à quelques ajustements et opérations ponctuelles de maintenance.

Bien que cet algorithme occupe une place importante dans le fonctionnement de la plateforme, sa conception et son développement principal ne font pas partie de mes réalisations personnelles.

## Architecture technique

| Domaine | Technologies |
|---|---|
| Back-end | PHP 8.2+, Symfony 7.4 |
| Persistance | Doctrine ORM, base de données relationnelle |
| Front-end | Twig, JavaScript, CSS, Webpack Encore |
| Traitements asynchrones | Symfony Messenger |
| Paiement | Intégration Monetico |
| Génération documentaire | Outils de génération PDF |
| Environnement de développement | Docker |
| Sécurité | Symfony Security, chiffrement AES-256-GCM |

L'application repose sur une architecture Symfony organisée autour de contrôleurs, de services métier, d'entités Doctrine et de repositories.

Cette séparation des responsabilités permet de structurer les opérations propres aux différents domaines fonctionnels : inscriptions, paiements, compétitions et génération documentaire.

### Vue d'ensemble de l'architecture

![Architecture technique — Tournament Management Platform](assets/architecture.png)

*Vue d'ensemble des principaux composants de l'application, des modules métier, des intégrations externes et des flux de traitement.*

Pour une présentation détaillée, consulter la [documentation de l'architecture technique](docs/architecture.md).

## Fonctionnalités principales

### Gestion des inscriptions et des participants

- Inscription individuelle et par équipe.
- Création et gestion des équipes.
- Gestion des capacités d'inscription.
- Gestion des listes d'attente.
- Paiement en ligne des inscriptions.
- Administration des participants.

### Gestion du tournoi et des compétitions

- Gestion des catégories et des niveaux sportifs.
- Organisation des groupes et des poules.
- Génération automatisée des rencontres.
- Affectation des équipes et des terrains.
- Gestion des différentes phases de compétition.
- Saisie et suivi des résultats.
- Calcul des classements et des statistiques.
- Gestion des phases finales.

### Administration de l'événement

- Gestion des gymnases et terrains extérieurs.
- Planification des bénévoles et attribution des tâches.
- Gestion de l'accueil et du check-in des équipes.
- Communications et campagnes e-mail.
- Génération de documents PDF et de factures.
- Configuration et administration du tournoi.

## Défis techniques

L'application répond à plusieurs problématiques techniques liées à la gestion d'un événement sportif.

Les sections suivantes présentent les caractéristiques techniques de la plateforme, sans attribuer l'ensemble des composants à un contributeur unique.

### 1. Génération automatisée des rencontres

Le moteur de génération des rencontres prend en compte plusieurs contraintes métier :

- Les catégories et les niveaux des équipes.
- La composition des groupes et des poules.
- La disponibilité et l'affectation des terrains.
- Les différentes phases du tournoi.
- La progression des équipes vers les phases finales.

**Attribution du développement**

Ce composant a été principalement conçu et développé par Loup, avec des contributions ultérieures limitées de ma part.

Il est présenté ici pour illustrer la complexité fonctionnelle et architecturale globale de l'application, et non comme une réalisation personnelle en conception algorithmique.

### 2. Gestion des inscriptions simultanées

L'ouverture des inscriptions peut provoquer une concentration importante de demandes simultanées.

L'application intègre une file d'attente permettant de réguler l'accès aux inscriptions.

Des transactions et des mécanismes de verrouillage pessimiste sont utilisés lors de la confirmation des paiements et du contrôle des capacités d'inscription pour gérer les accès concurrents.

**Concepts techniques :** gestion de la concurrence, transactions Doctrine, gestion des états et cohérence des données.

### 3. Protection des données personnelles

L'application intègre plusieurs mécanismes de protection des informations personnelles :

- Chiffrement AES-256-GCM de certains champs contenant des données personnelles.
- Utilisation de HMAC pour certains traitements liés aux adresses e-mail.
- Gestion des autorisations selon les différents rôles utilisateurs.

**Concepts techniques :** sécurité applicative, cryptographie appliquée et contrôle des accès.

### 4. Traitements asynchrones et génération documentaire

Certaines opérations, notamment la génération de documents et les communications par e-mail, sont prises en charge par des composants dédiés.

Symfony Messenger intervient dans l'organisation des traitements asynchrones.

**Concepts techniques :** architecture applicative, traitements asynchrones, tâches en arrière-plan et génération documentaire.

## Documentation technique

Les documents suivants présentent l'architecture de l'application, les problématiques techniques rencontrées et les évolutions envisagées.

### [Architecture technique](docs/architecture.md)

Présentation de l'architecture Symfony, des services métier, de la persistance Doctrine, des mécanismes de sécurité, de l'intégration des paiements et des traitements asynchrones.

### [Défis techniques et choix d'implémentation](docs/technical-challenges.md)

Analyse des principales problématiques techniques : gestion des inscriptions simultanées, cohérence des paiements, protection des données personnelles, traitements en arrière-plan et maintenabilité.

### [Feuille de route du projet](docs/roadmap.md)

Présentation des évolutions envisagées, notamment la saisie des scores par les joueurs, les préinscriptions avec sélection aléatoire et la refonte de l'interface utilisateur.

## Captures d’écran de l’application

Les captures suivantes présentent plusieurs vues représentatives de la plateforme AnnecyVB dans son état actuel.

L’application est entièrement fonctionnelle dans son périmètre actuel. L’interface visible ici correspond à l’état actuel de la plateforme ; une refonte du front-end est prévue dans la feuille de route.

### Connexion et accès joueur

![Connexion et accès joueur](assets/screenshots/00-connexion.png)

*Page publique de connexion des joueurs, avec branding du tournoi, accès à l’inscription et entrée vers l’espace joueur.*

### Tableau de bord administrateur

![Tableau de bord administrateur](assets/screenshots/01-admin-dashboard.png)

*Tableau de bord principal donnant accès aux modules opérationnels : préparation du tournoi, réglages live, bénévoles et gestion globale du tournoi.*

### Indicateurs d’inscription

![Indicateurs d’inscription](assets/screenshots/02-registration.png)

*Vue synthétique des indicateurs d’inscription : équipes créées, équipes inscrites, profils joueurs et taux de remplissage.*

### Gestion des équipes

![Gestion des équipes](assets/screenshots/03-teams.png)

*Vue d’administration des équipes inscrites, avec recherche, filtrage, export et actions d’édition.*

### Gestion sportive et déroulement du tournoi

![Gestion sportive et déroulement du tournoi](assets/screenshots/04-matches.png)

*Vue des tours actifs et passés, avec suivi de l’avancement et des horaires des différentes phases de compétition.*

### Classements

![Classements](assets/screenshots/05-results-rankings-a.png)

*Interface de consultation des classements par tour, avec fonctions d’export.*

### Gestion des bénévoles

![Gestion des bénévoles](assets/screenshots/06-volunteers-a.png)

*Vue récapitulative des bénévoles par tâche et par créneau pour le pilotage opérationnel de l’événement.*

**[Découvrir toutes les captures de l’application](docs/screenshots.md)**

> Certaines captures comportent des zones floutées afin de préserver des informations personnelles ou sensibles.

## Évolutions envisagées

L'application est entièrement fonctionnelle dans son périmètre actuel.

Plusieurs améliorations sont envisagées afin de faire évoluer l'expérience des participants, des bénévoles et des organisateurs.

### Saisie des scores par les joueurs

Une nouvelle interface permettra aux joueurs de renseigner directement les résultats de leurs rencontres.

Des mécanismes adaptés de vérification et de validation seront définis afin de préserver la fiabilité des résultats sportifs.

### Préinscriptions et sélection aléatoire des équipes

Un nouveau parcours de préinscription permettra aux équipes de manifester leur intérêt avant la sélection définitive.

Le processus intégrera un mécanisme de tirage au sort, avec des règles d'éligibilité, de sélection et de confirmation restant à définir.

### Refonte de l'interface utilisateur

Une refonte de l'interface est envisagée afin d'améliorer l'ergonomie, la cohérence visuelle et l'expérience utilisateur.

Une attention particulière sera portée à l'utilisation sur mobile et aux besoins des différents profils utilisateurs.

### Amélioration continue

De nouvelles fonctionnalités pourront être développées en fonction des retours et de l'évolution des besoins des participants, des bénévoles et des organisateurs.

## Code source et confidentialité

Le code source de l'application est hébergé dans un dépôt privé appartenant à l'organisation AnnecyVB.

Ce dépôt public est exclusivement destiné à servir de portfolio technique et de présentation du projet.

Il ne contient aucun code propriétaire de l'application, aucune donnée personnelle, aucun identifiant de paiement ni aucune information confidentielle de l'organisation.

---

**Projet :** Annecy Volleyball — Tournoi de Pentecôte

**Rôle :** Contributrice principale / Développeuse back-end

**Période :** 2021 – aujourd'hui

**Statut :** Application entièrement fonctionnelle, en évolution continue

**Code source :** Privé