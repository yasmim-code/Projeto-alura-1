body {
    background-color: #1a0101; /* Um vermelho bem escuro, quase preto */
    color: #e0e0e0; /* Um cinza claro para o texto */
    font-family: 'Roboto Mono', monospace;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
    overflow: hidden; /* Evita barras de rolagem desnecessárias */
}

.container {
    background-color: #0d0000; /* Preto bem escuro */
    border: 2px solid #8b0000; /* Vermelho sangue nas bordas */
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(139, 0, 0, 0.7); /* Sombra avermelhada */
    padding: 30px;
    text-align: center;
    max-width: 600px;
    width: 90%;
    position: relative;
    z-index: 1;
}

h1 {
    font-family: 'Creepster', cursive;
    color: #ff0000; /* Vermelho brilhante para o título */
    text-shadow: 0 0 10px #ff0000; /* Efeito neon vermelho */
    font-size: 3em;
    margin-bottom: 30px;
}

#flashcard-container {
    perspective: 1000px; /* Para a animação 3D do flip */
    min-height: 250px; /* Garante que o container não encolha demais */
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
}

.flashcard {
    background-color: #2c0000; /* Um vermelho escuro para o cartão */
    border: 1px solid #8b0000;
    border-radius: 8px;
    width: 100%;
    height: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.2em;
    cursor: pointer;
    transform-style: preserve-3d; /* Permite a rotação 3D */
    transition: transform 0.6s ease-in-out;
    position: relative;
    color: #eee;
    box-shadow: 0 0 15px rgba(139, 0, 0, 0.5);
    backface-visibility: hidden; /* Esconde a face de trás inicialmente */
}

.flashcard.flipped {
    transform: rotateY(180deg);
}

.flashcard-inner {
    position: absolute;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.6s;
    transform-style: preserve-3d;
}

.flashcard-front, .flashcard-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    box-sizing: border-box;
}

.flashcard-back {
    transform: rotateY(180deg);
    background-color: #3f0000; /* Um vermelho um pouco mais escuro para o verso */
}

button {
    background-color: #8b0000; /* Vermelho sangue para os botões */
    color: #fff;
    border: none;
    padding: 10px 20px;
    margin: 0 10px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1em;
    transition: background-color 0.3s ease;
    text-transform: uppercase;
    letter-spacing: 1px;
    box-shadow: 0 0 10px rgba(139, 0, 0, 0.5);
}

button:hover {
    background-color: #ff0000; /* Vermelho mais vibrante no hover */
    box-shadow: 0 0 15px rgba(255, 0, 0, 0.7);
}

/* Efeitos adicionais para deixar mais "terror" */
body::before {
    content: '';
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle, rgba(26, 1, 1, 0.8) 0%, rgba(0,0,0,1) 100%);
    z-index: 0;
}
const flashcards = [
    {
        question: "Qual filme é conhecido pela frase 'Eu vejo gente morta'?",
        answer: "O Sexto Sentido (1999)",
        info: "Um thriller psicológico que mistura elementos sobrenaturais com uma reviravolta chocante no final. Dirigido por M. Night Shyamalan."
    },
    {
        question: "Quem é o diretor da franquia 'Jogos Mortais'?",
        answer: "James Wan (criador do conceito e diretor do primeiro filme)",
        info: "A franquia é famosa por suas armadilhas elaboradas e dilemas morais, com Jigsaw como o antagonista principal."
    },
    {
        question: "Qual filme de terror se passa em uma casa mal-assombrada onde o mal se manifesta em 'jump scares' e possessões?",
        answer: "Invocação do Mal (2013)",
        info: "Baseado em eventos supostamente reais investigados pelos demonologistas Ed e Lorraine Warren."
    },
    {
        question: "Qual é o nome do icônico serial killer da franquia 'Pânico'?",
        answer: "Ghostface",
        info: "Ghostface é conhecido por sua máscara de fantasma e seu amor por filmes de terror, sempre ligando para suas vítimas antes de atacar."
    },
    {
        question: "Em 'Hereditário', qual elemento é usado para invocar o demônio Paimon?",
        answer: "O símbolo de Paimon e um boneco decapitado",
        info: "Um filme de terror psicológico intenso, com atuações marcantes e uma atmosfera de crescente desespero."
    },
    {
        question: "Qual filme de 1978 popularizou o subgênero 'slasher' e apresenta Michael Myers?",
        answer: "Halloween",
        info: "Dirigido por John Carpenter, é considerado um marco no cinema de terror, criando muitas convenções do gênero slasher."
    },
    {
        question: "Qual filme de terror mostra um grupo de amigos sendo caçados por um assassino que não pode ser parado e só segue suas vítimas lentamente?",
        answer: "Corrente do Mal (It Follows - 2014)",
        info: "Um filme com uma premissa original e uma atmosfera de paranoia constante, elogiado pela crítica por sua originalidade."
    },
    {
        question: "Em 'O Iluminado', qual frase Jack Torrance digita repetidamente na máquina de escrever?",
        answer: "'Todo trabalho e nenhuma diversão fazem de Jack um rapaz aborrecido.' (All work and no play makes Jack a dull boy.)",
        info: "Baseado na obra de Stephen King e dirigido por Stanley Kubrick, é um clássico do terror psicológico."
    }
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
    flashcardContainer.innerHTML = ''; // Limpa o flashcard anterior
    const card = createFlashcard(flashcards[index]);
    flashcardContainer.appendChild(card);
}

// Event Listeners para os botões de navegação
nextBtn.addEventListener('click', () => {
    currentCardIndex = (currentCardIndex + 1) % flashcards.length;
    displayCard(currentCardIndex);
});

prevBtn.addEventListener('click', () => {
    currentCardIndex = (currentCardIndex - 1 + flashcards.length) % flashcards.length;
    displayCard(currentCardIndex);
});

// Exibir o primeiro flashcard ao carregar a página
displayCard(currentCardIndex);

