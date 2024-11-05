# Info de la materia: ST0263 - Topicos Especiales en Telematica
#
# Estudiante(s): Santiago Arias Higuita - sariash@eafit.edu.co  <br>  Luis Miguel Giraldo Gonzalez - lmgiraldo4@eafit.edu.co
#
# Profesor: Alvaro Enrique Ospina Sanjuan - aeospinas@eafit.edu.co
#
# Desplegar un CMS en un clúster de alta disponibilidad en Kubernetes autoescalado en nube, con su propio dominio y certificado SSL


# 1. Descripción de la actividad


## 1.1. Que aspectos cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)
### Requerimientos Funcionales


### Requerimientos No Funcionales


## 1.2. Que aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)



# 2. Información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.



# 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.


## Detalles del desarrollo y tecnicos.


## Descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)



## 

# 4. Descripción del ambiente de EJECUCIÓN (en producción) lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

# IP o nombres de dominio en nube o en la máquina servidor.


## Descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)


## Como se lanza el servidor.



## Una mini guia de como un usuario utilizaría el software o la aplicación
### Acceder a las Credenciales de AWS en Vocareum

1. Dirígete a **Vocareum** y selecciona **AWS Details**.

   ![image (2)](https://github.com/user-attachments/assets/d431b314-969f-446b-ae7d-bc62ad137484)


2. Luego, haz clic en **Cloud Access** dentro de AWS CLI y selecciona **SHOW** para ver tus credenciales.
   
   ![image (3)](https://github.com/user-attachments/assets/914e9cff-b4a8-4dbe-b5e6-4c9db3129702)

  

4. Verás tus **Keys** (Access Key, Secret Access Key y Session Token). Estas credenciales son temporales y se utilizan para autenticarte en la CLI de AWS.
   
   ![image (4)](https://github.com/user-attachments/assets/daf0c555-7562-42e7-bf79-ede1efb2cd52)

   

### Configurar AWS CLI en PowerShell

1. Abre **PowerShell** como administrador en tu máquina local.

2. Configura tus credenciales de AWS ejecutando los siguientes comandos. Asegúrate de reemplazar los valores entre comillas por las credenciales actuales de tu sesión en AWS:

   ```powershell
   $env:AWS_ACCESS_KEY_ID = "#############"
   $env:AWS_SECRET_ACCESS_KEY = "###########"
   $env:AWS_SESSION_TOKEN = "#############"

3. Es necesario actualizar la configuración de tu clúster EKS cada vez que inicies una nueva sesión, para asegurarte de tener acceso actualizado a los recursos.

Ejecuta el siguiente comando en AWS CLI:

   ```powershell
   aws eks update-kubeconfig --Monda --region us-east-1
   ```

Nota: El nombre del cluster es Monda
![image (5)](https://github.com/user-attachments/assets/b0b69cab-8925-4b6d-9bf1-4430fcde5143)

4. Verifica el estado de los nodos con el siguiente comando

   ```powershell
   kubectl get nodes
   ```

5. Verifica que los pods están en estado de Running

   ```powershell
   kubectl get pods
   ```

## Otras configuraciones
### Creacion de EFS en AWS
Este paso describe cómo crear un sistema de archivos EFS y configurarlo para que los nodos de tu clúster EKS puedan comunicarse entre sí.

#### Crear un Grupo de Seguridad
Primero, es necesario crear un grupo de seguridad para los nodos de EFS. Este grupo permitirá que los nodos se comuniquen entre sí. Si ya tienes un clúster en funcionamiento, asegúrate de configurar correctamente este grupo de seguridad para evitar problemas de conexión.

![image (6)](https://github.com/user-attachments/assets/bd9f7bc1-5eef-433c-b27d-1dc6896e49df)


#### Asociar el Grupo de Seguridad y Asignar el Rol IAM
Asocia el grupo de seguridad recién creado a la EFS y asegúrate de asignar el rol **Labrole IAM** a la EFS. Esto permitirá que los nodos tengan los permisos necesarios para acceder al sistema de archivos EFS.

#### Crear un Access Point (Punto de Acceso)
Dentro de la consola de EFS, dirígete a la sección **Puntos de acceso** y crea un nuevo Access Point. Este punto de acceso será utilizado por los nodos del clúster para acceder al sistema de archivos de manera controlada.
![image (7)](https://github.com/user-attachments/assets/d0827490-64cf-4b06-85a0-aa3c831a1d8a)
![image (8)](https://github.com/user-attachments/assets/e9067629-0283-4135-a751-9a48669a2ced)


Id de efs: **fs-0055cafdfa44918a6** <br>
id de accespint:**fsap-06d2601af581672a7**



## Pantallazos (Opcionales)


# Referencias:




