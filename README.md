
# Building DumberOS GSIs #

It is recommended to have at least 32GB of RAM + 32GB of swap, and at least 300GB of free disk space! Building from scratch will take around two hours on a high end PC.

## Set up your environment ##

    apt update
    apt install git-core gnupg flex bison build-essential zip curl zlib1g-dev \
       gcc-multilib g++-multilib libc6-dev-i386 libncurses5-dev x11proto-core-dev \
       libx11-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig python3 \
       python-is-python3 python3-venv libncurses6 libtinfo6 ccache libstdc++6 \
       openjdk-17-jdk git-lfs
       
    git lfs install

Install the repo command

    mkdir -p ~/.bin
    echo 'export PATH=~/.bin:$PATH' >> ~/.bashrc
    source ~/.bashrc
    curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
    chmod a+x ~/.bin/repo

Add a swap file if you don't have a swap partition active

    sudo fallocate -l 32G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile

## Prepare for building ##

Create a new working directory for your LineageOS build and navigate to it:

    mkdir dumberos_build; cd dumberos_build

Initialize your LineageOS workspace:

    repo init -u https://github.com/miki151/dumberos_manifests.git --git-lfs

Download the source code:
                                
    repo sync -c

Set up the build environment. Optionally replace vanilla31 with another variant: vanilla30, gapps31, gapps30. Choose the "30" if you're building for the Qin F21 pro, use "31" otherwise.

    . build/envsetup.sh 
    lunch lineage_vanilla31-ap2a-userdebug 

## Build an image signed with test keys ##

    DISABLE_STUB_VALIDATION=true make systemimage
    
## Build a self-signed image ##

Generate new private keys if you haven't already:

    bash scripts/gen_cert.sh

Build and sign the image:

    DISABLE_STUB_VALIDATION=true make target-files-package otatools
    bash scripts/sign_target_files.sh $OUT/signed-target_files.zip
    unzip -joq $OUT/signed-target_files.zip IMAGES/system.img -d $OUT

## Image location

Grab your fresh DumberOS image:

    ls $OUT/system.img
