# DAM Web Components Library

## Introducció

Aquest repositori conté una col·lecció de **Web Components** desenvolupats de propòsit general. Aquesta llibreria està dissenyada per ser modular i reutilitzable, implementant els **principis de composició i encapsulació** dels Web Components.

* **Característiques**:
  - Components modulars basats en **Custom Elements**.
  - Estils encapsulats mitjançant **Shadow DOM**.
  - **Slots** per permetre la injecció de contingut personalitzat.
  - **Plantilles reutilitzables** per optimitzar el renderitzat.


## **Instal·lació i ús**

Per utilitzar aquests components en el teu projecte, has d'importar els fitxers JavaScript corresponents:

```html
<script type="module" src="./js/libcomponents/base_component.js"></script>
<script type="module" src="./js/libcomponents/scaffold_component.js"></script>
<script type="module" src="./js/libcomponents/tab_component.js"></script>
<script type="module" src="./js/components/pizzaCard.js"></script>
```

### **Ús de slots**

Els components que anem a definir fan ús de *slots* per personalitzar el contingut. 

Quan definim un Web Component podem definir dins el Shadow DOM *slots*, que serveixen per indicar el punt on s'injectarà el cotingut que s'especifique des de fora. 

Per exemple, si dins un webComponent (`ElMeuComponent`) definim el ShadowRoot així:

```js
this.shadowRoot.innerHTML = `
    <style>
        ...
    </style>
    <div class="class1">
        <slot name="slot1"></slot>
    </div>
    <div class="class2">
        <slot name="slot2"></slot>
    </div>
`;
```

Estem definint dos slots on afegit elements quan utilitzem el component. Concretament `slot1` i `slot2`. Això significa que es reemplaçaran pel que afegim com a `slot1` i com a `slot2`.

És a dir, podriem fer:

```html
<el-meu-component>
    <p slot="slot1">Aquest paràgraf va a l'slot 1</p>
    <p slot="slot2">Aquest paràgraf va a l'slot 2</p>
</el-meu-component>
```
De manera que el primer slot es reemplaça pel primer paràgraf i el segon pel segon.

Una vegada renderitzat, el veuria al DOM així:
```html
<el-meu-component>
    #shadow-root (open)
    <style>
        ...
    </style>
    <div class="class1">
        <p>Aquest paràgraf va a l'slot 1</p> <!-- Contingut injectat al slot1 -->
    </div>
    <div class="class2">
        <p>Aquest paràgraf va a l'slot 2</p> <!-- Contingut injectat al slot2 -->
    </div>
</el-meu-component>
</html>
```

En cas de no propocionar cap contingut, podem incorporar contingut predeterminat. Per exemple:

```js
this.shadowRoot.innerHTML = `
    <style>
        ...
    </style>
    <div class="class1">
        <slot name="slot1">Contingut Predeterminat dins l'slot1</slot>
    </div>
    <div class="class2">
        <slot name="slot2">Contingut Predeterminat dins l'slot2</slot>
    </div>
`;
```


Aquí tens la continuació del README, documentant detalladament els **tres components principals** i els seus **slots** per facilitar la personalització.

## **Components disponibles**

### **1BaseComponent**

El component **BaseComponent** és la base per a tots els altres Web Components. Defineix estils globals i proporciona una estructura comuna.

#### **Característiques**

- Proporciona una configuració bàsica per a colors, tipografia i estils.
- Tots els components que hereten d’aquest segueixen els estils i estructures definides.


#### Creació de nous components

Per tal de crear nous components, en lloc d'heretar d'HTMLElement, heretarem directament de `BaseComponent`. En aquest cas, a més, caldrà importar també la classe `BaseComponent`:

```js
import { BaseComponent } from './base_component.js'

export class NouComponent extends BaseComponent {...}
```


#### **🔹 Estils globals definits**

El **BaseComponent** inclou variables CSS reutilitzables:

```css
:host {
    font-family: 'Arial', sans-serif;
    --primary-color: #6200ea;
    --secondary-color: #03dac6;
    --text-color: #333;
    --background-color: #fff;
}
```

* **📌 Notes:**

- Qualsevol component que herete de `BaseComponent` **no necessita definir el seu Shadow DOM**, ja que ja està definit en el component base.
- Es poden utilitzar variables CSS per personalitzar l’aparença en components derivats.

### **ScaffoldComponent**

El **ScaffoldComponent** proporciona l’estructura general de l’aplicació, incloent-hi una **capçalera** i una **zona de contingut principal**.

#### **Slots disponibles**

Aquest component conté **dos slots** per permetre la personalització:

| Nom del Slot | Ús |
|-------------|--------------------------|
| `header` | Zona de la capçalera |
| `content` | Contingut principal |

#### **Exemple d’ús**

```html
<scaffold-component>
    <h2 slot="header">Benvingut a la Pizzeria</h2>
    <div slot="content">
        <p>Aquesta és una aplicació amb Web Components.</p>
    </div>
</scaffold-component>
```

#### **Renderitzat al DOM**

El component **ScaffoldComponent** injecta els elements dels slots dins del **Shadow DOM**, quedant així:

```html
<scaffold-component>
    #shadow-root (open)
    <style>...</style>
    <header>
        <h2>Benvingut a la Pizzeria</h2> <!-- Contingut injectat al slot "header" -->
    </header>
    <main>
        <p>Aquesta és una aplicació amb Web Components.</p> <!-- Contingut injectat al slot "content" -->
    </main>
</scaffold-component>
```

### **TabComponent**

El **TabComponent** proporciona un sistema de pestanyes navegables. Els usuaris poden definir **botons per a cada pestanya** i el **contingut associat**.

#### **Slots disponibles**

| Nom del Slot | Ús |
|-------------|---------------------------|
| `tabs` | Conté els botons de les pestanyes |
| `contents` | Conté el contingut de cada pestanya |

#### **🔹 Exemple d’ús**
```html
<tab-component>
    <button slot="tabs" class="tab">Pizzes</button>
    <button slot="tabs" class="tab">Entrants</button>

    <div slot="contents">
        <div id="llista-pizzes">
            <p>Aquí es mostren les pizzes disponibles.</p>
        </div>
        <div id="llista-entrants">
            <p>Aquí es mostren els entrants disponibles.</p>
        </div>
    </div>
</tab-component>
```

#### **Funcionament**

- Els botons dins del **slot `tabs`** actuen com a pestanyes.
- El contingut dins del **slot `contents`** es mostra segons la pestanya seleccionada.
- Només un contingut està visible a la vegada.

#### **Renderitzat al DOM**

El DOM, una vegada renderitzat queda aixì:

```html
<tab-component>
    #shadow-root (open)
    <style>...</style>
    <div class="tabs">
        <button class="tab active">Pizzes</button>
        <button class="tab">Entrants</button>
    </div>
    <div class="content">
        <div style="display: block;">Aquí es mostren les pizzes disponibles.</div>
        <div style="display: none;">Aquí es mostren els entrants disponibles.</div>
    </div>
</tab-component>
```
