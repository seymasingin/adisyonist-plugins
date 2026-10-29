---
description: Bugünün (veya verilen günün) restoran raporu — ciro, açık masalar, en çok satanlar, rezervasyonlar, vardiya ve kritik stoklar
argument-hint: "[YYYY-MM-DD | dün] [şube]"
---

Adisyonist skill'indeki "Günlük rapor" iş akışını uygula.

Gün ve şube: $ARGUMENTS
- Boşsa bugün ve tüm şubeler.
- "dün" gibi göreli ifadeleri önce YYYY-MM-DD tarihine çevir.
- Şube adı verildiyse önce `subeler` ile kimliğini bul.

Geçmiş bir gün istendiyse "Şu an" (açık masalar) bölümünü atla; o veri yalnızca anlık durumu gösterir.
