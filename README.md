import { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button"; import { Badge } from "@/components/ui/badge";

export default function AviatorPadraoApp() { const [historico, setHistorico] = useState(""); const [pontos, setPontos] = useState([]);

const analisarPadroes = () => { const valores = historico .split(/[,\s]+/) .map((v) => parseFloat(v)) .filter((v) => !isNaN(v));

const resultado = valores.map((valor, i, arr) => {
  if (i >= 3) {
    const ultimos = arr.slice(i - 3, i);
    if (ultimos.every((v) => v < 1.5)) {
      return { valor, alerta: true };
    }
  }
  return { valor, alerta: false };
});

setPontos(resultado);

};

return ( <div className="p-4 max-w-md mx-auto"> <h1 className="text-xl font-bold mb-4 text-center">Detector de Padrões - Aviator</h1> <Card> <CardContent className="space-y-4"> <Input placeholder="Cole aqui os últimos multiplicadores (ex: 1.02 1.10 1.34 ...)" value={historico} onChange={(e) => setHistorico(e.target.value)} /> <Button onClick={analisarPadroes} className="w-full"> Analisar Padrões </Button> <div className="space-y-2 max-h-64 overflow-y-auto"> {pontos.map((p, i) => ( <div key={i} className="flex justify-between items-center"> <span>Rodada {i + 1}: {p.valor.toFixed(2)}x</span> {p.alerta && <Badge variant="destructive">ENTRAR</Badge>} </div> ))} </div> </CardContent> </Card> </div> ); }

