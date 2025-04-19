# How to get Vivado running on your M series MacBook (or Windows on ARM)

## Background

This guide documents the exact steps I followed to get Vivado running on an Apple Silicon (M3 Pro) MacBook by installing a Windows ARM environment. The goal is to provide a clear, copy‑and‑pasteable procedure so that anyone without a native Windows PC, or with a Windows on ARM PC can set up the Vivado Design Suite successfully.

## Step 1: Install Parallels (or UTM)

On M‑series Macs you have two main virtualization options:

**Parallels**  
- Native Apple Silicon support, polished UI, seamless integration, good graphics acceleration.  
- Commercial license with free trial; requires licence

**UTM**  
- Free and open‑source; uses QEMU under the hood.  
- Slightly lower performance and fewer integration features compared to Parallels.

**Recommended:** If you need maximum performance and ease of use, choose Parallels. If you prefer a free solution and are comfortable with manual setup, UTM works well.

1. Download your chosen installer:
   - **Parallels:** https://www.parallels.com/products/desktop/  
   - **UTM:** https://mac.getutm.app/
2. Install the app as you would any macOS application.
3. Launch it and create a new virtual machine:
   - **Parallels:**
    - Run a Windows VM (Pretty straight forward when you open up Parallels)
   - **UTM:**
        - Attach a Windows 11 ARM ISO (download from Microsoft’s website).
        - Allocate at least 4 CPU cores, 8 GB RAM, and 100GB of storage for smooth Vivado performance and enough space to run Vivado.
        - This might take a while, so maybe grab a cup of coffee :)

## Step 2: Download and Install Vivado

1. In your Windows ARM virtual machine, open a web browser and go to [Download Vivado](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/2024-2.html).  
2. Sign in (or register) and select the Vivado Design Suite installer for Windows.  
3. Download the installer executable.
4. Run the installer:
   - Accept the license agreement.
   - Select default install directory (e.g., `C:\Xilinx\Vivado\`).
   - Include at minimum:
     - Vivado IDE
     - Device support for your target FPGA families
     - Tcl scripts
     - Make sure to select "Install Cable Drivers", see image below
![Image of Vivado Installer Wizard](/assets/vivado_drivers.png)
5. Wait for the installation to complete (may take 30–45 minutes or hours depending on your internet speed).
6. After installation is complete, and you try running Vivado for the first time, you will encounter this error
![Image of System Error](/assets/vvgl_system_error.png)

We will find out how to resolve this error in the next section.

## Step 3: Download the Latest Microsoft Visual C++ Redistributable

To fix the error above, we need to download the latest MS Visual C++ Redistributable

1. Open a browser in your Windows VM and go to [MS Visual C++ Redistributable](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)  
2. Download the **ARM** redistributable.
3. Run the executable and follow the prompts to install.
4. Restart Windows (if needed) when prompted to ensure the runtime is fully applied.
5. Try running Vivado again and hurray!, it finally works! Well, not just yet :(

## Step 4: Download Diligent Adept Runtime

When you plug in your Basys3 board (this is what I used, maybe other FPGA boards may need another work arounds, or not), it doesn't pop up in the Hardware Manager 
![No Connected Hardware](/assets/no_hardware_target.png)
To resolve this, you need to download the [Diligent Adept Runtime](https://digilent.com/reference/software/adept/start?redirect=1#software_downloads)

1. In Windows VM, browse to [Download Diligent Adept Runtime](https://digilent.com/reference/software/adept/start?redirect=1#software_downloads) 
2. Download the Windows installer (Adept Runtime for Windows Download).
3. Run the installer, accept defaults, and complete the installation.
4. After installation, connect your Basys3 board; Open Vivado and try autoconnecting again, this time, your FPGA should show up! 
5. HURRAY!!! Now you have Vivado running on your Mac