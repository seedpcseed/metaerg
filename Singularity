Bootstrap: docker
From: ubuntu:19.04

%maintenance
MAINTAINER Pat Seed
LABEL version="1.0.4"

%environment
PATH=$PATH:/NGStools:/NGStools/MinPath:/NGStools/aragorn:/NGStools/minced:/NGStools/Prodigal:/NGStools/ncbi-blast-2.9.0+/bin:/NGStools/diamond:/NGStools/hmmer/src:/NGStools/MinPath:/NGStools/metaerg/bin

%post
apt-get update && apt-get install -y
    autoconf \
    #build-essential \
    cpanminus \
    gcc-multilib \
    git \
    make \
    openjdk-8-jdk \
    perl \
    python \
    sqlite3 \
    tar \
    unzip \
    wget

apt-get update && apt-get install -y \
    expat \
    graphviz \
    libdb-dev \
    libgdbm-dev \
    libexpat1 \
    libexpat-dev \
    libssl-dev \
    libxml2-dev \
    libxslt1-dev \
    zlib1g-dev

cpanm Bio::Perl \
    DBI \
    Archive::Extract \
    DBD::SQLite \
    File::Copy::Recursive \
    Bio::DB::EUtilities \
    LWP::Protocol::https

git clone https://git.code.sf.net/p/swissknife/git swissknife-git && \
    cd swissknife-git && \
    perl Makefile.PL && \
    make install && \
    cd /NGStools

#aragorn
git clone https://github.com/TheSEED/aragorn.git && \
    cd aragorn && \
    gcc -O3 -ffast-math -finline-functions -o aragorn aragorn1.2.36.c && \
    cd /NGStools

#hmmer rRNAFinder need it
git clone https://github.com/EddyRivasLab/hmmer && \
    cd hmmer && \
    git clone https://github.com/EddyRivasLab/easel && \
    autoconf && \
    ./configure && \
    make  && \
    cd /NGStools

#blast for classifying rRNA sequences
wget ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.9.0+-x64-linux.tar.gz && \
    tar -xzf ncbi-blast-2.9.0+-x64-linux.tar.gz && \
    rm ncbi-blast-2.9.0+-x64-linux.tar.gz && \
    cd /NGStools

#prodigal
git clone https://github.com/hyattpd/Prodigal.git && \
    cd Prodigal && \
    make && \
    cd /NGStools

#minced
git clone https://github.com/ctSkennerton/minced.git && \
    cd minced && \
    make && \
    cd /NGStools

#diamond
mkdir diamond && \
   cd diamond && \
    wget http://github.com/bbuchfink/diamond/releases/download/v0.9.24/diamond-linux64.tar.gz && \
    tar -xzf diamond-linux64.tar.gz && \
    rm diamond-linux64.tar.gz diamond_manual.pdf && \
    cd /NGStools

#MinPath
wget http://ebg.ucalgary.ca/metaerg/minpath1.4.tar.gz && \
    tar -xzf minpath1.4.tar.gz && \
    rm minpath1.4.tar.gz && \
    cd /NGStools

#metaerg
git clone https://github.com/seedpcseed/metaerg.git

# Clean
apt-get remove -y autoconf \
    cpanminus \
    gcc-multilib \
    git \
    make && \
    apt-get autoclean -y

%runscript
exec "$@"
