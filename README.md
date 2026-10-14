# Arzpal.github.io

Portafolio personal de Andrés Ruiz Palomino. Página estática, sin build, sin dependencias npm.
Se sirve gratis desde GitHub Pages en `https://arzpal.github.io`.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | Todo el sitio (HTML + CSS + JS en un solo archivo, autocontenido) |
| `Andres-Ruiz-*.pdf` | Los CV (Technical Designer, Unity Developer, Technical Artist). Se generan desde `E:\Arzpal\futuro\cv\*.html` |
| `assets/reel.mp4` + `reel.jpg` | El reel del hero: 8 cortes, 24 s, 960×540, mudo |
| `assets/clips/<id>.mp4` + `.jpg` | El loop mudo de cada ficha (5-7 s) y su póster |
| `assets/gallery/` | Capturas de la galería del inspector (1280 px de ancho) |
| `assets/og.png` | La vista previa al pegar el enlace (1200×630) |
| `assets/board-dust*.webp` | El polvo del pizarrón horneado (escritorio y móvil) |
| `docs/*.pdf` | Los documentos de diseño publicados (en inglés, versión limpia para reclutador) |

No se publican (están en `.gitignore`): `_media-crudo/` (videos fuente, capturas en crudo y la
plantilla de `og.png`), `_traducciones/` (los .md de los documentos y su script de PDF) y los
`BRIEF-*`.

## Estructura de la página (v5, 14-oct-2026)

Barra fija · **hero** (nombre, rol, frase, selector de perfil y el reel) · línea de datos ·
**Recognition** · **Projects** (selector repetido, 5 destacados del perfil y el archivo compacto con
filtros por disciplina) · **Timeline** (estudios y formación en un gráfico en el tiempo; Education ya
no es sección aparte) · banda "How I work with AI" (dos carriles) · **Contact**.

- **Perfil:** Technical Designer por defecto; `?r=ud` abre Unity Developer. Los destacados viven en
  `FEAT` y el orden del archivo en `ARCHIVE_ORDER`, los dos por `id`.
- **Inspector:** cualquier hoja o renglón lo abre. Pieza grande (video o captura) con tira debajo,
  datos al costado, nota de IA, puntos y, solo si hay algo que poner, los bloques
  *How it works* (el diagrama), *Design docs* y *Code*.
- Bilingüe EN/ES; el idioma se guarda en `localStorage` (`arzpal-lang`).

## Cómo agregar contenido

El contenido vive en `PROJECTS[]`, `TOOLS[]`, `TIMELINE[]` y `EDUCATION[]`, al inicio del
`<script>`. Cada campo de texto acepta `"texto"` o `{ en: "...", es: "..." }`.

Campos de una ficha (`PROJECTS` / `TOOLS`):

| Campo | Para qué |
|---|---|
| `id` | Nombre estable. Lo usan `FEAT` y `ARCHIVE_ORDER` |
| `acc` | Color de identidad (`--k-<acc>` / `--ki-<acc>`) |
| `youtubeId` | Solo el ID. El iframe no se crea hasta el clic |
| `loop` | `assets/clips/<id>.mp4`; su póster es el `.jpg` del mismo nombre |
| `cover` / `image` | Portada propia (gana sobre el póster y sobre YouTube) |
| `gallery` | `[{ src, cap }]`. Con 4 o más, la hoja grande muestra 3 en tira |
| `diag` | Diagrama en tiza. Solo es la figura si no hay material; si lo hay, va a *How it works* |
| `award` | Sello de resaltador. **Solo con fuente pública**, igual que Recognition |
| `proof` | `{ td: [i, j], ud: [i, j] }`: índices de bullets reales para la hoja grande |
| `docs` | `[{ url, label, sub }]`: PDFs en `docs/`, se abren en pestaña nueva |
| `disc` | Disciplinas para los filtros del archivo: `design`, `code`, `impl`, `techart`, `vfx`, `tools`, `proto` |
| `orgShort` | Org corto para la cabecera de la hoja, si el largo no entra |
| `codeLinks` | El render lo soporta pero **no se usa**: Andrés prefirió no enlazar código (14-oct) |
| `aiUse` | La nota de IA del inspector: **para qué** se usó. Sin campo, sin nota |
| `nda` | Sello NDA |

⚠️ `HANDS.p` y `HANDS.t` (la nota a mano de cada hoja) son **posicionales**: siguen el orden de
`PROJECTS` y `TOOLS`. Si reordenas un arreglo, reordena el otro.

⚠️ **La IA ya no se marca por tarjeta ni por punto** (decidido el 13-oct-2026: daba a entender que
todo estaba hecho con IA). Los `{ t, ai: true }` de los bullets se quedan como registro de autoría,
pero no se pintan; lo que se publica es `aiUse`.

## Media

Con ffmpeg (instalado con winget). Recetas usadas:

```bash
# loop de ficha: 960x540, 30 fps, mudo
ffmpeg -ss <inicio> -i fuente.mp4 -t <dur> -an -vf "scale=960:540:force_original_aspect_ratio=increase,crop=960:540,fps=30,format=yuv420p" -c:v libx264 -crf 28 -preset slow -movflags +faststart assets/clips/<id>.mp4
# su póster
ffmpeg -ss <inicio> -i fuente.mp4 -frames:v 1 -vf "scale=960:540:force_original_aspect_ratio=increase,crop=960:540" -q:v 4 assets/clips/<id>.jpg
# captura de galería
ffmpeg -i captura.png -vf "scale=1280:-2" -q:v 4 assets/gallery/<nombre>.jpg
```

El reel se arma cortando cada tramo a `-crf 18`, uniéndolos con `concat` y saliendo a
`-crf 26 -preset slow -movflags +faststart`. Los cortes y su orden están en la memoria del
proyecto (`E:\Arzpal\memoria\portfolio-web.md`).

## El polvo del pizarrón (horneado)

Las capas `wide`, `ghosts`, `dry` y `cloud` ya no se pintan en vivo: van en `assets/board-dust.webp`
(y `-m` para móvil), capturadas de las capas reales sobre negro y convertidas a blanco con alfa =
luminancia, que equivale al `screen` sin fusión (comparado píxel a píxel: idéntico). Si se recalibra
el polvo: volver a poner las capas, capturarlas con Chrome headless a 2560×1440 (y 860×1864 con
`--clean:0`) con todo lo demás oculto, y convertir con `ffmpeg ... alphamerge ... -c:v libwebp -lossless 1`.
El grano fino sigue vivo.

La trayectoria lee de `TIMELINE` y `EDUCATION`: `start`, `end` (`"now"` = la fecha de `LAST_UPDATE`),
`fuzzy` (`start` / `end` / `both`: extremos sin mes conocido, que se dibujan difuminados) y `open`
(el id de la ficha que abre la fila).

## Regenerar el PDF del CV

Chrome headless desde Git Bash. Las tres banderas del medio no son opcionales: sin ellas Chrome
sale **sin escribir y sin error** (verifica la hora del archivo y que diga `bytes written`):

```bash
"/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu --no-sandbox --no-pdf-header-footer --user-data-dir="$TEMP/chrome-pdf" --virtual-time-budget=10000 --run-all-compositor-stages-before-draw --print-to-pdf="E:/Arzpal.github.io/Andres-Ruiz-Technical-Designer.pdf" "file:///E:/Arzpal/futuro/cv/Andres-Ruiz-Technical-Designer.html"
```

Son 2 páginas a propósito (corte forzado en `#projects-section`).

## Probar en local

Hace falta un servidor (los videos piden rangos y `file://` bloquea cosas):

```bash
npx --yes http-server -p 8777 -c-1
```

## Publicar

El repo `Arzpal/Arzpal.github.io` es público y Pages sirve desde `main` / `(root)`. Cada push lo
actualiza solo. **Nada privado aquí adentro.**
