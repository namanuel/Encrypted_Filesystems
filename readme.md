# Verschlüsseltes FileSystem mit DM-Crypt und LUKS 🍹

# Inhaltsverzeichnis

<!-- vim-markdown-toc GFM -->

* [Vorbereitung und Software](#vorbereitung-und-software)
  * [Aufgabenstellung](#aufgabenstellung)
  * [Ablauf](#ablauf)
  * [Software / Dependencies](#software--dependencies)
* [Theorie](#theorie)
  * [Festplattenverschlüsselung](#Festplattenverschlüsselung)
  * [Device Mapper](#Device Mapper)
  * [DM-Crypt](#DM-Crypt)
  * [LUKS](#LUKS)
* [Installation](#installation)
  * [cryptsetup mit LUKS](#cryptsetup-mit-luks)
  * [Dependencies](#dependencies)
    * [popt](#popt)
  	* [device mapper](#device-mapper)
* [Konfiguration](#konfiguration)
* [Changelog - VMware-Image](#changelog---vmware-image)
  * [Changelog](#changelog)
  * [VMware](#vmware)
    * [Parition](#parition)
* [Evaluierung / Testläufe](#evaluierung--testläufe)
  * [cryptsetup](#cryptsetup)
* [Diskussion und eigene Bewertung](#diskussion-und-eigene-bewertung)
* [Zusammenfassung](#zusammenfassung)
* [Quellen](#quellen)

<!-- vim-markdown-toc -->

## Vorbereitung und Software

### Aufgabenstellung

Beispielgebend für unterschiedliche Arten der Festplattenverschlüsselung wird DM-Crypt behandelt. DM-Crypt verschlüsselt Container/Partitionen und ist über virtuelles Blockdevice (/dev/mapper) ansprechbar, das als Filesystem gemountet werden kann. LUKS (Linux Unified Key Setup) dient dabei zum Schlüsselmanagement. Es ist daher DM-Crypt & LUKS in der vorgegebenen Slackware Distribution (VMware Image) einzukompilieren, zu konfigurieren und der Betrieb zu evaluieren. Im Zuge der Präsentation ist dies abschließend live vorzuführen.

---

### Ablauf

1. cryptsetup mit LUKS und Dependencies einkompilieren
2. Kernel Einstellungen ändern und neuen Kernel kompilieren/erstellen
3. Partition zu VM hinzufügen
4. Verschlüsselung der Partition mit cryptsetup und LUKS

---

### Software / Dependencies


**Dependencies Tree**

<img src="media/dependenciestree.jpg"></img>
	

## Theorie

### Festplattenverschlüsselung

---

Allgemein unter Festplattenverschlüsselung bzw. verschlüsselt Filesystem versteht man das Verschlüsseln einer Festplatte oder einer einzelnen Partition, um den unbefugten Zugriff auf sensible Daten zu verhindern.
Wichtig ist das die zum Booten verwendeten Daten unverschlüsselt bleiben da man sonst einen speziellen Bootmanager braucht zum Entschlüsseln braucht.

Meistens werden die Festplatten mit einem Passworts verschlüsselst, es gibt aber auch alternative Hardwareunterstützte Authentisierung mittels Security-Token, Fingerabdruck, PIN-Eingabe oder Chipkarten.

Durch den Zusätzlichen Rechenaufwand der Verschlüsselungsalgorithmen kann es zu Performanceinbüßungen kommen, der Datendurchsatz sinkt gegenüber unverschlüsselter Datenträger.
Eine Lösung wäre es einen schnelleren Prozessor zu verwenden oder durch die Verwendung von vorher genannten Hardware wie Security Token.

Es gibt verschiedene Softwaren für die unterschiedlichen Betriebssyteme um die Festplatte zu verschlüsseln:

- Windows: Bitlocker
- Linux: Loop-AES und DM-Crypt mit LUKS
- macOS: FileVault



### Device Mapper

---

Der Device Mapper wird hauptsächlich für den Logical Volume Manager (LVM) und für Geräteverschlüsselung verwendet. 
Er erlaubt die Erzeugung virtueller blockorientierter Geräte, indem er deren Adressbereich auf andere blockorientierte Geräte oder spezielle Funktionen abbildet.
Blockorientierte Geräte übertragen Daten in Blöcken und werden oft für die parallele Datenübertragung genutzt.


### DM-crypt

---

DM-Crypt ist ein Kryptografie Modul des Device Mappers im Linux Kernel. 


Es können verschiedene Algorithmen verwendet werden, wie z.B.:
- Mehrere aes versionen
	- aes-cbc-essiv:sha256 
	- aes-xts-plain64 
	- aes:64-cbc-lmk 
- Twofish
- Serpent

Mit dm crypt können wir mit dem verschiedenen Algorithmen unsere Daten ver- und entschlüsseln, das kann man auf beliebige Gerätedateien anwenden, meisten auf Paritionen, Festplatten oder logischen Laufwerken. 

Es wird eine zusätzliche Schicht zwischen verschlüsselten Rohdaten und dem Dateisystem aufgebaut. Für den Benutzer ist der Prozess vollkommen Transparent, deshalb wird es oft für die Festplattenverschlüsselung verwendet. 


### LUKS

---

LUKS steht für Linux Unified Key Setup.

Erweitert die verschlüsselte Datei um eine Header datei, in dem Metadaten sowie bis zu acht Schlüssel gespeichert werden
Zusätzlich kann mit der Hilfe von LUKS der Schlüssel bzw. Einer der acht Schlüsseln jederzeit geändert oder gelöscht warden, ohne die verschlüsselten Daten neu umschreiben zu müssen.

Ein Nachteil von LUKS ist, dass im Header stehen ziemlich viele Informationen z.b. eine Klartextkennung, den verwendeten Verschlüsselungs- und Hash-Algorithmus und die Größe des Masterschlüssels.
Das führt dazu das LUKS leicht zu verwalten ist, aber eben auch gegenüber Dritte oder Angreiferprogramme leicht erkennbar ist.


## Installation

### cryptsetup mit LUKS

```
tar xf cryptsetup-luks-1.0.4.tar.gz
cd cryptsetup-luks-1.0.4
./configure --prefix=/usr
make
make install
```

### Dependencies

#### popt

```
tar xf popt-1.18.tar.gz
cd popt-1.18
./configure --prefix=/usr
make
make install
```

### device mapper

```
tar xf device-mapper.1.02.28.tar.gz
cd device-mapper.1.02.28
./configure --prefix=/usr
make
make install
```

## Konfiguration

TODO: hab nicht wirklich was zum Konfigurieren außer Partition erstellen

## Changelog - VMware-Image

### Changelog

tar upgedatet
TODO:
und andere

### VMware

#### Parition

TODO: zeigen wie man eine Partition einfügt

## Evaluierung / Testläufe

### cryptsetup 

TODO:
Bens Idee ein Ablaufdiagramm
Befehle auflisten, wie man eine eingefügte Partition mountet und verschlüsselt

## Diskussion und eigene Bewertung 

alte Slackware => alte cryptsetup Version, macht vieles leichter

## Zusammenfassung

TODO: kurze Zusammenfassung, na no na ned ^^

## Quellen

Links ^^



# sources and further reading

* [binutils] GNU Binary Utilities Documentation. (2002). Retrieved from http://www.gnu.org/software/binutils/manual/
* [c11] ISO, & IEC. (2010). ISO/IEC 9899:201x, International Standard Programming languages — C, Committee Draft (N1570 ed.). ISO/IEC. Retrieved from http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
* [compsec] Stallings, W. (2018). Computer Security: Principles and Practice, Global Edition (4th ed.). Pearson.
* [edvac] Von Neumann, J., & Godfrey, M. D. (1993). First Draft of a Report on the EDVAC. IEEE Annals of the History of Computing, 15(4), 27–75. https://doi.org/10.1109/85.238389
* [inteldev] Intel. (2011). Intel 64 and IA-32 Architectures Software Developer’s Manual Combined Volumes. System, 3(253665). https://doi.org/10.1109/MAHC.2010.22
* [pracbin] Andriesse, D., & Francisco, S. (2018). PRACTICAL BINARY ANALYSIS Build Your Own Linux Tools for Binary Instrumentation, Analysis, and Disassembly (2nd ed.). No Starch Press.
* [x64beg] Hoey, J. Van. (2019). Beginning x64 Assembly Programming: From Novice to AVX Professional Paperback (1st ed.). Apress.
