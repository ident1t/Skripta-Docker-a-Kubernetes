# Docker
## Úvod do Dockeru
### Kontejner vs VM
Nejprve bude asi nejlepší si vůbec definovat, co je to *kontejner* a jaký je rozdíl mezi ním a *Virtuální Mašina (VM)*.
Dle definice je kontejner "*standardizované zapouzdřené prostředí, které spouští aplikace*." Lidsky řečeno kontejner je izolované virtuální prostředí, ve kterém běží nějaká aplikace využívající jádro operačního systému, ve kterém probíhá virtualizace.
To má ovšem i svá omezení, např. kvůli tomu nemůžeme spouštět aplikace, které nejsou uzpůsobené pro daný operační systém. A tím se dostáváme k rozdílu mezi VM a kontejnerem. Jelikož v případě VM operační systém virtualizuje i nové jádro, tudíž můžeme třeba virtualizovat operační systém Windows na UNIX-based systému. Potom tu máme další rozdíly jako:
- V případě kontejnerů nevyužíváme pro virtualizaci *hypervisor*, ale *container runtime*.
- Kontejnery nevyžadují tak vysoké hardwarové požadavky.<br><br>
  ... a další. Ale teď se pojďme přesunout přímo k Dockeru samotnému.

### Co to je Docker a jeho využití
Jak jsme si již řekli kontejnery potřebují pro virtualizaci *container runtime*, třeba jako je open-source Docker engine. Docker nám mimo jiné umožňuje vytváření vlastních imidžů pro naše aplikace a dokonce jejich zpřístupnění pomocí platformy Docker Hub (https://hub.docker.com), jednoduchý management kontejnerů a další funkce.<br>
Kdy ale vlastně využijeme kontejner? Na to je velice jednoduchá odpověď: Všude, kde chceme mít kompletně izolovanou aplikaci od zbytku, ať už kvůli bezpečnosti, nebo aby naše aplikace nebyla ovlivněna změnami v operačním systému, které by mohli ovlivnit kompatibilitu se systémem.

### Úvod do Docker networking
Teď když jsme si vysvětlili využití kontejnerů a Dockeru můžeme přejít k dalšímu důležitému tématu a to je *Docker networking*, alias ke komunikaci mezi kontejnery (a aplikacemi v nich) a datovou sítí. Tohle téma je ovšem příliš rozsáhlé, abychom se mu mohli příliš věnovat, tudíž to bude velice stručné.
