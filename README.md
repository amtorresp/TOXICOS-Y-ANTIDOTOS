import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { GripVertical } from 'lucide-react';

const ToxinAntidoteGame = () => {
  const [toxinData] = useState([
    { toxin: "ACO Cumarínicos", antidote: "Vitamina K (filoquinona)" },
    { toxin: "Anticolinérgicos", antidote: "Fisostigmina" },
    { toxin: "Antagonistas de canales de calcio", antidote: "Gluconato cálcico o Cloruro de calcio" },
    { toxin: "Antidepresivos tricíclicos", antidote: "Bicarbonato sódico" },
    { toxin: "Benzodiazepinas", antidote: "Flumazenilo" },
    { toxin: "β-bloqueantes", antidote: "Glucagón" },
    { toxin: "Cianuro", antidote: "Hidroxicobalamina (Vit. B12)" },
    { toxin: "Dabigatrán", antidote: "Idarucizumab" },
    { toxin: "Digoxina", antidote: "Anticuerpos antidigital (Fab antidigoxina)" },
    { toxin: "Heparina", antidote: "Sulfato de protamina" },
    { toxin: "Hierro", antidote: "Desferoxamina" },
    { toxin: "Insecticidas organofosforados", antidote: "Atropina + Pralidoximina" },
    { toxin: "Isoniazida", antidote: "Piridoxina (Vit. B6)" },
    { toxin: "Metahemoglobinizantes", antidote: "Azul de metileno" },
    { toxin: "Metanol y etilenglicol", antidote: "Etanol o Fomepizol" },
    { toxin: "Monóxido de carbono", antidote: "Oxígeno" },
    { toxin: "Opiáceos", antidote: "Naloxona" },
    { toxin: "Paracetamol", antidote: "N-Acetilcisteína" },
    { toxin: "Plomo, arsénico, oro, mercurio, antimonio y bismuto", antidote: "Dimercaprol" },
    { toxin: "Rivaroxabán, Apixabán", antidote: "Andexanet" },
    { toxin: "Setas con amatinas", antidote: "Penicilina G o Silibinina" },
    { toxin: "Veneno de víbora", antidote: "Suero antiofídico" }
  ]);

  const [antidoteOrder, setAntidoteOrder] = useState(
    toxinData.map(item => item.antidote).sort(() => Math.random() - 0.5)
  );
  const [showResults, setShowResults] = useState(false);
  const [showAllAnswers, setShowAllAnswers] = useState(false);
  const [draggedItem, setDraggedItem] = useState(null);
  const [draggedOverIndex, setDraggedOverIndex] = useState(null);

  const handleDragStart = (antidote) => {
    setDraggedItem(antidote);
  };

  const handleDragOver = (e, index) => {
    e.preventDefault();
    setDraggedOverIndex(index);
  };

  const handleDrop = (e, index) => {
    e.preventDefault();
    if (!draggedItem) return;

    const newOrder = [...antidoteOrder];
    const oldIndex = newOrder.indexOf(draggedItem);
    newOrder.splice(oldIndex, 1);
    newOrder.splice(index, 0, draggedItem);

    setAntidoteOrder(newOrder);
    setDraggedItem(null);
    setDraggedOverIndex(null);
  };

  const isCorrect = (toxin, index) => {
    return toxinData[index].antidote === antidoteOrder[index];
  };

  const calculateScore = () => {
    const correct = toxinData.filter((item, index) => 
      item.antidote === antidoteOrder[index]
    ).length;
    return Math.round((correct / toxinData.length) * 100);
  };

  return (
    <Card className="w-full max-w-7xl mx-auto p-6">
      <CardContent>
        <div className="text-center mb-6">
          <h1 className="text-2xl font-bold mb-4">Ordena los Antídotos</h1>
          <p className="mb-4">Arrastra los antídotos para emparejarlos con sus tóxicos correspondientes.</p>
        </div>

        <div className="grid grid-cols-2 gap-8">
          {/* Columna de Tóxicos (estática) */}
          <div className="space-y-2">
            <h2 className="font-bold text-lg mb-4">Tóxicos</h2>
            {toxinData.map(({ toxin }, index) => (
              <div
                key={toxin}
                className="p-3 rounded-lg border-2 border-gray-200 bg-gray-50 text-sm"
              >
                {toxin}
              </div>
            ))}
          </div>

          {/* Columna de Antídotos (arrastrable) */}
          <div className="space-y-2">
            <h2 className="font-bold text-lg mb-4">Antídotos</h2>
            {antidoteOrder.map((antidote, index) => (
              <div
                key={antidote}
                draggable={!showResults}
                onDragStart={() => handleDragStart(antidote)}
                onDragOver={(e) => handleDragOver(e, index)}
                onDrop={(e) => handleDrop(e, index)}
                className={`p-3 rounded-lg border-2 flex items-center gap-2 cursor-move text-sm
                  ${showResults ? 
                    (isCorrect(null, index) ? 'border-green-500 bg-green-50' : 'border-red-500 bg-red-50')
                    : draggedOverIndex === index ? 'border-blue-500 bg-blue-50' : 'border-gray-200'
                  }`}
              >
                <GripVertical className="text-gray-400 flex-shrink-0" size={16} />
                {antidote}
              </div>
            ))}
          </div>
        </div>

        <div className="mt-6 flex justify-center gap-4">
          {!showResults && (
            <Button 
              onClick={() => setShowResults(true)}
              className="bg-blue-500 hover:bg-blue-600"
            >
              Verificar Respuestas
            </Button>
          )}
          
          {showResults && !showAllAnswers && (
            <Button 
              onClick={() => setShowAllAnswers(true)}
              className="bg-green-500 hover:bg-green-600"
            >
              Mostrar Respuestas Correctas
            </Button>
          )}

          <Button 
            onClick={() => {
              setAntidoteOrder(toxinData.map(item => item.antidote).sort(() => Math.random() - 0.5));
              setShowResults(false);
              setShowAllAnswers(false);
            }}
            className="bg-gray-500 hover:bg-gray-600"
          >
            Reiniciar
          </Button>
        </div>

        {showResults && (
          <div className="mt-4 text-center">
            <p className="text-xl font-bold">
              Puntuación: {calculateScore()}%
            </p>
          </div>
        )}

        {showAllAnswers && (
          <div className="mt-6 p-4 bg-green-50 rounded-lg">
            <h2 className="font-bold mb-2">Respuestas Correctas:</h2>
            <div className="grid grid-cols-2 gap-4">
              {toxinData.map(({ toxin, antidote }) => (
                <div key={toxin} className="text-sm">
                  <span className="font-semibold">{toxin}</span> → {antidote}
                </div>
              ))}
            </div>
          </div>
        )}
      </CardContent>
    </Card>
  );
};

export default ToxinAntidoteGame;
