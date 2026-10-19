# molicgv.art

Portfolio artístico personal de Mónica Gómez, artista que explora la pintura acrílica texturizada inspirada en el mar, el movimiento del agua y los paisajes marinos.

El sitio presenta una selección de obras, una introducción a la artista, su proceso creativo y sus canales de contacto. Su propósito es ofrecer una presencia digital clara y accesible para su trabajo artístico, sin atribuirle una trayectoria, exposiciones ni datos que no estén confirmados.

## Tecnologías y características

- HTML5 semántico y CSS3; no utiliza JavaScript, frameworks ni dependencias de ejecución.
- Diseño adaptable a móvil y escritorio.
- Accesibilidad: navegación semántica, enlace para saltar al contenido, foco visible y textos alternativos para las imágenes.
- Metadatos SEO y Microdata de Schema.org para la artista y las obras representadas.
- Fotografías JPEG originales con variantes WebP responsive y fallback JPEG.
- La página conserva `noindex, nofollow` porque la publicación definitiva aún no se ha realizado.

## Estructura

```text
.
├── index.html                 # Contenido, navegación, metadatos y Microdata
├── styles.css                 # Presentación adaptable y accesible
├── assets/images/             # Fotografías originales y variantes WebP
├── AGENTS.md                  # Reglas para continuar el desarrollo
├── specs/                     # Especificación, plan y tareas SDD
├── docs/MAINTENANCE.md        # Manual de uso, edición, validación y publicación
└── .vscode/settings.json      # Preferencias locales del editor
```

## Visualización local

Abre `index.html` directamente en un navegador o sigue la [guía de mantenimiento](docs/MAINTENANCE.md) para usar el servidor HTTP estático integrado en Python. La hoja de estilos y las imágenes usan rutas relativas; no se instalan paquetes.

## Contacto

- Instagram: [@molicgv.art](https://www.instagram.com/molicgv.art/?hl=es)
- TikTok: [@molicgv.art](https://www.tiktok.com/@molicgv.art)
- Correo: [molicgv.art@gmail.com](mailto:molicgv.art@gmail.com)

## Estado

El portfolio está implementado como sitio estático y aún no se ha publicado en el dominio definitivo. RUST continúa en desarrollo y actualmente no hay exposiciones programadas.

## Mejoras futuras

- Revisar manualmente la experiencia responsive, el recorrido con teclado y tecnologías de asistencia.
- Publicar el sitio y revisar la política de indexación cuando el dominio definitivo esté listo.
- Incorporar nuevas obras únicamente cuando se disponga de contenido e imágenes confirmados.

## Créditos y atribuciones

Este repositorio nació como un ejercicio de 4Geeks Academy y posteriormente fue adaptado como portfolio artístico personal. La plantilla HTML Hello original atribuye su creación a Alejandro Sánchez y a otros colaboradores de 4Geeks Academy; se conserva esta referencia como crédito al origen del proyecto.

Los iconos SVG de Instagram y TikTok proceden de Font Awesome Free Brands y se mantienen bajo [CC BY 4.0](https://fontawesome.com/license/free). La atribución también se conserva junto a los iconos en `index.html`.
