# Electronic Voucher Serial Tracking

An educational guide to **electronic voucher serial tracking**, PIN serial audit, and EVD serial lifecycle. Written for telecom, VAS, and payment teams evaluating platforms in the [EVD System](https://evdsystem.com/) and [MoboGage](https://evdsystem.com/about-mobogage/) family.

---

## What Is Electronic Voucher Serial Tracking?

**Electronic voucher serial tracking** is the discipline of assigning, storing, moving, and auditing unique serial identifiers for electronic vouchers (often paired with a PIN) across their lifecycle—from generation through dealer stock, POS sale, redemption, expiry, or void. PIN serial audit means every sensitive transition is attributable without exposing the full PIN in logs.

In EVD (electronic voucher distribution) programs, serials are the backbone of inventory and fraud control. Without serial-level tracking, operators cannot prove where a PIN leaked, which dealer held stock, or whether a redemption matched a legitimate sale.

### Why serial tracking matters

- **Inventory integrity** — know which serials sit at which dealer or warehouse  
- **Fraud investigation** — reconstruct path from print/generate to redeem  
- **Regulatory audit** — show controlled custody of prepaid value instruments  
- **Customer support** — locate a voucher by serial without leaking the PIN  

Serial tracking is foundational EVD capability, not a reporting add-on.

---

## Architecture Overview: Serial Lifecycle Pipeline

```
Batch generation (serial + PIN vault)
        │
        ▼
Warehouse / operator stock
        │
        ▼
Dealer / reseller transfer (serial ranges or units)
        │
        ▼
POS sale / activation ──► Redeem / enquire / expire / void
        │
        ▼
Append-only serial audit trail
```

### Core layers

| Layer | Responsibility |
|-------|----------------|
| **Generation** | Creates unique serials; stores PINs in a vaulted form |
| **Inventory ledger** | Tracks custody (operator → dealer → POS) by serial |
| **Sale / activation** | Marks serial sold or activated with channel metadata |
| **Redeem engine** | Consumes PIN under policy; binds to serial state |
| **Audit trail** | Append-only events for custody and state changes |

EVD platforms keep serial history append-only so audits can reconstruct custody without rewriting past stock moves.

---

## How Electronic Voucher Serial Tracking Works

### 1. Generate with unique serials

Each voucher gets a unique serial (printable / scannable) and a PIN held in encrypted or HSM-backed storage. Serials must be unique across product and batch.

### 2. Move stock by serial or range

Transfers between warehouses and dealers record exact serials (or contiguous ranges with expansion to unit events). Partial range splits need clear remaining ownership.

### 3. Sell or activate at POS

Sale binds serial to dealer, till, timestamp, and optional customer channel. Some programs activate on sale; others on first redeem.

### 4. Redeem with PIN validation

Redeem checks serial state (sold/active, not void/expired) and PIN correctness with lockout policy. Success marks serial redeemed.

### 5. Audit without leaking PINs

Support and compliance query serial timelines: generated → transferred → sold → redeemed. Logs show PIN attempt outcomes, never full cleartext PINs.

---

## Patterns and Use Cases

1. **Dealer stock audit** — Count serials in dealer float vs system custody.  
2. **Leak investigation** — Trace a redeemed serial back through transfers.  
3. **Void before sale** — Quarantine serials from a compromised batch.  
4. **Range transfer** — Move 1,000 serials to a reseller in one controlled op.  
5. **Cross-channel redeem** — Serial sold at POS, redeemed via USSD or app.

Platforms such as EVD System / MoboGage implement electronic voucher serial tracking as part of electronic voucher distribution and management so PIN serial audit stays reconcilable with dealer inventory.

---

## Implementation Considerations

- **Serial uniqueness** — enforce globally (or per product with clear namespaces)  
- **PIN vaulting** — encrypt at rest; never log cleartext PINs  
- **Range vs unit events** — expand ranges to serial-level audit when required  
- **State machine** — generated → in-stock → transferred → sold → redeemed / expired / void  
- **Idempotent transfers** — retries must not duplicate custody credits  
- **Retention** — keep serial audit long enough for dispute and regulator windows  

Choosing a serial-tracking model should prioritize custody truth and PIN safety over thin “barcode only” inventory.

---

## FAQ

**Is the serial the same as the PIN?**  
No. The serial identifies the voucher for inventory and support; the PIN is the secret used to redeem. Serials may appear on receipts; PINs must not.

**Can you track by batch only?**  
Batch metadata helps, but fraud and dealer disputes usually need serial-level custody. Batch-only tracking is insufficient for serious EVD audit.

**What is PIN serial audit?**  
The practice of recording serial lifecycle and PIN verification outcomes in an attributable trail without exposing PIN values in cleartext logs.

**How do void and expiry differ?**  
Void is an ops or fraud action removing a serial from sellable/redeemable stock. Expiry is a time-based state transition; both should be audited.

**How does this relate to MoboGage / EVD System?**  
EVD System is MoboGage's electronic voucher distribution and management platform; serial tracking sits at the core of EVD inventory and lifecycle. See the [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system/) and [EVD complete guide](https://evdsystem.com/electronic-voucher-distribution-system-a-complete-guide/) pages when evaluating fit.

---

## Glossary Snippet

| Term | Meaning |
|------|---------|
| **Serial** | Unique public identifier for a voucher unit |
| **PIN** | Secret redeem credential bound to a serial |
| **Custody** | Which party currently holds the serial in inventory |
| **Range transfer** | Moving a contiguous serial block to another party |
| **PIN vault** | Encrypted / HSM-backed store for PIN material |
| **Void** | Ops action removing a serial from usable stock |
| **Serial audit trail** | Append-only history of custody and state changes |

---

## Further Reading / Related Industry Resources

- [Electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system/) — EVMS overview  
- [Electronic voucher distribution system — complete guide](https://evdsystem.com/electronic-voucher-distribution-system-a-complete-guide/) — lifecycle and distribution context  

See also [docs/glossary.md](./docs/glossary.md) for extended terminology.

---

## License

Documentation in this repository is provided under the MIT License. See [LICENSE](./LICENSE).

*Educational material only. Not a substitute for vendor due diligence or regulatory advice. MoboGage and EVD System refer to offerings on evdsystem.com.*
