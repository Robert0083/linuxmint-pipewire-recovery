# linuxmint-pipewire-recovery
Sistergő hang és Cinnamon összeomlás javítása Linux Mint 22.1 Xia alatt PipeWire/PulseAudio konfliktus után.

# Linux Mint 22.1 – PipeWire/PulseAudio helyreállítás és Cinnamon javítás

Ez a projekt dokumentálja, hogyan sikerült visszaállítani egy működésképtelenné vált Linux Mint 22.1 rendszert, amelyen a PipeWire és PulseAudio váltása miatt:

- a hang sistergő lett,
- a Cinnamon munkamenet összeomlott,
- a rendszer fekete képernyőn ragadt, TTY sem működött.

## Probléma röviden

- PipeWire/PulseAudio konfliktus miatt a `cinnamon-session` nem indult
- Recovery mód elindult, de a billentyűzet nem reagált
- Teljes rendszer összeomlás, grafikus és szöveges felület nélkül

## Megoldás

1. Live USB-ről boot
2. A rendszerpartíció csatolása és `chroot`
3. Cinnamon újratelepítése
4. PulseAudio újratelepítése, PipeWire eltávolítása
5. Újraindítás és validálás

## Parancsok

```bash
# 1. Rendszerpartíció csatolása (pl. /dev/sda3)
sudo mount /dev/sda3 /mnt
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
sudo chroot /mnt

# 2. Cinnamon visszaállítása
apt update
apt install --reinstall cinnamon-session cinnamon mint-meta-cinnamon

# 3. PulseAudio újratelepítése (és PipeWire törlése, ha kell)
apt install --reinstall pulseaudio
apt purge pipewire pipewire-bin libpipewire* wireplumber

# 4. Kilépés és újraindítás
exit
sudo reboot
