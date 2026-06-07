# LabSec9 – Analyse Dynamique Android avec Drozer


---

## Contexte

Ce laboratoire met en œuvre **Drozer**, un framework d'analyse de sécurité Android permettant d'interagir avec les composants exposés d'une application installée sur un terminal ou un émulateur.

L'objectif est de cartographier les composants déclarés par l'application pédagogique **OWASP MSTG UnCrackable Level 1** et d'identifier les surfaces d'exposition accessibles depuis le système Android.

L'analyse porte principalement sur :

* Les informations du package Android ;
* Les activités (Activities) ;
* Les services (Services) ;
* Les récepteurs de diffusion (Broadcast Receivers) ;
* Les fournisseurs de contenu (Content Providers) ;
* Le manifeste Android (AndroidManifest.xml).

---

## Objectifs du Laboratoire

* Déployer et configurer Drozer.
* Établir une communication entre la console Drozer et l'émulateur Android.
* Identifier le package cible.
* Cartographier les composants Android exposés.
* Examiner les déclarations du manifeste.
* Évaluer les surfaces d'attaque visibles via Drozer.

---

## Environnement

| Élément              | Valeur                    |
| -------------------- | ------------------------- |
| Plateforme           | Android Emulator          |
| Outil d'analyse      | Drozer                    |
| Agent Drozer         | Installé et actif         |
| Port Forwarding ADB  | 31415                     |
| Application analysée | `UnCrackable-Level1.apk`  |
| Package cible        | `owasp.mstg.uncrackable1` |

---

## Architecture du Laboratoire

```text
+----------------------+
| Drozer Console       |
| (Machine Hôte)       |
+----------+-----------+
           |
           | ADB Forward tcp:31415
           |
+----------v-----------+
| Android Emulator     |
|                      |
| Drozer Agent         |
| UnCrackable Level 1  |
+----------------------+
```

Cette architecture permet à la console Drozer exécutée sur l'hôte de communiquer avec l'agent Drozer installé dans l'émulateur Android.

---

## Préparation de l'Environnement

Avant l'analyse, les éléments suivants sont mis en place :

* Installation de l'APK cible.
* Installation de l'agent Drozer.
* Activation du serveur Drozer sur l'émulateur.
* Configuration du port forwarding ADB.

### Installation et Port Forwarding

![Installation Drozer](<screens/Capture d'écran 2026-06-01 094846.png>)

Le port forwarding permet de relier le port local de la machine hôte au service Drozer exécuté dans l'émulateur.

---

## Activation de l'Agent Drozer

Une fois lancé, l'agent Drozer attend les connexions provenant de la console d'administration.

### Agent Drozer Actif

![Agent Drozer](<screens/Capture d'écran 2026-06-01 100411.png>)

Cette étape confirme que le service Drozer est opérationnel sur l'émulateur.

---

## Connexion de la Console Drozer

La console Drozer est ensuite ouverte sur la machine hôte et connectée à l'agent.

### Console Connectée

![Console Drozer](<screens/Capture d'écran 2026-06-01 100449.png>)

Une fois la connexion établie, les différents modules Drozer peuvent être utilisés pour explorer l'application.

---

## Identification du Package

La première étape consiste à vérifier que l'application cible est correctement détectée.

### Informations du Package

![Informations Package](<screens/Capture d'écran 2026-06-01 100623.png>)

Drozer identifie le package :

```text
owasp.mstg.uncrackable1
```

Les informations générales de l'application sont alors accessibles pour les étapes suivantes.

---

## Analyse des Activités

Les activités représentent les points d'entrée visibles de l'application.

### Activité Principale

![Activité Principale](<screens/Capture d'écran 2026-06-01 100642.png>)

Drozer identifie l'activité principale déclarée dans le manifeste Android.

Cette activité correspond au point d'entrée utilisateur de l'application.

### Détails de l'Activité

![Détail Activité](<screens/Capture d'écran 2026-06-01 100838.png>)

Les informations complémentaires permettent de confirmer :

* le nom de la classe ;
* les attributs associés ;
* son rôle dans le cycle de vie de l'application.

---

## Analyse des Services

Les services Android permettent l'exécution de traitements en arrière-plan.

### Services Détectés

![Services](<screens/Capture d'écran 2026-06-01 100700.png>)

Dans le contexte de ce laboratoire, aucun service exporté n'est remonté par Drozer.

Cette observation réduit la surface d'exposition accessible depuis l'extérieur de l'application.

---

## Analyse des Broadcast Receivers

Les Broadcast Receivers permettent à une application de recevoir des événements système ou applicatifs.

### Receivers Détectés

![Receivers](<screens/Capture d'écran 2026-06-01 100717.png>)

Aucun receiver exporté pertinent n'a été identifié lors de cette phase d'analyse.

---

## Analyse des Content Providers

Les Content Providers constituent souvent une surface d'attaque importante lorsqu'ils sont mal protégés.

### Content Providers

![Providers](<screens/Capture d'écran 2026-06-01 100741.png>)

Aucun Content Provider exploitable n'est détecté dans le périmètre du laboratoire.

### Recherche d'URI Exposées

![Recherche Providers](<screens/Capture d'écran 2026-06-01 100857.png>)

La recherche complémentaire ne révèle aucune URI accessible permettant d'interagir avec des données applicatives.

---

## Analyse du Manifeste Android

Le manifeste Android constitue la source principale d'information concernant les composants déclarés par l'application.

### Consultation du Manifest

![Manifest Android](<screens/Capture d'écran 2026-06-01 100812.png>)

Cette étape permet de vérifier :

* les activités déclarées ;
* les permissions utilisées ;
* les composants exportés ;
* les paramètres de configuration de l'application.

---

## Synthèse des Observations

| Élément analysé     | Résultat        |
| ------------------- | --------------- |
| Package Android     | Détecté         |
| Activité principale | Identifiée      |
| Services exportés   | Aucun détecté   |
| Broadcast Receivers | Aucun détecté   |
| Content Providers   | Aucun détecté   |
| URI accessibles     | Aucune détectée |
| Manifest Android    | Consulté        |

---

## Résultats

Les objectifs du laboratoire ont été atteints :

* ✅ Déploiement de Drozer sur l'émulateur Android.
* ✅ Configuration du port forwarding ADB.
* ✅ Connexion réussie entre la console et l'agent Drozer.
* ✅ Identification du package cible.
* ✅ Cartographie des composants Android.
* ✅ Analyse des activités déclarées.
* ✅ Vérification des services, receivers et providers.
* ✅ Consultation du manifeste Android.

---

## Conclusion

Ce laboratoire a permis de découvrir l'utilisation de Drozer comme outil de cartographie et d'analyse dynamique des composants Android.

L'application pédagogique **OWASP MSTG UnCrackable Level 1** présente une surface d'exposition limitée dans le contexte observé : une activité principale identifiable mais aucun service exporté, aucun receiver exposé et aucun Content Provider accessible.

Cette phase constitue une étape importante avant des analyses plus avancées portant sur l'interaction avec les composants Android, l'exploitation de permissions ou les tests de sécurité applicative mobile.
