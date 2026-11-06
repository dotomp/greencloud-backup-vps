# backup vps hosting: how to set up cheap offsite backups with GreenCloud, from $25 a year (plans, limits, and who should skip it)

Most people searching for "backup VPS hosting" want one of two things: a VPS plan that includes backups, or a cheap server whose only job is to store backups from other servers. GreenCloud (also known as GreenCloudVPS) is a genuinely good answer to the second question, and a qualified answer to the first. Its Storage KVM VPS line starts at $25/year for 500GB, which works out to roughly $50 per terabyte per year — hard to beat for offsite storage. Its Budget KVM line starts at $6/month with one free backup/snapshot included.

The catch, and it matters: GreenCloud's promo plans are marked **no refund**, and you only get **one free backup/snapshot** per VPS. So you'll be building the backup routine yourself. This article covers the full plan list with prices, the real cost math, the fine-print restrictions, and a working backup setup — so you can decide in about ten minutes whether it fits your situation.

> **Quick picks**
> - **Offsite backup target:** Storage KVM VPS, $25/year (500GB) or $45/year (1TB), US/EU locations.
> - **Cheap general-purpose VPS with a snapshot slot:** Budget KVM VPS from $6/month.
> - **Deal-breakers to know first:** no refunds on promo plans, one free backup slot, and APAC locations get roughly half the traffic allowance.
> - 👉 [Check GreenCloud's current plans and which locations are in stock](https://greencloudvps.com/billing/aff.php?aff=6679)

## First, be clear about what "backup VPS hosting" means for you

There are two different products hiding behind this search term, and mixing them up is how people end up disappointed.

**Scenario A: you already have a VPS (or several) and want somewhere safe to send backups.** A second server at a different provider, in a different data center, is the classic offsite backup target. GreenCloud's Storage KVM VPS exists almost exactly for this: big SATA drives in RAID-10, a 10Gbps port, and pricing built around storage rather than CPU. Its own product page is upfront that these are enterprise SATA drives, not NVMe — you're trading IOPS for dollars per gigabyte, which is exactly the right trade for backups.

**Scenario B: you want a VPS where the provider handles backups for you.** Here GreenCloud is weaker than some rivals. Every Budget KVM plan includes exactly one free Backup/Snapshot, and that's the extent of the included backup offering. Compare that to providers like Hostinger, whose VPS plans back up automatically every week by default (daily backups cost extra), or cPanel-based hosts like hostworld, which run daily offsite backups through JetBackup and keep at least three daily copies you can restore yourself. If hands-off backups are your top priority, those models fit better.

One more thing worth killing early: RAID is not a backup. RAID-10 protects you when a drive dies. It does nothing when you delete a folder, a database gets corrupted, or your server gets compromised. That's why the snapshot GreenCloud gives you is a convenience, not a strategy — Liquid Web's backup guide puts it plainly: snapshots are for quick rollback before a big update, and they can't replace full backups.

## Full GreenCloud plan and pricing list

GreenCloud has several product lines (Windows VPS, Ryzen KVM, EPYC NVMe, managed options), but the two lines relevant to backup use are Budget KVM and Storage KVM. Here's everything on the current official pages.

**Budget KVM VPS (Linux, KVM, 10Gbps port, 1 free backup/snapshot included):**

| Plan | Storage | CPU | RAM | Traffic (US/EU) | Price | Order |
| --- | --- | --- | --- | --- | --- | --- |
| SSDKVM-1 | 15GB SSD | 1 core | 1GB | 1.5 TB | $6/mo | [ 订购 SSDKVM-1](https://greencloudvps.com/billing/aff.php?aff=6679) |
| SSDKVM-2 | 15GB SSD | 1 core | 2GB | 2 TB | $10/mo | [ 订购 SSDKVM-2](https://greencloudvps.com/billing/aff.php?aff=6679) |
| SSDKVM-3 | 30GB SSD | 2 cores | 4GB | 4 TB | $20/mo | [ 订购 SSDKVM-3](https://greencloudvps.com/billing/aff.php?aff=6679) |
| SSDKVM-4 | 60GB SSD | 4 cores | 8GB | 8 TB | $40/mo | [ 订购 SSDKVM-4](https://greencloudvps.com/billing/aff.php?aff=6679) |
| SSDKVM-5 | 120GB SSD | 8 cores | 16GB | 12 TB | $80/mo | [ 订购 SSDKVM-5](https://greencloudvps.com/billing/aff.php?aff=6679) |

APAC locations get half the traffic: 750GB, 1TB, 2TB, 4TB, and 6TB respectively for the five plans.

**Storage KVM VPS (Linux, 10Gbps, SATA drives in hardware RAID-10):**

| Plan | Storage | CPU | RAM | Traffic | Price | Order |
| --- | --- | --- | --- | --- | --- | --- |
| Storage-0.5TB | 500GB SATA | 1 core | 2GB | 3 TB | $25/year | [ 订购 Storage-0.5TB](https://greencloudvps.com/billing/aff.php?aff=6679) |
| Storage-1 | 1024GB SATA | 2 cores | 3GB | 6 TB | $45/year | [ 订购 Storage-1](https://greencloudvps.com/billing/aff.php?aff=6679) |
| Storage-2 | 2048GB SATA | 4 cores | 6GB | 10 TB | $80/year | [ 订购 Storage-2](https://greencloudvps.com/billing/aff.php?aff=6679) |

There's also an APAC variant of the same Storage line in Tokyo and Singapore — identical specs and prices ($25/$45/$80 per year), but traffic is halved to 1TB / 2TB / 4TB. US/EU Storage locations are Phoenix AZ, Ogden UT, and Amsterdam NL.

Beyond these two lines, GreenCloud sells Budget/Managed/SEO/Forex Windows VPS (from $8/month), Ryzen KVM from $6/month, EPYC NVMe KVM VDS from $10/month, macOS VPS on real Mac hardware, and dedicated servers. Third-party stock trackers currently list annual EPYC KVM deals around $25/year (1 vCPU, 2GB RAM, 25GB NVMe RAID-10, 3TB traffic) in cities like Chicago, San Jose, Frankfurt, and Los Angeles — those rotate with stock, so treat them as "sometimes available" rather than a standing offer.

## The real cost math for a backup server

This is the part most comparison articles skip, so here are the actual numbers.

A Storage-1 plan costs **$45/year, which is $3.75/month for 1TB of offsite storage**. The 2TB tier is $80/year — about $40 per terabyte annually. For context, backing up to a major cloud object store typically costs more than that in storage fees alone before you count egress or API charges, and you don't get a full Linux box to run scripts on.

If you need more than 2TB, Storage plans can add up to 20TB of extra storage at $5/TB/month. That changes the math: a 3TB setup costs $80/year + 1TB extra × $5/month = $140/year. Still reasonable, but at that point compare against the 2TB tier plus aggressive retention policies (rotating old backups out).

Monthly Billing KVM plans make less sense as pure backup targets. SSDKVM-1 gives you only 15GB for $6/month — that's $72/year for storage that would cost $25/year on the Storage line. The Budget KVM line earns its keep when you need an actual working server (website, VPN, small apps) and want a free snapshot slot as a safety net.

One regional warning: if your backup source server is in Asia and you're pulling or pushing large volumes through a Tokyo or Singapore GreenCloud box, traffic allowances are halved and extra traffic costs **$15/TB/month in APAC versus $3/TB/month in the US/EU** — a 5x difference. For backup workloads where the data volume is the whole point, US/EU locations are usually the cheaper path.

## The fine print that affects your decision

GreenCloud's checkout pages carry restrictions that don't show up in headline pricing. These are the ones that matter.

- **No refund on promo plans.** The Budget KVM Sale plans (the ones stocked in locations like Chicago DC2, Jacksonville, and Kansas City at the time of writing) are explicitly marked "No refund/Money back." Cancellations in general aren't guaranteed a refund either. There's no free trial to fall back on.
- **Late payment has teeth.** Invoices are issued 14 days before the due date. Pay more than 2 days late and a 10% late fee applies; services get suspended 1 day after expiry and terminated after 3 days. If you want an un-refunded VPS recreated, it costs $5.
- **Don't dispute with PayPal or your card issuer.** Filing a payment dispute gets all your services suspended or terminated immediately. Given the no-refund policy, the practical move is to open a support ticket instead — GreenCloud's own review coverage (including a 13-year customer account from Asia Virtual Solutions) describes the team approving payment deadline extensions without conditions.
- **Stock moves around.** The Budget KVM Sale line shows 0 availability at many popular locations (New York, Ashburn, Amsterdam, and others were sold out at the time of checking), with stock concentrating in whichever data centers have capacity. Your preferred city may not be available when you order.
- **Hardware varies by location.** Budget KVM Sale plans on EPYC Rome/Milan CPUs get NVMe drives; plans on older Intel E5 v4 CPUs get SSD RAID-10. Kansas City plans have a /112 IPv6 allocation instead of the usual /64. Minor stuff, but worth knowing before you pick a location for a long-lived backup box.

Payment options are broad: PayPal, credit card, Alipay, and cryptocurrency. That's friendlier than most US-registered hosts offer.

## Setting up the actual backup routine

Since GreenCloud gives you one snapshot and expects you to handle the rest, here's a setup that works on any of their Linux KVM plans. The widely used baseline strategy is simple: daily automated backups, weekly snapshots, an offsite copy at a different provider, and a restore test once a month.

A minimal push-based setup from your source server to a GreenCloud storage box looks like this:

1. **Create a backup directory on the target:** `mkdir -p /backups`
2. **Compress what matters** (from the source server): `tar -czvf /backups/vps_backup_$(date +%F).tar.gz /var/www /etc /home` — adjust the paths to your actual data.
3. **Automate it with a cron job** on the source server, scheduling the transfer (via `rsync` or `scp`) to run nightly.

For retention, the standard guidance is to keep backups for at least 30–90 days, with daily backups for critical data, weekly backups for system files, and monthly full backups for disaster recovery. Since incremental backups only store changes since the last backup, they're the practical choice for daily runs on a budget plan — full backups weekly or monthly.

Two details that trip people up:

- **Test restores.** A backup you've never restored is a hope, not a backup. Pull one file out of the archive once a month and confirm it opens.
- **Use the free snapshot wisely.** One slot means one point-in-time image. The sensible use is a pre-upgrade snapshot you refresh occasionally — layered on top of, and never instead of, the scheduled backups above.

If you're running backups to a Storage plan, note it's SATA in RAID-10 — restore speeds are fine for nightly jobs, but don't expect NVMe-level IOPS if you're restoring hundreds of gigabytes in a hurry.

## How GreenCloud stacks up against the alternatives

A quick, link-free comparison of the backup approaches you'll encounter, using verified current information:

- **Hostinger VPS**: automatic weekly backups included by default; upgrading to daily backups is a paid add-on managed through the schedule settings. Better if you want zero setup; the included cadence is slower.
- **hostworld (cPanel hosting)**: daily offsite backups via JetBackup with self-service restores, at least three daily copies retained. Only applies to their shared/cPanel-style hosting, not raw VPS.
- **hosting.com**: managed VPS customers get manual backups; dedicated servers can add their Cloud Backup (JetBackup-powered). Unmanaged plans leave you on your own — same situation as GreenCloud, often at a higher price.
- **BandwagonHost**: positioned around China-optimized CN2 GIA routes (its entry CN2 GIA-E plan runs $49.99/quarter or $169.99/year per current listings). If your backup source is in mainland China and route quality matters, that's the established pick — at several times GreenCloud's price.

GreenCloud's edge is narrow and specific: cheapest terabytes per year, 10Gbps ports, and about 30 locations including Tokyo (IIJ and SoftBank lines), Hong Kong, Singapore, and two Vietnamese cities — rare at this price tier. Its weakness is equally specific: backups are DIY, and promo plans don't come with a refund escape hatch.

## FAQ

**Does GreenCloud include automatic backups with its VPS plans?**
No recurring automatic backup schedule is included. Each plan comes with one free Backup/Snapshot that you trigger yourself, plus RAID-10 at the hardware level. Anything beyond that, you script.

**Can I get a refund if I change my mind?**
On the Budget KVM Sale plans, no — they're marked no refund at checkout. Cancellations on other plans aren't guaranteed refunds either. Assume money spent is money committed.

**What happens if my invoice is overdue?**
You get the invoice 14 days early. Two days overdue triggers a 10% late fee, suspension comes 1 day after expiry, and termination after 3 days.

**Is a GreenCloud Storage VPS fast enough for backups?**
For nightly rsync or tar jobs, yes — 10Gbps port, SATA drives in hardware RAID-10. It's built for capacity, not IOPS.

**Can I pay with Alipay or crypto?**
Yes. Accepted methods are PayPal, credit card, Alipay, and cryptocurrency.

## Picking a plan before you order

Match the plan to the job:

- **Storing backups from one or two small servers** (under 500GB total): Storage-0.5TB at $25/year. Done.
- **1TB+ of backups, or multiple source servers:** Storage-1 ($45/year) or Storage-2 ($80/year), US/EU location.
- **Backups to or from Asia with heavy transfer volume:** the US/EU Storage plans still usually win — APAC traffic allowances are halved and overage costs 5x more.
- **A running server (site, proxy, small app) plus a snapshot safety net:** Budget KVM from $6/month, or the annual EPYC deals around $25/year when in stock.
- **You want the provider to run backups for you:** GreenCloud isn't the right fit; look at providers with built-in scheduled backups instead.

Before you click buy, check two things: that your preferred location actually has stock, and that you're genuinely fine with the no-refund policy on promo plans. If both check out, 👉 [pick your GreenCloud plan and lock in the current pricing](https://greencloudvps.com/billing/aff.php?aff=6679) — at $25 a year for half a terabyte of offsite storage, the main risk you're taking isn't financial.
