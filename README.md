# Menú de Molly Coffee

Sitio estático con el menú de Molly Coffee, pensado para abrirse escaneando un
código QR desde el móvil.

El flujo es: se escanea el QR → se ve **la imagen** de la carta → desde ahí se
puede pasar al **menú en texto**, que al final tiene un botón para volver a la
imagen.

```
index.html        la imagen de la carta (lo que abre el QR)
menu.html         el menú en texto, con los alérgenos enlazados
cartel-qr.html    cartel A5 imprimible (el QR apunta a /)
menu-imagen.html  redirección a / (dirección antigua)
v2/index.html     redirección a / (dirección antigua)
assets/           imagen del menú (WebP + JPEG), PDF original y el QR
```

Hay **un solo código QR**, en `assets/qr-menu.svg` y `.png`. Codifica la URL, no
el contenido: se pueden cambiar precios, textos o incluso qué página se sirve en
`/` sin tener que reimprimir nada.

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
url = 'https://oscarla0.github.io/molly-coffee-menu/'
qr = segno.make(url, error='h')
qr.save('assets/qr-menu.svg', scale=10, border=2, dark='#1F1A17', light=None)
qr.save('assets/qr-menu.png', scale=24, border=3, dark='#1F1A17', light='#FFFFFF')
"
```

El cartel lleva el QR **incrustado en SVG**, así que tras regenerarlo hay que
volver a incrustarlo en `cartel-qr.html`.

## Fuentes

Los títulos usan Archivo Black y el cuerpo Spectral, cargadas desde Google
Fonts. Sin conexión caen a Arial Black y Georgia, que están en casi cualquier
equipo: la página sigue siendo legible, solo cambia la tipografía.

## Sobre los alérgenos

Los alérgenos están transcritos del PDF original del menú. Es información con
obligación legal (Reglamento UE 1169/2011): conviene revisarlos contra la carta
oficial antes de poner el QR a disposición del público.
