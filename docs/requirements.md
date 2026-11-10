# Cerințe

## Prioritizare și scop

Se folosește MoSCoW: **Must** este obligatoriu pentru MVP, **Should** este o extensie recomandată după MVP, **Could** este opțional dacă există timp, iar **Won't** este exclus din această etapă. Ordinea implementării nu elimină din MVP cerințele Must.

## Cerințe funcționale

| ID | Prioritate | Cerință | Criteriu de acceptanță |
| --- | --- | --- | --- |
| CF-01 | Must | Înregistrare cu nume, email și parolă. | Un email valid, unic și o parolă de minimum 12 caractere creează un cont; emailul duplicat este respins fără cont suplimentar. |
| CF-02 | Must | Autentificare și deconectare. | Datele corecte permit accesul; datele greșite produc un mesaj generic; după deconectare, sesiunea nu mai permite cereri protejate. |
| CF-03 | Must | Vizualizarea și actualizarea profilului. | Utilizatorul schimbă numele afișat și fusul orar, iar valorile persistă după reautentificare. |
| CF-04 | Must | Bibliotecă de exerciții. | Lista poate fi filtrată cumulativ după grupa musculară și tip; lipsa rezultatelor are mesaj dedicat. |
| CF-05 | Must | Crearea și modificarea programelor. | Un program salvat cu nume și zile de antrenament poate fi redeschis și modificat de proprietar. |
| CF-06 | Must | Configurarea exercițiilor planificate. | Pentru fiecare zi se pot selecta și ordona exerciții, cu număr întreg pozitiv de serii și repetări. |
| CF-07 | Must | Pornirea unei sesiuni dintr-o zi de program. | Sesiunea primește o copie a configurației zilei; modificarea ulterioară a programului nu schimbă sesiunea. |
| CF-08 | Must | Înregistrarea și salvarea seriilor efectuate. | Greutatea și repetările valide persistă; valorile invalide sunt respinse atât în interfață, cât și în API. |
| CF-09 | Must | Finalizarea unei sesiuni. | O sesiune cu cel puțin o serie validă apare o singură dată în istoric și contribuie la statistici. |
| CF-10 | Must | Consultarea istoricului. | Lista afișează sesiunile finalizate descrescător după dată; detaliile includ exercițiile și toate seriile efectuate. |
| CF-11 | Must | Calcularea și evidențierea PR. | O greutate strict mai mare decât maximul anterior actualizează PR; egalitatea nu generează un record nou. |
| CF-12 | Must | Dashboard de progres. | Exercițiul și perioada selectate determină greutatea maximă, volumul și frecvența; lipsa datelor este afișată explicit. |
| CF-13 | Should | Șabloane distincte de antrenament. | Aplicarea unui șablon produce o copie editabilă fără modificarea originalului. |
| CF-14 | Should | Compararea a două perioade. | Aceiași indicatori sunt calculați pentru intervalele selectate, cu tratarea perioadei fără date. |
| CF-15 | Could | Exerciții personalizate. | Un exercițiu creat este accesibil doar proprietarului și poate fi selectat în programele sale. |
| CF-16 | Could | Realizări pentru consecvență. | Un prag documentat de sesiuni finalizate acordă o realizare o singură dată. |

## Reguli de business pentru MVP

- RB-01: fiecare utilizator are acces exclusiv la propriile programe, sesiuni, statistici și PR-uri. Biblioteca standard este comună și se consultă fără a fi modificată de utilizatori.
- RB-02: greutățile se exprimă în kg și se salvează ca valori zecimale nenegative; repetările și numărul planificat de serii sunt întregi strict pozitivi. Înregistrarea a 0 kg este permisă pentru exerciții fără încărcare externă.
- RB-03: PR în MVP înseamnă cea mai mare greutate externă efectuată într-o serie cu minimum o repetare, pentru același utilizator și exercițiu. Nu se estimează 1RM și nu se compară exerciții diferite. Prima performanță constituie recordul inițial.
- RB-04: volumul este suma `greutate_kg × repetări` pentru seriile salvate ale sesiunilor finalizate din selecție. Pentru exercițiile fără încărcare externă, această măsură are valoarea zero și nu exprimă efortul corporal.
- RB-05: frecvența este numărul de sesiuni finalizate pe zi/săptămână în interval; cu filtru de exercițiu se numără doar sesiunile care îl conțin.
- RB-06: sesiunile au starea `in_progress` sau `completed`. Seriile pot fi schimbate doar în sesiuni în curs. Finalizarea este atomică și repetarea ei nu dublează sesiunea sau PR-ul. Editarea/ștergerea sesiunilor finalizate este amânată după MVP.
- RB-07: datele sunt stocate în UTC. Intervalele calendaristice se interpretează în fusul orar al profilului, cu început inclusiv și sfârșit exclusiv. Pentru data finală selectată în UI se folosește începutul zilei următoare ca limită exclusivă.

## Cerințe nefuncționale

Pragurile de performanță sunt ținte de acceptanță propuse, care vor fi măsurate după implementare.

| ID | Prioritate | Cerință | Verificare propusă |
| --- | --- | --- | --- |
| CNF-01 | Must | Izolarea datelor și autorizare pe server. | Cererile utilizatorului B pentru ID-urile private ale lui A returnează 404 și nu modifică date. |
| CNF-02 | Must | Protejarea autentificării. | Parole stocate doar ca hash adaptiv cu salt; JWT cu expirare de 60 minute, verificarea semnăturii și revocare la logout; limitare a încercărilor de login. |
| CNF-03 | Must | Transport și validare sigure. | În mediul public se folosește HTTPS; cookie-ul de sesiune este HttpOnly, Secure și SameSite; cererile de scriere verifică originea și un token CSRF; interogările SQL sunt parametrizate. |
| CNF-04 | Must | Integritatea datelor. | Cheile externe și constrângerile resping date invalide; un eșec în finalizare nu lasă sesiune finalizată fără actualizarea PR-ului. |
| CNF-05 | Must | Interfață responsive și accesibilă. | Fluxurile principale funcționează la lățimi de 360, 768 și 1440 px, fără scroll orizontal; formularele au etichete, focus vizibil și sunt operabile cu tastatura. |
| CNF-06 | Must | Erori clare și protejarea informațiilor. | Formularele păstrează datele introduse după erori; API-ul nu expune parole, tokenuri sau stack trace-uri în răspunsuri/loguri. |
| CNF-07 | Must | Mentenabilitate și verificare. | Module separate pe domenii, migrații versionate și verificări de tipuri, build și teste critice înaintea integrării. |
| CNF-08 | Should | Timp de răspuns predictibil. | p95 sub 500 ms pentru citiri și sub 1 s pentru finalizare, la 20 utilizatori concurenți și 10.000 de sesiuni în baza de test, fără latența rețelei client. |
| CNF-09 | Should | Recuperarea datelor. | Backup zilnic în mediul găzduit și probă de restaurare într-o bază separată înainte de livrare. |
| CNF-10 | Should | Compatibilitate. | Scenariile de acceptanță trec pe versiunile stabile Chrome, Firefox și Edge disponibile la momentul livrării. |

## Excluderi (Won't)

Diagnostic medical, recomandări clinice automate, generare de programe pe baza evaluărilor medicale, integrare cu dispozitive fitness, rețea socială, plăți și funcționare offline completă. Extensiile necesită reprioritizare explicită.
