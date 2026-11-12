
# Diagrama arhitecturii – Pushr

## Descriere

Aplicația Pushr utilizează o arhitectură de tip monolit modular, organizată în trei componente principale: frontend, backend și baza de date.

Frontend-ul este dezvoltat folosind React și TypeScript, backend-ul utilizează Node.js și Express, iar datele sunt stocate într-o bază de date PostgreSQL.

Componentele comunică prin intermediul API-urilor REST, utilizând protocolul HTTP/HTTPS și formatul JSON pentru schimbul de date.

## Diagrama arhitecturii

```mermaid
flowchart TB

    USER["Utilizator"]

    subgraph FRONTEND["FRONTEND – React + TypeScript + Tailwind CSS"]
        UI["Interfață utilizator"]
        AUTH_UI["Autentificare și profil"]
        PROGRAM_UI["Gestionarea programelor"]
        WORKOUT_UI["Înregistrarea antrenamentelor"]
        PROGRESS_UI["Istoric și progres"]
        CHARTS["Grafice – Recharts"]

        UI --> AUTH_UI
        UI --> PROGRAM_UI
        UI --> WORKOUT_UI
        UI --> PROGRESS_UI
        PROGRESS_UI --> CHARTS
    end

    subgraph BACKEND["BACKEND – Node.js + Express"]
        API["REST API"]
        AUTH["Modul autentificare și autorizare"]
        USERS["Modul utilizatori"]
        EXERCISES["Modul exerciții"]
        PROGRAMS["Modul programe"]
        WORKOUTS["Modul antrenamente"]
        RECORDS["Modul recorduri personale"]
        HISTORY["Modul istoric și statistici"]

        API --> AUTH
        API --> USERS
        API --> EXERCISES
        API --> PROGRAMS
        API --> WORKOUTS
        API --> RECORDS
        API --> HISTORY
    end

    subgraph DATABASE["BAZA DE DATE – PostgreSQL"]
        DB[("Baza de date Pushr")]
    end

    USER --> UI
    UI <-->|"HTTP/HTTPS – JSON"| API
    AUTH -->|"Verificare token JWT"| USERS
    USERS --> DB
    EXERCISES --> DB
    PROGRAMS --> DB
    WORKOUTS --> DB
    RECORDS --> DB
    HISTORY --> DB
```

## Comunicarea dintre componente

1. Utilizatorul interacționează cu interfața aplicației prin intermediul browserului.
2. Frontend-ul React trimite cereri HTTP către REST API-ul din backend.
3. Backend-ul Express primește cererile și le direcționează către modulul corespunzător.
4. Modulul de autentificare verifică identitatea utilizatorului folosind JWT.
5. Modulele backend accesează baza de date PostgreSQL pentru citirea și salvarea informațiilor.
6. Backend-ul transmite răspunsurile către frontend în format JSON.
7. Frontend-ul actualizează interfața și afișează datele utilizatorului, inclusiv graficele de progres.
