# Convenciones BEM — Espacios Magníficos

Guía para mantener el CSS del sitio ordenado y consistente. El proyecto ya sigue BEM; este documento explica las reglas y deja el inventario de bloques existentes para que cualquiera que agregue una sección nueva use el mismo patrón.

## 1. Qué es BEM

**B**loque, **E**lemento, **M**odificador. Es una forma de nombrar clases CSS para que, con solo leer el nombre, sepas qué es cada cosa y de qué depende.

```
.bloque { }
.bloque__elemento { }
.bloque--modificador { }
.bloque__elemento--modificador { }
```

| Parte | Qué es | Separador | Ejemplo en el proyecto |
|---|---|---|---|
| **Bloque** | Un componente independiente y reutilizable | — | `.feature-card`, `.pricing-card`, `.hero` |
| **Elemento** | Una pieza que solo existe dentro de su bloque | `__` (doble guion bajo) | `.feature-card__title`, `.pricing-card__price` |
| **Modificador** | Una variación de apariencia o estado de un bloque o elemento | `--` (doble guion medio) | `.feature-card--highlight`, `.nav__link--active` |

## 2. Reglas del proyecto

1. **Un bloque no sabe nada de dónde lo pongas.** `.pricing-card` debe verse igual dentro de `#precios` que si lo movieras a otra sección. Por eso el posicionamiento (`grid`, `margin` entre secciones) va en el bloque contenedor (`.pricing__grid`), nunca en el bloque hijo.
2. **Un elemento nunca se usa solo.** `.feature-card__title` no debe aparecer fuera de un `.feature-card`. Si una pieza se repite en varios bloques distintos, probablemente debería ser su propio bloque (como pasó con `.badge`, que se reutiliza dentro de `.hero__panel-row`, `.feature-card` y `.pricing-card`).
3. **Los elementos no se anidan en el nombre.** Es `.hero__panel-row`, no `.hero__panel__row`. Un bloque solo tiene un nivel de `__`, aunque el HTML esté anidado varios niveles.
4. **Los modificadores solo cambian variaciones, no estructura.** `--highlight`, `--featured`, `--active`, `--blocked` cambian color, tamaño o estado — nunca agregan un elemento que los demás no tengan.
5. **Minúsculas y guiones**, nunca camelCase ni guion bajo simple: `site-footer`, no `siteFooter` ni `site_footer`.
6. **Bloques reutilizables usan nombre genérico** (`.badge`, `.btn`, `.field`), no el nombre de la sección donde los viste primero.

## 3. Inventario de bloques actuales

| Bloque | Dónde se usa | Elementos principales | Modificadores |
|---|---|---|---|
| `.site-header` | Encabezado fijo | `__inner` | — |
| `.logo` | Marca, en header y footer | `__mark`, `__word` | `--on-dark` |
| `.nav` | Menú de navegación | `__list`, `__item`, `__link`, `__cta` | `--active` (en `__link`) |
| `.nav-toggle` | Botón de menú hamburguesa (móvil) | `__btn`, `__bar` | — |
| `.hero` | Sección de bienvenida | `__inner`, `__content`, `__title`, `__subtitle`, `__actions`, `__visual`, `__panel`, `__panel-header`, `__panel-list`, `__panel-row`, `__panel-time`, `__panel-event`, `__stat`, `__stat-number`, `__stat-label`, `__backdrop`, `__pattern` | `--blocked` (en `__panel-row`) |
| `.btn` | Cualquier botón o enlace con pinta de botón | — | `--primary`, `--ghost`, `--outline`, `--large`, `--small`, `--block` |
| `.badge` | Etiquetas pequeñas de estado | — | `--accent` |
| `.section-heading` | Encabezado repetido en cada sección | `__title`, `__subtitle` | `--on-tint` |
| `.features` | Sección "Qué ofrecemos" | `__grid` | — |
| `.feature-card` | Cada tarjeta de servicio | `__icon`, `__title`, `__description`, `__tag` | `--highlight` |
| `.testimonials` | Sección de clientes | `__grid` | — |
| `.testimonial-card` | Cada cita de cliente | `__quote`, `__footer`, `__avatar`, `__identity`, `__name`, `__role` | `--highlight` |
| `.pricing` | Sección de paquetes | `__grid` | — |
| `.pricing-card` | Cada paquete | `__name`, `__price`, `__amount`, `__period`, `__description`, `__features`, `__feature`, `__tag` | `--featured` |
| `.contact` | Sección de contacto | `__inner`, `__intro`, `__title`, `__text` | — |
| `.contact-form` | Formulario de cotización | `__row`, `__actions`, `__hint` | — |
| `.field` | Cada campo del formulario | `__label`, `__control`, `__error` | `--textarea` (en `__control`) |
| `.site-footer` | Pie de página | `__inner`, `__brand`, `__tagline`, `__nav`, `__heading`, `__link`, `__contact`, `__social`, `__bottom`, `__copy` | — |
| `.social-list` | Íconos de redes sociales | `__item`, `__link` | — |

## 4. Cómo agregar una sección nueva (ejemplo)

Si mañana agregas, por ejemplo, una sección de "Preguntas frecuentes":

```html
<section class="faq" id="preguntas">
  <div class="section-heading">
    <h2 class="section-heading__title">Preguntas frecuentes</h2>
  </div>

  <div class="faq__list">
    <article class="faq-item">
      <h3 class="faq-item__question">¿Con cuánta anticipación debo reservar?</h3>
      <p class="faq-item__answer">Lo ideal es 15 días antes para recintos grandes.</p>
    </article>

    <article class="faq-item faq-item--open">
      <h3 class="faq-item__question">¿Incluyen el montaje?</h3>
      <p class="faq-item__answer">Sí, en el paquete Evento completo y A la medida.</p>
    </article>
  </div>
</section>
```

```css
.faq__list {
  display: grid;
  gap: 1rem;
}

.faq-item {
  border: 1px solid var(--color-border);
  border-radius: 0.75rem;
  padding: 1.25rem;
}

.faq-item__question {
  font-weight: 600;
}

.faq-item--open {
  border-color: var(--color-accent);
}
```

Nota que `.faq` es el bloque de la sección (como `.features` o `.pricing`) y `.faq-item` es un bloque aparte, no un elemento de `.faq`, porque cada pregunta es un componente con su propia identidad (igual que `.feature-card` no es `.features__card`). Esta es la misma lógica que ya sigue el resto del proyecto.

## 5. Errores comunes a evitar

- ❌ `.faq__item__question` (doble `__`, anidamiento de elementos)
- ✅ `.faq-item__question`
- ❌ `.btn-primary-large` (modificadores pegados sin separador)
- ✅ `.btn.btn--primary.btn--large` (varias clases, cada una su propio modificador)
- ❌ `.hero .title` (depender de descendencia en vez de una clase propia)
- ✅ `.hero__title`
- ❌ `.pricing-card-featured` (un solo guion en vez de `--`)
- ✅ `.pricing-card--featured`
