# 📌 Projecte: Copilot IA + Ulleres AR per metges de família

## 🎯 Objectiu general
Desenvolupar un copilot d’intel·ligència artificial integrat en unes ulleres de realitat augmentada per oferir assistència mèdica contextual al metge de família, preservant la privacitat i seguretat de les dades.

Com treballar-ho: mantén l’objectiu curt, operatiu i revisa’l quan anotis una tasca nova: “Aquesta tasca acosta el projecte a l’objectiu? Sí/No”.

## 🧭 Abast
- Àmbit: consultes d’atenció primària.
- Patologies: top 100 més comunes.
- Funcionalitats: preguntes proactives, reconeixement de veu, accés a protocols mèdics, suport a la presa de decisions.

Com treballar-ho: afegeix criteris de “dins/fora” (in/out). Exemple:

Dins: suggeriments de preguntes en funció de símptomes comuns (tos, febre, dolor toràcic…).

Fora (per ara): integració amb dispositius mèdics hardware (ECG, oxímetres).

## 🧱 Fases del projecte
1. Requisits i modelització
2. Arquitectura tècnica
3. Desenvolupament i integració IA local
4. Validació, simulació i documentació final

Cada fase ataca un problema diferent:

Requisits → Què volem fer

Arquitectura → Com ho farem a alt nivell

Desenvolupament → Implementació tècnica

Validació → Ha funcionat com esperàvem?

## 🧑‍⚕️ Actors principals
- Metge de família
- Sistema IA local (agents M1–M5)
- Repositoris mèdics i protocols clínics

## 📅 Planificació inicial
| Fase | Durada | Objectius principals |
|------|---------|-----------------------|
| Requisits | Octubre 2025 | Model de casos d’ús, BPMN |
| Arquitectura | Novembre 2025 | C4 model complet |
| Implementació | Desembre - Març | Agents IA i interfície AR |
| Validació | Abril - Juny | Tests i documentació |

Fase inicial clàssica
Defineix requisits, arquitectura i diagrames amb claredat (molt útil per avaluació acadèmica i presentació del TFG).

Iteracions incrementals sobre la base establerta
Un cop tens M5 i els agents definits, pots implementar-los un per un en iteracions de curta durada:

Sprint 1 → Orquestrador + reconeixement de veu (M5 + M4)

Sprint 2 → Preguntes suggerides (M3)

Sprint 3 → Consultes RAG (M2)

Sprint 4 → Integració UI AR

Sprint 5 → Validació i ajustos finals

Validació contínua
Cada sprint inclou proves i retroalimentació. Si descobreixes un error conceptual, tornes enrere i ajustes l’arquitectura si cal.