# Dashboard, istoric și progres

## E-09 — Dashboard (`/dashboard`)

Sub salut și titlu apar, în ordine: sesiuni în curs cu „Continuă”, accesul „Vezi programele”, indicatorii pentru ultimele 30 de zile calendaristice (azi și precedentele 29 de zile), lista ultimelor sesiuni finalizate și recordurile curente. Indicatorii sunt sesiuni finalizate, volum total și greutate maximă pentru exercițiul selectat; fără exercițiu ales, greutatea maximă nu se agregă între exerciții.

Pe desktop indicatorii sunt alăturați; pe mobil sunt verticali. Un cont nou vede „Creează primul program” și explicații scurte, fără performanțe fictive. Selectarea unei sesiuni deschide istoricul detaliat; „Vezi progresul” deschide E-12.

## E-10 — Istoric (`/history`)

Titlul este urmat de filtrele Dată început și Dată sfârșit și butonul „Aplică”. Lista descrescătoare arată data, numele sesiunii, numărul de exerciții și volumul total. Fiecare element are „Vezi detalii”. Paginarea se află sub listă. Pe mobil, rândurile devin carduri cu aceleași informații.

## E-11 — Detaliu sesiune (`/history/:id`)

Sus apar „Înapoi la istoric”, numele, data, ora și starea „Finalizat”. Rezumatul include volumul și PR-urile obținute la finalizare. Fiecare exercițiu afișează seriile cu greutatea și repetările efective și legătura „Vezi progresul acestui exercițiu”. În MVP pagina este doar pentru consultare. După finalizarea din logger, această pagină include mesajul „Antrenament salvat”.

## E-12 — Progres (`/progress`)

Ordinea conținutului este: titlu, selector Exercițiu, Dată început, Dată sfârșit și „Aplică”. Urmează indicatorii Greutate maximă, Volum total și Sesiuni finalizate pentru selecție; apoi zonele de vizualizare a greutății maxime pe zi, volumului pe zi și frecvenței pe săptămână, însoțite de un tabel cu valorile.

PR-ul curent pentru exercițiu are o etichetă distinctă, „Record personal, toate sesiunile”, pentru a nu fi confundat cu maximul perioadei. Intervalul gol afișează explicația și opțiunea de a modifica filtrele. O cerere eșuată afișează eroare și „Reîncearcă”; nu este prezentată drept interval fără date.

## Legătura cu acceptanța

E-09–12 acoperă UC-03 și UC-04. Verificarea folosește valorile numerice din scenarii, atât în indicatori, cât și în tabelul asociat vizualizărilor. Filtrele și indicatorii trebuie să rămână lizibili la 360 px și operabili cu tastatura.
