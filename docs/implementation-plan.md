# Plan de implementare

## Justificarea tehnologiilor

Alegerea pornește de la experiența anterioară a echipei, pentru a concentra timpul pe funcționalități și integrare. Versiunile exacte și managerul de pachete vor fi fixate la inițializarea codului, împreună cu fișierul lock.

| Tehnologie | Motivație în Pushr |
| --- | --- |
| React | Ecrane interactive compuse din elemente reutilizabile pentru programe, exerciții și serii. |
| TypeScript | Tipuri pentru datele API și formulare; propus atât în frontend, cât și în backend pentru contracte consistente. |
| Tailwind CSS | Implementarea rapidă a unui sistem vizual coerent și a variantelor pentru mobil. |
| Recharts | Integrarea vizualizărilor progresului în componente React, cu tabel textual complementar. |
| Node.js și Express | API REST modular într-un ecosistem JavaScript/TypeScript familiar echipei. |
| JWT | Identificarea sesiunii în cereri; combinat cu sesiuni revocabile în DB pentru logout efectiv. |
| PostgreSQL | Relații între programe și rezultate, constrângeri și tranzacții pentru finalizare și PR. |

Nu se introduc inițial servicii distribuite sau funcții opționale care ar întârzia fluxul complet de bază.

## Organizarea propusă a codului

La inițializarea implementării se propun directoarele `frontend/`, `backend/` și `docs/`. Backend-ul va conține module pentru conturi, catalog, programe, sesiuni și progres, plus migrații și seed-uri. Contractele API se documentează înaintea integrării; tipurile comune pot fi extrase ulterior dacă apar duplicări relevante.

## Versionare și colaborare

- `main` păstrează modificări revizuite și verificabile. Fiecare activitate se dezvoltă pe o ramură scurtă, precum `feat/workout-logger`, `fix/pr-calculation` sau `docs/api-contract`.
- Commit-urile viitoare vor fi mici și descriptive, cu prefixe `feat:`, `fix:`, `docs:`, `test:` sau `chore:`. Acestea sunt reguli de lucru propuse, nu acțiuni executate în această etapă.
- Integrarea se face prin pull request, revizuit de cel puțin un alt membru. Autorul descrie comportamentul, cerințele acoperite și verificările efectuate.
- Înaintea integrării: verificare TypeScript, lint, build și testele relevante. Migrațiile DB intră în aceeași schimbare cu logica dependentă și sunt probate pe o bază de test.
- Se păstrează fișierul lock în Git și se exclud secretele și fișierele generate. Datele de test folosesc conturi fictive.
- Reperele pot primi etichete `v0.1.0` (conturi și catalog), `v0.2.0` (planificare și jurnal), `v0.3.0` (PR și progres), `v1.0.0` (MVP validat). API-ul pornește la `/api/v1`; schimbările incompatibile de contract se coordonează cu frontend-ul.

## Etape și priorități

Planul este estimativ, în șase iterații de aproximativ o săptămână, fără date calendaristice asumate. Capacitatea reală și termenul de predare pot ajusta durata; toate cerințele Must rămân necesare pentru MVP.

| Etapă | Prioritate și activități | Responsabil principal / sprijin | Dependențe | Condiție de încheiere |
| --- | --- | --- | --- | --- |
| I1 — Fundație | P0: fixarea contractelor, structura proiectului, schema inițială, migrații, schelet navigare, configurarea verificărilor. | Paul coordonează; Anemona DB/API; Timea UI. | Documentația preliminară. | Proiectele pornesc local; migrațiile rulează pe o bază goală; convențiile și ecranele sunt revizuite. |
| I2 — Conturi și catalog | P0: register/login/logout, profil, autorizare, seed exerciții și filtre. | Anemona backend; Timea ecrane; Paul integrare și QA. | I1. | UC-01 trece; filtrele funcționează; accesul între conturi este respins. |
| I3 — Planificare | P0: programe, zile, exerciții ordonate și ținte; editor complet. | Timea frontend; Anemona persistență; Paul integrare. | I2 și contracte programe. | UC-02 trece; salvarea este atomică și datele persistă după reîncărcare. |
| I4 — Logger și istoric | P0: pornire sesiune, copie a planului, salvare/reluare serii, finalizare și istoric. | Anemona logică și tranzacții; Timea logger/istoric; Paul QA. | I3. | Sesiunile pot fi salvate și consultate fără pierderea datelor; validările și repetarea cererilor sunt verificate. |
| I5 — PR și progres | P0: integrarea PR în finalizare, evenimente, agregări, dashboard și vizualizări. | Anemona calcule; Timea dashboard; Paul teste și integrare. | I4. | UC-03 și UC-04 trec integral; concurența la finalizare și valorile numerice sunt verificate. |
| I6 — Stabilizare și livrare | P0: remedieri, accesibilitate, verificări responsive, securitate, configurare găzduire și instrucțiuni reale de instalare. | Paul QA/livrare; Timea UI; Anemona API/DB și backup. | I1–I5. | Toate cerințele Must au dovadă de acceptanță; instalarea este reproductibilă și documentația reflectă codul. |
| După MVP | P1: șabloane și comparare perioade; P2: exerciții proprii și realizări. | Repartizare în funcție de capacitate, coordonată de Paul. | MVP acceptat. | Criteriile CF-13–16 sunt detaliate și implementate doar în timpul disponibil. |

## Verificări de acceptanță

Paul menține o listă care asociază fiecare cerință Must cu o verificare și rezultatul acesteia. Timea verifică ecranele la 360, 768 și 1440 px și navigarea cu tastatura. Anemona verifică migrațiile, autorizarea și tranzacțiile în baza de test.

Se propun teste unitare pentru formulele de volum/PR și intervale, teste de integrare pentru conturi, izolare între utilizatori, programe și finalizare idempotentă/concurentă, plus teste complete ale UC-01–04. Se includ cazuri cu greutate zero, record egal, prima performanță, sesiune goală și limite de zi/fus orar. Verificarea performanței folosește țintele CNF-08; nu se afirmă îndeplinirea lor înainte de măsurare.

## Riscuri și măsuri

| Risc | Măsură |
| --- | --- |
| Extinderea excesivă a scopului | Funcțiile opționale încep numai după acceptarea cerințelor Must. |
| Contracte diferite între frontend și backend | Exemple de cereri/răspunsuri și integrare la fiecare etapă, cu revizie comună. |
| Date pierdute sau duplicate la reîncercare | Confirmări explicite de salvare, upsert pentru serii și finalizare tranzacțională idempotentă. |
| PR/statistici incorecte | O singură definiție în backend și seturi de date de referință cu rezultate calculate manual. |
| Istoric afectat de modificarea programelor | Copii ale configurației în sesiuni și teste după editarea programului. |
| Timp insuficient înaintea predării | Rezervarea I6 pentru stabilizare și eliminarea extensiilor înainte de reducerea funcțiilor obligatorii. |

## Rezultatul acestei etape

Se livrează documentația preliminară, inclusiv wireframe-urile, mockup-urile și diagramele. Implementarea și publicarea aplicației, precum și configurarea regulilor GitHub, sunt activități viitoare. Planul descrie pașii următori ai echipei.
