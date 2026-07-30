# Idei și capabilități viitoare

Aceste idei sunt propuneri pentru Djarvis, nu promisiuni deja implementate. O capabilitate intră în plan numai după ce stabilim valoarea ei, datele necesare, permisiunile, platformele și criteriile de testare.

## 1. Rezumat personal de dimineață și seară

Djarvis poate oferi vocal un briefing configurabil: întâlnirile următoare, sarcinile restante, vremea, alertele serverelor, e-mailurile importante și obiectivul zilei. Seara poate întreba ce a fost finalizat și poate pregăti ziua următoare.

Exemplu: „Djarvis, care este planul meu pentru azi?”

Date necesare: calendar, sarcini și surse activate explicit. Risc: mic; nu trimite mesaje și nu modifică date fără confirmare.

## 2. Mementouri și rutine inteligente

Pe lângă alarme simple, Djarvis poate crea rutine: „în fiecare luni la 9 verifică serverul”, „amintește-mi să răspund dacă nu primesc răspuns timp de două zile” sau „când ajung acasă, amintește-mi de document”.

Rutinele au întotdeauna: declanșator, acțiune, canal de notificare, interval de activitate și buton/comandă de oprire. Cele care trimit mesaje, modifică date sau controlează dispozitive cer aprobare la creare.

## 3. Memorie personală controlată

Djarvis poate reține preferințe aprobate, note și context util: numele proiectelor, șabloanele preferate, modul în care vrei să fii anunțat sau deciziile luate. Memoria este vizibilă și gestionabilă prin comenzi:

- „Djarvis, reține că proiectul Atlas folosește serverul VPS-2.”
- „Djarvis, ce știi despre proiectul Atlas?”
- „Djarvis, uită informația despre Atlas.”

Nu memorăm automat conversații complete, parole, tokenuri, date bancare sau date medicale. Fiecare element de memorie are sursă, dată, nivel de sensibilitate și opțiune de ștergere.

## 4. Note vocale transformate în sarcini

Utilizatorul poate dicta o idee, iar Djarvis produce o notă, o listă de sarcini sau un rezumat. Înainte de a crea elemente în servicii externe, confirmă unde va fi salvat.

Exemplu: „Djarvis, notează: trebuie să verific backup-ul vineri și să trimit raportul lui Ana.”

Rezultat: o notă plus două sarcini propuse, cu dată lipsă cerută vocal dacă este necesar.

## 5. Asistent pentru întâlniri

Cu acordul participanților și în limitele legale aplicabile, Djarvis poate pregăti agenda, afișa documentele relevante, lua note, extrage decizii și produce un rezumat. Trimiterea rezumatului către participanți se face numai după aprobare.

## 6. Documente și fișiere

Djarvis poate căuta fișiere autorizate, organiza documente, explica conținutul, genera un rezumat și pregăti documente din șabloane. Orice mutare, redenumire, ștergere sau încărcare externă este tratată ca acțiune cu efect și cere confirmare.

Exemplu: „Djarvis, găsește contractul cu clientul X și spune-mi data expirării.”

## 7. Supravegherea schimbărilor importante

Djarvis poate urmări, la cererea utilizatorului, schimbări într-o pagină, disponibilitatea unui serviciu, starea unui domeniu, un preț sau un document. La schimbare, oferă o alertă vocală sau Telegram cu sursa și momentul detectării.

Pentru surse financiare sau medicale, Djarvis oferă informație și surse; nu ia decizii financiare sau medicale în locul utilizatorului.

## 8. Sănătatea dispozitivelor personale

Pentru dispozitivele înrolate, Djarvis poate raporta bateria, spațiul de stocare, conexiunea, ultima sincronizare și actualizările disponibile. El poate recomanda o acțiune; instalarea actualizărilor sau ștergerea datelor necesită confirmare.

Exemplu: „Djarvis, cum este MacBook-ul meu?”

## 9. Controlul casei inteligente — opțional

Dacă există dispozitive compatibile și o integrare autorizată, Djarvis poate controla lumini, prize, termostat sau scene: „Djarvis, activează modul lucru.” Comenzile legate de acces fizic, camere, alarme sau încuietori au politici stricte și, unde este necesar, confirmare suplimentară.

Această capabilitate nu face parte din MVP.

## 10. Automatizări între aplicații

Djarvis poate construi fluxuri simple din module aprobate, de exemplu: „când primesc o alertă critică de la server, trimite-mi în Telegram și creează o sarcină de verificare”. Fiecare flux afișează clar declanșatorul, datele utilizate și acțiunile produse; utilizatorul îl poate opri, modifica sau șterge.

## 11. Comandă „explică” și „anulează”

Acestea sunt capabilități esențiale pentru încredere:

- „Djarvis, de ce ai cerut confirmarea?” — explică politica aplicată.
- „Djarvis, ce ai făcut ultima dată?” — citește rezumatul auditului recent.
- „Djarvis, anulează ultima acțiune.” — inversează doar acțiunile reversibile sau explică de ce nu poate fi inversată.

## 12. Prioritizare recomandată

| Prioritate | Capabilități | Motiv |
| --- | --- | --- |
| După MVP | briefing, mementouri, note vocale, „explică/anulează” | valoare mare, risc controlabil |
| După integrări | documente, întâlniri, sănătate dispozitive, monitorizare schimbări | depind de permisiuni și adaptoare |
| Ulterior | memorie personală, automatizări între aplicații, smart home | necesită politici mature și testare extinsă |

## Regula pentru orice idee nouă

O funcție nouă trebuie să specifice: comanda vocală, răspunsul vocal, datele accesate, platformele, nivelul de risc, când cere confirmare, cum se anulează și cum este testată. Abia după aceea primește un modul de implementare și intră în roadmap.
