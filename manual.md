----
* E N   C O N S T R U C T I O N  
----
Au moment où je suis passé j'ai pris une photo des vignettes courantes que j'ai recopiées en local dans -> https://github.com/ChristianWia/vignettes/tree/main/vignettes

j'ai référencé le début de mes investigations dans -> https://github.com/Rdatatable/data.table/issues/6221#issuecomment-2211264589


les fichiers résulats se trouvent dans -> https://github.com/ChristianWia/vignettes

Je suis parti du fichier -> datatable-intro.Rmd (il fallait bien commencer)

Les outils sont issus de -> https://github.com/mondeja/mdpo

md2po  pour générer les .pot & .po

Command:
```
md2po datatable-intro-fr.Rmd --quiet --save --po-filepath e:/datatable-intro-fr.po
```

L'utilisation de md2po extrait ce qu'il y a entre les parties de code. La découpe faite par l'outil est assez fine. Elle suit la structure du paragraphe original. Les parties à traduire sont relativement petites (1 à 3 phrases en général) ce qui est plus agréable que d'avoir à traduire des gros blocs de texte. 

enlever la troncature des lignes (à la colonne 79 chez moi) dans les préférences Poedit pour avoir des lignes continues.

Le fichier .po est utilisable dans Poedit après quelques adaptations de l'entête

une fois satisfait on recolle les morceaux avec l'outil   po2md
à partir du fichier initial .Rmd et de ce qu'il y a dans le .po

Command:
```
po2md e:/datatable-intro-fr.Rmd --pofiles e:/datatable-intro-fr.po --save e:/datatable-intro-fr2.Rmd
```

ce qui donne le fichier  datatable-intro-fr2.Rmd

je vérifie mon travail en ouvrant datatable-intro-fr2.Rmd dans R studio et en passant knitr pour avoir un joli datatable-intro-fr2.html dans le navigateur.

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


## Issues

Jai actuellement un pb Windows lors du recollage (.. peut etre pas vous, selon les plateformes). L'issue -> https://github.com/mondeja/md-ulb-pwrap/issues/7  est acceptée par l'auteur donc ... wait and see. J'ai pris sa bibliothèque à la version juste n-1 ce qui m'oblige a corriger à la main quelques cas exotiques (fameuse question du ```r et du {r ) pour pouvoir passer. 

## Normes sur les formats

Il faut savoir que rien n'est encore défini (juillet 2024), ni au niveau de la méthode adoptée, des fichiers à fournir, de l'arborescence, du nom des fichiers, de la version de laquelle partir, ...
