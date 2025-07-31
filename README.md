# Guide how to setup Wireguard VPN server on your OpenWRT router. Guide suits even for Turris Omnia router, which use OpenWRT on backgroud.

All steps are made thru LuCi web admin intergace. You don't need SSH for this.

# Install packages [see wireguard-guide-1.png]
1) In your router’s webUI, navigate to System - Software, click Update lists.
2) To search field type Wireguard. Install packages: wireguard-tools, kmod-wireguard, luci-proto-wireguard, qrencode, and libqrencode
3) Restart your router

# Setup firewall [see wireguard-guide-2.png]
4) Navigate to Network - Firewall - Port forwarding and create new rule
5) Name: Wireguard-Turris, Protocol: UDP, Source: WAN, External Port: 51821, Destination zone: LAN, Internal IP adress: turris (192.168.0.1), Internal Port: 51821
6) Save, Save and Apply

# Setup intergace to handle traffic of VPN [see wireguard-guide-3.png] [see wireguard-guide-4.png]
7) Navigate to Network - Interfaces and create new Interface
8) Name: wg0, Protocol: Wireguard VPN, Bring up on reboot: checked, Click generate new pair, Listen port: 51821, IP adresess: 10.0.0.1/24 (must be differend from your normal LAN network IP range. Suitable for my situation, when LAN is 192.168.0.1/24).
9) On card Firewall settings set zone: LAN
10) Save, Save and Apply


# Section of Creating Peers, who can connect to your VPN. [see wireguard-guide-5.png] [see wireguard-guide-6.png]
11) Click edit on your newly created interface wg0 and go to card Peers. Each line of Peer is one device for VPN connections.
12) Click add Peer button. Name: Captain America PC (for example), Click button Generate new key pair, Click Generate preshared key
13) IP adresess: 10.0.0.2/32 (For every another user incremate last number of IP adress. Wolwerine will be: 10.0.0.3/32, Ironman: 10.0.0.4/32, Hulk: 10.0.0.5/32 and so on...)
14) End point IP adress: 85.207.119.6 (= your public IP adress, you can find at https://www.whatsmyip.org/)
15) End point port: 51821
16) Keep alive: 25
17) Save, Save and Apply

# Generate config file so device (user) can connect to your VPN server [see wireguard-guide-7.png]
18) Edit newly created Peer Captain America PC and click on button Generate configuration... on very bottom of the page.
19) You must edit data on this page. It doesn't persistate even when you click on Save button. Little bug here. Connection Endpoint: 85.207.119.6 (= your public IP adress), DNS servers: 8.8.8.8 (default is your OpenWRT adress, probably you doesn't have DNS server there, so use google DNS server instead)
20) Scan QR code to your Android wireguard App. Or for PC copy generated configuration and save it to your PC with Notepad and name it like captain-america-pc.conf After that load this config file with your Wireguard App on PC.

# Apply settings of interface [see wireguard-guide-8.png]
21) Navigate to Network - Interfaces and click on Restart button of your wg0 interface. Only this make your settings applied. Save and Apply button isn't enough. Do this every time when you change settings in your intergace.
22) Now your Wireguard VPN server is ready to accept clients (Peers). Go test it.
