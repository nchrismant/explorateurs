# Explorateurs — Simulation / Jeu d’exploration multithread en Java

Ce projet a pour objectif de proposer une **simulation / jeu** inspiré de jeux comme *Curious Expedition*, afin d’apprendre la **programmation multi-threading** et développer une **IHM graphique**.  
Le joueur incarne un groupe d’explorateurs qui partent à la recherche de trésors sur une île dangereuse et inexplorée — en tentant de maximiser les trésors collectés tout en minimisant les pertes humaines.

---

## 📌 Sommaire

- [Explorateurs — Simulation / Jeu d’exploration multithread en Java](#explorateurs--simulation--jeu-dexploration-multithread-en-java)
  - [📌 Sommaire](#-sommaire)
  - [🎯 Objectif du projet](#-objectif-du-projet)
  - [🎮 Stratégies disponibles](#-stratégies-disponibles)
  - [✨ Fonctionnalités](#-fonctionnalités)
  - [🧩 Structure du projet / Architecture](#-structure-du-projet--architecture)
  - [🚀 Installation \& Déploiement](#-installation--déploiement)
    - [Prérequis](#prérequis)
    - [1. Cloner le dépôt](#1-cloner-le-dépôt)
    - [2. Ouvrir dans Eclipse ou un autre IDE Java](#2-ouvrir-dans-eclipse-ou-un-autre-ide-java)
    - [3. Lancer l’application](#3-lancer-lapplication)
  - [🏗️ Design Patterns utilisés](#️-design-patterns-utilisés)
  - [🛠️ Technologies \& Outils utilisés](#️-technologies--outils-utilisés)
  - [👥 Auteurs \& Licence](#-auteurs--licence)

---

## 🎯 Objectif du projet

- Apprendre et expérimenter la **programmation multi-threading** (gestion de plusieurs explorateurs, monstres, événements, etc.).  
- Développer une **interface graphique (IHM)** permettant de visualiser la carte, les explorateurs, les monstres, les trésors, et l’évolution du jeu.  
- Concevoir une **simulation stratégique** et ludique : choix de stratégie, tour-par-tour, prise de décision, aléatoire vs stratégie.  
- Proposer un jeu complet, interactif, avec des **règles de survie, combat, collecte, hasard et stratégie**.  

---

## 🎮 Stratégies disponibles

À chaque nouvelle partie, le joueur choisit l’une des trois stratégies suivantes, chacune avec ses avantages et risques :

- **Aventurière** :  
  - Permet d’**escalader les obstacles** (montagnes / zones bloquées), au lieu de les contourner.  
  - L’explorateur n’est **pas armé** — s’il croise un monstre, il est tué immédiatement.  
  - C’est une stratégie **haut risque / haut gain** : rapide pour explorer, mais très dangereuse.

- **Offensive** :  
  - L’explorateur est équipé d’une **épée**.  
  - Lors d’un affrontement : le monstre est tué, mais l’explorateur perd **50 HP / PV** (la moitié de sa vie).  
  - Bon compromis entre risque et efficacité — utile quand on anticipe des rencontres ennemies.

- **Défensive** :  
  - L’explorateur est équipé d’un **bouclier**.  
  - Lors d’un affrontement : le monstre ne subit aucun dégât, mais l’explorateur perd **25 HP / PV** (un quart de sa vie).  
  - Stratégie **prudente**, visant à minimiser les pertes tout en continuant l’exploration.

Ces stratégies modulent le style de jeu : risque élevé mais rapide (Aventurière), combat offensif avec pertes modérées (Offensive), ou progression sûre mais lente (Défensive).

---

## ✨ Fonctionnalités

L’utilisateur peut :

- Voir un **menu principal** (nouvelle partie / quitter).  
- Lire les **règles du jeu**.  
- Choisir parmi **trois stratégies** disponibles pour lancer la simulation.  
- Visualiser la **carte** — avec ses limites.  
- Visualiser les **explorateurs**, **monstres**, **trésors**, **montagnes** à tout moment.  
- Suivre en temps réel le **nombre d’explorateurs en vie** et le **nombre de trésors restants**.  
- Voir sa **bourse d’or** s’incrémenter à chaque trésor récupéré.  
- Retourner au **menu principal** en fin de partie ou en cours de simulation.  
- À la fin de la partie, voir le **résultat global** : or total collecté, survivants, etc.  

---

## 🧩 Structure du projet / Architecture

```text
src/
 ├── configuration/ # Configuration des variables globales
 ├── element/       # Eléments de la carte
 ├── gui/           # Interface graphique, interactions utilisateur
 ├── images/        # Ressources visuelles
 ├── map/           # Gestion de la map (carte)
 ├── process/       # Gestion des éléments (multi-thread, builder, utility, ...)
 ├── strategy/      # Gestion des stratégies possibles (Aventurière, Offensive, Défensive)
 └── test/         
  └── Exploration.java # Main
```

## 🚀 Installation & Déploiement

### Prérequis

- Java JDK 8 ou supérieur  
- IDE Java (IntelliJ, Eclipse, NetBeans…) ou ligne de commande  

### 1. Cloner le dépôt  

```bash
git clone https://github.com/nchrismant/explorateurs.git
cd explorateurs
```

### 2. Ouvrir dans Eclipse ou un autre IDE Java

### 3. Lancer l’application

Exécuter la classe suivante :

```**bash**
src/test/Exploration.java
```

---

## 🏗️ Design Patterns utilisés

- **Strategy** :  
  - Implémenté dans le package `strategy/` avec `Strategy`, `ShieldStrategy`, `SwordStrategy` et `EquipmentStrategy`.  
  - Permet de modifier dynamiquement le comportement des explorateurs (combat, escalade, gestion des PV) selon la stratégie choisie : *Aventurière*, *Offensive*, *Défensive*.

- **Singleton** :  
  - `GameConfiguration` centralise les paramètres globaux du jeu et s'assure qu’une seule instance configurationnelle est utilisée dans toute la simulation.

- **Builder** :  
  - `GameBuilder` (dans `process/`) construit étape par étape les éléments complexes du jeu : carte, explorateurs, monstres, trésors.  
  - Permet de séparer la logique de création du cœur de la simulation.

- **Observer / MVC-like** :  
  - Le package `gui/` s’appuie sur une architecture proche de MVC :  
    - **Model** : éléments du jeu (`element/`, `map/`)  
    - **View** : affichage graphique (`gui/`)  
    - **Controller** : interactions utilisateur + notifications via les managers (`process/`)  
  - Les managers (ex : `ExplorerManager`, `IntersectionManager`) notifient la vue lorsqu’un élément change d'état (position, vie, trésors collectés).

- **Factory (informelle)** :  
  - Les classes du package `process/` (ex : `AnimalManager`, `ExplorerManager`) agissent comme des factories pour créer et gérer les entités de la carte de manière centralisée.

- **Threading Pattern** :  
  - La classe `Simulation` orchestre le multi-threading, où chaque explorateur évolue indépendamment.  
  - Gestion robuste des threads via les managers (`ExplorerManager`, `AnimalManager`) pour éviter les conflits et synchroniser les actions.

- **Utility / Helper Classes** :  
  - `GameUtility` fournit des méthodes réutilisables pour gérer les déplacements, collisions, vérifications de limites et interactions entre entités.

---

## 🛠️ Technologies & Outils utilisés

| Technologie      | Rôle              |
| ---------------- | ----------------- |
| **Java**         | Langage principal |
| **Eclipse**      | IDE recommandé    |

---

## 👥 Auteurs & Licence

- **AFATCHAWO Koffi Junior** — Étudiant L3 Informatique, Cergy Paris Université.
- **CHRISMANT Nathan** — Étudiant L3 Informatique, Cergy Paris Université.
- **DACRUZ Mathis** — Étudiant L3 Informatique, Cergy Paris Université.

Projet distribué sous licence **Open Source**.
