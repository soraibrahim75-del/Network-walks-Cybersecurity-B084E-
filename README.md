# NETWORKWALKS-IBRAHIM-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP
# WK1-PM1: My Cybersecurity Lab Setup (VirtualBox & Kali)

Hey! Welcome to my repo for my Week 1 Project Module 1 lab. This is where I documented how I set up my own virtual hacking lab using VirtualBox and Kali Linux. 

Honestly, this took way longer than I expected. The biggest headache was a weird VirtualBox v7 bug that completely killed Kali's internet connection. I managed to fix it, and I've put the exact fix down below so you don't have to suffer through it.

The Lab Specs
Here is what I ended up building:
- Base: Oracle VirtualBox (Latest version)
- Attacker VM: Kali Linux (Pre-built)
- Network: NATNetwork (to keep it isolated but still online)
- Subnet: 10.0.0.0/24
  Kali IP: 10.0.0.2/24 (Static)
- Extras: Bidirectional clipboard, drag/drop enabled, and my host /downloads folder shared with the VM.

---

How I Built It (Step-by-Step)

Here is the exact process I followed. 

1. The Prep Work
You'll need a PC with at least 8GB of RAM and virtualization turned on in the BIOS.
First, grab:
- [7-Zip](https://7-zip.org/download.html) (to extract the Kali VM file).
- [VirtualBox](https://virtualbox.org/wiki/Downloads) (don't forget the Extension Pack!).
- [Kali Linux Pre-built VM](https://kali.org/get-kali).

2. Creating the NATNetwork
Don't just use the default NAT adapter. We need a NATNetwork so VMs can talk to each other.
1. Open VirtualBox -> File -> Tools -> Network (or Ctrl+H).
2. Go to the NAT Networks tab.
3. Hit Create.
4. Name it NatNetwork and set the IPv4 Prefix to 10.0.0.0/24.
5. Leave DHCP checked for now and hit Apply.

3. Importing Kali & Fixing Settings
1. Extract that Kali .7z file using 7-Zip.
2. In VirtualBox, go to Machine -> Add and open the .vbox file.
3. Before you boot it, right-click the VM and go to Settings:
   - System: Bump the RAM up to at least 2048 MB (I used 4096 MB) and give it 2 CPUs.
   - *Network: Adapter 1 -> NAT Network, Name -> NatNetwork.
   - *Shared Folders: Add your Host Downloads folder. Name it downloads (all lowercase) and check auto-mount.
   - *General -> Advanced: Set Shared Clipboard and Drag'n'Drop to Bidirectional.

4. Assigning the IP & The "No Internet" Fix
Boot into Kali, open the terminal and let's force it to use 10.0.0.2.

bash
Set static IP, Gateway and DNS
sudo nmcli connection modify "Wired connection 1" ipv4.addresses 10.0.0.2/24
sudo nmcli connection modify "Wired connection 1" ipv4.gateway 10.0.0.1
sudo nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8, 1.1.1.1"
sudo nmcli connection modify "Wired connection 1" ipv4.method manual

Apply changes
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
