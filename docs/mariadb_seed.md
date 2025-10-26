📘 MariaDB Seed Playbook

Bestand: playbooks/mariadb_seed.yml
Doel: Installeren, configureren en initialiseren van MariaDB op AlmaLinux of RHEL-systemen met firewall-beveiliging en seed-data import.

🧩 Wat dit playbook doet

Controleert of het systeem een ondersteunde RedHat-familie is.

Installeert benodigde pakketten:

mariadb-server

python3-PyMySQL

firewalld

Start en enabled services:

mariadb

firewalld

Opent poort 3306/tcp permanent in firewalld.

Maakt een database aan (ecomdb) met een applicatiegebruiker (ecomuser) die alleen mag verbinden vanaf een specifiek webhost-IP.

Kopieert het SQL-seedbestand (db-load-script.sql) naar /tmp.

Importeert de seed-data idempotent in de database.

⚙️ Variabelen
Variabele	Beschrijving	Standaardwaarde
mysql_port	MySQL-poort	3306
mariadb_service	Servicenaam voor MariaDB	mariadb
py_mysql_pkg	Python-driver voor MySQL-modules	python3-PyMySQL
dbname	Naam van de database	ecomdb
dbuser	Database-gebruiker	ecomuser
dbpassword	Wachtwoord voor de gebruiker	ChangeMe!
web_host	IP-adres van de webserver die toegang krijgt	192.168.56.15
seed_sql_src	Pad naar het lokale SQL-bestand	{{ playbook_dir }}/files/db-load-script.sql
seed_sql_dest	Doelpad op de host	/tmp/db-load-script.sql
🚀 Gebruik

Run het playbook met sudo-rechten (eerste keer met wachtwoordprompt):

ansible-playbook playbooks/mariadb_seed.yml -t db --ask-become-pass


Na het draaien is:

MariaDB geïnstalleerd en actief.

De firewall correct geconfigureerd.

De database en gebruiker aangemaakt.

De initiële data geladen vanuit het seed-bestand.

📦 Seedbestand

Het seed-bestand staat in:

playbooks/files/db-load-script.sql


Voorbeeldinhoud:

CREATE TABLE IF NOT EXISTS products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  price DECIMAL(10,2) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

🧰 Tags
Tag	Doel
packages	Alleen de installatie uitvoeren
service	Services starten/herstarten
firewall	Firewall-configuratie
db	Database-acties
seed	Seed-import
🧠 Best Practice

Bewaar gevoelige waarden zoals dbpassword in Ansible Vault.

Gebruik group_vars/db_servers.yml om waarden voor productie te overschrijven.

Combineer met het web-deploy playbook om een volledige LAMP-stack te automatiseren.