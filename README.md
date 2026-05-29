# UDP Inter-AS Sin Estado de Sesion
## Un Vector de Ataque Estructural en Internet y el Caso para su No-Rutabilidad

**Autor:** RRozasC  
**Organizacion:** PIT Chile - Internet Exchange Point  
**Version:** v6 - Mayo 2026  
**Estado:** Documento de posicion tecnica abierto a comentarios

---

## La tesis en una linea

UDP sin estado de sesion previo no deberia ser ruteable entre Sistemas Autonomos. No porque UDP sea malo, sino porque un paquete UDP que llega sin contexto de sesion es estructuralmente indistinguible de un ataque de amplificacion.

## El problema

Los ataques de amplificacion UDP (DNS, NTP, Memcached, SSDP) siguen siendo el vector de DDoS volumetrico dominante. Un atacante con 1 Gbps puede generar 556 Gbps en la victima via NTP monlist. El operador destino tiene dos opciones: pagar scrubbing o caer. No existe una tercera.

BCP38 existe desde el ano 2000. 25 anos despues, un cuarto de las redes en internet siguen permitiendo spoofing. La voluntariedad no funciona.

La solucion que este documento propone no es un nuevo protocolo ni una nueva herramienta. Es una condicion de rutabilidad:

> Si hay un handshake previo verificable, el trafico UDP es legitimo y ruteable. Si no hay handshake, no hay razon para que ese paquete cruce fronteras de AS.

## Dos lineas argumentales

**Linea A (maximalista):** UDP no deberia ser ruteable entre ASes. Uso valido en redes privadas/LAN solamente.

**Linea B (compromiso, la propuesta viable):** UDP inter-AS es permitido si y solo si existe estado de sesion previo verificable. QUIC, WireGuard y DTLS cumplen este criterio hoy. DNS raw, NTP raw, UDP crudo: no.

## Por que los IXPs son el punto correcto de enforcement

Un firewall dropea el paquete cuando ya llego, ya consumio transito, ya cargo el borde. La no-rutabilidad mata el paquete antes de que viaje. Son cosas distintas.

El IXP es el punto donde los ASes se interconectan. Es el lugar arquitectonicamente correcto para aplicar esta politica, con o sin Route Server. El modelo propuesto (IXP2) usa certificacion descentralizada compatible con la tendencia hacia peering directo AS-to-AS.

## Contenido del repositorio

- `UDP_InterAS_Position_Paper_v6.pdf` — documento completo con todos los argumentos, contras, implementacion tecnica, comparacion con SAVNET/BAR-SAV, y plan de accion

## Estado del arte en IETF

- **SAVNET** y **BAR-SAV** (draft activo octubre 2025) abordan validacion de IP origen en L3. Complementarios, no equivalentes.
- **Ningun draft existente** propone no-rutabilidad UDP inter-AS basada en estado de sesion (L4). Este documento es el primero en formalizarlo.

## Como contribuir

Este documento esta abierto a critica tecnica, operacional y de politica. Se aceptan:

- **Issues:** contraargumentos tecnicos, casos de uso no cubiertos, experiencias de implementacion similar, datos empiricos que refuercen o contradigan la tesis
- **Pull requests:** correcciones factuales, referencias adicionales, mejoras al argumento
- **Discusiones:** casos especiales, implicaciones en IPv6, integracion con SAVNET/BAR-SAV

Si trabajas en un IXP, ISP, o tienes experiencia con DDoS amplification en produccion, tu perspectiva es especialmente relevante.

## Contexto del autor

Operador de red con 30 anos de experiencia en ISPs y 5 anos operando un IXP regional (PIT Chile) con presencia en Chile, Mexico, Argentina y Peru. Este documento surge de ver los mismos ataques UDP repetirse durante tres decadas sin que la industria haya resuelto la condicion estructural que los hace posibles.

## Licencia

CC BY 4.0 - libre para citar, adaptar y distribuir con atribucion.

---
