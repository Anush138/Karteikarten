# Karteikarten
// Speicher für alle Karteikarten
let flashcards = [];

// Funktion zum Hinzufügen einer neuen Karteikarte
function addCard() {
    let frontText = document.getElementById("front").value;
    let backText = document.getElementById("back").value;

    if (frontText === "" || backText === "") {
        alert("Bitte beide Felder ausfüllen!");
        return;
    }

    let card = {
        front: frontText,
        back: backText,
        learned: false // Status, ob die Antwort richtig/falsch war
    };

    flashcards.push(card);
    saveToLocalStorage();
    renderCards();
}

// Funktion zum Anzeigen der Karteikarten
function renderCards() {
    let container = document.getElementById("card-container");
    container.innerHTML = ""; // Zurücksetzen

    flashcards.forEach((card, index) => {
        let cardElement = document.createElement("div");
        cardElement.classList.add("card");
        cardElement.innerHTML = `<p>${card.front}</p>`;
        
        // Klick zum Anzeigen der Antwort
        cardElement.onclick = () => showAnswer(card, cardElement, index);
        
        container.appendChild(cardElement);
    });
}

// Funktion zum Anzeigen der Antwort mit Richtig/Falsch-Funktion
function showAnswer(card, element, index) {
    element.innerHTML = `
        <p>${card.back}</p>
        <button onclick="markCorrect(${index})">✅ Richtig</button>
        <button onclick="markWrong(${index})">❌ Falsch</button>
    `;
}

// Markiere als richtig
function markCorrect(index) {
    flashcards[index].learned = true;
    saveToLocalStorage();
    renderCards();
}

// Markiere als falsch
function markWrong(index) {
    flashcards[index].learned = false;
    saveToLocalStorage();
    renderCards();
}

// Funktion zum Speichern der Daten im lokalen Speicher
function saveToLocalStorage() {
    localStorage.setItem("flashcards", JSON.stringify(flashcards));
}

// Funktion zum Laden der Karteikarten beim Start
function loadFromLocalStorage() {
    let data = localStorage.getItem("flashcards");
    if (data) {
        flashcards = JSON.parse(data);
        renderCards();
    }
}

// Beim Laden der Seite gespeicherte Daten abrufen
document.addEventListener("DOMContentLoaded", loadFromLocalStorage);
