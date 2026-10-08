# Koolimatemaatika · mat.opetaja.ee

Eestikeelne Jekylli saidipõhi repositooriumile **martlaa/mat**. Kohanduv kujundus, klaviatuuriga kasutatavad alammenüüd, kõik kokkulepitud sisulehed ja PDF-kogumike kollektsioon. JavaScripti ega tasulist teemat pole vaja.

## 1. Lisa failid GitHubi

Kopeeri selle kausta **sisu** repositooriumi `martlaa/mat` juurkausta. Ära paiguta kogu saiti eraldi `mat` alamkausta. Kaasa ka peidetud `.github` kaust, kus asub avaldamistöövoog, ja `.gitignore`. Kui repos on olemas failid, võrdle neid enne asendamist.

Gitiga saab seda teha olemasolevas kloonis, failid esmalt sinna kopeerides:

```sh
git add .
git commit -m "Lisa Koolimatemaatika Jekylli veebileht"
git push origin main
```

Töövoog eeldab haru `main`; teise harunime korral muuda `.github/workflows/pages.yml` faili. GitHubi veebiliideses üles laadides veendu eraldi, et ka `.github/workflows/pages.yml` jõudis reposse.

## 2. GitHub Pages ja domeen

1. Ava repo **Settings → Pages** ning vali **Source → GitHub Actions**.
2. Käivita **Actions → Build and deploy Jekyll site → Run workflow** või tee uus muudatus `main` harusse. Oota ehitamise ja avaldamise õnnestumist.
3. Lisa **Settings → Pages → Custom domain** väljale `mat.opetaja.ee` ja salvesta see enne DNS-kirje lisamist.
4. Lisa domeeni `opetaja.ee` DNS-halduses järgmine kirje:

   | Tüüp | Nimi / host | Sihtväärtus | TTL |
   | --- | --- | --- | --- |
   | CNAME | `mat` | `martlaa.github.io` | vaikimisi või 3600 |

   Mõni teenusepakkuja nõuab hostiks täisaadressi `mat.opetaja.ee` või sihtväärtuse lõppu punkti (`martlaa.github.io.`). Sihtväärtusesse ei lisata `https://` ega `/mat`. Kui samal hostil `mat` on varasem A-, AAAA- või CNAME-kirje, lahenda konflikt enne uue kirje lisamist. Juurdomeeni ja `it.opetaja.ee` kirjeid pole vaja muuta.
5. Oota GitHubi DNS-kontrolli õnnestumist ja aktiveeri **Enforce HTTPS**. DNS-i levimine ja sertifikaadi valmimine võivad võtta kuni 24 tundi.

Fail `CNAME` sisaldab `mat.opetaja.ee`. Kohandatud Actions-töövoo puhul määrab domeeni siiski GitHubi **Custom domain** seadistus; ainult failist ei piisa. Fail on kaasas ka võimaliku harupõhise avaldamise jaoks.

GitHub soovitab domeeni omandi enne kasutamist konto **Settings → Pages** kaudu kontrollida. Kasuta seal näidatud TXT-kirjet ja täpset kontrollväärtust.

Konfiguratsioonis on `url: "https://mat.opetaja.ee"` ja `baseurl: ""`. Kui soovid esmalt proovida ilma kohandatud domeenita aadressil `https://martlaa.github.io/mat/`, kasuta ajutiselt `url: "https://martlaa.github.io"` ja `baseurl: "/mat"`. Oma domeenile üleminekul taasta algväärtused ning ehita uuesti.

## 3. Sisu muutmine

- `_data/navigation.yml`: menüüde pealkirjad ja aadressid. Fail kasutab YAML-iga ühilduvat JSON-kuju.
- `index.html`: avaleht.
- `opetajakoolitus/`, `artiklid/`, `projektid/`, `inimesed/`, `sundmused/`, `loputood/`, `kontakt/`: Markdown-vormingus sisulehed.
- `_kogumikud/`: üks Markdown-fail iga kogumiku kohta.
- `_layouts/` ja `_includes/`: ühised lehemallid ja menüü.
- `assets/css/style.css`: kujundus; `assets/images/`: kaanepildid.

Näidistekstid ei sisalda väljamõeldud elulugusid, kontaktandmeid ega sündmusi. Lisa kinnitatud andmed ning eemalda avalehe loomisel-oleku teade, kui sisu on avaldamiseks valmis. KMÜ lühend on jäetud kasutaja antud kujule. Kaastöö esitamise osa asub kogumike lehel.

### Kogumiku lisamine

Loo näiteks `_kogumikud/koolimatemaatika-2025.md`:

```yaml
---
title: "Koolimatemaatika 2025"
year: 2025
editors: "Lisa tegelikud toimetajad"
description: "Lisa kogumiku lühitutvustus."
cover: /assets/images/koolimatemaatika-2025.jpg
pdf_url: /assets/pdf/koolimatemaatika-2025.pdf
---
Lisa tutvustus või sisukord.
```

Lisa vastav kaanepilt ja PDF näidatud asukohtadesse. Failinimed ja ilmumisaasta siin on vormistusnäited. Eemalda `_kogumikud/naidis.md`, kui tegelik kogumik on lisatud. Kogumikud järjestatakse automaatselt aasta järgi kahanevalt.

`pdf_url` võib olla ka täielik `https://`-aadress, sealhulgas Google Drive'i jagamislink. Drive'is määra faili vaatamisõigus soovitud lugejatele ning kontrolli linki välja logitud brauseris. Nupp avab lingi; Drive'i puhul võib avaneda Drive'i eelvaade. PDF-i puudumisel jäta `pdf_url` tühjaks. Kaanepildi puudumisel kasuta kaasas olevat kohatäitepilti või jäta `cover` väli ära.

Kaanepilt on eraldi fail: see lahendus ei loo PDF-ist automaatselt pisipilti. Esimese lehekülje võib eksportida pildiks PDF-lugejast või Poppleri abil:

```sh
pdftoppm -f 1 -singlefile -scale-to 800 -jpeg kogumik.pdf assets/images/koolimatemaatika-2025
```

## 4. Kohalik eelvaade

Vaja on Ruby 3.3 ja Bundlerit. Saidikaustas:

```sh
bundle install
bundle exec jekyll serve
```

Ava `http://localhost:4000`. `_config.yml` muutmisel taaskäivita eelvaade. Kontrollimiseks:

```sh
bundle exec jekyll build --trace
```

Pärast edukat `bundle install` käivitust lisa tekkinud `Gemfile.lock` versioonihaldusse, et lukustada sõltuvuste versioonid. `_site/` väljundit ei ole vaja reposse lisada.

## 5. Kontroll enne avaldamist

Ava nii kitsal kui laial ekraanil avaleht, kõik menüüd ja üks kogumik. Kontrolli klaviatuuriga alammenüüde avamist (Tab, Enter või tühik), PDF-i lugemisõigusi ja kaanepilte. Asenda näidiskirje ning lisa kontaktandmed. Esimene GitHub Actionsi käivitus peab lõppema roheliselt.

Koostamisel kontrolliti konfiguratsioonifailide süntaksit, navigeerimisaadresside vastavust sisulehtedele ning mallide ja varade olemasolu. Kohalikus koostamiskeskkonnas Jekylli ei olnud: tegelikku Jekylli ehitust ega brauseri visuaalkontrolli ei tehtud. Repositooriumi, GitHub Pagesi seadistusi ega DNS-i selle paketi loomisel ei muudetud.

## Ametlikud juhendid

- [GitHub Pagesi kohandatud domeen ja DNS](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
- [Jekyll ja GitHub Actions](https://jekyllrb.com/docs/continuous-integration/github-actions/)
- [Jekylli kollektsioonid](https://jekyllrb.com/docs/collections/)
