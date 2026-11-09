# Real SQL: Vakansiyadan Bazar Trendinə

## İştirakçı Kitabçası

Bu sənəd workshop kitabçasının Markdown variantıdır (orijinal Word faylı da repoda var).

## Giriş

Bu workshop-da iştirakçılar Kursor Education-ın apardığı əmək bazarı araşdırmasının analitikini canlandırırlar: boss.az və glorri.az saytlarından toplanmış real 2026-cı il vakansiya elanları Oracle-a idxal edilib, məqsəd bazarın strukturunu (sektor bölgüsü, maaş səviyyələri, tələb olunan bacarıqlar, şəhər üzrə sıxlıq) SQL vasitəsilə açmaqdır.

**Format:** real biznes ssenarisi təqdim olunur, texniki tələb izah olunur, iştirakçı özü SQL yazır.

## Verilənlər bazası sxemi

Sxem workshop_user altında hazır olacaq, iki cədvəl:

**COMPANIES**

| Sütun | Tip | Qeyd |
| --- | --- | --- |
| company_id | NUMBER | PK |
| company_name | VARCHAR2(200) | Şirkət adı |
| source_site | VARCHAR2(10) | 'BOSS' / 'GLORRI' |

**VACANCIES**

| Sütun | Tip | Qeyd |
| --- | --- | --- |
| vacancy_id | NUMBER | PK |
| company_id | NUMBER | FK -> companies |
| source_site | VARCHAR2(10) | 'BOSS' / 'GLORRI' |
| job_title | VARCHAR2(300) | Vəzifə adı |
| sector | VARCHAR2(150) | Sahə (məs. İnformasiya texnologiyaları) |
| profession | VARCHAR2(200) | Peşə/alt-kateqoriya (yalnız BOSS-da dolu) |
| city | VARCHAR2(100) | Şəhər |
| employment_type | VARCHAR2(50) | Tam ştat / Yarım ştat və s. |
| experience_level | VARCHAR2(100) | Təcrübə aralığı (mətn) |
| education_level | VARCHAR2(50) | Yalnız BOSS-da dolu |
| salary_min / salary_max | NUMBER(10,2) | Yalnız salary_disclosed='Y' olanda dolu |
| salary_disclosed | CHAR(1) | 'Y' (BOSS) / 'N' (GLORRI) |
| posted_date | DATE | Elanın dərc tarixi |
| deadline_date | DATE | Yalnız GLORRI-də dolu |
| view_count | NUMBER | Baxış sayı |
| requirements_text | CLOB | Tələblər bölməsinin sərbəst mətni |

Bu iki cədvəl arasında JOIN, GROUP BY, analitik funksiyalar və REGEXP əsaslı mətn axtarışı tapşırıqlar boyu istifadə olunacaq.

## Dataset haqqında

Dataset real mənbələrdən götürülüb.

Bu fərq salary_disclosed bayrağında saxlanılır, dataset-in özü artıq "maaşlı vs. maaşsız" müqayisəsi üçün nəzərdə tutulub.

Elanlar 2026-cı ilə aiddir.

## Blok 1: İlkin kəşfiyyat

**Tapşırıq 1.** HR komandası bazarın strukturunu görmək istəyir: bazarda mövcud olan bütün fərqli sektor və peşələrin siyahısını tərtib edin.

- Texniki tələb: DISTINCT, əlifba sırası ilə ORDER BY

**Tapşırıq 2.** Regional işə cəlb siyasəti hazırlanır: Bakıdan kənar şəhərlərdəki ən yeni vakansiyaları tapın.

- Texniki tələb: WHERE city != 'Bakı', ORDER BY posted_date DESC

## Blok 2: Aqreqasiya

**Tapşırıq 3.** Hansı sektor ən çox vakansiya açır? Nəticəni azalan sıra ilə göstərin.

- Texniki tələb: GROUP BY sector, COUNT(*), ORDER BY ... DESC

**Tapşırıq 4.** Maaşı açıqlanan elanlarda sektor üzrə orta/minimum/maksimum maaşı hesablayın. Statistik cəhətdən ətəbarlı olsun deye, 3-dən az elanı olan sektorlar hesabata daxil olmasın.

**Tapşırıq 5.** Ən aktiv işəgötürən 10 şirkəti tapın (ən çox elan yerləşdirənlər).

- Texniki tələb: companies və vacancies arasında JOIN, GROUP BY company_name, COUNT(*), FETCH FIRST 10 ROWS ONLY

## Blok 3: Analitik Funksiyalar (əsas blok)

**Tapşırıq 6.** Hər sektorda ən yüksək maaşlı 3 vakansiyanı tapın (sektor rəhbərliyinə hesabat üçün).

- Texniki tələb: RANK() OVER (PARTITION BY sector ORDER BY salary_max DESC)

**Tapşırıq 7.** Hər vakansiyanın öz sektorunun orta maaşından nə qədər yüksək/aşağı olduğunu göstərin.

- Texniki tələb: AVG(salary_max) OVER (PARTITION BY sector), fərq sütunu

**Tapşırıq 8.** Şəhərləri vakansiya sıxlığına görə reytinqləyin.

- Texniki tələb: DENSE_RANK() OVER (ORDER BY COUNT(*) DESC)

**Tapşırıq 9.** Vakansiyaları maaş səviyyəsinə görə 4 bərabər qrupa (segment) bölün: "aparıcı seqment" hansı sektorlardan ibarətdir?

- Texniki tələb: NTILE(4) OVER (ORDER BY salary_max)

## Blok 4: Mətn analitikası (REGEXP)

**Tapşırıq 10.** Tələblər bölməsində "Oracle" və ya "SQL" sözü keçən vakansiyaların sayını və bazardakı faizini hesablayın.

- Texniki tələb: REGEXP_LIKE(requirements_text, 'Oracle|SQL', 'i'), ROUND

**Tapşırıq 11.** Ən çox tələb olunan 5 texniki bacarığı (Python, SQL, Excel, Linux, Rus dili və s.) tapın: hər biri üçün neçə vakansiyada keçdiyini göstərin.

- Texniki tələb: hər bacarıq üçün REGEXP_LIKE, sonra UNION ALL ilə bir cədvəldə birləşdirin

## Blok 5: Maaş şəffaflığı

**Tapşırıq 12.** Maaşı açıqlanan və açıqlanmayan elanların sektor bölgüsü fərqlənirmi?

**Tapşırıq 13.** Son müraciət tarixinə 7 gündən az qalan "təcili" vakansiyaları etiketləyin (yalnız deadline_date dolu olan GLORRI elanlarında).

## Blok 6: Yekun hesabat

**Tapşırıq 14.** Hər şirkətin açdığı bütün vəzifələri vergülla ayrılmış tek sətrdə təqdim edin (HR hesabat formatı) salary_disclosed='Y' olanları.

**Tapşırıq 15.** Bazar ortalamasından yüksək maaş təklif edən vakansiyaları tapın.

- Texniki tələb: alt-sorgu/CTE ilə ümumi AVG(salary_max) hesablayın, sonra WHERE salary_max > (...) ilə müqayisə edin
