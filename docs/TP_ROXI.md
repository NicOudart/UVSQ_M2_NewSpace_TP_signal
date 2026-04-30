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

Les données d'un radar impulsionnel Doppler comme ROXI ressemblent donc à une matrice 2D :

![](img/.png)

* Chaque élément de la matrice est un **nombre complexe** I+Q (In-phase / Quadrature-phase) contenant les informations d'**amplitude** et de **phase** des échos reçus pour un temps de retard et un sondage vertical donné.

* L'axe vertical correspond aux temps de retard des échos pour un sondage, aussi appelé "**fast-time**".

* L'axe horizotal correspond aux temps entre les sondages (répétitions d'impulsion), aussi appelé "**slow-time**".

Pour chaque niveau d'élévation, nous pouvons obtenir une présentation de la puissance reçue par le radar en fonction de la vitesse Doppler mesurée, appelée **spectre Doppler**.

Ces spectres peuvent ensuite être concaténés verticalement afin d'obtenir une représentation sous la forme d'une image appelée **spectrogramme Doppler**.

![](img/.png)

On peut voir chaque spectre Doppler comme la distribution des puissances reçues par le radar par rapport à la vitesse des hydrométéores au sein d'un volume sondé.
Il est courant d'essayer de modéliser cette distribution par un modèle Gaussien, afin d'en tirer 2 caractéristiques des précipitations : leur réfléctivité moyenne **Z**, et leur vitesse moyenne **VDop**.

De ces caractéristiques pourrons être inférées des **grandeurs météorologiques** d'intérêt : le régime de précipitation, sa phase, son intensité, etc.

## Objectifs

Lors de ce tutoriel, nous allons programmer une **chaîne de traitement des données de ROXI** sous la forme d'un **projet Python**, que nous utiliserons pour obtenir une **interprétation météorologique** classique.

Ce projet Python devra contenir des fonctions pour :

* Importer des données ROXI à partir d'un fichier HDF5 tel que celui qui vous sera fourni.

* Appliquer un filtre "anti-clutter" aux données brutes I+Q.

* Convertir en spectres Doppler les données I+Q acquises par le radar.

* Réaliser une intégration incohérente des spectres obtenus pour réduire le bruit.

* 

## Importation des données

## Analyse spectrale

## Réduction du bruit

## Filtrage

## Interprétation météorologique

### Compensation de l'altitude

### Modélisation Gaussienne

### Estimation de Z et VDop

### Exportation du résultat