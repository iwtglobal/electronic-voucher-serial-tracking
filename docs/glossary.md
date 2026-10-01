# Glossary — Electronic Voucher Serial Tracking

Educational terminology for **electronic voucher serial tracking**, PIN serial audit, and EVD serial lifecycle. Definitions are industry-oriented; vendor labels vary.

## Core Terms

| Term | Definition |
|------|------------|
| **Serial** | Unique identifier for one voucher unit (inventory and support key) |
| **PIN** | Secret redeem credential bound to a serial |
| **Batch** | Generation run that produces many serials under shared metadata |
| **PIN vault** | Encrypted or HSM-backed store for PIN material |
| **Custody** | Current inventory holder (operator, dealer, POS float) |
| **Range transfer** | Controlled move of a contiguous serial block |
| **Activation** | State change making a serial redeemable (often at sale) |
| **Redemption** | Successful use of PIN that consumes the voucher |
| **Void** | Ops/fraud removal from sellable or redeemable stock |
| **Expiry** | Time-based end of redeem eligibility |
| **PIN serial audit** | Attributable trail of serial events without cleartext PINs |
| **Lockout** | Temporary block after repeated bad PIN attempts |

## Flow States (Typical)

```
generated → in-stock → transferred → sold/activated → redeemed
                    └→ void / expired / quarantined
```

## Roles

| Role | Concern |
|------|---------|
| **Operator / issuer** | Generates serials; owns vault and policy |
| **Dealer / reseller** | Holds custody float; sells at POS |
| **POS / channel** | Presents sale and redeem entry points |
| **Customer** | Redeems with PIN; may quote serial for support |
| **Compliance / audit** | Reviews custody and state timelines |

## Design Notes

- Never log **cleartext PINs**; log attempt results and serial only.  
- Prefer **serial-level** custody events even when UI offers range moves.  
- Keep **void** and **expiry** as explicit states with actors and timestamps.  
- Correlate serial, batch, dealer, and redeem ID on every success path.

## Related Reading

Live product reading: [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system/), [EVD complete guide](https://evdsystem.com/electronic-voucher-distribution-system-a-complete-guide/).

MoboGage / EVD System materials on evdsystem.com describe electronic voucher distribution capabilities built around serial lifecycle and inventory control.
