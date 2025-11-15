# 🧠💼 FutureWorkSense  
## Monitoramento Inteligente de Ergonomia e Conforto no Futuro do Trabalho

### Integrantes
- **Caio Alexandre Ziviani Poci — RM: 562256**  
- **Thiago Alessandro Gois Ferreira — RM: 562446**  
- **Heitor Rocha Paganotto — RM: 561595**

---

## 🔗 Links Importantes

- **Simulação no Wokwi:** https://wokwi.com/projects/447620278516273153  
- **Vídeo Explicativo:** *(adicione aqui o link do YouTube quando subir)*

---

## 🔍 1. Descrição do Problema

No ambiente corporativo moderno, desconforto ergonômico, má postura e falta de foco são problemas que reduzem produtividade, aumentam estresse e elevam risco de LER/DORT. A solução atual raramente monitora em tempo real e reage automaticamente. O objetivo deste projeto é demonstrar um protótipo que:

- Monitora níveis de conforto/foco;
- Emite alertas locais imediatos;
- Envia telemetria para análise em nuvem.

---

## 💡 2. Solução Proposta — *FutureWorkSense*

Protótipo baseado em ESP32 que:

- Mede um índice (0–100) de conforto/foco (potenciômetro como simulador);
- Acende um LED quando o índice indicar desconforto/foco baixo;
- Envia os dados para **ThingSpeak** (GET) e para um **endpoint HTTP** (POST) — exemplificado por uma VM ou túnel para seu PC;
- Permite visualização de histórico e integração futura.

---

## 🏢 3. Uso na Vida Real (exemplo prático)

Num escritório, cada estação contém sensores reais (pressão de assento, inclinação, IMU na cadeira, sensor de luz). O sistema detecta postura inadequada, sugere pausas, ajusta automaticamente mobiliário conectado e gera relatórios para RH. Nosso projeto é a prova de conceito dessa arquitetura.

---

## ⚙️ 4. Funcionamento Geral

1. Leitura do potenciômetro → valor 0–100 (nível de conforto/foco).  
2. Se valor ≤ LIMIAR → LED acende e mensagem no Serial.  
3. A cada 15s → envio ao ThingSpeak (GET) e ao servidor (POST JSON).  
4. Dashboards no ThingSpeak e logs na VM/túnel.

---

## 🧪 5. Explicação Técnica

### 5.1 Protocolos escolhidos
- **HTTP GET** para ThingSpeak (compatível por design).  
- **HTTP POST (JSON)** para servidor (Azure VM, Node.js/Flask ou túnel local).  

> Observação: MQTT é excelente para IoT escalável, mas HTTP foi escolhido por simplicidade e compatibilidade com ThingSpeak + protótipo.

### 5.2 ThingSpeak
- Endpoint: `https://api.thingspeak.com/update?api_key=YOUR_KEY&field1=VALUE`  
- Limite: 1 atualização por canal a cada 15 segundos (por isso o delay no código).

### 5.3 Endpoint HTTP (VM / túnel)
- Método: POST  
- Header: `Content-Type: application/json`  
- Payload exemplo:
```json
{
  "focus_level": 72.4,
  "timestamp": "2025-11-15T12:00:00Z",
  "device": "espressif_esp32_01"
}
```
### 5.4 Server.js
```
/**
const express = require('express');
const fs = require('fs');
const app = express();

app.use(express.json()); // habilita body JSON

app.post('/', (req, res) => {
  const body = req.body;
  console.log('Recebido:', body);

  // opcional: guardar em arquivo de log
  fs.appendFileSync('receipts.log', `${new Date().toISOString()} ${JSON.stringify(body)}\n`);

  res.status(200).json({ status: 'ok' });
});

// Porta 80 ou 3000 (ajuste conforme firewall)
const PORT = process.env.PORT || 80;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```
### 5.5 .PY
```
# server_flask.py
# Usage:
#   pip install flask
#   python server_flask.py

from flask import Flask, request, jsonify
import datetime

app = Flask(__name__)

@app.route('/', methods=['POST'])
def receber():
    dados = request.get_json()
    print("Recebido:", dados)
    with open('receipts.log', 'a') as f:
        f.write(f"{datetime.datetime.utcnow().isoformat()} {dados}\n")
    return jsonify({'status': 'ok'}), 200

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)  


```
