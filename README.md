# DevJoint Internship — Week 1: Data Cleaning & Exploratory Analysis

Online Retail dataset (UK-based e-commerce, ~525,000 sətir) üzərində 
tam data cleaning və exploratory data analysis (EDA) layihəsi.

## Dataset
## Dataset mənbəyi
Dataset Kaggle-dan əldə edilib: 
https://www.kaggle.com/datasets/lakshmi25npathi/online-retail-dataset
UCI Online Retail — 01/12/2010 - 09/12/2011 tarixləri arası UK-based 
qeyri-mağaza onlayn pərakəndə satıcının bütün əməliyyatları.
Sütunlar: Invoice, StockCode, Description, Quantity, InvoiceDate, 
Price, Customer ID, Country

## Layihənin addımları

### 1. Data Loading & Preview
Dataset yükləndi, strukturu (shape, dtypes, null values, duplicates) 
yoxlanıldı. 525,461 sətir, 8 sütun. Description-da 2928, Customer ID-də 
107,927 boş dəyər, 6865 tam təkrarlanan sətir aşkarlandı.

### 2. Handling Null Values & Duplicates
- Description: StockCode-a görə qruplaşdırılıb mövcud adlarla dolduruldu, 
  qalanlar "Unknown Product" ilə işarələndi.
- Customer ID: Boşluqların yalnız 0.3%-i cancellation-la bağlı idi, 
  əksəriyyəti guest checkout idi — silinmədi, "Unknown" ilə dolduruldu, 
  Is_Guest_Purchase flag sütunu yaradıldı.
- 6865 tam təkrarlanan sətir silindi.

### 3. Outlier Detection & Processing
IQR metodu seçildi (z-score əvəzinə, çünki paylanma normal deyil). 
3 "Adjust bad debt" sətri (real satış olmadığı üçün) silindi. 
Quantity və Price-da tapılan outlier-lər (11.13% və 6.63%) silinmədi 
— bulk sifarişlər real biznes fəaliyyəti olduğu üçün, əvəzinə 
Is_Quantity_Outlier və Is_Price_Outlier flag sütunları yaradıldı.

### 4. Feature Engineering
Yeni sütunlar: TotalPrice (Quantity × Price), Purchase_Hour, 
Purchase_DayOfWeek, Is_Guest_Purchase, Is_Cancelled, 
Is_Quantity_Outlier, Is_Price_Outlier.

### 5. Visualization
- Correlation heatmap (numeric sütunlar arası)
- Distribution (Quantity histogram)
- Categorical breakdown (Is_Cancelled pie chart, Country bar chart)
- Əlavə biznes analizi: Pareto (müştəri gəlir konsentrasiyası), 
  saat/gün üzrə satış heatmap, top məhsullar (satış vs gəlir), 
  ölkəyə görə return nisbəti

### 6. Written Conclusion
5 əsas insight çıxarıldı — müştəri konsentrasiyası, B2B satış modeli, 
məhsul portfeli strategiyası, data quality məsələləri, Yaponiya 
bazarının yüksək return nisbəti.

## Əsas tapıntılar

**1. Müştəri gəliri kəskin təbəqələşib.** Pareto analizi göstərdi ki, 
müştərilərin ilk 25-27%-i, ümumi gəlirin 80%-ni yaradır. Bu, şirkətin 
gəlirinin əksəriyyətinin kiçik, sadiq bir müştəri qrupundan gəldiyini 
göstərir — biznes əsasən "az sayda böyük alıcı" üzərində qurulub, 
kütləvi təsadüfi müştəri bazası üzərində deyil. Bu, marketinq və 
retention resurslarının bu əsas seqmentə yönləndirilməsinin daha 
səmərəli olacağını göstərir.

**2. Bu, B2B/topdansatış xarakterli bir biznesdir, adi pərakəndə deyil.** 
Satışların demək olar hamısı iş saatlarında (9:00-16:00) baş verir, 
gecə satış praktik olaraq yoxdur. Şənbə günü satış həcmi digər 
günlərdən 100 dəfədən çox aşağıdır. Bu pattern, son istehlakçıların 
deyil, ofis vaxtında sifariş verən topdansatış müştərilərinin əsas 
alıcı bazası olduğunu təsdiqləyir — B2C onlayn mağazalarda adətən 
axşam/həftəsonu saatlarında zirvə müşahidə olunur, bu isə tam əksinədir.

**3. Məhsul portfeli iki fərqli qazanc strategiyasına bölünür.** 
"Regency Cakestand 3 Tier" az satılsa da (top-10 satış siyahısında 
yoxdur) baha qiyməti sayəsində ən çox gəlir gətirən məhsuldur. 
"World War 2 Gliders" isə əksinə — ucuz qiymətinə baxmayaraq, yüksək 
satış həcmi ilə gəlirə böyük töhfə verir. Deməli, şirkətin gəlir 
strukturu tək bir modelə deyil, həm "az sat-baha qazan", həm "çox 
sat-az qazan" strategiyalarına əsaslanır — hər ikisi eyni dərəcədə 
vacibdir və ayrı-ayrı idarə olunmalıdır.

**4. Gəlir hesablamaları çatdırılma xərclərini məhsul satışı ilə 
qarışdırır.** "DOTCOM POSTAGE" sətri, real bir məhsul olmadığı halda, 
top-10 gəlir mənbəyi arasına düşüb. Bu o deməkdir ki, hazırkı "ən çox 
gəlir gətirən məhsullar" reytinqi qismən yanlışdır — real məhsul 
performansını düzgün ölçmək üçün belə xidmət/logistika sətirləri 
ayrıca filtrlənib çıxarılmalıdır. Bu, gələcək analizlər üçün vacib bir 
data quality xəbərdarlığıdır.

**5. Yaponiya bazarı digərlərindən fərqli, problemli bir seqmentdir.** 
Yaponiyada return nisbəti ~27%-dir, halbuki digər bütün ölkələr 2-6% 
aralığındadır. Bu, təsadüfi statistik fərq deyil — bu qədər böyük fərq, 
Yaponiya bazarında ya məhsul-bazar uyğunsuzluğu (məs ölçü, dizayn 
zövqü fərqi), ya da çatdırılma/keyfiyyət problemi olduğunu göstərən 
aydın bir siqnaldır və ayrıca araşdırma tələb edir.


## Alətlər
Python, pandas, numpy, matplotlib, seaborn
