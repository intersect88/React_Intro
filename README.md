React

1. React overview
x. Typescript (é da inserire?)
2. Component Based Approach
3. Requirements
4. Virtual DOM & JSX/TSX Fundamentals
5. Styling
6. Events
7. Typed Function Components
8. Hooks: useStaste, useEffect
9. Dynamic List
10. Server Side Communication 
11. Typed Component & Props

React é una libreria javascript per la creazione di UI che puó essere utilizzato per: 
Spa e Multipage, web sites, Mobile ,Progressive Web App Fancy Layout (netflix),Add-in Office 365

Non é un framework ma bensí una libreria per la creazione di componenti

Powered function components 
si usano per lo piú funzioni che con gli ultimi aggiornamenti possono avere un lifecicle ed uno stato 
quindi non é necessario utilizzare le classi.

Hooks
Gestione dello stato e ciclo di vita dei componenti 

Nuove Api

**PERCHE` TYPESCRIPT ?**
Popolare 
static type system
cli integration 
esnext support 
soluzione sicura, solida e scalabile per applicazioni enterprise nel mondo web

**STATIC AND DYNAMIC TYPING**

Typescript usa un sistema di type checking statico ovvero che analizza
il codice prima della sua esecuzione ( a tempo di compilazione) quindi
prima che il codice arrivi su browser questo ci consente di rilevare
se ci sono errori. 

Nel type checking dinamico invece il codice viene processato in fase
di esecuzione quindi avremo l'errore direttamente sulla pagina del
browser

**COMPONENTS**

Un componente é un entitá che gestisce una piccola parte della nostra
applicazione. Per questo motivo ci consente di avere una SEPARATION OF
CONCERNS. 
I componenti possono essere riusabili e componibili tra loro.
Sono testabili.

**APPROCCIO COMPONENT BASE**

invece di
```html 
<div className="row">
    <div className="col">
```
possiamo usare questo approccio
```html
<App> //component
    <Navbar> //component
    <Row> //component
        <Col perc={30}> //component
            <Search/> //component
            <List/> //component
        </Col>
    </Row>
</App>
```

**PERCHÈ FUNZIONI E NON CLASSI**

Approccio moderno consigliato dal Team React in quanto il 
ciclo di vita della classe è molto più complesso.
Il ciclo di vita di una funzione invece si gestisce con un 
**hook** useEffect che gestisce inizializzazione, cambiamento e
destroy.
Gestione dello stato e del binding è più rognosa nelle classi.

**REQUISITI**

- Node.js (installare nvm)
- CLI: create-react-app (npm o npx) npx create-react-app demo --template typescript



**COM'È STRUTTURATO IL PROGETTO?**
-tsconfig.config - file di configurazione di typescript
- package.json - dipendenze del progetto e comandi
- public folder - template html di base
- nella cartella source:
    - serviceWorker.ts - per gestire le PWA e le applicazioni offline (es gestione cache)
    - setupTests.ts - per gli unit tests
    - react-app-env.d.ts - file di definizione delle variabili di ambiente soprattutto quelle custom

nell'index html ci sarà: 
```html
<div id="root"></div> 
```
che identifica il punto di partenza per la creazione della nostra applicazione

nell'index.tsx invece c'è un' API di react ReactDOM.render() che andra' a fare il render 
della nostra app (file app.tsx) in corrispondenza dell'elemento root dell'index.html 

**COMANDI**

- npm start ( lancia in modalita' sviluppo la nostra app)
- npm run build (crea una cartella di build che può essere uploadata su ftp sul sito oppure deploy automatico di CI/CD)

per provare la build ho bisogno di un http server e posso lanciare il comando:

    npm install serve (si potrebbe inserire nella devDependencies npm i serve -d)

    serve -s build (da aggiungere agli script all'interno del package.json )

**STRICT MODE**
Piccola parentesi su questa opzione che troviamo all'interno del file ts.config

Questa variabile fa in modo che vengano abilitate o meno un gruppo di funzionalità del linguaggio. Di particolare interesse ci sono:

- NoImplicitAny
- StrictNullChecks

NO IMPLICIT ANY

se definisco una variabile: 
 
 let counter; 

 e poi voglio utilizzarla mi verra'generato un errore a tempo di compilazione che ci segnala di non poter utilizzare
 una variabile che sia implicitamente anyt quindi dovrò esplicitarla (let counter : any;)
 o inizializzarla in modo che venga tipizzata per inferenza (proprietà di un linguaggio di dedurre il tipo della variabile in base al valore di inizializzazione)

 STRICT NULL CHECKS

interface User {
  name: string;
}

let user:User = {name: 'cacca'}

user = null;

In questo modo avrò un errore a tempo di compilazione
che mi dirà che non è possibile assegnare null alla variabile.
Per assegnare null devo esplicitare la dichiarazione che tale
variabile può essere null e questo posso farlo dichiarando in
questo modo: 

let user: User | null = {name: 'cacca'}

e successivamente usare la variabile in questo modo

{user?.name}
{user ? user.name : null}
{user && user.name}

**JSX**
è una sorta di estensione di javascript, 
per mimare un sistema di templating simile ad html 
ma ha la potenza di javascript.

In una funzione jsx posso ritornare anche template ed utilizzare quindi
la chiamata della funzione all'interno di altro codice JSX. 
Vediamo qualche esempio: 

const App = () => {
  return (
    <div>
      ciao
    </div>
  );
}

Un vincolo del jsx è quello di avere un nodo wrapper quindi
se voglio ritornare più ```<div>``` diversi dovrò farlo in uno dei modi
seguenti:

1) 
```js
const App = () => {
  return (
    <div>
        <div>
        ciao
        </div>
        <div>
        ciao
        </div>
    </div>
  );
}
```
Per fare in modo che non venga renderizzato il nodo wrapper posso usare:

2)
```js
const App = () => {
  return (
    <React.Fragment>
        <div>
        ciao
        </div>
        <div>
        ciao
        </div>
    </React.Fragment>
  );
}
```
```js
3) const App = () => {
  return (
    <>
        <div>
        ciao
        </div>
        <div>
        ciao
        </div>
    </>
  );
}
```
In Jsx posso anche utilizzare espressioni: 
esempio: 
```js
const App = () => {
  return (
    <div>
      {1+1+2}
    </div>
  );
}
```
o dichiarare variabili ed utilizzarle all'interno del 
sistema di templating
```js
const App = () => {

const hello = 'ciao';

  return (
    <div>
      {hello}
    </div>
  );
}
```
oppure 
```js
const App = () => {

  const products: number = 10;
  
  return (
    <div>
      {products === 0 ? <h3>Empty</h3> : <div>{products} products </div>}
    </div>
  );
}
```
Naturalmente se i valori da assegnare a products fossero composti da 
più codice, per semplificare potrei utilizzare due metodi o creare
due nuovi componenti.

Con metodi:
```js
const App = () => {

  const products: number = 10;

  const renderEmpty = () => <h3>Empty</h3>

  const renderProducts = () => <div>{products} products </div>

  return (
    <div>
      {products === 0 ? renderEmpty() : renderProducts()}
    </div>
  );
}
```
Creando due typed component: 
```js
const App = () => {

  const products: number = 10;
    
  return (
    <div>
      {products === 0 ? <Empty/> : <Products value={products} />}
    </div>
  );
}

export default App;

//Empty.tsx

export const Empty = () => <h3>Empty</h3>

//Products.tsx

type ProductsProps = { // type oppure interface

  value : number;
  data? : string; //opzionale

}

export const Products = (props: ProductsProps) => 
<h3>{props.value} products</h3>
```
posso usare inoltre una tipo speciale di react che mi consente
di utilizzare i generics e possiamo usarlo per la creazione del 
componente products
```js
//Products.tsx

type ProductsProps = { // type oppure interface

  value : number;
  data? : string; //opzionale

}

export const Products: React.FC<ProductsProps> = (props) => 
<h3>{props.value} products</h3>
```
**UN PO' DI STYLING**

installiamo bootstrap e font-awesome:

npm install bootstrap font-awesome  

posso importarli nel file di interesse o più globalmente
posso inserirli nel file index.tsx 
```js
import 'bootstrap/dist/css/bootstrap.min.css'
import 'font-awesome/css/font-awesome.min.css'
```


Con jsx e react abbiamo tutto il DOM tipizzato così come lo stesso css
quindi se ad un div voglio applicare uno stile io posso utilizzare
questa sintassi
```html
<div style={{color: 'white'}}>ciao</div> 
```
dove la parentesi {} più interna rappresenta un oggetto css


se in App.css definiamo la proprietà *centered* nel seguente modo
```css
.centered {
  max-width: 360px;
  margin: 20px auto;
  padding: 20px;
  background-color: #dedede;
  border-radius: 20px;
  text-align: center;
  color: black;
}

.centered.male {
  background-color: skyblue;
}

.centered.female {
  background-color: pink;
}
```
possiamo poi utilizzare una variabile wrapper per la gestione 
della class del nostro ```<div>``` e dello style del ```<h1>```
```javascript
import React from 'react';
import './App.css';

const App = () => {

  const gender: string = 'F';

  const wrapperStyle: string = gender === 'M' ? 'centered male' : 'centered female'
  const styles: React.CSSProperties = gender === 'M' ? {color: 'white', padding: 10} : {padding: 20}
  return (
    <div className={wrapperStyle}>
      <h1 style={styles}>
      you are {gender === 'M' ? 'male' : 'female'}
      </h1>
    </div>
  );
}

export default App;
```
in questo modo in base al valore gender avremo un comportamento
dinamico dello style dell'```<h1>``` con la variabile style e del ```<div>``` dove utilizziamo wrapperStyle.
In particolare abbiamo tipizzato anche la variabile stile
definendola come un vero e proprio oggetto css tramite 
il tipo React.CSSProperties


Seconda parte

- Styling
- React Fundamentals Hooks
- Dynamic List
- Server side communication
- Typed component & props
- Router

**Hooks Fundamentals**

- *useState()*

Ci consente di gestire uno stato. 
pensiamo ad esempio di voler gestire un menù che in base ad un click si apre o si chiude. 
per gestire lo stato di apertura/chiusura possiamo utilizzare
l'useState. 

```useState<boolean>(false)``` in questo modo definiamo lo stato di tipo boolean e con valore iniziale a false.

useState restituisce due proprietà: 
- lo stato : che chiamiamo *opened*
- una funzione :  che consente di scrivere lo stato che chiamiamo *setState*

utilizziamo quindi la sintassi destructuring per ottenere queste due proprietà

***
il *Destructuring assignement* consente di estrapolare da un oggetto 
delle proprietà e valori da array in variabili distinte
***

```javascript
const [opened, setOpened] = useState<boolean>(false);
```

dove:
useState<boolean>(false) è il modo di utilizzare l'hook per il quale definiamo il tipo, boolean, ed un valore iniziale, false.

```js
import { useState } from 'react';

function App() {
  const [opened, setOpened] = useState<boolean>(false);


  const toggle = () => {
     setOpened(!opened);
    console.log('click');
  }

  const isOpen = opened ? 'menu-open opened' : 'menu-open';

  return (
    <div>
      <nav className="menu">
      <div className={isOpen}/>
      <label className="menu-open-button" onClick={toggle}>
    <div/>
}
```

Con l'utilizzo dell'hook ogni volta che chiamo setOpened
si aggiorna lo stato interno del componente che viene riprocessato
andando quindi a renderizzare il corrette comportamente del componente.


Il modo in cui abbiamo dichiarato la variabile *isOpen* non è elegantissimo per le classi di stile dinamico per questo possiamo utilizzare *classNames* un utility simile alla ng-class di angular
```
npm install classnames @types/classnames
```
e andiamo a definire isOpen in questo modo

const isOpen = cn('menu-open', {'opened': opened});

dove in base al valore di *opened* (il nostro stato) verrà aggiunto
allo stile base *'menu-open'* la classe di stile *'opened'*

**Come creare una lista dinamica in React**
consideriamo questo menù con questa lista di item:

```html
<div className="menu-item"> <i className="fa fa-google"/> </div>
<div className="menu-item"> <i className="fa fa-windows"/> </div>
<div className="menu-item"> <i className="fa fa-facebook"/> </div>
<div className="menu-item"> <i className="fa fa-linkedin"/> <div>
<div className="menu-item"> <i className="fa fa-instagram"/> </div>
<div className="menu-item"> <i className="fa fa-youtube"/><div>
```

possiamo creare un oggetto item che contenga i classname per lo stile delle icone e ci aggiungiamo una proprietà *url*

```js
const items = [
  {icon: 'fa fa-google', url: 'http://www.google.com'},
  {icon: 'fa fa-windows', url: 'http://www.microsoft.com'},
  {icon: 'fa fa-facebook', url: 'http://www.facebook.com'},
  {icon: 'fa fa-linkedin', url: 'http://www.linkedin.com'},
  {icon: 'fa fa-instagram', url: 'http://www.instagram.com'},
  {icon: 'fa fa-youtube', url: 'http://www.youtube.com'}
]
```
all'interno del JSX se vogliamo utilizzare un'espressione di codice
possiamo farlo con le parentesi graffe
```js
.
.
.
  const goToLink = (url: string)=>{
    window.open(url);
  }
.
.
.

  {
    items.map(item => {
      return (
        <div className="menu-item" onClick={() => goToLink(item.url)}> 
          <i className={item.icon}/> 
        </div>)
    })
  }
```

quindi iteriamo con il metodo *map* per tutti gli item del nostro oggetto e aggiungiamo la funzione che al click dell'icona è possibile aprire in una nuova finestra la *url* corrispondente

**Conversione In componente**

Vogliamo trasformare il menù in un componente che accetta un array di elementi. Per fare ciò creiamo un nuovo file tsx e diamogli il nome del componente.
a questo punto il nuovo componente verrà definito in questo modo: 
```js
export const NewComponent: React.FC = () => {
    return (
        <div></div>
    )
}
```
Nel nostro caso noi vogliamo che il componente accetti una lista di items e questi item sono composti da un *icon* e da un *url* quindi possiamo sfruttare i *type* o le *interface* per andare a definire il nostro componente. Definiamo quindi due interfacce: 

```js
interface Item {
    icon: string;
    url: string;
}

interface AnimatedHamburgerProps {
    componentItems: Item[]
}
```
A questo punto cambiamo la definizione del nostro componente che diventerà: 

```js
export const AnimatedHamburger: React.FC<AnimatedHamburgerProps> = (props) => {}
```
quindi abbiamo definito come devo essere gli item e siamo andati a specificare le props del nostro componente tipizzandole ( al loro interno quindi ci sarà la proprietà *componentItems*).



https://codepen.io/lbebber/pen/RNgBPP



