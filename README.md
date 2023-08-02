# Linux-Kernel-Compilation
Linux Kernel Compilation
## Building Linux Kernel
 This method has been adapted from [kernel compile notes](https://askubuntu.com/questions/718381/how-to-compile-and-install-custom-mainline-kernel/718662#718662) with few aditions based on personal experience
 1. Download desired kernel from https://multipath-tcp.org/ as tar

 2. Update System and Install Necessary packages
    ```bash
    sudo apt-get update
    sudo apt-get dist-upgrade
    sudo apt-get install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison
    ```
 3. Configure kernel and build
    ```bash
    cd linux-6.0.1
    
    # Copy existing configuration from current kernel
    cp /boot/config-$(uname -r) .config
    
    # (OPTIONAL) edit config using GUI menu
    make menuconfig

    # Ubuntu config file has full debugging. 
    # Makes an enormous kernel and takes twice as long to compile
    # (OPTIONAL) We can disable debug_info
    scripts/config --disable DEBUG_INFO

    # We also need to override certificate checking
    scripts/config --disable SYSTEM_TRUSTED_KEYS
    scripts/config --disable SYSTEM_REVOCATION_KEYS

    # Build Kernel and Modules
    # -j option specifies number of parallel jobs you want the process to use
    # nproc shows number of procesing units available
    # time records the time taken to compile it took me 95minutes
    time make -j $(nproc)

    # Install necessary modules
    time sudo make modules_install

    # Install Kernel
    sudo make install
    ```
