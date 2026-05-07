# DonarOnline — CSS Custom Casa Ronald McDonald Colombia

Hojas de estilo personalizadas para el formulario multi-pasos de DonarOnline,
servidas vía jsDelivr y consumidas como `?custom_css=<base64>` en el iframe.

---

## 📁 Estructura

| Archivo | Uso |
|---|---|
| `css/boilerplate.css` | **Template maestro** — punto de partida para nuevas campañas. Documentado, con variables al tope. |
| `css/fcrm-oficial-v2-sos.css` | **CSS en producción** (campaña SOS). |

---

## ✏️ Cómo crear una nueva campaña

1. **Duplicar el boilerplate**
   ```bash
   cp css/boilerplate.css css/fcrm-<nombre-campaña>.css
   ```

2. **Editar SOLO el bloque `:root`** del nuevo archivo:
   - `--main-color` → color de marca (botones, focus, montos seleccionados)
   - `--bg-color` → fondo del formulario
   - `--msg-step-1/2/3` → mensajes de cada paso
   - `--btn-step-1/2/3` → texto de los botones por paso

3. **Commit + push a `main`**
   ```bash
   git add css/fcrm-<nombre-campaña>.css
   git commit -m "feat(css): nueva campaña <nombre>"
   git push
   ```

4. **Tomar el commit hash corto** (`git rev-parse --short HEAD`)

5. **Generar el Base64** de la URL del CDN:
   ```bash
   echo -n "https://cdn.jsdelivr.net/gh/licencias-marinovich/donaronline-custom-css@<HASH>/css/fcrm-<nombre>.css" | base64
   ```

6. **Pegar el Base64** en el iframe como `?custom_css=<BASE64>`

---

## 🐛 Bugs conocidos resueltos en boilerplate

- ❌ **Antes:** `body` y todos los descendientes de `.dsf` forzados a `transparent !important` → no se podía aplicar fondo.
  ✅ **Ahora:** variable `--bg-color` controla fondo. Sin overrides masivos a transparent.

- ❌ **Antes:** Mensajes por paso usaban `:has(.dsf__donor)` y `:has(.dsf__payment)` — clases que no siempre existen según la estructura de DonarOnline.
  ✅ **Ahora:** Usa las clases oficiales `.dsf__step-{N}-container` documentadas por DonarOnline, con fallback a `.dsf__payment` y `.dsf__card-data` por compatibilidad.

---

## 🔗 Referencias

- [Docs oficiales DonarOnline — Estilos](https://docs.donaronline.org/formulario-multi-pasos/guias/estilos)
- jsDelivr CDN: `https://cdn.jsdelivr.net/gh/licencias-marinovich/donaronline-custom-css@<HASH>/css/<archivo>.css`

---

## ⚠️ Importante sobre cache

jsDelivr cachea por commit hash. Si editas un CSS sin cambiar el hash en la URL Base64, **el viejo seguirá sirviéndose**. Siempre regenera el Base64 con el hash nuevo después de cada cambio.
