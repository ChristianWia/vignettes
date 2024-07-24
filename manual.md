----
* E N   C O N S T R U C T I O N  
----
Au moment où je suis passé j'ai pris une photo des vignettes courantes que j'ai recopiées en local dans -> https://github.com/ChristianWia/vignettes/tree/main/vignettes

j'ai référencé le début de mes investigations dans -> https://github.com/Rdatatable/data.table/issues/6221#issuecomment-2211264589


les fichiers résulats se trouvent dans -> https://github.com/ChristianWia/vignettes

Je suis parti du fichier -> datatable-intro.Rmd (il fallait bien commencer)

Les outils sont issus de -> https://github.com/mondeja/mdpo

md2po  pour générer les .pot & .po

L'utilisation de md2po extrait ce qu'il y a entre les parties de code. La découpe faite par l'outil est assez fine. Elle suit la structure du paragraphe original. Les parties à traduire sont relativement petites (1 à 3 phrases en général) ce qui est plus agréable que d'avoir à traduire des gros blocs de texte. 

Le fichier .po est utilisable dans Poedit après quelques adaptations de l'entête

une fois satisfait on recolle les morceaux avec l'outil   po2md
à partir du fichier initial .Rmd et de ce qu'il y a dans le .po

ce qui donne le fichier  datatable-intro-fr2.Rmd

je vérifie mon travail en passant knitr et avoir un joli datatable-intro-fr2.html
dynamique dans le navigateur.

## Issues

Jai actuellement un pb Windows lors du recollage (.. peut etre pas vous, selon les plateformes). L'issue -> https://github.com/mondeja/md-ulb-pwrap/issues/7  est acceptée par l'auteur donc ... wait and see. J'ai pris sa bibliothèque à la version juste n-1 ce qui m'oblige a corriger à la main quelques cas exotiques (fameuse question du ```r et du {r ) pour pouvoir passer. 

## Normes sur les formats

Il faut savoir que rien n'est encore défini (juillet 2024), ni au niveau de la méthode adoptée, des fichiers à fournir, de l'arborescence, du nom des fichiers, de la version de laquelle partir, ...
