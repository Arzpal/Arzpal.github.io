# Arzpal.github.io

Portafolio personal de Andrés Ruiz Palomino. Página estática, sin build, sin dependencias.
Se sirve gratis desde GitHub Pages en `https://arzpal.github.io`.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | Todo el sitio (HTML + CSS + JS en un solo archivo, autocontenido) |
| `Andres-Ruiz-Game-Programmer.pdf` | El CV que descarga el botón del hero (**falta subirlo**) |

## Cómo agregar un proyecto

Abre `index.html`, baja hasta el bloque comentado `PROYECTOS` (cerca del final) y copia la
plantilla que está ahí dentro del arreglo `PROJECTS`. Los campos:

- `title`, `org`, `dates`: texto libre.
- `tags`: `"professional"`, `"prototype"`, `"freelance"`, `"personal"`. Puedes poner varios.
  Los botones de filtro se generan solos: solo aparece el filtro de una etiqueta que realmente
  esté en uso.
- `youtubeId`: **solo el ID**, no la URL. De `youtube.com/watch?v=dQw4w9WgXcQ` va `"dQw4w9WgXcQ"`.
  Si el video es un Short o un `youtu.be/XXXX`, el ID es igualmente la parte final.
  Si todavía no hay video, deja `null` y la tarjeta se maqueta sola a ancho completo.
- `bullets` y `links` son opcionales.

El video no carga hasta que le das clic: hasta entonces solo se ve la miniatura. Eso mantiene la
página liviana aunque tengas 15 proyectos, y evita que YouTube trackee a quien solo pasó de largo.

## Pendientes antes de publicar

- [x] LinkedIn real (`linkedin.com/in/arzpal`) en hero y contacto.
- [x] `Andres-Ruiz-Game-Programmer.pdf` generado y en esta carpeta (2 páginas A4).
- [x] Videos de YouTube en los 6 proyectos que tienen material grabado.
- [x] Nada de detalle interno de DOB: el juego va como codename "Project Monster Shelter".
- [ ] Crear el repo público y hacer push (ver abajo).

## Regenerar el PDF del CV

El diálogo de impresión del navegador corta mal. Usa Chrome en modo headless, que respeta
el CSS de `@media print` sin depender de la configuración del diálogo:

```bash
"/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu --no-pdf-header-footer --print-to-pdf="E:\Arzpal.github.io\Andres-Ruiz-Game-Programmer.pdf" "file:///E:/Arzpal/futuro/cv/Andres-Ruiz-Game-Programmer.html"
```

El corte de página está forzado en la sección `#projects-section`: página 1 = perfil,
skills y experiencia; página 2 = proyectos y educación. Sin ese `break-before`, el bloque
de Zarzilla saltaba entero y dejaba media página en blanco.

## Publicar

Crea en GitHub un repo **público** llamado exactamente `Arzpal.github.io` (vacío, sin README) y:

```bash
git init
git add .
git commit -m "Portfolio site"
git branch -M main
git remote add origin https://github.com/Arzpal/Arzpal.github.io.git
git push -u origin main
```

En el repo: **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`**. En un par de
minutos queda vivo en `https://arzpal.github.io`. Cada `git push` posterior lo actualiza solo.

> Ojo: el repo tiene que ser público para que Pages funcione en cuenta gratis, así que nada
> privado aquí adentro.
