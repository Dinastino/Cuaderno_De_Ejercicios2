# Cuaderno_De_Ejercicios2
## Ejercicio 1
1. Explicar sobre el siguiente grafo de red los conceptos de circuito virtual entre el nodo 
10 y el 6, y datagrama entre el nodo 3 y el 6. Asumir arcos bidireccionales.




### Circuito Virtual entre el nodo 10 y el nodo 6

#### Posible Ruta

Una posible ruta fija entre el nodo 10 y el nodo 6 es:

10 → 5 → 2 → 4 → 6

yaml
Copiar
Editar

Esta ruta se mantiene durante toda la sesión de comunicación.  
Cada nodo intermedio guarda un estado de la conexión (como un número de circuito virtual).

#### Características

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

#### Comparación en Tabla

| Concepto         | Ruta Fija | Estado en Nodos | Reordenamiento | Ejemplo Real      |
|------------------|-----------|------------------|----------------|-------------------|
| Circuito Virtual |  Sí     |  Sí             |  No          | MPLS, X.25        |
| Datagrama        |  No     |  No             |  Sí          | IP (Internet)     |


## Ejercicio 2
Partiendo de la anterior red, generar las tablas de cada uno de los nodos de un circuito 
virtual entre los nodos 3 y 4. Tener en cuenta que los CV creados del anterior ejercicio 
siguen presentes. Asumir arcos bidireccionales.

### Circuito Virtual entre Nodos 3 y 4 (Red con CV Preexistente)

Este documento presenta las **tablas de encaminamiento de un circuito virtual (CV)** entre los nodos **3 y 4**, considerando que:

- La red tiene **arcos bidireccionales**.
- Ya existe un **circuito virtual activo entre los nodos 10 y 6**.
- Es necesario evitar el uso de enlaces o IDs ya utilizados por ese CV previo.


#### Circuito Virtual Preexistente (10 → 6)

Este CV utiliza la siguiente ruta:

10 → 5 → 2 → 4 → 6


Asumimos que los **IDs de CV en uso** son:

- 100 (10→5)
- 101 (5→2)
- 102 (2→4)
- 103 (4→6)


#### Ruta elegida para nuevo CV (3 → 4)

Para evitar conflictos con el CV 10→6, elegimos una ruta alternativa:

3 → 10 → 5 → 7 → 4


Esta ruta **evita los enlaces 5→2 y 2→4**, ya ocupados por el CV anterior.


#### Asignación de IDs de Circuito Virtual

| Enlace       | ID Entrada | ID Salida |
|--------------|-------------|------------|
| 3 → 10       | —           | 200        |
| 10 → 5       | 200         | 201        |
| 5 → 7        | 201         | 202        |
| 7 → 4        | 202         | 203        |


#### Tablas de Encaminamiento del Circuito Virtual 3 → 4

| Nodo | Puerto Entrada | ID CV Entrada | Puerto Salida | ID CV Salida |
|------|----------------|----------------|----------------|----------------|
| 3    | —              | —              | a (→10)        | 200            |
| 10   | a (←3)         | 200            | b (→5)         | 201            |
| 5    | b (←10)        | 201            | c (→7)         | 202            |
| 7    | c (←5)         | 202            | d (→4)         | 203            |
| 4    | d (←7)         | 203            | —              | —              |

> *Nota:* Los puertos (`a`, `b`, etc.) son simbólicos y representan interfaces de red.


#### Flujo del Paquete

Nodo 3: envía paquete con ID = 200 → Nodo 10  
Nodo 10: cambia ID 200 → 201 → Nodo 5  
Nodo 5: cambia ID 201 → 202 → Nodo 7  
Nodo 7: cambia ID 202 → 203 → Nodo 4  
Nodo 4: recibe paquete con ID = 203 (fin del circuito)

## Ejercicio 3
Aplicar los algoritmos indicados en los siguientes grafos, tomando como partida el nodo 
5. Indicar iteración a iteración lo que ocurre. Asumir arcos bidireccionales. 
a) Algoritmo de Dijkstra. 
b) Algoritmo de inundación hasta el nodo 6. Contar el total de paquetes generados.

- (a) Algoritmo de **Dijkstra**
- (b) Algoritmo de **Inundación** hasta el nodo 6 (incluye conteo de paquetes)

Se asume que todos los arcos son **bidireccionales**.


##  GRAFO 1 – (Primer grafo de la imagen 1)

###  A) Algoritmo de Dijkstra

####  Iteraciones desde nodo 5

| Iteración | Nodo Actual | Distancias desde nodo 5      | Acción                          |
|-----------|--------------|-------------------------------|---------------------------------|
| 0         | 5            | 5=0                           | Nodo inicial                    |
| 1         | 7, 4, 10     | 7=1, 4=1, 10=1                | Vecinos inmediatos              |
| 2         | 7            | 2=2, 6=2                      | Desde 7 se alcanzan 2 y 6       |
| 3         | 4            | —                             | Sin nuevas rutas                |
| 4         | 10           | 3=2                           | Desde 10 se alcanza 3           |
| 5         | 2            | 1=3                           | Desde 2 se alcanza 1            |

####  Resultados

| Nodo | Costo | Ruta más corta     |
|------|--------|--------------------|
| 1    | 3      | 5 → 7 → 2 → 1      |
| 2    | 2      | 5 → 7 → 2          |
| 3    | 2      | 5 → 10 → 3         |
| 4    | 1      | 5 → 4              |
| 5    | 0      | —                  |
| 6    | 2      | 5 → 7 → 6          |
| 7    | 1      | 5 → 7              |
| 10   | 1      | 5 → 10             |



###  B) Algoritmo de Inundación (hasta nodo 6)

####  Propagación

- **Nivel 0 (5)** → 4, 7, 10 → 3 paquetes
- **Nivel 1**:
  - 4 → 2
  - 7 → 2, 6 
  - 10 → 3
  → 4 paquetes
- **Nivel 2**:
  - 2 → 1
  → 1 paquete
- **Total: 3 + 4 + 1 = 8 paquetes**

 **Nodo 6 alcanzado en nivel 1**  
 **Total paquetes generados: 8**


##  GRAFO 2 – (Segundo grafo de la imagen 1)

###  A) Algoritmo de Dijkstra

####  Iteraciones desde nodo 5

| Iteración | Nodo Actual | Distancias                     |
|-----------|--------------|--------------------------------|
| 0         | 5            | 5=0                            |
| 1         | 7, 8         | 7=32, 8=25                     |
| 2         | 8            | 4=25+12=37                     |
| 3         | 7            | 3=32+34=66                     |
| 4         | 4            | 2=37+34=71, 6=37+32=69         |
| 5         | 6            | —                              |

#### Resultados

| Nodo | Costo | Ruta                 |
|------|--------|----------------------|
| 2    | 71     | 5 → 8 → 4 → 2        |
| 3    | 66     | 5 → 7 → 3            |
| 4    | 37     | 5 → 8 → 4            |
| 5    | 0      | —                    |
| 6    | 69     | 5 → 8 → 4 → 6        |
| 7    | 32     | 5 → 7                |
| 8    | 25     | 5 → 8                |

---

### B) Algoritmo de Inundación (hasta nodo 6)

####  Propagación

- **Nivel 0 (5)** → 7, 8 → 2 paquetes
- **Nivel 1**:
  - 7 → 3
  - 8 → 4
- **Nivel 2**:
  - 4 → 6 
- **Total paquetes:**
  - Nivel 0: 2
  - Nivel 1: 2
  - Nivel 2: 1  
  →  **Total: 5 paquetes**

 **Nodo 6 alcanzado en nivel 2**  
 **Total paquetes generados: 5**


### GRAFO 3 – (Imagen 2)

### A) Algoritmo de Dijkstra

#### Iteraciones desde nodo 5

| Iteración | Nodo Actual | Distancias acumuladas       |
|-----------|--------------|-----------------------------|
| 0         | 5            | 5=0                         |
| 1         | 6, 7, 10     | 6=30, 7=35, 10=50           |
| 2         | 6            | —                           |
| 3         | 7            | 8=51                        |
| 4         | 10           | 9=60                        |
| 5         | 8            | 3=59                        |
| 6         | 3            | 2=67                        |

#### Resultados

| Nodo | Costo | Ruta                      |
|------|--------|---------------------------|
| 2    | 67     | 5 → 7 → 8 → 3 → 2         |
| 3    | 59     | 5 → 7 → 8 → 3             |
| 5    | 0      | —                         |
| 6    | 30     | 5 → 6                     |
| 7    | 35     | 5 → 7                     |
| 8    | 51     | 5 → 7 → 8                 |
| 9    | 60     | 5 → 10 → 9                |
| 10   | 50     | 5 → 10                    |

### B) Algoritmo de Inundación (hasta nodo 6)

###  Propagación

- **Nivel 0 (5)** → 6, 7, 10 → 3 paquetes
- **Nivel 1**:
  - 6  → destino alcanzado

 **Nodo 6 alcanzado en nivel 1**  
 **Total paquetes generados: 3**

## Ejercicio 4
Diseñar las tablas de rutado jerárquico de la siguiente red:

- Subred A: Arriba izquierda
- Subred B: Arriba derecha
- Subred C: Abajo izquierda
- Subred D: Abajo derecha


### Identificación de nodos

>  Los nodos han sido identificados como `[Subred][Número]`.  
> Por ejemplo:
> - A1, A2, ..., A5 para nodos de la subred A  
> - B1, ..., B4 para la subred B  
> - C1, ..., C4 para la subred C  
> - D1, ..., D5 para la subred D



### ¿Qué es el ruteo jerárquico?

El **ruteo jerárquico** organiza una red compleja en niveles o subredes. Cada subred enruta internamente (rutas locales), y para comunicarse con otras subredes utiliza **gateways**, reduciendo el tamaño de las tablas.

### Ventajas:
- Menor complejidad
- Escalabilidad
- Menos información necesaria en cada nodo

---

#### Subred A (arriba izquierda)

| Salida | Destino     | Tipo         | Ruta               |
|--------|-------------|--------------|--------------------|
| A1     | A2          | Local        | A1 → A2            |
| A3     | A4          | Local        | A3 → A4            |
| A2     | A3          | Local        | A2 → A5 → A3       |
| A1     | A4          | Local        | A1 → A5 → A4       |
| A1     | A3          | Local        | A1 → A3            |
| A2     | A4          | Local        | A2 → A4            |
| A1     | A5          | Local        | A1 → A5            |
| A2     | A5          | Local        | A2 → A5            |
| A3     | A5          | Local        | A3 → A5            |
| A4     | A5          | Local        | A4 → A5            |
| A4     | Subred B    | Jerárquico   | A4 → B3            |
| A4     | Subred C    | Jerárquico   | A4 → C2            |
| A4     | Subred D    | Jerárquico   | A4 → C2 → D1       |



#### Subred B (arriba derecha)

| Salida | Destino     | Tipo         | Ruta               |
|--------|-------------|--------------|--------------------|
| B1     | B2          | Local        | B1 → B2            |
| B2     | B4          | Local        | B2 → B4            |
| B3     | B4          | Local        | B3 → B4            |
| B3     | B1          | Local        | B3 → B1            |
| B2     | B4          | Local        | B2 → B4            |
| B3     | Subred A    | Jerárquico   | B3 → A4            |
| B4     | Subred D    | Jerárquico   | B4 → D2            |
| B3     | Subred C    | Jerárquico   | B3 → A4 → C2       |


#### Subred C (abajo izquierda)

| Salida | Destino     | Tipo         | Ruta               |
|--------|-------------|--------------|--------------------|
| C1     | C2          | Local        | C1 → C2            |
| C1     | C3          | Local        | C1 → C3            |
| C2     | C3          | Local        | C2 → C3            |
| C1     | C4          | Local        | C1 → C4            |
| C2     | C4          | Local        | C2 → C4            |
| C3     | C4          | Local        | C3 → C4            |
| C2     | Subred A    | Jerárquico   | C2 → A4            |
| C2     | Subred B    | Jerárquico   | C2 → A4 → B3       |
| C2     | Subred D    | Jerárquico   | C2 → D1            |



#### Subred D (abajo derecha)

| Salida | Destino     | Tipo         | Ruta               |
|--------|-------------|--------------|--------------------|
| D1     | D2          | Local        | D1 → D2            |
| D1     | D3          | Local        | D1 → D3            |
| D2     | D4          | Local        | D2 → D4            |
| D3     | D4          | Local        | D3 → D4            |
| D5     | D2          | Local        | D5 → D2            |
| D5     | D3          | Local        | D5 → D3            |
| D5     | D4          | Local        | D5 → D4            |
| D2     | Subred B    | Jerárquico   | D2 → B4            |
| D1     | Subred C    | Jerárquico   | D1 → C2            |
| D1     | Subred A    | Jerárquico   | D1 → C2 → A4       |





## Ejercicio 5 
5. En un sistema con las siguientes características, indicar las máscaras utilizadas, la 
primera y última dirección de los equipos, y los elementos/dispositivos necesarios: 
- 7900 profesionales con datos sensibles. 
- 54 subredes para sensorización (100 sensores por subred). 
- 1290 tomas Ethernet. 
- 75000 clientes externos que deberán tener acceso a Internet por wifi. 
- Switches de máximo 24 bocas.

 
Se utiliza la red privada **`10.0.0.0/8`** para cubrir todas las necesidades

### Asignación de subredes desde `10.0.0.0/8`

> Total disponible: **16,777,216 direcciones** (`2^24`)


#### Profesionales (7,900 usuarios)

- Requiere al menos **8192 IPs**
- Asignar: **/19** → 8192 IPs
- Red asignada: `10.0.0.0/19`

| Parámetro        | Valor             |
|------------------|-------------------|
| Red              | `10.0.0.0/19`     |
| Máscara          | `255.255.224.0`   |
| IPs útiles       | 8190              |
| Primera IP       | `10.0.0.1`        |
| Última IP        | `10.0.31.254`     |
| Broadcast        | `10.0.31.255`     |


#### Sensorización (54 subredes, 100 sensores c/u)

- Cada subred requiere al menos **126 IPs**  
- Asignar: **/25** → 128 IPs (126 útiles)
- Necesita 54 bloques

| Subred Base      | Máscara         | IPs útiles | Rango IP ejemplo             |
|------------------|------------------|------------|------------------------------|
| `10.0.32.0/25`   | `255.255.255.128` | 126        | `10.0.32.1 – 10.0.32.126`    |
| ...              | ...              | ...        | ...                          |
| `10.0.58.0/25`   | último bloque     |            |                              |

> Total espacio usado: `54 × 128 = 6912` direcciones  
> Rango: `10.0.32.0 – 10.0.58.127`


#### Tomas Ethernet (1,290 equipos)

- Dividido en 3 bloques **/23** (510 IPs útiles)
- Asignación:

| Subred         | Máscara         | IPs útiles | Rango IP                   |
|----------------|------------------|------------|----------------------------|
| `10.0.60.0/23` | `255.255.254.0`   | 510        | `10.0.60.1 – 10.0.61.254`  |
| `10.0.62.0/23` | —                | 510        | `10.0.62.1 – 10.0.63.254`  |
| `10.0.64.0/23` | —                | 510        | `10.0.64.1 – 10.0.65.254`  |

> Total: 1530 direcciones (cubren 1290)


### Clientes Wi-Fi (75,000 usuarios)

- Requiere al menos 75,000 direcciones
- Asignar: **/15** (131,072 IPs)
- Red asignada: `10.0.66.0/15`

| Parámetro        | Valor             |
|------------------|-------------------|
| Red              | `10.0.66.0/15`    |
| Máscara          | `255.254.0.0`     |
| IPs útiles       | 131,070           |
| Primera IP       | `10.0.66.1`       |
| Última IP        | `10.1.255.254`    |
| Broadcast        | `10.1.255.255`    |


### Dispositivos necesarios

#### Switches (24 puertos máx.)

| Área           | Dispositivos | Puertos/Switch | Switches estimados |
|----------------|--------------|----------------|---------------------|
| Profesionales  | 7,900        | 24             | ~330                |
| Sensorización  | 5,400        | 24             | ~225                |
| Ethernet       | 1,290        | 24             | ~54                 |
| Wi-Fi (APs)    | 750 APs      | 24             | ~32                 |

> **Total switches estimados:** ~640  
> Se recomienda usar **switches con uplinks gigabit** o apilables para backbone.



#### Resumen de Asignaciones

| Segmento          | Red Base        | CIDR   | IPs útiles | Descripción                        |
|-------------------|------------------|--------|------------|------------------------------------|
| Profesionales     | `10.0.0.0/19`    | /19    | 8190       | Usuarios internos, datos sensibles |
| Sensorización     | `10.0.32.0/25` → `10.0.58.0/25` | /25 (×54) | 126/subred | Subred por zona o grupo de sensores |
| Tomas Ethernet    | `10.0.60.0/23` → `10.0.64.0/23` | /23 (×3) | 510/subred | Cableado fijo                      |
| Clientes Wi-Fi    | `10.0.66.0/15`   | /15    | 131,070    | Acceso público Wi-Fi               |


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

### Respuesta

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
