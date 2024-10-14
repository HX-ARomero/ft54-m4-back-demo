# Nest JS - Nest JS & TypeORM

[Volver a Inicio](../README.md)

## Links Documentación

<a href="https://nestjs.com/" target=_blank>
<img style="height: 50px; background-color: lightgrey; margin: 30px; padding: 15px; border-radius: 15px" src="https://cms.rootstack.com/sites/default/files/inline-images/nest.png" alt="Nest JS">
</a>
<a href="https://typeorm.io/" target=_blank>
<img style="height: 50px; background-color: lightgrey; margin: 30px; padding: 15px; border-radius: 15px" src="https://miro.medium.com/v2/resize:fit:739/1*rTbyH3zL7Ue8VyTHRMRDAA.png" alt="TypeORM">
</a>

## TypeORM

### Instalación

```bash
npm install @nestjs/typeorm @nestjs/config typeorm pg
```

## Archivo de configuración

- Ejemplo de archivo de configuración:

```ts
import { registerAs } from "@nestjs/config";
import { config as dotenvConfig } from "dotenv";
import { DataSource, DataSourceOptions } from "typeorm";
dotenvConfig({ path: ".development.env" });

const config = {
  type: "postgres",
  database: process.env.DB_NAME,
  host: process.env.DB_HOST,
  port: process.env.DB_PORT as unknown as number,
  username: process.env.DB_USERNAME,
  password: process.env.DB_PASSWORD,
  entities: ["dist/**/*.entity{.ts,.js}"],
  migrations: ["dist/migrations/*{.ts,.js}"],
  autoLoadEntities: true,
  logging: true,
  synchronize: true,
  dropSchema: true,
};

export const typeOrmConfig = registerAs("typeorm", () => config);
export const connectionSource = new DataSource(config as DataSourceOptions);
```

## Migraciones

Una migración en el contexto de bases de datos es un mecanismo que permite gestionar los cambios estructurales de la base de datos de manera controlada y versionada. Una migración guarda instrucciones específicas que modifican el esquema de la base de datos, como la creación, modificación o eliminación de tablas, columnas, índices, relaciones, etc.

### Cuándo se utilizan las migraciones:

1. Cambios en el esquema de la base de datos:
   - Cuando necesitas crear nuevas tablas o modificar las existentes (añadir/eliminar columnas, cambiar tipos de datos).
   - Cambios en las relaciones entre tablas (foreign keys).

2. Sincronización entre entornos:
  - Para mantener el mismo esquema de base de datos entre distintos entornos (desarrollo, pruebas, producción) de manera controlada.

3. Versionado del esquema:
   - Las migraciones permiten llevar un registro de todos los cambios realizados en la base de datos a lo largo del tiempo. Esto ayuda a identificar qué cambios se hicieron en cada versión.

4. Reversión de cambios:
   - Las migraciones permiten deshacer (revertir) cambios en caso de que haya un error o necesites volver a una versión anterior del esquema.

### Beneficios:

- Control de cambios: Puedes realizar modificaciones en el esquema de la base de datos de manera ordenada y reversible.
- Colaboración: Facilita el trabajo en equipo, ya que todos los desarrolladores pueden mantener el mismo esquema de base de datos.
- Despliegue seguro: Permite realizar cambios en producción de manera segura, minimizando errores.
  En resumen, las migraciones se utilizan para gestionar de forma controlada los cambios en la estructura de la base de datos a lo largo del ciclo de vida de una aplicación.

### Scripts

- En ARCHIVO "package.json":

```json
"scripts": {
	// ----- ----- ----- -----
	"typeorm": "ts-node ./node_modules/typeorm/cli",
	"migration:run": "npm run typeorm migration:run -- -d ./src/config/typeorm.ts",
	"migration:generate": "npm run typeorm -- -d ./src/config/typeorm.ts migration:generate",
	"migration:create": "npm run typeorm migration:create",
	"migration:revert": "npm run typeorm -- -d ./src/config/typeorm.ts migration:revert",
	"migration:show": "npm run typeorm -- -d ./src/config/typeorm.ts migration:show"
},
```

- "typeorm": Permite acceder al CLI de TypeORM}
- Debemos indicar la ubicación del archivo de configuración:
  - "tipeorm.ts" en nuestro caso.
- Scripts
  - "migration:run": Ejecutar una migración
  - migration:generate: Genera automáticamente una migración basada en los cambios detectados en las entidades.
  - "migration:create": Crea una migración vacía para que la escribas manualmente.
  - "migration:revert": Revertir migración
  - "migration:show": Mostrar migración

### Ejemplo de Migración

```ts
import { MigrationInterface, QueryRunner } from "typeorm";

export class NameRefactor1712799620352 implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.query(
      'ALTER TABLE "USERS" RENAME COLUMN "name" TO "fullname"'
    );
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.query(
      'ALTER TABLE "USERS" RENAME COLUMN "fullname" TO "name"'
    );
  }
}
```
