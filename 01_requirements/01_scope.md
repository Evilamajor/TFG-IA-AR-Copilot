# Abast del TFG: "TFG – IA AR Copilot"

## 1. Descripció general del projecte
El TFG proposa el disseny i prototipat d’un copilot mèdic integrat en unes ulleres de realitat augmentada (AR) per a metges de família. El sistema combina agents d’IA, microserveis i un model de computació local-first per assistir el professional durant la consulta, tot assegurant privacitat, disponibilitat i eficiència. L’objectiu és oferir suport contextual en temps real, reduir la càrrega cognitiva i millorar la qualitat de l’atenció sense dependre completament de serveis al núvol.

## 2. Abast funcional
### 2.1 Funcionalitats principals del sistema
- Assistència contextual proactiva durant l’entrevista clínica (preguntes suggerides, checklist de símptomes i factors de risc).
- Reconeixement de veu i transcripció en temps real orientada a comandes i dictat clínic.
- Recuperació augmentada amb coneixement (RAG) sobre protocols, guies i literatura mèdica prèviament indexada.
- Generació d’explicacions i resum de la consulta per suport a la decisió, mantenint traçabilitat de fonts.
- Visualització AR de recomanacions, alertes i passos següents sense interrompre el flux assistencial.
- Gestió d’estat de sessió local amb sincronització eventual cap a repositoris de referència.

### 2.2 Microserveis previstos
- **Orquestrador clínic**: coordina els agents d’IA, aplica la lògica de flux i assegura la coherència de l’estat.
- **Servei de veu local-first**: captura i transcriu audio de forma local amb models de reconeixement offline; opcionalment envia segments anonimitzats al núvol per millorar models.
- **Servei RAG mèdic**: gestiona l’indexació semàntica, la recuperació de documents i l’alineament amb el context de consulta.
- **Servei d’explicabilitat**: genera justificacions, cita fonts i gestiona el nivell de confiança de les recomanacions.
- **Servei d’integració AR**: renderitza overlay en ulleres, sincronitza comandes gestuals/voçals i assegura baixa latència visual.
- **Servei de seguretat i privacitat**: controla permisos, anonimització local, xifrat d’estat i rotació de claus.

### 2.3 Components AR i components d’IA
- **Components AR**: motor de renderitzat HUD, gestió de capes (text, gràfics, alertes), interacció gestual/veu, buffers de contingut per latència reduïda, adaptació a context d’il·luminació i postura del metge.
- **Components d’IA**: models de llenguatge per generació i resum, embeddings clínics per RAG, models d’ASR local, models de detecció d’intenció i classificació de símptomes, agents especialitzats (orquestrador, verificador de fonts, resumidor).

### 2.4 Reconeixement de veu local-first
- Processament prioritari a dispositiu o a passarel·la edge de la consulta per reduir latència i exposició de dades.
- Mode degradat: si la capacitat local és insuficient, enviament xifrat a un endpoint privat amb token efímer i anonimització de dades sensibles.
- Cache local de vocabulari mèdic i models adaptatius per accent/dialecte.

### 2.5 RAG mèdic i assistència contextual
- Indexació prèvia de protocols clínics (p. ex. guies de l’ICS, NICE, WHO) en un vector store local amb replicació opcional.
- Recuperació contextual segons motius de consulta, anamnesi i signes vitals disponibles.
- Filtres de confiança i explicabilitat: cada resposta ha d’anar acompanyada de citació de la font i nivell de risc.
- Suport a preguntes proactives i alertes de banderes vermelles segons criteris d’urgència.

## 3. Abast no funcional
### 3.1 Rendiment, latència i disponibilitat
- Latència objectiu per a respostes visuals: < 150 ms en interaccions locals; < 500 ms quan hi ha consulta a RAG.
- Disponibilitat esperada del prototip: 99% en sessions de prova, amb tolerància a fallades puntuals de xarxa.
- Escalabilitat limitada al nombre de consultes simultànies de la consulta pilot (1–3 sessions). No es contempla escala massiva.

### 3.2 Privacitat, seguretat i processament local-first
- Dades clíniques processades localment sempre que sigui possible; xifrat en repòs i en trànsit per a sincronització eventual.
- Polítiques d’accés mínim necessari, tokens d’accés efímers i registre d’auditoria local.
- Anonimització dels fragments enviats al núvol, amb redacció de PII i dissociació de l’identificador de pacient.

### 3.3 Justificació de consistència eventual
- El model de consistència eventual permet operar offline o amb connectivitat limitada, prioritzant la disponibilitat en consulta.
- Els estats de sessió i notes es reconcilien mitjançant hashing consistent i vectors de versió; els conflictes es resolen amb polítiques mèdiques definides (prioritat al dispositiu local o al registre més recent).
- La consistència forta només s’exigeix en operacions crítiques (consentiment, signatura de documents) que queden fora del prototip.

## 4. Funcionalitats incloses i excloses
### 4.1 Incloses
- Assistència a anamnesi i exploració bàsica en atenció primària.
- RAG clínic sobre corpus prèviament validat i limitat a top 100 patologies comunes.
- Reconeixement de veu local-first amb models preentrenats i adaptatius.
- Visualització AR de recomanacions i alertes.
- Mode degradat amb consistència eventual i sincronització quan hi ha connexió.

### 4.2 Excloses
- Integració amb dispositius mèdics hardware (ECG, oxímetre, monitor multiparamètric).
- Prescripció electrònica i signatura legal de documents.
- Diagnòstic automàtic sense supervisió humana; el sistema només proposa i justifica.
- Gestió de múltiples especialitats o entorns hospitalaris (l’abast és atenció primària).
- Monitoratge continu de pacient o telemedicina en remot.

## 5. Limitacions reals del prototip
- Capacitat limitada de còmput local a l’ullera/edge, condicionant la mida dels models utilitzats.
- Dependència de corpus prèviament indexat; no s’inclou ingestió contínua de noves fonts.
- Qualitat de reconeixement de veu subjecta a soroll ambiental i variabilitat d’accent.
- Validació en entorn simulat o pilot reduït, sense dades reals de pacients.
- Absència de certificació reguladora (CE, FDA); l’ús es limita a recerca i prova de concepte.

## 6. Rellevància respecte als sistemes distribuïts
- **TSAE (Threaded/Transactional State And Eventual consistency)**: el prototip adopta un model de sincronització asíncrona per sessions clíniques, permetent continuïtat d’operació sense bloquejos.
- **Hashing consistent**: utilitzat per distribuir claus d’indexos del vector store entre nodes edge o rèpliques, reduint reassignacions en afegir/eliminar nodes.
- **Arquitectura local-first**: prioritza el processament i l’emmagatzematge local, amb sincronització eventual i resolució de conflictes basada en versions, per garantir privacitat i disponibilitat.

## 7. Relació amb els criteris d’avaluació del TFG de la UOC
- **Qualitat tècnica**: definició d’arquitectura distribuïda, justificació de consistència eventual i ús d’algorismes de hashing consistent.
- **Rigor metodològic**: abast clar, criteris d’inclusió/exclusió i modelització de microserveis i agents.
- **Innovació i originalitat**: combinació d’AR, IA generativa i RAG mèdic amb enfoc local-first.
- **Aplicabilitat**: orientat a atenció primària, amb funcionalitats alineades amb necessitats clíniques reals.
- **Qualitat de la documentació**: redacció formal, traçabilitat i coherència amb plans de validació i arquitectura.
