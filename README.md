
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

## Build DumberOS ##

Create a new working directory for your LineageOS build and navigate to it:

    mkdir dumberos_build; cd dumberos_build

Initialize your LineageOS workspace:

    repo init -u https://github.com/miki151/dumberos_manifests.git --git-lfs

Clone both this and the patches repos:

    git clone https://github.com/miki151/dumberos_build lineage_build_unified
    git clone https://github.com/miki151/dumberos_patches lineage_patches_unified

Finally, start the build script - for example, to build all supported DumberOS variants:

    bash lineage_build_unified/buildbot_unified.sh treble DG31 DG30 DV31 DV30

In case of RESOURCE_EXHAUSTED or other quota errors, just give it another run. Afterwards, add the 'nosync' option to buildbot_unified.sh, to avoid syncing every time you build:

    bash lineage_build_unified/buildbot_unified.sh treble nosync ...

Be sure to update the cloned repos from time to time!

---
