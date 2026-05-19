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

Chaque sondage permettra donc de récupérer un profil d'**amplitudes et de temps de retard des échos reçus**.
En réalisant des sondages à mesure que le rover se déplace, on obtiendra une série de profils pour différentes **distances horizontales** depuis le point de départ.

* A partir des **temps de retard** et de la **distance horizontale**, on obtiendra une 1ère idée de la **structure du sous-sol** (strates, hétérogénéité, inclusions, etc.), mais sans réelles estimations de profondeurs.

* A partir des amplitudes des échos, le contraste de **propriétés électriques** à l'interface entre 2 couches du sous-sol pourra être estimé, donnant une **indication de la composition** des différentes strates (roches volcaniques, roches sédimentaires, glace, etc.).

* La vitesse des ondes électromagnétiques dans un matériau dépendant en 1ère approximation de sa **permittivité diélectrique**, on pourra alors convertir les temps de retard des échos en **profondeurs** des interfaces.

Il est prévu que WISDOM réalise des sondages **à intervalles réguliers** de 10 cm le long de **lignes droites**, un opération que l'on appelle "**traverses**".
Ainsi, les sondages obtenus permettront de visualiser une "coupe" du sous-sol le long de cet axe.

Afin d'ajouter une 3ème dimension à l'étude d'un site donné, il est également prévu que WISDOM réalise jusqu'à **3 traverses en parallèle**, une opération que l'on appelle "**grid**".
Les 3 vues en "coupe" obtenues permettront alors d'obtenir une visualisation quasi-3D du sous-sol.

![Animation WISDOM](img/WISDOM_animation.gif)

WISDOM est un radar **SFCW** ("Stepped Frequency Continuous Wave"), ce qui signifie qu'il n'émet pas une impulsion comme ROXI, mais des **signaux harmoniques continus de fréquences croissantes**, avec un pas régulier.

Pour chaque fréquence, le radar reçoit en retour un signal de même fréquence, résultant de l'interférence des différents échos aux différentes interfaces dans le sous-sol.
Son amplitude et sa phase seront enregistrés par l'instrument.

Ce type de radar fonctionne donc en **domaine fréquentiel** : avec ses mesures, il reconstruit l'**équivalent fréquentiel** de la **réponse impulsionnelle** du milieu sondé.
La **transformée de Fourier** des amplitudes et phases obtenues pour les différentes les fréquences permet de reconstituer la **série temporelle** qu'aurait obtenu un **radar impulsionnel équivalent** en sondant le même sous-sol.

C'est pourquoi on parle de "**spectres**" pour désigner les acquisitions de WISDOM pour un sondage donné.
La série temporelle obtenue après transformée de Fourier correspondra aux **échos** reçus pour différents **temps de retards**.

Cette méthode permet d'obtenir un radar nécessitant **moins de puissance**, avec des **signaux plus simples** à générer, ayant un **rapport signal / bruit** et une **dynamique meilleure** qu'un radar impulsionnel de résolution équivalente.
Bref, un instrument **plus compact** et **moins consommateur** en puissance : de gros avantages pour un instrument spatial !

|Nota Bene|
|:-|
|Un radar SFCW classique construit un complexe I/Q contenant les informations d'amplitude et de phase des signaux reçus pour chaque fréquence.|
|Mais ceci nécessite 2 chaînes d'acquisitions distinctes.|
|Par soucis de compacité de l'instrument, WISDOM ne mesure que la partie réelle I du signal.|
|Nous verrons dans la suite comment faire une analyse spectrale d'un signal réel.|

Les données WISDOM consistent donc en des spectres acquis à intervalles réguliers de distance, pour former une matrice 2D appellée "**spectrogramme**".

Après transformation de chaque spectre en série temporelle, on obtient une représentation des échos reçus en fonction du temps pour les différents sondages d'un traverse, appelé "**radargramme**".
Comme mentionné précédemment, l'axe des temps de retards pourra être converti en **profondeurs**, afin d'obtenir une vue en "coupe" du sous-sol.

3 radargrammes acquis en parallèle constitueront une **grid** WISDOM.

![Génération d'un radargramme](img/Radargram_generation_schematic.png)

Lors de la mission ExoMars, il est prévu qu'une opération **grid** WISDOM soit réalisée à proximité d'un **affleurement** rocheux d'intérêt : une **roche sédimentaire argileuse** qui semble ressortir du régolith de surface.
Une telle roche, formée à l'époque où Mars était encore habitable, pourrait renfermer des **traces d'une hypothétique vie passée**.

Les 3 vues en coupes obtenues grâce à WISDOM pourront confirmer si cet affleurement rocheux **s'enfonce jusqu'à 2 m** dans le sous-sol, profondeur nécessaire à la **préservation de bio-marqueurs**.
Si tel est le cas, l'équipe WISDOM pourra indiquer **où creuser** pour récupérer **un échantillon** de cette roche.

Voici les caractéristiques du radar WISDOM :

|Caractéristique                                |Valeur       |
|:---------------------------------------------:|:-----------:|
|Fréquences d'émission                          |0.5 à 3 GHz  |
|Pas de fréquence d'émission                    |2.5 MHz      |
|Nombre de pas de fréquence                     |1001         |
|Durée d'un pas de fréquence                    |200 µs       |
|Puissance d'émission                           |1 mW typique |
|Gain des antennes                              |0 dBi minimum|
|Résolution en distance dans le vide            |6 cm         |
|Distance ambiguë dans le vide                  |30 m         |
|Résolution en distance typique dans le sous-sol|3 cm         |
|Portée typique dans le sous-sol                |3 m          |

Pour plus d'informations : [site de WISDOM](https://www.wisdom-radar.eu).

|Nota Bene|
|:-|
|Une particularité de WISDOM est qu'il s'agit d'un radar **polarimétrique**.|
|En réalité, il ne possède non pas 2 mais 4 antennes, lui permettant d'émettre et de recevoir dans **2 polarisations linéaires**.|
|Cette information supplémentaire permettra entre autre d'étudier la rugosité des interfaces dans le sous-sol.|
|Pour les besoins de ce TP, nous avons simplifié le contenu des données en ne laissant qu'une seule des 4 configurations polarimétriques possibles.|

## Objectifs

Lors de ce tutoriel, nous allons programmer une **chaîne de traitement des données de WISDOM** sous la forme d'un **projet Python**, que nous utiliserons pour obtenir une **interprétation géophysique** classique.

Ce projet Python devra contenir des fonctions pour :

* Importer des données WISDOM à partir d'un fichier HDF5 tel que celui qui vous sera fourni.

* Convertir en radargrammes les traverses de spectres (spectrogrammes) acquis par le radar, en compensant les effets instrumentaux.

* Filtrer les échos parasites horizontaux.

* Appliquer un gain vertical pour compenser les pertes dans le sous-sol.

* Estimer la permittivité diélectrique du sous-sol à partir de la calibration du radar, et en déduire la conversion des temps de retard en profondeurs.

* Exporter les radargrammes obtenus sous la forme d'un fichier HDF5.

Créez le dossier de votre projet, et structurez-le avec des sous-dossiers et des fichiers vides pour l'instant.

**Pour faire simple, n'ajouterons pas de tests ou de documentation à notre projet Python.**

|Nota Bene|
|:-|
|Il est à noter que notre exemple ici a été grandement simplifié pour les besoins de ce TP.|
|Certains traitements, tels que la correction de l'effet de la température, ont déjà été appliqués aux données fournies.|
|De plus, lors de l'interprétation, nous allons faire des hypothèses simplificatrices sur les propriétés électriques du sous-sol, et la propagation des signaux WISDOM à travers celui-ci.|


## Importation des données

### Exemple de données WISDOM

Vous trouverez un exemple de fichier de **données WISDOM** au format **HDF5** [ici](https://github.com/NicOudart/UVSQ_M2_NewSpace_TP_signal/blob/master/example/WISDOM_20220315.h5).

On considérera qu'un fichier de données WISDOM issu d'une opération de "**grid**" contient toujours :

* `frequency_axis` : un "Dataset", vecteur de dimension 1001, contenant l'**axe des fréquences** (Hz) associé aux spectres WISDOM.

* `free_space` : un "Dataset", vecteur de dimension 1001, contenant un spectre réel (homogène à des volts) acquis dans une situation de "d'**espace libre**".
Cette acquisition nous servira lors du traitement de nos données.

* `calibration` : un "Dataset", vecteur de dimension 1001, contenant un spectre réel (homogène à des volts) acquis lors d'un **étalonnage** sur plaque métallique.
Cette acquisition nous servira lors de l'interprétation de nos données.

* `traverse_1`, `traverse_2` et `traverse_3` : 3 "Groups" pour les **3 "traverses"** de la "grid", contenant chacun un "Dataset" nommé `horizontal_distance_axis`, vecteur correspondant à l'axe des **distances horizontales** parcourues par WISDOM (m) pour ce "traverse", et un "Dataset" nommé `data`, matrice 2D correspondant au **spectrogramme** acquis pour ce "traverse". 

Ces données ont été acquises le **15/03/2022**, sur un **terrain polygonal** de la vallée d'**Adventdalen**, lors d'une campagne de test de WISDOM au **Svalbard**.

![Illustration de la campagne au Svalbard de 2022](img/Svalbard_map_illustration.png)

Les terrains **polygonaux** sont une formation géologique classique des plaines arctiques.

Le processus de gel / dégel du proche sous-sol (la zone dite "active") provoque des contraction / décontraction de celui-ci, entrainant des fissures dessinant des "polygones" irrégulier à sa surface.
Ces "polygones" sont clairement visibles sur les images aériennes du terrain, prises pendant l'été.

De l'eau peut s'infiltrer dans ces fissures, jusqu'à atteindre le pergélisol où elle se retrouve piégée sous forme de glace.
Se forme alors au fil des gels / dégels un cône appelé "**coin de glace**".

Si des terrains polygonaux ont déjà été détectés sur Mars, la présence de coins de glace reste débattue.
Dans le cas où de tels formations existeraient sur Mars, elles seraient une **cible de choix pour la recherche de traces de vie**.

![campagne au Svalbard de 2022](img/Svalbard_campaign_illustration.png)

Lors de la campagne au Svalbard de 2022, notre terrain polygonal d'étude était **entièrement recouvert de neige**, d'une épaisseur de 10 cm au milieu des polygones.

Une opération de "**grid**" a été réalisé avec une copie de secours ("flight spare") de WISDOM. 
3 traverses parallèles d'environ **25 m** ont été acquis avec un pas de **20 cm** en travers d'un "polygone", de manière à passer au-dessus des sillons le délimitant.

Nous espérons ainsi observer les sillons sous la neige, et potentiellement détecter un "**coin de glace**" en-dessous. 

Ce fichier nous servira d'exemple pour construire notre **chaîne de traitement des données WISDOM**.

|Nota Bene|
|:-|
|En réalité, les données WISDOM sont enregistrées spectre par spectre dans des fichiers binaires séparés, et non pas rassemblées dans un seul fichier HDF5.|
|Chaque fichier contient également des métadonnées qui ne vous sont pas fournies ici.|
|Pour les besoins de ce TP, nous avons donc fait en sorte de vous faire manipuler un format utile, et simplifié le contenu des données WISDOM.|

### Lecture du fichier

La 1ère étape de notre de chaîne traitement sera d'importer les données WISDOM d'un fichier HDF5.

|Ajoutez à votre projet Python une fonction `read`|
|:-|
|Cette section vous donnera les éléments nécessaires pour la compléter.|
|- Entrées :  le chemin d'un fichier HDF5.|
|- Sorties : 9 matrices `numpy`, correspondants aux 9 "Datasets" contenus dans le fichier.|

Pour lire et écrire un fichier HDF5, nous utiliserons la bibliothèque Python `h5py`.

Nous convertirons les "Datasets" contenus dans un fichier en matrices `numpy`.

N'oubliez donc pas d'importer ces 2 bibliothèques avec les commandes suivantes :

~~~
import h5py
import numpy as np
~~~

Pour importer un fichier HDF5 avec `h5py`, on crée un objet `File`.

Voici ce que cela donnerait sur notre exemple :

~~~
hf = h5py.File(".../WISDOM_20220315.h5",'r')
~~~

On peut alors récupérer les différents "Datasets" contenus dans le fichier avec la méthode `get` et leurs noms.
Pour les "Datasets" contenus dans des "Groups", il faudra utiliser `get` pour récupérer chaque "Group", puis à nouveau `get` pour récupérer les "Datasets" de chaque "Group".
On oubliera pas de convertir en matrices `numpy` les "Datasets" récupérés.

Voici ce que pourrait donner la récupération des 9 "Datasets" d'un fichier WISDOM :

~~~
frequency_axis = np.array(hf.get("frequency_axis"))

vect_free_space = np.array(hf.get("free_space"))
vect_calibration = np.array(hf.get("calibration"))

group1 = hf.get("traverse_1")
group2 = hf.get("traverse_2")
group3 = hf.get("traverse_3")

mat_traverse_1 = np.array(group1.get("data"))
mat_traverse_2 = np.array(group2.get("data"))
mat_traverse_3 = np.array(group3.get("data"))

distance_axis_1 = np.array(group1.get("horizontal_distance_axis"))
distance_axis_2 = np.array(group2.get("horizontal_distance_axis"))
distance_axis_3 = np.array(group3.get("horizontal_distance_axis"))
~~~

**Vous pouvez à présent compléter votre fonction `read`**.

Appliquez votre fonction à notre fichier exemple.

_Les dimensions des matrices récupérées sont-elles bien celles attendues ? Le type des données également ? Quelle est la longueur de chaque traverse ?_

Petite question pour voir si vous avez bien compris le fonctionnement du format HDF5 :

_A votre avis, pourquoi avoir rangé l'axe des distances horizontale dans le "Group" de chaque traverse, et pas l'axe des fréquences ?_

## Analyse spectrale

La 2nde étape de notre de chaîne traitement sera de générer des radargrammes pour chaque traverse contenu dans les données WISDOM récupérées.

|Ajoutez à votre projet Python une fonction `FFT`|
|:-|
|Cette section vous donnera les éléments nécessaires pour la compléter.|
|- Entrées : 4 matrices `numpy` contenant les 3 spectrogrammes des 3 "traverses" et l'axe des fréquences, telles que retournées par la fonction `read`.|
|- Sorties : 4 matrices `numpy`, contenant les 3 radargrammes des 3 "traverses", et l'axe des temps de retard correspondant.|

Afin d'obtenir des radargrammes interprétables, nous appliquerons dans cette fonction 3 techniques en amont de la  transformée de Fourier :

* Le retrait d'une mesure d'**espace libre**, afin de réduire les échos parasites provenant de l'instrument et son support.

* Le **zero-padding**, afin d'avoir des radargrammes interpolés verticalement.

* Le **fenêtrage**, afin d'éviter les problèmes liés aux "lobes secondaires".

### Nature des données

Dans un 1er temps, nous allons nous intéresser à la nature des données dont nous disposons.

Importez la bibliothèque `matplotlib`, qui va nous servir à faire des affichages graphiques :

~~~
import matplotlib.pyplot as plt
~~~

Essayons tout d'abord d'afficher un spectre issu de nos données.

Prenons le 103ème sondage du 1er "traverse", et regardons le spectre mesuré.
Affichons ce signal avec des commandes Python de ce genre :

~~~
plt.figure()
plt.plot(frequency_axis,mat_traverse_1[102],'r-')
plt.xlabel('Frequency (Hz)',fontsize=12)
plt.ylabel('Unicalibrated voltage (V)',fontsize=12)
plt.title('Spectrum - Traverse 1 - Sounding 102',fontsize=12)
plt.grid()
plt.show()
~~~

On obtient alors la courbe suivante :

![Exemple de spectre WISDOM](img/WISDOM_spectrum_example.png)

On voit nettement qu'un spectre WISDOM est un signal réel **périodique**.

Un spectre WISDOM correspond dans le domaine temporel à une serie d'impulsions renvoyées par les différentes interfaces du sous-sol, reçues pour différents temps de retard par rapport à l'émission.
On s'attend donc à obtenir une **somme de sinusoïdes**, dont la fréquence est d'autant plus élevée que le temps de retard de l'écho est grand.

D'où l'intérêt de vouloir réaliser une **analyse spectrale** de ce signal, afin de séparer temporellement les différents échos reçus par le radar.

|Nota Bene|
|:-|
|Considérer les spectres WISDOM comme une somme de sinusoïdes est en réalité une approximation de la réalité.|
|En effet, des limitations techniques font que WISDOM n'émet pas tout à fait les différentes fréquences avec la même puissance.|
|Ensuite, les pertes dans le sous-sol (absorption et diffusion) dépendent de la fréquence.|
|Tout ceci explique le fait que les basses fréquences ont l'air d'avoir une amplitude plus élevée que les hautes fréquences.|

Pour vérifier que l'on retrouve bien un signal périodique pour chaque spectre du 1er traverse, nous pouvons aussi afficher le spectrogramme.

Vous pouvez réaliser cet affichage avec des commandes Python de ce genre :

~~~
plt.figure()
plt.pcolormesh(distance_axis_1,frequency_axis,mat_traverse_1.T,cmap='bwr',vmin=-np.percentile(np.abs(mat_traverse_1),99.9),vmax=np.percentile(np.abs(mat_traverse_1),99.9))
plt.xlabel('Horizontal distance (m)',fontsize=12)
plt.ylabel('Frequency (Hz)',fontsize=12)
plt.colorbar(label='Uncalibrated voltage (V)')
plt.title('Spectrogram - Traverse 1',fontsize=12)
plt.grid()
plt.show()
~~~

On obtient alors l'affichage graphique suivant, qui confirme ce que nous avions observé sur le 103ème sondage :

![Exemple de spectrogramme WISDOM](img/WISDOM_spectrogram_example.png)

On voit que la somme de sinusoïdes n'est pas tout à fait la même pour chaque sondage, signe que la pronfondeur des différentes interfaces détectées varie avec la distance horizontale.

Avant de réaliser les conversions en domaine temporel, posez-vous les questions suivantes :

_Quel temps de retard maximal pourra mesurer WISDOM ? Retrouvez-vous bien la distance ambiguë dans le vide donnée précédemment ?_

_Quel sera la résolution temporelle de WISDOM ? Retrouvez-vous bien la résolution dans le vide donnée précédemment ?_

_Que risque-t-il de se passer si WISDOM détecte un écho d'une interface plus lointaine que sa distance ambiguë ?_

### FFT réelle avec Numpy

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

## Interprétation géophysique

### Détection de pics

### Mesure de la permittivité de surface

### Estimation de la profondeur

### Coins de glace ?

## Exportation du résultat

## Conclusion

## BONUS

