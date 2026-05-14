# WISDOM : radar à pénétration de sol pour l'étude du sous-sol martien

![En-tête TP WISDOM](img/Header_TP_WISDOM.png)

_"On a bien réfléchi et on s'est dit que le Graal, il devait sûrement être enterré. [...] Donc, par association d'idées, s'il est enterré, la meilleure chose à faire, c'est de creuser. [...] Après, on s'est posé la question de la profondeur. [...] On est partis sur trois pieds et demi."_

**Alexandre Astier, Kaamelott livre I, épisode 77 : Le Forage**

## Contexte scientifique

En 2030, le rover de la mission **ExoMars** (ESA), nommé "Rosalind Franklin", explorera le site d'Oxia Planum **à la recherche de potentielles traces d'une vie passée sur Mars**.

Dans l'hypothèse ou la vie serait apparue sur Mars lorsqu'elle était habitable, des milliards d'années d'exposition à des conditions hostiles (radiations et oxydation) en auront fait disparaitre toute trace à sa surface.
En revanche, on peut espérer qu'**à quelques mètres de profondeur dans le sous-sol**, qui n'a pas bougé faute d'activité tectonique, **de telles traces de vie auraient été préservées**.

C'est pourquoi le rover d'ExoMars sera le tout premier équipé d'une **foreuse** capable de récolter des échantillons **jusqu'à 2 m** dans le sous-sol martien.
Ces échantillons seront remontés dans le corps du rover, où une suite d'instruments pourra les analyser à la recherche de bio-marqueurs.

_Mais comment savoir où creuser ?_

Les forages seront longs et gourmands en énergie, on veut éviter d'abimer la foreuse sur de la roche trop dure, et le nombre de tubes à échantillons est limité.
Il est donc capital pour la mission d'**avoir un a priori sur le sous-sol avant de creuser**.

C'est là qu'interviendra le **radar à pénétration de sol WISDOM**, développé au LATMOS.

Il s'agit d'un instrument qui sera capable de révéler la **structure** des premiers mètres du sous-sol martien, et de donner des indices sur sa **composition**.
Avant toute opération de forage, WISDOM sondera le proche sous-sol pour déterminer si un site est **intéressant scientifiquement** et **sans danger pour la foreuse**.
Il donnera aussi un **contexte géophysique** aux échantillons récoltés.

![Illustration WISDOM](img/WISDOM_illustration.png)

Les **antennes** de WISDOM seront situées **à l'arrière** du rover, et pointeront **vers le sol**, à environ 38 cm de la surface.

Pour sonder le sous-sol, un signal électromagnétiques sera envoyé par l'**antenne émettrice** en direction du sol.
A chaque **interface** entre matériaux de **propriétés électriques différentes** dans le sous-sol, une partie de ce signal sera renvoyé vers l'**antenne réceptrice**.

Ce signal sera plus ou moins intense suivant le **contraste de propriétés électriques**, et plus ou moins long à revenir suivant la **distance** entre les antennes et l'interface.

Il est prévu que WISDOM réalise des sondages **à intervalles réguliers** de 10 cm le long de **lignes droites**, un opération que l'on appelle "**traverses**".
Ainsi, les sondages obtenus permettront de visualiser une "coupe" du sous-sol le long de cet axe.

Afin d'ajouter une 3ème dimension à l'étude d'un site donné, il est également prévu que WISDOM réalise jusqu'à **3 traverses en parallèle**, une opération que l'on appelle "**grid**".
Les 3 vues en "coupe" obtenues permettront alors d'obtenir une visualisation quasi-3D du sous-sol.

![Animation WISDOM](img/WISDOM_animation.gif)

WISDOM est un radar **SFCW** ("Stepped Frequency Continuous Wave"), ce qui signifie



|Nota Bene|
|:-|
|Une particularité de WISDOM est qu'il s'agit d'un radar **polarimétrique**.|
|En réalité, il ne possède non pas 2 mais 4 antennes, lui permettant d'émettre et de recevoir dans **2 polarisations linéaires**.|
|Cette information supplémentaire permettra entre autre d'étudier la rugosité des interfaces dans le sous-sol.|
|Pour les besoins de ce TP, nous avons simplifié le contenu des données en ne laissant qu'une seule des 4 configurations polarimétriques possibles.|

## Objectifs

## Importation des données

### Exemple de données WISDOM

![Illustration de la campagne au Svalbard de 2022](img/Svalbard_map_illustration.png)

### Lecture du fichier

## Analyse spectrale

### FFT

![Exemple de spectre WISDOM](img/WISDOM_spectrum_example.png)

![Exemple de spectrogramme WISDOM](img/WISDOM_spectrogram_example.png)

![Exemple de sondage WISDOM brut](img/WISDOM_raw_time_series_example.png)

![Exemple de radargramme WISDOM brut](img/WISDOM_raw_radargram_example.png)

### Mesure d'espace libre

![Exemple de sondage WISDOM après retrait du free-space](img/WISDOM_freespace_removal_time_series_example.png)

![Exemple de radargramme WISDOM après retrait du free-space](img/WISDOM_freespace_removal_radargram_example.png)

### Zero-padding

![Exemple de sondage WISDOM après zero-padding](img/WISDOM_zeropadding_time_series_example.png)

![Exemple de radargramme WISDOM après zero-padding](img/WISDOM_zeropadding_radargram_example.png)

### Fenêtrage

![Exemple de sondage WISDOM après fenêtrage](img/WISDOM_windowing_time_series_example.png)

![Exemple de radargramme WISDOM après fenêtrage](img/WISDOM_windowing_radargram_example.png)

## Amélioration de la lisibilité

### Filtrage horizontal

![Exemple de sondage WISDOM après retrait de la moyenne](img/WISDOM_mean_removal_time_series_example.png)

![Exemple de radargramme WISDOM après retrait de la moyenne](img/WISDOM_mean_removal_radargram_example.png)

### Gain vertical

![Exemple de sondage WISDOM après application d'un gain vertical](img/WISDOM_vertical_gain_time_series_example.png)

![Exemple de radargramme WISDOM après application d'un gain vertical](img/WISDOM_vertical_gain_radargram_example.png)

### Interpolation horizontale

![Exemple de radargramme WISDOM après interpolation horizontale](img/WISDOM_horizontal_interpolation_radargram_example.png)

## Filtrage fréquentiel

### Filtre passe-bas

### Filtre passe-haut

## Interprétation géophysique

### Détection de pics

### Mesure de la permittivité de surface

### Estimation de la profondeur

### Coins de glace ?

## Exportation du résultat

## Conclusion