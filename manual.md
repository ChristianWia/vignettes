----
* E N   C O N S T R U C T I O N  
----
# Overview

Au moment où je suis passé j'ai pris une photo des vignettes courantes que j'ai recopiées en local dans -> https://github.com/ChristianWia/vignettes/tree/main/vignettes

j'ai référencé le début de mes investigations dans -> https://github.com/Rdatatable/data.table/issues/6221#issuecomment-2211264589


les fichiers résulats se trouvent dans -> https://github.com/ChristianWia/vignettes

Je suis parti du fichier -> datatable-intro.Rmd (il fallait bien commencer)

Recopier datatable-intro.Rmd en datatable-intro-fr.Rmd pour commencer le traitement

Les outils sont issus de -> https://github.com/mondeja/mdpo

md2po  pour générer les .pot & .po à partir du .Rmd de travail

Command:
```
md2po datatable-intro-fr.Rmd --quiet --save --po-filepath e:/datatable-intro-fr.po
```


L'utilisation de md2po extrait ce qu'il y a entre les parties de code. La découpe faite par l'outil est assez fine. Elle suit la structure du paragraphe original. Les parties à traduire sont relativement petites (1 à 3 phrases en général) ce qui est plus agréable que d'avoir à traduire des gros blocs de texte. 

## Entete Poedit

Ajouter le header Poedit au début du .po pour préparer le fichier à Poedit :

msgid ""
msgstr ""
"Project-Id-Version: cluster 2.1.6\n"
"POT-Creation-Date: 2021-08-19 20:27\n"
"PO-Revision-Date: 2024-07-27 14:59+0200\n"
"Last-Translator: Christian Wiat <w9204-rs@yahoo.com>\n"
"Language-Team: none\n"
"Language: fr\n"
"MIME-Version: 1.0\n"
"Content-Type: text/plain; charset=UTF-8\n"
"Content-Transfer-Encoding: 8bit\n"
"Plural-Forms: nplurals=2; plural=(n > 1);\n"
"X-Generator: Poedit 2.4.2\n"

Passer le fichier sous Poedit.

enlever la troncature des lignes (à la colonne 79 chez moi) dans Poedit Préférences > Avancés , pour avoir des lignes continues.

Le fichier .po est utilisable dans Poedit après quelques adaptations de l'entête

une fois satisfait on recolle les morceaux avec l'outil   po2md
à partir du fichier initial .Rmd et de ce qu'il y a dans le .po

Command:
```
po2md e:/datatable-intro-fr.Rmd --wrapwidth 0 --pofiles e:/datatable-intro-fr.po --save e:/datatable-intro-fr2.Rmd
```

ce qui donne le fichier  datatable-intro-fr2.Rmd

je vérifie mon travail en ouvrant datatable-intro-fr2.Rmd dans R studio et en passant knitr pour avoir un joli datatable-intro-fr2.html dans le navigateur.

### Problème des inclusions de code

Zut des fautes :
==> rmarkdown::render('E:/datatable-intro-fr2.Rmd',  encoding = 'UTF-8');

processing file: datatable-intro-fr2.Rmd
  |..                                                 |   3% [unnamed-chunk-1] 
Quitting from lines  at lines 73-81 [unnamed-chunk-1] (datatable-intro-fr2.Rmd)
Error in `fread()`:
! impossible de trouver la fonction "fread"
                                                                                                             
Exécution arrêtée

Ca correspond à des sections r pas fermées:
```{r
options(width = 100L)
```
```{r}

Elles correspondent soit au format ```r soit au {r avec paramètres. Il n'y en a que quelques unes et elles peuvent être rattrappées manuellement. Pour cela ouvrir en parallele dans Rstudio le fichier source d'origine datatable-intro-fr.Rmd (le directeur) et suivre les sections de datatable-intro-fr2.Rmd en se repérant aux inclusions de code.

Vérifiez bien l'entête du fichier datatable-intro-fr2.Rmd, les indentations... Elle doit être similaire à ceci:

---
title: "Introduction to data.table"
date: "`r Sys.Date()`"
output:
  markdown::html_format
vignette: >
  %\VignetteIndexEntry{Introduction to data.table}
  %\VignetteEngine{knitr::knitr}
  \usepackage[utf8]{inputenc}
---

```{r, echo=FALSE, message=FALSE, warning=FALSE}
require(data.table)
knitr::opts_chunk$set(
  comment = "#",
    error = FALSE,
     tidy = FALSE,
    cache = FALSE,
 collapse = TRUE
)
.old.th = setDTthreads(1)
```

S'il n'y a pas de fautes de génération on peut déjà avoir un aperçu dans la fenetre Viewer de Rstudio

### Problème des inclusions de code indentées

Parcourir le texte. Il peut y avoir des problèmes d'indentation du texte (présence d'un char TAB avant le texte):
  ```r
  flights["XYZ"]
  # Returns:
  #    origin year month day dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier flight tailnum ...
  # 1:    XYZ   NA    NA  NA       NA             NA        NA       NA             NA        NA      NA     NA      NA ...
  ```
c'est a dire décalé de 3 char dans le source, mais a généré ceci (les # ont perdu leur indentation) : 
   ```r
   flights["XYZ"]
# Returns:
#    origin year month day dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier flight tailnum ...
# 1:    XYZ   NA    NA  NA       NA             NA        NA       NA             NA        NA      NA     NA      NA ...
   ```
qu'il faut reprendre manuellement en :
   ```r
   flights["XYZ"]
   # Returns:
   #    origin year month day dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier flight tailnum ...
   # 1:    XYZ   NA    NA  NA       NA             NA        NA       NA             NA        NA      NA     NA      NA ...
   ```
### Problèmes des inclusion de code avec paramètres

Chercher tous les ```{r et repérer ceux qui ne sont pas fermés par } et avancer avec le bouton Next : 

On voit par exemple plusieurs fois que :
```{r
options(width = 100L)
```
n'est pas fermé et doit être corrigé (suivre le directeur) en 
```{r echo = FALSE}
options(width = 100L)
```

ou autre chose:
```{r j_cols_no_with}
ans <- flights[, c("arr_delay", "dep_delay")]
head(ans)
```

ou autre chose : 
```{r j_cols_dot_prefix}
select_cols = c("arr_delay", "dep_delay")
flights[ , ..select_cols]
```
Suivre le fichier directeur pour cela.

## Commandes R relatives aux vignettes

### Afficher les vignettes disponibles

### Installer une version officielle de data.table from CRAN

### Installer la version de développement de data.table from Github Master 

Vous pouvez utiliser la méthode décrite dans -> [Rdatatable/data.table#installation](https://github.com/Rdatatable/data.table#installation)
ou bien passer par *devtools*
```
library(devtools)
install_github("Rdatatable/data.table")
```

## Inconvénients de la méthode des .po

### Appel des phrases traduites

Un peu comme le code R appelle les messages traduits des fichiers .po , il faudrait une vignette "skeleton" qui appelle les messages traduits pour chacune des vignettes ...-fr.Rmd  ...-es.Rmd etc...

Ou on ignore complètement la mécanique des fichiers .po et on traduit manuellement et directement les blocs de texte qui se trouvent entre les inclusions de code R dans le fichier .Rmd - c'est plus simple mais on ne bénéficie plus des fonctions de Poedit.

### Shunter le processus pour les mises à jour

Le produit -fr.RMD est ici un produit secondaire du -fr.po . C'est à dire qu'il n'existe que si ce dernier est défini. L'inconvénient avec le temps, et surtout parce que ça va plus vite, est que l'on risque de corriger directement le produit -fr.Rmd en oubliant de mettre à jour le -fr.po    

## Impact de la locale sur la traduction des vignettes 

Si on part du principe que la vignette traduite est clonée sur la vignette EN (même YAML, même squelette), certains elements sont néanmoins à prendre en compte.


1 vignettes qui n'accèdent pas à des ressoucres communes : 
 no pb,  seule la vignette .Rmd est à traduire 
 l'emplacement de la vignette est libre (une fois que l'on aura statué l'arborescence des vignettes traduites) 

2 vignette qui accede à des ressources communes :
Ex: datatable-sd-usage.Rmd qui accède au contenu des répertoires ./css et ./plots
Dans ce cas la vignette traduite doit se situer parmis les autres vignettes pour bénéficier de la même arborescence.

2.1 accès au CSS :
Le CSS est partagé entre les vignettes pour le menu (actuellement). 
Il est independant de la locale mais peut etre pas toujours, si on doit différencier les scripts LTR et RTL

2.2 accès aux images ou autres medias :

2.2.1 médias indépendants de la locale :
Par exemple des image où il n'y a pas de texte anglais.
 no pb - on laisse les transclusions anglaises existantes

2.2.2 medias dépendants de la locale :
C'est le cas des schémas, des architectures, des interfaces, des flux, des tableurs... où figure du texte EN. 
De plus si le .Rmd décrit ce qu'il y a sur l'image il faut assurer la cohérence.

2.2.2.1 le .Rmd ne décrit pas ce qu'il y a sur l'image 
 no pb  on garde l'image EN

2.2.2.2 le .Rmd décrit ce qu'il y a sur l'image 

2.2.2.2.1  soit on garde l'image anglaise :
Dans ce cas, si le .Rmd décrit ce qu'il y a sur l'image, le texte traduit doit reprendre les termes anglais pour la cohérence.

2.2.2.2.2  soit on crée un image locale (*) :
Dans ce cas l'image, si le .Rmd décrit ce qu'il y a sur l'image le .Rmd doivent avoir un texte traduit pour la cohérence.

(*) Je crois qu'il est possible d'agir sur le texte contenu pour modifier les images .svg

## Issues

Jai actuellement un pb Windows lors du recollage (.. peut etre pas vous, selon les plateformes). L'issue -> https://github.com/mondeja/md-ulb-pwrap/issues/7  est acceptée par l'auteur donc ... wait and see. J'ai pris sa bibliothèque à la version juste n-1 ce qui m'oblige a corriger à la main quelques cas exotiques (fameuse question du ```r et du {r ) pour pouvoir passer. 

## Normes sur les formats

Il faut savoir que rien n'est encore défini (juillet 2024), ni au niveau de la méthode adoptée, des fichiers à fournir, de l'arborescence, du nom des fichiers, de la version de laquelle partir, ...
