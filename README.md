---
description: CEXSV
hidden: true
icon: trophy
---

# Analiza traficului de rețea

## Ce este analiza traficului de rețea?

Analiza traficului de rețea implică monitorizarea și examinarea pachetelor de date care circulă printr-o rețea. Acest proces este crucial pentru asigurarea securității, gestionarea performanței și identificarea posibilelor probleme sau anomalii.

#### Importanța Analizei Traficului

* **Securitate**: Identificarea activităților neautorizate sau potențial dăunătoare.
* **Optimizarea performanței**: Detectarea congestiilor și ajustarea resurselor.
* **Diagnosticare**: Localizarea și soluționarea problemelor de conectivitate.

## Tipuri de trafic: trafic legitim vs. trafic malițios

În analiza traficului de rețea, o distincție crucială este făcută între traficul legitim și cel malițios:

* **Trafic legitim**: Acesta include datele generate de utilizatori autentici și aplicații aprobate care utilizează rețeaua conform politicilor de securitate stabilite.
* **Trafic malițios**: Implică activități neautorizate care pot cauza daune rețelei, cum ar fi atacuri de tip DDoS, tentative de phishing sau instalarea de malware. Identificarea acestui tip de trafic este esențială pentru protejarea infrastructurii de rețea.

## Modele de rețea

Modelele de rețea clasifică și oferă o structură pentru protocoalele și standardele de rețea. Un protocol de rețea este un set de reguli logice care definește modul în care dispozitivele și software-ul de rețea ar trebui să funcționeze.&#x20;

Modelul OSI este o încercare de standardizare a comunicațiilor de rețea. Deși nu este folosit efectiv în prezent, a avut un impact major asupra modului în care inginerii de rețea abordează rețelistica, și continuăm să ne referim la el și astăzi.

Standardul definește _ce trebuie să fie făcut_, iar protocolul descrie _cum să fie făcut_.

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Modelul OSI

* Reprezintă 'Open Systems Interconnection' (Interconectarea Sistemelor Deschise). Un model conceptual care categorizează și standardizează diferitele funcții dintr-o rețea.&#x20;
* Creat de ISO (Organizația Internațională pentru Standardizare).&#x20;
* Funcțiile sunt împărțite în 7 'straturi'.&#x20;
* Aceste straturi colaborează pentru a face rețeaua să funcționeze.



<table><thead><tr><th width="393">Layer</th><th>Definitions</th></tr></thead><tbody><tr><td>Application</td><td>This layer is closest to the end user. Interacts with software applications. HTTP and HTTPS are Layer 7 protocols. Identifies communication partners, synchronizes communications etc. <strong>It provides process-to-process communication.</strong></td></tr><tr><td>Presentation</td><td>Data in the application layer is in 'application format'. It needs to be 'translated' to a different format to be sent over the network. The Presentation Layer's job is to translate between application and network formats. One example of a function of the presentation layer is encryption of data as it send, so that only the intended recipient can read it, and of course decryption as it is received. The presentation layer also translates between different application-layer formats, to ensure that the data is in a format the receiving host can understand.</td></tr><tr><td>Session</td><td>Controls dialogues (sessions) between communicating hosts. Establishes, manages, and terminates connections between the local application.</td></tr><tr><td>Transport</td><td>Segments and reassembles data for communications between end hosts. Breaks large pieces of data into smaller segments which can be more easily sent over the network and are less likely to cause transmission problems if errors occurs. <strong>Provide host-to-host communication</strong>.</td></tr><tr><td>Network</td><td><strong>Provides connectivity between end hosts</strong> on different networks (outside the LAN). Provides IPs. Provides path selection between source and destination. Layer 3 provides the means of selecting the best path. Routers operate at Layer 3.</td></tr><tr><td>Data Link</td><td><strong>Provides node-to-node connectivity</strong> and data transfer (for example, PC to switch, switch to router, router to router). Because Layer 2 is adjacent to Layer 1, the physical layer, it also defines how data is formatted for transmission over a physical medium, like, copper UTP cables. Detects and (possibly) corrects Physical Layer errors. Uses Layer 2 addressing, separate from Layer 3 addressing. Switches operate at Layer 2.</td></tr><tr><td>Physical</td><td>Defines physical characteristics of the medium used to transfer data between devices (voltage levels, maximum transmission distances, physical connectors, cable specifications) etc. Digital bits are converted intro electrical (for wired connections) or radio (for wireless connection) signals.</td></tr></tbody></table>

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## TCP/IP

* Model conceptual și set de protocoale de comunicație utilizate în Internet și alte rețele
* **Cunoscut ca TCP/IP** datorită celor două protocoale fundamentale din această suită.
* **Dezvoltat de către Departamentul de Apărare al Statelor Unite** prin DARPA (_Defense Advanced Research Projects Agency_).
* **Structură similară cu modelul OSI**, dar cu mai puține straturi.
* **Modelul utilizat efectiv în rețelele moderne**.

> **NOTĂ**: Modelul OSI influențează în continuare modul în care inginerii de rețea gândesc și discută despre rețele.

## Life of Data

### 1. Nivelul Aplicație (Application Layer)

* **Protocol folosit**: HTTP/HTTPS.
* PC-ul generează un request (ex. GET pentru HTTP) pentru fișierul dorit.
* Utilizatorul interacționează printr-o aplicație, cum ar fi un browser web.

### 2. Nivelul Prezentare (Presentation Layer)

* **Funcție**: Transformarea datelor.
* Requestul este convertit în formatul necesar (de exemplu, text în UTF-8, date criptate în HTTPS).

### 3. Nivelul Sesiune (Session Layer)

* **Funcție**: Gestionarea sesiunilor.
* Este inițiată o sesiune de comunicare între PC și server (ex. prin stabilirea unei conexiuni TCP pentru HTTP).

### 4. Nivelul Transport (Transport Layer)

* **Protocol folosit**: TCP.
* Requestul este segmentat în pachete.
* TCP se asigură că datele ajung corect la destinație, în ordinea corectă, prin numere de secvență și retransmisie.

### 5. Nivelul Rețea (Network Layer)

* **Protocol folosit**: IP.
* Se adaugă adresele IP ale PC-ului și serverului (ex. IPv4 sau IPv6).
* Routerele folosesc aceste adrese pentru a ghida pachetele spre destinație.

### 6. Nivelul Legătură de Date (Data Link Layer)

* **Funcție**: Transmiterea pachetelor între dispozitivele fizice.
* Adresele MAC ale PC-ului și dispozitivului din rețea (ex. router) sunt utilizate pentru livrarea pachetelor.

### 7. Nivelul Fizic (Physical Layer)

* **Funcție**: Transmiterea efectivă a datelor.
* Pachetele sunt convertite în semnale electrice, optice sau radio și transmise prin cablu, fibră optică sau Wi-Fi.

### Procesul invers pe server:

1. Serverul primește pachetele la **nivelul fizic**.
2. Datele sunt reconstruite trecând prin fiecare nivel OSI.
3. La nivelul aplicație, serverul procesează requestul și trimite răspunsul (ex. fișierul cerut) înapoi către PC prin același proces în ordine inversă.

Astfel, fiecare strat al modelului OSI joacă un rol crucial în transmiterea și procesarea datelor dintre PC și server.

## Protocolul IP (Internet Protocol)

Protocolul IP este unul dintre cele mai fundamentale protocoale din stiva de protocoale TCP/IP și are rolul de a asigura rutarea pachetelor de date între dispozitivele conectate la rețea. IP definește structura pachetelor și adresele IP, care sunt folosite pentru a identifica sursa și destinația pachetelor pe internet.

Există două versiuni principale ale protocolului IP: **IPv4** și **IPv6**.

### IPv4 (Internet Protocol version 4)

IPv4 este versiunea de protocol IP care este în continuare cea mai utilizată în rețelele de internet. Acesta utilizează adrese de 32 de biți, ceea ce permite aproximativ 4 miliarde de adrese IP unice.

#### Caracteristici IPv4:

* **Adresa IP**: Formatul standard al unei adrese IPv4 este o secvență de 4 octeți (32 de biți), de obicei reprezentată în notația zecimală punctată, de exemplu: `192.168.1.1`.
* **Adresare**: Adresele IPv4 sunt împărțite în clase: A, B, C, D și E. Cele mai frecvent utilizate sunt clasele A, B și C.
* **Protocolul de rutare**: IPv4 folosește diverse protocoale de rutare, cum ar fi RIP, OSPF și BGP, pentru a transmite pachetele între routere.
* **Lungimea maximă a unui pachet**: Pachetele IPv4 au o dimensiune maximă de 65.535 octeți.
* **Limitări**: IPv4 se confruntă cu o penurie de adrese IP din cauza numărului limitat de adrese disponibile, ceea ce a condus la dezvoltarea IPv6.

#### Exemple de utilizare IPv4:

* Adresa de rețea: `192.168.0.0/24`
* Adresa de gateway: `192.168.0.1`
* Adresa de broadcast: `192.168.0.255`

***

### IPv6 (Internet Protocol version 6)

IPv6 este o versiune mai recentă a protocolului IP, introdusă pentru a rezolva limitările IPv4, în special problema epuizării adreselor IP.

#### Caracteristici IPv6:

* **Adresa IP**: IPv6 folosește adrese de 128 de biți, ceea ce permite o cantitate aproape nelimitată de adrese (aproximativ 340 trilioane de trilioane de trilioane de adrese).
* **Formatul adresei**: Adresele IPv6 sunt scrise în 8 grupuri de câte 4 caractere hexazecimale, separate prin două puncte, de exemplu: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`.
* **Adresare**: IPv6 include mai multe tipuri de adrese, cum ar fi adresele unicast (pentru o singură destinație), multicast (pentru mai multe destinații) și anycast (pentru o destinație apropiată).
* **Protocolul de rutare**: IPv6 folosește protocoale de rutare ca OSPFv3 și BGP4+.
* **Lungimea maximă a unui pachet**: Pachetele IPv6 au o dimensiune maximă de 4.294.967.295 octeți.
* **Autoconfigurare**: IPv6 include caracteristici precum autoconfigurarea, care permite dispozitivelor să obțină adrese IP fără a fi nevoie de un server DHCP.

***

### Diferențe între IPv4 și IPv6

| Caracteristică                    | IPv4                                 | IPv6                                                          |
| --------------------------------- | ------------------------------------ | ------------------------------------------------------------- |
| **Lungimea adresei**              | 32 de biți                           | 128 de biți                                                   |
| **Număr de adrese unice**         | 4 miliarde                           | Aproape nelimitate, 340 undecillioane                         |
| **Formatul adresei**              | X.X.X.X (ex: 192.168.1.1)            | XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX                       |
| **Rata de alocare**               | Limitată (probleme de epuizare)      | Practic nelimitată                                            |
| **Protocolul de autoconfigurare** | Nu există                            | Există (Stateless Address Autoconfiguration)                  |
| **Compatibilitate**               | Majoritatea rețelelor utilizând IPv4 | Necesită tranziție la IPv6, nu este compatibil direct cu IPv4 |

***

### Concluzie

IPv6 este soluția la problemele de scalabilitate și epuizare a adreselor IPv4. Totuși, tranziția de la IPv4 la IPv6 este încă în curs, iar multe rețele continuă să folosească IPv4, având nevoie de soluții de compatibilitate și de tranziție între cele două protocoale.

## Instrumente de analiză a traficului de rețea

### Prezentarea unor instrumente populare

#### 1. Wireshark

* **Descriere**: Wireshark este un analizator de pachete open-source, utilizat pentru captarea și analizarea traficului de rețea.
* **Caracteristici principale**:
  * Suport pentru sute de protocoale de rețea.
  * Interfață grafică intuitivă.
  * Posibilitatea de a aplica filtre avansate pentru analiza datelor.
  * Funcționalități pentru capturare în timp real și analiză offline.
  * Exportarea pachetelor în format `.pcap`.
* **Comenzi frecvent utilizate în Wireshark**:
  * Filtrare pachete:
    * `ip.addr == 192.168.1.1` – afișează pachetele pentru o anumită adresă IP.
    * `tcp.port == 443` – filtrează pachetele pe portul HTTPS.
  * **Follow TCP Stream**: Urmărește fluxurile de date pentru o sesiune TCP specifică.

***

#### 2. tcpdump

* **Descriere**: tcpdump este un utilitar în linie de comandă pentru captarea și analiza pachetelor de rețea.
* **Caracteristici principale**:
  * Performanță excelentă în capturarea datelor brute.
  * Posibilitatea de a filtra pachetele folosind Berkeley Packet Filters (BPF).
  * Compatibilitate pentru scripturi automate de monitorizare.
* **Comenzi frecvent utilizate în tcpdump**:
  * Capturarea traficului general: `tcpdump`
  * Salvarea capturilor într-un fișier: `tcpdump -w captura.pcap`
  * Capturarea pachetelor pentru o anumită adresă IP: `tcpdump host 192.168.1.1`
  * Capturarea traficului pe un port specific: `tcpdump port 80`

***

### Configurarea și utilizarea Wireshark

#### 1. Instalare

* Instalați Wireshark folosind managerul de pachete corespunzător sau descărcați aplicația de pe [wireshark.org](https://www.wireshark.org).

***

#### 2. Captarea pachetelor

1. Lansați Wireshark.
2. Selectați interfața de rețea pe care doriți să capturați traficul.
3. Faceți clic pe **Start** pentru a începe captarea.

***

#### 3. Filtrarea pachetelor

* Utilizați filtre pentru a restrânge datele afișate:
  * `http` – afișează doar traficul HTTP.
  * `tcp.port == 22` – afișează traficul pentru conexiunile SSH.

***

#### 4. Analizarea pachetelor

* Selectați un pachet pentru a vizualiza detaliile despre:
  * Headerul Ethernet/IP/TCP.
  * Conținutul pachetului (payload).
* Folosiți **Follow TCP Stream** pentru a analiza conversațiile complete.

***

#### 5. Exportarea și salvarea capturilor

* Salvați capturile prin **File > Save As** în format `.pcap` pentru analiză ulterioară.

***

## Captarea Pachetelor

### Cum se realizează captarea pachetelor de date în rețea

Captarea pachetelor presupune interceptarea și înregistrarea traficului de rețea. Aceasta implică:

1. **Selectarea interfeței de rețea**:
   * Interfața activă (Ethernet, Wi-Fi) pe care se dorește captarea pachetelor este selectată.
2. **Activarea modului promiscuous**:
   * Interfața de rețea este configurată pentru a intercepta toate pachetele din rețea, indiferent de destinația lor.
3. **Inițierea capturii**:
   * Instrumentele precum **Wireshark** sau **tcpdump** sunt utilizate pentru a înregistra pachetele.
4. **Filtrarea și salvarea datelor**:
   * Se pot aplica filtre pentru capturarea pachetelor relevante (protocol, adresă IP, porturi etc.).
   * Datele capturate pot fi salvate în format `.pcap` pentru analiză ulterioară.

***

### Tipuri de pachete și protocoale

#### 1. **TCP (Transmission Control Protocol)**

**Caracteristici:**

* Protocol orientat pe conexiune.
* Asigură:
  * Livrarea corectă a datelor.
  * Ordinea pachetelor.
  * Detectarea și retransmiterea pachetelor pierdute.

**Structura Headerului TCP:**

| Câmp                | Dimensiune | Descriere                                         |
| ------------------- | ---------- | ------------------------------------------------- |
| Port sursă          | 16 biți    | Portul de unde a fost trimis pachetul.            |
| Port destinație     | 16 biți    | Portul unde trebuie livrat pachetul.              |
| Număr secvență      | 32 biți    | Ordinea pachetelor în flux.                       |
| Număr de confirmare | 32 biți    | Confirmarea recepției datelor anterioare.         |
| Lungime header      | 4 biți     | Dimensiunea headerului.                           |
| Flag-uri            | 6 biți     | Informații despre conexiune (SYN, ACK, FIN etc.). |
| Fereastră           | 16 biți    | Capacitatea de recepție a receptorului.           |
| Checksum            | 16 biți    | Verificarea integrității pachetului.              |

**Procesul 3-Way Handshake:**

1. **SYN**: Clientul trimite un pachet cu flag-ul `SYN` pentru a iniția conexiunea.
2. **SYN-ACK**: Serverul răspunde cu un pachet care include `SYN` și `ACK`.
3. **ACK**: Clientul confirmă cu un pachet care include `ACK`.

**Exemple de utilizare:**

* Transfer de date în aplicații critice (HTTP, FTP, SMTP).

***

#### 2. **UDP (User Datagram Protocol)**

**Caracteristici:**

* Protocol fără conexiune.
* Nu garantează ordinea sau livrarea pachetelor.
* Este utilizat în aplicații unde latența este mai importantă decât fiabilitatea.

**Structura Headerului UDP:**

| Câmp            | Dimensiune | Descriere                              |
| --------------- | ---------- | -------------------------------------- |
| Port sursă      | 16 biți    | Portul de unde a fost trimis pachetul. |
| Port destinație | 16 biți    | Portul unde trebuie livrat pachetul.   |
| Lungime         | 16 biți    | Dimensiunea totală a pachetului UDP.   |
| Checksum        | 16 biți    | Verificarea integrității pachetului.   |

**Exemple de utilizare:**

* Streaming video/audio, DNS, VoIP.

***

#### 3. **ICMP (Internet Control Message Protocol)**

**Caracteristici:**

* Protocol folosit pentru mesaje de control și diagnosticare a rețelei.
* Nu transportă date pentru aplicații.

**Structura Headerului ICMP:**

| Câmp            | Dimensiune | Descriere                                                    |
| --------------- | ---------- | ------------------------------------------------------------ |
| Tip mesaj       | 8 biți     | Tipul mesajului (ex.: `0` - Echo Reply, `8` - Echo Request). |
| Cod mesaj       | 8 biți     | Cod suplimentar pentru tipul de mesaj.                       |
| Checksum        | 16 biți    | Verificarea integrității mesajului.                          |
| Date adiționale | Variabil   | Depind de tipul mesajului.                                   |

**Exemple de utilizare:**

* `ping` pentru verificarea conectivității.
* `traceroute` pentru diagnosticarea rutelor.

***

### Analiza pachetelor în rețea

#### Exemple:

1. **TCP - Descărcare fișier**:
   * Urmărirea unui flux TCP pentru a observa handshake-ul și transferul de date.
   * Confirmarea că toate pachetele au fost livrate și ordonate corect.
2. **UDP - Streaming video**:
   * Observarea traficului cu multe pachete mici, fără retransmisii.
3. **ICMP - Diagnosticare rețea**:
   * Detectarea erorilor de tip "Destination Unreachable" sau "Time Exceeded".

***

## Analiza Pachetelor și Detectarea Comportamentului Malițios

### Analiza Pachetelor

#### 1. Examinarea conținutului pachetelor

Pentru a analiza conținutul pachetelor și a identifica activități neobișnuite, se folosesc instrumente precum **Wireshark**. Procesul implică:

1. **Capturarea traficului de rețea**:
   * Selectați interfața activă și începeți capturarea.
   * Salvați captura într-un fișier `.pcap` pentru analiză ulterioară.
2. **Examinarea detaliilor pachetelor**:
   * Selectați un pachet din listă și inspectați headerul și payload-ul.
   * Observați detalii precum:
     * Adrese IP sursă și destinație.
     * Protocoale utilizate (TCP, UDP, ICMP).
     * Informații de autentificare (dacă sunt transmise în text clar).
3. **Identificarea activităților neobișnuite**:
   * Trafic neautorizat către porturi sensibile (de exemplu, 22, 3389).
   * Fluxuri masive de pachete din aceeași sursă (indiciu pentru atacuri DDoS).
   * Pachete anormale (dimensiuni ciudate sau date corupte).

***

#### 2. Filtrarea și sortarea pachetelor

Pentru a restrânge analiza, folosiți filtre specifice:

**Filtrare în Wireshark:**

| Scop                                   | Filtru Wireshark          |
| -------------------------------------- | ------------------------- |
| Afișarea traficului HTTP               | `http`                    |
| Trafic pentru o anumită adresă IP      | `ip.addr == 192.168.1.10` |
| Pachete trimise către un port specific | `tcp.port == 443`         |
| Trafic ICMP                            | `icmp`                    |

**Sortarea pachetelor:**

* Folosiți coloanele din Wireshark pentru a sorta pachetele după:
  * Adresa IP.
  * Port.
  * Timp.

***

### Detectarea Comportamentului Malițios

#### 1. Identificarea semnelor de atacuri de rețea

**a. Atacuri DDoS:**

* **Semne**:
  * Fluxuri mari de pachete către o singură destinație.
  * Pachete de tip SYN fără ACK corespunzător.
* **Exemplu practic**:
  1. Capturați traficul în rețea.
  2. Filtrați pachetele SYN folosind `tcp.flags.syn == 1 && tcp.flags.ack == 0`.
  3. Verificați dacă există un volum neobișnuit de conexiuni inițiate.

**b. Scanări de porturi:**

* **Semne**:
  * Multiple încercări de conectare pe porturi diferite din partea aceleiași surse.
* **Exemplu practic**:
  1. Aplicați filtrul `ip.src == <adresa_sursă>` în Wireshark.
  2. Analizați traficul pentru diferite porturi.

***

#### 2. Analiza fluxului de date pentru comunicări suspecte

**a. Observarea traficului criptat:**

* Traficul către domenii necunoscute folosind protocoale HTTPS poate fi suspect.
* **Exemplu practic**:
  1. Filtrați traficul HTTPS: `ssl`.
  2. Verificați adresele IP și domeniile utilizând rezolvarea DNS inversă.

**b. Detectarea transferurilor mari de date:**

* Verificați dimensiunile pachetelor pentru a identifica descărcările sau încărcările masive.
* **Exemplu practic**:
  1. Sortați coloana „Length” în Wireshark.
  2. Analizați sesiunile care implică pachete mari.

| **Service**                                      | **Port** |
| ------------------------------------------------ | -------- |
| **FTP (File Transfer Protocol)**                 | 21       |
| **SFTP (SSH File Transfer Protocol)**            | 22       |
| **SSH (Secure Shell)**                           | 22       |
| **Telnet**                                       | 23       |
| **SMTP (Simple Mail Transfer Protocol)**         | 25       |
| **DNS (Domain Name System)**                     | 53       |
| **HTTP (Hypertext Transfer Protocol)**           | 80       |
| **HTTPS (HTTP Secure)**                          | 443      |
| **FTP Data (Passive Mode)**                      | 20       |
| **IMAP (Internet Message Access Protocol)**      | 143      |
| **POP3 (Post Office Protocol v3)**               | 110      |
| **TFTP (Trivial File Transfer Protocol)**        | 69       |
| **MySQL Database**                               | 3306     |
| **RDP (Remote Desktop Protocol)**                | 3389     |
| **LDAP (Lightweight Directory Access Protocol)** | 389      |
| **SNMP (Simple Network Management Protocol)**    | 161      |
| **NTP (Network Time Protocol)**                  | 123      |

### Exerciții Practice

#### Exercițiul 1: Detectarea unui atac DDoS

1. Capturați traficul rețelei pentru 5-10 minute.
2. Utilizați filtrul `tcp.flags.syn == 1 && tcp.flags.ack == 0`.
3. Identificați sursele care inițiază multe conexiuni și verificați dacă există răspunsuri de tip ACK.

#### Exercițiul 2: Scanare de porturi

1. Capturați traficul într-un scenariu de test (folosiți un scanner de porturi, precum **nmap**, pentru a genera date).
2. Aplicați filtrul `ip.src == <adresa_sursă>`.
3. Verificați numărul de porturi accesate.

#### Exercițiul 3: Analiza fluxurilor criptate

1. Monitorizați traficul HTTPS folosind filtrul `ssl`.
2. Identificați traficul către IP-uri neobișnuite sau domenii necunoscute.
3. Verificați dimensiunea sesiunilor pentru posibile transferuri mari de date.

#### Exercițiul 4: Observarea traficului ICMP

1. Rulați comanda `ping` către un server cunoscut și capturați traficul.
2. Utilizați filtrul `icmp` în Wireshark.
3. Analizați tipul și codul mesajelor ICMP (Echo Request și Echo Reply).

***
