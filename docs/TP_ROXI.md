# ROXI : radar météorologique pour l'étude de la pluie

![En-tête TP ROXI](img/Header_TP_ROXI.png)

_"Rob McKenna had 231 different types of rain entered in his little book, and he didn't like any of them. [...] Since he had left Denmark the previous afternoon, he had been through types 33 (light pricking drizzle which made the roads slippery), 39 (heavy spotting), 47 to 51 (vertical light drizzle through to sharply slanting light to moderate drizzle freshening), 87 and 88 (two finely distinguished varieties of vertical torrential downpour), 100 (post-downpour squalling, cold), all the seastorm types between 192 and 213 at once, 123, 124, 126, 127 (mild and intermediate cold gusting, regular and syncopated cab-drumming), 11 (breezy droplets), and now his least favourite of all, 17."_

**Douglas Adams, So Long and Thanks for All the Fish (1984)**

---

## Contexte scientifique

**ROXI** est un **radar météorologique** en développement au LATMOS depuis 2016.
Fonctionnant en bande X (à une fréquence de 9,42 GHz), il est conçu pour l'**étude des précipitations**.

![Illustration ROXI](img/ROXI_illustration.png)

ROXI est à **visée zénithale** : son antenne est orientée vers le zénith, dans le but d'acquérir des "**profils verticaux**" des propriétés des précipitations en fonction de l'élévation.
Ces profils sont acquis à intervalles de temps réguliers, afin de suivre l'évolution temporelle d'un évènement de précipitations.

Il s'agit d'un radar **impulsionnel** : il émet une impulsion électromagnétique vers le zénith, et réceptionne les échos provenant des gouttes de pluie ou des cristaux de glace rencontrés par l'impulsion.

* Le temps de retard entre l'émission de l'impulsion et la réception des échos permettra de déterminer leur **distance au radar** (correspondant ici à l'élévation).

* La durée de l'impulsion définira alors la **résolution verticale** de l'instrument.

* La puissance des échos reçus comparée à la puissance de l'impulsion émise permettra d'estimer la **réflectivité** des hydrométéores, une information utile pour déterminer la **phase** (liquide ou solide) et l'**intensité** (taux de pluie) des précipitations.

Il s'agit également d'un radar **Doppler** : en réalité il réalise des milliers d'émissions d'impulsion en quelques secondes, afin d'estimer la vitesse des cibles par **effet Doppler**.
En effet, la différence entre les signaux acquis pour une même élévation mais pour des émissions d'impulsion différentes, est liée au mouvement des hydrométéores, et permet donc de discriminer leurs différentes **vitesses de chute**. 
Cette information est liée au diamètre et à la phase des hydrométéores, permettant ainsi la caractérisation de la **microphysique** des précipitations.

* Le taux de répétition entre 2 émissions d'impulsion définira la **portée maximale** de l'instrument (l'élévation maximale mesurable).

* Le temps entre 2 émissions d'impulsions, le nombre d'impulsions d'affilée, ainsi que la longueur d'onde du radar définiront la résolution en vitesses et la **vitesse ambigue** (vitesse maximale mesurable) du radar.
Nous détaillerons comment dans la suite.

![Animation ROXI](img/ROXI_animation.gif)

Les données d'un profil de radar impulsionnel Doppler comme ROXI ressemblent donc à une matrice 2D :

![Données d'un radar Doppler](img/Doppler_radar_data.png)

* Chaque élément de la matrice est un **nombre complexe** I/Q (In-phase / Quadrature-phase) contenant les informations d'**amplitude** et de **phase** des échos reçus pour un temps de retard et une émission d'impulsion donnée.

* L'axe vertical correspond aux temps de retard des échos pour une émission d'impulsion donnée, aussi appelé "**fast-time**".

* L'axe horizotal correspond aux temps entre 2 répétitions d'impulsion, aussi appelé "**slow-time**".

Pour chaque niveau d'élévation, nous pouvons obtenir par analyse spectrale une présentation de la puissance reçue par le radar en fonction de la vitesse Doppler mesurée, appelée **spectre Doppler**.

Ces spectres peuvent ensuite être concaténés verticalement afin d'obtenir une représentation sous la forme d'une image appelée **spectrogramme Doppler**.

![Analyse spectrale Doppler](img/Doppler_spectral_analysis.png)

On peut voir chaque spectre Doppler comme la distribution des puissances reçues par le radar par rapport à la vitesse des hydrométéores au sein d'un volume sondé.
Il est courant d'essayer de modéliser cette distribution par un modèle Gaussien, afin d'en tirer 2 caractéristiques des précipitations : leur réfléctivité moyenne **Z**, et leur vitesse moyenne **VDop**.

De ces caractéristiques pourrons être inférées des **grandeurs météorologiques** d'intérêt : le régime de précipitation, sa phase, son intensité, etc.

Voici les caractéristiques du radar ROXI :

|Caractéristique                  |Valeur   |
|:-------------------------------:|:-------:|
|Fréquence d'émission             |9.42 GHz |
|Puissance d'émission             |70 W     |
|Gain de l'antenne                |41 dBi   |
|Durée de l'impulsion             |666.67 ns|
|Période de répétition d'impulsion|682.67 µs|
|Nombre d'impulsions par profil   |4096     |
|Distance ambiguë                 |12.8 km  |
|Vitesse ambiguë                  |11.66 m/s|

## Objectifs

Lors de ce tutoriel, nous allons programmer une **chaîne de traitement des données de ROXI** sous la forme d'un **projet Python**, que nous utiliserons pour obtenir une **interprétation météorologique** classique.

Ce projet Python devra contenir des fonctions pour :

* Importer des données ROXI à partir d'un fichier HDF5 tel que celui qui vous sera fourni.

* Convertir en spectres Doppler les données I+Q acquises par le radar.

* Réaliser une intégration incohérente des spectres obtenus pour réduire le bruit.

* Appliquer un filtre "anti-clutter" aux spectres intégrés.

* Ajuster un modèle Gaussien aux spectres Doppler pour en déduire Z et VDop.

* Exporter les estimations de Z et VDop obtenues sous la forme d'un fichier HDF5.

Créez le dossier de votre projet, et structurez-le avec des sous-dossiers et des fichiers vides pour l'instant.

**Pour faire simple, n'ajouterons pas de tests ou de documentation à notre projet Python.**

|Nota Bene|
|:-|
|Il est à noter que notre exemple ici a été grandement simplifié pour les besoins de ce TP.|
|En particulier, l'estimation de Z, qui nécessite normalement une réelle calibration du radar pour compenser tous les effets instrumentaux.|

## Importation des données

### Le format HDF5

Lors de ce TP, nous partirons du principe que les données ROXI sont mises sous la forme de fichiers **HDF5**.

Le "Hierarchical Data Format" est un format de fichiers utilisé pour **stocker de grandes quantités d'informations** de manière organisée et accessible efficacement.
C'est pourquoi il est très classique en analyse de données.

Comme son nom l'indique, les données sont stockées hiérarchiquement, à la manière de l'arborescence d'un dossier sur votre ordinateur.
L'équivalent d'un dossier dans un fichier HDF5 est ce que l'on appelle un "**Group**".
On peut alors ranger les données dans des Groups, voir même dans des sous-Groups d'un Group.

Les données stockées sont enregistrées sous la forme de tableaux, que l'on appelle des "**Datasets**".

On peut également stocker des métadonnées, sous la forme "d'**Attributes**".

Nous verrons comment lire un fichier HDF5 avec Python, puis comment exporter des données dans ce format, grâce à la bibliothèque `h5py`.

### Exemple de données ROXI

Vous trouverez un exemple de fichier de **données ROXI** au format **HDF5** [ici](https://github.com/NicOudart/UVSQ_M2_NewSpace_TP_signal/blob/master/example/ROXI_20200811_161206.h5).

On considérera qu'un fichier de données ROXI contient toujours les 3 "Datasets" suivants, directement à la racine :

* `I+Q` : une matrice de dimensions 4x128x4096, correspondant à **4 profils** acquis d'affilée, contenant chacun **128 échantillons fast-time** pour **4096 échantillons slow-time**.
Chaque élément de la matrice est donc un nombre complexe I/Q mesuré par le radar (homogène à des volts).
Ces 4 profils étant acquis sur un lapse de temps très court, on peut considérer qu'il s'agit de 4 répétitions d'un même profil.

* `fast_time_axis` : un vecteur de dimension 128, contenant les temps de retard des échos (s), correspondants aux **128 échantillons fast-time**.

* `slow_time_axis` : un vecteur de dimension 4096, contenant les temps d'émission des impulsions (s), correspondants aux **4096 échantillons slow-time**.

Ces données ont été acquises le **11/08/2020** à **16:12:06 UTC** sur le site de l'**Observatoire de Versailles Saint-Quentin** (OVSQ), à Guyancourt.

Il s'agit d'une observation d'un **orage multicellulaire** survenu à la fin de la canicule d'août 2020.
Une vague chaleur atteignant les 38°C, ainsi qu'une forte humidité dans les basses couches atmosphériques ont favorisé la formation de cet évènement convectif, qui a duré de 12:30 à 18:00 UTC, avec des hydrométéores détectés jusqu'à 12 km d'élévation.
Si son intensité est restée relativement modérée au-dessus de Guyancourt, de violentes averses ont été enregistrées localement en région parisienne, notamment en Essonne.

Voici une animation Météo-France de l'évènement, avec les taux de précipitations estimés en mm/h :

![Animation orage Météo-France](img/Thunderstorm_meteofrance_animation.gif)

Ce fichier nous servira d'exemple pour construire notre **chaîne de traitement des données ROXI**.

|Nota Bene|
|:-|
|En réalité, les données ROXI n'étant pas très lourdes, elles sont enregistrées dans des fichiers binaires qui ne sont pas au format HDF5.|
|Un fichier ROXI contient également des métadonnées qui ne vous sont pas fournies ici.|
|Pour les besoins de ce TP, nous avons donc fait en sorte de vous faire découvrir un format utile, et simplifié le contenu des données ROXI.|

### Lecture du fichier

La 1ère étape de notre de chaîne traitement sera d'importer les données ROXI d'un fichier HDF5.

|Ajoutez à votre projet Python une fonction `read`|
|:-|
|Cette section vous donnera les éléments nécessaires pour la compléter.|
|- Elle prendra en entrée le chemin d'un fichier HDF5.|
|- Elle retournera 3 matrices `numpy`, correspondants aux 3 "Datasets" contenus dans le fichier.|

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
hf = h5py.File(".../ROXI_20200811_161206.h5",'r')
~~~

On peut alors récupérer les différents "Datasets" contenus dans le fichier avec la méthode `get` et leurs noms.
On oubliera pas de convertir en matrices `numpy` les "Datasets" récupérés.

Voici ce que pourrait donner la récupération des 3 "Datasets" d'un fichier ROXI :

~~~
mat_iq = np.array(hf.get('I+Q'))
fast_time_axis = np.array(hf.get('fast_time_axis'))
slow_time_axis = np.array(hf.get('slow_time_axis'))
~~~

**Vous pouvez à présent compléter votre fonction `read`**.

Appliquez votre fonction à notre fichier exemple.

_Les dimensions des matrices récupérées sont-elles bien celles attendues ? Le type des données également ?_

## Analyse spectrale

La 2nde étape de notre de chaîne traitement sera de générer un spectre à partir des données ROXI récupérées.

|Ajoutez à votre projet Python une fonction `FFT`|
|:-|
|Cette section vous donnera les éléments nécessaires pour la compléter.|
|- Elle prendra en entrée 3 matrices `numpy` telles que retournées par la fonction `read`.|
|- Elle retournera 2 matrices `numpy` : une contenant les 4 spectrogrammes Doppler correspondant aux 4 profils, et l'autre l'axe des fréquences correspondant à ces spectrogrammes.|

### Nature des données

Dans un 1er temps, nous allons nous intéresser à la nature des données dont nous disposons.

Importez la bibliothèque `matplotlib`, qui va nous servir à faire des affichages graphiques :

~~~
import matplotlib.pyplot as plt
~~~

Essayons tout d'abord d'afficher une série temporelle "slow-time" issue de nos données.

Prenons le 1er profil, et regardons la série temporelle "slow-time" pour le niveau du 11ème échantillon "fast-time".
Affichons les parties réelle et imaginaire du signal avec des commandes Python de ce type :

~~~
plt.figure()
plt.plot(slow_time_axis,np.real(mat_iq[0][10]),'r-')
plt.plot(slow_time_axis,np.imag(mat_iq[0][10]),'g-')
plt.xlabel('Slow-time (s)')
plt.ylabel('Unicalibrated voltage (V)')
plt.title('I+Q - acquisition 0 - fast-time level 10')
plt.grid()
plt.show()
~~~

On obtient alors la courbe suivante :

![Exemple de données I+Q](img/ROXI_I+Q_example.png)

Si on fait un zoom sur une partie de la courbe, on voit qu'il s'agit d'un **signal périodique** :

![Zoom sur les données I+Q](img/ROXI_I+Q_example_zoomed.png)

Ces oscillations correspondent aux décalages de fréquence par effet Doppler, liés aux mouvements verticaux des hydrométéores dans le volume sondé (au niveau du 11ème échantillon "fast-time").
Tous les hydrométéores n'ayant pas la même vitesse verticale, et la même réfléctivé radar, ce signal est une **somme de sinusoïdes complexes**, dont la fréquence est d'autant plus grande que la vitesse est élevée.

D'où l'intérêt de vouloir réaliser une **analyse spectrale** de ce signal, afin de séparer les différents décalages de fréquence Doppler, et la puissance associée à chacun.

Avant de réaliser cette analyse, posez-vous les questions suivantes :

* _Quelle fréquence Doppler maximale peut mesurer ROXI ?_

* _Quelle est la résolution en fréquence de ROXI ?_

* _Retrouvez-vous bien la durée de d'une impulsion et la période de répétition d'impulsion de ROXI ?_

Répondez-y en vous appuyant sur les axes "fast-time" et "slow-time" que vous avez récupérés.

### FFT avec Numpy

Pour réaliser l'analyse spectrale des séries temporelles "slow-time" de ROXI, nous allons utiliser la "**Fast Fourier Transform**".
Il s'agit d'un algorithme calculant la transformée de Fourier discrète de manière significativement plus rapide que la méthode naïve.

La bibliothèque `numpy` contient une implémentation de la FFT.

N'oubliez donc pas d'importer cette bibliothèque :

~~~
import numpy as np
~~~

Pour déterminer l'axe des fréquences qui ira avec nos spectres Doppler, on peut utiliser la méthode `fft.fftfreq` de `numpy`.
Cette méthode prend en entrée la longueur de l'axe temporel du signal à analyser, et le pas temporel correspondant.

On pourra donc utiliser des commandes Python de ce genre :

~~~
slow_time_dt = slow_time_axis[1]-slow_time_axis[0]
frequency_axis = np.fft.fftfreq(len(slow_time_axis),d=slow_time_dt)
~~~

La sortie d'une transformée de Fourier est une série de **nombres complexes**, contenant les informations d'**amplitude** et de **phase** du spectre.

L'information de phase n'est généralement pas exploitée, car elle ne contient pas d'information physique intéressante.
L'amplitude en revanche permet d'estimer la puissance reçue pour chaque décalage de fréquence Doppler.

L'amplitude du spectre en sortie de la FFT sera homogène à une tension.
Pour obtenir une grandeur homogène à une **puissance**, nous prendrons son module, et nous le mettrons au carré.

On utilisera la méthode `fft.fft` de `numpy`, avec une commande similaire à celle-ci :

~~~
spectrum_0_10 = np.abs(np.fft.fft(mat_iq[0][10]))**2
~~~

|Nota Bene|
|:-|
|Les puissances obtenues ici ne sont pas calibrées.|
|En effet, les tensions ne sont pas calibrées, et nous n'avons pas divisé par une impédance.|
|Il y a donc un offset avec la vraie puissance reçue pour chaque fréquence en dB.|

Essayez sur le 11ème échantillon "fast-time" du 1er profil.

_Observez-vous des composantes fréquentielles ressortir ?_
_Dans quelle ordre méthode `fft.fft` sort-elle les fréquences du spectre ?_

### FFTshift et dB

Vous l'avez compris, l'implémentation `numpy` de la FFT sort les fréquences dans un ordre un peu contre-intuitif.
La raison de cet ordre est purement algorithmique.

Pour **ordonner** les sorties de la fréquence négative la plus faible à la fréquence positive la plus élevée, avec le 0 au milieu, on pourra utiliser la méthode `fft.fftshift` de `numpy`.

Voici un exemple de commandes pour l'axe fréquentiel et le spectre renvoyé par la FFT :

~~~
spectrum_0_10 = np.fft.fftshift(spectrum_0_10)
frequency_axis = np.fft.fftshift(frequency_axis)
~~~

_D'un point de vue physique, que signifie avoir un décalage de fréquence Doppler positif ou négatif ?_

Les puissances reçues par un radar météorologique peuvent varier sur 6 ou 7 ordres de grandeur.
C'est pourquoi on converti en général les spectres Doppler en **décibels**.

Ceci peut facilement être fait avec une commande Python de ce genre : 

~~~
spectrum_0_10_db = 10*np.log10(spectrum_0_10)
~~~

Pour voir à quoi ressemble un spectre Doppler ROXI après ces 2 traitements, vous pouvez utiliser des commandes Python similaires à celles-ci :

~~~
plt.figure()
plt.plot(frequency_axis,spectrum_0_10_db,'r-')
plt.xlabel('Doppler frequency (Hz)')
plt.ylabel('Uncalibrated power (dB)')
plt.title('Doppler spectrum - acquisition 0 - fast-time level 10')
plt.grid()
plt.show()
~~~

Voici à quoi ressemble le spectre pour le 11ème échantillon "fast-time" du 1er profil :

![Exemple de spectre Doppler brut](img/ROXI_spectrum_raw_example.png)

On observe une distribution quasi-gaussienne, avec un pic aux alentours de 400 Hz de décalage Doppler.
Cette forme est assez classique pour un signal météorologique.
Un **modèle gaussien** sera d'ailleurs souvent utilisé lors de l'interprétation des spectres Doppler d'un radar météorologique.

**Vous pouvez à présent compléter votre fonction `FFT`**.

Appliquez votre fonction à notre exemple.
Regardez différents spectres pour différents profils et différents échantillons "fast-time".

_Le spectre est-il toujours gaussien ?_
_La fréquence du pic est-elle toujours la même ?_
_Voyez-vous des pics ayant l'air d'avoir une autre origine que les précipitations ?_

## Réduction du bruit

Vous l'avez sûrement remarqué, les spectres Doppler ROXI sont bruités.

Extraire le signal utile du bruit est un des grands enjeux du traitement du signal.
Dans le cas de ROXI, nous pouvons tirer profit du fait que nous disposons de 4 répétitions du même profil, pour diminuer le bruit en les intégrant.

L'étape suivante de notre chaîne de traitement sera donc logiquement d'implémenter une "**intégration incohérente**" (définirons ce terme dans la suite) pour réduire le bruit dans les spectres Doppler.

|Ajoutez à votre projet Python une fonction `integrate`|
|:-|
|Cette section vous donnera les éléments nécessaires pour la compléter.|
|- Elle prendra en entrée une matrice `numpy` contenant 4 spectrogrammes tels que retournée par `FFT`.|
|- Elle retournera une matrices `numpy` : une contenant le spectrogramme intégré à partir des 4 profils.|

### Intégration cohérente

Un **bruit blanc** dans une série temporelle donnera un spectre plat.
Le spectre d'un signal bruité contiendra donc un offset, dont le niveau correspondra au **niveau du bruit**.

Si on trace un spectre en décibels, la différence entre le pic du signal utile et le niveau du bruit donnera le ratio signal/bruit ou **SNR**.

On comprend intuitivement que plus le pic dépasse du niveau de bruit (donc plus le SNR est élevé), meilleure sera la détection puis la caractérisation du signal utile.

Mettons que l'on dispose de plusieurs répétitions d'une même série temporelle, et que l'on fait l'hypothèse que la **phase du signal** reste **cohérente** entre 2 répétitions, et que le **bruit** est **aléatoire**.
**Moyenner** les différentes répétitions permettra d'obtenir une série temporelle de **SNR plus élevé**, et donc un spectre au **niveau de bruit plus bas**.

Cette méthode reposant sur l'hypothèse de cohérence de la phase du signal entre 2 répétitions, on l'appelle "**intégration cohérente**".

Nous ne l'implémenterons pas ici, mais elle est faite par ROXI de manière cachée : en réalité, un profil ROXI est issu de **8 intégrations cohérentes**.
Ce qui explique pourquoi la période de répétition d'impulsion de ROXI n'est pas égale à 128 fois la durée de son impulsion, mais 128x8.

|Nota Bene|
|:-|
|Par linéairité de la transformée de Fourier, il est équivalent d'appliquer l'intégration à la série temporelle ou à son spectre.|
|On pourrait donc tout aussi bien l'appliquer avant FFT.|

Bien entendu, l'intégration cohérente à des limites : **plus on intègre longtemps**, plus les cibles auront bougé, et donc **moins l'hypothèse de cohérence de la phase sera vérifiée**.

_Quel est environ le niveau du bruit pour les différents spectres obtenus ? Est-il le même pour tous les spectres ? Comment expliquez-vous ceci ?_

_Quel est environ le SNR pour le spectre obtenu plus tôt avec le 11ème échantillon "fast-time" du 1er profil ?_

### Intégration incohérente

Nous implémenterons ici l'**intégration incohérente** (ou "non-cohérente").

Par opposition avec l'intégration cohérente, l'intégration incohérente **moyenne les puissances des spectres** obtenus après FFT et module au carré pour les différentes répétitions d'une même série temporelle, **en excluant l'information de phase**.

Le résultat est que le niveau de bruit du spectre ne varie pas, mais le spectre obtenu est **plus "lisse"**.

|Nota Bene|
|:-|
|L'opération "module" n'étant pas linéaire, contrairement à l'intégration cohérente, l'intégration incohérente doit être effectuée après FFT.|

Tout comme l'intégration cohérente, l'intégration incohérente a des limites : la forme des spectres évolue au fur et à mesure que les cibles se déplacent ou changent, et au bout d'un certain temps intégrer les spectres n'a plus aucun sens.
En météorologie radar, on ne dépasse généralement pas quelques secondes.

**Vous pouvez à présent compléter votre fonction `integrate`** : elle fera la moyenne des 4 spectrogrammes en puissances obtenus à partir des 4 profils contenus dans un fichier de données ROXI.

Si vous appliquez votre fonction à notre exemple, et que vous affichez le spectre obtenu pour le 11ème échantillon "fast-time", vous devriez obtenir ceci :

![Exemple de spectre Doppler intégré](img/ROXI_spectrum_integrated_example.png)

_Observez-vous bien le résultat attendu ? D'après-vous, quel sera l'intérêt de lisser ainsi les spectres Doppler de ROXI ?_

## Filtrage

### Ground clutter

![Exemple de spectre avant filtrage du clutter](img/ROXI_spectrum_before_filtering_example.png)

### Filtre médian

~~~
from scipy.signal import medfilt
~~~

~~~
spectrum_0_2_db = medfilt(spectrum_0_2_db,kernel_size=7)
~~~

![Exemple de spectre après filtrage du clutter](img/ROXI_spectrum_after_filtering_example.png)

## Spectrogramme

~~~
len_fast_time = len(fast_time_axis)
len_slow_time = len(slow_time_axis)
mat_spectrum = np.zeros((len_fast_time,len_slow_time))
~~~

~~~
plt.figure()
plt.pcolormesh(frequency_axis,fast_time_axis,mat_spectrum)
plt.xlabel('Doppler frequency (Hz)')
plt.ylabel('Fast-time (s)')
plt.colorbar(label='Uncalibrated power (dB)')
plt.title('Doppler spectrogram - 4 integrated acquisitions - 2020/08/11 16:12:06 UTC')
plt.show()
~~~

![Exemple de spectrogramme Doppler](img/ROXI_spectrogram_example.png)

## Interprétation météorologique

### Conversion en vitesses et élévations

![Exemple de spectrogramme Doppler avec des axes météorologiques](img/ROXI_spectrogram_meteo_axis_example.png)

### Modélisation Gaussienne

~~~
from scipy.optimize import curve_fit
~~~

~~~
def gaussian_model(x,ampli,vdop,std):
	
    signal = ampli*np.exp(-(x-vdop)**2/(2*std**2))/(std*np.sqrt(2*np.pi))
	
    return 10*np.log10(signal+10**(-57/10))
~~~

~~~
params,_ = curve_fit(gaussian_model,vdop_axis,mat_spectrum_db[10],p0=[1e3,0,1],bounds=([0,-12,0],[1e6,12,3]))
ampli,vdop,std = params
~~~

~~~
plt.figure()
plt.plot(vdop_axis,mat_spectrum_db[10],'r-')
plt.plot(vdop_axis,gaussian_model(vdop_axis,ampli,vdop,std),'b--')
plt.xlabel('Doppler velocity (m/s)')
plt.ylabel('Uncalibrated power (dB)')
plt.title('Doppler spectrum modelling - 4 integrated acquisitions - fast-time level 10')
plt.legend(['observation','model'])
plt.grid()
plt.show()
~~~

![Exemple de modélisation Gaussienne de spectre Doppler](img/ROXI_spectrum_modelling_example.png)

### Estimation de Z et VDop

![Exemple d'estimations de VDop](img/ROXI_VDop_estimations_example.png)

![Estimations de VDop pour l'orage complet](img/ROXI_VDop_thunderstorm_example.png)

![Exemple d'estimations de Z](img/ROXI_Z_estimations_example.png)

![Estimations de Z pour l'orage complet](img/ROXI_Z_thunderstorm_example.png)

### Exportation du résultat

