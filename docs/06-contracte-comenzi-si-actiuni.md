# Contracte pentru comenzi și acțiuni

## Principiu

Orice intrare — voce, Telegram, aplicație sau agent de programare — devine mai întâi o comandă normalizată. Nicio integrare nu execută direct textul primit de la utilizator.

## Model de comandă

```json
{
  "command_id": "cmd_01J...",
  "correlation_id": "cor_01J...",
  "source": "voice",
  "user_id": "usr_01J...",
  "device_id": "dev_01J...",
  "transcript": "Djarvis, deschide Chrome și caută vremea",
  "intent": "browser.search",
  "parameters": {
    "browser": "chrome",
    "query": "vremea"
  },
  "confidence": 0.93,
  "created_at": "2026-07-30T12:00:00Z"
}
```

`transcript` poate fi omis sau protejat conform setărilor de confidențialitate. Parametrii sunt validați față de schema intenției înainte de autorizare.

## Intenții inițiale

| Intenție | Parametri tipici | Risc implicit |
| --- | --- | --- |
| `app.open` | `app_name`, `device_id` | mic |
| `browser.search` | `browser`, `query` | mic |
| `browser.update` | `browser`, `device_id` | mediu |
| `calendar.create` | `title`, `start_at`, `duration`, `attendees` | mediu |
| `message.draft` | `template_id`, `recipient`, `variables` | mic |
| `message.send` | `draft_id` | mediu |
| `server.status` | `target_id`, `metric_set` | mic |
| `server.operation` | `target_id`, `operation` | ridicat |
| `developer.task` | `project_id`, `agent`, `mode`, `task` | variabil |

## Stările acțiunii

```text
received → parsed → awaiting_approval → queued → running → succeeded
                        │                         ├──────→ failed
                        └──────────→ rejected     └──────→ cancelled
```

O acțiune nu poate trece la `queued` sau `running` dacă permisiunea sau aprobarea cerută lipsește. Răspunsul vocal spune întotdeauna starea pe înțelesul utilizatorului.

## Cerere de aprobare

```json
{
  "action_id": "act_01J...",
  "summary_voice": "Vrei să actualizez Google Chrome pe MacBook?",
  "risk": "medium",
  "expires_at": "2026-07-30T12:05:00Z",
  "allowed_responses": ["approve", "reject"]
}
```

Aprobarea poate veni vocal pe dispozitivul activ, din aplicație sau Telegram, în funcție de politică. O aprobare expirată nu este reutilizată.

## Rezultat normalizat

```json
{
  "action_id": "act_01J...",
  "status": "succeeded",
  "voice_response": "Am deschis Chrome și am pornit căutarea pentru vremea de astăzi.",
  "display_summary": "Chrome deschis; căutare pornită.",
  "details": {
    "target_device": "MacBook"
  },
  "completed_at": "2026-07-30T12:00:03Z"
}
```

`voice_response` este obligatoriu pentru o comandă vocală, cu excepția erorilor de redare audio; în acel caz, aplicația afișează textul și trimite o notificare.

## Exemple de endpoint-uri VPS

| Metodă | Rută | Utilizare |
| --- | --- | --- |
| `POST` | `/v1/commands` | primește comandă normalizată |
| `GET` | `/v1/commands/{id}` | citește starea și rezultatul |
| `POST` | `/v1/actions/{id}/approve` | aprobă o acțiune validă |
| `POST` | `/v1/actions/{id}/reject` | respinge acțiunea |
| `GET` | `/v1/devices` | listează dispozitivele utilizatorului |
| `POST` | `/v1/devices/{id}/revoke` | revocă un dispozitiv |
| `GET` | `/v1/health` | verificare tehnică protejată după necesitate |

Specificația OpenAPI, schemele de validare și codul generat se vor adăuga în `packages/contracts/` când începe implementarea API-ului.
