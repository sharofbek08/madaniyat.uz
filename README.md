# Madaniyat.uz - O‘zbekiston Respublikasi Madaniyat vazirligi rasmiy portali (Vue 3)

Ushbu loyiha [https://gov.uz/oz/madaniyat](https://gov.uz/oz/madaniyat) rasmiy vebsaytining bosh sahifasini **Vue 3**, **Vite** va **Vanilla CSS** yordamida 1:1 aniqlikda qayta yaratilgan talqini hisoblanadi. Barcha ma'lumotlar to‘liq **Mock JSON** ko‘rinishida tashkil etilgan.

## 🚀 Texnologiyalar

- **Framework**: Vue 3 (Composition API `<script setup>`)
- **Build Tool**: Vite
- **Styling**: Vanilla CSS (Gov.uz rasmiy ranglari va dizayn tizimi)
- **Icons**: SVG & Lucide Icons
- **Data**: Mock JSON (Real davlat portali API tuzilmasiga asoslangan)

## 📁 Loyiha tarkibi

- **TopBar**: Davlat ramzlari (Bayroq, Gerb, Madhiya), MB valyuta kurslari, ob-havo, til tanlash, zaif ko‘ruvchilar uchun maxsus imkoniyatlar (shrift masshtabi, rangsiz va kontrast rejimlar).
- **AppHeader**: Vazirlik gerbi va emblemasi, tezkor qidiruv (`Ctrl+K`), ishonch telefoni pulslari, 7 ta bosh kategoriya va 60+ ta ichki bo‘limli menyu, sayt xaritasi tortmasi.
- **InnerMarquee**: Yuguruvchi satr (Tezkor xabarlar / Ticker).
- **NewsHeroSection**: Katta bosh maqola, kategoriya filtrlari, ko‘rishlar soni va yangiliklar tarmog‘i.
- **BannersCarousel**: Rasmiy aksiyalar va madaniy dasturlar slayderi.
- **ActivitiesSection**: Faoliyat yo‘nalishlari kartochkalari (Teatr, Muzey, Musiqa, Meros va h.k.).
- **InteractiveServicesSection**: Interaktiv xizmatlar va madaniy tadbirlar taqvimi.
- **StatisticsSection**: Vazirlik statistik ko‘rsatkichlari (raqamlarda).
- **VacanciesDocPoll**: 3 ta tab: Bo‘sh ish o‘rinlari, Lex.uz normativ hujjatlari va interaktiv ovoz berish so‘rovnomasi.
- **ProjectsSection**: Davlat va xalqaro madaniy loyihalar.
- **RegionalMapSection**: O‘zbekistonning 14 ta hududiy boshqarmalari bo‘yicha interaktiv katalog (rahbar, manzil, telefon, elektron pochta).
- **GovPartnersSection**: Davlat organlari va soha portallari havolalari.
- **SubscribeBanner**: Yangiliklardan xabardor bo‘lish (Email obuna).
- **AppFooter**: To‘liq rasmiy footer, rekvizitlar, ijtimoiy tarmoqlar, xatoliklar haqida xabar berish (`Ctrl+Enter`).
- **Modallar**: Qidiruv (`Ctrl+K`), Sayt xaritasi, Maxsus imkoniyatlar, Davlat ramzlari va Matndagi xatolik xabari (`Ctrl+Enter`).

## 🛠 O‘rnatish va ishga tushirish

```bash
# Frontend papkasiga o‘tish
cd frontend

# Bog‘liqliklarni o‘rnatish
npm install

# Mahalliy serverni ishga tushirish
npm run dev

# Ishlab chiqarish uchun build qilish
npm run build
```
