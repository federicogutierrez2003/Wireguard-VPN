# 1era Documentacion - Proyecto VPN CASERA 

## Descripcion 
Implementación de una Red Privada Virtual (VPN) con el protocolo WireGuard.
Utilizando la nube de AWS con los recursos gratuitos, desplegando una instancia virtual con SO Ubuntu.
Se implemento Criptografía Asimétrica para que el servidor y el cliente se reconozcan mediante llaves públicas y privadas.
Los beneficios son educativos y practicos, como la de Documentacion de proyecto, implementacion de VPN, practica en la nube.

## Arquitectura y flujo de datos
Aqui el flujo de los datos va completamente cifrado, empezando por el origen hacia el destino por medio de la MV (utilizada como Servidor) que estara 24/7 prendida, implementando en este los protocolos de WireGuard para consolidar la VPN. 

[ Mobile / Client Host (IP: 10.7.0.2) ] ──> ( Encrypted UDP Tunnel / Port 51820 )──> [ AWS EC2 Instance (Ubuntu Server) ] ──> [ Clean Internet Egress (AWS Public IP) ]

## El Principio del Menor Privilegio
El firewall esta configurado con la política de Denegación Implícita, todo el trafico entrante de internet esta bloqueado, exepto los 2 unicos puertos necesarios para el realizado de la VPN.

| Puerto | Protocolo | Servicio | Dirección | Origen |
| :---: | :---: | :---: | :---: | :---: |
| **22** | TCP | SSH | Entrada (Ingress) | `0.0.0.0/0` |
| **51820** | UDP | WireGuard | Entrada (Ingress) | `0.0.0.0/0` |
| **Todos** | Todos | Cualquiera | Salida (Egress) | `0.0.0.0/0` |

Esto esta configurado con Criptografia Asimetrica, desactivando la autenticacion con contraseña, para que solo sea accesible con el archivo .pem.
Utilizacion de UDP para WireGuard para evitar TCP Meltdown.

## Validación de Resultados

```bash
interface: wg0
  public key: [LLAVE_PÚBLICA_DEL_SERVIDOR_OCULTA]
  listening port: 51820

peer: [LLAVE_PÚBLICA_DEL_CELULAR_OCULTA]
  endpoint: [IP_PÚBLICA_DE_TU_CASA_SANITISED]:44535
  allowed ips: 10.7.0.2/32
  latest handshake: 45 seconds ago
  transfer: 2.19 MiB received, 14.66 MiB sent
```bash
