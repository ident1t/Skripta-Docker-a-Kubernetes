# Docker
## Úvod do Dockeru
### Kontejner vs VM

Nejprve bude asi nejlepší si vůbec definovat, co je to *kontejner* a jaký je rozdíl mezi ním a *Virtuální Mašinou (VM)*.
Dle definice je kontejner "*standardizované zapouzdřené prostředí, které spouští aplikace*." Lidsky řečeno kontejner je izolované virtuální prostředí, ve kterém běží nějaká aplikace využívající jádro operačního systému, ve kterém probíhá virtualizace.
To má ovšem i svá omezení, např. kvůli tomu nemůžeme spouštět aplikace, které nejsou uzpůsobené pro daný operační systém. A tím se dostáváme k rozdílu mezi VM a kontejnerem. Jelikož v případě VM operační systém virtualizuje i nové jádro, tudíž můžeme třeba virtualizovat operační systém Windows na UNIX-based systému. Potom tu máme další rozdíly jako:
* V případě kontejnerů nevyužíváme pro virtualizaci *hypervisor*, ale *container runtime*.
* Kontejnery nevyžadují tak vysoké hardwarové požadavky.<br>

... a další. Ale teď se pojďme přesunout přímo k Dockeru samotnému.

### Co to je Docker a jeho využití
Jak jsme si již řekli kontejnery potřebují pro virtualizaci *container runtime*, třeba jako je open-source Docker engine. Docker nám mimo jiné umožňuje vytváření vlastních imidžů pro naše aplikace a dokonce jejich zpřístupnění pomocí platformy Docker Hub (https://hub.docker.com), jednoduchý management kontejnerů a další funkce.<br>
Kdy ale vlastně využijeme kontejner? Na to je velice jednoduchá odpověď: Všude, kde chceme mít kompletně izolovanou aplikaci od zbytku systému, ať už kvůli bezpečnosti, nebo aby naše aplikace nebyla ovlivněna konfiguracemi jiných aplikací, které by mohly ovlivnit kompatibilitu se systémem.

### Úvod do Docker networking
Teď když jsme si vysvětlili využití kontejnerů a Dockeru můžeme přejít k dalšímu důležitému tématu a to je *Docker networking*, alias ke komunikaci mezi kontejnery (a aplikacemi v nich) a datovou sítí. Tohle téma je ovšem příliš rozsáhlé, abychom se mu mohli příliš věnovat, tudíž to bude velice stručné.


# Dodělat!!!

### Základní příkazy
Teď přišel čas si už ukázat základní příkazy na ovládání dockeru na UNIX-based systémech.
Nejprve si ukážeme, jak stáhnout image z registru pomocí `docker pull` příkazu v terminálu:

	docker pull [OPTIONS] NAME[:TAG|@DIGEST] 	//např. docker pull nginx:latest
Teď, když máme stažený náš image, můžeme přejít k vytvoření a spuštění našeho kontejneru pomíc příkazu `docker run`:

	docker run [OPTIONS] IMAGE [COMMAND] [ARG...] 	//např. docker run --name my-nginx nginx:latest
**Tohle je pouze nutné minimum, co je potřeba pro spuštění kontejneru, příkaz *docker run* má mnohem více proměnných.* <br>
Pro spuštění příkazu v běžícím kontejneru použijeme příkaz `docker exec`:

 	docker exec [OPTIONS] CONTAINER COMMAND [ARG...] 	//např. docker exec -it my-nginx /bin/bash 
V příkladě výše se pomocí přepínačů `-i` (interactive) a `-t` (terminal) dostaneme dovnitř kontejneru, přesněji do jeho interaktivního bash terminálu.<br>
Pomocí příkazu `docker ps` si můžeme vypsat všechny kontejnery spuštěné v Docker engineu:

	docker ps [OPTIONS]
A pro vypsání všech stažených imageů použijeme příkaz `docker images`:

	docker images [OPTIONS] [REPOSITORY[:TAG]]

### Ukázka
Ukážeme si spuštění jednoduchého kontejneru s imagem nginx.
## Docker v "texťáku" - Docker-Compose
### Docker vs Docker-Compose
De facto se liší jedině tím, že Docker-compose se konfiguruje pomocí konfiguračního souboru v jazyce YAML nebo JSON. Příklad takového souboru můžeme vidět níže:

```yaml
version: '3'
  services:
    web:
      image: nginx:latest
      ports:
      - "8080:80"
```
`version: '3'` Specifikuje verzi konfigurace.<br>
`services: ` Obsahuje definice všech služeb.<br>
`web: ` Jméno dané služby a obsahuje definice konkrétní služby.<br>
`image: nginx:latest` Specifikuje image pro kontejner.<br>
`ports: ` Specifikuje porty na zpřístupnění kontejneru.<br>

K aplikování daného souboru stačí pouze napsat do terminálu příkaz `docker-compose up`.<br>
<b>Pozor!</b> Toto platí pouze v případě, že se nacházíme ve stejné složce jako konfigurační soubor a soubor se jmenuje `docker-compose.yaml`. Jinak musíme specifikovat cestu k souboru pomocí přepínače `-f`.<br>

### Ukázka
Nyní si ukážeme, jak aplikovat danou konfiguraci v praxi. Nejprve vytvoříme nový soubor:

	touch nginx-deployment.yaml
Potom do souboru uložíme vzorek stejný jako ten výše:
 
```yaml
version: '3'
  services:
    web:
      image: nginx:latest
      ports:
      - "8080:80"
```
A nakonec stačí jenom aplikovat konfiguraci:

	docker-compose up -f nginx-deployment.yaml


# Kubernetes
## Úvod do Kubernetes
### Co to je Kubernetes?
Kubernetes je nástroj pro orchestraci a automatizování kontejnerů a clusterování serverů. Nejprve je důležité si uvědomit, že Kubernetes <b>neslouží</b> jako container-runtime, ani jako hypervisior.
Ke správnému fungování Kubernetes je potřeba mít nainstlovaný nějaký container-runtime, jako jsou Docker Engine nebo containerd.<br>
Kubernetes se využívá hlavně ve velkých počítačových systémech jako jsou data centra, private a public cloud. Kubernetes mimo jiné dokáže provádět load balancing mezi jednotlivými <b>nody</b> (node = operační jednotka clusteru, může se jednat o fyzický server nebo VM).

### Základní architektura
<img src="https://kubernetes.io/images/docs/components-of-kubernetes.svg" height="500px">

## Komponenty Kubernetes
### Pod
Musíme si ujasnit, že Kubernetes operuje s tzv. <b>pody</b>. Pod není to samé jako kontejner, ale pro zjednodušení to tak berme.<br>
### Deployment
V <b>deployment</b>u specifikujeme požadovaný status podu/ů, který chceme mít aplikován na clusteru.
Základní šablona deploymentu v YAML:
```yaml
apiVersion: 	#Verze API (může se lišit podle použitého komponentu).
kind:		#Druh komponentu, který chceme definovat.
metadata:	
  name:		#Jméno deploymentu.
  labels:	#Odkaz na danou aplikaci.
    app: 	#Daná aplikace.
spec:		#Specifikace podů
  replicas: 	#Počet podů.
  selector:
    matchLables:
      app: 	#Aplikace, pro kterou tvoříme ReplicaSet (mimo rámec prezentace).
  template:
    metadata:
      labels:
        app: nginx	#Aplikace, pro kterou má být aplikován template.
    spec:		
      containers:
      - name: nginx		#Jméno containeru.
        image: nginx:1.14.2	#Určitý image.
        ports:			#Porty otevřené porty containeru.
        - containerPort: 	#Port, přes který bude container přístupný uvnitř clusteru.
    
```

### Service

### Configmap a Secret

### Ukázka
