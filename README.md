# DAM Web Components Library

## Introducció

Aquest repositori conté una col·lecció de **Web Components** desenvolupats de propòsit general. Aquesta llibreria està dissenyada per ser modular i reutilitzable, implementant els **principis de composició i encapsulació** dels Web Components.

* **Característiques**:
  - Components modulars basats en **Custom Elements**.
  - Estils encapsulats mitjançant **Shadow DOM**.
  - **Slots** per permetre la injecció de contingut personalitzat.
  - **Plantilles reutilitzables** per optimitzar el renderitzat.


## 🏗️ **Instal·lació i ús**

Per utilitzar aquests components en el teu projecte, has d'importar els fitxers JavaScript corresponents:

```html
<script type="module" src="./js/libcomponents/base_component.js"></script>
<script type="module" src="./js/libcomponents/scaffold_component.js"></script>
<script type="module" src="./js/libcomponents/tab_component.js"></script>
<script type="module" src="./js/components/pizzaCard.js"></script>
```

---

## 📜 **Estructura del projecte**

```plaintext
/
│── index.html
│── js/
│   ├── main.js
│   ├── components/
│   │   ├── components personalitzats...
│   ├── libcomponents/
│   │   ├── base_component.js
│   │   ├── scaffold_component.js
│   │   ├── tab_component.js
│   ├── models/
│   │   ├──...
│   ├── services/
│   │   ├── ...
```

---

## **Components Disponibles**

### 1️ **BaseComponent**

Component base que proporciona **estils globals** per a la resta de components.

🔹 **Característiques**:

- Defineix variables CSS globals.
- Utilitza `box-sizing: border-box` per facilitar la gestió d'estils.
- Serveix com a base per a altres components.

📌 **Exemple d'ús**:
```js
import { BaseComponent } from './base_component.js';

class NouComponent extends BaseComponent {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
        this.shadowRoot.innerHTML = `<p>Hola des de NouComponent!</p>`;
    }
}
```

customElements.define('nou-component', NouComponent);

### 2 **ScaffoldComponent**

Component que proporciona **l'estructura base** de l'aplicació, permetent afegir una capçalera i contingut dinàmic.

* **Característiques**:

- Utilitza **slots** per injectar contingut personalitzat.
- Gestiona l'estructura de l'aplicació.

* **Exemple d'ús**:

```html
<scaffold-component>
    <h2 slot="header">Benvingut a la Pizzeria</h2>
    <div slot="content">
        <p>Aquesta és una aplicació amb Web Components.</p>
    </div>
</scaffold-component>
```

### **TabComponent**

Component que gestiona la navegació per pestanyes de manera dinàmica.

* **Característiques**:

- Utilitza **slots** per gestionar les pestanyes i el seu contingut.
- Estilitza i gestiona la interacció amb JavaScript.

* **Exemple d'ús**:

```html
<tab-component slot="content">
    <button slot="tabs" class="tab">Pizzes</button>
    <button slot="tabs" class="tab">Entrants</button>

    <div slot="contents">
        <div id="llista-pizzes">
            <!-- Pizzes aquí -->
        </div>
        <div id="llista-entrants">
            <!-- Entrants aquí -->
        </div>
    </div>
</tab-component>
```

## **Conceptes Clau: Slots i Plantilles**

### **Slots**

Els **slots** permeten als Web Components acceptar contingut personalitzat. En el nostre projecte, s'utilitzen per injectar contingut a `ScaffoldComponent` i `TabComponent`.

**Exemple d'ús de slots**:

```html
<scaffold-component>
    <h2 slot="header">Capçalera Personalitzada</h2>
    <div slot="content">Contingut dinàmic aquí</div>
</scaffold-component>
```

Això s'insereix dins del component definit com:

```js
this.shadowRoot.innerHTML = `
    <slot name="header"></slot>
    <div class="content">
        <slot name="content"></slot>
    </div>
`;
```

### **Plantilles (Templates)**

Les plantilles permeten definir HTML reutilitzable i optimitzar el rendiment dels components.

 **Exemple d'ús amb plantilla**:

```js
const template = document.createElement('template');
template.innerHTML = `
    <style>
        .targeta {
            border: 1px solid #ddd;
            padding: 15px;
            border-radius: 10px;
            background-color: #f9f9f9;
        }
    </style>
    <div class="targeta">
        <slot name="nom"></slot>
        <slot name="descripcio"></slot>
    </div>
`;
```
Això després es clona dins del `shadowRoot`:
```js
this.shadowRoot.appendChild(template.content.cloneNode(true));
```