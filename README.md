# Second-hand market API

Práctica Node.js para Keepcoding Web II Bootcamp.

Endpoints de la API :
* register
* auth
* tags
* advertisements

## Ejecución por defecto
Para iniciar el servidor, ejecutar:
`npm start`.

## Ejecución usando cluster
Nota: No usa cluster si la variable ${NODE_ENV} es distinta de 'production'

Con el entorno en modo **production**, usar el paquete compilado (Generado en ./dist). 

Es necesario instalar el package **pm2** y ejecutar `pm2 start index.js -i 0`.

Esto iniciará el servidor en modo cluster, usando las CPU's disponibles.


## Mongo Shell script
La versión MongoDB debe ser >= 3.4.2

Ir al directorio `scripts/database/mongodb/` y ejecutar el comando `mongo install_bd.js`. 

## Mensajes de Error
Los mensajes de error están definidos en el documento `assets/docs/errorMessages.json`, cuya estructura permite seleccionar la traducción de los mismos a un lenguaje determinado.

Se puede definir el lenguaje de mensajes de error a nivel base de datos en `user` o en la query.
Si ningunp de estos está definido, el sistema usa `en` por defecto.


## Ejemplos de Queries usando CURL

Query:

+ lang: error message language priority
+ token: for `adverts` request

### register
###### Registrar un nuevo usuario
```curl
curl -XPOST http://localhost:3000/apiv1/register?lang=es -H 'Content-Type: application/json' -d '{ "name": "dimpiax", "email": "dimpiax@gmail.com", "passwd": "root", "lang": "UA" }'
```

### auth
###### Recuperar el token de autorización de un usuario
Returns `AUTH_TOKEN`
```curl
curl -XPOST http://localhost:3000/apiv1/auth -H 'Content-Type: application/json' -d '{ "email": "dimpiax@gmail.com", "passwd": "root" }'
```

### tags
###### Recuperar el listado de tags existentes
```curl
curl -XGET http://localhost:3000/apiv1/adverts/tags?lang=es\&token=AUTH_TOKEN
```

### advertisements
###### Recuperar anuncios
Esta query permite varios parámetros.
Se incluyen en la respuesta los documentos que satisfagan las siguientes condiciones para cada propiedad:

+ name *(*string*)*: comienzan por la cadena facilitada
+ price *(*string | double*)*: se encuentran en el rango definido para la propiedad `price` 
+ toSell *(*boolean*)*: tipo de anuncio
+ tag *(*string*)*: el valor está incluido en la variable tipo array `tags` 
+ limit *(*number*)*: limita el número de documentos en la respuesta

+ start *(*number*)*: muestra la página de los registros recuperados relativa al límite especificado en limit. 
  Por ejemplo: si start es **5** y limit es **10**, se mostrarán los documentos a partir del índice *50* 

Los documentos pueden ser ordenados en orden ascendente especificando una propiedad:

+ sort *(*string*)*: propiedad a usar para la ordenación.

#### Ejemplos:

+ paging, sorting, price interval, tag
```curl
curl -XGET http://localhost:3000/apiv1/adverts?token=AUTH_TOKEN\&start=0\&limit=2\&sort=toSell\&price=10-230.15\&tag=motor
```
+ advertisement type
```curl
curl -XGET http://localhost:3000/apiv1/adverts?token=AUTH_TOKEN\&toSell=false
```

### Entorno de desarrollo

Herramientas usadas: Babel, Flow, ESLint.
