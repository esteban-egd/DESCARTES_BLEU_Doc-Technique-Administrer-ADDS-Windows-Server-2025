# `🌐` ︲ Documentation TP — Administration de Windows Server - DESCARTES BLEU

Ce dépôt présente la documentation technique à l'administration d'un Active Directory pour le domaine `descartesbleu.org`, dans le cadre des activités du projet Descartes-Bleu.

---

## 📑 ︲ Sommaire

- [📘 ︲ Introduction](#introduction)
  - [🎯 ︲ Contexte et objectifs du TP](#contexte)
  - [🖥️ ︲ Configuration de la machine serveur](#configuration)
  - [🗺️ ︲ Arborescence de l'Active Directory](#arborescence-ad)
- [🚀 ︲ Activité 2 : Installation des rôles AD DS et DNS](#installationroles)
  - [➕ ︲ Ajout du service AD DS](#ajoutroles)
  - [⚙️ ︲ Lancement et validation de l'installation](#validationinstall)
  - [👑 ︲ Promotion du serveur en contrôleur de domaine](#promotiondc)
  - [🌳 ︲ Configuration de la nouvelle forêt](#configurationforets)
  - [🔒 ︲ Paramétrage des options et du mode DSRM](#optionsdsrm)
  - [🔍 ︲ Vérifications et validation finale de la promotion](#validationpromotion)
  - [🔐 ︲ Simplification de la stratégie de mots de passe](#strategiemdp)
  - [🛡️ ︲ Configuration de la stratégie de mots de passe](#modificationstrategie)
- [👥 ︲ Activité 3 : Structurer et administrer Active Directory](#structurationad)
  - [📁 ︲ Création des unités d'organisation et des groupes](#creationuo)
  - [👤 ︲ Création des utilisateurs](#creationusers)
  - [💻 ︲ Intégration d'un PC client au domaine](#integrationposte)
- [📁 ︲ Activité 4 : Centraliser les données des utilisateurs](#centralisation)
  - [🔄 ︲ Redirection des dossiers des utilisateurs](#redirectionprofils)
  - [🔌 ︲ Activation du mode hors connexion](#modehorsconnexion)
- [⚙️ ︲ Activité 5 : Automatiser l'administration des utilisateurs](#automatisation)
  - [📁 ︲ Création de l'OU `stagiaires`](#creationoustagiaires)
  - [📜 ︲ Création des utilisateurs, des OU et des groupes via un script PowerShell](#scriptusers)
  - [✅ ︲ Vérification de la mise en place du script](#testscriptusers)
  
---

<a id="introduction"></a>
# `📘` ︲ Introduction

---

<a id="contexte"></a>
## `🎯` ︲ Contexte et objectifs du TP

> [!NOTE]
> L'objectif est de mettre en service le domaine Active Directory en installant le rôle **AD DS** (Services de Domaine Active Directory) ainsi que le rôle complémentaire **DNS** sur notre serveur `SRV-AD1`. Nous configurerons également une politique de mots de passe simplifiée. Puis nous administrerons les ressources, les partages et les GPO.

---

<a id="arborescence-ad"></a>
### 🗺️ ︲ Arborescence de l'Active Directory

---

<a id="configuration"></a>
## `🖥️` ︲ Configuration de la machine serveur

> [!NOTE]
> Pour cette intervention, les spécifications techniques du réseau local sont les suivantes :

- `📡` | **Adresse IP statique :** `172.16.0.1` / `255.255.0.0`
- `📦` | **Carte réseau :** `Réseau Interne`

---

<a id="installationRoles"></a>
# `🚀` ︲ Activité 2 : Installation des rôles AD DS et DNS

<a id="ajoutRoles"></a>
## `➕` ︲ Ajout du service AD DS

1. Sur le bureau de `SRV-AD1`, ouvrez le `Gestionnaire de serveur`.
2. Cliquez sur `Gérer`, puis sur sur `Ajouter des rôles et des fonctionnalités` .
3. Sur la page `Assistant Ajout de rôles et de fonctionnalités`, cliquez sur `Suivant`. (cette page explique que l'assistant permet d'installer des rôles et des fonctionnalités)
4. Laissez l'option par défaut `Installation basée sur un rôle ou une fonctionnalité` cochée, puis cliquez sur `Suivant`.
5. Laissez le `SRV-AD1` puis cliquez sur `Suivant`.
6. Dans la liste des `rôles de serveur`, cochez la case `Services de Domaine Active Directory` qui est tout simplement `AD DS`. Cliquez sur `Suivant`
7. La fenêtre `Fonctionnalités` s'ouvre. Il n'est pas nécessaire de sélectionner quoi que ce soit. Vous pouvez cliquer sur `Suivant`.
8. Cliquer sur `Suivant`.

> [!NOTE]
> Le service DNS s'installera automatiquement avec le service AD DS. Celui-ci est indispensable pour le fonctionnement d'Active Directory et permet la résolution de noms au sein du réseau local.

<details>
  <summary><strong>📸︲Lancement de l'assistant d'ajout de rôles</strong></summary>
  <img src="captures/2026-09-14 14_25_01-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_25_16-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_25_41-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_25_56-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Sélection des rôles AD DS et des fonctionnalités</strong></summary>
  <img src="captures/2026-09-14 14_28_45-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_29_03-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_30_23-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_30_53-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="validationInstall"></a>
## `⚙️` ︲ Lancement et validation de l'installation

1. Un écran récapitulatif des services sélectionnés s'affiche. Vérifiez la présence des rôles `AD DS` et `DNS`.
2. Cliquez sur `Installer` et patientez.

<details>
  <summary><strong>📸︲Progression et fin de l'installation des rôles AD DS et DNS</strong></summary>
  <img src="captures/2026-09-14 14_31_41-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_45_06-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="promotionDC"></a>
## `👑` ︲ Promotion du serveur en contrôleur de domaine

Une fois AD DS installé, le serveur possède bien les services nécessaires. Cependant il n'est pas encore actif. Pour ce faire, regardez la suite.

<details>
  <summary><strong>📸︲Fin de l'installation et lien de promotion post-déploiement</strong></summary>
  <img src="captures/2026-09-14 14_45_36-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_45_56-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<a id="configurationForets"></a>
## `🌳` ︲ Configuration de la nouvelle forêt

1. Sur l'écran récapitulatif de l'installation d'AD DS, cliquez sur `Promouvoir ce serveur en contrôleur de domaine`.
2. L'assistant de configuration Active Directory s'ouvre. Sur la page de `configuration du déploiement`, cochez `Ajouter une nouvelle forêt`.
3. Dans le champ `Nom de domaine racine`, saisissez `descartesbleu.org`. Cliquez sur `Suivant`.

<details>
  <summary><strong>📸︲Assistant Déploiement Active Directory (Nouvelle forêt)</strong></summary>
  <img src="captures/2026-09-14 14_46_31-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_47_13-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="optionsDSRM"></a>
## `🔒` ︲ Paramétrage des options et du mode DSRM

1. Sur la page des options du contrôleur de domaine :
   - Laissez les niveaux fonctionnels tels qu'ils sont.
   - Vérifiez que l'option `serveur DNS` est bien cochée.
2. Dans le champ `Mot de passe` pour le mode de restauration des services d'annuaire, je vous conseille ici de saisir le même mot de passe administrateur `btssio-lmc26`, confirmez-le et enfin cliquez sur `Suivant`.

<details>
  <summary><strong>📸︲Configuration des options du contrôleur de domaine et du mot de passe DSRM</strong></summary>
  <img src="captures/2026-09-14 14_47_52-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="validationpromotion"></a>
## `🔍` ︲ Vérifications et validation finale de la promotion

1. Sur la page des options DNS, un avertissement concernant la délégation DNS peut apparaître si la zone parente n'est pas encore enregistrée vous pouvez ignorer cet avertissement et cliquer sur `Suivant`.
2. Sur la page de vérification du nom NetBIOS, laissez comme suit le nom `DESCARTESBLEU`. Cliquez sur `Suivant`.
3. Sur la page des chemins d'accès, cliquez sur `Suivant`. (Cette étape vous informe simplement de l'emplacement les fichiers journaux et le partage `SYSVOL`)
4. Après le récapitulatif, une vérification s'effectue. Une fois validé, cliquez sur `Installer`. Une fois l'installation terminée vous pouvez redémarrer le serveur.
5. Une fois le serveur redémarré vous pouvez dès l'écran de verrouillage constater un changement. Il vous est maintenant demandé de saisir un nom d'utilisateur et un mot de passe. Il vous faudra donc marquer `Administrateur` et `btssio-lmc26`

<details>
  <summary><strong>📸︲Options DNS, NetBIOS et Options supplémentaires</strong></summary>
  <img src="captures/2026-09-14 14_48_46-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 14_49_47-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Examen des options et vérification de la configuration requise</strong></summary>
  <img src="captures/2026-09-14 15_06_28-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_07_36-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Redémarrage automatique et nouvel écran de verrouillage du domaine</strong></summary>
  <img src="captures/2026-09-14 15_08_59-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_12_21-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_12_39-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>
</details>

---

<a id="strategieMdp"></a>
## `🔐` ︲ Simplification de la stratégie de mots de passe

Dans le cadre d'une installation en laboratoire (donc hors production), simplifier la stratégie de mots de passe des comptes créés dans l'annuaire Active Directory facilitera les phases de tests et de manipulations.

<a id="modificationStrategie"></a>
## `🛡️` ︲ Configuration de la stratégie de mots de passe

1. Après le redémarrage, reconnectez-vous au serveur `SRV-AD1` en ouvrant une session sous le compte administrateur du domaine (`Administrateur`).
2. Ouvrez l'outil de gestion des stratégies. Pour ce faire, sur le `gestionnaire de serveur`, dans `Outils`, cliquez sur `Gestion de stratégie de groupe`. 
3. Déroulez l'arborescence : `Forêt : descartesbleu.org`, `Domaines`, `descartesbleu.org`.
4. Faites un clic droit sur `Default Domain Policy` puis sélectionnez `Modifier...`.
5. Dans l'`éditeur de gestion de stratégie de groupe`, déroulez l'arborescence :  `Configuration ordinateur`, `Stratégies`, `Paramètres Windows`, `Paramètres de sécurité`, `Stratégies de comptes` et enfin `Stratégie de mot de passe`.

> [!NOTE] 
> Il nous est demandé de changer 3 paramètres pour la simplification des mots de passe, qui est : fixer la longueur minimale à 5 caractères, le mot de passe ne doit jamais expirer et la complexité doit être désactivée.

6. Pour ce faire :
   - Dans `le mot de passe doit répondre à des exigences de complexité :` Basculez l'option sur `Désactivé` et validez.
   - Revenez en arrière et dans `Longueur minimale du mot de passe :` Fixez la valeur à `5` caractères au lieu du seuil par défaut.
   - Revenez encore en arrière et dans `Âge maximal du mot de passe :` Configurez la valeur à `0` jours pour que le mot de passe n'expire jamais.
7. Enfin, pour forcer cette politique de groupe sur le contrôleur de domaine, ouvrez une invite de commandes (Win + R, puis saisissez `cmd`) et exécutez la commande :
   ```cmd
   gpupdate /force
   ```

> [!NOTE]
> la commande `gpupdate /force` permet d'appliquer immédiatement les nouvelles règles de gestion des stratégies

<details>
  <summary><strong>📸︲Visualisation de la stratégie de sécurité locale</strong></summary>
  <img src="captures/2026-09-14 15_28_12-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_28_37-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_28_54-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Accès et navigation dans l'Éditeur de GPO</strong></summary>
  <img src="captures/2026-09-14 15_36_05-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_36_51-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_38_18-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_39_54-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Modification des paramètres de sécurité des mots de passe</strong></summary>
  <img src="captures/2026-09-14 15_40_13-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_48_57-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-09-14 15_47_53-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="structurationAD"></a>
# `👥` ︲ Activité 3 : Structurer et administrer Active Directory

<a id="creationUO"></a>
## `📁` ︲ Création des unités d'organisation et des groupes

> [!NOTE]
> Les Unités d'Organisation (OU) permettent de structurer l'entreprise par services. Les noms doivent respecter la convention : `sans majuscules et sans accents`.

---

> [!ATTENTION] 
> Conformément aux consignes du projet, les branches `Stagiaires` et `Salles` ne doivent pas être créées à cette étape.

---

1. Ouvrez le `Gestionnaire de serveur` sur votre contrôleur de domaine.
2. Dans le coin supérieur droit, cliquez sur le menu `Outils`.
3. Dans la liste déroulante, cliquez sur `Utilisateurs et ordinateurs Active Directory`.
4. Pour créer une OU, faites un `clic droit` sur `descartesbleu.org`.
5. Cliquez sur `Nouveau`, puis sur sur `Unité d'organisation`.
6. Dans la fenêtre qui s'affiche :
   * Saisissez : `administratif`
   * Laissez cochée l'option `Protéger le conteneur contre une suppression accidentelle`.
   * Cliquez sur `OK`.
7. Pour créer le groupe correspondant à l'OU, cliquez sur l'icône représentant une étoile et deux personnes, puis saisissez : `grp_administratif`
8. Répétez ces mêmes opérations pour toutes les OU et Groupes

Liste des OU et des groupes à créer :

 | Unité d'organisation    | Groupe à créer      |
 | :---------------------- | :-----------------  |
 | `administratif`         | `grp_administratif` |
 | `scolarite`             | `grp_scolarite`     |
 | `formateurs`            | `grp_formateurs`    |
 | `comptabilite`          | `grp_comptabilite`  |

<details>
  <summary><strong>📸︲Création d'une Unité d'Organisation (OU)</strong></summary>
  <img src="captures/2026-10-10 15_41_59-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 15_45_04-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Création du groupe associé dans l'OU</strong></summary>
  <img src="captures/2026-10-10 16_19_51-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 16_20_34-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="creationUsers"></a>
## `👤` ︲ Création des utilisateurs

1. Pour créer les utilisateurs, faites un clic droit dans l'OU correspondante, `Nouveau` puis `Utilisateur`.
2. Entrez le bon `Prénom` et le `Nom` en majuscule.
3. La politique de nomenclature demande de respecter le format `prenom.nom` pour le login. 
4. Dans la fênetre de définition du mot de passe, tapez `prenom.nom`.
5. Décochez `L'utilisateur doit changer le mot de passe...` et cocher à la place `Le mot de passe n'expire jamais`. Cliquez sur `Suivant`, puis sur `Terminer`.
6. Enfin, pour les ajouter dans un groupe :
    * Si un seul utilisateur, faites un clic droit sur celui-ci, cliquez sur `Ajouter à un groupe...`, entrez le nom du groupe par exemple `grp_comptabilite` puis sur `OK`.
    * Si plusieurs utilisateurs doivent être ajoutés à un groupe. Sélectionnez les utilisateurs avec la touche `ctrl` puis `clic droit`, `Ajouter à un groupe...` et saisissez le nom du groupe.
    
> [!NOTE]
> Il est aussi possible de ne rentrer que `grp` et cliquez sur `Vérifier les noms`. Les groupes dont le nom commence par `grp` seront alors proposés ; vous pourrez ainsi choisir le bon.

Liste des utilisateurs à créer :

| UO              | Utilisateurs                                   |
| :---------------| :--------------------------------------------- |
| `administratif` | Patrick SALRIE, Christine ALLONGUI             |
| `scolarite`     | Amina ZOUBIER                                  |
| `formateurs`    | Gaëlle CASTEL, Sylvain CHERRIER, Valéry PERRIN |
| `comptabilite`  | Cécile LE BOHEC                                |

<details>
  <summary><strong>📸︲Création d'un utilisateur et configuration des identifiants</strong></summary>
  <img src="captures/2026-10-10 16_41_12-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 16_42_17-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 16_42_44-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 16_43_01-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Ajout des utilisateurs aux groupes correspondants</strong></summary>
  <img src="captures/2026-10-10 17_04_19-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 17_01_07-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 17_02_01-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 17_02_51-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="integrationPoste"></a>
## `💻` ︲ Intégration d'un PC client au domaine (Guide technique de A à Z)

### 1. Configuration de l'adresse IP et du serveur DNS
1. Sur le PC client Windows 11, ouvrez le menu Démarrer, tapez `Panneau de configuration`, ouvrez-le.
2. Cliquez sur `Réseau et Internet`, puis sur `Centre Réseau et partage`.
3. Dans le volet de gauche, cliquez sur `Modifier les paramètres de la carte`.
4. Faites un clic droit sur la carte réseau nommée `Ethernet` et cliquez sur `Propriétés`.
5. Dans la liste, double-cliquez sur `Protocole Internet version 4 (TCP/IPv4)`.
6. Configurez les paramètres réseau comme suit :
   * Cochez `Utiliser l'adresse IP suivante` :
     * `Adresse IP` : `172.16.0.10`
     * `Masque de sous-réseau` : `255.255.0.0`
   * Cochez `Utiliser l'adresse de serveur DNS suivante` :
     * `serveur DNS préféré` : `172.16.0.1` *(l'adresse IP du contrôleur de domaine)*
7. Cliquez sur **OK** pour enregistrer.

<details>
  <summary><strong>📸︲Accès au Panneau de configuration et aux paramètres réseau</strong></summary>
  <img src="captures/2026-10-10 17_57_18-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 17_57_32-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 17_57_39-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 17_57_51-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Configuration de l'adresse IP statique et du serveur DNS (IPv4)</strong></summary>
  <img src="captures/2026-10-10 17_58_10-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 17_58_24-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 17_58_53-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

### 2. Vérification de la connectivité réseau
1. Ouvrez l'invite de commandes (cmd).
2. Vérifiez la bonne communication avec le contrôleur de domaine en effectuant un ping :
   ```cmd
   ping 172.16.0.1
    ```

<details>
  <summary><strong>📸︲Ouverture du terminal et vérification de la configuration réseau (ipconfig)</strong></summary>
  <img src="captures/2026-10-10 17_59_29-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_00_22-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Test de connectivité avec le contrôleur de domaine (ping)</strong></summary>
  <img src="captures/2026-10-10 18_00_42-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

### 3. Jonction au domaine

1. Ouvrez les `Paramètres`, puis rendez-vous dans `Système` et `Informations système`.
2. Cliquez sur `Domaine ou groupe de travail`.
3. Dans la fenêtre qui s'affiche, sélectionnez l'option `Modifier`.
4. Dans la partie `Domaine`, saisissez `descartesbleu.org` puis validez.
5. Une fenêtre d'authentification s'affiche :
   * Entrez le compte administrateur du domaine : `Administrateur`
   * Entrez le mot de passe associé : `btssio-lmc26`
6. Un message de bienvenue dans le domaine `descartesbleu.org` doit s'afficher.
7. Redémarrez obligatoirement le PC client Windows 11 pour finaliser les modifications.

> [!CAUTION]
> Pensez à créer un instantané (snapshot) du serveur et du client après les grandes étapes.

<details>
  <summary><strong>📸︲Accès aux informations système et propriétés du domaine</strong></summary>
  <img src="captures/2026-10-10 18_01_27-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_01_43-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_01_59-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Saisie du nom de domaine et authentification administrateur</strong></summary>
  <img src="captures/2026-10-10 18_02_21-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_02_30-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_03_01-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Validation de la jonction et invitation au redémarrage</strong></summary>
  <img src="captures/2026-10-10 18_03_09-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_03_15-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_03_27-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Validation de la jonction dans utilisateurs et ordinateurs du domaine</strong>
  <img src="captures/2026-10-10 18_05_39-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

### 4. Test de connexion avec un compte du domaine

1. Au redémarrage, sur l'écran de connexion de Windows, connectez-vous avec un compte utilisateur du domaine (par exemple : `patrick.salrie` ou tout autre compte créé dans les OU).
2. Vérifiez que la session s'ouvre correctement sous le profil choisi.

<details>
  <summary><strong>📸︲Sélection de l'option "Autre utilisateur" et saisie des identifiants du domaine</strong></summary>
  <img src="captures/2026-10-10 18_06_40-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_07_42-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Ouverture de session et vérification du profil utilisateur actif</strong></summary>
  <img src="captures/2026-10-10 18_08_34-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 18_10_46-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

> [!NOTE]
> Si toutes ces étapes fonctionnent, alors le client a bien rejoint le domaine et peut ouvrir une session réseau.
---

<a id="centralisation"></a>
# `📁` ︲ Activité 4 : Centraliser les données des utilisateurs

<a id="redirectionProfils"></a>
## `🔄` ︲ Redirection des dossiers des utilisateurs

> [!NOTE]
> Pour les formateurs, il est nécessaire d'activer la redirection de dossiers pour qu'ils retrouvent leurs fichiers sur n'importe quel PC du domaine.

1. Sur le bureau de `SRV-AD1`, ouvrez l'`Explorateur de fichiers` et allez sur le disque `DATA` (qui dans mon cas porte la lettre `E:`).
2. Faites un clic droit, `Nouveau` puis `Dossier`. Nommez-le `profils`.
3. Faites un clic droit sur ce dossier `profils`, puis cliquez sur `Propriétés`.
4. Dans la fenêtre qui s'ouvre, allez dans l'onglet `Partage` puis sur `Partage avancé...`.
5. Cochez `Partager ce dossier`. Et dans `Nom du partage`, entrez `profils$`.
> [!NOTE]
> Ajouter le signe `$` à la fin permet de masquer ce partage dans la navigation réseau. 
6. Cliquez sur `Autorisations`, et cochez `Contrôle total` pour `Tout le monde`. Cliquez sur `OK` et encore une fois `OK`.
7. Allez dans l'onglet `Sécurité`, puis cliquez sur `Avancé`. Appuyez sur le bouton `Désactiver l'héritage`. Un message va s'afficher, sélectionnez `Supprimer toutes les autorisations héritées de cet objet.`. Cela va nous permettre de faire un peu de ménage.
8. Une fois cela fait, cliquez sur `Ajouter`, puis sur `Sélectionnez un principal`. Tapez `Utilisateurs du domaine` ou un mot commençant de la même manière. Sélectionnez bien `Utilisateurs du domaine` et cliquez sur `OK`.
9. La fenêtre des autorisations s'ouvre. Cochez la case `Modification`, (assurez-vous que les cases : `Lecture et exécution`, `Affichage du contenu du dossier`, `Lecture` et `Écriture` soient également cochées). Vous pouvez cliquer sur `OK` pour fermer.
10. Sur le serveur, rendez-vous sur le `Gestionnaire de serveur`. Dans `Outils`, cherchez `Gestion de stratégie de groupe`.
11. Dans la fenêtre qui s'affiche, déroulez l'arborescence : `Forêt : descartesbleu.org` et faites un clic droit sur l'OU `formateurs`.
12. Cliquez sur `Créer un objet GPO dans ce domaine, et le lier ici...`, nommez-la `Redirection dossier formateurs`, faites `OK`.
13. Clic droit sur cette GPO puis sélectionnez `Modifier...`.
14. Dans l'`éditeur de gestion de stratégie de groupe` qui s'affiche, déroulez l'arborescence : `Configuration utilisateur`, `Stratégies`, `Paramètres Windows` et enfin `Redirection de dossiers`.
15. Faites un clic droit sur `Bureau` puis `Propriétés`. Dans la liste déroulante de `paramètre`, choisissez `De base - Rediriger les dossiers de tout le monde vers le même emplacement`.
16. Dans la liste déroulante de `Emplacement du dossier cible`, choisissez `Rediriger vers l'emplacement suivant`. Puis dans `Chemin d'accès de la racine` tapez le chemin `\\SRV-AD1\profils$\%USERNAME$\Bureau` et cliquez sur `Appliquer` (faites Oui au message).
17. Ensuite, dans l'onglet `Paramètres`, décochez la case `Accorder à l'utilisateur des droits exclusifs sur Bureau`.
> [!NOTE]
> Il nous est demandé de faire de même pour les dossiers `Documents`, `Images` et `Favoris`.
> Il nous faudra donc remplacer la fin du chemin cible par le bon dossier.

> [!NOTE]
> La variable `%USERNAME%` est automatiquement remplacée par le nom de la personne connectée. Elle permet ainsi, de créer automatiquement des dossiers au nom de chaque utilisateurs.

<details>
  <summary><strong>📸︲Création du dossier profils et partage masqué (profils$)</strong></summary>
  <img src="captures/2026-10-10 19_41_59-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_42_32-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_44_27-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_44_59-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Configuration des autorisations de partage et de la sécurité avancée (NTFS)</strong></summary>
  <img src="captures/2026-10-10 19_45_34-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_46_51-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_46_59-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_47_46-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_47_56-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_48_06-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Ajustement des autorisations de sécurité avancées (NTFS)</strong></summary>
  <img src="captures/2026-10-10 19_49_01-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Création et liaison de la GPO sur l'OU formateurs</strong></summary>
  <img src="captures/2026-10-10 19_51_54-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_52_33-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_52_51-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Configuration de la redirection du dossier Bureau (Cible et Paramètres)</strong></summary>
  <img src="captures/2026-10-10 19_54_33-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_54_50-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_55_11-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_56_46-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_57_02-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-10 19_57_23-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Vérification</strong></summary>
  <img src="captures/2026-10-11 12_35_07-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 12_38_12-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>


---

<a id="modeHorsConnexion"></a>
## `🔌` ︲ Création d'une GPO pour le mode hors connexion

> [!NOTE]
> Activer le mode hors connexion permet aux utilisateurs de travailler sur leurs fichiers même s'ils ne sont pas joignables ou déconnectés du réseau.

1. Sur le serveur, rouvrez `Gestion de stratégie de groupe`.
2. Sur la fênetre qui s'affiche, faites un clic droit sur l'OU `formateurs`.
3. Cliquez sur `Créer un objet GPO dans ce domaine, et le lier ici...`, nommez-la `Acces hors connexion dossier formateurs`, faites `OK`.
4. Clic droit sur cette GPO puis sélectionnez `Modifier...`.
5. Dans l'`éditeur de gestion de stratégie de groupe` qui s'affiche, déroulez l'arborescence : `Configuration ordinateur`, `Stratégies`, `Modèles d'administration`, `Réseau` et enfin `Fichiers hors connexion`.
6. Faites un clic droit sur `Configurer le mode de liaison lente` puis cliquez sur `Modifier`.
7. Dans la fenêtre qui s'ouvre, cochez la case `Activé`.
8. Dans la zone `Options` faites défiler jusqu'en bas, cliquez sur le bouton `Afficher`. La fênetre `Afficher le contenu` s'affiche.
9. Dans la zone `Nom de la valeur`, entrez `\\SRV-AD1\profils$` pour appliquer la règle à l'ensemble de ce dossier.
10. Dans la zone `Valeur`, entrez `Latency=1` pour définir le seuil de latence à 1 milliseconde, puis cliquez sur `OK` et une nouvelle fois sur `OK`.
> [!NOTE]
> `Latency=1` force les clients à basculer en mode toujours hors connexion permanent pour un accès plus rapide.

<details>
  <summary><strong>📸︲Création et liaison de la GPO "Acces hors connexion dossier formateurs"</strong></summary>
  <img src="captures/2026-10-11 12_40_06-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 12_41_33-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 12_41_55-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Accès au paramètre "Configurer le mode de liaison lente" dans l'éditeur GPO</strong></summary>
  <img src="captures/2026-10-11 12_43_34-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 12_44_09-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Activation et configuration du chemin UNC et du seuil de latence (Latency=1)</strong></summary>
  <img src="captures/2026-10-11 12_45_56-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 12_49_24-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 12_51_29-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Gpupdate /force</strong></summary>
  <img src="captures/2026-10-11 12_56_02-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png">
  <br>
  <img src="captures/2026-10-11 12_56_12-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="automatisation"></a>
# `⚙️` ︲ Activité 5 : Automatiser l'administration des utilisateurs

<a id="creationOUStagiaires"></a>
## `📁` ︲ Création de l'OU `stagiaires`

1. Créez l'OU `stagiaires`. Comme nous l'avons vu plus haut : faire `clic droit` sur `descartesbleu.org` dans `Utilisateurs et ordinateurs Active Directory`.
2. Cliquez sur `Nouveau`, puis sur sur `Unité d'organisation`.
3. Dans la fenêtre qui s'affiche :
   * Saisissez : `stagiaires`
   * Laissez cochée l'option `Protéger le conteneur contre une suppression accidentelle`.
   * Cliquez sur `OK`.

<details>
  <summary><strong>📸︲Création de l'Unité d'Organisation (OU) stagiaires</strong></summary>
  <img src="captures/2026-10-11 15_35_09-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 15_37_38-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<a id="scriptUsers"></a>
## `📜` ︲ Création des utilisateurs, des OU et des groupes via un script PowerShell

> [!NOTE]
> Le script présenté assurera de : 
> - lire le contenu du fichier CSV `stagiaires_sisr.csv`
> - l'OU `stagiaires_sisr` dans l'OU `stagiaires`
> - le groupe `grp_stagiaires_sisr` dans l'OU `stagiaires_sisr` 
> - activer les comptes fraîchement créés.

1. Sur le serveur `SRV-AD1`, assurez-vous d'avoir déposé le fichier `stagiaires_sisr.csv`, dans les documents par exemple.
2. Cliquez sur le menu Démarrer, recherchez et ouvrez `Windows PowerShell ISE`.
3. Collez ce script dans l'éditeur :

```powershell
# Emplacement du fichier .CSV contenant toutes les informations des utilisateurs
$users = Import-Csv -Path "C:\Users\Administrateur\Documents\stagiaires_sisr.csv" -Delimiter ";"

# Création OU 'stagiaires_sisr' dans OU 'stagiaires'
New-ADOrganizationalUnit -Name "stagiaires_sisr" -Path "OU=stagiaires,DC=descartesbleu,DC=org"

# Création du groupe 'grp_stagiaires_sisr' à l’intérieur de l’OU 'stagiaires_sisr'
New-ADGroup -Name "grp_stagiaires_sisr" -GroupCategory Security -GroupScope Global -Path "OU=stagiaires_sisr,OU=stagiaires,DC=descartesbleu,DC=org"

foreach($user in $users)
{
    $displayname = $user.prenom+" "+$user.nom
    $nom = $user.nom
    $prenom = $user.prenom
    $login = ($prenom +"."+$nom).ToLower()
    $mail = $user.mail
    $mdp = $user.motdepasse

    if($login.Length -ge 20){
        # Calcul du nombre de caractères supplémentaires
        $sub = $login.Length - 20

        # Découpage du prénom
        $prenom = $prenom.Substring(0,$prenom.Length - $sub)

        # Création du nouveau login
        $login = ($prenom +"."+$nom).ToLower()
    }

    # Lecture du fichier et création des utilisateurs à l'intérieur de stagiaires_sisr et du groupe
    New-ADUser -Name $displayname -GivenName $prenom -Surname $nom -SamAccountName $login -EmailAddress $mail -AccountPassword (ConvertTo-SecureString $mdp -AsPlainText -Force) -PasswordNeverExpires $true -Path "OU=stagiaires_sisr,OU=stagiaires,DC=descartesbleu,DC=org"

    Add-ADGroupMember -Identity "grp_stagiaires_sisr" -Members $login
    
    # Activation des comptes utilisateurs créés
    Enable-ADAccount $login
}
```

<details>
  <summary><strong>📸︲Ouverture de Windows PowerShell ISE sur SRV-AD1</strong></summary>
  <img src="captures/2026-10-11 15_38_46-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 15_39_03-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Script PowerShell pour la création des OU, groupes et utilisateurs (creationUsr.ps1)</strong></summary>
  <img src="captures/2026-10-11 16_29_30-05 - Activite 5 - Automatiser l’administration des utilisateurs.docx - Word.png"/>
</details>

<a id="testscriptUsers"></a>
## `✅` ︲ Vérification de la mise en place du script

1. Sur `SRV-AD1`, ouvrez la console `Utilisateurs et ordinateurs Active Directory` pour vérifier que l'OU, le groupe et les utilisateurs ont bien été créés.
2. Sur le poste client, connectez-vous avec un compte stagiaire pour vérifier que l'authentification et l'ouverture de session fonctionnent correctement.


<details>
  <summary><strong>📸︲Vérification de la création de l'OU, du groupe et des utilisateurs dans Active Directory</strong></summary>
  <img src="captures/2026-10-11 16_35_48-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Connexion et validation de la session client avec un compte stagiaire (tai.nguyen)</strong></summary>
  <img src="captures/2026-10-11 16_37_45-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-11 16_39_28-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="ressourcesPartagees"></a>
# `🔐` ︲ Act 6 : Administrer les ressources partagées

<a id="partagesManuels"></a>
## `📂` ︲ Créer et administrer des partages de fichiers (Manuel)

Pour mettre en place l'arborescence des partages sur le serveur de fichiers (`SRV-AD1`), procédez étape par étape :

1. Sur votre serveur `SRV-AD1`, ouvrez l'`Explorateur de fichiers` et rendez-vous sur le disque `DATA` sur le lecteur (dans mon cas) `E:`.
2. Créez un dossier racine nommé `partages`, puis à l'intérieur, créez les cinq dossiers principaux correspondant aux services du centre de formation : `administratif`, `comptabilite`, `formateurs`, `scolarite` et `stagiaires`.
3. Dans chacun de ces dossiers de services, créez deux sous-dossiers `commun` et `perso`.
4. Pour le dossier `perso` du service `administratif`, créez manuellement les dossiers individuels des utilisateurs `patrick.salrie` et `christine.allongui`.
5. Pour chaque dossier de type `commun` dans `administratif`, `comptabilite` etc, configurez le partage réseau en faisant un `clic droit` puis `Propriétés`, onglet `Partage` et `Partage avancé...`.
6. Cochez `Partager ce dossier`, nommez les partages clairement (ex dans mon cas: `commun_administratif`), dans `Autorisations`, attribuez le `Contrôle total` au groupe `Tout le monde`.
7. Pour appliquer les autorisations de Sécurité, se rendre dans l'onglet `Sécurité` puis cliquer sur le bouton `Avancé`. Désactivez `l'héritage de sécurité`, puis configurez les permissions de sorte que :
   - L'administrateur conserve le `Contrôle total` (donc ne rien toucher).
   - Ajouter le groupe associé au service (ex: `grp_administratif`) et cocher `modification` et enlever `suppression`
   - Ajouter `CREATEUR PROPRIÉTAIRE` avec l'autorisation de modification afin que seul le créateur d'un fichier peut le supprimer.
8. Pour les dossiers individuels perso des utilisateurs (ex: `christine.allongui` et `patrick.salrie`), désactivez également l'héritage de sécurité. Puis ajouter les users dans les autoristaions en  cochant `Modification` pour les utilisateurs concernées.

<details>
  <summary><strong>📸︲Arborescence des dossiers principaux de service dans partages</strong></summary>
  <img src="captures/2026-11-01 21_47_11-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Sous-dossiers commun et perso d'un service</strong></summary>
  <img src="captures/2026-10-30 15_22_32-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Dossiers individuels créés manuellement dans perso</strong></summary>
  <img src="captures/2026-10-30 15_53_39-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Propriétés de partage d'un dossier commun</strong></summary>
  <img src="captures/2026-10-30 16_03_19-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-30 16_04_23-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-10-30 16_05_20-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Autorisations de partage (Contrôle total pour Tout le monde)</strong></summary>
  <img src="captures/2026-11-01 20_53_01-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Paramètres de sécurité avancés et désactivation de l'héritage</strong></summary>
  <img src="captures/2026-10-30 16_10_38-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Permissions NTFS détaillées pour le groupe associé</strong></summary>
  <img src="captures/2026-11-01 20_59_43-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Permissions NTFS pour un dossier personnel utilisateur</strong></summary>
  <img src="captures/2026-11-01 21_00_51-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

### Détail des autorisations à appliquer sur l'arborescence :

- **Dossier racine `E:\partages`** :

- **Sous-dossier `commun` de chaque service (ex: `E:\partages\administratif\commun`)** :
  * `Administrateurs` : Contrôle total.
  * `grp_administratif` : Modification, Lecture et exécution, Affichage, Lecture, Écriture. Puis décocher suppression 
  * `CREATEUR PROPRIETAIRE` : Modification, Lecture et exécution, Affichage, Lecture, Écriture.

- **Sous-dossier `perso` de chaque service (ex: `E:\partages\administratif\perso`)** :
  * Dossiers individuels des employés (ex: `patrick.salrie`, `christine.allongui`) :
  * Désactivez l'héritage sur chaque dossier personnel d'employé.
  * Donnez le `Contrôle total` à l'utilisateur concerné (ex: `christine.allongui`) et à l'`Administrateur`.

---

<a id="partagesAuto"></a>
## `🤖` ︲ Créer et administrer des partages (Automatisée)

Pour automatiser la création des dossiers personnels des stagiaires à grande échelle et appliquer les droits de sécurité NTFS associés :

1. Ouvrez l'éditeur `Windows PowerShell ISE` en tant qu'administrateur sur le serveur `SRV-AD1`.
2. Installez le module requis pour la gestion fine des autorisations NTFS (tapez `Install-Module NTFSSecurity` puis validez les invites de sécurité en choisissant `[O] Oui` ou `[T] Oui pour tout`).
3. Rédigez le script PowerShell permettant de lire le fichier CSV des stagiaires, de créer dynamiquement leurs répertoires personnels et d'isoler les permissions de sécurité en coupant l'héritage :

```powershell
$users = Import-Csv -Path "C:\Users\Administrateur\Documents\stagiaires_sisr.csv" -Delimiter ";"

$personnalPathRoot = "E:\partages\stagiaires\SIO-SISR\perso"

foreach($user in$users)
{
    $login = ($user.prenom + "." + $user.nom).ToLower()
    $nom =$user.nom
    $prenom =$user.prenom

    if($login.Length -ge 20){
        $sub =$login.Length - 20

        $prenom = $prenom.Substring(0,$prenom.Length - $sub)$login = ($prenom + "." + $nom).ToLower()
    }

    # creation repertoire user
    New-Item -ItemType Directory -Path "$personnalPathRoot\$login"

    # recuperation autorisations NTFS repertoire cree
    $acl = Get-Acl -Path "$personnalPathRoot\$login"

    # supprimer l'heritage
    $acl.SetAccessRuleProtection($true,$false)

    # attribution liste autorisations modifiees
    Set-Acl -Path "$personnalPathRoot\$login" -AclObject $acl

    Add-NTFSAccess -Path "$personnalPathRoot\$login" -Account "$login@descartesbleu.org" -AccessRights Modify, Synchronize
}
```

<details>
  <summary><strong>📸︲Installation du module NTFS</strong></summary>
  <img src="captures/2026-11-01 22_32_06-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Execution du script</strong></summary>
  <img src="captures/2026-11-01 22_50_37-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

<details>
  <summary><strong>📸︲Confirmation</strong></summary>
  <img src="captures/2026-11-01 22_50_56-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-11-01 22_51_50-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
</details>

---

<a id="redirectionStagiaires"></a>
## `🔄` ︲ Redirection de dossier et validation

1. Ouvrez le `Gestionnaire de serveur`, cliquez sur `Outils` puis sur `Gestion de stratégie de groupe`.
2. Créez un nouvel objet GPO `GPO_Redirection_Stagiaires` et liez-le sur l'Unité d'Organisation (UO) correspondante aux stagiaires (`SIO-SISR`).
3. Faites un clic droit sur la GPO et choisissez `Modifier...`.
4. Dans l'éditeur, naviguez vers : `Configuration utilisateur` > `Stratégies` > `Paramètres Windows` > `Redirection de dossiers`.
5. Faites un clic droit sur `Documents`, `Propriétés`. Dans le paramètre de base, indiquez de rediriger le dossier vers le chemin UNC du dossier personnel de chaque utilisateur `\\srv-ad1\perso_sio_sisr\%USERNAME%`. 
6. Décochez `Accorder à l'utilisateur des droits exclusifs sur Documents` dans l'onglet `Paramètres`.
7. Validez les fenêtres. Pour appliquer immédiatement la stratégie sur un poste client, ouvrez une invite de commande et tapez `gpupdate /force`.
8. Pour tester, connectez vous a un session stagiaires et y creer un document vous devriez voir un logo sur `Documents`. 

> [!ATTENTION]
> N'oubliez pas de réaliser un snapshot de la machine virtuelle du serveur à la fin de cette mission.

<details>
  <summary><strong>📸︲Configuration de la GPO de redirection de dossiers pour les stagiaires</strong></summary>
  <img src="captures/2026-11-01 23_44_04-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-11-01 23_44_10-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
  <br>
  <img src="captures/2026-11-01 23_44_21-SRV-AD1 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png">
</details>

<details>
<summary><strong>📸︲Test et verif</strong></summary>
<img src="captures/2026-11-01 23_58_29-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png"/>
<br>
<img src="captures/2026-11-02 00_00_59-S126-P01 sur STBN-DELL5510 - Connexion à un ordinateur virtuel.png">
</details>
---

**Esteban GUILLERMIN EGIDIO** | **BTS SIO SISR**