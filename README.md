const flashcards = [
    {
        question: "Qual filme é conhecido pela frase 'Eu vejo gente morta'?",
        answer: "O Sexto Sentido (1999)",
        info: "Um thriller psicológico que mistura elementos sobrenaturais com uma reviravolta chocante no final. Dirigido por M. Night Shyamalan."
    },
    // ... (demais flashcards)
];

let currentCardIndex = 0;

const flashcardContainer = document.getElementById('flashcard-container');
const prevBtn = document.getElementById('prev-btn');
const nextBtn = document.getElementById('next-btn');

function createFlashcard(cardData) {
    const flashcard = document.createElement('div');
    flashcard.classList.add('flashcard');
    flashcard.innerHTML = `
        <div class="flashcard-inner">
            <div class="flashcard-front">
                <p>${cardData.question}</p>
            </div>
            <div class="flashcard-back">
                <p><strong>Resposta:</strong> ${cardData.answer}</p>
                <p><strong>Info:</strong> ${cardData.info}</p>
            </div>
        </div>
    `;

    flashcard.addEventListener('click', () => {
        flashcard.classList.toggle('flipped');
    });

    return flashcard;
}

function displayCard(index) {
    flashcardContainer.innerHTML = '';
    const card = createFlashcard(flashcards[index]);
    flashcardContainer.appendChild(card);
}

nextBtn.addEventListener('click', () => {
    currentCardIndex = (currentCardIndex + 1) % flashcards.length;
    displayCard(currentCardIndex);
});

prevBtn.addEventListener('click', () => {
    currentCardIndex = (currentCardIndex - 1 + flashcards.length) % flashcards.length;
    displayCard(currentCardIndex);
});

displayCard(currentCardIndex);
