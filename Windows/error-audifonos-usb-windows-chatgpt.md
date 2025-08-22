Gracias por los detalles adicionales, con eso se aclara bastante el patrón:

* Los audífonos funcionan, pero **solo se mutean cuando la laptop está en batería**.
* Al conectar la corriente, el problema desaparece.
* Cambiar de dispositivo de salida (Realtek ↔ USB) “resucita” el audio, pero por poco tiempo.

Esto apunta a que **no es un problema del navegador**, sino de **gestión de energía de Windows o del driver de audio USB cuando el equipo está en modo ahorro**.
Al estar en batería, Windows y el software de Acer reducen consumo en ciertos dispositivos, y en algunos casos esto afecta el bus USB o el codec de audio.

### Posibles causas y soluciones

#### 1. **Plan de energía limita el USB o el codec de audio**

Windows 11, en modo batería, puede suspender selectivamente los puertos USB o reducir la potencia al DAC de los audífonos.

**Solución:**

* Panel de control → *Opciones de energía* → Editar plan de energía → *Configuración avanzada*.
* Busca **Configuración de USB → Configuración de suspensión selectiva de USB** y ponlo en **Deshabilitado** (para batería y para corriente).
* En **Administración de energía del adaptador inalámbrico** y **PCI Express → Administración de energía del estado de enlace**, pon en **Máximo rendimiento** cuando está en batería.
* En **Configuración de multimedia → Al reproducir video**, pon **Optimizar calidad de video** en batería.

#### 2. **Controladores de audio/USB de Acer**

Algunos Nitro tienen software (NitroSense o similar) que gestiona perfiles de energía y dispositivos. Puede estar limitando la alimentación USB en batería.

**Solución:**

* Asegúrate de tener **actualizados los controladores de audio USB y chipset** desde la página de Acer (no solo Windows Update).
* Si tienes NitroSense, revisa en sus ajustes si hay un modo ahorro que afecte audio/USB; prueba deshabilitarlo o poner en modo *Rendimiento* cuando uses batería.

#### 3. **Ahorro de energía en dispositivos USB**

Windows puede apagar los dispositivos USB para ahorrar batería.

**Solución:**

* Administrador de dispositivos → *Controladoras de bus serie universal (USB)*.
* En cada hub/controlador raíz USB, ve a *Propiedades* → pestaña *Administración de energía* → desmarca **Permitir que el equipo apague este dispositivo para ahorrar energía**.

#### 4. **Plan de energía en Windows 11 (modo batería)**

En Windows 11 el control de energía es más agresivo que en versiones previas.

**Solución:**

* Configuración → *Sistema* → *Energía y batería* → Modo de energía: pon **Mejor rendimiento** también en batería, al menos cuando necesites usar audífonos.
* Desactiva cualquier “modo ahorro” extremo cuando uses los audífonos.

---

### ¿Por qué pasa solo al ver YouTube?

El audio USB requiere más energía que el chip integrado, y cuando hay reproducción de video/streaming, el sistema baja la frecuencia de reloj o suspende componentes si está en modo ahorro. Esto coincide con tu síntoma de que al conectarla a corriente desaparece el problema.

---

### Pasos recomendados:

1. Desactiva suspensión selectiva de USB y ahorro de energía en hubs USB.
2. Pon el plan de energía en **Mejor rendimiento** cuando uses batería.
3. Actualiza drivers de audio USB y chipset desde Acer.
4. Revisa NitroSense o software de Acer y configura perfil que no limite USB/audio en batería.

---

¿Quieres que te guíe **paso a paso (con las rutas exactas en Windows 11)** para desactivar todas estas opciones (USB selectivo, hubs, y plan de energía) para que el audio USB no se mutee más? ¿O prefieres que prioricemos primero la **desactivación de suspensión selectiva de USB** (la causa más común)?
