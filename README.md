
TP3 Sistemas Operativos 2 
## Ingeniería en Computación - FCEFyN - UNC 2021
---

</br>
<p align="center">


  <p align="center">
    Sistemas Embebidos
    <br>
    <a href="https://reponame/issues/new?template=bug.md">Report bug</a>
    ·
    <a href="https://reponame/issues/new?template=feature.md&labels=feature">Request feature</a>
  </p>
</p>

</br>

---


## Tabla de contenidos

<!-- 
- [## Ingeniería en Computación - FCEFyN - UNC 2021](#-ingeniería-en-computación---fcefyn---unc-2021) -->
- [Tabla de contenidos](#tabla-de-contenidos)
- [Introduccion](#introduccion)
- [Objetivo](#objetivo)
- [Desarrollo](#desarrollo)
- [SystemD](#systemD)
- [Servicios](#servicios)
- [Nginx](#Nginx)
- [Set up](#set-up)
- [Referencias](#referencias)

## Introduccion

Los sistemas embebidos suelen ser accedidos de manera remota. Existen distintas tenicas para hacerlo, una forma muy utilizada suelen ser las RESTful APIs. Estas, brindan una interfaz definida y robusta para la comunicación y manipulación del sistema embebido de manera remota. Definidas para un esquema Cliente-Servidor se utilizan en todas las verticales de la industria tecnológica, desde aplicaciones de IoT hasta juegos multijugador.


## Objetivo

El objetivo del presente trabajo práctico es que el estudiante tenga un visión end to end de una implementación básica de una RESTful API sobre un sistema embedido. El estudiante deberá implementarlo interactuando con todas las capas del procesos. Desde el testing funcional (alto nivel) hasta el código en C del servicio (bajo nivel).


## Desarrollo

En el siguiente trabajo se implementaron dos servicios:
</br>
</br>
**a-** Servicio de usuario
</br>
**b-** Servicio de descarga
</br>
</br>
Donde cada servicio expone una REST API. Como framework se implementó ulfius. A su vez, el servicio tiene un ngnix por delante para direccionar el request al servicio correspondiente.

El web server retorna 404 Not Found para cualquier path no existente.


## System D

Los servicios estan configurados con SystemD para soportar los comandos restart, reload, stop y start.
Se inicializan de manera automática cuando el sistema operativo bootee.
```
├── rsc
     ├── SoTp3Downloads.service
     └── SoTp3Users.service
```
Se utilizaron archivos de configuracion que pueden enontrarse en la carpeta */rsc*, al utilizar ```make install``` Se guardarán automaticamente en ```/etc/systemd/system/```


## Servicios


A continuacion se presentará una ejemplificacion de utilizacion de los servicios

## 1. Servicio de usuarios

```
http://192.168.100.6/SoTp3Users/api/users 
```
</br>

### POST /api/users

</br>
Request 

Se crea un usuario llamado *user1* con contraseña *user1*
``` 
curl --request POST --url http://192.168.100.6/SoTp3Users/api/users -u USER:SECRET --header 'accept: application/json' --header 'content-type: application/json' --data '{"username":"user1", "password": "user1"}'
```

Respuesta

![img1](/img/user_service.png)

En log

![logu](/img/log_user.png)


---
## 2. Servicio de descarga


### POST /api/servers/get_goes
</br>

Ante el request de un usuario, si el archivo solicitado ya ha sido descargado con anterioridad, se le avisa que ya existe y se envia el link. De otra manera se debe avisar que no está y descargarlo.



Request

```
curl --request POST --url http://192.168.100.6/SoTp3Downloads/api/servers/get_goes  -u USER:SECRET --header 'accept: application/json' --header 'content-type: application/json' --data '{"product": "ABI-L2-RRQPEF", "datetime": "2020-12-30"}'
```

Respuesta

En este caso se encontrará con dos tipos de mensajes, uno cuando el archivo este presente en el servidor, en este caso se le otorgara al usuario el archivo de descarga:

//IMAGEEEEM
 otro que le indique al usuario que esta descargando.
//IMAGEEN

### GET 

```
http://192.168.100.6//SoTp3Downloads/api/servers/get_goes 
```

El usuario puede ingresar y ver los archivos que se encuentran en el servidor:

![Frames](/img/dwn_file.png)

Tambien puede hacerlo mediante 
``` 
curl --request GET --url http://192.168.100.6/SoTp3Downloads/api/servers/get_goes -u USER:SECRET --header 'accept: application/json' --header 'content-type: application/json'
```

Log de descarga

![logdwn](/img/log_dwn.png)

## Nginx

Se configuro ngnix para poder direccionar el request al servicio correspondiente.
Archivo de configuracion utilizado

![ngnix](/img/ngnix.png)

## Set up 

Primero crear las carpetas  *bin* con ```mkdir bin``` 

Este programa se compila usando```make all``` dentro del repositorio.

Para limpiar hacer ```make clean``` :


## Referencias

- [Systrem D ](https://systemd.io/)
- [System D en Freedesktop](https://www.freedesktop.org/wiki/Software/systemd/)
- [nginx](https://docs.nginx.com/)
- [Ulfius HTTP Framework](https://github.com/babelouest/ulfius)
- [Kore Web PLataform](https://kore.io/)

[sysD]: https://www.freedesktop.org/wiki/Software/systemd/
[ngnx]: https://docs.nginx.com/
[ulfi]: https://github.com/babelouest/ulfius