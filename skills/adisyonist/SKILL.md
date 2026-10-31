---
name: adisyonist
description: How to answer restaurant-operations questions with the Adisyonist MCP tools — sales/revenue (ciro), open tables (açık masalar), reservations (rezervasyon), staff shifts (vardiya) and low stock (kritik stok) — plus ready-made playbooks for a daily report, a stock check and a week-over-week comparison. Use whenever the user asks about their restaurant, café or POS data in Adisyonist.
---

# Working with the Adisyonist tools

The `adisyonist` server is the user's own restaurant POS data, **read-only**. It fetches and computes;
**you** interpret, compare and summarise. Answer in the user's language (most users write Turkish).

## Ground rules
- Every tool already returns only the signed-in business's data. Never ask the user for a business id.
- Dates are `YYYY-MM-DD` in the **business's local time zone**. Omit the date for "today".
  Resolve "dün", "bu hafta", "geçen ay" yourself into explicit dates before calling.
- `satis_ozeti` counts **business days**, like the app's own report: a day runs from the business's
  day-end time (default 04:00, see `donem.is_gunu_bitis`) to the same time next day, so a check closed
  at 02:00 belongs to the previous day. Omit the date and the tool picks the current business day itself
  (before 04:00 that is still "yesterday"). Reservations and shifts use calendar days.
- Amounts are in `para_birimi` and are the **net collected amount** (after discounts).
  `indirim_toplam` is the discount given; gross = `ciro + indirim_toplam`.
- Multi-branch businesses: call `subeler` first, then pass `sube_id`. Without it, tools cover all branches.
- Period tools accept at most 92 days per call. For longer periods call per month and add up.
- Analytics follow the app's own report rules: a check counts for the staff member who opened the table
  (else who closed it), categories come from the menu, legacy checks without a branch belong to the default branch.
  `kategori_satis` revenue is per item before bill discounts, so it can differ slightly from `ciro`.
- If a tool returns an error, explain it plainly; do not invent numbers.
- Data is read-only: you cannot open/close tables, change prices or edit reservations. Say so if asked.

## Which tool
| Question | Tool |
|---|---|
| Ciro, adisyon sayısı, ortalama hesap, ödeme yöntemleri, en çok satanlar | `satis_ozeti(baslangic, bitis?, sube_id?)` |
| Şu an dolu masalar, açık hesap toplamı | `acik_masalar(sube_id?)` |
| Bir günün rezervasyonları | `rezervasyonlar(tarih?, sube_id?)` |
| Bir günün vardiyaları | `vardiyalar(tarih?, sube_id?)` |
| Minimum seviyenin altındaki stoklar | `kritik_stoklar(sube_id?)` |
| Şubeler ve kimlikleri | `subeler()` |
| En yoğun saatler, haftanın günleri, yoğunluk haritası | `saatlik_satis(baslangic?, bitis?, sube_id?)` — default last 7 days |
| Kategori bazında ciro ve en çok satanlar | `kategori_satis(baslangic?, bitis?, sube_id?)` |
| Personel başına adisyon, ciro, ortalama | `personel_performansi(baslangic?, bitis?, sube_id?)` |
| Şubeler yan yana | `sube_karsilastirma(baslangic?, bitis?)` |
| Gün/hafta/ay serisi, önceki dönemle % değişim | `satis_trendi(baslangic?, bitis?, sube_id?, gruplama?, onceki_donemle_karsilastir?)` — default last 7 days |

## Playbooks

### Günlük rapor (daily report)
Call in parallel: `satis_ozeti` (today), `acik_masalar`, `rezervasyonlar` (today), `vardiyalar` (today), `kritik_stoklar`.
Report in this order, short and scannable:
1. **Ciro** — total, number of checks, average check, discount; one line on payment mix (nakit / kart / yemek kartı).
2. **Şu an** — open tables and open-bill total; mention the largest open bill.
3. **En çok satanlar** — top 3 items with quantity.
4. **Bu akşam** — reservation count and total guests; list times.
5. **Ekip** — who is on shift.
6. **Dikkat** — critical stock items (name, current vs minimum). Skip the section if empty.

### Stok kontrolü (stock check)
Call `kritik_stoklar`. Turn the result into a purchase list grouped by branch: item, current, minimum,
and a suggested order of `minimum × 2 − current` (rounded up, in the item's unit). State that the
suggestion is a rule of thumb, not a forecast.

### Haftalık karşılaştırma (week over week)
Compute this week (Monday → today). Call `satis_trendi(baslangic=Monday, bitis=today)`: it returns the daily
series and compares with the immediately preceding period of the same length (the same weekdays of last week).
Show revenue, checks and average check side by side with the % change from `karsilastirma.degisim_yuzde`.
Call `satis_ozeti` for both ranges if top items are needed, and say what moved up or down.
Point out if this week is still in progress.

### Yoğunluk ve personel planı (staffing)
Call `saatlik_satis` for the last 4 weeks and `personel_performansi` for the same period. Name the busiest
weekday/time slots from `isi_haritasi` and `haftanin_gunleri.ortalama_gunluk_ciro`, and compare with `vardiyalar`
for the coming days. Keep it to suggestions; you cannot change shifts.

## Style
- Lead with the number the user asked for, then the context.
- Use the business currency symbol (TRY → ₺) and thousands separators of the user's locale.
- Keep tables small; no more than 5 rows unless the user asks for everything.
