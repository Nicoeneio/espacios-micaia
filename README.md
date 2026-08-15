# Espacios Neuroamigables — Deploy

Sitio de landings para espacios neuroamigables, deployado en Vercel en `espacios.eneio.online`.

## Estructura

```
/mediterraneo/index.html    Landing del Mediterráneo Hotel & Spa
/vercel.json                Redirects + headers
/qr-mediterraneo.png        QR → https://espacios.eneio.online/e/med
```

## Deploy en Vercel

1. Crear un nuevo proyecto en Vercel importando este repo
2. Framework Preset: **Other**
3. Settings → Domains → agregar `espacios.eneio.online`
4. En DNS de `eneio.online`, crear:
   ```
   CNAME  espacios  →  cname.vercel-dns.com
   ```
5. Verificar que `https://espacios.eneio.online/e/med` abre la landing

## Agregar un cliente nuevo

1. Crear carpeta `/<cliente>/index.html` con la landing generada
2. Agregar redirect en `vercel.json`:
   ```json
   { "source": "/e/<slug>", "destination": "/<cliente>", "permanent": false }
   ```
3. Generar QR apuntando a la URL corta `/e/<slug>`

## Reglas importantes

- **Redirects siempre con `"permanent": false`** (302, no 301). Un 301 queda cacheado en navegador.
- **QR impreso siempre a la URL corta** `/e/<slug>`, nunca a la ruta final.
- **`noindex` mientras estén en demostración.** Sacarlo cuando cliente confirme contenido.
- **Sin analytics, cookies ni píxeles.** Si hace falta medir, hacerlo agregado y anónimo del lado del servidor.
