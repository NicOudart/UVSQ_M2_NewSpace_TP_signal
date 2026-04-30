# ROXI : radar météorologique pour l'étude de la pluie

## Contexte scientifique

**ROXI** est un **radar météorologique** en développement au LATMOS depuis 2016.
Fonctionnant en bande X (à une fréquence de 9,42 GHz), il est conçu pour l'**étude des précipitations**.

![Illustration ROXI](img/ROXI_illustration.png)

ROXI est à **visée zénithale** : son antenne est orientée vers le zénith, dans le but d'acquérir des **profils verticaux** des propriétés des précipitations en fonction de l'élévation.
Ces profils sont acquis à intervalles de temps réguliers, afin de suivre l'évolution temporelle d'un évènement de précipitations.

Il s'agit d'un radar **impulsionnel** : il émet une impulsion électromagnétique vers le zénith, et réceptionne les échos provenant des gouttes de pluie ou des cristaux de glace rencontrés par l'impulsion.

* Le temps de retard entre l'émission de l'impulsion et la réception des échos permettra de déterminer leur **distance au radar** (correspondant ici à l'élévation).

* La durée de l'impulsion définira alors la **résolution verticale** de l'instrument.

* La puissance des échos reçus comparée à la puissance de l'impulsion émise permettra d'estimer la **réflectivité** des hydrométéores, une information utile pour déterminer la **phase** (liquide ou solide) et l'**intensité** (taux de pluie) des précipitations.

Il s'agit également d'un radar **Doppler** : en réalité il réalise des milliers de sondages verticaux en quelques secondes, afin d'estimer la vitesse des cibles par **effet Doppler**.
En effet, la différence entre les signaux acquis pour une même élévation à des instants différents est liée au mouvement des hydrométéores, et permet donc de discriminer leurs différentes **vitesses de chute**. 
Cette information est liée au diamètre et à la phase des hydrométéores, permettant ainsi la caractérisation de la **microphysique** des précipitations.

* Le taux de répétition entre 2 émissions d'impulsion définira la **portée maximale** de l'instrument (l'élévation maximale mesurable).

* Le temps entre 2 émissions d'impulsions, le nombre d'impulsions d'affilée, ainsi que la longueur d'onde du radar définiront la résolution en vitesses et la **vitesse ambigue** (vitesse maximale mesurable) du radar.
Nous détaillerons comment dans la suite.

Les données d'un radar impulsionnel Doppler comme ROXI ressemblent donc à 



Pour chaque niveau d'élévation, nous pouvons obtenir un  

## Objectifs

## Importation des données

## Analyse spectrale

## Réduction du bruit

## Filtrage

## Interprétation météorologique

### Compensation de l'altitude

### Modélisation Gaussienne

### Estimation de Z et VDop

### Exportation du résultat