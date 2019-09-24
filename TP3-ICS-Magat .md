# Victor Magat - TP3

## Exercice 1. Commandes de base

**1. Quels sont les 5 derniers paquets installés sur votre machine ?**

Voici la commande permettant de réaliser cela `cat /var/log/dpkg.log | grep " install " | head -5` Le fichier de log liste toutes les commandes effectués. Avec le grep on filtre seulement celles d'installation et on affiche seulement les 5 premières grâce à head.

**2. Utiliser dpkg et apt pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel!). 
Comment explique-t-on la (petite) différence de comptage ?**

Avec dpkg on peux avoir le nombre de paquets installés à l'aide de cette commande `dpkg -l | grep "ii" | wc -l` où on en obtient 515.

Avec apt on peux obtenir ce nombre via `apt list --installed | wc -l` qui nous donne un résultat de 516 paquets installés.

Cette différence s'explique par la première ligne présente dans la liste avec apt qui nous informe qu'il est en train de lister tous les paquets.

**3. Combien de paquets sont disponibles en téléchargement?**

Grâce à la commande `apt list | wc -l` on peux lister tous les paquets en téléchargements et on en obtient 64118

**4. Créer un alias “maj” qui met à jour le système**

Il suffit de créer ou d'éditer le fichier `.bash_aliases` et de rajouter cette ligne `alias maj="sudo apt full-upgrade"`

**5. A quoi sert le paquet fortunes? Installez-le.**

Les fortunes sont de petits messages, des citations, des proverbes, etc. affichés à chaque connexion dans le terminal.
`sudo apt install fortunes-fr` pour installer le paquet.

**6. Quels paquets proposent de jouer au sudoku?**

Avec la commande `apt list | grep "sudoku"` on retrouve trois paquets permettant de jouer au sudoku qui sont gnome-sudok, ksudoku et sudoku 

**7. Lister les derniers paquets installés explicitement avec la commande apt install**



## Exercice 2.

**A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule
commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des
commandes utiles) ? Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension
.sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.**

La commande **ls** est installée depuis le package **coreutils**. Pour le savoir, il faut d'abord utiliser la commande <code>which -a ls</code> qui nous affiche le chemin absolu de la commande **ls** (dans ce cas la, il existe deux commandes **ls**, nous recevons donc deux résultats). Après avoir récupéré le résultat qui nous intéresse (*/bin/ls*), nous allons exécuter la commande 
<code>dpkg -S /bin/ls</code>, qui nous indique donc que la commande **ls** fait partie du package **coreutils**
 
Pour arriver à ce résultat en une seule commande et pour n'importe quel programme, il faut utiliser les commandes
<code>which -a *programme* | xargs dpkg -S 2>/dev/null</code>, ou *programme* est le programme dont nous souhaitons connaitre le package d'installation. **xargs** permet de récuperer les résultats de la première commande et de les passer en argument à la deuxième. **2>/dev/null** permet de supprimer les erreurs du résultat.
 
Script origine-commande :
 
```bash
#!/bin/bash
 
echo $(which -a $1 | xargs dpkg -S 2>/dev/null)
```


## Exercice 3.

**Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package
spécifié dans cette commande.**

```bash
#!/bin/bash
if [ $# = 1 ] ; then
    result=$(dpkg -l | grep -w "ii  $1 " | wc -l)
    if [ $result = 0 ] ; then
        echo "NON INSTALLE"
    else
        echo "INSTALLE"
    fi
else
    echo "Arguments non valide
fi
```

On peut aussi utiliser cette commande `(dpkg -l "nom_package" | grep "^ii") && echo "installé" || echo "non installé"`

## Exercice 4.

**Lister les programmes livrés avec coreutils. A quoi sert la commande ’[’ et comment afficher ce qu’elle retourne ?**
 
Tous les programmes livrés avec coreutils sont listés grâce à la commande `dpkg -L coreutils | grep bin`.
La commande “[“ est un synonyme de la commande “test” qui permet de vérifier des conditions.

## Exercice 5. aptitude

**Installez le paquet emacs à l’aide de la version graphique d’aptitude.**

Après avoir installé le paquet grâce à `sudo apt install aptitude` et l'avoir lancé avec `sudo aptitude`. Il faut rechercher `emacs` grâce à **/ (Shift + :)**

## Exercice 6. Installation d’un paquet par PPA
**Certains logiciels ne figurent pas dans les dépôts officiels. C’est le cas par exemple de la version ”officielle”
de Java depuis qu’elle est développée par Oracle. Dans ces cas, on peut parfois se tourner vers un ”dépôt
personnel” ou PPA.**

**1. Installer la version Oracle de Java (avec l’ajout des PPA)**

sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-installer

**2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?**

Il contient les liens vers le ppa (dépôts personnels de paquets logiciels ou Personal Package Archives)

