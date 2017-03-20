# Résumé de la macro structures-AureHAL
Macro VBA pour requêter toutes les structures parentes ou filles d'une structure initiale dans AureHAL, et visualiser leur arborescence dans draw.io.

![structures hal navier](https://cloud.githubusercontent.com/assets/26523540/24092867/51ba6d9e-0d51-11e7-8666-962e52b44591.png)

À partir de l'identifiant AuréHAL d'une ou plusieurs structures (docid), la macro va requêter dans l'API leurs métadonnées, puis rechercher leurs structures mères **ou** filles, requêter leurs métadonnées, et ainsi de suite. La macro pacourt donc toute l'arborescence ascendante ou descendante des structures initiales.

La macro permet ensuite de visualiser cette arborescence dans l'outil en ligne **draw.io** (https://www.draw.io/), en créant un fichier CSV à importer dans l'outil. La visualisation peut ensuite être sauvegardée sur un compte Google Drive et être exportée sous différents formats d'image.

Les métadonnées requêtées pour chaque structure sont :
- ID
- Acronyme
- Label
- Validité (VALID, OLD, INCOMING)
- Type (institution, department, laboratory, researchteam...)
- Adresse
- ID des structures mères
- URL
- ID des structures filles
- Nombre de dépôts affiliés

# Description des fichiers
Le script est constitué de 2 fichiers :
- Structures_AureHAL.xlsm
- Parametres_drawio.txt

Le fichier **Structures_AureHAL.xlsm** contient 2 macros à lancer successivement :
- Structures_AureHAL : requête les métadonnées des structures initiales et celles des structures ascendantes ou descendantes.
- cleanDrawio : crée le fichier CSV à importer dans draw.io pour visualiser l'arborescence.

Le fichier **Parametres_drawio.txt** contient les paramètres draw.io utilisés par la macro cleanDrawio. Il doit se trouver dans le même répertoire.

# Disclaimer
La macro a été développée sur Excel 2013 par un documentaliste. Il n'y a pas de garantie qu'elle fonctionne et les utilisateurs sont invités à se plonger dans le code.

Si le portail HAL / AuréHAL rame ou est down, la macro ne pourra pas fonctionner.

**Q.** Pourquoi la macro ne requête pas à la fois les structures ascendantes et descendantes de la structure de départ ?

**R.** Pour chaque structure trouvée, la macro chercherait à la fois les mères et les filles, et ainsi de suite, jusqu'à parcourir toute l'arborescence d'AuréHAL. AuréHAL contient environ 500.000 structures ; beaucoup sont isolées, mais la macro plante au bout de quelques milliers de lignes.

L'arborescence descendante d'une institution ne dépasse pas quelques centaines de structures en général, l'arborescence montante d'une équipe de recherche bien moins.

# Utilisation de la macro
## Requêter les métadonnées, créer l'arborescence
Ouvrir le fichier Structures_AureHAL.xlsm

Afficher l'onglet Développeur dans le ruban : Fichier --> Options --> Personnaliser le ruban

Activer Microsoft XML, V6.0 si ce n'est pas activé automatiquement : Onglet Développeur --> VBA --> Outils --> Références --> Cocher Microsoft XML, V6.0

Activer Microsoft Forms 2.0 Object Library si ce n'est pas activé automatiquement : Onglet Développeur --> VBA --> Outils --> Références --> Cocher Microsoft Forms 2.0 Object Library

Rentrer l'ID d'une structure AuréHAL (docid) dans la case A2, ou plusieurs ID en A2, A3, A4 etc.

Lancer la première macro : Onglet Développeur --> Macros (ou Alt-F8) --> Sélectionner **Structures_AureHAL.xlsm!Structures_AureHAL.Structures_AureHAL** --> Exécuter.

Une boite de dialogue s'ouvre. Indiquer le sens de la recherche.
- Chercher les **structures parentes**, par exemple en partant d'une équipe de recherche, pour récupérer tous les laboratoires de tutelle, et toutes les institutions tutelles des laboratoires (arborescence ascendante).
- Chercher les **structures filles** (par défaut), par exemple en partant d'une institution, pour récupérer tous les laboratoires dont elle est tutelle, et toutes leurs équipes de recherche.

Indiquer la **ligne de démarrage** (par défaut 2). C'est utile si la macro a été lancée une fois, et qu'on rajoute à la fin de la liste générée, des ID supplémentaires pour créer une seconde arborescence dans le même fichier.

## Visualiser l'arborescence : créer le fichier CSV et l'importer dans draw.io
Une fois la macro Structures_AureHAL terminée d'exécuter, lancer la seconde macro :
Onglet Développeur --> Macros (ou Alt-F8) --> Sélectionner **Structures_AureHAL.xlsm!cleanDrawio.cleanDrawio** --> Exécuter.

Une fois la macro cleanDrawio terminée d'exécuter, ouvrir le répertoire du fichier des macros. Un nouveau fichier **Import_Drawio.csv** est apparu.
- Ouvrir le fichier Import_Drawio.csv dans un éditeur de code comme Notepad++
- Copier tout son contenu
- Aller sur le site https://www.draw.io/
- Créer un nouveau diagram
- Importer le contenu du fichier Import_Drawio.csv : File --> Import from --> CSV --> Supprimer le code d'exemple existant et coller tout le contenu du fichier Import_Drawio.csv --> Import
