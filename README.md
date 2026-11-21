# ATT&CK Incident Mapper

**[Open the tool →](https://tjackson8817.github.io/OT-ATTACK-Incident-Mapper/)**

Paste an incident narrative in plain language. The tool splits it into attack steps and maps each step to MITRE ATT&CK for Enterprise, ICS and Mobile techniques. It then shows:

- the kill chain by tactic, including tactics where nothing was observed
- which log sources and analytics would have detected each step
- MITRE mitigations, ranked by how many steps of the attack each one breaks
- known groups, malware and campaigns with the most technique overlap

It is built for OT security assessments, tabletop exercises and incident reviews, where an attack crosses from the business network into control systems.

Everything runs in your browser. The incident text is never uploaded.

Part of the Sale Fish OT Tools suite, alongside [OT Risk Analyzer](https://github.com/tjackson8817/OT-Risk-Analyzer), [OT Asset Inventory](https://github.com/tjackson8817/OT-Asset-Inventory) and [OT Segmentation Checker](https://github.com/tjackson8817/OT-Segmentation-Checker).

## Two versions

| | GitHub Pages version (this repo) | claude.ai version |
|---|---|---|
| Link | [tjackson8817.github.io/OT-ATTACK-Incident-Mapper](https://tjackson8817.github.io/OT-ATTACK-Incident-Mapper/) | [claude.ai/artifact/BA4FXhEBtf1jptpq7whp1p](https://claude.ai/artifact/BA4FXhEBtf1jptpq7whp1p) |
| Account needed | None | A Claude account |
| **Map incident** (phrase rules + text matching) | Yes | Yes |
| **Map with Claude** (AI reading of loosely written reports) | No | Yes |
| Exports, save/open mapping, ATT&CK data updates | Yes | Yes |
| Works offline | Yes: download `index.html` and open it | No |

## Quick start

1. Open the tool and paste the incident, one action per sentence, in the order it happened. You can also pick a built-in example from **Load example…**.
2. Keep **Enterprise** and **ICS** on for most OT incidents.
3. Click **Map incident**.
4. On **Attack steps**, untick techniques that don't fit and tick any low-confidence ones you agree with. Add anything missing with the search box.
5. Review the **Kill chain matrix**, **Detection**, **Mitigations** and **Similar threats** tabs.
6. On **Export**, save an Excel workbook, ATT&CK Navigator layers, a CSV or a Markdown report. Click **Save mapping** to keep the engagement and reopen it later.

## Sample

`example-water-treatment-plant-incident.txt` is a synthetic attack on a fictional water plant (phishing → credential theft → VPN and VNC into the HMI → Modbus setpoint changes, alarm suppression, DNP3 cold restart). The `example-water-treatment-plant-incident.*` files are the tool's output for it with no manual edits:

- the Excel workbook
- the CSV
- the Markdown report
- Navigator layers for Enterprise and ICS
- a mapping file you can reopen with **Open mapping**

## Updating ATT&CK data

The tool ships with MITRE ATT&CK data current to **13 October 2026**.

- **In your own browser:** click **Update ATT&CK data** and drop in `enterprise-attack.json`, `ics-attack.json` and `mobile-attack.json` from [github.com/mitre-attack/attack-stix-data](https://github.com/mitre-attack/attack-stix-data). The tool lists the techniques that were added, removed and renamed. Existing mappings follow MITRE's revoked-by links to the new IDs. Loaded data stays in that browser only.
- **For everyone:** the built-in data is rebuilt from the same files and `index.html` is replaced in this repo.

## Changes

- **November 2026:** phrase-rule fixes.
  - "VPN" maps to External Remote Services only when someone actually connects through it.
  - Commonly Used Port (T0885) is no longer added on every mention of Modbus or DNP3.
  - New matches:
    - setpoint changes (T0836)
    - alarm suppression (T0878)
    - cold restarts and stop commands (T0816)
    - telnet (Remote Services)
    - stored passwords (T1552.001, T1555)
    - "identified the PLC" (Remote System Discovery)
  - Exports now download normally outside claude.ai.
- **October 2026:** ATT&CK October 2026 release. Detection strategies and mitigation guidance were added, with Excel export.

## Limitations

Mappings are a starting point for analyst review, not a certified assessment. Similar threats shows technique overlap, not attribution.

---
MITRE ATT&CK® is a registered trademark of The MITRE Corporation. This tool is not produced or endorsed by MITRE.
Sale Fish Marketing & Consulting · [salefishmarketing.com](https://www.salefishmarketing.com/contact.html)
