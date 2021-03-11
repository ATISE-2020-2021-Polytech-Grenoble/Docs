#Documentation Cubesat Space Protocol

Cubsat Space Protocol est une pile de protocoles écrits en langage C. Son objectif est de simplifier le communication entre les systèmes embarqués au sein d'un petit réseau.
La conception suit un modèle TCP/IP et inclut un protocole de transport, un protocole de routage et des plusieurs interfaces de couche MAC. L'idée est de donner aux développeurs de cubesat les mêmes fonctionnalités qu'une pile TCP/IP, mais sans ajouter l'énorme coût du header IP.

##Documentation en ligne
 
Voici un lien vers la [Documentation de libcsp](https://github.com/libcsp/libcsp).

Les informations relatives à libcsp et à son utilisation sont disponibles dans la partie doc/ du lien ci-dessus. Il existe 3 exemples en langage C dont nous pourrons nous inspirer dont un exemple de communication serveur-client via des sockets.

##Installation

Se placer dans le répertoire libcsp/ :

```
$./waf configure
$./waf build install
```

Installer libsocketcan (d'après les informations trouvées sur ce [lien](https://blog.mbedded.ninja/programming/operating-systems/linux/how-to-use-socketcan-with-c-in-linux/)) :
```
$git clone https://git.pengutronix.de/git/tools/libsocketcan

$cd libsocketcan
~/libsocketcan$ ./autogen.sh
~/libsocketcan$ ./configure
~/libsocketcan$ make
~/libsocketcan$ sudo make install
```

Installer libzmq :

```
$sudo apt-get install libzmq3-dev
```

Installer Python :
```
$sudo apt-get install python3-dev
```

Enuite, il est possible de compiler les fichiers de tests à configurer comme ceci :
```
./examples/buildall.py
```