# Termlinux
Install any version of Linux on Termux! 



Install Arch XFCE: Run the command pkg update -y && pkg install wget curl proot tar -y && wget https://raw.githubusercontent.com/AndronixApp/AndronixOrigin/master/Installer/Arch/armhf/arch-xfce.sh -O arch-xfce.sh && chmod +x arch-xfce.sh && bash arch-xfce.sh 





Ubuntu 18.04 Bionic Beaver LXDE: pkg update -y && pkg install wget curl proot tar -y && wget https://raw.githubusercontent.com/AndronixApp/AndronixOrigin/master/Installer/Ubuntu/ubuntu-lxde.sh -O ubuntu-lxde.sh && chmod +x ubuntu-lxde.sh && bash ubuntu-lxde.sh




Fedora 33 XFCE: pkg update -y && pkg install wget curl proot tar -y && wget https://raw.githubusercontent.com/AndronixApp/AndronixOrigin/master/Installer/Fedora/fedora-xfce.sh -O fedora-xfce.sh && chmod +x fedora-xfce.sh && bash fedora-xfce.sh







To install Wine: use SpilledWine 0.5 to install Wine on Termux. 

#!/bin/sh

LOGFILE="/data/data/com.termux/files/home/setup_spilled_wine.log"

log_message() {
    echo "$(date) - $1" >> $LOGFILE
}
log_message "Displaying README.txt and waiting for user input..."
cat /data/data/com.termux/files/home/README.txt
echo "Press Enter to continue..."
read -r
log_message "Updating and upgrading packages..."
pkg update && pkg upgrade -y >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to update and upgrade packages."
    exit 1
fi
log_message "Installing required packages..."
pkg install qemu-user-static gcc git cmake make termux-tools xorg-xhost lxde -y >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to install required packages."
    exit 1
fi
log_message "Cloning Box86 repository..."
git clone https://github.com/ptitSeb/box86 >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to clone Box86 repository."
    exit 1
fi

cd box86 || { log_message "Failed to change directory to box86."; exit 1; }

log_message "Building Box86..."
cmake .. -DRPI4ARM64=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to configure Box86 build."
    exit 1
fi

make >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to build Box86."
    exit 1
fi

make install >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to install Box86."
    exit 1
fi
log_message "Cloning Box64 repository..."
git clone https://github.com/ptitSeb/box64 >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to clone Box64 repository."
    exit 1
fi

cd box64 || { log_message "Failed to change directory to box64."; exit 1; }

log_message "Building Box64..."
cmake .. -DRPI4ARM64=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to configure Box64 build."
    exit 1
fi

make >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to build Box64."
    exit 1
fi

make install >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to install Box64."
    exit 1
fi
log_message "Downloading prebuilt Wine..."
wget https://github.com/AIOWinDev/spilledwine/raw/main/Wine-Builds-9.13.tar.gz -O /data/data/com.termux/files/home/wine.tar.gz >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to download Wine."
    exit 1
fi

log_message "Installing prebuilt Wine..."
tar -xzvf /data/data/com.termux/files/home/wine.tar.gz -C /data/data/com.termux/files/home/ >> $LOGFILE 2>&1
if [ $? -ne 0 ]; then
    log_message "Failed to extract Wine."
    exit 1
fi
log_message "Configuring Wine..."
winecfg
log_message "Setup completed. Please start Termux-X11 or XServer XSDL and let them run in the background."
echo "Setup completed. Please start Termux-X11 or XServer XSDL and let them run in the background."


To install additional settings, use this script: 


#!/bin/sh

LOGFILE="/data/data/com.termux/files/home/in_addons.log"
log_message() {
    echo "$(date) - $1" >> $LOGFILE
}
echo "Would you like to install extra gaming settings? (y/n)"
read -r response

if [ "$response" = "y" ]; then
    echo "Proceeding with extra settings installation..."
    log_message "Installing 7-Zip 9.20..."
    wget https://www.7-zip.org/a/7z920.exe -O /data/data/com.termux/files/home/7z920.exe >> $LOGFILE 2>&1
    log_message "7-Zip downloaded."
    log_message "Extracting and installing 7-Zip..."
    wine /data/data/com.termux/files/home/7z920.exe >> $LOGFILE 2>&1
    log_message "7-Zip installed."
    log_message "Installing Wine Mono..."
    wget https://dl.winehq.org/wine/wine-mono/8.0.0/wine-mono-8.0.0-x86.msi -O /data/data/com.termux/files/home/wine-mono.msi >> $LOGFILE 2>&1
    wine msiexec /i /data/data/com.termux/files/home/wine-mono.msi >> $LOGFILE 2>&1
    log_message "Wine Mono installed."
    log_message "Installing Wine Gecko..."
    wget https://dl.winehq.org/wine/wine-gecko/2.47.0/wine-gecko-2.47.0-x86.msi -O /data/data/com.termux/files/home/wine-gecko.msi >> $LOGFILE 2>&1
    wine msiexec /i /data/data/com.termux/files/home/wine-gecko.msi >> $LOGFILE 2>&1
    log_message "Wine Gecko installed."
    log_message "Installing DirectX..."
    wget https://download.microsoft.com/download/1/7/4/1746C5F2-4E47-4697-B100-7A425BF9B3F4/directx_Jun2010_redist.exe -O /data/data/com.termux/files/home/directx_installer.exe >> $LOGFILE 2>&1
    wine /data/data/com.termux/files/home/directx_installer.exe >> $LOGFILE 2>&1
    log_message "DirectX installed."
    log_message "Installing Visual C++ Redistributables..."
    wget https://download.microsoft.com/download/9/7/2/972B92E7-1F69-4B45-A8A0-7E6B546E6D85/vcredist_x86.exe -O /data/data/com.termux/files/home/vcredist_x86.exe >> $LOGFILE 2>&1
    wine /data/data/com.termux/files/home/vcredist_x86.exe >> $LOGFILE 2>&1
    log_message "Visual C++ Redistributables installed."

else
    echo "Skipping additional settings."
fi







To start Wine, run this script: #!/bin/sh

FILE_PATH=$1
if ! pgrep -x "XServer" > /dev/null && ! pgrep -x "xinit" > /dev/null; then
    echo "Please start XServer XSDL or Termux-X11 before running this script."
    exit 1
fi
export DISPLAY=:0

wine "$FILE_PATH"


