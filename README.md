<p align="center"><img src="assets/logo.svg" alt="logo" width="400"></p>

![PROVEN](https://img.shields.io/badge/PROVEN-37%20passed-brightgreen)

# AI Audit Trail

**Ed25519 + Merkle + witness. Каждое решение LLM подписано, любая правка обнаруживается.**

---

## За 30 секунд

**Что это:** Audit trail с Ed25519 подписями и Merkle-деревом. Каждое решение LLM подписано, любая правка обнаруживается. Witness в GitHub Actions каждый час.

**Кому:** Банки, страховые, regulated industries — доказать регулятору, что модель видела и решила.

**Почему сейчас:** Лог чата — не доказательство. Audit trail с Merkle-деревом — доказательство, которое регулятор проверяет без доступа к системе.

**Что доказано:** 37 passed + 2 skipped. Ed25519 + Merkle + hourly witness работают. apumia prove → PROVEN.

**Чего не доказано (честно):** Нет production-развёртывания. Witness — только hourly в CI.

---

## Оффер

По запросу.

**Следующий шаг:** Покажу Merkle-proof вживую — 15 минут.

---

## GitHub

- **Private code:** apumiaaifund/ai-audit-trail
- **Public demo:** apumiaaifund/ai-audit-trail-demo

## License

MIT

---

### 🛡️ Верификация качества и безопасности (Apumia DevSecOps Gate)

> **Статус:** `VERIFIED & PROVEN` • **Дата аудита:** `16.11.2026`  
> **Контроль архитектуры (`Apumia Blueprint`):** `CLEAN` (`0` циклических зависимостей, контроль сложности McCabe)  
> **Анализ безопасности (`Bandit / Zero-Trust`):** `0` критических уязвимостей  
> **Протокол достоверности (`Apumia Hub`):** `_synthetic: false` | `preservation: 100.0%` | `verified: true`
