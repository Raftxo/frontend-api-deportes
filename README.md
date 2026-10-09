# Deportes Ayto — frontend

Frontend del portal municipal de instalaciones deportivas. Permite consultar espacios y ofrece páginas para iniciar sesión, registrarse, gestionar reservas y acceder a las vistas de usuario. Se comunica con una API externa.

## Desarrollo local

```bash
npm install
npm run dev
```

Configura `VITE_API_URL` con la URL base de la API, incluido `/api`. Para desarrollo local puedes partir de `.env.example`; el backend debe estar disponible y permitir peticiones CORS desde el frontend.

## Despliegue en Netlify

- **Build command:** `npm run build`
- **Publish directory:** `dist`
- Define `VITE_API_URL` en las variables de entorno del sitio con la URL HTTPS de la API de producción, incluido `/api`. Vite incorpora esta variable al compilar, así que hay que volver a desplegar después de cambiarla.

### Problema: `login.html` devuelve “Page not found”

Vite no incluía automáticamente todas las páginas HTML en el build de producción: solo generaba `index.html`, aunque `login.html` y las demás páginas estuvieran en el repositorio. Netlify publica el contenido de `dist`, por lo que esas rutas no existían en el sitio.

`vite.config.js` define explícitamente las entradas multipágina (`index.html`, `login.html`, `registro.html`, `dashboard.html` y `conserje.html`). Tras ejecutar `npm run build`, comprueba que las cinco aparezcan en `dist`; después, confirma que Netlify publique esa carpeta y vuelve a desplegar. No hace falta una regla de redirección SPA para estas páginas HTML.

### Si el frontend no conecta con la API

Comprueba que `VITE_API_URL` apunte al backend desplegado y no a `localhost`, que la URL use HTTPS y que el backend permita el origen de Netlify mediante CORS. Revisa también la consola del navegador y los logs de Netlify para distinguir errores de API de errores de publicación.
