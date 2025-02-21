VoIP
Solution existantes Gratuites, open Sources ,Asterisk, Free PBX, Front Panel Operator
Exemples de solutions existantes payantes , Oracle Sibel, Microsoft Teams, Discord
Appels Voix: Possibilité de passer et recevoir des appels via Internet. Messagerie
Vocale: Configuration de boîtes vocales pour chaque utilisateur. Conférences
Téléphoniques: Mise en place de salles de conférence pour des appels
multi participants.
Intégration avec des Applications Compatibilité: avec des logiciels de communication
comme Skype, Microsoft Teams, etc.
Mobilité: Accès aux fonctionnalités de téléphonie depuis n'importe quel appareil
connecté à Internet.
La VoIP (Voice over Internet Protocol) est une technologie qui permet de transmettre la voix
sur des réseaux IP, offrant une alternative économique et flexible aux systèmes de
téléphonie traditionnels. Ce projet vise à mettre en place un système de téléphonie VoIP pour
gérer les communications vocales au sein d'une organisation. Dans un contexte où les
communications sont cruciales, la VoIP offre des avantages significatifs en termes de coûts
et de fonctionnalités.
Présentation Fonctionnelle
Un système VoIP utilise le protocole IP pour transmettre les
communications vocales. Voici les principales fonctionnalités
mises en place dans ce projet :
Introduction et contextualisation
1
Avantages et Inconvénients.
Avantages
Réduction des Coûts
Exemples dʼImplémentations
Télétravail
Une entreprise utilise un système VoIP pour permettre à ses employés de travailler à distance
tout en restant connectés. Les appels peuvent être passés et reçus depuis n'importe quel
endroit avec une connexion Internet.
Solutions Existantes sur le Marché
Inconvénients :
Skype: Une solution populaire pour les appels vocaux et vidéo.
Zoom: Connu pour ses fonctionnalités de visioconférence et d'appels vocaux.
RingCentral: Une solution complète pour les communications d'entreprise.
Linphone: Une solution open source pour les appels VoIP.
Teams Microsoft
Discord
Qualité de Service: Peut être affectée par la congestion du réseau, nécessite internet
Les appels via Internet sont généralement moins coûteux que les appels traditionnels.
Flexibilité: Possibilité d'intégrer diverses applications et services.
Évolutivité: Facilité d'ajouter de nouveaux utilisateurs et fonctionnalités.
Sécurité: Nécessite des mesures de sécurité pour protéger les communications audio et
vidéos.
Réactivité si problème type maj défaillantes, pas de vrai supports
Dépendance à Internet: La qualité des appels dépend de la stabilité de la connexion Internet.
2
Centre d'Appels
Un centre d'appels utilise la VoIP pour gérer les appels entrants et sortants. Les agents
peuvent travailler depuis différents lieux, offrant une flexibilité accrue.
Travail en déporter , Etranger par exemple
Customer Relationship Management: Traitement Multi Services
(exemple une entreprise avec un service technique, un service client , un support commercial)
Pourriez-vous dire en quoi la configuration VoIP dʼun call center
serait différente de la configuration VoIP dʼun standard téléphonique
dʼune entreprise ?
Réponse au Question du docs
Son coût réduit. En effet, les appels VoIP sont généralement moins chers
que les appels téléphoniques traditionnels.
Peut-on dire la même chose de ses coûts opérationnels et de
maintenance ?
pas du tout car imaginons un central voip qui ne fonctionne pas, une entreprise qui a comme
coeur de métier le support client, ne peux pas de permettre une coupure de service pour raisons
x et y
sans rajout de couts opérationnels, la maintenance est aussi lié au produits on ne pas avoir les
mêmes attentes dʼun contrat utilisations basique que si on bénéficie dʼun contrat pro en
intervention a -4h comme orange le propose par exemple. Les attentes sont forcéments
differentes et de ce fait les couts également.
les différences se situe en qualité de service, un standard téléphonique de call center est régie
par des horaires fixes, un fonctionnement qui prends différents facteurs en charges tel que
affectation de files, priorisations des appels par secteurs (service commercial, support
3
technique) différentes équipes et historisations complète des appels ce que ne permet pas
forcement une configuration voip malgré la présence de commutateur logiciels.
Identifiez des sites marchands ou de service dont customer service
implique des services VoIP, donnez quelques exemples et décrivez
une architecture possible de leur système.
darty , la fnac, france travail, la poste (service banquaire, service livraison, service courrier)
lʼarchitecture est basé sur une autentification , une localisation et ensuite une orientation par
ser vice
Effectuez quelques recherches sur les chiffrements les mieux adaptés à la VoIP.
protocole chiffrements: SRTP , DTLS, TSL (voir notre présentation) SSL, AES 256
par certificat, clés de chiffrements
en usage: Utilisation VPN type Proton VPN , Tunnel Bear version pro , WIndscribe Version Pro
compléments :
https://datatracker.ietf.org/doc/html/rfc3711
4
La VoIP représente une solution moderne et efficace pour les besoins de téléphonie. Bien
qu'elle dépende de la qualité de la connexion Internet, ses avantages en termes de coûts et de
flexibilité en font un choix judicieux pour de nombreuses organisations. Ce projet démontre la
faisabilité et l'efficacité de l'utilisation de la VoIP pour les communications vocales.
Conclusion
sudo apt install build-essential wget libncurses5-dev libssl-dev libxml2-dev libsqlite3-dev uuiddev perl libwww-perl sox mpg123
1. Installation des dépendances
2. Installation et configuration d'Asterisk
2.1. Téléchargement et compilation d'Asterisk
Accédez au répertoire /usr/src, téléchargez l'archive d'Asterisk, puis décompressez-la :
cd /usr/src
sudo wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-22-current.tar.gz
sudo tar xvf asterisk-22-current.tar.gz
cd asterisk-22.2.0
Procédez ensuite à la configuration, compilation et installation d'Asterisk :
sudo ./configure
sudo make
sudo make install
sudo make samples
sudo make config
3. Configuration de PJSIP
3.1. Modification du fichier pjsip.conf
Ouvrez le fichier de configuration :
sudo nano /etc/asterisk/pjsip.conf
; ---------------- TRANSPORT UDP ----------------
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0:5060
; ---------------- TRANSPORT TLS ----------------
[transport-tls]
type=transport
protocol=tls
bind=0.0.0.0:5061
cert_file=/etc/asterisk/keys/certificate.pem
priv_key_file=/etc/asterisk/keys/private.key
method=tlsv1_2
; ---------------- TEMPLATE DES ENDPOINTS ----------------
[endpoint_internal](!)
type=endpoint
context=from-internal
disallow=all
allow=ulaw
transport=transport-tls
[auth_userpass](!)
type=auth
auth_type=userpass
[aor_dynamic](!)
type=aor
max_contacts=10
; ---------------- CONFIGURATION DES UTILISATEURS ----------------
[alice](endpoint_internal)
auth=alice
aors=alice
[alice](auth_userpass)
password=bonjour
username=alice
[alice](aor_dynamic)
[bob](endpoint_internal)
auth=bob
aors=bob
[bob](auth_userpass)
password=bonjour
username=bob
[bob](aor_dynamic)
[julien](endpoint_internal)
auth=julien
aors=julien
[julien](auth_userpass)
password=bonjour
username=julien
[julien](aor_dynamic)
3.2. Génération des certificats TLS
Exécutez la commande suivante pour générer les certificats :
cd /etc/asterisk/keys
openssl req -x509 -newkey rsa:2048 -keyout private.key -out certificate.pem -days 365 -nodes
4. Configuration des extensions
4.1. Modification du fichier extensions.conf
Ouvrez le fichier :
sudo nano /etc/asterisk/extensions.conf
Ajoutez les règles d'appel suivantes :
[from-internal]
; Extensions directes des utilisateurs
exten =&gt; 6001,1,Dial(PJSIP/alice,10)
same =&gt; n,VoiceMail(6001)
same =&gt; n,Hangup()
exten =&gt; 6002,1,Dial(PJSIP/bob,10)
same =&gt; n,VoiceMail(6002)
same =&gt; n,Hangup()
exten =&gt; 6004,1,Dial(PJSIP/julien,10)
same =&gt; n,VoiceMail(6004)
same =&gt; n,Hangup()
; IVR principal (Menu interactif)
exten =&gt; 6003,1,Answer()
same =&gt; n,Set(TIMEOUT(response)=10)
same =&gt; n,agi(googletts.agi,&quot;Bonjour, et bienvenue à la plateforme&quot;,fr)
same =&gt; n,agi(googletts.agi,&quot;Appuyez sur 1 pour joindre le service compte&quot;,fr)
same =&gt; n,agi(googletts.agi,&quot;Appuyez sur 2 pour joindre le service RH.&quot;,fr)
same =&gt; n,agi(googletts.agi,&quot;Appuyez sur 3 pour joindre le service
informatique&quot;,fr)
same =&gt; n,WaitExten(5)
; Options de l&#039;IVR
exten =&gt; 1,1,Goto(from-internal,6001,1)
exten =&gt; 2,1,Goto(from-internal,6002,1)
exten =&gt; 3,1,Goto(from-internal,6004,1)
; Gestion des entrées invalides
exten =&gt; i,1,agi(googletts.agi,&quot;Option non valide.&quot;,fr)
same =&gt; n,Goto(6003,1)
; Gestion du timeout
exten =&gt; t,1,agi(googletts.agi,&quot;Vous n&#039;avez pas saisi d&#039;option.&quot;,fr)
same =&gt; n,Goto(6003,1)
; Boîte vocale générale
exten =&gt; 6099,1,VoiceMailMain()
same =&gt; n,Hangup()
Ajoutez les paramètres suivants :
[general]
format=wav49|gsm|wav|ulaw
[default]
; Format : numéro de messagerie =&gt; mot de passe, nom d&#039;utilisateur
6001 =&gt; 1234, alice
6002 =&gt; 1234, bob
6. Installation de GoogleTTS
wget -O GoogleTTS.tar.gz http://github.com/zaf/asterisk-googletts/tarball/master --no-checkcertificate
tar -xvf GoogleTTS.tar.gz
cd zaf-asterisk-googletts-5566ddc
tcp googletts.agi /var/lib/asterisk/agi-bin/
Ouvrez le fichier :
sudo nano /etc/asterisk/voicemail.conf
5.1. Modification du fichier voicemail.conf
5. Configuration de la messagerie vocale
7. Redémarrage d'Asterisk
Appliquez les modifications en redémarrant le service Asterisk :
sudo systemctl restart asterisk.service
