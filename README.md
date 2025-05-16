# Cuaderno_De_Ejercicios2
## Ejercicio 1
1. Explicar sobre el siguiente grafo de red los conceptos de circuito virtual entre el nodo 
10 y el 6, y datagrama entre el nodo 3 y el 6. Asumir arcos bidireccionales.

   


### Circuito Virtual entre el nodo 10 y el nodo 6

### Posible Ruta

Una posible ruta fija entre el nodo 10 y el nodo 6 es:

10 → 5 → 2 → 4 → 6

yaml
Copiar
Editar

Esta ruta se mantiene durante toda la sesión de comunicación.  
Cada nodo intermedio guarda un estado de la conexión (como un número de circuito virtual).

### Características

-  Todos los paquetes siguen la misma ruta
-  Orden garantizado
-  Requiere configuración previa
-  Consume recursos en cada nodo intermedio



###  Datagrama entre el nodo 3 y el nodo 6



#### Posibles Rutas

- Ruta 1: `3 → 10 → 5 → 2 → 4 → 6`
- Ruta 2: `3 → 10 → 5 → 7 → 4 → 6`

El camino puede variar dinámicamente según el estado de la red.

#### Características

-  No requiere configuración previa
-  Mayor tolerancia a fallos
-  Puede haber desorden en la llegada de paquetes
-  Mayor carga en el nodo destino (reensamblaje)

#### Ejemplos de uso

- Redes basadas en IP (Internet)
- Enrutamiento dinámico

---

## 🧭 Comparación en Tabla

| Concepto         | Ruta Fija | Estado en Nodos | Reordenamiento | Ejemplo Real      |
|------------------|-----------|------------------|----------------|-------------------|
| Circuito Virtual |  Sí     |  Sí             |  No          | MPLS, X.25        |
| Datagrama        |  No     |  No             |  Sí          | IP (Internet)     |




## Ejercicio 5 
5. En un sistema con las siguientes características, indicar las máscaras utilizadas, la 
primera y última dirección de los equipos, y los elementos/dispositivos necesarios: 
- 7900 profesionales con datos sensibles. 
- 54 subredes para sensorización (100 sensores por subred). 
- 1290 tomas Ethernet. 
- 75000 clientes externos que deberán tener acceso a Internet por wifi. 
- Switches de máximo 24 bocas. 


## Ejercicio 6
6. Calcular la eficiencia un sistema basado en UDP/IP desde un mensaje de la capa de 
aplicación de 1000000 bytes, sabiendo que la longitud máxima del campo de datos es: 
- UDP: 65527 bytes. Cabecera de 8 bytes. 
- IP: 65535 bytes. Cabecera de 4 bytes. 
- Ethernet: 1500 bytes. Cabecera de 8 bytes.


### Respuesta  
**Cantidad de paquetes**

Trama maxima ethernet = 1500 bytes sin contar su propia cabecera

$$\text{Tamaño util para datos} = 1500bytes - 4\text{bytes(Cabecera IP)} - 8\text{bytes(Cabecera UDP)} = 1488$$

Se necesitan dividir 1.000.000 *bytes* de datos en tramas de 1488 *bytes* $1.000.000 / 1488 ≈ 671.14$ es decir se necesitan 672 tramas de datos:
- 671 fragmentos × 1488 bytes = 999.648 bytes
- 1 fragmento final con los 352 bytes restantes

####  Sobrecarga de Cabeceras

- **Primer fragmento**: 8 (UDP) + 4 (IP) + 8 (Ethernet) = 20 bytes  
- **Restantes 671 fragmentos**: 12 bytes cada uno (4 IP + 8 Ethernet)  

Total sobrecarga:  $20 + (671 \times 12) = 20 + 8052 = 8072 \text{bytes}$

#### Calculo de eficiencia
Se calcula con la formula:

$$Eficiencia = \frac{\text{Datos útiles}}{ \text{(Datos útiles + Sobrecarga)}}$$

Si se sustituyen los valores:

$$\text{Eficiencia} = \frac{1.000.000}{(1.000.000 + 8072)} ≈ 0.992$$

La eficiencia del sistema es aproximadamente: 99.2%

## Ejercicio 7
7.Con los datos del ejercicio anterior, calcular la eficiencia de un sistema RTP/UDP, con 
cabecera RTP de 12 bytes y 65535 bytes de datos. 

### Respuesto

| Capa       | Cabecera (bytes) |
|------------|------------------|
| RTP        | 12               |
| UDP        | 8                |
| IP         | 4                |
| **Total cabeceras** | **32**         |

Se calcula el tamaño util de trama y los fragmentos necesarios:
$$\text{Tamaño util para datos} = 1500bytes - 4\text{bytes(Cabecera IP)} - 8\text{bytes(Cabecera UDP)} - 12\text{bytes(Cabecera RTP)} = 1476$$

$$1.000.000 / 1476 ≈ 677.01$$  

Se necesitan un total de 678 paquetes.  
Se calcula la sobrecarga: $\text{Sobrecarga total} = 678 × 32 = 21.696 \text{bytes}$

#### Calculo de eficiencia
Se calcula con la formula:

$$Eficiencia = \frac{\text{Datos útiles}}{ \text{(Datos útiles + Sobrecarga)}}$$

Si se sustituyen los valores:

$$\text{Eficiencia} = \frac{1.000.000}{(1.000.000 + 21.696)} ≈ 0.9788$$

La eficiencia del sistema es aproximadamente: 97,88%

## Ejercicio 8
8. Realizar el pseudocódigo de un cliente y servidor UDP/TCP que devuelva la hora de 
una región determinada.

El cliente envía al servidor el nombre de la región, y el servidor responde con la hora correspondiente. Esta comunicación puede realizarse tanto mediante el protocolo TCP como mediante el protocolo UDP.

#### Pseudo codigo para un servidor y cliente DNS con TCP
- Servidor
````
funcion servidor_TCP():
    crear_socket_TCP()
    enlazar_socket(ip, puerto)
    escuchar_conexiones()

    mientras verdadero:
        cliente_socket, direccion = aceptar_conexion()
        mensaje = recibir(cliente_socket)
        
        si mensaje es una zona horaria válida:
            hora = get_time(mensaje)
            enviar(cliente_socket, hora)
        sino:
            enviar(cliente_socket, "Zona horaria no válida")

        cerrar(cliente_socket)
````
- Cliente
````
funcion cliente_TCP():
    crear_socket_TCP()
    conectar(ip_servidor, puerto)

    zona = pedir_input("Introduce la zona horaria: ")
    enviar(socket, zona)

    respuesta = recibir(socket)
    mostrar("Hora en " + zona + ": " + respuesta)

    cerrar(socket)

````

#### Pseudo codigo para un servidor y cliente DNS con UDP
- Servidor
````
funcion servidor_UDP():
    crear_socket_UDP()
    enlazar_socket(ip, puerto)

    mientras verdadero:
        mensaje, direccion = recibir_desde()
        
        si mensaje es una zona horaria válida:
            hora = get_time(mensaje)
            enviar_a(hora, direccion)
        sino:
            enviar_a("Zona horaria no válida", direccion)

````
- Cliente
````
funcion cliente_UDP():
    crear_socket_UDP()

    zona = pedir_input("Introduce la zona horaria: ")
    enviar_a(zona, ip_servidor, puerto)

    respuesta, direccion = recibir_desde()
    mostrar("Hora en " + zona + ": " + respuesta)

    cerrar(socket)
````

## Ejercicio 9
9. Indicar el procedimiento a seguir mediante el protocolo DNS para obtener la dirección 
IP de la siguiente URL: iuj.ac.jp

Este proceso de resolucion de nombres pasa por varias etapas.

### 1. Consulta en Caché Local

- El sistema operativo o navegador consulta primero su **caché local DNS**.
- Si la dirección IP ya se conoce y no ha expirado, se devuelve directamente.
- Si no está en caché, se inicia una **consulta recursiva** al servidor DNS configurado (por ejemplo, el del ISP o uno público como *8.8.8.8*).


### 2. Consulta al Servidor DNS Recursivo

El servidor DNS recursivo realiza los siguientes pasos de forma **iterativa**:

#### a. Consulta al Servidor Raíz

- Se consulta por la IP de *iuj.ac.jp*.
- El servidor raíz responde con la dirección de los servidores DNS para el dominio ***.jp***.

#### b. Consulta al Servidor TLD *.jp*

- Se consulta por ***ac.jp***.
- El servidor *.jp* responde con la dirección de los servidores DNS para *ac.jp*.

#### c. Consulta al Servidor de Segundo Nivel *ac.jp*

- Se consulta por ***iuj.ac.jp***.
- El servidor de *ac.jp* (o directamente de *iuj.ac.jp*) responde con la dirección IP correspondiente.


### 3. Respuesta Final

- El servidor recursivo guarda la IP en su **caché** (durante el tiempo definido por el TTL).
- La dirección IP se devuelve al cliente original.
- El cliente ya puede conectarse al servidor web *iuj.ac.jp* usando esa IP.


## Ejercicio 10
10. Indicar el intercambio de mensajes completo que se produce desde que se introduce 
la URL www.twitch.tv hasta que empieza a visualizarse una transmisión en directo.

### 1. Resolución DNS

#### Obtener la dirección IP de `www.wikipedia.org`

1. **Consulta en caché local**  
   - El navegador/sistema operativo busca si ya conoce la IP.
   - Si está en caché, se usa directamente.

2. **Consulta recursiva al servidor DNS (por ejemplo, 8.8.8.8)**  
   - Si no está en caché, se realiza el proceso completo:
     - Consulta al servidor raíz → devuelve servidores del TLD `.org`
     - Consulta al TLD `.org` → devuelve servidores de `wikipedia.org`
     - Consulta al servidor autoritativo de `wikipedia.org` → devuelve IP

3. **Respuesta**  
   - El navegador recibe una IP como `208.80.154.224`


### 2. Establecimiento de Conexión Segura (TCP + TLS)

#### Establecer una conexión HTTPS

1. **3-Way Handshake TCP (puerto 443)**
Cliente → Servidor: SYN
Servidor → Cliente: SYN-ACK
Cliente → Servidor: ACK

2. **Handshake TLS**
- Cliente: *ClientHello* (versiones TLS, cifrados, etc.)
- Servidor: *ServerHello*, certificado digital, claves públicas
- Se negocia una clave simétrica segura
- Se establece un canal cifrado


### 3. Solicitud HTTP GET

#### Solicitar el contenido HTML

GET / HTTP/1.1
Host: www.wikipedia.org
User-Agent: Mozilla/5.0 ...
Accept: text/html


El usuario visualiza la página principal de Wikipedia, completamente cargada, segura y lista para interactuar.
