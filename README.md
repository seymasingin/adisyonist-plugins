# Adisyonist — Claude ve Antigravity eklentisi

[Adisyonist](https://adisyonist.com) restoran yönetim sisteminin resmi Claude ve Google Antigravity eklentisi.
Restoranınızın satış, masa, rezervasyon, vardiya ve stok verilerini yapay zekâ asistanınıza **salt okunur** olarak bağlar.

## Neler yapabilirsiniz

- "Bugünkü ciro ne kadar?" — satış özeti, ödeme dağılımı, en çok satanlar
- "Şu an kaç masa açık?" — açık masalar ve açık hesap toplamı
- "Bu akşam kaç rezervasyon var?" — günün rezervasyonları
- "Bugün kim vardiyada?" — personel vardiyaları
- "Hangi ürünler bitmek üzere?" — kritik stoklar ve sipariş önerisi

Hazır komutlar:

| Komut | Ne yapar |
|---|---|
| `/adisyonist:gunluk-rapor [tarih] [şube]` | Ciro, açık masalar, en çok satanlar, rezervasyonlar, vardiya, kritik stoklar |
| `/adisyonist:stok-kontrol [şube]` | Kritik stoklar ve sipariş önerisi |
| `/adisyonist:haftalik-rapor [şube]` | Bu hafta ile geçen haftanın karşılaştırması |

## Gereksinimler

- Adisyonist **işletme sahibi** hesabı
- Aktif **Adisyonist Premium** abonelik

## Kurulum

**Claude Code**

```bash
claude plugin marketplace add seymasingin/adisyonist-plugins
claude plugin install adisyonist@adisyonist
```

Ardından Claude Code içinde `/mcp` komutuyla Adisyonist hesabınıza giriş yapın.

**Claude (web, masaüstü, mobil)**

Ayarlar → Bağlayıcılar → Özel bağlayıcı ekle → `https://mcp.adisyonist.com/mcp`

**Google Antigravity**

```bash
agy plugin install https://github.com/seymasingin/adisyonist-plugins
```

İlk araç çağrısında Antigravity Adisyonist giriş ekranını açar; işletme sahibi hesabınızla giriş yapın.

## Güvenlik ve gizlilik

- Erişim yalnızca **okuma** amaçlıdır; Claude masa açamaz, fiyat değiştiremez, kayıt silemez.
- Giriş Adisyonist'in kendi onay ekranından yapılır; şifreniz Claude ile paylaşılmaz.
- Her bağlantı yalnızca giriş yapan işletmenin verisini görür.
- Bağlantıyı istediğiniz zaman Claude ayarlarından veya `agy plugin uninstall adisyonist` ile kaldırabilirsiniz.

## Depo yapısı

- `.claude-plugin/marketplace.json`, `plugins/adisyonist/` – Claude eklentisi
- `plugin.json`, `mcp_config.json`, `rules/`, `skills/`, `assets/` – Antigravity eklentisi

`skills/adisyonist/SKILL.md` ile `plugins/adisyonist/skills/adisyonist/SKILL.md` aynı dosyadır; birini
değiştirince diğerini de güncelleyin.

## Belgeler

- [Kullanım kılavuzu](https://en.adisyonist.com/user-guide/)
- [Gizlilik politikası](https://en.adisyonist.com/privacy-policy/)
- [Kullanım koşulları](https://en.adisyonist.com/terms-of-use/)
- [İade politikası](https://en.adisyonist.com/refund-policy/)

## Destek

info@adisyonist.com · https://adisyonist.com
