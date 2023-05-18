# Jailbreaking IOS

## Tutorial: Jailbreaking iPhone on macOS

Jailbreaking your iPhone can give you additional control and customisation options beyond what is offered by Apple's official iOS. This tutorial will guide you through the process of jailbreaking your iPhone on macOS using the Palera1n tool.

### Prerequisites

Before you begin, ensure that you meet the following requirements:

* Your device is running iOS 15 to iOS 16.4.1.
* Your device model is one of the following: iPhone X, iPhone 8, iPhone 8 Plus, iPhone 7, iPhone 7 Plus, iPhone SE (1st), iPhone 6S, iPhone 6S Plus, iPhone 6, iPhone 6 Plus, iPad Mini 2, iPad Mini 3, iPad Mini 4, iPad 5th, iPad 6th, iPad 7th, iPad Air, iPad Air 2, iPad Pro 1st, iPad Pro 2nd, iPod Touches 6 & 7.
* If you have an A11 device on iOS 16 with a passcode set, you will need to erase all content and settings before jailbreaking.

### Step 1: Download Palera1n

1. Visit the Palera1n website at [https://pangu8.com/jailbreak/palera1n/#palerain-download](https://pangu8.com/jailbreak/palera1n/#palerain-download).
2. Download the appropriate version of Palera1n for your macOS. For most macOS users, the recommended version is `palera1n-macos-universal`, which can be downloaded from [https://cdn.nickchan.lol/palera1n/c-rewrite/releases/v2.0.0-beta.6/palera1n-macos-universal](https://cdn.nickchan.lol/palera1n/c-rewrite/releases/v2.0.0-beta.6/palera1n-macos-universal).

### Step 2: Installation

1. Enable Full Disk Access for Terminal by following these steps:
   * For macOS Monterey and below: Go to System Preferences → Security & Privacy → Privacy → Full Disk Access.
   * For macOS Ventura and above: Go to System Settings → Privacy & Security → Full Disk Access.
   * If Terminal is not listed, click the plus icon, and select it from Applications → Utilities.
2. Open a Terminal window.
3. Change the directory to the location where Palera1n was downloaded. Use the command `cd ~/Downloads` if you downloaded it to the Downloads folder.
4. Run the following commands in Terminal one by one:
   * `sudo mkdir -p /usr/local/bin`
   * `sudo mv ./palera1n-macos-universal /usr/local/bin/palera1n` (Replace `./palera1n-macos-universal` with the version you downloaded)
   * `sudo xattr -c /usr/local/bin/palera1n`
   * `sudo chmod +x /usr/local/bin/palera1n`

### Step 3: Jailbreaking

1. Connect your device to your Mac using a USB cable.
2. In the Terminal window, run the command `palera1n`. Make sure your device is plugged in before entering this command.
3. When prompted, press Enter and follow the on-screen instructions to enter DFU mode. The instructions will typically involve holding the power button and home button together for a specified number of seconds. Release the power button while keeping the home button pressed. The tool will notify you if the DFU mode entry was unsuccessful.
   * Note: Some USB-C to Lightning cables may not work reliably. It is recommended to use a USB-A to Lightning cable for a more stable connection.

### Step 4: Handling Issues for A9(X) and Earlier Devices

A9(X) and earlier devices may encounter an issue where they get stuck midway through the jailbreaking process in pongoOS. Follow these steps to work around this issue:

1. If your device gets stuck in pongoOS during the process, press **Control + C** on your keyboard in the Terminal window.
2. Rerun the command `palera1n` that you ran previously.
3. You'll need to repeat this step every time you rejailbreak your device.

## SSH to IPhone

Once your iPhone is jailbroken, you can use SSH (Secure Shell) to connect to it from your macOS. Here's a step-by-step guide on how to SSH into your jailbroken iPhone:

### Step 1: Install OpenSSH on your iPhone

1. Launch the Cydia app on your jailbroken iPhone.
2. Tap on the "Search" tab at the bottom and search for "OpenSSH".
3. Select the "OpenSSH" package from the search results.
4. Tap on "Install" and then "Confirm" to begin the installation process.
5. Once the installation is complete, OpenSSH will be installed on your iPhone.

### Step 2: Find your iPhone's IP address

1. On your iPhone, go to the "Settings" app.
2. Tap on "Wi-Fi" and make sure you are connected to the same Wi-Fi network as your Mac.
3. Tap on the (i) icon next to your connected Wi-Fi network.
4. Note down the IP address listed under the "IP Address" section. This is your iPhone's IP address on the local network.

### Step 3: SSH into your iPhone from macOS

1. Open the Terminal app on your macOS. You can find it in the "Utilities" folder within the "Applications" folder.
2.  In the Terminal, use the following command to connect to your iPhone via SSH:

    ```shell
    shellCopy codessh root@<your_iPhone_IP_address>
    ```

    Replace `<your_iPhone_IP_address>` with the IP address you noted down in Step 2. For example:

    ```shell
    shellCopy codessh root@192.168.1.10
    ```
3. Press Enter and then enter the root password for your iPhone when prompted. The default password is usually "alpine" unless you have changed it. Note: It is highly recommended to change the default root password for security reasons. You can do this by running the command `passwd` after connecting via SSH.
4. Once you have entered the correct password, you should now be connected to your jailbroken iPhone via SSH. You can now execute commands and navigate the file system on your iPhone using the Terminal on your macOS.

## Removing the Jailbreak

If you wish to remove the jailbreak from your device, you can follow these steps:

* For Rootless Jailbreak: Run the command `./palera1n --force-revert` in the Terminal.
* For Rootful Jailbreak: Run the command `./palera1n --force-revert -f` in the Terminal.

These commands will revert the jailbreak modifications and restore your device to the stock iOS state.
