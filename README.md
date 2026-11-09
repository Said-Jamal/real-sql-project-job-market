# real-sql-project-job-market
# Real SQL: Vakansiyadan Bazar Trendinə

Kursor Education-ın keçirdiyi "Real SQL" workshop-unda həll etdiyim tapşırıqlar. Məqsəd Azərbaycan əmək bazarının strukturunu (sektor bölgüsü, maaş səviyyəsi, tələb olunan bacarıqlar, şəhər üzrə sıxlıq) SQL ilə açmaqdır.

## Dataset

Real 2026-cı il vakansiya elanları, Oracle bazasına idxal edilib. İki cədvəl var:

- `COMPANIES`: 1409 şirkət
- `VACANCIES`: 4419 vakansiya (vəzifə, sektor, şəhər, maaş, tarix, tələblər mətni və s.)

Dataset və tapşırıqlar workshop materiallarıdır, paylaşmaq üçün icazə alınıb.

## Repo strukturu

```
data/        COMPANIES_export.csv, VACANCIES_export.csv
questions/   Workshop kitabçası (15 tapşırıq)
solutions/   Kursor_HR_SQL_Workshop_2.sql (mənim həllərim)
```

## İstifadə olunan mövzular

DISTINCT, GROUP BY / HAVING, JOIN, analitik funksiyalar (RANK, DENSE_RANK, NTILE, AVG OVER), CTE, REGEXP_LIKE, UNION ALL. Alət: Oracle SQL Developer.

## Status

- Tapşırıq 1-11: tamamdır
- Tapşırıq 12-15: üzərində işləyirəm, hazır olduqca əlavə olunacaq

## Datada nəzərə alınası məqamlar

- Dataset-də kitabçada göstərilən BOSS və GLORRI saytlarından başqa JOBSEARCH, ABB və KAPITAL mənbələri də var.
- JOBSEARCH elanlarında şəhər sütunu boşdur, buna görə şəhər üzrə analizdə NULL dəyərlər xaric edilir (Tapşırıq 8).
- Maaşlarda kənar dəyərlər var (məsələn, maksimum 40000, median isə 800). Ortalama hesablayan sorğuların nəticəsi buna həssasdır.
