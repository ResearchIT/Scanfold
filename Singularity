Bootstrap: docker
From: centos:7

%post
    yum -y update
    yum -y install epel-release wget git gcc-c++ zlib-devel make which glibc-devel glibc-static
    yum -y install python36 python36-devel
    wget https://bootstrap.pypa.io/get-pip.py
    python36 get-pip.py
    python36 -m pip install biopython
    python36 -m pip install numpy
    python36 -m pip install pybigwig
    python36 -m pip install requests

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
        git clone https://github.com/moss-lab/ScanFold.git /opt/scanfold
    else
        pushd /opt/scanfold
        git pull
        popd
    fi
 

    pushd /opt/scanfold
    git checkout master
    popd

    
%environment
    export PATH=/opt/rnastructure/RNAstructure/exe:/opt/scanfold:$PATH
