# Menú de Molly Coffee

Sitio estático con el menú de Molly Coffee, pensado para abrirse escaneando un
código QR desde el móvil.

- `index.html` — el menú en texto (mobile-first, con alérgenos enlazados)
- `menu-imagen.html` — el menú original en imagen, con zoom y descarga del PDF
- `cartel-qr.html` — cartel A5 imprimible con el código QR
- `assets/` — imagen del menú (WebP + JPEG), el PDF original y el código QR

No hay build ni dependencias: son archivos HTML con el CSS incrustado. Se pueden
abrir directamente con doble clic.

## Cambiar un precio o un producto

Todo está en `index.html`, en la sección correspondiente. Cada línea del menú
tiene esta forma:

```html
<li><div class="row">
  <span class="name">Cortado</span>
  <span class="algs"><a class="alg" href="#a7" aria-label="Alérgeno: leche">7</a></span>
  <span class="leader"></span>
  <span class="price">2,50 €</span>
</div></li>
```

- El **nombre** va en `.name` y el **precio** en `.price`.
- Los **alérgenos** son las insignias `.alg`: el número visible, el `href="#aN"`
  y el `aria-label` deben coincidir con la entrada correspondiente de la tabla
  de alérgenos del final del archivo.
- Los iconos de **frío** y **caliente** son
  `<svg class="ic"><use href="#ic-cold"/></svg>` y `#ic-hot`.

## Volver a publicar

```sh
git add -A && git commit -m "Actualiza precios" && git push
```

GitHub Pages republica solo en menos de un minuto. El código QR no hay que
regenerarlo: apunta a la URL, no al contenido.

## Regenerar el código QR

Solo es necesario si cambia la URL del sitio:

```sh
python3 -m venv .venv && .venv/bin/pip install segno
.venv/bin/python -c "
import segno
u='https://USUARIO.github.io/molly-coffee-menu/'
segno.make(u, error='h').save('assets/qr-menu.svg', scale=10, border=2)
segno.make(u, error='h').save('assets/qr-menu.png', scale=24, border=2)
"
```

## Fuentes

Los títulos usan Archivo Black y el cuerpo Spectral, cargadas desde Google
Fonts. Sin conexión caen a Arial Black y Georgia, que están en casi cualquier
equipo: la página sigue siendo legible, solo cambia la tipografía.

## Sobre los alérgenos

Los alérgenos están transcritos del PDF original del menú. Es información con
obligación legal (Reglamento UE 1169/2011): conviene revisarlos contra la carta
oficial antes de poner el QR a disposición del público.
