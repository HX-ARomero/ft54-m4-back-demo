# Nest JS - Nest JS Authentication I

[Volver a Inicio](../README.md)

## Links

- [bcript - Documentación](https://bcrypt.online/)
- [JWT - JSON Web Token - Documentación](https://jwt.io/)

## 🎯Bcrypt

### Comando de Instalación

```bash
npm install bcrypt
```

### Cost Factor en bcrypt

Es un parámetro que determina cuántas veces se aplica el algoritmo de cifrado para una contraseña, lo que afecta tanto a la seguridad como al tiempo de procesamiento necesario para cifrar y verificar la contraseña. El Cost Factor también se conoce como **work factor** o **log rounds**.

- Cuanto mas alto, mayor seguridad y costo de procesamiento.
- 10 es un valor equilibrado entre seguridad y costo de procesamiento.

## 🎯JWT - JSON Web Token

### Comando de Instalación

- Instalar tipos de JWT:

```bash
npm install --save @nestjs/jwt
```
