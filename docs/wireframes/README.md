# Ecrane principale și navigare

Acest director conține specificații textuale de wireframe, fără diagrame sau imagini. Ordinea elementelor descrie ierarhia vizuală și amplasarea propusă; documentele sunt schițe preliminare, nu interfețe implementate.

| Document | Ecrane |
| --- | --- |
| [Cont și navigare](account.md) | Autentificare, înregistrare, profil și structura comună. |
| [Planificare și sesiune](workouts.md) | Bibliotecă, programe, editor și înregistrarea antrenamentului. |
| [Istoric și progres](progress.md) | Dashboard, istoric, detalii și indicatori. |

## Fluxuri de navigare

1. Vizitatorul intră în autentificare, poate trece la înregistrare, apoi revine la autentificare; după login ajunge la dashboard.
2. Din dashboard deschide „Programe”, alege „Program nou”, configurează zilele și deschide selectorul bibliotecii. Confirmarea selecției îl întoarce în editor; salvarea deschide detaliul programului.
3. Din detaliul programului alege o zi și „Începe antrenamentul”. Salvează seriile în ecranul sesiunii, apoi finalizează și ajunge la rezumatul din istoric.
4. Din „Istoric” deschide o sesiune; din detaliu poate deschide „Progres” pentru un exercițiu. Revenirea la istoric păstrează perioada și pagina selectate.
5. Din navigarea principală deschide „Profil”, salvează modificările și revine la pagina anterioară. „Deconectare” închide sesiunea și deschide autentificarea.

Accesarea directă a unei pagini private fără autentificare deschide login-ul. După autentificare se poate reveni la destinația internă cerută, dacă utilizatorul are acces. Un ID inexistent sau inaccesibil afișează „Resursa nu a fost găsită”.

## Comportament comun

- Desktop: antet cu numele Pushr și contul; navigare laterală; titlu și acțiunea principală în partea de sus a conținutului.
- Mobil: antet compact și meniu accesibil pentru Dashboard, Programe, Exerciții, Istoric, Progres și Profil; conținut pe o coloană.
- Formularele au etichete persistente, erori lângă câmp și o confirmare după salvare. Stările de încărcare și eroare sunt distincte de listele goale.
- Butoanele de trimitere sunt dezactivate cât timp cererea este în curs. Datele introduse nu sunt șterse la eroare.
- Părăsirea unui formular cu modificări nesalvate solicită confirmare. Seriile deja confirmate ca salvate rămân pe server.
- Graficele aplicației vor avea și un rezumat numeric/tabel accesibil; documentația nu conține grafice sau diagrame.
