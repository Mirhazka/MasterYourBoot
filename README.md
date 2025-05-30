# 🧪 Maitrise son boot avec un dualboot Linux & Windows

## 🎯 Objectif

Ce projet a pour but de :
- Tester l'installation d'un dualboot **Windows 11 Famille / Kubuntu** dans un environnement virtualisé (VirtualBox).
- Puis de **réaliser cette installation proprement sur ma machine physique** (tour principale).
- Documenter l’ensemble du processus et les choix techniques effectués.

---

## ⚠️ Problème initial

Suite à une mise à jour de **Windows 11 Pro**, un problème est survenu au démarrage de la machine :

> Un écran bleu de sélection m'affichait **deux fois la même option** :
> - Windows 11 sur le volume 5
> - Windows 11 sur le volume 5

Alors qu’un seul système Windows était réellement installé.  
Cela rendait le démarrage **confus**, et la configuration du boot était manifestement **corrompue** ou **mal gérée** par Windows.

### 🔧 **Résolution technique**

J'ai utilisé l'outil en ligne de commande `bcdedit` pour lister et nettoyer les entrées de démarrage :
1. **Ouverture du terminal en administrateur** (PowerShell ou CMD)
2. **Affichage des entrées du chargeur de démarrage** :
```powershell
bcdedit /enum
```
3. **Identification de l’entrée fantôme** (duplicata avec la même description et identifiant différent)
4. **Suppression de l’entrée erronée** :
```powershell
bcdedit /delete {identifiant}
```

> Remplacer `{identifiant}` par le GUID de l’entrée à supprimer (exemple : `{7fc2a9e0-4a0d-11ee-bc5d-806e6f6e6963}`)

5. **Vérification de la partition système** avec l’outil de gestion des disques (`diskpart` ou `diskmgmt.msc`) pour s'assurer que rien d'anormal ne persistait.

### 🎯 Conclusion

Cette mésaventure a motivé une **réinstallation propre** de l’ensemble de mon système, avec une volonté claire de :
- Répartir les systèmes d’exploitation sur **des disques séparés** (Kubuntu / Windows)
- Mieux **maîtriser les partitions EFI et Recovery**
- Avoir un environnement **plus stable, sain et organisé**

---

## 💻 Présentation de ma tour

Ma machine principale est bien plus qu’un simple PC : c’est **mon outil central** pour explorer, apprendre, créer et me divertir.  
Je l’ai **montée moi-même de A à Z**, en sélectionnant chaque composant selon mes besoins et mes envies. Je réalise également moi-même l’entretien et le remplacement des pièces, ce qui me permet de **maîtriser totalement son évolution dans le temps**.

### 🎯 Deux usages complémentaires :
1. 🎮 **Divertissement** : jeux vidéo, multimédia, streaming, bureautique.
2. 🧠 **Projets techniques et pédagogiques** : expérimentation, virtualisation, développement, administration système et réseau, cybersécurité.

Elle constitue mon **poste de travail principal à domicile**.  
Dans le cadre de mes formations, je travaille en déplacement avec un **SSD externe dédié**, que je branche sur les machines de l’école. Cela me permet de **rapatrier et poursuivre mes projets techniques à la maison**, en profitant de la puissance et de la souplesse de ma propre tour.

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

### ⚠️ Pourquoi repartir de zéro ?

Lors d’une ancienne formation, j’ai installé **Windows 11 Pro** en suivant les conseils d’une connaissance.  
La méthode utilisée pour activer le système n'était pas claire (clé obtenue via une commande dans l’invite de commande), et pourrait être à l’origine de **problèmes récents** survenus après une mise à jour (notamment l’apparition d’un menu de double boot fantôme pour un seul système).

Je choisis désormais de **repartir proprement**, avec une installation neuve de **Windows 11 Famille**, en version officielle, car :
- c’est suffisant pour mon usage quotidien (jeux, bureautique, multimédia),
- l’aspect professionnel (Linux, virtualisation, réseau) sera entièrement assuré sous **Kubuntu**.

---

## 💽 Répartition des disques

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

Ayant plusieurs disques physiques à disposition, j’ai choisi une **compartimentation matérielle** plutôt que logicielle. Plutôt que de diviser un seul disque en plusieurs partitions, chaque système d’exploitation ou usage majeur (jeux, données, OS…) dispose de **son propre support physique**. Cela présente plusieurs avantages :

- **Séparation claire des usages** : Windows et Linux sont complètement isolés sur des SSD différents, ce qui réduit les risques d’interférences entre les deux systèmes (notamment au niveau du boot).
- **Fiabilité et sécurité** : un disque dédié par environnement permet une **meilleure tolérance aux pannes**. Si un système est corrompu ou si un disque tombe en panne, les autres ne sont pas impactés.
- **Maintenance facilitée** : je peux remplacer ou mettre à niveau un disque dur sans toucher aux autres. Par exemple, si je souhaite remplacer un de mes HDD par un SSD plus performant, je peux **cloner le disque entier** très facilement sans me soucier d'autres partitions ou systèmes qui pourraient être dessus.
- **Performances optimisées** : les accès disques sont mieux répartis, en particulier entre le système et les jeux, qui bénéficient du NVMe.

C’est une approche que je trouve **plus propre, plus claire et plus durable dans le temps**, notamment dans le cadre d’un usage mixte personnel/professionnel.

---

## 🐧 Pourquoi **Kubuntu** ?

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

---

## 🪟 Pourquoi **Windows 11 Famille** et non Pro ?

Lors de ma précédente formation (TSSR), j’ai été encouragé à utiliser **Windows 11 Pro** pour ses fonctionnalités orientées entreprise (comme les GPO, Hyper-V, ou l’intégration à un domaine). Cependant, je ne souhaitais pas acheter une licence uniquement pour la formation, et j’ai malheureusement suivi les conseils d’un camarade qui m’a proposé une “astuce” pour activer Pro via une ligne de commande.

Même si cela a fonctionné temporairement, cela a rapidement causé de nombreux problèmes après des mises à jour Windows : bugs, ralentissements, et surtout des perturbations du **double boot**, ce qui a nui à la stabilité de ma machine.

Aujourd’hui, je repars sur des bases saines avec une volonté claire de **compartimenter** mes usages :

- Windows 11 Famille pour :
    - 🎮 Les jeux
    - 🖋️ La bureautique
    - 🎬 Le divertissement
- Kubuntu pour :
    - 🛠️ L’administration système
    - 🧪 La virtualisation
    - 🔐 La cybersécurité
    - ⚙️ Le scripting / DevOps

Windows 11 **Famille** est plus **léger**, plus **stable**, et parfaitement **suffisant** pour mes usages du quotidien. Les fonctionnalités “Pro” dont j’aurais éventuellement besoin seront entièrement gérées dans des machines virtuelles Linux, bien plus adaptées à l’apprentissage et à l’expérimentation.

---
