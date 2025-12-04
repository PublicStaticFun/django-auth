![Imagen del repositorio](https://github.com/PublicStaticFun/django-auth/blob/main/Portada7.png?raw=true)

# Auth en Django

## Objetivo
El repositorio explica cómo crear una API en Django REST Framework con autenticación en mediante tokens, enfocada en tres funciones principales:
* Registro de usuario
* Login / Autenticación
* Acceso a rutas protegidas

El fin es que aplicaciones frontend (React, Vue, etc.), puedan autenticarse contra la API mediante un token.

## Concepto de Token
Un **token** es una cadena única (letras y números) generada después de validar usuario y contraseña. Este token sirve como "pase de acceso" para rutas protegidas.

Se envía siempre mediante el **header Authorization**, si no se envía o es incorrecto, la API responde con no autorizado.

## Flujo general de autenticación con tokens
a) El frontend envía usuario y contraseña: 

La API valida esos datos contra la base de datos.

b) Si son correctos:

Se genera un token único para ese usuario, y el backend se lo devuelve al frontend en formato JSON.

c) El frontend guarda ese token:

Lo usa para solicitar rutas protegidas.

d) Para acceder a rutas protegidas:

El frontend debe enviar el token en el header, y la API verifica el token antes de entregar información.

## Creación de rutas básicas
Tres endpoints principales:
* /register: crear usuario y devolver token
* /login: validar usuario existente y devolver token
* /profile: ruta protegida que exige token

## Registro de usuarios
El proceso es:
1. Recibir email, username y password
2. Validar que los datos sean correctos usando un serializer.
3. Guarda el usuario en la base de datos
4. Generar token para ese usuario
5. Devolver:
   * El token
   * La información del usuario
   * Un estado 201 Created

Si los datos no son válidos:
* Se devuelven errores y código 400 BAD REQUEST

## Login de usuarios
El proceso es:
1. Recibir username y password
2. Verificar que el usuario exista (404)
3. Comparar la contraseña ingresada con la almacenada
4. Si es correcta, devolver:
   * Token (existente o recién creado)
   * Datos del usuario
   * Estado 200 OK

Errores posibles:
* Usuario no encontrado
* Contraseña invalida

## Rutas protegidas (profile)
1. Para acceder a /profile, el usuario debe enviar su token en el header
2. Si el token no está o es incorrecto:
   * La API responde credenciales no provistas o token inválido
3. Se utiliza clases de autenticación y permisos para obligar el uso del token.

Una vez validado el token, la API puede:
* Obtener el usuario autenticado desde request.user
* Devolver cualquier dato del perfil

## Utilidad del sistema
Este mecanismo es útil para:
* Aplicaciones frontend que usan APIs
* Evitar que cualquiera acceda a datos sensibles.
* Mantener sesiones sin cookies o autenticación basada en sesiones tradicionales.
