# HUGOSPERU · Portafolio del Derecho — portada estática

Sitio estático puro (HTML + CSS + un poco de JS). No tiene build, no tiene dependencias,
no necesita `npm`. Se puede publicar tal cual en Azure Static Web Apps, Cloudflare Pages,
Netlify, GitHub Pages o cualquier servidor de archivos.

## Archivos

```
hugosperu-web/
├── index.html                  Portada: hero, 17 materias, estudio, contacto
├── aviso-de-privacidad.html    Aviso de privacidad (Ley 29733)
├── 404.html                    Página de error
├── staticwebapp.config.json    Cabeceras de seguridad y 404 (solo Azure SWA)
└── assets/
    ├── styles.css              Toda la hoja de estilos
    └── favicon.svg             Icono (§)
```

## Datos que ya están puestos

| Dato | Valor | Dónde cambiarlo |
|---|---|---|
| WhatsApp | +51 999 760 751 | `index.html`, buscar `wa.me/51999760751` (5 apariciones) |
| Correo | hugosperu@gmail.com | `index.html` y `aviso-de-privacidad.html` |
| Materias | 17, enlazadas a `hhugosperu.wpcomstaging.com` | `index.html`, sección `#materias` |
| Otros proyectos | ARCONSEG y canales Odysee, plegados | `index.html`, bloque `<details class="otros">` |

**No hay dirección física ni teléfono fijo** porque no se proporcionaron. Si se añaden,
el sitio de contacto es la sección `#contacto`.

## Ver en local

Abre `index.html` con doble clic. Para probar como se verá en producción, con rutas
absolutas funcionando:

```
python -m http.server 8080
```

y visita `http://localhost:8080`.

## Publicar en Azure Static Web Apps

1. Crea un repositorio en GitHub y sube **el contenido** de esta carpeta a la raíz
   (no la carpeta entera: `index.html` debe quedar en la raíz del repo).
2. Portal de Azure → busca **Static Web Apps** → **Crear**.
3. Suscripción: *Azure subscription 1*. Grupo de recursos: nuevo, `rg-hugosperu`.
   Nombre: `hugosperu-web`. **Plan type: Free** (viene en Standard y Standard cobra).
4. Origen: **GitHub** → autoriza → repositorio y rama `main`.
5. En *Build Details*: **Build Presets: Custom**, *App location* `/`, *Api location* vacío,
   *Output location* vacío. Al no haber build, no hay nada que compilar.
6. Crear. El primer despliegue tarda 2–4 minutos y queda en
   `https://<nombre-aleatorio>.azurestaticapps.net`.

Cada `git push` a `main` republica solo.

## Dominio propio (hugosperu.com)

El dominio está hoy en GoDaddy Website Builder. **GoDaddy no soporta ALIAS/ANAME en el
apex**, así que `hugosperu.com` (sin www) no se puede apuntar directamente a un Static
Web App. Dos salidas:

- **Cloudflare gratis como DNS** (recomendada): mantén el dominio registrado en GoDaddy,
  cambia solo los *nameservers* a los que te dé Cloudflare, y crea ahí un `CNAME` de
  `@` → el hostname `*.azurestaticapps.net` (Cloudflare hace CNAME flattening) y otro de
  `www` → igual. Azure pide además un `TXT` `asuid` con el ID de verificación.
- **Azure DNS**: zona DNS en Azure, unos 0,50 US$ al mes, dentro del presupuesto.

Antes de mover el DNS: baja el TTL actual a 300 s y espera una hora. Y no borres el sitio
de GoDaddy hasta comprobar que el nuevo responde.

## Pendiente de decidir

Las 17 materias enlazan hoy al sitio de WordPress (`hhugosperu.wpcomstaging.com`), que es
un entorno de *staging*: no está pensado para ser la casa definitiva del contenido. La
migración de esas páginas y sus PDFs a estático es una segunda fase, aparte de esta portada.
