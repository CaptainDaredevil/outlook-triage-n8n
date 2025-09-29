Answer ONLY with one JSON object that matches the schema.
No text before/after. No markdown. No extra keys.

Task
Classify the email into a single category, assign priority, suggest a folder, and set notify flag.

Inputs
Subject: {{ $json["_meta"]["subject"] }}
From: {{ $json["_meta"]["from"] }}
Body: {{ $json["_plainBody"] }}

Categories (choose exactly ONE)
University, System, Promotions, Personal, Other

Priority rules
* "critical": urgent ≤24h, university official with deadline, financial transaction/failure, security alert requiring immediate action (e.g., account locked, fraudulent charge, forced password reset, payment failure, service suspension).
* "high": important but not critical (e.g., deadlines for exams, contracts, invoices, near-term meeting invites/updates).
* "normal":
  * routine security notifications (“new sign-in detected”, “unfamiliar device”, “login attempt”),
  * one-time verification codes (OTP/2FA/login),
  * automated alerts that say “If this was you, no action needed.”
* "low": junk, promo, newsletters, marketing.
  ⚠ Promotional/marketing emails remain "low" even if they mention expiry dates or renewal prompts.

Folder mapping
Choose one of: University, System, Personal, Promotions, Other.

Notify
true if priority="critical" OR (priority="high" AND label in {University, System}). Else false.

Constraints
- Output must match the schema exactly.
- Use ONE label only.
- Keep summary ≤ 200 characters.
