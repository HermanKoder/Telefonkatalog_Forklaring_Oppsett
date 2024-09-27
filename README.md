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
### 4. SSH sammenkobling remotely
Nå kan du endelig koble skjermen ut av pi'en og logge inn fra pcen din. Åpne terminalen din på windows ved å skrive "CMD" inn i search bar. Når du er der kan du skrive
    
    ssh username@ip
### 5. Git, Python, MariaDB
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
### 6. Telefonkatalog
Her er Telefonkatalog koden som du kan bruke og leke med, legger den ved i en fil også. Husk å endre inlogging detaljer i **test_kobling.py**

**Telefonkatalog koden:**

```python
import mysql.connector

def printMeny():
    print("------------------- Telefonkatalog -------------------")
    print("| 1. Legg til ny person                              |")
    print("| 2. Søk opp person eller telefonnummer              |")
    print("| 3. Vis alle personer                               |")
    print("| 4. Avslutt                                         |")
    print("------------------------------------------------------")
    menyvalg = input("Skriv inn tall for å velge fra menyen:")
    utfoerMenyvalg(menyvalg)

def utfoerMenyvalg(valgtTall):
#input returnerer string
    if(valgtTall == "1"):
        registrerPerson()
    elif(valgtTall == "2"):
        sokPerson()
        printMeny()
    elif(valgtTall == "3"):
        visAllePersoner()
    elif(valgtTall == "4"):
        bekreftelse = input("Er du sikker på at du vil avslutte? J/N")
        if(bekreftelse == "J" or bekreftelse == "j"):
            exit()
            #Gidder ikke sjekk for n. Fortsetter hvis ikke j
        else:
            printMeny()             
    else:
        nyttForsoek = input("Ugyldig valg. Velg et tall mellom 1-4.")
        utfoerMenyvalg(nyttForsoek)

def registrerPerson():
    fornavn = input("Skriv inn fornavn:")
    etternavn = input("Skriv inn etternavn:")
    telefonnummer = input("Skriv inn telefonnummer:")

    lagreIDatabase(fornavn, etternavn, telefonnummer)

    input("Trykk en tast for å gå tilbake til menyen")
    printMeny()

def visAllePersoner():
    mydb = mysql.connector.connect(
    host="172.29.51.19",
    user="test",
    password="1234",
    database="telefonkatalog"
    )

    mycursor = mydb.cursor()

    mycursor.execute("SELECT * FROM person")

    myresult = mycursor.fetchall()

    for x in myresult:
        print(x)

    mydb.close()
    printMeny()

def sokPerson():
    print("1. Søk på fornavn")
    print("2. Søk på etternavn")
    print("3. Søk på telefonnummer")
    print("4. Tilbake til hovedmeny")
    sokefelt = input("Skriv inn ønsket søk 1-3, eller 4 for å gå tilbake:")
    if(sokefelt == "1"):
        navn = input("Fornavn:")
        finnPerson("fornavn", navn)
    elif(sokefelt == "2"):
        navn = input("Etternavn:")
        finnPerson("etternavn", navn)
    elif(sokefelt == "3"):
        tlfnummer = input("Telefonnummer:")
        finnPerson("telefonnummer", tlfnummer)
    elif(sokefelt == "4"):
        printMeny()
    else:
        print("Ugyldig valg. Velg et tall mellom 1-4.")
        sokPerson()

        
#typeSok angir om man søker på fornavn, etternavn, eller telefonnumer
def finnPerson(typeSok, sokeTekst):

    mydb = mysql.connector.connect(
      host="172.29.51.19",
      user="test",
      password="1234",
      database="telefonkatalog"
    )

    mycursor = mydb.cursor()

    mycursor.execute("SELECT * FROM person where " + typeSok + " ='" + sokeTekst + "'")

    myresult = mycursor.fetchall()

    for x in myresult:
        print(x)
    
    

def lagreIDatabase(fornavn, etternavn, telefonnummer):

    mydb = mysql.connector.connect(
    host="172.29.51.19",
    user="test",
    password="1234",
    database="telefonkatalog"
    )

    mycursor = mydb.cursor(fornavn, etternavn, telefonnummer)

    sql = "INSERT INTO person (fornavn, etternavn, telefonnummer) VALUES (%s, %s, %s)"
    val = (fornavn, etternavn, telefonnummer)
    mycursor.execute(sql, val)

    mydb.commit()

    print(mycursor.rowcount, "record inserted.")



printMeny() #Starter programmet ved å skrive menyen første gang
```

**Login koden:**

```python
import mysql.connector # pip install mysql-connector-python

mydb = mysql.connector.connect(
host="ip_adressen_til_PI",
user="username",
password="password",
database="telefonkatalog")

cursor = mydb.cursor()
cursor.execute("SELECT * FROM person")
resultater = cursor.fetchall()

for dings in resultater:
    print(dings)
```