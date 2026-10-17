# Políticas de reinicio

## Para esta parte de la práctica
1. Usa el archivo Dockerfile que se encuentra en cada carpeta con el nombre de la política de reinicio para crear una imagen. Analiza las instrucciones de cada Dockerfile 
2. Usa la imagen para ejecutar un contenedor agregando la política respectiva y observar lo que sucede verificando el cumplimiento de la política de reinicio

### no
No reinicia el contenedor bajo ninguna razón. Esta es la política por default
```
docker run -d --name prueba-no reinicio-no:1.0
```

### always
Reinicia siempre el contenedor si se detiene. Si se detiene manualmente, sólo se reiniciará cuando se reinicie el demonio Docker o cuando se reinicie manualmente el propio contenedor
```
docker run -d --name prueba-always --restart always reinicio-always:1.0
```

### unless-stopped

Similar a always, excepto que cuando el contenedor se detiene (manualmente o de otra manera), no se reinicia incluso después de reiniciar el demonio Docker.
```
docker run -d --name prueba-unless-stopped --restart unless-stopped reinicio-unless-stopped:1.0
``

### on-failure
Se reinicia únicamente cuando hay una falla en la ejecución del código que se manifiesta con un código diferente de 0. No se reinicia si el contenedor se detiene manualmente.

```
docker run -d --name prueba-on-failure --restart on-failure:3 reinicio-on-failure:1.0

### Análisis del Dockerfile
#### 
- FROM centos:8: establece la imagen base.
- COPY inicio.sh /inicio.sh: copia el script al contenedor.
- RUN chmod +x /inicio.sh: otorga permisos de ejecución al script.
- CMD ["/inicio.sh"]: ejecuta el script al iniciar el contenedor.

### Observaciones de las pruebas
- no: el contenedor finalizó con código 1 y no se reinició. Resultado: 1 0 exited.
- always: el contenedor se reinició automáticamente cada vez que terminó el script.
- unless-stopped: el contenedor se reinició automáticamente, pero permaneció detenido después de ejecutar docker stop.
- on-failure: el contenedor realizó tres reinicios automáticos y después quedó detenido. Resultado: 1 3 exited.
Finalmente se comprobó que cada política controla de manera diferente el reinicio de los contenedores Docker, permitiendo decidir si se reinician automáticamente, si respetan la detención manual o si limitan los intentos ante un error.

