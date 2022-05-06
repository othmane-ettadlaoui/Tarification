--- 
title: "La tarification a priori"
author: "Abdoul Oudouss DIAKITE, Othmane ETTADLAOUI"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  This is a minimal example of using the bookdown package to write a book.
  The HTML output format for this example is bookdown::bs4_book,
  set in the _output.yml file.
biblio-style: apalike
csl: chicago-fullnote-bibliography.csl
---


# Introduction{-}

L’assurance est une opération de risque d’un assuré à un assureur. Cette opération de transfert se fait par un paiement de prime par l’assuré à l’assureur. Ce dernier s’engage à indemniser son client en cas de survenance d’un sinistre pendant toute la période couverte par le contrat. 
La prime reçue par l’assureur doit refléter le risque qu’il est prêt à couvrir d'où la nécessité de se demander combien faut-il recevoir en prime pour assurer $\lambda$ niveau de risque ?


## Objectif

Dans ce projet, nous allons faire une étude sur des données que nous décrirons plus tard. Le but est d'appliquer différentes méthodes vues en assurance non-vie et de ressortir le meilleur modèle de tarification. Bien sûr nous allons commencer par une étude statistique de nos données ainsi qu'un ensemble de représentations graphiques.

##  Les données du projets

Cette base de données contient 16082 images d'une assurance automobile. ([_Télécharger_](https://github.com/AODiakite/Tarification/blob/master/data/base5.sas7bdat)).
Le code suivant permet de charger les données qui se trouvaient au préalable dans le dossier _Data_.

```r
library(haven)
database <- read_sas("Data/base5.sas7bdat", 
    NULL)
knitr::kable(
  head(database[,1:8], 10), booktabs = TRUE,
  caption = 'A table of the first 10 rows of our data.'
)
```



Table: (\#tab:unnamed-chunk-1)A table of the first 10 rows of our data.

| NAP| PERMIS|DEB_IMAG   |FIN_IMAG   |SEX |STATUT | CSP| USAGE|
|---:|------:|:----------|:----------|:---|:------|---:|-----:|
|  83|    332|2004-01-01 |2004-02-01 |M   |A      |  50|     3|
| 916|    333|2004-02-01 |NA         |M   |A      |  50|     3|
| 550|    173|2004-05-15 |2004-12-03 |M   |A      |  50|     2|
|  89|    364|2004-11-29 |NA         |F   |A      |  55|     2|
| 233|    426|2004-02-07 |2004-05-01 |M   |A      |  60|     1|
| 666|    429|2004-05-01 |NA         |M   |A      |  60|     1|
|  80|    461|2004-04-02 |2004-05-01 |M   |A      |  48|     3|
| 666|    462|2004-05-01 |NA         |M   |A      |  48|     3|
| 173|    405|2004-10-29 |NA         |F   |A      |  50|     2|
| 474|    386|2004-01-01 |2004-06-22 |M   |A      |  55|     2|

## Description des données
Évidemment, il est très difficile de comprendre certaines abréviations dans les données que nous venons de télécharger. Ne vous inquiétez surtout pas ! Le tableau suivant  contient la [description](https://github.com/AODiakite/Tarification/blob/master/data/D%C3%A9tails%20des%20variables.xls) de chaque colonne de la base 5 que nous appellerons dorénavant **_database_**. 


Table: (\#tab:unnamed-chunk-2)Descriptions de database.

|Description                                                              |Code     |
|:------------------------------------------------------------------------|:--------|
|age du conducteur                                                        |agecond  |
|ancienneté de permis                                                     |permis   |
|sexe du conducteur                                                       |sex      |
|statut matrimonial                                                       |statut   |
|catégorie socio-professionnelle                                          |csp      |
|usage du véhicule                                                        |usage    |
|option kilométrage limité                                                |k8000    |
|zone géographique                                                        |zone     |
|coefficient de réduction majoration (bonus/malus)                        |RM       |
|date de début d'image                                                    |deb_imag |
|date de fin d'image                                                      |fin_imag |
|nombre d'années-police                                                   |nap      |
|nombre de sinistres responsables dans les 4 années précédent l'image     |sinap1   |
|nombre de sinistres non responsables dans les 4 années précédent l'image |sinap2   |
|nombre de sinistres parking dans les 4 années précédent l'image          |sinap3   |
|nombre de sinistres incendie/vol dans les 4 années précédent l'image     |sinap4   |
|nombre de sinistres bris de glace dans les 4 années précédent l'image    |sinap5   |
|nombre de mises en demeure dans les 4 années précédent l'image           |sinap6   |
|charge de sinistres                                                      |charge   |


> Passons maintenant à l'étude statistique !
