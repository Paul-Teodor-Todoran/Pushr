# Planificare și antrenament

## E-04 — Biblioteca (`/exercises`)

Sub titlul „Exerciții” apar căutarea după nume și selectoarele Grupă musculară și Tip. Urmează lista cu numele, grupa principală și tipul fiecărui exercițiu. Pe desktop filtrele sunt pe același rând; pe mobil se așază vertical. Lista include paginare și stări pentru încărcare, eroare și lipsa rezultatelor.

Deschiderea din editor folosește un selector cu aceleași filtre și butoanele „Adaugă selecția” și „Anulează”. Confirmarea păstrează ziua în curs de editare. Biblioteca accesată din meniul principal este doar pentru consultare.

## E-05 — Programe (`/programs`)

Titlul și „Program nou” sunt în partea de sus. Urmează carduri cu numele programului, zilele configurate și „Deschide”. Starea goală explică faptul că primul program trebuie creat și oferă aceeași acțiune principală.

## E-06 — Editor program (`/programs/new`, `/programs/:id/edit`)

De sus în jos: legătura „Înapoi la programe”, titlul, Nume program și Descriere opțională. Urmează lista zilelor, fiecare cu nume, ziua săptămânii opțională, ordinea și acțiunea „Adaugă exerciții”. În fiecare zi, exercițiile au nume, Serii, Repetări, „Mută mai sus”, „Mută mai jos” și „Elimină”. Zilele se adaugă cu „Adaugă zi”.

La final apar „Salvează programul” și „Anulează”. Pe mobil câmpurile unui exercițiu se grupează vertical. Salvarea deschide detaliul; anularea revine la listă, cu confirmare dacă există modificări. Programul nu poate fi salvat fără nume, fără zile sau cu o zi fără exerciții.

## E-07 — Detaliu program (`/programs/:id`)

Antetul conține numele și „Editează”. Fiecare zi afișează exercițiile ordonate și țintele, de exemplu „Bench Press — 3 × 8”. Acțiunea „Începe antrenamentul” aparține unei zile, astfel încât sursa sesiunii să fie explicită. Dacă există deja o sesiune în curs pentru ziua respectivă, interfața oferă „Continuă sesiunea” și evită pornirea accidentală a alteia.

## E-08 — Sesiune activă (`/sessions/:id`)

Sus apar numele zilei, ora începerii și eticheta „În curs”. Fiecare exercițiu are ținta planificată și o listă de serii: număr, câmp Greutate (kg), câmp Repetări, „Salvează seria”, „Șterge” și starea „Salvat”/„Nesalvat”. „Adaugă serie” permite depășirea numărului planificat. Seriile planificate fără valori sunt doar câmpuri de introducere și nu sunt rezultate efectuate.

Acțiunea finală este „Finalizează antrenamentul”. Ea salvează mai întâi modificările valide și apoi cere finalizarea. Dacă o salvare eșuează, finalizarea nu continuă. Succesul deschide detaliul sesiunii cu rezumat și PR-uri noi. Părăsirea paginii nu finalizează sesiunea; utilizatorul o poate relua din dashboard.
