# Introduction

This document describes usage of the Mopria Certification Tool (MCT), which runs required tests for devices to receive Mopria certification.

* Version: ${version}
* Build date: ${build_date}

## Installation

MCT supports any operating system with full Java 8 + JavaFX support (Java 9+ is not yet supported). This requirement can be met with most Windows, Mac, and Linux distributions.

After downloading the latest `.zip` archive from Mopria, simply expand the archive and double-click on the extracted `MopriaCertificationTool.jar` file.

![Home Screen](home_version.png)

When successful, you'll see a window containing the current tool version number, build date, and supported specifications.  

Upon successful execution, MCT will create a data folder in the following directory: 

* Windows: `%userprofile%/AppData/Roaming/MopriaCertificationTool`
* Linux/Mac: `~/.mct/`

Do not modify the contents of this directory. It may be removed if the program is no longer used.

## Analytics

When you start MCT for first time, you be asked to allow user data collection. We use user data to help improve the MCT tool. We do not collect any personal identifying information or device specifics. All client IP address's are anonymized before being sent to Google Analytics.  For more information, please see the [Mopria Privacy Policy](https://mopria.org/privacy-policy). Data collected includes:

| Name | Description |
| ---  | ---  |
| Page | What screen the event took place on |
| Action | What action happened ex) Test Executed, Button hit, etc |
| Plan | Randomly generated ID for test being executed |
| Result | Whether or not the test passed or failed |
| Specification | Which specification is being tested against |
| Time Elapsed | How much time the Action took |
| Tool Version | Which version of MCT is being run |
| Java Version | Which version of Java the client is using |
| Operating System| Which operating system client is using |

You may opt out of data collection at any time by clicking the "Settings" button on the MCT home screen, turning the toggle to "Off" and clicking "Save".  

![Analytic Prompt](mct_settings.png)

## Creating a Project

To create a project, click the `+` button in the lower right of the project list.

![Add Project Button](home_add.png)

In the following dialog, you can select a target device and the relevant specifications.

![Create Project Dialog](home_create_project.png)

If a device does not appear on the list, it was not discovered on the current network using MDNS ("Bonjour") discovery. Ensure the app is running on the same network, and that firewall rules allow you to perform MDNS discoveries.

If the device you want to test is still not in the list, you may select "Add by IP address", but MDNS-based tests will fail.

## Working with Projects

You can create many test projects, but only one per test device.

Action buttons in the top right let you manage the project. Using these buttons, you can delete a project, export its contents, or reconfigure it (to add or remove specifications and network transports for the device).

![Project Action Buttons](home_project_actions.png)

Each specification selected results in a "Test Plan". Click on one to begin testing.

![Test Plans](home_project_plans.png)

## Running Tests

Select a test or test group and click the "Play" button to run those tests. A log for each test appears in the panel below.

Tests may produce on-screen prompts, which must be answered for the test to proceed.

After test execution is over, click on a test to review its results.

Test results may also be deleted, exported, or run again using the action buttons on the lower right.

![Test Report Actions](plan_test_actions.png)

To return to the project list, use the left arrow button at the top left of the screen.

Note: Some tests require device capabilities to be queried first. If necessary, a capability test will be executed automatically, which may update the `Device Info` and may also change the available tests in the current plan.

### Test Categories

Tests are categorized according to their current status:

* **Mandatory**. Mandatory tests are required whenever a device has support for features being tested. Features that SHOULD or MAY be supported result in a Mandatory test whenever the device claims support for that feature.
* **Advisory**. Advisory tests validate behavior that is not strictly required but SHOULD or MAY be implemented a certain fashion.
* **Deprecated**. Deprecated tests are planned for removal but may still provide useful data.
* **Proposed**. Proposed tests are planned to be added to a future version of the relevant test case library.
* **Always**. "Always" tests are used to discover device features, which are used to update the test plan so that it only contains tests that apply to your device. Any "Always" test is executed once, automatically, before other tests in the plan can run.

Only **Mandatory** tests are used to determine eligibility for certification.

### Test Results

* ![Unknown](unknown.png) **Unknown**. There are no test results for this test, or its results have been deleted.
* ![Queue](queue.png) **Queue**. The test is queued to run.
* ![Run](run.png) **Run**. The test is currently running.
* ![Warn](warn.png) **Warn**. The test executed and failed, but due to the test category, the device may still be certified.
* ![Pass](pass.png) **Pass**. The test executed to completion with success.
* ![Fail](fail.png) **Fail**. The test executed, but during execution a failure was detected. To certify the device, you may need to:
  * Review the test report to see why the test failed.
  * Adjust device or network setup and run the test again.
  * Address a problem in the target device.
  * File a waiver if the test is inaccurately reporting results.
* ![Unverified](unverified.png) **Unverified**. The test executed to completion with success, but additional test evidence is required.

## Running Scanner Certification Tests

Scanner tests are written to implement the Mopria Alliance Scanner Test Case Library (STCL). Tests in this library may be used to certify behavior of a scanner device.

Many scanner tests require a "test sheet", a double-sided page with content in specific locations that can be detected automatically. To create this sheet print the included `print_scan_sampler.pdf` document.

![Test Sheet Letter](test_sheet_letter.png)

Use the following print settings to create the test sheet:

Setting | Value
---|---
Page Size | Letter or A4
Quality | High
Borderless | Enabled (if possible)
Color mode | Full color (required for testing color scanners)
Two-sided | Long-edge duplex (required for testing duplex scanners)

Note that the test sheet should be printed at 100% size so that each QR code is 1" x 1" in the printed output and 1" x 1" from the top left of the page.

* For testing scan offset, `print print_scan_offset_5x7sampler.pdf` a 5x7 inch document or `print_scan_offset_A4sampler.pdf`, a A4 document.
* For testing compression, print `print_scan_compression_4x6_sampler.pdf` or 
  
  `print_scan_compression_USLetter_sampler.pdf`, a 4x6 inch or US Letter document.
* For testing maximum scan area using a larger format scanner (such as Ledger or A3), a large-format test sheet should be printed or otherwise created. For example, a A4/Letter sheet could simply be affixed to a larger A3/Ledger sheet if necessary. The larger sheet should be at the input size required by the scanner, with the content at 100% size with content aligned to the top-left of the sheet, both front and back, as shown below:
![Test Sheet Ledger](test_sheet_ledger.png)
* When loading the sheet during tests:
  * The FRONT side must face up or down as pictured on the scanner.
  * In general, orient the page so the longest possible edge enters the scanner (ADF) or aligns with the scanner bar (Platen).

## Running Printer Certification Tests

Printer tests implement the Mopria Alliance Printer Test Case Library (PTCL). These tests allow you to certify a Mopria-compatible printer.

To prepare for testing:

* Ensure you have an adequate supply of paper and ink/toner.
* Have on hand all Mopria page sizes supported by your printer. Have a larger amount of your printer's "default" paper size, usually Letter or A4.
* Some tests end in an "UNVERIFIED" state requiring additional evidence, using this icon: ![Unverified](unverified.png). Printed pages requiring verification will also show this icon. To supply this evidence, scan and import the printed results.
  * Use a full-color, duplex scanner, using portrait mode and long-edge duplex, preserving the front and back of each page into PDF document(s). A single PDF may contain results for many tests.
  * If you are not verifying any duplex tests, a one-sided scan may be used. When importing this evidence, MCT will pad the input with blank back-side pages to simulate a full-duplex scan.
  * The scanned test evidence must match the specific execution of a test. For example, if you run a certain test multiple times, only the most recent printed results can be used as evidence.
  * Drag and drop the scanned PDF file(s) into the test plan UI.    
    ![Dragged Evidence](dragdrop_evidence.png)
  * OR, use the floating action button to select a scanner on your network and scan the pages from within the test tool.    
    ![Import](import_test_evidence_floatingbutton.png)
  * Imported data is automatically attached to matching test(s) by matching QR codes. If the QR code is distorted, cropped, or unreadable, the process will fail.
  * Updated tests automatically verify the data is correct and produce a final pass/fail result.
* Some tests require a "reference client". The Mopria Print Service may be used, or another reference client if one is approved.

## Running Print Client Certification Tests

Print Client tests cover implementations of the Print Client role defined in the Mopria Print Specification. In most tests, MCT advertises as a virtual printer which must be discovered and printed to by the Print Client under test.

To prepare for testing:

* Copy the Print Client sampler files to your client device.
* Ensure the Print Client is on the same local area network with the MCT. It's best if this LAN does not have too many devices.
* Create a new project in MCT by using the + button and select Add Client Device from the dropdown.

![Add Print Client](add_new_print_client.png)

* Add a new entry with a name and model corresponding to the Print Client:

![Add Print Client Model](add_print_client_model.png)

* Select an available "Print Client" specification and open the corresponding plan.
* Run the "Setup / Select Features" test and check each feature implemented by your Print Client. If you're not sure, enable it so you can run corresponding tests and find out. You can always run Select Features again later to unselect features your Print Client does not actually have.
* Run the "Setup / Client Information" test to record additional information about the client.

During testing, MCT will expose virtual printers at various ports (6310 and higher). Virtual printers will contain the name "fake printer" and may be prefixed with your user name, which is configurable in settings. You can ignore any extra numbers added in parenthesis during tests; for example, ignore the `(2)` in `user's fake printer (2)`.

The machine running MCT must allow use of ports as above. Your network and print client must allow discovery and connection to the MCT host and various ports, as advertised during each test.

Some tests require a "pass-through" print. This refers to sending a document in its original, native format (such as JPG, XLS, or DOC) instead of first converting it to a Mopria type like PWG-Raster, PCLM, or PDF. Consult your print client's documentation to see if this is supported and if so, how to activate this feature.

Finishing tests will run to validate each Mopria finishing supported by the Printer. Some finishings may only work on large-format media size. To support this, finishing tests will first attempt to validate a print on the default media size. If validation fails, the test will then request a large-format size (A3 for a printer with A4 default, Ledger for a printer with Letter default).

## Submitting Test Results

When you are satisfied with the results of all test executions for a device, you can extract a complete archive of test results using the export (![export](export.png)) button in the main window.

![Project Action Buttons](home_project_actions.png)

This produces a `.zip` file that can be uploaded to Mopria using the certification portal. See the [Mopria Certification Program Website](https://portal.mopria.org/CertificationProgram.aspx) for more information.

Test result for a particular test can also be submitted by selecting the test whose result has to be exported. The window gives
the option to submit selected test result or all the results of the tests that has been executed.

![Test Action Buttons](export_test_results.png)

### Test Waivers

If a test fails but you believe it should not (for example, due to a defect in the test tool), you can enter a test waiver request.

1. Select the failed test.
2. Click the "Request waiver" ![waiver](waiver.png) action button.
3. Enter a markdown-formatted description of why the submission should be accepted regardless of the failure.

The text you supply will be exported along with any test report for that test, and may be evaluated during the certification approval process.

Note that running the test again will automatically clear the waiver request.

## Advanced Topics

### Version Checks

At startup, MCT attempts to inform you of available updates. To perform this check it must be able to reach github.com.

In networks where a proxy is required, MCT will attempt to autodetect it. If the proxy on your network cannot be autodetected, you can supply the proxy manually by launching MCT from the command line (updated with the correct proxy information):

```bash
java -Dhttps.proxyHost=proxyaddr.yourcorp.com -Dhttps.proxyPort=1234 -jar MopriaCertificationTool.jar
```

### Uninstalling

There is no uninstaller for the MCT program but it can be done in 2 steps.

1.  Delete the expanded [archive](#installation) containing the executable `.jar` file
2.  Delete the [data folder](#installation) generated by MCT

### Java 8 in Ubuntu

Later versions of Ubuntu Linux attempt to update the user to Java 11+ with corresponding and later OpenJFX versions. MCT is not compatible with later versions of Java or OpenJFX at this time.

To prevent Ubuntu from upgrading these packages, ensure `deb http://security.ubuntu.com/ubuntu bionic main universe` is temporarily in your `/etc/apt/sources.list`, and run:

```bash
sudo apt install openjfx=8u161-b12-1ubuntu2 libopenjfx-java=8u161-b12-1ubuntu2 libopenjfx-jni=8u161-b12-1ubuntu2 openjfx-source=8u161-b12-1ubuntu2
sudo apt-mark hold libopenjfx-java libopenjfx-jni openjfx openjfx-source
```

This will "hold back" updates for required packages.

### Running Multiple Instances

MCT is requires full control over its data directory and will only allow one instance at a time.

To run more than one copy of MCT simultaneously, each instance must have its own data directory. To do this, set the environment variable `MCT_USER_DATA` to another directory.

For example, in Linux:

```bash
MCT_USER_DATA=~/.mct2 java -jar MopriaCertificationTool.jar
```

### Handling a Memory Problem

MCT needs sufficient memory to run test suites. We recommend a minimum of 512 MB heap space or more to function smoothly.

### Memory problem- warning on Project page (or) If test execution fails due to the error “Java heap space”:

* Warning message: Upon launching MCT on the project page, If there is a warning with dialog message: "Only (N)MB available. Please increase available RAM to above 512 MB. See user guide for more".
							(or)

* Failure message: If any of the test execution fails due to the error message : "FAIL - Java heap space"

**Resolution**: If any of the above errors are observed then, Close MCT, open the command prompt and increase the maximum Java heap size using the below command at the start up.
```
java -Xmxng -jar MopriaCertificationTool.jar
```
where: 
“Xmx” specifies the maximum memory allocation pool for JVM.
"n" refers to the number.
“g” refers to Giga byte(s).

**Example**: ```java -Xmx1g -jar MopriaCertificationTool.jar ```, where 1 GB of RAM is allocated as the max heap size for JVM to function smoothly.

* Perform smaller operations (especially evidence import) in smaller batches.


### Submitting Issues

If the tool does not behave as expected, or test results are not as expected, try the following:

* Update to the latest Java 8 version.
* Move to a less-crowded network.
* Review test results carefully to determine if the tested device behavior is actually as-designed.
* Review `mct.log` in your application data folder (see Installation above).

If none of the above solve your issue, open a defect at https://github.com/MopriaAlliance/MopriaCertificationTool/issues (Mopria approval required for access). Be sure to include exported test results, screenshots, and/or `mct.log` as appropriate to describe the issue.

### Import evidence problem

Import evidence for multiple PTCL test sheets sometimes may cause few tests to fail, in that case please rescan the failed tests as there can be lot of varieties of MFP in market.

## BLE Enabled Printer advertising BLE Advertisement

1)	MCT Utility App setup on the Mobile device.
2)	View BLE Advertisement Data on the MCT Utility App.
3)	Export the BLE Advertisement data.
4)	Import the BLE Advertisement data to the MCT tool.
5)	Run the Tests.

### STEP1: MCT Utility App setup on the Mobile device:
1.	Get the MCT Utility Apk from the (MCT Package).
2.	Install the MCT Utility Apk on the Mobile device.
3.	Launch the MCT Utility app.
4.	Click on BLE Scanning

![utility](mct_utility_screen.png)

5.	Grant the Bluetooth and Location permissions.
6.	The BLE Advertisement data would be displayed on the Screen.

### STEP2: View BLE Advertisement Data on the MCT Utility App:
1.	Once the [STEP1] is completed, the user would be able to see the BLE Advertisement on the MCT Utility App.
2.	Sample BLE Advertisement Data on MCT Utility App Look like:
      
![utility](ble_advertisement.png)

### STEP3: Export the BLE Advertisement Data:
1.	Once the above BLE Advertisement data is displayed on the MCT Utility App.
2.	Tap on the data to export the data in JSON format Via (Outlook/Gmail/Others...)

![utility](ble_advertisement_sharing.png)


3.	After exporting the data one can view the JSON Format data by opening it, and
4.	The Sample JSON format Data Looks like:

![utility](sample_json.png)
      
### STEP4: Import the BLE Advertisement data to the MCT tool:
1.	Create a Project with “BLE v1.0” Specification.
2.	Click on “Setup” and then Click on “Run Selected Tests” (Play Button) on right bottom screen.

![utility](mct_setup_ble.png)

3.	Now a Popup would be appeared as “Please provide the file exported from the BLE Helper”.
4.	Click on the “+” Symbol in order to add the Exported JSON File in the [STEP3].

![utility](mct_test_ble.png)

5.	Go to the File Path where the JSON File is Present in the System and select it and click on open.
6.	Now the Selected File name will be appearing on the Popup.

![utility](mct_test_ble1.png)

7.	Click on “ok”.
8.	Now the BLE v1.0 Test cases would get populated.

### STEP5: Run the Tests:
1.	Once [STEP4] is successfully completed and BLE v1.0 Test cases are Populated.
2.	Now the user can click on “BLE v1.0” and then Click on “Run Selected Tests” (Play Button) on right bottom screen in order run all the tests together. (The user can even select individual tests and run them.)

![utility](mct_test_ble_run.png)

3.	After the test execution is completed. The user can see the test results as below and export them.
4.	Sample Screenshot look like:

![utility](mct_test_ble_run1.png)
      

## IPP over USB (Descriptors)

The MCT utility App can be also used to read the USB descriptor of the Device.
We can able to read below descriptors.
1.	Device descriptor.
2.	Configuration descriptor.
3.	Interface descriptor.
4.	End point descriptor.
5.	Class specific descriptor.

Note: The printer should be connected to the Android device via USB to read the USB descriptor details.

### Steps to check the descriptor info:
1.	Launch the MCT Utility App on an Android device (Accept EULA)
2.	Click on the "USB Descriptors" section to read the usb descriptor details. Make sure the printer is connected to the Android device via USB.
3.	After clicking on the USB Descriptors from Home Page you will be landed in USB descriptor screen where you see the status of the device whether it is attached or not. Also you can see the four buttons where you can click and see the respective descriptor details as a Table below the buttons. Refer below attached Screenshot.
4.	For e.g when you click the device button you can see a new table below those button with the descriptor field and value. You can also scroll through the Table if it contains more data.
5.	When there is an error you will get the Toast with the error message.

![utility](ipp_usb1.png)

![utility](ipp_usb2.png)


## Running IPP Over USB Certification tests with the help of MCT Utility

### Pre-requisites:
Printer(Supporting IPP Over USB), Android device, Printer USB cable, USB to type C OTG or USB to Micro USB (Based on Android device port)- Sample images are shown in the below steps.

* Prepare the printer
    * Turn Wireless OFF on the printer (verify it is off)
    * Turn Wi-Fi Direct and/or Wireless Direct OFF on the printer (verify it is off)
    * Unplug any Ethernet cable connections from the printer

Note: The devices on which MCT and MCT Utility are running should be connected to the same network.

### Steps:
1.	Run the IPP Over USB test on MCT.
2.	Install MCT Utility on an Android device.
3.	Start and discover the MCT Published service.
4.	Connect the Android device to the Printer & Run the test.
5.	Transfer the results to MCT.

### Step 1: Run the IPP Over USB test on MCT
1.	Create a project on MCT by selecting the printer and printer spec.
2.	Complete the setup and all the PTCL tests gets populated.
3.	All the IPP Over USB PTCL tests can be seen under "IPP Over USB" section.
4.	Select the test and click on Run button.

![utility](IPPOverUSB1.png)

5.	Follow the instructions shown on the dialog box.

![utility](IPPOverUSB2.png)

### Step 2: Install MCT Utility on an Android device:
1.	Get the MCT Utility apk from the same MCT Package, where MopriaCertificationTool.jar file is available.
2.	Install the MCT Utility application on an Android device (Recommended Android 12).
3.	Launch the MCT Utility application.
4.	Accept EULA (Terms & Conditions).
5.	Select IPP Over USB section.

![utility](IPPOverUSB3.png)


### Step 3: Start and discover the MCT Published service
1.	Start the discovery by enabling Start/Stop button.

![utility](IPPOverUSB4.png)

2.	MCT published service would get discovered.
Example: MCT Test Server139 (In this example), this will differ based on their IP Address.

![utility](IPPOverUSB5.png)

3.	Tap on the discovered service to get the test case details from MCT.
4.	The PTCL test case would get populated. (If not populated, If an empty screen is seen, then check troubleshooting steps below)

### Step 4: Connect the Android device to the Printer and Run the test
1.	Connect One end of the Printer USB cable to the printer

![utility](IPPOverUSB6.png)

2.	Connect the other end of the USB cable to OTG
3.	Connect the OTG Type C or Micro USB (Based on the device) to the Android device port.

![utility](IPPOverUSB7.png)

4.	A permission dialog box would pop up requesting access to the printer >> Click on "OK"
5.	Tap on Run button available on the test case to initiate execution.

![utility](IPPOverUSB8.png)

### Step 5: Transfer the results to MCT
1.	Once the test execution is completed and Print output is successful.
2.	"TRANSFER RESULTS TO MCT" button gets enabled.
3.	Tap on it to transfer the results back to MCT for evaluation.

![utility](IPPOverUSB9.png)

4.	If there is no problem, then the test will PASS else the test might fail, Check for the failure reason.
5.	Click on "OK" button on the test status dialog box to land back on to the home page.
6.	Click on "OK" button on the MCT instruction dialog box to see the results on MCT.
7.	Import the additional evidence required (If any).


### Troubleshooting:
If there is any issue in the discovery of MCT published service or PTCL test not getting populated on MCT Utility, It could be probably because of some network configuration related issue.

In some cases this might work after switching the network profile on the system on which MCT is running from "Public" to "Private" or from "Private" to "Public" as shown below. And then try re-running the test again from MCT and check if the issue is resolved.

![utility](IPPOverUSB10.png)

## Running Cloud Hardware Printer Certification Tests

Cloud Hardware Printer tests implement the Mopria Alliance Cloud Printer Test Case Library (CPTCL). These tests allow you to
certify a Mopria-compatible cloud hardware printer.

### Steps:

1. Create a Cloud Hardware Printer project.
2. Complete the Cloud Hardware Printer Registration.
3. Running CPTCL test cases.

### Step 1: Create a Cloud Hardware Printer project:

1. Launch MCT.
2. Click on the Create new project(+) icon on project page.
3. Expand the drop down list and select "Add Cloud Printer".

![utility](CloudHW1.png)

4. Enter the "Printer Name" and "Make & Model".
5. Click on "ADD" button.
6. Select "Cloud Printer v1.2" from the list.

![utility](CloudHW2.png)

7. Click on "CREATE" button.
8. Open the project by clicking on "Cloud Printer v1.2" on the project page.

![utility](CloudHW3.png)


### Step 2: Complete the Cloud Hardware Printer Registration:

1. Select "Register Printer with Cloud" test under setup.
2. Click on the "Run" button.
3. Note the Registration Server URL shown on the dialog box or the Well known mopria configuration URL.
	Example: http://192.168.xxx.xxx:8080/.well-known/mopria-configuration

![utility](CloudHW4.png)

4. Enter any one of the URL on the Cloud Hardware Printer & Initiate the Registration request.
5. Printer keeps polling, to check the status of registration request at regular intervals.
6. Printer would be waiting for the cloud administrator(MCT) to approve the registration request.
7. Registration would be completed once the request is approved.
8. Run "Authenticate Printer with Cloud" test.

![utility](CloudHW4_1.png)

9. Initiate Authentication token request on the printer.
10. An access token gets generated once the printer authenticates with the cloud service.
11. The cloud hardware printer will use this access token for all further communications with IPP Infra.
12. MCT would now populate all the CPTCL test cases based on the capabilities.


### Step 3: Running CPTCL test cases:

1. Select the CPTCL test case and click on "Run" button.
2. Follow the instructions given on the dialog box.
3. Test steps and Evaluation criteria would be available for each test on the left side panel.
4. MCT would evaluate and Pass/Fail the test case based on the criteria.
5. Logs of each test can be seen in the report view at the bottom of the MCT screen.


## Cloud Client

TBD

## Cloud Cloud

TBD
