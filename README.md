# backup solutions: how to compare plans and pricing, apply the 3-2-1 rule, and pick the right protection for your files and servers

Everyone agrees backups matter right up until the moment they have to actually set one up. Then the questions pile up fast: software or cloud service? Per-device or per-GB pricing? What happens to your data when you need to download all of it back? And why do two products that both say "backup" on the tin behave completely differently?

This article walks through the things that actually separate backup solutions from each other, what they cost, and how to match a plan to your situation — whether you're protecting a laptop full of documents or a rack of servers. Along the way, we'll use Sharktech's backup offerings as a concrete worked example, because they cover both of the main approaches: managed backup software and raw S3-compatible storage.

## What actually counts as a backup solution

The first trap in this market is category confusion. A lot of people searching for backup solutions are actually looking at three different kinds of products:

**Backup software with cloud storage bundled.** You install an agent, pick what to protect, and the vendor handles scheduling, encryption, versioning, and the offsite copy. Acronis, Carbonite, Backblaze, and iDrive all work this way. PCMag's current business shortlist includes Acronis Cyber Protect, Backblaze Business Backup, iDrive Team, MSP360, CrashPlan, and Carbonite Professional — all in this category. You're paying for the software doing the work, not just the disk space.

**Raw object storage.** S3-compatible buckets that you point your own tooling at — restic, BorgBackup, rclone, Veeam, or whatever your infrastructure already uses. Nothing is automatic. You get durability, an API, and a bill based on capacity. Whether a backup actually happens is entirely on you and your scripts.

**Cloud sync services.** Dropbox-style folder syncing. These are not backups. They replicate deletes, propagate ransomware-encrypted files, and keep limited version history. The distinction matters enough that storage vendors write whole articles about it, and it's the single most common way people discover their "backup" wasn't one.

If you run servers or manage infrastructure, the second category is usually where you end up — most modern backup tools speak the S3 API natively. If you're protecting endpoints (laptops, desktops, the owner's phone), the first category saves you from becoming your family's or company's unpaid backup administrator.

## The 3-2-1 rule, briefly

Almost every serious guide converges on the same framework: keep **3** copies of your data, on **2** different types of media, with **1** copy offsite. The reasoning is boring and correct — hardware fails, ransomware encrypts everything it can reach, and people delete things by accident.

The offsite copy is the part that usually forces you into this market at all. And here's where the details of a provider's pricing model start to matter a lot, because "offsite" is where hidden costs live.

## The pricing models you'll run into

Backup solutions price in three main ways, and picking the wrong model for your data profile can double your bill.

**Per-device pricing** works well when machines hold a lot of data. Backblaze built its reputation on unlimited-per-computer pricing, which is great value if you have 4TB of photos and painful if you have 40GB of spreadsheets.

**Per-GB pricing** scales with what you actually store. This is where you need to read the overage rates carefully, because the base plan is rarely the whole story — some services charge a different rate per additional GB depending on your billing cycle, which sounds trivial until you outgrow the base tier.

**Flat-rate storage with egress terms.** The storage price is only half the equation. Retrieval (egress) fees are the classic gotcha. Amazon S3 Standard, for reference, runs about $0.023/GB/month for storage on the first 50TB, with egress around $0.09/GB. Storing 1TB there costs roughly $23.55/month — and downloading that terabyte back out costs about $90. A restore is the one time you absolutely need to move all your data, which is precisely when a big egress bill lands. Some providers waive or reduce egress specifically because backup customers hammer them on this point.

A useful habit before signing up anywhere: calculate what a *full restore* would cost, not what storing the data costs. That number tells you more about a backup provider than any feature list.

## Two ways Sharktech approaches backup

Sharktech is a hosting provider — dedicated servers, VPS, OpenStack cloud — with data centers in Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam. That background shapes their two backup offerings, and they solve noticeably different problems.

### Managed cloud backup (Acronis-powered)

Sharktech's cloud backup service is built on a long-term Acronis partnership. It's the install-an-agent-and-forget-it model: the software handles scheduling (daily or hourly, your choice), encryption, and compression, supports both desktop and server operating systems plus mobile devices, and restores can be as granular as a single file or as broad as an entire system. Because it's Acronis under the hood, it also carries the cyber-protection layer — anti-malware, URL filtering, and patch management — rather than being a pure copy operation. That matters for ransomware, since a backup system that can detect an attack in progress gives you a fighting chance of keeping a clean restore point.

The pricing structure is the interesting part:

- **$4/month** for 200GB of cloud storage, then $0.02 per additional GB
- **$8 per 3 months** for 200GB, then $0.04 per additional GB
- **$12 per 6 months** for 200GB, then $0.06 per additional GB
- **$24 per year** for 200GB, then $0.12 per additional GB

Notice the pattern: the longer you prepay, the cheaper the base plan works out per month ($24/year is effectively $2/month), but the *more expensive* the overage rate becomes. The monthly plan has the cheapest additional-GB rate; the annual plan has the priciest. So the right cycle depends entirely on whether you'll stay under 200GB. A file-sync-and-sharing add-on is also available at $0.03/GB monthly (scaling up to $0.24/GB on the annual cycle) if you want Dropbox-like sharing on top of backup.

The math is easy to run. At 200GB you're paying about $0.02/GB on monthly billing. Outgrow that to 500GB and you're at roughly $8/month on the monthly plan — but a 500GB user on the annual plan would pay $24 + (300 × $0.12) = $60/year in overage alone. Prepaying is only a deal if your data volume is predictable.

👉 [Check Sharktech's cloud backup plans and current pricing here](https://bit.ly/SharKTech)

### S3-compatible object storage

The second offering is aimed squarely at the infrastructure crowd: S3 Object Storage, fully S3-API-compatible, running on redundant clusters inside Sharktech's own data centers with triple redundancy, DDoS-protected network, and 40Gbps connectivity. If you use restic, BorgBackup, rclone, Veeam, or any tool that speaks S3, it plugs in without custom work.

The pricing is a flat **$4.90 per TB per month**, and the invoice is deliberately short — storage and bandwidth are the only line items. The entry configuration pairs 1TB of storage with 1TB of bandwidth at $4.90/month total. Compare that to the hyperscaler math above: same terabyte on AWS S3 Standard is about $23.55/month in storage before you pay roughly $90 to retrieve it. (An earlier Sharktech announcement listed a 250GB entry at $4/month; the current published rate on their S3 page is the flat $4.90/TB.)

Two things make this format work for backup specifically. First, no egress surprise — bandwidth is a configurable line item, not a per-GB penalty shot. Second, object storage as a backup target gives you things sync services can't: retention control, separation from the machines being backed up, and straightforward immutability setups, which is what actually stops ransomware from eating your offsite copy along with everything else.

Sharktech's order page currently lists storage configurations of 1, 2, 4, 16, 80, and 100TB, with bandwidth options from 1TB up to 10,000TB, and custom plans available beyond that. 24/7 support from on-site engineers is included across their services, and their cloud carries a 99.999% uptime guarantee.

👉 [Get 1TB of S3 storage at the flat $4.90/TB rate](https://portal.sharktech.net/cart.php?a=add&pid=643&carttpl=s3_storage_cart&billingcycle=monthly&configoption[1858]=13673&configoption[1859]=1&aff=1611)

## Full plan comparison

Here is every backup-related plan Sharktech currently publishes, in one place. Prices are as listed on their official product pages at the time of writing.

| Service | Plan | What's included | Price | Billing cycle | Get it |
| --- | --- | --- | --- | --- | --- |
| Cloud Backup (Acronis) | Monthly | 200GB storage, $0.02/GB additional, anti-malware + encryption | $4.00 | Monthly | [Sign up](https://bit.ly/SharKTech) |
| Cloud Backup (Acronis) | Quarterly | 200GB storage, $0.04/GB additional | $8.00 | Every 3 months | [Sign up](https://bit.ly/SharKTech) |
| Cloud Backup (Acronis) | Semi-annual | 200GB storage, $0.06/GB additional | $12.00 | Every 6 months | [Sign up](https://bit.ly/SharKTech) |
| Cloud Backup (Acronis) | Annual | 200GB storage, $0.12/GB additional | $24.00 | Yearly | [Sign up](https://bit.ly/SharKTech) |
| Cloud Backup add-on | Sync & Share | File syncing/sharing on top of backup | $0.03/GB (monthly rate; higher on longer cycles) | Flexible | [Ask about the add-on](https://bit.ly/SharKTech) |
| S3 Object Storage | 1TB | 1TB storage + 1TB bandwidth, triple redundancy, S3 API | $4.90 | Monthly | [Order 1TB](https://portal.sharktech.net/cart.php?a=add&pid=643&carttpl=s3_storage_cart&billingcycle=monthly&configoption[1858]=13673&configoption[1859]=1&aff=1611) |
| S3 Object Storage | 2TB | Flat-rate storage, bandwidth configured at checkout | $9.80 | Monthly | [Configure](https://portal.sharktech.net/cart.php?a=add&pid=643&aff=1611) |
| S3 Object Storage | 4TB | Flat-rate storage, bandwidth configured at checkout | $19.60 | Monthly | [Configure](https://portal.sharktech.net/cart.php?a=add&pid=643&aff=1611) |
| S3 Object Storage | 16TB | Flat-rate storage, bandwidth configured at checkout | $78.40 | Monthly | [Configure](https://portal.sharktech.net/cart.php?a=add&pid=643&aff=1611) |
| S3 Object Storage | 80TB | Flat-rate storage, bandwidth configured at checkout | $392.00 | Monthly | [Configure](https://portal.sharktech.net/cart.php?a=add&pid=643&aff=1611) |
| S3 Object Storage | 100TB | Flat-rate storage, bandwidth configured at checkout | $490.00 | Monthly | [Configure](https://portal.sharktech.net/cart.php?a=add&pid=643&aff=1611) |
| S3 Object Storage | Custom | Beyond 100TB or unusual bandwidth needs | Quoted | Custom | [Request a custom plan](https://bit.ly/SharKTech) |

All S3 plans run in any of the five data center locations and include DDoS protection on the storage network. Larger bandwidth allocations (up to 10,000TB) are selected during checkout, so the storage price above is the capacity line item only.

## Which type fits your situation

The two Sharktech services map cleanly onto two very different users, and choosing between them is mostly a question about who manages the backup job.

**You want set-and-forget protection for machines.** Laptops, office desktops, a small business's file server, the boss's phone. The Acronis-based service is built for this — install the agent, set a schedule, and restores can be single files or whole systems. The anti-malware layer is a genuine feature here rather than marketing fluff, because endpoint ransomware is the threat model this category exists for. If you'll stay near 200GB, any billing cycle works; if your data grows, the monthly plan's $0.02/GB overage is the one that keeps scaling sanely.

**You run infrastructure and already have backup tooling.** Homelab, VPS fleet, dedicated servers, CI/CD artifacts, database dumps. You don't want an agent — you want a durable, cheap, S3-speaking target with no egress ambush. At $4.90/TB flat, a 4TB backup repository costs $19.60/month, which is the kind of arithmetic that makes self-managed backup with restic or Borg realistic. Just remember the trade: with raw storage, nothing backs itself up. Your retention policy, your schedule, your restore testing.

A pattern that works well and costs very little: run local backups for fast restores, replicate to the S3 bucket for the offsite leg of 3-2-1. Both restic and Borg handle this natively, and rclone can bridge almost anything else.

## What to verify before you commit anywhere

Whoever you end up buying from, the same short checklist applies:

1. **Price a full restore, not just storage.** Egress fees are the number that hurts.
2. **Read the overage rate, not just the headline price.** Base tiers are marketing; overage rates are what you pay after month three.
3. **Check whether versioning/immutability is supported.** This is the difference between a backup and a ransomware-syncing mechanism.
4. **Confirm the data center locations** if latency or data residency matters to you.
5. **Test a restore immediately after setup.** An untested backup is a hypothesis, not a backup.

On reputation: Sharktech has been around roughly two decades, serves over 1,000 businesses, and third-party coverage is consistent with what you'd expect from a mid-size specialist — HostAdvice's 2026 review of their public cloud highlights flexible, transparent pricing, and their Trustpilot presence is real but small (13 reviews at last check), so don't expect the review volume of a consumer brand. Their support model is direct access to engineers by phone and email around the clock, which is a structure that tends to matter more than review counts when something breaks at 2 a.m.

## The short version

Backup solutions split into managed software (pay for automation and anti-ransomware features) and raw object storage (pay for capacity and wire it up yourself). Sharktech straddles both: a $4/month Acronis-powered cloud backup with cyber protection built in, and S3 storage at a flat $4.90/TB with the bandwidth line item kept separate instead of hiding in egress fees. Match the product to who's going to run the backup job, run the overage math before picking a billing cycle, and test a restore the day you set it up. The best backup solution is the one you can prove works — everything else is brochure material.

👉 [Compare Sharktech's backup plans and pricing directly](https://bit.ly/SharKTech)
