M300 - Data Center (50)
======

Dieses Repository zeigt den möglichen Einsatz von Containern im Data Center auf.

#### Einleitung

Die nachstehende Dokumentation wurde von Michael Blickenstorfer im Rahmen des Moduls M300 (Plattformübergreifende Dienste in ein Netzwerk integrieren) erarbeitet und befasst sich mit dem Einsatz von Containern im Data Center. Dabei geht es um das Kombinieren, Verteilen, Finden und Verwalten von Containern, sowie die Auslagerung von Containern in die Cloud.

#### Revision History

| Datum         | Änderungen                                                                         |  Kürzel  |
| ------------- |:-----------------------------------------------------------------------------------| :------: |
| 24.10.2018    | Erstellung der Datei & erste Änderungen eingeführt                                 |    MBL   |
|      ...      | ...                                                                                |    ...   |

#### Voraussetzungen
* [X] macOS High Sierra (Version 10.13.6)
* [X] Docker for Mac (Version 18.06.1-ce-mac73)

#### Inhaltsverzeichnis
* 01 - [Docker Compose](xxx)
* 02 - [Service Discovery, Cluster & Orchestrierung](xxx)
* 03 - [Docker Datacenter](xxx)
* 04 - [Docker in der Cloud](xxx)

___

![](XXX "Docker Compose") 01 - Docker Compose
======

> [⇧ **Nach oben**](X-X)

Docker Compose ist dazu gedacht, Docker-Umgebungen schneller erstellen zu können.

Dabei werden **YAML-Dateien** genutzt, um die Konfiguration für eine Gruppe von Containern abzulegen.

Compose befreit uns davon, unsere eigenen Skripten für die Orchestrierung warten zu müssen – einschliesslich des Startens, Verlinkens, Aktualisierens und Stoppens unserer Container.

**Anweisungen in docker-compose.yml**

* `apache:` oder `mysql:` 
    * Deklariert den Namen des zu bauenden Containers.
* `build:`
    * Verzeichnis wo das Dockerfile liegt.
* `ports:`
    * Port mapping analog *-p* bei *docker run*.
* `volumes:`
    * Volume mapping analog *-v* bei *docker run*.
* `links:`
    * Linking analog *-l* bei *docker run*.

**Befehle** <br>
* `docker-compose up`
    * Startet alle Container, die in der Compose-Datei definiert sind, und sammelt die Ausgabe. Meist verwendet man das Argument -d, um Compose im Hintergrund laufen zu lassen.
* `docker-compose build`
    * Baut alle Images aus Dockerfiles neu. Der Befehl up baut nur fehlende Images, daher verwenden man diesen Befehl, wenn ein Image aktualisiert werden muss.
* `docker-compose ps`
    * Stellt Informationen zum Status der durch Compose verwalteten Container bereit.
* `docker-compose run`
    Ruft einen Container für das einmalige Ausführen eines Befehls auf. Damit werden auch alle verlinkten Container gestartet, sofern nicht das Argument `--no-dep` mitgegeben wird.
* `docker-compose logs`
    * Gibt aggregierte und farbig hervorgehobene Logs für die durch Compose verwalteten Container aus.
* `docker-compose stop`
    * Stoppt Container, ohne sie zu entfernen.
* `docker-compose rm`
    * Entfernt gestoppte Container. Mit dem Argument `-v` werden auch alle durch Docker verwalteten Volumes entfernt.

**Installation** <br>
Docker Compose ist bei Docker for Mac bereits mitinstalliert. Bei Linux lautet der Installationsbefehl wie folgt:
```Shell
    $ sudo apt-get install -y docker-compose
```

Weitere Informationen unter: https://docs.docker.com/compose/


### YAML
***
YAML, "YAML Ain’t Markup Language", ist eine vereinfachte Auszeichnungssprache (Markup Language) zur Datenserialisierung, angelehnt an XML (ursprünglich) und an die Datenstrukturen in den Sprachen Perl, Python und C sowie dem in RFC 2822 vorgestellten E-Mail-Format. 

**Design Ziele** <br>
Die grundsätzliche Annahme von YAML ist, dass sich jede beliebige Datenstruktur nur mit assoziativen Listen, Listen (Arrays) und Einzelwerten (Skalaren) darstellen lässt. Durch dieses einfache Konzept ist YAML wesentlich leichter von Menschen zu lesen und zu schreiben als beispielsweise XML, außerdem vereinfacht es die Weiterverarbeitung der Daten, da die meisten Sprachen solche Konstrukte bereits integriert haben.



![](XXX "Service Discovery, Cluster & Orchestrierung") 02 - Service Discovery, Cluster & Orchestrierung
======

> [⇧ **Nach oben**](X-X)

Service Discovery ist der Prozess, Clients eines Service mit Verbindungsinformationen (normalerweise IP-Adresse und Port) einer passenden Instanz davon zu versorgen.

In einem statischen System auf einem Host ist das Problem einfach zu lösen, denn es gibt nur eine Instanz von allem.

In einem verteilten System mit mehreren Instanzen von Services, die kommen und gehen, ist das aber viel komplizierter.

Eine Möglichkeit ist, dass der Client einfach den Service über den Namen anfordert (zum Beispiel db oder api) und im Backend dann ein bisschen Magie geschieht, die dazu die passenden Daten liefert.

Für unsere Zwecke kann Vernetzung als der Prozess des Verknüpfens von Containern betrachtet werden.

Es geht nicht darum, reale Ethernet-Kabel einzustecken. Containervernetzung beginnt mit der Annahme, dass es eine Route zwischen Hosts gibt – egal, ob diese Route über das öffentliche Internet läuft oder nur über einen schnellen lokalen Switch.

Mit dem Service Discovery können Clients also Instanzen finden, und die Vernetzung kümmert sich darum, die Verbindungen herzustellen.

Vernetzungs- und Service-Discovery-Lösungen haben häufig gemeinsame Funktionalität, da Service-Discovery-Lösungen auf Ziele im Netz verweisen und Vernetzungslösungen häufig auch Service-Discovery-Features enthalten.

Weitere Funktionen von Service Discovery können sein:
* Health Checking
* Failover
* Load Balancing
* Verschlüsselung der übertragenen Daten
* Isolieren von Containergruppen.

**Lastverteilung (Load Balancing)** <br>
Mittels Load Balancing (Lastverteilung) werden in der Informatik umfangreiche Berechnungen oder große Mengen von Anfragen auf mehrere parallel arbeitende Systeme verteilt.

Insbesondere bei Webservern ist eine Lastverteilung wichtig, da ein einzelner Host nur eine begrenzte Menge an HTTP-Anfragen auf einmal beantworten kann.

Für unsere Zwecke kann Lastverteilung als der Prozess des Verteilens von Anfragen auf verschiedene Container betrachtet werden.

### Cluster
***
Ein Rechnerverbund oder Computercluster, meist einfach Cluster genannt (vom Englischen für "Rechner-Schwarm", "-Gruppe" oder "-Haufen"), bezeichnet eine Anzahl von vernetzten Computern.

Der Begriff wird zusammenfassend für zwei unterschiedliche Aufgaben verwendet:
* Die Erhöhung der Rechenkapazität (HPC-Cluster)
* Die Erhöhung der Verfügbarkeit (HA-Cluster)

> HPC = "high performance computing" - Hochleistungsrechnen <br>
> HA = "high available" - hochverfügbar

Die in einem Cluster befindlichen Computer (auch Knoten, Nodes oder Server) werden auch oft als Serverfarm bezeichnet.


### Orchestrierung
***
Orchestrierung (Orchestration, Instrumentierung, Inszenierung) bezeichnet das flexible Kombinieren mehrerer Services zu einer Komposition.

Diese Komposition beschreibt einen ausführbaren Dienst (Service).

Sowohl unternehmensinterne als auch unternehmensexterne Dienste können kombiniert werden.

Der Prozessfluss wird durch einen Teilnehmer kontrolliert. Jeder Dienst hat dabei einen eingeschränkten Sichtbereich (Scope) und kann für Prozesse nur innerhalb seines Sichtbereichs entscheiden. 


### Docker Swarm
***
Swarm ist das native Clustering-Tool von Docker.

![](http://iotkit.mc-b.ch/tbz/M300V3/html/50-DataCenter/images/Docker/Swarm.png)

Es nutzt die Standard-API, was heißt, dass Container über normale `docker run` Befehle gestartet werden können und sich Swarm dann darum kümmert, einen passenden Host zu finden, auf dem der Container läuft.

Es bedeutet auch, dass andere Tools, die die Docker API verwenden – wie z.B. Compose und selbst geschriebene Scripts –, Swarm ohne Anpassung nutzen können und trotzdem die Vorteile haben, die die Ausführung in einem Cluster statt auf einem einzelnen Host mit sich bringt.

Die grundlegende Architektur von Swarm ist recht klar: 
*  Auf jedem Host läuft ein Swarm-Agent und auf einem "Main-Host" ein Swarm-Manager (in kleinen Test-Clustern kann auf diesem Host auch ein Agent laufen).
*  Der Manager ist für das Orchestrieren und Schedulen von Containern auf dem Host zuständig.

Swarm kann in einem Hochverfügbarkeitsmodus laufen, in dem `etcd`, `Consul` oder `ZooKeeper` für das Failover an einen Backup-Manager genutzt werden.

Es gibt verschiedene Methoden, wie Hosts gefunden und dem Cluster hinzugefügt werden, was man in Swarm als Discovery bezeichnet. Standardmäßig wird das Token-basierte Discovery eingesetzt, bei dem die Adressen der Hosts in einer Liste verwaltet werden, die sich auf dem Docker Hub befindet.

**Installation** <br>
1. Initialisierung auf der Master VM:
    ```Shell
        $ vagrant ssh master
        $ docker  swarm  init  --advertise-addr  192.168.55.100
    ```
2. Auf den Worker-VMs muss der nachfolgende Befehl aisgeführt werden, um dem Swarm beizutreten:
    ```Shell
        $ docker  swarm  join  \
    --token  SWMTKN-1-56lxa2y65iwzwpzdjekzpzqyrbe9v18j0e176corfso479knmd-46io8mgywlpbetse81cr7zsjc 192.168.55.100:2377
    ```
3. Nach dem Join der weiteren VMs, können auf dem Master die Nodes (Hosts bzw. Server) ausgegeben werden:
    ```Shell
        $ docker node ls

        ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
        437kxcuaowr68w43hlehxplpv    worker1   Ready   Active
        ommiame21iunrdcbqnb8v0ryn *  master    Ready   Active        Leader    
    ```

Falls der Befehl um dem Swarm beizutretten nicht mehr bekannt ist, kann dieser wie folgt angezeigt werden:
```Shell
    $ docker swarm join-token worker
```

Anschliessend, können zu Testzwecken eine Reihe von Container gestartet werden:
```Shell
    $ docker service create --replicas 2 --name helloworld alpine ping docker.com
    $ docker service create --replicas 1 --name hello alpine ping docker.com
```

Mittels `docker ps` auf *master* und *worker1* können die erstellten Container abgefragt werden:
```Shell
    ubuntu@master:~$ docker ps

        CONTAINER ID        IMAGE                                                                            COMMAND             CREATED             STATUS              PORTS               NAMES
        80b395876b73        alpine@sha256:dfbd4a3a8ebca874ebd2474f044a0b33600d4523d03b0df76e5c5986cb02d7e8   "ping docker.com"   27 seconds ago      Up 26 seconds                           helloworld.2.u5y8mox6rcun6zj0ywgdb4dtg

    ubuntu@worker1:~$ docker ps

        CONTAINER ID        IMAGE                                                                            COMMAND             CREATED             STATUS              PORTS               NAMES
        d1ce7db16ff1        alpine@sha256:dfbd4a3a8ebca874ebd2474f044a0b33600d4523d03b0df76e5c5986cb02d7e8   "ping docker.com"   9 seconds ago       Up 8 seconds                            hello.1.sb5umx2k8fs713zd2vd9nc1ud
        87e84a2f9d81        alpine@sha256:dfbd4a3a8ebca874ebd2474f044a0b33600d4523d03b0df76e5c5986cb02d7e8   "ping docker.com"   34 seconds ago      Up 33 seconds                           helloworld.1.xio21pojp67ksxc4lzkt22lcp
```

Eine Liste der laufenden Services bekommen wir mittels:
```Shell
    ubuntu@master:~$ docker service ls
        
        ID            NAME        MODE        REPLICAS  IMAGE
        qey1euvdxzua  hello       replicated  1/1       alpine:latest
        yu0s4isrxn53  helloworld  replicated  2/2       alpine:latest
```

Weitere Informationen zu den Services bekommen wir mittels:
```Shell
    ubuntu@master:~$ docker service inspect --pretty helloworld

        ID:             yu0s4isrxn538nvbl7axswl0o
        Name:           helloworld
        Service Mode:   Replicated
        Replicas:      2
        Placement:
        UpdateConfig:
        Parallelism:   1
        On failure:    pause
        Max failure ratio: 0
        ContainerSpec:
        Image:         alpine:latest@sha256:dfbd4a3a8ebca874ebd2474f044a0b33600d4523d03b0df76e5c5986cb02d7e8
        Args:          ping docker.com
        Resources:
        Endpoint Mode:  vip


        ubuntu@master:~$ docker service ps helloworld
        ID            NAME          IMAGE          NODE     DESIRED STATE  CURRENT STATE          ERROR  PORTS
        xio21pojp67k  helloworld.1  alpine:latest  worker1  Running        Running 7 minutes ago
        u5y8mox6rcun  helloworld.2  alpine:latest  master   Running        Running 7 minutes ago


        ubuntu@master:~$ docker service ps hello
        ID            NAME     IMAGE          NODE     DESIRED STATE  CURRENT STATE          ERROR  PORTS
        sb5umx2k8fs7  hello.1  alpine:latest  worker1  Running        Running 7 minutes ago
```

Werden mehr Services benötigt, können diese wie folgt erzeugt werden:
```Shell
    ubuntu@master:~$ docker service scale helloworld=5

        helloworld scaled to 5

    ubuntu@master:~$ docker service ps helloworld

        ID            NAME          IMAGE          NODE     DESIRED STATE  CURRENT STATE           ERROR  PORTS
        xio21pojp67k  helloworld.1  alpine:latest  worker1  Running        Running 12 minutes ago
        u5y8mox6rcun  helloworld.2  alpine:latest  master   Running        Running 12 minutes ago
        lwpv9cclbo02  helloworld.3  alpine:latest  worker2  Running        Running 6 seconds ago
        l500gicijnil  helloworld.4  alpine:latest  worker2  Running        Running 6 seconds ago
        0obzl22j91kt  helloworld.5  alpine:latest  master   Running        Running 10 seconds ago
```

### Redis
***
Redis ist eine In-Memory-Datenbank mit einer einfachen Schlüssel-Werte-Datenstruktur (Key Value Store) und gehört zur Familie der NoSQL-Datenbanken (ist also nicht relational).

![](http://iotkit.mc-b.ch/tbz/M300V3/html/50-DataCenter/images/KeyValue.png)

**Quick-Facts**
* Einfachster Vertreter von NoSQL Datenbanken
* Einfache Struktur aus Schlüssel und Werte-Paaren
* Weder Schlüssel noch Werte aus komplexen Datentypen
* Der Schlüssel ist eineindeutig
* Es werden keine Indexe aufgebaut
* Datensätze können sehr schnell geschrieben und gelesen werden
* Nicht geeignet für komplexere Operationen, wie das Vergleichen von verschiedenen Datensätzen oder die Produktbildung von mehreren Datensätzen
* Werden oft "In-Memory" gehalten und verarbeitet

**Anwendungen** <br>
* Volatiler Zwischenspeicher (sprunghaft, flüchtig) von Webapplikationen, wie z.B. das Speichern von aufbereiteten HTML Seiten
* Unterstützung des Publish- & Subscribe-Pattern für das Empfangen und Abfragen von IoT-Werten
* Persistente Speicherung von Datensätzen mit sehr einfacher Datenstruktur, welche als Schlüssel Wert-Paar dargestellt werden können

**Redis starten** <br>
Redis Service(s) starten:
```Shell
    $ docker service create --name redis --replicas 2 --publish 6379:6379 redis
```

... Oder Global auf alle Swarm Nodes verteilt, d.h. auf jeder Node wird mindestens ein Redis-Service gestartet:
```Shell
    $ docker service create --name redis --mode global --publish 6379:6379 redis
```

Redis-CLI mit einem laufenden Redis Server verbinden (Service Discovery):
```Shell
    $ redis-cli -h master -p 6379
```

Wichtig: Der Redis-Service kann, egal auf welchen Nodes er gestartet wurde, via *master* erreicht werden.

**Redis verwalten**

Key/Value 
* Speichern eines Servernamens und anschliessendes Lesen und Löschen:
    ```C
        SET server:name "myhost"
        GET server:name 
        DEL server:name
    ```

Verwalten von Countern
* Dabei ist sichergestellt, dass ein Wert nur einmal zurückgegeben wird, auch bei mehreren Clients:
    ```C
        SET resource:lock "Redis Demo"
        EXPIRE resource:lock 120
        TTL resource:lock
        GET resource:lock
    ```

Zeitabhängige Werte
* D.h. ein Wert ist nur die Zeit in Sekunden gültig:
    ```C
        SET connections 10
        INCR connections 
        GET connections
    ```

Listenwerte <br>
* `RPUSH` um neue Werte am Ende, `LPUSH` um Werte am Anfang, `LRANGE` um Werte von/bis auszugeben, `LLEN` für die Anzahl Werte und `RPOP` um letzten Wert zurückzugeben und diesen von der Liste zu löschen:
    ```C
        RPUSH hosts a1
        RPUSH hosts m1
        RPUSH hosts b1
        LLEN hosts
        LRANGE hosts 1 2
        RPOP hosts
    ```

Sets (Unsortierte Listen) <br>
* `SADD` um Werte einzutragen, `SREM` um zu löschen und `SISMEMBER` um Abzufragen ob ein Eintrag existiert.
    ```C
        SADD superpowers "flight"
        SADD superpowers "x-ray vision"
        SADD superpowers "reflexes"

        SREM superpowers "reflexes"

        SISMEMBER superpowers "flight"
        SISMEMBER superpowers "reflexes" 
    ```


Hash Maps
* `HSET` und `HMSET` um Werte zu setzen und `HGETALL` und `HGET` um Werte zu lesen.
Counter in Hash Maps:
    ```C
        HSET user:1000 name "John Smith"
        HSET user:1000 email "john.smith@example.com"
        HSET user:1000 password "s3cret"

        HGETALL user:1000

        HMSET user:1001 name "Mary Jones" password "hidden" email "mjones@example.com"

        HGET user:1001 name
    ```

Counter in Hash Maps
* Beispiele:
    ```C
        HSET user:1000 visits 10
        HINCRBY user:1000 visits 1 
        HINCRBY user:1000 visits 10 
        HDEL user:1000 visits
        HINCRBY user:1000 visits         
    ```


### Load Balancing Praxisbeispiel
***
Für dieses Beispiel verwenden wir `hello-world` von der Docker Cloud und cURL.

1. hello-world starten:
    ```Shell
        $ docker service create --name hw --replicas 2 -p 80:80 dockercloud/hello-world
    ```
2. Webseite mittels cURL Abfragen:
    ```Shell
        $ curl http://localhost
    ```

Die Abfragen werden gleichmässig auf die zwei hello-world Services verteilt.


### Kubernetes
***
Kubernetes (auch "K8s" oder einfach "K8") ist ein Open-Source-System zur Automatisierung der Bereitstellung, Skalierung und Verwaltung von Container-Anwendungen, das ursprünglich von Google entworfen und an die Cloud Native Computing Foundation gespendet wurde. Es zielt darauf ab, eine "Plattform für das automatisierte Bespielen, Skalieren und Warten von Anwendungscontainern auf verteilten Hosts" zu liefern. Kubernetes unterstützt eine Reihe von Container-Tools, einschließlich Docker.

Die Orchestrierung mittels Kubernetes wird von führenden Cloud-Plattformen wie Microsofts Azure, IBMs Bluemix, Red Hats OpenShift und Amazons AWS unterstützt.

**Merkmale**
* Immutable (Unveränderlich) statt Mutable
* Deklarative statt imperative (Ausführen von Anweisungen) Konfiguration
* Selbstheilende Systeme (z.B. Neustart bei Absturz)
* Entkoppelte APIs – LoadBalancer / Ingress (Reverse Proxy)
* Skalieren der Services durch Änderung der Deklaration
* Anwendungsorientiertes Denken statt Technisches (z.B. Route 53 bis AWS)
* Abstraktion der Infrastruktur statt in Rechnern Denken

**Objekte**
* `Pod`
    * Ein Pod repräsentiert eine Gruppe von Anwendungs-Containern und Volumes, die in der gleichen Ausführungsumgebung (gleiche IP, Node) laufen.
* `ReplicaSet`
    * ReplicaSets bestimmen wieviele Exemplare eines Pods laufen und stellen sicher, dass die angeforderte Menge auch verfügbar ist.
* `Deployment`
    * Erweitern ReplicaSets um deklarative Updates von Pods (z.B. von Version 1 auf 1.1).
* `Service`
    * Ein Service (manchmal auch als Microservice bezeichnet) steuert den Zugriff auf einen Pod (IP-Adresse, Port). Während Pods ersetzt werden können (z.B. durch Update auf neue Version) bleibt ein Service stabil.
* `Ingress`
    * Ähnlich einem Reverse Proxy ermöglicht ein Ingress den Zugriff auf einen Service über einen URL.

**Installation** <br>
Es gibt unterschiedlichen Varianten, Kubernetes zu installieren. 

Für Docker gibt es unter https://docs.docker.com/docker-for-mac/kubernetes/ eine ausführliche Anleitung für die Installation von Kubernetes.

**Beispiel** <br>
Ein Apache-Webserver kann mit Kubernetes wie folgt bereitgestellt werden:

```Shell
    apiVersion: v1
    kind: Service
    metadata:
    name: apache
    labels:
        app: apache
        group: web
        tier: frontend
    spec:
    type: LoadBalancer
    ports:
    - port: 80
        protocol: TCP
    selector:
        app: apache
    ---

    apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
    kind: Deployment
    metadata:
    name: apache
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: apache
    template:
        metadata:
        labels:
            app: apache
            group: web
            tier: frontend
        spec:
        containers:
        - name: apache
            image: httpd
            ports:
            - containerPort: 80
            name: apache
```


### Kubernetes Cluster
***

![](http://iotkit.mc-b.ch/tbz/M300V3/html/50-DataCenter/images/KubernetesCluster.png)


**Achtung:** Die Host-only Variante funktioniert, je nach Version von Kubernetes, nicht sauber. Deshalb besser mit eigenem vorgeschaltetem Router (z.B. Lernkube) verwenden.

Bei Verwendung mit Lernkube in Vagrantfile (kubernetes und node1), Private IP deaktivieren und Public Bridge und Route aktivieren.

```Ruby
    # config.vm.network "private_network", ip: "192.168.60.100"
    config.vm.network "public_network", bridge: "enp0s8"

    config.vm.provision "shell",
    run: "always",
    inline: "route add default gw 192.168.3.1 enp0s8 && route del default gw 10.0.2.2 enp0s3"
```

Die Kubernetes/Vagrant Installation ist standardmässig Clusterfähig. Bei der Installation wird der Befehl ausgegeben, um weitere Nodes hinzuzufügen. Dieser sieht in etwa so aus:
```Shell
    $ kubeadm join --token <token> 192.168.60.100:6443 --discovery-token-ca-cert-hash <hash>
```

Weitere Nodes können anschliessend mittels der Docker-Umgebung hinzugefügt werden.

Vorgehen, Docker VM normal aufsetzen und kubeadm installieren:
```Shell
    sudo -i
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    sudo echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" >> /etc/apt/sources.list.d/Kubernetes.list
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm
```

Nach der Installation ist der `kubeadm join` Befehl von oben auszuführen. Dadurch wird die Docker VM dem Kubernetes Cluster hinzugefügt. Falls der `kubeadm join` nicht mehr sichtbar ist, kann ein neuer Token generiert wreden.
```Shell
    $ sudo kubeadm token create --print-join-command
```
Getestet kann der Cluster mittels dem dockercloud/hello-world Images:
```
    $ kubectl create -f test/dockercloud.yaml
```

Es werden drei Container (Pods) erstellt, welche gleichmässig auf die Nodes verteilt werden.

Node wieder aus Cluster entfernen:
```Shell
    $ kubectl drain --ignore-daemonsets <node>
    $ kubectl delete node <node>
```



![](XXX "Docker Datacenter") 03 - Docker Datacenter
======

> [⇧ **Nach oben**](X-X)

![](http://iotkit.mc-b.ch/tbz/M300V3/html/50-DataCenter/images/Docker/DCOverview.png)

Docker Datacenter bietet eine integrierte Plattform für Entwickler und IT-Operatoren für das Betreiben von Unternehmenssoftware.

Docker Datacenter bringt Sicherheit, Richtlinien und Kontrollen in den Anwendungslebenszyklus, ohne dabei auf Flexibilität oder Anwendung-Portabilität zu verzichten.

Docker Datacenter integriert sich in ein Unternehmen - vom Netzwerk, offenen APIs und Schnittstellen bis hin zur Flexibilität, um eine Vielzahl von Workflows zu unterstützen.

### Installation
***
Im Verzeichnis XYZ befindet sich ein Vagrantfile welches zwei Nodes generiert.
* `node1` ist der Master 
* `node2` ist der Slave

Die Umgebung kann wie folgt gestartet werden:
```Shell
    $ cd dc
    $ vagrant up
```

**Node 1** <br>
Die Master Node bzw. die Docker Container können wie folgt gestartet werden:
```Shell
    $ vagrant ssh node1

    $ docker run --rm -it \
        -v /var/run/docker.sock:/var/run/docker.sock \
        --name ucp docker/ucp install -i \
        --swarm-port 3376 --host-address 192.168.65.100
```

Anschliessend, wie im Konsolen-Output angegeben, verfahren. Die Admin-Oberfläche kann unter https://192.168.65.100 erreicht werden.

Eine Lizenz ist zum Testen nicht notwendig.

**Node 2** <br>
In einem separaten Terminal die zweite Node starten:
```Shell
    $ vagrant ssh node2
```

Über die Admin-Oberfläche die Node2 hinzufügen - der entsprechende `docker run` Befehl wird angezeigt.

Diesen auf Node2 im Terminal eingeben.

Zum Schluss kann versucht werden eine Private-Registry zu installieren:
```Shell
    $ docker run -it --rm docker/dtr install \
        --dtr-external-url https:// \
        --ucp-node node2 \
        --ucp-username admin \
        --ucp-insecure-tls \
        --ucp-url https://192.168.65.100
```



![](XXX "Docker in der Cloud") 04 - Docker in der Cloud
======

> [⇧ **Nach oben**](X-X)

Praktisch jeder grosser Cloud Anbieter bietet heute die Möglichkeit, Docker Container in dessen Cloud zu betreiben.

Angebote gibt es u.a. von:
* Amazon Web Services (AWS Cloud)
* Google
* Microsoft
* Swisscom
* Cloud Foundry (open-source)

### AWS Cloud
***
Um einen Docker Container in der Amazon Cloud zu betreiben sind folgende Schritte notwendig:
* AWS CLI installieren (Siehe http://docs.aws.amazon.com/cli/latest/userguide/installing.html)
* Mittels `aws-configure` den Zugriff freischalten
 
Weitere Informationen unter: <br>
http://iotkit.mc-b.ch/tbz/M300V3/html/50-DataCenter/590-Cloud/02-AWS.html