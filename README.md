# M300
## 1. Description
Modulnummer:    300
Modulname:      Plattformübergreifende Dienste nach Vorgabe für eine heterogene Systemumgebung konfigurieren, in Betrieb nehmen, testen und freigeben.

Das Modul 300 dreht sich rund um Plattformübergreifende Dienste. Genauere Modulinformationen können auf der Internetseite der ICT-Berufsbildung gelesen werden.
[ICT-Berufsbildung Modulbaukasten](https://cf.ict-berufsbildung.ch/modules.php?name=Mbk&a=20101&cmodnr=300&noheader=1)

## 2. Information

### 2.1 Github
Der IoTKit ist ein NXP FRDM-K64F Board kompatibles Board.

Das Board besteht aus einer Kombination zwischen des NXP FRDM-K64F Board und einzelner Shields. Aus diesem Grund sind Pin's doppelt belegt und wo dies der Fall ist kann nur eine Funktion genutzt werden.

![IoTKitV3 K64F Board](IoTKit.png)

**Board Features**
- Bluetooth® V4.1 module (SPBTLE-RF)
- Proximity sensor, gesture and ambient light sensing (ALS) module (VL6180X)
- low power, high sensitivity, RED, GREEN and BLUE color light sensor (RGB)
- Hall Sensor
- NFC/RFID Reader
- Capacitive digital sensor for relative humidity and temperature (HTS221)
- High-performance 3-axis magnetometer (LIS3MDL)
- 3D accelerometer and 3D gyroscope (LSM6DSL)
- Wi-Fi® module ESP8266
- OpenSDA USB Debug and Programming adapter
- Reset Button
- SPI Header
- GND/5V/Signal Arduino Pins D13 - D2
- I2C Header
- MOSFet
- Expansion connectors: Arduino™ Uno V3
- SWDAP
- ARM Cortex M4 K64F MCU
- Grove Header
- OLED Display
- 4 LED an GPIO D10 - D13
- H-Bridge
- Buzzer
- Stepper Driver
- 12 Volt Power Supply
- Charger 5V from batt.
- GND/5V/Signal Arduino Pins A0 - A5
- UART Header
- Encoder Switch

### 2.2 Github Client

#### 2.2.1 Hall-Sensor
Ein Hall-Sensor misst Magnetfelder mithilfe des Hall-Effekts. 

Der Hall-Sensor wird genützt zur Erfassung eines Permanentmagnetes, somit kann der Nordpol und Südpol des Magneten bestimmt werden.

**Anwendungen**
- Alarmanlagen
- Geschwindigkeitsmessung
- Turbinendrezahlerfassung

#### 2.2.2 PIR Sensor
Der PIR Sensor (passive infrared) ist eine Art von Bewegungsmelder. Er ist einer der am häufigsten verwendeten Bewegungsensoren. Bei Winkeländerungen reagiert er am optimalsten, dies bedeutet, wenn eine Person am Sensor vorbeigeht, reagiert dieser.

**Anwendungen**
- Einschalten einer Beleuchtung
- Auslösung eines Alarms
  
#### 2.2.3 Ultraschall Abstandmesser
Die Entfernung eines Objektes kann mit einem Ultrashall Abstandmesser gemessen werden.

Gestartet wird der Erkennungszyklus mittels eines Impluses von mindestens 10 Mikrosekunden. Dieser Impuls wird durch die "pulse trigger" Leitung geschickt. Der Sensor sendet eine Serie von acht Schaltimpulsen, sobald diese wieder tief wird. Die Leitung wird wieder auf tief gesetzt, sobald das erste Ultraschallecho empfangen wurde. Mithilfe dieser Zeitspanne kann der Abstand zwischen den Objekten bestimmt werden.

**Anwendungen**
- Erkennen von Hindernissen bei Robotern

#### 2.2.4 Temperatur Sensor
Die Temperatur und relative Luftfeuchtigkeit kann gleichzeitig mithilfe eines DHT11 (multifunktionaler Sensor) gemessen werden. Es ist ihm möglich bei einer Luftfeuchtigkeit zwischen 20% und 90% und einer Temperatur zwischen 0° bis 50° Celsius zuverlässige Werte zu liefern.

**Anwendungen**
- Überwachung von Temperatur und Luftfeuchtigkeit
- Ein- / Ausschalten der Heizung, Klimaanlage, etc.

### 2.3 Virtual Box

### 2.4 Vagrant

### 2.5 Visual Studio Code

## 3. Leistungsbeurteilung 01 (LB01)
Diese Beurteilung beinhaltet keine Dokumente, da es sich hier um eine Theorieprüfung handelt.

## 4. Leistungsbeurteilung 02 (LB02)
Hier können wir ein eigenes Projekt "Virtuelle Maschine mit einem Vagrant-File automatisieren nach eigener Wahl". Die Projektwahl musste einfach dem [Bewertungsraster](https://bscw.tbz.ch/bscw/bscw.cgi/31351309?op=preview&back_url=31350371) entsprechen. Hierfür gab es ein paar [Beispiele](https://github.com/mc-b/M300/tree/master/vagrant).

Ich habe mich dazu entschieden eine Umgebung mit zwei virtuelle Maschinen zu erstellen. Auf der einen Maschine wurde ein Apache Webserver und ein Datenbankverwaltugnstool "Adminer" installiert und konfiguriert. Die zweite Maschine wurde als ein Datenbankserver installiert und konfiguriert.

Die Dokumentation zu diesem Projekt kann [hier](https://github.com/brasricardo/M300-Services/tree/master/lb02) gefunden werden.

## 5. Leistungsbeurteilung 03 (LB03)
Hier können wir unser eigenes Projekt, welches bei LB02 durchgeführt wurde, mit einem Gateway erweitern und den Gateway mit einem (Cloud) Dienst verbinden.

Die Dokumentation zu diesem Projekt kann [hier](https://github.com/Wind-net/M242-IOT/tree/master/lb03) gefunden werden.
