---
layout: enviar
title: Control de enfoque con tabindex
authors:
- robdodson
date: 2018-11-18
description: Los elementos HTML nativos como <button> o <input> tienen accesibilidad de teclado incorporado de forma gratuita. Si está creando componentes interactivos personalizados, use tabindex para garantizar que sean accesibles con el teclado.
---

Los elementos HTML nativos como `<button>` o `<input>` tienen accesibilidad de teclado incorporado de forma gratuita. Si está creando componentes interactivos *personalizados* , use el atributo `tabindex` para garantizar que sean accesibles con el teclado.

{% Aside %} Siempre que sea posible, use un elemento HTML nativo en lugar de construir su propia versión personalizada `<button>` , por ejemplo, es muy fácil de diseñar y Ya tiene soporte completo de teclado. Esto le ahorrará la necesidad de administrar `tabindex` o para agregar semántica con ARIA. {% endAside %}

## Comprueba si tus controles son accesibles con el teclado

Una herramienta como Lighthouse es excelente para detectar ciertos problemas de accesibilidad, pero Algunas cosas solo pueden ser probadas por un humano.

Intente presionar la tecla `TAB` para navegar por su sitio. ¿Eres capaz de llegar todos los controles interactivos en la página? Si no, es posible que deba usar [`tabindex`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex) para mejorar la focalización de esos controles.

{% Aside 'warning' %} Si no ve un indicador de enfoque, puede estar oculto por su CSS Verifique cualquier estilo que mencione `:focus { outline: none; }` . Puede aprender cómo solucionar esto en nuestra guía sobre [enfoque de estilo](/style-focus) . {% endAside %}

## Insertar un elemento en el orden de tabulación

Inserte un elemento en el orden de tabulación natural usando `tabindex="0"` . Por ejemplo:

```html
<div tabindex="0">Focus me with the TAB key</div>
```

Para enfocar un elemento, presione la tecla `TAB` o llame al método `focus()` del elemento.

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/tabindex-zero?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-zero on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

## Eliminar un elemento del orden de tabulación

Eliminar un elemento usando `tabindex="-1"` . Por ejemplo:

```html
<button tabindex="-1">Can't reach me with the TAB key!</button>
```

Esto elimina un elemento del orden de tabulación natural, pero el elemento aún puede ser enfocado llamando a su método `focus()` .

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/tabindex-negative-one?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-negative-one on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

## Evitar `tabindex > 0`

Cualquier `tabindex` mayor que 0 salta el elemento al frente de la pestaña natural orden. Si hay varios elementos con un `tabindex` mayor que 0, la pestaña el orden comienza desde el valor más bajo mayor que cero y avanza hacia arriba.

Usar un `tabindex` mayor que 0 se considera un **antipatrón** porque los lectores de pantalla navegan la página en orden DOM, no en orden de tabulación. Si necesitas un para que el elemento aparezca antes en el orden de tabulación, se debe mover a un lugar anterior en el DOM.

Lighthouse facilita la identificación de elementos con un `tabindex` > 0. Ejecute el Auditoría de accesibilidad (Lighthouse> Opciones> Accesibilidad) y busque el resultados de la auditoría "Ningún elemento tiene un valor [tabindex] mayor que 0".

## Cree componentes accesibles con " `tabindex` itinerante"

Si está creando un componente complejo, es posible que deba agregar un teclado adicional apoyo más allá del foco. Considere el elemento de `select` nativo. Es enfocable y puede usar las teclas de flecha para exponer funcionalidades adicionales (la opción seleccionable opciones).

Para implementar una funcionalidad similar en sus propios componentes, use una técnica conocida como " `tabindex` itinerante". Tabindex móvil funciona estableciendo `tabindex` en -1 para todos los niños excepto el actualmente activo. El componente luego usa un teclado detector de eventos para determinar qué tecla ha presionado el usuario.

Cuando esto sucede, el componente establece el `tabindex` de `tabindex` del niño previamente enfocado a -1, establece el `tabindex` de `tabindex` del niño a `tabindex` en 0 y llama al `focus()` método sobre el mismo.

**antes de**

```html/2-3
<div role="toolbar">
  <button tabindex="-1">Undo</div>
  <button tabindex="0">Redo</div>
  <button tabindex="-1">Cut</div>
</div>
```

**Después**

```html/2-3
<div role="toolbar">
  <button tabindex="-1">Undo</div>
  <button tabindex="-1">Redo</div>
  <button tabindex="0">Cut</div>
</div>
```

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/roving-tabindex?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-negative-one on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

{% Aside %} ¿Curioso para qué sirven esos atributos `role=""` ? Te dejan cambiar el semántica de un elemento para que sea anunciado correctamente por un lector de pantalla. Puede obtener más información sobre ellos en nuestra guía sobre [Lo básico del lector de pantalla](/semantics-and-screen-readers) . {% endAside %}

## Recetas de acceso de teclado

Si no está seguro de qué nivel de soporte de teclado podrían tener sus componentes personalizados necesita, puede consultar el [Prácticas de autoría de ARIA 1.1](https://www.w3.org/TR/wai-aria-practices-1.1/) . Esta práctica guía enumera patrones comunes de UI e identifica qué teclas Los componentes deben ser compatibles.
