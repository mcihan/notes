############################
# NOTES 1 FILE
############################

# INDEX

- NOTES 1 FILE
  - JAVASCRIPT
  - WEB
  - NETWORK
  - PATTERN
  - DATABASE
  - SECURITY
  - HARDWARE
  
- NOTES 2 FILE
  - JAVA
  - UNIX-LIKES

- NOTES 3 FILE
  - OTHERS: diğer başlıklara eklenemeyenler

- NOTES 4 FILE
  - MOBIL
  - GIT
  - MATH

############################

############################
# JAVASCRIPT
############################

############################

Web tarayıcılarında uzun zamandır detskelenen yeni API. Promise tarzı çalışması en büyük avantajıdır. örnek kullanım:

```js
fetch('send-ajax-data.php')

    .then(data => console.log(data))

    .catch(error => console.log('Error:' + error));
```

ES7 async/await example:

```js
async function doAjax() {
    try {
        const res = await fetch('send-ajax-data.php');
        const data = await res.text();
        console.log(data);
    } catch (error) {
        console.log('Error:' + error);
    }
}

doAjax();
```

Fect API spesifikasyonu burada: https://fetch.spec.whatwg.org/ (archive date: 27/11/2019)

Eski tarayıcılar için core Javascript polyfill kütüphanesi: https://github.com/github/fetch (whatwg-fetch isimli npm paketi). Fetch polyfill kendi içinde var XMLHttpRequest()'i wrap eder. Kaynak: https://github.com/github/fetch/blob/master/fetch.js (archive date: 27/11/2019)

Fetch, jQuery.ajax()’den bazı farklılıklar var. bu linkte farklılıklar yazmaktadır: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch (archive date: 27/11/2019)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# immutable.js

```js
const { Map } = require('immutable')

const map1 = Map({ a: 1, b: 2, c: 3 })

const map2 = map1.set('b', 50)

map1.get('b') + " vs. " + map2.get('b') // 2 vs. 50
```

Yukarıdaki Map sınıfı, js'in default bir sınıfı diil. tamamen immutable'a özel bir sınıftır. yani aşağıdaki gibi bir obje diildir:

```js
var myArr = { a: 1, b: 2, c: 3 };
```

# immer.js

```js
import produce from "immer"

const baseState = [
    {
        todo: "Learn typescript",
        done: true
    },
    {
        todo: "Try immer",
        done: false
    }
]

const nextState = produce(baseState, draftState => {
    draftState.push({todo: "Tweet about it"})
    draftState[1].done = true
})
```

at the end baseState will be untouched. dikkat edilirse; immutable.js'teki gibi native olmayan JS class'ı kullanılmamış. sadece draftState native olmayan bir JS class'ıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# vanilla-js
vanilla js bir kütüphane/framework gibi geçiyor fakat içi boş(kod olmayan) bir js projesidir. vanilla-js, saf js kodu ile yazılmış projelere takma isim olarak kullanılmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Polyfill
bu terimin türkçesi yoktur.

sadece web development'ında kullanılan bir terimdir. eğer bir kod parçası, web tarayıcının desteklemediği bir özelliği çalıştırabilmemizi sağlıyorsa, o kod parçasına Polyfill denir.

# Shim
tüm programlama dünyasında kullanılan bir terimdir. eğer bir kod parçası, bizim yazdığımız API metodunun önünü intercept edip başka bir metoda yönlendiriyorsa, o kod parçasına shim denir. shim'ler genelde geriye uyumluluk için kullanılırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cross-env
https://www.npmjs.com/package/cross-env sitesinde dağıtılan node modülüdür. bu modül ile npm scriptlerinde çağrılan komutlara environment variable'ları geçebilmemizi sağlamaktadır. bu özellik normalde de npm tarafından desteklenir, fakat cross-env, bunu OS'tan bağımsız bir syntax ile yapabilmemizi sağlar.

örnek usage:

```json
// package.json file
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
}
```

cross-env isimli node modül 2 adet komut export eder: cross-env ve cross-env-shell. cross-env komuta parametre geçilen komut variable'ı okuyabilir. fakat ikinci komut bu variable'ı okuyamaz. iki veya daha fazla komut üstüste çağrılacak ise cross-env-shell kullanılması şarttır. cross-env-shell için örnek:

```json
{
  "scripts": {
    "greet": "cross-env-shell XXX=111 YYY=222 \"echo $XXX && echo $YYY\""
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# flow
flow.org sitesinde dağıtılan js için type desteği getiren bir typechecker'dır.

örnek JS kodu:

```js
let num: number = 1 + 2;

//fonksiyon örneği:
function square(n: number): number {
  return n * n;
}

square("2"); // hem derleyici hata verir, hemde IDE olan flow eklentimiz (yada lint içindeki flow eklentimiz) hata verir.

// sol tarafta bir nesneye referansı atanmadan yazılan örnekler:
(1234: number);
("hi": string);
(true: boolean);
([1, 2]: Array<number>);
({ prop: "value" }: Object);
(function method() {}: Function);

```

Flow tipleri JS syntax'ında yoktur. bu sebeple derleyicimizde de ayar yapmamız gerekecektir.

- configs:
  - JS dosyanın flow'a göre algılanması için "// @flow" satırının, JS dosyanın bir yerinde olması gerekmektedir.
  - projenin root'unda .flowconfig dosyası olmalıdır. bu dosya için ignore/include edilecek dizinler gibi ayarlar bulunuyor

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Webpack
birçok js, css, sass, html gibi birçok web teknolojisi dosyasını birleştirmek için kullanılan komut satırı uygulaması, node module uygulamasıdır.

projenin root'undaki webpack.config.js dosyası örneği:

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins

module.exports = {
  // entry point
  // bu dosyadaki tüm import'lar birleştirilip tek bir dosya haline getirilecek.
  // default değeri: ./src/index.js
  entry: './path/to/my/entry/file.js'

  output: {
    path: path.resolve(__dirname, 'dist'), // bu sadece current dizini almak icin kullanılan bir fonksiyon. "current-dizin/dist" dizinin altında file bundle file oluşturulacak.
    filename: 'my-first-webpack.bundle.js'
  }

  // loaders
  // webpack json ve js haricinde hiçbir formattan anlamaz.
  // diğer tipteki tüm dosyaları ancak loader'lar aracılığı ile import edebiliriz.
  // asagidaki örnek: txt dosyası require() yada import edilecek ise "raw-loader" id'li loader ile import edilmeli anlamına gelmektedir.
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }

  // plugins
  // pluginlerin loader'lardan daha fazla yetenekleri vardır.
  // asagidaki örnek plugin, oluşturulan bundle js dosyasına link eden bir html dosyası oluşturuyor.
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};
```
# webpack-dev-server
webpack projesi altında olan fakat fakrlı bir node modülüdür ve ayrı repo'da geliştirilmektedir.

package.json dosyamıza nasıl run edeceğimizi girelim:

```json
"scripts": {
  "start-dev-server": "webpack-dev-server"
}
```

webpack.config.js dosyamıza ayarlarımızı ekleyelim:

```js
var path = require('path');

module.exports = {
  devServer: {
    contentBase: path.join(__dirname, 'dist'), // hangi dizinin sunucu tarafında dışarıya açılacağı. burada genelde target/dist/output isimlerini görebiliriz.
    compress: true, // kodun minify edilip edilmeyeceği
    port: 9000, // sunucunun dışarı açılacağı port
    after: function(app, server, compiler) {
      // do anything here...
      // this is event of server started.
    }
  }
};

```

Yukarıdakileri konfigleri dosyalara girmeseydik, sadece komut satırından da bu ayarları parametre geçip sunucyu başlatabilirdik:

```sh
node ./node_modules/webpack-dev-server/bin/webpack-dev-server.js --entry /entry/file --output-path /output/path
## yada
./node_modules/.bin/webpack-dev-server.cmd --entry /entry/file --output-path /output/path
```

# babel-loader
babel kodu convert ediyor. webpack, babel'in çevirdiği kodlardan bundle yaratması lazım. bu sebeple babel-loader paketinin node modüllerde olması ve config'lerden aktif edilmesi şarttır.

# hot module replacement (yada HMR)
webpack runtime'da "module" isminde bir static objeyi variable olarak inject ediyor. import etmedne bu objeyi kullanabiliyoruz. hot reload olduğunda, instance'larımızı yeniden yaratabileceğimiz callback metodlarımızı tanımlayabiliyoruz. örnek:

```js
var requestHandler = require("./handler.js");
var server = require("http").createServer();
server.on("request", requestHandler);
server.listen(8080);

// check if HMR is enabled
if(module.hot) {
	// handler.js dosyası (dependency) update olursa aşağıdaki fonksiyon çalışacak.
	module.hot.accept("./handler.js", function() {
		
    // requestHandler instance'ımızı tekrardan yaratıyoruz.
		server.removeListener("request", requestHandler);
		requestHandler = require("./handler.js");
		server.on("request", requestHandler);
	});
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# javascript performans avantajı nereden kaynaklı?

javascript tek thread çalışır. thread içerisinde çağrılan paralel gibi görünen threadler'in (setTimeout, ajax gibi) callback'leri aslında asıl thread bittikten sonra çalıştırılır. örneğin ajax işlemine callback parametresi geçtik. ajax işlemi browser'a verilir ve asıl thread bitmeden javascript o callback'i ajax sonuc dönmüşse dahi çalıştırmaz.

ecmascript thread mekanizmasında zorunlu standartlar getirmemiştir. dolayısı ile ecmascript'in javascript harici diğer implementasyonları tek thread olmak zorunda değildir.

thread kavramı process thread'i olarak algılanmamalıdır. javascript motoru multi-thread'dir. javascript'in çağırdığı ajax işlemi multithread olabilir. fakat javascript'in kod seviyesinde tek thread olarak işletilir.

# sunucudaki performans üstünlüğü

javascript tek thread olmasına rağmen daha hızlı çalışır. servletlere istek gelir. servlete gelen istek databaseye gider. database'den işlemin dönüş yapması 5 saniye olsun. bu 5 saniye boyunca 1 thread beklemede durmaktadır (blocking-IO). java'da database işlemini thread yapsak ve asagidaki gibi olsa main thread devam eder ve databse işlemi threadde çalışır.


```java
//class definition on server side
class ThreadX extends Thread {

     run(){
           aLongDatabaseOperation();
           System.out.println("3");
     }
}


//servlet code start

System.out.println("1");
t = new ThreadX();
t.run();
System.out.println("2");

//servlet end
```

yukarıdaki kod bloğunda hala javascript daha hızlı olacaktır. günümüzde 1 cpu max 8 cpu'lu olabiliyor. bu durumda; thread'in database işelmini beklemesi demek, cpu'yu da boşuna bekletmesi demek. aynı zamanda thread memory'si demektir. CPU aynı anda birçok işlem yapabilseydi javascript bu kadar performanslı olmayacaktı. js yukarıdaki gibi bir işlemde main thread'i bitiriyor. yani ekrana 1 ve 2 print ediliyor. eğer veritabanı işlemi bitmiş ise; js hemen ekrana 3 basıyor. fakat database işlemi bitmemişse; servlete gelen diğer isteklere cevap vermeye devam ediyor.

# performans comparison: single-thread non-blocking I/O vs multithread blocking-IO

yukarıdaki servlet örneği alsında bu karşılaştırmanın bir spesifik örneği oldu. sistemden sisteme bu kaşrılaştırmanın olumlu ve olumsuz yanları olabilir. fakat servlet'lerde sunucuya 1000 istek geldi diyelim. 10000 thread açılacak java'da. oysa javascriptte 1 thread olacak. javascript motoru tabi arkaplnada çok thread açıyor. fakat açılan thread'ler hiç blocklanmadıkları için hemen kapanıyorlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# moment.js

tarih/saat işlemlerini kolaylaştıran javascript kütüphanesi. bazı özellikler:

- tarihi fromatlayıp gösterme

- bir tarihi verip şu andan ne kadar eski olduğunu gösterme

- toplama çıkarma 

# moment-timezone.js

- timezone bilgilerini içeren moment eklentisi. bir datetime bilgisi, istenilen timezone'a çevrilebilmesi sağlanıyor.

- moment-timezone ile convert işlemi yapabilmek için bağımlılıklarımız şunlardır:

 - moment.js: core kütüphane

 - moment-timezone.js: timezonu çevirme api'si için

 - moment-timezone-with-data.js: convert işlemini yapan js içerisinde her ülkenin timezone bilgisi kodları ve saatleri ile birlikte yok. o yüzden bu dosyaya da ihtiyacımız var. dosyanın isminden de nalaşıldığı gibi bu dosyanın içinde zaten moment-timezone.js vardır.

# moment-timezone-with-data.js vs moment-timezone-with-data-2012-2022.js

moment-timezone-with-data.js tüm yılların bilgisini içerirken, 2012-2022.js sadece belli yılların bilgisini içeriyor. js dosyasının hafif kalabilmesi için böyle bir dosya daha oluşturulmuş. 2012-2022 yılları dışında işlem yapılacaksa 2012-2022.js dosyası yeterli değildir.



• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •



# numeral.js

sayısal işlemleri kolaylaştıran javascript kütüphanesi. bazı özellikler:

- istenilen formatta para bimiri, byte birimi formatlayarak gösterebilme

- 1inci, 2inci gibi çıktıları verebilme

- üs sayılarını formatlayabilme 1e+2 gibi

- basit matematik işlemleri yapabilme





• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •



# npm package versions



- version

Must match version exactly

- \>version

Must be greater than version

- \>=version

version'dan küçük sürümler

- ~version

"Approximately equivalent to version". See: "semver". example: ~1.2.3, will accept 1.2.5 but will miss 1.3.0.

- ^version

"Compatible with version". See semver. example: ^1.2.3, will accept 1.3.1 but will miss 2.0.0.

- 1.2.x

1.2.0, 1.2.1, etc., but not 1.3.0

- http://...

will use the remote folder as dependency

- #

Matches any version

- ""

(just an empty string) Same as *

- version1 - version2

\>=version1 <=version2.

- range1 || range2

Passes if either range1 or range2 are satisfied.

- git

Git URLs as Dependencies

- user/repo

GitHub URLs

- tag

A specific version tagged and published as tag. See "npm-dist-tag".

- path/path/path

use local folder for dependency.



# npm update

npm sürümleri bazen # gibi dinamik sürümler kullanabiliyor. bu sebeple bunların update kontrolü için bu komut çalıştırılmaktadır.



# "main" inside package.json
the file which is exported when the library is importing anywhere.

example: "main":"app.js"

# npm install
çalıtırıldığı dizindeki package.json dosyasını okur ve dependencies ve devDependencies içindeki tüm projeleri internetten çalıştırıldığı dizinin /node_modules dizininin altına yükler. devDependencies paketkelri build sırasında output/target paketi içerisinde bulundurulmayan paketleri içermektedir.

# npm install -g module1
-g parametresi module1'i global paket olarak sisteme ekliyor. yani artık sadece projenin bulunduğu dizinden değil tüm dizinlerden çağrılabilir durumda oluyor.

# npm list
çalıştığı dizinde node_modules altındaki modullerin listesini ağaç şeklinde listeler. -g parametresi ile çalıştırılırsa bulunduğu dizine bakmaz ve global kurulan node paketlerinin listesini ağaç şeklinde gösterir.

# directory structure
- NODE_INSTALLATION_PATH/bin -> path'e eklenmiş olması gereklidir. burada global kullanılan node paketlerinin executable doyalarının kısayolları mevcut. "npm" node ile çalışan bir modül olduğundan burada (path'te) her zaman npm kısayolu bulunur.  

- NODE_INSTALLATION_PATH/lib/node_modules/module1 --> global kurulan npm paketleri buradadır. module'in kendi dependency'leri "NODE_INSTALLATION_PATH/lib/node_modules/module1/node_modules" altındadır. npm projesi de buranın içindedir.

- NODE_INSTALLATION_PATH/include/node --> node'un kendi sistem dosyaları buradadır. node'un executable dosyasının kısayolu "bin" dizinindedir.

- NODE_INSTALLATION_PATH/share --> manual/help gibi dosyaları barındırır. 

- $HOME/.npm --> internetten indirilen tüm repository yedekleri buradadır. meven .m2 dosyası ile aynı mantıkta çalışmaktadır.

# npm link module1
bir uygulamamız ve bir de ondan bağımsız module1 isminde paket geliştiriliyor olalım. uygulamamız module1'e depend olsun. module1'de değişiklik olduğunda bu paketi repoya atıp, daha sonra uygulamamızdan npm install çalıştırmmaız gerekecek. bunu sürekli yapmamak için link komutu kullanılabilir. module1 dizini içerisinde "npm link" komutunu çalıştırırız. daha sonra uygulamamızın root dizinine gidip "npm link module1" komutunu çalıştırırız. artık bizim proje module1 dizinini direk görüyor olur. her değişiklik anında görülmüş olur. npm link çok basit bir taktikte kısayol atarak bu işi çözüyor.

npm link işleminin geri alınması için, modüle olan paketin dizinie gidip "npm unlink" ve daha sonra uygulamamızın dizinine gidip "npm unlink module" komutunu çalıştırmamı grekmektedir.

# scripts
komut listesi barındırıyor. isteğe bağlı komutlar ve default komutlar mevcut. örneğin:

```json
"scripts": {

  "start": "node node_modules/react-native/local-cli/cli.js start",

  "xxx": "node node_modules/react-native/local-cli/cli.js start"

 },
```

yazarsak; 

- xxx içineki komutu "npm run-script xxx" ile çalıştırabiliriz.

- start'a yazdığımız komutu "npm start" ile çağrabiliriz. "start" script'i, npm'in birçok standart komutlarından sadece bir tanesidir. stadart olduğu için "run-script" ile çağrılmamaktadır. custom script ismi (standart olmayan isim) kullandığımızda run-script ile çağırmamız şart.

"start" komutu; "prestart", "start" ve "poststart" komutlarını (eğer package.json'da yazılmış ise) sırası ile çalıştırmaktadır.

# package-lock.json

bu dosya package.json ile aynı dizinde otomatik npm tarafından oluşturulur. burada node_modules altındaki dizinlerin bir ağaç yapısı şeklinde metadataları ile bulundurur. bunun birkaç sebebi var: 

- npm tekrar install yaptığı zaman daha hızlı şekilde kontrol yapması sağlanır (tekrar tüm dizinler okunup metadalarına bakmaya gerek kalmaz.). 

- bu dosya source code repository'de history'si durur ve eski paketler history'den kontrol edilebilir.

- bir geliştirici yanlış paketleri kurmuş ve comit etmiş ise buradan farklı olan kısımlar incelenip neden faklı paket kurduğu incelenmeye başlanabilir.



# npm-shrinkwrap.json

manuel oluşturulan bir dosyadır. package-json ile aynı yerde olmalıdır. package.json içindeki depenedencies'lerin bazıları dinamik (1.0.* gibi) sürümlere sahiptir. dolayısı ile herkesin makinesinde  npm paketleri olabilir. işte bunun için npm-shrinkwrap.json dosyası çözüm olarak sunulmuştur. bu dosyada yazan paketler spesifik sürümlerde tutulmaktadır. böylece birileri bu dosyayı update etmediği sürece npm install komutu herkeste aynı sürümü kuracaktır. bu dosya yerne direk package.json içerisindeki sürümlerle de oynanabilir fakat bu sadece bir alternatiftir. örneğin momentjs paketini sürekli yükselmesinde sakınca görülmez fakat geliştirme yapılan süreçte bunu fix tutmak isteyebilirsiniz. bunu yorum satırı olarak yazacağımıza npm-shrinkwrap.json dosyasından yararlanırız ve package-jsonda monmentjs "latest" tag'i ile kalmaya devam edecektir.

"npm shrinkwrap" komutu ile hızlıca package-lock.json dosyasından  npm-shrinkwrap.json dosyası oluşturmaktadır.



# --no-shrinkwrap

npm install işleminin yanına bu parametre eklendiğinde install işlemini npm-shrinkwrap.json ve package-lock.json dosyalarını ignore ederek yapıyor.

# --save (yada -S)

npm install --save jquery

bu parametre npm 5 öncesi versiyonlarda package.json'a dependency'yi eklemek için yapılıyordu. npm 5 ile birlikte sadece install komutu zaten package.json'a ekleme yapıyor.

# npx
npm'in güncel sürümlerinde beraber yüklü gelen, npm'i wrap eden yeni bir komut satırı uygulaması, aynı zamanda node modülüdür. npx, npm'e göre daha az komut yazarak işlem yapabilmemizi sağlar. örneğin:

> npx install packageX

packageX'i kurduğu gibi packageX'i execute eder. genelde kurulan paket, kurulduktan sonra kullanıcı tarafından çağrıldığı için böyle hazır bir yola gidilmiş. 

bunun gibi birçok özelliği vardır.

# scoped packages
"@sample-scope/sample-package" şeklinde dağıtılan paketler scope'u açan kullanıcılar tarafından yönetiliyor. tamamen gruplama amaçlı yapılmış bir şeydir. herkes dilediği gibi istediği scope'a paket atamıyor. yetkilendirmeler söz konusu.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# nodejs

## express

nodejs için sunucu yazılmasını kolaylaştıran framework'tür, npm modülüdür. özellikle rest controller işini yapması ile ön plana çıkar.

## REPL

Read Eval Print Loop işlevini gören nodejs içinde gelen komut satırı uygulamasıdır. sadece "node" komutu komut satırına yazıldığında komut satırı yeni bir satıra geçer ve son kullanıcıdan javascrpt girişi bekler. buraya yazılan javascript'ler o anda işletilir. 

REPL genel bir karamdır. Örneğin Java 9 ile de REPL gelmiştir.

# yarn vs bower
npm'e alternatif nodejs için paket yöneticisileri.

# Gulp vs Grunt
nodejs modülleridir. bunlar sayesnde projemizde minify, auto-watch and auto-deploy gibi işlemlerimizi tasklar tanımlayarak yapabiliyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# how to use common static string variables

javascript kütüphanelerinin soruce code'una bakıldığında stringler hardcode girilmiş olarak görülebilir. bunun sebebi minify edilirken, yada minify edilmeden prod ortamına hazırlanırken, kodun daha hızlı çalışabilmesi için stringlerin gömülmesidir. 

örneğin; momentjs'te

> moment().add(1, 'month');

buraya gönderilen stringlerin common bir yerde tutulması tavsiye edilir.

momentjs geliştirme ortamında bu code'lar hardcode değildir. prod'a paketlenirken hardcode girilir.

yani; CommonStrings diye bir sınıf isteyerek yaratılmıyor. çünkü momentjs'te moment().add(1, CommonStrings.MONTH); kodu yerine momentjs'te moment().add(1, "month"); daha hızlı çalışmaktadır. dev ortamında CommonStrings varken canlı javascript kodlarında CommonStrings kullanılmamaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# javascript core ile obje klonlama

Ecmascript güncel versiyonları ile gelen bu syntax (yazılımcılar arasında "object spread" deniliyor):

```js
var newObject = { ...oldObject, {} };
```

aşağıdaki ile aynı işi yapmaktadır:

```js
var newObject = Object.assign(oldObject);
```

assign metodu sonsuz parametre alıyor. tüm parametreleri tek objede topluyor. 

Tek farkı şudur: Object.assign setter(newValue) kullanarak yeni objetye değerleri atıyor. yani setter metodu tetikleniyor. fakat spread direk nesneyi property atayarak oluşturuyor.

# Jquery ile obje klonlama

- Shallow copy

Shallow türkçe kelime anlamı: yüzeysel

```js
var newObject = jQuery.extend({}, oldObject);
```

shallow copy yeni obje yaratıyor. eski objenin en üstteki propertie'lerini geziyor. bu propertie'leri literal tipte ise; variablelelerini kopyalıyor. eğer propertie'ler obje ise referansları ile kopyalıyor. alt elementlere girmiyor.

- Deep copy

```js
var newObject = jQuery.extend(true, {}, oldObject);
```

Deep copy'de tüm alt elementler dahi geziliyor ve tüm değerler variable olarak yeni objeye kopyalanıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# reactJS

MVC mimarisindeki V katmanını karşılayan js kütüphanesidir. çünkü React'ın yaptığı; React.createElement(); yapıp render edilecek view'ı döndürmektir. Businnes logic'in içerde yapılıp yapılmadığı developer'ın kendisine kalmıştır.

React bir framework değildir. React fonksiyonel componentleri sadece render edilecek objeyi döndürecek şekilde tasarlanmıştır. react bu sebeple bir framework değildir.

- # jsx

Jsx dosya uzantısıdır. developer kodları tarayıcıya atmadan, bir transpiller aracılığı ile kodları js'e çevirir. developer web tarayıcısından debug ettiğinde js dosyası görür. jsx, direk olarak ecmascript implementasyonu değildir. fakat ek kurallar içererek bazı kolay okunabilirlik, hızlı yazılabilirlik gibi özellikler kazandırmaktadır. jsx sadece react için yazılmış bir syntax değildir. fakat ilk reactjs ile çıktığından her tarafta bu şekilde anılır. piyasada "react" syntax'ı denildiğinde jsx uzantısı kastedilir. oysa bu algı yanlıştır.

react örnek kodları minimum ecmascript 6 (güncel) sürümünü kullanır. fakat istenirse eski sürüm js'ler ile de react kütüphanesi çağrılıp kullanılabilir. react kodları incelenirken javascript'teki class gibi birçok yeni yapı görülebilmektedir.

Örnek bir jsx dosyası:

```jsx
var Sayac = React.createClass({

  artir: function () {

        var a = 3;
   },

  render: function () {

    return (

         <div>
          <View style={styles.progressCircleContainer}>
              <Progress.Circle size={50} indeterminate thickness={10} />
          </View>
        </div>
    }
})
```

Yukarıdaki kod tarayıcıda transpiller'dan geçtikten sonra aynı kalır; sadece render'ın içi şu şeklde döner:

```js
return React.createElement(

        "div",

        null,

        React.createElement(

             View,

             { style: styles.progressCircleContainer },

             React.createElement(Progress.Circle, { size: 50, indeterminate: true, thickness: 10 })
        )
```

AngularJs'te olduğu gibi tüm değişkenlerin değişip değişmediğini kütüphanenin kendisi hep kontrol etmiyor. SetState() metodunu yazılımcının çağırması bekleniyor. Bu sebeple performans açısından avantaj sağlanıyor.

React componentlerinde (view'da) bir değişiklik olduğunda, react komple component'i tekrar render ediyor. Burada performans sorunu ortaya çıkıyor. Fakat bunu çözmek için virtual-Dom mekanizması devreye giriyor. React sanal-dom üzerinde değişiklikleri render ediyor ve gerçek dom'a sadece değişiklikleri aktarıyor (buradaki sürece verilen özel isim: __Reconciliation (yada Uyumlaştırma)__)

Sayfa render edilirken state değişikliği yapılmaması gerekiyor.

State deişikliklerinde zaten render metodu çalışacağı için sayfa yeniden yeni değerlere göre değişecektir. dolayısı ile yazılımcı update kodlarını yazmaktan kurtulacaktır. angular-js'te two-way binding olduğundan ekrandaki değişkenler otomatik güncellenir, fakat businnes logic isteyen kodlar işlenmez. sadece variable'ler güncellenir. angular-js'te de sayfa initialize olurken herşeyi parametreki alırsa, orda da güncelleme için kod yazmak gerkemez, fakat bu durumda o projede zaten react kullanılması daha mantıklı olacaktır. çünkü react bu feslefe üzerine kuruludur.

__Fiber__ react'ın 16'ıncı versiyonu ile gelen Reconciliation motorudur.

- # page navigation
reactJS'te geçişler single page'dir. web tarayıcısında farklı bir sayfaya gidilmez. fakat rect-native'de bazı routing kütüphaneleri tek activity kullanırken, bazıları her sayfayı farklı activity'lere yönlendirebilir.

React'ta temel olarak routing aşağıdaki şekilde çalışır. About, Users, Home componentleri bulunan sayfaya göre render ediliyor yada edilmiyor. bir routing gerçekleştiğinde tüm sayfa değil sadece Switch'in içi değişiyor. tab yapısına çok benziyor.

```jsx
<Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
            <li>
              <Link to="/users">Users</Link>
            </li>
          </ul>
        </nav>

        <Switch>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/users">
            <Users />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
```

 - ## react-router
   routing yapmak için bir kütüphanedir. eğer react-native kullanıyorsak react-router-native paketini de eklmememiz şarttır. eğer html geliştiriyorsak; react-router-dom eklememiz şarttır.

   DOM ve native paketlerinde aynı interface'ler ile API sunar.

 - ## react-router-redux (yada eski adı:redux-simple-router)
   tekrar deprecated oldu, artık __connected-react-router__ paketi kullanılıyor.

   react-router'ı wrap ediyor. bu kullanımı opsiyonel olan bir js kütüphanesidir. redux state'ine otomatik olarak:
   - url
   - url'ile geçilen path parametreleri
   
   gibi birçok history meta bilgisi ekliyor. bu şekilde developement/debug işlerimizi çok kolaylaştırabiliyoruz.

   __routerMiddleware__ connected-react-router'ın middleware'idir. kullanılması zorunlu diildir. fakat dispatcher'ın push gibi history işlemleri yapabilmemiz için şarttır. eğer bu olmazsa sadece linkler ile gidilebilir. routerMiddleware initialize olurken parametre olarak history (history.js değil! history başka başlıkta anlatılıyor) kütüphanesini istemektedir.

   connected-react-router, redux state'ini değiştirdiği için, state'deki değişikliklerimizi belirleyen reducer'larımıza kütüphanenin reducer'ını eklememiz şarttır:

   ```js
   // reducers.js
   import { combineReducers } from 'redux'
   import { connectRouter } from 'connected-react-router'

   const createRootReducer = (history) => combineReducers({
      router: connectRouter(history),
      ... // rest of your reducers
   })
   export default createRootReducer
   ```

   Resmi github sayfaındaki FAQ'da güzel notlar var: https://github.com/supasate/connected-react-router/blob/master/FAQ.md (archive date: 03/12/2019)

 - ## react-native-router-flux
   
   TODO bu kısımı tümüyle gözden geçir !!

   sadece react-native için alternatif routing kütüphanesidir. react-navigation için farklı bir API sunmaktadır. Bu sebeple kod sadece javascript'ten ibarettir.

 - ## react-navigation vs react-native-navigation
   ikiside sadece react-native için routing kütüphanaleridir.

   react-navigation kodunun çoğu javascript iken, react-native-navigation'ın çoğu kodu native dillerle yazılmıştır.

   - ### react-navigation

        ```js

         // App.js

         import { createStackNavigator } from "react-navigation";

         import Home from "./scenes/Home";
         import PushedView from "./scenes/PushedView";

         export default createStackNavigator({
            Home: {
                screen: Home,
                navigationOptions: {
                    title: "Home"
                }
            },
            PushedView: {
                screen: PushedView
            }
         });

        ```

        ```js
        // scenes/Home.js

        type Props = {
            navigation: Object
        };

        export default class Home extends Component<Props> {

            goToPushedView = () => {
                this.props.navigation.navigate("PushedView");
            };

            render() {
                return (
                    <View>
                        <Button
                            onPress={this.goToPushedView}
                            title={"Push something"}
                        />
                    </View>
                );
            }
        }
        
        ```

        Yukarıda this.props.navigation.navigate yerine this.props.navigation.push kullanılırsa, aynı sayfalara tekrar gidilmesi sağlanabilir. Aynı sayfalara tekrar gidince aynı activity'den yeni bir tane daha oluşturulmuş olur.

   - ### react-native-navigation

         1 ve 2inci sürümleri arasında çok büyük farklılıklar var.

 - ## NavigatorIOS
   sadece ios için çalışan bir routing kütüphanesi. desteği kaldırıldı.

 - ## createStackNavigator vs createSwitchNavigator
   react-navigation'ın (sadece native için geliştirilen routing kütüphanesi) fonksiyonlarıdır.

   createSwitchNavigator authantication süreçleri gibi (örnek login olma) geri tuşunun olmayacağı ve her defasında yeni açılan sayfanın eskisini yoketmesi ile çalışan sayfalarda kullanılır. kullanıcı geri gidememelidir.

   createStackNavigator ise geri tuşuna basıldığında bir önceki sayfaya gidilen cinsten routing yapmamızı sağlayan metoddur.

   bu metodlara sayfalarımızı (component) ve config'lerimiz geçeriz.

  - ## history

    history.js (https://github.com/browserstate/history.js/) ile bir bağlantısı yoktur.

    https://github.com/ReactTraining/history reposunda geliştirilen pakettir.

    redux ve react-native'den tamamen bağımsız bir js kütüphanesidir. react-router-redux bu kütüphaneden yararlanarak çalışır. 

    normal koşullarda web tarayıcısı history'yi tutar. "history" paketi ise js motoru üzerinde history'nin tutulmasını sağlar ve bize history change listener, change history gibi birçok metodun bulunduğu API sunar.

    aşağıdaki import'lar yapılarak history kullanılabilir olur:

    > createBrowserHistory

    html 5 destekleyen tarayıcılar

    > createMemoryHistory

    react-native gibi js motorunun web tarayıcılar tarafından yönetilmediği platformlar için

    > createHashHistory

    html5 olmayan tarayıcılar

    örnek kod:
    
    ```js
    import createHistory from "history/createBrowserHistory"

    const history = createHistory()
    // Get the current location.
    const location = history.location
    ```

  - ## react-history
    "history" paketi için react component wrapper'ıdır. örnek kod:

    ```xml
    <History>
            {({ history, action, location }) => (

              <p>
                The current URL is {location.pathname}
              </p>
            )}
    </History>
    ```

# setState()
aldığı ilk parametre bir json objesidir. bu obje state'e ekleniyor, var olan değerler varsa update ediliyor. ikinci parametre ise opsiyonel callback metodu.

setState metodu asenkrondur. bu sebeple, setState'e yolladığımız callback metodumuzunda hiçbir zaman senkron çalışmayacağını unutmamak gerekli. js tek thread çalıştırılıyor. dolayısı ile thread'imiz, yani setState'i çağıran metodumuz bitmeden, callback metodumuz da çağrılmayacak. 

Component'in içindeki this.props ve this.state değerleri setState metodu tetiklenip, render işlemi bittikten sonra güncellenir.

dolayısı ile bazı durumlarda; aşağıdakinin yerine:

```js
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

bunu yapmak isteyebiliriz:

```js
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

Bu bize; render tamamlanmış olmasa da, en son çağrılan setState'in state değerini verecektir.

aynı zamanda react setState işlemlerini kuyruğa alıp sıra ile işlemez. onun yerine hepsini birleştirir öyle render'ı çalıştırır.

```js
handleIncrement = () => {
  this.setState({ count: this.state.count + 1 })
  this.setState({ count: this.state.count + 1 })
  this.setState({ count: this.state.count + 1 })
}

handleDecrement = () => {
  this.setState({ count: this.state.count - 1 })
  this.setState({ count: this.state.count - 1 })
  this.setState({ count: this.state.count - 1 })
}
```

Yukarıdaki handleIncrement metodundaki kod'daki state buna eşittir:

```js
Object.assign(  
  {},
  { count: this.state.count + 1 },
  { count: this.state.count + 1 },
  { count: this.state.count + 1 },
)
```

Dolayısı ile bu şekilde yapmalıyız:

```js
handleIncrement = () => {
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  this.setState((prevState) => ({ count: prevState.count + 1 }))
}
```

# forceUpdate()
render metodunun çalışmasını sağlıyor. child componentlerinde update olmasını sağlıyor. bu metodun kullanılması önerilmiyor. zaten state yada props değiştiğinde render otomatik çağrılıyor. bu sebeple bu metoda gerek yok. faat bazen class yada dışardaki farklı static değerler render içinde kullanılıyorsa (ki bu hiç önerilmiyor) forceUpdate metoduna ihtiyaç olmaktadır.

# component objesi metodları (lifecycle)

- #### sayfa render olurken bir component'in sırası ile aşağıdaki metodları çağrılır:

  > constructor

    burada setstate metodu çalışmaz. this.state = {...} state değişkenşni direk değiştiririz.

  > componentWillMount
  
  bu metod tamamlanmadan render çalıştırılmaz. bu sebeple burada çalıştırılan setstate() işlemleri re-render işlemini tetiklemez. componentWillMount içinde çalıştırılan async metodlar bu kurala dahil diildir.

  > render( )
  
  pure bir metod olmalı aynı state'e karşılık hep aynı şeyi döndürmeli.

  > componentDidMount
  
  ajax call'ların burada yapılması önerilir. burada setstate işlemi re-render'ı tetikler.

- #### Bir componentin state'inin yada props'unun değişmesiyle tekrardan render edilmesi sonucu çağrılan methodlardır:

  > componentWillReceiveProps( nextProps )
  
  metodun icinde nextProps vs this.props karşılaştırması yapabiliriz

  > shouldComponentUpdate( nextProps, nextState )
  
  eger false dondurursek buradan sonra hicbir metod cağrılmaz

  > componentWillUpdate( nextProps, nextState) )
  
  setState işlemi burada yapılmamalı. eğer şart ise bir önceki metodlar tercih edilmeli.

  > render( )

  (yukarıda açıklandı)

  > componentDidUpdate( )
  
  ajax işlemleri burada olmalı



- #### Komponent yok edilirken çalıştırılacak metod:

  > componentWillUnmount()
  
  obje destroy olduğunda tetikleniyor



# prop vs state

props; properties'in kısaltmasından gelir.

componentlerde hem prop hemde state isimli json'lar vardır. ikisinden biri değiştiğinde render tekrardan çağrılır. bu sebeple bu iki json, ancak setState() gibi metodlarla değiştirilmelidir. çünkü react ancak bu şekilde render'ı çağıracaktır. state.myValue=9; değişikliğini react bilemez, ve render'ı çağıramayacaktır.

prop bir üst componentten aktarılan event ve variable'lardır. state ise component'in tamamen kendi içinde tuttuğu variable'lardır. propslar mantıken değiştirilmemelidir. state ler ise; değiştiğinde sayfanın render'ının çalışması gerektiği verileri içermelidir. değiştiğinde sayfa render edilmeyecekse, component class'ının variable'ı tanımlanmalıdır.

bir component prop'ların nereden geldiğini hiç bilmez. componenti çağıran component ona datayı kendi state'indne yada redux'ın stateinden atmış olabilir. aşağıdan yukarıya data geçilemez (tek istisna: prop ile component'e onEventChange metodu yollarız. ve değişikliği bu metod aracılığı ile manuel isteyebiliriz). bu duruma __yukarıdan-aşağıya (yada tek yönlü yada top-down or unidirectional) veri akışı (yada data flow)__ denir.

# defaultProps

```js
class CustomButton extends React.Component {
  // ...
}

CustomButton.defaultProps = {

  color: 'blue'
};
```

undefined propertie'leri için kendini koruyan kod yaratıyor. null prop'lar hala null kalmaya devam eder.

# props.children

```js
export default class Panel extends React.Component {

  render() {

    return (

      <div>

        <div className="panel-header">{this.props.title}</div>

        <div className="panel-body">{this.props.children}</div>

      </div>
    );
  }
}
```

Yukarıdaki component tanımlandıktan sonra aşağıdaki gibi kullanılabilir:

```xml
<Panel title="Browse for movies">

  <div>Movie stuff...</div>

  <div>Movie stuff...</div>

  <div>Movie stuff...</div>

</Panel>
```

# propType

asagidaki gibi proptan geçilecek parametrelerin tipi belirleyebiliriz.

```js
class MyComponent extends React.Component {
     ...
}

MyComponent.propTypes = {

  myValue1: PropTypes.array, //yada PropTypes.array.isRequired

  myValue2: PropTypes.bool,

  myValue3: PropTypes.instanceOf(ClassName),

  myValue4: PropTypes.oneOf(['potato', 'turkey'])
}
```

react'ın eski versioyonlarında propsTypes yerine contextTypes kullanılıyordu.

# controlled vs uncontrolled component

A controlled component accepts its current "value" as a prop + callback to change that value. example contolled component:


```jsx
<input value={someValue} onChange={handleChange} />
```

Uncontrolled component:

```jsx
<input onChange={handleChange} />
```

Uncontrolled olan versiyonda two-way binding olmayacak.

controlled component daha çok react'a uygun tarzda yazılmış oluyor.

react event'leri component'in kensininde define etmiş şekilde sayfayı render etmiyor. react arkaplanda her event'i tutuyor ve yönetiyor.  örneğin yukarıdaki örneklerde onChange event'lerini direk html inputumuz içerisinde göremeyiz.

react'ın DOM'u kendisinin yönetmesinden sebep; beklenmedik durumlarla karşılaşabiliriz. örnek:

```jsx
<input
    inputType="text"
    value="try to change this"
/>
```

Yukarıdaki input son kullanıcı tarafından değiştirilemez olur. çünkü react "value" değerini sabitlemiştir. input'a onChnage event'i koysak tetiklenecektir. fakat değer yine değişmeyecektir.

controlled olup olmadığı "value" keyword'ü ile karar veriliyor. tabi component'in onchange tarzı metodu da olmalıdır. onchange'in keywordü önemli diil.

"value" keywordü bazı html objelerinde olmadığı için react render ederken onlara özel olarak ek alanlar/çözümler yaratılmıştır. örnekler:
- input type "text" için; "defaultValue" ile value değeri atayabilmemizi sağlamaktadır.
- textarea'nın normalde (html standartlarında) value değeri yoktur. içerisindeki metin textarea etiketinin dışına yazılır. fakat react'ta value kullanılabilirdir.

react'ın kendi DOMlistener'ları vardır. her event'i, native tarayıcı eventlerinden önce react okur, yada sonra yazılımcının yazdığı fonksiyonlara yollar. Yazılımcının yazdığı metodlara event geldiğinde SyntheticEvent gelir. bu even normal event'i wrap etmiş bir objedir. Eğer tarayıcının event'ine ihtiyacımız var ise, gelen event objesinin içindeki nativeEvent isimli "DOMEvent" objesi kullanılabilir.

React event dönüşlerini de hemen tarayıcıya iletmez. arada yine react metodu olacağı için bazı beklenmedik durumlar ile karşılaşabiliriz. örnek: event metodlarımızdan false döndürmek olay yayılımını (propagation'ı) durdurmayacaktır. bu sebeple stopPropagation() gibi fonksiyonları kullanmamız gerekir. 

# componentwillmount mı? componentdidmount mı? ajax call için uygun?

willmount'ta ajax call yapılabilir. fakat öneri didmount'ta yaılmasıdır. çünkü: 

- client side için sebep: asenkron bir isteğin (örnek ajax call) ne zaman dönüş yapacağı belirsizdir ve değişkendir. biz kodumuzu geliştirirken render metodunu 2 türlü test etmeliyiz: response-data boşken (daha dolmamış) ve doluyken. fakat eğer testlerimiz/kod geliştirmemiz sırasında ajax calları erken dönüş yapıyorsa; biz response-data boşken (daha dolmamış) durumunu test edememiş olabiliriz. buna ihtimal bırakmamak için ajax calları didmount'ta yapmalıyız.

piyasada yazılımcılar; render işlemi başlamadan requesti yapayım düşüncesindeler. böylece render ederken request gitmiş olur diye düşünülüyor.

- server side için sebep: jsp gibi, react ta istenirse suncuda derlenip client tarafa atılabilir. sunucu tarafta derlendiğinde componentwillmount metodu hayat döngüsünde çalıştırılmaz. bu sebeple ajax callarımızı componentdidmount'de çalıştırmalıyız.

# container vs component

component react sınıfıdır. bu sınıftan türetip tekrar kullanılabilir componentlerimizi oluştururuz. cointainer (yada container component) ise resmi olmayan bir tanımdır. best practice'lere göre container;

- direk GUI objeleri render etmemeli. container'ın render kısmında, componentler GUI objelerini render etmeli.

- cointainerlar redux/store'lara bağlantı kurup componentlere basmaları gereken bilgileri ve handle metodlarını prop olarak vermelidir.

container'lar sayfa veya sayfanın bölümleri (footer, widget gibi) componentlere denir. componentler ise; textboxlar, buttonlar, textler olarak kullanılır.

yine resmi olmayan tanımlarda; cointainer olmayan componentlere "__Dumb (yada çöp yığını) component__" veya "__presentational (yada sunumsal) component__" adı verilmektedir.

conitaner'lar "__smart component__" olarakta adlandırılırlar. bazıları container'lara "screen" demeyi de tercih edebiliyor. fakat screen, footer componentlerini kapsamamaktadır. bu sebeple screen demek pek doğru olmayacaktır (bu biraz da projenin yapısına bağlı).

# fonksiyon call bindings

Metodları çağrırıken metodların içindeki "this" değerinin undefined olmaması için doğru context'in bind edilmesi gerekmektedir. Çünkü JSX'teki html objeleri bizim JS class'ı (component class'ımız oluyor) içindeki fonksiyonu çağırdığında this değeri o sıra js'teki genel context olacaktır. Oysa biz o fonksiyona; JS class'ımızın context'ini geçmek istiyoruz ki; class içerisindedki state'i props'u kullanabilelim. örnek doğru yöntem:

```jsx
class MyComponent extends React.Component {

  constructor(props) {
    ...
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // eger yukarıda gerçek context'i bind etmezsek bu değer bizim istediğimiz değer olmayacak. (peki hangi değer olacak. bu ayrı bir araştırma konusu.)
    this.setState(...
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

Hiç bind işlemi yapılmaz bu yapılabilir:

```jsx
<button onClick={(e) => this.handleClick(e)}>
  Click me
</button>
```

Fakat bunun 2 dezavantajı var: (kaynak: https://en.reactjs.org/docs/handling-events.html (archive date: 10/11/2019) "Passing Arguments to Event Handlers" başlığından hemen önceki cümle )
- her render işleminde bu metod yeni anonim metod generate edecektir
- bu metod html-button'a diilde bir React component'ine (örnek MyButtonComponent) prop olarak aktarılsaydı, her render edilişinde yeni anonim metod geleceği için,  MyButtonComponent tekrardan render edilecekti.

# Fonksiyonel bileşen (yada functional component) vs class component (yada sınıf bileşen) 

fonksiyonel:

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

aynı işi göre sınıf bileşeni:

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

# React.PureComponent
React.Component'e ile neredeyse tamamen aynıdır. çok ufak farkları vardır. daha hafiftir/performanslıdır.

# Errors
Exception değildir. js'te veya jsx'lerde olan hataları yakalamak için kullanılır. 

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Bir sonraki render'da son çare arayüzünü göstermek için
    // state'i güncelleyin.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // Hatanızı bir hata bildirimi servisine de yollayabilirsiniz.
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // İstediğiniz herhangi bir son çare arayüzünü render edebilirsiniz.
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}
```

bu component'imizi bu şekilde kullanabiliriz:

```jsx
<ErrorBoundary>
    <MyWidget />
</ErrorBoundary>
```

Yukarıda ErrorBoundary, sadece child component'lerde hata olursa hatayı catch edebilecektir.

React 16'dan itibaren, bir hata sınırı tarafından yakalanmamış hatalar, tüm React bileşen ağacının devreden çıkmasına neden olacaktır. Bunun sebebi:

- Messenger gibi bir üründe hatalı bir arayüzün görünür kalması, birinin yanlış kişiye mesaj göndermesine neden olabilir.
- bir ödeme uygulamasında yanlış miktarın görüntülenmesi, hiçbir şey görünmemesinden daha kötüdür.

# hook
react 16.8'den gelen bir özelliktir. kullanımı opsiyoneldir. funcitonal componentlere benzer ve ek özellikler sunar. hook, bazı fonksiyonlar sunar. bu fonksiyonlar bazı eventlerde devreye girerler. yani fonksiyonel component'lere ek özellik katarlar. dolayısı ile normal component'lerde kullanılamazlar. direk örnek üzerinden gidersek:

```jsx
import React, { useState } from 'react';

function Example() {
  // "useState" metodu 2 parametre dönüyor:
  //   1- variable'ı tutan değer
  //   2- variable'ı set eden fonksiyon
  // örneğimizde;
  // "count" diyeceğimiz yeni bir state değişkeni tanımlayın
  // "count"'un ilk değeri 0.
  // setCount set ediyor olsun.
  const [count, setCount] = useState(0);

  // bu kod bloğu örneğinde kullanmadığımız, diğer örnek state'lerimiz:
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

setCount fosnkiyonu setState gibi merge işlemi ile uğraşmaz. direk olarak tüm objeyi/state'i replace eder.

"useEffect" componentDidMount + componentDidUpdate + componentWillUnmount metodlarının tek bir metod'da toplanması olarak düşünülebilir. useeffect hem sayfa ilk init olduğunda ve daha sonraki her render işleminde çalışır. örnek:

```js
import React, { useState, useEffect } from 'react';

function Example() {
  
  // some code here (from below example)

  useEffect(() => {
    
    document.title = `You clicked ${count} times`;
  });

  // some code here (from below example)
}

```

useEffect fonskiyonunda return ettiğimiz fonksiyon var ise, component remove olduğunda o fonksiyon çalıştırılır. burada kaynakları serbest bıraktığımız kodlar yazılmalıdır. örnek:


```js
import React, { useState, useEffect } from 'react';

function Example() {

  useEffect(() => {
    
    // any code here

    return () => { 
        // clear resources here! 
    }
  });

}

```

useEffect fonskiyonuna ikinci parametre olarak variable geçebiliriz. bu varible'lar dğeiştiğinde otomatik olarak useEffect çalışacakır. normalde useEffect sadece componentDidMount + componentDidUpdate + componentWillUnmount'da çalışacaktır. fakat ek olarak bir değer veridğimizde o değerler değiştiğinde de useEffect çalışır.

# custom hook
kendi hook'umuzu yaratabliriz. direk örnek üzerinden gdelim:

```jsx
import React, { useState, useEffect } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`The count is ${count}`);
  });

  return (
    <div>
      <p>Count is {count}</p>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        increase
      </button>
    </div>
  );
}
```

Yukarıda useEffect her renderda çalışır.

eğer bunu yazarsak useEffect sadece bir kere component mount olduğunda çalışacaktır:

```js
useEffect(() => {
    console.log(`The count is ${count}`);
}, []);
```

eğer bunu yaparsak sadece "name" değiştiğinde useEffect çalışacktır:

```js
useEffect(() => {
    console.log(`The count is ${count}`);
}, [name]);
```

Şimdi gelelim custom hook'lara. useEffect sadece çağrıldığı componentin (fonskiyonel coomponentin) render'ını takip eder. dolayısı ile dış dünyadan izoledir. bu şekilde state tutan ve kendince render olan fonksiyonlar yaratabiliriz. bunara hook denir. kendi hook'larımızı "__use__" prefixi ile yazmamız önerilmektedir.

örnek bir custom hook'u inceleyelim:

```jsx
import React from "react";

// "useUserCollection" bizim custom hook'umuz.
// "useUserCollection" MyComponent içerisinde çağrılıyor. bu sebeple buradaki tüm kodlar sanki MyComponent içindeymişçesine çalışacaktır.
// örneğin useUserCollection içindeki useEffect, MyComponent her render olduğunda çalışacaktır.
const useUserCollection = () => {
  const [filter, setFilter] = React.useState("");
  const [userCollection, setUserCollection] = React.useState([]);

  const loadUsers = () => {
    fetch(`https://jsonplaceholder.typicode.com/users?name_like=${filter}`)
      .then(response => response.json())
      .then(json => setUserCollection(json));
  };

  React.useEffect(() => {
    // bu blok MyComponent her render olduğunda çalışacaktır.
    console.log("useEffect of useUserCollection");
  });

  return { userCollection, loadUsers, filter, setFilter };
};

export const MyComponent = () => {
  // useUserCollection custom hook'umuz bize birden fazla obje döndürüyor.
  const { userCollection, loadUsers, filter, setFilter } = useUserCollection();

  // burası componentimizdeki custom businnes logic
  React.useEffect(() => {
    loadUsers();
  }, [filter]);

  return (
    <div>
      <input value={filter} onChange={e => setFilter(e.target.value)} />
      <ul>
        {userCollection.map((user, index) => (
          <li key={index}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
};
```

# react-helmet
node modülüdür. Helmet component'ini sunar. Helmet componenti içinde render edilen html objeleri, header'a koyulurlar (eğer zaten önceden header'da aynı etiketler var ise onları override eder).

örnek:

```jsx
import React from "react";
import {Helmet} from "react-helmet";

class Application extends React.Component {
  render () {
    return (
        <div className="application">
            <Helmet>
                <meta charSet="utf-8" />
                <title>My Title</title>
                <link rel="canonical" href="http://mysite.com/example" />
            </Helmet>
            
            ..
            <input type="text" defaultValue="hello world">
            ..
        </div>
    );
  }
};
```

# ReactNative

Mobil platformlar için geliştirilmiş bir framewowk'tür. Bu çatıda react-js syntax'ı ile (sınıfların isimleri aynı örnek: component) javascript kodları, react-native modülü sayesinde mobil işletim sistemlerinin native kodlarına çevrilmektedir. cordova gibi yapılarda webview üzerinde çalışan arayüz, react-native'de native arayüzde çalışmaktadır.

cordova ile aynı şekilde parse işlemi yapılmaktadır. resource dosyalarında bulunan jsx dosyaları, runtime sırasında native arayüze çevrilmektedir. native componentlerin çevrilmesi az da olsa bir süre almaktadır. bu açıdan bakıldığında cordova ile bir fark yoktur. fakat daha sonra native componentler, webview'dakilerden daha hızlı çalışmaya devam etmektedirler. işte cordovaya göre performans açısından avantaj sağladığı nokta budur.

# JavaScriptCore

jsx'lerin parse işlemi JavaScriptCore ile yapılmaktadır. JavaScriptCore react-native içerisinde gelmekte ve uygulamanın runtime'ına eklenmektedir. JavaScriptCore ile çalışan uygulama native modül geldikçe native tarafta işlem yapmaktadır. örneğin; bir json objesi, yada bir integer native tarafta tutulmamaktadır. ancak ve ancak native modül ihtiyacı (kod satırı native modülü çağrıyorsa) native tarafta işlem gerçekleştirilmektedir. tüm işlemler javascript'te yapılmaktadır + render edilen sayfalar native modüller çağrarak UI render ederler. fakat o native UI'ların eventleri tekrar javascript core'da işlem yaparlar. örneğin onppres'teki i++ kodu javascript'te çalıştırılır.

google-chrome ile debug edilen native uygulama daki JavaScriptCore google-chrome'un içindekidir. google-chrome ile android içerisinde debug edilen uygulama websocker aracılığı ile haberleşmektedir.

# Reconciliation (yada uyumlaştırma)
React altyapısı arkaplanda tüm sayfayı render etmez. sadece gerekli alanları render eder. böylece performance artışı sağlar. burada bir hespalama yapılması gereklidir. hangi objelerin update olup olmayacağı algoritmasına Reconciliation denir.

react; __virtual dom (yada VDOM)__ oluşturur ve VDOM ile gerçek DOM'u karşılaştırarak Reconciliation yapar.

# shadow Dom
shadow dom kavramı native Web browser'ların kullandığı bir teknolojidir. react veya virtual dom ile hiçbir bağlantısı yoktur. shadown DOM query ile erişilemeyen DOM objeleri yaratabilmemizi sağlıyor. böylece sayfadaki CSS'lerden ve JS eventleri bu Objelere etki edemiyor. Örneğin facebook'un "share" buttonu. tüm dünyadaki web sayfalarında standart CSS ve event'lerle çalışması bekleniyor ise; bu "share" objelerini shadow DOM dom ile yapmalıyız.

shadow dom'lar web tarayıcı inspectoründe görülebilir fakat JS ile erişilemezler.

```js
// Shadow DOM API'si çok iyi araştırılmadı. aşağıdaki kodda hatalar olabilir. fakat temel olarak çalışma mantığı bu şekilde.

<span class="shadow-host">
</span>

// select element by class id
const shadowEl = document.querySelector(".shadow-host");

// make this element shadow
// attachShadow fonksiyonu "shadowEl" içindeki tüm dom objelerini shadow'a olarak set ediyor.
// bizim örnekte "shadowEl" içinde hiçbir şey yok. bu sebeple hiçbir DOM objesi shadow içine alınmayacak fakat "shadowEl" ve içi artık shadow DOM'a aittir.
const shadow = shadowEl.attachShadow({mode: 'open'});

// create a new element
const link = document.createElement("a");
link.href = "facebook.com/share"

// add the new element inside the shadow element
shadow.appendChild(link);
```

# Virtual DOM for react native

react native'de html dom'u olmadığından nasıl çalışıyor? react-core içerisindeki yapıda virtual dom görevini gören sistem çok esnek tasarlanmış. programlama dilinden bağımsız. dolayısı ile render'dan return edilen değerler ile varolan view karşılaştırılıyor. bu kaşrılaştırma sonucu sadece değişen kısımlar, o andaki sayfaya uygulanabiliyor.

# react activity

react uygulaması bir activity'dir. manifestte belirttiğimiz (yada ana sayfamızı yapmak zorunda diiliz) main-activity'miz ReactActivity'den türemiş bir activity olmalıdır. uygulama bunu çağırdığında react o activity için devreye girecektir.

React yapısı gereği activity dışında android'in Application hayat döngüsünde de bir kaç setupu vardır. Hatta Application'a ek olarak ReactApplication'dan da ekstend işlemi yapmalıyız. (android'in Application hayat döngüsü başka başlıkta anlatılıyor)

# native koda erişme

reat-native'de hem native koddan js'e hemde tersi mümkündür. fakat erişim işlemi asenkrondur.

# native-base

react-native gui elementlerinden extend edip kullanmamızı sağlayan react-native için geliştirilmiş bir modülüdür. yani bir katman daha eklenerek daha fazla nitelik kazandırılmaya çalışılıyor. aynı jsp->jsf katmanları gibi. native-base özellikle yeni event'ler/nitelikler değilde; hem ios hemde android'de kendi platformlarına göre daha oturaklı komponent yaratmak için tema(theme) felsefesinde geliştirilmektedir. 

# react-devtools

bu isim ile web tarayıcıları için eklenti mevcut. bu eklenti tarayıcıya kurulduğunda, tarayıcının developer tool'una ek bir sekme geliyor. bu şekilde sadece react-js debug'ı kolaylaşıyor. normalde sadece web tarayıcısı debug için yeterlidir. fakat bu eklenti inspect element işini COmponent bazlı yapıyor/gösteriyor ve her elemente geçilen prop'ları gösteriyor.

react-devtools ismi aynı olsa da bağımsız bir npm modülüdür. electron tabanlı bu npm modülü, react-devtools eklentini barındırıyor.

# react-native-debugger

react-devtools tabanlı bir debugger. ektra bazı özellikler içeriyor. örnek redux support.

# hot reload vs live reload

live reload'da proje js dosyalarında herhangi bir değişiklik olduğunda emulatordeki uygulamamız otomatik komple restart olur ve state'ini kaybeder.

hot reload'da ise uygulama restart olmaz ve state'ini kaybetmez. fakat değişiklikler RAM'de çalışan uygulamaya yansır. dolayısı ile değişikliği görebilmemiz için ilgili javascript kod bloğunun tekrar işletilmesi yeterli olacaktır.

yukarıdakiler sadece js dosyaları için geçerlidir. diğer dosyalar için uygulamayı tekrardan komple run etmek gerekir.

# npm start -- --reset-cache

sadece "npm start" yaptığımızda js dosyaları bundle halinde hazır hale getirilir ve http üzerinden sunuma açılır. js dosyalarında bir değişiklik olduğunda ise bu bundle baştan sona işlenmez sadece değişiklik olan kısım hemen bundle'de güncellenir. npm start komutu iptal edilip tekrar çalıtırıldığında npm tekrar bundle'yi üretmez. eskisini kullanır. fakat "-- --reset-cache" komutu ile bundle tekrardan sıfırdan üretilir.

"npm start", nodejs sunucusu ayağa kaldırıyor ve aşağıdaki gibi public URL'leri dışarıya açıyor:

http://localhost:8081/index.ios.bundle  //bundle js'i komple veriyor

http://localhost:8081/index.android.bundle

http://localhost:8081/debugger-ui.html //buradaki sayfada webWorker (javascript thread teknolojisi) kullanılıyor. Bu sayfa packager (npm start ile başlatılan uygulama) tarafından sağlanıyor. packager WebSocket teknolojisi ile  ile haberleşiyor.

emulatorde yada cihazda uygulamamızı başlattığımızda, emülatör gidip kodları http://localhost:8081/index.ios.bundle'dan çekmesi gerekir. packager'ın ip:port bilgisini mobil cihazımızın "react dev menu"sünde set etmemiz gerekir.

emülator/cihaz ve debugger(örnek: web tarayıcısı) uygulamaları nodejs sunucusuna; yani "npm start" ile başlattığımız packager'a bağlanıyorlar. artık ikisi(debugger ve emulatör) packager'i proxy olarak kullanarak haberleşiyor. eğer haberleşme başlar ise; web tarayıcısıda index.android.bundle dosyasını kendine packager'dan indiriyor. artık javascript dosyası emulatoröden diil, web tarayıcısının kendisinde koşmaya başlıyor.

# metro bundler
react-native'in bundle hazırlamasını yarayan ve npm modülüdür. "npm start" yaptığımızda bu çalışır.

# ref
ref her componente daha hızlı erişebilmemizi sağlayan bir fonksiyonalite sağlar. react'ta eskiden şu şekilde kullanım vardı:

tanımlama kodu:

> ref="myInput"

erişim için kod:

> this.refs.myInput

Daha sonra bu şekilde kullanım önerilmeye başlanmıştır:

tanımlama kodu:

> ref={(input) => { textInput = input; }}

erişim için kod:

>  this.textInput


# react için bazı best practice'ler/notlar

- bir component çok fazla prop almaya başladığında çok fazla özellşemeye başlamış anlamına gelir. bir süre sonra, her prop için if blokları component içinde artar. bu da karmaşıklığın artmasına sebep olur. bu sebeple; belli bir süre component'ler klonlanarak/kopyalanarak ayrı bir component haline getirilir. çünkü çok özelliştirme abartılırsa, yeni bir component olması gerektiği anlamına gelmektedir.

- bir liste elemanları için birer obje oluşturduğumuzda, listenin eleman sayısına göre/index'e göre key kullanmamamız şarttır. hatalı kullanıma örnek:

```jsx
const todoItems = todos.map((todo, index) =>
  // Bunu yalnızca öğelerinizin sabit ID'leri yoksa yapın
  <li key={index}>
    {todo.text}
  </li>
);
```

key=index yerine key=todo.id kullanmalıyız. çünkü listeye daha sonradan yeni bir eleman eklenirse, liste indexe göre hazırlandığı için hatalar meydana gelecektir.

"key" react için özel bir keyword'dür. eğer 'id' yoksa 'key'e bakılır. key random da atanmamalıdır. çakışma olursa hata olacaktır. id veya key her zamn atamak zorunda değiliz, fakat listeye elemen eklenecekse, çıkarılacaksa (yani re-render) edilecek ise mutlaka key yada id olmalıdır.

- react componentleri birbirinden extend etmemeleri (class extends yöntemi) önerilir. onun yerine birbirini kapsayan (composite) componentler tercih edilmelidir. önerilen kullanım aşağıdaki gibidir:

```jsx
<MyCustomComponent> <AnotherComponent> ... </AnotherComponent> </MyCustomComponent>
```

"composition over inheritance" başlığında anlatıldığı gibi bu durum react içinde geçerlidir. varolan data sadece proplardan ve kendi state'mizden gelmelidir. diğer datalar bizi ilgilendirmemelidir. eğer react component'lerimiz inheritance olursa, super class'tan gelecek objelerinde variable'larına göre kod yazmamız gerekir.

dikkat edilirse redux kullanırken mapToProps ile redux store'undan ihtiyacımız olan değerleri prop olarak kendi komponent'imize bağlarız. dolayısı ile redux'ın store'u react'tan bağımsız bir yapı olmasına rağmen, props aracılığı ile component içine çektik. böylece focus'umuz sadece props ve state değerleri olmuştur. komplekliği azaltmış oluyoruz.

- tüm hibrit uygulamalarda olduğu gibi Native tarafın UI Thread'i ile Javascript moturunun thread'i farklı olduğundan bu ikisinin haberleşmesi de çok yavaştır. özellikle döngü içinde her seferinde native kod çağırmak çok maliyetlidir.

- react setState içerisinde === kontrolü ile değişiklik olup olmadığına bakar.  eğer değişiklik varsa sadece değişikliğin paramtre olarak yollandığı komponentleri render eder. sebeple bussines logic'ler render içinden kaldırılıp, render metodu içindekiler komponentlere bölünerek, bussinnes logicler oralara taşınmalıdır. bu şekilde istatistiksel olarak daha az kod işlenecektir. zira belki o komponent hiç tekrardan render edilmeyecek ve businnes logic kısmı dahi çalıştırılmayacaktır. fakat businnes logic component dışında olsaydı render metodunun başlaması ila çalıştırılıyor olacaktı.

   örnek;

   ```
   render(){
     if(state.hesaplar == true){
          <div> state.accounts </div>
     }
   } 
   ```

   re-render edildi. if kontrolü yapıldı. oysa hesaplar değerinde hiç değişiklik yoktu. o zman aşağıdakini yazmak daha mantıklı olacaktır:

   ```
   render(){
      <HesaplarKOmponent value={state.hesaplar} accounts={state.accounts}/>
   } 
   ```

- state'i bir value'ya atayıp değiştirip sonrada o value'yu state'e set etmek yanlış. aşağıdaki örnek yanlış:
   ```
   var x = this.state.x;

   x.z.push("foo"); // z is a list

   this.setState({x:x});
   ```

   burada olabilecek sorunlar: 

     - react setState metodunu işletirken state'i değişmemiş sanabilir. çünkü içini biz zaten değiştirdik (o anda ilgili tüm componentShouldUpdate gibi metodları incelemek gerekir) 

     - paralel bir thread state'i kullanıyor/değiştiriyor olabilir. state'i elle değiştirmemiz bir çakışmaya yol açabilir.

  bu sebeple bunun yerine Object.assign kullanıp obje klonlanmalıdır. listeler içinse [].catcat([ newElement1, newElement2 ]) kullanmalıyız.

- React render veya içindeki farklı bir component'i render edip etmeyeceğine karar vermek için deep comparison yapıyor. yukarıdaki notta state'i elle değiştirmekten bahsettik. eğer elle değiştirseydik ve hemen ardından setstate çağırsaydık bu bizim zaten dezavantajımıza olabilirdi. çünkü Immutability react'ta çoğu zaman avantaj sağlıyor. çünkü genelde state'in içinden bir eleman değiştirildiğinde zaten component'in render olması gerekecek. fakat react önce statein tümünü dolaşarak buna karar veriyor. fakat react dolaşırken, daha az dolaşmasını sağlamalıyız. bu sebeple en tepedeki bir objenin klonunu o obje yerine yerleştirdiğimizde, react direk olarak en tepeden kontrol etmeye başlayacağı için değişikliği görecek ve render işlemine hemen başlayacak. yani react framework'ünün compare işlemini kısaltmış olacağız. tabi bu her zaman geçerli olan bir durum diil. örneğin; 

   state = {x, y} olsun. render metodunuzun içinde sadece Y'yi kullanan 100 adet component olsun. 1 adet componentte Y içindeki A değerini kullansın. sadece A yı değiştirmek istersek, object assaign ile Y'yi komple yeni obje olarak clonlayacağımızı için 100+1=101 adet component tekrar render olacaktır. oysa sadece Y.A yı değiştirseydik, sadece 1 komponent render olacaktır. yani object assaign her zaman avantaj sağlamayabilir. 

- loading bar

  paraleleden işlem yapılırken loading bar gösterilir.

  ```jsx
  render(){

   if(state.loading){

      <LoadingBar>

   } else {

      <SCREEN>
   }
  }
  ```

  yerine;

  ```jsx
  render(){
      <LoadingBar hidden=state.loading>
      <SCREEN>
  }
  ```

  yapılmalıdır. çünkü state.loading değiştiğinde sadece zaten render olmuş bir bar hide edilecektir. diğer türlü state.loading değiştiğinde tüm sayfa render edilecek.

- render metodu içinde businnes logic hiç olmamalı. render'ın görevi sadece sayfayı oluşturmaktır. logic'lerle ilgilenmek farklı bir görev.

- render metodu içinde anonim metod olmamalı. çünkü render'ın sürekli çağrılacağı düşünülerek uygulama tasarlanmalı. anonim metodlar runtime sırasında oluştururlur. bu da zaman kaybı yaratır. anonim metod yerine; önceden bir kere metod oluşturulmalı ve o metod kullanılmalıdır.

  aslında bu kural genel olarak herşey için geçerli fakat genelde her taraftaki anonim metodları sabit metod olarak ayarlamak kodun daha kötü okunmasına sebep oluyor. buna normal koşullarda tölerans gösterebiliyoruz. fakar render metodu sürekli çalışacağı için burada bu konuda tölerans göstermemek gerekiyor.

# "react-native link" komutu
/root/node_modules/ dizini altında native pltaform bağımlı kütüphanaler olabilir. burada bulunan kütüphanelerin /root/ios/ ve /root/android/ altındaki projelere import edilebilmesi için bu komut kullanılır. istenirse komutsuz manuel de import yapılabilir.

# react-native-cli vs react-native
react-native-cli komut satırından react-native projesi oluşturma gibi komtuları yerien getirmek için yapılmış, ilgili projenin dependency'lerinde olmak zorunda olmayan bir node modülüdür.

react-native ise react-native'in runtime'da çalıştırması gereken bir node modülüdür.

# react vs react-dom vs react-native
react-native çıkmadan önce sadece reactj vardı. reactjs, react paketi ile dağıtılıyordu. react-native çıkışı ile birlikte "react" paketi her platform için "react-core" paket oldu. web development yapanlar react-dom'u, mobile geliştiricileri is react-native'i de package.json'a eklemelidirler.

# RNPM (yada react native package manager)
eskiden kullanılan fakat artık react kütüphanesinin içine entegre edilen node modülüdür. komut satırından paket kurulumları, link işlemlerini yapabilmemizi sağlıyordu.

# react dizin yapısı

- / - root dizini

- /android - gradle root projesi. burası android studio için proje dizini. eclipse için workspace dizini.

- /android/settings.gradle - burada npm içindeki modullerin native java kodlarının dizinleri mevcut. bu şekilde her proje IDE tarafından workspace'te görülebilir olacaktır. bu sebeple android studio açıldığında bir sürü proje görünmektedir.

- /android/app - android studio için android modül dizini. eclipse için android proje dizini.

- /android/app/build.gradle - dependencies kısmında compile('com.facebook.react') ile jar'lar direk gradle networkünden indirilmektedir. yine dependencies kısmında "compile project(':react-native-code-push')" satırları mevcut. bu projeler bi üst dizindeki settings.gradle'da workspace'e tanıtılmıştı.

- /android/app/src/main/java - java kodları

- /android/app/src/main/AndroidManifest.xml

- /android/app/build/.../index.android.bundle - bütün /app dizinindeki js dosyaları burada tek dosya haline getiriliyor ve apk içine taşınıyor. android runtime'da bu dosyayı okuyor. 

- /ios

- /app - react javascript kodları ve kaynak dosyaları

- /index.android.js - android projesinde başlayacak olan ilk sayfa (yeni react init'le gelmiyor)

- /index.ios.js - ios projesinde başlayacak olan ilk sayfa (yeni react init'le gelmiyor)

- /index.js - ilk açılacak dosya

- /package.json - libs

- /node_modules - npm yöneticisinin dosyaları

# create-react-app

https://github.com/facebook/create-react-app reposundan dağıtılan, bir reactjs projesi oluşturup ayağa kaldırmamızı yarayan node-modülüdür. proje ilk kurulduğunda sadece 3 bağımlılık vardır:

```json
//package.json file
"dependencies": {
  "react": "^16.11.0",
  "react-dom": "^16.11.0",
  "react-scripts": "3.2.0"
},
"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test",
  "eject": "react-scripts eject"
},
```

react-scripts modülü diğer tüm işleyişi halledecek script'lerimizi içerir: node server'ı kaldırabilir ve dışarıya projemizi web tarayıcısı üzerinden açabilir, projedeki değişiklikleri otomatik takip edip hot reloading yapabilir, ES kodlarını eski sürümlere göre build eder, productione ve dev ortamlarına göre derleme yapabilir...

# https://github.com/react-boilerplate/react-boilerplate
bu boilerplate hem create-react-app'in yaptığını yapıyor hemde proje içindeki dosyaları da oluşturuyor. her component, container, redux ve saga, i18n için gerekli tüm dosyaları da oluşturuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# watchman
facebook'un geliştirdiği bir yazılım. bir dosya dizini veriliyor, bu dosya dizinlerini sürekli takip ediyor eğer bir değişiklik olursa, önceden belirlenen komutları tetikliyor.

executable dosyası, ek olarak fb-watchman ismi ile nodejs paketi olarak dağıtılıyor. böylece javascipt api'si ile de kullanılabiliyor.

genelde web uygulamarında developement yaparken bu uygulamadan çok yararlanılıyor. live-reloading işlemi yapılması sağlanıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# modernizr
bu js libary'si ile hangi teknolojilerin tarayıcıda varolup olmadığı kontrol edilebiliyor. asagidaki örnekteki gibi tüm teknolojiler tek bir variable'da kontrol edilebiliyor. örnek:

```js
if(modernizr.cookies){

  //cookies enabled
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Javascript language

# null vs undefined
farklıdır. undefined hiç değer almamış anlamına gelir. örneğin;

```js
var a1;

var a2 = null;

typeof a1; //prints undefined

typeof a9; //prints undefined. a9 is not even typed before.

typeof a2; //prints null;
```

# data types
- javascriptte number, boolean ve string birer primitive type'tır.

- fonksiyonlar (aşağıda detaylı anlatım var), typeof operatörü ile bakıldığında "fonksiyon" döndürürler. bu tipte olmalarının sebebi daha hiç new instance'ı ile oluşturulmamış olmasıdır.

- diğer data tiplerinin tümü (null dahil) objedir. array'ler objeden türemiş birer objedir. dolayısı ile objedir.

# Accessing Object Properties
- objectName.propertyName

- objectName["propertyName"]

# global vs local variable
local değerler metodların sadece içinden erişilebilir. oysa global değerler türm program tarafından erişilebilir. bir metod içerisinde "var" ile variable tabımlaması yapılmadan bir dataya değer atanırsa; o değer otomatik olarak global olarak tanımlanır. fakat böyle bir şeyin yapılması kesinlikle tavsiye edilmez.

# === vs ==
- == karşışaltırılan değerlerin eşit olup olmadığına (valueOf() metodu ile) bakarken, === ek olarak tiplerinde eşit olup olmadığına bakar.

- zorunlu kalmadıkça == kontrolü yapmamak gerekli. mantığı biliyor olsakta, başkasının okumasını zorlaştırıyoruz.

- === kontrolü daha doğal ve basit. javascript bilgisi dahi gerektirmiyor. tek istisna NaN'da oluyor. NaN === NaN -> false dönüyor.

- == için kurallar sırasıyla işletilmelidir:

  1- aynı tiptelerse === karşılaştırması yapılıyor. farklı tipler için aşağıdaki kurallar geçerlidir:

  2- null ve undefined eşittir

  3- sayı ve string karşılaştırılınca string önce sayıya çevrilir. js içinde Number(string) metodu mevcut.

  4- en az bir operand boolean ise; true->1, false->0'ya çevirir ve tüm kuralları tekrar işletir.

  5- obje karşısında string yada number varsa, obje.valueOF çağrılır ve kuraller tekrar işletilir.

  6- diğer tüm durumlar false döner

# NaN
__Not-A-Number__ anlamına gelmektedir. isNaN gibi metodlar bazen beklenmedik sonuçlar verebilir. örneğin " " değeri 0 a eşit sayıldığından isNaN false döner.

NaN bilgisayar bilimlerinde bir sayıdır. örneğin 0/0 tanımlanamaz ve bu sebeple NaN döndürmelidir. bu sebeple javascriptte typeof ile bakarsak; NaN, "number" tipindedir. 

JS beklenmedik sonuçlar dönebilmektedir. birçok alışılmadık durumları buradan inceleyebiliriz: jsfuck.com 

# debugger; keyword
bu keywordü içeren satırlar debug modda olan IDE tarafından durdurulurlar.

# "use strict"
bu satır ile javascript dilinin derlenmesi daha detaylı ve daha temiz olmasını sağlar. örneğin global değerler yaratılmasını engeller.

bu satır bir metodun içine yazılırsa sadece metod için geçerli olmaktadır. eğer globale yazılırsa tüm sistemi etkiler.

# Regular expressions
objeden türemişlerdir. javascriptte özel syntaxı vardır. tırnak içerisine almaya gerek yoktur.

# Functions
js'te class yoktur (ecmascript 6 ile sonradan geldi). sınıf olmamasına rağmen nesneye tabanlı bir dildir. "nesne" yerine "prototip" kavramı vardır. fonksiyon yaratılır. bu fonksiyonlar çalıştırıldığında bir obje döner. bu dönen obje, o fonksiyonun return edilen elemanıdır fakat aynı zamanda içerde "this" ile tanımlanan değerlere de erişmemize aracı olur. yani aslında dönen nesne arkasında bizim kullanamadığımız (erişemediğimiz) bir çok variable mevcuttur.

asagidaki person fonksiyonu bize objeyi döndürmek için kullanılan fonksiyondur. nesne döndürdüğü için constructor da denir.

```js
function person(first) {

    this.firstName = first;

    this.lastName = null;

    this.changeLastName = function (name) {

        this.lastName = name;
    };
}

var myFather = new person("John");

print(myFather.firstName);
```

new ile yukarıda yeni instance tanımladık. aynı şeyi new'siz yapsaydık yine person metodu içindekiler çalıştırılacaktı, fakat; person içindeki this objesi, yeni bir level'da (scope'ta) değil, "window" objesinde çalıştırılacaktı. yani;

```js
var myMother = person("Esra");

print(myMother); //prints undefined

print(window.firstName);//prints Esra
```

Asagidaki (nesne2) tanımlama ile fonksiyonsuz bir şekilde obje türetmiş oluyoruz.

```js
var nesne2 = {

    Ad: 'Melih',

    Soyad: 'Orhan',

    AdSoyad: function () {

        return this.Ad + ' ' + this.Soyad
    }
}
```

Benzer şekilde şu metod ile de nesne tanımlayabiliriz:

```js
var obj1 = Object.create(Object.prototype, {n:{value:1}, b:{value:true}});
```

Yukarıdaki satırda, Object.prototype yazılmasının sebebi; türeteceğimiz nesnenin, objeden türeyecek olmasıdır.

# descriptor
her property'nin descriptor'ları vardır. bir obje içine bir property eklediğimizde js'in aşağıdaki metodunu kullanabiliriz. bu metod üzerinden gidersek descriptor'ları daha iyi anlayabiliriz.

> Object.defineProperty(obj, prop, descriptor)

örnek:

```js
Object.defineProperty(myObject, 'myInteger', {
  value: 99, //myInteger'nin değeri
  writable: true, // myInteger = 1; ile değiştirilip değiştirilemeyeceği
  enumerable: true, // for-loop ile dönüldüğünde bu değerin de döngüye girip girmeyeceği
  configurable: true // property'nin silinip silinmeyeceği veya tipinin değiştirilip değiştirilmeyeceği
  get: function() { return val; },
  set: function(newValue) { val = newValue; }
});
```

Artık myObject.myInteger bize 99 u verecektir.

Dikkat edilirse defineProperty'nin aldığı tüm json elementine "descriptor" deniliyor. yani her biri birer descriptor değil.

ES2017 ile getOwnPropertyDescriptors metodu gelmiştir (başka yerde anlatılıyor).

# own-property
bir nesnenin süper variable'ları bu kümeye dahil değildir. __hasOwnProperty("propertyToCheck")__ ile sorgulayabiliriz.

# closure (yada lexical closure yada function closure)
closure türkçe kelime anlamı: kapatma

```js
var add = (function () {

    var counter = 0;

    return function () {return counter += 1;}

})();
```

her add() çağrışında counter değeri 1 artar. çünkü kendini çağıran isimsiz metod yine bir isimsiz metod döndürüyor. fakat bu dönen metod execute edilmiyor. biz her add() metodunu çağırdığımızda bunu execute etmiş oluyoruz: function () {return counter += 1;}

burada counter değeri global bir değer değildir. anonym bir metodun içinde olan bir metoddur. yokolmamaktadır.

closure ile API'lerin private metodlar ve değerler içerebilmesi sağlanmaktadır.

resmi olarak closure'un ne olduğu net diil. bazıları: return edilen metod'un önceki metoddaki variable'lara hala erişebilme tekniğine "Closure" denirken, bazıları ise return edilen fonksiyona closure adını veriyor. 

# fonksiyon ve obje içinde gelen property ve metodlar

#### fonksiyon properties:

- __lenght__

  fonksiyon kaç parametreli olduğunu tutar.

  ```js
  function func2(a, b) {}
  console.log(func2.length);
  // output: 2
  ```

- __arguments__

  (deprecated or not standart) fonksiyon çağrılınca ona yollanan argümanlar dizi olarak tutulur. bu argümanlar bu property'de tutulur.

- __arity__

  (deprecated or not standart) lenght ile aynı görevi görür. fakat lenght standartlarda oldugundan lenght kullanılmalıdır.

- __caller__

  (deprecated or not standart) fonksiyon o anda çağıran fonksiyon referansı.

- __display name__

  (deprecated or not standart) default olarak metodun kendi ismidir, fakat developer bunu istediği gibi ezebilir.

- __name__

  fonksiyon adının string halini tutar.

- __prototype__

  tipi json objesidir. array değildir. js objesi olduğundan saf js syntax'ında olduğu gibi buraya bir çok değer atayabiliriz:

  > myFunction.prototype.newProp = 99;
  
  prototype objesi, ilgili fonksiyondan üretilecek veya daha önceden üretilmiş her instance'a "__\_\_proto\_\___" properti'si ismi ile referans geçilir. bu sebeple buraya atadığımız her değer diğer eski ve yeni yaratılacak her instance'ın \_\_proto\_\_'sunda vardır.
  
  Aynı zamanda önceden veya sonradan oluşturulan instance'lara prop olarak kopyalanır. örnek:

  ```js
  myFunction.prototype.newProp = 99;

  instanceOfFunc.abc; // 99

  instanceOfFunc.__proto__.abc; // 99 

  instanceOfFunc.prototype // function'dan üretilen instance'ın/objenin prototype'si olmaz.
  ```

  prototype ile yeni prop eklemek çok kolaydır. farkat varolan prop'u güncellemek daha karmalıktır. zira instance'ı üretmek için kullanılan constructor, zaten prop'u override edecektir.

#### fonksiyon metods:

- __call(thisArg, arg1, arg2 ...)__

  ilgili fonksiyonu execute eder. args ile execute ederken göndereceğimiz parametreleri bir dizide göndeririz. thisArgs ise, fonksiyonun içindeki "this" scope'unu ezmek için kullanırız.

- __apply(thisArg, args)__

  "call" ile aynı görevi görür. sadece parametre alışı farklıdır. args bir dizi olmak zorundadır.

- __bind(thisArg, arg1, arg2 ...)__

  "call" ile aynı görevi görür. tek farkı; "bind", metodu execute etmez. ona parametreleri geçer. metod farklı satırda execute edilebilir. eğer argümanlar(args1, args2...) yollanmazsa; fonksiyon bir sonraki kod bloklarında execute edildiğinde de verilebilir. bind metodunda args'ler verilirse; ilerde execute ederken vermek gerekmez.

- __isGenerator()__

  (deprecated or not standart) fonksiyon içerisinde "yield" keywordu bulunup bulunmadığını döndürür. (yield başka başlıkta anlatılıyor)

- __toSource()__

  (deprecated or not standart) fonksiyon içindeki kod bloğunu string olarak döndürür.

- __toString()__

  toSource() ile aynı görevi görür. fakat tostring daha standarttır.

#### objelerin içerdiği fonksiyonlar:

- __valueOf()__

  o objenin primitive değerini döndürmelidir. == işaretinin yaptığı karşılaştırma için kullanılır. bu sebeple unique bir primitive döndürmelidir. valueOf() metodu objenin tipinden bağımsız bir eşitlik kontrolüdür. (başka başlıkta == vs === anlatılıyor)

  istisna olarak bir array'in valuof'u array'in kendisini döner.

  eğer valueof, developer tarafından hiç override edilmediyse json objesi döner. bu json objesinde ne olduğuna örnek üzerinden bakarsak:

  ```js
  function person(first) {

      this.firstName = first;

      this.lastName = null;

      this.changeLastName = function (name) {

          this.lastName = name;
      };
  }

  var myFather = new person("John");

  print(myFather.firstName);
  // output:
  // {
  //   firstName: John,
  //   lastName: null,
  //   changeLastName = function (name) {
  //         this.lastName = name;
  //   },
  //   __proto__: {}
  // }
  ```

# accessor property
Özel bir syntax'tır. "get a()" satırı, "object.a" dediğimizde döndürülecek degeri belirler. benzer şekilde "set a(v)" satırı, onu set ettiğimizde çalıştırılacak metodu çalıştırır. örnek:

```js
var o = {

  n: 1,

  get a(){ return this.n; },

  set a(v){ this.n = v*v; }
};

o.a // prints 1

o.a = 4;  //set ettğimiz satır

o.a // prints 16
```

# multi equal character on same line
```
var a = b = c;
a = b = c;
z = a = b = c;
```
gibi. durumlarda; c sol taraftaki her yere atanır.

# js'te bir syntax örneği

```js
(  function(name) {
              console.log("hello " + name)
}) ("ahmet")
```

Burada, "name" parametresi alan anonim bir fonksiyon tanımlanıyor. Bu fonksiyonu oluşturuyoruz (parantez içerisinde) ve hemen yanında metod çağırdığımızda yaptığımız gibi parantez içinde parametrelerini veriyoruz ve anonim fonksiyonu execute ediyoruz. sonuç olarak console'da "hello ahmet" çıktısını alıyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# amd (Asynchronous Module Definition) vs commonjs
js dosyaları yazıldığında ve daha sonra kullanılabilir hale getirilmesi için belli bir formatta yazılması gerekmektedir. bu formatların standartlarıdır.

amd ve commonjs en çok tercih edilen standartlardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# require.js
javascript kütüphanesi. sadece ilgili js modüllerinin yüklenmesini sağlamaktadır. yüklenen modüllerin bağımlılıkları varsa onları da otomatik yüklemektedir.

requirejs(['app/main']); ile istediğimiz modülün yüklenmesini sağlıyoruz.

modül tanımlamasını şu şekilde yapıyoruz:

```js
define("alpha", ["credits","products"], function(credits,products) {

        //my module code is here

});
```

bağımlılık olmadığı durumlarda define ikinci aldığı dizi parametresini hiç almayabilir. alpha ise bu tanımlayacağımız modülün id'si. biri dependency ile bu id ile cagrıyor bizim modülü.

require("myLib") ile bize "myLib" id'li kütüphaneyi dönüyor. bu objeyi kullanarak; içiden bir metod çağrabiliriz:

require("myLib").myMethod();

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# libary oluştuturken genelde yapılan tanımlamalar:

```js
if (typeof define === 'function' && define['amd']) {

     define(['jquery'], factory);

} else {

.....
```

Burada "factory" takip edilirse; API'nin (libnarynin) olduğu kodu olduğu görülür.

"define" bir fonksiyon mu diye kontrol edilmiş. "define" requeire.js'in modül tanıtlatmak için public sunduğu bir metod'dur. eğer bu varsa require.js vardır demek oluyor. require.js var ise; define['amd'] ile define içerisinde amd desteğinini olup olmadığına bakılır.

define['amd'] = define.amd dir.

amd desteği olmadığına bakılıyor. eğer oda var ise if bloğuna giriliyor. require.js ile yeni bir modül yaratıyor. 'jquery' dependency'sini alacak şekilde yeni modülümüz tanımlanıyor. else durumlarda require.js yokken yapılacaklar yazılıyor.

- örnek

```js
(function (global) {

    'use strict';

    var MyModule = function () {

      /* Your awesome module logic here… */

    };
    
    // …and here

    MyModule.foo = 'bar';

    // AMD support

    if (typeof define === 'function' && define.amd) {

        define(function () { return MyModule; });

    // CommonJS and Node.js module support.

    } else if (typeof exports !== 'undefined') {

        // Support Node.js specific `module.exports` (which can be a function)

        if (typeof module !== 'undefined' && module.exports) {

            exports = module.exports = MyModule;

        }

        // But always support CommonJS module 1.1.1 spec (`exports` cannot be a function)

        exports.MyModule = MyModule;

    } else {

        //eger amd, nodejs, commonjs yoksa windows.mymodule ile erişilebilmesi için bu işlem yapılıyor.

        global.MyModule = MyModule;
    }
})(this);
```

sondaki this ile windows objesi yakalanmış oluyor. kendini execute eden metod "global" paramteresi ile this'i (window'u) alıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# web page lifecycle

- #### events

  - > $(document).ready( func );

    readystate interactive olduğunda bu metodu jquery çalıştırmaktadır. (readystate asagida anlatiliyor)

  - > document.addEventListener("DOMContentLoaded", func);

    executes when HTML-Document is loaded and DOM is ready to run any javascript safely.

    it waits to execute all internal inline javascripts (script tags) and also external src javascript documents. only exeption is "async" and "defer" attributes of external scripts.

    browser'ların autofill-form(otomatik form tamamlmama) DOMContentLoaded eventini beklemektedir.

    DOMContentLoaded eğer sayfa yüklendikten sonra handler'a set edilirse hiçbir zaman execute edilmez.

  - > $(window).load( func ); yada window.onload = func;

    iframelerin içi yüklendiğinde, resimler tamamen indirildiğinde bu metod çağrılır

  - > window.onunload = func;

    kullanıcı sekmeyi kapattığı zaman

  - > window.onbeforeunload = func;

    kullanıcı sekmeyi kapatmak istediği zaman. burada tek seçeneğimiz string döndürmek:

    ```js
    window.onbeforeunload = function() {

        return "There are unsaved changes. Leave now?";
    };
    ```

    bu stringi tarayıcı bir dialog'da gösteriyor ve evet hayır sçeneğini sunuyor. eveti seçerse sekme kapatılıyor. hayırı seçerse kapatılmıyor.


- #### readystate

DOMContentLoaded sadece readystate loading durumundayken handlera attach edilebilir.

```js
if (document.readyState == 'loading') {

  document.addEventListener('DOMContentLoaded', func);
}
```

doument readyState's:

> "loading" – the document is loading.

> "interactive" – the document was fully read.

> "complete" – the document was fully read and all resources (like images) are loaded too.

readyste'in değiştiği event'i de yakalayabiliriz:

```js
document.addEventListener('readystatechange', func );
```

- #### eventler ile gili notlar
  - asagidaki iki metod aynı şeyi yapar:

    - $(window).load( func );

    - $(window).on('load', func )

    Yukarıdaki load metodu document'te alabilir, button da alabilir.

  - asagdaki iki code ayni anlama geliyor. jquery sık kullanıldığı için bu şekilde ayarlamis.

    - $( document ).ready( func );

    - $( func );

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# $ prefix on variables
javascript kodlarında çoğu zaman jquery'nin namespae'i olan $ işareti görülür. bunun sebebi oluşturulan variable'ın jqueryden dönen bir değeri tuttuğunu belli etmekttir. örneğin;

var a = 3;
var $myObject = $("#email");

buna benzer bir durum birçok diğer kütüphaneyi kullanan programcıların yaptığı görülebilir. örneğin; underscore js kullananlar   
\_myVariable diye adlandırırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# namespace

javascript'te en global değerler namespace olarak kullanılır. örneğin bir kütüphane eklediğimizde kendini buraya bir tanımlama yaparak ekler. bu şekilde herkes buradan onu çağırır. bu sebeple javascriptte buradaki değerler namespace dendir.

genelde bu tanımlama ile init yapılır:

var MAYAPPLICATION = MYAPPLICATION || {};

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# yield keyword

yield türkçe kelime anlamı: esneme.

fonksiyona kalındığı yerden devam edilebilmesini sağlar. yield return eder, fakat bir "Generator" objesi döndürür. "function \*" ibaresindeki yıldız işareti fonksiyonun bir generator objesi döndürdüğünü temsil eder. örnek:

```js
function* foo(x) {

    while (true) {

        x = x # 2;
        yield x;
    }
}

var g = foo(2);

g.next(); // -> 4

g.next(); // -> 8

g.next(); // -> 16
```

# yield*
yield içindekileri bir üstteki metodun generator'ı ile birleştirmek istersek yield# kullanırız. örnek:

```js
function* g1() {

  yield 2;

  yield 3;

  yield 4;
}

function* g2() {

  yield 1;

  yield* g1();

  yield 5;
}

var iterator = g2();

console.log(iterator.next()); // {value: 1, done: false}

console.log(iterator.next()); // {value: 2, done: false}

console.log(iterator.next()); // {value: 3, done: false}

console.log(iterator.next()); // {value: 4, done: false}

console.log(iterator.next()); // {value: 5, done: false}

console.log(iterator.next()); // {value: undefined, done: true}
```

# yield ile hiçbir şey döndürülmediğinde ne olur?
yield keywordunun yanında hiçbir şey olmadığında, örnek:

```js
function* g1() {

  yield;
}
```

iterator şunu döner: {value: undefined, done: true};

# generator nasıl done=true olduğunu anlıyor?
eğer yield'dan sonra herhangi bir kod satırı var ise; done:false oluyor.

# var iterator = g2();
g2 bir genrator donen obje ise; "var iterator" atık genrator objesidir ve "g2()" kodunda parentez açılmasına rağmen; hiçbir kod execute edilmez. g2'nin ilk satırları dahi ancak iterator.next() dediğimiz zaman çağrılır.

# yield kullanılan fonksiyonlarda return keywordü
return de iterator next metodunda yield gibi değer dönüdürür. yield ile apaynı davranışı sergiler. fakat her zaman done:true'dur. bundan sonra itetrator devam etmez. tek fark budur.

return değeri aşağıdaki gibi değer atama görevi görmektedir.

```js
function# g1() {

  yield 2;
  return 3;
}

function# g2() {

  yield 1;
  var a = yield# g1();
  console.log(a);
  yield 4;
}

var iterator = g2();

console.log(iterator.next()); // {value: 1, done: false}

console.log(iterator.next()); // {value: 2, done: false}

console.log(iterator.next()); // {value: 3, done: false}

3 //console.log(a) çıktısı

console.log(iterator.next()); // {value: 4, done: false}

console.log(iterator.next()); // {value: undefined, done: true}
```

Yukarıda "var a = yield# g1();" daki satırda # karakteri hiç kullanılmasaydı:

var a = yield g1();

a değerine hiçbir değer atanmayacaktı. generator'ımız;

{value: 2, done: false}

yerine;

{value: Generator, done: false} 

dönecekti. ve a değeri undefined olacağından console.log undefined basacaktı. fakat; next(); 'e yollayacağımız ilk parametre a'ya atanıyor olacaktı. aşağıda direk örnekle gidelim:

var a = yield g1(); satırını işleten next() metodundan sonraki next() metoduna paramtre geçseydik: next(99); o zaman "var a" değerimiz 99 olacaktı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# noConflict metodları
javascriptte namespaceler diğer dillerdeki gibi düzenli yönetilemeyebiliyor. karışıklık olmaması için her kütüphane en tepede noConflict isimli bir metod tanımlıyor. örneğin jquery'de bu metod şunu yapıyor.

```js
var j = jQuery.noConflict();
j( "div p" ).hide();
$( "content" ).style.display = "none";
```

Yukarıda noConflict metoduna true parametresi geçilseydi artık 3 üncü satırda $ işareti de kullanılamaz hale gelecekti. noConflict(true)'yu, $ işaretini başka bir kütüphane kullanıyor ise yapabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# arrays

- #### obje'den türemiştir.

var cars = ["Saab", "Volvo", "BMW"];

var cars = new Array("Saab", "Volvo", "BMW");//array fonksiyonu.

new Array tercih edilmemeli, çünkü yanlış anlaşılma olabilir. zira constructor tek parametre ve sayı alınca boş array oluşturuyor:

new Array(20); //20'lik bir dizi oluştu. bazıları bunu 1 elemanlı bir dizi olduğunu düşünebilir.

- #### erişmek için:

> cars[1];

javascript'te arrrayler birer objedir. aslıda her dizi elemanı bir propertie'dir. sadece access'i farklıdır. bunun sebebi:

herhangi bir objede 0 veya 1 isimli propertie'ye şu şekilde erişebiliyoruz:

> myArr["0"]

yada

> myArr[0]

Sayı vererek eriştiğimizde aslında; javascript interpreter'ı onu string'e çeviriyor. Bu sebeple myArr["0"] değeri myArr[0] ile aynı şeyi veriyor.

Yani bu erişim arraylere özgü bir durum değildir. arraylarin içindeki metodlar otomatik propertieleri sayısal cinsten tutmaktadır. örneğin "jquery objesi" (konu başka başlıkta anlatılıyor) array dönmüyor fakat liste içinden bir elemana erişmek istediğimizde; yine index ile erişebiliyoruz.

- #### array'in hazır metodları:

- cars.length;

- cars.sort(); default sorts alphabetically.

  sort; isteğe bağlı olarak bir parametre olarak fonksiyon alıyor. örnek:

  cars.sort(function(a, b){

    return (b - a);
  });

  negatif deger dönerse a, b den küçük demektir. tüm dizinin elemanları birbiri ile karşılaştırıyor.

- fruits.pop(); //removes last element

- fruits.push("Kiwi"); //adds element to last index and return the last new index

- fruits.shift(); // removes first element

- fruits.unshift("Lemon"); // adds element to first index

- fruits.splice(2, 4, "Lemon", "Kiwi"); //2'inci elemandan itibaren, 4 tane eleman siliyor (silinen elemanların indexleri: 3,4,5,6) + 2'inci indexten itibaren sağ tarafta olan bütün argümanları; "lemon" ve "kiwi"'yi diziye ekliyor ekliyor. örnek:

   var fruits = ["1", "2", "3", "4"];

   fruits.splice(2, 0, "x", "y");

   prints(fruits); // prints 1,2,x,y,3,4

- arr1.concat(arr2, arr3);

  sırası ile birleştiriyor. sonsuz parametre kabul ediyor.

- fruits.slice(1,3); returns new array from 1-3 index

# For loop comparison table

| how to call                                                      | change scope/context | iterate over objects | how to break                              | access all array list | change orjinal variable | iterate over empty slots |
|------------------------------------------------------------------|----------------------|----------------------|-------------------------------------------|-----------------------|-------------------------|--------------------------|
| lodash.forEach(arr,   function( index, val )); (or)  lodash.each | false                | true                 | return  false/null/undefined  on callback | false                 | true (read NOT-A)       | true                     |
| jQuery.each( arr,  function( index, val ));                      | false                | true                 | return false/null/undefined on callback   | false                 | true (read NOT-A)       | true                     |
| for(val in arr) { ... }                                          | true                 | true                 | break keyword                             | true                  | true                    | false                    |
| for(i=0; i<arr.lenght; i++) { ... }                              | true                 | false                | break keyword                             | true                  | true                    | true                     |
| underscore.each(arr,function(val, index, arr),context)           | true                 | true                 | not possible                              | true                  | true (read NOT-A)       | true                     |
| underscore.every(arr,function(val, index, arr),context)          | true                 | true                 | return false/null/undefined on callback   | true                  | true (read NOT-A)       | true                     |
| arr.forEach(function(val, index, arr), context);                 | true                 | false                | not possible                              | true                  | true (read NOT-A)       | false                    |

NOT-A:

orjinal elemanı callback'e geçiyor. dolayısı ile değişkenler burada değiştirilebilirler. fakat genel javascript kuralları gereği bilinmesi gerekenler var. callback metodu yeni bir fonksiyon olacağı için geçilen değerler 2 durumda değiştirilemezler:

1- eğer yeni bir obje ataması yapılırsa (çünkü yeni objenin referansı farklı. orjinal  obje'nin içeriği değiştirilmemiş olacak. ve orjinal dizi; referansla elemanları tutuyor.

2- değiştirilen eleman bir primitive (string, int...) ise. çünkü primitive'ler metodlara atanırken sadece değerleri geçer (referansları değil).

Bu iki durum için geçici olarak "access all array list" kuralı true ise; orjinal objenin içindeki referans manuel değiştirilir.


- #### Tablo hakkında

- lodash.forEachRight ve lodash.eachRight aynı metodlardır. lodash.each ile aynı özelliklere sahipler. tek farkı liste elemanlarının sırasını tersten dönmesidir.

- "iterate over empty slots" kuralı:

   boş slotlar (hiç atanmamış değerler) için callback çağrılıyor dönmüyor.

   örnek:

   var a = ['a0'];

   a[3] = 'a3';

   a[4] = undefined;

   a[5] = null;

   sadece a[1] ve a[2] boş. bunlar için for loop callbak çağrılması.

   eğer boş olup olmadığı kütüphane içinde kontrol ediliyorsa bu yavaşlığa sebep olmaktadır.

- "iterate over object" kuralı:

   eğer list bir js objesi ise, yine döngü çalışıyor ve her property'si dönmeye başlıyor. 

   method'un aldıgı parametreler'in anlamı değişiyor:

   list array ise; (value, index)

   list obje ise; (value, key)

- elle for döngüsünün yazılması ( for(i++) şeklinde ) diğer tüm döngülerden çok daha performanslı çalışmaktadır.

- $.each() metodu ile $(selector).each() aynı değildir. $(selector).each() bir jquery objesinin içerisinde dönmeye yarıyor.

- çoğu kütüphanenin "map" metodları da vardır. bu metodlar for metodları ile aynı parametreleri alır. map metodları callback metodlarından return edilen her elemanı yeni bir diziye atara. bu yeni diziyi de map metodu return eder. map metodunun temel felesefesi budur. for yerine map yada map yerine for kullanmak anti-pattern'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# class

ecmascript'in güncel standartlarında class'lar vardır. fakat geriye uyumluluk adına ve javascript kültüründe olmadığından pek kullanılmıyorlar. class yapıları fonksiyonlara çok benzemektedir. zaten bu sebeple, fonksiyonlara javascriptte bazı insanlar "class" demektedir (yanlış fakat öyle kullanalar var). fonksiyon yapısının sınıfa benzediğini gösteren bir örnek:

```js
var myFunctionName function() {

        var myPrivateVar;

        this.myPublicMethod1(){

            return null;
        }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ecmascript vs javascript vs jscript vs ActionScript
javascript ve jscript vs ActionScript; birer ecmascript implementasyonudur. buna rağmen, bazı implementasyonlar kendilerince spesifikasyon dışı ek özellikler içerebilirler.

# CoffeeScript vs typescript
javascript'e çevrilen bir programlama dilleridir.

# source-to-source compiler (yada transcompiler yada transpiler)
Bir dil, derlenmeden önce başka bir dile çevrilebilir. Bu işlemi yapan çeviriciye transpiler denir.

# typescript version history

aşaıdaki sürümler arasında minor versiyonlarda mevcuttur.

| version | release date      | important changes                                                                                                                                      |
|---------|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.1     | 6 October 2014    | performance improvements                                                                                                                               |
| 1.3     | 12 November 2014  | protected modifier, tuple types                                                                                                                        |
| 1.4     | 20 January 2015   | union types, let and const declarations, template strings, type guards, type aliases                                                                   |
| 1.5     | 20 July 2015      | ES6 modules, namespace keyword, for..of support, decorators                                                                                            |
| 1.6     | 16 September 2015 | JSX support, intersection types, local type declarations, abstract classes and methods, user-defined type guard functions                              |
| 1.7     | 30 November 2015  | async and await support,                                                                                                                               |
| 1.8     | 22 February 2016  | constraints generics, control flow analysis errors, string literal types, allowJs                                                                      |
| 2.0     | 22 September 2016 | null- and undefined-aware types, control flow based type analysis, discriminated union types, never type, readonly keyword, type of this for functions |
| 2.1     | 8 November 2016   | keyof and lookup types, mapped types, object spread and rest,                                                                                          |
| 2.2     | 22 February 2017  | mix-in classes, object type,                                                                                                                           |
| 2.3     | 27 April 2017     | async iteration, generic parameter defaults, strict option                                                                                             |
| 2.4     | 27 June 2017      | dynamic import expressions, string enums, improved inference for generics, strict contravariance for callback parameters                               |
| 2.5     | 31 August 2017    | optional catch clause variables                                                                                                                        |
| 2.6     | 31 October 2017   | strict function types                                                                                                                                  |
| 2.7     | 31 January 2018   | constant-named properties, fixed length tuples                                                                                                         |
| 2.8     | 27 March 2018     | conditional types, improved keyof with intersection types                                                                                              |
| 2.9     | 14 May 2018       | support for symbols and numeric literals in keyof and mapped object types                                                                              |
| 3.0     | 30 July 2018      | project references, extracting and spreading parameter lists with tuples                                                                               |
| 3.1     | 27 September 2018 | mappable tuple and array types                                                                                                                         |
| 3.2     | 30 November 2018  | stricter checking for bind, call, and apply                                                                                                            |
| 3.3     | 31 January 2019   | relaxed rules on methods of union types, incremental builds for composite projects                                                                     |
| 3.4     | 29 March 2019     | faster incremental builds, type inference from generic functions, readonly modifier for arrays, const assertions, type-checking globalThis             |
| 3.5     | 29 May 2019       | faster incremental builds, omit helper type, improved excess property checks in union types, smarter union type checking                               |
| 3.6     | 28 August 2019    | Stricter generators, more accurate array spread, better unicode support for identifiers                                                                |

# ECMAScript
ECMA-262 ve ISO/IEC 16262 standartları ie oluşturulan betik dilidir.

# ECMAScript (yada ES) version history

| Name                                                     | Date Puplished |
|----------------------------------------------------------|----------------|
| ES5                                                      | 2009           |
| ES5.1                                                    | 2011           |
| ECMAScript 2015 (yada ES2015 yada ECMAScript 6 yada ES6) | 2015           |
| ECMAScript 2016 (yada ES2016 yada ECMAScript 7 yada ES7) | 2016           |
| ECMAScript 2017 (yada ES2017 yada ECMAScript 8 yada ES8) | 2017           |
| ECMAScript 2018 (yada ES2018 yada ECMAScript 9 yada ES9) | 2018           |

"ES.Next" ECMAScript'in br sonraki versiyonunu belirten özel bir takma addır.

Ecmascript ile javascript'in versiyonları aynıdır. Çünkü ecmascript'in referans implementasyonu javascript'tir.

Typescript'in herhangi bir sürümü, istenilenilen javascript'in herhangi bir sürümüne çevrilebilir. ancak istisna olarak bazı özellikler javascript'te hiç desteklenmiyor olabilir. böyle durumlarda, js tarafında sürüm yükseltmek gerekebilir.

# ecmascript implementasyonları standardize edilirken, her sürüm kendi içinde belli sürümlere ayrılır.

| sürüm kodu | sürüm adı | açıklama                                          |
|------------|-----------|---------------------------------------------------|
| stage-0    | Strawman  | just an idea, possible Babel plugin               |
| stage-1    | Proposal  | this is worth working on                          |
| stage-2    | Draft     | initial spec                                      |
| stage-3    | Candidate | complete spec and initial browser implementations |
| stage-4    | Finished  | will be added to the next yearly release          |

# babel
bir node modülü. bu mödül js sürümleri arası compile sağlıyor. bu şekilde güncel yazılan kodların eski tarayıcılarda çalışabilmesini sağlıyor.

babel ecmascript'te kullanılan tipleri ve flow'da kullanılan tipleri kaldırmaktadır. çünkü convert işlemi sonunda olan JS kodudur ve js'te tip yoktur.

babel config dosyaları:
- .babelrc
- package.json içindeki "babel" anahtarı altındaki ayarlar
- babel.config.js

# .babelrc json vs babel.config.js
.babelrc json iken, babel.config.js js dosyasıdır ve programatik olarak configure edilebilir. örnek:

.babelrc file:
```json
{
  "presets": [...],
  "plugins": [...]
}
```

babel.config.js file:
```js
module.exports = function (api) {
  api.cache(true);

  const presets = [ ... ];
  const plugins = [ ... ];

  return {
    presets,
    plugins
  };
}
```

babel için birçok node mudülü vardır:

- __@babel/core__

  JS API'si sunar. aşağıdaki gibi import edilip, js içerisinde code'u string olarak alıp kodu çevirebilir.

  ```js
  var babel = require("@babel/core");
  //yada
  import { transform } from "@babel/core";
  //yada
  import * as babel from "@babel/core";

  //convert code
  babel.transform(code, options, function(err, result) {
      result; // => { code, map, ast }
  });
  ```

- __@babel/cli__

  babel'in komut satırından kullanılabilmesi için gerekli node modülüdür.

# babel plugins

  babel'de tonlarca eklenti var. örneğin ecmascript'in her özelliği bir eklenti. bu şekilde convert işlmelerinde özellik özellik enable işlemi yapabiliriz.

  eklentiler 2 gruba ayrılıyor:

  - Transform Plugins

    Belli kod syntax'larını transform ediyor. örnekler:

    - @babel/plugin-transform-exponentiation-operator
    - @babel/plugin-transform-arrow-functions

  - Syntax Plugins

    Sadece parse işlemi yapıyorlar. transform yapamazlar.

  Genelde birçok plugin birarada kullanılır. bunlar gruplanmış şekilde hazır olarak dağıtılırlar. bunu bu şekilde babel config dosyamızda belirtebiliriz:

  ```json
  {
    "presets": ["es2015", "react", "stage-2"]
  }
  ```

  preset değilde, sadece plugin kullanmak istersek yine benzer config'i vardır:

  ```json
  {
    "plugins": ["transform-decorators-legacy", "transform-class-properties"]
  }
  ```

  Pluginler ve presetler aynı configde olabilir. çalışma sırası için kurallar vardır:
  
  - Plugins run before Presets.
  - Plugin ordering is first to last.
  - Preset ordering is reversed (last to first).

# @babel/preset-env
bir babel preset'idir. desteklenen tarayıcı bilgisini confg'e gireriz ve bu tarayıcı grubuna uyumlu kod üretir.

örnek kullanım:

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
                    "chrome": "58",
                    "ie": "11"
        }
      }
    ]
  ]
}

```

# Babel 6 "loose" mode

- sadece ES5'e çevirirken kullanılan bir mod'dur.

- babel uygulaması, ES6 kodunu ES5 koduna çevirirken, daha çok ES6'ya uygun bir şemantik kullanmaya gayret ederek bunu yapıyor.

- this mode normally is not recommended.

# ES2017 ile gelen bazı yenilikler

- #### async ve await keywordleri

"await" is verb type of "wait" word.

Promise (Ecmascript güncel sürümleri ile gelen nesne) dönen metodları "myMethod.than()" ile çağırmaktansa onları şu şekilde senkron çağırabiliyoruz:

var value = await myMethod();

artık yukarıdaki satır senkron işleyecektir. yukarıdaki satırı içeren metod her zaman async olmak zorunda. yani;

```js
async methodX() {
  ...
  var value = await myMethod();
  ...
  return anything;
}
```

ve artık herhangi bir yerden methodX çağrıldığında, methodX Promise dönecektir ve promise succes'i anything'dir. yani;

```js
methodX().then(  function(value){ 
                           console.log("value" + value); //prints anything
                        }
);
```

yukarıda eğer await metodu error verirse o satır exception throw eder.


- #### Object.values()

```js
const person = { name: 'Fred', age: 87 }
Object.values(person) // ['Fred', 87]
```

Array içinde aynı özellik geçerlidir.

- #### Object.values()

```js
const person = { name: 'Fred', age: 87 }
Object.entries(person) // [['name', 'Fred'], ['age', 87]]
```

Array içinde aynı özellik geçerlidir.

- #### Object.getOwnPropertyDescriptors()

setter işlemimizi ezmiş olalım:

```js
const person1 = {
    set name(newName) {
        console.log(newName)
    }
}
```

artık bu işimizi görmeyecektir:

```js
const person2 = {}
Object.assign(person2, person1)
```

fakat getOwnPropertyDescriptors metodu bize istediklerimizi döndürecektir.

```js
const person3 = {}
Object.defineProperties(person3, Object.getOwnPropertyDescriptors(person1))
```

# ES2016 ile gelen bazı yenilikler

- #### Exponentiation Operator

Math.pow(4, 2) == 4 ** 2

# ES2015 ile gelen bazı yenilikler

- #### Default Parameter Values

```js
function f (x, y = 7) { 

  return x + y;
}
```

- #### class yapıları (extend, constructor desteği ile)

- #### let operatörü. (Block-Scoped Variables)

let a=5;

let for gibi döngülerin içerisinde tanımlandığında örnek:

for(let i=0; ...) ...

for dışından okunamaz olur. oysa "var" okunabiliyordu. Aynı durum if içinde geçerli. if içinde let tanımlaması if dışında geçersiz oluyor.

aynı zamanda let ile tanımlanan değerler, aşağıdaki satırlarda tekrar tanımlanamıyor. hata fırlatıyor. oysa var operatöründe aynı değer tekrar tanımlanabiliyordu.

- #### const keyword

Constants kelimesinden geliyor. java'daki final'a denk geliyor.

```js
const a = 2;
```

- #### Template literals (yada Template strings)

  ` karakteri ile yazılan string'lerde bir çok özellik mevcut. tüm özellikler burada: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals (archive date: 16/11/2019) bunlardan bazıları:
  
  - multi-line strings

    ```js
    var myStr = `hello
    world`;
    ```

  - Expression interpolation

    ```js
    var myStr = `first element is: ${a} . but both element total are: ${a + b}`
    ```

- #### String format

```js
var soyad="Aykaç";

var fullName=`Merhaba ${soyad}`;
```

- ### Set liste türü

aynı elemanlar dublicate olmuyor. örnek:

```js
var items=new Set();
items.add(1);
```

- #### Iterators

- #### Maps

key/value tutan liste türü.

- #### WeakMap

Maps ile aynı. sadece enumerable diildir.

- #### is ve isnt operatörü

javadaki instanceOf operatörü ile aynı işi görüyor.

- #### Destructuring Assignment

aynı satırda çoklu değer atama işlemleri sağlanıyor. örnek:

var [a,b] = [9,3];

//eğer sağ tarafta fazladan eleman var ise, sadece sol tarafta istenmeyen eleman yerine boşluk atanabiliyor.

var list = [ 1, 2, 3 ]; 

var [ a, , b ] = list;

- #### sınırsız parametre

```js
function argsTester(...theArgs){

  alert(theArgs.length);
}
```

burada the args aynı tipte olan verileri tutmuyor. tüm objeleri bir dizide tutuyor.

- #### Expression Bodies

```js
odds  = evens.map(v => v + 1) 

pairs = evens.map(v => ({ even: v, odd: v + 1 })) 

nums  = evens.map((v, i) => v + i)
```

sırası ile aşağıdaki satırlara denk gelmektedir:

```js
odds  = evens.map(function (v) { return v + 1; });

pairs = evens.map(function (v) { return { even: v, odd: v + 1 }; });

nums  = evens.map(function (v, i) { return v + i; });
```

- #### parametre birleştirme

var params = [ "hello", true, 7 ]; 

var other = [ 1, 2, ...params ]; // [ 1, 2, "hello", true, 7 ]


- #### parantezsiz metod çağırma işlemi

ajax("google.com")

yerine artık:

ajax "google.com"

yazılabilir.

- #### direct support for octal and binary

```JS
0b111110111 // 503;

0o767 // 503;
```

- #### Property Shorthand

json değerleri sağ taraftaki ile aynı isme sahip ise atama yapmaya gerek kalmıyor.

```js
obj = { 

        x, 

        y };
```

eskiden:

```js
obj = { x: x, 

        y: y

      };
```

- #### module Value Export/Import

```js
//  lib/math.js 
export var pi = 3;

//  someApp.js
import # as math from "lib/math"; 
console.log(math.pi);

//  otherApp.js 
import { pi } from "lib/math"; 
console.log(pi);
```

- #### sınıf içindeki metodlara static operatörü geldi

- #### generators (yield keywordü)

- #### promise nesnesi geldi

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Browser Object Model (yada BOM)

html tarayıcılarının javascript'e ekledikleri javascript objeleridir. bu objeler nodejs gibi ortamlarda olmuyorlar.


## window

genel bir objedir. js'te global tanımlanan her değer buraya bağlanmaktadır. örnek metod:

> window.close()

## window.screen

fiziksel ekranla ilgili bilgiler içeren javascript objesidir. örnek:

> screen.width

> screen.height

## window.location

url ile ilgili metodlar buradadır.

> window.location.href 

## window.history

> history.back();

## window.navigator

tarayıcının user agent bilgilerini barındırır.

## document.cookie

cookie metodları buradadır.

## document

windows.document ile de aynı objeye erişilebiliyor. DOM ile ilgili metodlar buradadır. örnek:

> document.appendChild(element);

> document.write(text);

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# jquery object (yada jQuery collection object)

$( 'li' ); metodundan dönen obje, jquery objesi olarak adlandırılır. bu adlandırma jquery'nin kendi içinde belirlediği bir standartdır. yazılımcıların birbiri arasında anlaşabilmesi için konulan özel bir isimdir. javascript'te herşey ( $, JQuery dahi ) birer obje olduğundan "javascript objesi" ile isim karışıklığı yaşanabilmektedir.

jquery object, array objesi ile benzer propertie'ler içeriyor fakat Array yada Array'den türemiş bir obje değildir.

Örnek kullanımlar:

```js
var listItems = $( 'li' );
var rawListItem = listItems[0]; // or listItems.get( 0 );
$( '<p>Hello!</p>' ); //creates new element

//search inside a spesific block
var paragraph = $( 'p' );
$( 'a', paragraph );
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# javascript executes code in 3 phases

1- First, the engine walks through the code, looks for function declarations and hoists them (moves them to the top)

2- it hoists variable declarations

3- finally it runs the "normalized" code.

example:

```js
function bar() {

        return foo; // foo degerinin tanımı yok.

        foo = 10; //foo degerinin hala tanımı yok.

        function foo() {} //foo degerinini tanımlaması burda. yukarıda eksikti. bu sebeple kodun en başına alınacak.

        var foo = 11; //foo degerinin artik tanımlanması var. "var" keyword'üne artık gerek yok.
}
```

"normalized" code:

```js
function bar() {

    function foo() {} 

    return foo; //kod execute edildiğinde kod bu kısımda sonlanacaktır. aşağıdaki kodlar unused'tir.

    foo = 10;

    foo = 11;
}
```

############################

############################
# WEB
############################

############################

# Web Hypertext Application Technology Working Group (yada WHATWG)
https://github.com/whatwg reposunda geliştirilen web standartlarını dökümante edilen bir web projesi geliştiren topluluktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# WebAssembly (yada Wasm)
web tarayıcılarında sunucudan gönderilen c/c++ gibi native code'lar çalıştırılıyor. bu kod güvenlik amaçlı sandbox'ta çalıştırılıyor ve yetkileri çok kısıtlı. js gibi sadece belli kaynaklara erişebiliyor (cookies, DOM...).

örnek kod:

```java
var wasmModule = new WebAssembly.Module(wasmCode);
var wasmInstance = new WebAssembly.Instance(wasmModule, wasmImports);
wasmInstance.exports.factorial(10); //native taraftaki faktoriel alan fonksiyonumuzu çağırdık.
```

örneğin; client tarafta video editleme uygulaması yaptık. bu gibi durumlarda bu teknoloji performans'ı çok etkiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# postback

web sayfasında forma tıklanıp, aynı sayfaya istek yapılan istek postback işlemidir. "POSTed back" to same page teriminden gelir. genelde jsp, jsf, asp gibi platformlarda karşımıza çıkar, fakat teknolojiden bağımsızdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# REST Resource Naming
- URL bir identifier olduğu unutulmamalıdır
  
bu sebeple fiil içermemelidir. fiil içermemelidir.
örnek: 

> /api/message/3/delete/

yerine

> /api/message/3

  DELETE metodu desteği olmalıdır

- tekil kaynak çağırma
  
  /user-management/users/{id}
  /user-management/users/admin

- çoğul kaynak çağırma
  
  /device-management/managed-devices
  /user-management/users
  /user-management/users/{id}/accounts

  Tekil ve çoğul kaynaklarda "users" yada "user" yazılıp yazılmayacağı önemsizdir. fakat tüm servislerimizde ya çoğul yada tekil olan kelimeleri kullanmalıyız.

- fiil içerme durumları
  
  yapılacak işlem bir prosedürü başlatacak ise; fiil kullanılmak zorundayız. örnek:
  /cart-management/users/{id}/cart/checkout

- geçerli olmayan standartlar
  
  Google, facebook, dropbox, instagram gibi büyük servislerin api'leri aşağıdaki standartlar konusunda farklı yol izlemektedirler. Hatta aynı şirketin farklı servisleri farklı yöntemler kullanmaktadır.

  - okunabilirlik
    - userManagement
    - user-management
    - user-management
    - usermanagement

  - büyük küçük harf standartları

    rfc2616 standartlarına göre ( w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.2.3 ) büyük küçük harf hostname'e kadar önemsiz iken, path kısmında önemlidir.

  - versioning
    
    - With a timestamp, a release number
    - In the path, at the beginning or at the end of the URI
    - As a parameter of the request's body
    - In a HTTP Header
    - With an optional (example: /api/users/1?v=2 )

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# placeholder
türkçe kelime anlamı: yer tutucu
bazı html form elemenleri için bir attribute'dir. bu attribute ile son kullanıcı text girmemiş ise, placeholder value'sinde yazan string değeri ekranda gösterilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# carousel
Türkçede kelime anlamı: atlıkarınca
HTML'de sağ ve sola doğru oklar aracılığı ile resim galerisi gezintisi sağlayan component'e verilen özel isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Favicon
Favorites icon'un kısaltmasıdır. bu resim dosyası, sitenin sık kullanılanlara eklendiğinde, veya siteye gidildiğinde görünecek ufak simgesidir. html kodları içerisinde yer alır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# access inside html popup with pure (core) js

```js
var myWindow = window.open("", "myWindow", "width=200, height=100");
myWindow.document.write("hello I'm popup");

//or get the opener object
var opener = window.opener;

if(opener) {
    var oDom = opener.document;
    var elem = oDom.getElementById("elementId");
    if (elem) {
        var val = elem.value;
    }
}
```

html, aynı anda birden fazla popup desteklemektedir. fakat çoğu tarayıcı varsayılan olarak ikinci açılan popup'ları engellemektedir. bu sebeple açılması tavsiye edilmez.

# Backlink
Backlink, bir web sayfasından başka bir web sitesine giden köprü bağlantıya verilen isimdir. Bir web sitesine, diğer sitelerden verilen backlink ne kadar fazla olursa, bu web sitesi arama motorları açısından o denli popüler olur. burada önemli kriter: X sitesi, Y sitesine link veriyorsa, X'teki sayfanın içeriğinin Y'deki ile bağlantılı/alakalı olmasıdır. ilgisiz konulardaki linkler verimsiz olacağından arama motorlarındaki puanı olumsuz etkiler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# tampermonkey vs greasemonkey vs Violentmonkey

web tarayıcıları için birbirlerine alternatif eklentilerdir.eklentiler sayesinde son kullanıcı javascriptleri, istediği event (sadece standart javascript eventleri değil, ekstra eventler de kullanılabiliyor) sonrası çalıştırabiliyor. internette birçok hazır script paylaşılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# chart.js
web teknolojilernii kullanarak ekrana sadece chart çizmek için kullanılan js kütüphanesi.

# d3
web teknolojileri ile ekrana her türlü görüntü oluşturmak için kullanılan js kütüphanesi.

# jqplot
jQuery eklentisi. sadece chart oluşturmaya yarıyor.

# çizge (yada graph yada grafik yada tr:graf yada çizit)
elle yada araçla çizilen her türlü görüntüye grafik adı veriliyor.

"Graph" kelimesi tek başına bilişimde; bir veri yapısı çeşidini temsil eder (başka başlıkta anlatılıyor).

# chart
grafiğin bir alt kümesidir. sadece değişkenler arasındaki ilişkiyi göstermeye yarayan çizgisel anlatım şekli.

çok kullanılan bazı chart çeşitleri:

- Line chart (yada Çizelge): x-y ekseni üzerinde bir fonksiyonun gösterilmesidir.

- bar chart: birden fazla kalın çubukların yanyan dizilerek x-y ekseni üzerinde gösterilmesi. 

- pie chart (or a circle chart): pasta üzerinde oranların paylaştırıldığı grafik

- histogram: gruplandırılarak gösterilen bir "Bar chart" çeşididir. örnek olarak aşağıdaki datayı bar chart'ta gösterdiğimizi düşünelim:

| BOY (CM) GRUPLAR | KİŞİ SAYISI |
|------------------|-------------|
| 161-165          | 5           |
| 166-170          | 10          |
| 171-175          | 9           |

# diagram (yada tr:diyagram)
grafiğin bir alt kümesidir. sadece herhangi bir olayın değişimini gösteren grafik. UML diyagramları buna örnektir.

# Cartogram (yada kartogram)
grafiğin bir alt kümesidir. istatistiki bilgilerin harita üzerinde gösterildiği grafiktir.

# indicator (yada indikatör)
gösterge anlamına gelmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# flux

türkçe kelime anlamı: akış, akıntı

flux client side uygulamalar için mimarisel bir patterndir. facebook tarafından react için tasarlandı ve fakat react dışında birçok farklı framework ile de çalışabilir.

facebook flux patternini çıkardığında, aynı isimde bir de javascript implementasyonu ile birlikte çıkardı. piyasada birçok flux pattern'i uygulayan flux impelmentasyonu bulunmaktadır.

flux patter'ni temel olarak MVC'ye alternatiftir. MVC'de M V ve C arasında gidiş dönüş bazen sonsuz döngü yaratabilmekte ve karmaşıklığa sebep olabilmektedir. Oysa flux'ta tek yönlü bir hareket vardır:

Action --> Dispatcher --> Store --> View.

Dispatch türkçe kelime anlamı: sevk etmek. yazılım dünyasında metoda/fonksiyona mesaj(parametre) yollama(sevk etme) anlamında kullanılır.

Bakıldığında redux, flux patterinin biraz değişik bir implementasyonu olduğunu görebiliriz.

# redux
türkçe kelime anlamı: canladırma (örnek: ekonomi canlanması)

javascript kütüphanesidir.

tüm uygulamada ortak bir store kullanılmasını sağlamaktadır. bu store'un her taraftan ortak metodlarla yönetilebilmesini sağlamaktadır. store'un şeması (içinde hangi bilgilerin yer alacağı), uygulamanın o andaki state'ine göre önceden tanımlıdır. sadece verilerin içeriği o sıra uygulama tarafından doldurulur/kullanılır. bu sebeple redux Predictable (önceden kestirilebilir) bir storage'dir.

redux genel kullanımı:

storu değiştirmek için yapılacak olan bir __aksiyon (yada action)__ vardır. örneğin bu aksiyonumuz bu olsun:

```
{
  type: ADD_TODO, //aksiyon tipimiz
  text: 'Go to school' //aksiyonumuzun içeriği
}
```

Aksiyon alacak kişi bu tarz bir json'ı göndereceğini önceden bilemez (bilmesi gerekmez). onun yerine bize bu json'u döndüren bir metod yazarız. 

```js
function addTodo(text) {

  return {

    type: ADD_TODO,

    text
  }
}
```

İşte bu yukarıdaki metod "__Action Creator__"'dır. Bunun gibi bir sürü metodumuz olacaktır.

Normalde beklenen: addTodo metodunun en son satırında storage'a kayıt atması olacaktır:

```js
function addTodo(text) {

  const action = {

    type: ADD_TODO,

    text

  };

  dispatch(action);
}
```

fakat redux'ta bunu yapmaya gerek yok. onun yerine şu yeterlidir:

```js
import { createStore } from 'redux'

// createstore tüm uygulamdan bir kerelik yapılıyor.
// hangi parametrelerin geçileceği sonraki adımlarda anlatılacak. şu adımda önemli diil.
const store = createStore(...); 

// dispatch fonksiyonu sync çalışır ve response olarak son state'i döndürür.
// saga gibi middleware'ler kullanıldığında, middleware ne zaman dönüş yaparsa o zaman dispatch dönüş yapacaktır. kodumuz dispatch'in ne döndürdüğüne değil, store'a bağladığımız listener'lara göre aksiyon almalıyız. çünkü saga gibi middleware'ler yield ile çalışır. bu sebeple, başlatacağımız bir işlem birçok başka işlemi tetikler. ve bunlar yield sayesinde asenkron olurlar. dolayısı ile dispatch state'in en son halini dönmez. 
store.dispatch(addTodo(text));
```

Geldik bu aksiyona göre veritabanını (storageyi) değiştirmeye... Değişiklikleri __reducer (yada reducing function)__ metodlarımız ile yapıyoruz.

reducer kelime anlamı: redüktör (indirgeyici)

```js
function todoApp(currentState = initialState, action) {

   switch (action.type) {

     case ADD_TODO:

        return {

            text: text

            ...currentState
        }

     default: 

        return currentState;
  }
}
```

Yukarıdaki metodda currentState aslında currentStorage'dir. Fakat genelde/örneklerde hep state olarak yazılıyor.

Yukarıdaki state, redux tek bir storage kullandığında her tarafta aynıdır.

switch case'deki default durumu; bilmediğimiz bir aksiyon ile gelinirse storage'de hiçbir şey değiştirmeyiz. bu bi hata durumudur aslında.

ADD_TODO gelirse; şimdiki stateye text diye bir property daha eklemiş oluruz.

currentState = initialState Ecmascript 6 ile gelen bir syntax özelliğidir. currentState null gelirse initialState ile doldururuz. bu şekilde uygulama ilk açıldığında buraya gelme durumunda storageyi initialize eder.

reducer'den döndürdüğümüz(return ettiğimiz) nesne, bizim artık yeni statemizdir.

tahmin edildiği gibi her reducer metodunun tüm state'i görerek işlem yapması sakıncalıdır. bunun için her reducer'a parametre olarak tüm state değil, state'in sadece ilgili parçası gönderilir. örneğin şöyle yaptığımızı düşünelim:

```js
function reducerMethods(state = {}, action) {

  return {

    a: reducerA(state.a, action),

    b: reducerB(state.b, action),

    c: reducerC(state.c, action)

  }
}
```

Yukarıdaki bu işlemi redux kolaylaştırıyor. __combineReducers__ isimli metod sunulmuş. şu şekilde örnekleyebiliriz:

```js
import { combineReducers } from 'redux'

const reducerMethods = combineReducers({

  a: reducerA,

  b: reducerA,

  c: reducerC
})
```


# genel metodlar

```js
const store = createStore(reducer, initialState);
```

artık bu store üzerinde işlem yaparken sadece bu reducer çağrılır.

```js
store.getState()
```

tüm state'in son halini döndürür

```js
store.dispatch(action)
```

alınan aksiyon json'u ile reducer metodu çağrılır. reducer'ları kombine etmek zorundayız. eğer kombine etmezzsek tek bir reducer olacağından metod çok büyük/uzun olur.

# store objesi
createstore fonksiyonu ile yaratılan redux store'umuz ile döndürülen js objesi bu fonnksiyonları içerir:

```js
type Store = {
  dispatch // dispatch fonksiyonu
  getState // uygulamamızın son state'ini dönen fonksiyon
  subscribe // state değişiklikleri için atadığımız listenler'ları bu fonksiyon ile register ediyoruz
  replaceReducer // yeni reducer kullanmak istediğimizde bu fonksiyona onu yollamak zorundayız.
}
```

# enhancer
redux'ın core'unda olan bir özelliktir. bu özellik eklenti altyapısına benzer. redux createstore fonksiyonuna istediğimiz enhancer'larımızı geçeriz. createstore fonksiyonu, bize enhancer'daki kurallara göre fakrlı bir çeşit store objesi döner.

enhancer genelde sayfa yazan yazılımcılar tarafından yazılmaz. genelde kütüphaneler/framework'ler enhancer geliştirirler.

örneğin redux-devtools-extension'ı runtime'da web sayfasının js objesin bu fonksiyonu inject eder:

```js
window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
```

Biz bu fonksiyonu parametrelerle çağırıp enhancer instance'ımızı oluşturabiliriz:

```js
import { createStore, applyMiddleware, compose } from 'redux';

var composeEnhancers;

if( window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ != undefined){

    composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__( { } ); // şimdilik boş parametrelerle çağıralım.
} else {
    // "compose" is the redux's default enhancer
    composeEnhancers = compose;
}

// enhancer, diğer tüm middleware'leri wrap eder:
const enhancer = composeEnhancers(
  applyMiddleware(...middleware),
  // other store enhancers if any
);
const store = createStore(reducer, enhancer);
```

# react-redux

redux'un react ile bütünleşik kullanılmasını sağlayan kütüphanedir. bu paket olmazsa, state yenilendiğinde o anda yürütülen komponent'ler otomatik render edilmez. react'ta event bazlı çalışma şart olduğundan, bu modülün mantık olarak kullanılması şarttır.

react-redux eklentisinin sunduğu bir özellik "__connect__" metodudur. bu metod aracılığı ile React'ın componentinin props'una redux-state'inden bir obje geçebiliyoruz ve dispatch metodlarımızı da component içine kullanıma açabiliyoruz.

dispatch ve state property'lerimiz react component'lerine props olarak geçilmektedir.

# Redux Thunk vs Redux Promise vs Saga

3'ü redux için middleware görevi görür. redux üzerinde asenkron işlemleri kolaylaştırırlar. asenkron işlemler için kullanılması zorunlu değildir. fakat büyük uygulamalarda kolaylık ve standart sağlar.

# redux middleware

redux core içinde bulunan bir özelliktir. dispact işlemine başlamadan önce ve sonra istediğimiz callback metodlarını atabilmemizi sağlar. örneğin her dispatch öncesi ve sonrası state'imizi console'a basmak istersek bir logger-middleware'i yazarız. örnek:

```js
function logger-middleware({ getState }) {

  return next => action => {

    console.log('store before dispatch' + getState());
    let returnValue = next(action);
    console.log('store after dispatch', getState());
    return returnValue;
  }
}
```

Yukarıdaki yazım kodu ecmascript 6 ve sonrası olduğu için farklı görünebilir. ilk satırlar aslında şunu yapıyor:

```js
function logger-middleware({ getState, dispatch }) {

 return function (next) {

      return function (action) {
```

Yukarıdaki middleware'imizi createStore'a parametre olarak yollamamız gerekmektedir:

```js
let store = createStore(

  reducers,

  initParamas,

  applyMiddleware(logger-middleware)

);
```

eğer birden fazla middleware'miz var ise bunları applyMiddleware metoduna parametre geçmemiz gerekiyor:

```js
applyMiddleware(logger-middleware, middleware2, middleware3 );
```

middleware'ler içerisinde sadece next metodu ile diğer middleware çağrılmak zorunda değil. middleware içinde aynı zamanda getState.dispatch() metodunu kullanabiliriz. çağıracağımız dispatch metodu ile paralelden yeni bir aksiyon zinciri başlatılacaktır. dispatch metodu senkron çalışmaktadır. bu sebeple yeni zincir thread olarak başlamayacaktır.

# Saga
saga kelime anlamı: destan

async caller'ı çok kolaylaşıran bir middleware'dir.

```js
import createSagaMiddleware from 'redux-saga'

import mySaga from './sagas';

const sagaMiddleware = createSagaMiddleware();

const store = createStore(

  reducer,

  applyMiddleware(sagaMiddleware)
);

sagaMiddleware.run(mySaga); //bunu saga'nın kendisi istiyor. zorunlu.
```

bir tane action-creator yazalım:

```js
export function actionFetchUser(userId) {

  return {

    type: FETCHING_DATA,

    userId: userId

  }
}
```

Şimdi "sagas" olan dosyaya bakalım:

```js
import { call, put, takeEvery } from 'redux-saga/effects'

function* fetchUser(action) {

   try {

      const user = yield call(Api.fetchUser, action.userId);

      yield put({type: "USER_FETCH_SUCCEEDED", user: user});

   } catch (e) {

      yield put({type: "USER_FETCH_FAILED", message: e.message});
   }
}

function* mySaga() {

  yield takeEvery("FETCHING_DATA", fetchUser);
}

export default mySaga;
```

Yukarıda call metodunun aldığı ilk parametre bir fonksiyondur. bu fonksiyona yollanacak parametreler call'a sırası ile yollanır.

Yukarıda mySaga metodu sadece program başlarken bir kere çağrılır. gerekli "fetchUser" gibi metodları hazırlar. artık redux.dispatch(actionFetchUser("yusuf")); metodu sadece işlem yaptığımızda çağrılacaktır.

redux.dispatch(actionFetchUser("yusuf")); metodunu çağırdığımızda sırası ile:

1- action-creator olan actionFetchUser metodu çalışacaktır.

2- reducer devreye girecek ve store'u sadece FETCHING_DATA için update edecektir.

3- "sagas" dosyasının içindeki fetchUser(action) metodu çalışacak. artık runtime, "put" metodunu görene kadar başka kod çalıştırmayacaktır. put'a geldiği zaman put'a geçtiğimiz parametre action olarak kabul edilmekte, ve yeni bir redux akışı başlatmaktadır.

Şimdi  bir actionCreator çağrırısak diye ikinci bir metodu ekleyelim:

```js
function* mySaga() {

  yield takeEvery("FETCHING_DATA", fetchUser);

  yield takeEvery("FETCHING_IMAGE", fetchImage); 
}
```

fetchImage metodunu da fetchUser gibi yaratmamız gerekecek. mySaga metodunun her satırında takeEvery yerine takeLatest gibi metodlar da koyabiliriz. örneğin sadece fetchImage'ye takeLatest koyarsak, eğer aynı anda birden fazla fetchImage metodu çağrılıyorsa; diğerlerini kill edip sadece en son geleni yapar. önceki bilgilerin gereksiz olacağı durumlarda bu yöntem kullanılır. takeEvery'de ise her elen istek sonuna kadar sonlandırılır.

actionCreator'ın her metodunu mySaga'ya eklemek zorunda değiliz. saga'da yok ise, direk reducer'a gidecektir. örneğin put metodlarımıza yolladığımız bazı action'ları saga'da kullanmamalıyız. eğer her put işlemi saga'da tanımlıysa o zaman sonsuz döngüye girmiş oluruz. çünkü hiçbir metod reducer'a giremeyecek. sürekli yeni redux akışı başlatacak. 

# gerçek çalışan saga örneği

aşağıdaki kodlar https://github.com/react-boilerplate/react-boilerplate içerisine kod eklenerek yazılmıştır.

her kod bloğunun başında yorum satırı olarak dosya ismi yazmaktadır. aşağıdaki tüm dosyalar /app/containers/UiAcquirerList içerisindedir.

Aşağıdaki container bir button gösteriyor. butona tıklandığında Acquirer'ları servisten çekiyor ve ekranda string olan gösteriyor.

```js
/*
 * constants.js
 */

// burada her değer uzun bir string'e denk geliyor. Aynı isimde contaner/componentler olabilir. bu sebepl her birini path'i ile aynı isimde yapıyoruz. bu şekilde conflict'i %100 engellemiş oluyoruz.

export const DEFAULT_ACTION = 'app/UiAcquirerList/DEFAULT_ACTION';
export const FETCH_ACQUIRERS = 'app/UiAcquirerList/FETCH_ACQUIRERS';
export const FETCH_ACQUIRERS_SUCCESS = 'app/UiAcquirerList/FETCH_ACQUIRERS_SUCCESS';
export const FETCH_ACQUIRERS_FAILED = 'app/UiAcquirerList/FETCH_ACQUIRERS_FAILED';

```

```js
/*
 * actions.js
 */

import { DEFAULT_ACTION, FETCH_ACQUIRERS, FETCH_ACQUIRERS_SUCCESS, FETCH_ACQUIRERS_FAILED } from './constants';

export function defaultAction() {
  return {
    type: DEFAULT_ACTION,
  };
}

export function fetchAcquirersAction() {
  return {
    type: FETCH_ACQUIRERS,
  };
}

export function fetchAcquirersSuccessAction(acquirerData) {
  return {
    type: FETCH_ACQUIRERS_SUCCESS,
    acquirerData,
  };
}

export function fetchAcquirersFailedAction(error) {
  return {
    type: FETCH_ACQUIRERS_FAILED,
    error,
  };
}
```

```js
/*
 * reducer.js
 */

import produce from 'immer'; // immutable nesnelerle çalışmamızı sağlayan bir kütüphane
import { DEFAULT_ACTION, FETCH_ACQUIRERS, FETCH_ACQUIRERS_SUCCESS, FETCH_ACQUIRERS_FAILED } from './constants';

export const initialState = {};

/* eslint-disable default-case, no-param-reassign */
const uiAcquirerListReducer = (state = initialState, action) =>
  produce(state, draft => {
    switch (action.type) {
      case DEFAULT_ACTION:
        break;
      case FETCH_ACQUIRERS_SUCCESS:
        draft.acquirerData = action.acquirerData;
        break;
      case FETCH_ACQUIRERS_FAILED:
        draft.error = action.error;
        break;
      case FETCH_ACQUIRERS:
        break;
    }
  });

export default uiAcquirerListReducer;
```

```js
/*
 * selectors.js
 */

/*

reselect reduxtan bağımsız fakat onlar tarafından geliştirilen bir kütüphane.

createSelector fonksiyonu; parametre olarak json objeleri alıyor. en son paramtresinde ise, önceden aldığım tüm objeleri return ediyor.

const mySelector = createSelector(
  state => state.values.value1,
  state => state.values.value2,
  (value1, value2) => value1 + value2
)

yukarıdaki 2 parametreyi, tek array olarak tek bir parametrede de yollayabilirdik:

const mySelector = createSelector(
  [
    state => state.values.value1,
    state => state.values.value2
  ],
  (value1, value2) => value1 + value2
)
*/

import { createSelector } from 'reselect';
import { initialState } from './reducer'; // initial state'imiz boştu. ama aynı objeyi referans etmemiz daha doğru olacaktır.

/* 
asagidaki sytax ecmascript'in güncel sürümlerindeki anonim fonksiyon ile yazılmıştır.
const selectUiAcquirerListDomain = (state) => 
                                            { 
                                               return state.uiAcquirerList || initialState;
                                            }

state.uiAcquirerList null ise initialState'i döndür anlamına geliyor.
zaten createSelector, bizden parametre olarak fonksiyon istiyor.
*/
const selectUiAcquirerListDomain = state => state.uiAcquirerList || initialState;

const makeSelectUiAcquirerList = () =>
  createSelector(
    selectUiAcquirerListDomain,
    // substate, aynı şekilde döndürülüyor. burada istersek if logic'leri yazabilirdik.
    substate => substate,
  );

export default makeSelectUiAcquirerList;
export { selectUiAcquirerListDomain };

```

```js
/*
 * saga.js
 */

import { takeLatest, call, put } from 'redux-saga/effects';
import request from 'utils/request';
import { FETCH_ACQUIRERS } from './constants';
import { fetchAcquirersSuccessAction, fetchAcquirersFailedAction } from './actions';

export function* fetchAcquirersHandle() {
  // saga içindeki her işlem (hata durumu, success durumu...) event-sourcing pattern'indeki gibi ayrı ayrı olması best-practice'dir.

  const requestURL = 'http://localhost:8080/acquirers';
  try {
    // "call" promise tarzı saga'nın sunduğu bir metod. burada asenkron işlemleri bekletebilmemiz gerekiyor.
    // bu sebeple saga bunun için özel bir metod sunuyor. bu kodu elle de yazabilirdik.
    // call işlemi burada Blocking'dir.
    const response = yield call(request, requestURL);
    yield put(fetchAcquirersSuccessAction(response));
  } catch (error) {
    // "put" fonksiyonu, action'ı parametre olarak alıyor.
    // "put", dispather'ı çağırıyor ve yepyeni bir saga akışı başlıyor.
    // "put", non-blocking bir fonksiyondur.
    yield put(fetchAcquirersFailedAction(error));
  }
}

export default function* uiAcquirerListSaga() {
  // bu kod bloğu sadece sayfa init olunca bir kere çalışıyor.
  // burada action'lar kendini register ediyor.
  // her action'ın saga çalıştırıp çalıştırmayacağı sadece burası karar veriyor.
  // artık FETCH_ACQUIRERS ile bir redux akışı başladığında, fetchAcquirersHandle isimli saga devreye girecek.
  // takeLatest alternatifi birçok fonksiyon var. takeLatest şunu salıyor:
  // fetchAcquirersHandle (saga fonksiyonumuz) yield ile yazılmış. yani; adım adım işletilebiliyor. her yield satırına gelene kadar bir adım olarak ele alabiliriz. yani fetchAcquirersHandle.next(), fetchAcquirersHandle.next() ile adım adım işletilebiliyor. saga bu işi kendisi üstleniyor. dolayısı ile tek thread olarak çalışan javascript'i bizim için adım adım işleterek parçalara bölüyor. burada bu avantaj doğuyor: fetchAcquirersHandle fonksiyonu call işlemi başlatsın. call işleminde artık fetchAcquirersHandle thread'i duracaktır. daha call fonksiyonu dönüş yapmadan (response gelmeden) bir daha fetchAcquirersHandle işlemi tetiklensin. işte o zaman redux-saga paketi bizim için yeni call'u başlatıp eski call'u ignore ediyor. aşağıdaki satırdaki takeLatest, fetchAcquirersHandle'ın bu şekilde işletilmesini deklere ediyor. takeLatest alternatifi birçok fonksiyon mevcut.
  yield takeLatest(FETCH_ACQUIRERS, fetchAcquirersHandle);
}

```

```jsx
/**
 * index.js
 */

import React from 'react';
import PropTypes from 'prop-types';
import { connect } from 'react-redux';
import { createStructuredSelector } from 'reselect';
import { compose } from 'redux';

import { useInjectSaga } from 'utils/injectSaga';
import { useInjectReducer } from 'utils/injectReducer';
import makeSelectUiAcquirerList from './selectors';
import reducer from './reducer';
import saga from './saga';
import { fetchAcquirersAction } from './actions';

// react component'i değil, fonksiyonel komponent kullanılmıştır.
export function UiAcquirerList({ uiAcquirerList, dispatch }) {
  useInjectReducer({ key: 'uiAcquirerList', reducer });
  useInjectSaga({ key: 'uiAcquirerList', saga });

  const loadMerchantHandle = () => {
    dispatch(fetchAcquirersAction());
  };

  let err;
  if (uiAcquirerList.error) {
    err = <div>error: {JSON.stringify(uiAcquirerList.error.toString())}</div>;
  }

  return (
    <div>
      <div>
        <input type="button" value="Load All Acquirers" onClick={loadMerchantHandle} />
      </div>
      <div>acquirerData: {JSON.stringify(uiAcquirerList.acquirerData)}</div>
      {err}
    </div>
  );
}

UiAcquirerList.propTypes = {
  dispatch: PropTypes.func.isRequired,
  uiAcquirerList: PropTypes.object,
};

const mapStateToProps = createStructuredSelector({
  uiAcquirerList: makeSelectUiAcquirerList(),
});

function mapDispatchToProps(dispatch) {
  return {
    dispatch,
  };
}

const withConnect = connect(
  mapStateToProps,
  mapDispatchToProps,
);

export default compose(withConnect)(UiAcquirerList);

```

# redux araçları

redux'un araçlarını kullanabilmek için uygulamamızın kodları içine kod eklemeliyiz.

- redux-devTools-extension

  web tarayıcıları için redux debug tarayıcı eklentisi.

- redux-devtools

  tarayıcılardan bağımsız çalışabilen redux debug modülü.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Web Debugging Proxy Application

Yaızlımlar network üzerinden ajax işlemi yaparlar. bu ajaxları intercept edip devam ettirmek ve manipüle etmek için aracı programlardan yararlanılır. bu progrmalara genelde "Web Debugging Proxy Application" uygulamaları denir.

tüm network trafiğini intercep eden uygulamalar:
- Fiddler
- Charles
- mitmproxy (açık kaynaklı): ismi man-in-middle-proxy kısaltmasından gelmektedir.
- tamperchrome: Chrome için google'nin geliştirdiği eklenti. sadece google chrome üzerinden çıkan ajax işlemlerini intercept edebiliyor.

# Temel çalışma mantığı

"Web Debugging Proxy Application" aslında bir proxy uygulaması. bu sebeple örneğin; firefox uygulamasınından gidecek ajax istekleri incelenecek ise; firefox'un proxy ayarlarına "Web Debugging Proxy Application" proxy sunucusunun açtığı ip:port bilgisi veriliyor. artık firefox'un yaptığı istekler "Web Debugging Proxy Application" GUI ekranında listelenmeye başlıyor.

# localhost problemi
localde development yaparken "Web Debugging Proxy Application" uygulamaları localhost'a yada 127.0.0.1'e yapılan istekleri yakalayamayabilir. bu gibi durumlarda localhost yerine makine-name'mizi kullanabiliriz. çünkü localhost direk loopback interface'sine yönlendirilirken, makine-name'mimiz network üzerinden sorgulamaya çalışılıyor.

# Web Debugging Proxy Application'lerin SSL bağlantılarını izlemesi
normal koşullarda ssl bağlantısını kimse takip edemez. fakat, Proxy uygulamamız kendi "certificate Authority" sertifikasını işletim sistemine yada takip edeceğimiz client'a (java app, web browser...) kurdurduyor. Client isteği proxy'ye atıyor. proxy o anda, sisteme tanıtılmış olan certificate Authority'ye uygun bir sertifika üretiyor. proxy, isteği sunucuya kendi atıyor(yönlendirmiyor). dönen cevabı da kendi sertifikası ile imzalayıp döndürüyor. yani proxy, gerçek isteği yönlendirilmiyor. proxy arada sahtecilik yapıyor.

burada bazı sorular/sıkıntılar var. çözümleri ile birlikte açıklamak gerekirse:

- web tarayıcı DNS sorgusu sonrası sunucuya ip ile gidiyor. dolayısı ile http isteği ip ile bağlantı kurulup yapılıyor. yani proxy hangi domaine gitmek istediğini bilemiyor. bu sebeple nasıl o domaine uygun sertifikayı o anda üretiyor?

  Proxy ip'ye aynı anda bir request atıp handshake işlemi yapıyor. proxy o ip'li sunucunun sertifikasına bakıyor. sertifikasında olan "Common Name" bilgisi zaten domain bilgisini vermektedir.

- bazı sertifikalar birden fazla domain bilgisi içerebiliyor. bu gibi durumda, client'ın hangi domaine gitmek istediğini proxy nereden bilebilir?

  Proxy sunucudan indirdiği ssl sertifikasındaki "Subject Alternative Name" değerine bakıyor. burada birden fazla domain ismi oluyor. proxy o anda tüm bu domainlere uygun bir sertifika üretiyor ve client ile bununla haberleşiyor.

şirketler çalışanlarının https bağlantıları bu yöntemle takip etmektedirler. her işletim sistemine certificate Authority tanıtmaktadırlar.

eğer bir kullanıcının tcp ile iletişim kurulacak yazılımına "certificate Authority" kurulmamış ise; tüm network trafiği izlensede kullanının dataları görünemez. çünkü client taraf, daha ilk mesajından itibaren sunucunun private key'i ile şifreli şekilde data'ları yolluyor. şirketler "certificate Authority" sertifikasını tüm web tarayıcılarına ve işletim sistemine baştan root yetkisi ile kuruyor ve kimseye sildirtmiyor. bu sahte "certificate Authority" sayesinde şirket aradaki tüm bağlantıları açık şekilde izleyebiliyor.

peki biz portable bir firefox kurup, işletim sisteminden sertifikaları algılamamasına göre konfigüre edersek ne olacak? bu sefer bağlantıyı şirket takip edemeyecek fakat bağlantıda giden gelen data'lar anlamsızlaşacak ve iletişim kurulamayacak.

# mitmproxy modes
mitmproxy birden fazla mode ile çalışabilir. bazıları:
- socks: sock proxy'si gibi davranabilir
- Reverse: http server gibi davranıp gelen istekleri farklı bir yere yönlendirir
- regular: default mode. Reverse proxy gibidir. fakat client'ı asıl gitmesi gereke yere yönlendirir.
- Transparent: önce router'a giden client, daha sonra router tarafından mitmproxy'ye yönlendirilir. daha sonra mitmproxy default mode'daki gibi gelen istekleri yönlendirir. (aşağıdaki başlıkta daha detaylı anlatılıyor)
- Upstream: birden fazla proxy zinciri varsa, mitmproxy bu zincirdeki bir proxy olarak çalışabilir.

# Transparent Proxying (yada intercepting proxy yada inline proxy yada forced proxy)
Transparent: saydam, şeffaf, transparan

2 farklı anlamı vardır:

- network katmanında kullanıcı hiçbir ayar yapmasına gerek kalmadan yapılan proxy işlemidir.

- https://tools.ietf.org/html/rfc2616 standardında belirtildiği gibi request ve response'u hiç değiştirmeyen proxy'lere denir. __non-transparent proxy__ ise request veya response üzerinde bazen yada her zaman değişiklik yapan proxy'lerdir.

Mitmproxy Transparent mode'a ayarlandığında aşağıdakilerden bir tanesi yapılmalıdır; 
- spoof edilecek cihaz'ın network ayalarında gateway olarak mitmproxy'nin çalıştığı bilgisayarın ip'si verilmelidir. yani gateway = mitmproxy olacaktır.
- router'ımızda mitmproxy konfigürasyonu yapılmalıdır. bu ayar ile client, önce router'a gitmekte, router istekleri ise mitmproxy'nin olduğu sunuya yönlendirmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Cascading Style Sheets (yada CSS)

# sürümleri

2'inci sürümde önemli hatalar bulunduğundan 2.1 spec'i ile bu hatalar düzeltilmiştir.

3'üncü sürüm ise modüler yapıdadır. spec'ler modüllere bölünmüştür ve hepsi 2.1 üzerine kurulu yeni sürümler çıkarır. her spec (modül) faklı hızda ilerlemektedir. örnek modüller: Selector, borders, images...

Bazı modüller 2010'ların başlarında 4'üncü sürümlere atlamıştır. Oysa diğer modüller hala 3üncü sürümdedir. Yani resmi olarak tek bşr CSS3 standartı yoktur. Her alt modülü ayrı ayrı spec numarası ile berlitmek gereklidir.

Bu linkte çok basit bir görsel ile açıklanmıştır: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS3#Modules_and_the_standardization_process

CSS specleri belirtilirken 'sürüm' terimi yerine 'level' kullanılır. örnek:

- 'Selectors Level 3' -> selector modülünün css3 versiyonu
- CSS level 1  

# Bir HTML Objenin kenarların propertieleri (bölümleri)
Bu modele tasarımcılar kendi aralarında "Box Model" olarak adlandırıyor.

Content -> Padding -> Border -> Margin 

# outline
bu propertie box model'deki margin (yada tr:marj) bölümüne denk geliyor. fakat kullandığı alanı diğer elemenentlerin üstüne yazıyor. yani kendi alanı olmuyor. örneğin 10px'lik kırmızı bir outline yaptık. bu kırmızı background sayfanın arkaplan renginin üzerine yazılıyor. fakat sağ ve sol taraftaki elemetler ile 10 px'lik boş yer kaplamadığı için, etrafındaki elementlerin gözükmesin engelliyor.

# display property
- none: hides the element

- block: this elements start from new line

- inline: bir önceki eleman ile aynı satırda devam etmeye çalışıyor.

# div vs span
ikiside temelde bir bölgeyi (html obje grubunu) tanımlamak için kullanılmaktadır. Fakat span varsayılan olarak display:inline iken; div display:block'tur. css ile bu özellikleri override edilebilir. aslında her html elementinin varsayılan olarak block yada inline olup olmadığı bilgisi vardır. fakat genelde diğer elementler (p, a...) div veya span'ın içine alındığından, display özelliği en çok bilinmesi gereken div ve span'dır.

# position propertie
position şu değerleri alabilir:

- __static__: varsayılan değerdir. top, right, left, bottom property'lerindeki değerlerini hiçe sayar. 

aşağıdaki örnekte; 'left: 30px' un hiçbir önemi yoktur. myClass içeren div elementi normal olması gerektiği poziyondadır. sadece top, right, left, bottom property'lerindeki değerlerini hiçe sayar.

```html
<style>
div.myClassStaticExample {
  position: static;
  left: 30px;
  border: 3px solid #73AD21;
}
</style>

<h2>ABC</h2>

<div class="myClassStaticExample">XYZ</div>
```

- __relative__: html'de olması gereken normal pozisyonunu sıfır noktası olarak kabul eder. 

relative'in, static ile tek farkı: top, right, left, bottom property'lerindeki değerlerini hiçe saymamasıdır. 

static'teki aynı örneği; static değerini relative yaparak örneklendirirsek:


```html
<style>
div.myClassRelativeExample {
  position: relative;
  left: 30px;
  border: 3px solid green;
}
</style>

<h2>ABC</h2>

<div class="myClassRelativeExample">XYZ</div>
```

- __fixed__: top, right, left, bottom propertie'lerindeki değerleri viewport'unun görünen kısmını baz alarak kullanır. sayfa scroll ettikçe yeri değişmez. pencerede sürekli aynı pozisyonda durur.

- __absolute__: fixed ile benzerdir. tek farkı; viewportun görünen kısmına değil, sayfanın başlangıcını referans (sıfır noktası) olarak kabul etmesidir. böylece scroll ettikçe sayfada yeri değişir.

- __sticky__: sayfa ilk yüklendiğinde "relative" özelliğindedir. fakat safya scroll edildiğinde ve görünmeyecek duruma geldiğinde, otomatik olarak "fixed" niteliğine geçer.

# z-index
bu propertie top, bottom, right, left gibidir. x ve y kordinatlarına ek olarak z kordinatını verir. elementler üstüste denk geldiğinde hangi elementin son kullanıcıya gözükeceğini belirlemek için kullanılır.

# overflow
bir objenin kendi bloğuna sığmaması durumunda srollbarın davranışına karar verir. sadece overflow-x, overflow-y sadece bir kısımda scrollun çıkıp çıkmayacağına karar vermek için kullanılır.

# transform
bu propertie birçok farklı değer alabiliyor. elementi 2d ve 3d olarak çevirebiliyor. anlık olarak çevirme işlemi yapılabildiği gibi, direk sayfa yüklendiğinde çevrilmiş hali ile de objeyi oluşturmuş olabiliyor.

örnekler:

```html
<div style="width: 150px; 
            height: 80px;
            background-color: red;
            transform: rotate(20deg)">Hello World 1!</div>

<div style="width: 150px; 
            height: 80px;
            background-color: blue;
            transform: skewY(20deg)">Hello World 2!</div>

<div style="width: 150px; 
            height: 80px;
            background-color: yellow;
            transform: scaleY(1.5)">Hello World 3!</div>
```

# media query

aşağıdaki örnekte eğer ekran min-width 480 ise; içerdeki css etki ediyor. yoksa bu css'ler disable oluyor.

```css
@media screen and (min-width: 480px) {

 body {
      background-color: lightgreen;
     }
}
```

format bu şekildedir:

```css
@media not|only mediatype and (expressions) {

 CSS-Code;
}
```

mediatype şunlar olabilir:

all, print (print edilirken bu css uygulanır), screen (pc, mobil, tablet), speech (when screen reader reads the page)

diğer media tipleri yeni sürüm css'lerde deprecated olmuştur (2017 yılı).

web tarayıcılarda olan "reader view" özelliği web tarayıcıların kendince kullandıkları bir yöntemdir. özel olarak css/javascript'ten yakalanamamaktadırlar. örneğin web tarayıcıları "reader view"'a basılınca sadece en az 10 karakter içeren \<p> içindeki textleri görüntülemektedirler.

# !important

'css Precedence' konusu altında incelenmektedir.

kullanımı:

```css
body {
    background-color: white !important;
    }
```

aynı html elementini etkileyen birçok css değeri olabilir. örneğin; bir \<p> elementine inline css ile text rengini değeiştirelim. birde css dosyamızda rengini değiştirelim, bir de başka bir css dosyasında rengini dğeiştirelim. hangisi geçerli olacak? css standratlarında bunların kuralları var. öncelik inline css'tedir. daha sonraki öncelik (eğer inline css yok ise önemsenecek değer); __spesifik__ olan css değerlerindedir. spesifik; o elementi daha iyi tanımlayan değer olarak düşünülebilir. örneğin 'id' en spesifik css selector'üdür. dolayısı le selector'de ne yazdığımız çok önemli. öncelik sırası şu şekildedir:

| css selector type | priority - comment                                                                                                            |
|-------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Inherited styles  | Lowest specificity of all selectors - since an inherited style targets the element's parent, and not the HTML element itself. |
| *                 | Lowest specificity of all directly targeted selectors                                                                         |
| element           | Higher specificity than universal selector and inherited styles.                                                              |
| attribute         | Higher specificity than element selector                                                                                      |
| class             | Higher specificity than attribute, element and universal selectors.                                                           |
| ID                | Higher specificity than class selector.                                                                                       |

'!important' ile o özelliği en yüksek önceliğe taşımız oluyoruz.

Eğer 2 important var ise bu sefer CSS selector değerine göre karar veriliyor. fakat eğer 2 css selector aynı ise, bu sefer css dosyasındaki satır sayısı önemli oluyor.

# css selector

| Selector               | Example               | Example description                                                                                    |
|------------------------|-----------------------|--------------------------------------------------------------------------------------------------------|
| .class                 | .intro                | Selects all elements with class="intro"                                                                |
| #id                    | #firstname            | Selects the element with id="firstname"                                                                |
| *                      | *                     | Selects all elements                                                                                   |
| element                | p                     | Selects all \<p> elements                                                                              |
| element,element        | div, p                | Selects all \<div> elements and all \<p> elements                                                      |
| element element        | div p                 | Selects all \<p> elements inside \<div> elements                                                       |
| element>element        | div > p               | Selects all \<p> elements where the parent is a \<div> element                                         |
| element+element        | div + p               | Selects all \<p> elements that are placed immediately after \<div> elements                            |
| element1~element2      | p ~ ul                | Selects every \<ul> element that are preceded by a \<p> element                                        |
| \[attribute]           | \[target]             | Selects all elements with a target attribute                                                           |
| \[attribute=value]     | \[target=_blank]      | Selects all elements with target="_blank"                                                              |
| \[attribute~=value]    | \[title~=flower]      | Selects all elements with a title attribute containing the word "flower"                               |
| \[attribute \| =value] | \[lang \| =en]        | Selects all elements with a lang attribute value starting with "en"                                    |
| \[attribute^=value]    | a\[href^="https"]     | Selects every \<a> element whose href attribute value begins with "https"                              |
| \[attribute$=value]    | a\[href$=".pdf"]      | Selects every \<a> element whose href attribute value ends with ".pdf"                                 |
| \[attribute*=value]    | a\[href*="w3schools"] | Selects every \<a> element whose href attribute value contains the substring "w3schools"               |
| :active                | a:active              | Selects the active link                                                                                |
| ::after                | p::after              | Insert something after the content of each \<p> element                                                |
| ::before               | p::before             | Insert something before the content of each \<p> element                                               |
| :checked               | input:checked         | Selects every checked \<input>  element                                                                |
| :default               | input:default         | Selects the default \<input>  element                                                                  |
| :disabled              | input:disabled        | Selects every disabled \<input>  element                                                               |
| :empty                 | p:empty               | Selects every \<p> element that has no children (including text nodes)                                 |
| :enabled               | input:enabled         | Selects every enabled \<input>  element                                                                |
| :first-child           | p:first-child         | Selects every \<p> element that is the first child of its parent                                       |
| ::first-letter         | p::first-letter       | Selects the first letter of every \<p> element                                                         |
| ::first-line           | p::first-line         | Selects the first line of every \<p> element                                                           |
| :first-of-type         | p:first-of-type       | Selects every \<p> element that is the first \<p> element of its parent                                |
| :focus                 | input:focus           | Selects the input element which has focus                                                              |
| :hover                 | a:hover               | Selects links on mouse over                                                                            |
| :in-range              | input:in-range        | Selects input elements with a value within a specified range                                           |
| :indeterminate         | input:indeterminate   | Selects input elements that are in an indeterminate state                                              |
| :invalid               | input:invalid         | Selects all input elements with an invalid value                                                       |
| :lang(language)        | p:lang(it)            | Selects every \<p> element with a lang attribute equal to "it" (Italian)                               |
| :last-child            | p:last-child          | Selects every \<p> element that is the last child of its parent                                        |
| :last-of-type          | p:last-of-type        | Selects every \<p> element that is the last \<p> element of its parent                                 |
| :link                  | a:link                | Selects all unvisited links                                                                            |
| :not(selector)         | :not(p)               | Selects every element that is not a \<p> element                                                       |
| :nth-child(n)          | p:nth-child(2)        | Selects every \<p> element that is the second child of its parent                                      |
| :nth-last-child(n)     | p:nth-last-child(2)   | Selects every \<p> element that is the second child of its parent, counting from the last child        |
| :nth-last-of-type(n)   | p:nth-last-of-type(2) | Selects every \<p> element that is the second \<p> element of its parent, counting from the last child |
| :nth-of-type(n)        | p:nth-of-type(2)      | Selects every \<p> element that is the second \<p> element of its parent                               |
| :only-of-type          | p:only-of-type        | Selects every \<p> element that is the only \<p> element of its parent                                 |
| :only-child            | p:only-child          | Selects every \<p> element that is the only child of its parent                                        |
| :optional              | input:optional        | Selects input elements with no "required" attribute                                                    |
| :out-of-range          | input:out-of-range    | Selects input elements with a value outside a specified range                                          |
| ::placeholder          | input::placeholder    | Selects input elements with placeholder text                                                           |
| :read-only             | input:read-only       | Selects input elements with the "readonly" attribute specified                                         |
| :read-write            | input:read-write      | Selects input elements with the "readonly" attribute NOT specified                                     |
| :required              | input:required        | Selects input elements with the "required" attribute specified                                         |
| :root                  | :root                 | Selects the document's root element                                                                    |
| ::selection            | ::selection           | Selects the portion of an element that is selected by a user                                           |
| :target                | #news:target          | Selects the current active #news element (clicked on a URL containing that anchor name)                |
| :valid                 | input:valid           | Selects all input elements with a valid value                                                          |
| :visited               | a:visited             | Selects all visited links                                                                              |

# farklı yazım şekilleri

aşağıdaki iki örnekte aynı anlama gelmektedir:

```css
div {
  padding-top: 10px;
  padding-right: 20px;
  padding-bottom: 30px;
  padding-left: 40px;
}
```

```css
div {
  padding: 10px 20px 30px 40px;
}
```

# pseudo element

web tarayıcısının otomatik tespit ettiği bölgelerdir. bu bölgelere css uygulayabilmemizi sağlar. html DOM'undan çekilemezler.

```css
selector::pseudo-element {
  property:value;
}
```

örnek 1:

```css
p::first-line {
  color: #ff0000;
  font-variant: small-caps;
}
```

örnek 2:

h1 başlığının sağına resim ekler. 

```css
h1::after {
  content: url(smiley.gif);
}
```
web tarayıcısında developer menu açılıp, sayfanın html satırları incelendiğinde aşağıdaki bölge'de şunu görürürüz:

> ::after

# pseudo class

pseudo element; belli bir bölge için css yazabilmemizi sağlarken, pseudo class ise bir özelliği temsil eder ve o özelliğe sahip element için css yazabilmemizi sağlar.

pseudo element'ler çift iki nokta ile gösterilirken,  pseudo class'larda tek iki nokta karakteri vardır:

```css
selector:pseudo-class {
  property:value;
}
```

örnek 1:

```css
/* visited link */
a:visited {
  color: #00FF00;
}

/* mouse over link */
a:hover {
  color: #FF00FF;
}
```

örnek 2:

```css
p:first-child i {
  color: blue;
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# bookmarklet

web tarayıcıları url'den javascript çalıştırılmasına izin verir. bu sebeple birçok hazır js sık kullanılanlara eklenerek tarayıcıya ek işlevler kazandırılır. bu uygulamacıklara bookmarklet denir. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DOM Events
asagida bazı event ve bu eventlerin bazı grupları listelenmiştir.

- #### mouse events:

  - oncontextmenu : user right clicks to an object

  - ondblclick : on double click

  - onmousemove :bir obje üzerinde fare hareket ettiği zaman

  - onmouseenter : fare elementin üzerine geldiğinde

  - onmouseover : onmouseenter ilgili obje içerisindeki diğer objeleri kapsamazken, onmouseover altobjeleri de kapsıyor.

  - onmouseleave : fare objenin dışında çıktığında

  - onmouseout : onmouseleave'in altobjeleri de kapsadığı durum

- #### key events

  klavyeden bir tuşa basıldığında sırası ile bu eventler çağrılıyor: keydown, (bu sırada text ekranda yazılmış görünüyor), keypress, keyup

  keypress metodu CTRL+V gibi işlemleri yakalamazken,  keydown bu işlemleri de yakalayabliyor.

- #### Frame/Object Events

  asagidakiler her element'te geçerli olan eventler değil. çoğu sadece body tag'inde çalışıyor.

  - onabort: The event occurs when the loading of a resource has been aborted

  - onbeforeunload: The event occurs before the document is about to be unloaded

  - onerror: The event occurs when an error occurs while loading an external file

  - onhashchange: The event occurs when there has been changes to the anchor part of a URL

  - onload: The event occurs when an object has loaded

  - onpageshow: The event occurs when the user navigates to a webpage

  - onpagehide: The event occurs when the user navigates away from a webpage

  - onresize: The event occurs when the document view is resized

  - onscroll: The event occurs when an element's scrollbar is being scrolled

  - onunload: once a page has unloaded

- #### Form Object events

  - onblur: object loses focus

  - onchange: the selection, or the checked state have changed (for input, keygen, select, and textarea)

  - onfocus: The event occurs when an element gets focus

  - oninput: keypress gib fakat CTRL+V ile yazılanları da algılıyor.

  - oninvalid: örneğin; textbox'un boş olması

  - onreset: form reset button clicked

  - onselect: the user selects some text (for input and textarea)

  - onsubmit: a valid form's submit button clicked

- #### drag events

- #### clipboard events
  - oncopy 
  - oncut
  - onpaste

- #### print events
  - onafterprint
  - onbeforeprint

- #### Media Events

- #### Animation Events

# event object
javascript'te event callback metoduna geçilen event nesnesinin bazı propertie'leri ve metodları asagidadir.

asagidakilere ek olarak her event (örneğin click-event) kendi propertie'sini eklemektedir.

- #### propertie'ler

- bubbles: Returns whether or not a specific event is a bubbling event.

- cancelable: Returns whether or not an event can have its default action prevented

- currentTarget: Returns the element whose event listeners triggered the event

- defaultPrevented: Returns whether or not the preventDefault() method was called for the event

- eventPhase: Returns which phase of the event flow is currently being evaluated

- isTrusted: if user call the action or not.

- target: Returns the element that triggered the event

- timeStamp: Returns the time (in milliseconds relative to the epoch) at which the event was created

- type: Returns the name of the event

- view: Returns a reference to the Window object where the event occured

- #### metod'lar

- event.preventDefault()

```html
<script>
function engelle(e){
    e.preventDefault();
}
</script>

<a href="http://www.bilisimturk.org" onclick="engelle(event)">Tıkla</a>
```

a tag'ine tıklandığında sayfa başka yere gitmesi gerekirken gitmeyecektir. preventDefault bunu engelliyor. örneğin; keypress eventine şu metodu yazarsak rakam haricindeki kullanıcı girişlleri bir input'a yapılamayacaktır:

```js
if(e.keyCode>=58 || e.keyCode<=47)

   e.preventDefault();
```

- event.stopPropagation();
DOM'da Div içinde p, onunda içinde psan elementi olsun. asagidaki tanımlamalar yapılmış olsun. spana fare ile tıkladığımızda, stopPropagation diğer metodların çalışmasını engelleyecektir.

```js
$("span").click(function(event){

   event.stopPropagation();

   alert("The span element was clicked.");
});

$("p").click(function(event){

   alert("The p element was clicked.");
});

$("div").click(function(){

   alert("The div element was clicked.");
});
```

- stopImmediatePropagation()
aynı objenin click event'ine 2 tane metod atılmış olsun. diğer metodların çalışmasını engellemek için bu metod kullanılır.

- return of callback method

event metodu return false yapınca da belli anlama geliyor. aşağıdaki tabloda net görülmektedir.

|                          | stop bubbling | prevent default action | prevent event handlers        |
|--------------------------|---------------|------------------------|-------------------------------|
|                          |               |                        | Same Element / Parent Element |
| return false             | Yes           | Yes                    | No      /     No              |
| preventDefault           | No            | Yes                    | No      /     No              |
| stopPropagation          | Yes           | No                     | No      /     Yes             |
| stopImmediatePropagation | Yes           | No                     | Yes     /     ?               |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# http headers

> Age

proxy üzerinde kaç defa cache'den bu değerin kullanıldığıdır. otomaik proxy tarafından her defasından 1 arttırılır.

> Connection

keep-alive, close gidi değerler alır. Bir sonraki isteğin bu istekte kullaılan tc soket üzerindn gitmesini sağlar. bir sonraki istekler gönderileceği kesin ise; performans artışı sağlar. bu header'dan da anlaşıladığı gibi ssl bağlantıları her istekte eğer keep-alive gidiyorsa yeni soket üzerinden yapılmaz.

> Authorization

Standart auth işlemleri için kullanılan header

> Proxy-Authorization

Authorization ile aynı görevde fakat proxy'nin önemsediği değerdir.

> Accept

requestin kabul edeceği mimetype

> Accept-Charset, Accept-Encoding, Accept-Language

requestin kabul edeceği tipler

> Content-MD5

sadece body kısmının md5'i.

> Content-Type

sadece body kısmımının mimetype'si.

> TE

Transfer encoding'dir. Body kısmındaki verinin nasıl tutulduğunu belirtir. örneğin compress yapılmış ise; gzip gibi.

> Date

requestin oluşturulduğu tarih

> Max-Forwards

bu isteğin en fazla kaç tane proxy (aracı) üzerinden gidebileceğini liitler.

> DNT

do not track. 1 veya 0 değerini alır.

> Origin

CORS çin gereklidir. O anda URL barda bulunan domain adresidir. Bunu ajaxı yapan javascript değil, tarayıcının kendisi doldurmaktadır.

> Range

Büyük body kısmı olduğunda sürekli olarak arkaplanda tekrar işlem gönderiliyor. Böyle durumlarda bu header'a bod'nin hangi byte'larının (byte aralığı) gönderldiği belirtilmektedir.

> Forwarded

X-Forwarded-For yerine kullanılan standartlaşmış bir header'dır.

client proxy'ye istek yaptığında bu değer boştur. daha sonra ilk proxy 2'inci proxy'ye istek atarken bunu doldurur. 1. proxy 2.inci proxy'ye istek yaparken bu header bu şekildedir:

Forwarded: for=CLIENT_IP

Daha sonra 2. proxy gerçek sunucuya isteği yönlendirdiğinde bu header şöyledir:

Forwarded: for=CLIENT_IP, for=PROXY1_IP; by=PROXY3_IP; proto=http; host=example.com

> Via

forwarded ile aynı bilgileri içerir aynı zamanda kullanıla protokollerin sürümlerini de içerir.

> Host

istek yapılan url'nin adresi ve portu. path buna dahil değildir.

> Referer

Kelime olarak yanlış yazılmıştır fakat artık herkes bu şekilde kullanmaktadır.

Bir başka isteye yönlendirmelerde referer kısmı doldurulur. kullanıcının hangi siteden yönlendiğini anlamak için kullanılır.

> X-Forwarded-For

standart olmayan bir header'dır. Forwarded'dan önce vardı. fakat standart olarak referer sonradan belirlendi.  Fakat hala x-forwared-for'u kullanan yazılım/donanımlar var.

X-Forwarded-For IP bilgilerini tutar. bir proxy bir isteği başka yere yönlendirdiği zaman bu bilgide orjinal isteği yapan sunucunun ip'sini barındırır. örnek:

X-Forwarded-For: client, proxy1, proxy2

> X-Forwarded-Host

X-Forwarded-For ile aynı bilgiler içerir. sadece IP yerine domain bilgisi tutar.

> X-Forwarded-Proto

X-Forwarded-For varken protokollerin yazlması için ayrı bir header belirlenmişti. her aracı bir sonraki aracıya isteği yaparken hangi protokol kullandıysa (örnek https) onu buraya yazar.

> X-Forwarded-IP

standart değildir. mail sunucularına istek yaparken kullanılan bir değerdir.

> Access-Control-Request-Method

CORS için gerekli. "Preflight" isteklerinde kullanılır. Preflight onayı alınırsa gönderilecek asıl isteğin metodudur (örnek : "put").

sunucu asıl request'e izin verip vermeyeceğine karar verebilmek için preflight'ta bu tarz detayları ister.

> Access-Control-Request-Headers

CORS için gerekli. "Preflight" isteklerinde kullanılır. Preflight onayı alınırsa gönderilecek asıl isteğin header'ını barındırır.

sunucu asıl request'e izin verip vermeyeceğine karar verebilmek için preflight'ta bu tarz detayları ister.

> Access-Control-Expose-Headers

CORS isteklerinde sunucu bu header'ı client'a döndürür. web tarayıcı JS tarafında sadece:

- burada belirtilecek olan header'ların
- default olarak web standartlarında belirtilmiş header'ların

okunmasına izin verecektir.

> Access-Control-Allow-Origin

Response'ta bulunur. Preflight ‘a cevapta bulunur. Server'ın izin verdiği tüm domain'leri içerir. bu domainler url bar'daki domainler oluyor.

> Access-Control-Allow-Methods, Access-Control-Allow-Headers

Response'ta bulunur. Preflight'a cevapta bulunur. Server'ın izin verdiği bilgilerdir.

> Access-Control-Allow-Credentials

Response'ta bulunur. Preflight ‘a cevapta bulunur. Server'ın izin verdiği güvenlik bilgilerini içerir. örneğin; asıl atılacak istekte cookie olup olmayağını belirtir.

> Access-Control-Max-Age

Preflight responsunda bulunur. Bu o isteğin ne kadar süre gereli olduğunu belirtir.

> X-Requested-With: 

Bu header her zaman XMLHttpRequest değerini alır. Bunun yollanma sebebi isteğin CORS olup olmadığını ayırt edebilmektir. Çünkü CORS standatlarında yollanan isteklerde sadece bazı header'lar yer alabiliyordu. Bu header listede olmadığından CORS standartları çerçevesinde atılmadığını anlamak için kullanılır.

> User-agent

tarayıcı yada isteği yapan client'ın bilgisi

> Upgrade

istek yapılan yere http sürümünü güncelemesi isteğidir.

> Status

- 1xx Informational responses: büyük data gönderilmeye devam ediliği zaman, protokol sürümü değiştirileceği zaman...

- 2xx Success

- 3xx Redirection

- 4xx Client errors: url too long, bad request, method not found, auth failed...

- 5xx Server error: internal problems on server…


Status code'lar spring'de bu şekilde döndürülüyor:

```java
import org.springframework.http.ResponseEntity;

@RequestMapping(value = "/controller", method = RequestMethod.GET)
@ResponseBody
public ResponseEntity sendViaResponseEntity() {
    return new ResponseEntity(HttpStatus.NOT_ACCEPTABLE); // ilgili hata code'unu HTTP responsu'nun status'unda dönüyor
}
```

Diğer durumlarda spring yine gerçek hataları atıyor. örneğin filterlarda olmadık bir exception olursa, internal server error hata kodu dönüyor. standart http kodları dönüyor.

Hata kodları detaylı şekilde burada anlatılıyor: https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml (archive date: 16/11/2019)

Yukarıdaki header'lara ek olarak cache için kullanılan header'lar da vardır. bu header'lar başka başlıkta anlatılıyor.

# header isimlendirmesi

Eskiden x- ile başlayanlar standart olmayan header için kullanılıyordu. fakat bazı header'lar sık kullanılmaya başlandığından, artık standart hale geldiler. bu sebeple artık custom header'lara farklı bir prefix yazılması önerilir.

# relative quality factor
q parametresi "accept" keyword'ünü içeren header'ların (Accept-Charset, Accept-Encoding...) value kısmında kullanılmaktadır. bu değer 0 ile 1 arasında bir sayı değeri almaktadır. örnek:

```
Accept: text/plain; q=0.5, text/html, text/x-dvi; q=0.8, text/x-c
```

Yukarıdaki header şu analam geliyor: __html__ ve __x-c__ en öncelikli (q=1 default), eğer bu tipleri sunucu desteklemiyorsa, __dvi__ olarak cevap version. eğer onu da desteklemiyorsa, __plain__ cevap version.

# header'lar cookieleri barındırır

header'lar arasında cookie diye bir değer bulunur. bu degerde cookieler taşınır. formatı şu şekildedir:


- client sunucuya atarken Header içerisinde şu satırlar yer alır:

Cookie: name=value; name2=value2
Cookie: name3=value3

Yukarıda expire secure gibi nitelikler isteğe bağlıdır.

- Suncudan client'a repsonse içerisinde dönen heraderlar

Yukarıdaki ile aynı formattadır. tek farkı "Cookie:" yerine "Set-Cookie:" yazmasıdır. Aynı zamanda expire, secure gibi özellikler burada isteğe bağlı belirtilmelidir.

Set-Cookie: name=value; name2=value2; Expires=12.04.2018; Secure
Set-Cookie: name3=value3

her tarayıcı isteğin yapıldığı domaine bağlı tüm cookie'leri sunucuya otomatik gönderir. her defasında yollanması istenmeyen cookie'ler, cookie olarak değilde, farklı storage'lere atılmalıdır.

# upload file 

tarayıcılardan sunucuya dosya yükleme işlemleri var. bu işlemler http standartları altında yapılmaktadır. upload işlemi POST tipindedir.

"contenttype" header'ı "multipart/form-data" olmalıdır. bu işlemde html form içindeki elemanlar yollanırmış gibi data yollanır.  

Header'da aşağıdaki bilgi olmalı:

```
contentType = part/form-data; boundary=--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ
```

tarayıcılar otomatik olarak bir form'daki bilgileri request'e uygun şekilde dolduruyor. Yukarıdaki header'daki string'imizi body kısmında kullanıyoruz. her form elemanını ayrı ayrı aynı body'de yolluyoruz. body kısmı:

```
--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ

Content-Disposition: form-data; name="username"



Ahmet

--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ

Content-Disposition: form-data; name="Filedata"; filename="profile_photo.png"

Content-Type: application/octet-stream



PHOTO_FILE_AS_BINARY_IS_HERE

--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ

Content-Disposition: form-data; name="country"



England

--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ
```

yukarıdaki işlem tek bir tcp soketi üzerinden yollanır. stream'ing yaparak yollar. ve yukarıdaki tek bir request'tir. tarayıcılarda tek işlem olarak algılanır. 

java rest sunucusu üzerinde metod çok genel olarak şu şekilde olacaktır:

```java
@post
fileUpload(inputstream) {

   while( inputstream.hasnext() ){

        inputstream.read();
   }
   return http.status.ok;
}
```

Alt seviyeli bakıldığında; herhangi kısa bir http isteği de steraming yaparak yollanır. fakat büyük bir data olmadığı için inputStream'ler işin içine katılmaz. bu sebeple upload işlemlerinde inputStream'ler işin içine girer.

# download file
tarayıcılar bir dosya download edilmesi gerektiğinde, GET isteği ile download yapar. get'in response'u data'nın direk kendisidir (binary data). get işleminde hiçbir header eklemeye gerek yok. işlem tek bir request'tir. sadece response'un alınması uzundur. java sunucu tarafı psudo olarak aşağıdaki gibidir:

```java
@RequestMapping(path = "/download", method = RequestMethod.GET)

public ResponseEntity<Resource> download(String param) throws IOException {

  InputStreamResource resource = new InputStreamResource(new FileInputStream(file));

  return ResponseEntity.ok()

          .contentLength(file.length())

          .contentType(MediaType.parseMediaType("application/octet-stream"))

          .body(resource);
}
```

# multipart/alternative vs multipart/mixed
bu isteklerin standartları mail sunucuları tarafından kullanılmaktadır. normal web sayfalarında kullanılmazlar. bunun gibi birçok mimetype mevcuttur.

# chunked http request
chunk türkçe kelime anlamı: yığın

http 1.1 ile gelen bir özelliktir. http 2.0 ile bu özellik artık kullanılamamaktadır. çünkü http 2 daha farklı stream'ing özellikleri sunmaktadır.

"Content-Length" header'ı body'nin byte boyutunu belirtmektedir. eğer bu değer yoksa "Transfer-Encoding: chunked" header'ı elle yada server tarafından otomatik eklenmektedir. eğer işlem chunked ise sürekli olarak payload/body karşı taraftan çekilmektedir.

chunked işlemini hem sunucu hem client atabilir.

her chunk satır sonu karakteri ile sonlanmaktadır.

Her tarayıcı ve web browser (default ayarları ile, ek ayar yapılmamışsa) giden body'nin boyutu hesaplar ve belli bir limiti geçmişse chunked olarak karşıya atarlar.

"Content-Encoding: gzip" header'ı sıkıştırılmış chunked data atabilmemizi sağlamaktadır.

# chunked vs multipart

chunked alt seviyeli ve (HTTP API'yi geliştirmiyorsa) yazılımcı tarafından farkedilmemektedir. fakat multi-part daha üst seviyeli bir işlemdir.

# http version history

http versiyonları HTTP/1.1, HTTP/2 şeklinde gösterilmektedir.

- 0.9 (1991)
- 1.0 (1996)
- 1.1 (1999)
- 2 (2015)

Her sürüm arası köklü değişiklikler yapılmıştır.

# HTTP 1.1 vs HTTP 2

- http2 de her request frame'lere bölünmüştür. bir request/response birçok frame'den oluşabilir. en ufak birim framedir.

- eskiden her istek birer tcp bağlantısından gider gelirdi. yani birçok istek paralel yapılmak istendiğinde birçok tcp bağlantısı açılırdı. artık tek tcp üzerinden birçok frame karşı tarafa yollanabilir. bu sebeple hangi frame hangi request'e ait olduğunu anlayabilmek için her frame içinde stream-id kullanılır. aynı anda response geliyor olabilir ve biz data yolluyor olabiliriz. aynı stream'den giden ve gelen olamaz fakat diğer stream'lerden dönüş alınıyor olabilir. bu özelliğe "Multiplexing" deniyor.

- tek tcp bağlantısı olması (her domain için) daha basit bir yapıya kavuşturmuş oldu. eskiden bir tarayıcı her domain için ortalama 6 soket açıyordu. bu ölçümlere göre daha çok performans kaybına (cpu, ram) sebep oluyor ve komplekliği arttırıyordu.

- http dünyasında "message" terimi bir response yada request'i temsil etmek için kullanılır. bu terim http2'ye özgü değildir.

- header compress özelliği gelmiştir.

- priority: client birçok istek yapsın, bunların dönüşünde hangilerine öncelik verilemsi gerketiğinin bilgiside karşıya yollanıyor. bu özellik ile http-api'leri kendi içinde öncelik veriyor. bunun için yazılımcının ek bir ayar yada bilgiye ihtiyaç duymuyor.

- "Server push" özelliği: sunucuya index.html'i döndürsün diye istrek yaptık. bize index.css, main.css gibi değerleri de dönebiliyor ekstradan. bu requestleri zaten %99 yapacağını bildiği için sunucu request beklemeden atıyor. web tarayıcısıda bunları cache'leyebiliyor (isterse).

- binary data: eski sürümlerde satır sonu karakterlerinin bir önemli vardı. dolayısı ile satır sonu karakterine denk gelecek bir encoidng kullanmak zorundaydık. oysa artık byte seviyesindeki sinyaller kullanıldığından, byte seviyesinden bilgi alımı için ekstra parse/kontrol yapmaya gerek yoktur.

- http1.1 client, http2 sunucusunun döndüreceği cevabı algılayamaz.

# SPDY

bir kısalmta değildir. özel isimdir. google'ın http alternatifi geliştirdiği ptotokoldür. fakat http2 çıkışı ile artık geliştirilmemektedir.

# http request methods

- get

  return details of given data.

- post

  - HTML üzerinden bilgi göndermek için kullanılır (form bilgileri gibi)
  - daha karmaşık işlemler gerektiğinde bu yöntem tercih edilmelidir:
    - birden fazla kaynak gönderirken
    - içiçe kaynak güncellemelerinde

- patch

  kaynağı güncellemek için kullanılır

- put

  override (or create if not exist) the given data.

  put ile yepyeni bir kaynak oluşturulup yollanabilirken, patch ile mutlaka sadece varolan kaynak güncellenebilir. 

- options

  returns all methods of the given url.

- CONNECT

  http standartları üzerinde (ssh a benzer şekilde) daha düşük seviyede (tcp socket) tünel açıp artık yapılacak isteklerin oradan yapılması sağlanır. örneğin chrome tarayıcısında connect ile bir http isteği yapılır, onay alındıysa, artık yapılacak istekler o tünel üzerinden, connect isteği yaptığımız sunucuya gidecektir. connect isteği yaptığımız sunucu ile aramızda bir proxy var ise; o proxy artık yaptığımız işlemleri göremeyecektir. dolayısı ile aalında connect işlemi ile gerçek sunucu ile direk iletişimde olmaktayız. bu güvenlik açıkları da meydana getirmektedir. bu sebeple canlı sistemlerde CONNECT metodu devre dışı bırakılmaktadır.

- trace

  requestte gönderilen bilgiyi aynı şekilde döndürmektedir. test amaçlı kullanılmaktadır.

- DELETE

  delete given data

- HEAD

  get metodu ile aynıdır. sadece reposneta body yoktur. response header'da, LastModified/ContentLength gibi değerler mevcuttur. bu bigliler kullanarak tekrar get ile datayı çekmeye gerek varmı diye kontrol edilebilir. (başka başlıkta anlatılıyor)

https://tools.ietf.org/html/rfc7231 (archive date: 03/12/2019)'de her işlemde hangi status codun dönmesi gerektiği ve hangi dataların hangi durumlarda dönmesi gerektiği belirtilmiştir. örneğin; "4.3.3.  POST" başlığında POST işlemi ile bir kaynak oluşturulduğunda, response olarak 201 dönülmeli ve header'da "Location" dönülmesi ve bu değerde (location değerinde), oluşturduğumuz yeni kaynağa erişebilmek için gerekli "get" isteği url'sinin olması gerektiği belirtilmektedir.

rfc7231, https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml (archive date: 03/12/2019) gibi sayfalardan referans olarak kullanılmaktadır.

# conditional requests
Herhangi bir requestimiz'e cache header'larını ekleyerek duruma göre işlem yapmasını bekleyebiliriz. örneğin; get'e header ekleyebiliriz. eğer veri değişmişse bize yeni data döndürülür. put'a header ekleriz. eğer data şimdiki yollayacağımız data ile aynı ise, sunucuda put yapmaya gerek kalmaz. bu tarz request'lere conditional request denir. http standartlarında HEAD metodu bu iş için tasarlanmıştır. head'de body atılmaz sadece header'lar atılır. HEAD değişiklik var mı yok mu onun bilgisini döner. fakat body dönmez. standartlarda yoktur. bu sebeple herkes genelde; HEAD kullanmıyor. onun yerine diğer get veya put'a header eklemeyi tercih ediyor. çünkü head kullanılırsa tekrardan put ve get metodunu tekrar sunucuya atmak gerekebilir. fakat direk put ve get'e header ekleyince, sunucu conditiona'ı sağlarsa işlemi yapacaktır (get ise yeni datayı  döndürücek, put ise datayı update edecektir).

HEAD metodu header'da "ETag" veya/ve "Last-Modified" tagini barındırıyor. buna cevap olarak sunucu; aynı (istek) path'e "get" isteği yapılsaydı en son atılan response'takine göre değişiklik varmı yok mu onun bilgisini döner.

HEAD yerine get ve put kullanılacaksa hem "ETag" veya/ve "Last-Modified" hemde aşağıda anlatılan "if-match" gibi metodlarda kullanılabilir. 

apache http server static dosyaları dışarıya açarken bu özelliği otomatik sunar. yazılımcının ek bir configürasyon/kod girmesine gerek kalmaz.

javaee yada spring'de controllerlarda head isteniyor ise; bu metod elle hazırlanmalıdır. örnek bir wrapper yazılabilir:

```java
protected void doHead(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    doGet(req, response);
}
```

yukarıdaki metod filter'a eklenip eğer request HEAD ise çağrılabilir. bu şekilde her metod için ayrı HEAD yazmak zorunda kalmayabiliriz.

web tarayıcıları web standartlarını varsayılan olarak uygular ve static dosyalar için (örnek css, <img> ...) conditional request kullanıyor fakat head ile değil. bir elemanı çekmek için en son tarayıcnın çektiği tarihi "get" metoduna "If-Modified-Since" olarak ekliyor. sunucu eğer hala değişiklik yoksa cevap olarak bunu bildiriyor. eğer değişiklik var ise; direk cevap olarak yeni datayı gönderiyor. bu control web tarayıcı tarafından cache disable edilmedikçe otomatik yapılır.

"spring-data-rest" modülü databasedeki değerleri direk önyüze sunucucak kontrollerları da otomatik hazırlıyor. bikaç anotation kolayca entitylerimizi önyüe hazırlıyor ve head kontrollerini de standartlara bağlı olarak dışarıya açıyor. spring-data-rest pek tavsiye edilmez. çünkü database nesneleri genelde direk tümü ile önyüze açılmazlar. bazıları static parameter servislerini spirng-data-rest ile dışarıya açıyor. spring-data-rest modülü her nentity için put, get gibi tüm metodları dışarıya hazırlıyor ve Richardson Maturity Model Level-3'ü de hazırlıyor.

database'lerde otomatik olarak bir satır değiştiğinde, o satırda belirlediğimiz bir sutnuna otomatik tarih atılabiliyor. bu özellik için javada da entity içinde o sutun için özel bir anotation bulunmaktadır. bu şekilde önyüze HEAD metodlarımızı hazırlarken bu sutundan yararlanabiliriz.

# strong ve weak validator

W/ karakterlerini hash'lerin önüne atarsak sunucunun weak kontrol yapmasını istemiş oluruz. atmazsak strong istemiş oluruz.

strong ve weak'in anlamı yazılım geliştiricinin tanımına kalmıştır. örneğin bazı mimarilerde strong kontrol, header'ları dahi kontrol eder. bazı mimarilerde ise; strong iligli kaynağın çok önemli verilerinin değişip değişmediği anlamına gelir.

# cache ile ilgili header'lar

(cache harici diğer header'lar başka başlıkta anlatılıyor)

- #### Cache-Control (eski sürüm http'lerde "Pragma"):

alabildiği değeler: (multiple değerler alabiliyor)

- no-cache: data must be re-validated on each requeest

- no-store: never cache

- public: can be cached by the browser and any intermediate (proxy) caches

- private: only browser should cache it

- max-age: max age of cached content as second. on new http versions replaced by "Expires" header.

- s-maxage: max-age ile aynı görevi görüyor fakat bu client haricindeki aracılar (proxy) için geçeri bir değerdir.

- #### ETag

Cache verisinini değişip değişmediğini kontrol etmek için kullanılan değer.

- #### If-Match

örnek kullanım:

If-Match: "34242"

sağ taraf kaynağın hash'idir. eğer değişiklik var ise sunucu kaynağı dönmeyecek. eğer değişiklik yok ise kaynağı dönecek.

if-match içine birden fazla değer atanabilir. örnek:

If-Match: "34242", "34666"

- #### If-None-Match

If-Match'in tersidir. eğer kaynak'ın hash'i bu değil ise tüm kaynak döndürülecektir.

- #### If-Modified-Since

burada bir tarih değeri sunucuya yollanıyor. eğer bu tarihten sonra ilgili kaynak güncellenmiş ise tüm kaynak cevap olarak dönecektir.

içine birden fazla tarih değeri atabiliriz.

- #### If-Unmodified-Since

If-Modified-Since'in tersi.

- #### IF-Range

içine tarih yada hash degerini alabiliyor. bu header ile beraber farklı bir header'da "Range" değerini yollamak şart.

içine sadece bir tane değer alabiliyor.

if the entity (only the part of range) is unchanged, send me the part(s) that I am missing; otherwise, send me the entire new entity.

- #### Last-Modified

sunucu tarafından client'a gönderilen bir header'dır. ilgili kaynağın en son ne zaman güncellendiğini belirtir.

- #### 304 Not Modified

bu status sunucu tarafından client'a eğer değişiklik yok ise gönderiliyor. eğer kaynakta değişiklik var ise sunucu 200 OK döndürüyor. fakat her request için aynı cevap dönmeyebiliyor. istisnai durumlar çok fazla.

# payload

türkçe kelime anlamı: yük.

iletişim protokollerinde metada'lar dışında kalan mesaj bloğudur. asıl gönderilmek istenen mesaj kısmıdır. doğal olarak; sadece ajax işlemlerinde kullanılan bir terim değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Viewport
Sayfa yapısı ile ilgili kararları vermemize yarayan meta etiketidir. HTML HEAD tag'larının içerisinde olmalıdır.

örnek;

sayfa ilk yüklendiğinde %100 zoomlanmış olmalıdır:

<meta name="viewport" content="initial-scale=1" />

genişliğin 360 pixel olduğunu belirtiyoruz. tarayıcı buna göre sayfayı fit edebilir.

<meta name="viewport" content="width=360" />

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Document Object Model
DOM, bir programlama lisanı veya sadece JavaScript için tasarlanmış bir olgu değildir. DOM, HTML ve XML dokümanları için API'dir. genelde DOM denilince HTMK dökümanları için olan API akla gelir fakat sadece bu değildir.

# xhtml (yada Extensible HyperText Markup Language)
html standartları katı kurallara sahip değildir. örneğin xml teglaeri büyük harf yazılabilir. oysa xhtml bu kuralları daha katı standartlara baglamıştır. xhtml tarayıcılar tarafından desteklenir çünkü yine xml yapısındadır. sadece kurallar daha katı tututlur. temiz kod sağlanır.

# DHTML (yada Dynamic HTML)
CSS Javascript ve HTML çalışan yapılara verilen isimdir.

# HTML DOM Levels
HTML DOM'ün sürümleridir. DOM'un çalışma prensiplerinin kurallarıdır (spesifikasyonlarıdır). DOM 1, DOM 2, DOM 3, DOM 4...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cookie security policy
cookie'ler:

- port: aynı domain'inin tüm port'larındaki sercisler tarafından kullanılabilir. port önemsizdir.
- subdomain: subdomain'ler cookie spesific programatic olarak belirtilebilmektedir.
- path: path bilgisi önemsizdir.

# cookies properties

- secure: sadece https üzerinden giden ajax isteklerine eklenir. http ise gönderilmez.

- httpOnly: javascript tarafından bu cookieler okunamaz. fakat her ajax isteğinde sunucuya gönderilir.

- Expires (yada Max-Age): son kullanma tarihi

- Path: o domain için kimlerin bu cookie'ye erişebileceği yetkisidir. path'in prefix'idir. tüm bu prefix'e ait subdirectorie'ler (sub-url'ler) bu cookie'ye erişebilir.

- same-site: Strict yada Lax değerini alabilir. eğer sunucu göndermişse bu cookie'yi, sunucu sadece bana gelen isteklerde bu cookie kullanılsın diyebilir. yani aynı html sayfasında başka bir iş akışı başka bir url'ye istek yaparsa bu cookie gönderilmez. böyle durumlar için cookie strict olarak set edilmelidir.

- domain: hangi domainlerin buna erişicek yetkisi olup olmadığıdır. normalde tarayıcılar başka domainden cookie okumaya veya yazmaya izin vermez fakat bu kısım 2 sebepten ötürü yapılmıştır:
  - third party cookie'lere izin veriliyor (başka başlıkta anlatılıyor)
  - sub-domain'ler belirtilebilir: her subdomain (a.google.com, b.google.com gibi) sadece kendi yada o domaine bağlı tüm subdomain'ler tarafından erişilebilir cookie set edebilir.

# third party cookie vs first party cookie
- a.com'a gittik.
- r.com içerisinden bir js yüklendi
- r.com kendi içerisinde cokkie set etti (javascript'te cookie set ederken domain de belirtilebilir). bu senaryoda r.js kendi domainine cookie set ediyor.
- Daha sonra b.com'a gidildi ve yine r.com'un bir js'i yüklendi. r.com b.com'a gidildiğinde, bu kullanıcının a.com'a da gittiğini görebiliyor. çünkü kendi cookie'leri third party.

son kullanıcı tarayıcıların ayarlarından basit bir şekilde bunu disable edebiliyor.

# media file transfer cookies
media dosyaları web tarayıcı tarafından otomatik sunucudan çekilirken, varolan cookie'ler sunucuya yollanmaktadır. nihayetinde resimlerde çekilirken, http isteği yapılmaktadır.

# firefox and chrome cookie management
- chrome ile bir sayfada inspect element yapıldığında, cookie kısmında www.a.com grubu altındaki cookie'ler listelendiğinde, domain sutununda bazen www.a.com'dan farklı domain'lerin olduğunu görebiliriz. bunun sebebi a.com'da (inspect ettiğimiz sayfada)  farklı iframe'ler olmasıdır. farklı domain grupları ise, javascript tarafında gidilen sayfanın (a.com'un), farklı domainler için set ettiği cookie'lerdir. oysa firefoxta cookie'ler listelenirken sadece domaine göre gruplandırılmaktadır.

- firefox ve chrome private mod'larda cookie'ler ile ilgili hiçbir farklı politika izlemiyor.

- cookie'ler güvenlik sebebi ile varsayılan olarak tarayıcılar ile editlenemiyorlar. bunun için eklenti kullanmak şart.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Diğer sitelere login olup olmadığı bilgisini edinme

Örneğin facebook'a login olunca, instagram ayrı sekmede facebook'a login olduğumuzu anlayabiliyor. Bunu ufak bir hack yöntemi ile gerçekleştiriyorlar. Bu açık bir CSRF tipi bir saldırıdır. Bir siteye giderken yönlendirmeler kullanılabilmektedir. Örneğin; https://www.facebook.com/login.php?next=www.facebook.com/bookmarks/Fpages  bu sayfa ğer kullanıcı login olmuşsa bizi "next" parametresinde yazan yere yönlendirecektir. Eğer login olmamışsak login sayfasına yönlendirecektir. İşte bu isteği  bir sekmeden yaptığımızda, login syafasına gidilmiyorsa, facebook'a login olunmuştur demektir. Fakat farklı sekemede farklı domainden request yapmamız güvenlik gereği engellenmiştir. İşte JSONP de olduğu gibi benzer bir trik uygulanıyor.  <IMG> elementi sayfaya (DOM'a) include edilirmiş gibi bir kod yazılıyor:

```html
<img onload="alert('logged in to fb')" onerror="alert('not logged in to fb')" src="https://www.facebook.com/login.php?favicon.ico">
```

Bu resim dosyasına web tarayıcıları her zaman farklı domaine izin veriyor. çünkü siteler normalde CDN kullanmaktadır video ve resimler için. Yukarıdaki örnekte https://www.facebook.com/login.php?next=www.facebook.com/myprofile/photo.jpg olduğunda dosya eğer login olmuşsa kullanıcı çekilmektedir. Dolayısı ile kullanıcının login olup olunmadığı, hatta bazen profil fotosu da çekilebilmektedir.

Bazı siteler tüm resimleri CDN'e yüklediklerinden işler daha da karışmaktadır ve bu tehlikeyi engellemektedirler. Fakat bazı sitelerin en çok kullanılan standart dosyası olan favico.ico resim dosyası çekilmektedir.

# Content Blocking (yada eski adı:tracking protection (yada eski adı:izlenme koruması))

- bu özellik başka bir çok tarayıcı eklentisinde de mevcuttur.

- başka domaine istek yapmamızı engellemektedir. Bu listeler firefox içerisine gömülmüştür (blacklist). Örneğin; facebook.com/next=facebook.com/# firefoxta, facebook haricindeki tüm sitelerden resim objesi içerisinden dahi engellenmektedir. böylece başka sitelere login olup olmadığımızı kimse takip edemeyecektir.

- firefox private mode'da Content Blocking'i açık tutuyor yada basit GUI seçeneklerinde normalde açılabilmesini sağlıyor.

# mixed content

https başlatılan bir sitede daha sonra http yüklenen bir dosya var ise bu duruma denir.

2 ye bölünür:

- active: css, html, js, iframe, flash gibi sayfanın yapısını değiştirebilecek tehlikeli dosyalar. bu dosyalar tarayıcılar tarafından engellenmektedir ve developer consola çıktısı kırmızı ile uyarı şeklinde verilmektedir.

- pasif: video, ses, resim gibi sayfanın sadece görünümünü değiştirebilecek dosyalar. bu dosyalar tarayıcılar tarafından engellenmiyor fakat developer console'a warning veriliyor.

# XSS (Cross Site Scripting)

Cascading Style Sheets (CSS) ile aynı kısaltmaya sahip olduğu için XSS olarak kısaltılır.

HTML formalarında, yada url'den giden parametreler manuel olarak değiştirilerek, sunucu tarafta SQL injection , yada HTML tarafında değişikliklere yol açan script'lerdir.

XSS saldırılarını engellemenin en temel ve köklü yöntemi HTML'de her karaktere gelen entity'leri kullanmaktır.

çeşitleri: (kaynak: https://www.owasp.org/index.php/Types_of_Cross-Site_Scripting (archive date: 09/11/2019) )

- # Stored XSS (AKA Persistent or Type I)

  kötü amaçlı kodların web sunucusunda veritabanında kayıt edildiği durumları temsil eder. "commentler" buna en iyi örnektir. buradaki datalar başka user'lar tarafından görüntülendiğinde kötü amaçlı kodlar çalışacaktır.

- # Reflected XSS (AKA Non-Persistent or Type II)

  bu tarz xss'ler sadece o andaki user için çalışır. yani genelde input veya url alanlarına yapılan girişler sonucunda, kotü amaçlı kodlar kendisine yansır. tabi saldırılar diğer kullanıcılara yapılacağı için, genelde başkasının bir yerlere tıklamasını sağlamak gerekir. örneğin sahte bir email atıp, kötü amaçlı kodların user'ın kendisi tarafından tetiklenmesi beklenir.

- # DOM Based XSS (AKA Type-0)

  DOM Manipülasyonu yapan XSS'lerdir.

# HTML character entities

  Bir string'in her karakteri (yada sadece özel karakterleri) html referansına çevirirsek ekranda her zaman bu metin gösterilecektir. fakat ekranda HTML olarak gösterilmesi gerekiyorsa işte o zamna riskler artmaktadır. buna örnek bir durum: gmail web arayüzünün bize html formatında atılan bir emaili, web browser'ımızda bir divin içerisinde göstermesi örnek verilebilir. işte bu gibi durumlarda verilen HTML string'ini safe hale getiren (eventlerini silen), bilinen XSS saldırılarını temizleyen kütüphaneler kullanılmaktadır. örnek kütüphane: https://github.com/ezyang/htmlpurifier 

  burada firefox html 4.0 için tüm character entity listesi vardır:

  https://www-archive.mozilla.org/newlayout/testcases/layout/entities.html (archive date: 08/11/2019)

  Birçok karakterin hem name hemde code olarak gösterimi vardır. örnek:

  | Result | Description                        | Entity Name | Entity Number |
  |--------|------------------------------------|-------------|---------------|
  |        | non-breaking space                 | &nbsp;      | &#160;        |
  | <      | less than                          | &lt;        | &#60;         |
  | >      | greater than                       | &gt;        | &#62;         |
  | &      | ampersand                          | &amp;       | &#38;         |
  | "      | double quotation mark              | &quot;      | &#34;         |
  | '      | single quotation mark (apostrophe) | &apos;      | &#39;         |
  | ¢      | cent                               | &cent;      | &#162;        |
  | £      | pound                              | &pound;     | &#163;        |
  | ¥      | yen                                | &yen;       | &#165;        |
  | €      | euro                               | &euro;      | &#8364;       |
  | ©      | copyright                          | &copy;      | &#169;        |
  | ®      | registered trademark               | &reg;       | &#174;        |

  Bazı karakterleri bir önceki karakterlerin üzerine ekleme de yapabiliyor:

  | Mark | Character | Construct | Result |
  |------|-----------|-----------|--------|
  | ̀     | a         | a&#768;   | à      |
  | ́     | a         | a&#769;   | á      |
  | ̂     | a         | a&#770;   | â      |
  | ̃     | a         | a&#771;   | ã      |
  | ̀     | O         | O&#768;   | Ò      |
  | ́     | O         | O&#769;   | Ó      |
  | ̂     | O         | O&#770;   | Ô      |
  | ̃     | O         | O&#771;   | Õ      |

  html 5'te birçok karakter daha destekleniyor:

  https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references (archive date: 08/11/2019)

  HTML ile artık karakterler unicode code point'i ile referans ediliyor. tüm unicode desteklenmemektedir. sadece belli bir kısmı destekleniyor.

  Fakat XHTML ile HMTL arasında bir ortak çözüm olmadığndan ve HTML geriye uyumluluğu için &amp;, &lt;, &gt;, &quot; and &apos; haricindeki karakterler normal yazılması önerilir. kaynak: https://wiki.whatwg.org/wiki/HTML_vs._XHTML (archive date: 08/11/2019)

# CSRF (yada Cross-Site Request Forgery yada XSRF)

yetkisi olmadan istek yapan http istek çeşitleridir. örneğin; yetkisi olmadan şifre değiştirmeye çalışan, yada yetkisi olmadan mesaj silen her HTTP isteği CSRF grubu altındadır. 

# CSRF vs XSS

xss sadece client side koşulan kötü amaçlı scriptleri kapsar. XSS'te yetki yada HTTP-istek konusu olmak zorunda değildir. xss, CSRF kümesini ve daha fazlasını kapsar.

# chrome guest mode vs private mode
- her iki modda da pencereler kapatıldığında bilgiler saklanmaz.

- private mode sık kullanılanlarda değişiklik yapabilir ve eklentileri kullanmaya devam edebilir. oysa guest mode, anında (gues isimli)  bir profil açar ve pencere kapatılınca o profili siler.

- google chrome'un gizli moduna "incognito mode" ismi verilmiştir. Incognito tütkçe anlamı: kılık değiştirmek, sahte ad.

# firefox profiles
firefox -p parametresi ile profile manager açılır. chrome'da olduğu gibi farklı profiller mevcuttur.

# Multi-Account Containers
firefox'un bir eklentisidir. bu eklenti ile aynı profile içerisinde her sekmenin birbirinin cookie'leri ve diğer storageleri hiç görmemesi sağlanabilir. örneğin 2 facebook hesabı farklı sekmelerde açılabilir.

# adblock vs adblock plus
sadece reklamları engellemek amaçlı yapılan tarayıcı eklentileridir. analitic için gönderilecek istatistik içeren kod bloklarını engllemektedir. bu şekilde zaten analitic istekleride engellenmiş olmaktadır. 2 farklı proje. fakat subscribe olunana engelleme filtreleri para verilip kaldırılabiliyor.

# uBlock Origin vs uMatrix
bu eklentiler tarayıcıdan content engellemesi için yapılmıştır. herhangi bir javascript'i yada nesneleri filtreler hazırlayarak engelleyebilmemizi sağlar. filtreler, diğer add-block eklentilerindeki filtreleri de desteklemektedir(uyumludur). bu iki eklenti ek olarak diğer addblock eklentilerine göre daha gelişmiş filtreleme özellikleri içeriyor.

# noscript
tarayıcıdaki özellikleri devre dışı bırakarak güvenlik sağlamakta

# ghostery
analitic firmalarının istek yapmasını engelliyor. fakat bu sonuçları kendi sunucularında topluyor ve yine engellediği firmalara satıyor.

# WOT (yada Web of Trust yada myWOT)
bu eklenti kullanıcılardan site güvenliği hakındaki oylamalarını toplamaktadır. ve bu oyları sürekli tarayıcının tepesinden göstermektedir.

# addblocker web sitemizdeki bir kaynağı engellerse ne yapmalıyız?
- "addblocker plus" eklentisi firefox developer menüsünde özel sekme ekliyor. buradan bir web sitesine gidildiğinde hangi objelerin engellendiğini görebiliyoruz.
- önce hangi filtrenin sitemizdeki kaynağı engellediğini bulmalıyız. bunun için addblocker eklentimizdeki listeleri tek tek disable edip her disable işleminden sonra web sayfamızı reload etmeliyiz. hangi filtrenin engellediğini tespit edersek, o filtrenin resmi sitesine gidip geliştiricileri ile iletişime geçmemiz gerekir.
- filtreler addblock eklentileri/uygulamalarından bağımsız olarak başkaları tarafından geliştirilir.
- eğer engellenen objeler reklam ise, geliştiriciler olumsuz dönüş yapacaktır.

# add blocker'a yakalanmamak için ne yapılmalıdır
- html veya kaynakların url'lerinde "add, addvertise, banner" veya bunları içeren string'ler var ise bu kaynakalrımız mutlaka engellenecektir.
- bazı reklamların tasarımı uygun ise, bazı filtreler bunları kaldırmıyor. uygunluğun birçok standardı var. bunlardan bazıları:
  - browser'da bellek tüketimine sebep olmaması
  - video olmaması
  - resim ise resim formatının düşük kalitede olması
  - resim ise ufak boyutta olması
  - ekranda son kullanıcıyı rahatsız etmeyecek pozisyonda olması
  - bazı filtreler resimleri de engelliyor. sadece text reklamları kabul ediyor (hafif olduğu için)
  - arkada çalıştırdğı js'in kullanıcı gizliliğine saygı duyuyor olması
  - ekranda ufak boyutta olması

Bazı filtreler +18 içerikleri de engelliyor. bu filtreler genelde iş yerlerinde aktif ediliyor.

# add blocker'ı algılamak
browser'da addblocker olup olmadığı kolaylıkla algılanabilir. hatta kullanıcının hangi filtreleri devreye almış olduğu bile tespit edilebiliyor. bunlar için birçok farklı yöntemler var. fakat bu yöntemler sürekli olarak değiştiriliyor, çünkü her tespit eden koda karşılık, addblocker'lar tarafından bunları engelleyen kodlar tasarlanıyor.

add blocker'ı algılamak için çok basit ve temel bir örneği ele alalım:
- /ads.js'e bir istek yaparız. eğer isteğimiz engelleniyor ise, kullanıcı addblocker kullanıyor demektir.
- sitemizde id'si "addvertising" olan bir div yaratırız. sayfa yüklendikten sonra bir js ile bu div'in içindekilerin yada div'in manüpüle edilip edilmediğine bakarız. eğer manüpilasyona uğramışsa, son kullanıcı addblocker kullanıyor demektir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HTML icin "Object" ve "embed" tag'leri
iki tag'de aynı görevi görmektedir. örnek kullanım:

```html
<object type="application/pdf" data="filename.pdf" width="100%" height="100%">
</object>
```

bu şekilde tarayıcının eklentileri çağrılabilir durumda olmaktadır. örneğil pdf, wsf (flash) gibi. tarayıcı "data" da belirtilen dosya uzantısına göre yada "type" ile belirtilen mime-type'a göre eklentiyi devreye sokar. son kullanıcı bu dosyaların neyle açılacağını tarayıcının özelliklerinden set etmiş olmalıdır. örneğin bir video açılacakken tarayıcının embed video görüntüleyiciside açılabilir, vlc'nin tarayıcıya kurduğu eklentide açılabilir.

HMTL5'te embeed tagı ile açılan video player'ın teması neredeyse tümüyle değiştirilebilir durumdadır. böylece herkes kendi player'ını tasarlayabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

| İSİM                                                                                         | JS ENGINE           | BROWSER ENGINE       |
|----------------------------------------------------------------------------------------------|---------------------|----------------------|
| internet explorer (11. sürümde geliştirmesi durduruldu. Artık Edge tarayıcısı kullanılıyor.) | ?                   | Trident(yada MSHTML) |
| Microsoft Edge (yada Spartan) (2018'in sonlarına kadarki sürümler)                           |                     | EdgeHTML             |
| Microsoft edge (yada Spartan)  (2018'den sonraki sürümler)                                   | chakra              | Blink                |
| firefox                                                                                      | SpiderMonkey        | gecko                |
| safari                                                                                       | Nitro*(1)           | WebKit               |
| google chrome                                                                                | v8 (yada Chrome V8) | Blink                |
| opera (14. sürüme kadar)                                                                     | Carakan             | Presto               |
| opera (14. sürümden sonra)                                                                   | v8                  | Blink                |
| yandex browser                                                                               | v8                  | Blink                |

- (1) Safari Js Engine
  - WebKit, JavaScriptCore ve webcore olarak 2 temel projeden oluşuyor.
  - JavaScriptCore, WebKit projesinin bir alt projesidir.
  - JavaScriptCore tekrardan birçok kere optimize edildi. Sırası ile yeni isimleri: SquirrelFish --> SquirrelFish Extreme (yada SFX yada Nitro)

# NodeJS
V8 tabanlıdır.

# SunSpider
benchmark(kıyaslama) tool that measure JavaScript performance.

# Netscape
Netscape firması tarafından geliştirilen tarayıcı. daha sonra açık kaynaklı oldu ve mozilla kurumu ile geliştirilmeye devam edildi. fakat mozilla daha sonra sıfırdan tarayıcı yazdı.

# Aurora
Firefox'un beta ile nightly version arasındaki sürümü.

# Firefox focus
firefox'un sürekli gizli modda açılan ve gömülü adblocker içeren versiyonu. lisans sorunları sebebi ile; almanca sürümlerinde "firefox klar" olarak adlandırılmaktadır.

# Firebug
Firefox'un içinde gelen default developer tool'un alternatifi açık kaynaklı bir eklentidir. artık geliştirilmiyor.

# Fennec
firefox'un tüm mobile versiyonları için kullanılan resmi bir takma isimdir.

# IceCat (yada eski adı: IceWeasel)
gnu projesi kapsamında firefox'un  içindeki lisanslı paketlerin çıkarılarak yaratılan forkudur. içerisinde varsayılan olarak eklentiler barındırıyor.

# Fennec F-Droid
Firefox f-droid gibi marketlerde yer almamaktadır ve public ftp indirme sunucusu sunmamaktadır. bu sebeple community, firefox'un kodunu sürekli derleyerek f-droid mağzasında bu isimde dağıtmaktadır.

# Waterfox
düzenli olarak firefox'tan fork olan proje. telemetrilerin disable edildiği, çok temel isteklerin yapıldığı proje.

# Palemoon
firefox tabanlı fakat firefox'u düzenli olarak fork etmeyen bir proje.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# submit request vs Ajax
when user click on the submit button, it sends a ajax request which can be only POST or GET and the server can redirect the user to another page with it's answer.

```html
<form method="post" action="/server_url">

  <input type="text" name="username">//name attribute data adding to request body if it is post. if the request is get when it will add to user pramaters.

  <input type="submit">Submit</input>
</form>
```

HTML standartlarında form içinde form olmaması gerektiği belirtilmektedir. kaynak: "Example of a form" başlığında https://www.w3.org/MarkUp/html3/forms.html (archive date: 11/12/2019)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

- hash mark (yada #)

- hash-bang (yada #!)

- ! (yada bang yada ünlem işareti yada Exclamation Mark)

- Fragment
a small part broken or separated off something

- URL'lerde sadece hash işareti ile linkler sayfadaki  bir yere yönlendirmek için (sunucuya istek olmadan) kullanılır. # ile yönlendirilen sayfalar "fragment identifier" aracılığı ile sayfanın hangi kısmına gideceğini bilirler.


- _escaped_fragment_=
Bu string google (ve diğer) arama robotları tarafından html sayfalarını gezerken kullanılmaktadır. Arama için indexlenecek sayfa url'sinde hash-bank var ise; ! işareti kısmını bu string ile değiştiriyor. Bu şekilde gidilen sayfa bu keyword'ü okuyor ve eğer bu keyword var ise static (içerde ajax işlemi yapmayan, robotlar javascript çalıştırır) bir html sayfası döndürüyor. Çünkü arama motorları dinamik sayfaları çalıştıramaz.

- \<meta name="fragment" content="!"> etiketi Ajax içeren sayfalara koyulur. Eğer robot bu syafaya gelirse bu etiketi görünce aynı url'ye _escaped_fragment_= ekleyip yeni url'ye gider.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# html 5 yenilikler

- table footer gibi div yerine kullanılacak hazır tasarım şablonları

- video ve audio elemenleri geldi. bunlarla flash yada eklenti ihtiyacı kalktı.

- bazı elementler ve attribute'ler çıkarıldı, ve yenileri eklendi.

- form elementler için tipler geldi.

- canvas

- offline web app (web-cache or app-cache): cache mekanizması. artık manifest dosyaları ile hangi dosyaların tamamen offline hangilerinin ise kesinlikle internetten çekilebileceği belirtilebiliyor. \<html manifest="cache.appcache"> ile dosya verilmelidir. web cache daha sonra standartlardan kaldırıldı. html5 öncesi tarayıcılarda hiçbir şekilde cache mekanizması yoktu. ya özel javascript kütüphaneleri ile kendi cache mekanizmaları yazılıyordu, yada web tarayıcıları hiçbir standarta bağlı olmadan kendilerince cache mekanizması uyguluyorlardı. bu sebeple bazı yazılımcılar web geliştirmesi yaparken bilinçsizce cache temizliği yaparlar.

- drag and drop

- history api: artık url'lerde hash # tag'i kullanımı yerine direk oralarak html 5in getirdiği history kullanılmaktadır. HTML 5 ile artık URL değişmeden sayfanın yenilenmesi sağlanabiliyor.

  html5 öncesi sayfa iki şekilde değişebiliyordu: 1-hastag vs 2-url ile gidilen yeni sayfa.

  html5 ile gelen history mekanizmasında hashtag ihtiyacı kalkmıştır.

  window.history.pushState(stateObj, "page 2", "bar.html"); Yeni sayfa aynı url olmak zorunda değil. Aynı url olmadığında da sayfanın state'i yenilendiği için hastag'lere gerek kalmamıştır. parametreler:
  
  - stateobj: yenilenen sayfada event listener ile çekilebilmektedir.
  - "page 2": title. bu deger ignore ediliyor. boş gönderilebiliyor.
  - "bar.html": relative path olmalıdır. çünkü aynı domainde ancak history olabilir.

- web storage (local + session storage): anahtar - value mantığı ile çalışır.

- web mesaging: iframe içerisinde event listenlerlarla (message event'i ile) data dönderimi ve alımı yapılıyor.

- Web Worker: js thread mekanizması. aksi durumlarda tarayıcı son kullanıcıya "not repsoding" mesajı verebiliyor.

- websocket (başka başlıkta anlatılıyor)

- gecolocation: locasyon tespiti için API.

- web sql database: sorgu yapmayı sağlayan database.

- indexed db : data çok anahtar ile veri tutma işlemleri için tercih ediliyor. nosql gibi düşünülebilir.

- webrtc: tarayıcı ile  bir tarayıcı arasında direk iletişim protokolü.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# websocket

socket'lere bağlanamazlar. Karşı tarafında websocket kullanması şart. tcp üzerine kurulu http alternatifidir.

browser to browser haberleşme mümkün diil, çünkü browserda server-web-socket açılamaz. 

server taraf istediği port'ta ve path'te web socket'i sunucu modunda açabilir.

Firefox developer tool içinde websocket'leri takip edebilmek için bir özellik mevcut değil. Chrome'da bu mevcut (yıl 2018). CHrome'da network sekmesinde ilgili soket işleminin içinde "Frame" sekmesinden görülmektedir.

protocol: http alternatifi olduğu için http yerine 'ws' kullanılıyor. wss ise secure (ssl) kullanan versiyonudur.

websocket tam iki taraflı iletişimi sağlar. bir request'e karşılık response olmak zorunda değildir. iki tarafta istediği kadar ve istediği zaman mesaj atabilir. bu tarz iletişimlere duplex (yada tr:dubleks, türkçe kelime anlamı: çift(dual) ) iletişim protokolü denir. 

websocket bağlantısı kurulduğunda client sunucya önce bir http isteği gönderir (80 portu diil. direk websocket'in bağlanacağı port'a yollar bu isteği):

örnek:

```
GET ws://websocket.example.com/ HTTP/1.1

Origin: http://example.com

Connection: Upgrade

Host: websocket.example.com

Upgrade: websocket
```

Burada websocket'e geçiş yapmalarını söyler. sunucda bunu kabul eder:

```
HTTP/1.1 101 WebSocket Protocol Handshake

Date: Wed, 16 Oct 2013 10:07:34 GMT

Connection: Upgrade

Upgrade: WebSocket
```

Bu istekten sonra artık aynı tcp bağlantı üzerinden sürekli stream'in yapılır.


# Message vs fragment

fragment türkçe kelime anlamı: parça

API'lerin sunduğu onMessage eventlerimiz her message geldiğinde tetiklenir. her message arkaplanda en az bir adet frame denilen parçalardan oluşur. frame'ler şu akışta daha basit anlaşılabilir:

```
Client: FIN=1, opcode=0x1, msg="hi"

Server: bir mesaj geldi: Hi

Client: FIN=0, opcode=0x1, msg="ben"

Server: mesaj dinlenmeye devam ediyor...

Client: FIN=0, opcode=0x0, msg="ahmet"

Server: mesaj dinlenmeye devam ediyor...

Client: FIN=1, opcode=0x0, msg="doganoglu'yum."

Server: bir mesaj geldi: ben ahmet doganoglu'yum.
```

- FIN=1 ve FUN=0 mesajın devam edip etmeyeceği bilgisini yolluyor.

- opcode=0x1 text mesajı atıldığını, opcode=0x2 binary mesaj atıldığını, opcode=0x0'ın ise bir anlamı yok. opcode=0x0 zaten önceden belirlenmiş mesaj tipinin devamında gelen frame'ler tarafından kullanılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# web 1.0, 2.0, 3.0
web dünyasını belirleyen standartlardır. web 1.0 sadece html'in olduğu fakat ajax işleminin dahi gerçekleşmediği web standartlarını temsil eder. 2.0 ise 2003'ten itibaren olan tüm standartların tümünü temsil eder. 3.0 standartları henüz belirlenmedi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# canvas (yada Tuval) vs svg (yada Scalable Vector Graphics)
svg her çözünürlüğe uygundur.

# svg örneği:
```xml
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>
```

# canvas örneği:

```js
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #000000;"></canvas>

var c = document.getElementById("myCanvas");

var ctx = c.getContext("2d");

ctx.fillStyle = "red";

ctx.fillRect(0,0,150,75);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SASS
Sass, CSS'i bir programlama diline benzer yapıyla geliştirmemizi sağlayan, sürekli ihtiyaç duyduğumuz ve CSS'te bulunmayan birçok özelliği kullanarak, daha pratik ve okunaklı kod yazmamıza olanak verir.

Sass ile CSS yazarken; değişkenler (variables), döngüler (for, each, while), karar yapıları (if, else), fonksiyonlar kullanılabilir.

SCSS ve SASS iki farklı syntax sunan formata sahiptir. bu kodlar bir tool aracılığı ile derlenerek css formatı oluşturulur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mustache vs underscore vs lodash

bu iki javascript kütüphanesi de hazırlanan template dosyalarını verilen model ile html'e render etmeye yaramaktadır.

underscore ek olarak jquery gibi fonksiyonel fakat dom'dan bağımsız metodlar içerir. kolay döngüler kurar, array içerisinde koşulları kontrol etme metodları gibi.

lodash; underscore'a alternatif açık kaynaklı kütüphane. underscore'dan daha fazla özellik içeriyor. underscore daha core çok metodlar içeriyor.

ecmascript'in 6'ıncı versiynu ile artık underscore veya lodash ile yapılan %80 özellik default olarak sağlanabiliyor. underscore ve lodash'in kullanılmasının sebebi eski tarayıcılarda da aynı metodların desteklenmesi ve %20'luk ecmascirptte olmayan fonksiyonların kulanılmasıdır.

# jquery vs jquery mobile vs jquery ui
jquery, javascript için temel metodları içeren kütüphanedir.

jquery ui ise date-picker, drag-drop gibi görsel bölgelerde düzenlemeler sağlayan kütüphanedir.

jquery mobile ise, mobile platformlarına uygun davranışlar sergilemesi için gerekli metodları içerir. örneğin mobile'de farklı eventt'ler varken, desktopta farklıdır. jquery html elementlerine verilen attribute'ler ile sayfada nasıl duracağına vs karar verebilir. bu durumlar için mobile'e daha uygun bir kütüphane gerekir. bu sebeple jquery-mobile kullanılmalıdır.

jquery eklenti altyapısına sahiptir.

# knockout js
javascript mvc kütüphanesidir.

# backbone js
javascript mvc kütüphanesidir.. fakat angular ve knockout'a göre core bir framework'tür. herşeyi kendi başınıza tasarlamanız gerekir.

Backbone contoller isminde bir sınıf içermez. bu sebeple bazı yerlerde MV bazlı bir kütüphane olduğu yazar. fakat backbone MVC'dir. çünkü backbone-View objesi aslında, mvc'deki contoller'ın görevini karşılamaktadır. MVC'deki View'ı ise; backbone-template'leri görmektedir. Template'ler backbone-View içerisinde oluşturululduğundan yanlış algılamaya sebep olabilmektedir.

# bootstrap
bootstrap, UI objeleri yaratmaya yarayan bir kütüphanedir.

"boostrap ui" ise, angular modülüdür. birçok bootsrap nesnesi angular ile yazılmıştır. bu şekilde angular ile daha uyumlu çalışılabilir. bootstrap jquery'ye depend ederi. oysa bootstrap modülü etmez. bu sebeple projeye jquery include etme zorunluluğu ortadan kalkar.

# bootstrap css vs bootstrap js
Bootstrap gösrselleştirme kütüphanesi. Fakat js dosyası bazı ek özellikler (genelde animasyon işlemleri) sunmak için isteğe bağlı kullanılabilir. bootstrap js, jquery'ye depend eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# jquery version history

| version | release date       | latest version and its release date | note                                                                          |
|---------|--------------------|-------------------------------------|-------------------------------------------------------------------------------|
| 1.0     | August 26, 2006    |                                     | First stable release                                                          |
| 1.1     | January 14, 2007   |                                     |                                                                               |
| 1.2     | September 10, 2007 | 1.2.6                               |                                                                               |
| 1.3     | January 14, 2009   | 1.3.2                               |                                                                               |
| 1.4     | January 14, 2010   | 1.4.4                               |                                                                               |
| 1.5     | January 31, 2011   | 1.5.2                               |                                                                               |
| 1.6     | May 3, 2011        | 1.6.4                               |                                                                               |
| 1.7     | November 3, 2011   | 1.7.2 (March 21, 2012)              | New Event APIs: .on() and .off(), while the old APIs are still supported.     |
| 1.8     | August 9, 2012     | 1.8.3 (November 13, 2012)           |                                                                               |
| 1.9     | January 15, 2013   | 1.9.1 (February 4, 2013)            |                                                                               |
| 1.10    | May 24, 2013       | 1.10.2 (July 3, 2013)               |                                                                               |
| 1.11    | January 24, 2014   | 1.11.3 (April 28, 2015)             |                                                                               |
| 1.12    | January 8, 2016    | 1.12.4 (May 20, 2016)               |                                                                               |
| 2.0     | April 18, 2013     | 2.0.3 (July 3, 2013)                | Dropped IE 6–8 support for performance improvements and reduction in filesize |
| 2.1     | January 24, 2014   | 2.1.4 (April 28, 2015)              |                                                                               |
| 2.2     | January 8, 2016    | 2.2.4 (May 20, 2016)                |                                                                               |
| 3.0     | June 9, 2016       | 3.0.0 (June 9, 2016)                |                                                                               |
| 3.1     | July 7, 2016       | 3.1.1 (September 23, 2016)          |                                                                               |
| 3.2     | March 16, 2017     | 3.2.1 (March 20, 2017)              |                                                                               |
| 3.3     | January 19, 2018   | 3.3.1 (January 20, 2018)            |                                                                               |
| 3.4     | April 10, 2019     | 3.4.1 (May 1, 2019)                 |                                                                               |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# angular js
DOM manipülasyonları jquery gibi kütüphanelerle yapılıyor. Fakat MVC kavramını client tarafta uygulamak için angular gibi kütüphaneler kullanılmaktadır. angular mvc yapısı ile çalışır.

- #### sürüm

angularjs, 2inci sürümü ile sıfırdan yazıldı ve yeni bir siteye taşındı (angular.io). angular geriye uyumlu yazılmadı.

3üncü sürüm, isim çakışması sebebi ile atlandı. 2'den sonra direk 4 üncü sürüme çıkıldı.

| sürüm no | çıktığı yıl |
|----------|-------------|
| 2        | 2016        |
| 4        | 2017        |
| 5        | 2017        |
| 6        | 2018        |
| 7        | 2018        |
| 8        | 2019        |

Asagidaki tüm bilgiler angular 1.6 baz alınarak yazılmıştır.

- #### jquery

  - angular js, jquery'ye ye depend etmez. kendi içinde jQuery Lite'ı (jqLite) bulundurur. eğer jQuery var ise, angular jqLite yerine jQuery kullanır. jqLite sadece angular ihtiyacı metodları ve en temel jquery metodlarını sunar.

  - fonksiyonlara geçilen "element" objeleri her zaman jquery objesine wrap edilmiş olarak geçilir. örneğin;


  ```js
  angular.module('myModule', [])

    .directive('failswithoutjquery', function() {

      return {

        restrict : 'A',

        link : function(scope, element, attrs) {

                 element.hide(4000);
               }
      }
  });
  ```

  Yukarıdaki örnekte element objesi jquery objesi. hide elementi ise jquery lite'ta yok. bu sebeple eğer jquery inject edilmemişse bu kod bloğu hata verecektir.

  angular.element DOM objesinin element (jquery ile wrap edilmiş) olarak dönmektedir.

- #### modülerlik

  angular js eklenti desteği mevcut değil. çünkü her şey bir angular modülü olarak tanımlanabiliyor ve başka modüller tarafından çağrılabiliyor. örneğin routing ayrı bir modüldür ve ayrı inject edilir.

  yazılım geliştirici yeni modüller ekler. bir sayfada birden fazla modül olabilir. her modülün altında birden fazla component, directive ve controller olabilir.

- #### Component

kullanımı:

```xml
<w3-test-directive></w3-test-directive>
```

yada

```xml
<div w3-test-directive></div>
```

yada

```xml
<div class="w3-test-directive"></div>
```

yada

```xml
<!-- directive: w3-test-directive -->
```

tanımlanması:

```js
var app = angular.module("myApp", []);

app.directive("w3TestDirective", function() {

    return {

        template : "<h1>Made by a directive!</h1>"

 };
});
```

- #### watch variables

two-way binding olduğundan; herhangi bir dinamic değer değiştirdiğinde onu referans eden tüm değerlerde anında değişmektedir. fakat javascript'te bir değer console'danda değiştirilebilir. bunu angular nasıl anlıyor?

angular bunları  anlayamıyor. bu sebeple click metodu yerine ng-click gibi alternatifler mevcut. çünkü her angular stanardındaki metodların çağrılması sonrası tüm değerler manuel kontrol ediliyor. yani console'dan, daya on-click eventi üzerinden bir değer değiştirildiğinde angular bunu göremiyor.

bu sebeple şöyle bir sorun da ortaya çıkıyor:

contoller içinde async bir thread üzerinden (timeout, ajax call, cordova pugin'in return callback'leri gibi) bir dinamic değer değiştirildiğinde, contoller'ın main thred'inden sonra çalışırsa controller'ın haberi olmuyor. bu sebeple $scope.apply() metodu geliştirilmiştir. bu metodu çağrıldığında tüm dinamic değerler angular tarafından tekrar kontrol edilmektedir. örneğin; ajax-callback thred'in sonunda apply() methodu çağrılabilir.

angular $http modülü ajax işlemlerini yapmak için geliştirilmiştir. bu metdun callbakclerinde otomatik olarak apply metodu çağrılır. bu sebeple yaızlımcının bu metodu çağırmasına gerek yoktur. fakat jquery.ajax yada javascript core ajax işlemi yapıyorsa apply metodu şarttır.

apply() metodu isteğe bağlı bir metod parametresi de alabilir. bu metod parametresi içindeki fonksiyon çalışmasını bitirdiğinde apply işlemini gerçekleştirir.

- #### watch metodu

```js
$scope.myVar= "estVar";

$scope.$watch('myVar', function() { 

      alert('hey, myVar has changed!');

});
```

$watch metodunda apply() işlemi sırasında değişen variable'ler için event yaratabilmemizi sağlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# web component
tarayıcılar tarafından yeni desteklenmeye başlanan özellik. custom htmle elemantleri yaratmamıza yarıyor. kendi eventleri ve içinde birçok html elementi barındırabiliyor. <div class="my element"> gibi varolan alternatif çözümler ile neredeyse aynı kapıya çıkıyor. fakat okunabilirlik açısından çok daha ileri seviyede olmaktadır.

yazdığımız bir html tag'ini register etmezsek (tarayıcının desteklemesi gerekir) DOM üzerinde aktif olamaz fakat yinede tarayıcı hata vermediği için çalışmaya devam edebilir. fakat web component'ini destekleyen bir tarayıcı ise, yeni tip html objemizi tarayıcıya register ederiz ve gerçek bir html objesiymiş gibi kullanabilir (best practise uygun).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HTTP Basic Authentication
http ile istek yaparken auth mekanizmasının en kolay ve standartlaşmış şeklidir. sadece requestin headırınına base64 tipinde username ve password bilgisi girilir. yapılan http isteğine eger bilgiler dogru ise cevap dönülür. standrat bir güvenlik şekli olduğundan, bazı web tarayıcıları bunu destekler ve popupta şifre ve kulanıcı adı sorar.

Basic auth işlemi her HTTP requestte yapılır. yani headera sadece ekleme yapılır. web tarayıcıları localde bir site için cookie olarak saklar request ile giden değeri. bu sebeple her defasında sormaz username ve passwordü. aynı şekilde örneği javada client objesi ile birçok istek yapabiliyoruz. aynı obje ile bir kere istek authantike olunduktan sonra cookieleri client objesi tutuyor ise bir daha request gerekmeyebilir.

# HTTP Digest authentication
Diggest auth'ta, önce, client sunucuya güvenlik sorgusundan geçeceğini belirten bir istek yapar. Bu isteğe cevap olarak sunucu, tek kullanımlık bir anahtar değer döndürür. Client tarafı hem bu değeri hemde MD5 algortimasını kullanarak şifreleyip sunucuya gönderir (ek olarak; client tarafı da bir tek kullanımlık anahtar kullarnı + URL adresi de şifrelemede kullanır …). Sunucu kullanıcı adı ve şifre ile aynı algoritmayı kendi uygular ve sonucun aynı olup olmadığına bakarak onay verir.

MD5 geri çevrilemez bir algoritma olduğu için güvenlik olarak basit auth'tan daha başarılır.

standart bir güvenlik şekli olduğundan, bazı web tarayıcıları bunu destekler ve popupta şifre ve kullanıcı adı sorar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# WebGL
Tarayıcılarda plugin gerektirmeden 3d ve 2d oyunlar, nesneler yaratılmasını sağlayan javascript API'sidir. OpenGL ES altyapısını kullanmaktadır.

# OpenGL
AÇık kaynaklı DirectX alternatifidir.

# OpenGL ES (yada OpenGL for Embedded Systems)
OpenGL'nin cep telefonu gibi gömülü sistemler için tasarlanmış versiyonudur.

# NativeClient (yada NaCl)
AÇık kaynaklı geliştirilen API. Bu API ile eklentiye gereksinim olmadan WebGL gibi 3d ve 2d modeller hazırlanabiliyor. Tek farkı herhangi bir proramlama dili ile yazılıp derlenmiş olan dosyanın tarayıcı tarayından (derlenmeye gerek kalmadan) direk olrsaka execute edilebilmesidir.Performans ve basitlik (javascript ile yazmak yerine diğer dillerle de yazabilme) açısından WebGL'den çok daha başarılıdır. NaCl, PPAPI destekli bir eklentidir.

# Plugin Application Programming Interface (yada PAPI)
Tarayıcıların native işletim sisteminden yararlanması için tarayıcı tarafındaki API.

# Netscape Plugin Application Programming Interface (yada NPAPI)
Birçok tarayıcı desteklemektedir, ilk Netscape çıkardığı için ismi böyle kalmıştır. Artık NPAPI özelliği çoğu tarayıcıdan kaldırılmaktadır.

# Pepper Plugin API (PPAPI)
Google'ın geliştirdiği, NPAPI'nin türevidir. Daha çok portabilite ve  işletim sistemi processinde çalışmaya yönelik geliştirmeler içermektedir.

chromium based browser's built-in PDF viewer is a plug-in which based on PPAPI.

# ActiveX
Microsoftun Internet explorer tarayıcısı için geliştirdiği PAPI. Microsoft Edge (windows 10 ile gelen yeni tarayıcı), ActiveX'i desteklememektedir.

# Silverlight
Adobe Flash alternatifi açık kaynaklı Microsoft'un geliştirdiği yazılım. Lisans sebeple ile silverlight kullanmayanlar projeyi forklayarak Moonlight projesini oluşturmuştur. fakat bu 2 projeninde geliştirilmesi durdurulmuştur.

# WebExtensions
Firefox 57inci sürümü ile zorunlu kıldığı yeni eklenti API'sinin ismidir. BU API chromium tabanlı tüm tarayıcılarda (örnek: Opera) da kullanıldığı için cross-browser eklenti yazılmasını sağlamaktadır.

############################

############################
# NETWORK
############################

############################

# Captive Portal (Kısıtlama Portalı)
Captive portal tekniği HTTP istemcisini İnterneti normal olarak kullanmadan önce ağ üzerinde özel bir Web sayfasını görmeye zorlar. Kısıtlama Portalı Web tarayıcısını kimlik denetleme cihazına çevirir. Bu, kullanıcı bir tarayıcı açıp İnternete erişmeye çalışana kadar porttan veya adresten bağımsız olarak bütün paketleri engelleyerek olur. Bu sırada tarayıcı kimlik denetleme ve/veya ödeme, ya da basitçe uygun kullanım poliçesini görüntüleyip kullanıcının onaylamasını sağlayacak Web sayfasına yönlendirilir. Kısıtlama Portalı uygulamaları en çok Wi-Fi erişim noktalarında kullanılır. Ayrıca kablolu kullanımı kontrol etmek için de kullanılabilir. (Otel odaları, iş merkezleri, vs.). kaynak: https://bidb.itu.edu.tr/seyir-defteri/blog/2013/09/07/captive-portal-(k%C4%B1s%C4%B1tlama-portal%C4%B1)(archive date: 31/12/2019)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DNS over TLS (DoT) vs DNS over HTTPS (DoH)
Standart DNS client'lar, DoT ve DoH sunucularına istek yapamazlar. çünkü farklı protokollerdir. satndart dns'te isteğin çoğu açık gider/gelir. oysa DoT ve DoH 'ta istek tamamen şifrelenmiştir.

DoT soket bağlantısı açıp, o soketi üzerinden TLS ile dns sorgusu atar.  DoH ise http protokolünden yararlanarak DNS isteği atar.

Bu iki ptotokolün amacı ISP'lerden gizlenmek değildir. çünkü ISP, dns sorgusundan sonra, gideceğimiz IP adresini zaten görecektir. buradaki amaç;

- man-in-the-middle-attack'ı önlemek
- dns sorgusu yapılıp, cevabı şifreli bir bağlantıda kullanılacak ise güvenliği sağlamış oluruz
- ISP'lerin DNS sunucularındaki yaptığı engellemeleri ortadan kaldırmak

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# network interface

wifi, kablolu, loopback, sanal network interfaceleri aynı makinada olabilir.

java ile bunların listesi şu metod'dan geliyor: NetworkInterface.getNetworkInterfaces();

istediğimiz network interfacesinden soket açabiliriz. bir soketi o network adresine bind etmemiz yeterli olacaktır.

bazı interfaceler:

- eno1 yada em1 onboard or embedded network card (genelde anakart içinde olan kartlar)

- eth# genelde anakart dışında ekstra bir kart olduğunda

- w# wireles

- lo loopback

- vbox# virtualbox'un kendi içinde yarattığı networkler

- virbr# libvirt (kvm için sanal makine kütüphanesi) için kendi içinde kullandığı networkler

- docker# dockerın kendi içinde kullandığı networkler

bu isimleri driver'ın kendisi belirliyor. donanım değil. örneğin aynı donanımı, üçüncü parti geliştirilen açık kaynaklı ve daha sonra kendi resmi sitesindeki driver ile kullandığımızı varsayalım. ikisinde de farklı isim olabilir. eğer driver'dan bu isim algılanamaz ise; işletim sistemi kendi standartlarını kullanıyor. bu sebeple isimlendirmede kesinlik söz konusu değildir. birçok çeşit mevcut: fiberoptikler ayrı, kartın modeline göre yada bir özelliğine göre isimlendiren driver'ler de mevcut.

bir ağdayken her bilgisayarın değil, her network kartının bir ip'si vardır. işletim sisteminde bir yazılım uzak makineye bağlantı açsın. bu bağlantı sadece bir interface üzerinden sağlanabilir. eğer birden fazla NI varsa, destination ip'mize göre işletim sisteminde tanımlı olan routing table'lar aracılığı ile yazılım ilgili NI'a yönlendirilecektir. NI yollanacak datayı yollanacak yere ulaştıracaktır. fakat isterse yazılımlar diğer interfaceleri kullanabilir. NetworkInterface.getNetworkInterfaces() kodu ile interface listesi döndüğünde, listeden istediğimiz elemanı alabiliriz. array döndüğü için interface name'i önceden bilmek zorunda değiliz.

```java
Socket socket = new Socket();
socket.connect(new InetSocketAddress(remote_ip, port));
```

Yukarıdaki yerine aşağıdakini yapabilirsak, hangi interfaceden dışarıya datayı ileteceğimize biz karar vermiş oluruz:

```java
NetworkInterface nif = NetworkInterface.getByName("wlo0"); // interfacemizi önceden bildiğimiz varsaydık (yoksa ilk elemanı alabilirdik)
Enumeration<InetAddress> nifAddresses = nif.getInetAddresses(); // NI'ın IPv4, IPv6 adresleri array olarak dönmektedir.

Socket socket = new Socket(); //bu satır değişmedi
socket.bind(new InetSocketAddress(nifAddresses.nextElement(), 0)); // ipv4 adresi ile soketimizi bu NI'a bind etmiş oluruz. bu kısım olmazsa OS routing table'lara bakarak bizi uygun NI'ye attach edecekti.
socket.connect(new InetSocketAddress(remote_ip, port)); //bu satır değişmedi
```

# network interface card (yada NIC yada network interface controller)

network interface'ine denk gelen fiziksel kart.

# Loopback interface

ipconfig komutu ile görelebilen bu interface, tamamen sanal bir ağ bağdaştırıcıdır. bir donanımmış gibi davranır. Local makinanın DNS kayıtlarında ip adresi: 127.0.0.1'dir. Bu IP'nin hostname'i localhost'tur. bilgisayarın kendi içinde haberleşmesi için (örneğin local sunucuya istek yapmak için) kullanılır. cihazda hiç network kartı olmasa bile loopback vardır.
# multiple ip address to one interface

```java
NetworkInterface ni = NetworkInterface.getByName("wlp4s0");

ni.getInetAddresses(); //bu metod bize bir liste döner. bu listede ipv4 ve ipv6 adresleri vardır. bazen network manager sadece ipv4 yada ipv6'yı atamış olabilir.

ni.getInterfaceAddresses(); //getInetAddresses() ile aynıdır. ekstra olarak ip bilgisi yerine network class bilgisini de döner.
```

# virtual interface vs subinterface

örnek: A interface'inin sub-interface'leri olabilir. A'ya gelen istekler, hem A hemde onun sub'larına klonlanarak dağıtılır.

her subinterface, bir virtual interface'dir. oysa tersi her zaman doğru değildir.

# server socket vs client socket

```java
//sunucu taraftaki kod

ServerSocket serverSocket = new ServerSocket(8089); 

Socket clientSocket = serverSocket.accept(); //bu metod sürekli bekliyor

clientSocket.getPort(); // returns random port number (X no'lu port olsun)

clientSocket.getLocalPort(); // returns 8089

//client taraftaki kod:

Socket socket = new Socket(IP_OF_SERVER, 8089);

socket.getPort(); // returns 8089

socket.getLocalPort(); // returns random port - yukarıdaki ile aynı port numarası (X)

yukarıda görüldüğü gibi; accept işlemi sonrası hala haberleşme server-soket portu (8089) üzerinden devam ediyor. fakat paralelden server-soket o porttan dinlemeye devam ediyor. 
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DNS (yada Domain Name System)
internet adres resolving sistemini temsil eden genel bir terimdir. 

__DNS Server (yada Name Server)__ ise IP'leri resolving yapmakla yükümlü yazılımdır. Bazen __Domain Name Server__ denir fakat yanlış bir kullanımdır.

# International Corporation for Assigned Names and Numbers (yada ICANN)
2019 yılı itibariyle en yetkili kuruluştur. Amerika'ya bağlıdır. __registrar (yada kaydedici)__ firmalar (örnek: godaddy) ICANN'dan lisans alıyorlar.

# IANA (yada Internet Assigned Numbers Authority)
ICANN için gerekli bazı teknik konuları tamamlayan kuruluştur.

# RIR (yad Regional Internet Registry)
ICANN ve IANA, dünyanın 5 farklı bölgesinde yetkilendirilmiş kuruluşlar vardır. Bu kuruluşlar kendi bölgelerinden sorumludur.

- AfriNIC
African Region Internet Registry: Afrika

- APNIC
Asia Pacific Network Information Centre: Asya ve Pasifik

- ARIN
American Registry for Internet Numbers: Kuzey Amerika

- LACNIC
Latin America and Caribbean Internet Addresses Registry: Latin Amerika ve Karaibler bölgesi

- RIPE NCC
Réseaux IP Européens Network Coordination Centre: Avrupa bölgesi.

RIR alanları yukarıda yazıldığı kadar keskin çizgilerle ayrılmaz. Örneğin, ARIN bölgesi esas olarak Kuzey Amerika olmakla beraber Güney Amerika ve Afrikaya da servis verir. RIPE bölgesi esas olarak avrupa olmakla beraber ortadoğu ve orta-asya içlerine kadar servis vermektedir. Türkiye RIPE NCC bölgesindedir. RIPE NCC'nin merkezi Hollanda’dadır.

# LIR (yada Local Internet Registry)
kullanıcıya IP veren kuruluşlardır. ISP (yada Internet Sevice Provider)'lere denk gelir. Yasal anlamda şirketi olan herkes bu görevi alabilir.

# root servers
DNS sunucuları tüm domain bilgilerini saklayamaz. local cache'inde yok ise, root server'lara sorar. root server dünyada 12 adettir (yıl 2019). bunlar ICANN tarafından belirlenmiştir. fakat sadece 1 tanesi ICANN kuruluşa direk bağlı bir sunucudur. bu sunucuya __ICANN Managed Root Server (yada IMRS)__ deniyor. diğerlerinin ICANN'a bağlı olmamasının sebebi, merkezi yönetimden kaçınmaktır.

ICANN'ın resmi sayfası olan www.dns.icann.org sitesinde root-servers.org'a referans verilmiştir. buradan 12 server'ı görebiliriz.

12 root-server'ın bilgileri DNS server yazılımlarında 'Root Hints File'a kaydedilir.

# whois server
registrar'a yeni bir domain eklendiğinde, registrar bu bilgiyi o domainin uzantısının sorumlusu olan "whois server"'a yollar. örneğin .com ve .net alan isimleri için verisign şirketinin whois sunucusu merkezi sunucudur. daha sonra bu bilgi diğer üçüncü parti whois server'lar tarafından klonlanır.

Dolayısı ile; root server'lar dahi .com ve .net için gerekli tüm domain'leri local'lerinde tutmazlar.

Versign firması ICANN'a bağlıdır ve ICANN tarafından yetkilendirilmiştir.

ICANN'dan her firma godaddy gibi kolayca yetki alamaz. fakat godaddy'ye kolayca aracılık edebilir, yani godaddy'nin bayisi olabilir.

# hostname vs domain name
domain name bir bilgisayara DNS sunucusu tarafından atanmış string'dir. oysa hostname local bir network'te her makinanın kendisini dışarıya tanıttığı isimdir. network içinde hostname kullanılırken, dışarıya gidişlerde domain kullanılır. dışardaki bir yere giderken, oradaki networkteki bir alt makineye erişmek istediğimizde hostname.domainname.com gibi bir adresten yararlanılır. hostname ve domain name aynı görevi görür. www internet tarayıcıları için gerekli varsayılan makinanın hostname'idir.

# subdomain
mail.google.com, map.google.com'deki mail ve map birer subdomain'dir. ayrı IP'leri vardır. fakat istenirse aynı IP'ye de yönlendirilebilir.

# domain'i hosting'e bağlama
wix.com bizim sayfamızı barındırıyor (sunucu hosting firmamız) olsun. godaddy.com'dan da domainimizi almış olalım.

- wix.com'a domain-name'imizi bildiriyoruz.
- wix.com bize wix'in kendi dns'lerini veriyor. bunlar genelde ns1.wix-dns.com tarzında URL'ler oluyor. 
- godaddy'de o domaine ait nameserver'larımızı tanımlıyoruz. bunun için godaddy'ye login oluyor, ilgili domainimizi seçiyor, ayarlarına giriyor ve nameserver alanlarımızı dolduruyoruz. burada birden fazla nameserver olabilir. çünkü yedek nameserver olabiliyor.

Artık domanimize istek yapıldığında, godaddy dns sorgusunu yapanı ns1.wix-dns.com'a yönlendiriyor. ns1.wix-dns.com gelen sorguyu görüyor ve onu sunucu bilgisayarın IP'sini dönüyor.

Bu örnekte godaddy'ye veridğimiz dns sunucuları bizim kendi kişisel sunucularımız olabilir. Genelde büyük firmalar kendi dns suncuularını kullanıyor.

# fully qualified domain name (yada FQDN)
hostname + domain name birarada olduğunda verilen isimdir. örnek: tr.wikipedia.com, www.wikipedia.com...

# top-level domain (yada TLD)
.com, .org, tr gibi terimlere verilen isimdir. TLD'nin çeşitleri vardır:

- # country code top-level domain (ccTLD)
  .tr, .fr, .de, .us gibi ülke bazlı gruplma için gerekli domain bilgisini ifade eder.

- # generic top-level domains (yada gTLD)
  .com, .org gibi kurumların tipini belli eden domain ifadeleridir.

# DNS sorgu tipleri

  - # Özyinelemeli (yada Recursive) Sorgu
    dns sunucusuna yapılan sorgu isteğidir. örneğin browser'ın ISP sunucusuna yaptığı sorgu isteği.

  - # Tekrarlamalı (yada Iterative) Sorgu
    DNS sunucuları kendi local'lerinde dns bilgisini bulamaz ise diğer dns sunucularına sorabilir. işte bu sorgular (dns'lerin kendi aralarında kullandığı sorgular) Tekrarlamalı Sorgu olarak adlandırılır.

# Ters DNS Çözümlemesi (yada Reverse DNS lookup yada reverse DNS resolution yada rDNS)
  IP'den dns bulma işlemidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Bluetooth üzerinden internet paylaşımı

Anlatım; Android 5.1.1, Ubuntu Gnome 16.04 üzerinden yapılmıştır.

#### Sunucu Android ise;

Android üzerinden Bluetooth paylaşımı açmak için ek programa ihtiyaç yok.

1- Android Genel ayarlar -> Bağlantılar -> İnternet paylaşımını ve Mobil … -> Bluetooth bağlantısı seçili hale getirilir.

2- Android Genel ayarlar -> Bağlantılar -> Bluetooth --> içerisinde "zaman aşımı" ayarı mevcut. bu ayar ile Bluetooth'un dışardaki diğer cihazlara görünürlük süresi vardır. bu süre sonrasında gizli bağlantı olarak kalacaktır. bu süre (isteğe bağlı) iptal edilebilir.

#### Sunucu Ubuntu desktop ise;
TODO

#### windows sunucu ise;
TODO

#### Client Android ise;

Paylaşımdan faydalanacak android'te ek uygulamaya gerek yoktur.

1- Android Genel ayarlar -> Bağlantılar -> Bluetooth --> açık hale getirilir. sonrasında etraftaki Bluetooth kaynakları burada listelendiği görülecektir.

2- Listeden Sunucu android cihazı seçilir. Hem sunucu hem client'a bir onay mesajı gelir. Onay mesajı iki tarafta onaylanır.

3- Client cihazda açılan listede sunucu Bluetooth kaynağının yanında ayarlar simgesi var. ona tıklandığında yeni bir pencere açılmaktadır. Sadece Client tarafta o pencerede "internet erişimi" seçeneğine basılmalıdır. Artık bağlantı hazırdır.

#### Client Ubuntu desktop ise;

Hiçbir ek uygulamaya gerek yoktur.

1- Genel Ubuntu ayarlar -> Hardware -> Bluetooth -> açık olduğunda listede Bluetooth kaynakları görülecektir. Bu listedeki sunucu'ya bağlanılır. Hem sunucu hem client tarafa onay mesajı gelir.

2- Genel Ubuntu ayarlar -> Hardware -> Network -> Bluetooth -> On tuşuna basılır. İnternet artık hazırdır.

# Wireles paylaşımı

#### Sunucu Android ise;
Android Genel ayarlar -> Bağlantılar -> İnternet paylaşımını ve Mobil … -> Mobil erişim noktası --> aktif hale getirilmesi yeterlidir. wireles herkes artık görünecektir.

#### Sunucu Ubuntu desktop ise;
Genel Ubuntu ayarlar -> Hardware -> Network -> Wi-Fi --> Use as hotspot -> ekranda wifi bilgileri görünecektir.

#### sunucu windows ise;
TODO

# Usb Kablosu ile internet paylaşımı

#### Client ubuntu ise;
TODO

#### Client windows ise;
TODO

#### Client android ise;
TODO

#### Sunucu android ise;
Android Genel ayarlar -> Bağlantılar -> İnternet paylaşımını ve Mobil … -> USB bağlanıyor (usb kablosu sunucu ile android cihaz arasında takılı olmalı) --> seçili hale getirilmesi yeterlidir.

#### Sunucu Ubuntu desktop ise;
TODO

#### Sunucu taraf windows ise;
TODO

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CCNA (yada Cisco Certified Network Associate)
- Cisco kurumumun verdiği network sertifikaları grubudur. altında birçok çeşit sertifika barındırır: güvenlik, data center teknolojileri, bulut bilişim gibi...
- en çok bilinen sertifika "CCNA Routing and Switching"'dır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# imap (yada Internet Access Message Protocol) vs pop3 (yada Post Office Protocol version 3)

ikiside birer mail sunucu ile haberleşme protokolleridir. imap, pop3'ten daha gelişmiştir.

# smtp (yada Simple Mail Transfer Protocol)
imap ve pop3 gibi client-sunucu arasındaki mail protokolüdür. ek olarak smtp; sunucular arası mailleşmek içinde kullanılabilmektedir.

# mail sunucusu
mail suncusu diğer emaillerle haberleşen yazılımdır. tabi client'lar ile de haberleşir. mailleri clientlardan alır ve kaşı tarafa email atar. bunları backupunu da tanımlı veritabanında saklar. fakat saklmak zorunda değildir.

# mail client
mail client'lar mail suncusu tarafından sağlanmak zorunda değildir. web client, mail sunucusuna mail suncunun açmış olduğu API ile erişir.

# Microsoft Exchange
sunucu yazılımının ismidir. herkes kendi local networküne kurabilir. mail sunucusu görevinin yanında birçok ek işlev de sağlamaktadır.

# Messaging API (MAPI)
Microsoft tarafından kullanılan IMAP gelişmişi protokoldür. temel amacı email olsada birçok ek işleve destek vermektedir.

# CC (yada carbon copy)
Carbon türkçe kelime anlamı: kopya kağıdı

carbon copy türkçe kelime anlamı: kopya, tıpatıp benzeri

# BCC (blind carbon copy)
diğerinin göremediği alıcı.

# Multipurpose Internet Mail Extensions (MIME)
SMTP mail ptokolü ile birlikte artık emaillerde text yerine farklı tiplerde datalar kullanılabilmektedir. MIME bu standartın ismidir.

# MIME Type
MIME desteği ile gönderilen emaillerde, gönderilen data tipidir. Bu tipi yazmak için belirli bir standart format vardır: formatın-ailesi / format-uzantısı. örneğin: application/pdf, text/html, image/png

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DMZ (yada demilitarized zone yada perimeter network)
Örnek üzerinden gidilirse; bir şirket ağında tüm iç networkün önünde bir güvenlik duvarı vardır. Dışarıdan içeriye giriş yoktur yada kısıtlıdır. Fakat bazı müşterilerin erişebileceği, yada tüm halka açık bir alt ağ olmasını isteyebilirsiniz. Bu alt ağa verilen özel isim DMZ'dir.

DMZ için modem ayarlarından bir ip verilir. bu ip; bir makineye yada alt ağa erişimi sağlayan bir router'a ait olabilir. artık modeme gelen tüm istekler aynı şekilde o ip'ye iletilir. bu durumda dmz dışında kalan cihazlar ne yapacaktır? dmz ip'si, sürekli olarak tüm portları kullanmadığından diğer portlardan boş olanlar diğer makinelere tahsis edilecektir. ama her zaman öncelik dmz makinasında olmalıdır. dmz aarları ile diğer makineler için yapılan port forwardingler çok karışık/conflict bir durum yaratabilir. 

# local area network (yada LAN yada yerel alan ağı)
kısıtlı bir alandaki network'e verilen genel isimdir.

# wide area network (yada WAN yada Geniş alan ağı)
büyük coğrafik alanlardaki üst ağa verilen genel isimdir. WAN, LAN gibi terimlerin uygulama bazında kesin bir karşılığı yoktur. WAN genelde piyasada; internet servis sağlayıcıları (yada internet service provider yada ISP) 'nın networkü için kullanılır. çünkü en geniş coğrafya'ya onlar hizmt vermektedirler.

# metropolitan area network (yada MAN yada Metropol alan ağı)
wan ve lan arasındaki büyüklükteki network'lere verilen genel isimdir. WAN ile ülkeler ve ehirler birbirine bağlanırlar. Fakat şehirler veya ilçeler kendi içlerinde MAN diye  bir altağda olurlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# iptables
netfilter projesi altında geliştirilen, linux üzerinde çalışan bir komut satırı uygulamasıdır. ip/port filtrelemesi yapar. güvenlik duvarı görevi görür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# intranet
şirket içi (yada özel bir amaç için) networke verilen takma isim.

# Ekstranet (yada extended intranet yada extranet)
intranete ek olarak organizasyona bağımlı tedarikçiler, müşteriler için genişletilen ağdır. internet ağı buranın dışında kalır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Samba
windows harici sistemler için yazılmış, açık kaynaklı dosya paylaşım yazılımıdır. gene olarak dosya paylaşım mekanizmalarında; bir dosya paylaşım yazılımı hem sunucu hem client görevi görebilmektedir. dosya paylaşımı yapıldığında sunucu kısmı, başka makineden dosya okuma işlemleri client modülleri ile gerçekleşir. samba her iksini birlikte yapabilmektedir.

# SMB
windowsun dosya paylaşım protokolüdür. samba yazılımıda bunu desteklemektedir. smb protokolünün birçok türevi ve sürümü vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DHCP (yada Dynamic Host Configuration Protocol)
Network'e IP atayan uygulamadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Subetnet (Alt ağ)
IP blokları gruplanarak; alt ağlar meydana getirilir. BU şekild ehem gruplama yapılmış olur, yönetimi daha kolay olur ve broadcasting işlemleri networku daha az yorar (çünkü sadece ilgili alt ağa broadcast yapılacaktır).

# broadcast (yayın)
network üzerinde bir paket yollandığında bir IP belirtilerek yollanır. Fakat her alt ağda bir broadcast Ip'si vardır. bu ip'ye atılan paketler, tüm alt ağa komple gönderilir.

# Multicast
network üzerinde belirli bir IP uzayına yapılan yayındır.

# Unicast
Network üzerinde sadece 1 adet IP ye iletilen yayındır.

# Subnet Id
Bir alt ağın ID'sidir. örneğin; 192.168.0.0 şeklindedir ve bir IP adresi değildir.

# Subnet Mask (yada IP Mask)
İki makine birbiri ile aynı ağda olup olmadığını IP ve "IP Mask" değerlerini kullanarak anlayabilirler. "IP Mask" değeri bir altağda herkeste aynıdır.

Örneğin; Aynı networkte, IP maskesi 255.255.255.0 ise, tüm makinelerin IP'lerini ilk 3 sekizliği (yani ilk 24 biti) diğer karşılaştırılan IP ile aynı olmak zorundadır.

# network class
255.255.255.224 bit bazında yazıldığında: 11111111 11111111 11111111 11100000, 27 tane "1" bit olduğundan, "/27" class network olarak adlandırılır.

# default gateway (yada varsayılan ağ geçici)
dış ağa bağlı router'ın ip'sidir. iç network'te bulamadığımız ip'leri ona yönlendiririz. bu şekilde ilgili yönlendirmeleri artık ona bırakmış oluruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# TCP Socket states

- LISTEN - sunucu - soket gelebilecek bağlantıları bekliyor

- SYN_SENT - client - soket bağlantı kurmak için karşıya istek yapıyor durumda

- SYN_RECV - server - client server-sokete bağlanma isteği yolladı, server kabul etti, ve son onayı tekrar client'tan bekliyor durumda

- ESTABLISHED - sunucu ve istemci - soket bağlantı kurmuş durumda

Buradan aşağıdaki tüm durumlar kapatılma ile ilgili:

- FIN_WAIT1 - sunucu ve istemci - bağlantı sonlandırma bildirisi yollandı. cevap bekleniyor.

- FIN_WAIT2 - sunucu ve istemci - FIN_WAIT1 ile gönderilen bağlantı sonlandırma isteğine, uzak soket onay döndü. artık uzağın bağlantısını kapattığına dair son onayı yollaması bekleniyor. bu bekleniyorken karşıya hiçbir şey atılmıyor. yani; paket alındı, tekrar yeni paket bekleniyor.

- TIME_WAIT - sunucu ve istemci - Uzak soket bağlantıyı kapattığına dair son onayı yolladı. soket closed duruma geçiş için son procesi yürütüyor.

- CLOSED - sunucu ve istemci - bağlantı sonlandırıldı.

- CLOSE_WAIT - sunucu ve istemci - karşı taraf, katapma bildirisini bize gönderdi. Karşı tarafa bildiri alındığına dair onay yollanır ve soketi kapatma işlemine geçilir. soketi kapatma işlemi için yazılımın onay vermesi gerekir. bu yüzden bu statüde takılan birçok soket oluyor.

- LAST_ACK - sunucu ve istemci - CLOSE_WAIT sonrası Soket kapatıldığında, son olarak kapatıldı bilgisi karşı tarafa gönderilir. gönderildiğinde bu soket CLOSED olur.

# acknowledgement (yada acknowledgment yada ack)
protokol içerisindeki onay mesajıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# virtualbox ile networku kullanma
virtualbox network ayarlarında network bağdaştırıcısı eklerken birden fazla farklı ayar mevcuttur. bunlar:

- NAT: Burada host ve guest işletim sistemi arasında private virtual bir network oluşturulur. host bir router gibi davranır ve NAT ile istekleri makineden dışarıya çıkarır. guest'in aldığı IP, private network içinden bir IP'dir.

- NAT Network: NAT yapısı ile aynı çalışmaktadır. Sadece NAT yapısında tek bir makine variken burada komple bir network vardır. Dolayısı ile yine fiziksel kartımız bir router gibi davranıp, sanal VM'lerden oluşan bir networku dışarıya NAT ile çıkarmaktadır. Sanal networkte olan VM'ler birbirlerini de görebilmektedirler.

- bridge networking: host'un bağlı olduğu DHCP'den yeni bir gerçek IP alır. DHCP'den iki tane IP alabilmek için farklı bir cihazmış gibi davrandırtıyor. BUnu yapabilmek için; Virtualbox kendi istekleri için sadece MAC adresi değiştiriyor. bu network çeşidi bazı işletim sistemi ve farklı donanım kartları/teknolojileri için desteklenemiyor.

- Host-only networking: sanal bir ağ kartı yaratır. bu sanal ağ kartı içerisinde itediğimiz kadar VM'i birbiri ile iletişime geçirebilir. dış dünya ile iletişimde olmazlar. sadece kendi aralarında haberleşiler. istenildiği kadar sanal ağ kartı yaratılıp, VM'ler kendi içinde gruplandırılabilir. sanal ağ kartının yanında, networkte sanal DHCP sunucuda yaratılmaktadır (virtualbox ayarlarından).

- Internal networking: Host-only networking yöntemi ile apaynıdır. sadece sanal bir kart yaratılmaz. sanal bir kart yaratılmaması; varolan bir fiziksel kartın kullanılmasına sebep olur.

Virtualbox ortamında birden fazla ağ bağdaştırıcısı eklenebilir. Dolayısı ile örneğin; bir VM içerisindeki işletim sistemi hem internal network ile diğer VM'ler ile haberleşirken, NAT bağdaştırıcısı ile internete çıkabilmektedir.

network interface konusu başka başlıkta anlatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Quality of Service (QoS)
Genel bir ismi var fakat özel olarak; internet ağı içinde network'te giden gelen paketlere öncelik vermek için gerekli yapılandırma ve ayarlar bütünüdür. Örneğin modem, voip ve http üzerinden isteklere, download edilen bir dosyadan daha fazla öncelik verir. Bu sadece bir parametredir. bunun gibi bir çok önlem alınabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# VPN (yada Virtual Private Network)

B bilgisayarı ve N networku var. B'nin N networkü üzerinde çalışması isteniyor fakat ayrı networkteler. bu durumda vpn kullanılıyor. b bilgisayarına ve N netowkrunden bir X bilgisayara vpn yazılımı kuruluyor. X üzerindeki yazılım, kendi işletim sistemi üzerinde bir sanal network kartı oluşturuyor (ipconfig ile görülebilir). X bu sanal kart ile, N'in DHCP'sinden ikinci bir IP'yi alıyor. İkinci IP'yi B bilgisayarı için ayırıyor. B üzerindeki yazılım ve X üzerindeki yazılım artık direk birbirleri arasında bağlantı içerisinde işlemleri yapmaya başlıyorlar.

Bu senaryoda X bilgisayarı 24 saat açık kalmalı ki B isteği zaman bağlanabilsin. BUnun yerine modem'ler vpn destekli olursa, vpn konfigurasyonları modeme yapılıyor, modem aracılığı ile B bilgisayarına DHCP'den ip atanıyor ve aynı işlemler gerçekleştiriliyor.

Bu 2 senaryoda da, modemin içindeki yazılım veya X bilgisayarına kurulan yazılım sunucu uygulaması oluyor.

VPN yazılım katmanında olduğu için bir çok özellik sunabilir. Örneğin; vpn yazılımı belirli IP'ler dışındaki isteklere çıkışınızı engelleyebilir, şifreleme yöntemlerini değiştirebilir, güvenlik için paketleri inceleyebilir, log sistemi olabilir, GUI sunabilir, bir yazılımın VPN üzerinden gitmesine ayarlarken, diğer yazılımların istedikleri yerden çıkmaları sağlanabilir.

Android 5 ile bir uygulama root yetkisi istemeden, diğer uygulamarı vpn üzerinden iletişimini sağlayabilir.

VPN bu yapının genel ismidir. Altında bir çok protokol, bu protokollerden yararlanarak bağlantı kurmamızı sağlayan birçok bulut hizmet, ve bu bağlantıları sağlayan yazılımlar mevcuttur.

# OpenVPN
Açık kaynaklı VPN uygulaması ve protokolüdür. Aynı zamanda yazılımı geliştiren firma, openvpn protokolü ve yazılımları ile bulut vpn hizmeti de sunmaktadır.

# Proxy server (yada Vekil sunucu yada yetkili sunucu)
A bilgisayarı istekleri önce P bilgisayarına gönderir. P bilgisayarı bir proxy'dir. bu istekleri dışarıya aktarır ve dönen değerleri direk A bilgisayarına gönderir. P bilgisayarı burada bir proxy'dir. Amacı aracılık yapmaktır. Bunun bir çok sebebi olabilir:

- aynı proxy'den çıkanlar için cache mekanizması yapmak.
- yasaklı internet sitelerine erişmek.
- gidilen yere yavaş gidiliyor ise proxy hızı ile bu süreyi kısaltmak.
- siteleri yasaklamak
- proxy ile istekleri filtreleyip/inceleyip güvenliği sağlamak gibi.

# vpn vs proxy
consept olarak farklı olsalarda teknik altyapı incelendiğinde birbirinin görevlerini tamamlayabildikleri görülür. fakat ikisini amacı farklı olduğundan çok farklı işleyişleri vardır.  

# SOCKS proxy
SOCKS bir çeşit proxy türüdür. Birçok sürümü vardır: 4, 4a, 5

# Network Address Translation (yada NAT yada Ağ Adresi Dönüştürme)
Router'ın bir IP adresi olsun. BU router'ın arkasında birden fazla cihaz olabilir. Yani bir (çıkış) IP'yi birçok cihaz kullanmaktadır. Yönlendirici kendi içinde haritalama yaparak kimin hangi IP'yi kullandığını belirleme mekanizmasıdır. Bu mekanizmanın birden fazla yöntemi vardır:

  - # Static NAT
    Routerlarda çıkış IP'si tek olmayabilir. Bize bir çıkış IP aralığı verilmiş olabilir. Böyle durumlarda içerideki her cihazı, sabit olarak bir çıkış IP'sine haritaladığımızda bu yöntemi kullanmış oluruz.

  - # Dynamic NAT
    Static ile aynı mntıkta çalışır. Tek farkı hangi IP'nin hangi çıkış ile gideceğine router o sıra karar verir (önceden belirlenmez).

# Port Adres Çevirimi (Port Address Translation yada PAT yada NAPT yada Network PAT)
Overloading (aşırı yüklenme) olduğu zmanalar tercih edilen NAT yöntemidir. Örneğin; bir çıkıp IP'miz var. İçerde ise on tane makinamız. BU durumda router, içerdeki her IP'nin her IP:PORT'u çıkıştaki bir IP:PORT'a bağlar. çıkışta bir adet IP'miz olduğundan maximum TCP soket portu sayısı kadar farklı bağlantı kurbiliriz. BU durumda localde 2 makina olsun, 2 makina birde 65bin adet portunu aynı anda kullanamayacaktır.

# Port yönlendirme (yada port mapping yada port redirection)
Router ayarrında olan bir özelliktir. Dışarıdan X portuna gelen bağlantılar, local tarafta A IP'li bilgisayarın Z portuna yönlendirilmesi sağlanabiliyor.

# Port Triggering (yada Port tetikleme)
ort yönlendirme ile aynı şeydir. sadece ek olarak; yönlendirilen port kullanılmadığında kapalı kalır. bu şekilde ek güvenlik sağlanır.

# virtual servers
port forwarding ile aynı şeydir. fakat bu terim sadece external port ve internal port aynı olduğu zaman kullanılır.

# Hub
Yerel ağ oluşturmak için gerekli cihaz. Sadece veriyi alıp dağıtmak için kullanılır. Bir özelliği yoktur. Gelen veri-paketlerini bağlı olan tüm kablolarına dağıtır.

# Switch (yada switching hub yada bridging hub yada MAC bridge yada dağıtıcı yada ağ anahtarı)
Hub ile aynı görevi görür. Fakat hub'dan daha zekidir. Gelen paketi inceleler ve sadece hangi port (kablo girişi) ya gönderecekse oraya aktarır. Gereksiz ağ trafiğini de engellemiş olur.

# Router (yada Yönlendirici)
Router'lar switchlere benzer mantıkta çalışırlar fakat bağlı olan bilgisayarları değil, netowrkleri birbirine bağlarlar. bu yüzden normalde az portu olan bir cihazdır. fakat dışarıda son kullanıcıya satılan router'lar içerilerinde switch teknolojisi ile entegre gelirler. bu şekilde router sonraıs swtich alma gereksinimini ortadan kaldırır. bu yüzden daha çok porta sahip router'lar vardır. switch MAC LAN içerisinde MAC adresleri bilir ve ona göre paketleri filtreler. OYsa router IP adresleri de bilir. AYnı zamanda router bir firewall modülü içerir.

# Modem
Dijital-analog sinyal çevirimini yaparak verilerin kablo üzerinden iletilmesini sağlar. BU yüzden ismini modulator-demodulator kelimelerinden almaktdır. SOn kullanıcıya sunulan modem'lrde router modülü entegre gelir. bu şekilde router almaya gerek kalmaz.

# Dial-up Internet access (ada Çevirmeli ağ)
telefon numarası çevirilerek erişimin sağlandığı, bir bilgisayar ağı biçimidir.

# digital subscriber line (DSL)
bakır kablo ile iletişim sağlanan iletişim yapısıdır. xDSL olarak adlandırılırlar. x yerine kullanılan teknolojinin harfi gelir: aDSL, vDSL gibi.

# Access Point
bilgisayarın ilgili iletişim ağından erişilen ilk noktadır. Örneğin bir bilgisayar bir wifiye erişiyorsa, wifi cihazı bir access point olur. aynı şekilde wifi haricindeki örneklerde verilebilir. fakat günlük hayatta halk arasında genelde sadece wifi için bu terim kullanılmaktadır.

# Dijital (sayısal) Sinyal (signal)
Bu sinyalde iki değer vardır.Bu iki değer 1 ve 0 dır. Bu iki değer farklı şekillerde adlandırılabilir. Açık ve kapalı,doğru ve yanlış,yüksek ve düşük gibi.

# Analog sinyal (signal)
Yönü ve şiddeti zamanla değişen sinyallere denir. Analog sinyale en güzel örnek ses sinyalidir. Digital sinyallerde max 1, min 0 diğe temsil edilirken, Analog sinyallerde 1024 ve 0 kullanılır.

# Fiberoptik
Metal kablolar yerine fiber kablolar kullanılır. fiber kablolar cam veya plastik'ten meydana gelmektedir. ışık ile veri iletimini sağlar. metal kalbolara göre temel avantajları: manyetik alandan hiç etkilenmemeleri ve daha diğer gürültü çeşitlerinden daha az etkilenmeleridir.

# Ethernet
Sadece kablo aracılığı ile cihazlar ararsı iletişim kurulması için gerekli standratlardır. Wi-Fİ ise radyo dalgaları ile iletişim kurma standardıdır. fiber ethernet değildir.

# IEEE 802
Tüm netowkr iletişim standratlarının tanımlandığı standartlar ailesidir. ALtında birçok standrat vardır. Bu standartlar yanında numara ve karakterlerle temsil edilirler. Örneğin; IEEE 802.11 kablosuz ağ standartlarıdır. 802.11g kablosuz ağ starndartlarının bir sürümüdür. IEEE 802.3 ethernet standartlarıdır.

# SIP (Session Initiation Protocol)
bir voip protokolüdür.

# internet telefonu
internet bandı kullanılarak haberleşmeyi sağlayan altyapılara verilen genel isimdir.

# ISDN (Integrated Services Digital Network)
en eski usul telefonlardan sonra çıkan ilk dijital telefon altyapısıdır. internet ağı üzerinden haberleşmez.

# VoIP (yada voice over ip)
yazılım protokolüdür. genel bir isimdir. cihazlar aracılığı ile paketler diğer telefon altyapılarına çevrilecek şekilde kolay tasarlanmıştır. bu şekilde internet üzerinden, normal telefonlar ile konuşulabilmektedir. normal telefonlardan daha ucuz olabilirler. çünkü normal telefon hatları mesafeler uzadıkça daha maliyetli olmaktadır. fakat voip hizmeti sunan sunucular, bizi internet ağı üzerinden ulaşacağımız telefona en yakın yere yönlendirip, yönlendirdiği endpoint üzerinden paketleri çevirerek normal telefon hattı olan bir yere bağlamaktadır. bu da maliyeti ucuzlatmaktadır.

# UPnP (yada Universal Plug&Play yada Evrensel Tak&Çalıştır)
Elektronik cihazların internet ağı üzerinde biribirini otomatik algılaması için geliştirilen bir protokoldür. Ağ'ı yöneten router'ın UPnP desteği olması ve çalışan yazılımın (elektronik cihaz içerisindeki) UPnP supportu olması şarttır.

örneğin UPnP destekli modemler, modeme bağlı client'lardan aldığı komutlar doğrultusunda port yönlendirme yapabiliyor. böylece örneğin vnc içerisindeki bir seçenek ile port yönlendirme yapılmış oluyor ve teamviewer gibi merkezi sunucuya ihtiyaç kalmıyor.

# Delik Açma (yada Hole Punching)
Teamviewer gibi port yönlendirmenin yapılmaması istenen sistemlerde birbiri ile haberleşmesi gereken makinalar söz konusudur. böyle durumlarda merkezi bir sunucu ihtiyacı vardır. bu sunucu "S", "A"  ve "B" makinelerinin haberleşmesini sağlar. Öncelikle A  ve B, S'e bağlanır. S, A'ya b'nin, b'ye de a'nın bilgilerini verir. artık a yada b programın ihtiyacına göre sunucu gibi davranır ve yeni bir soketten dinleme yapmaya başlar. örneğin; a dinleme yapmaya başladı. b ise anın portunu biliyor. bu yöntem ile artık b a ile soket üzerinden haberleşebilir. bu yöntem'e delik açma yöntemi denir.

bu yöntem her zaman %100 temiz çalışmayabiliyor. zira router'ların nat sistemi her zaman beklenilen gibi işlemeyebiliyor. örneğin; dışarıdan  bağlantı gelince router bunu tehlike olarak görüp portu kapatabilir. yada local-external port arada değişebilir vs... bu sebeple son kullanıcı uygulamalarında bu teknik kullanılmaz. teamviewer gibi uygulamalarda teamviewer'ın sunucusu a'dan b'ye ekran görüntülerini kendi üzerinden aktarır. yani tüm transfer edilen veriler teamviewer sunucuları üzerinden geçer. teamviewer sunucusu yazılımsal aracı görevi görür.

# Encapsulation
OSI TCP layer'daki gibi bir protokolün, alt protokole data yerleştirmesidir. tersi (__decapsulation__) ise alt protokoldeki dataların üst protokollere taşınmasıdır/yerleştirilmesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# routing table (yada routing information base yada RIB)
router içinde yada bilgisayarda tanımlı olan bir tablodur. bu tabloda hangi ip'ye gidildiğinde bizi nereye yönlendireceği belirtilir. normalde; bir bilgisayar bir ip'ye gitmek istediğinde network'te bağlı olduğu router'a paketi atar ve paket router tarafından gerekli yere yollanır. fakat bazen;

- uzak networkteki iç bilgisayarlara yönlendime yapmak zorunda kalabiliriz

- hızlı routing olması amaçlı bildiğimiz makinelere routing yaptırabiliriz

gibi...

# komut satırı uygulamaları ile takip etme
traceroute posix'lerde, tracert ise windows sistemlerde komut satırı uygulamalarıdır. bu uygulamalar bir ip'ye gidilmesi için gerekli tüm aradaki node'ların(router'ların) iplerini gösteriri. bu işlemi gerçektende karşıya paket atarak yapar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DigitalOcean
Amazon'un bulut hizmetleri gibi sanal sunucular sunan bir firma.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Skype Wi-Fi
Microsoft'un skype-wifi için anlaştığı wifi olan bir mekanda skype hesabınız ile wifiye login oluyorsunuz. bu skype hesabınızda önceden kredi yüklü olmalıdır. wifi'ye login olunduğunda skype-wifi uygulaması başlatılmalıdır. Skype wifi artık dakika üzerinden ücret tarifesi başlatmakta ve interneti sınırsız olarak kullandırtmaktadır. skype-wifi, Skype uygulamasından tamamen bağımsız bir uygulamadır.

# Hot spot firmaları
Skype Wi-Fi gibi bir çok firma anlaşmalı olarak bu işi yapabilmektedir. tek bir modemden hotspot yaparak birçok ağ yaratılıp birçok markanın ağına bağlanabiliyoruz. bunnu yapan hizmet sağlayıcılar mevcut.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# instant messaging (IM)
anlık mesjalaşma teknolojilerine verilen genel isim.

# Extensible Messaging and Presence Protocol (XMPP)
mesajlaşma protokolüdür. merkezi bir sunucusu yoktur. herkes bir sunucu kurup bu hizmeti bağımsızca kullanabilmeyi sağlar. eskiden google ve facebook chat protokollerinden XMPP tabanlı sistemlerden yararlanıyordu fakat artık bu teknolojiyi bıraktılar. şu anda XMPP android ve ios için push notification'larda kısmen kullanılmaktadır.

# Jabber
XMPP'nin eski ismidir. Şu anda Jabber.org sitesinden XMPP protokolü kullanan bir chat mesjalaşma sunucusu hizmet vermektedir.

# Xabber
android XMPP client app.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Open Systems Interconnection (OSI)
ISO tarafından belirlenmiş ağ katmanlarının belirlendiği modelidir.

# Transmission Control Protocol (TCP)
Güvenli veri aktarımı (hata denetimi içeren) taşıma katmanı protokolüdür.

# TCP/IP
OSI'ye alternatif bir ağ katman modelidir.

# OSI Model
Üst seviyeden aşağıya doğru detaylar aşağıda yazmaktadır:

- isim
  - ilgilendiği birim
  - açıklama
  - örnek protokoller


- Application
  - Data
  - High-level APIs
  - HTTP, FTP, Telnet, SMTP, SSH


- Presentation (Sunum)
  - Data
  - Translation of data between a networking service and an application; including character encoding, data compression and encryption/decryption
  - MIME, TLS


- Session (Oturum)
  - Data
  - Managing communication sessions


- Transport
  - Segment (when using TCP) / Datagram (when using UDP)
  - Reliable transmission of data segments between points on a network
  - TCP, UDP


- Network
  - Packet
  - managing a multi-node network, including addressing, routing and traffic control
  - IPv4, IPv6


- Data link
  - Frame
  - Reliable transmission of data frames between two nodes connected by a physical layer. Çoğu bilgisayarda işlemlerin bir kısmı donanım kısmında yapılır.


- Physical
  - Bit
  - Transmission and reception of raw bit streams over a physical medium. Donanım kısmında yapılır.

# tcp soket header
header kısmı en aşağıdaki data haricindeki kısmı kapsar.

Aşağıdaki büyük grafik bir tcp paketini göstermektedir. Birçok tcp paketi birleşerek bir segment'i oluşturur.

```
    0                   1                   2                   3   

    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |          Source Port          |       Destination Port        |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |                        Sequence Number                        |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |                    Acknowledgment Number                      |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |  Data |           |U|A|P|R|S|F|                               |

   | Offset| Reserved  |R|C|S|S|Y|I|            Window             |

   |       |           |G|K|H|T|N|N|                               |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |           Checksum            |         Urgent Pointer        |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |                    Options                    |    Padding    |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |                             data                              |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

- #### Sequence Number vs Acknowledgment
ikiside datanın başlangıç byte numarasıdır. karşı tarafa paket attığımızda bizim aynı pakette yolladığımız data'nın tüm sgement'teki kaçıncı bayt olduğu bilgisi sequence'de yollanıyor. oysa ACK'da karşı taraftan kaçıncı sıradaki datayı istediğimizi bilgisini yolluyoruz. örnek; yolladığımız pakette SEQ=5, ACK=90 olsun. bu şu demek oluyor: yolladığım paket tüm segment için 5'inci byte'tan başlayan bir data içeriyor. ve ben buna karşılık karşıdan 90inci bayttan başlayan datayı istiyorum. karşı taraftan ne kadar büyüklükte bayt geleceği ve benim ne kadar büyüklükte bayt yolladığım blgisi SEQ ve ACK'da belirtilmemektedir.

- #### data offset
options kısmının uzunluğu değişebiliyor. bu sebeple sadece tüm header'ın uzunluğu burada belirtilmektedir. bu şekilde data'nın nereden başladığını bilebiliriz.

- #### Reserved
hiçbir işe yaramaz. ilerde  tcp sürümü çıktığında kullanılması için rezerve edilmiştir.

- #### Control Bits (yada Flags)
bazı sinyalleri içerir.

- #### window
tcp protokolünde her paket sırası ile yollanır fakat cevap gelmeden diğerleri de yollanır. bunun yapılmasının sebebi kuyruk mekanizması yerine pararlel mekanizma ile zamandan kazanılmasıdır.

  window kısmı byte biriminde bir uzunluk belirtir. bu kısımda bu paketi yollayan kişinin kaç adet paket daha almaya hazır olduğunu belirtir. bu şekilde karşı taraf paralel olarak buradaki boyu kadar byte yollayabilir. böylece karşılıklı denge ile paralel data yollanır. eğer bu window sayısı olmasaydı, herkes birbirine aynı anda tüm bandwith kadar paket yollardı. rşı tarafın müsaitlik durumunu buradaki sayı ile anlayabiliriz.

- #### Checksum
destination ip + source ip + protocol bilgisi (bizim senaryomuzda "TCP") + tcp header ve body length --> bu 3 adet bilgi daha alt seviyeli bir katmanda yollanıyor. bu 3 adet bilgiyinin hash'i checksum'a atılıyor.

- #### urgent point
"URG" control bit'i set edilmişse bu kısımda öncelik numrası verilmektedir. paketin önceliği için gerekli sıra numarası vardır.

- #### options
opsiyonel bir alandır. ekstra bilgiler içerir. örnek: timestamp.

- #### padding
tcp header'ın belli bir sayının katı olması beklenir. yani örneğin; 29 bit olamaz, onun yerine 32 olabilir. dolayısı ile 32'ye tamamlamak için padding kısmına boş data atılır.

# heartbeat
tcp soketi herşeyin sorunsuz olduğu koşullarda data yollanmazsa bile kapanmaz. sürekli açık bekler. fakat arada proxy varsa, firewall varsa gibi sebeplerden tcp soketleri aracılar tarafından kapatılabilir. bu sebeple tcp soketimizi açık kalmasını istiyorsak düzenli olarak karşı tarafa yaşadığımızı belli eden sinyal atmalıyız. protokol dünyasında bu tarz sinyallere yazılımcılar arasında heartbeat deniyor.

tcp standartlarında özel bir sinyal yok. fakat herkes tarafından kullanılan bir tcp sinyali var: "keep-alive". bu data kısmı boş olan bir tcp paketidir. hangi aralıklarla atılacağı tamamen yazılımcının kendisine kalmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# mac adres (yada media access control address)
mac adres donanımın içinden sürücüler aracılığı ile yazılım tarafından okunur. fakat mac adres bilgisi (en azından network cihazları için) network-internet ağı iletişimlerinde yazılım katmanında taşındığı için kolaylıkla sahte bilgi kullanılabilmektedir.

'mac' birçok terimin kısaltmasına denk gelmketedir: https://en.wikipedia.org/wiki/Mac . bazıları:
- modified, access, creation times on filesystem
- 'message authentication code' kriptografide kullanılan, mesajın hash değeridir. bu şekilde mesjaın değişip değişmediği doğrulanabilir. mac değeri key ile imzalandığı için aynı zamanda, mesajın istenilen alıcıdan geldiği de doğrulanmış olur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ssh (secure shell)
ağ üzerinde gücenli veri iletimi sağlamak için geliştirilmiş protokol.

shh uygulaması login işlemini doğrularken, o anda sunucu tarafta run edilen işletim sistemi user'ının şifresini istemez. onun yerne ssh sunucusunun config dosyasında belirtilen şifreyi yada public key'i ister.

ssh tamamen işletim sisteminden bağımsız olan bir uygulamadır. ssh'a bağlandığımızda ssh üzerinden verdiğimiz komutlar, ssh server uygulamasına TCP üzerinden yollanır. server uygulama bu komutları direk uzak işletim sistemine yollar. eğer admin gerektiren bir işlem yaparsak işlemtim sisteminin user admin şifresini girmemiz gerekir. 

ssh ekrtra olarak ("-X" parametresi verilmiş ise), kurduğu TCP bağlantısı üzerinden uzak masaüstündeki X server'a bağlanabilmemizi de sağlar.

# openssh
açık kaynaklı ssh sunucu ve client uygulamasıdır.

# openssl
açık kaynaklı ssl işlemleri yapan komtu satırı uygulaması ve kütüphanesidir.

# GnuPG (yada GNU Privacy Guard)
__OpenPGP__ implementasyonudur.

OpenPGP ise __PGP yada Pretty Good Privacy)__'nin forkudur.

hem OpenPGP hemde GnuPG kendi komut satırı uygulamalarını sunar. bu uygulama ile dosya, disk (herhangi bir data) şifrelenip decript edilebilir. şifreleme metodu ve diğer parametreler dışarıdan verilebilir.

OpenPGP, "pgp", GnuPG "gpg" komut satırı uygulamasını sunar.

opengpg ve diğer tüm implementasyonlar sadece şifrelme ile değil aynı zamanda sıkıştırma, digital imza gibi ek özellikler de sunar.

# FTP
dosya transferi için kullanılan protokol.

# FTPS (yada FTPES yada FTP-SSL yada S-FTP yada FTP Secure yada FTP over SSL)
FTP prokolünün SSL ve TSL ile birlikte kullanımı için geliştirilen protokol.

# SFTP (yada SSH file transfer protocol)
ssh yapısı ile tamamiyle bütünleşik ftp protokolüdür.

# FTP over SSH (yada SSH üzerinde FTP kullanımı)
SFTP ile karıştırılmamalıdır. SSH bağlantısı açıp, bağımsız olarak o bağlantı üzerinden FTP ile dosya yollama işlemleridir.

# scp (yada secure copy)
dosya transferi protokolüdür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Request for Comments (RFC)
is a type of publication from the Internet Engineering Task Force (IETF) and the Internet Society (ISOC) which includes contents about Internet.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Lightweight Directory Access Protocol (yada LDAP)
Uzak ve birbirine direk bağlı ağlarda bilgisayarlar arası paylaşım/izin gibi yetkileri ayarlayan protokoldür. Protokolün kullanılabilmesi için LDAP sunucusu ve her bilgisayarda client yazılımı olması gereklidir. Microsoft Active Directory, OpenLDAP bu protokolün kullanımını sağlayan yazılımlardır.

************************

# Surface web (yada Visible Web yada Indexed Web yada Indexable Web yada Lightnet)
Internette arama motorlarına kayıtlı siteler.

# Derin ağ (yada derin internet yada deep web yada invisible web yada hidden web)
Arama motorlarının indexlenmediği bir web sitesi, derin internet'teki bir site olmuş oluyor. bunun için özel ek bir işlem yapmaya gerek yok. site yetkilileri arama motorlarına indexlemeleri için başvuru yapmazlarsa indexlenmezler. bu tarz siteler genelde özel amaçlar yada herkes tarafından görünmemek için tercih ediliyor. url'yi bilen adrese girebiliyor. 

# dark web
Ancak özel programlarla erişilebilen derin ağ sitelerine verilen isimdir. 

# dark net
Dark web'e erişim için geliştirilmiş network sistemidir. Örneğin "tor" bir darknet türüdür.

# Tor (The Onion Router)
Anonim iletişim için gerekli ağ ve bu ağa bağlanmak için ilgili yazılımları geliştiren proje adıdır. Ağın ayakta kalabilmesi için sunucu kaynakları gerekli. burada tor yazılımları kuran kullanıcılar, isterse birer tor ağı olabiliyor. tor ağı protokolleri log takibini zorlaştıracak şekilde tasarlanmaktadır. mutlak gizliliği sağlamamaktadır. proje açık kaynaklıdır. Ağ'a bağlanmak için gerekli tool'lar ve protokoller OSI network katmanında en üst seviyede (Application layer) çalışmaktadır. tor network routing'leri ile gidilebilecek siteler mevcut. bu sitelere erişmek için sadece tor kullanmak gereklidir. .onion uzantılı siteler mevcuttur.

# Tor Browser
Tor project'in resmi masaüstü tarayıcısı. firefox türevi. tüm masaüstü işletim sistemlerinde çalışan açık kaynaklı portable versiyonları mevcut.

# Orbot
android için uygulamaların tor networkuna bağlanmasını sağlayan uygulama. androidin yeni gelen vpn özelliği sayesinde, her uygulamanın ayrı ayrı tor'dan çıkıp çıkmayacağı ayarlanabiliyor.

# Orweb
Tor project'in resmi android tarayıcısı. varsayılan android stock tarayıcı türevi. fakat orfox'un stabilleşmesi üzerine artık marketten kaldırılmıştır.

# Orfox
Tor project'in resmi android tarayıcı versiyonu. firefox türevi.

############################

############################
# PATTERN
############################

############################

# GOF (yada Gang of Four)
1994'te "Design Patterns: Elements of Reusable Object-Oriented Software" isimli kitap Erich Gamma, Richard Helm, Ralph Johnson ve John Vlissides tarafından yazılmıştır. bu kitapta birçok objcet orianted pattern anlatılmaktadır. kitap birçok yazılımcı tarafından bilinir. bu kitabın yazarlarına "Gang of Four" takma adı verilmiştir. işlenen bazı konular:
- genel object orianted felsefesi
- Structural patterns (adapter, bridge, composite, decorater, facade, proxy...)
- Creational patterns (abstract factory, builder, factory method, prototype, singleton...)
- Behavioral patterns (observer...)
- c++ üzerinden örnekler

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Pattern list by types

Aşağıdaki liste sıra ile anlatılmıştır.

- Structural pattern
  - Private class data pattern
  - Facade pattern
  - Proxy Pattern (yada vekil deseni)
  - Delegation Pattern
  - Adapter pattern
  - Composite pattern
  - Bridge pattern
  - Decorator pattern (anlatımı henüz yok)
- Behavioral pattern
  - Iterator pattern
  - Null object pattern
  - Observer pattern (yada Gözlemci deseni)
  - State pattern
  - Specification pattern
- Concurrency pattern
  - Thread pool pattern
- Architectural pattern
  - çoğunlukla microservis tabanlı sistemde uygulanabilecek pattern'ler
    - Sidecar pattern (yada sidekick pattern yada decomposition pattern)
    - database per service pattern
    - Decompose by business capability Context
    - microservice pattern (yada mikroservis deseni) vs Monolithic pattern (yada monolitik desen)
    - Externalized configuration pattern
    - api gateway pattern
    - api composition pattern
    - Circuit Braker (yada devre kesici)
    - CQS (yada Command Query Separation) vs CQRS (yada Command Query Responsibility Segregation)
    - two-phase commit (yada 2PC)
    - saga pattern
    - event sourcing pattern
    - Circuit Braker pattern (yada devre kesici deseni)
    - Service registry pattern
    - Client-side Service Discovery Pattern vs Server-side Service Discovery Pattern
    - Self Registration pattern
    - Blue-Green Deployment Pattern
    - Microservice chassis
  - Domain driven design (yada DDD)
  - MVC vs MVP vs MVVM
  - Data Access Object (yada DAO) (Bu pattern Structural grubu altında da sayılabilir)
- Creational pattern
  - Prototype pattern
  - Singleton pattern
  - Builder pattern
  - Factory pattern + Factory method pattern + abstract factory pattern (yada Factory of Factory pattern)

"Microservices patterns" kitabının yazarı Chris Richardson, kendisinin katkıda bulunduğu microservices.io/patterns sayfası altında "Deployment patterns" olarak aşağıdakileri ayrı ayrı birer pattern olarak listelemiştir. Aşağıdaki konular bu dökümanda anlatılmamıştır.

- Multiple service instances per host - deploy multiple service instances on a single host
- Service instance per host - deploy each service instance in its own host
- Service instance per VM - deploy each service instance in its VM
- Service instance per Container - deploy each service instance in its container
- Serverless deployment - deploy a service using serverless deployment platform
- Service deployment platform - deploy services using a highly automated deployment platform that provides a service abstraction

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Private class data pattern
bir java sınıfında sınıf değişkenleri tamamen dışarıya kapatılmak istenebilir. bunun için tüm değişkenleri private yapıp setter'larını kaldırmamız gerekecek.
Fakat buda yetmez bu dataların o sınıftan dahi değiştirilmemesini isteyebiliriz. Bunun için tüm data'yı yine sınıf değişkeni olarak; bütün dataları içeren tek bir sınıf yaratabiliriz. Bu oluturduğumuz yeni sınıf consctuctor haricinde setter'a sahip olmamalıdır.

```java
class Math{
  private int kose1;
  private int kose2;

  alanHesapla(){....}
  
  //getters
}
```

Bu yapılması daha iyi olacaktır:

```java
class MathData{
  private int kose1;
  private int kose2;

  MathData(kose1, kose2){...}

  //getters
}

class Math{
  private MathData data;
  alanHesapla(){....} // can call: data.getKose1();
  .....
}
```

Artık Math sınıfımızda data.setKose1(); çağıramayız. Oysa ilk Math sınıfımızda kose1=3; yapabilirdik.

Yani dışarıya kapattığımız yetmezmiş gibi; ilgili sınıfımızda da önlem almış oluyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Facade pattern
facade kelime anlamı: cephe, Bir yapının ön tarafta bulunan bölümü

birden fazla sınıfın işlevini birleştirip çağırmak durumunda kalabiliyoruz. örneğin; getComputer() işlemi kendi içerisinde %90 getKlavye(); getMotherBoard(); gibi işlemleri yapması gerekiyor. Bunları tek tek çağırmak yerine facada sınıfı yaratıp, sadece facada sınıfından yararlanma metodolojisidir.

örnek: Simple Logging Facade for Java (SLF4J). SLF bir interface ile tüm implementasyonların değil, birçok log kütüphanelerinin kullanılabilmesi için arayüz sunmaktadır. burada SLF4J kütüphanesinin init() metodunu çağırdığımızda:
- A log kütüphanesi için 2 adet metod
- B log kütüphanesi için ise 1 metod çağrıyor olabilir.

Patternin tanımında belirttiğimiz gibi; bir metod (init metodu) diğer tüm ihtiyaçlarımızı görecek metodları çağırmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Proxy Pattern (yada vekil deseni)

CommandExecuter --> Interface olsun
CommandExecuterImpl --> türemiş sınfımız olsun
CommandExecuterProxy --> türemiş sınfımız olsun

- CommandExecuterProxy sınıfının içinde CommandExecuterImpl otomatik initialize olsun.
- CommandExecuterProxy sınıfının runCommand(String commandStr) metodunu çağırdığımızda bizim için CommandExecuterImpl metodunun runCommand'ını çağırır. fakat ek olarak önce komutu koşturabilicek yetki olup olmadığı kontrol edilir. BUnun gibi birçok kontrol yapılabilir. Benzer şekile otomatik CommandExecuterImpl initialize edilirken ona gerekli parametreleri geçer. 

Proxy pattern'i CommandExecuterImpl 'ın kullanımına aracılık ediyor ve bize kolayklaştırıyor. Daha da spesifik bir örnek verirsek; CommandExecuterProxy yerine CommandExecuterUser yada CommandExecuterAdmin isimli 2 proxy yapabiliriz. Bu proxy'ler CommandExecuterImpl'ı initialize ederken parametrelerini biliyor olucak ve komutu çalıştırmadan doğru yetkilerle kontrol edeceklerdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Delegation Pattern (yada Delegator Pattern)
bazı kaynaklarda "__proxy chains__" olarakta geçmektedir.

proxy pattern'e çok benzer.

Genelde kalıtım yerine, composition tercih edilir. işte bu durumda composition olacak sınıfın metodlarını, sınıfımızın içindeki proxy objesi ile çağırırız. bu gibi durumlarda Delegator pattern kullanılabilir.

proxy ile farkı şudur: Delegator pattern, referan olsuğu sınıfın metodlarını aynen çağırırken, proxy pattern arada loglama, kontrol gibi birçok işlev görebilmesi söz konusudur.

kaynak: http://best-practice-software-engineering.ifs.tuwien.ac.at/patterns/delegation.html (archive date: 24/11/2019)

```java
class MyClass {

  Delegator d;

  method1(){
    d.method1();
  }

  method2(String a){
    d.method2(String a);
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# adapter pattern

adapter is a bridge between two incompatible interfaces.

örnek;

```
- Reader         (Interface)
  - PpdfReader   (Implementation)
  - DocReader    (Implementation)
  - PhotoReader  (Implementation)
```

Yukarıda Photo, pdf ve doc'tan çok farklı bir media dosyası. örneğin; Reader interfacesi getAllAsText(); metoduna sahip. Fakat bunu photoda nasıl implemente edeceğiz?

Öncelikle getAllText yerine fotonun meta-data bilgilerini text olarak döndüreceğimizi kabul ederek ilerleyelim.

Foto'dan meta-data'ları döndüren sınıfımız olsun:

```
PhotoMetaDataReader   (Interface)
 - getPhotoDate()     (Method)
 - getPhotoId()       (Method)
```
 
Yeni bir adapter sınıfı yaratırız:

```java
class PhotoReaderAdapter {

    PhotoMetaDataReader p;

    getAllMetaData(){

       return p.getPhotoDate() + p.getPhotoId();
    }
}
```
Artık PhotoReaderAdapter'ı PhotoReader içinde kullanabiliriz.

Böylece PhotoReader ile PhotoMetaDataReader'ı PhotoReaderAdapter ile birbirine bağlamış olduk.
Burada adaptee rolünde olan PhotoMetaDataReader interface'sidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# composite pattern

- örnek;

Data clasımız olsun. Folder ve File implementasyonumuz olsun. File sadece tek bir "Data" Objesi barındıracaktır. Oysa Folder, bir Data Listesi barındıracaktır. Bu "Data" listesi içinde file yada folder olabilir.

Şimdi main class'ımızda ;

```java
Folder root = new Folder();
root.add(new Folder());
root.add(anotherSubFolder);
```

yukarıdaki kod akışında içiçe dizinler yaratabiliyoruz. İşte bu tarz bileşik data tutulup ağaç yapısı hazırlabilicek tasarımlara "composite pattern" denir. 

- örnek;

```java
class Employee {

  String name;
  List<Employee> subEmployees;
}
```

YUkarıdaki obje ile tüm şirketin hiyerarşisi bile yaratılabilir. en üstte genel müdür employee'si olacaktır.

- örnek;

AritmeticExpression sınıfı. Bu sınıf büyük bir denklemi barındırıyor. List<Operand> barındırıyor. Her operan yine kendi içinde operand listesi barındırıyor.

1 # ( 2 + ( 3 + 4 ) ) denklemimizi örnek alabilir. bir Operand 1,# ve X'i barındırıyor olacak. X operand'ı ise içinde; 2, + ve Y operandını bulunduracak. Y operandı ise; 3, + 4 ü barındırıyor olucak gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# bridge pattern

Satış raporlarının çıktısı alan kodlarımız olsun:

```
- Rapor         (Interface)
  - Internet    (Interface)
    - PDF       (Implementation)
    - Doc       (Implementation)
  - Magaza      (Interface)
    - PDF       (Implementation)
    - Doc       (Implementation)
```

Bridge pattern'i diyorki; ilgili sınıftan soyut olan bir yapıyı ayırın. Bizim sistemde soyut olan kavram "format" (pdf, doc) olmasıdır. PDF ve doc'u soyutlaştırmamız gerek.

Sonuçta bu olması isteniyor:


```
- Rapor         (Interface)
  - Internet    (Implementation)
  - Magaza      (Implementation)

- Format      (Interface)
  - PDF       (Implementation)
  - Doc       (Implementation)
```

Artık yeni rapor yaratmak istediğimizde yaratılacak rapor nesnesine Format sınıfını parametre geçeceğiz.

Köprü rolünde olan sınıf Format'tır. FormatBridge diye adlandırabilirdik.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Iterator Pattern 
for loop yerine iterator objesi ile dönmemizi sağlayan pattern'dir.

Iteratorler'in avantajı şudur:
- uygulamamızda tüm koleksiyon nesnelerimiz aynı iterator interfacesinden türemiş sınıfları kullanır. bu şekilde bize gelen koleksiyon tipini düşünmeden o koleksiyonda işlem yapabiliriz (döngü kurma gibi).
- koleksiyon sınıfımızdaki iterator objesi bize filtreli dönüşte yapabilir. örnek:

Radyo dinleme cihazımız olsun.

```java
Kanal {
   Frekans;
   Language;
}

class KanalListesi {

   kanalEkle(Kanal);
   kanalSil(Kanal);
   
   iterator(Language);
}
```

Yukarıdaki KanalListesi koleksiyonumuzun döndüreceği iterator'ın next(); metodu bize aldığı Language parametresine göre filtrelenip dönecektir. Zaten iterator'ın next() metodunun implementasyonunu KanalListesi sınıfını yazan yazılımcı yazacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Null Object Pattern
AbstractUser class'ımızı olsun. bundan 2 tane implementasyon olsun. 1 tanesi normal koşullarda yapacağımızı User implementasyonumuz, diğeri ise NullUser implementasyonumuz. NullUser hiç null pointer döndürmesin. örnek: NullUser.getName(); --> "User not available" dönsün. gibi. 

Bir ekran koruyucu yazılımımız olsun. Bu yazılım ekranda toplar gösterecek olsun. Toplar için efect'lerimiz olsun. Efectler biz dizinden yükleniyor olsun. dizinde hiçbir dosya olmadığında NullEffect sınıfımız devreye girecek ve default olarak ekranın efektsiz çalışmasını sağlayacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Observer pattern (yada Gözlemci deseni)

Observer tasarım deseni; birbirleri ile bire çok (yani bir nesnenin içinde başka bir nesnenin listesinin bulunması olarak düşünebiliriz) ilişki olan nesneler arasında olay bazlı bir etkileşim olduğu durumları düzenler. Örnek senaryolar:

- Bir e-ticaret sitesinde bir üründeki stok değişiminde o ürünü takip eden üyelere haber verilmesi" 

- facebookta bir paylaşıma yapılan yorumlar için paylaşımcıya ve diğer yorumculara bildirim gitmesi

İlk senaryodan gidelim;

- "Observer (yada consumer)" yapısına denk gelen sınıf "Üye" sınıfı olsun. 

- "Observable (yada Subject yada object)" yapısına denk gelen sınıf "Ürün" sınıfı olsun. 

ürün içinde, üyelerin listesi olsun.

Urun.updateFiyat(int newFiyat) metodunun içinde tum uye listesine email ile fiyatın değiştiğini haber veren işlem yapılmalıdır.

# publish-subscribe vs Observer
publish-subscribe sistemlerde arada event bus (yada sistemde adlandırabileceğimiz herhangi bir mekanizma: broker, message broker, event bus) vardır. oysa observer sistemde event bus yoktur. Observer'da, subject direk Observer'ların metodlarını çağırır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# state pattern

Her durum için ayrı nesneler yaratılıp, akışın her duruma göre davranışlarını bu yarattığımız nesnelerde bulundurabiliriz. Bu şekilde if/else gibi durumlardan kısmen kurtulmuş oluruz.

örnek:

```java
WebApp app = new WebApp(); // web app started on desktop browser which was used in a small area
...
app.alert(); //the alert was opened as OS native (big popup alert)
...
app.setState(new FullScreen()); //user now changed the browser screen as fully screen
...
app.alert(); //the alert is a modal now. because user is already focused on screen
...
```
• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Specification pattern
Yazıcağımız kaynakların dışarı açılacak metodlarını protected olabilir. Dolayısı ile herkesin erişmesini istemediğimiz metodlar olabilir. Sadece dışarıya erişime açmak istediğimiz metodları bir interface aracılığı ile açmalıyız. bu açtığımız interface'lere "contract (yada şartname)" de deniliyor.

Bu konu "service contract" başlığında daha detaylı anlatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Thread pool pattern
İlgili uygulamada thread sayısının merkezi bir modülden limitlenerek sırası ile çalıştırılma patternidir.
Merkezi modülümüz isterse thread'leri önceden işletim sisteminde hazır tutabilir, yada işi biten thread'leri kapatmadan, sıradaki thread'e direk teslim edebilir... Fakat bu detaylar pattern'e ait değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sidecar pattern (yada sidekick pattern yada decomposition pattern)
Bir uygulama yanında sidecar dediğimiz farklı bir uygulamayı bulundurur. Sidecar, asıl uygulamamızın yapması gerekn bazı işlevleri onun için yerine getirecektir. Bunun avantajını direk örnek üzerinden inceleyelim:

Spring Cloud Eureka (discovery server) client'leri java için yazılmış durumda. Peki cloud'da nodejs microservisimiz nasıl eureka'ya bağlanacak. işte böyle bir durumda sidecar pattern aklımıza gelir.

- Yeni bir microservis yaratırız.
- spring-cloud-netflix-sidecar bağımlılığını ekleriz.
- sidecar projesinin config dosyasına da nodejs'in healt-check url'sini veririr.
- nodejs uygulamamızın bir portundan sürekli olarak healtcheck (spring cloud doc'unda standardı yazıyor) status OK cevabı döneriz.

Proje ayağa kaldırıldığında sidecar sürekli olarak healcheck'e request atacak ve cevap aldığı sürece asıl eureka server'ımıza register olacak. sidecar eurekaya register olması için nodejs'e aracılık etmiş oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# database per service pattern
her microservisin kendi database'i olması mimarisidir. temel olarak şu tarz alt çözümlere gidilebilir:

  - Private-tables-per-service – each service owns a set of tables that must only be accessed by that service
  - Schema-per-service – each service has a database schema that’s private to that service
  - Database-server-per-service – each service has it’s own database server.

tablolar arası ilişkilerin nasıl tutulacağı konusu için çözümler aşağıdadır. aşağıdaki listedeki her 2 durum içinde ilişkiler db tarafında tutulduğubda, kolonlarda foreign key olamayacak.

  - 1- ilişki tablosu sadece 1 veritabanında olacak.
  - 2- ilişki tablosu ilgili tüm servislerde tutulacak.

Yukarıda 1'inci durumda bir servis eğer ilişki tablosunu içermiyor ise; her defasında ilişki tablosunu kullanan microservice istek yapacaktır. bu da network yavaşlığına sebep olacaktır.

2inci maddede ise, ilişkiler iligli servislerin veritabanlarında olacağı için 2 dezavantaj var:
  - dublicate veriler oluyor (ilişkiler birçok veritabanında saklanıyor)
  - update ve add işlemleri tüm veritabanlarında güncelleniyor. bu yavaşlık sebebi.

avantajı ise:
  - 1inci çözümde olduğu gibi her ilişki tablosuna erişim gerektiğinde diğer servislere istek yapılmıyor. direk db'den okunuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Decompose by business capability Context
Sadece CRUD servislerimiz olsun. Örnek order service, user service. bu servisler businnes logic içermesin. onların önüne farklı microservisler koyabiliriz. bu servislerimiz ise daha yüksek seviyeli işler yapsın. businnes logic içersin ve CRUD servislerini kullansın. bu tarz mimarilerde ödeki servislere __capability servis__ denir (terminolojik olarak birçok alternatifi mevcut).

burada CRUD servislerimizde olması beklenen businnes logic'leri decomposition yapmış olduk.

Bunu yapmanın avantajları:
- CRUD ile bussines logic'leri ayırmak. daha basit yapı oluşturma.
- "database per service pattern" uygulandığında ilişki tabloları için her servis diğerine gitmesi gerekecek. oysa önlerinde capability servis olsa, herşeyi o yönetse ilişki yönetimini tek bir noktadan daha kolay yapabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# microservice pattern (yada mikroservis deseni) vs Monolithic pattern (yada monolitik desen)

# kural

- bağımlılıkların neredeyse hiç olmaması (veya minimum olması) gerekiyor. aslında businnes logic'in olduğu kısımlar özellikle ayrılması gerekiyor. bazı durumlarda dependency olabilir. örneğin; tüm modellerimizi init olurken bir log atıyor olalım. bunu eski usul mimarilerde, tüm modellerimizi GenericModel isimli bir sınıftan extend ederek yapabilirdik. GenericModel 'in consrutoruna bir log metodu koymamız yeterli olacaktı. Fakat microserviste ortak bir dependency olamayacağı için GenericModel olmayacak diye bir durum söz konusu diildir. Businnes logic içermeyen kısımlar (dışardan satın alınan kütüphaneler gibi iş gören kodlar) dependency olarak kullanılabilir.

- ortak model barınmaması gerekiyor. ortak olabilecek kısımlar aynı projede olmaları gerekiyor. eğer olmuyorsa mimarimizde bir terslik var anlamına gelir.

ortak util sınfı olursa gerekirse util sınıfı rest servisi (veya SOAP veya Remote Procedure Call (RPC)) haline getirip, metod olarak diilde web servisi olarak kullanmak gerekiyor.

# avantajlar

- bir projede bir kütüphane değiştirildiğinde diğer kütüphanelerin zarar görmediğinden emin olabilir. örneğin; springmvc kullanırken aynı projede, spring'in  bir modülünü ekleyeceğiz. yeni kütüphaneyi eklerken springmvc dependency'leri ile çakışmıyor görünse de, her zaman beklendiği gibi olmayabiliyor. bu durumun önüne geçilmiş oluyor. docker gibi her proje/yazılım birbirinden bağımsız olmuş oluyor.

- her proje farklı dillerde yazılabiliyor.

- projelerde aynı kütüphanenin farklı sürümleri kullanılabiliyor

- projeler çok ufak ve fazla parçalara bölünmüş olacağından; deploymentlar daha ufak parçalar şeklinde ve daha az riskli şekilde yapılabilecektir. yani; tüm paketi birden çıkmak zorunda kalmıyoruz. continius delivery'yi kolaylaştırıyor.

- projeler çok ufak ve fazla parçalara bölünmüş olacağından; doğası gereği modülerliği arttırmaya yöneltiyor.

- bir modülde sorun çıkınca tüm sistem çökmemiş oluyor. sadece o modül kullanılamıyor.

- update işlemleri için tüm sistemi durdurmaya gerek yok.

- localde development için tüm sistemi açmaya gerek yok.

- operasyonler işlerde startup süreçleri hızlanır.

- devre dışı bırakılacak modüller için sistemi kapatmaya gerek yok. sadece o servisleri kapatmak yeterli.

# dezavantajlar

- her projenin farklı databasesi olması, özel sorgular ve atomic işlemler için zorluk çıkarabiliyor

- dependency olmaması gerektiğinden, tecrübeli yazılımcılar olmazsa birçok dublicate kod içerebiliyor.

# cloud native architecture

bir mimaridir. native doğal anlamına gelir. cloud native'den kasıt; cloud yapısına en uygun olandır.

spring'in cloud native'i karşılayan modülleri vardır. "Spring cloud netflix" ismi ile geçer çünkü tüm implementasyonlar netfix'in spring'den bağımsız geliştirdiği uygulamaları kullanabilmek için yapılmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Externalized configuration
config server pattern'idir. servislerin tek bir servisten tüm configleri çekebilmesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# api gateway
bir sunucu yazılımdır. edge server gibi çalışır. özellikleri:

- Farklı protokoller arası dönüşüm. örnek: SOA->REST, TCP->SOA
- darboğaz (throttling) yönetimi. her user'dan gelen istekleri sınırlandırmak. yani; belli zamna dilimlerinde, bazı isteklere limit getirebilmek.
- kimlik doğrulama
- api versiyonlama
- api grubu devreye alım/kapatma/açma
- Loglama, izleme, bildirim
- Servis birleştirme (service composition). birden fazla iç servisi tek bir servis olarak dışarıya açabilme.
- caching

piyasadaki bazı api gateway'ler: tyk, Kong

# kong
kong yazılımı başladığında config'leri 3 farklı yerde okuyabilir:
- bir yml config dosyasından
- özel bir port üzerinden http istekleri ile config'ler belirtilebilir. her ayar ayrı ayrı da yollanabilirken, tek bir seferde tüm config http api isteği ile de atılabilir. kong runtime sırasında kendini güncelleyebiliyor.
- config'leri database'den okuyor.

kong açıldığında default olarak bu port'lardan hizmet vermeye başlar:

- :8000 listens for incoming HTTP traffic from your clients, and forwards it to your upstream services.
- :8443 same of 8000 but SSLL enabled
- :8001 admin api
- :8444 admin api with SSL

örnek config dosyası:

```yml
_format_version: "1.1"

services:
- name: my-service
  url: https://example.com # servisimizin hizmet verdiği url
  plugins:
  - name: key-auth # bu servise istek geldiğinde devreye girecek (araya girecek) eklentiler
  routes:
  - name: my-route # sadece routing'in ID'si.
    paths:
    - /my-service1 # artık http://kong-host:8080/my-service1 'e gelen istekler https://example.com 'a yönlendirilecektir.
```

Kong'da eklentiler lua ile yazılmaktadır. eklenti altyapısı oldukça basit. Servlet filter'ı yazar gibi request ve respons'u alıp manipüle edip bir sonraki eklentiye yollabiliyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# API Composition
api gateway uygulamaları bunu bizim için yapabilir. edge proxy diye adlandırılan bu sunucular api'lerimizi dışarıdan erişime açarken tek bir endpoint'ten açar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CQS (yada Command Query Separation) vs CQRS (yada Command Query Responsibility Segregation)
Segregation kelime anlamı: ayırma

ikisi çok benzerdir fakat farklı patternlerdir.

2 pattern de metodları iki farklı gruba alınmasını önerir:

- Commands: Objenin veya sistemin durumunu değiştirir. Void dönüş yapar.

- Queries: Sadece sonucu geriye döner herhangi bir objenin veya sistemin durumunu değiştirmez.

Bir command metodu (eğer dönüş yapmasının yan etkisi olmayacaksa), dönüş yapabilir. fakat bu pek tercih edilen bir yöntem olmamalıdır.

CQS daha çok mikro seviyede, CQRS  ise makro seviyede sistemi ele alan pattern'lerdir. CQS büyük resimden bakıldığında sistemdeki herhangi bir fonksiyonun ya query yada read işlemi yapmasını önerir. oysa CQRS (büyük resimden bakarak) bunların da ayrılmasını önerir:
- fonskiyonlara yollanan ve response alınan modeller/objeler
- microservislerin sadece query veya command olarak ayrılması
- sadece fonskiyon seviyesinde değil, sınıflar bazında ayrılması (yani; bir sınıftaki tüm metodlar ya query yapacak yada command yapacak)

kaynak: https://stackoverflow.com/questions/34255490/difference-between-cqrs-vs-cqs/53449521#53449521 (archive date: 03/01/2020) Luke Puplett'in cevabında açıklama mevcut. Mesajın altında Greg Young cevap yazmış fakat Greg web projelerinde kullanımı hakkında yorum düşmüş.

CQRS ilk "Greg Young" tarafından ortaya atılmıştır.

# View Models (yada Projections yada Query Models)
Query fonksiyonlarımız data okur (veritabanından). bu data'lar yansıtılmak için kullanılan datalar olduğu için, CQRS pattern'i dökümanlarında bu şekilde isimlendirilmiştir.

Bu patternin faydaları:
- CQRS yapıldığında "event sourcing"'de kolaylaşacaktır. çünkü sadece "Commands" servisimize gelen istekleri loglamamız yeterli olacaktır.
- Sadece read yapan servislerimize ayrı performans çözümleri uygulamamıza imkan sağlayabileceğiz:
  - Sadece read yapan servislere database'yi read only açabiliriz.
  - Sadece okuduğumuz fakat yazmadığımız kısımlar var ise bu dataları farklı database'ye alabilir ve sadece read-only servislere açabiliriz.
- Bazı servislerimiz sadece database'ye read yetkisi olduğundan o servislerimizde güvenliği arttırmış oluruz.
- Sistemimizi micro servislere bölüştürmemiz kolaylaşacaktır.
- Read only query metodlarımız, yazma işlemi yapan metodlarla aynı sınıflarda olmayacağı için büyük DAO'lar karşımıza çıkmayacak.
- Database'yi değiştiren servislerin hangileri olduğunu bildiğimizden hata tespiti kolaylaşabilir

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# two-phase commit (yada 2PC)

database/network sistemlerinde transaction işlemi yapacak yazılım karşıya önce "prepare for commit" mesajı iletir. eğer OK dönüşünü alırsa "commit" mesajını, her servise ayrı ayrı gönderir. Bu yaklaşım genelde uzakta birden fazla birbirinden bağımsız veritabanlarında işlem çalıştırılacağı zaman yapılır. böylece atomic işlem daha garantiye alınmış olunur.

2PC'de yazılım, diğer servise yada veritabanı servisine "prepare for commit" mesjaını atması ve dönüşünü alması fazına __voting phase__ denir.

Klasik sistemlerde ise direk olarak ilk işlemde "commit" mesajını gönderilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# saga pattern
türkçe kelime anlamı: destan

2PC'ye alternatiftir. 2PC senkron bir çok çözümdür. saga asenkron ve reaktiftir. saga asenkron olabilmek adına, tüm mikroservislerin ortak event bus'a bağlı olmaları gerekir. bu şekilde saga, global yürüyen transaction sürecinden haberdar olabileceklerdir. kaynak: https://developers.redhat.com/blog/2018/10/01/patterns-for-distributed-transactions-within-a-microservices-architecture/ (archive date: 03/01/2020) "saga pattern" başlığındaki ilk paragraf.

yine birçok dağıtık servisimiz olsun. birçok servisimiz kendi bağımsız veritabanına kayıt/güncelleme işlemi yapıyor olsun. ortak bir event bus ile birbirleri arasında haberleşebiliyor olsun. son kullanıcı global bir transaction işlemini tetiklesin. transaction'ın ilk adımı (yani ilk event) event bus'a yollansın. benzer şekilde eğer paralel'den yollanabilecek ise diğer event'lerde yollansın. bu event'leri ilgili servisler kendilerine çekecek ve işletmeye başlayacak. ilgili event'ler tamamlandıkça diğer event'lerde çağrılacak. bazen bir event diğer bir event'i de tetikleyebilir (side effect).

peki hata olursa ne olacak? işlemler geri alınacak. her mikro servis, kendi içinde yaptığı işlemleri geri almayacak. işte 2PC ile farklar biri burada. saga'da işlemin sonucu saga'ya servis tarafından döndürülür. saga businnes logic'e göre tekrar aynı servise geri alma işlemini yollayacaktır. geri alma işlemi hiç veritabanına yazmama diildir. geri alma işlemi güncelleme yada (event sourcing kullanılıyot ise) yeni bir event state'i oluşturmak olacaktır. kaynak: https://developers.redhat.com/blog/2018/10/01/patterns-for-distributed-transactions-within-a-microservices-architecture/ (archive date: 03/01/2020) "Two-phase commit (2pc) pattern" balığında ikinci resimde görülmektedir ki, bir işlem abort edildiğinde, ilgili servise verilerin veritabanına kayıt edilmemesi isteği yollanmakta.

saga'nın debug'ı, 2PC'e göre daha zordur. kaynak: https://developers.redhat.com/blog/2018/10/01/patterns-for-distributed-transactions-within-a-microservices-architecture/ (archive date: 03/01/2020) "saga pattern" başlığındaki ilk paragraf. "Disadvantages of the Saga pattern" başlığı.

saga ile 2PC işleyiş olarak aynı gibi düşünülebilir. fakat saga yapısında; aynı anda birden fazla event(dış servis) çağrılabiliyor olması bir avantaj. unutmamak lazım ki; event'lerin başka eventleri çağırdığı durumlarda, paralel olarak event çağırmanın çok büyük avantaj olduğunu göreceğiz. 2PC'de paralel event çağırma diye bir kavram yok. çünkü işlem zaten sync olarak ilerlemektedir.

benzer şekilde saga'nın reactif olması da bir avantajdır. reactif olmasından kasıt şu: tüm event'lerin çağrılıp daha sonra thread'in kapatılması ve callback'in çağrılması yeteneği. 2PC'de işlemler bitene kadar thread'imiz açıkken, saga'da işlemler çağrılıp thread sonlanabilir. event'ler bittiğinde, bir callback metod tetikletebiliriz. 

2PC'de bir transaction sadece bir servisin kendi içinde (seviyesinde) değil, tüm servisler (sistem) seviyesinde kullanılır. dolayısı ile transaction'ın başladığı andan, bittiği ana kadar veritabanında bazı değerler kilitli kalacaktır. bu durum saga'da değildir. çünkü saga'da; 'transaction' sadece servislerin kendi içlerinde(seviyelerinde) gerçekleşecektir.

2PC'de kaynakların (örnek veritabanı) kilitlenmesi, 2PC'nin reactif manifestosuna uymamadığını gösterir. kaynak: http://insticc.org/node/TechnicalProgram/icsoft/presentationDetails/79187 (archive date: 03/01/2020) Bu link PDF indiriyor. Linkin paylaşıldığı (yazarların ismi olduğu paylaşım sayfası) link: http://insticc.org/node/TechnicalProgram/icsoft/presentationDetails/79187 (archive date: 03/01/2020) "INTRODUCTION" başlığında ikinci paragrafın ortasında.

transaction yönetimleri saga ile 2 farklı şekilde yönetilebilir (implemente edilebilir):

- __Choreography (yada Koreografi)__

  bazı kaynaklarda "event" yöntemi olarak adlandırılıyor. fakat bu çok doğru diil.
  
  Koreografi kelime anlamı: Bir dans gösterisini oluşturan adım, figür ve anlatımların bütünü.

  her event diğer event'leri tetiklemesi için event bus'a mesaj gönderir. dolayısı ile tüm sistem merkezi bir noktadan yönetilmez. örnek: bir servis tetikledi, o servis diğer servisleri de tetikleyebilir. yani bir orchestrator yoktur. kaynak: https://medium.com/trendyol-tech/saga-pattern-briefly-5b6cf22dfabc (archive date: 03/01/2020) "Choreography-Based Saga" başlığındaki ilk paragraf.

- __Orchestration__

  bazı kaynaklarda "command" yöntemi olarak adlandırılıyor. fakat bu çok doğru diil.

  merkezi bir yerden tüm event'ler çağrılır. çağrılan servisler diğer başka servisleri tetiklememelidir.
  
Choreography biraz daha çok saga'ya yakınken, Orchestration biraz daha çok 2PC'e yakındır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# event sourcing pattern
order tablosundan gidelim. order'ın tamamlanması için birçok aşama vardır. işleme alınma, ödeme alınma, kargoya verilme gibi... bunların her biri event'tir bu event'ler "event store" da tutulmalı.

böylece;
- order'ın son haline event'leri takip ederek bile ulaşabiliriz.

  not: order'ın so halini sadece CQRS'teki query'lerimiz için bir tabloda tutarız. fakat her event sonunda bu order tablomuzu güncellemeyiz. sadece tüm event'ler tamamlandığında order tablomuzu güncelleriz.

- eski state'e dönüp sistemi debug edebilmemizde büyük avantaj sağlar.

- event-driven architecture sağlar (reactive sistemler için ideal bir yapı)

- her event ne yapacağına kendi karar verecektir. bu da daha modüler/mikro servis mimarisine geçişimizi uygun kılacaktır.

# isimlendirme
event isimler geçmiş zaman olmalı ve fiil içermelidir. uygulamanın hangi durumda olduğunu net bir şekilde açıklamalıdır. örnekler: 

| Badly named     | Well named      |
|-----------------|-----------------|
| AccountInserted | AccountCreated  |
| AccountUpdated  | FeeCharged      |
| AccountUpdated  | MoneyWithdrawn  |
| AccountUpdated  | AccountCredited |
| AccountDeleted  | AccountClosed   |

# Event storming
domain expertlerin ve devleoper'ların bir araya gelerek yaotığı toplantıdır. bu toplandıda event'lerin ne olduğuna karar verilir.

# domain events vs event-sourcing

domain event'leri microservislerimiz arasındaki haberleşmede kullanılan event'ler olarka düşünülebilir. event sourcing ise her microservisin kendi içinde tuttuğu event'lerdir. bu 2 event'ler ayrı ayrı tablolarda tutulması önerilir. böylece her node bağımsızca test edilebilir ve geliştirilebilir olacaktır.

bu durumu bu linkteki: https://www.innoq.com/de/blog/domain-events-versus-event-sourcing/ (archive date: 18/10/2019) "Events from Event Sourcing ≠ Domain Events" başlığındaki bu resim iyi açıklamaktadır: https://www.innoq.com/de/blog/domain-events-versus-event-sourcing/context-map.png (archive date: 18/10/2019) 

# snapshots
event'lerin sayısı çok artabilir ve bir command birçok event çalıştırabilir. dolayısı ile, bir her event son durumu (state'i) okumak için sürekli ilk event'in log'undan son event'in loguna kadar okuyup, hesaplama yapması gerekebilir. örneğin; aynı kullanıcı toplu transaction işlemleri üstüste yapıyor. her event; yani transaction öncesi bakiyede para kalıp kalmadığını kontrol edeceğiz. tüm eventlerin ne kadar harcadğını bulmak zorundayız. bunu yapmak yerine 10 adet işle sonrası yeni bir snapshot yaratabiliriz. bu snapshot'ta soon durumun özeti olabilir. bu snapshotları event tablosunda da tutabiliriz yada farklı bir tabloda sadece snapshot'ları tutabiliriz.

her snapshot'un event tablosunda tutulduğu bir örnek: https://theburningmonk.com/2019/08/a-simple-event-sourcing-example-with-snapshots-using-lambda-and-dynamodb/ (archive date: 18/10/2019)

snapshot'ların ne aralıklarda alınacağı, sistemin mimarisi, uygulamanın domaini ve performans ölçekleri gibi birçok parametreye bağlıdır. snapshot almak; hem maliyetli hemde kod logici ve karmaşıklığı getrdiği unutulmamalıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Circuit Braker pattern (yada devre kesici deseni)
başka başlıkta anlatılıyor (Hystrix ile birlikte anlatılıyor).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Service registry
"discovery server" konusu altında (başka başlıkta) anlatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Client-side Service Discovery Pattern vs Server-side Service Discovery Pattern
her iki pattern'de de, service discovery'ye her node kendi ip'sini bildiriyor.

Client-side pattern'inde her node, service discovery'ye erişerek, diğer node'lara gideceği ip adreslerini alır.

Server-side pattern'de ise her node direk olarak sistemdeki "load balancer"'a istek yaparak gitmek istediği servise erişir. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Self Registration pattern
her node'un kendini service regisrty'ye kaydetmesi gerekmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Blue-Green Deployment Pattern
tüm mikroservisleri durdurup yeni sürüm çıkmak ve hata olma durumunda geri alınmasını beklemek çok zaman alabilir. bunun önüne geçmek için bir ortamda daha yeni sürüm sistem ayağa kaldırılır. eski sistem hala ayaktadır. eski sistem sadece tek bir yönlendirme yapılarak canlıya alınmış olur. bu yönteme Blue-Green Deployment denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Microservice chassis
chasis türkçe karşılığı: şasi (Motorlu kara taşıtlarının iskelet bölümü)

her yeni mikroservis oluşturulduğunda bir framework'ten yararlanıp, cross-cutting concern'lerin (loggining, security...) tekrar tekrar yazılmaması ve bununla zaman kaybedilmemesi gerekmektedir. örnek spring boot-starter bunun için ideal bir örnektir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Domain driven design (yada DDD)
DDD ilk olarak Eric Evans tarafından "Domain-Driven Design: Tackling Complexity in the Heart of Software" kitabında 2003 yılında ortaya atılmıştır. Daha sonra, 2014'te Eric, tanımların referanslarını kısaca yazdığı "Domain-Driven Design Reference: Definitions and Pattern Summaries" isimli kitapta özetlemiştir ve çok ufak eklemeler yapmıştır.

Büyük çapta ve businnes logic'in kapsamlı ve kompleks olduğu uygulamalarda kullanılması tercih edilen bir yaklaşımdır. Uygulamada businnes logic'lerin bulunduğu ve domain uzmanlarına (analist veya/ve product owner ve/veya sektörde bu işte çalışan birileri olabilir) gösterildiğinde dahi anlaşılabilecek kadar diğer kodlardan izole olmuş bir katman yapılması önerilir. Bu katmandaki isimlendirmeler teknik değil; domain uzmanlarının verdiği isimler olmalıdır. Bu sebeple bu isimlendirmeye "Ubiquitous (yaygın) Language" adı verilmiştir. Bu katmanın dahil olduğu 4 temel katman bulunmalıdır:

- Presentation: web servislerinin bulunduğu, önyüzlerin bulunduğu kodlar.

- Application: bu kısım use case'leri içerir. örneğin stok kontrolü...

- Domain: tasarımın asıl ihtiyacı. yukarıdaki paragrafta açıklandı. bu kısım businnes logic'lerin olduğu asıl kısımdır. Sistemin tüm diğer katmanları bu katmana hizmet verecek şekilde tasarlanmalıdır.

- Infrastructure: diğer hizmetlerin (mail, database...) ve kütüphanelerin bulunduğu sistem. bu kısmı diğer (yukarıdaki) 3 temel katmanın içine atmıyoruz.

DDD, birçok pattern'i öneriyor/içeriyor. Bunların en başında OOP gelmektedir.

Eric'in kitabından bazı notlar:

- bounded context

  Bir entity herkes için farklı görünebilir. örnek; satılan bir ürün; aynı şirkette,
  - satış departmanı için satılacak ürün
  - lojistik depatmanında kargo
  - yazılımcı içinse bir java sınıfıdır

  Herkes için farklı görünebilir, fakat aslında aynı kaynaktan (varlıktan) bahsediliyor.

  "Bounded context" bir entity'nin belli sınırlar içindeki anlamını ifade etmek için kullanılan bir terimdir.

- Ubiquitous Language

  kod, diyagramlar, analizler, takım arası iletişimdeki konuşmalar ve yazışmalar tek bir dil üzerinden yürümelidir. bu dil domain expert'lerinin kullandığı dil olmalıdır. herkes bu dili kullanmaya zorlanmalıdır.

- continious integration 

  sürekli test otomasyonları ile test yapılmasının önemine değinilmektedir. hem integration hemde unit testler sürekli koşulmalıdır. sürekli test; domain/businnes ihtiyaçların doğru karşılandığından emin olabilmemizi sağlayacaktır.

- model designers

  developer ekibi her zaman domain expert'ler ile birlikte çalışmalı ve Ubiquitous Language ile iletişimde olmalıdır. kodun herhangi bir yerine müdahale edecek tüm yazılımcıların domain modellerine hakim olması gerekmektedir.

- layered architecture

  domain models should be free of how storing themselves, displaying themselves...

- Domain Events

  domain expertlerini de ilgilendiren olaylar kolay takip edilebilir olmalıdır. aynı zamanda her event'in ismi/tanımı ve ne yaptığı açıkça belirtilmeli ve herkes tarafından event sonrası ve öncesi nelerin gerçekleştiği herkes tarafından bilinmeli. isimlendirmeler yine ortak (Ubiquitous) olmalıdır.

- servisler

  objelerin doğasında olmayan yazılımsal süreçler servis olarak tasarlanmalıdır. servis isimleri yine ortak (Ubiquitous) olmalıdır.

- modüller

  - bir modül içinde bulunan modeller ve diğer sınıflar, diğer modüllerdeki modellere "low coupling" olacak şekildeyse, modüllerimizi optimum bölünmüştür demektir.

  - Model, Services, Repositories gibi yazılımsal anlamdaki konseptlerine göre bölünmemeli. Bunun yerine domain temelli bölünmelidir: UserRepository, UserService...

- Aggregate vs Aggregate Root

  birden fazla entity'nin transaction'larda birlikte uyumlu şekilde iş akışını tamamlamaları gerekir. birden fazla entity'nin birlikte kullanılması durumu "aggregate" olarak (kitapta) ifade edilmektedir.  
  
  "aggregate root" birlikte çalışlan entity'lerin arasında olan temel entity'ye verilen isimdir. 
  
  örnek: "sipariş" bir 'aggregate root'tur. ancak siparişe bağlı, 'ödeme bilgisi', 'müşteri bilgisi' entity'ler aggrete root'a bağlı normal entity olarak nitelenirler. fakat hepsi tek bir çatı altında düşünülür.

  Aggregate Root direk olarak diğer Aggregate Root'larla iletişime geçer. bir Aggregate Root, bir entity ile direk haberleşmez.

- Repository

  her Aggregate Root diğer Aggregate Root'lar üzerinde manipülasyon yapmak istediğinde, repository'lerden yararlanmalıdır.

- Refactorying, Clean&Readable Code, Factory sınıfları, takım çalışmasının önemi, side effect free functions...

  Önemlerine bir kez daha bu kitapta özellikle değiniliyor. zaten çoğu bilgi kitap ilk yazıldığında piyasada bilinmiyordu.

# DDD'den ayrı fakat içinde geçen bazı terimler

  - # domain
    domain is the field for which a system is built. Airport management, insurance sales, coffee shops...

  - # model
    representation of a real world object. example: student class is not a real student object. student class includent only name, surname, age and some others representations.

  - # domain model
    a model (class) which is using by a domain.

  - # domain model vs database object
    domain objeleri, database objelerine benzer fakat database objelerinden tamamiyle ayrı olabilir. ilk uygulama tasarlandığında buradaki modeller tasarlanmalıdır. daha sonra db-admin bunları nasıl isterse o şekilde databaseye kaydeder. databasede hızlı arama olsun diye bazı şeyler bölünebilir vs... 
  
    bu ikisi arasında gerçek hayata benzer olan objeler domain objeleridir.

  - # entity
    veritabanındaki bir tablonun yazılım tarafındaki temsilidir.

  - # value object
    property'leri eşit olduğunda, birbirleri yerine kullanılabilen objeler'dir.
    
    örnek: 
    
    Ahmet1 ve Ahmet2 veritabanı objelerimiz olsun. ikisinin tüm property'leri (isim, soyisim, yaş...) aynı olsun. buna rağmen bu objeler birbirlerine eşit değildir. bu sebeple bu objeler "value object" değildir.

    örnek:

    Koordinat düzlemindeki noktayı tutan bir point1 ve point2'miz olsun. bu objelerin property'leri (x, y, z) eğer ynı ise bu 2 obje birbirlerine eşit demektedir. bu sebeple point objesi bir "value object"'tir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# MVC vs MVP vs MVVM
bu konuya girmeden önce birkaç terimi incelemek gerekli.

- # concern
    concern türkçe kelime anlamı: ilgi, endişe, ait olmak

    yazılım projelerinde concern her bir fonksiyonalitenin yazılım tarafındaki katmanıdır. örneğin loggining layer'ı bir concern'dir.

- # Cross-Cutting Concern
    yazılımımızın bir katmanında diğer concern'lere ihtiyaç duyabiliyoruz. loggining gibi concern'ler özellikler diğer tüm katmanlar tarafından ihtiyaç duyulmaktadır. başka katmanlardan çağrılan concernlere "Cross-Cutting Concern" denir.

- # Separation Of Concern
    SoC concern'lerin birbirinden ayrılması için tasarlanmış pattern'lerdir/çözümlerdir.

    aşağıdakilerin her biri SoC çözümü için geliştirilmiş birer patterndir:

    - CQRS
    - MVC
    - MVP
    - MVVM

- # aspect oriented programming (yada cephe yönelimli programlama yada AOP)
    bu konu başka yerde de anlatılıyor.

    AOP, concern'leri birbirinden ayırmayı temel alan programlama felsefesidir.

# MVC (yada Model View Controller)

mvc'de contoller view'a bağımlıdır. contoller'ın bir metodunu çağrırıken, view yoksa muhtemelen exception alırız. çünkü örneğin; muhasebe sayfamızda kullanıcının parası bittiyse "0 tl" kırmızı ile gösterilecektir. bu sebeple unit test contoller'ların metodlarını çağırarak yapamıyoruz. (Bu sebeple MVP, MVC'den türetilmiştir.)

bir android projesinde ilk düşünüldüğünde MVC şunlara karşılık gelebilir:
- M: herhangi bi sınıftaki data sınıfı yada objesi
- V: layout dizinindeki xml dosyamız
- C: activity sınıfımız

oysa bu bakış açısı doğru diil. programı yazarken mimariyi biz tasarlamaktayız. örneğin; layout oluşturmayıp, tüm GUI elementlerini activity'mizde oluşturabiliriz. araştırma yapılırsa MVP gibi birçok patter'nin android porojelerinde kullandığı görülebilir.

Model sınıfı, sadece dataların tutulduğu sınıf diildir. Businnes logic'te burada olduğu unutulmamalıdır. Contoller sadece view ve model arasında bir aracıdır.

# MVP (yada Model View Presenter)

MVC'deki contoller yerine Presenter gelir, ve MVC'deki View artık daha çok View değişikliklerinden de sorumludur. örnekle ilerleyelim:

```java
class MyPresenter {

    public MyPresenter(MyView view, MyModel model) {

        this.view = view;
        this.model = model;
    }

    public void onEventX(int buttonNumber) {

        model.change(model.change());

        view.showWarning("You create an event!"); //it does not cares what views does. view can show alertbox or can show small text inside screen.
    }
}
```

Burada MyView bir interface. MyView'ın View tarafında implemente edilmesi gerekiyor. Yani view, her olayda kendinde nelerin güncelleneceğini belirlemesi gerekiyor. Dolayısı ile view'ı mock'layıp, presenter'ın metodlarını çağıran Unit testler yazabiliriz.

Yukarıdaki kod örneği MVC'de olsaydık: view.showRedAlertBox("You create an event!") olacaktı.

MVP ikiye ayrılıyor:

- # Passive View

  Presenter, modeli güncelledikten sonra view'a yeni model'i yollar. daha doğrusu; ekranda güncellenecek yeri yollar. örnek:

  view.showAlert("paranız azaldı:" + model.getMoney() );

  view'ın model değişikliğinden haberdar olabilmesi için Presenter'ın view'a bildirmesi gerekir.

- # Supervising Controller

  passive'dekinin tersine, modelde yapılan bir değişiklik otomatik olarak view tarafından bilinir. view kendini update eder. bunun için binding yapılması şarttır.

# MVVM (yada Model View ViewModel)

MVP'den sonra türetilmiştir. Çünkü MVP projeleri observer yapısını direk olarak sağlıyor (ki bu güncel mimarilerde özellikle çok tercih ediliyor).

MVVM'de Presenter yerine ViewModel kullanılıyor.

MVVM'de data binding (yada reactive tarzı streamler) kullanmak şart. Çünkü; View, ViewModel'a erişiyor fakat tersi yapılamıyor. ViewModel, modele erişebiliyor fakat tersi yapılamıyor. Bu sebeple MVVM projelerinde çoğunlukla MVVM altyapısı sunan framework'lerden yararlanılır. MVVM android tarafından resmi olarak desteklenmektedir.

View, model'deki verilere data binding yapar. Ve event'leri ViewModel'a bildirir. ViewModel model'i günceller. modelde olan değişiklik, view'a binding olduğu için otomatik yansır.

MVVM; MVP'nin Supervising Controller'ine çok benzer. Fark şudur: ViewModel, view'a erişemez.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# data access object (yada DAO)

örneğin Employee sınıfı modelimiz olsun. bunun erişimi için gerekli aşağıdaki sınıfları bir EmployeeDAO isimli sınıfına koyabiliriz:

```java
List<Employee> findById();

List<Employee> findByName();

boolean insertEmployee(Employee employee);

boolean updateEmployee(Employee employee);
```

Bazıları pattern olmadığını, sadece "seperate of logic" temeline uyduğunu belirtiyor. Bazıları ise "Structural" pattern altında olduğunu belirtmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# prototype pattern
Prototip deseni belirli nesnelerin kopyasını oluşturarak daha sonra yapılacak işlemler için, oluşturulan kopyanın kullanılmasını sağlayan yaratımsal desendir.

```java
User user2 = user1.clone();
user2.setName("mehmet");
```

Örneğin biyolojik bir hücre kendini iki parçaya blüyor ve aynı hücreden iki tane oluyor.

Bazı durumlarda new ile obje yaratmak çok maliyetli olabiliyor. Bu gibi durumlarda terchi edilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# singleton pattern
bir objeyi, tüm uygulamada aynı instance'ı kullandırma yöntemidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# builder pattern

```java
User myUser = User.UserBuilder("ahmet", "agaogli")

                  .age(30)

                  .phone("1234567")

                  .address("Fake address 1234")

                  .build();
```

avantajlar:
- okunaklılığı arttırıyor. bir constuctor'da 10 tane parametre olursa, hangi parametre nedir anlaşılmaz.
- constuctor'lara tüm parametreleri vermek istemeyiz. null göndermek yerine bu yapı tercih edilir. 10 adet alabilen bir constuctor için tüm kombinasyonları hazırlamak maliyetli olacaktır ve kod kalabalığı yaratacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# factory pattern
tek bir metod ile; new MyClass(param1, param2...) gibi bir yapıya ihtiyac duymadan yeni sınfıları türetme yöntemidir. örneğin; sadece getMyClass(param1, param2...) yazıp aynı ihtiyacı giderebiliriz.

Bu pattern'de bir factory sınıfı vardır ve bu sınıfın içinde factory metodları vardır. factory metodlar dışında public metodlar olmamalıdır.

# Factory Method pattern
"factory pattern" ile aynıdır. tek fark özel bir sınıfta değilde, zaten varolan bir sınıfın içinde factory metodu yazılmasıdır. ayrı bir factory sınıfı yaratmaya gerek kalmaz.

# abstract factory pattern (yada Factory of Factory pattern)
"factory pattern" ile aynıdır. fakat burda döndürdüğümüz sınıf birden fazla çeşitte olunca bu kulanılır. döndürdüğümüz sınıflar tek bir sınıftan implemente edilmiş olmalıdır (bu Object orianted mantığından gelir).

# static factory metod vs constructor

örnek: 

> New BigInteger("3")

vs

> BigInteger.valueOf("3")

- static'lerin avantajları:

  - constuctor'da metod ismi kullanmaz. sınıf ismi kullanır. bu sebeple constructor'ı çağırdığımızda yapılacak işin amacı için dökümantasyona bakmak şarttır

  - static metod bize zaten varolan instance'ı dönebilir. oysa consctuctor yeni instance yaratması şarttır.

  - bazı diller constructor overload'a izin vermiyor. eğer SL4J, SLF.net gibi tüm cross-language facada hazırlanacaksa static factory kullanmak gerekebilir.

  - static factory metod'lar aldığı parametreye göre (uygunluğu varsa) farklı bir sınıfta dönebilir. bunu constructor'da yapamayız.

- static'lerin dezavantajları:

  - OOP felsefesine aykırı

  - constructor'u olmayan sınıflar'dan subclass yaratılamaz. protected constructor yaratılır, fakat başka paketlerden çağrılamazlar. bu dezavantajı gidermek için "composition over inheritance" yapılabilir.

  - direk consturctor çağırmak kolayken, static factory metodları kullanmak ve bulmak için API'ı iyice okumak gerekir.

genelde kullanılan static factory metod isimlendirmeleri:

- from: örnek Date.from(instant); isntant birçok formatta olabilir. from bize istediğimiz sınıf yada subclass'a cast edecek.

- of: Set\<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING); birçok parametre alarak bize uygun olan classı döner.

- valueOf: from ile aynı.

- getInstance veya instance: Calendar.getInstance(); getInstance bazen ekstra paramere isteyebilir.

- create or newInstance: Array.newInstance(classObject, arrayLen); yeni bir instance döndürür. ayarları prametre olarak alır.

Yukarıdaki isimlendirmeler zorunlu diildir. fakat genelde bu isimlendirme kullanılır. örneğin; Calendar.getInstance() aynı instance'yi döndürmüyor. singleton pattern'de getInstance metodu aynı instance'yi döndürür. fakat calendar buna uymamış. calendar bu metodu şu amaçla böyle isimlendirmişler: önce default Locale'i bul ve bana instance'yi oluştur.

############################

############################
# DATABASE
############################

############################

# Bazı SQL komutları

- Ayni olan degerleri gormemek icin

```sql
SELECT DISTINCT City FROM Customers;
```

- sıralı çekmek için

```sql
SELECT # FROM Customers
ORDER BY Country DESC; CustomerName
```

- tabloya ekleme ya sutun isimleri belirtilerek, yada belirtilmezse kolon sırasına göre yapılır.

```sql
INSERT INTO table_name
VALUES (value1,value2,value3,...);
```

yada

```
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
```

- update

```sql
UPDATE Customers
 SET ContactName='Alfred Schmidt', City='Hamburg'
 WHERE CustomerName='Alfreds Futterkiste';
```

- delete

```sql
DELETE FROM Customers
 WHERE CustomerName='Alfreds Futterkiste' AND ContactName='Maria Anders';
```

- show only top X rows

```sql
SELECT TOP 2 # FROM Customers;
```

yada

```sql
SELECT TOP 50 PERCENT # FROM Customers;
```

- IN Operatörü

```sql
SELECT column_name(s)
 FROM table_name
 WHERE column_name IN (value1,value2,...);
```

- Beetween operatörü

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

- AS Operatörü

Result veya sorgu sırasında kolon isimlerinin değiştirilmesi

```sql
SELECT CustomerName, Address+', '+City+', '+PostalCode+', '+Country AS  Address
 FROM Customers;
```

başka bir örnek;

```sql
SELECT CustomerName, CONCAT(Address,', ',City,', ',PostalCode,', ',Country)  AS Address
 FROM Customers;
```

başka bir örnek;

```sql
SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Customers AS c, Orders  AS o
WHERE c.CustomerName="Around the Horn" AND  c.CustomerID=o.CustomerID;
```

başka bir örnek;

```sql
SELECT Orders.OrderID, Orders.OrderDate, Customers.CustomerName
FROM  Customers, Orders
WHERE Customers.CustomerName="Around the Horn" AND  Customers.CustomerID=Orders.CustomerID;
```

- JOIN Operatörü

```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM  Orders
INNER JOIN Customers
ON Orders.CustomerID=Customers.CustomerID;
```

- INNER JOIN: sadece iki taraftada olan satırları döndürüyor
- LEFT JOIN: sol tarafta olan tüm satırları döndürüyor
- RIGHT JOIN: sağ tarafta olan tüm satırları döndürüyor
- FULL JOIN: her iki tabloda olan satırların hepsini döndürüyor


- UNION Operatörü

```sql
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
```

"UNION ALL" kullanılırsa; dublicate value'larda gelecektir.


- SELECT INTO operatörü

Copy all values to another table

```sql
SELECT *
INTO CustomersBackup2013
FROM Customers;
```

- INSERT INTO operatörü

```sql
INSERT INTO Customers (CustomerName, Country)
 SELECT SupplierName, Country FROM Suppliers;
```

- create table

```sql
CREATE TABLE Persons
(
PersonID int,
LastName varchar(255) NOT NULL,
FirstName varchar(255) UNIQUE,
Address varchar(255),
City varchar(255)
);
```

- tablo şeması değiştirme

```sql
ALTER TABLE Persons
ADD DateOfBirth date
```

- View yaratma

```sql
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```

- Hazır fonksiyonlar

Örnekte count fonksiyonunun kullanımı mevcuttur:

```sql
SELECT COUNT(*) AS NumberOfOrders FROM Orders;
```

- Group By operatörü

fonksiyonlar kullanıldığında, fonksiyonun işleneceği değer kümesi group by tarafından belirlenmektedir. aşağıdaki örnekte column_name aynı olan satırlar için my_function(column_name) metodu çağrılmaktadır.

```sql
SELECT column_name, my_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

Group by kullanılınca, WHERE metodu metod çağrılmadan önce uygulanmaktadır. metod çağrıldıktan sonra where kullanımı HAVING" keywordu ile olmaktadır.

- Truncate Table_name

Tablonun içeriğini komple siler. Bu silme sorgusu "delete \*" tarzı sorgulardan daha hızlıdır. çünkü hiçbir döngü yapmadan direk içeriğin referansını siler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mongo DB User/Auth

mongo da varsayılan olarak veritabanında hiç kullanıcı yokken herkes erişimi sağlayabilmektedir. bir kullanıcı eklendiğinde artık erişimlerin tümü kısıtlanmıştır.

mongo'da "admin" database'si sistem tarafından kullanılan bir database'dir. bu database'nin içinde kullanıcılar ve rol bilgileri collection'larda tutulur. bu sebeple "admin" databasesi, örneğin robomongo tool'unda "System" klasörü altında gösterilir.

ilk mongo açıldığında "admin" databasesinde bir user yaratılmalıdır. bu user'a "userAdminAnyDatabase" rolü atanmalıdır. bu role ile bu kullanıcı artık yaratılacak her yeni database'ye yeni kullanıcı ekleyebilir hale gelir. bu kullanıcı collection'lara veri ekleme çıkarma gibi işlemler yapamaz. bu rol'de atama işlemini şu komutlarla yapabiliriz:

```js
use admin

var user = {
  "user" : "admin_user",
  "pwd" : "manager",
  roles : [
      {
          "role" : "userAdminAnyDatabase",
          "db" : "admin"
      }
  ]
}

db.createUser(user);

exit
```

şimdi mongo'ya bu yetki ile bağlanıp yeni bir database ve ona bağlı bir user yaratalım:

> mongo admin -u admin_user -p

```js
use test_db

var user = {
  "user" : "test_db_user",
  "pwd" : "123",
  roles : [
      {
          "role" : "readWrite",
          "db" : "test_db"
      }
  ]
}

db.createUser(user);

exit
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Oracle Exadata (yada Oracle Exadata Database Machine)
sadece oracle db kullanımı için optimize edilmiş, oracle tarafından satılan fiziksel sunuculardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# varchar vs char

- VARCHAR is variable-length (dynamic)
- CHAR is fixed length.

char; varchar'a göre çok daha performans sağlar fakat daha çok yer kaplar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# code first vs database first vs model first
birer database yazılım ilişki kurma yaklaşımlarıdır (yada approach).

- ## code first
önce kod yazılır, ardından entity framework yazılımı db'yi oto oluşturur.

- ## database first
önce sql kodları ile db oluşturulur, ardından yazılımcı buna denk gelen modelleri kodda yazar.

- ## model first
genelde IDE'lerde db dizayn araçları ile yapılan bir işlemdir. db'yi bu araçta hazırlarsınız, bu araç hem kod hem de db tarafını otomatik oluşturur. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# pivot table

pivot türçe anlamları: en önemli nokta, püf nokta, çark etmek, eksen üzerinde dönmek.

pivot olarak belirlediğimiz sutuna ait satırları, sutun haline çevirip oluşturduğumuz tabloya pivot tablosu denir.

en basit örnekle ilerleyelim:

- bir siparis tablomuz var.

- Bir müsterinin verdigi her bir siparis bir satirda gösteriliyor.

- Her müsteri için son 6 aylik siparis bilgilerini görmek istiyoruz.


> ||| SiparisID ||| MusteriAdSoyad ||| UrunAdi ||| Tutar ||| Donem |||

tablomuz olsun.

pivot tablomuzu oluşturursak;

```sql
SELECT *

FROM (

      SELECT 

       MusteriAdSoyad

      ,Donem

      ,sum(Tutar) as ToplamTutar  

      FROM Siparis

      group by MusteriAdSoyad ,Donem

     ) as gTablo

PIVOT

(

  SUM(ToplamTutar)

  FOR Donem IN ([200911],[200912],[201001])

)

AS p
```

Tablomuz şöyle olacaktır:

> ||| MusteriAdSoyad ||| 200911 ||| 200912 |||

MS Excel gibi uygulamalarda da pivot tablosu yapabilmek için fonksiyonlar hazır sunulmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# obje
veritabanı dünyasında obje; tablo, view, index'lerin (ve daha fazlası fakat herşey değil!) grup ismidir. bu durumda; örneğin USER tablosu bir database instance'sidir.

# Schema (yada şema)
1 veritabanında birden fazla şema olabilir. her şema altında birden fazla tablo vardır. Şema aslında tabloların, ilişkilerin ve daha birçok database objesinin tanımıdır.

A user'ı, Y şemasının altındaki tüm objelere erişebilir gibi ayarlamalar kolayca yapılabilir.

bir tablonun ismini yazarken şu şekilde formatlarız: şema_ismi.tablo_ismi

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Redis
- açık kaynaklı
- key-value db
- in-memory
- belirli aralıklarla (isteğe bağlı olarak) verileri dosyaya kaydedebilir.

# h2
- in-memory

# SQLite
sadece bi dosya içinde saklanabilen ilişkisel veritabanı altyapısıdır. sunucu runtime yazılımına ihtiyaç duymaz. çünkü sadece 1 tek dosyadan ibarettir. bu dosyaya erişenler dosyayı okuyup veritabanından neler olduğunu görebilir. aslında sadece bir dosya formatıdır. yazılım veya bir servis değildir.

# DB2
IBM tarafından geliştirilen ilişkisel veritabanı. güncel sürümlerinde no-sql tarzı özelliklerde sunmaya başlamıştır.

# infinitispan
- key/value in-memory database
- java embeeded library

# HSQLDB (Hyper SQL Database)
- açık kaynaklı
- relational db
- written in Java
- supports JDBC
- very small
- in-memory

# Derby
- relational database
- developing by Apache
- supports JDBC
- in-memory

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Strong (güçlü) ve weak (zayıf) entity

bir tablo (entity) sadece kendi başına bir objeyi ifade edebiliyor (kendi başına yaşayabiliyor / anlam ifade ediyor ise) o tablo güçlüdür. tersi durumda zayıftır. örneğin; "oda" zayıf bir nesnedir. çünkü bina olmadan, oda hiçbir şey ifade etmez. fakat bina tek başına yeterlidir. bina evdende kendi başına yer alabilir.

aslında tam olarak resmi bir tespit etme yöntemi vardır. şu örnekten gidelim:

Customer(customerid, name, surname) --> dışardan tablo referans edilmesine gerek yok. kendi başına yeterli. güçlü.

Adress(addressid, adressName, customerid) --> customerid ile customer verilmesi lazım. adress güçsüz bir varlık.

Adres bir müşteriye ait olmasa; güçlü olacaktı. yani; veritabanına göre gerçek hayattaki nesneler güçlü yada güçsüz olabilir. gerçek hayattan örnek vererek tespit doğru değildir. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# aday anahtar (yada candidate key)
join yada benzeri bir sorgu sonucu dönen sonuç tablosunda; bir yada birden sutun, o tablo için private key oluyosa; bu sutunlar aday anahtar demek oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# data domain
veritabanlarında data domain; bir sutunun alabileceği değerler kümesini belirtmektedir. örneğin; tc, 10000000 ile 99999999 arasında değer alabilir. is_enabled, true yada false alabilir gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Normalizasyon
tekrarlanmayı engellemek için best practice'lere uygun veritabanı modeli uygulamak için yapılan işlemlerdir.

# Denormalized
1NF'in dahi karşılanmadığı veritabanı dizaynıdır.

Aşağıdaki örnek veritabanında Adres bilgileri calistigi ofisin adres bilgileridir. Sutun ismi uzun olacağından anlaşılır bir sutun isimi yazılmadı.

> ||| AdresId ||| Name ||| Surname ||| Adres1_İlçe ||| Adres2_İlçe ||| Calistigi_Ofis |||

# 1NF (yada 1st normal form)
normal form: model/tip anlamına gelen norm ve form kelimelerinin birleşimidir.

- You can only have one value in a column (virgüllerle aynı sutunda değer olmamalı)
 -You should not create multiple columns for a one to many relationship

> ||| AdresId ||| Name ||| Surname ||| Adres ||| İlçe ||| Calistigi_Ofis |||

Adres2_İl'i eskiden dolu olan satırlar 2 satır olarak ayrı ayrı yazılmak durumunda kalacaklardır.

# 2NF
Her NF, derecesinden düşük NF'leri zaten destekliyor olması gerekiyor.

- (primary key birden fazla sutundan oluşuyor olabilir. bunu gö önünde bulundurarak;) her non-primary sutun; primary sutuna bağlı değilde, sadece 1 parçasına(sutununa) bağlıysa, o parçasına bağlı olduğu sutun ile birlikte farklı bir tabloya alınmalıdır.

Yukarıdaki örnekte primary-key: Name+Surname+Calistigi_Ofis birliktedir. Adres bilgileri sadece Calistigi_Ofis'ya aittir. bu sebeple farklı tabloya taşınırlar.

> ||| Name ||| Surname ||| Calistigi_Ofis |||

> ||| AdresId ||| Adres ||| İlçe ||| Calistigi_Ofis |||

# 3NF

- her non-primary sutun direk olarak sadece primary sutuna bağlı olmalıdır.

Örnekte ilk tablo olmuş 2NF'e uymuş. ikinci tabloda 2nf'e uymuş. bu durumda 3nf'i kontrol etmek gerekmekte. ilk tablo 3 nf'e uymuş. fakat ikinci tablo 3nf'e uymamış. çünkü; ilçe sutunu adresId'ye tam bağımlı değil. zaten bu sebepten tekrarlanabilir veri oluşturuyor. örneğin; ilçe 10 kere tekrar edilebilir o sutunda. ilçe farklı bir tabloya atılmalı.

> ||| Name ||| Surname ||| Calistigi_Ofis |||

> ||| AdresId ||| Adres ||| İlçeId ||| Calistigi_Ofis |||

> ||| İlçeId ||| İlçe |||

##### Daha fazla form'lar mevcuttur:

- Boyce–Codd normal form (yada BCNF yada 3.5NF)

- 4NF

Fakat en çok ilk üçünden bahsedilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# function vs stored prosedure
fonksiyonlar veritabanında insert, update, delete gibi herhangi bir değişiklik yapacak işlem yapamazlar. bunun sonucu olarak (mantıken); çoğu zaman en az bir parametre alması ve mutlaka bir değer döndürmesi gereklidir.

fakat stored prosedureler veritabanında değişiklik yapabilirler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Unique Key (yada UK) vs Primary Key (yada PK)

bir sutun tekil değerler içeriyor ise unique'tir. unique sutunlar null değerler alabilir.

veritabanlarında bir sutunun unique olduğu belirtildiğinde; veritabanı sadece aynı değerlerin kaydedilmesini önleyecektir.

PK ise; veritabanının o sutunu diğer tablolar ile ilişkilendirmek için kullanacağını belirtmek için kullanılır. ilişkilendirme olabilmesi için PK, unique'tir.

Bir taloda Unique sutun birden fazla olabilir. Oysa PK, bir tanedir. PK bazı durumlarda birden fazla sutunu birden kapsamaktadır. Fakat o sutunlar tek başına bir adet PK olarak ele alınır. Yani PK yine bir adettir (fakat birden fazla sutundan oluşur). Bu tarz PK'lara composite (bileşik) sıfatı verilmektedir.

Best practice açısından her tabloda bir primary key olmalıdır. ve her tablodaki primary key bir bilgiye tekabul etmemelidir. örneğin tc olmamalıdır. bunun temel 2 sebebi:

- primary key update yapmak zordur (diğer tablolarda aynı anda update gerekir vs)

- privary key'in ömür boyu unique + null olmayacağının garantisini vermek zordur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# sutun indexlemesi
bir sutun indexlendiğinde artık üzerinde daha hızlı querry çalıştırılabilir hale gelir. fakat her update edilişi yavaşlar çünkü indexin de update edilmesi gerekecektir.

```sql
CREATE INDEX my_index_name
ON Persons (LastName)
```

indexi kaldırma işlemi:

```sql
DROP INDEX index_name ON table_name
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SQL sorguları yaptıkları işlere göre gruplandırılırlar:

DDL (Data Definition Language) --> şemalar üzerinde işlem yapan operatörler: alter, create table gibi

DML (Data Manipulation Language) --> satırlar üzerinde işlem yapmaya yarayan operatörler: SELECT, INSERT, UPDATE gibi

DCL (Data Control Language) --> userların yetkilerini ayarlamak için kullanılan sql operatörleri

TCL (Transaction Control Language) --> Transaction işlemlerinde kullanılan sql operatörleridir: COMMIT, ROLLBACK gibi

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# NoSQL (yada Not Only SQL) vs Relational Database Management System (yada RDBMS yada İlişkisel veri tabanı yönetim sistemi)
ilişkisel veritabanı; klasik olarak tablolarda tutulan veritabanlarıdır.

nosql ise her verinin (RDMS'deki satır) kendi başına farklı kolonlar içerebildiği veritabanlarıdır. Bir JSON yada XML içerisinde tutulabilir gibi bilgiler tutulmaktadır. Bir veriye yeni bir sutun yada varolan sutunun tipini farklı bir formatta tutmak istediğimizde, diğer satırları elden geçirmeye gerek yoktur. Bu ne kadar avantaj olarak görünsede, farklı fromatta ve sutun bilgisine sahip verilerin bilgilerinin bir yerlerde tutuluyor olması gerektiğini de unutmamak gerekir.

Nosql veritabanları kendi içlerinde farklı mantıklarda çalışabilmektedirler. kimisinde bir özellik yok iken, diğerinde mevcut olabilir.

mongo db'de veri yapıları ve ona denk gelen RMDB karşılıkları aşağıdaki gibidir:

- collection --> tablo
- document -> satır

ilişkiler mondodb'de tutulmamaktadır. fakat bu ilişkiler yazılımsal olarak sağlanabilir.

mongo db de veriler json formatında tutuluyor. json formatı dışında bir tipte değer tutmak gerekirse onu string'e cast edip tutabiliriz. yada database dışında bir yerde tutup, onun URL'sini json string'i olarak saklayabiliriz.

nosql'de standart sql query'leri yoktur.

db.users.find( { status: "A", age: { $lt: 30 } } ) gibi sorgu ile sadece tek tablo üzerinde sorgu yapılabilir. $lt değeri aldığı integer değerden daha küçük tüm satırları temsil etmek için kullanılır.

## nosql database tipleri

- #### Document Oriented (yada Döküman Tabanlı)
Her nesneyi (id'si olan bir nesneyi) ayrı bir döküman gibi saklar. örnek sistemler: MongoDB

- #### Key-Value (yada Anahtar-Değer)
Bilgiler bir anahtar ve karşılığında bir değer şeklinde saklanıyor. genelde çok fazla işlem ama ufak verileri saklamak için kullanılan sistemlerde kullanılıyor (caching).

Redis veritabanı value'nun tipinide metadata olarak tutmaktadır. bu özellik ver key-value veirtabanında olmak zorunda değildir.

örnek sistemler: Hbase, Cassandra, Riak, Berkeley DB, Redis

- #### Wide-column (yada Column Oriented yada Column family yada Sütun Tabanlı)
Her kayıtta bir sütuna ait tüm veriler saklanır. bu sistem Document Oriented ile benzetilebilir fakat çok farklıdır: Document base sistemde her döküman releational database'deki bir tablomuza denk gelmektedir. Oysa Column based sistemde o tablo ile ilişkili diğer tüm tablolarda tek bir dökümanda tutulur. örneğin bir order'ımız var. bu order'a bağlı bir çok event (örnek transaction) olsun. tüm bu datalar tek bir döküman içerisindedir. Document based sistemde diğer dökümanlara referans olabilir, fakat diğer dökümandaki değerler fiziksel olarak aynı dökümanda tutulmaz.

Örnek sistemler : Hbase, Cassandra

- #### Graph
Kayıtlar, graph(structure çeşidi) ile ilişkilendirilmiş şekilde tutulmaktadır. bu ilişkiler, relational database'lerdeki ilişkilerle aynıdır fakat foreign key tarzı kavramlar yoktur.

sosyal network ağı gibi bilgilerin tutulmasında kullanılırlar. ahmet'in like'ladıkları, ahmetin dislike'ladığı, ahmetin arkadaşları...

örnek sistemler: Neo4J, Giraph

Yukarıdaki listeye bakıldığında aslında herhangi bir db ile yaptığımız bilgiyi diğeri ile de saklayabiliriz. örneğin; Document tabanlı sistemdeki kaydı, Key-Value ile de tutabiliriz. fakat seçimimizi datamızı nasıl çekeceğimize (nerelere sorgu atıp çekmek isteyeceğimize), nasıl kaydedeceğimize göre değişmektedir.

örnek:
ilişkisel veritabanı gibi data kaydedecek isek, fakat ilişkiye ihtiyacımız yok ise, Document tabanlı database bu iş için uygun. çünkü ilerdeki zamanlarda her data sutunu'na göre sorgu atmak isteyeceğiz. oysa bu data'yı Key-Value database'de saklıyor olsaydık, sorgularımızda büyük problem yaşardık. çünkü Key-Value database value'da ne olduğu ile hiç ilgilenmez. Key-Value genelde value'su tek bir değer olan datalar için uygundur. bu şekilde daha hılzı okuma ve yazma yapabiliriz.

Document taanlı sistemlerde her döküman genelde json şeklinde saklanır. çünkü json en hafif veri tiplerindendir ve okunabiliriği yüksektir. fakat her zman json olmak zorunda değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SQL vs OTHERS

#### SQL (Structured Query Language)
tüm ilişkisel veritabanları için ortak dildir. Tüm sistemlerde çalışabileceği için az özellik içerir.

#### PL/SQL (yada Procedural Language/SQL)
Oracle 'ın SQL türevi dili. Oracle SQL tool'u, SQL işlemleri yapan, tekrar kullanılabilir GUI formları hızlıca hazırlamaya yarayan GUI'ler içeriyor.

#### PL/pgSQL
PostgreSQL 'ın SQL türevi dili.

#### TSQL (yada Transact-SQL)
Microsoft SQL Server'ın SQL türevi dili.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ACID
veritabanı transaction(türkçe karşılığı: işlem) özellikleri:

- __Atomicity__: Ya hep ya hiç demektir. Bir transaction içinde bütün işlemler yapılır, ya da biri dahi yapılamıyorsa hiçbiri yapılmaz. rollback özelliğidir. Eğer transaction içinde bir hata olursa sistem en başa dönebilmelidir (yaptıklarını geri alabilmelidir).

- __Consistency (yada tutarlılık)__: Yapılan işlemler sonucunda oluşan çıktıların tutarlılığı anlamına gelir. veritabanındaki ilişkilere ve diğer kurallara uyacak şekilde işlemler tamamlanır. aksi durumda işlem yapılmaz.

- __Isolation (yada izolasyon yada yalıtım)__: Transaction gerçekleşirken dışardan müdahaleye izin verilmemilidir. gerekiyorsa kullanılan tablolar yada satırlar dışarıdan erişim için bekletilmelidir (lock'lanmalıdır).

- __Durability (yada dayanıklılık)__: işlem client taraftan tamamlandı denildiğinde (commit edildiğinde), verirnini sorunsuzca kaydedildiğini garanti etme özelliğidir. herhangi bir elektrik kesintisinde dahi sistemin kayıt işlemini garanti etmesi gerekir. 

# İsolasyon'un olmaması yada bazen olması durumunda çıkabilecek problemler:

- __Lost Updates__: Aynı anda birden fazla transaction aynı anda bir veri üzerinde güncelleme yapmak istediği zaman en son işlem yapan transaction ın kaydı geçerli olur. Bu yüzden veri kaybı yaşanır.

- __Dirty Reads__:Bir transaction ın bir veri üzerinde yapmış fakat commit etmemiş olduğu bilgileri diğer transaction tarafından gerçek kayıtmış gibi okuması durumudur.

- __Non-Repeatable Reads__: Bir transaction bir veriyi okusun ve kendi içinde işleme soksun. Bu transaction commit etmeden başka bir transaction bu veriyi okusun. Daha sonra işlem yapan transaction tekrar veriyi okumak istediğinde veri, aynı veri olmayacağı için sorun yaşanmaktadır.

- __Phantom (hayalet) Reads__: Aynı anda çalışan transaction düşünelim.Bir transaction bir tablodaki verilerin tamamını çeksin ve kendi içindeki işleme soksun.Bu sırada da diğer transaction aynı tabloya veri ekleme işlemi yapsın.Daha sonra ilk transaction tekrar verileri çekmek istediğinde yeni eklenen veriler hayalet veri olarak görünür.

# Isolation levels:
ANSI ve ISO SQL standratlarının belirlediği 4 çeşit izolasyon tipi mevcuttur. Bunlara ek olarak her database farklı seviyeler sunabilir.

- # Read Uncommited (level 0)

  bu level'da bir transaction işlem yaparken yaptığı her değişikilik diğer transaction'lar tarafından görülebilir.

  - engelleyemeği sorunlar: Dirty + Non-Repeatable + Phantom

- # Read Commited (level 1)

  bu level'da bir transaction işlem yaparken kullandığı satırları tamamen lock'lar.

  - engellediği sorunlar: Dirty

  - engelleyemeği sorunlar: Non-Repeatable + Phantom

- # Repeatable Read (level 2)

  bu level'da bir transaction işlem yaparken kullandığı bölgeleri lock'lar. yeni kayıt eklenmesini dahi durdurur. bölge karamı transaction'ın içerisindeki "where" ile belirleyeceği alandır.

  - engellediği sorunlar: Dirty + Non-Repeatable

  - engelleyemeği sorunlar: Phantom

- # Serializable (level 3)

  kullanılan ilgili tüm tabloları tamamen lock'lar.

  - engellediği sorunlar: tümü

  - engelleyemeği sorunlar: yok

# JPA locking mechanism

 - # kullanımı 
JPA her transaction için hangi lock modu'unu kullanabileceğimizi belirlememize izin veryor. kullanım örnekleri:

```java
// entityManager Find
entityManager.find(Student.class, studentId, LockModeType.OPTIMISTIC);

// Query
Query query = entityManager.createQuery("from Student where id = :id");
query.setParameter("id", studentId);
query.setLockMode(LockModeType.OPTIMISTIC_INCREMENT);
query.getResultList();

// explicit Locking
Student student = entityManager.find(Student.class, id);
entityManager.lock(student, LockModeType.OPTIMISTIC);

// refresh (it updates the lock mode)
Student student = entityManager.find(Student.class, id);
entityManager.refresh(student, LockModeType.READ);

// NamedQuery
@NamedQuery(name="optimisticLock",
  query="SELECT s FROM Student s WHERE s.id LIKE :id",
  lockMode = WRITE)
```

- # JPA Optimistic locking
Entity'mizde (tablomuzda) @Version tagini almış bir sutun olmalıdır. JPA bizim için bu sutunu otomatik yönetir. Her sutun değiştiğinde bu sürüm arttırılır. DB'deki ilgili satırımızın sürümü 5 olsun. Daha sonra bir transaction gelip bu satırı update etmeye kalktığında, eğer transaction verisi sürüm 4 ise OptmisticLockException alacaktır. Ancak 5'incisi sürüme sahip bir transaction o satırı update edebilir.

Optimistic locking tamamen yazılımsal katmandaki logic ile yapılan bir özelliktir. DB'nin desteklemesi gerekmez. dolayısı ile aşağıdaki 2 lock modu'unda da veritabanı seviyesinde lock mekanizması kullanılmaz.

- Optimistic locking Lock Modes

  JPA Optimistic locking için 2 tür mode sunar:

  - __OPTIMISTIC__ veya __READ__ enum'ları aynı yere referans eder

    veri update edildiğinde versiyon numarası yükseltilir.

  - __OPTIMISTIC_FORCE_INCREMENT__ veya __WRITE__ enum'ları aynı yere referans eder

    OPTIMISTIC ile aynı mantıkta çalışıyor, fakat ek olarak; veri okunduğunda dahi versiyon numarasını arttıyor. Bu mode; bir datayı okuduğumuzda onu serbest bırakana kadar bakaları tarafından güncellenememesini sağlar. çok önemli datalar gösterildiğinde bu kullanılabilir.

- # JPA pessimistic locking
pessimistic kelime anlamı: kötümser

database'in lock mekanizmasını desteklemesi gerekir. JPA bir satır okunduğunda, o satırı database tarafında lock konumuna geçirir. locklanan satır diğer uygulamalar tarafından okunamaz. 

çeşitleri:

- __PESSIMISTIC_READ__

  locklanan entity diğer transaction'lar tarafından okunabilir fakat değiştirilemez. 
  
- __PESSIMISTIC_WRITE__

  locklanan entity diğer transaction'lar tarafından okunamaz.

- __PESSIMISTIC_FORCE_INCREMENT__


############################

############################
# SECURITY
############################

############################

# SQL injection (yada SQLi)

- SQL injection'dan kaçmak için ORM framework sürümümüzün güncel olmasına dikkat etmeliyiz. çünkü orm'nin aşağıda örneklendirilen bind işleminde kontroller gerçekleştiriyor.

- Dışardan alınan parametreleri bind ederek tanımlamalıyız:

  ```java
  Query query = createQuery("from LoginUsers where userName=:userName");

  query.setParameter("username", userName); //ORM burada userName'in tipini biliyor ve kontrolleri kendisi yapıyor. 
  ```

  Bind etmeyi sadece SQL komutları için değil, JPQL için dahi yapmalıyız. SQL ile JPQL farksızdır. JPQL; ancak bind edilirse güvenliği sağlar.

- Dinamik querry yapılacak ise Criteria API'den faydalanılmalı

- # tipleri

  - # In-band SQLi (yada Classic SQLi)
      
      - # Error-based SQLi
        Exception detayları uygulamadan denüyor ise, bunlardan yola çıkarak sunucudan bilgi toplanır.

      - # Union-based SQLi
        "select * from X where ' $VAR '" gibi sql'lerde VAR inputunu değiştirerek sunucudan bilgi toplanırız. VAR yerine bir şey eklememiz ancak ve ancak UNION sorguları ile mümkündür. bu sebeple bu ataklara Union-based denir.

  - # Inferential SQLi (yada Blind SQLi)

      - # Boolean-based (yada content-based) Blind SQLi
          Suncuda bu şekilde bir http controler olsun:

          ```java

          @HTTTP_GET
          isExist(String input){

              boolean exist = doSQL("SOME SQL" + input);

              if(exist){
                return "true";
              } else {
                return "false";
              }
          }
          ```

          Bu kontrollerdan bilgi almak için şöyle bir yöntem geliştirilebiliriz:

          input'taki sql'imiz önce tüm tabloları çeken sql'i çalıştıracak. sonra bu tablo listesindeki ilk elemanın (ilk tablo adının) ilk karakeri A mı diye bakılacak. eğer A ise "boolean exist = true" olacak şekilde dönüş yapılmalı. eğer true olursa demekki ilk karakter A. 

          Aynı işlemi ilk tablonun ikinci elemanı içinde yapmalıyız ve tüm alfabe ve özel karakterler için yapmalıyız. Her tablo adı 100 karakter olsa, 100 tablo olsa, 50 karakter kombinasyonu olsa, 100\*100*50 adet HTTP isteğinde tüm tablo listesini almış oluruz.

          Bu işlemi daha hızlandırmak için fakrlı yöntemler mevcut. Örneğin; Tablo isminin ilk karakterinin ASCII karşılığı alınır. Örneğin bu değer 50 olsun. Biz sql sorgumuzda bu değer 50'den küçükse "boolean exist = true" dönecek şekilde ayarlarız. Eğer true dönerse; ASCII 50'nin üzerindeki hiçbir karakter için HTTP denemesi yapmamıza gerek kalmayacak. Bu tarz trick'lerle deneme sayımızı çok azaltabiliriz. 

      - # Time-based Blind SQLi

          controller'ımız boolean dahi dönmüyor olsun:

          ```java
          @HTTTP_GET
          isExist(String input){

              boolean exist = doSQL("SOME SQL" + input);

              if(exist){
                return "true";
              } else {
                return "false";
              }
          }
          ```

          Buradan bilgi alabilmek için Boolean-based ile aynı SQL'leri çalıştıracağız. Fakat dönüşümüzde true false alamadığımızdan, dönüş değerimiz yerine zaman aralığından faydalanacağız. SQL'lerimiz true durumunda sleep(7) gibi bir SQL fonksiyonu çalıştıracak. eğer 7 saniye içinde cevap gelmezse o SQL sonucunu true olarak varsayacağız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Insecure Direct Object References (yada IDOR)
Objelere erişmke için kullandığımız ID'ler, eğer uygulamamızın arkaplanında kullandığımız referanslar ise, o zaman bu referansları kullanarak kötü niyetli kullamnıcılar güvenlik açıklarından yararlanabilir. bu tarz ID'lere (referanslara) IDOR denir.

örnek:
Account objemizin ID'si 1001 olsun. eğer bu ID diğer tüm metodlarda (isil, update) kullanılan sabit bir ID ise, bu ID, IDOR'dur. çünkü aynı ID sil metoduna yollandığında o hesap silinecektir.

bu durumu önlemek için;
- önyüze dönülen her referansa sadece o işlem için geçici farklı bir referans atanır
- her işlemde kullanıcının bu işlemi yapmaya yetkisi var mı diye detaylı kontrol edilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# PIN (yada Personal Identification Number)

geçici olarak kullanılan şifredir.



# Multi-factor authentication (yada MFA)
factor türkçe kelime anlamı: faktör, etken

işlem yapabilmek için birden fazla onay adımının olması ve her onay adımında farklı kanallardan güvenlik doğrulamalarının yapılmasıdır. örneğin; ilk adımda şifre giriliyorsa, ikinci adımda sms'in girilmesi bambaşka bağımsız bir kanal üzerinden sağlandığı için MFA olur. fakat şifre + CAPTCHA girişi yapmak MFA'yı karşılamaz.



# Two-factor authentication (yada 2FA)
MFA'nın 2 adımlı olan türevidir.



# Two-step verification (yada two-step authentication)
MFA diildir. MFA örneğinde verildiği gibi farklı kanallardan sağlanmayan güvenlik adımlarını temsil eden gruptur.



# TOTP (yada Time-Based One-Time Password)
Bir OTP çeşididir. Offline olarak OTP üretir. Bu OTP sadece belirli bir süre geçerlidir. Bu sebeple Time based'dir.



# Google authenticator
TOTP kullanımını sağlayan bir GUI uygulamasıdır. Kullanıcı parametre olarak OTP'nin yenileneceği döngü süresi (varsayılan 30 saniye) ve secret-key'i google'den alır. Bu parametreler ile 30 saniyede bir yeni bir OTP üretir. Bu OTP'nin algoritması bellidir. Bu sebeple offline olarak OTP üretilebilir.

Google authanticator açık kaynaklı bir algoritmaya dayanır. bu sebeple alternatifi birçok client ve programlama dili API'si mevcuttur. 



# otp hakkında
otp süresi algoritmaa göre değil, kullandılığı hizmetin kendisine göre değşir. yani; google'ın kullandığı algoitma 30 saniyeliktir. fakat aynı algoritmayı 40 saniye olacak şekilde de ayarlayabiliriz.

otp şifresi T zamanında üretilmiş olsun. artık bu T zamanında üretilen otp google için 30 saniye süresinde kabul görecektir. T+1 zamanında aynı secret key ile bir OTP üretelim. Bu password'de artık 30 saniye geçerli olacaktır. dolayısı ile T+1 de oluşturduğumuz key daha farklıdır. Fakat T+2 anında her 2 key'imizde geçerlidir.

Örneğin; https://github.com/bilelmoussaoui/Authenticator uygulaması ile masaüstünde aynı secret key ile iki şifre oluşturduğumuzda, iki şifre farklı zamanlarda oluşturulacağı için farklı key göstreceklerdir. her 30 saniyede bir yeni key oluşturacaklar ve her ikisinin döngüsünün sona erdiği an farklı olduğundna hep farklı key üretecekler.

Bazı servisler her 2 anahtarı da kabul etmektedir. Fakat google bunu kabul etmiyor. "Google authenticator" uygulaması bu durumu şu şekilde çözüyor. "Google authenticator" kendi içinde belli zaman aralıkları belirlemiş. Her 30 saniye bir zamn aralığı içinde, yani 00:00:00 - 00:00:30 ilk zaman aralığı.  00:00:30 - 00:00:60 ikinci zaman aralığı. Eğer 00:00:35 anında bir key üretirsek; bizi ikinci zaman aralığına otomatik alıyor ve 00:00:30 anında şifreyi üretmişiz gibi varsayıyor ve buna göre hesaplamaları yapıyor. Dolayısı ile her 30 saniye için en fazla 1 farklı key üretebiliyoruz. Bu tamamen "Google authenticator"'ın önyüzde uyguladığı bir mantıktır. anahtar üretim algoritması ile bağlantılı bir durum diildir.

Aynı durum https://github.com/bilelmoussaoui/Authenticator için geçerli değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Post-quantum cryptography (yada quantum-proof yada quantum-safe yada quantum-resistant)
Kuantum hesaplamaları fizik kuralları gereği her türlü hesaplamayı verimli şekilde yapamamaktadır. Bu sepele kuantum teknolojisinin hızlı hesaplamaya tabi tutamayacağı algortimalar ortaya atılmıştır. Bu önlemleri üretebilmek için matematik ve fizik bilgisi de gerekmektedir.

One-time-password, n deneme sonrası hesabın devre dışı bırakılması gibi önlemlerde de terminolojik olarak bakıldığında "quantum resistant security"'yi sağlamaktadır.

2019 yılı itibari ile çoğu asimetrik şifreleme (private/public key'e dayanan şifreleme) algoritmaları kuantum bilgisyaralarına karşı dirençsiz. Simetrik algoritmalar ise dirençli. Fakat simetrik algoritmalar'ın keyleri yarıya düşürülmüş kadar daha hızlı çözülebiliyor. örneğin 512'lik bir anahtara sahip bir şifreleme algoritması, eğer kuantum bilgisayarı ile kırılırsa, 256'lık hızında kırılabiliyor.

2019 yılı itibari ile özgür lisanslı ve ticari anlamda tümüyle kullanılabilecek quantum resisdent algoritması yoktur. Olanlar ya lisanslı, yada key'leri büyük olması gibi önemli dezavantajları mevcut. 

# quantum algorithms
kuantum bilgisayarların verimli çözdüğü algoritmalardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HTTP Public Key Pinning (yada HPKP)
Ssl işlemlerine ek güvenlik yapmak için kullanılan bir yöntemdir. Sunucu cevaplarının header'ına şunu ekler:

```json
Public-Key-Pins: 
         pin-sha256="cUPcTAZWKaASuYWhhneDttWpY3oBAkE3h2+soZS7sWs=";

         pin-sha256="M8HztCzM3elUxkcjR2S5P4hhyBNf6lHkmjAHKhpGPWE=";

         max-age=5184000
```

Tarayıcı bu değerleri tüm yaşamı boyunca (en fazla max-age e göre) saklar. Pin-sha değeri sunucunun public key sha'dır. Eğer ilerde bir hacker sunucunun döndürdüğü public key'i değiştirirse; tarayıcı eski sha'ları da kontrol edeceği için bir sorun olduğunu anlayacak ve kullanıcıyı uyaracaktır.

2 tane pin olmasının sebebi max-age süresince sunucunun gerçektende public key'ini değiştirmesi durumunda kullanılacak backup'tır. Sunucu dilediği kadar pin-sha yollayabilir.

Bir siteye bir tarayıcıdan ilk defa gidiliyorsa; bu güvenlik önlemi bir işe yaramaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# browser fingerprint
internet siteleri kullanıcılardan kısıtlı bilgi alabilmektedir. fakat bu bilgilerin kombinasyonları ile yine makinanın uniqu'liğini tespit edebilmektedirler.

- cookie'nin açıp olup olmadığı bilgisi
- hangi fontların yüklü olduğu listesi
- user agent
- cpu tipi
- timezone
- index db olup olmadığı
- do not track yollanıp yollanmadığı
- screen resolution
- dil

gibi daha bir çok parametre ile tarayıcının uniqu'liği ortaya koyulabilmektedir. hatta bazı özellikler tek başlarına %100 unique'liği sağlayabilmekte, bazı özellikler yine tek başlarına %99 oranında unique'liği sağlayabilmektedirler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Biyometri
yaşayan organizmaların ölçümlerine verilen genel isimdir. tüm niteliklerin sayısal ortama aktarılmış halidir.

# biyometrik doğrulama yöntemler
parmak izi(fingerprint), ayak izi, avuçiçi izi, damar izi, göz/retina izleri ve daha milaylarcası.

# parmak izi
parmak izi en sık kullanılan, en basit ymntemlerle tespit edilen ve karşılaştırılabilen metodlardandır. bu sebeple sık kullanılır. parmak izinin %100 benzersiz olduğuna dair bir tez yoktur. fakat şu ana dek aynı parmak izi olduğu görülmemiştir. bu sebeple herkes eşsiz olarak sayar. bazı kurumlar, bu ebeplten ötürü sadece 1 parmağın değil, garantilemek amacı ile birden fazla parmak izini birden saklamaktadır. parmak izi benzerliğini kontrol etmek için kullanılan birçok algoritma mevcut. aynı zamanda alınan parmak izinin ne kadar detaylı alındığı da önemlidir.

# biyometrik doğrulama için kulanılan cihazlar
siemens bir cihaz üretsin. bu cihazı XX firması hastanelerde kullansın.

- bazı cihazlarda siemens, hangi bilgiyi (örneğin parmaklardan; parmaklarda milyar çeşit bilgi mevcut) son kullanıcıdan (buradaki senaryoda hastalardan) aldığını paylaşmaz. bu şekilde XX firmasının veritabanı hacklenirse; insanların hangi bilgilerinin çalındığı bilinmemiş olacaktır. ancak siemens'in içinden de, cihazla ilgili gizli bilgilerede erişmeleri gerekecektir.

- aynı zamanda son kullanıcıdan (buradaki senaryoda hastalardan) çekilen bilgiyi şifreler ve bu datayı cihazın output'undan şifrelemiş şekilde paylaşır. şifrenin private key'i de XX firması ile paylaşılmaz. bu şekilde XX'in veritabanı hacklenirse; hacker'lar ancak siemens'in içinden de, cihazla ilgili gizli bilgilerede erişmeleri gerekecektir. siemens'in ürettiği key hem her cihaz(model) için farklı iken, birde ikinci bir şifreleme satılan her firma için ayrı olabilmektedir. ve her firma için üretilen key'ler rastgele (manuel) üretilir. manuel için bir örnek; denizin dalga sesi çekilir ve bu ses ile key üretilir.

XX firması zaten kişinin ilgisi önemli değildir. XX firması için önemli olan, kişinin o kişi olup olmadığını anlamak. yani; eşleştirme yapmak. datanın içeriği önemli değildir. şifreli halini karşılaştırması XX firması için önemli değildir. bir diğer değişle; parmak izi cihazı gömülü olarak kendi içinde şifreleyerek dışarı bilgi veriyor, fakat XX firması karşılaştırma yapmak istediğinde cihaz kendi içinde private key'i ile bu bilgiyi açıyor ve karşılaştırma yapıyor. dışarıya true yada false dönüyor. cihaz kendisine gelen bilgi %100 eskisi ile aynı ise false dönüyor (eşit diil). çünkü parmak damar izi bilgisi; o anda odanın sıcaklığı, kişinin o gün yediği yemeklerden dahi çok az da olsa etkileniyor. %100 eş bilgi ise; bu kişi databaseden veriyi kopyalamış anlamına geliyor. bu sebeple örneğin ancak %93-%99.999 eşleşen bilgiler onaylanıyor.

hacker %100 benzetilmiş data yerine %99 atmaya çalışıabilir. dolayısı ile %98 yapmak daha mantıklı diil mi? yani; en üst aralık %98 olması daha mantıklı diil mi?

çünkü cihaza input veren hacker ya %100 benzer veri verebilir, yada hiç alakasız bir veri verebilir (%80'in altında). çünkü cihaza giren input şifrelenmiş olmalı. bu şifrelenecek private key'i kimse bilmiyor. dolayısı ile şifrelenmiş data üzerinde deişiklik yapılınca, zaten şifre private key ile açıldığında saçmalanmış bir veri otaya çıkacaktır. yani hacker'ın %99 benzer bir veri yapması imkansız. bu sebeple %99 benzer veriler onaylanıyor.


• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# hijack attack
hijack türkçede hırsızlık anlamına gelmektedir. güvenlik saldırısı hijack genel anlamı ile bilginin çalınması anlamına gelir. kelime anlamı gereği çok genel bir tabirdir. fakat hijak saldırıları genelde; iletişimde olan iki makine arasında olan hacker'ın, iletişim sırasında paketleri kendi üzerinden geçirmesi ile olan saldırılar için kullanılır. bu tarz saldırılara aynı zamanda "man-in-the-middle attack" denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# pentest (yada penetration)
penetration türkçe anlamı nufuz etmek, delmek, içine girmektir.

sisteme sızma testleridir. bu testler ile güvenlik açıkları tespit edilir.


# Whitebox vs blackbox vs greybox
pentest tipleridir. pentesti yapacak ekibe sistem hakkında hiçbir bilgi verilmiyor ise blackbox testidir. kodlar, netowok bilgileri gibi tüm sistem bilgileri veriliyor ise Whitebox testidir. kısmen bazı bilgileri veriliyor ise greybox tipinden test yapılıyor demektir.

# blackbox
kelime anlamı: mat (parlak olmayan, ışık geçirmeyen).

blackbox bilişim dünyasında sadece input ve output'ları gözlenebilen fakat içi görünmeyen herşeye denir.

# Vulnerability Assessment (yada zaafiyet tarama)
bu çalışmalar, çoğu zaman pentest'lere benzediklerinden dolayı pentest ile karıştırılmaktadırlar. zaafiyet taramaları; sistemde direk güvenlik açığı oluşturmasada, güvenliği arttırıcı etkenleri tespit etme çalışmasıdır. örneğin; sunucuda kullanılan java sürümü eski olduğunu varsayalım. bu durum pentestler'den dönmesede, zaafiyet taramasından dönmelidir. eğer java eski olduğundan; kullanıcıların bilgilerine erişiliyorsa o zaman pentest sonuçlarında raporlanmalıdır.

# Alpha Testing Vs Beta Testing
junit entegration ve sistem testleri koşulduktan sonra yapılan testlerdir. buradan sonra yapılan testler "user acceptence test" olduğu için alpha ve beta da user acceptence testi olarak geçer.

alpha testleri genelde bu işin uzmanı kişilerin yaptığı testlerdir. beta testleri ise public fakat sadece belli kişilerin test etmesi sürecidir.

# Functional Testing vs Non-Functional Testing
fonksiyonel test sadece belirli bir özelliğe istinaden koşulan testlerdir. oysa Non-Functional performans gibi kriterleri ölçmek için yapılan testlerdir.

# regresyon testleri
yazılıma eklenen yeni bir özelliğin, sistemde zaten varolan diğer özellikleri etkileyip etkilemediğinin testleridir. bu durumda aslında tüm sistem baştan sona test edilmesi gerekir fakat bu yükske maliyet olacaktır. bu sebeple; sadece etkisi olabilecek potansiyel modüller test edilmelidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# BackTrack
pentest yapabilmek için gerekli tool'ların bir arada sunan, linux tabanlı, canlı cd ile çalışan işletim sistemidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# IPSec
Internet Protokol katmaninda gizlilik ve dogruluk saglamaya yönelik güvenlik standartlari grubudur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Single sign-on (yada SSO)
Artık kullanıcılar birçok farklı sisteme ayrı ayrı giriş yapmakta ve ayrı ayrı şifre tutmaktadırlar. BU şifreleri tek bir merkezi yerden yönetilebiliyor olması durumunda;

avantaj:
- son kullanıcı için daha rahat, tek şifre çünkü

dezavantaj:

- çalınması durumunda bir tehlike oluşturmaktadır çünkü tüm sistemler için şifre aynı. buradaki çalınma şifrenin diğer sistemlerin herhangi biri tarafından çalınmasıdır.ss

dezavantajı gidermek için SSO çözümü uygulnabilir. Yine şifre yönetimi tek bir yerden olacaktır. Fakat kullanıcıya token gönderilecektir. Şifre giriş ekranı domain adresi; ortak şifre yönetim sisteminin olmalıdır. SOn kullanıcı bu şekilde şifresini diğer sistemlere girmemiş oluyor. Dİğer sistemler hack'lendiği zaman, kullanıcının sadece token'ını kullabilecektir.

# authentication (yada kimlik doğrulama)
sadece kullanıcıyı doğrulama mekanizmasıdır.

# authorization (yada yetkilendirme)
Sadece ilgili kullanıcının nelere yetkili olup olmadığı yönetimidir. Yetki kontrolü yapan sistemde, kimlik doğrulama olmak zorunda değildir. Bunun en bilinen örneği Oauth'dır.

# OpenId 1.0 ve 2.0

- Kimlik doğrulama protokolüdür.

- açık kaynaklıdır.

- örneğin twitter gibi bir siteyi provider(kimlik doğrulayıcı) olarak kullanıp, başka yere login olmak isterseniz, login olacağınız sitenin, twitter'dan anahtar(onay) almış olması şart değildir.

- openid protokolünü destekleyen herhangi bir provider ile, openid login'i destekleyen herhangi bir siteye login olunabilir.

- herkes birer openid provider'ı olabilir. 

- openid provider ile login olunmaya çalışılan site arasındaki haberleşme protokolüdür.

süreç:

- kullanıcı openID-provider adresini login olanacağı web siteye girer (muhtemelen "login with Twitter" gibi bir butona tıklar)

- web sitesi tiwtter'a yönlendirme yapar

- kullanıcı twitter'a login olur (naısl login olacağına openID hiç karışmaz)

- kullanıcı başarılı yada başarısız olursa login olmaya çalıştığı web sitesine yönlendiririlir.

- yönlendirilirken twitter web sitesine kullanıcı için kontakt bilgileri gönderir. burada bir çok farklı birlgi olabilir. muhtemelen twitter'a login olunduktan sonra nelerin atılaağı son kullanıcıya sorulur.

# Oauth 1.0 ve 2.0

- Yetkilendirme protokolüdür. HTTP üzerinden çalışır.

- açık kaynaklıdır.

- örneğin twitter gibi bir siteyi provider olarak kullanıp, başka yere login olmak isterseniz, login olacağınız sitenin, tiwtter'dan anahtar(onay) almış olması şarttır.

- 2.0, geriye uyumlu değildir.

- openid'dekine benzer bir süreç vardır. fakat web sitesi twitter'dan sadece login bilgilerini almaz. aldığı token ile birlikte sürekli olarak arkaplanda twitter'ın kaynaklarına user adıne erişip işlem yapabilir. bu sebeple oauth bir yetkilendirme protokolüdür. 

# oauth 2.0

- ## roller

  - ### Resource owner
    kullanıcının kendisi

  - ### Resource server
    API. user'ın kişisel bilgilerini barındırır. dışarıdan gelen isteklere cevap verip vermeyeceğini bilmelidir. işte bunu anlaması için oauth 2.0 protokolünden yararlanacaktır. bu sunucuya client istek yapacaktır.

  - ### Authorization server
    bu kısım istenirse API ile aynı uygulama içerisinde de olabilir.

  - ### Client
    third-party app

yukarıdaki rollere bazı örnekler:

- banka uygulamamız var. sunucu tarafta birçok microservisimiz var. bir mikro servisimiz sadece auth server görevi görsün. diğer tüm micro servisler API olacaktır. mobil veya ATM ise birer client rolünde olacaktır.

- amazon.com'a login olucaz. amazona hızlı login olabilmek için facebook profilimizle login olduk. facebook hangi bilgileri vereceğini bize sorar. biz onayı verdiğimizde amazon bu bilgilere erişir. amazon burada client, facebook ise API ve auth server birliktedir.

- ## süreç

client taraf prod ortamına çıkmadan önce auth-server'dan (provider'dan) bir client_id ve client_secret alması şarttır.

aynı zamanda client taraf provider'a redirectURL'ler de vermesi gerekir. bu url'ler success ve error durumunda kullanıcının client tarafa yönlendirilmesi için şarttır. client'ın test ve prod ortamı için provider'dan iki anahtar almak uygun olacaktır.

- ## state

user oauth2 sunucusuna yönlendirildiğinde state parametresi isteğe bağlı atılır. bu parametreyi alan provider bu degeri redirect url ile tekrar client'e geri yollar.

- ## Authorization Code

provier'dan onay alındığında kullanıcı client'a redirect ediliyor. client tarafa "Authorization Code" veriliyor. bu kod ile client taraf artık API'ye istek yapamaz. redirect sırasında araya biri girmiş olabilir. bu sebeple Authorization Code'u client kendi sunucularına atıp, "Authorization Code" + "client_secret" ile auth severdan access_token'ını rica eder. secret ön tarafta uygula a tarafında tutulmadığı için "Authorization Code" çalınırsa güvenlik açığı yaratmaz.

- ## login olma aşaması

web browser'dan user'ı provider'ın sitesine yönlendirirken şu parametrelerle yönlendirmek gerekli:

- client_id

- reponse_type: "code". Authorization Code istediğinizi belirten bir kod.

- redirect_url: opsiyonel. provider'a kaydlurkende bu url veriliyor. isterse bu adımda o değer override edilebilir. redirect url http:// diilde app:// gibi de olabilir. bu şekilde native uygulamadan web view açarak login ettiğimiz kullanıcı tekrar uygulamamıza yönlendirilebilir.

- scope: provider'ın belşirlediği stringler. bu stringler hangi yetkileri istediğinizi belirtiyor. arada oşluk olucak şekilde formatlı olmalılar.

- state: (yukarıda açıklandı.) buraya verilen strignler aynen geri client'a yollanıyor.

yukarıdaki istek provider'ın Authorization url'sine yapılmalıdır.

- ## auth-code'u access token'a çevirme isteği

login olma aşaması bittiğinde client tarafa auth-kod gönderildi. bu kodu acces token'a çevirmek için provider'ın token url'sine post isteği aşağıdaki parametrelerle yapılmalıdır:

- grant_type: "authorization_code"

- code: bize bu oturumda gelen auth-code

- redirect_uri: success olma durumunda buraya bir istek yapılacak

- client_secret

- client_id

bu istek yapılırken provider gelen isteğinde yetkisi olup olmadığını login olabilmesinden anlamar ister. örneğin bu post işlemi için basic-authantication gerekebilir. bunu provider'ın kendisi istediği gibi belirleyebilir.

client_secret gizli olmalıdır. fakat server sideı olmayan uygulamalar (örnek: bazı native app'ler veya javascript ile çalışan bazı tarayıcı eklentileri gibi) client_secret'ı user'ın local makinesinde tutacağından bir güvenlik açığı meydana getirmektedirler. proiver'lar sunucusu olmayan uygulamaları anlayamaz. buna yapacak bir şey yok.

- ## api calls

access token'ı alan app artık api'ye istek yapabilir. oauth2 bu istek hakkında standart içermiyor. fakat genelde header'daki Authorization değerine "Barear 345345234234234" şeklinde yollanıyor. resource server gelen istekteki access_token'ı alıyor ve ath servera gönderiyor. acees_token'ın geçerli olup olmadığının cevabını auth server'dan alıyor. her istekte bu işlem tekrarlanıyor. eğer gelen istekteki access_token bir jwt ise; o zaman auth server'a gitmeye gerek kalmaz. resource server'ın kendisi token'ı private key ile açıp kontrol edebilir.

# openid connect (yada OIDC)
openid 2.0'dan sonra çıkan sürümü. isim değişikliği sonrası 1.0 sürümünden devam edilmektedir.  Openid connect, oauth 2.0 üzerine kurulu bir yapıdır. Böylece oauth'da eksik olan kimlik doğrulama özeliği getirilmiştir.

# Facebook Connect
Facebook'un diğer sitelere login olabilmemizi sağlayan kendi protokolü.

# token
SSO'larda token sadece yetkilendirme için kullanılır. sadece kimlik doğrulama için token kullanımına gerek yoktur. yetkisine göre cevap dönülmesi gereken yerlerde token kullanılmaktadır.

rest sessionlarda kullanılan token'lar farklı amaçtadır. onlar her defasında sunucuya gönderilir, bu şekilde kullanıcı login mi değilmi diye anlaşılır.

# JSON Web Token (yada JWT)
Session yönetimlerinde giden token'ın (kullanıcı auth konusuyla ilgili giden gelen tüm bilgilerin) formatı ile ilgili özel bir standarttır. içinde ne gideceği ve nasıl doğrulanacağı hakkında standart içermez.

JWT üç temel kısımdan oluşur:

- ## jwt header

```json
{

    "alg": "HS256", //Veri bütünlüğünü korumak için kullanılacak cryptotic algoritmayı belirtir.

    "typ": "JWT" //sabit

}
```

- ## jwt payload

```json
{

    "userId": "3344552266",

    "expire": 1486220816,

    "roles": ["admin", "user"]

}
```

payload içerisinde ne olacağına JWT karışmaz. fakat standart olması amaçlı bazı önceden belirlenmiş propertie'ler vardır.


- ## jwt imza

bu kısım bu algoritma ile oluşturulur:

>PART1 = toBASE64(HEADER)

>PART2 = toBASE64(PAYLOAD)

>PART1_PART2 = PART1 + "." + PART2

> SECRET_KEY= "MY_KEY"

> PART3= toBASE64( toHMACSHA256(PART1_PART2, SECRET_KEY) )

Artık JWT Token'ımızın tümü şudur:

> JWT_TOKEN = PART1_PART2 + "." + PART3

Dikkat edilirse; her kısım arasında . (nokta) vardır. Giden token base64 olduğundan yazılımcı bu token'ı URL'den de sunucuya atabilir. Dikkat edilirse veri bütünlüğü sağlandığı için sunucuda session ihtiyacı kalkar. Tüm bilgiler token içerisinde saklanabilir.

Standartlarda olmasada genelde JWT http requestinin header kısmına şu şekilde skalanır: 

> Authorization: Bearer JWT_TOKEN_BURADA

# jwt token types

- access token: isteği yaptığımızda geçerli olan jwt token'ı.

- refresh token: hacker acces token'ı çalarsa access token'ın son kullanma tarihine kadar işlem yapabilir. bunu kimse engelleyemez. bu sebeple access token'ın süresi çok kısa tutulur. client, sıklıkla yada access token expire olduğunda en sonki token'ı ve refresh token'ı sunucuya gönderir. reponse olarak sunucudan daha güncel bir access token cevabı alır. bu işlem %100 bir güvenlik sağlamasada access token çalınma durumunda + refresh token'ın çalınmaması durumunda güvenliği sağlamaktadır.

  refresh tokenlar sunucu tarafta tutulmaktadır. bu durum token-based'in mantığını bozmamaktadır. çünkü refresh token'lar bir databasede tutulabilirler. yani hızlı erişim belleğinde tutulma ihtiyacı yoktur. çünkü az sıklıkla erişilmektedirler. database'de olan bu tarz bilgiler 'state' statüsünde değillerdir. state olsaydı o zaman user tablosundaki surname de olurdu.

  eğer refresh token çalınırsa, son kullanıcı refresh token'ı bloke etmesi için sunucuya bildirimde bulunabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# rootkit
sistemin admin kullanıcı haklarını ele geçirip işlem yapmaya çalışan malware'dir. genelde algılanmaları zordur, çünkü zaten en yüksek seviyede yetkiye sahiptirler ve bu sebeple kendilerini gizlemeleri kolaydır.

# IP spoofing (yada IP sahteciliği)
IP adresini farklı gösterip request gönderme işlemdir. Tabi cevap bizim isteği yolladığımız makinaya eğil, IP sahibi makinaya gidecektir. Fakat IP sahteciliğinde dönüş cevabı önemsenmez. Yapılmasının amaçları farklıdır. Örneğin; sunucunun bant kaynaklarını tüketmek.

# DoS saldırısı (yada Denial of Service attack)
sisteme sızmaya çalışmadan, sistemin kaynaklarını (örneğin bant genişliği) tüketip kötü amaçlara kullanan saldırı tipleridir.

# DDoS saldırısı (yada Distributed DoS attack)
birçok makineden paralel DoS atağı yapma saldırısıdır.

# Malware (yada malicious software)
category of all dangerous softwares.

# Spyware
kullanıcıdan habersiz arkaplanda bilgi toplamak amaçlı yazılmış zararlı uygulamadır. örnek: keylogger.

# Phishing
an attempt (the operation) to get your personal information

# Adware
reklam gösteren uygulamalardır.

# Ransomware
Ransom türkçe kelime anlamı: fidye

fidye isteğen yazılımlar. kaşrılığında bilgisayarı tekrar virüsten arındıracağını idda eder.

# Scareware
farklı bir yazılım yükleme vaadiyle (gerçekten o yazılım yüklenir) kurulup, arkaplanda kötü amaçlı farklı yazılımlar çalıştıran uygulamalardır.

# Botnet (yada zombi ağı)
her bilgisayara botnet dediğimiz bu zararlı yazılım yüklenir. bu zararlı yazılım, kurulduğu makineye uzaktan erişim için güvenlik açığı sağlar. bu şekilde tek bir noktadan tüm uzak bilgisayarlar yönetilebilir olur. bu kurulan network ağına "zombi ağı" denir. her bilgisayara ise "zombi (yada zombie)" denir.

# backdoor
bilgisayarın uzaktan yönetilebilmesi için açılan güvenlik açığına verilen isimdir.

# trojen (yada Truva atı)
Scareware ile farkı genel olarak yoktur.

# virüs
otomatik yada kullanıcı tıklamalarıyla farklı makinelere yayılma özelliği olan malware'lerdir.

# Worm (yada solucan)
başka makinelere otomatik yayılır.

# Brute-force attack (yada kaba kuvvet atak)
Sadece deneme yanılma ile sürekli olarak farklı girdilerle doğru çıktıları elde etmeye çalışma işlemidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Social enginnering
Sosyal mühendislik, internette insanların zaafiyetlerinden faydalanarak çeşitli ikna ve kandırma yöntemleriyle istenilen bilgileri elde etmeye çalışmaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# TLS (yada Transport Layer Security)
TLS ve SSL mesajlaşma sırasında şifreleme yöntemlerini belrleyen alternatif protokollerdir. fakat TLS, SSL 3.0'a baz alınarak geliştirilmiştir. Aslında bir nevi yeni bir sürümdür fakat lisans sebepleri yüzünden yeni bir isim kullanılması uygun görülmüştür.

TLS, SSL'e göre daha genişlenitlmiş bir protokoldür. TLS, SSL protokolünü işler fakat iletişim başlangıcında direk olarak güvenli bağlantı üzerinden haberleşmez. Daha sonra güvenli bağlantıyı kurar. Oysa SSL de direk güvenli bağlantı işletilmelidir.

# ssl version history

| version | publish date |
|---------|--------------|
| SSL 1.0 | Unpublished  |
| SSL 2.0 | 1995         |
| SSL 3.0 | 1996         |
| TLS 1.0 | 1999         |
| TLS 1.1 | 2006         |
| TLS 1.2 | 2008         |
| TLS 1.3 | 2018         |

Artık SSL 2.0 ve 3.0 bulunan güvenlik açıkları sebebi ile güvensiz olduğu belgelerle resmileştirilmiştir.

# Simetrik Şifreleme Algoritması (yada gizli anahtarlı şifreleme yada secret key encryption yada symmetric-key algorithm)
bazı yerlerde eş anlamlı olarak bu terimlerde kullanılıyor: __single-key yada shared-key yada one-key__

Veriyi şifrelemek ve şifreli veriyi çözmek için her iki tarafında bildiği ortak-tek bir anahtar kullanılır.

# Asimetrik şifreleme Algoritmasi (yada asymmetric algorithm yada açık anahtarlı şifreleme Algoritmasi)
public-private anahtar yapısına dayanır. public key ile şifrelenmiş bir data'yı, ancak o private key ile birlikte oluşturulan public key ile düzgün şekilde açabiliriz. (aynı şey tersi için de geçerli - public ile şifrelenen sadece private ile açılabilir)

# anahtar (yada key) ve şifrelerin çözülme hızı
- public ve private key'ler birer şifredir. dosya olmalarının sebebi büyük boyutta olmalarıdır. örnek 512 karakterli şifre.

- anahtar ile şifrelenmiş bir verinin çözülmesi uzun sürmektedir. örnek: ortalama değerler üzerinden konuşursak; 2017'de ortalama bir bilgisayar, 128'lik bir sha ile şifrelenmiş bir veriyi 2 trilyon yılda çözebiliyor.

# passphrase
pass (geçiş) ve pharese (ifade) kelimelerinin birleşimidir. parola anlamında kullanılır.

ssh-keygen gibi komut satırı uygulamaları ile private-public key oluşturulmaktadır. bu private key'ler oluşturulduğunda bizden passphrase istenebilir. bu opsiyoneldir. eğer passphrase istenirse oluturduğumuz key bu passphrase ile şifrelenir. aynı şekilde public key'de buna istinaden oluşturulur. dolasyısı ile private key dosyamıza ulaşan hacker, şifreli dosyamıza ulaşmış olur.

# sayısal imza (yada e-imza yada elektronik imza yada sayısal imza yada electronic signature yada e-signature)
bu kavram bir şifrelereme algoritması değildir. sadece data'yı şifreleyen tarafın, kendi imzasınıda dataya eklemesi ve bu şekilde açan tarafın bu mesajın sadece o alıcıdan geldiğine emin olmasını sağlamasıdır + verinin bütünlüğünün (değiştirilmemiş olduğunun) sağlanmış olmasıdır.

# RSA
Bir çeşit asimetrik şifreleme algoritmasıdır.

# DES (yada Data Encryption Standart)
simetrik bir şifreleme algoritmasıdır.

# AES (yada Advanced Encryption Standard yada Gelişmiş Şifreleme Standardı)
DES artık eski kaldığı için pek tercih edilmemektedir. bu algoritma onun yerini alması için geliştirilmiştir. simetriktir.

# cipher (yada cypher)
şifrelemek için kullanılan algoritmadır.

# encrypting vs coding (yada encoding) | decrypting vs decoding
coding ve decoding bir girdiyi farklı bir formata çevirmek anlamına gelir. çevirirken public olan bir şema/formata çevrilmesi gerekir. yani şifreli bir işlem yapılmamaktadır. eğer şifreli bir işlem olursa o zaman encrypting/decrypting olurlar.

# base64
çift yönlü bir algoritmadır. anahtarsız geri çevrilme işlemi yapılabilir. bu sebeple şifreleme algoritması değildir. sadece çıktı kümesindeki karakterler ASCII 'nin sadece belirli elemanlarını içermektedir. çıktıda limitli bir karakter kümesi olabileceğinden terslik yapabilecek karakterler ortadan kaldırılmış olur. genelde işaret kararkerleri olmadığından; url, dosya sistemindenki dosya isimleri, xml, json içerisindeki degerler gibi kullanım alanları vardır.

base64'ün birçok implementasyonu var:

- RFC 3548 or RFC 4648 -> standart base64. Bu genel kabul gören ve özel bir amaç için tasarlanmayan base64.

- RFC 2045 -> MIME BASE64

- ve daha birçoğu: https://en.wikipedia.org/wiki/Base64

# java'da base64
javada birçok base64 encode/decode kütüphanesi var.

- java 6 ile

javax.xml.bind.DatatypeConverter.printBase64Binary("hello".getBytes()) ve DatatypeConverter.parseBase64Binary("aGVsbG8gd29ybGQ=") metodları mevcuttur.

- Java 8'de

daha esnekleştirilerek birçok base64 destekleyecek şekilde yeni embeded kütüphane gelmiştir. direk örnek üzerinden incelersek:

```java
Base64.Encoder enc = java.util.Base64.getEncoder(); // standart base64

Base64.Encoder encURL = Base64.getUrlEncoder(); // url safe base64

Base64.Encoder mimeEncoder = Base64.getMimeEncoder(); // mime base64

enc.encode(data.getBytes()); //ile encoding yapılıyor.
```

# hash algoritmaları (yada özet fonksiyonları)
hash türkçe anlamı: bir malzemenin işlemden geçirilerek yeniden sunulan malzeme anlamına gelir. örneğin; kıyılan etin tekrar sunulması gibi.

- çıktı ile, girdiyi bulmak mümkün diildir. yani algoritma tek yönlü çalışır. şifreleme algoritmalarında ise bunun tersi şeklinde; çıktıdan girdiyi (bir anahtar aracılığı ile) elde edebilirsiniz.

- tüm hash algoritmaları gibi; birden fazla girdi, aynı çıktıyı üretebilir. aynı büyüklükteki string'ler bile, aynı hash sonucunu verebilir. çünkü girdi her girdi'ye karşı bir çıktı olması şart olduğundan, birden fazla girdi aynı çıktıyı vermek zorunda kalacaktır. bu duruma collision (yada çatışma yada çarpışma) denir.

# hash kodları (yada hash toplamları (yada hash sums) yada Hash digest (yada hash özeti))
hash fonksiyonlarından dönen değerlere (çıktı değerine) denir.

# merkle tree (yada Hash tree)
özel bir tree veri yapısıdır. ağacın her yaprağı altındaki ona bağlı yaprakların hash'ini tutmaktadır. en üstteki ağacın kök yaprağına da "hash sum" adı verilir.

# kontrol toplamları (yada checksums)
dosyaların doğrulunu kontrol etmek istediğimizde yine "hash sums" değerine bakarız. fakat son kullanıcı açısından "hash sums" ile checksums farklı isimlendirilmiştir. oysa teknik anlamda aynı şeydir. 

# Blok Şifreleme (yada Block Cipher)
- girdi ve çıktının boyutunu etkilemez. algoritmanın detayları ile alakalıdır.

- blok şifreleme kullanan algoritmalarda, şifrelenecek tüm mesaj 'block size' kadar bölünür. her bölünen parça ayrı ayrı şifrelenerek şifrelenmiş halleri birleştirilir.

# MD (Message-Digest)
bir çeşit hash algoritmasıdır.

- md2
- md4
- md5: 128 bitlik bir çıktı verir. piyasada en sık kullanılandır.
- md6

# SHA (yada Secure Hash Algorithm)
bir çeşit hash algoritmasıdır.

- sha (yada sha-0)

- sha-1 - 160 bit (40 karakter) lik hash (çıktı) üretir.

- sha-2 - altında bir çok  şifreleme barındırır. her şifreleme ürettiği hash'in boyutuna göre isimlendirirlir. örnek:

   - 256'lık çıktı veren algoritma birçok isimde gösterilir: SHA-256 (yada SHA 256 yada SHA256 yada SHA-256 bit yada SHA2 256 yada SHA-2 256 bit)

   - SHA-224, SHA-384, SHA-512 gibi birçok örnek verilebilir.

   - Sayı ne kadar yüksekse o kadar güvenlik arttırılmış olur, çünkü kırılması o kadar zordur. çünkü; kombinasyonlar çok daha fazla olmaktadır.

# Neden hala SHA-512 yerine SHA-256 kullanılması öneriliyor?
Teorikte SHA-512 kullanılması önerilir. fakat pratikte 256'yı çözecek bir kudret olmadığından, 256 şu sebeplerden tercih ediliyor:

- 512'nin daha çok networkte bandwidth (yada bant genişliği), memory, cpu vb harcıyor olması

- 256'nın yazılımlar tarafından daha çok desteklendiği için (eski olduğu için) daha az bug'lı olması

# çığ etkisi (yada avalanche effect)
X string'in hash'i Y olsun. X stringinin bir karakterini değiştirdiğimizde ve tekrar hash aldığımızda Y ye benzer bir sonuc bekleriz, fakat çok farklı bir sonuc alırız. hash algoritmaları bunu bilerek yaparlar ki, değişiklikler daha belirgin olsun. buna çığ etkisi ismi verilmiştir.

# end-to-end encryption (yada E2EE)
arada farklı aracı makineler olsada şifrelenmiş data taşındığından ve sadece gönderen ve alıcının çözebildiği şifreleme sistemleridir.

# ssl sertifikları ile web sayfaları ilişkisi

- ## sertifika (yada certificate)
public key + ilgili kurum bilgileri

- ## ssl (secure socket layer)
soket üzerinden sertifika ile şifreleyerek data alışverişi yapan sanal katmandır.

tarayıcı üzerinden bir internet sitesine gittiğimiz zaman, public key'i site bize gönderir. artık yaptığımız her işlem (ajax işlemi) bu public key ile şifrelenerek gönderilir. artık bu bilgileri sadece private anahtara sahip internet sitesi açabilecektir.

- ## CA (yada Certification Authority yada sertifika otoritesi)
Sertifika vermeye yetkisi olan kurum. bu kurumun kendi sertifikasına "root" (kök) sertifikası denir. Pazarı yüksek olan bazı firmalar: IdenTrust, Sectigo, DigiCert, GoDaddy, Globalsign, Verisign. 

internet siteleri ssl sertifikalarını aracı kurumlara onaylatırlar. süreç bu şekildedir:
- web sunucu kendi public key (__SITE_PUB__) + private key'ini (__SITE_PRV__) localde oluşturur.
- public key'i CA'ya yollar.
- CA kendi private key'i (__CA_PRV__) ile __SITE_PUB__'nin hash'ini şifreyip yeni anahtar oluşturur (__X__).
- CA, __X__'i web sunucusu yetkililerine yollar.

Kullanıcı bir web sitesini ziyaret ettiğinde;
- web tarayıcısında CA'in public key'leri (__CA_PUB__) zaten yüklü olmalıdır.
- browser web sunucusundan ziyaret ettiği sitenin public key'ini (__X__) indirir.
- browser __CA_PUB__ ile __X__'i açar. __X__ kendi içinde domain ve ip bilgisi tutmaktadır. bu sebeple tarayıcı gittiği sitenin doğru olup olmadığını anlar.
- browser o anda simetrik şifreleme için bir anahtar yaratır (__S__). __S__ i tarayıcı sunucuya public key (__SITE_PUB__) ile şifreleyip yollar.
- Artık browser sunucuya bir istek yolladığında sadece __S__ şifrelenecektir. yani artık public/private keylere ihtiyaç yoktur. 

Web tarayıcıları __S__ değerini son kullanıcıya dahi vermiyor. Handhsake sırasında yaratılan ve simetrik haberleşmede kullanılAcak olan __S__ public key (__SITE_PUB__) ile şifrelendiğinden ve sadece sunucu tarafından açıldığı için, bİr hacker tüm akışı baştan sona takip edebilse bile, hiçbir şey yapamayacaktır.

TLS 1.2 için yukarıdaki adımları daha detaylı inceleyecek olursak:

  - client "__client hello__" (tam olarak bu string kullanılıyor) tipindeki mesajı ile handshake isteği yollar. bu mesajda:
    - client'ın kabul ettiği cipher'lerin listesi de mevcuttur.
    - session id: önceden varsa bu değer doludur. eğer bu değeri sunucuda onaylarsa tekrar handshake yapılmasına gerek kalmaz.
    - tls version
    - random bytes
    - extensions: extra değerler burada yollanır. örneğin opsiyonel kullnılan "session ticket" özelliği bilgileri burada yollanır.

  - server ise response olarak "__server hello__" tipinde mesaj döner.

  - buradan sonra server sertifikasını yollar ve client sunucu sertifikasını kendi içinde valide eder.

  - daha sonra "Key Exchange" adımına gelinir. burada kullanılabilecek bazı yöntemler şunlardır:
    - TLS_RSA
    - Diffie-Hellman (TLS_DH)
    - ephemeral Diffie-Hellman (TLS_DHE)
    - Elliptic Curve Diffie-Hellman (TLS_ECDH)
    - ephemeral Elliptic Curve Diffie-Hellman (TLS_ECDHE)
    - anonymous Diffie-Hellman (TLS_DH_anon)

    Bu adımda __S__ anahtarı oluşturulur. Nasıl oluşturulacağı burada belirtilen algoritmaya bağlıdır.

    Bu akışta kullanılan bazı terimler:

    - ## pre master key
        web tarayıcısnın kendi içinde oluşturduğu random key (__BS__)

    - ## master secret
        Web tarayıcısı handshake sırasında bir random bilgi, sunucuda dönüşünde bir random key yollar. bu 2 data ve __BS__ kullanılarak "master secret" (__S__) oluşturulur.

  - daha sonra "Server Hello Done" mesajı sunucudan client'a iletilir.

https ile haberleşildiğinde tüm http mesajı şifrelenir. header yada farklı bir bilgi açık bırakılmaz. url path'leri ve query variable'ları dahi şifrelenir. fakat neredeyse tüm web servisleri şifrelenmesi gereken datalar olduğunda hiçbir zaman GET isteği ile query parametrelerinde o datalar yollanmaz. onun yerine POST payloadında yollanır. aslında ikiside aynı kapıya çıkar. fakat GET isteği ile atılan istekler, hem web tarayıcısınının history'sinde, hemde server tarafının access log'larında görülür. bu sebeple post tercih edilir.

web sitenin anlaştığı sertifika firmasının web tarayıcılarla anlaşmış olması gerekmektedir. aksi durumda sertifika kırmızı uyarı verir. kırmızı uyarı sertifika var, fakat CA'lara tanıtılmamış anlamına gelir.

Bazı çakışmalar/uyuşmazlıklar:
- farklı web tarayıcıları farklı sertifika sağlayıcılarla anlaşabilir.
- aynı web tarayıcısı mobil versiyonu ile masaüstü versiyonunda aynı sertifika firmalarının bilgilerini bulundurmayabilir.
- aynı tarayıcı farklı versiyonlarında farklı sertifika sağlayacıyıcılarının bilgilerini bulundurabilir. 
- aynı tarayıcı farklı versiyonlarda aynı CA'leri bulundurur fakat farklı sertifikalarını  (farklı sürümlerini) bulundurabilir

bu tarz durumlar yaşamamak için her zaman globalsign, Verisign gibi gibi çok terhcih edilen firmalardan sertifika almakta fayda vardır.

sertifika çeşitlerine göre; web tarayıcısında logo, firma ismi vs gösterilme durumları vardır.

- ## Session ID
web sunucuna ilk istek atılacağı zaman client varsa eski Session ID'lerden devam edebilir. eğer önceden açılmış bir Session ID yoksa bu bilgi null gönderilir. session id var oldukça handshake'e gerek kalmaz.

- ## SessionTicket
session id'den farklı bir özelliktir. TLS extension olarak kullanılır. yani ekstra bir özelliktir. RFC buradadır: https://tools.ietf.org/html/rfc5077

- ## self sign key

public ve private key herhangi bir tool ile anında oluşturulabilir. bu anahtarlarda siteye gömülüp kulanılabilir. fakat web tarayıcısı kırmızı uyarı verecektir.

self sign aracılığı ile yapılan bağlantıların dışarıdan çözümlenme gibi bir durum söz konusu olamaz. aynı durum CA'e onaylatılan sertifikalar içinde geçerlidir.

self sign imza kullanırken şifre anahtarlarının boyutu arttırılarak güvenlik seviyesi çok rahat şekilde arttırılabilir. fakat dosya paylaşımı gibi uzun data işlemi gerektiren işlemlerde, anahtar boyutu sebebi ile şifreleme ve açma işlemleri cpu'ya yük bindirebilir.

- ## Intermediate CA
CA'lere 3üncü parti kuruluşlarda aracılık yapabilir. bu sebeple web tarayıcılarımızın "certificates" sekemlerinde içiçe sertifikalar görürüz.

- ## End Entity Certificate
son kullanıcıya ziyaret ettiği site aracılığı ile yollanan public key'dir.

- ## Certificate Signing Request (yada CSR)
CA'ya müşterisinin (yani web sunucusunun sahibinin) yaptığı sertifika imzalatma isteğidir.

- ## Subject Alternative Name
firefox sayfa ayarlarında web sitenin sertifika özelliklerini incelendiğimizde bu alanı görürürüz. bazı sertifikalar birden fazla domain için kullanılabiliyor. bu gibi durumlarda izin verilen domain listesi bu değerin içerisinde yazmaktadır.

- ## sayısal imza
Gönderici ---> Mesajı gönderen kişi, mesajı kendi private key'i ile şifreler. Mesajın yanına public key'ini de yerleştirir. alıcı taraf public key'i kullanarak private ile şifrelenmiş mesajı çözer. bu şekilde 2 şey kanıtlanmış olur:

  - verinin nereden geldiği kesinleşmiş olur. public key ile açtığımız veri ancak private ile şifrelenmiş olabilir. dolayısı ile private'ye sahip olan kişi, bizim iletişimde olduğumuz kişidir.

  - verinin bütünlüğü korunmuş olur. çünkü asıl mesaj şifrelenmeden önce hash değeri hesaplanıp, hash değeri şifrelenmeden önce mesajın sonuna koyuluyor. asıl mesajı açan alıcı, tekrar bir hash işlemi gerçekleştiriyor. hash sonucu çıkan sonuç mesajın sonunda olan hash değeri ile aynı olup olmadığına bakıyor.

- ## "servers" tab on firefox certificate manager
self-sign imzalı bir web sitesine firefox üzerinden gittiğimizde firefox exception fırlatır. eğer son kullanıcı add exception'a tıklarsa o sertifika bu listeye eklenir. 

- ## wildcard certificate
wildcard türkçe kelime anlamı: joker

her subdomain (a.google.com, b.google.com gibi) için ayrı sertifika alınması gerekir. fakat wildcard sertifikalar tüm subdomain'leri desteklemektedir.

## Java KeyStore (yada JKS) 
private ve public key'lerin, sertifikaların bir arada tutulabildiği dosyaya Java dünyasında verilen isimdir. keystore bir format değildir, bir java sınıfıdır:

> KeyStore store = KeyStore.getInstance("PKCS12");

__keytool__ komut satırı uygulaması keystore dosyaları üzerinde değişiklik yapmamıza olanak sağlıyor. tabi tüm keystore dosya formatları bu uygulama tarafından desteklenmiyor.

JKS gibi içinde certifika, key saklanan dosyalara yazılım dünyasında __keyring__ denir.

# TrustStore
içerisinde CA'ların sertifikaları bulunduran doayalara java dünyasında verilen isimdir. TrustStore bu dosyayı temsil eden sınıfın ismidir.

# Keychain
macOS'larda yüklü gelen GUI desteği olan key manager uygulamasıdır.

# X.509
Standartlaşmış digital bir sertifika formatıdır. Sertifika formatı dahilinde; sertifika imzalama algoritması, sertifika public key'i, versiyon, algoritma ID, seri numarası, konu, geçerlilik süresi gibi bilgiler barındırır.

# X.509 kullanan sertifika uzantıları

.pem

.cer, .crt, .der

.p7b, .p7c (PKCS#7)

.p12 (PKCS#12)

.pfx

# PKCS 
"RSA Security" kuruluşu tarafından yayımlanan "public-key cryptography standards"'ın kısaltmasıdır. yayımlanan bu güvenlik standartları ailesidir. 

15 adet standart içerir (yıl 2019). her sürüm farklı bir dosya formatı tanımlamaktadır.

```
ismi       version
PKCS #1    2.2
PKCS #3    1.4
PKCS #7    1.5
```

Tüm sürümler farklı zamanlarda çıkarılmıştır. Fakat sırası ile çıkmamıştır. Bu sebeple yeni sürümler daha iyi olarak düşünülmemelidir. dikkat edilirse PKCS sürümü ile isminde bulunan numara aynı değil.

# http'den https'e yönlendirme
http ile gidilen site dönüş değerinde https'e yönlendirme isteğinda bulunur. web tarayıcıda kullanıcyı https'e yönlendirir. eğer arada trafiğimizi izleyip manipüle eden birileri varsa, bu isteğin cevabında https'e yönlendirme yapmaz. normal http sitenin get isteğinin cevabını döndürür. artık son kullanıcı http ile iletişime devam edeceği için tüm iletişim baştan sona izlenebilir.

# "https everywhere" vs "smart https"
Web tarayıcı eklentileridir. Smart; her url'yi sorgulamadan https'e çevirir. Eğer herhangi bir cevap gelirse siteye gider, yoksa http olana gider. Fakat "https everywhere" her site için önceden ruleset tutar ve aşağıdaki gibi gelişmiş yönlendirmelerde yapabilir.

```
http://fr.wikipedia.org/wiki/Chose

https://secure.wikimedia.org/wikipedia/fr/wiki/Chose
```

# HSTS (yada HTTP Strict Transport Security)
Kullanıcyı http'den https'e yönlendirildiğinde sunucu bu header'ı döner:

> Strict-Transport-Security: max-age=16070400; includeSubDomains

max-age süresinde tarayıcı son kullanıcı http'ye gitmek istese bile https'e otomatik yönlendirecektir.

# Preloaded HSTS sites
HSTS için bir kere olsun siteye gitmek gerekiyor. bunu da yapmaya gerek kalmaması için tüm web tarayıcıların ortak tuttuğu bir liste var:

https://cs.chromium.org/chromium/src/net/http/transport_security_state_static.json

Bu listeye kadolmaz ücretsiz. bu bilgileri web tarayıcıları sürekli localde güncel tutuyor ve bu listedeki sitelere gitmek isteyen kullanıcıları direk https'e yönelndiriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Open Web Application Security Project (yada OWASP)
OWASP is an online community which creates freely-available articles, methodologies, documentation, tools, and technologies in the field of web application security.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Bot
Türkçe ismi de ‘bot''tur. Bazı kaynaklarda ‘robot' olarakta kullanılır. Otomatik işlem yapan yazılımlardır. örneğin; oyunlardaki bot'lar farklı yazılım modülleri (yazılımlardır).

# Internet bot
İnternet üzerinde otomatik işlem yapan yazılımlardır.

# Web crawler (yada web spider)
Bir çeşit internet bot'udur. Sadece web sayfalarını dolaşan yazılımlardır. Bu dolaşma işlemi, sitelerin eski halalerini arşivlemek, siteleri arama motorları için indexlemek, bir siteyi tüm alt syafaları ile download etmek (bazı download manager'lar yapıyor) gibi birçok amaç için olabilir.

# Web scraper (yada web harvesting yada web data extraction yada Web kazıma yada web hasat yada web veri çekimi)

HTML üzerinde verileri parse edip uygun verileri ayıklamak için gerekli yazılımdır. web crawler siteleri gezerken siteleri arkaplanda indirirler ardından, web scraper sitelerin içindeki bilgileri veritabanlarına uygun şekilde kaydeder.

############################

############################
# HARDWARE
############################

############################

# F5 Loadbalancer
F5 Network firması tarafından dağıtılan load balancer cihazıdır. birçok modeli vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ARM (yada eski adı:Advanced RISC Machine yada Acorn RISC Machine)
cep telefonları ve bazı oyun konsollarında kullanılan cpu tasarımıdır. CPU tasarımıdır; çünkü ARM firması, kendi başına işlemci üretmez, dizayn ve lisansı satar. CPU mimarisi özelleştirilebilir olduğundan her üretici farklı cpu performansları sergiler.

ARM'nin birçok sürümü vardır: ARMv1, ARMv2, ARMv2a...

Bu sürümler "aile" olarak gruplanmışlardır ve aile sürümü ile mimaris sürüm numaraları farklı gider. örnek: ARM8 ailesinin içinde, ARMv4 vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# sensor (yada sensör yada algılayıcı yada detector)

# Accelerometer sensor (yada ivmeölçer sensör) vs Jiroskop (yada Gyroscope)
İvmeölçer yönden bağımsız cihazın ivmesini ölçer, jiroskop ise telefonun hangi yöne doğru yatırılmış olduğunun bilgisi verir. jiroskop 360 derecede telefonun hangi pozisyonda olduğunu belirler ve bu pozisyonda saniyede kaç derece yön değiştirdiğini belirler. bu iki sensör çoğu telefonda bulunmaktadır bu sebeple halk arasında bu ikisinin görevi birbiri ile çok karıştırırılır. 

ivmeölçer m/s2 olarak dönüş yaparken, jiroskop rad/s (radian per second) olarak dönüş yapar. 

pi radyan = 180 derecedir.

# yakınlık sensörü (yada proximity sensor)
ekrana bir cisim yaklaşıp yaklaşmadığı bilgisini döner. örneğin; telefonla konuşurken ekrana kulak değmesin diye ekranı otomatik kapatmak için kullanılır.

# Manyetometre (yada magnetometer)
Bu sensör, manyetik alanları ölçerek hangi yönün kuzey olduğunu söyleyebiliyor. harita servisleri için çok kullanılıyorlar. Pusula (yada Compass) amacı ile kullanılıyor. Burada ufak bir ince nokta var: magnetometer ne kdar kusursuz donanımdan yapılsada, her zaman tam olarak dünyanın dönüşüne göre kutup yönünü göstermemektedir. çünkü dünyanın manyetik kutup noktası ile dünyanın geometrik/dünya dönüşü üzerinden hespalanan kutup noktası (tepe noktası) tam olarak aynı değil ve bu değer zaman içinde değişiklik göstremketedir. çok ince hesaplamalarda bunu dikkate almak gerekir.

# ışık sensörü (yada Photodetector yada tr:fotosel yada photosensors)
ortamdaki olan ışık seviyesini belirler. 
örnek kullanım: 
- oto parlaklık ayarı seçilmişse, güneşin durumuna göre parlaklığı oto arttırırır/azaltır.
- kamera uygulaması flash ayarını otomatik ayarlar

# Barometer (yada barometre)
ortamdaki/Atmosferdeki hava basıncını ölçen sensördür. dağcılar yüksekliği bulmak için ortam basıncını ölçmektedirler.

# Kalp Ritim Ölçer (yada Heart Rate)
sadece parmak ucu ile temas ettirildiğinde kalp atış hızını hesaplayabilmektedir.

# gravity (yada yerçekimi)
x, y, ve z için ayrı ayrı bu data sunulabilmektedir. örneği telefon dik tutulduğunda sadece Y yönünde bir çekim olduğu belirlenebilmektedir.

dönüş birimi: m/s2

# step counter
adım sayarlar genelde telefonun ayrı bir özel API'si ile sağşanmkatadır. çünkü adım saymak basit bir geliştirici için zordur. onun yerine işletim sistemi diğer sensörleri kullanarak basamak sayısını öngörmeye çalışır ve bunu ayrı bir API aracılığı ile sunar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Internet Protocol television (yada IPTV)
streaming bazlı internet altyapısı ile kullanılan TV izleme teknolojilerinin genel ismidir. uydu, kablolu internet gibi farklı teknolojiler altyapıda olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ROM
Bir kere bile program yazılmıyor içine. program hard-code çipin içindeki donanımda oluyor.

# Programmable rom (yada PROM)
bir kere program yazılır daha sonra tekrar içi silinemediğinden pek kullanılması tercih edilmiyor.

# EPROM (yada Erasable and Programmable Read Only Memory)
ultraviyole ışınlarla içi silinebilir.

# EEPROM (yada E2PROM yada Electrically Erasable and Programmable Read Only Memory)
Elektrik ile içi silinebilir. Ultraviyole ışık yerine elektrik kullanması çok büyük kolaylık sağlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# satellite (yada tr:uydu)

## uplink (yada U/L yada UL) vs downlink
plink uyduya client tarafından atılan sinyallardir. downlink ise sunucudan client'a atılan sinyallerdir.

## Uydu interneti
- Dünya yörüngesinde birçok uydu var. bu uyduların bazıları birden fazla görev üstlenirken bazıları sadece 1 görev yapmaktadırlar.

- internet kullanımı için bazı uydular vardır. bu uydular ile internet servis sağlayıcılar tarafından yörüngeye yollanırlar.

- bazı uydular sadece bilgi dağıtırlar bazıları ise hem alıp hem cevap verebilirler.

- eğer tek yönlü çalışan bir uydu ile internet sağlanacak ise; süreç şu şekilde ilerlemektedir:

  - pc'ye bağlı bir modem ile kablo servis sağlayıcısına yönlendirilmiş olmalıdır.

  - pc indirmek istediği dosyanın isteğini kablo üzerinden servis sağlayıcısına yapar.

  - servis sağlayıcı cevabı uydu üzerinden tüm dünyaya yollar.

  - pc cevabı alırken başka insanlarda bu aldığı cevabı okuyabilecektir. tek yönlü uydu internetleri genelde büyük dosya indirirken avntaj sağlamaktadır. 

- çift yönlü uydularda servis sağlayıcısına kablo ile bağlanmak zorunda değiliz.

uydu yörüngesini tespit maliyetli olduğundan, uydu interneti de maliyetli. Bu sebeple araba gibi cihazları internete bağlarken, araba firmaları, mobil telefon hatları firmaları ile anlaşıp, arabaya mobil hat gömmektedir. Bu şekilde 3g/4g gibi teknolojilerle standart internet kullanılmaktadır. kaynak: https://www.washingtonpost.com/technology/2019/12/17/what-does-your-car-know-about-you-we-hacked-chevy-find-out/ (archive date: 26/12/2019)

# amaçlarına göre uydular

- askeri güvenlik

- haritalama hizmetleri

- Navigasyon hizmetleri

- hava durumu tahmini

- bilimsel arge (örnek: Uluslararası Uzay İstasyonu (yada International Space Station yada ISS))

- internet

- tv yayını

- telefon (uydu telefonları için büyük antenli cep telefonu ve uydu alıcısı olan bir telefon şarttır. ve cep telefonunun uyduyu görebilecek bir alanda olması şarttır.)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Retina Display
retina kelimesi kamera için gerekli algılayıcıya verilen isimdir. bu isim insan gözündeki biyolojik retinadan gelmektedir. "Retina Display" ise Apple'ın patentini aldığı özel bir isimdir. Retina Display yüksek piksel yoğunluğuna sahip ekranlar için kullanılan bir markadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Harita hizmetlerinde trafik bilgisi kaynağı
Google Maps, Yandex maps gibi hizmetlerin trafik bilgileri anlık olarak güncelleniyor. Bunlar anlaşmalı lojistik firmalarının araçları (öneğin kargo firması), akıllı telefon kullanan ve GPS istatistiği paylaşan kullanıcıların bilgilerinin ortalamaları alınarak trafik bilgisi edinilmektedir.

Trafik bilgisi hesaplamaları birçok karmaşık algoritmalar içeriyor. Örneğin; harita kaynaklarında caddenin akışı (yönü) bellidir. Bu yöne ters giden kullanıcıların konum bilgileri hesaplamadan çıkarılmaktaır. Tamamen duran (beklemede) olan kullanıcıların konum bilgileri devre dışı bırakılmaktadır gibi... Sadece tekil kullanıcı göz önünde bulundurulmayıp, aynı çervede (örneğin 5 metrekarelik alanda) olan kullanıcıların tümü birlikte göz önünde bulundurulmaktadır. Örneğin; herkes hızlı hareket ediyor, sadece 1 araç yavaş ise o araç hesplama dışı bırakılmaktadır.

Caddelerde geçen araçları algılayan sensörler ile trafik ölçümü yapılabilmektedir. Fakat bu sensörlerin maliyeti (bakımı gibi) çok yüksektir.

Benzer şekilde caddelerde bulunana kameralardan görüntü işleme ile trafiği ölçmekte çok maliyetlidir.

# GPS'in her zaman konumu tespit edememesi
GPS normalde uydulardan yararlanarak konumunu tepsit etmektedir. Fakat AGPS teknolojisi ile baz istansyonlarindan da konum tespit edilebilmektedir. Bu teknoloji mobil internet verisi (3g, 4g gibi) üzerinden bilgi alisverisi yapmaktadir. Bu sebeple AGPS açikken internet verisinin de açik olmasi gerekmektedir. AGPS, GPS gibi uyduyu kendi bulabilir, fakat bazen GPS'in uydudan tam olarak konumunu tepsit etmesi 5 dakikayi dahi geçebilir. Oysa AGPS uydu bilgilerini direk olarak baz istasyonlarindan almaktadir. Baz istasyonlari bizim cep telefonlarimizdan güçlü oldugu için aninda konum tespiti yapabiliyoruz. AGPS bir kere uydularin pozisyonlarini aldiktan sonra baz istasyonlari ile iliskisini kesmektedir.

# geolocation nasıl konum tespit ediyor

html 5 ile gelen bu özellik tarayıcılarda bir API sunuyor fakat tarayıcının nasıl konumu tespit edeceği standartlar arasında değildir.

google sokak resimlerini çekmek için sokak sokak arçalarla dolaşıyor, bunun gibi bir çok işlem için sokak sokak dolaşan araçlar mevcut. bu araçlar etrafta algıladıkları wifi-isimlerini kaydediyor. bu isimler databasede, konum ve wifi sinyal gücü (o andaki wifi uzaklık bilgisi) bazlı toplanıyor.

web tarayıcımızda konum tespiti yapmak istediğimiz anda, tarayıcı etraftaki wifi isimlerini sinyal güçlerine göre topluyor (admin yetkisine gerek duymadan çoğu işletim sistemi bu bilgiyi sağlıyor). bu bilgiyi "Google Location Services" isimli web servisine gönderiyor ve eğer databasede kayıtlı ise konum bilgisini alıyor.

Google Location Services gibi birçok alternatif var (örnek: Mozilla Location Services), fakat en büyük veritabanı şimdilik (yıl 2018) google'da.

mobil tarayıcılarda "Google Location Services" hizmetine ek olarak, tarayıcı, GPS üzerinden de konum tespiti yapmaya çalışıyor.

"Google Location Services" database'i sadece sokak sokak gezerek bilgi toplamıyor. bir kullanıcı, hem wifi hem gps üzerinden konumunu tespit etmişse; aynı wifi'ye bağlı veya aynı çıkış IP'sine sahip olan her cihazı aynı lokasyonda olarak tanımlıyor. yani bir internet cafede yada şirket içerisinde bir kere lokasyon tanımlatmış biri, 1 hafta boyunca tüm herkese lokasyonun sağlanması için yeterli olabilir.

Yukarıda sayılan 2 madde gibi birçok algoritma daha mevcut: google servislerine kaydolmuş ve google uygulamamarını kullanan kişilerin cihazlarındaki uygulalardan toplanan wifi sinyalleri ve gps ile belirlenen konum bilgilerinden de çok fazla bilgi toplanabiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# bazı apple ürünleri

# iOS version history
| sürüm | çıktığı yıl |
|-------|-------------|
| 3     | 2010        |
| 4     | 2010        |
| 5     | 2012        |
| 6     | 2014        |
| 7     | 2014        |
| 9     | 2016        |
| 10    | 2017        |
| 12    | 2018        |

# macos version history

| number        | codename      | release date       | Most Recent Version and its release date |
|---------------|---------------|--------------------|------------------------------------------|
| Mac OS X 10.0 | Cheetah       | March 24, 2001     | 10.0.4 (June 22, 2001)                   |
| Mac OS X 10.1 | Puma          | September 25, 2001 | 10.1.5 (June 6, 2002)                    |
| Mac OS X 10.2 | Jaguar        | August 24, 2002    | 10.2.8 (October 3, 2003)                 |
| Mac OS X 10.3 | Panther       | October 24, 2003   | 10.3.9 (April 15, 2005)                  |
| Mac OS X 10.4 | Tiger         | April 29, 2005     | 10.4.11 (November 14, 2007)              |
| Mac OS X 10.5 | Leopard       | October 26, 2007   | 10.5.8 (August 5, 2009)                  |
| Mac OS X 10.6 | Snow Leopard  | August 28, 2009    | 10.6.8 v1.1 (July 25, 2011)              |
| Mac OS X 10.7 | Lion          | July 20, 2011      | 10.7.5 (September 19, 2012)              |
| OS X 10.8     | Mountain Lion | July 25, 2012      | 10.8.5 (12F45) (October 3, 2013)         |
| OS X 10.9     | Mavericks     | October 22, 2013   | 10.9.5 (13F1112) (September 18, 2014)    |
| OS X 10.10    | Yosemite      | October 16, 2014   | 10.10.5 (14F27) (August 13, 2015)        |
| OS X 10.11    | El Capitan    | September 30, 2015 | 10.11.6 (15G1510) (May 15, 2017)         |
| macOS 10.12   | Sierra        | September 20, 2016 | 10.12.6 (16G1212) (July 19, 2017)        |
| macOS 10.13   | High Sierra   | September 25, 2017 | 10.13.6 (17G65) (July 9, 2018)           |
| macOS 10.14   | Mojave        | September 24, 2018 | 10.14.6 (18G87) (August 1, 2019)         |

# iphone model farklılıkları
https://www.apple.com/tr/iphone/compare/ bu siteden karşılaştırma yapılabilir.

- "Plus" olan modeller (örnek "iphone 8 PLUS" vs "iphone 8"): sadece ekran boyutu, pil kapasitesi, kamera kapasitesi değişiyor.
- "S" olan modeller: S olan modellerde ekran boyutu değişmiyor fakat kamera ve pil ömrü değişebiliyor.

# iMac
masaüstü bilgisayar serisinin ismidir. kasa monitörün içindedir.

# Mac Mini
monitör olmadan satılan ufak kasadır.

# Mac
apple'ın laptop ve masaüstü bilgisayarlar için olan OS ismi.

# MacBook
laptop serisi.

# Mac Pro
sunucu bilgisayarlar.

# iPod
portable media oynadıcı.

# iPad
tabletler.

# MacBook Air
özellikle ince olması için tasarlanmış laptoplar.

# PowerPC
AMD, ARM ve Intel CPU'larına alternatif CPU mimarisi. Apple, IBM ve Motorola tarafından geliştiriliyor.

# Macintosh
Apple'ın bilgisayar modellerinin serisinin ismidir. hem laptop hem bilgisayarları kapsar. yani macOS çalıştırılan her cihazı kapsar.

# Hackintosh (yada OSx86)
MacOS'un Intel CPU'lu mimarilerde çalışması için yapılan projedir. Eskiden apple sadece powerpc'li mimarilerde çalışıyordu. daha sonra apple resmi olarak Intel mimarilerde çalışabilir hale geldi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HDMI (yada High-Definition Multimedia Interface)
Girişi ince ve çubuk gibi olan yeni media aktarım kablosu.

```
##########################
#   ..................   #
#  ====================  #
 #  ..................  #
  ######################
```

# DVI (yada Digital Visual Interface)
Girişi dikdörtgen (vga'ya göre daha ince ama daha uzun) olan media aktarım kablosu.

```
__________________________
|      ................  |
|  __  ................  |
|      ................  |
|________________________|
```

# VGA (yada Video Graphics Array)
Girişlerindeki köşeleri yuvarlar olan eski media aktarım kablosu.

```
 __________________________
(   ..................     )
 (   ..................   )
  (  ..................  )
   ――――――――――――――――――――――
```

# serial port (yada seri port)
kablo ile iletişimde dataların sıra ile iletildiği iletişim/kablo tipidir.

# COM (yada communication port)
Bir cihazda bir den fazla giriş varsa COM1, COM2 şeklinde adlandırılır. MS Windows bilgisayarlarındaki seri portlu girişler için bu terimi kullanmıştır.

# LinePrinTer (LPT yada Paralel Port)
EN çok pritner'larda kullanıldıklarından LinePrinTer olarak isimlendirilmişti. Her seferinde 8 bit (1 bayt) ile haberleşirler.

# Universal Serial Bus (yada USB)
- Seri port çeşididir. fakat eski seri port'lara göre ekstradan güç kaynağı yolu da vardır. bu eşkilde takılan cihaza elektrikte verir. üstelik eski seri portlara göre daha hızlı veri aktarımı yapabilmektedir.
- USB'nin bir çok sürümü vardır: 

# usb version history

| VERSION | MAX SPEED |
|---------|-----------|
| 1.0     | 1.5 MB/s  |
| 1.1     |           |
| 2.0     | 60 MB/s   |
| 3.0     | 625 MB/s  |
| 3.1     |           |
| 3.2     |           |

- USB'nin birçok sürümünü aşağıdaki kombinasyonları mevcuttur:

  - Type A
  - Type B
  - Type C
  - Mini A
  - Mini B
  - Mini AB
  - Micro A
  - Micro B
  - Micro AB

# plug and play (yada PnP yada tak çalıştır)
USB ve PCI'da kullanılabilen bir teknolojidir. USB'ye takılan cihaz belli standartlara uyduğu sürece (plug and play standartlarına) işletim sistemi takılan cihaz için kısmen/tamamen sürücü ihtiyacını algılayabilmektedir.

PnP desteği olan bir cihazı tanımama durumunun sebepleri:

- işletim sisteminin usb driver'ında sorun olması
- işletim sisteminda sorun olma durumu
- aygıtın sürücüleri sorunsuz tanınmıştır. fakat cihaz kendi sürücüsünün bir kısmını dışarıya verecek şekilde tasarlanmıştır. cihazın yönetimi için gerekli tüm sürücü tanınacak diye bir kaide yoktur. son kullanıcı olarak bizde hatalı tanındığını düşünürüz.
- iletişimde olan GUI uygulaması bizim işletim sistemimizde düzgün çalışmıyordur.

# Diğer kablo iletişim tipleri
- PCI (YADA Peripheral Component Interconnect)
- PCI-X: PCI'dan daha gelişmiş
- PCI express (yada PCI-e): PCI'dan daha gelişmiş

PCI girişleri genelde buna benzer:

```
 _________________________________________
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  ―――――――――――――――――――――――――――――――――――――――|
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
 ‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Üç boyutlu ses (yada environmental audio yada surround sound)
ses yaratan kaynakların yönlerinin kulağa doğru şekilde gelmesini sağlayan ses sisteminin genel ismidir. sanal ve gerçek olarak iki şekilde bu ses yaratılabilir. gerçek için; hoparlörlerin konumu önemlidir (sol alt, sol üst, sağ alt, arka gibi). çalınan ses dosyası da her ses objesini hangi kanaldan yayılması gerektiği bilgisini içermelidir. oysa sanal surround ses için ses sistemine ihtiyaç yoktur. sadece çalınan ses dosyası her hoparlörün nasıl çalışması gerektiği bilgisini içerir. basit tekniklerle sanal; bazı hoparlörlerin sesinin kısılması, tizin yada basın arttırılması gibi taktiklerle sanal surround ses çıkarılmaya çalışılır. örneğin 2 hoparlör var ise; önümüzden geçen bir helikopter için önce sağ hoparlörden daha fazla ses çıkarılır sonrada sol hoparlörlerden daha fazla ses çıkarılır. böylece helikopter sağdan sola geçiyormuş havası yaratılmış olunur.

# mono vs stereo
mono bir sesi tüm hoparlörden aynı şekilde yansıtan ses sistemidir. oysa stereo'da her hoparlör farklı davranışlar sergileyebilirler.

# Dolby Digital (yada eski adı: Dolby Stereo Digital)
Dolby firmasının geliştirdiği ses teknolojisinin özel ismidir. içerisinde birçok alt teknoloji barındırmaktadır. 

# Woofer vs speaker (yada hoparlör)
Woofer bas için özel geliştirilmiş bir speaker çeşididir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Personal computer (yada PC)
Genel bir terimdir. özel bilgisayar serisi ismi değildir. mac'lerde PC grubuna girer.

# Phablet (yada Phonelet yada Tabphone yada Fablet)
akıllı cep telefonları ile tabletlerin arasında büyüklükte olan cep telefonlarına verilen genel isimdir. en bilindik örnek samsung note serisidir.

# Commodore 64 (yada C64)
Eskiden piyasaya sunulan ve satışlarda rekor kıran bir ev bilgisayar modeli.

# mainframe
eski zamanlarda işletim sisteminde user'ların üzerinde process'ler çalıştırması çok gelişmiş kavramlardı. o zamanlarda bu işi çok güçlü makinalar özel amaçlar için gerçekleştiriyordu. günlük kullanımlarda yoktu. bu makinelere eskiden verilen genel isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Otomat (yada automata yada automaton)
- otomatik olarak ardışık veya döngüsel işlemleri gerçekleştirebilen mekanik veya elektro-mekanik düzenektir. 
- canlı bir varlığın yapacağı kimi işleri yapabilen mekanik ya da elektrikli araç ya da aygıt.

# otomat teorisi (yada özdevinim kuramı yada Automata theory)
soyut 'matematiksel' makineleri veya sistemleri ve bu makineleri kullanarak hesaplama problemlerinin çözülebilmesini araştıran daldır.

# Sonlu Durum Makinası (yada Finite State Machine yada Finite State Automaton)
eylemler sonucu belli statelere düşen makinalardır. sonsuz durum söz konusu değildir. belli eylemler sonucu belli state'lere düşer. 

# Muntazam Diller (yada Biçimsel Diller yada Formal Languages)
Tüm kuralları belli olan (syntax, words... gibi) yazılı dillere verilen bir isimdir. Örneğin; bilgisayara programlama dillerinin tümü biçimseldir.

# Turing makinesi
Turing (başka başlıkta detay var) makinesinin tarihte büyük önemi vardır. bilgisayarlar için programlama mantığının en temel işlevini yerine getirmektedir. Basitçe; 
- bir teyp (bant) üzerinden veri okur
- bir önceki veriyi okur yada bir sonrakini okur
- koşula(duruma) göre bir yere bir veri yazar(kaydeder). 
- tekrar ilk adıma gider

O zamanlar geliştirilen turing makinesi, onun özel ismi olsa da, günümüzde bu temel üzerine kurulu makinelere hala turing makinesi diyenler olmaktadır. Turing makinesi otomatlar içinde temel bir örnektir.

# Turing Complete
hesaplamalar/işlemler yapıp sonucu bulabilmemizi sağlayacak tüm makinelre turing-complete denir. örneğin java turing complete'dir. her şeyi yapabilir ve sonuca gidebiliriz. fakat hesap makinesi yada SQL dili turing complete değildir. sadece bi yere kadar işlerimizi görebilir. programlama dilleri sanal olsalarda turing-complete olabilirler. turing complete bir donanım da olabilir.

(not: sql ile de arkada native bir işlemi tetiklersem herşeyi yapabiliriz. fakat yularıdaki cümlede bu tarz durumların yapılamadığını varsaydık. sql'i sadece tablo çeken ve tablolara yazan komutlar olduğunu varsaydık.)

# polynomial vs non-deterministic polynomial
- bir programın inputa göre ne zaman sonlanacağı (çözülebileceği) çokterimli (polinomsal) bir ifade ile belirtilebiliyorsa o program polynomial'dir.
- polynomial sadece "P" ile de gösterilir. Tersi durum içinde "NP" kısaltması kullanılmaktadır.
- NP durumları n değerinin azaltıldığı ve bazı koşullarda kodun içinde kendini azalttığı programlarda söz konusu olmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Genetic algorithm
belli bir sayıda aday çözüm üzerinde seçim, çaprazlama ve mutasyon operatörlerini kullanarak en optitmum çözümler elde etmeye çalışan algoritmalardır.

Temel olarak şu döngü ile süreç işletilir:
- başlangıç popülasyonu (çözüm kümesi) (rastegel yada tahmin üzerine)
- uyumluluk hesaplaması (çözüm kümesindeki her eleman için uyumuluk hesaplaması yapılır)
- seçim operatörü uygulaması (her çözümün ayrı ayrı en iyi olanının incelenmesi)
- çaprazlama operatörü
- mutasyon operatörü
- program devam? program dur? kararı (en iyi çözüm yeterli mi? değilmi?)
  (eğer devam edilirse program 2'inci adıma dönecektir)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# quick format vs normal format
sadece ms windows'un sunduğu bir format atma şeklidir. tek farkı normal formatın, quick formata ek olarak, diski komple bad sectorlere karşı denetlemesidir.

# bad sector noktaları nereye kaydediliyor
bad sector tespit edildiği zaman MBR'ye kaydediliyor. bu sebeple; mbr yeniden yazıldığında bu bad sectorlerin listesi yokoluyor. zaten mbr boyutu çok ufak olduğundan sadece bir kısım bad sector burada tutulabiliyor.

# Self Monitoring Analysis Reporting Technology (yada S.M.A.R.T.)
sabit disklerde bulunan bir teknolojidir. hdd'de bulunan bad sectorleri, anormal sıcaklık durumları gibi bir çok parametreyi algılayıp son kullanıcıya iletilmesi için sürücüleri aracılığı ile işletim sistemine bildirir. SMART bilgileri işletim sistemine bildirir fakat kendi içinde bir kayıt olmalıdır. bu kayıtlar HDD parçasının özel bir kısmında (işletim sisteminin göremeyeceği bir kısımda) bulunmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cpu cache
cpu kendi içinde memory tutmaktadır. BU şekilde ram'den daha hızlı çekmesi gerektiği bilgileri burada tutar. ama pahalı olduğundan ram'e göre daha ufaktır. l1, l2 gibi adlandırılırlar. eskiden bu bellekler anakart üzerindeydi fakat daha sonra anakarta erişim süresi ile zaman kaybetmemek için cpu'lara entegre edildi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# iot (yada internet of things yada nesnelerin interneti) vs ioe (yada internet of everything)
bu iki kavramın farkı net olarak belirgin değildir. ikiside aynı kapıya çıkmakta. IoE, Cisco firmasının kendi iot ürünlerine verdiği isimdir. fakat genel olarak ioe; tüm internet'e girebilen kavramları kapsar (internet of social media, Internet of Information(blogs and websites) gibi), oysa iot sadece internete girebilen cihazları kapsamaktadır. iot, ioe'den türemiştir diyebiliriz. ioe o kadar geniş bir kümeyi içine alırki; internete bağlanmış insanlar (son kullanıcı) bile bu kümenin içindedir.

# m2m (machine to machine)
iot'den çok farklı bir kavram değil. makineler arası iletişim kuran cihazlara denir. m2m'lerde, iot lerden farklı olarak; insana sunulan arayüzün olmamasıdır (yada aşırı kısıtlı olmasıdır). bunun dışında; m2m'de makineler birbiri arasında iletişim kurarken, iot kavramında; daha komplike bir yapıyı ele alır (iot için geliştirilmiş özel yazılımlar ve algoritmalar ve protokoller). m2m, iot kümesinin içindeki bir elemandır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mobil iletişim terimleri

## GSM (yada Global System for Mobile Communications)
Baz istasyonlarından yararlanılarak kullanılan mobil iletişim protokolleridir.

# GSM standartlarındaki protokollerin sürümleri (nesilleri - generations):

- 0G (yada pre-cellular) analog veri akışı kullanılırdı.

- 1G analog veri akışı kullanılırdı.

- 2G - İlk sayısal veri akışı kullanan GSM teknolojisidir.

- 2.5G - kullandığı teknoloji: GPRS yada G (GPRS'in ilk karakteri) yada General Packet Radio Service

- 2.75G - kullandığı teknoloji: Enchanced GPRS yada EDGE yada E (EDGE'nin ilk karakteri)

- 3G - kullandığı teknolojiler:

   - UMTS (yada Universal Mobile Telecommunications Service)

   - FOMA

   - W-CDMA

   - WCDMA (yada Wideband Code Division Multiple Access)

   - WCDMA2000

   - HSPA yada H yada High Speed Packet Access

     HSPA = HSDPA (yada High Speed Downlink Packet Access) + HSUPA (yada High Speed Uplink Packet Access)

- 3.5G (3G+ yada Turbo 3G yada H+ yada HSPA+ yada Envolved HSPA)

- 4G (yada LTE yada Long-Term Evolution)

- 4.5G (yada 4.25G yada LTE-A yada LTE-Advanced)

Not: 

- LG G3 (Canada), LG G3 (Korea) gibi aynı cihazın birçok farklı ülke ismi ile türevi görülecektir. Yukarıdaki tabloda görüldüğü üzere özellikle 3G'nin birden fazla teknolojisi var. Bir ülke birden fazla teknolojiyi destekleyebilir. İşte bu sebeple teknolojilere uygun aynı telefonun modelleri çıkarılmaktadır.

- Bazı telefonların aynı modeli Wi-Fi yada LTE gibi farklı prefix'ler almaktadır. 4G'li olan cihazlar hem Wi-Fi hem 4g'yi desteklemektedir. Oysa Wi-Fi olan sadece Wi-Fi'yi destekliyor. Wi-Fi olan modeller %99 tablettir yada phaplettir. SIM kart hiç takılamıyordur. Bu tarz özellik karşılaştırmaları bu tarz sitelerden yapılabilir: https://www.gsmarena.com/compare.php3?&idPhone1=8505&idPhone2=5253

1 saniyede teorik max download/upload hızları (bu hızlara pratikte ulaşamıyorlar):

| Sürüm | max download/upload speed |
|-------|---------------------------|
| 0G    | internet yok              |
| 1G    | internet yok              |
| 2G    | 10kb      / 10kb          |
| 2.5G  | 50kb      / 25kb          |
| 2.75  | 200kb     / 100kb         |
| 3G    | 370kb     / 120kb         |
| 3.5G  | 14-84mb   / 5-23mb*       |
| 4G    | 100mb     / 50mb          |
| 4.5G  | 1gb       / 500mb**       |

- 3.5G'nin birçok  sürümü çıktı. en eski ve en yeni sürümünü hızları yazılmıştır.

- 3.5 ve üzerinde hız çok yüksek olduğundan kota tüketimi hızlı olmaktadır. bu sebeple güncel mobil uygulamalar bundan korunacak şekilde tasarlanmaktadır. örneğin; video player'lar hiçbir koşulda videonun tamamını indirmiyor. sadece kullanıcının bulunduğunu noktanın ve birkaç saniye ilerisi yükleniyor.

# GPS (yada Global Positioning System)
sadece konum bilgisini elde etmek için gerekli protokol. BUnun için fırlatılmış uydulardan bilgi alır. Bu bir GSM protokolü değildir.

# Roaming (Dolaşım)
- Aynı hat ile yurtdışında GSM protokolünün kullanımına verilen isimdir.

- network dünyasında ise bu terim farklı amaçla kullanılıyor: bir cihaz, bir baz istasyonu/modem'e bağlıdır. eğer cihaz yer değiştirirse farklı bir cihaza bağlanabilir. bu bağlantı sırasında yapığı işlem kesilmeyebilir. işte böyle bir durumda bu cihaz dolaşım yapıyor anlamına gelir.

# Cellular network (hücresel ağ)
WIFI için modemler, GSM için baz istasyonları birer hücredir. Hücrelerden yararlanan bu tip ağların tümüne hücresel ağ denir. Fakat piyasada genelde mobil ağları temsilen kullanılır.

# SIM (yada Subscriber Identity Module yada Abone Kimlik Modülü) kart boyutları

Full-size (1FF) telefon kartları ile birlikte dağıtılan SIM'lerdir. gerçek bir kredi kartı büyüklüğündedir. genelde telefon kulübelerine takmak için kullanılır.

Mini-SIM (2FF) - 3ff'e göre biraz daha büyük b ir çerçeve kaplar. 

Micro-SIM (3FF) - çip'in etrafını çok çok ufak bir çerçeve kaplar

Nano-SIM (4FF)  - sadece çip'in kendisi vardır.

# SMS (yada short message service)
sadece text yollanması sağlanıyor.

# MMS (yada Multimedia Messaging Service)
media dosyaları da yollanabiliyor.

# Unstructured Supplementary Service Data (USSD)
- yazılımcılar arasında "Quick Codes" veya "Feature codes" olarak adlandırılırlar.

- SIM kartın servis sağlayıcısına istek atıp cevap almayı sağlayan kodlardır. örnek: \*123# bize kalan kullanım miktarımızı döndürmektedir.

- bazı kodlar USSD değildir. örnek IMEI döndüren \*#06# kod telefon üreticisinin bir kodudur ve servis sağlayıcısına erişim gerektirmeden cevap döndürür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# IMEI (yada International Mobile Equipment Identity)
Cihazın donanımında yüklenen bu tekil 15 haneli değer, yazılım tarafında dışarı iletilirken değiştirilebilmektedir. BU sebeple servise sağlayıcılar birden fazla IMEI'li cihazın kullanımına izin vermektedir. Fakat türkiye gibi ülkelerde cep telefonu numarası ile IMEI isteğe bağlı eşleştirilmektedir. Zira türkiyede aynı anda birden fazla cihaz aynı IMEI kullanamamaktadır. Böyle bir durumda telefon kullanılabilir, fakat o cihazda hat kullanılamaz duruma gelmektedir. Hat takılamayan cihazlarda IMEI bulunmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Near Field Communication (yada NFC) vs Bluetooth vs Wireless vs Wifi (yada Wireless Fidelity)
Wireless tüm kablosuz iletişimlere verilen genel isimdir. Telsizler, WI-FI, NFC, Bluetooth gibi tekneolojiler wireless kategorisindedir. Wi-fi halk arasında en çok kullanılan teknoloji olduğunda çoğu zaman sadece wireless olarak adlandırılır.

| name      | max distance (about) | Bandwidth |
|-----------|----------------------|-----------|
| NFC       | 5 cm                 | 2 kbps    |
| Bluetooth | 30 metre             | 800 kbps  |
| wifi      | 90 metre             | 11 Mbps   |

# bluetooth version history

| version |
|---------|
| 1.0b    |
| 1.1     |
| 1.2     |
| 2.0     |
| 2.1     |
| 3.0     |
| 3.1     |
| 4.0     |
| 4.1     |
| 4.2     |
| 5       |
| 5.1     |

her versiyon tüm versiyonlar ile geriye uyumlu çalışıyor.

Bluetooth 4.0, "Bluetooth Smart" ismi ile pazarlandı. Bluetooth 4.0, aynı zamanda "Bluetooth Smart" özelliği içeriyor. Bu özellik opsiyonel kullanılabiliyor. "Bluetooth Smart" özelliği geriye uyumlu diil. hatta "Bluetooth Smart" kullanan bir cihaz 4.0 öncesi Bluetooth cihazları ile birbirlerini pair olarak göremezler. kaynak: https://www.lifewire.com/why-your-bluetooth-wont-pair-534650 (archive date: 05/12/2019)

"Bluetooth Smart Ready", Bluetooth 4.0 olan "host" olan cihazların logosunun altında yazmaktadır. "Ready" ek bir özellik değildir.

Bazı sürümler sadece yazılımsal update gerektirirken, bazı sürümler hem yazılımsal hemde donanımsal güncellemeyi şart kılar.

her versiyon ekstra bir özellik içeriyor:

| version of Bluetooth | Basic rate (BR) | Enhanced Data Rate (EDR) | High Speed (HS) | Low Energy (LE) | Slot Availability Masking (SAM)  |
|----------------------|-----------------|--------------------------|-----------------|-----------------|----------------------------------|
| 1.x                  | Yes             | No                       | No              | No              | No                               |
| 2.x                  | Yes             | Yes                      | No              | No              | No                               |
| 3.x                  | Yes             | Yes                      | Yes             | No              | No                               |
| 4.x                  | Yes             | Yes                      | Yes             | Yes             | No                               |
| 5.x                  | Yes             | Yes                      | Yes             | Yes             | Yes                              |

Bluetooth standartlarına ek olarak bazı standartlar geliştiriliyor.  bu şekilde; örneğin; sadece "stereo kulaklıklar" için standart geliştirilmiş oluyor. bu standartlara Bluetooth dünyasında "profil" deniliyor. birçok profil burada açıklanıyor: https://en.wikipedia.org/wiki/List_of_Bluetooth_profiles (archive date: 05/12/2019)

Bluetooth profilleri sadece belli Bluetooth versiyonlarını desteklemektedir.

Bluetooth cihazlarınında MAC adresleri vardır.

Android üzerinden örnek kod ile akışı anlatalım:

```java
BluetoothHeadset bluetoothHeadset;

// Get the default adapter(device)
BluetoothAdapter bluetoothAdapter = BluetoothAdapter.getDefaultAdapter();

private BluetoothProfile.ServiceListener profileListener = new BluetoothProfile.ServiceListener() {

    public void onServiceConnected(int profile, BluetoothProfile proxy) {
        if (profile == BluetoothProfile.HEADSET) {
            bluetoothHeadset = (BluetoothHeadset) proxy;
        }
    }
    public void onServiceDisconnected(int profile) {
        if (profile == BluetoothProfile.HEADSET) {
            bluetoothHeadset = null;
        }
    }
};

// Establish connection to the proxy.
bluetoothAdapter.getProfileProxy(context, profileListener, BluetoothProfile.HEADSET);

// ... call functions on bluetoothHeadset

// Close proxy connection after use.
bluetoothAdapter.closeProfileProxy(bluetoothHeadset);
```

Pair olan iki cihaz arasındaki iletişim client-server mimarisi ile aynıdır. Android'de BluetoothServerSocket ve BluetoothSocket kullanılarak bilgi alışverişi olur. 

# Radyo Frekansı ile Tanımlama (yada RFID yada Radio-frequency identification yada High-Frequency RFID yada HF RFID)
radyo frekanları ile veri transferi teknolojisidir. "Tanımlama" kelimesini almasının sebebi, ortamdaki birçok farklı frekans/data arasından doğruyu istenileni algılayabilmesidir.

RFID üç temel nesneden meydana gelir: Okuyucu, Anten ve etiket. Etiket ve anten aynı cihazda olmalıdır. Etiket olan cihazda bir çip olması da gerekmektedir.

NFC, RFID'nin bir uzantısıdır. NFC çift yönlü iletişimi sağlarken, RFID tek yönlüdür. Bunun sebebi aslında; NFC'nin hem "nfc okuyucu" hemde "nfc etiketi" olma özelliğidir.

RFID etiketleri 3 çeşit olabilir:
- aktif olanlar kendi içlerinde bir güç kaynağı barındırırlar ve uzak mesafeden ID'nin okunabilmesini sağlarlar.
- pasifler ise gücü okuyucudan alırlar bu sebeple yakın mesafeden okunmaları gerekir.
- yarı pasifler ise; kendi içleride pil barındırırlar fakat sadece okunduklarında devreye girerler ve okuyucudan güç harcamazlar. sadece tetiklenirler. Bu çeşit piller günlük kullanılan kartların içine sığmakta ve çok uygun fiyattadırlar.

# Smart kart vs manyetik kart vs chip'li kart

## manyetik kart
kartın arkasında bulunan bir şerit üzerindedir. çabuk bozulur ve kopyalanabilirler. bu şerit içindeki veri değiştirilememektedir.

## çipli kartlar
çip elektronik devre anlamına gelir. chip'li kartlar bir elektronik devreye sahiptirler. process işlemi yapabilirler. içinde bellek vardır. içindeki değerler process işlemi sırasında değiştirilebilirler. chip gibi gelişmiş tüm kart teknolojilerine genel olarak "smart cart" adı verilir. bu kart tipleri sadece kredi kartlarında değil, birçok kartlarda tercih edilmektedirler.

Manyetkikler sadece bilgi barındırırken, çipliler bilgiye ulaşmadan önce ve sonra process yapacak elektronik devrelere sahiptirler. Bu sebeple; çipin içindeki bilgi klonlanamaz. Çünkü kart-reader tarafındaki private key ile ancak çip(kart) doğru dönüşü yapmaktadır. Zaten yaptığı dönüşte yine bir key ile şifrelenmiş ve ancak kart-reader içine gömülmüş olan key ile açılabilmektedir.

# temaslı kartlar
çipli ve manyetikli kartlar fiziksel olarak okuyucuya temas ettirilmelidir.

# temazsız kartlar (yada Proximity card yada Proximity card)
Proximity kelime anlamı: yakınlık

temaslı akıllı kartlar direk olarak, güç kaynağına temas ettirildiği için elektrik akımını alabilirler. temassız kartlarda ise ekstradan bir anten vardır. kart okuyucu sürekli elekromanyetik dalga üretir. karttaki anten elektromanyetik sinyalleri elektrik akımına çevirir. bu şekilde kart içerisinde güç kaynağı elde edilmiş olur. data okunma işlemi yapıldıktan sonra kart tarafında da elektromanyetik dalga üretilir ve data okuyucuya yollanır.

Wireless şarjlarda aynı teknoloji ile çalışır. Wireless şarj aleti elekromanyetik dalga yaratır. Bu dalgaları alan cep telefonu şarj olmaya başlar.

temazsız kartlarda da akıllı olan (process yapan) veya sabit bilgiyi döndüren kartlar olabilmektedir.

pasif olan standart temazsız kartlar 2-5 santim arasından okuma işlemini yapmaktadırlar.

aktif olan standart temazsız kart teknolojileri 150 metreye kadar algılama yapabiliyor.

# MIFARE
özel şirkete ait temazsız kartlar için geliştirilmiş çip ailesidir. birçok sürümü vardır.

# beacon
türkçede el feneri; işaret ışığı (örnek: arabalarda olan) anlamına gelen cihazların tümüne verilen isimdir.

# Bluetooth beacons
Düşük enerjili Bluetooth teknolojisi ile çalışan cihazlara verilen genel isimdir. Bu cihazlar yakında olan diğer Bluetooth alıcılara bildirim yollamak ve eğer alıcıda kabul etmiş ise; bildirim almak için kullanılmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# nano teknoloji
günümümüzdeki tüm teknolojik cihazlar (elektronik aletler...) sadece birçok molekülün bir araya getirilerek, karıştırılarak, yada kimyasal filtreleme işlemlerinden geçirilerek, ufak parçalara bölünereke yapılmaktadır. fakat nano teknoloji ile; atomlar (yada atomların yanyana gelerek yarattıkları moleküller) tamamiyle insanın tasarladığı gibi oturtulabilecekler. yani; atom seviyesinde montaj yapılabilmektedir. durum böyle olunca şu andakinden binlerce kat daha ufak cihazlar tasarlanabilecek.

# kuantum bilgisayarlar (yada Quantum computers)
bit yerine qubit(yada qbit yada quantum bit)'lerin kullanıldığı donanımlardır. bir bit; 1 yada 0 dan oluşurken, qubit, hem 1 hemde 0 bir arada oluyor. örneğin; %30 1, %70 0 (yada 0.3 olarak yazılıyor)  gibi durumlarda olabiliyor. bu da işlemlerin hızlanmasına sebep oluyor. 0-1 arasında olan bu değere süperpozisyon denilmektedir.

çalışan algoritmaların ve programlama dillerinin buna göre tasarlanmış olması gerekiyor.

kuantum mekaniği atom ve atom altı teknolojilerle ilgileniyor. kuantum mekaniği alanındaki çalışmalar ile nano teknoloji alanındaki çalışmalar çok fazla ortak yönleri olduğundan insanlar arasında bu iki teknoloji karıştırılmaktadır. oysa nano teknoloji ile quantum mekaniği farklı konulardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# integrated circuit (yada monolithic integrated circuit yada IC yada chip yada microchip yada çip yada mikroçip yada tümdevreyada yonga yada kırmık yada tümleşik devre yada entegre devre)
elektronik devre anlamına gelir. birden fazla devre elemanının birleştirilmiş parçadır.

# Mikroişlemci (microprocessor)
içinde ALU bulunana mantıksal işlemleri yapmak için giriş çıkış birimleri olan elektronik çip'tir. genel amaçla tasarlanır. dışarı tarafta neye hizmet edeceklerin bir önemi yoktur.

# mikrodenetleyici (microcontroller)
dışarıda neye hizmet edeceklerine göre özel tasarlanırlar. bacaklarından aldığı komutları işlerler. bu komutlar dışında parametreleri yoktur. microprocessor, microdenetleyici'den daha pahalıdır. çünkü microprocessor içindeki ALU genel amaçlı olduğu için birçok ALU işlemi barındırır. oysa mikrodenetleyici içindeki ALU sadece mikrodenetleyici API'si içindeki komutları çlıştıracak şekilde optimize edilmiştir.

# central processing unit (CPU)
CPU bir Mikroişlemci'dir. sadece bu özel ismi; tüm sistemde (bilgisayar, telefon, buzdoladı…) asıl microişlemci görevinin hangi parçaya ait olduğunu göstermek için verilmektedir. Bir bilgisayarın tüm birimlerinde birer ufakta olsa microişlemci olabilir. Bu durumda tüm sistemde birden fazla microişlemci olmuş oluyor. Fakat tüm sistem için asıl CPU'lar bellidir.

# CPU ve GPU (graphic process unit)
arasındaki farkı anlamanın basit bir yolu, görevleri nasıl işlediklerini karşılaştırmaktır. CPU ardışık seri işlem için optimize edilen birkaç çekirdekten oluşurken, GPU birden fazla işi eşzamanlı olarak yürütmek için tasarlanan binlerce daha küçük, daha verimli çekirdekten oluşan büyük ölçüde paralel bir mimariye sahiptir. GPU'lar paralel iş yüklerini verimli bir şekilde işlemek için binlerce çekirdeğe sahiptir.

# PIC
are microcontrollers made by Microchip Technology. PIC'ler içerisindeki bulundurduları bellek sayesinde programlanabilirlerdir.

# programmable logic controller (yada PLC yada programmable controller)
microcontroller'dır. içindeki hafızası sayesinde programlanabilirdir. özellikle endüstride fabrikalarda makinelerin işleyişini yönetmek için kullanılır. kolay ve hızlı programlanabilmesi için genelde IDE gibi destekleri ve hazır kütüphaneleri vardır.

PLC dünya pazarında sırası ile (çoktan aza doğru) bu markalar (brands) hakim: Siemens, Rockwell Automation, Mitsubishi, Schneider.

# Arduino
açık kaynak donanım prensiplerine dayalı microcontoller'dır. işletim sistemi yada firmware barındırmaz. direk olarak üzerine yazılan kod çalıştırılır. çok basit bir yapıdadır.

# rasperry pi
işletim sistemi barındırır. bir mini-bilgisayar gibi düşünülebilir. donanım açık kaynak değildir.

# single-board computer (yada SBC)
sadece kasa taraı olan mini bilgisayarlara verilen isim. örneğin: rasperry pi.  Arduino tam olarak bütün bir bilgisayar kasası işlevini yerine getiren modüllerden oluşmadığından (video kart girişi, enthernet girişi gibi) single-board computer grubuna giremez.

# Gömülü sistem (yada Embedded system)
içindeki logic'i programlamak (güncellemek) için elektriği kesmen gereken sistemdir.

elektrik kesilir. donanımın programlama için (developerlar için) farklı bacakları vardır. bu ayaklar aracılığı ile yine elektrik verilerek programlanır. programlama sonucu gömülü sistem içindeki memory'ye veri yazılır. daha sonra tekrar normal şekilde ayağa kaldırılır.

Bu tarz cihazlar genelde bir sistem için spesifik amaçla hazırlanmış olurlar ve genelde bir sistemin içine yapışmış şekilde gelirler. bu sebeple insanlar embbeded kelimesinin gerçek sebebini bilmeden kullanırlar. fakat asıl sebep yukarıdaki gibidir.

# PDA (yada Personal Digital Assistant yada personal data assistant yada handheld PC)
Cebe sığacak boyutta olan tüm bilgisayarlardır. Günümüzün cep telefonları PDA kategorisindedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# temel devre elemanları

# pozitif/negatif yük
bir maddede proton'dan fazla elektron var ise; o madde eksi yüklüdür. tersi olunca o madde + yüklü olur. bu sebeple; bir devrede elektronlar gerçekte -'dan +'ye doğru akar. fakat hesaplamalarda tam tersi uygulanır.

# voltaj
iki kutupun arasındaki potansiyel farktır. birimi volt. potansiyel farkın yarattığı hız.

# direnç (yada resistor)
Birimi Ohm (birimin simgesi (omega işareti) Ω'dur).

# akım (yada current)
birimi amper'dir. potansiyel faRkın dirençe karşı uyguladığı kuvvete amper denir.

# OHM Kanunu formülü
V = I # R

# pil
güç kaynağıdır (yada power supply). pilin üstünde voltaj ve amper yazması gerekir.

# Elektriğin gücü
Birimi WATT. V*I formulünden bulunur. WATT bir güç birimidir.

# DC (yada direct current)
- den artıya gidiş sürekli düzenli. bir elektron hiç kablo üzerinden gidiş geliş yapmıyor.

tomas edison'un çalışmaları ağırlı olraka bu yöndeydi. 

# General Electric
tomas editosun'un kurduğu 2017'de hala faaliyette olan şirket.

# AC (yada alternatif current)
tesla'nın çalışmaları ağırlı olarak bu yöndeydi. ac'de akış halinde olan elektronlar kısa vadeli gitgeller yapabiliyor.

# bobin
elektrik yükü biriktiren component.

# röle
içinde bir mıknatıs barındırıyor. anahtarlama yerine kullanılıyor. anahtarlama uçlarına güç verilerek açılıp kapatılıyor.

# priz
Prizler guc kaynagidir. Bir tarafi arti bir tarafi eksidir. prizler takilan devrenin direncine gore amper yayar. Bu sebeple sabit enerji harcanmaz. TakiLan alete gore elektrik harcanir.

arti ve eksi yonu prizlere ters takilirsa devre yine calisir. Fakat bu durum normal piller/kaynaklar icin gecerli diildir. Prizlerin ici buna gore tasarlanmistir. Bu durum ileri seviyede elektrik bilgisi gerektiren bir fizik konusuna dayanır.

# akım ve volt değişimi
seri bağşanmış akışlarda direnç ile karşılaştıkça amper azalır voltaj da azalır.
oysa paralel bağlanmış akımlar var ise; amper aynı kalır fakat voltaj 2'ye bölünür.

# transistör
anahtarlama yerine kullanıyor. anahtarlama uçlarına güç verilerek enable/disable ediliyor. transistör benzer şekilde farklı amçlar içinde kullanılıyor. örneğin; voltaj yükseltici.

# diode (yada diot)
ac'yi dc'ye çeviren component. ve sadece tek yönde geçişe izin veriyor. + dan eksiye gidiş varken , pil yön değiştirdiğinde akımın geçmemesini sağlar.

# alternatör
mekanik enerjiyi elektrige ceviren sistemdir. Barajlar buna ornektir. Barajlar basınç ile elektrik üretir. 

# çip'lerin bacakları
çiplerin birçok bacağı vardır. bir çipi kullanmak istediğimizde bu bacaklarına komut gönderiririz. bir komutu tanımlamak için en az 8 bit'e ihtiyacımız vardır. 8 sayısı standart olarak belirlenmiştir. zaten 8'in altı çipin dışardaki API'sini çok kısıtlı olmasına sebep olacaktır.
Bir çip eğer 8-bit desteği var ise (örneğin bir bilgisayardaki işlemci 32-bit 64-bit desteği olması gibi), o çipe sadece 1 komut sadece 8 bacağından birden (aynı anda) verilmelidir. Ancak işlem bittiğinde  bir komut daha verilebilir. Fakat 16-bit bir çip'e aynı anda 2 farklı komut verilebilir. Çip bu aldığı 2 komutu paralel şekilde yürütecektir.

# componentlerde uzun bacak vs kısa bacak
uzun bacak her zaman + yönünde, kısa bacak - yönünde bağlanmalıdır. kırmızı renk genelde + yönünde bağlanacak kısmı temsil eder. siyah ise - yönünü temsil eder.

# potansiyometre (yada reosto)
ayarlı (arttırıp azaltılabilen) dirençtir.

# osiloskop
elektriğin dalgalarını (direct or alternatif current) grafikte gösteren cihazdır.

# kısa devre
akım her zaman direnci az olan yoldan geçer. Örneğin aşağıdaki gibi bir durumda akım boş olan yoldan geçecekti. boş olan yol aslında çok çok az da olsa direnç barındırır. bu sebeple V=IR formülünde R küçüldükçe I artacağı için I çok büyür ve kablo buna dayanamaz erir yada güç kaynağında patlama meydana gelebilir. bu duruma kısa devre adı verilir.

```
- +V- ----------R----------
-         -         -     -
-         -         -     -
-         -----------     -
-                         - 
-                         -
---------------------------
```

# Topraklama
- istege bagli yapilir.
- Ucuncu bir kablo (hat) yeryuzune cekilir. bu kabloda cok ufak bir direnc vardir. Eger akim yuksek gelirse akim az direnc olan yerden gitmek isteyeceginden akim topraga gider.
- Toprak notr dur. Uzerine gelen elektrik yeryuzune dagilir. Sonsuz buyuklukte direnc olarak dusunulemez. Gelen elektrik dagilir.

# flip flop
en az bir bitlik veri tutabilen devredir. dolayısı ile önceki girişlerinde çıkışa etkisi vardır. bir çıkışın devrenin içinden farklı bir elemana bağlanması sonucunda böyle bir mantık devresi ortaya koyulabilmektedir. bu şekilde çıkış (yanibir önceki durum), yeni girişlere etki edecektir.

En basit flip flop'lardan biri SR Flip flop'tur. Girişleri S ve R olarak adlandırılır. S girişi set inputumuz, R girişimiz ise reset inputumuzdur. set girişimiz bir önceki durumu değiştirmek için diil, durumu 1'e set etmek için kullanılır. yani çıkışımızı1 yapmak istiyorsak set'i 1 yapmalıyız. ama çıkışımızı sıfırlamak istiyorsak; ancak reset ile sıfırlayabiliriz. eğer reset ve set 1 olursa flip flop patlıyor çünkü mantıksız bir durum. 

JK Flip Flop ise SR flip flopun R=S=1 durumuda patlamaması için yapılmıştır. bu devre S=R=1 durumunda bir önceki çıkışın (durumun) tersini vermektedir.

D flip flop tek girişlidir. D harfi data'nın ilk harfinden gelmektedir. Girişin tersinin JK flip flopa bağlanması ile oluşuturulan bir flip floptur. giriş ne ise çıkış odur. sadece clk değiştiğinde çıkış değişeceği için veriyi saklamış olur.

# clock pulse
kısaltması: clk. bu da devrenin bir girişine bağlı bir inputtur. belirli aralıklarda 1 ve 0 değerini verirler. bu şekilde sadece belli durumlarda devre tetiklenir. örneğin; Eğer flip-flop pozitif kenar tetiklemeli (rising edge triggered) ise, çıkışlar sadece girişlerin pozitif kenardaki değerlerine göre değişir. yani tam olarak; clk sıfırdan 1'e geçerken flip flop yeniden inputlar ile çıkışı hesaplayacaktır.

# elektrik alanı vs manyetik alan vs elektromanyetik alan
elektrik yükü sadece hareket halindeyken kendi etrafında hem "elektrik alanı" (doğal bir enerji çeşidi) yayar hemde "manyetik alan" oluşturur. bu iki alana birlikte "elektromnyetik alan" denir. elektromanyetik alan elektroniların hareket halinde olduğu alandır. elektronların frekansı ve dalga boyuna göre insanlar bunları gruplandırmıştır. örneğin; gama ışınları, X-ışını, morötesi, görünür ışık, kızılötesi, mikrodalga, radyo ve tv dalgaları gibi. Bunun dışında NFC, WI-FI, RFID, temassız akıllı kartlar gibi tüm teknolojiler manyetik alanın farklı dalga boylarındaki protokollerini temsil eder.
Örneğin; çipli kartlarda elektronik devre vardır. Uzaktan elektromanyetik alan yaratılarak elektroniların o devreyi çalıştırması ve sonucun geri alınması sağlanır. BÖylece uzaktan elektrik kaynağı olmayan kart çalıştırılmış olunur.

# elektrik hızı
Elektrik ışık hızına cok yakindir.  Tasindigi ortama gore de hızı degisir. Bu sebeple radyo ve internet gibi kanallar da çok hızlı çalışır. internetin yavaş olmasının sebebleri;
- paralel işlemlerin yazılımlar tarafından sırada bekletilmesi
- elektriği taşındığı kabloda kayba uğraması ve buna bağlı olarak bazı data paketlerinin tekrar sırada beklemesi
- elektronik devreye varan elektriğin farklı formlarda çevrilmesi için geçen süreler
- Bir elektronik chip'e kablodan gelen elektrik ilk transistöre aktarilir. Transistor cok daha duzenli bir bicimde 1 ve 0 lari clock pulse frekansinda devrenin icine yollar. clock pulse için geçen sürelerde kayıptır. Çünkü her pulse belli bir zaman dilimi içinde tanımlanır. Örneğin saniyenin binde biri boyunca gelecek volt değeri 1 yada 0 olarak kabul edilmesi... Clock pulse'lar sadece kabloların cihaza takıldığı girişlerde yoktur. Cihazın kendi içinde her modülün arasında da birimler arası haberleşmede clock pulse'lar kullanılır. hatta her elektronik parçanın kendi içindeki birimlerde clock pulse'lar ile haberleşir.

# Elektromanyetik tayf (yada elektromanyetik spektrum)
spektrum çeşitlilik anlamına gelir. elektromanyetik spektrum elektromanyetik daglaların ve frekansın değişmesi ile yaratılan elektromanyetik alan çeşitlerini temsil eder.

# elektrik vs elektronik
elektrik; yüklerin hareketini fiziksel olarak inceleyen bir bilim dalıdır. çalıştığı bazı konular: statik elektrik, elektrik akımı, elektrik alan, manyetik alan...

elektronik ise; maddenin elektriksel özelliklerini kullanarak digital ortamlarda sinyal işleme üzerine çalışır.

# electricity (yada elektrik) vs Electric
Electric'in tam olarak türkçe karşılığı olmadığı için elektrik olarak kullanılmaktadır. 

electricity, elektriği ifade eder.

electric; ise "electric kettle" gibi onu kullanan sistemlerin önüne koyulan özel bir terimdir. 

__'Electrical'__ ise; 'elektriksel' olarak türkçeye çevrilir ve elektrikle ilgili olan anlamında kullanılır: elektriksel arıza (yada electrical faults)...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# uninterruptible power supply (yada uninterruptible power source yada UPS)
asıl güç kaynağı kesilince devreye giren yedek güç kaynağı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Firmware
- kelime kökü: firm (firma) + ware (mal, eşya)
- Donanım parçası içerisinde bulunan salt okunabilir yazılım. Teorikte sadece salt okunur olması beklenmektedir. fakat pratikte güncelleme yapılabilmesi için yazılabilir olarak tasarlanabilirler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# PATA (yada Parallel ATA yada IDE)
Anakart ile diskler (hdd, dvd/cd gibi) arasındaki veri yolu bağlantı tipidir.

# SATA (yada Serial ATA)
PATA'nın üzerine kurulu daha yeni ve hızlı teknolojidir.

# AHCI (yada Advantec Host Control Interface)
SATA desteği olan anakartlar ile sadece HDD arası veri yolu bağlantı tipidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Original Equipment Manufacturer (yada OEM yada orijinal ürün üreticisi)
Normalde ürünler kutu içerisinde satılır. Fakat toplu alış yapan şirketler için kutular fazla hacim aldığından (maliyeti yükselttiğinden) kutusuz şekilde satılırlar. Bu kutusuz ürünlerin hiçbir farkı yoktur. Kutusuz bu ürünlere "OEM ürün" denir.
OEM'li bir ürün, yada kutulu bir ürün farklı servis/destek/parça ile birlikte satılabiliyor. Örneğin; daha uzun yada daha kısa garanti süreli, destek hizmeti olmadan, ekipman parçaları olmadan gibi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# plc
2 inputu ve 2 outputu olan bir device örneği:

```
  =====I0=====I1======

  ====================

  ===  SIEMENS PLC ===

  ====================

  =====Q0=====Q1======
```

- PLC'nin içine programımızı attığımızda gelen inputlara göre çıktılara 1 ve 0 ları yönlendirecektir. Girişlerdeki clock pulse'a göre her daim yazdığımız kodlar tekrar tekrar işletilecektir.

- Girişimiz birçok input'u olabilir. özellikle bayt girişlerine çok ihtiyaç olduğundan kare olarak dikdörtgen şeklinde girişlerimiz olabilir. örnek: 50x8 lik bir dikdörtgenimiz olabilir. 50 bayt anlamına geliyor. her giriş bir bit'i temsil eder.

- Cihazın girişleri ve çıkışları 24 volt oluyor (çoğunlukla). Bu sebeple direk olarak çıkışımızı güç isteyen bir kaynağa (örnek motora) bağlayamayız.

- PLC'ler özellikle otomasyon sistemlerinde kullanıldığından titreşimlere, darbelere, ve sıcak soğuk gibi hava koşullarına dayanıklı üretilmektedirler. En azından aurdinio gibi cihazlara göre çok daha dayanıklı.

## programlama dilleri

birçok  yötem ile programlama yapılabilir:

- Ladder (yada Merdiven) Diyagramı
Elektrik devreleri bilgisi olanlar tercih eder.

örnek:
```
  -----| I0 |------------| Q1 |----
                |
                |
                |
  -----| I1 |----
```

Yukarıda input0 veya input1'imiz 1 olursa ancak quit1'imiz 1 olacaktır.

Bu gibi birçok programı tanımlayıp, plc'nin içine atabiliyoruz. PLC çalıştığında bunların tümünü sırası ile tekrar tekrar işletecektir.

## Statement List Editor (yada STL)
Programlama dili bilenler tercih eder.

örnek:

```
LD I0
AND
LD I1
```

## Function Block Diagram (yada FBD)
Elektronik (logic kapılar) bilgisi olanlar tercih eder.

örnek:

```
   IO ------------

         |  OR   |-----------Q1

         |       |

   IO ------------
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# sleep vs Hibernate vs OTHERS

- sleep (yada Stand By yada Suspend yada Suspend to RAM yada uyku) ile işletim sistemi tüm uygulamalrı pause eder ve sistemin az enerji harcamasını sağlar. sistem kapanmaz. bu sebeple saniyeler içerisinde devam edebilir.

- Hibernate (yada Suspend to Disk yada hazırda beklet), sleep ile aynı mantıktadır. fakat sistemi tamamiyle kapatabilmek için RAM'deki bilgileri harddiskte saklar. açılması zaman almaktadır.

- Hybrid Sleep (yada Suspend to both) modunda kullanıcı logout edilir. sadece işletim sistemi ve servisleri Hibernate edilir. windows, 8 ve sonrası sürümlerde "shut down" işlemi ile "Hybrid Sleep" uygulamaktadır. gerçek bir sistem kapanması için "restart" işlemi yapılmalıdır. Windows ayalarında "fast startup" (yada Fast Boot) isimli bu özellik devre dışı bırakılabilir.

  Bazı işletim sistemlerinde "hybrit Sleep" aynı işlevi yapmamaktadır. Bazı sistemlerde şu yapılıyor: Sistemdeki tüm uygulamalar pause ediliyor (sleep'te olduğu gibi). RAMdeki veriler hibernate'deki gibi hdd'ye saklanıyor. RAM'deki veriler silinmiyor. aynen bırakılıyor. Yani sleep moduna ekstradan RAM'deki veriler HDD'de de saklanmış oluyor. Bu şekilde; makina aksi bir durumda kapanır ise en kötüsü hibernate tarzı bir açılış yapıyor. Eğer sistem kapatılmamış ise, sleep gibi hılı açılıyor.

- Connected Standby (yada InstantGo) mobile cihazlarda görülen bir yöntemdir. masaüstlerine çok sonradan gelmiştir. sleep ile aynı mantıktadır. tek farkı, ek olarak; isteyen uygulamalar internet ağını kullanacak kod bloklarını sürekli arkaplanda çalıştırabilmeleridir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# özet boot süreci

- bios (eski süreç)

> bios ----(hdd)----> mbr ----> bootloader ----> os

- uefi sonrası yeni süreç:

> UEFI ----(hdd)----> bootloader ----> os

- uefi sonrası, BIOS uyumlu kurulmuş bir disk üzerinde işletim sistemini başlatma:

> UEFI ----(hdd)----> mbr ----> bootloader ----> os


# Uyku ve diğer kapanma metodları sonrası boot süreçleri nasıl değişiyor

Uyku'ya alınan cihazın açılması firmware'den itibaren olmuyor. Sistem hiç kapanmamış olduğu için direk olarak işletim sistemi devam ediyor.

Hibernate işleminde işletim sistemi normal kapanma metodunu uyguuyor. Açılıştada normal açılma süreci işleniyor. İşletim sistemleri otomatik olarak son durumunu algılayıp, hibernate edilmiş ise dosyadan RAM'e aktarım yapıp devam ediyor. Eğer hibernat edilmemiş ise; normal olarak açılıyor. Boot sürecinde bir değişiklik söz konusu değil.

# MacOS boot işlemi
Apple, EFI'yi ilk kullanan şirketlerden biri. 2005 yılında, Apple Intel işlemcilere geçiş yaptığında klasik BIOS yerine EFI kullanmayı tercih etmişti. Yıllar sonra, UEFI spesifikasyonu sayesinde tüm PC'lerde UEFI kullanılmaya başlandı ve bu bir standart haline geldi. Mac'lerde şu an Apple'ın kendine göre uyarladığı bir EFI bulunmakta. Mac'lerdeki firmware tıpkı normal PC'lerde olduğu gibi UEFI ve Legacy modda işletim sistemi boot edebilir. Ancak Apple'ın yaptığı düzenlemeler olmadan macOS önyüklenemez.

# CSM (yada Compatibility Support Module)
UEFI'nin 'BIOS compatibility mode'una verilen özel isim. UEFI devre dışı bırakılamaz. UEFI MBR-tarzı bölümlü HDD'lerden işletim sistemi açabilir. BU da UEFI'nin geriye uyumlu çalıştığı anlamına gelir. Fakat CSM, her UEFIde olmak zorunda değildir.


# BIOS
- Bir firmware'dir.
- Bios anakartin üstündedir.
- Makina baslatildiginda hangi aygıttan (cd, dvd, hdd, usb gibi) boot edileceğini belirler.
- HDD ile baslatilirsa; HDD'nin ilk sektörünü okur. Burada okuduğu kismi execute eder. Execute edilen bu bölge MBR (Master boot record) olarak adlandirilir.

# MBR
iki kisimdan olusur:
- Master Boot Code --> execute edilebilir olan kisim.
- Master Partition Table --> hdd için bölümlerin haritasi. en çok 4 bölüm tutabilir. bu sebeple 4 bölümden (birincil bölüm veya primary partition) fazlaya ayirmak gerekirse, hardiskki "genisletilmis bölüm (extensable partititon)" özelliğinden yararlanılır. genisletilmis bölüm en fazla 1 adet olabilir. fakat altinda sinirsiz bölüm barindirabilir. altinda olan her bölüme "mantiksal bölüm (logical partititon)" denir.

# bootloader
- mbr tarafindan execute edilebilen bir sistemdir. bu sistem ile isletim sistemleri baslatilir.

- her isletim sisteminin bootloader dosyalarını bir yerde saklaması gereklidirki MBR o dizini okusun ve execute etsin.

- bu dizin ubuntuda varsayılan olarak /boot/grub'tur. bu dizinin ubuntu'da, baslatilacak islemim sistemi ile ayni bölümde olma sarti yoktur. örnegin /boot/grub, /dev/sda9'da iken, ubuntunun tüm dosyalari /dev/sda5'te olabilir. eger bölümleri ayirirsak, ubuntu'yu sildigimizde grub sistemi de silinmeyecegi için HDD'de kurulu diğer işletim sistemleri hala basslatilabilir olacaktir. fakat grub, ubuntu ile ayni partititon'da olursa, ubuntu partititon'u silindiginde (yada formatlandığında), grup baslamayacagi için HDD'de kurulu diğer işletim sistemleri de baslamayacaktir.

- Windows'un bootloader'ı "Windows Boot Manager (BOOTMGR)" olarak isimlendirilmektedir. Dosyaları; "System Reserved" isimli partititon'da durmaktadır.

- herhangi bir işletim sistemi HDD'ye kuruldugunda, mbr dizini üzerinde degisiklik yapar. bu degisiklikler:

  - mbr içine hangi bölümün master boot (active) oldugu yazilir. yani; hangi bölümdeki bootloader'in baslatilacaginin adresidir. (her partititon altinda bootloader olabilir, ama sadece 1 tanesi baslayacaktir). işletim sistemi kurulumunda yeni bir bootloader kurulmayacak ise bu işlemin yapılmasına gerek yoktur.

  - grub normal koşullarda mbr alanına kurulur. fakat mbr alanı çok küçük olduğundan, grub kendini 2 parçaya böler. bir kısmı mbr içinde, 2inci kısmı farklı bir (boot flag'inin olduğu) partition'a saklanır. 2-stage bootloader kavramı burada ortaya çıkmıştır. normal koşullarda mbr'nin içindeki executor, ext4 dizinini okuyamaz. ubuntuda grub dosyaları ext bölümünde dururur. mbr'nin grubu çalıştırması için ext dosys sistemini tanıması lazım. işte bu ext-tanıma kısmı modülü 1inci stage bootloader kısmı ile çözülmüş oluyor.

- windows, her kurulumdan sonra sürekli mbr'yi kendi bootloader'ına yönlendirir. windows'un bootloader'ı diğer işletim sistemlerini zaten tanımaz. bu sebeple her windows kurulumu sonrası, liux ve diğer işletim sistemler ile grub'u tekrar kurmak gerekmektedir.

# fixmbr
windowsun komut satırı uygulaması. bu komut ile mbr tamamen sıfırdan değiştirilmektedir. fixmbr komutu sonrası windows kendi bootloader'ına yönlendirme yapacak şekilde mbr'yi yeniden yazmaktadır.

# ntldr
Windows NT için bootloader'dır. günümüzde tamamen yeniden tasarlandığı için artık kullanılmamaktadır. Yeni tasarım Windows Boot Manager (BOOTMGR) üzerinedir. BOOTMGR başladığında eski bir windows sürümü seçilirse, BOOTMGR, ntldr'yi başlatmaktadır.

# GRUB (yada GRand Unified Bootloader) vs GRUB2
Grub yazılımı 2inci sürümünde neredeyse sıfırdan tasarlandı. Bu yüzden çoğu yerde farklı yazılımmış gibi algılanmaktadır. Oysa Grub2, Grub'un güncel sürümüdür. Grub'un eski sürümüne "GRUB Legacy" adı da verilir. "Grub legacy" sadece bios-uyumlu bölümlendirmeleri destekler.

# SYSLINUX
ntfs veya fat32 dosya sistemlerine kurulu linux başlatmak için gerekli bir bootloader çeşididir.

# LILO (yada LInux LOader)
Bir BIOS uyumlu bootloader çeşididir. Daha sonra UEFI'ler için ELILO isminde bir türevi çıkarılmıştır.

# OS-Loader
bootloader, ismi gereği, genel bir terimdir. mbr'nin içindeki sistemde, mbr'nin başlattığı sistemede aslında bootloader denilebilir. çünkü sistemi boot/load ediyorlar. bu sebeple bazı insanlar, günümüzdeki sistemelere "2 stage bootloader" olarak varsayıyor. 1'inci stage: MBR bootloader'ının execution'u. 2'inci stage ise: GRUB, BOOTMGR gibi yazılımların execution'udur. dolayısı ile bir makalede; mbr'ye bootloader yazınca, 2inci aşamaya OS-Loader diyen kaynaklar olabiliyor.

# UEFI (yada Unified Extensible Firmware Interface yada eski isim: EFI)
Yeni gelen bir sistemdir. BIOS'a alternatif bir firmwaredir. UEFI her konuda çok daha gelişmiştir diyebiliriz. Sadece süreç olarak bakıldığında; MBR gereksinimi ortadan kalkmıştır. Bakınız: "özet boot süreci".

Bazı UEFI Arayüzlerinde "BIOS" kelimesi görülebilmektedir. Bunun sebebi BIOS tabanlı sistemler üzerinde ekstra modül olarak UEFI kullanılmasıdır.

# UEFI boot manager
Bağlı olan tüm diskler içindeki tüm başlatılabilir sistemlerin (işletim sistemi, bootloader, mbr) listesini tutar ve bunlara ekleme/çıkarma/değişiklik yapılmasını sağlar. kısmen Grub'un görevini görmektedir diyebiliriz. listenin veritabanı anakartın içindedir. Bu listedeki öncelik sırasına göre, anakartı üzerinde olan UEFI ilgili sistemi başlatacaktır.

Bu liste dışardan editlenebilmektedir. Örneğin Linux'ta "efibootmgr" uygulaması bunu yapıyor. Her işletim sistemi, kurulumunda bu listeye ekler yada siler yada editler. Aynı şekilde anakarttaki UEFI yazılımı GUI'si ile son kullanıcı manuel de ekleme/silme/değiştirme yapabilir.

Listenin içinde BIOS-uyumlu bölümlerde olabilir. Çünkü UEFI, geriye uyumlu; yani; BIOS uyumludur.

örnek bir "efibootmgr" komut satırı çıktısı (her satırın açıklaması aşağıdasında yazıyor):

> BootCurrent: 0002 

şu anda OS'u başlatmak için kullanılmış olan boot

> Timeout: 3 seconds

aşağıdaki liste pc açıldığında görüntülenir. 

> BootOrder: 0003,0002,0000,0004

boot deneme sırasını temsil eder. biri açılmazsa diğerini açmaya çalışır.

> Boot0000# CD/DVD Drive  BIOS(3,0,00)

BIOS kelimesi, eski bios uyumlu açılışı temsil eder.

\# (yıldız karakteri) bu kaydın aktif olduğunu belirtiyor

> Boot0001# Hard Drive    HD(2,0,00)

hdd'yi aç anlamına geliyor. özellikle partititon belirtmemiş. bu durumda UEFI native mod'da HDD yi açar. GPT bölümlü olan bu HDD içerisinde, UEFI partititonunu bulur. "Fallback path mode (yada fallback mode)" denir buna. yani; native uefi mode (bios değil) fakat hdd karar veriyor hangi partititonun açılacağına. detaylar için ESP ile ilgili başlıklarına bakınız.

> Boot0002# Fedora        HD(1,800,61800,cb3e-4727-8fed-5ce0c040365d)File(\EFI\fedora\grubx64.efi)

uefi native mode partititon belirtilmiş. ".efi" uzantısı UEFI firmware'nin execute edilebilir bootloader dosyası uzantısıdır.

> Boot0003# opensuse      HD(1,800,61800,cb3e-4727-8fed-5ce0c040365d)File(\EFI\opensuse\grubx64.efi)

>  Boot0004# Hard Drive    BIOS(2,0,00)P0: ST1500DM003-9YN16G

hdd yi aç der. özellikle partititon belirtmemiş. fakat BIOS uyumlu olsun demiş. bu durumda HDD içerisinden MBR yi okur eskisi gibi. Yukarıdaki Boot0001'da, UEFI geri uyumlu modda olmadığı için GPT'yi okumuştu. Burada ise MBR okunacak.

Bazı bilgisayarlarda efibootmgr ile yapılan işlemlerde hata almasakta, yapılan işlemin etki etmediğini görebiliriz. bunun sebebi boot option'larının cihaz tarafından hardcode tanımlanmış olmasıdır. böyle durumlarda; bootorder'ı takip edip, /EFI altında istenmeyen dizinlerin önüne saçma prefixler verilebilir. yukarıdaki örnekten gidelim: bootorder 0003 opensuse'yi açacak. eğer opensuse'yi istemiyorsak ve efibootmgr yazılımı etki etmiyor ise; /EFI/opensuse dizinini silebiliriz yada /EFI/\_opensuse olarak rename ederiz. bu şekilde yedeğini almış oluruz.

# Secure boot
- UEFI ile gelen bir özellik. manuel olarak devre disi birakilabilmektedir. açılacak olan işletim sisteminin bootloader'ının anahtarının, UEFI içerisindeki anahtarlarla uyuşması gerekiyor.

- Bu sistemle rastgele bir kullanıcının USB'den istediği gibi işletim sistemini çalıştırması engellenmiş oluyor.

- Makine ile kurulu gelen işletim sistemlerinin anahtarı UEFI içerisine kaydedilmiş oluyor.

- Microsoft, windows'un anahtarını bazı farklı işletim sistemleri firmalarına da veriyor (örneğin Fedora). BU şekilde o makinada, güncel fedora ISO'su ile direk işletim sistemi başlatılabiliyor oluyor.

# UEFI based botloaders
UEFI firmware, direk olark bootloader'ı çalıştırır. BIOS tabanlı sistemlerde ise, MBR bootloader'ı çalıştırırdı. UEFI çok daha gelişmiş oluğundan execute ettiği bootloaderda daha gelişmiştir. Farklı executable dosyası çalıştırırlar. Bu sebeple bootloader'larda kendilerini güncellemişlerdir. Eğer HDD içerisinde sadece UEFI'nin execute edebileceği bootloader var ise; aynı HDD BIOS'lu anakartlarda işletilemeyecektir.

# Volume Boot Record (yada VBR yada volume boot sector yada partition boot record yada partition boot sector)

mbr çok ufak oldugundan, tüm detaylari üzerinde tutmak mümkün diildir. bu sebeple her mantiksal ve birincil bölümün basinda VBR olur. vbr'ler de mbr'ler gibidir. bölüm bilgilerini daha detayli sekilde tutarlar.

# GUID Partition Table (yada GPT)

- UEFI'nin tanıdığı, mbr'ye göre çok daha gelismis bir bölüm tutma semasidir.

- eski bios-mbr uyumlu bölümlendirmelere "msdos partition table" adı verilir.

- GPT bilgileri diskin en başındadır. GPT olan bir HDD'de MBR'de vardır. MBR bölümü her zaman bırakılır. Bu şekilde geriye uyumluluk sağlanmış olur.

- GPT olarak tasarlanmış bir diski makineye taktığınızda BIOS anakartlar ile buradaki işletim sistemlerini başlatamazsınız. Fakat burada yanlış anlaşılma olmasın: Sadece işletim sistemi boot edemezsiniz. Onun dışında, bir işletim sistemi yürütülür durumdayken, GPT bölünlenmiş HDD'yi mount edip, hem windows hemde diğer tüm işletim sistemleri ile okuyup yazabilirsiniz (default olarak işletim sistemi okuyamasa bile üçüncü parti uygulamalarla yapılabilir). Çünkü HDD'yi okuyan bir yazılımdır. BIOS değil. Yani işletim sistemi boot etmekle, HDD'yi sadece data olarak kullanmak tamamen ayrı kavramlardır.

# Windows recovery partititon
- Bu bölüm windows yüklemesi sırasında otomatik oluşturuluyor.

- Bölüm winRE dosyalarını içeriyor. WinRE windows kurtarma işlemlerini yapan bir yazılım.

# ESP (yada EFI System Partition yada EFISYS)
-Diskte varolan işletim sistemlerinin EFI dosyaları (bootloader'ları) + Bootloader için gerekli sistem sürücüleri (driver) dosyaları bu bölümdedir.

- FAT32 olmak zorundadır. FAT16 olması UEFI implemementasonunda yazmaktadır. Fakat Windows işletim sistemi fat16 olması durumunda sorun çıkardığı için, artık her yerde FAT32 kullanılmaktadır.

- boyutu değişkendir. Örnek boyut: Ortalama 40 mb sadece windows-10 ve ubuntu yüklü bir makine için yeterlidir. Fakat İlerde yapılabilecek ek işletim sistemi kurulumlarının EFI dosyaları için bu bölümün boyutunu yüksek tutmakta yarar var.

- \EFI dizini altında her işletim sisteminin kendi bootloader'ı ayrı dizinlerde mevcuttur. \EFI\BOOT dizini HDD "fallback mode" ile yürütüldüğünde devreye girer. Yani; UEFI, hiçbir partition belirtmeden HDD'yi boot ettiğinde, UEFI, ESP bölümünü bulur. Daha sonra içerisinde \EFI\BOOT\BOOTx64.EFI (yada standrat önceden belirlenmiş farklı bootloader path'leri) dosyasını execute eder. Yani işletim sistemlerinin bootloader'ları dışında genel bir UEFI arayüzü (bootloader'ı) mevcuttur. Bu bootloader "default bootloader" olarak da adlandırılmaktadır. Bazı UEFI firmware'leri bu bootloader'ı bulamadığı zaman windowsun default bootloader'ını da açmayı denemektedir: EFI/Microsoft/BOOT/bootmgfw.efi.

- \EFI dizini altında sürücüler ve işletim sistemi çekirdekleri de mevcuttur. EFI dosyası executable'ı olarak linux çekirdeği çalıştırılabiliyor. Linux güncel sürümleri bunu desteklemektedir. BU özellik; EFI Boot Stub (yada EFI Stub) olarak isimlendiriliyor.

- \EFI altında aynı zamanda sürücü (driver) dosyaları bulunmaktadır. Sürücüler: dosya dizinlerine erişim sürücüleri (ext, ntfs gibi), network cardlar'ın daha geniş özelliklerle kullanılabilmesini, bootloader sırasında takılan exernal usb cihazların tanınması gibi işler için gerekli olabilmektedirler.

# MSR (yada Microsoft System Reserved Partition yada Sistem ayrıldı bölümü)
- sadece GPT disklerde oluşan bir bölümdür.

- ESP'den sonra ve windows kurulu bölümden önce olmak zorundadır.

- windows sürümüne göre dosya sistemi formatı ntfs yada fat32 olabilir.

- Boyutu windows sürümü ve disk boyutuna göre değişebilir.

- bootmgr bu bölümde yüklüdür.

# Redundant Array of Inexpensive Disks (yada Redundant Array of Independent Disks yada RAID)
- Birden fazla diski tek bir disk gibi gösterme özelliğidir.

- RAID kendi içeirisinde birçok özelliği vardır: RAID0, RAID1 gibi... Bunlardan sadece bir tanesi sçeilmek zorundadır.

  - RAID0: 2 diskimiz var. 2 disk tek bir disk gibi gösteriliyor. 2 tarafta da farklı datalar tutuluyor. performans artışı sağlıyor. çünkü bazı durumlarda iki diskten ayrı ayrı dosyaları aynı anda okuyabiliyor. tek diskte olsaydık aynı anda bir dosya okuyabilecektir.

  - RAID1: 2 diskimiz var. 2 disk tek bir disk gibi gösteriliyor. 2 tarafta da aynı datalar tutuluyor. Bu eşkilde birine bir zarar geldiğinde diğerinden devam edilebiliyor. Performans açısından kötü.

- Sadece Donnanımsal yada sadece yazılımsal çözümlerle sağlanmaktadırlar.

# Flag (Bayrak)

GPT ve MBR her partitition için bir bayrak bulundururlar. Bayrakların birer anlamı vardır.

- boot: tüm hdd de sadece bir bölümde bu set edilebilir. ilk o bölümün boot edileceği anlamına gelir. MBR sistemlerde çalıştırılacak bootloader'ın olduğu bölüm iken, GPT istemlerde EFI kurulu bölüme atanmalıdır.

- Diag: o bölümün diagnostics/recovery amaçlı kullanıldığını belirtir.

- Hidden: microsoft otomatik mount edilmesini istenmediği bölümlere bu bayrağı atıyor.

- RAID: raid teknoloji için kullanılan bir bölüm olduğunu gösterir

- Msftres: sadece gpt'de disklerin içindeki bölümlerde mevcuttur. bölümün "Microsoft Reserved partition" olduğunu gösterir.

- msftdata: microsoft kendi oluşturuduğu dizinlere bunu atıyor. bazı linux sistemleri de kendi dizinlerine bu flag'i attığı görülüyor.

- swap: hdd üzerinde kurulu tüm linux sistemler bu bölümü swap alanı olarak kullanabileceklerini belirtir.

- BIOS_GRUB: GPT Kurulu bir HDD'de, sadece "grub legacy"'nin kurulu olduğu bir bölüm var ise, o bölüme bu bayrak atanır.

- legacy_boot:  GPT yapılı disklerde, SYSLINUX botloader'ın bulunduğu bölüme atanmaktadır.

# Partititon name vs partititon label
- name, GPT alanında olan bir etikettir.

- label ise partititon'un içinde olan bir etikettir.

# Gparted
- Linux üzerinde çalışan, GUI içeren bir disk yönetim aracıdır. gparted komut satırı "(GNU) parted" uygulamasının önyüzüdür.

# fdisk
parted uygulaması ile aynı görevi gören komut satırı uygulaması.

# Gparted anahtar icon'u
- her partition'un yanında bu icon olabilir. bu icon o partititon'un mount edilmiş olduğu ve bu sebeple üzerinde işlem yapılmasına izin vermeyeceği anlamına gelir.

- anahtar işaretli bir bölüm üzerinde işlem yapmak için o bölüm önce unmount edilmelidir. eğer ilgili bölüm, çalıştırılan işletim sistemi dosyalarını içerior ise unmount edilemez. böyle bir durumda ilgili bölüm ancak canlı cd üzerinden, yada aynı HDD üzerinde  bir bölümdeki işletim sistemi çalıştırılarak yapılabilir.

# bootable usb creators

| name                                           | runs on              | allowed iso types | note                                                                |
|------------------------------------------------|----------------------|-------------------|---------------------------------------------------------------------|
| unetbootin                                     | cross-paltform       | linux-based       | open source                                                         |
| rufus                                          | windows              | any               | open source                                                         |
| WinToUSB                                       | windows              | windows           | closed source                                                       |
| WinUSB                                         | ?                    | windows           | fork fake app                                                       |
| WoeUSB                                         | linux                | windows           | open source                                                         |
| mkusb                                          | linux (command line) | linux             | ?                                                                   |
| Etcher                                         | any                  | any               | (read below)                                                        |
| LinuxLive USB Creator                          | windows              | linux             | writes open source but code is not available                        |
| YUMI (yada Your Universal Multiboot Installer) | windows              | any               | multi iso on one usb feature. source code is available on zip file. |
| UUI (yada Universal USB Installer)             | windows              | any               | source code is available on zip file.                               |
| USB-creator (yada Startup Disk Creator)        | Ubuntu,windows       | Ubuntu            | ubuntu pre-installed app                                            |
| Windows USB/DVD Download Tool                  | windows              | windows           | ms official app                                                     |
| multibootusb                                   | windows, linux       | any               | open source                                                         |

# etcher vs balena-etcher
etcher isim değişikliği ile balena-etcher (yada balenaEtcher) olmuştur.
etcher electron üzerine kurulu, ISO dosyasını tümüyle byte byte USB medyaya basmaktadır ve distro-spesifik configürasyonlara izin vermemektedir. bu sebeple ms-windows gibi ISO'larda başarı sağlayamamaktadır.

# Windows To Go
windows 8 ile gelen bir özellik. sadece enterprise edition'larda mevcut. artık windows usb'ye de kurulabiliyor. bazı usb cihazları bunu destekleyebiliyor.

# remastersys
desteği biten bir yazılım. çalışmakta olan linux sistemini, o haliyle iso haline getirmeye yarıyor.

# mount point
bir partition işletim sisteminde bir dizinden erişilmek zorundadır. örneğin windowsta her partititon en tepedeki dizinden ayrılır. oysa posix'lerin tümünde istenilen dizinden istenilen partitiona erişim sağlanır. bu bağlama işlemine "mounting" adı verilir ve bağlanan dizin adresine de "mounting point" adı verilir.

# logical drive (yada volume) 
bilişimde kullanılan "volume" kelimesinin türkçesi yok.

volume şu anlama gelmektedir: hacim, yoğunluk, yığın. bunlardan bağımsız olarak "ses kuvveti" anlamına da gelmektedir.

bilişimde volume birden fazla partition'u barındırabilen özel bir bellek alanını temsil etmektedir.

# drive (yada sürücü)
fiziksel bellek alanını temsil eder. örnek: hdd, usb, floppy, cd.

# partition (yada bölüm)
her partition sanaldır.

# linux boot parameters
grup ekranında linux çekirdeği başlamadan önce ona parametre geçebiliriz. örnek parametreler:

- acpi=off : ACPI yönetimini devre dışı bırak

- noapic  : APIC desteğini devre dışı bırak

- nolapic  : Yerel APIC desteğini devre dışı bırak

- edd=on  : EDD desteğini etkin kıl

- nodmraid : Yazılımsal RAID özelliğini devre dışı bırak

- quiet: linux'un output'a loglamamasını sağlar. quiet disable bırkılırsa, linux output'a log'ları basar.

- splash: linux boot olurken ekranda logo görünmesini sağlar. eğer splash açık ise quiet'ın önemi olmayacktır. quiet vs splash birlikte kapatılmalıdır ki loglar ekranda gözüksün.

- nomodeset: ekran kartları ile ilgili sorunlar yaşandığında kullanılan bir seçenektir. sistem başlayana kadar (X (pencere yöneticisi) başlayana kadar), video sürücülerinin devre dışı bırakıp kernel'deki video modunun kullanılmasını sağlar. nomodeset; __kernel mode setting (yada KMS)__yi disable eder.

Bu seçeneklerden birini uygulamak için:

- grup ekranında linuxu başlatmak için seçeceğimiz satır'da enter tuşuna basmadan önce "e" tuşuna basarız. o satırı düzenlemeyi açar. 

> linux	/boot/vmlinuz-4.15.0-45-generic root=UUID=86e0d3db ro  quiet splash $vt_handoff

yukarıdaki satırın sonuna istediğimiz parametreyi ekleyebiliriz ve F10 ile başlarız. bu düzenleme sadece o oturuma mahsus olacaktır. bu işlemi kalıcı hale getirmek için:

- /boot/grub/grupcfg dosyamızı düzenlememiz gerekmektedir. fakat grupcfg dosyası her linux update'i sonrası, yeniden oluşturuluyor. bu sebeple bu dosya kalıcı bir çözüm diil.
- /etc/default/grub dosyasının düzenlenmesi gerekli. her linux kernel update'i sonrası, bu dosyadaki config'ler okunarak, grupcfg dosyası oluşturuluyor.

# update-grub vs "grub-mkconfig -o /boot/grub/grub.cfg"
ikiside tamamen aynı görevi görüyor. update-grub sadece bir wrapper.

grub menü dosyasını default configurasyonlara göre re-generate ediyor. 

# update-grub vs grub-install
update-grub sadece menü dosyasını tekrar oluştururken, grub-install tüm grub sistemini sisteme yüklüyor.

# update-grub vs update-grub2
bu 2 komut birbirine link ediyor. grub2 çıkınca  update-grub2 komutu çıktı, fakat update-grub her 2 grub sürümünü de update edebiliyor.

# Nouveau
name of open source nvidia driver