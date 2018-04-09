# Ehliyet Ofisi Bulutfon PHP SDK

EOB'ye erişmek için Curl ile istekde bulunmanız yeterlidir.

* [Dokümantasyon]()
* [Örnek Uygulama]()

## Kullanım

Bulutfon sistemi üzerinden aldığınız master token id ile işlemlerinizi. Dökümantasyona göre yapabilirsiniz.
^
### Otomatik Arama Başlatmak 

Sistemin Amazon servislerini kullanarak bulutfon sistemine otomatik çağrı isteğinde bulunmasını istiyorsanız. Aşağıdaki servisi kullanabilirisiniz.

```php
$GenerateNumber = rand(111111111111111,999999999999999); // Sistem tarafından üretilen rasgele numara.
$data = array(
"type" => "100", // Type işlem tipi 100 - Otomatik Aramayı Sentezleme Yaparak Başlatır.
"token" => "TokenCode", //Token kodu bulutfon sistemlerinden temin edilmeli. Base64 Encode edilerek gönderilmeli.
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


