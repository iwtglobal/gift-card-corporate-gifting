# Architecture — Gift Card Corporate Gifting

High-level reference for evaluating **gift card corporate gifting**, bulk B2B gift cards, and corporate gift card programs. Educational only; vendor designs vary.

## Logical Components

```
┌──────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│ Buyer admin /    │────►│ Program + budget │────►│ Order / PO      │
│ HR / marketing   │     │ configuration    │     │ approval engine │
└──────────────────┘     └────────┬─────────┘     └────────┬────────┘
                                  │                        │
                                  ▼                        ▼
                         ┌──────────────────┐     ┌─────────────────┐
                         │ EVD inventory /  │────►│ Delivery        │
                         │ card issuance    │     │ adapters        │
                         └──────────────────┘     └────────┬────────┘
                                                           │
                                                           ▼
                                                  ┌─────────────────┐
                                                  │ Redeem + report │
                                                  │ + liability     │
                                                  └─────────────────┘
```

## Suggested Decision Metadata

| Field | Meaning |
|-------|---------|
| `program_id` | Corporate program configuration scope |
| `order_id` | Idempotent PO / bulk issuance request |
| `buyer_org_id` | Contracting organization |
| `denom_mix` | Face values and quantities ordered |
| `delivery_mode` | `email` / `sms` / `api` / `print` / `claim_link` |
| `activation_mode` | `immediate` / `delayed` / `first_use` |
| `approver_id` | Present when dual control is required |
| `liability_snapshot` | Issued − redeemed − expired for reporting |

## Audit Events Worth Keeping

- Program create/update (rules, budget, brands)  
- Order approval and funding confirmation  
- Bulk allocate / generate with serial or card ID ranges  
- Delivery success, bounce, and retry  
- Activation and redemption events per instrument  
- Program liability exports to buyer and operator finance  

## Failure Modes to Design For

- Double issuance from order retries (require idempotent `order_id`)  
- Recipient list larger than funded budget  
- PIN leakage in application logs or support tools  
- Orphan issued cards with no delivery record  
- Untracked breakage when expiry policies differ by jurisdiction  

## Related Reading

Live product reading: [evdsystem.com](https://evdsystem.com/), [About MoboGage](https://evdsystem.com/about-mobogage/).

Platforms in the MoboGage / EVD System family keep corporate gift card issuance auditable so finance can reconstruct program liability without rewriting voucher history.
