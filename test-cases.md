# Test cases

Expected results are derived from the workflow logic. Run each case with a throw-away `example.com` email, then delete the test contacts in HubSpot and test rows in the sheet.

| # | Scenario | Input highlights | Expected result |
|---|---|---|---|
| 1 | Valid HOT SaaS lead | Industry SaaS, high budget, decision maker, immediate timeline, clear pain points | Score ≥ 80, tier `HOT`, route `HOT_SaaS`, HubSpot contact created, audit row added, SaaS email sent |
| 2 | Valid HOT E-commerce lead | Same as 1, industry E-commerce | Route `HOT_Ecommerce`, E-commerce sales email |
| 3 | HOT, other industry | Same as 1, industry Professional Services | Route `HOT_General`, General sales email |
| 4 | WARM lead | Medium budget and need, 3–6 month timeline | Score 50–79, route `WARM`, nurture email |
| 5 | COLD lead | Low budget, low need, no pain points | Score < 50, route `COLD`, marketing nurture email |
| 6 | Invalid email | `not-an-email` | Stops after validation, Telegram "Invalid Lead Blocked", no HubSpot call, no row |
| 7 | Missing required field | Empty company | Same as 6 |
| 8 | Duplicate lead | Re-submit an email already in the sheet | Telegram "Duplicate Lead Skipped", no LLM call, no second HubSpot contact |
| 9 | VIP by score | Score ≥ 90 | Extra Telegram VIP alert, normal flow continues |
| 10 | VIP by budget | Budget `$5,000+`, low score | VIP alert fires (OR condition), tier follows score |
| 11 | HubSpot failure | Use a wrong HubSpot token | Telegram "HubSpot Sync FAILED" after retries |
| 12 | Unrouted lead | Force a route value that matches no branch | Telegram "Unrouted Lead — Manual Review Needed" |
| 13 | Case / whitespace | ` Test@Example.COM ` | Email normalised to `test@example.com`, duplicate check works |
| 14 | LLM returns malformed JSON | Break the parser schema | Output parser error; confirm the failure is visible in the n8n execution log |
