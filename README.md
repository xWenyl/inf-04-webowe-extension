# INF.04 Aplikacje webowe

❗ | Więcej informacji na https://zawodowe.it 



## Komponent

Komponent to niezależna część kodu wielorazowego użytku. Może to być przycisk, tabela, formularz, cokolwiek zadecydujemy.
Standardem jest pisanie komponentów funkcyjnych i nadawanie im nazw wielkią literą.



<details>

<summary>Przykład komponentu zdefiniowanego w pliku Hello.js i użytego w App.js:</summary>

```jsx
// Plik Hello.js
function Hello() {
    return (
        // Komponenty mogą zwracać tylko jeden element korzenny, w tym przypadku div
        <div>
            <h2>Hello, world!</h2>
            <p>How are you doing?</p>
        </div>
    );
}

export default Hello;

```


```jsx
// Plik App.js
// Wymagany jest import komponentu
import Hello from './Hello.js';

function App() {
    return (
        <div>
            <h1>Welcome to my app.</h1>
            <Hello />
        </div>
    );
}

export default App;
```

</details>

---

## `useState()`

React daje nam dostęp do tzw. hooków. Są to funkcje, które pozwalają nam kontrolować działanie aplikacji.
Jednym z nich jest `useState()`. Jako argument przyjmuje dowolną wartość i zwraca nam tzw. state (mający podaną wcześniej wartość), oraz funkcję zmieniającą dany state.

> Porada:
**State** to stan naszej aplikacji. Działa jak normalna zmienna, ale zamiast `=`, jego wartość zmieniamy specjalną funkcją utworzoną za pomocą `useState()`. Różni się też tym, że zmiana wartości state aktualizuje nasz interfejs.

<details>

<summary>Komponent Licznik.js używający state:</summary>

```jsx
// Aby używać hooków należy je zaimportować
import { useState } from 'react';

function Licznik() {
    const [licznik, setLicznik] = useState(0);

    function zwieksz() {
        // Można to rozumieć następująco: licznik = licznik + 1;
        setLicznik(licznik => licznik + 1);
    }

    function resetuj() {
        // Można to rozumieć tak: licznik = 0;
        setLicznik(0);
    }

    return (
        <div>
            <h2>{licznik}</h2>
            <button onClick={zwieksz}>Zwiększ licznik</button>
            <button onClick={resetuj}>Resetuj licznik</button>
        </div>
    );
}
```

</details>

---

## `useRef()`

`useRef()` przyjmuje wartość i jest używany do stworzenia 'adresu', do którego możemy przypisać znacznik HTML.

<details>

<summary>Komponent używający `useRef`:</summary>

```jsx
// Aby używać hook'ów należy je zaimportować
import { useRef } from 'react';

function Licznik() {
    // buttonRef to jakiś pusty adres
    const buttonRef = useRef(0);

    // Po przypisaniu buttonRef to adres danego przycisku
    // Natomiast buttonRef.current to w tym przypadku przycisk
    return (
        <div>
            <button ref={buttonRef}>Kliknij</button>
        </div>
    );
}
```

</details>

---

<br>

## Obsługa inputów

Wyróżniamy dwa, różne sposoby na obsługiwanie inputów:

1. Używając `useRef()`
2. Używajac `useState()`

Różnią się tym, że używając `useState()` możemy programatycznie zmieniać wartość inputa, a używając `useRef()` możemy tą wartość programatycznie jedynie odczytać


<details>

<summary>useRef</summary>

```jsx
import { useRef } from 'react';
function App() {
  const inputRef = useRef(0);

  // W ten sposób możemy odczytać wartość inputa używając inputRef.current.value
  return <input ref={inputRef}>
}
export default App;
```

</details>

---

<details>

<summary>useState</summary>

```jsx
import { useState } from 'react';
function App() {
  const [tekst, setTekst] = useState('');

  // W ten sposób możemy:
  // 1. odczytać wartość inputa używając zmiennej tekst
  // 2. nadpisać wartość inputa używając funkcji setTekst
  return <input value={tekst} onChange={(e) => setTekst(e.target.value)}>
}
export default App;
```

</details>

---

<br>

## `.map()`

<details>

<summary>Przykład</summary>

Gdy nasze dane znajdują się w tablicy możemy je wypisać używając metody `.map()`.

Daje nam to możliwość generowania znaczników HTML jakbyśmy to robili używając pętli.
:::tip Pamiętaj
Jeśli elementy mają być wypisane w liście należy użyć `<ul>`, bądź `<ol>` zależnie od polecenia

```jsx
const tablica = ['a', 'b', 'c'];

// Wypełniamy znacznik <ol> danymi z tablicy
<ol>
    {tablica.map(element => (
        <li key={element}>{element}</li>
    ))}
</ol>;
// W ten sposób w naszym <ol> stworzymy 3 znaczniki <li>:
// <li key="a">a</li>
// <li key="b">b</li>
// <li key="c">c</li>
```

</details>

---

<br>

## Przykładowe zadania

<details>

<summary> Zadanie kursy</summary>

```jsx
//Plik App.js
import "./App.css";
import { useState } from "react"; // Importujemy useState

function App() {
  const kursy = [
    "Programowanie w C#",
    "Angular dla początkujących",
    "Kurs Django",
  ]; // Tablica zawierająca 3 kursy

  const [imie, setImie] = useState(""); // State do inputu z imieniem
  const [kurs, setKurs] = useState(); // State do inputu z numerem kursu

  function handleSubmit(e) {
    // Funkcja wywołana po wysłaniu formularza
    e.preventDefault(); // Blokujemy odświeżenie strony po przesłaniu formularza

    console.log(imie);
    if (kursy[kurs - 1]) {
      console.log(kursy[kurs - 1]);
    } else {
      console.log("Nieprawidłowy numer kursu");
    }
  }

  return (
    <div className="App">
      <h2>Liczba kursów {kursy.length}</h2> {/* Wypisujemy listę kursów */}
      <ol>
        {kursy.map((kurs) => (
          <li key={kurs}>{kurs}</li> // Wypisanie elementów tablicy "kursy" za pomocą metody .map()
        ))}
      </ol>
      <form onSubmit={handleSubmit}>
        {/* Formularz, który po przesłaniu wywołuje funkcję handleSubmit() */}
        <div className="form-group">
          <label htmlFor="imieInput">Imię i nazwisko:</label>
          <input
            type="text"
            className="form-control"
            id="imieInput"
            value={imie} // Przypisanie state "imie" do wartosci inputu
            onChange={(e) => setImie(e.target.value)} // Zmiana wartości state "imie"
          />
        </div>
        <div className="form-group">
          <label htmlFor="kursInput">Numer kursu:</label>
          <input
            type="text"
            className="form-control"
            id="kursInput"
            value={kurs} // Przypisanie state "kurs" do wartości inputu
            onChange={(e) => setKurs(e.target.value)} // Zmiana wartości state "kurs"
          />
        </div>
        <button className="btn btn-primary">Zapisz do kursu</button>
      </form>
    </div>
  );
}

export default App;
```



```css
/* Plik App.css */
.App {
  width: 60%; /* Ustawienie szerokości aplikacji na 60% body*/
  margin: 0 auto; /* Wyśrodkowanie aplikacji */
}
```

</details>

---