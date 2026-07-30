# Specificație tehnică

Acest document stabilește proiectarea tehnică de referință. Tehnologiile marcate **recomandat** sunt propuneri; nu devin decizii definitive până când nu sunt validate într-un prototip.

## 1. Model de funcționare

```text
Microfon / Telegram / aplicație mobilă
                 │
                 ▼
      Client Djarvis pe dispozitiv
      wake word → STT → comandă
                 │ HTTPS / WebSocket securizat
                 ▼
             VPS Djarvis
 autentificare → politică → orchestrator → coadă de sarcini
             │          │               │
             ▼          ▼               ▼
       calendar/e-mail  monitorizare   agent de programare
             │
             ▼
      rezultat structurat + audit
                 │
                 ▼
         TTS / notificare / Telegram
```

O acțiune locală simplă, precum deschiderea Chrome pe Mac, se execută direct pe client pentru latență mică. O acțiune care folosește servicii externe, trebuie programată sau verifică servere este gestionată prin VPS. Dacă VPS-ul nu este disponibil, comenzile locale permise trebuie să poată funcționa în continuare.

## 2. Componente

| Componentă | Execută | Responsabilități |
| --- | --- | --- |
| Client desktop | macOS/Windows | microfon, wake word, STT/TTS, tray/menubar, acțiuni locale, conexiune securizată la VPS |
| Client mobil | iOS/iPadOS/Android | comenzi, notificări, aprobări, sincronizare de stare; execuție locală doar unde platforma permite |
| API VPS | VPS personal | autentificare, autorizare, orchestrare, API pentru clienți |
| Worker-e VPS | VPS personal | sarcini programate, monitorizare, integrări calendar/e-mail, alerte |
| Bază de date | VPS personal | utilizatori, dispozitive, sarcini, politici, audit și configurări ne-secrete |
| Broker/coadă | VPS personal | executare asincronă, retry controlat și izolare între sarcini |
| Integrare Telegram | VPS personal | recepție comenzi, aprobare și transmitere rezultate |

## 3. Flux vocal

1. Clientul detectează wake word-ul „Djarvis” sau primește activare prin buton.
2. Microfonul capturează comanda într-o fereastră scurtă și o trimite motorului STT.
3. STT produce textul, limba și un scor de încredere.
4. Parserul extrage intenția și parametrii; scorul mic sau datele lipsă activează o întrebare vocală.
5. Politica decide: permis, necesită confirmare, sau refuzat.
6. Orchestratorul rulează acțiunea local sau la distanță.
7. Rezultatul este convertit în răspuns scurt și natural, apoi redat de TTS.
8. Evenimentul este salvat în audit. Datele audio brute nu se păstrează implicit.

Ținte inițiale de experiență (măsurate pe dispozitiv): detectarea locală a wake word-ului sub 500 ms, răspuns de confirmare sub 2 secunde pentru o comandă locală și răspuns de status sub 5 secunde când trebuie interogat VPS-ul. Valorile finale se stabilesc după testarea modelelor STT/TTS alese.

## 4. Tehnologii: propunere de evaluat

| Zonă | Variantă recomandată | Alternative / observații |
| --- | --- | --- |
| Nucleu/VPS | TypeScript + Node.js sau Rust | Rust este potrivit pentru performanță și client local; TypeScript reduce timpul de dezvoltare pentru integrări. Decizia se ia după prototip. |
| Desktop | Tauri + UI web | permite clienți macOS/Windows și o integrare bună cu Rust; alternativ Electron dacă ecosistemul cerut o justifică. |
| Mobil | React Native sau Flutter | se alege după testarea push, fundal, STT și integrare nativă iOS/Android. |
| API | REST pentru cereri; WebSocket pentru progres live | toate versiunile API sunt prefixate, de exemplu `/v1`. |
| Bază de date | PostgreSQL | bună pentru audit, tranzacții și politici structurate. |
| Cache/coadă | Redis + worker | opțional în MVP; introdus înainte de joburi asincrone și monitorizare extinsă. |
| Observabilitate | OpenTelemetry + metrici Prometheus | jurnale structurate și alerte fără secrete. |
| Configurare secrete | manager de secrete sau secret store al platformei | niciodată în repository. |

Nu implementăm toate aceste servicii în prima versiune. MVP-ul poate porni cu client desktop, API minim și PostgreSQL; coada, mobilul și integrarea IA se adaugă pe etape.

## 5. Identitate, dispozitive și sesiuni

- Fiecare utilizator are un identificator intern (`user_id`).
- Fiecare dispozitiv înrolat are `device_id`, nume, platformă, versiune client, cheie publică/credential și stare: activ, revocat sau expirat.
- Clientul primește un token de acces cu durată scurtă și îl reînnoiește cu un mecanism securizat.
- VPS-ul validează utilizatorul, dispozitivul și permisiunea pentru fiecare comandă.
- Un dispozitiv poate fi revocat imediat din aplicație sau Telegram; tokenurile lui nu mai sunt acceptate.

## 6. Datele principale

| Entitate | Câmpuri esențiale | Scop |
| --- | --- | --- |
| `User` | id, nume, rol, stare | control acces |
| `Device` | id, user_id, platformă, nume, stare, last_seen_at | gestionare dispozitive |
| `Command` | id, sursă, text, intenție, parametri, status | urmărește cererea |
| `Action` | id, command_id, tip, risc, executor, status | urmărește execuția |
| `Approval` | id, action_id, solicitant, aprobat_de, status, expirare | confirmări |
| `Integration` | id, tip, user_id, stare, scope-uri | calendar, e-mail, Telegram etc. fără secrete în clar |
| `AuditEvent` | id, actor, eveniment, țintă, rezultat, moment | trasabilitate |
| `Alert` | id, sursă, severitate, mesaj, stare | alerte infrastructură |

## 7. Rețea și API

- Toate comunicațiile client–VPS folosesc HTTPS/TLS.
- API-ul refuză trafic neautentificat; rate limiting se aplică pentru autentificare, Telegram și comenzi costisitoare.
- WebSocket-ul este folosit doar pentru progres, rezultate live și prezența dispozitivelor; se revalidează sesiunea la conectare.
- Operațiunile care pot fi repetate primesc un `idempotency_key`, astfel încât o reîncercare să nu trimită de două ori același e-mail sau să nu creeze două întâlniri.
- Fiecare cerere are un `correlation_id`, prezent în jurnale, audit și răspuns pentru depanare.

## 8. Execuția agentului de programare

Modulul `developer-tools` nu oferă acces liber la computer. El lucrează cu un proiect selectat și o listă de directoare autorizate. O sarcină transmite agentului numai contextul necesar și exclude `.env`, chei, tokenuri și alte secrete. Rezultatul include planul, fișierele afectate, comenzile rulate și testele executate.

Pentru fiecare sarcină se setează unul dintre moduri:

- `analysis` — doar citește și raportează;
- `plan` — propune pași, fără a modifica;
- `local_write` — modifică numai în proiectul aprobat;
- `publish` — poate face commit/push/deploy numai după aprobarea prevăzută.

## 9. Disponibilitate și recuperare

- Clientul persistă local comenzile care pot fi reluate și le marchează cu idempotency key.
- Worker-ele au retry cu limită și backoff; după limita stabilită, sarcina devine `failed` și produce alertă.
- Backup-ul bazei de date este programat și verificat printr-un test de restaurare periodic.
- Acțiunile nesigure la repetare (trimitere e-mail, modificări sistem) nu sunt reexecutate automat fără o regulă explicită.

## 10. Observabilitate și audit

Se colectează metrici: disponibilitatea API, latența comenzilor, rata erorilor STT/TTS, durata acțiunilor, joburi eșuate și starea integrărilor. Jurnalele sunt structurate, cu `correlation_id`, dar fără parole, tokenuri, audio brut sau conținut de e-mail nejustificat.

Pentru fiecare execuție trebuie să putem răspunde: cine a cerut-o, de pe ce dispozitiv, ce a înțeles Djarvis, ce politică a permis-o, cine a aprobat-o, ce s-a executat și care a fost rezultatul.
