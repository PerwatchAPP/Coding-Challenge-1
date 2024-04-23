
# Dinamik Stok Yönetim Sistemi
### Detaylara https://docs.google.com/document/d/1Mo0WAr6lbr67JPe43JEKbsdf36Q8Phih9_2DyCgt3sE/edit?usp=sharing adresinden ulaşabilirsiniz.
Bu sistemde sizden istenilenler sırasıyla şu şekildedir:

(bütün apiler `/api/v1` ile başlayacaktır.)

(Yanında 🔒 emojisi olan apiler jwt tokeni gerektirmektedir).

## /register [POST]

Kullanıcı oluşumu için beklenen bilgilerin girilerek kayıt edileceği adrestir. Kullanıcıdan beklenen girdiler ise sırasıyla aşağıda yer almaktadır:

- Ad: Ad soyadıyla birlikte eşsiz olmalıdır
- Soyad: Soyadı ad ile birlikte eşsiz olmalıdır.
- Kullanıcı adı: Kullanıcı adı eşsiz olacaktır
- Şifre: en az 6 karakter maks 16 karakter olması gerekemekle beraber aynı zamanda veritabanında şifrelenmiş bir şekilde tutulmalıdır.

## /login [POST]

Bu API kullanıcıdan kullanıcı adı ve şifre alacaktır, dönüt olarak ise bir adet jwt token dönmesi beklenmektedir.

## /account [GET] 🔒

Burada ise şifre bilgisi hariç kullanıcının bilgileri dönülecektir.

## /form/create [POST] 🔒

Burada kullanıcıdan form oluşturulması istenilecektir. Bu form dinamik oluşturulacak stoklar için kullanıcının kendi stok girdi formlarını oluşturmalarına olanak sağlayacaktır (örn: Google Form). Burada gerekli olarak istenilen girdiler ise şu şekildedir:

- Form ismi: Kullanıcı bazında eşsiz olması beklenmektedir.

## /form [GET] 🔒

Giriş yapmış kullanıcıya ait formların listesini dönmesi beklenmektedir. Buradaki gereksinim ise kesinlikle sayfalama kullanılmasıdır.

## /form/:_id [GET] 🔒

Veritabanı id’si iletilen form ait bilgiler dönülecektir. Ancak burada sadece id’si verilen forma sahip (oluşturmuş) olan kullanıcı bu forma ait bilgilere erişebilecektir.

## /form/:_id [DELETE, PUT] 🔒

Buradaki iki method içinde gerekli işlemler gerçekleştirilecektir. Forma ait herşey düzenlenebilir olmakla beraber aynı zamanda her form silinebilmektedir. (soft veya hard delete sizin terchinize kalmıştır)

## /form/:_id/field [POST] 🔒

Bu API ile ilgili forma alanlar eklenecektir. Burada eklenebilir alan türleri şu şekilde olacaktır:

- Combobox
- Text
- Checkbox
- Number
- NumberDecimal

## /form/:_id/field [GET] 🔒

Forma ait tüm alanları dönecektir. Kesinlikle alan sırasına göre sıralanmış olarak bütün alanlar dönülecektir.

## /form/:_id/field/:field_id 🔒[GET, DELETE, PUT]

Field_id’si verilen alanla alakalı CRUD işlemleri gerçekleştirilecektir.

- GET: ilgili alan bilgileri getirilecektir.
- DELETE: ilgili alan form’dan kaldırılacaktır. (kendisinin silinip silinmeyeceği izlediğiniz stratejiye ve size bağlıdır)
- PUT: alanla ilgili bilgilerin düzenlemesi sağlanacaktır. Alan oluşturma geçerli olan koşullar aynen geçerlidir.


## /form/:_id/stock [POST] 🔒

Burada kullanıcıya ait stok listesine iletilen form üzerinden doğrulanarak stok girilecektir. Örneğin:

Eğer kullanıcı şu alanlarla bir form oluşturmuş ise;
- Ürün Adı
- Alan Adı : Ürün Adı
- Türü : Text
- Min : 10
- Max : 120
ve var ise diğer opsiyonel alanlar
- Stok Sayısı
- Alan Adı : Stok Sayısı
- Türü : Number
- Min : 0
- Max : -1 (eğer maks -1 olarak belirlendi ise alan oluşturulurken burada herhangi bir limit olmadığı anlamına gelmektedir.)
ve var ise diğer opsiyonel alanlar
- Ürün Fiyatı
- Alan Adı : Ürün Fiyatı
- Türü : NumberDecimal
- Min : 0
- Max : -1
- Para Birimi
- Alan Adı : Para Birimi
- Türü : Combobox
  - Seçenekler:
    - USD
    - TRY
    - EUR
    - GBP
  - Default Seçenek : TRY
- Ürün Satışta
- Alan Adı : Ürün Satışta
- Türü : Checkbox

O zaman post verisi şu şekilde olmalıdır:

  
    {
      "fields": [
        {
          "name": "Ürün Adı",
          "value": "500 ml su"
        },
        {
          "name": "Stok Sayısı",
          "value": 50
        },
        {
          "name": "Ürün Fiyatı",
          "value": 4
        },
        {
          "name": "Para Birimi",
          "value": "TRY"
        },
        {
          "name": "Ürün Satışta",
          "value": true
        }
      ]
    }
## /form/:_id/stock [GET] 🔒

Kullanıcının belirli bir forma ait stokları getirecektir.

## /form/:_id/stock/:stock_id [GET, DELETE, PUT] 🔒

Stock_id’si verilen stokla ilgili CRUD işlemleri gerçekleştirilecektir.

- GET: ilgili stok bilgileri getirilecektir.
- DELETE: ilgili stok form’dan kaldırılacaktır.
- PUT: stokla ilgili bilgilerin düzenlemesi sağlanacaktır.
# Zorunlu Gereksinimler
- Veritabanı olarak MongoDB kullanılmalıdır.
- Değişken isimleri ve var ise yorumlar tamamen ingilizce olarak yazılmalıdır
- Kod stili olarak camelCase kullanılmalıdır
- JWT için redis kullanmadan doğrulama yapılması ve şifre hariç kullanıcı bilgilerinin JWT datasından alınabilmeli
- Zorunlu kütüphaneler hariç projenin temeli niteliğindeki özelliklerini yerine getirmek amacıyla üçüncü parti kütüphaneler kullanılmamalıdır. (Kullanılması eksi puan olarak sayılacaktır). İstisnalar REST API için fiber ve fiber eklentilerinin kullanımı üçüncü parti olarak sayılmayacaktır
