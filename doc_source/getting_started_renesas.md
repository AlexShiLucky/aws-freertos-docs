# Getting Started with the Renesas Starter Kit\+ for RX65N\-2MB<a name="getting_started_renesas"></a>

Before you begin, see [Prerequisites](freertos-prereqs.md)\.

If you do not have the Renesas RSK\+ for RX65N\-2MB, visit the AWS Partner Device Catalog, and purchase one from our [partners](https://devices.amazonaws.com/detail/a3G0L00000AAOkeUAH/Renesas-Starter-Kit+-for-RX65N-2MB)\.

## Setting Up the Renesas Hardware<a name="renesas-setup-hardware"></a>

**To set up the RSK\+ for RX65N\-2MB**

1. Connect your computer to the USB\-UART port on your RSK\+ for RX65N\-2MB\.

1. Connect a router or internet\-connected Ethernet port to the Ethernet port on your RSK\+ for RX65N\-2MB\.

**To set up the E2 Lite Debugger module**

1. Use the 14\-pin ribbon cable to connect the E2 Lite Debugger module to the ‘E1/E2 Lite’ port on the RSK\+ for RX65N\-2MB\.

1. Use a USB cable to connect the E2 Lite debugger module to your host machine\. When the E2 Lite debugger is connected to both the board and your computer, a green ‘ACT’ LED on the debugger flashes\.

1. Connect the center positive \+5V power adapter to the PWR connector on the RSK\+ for RX65N\-2MB\.

1. After the debugger is connected to your host machine and RSK\+ for RX65N\-2MB, the E2 Lite debugger drivers begin installing\.

   Note that administrator privileges are required to install the drivers\.

## Setting Up Your Environment<a name="renesas-setup-env"></a>

To set up Amazon FreeRTOS configurations for the RSK\+ for RX65N\-2MB, use the Renesas e2studio IDE and CC\-RX compiler\. 

**Note**  
The Renesas e2studio IDE and CC\-RX compiler are only supported on Windows 7, 8, and 10 operating systems\.

**To download and install e2studio**

1. Go to the [Renesas e2studio installer](https://www.renesas.com/in/en/software/D4000820.html) download page, and download the offline installer\.

1. You are directed to a Renesas Login page\.

   If you have an account with Renesas, enter your user name and password and then choose **Login**\.

   If you do not have an account, choose **Register now**, and follow the first registration steps\. You should receive an email with a link to activate your Renesas account\. Follow this link to complete your registration with Renesas, and then log in to Renesas\.

1. After you log in, download the e2studio installer to your computer\.

1. Open the installer and follow the steps to completion\.

For more information, see the [e2studio](https://www.renesas.com/us/en/products/software-tools/tools/ide/e2studio.html#productInfo) on the Renesas website\.

**To download and install the RX Family C/C\+\+ Compiler Package**

1. Go to the [RX Family C/C\+\+ Compiler Package](https://www.renesas.com/us/en/software/D4000652.html) download page, and download the V2\.08 package\.

1. Open the executable and install the compiler\.

For more information, see the [C/C\+\+ Compiler Package for RX Family](https://www.renesas.com/us/en/products/software-tools/tools/compiler-assembler/compiler-package-for-rx-family.html#productInfo) on the Renesas website\.

**Note**  
The compiler is available free for evaluation version only and valid for 60 days\. On the 61st day, you need to get a License Key\. For more information, see [Evaluation Software Tools](https://www.renesas.com/us/en/products/software-tools/evaluation-software-tools.html)\.

## Download and Configure Amazon FreeRTOS<a name="renesas-download-and-configure"></a>

After you set up your environment, you can download Amazon FreeRTOS\.

### Download Amazon FreeRTOS<a name="renesas-download"></a><a name="renesas-download-free-rtos"></a>

**To download Amazon FreeRTOS**

1. Browse to the AWS IoT console\.

1. In the navigation pane, choose **Software**\.

1. Under **Amazon FreeRTOS Device Software**, choose **Configure download**\.

1. Under **Software Configurations**, find **Connect to AWS IoT\- Renesas**, and then choose **Download**\.

1. Unzip the downloaded file to the `AmazonFreeRTOS` folder, and make a note of the folder's path\.

**Note**  
The maximum length of a file path on Microsoft Windows is 260 characters\. The longest path in the Amazon FreeRTOS download is 122 characters\. To accommodate the files in the Amazon FreeRTOS projects, make sure that the path to the `AmazonFreeRTOS` directory is fewer than 98 characters long\. For example, `C:\Users\Username\Dev\AmazonFreeRTOS` works, but `C:\Users\Username\Documents\Development\Projects\AmazonFreeRTOS` causes build failures\.  
In this tutorial, the path to the `AmazonFreeRTOS` directory is referred to as `BASE_FOLDER`\.

### Configure Your Project<a name="renesas-freertos-config-project"></a>

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

## Build and Run Amazon FreeRTOS Samples<a name="renesas-build-and-run-example"></a>

Now that you have configured the demo project, you are ready to build and run the project on your board\.

Before you run the demo project, use the MQTT client in the AWS IoT console to subscribe to the demo's MQTT topic\.

**To subscribe to the MQTT topic**

1. Sign in to the AWS IoT console\.

1. In the navigation pane, choose **Test** to open the MQTT client\.

1. In **Subscription topic**, enter **freertos/demos/echo**, and then choose **Subscribe to topic**\.

### Build the Amazon FreeRTOS Demo in e2studio<a name="renesas-freertos-import-project"></a>

**To import and build the demo in e2studio**

1. Launch e2studio from the Start menu\. 

1. On the **Select a directory as a workspace** window, browse to the folder that you want to work in, and choose **Launch**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/e2studio1.png)

1. The first time you open e2studio, the **Toolchain Registry** window opens\. Choose **Renesas Toolchains**, and confirm that **CC\-RX v2\.07\.00** is selected\. Choose **Register**, and then choose **OK**\.

1. If you are opening e2studio for the first time, the **Code Generator Registration** window appears\. Choose **OK**\.

1. The **Code Generator COM component register** window appears\. Under **Please restart e2studio to use Code Generator**, choose **OK**\.

1. The **Restart e2studio** window appears\. Choose **OK**\.

1. e2studio restarts\. On the **Select a directory as a workspace** window, choose **Launch**\.

1. On the e2studio welcome screen, choose the **Go to the e2studio workbench** arrow icon\.

1. Right\-click the **Project Explorer** window, and choose **Import**\.

1. In the import wizard, choose **General**, **Existing Projects into Workspace**, and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/e2studio2.png)

1. Choose **Browse**, locate the directory `<BASE_FOLDER>\demos\renesas\rx65n-rsk\ccrx-e2studio`, and then choose **Finish**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/e2studio3.png)

1. From **Project** menu, choose **Project**, **Build All**\.

   The build console issues a warning message that the License Manager is not installed\. You can ignore this message unless you have a license key for the CC\-RX compiler\. To install the License Manager, see the [License Manager](https://www.renesas.com/us/en/software/D4000398.html) download page\.

### Run the Amazon FreeRTOS Project<a name="renesas-run"></a>

**To run the project in e2studio**

1. Confirm that you have connected the E2 Lite Debugger module to your RSK\+ for RX65N\-2MB

1. From the top menu, choose **Run**, **Debug Configuration**\.

1. Expand **Renesas GDB Hardware Debugging**, and choose **aws\_demos HardwareDebug**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/e2studio4.png)

1. Choose the **Debugger** tab, and then choose the **Connection Settings** tab\. Confirm that your connection settings are correct\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/freertos/latest/userguide/images/e2studio5.png)

1. Choose **Debug** to download the code to your board and begin debugging\.

   You might be prompted by a firewall warning for `e2-server-gdb.exe`\. Check **Private networks, such as my home or work network**, and then choose **Allow access**\.

1. e2studio might ask to change to **Renesas Debug Perspective**\. Choose **Yes**\.

   The green 'ACT' LED on the E2 Lite Debugger illuminates\.

1. After the code is downloaded to the board, choose **Resume** to run the code up to the first line of the main function\. Choose **Resume** again to run the rest of the code\.

   In the AWS IoT console's MQTT client, you see MQTT messages sent by your device\.

For the latest projects released by Renesas, see the `renesas-rx` fork of the `amazon-freertos` repository on [GitHub](https://github.com/renesas-rx/amazon-freertos)\.