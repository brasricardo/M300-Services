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

## 2. Dokumentation

### 2.1 Vagrantfile

*Database*
```
# Every Vagrant virtual environment requires a box to build off of.
  config.vm.define "database" do |db|
    db.vm.box = "ubuntu/xenial64"
	db.vm.provider "virtualbox" do |vb|
	  vb.memory = "512"  
	end
    db.vm.hostname = "db01"
    db.vm.network "private_network", ip: "192.168.55.100"
    # MySQL Port nur im Private Network sichtbar
	# db.vm.network "forwarded_port", guest:3306, host:3306, auto_correct: false
	#path: "db.sh"
	  db.vm.provision "shell", inline: <<-SHELL
	  sudo apt update
	  sudo apt upgrade

		# ---------------------------------------
		#          MySQL Setup
		# ---------------------------------------

		# Setting MySQL root user password root/root
		sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password Admin1234'
		sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password Admin1234'


		# Installing packages
		sudo apt-get install -y mysql-server mysql-client

		# Allow External Connections on your MySQL Service
		sudo sed -i -e 's/bind-addres/#bind-address/g' /etc/mysql/mysql.conf.d/mysqld.cnf
		sudo sed -i -e 's/skip-external-locking/#skip-external-locking/g' /etc/mysql/mysql.conf.d/mysqld.cnf
		mysql -u root -proot -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Admin1234' WITH GRANT OPTIONS; CREATE USER 'database'@'%' IDENTIFIED BY 'DBuser1234'; GRANT SELECT ON *.* TO 'database'@'%' IDENTIFIED BY 'DBuser1234; FLUSH privileges;"
		sudo service mysql restart
		# create client database
		mysql -u root -pAdmin1234 -e "CREATE DATABASE Test;"


		# Firewall configuration
		sudo ufw -f enable
		sudo ufw deny out to any
		sudo ufw allow from 192.168.55.1 to any port 22
        sudo ufw allow from 192.168.55.101 to any port 22
		sudo ufw allow from 192.168.55.101 to any port 3306
	SHELL
  end
```

Mit diesem Script wurde die Erstellung und die Konfiguration von einem MySQL-Server und Firewalleinstellungen durchgeführt. Für den Databaseserver wurde ebenfalls noch ein SSH-Key erstellt, damit dies eine sichere Verbindung zum Webserver aufbauen, dies wurde jedoch manuell durchgeführt (Weiteres kann unter Sicherheitsmassnahmen gefunden werden). Am Anfang des Scriptes wurden ein paar VM-Komponenten definiert, wie z.B. Name der VM, was für ein Betriebssystem, wie viel memory, IP-Adresse und forwarded Ports. Im nächsten Teil wird noch gesorgt, dass das Betriebssystem auf dem neustem Stand ist bevor die Installation und Konfigruation durchgefürht wird. Nachdem das System auf dem neustem Stand ist wird noch das Root Passwort auf "Admin1234" definiert und danach kann die Installation beginnen. Nach der Installation werden noch ein paar Einträge in der Konfig Datei von MySQL geändert, damit man eine Verbindung von extern zum Server aufbauen kann. Zudem wird dem "root" alle Rechte von überall gegeben und ein zusätzlicher user "database" wird erstellt mit nur leserechte. Danach wird der ganze Service neugestartet und eine "Test"-DB wird erstellt. Zum Schluss wird noch die Firewall konfiguriert, damit eine gewisse Sicherheit gegeben ist (Weitere Infromation/ Dokumentation unter Sicherheitsmassnahmen).


*Webserver*
```
config.vm.define "web" do |web|
    web.vm.box = "ubuntu/xenial64"
    web.vm.hostname = "web01"
    web.vm.network "private_network", ip:"192.168.55.101"
	web.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
	web.vm.provider "virtualbox" do |vb|
	  vb.memory = "512"  
	end
	# Braucht es hier nicht, weil Standardseite    
  	# web.vm.synced_folder ".", "/var/www/html"  
  	# Wird nur in 1. VM gemountet, Fehler?
  	web.vm.synced_folder ".", "/vagrant"  	
	web.vm.provision "shell", inline: <<-SHELL
		sudo apt-get update
        sudo apt -y upgrade
		sudo apt-get -y install debconf-utils apache2 nmap
		sudo apt-get -y install php libapache2-mod-php php-curl php-cli php-mysql php-gd mysql-client  
		
		#SSL-Certificate
		sudo mkdir /etc/apache2/ssl
		cd /etc/apache2/ssl

		sudo openssl genrsa -out apache.key 4096
		sudo openssl req -new -x509 -key apache.key -out apache.cert -days 3650 -subj /CN=example.com

		sudo sed -i '14iSSLEngine on' /etc/apache2/sites-enabled/000-default.conf
		sudo sed -i '15iSSLCertificateFile /etc/apache2/ssl/apache.cert' /etc/apache2/sites-enabled/000-default.conf
		sudo sed -i '16iSSLCertificateKeyFile /etc/apache2/ssl/apache.key' /etc/apache2/sites-enabled/000-default.conf

		sudo sed -i '1s/.*/<VirtualHost *:433>/' /etc/apache2/sites-enabled/000-default.conf

		sudo sed -i '1i<VirtualHost *:80>' /etc/apache2/sites-enabled/000-default.conf
		sudo sed -i '2iServerName example.com' /etc/apache2/sites-enabled/000-default.conf
		sudo sed -i '3iDocumentRoot /var/www/html' /etc/apache2/sites-enabled/000-default.conf
		sudo sed -i '4iRedirect permanent / https://192.168.55.101/' /etc/apache2/sites-enabled/000-default.conf
		sudo sed -i '5i</VirtualHost>' /etc/apache2/sites-enabled/000-default.conf

		sudo a2enmod ssl
		sudo service apache2 restart

		# Firewall configuration
		sudo ufw -f enable
		sudo ufw deny out to any
		sudo ufw allow 80/tcp
        sudo ufw allow 433/tcp
        sudo ufw allow from 192.168.55.100 to any port 22
		sudo ufw allow from 192.168.55.1 to any port 22

		# Admininer SQL UI 
		sudo mkdir /usr/share/adminer
		sudo wget "http://www.adminer.org/latest.php" -O /usr/share/adminer/latest.php
		sudo ln -s /usr/share/adminer/latest.php /usr/share/adminer/adminer.php
		echo "Alias /adminer.php /usr/share/adminer/adminer.php" | sudo tee /etc/apache2/conf-available/adminer.conf
		sudo a2enconf adminer.conf 
		sudo a2enmod cgi
		sudo cp /vagrant/rest /vagrant/restsql /usr/lib/cgi-bin/ && sudo chown www-data /usr/lib/cgi-bin/rest* && sudo chmod 755 /usr/lib/cgi-bin/rest*
		sudo mkdir -p  /var/www/html/data && sudo chown www-data:www-data /var/www/html/data 
		sudo service apache2 restart 
	  echo '127.0.0.1 localhost web01\n192.168.55.100 db01' > /etc/hosts
SHELL
	end  
 end
```
Mit diesem Script wurde die Erstellung und die Konfiguration von einem Apache Webserver mit Adminer und Firewalleinstellungen durchgeführt. Da beim Databaseserver ein SSH-Key generiert wurde wird der public key hier im Webserver eingelesen, damit die sichere Verbindung garantiert werden kann, dies aber ebenfalls manuell (Weiteres kann unter Sicherheitsmassnahmen gefunden werden). Am Anfang des Scriptes wurden ein paar VM-Komponenten definiert, wie z.B. Name der VM, was für ein Betriebssystem, wie viel memory, IP-Adresse und forwarded Ports. Im nächsten Teil wird noch gesorgt, dass das Betriebssystem auf dem neustem Stand ist bevor die Installation und Konfigruation durchgefürht wird. Nachdem das System auf dem neustem Stand werden noch ein paar Sachen für den Apache-Dienst installiert, weie z.B. php. Nach dieser Installation geht es weiter mit dem absichern der Verbindung zum Webserver mithilfe von einem SSL-Zertifikat. Es wird ein Ordner erstellt für die Zertifikate, danach werden diese generiert und sobald diese generiert sind werden noch die Konfigurationsdatei des apache bearbeitet. In dieser Datei wird jeglich der Ort der Zertifikate bestummen und eine Weiterleitung von HTTP zu HTTPS wird noch gemacht. Zum Schluss der Konfiguration des Zertifikates wird noch ein SSL Apachemodul aktiviert und der Apache-Dienst wird neugestartet. Als nächstes wird noch die Firewall konfiguriert, damit eine gewisse Sicherheit gegeben ist (Weitere Infromation/ Dokumentation unter Sicherheitsmassnahmen). Im letzten Schritt wird das Datenbankverwaltungstool Adminer installiert. Dieser wird mithilfe des wget-Befehles heruntergeladen und mit den darauf folgenden Schritten installiert. Am Schluss werden noch die richtigen Rechte den entsprechenden Dateien verteilt und dann wird der Apache-Dienst nochmals neugestartet.

### 2.2 Sicherheihtsmassnahmen

#### 2.2.1 Benutzerrechte MySQL-User

Um die Sicherheit des MySQL-Server ein wenig zu erhöhen wurde ein weiterer User erstellt, welcher nur über lese Rechte verfügt. Damit will man sicherstellen, dass nicht jeder Schreibrechte oder Vollzugriff erhält, wenn er es nicht für seine "Arbeit" braucht.

#### 2.2.2 Firewall Database

Die Firewall der Database enthält folgende vier Regeln:

```
sudo ufw deny out to any
		
```
Diese Regel blockt den ganzen Verkehr nach aussen, da der Databaseserver keine Internetverbindung braucht und nur als Databaseserver gebraucht wird, braucht er nicht viele Ausnahmenregel. Sondern nur die folgenden drei:

```
sudo ufw allow from 192.168.55.1 to any port 22
        
```

Diese Regel ermöglicht es dem Host (Client) mit der IP-Adresse 192.167.55.1 eine SSH-Verbindung aufzubauen.

```
sudo ufw allow from 192.168.55.101 to any port 22
		
```

Diese Regel ermöglicht es dem Webserver mit der IP-Adresse 192.167.55.101 eine SSH-Verbindung aufzubauen.

```
sudo ufw allow from 192.168.55.101 to any port 3306
```

Diese Regel ermöglicht es dem Webserver mit der IP-Adresse 192.167.55.101 eine Verbindung zum SQL-Port aufzubauen, damit er die Daten lesen kann und diese dann auf dem Webserver anzeigen kann.

#### 2.2.3 SSL-Zertifikat (HTTPS)

Damit die Verbindung zum Webserver auch den heutigen Anforderungen entspricht wurde ein SSL-Zertifikat erstellt, damit eine sichere HTTPS-Verbindung aufgebaut werden kann. Zum HTTP-Verbindungen zu vermeiden, wurde eine Weiterleitung der HTTP-Anfragen auf HTTPS erstellt, somit sind alle Verbindungen zum Server gesichert. Dies ist wichtig, weil Databaseserver eine heikle Sache sind, weil diese wichtige und sensible Daten enthalten können und das sind immer interessante Informationen für Hacker.

#### 2.2.4 Firewall Webserver

Die Firewall des Webservers enthält folgende fünf Regeln:

```
sudo ufw deny out to any
		
```
Diese Regel blockt den ganzen Verkehr nach aussen, da der Webserver keine Internetverbindung braucht und nur als interner Websever gebraucht wird, braucht er nicht viele Ausnahmenregel. Sondern nur die folgenden drei:

```
sudo ufw allow 80/tcp       
```

Diese Regel ermöglicht eine HTTP-Verbindung von einem anderem Client.

```
sudo ufw allow 433/tcp       
```

Diese Regel ermöglicht eine HTTPS-Verbindung von einem anderem Client.

```
sudo ufw allow from 192.168.55.100 to any port 22		
```

Diese Regel ermöglicht es dem Databaseserver mit der IP-Adresse 192.167.55.101 eine SSH-Verbindung aufzubauen.

```
sudo ufw allow from 192.168.55.1 to any port 22
```

Diese Regel ermöglicht es dem Host (Client) mit der IP-Adresse 192.167.55.1 eine SSH-Verbindung aufzubauen.

## 3. Testprotokoll
### 3.1 Databaseserver MySQL-Server

| Testfall: 001     | Databaseserver MySQL-Server installation                                                             |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Der Databaseserver wurde erfolgreich als MySQL-Server installiert.                                   |
| Beschreibung:     | Mit dem Befehl "sudo service mysql-server status" den Status des Servers überprüfen.                 |
| Soll-Wert:        | service mysql-server is running.                                                                     |
| Ist-Wert:         | service mysql-server is running.                                                                     |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 002     | Webserver Apache2 installation                                                                       |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Der Webserver wurde erfolgreich mit dem Apache2-Dienst installiert.                                  |
| Beschreibung:     | Mit dem Befehl "sudo service apache2 status" den Status des Servers überprüfen.                      |
| Soll-Wert:        | service apache2 is running.                                                                          |
| Ist-Wert:         | service apache2 is running.                                                                          |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 003     | Webserver Adminer installation                                                                       |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Der Webserver wurde erfolgreich mit dem Adminer-Dienst installiert.                                  |
| Beschreibung:     | Mit dem Befehl "ll /etc/apache2/conf-available/" kann man überprüfen, ob adminer.conf vorhanden ist. |
| Soll-Wert:        | Adminer.conf sollte in der Auflistung angezeigt werden.                                              |
| Ist-Wert:         | Adminer.conf wird in der Auflistung angezeigt.                                                       |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 004     | Databaseserver Mysql-Benutzer                                                                        |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Die Benutzer root und database wurden erstellt und verfügen über die richtigen Berechtigungen.       |
| Beschreibung:     | Mit dem Befehl "select * from mysql.user;" können die erstellte Benutzer angezeigt werden.           |
| Soll-Wert:        | root und database sollen in der Benutzerliste angezeigt werden.                                      |
| Ist-Wert:         | root und database werden in der Benutzerliste angezeigt.                                             |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 005     | Databaseserver Firewallkonfiguration                                                                 |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Die Firewallrules wurden gemäss Vagrantfile und Dokumentation erstellt.                              |
| Beschreibung:     | Mit dem Befehl "sudo ufw status" können die erstellte Firewallrules angezeigt werden.                |
| Soll-Wert:        | Firewallrules sollen dem Vagrantfile und der Dokumentation entsprechen.                              |
| Ist-Wert:         | Firewallrules entsprechen dem Vagrantfile und der Dokumentation.                                     |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 006     | Databaseserver Firewallkonfiguration                                                                 |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Die Firewallrules wurden gemäss Vagrantfile und Dokumentation erstellt.                              |
| Beschreibung:     | Mit dem Befehl "sudo ufw status" können die erstellte Firewallrules angezeigt werden.                |
| Soll-Wert:        | Firewallrules sollen dem Vagrantfile und der Dokumentation entsprechen.                              |
| Ist-Wert:         | Firewallrules entsprechen dem Vagrantfile und der Dokumentation.                                     |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 007     | Webserver Firewallkonfiguration                                                                      |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Die Firewallrules wurden gemäss Vagrantfile und Dokumentation erstellt.                              |
| Beschreibung:     | Mit dem Befehl "sudo ufw status" können die erstellte Firewallrules angezeigt werden.                |
| Soll-Wert:        | Firewallrules sollen dem Vagrantfile und der Dokumentation entsprechen.                              |
| Ist-Wert:         | Firewallrules entsprechen dem Vagrantfile und der Dokumentation.                                     |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 008     | Verbindung von Host zu Webserver                                                                     |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Mit dem Hostbrowser kann die default- und adminer.php-Seite des Webservers geöffnet werden.          |
| Beschreibung:     | Mithilfe eines Browser auf dem Host und der Linkadresse http://192.168.55.101/ & -/adminer.php testen|
| Soll-Wert:        | Mit dem Hostbrowser soll die default- und adminer.php-Seite des Webservers geöffnet werden können.   |
| Ist-Wert:         | Mit dem Hostbrowser kann die default- und adminer.php-Seite des Webservers geöffnet werden.          |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 009     | Adminer Login                                                                                        |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Mit dem Hostbrowser kann unter http://192.168.55.101/adminer.php mit den 2 Users angemeldet werden.  |
| Beschreibung:     | Mithilfe eines Browser und der Linkadresse http://192.168.55.101/adminer.php mit den Users testen.   |
| Soll-Wert:        | Mit dem Hostbrowser und den beiden Users soll der Login erfolgreich sein.                            |
| Ist-Wert:         | Mit dem Hostbrowser und den beiden Users kann der Login erfolgreich durchgeführt werden.             |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

| Testfall: 010     | Adminer Tabelle erstellung                                                                           |
| ----------------- | -----------------------------------------------------------------------------------------------------|
| Ziel:             | Auf der Adminer-Seite kann mit dem root User Tabellen erstellt werden und mit database nicht.        |
| Beschreibung:     | Mithilfe eines Browser auf dem Host und der Linkadresse http://192.168.55.101/ & -/adminer.php testen|
| Soll-Wert:        | Auf der Adminer-Seite soll der root User Tabellen erstellen können und der database User nicht.      |
| Ist-Wert:         | Auf der Adminer-Seite kann der root User Tabellen erstellen und der database User nicht.             |
| Analyse:          | alles in Ordnung.                                                                                    |
| Weitere Schritte: | -                                                                                                    |

## 4. Reflexion
Diese LB02 war eine lehrreiche und spannende Erfahrung. Ich konnte dank diesem Projekt Github und vorallem Vagrant sehr gut kennenlernen. Obwohl mri Github zuvor bekannt war kannte ich mich leider nicht aus. Dafür bin ich au dankbar, dass wir Github brauchen mussten, da ich etwas neues und sinnvolles lernen konnte. Mithilfe von Github konnte ich immer einen guten Überblick behalten und egal mit welchem Gerät jederzeit anschauen und bearbeieten. Dies vorallem, weil man alle Dokumente an einem Ort hatte und noch dazu eine neue Art von Schreiben kennenlernen durfte (Markdown). Ich hatte während des ganzen Projektes fast keine Probleme, bis auf zwischendurch wo die VM nicht mehr funktionieren wollte und ich das Vagrantfile nochmals ausführen musste und wieder alles ein wenig testen. Nach diesem Projekt kann ich zum Glück vieles brauchen und in Zukunft anwenden, da mit Vagrant viele VMs einfach und schnell erstellt werden können und man so weniger komplexe und einen kleineren Zeitaufwand hat. Und Github werde ich in Zukunft mehrmals wiederverwenden, da ich jetzt weiss wie nützlich und hilfreich dieses Tool sein kann.