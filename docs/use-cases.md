# Scenarii de utilizare

Scenariile descriu comportamentul propus și pot fi folosite ca teste de acceptanță după implementare. Actorul principal este utilizatorul; backend-ul validează și persistă datele. Identificatorii CF/CNF/RB trimit la [cerințe](requirements.md).

## UC-01 — Înregistrare, autentificare și profil

**Scop:** obținerea accesului la spațiul personal. **Cerințe:** CF-01–03, CNF-01–03. **Precondiții:** aplicația și API-ul sunt disponibile; emailul de test nu există. **Declanșator:** vizitatorul selectează „Creează cont”.

1. Utilizatorul introduce numele „Alex”, un email de test valid și o parolă de minimum 12 caractere, apoi trimite formularul.
2. Sistemul validează datele, creează contul și afișează confirmarea cu acces la autentificare.
3. Utilizatorul se autentifică folosind aceleași date.
4. Sistemul stabilește sesiunea și deschide dashboard-ul personal gol.
5. Utilizatorul deschide profilul, schimbă numele în „Alex Fitness” și fusul orar în `Europe/Bucharest`, apoi salvează.
6. Utilizatorul se deconectează și se autentifică din nou.

**Alternative și erori:** emailul deja folosit produce 409 și nu creează un al doilea cont; câmpurile invalide sunt marcate; parola greșită produce 401 cu mesaj generic; o eroare de rețea permite reîncercarea fără pierderea câmpurilor nesensibile.

**Postcondiții:** există un singur cont, cu profilul actualizat și parolă stocată ca hash.

**Verificare:** numele și fusul orar persistă după pasul 6; o cerere protejată făcută cu sesiunea revocată primește 401; nu sunt returnate hash-uri de parolă în răspunsurile API.

## UC-02 — Crearea și modificarea unui program

**Scop:** planificarea unei zile de antrenament reutilizabile. **Cerințe:** CF-04–06. **Precondiții:** utilizator autentificat; biblioteca conține Bench Press și Incline Dumbbell Press, asociate grupei „Piept”. **Declanșator:** selectarea „Program nou” din „Programe”.

1. Utilizatorul denumește programul „Push/Pull/Legs”.
2. Adaugă ziua „Push”, pentru luni.
3. Deschide selectorul de exerciții și filtrează după „Piept”.
4. Adaugă Bench Press cu 3 serii a câte 8 repetări și Incline Dumbbell Press cu 3 serii a câte 10 repetări.
5. Salvează programul și îl redeschide din listă.
6. Schimbă repetările planificate pentru Bench Press la 10 și salvează.

**Alternative și erori:** zero serii sau repetări este respins fără salvare parțială; o căutare fără rezultate afișează o stare goală; închiderea selectorului fără confirmare nu adaugă exerciții; la părăsirea formularului modificat se solicită confirmarea abandonării.

**Postcondiții:** programul aparține utilizatorului și conține ziua și exercițiile în ordinea aleasă.

**Verificare:** reîncărcarea paginii arată 3 × 10 pentru Bench Press și 3 × 10 pentru Incline Dumbbell Press; alt cont nu poate citi sau modifica programul, nici prin apel direct al API-ului.

## UC-03 — Înregistrarea unui antrenament și actualizarea PR

**Scop:** salvarea unei sesiuni reale și detectarea unui record. **Cerințe:** CF-07–11, RB-02–06. **Precondiții:** utilizator autentificat, program cu Bench Press; există o sesiune finalizată anterioară cu maximul de 60 kg pentru acest exercițiu. **Declanșator:** selectarea „Începe antrenamentul” pentru o zi a programului.

1. Sistemul creează o sesiune în curs, cu copia exercițiilor și țintelor planificate.
2. Utilizatorul introduce și salvează trei serii Bench Press: 60 kg × 8, 65 kg × 6 și 65 kg × 5.
3. Sistemul confirmă salvarea seriilor; acestea pot fi regăsite după reîncărcarea paginii.
4. Utilizatorul selectează „Finalizează antrenamentul”.
5. Sistemul finalizează sesiunea, calculează volumul și actualizează PR-ul într-o tranzacție.
6. Aplicația deschide rezumatul și evidențiază noul PR de 65 kg.

**Alternative și erori:** greutatea negativă sau repetările zero sunt respinse, iar sesiunea rămâne în curs; finalizarea fără serii este respinsă; la pierderea conexiunii, interfața marchează datele nesalvate și permite reîncercarea; repetarea finalizării returnează același rezumat. O serie ulterioară egală cu 65 kg nu creează un PR nou.

**Postcondiții:** există o singură sesiune finalizată; PR-ul este 65 kg și trimite la prima serie care a atins această valoare în sesiune.

**Verificare:** volumul Bench Press al sesiunii este `60 × 8 + 65 × 6 + 65 × 5 = 1.195 kg·repetări`; istoricul conține cele trei serii; două cereri de finalizare nu dublează datele. Schimbarea programului nu modifică rezultatele sesiunii.

## UC-04 — Consultarea istoricului și progresului

**Scop:** evaluarea performanței într-o perioadă. **Cerințe:** CF-10–12, RB-04–07. **Precondiții:** cont cu două sesiuni finalizate Bench Press, pe 2 și 5 noiembrie 2026, fiecare cu o singură serie: 60 kg × 8, respectiv 65 kg × 6; nicio altă sesiune în interval. **Declanșator:** deschiderea „Istoric”, apoi „Progres”.

1. Utilizatorul deschide istoricul și selectează sesiunea din 5 noiembrie.
2. Sistemul afișează Bench Press, 65 kg și 6 repetări, cu starea finalizată.
3. Utilizatorul deschide „Progres”, selectează Bench Press și perioada 1–7 noiembrie 2026 inclusiv, în fusul orar al profilului.
4. Sistemul afișează seria temporală și indicatorii calculați pentru selecție.
5. Utilizatorul alege un interval fără sesiuni.

**Alternative și erori:** intervalul cu început după sfârșit este respins; datele inaccesibile altui cont produc 404; eșecul API afișează „Reîncearcă”, fără a prezenta valori vechi drept rezultate curente.

**Postcondiții:** consultarea nu modifică datele salvate.

**Verificare:** pentru 1–7 noiembrie, maximul este 65 kg, volumul este `480 + 390 = 870 kg·repetări`, iar frecvența este 2 sesiuni în săptămâna selectată. Pentru intervalul gol apare „Nu există date pentru perioada selectată”, fără record inventat și fără grafic cu puncte fictive.
