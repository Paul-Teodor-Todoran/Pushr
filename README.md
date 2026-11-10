# Pushr

Pushr este o aplicație web pentru planificarea antrenamentelor de forță și fitness, înregistrarea rezultatelor și urmărirea progresului personal.

## Problema abordată

Notițele, foile de calcul și aplicațiile separate fragmentează informațiile despre programe și performanțe. Pushr reunește planificarea, jurnalul sesiunilor și analiza rezultatelor într-un singur loc, asociat contului utilizatorului.

## Obiective

- Crearea și modificarea programelor personale de antrenament.
- Înregistrarea seriilor, repetărilor și greutăților efectuate în fiecare sesiune.
- Păstrarea unui istoric verificabil al rezultatelor.
- Identificarea automată a recordurilor personale (PR).
- Prezentarea evoluției prin grafice și statistici.

## Utilizatori vizați

Persoane care frecventează săli de fitness, de la începători la practicanți experimentați, care vor să își organizeze antrenamentele și să își urmărească performanțele. Prima versiune gestionează exclusiv datele personale; rolurile de antrenor și funcțiile sociale nu fac parte din scopul inițial.

## Echipa

| Membru | Contact | Rol și responsabilități |
| --- | --- | --- |
| Todoran Paul | paul.todoran05@e-uvt.ro | Project Management / QA / Fullstack Dev: planificare, integrare frontend–backend, teste de acceptanță și coordonarea livrărilor. |
| Tatarasanu Timea | timea.tatarasanu05@e-uvt.ro | Frontend Dev / Designer: ecrane, navigare, interfață responsive, formulare și vizualizarea progresului. |
| Solia Anemona | anemona.solia05@e-uvt.ro | Backend Dev / DB Dev: API REST, autentificare, schema bazei de date, persistență și calculul statisticilor/PR. |

## Funcționalități propuse

Versiunea minimă livrabilă (MVP) include toate funcționalitățile obligatorii:

- Înregistrare, autentificare, deconectare și gestionarea profilului.
- Bibliotecă de exerciții cu filtrare după grupa musculară și tip.
- Workout Planner: programe, zile de antrenament, exerciții, serii și repetări planificate.
- Workout Logger: înregistrarea rezultatelor efective și finalizarea sesiunilor.
- Workout History: listă cronologică și detaliile sesiunilor salvate.
- PR Tracking: evidențierea celei mai mari greutăți efectuate pentru fiecare exercițiu.
- Progress Dashboard: greutate maximă, volum și frecvență, filtrate pe perioadă.

Extensiile opționale sunt șabloanele explicite de antrenament, compararea perioadelor, exercițiile personalizate și realizările pentru consecvență. Reutilizarea unui program existent face parte din MVP; salvarea lui într-o colecție separată de șabloane este opțională.

Aplicația nu oferă diagnostic, interpretare clinică, recomandări medicale automate sau programe generate din evaluări medicale.

## Tehnologii alese

| Zonă | Tehnologii | Utilizare |
| --- | --- | --- |
| Frontend | React, TypeScript | Interfață interactivă și contracte de date tipizate. |
| Stilizare | Tailwind CSS | Ecrane responsive și stiluri consistente. |
| Grafice | Recharts | Vizualizarea progresului din datele furnizate de API. |
| Backend | Node.js, Express | API REST, validare și logică de business. |
| Autentificare | JWT | Identificarea utilizatorului în cererile autentificate. |
| Bază de date | PostgreSQL | Persistența datelor și relațiilor dintre acestea. |

Echipa are experiență anterioară cu aceste tehnologii. Alegerea este preliminară; eventualele modificări vor fi documentate înainte de implementare.

## Documentație

- [Cerințe și priorități](docs/requirements.md)
- [Scenarii de utilizare și criterii de verificare](docs/use-cases.md)
- [Specificații textuale ale ecranelor și navigării](docs/wireframes/README.md)
- [Arhitectura propusă](docs/architecture.md)
- [Model de date și endpoint-uri](docs/data-model.md)
- [Plan de implementare și responsabilități](docs/implementation-plan.md)

Repository: [Paul-Teodor-Todoran/Pushr](https://github.com/Paul-Teodor-Todoran/Pushr).
