# 🍽️ Demo Restaurante condor.ai — REFERENCIA para adaptar a nuevos clientes

> **Para Claude Code:** Este repo es una **plantilla de referencia**. Tu trabajo es **adaptarla** a un restaurante específico que te indicará el usuario (con su logo, info y fotos de platos). Mantén la MISMA estructura, animaciones y nivel de calidad; solo cambia el contenido. Lee TODO el `index.html` y `menu.html` antes de tocar nada, para entender el patrón.

---

## Qué es esto

Una landing page premium para restaurantes (demo original: "La Causa de Don Lucho", comida criolla peruana en Barranco). Sirve como **referencia visual y de código** para crear réplicas para otros restaurantes, rápido y con la misma calidad.

## Estructura del proyecto

```
demo-restaurante/
├── index.html        ← Landing principal (nav, hero, historia, platos con video, cómo llegar)
├── menu.html         ← La carta completa (categorías + platos + precios)
├── assets/
│   ├── logo.png      ← Logo del restaurante
│   ├── barra.jpg, cocina.jpg, mesa.jpg, patio.jpg, salon.jpg, musica.jpg  ← Fotos del local
│   └── README.md
├── videos/
│   ├── PROMPTS.md    ← ⭐ Los prompts de Higgsfield usados para generar cada video de plato
│   ├── ceviche.mp4, lomo-saltado.mp4, ...  ← 1 video vertical 9:16 por plato/cóctel
│   └── README.md
└── CLAUDE.md         ← este archivo
```

## Stack
- **HTML + CSS + JS puro** (todo inline en cada archivo, sin frameworks ni build). Se edita directo y se publica en GitHub Pages.
- Fuentes: Playfair Display (títulos) + Lato (cuerpo).
- Los videos de platos son verticales 9:16, se muestran en la sección "Platos" y la carta los detecta por nombre de archivo.

---

## 🛠️ Cómo adaptar a un restaurante nuevo (flujo recomendado)

El usuario te dará una carpeta con el **logo, la info del restaurante y fotos de sus platos**. Pasos:

1. **Lee primero** `index.html` y `menu.html` completos para entender el patrón (secciones, clases CSS, animaciones).
2. **Reemplaza el contenido de texto** en `index.html` y `menu.html`:
   - Nombre del restaurante (busca "La Causa de Don Lucho" / "Don Lucho" y reemplaza en todos lados, incluido `<title>`, Open Graph, nav, footer).
   - Historia, descripción, ciudad/dirección, horarios.
   - **WhatsApp:** busca el número `51998877665` y el texto de reserva, reemplázalos por los del cliente (ojo con el prefijo país: Perú `51`, Chile `56`).
   - Link de Google Maps en "Cómo llegar".
3. **Reemplaza el logo:** `assets/logo.png` por el del cliente (mismo nombre o ajusta la ruta).
4. **Reemplaza las fotos del local** en `assets/` (barra, salon, etc.) por las del cliente, o quita las secciones que no apliquen.
5. **Los platos:** cada restaurante tiene distinta cantidad. **Ajusta el HTML** — agrega o quita tarjetas de plato según corresponda. No fuerces 8 si tiene 5, ni dejes 8 si tiene 12.
6. **Genera los videos de los platos** con el CLI de Higgsfield (ver abajo) a partir de las fotos del cliente.
7. **Verifica** que todos los links (WhatsApp, carta, Maps, anclas) lleven a algo y que nada quede con datos del demo viejo.

> ⚠️ Importante: **la estructura, las animaciones y el estilo se mantienen**. Lo que cambia es el contenido (textos, logo, fotos, videos, cantidad de platos). El objetivo es que cada réplica se vea tan premium como el demo.

---

## 🎬 Generar imágenes/videos de platos con Higgsfield CLI

El usuario te pasará **fotos de los platos del cliente**. Para los videos, usa el CLI de Higgsfield (ya autenticado en la máquina del usuario). Mira `videos/PROMPTS.md` como referencia del estilo de prompt que funciona.

**Verifica primero que esté autenticado:**
```bash
higgsfield account status
```
Si dice "Session expired", el usuario debe correr `higgsfield auth login` (es interactivo, lo hace él).

**Generar un video de un plato a partir de su foto** (image-to-video, vertical 9:16, ~5s):
```bash
higgsfield generate create kling3_0 \
  --prompt "Cinematic macro food video of [PLATO], glossy, steam rising, shallow depth of field, slow push-in, warm appetizing lighting, 4K food commercial, vertical 9:16" \
  --start-image "ruta/a/la/foto-del-plato.jpg" \
  --aspect_ratio 9:16 --duration 5 --mode pro \
  --wait --wait-timeout 15m
```
- El comando devuelve una URL; descarga el archivo y guárdalo en `videos/` con el nombre del plato (ej. `ceviche.mp4`).
- **Consulta el costo antes** con `higgsfield generate cost kling3_0 ...` si quieres estimar créditos.
- Modelos útiles: `kling3_0` (buen costo, hasta 15s), `seedance_2_0` (más caro, con audio). Solo imagen: `nano_banana_2`.

**Reutiliza los prompts probados** de `videos/PROMPTS.md` — ya están afinados para comida apetitosa; solo cambia el plato.

---

## ✅ Checklist antes de entregar una réplica
- [ ] Ningún texto/nombre/teléfono del demo viejo ("Don Lucho", `51998877665`) quedó suelto.
- [ ] Logo, fotos y videos son del cliente nuevo.
- [ ] Cantidad de platos coincide con la del restaurante real.
- [ ] WhatsApp con el prefijo de país correcto y mensaje de reserva del cliente.
- [ ] Google Maps apunta a la dirección real.
- [ ] Se ve bien en celular (es mobile-first).
- [ ] Todos los links llevan a algo (carta, anclas, WhatsApp).
