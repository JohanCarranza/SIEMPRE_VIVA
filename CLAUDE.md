# Siempre Viva - Handoff técnico para Claude Code

## 1. Negocio y propósito

**Siempre Viva** es una tienda de detalles hechos a mano enfocada en:

- Flores eternas a crochet.
- Figuras amigurumi a crochet.
- Regalos personalizados y piezas decorativas duraderas.

El objetivo de la web es funcionar como catálogo comercial ligero, permitir revisar productos con fotos/perspectivas, agregar unidades disponibles al carrito y enviar el pedido por WhatsApp para confirmar disponibilidad, personalización, pago y entrega.

## 2. Stack tecnológico actual

El proyecto actualmente es una web estática sin framework.

- **HTML:** `index.html`
- **CSS:** CSS embebido dentro de `index.html`
- **JavaScript:** JavaScript vanilla embebido dentro de `index.html`
- **Frameworks:** ninguno
- **Build tools:** ninguno
- **Package manager:** ninguno
- **Backend:** ninguno
- **Base de datos:** ninguna
- **Estado local:** `localStorage` para el carrito
- **Integración externa:** WhatsApp mediante enlaces `https://wa.me/...`
- **Fuentes externas:** Google Fonts
  - `DM Sans`
  - `Playfair Display`

No hay dependencias instaladas como `node_modules`, React, Astro, Tailwind, Vite, Next.js ni librerías de UI.

## 3. Arquitectura y mapa de archivos

Estructura actual esperada:

```text
SIEMPRE_VIVA_WEB/
├─ index.html
├─ CLAUDE.md
├─ .gitignore
├─ Images/
│  ├─ GENERAL.png
│  ├─ NAILONG-1.png
│  ├─ NAILONG-2.png
│  ├─ NAILONG-3.png
│  ├─ NAILONG-4.png
│  ├─ CHARMANDER-1.png
│  ├─ CHARMANDER-2.png
│  ├─ CHARMANDER-3.png
│  ├─ CHARMANDER-4.png
│  ├─ CAPIBARA-PEQUEÑO-1.png
│  ├─ CAPIBARA-PEQUEÑO-2.png
│  ├─ CAPIBARA-PEQUEÑO-3.png
│  ├─ CAPIBARA-PEQUEÑO-4.png
│  └─ CAPIBARA-LLAVERO.png
├─ .git/
└─ .agents/
```

Notas:

- `index.html` contiene toda la UI, estilos y lógica.
- `Images/` contiene todos los assets visuales usados por el sitio.
- Hay una carpeta `.git`, pero en la última revisión `git status` falló con `fatal: not a git repository`. Antes de migrar a GitHub, verificar o reinicializar Git.
- `.agents/` es metadato local de herramientas; no debería ser necesario para el sitio.

## 4. Estado actual de desarrollo

### Listo / funcional

- Landing page responsive.
- Header sticky con navegación interna.
- Hero principal con imagen `Images/GENERAL.png`.
- Sección de estadísticas.
- Catálogo dinámico renderizado desde el arreglo `products` en JavaScript.
- Filtros por categoría.
- Búsqueda por texto.
- Catálogo horizontal deslizable para evitar una grilla demasiado larga.
- Tarjetas de producto simplificadas con:
  - nombre
  - disponibilidad
  - precio o `Cotizar`
  - botón `+`
- Modal de producto con:
  - imagen principal ampliada
  - miniaturas de perspectivas
  - variantes cuando aplica
  - disponibilidad
  - botón de agregar al carrito o consulta
- Carrito lateral persistido en `localStorage`.
- Control básico de stock:
  - No permite agregar más unidades que el stock disponible.
  - Productos sin stock aparecen como no agregables.
- Envío del pedido por WhatsApp.
- Número de WhatsApp configurado: `51930628439`.
- Productos actuales:
  - `Nailong tejido`
  - `Charmander tejido`
  - `Capibara Pequeño`
  - `Capibara Llavero`
- Variantes internas de `Capibara Pequeño`:
  - `Pequeño · 5 cm`
  - `Normal · 10 a 12 cm`
  - `Gigante · 30 cm`

### Pendiente / mejoras recomendadas

- Optimizar imágenes para web:
  - Convertir PNG pesados a WebP.
  - Mantener una carpeta de originales fuera del deploy o ignorada por Git.
  - Actualizar rutas en `index.html` tras convertir.
- Separar código en archivos:
  - `index.html`
  - `styles.css`
  - `app.js`
  - opcionalmente `products.js` o `products.json`
- Agregar favicon.
- Agregar metadata Open Graph para compartir en redes.
- Revisar enlaces sociales reales:
  - Instagram
  - TikTok
- Verificar estado real de Git:
  - Existe `.git`, pero `git status` no reconoce el repositorio en la revisión actual.
  - Si está corrupto o incompleto, reinicializar antes de subir a GitHub.
- Definir precios reales para:
  - `Capibara Normal`
  - `Capibara Gigante`
  - `Capibara Llavero`
- Definir stock real para productos por pedido.
- Considerar panel o archivo externo para productos si el catálogo crece.
- Agregar pruebas manuales documentadas para:
  - abrir modal de cada producto
  - cambiar variantes
  - agregar al carrito
  - eliminar del carrito
  - enviar a WhatsApp
  - probar en móvil

### Bugs conocidos / riesgos

- El proyecto depende de JavaScript embebido en un solo archivo, por lo que cambios grandes pueden romper el render del catálogo si no se valida sintaxis.
- Los nombres de archivo con `Ñ` pueden ser problemáticos en algunos hostings. Recomendado renombrar a ASCII antes de publicar:
  - `CAPIBARA-PEQUEÑO-1.png` -> `CAPIBARA-PEQUENO-1.png`
  - etc.
- El carrito en `localStorage` puede conservar datos antiguos si se cambian IDs o variantes. La lógica actual filtra datos inválidos, pero conviene limpiar `localStorage` durante pruebas grandes.
- No hay validación de stock real en servidor; todo el stock es informativo y local.

## 5. Reglas de desarrollo para Claude Code

### Diseño y UX

- Mantener diseño elegante, artesanal y comercial.
- Priorizar experiencia móvil primero.
- No agregar secciones tipo landing genérica si el usuario pide funcionalidad de tienda.
- Mantener tarjetas de producto limpias:
  - nombre
  - disponibilidad
  - precio/cotizar
  - botón de acción
- Evitar saturar las tarjetas con texto largo.
- Los detalles largos deben ir en el modal.
- Validar que textos no se corten en móvil.
- Mantener catálogo deslizable horizontalmente si hay varios productos.

### Catálogo y productos

- Los productos actuales viven en el arreglo `products` dentro de `index.html`.
- Cada producto debe tener:
  - `id`
  - `name`
  - `category`
  - `price` o `null` si es cotización
  - `stock`
  - `tag`
  - `desc`
  - `images`
- Si un producto tiene variantes, usar `variants`.
- Cada variante debe tener:
  - `id`
  - `label`
  - `name`
  - `size` si aplica
  - `price` o `null`
  - `stock`
  - `tag`
  - `images` si usa fotos distintas
- No reutilizar variantes cuando el usuario indique que debe ser un producto independiente.

### WhatsApp

- Mantener pedidos por WhatsApp.
- Número actual: `51930628439`.
- Formato obligatorio para enlaces: `https://wa.me/51930628439?...`
- Los mensajes deben incluir:
  - producto
  - variante si aplica
  - cantidad
  - precio/cotización
  - solicitud de confirmación de disponibilidad, personalización y entrega

### Imágenes

- Las imágenes actuales están en `Images/`.
- No ignorar imágenes que estén siendo usadas por la web.
- Recomendación antes de producción:
  - convertir imágenes de producto a `.webp`
  - usar nombres ASCII y sin espacios
  - mantener originales pesados en `Images/originals/` o fuera del repo
- Al cambiar nombres de imágenes, actualizar todas las rutas en `index.html`.

### Git y migración

- Pedir confirmación explícita antes de:
  - `git add`
  - `git commit`
  - `git push`
  - crear ramas
  - reescribir historial
- No ejecutar `git reset --hard` salvo solicitud explícita.
- Revisar `git status` antes de cualquier commit.
- Si Git está roto o no inicializado, informar antes de actuar.

### Validación mínima después de cambios

Después de editar `index.html`, validar:

- Sintaxis JavaScript con Node si está disponible.
- Que el catálogo renderice productos al cargar.
- Que las rutas de imágenes existan.
- Que el modal abra.
- Que el carrito agregue/elimine.
- Que WhatsApp abra con el mensaje correcto.

## 6. Comandos útiles

No hay build. Para abrir localmente:

```powershell
start .\index.html
```

Para listar archivos:

```powershell
Get-ChildItem -Recurse -File
```

Para verificar rutas de imágenes referenciadas en `index.html`:

```powershell
$html=Get-Content -Raw index.html
$matches=[regex]::Matches($html, 'Images/[A-Za-z0-9_ÑñÁÉÍÓÚáéíóúÜü-]+\.png') | ForEach-Object { $_.Value } | Sort-Object -Unique
$matches | ForEach-Object { [PSCustomObject]@{Path=$_; Exists=(Test-Path $_)} }
```

