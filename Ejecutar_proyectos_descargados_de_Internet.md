
Al descargar un zip desde **Google Drive** a un segundo Mac, es bastante probable que macOS le aplique automáticamente el atributo extendido `com.apple.quarantine` a los archivos comprimidos, o al descomprimirlos. Este atributo le indica al sistema que los archivos vinieron de Internet y activa verificaciones de seguridad (_Gatekeeper_) al intentar abrir binarios o apps incluidas.  

---

### Por qué pasa

- Google Drive se considera una fuente “externa”. Cuando descargas algo así, macOS marca los archivos con `quarantine`.
    
- Al descomprimir un zip que tiene ese atributo, ese marcado puede propagarse a los archivos dentro.  
    
- En el Mac donde creaste el proyecto, los archivos se generaron localmente o nunca pasaron por un mecanismo que los marque como descargados desde internet, por eso no tienen ese atributo y no generan errores.
    

---

### Cómo quitar ese atributo (“des-cuarentena”) en el Mac destino

1. Abre Terminal.
    
2. Ve a la carpeta del proyecto descomprimido, por ejemplo si está en Descargas:
    
    ```bash
    cd /ruta/al/proyecto
    ```
    
3. Ejecuta este comando para eliminar recursivamente el atributo de cuarentena de todos los archivos:
    
    ```bash
    xattr -r -d com.apple.quarantine .
    ```
    
    - `-r`: recursivo
        
    - `-d`: borrar el atributo especificado
        
    - `.` indica “este directorio completo”
        
4. Si algún archivo da “permiso denegado”, usa `sudo` al inicio:
    
    ```bash
    sudo xattr -r -d com.apple.quarantine /ruta/al/proyecto
    ```
    
1. Luego abre de nuevo el proyecto desde Rider y ejecuta. Safari (o _Gatekeeper_) debería dejar de bloquear los archivos.
    

> Cuando pases un proyecto por zip o Drive, **en el Mac destino justo después de descomprimir**, haz el `xattr -r -d com.apple.quarantine .` antes de intentar abrir binarios o apps.
    
---
[↩️ Volver](README.md)