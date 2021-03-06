# Creazione di una semplice VLAN

:heavy_exclamation_mark: **Prerequisito**: avere Packet Tracer (risorsa Cisco) installato sul computer.

La creazione della VLAN avverrà tramite **tre step** diversi:

  1. [VLAN base](https://github.com/matteob2609/Packet-Tracer-creazione-VLAN#ghost-step-1---vlan-base);

  1. [VLAN trunking](https://github.com/matteob2609/Packet-Tracer-creazione-VLAN#ghost-step-2---vlan-trunking);

  1. [VLAN routing](https://github.com/matteob2609/Packet-Tracer-creazione-VLAN#ghost-step-3---vlan-routing).

:heavy_exclamation_mark: **N.B. in questo caso sarà utilizzata la rete 192.168.0.0/24.** :heavy_exclamation_mark:

---

### :heavy_check_mark: RISULTATO FINALE

![image](https://user-images.githubusercontent.com/61114792/109317443-02386f80-784d-11eb-8fc5-6efd618fe54d.png)

---

### :ghost: STEP 1 - VLAN BASE

Creare il modello di partenza utilizzando 1 **Switch 2960**, 4 **PC** ed effettuare il collegamento tra loro selezionando la connessione automatica.

Disporre in questo modo i device:

![image](https://user-images.githubusercontent.com/61114792/109380809-58ea8b80-78d7-11eb-9278-679ef19f5659.png)

(Per visualizzare le etichette delle porte selezionare _Options -> Preferences -> Show port labels_).

Verranno create 3 VLAN, chiamate Amministrazione (VLAN10), Vendite (VLAN20) e Gestione (VLAN30).

Ai PC dell'amministrazione assegnare i seguenti IP e la seguente subnet mask (_PC -> Desktop -> IP Configuration_):

  - **PC10_1**
                              
        IP Address          192.168.10.1 
        Subnet Mask         255.255.255.0
 
  - **PC10_2**

        IP Address          192.168.10.2 
        Subnet Mask         255.255.255.0

Al PC delle vendite assegnare il seguente IP e la seguente subnet mask:

  - **PC20_1**

        IP Address          192.168.20.1
        Subnet Mask         255.255.255.0

Al PC della gestione assegnare il seguente IP e la seguente subnet mask:

  - **PC30_1**

        IP Address          192.168.30.1
        Subnet Mask         255.255.255.0

:pushpin:`Checkpoint: eseguire il 'ping' da PC10_1 a PC10_2 per controllare che i PC comunichino tra loro, confermando il corretto assegnamento degli indirizzi.`

Per creare le VLAN ed abilitarle bisognerà accedere allo Switch, selezionado la schermata _Config_ e successivamente su _VLAN Database_:

    VLAN Number          10
    VLAN Name            Amministrazione
    Add

    VLAN Number          20
    VLAN Name            Vendite
    Add

    VLAN Number          30
    VLAN Name            Gestione
    Add

Dopo aver fatto quest'operazione, individuare quali interfacce dello Switch sono collegate alle diverse VLAN e assegnarle.

Per fare questo bisognerà selezionare lo Switch, spostarsi su _Config_ e selezionare l'interfaccia:

  - **FastEthernet 0/1**, impostarla su _Access_ e selezionare la VLAN 10, escludendo le altre;
 
  - **FastEthernet 0/2**, come la FastEthernet 0/1, in quanto le due interfacce appartengono alla stessa VLAN;

  - **FastEthernet 0/3**, impostarla su _Access_ e selezionare la VLAN 20, escludendo le altre;

  - **FastEthernet 0/4**, impostarla su _Access_ e selezionare la VLAN 30, escludendo le altre;

:pushpin:`Checkpoint: per verificare che le porte siano state impostate correttamente e che siano state assegnate alle diverse VLAN eseguire il comando 'show vlan brief' dalla riga di comando dello Switch. L'output dovrà essere:`

    10   Amministrazione                  active    Fa0/1, Fa0/2
    20   Vendite                          active    Fa0/3
    30   Gestione                         active    Fa0/4
    1002 fddi-default                     active    
    1003 token-ring-default               active    
    1004 fddinet-default                  active    
    1005 trnet-default                    active    

[Torna su](https://github.com/matteob2609/Packet-Tracer-creazione-VLAN#creazione-di-una-semplice-vlan)

---

### :ghost: STEP 2 - VLAN TRUNKING

Aggiungere al modello precedente 1 **Switch 2960**, altri 4 **PC** ed effettuare il collegamento tra loro selezionando la connessione automatica.

Disporre in questo modo i device:

![image](https://user-images.githubusercontent.com/61114792/110806183-16836000-8282-11eb-8298-641f7cdfd871.png)

Ai PC delle vendite assegnare i seguenti IP e la seguente subnet mask (_PC -> Desktop -> IP Configuration_):

  - **PC20_2**
                              
        IP Address          192.168.20.2
        Subnet Mask         255.255.255.0
 
  - **PC20_3**

        IP Address          192.168.20.3
        Subnet Mask         255.255.255.0
        
Al PC dell'amministrazione assegnare il seguente IP e la seguente subnet mask:

  - **PC10_3**
  
        IP Address          192.168.10.3
        Subnet Mask         255.255.255.0
        
Dopo aver fatto quest'operazione, individuare quali interfacce dello Switch sono collegate alle diverse VLAN e assegnarle.

Per fare questo bisognerà selezionare lo Switch, spostarsi su _Config_ e selezionare l'interfaccia:

  - **FastEthernet 0/1**, impostarla su _Access_ e selezionare la VLAN 20, escludendo le altre;
  
  - **FastEthernet 0/2**, impostarla su _Access_ e selezionare la VLAN 20, escludendo le altre;
  
  - **FastEthernet 0/3**, impostarla su _Access_ e selezionare la VLAN 10, escludendo le altre;
        
Ora bisogna impostare in **trunk** le due porte che collegano i due Switch, in questo caso le porte _Fa0/5_ e _Fa0/4_.

Per fare questo bisognerà selezionare lo **Switch0**, spostarsi su _Config_ e selezionare l'interfaccia:

  - **FastEthernet 0/5**, impostarla su _Trunk_ e selezionare la VLAN10 e la VLAN20, escludendo la VLAN30 in quanto non è presente al di sotto.

Per fare questo bisognerà selezionare lo **Switch1**, spostarsi su _Config_ e selezionare l'interfaccia:

  - **FastEthernet 0/4**, impostarla su _Trunk_ e selezionare la VLAN10 e la VLAN20, escludendo la VLAN30 in quanto non è presente e il suo traffico non può essere trasmesso verso lo Switch di sopra.

:pushpin:`Checkpoint: per verificare che le VLAN siano state impostate correttamente tra i due Switch provare a eseguire dal PC10_1 il comando 'ping' verso il PC10_3. Se il comando da esito positivo vuol dire che i due PC comunicano tra loro.`

:heavy_exclamation_mark: **N.B. Il risultato del 'ping' da PC10_1 a PC20_2 dovrà dare esito negativo, in quanto non appartengono alla stessa VLAN e il routing inter-VLAN non è ancora stato implementato.** :heavy_exclamation_mark:

[Torna su](https://github.com/matteob2609/Packet-Tracer-creazione-VLAN#creazione-di-una-semplice-vlan)

---

### :ghost: STEP 3 - VLAN ROUTING

Aggiungere al modello precedente 1 **Router 2911** ed effettuare il collegamento allo Switch tramite la connessione automatica
Configurando questo Router, detto Router on-a-stick, sarà permesso il routing inter-VLAN.

Disporre in questo modo i device:

![image](https://user-images.githubusercontent.com/61114792/109317443-02386f80-784d-11eb-8fc5-6efd618fe54d.png)

Per prima cosa sarà necessario abilitare la porta _Gig0/0_ del Router, facendo click su di esso, selezionando _Config_ e in _Interface_ selezionare _GigabitEthernet 0/0_, mettendo la spunta su _On_.

Visto che ci sono tre VLAN, occorre definire rispettivamente tre sottointerfacce dell'interfaccia _GigabitEthernet 0/0_.

Spostarsi su _CLI_ e dare i rispettivi comandi per creare le tre sottointerfacce:

  - _enable_
  
  - _conf t_
  
  - _interface gigabitethernet 0/0.10_, per le rimanenti due sostituire il .10 con il numero rispettivo;
  
  - _encapsulation dot1Q 10_, lo stesso discorso di sopra vale anche qui;
  
  - _ip address 192.168.10.254 255.255.255.0_, assegno l'indirizzo del gateway della rispettiva VLAN (ultimo disponibile). Stesso discorso di sopra solamente che vale per l'indirizzo IP, modificando ... .10 ...

Ora bisogna impostare in **trunk** la porta dello Switch che si interfaccia con il Router.

Per fare questo bisognerà selezionare lo Switch, spostarsi su _Config_ e selezionare l'interfaccia:

  - **FastEthernet 0/6**, impostarla su _Trunk_ e selezionare tutte e tre le VLAN;

:pushpin:`Checkpoint: eseguire il 'ping' da PC10_1 ad un qualsiasi PC appartenente ad una VLAN diversa per verificare che riescano a comunicare tra loro.`

[Torna su](https://github.com/matteob2609/Packet-Tracer-creazione-VLAN#creazione-di-una-semplice-vlan)
