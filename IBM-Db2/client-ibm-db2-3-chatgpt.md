Para verificar el estado de un puerto específico en Windows desde la línea de comandos (CMD), puedes usar varias herramientas que permiten comprobar si el puerto está abierto y escuchando o si está bloqueado por un firewall.

Aquí te muestro las opciones más comunes para realizar esta verificación:

### 1. **Usando `netstat` (Network Statistics)**

El comando `netstat` permite ver las conexiones de red activas, incluyendo los puertos en los que el sistema está escuchando.

- Abre una **ventana de CMD** como **Administrador**.
- Ejecuta el siguiente comando para listar todos los puertos en los que tu máquina está escuchando:

```bash
netstat -an | findstr :<puerto>
```

Por ejemplo, si quieres verificar el estado del puerto `50000` (que es el puerto por defecto de Db2), ejecutarías:

```bash
netstat -an | findstr :50000
```

### ¿Qué significa la salida?
- **Estado LISTENING**: El puerto está abierto y el sistema está escuchando conexiones en ese puerto.
- **Estado TIME_WAIT, ESTABLISHED, o CLOSE_WAIT**: Indica que hay conexiones activas en ese puerto.
- Si no aparece nada, significa que el puerto no está siendo usado o no está abierto.

### 2. **Usando `telnet` (si está habilitado)**

Si tienes habilitado **Telnet** en tu máquina, puedes usarlo para probar si un puerto específico está accesible en tu máquina o en un servidor remoto. Si el puerto está abierto, Telnet se conectará correctamente; de lo contrario, te mostrará un error.

- Para probar la conectividad de un puerto en tu máquina local, ejecuta el siguiente comando:

```bash
telnet 127.0.0.1 <puerto>
```

Por ejemplo, para probar el puerto `50000`, el comando sería:

```bash
telnet 127.0.0.1 50000
```

Si el puerto está **abierto** y el servicio está **escuchando**, verás una pantalla en blanco o un mensaje de bienvenida del servicio. Si está **cerrado** o bloqueado, recibirás un error como **"Connection failed"**.

### 3. **Usando `PowerShell` con `Test-NetConnection`**

Si prefieres usar **PowerShell**, puedes utilizar el cmdlet `Test-NetConnection`, que es útil para verificar puertos específicos.

- Abre **PowerShell** como **Administrador**.
- Usa el siguiente comando para probar un puerto:

```powershell
Test-NetConnection -ComputerName 127.0.0.1 -Port <puerto>
```

Ejemplo para verificar el puerto `50000`:

```powershell
Test-NetConnection -ComputerName 127.0.0.1 -Port 50000
```

La salida incluirá algo como esto:
```powershell
ComputerName     : 127.0.0.1
RemoteAddress    : 127.0.0.1
RemotePort       : 50000
InterfaceAlias   : Ethernet
SourceAddress    : 192.168.x.x
TcpTestSucceeded : True
```

Si `TcpTestSucceeded` es `True`, significa que el puerto está abierto y accesible. Si es `False`, el puerto no está disponible.

### 4. **Usando `nmap` (si está instalado)**

Si tienes instalado **nmap**, puedes usarlo para escanear puertos. Aunque **nmap** no es una herramienta de línea de comandos estándar de Windows, es muy útil si lo tienes instalado.

- Para escanear un puerto específico en tu máquina, ejecuta:

```bash
nmap -p <puerto> 127.0.0.1
```

Por ejemplo:

```bash
nmap -p 50000 127.0.0.1
```

Si el puerto está abierto, `nmap` mostrará el estado del puerto como "open". Si está cerrado, mostrará "closed".

### 5. **Verificación de Firewall**

A veces el puerto está **físicamente abierto**, pero puede estar bloqueado por el **firewall** de Windows. Para verificarlo:

- Abre el **Firewall de Windows**:  
  `Control Panel` > `System and Security` > `Windows Defender Firewall`
- Revisa las **reglas de entrada** y **salida** para el puerto específico que estás verificando (por ejemplo, el puerto `50000` para Db2).

### Resumen:

- Usa **`netstat`** para ver puertos activos en tu máquina.
- Usa **`telnet`** si quieres probar la conectividad directamente.
- **PowerShell** (`Test-NetConnection`) es una alternativa moderna y fácil para verificar puertos.
- Si tienes **nmap**, es una excelente herramienta para hacer escaneos completos.
- No olvides revisar el **firewall** de Windows si el puerto parece cerrado aunque debería estar abierto.

Cada una de estas herramientas puede darte información sobre el estado de un puerto específico en tu máquina.