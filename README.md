# Real SQL: Vakansiyadan Bazar Trendinə

Kursor Education-ın keçirdiyi "Real SQL" workshop-unda həll etdiyim tapşırıqlar. Məqsəd Azərbaycan əmək bazarının strukturunu (sektor bölgüsü, maaş səviyyəsi, tələb olunan bacarıqlar, şəhər üzrə sıxlıq) SQL ilə açmaqdır.

## Fayllar

| Fayl | Nədir |
| --- | --- |
| [Kursor_HR_SQL_Workshop_2.txt](Kursor_HR_SQL_Workshop_2.txt) | Tapşırıqlar üzrə mənim SQL həllərim |
| [Tapshiriqlar.md](Tapshiriqlar.md) | Workshop kitabçası: sxem və 15 tapşırıq (burada oxumaq üçün) |
| Real SQL ... İştirakçı Kitabçası (.docx) | Eyni kitabçanın orijinal Word variantı (yükləmək üçün) |
| [COMPANIES_export.csv](COMPANIES_export.csv) | Şirkətlər cədvəli, 1409 sətir |
| [VACANCIES_export.csv](VACANCIES_export.csv) | Vakansiyalar cədvəli, 4419 sətir (böyük fayldır, GitHub səhifədə göstərmir, yükləmək lazımdır) |

## Dataset

Real 2026-cı il vakansiya elanları, Oracle bazasına idxal edilib. İki cədvəl var:

- `COMPANIES`: şirkət adı və mənbə sayt
- `VACANCIES`: vəzifə, sektor, şəhər, maaş, tarix, tələblər mətni və s.

Dataset və tapşırıqlar workshop materiallarıdır, paylaşmaq üçün icazə alınıb.

## İstifadə olunan mövzular

DISTINCT, GROUP BY / HAVING, JOIN, analitik funksiyalar (RANK, DENSE_RANK, NTILE, AVG OVER), CTE, REGEXP_LIKE, UNION ALL. Alət: Oracle SQL Developer.

## Status

- Tapşırıq 1-11: tamamdır
- Tapşırıq 12-15: üzərində işləyirəm, hazır olduqca əlavə olunacaq

## Datada nəzərə alınası məqamlar

- Dataset-də kitabçada göstərilən BOSS və GLORRI saytlarından başqa JOBSEARCH, ABB və KAPITAL mənbələri də var.
- JOBSEARCH elanlarında şəhər sütunu boşdur, buna görə şəhər üzrə analizdə NULL dəyərlər xaric edilir (Tapşırıq 8).
- Maaşlarda kənar dəyərlər var (məsələn, maksimum 40000, median isə 800). Ortalama hesablayan sorğuların nəticəsi buna həssasdır.
