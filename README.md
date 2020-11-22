# Redes1-Proyecto1_Grupo12
## 201700326
## 201709244

## Definición de las redes topologia 1

| Direccion de Red | Primera Direccion Asignable | Ultima Direccion Asignable | Direccion de Broadcast |
|:-------------:|:-------------:|:-------------:|:-------------:| 
| 192.168.40.0/24 | 192.168.40.1 | 192.168.40.254 | 192.168.40.255 |
| 192.168.30.0/24 | 192.168.30.1 | 192.168.30.254 | 192.168.30.255 |
| 192.168.20.0/24 | 192.168.20.1 | 192.168.20.254 | 192.168.20.255 |
| 192.168.10.0/24 | 192.168.10.1 | 192.168.10.254 | 192.168.10.255 |
| 22.0.0.0/24     | 22.0.0.1     | 22.0.0.254     | 22.0.0.255     |
| 32.0.0.0/24     | 32.0.0.1     | 32.0.0.254     | 32.0.0.255     |
| 42.0.0.0/24     | 42.0.0.1     | 42.0.0.254     | 42.0.0.255     |
| 52.0.0.0/24     | 52.0.0.1     | 52.0.0.254     | 52.0.0.255     |
| 62.0.0.0/24     | 62.0.0.1     | 62.0.0.254     | 62.0.0.255     |
| 72.0.0.0/24     | 72.0.0.1     | 72.0.0.254     | 72.0.0.255     |
| 90.0.0.0/24     | 90.0.0.1     | 90.0.0.254     | 90.0.0.255     |

## Definición de las vlans topologia 1
| # VLAN | Nombre |
|:------:|:------:|
| 22     | RED1   |
| 32     | RED2   |
| 42     | RED3   |
| 52     | RED4   |

<h2>Topologia 1</h2>

![image](https://i.imgur.com/chaeRkB.png)

## Configuracion

### VTP

### VTP modo servidor

Se necesita que Distribucion1 quede como el servidor

### Comandos:

- config t
- vtp domain {nombre}
- vtp password {contraseña}
- vtp mode server
- end

verificamos con sh vtp status

![image](https://i.imgur.com/31ZJWlM.png)

### VT modo cliente

Distribucion2 quedara como cliente

### Comandos:

- config t
- vtp domain {nombre}
- vtp password {contraseña}
- vtp mode client
- end

verificamos con sh vtp status

![image](https://i.imgur.com/diOUZze.png)


### Creación de VLANS

#### Comandos:
- conf t
- vlan {numero}
- name {nombre}
- end
  
Podemos ver en el ESW con vl, si estan todas las vlans creadas

![image](https://i.imgur.com/3W8XkUQ.png)

### Configuración de puertos
- conf t
- interface {interface}
- switchport mode trunk
- end

Verificamos con "sh interfaces status"

![image](https://i.imgur.com/eHl3486.png)

En el switch de capa dos, con la ayuda de la GUI configuramos modo access y trunk

![image](https://i.imgur.com/jzrbqEa.png)

### Portchannel

- config t
- interfaces range fa{numero}/{numero} - {numero}
- channel-group {numero} mode on
- end

verificamos con sh etherchannel summary

![image](https://i.imgur.com/WlwCSpU.png)

### Interfaces vlan

Como necesitamos que el ESW funcione como gateway para la comuicación entre vlans, haremos 4 interafces logicas vlans.

### Comandos

- config t
- interface vlan {numero}
- ip address {ip} {mascara}
- end

Verificamos que todas las interfaces esten creadas con sh ip interface brief

![image](https://i.imgur.com/mO9oUjw.png)

### IPS Virtuales con HSRP

Tenemos 4 redes para las vlans, necesitamos que los dos esw funcionen como gateway entonces necesitamos 4 ips virtuales, 1 por cada interface vlan

### Comandos

- config t
- interface vlan {numero}
- standby {grupo} ip {direccion ip}
- standby {grupo} priority {numero de prioridad}
- standby {grupo} preempt delay minimum {numero}
- end

Verificamos con sh standby brief

![image](https://i.imgur.com/VrTYiA1.png)

### Ips interfaces de red

Cada router tendra su propia red, asignamos una ip a cada interfaz

### Comandos:

- config t
- interface fa{numero}/{numero}
- ip address {direccion} {mascara}
- end

verificamos con sh ip interface brief

![image](https://i.imgur.com/jlhYpe3.png)

### Aprender redes por medio de RIP

### Comandos:

- config t
- router rip
- version 2
- network {direccion de red}
- no auto-summary
- end

verificamos con sh ip route

![image](https://i.imgur.com/1TtG6Ec.png)

### Aprender redes con ruteo estatico

- config t
- ip route {red a aprender} {mascara de la red} {ip de la interfaz}
- end

verificamos con sh ip route

![image](https://i.imgur.com/1TtG6Ec.png)

### Captura de paquetes

Se capturara de un ping de la 192.168.40.20 a la 192.168.10.10 en el siguiente punto:

![image](https://i.imgur.com/BI6yeSI.png)

![image](https://i.imgur.com/MIyCUZu.png)

### Ruta principal

![image](https://i.imgur.com/FWSwZNl.png)
