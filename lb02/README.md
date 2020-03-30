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

### 1.3 GitHub.com

Als alles erstes habe ich unter https://github.com einen Account mit meiner Schuladresse (ricardo.bras@edu.tbz.ch) erstellt. Danach wurde ich von GitHub durch eine Konfiguration von GitHub durchgeführt. Diese Bestand unter anderem aus dem Erstellen meines ersten Repository "brasricardo\M300-Services". Nachdem die Konfiguration vollendet war habe ich mein Account noch zu einem Pro-Account heraufgestuft, mithilfe der Schuladresse konnte ich dies kostenlos machen.

### 1.4 GitHub Desktop

Ich habe von GitHub Desktop per Zufall über eine Online-Anleitung zu Vagrant und GitHub zum ersten mal gehört. Da ich diese Features noch interessant fand habe ich es mir heruntergeladen und getestet. Die Software ist sehr nützlich da man vieles Verknüpfen kann, wie z.B. Visual Studio Code. Man kann mit nur einem Klick das ganze Repository auf Visual Studio Code öffnen und direkt dort bearbeiten. Nachdem man es dort bearbeitet hat kann man die Files speichern und wieder zu GitHub Desktop wechseln dieser hat dann gemerkt, dass es Änderungen in den Files gab und zeigt diese an mit einem "Commit-Button". Mit diesem kann man dann die Änderungen "Commiten" und dann noch "pullen", damit diese dann auch im Online-Repo geändert werden. Die Software zeigt ebenfalls an, falls sich etwas Online geändert hat, welches sich lokal noch nicht hat und zeigt dann einen "Push-Button" an, mit dem man die Online-Änderungen auf das lokale Repository speichern kann. 

![GitHub Desktop Commit](Git-Hub_Desktop-Commit.PNG)

![GitHub Desktop Push](Git-Hub_Desktop-Push.PNG)

### 1.5 Git-Client

Mit Git-Client konnte ich die Git-Befehle auf meinem lokalen Client auf einem Bash eingeben. Diese Bash ist nichts anderes als eine Linux-Bash mit der man Git-Befehle ausführen kann. Ich habe sie nicht oft gebraucht, da ich hauptsächlich mit GitHub Desktop gearbeitet habe. Jedoch habe ich sie am Anfang für die Erstellung eines Private & Public Key gebraucht, welches ich dann mit GitHub verbunden habe, damit man sich nicht immer anmelden muss

```
$  ssh-keygen -t rsa -b 4096 -C "beispiel@beispiel.com"     #Danach der Konfiguration folgen und abschliessen.
```

Nachdem die Keys erstellt wurden musste ich den Inhalt des Public-Keys kopieren und dies in meinem GitHub Profil unter Settings und SSH & GPS Keys einfügen und abspeichern.


### 1.6 Vagrant

Ich habe gelernt, dass mithilfe von Vagrant die Installation und erst Konfiguration von VMs sehr einfach und unkompliziert funktionieren kann. Damit ich Vagrant nutzen konnte habe ich es von https://www.vagrantup.com/ heruntergeladen und auf meinem Client installiert. Nachdem die Installation abgeschlossen war konnte man bereits loslegen. Ich habe in mein Verzeichnis gewechselt, welches sich unter C:\Users\Ricardo\M300 befindet. Dieses Verzeichnis ist ein "Clone" meines GitHub-Repository. 

Mit Vagrant konnte ich dann eine VM erstellen, konfigurieren und starten.

```
$ vagrant init ubuntu/xenial64        #Vagrantfile erzeugen
$ vagrant up --provider virtualbox    #Virtuelle Maschine erstellen & starten
```

Sobald die Installation zu ende ist und keine Fehler vorgekommen sind, läuft die Maschine und man kann per CMD eine SSH-Verbindung zu dieser herstellen.

```
vagrant ssh
```

Falls man eine bereits erstellte VM löschen möchte kann dies ganz einfach mit folgendem Befehl durchgeführt werden:

```
vagrant destroy -f
```

Der Inhalt des Vagrantfiles (Konfiguration) kann grob unter dem Punkt 2.Konfiguration und https://github.com/brasricardo/M300-Services/tree/master/lb02/lam gefunden werden.

### 1.7 Virtual Box

Damit ich mein Projekt realisieren konnte musste ich VirtualBox installieren für die Virtualisierung einer Maschine, welche mithilfe von Vagrant erstellt werden würde. VirtualBox konnte ich ganz einfach über https://www.virtualbox.org/ herunterladen und dann mit einem GUI installieren. Nach der Installation musste ich keine weitere Schritte in VirtualBox vornehmen, da sich Vagrant um alles kümmerte.

### 1.8 Visual Studio Code

Visual Studio Code ermöglicht es mir mein Vagrant- oder die README-Files ganz einfach über einen Code-Editor übersichtlich und gut zu gestalten und schreiben. 

## 3. Leistungsbeurteilung 01 (LB01)
Diese Beurteilung beinhaltet keine Dokumente, da es sich hier um eine Theorieprüfung handelt.

## 4. Leistungsbeurteilung 02 (LB02)
Hier können wir ein eigenes Projekt "Virtuelle Maschine mit einem Vagrant-File automatisieren nach eigener Wahl". Die Projektwahl musste einfach dem [Bewertungsraster](https://bscw.tbz.ch/bscw/bscw.cgi/31351309?op=preview&back_url=31350371) entsprechen. Hierfür gab es ein paar [Beispiele](https://github.com/mc-b/M300/tree/master/vagrant).

Ich habe mich dazu entschieden eine Umgebung mit zwei virtuelle Maschinen zu erstellen. Auf der einen Maschine wurde ein Apache Webserver und ein Datenbankverwaltugnstool "Adminer" installiert und konfiguriert. Die zweite Maschine wurde als ein Datenbankserver installiert und konfiguriert.

Die Dokumentation zu diesem Projekt kann [hier](https://github.com/brasricardo/M300-Services/tree/master/lb02) gefunden werden.

## 5. Leistungsbeurteilung 03 (LB03)
Hier können wir ein eigenes Projekt "Virtuelle Maschine mit einem Docker-File automatisieren nach eigener Wahl". Die Projektwahl musste einfach dem [Bewertungsraster](https://bscw.tbz.ch/bscw/bscw.cgi/d31406123/BewertungsrasterLB3.pdf) entsprechen.



Die Dokumentation zu diesem Projekt kann [hier](https://github.com/brasricardo/M300-Services/tree/master/lb03) gefunden werden.
