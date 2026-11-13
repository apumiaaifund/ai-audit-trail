# ai-audit-trail — Public Demo

[![Verified](https://img.shields.io/badge/ai--audit--verify-VALID-brightgreen)](https://github.com/apumiaaifund/ai-audit-trail-demo/releases/tag/v0.1.0-demo)
[![Tests](https://img.shields.io/badge/tests-23%20passed-brightgreen)](https://github.com/apumiaaifund/ai-audit-trail)
[![EU AI Act](https://img.shields.io/badge/EU%20AI%20Act-Article%2012-blue)](https://github.com/apumiaaifund/ai-audit-trail-demo/releases/tag/v0.1.0-demo)


**Offline-verifiable audit trail for AI systems** — AAT + AWR/2 (W3C VC 2.0), EU AI Act Article 12 evidence pack.

Публичная витрина. Полный исходный код доступен по запросу (Private репозиторий + ZIP).

## Проблема

EU AI Act Article 12 (record-keeping) обязателен для high-risk AI с 02.08.2026. Но:
- SaaS-вендоры просят «поверьте нам» — аудитор не может проверить evidence без доверия к вендору
- Логи AI превращаются в PHI/PII-хранилище (GDPR Art. 17 — right to erasure)
- Нет способа доказать, что action был выполнен ДО записи в лог

## Модули

| Модуль | Что |
|---|---|
| merkle.py | Merkle tree с корректной обработкой нечётных листьев (carry upward) |
| aat_schema.py | AAT-совместимая схема события |
| anchoring.py | Трёхуровневое якорение: Merkle + RFC 3161 + Ed25519 + witness |
| audit_trail.py | Append-only HMAC-цепочка, tombstone, export_for_auditor() |
| euaia_evidence.py | Article 12 evidence pack + HTML renderer |
| runtime_gate.py | Pre-execution gate (allow/simulate/deny), запись до выполнения |
| otel_bridge.py | OpenTelemetry GenAI semantic conventions |
| verifier.py | Независимый верификатор Merkle + RFC 3161 + Ed25519 |
| vc_adapter.py | W3C VC 2.0 receipts через awr — **[WIP]** (awr не в PyPI, тесты skip) |
| cli.py | ai-audit-verify — offline verifier |

## Что в demo/

| Файл | Что это |
|---|---|
| demo/aat_bundle.json | Пример AAT-bundle: 10 записей (4 inference + 3 gate + 2 identity + 1 tombstone), Merkle-proofs, Ed25519 anchor |
| demo/evidence.html | EU AI Act Article 12 evidence pack (coverage + events) |


## Identity (Ed25519 + sparse Merkle non-revocation)

Бонус к audit trail: слой identity для AI-агентов. Кто именно действовал — проверяемо офлайн.

- **Ed25519 подписи** — non-repudiation: issuer подписывает приватным, verifier проверяет публичным
- **Sparse Merkle Tree** — настоящее доказательство неотзыва (32 sibling-хеша, verifier сам пересчитывает путь)
- **Интеграция в runtime_gate** — credential проверяется ДО записи в trail
- **DENY при revoke** — отозванный credential → disposition=deny, identity_reason=revoked

### Демонстрация (из demo/aat_bundle.json, 10 записей)

- 4 inference — AI-запросы
- 3 gate (read/write/destructive) — политика pre-execution
- **2 identity** — valid credential → allow, revoked credential → deny
- 1 tombstone — GDPR Art. 17 (append-only удаление)

### Проверка

```bash
ai-audit-verify demo/aat_bundle.json
# VALID
```

Откройте demo/evidence.html — таблица из 10 строк, колонка Proof = OK.

## Ключевые свойства

- Offline, deterministic verification — ai-audit-verify bundle.json на air-gapped машине без сети
- Трёхуровневое якорение — Merkle + RFC 3161 + Ed25519 + witness
- Кросс-экосистемность — AAT-bundles + W3C VC 2.0 receipts под did:key
- Pre-execution gate — запись до действия; destructive-tier без human approval -> DENY
- Tombstone — append-only удаление с сохранением content_fingerprint (GDPR Art. 17)
- 23 теста — 21 unit + 2 VC-адаптер

## Цены

| Уровень | Цена | Кому | Что входит |
|---|---|---|---|
| Verifier (open-core) | Free | Аудиторы, разработчики | CLI, merkle.py, verifier.py, conformance suite |
| Team / Pilot | USD 500-1500/мес | 1 система, до 100k событий | Managed RFC 3161, evidence pack HTML/PDF |
| Business | USD 3000-5000/мес | до 5 систем, 1M событий | + SSO/RBAC, retention, customer-branded portal |
| Custom pilot | USD 15-30K (6-8 недель) | Regulated-контур | Интеграция под ваш сценарий, TSA-настройка, audit support |

Для покупки и вопросов: apumiaaifund@gmail.com

## Лицензия

Proprietary. См. LICENSE. Этот репозиторий — marketing materials only, без исходного кода.
