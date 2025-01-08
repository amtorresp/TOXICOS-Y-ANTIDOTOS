// HTML Structure for the Game
const toxins = [
  { toxin: 'ACO Cumarínicos', antidote: 'Vitamina K (filoquinona)' },
  { toxin: 'Anticolinérgicos', antidote: 'Fisostigmina' },
  { toxin: 'Antagonistas de canales de calcio', antidote: 'Gluconato cálcico o Cloruro de calcio' },
  { toxin: 'Antidepresivos tricíclicos', antidote: 'Bicarbonato sódico' },
  { toxin: 'Benzodiazepinas', antidote: 'Flumazenilo' },
  { toxin: 'β-bloqueantes', antidote: 'Glucagón' },
  { toxin: 'Cianuro', antidote: 'Hidroxicobalamina (Vit. B12)' },
  { toxin: 'Dabigatrán', antidote: 'Idarucizumab' },
  { toxin: 'Digoxina', antidote: 'Anticuerpos antidigital (Fab antidigoxina)' },
  { toxin: 'Heparina', antidote: 'Sulfato de protamina' },
  { toxin: 'Hierro', antidote: 'Desferoxamina' },
  { toxin: 'Insecticidas organofosforados', antidote: 'Atropina + Pralidoximina' },
  { toxin: 'Isoniazida', antidote: 'Piridoxina (Vit. B6)' },
  { toxin: 'Metahemoglobinizantes', antidote: 'Azul de metileno' },
  { toxin: 'Metanol y etilenglicol', antidote: 'Etanol o Fomepizol' },
  { toxin: 'Monóxido de carbono', antidote: 'Oxígeno' },
  { toxin: 'Opiáceos', antidote: 'Naloxona' },
  { toxin: 'Paracetamol', antidote: 'N-Acetilcisteína' },
  { toxin: 'Plomo, arsénico, oro, mercurio, antimonio y bismuto', antidote: 'Dimercaprol' },
  { toxin: 'Rivaroxabán, Apixabán', antidote: 'Andexanet' },
  { toxin: 'Setas con amatinas', antidote: 'Penicilina G o Silibinina' },
  { toxin: 'Veneno de víbora', antidote: 'Suero antiofídico' },
];

const app = document.getElementById('app');
app.innerHTML = `
  <h1>Unir Tóxicos con sus Antídotos</h1>
  <div class="game-container">
    <div class="toxins">
      ${toxins.map(t => `<div class="toxin">${t.toxin}</div>`).join('')}
    </div>
    <div class="antidotes">
      ${toxins.map(t => `<div class="antidote" draggable="true" ondragstart="drag(event)">${t.antidote}</div>`).join('')}
    </div>
  </div>
  <button onclick="checkAnswers()">Resolver</button>
  <button onclick="showCorrectAnswers()">Mostrar respuestas correctas</button>
  <div id="result"></div>
`;

// Shuffle antidotes for random placement
const antidoteContainer = document.querySelector('.antidotes');
for (let i = antidoteContainer.children.length; i >= 0; i--) {
  antidoteContainer.appendChild(antidoteContainer.children[Math.random() * i | 0]);
}

// Drag and Drop Functionality
function drag(event) {
  event.dataTransfer.setData("text", event.target.innerText);
}
function allowDrop(event) {
  event.preventDefault();
}
function drop(event) {
  event.preventDefault();
  const antidote = event.dataTransfer.getData("text");
  event.target.innerText = antidote;
}

// Check Answers
function checkAnswers() {
  const antidoteElements = document.querySelectorAll('.antidotes .antidote');
  const result = document.getElementById('result');
  result.innerHTML = '';

  antidoteElements.forEach((antidoteEl, index) => {
    if (antidoteEl.innerText === toxins[index].antidote) {
      antidoteEl.style.backgroundColor = 'green';
    } else {
      antidoteEl.style.backgroundColor = 'red';
    }
  });
}

// Show Correct Answers
function showCorrectAnswers() {
  const antidoteElements = document.querySelectorAll('.antidotes .antidote');
  antidoteElements.forEach((antidoteEl, index) => {
    antidoteEl.innerText = toxins[index].antidote;
    antidoteEl.style.backgroundColor = 'green';
  });
}
