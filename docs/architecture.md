# Arhitectura aplicației

## Abordare

Se propune o aplicație cu frontend React, API REST Express și bază de date PostgreSQL. Backend-ul este un monolit modular: domeniile sunt separate în cod, dar se livrează ca un singur serviciu. Dimensiunea echipei și termenul limitat favorizează o integrare simplă și tranzacții locale. Arhitectura este descrisă textual, fără diagramă, conform cerinței proiectului pentru această documentație.

## Componente și responsabilități

| Componentă | Responsabilitate | Comunicare |
| --- | --- | --- |
| Frontend React/TypeScript | Pagini, navigare, formulare, stări de încărcare și erori; Recharts pentru progres. | Cereri HTTPS/JSON la `/api/v1`; nu accesează direct baza de date. |
| Strat HTTP Express | Rutare, parsarea cererilor, validare, autentificare, autorizare și tratarea erorilor. | Apelează serviciile interne și întoarce coduri HTTP și JSON. |
| Modul conturi | Înregistrare, login/logout, sesiuni și profil. | Citește/scrie utilizatori și sesiuni de autentificare. |
| Modul catalog | Biblioteca standard și filtrele. | Citește exerciții și grupe musculare. |
| Modul planificare | Programe, zile și ținte de antrenament. | Verifică proprietarul și persistă configurația programului. |
| Modul sesiuni | Copierea configurației, serii efective și finalizare. | Apelează calculul PR și persistă totul tranzacțional la finalizare. |
| Modul progres | PR, maxime, volum și frecvență. | Agregă seriile sesiunilor finalizate, limitate la utilizator și interval. |
| Strat de acces la date | Interogări parametrizate și tranzacții. | Conexiuni PostgreSQL printr-un pool; nu primește cereri direct din browser. |
| PostgreSQL | Persistență, chei externe, unicitate și constrângeri. | Accesibil exclusiv backend-ului în mediul găzduit. |

## Comunicarea într-un flux complet

1. Browserul cere pornirea unei zile de program prin API. Middleware-ul verifică JWT-ul și sesiunea nerevocată; serviciul verifică proprietarul programului.
2. Backend-ul copiază exercițiile și țintele într-o sesiune nouă și returnează ID-ul ei. Frontend-ul deschide logger-ul.
3. Fiecare salvare de serie trimite greutate și repetări. Validarea serverului și constrângerile bazei împiedică valorile invalide.
4. La finalizare, serviciul blochează sesiunea în tranzacție, verifică existența seriilor și actualizează PR-urile. Accesul concurent la PR este serializat pe perechea utilizator–exercițiu; prima creare folosește unicitate și upsert pentru a preveni duplicatele.
5. Tranzacția salvează și evenimentele de record și marchează sesiunea `completed`. Un eșec anulează toate aceste modificări. Repetarea cererii pentru o sesiune finalizată returnează rezumatul existent.
6. Dashboard-ul și istoricul cer datele actualizate. Calculele authoritative rămân în backend; frontend-ul doar formatează valorile și vizualizările.

## Autentificare și securitate propusă

JWT-ul este transmis prin cookie HttpOnly, Secure în mediul public și SameSite=Lax. Payload-ul conține identificatorul utilizatorului, identificatorul sesiunii și expirarea de 60 minute. API-ul verifică semnătura, expirarea și existența unei sesiuni nerevocate. Logout-ul revocă sesiunea în baza de date și șterge cookie-ul. După expirare se cere reautentificarea; refresh token-urile sunt amânate.

Frontend-ul și API-ul sunt propuse sub aceeași origine, cu API-ul expus la `/api/v1`. Cererile de scriere necesită token CSRF asociat sesiunii și verificarea antetului Origin. Parolele se păstrează ca hash adaptiv cu salt, folosind o bibliotecă dedicată. Parametrii concreți de hashing și pragurile de limitare login vor fi fixați și testați înaintea livrării. Autorizația se aplică pe server fiecărei resurse private, indiferent de ce afișează UI-ul.

## Livrare și configurare

În dezvoltare, frontend-ul, API-ul și PostgreSQL pot rula separat; proxy-ul frontend redirecționează `/api` spre backend. În mediul public, un server web/reverse proxy servește frontend-ul și redirecționează API-ul sub aceeași origine. Furnizorul de găzduire nu este stabilit încă.

Se propun variabile de mediu pentru conexiunea la bază, cheia JWT, originea aplicației și portul API. Secretele nu intră în Git; viitorul `.env.example` va conține doar nume și valori fictive. Migrațiile și datele inițiale ale bibliotecii se păstrează în repository. Logurile vor include ID de cerere, rută, cod și durată, fără parole sau tokenuri.

## Limite și compromisuri

Statisticile sunt calculate la cerere în MVP, cu indexuri pe utilizator, dată și exercițiu. Nu sunt necesare inițial microservicii, cozi, cache distribuit sau WebSocket. Seriile confirmate se păstrează pe server, dar funcționarea offline nu este inclusă. Extinderea către editarea sesiunilor finalizate va necesita recalcularea PR-urilor și a evenimentelor istorice.
