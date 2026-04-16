# Laboratorio WAN - Guía de Implementación Cisco Packet Tracer

## 📋 Resumen del Proyecto

Topología jerárquica de 3 capas con redundancia completa:
- **Capa WAN**: 2 routers ISR 4331 con HSRP
- **Capa Core**: 2 switches Catalyst 3650 L3 con EtherChannel
- **Capa Distribución**: 2 switches Catalyst 2960
- **Capa Acceso**: 6 Access Points WRT300N
- **Clientes**: 100 laptops distribuidas en 6 zonas

---

## 🏗️ Topología Física

```
                    [NUBE ISP]
                   /          \
             [ISP1]            [ISP2]
                |                |
           Gi0/0|           Gi0/0|
         [ISR-WAN-1]====[ISR-WAN-2]  (HSRP VIP: 10.0.0.3)
              Gi0/1|      |Gi0/1
                   |      |
              [CORE-SW-1] [CORE-SW-2]  (EtherChannel Po1)
                  /  \    /  \
                 /    \  /    \  (Trunks 802.1Q)
           [DIST-SW-A]  [DIST-SW-B]
            / | \        / | \
          AP1 AP2 AP3   AP4 AP5 AP6
          (17)(17)(17)  (17)(15)(17) laptops
```

---

## 📁 Archivos de Configuración

1. `config_ISR-WAN-1.txt` - Router WAN primario
2. `config_ISR-WAN-2.txt` - Router WAN secundario (HSRP)
3. `config_CORE-SW-1.txt` - Switch Core primario
4. `config_CORE-SW-2.txt` - Switch Core secundario
5. `config_DIST-SW-A.txt` - Switch Distribución A
6. `config_DIST-SW-B.txt` - Switch Distribución B
7. `config_DHCP-Server.txt` - Configuración servidor DHCP

---

## 🚀 Pasos de Implementación en Packet Tracer

### Paso 1: Agregar Dispositivos
- **Routers**: 2x ISR 4331 (agregar módulo HWIC-2T en ambos)
- **Switches Core**: 2x 3650-24PS
- **Switches Distribución**: 2x 2960-24TT
- **Access Points**: 6x WRT300N
- **End Devices**: 100x Laptop-PT
- **Server**: 1x Server-PT

### Paso 2: Conexiones Físicas

| Dispositivo A | Puerto | Dispositivo B | Puerto | Tipo |
|--------------|--------|---------------|--------|------|
| ISR-WAN-1 | Gi0/0 | Cloud-PT | --- | Copper Straight-Through |
| ISR-WAN-2 | Gi0/0 | Cloud-PT | --- | Copper Straight-Through |
| ISR-WAN-1 | Gi0/1 | CORE-SW-1 | Gi1/0/1 | Copper Straight-Through |
| ISR-WAN-2 | Gi0/1 | CORE-SW-2 | Gi1/0/1 | Copper Straight-Through |
| CORE-SW-1 | Gi1/0/23 | CORE-SW-2 | Gi1/0/23 | Crossover (EtherChannel) |
| CORE-SW-1 | Gi1/0/24 | CORE-SW-2 | Gi1/0/24 | Crossover (EtherChannel) |
| CORE-SW-1 | Gi1/0/2 | DIST-SW-A | Gi0/1 | Copper Straight-Through |
| CORE-SW-1 | Gi1/0/3 | DIST-SW-B | Gi0/1 | Copper Straight-Through |
| CORE-SW-2 | Gi1/0/2 | DIST-SW-A | Gi0/2 | Copper Straight-Through |
| CORE-SW-2 | Gi1/0/3 | DIST-SW-B | Gi0/2 | Copper Straight-Through |
| SERVER-DHCP | --- | CORE-SW-1 | Gi1/0/10 | Copper Straight-Through |

### Paso 3: Configurar Access Points
- Cada AP conectado a puertos de acceso en VLANs correspondientes
- Canales: AP1(1), AP2(6), AP3(11), AP4(1), AP5(6), AP6(11)
- SSID: ZONA-A, ZONA-B, ZONA-C, ZONA-D, ZONA-E, ZONA-F
- Seguridad: WPA2-Personal

### Paso 4: Configurar Laptops
- DHCP automático en cada zona
- Verificar conectividad con `ping` a gateway

---

## ✅ Verificación de la Implementación

### Comandos de verificación en Routers:
```
show standby brief              # Ver HSRP
show ip route                   # Ver rutas
show ip interface brief         # Ver interfaces
```

### Comandos de verificación en Switches Core:
```
show etherchannel summary       # Ver EtherChannel
show standby brief              # Ver HSRP
show vlan brief                 # Ver VLANs
show ip route                   # Ver routing L3
```

### Comandos de verificación en Switches Distribución:
```
show vlan brief                 # Ver VLANs
show spanning-tree              # Ver STP
show interfaces trunk           # Ver trunks
```

---

## 📊 Resultados Esperados

- ✅ Conectividad total entre las 100 laptops
- ✅ Redundancia: Si falla un router/switch core, la red sigue operando
- ✅ Balanceo de carga entre enlaces
- ✅ DHCP automático en todas las zonas
- ✅ Acceso a Internet a través de ambos ISP

---

## 📝 Notas Importantes

1. **Packet Tracer Version**: Se requiere versión 8.0 o superior para todos los features
2. **Módulos**: Agregar HWIC-2T a los routers manualmente
3. **EtherChannel**: Configurar como L3 en los switches core
4. **HSRP**: Prioridad más alta en dispositivos primarios
5. **STP**: Configurar Root Guard en puertos de acceso

## 👨‍💻 Autor
Kaleth Laboratorio de Redes WAN - Implementación completa para Packet Tracer
