# Espacios Neuroamigables — deploy

## Estructura

```
/mediterraneo/index.html    landing del Mediterráneo Hotel & Spa
/vercel.json                redirects + cabeceras
/qr-mediterraneo.png        QR → https://espacios.eneio.online/e/med
```

## Publicar

1. Subir esta carpeta a un repo de GitHub (`espacios-micaia`).
2. En Vercel: New Project → importar el repo → Framework Preset: **Other** → Deploy.
3. Settings → Domains → agregar `espacios.eneio.online`.
4. En el DNS de eneio.online, crear el registro que indique Vercel (normalmente
   `CNAME  espacios  →  cname.vercel-dns.com`).
5. Esperar la propagación y verificar que `https://espacios.eneio.online/e/med` abra la landing.

## Agregar un cliente nuevo

1. Crear la carpeta `/<cliente>/index.html` con la landing generada por la skill.
2. Agregar el redirect corto en `vercel.json`:
   `{ "source": "/e/<slug>", "destination": "/<cliente>", "permanent": false }`
3. Generar el QR apuntando a la URL corta.

## Reglas que no conviene romper

- Los redirects van con `"permanent": false` (302). Un 301 queda cacheado en el
  navegador para siempre y te impide mover la ruta más adelante.
- El QR impreso apunta **siempre** a la URL corta `/e/<slug>`, nunca a la ruta final.
- `noindex` mientras las landings estén en modo demostración. Sacarlo cuando los
  contenidos estén confirmados por el cliente.
- Sin analytics de terceros, sin cookies, sin píxeles. Si más adelante hace falta
  medir uso, que sea agregado y anónimo, del lado del servidor.
