# Getting Started with the Xilinx Avnet MicroZed Industrial IoT Kit<a name="getting_started_xilinx"></a>

Before you begin, see [Prerequisites](freertos-prereqs.md)\.

If you do not have the Xilinx Avnet MicroZed Industrial IoT Kit, visit the AWS Partner Device Catalog to purchase one from our [partner](https://devices.amazonaws.com/detail/a3G0L00000AANtqUAH/MicroZed-IIoT-Bundle-with-Amazon-FreeRTOS)\.

## Setting Up the MicroZed Hardware<a name="xilinx-setup-hardware"></a>

The following diagram might be helpful when you set up the MicroZed hardware:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/microzed.png)

**To set up the MicroZed board**

1. Connect your computer to the USB\-UART port on your MicroZed board\.

1. Connect your computer to the JTAG Access port on your MicroZed board\.

1. Connect a router or internet\-connected Ethernet port to the Ethernet and USB\-Host port on your MicroZed board\.

## Setting Up Your Environment<a name="xilinx-setup-env"></a>

To set up Amazon FreeRTOS configurations for the MicroZed kit, you must use the Xilinx Software Development Kit \(XSDK\)\. XSDK is supported on Windows and Linux\.

### Download and Install XSDK<a name="install-xsdk"></a>

To install Xilinx software, you need a free Xilinx account\.

**To download the XSDK**

1. Go to the [Software Development Kit Standalone WebInstall Client](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools.html) download page\.

1. Choose the option appropriate for your operating system\.

1. You are directed to a Xilinx sign\-in page\.

   If you have an account with Xilinx, enter your user name and password and then choose **Sign in**\.

   If you do not have an account, choose **Create your account**\. After you register, you should receive an email with a link to activate your Xilinx account\. 

1. On the **Name and Address Verification** page, enter your information and then choose **Next**\. The download should be ready to start\.

1. Save the `Xilinx_SDK_version_os` file\.

**To install the XSDK**

1. Open the `Xilinx_SDK_version_os` file\.

1. In **Select Edition to Install**, choose **Xilinx Software Development Kit \(XSDK\)** and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk1.png)

1. On the following page of the installation wizard, under **Installation Options**, select **Install Cable Drivers** and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk2.png)

If your computer does not detect the MicroZed's USB\-UART connection, install the CP210x USB\-to\-UART Bridge VCP drivers manually\. For instructions, see the [Silicon Labs CP210x USB\-to\-UART Installation Guide](https://www.xilinx.com/support/documentation/boards_and_kits/install/ug1033-cp210x-usb-uart-install.pdf)\.

For more information about XSDK, see the [Getting Started with Xilinx SDK](https://www.xilinx.com/html_docs/xilinx2018_2/SDK_Doc/index.html) on the Xilink website\.

## Download and Configure Amazon FreeRTOS<a name="xilinx-download-and-configure"></a>

After you set up your environment, you can download Amazon FreeRTOS\.

### Download Amazon FreeRTOS<a name="xilinx-download"></a><a name="xilinx-download-free-rtos"></a>

1. Browse to the AWS IoT console\.

1. In the navigation pane, choose **Software**\.

1. Under **Amazon FreeRTOS Device Software**, choose **Configure download**\.

1. Under **Software Configurations**, find **Connect to AWS IoT\- Xilinx**, and then choose **Download**\.

1. Unzip the downloaded file to the `AmazonFreeRTOS` folder, and make a note of the folder's path\.

**Note**  
The maximum length of a file path on Microsoft Windows is 260 characters\. The longest path in the Amazon FreeRTOS download is 122 characters\. To accommodate the files in the Amazon FreeRTOS projects, make sure that the path to the `AmazonFreeRTOS` directory is fewer than 98 characters long\. For example, `C:\Users\Username\Dev\AmazonFreeRTOS` works, but `C:\Users\Username\Documents\Development\Projects\AmazonFreeRTOS` causes build failures\.  
In this tutorial, the path to the `AmazonFreeRTOS` directory is referred to as `BASE_FOLDER`\.

### Configure Your Project<a name="xilinx-freertos-config-project"></a>

To run the demo, you must configure your project to work with AWS IoT\. To configure your project to work with AWS IoT, your board must be registered as an AWS IoT thing\. This is a step in the [Prerequisites](freertos-prereqs.md)\.

**To configure your AWS IoT endpoint**

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iotv2/)\.

1. In the navigation pane, choose **Settings**\.

   Your AWS IoT endpoint appears in the **Endpoint** text box\. It should look like `<1234567890123>-ats.iot.<us-east-1>.amazonaws.com`\. Make a note of this endpoint\.

1. In the navigation pane, choose **Manage**, and then choose **Things**\. Make a note of the AWS IoT thing name for your device\. 

1. With your AWS IoT endpoint and your AWS IoT thing name on hand, open `<BASE_FOLDER>\demos\common\include\aws_clientcredential.h` in your IDE, and specify values for the following `#define` constants:
   + `clientcredentialMQTT_BROKER_ENDPOINT` *Your AWS IoT endpoint*
   + `clientcredentialIOT_THING_NAME` *Your board's AWS IoT thing name*

**To configure your AWS IoT credentials**

To configure your AWS IoT credentials, you need the private key and certificate that you downloaded from the AWS IoT console when you registered your device as an AWS IoT thing\. After you have registered your device as an AWS IoT thing, you can retrieve device certificates from the AWS IoT console, but you cannot retrieve private keys\.

Amazon FreeRTOS is a C language project, and the certificate and private key must be specially formatted to be added to the project\. You need to format the certificate and private key for your device\. 

1. In a browser window, open `<BASE_FOLDER>\tools\certificate_configuration\CertificateConfigurator.html`\.

1. Under **Certificate PEM file**, choose the `<ID>-certificate.pem.crt` that you downloaded from the AWS IoT console\.

1. Under **Private Key PEM file**, choose the `<ID>-private.pem.key` that you downloaded from the AWS IoT console\.

1. Choose **Generate and save aws\_clientcredential\_keys\.h**, and then save the file in `<BASE_FOLDER>\demos\common\include`\. This overwrites the existing file in the directory\.
**Note**  
The certificate and private key should be hard\-coded for demonstration purposes only\. Production\-level applications should store these files in a secure location\.

## Build and Run Amazon FreeRTOS Demo Project<a name="xilinx-build-and-run-example"></a>

Now that you have configured your project, you are ready to build and run the demo project on your board\.

Before you run the demo project, use the MQTT client in the AWS IoT console to subscribe to the demo's MQTT topic\. 

**To subscribe to the MQTT topic**

1. Sign in to the AWS IoT console\.

1. In the navigation pane, choose **Test** to open the MQTT client\.

1. In **Subscription topic**, enter **freertos/demos/echo**, and then choose **Subscribe to topic**\.

### Open the Amazon FreeRTOS Demo in the XSDK IDE<a name="xilinx-freertos-import-project"></a>

1. Launch the XSDK IDE with the workspace directory set to `<BASE_FOLDER>\demos\xilinx\microzed\xsdk`\. 

1. Close the welcome page\. From the menu, choose **Project**, and then clear **Build Automatically**\.

1. From the menu, choose **File**, and then choose **Import**\.

1. On the **Select** page, expand **General**, choose **Existing Projects into Workspace**, and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk3.png)

1. On the **Import Projects** page, choose **Select root directory**, and then enter the root directory of your demo project\. To browse for the directory, choose **Browse**\. 

   After you specify a root directory, the projects in that directory appear on the **Import Projects** page\. All available projects are selected by default\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk4.png)
**Note**  
If you see a warning at the top of the **Import Projects** page \("Some projects cannot be imported because they already exist in the workspace\."\) you can ignore it\.

1. With all of the projects selected, choose **Finish**\. The XSDK IDE opens all of the projects that are required for the `aws_demos` project to build and run on the MicroZed board\.

1. From the menu, choose **Window**, and then choose **Preferences**\.

1. In the navigation pane, expand **Run/Debug**, choose **String Substitution**, and then choose **New**\. 

1. In **New String Substitution Variable**, for **Name**, enter **AFR\_ROOT**\. For **Value**, enter the root path of the `aws_demos`\. Choose **OK**, and then choose **OK** to save the variable and close **Preferences**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk-string-sub.png)

### Build the Amazon FreeRTOS Demo Project<a name="xilinx-freertos-build-project"></a>

1. In the XSDK IDE, from the menu, choose **Project**, and then choose **Clean**\. 

1. In **Clean**, leave the options at their default values, and then choose **OK**\. XSDK cleans and builds all of the projects, and then generates `.elf` files\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk-clean.png)
**Note**  
To build all projects without cleaning them, choose **Project**, and then choose **Build All**\.  
To build individual projects, select the project you want to build, choose **Project**, and then choose**Build Project**\.

### JTAG Debugging<a name="xilinx-jtag"></a>

1. Set your MicroZed board's boot mode jumpers to the JTAG boot mode:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xilinx-jtag.png)

1. Insert your MicroSD card into the MicroSD card slot located directly under the USB\-UART port\.
**Note**  
Before you debug, be sure to back up any content that you have on the MicroSD card\.

   Your board should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xilinx-jtag-full-board.png)

1. In the XSDK IDE, right\-click **aws\_demos**, choose **Debug As**, and then choose **1 Launch on System Hardware \(System Debugger\)**\.

1. When the debugger stops at the breakpoint in `main()`, from the menu, choose **Run**, and then choose **Resume**\.
**Note**  
The first time you run the application, a new certificate\-key pair is generated\. For subsequent runs, you can comment out `vDevModeKeyProvisioning()` in the `main.c` file before you rebuild the images and the `BOOT.bin` file\. This prevents the copying of the certificates and key to storage on every run\.

You can opt to boot your MicroZed board from a MicroSD card or from QSPI ﬂash to run the Amazon FreeRTOS demo project\. For instructions, see [Generate the Boot Image for the Amazon FreeRTOS Demo Project](#xilinx-build-boot-image) and [Run the Amazon FreeRTOS Demo Project](#xilinx-run)\.

### Generate the Boot Image for the Amazon FreeRTOS Demo Project<a name="xilinx-build-boot-image"></a>

1. In the XSDK IDE, right\-click **aws\_demos**, and then choose **Create Boot Image**\. 

1. In **Create Boot Image**, choose **Create new BIF file**\.

1. Next to **Output BIF file path**, choose **Browse**, and then choose `aws_demos.bif` located at `<BASE_FOLDER>\demos\xilinx\microzed\xsdk\aws_demos\bootimage\aws_demos.bif`\.

1. Choose **Add**\. 

1. On **Add new boot image partition**, next to **File path**, choose **Browse**, and then choose `fsbl.elf`, located at `<BASE_FOLDER>\lib\third_party\mcu_vendor\xilinx\fsbl\Debug\fsbl.elf`\.

1. For the **Partition type**, choose **bootloader**, and then choose **OK**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk5.png)

1. On **Create Boot Image**, choose **Create Image**\. On **Override Files**, choose **OK** to overwrite the existing `aws_demos.bif` and generate the `BOOT.bin` file at `demos\xilinx\microzed\xsdk\aws_demos\bootimage\BOOT.bin`\.

### Run the Amazon FreeRTOS Demo Project<a name="xilinx-run"></a>

To run the Amazon FreeRTOS demo project, you can boot your MicroZed board from a MicroSD card or from QSPI flash\.

As you set up your MicroZed board for running the Amazon FreeRTOS demo project, refer to the diagram in [Setting Up the MicroZed Hardware](#xilinx-setup-hardware)\. Make sure that you have connected your MicroZed board to your computer\.

#### Boot the Amazon FreeRTOS Project from a MicroSD Card<a name="xilinx-build-boot-sd"></a>

Format the MicroSD card that is provided with the Xilinx MicroZed Industrial IoT Kit\.

1. Copy the `BOOT.bin` file to the MicroSD card\.

1. Insert the card into the MicroSD card slot directly under the USB\-UART port\.

1. Set the MicroZed boot mode jumpers to SD boot mode:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xilinx-sd.png)

1. Press the RST button to reset the device and start booting the application\. You can also unplug the USB\-UART cable from the USB\-UART port, and then reinsert the cable\.

#### Boot the Amazon FreeRTOS Demo Project from QSPI flash<a name="xilinx-build-boot-qspi"></a>

1. Set your MicroZed board's boot mode jumpers to the JTAG boot mode:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xilinx-jtag.png)

1. Verify that your computer is connected to the USB\-UART and JTAG Access ports\. The green Power Good LED light should be illuminated\.

1. In the XSDK IDE, from the menu, choose **Xilinx**, and then choose **Program Flash**\. 

1. In **Program Flash Memory**, the hardware platform should be filled in automatically\. For **Connection**, choose your MicroZed hardware server to connect your board with your host computer\.
**Note**  
If you are using the Xilinx Smart Lync JTAG cable, you must create a hardware server in XSDK IDE\. Choose **New**, and then define your server\.  

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk6.png)

1. In **Image File**, enter the directory path to your `BOOT.bin` image file\. Choose **Browse** to browse for the file instead\.

1. In **Offset**, enter **0x0**\.

1. In **FSBL File**, enter the directory path to your `fsbl.elf` file\. Choose **Browse** to browse for the file instead\.

1. Choose **Program** to program your board\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xsdk7.png)

1. After the QSPI programming is complete, remove the USB\-UART cable to power off the board\.

1. Set your MicroZed board's boot mode jumpers to the QSPI boot mode:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/xilinx-qspi.png)

1. Insert your card into the MicroSD card slot located directly under the USB\-UART port\.
**Note**  
Be sure to back up any content that you have on the MicroSD card\.

1. Press the RST button to reset the device and start booting the application\. You can also unplug the USB\-UART cable from the USB\-UART port, and then reinsert the cable\.

## Troubleshooting<a name="xilinx-troubleshooting"></a>

### General Troubleshooting Tips<a name="xilinx-troubleshooting-general"></a>
+ If you encounter build errors that are related to incorrect paths, try to clean and rebuild the project, as described in [Build the Amazon FreeRTOS Demo Project](#xilinx-freertos-build-project)\.
**Note**  
If you are using Windows, make sure that you use forward slashes when you set the string substitution variables in the Windows XSDK IDE\.