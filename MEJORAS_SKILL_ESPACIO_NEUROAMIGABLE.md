# Mejoras para la Skill espacio-neuroamigable

Basadas en optimización real del Mediterráneo Hotel & Spa en Samsung S10 y otros dispositivos móviles.

## Problemas identificados en la template actual

### 1. **Viewport meta tag insuficiente**
**Línea 5:**
```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

**Cambiar a:**
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
```

**Por qué:** Desabilita el zoom que obliga al usuario a hacer pinch, y activa fullscreen en iOS.

---

### 2. **Body no está optimizado para webapp nativa**
**Línea 27-35:**
```css
body{
  background:var(--greige);
  color:var(--tinta);
  font-family:'{{F_TXT}}',system-ui,sans-serif;
  font-weight:400;
  font-size:17px;
  line-height:1.62;
  -webkit-font-smoothing:antialiased;
}
```

**Cambiar a:**
```css
body{
  margin:0;padding:0;
  height:100vh;width:100%;
  position:fixed;
  overflow:hidden;
  background:var(--greige);
  color:var(--tinta);
  font-family:'{{F_TXT}}',system-ui,sans-serif;
  font-weight:400;
  font-size:clamp(16px,4.5vw,18px);
  line-height:1.62;
  -webkit-font-smoothing:antialiased;
  -webkit-text-size-adjust:100%;
  -webkit-user-select:none;
  user-select:none;
  -webkit-touch-callout:none;
}
```

**Por qué:** 
- `100vh fixed` bloquea el scroll de navegador (rebote elástico)
- `overflow:hidden` impide scrolling no deseado
- `clamp()` en font-size escala fluidamente sin media queries
- `-webkit-user-select:none` da sensación nativa

---

### 3. **Wrapper sin scroll interno**
**Línea 36:**
```css
.wrap{max-width:520px;margin:0 auto;padding-bottom:104px}
```

**Cambiar a:**
```css
.wrap{
  max-width:100%;
  margin:0;
  padding-bottom:150px;
  width:100%;
  height:100vh;
  overflow-y:auto;
  -webkit-overflow-scrolling:touch;
  position:absolute;
  top:0;
  left:0;
  right:0;
  bottom:0;
}
```

**Por qué:**
- `overflow-y:auto` en el contenedor permite scroll suave
- `-webkit-overflow-scrolling:touch` = scroll acelerado por GPU en iOS
- `height:100vh` + `position:absolute` aseguran que llene toda la pantalla
- `max-width:100%` usa todo el ancho disponible (elimina espacio en laterales)

---

### 4. **Tamaño de fuente base muy pequeño**
**Línea 32:** `font-size:17px`

**Cambiar a:** `font-size:clamp(16px,4.5vw,18px)` (ya en punto 2)

---

### 5. **Títulos (h1, h2) no responsivos**
**Línea 45-48 (h1):**
```css
.top h1{
  font-family:'{{F_TIT}}',serif;font-weight:600;font-size:31px;line-height:1.2;
  color:var(--greige);margin:0 0 16px;letter-spacing:.01em
}
```

**Cambiar a:**
```css
.top h1{
  font-family:'{{F_TIT}}',serif;font-weight:600;font-size:clamp(28px,8vw,36px);line-height:1.2;
  color:var(--greige);margin:0 0 18px;letter-spacing:.01em
}
```

**Línea 66-69 (h2):**
```css
h2{
  font-family:'{{F_TIT}}',serif;font-weight:600;font-size:26px;line-height:1.25;
  margin:0 0 12px;color:var(--tinta)
}
```

**Cambiar a:**
```css
h2{
  font-family:'{{F_TIT}}',serif;font-weight:600;font-size:clamp(24px,7vw,32px);line-height:1.25;
  margin:0 0 14px;color:var(--tinta)
}
```

---

### 6. **Párrafos con tamaño fijo**
**Línea 70:**
```css
p{margin:0 0 14px;color:var(--tinta-2)}
```

**Cambiar a:**
```css
p{
  margin:0 0 14px;
  color:var(--tinta-2);
  font-size:clamp(16px,4.5vw,18px)
}
```

---

### 7. **Barra fija insuficientemente posicionada**
**Línea 147-150:**
```css
.barra{
  position:fixed;left:0;right:0;bottom:0;z-index:40;
  background:linear-gradient(to top, var(--greige) 66%, rgba(234,228,220,0));
  padding:16px 24px calc(18px + env(safe-area-inset-bottom));
```

**Cambiar a:**
```css
.barra{
  position:fixed;left:0;right:0;bottom:0;z-index:50;
  background:linear-gradient(to top, var(--greige) 60%, rgba(234,228,220,.8) 100%);
  padding:18px 14px calc(20px + env(safe-area-inset-bottom));
  backdrop-filter:blur(4px);
  width:100%
}
```

**Por qué:** 
- `z-index:50` > `.wrap` para que no sea cubierto
- `backdrop-filter` mejora legibilidad
- `width:100%` explícito para Mobile Safari

---

### 8. **Padding lateral muy generoso**
**Línea 39 (.top):** `padding:30px 24px 26px`
**Línea 61 (section):** `padding:34px 24px 0`

**Cambiar a:**
```css
.top{background:var(--marron);padding:28px 16px 24px;text-align:center}
section{padding:28px 14px 0}
```

**Por qué:** Reduce espacio lateral sin perder proporciones. En pantallas pequeñas, 24px es demasiado.

---

### 9. **Grid de pictogramas fijo en 3 columnas**
**Línea 103:**
```css
.grid{display:grid;grid-template-columns:repeat(3,1fr);gap:9px;margin-top:16px}
```

**Cambiar a:**
```css
.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(85px,1fr));
  gap:8px;
  margin-top:14px
}
```

**Por qué:** Se adapta automáticamente a 2, 3, 4 columnas según ancho. En 375px son 3, en 360px son 2.

---

### 10. **Botones sin altura mínima**
**Línea 139-142:**
```css
.btn{
  display:block;width:100%;border:none;border-radius:var(--r);
  font:inherit;font-weight:500;font-size:16px;padding:15px;cursor:pointer;text-align:center
}
```

**Cambiar a:**
```css
.btn{
  display:flex;
  align-items:center;
  justify-content:center;
  width:100%;
  border:none;
  border-radius:var(--r);
  font:inherit;
  font-weight:500;
  font-size:clamp(15px,4.5vw,17px);
  padding:15px 14px;
  cursor:pointer;
  text-align:center;
  min-height:48px
}
```

**Por qué:** min-height:48px es estándar de accesibilidad táctil en iOS/Android.

---

## Resumen de cambios necesarios

| Elemento | Cambio | Impacto |
|---|---|---|
| Viewport meta | Desabilita zoom + fullscreen iOS | **Crítico** |
| Body | 100vh fixed + no select | **Crítico** |
| .wrap | Scroll interno + max-width:100% | **Crítico** |
| Font-size | clamp() fluido | **Alto** |
| Padding lateral | 24px → 14-16px | **Alto** |
| z-index .barra | 40 → 50 | **Medio** |
| Grid | auto-fit en lugar de repeat(3) | **Bajo** |

## Testing recomendado

1. **Samsung S10 real** (360x800px)
2. **iPhone 12 Mini** (375x812px)
3. **iPad Mini** (768x1024px)
4. Verificar que **no hay zoom forzado**
5. Verificar que **el scroll es fluido**
6. Verificar que **los botones son tocables** (>48px)

## Archivo a actualizar

- `scripts/_landing_template.html` (líneas 1-200+)

El script `generar_landing.py` no necesita cambios, solo la template.
