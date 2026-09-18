<div align="center">

# Enquiry Tracker

**Every enquiry, offer and follow-up in one place — with a permanent record of who changed what.**

An internal sales-enquiry system built in-house for **[Pentaaqua Pvt. Ltd.](https://www.pentaaqua.com)**,
a water and wastewater treatment company.

`Build V 6.0.4.9` · `Security Patch 6.0.0.4` · `BXG`

<img src="docs/img/02-dashboard-dark.png" alt="The Enquiry Tracker dashboard" width="100%">

</div>

---

## Why it was built

Enquiries lived in a spreadsheet. That works until three people are editing it, someone
overwrites a status, an offer goes out twice, and nobody can say who changed the deadline or
when. The questions that mattered were never a formula away: *what is pending right now, what
is past its deadline, who needs a call today, and what did we promise this customer last month?*

So the spreadsheet stayed as the store, and everything else was built on top of it: a proper
interface, real accounts, and an audit trail that cannot be edited by the people it records.

---

## The screens

### Signing in

<img src="docs/img/01-sign-in.png" alt="Sign-in page" width="100%">

Password, then a six-digit code emailed to the address on file. A device can be registered so
it is asked for the password only, for thirty days. Sessions last thirty days and survive
closing the browser.

### The dashboard

Ten counters across the top — total, pending, offer sent, under negotiation, accepted, on hold,
regret, order lost, past deadline, follow-up due. Each one is a filter: press *Follow-up due*
and the list below shows only the enquiries that need a call today.

Underneath, the enquiry list. The enquiry number carries its registration date, the project
carries the plant location, and the company carries the person in charge — so a row answers
most questions without being opened.

<img src="docs/img/03-dashboard-light.png" alt="The dashboard in light mode" width="100%">

Light and dark are both first-class. The interface follows the device, or you can pin it.

### One enquiry

<img src="docs/img/04-enquiry-open.png" alt="An enquiry opened" width="100%">

Everything about an enquiry on one screen: the details as registered, the offer and its status,
the deadline, the documents, the follow-up log, and the remarks the team has pinned to it.
Print it, download it as a PDF, or email a summary — without leaving the page.

Every change is written to an audit trail with the time, the person and their initials, the old
value and the new one. Nobody can quietly edit history, including an administrator.

### Team messages

<img src="docs/img/05-messages.png" alt="Team messages" width="100%">

Three kinds of message: a plain conversation, a **remark pinned to an enquiry** (it shows on
that enquiry for everyone who can see it), and a **reminder with a time on it** that comes back
when it is due. Private conversations are private — an administrator cannot read them either.

### On a phone

<div align="center">
<img src="docs/img/06-phone-dark.png" alt="Phone, dark" width="45%">
&nbsp;&nbsp;
<img src="docs/img/07-phone-light.png" alt="Phone, light" width="45%">
</div>

Not a shrunken desktop. The table becomes cards, the filter row folds behind one button, the
header buttons become icons so the person's own name and photo stay on screen, and every clamp
that trims text on a laptop is released so nothing is cut off. Eight text sizes, because the
people using it are not all looking at a 15-inch screen.

---

## What it does

**The list**
- Search across company, enquiry number, project, location, contact and person in charge
- Filter by status, product type, person responsible and date range; sort by any column
- New enquiries glow until each person has opened them — cleared per person, not for everyone
- Follow-up due and past deadline surfaced as chips, not buried in a date column

**One enquiry**
- Details, offer reference, status, deadline, follow-up log, documents and remarks on one screen
- Print, PDF, or email a summary
- Attachments stored against the enquiry
- A full, unalterable history of every field change

**Registration**
- A public form for customers, with multiple products in one enquiry
- A separate internal form for staff
- Staff open the internal form from the dashboard already signed in — no second password

**Team**
- Messages, remarks pinned to enquiries, and timed reminders
- Accounts with roles, approval before a new account can sign in, and per-person initials shown
  against everything they touch

**Assistance**
- Questions answered from the enquiry data, or general questions, through the Google Gemini API
- Nightly automatic backups by email

---

## Security

This holds a company's entire sales pipeline, so it was treated that way. Highlights, at the
level it is sensible to describe in public:

- **Passwords are one-way hashed** with a per-account salt. They cannot be read back by anyone,
  including an administrator — only reset.
- **Two-step sign-in.** The second step will not move without cryptographic proof that the
  password was correct moments earlier, so nobody can skip to guessing six digits for an account
  whose password they do not know.
- **Registered devices** are signed and bound to the account, the browser and the password. A
  key cannot be moved to another machine, and changing the password un-registers every device.
- **Lock-out** after five wrong attempts. A wrong login ID and a wrong password take the same
  time and give the same answer, so account names cannot be discovered by probing.
- **Sessions are signed tokens** tied to an account generation number: a password change or an
  administrator's action ends every open session instantly. Signing out ends only the device you
  are on; *sign out everywhere* is there for a lost phone.
- **Least privilege in the data itself.** Storage links and document identifiers are removed
  server-side before a page is sent to anyone who should not have them — not hidden with CSS.
- **Spreadsheet formula injection closed.** A submitted value beginning with `=`, `+`, `-` or
  `@` was becoming a live formula in the sheet; every written cell is now neutralised.
- **Uploads** restricted to a known list of file types with a size cap, filenames sanitised, and
  nothing given a public link.
- **Outbound mail is fenced** to addresses already on file or the company domain, so the
  database cannot be mailed to an arbitrary address by a signed-in user.
- **Cannot be framed** by another site.

Every claim above is covered by a test that performs the attack rather than inspecting the code.

---

## How it is built

| | |
|---|---|
| **Runtime** | Google Apps Script — no server to patch, no hosting bill |
| **Store** | Google Sheets, so the team can still open the spreadsheet they know |
| **Documents** | Google Drive, private by default |
| **Interface** | One self-contained HTML page — no framework, no build step, no CDN |
| **AI** | Google Gemini API, key held server-side only |
| **Mail** | Sign-in codes, approvals, summaries and backups |

Roughly 6,000 lines of interface and 4,600 lines of server code across eight files.

---

## Versioning

Every change, however small, moves the build number. A **separate security patch number** sits
beside it and moves only when security itself changes:

```
Build V 6.0.4.9 · Security Patch 6.0.0.4 BXG
```

It is shown in the header, in the footer and on the sign-in page, and the first line of every
source file names it — so there is never a doubt about which copy is deployed. The dashboard
compares its own build against the server files and says plainly which side is behind.

---

## Testing

About **180 automated checks**, run in a real browser against the real files before every
release. They do not read the code, they exercise it: forged and tampered device keys, replayed
sign-in handoffs, privilege escalation attempts, formula injection, oversized uploads, brute
force, message isolation between three people, contrast ratios in both themes, and layout at
360, 390, 430 and 1400 pixels wide.

---

## Status

In daily use at Pentaaqua. Being evaluated for a move to self-hosting, which would bring real
first-party sessions and a proper database while keeping a nightly copy written back to the
spreadsheet the team is used to.

---

<div align="center">

Designed and built in-house by **Brijrajsinh Gohil** · **BXG**

This repository is a write-up, not a distribution: the source is not published, and the system
is not offered as a template or a product.

© 2026 Pentaaqua Pvt. Ltd. All rights reserved. · [www.pentaaqua.com](https://www.pentaaqua.com)

</div>
