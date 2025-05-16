# Cuaderno_De_Ejercicios2


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



