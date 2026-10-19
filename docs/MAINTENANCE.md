# Guía de mantenimiento de molicgv.art

Manual práctico para retomar, editar, validar y publicar el portfolio. La web es estática: su contenido está en `index.html`, los estilos en `styles.css` y las fotografías en `assets/images/`. No requiere Flask, JavaScript, frameworks, paquetes ni un proceso de compilación.

## 1. Abrir y ejecutar localmente

### Requisitos

- VS Code para editar el proyecto.
- Un navegador moderno para visualizarlo.
- Git para crear ramas, guardar cambios y sincronizar con GitHub.
- Python 3 solo si quieres previsualizar mediante un servidor HTTP local. Se usa su módulo estándar `http.server`; no hay que instalar Flask ni añadir archivos al proyecto.

### Abrir el repositorio

En VS Code, selecciona **Archivo > Abrir carpeta...** y elige la carpeta raíz del repositorio `molicgv-art` (la que contiene `index.html`). También puedes abrir PowerShell en esa carpeta y ejecutar `code .`, si el comando de VS Code está disponible en el `PATH`.

Para abrir PowerShell desde VS Code, selecciona **Terminal > Nuevo terminal**. Comprueba que la terminal está situada en la raíz:

```powershell
Get-Location
Test-Path .\index.html
```

El segundo comando debe devolver `True`. Si no, cambia a la carpeta del proyecto:

```powershell
Set-Location "C:\ruta\a\molicgv-art"
```

### Servidor HTTP local (opción recomendada)

En Windows, prueba primero el lanzador de Python:

```powershell
py -m http.server 8000
```

Si `py` no está disponible, prueba:

```powershell
python -m http.server 8000
```

En macOS o Linux, normalmente se usa:

```sh
python3 -m http.server 8000
```

Ejecuta el comando desde la raíz del repositorio. Abre [http://localhost:8000/](http://localhost:8000/) en el navegador. Para detener el servidor, vuelve a la terminal y pulsa `Ctrl+C`.

También puedes abrir `index.html` directamente, aunque el servidor HTTP representa mejor cómo se cargará la página publicada. Después de editar, guarda el archivo (`Ctrl+S`) y recarga el navegador (`Ctrl+R`). No hay recarga automática. Si el navegador conserva CSS o imágenes antiguas, fuerza una recarga con `Ctrl+F5`.

Si el puerto 8000 ya está ocupado, detén el otro servidor o usa otro puerto, por ejemplo `py -m http.server 8001`, y visita `http://localhost:8001/`.

## 2. Estructura del proyecto

| Ruta | Función | ¿Necesaria para mostrar la web? |
|---|---|---|
| `index.html` | Contenido visible, secciones, enlaces, metadatos y Microdata de Schema.org. | Sí |
| `styles.css` | Colores, tipografía, composición adaptable, estados de foco e imágenes. | Sí |
| `assets/images/` | Fotografías originales JPEG/JPG y sus variantes WebP. | Sí, para las imágenes |
| `README.md` | Presentación, estado y resumen del proyecto. | No |
| `AGENTS.md` | Instrucciones para futuros cambios asistidos por Copilot. | No |
| `specs/` | Especificación, plan y tareas del desarrollo siguiendo SDD. | No |
| `docs/MAINTENANCE.md` | Este manual para operar y mantener el sitio. | No |

Para ejecutar la web solo se necesitan `index.html`, `styles.css` y `assets/images/`. README, instrucciones, especificaciones y esta guía son documentación de desarrollo; no se cargan en la página.

## 3. Cambiar textos y enlaces

Abre `index.html`, busca el identificador o la clase indicada y modifica solo el texto entre las etiquetas. Mantén las etiquetas, identificadores, atributos `itemprop` y enlaces intactos salvo que también estés actualizando esa función. Guarda y recarga la página para revisar el resultado.

### Inicio

- La sección es `<section id="inicio">`.
- El título visible es `<h1 id="inicio-title">molicgv.art</h1>`.
- La frase de presentación usa `.hero-tagline` y el párrafo introductorio usa `.hero-intro`.
- El título de la pestaña y la descripción para buscadores están en `<title>` y `<meta name="description">`, dentro de `<head>`. Si cambia la identidad o el contenido principal, mantenlos coherentes con lo visible.

### Sobre mí

La sección es `#sobre-mi`. El saludo está en `.about-name` y la biografía en `.about-text`. El nombre “Mónica Gómez” lleva `itemprop="name"`; conserva ese atributo exclusivamente alrededor del nombre, no del saludo.

### Trayectoria y obras

La sección es `#trayectoria`; la galería actual está en `.work-grid`. Cada obra usa un `figure.work-item` con un `figcaption` que contiene su título (`h3`) y descripción (`p`). Publica únicamente títulos, datos y descripciones confirmados por la artista. RUST debe seguir identificado como obra en desarrollo mientras ese sea su estado; no le asignes detalles ni una fotografía no aprobados.

### Próximas presentaciones

La sección es `#presentaciones`; el texto de agenda usa `.presentation-note`. Actualízalo cuando cambie la información confirmada sobre exposiciones. Actualmente indica que no hay exposiciones programadas.

### Contacto

El enlace de Instagram aparece en la sección `#presentaciones` y en el pie, dentro de `.footer-links`. TikTok y el correo electrónico están en el pie. Cambia los valores `href` de los enlaces y sus `aria-label` cuando corresponda; conserva los nombres accesibles. El correo usa el esquema `mailto:`, por ejemplo `mailto:nombre@dominio.example`. Comprueba las URL autorizadas antes de guardarlas.

## 4. Añadir una obra a la galería

1. **Prepara la fotografía.** Usa una imagen nítida y bien encuadrada de la obra. Decide la orientación de presentación y conserva su proporción; no recortes ni alteres el original sin autorización.
2. **Guárdala en `assets/images/`.** Usa un nombre simple, en minúsculas y sin espacios, por ejemplo `obra-nueva.jpg`. Conserva siempre el original; nunca lo sobrescribas con una versión comprimida.
3. **Anota los datos reales del archivo.** Comprueba ancho y alto en las propiedades del archivo o en tu editor. Usa esas dimensiones en los atributos `width` y `height` del `<img>` y asegúrate de que las variantes mantienen la misma proporción.
4. **Prepara WebP si hace falta.** No todas las obras requieren múltiples tamaños; evita añadir derivados innecesarios. Si tienes disponible la utilidad independiente `cwebp`, puedes crear variantes sin incorporarla como dependencia del proyecto:

   ```powershell
   cwebp -q 82 -resize 480 0 assets/images/obra-nueva.jpg -o assets/images/obra-nueva-480.webp
   cwebp -q 82 -resize 900 0 assets/images/obra-nueva.jpg -o assets/images/obra-nueva-900.webp
   ```

   `-q` controla la calidad (0–100) y `-resize 480 0` limita el ancho a 480 píxeles manteniendo la proporción. Ajusta tamaños y calidad al uso real y revisa visualmente el resultado. `cwebp` es una herramienta externa opcional; no se instala ni se declara en el repositorio. Como alternativa, exporta una copia WebP desde un editor de imágenes que admita ese formato. Guarda siempre la exportación con otro nombre y conserva intacto el original. Si no puedes generar WebP, añade la obra con JPEG; la etiqueta `<img>` seguirá funcionando como imagen.

5. **Añade un elemento al `.work-grid`.** El ejemplo muestra la estructura actual con un título y una descripción que debes reemplazar por información confirmada. Las dimensiones `900 × 600` son solo ilustrativas: cámbialas por las dimensiones reales del JPEG.

   ```html
   <figure class="work-item" itemprop="subjectOf" itemscope itemtype="https://schema.org/VisualArtwork">
     <div class="image-frame">
       <picture>
         <source
           type="image/webp"
           srcset="assets/images/obra-nueva-480.webp 480w, assets/images/obra-nueva-900.webp 900w"
           sizes="(min-width: 48rem) 40vw, 100vw">
         <img
           src="assets/images/obra-nueva.jpg"
           width="900"
           height="600"
           alt="Descripción concreta de los elementos visibles en la obra."
           itemprop="image"
           loading="lazy"
           decoding="async">
       </picture>
     </div>
     <figcaption>
       <h3>Título confirmado de la obra</h3>
       <p itemprop="description">Descripción breve basada en información confirmada.</p>
     </figcaption>
   </figure>
   ```

   `<picture>` permite ofrecer distintos formatos. El navegador usa el primer `<source>` que puede mostrar; `type="image/webp"` anuncia WebP. `srcset` enumera los archivos y sus anchos intrínsecos (`480w`, `900w`); esos números deben coincidir con el ancho real de cada archivo. `sizes` estima el ancho que ocupará la imagen en el diseño para que el navegador elija una variante apropiada. `<img src="...jpg">` es el fallback y también aporta `alt`, dimensiones y carga diferida.

6. **Escribe un `alt` informativo.** Describe brevemente lo que una persona necesita saber de la imagen, por ejemplo los elementos, colores o composición que realmente se ven. No repitas “imagen de” ni inventes título, técnica o significado. Si la foto es puramente decorativa y no aporta información, usa `alt=""`.
7. **Revisa la cuadrícula.** `.work-grid` tiene una columna en móvil y dos desde `48rem`; el CSS actual da una colocación especial a `.work-item--sea`. Para obras nuevas, empieza con `work-item` sin ese modificador. Si aumentan las obras y quieres tres columnas u otro ritmo, modifica `.work-grid` y sus reglas `@media` en `styles.css`, y prueba las filas resultantes en móvil y escritorio.
8. **Valida en navegador.** Con el servidor local abierto, comprueba que la obra, el pie de foto y los enlaces no se solapen en una ventana estrecha y otra amplia. Revisa DevTools > Network para errores 404 y confirma que se sirve WebP cuando está disponible. Abre también la ruta JPEG directamente; para probar la selección del fallback sin editar archivos, desactiva temporalmente el `<source>` desde DevTools y vuelve a inspeccionar la imagen. Recarga normalizando la caché cuando termines.

## 5. Cambiar estilos

Los estilos están en `styles.css`. Las variables globales están en `:root`, al inicio del archivo:

- Colores: `--color-ivory`, `--color-deep-blue`, `--color-petrol`, `--color-mist`, `--color-sand`, `--color-white`, `--color-ink` y `--color-line`.
- Tipografías: `--font-display` para títulos y detalles editoriales; `--font-body` para el texto general. Son pilas de fuentes del sistema, sin fuentes remotas.
- Espaciados: `--space-1` a `--space-7`, más `--page-gutter` para los márgenes laterales.
- Anchos: `--content-width` está definido como variable, pero no asumas que controla todo el ancho del sitio: comprueba sus usos antes de cambiarlo.

Actualiza una variable y guarda; los selectores que usan `var(--...)` reflejarán el cambio. Revisa contraste, especialmente enlaces y foco, sobre cada fondo.

Selectores prácticos existentes:

- Navegación: `.site-header`, `.site-header nav`, `.nav-list`, `.site-header .nav-list a` y `.site-header .nav-list a:hover`.
- Secciones y cabeceras: `.section`, `.hero`, `.about`, `.works`, `.presentations`, `h1`, `h2`, `h3`.
- Imágenes: `.image-frame`, `.image-frame img`, `.hero-image`, `.about-image`, `.work-item .image-frame`, `.process-image`.
- Galería y pies: `.work-grid`, `.work-item`, `.work-item figcaption`.
- Enlaces: `.text-link`, `.footer-links`, `.footer-icon-link`.

La cuadrícula es de una columna por defecto; el breakpoint `@media (min-width: 48rem)` la distribuye en dos. El breakpoint `64rem` ajusta algunos tamaños de título y la portada. Al modificar la navegación, imágenes o distribución, conserva `:focus-visible`, el tamaño adaptable y la regla `@media (prefers-reduced-motion: reduce)`.

## 6. Git y GitHub

El repositorio personal es `mgomezv26/molicgv-art` y el remoto esperado se llama `origin`. Antes de trabajar, confirma la carpeta y los remotos:

```powershell
git status
git remote -v
```

`origin` debe mostrar `https://github.com/mgomezv26/molicgv-art.git`. No añadas el repositorio de 4Geeks como remoto.

Flujo recomendado para una mejora:

```powershell
git switch main
git pull origin main
git switch -c docs/actualizar-guia
git status
git add README.md AGENTS.md docs/MAINTENANCE.md
git commit -m "docs: actualizar guía de mantenimiento"
git push -u origin docs/actualizar-guia
```

Adapta la lista de `git add` a los archivos que realmente quieras incluir. Otros mensajes posibles:

```text
feat: añadir una obra a la galería
fix: corregir texto alternativo de la portada
docs: aclarar pasos de publicación
```

Después de subir una rama, crea un pull request desde GitHub con base `main` y revisa el diff antes de fusionarlo. Para cambios pequeños también puedes acordar otro flujo, pero evita trabajar directamente sobre `main` sin revisar el estado.

Para revisar antes del commit, usa `git diff` y `git diff --check`. Para consultar el historial, ejecuta `git log --oneline`. Si descartas una edición local, primero inspecciona `git diff -- ruta/al/archivo` y guarda una copia o crea una rama; `git restore -- ruta/al/archivo` reemplaza esa edición con la versión del último commit.

Para deshacer un commit ya compartido, prefiere un commit inverso:

```powershell
git revert <id-del-commit>
```

Para recuperar archivos de un commit anterior sin mover toda la rama, inspecciónalos primero con `git show <id-del-commit>:ruta/al/archivo`; si decides restaurarlos, `git restore --source=<id-del-commit> -- ruta/al/archivo` sustituye ese archivo en el árbol de trabajo. Revisa `git diff` antes de guardar el resultado. Evita `git reset --hard` y no fuerces el push.

Si hay cambios sin guardar, no ejecutes `pull`, `restore` o un cambio de rama sin revisar `git status`. Si el push falla, comprueba la rama con `git branch --show-current`, el remoto con `git remote -v` y autentícate mediante Git Credential Manager o el mecanismo oficial de GitHub; nunca pegues tokens en comandos ni en el repositorio. Si la rama quedó detrás de `main`, ejecuta `git fetch origin` y, estando en tu rama de trabajo, `git rebase origin/main`; resuelve conflictos archivo por archivo y vuelve a validar antes del push.

## 7. Publicar con GitHub Pages

### Estado actual y pasos pendientes

El remoto y la rama local `main` están configurados para `origin` en `mgomezv26/molicgv-art`. El sitio aún no se ha publicado en el dominio definitivo. No se ha activado ni verificado GitHub Pages en los ajustes del repositorio; su activación y la URL pública quedan pendientes. Esta guía no activa Pages ni cambia los metadatos.

Cuando decidas publicar:

1. En GitHub, abre `mgomezv26/molicgv-art` y entra en **Settings > Pages**.
2. En **Build and deployment**, elige **Deploy from a branch**, selecciona `main` y la carpeta `/(root)`, y guarda. El proyecto no necesita una acción de compilación ni un framework.
3. Espera a que GitHub Pages termine el despliegue. La sección **Settings > Pages** mostrará la URL pública. Para un repositorio de proyecto, la URL esperada suele ser `https://mgomezv26.github.io/molicgv-art/`; confirma la URL que muestre GitHub antes de compartirla.
4. Abre la URL pública y comprueba la página, imágenes, estilos, enlaces y consola del navegador. Revisa el resultado del despliegue en **Actions** o en el estado que muestre la página de Pages.

Después, publica cambios mediante una rama y un pull request, o mediante el flujo acordado para `main`. Pages vuelve a desplegar la rama configurada tras los cambios publicados; espera a que el despliegue termine y vuelve a revisar la URL pública.

### SEO antes de hacer pública la web

En `index.html`, revisa `<title>`, `meta name="description"`, los metadatos `og:*` y el Microdata visible. Actualmente existe `<meta name="robots" content="noindex, nofollow">`; mantenlo hasta que la artista autorice expresamente la indexación y la publicación. No añadas canonical mientras no se confirme el dominio definitivo y accesible por HTTPS. Activar Pages no equivale a autorizar retirar `noindex`.

### Dominio personalizado (futuro)

Cuando haya un dominio definitivo y control de su DNS, sigue las instrucciones vigentes de GitHub Pages y del registrador: añade el dominio en **Settings > Pages > Custom domain**, configura los registros DNS indicados por GitHub, espera la verificación y habilita HTTPS cuando esté disponible. Comprueba que el dominio resuelve, sirve la web y redirige correctamente antes de actualizar metadatos sociales o añadir canonical. No inventes ni escribas una URL canónica antes de esa confirmación.

## 8. Validación y problemas frecuentes

### Lista de comprobación

- **HTML/CSS:** revisa los cambios en VS Code, guarda y abre la página en el navegador. Comprueba la consola de DevTools (`F12`) por errores y valida HTML con el validador Nu HTML Checker y CSS con el validador CSS de W3C cuando los cambios lo justifiquen. No añadas herramientas al proyecto para una comprobación puntual.
- **Teclado:** recorre con `Tab` y `Shift+Tab`; el foco debe verse. El primer `Tab` muestra “Saltar al contenido”; `Enter` activa enlaces y la navegación por anclas lleva a la sección correcta.
- **Responsive:** prueba una anchura móvil y una de escritorio con DevTools > Device Toolbar. Comprueba que no haya scroll horizontal, contenido cortado, textos superpuestos o imágenes deformadas. Contrasta también los cambios alrededor de los breakpoints de `48rem` y `64rem`.
- **Enlaces:** verifica que los `href="#..."` tengan un `id` correspondiente y único; prueba los enlaces externos, Instagram, TikTok y `mailto:` en navegador. Para los enlaces externos, confirma que el destino y la cuenta siguen siendo los autorizados.
- **Accesibilidad:** comprueba nombres accesibles de enlaces, textos alternativos, jerarquía de encabezados, foco visible y contraste. Si se revisa movimiento, activa la preferencia de movimiento reducido del sistema.
- **SEO y datos estructurados:** confirma que título y descripción coinciden con el contenido, que `noindex, nofollow` sigue presente mientras no haya autorización, que no se ha añadido canonical prematuro y que las propiedades Microdata describen solo contenido visible y real.
- **Lighthouse:** en Chrome/Edge abre DevTools > **Lighthouse**, selecciona móvil o escritorio y ejecuta la auditoría de Accesibilidad, Prácticas recomendadas, SEO y Rendimiento. Interpreta SEO teniendo en cuenta que `noindex` es intencional mientras el sitio no esté listo para indexarse.
- **WebP y fallback:** en DevTools > Network revisa que las variantes WebP y sus rutas respondan. Confirma que cada `src` JPEG existe y se abre directamente. Para comprobar el fallback, desactiva temporalmente el `<source>` desde DevTools y verifica que el `<img src="...jpg">` se renderiza; no borres archivos para esta prueba.

### Soluciones habituales

| Problema | Qué comprobar |
|---|---|
| Una imagen no aparece | Asegúrate de que el servidor se inició en la raíz del repositorio; compara literalmente mayúsculas, guiones, extensión y ruta con el nombre en `assets/images/`. Mira Network por respuestas 404. Si falla WebP, comprueba también el `src` JPEG. |
| Los estilos no cambian | Guarda `styles.css`, revisa que `index.html` enlaza `styles.css` en la misma raíz y fuerza recarga con `Ctrl+F5`. DevTools > Network ayuda a detectar un CSS antiguo o una ruta incorrecta. |
| El servidor no arranca o muestra “address already in use” | Comprueba `py --version` o `python --version`. Si 8000 está ocupado, usa `py -m http.server 8001` y visita el puerto 8001. Detén el proceso con `Ctrl+C`. |
| Se muestra un listado de carpetas o falta la página | Detén el servidor y vuelve a iniciarlo desde la carpeta que contiene `index.html`; confirma con `Test-Path .\index.html` en PowerShell. |
| Un enlace interno no funciona | Compara el identificador del fragmento del enlace, por ejemplo `#trayectoria`, con el valor exacto de `id` en la sección. Los identificadores deben ser únicos. |
| `git push` es rechazado | Comprueba `git status`, rama y `git remote -v`; confirma que `origin` es el repositorio personal. Ejecuta `git fetch origin` y actualiza tu rama de forma segura antes de reintentar. No uses `--force` para ocultar el problema. |
| Hay conflictos al actualizar la rama | Lee cada marcador de conflicto, conserva el contenido correcto de ambas ramas cuando corresponda, elimina los marcadores y ejecuta `git diff --check`. No sobrescribas trabajo sin revisar `git status`. |

## Referencias del proyecto

- [README](../README.md): resumen e identidad del portfolio.
- [AGENTS.md](../AGENTS.md): reglas de contenido, tecnología, accesibilidad y SEO para los cambios futuros.
- [Especificación](../specs/001-landing-artist/spec.md), [plan](../specs/001-landing-artist/plan.md) y [tareas](../specs/001-landing-artist/tasks.md): documentos SDD.