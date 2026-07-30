# Plan de dezvoltare

## Regula de livrare

Fiecare funcționalitate este realizată ca un pachet complet: cerință, comandă, validare, permisiuni, execuție, rezultat, jurnal, teste și documentație. Nu considerăm o funcție finalizată doar pentru că „pare să meargă”.

## Faza 0 — Fundament

- Alegerea tehnologiei și a modului de distribuție pentru desktop, mobil și VPS.
- Crearea structurii de directoare, convențiilor de cod și mediilor de configurare.
- Model minim pentru utilizatori, dispozitive, comenzi, sarcini, rezultate și audit.
- Politică inițială de permisiuni și fișiere `.env.example` fără valori reale.
- Teste automate de bază și verificări la integrarea codului.

**Criteriu de finalizare:** o comandă text simulată parcurge în siguranță fluxul interpretare → autorizare → rezultat → audit.

## Faza 1 — Nucleu local fără IA

- Comenzi text explicite.
- Deschiderea aplicațiilor permise pe macOS; proiectarea adaptorului pentru Windows.
- Căutare și navigare controlată în Chrome și Brave; actualizarea browserului doar după confirmare.
- Listă configurabilă de aplicații și aliasuri.
- Confirmare pentru comenzi care nu sunt numai de citire.
- Jurnal local și răspuns lizibil pentru utilizator.

**Criteriu de finalizare:** „deschide [aplicație]” funcționează numai pentru aplicații autorizate și este testat.

## Faza 2 — Voce

- Buton de activare vocală, apoi wake word opțional.
- STT și TTS selectate separat de nucleu; răspunsul TTS este obligatoriu pentru fiecare rezultat vocal.
- Limba română ca prioritate; engleza poate fi adăugată ulterior.
- Confirmări vocale și text pentru a evita interpretările greșite.

**Criteriu de finalizare:** o comandă vocală de deschidere aplicație are același control de permisiuni ca versiunea text, iar Djarvis confirmă și raportează rezultatul prin voce.

## Faza 3 — Calendar, mesaje și Telegram

- Conectare autorizată la calendar și poștă.
- Creare de întâlniri cu verificarea conflictelor și confirmare înainte de salvare.
- Șabloane de mesaje versionate, cu câmpuri obligatorii.
- Bot Telegram limitat la utilizatori/chat-uri autorizate; răspunsuri și aprobări.

**Criteriu de finalizare:** Djarvis poate pregăti și, după confirmare, trimite un mesaj sau crea o întâlnire, cu audit complet.

## Faza 4 — VPS și monitorizare

- API securizat între clienți și VPS.
- Sondaje de sănătate pentru servere și servicii autorizate.
- Metrici de bază: disponibilitate, CPU, memorie, disc, erori, backup.
- Rapoarte la cerere și alerte Telegram configurabile.

**Criteriu de finalizare:** comanda „statistică server” afișează date corecte, cu sursa și momentul colectării.

## Faza 5 — Analiză și IA opțională

- Analiză bazată pe reguli și rapoarte, apoi adaptor de IA.
- IA propune intenția sau un plan; nucleul validează și aplică permisiunile.
- Evaluare cu scenarii reale, inclusiv comenzi ambigue și acțiuni refuzate.
- Setări explicite pentru datele care pot ajunge la un model extern sau local.

**Criteriu de finalizare:** IA nu poate executa acțiuni în afara politicilor și toate deciziile importante pot fi urmărite.

## Faza 5.1 — Agenți de programare controlați vocal

- Selectarea și conectarea agenților Codex, Claude Code și/sau VS Code, în funcție de integrarea disponibilă.
- Transformarea unei cereri vocale într-o sarcină structurată pentru proiectul corect.
- Vizualizarea planului, progresului, fișierelor afectate și rezultatului.
- Politici explicite pentru modificări locale, comenzi de terminal, instalări, commit și push.

**Criteriu de finalizare:** Djarvis poate trimite o sarcină de dezvoltare unui agent, poate raporta rezultatul și nu permite publicarea sau modificările cu risc fără aprobarea configurată.

## Faza 6 — Extindere multi-platformă

- Client Windows.
- Aplicații iOS/iPadOS și Android pentru comenzi, notificări și aprobări.
- Funcționare în fundal: tray/menubar pe desktop; notificări push, sincronizare și acțiuni permise de sistem pe mobil.
- Sincronizare sigură a dispozitivelor și revocarea unui dispozitiv pierdut.
- Testare pe dispozitive reale și documentarea limitărilor de fundal.

## Pentru fiecare funcționalitate nouă

1. Definim ce problemă rezolvă și exemple de comenzi.
2. Stabilim datele folosite, permisiunile și confirmările.
3. Implementăm modulul și contractele sale.
4. Adăugăm teste pentru cazuri normale, erori și acces neautorizat.
5. Actualizăm documentația și lista de capabilități.
