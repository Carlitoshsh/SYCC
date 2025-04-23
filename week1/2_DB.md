
## Base de Datos y NodeJs

### Configuración de SQL Server

Para interactuar con SQL Server desde nuestra aplicación Node.js, necesitaremos lo siguiente:

1.  **Instalación de SQL Server:** Si aún no lo tienes instalado, deberás descargar e instalar SQL Server Developer Edition (que es gratuita para desarrollo) o cualquier otra edición que prefieras. Puedes encontrar las instrucciones de instalación en el sitio web de Microsoft.

2.  **SQL Server Management Studio (SSMS):** Esta herramienta proporciona una interfaz gráfica para administrar tus bases de datos SQL Server. También es recomendable instalarla.

3.  **Creación de una Base de Datos:** Utiliza SSMS para conectarte a tu instancia de SQL Server y crear una nueva base de datos que utilizará tu aplicación web.

4.  **Configuración de Autenticación:** Asegúrate de tener configurado un método de autenticación (por ejemplo, autenticación de Windows o autenticación de SQL Server con un usuario y contraseña) para acceder a tu base de datos. Anota las credenciales que necesitarás en tu aplicación.

## Conexión de Node.js con SQL Server (Usando .env)

Esta sección muestra cómo establecer una conexión a una base de datos SQL Server desde una aplicación Node.js, utilizando el paquete `tedious` ([recomendado por Microsoft](https://learn.microsoft.com/es-mx/sql/connect/node-js/node-js-driver-for-sql-server?view=sql-server-ver16)) y almacenando la información sensible en un archivo `.env`.

#### **1. Instala las Dependencias:**

Primero, asegúrate de tener instalado el paquete `tedious` y el paquete `dotenv` para cargar las variables de entorno desde el archivo `.env`:

```bash
npm install tedious dotenv
```

#### **2. Crea el Archivo `.env`:**

En la raíz de tu proyecto, crea un archivo llamado `.env` y añade lo siguiente:

```
DB_SERVER=TU_SERVIDOR\INSTANCIA
DB_DATABASE=NOMBRE_DE_TU_BASE_DE_DATOS
DB_USER=TU_USUARIO_SQL
DB_PASSWORD=TU_CONTRASEÑA_SQL
DB_ENCRYPT=true
DB_TRUST_SERVER_CERTIFICATE=true
```

Crea una copia del archivo `.env` y renómbrala a `.env.example`. Este archivo .`env.example` se incluye en el repositorio para guiar a otros desarrolladores.

**En el archivo `.env`** añade las siguientes variables de entorno con tus credenciales reales. Para la seguridad del proyecto, debe añadirse este archivo al `.gitignore`.

Reemplaza los siguientes valores con tu configuración:

- `TU_SERVIDOR\INSTANCIA`: El nombre de tu servidor SQL Server y la instancia (si aplica). Si es una instancia por defecto, usualmente solo necesitas el nombre del servidor o la dirección IP.
- `NOMBRE_DE_TU_BASE_DE_DATOS`: El nombre de la base de datos a la que quieres conectarte.
- `TU_USUARIO_SQL`: El nombre de usuario para autenticarte en SQL Server.
- `TU_CONTRASEÑA_SQL`: La contraseña para el usuario de SQL Server.
- `DB_ENCRYPT`: Establece a true si tu servidor SQL Server requiere conexiones encriptadas, de lo contrario false.
- `DB_TRUST_SERVER_CERTIFICATE`: En entornos de desarrollo donde los certificados pueden no ser válidos, puedes establecerlo a true. _Advertencia: No se recomienda para entornos de producción._.

#### **3. Configura la Conexión en tu Archivo JavaScript**

Crea un archivo `db.js` para agregar el siguiente código:

```javascript
// db.js
import 'dotenv/config'; // Carga las variables de entorno desde .env

import tedious from 'tedious';

const config = {
  server: process.env.DB_SERVER,
  database: process.env.DB_DATABASE,
  authentication: {
    type: 'default',
    options: {
      userName: process.env.DB_USER,
      password: process.env.DB_PASSWORD,
    },
  },
  options: {
    encrypt: process.env.DB_ENCRYPT === 'true',
    trustServerCertificate: process.env.DB_TRUST_SERVER_CERTIFICATE === 'true',
  },
};

const getConnection = () => {
  return new tedious.Connection(config);
};

export { getConnection, tedious as Connection, tedious as Request };
```

**Explicación del Código de Conexión:**

* `import 'dotenv/config';`: Carga las variables de entorno desde el archivo \`.env\` al objeto `process.env`.
* `import tedious from 'tedious';`: Importa la librería `tedious` para interactuar con SQL Server.
* `const config = { ... };`: Define la configuración de la conexión, utilizando las variables de entorno para:
    * `server`: Dirección del servidor SQL Server.
    * `database`: Nombre de la base de datos.
    * `authentication`: Detalles de autenticación (usuario y contraseña).
    * `options`: Opciones de conexión como `encrypt` y `trustServerCertificate`.
* `const getConnection = () => { ... };`: Función para crear y retornar una nueva conexión `tedious.Connection` con la configuración.
* `export { getConnection, tedious as Connection, tedious as Request };`: Exporta la función de conexión y la librería `tedious`.

#### **4. Utiliza la conexión en tu archivo `server.js`**

```javascript
// server.js
import express from 'express';
import { getConnection, Request } from './db.js'; // Importa la conexión

const app = express();
const port = 3000;

app.get('/citas', async (req, res) => {
  const connection = getConnection();

  connection.on('connect', (err) => {
    if (err) {
      console.error('Error al conectar a la base de datos:', err);
      res.status(500).send('Error al conectar a la base de datos');
    } else {
      console.log('Conectado a la base de datos');
      ejecutarConsulta(connection, res);
    }
  });

  connection.connect();
});

function ejecutarConsulta(connection, res) {
    // Reemplaza el codigo con tu consulta SQL 
  const request = new Request("SELECT * FROM CitaMedica", (err, rowCount) => {
    if (err) {
      console.error('Error al ejecutar la consulta:', err);
      res.status(500).send('Error al obtener las citas médicas');
    } else {
      console.log(`${rowCount} filas retornadas.`);
    }
    connection.close();
  });

  const citas = [];
  // En cada lectura de cada fila/row recuperada, añade al arreglo de citas
  request.on('row', (columns) => {
    const cita = {};
    columns.forEach((column) => {
      cita[column.metadata.colName] = column.value;
    });
    citas.push(cita);
  });

  request.on('requestCompleted', () => {
    res.json(citas);
  });

  connection.execSql(request);
}

app.listen(port, () => {
  console.log(`Servidor escuchando en http://localhost:${port}`);
});
```

#### **4. Ejecuta la Aplicación:**

En tu terminal, dentro del directorio del proyecto (`hola-mundo-express`), ejecuta el siguiente comando para iniciar el servidor:

```bash
npm run dev
```

#### **5. Visualiza el Resultado:**
Abre tu navegador web y navega a `http://localhost:3000/citas`. Deberías ver el resultado de la consulta.
