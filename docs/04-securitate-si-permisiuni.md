# Securitate, permisiuni și secrete

## Principii obligatorii

- Acces minim necesar: fiecare modul și token primește doar drepturile de care are nevoie.
- Separarea citirii de scriere și de operațiunile administrative.
- Confirmare explicită înainte de trimiterea mesajelor, crearea evenimentelor sau modificarea infrastructurii, cu excepțiile definite în politici.
- Jurnal de audit pentru solicitare, utilizator, acțiune, rezultat și moment. Jurnalul nu conține parole, tokenuri sau conținut sensibil inutil.
- Revocare rapidă pentru dispozitive, sesiuni și integrări compromise.

## Tokenuri, parole și chei API

Tokenul botului Telegram, parolele, cheile API și certificatele **nu se introduc în cod, documentație, capturi de ecran sau Git**. Acestea se păstrează local într-un manager de secrete sau în variabile de mediu gestionate de platforma de deployment.

În repository există doar un fișier exemplu, de forma `.env.example`, cu numele variabilelor și fără valori:

```dotenv
TELEGRAM_BOT_TOKEN=
TELEGRAM_ALLOWED_CHAT_IDS=
CALENDAR_CLIENT_ID=
CALENDAR_CLIENT_SECRET=
```

Fișierul local `.env` trebuie ignorat de Git. Dacă un token a fost distribuit accidental, el se revocă/regenerază imediat înainte de utilizarea botului.

## Politica Telegram

- Acceptăm comenzi doar din `chat_id`-uri autorizate.
- Asociem fiecare chat cu un utilizator și un nivel de permisiune.
- Nu interpretăm mesajele Telegram ca fiind de încredere doar pentru că au ajuns la bot.
- Cerem aprobare suplimentară pentru acțiuni externe sau sensibile.
- Limităm comenzile repetate pentru a preveni abuzul.

## Politica pentru servere

Monitorizarea pornește cu acces read-only. Comenzile de schimbare (restart, deploy, ștergere, modificare configurare) rămân dezactivate până când sunt documentate separat, au o politică de autorizare și o confirmare potrivită riscului.

## Politica browserului

Căutarea și citirea paginilor sunt tratate ca acțiuni de citire. Autentificarea, trimiterea formularelor, descărcările, modificarea conținutului online și actualizarea browserului sunt acțiuni cu efect extern și cer confirmare. Datele de autentificare nu sunt citite, afișate sau trimise de Djarvis.

## Politica agenților de programare

O sarcină poate avea mod de **analiză**, **plan**, **modificare locală** sau **publicare**. Djarvis trebuie să arate agentul selectat, proiectul țintă și modul înainte de pornire. Commit-urile, push-urile, publicarea, instalările și orice acces la servicii externe cer confirmarea prevăzută în politică. Nu se transmite automat cod privat, secrete sau fișiere din afara proiectului selectat.

## Date și confidențialitate

Înainte de activarea oricărei funcții se stabilește ce date sunt colectate, motivul colectării, unde sunt păstrate, cât timp și cine le poate accesa. Pentru IA externă, transmiterea datelor trebuie să fie vizibilă, configurabilă și dezactivată implicit până la aprobarea explicită.
