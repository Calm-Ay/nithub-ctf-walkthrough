# NITHUB CTF — Guided Walkthrough

**Author:** Rasaq Ayomide (Calm Ay)
**Lab:** NITHUB Security Lab CTF — web application
**Result:** ✅ Admin payroll unlocked — flag captured
**Live page:** [calm-ay.github.io/nithub-ctf-walkthrough](https://calm-ay.github.io/nithub-ctf-walkthrough/)
**Profile:** [calm-ay.github.io](https://calm-ay.github.io) · [LinkedIn](https://www.linkedin.com/in/rasaq-ayomide-sec) · [GitHub](https://github.com/Calm-Ay)

---

A guided walkthrough of the NITHUB Security Lab web CTF — six stages from a first `robots.txt` read to executive payroll. Tools: Burp Suite, ffuf/gobuster, browser DevTools. Each stage carries the *why*, not just the payload.

## The chain

1. **Recon** — read `robots.txt` for `Disallow: /dev-backup` (same paths ffuf/gobuster would brute-force). Map the attack surface.
2. **Info disclosure** — directory listing on `/dev-backup` exposes `config.php.bak` and `notes.txt`; the notes spell out three weaknesses.
3. **SQLi login bypass** — in Burp Repeater, `' OR '1'='1' --` in the username makes the concatenated login query always-true; auth bypassed.
4. **IDOR to the CEO** — the server serves any `?id=` without an ownership check; walk `1002 → 1005` to reach the CEO record.
5. **Role cookie tampering** — edit `role=employee` → `role=admin` (Burp or DevTools) to unlock `/admin/payroll`.
6. **Capture** — the executive payroll loads and the flag drops.

**Takeaway:** none of the five bugs is exotic. The lesson is chaining — a leaked backup, a string-built query, an unchecked id, and a trusted cookie form one continuous path, not four isolated findings.

## Full report

📄 [`NITHUB_CTF_Walkthrough.pdf`](./NITHUB_CTF_Walkthrough.pdf) — every command, request, and Burp step from recon to flag, with the reasoning and defensive takeaway for each stage.

---

*For educational and authorized penetration-testing purposes only.*
