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

 b) For at du skal kunne koble til Pi'en fra en annen pc, da trenger du IP-en. Skriv inn "ip a" da får du masse ting som popper opp. Du skal lete etter der det står inet og huske den IP-en som står der. VIKTIG.
### 4. 