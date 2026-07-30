# Viziune și funcționalități

## Ce este Djarvis

Djarvis este un asistent personal care rulează local pe dispozitiv și, pentru operațiuni la distanță, comunică securizat cu un VPS personal. Interfața principală este o conversație vocală completă: utilizatorul vorbește cu Djarvis, iar Djarvis răspunde cu voce. Textul este disponibil complementar pentru afișare, verificare, accesibilitate, control și audit.

Principiul de bază este: **comandă vocală înțeleasă → confirmare vocală când este necesar → acțiune executată → răspuns vocal, rezultat înregistrat și transmis**.

În prima etapă, Djarvis folosește comenzi și reguli definite clar, fără IA obligatorie. Ulterior, IA va deveni un modul opțional pentru limbaj natural, analiză și planificarea sarcinilor.

## Platforme și mod de utilizare

| Platformă | Rol inițial | Rol țintă |
| --- | --- | --- |
| macOS | aplicație principală, ascultare în fundal și executare locală | client complet cu comandă vocală și text |
| Windows | client local care poate rula în fundal, primi comenzi/notificări și executa acțiuni locale autorizate | paritate funcțională cu macOS |
| iPhone / iPad | client mobil care primește notificări, trimite comenzi și aprobă acțiuni în fundal în limitele iOS/iPadOS | aplicație mobilă cu funcții locale permise de sistem |
| Android | client mobil care rulează servicii autorizate în fundal, primește notificări și trimite comenzi | aplicație mobilă cu funcții locale permise de sistem |
| VPS personal | orchestrare, joburi programate și integrare securizată | centru privat de automatizare și observabilitate |
| Telegram | chat de control și raportare | canal de alerte, rezultate și aprobări |

## Funcționare în fundal

Djarvis este proiectat să fie disponibil în fundal pe **macOS, Windows, iPhone/iPad și Android**. Fiecare client menține conexiunea necesară cu VPS-ul, poate primi notificări, rezultate și cereri de aprobare și poate raporta starea sa. Pe desktop, Djarvis poate rula ca aplicație de meniu/tray și poate executa automatizări locale autorizate.

Pe mobil, sistemul de operare poate restricționa ascultarea permanentă, pornirea automată sau deschiderea arbitrară a aplicațiilor. În consecință, iPhone/iPad și Android vor folosi mecanismele oficiale disponibile: notificări push, acțiuni din notificare, activare manuală/vocală permisă de platformă și sincronizare în fundal. Proiectul nu va ocoli aceste restricții; când o comandă trebuie executată pe un telefon indisponibil, Djarvis va raporta clar acest lucru.

## Funcționalități de bază

### 1. Pornirea și deschiderea aplicațiilor

Exemple de comenzi:

- „Djarvis, pornește Safari.”
- „Djarvis, deschide Telegram.”
- „Djarvis, pornește aplicația de muzică.”

Modulul identifică dispozitivul activ și aplicația autorizată, apoi deschide aplicația local sau transmite solicitarea clientului de pe acel dispozitiv. Pentru acțiuni locale nu este necesar VPS-ul.

### 2. Control browser și căutare web

Djarvis poate utiliza browserele autorizate, inclusiv **Google Chrome** și **Brave**, pentru a deschide o pagină, a căuta informații și a prezenta un rezumat cu linkurile sursă. Browserul ales poate fi configurat separat pentru fiecare dispozitiv.

Exemple de comenzi:

- „Djarvis, caută în Chrome vremea pentru mâine în Chișinău.”
- „Djarvis, deschide Brave și caută documentația pentru X.”
- „Djarvis, actualizează Chrome.”

Acțiunile de citire, precum căutarea și deschiderea unei pagini, pot fi executate direct. Acțiunile cu efect extern — autentificare, completare/trimitere formulare, descărcări, modificări ale unei pagini sau actualizarea browserului — necesită confirmare înainte de executare. Djarvis raportează rezultatul și nu transmite parole sau date private în mod automat.

### 3. Întâlniri și calendar

Djarvis creează întâlniri pe baza unei comenzi vocale sau text: titlu, dată, oră, durată, participanți și eventual locație/link. Înainte de salvare, prezintă un rezumat pentru confirmare. Integrarea cu calendarul și poșta se realizează numai după autorizarea contului respectiv.

Exemplu: „Creează miercuri la 15:00 o întâlnire de 30 de minute cu Ana despre raport.”

### 4. Mesaje și e-mailuri din șabloane

Djarvis selectează un șablon aprobat, completează câmpurile necesare și prezintă textul înainte de expediere. Trimiterea automată se activează numai pentru destinatari, șabloane sau reguli aprobate explicit.

Exemplu: „Pregătește mesajul de confirmare pentru clientul X folosind șablonul ofertă.”

### 5. Analiză

Asistentul poate analiza surse autorizate: texte, fișiere, date operaționale, loguri sau rezultate ale unor verificări. Rezultatul trebuie să conțină concluzia, sursa/perioada analizată și elementele care necesită atenție. În absența IA, analiza începe cu reguli, filtre, rapoarte și praguri configurate.

### 6. Monitorizare servere și infrastructură

Prin comenzi precum „Djarvis, statistică server” sau „verifică serverele”, Djarvis poate colecta starea serviciilor autorizate: disponibilitate, resurse, spațiu pe disc, erori, backup-uri și alerte. El nu modifică infrastructura fără o acțiune explicită și o confirmare atunci când operațiunea este sensibilă.

Exemple de răspuns:

- „Serverul VPS este disponibil; CPU mediu 18%, spațiu liber 62%.”
- „Serviciul X nu răspunde de 5 minute; am creat o alertă în Telegram.”

### 7. Telegram

Telegram este un canal complementar, nu singura sursă a adevărului. Djarvis trimite rezultate, alerte și solicitări de aprobare într-un chat autorizat. Comenzile primite prin Telegram sunt validate identic cu cele vocale: utilizator permis, acțiune permisă și jurnal de audit.

### 8. Asistență pentru dezvoltare software

Djarvis poate deveni interfața vocală pentru agenți de programare, inclusiv **Codex**, **Claude Code** și agenții disponibili în **Visual Studio Code**. Utilizatorul descrie vocal sarcina, iar Djarvis o transformă într-o cerere structurată: proiectul vizat, obiectivul, constrângerile, criteriile de acceptare și agentul ales.

Exemple de comenzi:

- „Djarvis, deschide proiectul X în VS Code și cere agentului să repare eroarea de autentificare.”
- „Djarvis, spune-i lui Codex să adauge teste pentru modulul calendar și arată-mi planul înainte să modifice fișiere.”
- „Djarvis, cere-i lui Claude Code să analizeze acest proiect și să îmi spună ce trebuie îmbunătățit.”

Fluxul este: comandă vocală → transcriere → confirmarea sarcinii → agent selectat → progres și rezultat → rezumat vocal/text. Djarvis poate deschide proiectul în VS Code, transmite contextul aprobat și afișa modificările propuse. Pentru sarcini de analiză poate cere doar un plan sau raport. Pentru modificări de fișiere, rularea comenzilor, instalări, commit-uri, push-uri ori acces la servicii externe, nivelul de confirmare este definit de politică și de preferințele utilizatorului.

## Comenzi vocale și text

Fluxul vocal are componente distincte: detectarea activării (wake word), recunoașterea vocală (STT), interpretarea comenzii, executarea și sinteza vocală a răspunsului (TTS). TTS este o componentă obligatorie pentru experiența principală Djarvis, nu o opțiune secundară.

Exemplu de dialog:

> Utilizator: „Djarvis, care este statistica serverului?”
>
> Djarvis: „Serverul principal este disponibil. CPU este la 18%, memoria la 43%, iar spațiul liber este 62%. Nu există alerte critice.”

Răspunsul vocal trebuie să confirme clar ce a fost înțeles, ce acțiune a fost executată și care este rezultatul. Pentru cereri ambigue sau acțiuni cu efect extern, Djarvis întreabă verbal înainte de a continua. Sistemul poate funcționa și fără wake word, printr-un buton sau chat, pentru confidențialitate, accesibilitate și testare.

Pentru fiecare comandă, interpretarea va produce o intenție clară, de exemplu `open_application`, `create_calendar_event`, `draft_email`, `server_status`. Dacă informația este ambiguă, Djarvis întreabă; nu ghicește și nu execută.

## IA: etapă ulterioară, nu dependență inițială

Integrarea IA va fi un adaptor separat. Aceasta poate ajuta la înțelegerea limbajului natural, rezumate, analiză și planificarea sarcinilor, însă nu primește implicit drept de executare. Orice acțiune propusă de IA trece prin aceeași politică de permisiuni, validare și audit ca o comandă clasică.

„Antrenarea” nu înseamnă că modelul primește automat toate datele personale. Vom defini ulterior separat: ce date pot fi folosite, unde sunt stocate, perioada de retenție, metoda de anonimizare și dreptul de ștergere.

## Referință de inspirație

[Priler/jarvis](https://github.com/Priler/jarvis) este o referință tehnică pentru ideea de asistent vocal local, cu componente precum STT, TTS și wake word. Djarvis este un proiect independent: nu copiem codul, designul sau activele lui. Înainte de reutilizarea oricărui element se verifică licența și compatibilitatea acesteia cu proiectul nostru.
