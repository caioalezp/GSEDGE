# 🧠💼 FutureWorkSense  

# Caio Alexandre Ziviani Poci / RM:562256
# Thiago Alessandro Gois Ferreira / RM:562446
# Heitor Rocha Paganotto / RM:561595

### Monitoramento de Ergonomia e Conforto no Futuro do Trabalho

Este projeto apresenta um sistema IoT para monitoramento de **conforto ergonômico** em ambientes de trabalho modernos, Como a falta de conforto e foco pode impactar no trbalho, ilustrando como tecnologias inteligentes irão apoiar o trabalhador no **Futuro do Trabalho**.

O dispositivo coleta dados em tempo real, envia para a nuvem (**ThingSpeak**) e para uma **VM na Azure**, permitindo análise e visualização contínua do nível de conforto.

---

## 🚀 Objetivo

O sistema foi projetado para:

- Medir um **índice de conforto ergonômico** (simulado por um potenciômetro).
- Acender um LED quando o valor indicar desconforto.
- Enviar dados automaticamente para:
  - ThingSpeak (dashboard online)
  - Servidor na Azure (processamento/backend)
- Demonstrar um conceito moderno do Futuro do Trabalho:

> Ambientes inteligentes que ajustam e monitoram o bem-estar do trabalhador.

---

## 🛠️ Tecnologias Utilizadas

- ESP32  
- C++ (Arduino)  
- Wi-Fi  
- ThingSpeak API  
- Azure Virtual Machine (HTTP)  
- Potenciômetro (simulação de sensor de conforto)  
- LED  

---

## 📊 Como o Sistema Funciona

### 🔍 Coleta  
O potenciômetro gera um valor entre **0 e 100**, representando o nível de conforto.

### 🚨 Alerta  
Se o nível estiver **fora do ideal**, o LED acende indicando desconforto e a necessidade de ajuste postural.

### ☁️ Envio para Nuvem  
A cada **15 segundos**, os dados são enviados:

- Para o **ThingSpeak**  
- Para a **Azure VM** em formato JSON  

---

## 📡 Formato das Requisições

### 🔵 ThingSpeak (GET)
 
