# Nest JS - Nest JS Routing

[Volver a Inicio](../README.md)

## 🎯Obtener información desde el Request

En NestJS, obtener parámetros de ruta, query parameters y el body de una solicitud es sencillo gracias a los decoradores que permiten acceder a estos datos dentro de los métodos de un controlador.

### 1. Obtener Parámetros de Ruta (Params):

Para acceder a los parámetros que forman parte de la URL, como en /users/:id, se usa el decorador @Param().

Ejemplo:

```ts
@Get(':id')
getUserById(@Param('id') id: string) {
  return `User with ID: ${id}`;
}
```

El valor de :id será capturado y accesible mediante el decorador @Param('id').

### 2. Obtener Query Parameters:

Los query parameters son valores que se pasan en la URL después del signo de interrogación ?, como en /users?age=25. Para acceder a ellos, se usa el decorador @Query().

Ejemplo:

```ts
@Get()
getUsers(@Query('age') age: string) {
return `Users with age: ${age}`;
}
```

Si la URL es /users?age=25, el valor de age será accesible mediante @Query('age').
También se pueden obtener todos los query parameters de una sola vez:

```ts
@Get()
getUsers(@Query() query: any) {
  return `Query params: ${JSON.stringify(query)}`;
}
```

Esto capturará todos los query parameters en un objeto.

### 3. Obtener el Body de la Solicitud:

Para acceder al cuerpo de una solicitud en métodos POST, PUT o PATCH, se usa el decorador @Body().

Ejemplo:

```ts
@Post()
createUser(@Body() createUserDto: CreateUserDto) {
  return `User created with data: ${JSON.stringify(createUserDto)}`;
}
```

El decorador @Body() permite acceder al cuerpo de la solicitud. Se suele utilizar junto con DTOs (Data Transfer Objects) para definir y validar la estructura de los datos esperados.
Si solo quieres obtener una propiedad específica del cuerpo:

```ts
@Post()
createUser(@Body('name') name: string) {
  return `User created with name: ${name}`;
}
```

### 4. Acceso combinado:

Es común acceder a múltiples fuentes de datos en una misma ruta. NestJS permite combinar decoradores como @Param(), @Query() y @Body() en un mismo método.

Ejemplo:

```ts
@Post(':id')
updateUser(
  @Param('id') id: string,
  @Query('status') status: string,
  @Body() updateUserDto: UpdateUserDto
) {
  return `User with ID: ${id}, status: ${status}, updated with data: ${JSON.stringify(updateUserDto)}`;
}
```

Resumen rápido:
@Param(): Obtiene los parámetros de la URL (ruta).
@Query(): Accede a los parámetros de consulta (query parameters).
@Body(): Captura el cuerpo de la solicitud.
Se pueden combinar estos decoradores en un mismo método para manejar diferentes partes de la solicitud.
NestJS facilita el manejo de estas entradas de la solicitud de manera sencilla y tipada.

## 🎯Guardianes (Guards)

Propósito:

- Los Guardianes en NestJS se utilizan para controlar el acceso a las rutas, es decir, para implementar lógica de autorización. Los guardianes determinan si una solicitud debe ser manejada por la ruta correspondiente o no.

Cómo funcionan:

- Se ejecutan antes de que el controlador maneje la solicitud.
  Retornan un booleano (true para permitir el acceso, false para denegarlo) o una Promesa que resuelve a un booleano.

## 🎯Interceptores

Propósito:

- Los Interceptores en NestJS permiten modificar o extender el comportamiento de las solicitudes y respuestas. Se pueden utilizar para una variedad de tareas como el manejo de logging, transformación de datos, y manipulación de errores.

Cómo funcionan:

- Se ejecutan antes y después del método del controlador.
  Pueden modificar los datos de la solicitud y la respuesta.
  Se pueden usar para envolver la lógica del controlador y agregar lógica adicional.

## Diferencias clave entre Guardianes e Interceptores

Propósito:

- Guardianes: Se utilizan para control de acceso y lógica de autorización.
- Interceptores: Se utilizan para manipular y transformar datos antes de que se envíen al cliente o después de que se reciban del cliente.

Momento de ejecución:

- Guardianes: Se ejecutan antes de que el controlador maneje la solicitud.
- Interceptores: Se ejecutan tanto antes como después de la ejecución del método del controlador.

Modificación de datos:

- Guardianes: No modifican los datos de la solicitud o la respuesta.
- Interceptores: Pueden modificar los datos de la solicitud y la respuesta.

Resultado:

- Guardianes: Retornan un booleano que determina si se permite el acceso.
- Interceptores: Retornan un Observable que permite transformar los datos antes de enviarlos al controlador o al cliente.
