# M300

## 1. Information

### 1.1 Beschreibung
Für die Durchführung meiner LB02 habe ich folgende Dienste genützt:

- GitHub.com
- GitHub Desktop
- Git-Client (SSH-Key)
- Vagrant
- VirtualBox
- Visual Studio Code


### 1.2 Netzwerkplan

![Netzwerkplan LB02](Netzwerkplan_M300.png)

### 1.3 Virtual Box

Damit ich mein Projekt realisieren konnte musste ich VirtualBox installieren für die Virtualisierung einer Maschine, welche mithilfe von Vagrant erstellt werden würde. VirtualBox konnte ich ganz einfach über https://www.virtualbox.org/ herunterladen und dann mit einem GUI installieren. Nach der Installation musste ich keine weitere Schritte in VirtualBox vornehmen, da sich Vagrant um alles kümmerte.

### 1.4 Vagrant

Ich habe gelernt, dass mithilfe von Vagrant die Installation und erst Konfiguration von VMs sehr einfach und unkompliziert funktionieren kann. Damit ich Vagrant nutzen konnte habe ich es von https://www.vagrantup.com/ heruntergeladen und auf meinem Client installiert. Nachdem die Installation abgeschlossen war konnte man bereits loslegen. Ich habe in mein Verzeichnis gewechselt, welches sich unter C:\Users\Ricardo\M300 befindet. Dieses Verzeichnis ist ein "Clone" meines GitHub-Repository. 

Mit Vagrant konnte ich dann eine VM erstellen, konfigurieren und starten.

```
$ vagrant init ubuntu/xenial64        #Vagrantfile erzeugen
$ vagrant up --provider virtualbox    #Virtuelle Maschine erstellen & starten
```

### 1.5 Visual Studio Code

Visual Studio Code ist eine Software, welche als source code editor gebraucht wird. Das spezielle an diesem Editor ist, dass es eine menge von verschiedenen Programmiersprachen wie z.B. Java, Javasprict, C++, usw. unterstützt.

## 3. Leistungsbeurteilung 01 (LB01)
Diese Beurteilung beinhaltet keine Dokumente, da es sich hier um eine Theorieprüfung handelt.

## 4. Leistungsbeurteilung 02 (LB02)
Hier können wir ein eigenes Projekt "Virtuelle Maschine mit einem Vagrant-File automatisieren nach eigener Wahl". Die Projektwahl musste einfach dem [Bewertungsraster](https://bscw.tbz.ch/bscw/bscw.cgi/31351309?op=preview&back_url=31350371) entsprechen. Hierfür gab es ein paar [Beispiele](https://github.com/mc-b/M300/tree/master/vagrant).

Ich habe mich dazu entschieden eine Umgebung mit zwei virtuelle Maschinen zu erstellen. Auf der einen Maschine wurde ein Apache Webserver und ein Datenbankverwaltugnstool "Adminer" installiert und konfiguriert. Die zweite Maschine wurde als ein Datenbankserver installiert und konfiguriert.

Die Dokumentation zu diesem Projekt kann [hier](https://github.com/brasricardo/M300-Services/tree/master/lb02) gefunden werden.

## 5. Leistungsbeurteilung 03 (LB03)
Hier können wir ein eigenes Projekt "Virtuelle Maschine mit einem Docker-File automatisieren nach eigener Wahl". Die Projektwahl musste einfach dem [Bewertungsraster](https://bscw.tbz.ch/bscw/bscw.cgi/d31406123/BewertungsrasterLB3.pdf) entsprechen.



Die Dokumentation zu diesem Projekt kann [hier](https://github.com/brasricardo/M300-Services/tree/master/lb03) gefunden werden.
