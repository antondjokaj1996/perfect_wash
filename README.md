# Perfect Wash — informativna stranica

Statična jednostrana prezentacija za **Perfect Wash** (vešeraj i hemijska perionica,
Dušana Milutinovića 1, Zabjelo, Podgorica). Rekreirano po uzoru na
`perfect-wash-demo.lovable.app`, sa dodatim logotipom firme i paletom izvedenom
iz logotipa (brand plava `#315B89`).

## Sadržaj

```
index.html          cijela stranica (HTML + CSS + minimalni JS, jedan fajl)
robots.txt
assets/
  logo.png              logo sa wordmarkom, transparentna pozadina (koristi se u hero)
  logo-mark.png         samo znak (ruka + mjehur) — header
  logo-mark-white.png   bijela verzija znaka — footer i watermark
  logo-circle.png       okrugla "sticker" verzija — og:image / social preview
  icon.png              app ikonica (plavi krug + bijeli znak), 512×512
  favicon.png           64×64
  apple-touch-icon.png  180×180
  logo-original.jpg     originalna fotografija logotipa (izvor, ne koristi se na stranici)
```

## Sekcije

Hero → O nama → Zašto baš mi (6) → Kako funkcioniše (3 koraka) → Usluge (4 + CTA)
→ Statement band → Cjenovnik → Povjerenje / statistika → FAQ → Lokacija → Kontakt → Footer.

Anchor linkovi: `#usluge`, `#kako`, `#cjenovnik`, `#lokacija`, `#faq`.

## Tehnički detalji

- Bez build koraka, bez frameworka, bez dependency-ja.
- Jedini vanjski resurs je Google font **Manrope** (`fonts.googleapis.com`), sa
  system fallback stackom ako se ne učita.
- Fotografije nisu korištene — sve vizuale su CSS/SVG + logo, pa nema slomljenih slika.
- FAQ akordeon je `<details>`/`<summary>` — radi i bez JavaScripta.
- JS radi samo tri stvari: sjena na sticky headeru, reveal animacija (poštuje
  `prefers-reduced-motion`) i godina u footeru.
- SEO: meta description/keywords, Open Graph, Twitter card, `LocalBusiness`
  (`DryCleaningOrLaundry`) i `FAQPage` JSON-LD.
- Mobilno: sticky "Pozovi" bar na dnu ispod 700px.

## Lokalni pregled

```bash
open /Users/anton/git/perfect_wash/index.html
```

Radi direktno preko `file://` — sve putanje su relativne, nema `fetch`-a ni ES modula.
Lokalni server treba samo ako se testira `robots.txt` ili apsolutni `og:image` URL:

```bash
python3 -m http.server 8791 --directory /Users/anton/git/perfect_wash
```

## Hosting

Folder je statičan — dovoljno je uploadovati `index.html`, `robots.txt` i `assets/`.

- **Netlify / Vercel / Cloudflare Pages** — drag & drop foldera, bez build komande.
- **GitHub Pages** — commit u repo, Settings → Pages → branch root.
- **Klasični shared hosting** — FTP u `public_html/`.

## Domen: Namecheap → GitHub Pages

Hosting ostaje GitHub Pages (besplatno, custom domen i SSL uključeni). Plaća se samo domen.

### 1. GitHub
Repo → **Settings** → **Pages** → **Custom domain** → `perfectwash.me` → Save.
GitHub tada commituje `CNAME` fajl u repo. Fajl je već u ovom folderu — **ne brisati ga**,
jer bez njega custom domen prestaje da radi pri sljedećem uploadu.

### 2. Namecheap DNS
Domain List → Manage → **Advanced DNS**.

Prvo obrisati default parking zapise:
- `URL Redirect Record` za `@`
- `CNAME` `www` → `parkingpage.namecheap.com`

Pa dodati:

| Type | Host | Value |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| AAAA | @ | 2606:50c0:8000::153 |
| AAAA | @ | 2606:50c0:8001::153 |
| AAAA | @ | 2606:50c0:8002::153 |
| AAAA | @ | 2606:50c0:8003::153 |
| CNAME | www | antondjokaj1996.github.io. |

AAAA zapisi su opcioni (IPv6), ali se preporučuju.
Ne koristiti Namecheap "URL Redirect" za apex — mora A zapis.

### 3. HTTPS
Kad GitHub završi DNS provjeru, u Settings → Pages uključiti **Enforce HTTPS**.
Certifikat (Let's Encrypt) obično dođe za nekoliko minuta, GitHub navodi do 24h.

### Napomene
- Putanje u `index.html` su relativne, pa rade i na `/perfect_wash/` i na root domenu —
  ništa se ne mijenja pri prelasku.
- Stari `antondjokaj1996.github.io/perfect_wash/` se nakon toga sam 301-redirectuje na domen.

### Prije puštanja u produkciju

1. ~~`canonical`~~ — postavljen na `https://perfectwash.me/`.
2. ~~`og:image` / `twitter:image`~~ — apsolutni URL-ovi postavljeni.
3. Provjeriti koordinate u JSON-LD-u (`42.4209027, 19.2504929`) i Google Maps linkovima.
4. Opciono: `assets/logo-original.jpg` se može izbrisati — nije referenciran.
5. Ako domen NIJE `perfectwash.me`: zamijeniti ga u `CNAME` fajlu i na 5 mjesta u
   `index.html` (`canonical`, `og:url`, `og:image`, `twitter:image`, JSON-LD `image`/`logo`/`url`).
