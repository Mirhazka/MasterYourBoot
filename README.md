# 🧪 Dualboot Windows 11 Famille & Kubuntu 24.04 LTS
## 🎯 Objectif
Ce dépôt documente la mise en place d'un système en **dualboot** avec :
- **Windows 11 Famille**
- **Kubuntu 24.04 LTS**

L'objectif est de créer une procédure reproductible, testée d'abord en machine virtuelle, puis appliquée sur une machine réelle.  
Les problèmes rencontrés et leurs solutions sont détaillés à chaque étape.

Ce projet s’appuie en grande partie sur le tutoriel publié par [IT-Connect](https://www.it-connect.fr/tuto-dual-boot-windows-et-linux-ubuntu-installation-sur-pc/), que j’ai adapté à mes besoins, à ma machine et à mes préférences personnelles (distribution, partitionnement, outils…).

## 🔄 Avancement du projet
- [x] Analyse du problème de boot sur Windows 11 Pro
- [x] Nettoyage via la commande `bcdedit`
- [x] Choix des OS à installer
- [x] Planification  & répartition des disques
- [ ] Laboratoire virtuel avec `VirtualBox` du dualboot
- [ ] Installation physique sur l'ordinateur du dualboot
- [ ] Configuration post-intall (GRUB, mise à jour, pilotes, logiciels, ...)
- [x] Rédaction du fichier `README.md`
- [ ] Rédaction du fichier `0-creation-cle-usb.md`
- [ ] Rédaction du fichier `1-install-windows11.md`
- [ ] Rédaction du fichier `2-install-kubuntu.md`
- [ ] Rédaction du fichier `3-configure-post-install.md`
- [ ] Rédaction du fichier `4-laboratoire-VM.md`
- [ ] Rédaction du fichier `5-installation-physique-PC.md`
- [ ] Création du dossier `Ressources` pour les images et/ou capture d'écran

## 1. 🗂 Arborescence du dépôt 
Voici comment sera organisé ce dépôt :
```
masteryourboot/
│
├── README.md
├── 0-creation-cle-usb.md
├── 1-install-windows11.md
├── 2-install-kubuntu.md
├── 3-configure-post-install.md
├── 4-laboratoire-VM.md
├── 5-installation-physique-PC.md
└── Ressources/
	├── BalenaEtcher/
	├── DoubleBoot/
	├── Kubuntu/
	├── Laboratoire/
	├── Partitionnement/
	└── Windows11/
```

## 2. ⚙️ Prérequis

- Un PC avec un **firmware** UEFI (mode UEFI activé)
- Accès aux images ISO de **Windows 11 Famille** & **Kubuntu**
- Une **clé USB** (minimum 8 Go) pour créer un support d’installation bootable.
- Une **connexion Internet** pour les mises à jour éventuelles et les outils de téléchargement.
- Droits d’administrateur sur le système.
- Logiciels qui seront uitilisés :
	- **Balena Etcher** pour rendre la clé USB Bootable
	- **VirtualBox** pour le laboratoire de test
	- **GParted** pour formater la clé USB


## 3. 🚧 Point de départ

### 💡 Motivation
Depuis longtemps, je voulais avoir un dualboot Windows/Linux.
- 🎮 **Windows** pour les jeux, le multimédia, la bureautique
- 🧠 **Linux (Kubuntu)** pour les projets techniques, la cybersécurité et l’apprentissage

### 🧨 Le déclencheur

Durant ma formation TSSR, j’ai utilisé **Windows 11 Pro** via une “astuce” (activation par ligne de commande).  
Ça a fonctionné… jusqu’à ce que les mises à jour rendent mon système instable et le boot incohérent.

> Un écran bleu s’affichait au démarrage avec **deux fois la même entrée** :
> - Windows 11 sur le volume 5
> - Windows 11 sur le volume 5

Un seul Windows était installé, mais le chargeur de démarrage était corrompu.

### 🔧 Résolution

J’ai nettoyé les entrées de boot avec `bcdedit` :
```powershell
bcdedit /enum
bcdedit /delete {identifiant}
```

> Remplacer `{identifiant}` par le GUID de l’entrée à supprimer (exemple : `{7fc2a9e0-4a0d-11ee-bc5d-806e6f6e6963}`)

Puis j'ai redémarré mon ordinateur, et je n'ai plus eu ce soucis.

### 🎯 Conclusion

J’ai décidé de :
- Réinstaller Windows et Linux proprement et avec des licences officiels
- Les installer **sur des disques séparés**
- Mieux comprendre et gérer le **boot EFI / partitions Recovery**
- Avoir un système **stable, propre et durable**

## 4. 💻 Présentation de ma tour

C’est **mon outil principal** pour apprendre, créer, expérimenter et me divertir.  
Je l’ai montée de mes mains, pièce par pièce, en fonction de mes besoins. Je m’occupe aussi de la maintenance et de l’évolution matérielle.

> Pour mes formations, je travaille sur un SSD externe dédié, que je branche sur les machines de l’école. Je peux ainsi **continuer mes projets techniques à domicile**.

### 🔧 Configuration matérielle

| Composant       | Détail                                                            |
| --------------- | ----------------------------------------------------------------- |
| Processeur      | AMD Ryzen 5 3600X                                                 |
| RAM             | G.Skill Ripjaws V Black - 4 x 8 Go (32 Go) - DDR4 3200 MHz - CL16 |
| Carte graphique | ASUS TUF GeForce RTX 3070 O8G GAMING                              |
| Carte mère      | ASUS ROG CROSSHAIR VIII HERO WIFI                                 |
| Refroidissement | ASUS ROG STRIC LC II 360 ARGB AM5                                 |
| Boîtier         | Corsair iCUE 4000X RGB Tempered Glass (Noir)                      |
| Alimentation    | ASUS ROG STRIX 850W Gold Aura Edition                             |
| OS actuel       | Windows 11 Pro (à remplacer)                                      |
| Objectif futur  | Dualboot Kubuntu + Windows 11 Famille                             |

## 5. 💽 Répartition des disques
### 📌 Actuelle

|Disque|Type|Contenu|
|---|---|---|
|0|SSD 250 Go|Windows 11 Pro (C:), EFI, Recovery|
|1|SSD 250 Go|Vide|
|2|HDD 1 To|Data 1 (I:), Logiciels Windows (W:)|
|3|HDD 1 To|Data 2 (D:), Logiciels Linux (L:)|
|4|NVMe 2 To|Jeux vidéo (J:)|

### 🗂️ Répartition souhaitée après réinstallation

|Disque|Type|Contenu|
|---|---|---|
|0|SSD 250 Go|Windows 11 **Famille**|
|1|SSD 250 Go|Kubuntu|
|2|HDD 1 To|Données + Logiciels|
|3|HDD 1 To|Données + Logiciels|
|4|NVMe 2 To|Jeux vidéo|

### 🧠 Pourquoi cette organisation ?

J’ai fait le choix de **séparer physiquement les systèmes** plutôt que de créer des partitions logiques sur un seul disque.  
Cela présente plusieurs avantages :

- ✅ **Isolation totale des OS** 
- ✅ **Maintenance simplifiée**
- ✅ **Moins de risques d’interférences**
- ✅ **Performances optimales**
- ✅ **Clarté sur l’usage de chaque disque**

> C’est une approche que je trouve **plus propre, plus claire et plus durable dans le temps**, notamment dans le cadre d’un usage mixte personnel/professionnel.

## 6. 🐧 Pourquoi **Kubuntu** ?

Je suis habitué à l’environnement Ubuntu, que j’ai utilisé durant ma précédente formation (TSSR). J’y ai développé des automatismes avec la ligne de commande, les outils réseau, le scripting et les outils système.  
Il me semblait donc logique de continuer sur une base **Ubuntu** pour assurer la compatibilité avec mes projets techniques et pédagogiques.

Cependant, je n’apprécie pas l’interface GNOME, que je trouve trop rigide, peu intuitive et trop gourmande en ressources. Je recherchais donc un environnement de bureau :

- Plus **léger** et fluide,
- Plus **moderne** visuellement,
- Plus **personnalisable** (notamment pour un thème visuel inspiré de l’univers **Cyberpunk**),
- Et surtout qui conserve la **stabilité, les outils et la logique Ubuntu**.

Après comparaison, **Kubuntu** s’est imposé comme le meilleur choix. Basé sur Ubuntu, il utilise **KDE Plasma** comme environnement de bureau, qui répond à tous mes critères :

- 💻 **Performant** : KDE Plasma est bien plus léger que GNOME et consomme moins de ressources, même sur des machines puissantes.
- 🎨 **Très personnalisable** : il permet de modifier l’apparence jusqu’aux moindres détails (thèmes, transparences, docks, animations), parfait pour créer une interface **Cyberpunk élégante et fonctionnelle**.
- 🧩 **Compatible avec mes usages futurs** : virtualisation, cybersécurité, scripting, outils DevOps… tout ce que je souhaite approfondir dans ma future formation AIS est parfaitement supporté.
- 🔧 **Stable et éprouvé** : la base Ubuntu LTS garantit stabilité et support long terme.

En résumé, **Kubuntu combine puissance, esthétique, légèreté et efficacité**, tout en m’évitant de devoir réapprendre un environnement Linux différent alors que je suis encore en phase de montée en compétence.

## 7. 🪟 Pourquoi **Windows 11 Famille** et non Pro ?

Bien que la version **Professionnel** me permette de faire plus de choses d'un point de vue administration système que la version **Famille**, pour l'usage que je vais en faire, cela ne me sera pas utile.
Si je souhaite m'entrainer et faire des test sur la partie administration système de l'environnement **Windows**, je le ferais via un laboratoire virtuel sous **Kubuntu**.

Aujourd'hui, je repars sur des bases saines avec une volonté de **compartimenter** mes usages :
- Windows 11 Famille pour :
    - 🎮 Les jeux
    - 🖋️ La bureautique
    - 🎬 Le divertissement
- Kubuntu pour :
    - 🛠️ L’administration système
    - 🧪 La virtualisation
    - 🔐 La cybersécurité
    - ⚙️ Le scripting / DevOps

J'ai donc choisi **Windows 11 Famille** puisque j'installe un dualboot avec **Kubuntu** à côté ce qui me permet de déroger à **Windows 11 Pro**.  
En plus de cela, **Windows 11 Famille** est plus léger et est parfaitement **suffisant** pour mes usages du quotidien.  
Les fonctionnalités *professionnel* dont j'aurais besoin, comme par exemple avoir la main sur les mises à jours **Windows Update**, seront géré via des scripts PowerShell.  

## ⚠️ Disclaimer
Ce projet est **personnel et pédagogique**.  
Il ne garantit ni support officiel ni compatibilité avec toutes les configurations.  
Les manipulations proposées ici peuvent entraîner une **perte de données**, un **effacement de partitions** ou un **système instable** si elles ne sont pas appliquées correctement.

**Tu es seul responsable** de ce que tu fais avec ta machine.  
Pense à toujours effectuer une **sauvegarde complète** avant toute intervention critique.

> Ce guide a été conçu avec soin, testé sur **ma configuration**, et documenté pour aider celles et ceux qui souhaitent apprendre. Mais chaque PC est différent.
