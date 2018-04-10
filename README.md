# Ehliyet Ofisi Bulutfon PHP SDK

EOB'ye erişmek için Curl ile istekde bulunmanız yeterlidir.

* [Dokümantasyon]()
* [Örnek Uygulama]()

## Kullanım

Bulutfon sistemi üzerinden aldığınız master token id ile işlemlerinizi. Dökümantasyona göre yapabilirsiniz.

## İşlem Tipleri - Type Code 
İşlem tipini belirtir. Her işlemin farklı bir kodu bulunmaktadır.

100 - Otomatik Arama Başlatır - Amazon Polly
101 - Otomatik Arama Detay Raporu
102 - Tüm Arama Kayıtlarının Listelenmesi
103 - Ses Kayıt Dinleme
104 - Dahili Lisleme



### 100 - Otomatik Arama Başlatır - Amazon Polly

Otomatik arama başlatmak için aşağıdaki endpoint gerekli alanları doldurarak post edilmesi yeterlidir. "token" değerini hizmet aldığınız firmadan alabilirsiniz. "token" değerini base64encode olarak şifrelenmiş bir şekilde post etmeniz gerekmektedir.

```php
$GenerateNumber = rand(111111111111111,999999999999999); // Sistem tarafından üretilen rasgele numara.
$data = array(
"type" => "100", // Type işlem tipi 100 - Otomatik Aramayı Sentezleme Yaparak Başlatır.
"token" => "</TokenCode>", //Token kodu bulutfon sistemlerinden temin edilmeli. Base64 Encode edilerek gönderilmeli.
"title" => $GenerateNumber, //Aramaya verilen isim
"lang" => "Filiz", //AWS Services sistemlerinde bulunan dil bilgisi
"tts" => "OkunacakMetin", // Text To Spech olarak hangi metni seslendirmek isteniyorsa - Amazon Web Services
"did" => "DidNumber", //Bulutfon sistemlerinde onaylı telefon numarası
"receivers" => "PhoneNumbers", //Telefon numaraları 90 şeklinde ve aralarına , işareti şeklinde
"sound_url" => "http://SERVER_ADRES/tmp/mp3/".md5($GenerateNumber).".mp3", //Dışardan çağırılan ses dosyası
"announcement_id" => "", //Bulutfon sisteminde ses dosyasının id numarası
"gather" => "true"
);   
$data_string = json_encode($data);
$ch = curl_init("SERVER_ADRES");
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string))
);       
$result = curl_exec($ch);
echo $result;

```

### 101 - Otomatik Arama Detay Raporu

Bu ekranda gönderilmesi gereken değerler. type, token ve id değerleridir. 


```php
$data = array(
"type" => "101", //Type
"token" => "</TokenCode>", //Token Base64 Encode
"id" => </IDdNumber>, // Listeme Ekranında Görünecek Otomatik Arama İsmi

);   
$data_string = json_encode($data);
$ch = curl_init("http://SERVER_IP_ADRES/jsonData.php");
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string))
);       
$result = curl_exec($ch);
echo $result;
```


### 105 - Otomatik Arama Kayıtlarının Listelenmesi 

Bu istek türünde sistem tarafından yapılmış tüm otomatik arama kayıtlarına erişebilirsiniz. Kayıtların listenmesi için "type" ve "token " değerlerinin gönderilmesi zorunludur. 

```php
$data = array(
"type" => "105",  // İşlem Tipi
"token" => "</TokenCode>" ,
);   
$data_string = json_encode($data);
$ch = curl_init("http://SERVER_IP_ADRES/jsonData.php");
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string))
);       
$result = curl_exec($ch);
echo $result;
```


