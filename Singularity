Bootstrap: docker
From: centos:7

%post
    yum -y update
    yum -y install epel-release wget git gcc-c++ zlib-devel make which
    yum -y install python36 python36-devel
    yum -y install python34-pip
    wget https://bootstrap.pypa.io/get-pip.py
    python36 get-pip.py
    python36 -m pip install biopython
    python36 -m pip install numpy
    python36 -m pip install pybigwig
    python36 -m pip install requests
    python3 -m pip install biopython numpy requests

    # Install ViennaRNA & Python bindings
    if [ ! -f /usr/bin/RNAfold ];
    then
        wget https://www.tbi.univie.ac.at/RNA/download/epel/epel_7/perl-rna-2.4.11-1.x86_64.rpm
        wget https://www.tbi.univie.ac.at/RNA/download/epel/epel_7/viennarna-2.4.11-1.x86_64.rpm
        wget https://www.tbi.univie.ac.at/RNA/download/epel/epel_7/python3-rna-2.4.11-1.x86_64.rpm
        yum -y install viennarna-2.4.11-1.x86_64.rpm perl-rna-2.4.11-1.x86_64.rpm python3-rna-2.4.11-1.x86_64.rpm
    fi

    # Install RNAstructure 
    if [ ! -d /opt/rnastructure ];
    then
        mkdir -p /opt/rnastructure
        wget http://rna.urmc.rochester.edu/Releases/current/RNAstructureSource.tgz
        tar xzvf RNAstructureSource.tgz -C /opt/rnastructure
        pushd /opt/rnastructure/RNAstructure
        make ct2dot
        popd
    fi

    # Install ScanFold
    if [ ! -d /opt/scanfold ]
    then
        mkdir -p /opt/scanfold
        git clone https://github.com/baberlevi/ScanFold.git /opt/scanfold
        pushd /opt/scanfold
        git checkout master
        popd
    else
        pushd /opt/scanfold
        git checkout master
        git pull
        popd
    fi
 
%environment
    export PATH=/opt/rnastructure/RNAstructure/exe:/opt/scanfold:$PATH
    export DATAPATH=/opt/rnastructure/RNAstructure/data_tables

%runscript
    exec python36 $@

%apprun scan
    exec python3 /opt/scanfold/ScanFold-Scan_Webserver.py $@

%apprun fold
    exec python36 /opt/scanfold/ScanFold-Fold_spinoff.py $@

%help
Run this container by specifying the scan or fold step:
e.g.
$ singularity run shub://ResearchIT/scanfold fold -i input.tsv ...

