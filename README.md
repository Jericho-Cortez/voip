# Documentation VoIP

## Introduction

La **VoIP** (Voice over Internet Protocol) est une technologie permettant de transmettre la voix sur des réseaux IP, offrant une alternative économique et flexible aux systèmes de téléphonie traditionnels. Ce projet vise à mettre en place un système de téléphonie VoIP pour gérer les communications vocales au sein d'une organisation.

## Solutions Existantes

### Gratuites et Open Source
- **Asterisk**
- **FreePBX**
- **Front Panel Operator**
- **Linphone**

### Payantes
- **Oracle Sibel**
- **Microsoft Teams**
- **Discord**
- **RingCentral**
- **Zoom**
- **Skype**

## Fonctionnalités

- **Appels Voix** : Passer et recevoir des appels via Internet.
- **Messagerie Vocale** : Configuration de boîtes vocales pour chaque utilisateur.
- **Conférences Téléphoniques** : Mise en place de salles de conférence pour appels multi-participants.
- **Intégration avec des Applications** : Compatibilité avec des logiciels comme Skype, Microsoft Teams, etc.
- **Mobilité** : Accès aux fonctionnalités depuis n'importe quel appareil connecté à Internet.

## Avantages et Inconvénients

### ✅ Avantages
- **Réduction des coûts** : Les appels via Internet sont moins coûteux que les appels traditionnels.
- **Flexibilité** : Intégration facile avec divers services.
- **Évolutivité** : Ajout simple de nouveaux utilisateurs et fonctionnalités.

### ❌ Inconvénients
- **Qualité de Service** : Dépend de la connexion Internet.
- **Sécurité** : Nécessite des mesures de protection supplémentaires.
- **Dépendance à Internet** : En cas de panne, les services VoIP sont indisponibles.

## Exemples d’Implémentation

### Télétravail
Une entreprise utilise la VoIP pour permettre à ses employés de travailler à distance.

### Centre d'Appels
Un centre d’appels gère les appels entrants et sortants avec la VoIP, permettant plus de flexibilité.

### CRM Multi-Service
Utilisation dans des entreprises ayant différents services : technique, client, commercial, etc.

## Sécurité et Chiffrement

Les protocoles recommandés pour sécuriser la VoIP :
- **SRTP**
- **DTLS**
- **TLS**
- **SSL**
- **AES 256**

Utilisation recommandée de VPN comme **ProtonVPN**, **TunnelBear** ou **Windscribe Pro**.

[🔗 RFC 3711 - Chiffrement VoIP](https://datatracker.ietf.org/doc/html/rfc3711)

---

## Installation d'Asterisk

### 📌 1. Installation des dépendances
sudo apt install build-essential wget libncurses5-dev libssl-dev libxml2-dev libsqlite3-dev uuid-dev perl libwww-perl sox mpg123

###📌 2. Installation et configuration d'Asterisk
cd /usr/src
sudo wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-22-current.tar.gz
sudo tar xvf asterisk-22-current.tar.gz
cd asterisk-22.2.0
sudo ./configure
sudo make
sudo make install
sudo make samples
sudo make config

###📌 3. Configuration de PJSIP
sudo nano /etc/asterisk/pjsip.conf
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0:5060

[transport-tls]
type=transport
protocol=tls
bind=0.0.0.0:5061
cert_file=/etc/asterisk/keys/certificate.pem
priv_key_file=/etc/asterisk/keys/private.key
method=tlsv1_2

[alice](endpoint_internal)
auth=alice
aors=alice
[alice](auth_userpass)
password=bonjour
username=alice
[alice](aor_dynamic)

cd /etc/asterisk/keys
openssl req -x509 -newkey rsa:2048 -keyout private.key -out certificate.pem -days 365 -nodes

###📌 4. Configuration des extensions
sudo nano /etc/asterisk/extensions.conf

[from-internal]
exten => 6001,1,Dial(PJSIP/alice,10)
same => n,VoiceMail(6001)
same => n,Hangup()

exten => 6002,1,Dial(PJSIP/bob,10)
same => n,VoiceMail(6002)
same => n,Hangup()

exten => 6004,1,Dial(PJSIP/julien,10)
same => n,VoiceMail(6004)
same => n,Hangup()

###📌 5. Configuration de la messagerie vocale
sudo nano /etc/asterisk/voicemail.conf
[default]
6001 => 1234, alice
6002 => 1234, bob

###📌 6. Installation de GoogleTTS
wget -O GoogleTTS.tar.gz http://github.com/zaf/asterisk-googletts/tarball/master --no-check-certificate
tar -xvf GoogleTTS.tar.gz
cd zaf-asterisk-googletts-5566ddc
cp googletts.agi /var/lib/asterisk/agi-bin/

###📌 7. Redémarrage d'Asterisk
sudo systemctl restart asterisk.service



