LAN-Cache v1.1.3 GnB-LAN internal 
==============

Based off work of https://gitlab.com/frag-o-matic/lan-cache
So Credits go to Bruno Gysels and MultiPlay.co.uk for the base they made!

Credits for this work of art as of 24 Febr. 2016 is to NexusofDoom !!!

OS: Debian 8.5 x64 (Jessie)

If you are to lazy to read below you can use the script I created for this: https://github.com/bntjah/lc-installer (warning might contain bugs! So proceed on your own accord!)

	1) apt-get install build-essential libpcre3 libpcre3-dev zlib1g-dev libreadline-dev libncurses5-dev git libssl-dev
	2) nano /etc/dhcp/dhclient.conf
	- 2.1 Add the lines: prepend domain-name-servers 8.8.8.8, 8.8.4.4;
	3) git clone -b master http://github.com/imac2009/lancache
	4) curl http://nginx.org/download/nginx-1.11.3.tar.gz | tar zx
	5) ./configure --with-http_ssl_module --with-http_slice_module
	6) make
	7)  make install
	8) *grab a coffee right here*
	9) Creating the necessary ip's
		9.1) Paste the following in /etc/network/interfaces as root
		# Ip used for STEAM caching
		auto eth0:1
		iface eth0:1 inet static
		address 192.168.202.4
		netmask 255.255.252.0
		
		# Ip used for RIOT caching
		auto eth0:2
		iface eth0:2 inet static
		address 192.168.202.5
		netmask 255.255.252.0
		
		# Ip used for Blizzard caching
		auto eth0:3
		iface eth0:3 inet static
                address 192.168.202.6
		netmask 255.255.252.0
		
		# Ip used for Hirez caching
		auto eth0:4
		iface eth0:4 inet static
		address 192.168.202.7
		netmask 255.255.252.0

		# Ip used for Origin caching	
		auto eth0:5
		iface eth0:5 inet static
		address 192.168.202.8
		netmask 255.255.252.0
		
		# Ip used for Sony caching
		auto eth0:6
		iface eth0:6 inet static
		address 192.168.202.9
		netmask 255.255.252.0
		
		# Ip used for Microsoft caching
		auto eth0:7
		iface eth0:7 inet static
		address 192.168.202.10
		netmask 255.255.252.0
		
		# Ip used for Tera caching
		auto eth0:8
		iface eth0:8 inet static
		address 192.168.202.11
		netmask 255.255.252.0
        
		# Ip used for GOG caching
		auto eth0:9
		iface eth0:9 inet static
		address 192.168.202.12
		netmask 255.255.252.0

		# Ip used for ArenaNetworks caching
		auto eth0:10
		iface eth0:10 inet static
		address 192.168.202.13
		netmask 255.255.252.0

		# Ip used for WarGaming caching
		auto eth0:11
		iface eth0:11 inet static
		address 192.168.202.14
		netmask 255.255.252.0
        
    
	10) Create the user lancache
		 adduser --system --no-create-home lancache
		 addgroup --system lancache
		 usermod -aG lancache lancache
	
	11) Just create the folders:
		 mkdir -p /srv/lancache/data/blizzard
		 mkdir -p /srv/lancache/data/microsoft
		 mkdir -p /srv/lancache/data/installs
		 mkdir -p /srv/lancache/data/other
		 mkdir -p /srv/lancache/data/tmp
		 mkdir -p /srv/lancache/data/hirez/
		 mkdir -p /srv/lancache/data/origin/
		 mkdir -p /srv/lancache/data/riot/
		 mkdir -p /srv/lancache/data/sony/
		 mkdir -p /srv/lancache/data/steam/
		 mkdir -p /srv/lancache/logs
		 mkdir -p /srv/lancache/data/wargaming
		 mkdir -p /srv/lancache/data/tera
		 mkdir -p /srv/lancache/data/arenanetworks
		
	- 11.1 chowning can be achieved by: 
		 chown -R lancache:lancache /srv/lancache

	12) Copy the conf folder and contents (where you originally git cloned it to in step 4) to /usr/local/nginx/conf/
		 cp -R ~/lancache/conf /usr/local/nginx/
	13) Copy the Lancache file from init.d/ to /etc/init.d/ by:
		 cp -R lancache /etc/init.d/lancache
	14) Make it an executable:
		 chmod +x /etc/init.d/lancache
	15) Put it in the standard Boot:
		 update-rc.d lancache defaults
	16) Copy limits.conf to /etc/security/limits.conf 
	17) Start Lancache / Nginx by:
		 /etc/init.d/lancache start
	18) This step is extra but adviced for passing through HTTPS traffic
		18.1) git clone https://github.com/dlundquist/sniproxy
		18.2) nano /etc/sniproxy.conf
		Copy the data from https://github.com/OpenSourceLAN/origin-docker/blob/master/sniproxy/sniproxy.conf to the file
		18.3) cd sniproxy/src
		18.4) Start sniproxy with ./sniproxy -c /etc/sniproxy.conf
		

	Optional A) Monitor Through nload
		-A.1  apt-get install nload -y
		-A.2  nload -U G - u M -i 102400 -o 102400
	Optional B) Monitor Network Usage Through iftop
		-B.1  apt-get install iftop -y
		-B.2  iftop -i eth1
		Note ETH1 is the Interface I've defined for Lancache to use
		
Please note that this is how my setup runs on Debian x64 with ZFS configured.
