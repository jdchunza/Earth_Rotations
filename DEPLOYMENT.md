# Publicar TerraQuake en Railway

El `Dockerfile` de la raíz empaqueta el mapa, la página inicial y la API Spring
Boot en un solo servicio. Railway puede construirlo directamente desde GitHub.
MongoDB debe ser una instancia administrada en MongoDB Atlas; no uses
`localhost` para la base de datos en producción.

## Preparar MongoDB Atlas

1. Crea un clúster y un usuario de base de datos en MongoDB Atlas.
2. En **Network Access**, permite conexiones desde Railway. Si no tienes una
   lista de IP de salida fija, permite `0.0.0.0/0` y usa una contraseña fuerte,
   exclusiva para ese usuario.
3. Copia la URI de conexión de la aplicación, reemplaza la contraseña y añade
   `/earth_rotations` como nombre de base de datos antes de los parámetros
   (`?`). Si la contraseña contiene caracteres especiales, codifícala para URL.

## Crear el servicio en Railway

1. En Railway, crea un proyecto con **New Project > Deploy from GitHub repo** y
   selecciona `jdchunza/Earth_Rotations`, rama `main`.
2. En **Variables** del servicio, añade `SPRING_DATA_MONGODB_URI` y pon como
   valor la URI completa de Atlas. Este nombre usa el enlace estándar de
   configuración de Spring Boot. También se admite `MONGODB_URI` como
   alternativa.
3. Confirma que el servicio use el `Dockerfile` de la raíz y espera a que el
   despliegue termine correctamente.
4. En **Settings > Networking**, genera un dominio público para el servicio.
5. Abre el dominio para ver el mapa. La página inicial está en
   `/Pantalla%20inicial/index1.html`; su botón **Explorar el mapa** abre el mapa.
   Comprueba también `/actuator/health`, que debe responder con `status: UP`.

Railway usa precios basados en consumo y puede pedir un método de pago; confirma
el costo y los créditos disponibles en tu cuenta antes de desplegar. La
aplicación también necesita Internet para cargar Three.js y las texturas del
globo desde sus CDN.
