# examen-1-AV
Examen
### 1. Explica métodos para 'abrir' unha consola/shell a un contenedor en execución.
Pues usar un "docker exec" 
y después docker exec -it (el nombre del contenedor que le puse

Esto abre una sesión interactivo con el contenedor.

Aunque también se podría hacer con docker attach (nombre_contenedor)
### 2. No contenedor anterior (en execución), qué opciones tes que ter usado ó arrincalo para poder interactuar coas súas entradas e salidas

Debemos usar el -i ( permite mantener el flujo de entrada abierto).
-t : asocia la terminal a la sesión.
en conclusión usamos docker run -it (la imagen) .
### 3. Cómo sería un ficheiro docker-compose para que dous contenedores se comuniquen entre si nunha rede só deles?
Abririamos un yml primero y ahí pondríamos: 
```
version: '3.8'
services:
  contenedor1:
     image: alpine 
     command: tail -f /dev/null
     networks:
       red_privada
        aliases:
        - nombre_contenedor1
  contendor2:
    image:alpine
    command: tail -f /dev/null
    networks:
      red_privada
         nombres:
         - nombre_contenedor2
    networks:
       red_privada:
          driver:bridge
```

### 4. Qué tes que engadir ó ficheiro anterior para que un contenedor teña unha IP fixa?
Para asignarle una ip fija al contenedor modifcamos la config de la red en el archivo que tengamos en  el docker-compose y añadimos:
```
networks: 
  red_privada
  driver: bridge
  ipam:
    config:
    - subnet:192.168.1.0/24
```
Después de esto le asignamos una ip fija al contenedor:

```
contenedor1:
  networks:
    red_privada:
    ipv4_adress: 192.168.1.100
 ```    
### 5. Qué comando de consola podo usar para sabe-las ips dos contenedores anteriores?
Pues para saber las ips de los contenedores anteriores vamos usar:
" docker inspect -f " 



### 6. Cál é a funcionalidade do apartado "ports" en docker compose?

Se usa principialmente para  mapear puertos del contenedor a puertos del host.
Por ejemplo si sale un puerto 8080:80 signfica que se redirige al puerto 80 del contenedor.


### 7. Para qué serve o rexistro CNAME? Pon un exemplo

Sirve para redirigir un dominio hacia otro.
Ejemplo: 
blog.example.com > www.example.com

### 8. Cómo podo facer para que a configuración dun contenedor DNS non se borre se creo outro contenedor?

Para evitar perder la configuración del DNS una solución podría ser usar volúmenes persistentes.

### 9. Engade unha zoa tendaelectronica.int no teu docker DNS que teña:
 - www á IP 172.16.0.1
 - owncloud sexa un CNAME de www
 - un rexistro de texto có contido "1234ASDF"
 Comproba que todo funciona có comando "dig"
 Mostra nos logs que o servicio funciona ben usando a saída da terminal ó levantar o compose ou có comando "docker logs [nomeContenedorOuID]"
(o apartado 9 realízase na máquina virtual)


```
$TTL 3600
@   IN  SOA ns.tendaelectronica.int. admin.tendaelectronica.int. (
    2024111501 ; Serial
    3600       ; Refresh
    1800       ; Retry
    1209600    ; Expire
    3600 )     ; Minimum TTL
@   IN  NS  ns.tendaelectronica.int.
www IN  A   172.16.0.1
owncloud IN CNAME www
@   IN  TXT "1234ASDF"

```
#FINAL


