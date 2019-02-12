# Compte-rendu du TP5 de CCNA


## Sommaire

* I. [Mise en place du lab](#i-mise-en-place-du-lab)

* II. [Spéléologie Réseau](#ii-spéléologie-réseau)
* 1. [ARP](#1-arp)
  * 2. [Interception de trafic avec Wireshark](#2-wireshark)
    * [ARP et `ping`](#a-interception-darp-et-ping)
    * [netcat](#b-interception-dune-communication-netcat)
    * [Trafic Web (HTTP)](#c-interception-dun-trafic-http)

## I - Préparation du lab

**Réseaux :**

* `net1` : `10.5.1.0/24`
* `net2` : `10.5.2.0/24`
* `net12` : `10.5.12.0/30`

J'ai choisi le réseau `10.5.3.0/30` pour le `net12` car c'est un réseau privé et parce que nous n'avons que 2 routeurs dedans.

**Machines :**

Machine | `net1` | `net2` | `net12`
--- | --- | --- | ---
`client1.tp5.b1` | X | `10.5.2.10` | X
`client2.tp5.b1` | X | `10.5.2.11` | X
`router1.tp5.b1` | `10.5.1.254` | X | `10.5.12.1`
`router2.tp5.b1` | X | `10.5.2.254` | `10.5.12.2`
`server1.tp5.b1` | `10.5.1.10` | X | X


**Checklist IP VMs** 

On parle de `client1.tp5.b1`, `client2.tp5.b1` et `server1.tp5.b1` :

* [X] Désactiver SELinux
  * déja fait dans le patron
* [X] Installation de certains paquets réseau
  * déja fait dans le patron
* [X] Désactivation de la carte NAT
  * déja fait dans le patron
* [ ] Définition des IPs statiques
* [ ] La connexion SSH doit être fonctionnelle
  * une fois fait, vous avez vos trois fenêtres SSH ouvertes, une dans chaque machine
* [ ] Définition du nom de domaine

**Checklist IP Routeurs**

On parle de `router1.tp5.b1` et `router2.tp5.b1` :

* [X] Définition des IPs statiques
* [X] Définition du nom de domaine

`router2.tp5`:
    ```bash
    router1.tp5#show ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                10.5.1.254      YES manual up                    up
    Ethernet0/1                10.5.12.1       YES manual up                    up
    Ethernet0/2                unassigned      YES unset  administratively down down
    Ethernet0/3                unassigned      YES unset  administratively down down
    ```


`router2.tp5`:

    ```bash
    router2.tp5#show ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    Ethernet0/0                10.5.2.254      YES manual up                    up
    Ethernet0/1                10.5.12.2       YES manual up                    up
    Ethernet0/2                unassigned      YES unset  administratively down down
    Ethernet0/3                unassigned      YES unset  administratively down down
    ```