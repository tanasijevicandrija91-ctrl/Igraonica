# Meni za igraonicu

Jednostavan digitalni meni sa slikama: pice, slatkiši, bezalkoholna i alkoholna pića, na srpskom i engleskom. Roditelj skenira QR kod i odmah vidi meni na telefonu. Nema servera ni baze, Vercel samo servira fajlove.

## Fajlovi

| Fajl | Čemu služi |
|---|---|
| `index.html` | Meni koji gost vidi |
| `menu.json` | Sav sadržaj: naziv, kategorije, artikli, cene, slike, radno vreme |
| `slike/` | Slike artikala |
| `qr.html` | Generator QR kodova za štampu (na sajtu: `/qr`) |
| `vendor/qrcode.js` | QR biblioteka (MIT licenca, Kazuhiko Arase) |
| `vercel.json` | Čisti linkovi i keš za `menu.json` |

## Objava

1. Napravi repo na GitHub-u i ubaci ove fajlove u koren:
   ```bash
   git init
   git add .
   git commit -m "Meni igraonice"
   git branch -M main
   git remote add origin git@github.com:TVOJ-NALOG/meni-igraonica.git
   git push -u origin main
   ```
2. Na vercel.com: **Add New → Project** → izaberi repo → Framework Preset **Other**, sve ostalo prazno → **Deploy**.
3. **Settings → Domains** → dodaj domen, npr. `meni.zvrk.rs`.

Svaki push na `main` je na sajtu za oko pola minuta.

## Izmena cena i artikala

Sve je u `menu.json`. Na GitHub-u: otvori fajl → olovka → izmeni → **Commit changes**.

Artikal sa jednom cenom:

```json
{
  "name": { "sr": "Krofna", "en": "Doughnut" },
  "description": { "sr": "Glazura od jagode i šarene mrvice", "en": "Strawberry glaze and rainbow sprinkles" },
  "image": "slike/krofna.svg",
  "price": 180
}
```

Artikal sa više cena (parče i cela pica, mala i velika porcija):

```json
"variants": [
  { "label": { "sr": "Parče", "en": "Slice" }, "price": 220 },
  { "label": { "sr": "Cela", "en": "Whole" }, "price": 1090 }
]
```

Ostala polja artikla:

- `unit`: količina, npr. `"0,25 l"` ili `"22 cm"`. Prikazuje se pored cene.
- `available`: `false` znači da artikla trenutno nema. Ostaje na meniju, posivljen.
- `description` nije obavezan.

Cena se piše kao broj, bez tačke i valute: `1090`, ne `1.090 din`.

Kategorija ima `name`, boju `color` (hex), opcionu napomenu `note` i opcionu oznaku `badge` (alkoholna pića imaju `"18+"`). Redosled u fajlu je redosled na meniju.

## Slike

Uz meni dolaze nacrtane slike, da meni odmah izgleda gotovo. Kad budeš imao prave fotografije:

1. Slikaj artikle telefonom, položeno, na čistoj podlozi i pri dnevnom svetlu.
2. Iseci ih na odnos **4:3** (npr. 1200×900 px) i smanji na ispod 200 KB. Besplatno ide na squoosh.app (format WebP ili JPG).
3. Ubaci ih u folder `slike/`, npr. `slike/krofna.jpg`.
4. U `menu.json` promeni putanju: `"image": "slike/krofna.jpg"`.

Ako slika nedostaje ili je putanja pogrešna, na kartici ostaje obojena površina umesto slike, pa se meni ne „lomi".

## QR kodovi

1. Otvori `https://tvoj-domen/qr`.
2. Link je već popunjen domenom sajta. Upiši koliko kopija želiš (9 staje na jednu A4 stranu).
3. **Štampaj kartice**, ili preuzmi kod kao SVG (za štampariju) ili PNG.

Polje „Stolovi" je za slučaj da stolovi imaju brojeve. Tada svaki sto dobija svoj kod, ali za igraonicu je obično dovoljan jedan isti kod svuda.

**Štampaj tek kad je pravi domen povezan**, jer QR kod zauvek vodi na link koji je u njemu. Izmene menija ne zahtevaju novu štampu.

## Jezici

Uključeni su srpski i engleski. Meni sam bira jezik telefona, a gost ga menja dugmićima SR i EN u zaglavlju. Za treći jezik (npr. ruski):

1. U `languages` dodaj `{ "code": "ru", "label": "Русский", "locale": "ru-RU" }`.
2. Svim nazivima i opisima dodaj `"ru": "..."`.
3. U `index.html`, u objekat `UI` na vrhu skripte, dodaj `ru` sa prevodom tekstova interfejsa. Bez toga interfejs za taj jezik ostaje na engleskom.

## Lokalno testiranje

Dupli klik na `index.html` neće učitati meni, jer browser ne čita `menu.json` sa diska. U folderu projekta pokreni:

```bash
npx serve .
```

i otvori adresu koju prikaže.
