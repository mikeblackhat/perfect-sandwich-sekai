# 🥪 Perfect Sandwich — Sekai Edition

Recreación interactiva en HTML5 / JavaScript del popular mini-juego viral de TikTok y Effect House **"Perfect Sandwich"**, optimizado para la plataforma [Sekai](https://sekai.ai).

Apila con precisión 8 ingredientes secuenciales y el pan superior sobre la base usando físicas realistas de **Matter.js**, calculando tu porcentaje de precisión según la desviación lateral de cada ingrediente.

---

## 🎮 Enlaces y Demo

* **Jugar en Sekai (Web / PC):** [https://sekai.ai/drafts/9d52ea2a-a978-4372-8953-144ffb81e7ec](https://sekai.ai/drafts/9d52ea2a-a978-4372-8953-144ffb81e7ec)
* **Editar en Sekai Studio:** [https://sekai.ai/create?draft=9d52ea2a-a978-4372-8953-144ffb81e7ec](https://sekai.ai/create?draft=9d52ea2a-a978-4372-8953-144ffb81e7ec)

---

## ✨ Características

- **8 Ingredientes Secuenciales + Pan Superior**:
  1. Lechuga (`Veggie`)
  2. Tocino (`Bacon`)
  3. Chocolate (`Choco`)
  4. Huevo frito (`Egg`)
  5. Plátano (`Banana`)
  6. Jamón (`Ham`)
  7. Gatito (`Cat`)
  8. Calamar (`Squid`)
  9. Pan Superior (`Bread_Asset`)
- **Física de Alta Estabilidad con Matter.js**:
  - Calibrador de colisión de 36px en la base para evitar atravesamiento (*tunneling*).
  - 16 iteraciones de posición y velocidad por frame.
  - Gravedad ajustada a 2.4 con impulso inicial controlado.
  - Espaciado compacto y ceñido de 16px entre capas.
- **Puntuación de Precisión**:
  - Cálculo de precisión lateral (0% a 100%) midiendo la desviación respecto al centro.
  - Bonificación por velocidad y bonificación por colocación del pan superior.
- **Efectos y Pulido Visual**:
  - Rayos de victoria giratorios (`Rays.png`) en capas detrás del sándwich.
  - Cuenta regresiva de estabilidad 3-2-1 antes de mostrar el resultado final.
  - Botón de reinicio interactivo ("Try Again").
  - Selector bilingüe (EN / ES) integrado en el HUD.
- **Compatible con Sandbox de Sekai**:
  - Build autocontenida (`index_sekai.html`) con texturas comprimidas en WebP (751 KB < 1.5 MB).
  - Sin dependencias de rutas relativas locales.

---

## 📁 Estructura del Proyecto

```
perfect-sandwich-sekai/
├── textures/                # 11 texturas originales en PNG
│   ├── Bacon_Asset.png
│   ├── Banana_Asset.png
│   ├── Bread_Asset.png
│   ├── Cat_Asset.png
│   ├── Choco_Asset.png
│   ├── Egg_Asset.png
│   ├── Ham_Asset.png
│   ├── Rays.png
│   ├── Retry_Button.png
│   ├── Squid_Asset.png
│   └── Veggie_Asset.png
├── index_light.html         # Versión ligera (23 KB) para desarrollo local
├── index_sekai.html         # Build optimizada para Sekai (751 KB, WebP Base64)
├── index.html               # Versión completa original (3.4 MB)
├── SEKAI_INSTRUCTIONS.md    # Guía detallada de publicación en Sekai MCP
└── README.md
```

---

## 🚀 Cómo Ejecutar Localmente

Puedes abrir directamente cualquiera de los archivos `.html` en tu navegador, o levantar un servidor local rápido:

```bash
python3 -m http.server 8080
```
Luego abre `http://localhost:8080/index_light.html` o `http://localhost:8080/index_sekai.html`.

---

## 🛠️ Publicación en Sekai

El proyecto incluye soporte para el skill `sekai-publisher`:

```bash
# Empaquetar assets a WebP Base64:
python3 ~/.gemini/config/skills/sekai-publisher/scripts/pack_game.py index_light.html --assets textures --output index_sekai.html

# Subir a Sekai:
python3 ~/.gemini/config/skills/sekai-publisher/scripts/upload_game.py index_sekai.html --title "Perfect Sandwich" --game-id "9d52ea2a-a978-4372-8953-144ffb81e7ec"
```
