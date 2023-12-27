# Docker
## Úvod do Dockeru
### Kontejner vs VM
  Nejprve bude asi nejlepší si vůbec definovat, co je to *kontejner* a jaký je rozdíl mezi ním a *Virtuální Mašina (VM)*.
Dle definice je kontejner "*standardizované zapouzdřené prostředí, které spouští aplikace*." Lidsky řečeno kontejner je izolované virtuální prostředí, ve kterém běží nějaká aplikace využívající jádro operačního systému, ve kterém probíhá virtualizace.
To má ovšem i své omezení, protože tím pádem nemůžeme spouštět aplikace, které nejsou uzpůsobené pro daný operační systém. A tím se dostáváme k rozdílu mezi VM a kontejnerem. Jelikož v případě VM operační systém virtualizuje i nové jádro, tudíž můžeme třeba virtualizovat operační systém Windows na UNIX-based systému. Potom tu máme další rozdíly jako:
- V případě kontejnerů nevyužíváme pro virtualizaci hypervisor, ale container runtime.
- Kontejnery nevyžadují tak vysoké hardwarové požadavky.
A další. Ale teď se pojďme přesunout přímo k Dockeru samotnému.

### Co to je Docker?
