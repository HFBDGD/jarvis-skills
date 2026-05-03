# Skill: Solar Monitor

## What it is
A live monitoring system for Arnold's home solar panels via the Growatt API. Sends Telegram alerts when power crosses thresholds, plus daily summaries.

## Workflow
**Solar Monitor Phase 1 v4 - Pure HTTP** (ID: JTxWT1L5R2ZjHmUJBurXB) — active, three triggers.

## Triggers
| Trigger | Time | Purpose |
|---|---|---|
| Every 60 Minutes | Hourly | Check power, detect threshold transitions, append to sheet |
| Daily Noon Check | 12:00 PM AEST | Peak power check with appliance recommendations |
| Daily 7PM | 7:00 PM AEST | Daily summary of total generation |

## Alert thresholds
| Event | Condition | Message |
|---|---|---|
| ROSE_ABOVE_1.0kW | Current power >= 1.0 kW | Good time for AC, dishwasher, washing machine |
| DROPPED_BELOW_0.5kW | Current power < 0.5 kW | Turn off non-essential devices |

Alerts only fire on **transitions** — not repeatedly while in the same state.

## Quiet hours
No alerts sent between 8:00 PM and 6:00 AM Melbourne time.

## Appliance power guide (from noon report)
| Power Level | Category | What to run |
|---|---|---|
| >= 4 kW | EXCELLENT | AC, oven, multiple appliances |
| >= 2.5 kW | GOOD | Dishwasher, washing machine |
| >= 1.0 kW | MODERATE | Light appliances only |
| >= 0.5 kW | LOW | Grid preferred |
| < 0.5 kW | VERY_LOW | Grid only |

## APIs used
- Login: `https://server-au.growatt.com/newTwoLoginAPI.do` (session cookie auth)
- Plant data: `https://openapi-au.growatt.com/v1/plant/list` (token auth)

## Storage
Google Sheet: `solar_monitor` (ID: 1mqaXuuMixzqe91d5RrIih_WYFdEa0AuekUx9LGeOaAc)
Columns: timestamp, current_power_kw, alert_type, should_alert

## Telegram
All alerts and summaries go to chat ID: 1534181531

## Known issues
- 7PM daily summary shows 0.00 kWh generated — Growatt API returns zero for today's energy after the inverter shuts down at sunset. The actual daily total is available in the Growatt app.
- Credentials (Growatt username/password + API token) are hardcoded in the workflow nodes — intentional workaround as n8n credential types don't work for this API.
