# Siteyi GitHub Pages ile yayınlama — adım adım

Site tamamen statik (`index.html` + `data.json`), build/sunucu gerekmiyor. GitHub Pages ücretsiz.

## İlk kurulum (bir kere)

1. **GitHub'da yeni bir repo aç.** github.com → "New repository" → herkese açık (public) olsun
   (GitHub Pages ücretsiz planı public repo istiyor; özel repo istersen GitHub Pro/Team gerekir).
   Örnek isim: `suru-haritasi-site`.

2. **Proje klasöründe git başlat ve bu repoya bağla** (proje kökünde, `Suru_Haritasi/` içinde):
   ```
   git init
   git remote add origin https://github.com/<kullanici-adin>/suru-haritasi-site.git
   ```

3. **GitHub Pages sadece kök dizini veya `/docs` klasörünü kaynak olarak kabul ediyor** — proje
   `data/site/` altında olduğu için en basit yol, yayınlanacak dosyaları `/docs`'a kopyalamak:
   ```
   mkdir -p docs
   cp data/site/index.html data/site/data.json docs/
   ```

4. **Commit ve push:**
   ```
   git add docs .gitignore
   git commit -m "Sürü Haritası sitesi ilk yayın"
   git branch -M main
   git push -u origin main
   ```
   (`.env`, `.venv`, `__pycache__` gibi dosyaları `.gitignore`'a ekle, gizli anahtarları asla push etme.)

5. **GitHub'da Pages'i aç:** Repo → Settings → Pages → "Build and deployment" → Source:
   **"Deploy from a branch"** → Branch: **main**, klasör: **/docs** → Save.

6. Birkaç dakika içinde site şurada yayında olur:
   `https://<kullanici-adin>.github.io/suru-haritasi-site/`

## Her hafta yeni rapor çıktığında (güncelleme)

1. Yeni haftanın `hafta-NN-<kategori>-ayiklama.jsonl` dosyalarını her zamanki gibi üret.
2. Site verisini yeniden oluştur:
   ```
   python site_uret.py
   ```
   Bu, `data/site/data.json`'u geçmiş tüm haftaların verisiyle yeniden yazar.
3. Güncel dosyaları `docs/`'a kopyala ve push et:
   ```
   cp data/site/index.html data/site/data.json docs/
   git add docs
   git commit -m "hafta-NN verisi eklendi"
   git push
   ```
4. GitHub Pages birkaç dakikada otomatik yeniden yayınlar — elle bir şey yapmana gerek yok.

## Alternatif (ileri seviye): GitHub Actions ile `data/site/`'i doğrudan yayınla

`/docs`'a kopyalamak istemiyorsan, `.github/workflows/pages.yml` ile `data/site/` klasörünü
doğrudan Pages'e yükleyen bir Actions iş akışı kurulabilir (`actions/upload-pages-artifact` +
`actions/deploy-pages`). Bunu istersen ayrıca kurarım — günlük kullanım için yukarıdaki
`/docs` kopyalama yöntemi daha basit ve yeterli.
