# Oppsett av Raspberry Pi, Linux, SSH, Databaser og Telefonkatalog

### 1. Raspberry og Ubuntu
a) Dra SD kortet ut av Raspberry Pi'en og plugg inn i pcen din,deretter går du inn på denne nettsiden. (https://www.raspberrypi.com/software/)

b) Når du trykker på installer så kommer det en skjerm hvor du velger versjoner og der velger du. (Raspberry pi 4 | Ubuntu | SD Card)

c) La det laste ned, deretter flytt SD kortet tilbake inn i raspberry pi'en og last ned det som kommer opp der også.
### 2. Linux
a) Nå har du logga inn og er inne på skrivebordet. Trykk på CRTL + ALT + T for å åpne terminalen.

b) Oppdatere programvare og oppdateringer, skriv disse commandsa

    sudo apt update (finner oppdateringer)

    sudo apt upgrade (installerer oppdateringer)

c) På tide å sette opp brannmur, skriv disse commandsa

    sudo apt install ufw

    sudo ufw enable (Skrur på brannmur)

    sudo ufw allow ssh (koble til Raspberry pi'en gjennom brannmur)

    sudo ufw status (Sjekk status på brannmur)
### 3. SSH
a) Vi skal fikse at Pi'en kan kobles til remotely med ssh, skriv disse commandsa

    sudo apt install openssh-server(Installerer ssh-server)

    sudo systemctl enable ssh (SSH skrur seg på ved oppstart)

    sudo systemctl start ssh (starter SSH)

 b) For å koble til via SSH fra en remote PC så trenger du ip-en til pi'en, den skaffer du ved å skrive "ip a", etter du har trykka "enter" så kommer dette opp
    
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
    2: eth0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether d8:3a:dd:f1:5e:8e brd ff:ff:ff:ff:ff:ff
    3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether d8:3a:dd:f1:5e:8f brd ff:ff:ff:ff:ff:ff
    inet 10.2.4.30/8 brd 10.255.255.255 scope global dynamic noprefixroute wlan0
       valid_lft 689845sec preferred_lft 689845sec
    inet6 fe80::5e93:95fb:ba4a:fa03/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
Veldig forvirrende men det eneste du skal se etter er **inet 10.2.4.30/8 brd 10.255.255.255** innenfor
**3: wlan0:** du skal velge den første der **10.2.4.30**, denne ser forskjellig ut for alle.
### 4. Git, Python, MariaDB
a) Nå skal du laste ned litt mer greier.

    sudo apt install python3-pip (laster ned python)

    sudo apt install git (laster ned git)

    sudo apt install mariadb-server (laster ned mariadb)

    sudo mysql_secure_installation (laster ned kodespråk SQL for mariaDB)

b) Lag bruker med alle rettigheter som du skal bruke på MariaDB

    (logger inn i admin bruker, lagt inn fra før)
    sudo mariadb -u root 

    (lag ny bruker, bytt ut username og password, HUSK LOG IN *VIKTIG* skriv ned login)
    CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
    
    (gir alle rettigheter til brukern du lagde, bytt ut username med brukeren du lagde)
    GRANT ALL PRIVILEGES ON *.* TO 'username'@’localhost’ IDENTIFIED BY 'password';

    (oppdaterer rettighetene)
    FLUSH PRIVILEGES;
