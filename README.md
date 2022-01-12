# openSUSE-Leap-15.3-ARMv7-XFCE-cho-Nexus-7-2012-wifi-rev-E.1565-kernel-5.14-rc3-support-USB-OTG
Kỉ niệm hôm nay bắt đầu chích Vero Cell/Sinopharm cho dân, như đã hứa lên bài openSUSE-Leap-15.3-ARMv7-XFCE cho Nexus 7 2012 grouper rev e1565 kernel 5.14-rc3-next-grate



Link download trên Google drive

openSUSE Leap 15.3/15.4 ARMv7 XFCE/LXQT/X11 kernel-5.15.0-rc4-next-20211011-postmarketos-grate

https://drive.google.com/drive/u/0/folders/1iaBFiZp1S-fj7GV50AfGyvs8DFAubN4O


openSUSE Leap 15.3 ARMv7 XFCE f2fs rootfs

https://drive.google.com/drive/folders/1-VlUhCEQz3kVNNCRYISqfc1tXAkLhwrk?usp=sharing



openSUSE Leap 15.3 ARMv7 XFCE ext4 rootfs

https://drive.google.com/drive/folders/19ArXlOwX2xN8-SHjxPMzMQ-kj-Xkvbf0?usp=sharing



Cài android-tools và fastboot trên Linux/Windows



Unlock bootloader cho Nexus 7, tham khảo trên mạng

https://wiki.postmarketos.org/wiki/Google_Nexus_7_2012_(asus-grouper)



Variants

grouper rev. PM269 - without GSM (oldest)
grouper rev. E1565 - without GSM (modern revision)
tilapia rev. E1565 - with GSM





Do I have grouper or tilapia?





TWRP (adb shell) $ grep androidboot.baseband=unknown /proc/cmdline && echo grouper || echo tilapia



Which hardware revision of grouper do I have?





TWRP (adb shell) $ find /sys/devices/ | grep -c max776 && echo You have E1565



TWRP (adb shell) $ find /sys/devices/ | grep -c tps6591 && echo You have PM269



Cài TWRP 3.3.1-0 trở lên



Đưa máy về bootloader. Kết nối Nexus 7 2012 wifi vào PC/laptop. Chuẩn usb 1.0, 1.1, 2.0 dùng tốt. Chuẩn usb 3.0 dễ bị over cache push vào Nexus 7 2012, unsupport



$ sudo adb reboot bootloader



Flash boot image vào boot partition (đổi tên boot.img-asus-grouper thành boot.img)



$ fastboot flash boot boot.img



Vào TWRP 3.3.1-0 trở lên, vào Advance → Terminal



$ df



$ umount /dev/block/mmcblk0p__  <- fill partition number (# 2 lần)



Dùng lệnh adb để bung rootfs vào mmcblk0p__ trên PC/laptop



$ sudo adb start-server



Chuyển đến thư mục chứa rootfs image



$ sudo adb push openSUSE-Leap-15.3-ARMv7-XFCE+asus-grouper.img /dev/block/mmcblk0p__  <- fill partition number



grouper has likely data on /dev/block/mmcblk0p9 but make sure!
tilapia has likely data on /dev/block/mmcblk0p10 but make sure!



Các utils để trong /opt gồm các scripts:



- sysctl.conf tối ưu VMs và thông số kernel



- cpufreq.start tối ưu Ondemand governor



- temp_throttle để kìm hãm con ngựa Tegra thành Troy không quá nhiệt (hơn 60 độ kernel sẽ tự khởi động lại, để 59 độ là max, ở 53 và 55 độ ổn định)



- clear_RAM để remove thêm RAM nếu cần



***Fix sound ALC5642 cho tegra-rt5640***



$ sudo lsmod | grep "^snd" | cut -d " " -f 1



snd_soc_tegra30_i2s

snd_soc_tegra30_ahub

snd_soc_tegra_pcm

snd_soc_tegra_machine

snd_soc_tegra_utils

snd_soc_core

snd_pcm_dmaengine

snd_pcm

snd_timer

snd



$ sudo nano /etc/modules



snd_soc_tegra30_i2s

snd_soc_tegra30_ahub

snd_soc_tegra_pcm

snd_soc_tegra_machine

snd_soc_tegra_utils

snd_soc_tegra_wm8903

snd_soc_core

snd_pcm_dmaengine

snd_pcm

snd_timer

snd



$ reboot



Checking soc soundcard loaded:



$ sudo cat /proc/asound/card*/id



ALC5642



$ sudo alsa force-reload



$ alsamixer



Enable các thông số thiết lập (phím M hoặc phím mũi tên lên/xuống): "Speaker R" "Speaker L" "DAC MIXR INF1" "DAC MIXL INF1" "SPOL MIX DAC R1" "SPOL MIX DAC L1" "Stereo DAC MIXR DAC R1" "Stereo DAC MIXL DAC L1"



Wifi dùng wifi-menu/wpa_supplicant/iwd và network-manager



Bluetooth dùng được bluez5 và blueman



NFC dùng được với neard



Image source: http://download.opensuse.org/ports/armv7hl/distribution/leap/15.3/appliances/openSUSE-Leap-15.3-ARM-XFCE.armv7-rootfs.armv7l-2021.05.31-Build8.1.tar.xz



usr/passwd



root/[set at oem install]
