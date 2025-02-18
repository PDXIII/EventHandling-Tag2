# Ereignisbehandlung Tag 2

## Lernziele

### Anwenden der Ereignisbehandlung in praktischen Szenarien:

- Verhindern der Standardaktionen von Ereignissen.
- Arbeiten mit verschiedenen Phasen des Ereignisflusses (Capturing, Target, Bubbling).

## Inhalte

### Verhindern der Standardaktionen:
Verwendung von `preventDefault`:
        
```jsx
document.getElementById("myForm").addEventListener("submit", function(event) {
	event.preventDefault();
	alert("Form submission prevented!");
});
```
        
### Ereignisfluss und -phasen:
Erklärung der drei Phasen des Ereignisflusses: Capturing, Target, Bubbling:
        
```jsx
document.getElementById("outer").addEventListener("click", function() {
	console.log("Outer div clicked - Capturing phase");
}, true); // Capturing phase 

document.getElementById("inner").addEventListener("click", function() {
	console.log("Inner div clicked - Target phase");
});

document.getElementById("outer").addEventListener("click", function() {
	console.log("Outer div clicked - Bubbling phase");
}); // Bubbling phase
```
        

### **Übersicht**
- **Stunde 1:** `preventDefault()` – Verhindern von Standardaktionen  
- **Stunde 2:** Einführung in den Ereignisfluss (Capturing, Target, Bubbling)  
- **Stunde 3:** Praktische Umsetzung des Ereignisflusses  


---

## **🕐 Stunde 1: Verhindern der Standardaktionen (`preventDefault`)**
### **Ziel:** Verständnis der Methode `preventDefault()` und wann sie genutzt wird.

### **Theorie: Was macht `preventDefault()`?**
- Jedes Ereignis hat eine Standardaktion (z. B. ein Link navigiert oder ein Formular wird abgeschickt).
- `preventDefault()` verhindert diese Standardaktion.

### **Beispiele für `preventDefault()`**

#### **1. Verhindern des Absenden eines Formulars**
```js
document.getElementById("myForm").addEventListener("submit", function(event) {
	event.preventDefault();
	alert("Form submission prevented!");
});
```
➡ **Ohne `preventDefault()`:** Das Formular wird gesendet.  
➡ **Mit `preventDefault()`:** Die Seite bleibt erhalten, das Formular wird nicht abgeschickt.

**Achtung**

Neben `submit`auch `change` vorstellen!

```html
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kontaktformular</title>
</head>
<body>
    <form id="kontaktForm">
        <label for="vorname">Vorname:</label>
        <input type="text" id="vorname" name="vorname" required>
        
        <label for="nachname">Nachname:</label>
        <input type="text" id="nachname" name="nachname" required>
        
        <label for="nachricht">Nachricht:</label>
        <textarea id="nachricht" name="nachricht" rows="5" required></textarea>
        
        <button type="submit">Absenden</button>
    </form>

    <script>
        document.getElementById("kontaktForm").addEventListener("submit", function(event) {
            event.preventDefault(); // Verhindert das Neuladen der Seite
            
            // Formularwerte auslesen
            const vorname = document.getElementById("vorname").value;
            const nachname = document.getElementById("nachname").value;
            const nachricht = document.getElementById("nachricht").value;
            
            // Ausgabe in die Konsole
           console.table([{ Vorname: vorname, Nachname: nachname, Nachricht: nachricht }]);

        });
    </script>
</body>
</html>
```

**Beispiel Link:**

``` html

<a href="https://example.com" id="meinLink">Klick mich!</a>

<script>
  document.getElementById("meinLink").addEventListener("click", function(event) {
    event.preventDefault(); // Verhindert, dass der Link die Seite wechselt
    console.log("Der Link wurde geklickt, aber die Seite wird nicht gewechselt.");
  });
</script>
```
---

## **🕑 Stunde 2: Einführung in den Ereignisfluss**
### **Ziel:** Verständnis der Phasen von Events (Capturing, Target, Bubbling)

### **Ereignisfluss – 3 Phasen**
1. **Capturing Phase (Erfassung)** – Das Event wandert von **oben nach unten**.  
2. **Target Phase (Ziel)** – Das Event erreicht das Element.  
3. **Bubbling Phase (Blasen)** – Das Event wandert von **unten nach oben** zurück.

### **Beispiel für Ereignisfluss**
```js
document.getElementById("outer").addEventListener("click", function() {
	console.log("Outer div clicked - Capturing phase");
}, true); // Capturing Phase (dritte Option = true)

document.getElementById("inner").addEventListener("click", function() {
	console.log("Inner div clicked - Target phase");
});

document.getElementById("outer").addEventListener("click", function() {
	console.log("Outer div clicked - Bubbling phase");
}); // Bubbling Phase (Standard)
```

### **Erklärung:**
- **Capturing Phase**: Das äußere `div` fängt das Ereignis zuerst ab.
- **Target Phase**: Das Ereignis trifft auf das angeklickte Element (`inner`).
- **Bubbling Phase**: Das Ereignis „blubbert“ wieder nach oben zum `outer`.

### 📌 Lernziele dieser Stunde:

- ✅ Verstehen, wie Ereignisse durch das DOM wandern (Event Propagation).
- ✅ Unterscheiden zwischen Bubbling und Capturing.
- ✅ Wissen, wann Ereignis-Delegation sinnvoll ist.
- ✅ Praktische Anwendungen der Ereignis-Delegation umsetzen.

### 🕐 1. Einführung: Wie Ereignisse im DOM propagieren (10 Min.)

**🧐 Frage zur Aktivierung des Vorwissens:**

- „Was passiert, wenn ihr auf einen Button in einer Liste klickt?“
- „Wie erkennt JavaScript, welches Element geklickt wurde?“

#### 📖 Theorie: Wie Ereignisse das DOM durchlaufen

Wenn ein Event auf einem Element ausgelöst wird (z. B. ein click auf einen Button), bewegt sich das Event durch das DOM in drei Phasen:

1. Capturing Phase (Erfassung): Das Event startet vom window und wandert durch die Elternelemente nach unten zum Ziel.
2. Target Phase: Das Event erreicht das eigentliche Ziel-Element (z. B. einen Button).
3. Bubbling Phase (Blasenbildung): Das Event wandert wieder nach oben durch die Elternelemente zurück zum window.

**💡 Standardverhalten:**

Die meisten Ereignisse nutzen die Bubbling Phase (von unten nach oben).
Die Capturing Phase wird selten genutzt, kann aber mit true als dritten Parameter in addEventListener aktiviert werden.

### 🕑 2. Praktische Demonstration: Event-Bubbling & Capturing (15 Min.)

**📌 HTML-Testumgebung:**

```
<div id="parent" style="padding: 20px; background-color: lightblue;">
  Parent-DIV
  <button id="child">Klick mich!</button>
</div>
```

**📌 JavaScript: Standard-Bubbling**

```
document.getElementById("parent").addEventListener("click", function () {
  console.log("Parent-Element wurde geklickt!");
});

document.getElementById("child").addEventListener("click", function () {
  console.log("Child-Element wurde geklickt!");
});
```

**💡 Erwartetes Verhalten:**

1. Beim Klick auf den Button wird zuerst „Child-Element wurde geklickt!“ ausgegeben.
2. Danach „Parent-Element wurde geklickt!“ – das Event steigt nach oben (Bubbling).

**🧐 Frage an die Klasse:**

„Warum wird zuerst die Nachricht des Child-Elements und dann die des Parent-Elements ausgegeben?“

💬 Antwort: Weil das Event vom Button aufsteigt (Bubbling).

**📌 JavaScript: Capturing aktivieren**

```
document.getElementById("parent").addEventListener(
  "click",
  function () {
    console.log("Parent-Element (Capturing)");
  },
  true // Capturing aktivieren
);
```

**💡 Erwartetes Verhalten:**

1. Zuerst „Parent-Element (Capturing)“ (weil das Event von oben nach unten durchläuft).
2. Dann „Child-Element wurde geklickt!“.

**🧐 Frage an die Klasse:**
„Wann könnte Capturing nützlich sein?“

💬 Antwort: Zum Beispiel, wenn man sicherstellen will, dass Eltern-Elemente zuerst auf Ereignisse reagieren.

### 🕒 3. Ereignis-Delegation: Warum und wie? (15 Min.)

#### 📌 Warum Ereignis-Delegation?

Wenn wir mehrere ähnliche Elemente haben, wäre es ineffizient, jedem Element einen separaten Event-Listener zu geben.
Stattdessen können wir den Event-Listener an ein übergeordnetes Element hängen und dann anhand von event.target bestimmen, welches untergeordnete Element geklickt wurde.

**📌 Beispiel ohne Delegation (ineffizient):**

```
document.querySelectorAll(".list-item").forEach((item) => {
  item.addEventListener("click", function () {
    console.log("Item geklickt: " + this.textContent);
  });
});
```

#### 🔴 Problem:

Jeder Listeneintrag bekommt einen eigenen Event-Listener, was bei großen Listen ineffizient ist.

**📌 Ereignis-Delegation richtig nutzen:**

```
<ul id="list">
  <li class="list-item">Element 1</li>
  <li class="list-item">Element 2</li>
  <li class="list-item">Element 3</li>
</ul>
document.getElementById("list").addEventListener("click", function (event) {
  if (event.target.tagName === "LI") {
    console.log("Geklickt: " + event.target.textContent);
  }
});
```

**💡 Erklärung:**

Der Event-Listener wird nur einmal auf `<ul>` gesetzt.
Mit `event.target` überprüfen wir, ob tatsächlich ein `<li>`-Element angeklickt wurde.

Effizient, besonders bei dynamisch hinzugefügten Elementen!

**🧐 Frage an die Klasse:**

„Wann könnte Ereignis-Delegation praktisch sein?“
💬 Antwort: Bei dynamisch generierten Inhalten, z. B. Chatnachrichten, Menüs, Tabellen.

### 🕓 4. Praktische Übung (30 Min.)

**Aufgabe:**

1. Erstelle eine `<ul>`-Liste mit 5 `<li>`-Elementen.
2. Setze einen einzigen Event-Listener auf `<ul>`, der ausgibt, welches `<li>`-Element angeklickt wurde.
3. Füge per Button-Klick neue `<li>`-Elemente hinzu – sie sollen auch auf Klicks reagieren!

**💡 Lösung:**

```
<button id="addItem">Neues Element hinzufügen</button>
<ul id="list">
  <li class="list-item">Element 1</li>
  <li class="list-item">Element 2</li>
  <li class="list-item">Element 3</li>
</ul>
document.getElementById("list").addEventListener("click", function (event) {
  if (event.target.tagName === "LI") {
    console.log("Geklickt: " + event.target.textContent);
  }
});

document.getElementById("addItem").addEventListener("click", function () {
  const newItem = document.createElement("li");
  newItem.textContent = "Neues Element";
  document.getElementById("list").appendChild(newItem);
});
```

**✅ Erwartetes Verhalten:**

Beim Klick auf ein `<li>` wird dessen Text ausgegeben.
Neue `<li>`-Elemente reagieren ebenfalls, ohne dass neue Event-Listener hinzugefügt werden müssen!

### 🎯 Zusammenfassung (10 Min.)

- ✅ Event-Bubbling: Ereignisse wandern von unten nach oben im DOM.
- ✅ Event-Capturing: Ereignisse wandern von oben nach unten.
- ✅ Ereignis-Delegation: Effiziente Technik, um Event-Handling auf viele Elemente zu übertragen.
- ✅ Vorteil: Spart Speicherplatz und ermöglicht dynamische Inhalte.

**❓ Reflexionsfragen:**

- „Warum ist Bubbling das Standardverhalten?“
- „Wann wäre Event-Capturing sinnvoll?“
- „Warum sollte man `event.target` in der Ereignis-Delegation nutzen?“


---

## **🕒 Stunde 3: Praktische Umsetzung des Ereignisflusses**
### **Ziel:** Anwendung der Phasen in realen Szenarien.

Hier ist der passende HTML-Code mit zwei Untermenüs, die jeweils eigene Event-Listener haben.  

### **Funktionalität:**  
- Klick auf das **Hauptmenü** öffnet es.  
- Klick auf ein **Untermenü** öffnet nur dieses, ohne andere Menüs zu beeinflussen.  
- **`stopPropagation()`** sorgt dafür, dass Klicks innerhalb der Menüs nicht zum Schließen führen.  

---

### **📌 Code: HTML + JavaScript**
```html
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Menü mit Untermenüs</title>
    <style>
        .menu {
            width: 200px;
            padding: 10px;
            background-color: lightblue;
            margin-bottom: 15px;
        }
        .submenu {
            padding: 10px;
            background-color: coral;
            margin-left: 10px;
        }
    </style>
</head>
<body>

    <div id="menu" class="menu">Hauptmenü
        <div id="submenu1" class="submenu">Untermenü 1</div>
        <div id="submenu2" class="submenu">Untermenü 2</div>
    </div>

    <script>
        // Hauptmenü-Klick öffnet oder schließt es
        document.getElementById("menu").addEventListener("click", function(event) {
            event.stopPropagation(); // Stoppt Bubbling
            alert("Menü wurde geöffnet!");
            this.classList.toggle("open");
        });

        // Untermenü 1
        document.getElementById("submenu1").addEventListener("click", function(event) {
            event.stopPropagation();
            alert("Untermenü 1 wurde geöffnet!");
        });

        // Untermenü 2
        document.getElementById("submenu2").addEventListener("click", function(event) {
            event.stopPropagation();
            alert("Untermenü 2 wurde geöffnet!");
        });

        // Klick außerhalb schließt das Menü
        document.addEventListener("click", function() {
            document.getElementById("menu").classList.remove("open");
        });
    </script>

</body>
</html>
```

---

### **💡 Erklärung:**  
✔ **`stopPropagation()`**:  
Verhindert, dass Klicks auf ein Untermenü auch das Hauptmenü beeinflussen.  

✔ **Klick auf das Dokument (`document.addEventListener`)**:  
Schließt das Hauptmenü, wenn man **außerhalb klickt**.  

✔ **`classList.toggle("open")`**:  
Schaltet den Status des Menüs zwischen „geöffnet“ und „geschlossen“.  

---

### **🔥 Warum ist das wichtig?**  
✅ Verhindert, dass sich Untermenü-Klicks auf das Hauptmenü auswirken.  
✅ Praktisch für **Navigationsmenüs und interaktive Interfaces**.### 

**Übung: Ereignissteuerung optimieren**
👉 Aufgabe: Ein `<ul>`-Element, das nur Klicks auf `<li>`-Elemente erlaubt.  
👉 Ziel: `event.target` verwenden, um nur auf `<li>`-Klicks zu reagieren.

```js
document.getElementById("list").addEventListener("click", function(event) {
	if (event.target.tagName === "LI") {
		alert("Listenelement geklickt: " + event.target.innerText);
	}
});
```
➡ **Ohne diese Überprüfung**: Auch Klicks auf die `<ul>` werden registriert.

---

## 🔹 Unterschied zwischen `event.target` und `event.currentTarget` in JavaScript

Beide Begriffe beziehen sich auf das Element, das ein Ereignis ausgelöst hat, aber mit einem wichtigen Unterschied:

### Property	Beschreibung

event.target	Das Element, auf dem das Ereignis ausgelöst wurde (kann tief im DOM verschachtelt sein).
event.currentTarget	Das Element, an das der Event-Listener gebunden ist (meistens der übergeordnete Container bei Event-Delegation).
📌 Beispiel: Unterschied zwischen target und currentTarget
Angenommen, wir haben eine div, die mehrere button-Elemente enthält:

```
<div id="container">
  <button id="btn1">Button 1</button>
  <button id="btn2">Button 2</button>
</div>
```

Nun fügen wir einen einzigen Event-Listener auf #container hinzu (Event-Delegation):

```
document.getElementById("container").addEventListener("click", function(event) {
  console.log("event.target:", event.target); // Zeigt das tatsächliche Element, das geklickt wurde
  console.log("event.currentTarget:", event.currentTarget); // Zeigt immer #container
});
```

**🔹 Was passiert, wenn der Benutzer auf Button 1 klickt?**

Die Konsole zeigt:

```
event.target: <button id="btn1">Button 1</button>
event.currentTarget: <div id="container"></div>
```

`event.target` → Das tatsächliche Element, auf das der Benutzer geklickt hat (`button#btn1`).
`event.currentTarget` → Das Element, an das der Listener gebunden wurde (`div#container`).

### 📌 Wann benutzt man `event.target` vs. `event.currentTarget`?

#### ✅ `event.target` wird verwendet, um das spezifische Element zu ermitteln, das das Ereignis ausgelöst hat.

→ Beispiel: Prüfen, ob ein bestimmter Button innerhalb eines Containers angeklickt wurde:

```js
document.getElementById("container").addEventListener("click", function(event) {
  if (event.target.tagName === "BUTTON") {
    console.log("Ein Button wurde geklickt:", event.target.innerText);
  }
});
```

#### ✅ `event.currentTarget` wird verwendet, wenn man sich auf das Element bezieht, an dem der Listener hängt, unabhängig davon, was genau geklickt wurde.

→ Beispiel: Styling oder Verhalten auf den gesamten Container anwenden:

```js
document.getElementById("container").addEventListener("click", function(event) {
  event.currentTarget.style.backgroundColor = "lightgray"; // Ändert den Hintergrund des Containers
});
```

#### 📌 Fazit

- 🔹 `event.target` ist das eigentliche Element, das das Ereignis ausgelöst hat (kann ein Kind-Element sein).
- 🔹 `event.currentTarget` ist das Element, an das der Listener gebunden wurde (immer das gleiche bei Event-Delegation).
- 🔹 `event.target` wird oft bei Event-Delegation verwendet, um gezielt Aktionen auf Kind-Elemente anzuwenden.

#### 💡 Merksatz:

`event.target` ist das Kind, das das Ereignis gestartet hat – event.currentTarget ist der Elternteil, der zuhört! 🎯


