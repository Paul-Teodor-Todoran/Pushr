# Cont și structură comună

## E-01 — Autentificare (`/login`)

În centrul paginii apare un formular compact. De sus în jos: numele Pushr, titlul „Autentificare”, câmpul Email, câmpul Parolă cu control de afișare, butonul „Intră în cont” și legătura „Nu ai cont? Creează cont”. Pe mobil formularul folosește lățimea disponibilă cu margini laterale.

Login-ul reușit deschide `/dashboard`. Datele greșite afișează un mesaj generic deasupra butonului. Câmpurile obligatorii au erori individuale; emailul se păstrează după o eroare. Parola nu se include în URL sau mesaje.

## E-02 — Înregistrare (`/register`)

Aceeași structură ca E-01. Conținutul este: „Creează cont”, Nume afișat, Email, Parolă, Confirmă parola, indicația privind minimum 12 caractere, butonul „Creează cont” și legătura „Ai deja cont? Autentificare”. Confirmarea parolei este validată în interfață; API-ul primește o singură parolă.

După succes, utilizatorul vede confirmarea și revine la autentificare cu emailul precompletat. Emailul duplicat și parola neconformă au mesaje explicite. Înregistrarea nu afișează dashboard-ul înainte de autentificare.

## E-03 — Profil (`/profile`)

Sub navigarea comună apar titlul „Profilul meu”, Email (doar citire în MVP), Nume afișat, selector Fus orar și unitatea „kg” (fixă în MVP). La final: „Salvează modificările” și „Deconectare”. Salvarea păstrează pagina și afișează confirmarea; deconectarea revocă sesiunea și deschide `/login`.

## Verificare vizuală propusă

La 360 px câmpurile și butoanele rămân vizibile fără scroll orizontal. Ordinea de tabulare urmează ordinea vizuală; fiecare câmp are etichetă, iar erorile pot fi identificate fără a depinde doar de culoare.
