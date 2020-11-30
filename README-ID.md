# Panduan Hackintosh Catalina dengan Opencore Thinkpad T450 & T450s 
Repo ini berisi panduan instalasi dan file EFI yang diperlukan untuk Hackintosh Big Sur Public Release, Catalina dan Mojave yang dapat digunakan pada T450 atau T450s karena memiliki hardware yang sama.
Semuanya berfungsi dengan baik dan stabil seperti dijelaskan dalam bacaan ini

## Beberapa yang harus diketahui tentang repo ini:

- **Panduan ini bukan untuk model processor Haswell Generasi ke 4**
- **Folder EFI dan patched ACPI itu pembuatan pertama oleh [EchoEspirit](https://github.com/EchoEsprit/Hackintosh-Catalina-OpenCore-Lenovo-T450s-efi) dan dioptimalkan lebih lanjut oleh [i3p9](https://github.com/i3p9/Hackintosh-Catalina-OpenCore-Lenovo-T450s-efi). Saya mengubah beberapa hal dan memperbaiki beberapa kesalahan yang terjadi pada T450 + menambahkan Intel WiFi drivers dari [Openintelwireless](https://github.com/OpenIntelWireless)**
- **Saya akan mencoba sebaik mungkin untuk terus memperbarui repo dengan kexts dan versi Opencore terbaru**
- **EFI ini berkerja dengan Big Sur Public Release (11.0.1), Catalina dan Mojave**
- **EFI ini dikonfigurasi dengan Big Sur. Jika ingin menggunakannya di Catalina atau Mojave, baca seluruh panduan untuk mengetahui dibagian mana harus dilakukan perubahan**

![img](https://img.shields.io/github/last-commit/racka98/Lenovo-Thinkpad-T450-T450s-Hackintosh-Guide-Opencore.svg?color=green&label=last-commit) ![img](https://img.shields.io/badge/macOS%20support-Big%20Sur--11.0.1-blue) ![img](https://img.shields.io/badge/Opencore%20version-0.6.3-red)

![Tentang Mac Big Sur](https://imgur.com/vYo3Jt4.png)

# Perkenalan

Folder EFI dan Panduan untuk Thinkpad T450 dan T450s Hackintosh Catalina.

- `CPUs yang teruji`: **i5-5200U/5300u & i7-5600u**
- `VGA terintegrasi`: **HD Graphics 5500**
- `Sound Card`: **ALC292**
- `Kartu WIFI yang teruji`: **DW1820A 00JT494/Broadcom BCM94360CSAX/Intel 7265/7260**

# Bios

- `Security -> Security Chip`: **Disabled**;
- `Memory Protection -> Execution Prevention`: **Enabled**;
- `Virtualization -> Intel Virtualization Technology`: **Enabled**;
- `Virtualization -> Vt-directed IO`: **Disabled**;
- `Internal Device Access -> Bottom Cover Tamper Detection`: harus di **Disabled**;
- `Anti-Theft -> Current Setting`: **Disabled**;
- `Anti-Theft -> Computrace -> Current Setting`: **Disabled**;
- `Secure Boot -> Secure Boot`: **Disabled**;
- `UEFI/Legacy Boot`: **UEFI Only**;
- `Fingerprint Sensor`: **Disabled** `(Menyebabkan masalah wake from sleep)`;
- `CSM Support`: **Yes**.

# Apa yang berfungsi

- Sleep / Wake
- Wifi and Bluetooth (Built-in Intel 7265 or 7260 cards with Airportitlwm.kext) **itlwm.kext sangat direkomendasikan untuk Catalina karena Airportitlwm menyebabkan masalah dengan Trackpad setelah wake from sleep {See on Post Install}**
- AirPort Extreme (Broadcom BCM94360CSAX & NGFF A/E Adapter) **Recomendasi Upgrade untuk native WiFi & Bluetooth**
- Handoff, Continuity, AirDrop
- iMessage, FaceTime, App Store, iTunes Store (Change Config.plist -> PlatformInfo -> Generic -> MLB and SystemSerialNumber)
- Ethernet
- Onboard audio (Use alc_fix untuk memperbaiki jack yang tidak berfungsi setelah dipasang kembali)
- USB 2.0 / USB 3.0
- Dual Batteries
- Touchpad
- Trackpoint
- miniDP
- SD Card Reader (Terimakasih kepada @willmav5000)
- Gunakan [one-key-hidpi](https://github.com/xzhih/one-key-hidpi) untuk mengaktifkan HiDPI
- Jika Anda menggunakan mouse usb dengan tombol samping, Anda dapat menipu mouse usb apel dengan mengubah pid and vid di AnyAppleUSBMouse.kext/Info.plist dan mengaktifkan di config.plist.

# Apa yang tidak berfungsi

- VGA
- Sidecar (Wired Sidecar berfungsi tapi hanya di Macbook9,1 SMBIOS, dengan daya tahan baterai yang buruk, kamu dapat memilih apa yang kamu inginkan)
- Dengan IntelBluetoothFirmware.kext dan Airportitlwm.kext atau itlwm.kext mengaktifkan Bluetooth headphones hanya berkerja ketika anda tidak terkoneksi jaringan wifi or mematikan wifi. Ini adalah masalah dengan Wifi card Intel 7265 dan 7260. Anda dapat mendapatkan wifi card seri 8x untuk memperbaiki hal ini atau membeli rekomendasi wifi card (DW1820A 00JT494 atau Broadcom BCM94360CSAX)

## Catatan: Jika ingin mengedit config.plist, jangan gunakan OpenCore configurator or Clover configurator, gunakan PlistEdit pro (disertakan dalam Utilities) atau Xcode.

# Panduan Instalasi

## macOS Big Sur

**Untuk rilis ini, Anda harus menggunakan macOS untuk membuat USB Installer. Instalasi USB yang dibuat di Linux atau windows tidak akan berfungsi. Jika Anda tidak memiliki mac asli atau hackintosh lain, Anda selalu dapat menggunakan VM (Lihat Catatan no. 2)**

**Ini adalah cara cepat dan sederhana untuk pembuatan USB Installer** 
1. Download gibMacOS: https://github.com/corpnewt/gibMacOS

2. Klik kanan kemudian pilih gibMacOS.command (itu akan terbuka dengan terminal)

3. Pilih nomor dari daftar yang disediakan (Big Sur akan menjadi nomor 1 dalam daftar list dan disebut 11.0.1 Public release)

4. Ini akan mengunduh Big Sur (12.19 GB) and dan akan menyimpan di folder gibMacOS dibawah folder `macOS Downloads/publicrelease/11.0.1 macOS Big Sur`

5. Buka InstallAssistant.pkg akan mengekstrak instalasi ke dalam folder Applications

6. Format flashdisk menjadi Mac OS Extentended (Journaled) dengan GUID partition scheme

7. Sekarang jalankan: `sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

> MyVolume akan diganti dengan nama USB yang Anda berikan ketika itu di format. Atau Anda dapat menamai usb sebagai MyVolume saat Anda memformat.
> Ini akan membuat usb bootable yang berfungsi pada mac asli.

8. Sekarang Anda dapat melanjutkan "Pengaturan OpenCore's EFI environment" dari [sini](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html#setting-up-the-installer).

9. Copy file OC and BOOT dari EFI saya dan jalankan instalasi.

10. Pilih Install macOS Big Sur dari boot menu dan jangan dari partisi recovery.

Di [Panduan Instalasi Dortania](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) itu dapat menjadi rujukan yang lebih jelas. Saya belum sempat menulis panduan mendetail.

## Catatan: 
## 1. Jika Anda Menginstall Catalina atau Mojave ini sangat penting anda harus menonaktifkan Airportitlwm.kext di Kernel/Add/20 of Config.plist dan mengaktifkan itlwm.kext instead. Baca setelah instalasi #4.
## 2. For those having a black screen or frozen installer when booting the install USB, create the USB using macOS and not Linux or Windows. Details on that [here](https://github.com/racka98/Lenovo-Thinkpad-T450-T450s-Hackintosh-Guide-Opencore/issues/2#issuecomment-732408469)

# Setelah Instalasi
Setelah Anda memverifikasi bahwa mesin Anda melakukan boot dengan benar tanpa masalah apa pun seperti yang dijelaskan dalam "Apa yang berfungsi", lanjutkan nmelakukan hal berikut ini

### 1. Nonaktifkan mode Verbose (layar hitam dengan log pada saat memulai)
di config.plist, berpindah pada NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args dan cuma hapus argument '-v'

### 2. Disable AppleDebug and ApplePanic
Didalam config.plist, arahkan ke Misc -> Debug dan rubah keduanya AppleDebug dan ApplePanic ke False (NO)

Anda dapat menonaktifkan layar boot picker jadi anda dapat langsung boot ke Apple logo dengan menyeting ShowPicker dibawah Misc -> Boot ke False (NO)

### 3. Mengaktifkan WiFi with dengan chipset Intel di Catalina dan Mojave
Jika anda menggunakan Catalina atau Mojave, anda dapat mengaktifkan chipset wifi card intel dengan berpindah (di config.plist) ke Kernel -> Add -> 20 dan seting Enabled ke False/NO (menonaktifkan Airportitlwm.kext) dan di 21 seting Enabled ke True/YES (aktifkan itlwm.kext). After enabling these and rebooting install Heliport App (ada di folder Utilities).Setelah mengaktifkan ini dan restart kemudian install aplikasi Heliport 

atau kamu dapat menggunakan  Airportitlwm.kext untuk Catalina dari folder Intel wifi kexts dan dapatkan wifi native di Catalina idengan mengorbankan kehilangan trackpad setelah wake from sleep.

**Bagi mereka yang berada di Big Sur Anda dapat dengan nyaman menggunakan Airportitlwm.kext termasuk issue masalah trackpad setelah wake from sleep tidak terjadi di Big Sur.**

**Catatan:**

  **1. Airportitlwm.kext memberi Anda menu WiFi asli dan mengaktifkan layanan lokasi, tetapi sering menyebabkan masalah dengan trackpad & trackpoint setelah bangun dari tidur (tidak berfungsi) di Catalina dan Mojave (bukan Big Sur). Cara cepat untuk memperbaiki laptop adalah dengan menidurkan kembali laptop dengan menutup tutupnya sampai lampu tidur merah mulai berkedip kemudian membangunkan laptop lagi. Juga itu hanya terjadi ketika Anda meletakkan laptop dalam mode tidur untuk waktu yang sangat lama (lebih dari 2 atau 3 jam). Jadi bagi yang tidak menidurkan laptopnya dalam waktu yang sangat lama dan baru mematikannya setelah digunakan, kext ini oke untuk digunakan.**
  
  **2. Airportitlwm.kext yang termasuk dalam EFI ini cuma untuk for Big Sur. Bagi mereka yang berada di Catalina atau Mojave, Anda harus mengunduh Airportitlwm.kext yang sesuai dari [Openintelwireless](https://github.com/OpenIntelWireless) atau menggunakan salah satu Intel WiFi Kexts  didalam folder dari repo ini (Disarankan) dan ganti salah satu di EFI -> Kexts.**
  
### 4. Menambahkan Device Properties untuk Serial number dan info lainnya
Ikuti panduan ini [guide](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#generate-a-new-serial) untuk set up serial number dan info yang menyertainya untuk iServices

fork [dari](https://github.com/racka98/Lenovo-Thinkpad-T450-T450s-Hackintosh-Guide-Opencore)


