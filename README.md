# OCR

## Liens

Fiche projet: https://docs.google.com/document/d/1oDYKI5eYqDuzuFkTAhs52ecKRDW1HwlQV81ZFmGV1_I

Dataset: https://fki.tic.heia-fr.ch/databases/download-the-iam-handwriting-database

# Première étape

* Quelle direction prendre ?
    * La création d'un outil OCR : c'est essentiellement ce qui est proposé dans la fiche de projet initiale. C'est très intéressant techniquement, mais, en pratique, il sera difficile d'égaler ce qui est déjà disponible : il y'a peut être de quoi faire sur l'écriture manuscrite.
    * L'utilisation d'outil OCR déjà défini pour ensuite par exemple classifier des documents ou extraire de l'information (détail d'un facture, référence...). Ce qui correspondra plutôt à du NLP.
* Quel est votre problématique ?
* Quel est votre objectif (une première idée de seuil de performance, métrique...), une idée de la méthodologie ?
* Donnez un nom plutot créatif à votre projet (contenant ‘Py’ en référence à Python par exemple) !  Une fois fait, je pourrai instancier un repo Github qui vous sera utile pour les dernières étapes du projet .
* La tâche est relativement simple, vous devrez fixer votre jeu de données et faire une analyse de celles-ci.
* Faire des visualisations pertinentes expliquant la structure / difficultés / biais du dataset.
* Ce rapport doit être court et tenir sur 3-4 pages maximum.




## Data

- data/ascii.tgz - Contains summarized meta information in ascii format.
- data/formsA-D.tgz data/formsE-H.tgz data/formsI-Z.tgz - Contains form images (example: a01-122.png).
- data/lines.tgz - Contains text lines (example: a01/a01-122/a01-122-02.png).
- data/sentences.tgz - Contains sentences, one for each line (example: a01/a01-122/a01-122-s01-02.png).
- data/words.tgz - Contains words (example: a01/a01-122/a01-122-s01-02.png).
- data/xml.tgz - Contains the meta-infornation in XML format (example: a01-122.xml).




### data/ascii.tgz

Resumé de chaque dataset avec un header en commentaire ("#")

### data/forms

pages de texte scannées

<img src="data/formsE-H/e01-014.png"/>

```
 format: a01-000u 000 2 prt 7 5 52 36

     a01-000u  -> form id
     000       -> writer id
     2         -> number of sentences
     prt       -> word segmentation
                     prt: some lines correctly segmented
                     all: all lines correctly segmented
     7 5       -> 5 of 7 lines are correctly segmented into words
     52 36     -> the form contains 52 words, 36 are in lines which
                  have been correctly segmented

```

### data/lines

lignes de texte isolées et labellisées

<img src="data/lines/a01/a01-000u/a01-000u-00.png"/>

```
format: a01-000u-00 ok 154 19 408 746 1663 91 A|MOVE|to|stop|Mr.|Gaitskell|from

     a01-000u-00  -> line id for form a01-000u
     ok              -> result of word segmentation
                            ok: line is correctly segmented
                            err: segmentation of line has one or more errors

                        notice: if the line could not be properly segmented
                                the transcription and extraction of the whole
                                line should not be affected negatively

     154             -> graylevel to binarize line
     19              -> number of components for this line
     408 746 1663 91 -> bounding box around this line in x,y,w,h format

     A|MOVE|to|stop|Mr.|Gaitskell|from
                     -> transcription for this line. word tokens are separated
                        by the character |
```

### data/sentences

phrases isolées et labellisées

<img src="data/sentences/a01/a01-000u/a01-000u-s00-00.png"/>

```
# format: a01-000u-s0-00 0 ok 154 19 408 746 1663 91 A|MOVE|to|stop|Mr.|Gaitskell|from
#
#     a01-000u-s0-00  -> sentence/line id for form a01-000u
#     0               -> sentence number within this form
#     ok              -> result of word segmentation
#                            ok: line is correctly segmented
#                            er: segmentation of line has one or more errors
#
#                        warning: if a sentence starts or ends in the middle of
#                                 a line which is not correctly segmeted, a
#                                 correct extraction of the sentence can fail.
#
#     154             -> graylevel to binarize line
#     19              -> number of components for this part of the sentence
#     408 746 1663 91 -> bounding box around for this part of the sentence
#                        in the x,y,w,h format
#
#     A|MOVE|to|stop|Mr.|Gaitskell|from
#                     -> transcription for this part of the sentence. word
#                        tokens are separated by the character |
```

### data/words

mots isolés et labelisés

<img src="data/words/a01/a01-000u/a01-000u-00-00.png"/>

```
# format: a01-000u-00-00 ok 154 1 408 768 27 51 AT A
#
#     a01-000u-00-00  -> word id for line 00 in form a01-000u
#     ok              -> result of word segmentation
#                            ok: word was correctly
#                            er: segmentation of word can be bad
#
#     154             -> graylevel to binarize the line containing this word
#     1               -> number of components for this word
#     408 768 27 51   -> bounding box around this word in x,y,w,h format
#     AT              -> the grammatical tag for this word, see the
#                        file tagset.txt for an explanation
#     A               -> the transcription for this word
#
```