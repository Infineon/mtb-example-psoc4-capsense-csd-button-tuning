# PSoC 4: CapSense CSD Button Tuning

This code example demonstrates how to manually tune a self capacitance (CSD)-based button widget in PSoC® 4 devices using the CapSense® Tuner.

## Overview

This document provides the following:

   1. A high-level overview of the CSD widget tuning flow
   2. An example to show how to manually tune a CSD button widget
   3. Details on how to use the CapSense Tuner to monitor the CapSense raw data and fine-tune the CSD button for optimum performance such as reliability, power consumption, and response time

The code scans a single button widget using the CSD sensing method. It also sends the CapSense raw data over an I2C interface to the on-board KitProg which then could be received using the CapSense Tuner. The tuning of the CSD button is performed with the help of data that is received in the CapSense Tuner. The successful tuning of the button is indicated by an user LED in the prototyping kit, the LED is turned ON when the finger touches the button and turned OFF when the finger is removed from the button.

[Provide feedback on this Code Example.](https://cypress.co1.qualtrics.com/jfe/form/SV_1NTns53sK2yiljn?Q_EED=eyJVbmlxdWUgRG9jIElkIjoiQ0UyMzA5MjYiLCJTcGVjIE51bWJlciI6IjAwMi0zMDkyNiIsIkRvYyBUaXRsZSI6IlBTb0MgNDogQ2FwU2Vuc2UgQ1NEIEJ1dHRvbiBUdW5pbmciLCJyaWQiOiJibHBkIiwiRG9jIHZlcnNpb24iOiIxLjAuMCIsIkRvYyBMYW5ndWFnZSI6IkVuZ2xpc2giLCJEb2MgRGl2aXNpb24iOiJNQ0QiLCJEb2MgQlUiOiJJQ1ciLCJEb2MgRmFtaWx5IjoiUFNPQyJ9)

## Requirements

- [ModusToolbox® software](https://www.cypress.com/products/modustoolbox-software-environment) v2.2

  **Note:** This code example version requires ModusToolbox software version 2.2 or later and is not backward compatible with v2.1 or older versions.

- Board Support Package (BSP) minimum required version: 1.0.0
- Programming Language: C
- Associated Parts: [PSoC 4000S](https://www.cypress.com/documentation/datasheets/psoc-4-psoc-4000s-family-datasheet-programmable-system-chip-psoc), [PSoC 4100S](https://www.cypress.com/documentation/datasheets/psoc-4-psoc-4100s-family-datasheet-programmable-system-chip-psoc), and [PSoC 4100S Plus](https://www.cypress.com/documentation/datasheets/psoc-4-psoc-4100s-plus-datasheet-programmable-system-chip-psoc)

## Supported Toolchains (make variable 'TOOLCHAIN')

- GNU Arm® Embedded Compiler v9.3.1 (`GCC_ARM`) - Default value of `TOOLCHAIN`
- Arm compiler v6.11 (`ARM`)
- IAR C/C++ compiler v8.42.2 (`IAR`)

## Supported Kits (make variable 'TARGET')

- [PSoC 4100S Plus Prototyping Kit](https://www.cypress.com/CY8CKIT-149) (`CY8CKIT-149`) - Default target
- [PSoC 4000S CapSense Prototyping Kit](https://www.cypress.com/CY8CKIT-145) (`CY8CKIT-145`)
- [PSoC 4100S CapSense Pioneer Kit](https://www.cypress.com/CY8CKIT-041-41xx) (`CY8CKIT-041-41xx`)

## Hardware Setup

This example uses the board's default configuration. See the kit user guide to ensure that the board is configured correctly.

**Note:** PSoC 4 kits ship with KitProg2 installed. ModusToolbox software requires KitProg3. Before using this code example, make sure that the board is upgraded to KitProg3. The tool and instructions are available in the [Firmware Loader](https://github.com/cypresssemiconductorco/Firmware-loader) GitHub repository. If you do not upgrade, you will see an error like "unable to find CMSIS-DAP device" or "KitProg firmware is out of date".

## Software Setup

This example requires no additional software or tools.

## Using the Code Example

### In Eclipse IDE for ModusToolbox:

1. Click the **New Application** link in the **Quick Panel** (or, use **File** > **New** > **ModusToolbox Application**).  This launches the [Project Creator](http://www.cypress.com/ModusToolboxProjectCreator) tool.

2. Pick a kit supported by the code example from the list shown in the **Project Creator - Choose Board Support Package (BSP)** dialog.

   When you select a supported kit, the example is reconfigured automatically to work with the kit. To work with a different supported kit later, use the [Library Manager](https://www.cypress.com/ModusToolboxLibraryManager) to choose the BSP for the supported kit. You can use the Library Manager to select or update the BSP and firmware libraries used in this application. To access the Library Manager, click the link from the **Quick Panel**.

   You can also just start the application creation process again and select a different kit.

   If you want to use the application for a kit not listed here, you may need to update the source files. If the kit does not have the required resources, the application may not work.

3. In the **Project Creator - Select Application** dialog, choose the example by enabling the checkbox.

4. Optionally, change the suggested **New Application Name**.

5. Enter the local path in the **Application(s) Root Path** field to indicate where the application needs to be created.

   Applications that can share libraries can be placed in the same root path.

6. Click **Create** to complete the application creation process.

For more details, see the [Eclipse IDE for ModusToolbox User Guide](https://www.cypress.com/MTBEclipseIDEUserGuide) (locally available at *{ModusToolbox install directory}/ide_{version}/docs/mt_ide_user_guide.pdf*).

### In Command-line Interface (CLI):

ModusToolbox provides the Project Creator as both a GUI tool and a command line tool to easily create one or more ModusToolbox applications. See the "Project Creator Tools" section of the [ModusToolbox User Guide](https://www.cypress.com/ModusToolboxUserGuide) for more details.

Alternatively, you can manually create the application using the following steps:

1. Download and unzip this repository onto your local machine, or clone the repository.

2. Open a CLI terminal and navigate to the application folder.

   On Linux and macOS, you can use any terminal application. On Windows, open the **modus-shell** app from the Start menu.

   **Note:** The cloned application contains a default BSP file (*TARGET_xxx.mtb*) in the *deps* folder. Use the [Library Manager](https://www.cypress.com/ModusToolboxLibraryManager) (`make modlibs` command) to select and download a different BSP file, if required. If the selected kit does not have the required resources or is not [supported](#supported-kits-make-variable-target), the application may not work.

3. Import the required libraries by executing the `make getlibs` command.

Various CLI tools include a `-h` option that prints help information to the terminal screen about that tool. For more details, see the [ModusToolbox User Guide](https://www.cypress.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox install directory}/docs_{version}/mtb_user_guide.pdf*).


### In Third-party IDEs:

1. Follow the instructions from the [CLI](#in-command-line-interface-cli) section to create the application, and import the libraries using the `make getlibs` command.

2. Export the application to a supported IDE using the `make <ide>` command.

    For a list of supported IDEs and more details, see the "Exporting to IDEs" section of the [ModusToolbox User Guide](https://www.cypress.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox install directory}/docs_{version}/mtb_user_guide.pdf*.

3. Follow the instructions displayed in the terminal to create or import the application as an IDE project.


## Tuning Flow Summary

The following flowchart gives a high-level summary on how to tune a CSD-based CapSense button in PSoC 4 devices. See the “Manual Tuning” section in [AN85951 – PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/documentation/application-notes/an85951-psoc-4-and-psoc-6-mcu-capsense-design-guide) for information on the hardware and threshold parameters that determines the CapSense touch performance.

**Figure 1. High-Level Overview of CSD Button Tuning**

<img src="images/tuning-flowchart.png" alt="Figure 1" width="300"/>

## Operation

This process involves the following stages:

- [Stage 1: Set Initial Hardware Parameters](#stage-1-set-initial-hardware-parameters)

- [Stage 2: Measure SNR](#stage-2-measure-snr)

- [Stage 3: Modify Hardware Parameters or Adjust Filter Settings](#stage-3-modify-hardware-parameters-or-adjust-filter-settings)

- [Stage 4: Set the Software Threshold Parameters Using CapSense Tuner](#stage-4-set-the-software-threshold-parameters-using-capsense-tuner)



### Stage 1: Set Initial Hardware Parameters
------------

Connect the board to your PC using the provided micro USB cable through the KitProg3 USB connector, and launch the CapSense Configurator.

See the "Launch the CapSense Configurator" section from the [ModusToolbox CapSense Configurator Guide](https://www.cypress.com/file/492896/download).

In the **Basics** tab, you will find a single widget ‘Button0’ configured as a CSD button and the CSD Tuning method selected as *Manual Tuning*.

**Figure 2. CapSense Configurator - General Settings**

<img src="images/basic-csd-settings.png" alt="Figure 2" width="600"/>


#### 1. Set the initial general parameters.

   1. Navigate to the **Advanced** tab, and then select the **General settings** sub-tab. Leave all the filter parameters at their default settings. Filters will be enabled depending on the SNR and system time requirements.

   2. Select **Enable self-test library** to perform sensor capacitance measurement using the CapSense Middleware APIs in the firmware. See [Step 3. Set the sense clock divider and sense clock source](#3-set-the-sense-clock-divider-and-sense-clock-source).

      **Figure 3. CapSense Configurator - General Settings**

      <img src="images/advanced-general-settings.png" alt="Figure 3" width="600"/>


#### 2. Set the initial hardware parameters.

   1. Select the **Advanced** tab and then select the **CSD Settings** sub-tab.

   2. Configure the parameters as Table 1 and Figure 4 show.

      **Table 1. Advanced Tab - CSD Settings**

      | Parameter | Value | Remarks |
      | --- | --- | --- |
      | Modulator clock divider | 1 (To obtain the maximum allowed by the selected device) | A higher modulator clock frequency reduces flat spots, and increases measurement accuracy and sensitivity. It also reduces the sensor scan time, which results in lower power consumption. Thus, it is recommended to select the highest possible available modulator clock frequency. |
      | Inactive sensor connection | Ground (default) | Inactive sensors are connected to ground to provide good shielding from noise sources. Use inactive sensor connection as shield for liquid-tolerant designs, if your design contains a proximity sensor, or if the adjacent sensors are being used to reduce Cp of sensors.
      | IDAC sensing configuration | IDAC sourcing (default) | Choose IDAC sourcing mode because it is more susceptible to VDD noise compared to IDAC sinking mode. However, if you have clean/noise-free VDD, you may choose IDAC sinking mode for a higher SNR.
      | Enable IDAC auto-calibration | Checked | Enabling auto-calibration allows the device to automatically choose the optimal IDAC value such that it calibrates the raw count of the sensor to 85 percent of its maximum value.
      | Enable compensation IDAC | Checked | Enabling the compensation IDAC selects the dual-IDAC mode operation of the CSD. Dual-IDAC mode gives higher signal values compared to single-IDAC mode for fixed values of CapSense parameters.
      | Enable shield electrode | Unchecked (default) |Enable shield if your design requires a large proximity sensing distance, liquid tolerance, or if the shield is being used to reduce the Cp of sensors. Before enabling this option, ensure that the PCB has a shield electrode or hatched pattern connected to the device pin.

      **Figure 4. CapSense Configurator - Advanced CSD Settings**

      <img src="images/advanced-csd-settings.png" alt="Figure 4" width="600"/>


      **Note:** You can change the modulator clock frequency to 48MHz only after changing the IMO clock frequency to 48 MHz. To do this, go to the **System Tab** in the **Device Configurator** tool, select **System Clocks** > **Input** > **IMO**. Select 48 from the **Frequency(MHz)** drop-down list.

#### 3. Set the sense clock divider and sense clock source.

   1. Navigate to the **Advanced** tab, and then select the **Widget Details** window.

   2. Set the sense clock divider and sense clock source per the following guidelines:

      - Sense clock divider

         The CapSense Configurator in ModusToolbox allows you to set the sense clock frequency in terms of the sense clock divider as shown in Equation 1:

         **Equation 1. Sense Clock Divider**

         <img src="images/scd-equation.png" alt="Equation 1" width="250"/>

         Set the maximum possible sense clock frequency which will completely charge and discharge the sensor parasitic capacitance. Verify the charging and discharging of the sensor waveform with an oscilloscope by probing the sensor using an active probe.

         Use Equation 2 to compute the maximum sense clock frequency using the Cp of the sensor and the total series resistance R<sub>Series</sub>.     Note that the CapSense Configurator allows a maximum sense clock frequency of 6 MHz; therefore, you cannot set it above 6 MHz.

         **Equation 2. Maximum Sense Clock Frequency**

         <img src="images/fsw-equation.png" alt="Equation 2" width="250"/>

         Cp is the parasitic capacitance of the sensor electrode.

         R<sub>SeriesTotal</sub> is the total series resistance, which includes the 500-ohm pin internal resistance, the external series resistance (in CY8CKIT-149, it is 2 kilo-ohm), and the trace resistance. Include the trace resistance if high-resistive material such as ITO, or conductive ink is used. The external resistor is connected between the sensor pad and the device pin to reduce the radiated emission and for ESD protection.

         Use one of the following options to determine the Cp of the sensor:

         - [Option 1: Using BIST API in CapSense Middleware](#option-1-using-bist-api-in-capsense-middleware)

         - [Option 2: Using LCR Meter](#option-2-using-lcr-meter)

         #### **Option 1: Using BIST API in CapSense Middleware**

         While using this procedure, ensure that you have enabled the **Enable self-test library** option in the CapSense Configurator. After you obtained the Cp value, you can disable this option.

         1. Estimate the Cp of the sensor electrode using the `Cy_CapSense_MeasureCapacitanceSensor()` function in firmware. The measured capacitance value is in femtofarad (fF).

         2. Program the board in Debug mode.

            In the IDE, use the **\<Application Name> Debug (KitProg3)** configuration in the **Quick Panel**.

            For more details, see the "Program and Debug" section in the Eclipse IDE for ModusToolbox User Guide: *{ModusToolbox install directory}/ide_{version}/docs/mt_ide_user_guide.pdf*.

         3. Place a breakpoint after the capacitance measurement.

         4. In the **Expressions** window, add the Cp measurement variables (**sensor_cp**).

            The status of the measurement can also be read through the return value of the function in the **Expressions** window.

         5. Click the **Resume** button (green arrow) to reach the breakpoint.

            Note that the function return value reads `CY_CAPSENSE_BIST_SUCCESS_E` and the measurement variables provide the capacitance value of the sensor elements in **femtofarad** as shown in Figure 5.

         6. Click the **Terminate** button (red box) to exit Debug mode.

            **Figure 5. Cp Measurement Using BIST**

            <img src="images/debug-cp-measure.png" alt="Figure 5" />


         #### **Option 2: Using LCR Meter**

         Measure the Cp of the sensor electrode of the button using an LCR meter. The Cp should be measured between the sensor electrode (sensor pin) and the device ground.

         **Table 2. Sense Clock Divider Settings in CapSense Configurator**

         |Development Kit | Cp of Sensor Electrode (pF)| R<sub>SeriesTotal</sub> (ohm) |  Maximum Sense Clock frequency (kHz)| Sense Clock Divider Setting in Configurator|
         | --- | --- | --- | --- | --- |
         |CY8CKIT-149 (Pin P4.5) | 9 | 2.5K | 4444 | 11 |
         |CY8CKIT-145 (Pin P1.5) | 9 | 1060 | 6000 | 8 |
         |CY8CKIT-041-41xx (Pin P0.1) | 44 | 1060 | 2144 | 23 |

      - Sense clock source

         Select **Auto** as the sense clock source to automatically choose the correct Spread Spectrum (SSC) or PRS clock as the sense clock source to deal with EMI/EMC or Flat Spots issues.

   3. Ensure that the following conditions are also satisfied when selecting the sense clock frequency and sense clock source:

      - The auto-calibrated IDAC value should lie in the mid-range (for example, 18-110) for the selected F<sub>sw</sub>. If the auto-calibrated IDAC value lies out of the recommended range, F<sub>sw</sub> is tuned such that IDAC falls within the recommended range. See [Ensure that the auto-calibrated IDAC is within the recommended range](#2-ensure-that-the-auto-calibrated-idac-is-within-the-recommended-range).

      - If you are explicitly using the PRS or SSCx clock source, ensure that you select the sense clock frequency that meets the conditions mentioned in the [ModusToolbox CapSense Configurator Guide](https://www.cypress.com/file/492896/download).

### 4. Set the initial scan resolution.

   Set the scan resolution to an initial low value of 10. This will be modified in [Stage 3: Modify Hardware Parameters or Adjust Filter Settings](#stage-3-modify-hardware-parameters-or-adjust-filter-settings) based on the Signal-to-Noise ratio (SNR) and system timing requirements.

   **Figure 6. Advanced Tab - Widget Details Sub-Tab**

   <img src="images/advanced-widget-settings.png" alt="Figure 6" width="600"/>

#### 5. Program the board.

   - **Using Eclipse IDE for ModusToolbox:**

      1. Select the application project in the Project Explorer.

      2. In the **Quick Panel**, scroll down, and click **\<Application Name> Program (KitProg3_MiniProg4)**.


   - **Using CLI:**

     From the terminal, execute the `make program` command to build and program the application using the default toolchain to the default target. You can specify a target and toolchain manually:
      ```
      make program TARGET=<BSP> TOOLCHAIN=<toolchain>
      ```

      Example:
      ```
      make program TARGET=CY8CKIT-149 TOOLCHAIN=GCC_ARM
      ```

### Stage 2: Measure SNR
------------------

#### 1. Set up the CapSense Tuner to view the sensor data.

   1. Launch the CapSense Tuner.

      See the "Launch the CapSense Tuner" section from the [ModusToolbox CapSense Tuner Guide](https://www.cypress.com/file/504381/download).

   2. Go to **Tools** > **Tuner Communication Setup** and set the parameters as Figure 7 shows. Click **OK**.

      **Figure 7. Tuner Communication Setup**

      <img src="images/tuner-comm-setup.png" alt="Figure 7" width="600"/>

   3. Click **Connect**.

      **Figure 8. CapSense Tuner Window**

      <img src="images/tuner-connect.png" alt="Figure 8" width="250"/>

   4. Click **Start**.

      **Figure 9. CapSense Tuner Start**

      <img src="images/tuner-start.png" alt="Figure 9" width="250"/>

      The **Widget/Sensor Parameters** tab gets updated with the parameters configured in the **CapSense Configurator** window.

      **Figure 10. CapSense Tuner Window**

      <img src="images/tuner-widget-settings.png" alt="Figure 10" width="600"/>

   5. Select the *Button0* check box and *Synchronized* under **Read mode**, and then navigate to the **Graph View** as Figure 11 shows.

      The graph view displays the raw counts and baseline for *Button0* in the **Sensor Data** window. Ensure that the **Raw Counts** and **Baseline** checkboxes are selected to view the sensor data.

      **Figure 11. CapSense Tuner Graph View**

      <img src="images/tuner-graph-view.png" alt="Figure 11" width="600"/>

      **Note:** At this point, when the configured button is touched, you may or may not notice the touch signal in the **Sensor Signal** graph. The sensor may false-trigger which can be seen in the touch status going from 0 to 1 in the **Status** window.

#### 2. Ensure that the auto-calibrated IDAC is within the recommended range.

   As discussed in previous section, the sense clock frequency will be tuned to bring IDAC code to the recommended range in this step. Click on **Button0** in the **Widget Explorer** to view the IDAC value in the sensor parameters window as shown in Figure 12. If the IDAC value is within the range (18 to 110), this step is not required.

   Increasing the sense clock divider will decrease the IDAC value for a fixed IDAC gain and calibration percent and vice versa.

   **Figure 12. IDAC Value**

   <img src="images/tuner-idac-setting.png" alt="Figure 12" width="600"/>


#### 3. Fine-tune the sense clock frequency to bring the IDAC within range.

   1. Click **Button0** in the Widget Explorer.

   2. Increase or decrease the sense clock divider in the **Widget Hardware Parameters** window.

   3. Click the **To Device** button to apply the changes to the device as shown in Figure 13.

      **Figure 13. Apply Changes to Device**

      <img src="images/tuner-apply-to-device.png" alt="Figure 13`" width="600"/>

   4. Observe the Modulator IDAC and Compensation IDAC value in the **Sensor Parameters** section of the **Widget/Sensor Parameters** window.

   5. Repeat steps 1 to 4 until you obtain the IDAC value in the range of 18 to 110.

      **Note:** As Figure 13 shows, IDAC values are already in the recommended range. Therefore, you can leave the sense clock divider to the value as shown in Table 2.

#### 4. Measure SNR.

   1. Switch to the **SNR Measurement** tab, select the **Button0** sensor, and then click **Acquire Noise** as Figure 14 shows.

      **Figure 14. CapSense Tuner - SNR Measurement: Acquire Noise**

      <img src="images/tuner-acquire-noise.png" alt="Figure 14" width="600"/>

   2. Once the noise is acquired, touch the button on the kit, and then click **Acquire Signal**. Ensure that the finger remains on the button as long as the signal acquisition is in progress.

      The calculated SNR on this button is displayed, as Figure 15 shows. Based on your end-system design, test with a finger that matches the size of your normal use case. Typically, finger size targets are ~8 to 9 mm.

      **Figure 15. CapSense Tuner - SNR Measurement: Acquire Signal**

      <img src="images/tuner-acquire-signal.png" alt="Figure 15" width="600"/>

### Stage 3. Modify Hardware Parameters or Adjust Filter Settings
---------------

Skip this stage if the following conditions are met:
- Measured SNR from the previous step is greater than 5:1
- Signal count is greater than 50
- Response time requirement are met

If the SNR is less than 5:1, do the following to increase the touch performance. The main parameters that influence SNR are **scan resolution** and **filters**.

It is best to find a balance between the resolution and filters to achieve proper overall tuning. If your system is very noisy (counts >20), you may want to prioritize adding a filter. On the other hand, if your system is relatively noise-free (counts <10), you should focus on resolution, as this will increase the sensitivity and signal of your system.

#### **Scan Resolution**

Scan resolution can be increased to increase the signal at a disproportionate rate to noise to improve overall SNR. Increasing the resolution adds to the overall hardware scan time based on Equation 3.

**Equation 3. Scan Time**

<img src="images/scantime-equation.png" alt="Equation 3" width="200"/>

Do the following to update the scan resolution:

1. Update the scan resolution (N) directly in the **Widget/Sensor Parameters** tab of the CapSense Tuner.

2. Increase the scan resolution by one and repeat steps in [Measure SNR](#4-measure-snr) until the minimum SNR of 5:1 and at least a signal count greater than 50 are achieved.


#### **Firmware Filters**

Firmware filters help to reduce noise without increasing the signal. Based on your noise type, you can enable a filter to improve SNR. Each filter will add additional processing time as well as memory use. If your system is very noisy (counts > 20), add a filter.

1. Open CapSense Configurator and select the appropriate filter as shown in Figure 16.

2. Reprogram the device to update filter settings.

   **Note:** Add the filter based on the type of noise in your measurements. See [ModusToolbox CapSense Configurator Guide](https://www.cypress.com/file/492896/download) for details.

   **Figure 16. Filter Settings in CapSense Configurator**

   <img src="images/advanced-filter-settings.png" alt="Figure 16" width="600"/>

   **Note:** The current example has SNR around 10:1; therefore, filters are not enabled.

   After setting the scan resolution and filter settings, check the total scan time based on Equation 3 to determine whether system requirements are met. This timing will impact the response time and is a crucial factor in the overall power consumption of the device in CapSense applications.

   If the total sensor scan time meets your requirements, go to the next step.

   If not, adjust the tuning to speed up the scan time (decrease the scan resolution or increasing the F<sub>MOD</sub>). If the SNR is greater than 10 on any sensor, lower the scan resolution or remove filters to decrease the scan time, but keep the SNR greater than 5:1. It is best to find a balance between scan resolution and filters to achieve proper overall tuning.

3. Use Table 3 to set the hardware tuning parameters to achieve 5:1 SNR.

   **Table 3. Final Hardware Tuning Parameters to Achieve 5:1 SNR (With Firmware Filters Disabled)**

   |Development Kit | Sense Clock Divider Setting in Configurator| Scan resolution |
   | --- | --- | --- |
   |CY8CKIT-149| 11| 10 |
   |CY8CKIT-145| 8| 9 |
   |CY8CKIT-041-41xx| 23| 11 |


### Stage 4. Set the Software Threshold Parameters Using CapSense Tuner
---------------------

After you have confirmed that your design meets the timing parameters, and the SNR is greater than 5:1, set your threshold parameters as follows:

1. Switch to the **Graph View** tab and select **Button0** (CSD).

2. Touch the sensor and monitor the touch signal in the **Sensor Signal** graph.

   The **Sensor Signal** graph should show the signal as Figure 17 shows.

   Ensure that you observe the difference count (that is, the signal output) in the **Graph View** tab in Figure 17, not the raw count output for setting these thresholds. Based on your end system design, test the signal with a finger that matches the size of your normal use case. Typically, finger size targets are ~8 to 9 mm. Consider testing with smaller sizes that should be rejected by the system to ensure that they do not reach the finger threshold. Also ensure to ground the metal finger.

   **Figure 17. Sensor Signal when the Sensor Is Touched**

   <img src="images/tuner-diff-signal.png" alt="Figure 17" width="600"/>

3. When the signal is measured, set the thresholds according to the following recommendations:

   * Finger Threshold = 80 percent of signal
   * Noise Threshold = 40 percent of signal
   * Negative Noise Threshold = 40 percent of signal
   * Hysteresis = 10 percent of signal
   * Debounce = 3

4. Set the threshold parameters in the **Widget/Sensor Parameters** section of the CapSense Tuner, as Figure 18 shows:

   **Figure 18. Widget Threshold Parameters**

   <img src="images/tuner-threshold-settings.png" alt="Figure 18" width="300"/>

   See Table 4 to set the threshold parameters in the CapSense Tuner for different development kits.

   **Table 4. Threshold Parameters for Different Development Kits**

   | Development Kit | Difference Counts | Finger Threshold | Noise Threshold | Negative Noise Threshold | Hysteresis | Low Baseline Reset | Debounce |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   |CY8CKIT-149| 60 | 48 | 24 | 24 | 6 | 30 | 3 |
   |CY8CKIT-145-40XX| 80 | 64 | 32 | 32 | 8 | 30 | 3 |
   |CY8CKIT-041-41XX| 60 | 48 | 24 | 24 | 6 | 30 | 3 |


5. Apply the settings to the device and to the project by clicking on **To Device** icon and then **To Project** icon as Figure 19 shows, and close the tuner.

   **Figure 19. Apply to Project setting**

   <img src="images/tuner-apply-settings.png" alt="Figure 19" width="600"/>

   If your sensor is tuned correctly, you will observe the touch status go from 0 to 1 in the **Status** sub-window of the **Graph View** window as Figure 20 shows. The successful tuning of the button is also indicated by a user LED in the prototyping kit; the LED is turned ON when the finger touches the button and turned OFF when the finger is removed from the button.

   **Figure 20. Sensor Status in CapSense Tuner**

   <img src="images/tuner-status.png" alt="Figure 20" width="600"/>

6. Launch CapSense Configurator. You should now see all the changes that you made in the CapSense Tuner reflected in the CapSense Configurator.

## Debugging

You can debug the example to step through the code. In the IDE, use the **\<Application Name> Debug (KitProg3_MiniProg4)** configuration in the **Quick Panel**. For more details, see the "Program and Debug" section in the [Eclipse IDE for ModusToolbox User Guide](https://www.cypress.com/MTBEclipseIDEUserGuide).


## Design and Implementation

The project contains a single button widget configured in CSD sensing mode. See the "CapSense CSD Sensing Method" section in the [AN85951 – PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/documentation/application-notes/an85951-psoc-4-and-psoc-6-mcu-capsense-design-guide)  for details on CapSense CSD sensing mode. See the [Operation](#operation) section for step-by-step instructions to configure the other settings of the CapSense Configurator.

The project uses the [CapSense Middleware](https://github.com/cypresssemiconductorco/capsense) (see ModusToolbox User Guide for more details on selecting a middleware). See [AN85951 – PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/documentation/application-notes/an85951-psoc-4-and-psoc-6-mcu-capsense-design-guide) for more details on CapSense features and usage.

The [ModusToolbox software](https://www.cypress.com/products/modustoolbox-software-environment) provides a GUI-based tuner application for debugging and tuning the CapSense system. The CapSense Tuner application works with the EZI2C and UART communication interfaces. This project has an SCB block configured in EZI2C mode to establish communication with the on-board KitProg, which in turn enables reading the CapSense raw data by the CapSense Tuner. See [EZI2C - Peripheral Settings](#resources-and-settings).

The CapSense data structure that contains the CapSense raw data is exposed to the CapSense Tuner by setting up the I2C communication data buffer with the CapSense data structure. This enables the Tuner to access the CapSense raw data for tuning and debugging CapSense data.

The successful tuning of the button is indicated by a user LED in the prototyping kit; the LED is turned ON when the finger touches the button and turned OFF when the finger is removed from the button. Figure 21 shows the firmware flow for this code example.

**Figure 21. Firmware Design**

<img src="images/firmware-flowchart.png" alt="Figure 21" width="300"/>

### Resources and Settings

**Figure 22. EZI2C Settings**

<img src="images/ezi2c-config.png" alt="Figure 22" width="600"/>

**Table 5. Application Resources**

| Resource  |  Alias/Object     |    Purpose     |
| :------- | :------------    | :------------ |
| SCB (I2C) (PDL) | CYBSP_EZI2C          | EZI2C slave driver to communicate with the CapSense Tuner |
| CapSense | CYBSP_CSD | CapSense driver to interact with the CSD hardware and interface CapSense sensors |
| Digital Pin | CYBSP_USER_LED2 | To show the button operation|

## Related Resources

| Application Notes                                            |                                                             |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [AN79953](https://www.cypress.com/AN79953) – Getting Started with PSoC 4 | Describes PSoC 4 devices and how to build your first application with PSoC Creator |
| [AN85951](https://www.cypress.com/AN85951) – PSoC® 4 and PSoC® 6 MCU CapSense® Design Guide  | Describes how to design capacitive touch sensing applications with the PSoC 6 families of devices |
| **Device Documentation**                                               |
| [PSoC 4 Datasheets](https://www.cypress.com/search/all/PSOC%204%20datasheets?sort_by=search_api_relevance&f%5B0%5D=meta_type%3Atechnical_documents) | [PSoC 4 Technical Reference Manuals](https://www.cypress.com/search/all/PSoC%204%20Technical%20Reference%20Manual?sort_by=search_api_relevance&f%5B0%5D=meta_type%3Atechnical_documents) |
| **Code Examples**
| [Using ModusToolbox](https://github.com/cypresssemiconductorco/Code-Examples-for-ModusToolbox-Software) |
| **Development Kits**                                         | Buy at www.cypress.com                                       |
| [CY8CKIT-149](https://www.cypress.com/CY8CKIT-149) PSoC® 4100S Plus Prototyping Kit | [CY8CKIT-145](https://www.cypress.com/CY8CKIT-145) PSoC® 4000S CapSense Prototyping Kit |
|[CY8CKIT-041-41xx](https://www.cypress.com/CY8CKIT-149) PSoC® 4100S CapSense Pioneer Kit| |
 **Libraries**                               |                                                           |
| PSoC 4 Peripheral Driver Library (PDL) and docs  | [mtb-pdl-cat2](https://github.com/cypresssemiconductorco/mtb-pdl-cat2) on GitHub |
Cypress Hardware Abstraction Layer (HAL) Library and docs     | [mtb-hal-cat2](https://github.com/cypresssemiconductorco/mtb-hal-cat2) on GitHub |
| **Middleware**
| CapSense® library and docs                                    | [capsense](https://github.com/cypresssemiconductorco/capsense) on GitHub ||
**Tools**                                                    |                                                              |
| [Eclipse IDE for ModusToolbox](https://www.cypress.com/modustoolbox)     | The cross-platform, Eclipse-based IDE for IoT designers that supports application configuration and development targeting converged MCU and wireless systems.             |
| [PSoC Creator™](https://www.cypress.com/products/psoc-creator-integrated-design-environment-ide) | The Cypress IDE for PSoC and FM0+ MCU development.            |


## Other Resources

Cypress provides a wealth of data at www.cypress.com to help you select the right device, and quickly and effectively integrate it into your design.

## Document History

Document Title: *CE230926* - *PSoC 4: CapSense CSD Button Tuning*

| Version | Description of Change |
| ------- | --------------------- |
| 1.0.0   | New code example.  <br /> This version is not backward compatible with ModusToolbox software v2.1. |

------

All other trademarks or registered trademarks referenced herein are the property of their respective owners.

![Banner](images/ifx-cy-banner.png)

-------------------------------------------------------------------------------

© Cypress Semiconductor Corporation, 2020. This document is the property of Cypress Semiconductor Corporation and its subsidiaries ("Cypress"). This document, including any software or firmware included or referenced in this document ("Software"), is owned by Cypress under the intellectual property laws and treaties of the United States and other countries worldwide. Cypress reserves all rights under such laws and treaties and does not, except as specifically stated in this paragraph, grant any license under its patents, copyrights, trademarks, or other intellectual property rights. If the Software is not accompanied by a license agreement and you do not otherwise have a written agreement with Cypress governing the use of the Software, then Cypress hereby grants you a personal, non-exclusive, nontransferable license (without the right to sublicense) (1) under its copyright rights in the Software (a) for Software provided in source code form, to modify and reproduce the Software solely for use with Cypress hardware products, only internally within your organization, and (b) to distribute the Software in binary code form externally to end users (either directly or indirectly through resellers and distributors), solely for use on Cypress hardware product units, and (2) under those claims of Cypress's patents that are infringed by the Software (as provided by Cypress, unmodified) to make, use, distribute, and import the Software solely for use with Cypress hardware products. Any other use, reproduction, modification, translation, or compilation of the Software is prohibited.
TO THE EXTENT PERMITTED BY APPLICABLE LAW, CYPRESS MAKES NO WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, WITH REGARD TO THIS DOCUMENT OR ANY SOFTWARE OR ACCOMPANYING HARDWARE, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. No computing device can be absolutely secure. Therefore, despite security measures implemented in Cypress hardware or software products, Cypress shall have no liability arising out of any security breach, such as unauthorized access to or use of a Cypress product. CYPRESS DOES NOT REPRESENT, WARRANT, OR GUARANTEE THAT CYPRESS PRODUCTS, OR SYSTEMS CREATED USING CYPRESS PRODUCTS, WILL BE FREE FROM CORRUPTION, ATTACK, VIRUSES, INTERFERENCE, HACKING, DATA LOSS OR THEFT, OR OTHER SECURITY INTRUSION (collectively, "Security Breach"). Cypress disclaims any liability relating to any Security Breach, and you shall and hereby do release Cypress from any claim, damage, or other liability arising from any Security Breach. In addition, the products described in these materials may contain design defects or errors known as errata which may cause the product to deviate from published specifications. To the extent permitted by applicable law, Cypress reserves the right to make changes to this document without further notice. Cypress does not assume any liability arising out of the application or use of any product or circuit described in this document. Any information provided in this document, including any sample design information or programming code, is provided only for reference purposes. It is the responsibility of the user of this document to properly design, program, and test the functionality and safety of any application made of this information and any resulting product. "High-Risk Device" means any device or system whose failure could cause personal injury, death, or property damage. Examples of High-Risk Devices are weapons, nuclear installations, surgical implants, and other medical devices. "Critical Component" means any component of a High-Risk Device whose failure to perform can be reasonably expected to cause, directly or indirectly, the failure of the High-Risk Device, or to affect its safety or effectiveness. Cypress is not liable, in whole or in part, and you shall and hereby do release Cypress from any claim, damage, or other liability arising from any use of a Cypress product as a Critical Component in a High-Risk Device. You shall indemnify and hold Cypress, its directors, officers, employees, agents, affiliates, distributors, and assigns harmless from and against all claims, costs, damages, and expenses, arising out of any claim, including claims for product liability, personal injury or death, or property damage arising from any use of a Cypress product as a Critical Component in a High-Risk Device. Cypress products are not intended or authorized for use as a Critical Component in any High-Risk Device except to the limited extent that (i) Cypress's published data sheet for the product explicitly states Cypress has qualified the product for use in a specific High-Risk Device, or (ii) Cypress has given you advance written authorization to use the product as a Critical Component in the specific High-Risk Device and you have signed a separate indemnification agreement.
Cypress, the Cypress logo, Spansion, the Spansion logo, and combinations thereof, WICED, PSoC, CapSense, EZ-USB, F-RAM, and Traveo are trademarks or registered trademarks of Cypress in the United States and other countries. For a more complete list of Cypress trademarks, visit cypress.com. Other names and brands may be claimed as property of their respective owners.
