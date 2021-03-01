El json-parser de express que utilizamos anteriormente es el llamado middleware.

Los middleware son funciones que se pueden utilizar para manejar objetos de request y response.

El json-parser que usamos anteriormente toma los datos sin procesar de las solicitudes que están almacenadas en el objeto request, los parsea en un objeto de JavaScript y lo asigna al objeto request como una nueva propiedad de body.

En la práctica, puede utilizar varios middleware al mismo tiempo. Cuando tiene más de uno, se ejecutan uno por uno en el orden en que se utilizaron en express.

Implementemos nuestro propio middleware que imprime información sobre cada solicitud que se envía al servidor.

Middleware es una función que recibe tres parámetros:

const requestLogger = (request, response, next) => {
console.log('Method:', request.method)
console.log('Path: ', request.path)
console.log('Body: ', request.body)
console.log('---')
next()
}

Al final del cuerpo de la función, se llama a la función next que se pasó como parámetro. La función next cede el control al siguiente middleware.

El middleware se utiliza así:
app.use(requestLogger)

Las funciones de middleware se llaman en el orden en que se utilizan con el método use del objeto del servidor express. Tenga en cuenta que json-parser se utiliza antes del middleware requestLogger, porque de lo contrario request.body no se inicializará cuando se ejecute el registrador.

Las funciones de middleware deben utilizarse antes de las rutas si queremos que se ejecuten antes de llamar a los controladores de eventos de ruta. También hay situaciones en las que queremos definir funciones de middleware después de las rutas. En la práctica, esto significa que estamos definiendo funciones de middleware que solo se llaman si ninguna ruta maneja la solicitud HTTP.

Agreguemos el siguiente middleware después de nuestras rutas, que se usa para capturar solicitudes realizadas a rutas inexistentes. Para estas solicitudes, el middleware devolverá un mensaje de error en formato JSON.

const unknownEndpoint = (request, response) => {
response.status(404).send({ error: 'unknown endpoint' })
}

app.use(unknownEndpoint)

conta Heroku: ph^*'8f844*jDvB
