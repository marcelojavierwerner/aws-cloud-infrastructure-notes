# AWS Cloud Infrastructure & Data Architecture Basics

Este repositorio contiene la documentación, arquitectura y guía paso a paso para el despliegue de una infraestructura en la nube en **Amazon Web Services (AWS)**. El proyecto aborda la configuración de redes virtuales segregadas, cómputo escalable, almacenamiento persistente y la integración de bases de datos relacionales en entornos privados.

---

## 📐 Diagrama de Arquitectura

```text
+-----------------------------------------------------------------------------------+
| AWS Cloud (Region: us-east-1)                                                     |
|                                                                                   |
|  +-----------------------------------------------------------------------------+  |
|  | VPC (10.0.0.0/16)                                                         |  |
|  |                                                                             |  |
|  |   +--------------------------+             +----------------------------+   |  |
|  |   | Availability Zone A      |             | Availability Zone B        |   |  |
|  |   |                          |             |                            |   |  |
|  |   |  [ Public Subnet 1 ]     |             |  [ Public Subnet 2 ]       |   |  |
|  |   |  10.0.0.0/24             |             |  10.0.2.0/24               |   |  |
|  |   |  +--------------------+  |             |  +----------------------+  |   |  |
|  |   |  | NAT Gateway        |  |             |  | EC2 Instance         |  |   |  |
|  |   |  +--------------------+  |             |  | (API / Web Server)   |  |   |  |
|  |   |                          |             |  +----------+-----------+  |   |  |
|  |   |                          |             |             |              |   |  |
|  |   |  [ Private Subnet 1 ]    |             |  [ Private Subnet 2 ]      |   |  |
|  |   |  10.0.1.0/24             |             |  10.0.3.0/24               |   |  |
|  |   |                          |             |  +----------------------+  |   |  |
|  |   |                          |             |  | RDS Instance         |  |   |  |
|  |   |                          |             |  | (Relational DB)      |  |   |  |
|  |   |                          |             |  +----------------------+  |   |  |
|  |   +--------------------------+             +----------------------------+   |  |
|  +-----------------------------------------------------------------------------+  |
|                                         ^                                         |
+-----------------------------------------|-----------------------------------------+
                                          | Internet Gateway
                                          v
                                    Internet / User
```

---

## 🎯 Objetivos de la Infraestructura

* **Configuración de Red Virtual (VPC):** Creación y segmentación de subredes públicas y privadas distribuidas en múltiples Zonas de Disponibilidad (AZ).
* **Cómputo en la Nube (EC2):** Despliegue, dimensionamiento y administración de instancias Linux.
* **Gestión de Almacenamiento (EBS):** Creación de volúmenes de almacenamiento en bloque Elastic Block Store (EBS), formateo de sistemas de archivos y estrategias de respaldo mediante Instantáneas (*Snapshots*).
* **Seguridad & Acceso Privado:** Implementación de Grupos de Seguridad (*Security Groups*) y Tablas de Enrutamiento para restringir el acceso a la capa de datos (**Amazon RDS**) únicamente desde servidores de la red privada/pública autorizados.

---

## 🛠️ Servicios Utilizados (AWS Stack)

* **Amazon VPC:** Red lógica aislada para definir reglas de enrutamiento, CIDR blocks y gateways.
* **Amazon EC2:** Cómputo donde se alojan aplicaciones backend o servicios de ML.
* **Amazon EBS:** Almacenamiento persistente de alto rendimiento montado en la instancia EC2.
* **Amazon RDS:** Motor de base de datos relacional administrado en subred privada.
* **AWS IAM & Security Groups:** Control de accesos e inspección de tráfico a nivel de red e instancia.

---

## 🚀 Implementación Paso a Paso

### 1. Red Virtual y Seguridad (VPC & Subnets)
1. Definición del bloque CIDR principal `10.0.0.0/16` para la VPC.
2. Subdivisión en 2 Subredes Públicas (`10.0.0.0/24`, `10.0.2.0/24`) y 2 Subredes Privadas (`10.0.1.0/24`, `10.0.3.0/24`).
3. Asociación de un **Internet Gateway (IGW)** a la tabla de enrutamiento pública para dar acceso web exterior.
4. Configuración del **NAT Gateway** para permitir salidas de actualización seguras desde subredes privadas.

### 2. Aprovisionamiento de Cómputo (EC2) y Almacenamiento (EBS)
1. Lanzamiento de una instancia `t2.micro` / `Amazon Linux` dentro de la subred pública.
2. Creación y adición de un volumen adicional **EBS (gp2/gp3)**.
3. Conexión por SSH mediante llaves de seguridad encriptadas (`.pem` / `.ppk`).
4. Formateo y montaje del sistema de archivos en Linux.
5. Generación de una **Instantánea (Snapshot)** de respaldo para garantizar continuidad del negocio e integridad de datos.

### 3. Capa de Datos Privada (RDS)
1. Creación de la instancia **Amazon RDS** asignada exclusivamente a la subred privada.
2. Restricción del puerto del motor de base de datos a través de *Security Groups*, aceptando únicamente peticiones originadas desde el *Security Group* del servidor EC2.

---

## 💡 Casos de Uso en Ciencia de Datos & MLOps

Esta infraestructura no es solo un ejercicio de administración, sino la arquitectura base para despliegues de datos modernos:

* **Servido de Modelos (ML APIs):** La instancia EC2 funciona como servidor de inferencia (ej. API en FastAPI o Flask que ejecuta un modelo predictivo).
* **Almacenamiento de Datasets:** El volumen EBS proporciona espacio desacoplado para almacenar lotes de datos, logs o archivos temporales de procesamiento ETL sin comprometer el disco del SO.
* **Base de Datos Operacional de Feature Store / OLTP:** La instancia RDS aislada en la subred privada almacena datos estructurados, históricos de usuarios o resultados de predicciones garantizando máxima seguridad.

---

## 🏅 Certificaciones Oficiales relacionadas

| Badge / Certificación | Emisor | Estado |
| :--- | :--- | :--- |
| **AWS Academy Graduate - Cloud Foundations** | Amazon Web Services (AWS) |  Obtenido |
[![AWS Academy Cloud Foundations](https://img.shields.io/badge/AWS_Academy-Cloud_Foundations-orange?style=for-the-badge&logo=amazon-aws)]((https://www.credly.com/badges/6a0adf6b-1db5-4a64-a182-58c4f2ebc547/linked_in_profile))

> *Los laboratorios y arquitecturas documentados en este repositorio respaldan los conocimientos prácticos requeridos para la acreditación AWS Academy Cloud Foundations.*

---
## 👤 Autor

**Marcelo Javier Werner**  
*Data Science Student | Software & Cloud Infrastructure Enthusiast* 
🎓 **AWS Academy Graduate - Cloud Foundations**
* **LinkedIn:** [https://www.linkedin.com/in/marcelowerner/]
* **GitHub:** [https://github.com/marcelojavierwerner]
* **Proyecto:** SerWer Data Lab
