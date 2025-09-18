# Manual paso a paso para instalar y configurar JetBrains Rider en macOS (chip Apple M2)


## 1. Instalación de Homebrew

1. Abre la **Terminal** y comprueba si Homebrew está instalado:
    
    ```shell
    brew --version
    ```

2. Si no aparece ningún número de versión, instala Homebrew con el siguiente script oficial (necesita privilegios de administrador). El comando descarga y ejecuta el instalador:
    
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
    
3. Al finalizar la instalación, ejecuta en el terminal las instrucciones que pone al final:

```shell
echo >> /Users/nombreusuario/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/nombreusuario/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

4. Verifica que todo funciona:

```shell
brew --version
```
    

## 2. Instalación del SDK de .NET para Apple Silicon

El SDK de .NET ofrece las herramientas necesarias para compilar proyectos en C#, Web API y .NET MAUI. En Macs con procesadores ARM, las versiones **arm64** de .NET se instalan en `/usr/local/share/dotnet/`. Utiliza Homebrew para instalar la versión actual del SDK (se instalará la variante compatible con Apple M2):

```shell
brew install --cask dotnet-sdk
```

También puedes instalar una versión específica con `brew install --cask dotnet-sdk@<version>` (`brew install --cask dotnet-sdk@9`). 

Para actualizar, ejecuta:

```shell
brew upgrade --cask dotnet-sdk
```

Una vez instalado, comprueba la instalación con estos comandos:

```shell
which dotnet      # indica la ruta del ejecutable  
dotnet --info     # muestra información detallada de la instalación  
dotnet --version  # muestra la versión instalada  dotnet --list-sdks`
```

Comandos básicos de `dotnet`:

- `dotnet new --list`: muestra plantillas.
    
- `dotnet new console -o NombreProyecto`: crea un proyecto de consola.
	

## 3. Instalación JetBrains Rider (Apple Silicon)

JetBrains ofrece un instalador específico para procesadores Apple Silicon. Puedes descargar la imagen `.dmg` directamente. Asegúrate de descargar la versión para **Apple Silicon** si es el caso.

1. Descarga el archivo `.dmg` de Rider (Apple Silicon) desde [la página de descargas de Rider](https://www.jetbrains.com/rider/download/).
    
2. Monta la imagen y arrastra **JetBrains Rider.app** a la carpeta **Applications** [jetbrains.com](https://www.jetbrains.com/help/rider/Installation_guide.html#:~:text=1,image).
    
3. Abre la aplicación desde **Launchpad** o **Applications**.
    
4. Acepta el contrato de licencia y selecciona la opción "Free Non-Commercial Use"; es necesario registrarse.

> En **Featured Plugings** marca ***"Rider Android Support"***, utilizado en MAUI.

> Hasta aquí son los pasos necesarios para el curso de "Introducción a C#" y "Desarrollo backend mediante API REST con .NET"


## 4. Instalación de _workloads_ de .NET MAUI

Para desarrollar aplicaciones multiplataforma con MAUI necesitas instalar las cargas de trabajo (_workloads_) desde la CLI:

```shell
# Instala el workload principal de MAUI 
sudo dotnet workload install maui  
# Instala herramientas WebAssembly para aplicaciones Blazor (web) 
sudo dotnet workload install wasm-tools  
# Lista los workloads instalados 
dotnet workload list
```


## 5. Instalación del JDK (Java 17) con Homebrew

.NET MAUI para Android necesita una JDK (Java 11 como mínimo). JetBrains recomienda tener **JDK 11 o superior** [jetbrains.com](https://www.jetbrains.com/help/rider/MAUI.html#:~:text=3,Java%20JDK%2011%20or%20later). La comunidad suele utilizar el JDK de Eclipse Temurin. Para instalar **Java 17**:

```shell
brew install --cask temurin@17
```

Verifica la instalación ejecutando:

```shell
java -version 
javac -version
```

Configura la variable de entorno `JAVA_HOME` y actualiza el `PATH` para que otras herramientas (como Android Studio) detecten la JDK. Añade estas líneas a `~/.zshrc` y recarga la configuración:

Edita primero el archivo `.zshrc` desde el terminal (si no existe lo crea)

```shell
nano ~/.zshrc
```

Luego añade las siguientes líneas dentro del archivo:

```shell
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export PATH="$JAVA_HOME/bin:$PATH"
```

Guarda los cambios con `[^ Control] + o` y luego `[^ Control] + x` para salir

Luego reinicia Zsh

```
source ~/.zshrc
```

Comprueba que reconoce la ruta para JAVA_HOME en el terminal:

```shell
echo $JAVA_HOME
```


## 6. Instalación y configuración de Xcode

Para compilar y ejecutar aplicaciones .NET MAUI en iOS y macOS necesitas **Xcode**. La documentación de JetBrains indica que hay que instalar Xcode para gestionar recursos y firmar las aplicaciones [jetbrains.com](https://www.jetbrains.com/help/rider/MAUI.html#:~:text=To%20configure%20resources%20and%20targets%2C,com). Sigue estos pasos:

1. **Instalar Xcode** desde la App Store.
    
2. **Ejecutar Xcode** al menos una vez y acceder con tu **Apple ID** para habilitar las herramientas de desarrollo [jetbrains.com](https://www.jetbrains.com/help/rider/MAUI.html#:~:text=To%20configure%20resources%20and%20targets%2C,com).
    
3. Añade la ruta de Xcode manualmente desde el terminal y acepta la licencia mediante:
    
	```shell
	sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
	sudo xcodebuild -license accept
	```
	
> Verifica con `xcode-select -p`


> **Configurar Xcode en Rider**: al abrir JetBrains Rider, el IDE detectará automáticamente la versión de Xcode. Si tienes varias versiones, puedes elegir la que prefieras en **Preferences → Build, Execution, Deployment → Apple Platforms**. [jetbrains.com](https://www.jetbrains.com/help/rider/MAUI.html#:~:text=JetBrains%C2%A0Rider%20will%20detect%20Xcode%20automatically,Platforms%20page%20of%20JetBrains%C2%A0Rider%20settings).
    

## 7. Instalar Android Studio y configurar el SDK

El desarrollo para Android con MAUI requiere un SDK de Android y emuladores. La propia guía de JetBrains advierte que, para apuntar a plataformas móviles, hay que instalar Android SDKs con una JDK y Xcode [jetbrains.com](https://www.jetbrains.com/guide/dotnet/tutorials/maui-development/install-maui/#:~:text=Android%20and%20iOS%20SDKs). Sigue estos pasos:

1. **Instalar Android Studio**. Puedes descargarlo desde el sitio oficial o usar Homebrew:
    
```shell
brew install --cask android-studio
```
    
2. **Ejecutar Android Studio**. La primera vez, el **Setup Wizard** descargará los componentes base. Necesario aceptar las tres licencias. Si se abre la pantalla de bienvenida, elige **More Actions → SDK Manager** (o desde un proyecto: _File → Settings/Preferences → Languages & Frameworks → System Settings → Android SDK_).
    
3. **Configurar las plataformas**: en la pestaña **SDK Platforms** selecciona las versiones de Android que necesites (por ejemplo, Android 14 – API 34 y Android 15 – API 35). Pulsa **Apply** para descargar e instalar.
    
4. **Configurar las herramientas**: en la pestaña **SDK Tools** marca al menos:
    
    - **Android SDK Platform-Tools**
        
    - **Android SDK Build-Tools** (versión 34.x o superior)
        
    - **Android Emulator**
        
    - **Android SDK Command-line Tools (latest)**
        
    
    Aplica los cambios para instalar.
    
5. **Crear emuladores (AVD)**: En la pantalla de inicio de Android Studio,desde `More Actions -> Virtual Device Manager` puedes crear dispositivos virtuales seleccionando modelos de teléfono/tableta y la imagen del sistema deseada.
    
6. **Variables de entorno para Android**: muchos comandos de Android y JetBrains Rider buscan la ruta del SDK en la variable `ANDROID_HOME`. La documentación oficial indica que hay que definir `ANDROID_HOME` apuntando al directorio del SDK y añadir sus rutas a `PATH` [developer.android.com](https://developer.android.com/tools/variables#:~:text=setting%20environment%20variables,tools). Añade las siguientes líneas a tu `~/.zshrc` desde un Terminal:
    
```shell
export ANDROID_HOME="$HOME/Library/Android/sdk"
export PATH="$PATH:$ANDROID_HOME/platform-tools"
export PATH="$PATH:$ANDROID_HOME/emulator"
export PATH="$PATH:$ANDROID_HOME/cmdline-tools/latest/bin"
```
    
6. **Recargar la configuración** con `source ~/.zshrc` para que las variables tengan efecto.
    


## 8. Configurar JetBrains Rider para .NET MAUI y Android

1. **Seleccionar el SDK de .NET**: al iniciar Rider por primera vez, el IDE detecta automáticamente los SDKs instalados. Si no encuentra ninguno, ve a **Preferences → Build, Execution, Deployment → . NET** y especifica la ruta de `dotnet`.
    
2. **Instalar el plugin Android**: la guía de JetBrains indica que, para apuntar a Android, debes instalar el plugin **Rider Android Support** [jetbrains.com](https://www.jetbrains.com/guide/dotnet/tutorials/maui-development/install-maui/#:~:text=You%E2%80%99ll%20need%20to%20install%20another,you%20to%20restart%20the%20IDE). Si no lo instalaste en la primera ejecución de Rider:
    
    - Dirígete a **Preferences → Plugins**.
        
    - Busca “Rider Android Support” y pulsa **Install**. Reinicia Rider cuando te lo solicite.
        
3. **Configurar el SDK de Android y la JDK**:
    
    - Ve a **Preferences → Build, Execution, Deployment → Android**.
        
    - Indica la **ubicación del SDK de Android** (`ANDROID_HOME`) y la **ubicación de la JDK** que instalaste (Java 17). El botón **Install new** permite gestionar diferentes versiones del SDK [jetbrains.com](https://www.jetbrains.com/guide/dotnet/tutorials/maui-development/install-maui/#:~:text=With%20the%20IDE%20restarted%2C%20return,Java%20Development%20Kit%20Location%20values).
      
      > Para añadir la ruta de "Java Development Kit Location", en la terminal copia el resultado de la siguiente instrucción y pégala en el textbox correspondiente:
      > 	`echo $JAVA_HOME`
      > 	
      > Devuelve algo como: `/Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home`
        
4. **Verificar Xcode**: en **Preferences → Build, Execution, Deployment → Apple Platforms**, comprueba que Rider detecta la versión de Xcode instalada [jetbrains.com](https://www.jetbrains.com/help/rider/MAUI.html#:~:text=JetBrains%C2%A0Rider%20will%20detect%20Xcode%20automatically,Platforms%20page%20of%20JetBrains%C2%A0Rider%20settings). Si tienes varias versiones, selecciona la adecuada.
    
5. **Crear o abrir proyectos**: utiliza **File → New Solution…** y selecciona **.NET MAUI App** para crear un proyecto multiplataforma. Rider generará configuraciones de ejecución para iOS, Android y Mac automáticamente [jetbrains.com](https://www.jetbrains.com/help/rider/MAUI.html#:~:text=JetBrains%C2%A0Rider%20includes%20a%20configurable%20project,template%20for%20new%20MAUI%20projects).
    
6. **Ejecutar y depurar**: en la barra de ejecución puedes elegir la configuración (Android, iOS, Mac) y lanzar la aplicación en un emulador o dispositivo real. Para iOS, asegúrate de tener un simulador instalado desde Xcode; para Android, usa el AVD Manager.
    

## 9. Configurar el archivo `~/.zshrc`

Ejemplo completo:

```
### — Básico: Consola C# / Web API (.NET) — ###
# Si usaste Homebrew o el instalador oficial de Microsoft, dotnet ya estará en PATH, sin necesitar más.

### — Desarrollo .NET MAUI para Android — ###
# Java (JDK 17). Recomendado por compatibilidad con Android.
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export PATH="$JAVA_HOME/bin:$PATH"

# Android SDK. Rutas actualizadas según documentación oficial.
export ANDROID_HOME="$HOME/Library/Android/sdk"
export PATH="$PATH:$ANDROID_HOME/platform-tools"
export PATH="$PATH:$ANDROID_HOME/emulator"
export PATH="$PATH:$ANDROID_HOME/cmdline-tools/latest/bin"
```

Recuerda ejecutar `source ~/.zshrc` para recargar la configuración después de hacer cambios.

## 10. Visual Studio Code (opcional)

Aunque JetBrains Rider será el IDE principal, también puedes usar **Visual Studio Code** como editor ligero. Se recomienda instalar las extensiones oficiales de Microsoft:

1. **C#** (csharp) – proporciona análisis y depuración.
    
2. **C# Dev Kit** – añade plantillas de proyectos y mejor integración.
    

Busca las extensiones en la pestaña **Extensions** (`⌘⇧X`), instálalas y reinicia VS Code.

---
[↩️ Volver](README.md)