# TFG – IA AR Copilot

## Introducció
Aquest repositori recull el Treball de Final de Grau (TFG) d’Enginyeria Informàtica de la UOC dedicat al disseny d’un **copilot clínic en Realitat Augmentada (RA)** amb suport d’**Intel·ligència Artificial local-first**. El projecte persegueix oferir a professionals sanitaris informació clínica rellevant en temps real mitjançant ulleres de RA, minimitzant la dependència de la xarxa, la latència i els riscos de privacitat. L’objectiu global és validar la viabilitat tècnica i metodològica d’un ecosistema distribuït, modular i segur que faciliti la presa de decisions assistencials.

## Objectius del TFG
- Definir una arquitectura de microserveis que integri components d’IA (LLM, RAG mèdic, detecció de paraula clau) i processament de veu local o hibrid.
- Establir un flux de dades local-first amb **TSAE (Topology-Sensitive Anti-Entropy)**, **hashing consistent** i **consistència eventual**, assegurant tolerància a fallades i escalabilitat.
- Prototipar una interfície RA minimalista que presenti alertes, protocols clínics i assistència contextual en temps real.
- Desenvolupar un set de proves i mètriques per avaluar latència, exactitud del reconeixement de veu i usabilitat del copilot.
- Documentar la metodologia, les consideracions ètiques i les línies de treball futur.

## Estructura del repositori
- `00_docs/`: Antecedents, marc teòric i documentació general de suport.
- `01_requirements/`: Requeriments funcionals i no funcionals, inclòs l’abast del projecte i els casos d’ús prioritzats.
- `02_architecture/`: Diagrames i descripcions de l’arquitectura lògica, física i de desplegament (microserveis, comunicacions, coherència de dades).
- `03_data_models/`: Esquemes de dades, definicions de models i guies de normalització/anonimització.
- `04_behavior/`: Models de comportament, fluxos d’estat i lògiques de coordinació del copilot.
- `05_ui_ux/`: Prototips de RA, disseny d’interacció i pautes d’experiència d’usuari.
- `06_validation/`: Pla de validació, proves unitàries, mètriques i instruments d’avaluació.
- `07_documentation/`: Plantilles de memòria, guies d’ús i materials complementaris.
- `README.md`: Document introductori i guia d’orientació del repositori.
- `desktop.ini`: Fitxer auxiliar sense impacte en el projecte.

## Dependències i requisits tècnics
- **Entorn base:** Python 3.10+, Node.js 18+ (per prototips de client), Docker 24+ per a orquestració local.
- **IA i veu:** models Whisper (tiny/base) o equivalents optimitzats, LLM compactes (p. ex. GPT4All o Vicuna local), llibreries `torch`, `transformers` i `faster-whisper` quan sigui pertinent.
- **Comunicació i coordinació:** gRPC/REST per als microserveis, Redis o NATS per a cues d’esdeveniments, i un anell de hashing consistent per al repartiment de càrrega.
- **RA i prototips:** entorn WebXR (Three.js + A-Frame) o SDK del dispositiu RA triat, amb suport per a renderitzat lleuger.
- **Sistema operatiu recomanat:** Ubuntu 22.04 LTS amb accés a GPU opcional per accelerar el processament de veu i LLM locals.

### Execució dels microserveis d’IA
1. **Desplegament local amb Docker Compose** (exemple orientatiu):
   ```bash
   docker compose -f 02_architecture/docker-compose.yml up --build
   ```
   - `ia-gateway`: orquestrador REST/gRPC que aplica lògica de consistència eventual i gestiona la cache local-first.
   - `asr-service`: microservei de reconeixement de veu amb Whisper optimitzat.
   - `rag-service`: servei RAG amb index local (FAISS) i embeddings biomèdics.
   - `context-engine`: motor de context que aplica polítiques TSAE i sincronització basada en versions vectorials.
2. **Execució directa (sense Docker)**: crear un virtualenv, instal·lar dependències (`pip install -r requirements.txt`) i arrencar cada servei amb els scripts documentats a `02_architecture/`.

### Processament de veu
- Cal disposar de models Whisper descarregats prèviament (`WHISPER_MODEL_PATH`) i, opcionalment, d’acceleració GPU (CUDA). Els scripts de l’`asr-service` permeten configurar el mode streaming i la detecció de paraula clau per activar el copilot sense contacte.

### Prototips d’AR
- Els prototips WebXR es poden servir amb `npm install` i `npm run dev` dins de `05_ui_ux/`. Per a dispositius RA específics, adaptar el paquet segons l’SDK (p. ex. HoloLens). Un servidor local (`localhost:3000`) exposa les escenes de prova amb superposició d’alertes i checklists.

## Arquitectura del sistema
L’ecosistema adopta un **model de microserveis** per aïllar responsabilitats i facilitar el desplegament híbrid (edge + cloud). Les decisions clau són:
- **Consistència eventual i TSAE:** la sincronització asíncrona basada en intercanvi d’estats i versions vectorials minimitza la latència i tolera desconnexions, coherent amb l’ús en entorns hospitalaris amb cobertures irregulars.
- **Hashing consistent:** garanteix un repartiment equilibrat de càrrega entre instàncies del `context-engine` i del `rag-service`, reduint reubicacions de dades en escenaris d’escalat dinàmic.
- **Local-first:** priorització de caches locals (notes, protocols, embeddings) per operar sense dependència de la xarxa, amb replicació eventual cap al backend quan la connectivitat és disponible.
- **Separació funcional:** microserveis per veu, RAG, context i interfície, comunicant-se via gRPC/REST i un bus d’esdeveniments; això simplifica actualitzacions independents i la validació modular.
- **Seguretat per disseny:** xifrat de canal (TLS), control d’accessos per token i limitació de dades sensibles al perímetre local, reduint riscos de filtració.

## Dades i privacitat
- **Tipus de dades:** transcripcions de veu, metadades de sessió, protocols clínics públics i esdeveniments contextuals (alertes, checklists). No s’inclouen dades identificatives reals de pacients.
- **Generació sintètica:** els conjunts de prova deriven de dades simulades o anonimitzades per validar el comportament del copilot sense comprometre la privacitat.
- **Anonimització:** eliminació de camps identificatius, pseudonimització d’usuari i filtratge de paraules clau sensibles en el pipeline de veu.
- **Consideracions ètiques:** ús exclusiu per a recerca acadèmica; qualsevol desplegament real requeriria aprovació ètica, conformitat RGPD i avaluació clínica formal. Les respostes de l’IA no substitueixen el criteri mèdic.

## Pla de validació
- **Proves unitàries i d’integració:** cobertura dels microserveis (`asr-service`, `rag-service`, `context-engine`) mitjançant suites automatitzades. Les proves verifiquen contractes API, coherència de versions i gestió de fallades.
- **Mètriques clau:**
  - Latència end-to-end (captura d’àudio → resposta de l’assistent) en ms.
  - Exactitud del reconeixement de veu (WER) per a comandes clíniques i terminologia específica.
  - Taxa d’actualització del context (temps de convergència TSAE) i consistència observada.
- **Proves d’usabilitat:** sessions amb escenaris simulats, recollint temps de tasca, taxa d’errors i satisfacció (SUS). Feedback per iterar en disseny RA i diàlegs del copilot.
- **Validació del comportament del copilot:** verificació de respostes a protocols (per exemple, suport a maniobra BLS/ALS), detecció de paraula clau i adaptació contextual (alertes en temps real segons senyals d’esdeveniments).

## Escenari d’ús exemplar
1. El metge activa el copilot pronunciant la paraula clau configurada; el `asr-service` inicia el mode streaming.
2. La transcripció s’envia al `context-engine`, que resol l’estat clínic i consulta el `rag-service` per recuperar protocols aplicables.
3. El visor de RA mostra un checklist contextual (p. ex. via WebXR) amb passos prioritzats i alertes de risc.
4. Les interaccions posteriors (veu o gestos) actualitzen l’estat; el sistema replica els canvis de manera eventual cap a altres nodes o al backend, mantenint la consistència del context compartit.

## Metodologia del projecte
- **Enfocament iteratiu-incremental** amb sprints curts per validar funcionalitats de veu, RA i coherència distribuïda.
- **Modelatge previ:** definició de requisits i casos d’ús (`01_requirements/`), seguit de prototipat ràpid i proves de concepte.
- **Infraestructura com a codi:** definició de serveis i xarxa en fitxers de composició; automatització de desplegament i proves.
- **Avaluació contínua:** integració de mètriques de rendiment i qualitat en cada iteració, amb traçabilitat de decisions d’arquitectura.

## Resultats esperats i treballs futurs
- **Resultats esperats:** demostració d’un pipeline complet (veu → context → RA), evidència de latència reduïda gràcies al local-first i proves inicials d’usabilitat que confirmin la utilitat del copilot.
- **Treballs futurs:** integració amb sensors biomèdics, millora de models de llenguatge específics en català/castellà, optimització de l’alineament multimodal, integració amb EHR mitjançant FHIR i validacions clíniques amb usuaris reals.

## Informació del projecte
- **Cronograma:** planificació en fases (anàlisi, disseny, implementació, validació) descrita a `07_documentation/cronograma.md`.
- **Autor:** Estudiant del Grau d’Enginyeria Informàtica – UOC.
- **Llicència:** Mitjans i codi subjectes a llicència acadèmica occidentalitzada (revisar `LICENSE` si escau). Ús limitat a finalitats docents i de recerca.
- **Contribucions:** S’accepten propostes de millora mitjançant issues o forks. Cal mantenir l’enfocament en privacitat, local-first i compatibilitat amb RA.
