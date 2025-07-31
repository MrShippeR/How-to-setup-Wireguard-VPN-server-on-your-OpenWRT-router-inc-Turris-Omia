# Wireguard OpenWRT setup guide
Guide how to install and config Wireguard VPN server on your OpenWRT router. Guide suits even for Turris Omnia router, which use OpenWRT on backgroud. All steps are made thru LuCi web admin intergace. You don't need SSH for this.

## Install packages
1) In your router’s webUI, navigate to System - Software, click Update lists.
2) To search field type Wireguard. Install packages: wireguard-tools, kmod-wireguard, luci-proto-wireguard, qrencode, and libqrencode
3) Restart your router
<img width="1921" height="1015" alt="wireguard-guide-1" src="https://github.com/user-attachments/assets/440551ac-a74b-4d4f-809c-8246dc34d950" />



## Setup firewall
4) Navigate to Network - Firewall - Port forwarding and create new rule
5) Name: Wireguard-Turris, Protocol: UDP, Source: WAN, External Port: 51821, Destination zone: LAN, Internal IP adress: turris (192.168.0.1), Internal Port: 51821
<img width="1921" height="1245" alt="wireguard-guide-2" src="https://github.com/user-attachments/assets/2bd6a17f-ced7-4a5c-8040-34659d0b10a6" />

6) Save, Save and Apply

## Setup intergace to handle traffic of VPN
7) Navigate to Network - Interfaces and create new Interface
8) Name: wg0, Protocol: Wireguard VPN, Bring up on reboot: checked, Click generate new pair, Listen port: 51821, IP adresess: 10.0.0.1/24 (must be differend from your normal LAN network IP range. Suitable for my situation, when LAN is 192.168.0.1/24).
<img width="1920" height="944" alt="wireguard-guide-3" src="https://github.com/user-attachments/assets/cdd46633-a6a9-49f4-9a3e-4cf3f53fbdff" />

9) On card Firewall settings set zone: LAN
<img width="1919" height="990" alt="wireguard-guide-4" src="https://github.com/user-attachments/assets/1334a109-8fcc-44fe-a2b0-cb787dc37538" />

10) Save, Save and Apply

## Section of Creating Peers, who can connect to your VPN.
11) Click edit on your newly created interface wg0 and go to card Peers. Each line of Peer is one device for VPN connections.
<img width="1920" height="953" alt="wireguard-guide-5" src="https://github.com/user-attachments/assets/71b34545-166c-4fbc-8226-76984f68ce97" />

12) Click Add peer button. Name: Captain America PC (for example), Click button Generate new key pair, Click Generate preshared key
13) IP adresess: 10.0.0.2/32 (For every another user incremate last number of IP adress. Wolwerine will be: 10.0.0.3/32, Ironman: 10.0.0.4/32, Hulk: 10.0.0.5/32 and so on...)
14) End point IP adress: 85.207.119.6 (= your public IP adress, you can find at https://www.whatsmyip.org/)
15) End point port: 51821
16) Keep alive: 25
<img width="1920" height="1139" alt="wireguard-guide-6" src="https://github.com/user-attachments/assets/eed94680-fbc2-4d0d-b142-d9e3a6b8bb4f" />

17) Save, Save and Apply

## Generate config file so device (user) can connect to your VPN server
18) Edit newly created Peer Captain America PC and click on button Generate configuration... on very bottom of the page.
19) You must edit data on this page. It doesn't persistate even when you click on Save button. Little bug here. Connection Endpoint: 85.207.119.6 (= your public IP adress), DNS servers: 8.8.8.8 (default is your OpenWRT adress, probably you doesn't have DNS server there, so use google DNS server instead)
20) Scan QR code to your Android wireguard App. Or for PC copy generated configuration and save it to your PC with Notepad and name it like captain-america-pc.conf After that load this config file with your Wireguard App on PC.
<img width="1921" height="1015" alt="wireguard-guide-7" src="https://github.com/user-attachments/assets/3d39437c-3589-4e3b-81e0-8e6e008b00c4" />

## Apply settings of interface
21) Navigate to Network - Interfaces and click on Restart button of your wg0 interface. Only this make your settings applied. Save and Apply button isn't enough. Do this every time when you change settings in your intergace.
<img width="1921" height="1015" alt="wireguard-guide-8" src="https://github.com/user-attachments/assets/9d78abed-bf33-46aa-873a-14bfe18343a6" />

22) Now your Wireguard VPN server is ready to accept clients (Peers). Go test it.

