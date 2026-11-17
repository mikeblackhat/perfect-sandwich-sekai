# Guía de Publicación y Mantenimiento de Perfect Sandwich en Sekai

Este documento explica cómo está estructurado el proyecto, por qué fallaba la carga de texturas antes, y cómo empaquetar y subir actualizaciones a Sekai sin complicaciones.

---

## 1. El Problema de los Assets en Sekai

En Sekai, los juegos corren dentro de un `iframe` aislado (sandbox) sin un servidor de archivos relativo:
- Si tu código usa `<img src="textures/Bacon_Asset.png">`, Sekai **no puede resolver la ruta** porque no existe la carpeta local dentro del sandbox.
- Por otro lado, si se incrustan las imágenes en Base64 sin comprimir (archivos PNG brutos), el archivo HTML pesa más de **3.4 MB**, superando el límite estricto de subida de Sekai (**1.5 MB**).

### La Solución Implementada:
1. **Compresión WebP con Canal Alfa**: Las 11 texturas originales se comprimen a WebP con calidad 85 y compresión de alta fidelidad, reduciendo el peso de ~1.8 MB a solo ~360 KB conservando la transparencia y nitidez.
2. **Empaquetado Monolítico**: El script genera [index_sekai.html](index_sekai.html) (751 KB), que contiene todo el código y todas las texturas directamente incrustadas como `data:image/webp;base64,...`.
3. **Validación con Flash**: Sekai aprueba el juego al 100% pasando todos los análisis estáticos sin errores (`findings: []`).

---

## 2. Herramientas Automatizadas del Skill `sekai-publisher`

El nuevo skill global está instalado en:
`~/.gemini/config/skills/sekai-publisher/`

Incluye 3 scripts clave que puedes ejecutar desde la terminal en cualquier momento:

### A. Autenticación y Refresco de Tokens (`sekai_auth.py`)
Sekai rota sus tokens de acceso cada 15 minutos. Este script verifica la validez y refresca automáticamente los tokens en `opencode.db` y `mcp_config.json`:
```bash
python3 ~/.gemini/config/skills/sekai-publisher/scripts/sekai_auth.py
```

### B. Empaquetar el Juego (`pack_game.py`)
Toma [index_light.html](index_light.html) (que tiene el código limpio y rutas relativas para desarrollo local) y genera [index_sekai.html](index_sekai.html) listo para producción:
```bash
python3 ~/.gemini/config/skills/sekai-publisher/scripts/pack_game.py \
  index_light.html \
  --assets textures \
  --output index_sekai.html
```

### C. Subir o Actualizar el Borrador en Sekai (`upload_game.py`)
Sube la versión empaquetada directamente a tu cuenta de Sekai sin tener que copiar y pegar código:
```bash
# Para crear un nuevo borrador:
python3 ~/.gemini/config/skills/sekai-publisher/scripts/upload_game.py \
  index_sekai.html \
  --title "Perfect Sandwich" \
  --intro "¡Apila los 8 ingredientes y el pan superior con física realista!"

# Para actualizar el borrador existente:
python3 ~/.gemini/config/skills/sekai-publisher/scripts/upload_game.py \
  index_sekai.html \
  --title "Perfect Sandwich" \
  --game-id "9d52ea2a-a978-4372-8953-144ffb81e7ec"
```

---

## 3. Enlaces Directos del Juego Actual

- 🎮 **Jugar en navegador (PC / Mac):**  
  [https://sekai.ai/drafts/9d52ea2a-a978-4372-8953-144ffb81e7ec](https://sekai.ai/drafts/9d52ea2a-a978-4372-8953-144ffb81e7ec)

- 📱 **Editar en Sekai Studio (Móvil / App):**  
  [https://sekai.ai/create?draft=9d52ea2a-a978-4372-8953-144ffb81e7ec](https://sekai.ai/create?draft=9d52ea2a-a978-4372-8953-144ffb81e7ec)

---

## 4. Archivos en este Repositorio

- `index_light.html`: Versión ligera (23 KB) con referencias a `textures/`, ideal para probar y modificar localmente.
- `index_sekai.html`: Versión optimizada (751 KB) con texturas WebP incrustadas, lista para subir a Sekai.
- `index.html`: Versión original con PNGs Base64 de alta resolución (3.4 MB).
- `textures/`: Carpeta con los 11 assets gráficos originales.
