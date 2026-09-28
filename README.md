##  MarketSoft API
API REST desarrollada para la gestión básica de un supermercado, implementando un backend robusto con arquitectura MVC, persistencia de datos relacional y documentación interactiva.

Autor: Santiago Toro Amariles

Institución: Universidad de Manizales

## Tecnologías Exigidas
Este proyecto se hizo utilizando el siguiente stack tecnológico moderno:

Node.js (v24.21.0): Entorno de ejecución para JavaScript en el servidor.

Express: Framework web rápido y minimalista para Node.js.

PostgreSQL (v17): Sistema de gestión de bases de datos relacional.

Sequelize: ORM (Object-Relational Mapping) para la gestión y modelado de datos en PostgreSQL.

Swagger-UI-Express & swagger-jsdoc: Herramientas para la documentación interactiva de la API.(Tambien se hicieron pruevas en thunder client)

## 2. Guía de Creación de la Base de Datos (PostgreSQL)(Nota:aca dejo la guia de como yo cree la base de datos para el proyecto)
1. Abri PostgreSQL o pgadmin

2. creee una nueva base de datos con el nombre de la carpeta
(ejemplo)
SQL
CREATE DATABASE marketsoft;
Me asegure de tener las credenciales de acceso a mi servidor local de PostgreSQL listas para el archivo de configuración.

3. Variables de Entorno (.env)
Crea un archivo llamado .env en la raíz del proyecto basándote en el entorno local. Configura los siguientes parámetros requeridos para la conexión con la base de datos:

Fragmento de código
PORT=3000
DB_NAME=en que la pernona ponga
DB_USER=tu_usuario_postgres
DB_PASSWORD=tu_contraseña_postgres
DB_HOST=localhost
DB_PORT=5432
## Instrucciones de Instalación y Ejecución
Sigue estos pasos para clonar, instalar dependencias y poner en marcha el servidor localmente:

Clonar o abrir el repositorio en tu entorno de desarrollo (Visual Studio Code).

Instalar las dependencias del proyecto ejecutando en la terminal:

Bash
npm install
Iniciar el servidor en modo de producción/desarrollo:

Bash
npm start

Nota: si llega a aparecer que el puerto 3000 ya lo utilizaron pon Stop-Process -Id (Get-NetTCPConnection -LocalPort 3000).OwningProcess -Force
despues abre tu navegador web y accede a la documentación interactiva de Swagger en:

Plaintext
http://localhost:3000/api-docs/
## Arquitectura MVC y Relaciones Sequelize
El proyecto implementa el patrón MVC (Modelo-Vista-Controlador):

Models (models/): Este define las tablas y las Relaciones de Sequelize (por ejemplo, un Provider tiene muchos Products; una Sale pertenece a un User y tiene muchos SaleDetails).

Controllers (controllers/): Contiene la lógica de negocio para procesar las peticiones.

Routes (routes/): Define los endpoints HTTP y los conecta con sus controladores respectivos.

View: La API responde mediante estructuras JSON limpias y se documenta visualmente con Swagger.

## Nota sobre sequelize.sync
El proyecto utiliza la siguiente configuración para facilitar la ejecución académica local:

JavaScript
sequelize.sync({ alter: true })
Esto Me permitio que Sequelize actualice automáticamente las tablas en la base de datos según los modelos definidos.

## Endpoints Disponibles
Productos
Plaintext
GET    /api/products
GET    /api/products/:id
POST   /api/products
PUT    /api/products/:id
DELETE /api/products/:id
Usuarios
Plaintext
GET    /api/users
GET    /api/users/:id
POST   /api/users
PUT    /api/users/:id
DELETE /api/users/:id
Proveedores
Plaintext
GET    /api/providers
GET    /api/providers/:id
POST   /api/providers
PUT    /api/providers/:id
DELETE /api/providers/:id
Ventas
Plaintext
GET    /api/sales
GET    /api/sales/:id
POST   /api/sales
PUT    /api/sales/:id
DELETE /api/sales/:id
Detalles de venta
Plaintext
GET    /api/sale-details
GET    /api/sale-details/:id
POST   /api/sale-details
PUT    /api/sale-details/:id
DELETE /api/sale-details/:id

## Ejemplos de JSON  Nota: estos son algunos ejemplos que utilice para ejecutar (estos son creados aleatoriamente con ia)
Crear proveedor
POST /api/providers

JSON
{
  "name": "Distribuciones ABC",
  "phone": "3001234567",
  "email": "ventas@abc.com",
  "city": "Bogotá"
}
Crear producto
POST /api/products

JSON
{
  "name": "Arroz 1 kg",
  "description": "Arroz blanco",
  "price": 4500,
  "stock": 50,
  "providerId": 1
}
Crear usuario
POST /api/users

JSON
{
  "name": "Ana Pérez",
  "email": "ana@example.com",
  "role": "customer"
}
Crear venta
POST /api/sales
(El total no se envía; el backend lo calcula automáticamente a partir de los precios y cantidades, descontando además el stock disponible).

JSON
{
  "userId": 1,
  "details": [
    {
      "productId": 1,
      "quantity": 2
    }
  ]
}
## Validaciones Implementadas
Productos: Nombre obligatorio, precio mayor que 0, stock no negativo y validación de existencia del proveedor.

Usuarios: Nombre obligatorio, formato de correo electrónico válido y unicidad del email.

Ventas: Usuario existente, mínimo un producto por venta, validación de stock suficiente y cálculo automático del total de la transacción.

DetalleVenta: Venta y producto existentes, cantidad mayor a 0 y toma del precio unitario actual del producto.

### Experiencia con Errores Comunes durante el Desarrollo
Durante la construcción e integración del backend en este proyecto profe se tuvieron que solventaron varias situaciones clave que fortalecieron el proceso de desarrollo:

## Route.get() requires a callback function:

Ocurrió por un error de importación o exportación en los archivos de rutas, provocando que una función del controlador llegara como undefined. Se resolvió estructurando correctamente las exportaciones de los controladores y asegurando que cada ruta apuntara a una función válida.

## EADDRINUSE:

address already in use :::3000: este se presentó cuando una instancia anterior del servidor se quedó colgada ejecutándose en segundo plano en el puerto 3000. Se solucionó cerrando el proceso activo en el sistema operativo antes de reiniciar la aplicación.

Ademas se tuvo que solverntar algunos otros probles con escritura de archos y funcionamiento dealgunas de las herramientas tecnologicas lo cual me llevo a buscar soluciones como borrar historias probar de otras formas como Thunder client etc

## Llave duplicada en la Clave Primaria (Providers_pkey): 

- Al probar los endpoints POST en Swagger, se intentaba enviar manualmente un ID estático ya existente ("id": 1). Se comprendió que las llaves primarias autoincrementales no deben enviarse en el cuerpo de la petición (requestBody), permitiendo que sea la base de datos la encargada de gestionarlas de forma limpia.

## 11. Conclusión Final

Mi conclucion final es que durante el desarrollo de este proyecto es que me ha permitido consolidar varios consceptos que no cocia o no los los sabia aplicar correctamente ademas para este se utilizaron diseños avanzados de arquitectursa backend escalables por otro lado no menos importante se logro la integracion exitosa entre Node.js, Express, Sequelize y PostgreSQL lo que nos demuestra la importancia de mantener un buen desacoplamiento de capas (MVC) y una documentación clara (Swagger). por ultimo yo pienso que este proyecto sienta bases solidas para futuras creaciones de sistemas comerciales seguros con validaciones robustas y seguras y la integridad de los datos

## Muchas gracisas por su tiempo