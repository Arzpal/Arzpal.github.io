# Arzpal.github.io

Portafolio personal de Andrés Ruiz Palomino. Página estática, sin build, sin dependencias npm.
Se sirve gratis desde GitHub Pages en `https://arzpal.github.io`.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | Todo el sitio (HTML + CSS + JS en un solo archivo, autocontenido) |
| `Andres-Ruiz-Game-Programmer.pdf` | El CV que descarga el botón del hero |
| `assets/` | Imágenes propias de las tarjetas (`cover`) y de los modales (`image`) |

> El bundle del rediseño v2 (`newREADME.md`, `reference-design-v2.dc.html`,
> `current-site-before.html`) se borró el 18-ago-2026: solo queda viva la versión final.
> El sitio anterior al rediseño sigue recuperable con `git show HEAD:index.html` mientras no se
> comitee el rediseño. La paleta exacta vive en las variables de `:root` de `index.html`, y el
> modelo de datos y las reglas del diseño están en este archivo.

## Estructura de la página

Barra fija · hero · **01 Projects** · **02 Timeline** · **03 Tools & Prototypes** ·
**04 Education** · **05 Get in touch**. Las tarjetas de Projects y Tools abren un modal con
video, rol, descripción, puntos y links.

La página es **bilingüe EN/ES** con un switch en la barra, y tiene un **toggle "AI-assisted"**
que muestra u oculta los puntos marcados como hechos con IA. Ambos se guardan en `localStorage`
(`arzpal-lang`, `arzpal-showai`).

## Cómo agregar contenido

Todo el contenido vive en cuatro arreglos al inicio del `<script>` de `index.html`:

| Arreglo | Sección |
|---|---|
| `PROJECTS[]` | 01 Projects |
| `TIMELINE[]` | 02 Timeline |
| `TOOLS[]` | 03 Tools & Prototypes |
| `EDUCATION[]` | 04 Education |

**Cada campo de texto acepta dos formas:** `"texto"` (igual en los dos idiomas) o
`{ en: "...", es: "..." }`. Si a una entrada le falta el par `en`/`es`, esa vista queda vacía
en el idioma que falte.

### Un proyecto nuevo

Copia un bloque de `PROJECTS` y ponlo en la posición donde debe salir: **el orden del arreglo
es el orden en pantalla.**

- `youtubeId`: **solo el ID**, no la URL. De `youtube.com/watch?v=dQw4w9WgXcQ` va `"dQw4w9WgXcQ"`.
  Con `null` la tarjeta muestra el patrón rayado y el aviso; en cuanto haya video se llena sola.
- `nda: true`: cambia ese aviso a "sin material público · bajo NDA".
- `roleTags`: **máximo 3**. Con más, la imagen deja de leerse.
- `short`: **una sola frase**. Si crece, la altura del grid se descuadra.
- `tags`: `professional` | `team` | `personal` | `prototype` | `freelance`.

### Un punto hecho con IA

Cambia el string por `{ t: "...", ai: true }`. Eso pinta el badge `CON IA` y lo controla el
toggle de la barra. **Cuando el toggle está apagado esos puntos se ocultan, no se atenúan.**
En `TOOLS` la marca es `ai: true` en la entrada completa.

Criterio acordado: describir **el resultado concreto**, no la habilidad de usar IA.

### Imágenes propias (mejor que el thumbnail de YouTube)

1. Guárdalas en `assets/` con nombre por proyecto (`assets/dont-chicken-out-cover.jpg`).
2. Agrega a la entrada `cover` (tarjeta, 16:9, ~640×360, JPG ~80%) e `image` (fondo del modal,
   ~1280×720).
3. `cover` gana sobre el thumbnail de YouTube e `image` gana sobre `maxresdefault`. Si no
   existen, se mantiene el comportamiento actual.
4. La tarjeta aplica `grayscale(1)` y solo lo quita en hover: **las capturas tienen que leerse
   bien en gris.**

El video no carga hasta que abres el modal: hasta entonces solo se ve la miniatura. Eso mantiene
la página liviana y evita que YouTube trackee a quien solo pasó de largo.

## Reglas para no romper el diseño

1. **Nada de colores nuevos.** Si hace falta un tono, derívalo de las variables de `:root`.
2. Máximo **dos fondos**: `#08090a` y `#0f1112`. Las secciones alternan.
3. El **aqua `#00e5c0` es acento**, no color de área. Nunca como fondo de bloques grandes.
4. Una familia por rol: **Archivo** títulos · **IBM Plex Sans** cuerpo · **IBM Plex Mono**
   etiquetas, fechas, roles y nav. Nada de fuentes nuevas.
5. Los textos de proyecto son del autor: **no reescribirlos** al agregar datos, solo traducir
   cuando falte el par `en`/`es`.
6. La barra va **por encima del modal** a propósito (`z-index` 200 vs 100): se puede cambiar
   idioma o el toggle con un proyecto abierto.
7. Los enlaces de la nav hacen scroll por JS **sin cambiar la URL**. Nada de `href="#work"`.

## Regenerar el PDF del CV

El diálogo de impresión del navegador corta mal. Usa Chrome en modo headless, que respeta
el CSS de `@media print` sin depender de la configuración del diálogo:

```bash
"/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu --no-pdf-header-footer --print-to-pdf="E:\Arzpal.github.io\Andres-Ruiz-Game-Programmer.pdf" "file:///E:/Arzpal/futuro/cv/Andres-Ruiz-Game-Programmer.html"
```

El corte de página está forzado en la sección `#projects-section`: página 1 = perfil,
skills y experiencia; página 2 = proyectos y educación. Sin ese `break-before`, el bloque
de Zarzilla saltaba entero y dejaba media página en blanco.

## Probar en local

Abrir `index.html` con doble clic funciona. Si el navegador bloquea algo por `file://`:

```bash
node -e "const h=require('http'),f=require('fs'),p=require('path');h.createServer((q,s)=>{let r=q.url==='/'?'/index.html':q.url;f.readFile(p.join(process.cwd(),r),(e,b)=>e?s.writeHead(404).end():s.writeHead(200,{'content-type':r.endsWith('.html')?'text/html; charset=utf-8':'application/octet-stream'}).end(b))}).listen(8777,()=>console.log('http://127.0.0.1:8777'))"
```

## Publicar

El repo público `Arzpal/Arzpal.github.io` ya existe y Pages sirve desde `main` / `(root)`.
Cada push lo actualiza solo:

```bash
git add index.html README.md
git commit -m "Rediseño v2"
git push
```

> Ojo: el repo tiene que ser público para que Pages funcione en cuenta gratis, así que nada
> privado aquí adentro.
