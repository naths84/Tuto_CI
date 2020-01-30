[Ébauche de tutoriel]

# Création de vidéos pour la classe inversée sur Ubuntu
Voici les étapes que nous allons suivre:
- Créer l'exposé avec les animations sur LibreOffice. 
- Enregistrer la bande-son de l'exposé.
- Utiliser une extension dans LibreOffice pour générer un pdf.
- Convertir chaque page du PDF en une image JPEG ou PNG. 
- Monter la vidéo dans Flowblade.

### Création de l'exposé.
Supposé fait.

### Bande son

Utiliser une application sur son smartphone ou bien sur l'ordinateur pour enregistrer le son de la présentation. Pour le montage et corriger les erreurs, il est plus facile d'enregistrer un fichier par son par slide. Transférer ensuite les fichiers sur son ordinateur afin de les avoir à disposition pour le montage de la vidéo.

### ODP ou PPT vers PDF avec le contenu en fonction des animations. 
LibreOffice ne permet pas d'exporter un PDF qui affiche le contenu en fonction de l'ordre des animations. La seule option disponible est l'exportation du PDF dans lequel chaque page représente une slide de l'exposé avec tout son contenu. Il faut installer l'extension [ExpandAnimations](https://www.monperrus.net/martin/export+animations+to+pdf+in+libreoffice-openoffice) dans LibreOffice (c.f. [comment installer une extension](https://wiki.documentfoundation.org/Documentation/HowTo/install_extension)) pour remédier à cela. Une fois l'extension installée, aller sur l'onglet Outils->Extensions (ou Add-Ons) et cliquer sur Expand Animations pour créer le PDF.

#### Une image par page.
Afin de générer une image par page, il faut ouvrir un terminal dans le dossier dans lequel se trouve le PDF et exécuter la commande suivante. Elle exécute le package pdftoppm disponible sur Ubunut.

```sh
$ pdftoppm nomDuPDF.pdf prefixe_image -r 300 -png
```
Les images apparaissent toutes au chemin spécifié lors de l'exécution de la commande. 

Par exemple si le PDF s'appelle presentation.pdf et que l'on souhaite générer les images dans le même dossier avec un préfixe imagePresentation on exécute la commande:
```sh
$ pdftoppm presentation.pdf imagePresentation -r 300 -png
```
Les images générées s'appelleront 

imagePresentation1.png
imagePresentation2.png
imagePresentation3.png
...
#### Monter la vidéo dans Flowblade.

Dans Flowblade créer un nouveau projet avec comme profil HD 720 24 fps, une bande-son et une bande vidéo. Importer les images générées au point précédent dans Flowblade (la fonction drag and drop fonctionne) et les fichiers sons. Ajouter les fichiers son à la suite dans la piste son et ensuite les images en les synchronisant avec la bande son. Enregistrer un backup snapshot du projet Fichier->Save Backup Snapshot. Puis finalement aller à l'onglet rendu et générer le rendu du projet. 

#### Conseil final.
La vidéo peut être assez volumineuse, il suffit de l'importer dans YouTube en vidéo non répertoriée et de la télécharger depuis YouTube pour obtenir une vidéo très légère dont l'encodage fonctionne bien avec Moodle. 

#### Avantage comparé à une autre méthode
Lorsqu'une erreur se glisse sur un slide ou dans la bande-son, la correction est très aisée.
