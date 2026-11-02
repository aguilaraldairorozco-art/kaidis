# KAIDIS – Sorgolitas

Sitio web comercial con catálogo público, pedidos por WhatsApp y panel administrativo conectado a Supabase.

## Requisitos
- Node.js 20 o superior.
- Una cuenta gratuita de Supabase.
- Una cuenta de hosting estático, por ejemplo Netlify o Vercel.

## 1. Configurar la base de datos y el almacenamiento
1. Crea un proyecto en https://supabase.com/.
2. Abre **SQL Editor** en el proyecto.
3. Copia y ejecuta todo el contenido de `supabase/schema.sql`.
4. En **Authentication > Providers**, deja habilitado Email.
5. En **Authentication > Settings**, desactiva el registro público (Enable sign ups). El panel debe ser privado.
6. En **Authentication > Users**, crea manualmente el usuario o usuarios administradores. No publiques esas credenciales.

Las políticas permiten lectura pública del catálogo activo y escritura solo a usuarios autenticados. No compartas la clave `service_role`; el sitio utiliza exclusivamente la clave pública `anon`.

## 2. Conectar el sitio
1. Copia `.env.example` y renómbralo a `.env`.
2. En Supabase abre **Project Settings > API**.
3. Copia Project URL en `VITE_SUPABASE_URL`.
4. Copia la clave `anon` / publishable en `VITE_SUPABASE_ANON_KEY`.
5. Instala dependencias y ejecuta:

```bash
npm install
npm run dev
```

Abre la dirección local que muestre Vite. Para generar la versión de publicación:

```bash
npm run build
```

El resultado se genera en `dist/`.

## 3. Publicar en internet
### Netlify
1. Sube este proyecto a un repositorio privado o público de GitHub.
2. En Netlify selecciona **Add new site > Import an existing project**.
3. Configura `npm run build` como comando y `dist` como directorio de publicación.
4. Agrega las dos variables `VITE_SUPABASE_URL` y `VITE_SUPABASE_ANON_KEY` en las variables de entorno.
5. Publica. Cada actualización del repositorio puede volver a desplegarse automáticamente.

También puedes usar Vercel con framework preset **Vite**, build `npm run build` y output `dist`.

## 4. Usar el panel
- En el sitio, pulsa **Administración** al final de la página.
- Inicia sesión con el correo y contraseña creados en Supabase.
- En Productos puedes crear, editar y eliminar productos.
- En Publicidad puedes publicar y eliminar anuncios/promociones.
- En Galería puedes subir y eliminar fotografías.
- En Ajustes puedes cambiar la descripción, WhatsApp, correo, logotipo y archivo MP3.
- Los archivos se guardan en el bucket persistente `kaidis-media` de Supabase Storage.

## 5. Pedidos y música
- El formulario de pedido arma un mensaje y abre WhatsApp `+52 971 150 1098`. El visitante debe pulsar enviar en WhatsApp.
- La música aparece solo cuando se carga un MP3 y nunca se reproduce automáticamente. El visitante controla reproducir/pausar.
- Los precios son opcionales. No se precargan variedades ni precios no proporcionados.

## 6. Notas importantes antes de producción
- Este proyecto es una base funcional inicial. Antes de anunciarlo, prueba el login, cargas y eliminaciones con una cuenta administradora.
- Mantén desactivado el registro público y crea solo usuarios de confianza.
- El bucket de medios es público para que el sitio pueda mostrar las imágenes. No subas documentos privados ni datos personales.
- La tabla de galería permite lectura pública. El contenido que se agregue allí debe ser material que KAIDIS desea publicar.
- Configura dominio, aviso de privacidad y textos comerciales definitivos antes de difundir el sitio.
- La opción de cambiar logotipo y música se guarda en los ajustes. Los medios anteriores no se borran automáticamente del almacenamiento al reemplazarlos.
- La web no cobra en línea ni confirma inventario: recibe solicitudes por WhatsApp para confirmar disponibilidad y entrega.
