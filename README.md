# Desarrollo Web en ASP.NET Core 5 (2021)

Mini ejercicio de control de ingresos y gastos realizado en ASP.NET Core 5 como parte de un curso gratis que tomé de Udemy.

## <font color="green">Herramientas necesarias para hacer funcionar .NET en Linux</font>

**OS:** GarudaLinux
**Distro:** ArchLinux

#### 1. Instalar Visual Studio Code (no Code-OSS).
> sudo pacman -S visual-studio-code-bin

***

#### 2. Instalar dotnet (no usando snap)

* La instalación se realiza a través del siguiente script:
> https://dot.net/v1/dotnet-install.sh

* Una vez obtenido el script, se ejecuta el siguiente comando:
> **sh dotnet-install.sh --install-dir /usr/share/dotnet -channel Current -version XXXX**
>
> **NOTA:** Se debe reemplazar **XXXX** con la versión que se desea.  En caso fuese la última registrada, usar **latest**.
>
> **Ej.:**
> **sh dotnet-install.sh --install-dir /usr/share/dotnet -channel Current -version latest**
> **sh dotnet-install.sh --install-dir /usr/share/dotnet -channel Current -version 5.0.406**

***

#### 3. Configurar variables de entorno

* Se debe configurar la variable **_DOTNET_ROOT_** dentro de las variables de entorno.  En Arch Linux se hace dentro del archivo:
> **/etc/environment**
- Y se debe ingresar la siguiente línea:
> **export DOTNET_ROOT="/usr/share/dotnet"**
- Al finalizar, se debe ejecutar el siguiente comando para que los cambios se apliquen:
> **source /etc/environment**

* Se debe configurar la variable PATH en el archivo **_/etc/profile_** las siguientes rutas:
> - **/usr/share/dotnet**
> - **/home/$USER/.dotnet/tools**

***

#### 4. Configurar base de datos SQL Server

* Para este proyecto se hará uso de un contenedor de Docker para contar con SQL Server.  Para ello ya se debe contar con docker:
> **sudo pacman -S docker**

* Una vez esta instalado y levantado el servicio de docker, se debe ejecutar el siguiente comando, para obtener la imagen de SQL Server:
> **docker pull mcr.microsoft.com/mssql/server**

* Una vez ha sido descargada la imagen, se levanta el servicio de SQL Server con el siguiente comando:
> **docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server**
>
>> Como se puede observar, se utiliza un volumen para poder persistir los datos de la aplicación.

***

#### 5. Crear proyecto ASP.NET MVC (.NET Core 5)

* Desde la terminal se debe ejecutar el siguiente comando:
> **dotnet new mvc -o <nombre_de_mi_proyecto> -f net5.0**
* Una vez creado el proyecto, se debe crear el archivo **_global.json_** para especificar la versión del SDK de .NET Core:
> **dotnet new globaljson --sdk-version 5.0**

***

#### 6. Instalar la herramienta de EntityFramework y Dotnet-aspnet-codegenerator

* Desde la terminal, dentro de la carpeta del proyecto, se debe ejecutar el siguiente comando para instalar la herramienta de EntityFramework:
> **dotnet tool install -g dotnet-ef**
>
>> Esta herramienta sirve para ejecutar las migraciones y hacer actualizaciones a la base de datos.

* Para instalar la herramienta de Dotnet-aspnet-codegenerator se debe ejecutar el siguiente comando:
> **dotnet tool install -g dotnet-aspnet-codegenerator**
> **dotnet tool update -g dotnet-aspnet-codegenerator**
>
>> Esta herramienta sirve para poder generar controladores y otras funciones que brinda Visual Studio.

## <font color="green">Utilidades</font>

### Migraciones

* Para efectuar correctamente una migración, se debe ejecutar el siguiente comando:
> **dotnet-ef migrations add <nombre_de_mi_migracion>**

* Una vez creada la migración, se puede hacer la actualización de la base de datos con respecto a sus migraciones:
> **dotnet-ef database update**

### Generar un controlador

* El comando para generar un controlador en base a un Modelo, un DbContext y un Layout (como se haría en Visual Studio) es el siguiente:
> **dotnet-aspnet-codegenerator controller -name <nombre_controlador>Controller -m <nombre_de_modelo> -dc <nombre_de_db_context> -l <ruta_del_layout>**