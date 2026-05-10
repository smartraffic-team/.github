<p align="center">
   <img src="https://github.com/smartraffic-team/artefacts/blob/main/logo.png">
</p>

**Sistema de controle de tráfego inteligente** que utiliza visão computacional para detectar veículos e pedestres em tempo real, controlando semáforos de forma autônoma via integração com Arduino.

---

## Descrição

O **SmarTraffic** foi desenvolvido para otimizar a mobilidade urbana. Utilizando modelos de detecção de objetos (YOLO) para identificar diferentes agentes no trânsito e tomar decisões dinâmicas sobre o estado dos semáforos.

---

## Tecnologias

| Camada          | Tecnologias                                  |
|----------------|----------------------------------------------|
| Visão Computacional | Python, YOLO (Ultralytics), OpenCV          |
| Comunicação Serial  | PySerial                                    |
| Hardware            | Arduino, LEDs (verde, amarelo, vermelho)   |
| Firmware            | C++ (Arduino IDE)                          |

---

## Funcionalidades

- Detecção em tempo real de:
  - Veículos
  - Pedestres 
- Envio de comandos via serial (Python → Arduino)
- Controle autônomo do semáforo (verde → amarelo → vermelho)
- Lógica com **limiar de ativação** para evitar trocas constantes de estado

---

## Estrutura do Repositório

O projeto está organizado em **4 repositórios principais**:
```
smartraffic/
├── computer-vision/ -> Python + YOLO + OpenCV
├── arduino/ -> Firmware C++ para Arduino
├── artefacts/ -> Artefatos do projeto (diagramas, imagens, etc.)
└── docs/ -> Website do projeto + documentação técnica
```
---

## Fluxograma

```mermaid
flowchart TD
    A[Início] --> B[Capturar frame da câmera]
    B --> C[Rodar detecção YOLO]
    C --> D{Detectou PCD?}
    
    D -- Sim --> E[Comando: VERMELHO para veículos]
    E --> F[Estende tempo de travessia]
    F --> G[Registra evento de prioridade máxima]
    G --> B

    D -- Não --> H{Detectou pedestre comum?}
    H -- Sim --> I[Ciclo atual termina?]
    I -- Sim --> J[Libera travessia pedestres]
    J --> B
    I -- Não --> K[Aguarda fim do ciclo]
    K --> B

    H -- Não --> L{Detectou veículo?}
    L -- Sim --> M[Fluxo contínuo para veículos]
    M --> B

    L -- Não --> N[Mantém estado atual do semáforo]
    N --> B
```
