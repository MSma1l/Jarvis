# Arhitectură și structură de proiect

## Principii de organizare

1. O responsabilitate clară pentru fiecare modul.
2. Funcțiile similare stau împreună, nu într-un fișier general foarte mare.
3. Modulele comunică prin interfețe/contracte bine definite.
4. Comenzile, permisiunile și acțiunile sunt separate.
5. Codul specific unei platforme nu intră în nucleul comun.
6. Fiecare integrare externă are propriul adaptor și propriile teste.

## Arhitectură logică

```text
Voce (intrare și răspuns) / aplicație / Telegram
            ↓
  Interpretare comandă (fără IA sau cu IA)
            ↓
 Validare + politică de permisiuni + confirmare
            ↓
       Orchestrator de sarcini
     ↙        ↓         ↘
 dispozitiv  integrări   infrastructură/VPS
     ↓        ↓         ↓
  răspuns vocal + rezultat + jurnal de audit + notificare
```

Nucleul nu trebuie să știe detalii despre macOS, Windows, Telegram sau un provider de calendar. El cere, de exemplu, „deschide aplicația X”, iar adaptorul platformei hotărăște cum este realizată acțiunea.

## Structura propusă

```text
.
├── apps/
│   ├── desktop/                 # aplicația desktop și integrarea macOS/Windows
│   ├── mobile/                  # clientul iOS/iPadOS/Android
│   ├── server/                  # API și worker-e pentru VPS
│   └── telegram-bot/            # adaptorul de chat Telegram
├── packages/
│   ├── core/                    # entități, intenții, contracte și orchestrare
│   ├── commands/                # definiții/validare comenzi
│   ├── permissions/             # politici, confirmări și autorizare
│   ├── voice/                   # wake word, STT, TTS
│   ├── automation/              # deschidere aplicații, joburi și acțiuni locale
│   ├── browser/                 # Chrome/Brave: căutare, navigare și actualizări autorizate
│   ├── developer-tools/         # Codex, Claude Code și VS Code: sarcini și progres
│   ├── documents/                # căutare, rezumare și operații controlate cu fișiere
│   ├── calendar/                # creare și sincronizare evenimente
│   ├── knowledge/                # memorie personală explicită și gestionabilă
│   ├── messaging/               # șabloane, e-mail și mesaje
│   ├── monitoring/              # verificări server, metrici și alerte
│   ├── analysis/                # rapoarte, reguli și analiză de date
│   ├── ai/                      # adaptoare IA opționale, fără acces direct la execuție
│   ├── integrations/            # clienți pentru servicii externe
│   ├── routines/                 # mementouri, programări și automatizări aprobate
│   ├── device-health/            # statusul dispozitivelor înrolate
│   ├── smart-home/               # integrare opțională pentru casa inteligentă
│   ├── notifications/           # Telegram, push și alte canale
│   └── audit/                   # jurnal de activitate și trasabilitate
├── config/
│   ├── examples/                # fișiere exemplu, fără secrete
│   └── policies/                # reguli de permisiuni și praguri de alertă
├── docs/                        # documentația proiectului
├── infrastructure/              # configurare VPS, deployment și monitorizare
├── tests/
│   ├── integration/
│   └── end-to-end/
└── scripts/                     # numai scripturi de dezvoltare/operare documentate
```

Structura poate fi adaptată limbajului ales, dar regulile de separare rămân valabile. Un fișier de tip „utilitare” nu va deveni locul implicit pentru logică fără categorie; funcțiile se mută într-un modul cu nume și responsabilitate explicită.

## Exemple de responsabilități

| Domeniu | Responsabilitate | Nu trebuie să facă |
| --- | --- | --- |
| `voice` | detectează activarea, transformă vocea în text (STT) și răspunde vocal (TTS) | să decidă singur permisiuni sau acțiuni |
| `commands` | transformă intrarea în intenții validate | să execute direct comenzi de sistem |
| `permissions` | decide dacă acțiunea este permisă și necesită aprobare | să integreze calendarul |
| `automation` | execută acțiuni locale autorizate | să interpreteze vorbirea brută |
| `browser` | caută și navighează în browsere autorizate | să trimită formulare sau să se autentifice fără confirmare |
| `developer-tools` | pregătește și transmite sarcini către agenți de programare | să modifice sau publice cod fără politica de aprobare |
| `knowledge` | gestionează memoria explicită a utilizatorului | să păstreze secrete sau conversații fără acord |
| `routines` | programează mementouri și automatizări aprobate | să execute acțiuni riscante fără politică |
| `documents` | găsește și rezumă documente autorizate | să șteargă sau să distribuie fișiere fără confirmare |
| `monitoring` | colectează și evaluează starea serverelor | să repornească servicii fără politică/confirmare |
| `ai` | propune interpretări și planuri | să ocolească permisiunile |
| `audit` | păstrează evenimentele relevante | să păstreze parole sau tokenuri |

## Tipuri de acțiuni

- **Doar citire:** statistici, status server, analiză, căutare.
- **Reversibile:** crearea unei schițe de e-mail sau a unei întâlniri neexpediate.
- **Cu efect extern:** trimiterea unui mesaj, crearea unui eveniment confirmat.
- **Sensibile:** schimbări de configurare, restart servicii, acces la date sensibile.

Ultimele două categorii cer confirmare potrivită politicii; acțiunile sensibile pot cere și aprobare din Telegram sau aplicația mobilă.
