# ST0263 - Tópicos Especiales en Telemática

## Información de la Materia

**Proyecto:** Desplegar un CMS Drupal en un clúster de alta disponibilidad en Kubernetes autoescalado en la nube, con su propio dominio y certificado SSL. El sitio se llama: [reto2-toptel.site](https://reto2-toptel.site).

### Estudiantes

- **Santiago Arias Higuita** - [sariash@eafit.edu.co](mailto:sariash@eafit.edu.co)
- **Luis Miguel Giraldo Gonzalez** - [lmgiraldo4@eafit.edu.co](mailto:lmgiraldo4@eafit.edu.co)

### Profesor

- **Alvaro Enrique Ospina Sanjuan** - [aeospinas@eafit.edu.co](mailto:aeospinas@eafit.edu.co)

---

## 1. Descripción de la Actividad

### 1.1. Aspectos Cumplidos y Desarrollados

#### Requerimientos Funcionales

- **Despliegue de CMS Drupal:** Se desplegó exitosamente un CMS Drupal en un clúster de Kubernetes de alta disponibilidad en la nube.
- **Balanceador de Cargas:** Implementación de Nginx como balanceador de cargas en la capa de aplicación.
- **Dominio y Certificado SSL:** Configuración del dominio `reto2-toptel.site` con certificado SSL utilizando Cert Manager y Let's Encrypt.
- **Clúster Autoescalado:** Configuración de autoescalado en el clúster de Kubernetes para gestionar dinámicamente la carga de trabajo.
- **Servidores Adicionales:**
  - **DBServer:** Implementación de la base de datos dentro del clúster Kubernetes utilizando Docker.
  - **FileServer:** Configuración de un servidor NFS para la gestión de archivos.
- **Integración de AWS EFS CSI Driver:** Utilización del driver CSI de AWS EFS para el almacenamiento persistente.
- **Manifiestos YAML:** Toda la infraestructura fue definida y gestionada mediante manifiestos YAML.

#### Requerimientos No Funcionales

- **Alta Disponibilidad en la Capa de Aplicación y Almacenamiento:** Se aseguró la alta disponibilidad en la capa de aplicación mediante el uso de Kubernetes y Nginx, así como en la capa de almacenamiento utilizando AWS EFS.
- **Seguridad:** Implementación de HTTPS para asegurar las comunicaciones mediante certificados SSL.
- **Escalabilidad:** Configuración de mecanismos de autoescalado tanto a nivel de clúster como de aplicaciones para manejar incrementos en la carga.
- **Gestión de Configuración:** Uso de herramientas como kubectl para la gestión y despliegue de la infraestructura.

### 1.2. Aspectos No Cumplidos

#### Requerimientos Funcionales

- **Alta Disponibilidad en la Base de Datos:** No se logró implementar una base de datos de alta disponibilidad. La base de datos se desplegó dentro del clúster Kubernetes sin configuración de redundancia o failover.

#### Requerimientos No Funcionales

- **Monitoreo y Logging Avanzado:** No se implementaron soluciones avanzadas de monitoreo y logging para el clúster y las aplicaciones desplegadas.
- **Automatización de Backups:** No se configuraron mecanismos automatizados para la realización de backups de la base de datos y del sistema de archivos.

---

## 2. Información General de Diseño

- **Diseño de Alto Nivel:** El sistema está diseñado para ser altamente disponible y escalable, utilizando Kubernetes como plataforma de orquestación de contenedores. Se emplea un balanceador de cargas Nginx para distribuir el tráfico de manera eficiente y asegurar la disponibilidad de la aplicación Drupal.
  
- **Arquitectura:**
  - **Clúster Kubernetes en AWS EKS:** Utilización de Amazon EKS para gestionar el clúster de Kubernetes, aprovechando los servicios administrados para mejorar la disponibilidad y reducir la complejidad de gestión.
  - **Ingress NGINX:** Configuración de Ingress NGINX para gestionar el enrutamiento del tráfico hacia los servicios internos del clúster.
  - **AWS EFS CSI Driver:** Integración del driver CSI de AWS EFS para proporcionar almacenamiento persistente a las aplicaciones desplegadas.
  - **Cert Manager y Let's Encrypt:** Implementación de Cert Manager junto con Let's Encrypt para la gestión automática de certificados SSL, asegurando las comunicaciones HTTPS.
  - **Kubectl:** Herramienta principal para la gestión y despliegue de recursos en el clúster Kubernetes.
    
  ![image](https://github.com/user-attachments/assets/cb13ad10-0c78-472a-af41-adaf94ccffdf)

- **Patrones Utilizados:**
  - **Infraestructura como Código (IaC):** Uso de manifiestos YAML para definir y gestionar toda la infraestructura y los recursos del clúster.
  - **Microservicios:** Descomposición de la aplicación en servicios independientes (CMS, base de datos, sistema de archivos) que pueden escalarse y gestionarse de manera autónoma.
  - **Declarative Configuration:** Adopción de configuraciones declarativas para asegurar la consistencia y reproducibilidad del entorno.

- **Mejores Prácticas Implementadas:**
  - **Uso de Servicios Administrados:** Aprovechamiento de servicios administrados como AWS EKS y AWS EFS para mejorar la fiabilidad y reducir la carga operativa.
  - **Seguridad por Defecto:** Configuración de HTTPS y gestión segura de credenciales para proteger la infraestructura y los datos.
  - **Autoescalado:** Implementación de políticas de autoescalado para asegurar que la infraestructura puede adaptarse a las variaciones en la carga de trabajo.
  - **Despliegue Continuo:** Uso de herramientas y prácticas que facilitan el despliegue continuo y la integración continua (CI/CD) para mejorar la eficiencia del desarrollo y despliegue.
 



---

## 3. Ambiente de Desarrollo y Técnico

### 3.1. Lenguajes de Programación y Tecnologías

- **Lenguaje Principal:** YAML para la definición de manifiestos de Kubernetes.
- **Frameworks/Librerías:**
  - **Kubernetes 1.30 :** Orquestación de contenedores y gestión de la infraestructura.
  - **Ingress NGINX:** Gestión del enrutamiento del tráfico HTTP/HTTPS.
  - **AWS EFS CSI Driver:** Integración de almacenamiento persistente.
  - **Cert Manager:** Gestión de certificados SSL.
- **Herramientas y Paquetes:**
  - **AWS CLI:** Herramienta para conectarse y gestionar configuraciones con el servicio de AWS.
  - **kubectl:** Herramienta de línea de comandos para interactuar con Kubernetes.
  - **Docker:** Contenerización de aplicaciones, específicamente para la base de datos.
  - **Helm (opcional):** Gestión de paquetes para Kubernetes.

### 3.2. Detalles del Desarrollo y Técnicos

El desarrollo se realizó principalmente mediante la creación y gestión de manifiestos YAML que definen los recursos de Kubernetes necesarios para desplegar el CMS Drupal, el balanceador de cargas, la base de datos y el sistema de archivos. Se utilizaron las siguientes configuraciones y prácticas técnicas:

- **Despliegue de Drupal CMS:** Definido como un Deployment en Kubernetes, con réplicas para asegurar la alta disponibilidad.
- **Balanceador de Cargas NGINX:** Configurado como un Ingress Controller para gestionar el tráfico entrante y distribuirlo entre las instancias de Drupal.
- **Base de Datos:** Desplegada como un Deployment con una única réplica, utilizando un contenedor Docker para PostgreSQL (o MySQL según se requiera).
- **Sistema de Archivos NFS:** Implementado utilizando AWS EFS a través del AWS EFS CSI Driver, montando el almacenamiento persistente en los pods necesarios.
- **Gestión de Certificados:** Configuración de Cert Manager para solicitar y renovar certificados SSL automáticamente mediante Let's Encrypt.
- **Autoescalado:** Definición de Horizontal Pod Autoscalers (HPA) para escalar automáticamente las aplicaciones según la carga de trabajo.
- **Configuración de Networking:** Definición de servicios Kubernetes (ClusterIP, NodePort, LoadBalancer) para gestionar el acceso interno y externo a los servicios.

### 3.3. Configuración de Parámetros del Proyecto

La configuración de los parámetros del proyecto se realizó mediante la definición de variables de entorno y configuraciones específicas en los manifiestos YAML. A continuación, se detallan los principales parámetros configurados:

- **Dominio y SSL:**
  - **Dominio:** `reto2-toptel.site`
  - **Certificado SSL:** Gestionado por Cert Manager y Let's Encrypt, configurado para emitir certificados para el dominio especificado.
  
- **Networking:**
  - **IPs y Puertos:** Configuración de Ingress para gestionar el tráfico en el puerto 443 (HTTPS) y redirigirlo a los servicios internos.
  - **DNS:** Configuración de registros DNS para apuntar el dominio `reto2-toptel.site` al balanceador de cargas de Kubernetes.
  
- **Conexión a Bases de Datos:**
  - **Tipo de Base de Datos:**  MySQL desplegada dentro del clúster Kubernetes.
  - **Configuración de Conexión:** Variables de entorno definidas en los manifiestos de Deployment para establecer las credenciales y la cadena de conexión a la base de datos.
  
- **Sistema de Archivos:**
  - **NFS Server:** Implementado mediante AWS EFS CSI Driver, montando el sistema de archivos en los pods que lo requieren.
  - **Parámetros de Montaje:** Definición de PersistentVolume y PersistentVolumeClaim para gestionar el almacenamiento persistente.
  
- **Variables de Ambiente:**
  - **Configuraciones de Drupal:** Variables como `DRUPAL_DB_HOST`, `DRUPAL_DB_USER`, `DRUPAL_DB_PASSWORD`, definidas en los manifiestos de Deployment.
  - **Configuraciones de NGINX:** Parámetros específicos para la configuración del balanceador de cargas, gestionados a través de ConfigMaps.
  
- **Parámetros de Autoescalado:**
  - **HPA Configuración:** Definición de thresholds de CPU y memoria para escalar las réplicas de los Deployments según la demanda.
  
- **Cert Manager y Let's Encrypt:**
  - **Issuer:** Configuración de Issuer o ClusterIssuer en Cert Manager para gestionar la emisión de certificados.
  - **Certificados:** Definición de recursos de Certificate para solicitar certificados SSL para el dominio especificado.
  
### 3.4. Configuración de Entorno de Trabajo AWS CLI

#### Acceder a las Credenciales de AWS en Vocareum
    
   1. Dirígete a **Vocareum** y selecciona **AWS Details**.
       
       ![Acceder a AWS Details](https://github.com/user-attachments/assets/d431b314-969f-446b-ae7d-bc62ad137484)
    
   2. Haz clic en **Cloud Access** dentro de AWS CLI y selecciona **SHOW** para ver tus credenciales.
       
       ![Mostrar Credenciales](https://github.com/user-attachments/assets/914e9cff-b4a8-4dbe-b5e6-4c9db3129702)
    
   3. Verás tus **Keys** (Access Key, Secret Access Key y Session Token). Estas credenciales son temporales y se utilizan para autenticarte en la CLI de AWS.
       
       ![Visualizar Keys](https://github.com/user-attachments/assets/daf0c555-7562-42e7-bf79-ede1efb2cd52)
    
   #### Configurar AWS CLI en PowerShell
    
   1. Abre **PowerShell** como administrador en tu máquina local.
    
   2. Configura tus credenciales de AWS ejecutando los siguientes comandos. Asegúrate de reemplazar los valores entre comillas por las credenciales actuales de tu sesión en AWS:
    
        ```powershell
        $env:AWS_ACCESS_KEY_ID = "#############"
        $env:AWS_SECRET_ACCESS_KEY = "###########"
        $env:AWS_SESSION_TOKEN = "#############"
        ```
    
   3. Actualiza la configuración de tu clúster EKS cada vez que inicies una nueva sesión para asegurarte de tener acceso actualizado a los recursos. Ejecuta el siguiente comando en AWS CLI:
    
        ```powershell
        aws eks update-kubeconfig --name Monda --region us-east-1
        ```
    
        *Nota: El nombre del cluster es Monda.*
        
        ![Actualizar Kubeconfig](https://github.com/user-attachments/assets/b0b69cab-8925-4b6d-9bf1-4430fcde5143)
    
Estos pasos aseguran que las credenciales de AWS estén correctamente configuradas en tu entorno de desarrollo, permitiendo una interacción segura y eficiente con los recursos de AWS necesarios para el despliegue del clúster Kubernetes.

---

## 4. Ambiente de Ejecución (Producción)

### 4.1. Tecnologías en Producción

- **Lenguaje Principal:** YAML para la definición de manifiestos de Kubernetes.
- **Frameworks/Librerías:**
  - **Kubernetes:** Orquestación de contenedores y gestión de la infraestructura.
  - **Ingress NGINX:** Gestión del enrutamiento del tráfico HTTP/HTTPS.
  - **AWS EFS CSI Driver:** Integración de almacenamiento persistente.
  - **Cert Manager:** Gestión de certificados SSL.
  - **Amazon EKS:** Servicio de Kubernetes administrado para la orquestación de contenedores.
- **Servicios de AWS:**
  - **Amazon EKS (Elastic Kubernetes Service):** Plataforma administrada para ejecutar Kubernetes en AWS, facilitando la implementación, gestión y escalado de aplicaciones basadas en contenedores.
  - **Instancias EC2 (Elastic Compute Cloud):** Utilizadas en grupos de nodos del clúster EKS para ejecutar los pods de Kubernetes, proporcionando la capacidad de cómputo necesaria.
  - **CloudFormation:** Automatización de la infraestructura mediante plantillas de CloudFormation para la configuración y gestión de la VPC (Virtual Private Cloud), subredes, y otros recursos de red.
  - **Amazon EFS (Elastic File System):** Sistema de archivos elásticos para almacenamiento compartido, utilizado por los pods para el almacenamiento persistente de datos.
  - **Amazon RDS (Relational Database Service):** (Opcional) Servicio administrado para bases de datos relacionales, en caso de optar por una solución administrada para la base de datos.
  - **AWS IAM (Identity and Access Management):** Gestión de roles y permisos para asegurar el acceso adecuado a los recursos de AWS.
  - **AWS Certificate Manager:** Automatización de la provisión, administración y despliegue de certificados SSL/TLS para asegurar las comunicaciones.
- **Herramientas y Paquetes:**
  - **kubectl:** Herramienta de línea de comandos para interactuar con Kubernetes.
  - **Docker:** Contenerización de aplicaciones, específicamente para la base de datos.
  - **Helm (opcional):** Gestión de paquetes para Kubernetes.

### 4.2. Dominios y Direcciones

- **IP/Nombres de Dominio:** `reto2-toptel.site`
- **URL de Acceso:** [https://reto2-toptel.site](https://reto2-toptel.site)   

### 4.3. Configuración de Parámetros en Producción

En esta sección se detalla la configuración de los parámetros del proyecto en el ambiente de producción, abarcando aspectos como la configuración de la VPC, creación del clúster EKS, configuración de grupos de nodos, verificación de grupos de seguridad y configuración de Amazon EFS.

#### 1. Configurar CloudFormation para la VPC

Para establecer la VPC necesaria para el clúster Kubernetes, se utilizará una plantilla de CloudFormation específica para IPv4 disponible en la documentación oficial de AWS.

1. **Acceder a la Plantilla de CloudFormation:**
   
   Visita el siguiente enlace para obtener la plantilla de CloudFormation para la VPC:
   
   [Crear una VPC con CloudFormation](https://docs.aws.amazon.com/eks/latest/userguide/creating-a-vpc.html)

2. **Implementar la Plantilla en CloudFormation:**
   
   - Navega a la consola de **CloudFormation** en AWS.
   - Selecciona **Crear una pila** y elige **Subir un archivo de plantilla**.
   - Sube la plantilla descargada y sigue las instrucciones para configurar los parámetros necesarios.
   - Esto establecerá la VPC denominada `funcionaaa` con la configuración requerida para el clúster EKS.
     
![image](https://github.com/user-attachments/assets/4c8f735c-6691-44d3-accf-36982425a242)

![image](https://github.com/user-attachments/assets/f8b8096c-645d-46e7-9106-3aae20720751)


#### 2. Crear el Clúster en Amazon EKS

Una vez configurada la VPC, procede a crear el clúster de Kubernetes en Amazon EKS.


1. **Acceder a Amazon EKS:**
   
   - En la consola de AWS, navega a **Amazon EKS**.

2. **Crear un Nuevo Clúster:**
   
   - Selecciona **Crear clúster**.
   - Configura los siguientes parámetros:
     - **Nombre del clúster:** Por ejemplo, `monda`.
     - **Versión de Kubernetes:** Selecciona la versión **1.30**.
     - **VPC:** Elige la VPC creada anteriormente (`funcionaaa`).
     - **Add-ons:** Añade todos los add-ons disponibles que ofrece EKS para mejorar la funcionalidad y gestión del clúster.
   
3. **Finalizar la Creación:**
   
   - Revisa la configuración y confirma la creación del clúster.
   - Espera a que el clúster se cree completamente antes de proceder al siguiente paso.
![image](https://github.com/user-attachments/assets/50374753-02f8-4bef-8b3f-fa28a305570b)

#### 3. Configurar Grupos de Nodos en el Clúster EKS

Después de crear el clúster, es necesario configurar los grupos de nodos que ejecutarán los pods de Kubernetes.

1. **Acceder a la Sección de Compute:**
   
   - Dentro del clúster EKS recién creado, ve a la sección **Compute**.
   - Selecciona **Agregar grupo de nodos**.

2. **Configurar el Grupo de Nodos:**
   
   - **Nombre del Grupo de Nodos:** Por ejemplo, `nodos`.
   - **VPC:** Selecciona la VPC `funcionaaa`.
   - **Tipo de Instancia:** Elige `t3.medium` para asegurar una buena cantidad de RAM y capacidad para alojar suficientes pods para los servicios.
   - **Configuraciones Adicionales:** Añade cualquier configuración adicional requerida por los add-ons seleccionados.
   
3. **Finalizar la Creación del Grupo de Nodos:**
   
   - Revisa y confirma la configuración.
   - Espera a que los nodos se creen y se unan al clúster.
![image](https://github.com/user-attachments/assets/b7a590b5-e3c7-4920-a079-711445d7b849)

![image](https://github.com/user-attachments/assets/7ca73f7e-52f0-4db0-abca-63970bab2236)


#### 4. Verificar Grupos de Seguridad en EC2

Es crucial asegurarse de que los grupos de seguridad estén correctamente configurados para permitir el tráfico necesario entre los componentes del clúster.

1. **Acceder a la Consola de EC2:**
   
   - En la consola de AWS, navega a **EC2**.
   - Ve a **Grupos de Seguridad**.

2. **Verificar Grupos de Seguridad del Clúster:**
   
   Asegúrate de que los grupos de seguridad para los nodos **master** y **slave** estén creados y configurados con los siguientes puertos:

   | Grupo de Seguridad ID        | Tipo de Tráfico       | Protocolo | Puerto   | Origen          |
   |-------------------------------|-----------------------|-----------|----------|-----------------|
   | `sg-f8bd5e91`                 | All UDP               | UDP       | 0-65535  | -               |
   | `sg-04552953a85a0b89c`        | Custom TCP            | TCP       | 9443     | `0.0.0.0/0`     |
   | `sg-0e8226a29ed3f4782`        | All ICMP - IPv4       | ICMP      | All      | -               |
   | `sg-0c397473c67eafdb1`        | All UDP               | UDP       | 0-65535  | -               |
   | `sg-04e2626b7eb109a8b`        | All TCP               | TCP       | 0-65535  | -               |
   | `sg-073e45933697d6fee`        | Custom TCP            | TCP       | 8890     | `0.0.0.0/0`     |
   | `sg-03c23aeef56c24ae0`        | SSH                   | TCP       | 22       | `0.0.0.0/0`     |
   | `sg-070d16283f1668f72`        | Custom TCP            | TCP       | 8888     | `0.0.0.0/0`     |
   | `sg-025b09c43a986be2d`        | All TCP               | TCP       | 0-65535  | -               |
   
   - **Nota:** Asegúrate de que estos grupos de seguridad están asociados correctamente a los nodos correspondientes y que las reglas permiten el tráfico necesario para la operación del clúster.
     
![image](https://github.com/user-attachments/assets/8c5fdfef-e5cf-46f0-b9df-a5a98fa00661)


#### 5. Configurar Amazon EFS para Almacenamiento Persistente

Para gestionar el almacenamiento persistente, se configurará un sistema de archivos EFS que será accesible por los nodos del clúster.

1. **Crear un Grupo de Seguridad para EFS:**
   
   - En la consola de **EC2**, crea un nuevo **Grupo de Seguridad**.
   - **Nombre del Grupo de Seguridad:** Por ejemplo, `EFS`.
   - **Regla de Ingreso:** Abre el puerto **2049** (NFS) para permitir el tráfico desde los nodos del clúster.
   
2. **Configurar el Sistema de Archivos EFS:**
   
   - Navega a **Amazon EFS** en la consola de AWS.
   - Selecciona **Crear sistema de archivos**.
   - **VPC:** Selecciona la VPC `funcionaaa`.
   - **Grupos de Seguridad:** Asigna el grupo de seguridad `EFS` creado anteriormente.
   - **Subredes:** Selecciona las subredes públicas y privadas donde el sistema de archivos se comunicará.
   - **Nombre del EFS:** Por ejemplo, `MOnda`.
   
4. **Integración con Kubernetes:**
   
   - Utiliza el **AWS EFS CSI Driver** para montar el sistema de archivos en los pods necesarios.
   - Define **PersistentVolume** y **PersistentVolumeClaim** en los manifiestos YAML de Kubernetes para gestionar el almacenamiento persistente.
  
   ![image](https://github.com/user-attachments/assets/1fe7acc5-465f-45bd-be60-c5a1a9602494)


#### Resumen de Parámetros Configurados

- **VPC:**
  - **Nombre:** `funcionaaa`
  - **Configuración:** Establecida mediante plantilla de CloudFormation para IPv4.
  
- **Clúster EKS:**
  - **Nombre:** `reto2-cluster`
  - **Versión de Kubernetes:** 1.30
  - **Add-ons:** Todos los disponibles seleccionados.
  - **Grupos de Nodos:** 
    - **Nombre:** `nodos`
    - **Tipo de Instancia:** `t3.medium`
  
- **Grupos de Seguridad:**
  
  | ID de SG                    | Descripción                     | Reglas Principales                                 |
  |-----------------------------|---------------------------------|----------------------------------------------------|
  | `sg-f8bd5e91`               | ElasticMapReduce-master         | All UDP (0-65535)                                  |
  | `sg-04552953a85a0b89c`      | ElasticMapReduce-master         | Custom TCP (9443) desde `0.0.0.0/0`                |
  | `sg-0e8226a29ed3f4782`      | ElasticMapReduce-master         | All ICMP - IPv4, All TCP (0-65535)                 |
  | `sg-0c397473c67eafdb1`      | ElasticMapReduce-slave          | All UDP (0-65535)                                  |
  | `sg-04e2626b7eb109a8b`      | ElasticMapReduce-slave          | All TCP (0-65535)                                  |
  | `sg-073e45933697d6fee`      | ElasticMapReduce-slave          | Custom TCP (8890) desde `0.0.0.0/0`                |
  | `sg-03c23aeef56c24ae0`      | ElasticMapReduce-slave          | SSH (22) desde `0.0.0.0/0`                         |
  | `sg-070d16283f1668f72`      | ElasticMapReduce-slave          | Custom TCP (8888) desde `0.0.0.0/0`                |
  | `sg-025b09c43a986be2d`      | ElasticMapReduce-slave          | All TCP (0-65535), All ICMP - IPv4, All UDP (0-65535) |

- **EFS:**
  - **ID de EFS:** `fs-0055cafdfa44918a6`
  - **ID de Access Point:** `fsap-06d2601af581672a7`
  - **Grupo de Seguridad:** `efs-sg`
  - **Puerto Abierto:** 2049 (NFS)

Estos parámetros aseguran que la infraestructura esté correctamente configurada para soportar el despliegue de la aplicación en un ambiente de producción, garantizando la conectividad, seguridad y persistencia necesarias para el funcionamiento eficiente del CMS Drupal en el clúster Kubernetes.

---


### 4.4. Despliegue del Servidor

#### 1.Aplicar el Cluster Issuer:


 ```powershell
kubectl apply -f cluster-issuer.yaml
```
#### 2.Verificar el Cluster Issuer:

```powershell
kubectl get clusterissuer
```
Debería salir:
```powershell
NAME               READY   AGE
letsencrypt-prod   True    30s
```
![image](https://github.com/user-attachments/assets/b4ca2a40-d4d1-4a42-b26b-951ecf41f840)

#### 3.Crear el StorageClass del EFS:

```powershell
kubectl apply -f efs-storageclass.yaml
```
#### 4.Verificar el StorageClass del EFS:

Debería salir:
```powershell
kubectl get storageclass
```
Debería salir:
```powershell
NAME            PROVISIONER         RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
efs-sc          efs.csi.aws.com     Delete          Immediate           false                  10s
```
![image](https://github.com/user-attachments/assets/848236ad-da56-474d-bfc3-4d49185bb308)

#### 5.Crear los secrets:
```powershell
kubectl apply -f mysql-secret.yaml
kubectl apply -f drupal-secret.yaml
```
#### 6.Verificar los secrets:

```powershell
kubectl get secrets
```

Debería salir:
```powershell
NAME                   TYPE                                  DATA   AGE
mysql-secret           Opaque                                3      5s
drupal-secret          Opaque                                2      5s
```
![image](https://github.com/user-attachments/assets/a969d75a-4723-4e86-89bf-25c2b113cc9f)

#### 6.Crear los PV y los PVC:

```powershell
kubectl apply -f mysql-pv-pvc.yaml
kubectl apply -f drupal-pv-pvc.yaml
```
#### 7.Verificar los PV:
```powershell
kubectl get pv
```
Debería salir:
```powershell
NAME         CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM               STORAGECLASS   REASON   AGE
mysql-pv     20Gi       RWO            Retain           Bound    default/mysql-pvc   efs-sc                  5s
drupal-pv    10Gi       RWX            Retain           Bound    default/drupal-pvc  efs-sc                  5s
```
#### 8.Verificar los PVC:
```powershell
kubectl get pvc
```

Debería salir:
```powershell
NAME          STATUS   VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pvc     Bound    mysql-pv   20Gi       RWO            efs-sc         5s
drupal-pvc    Bound    drupal-pv  10Gi       RWX            efs-sc         5s
```

![image](https://github.com/user-attachments/assets/85c01f04-a29f-41f7-87b5-fc1e92909eb5)

#### 9.Desplegar MYSQL:
```powershell
kubectl apply -f mysql-deployment.yaml
```
![image](https://github.com/user-attachments/assets/222eb3ac-c34b-4378-85be-c6ef9c60d68d)

#### 10.Verificar MYSQL:
```powershell
kubectl get pods -l app=mysql
```
Debería salir:
```powershell
NAME                     READY   STATUS    RESTARTS   AGE
mysql-xyz123             1/1     Running   0          10s
```

```powershell
kubectl logs deployment/mysql
```
Debería salir:
```powershell
[Entrypoint] MySQL Docker Image 8.0.XX-XX
[Entrypoint] Initializing database
...
[Server] ready for connections.
```
#### 11.Desplegar Drupal:
```powershell
kubectl apply -f drupal-deployment.yaml
```
#### 12.Verificar Drupal:
```powershell
kubectl get pods -l app=drupal
```
Debería salir:
```powershell
NAME                      READY   STATUS    RESTARTS   AGE
drupal-abc123             1/1     Running   0          10s
```

```powershell
kubectl logs deployment/drupal
```

Debería salir:
```powershell
AH00558: apache2: Could not reliably determine the server's fully qualified domain name
...
[notice] Apache/2.4.XX (Unix) configured -- resuming normal operations
```
#### 13.Crear el Ingress:
```powershell
kubectl apply -f drupal-ingress.yaml
```
#### 14.Verificar el Ingress:
```powershell
kubectl get ingress
```
Debería salir:
```powershell
NAME             CLASS    HOSTS             ADDRESS        PORTS     AGE
drupal-ingress   <none>   reto2-toptel.site   x.x.x.x        80, 443   5s
```
#### 15.Verificar certificado TLS:

```powershell
kubectl describe certificate reto2-toptel-site-tls
```

Debería salir:
```powershell
...
Status:
  Conditions:
    Type:    Ready
    Status:  True
...
Not After:  2024-01-01T00:00:00Z
```
#### 16.Escalar Drupal a Dos Replicas:
```powershell
kubectl scale deployment drupal --replicas=2
```

#### 17.Verificar las Dos Replicas de Drupal:
```powershell
kubectl get pods -l app=drupal
```
Debería salir:
```powershell
NAME                      READY   STATUS    RESTARTS   AGE
drupal-abc123             1/1     Running   0          2m
drupal-def456             1/1     Running   0          10s
```
![image](https://github.com/user-attachments/assets/24913304-443e-4901-b7d6-de38127fd63e)


#### 18.Entrar al sitio web y observar las instancias de drupal:
Verificar en el sitio web en mi caso reto2-toptel.site para ver si funciono la instalación.

![image](https://github.com/user-attachments/assets/de95bf0b-870c-4fe0-8b9a-a9e256fd137a)


### 4.5. Guía de Usuario

Con el proceso de instalación realizado la pagina web ya queda configurada para el usuario por lo que realmente, no necesita mas que empezar a usar el servicio. Se deja un tutorial https://www.youtube.com/watch?v=vCp9vFKKDgI&list=PLDbrnXa6SAzVLc19x0x0XPw8Fool9iRMW&ab_channel=dfbastidas de como puede usar el servicio de Drupal , algo que ya va por fuera del ejercicio.


## 5. Referencias

1. **Apasoft Training.** [Despliegue de Aplicaciones en Kubernetes - Video 7](https://www.youtube.com/watch?v=1wnbPZsc16I&list=PLkqaOL-oB94HAIRkA_5qdqk-x-1Hgo4i2&index=7&ab_channel=ApasoftTraining). *YouTube*. Accedido el 6 de noviembre de 2024.

2. **Shubham K. Sawant.** "Deploy Drupal App on Kubernetes." *Medium*. Disponible en: [https://shubhamksawant.medium.com/deploy-drupal-app-on-kubernetes-a56244e514ca](https://shubhamksawant.medium.com/deploy-drupal-app-on-kubernetes-a56244e514ca). Accedido el 6 de noviembre de 2024.

3. **st0263eafit.** "eks-wp." *GitHub*. Disponible en: [https://github.com/st0263eafit/st0263-242/tree/main/eks-wp](https://github.com/st0263eafit/st0263-242/tree/main/eks-wp). Accedido el 6 de noviembre de 2024.

4. **Nosotrs mismos** Por nuestro mi Ingenio y mas de 35 horas de trabajo. Este ha sido el trabajo por mucho al que le hemos dedicado más tiempo en nuestra carrera universitaria(todo por solucionar un problema que no se puede solucionar) .
---



