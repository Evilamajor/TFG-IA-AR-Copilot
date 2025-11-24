# TFG – IA AR Copilot  
## Assistència mèdica amb ulleres de Realitat Augmentada i Intel·ligència Artificial local-first

Aquest projecte forma part del Treball de Final de Grau d’Enginyeria Informàtica. L’objectiu és dissenyar un **sistema d’assistència mèdica** basat en ulleres de **Realitat Augmentada (AR)** amb un **copilot d’Intel·ligència Artificial** capaç de proporcionar informació clínica rellevant en temps real, minimitzant la latència i garantint la seguretat del pacient.

---

## 🎯 Objectius del projecte

- Desenvolupar un **model funcional** d’assistent clínic basat en AR i IA.
- Integrar un sistema de **reconeixement de veu local** (Whisper o variants optimitzades).
- Aplicar **mecanismes de consistència eventual** propis de sistemes distribuïts.
- Explorar la viabilitat d’un sistema **local-first**, privat i segur.
- Simular o prototipar un **flux de suport clínic** (protocols, alertes, assistència contextual).
- Crear una arquitectura modular que permeti **l’escalat i l’evolució futura** del projecte.

---

## 📌 Abast del projecte (versió inicial)

La versió presentada en el TFG inclou:

- Una **arquitectura conceptual** i prototip funcional.
- Implementació de microserveis bàsics:
  - Processament de veu
  - RAG mèdic amb protocols clínics
  - Motor d’assistència contextual
- Interfície AR minimalista (prototiipada).
- Gestió d’esdeveniments amb principis de **consistència eventual**.

Queda **fora** de l’abast del TFG:

- Integració amb sistemes reals del ICS / CatSalut.
- Desplegament comercial.
- Hardware AR específic (HoloLens, RealWear…).
- Validacions clíniques reals.

L’abast detallat es pot consultar a:  
📄 `01_requirements/01_scope.md`

---

## 🏗️ Arquitectura prevista (resum)

El sistema es basa en:

- Un conjunt de **microserveis d’IA** executats localment o en entorn híbrids.
- Un **motor de coordinació** que aplica algoritmes de tipus TSAE i consistent hashing.
- Un **visor AR** que mostra informació adaptada al context mèdic.

Diagrames detallats a:  
📄 `02_architecture/`

---

## 📂 Estructura del repositori

