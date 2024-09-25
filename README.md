# Oppsett av Raspberry Pi, Linux, SSH, Databaser og Telefonkatalog

# 1. Raspberry og Ubuntu
    a) Dra SD kortet ut av Raspberry Pi'en og plugg inn i pcen din, deretter går du inn på denne nettsiden. (https://www.raspberrypi.com/software/)

    b) Når du trykker på installer så kommer det en skjerm hvor du velger versjoner og der velger du. (Raspberry pi 4 | Ubuntu | SD Card)

    c) La det laste ned, deretter flytt SD kortet tilbake inn i raspberry pi'en og last ned det som kommer opp der også.
# 2. Linux
    a) Nå har du logga inn og er inne på skrivebordet. Trykk på CRTL + ALT + T for å åpne terminal.

    b) Oppdatere programvare og oppdateringer (copy, paste)
    sudo apt update (finner oppdateringer)
    sudo apt upgrade (installerer oppdateringer)

    c) På tide å sette opp brannmur
    sudo apt install ufw
    sudo ufw enable (Skrur på brannmur)
    sudo ufw allow ssh (koble til Raspberry pi'en gjennom brannmur)
    sudo ufw status (Sjekk status på brannmur)
# 3. SSH