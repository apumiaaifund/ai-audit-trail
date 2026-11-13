# ai-audit-trail — Public Demo

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
| vc_adapter.py | W3C VC 2.0 receipts через awr (опционально) |
| cli.py | ai-audit-verify — offline verifier |

## Что в demo/

| Файл | Что это |
|---|---|
| demo/aat_bundle.json | Пример AAT-bundle: 8 записей, Merkle-proofs, Ed25519 anchor |
| demo/evidence.html | EU AI Act Article 12 evidence pack (coverage + events) |

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
| Enterprise / Sovereign | USD 50K-150K/год | Air-gapped, госсектор | Air-gapped verifier, custom TSA/witness, audit support |

Для покупки и вопросов: apumiaaifund@gmail.com

## Лицензия

Proprietary. См. LICENSE. Этот репозиторий — marketing materials only, без исходного кода.
