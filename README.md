Installation des outils
# Installation de l'outil SpliceLauncher
# C'est un outil qui permet d'étudier l'épissage alternatif dans les données RNA-Seq.
# Avant l'installation de l'outil, il demande des prérequis c'est à dire l'installation des outils de dépendances ( STAR,samtools,bedtools,R et ses packages( WriteXLS et Cairo)).
# Pour installer ces outils, nous avons effectué un environnement conda (miniconda) pour faciliter l'installation de ces derniers :

# installation miniconda sous linux: 
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh

# Creation d'un environnement virtual Conda de nom transcriptomique
conda create --name transcriptomique python = 3.9

# Activation de l'environnement transcriptomique
conda activate transcriptomique 

# Ajout de canal Bioconda et forge
conda config --add channels bioconda # Ajout du canal bio
conda config --add channels conda-forge

# Installation des outils de dependance via conda ( STAR,samtools,bedtools,R et ses packages( WriteXLS et Cairo))

1 - # outil STAR 
conda install -c bioconda star= 2.7.0c # Pour l'installation
STAR --version # Pour verifier l'installation

2- # outil samtools
conda install samtools=1.10
samtools --version

3- # outil beedtools 
conda install -c bioconda bedtools=2.25.0 # Pour l'installation 
bedtools --version # Pour verifier l'installation

4 - # outil R 
conda install -c conda-forge r # Pour l'installation
R --version # Pour verifier l'installation
# installation des packages
R+ entré # Pour ouvrir R d'abord
# Se servir de la commande install. packages("WriteXLS") et install. packages("Cairo")   Nb: il se peut qu'il demande de choisir le CRAN miror, il faut choisir celui le plus proche de votre zone geographique.
q() # Pour fermer R

# Maintenant l'installation de l'outil splicelauncher 
# On commence par cloné le GitHub de l'outil
git clone https://github.com/LBGC-CFB/SpliceLauncher # cloné le github
cd ./SpliceLauncher # Se déplacer dans le repertoire cloné

# l'outil SpliceLauncher et ses dépendances sont intégrés dans un image singularity (récemment appelé apptainer)
sudo apt install singularity # Pour installer l'outil de conteneurisation
sudo singularity build /path/to/SpliceLauncher.simg /path/to/splicelauncher.recipe # Pour intégrer tout les dépendances et SpliceLauncher dans une image singularity

# Bug et debug
# Avec cette commande j'avais des problèmes avec des bugs et l'erreur était une connextion de faible debit. Je l'ai su par la commande:
ping google.com # Pour verifier le débit de la connexion
# Donc il est recommander d'effectuer cette opération de construction d'image de singularity avec un bon débit connexion 

# Utilisation de l'image
sudo singularity run /path/to/SpliceLauncher.simg --config /path/to/my_config.cfg --help # Pour utiliser l'image



# il reste à faire le Run de SpliceLauncher sur des données et cela je pourrait pas le faire avec mon ordinateur personnelle car les données sont lourdes et elle n'a pas la capacité. 





# Installation de l'outils regtools pour pouvoir comparer avec vos résultats obtenus par RSeQC
# l'outils regtools permet: 
# D'identifier les preuves d’épissage aberrant dans les lectures d’ARN à proximité d’une liste de variants.
# Extrayer les jonctions exon-exon d’un fichier BAM RNAseq.
# Annoter les jonctions exon-exon avec des informations provenant d’un transcriptome connu.
# Annoter les variantes avec des annotations splice-region (la définition de cette région est configurable).

# Pour l'intallation c'est pas compliqué l'outil ne demande pas des outils de dépendances 
# il suffit de cloné le git d'abord
    git clone https://github.com/griffithlab/regtools  # Cloné le git
    cd regtools/ # Se deplacer sur le dossier cloné
    mkdir build  # Créer un dossier build
    cd build/    # Se deplacer dans le dossier
    cmake ..     # génération de fichiers de configuration pour la compilation de l'outil 
    make         # compilation

    regtools --version  # Pour verifier l'installation 
Bug et debug
Bug: Surprise, l'outil est installer mais avec la commande de verification on le trouve pas.
cause: C'est aprceque l'outil une fois installer n'est pas ajouter au path de votre système.
Debug: Avec cette commande:
export PATH=$PATH:$(pwd) # ajout au path
regtools --version # Verification tout marche bien
# l'outil est installer

# Reste à lancer l'outil sur les données, l'ordi personnelle ne peut pas le faire à cause des données lourdes.

# Actuellement je suis entrain d'installer rmats qui doit marcher aussi sous linux.











