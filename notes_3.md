############################
# NOTES 3 FILE
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
# OTHERS
############################

############################

# dry run
A dry run testing is a process which is run is a controlled conditions to minimize the negative effects of any product failure.

örnek: iki dizini sync yapan bir uygulama olsun. sync işlemi riskli. bu sebeple öncesinde dry run koşulur. dry run ile sync uygulaması önce bir dry işlem yürütür. bu işlem gerçek kopyalama işlemi yaparmış gibi ilerler, tek farkı gerçekten kopyalamamasıdır.

dry run resmi bir tanım değildir. bu sebeple, dry run işleminin tam olarak ne yapacağı uygulamanın tasarımcılarına kalmıştır. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# API first
"database first" gibi bir yaklaşımdır. önce API hazırlanır, daha sonra bunun arka tarafı yazılmaya başlamnır. Bu şekilde testçiler, önyüzcüler de geliştirmeye daha sorunsuzca başlar. örneğin swagger/openapi ile hazırlayacağımız yml dosyasını herkesle paylaşırsak, herkes geliştirmeye paralelden başlar ve birbirini beklemek zorunda kalmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# subset (yada altküme)
subset'in tersi: __superset__

if A is subset of B; then any program/code written in A, is valid for B. subset/superset consept is only about language syntax.

TypeScript is a superset of JavaScript that compiles to plain JavaScript. fakat; Her typescript kodu, js codunu deryelebilecek diye bir kaide yok. bunun birçok sebebi olabilir. örnek: syntax uyabilir ama native primitive objeler uymak zorunda diil.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# binary blob
blob kelime anlamı: damla

context'e göre farklı anlamları var:

-  açık kaynaklı bir OS çekirdeğinde; proprietary device driver (closed-source device driver)'ları için kullanılan bir terimdir. buradan yola çıkan bazı insanlar, daha sonra binary blob terimini açık kaynaklı olan bir yazılımın içindeki kapalı kod binary'leri içinde kullanmaya başlamışlardır. 

- database sistemlerinde; __Binary Large OBject (BLOB)__ olarak kullanılıyor. bu objeler media objeleri gibi herhangi bir binary dosyaları olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# kernel space vs user space (yada userland)
tüm user (root user'ı dahil) process'lerinin harcadığı memory bölgesi userspace'tir. kernel space ise, system call'lar yapıldığı sırada, system call'ların kendi içinde kullandığı ve OS çekirdeğinin kullandığı memory'dir.

# DMA (yada direct memory access)
Her işlem için gerekli data CPU ya taşınmalıdır. kopyalama işlemleri de buna dahildir. fakat donanımlar özel olarak buna bir çözüm getirmişlerdir. donanımlar artık bir io'nun diğer io'ya direk data taşımasına izin vermektedir. bu duruma DMA denir.

DMA yapabilmek için hem donanımın desteklemesi, hemde OS ve DMA işlemi yapmaya çalışan yazılımın kodunun buna göre yazılmış olması gerekmektedir.

işlem başlatılması ve sonlanması gibi durumlarda CPU'ya gidecek fakat her data byte'ı tek tek CPU'ya taşınmaya gerek kalmayacak.

DMA işlemi aynı I/O cihazından aynı I/O cihazına da yapılabilir.

*nix'lerdeki read() system call'u arkaplanda datayı direk belleğe CPU'ya gitmeden taşımaktadır.

# zero-copy
user space'e taşımaya gerek kalmadan yapılan dosya kopyalama işlemidir.

Bunun implementasyonu OS'a göre değişebilmektedir. zero-copy için OS'a system call yapılır. systemcall'un nasıl çalıştığı yazılımcı tarafından bilinmez. sadece dosya kopyalama işlemini temsil eder.

- normal for döngüsü ile dosya kopyalama

  normal bir for-loop ile okusaydık önce system call ile birkaç byte okuyacaktık (*nix sistemlerde read() system call'u). kernel space'e taşınan byte, user space'e yollanacaktı. oradan da write ile user space'den önce kernel space, oradan da I/O'ya yollanacaktı.

  *nix'teki "read()" system call'unu işlemi çağrıldığında, öncelikli olarak DMA üzerinden işlem yapılıp bu data direk olarak kernel'a (read fonksiyonunun içine) taşınmaktadır. read fonksiyonu da user space'e taşımaktadır.

- zero-copy ile kopyalama:

  data, kernel space'e taşınır (fakat user space'e taşımaz). direk kernel space'den I/O ya yollanır.
  
  Burada unutmamak lazım ki; o işlemde kullanılan kernel space (belki) CPU'nun içindeki (RAM'den daha hızlı olan) belleklerde olabilir. bu durumda RAM'e hiç gidilmemiş olacaktır. bu da daha hızlı kopyalama anlamına gelecektir.

  Eğer OS implementasyonu daha iyi ayarlanmış ise; dosya kopyalama için belki de hiç RAM kullanılmayacak, direk DMA kullanarak harddisk'ten harddisk'e kopyalama yapacaktır.

  Yukarıdakilerden hangisinin olacağı OS'un implementasyonuna ve donanımın desteğine bağlıdır.

java'da NIO ile bu kopyalama işlemini yapabiliriz:

```java
RandomAccessFile fromFile = new RandomAccessFile("fromFile.txt", "rw");
FileChannel      fromChannel = fromFile.getChannel();

RandomAccessFile toFile = new RandomAccessFile("toFile.txt", "rw");
FileChannel      toChannel = toFile.getChannel();

long position = 0;
long count    = fromChannel.size();

toChannel.transferFrom(fromChannel, position, count); //returns the number of bytes that actually transferred 
```

*nix sistemlerde C'deki sys/socket.h içindeki sendfile() system call'u bunu yapmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# lua
Portekizce'de 'ay' anlamına gelmektedir.

- lua dili, shell diline çok benziyor.

- betik dili olduundan, sanal makine gerektiriyor.

- fonksiyonların return değerlerinde birden faza değer dönebilmeyi destekliyor.

```lua
function triple(x)
    return x, x, x
end

local a, b, c = triple(5)
```

- lua içerisinde standart olan, "table" tarzı primitive gibi kullanılabilen yapılar mevcut.

- lua comment syntax

```lua
--[==[
  comment line 1
  We can include "]=]" inside this comment
--]==]

-- this is single line comment
```

# C API

- özellikle C ile interoperability'si çok başarılı olduğundan C ile (yada herhangi bir dille) yazılmış uygulamaların bir kısmı yada eklentileri Lua ile yazılmaktadır.
  
  bahsedilen interoperability Lua VM'inin yönetiminine izin veriyor. Lua ekibi, C için bir kütüphane yazmış. Bu c kütüphanesini import edip, bu kütüpohane aracılığı ile Lua VM'i runtime'da açıp, üzerinde lua kodu çalıştırabilir, VM üzerindeki variable'larda değişiklik yapabilir.

  ```c
  #include <stdlib.h>

  // Lua C kütüphaneleri
  #include <lauxlib.h>
  #include <lua.h>
  #include <lualib.h>

  int main(void)
  {
      // open Lua VM
      lua_State *lvm_hnd = lua_open();
      
      // load libraries inside VM
      luaL_openlibs(lvm_hnd);

      // Load a standard Lua function from global table:
      lua_getglobal(lvm_hnd, "print");

      // Push an argument onto Lua C API stack:
      lua_pushstring(lvm_hnd, "Hello");

      // Call Lua function with 1 argument and 0 results:
      lua_call(lvm_hnd, 1, 0);

      // close VM
      lua_close(lvm_hnd);

      return 0;
  }
  ```

- Sanal makinesi çok ufak boyutta ve az bellek tüketimi yapıyor. Sanal makine C ile yazılmış. Bu sebeple C ile programlanan gömülü sistemlerde Lua da kolayca kullanılabilmektedir. Zira Lua sanal makinesi de C olduğudan, direk execute edilebilir ortam yakalanmış olur. Lua'nın gömülü sistemlerde tercih edilmesinin sebebi de budur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# httpbin
httpbin test amaçlı kullanılan, hazır http metodları sunan sunucu yazılımlara verilen genel bir prefix/suffix'tir. birçok httpbin servisi vardır:

- www.github.com/postmanlabs/httpbin
- www.github.com/kevin1024/pytest-httpbin

httpbin'lerin bazı hazır metodları:
- requestin herhangi bir header'ını dönen metodlar (örnek user agent)
- echo servisleri
- resim dönen servisler
- tüm bu servislerin get, post, put... kombinasyonları

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# gRPC (yada gRPC Remote Procedure Calls)
google tarafından geliştirilen bir ptotokoldür. arkaplanda HTTP/2 ve __Protocol Buffers (yada Protobuf)__ kullanılır.

supports: authentication, bidirectional streaming and flow control, blocking or nonblocking bindings, cancellation, timeouts.

gRPC sadece prtotobuf ile binary desteklesede, external olarak kullanacağımız kütüphanelerle json, xml gibi diğer formatlarıda desteklyebiliyor. kaynak: https://grpc.io/faq/ (archive date: 19/10/2019)

# __Protocol Buffers (yada Protobuf)__
binary'ye serileştirme yöntemidir. google tarafından geliştiriliyor. xml, json gibi human-readable çıktı yapmamaması dezavantaj, fakat performans ve memory konusunda çok daha verimli.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Kernel panic
*nix sistemlerde kullanılan bir terimdir. işletim sisteminin, çözümü olmayan durumu yakaladığında bilinçli olarak fırlattığı bir hatadır. windows'ta bu hata tipine __stop error (yada blue screen of death yada blue screen yada BSoD)__ denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# c# namespace
c# taki namespace'ler, java'daki package'lere denk gelir.

```c#
using Other.Package.Name;

namespace namespace_name {
   // classes
}

// inner namespace
namespace N1
{
    namespace N2
    {
        class A {}
        class B {}
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# location transparency
bir kaynağın erişim adresini (örneğin network ise ip'si, dosya istemi ise dosya adresi, mikroservisler ise servis'e erişmek için ip'si) bilmek zorunda kalmadığımız, onun yerine o kaynağa gidebileceğimiz sanal bir adres verilmesi durumudur.

örnekler:

- bir makinen gerçek ip'sini vermek istemiyorsa, o makineye bağlanan kişilere farklı bir ip veriririz ve yönlendirme yaparız. kişiler gerçek ip'yi bilmez.

- docker servislerimiz ayakta olsun. birbirinin ip'lerini bilmiyorlar. fakat birbirlerine hostname'ler aracılığı ile erişiyorlar. kimse kimin nerde olduğunu bulmak için ekstra efor harcamıyor. böyle bir durumda docker, sağladığı ortamda location transparency vardır diyebiliriz.

- kafka, yada axon'da event başlattığımızda, event'i başlatan uygulama event'i kimin çekeceğini bilmez. routing işlemini kafka veya event'i yöneten mekanizma yapar. bu durumda; axon veya kafka location transparency sağlar anlamına gelir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# single point of failure (yada SPOF)
çökmesi/bozulması durumunda, diğer tüm alt sistemlerin durmasına sebep olacak alt sisteme denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# NATS
ikinci adı: STAN (Nats'ın tersi) kaynak: https://github.com/nats-io/nats-streaming-server/blob/master/server/clustering.go#L54 (archive date: 10/10/2019) (54 üncü satırda yorumda da bu isim kullanılmış)

publish/subscribe mesaj alışverişini sağlayan açık kaynaklı kütüphanedir. NATS ile haberleşilen ortamda "go" dili ile yazılmış bir sunucu bulunmaktadır. Sunucu kendi için kuyruk mekanizması barındırır. Birçok programlama dili için client yazılmıştır.

NATS; Neural Autonomic Transport System'ın kısaltmasından türetilmiştir. Geliştirici ekip, NATS'ı, merkezi bir sinir sistemi gibi çalıştığını düşündüğü için böyle bir isim üzerine karar vermiştir.

NATS'ın 2 çeşit sunucus vardır: "NATS Streaming Server" ve "NATS Server". streaming server, normal NATS Server'ı wrap ediyor ve ek özellikler sunuyor. streaming server isminden anlaşılacağı gibi data steram'in yapmıyor. streamin server server'daki datayı persist ediyor (veritabanına kayıt ediyor) ve bunun sayesinde eski mesjaları herkesin alabilmesini de sağlıyor. diğer tüm özellikler butrada: https://nats-io.github.io/docs/nats_streaming/intro.html#features

nats-sub ve nats-pub go ile yazılmış, NATS client'larıdır. bu client'lar sadecd komut satırından parametre alırak client işlem yaparlar. sadece demo/test amaçlı geliştirilmişlerdir.

örnek komut:

> nats-pub "hello world"

java client için basit kod örneği;

```java
Connection nc = Nats.connect("nats://localhost:4222");
nc.publish("topic1", "hello world");
nc.close();
```

streaming publish tarafının kodu:
```java
StreamingConnectionFactory cf = new StreamingConnectionFactory("cluster-id", "client-id");
StreamingConnection sc = cf.createConnection();
sc.publish("topic-id", "my-message".getBytes()); // synchronous
```

streaming subscribe tarafı kodu:
```java
// kendi içimizde tuttuğumuz bir obje. bu sadece bir integer. her defasında, value'sunu 1 düşürürüz.
// en sonda thread'imizi bekletmek için bu objemizden yararlanacağız
final CountDownLatch doneSignal = new CountDownLatch(1);// 1'den aşağıya geri sayım yapabileceğiz

Subscription sub = sc.subscribe("topic1", new MessageHandler() {

    public void onMessage(Message m) {
        System.out.printf("Received a message: " + m.getData() );
        doneSignal.countDown();
    }

}, new SubscriptionOptions.Builder().deliverAllAvailable().build() ); // 3rd parameter is Options

// burada countdown 0 olmadan thread beklemeye alınıyor
doneSignal.await();

sub.unsubscribe();
sc.close();
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Axon framework
java için açık kaynaklı CQRS, Saga, "event sourcing" pattern'lerini kolayca uygulamaya yarayan bir framework. CQRS, Saga, "event sourcing" patternleri manuel olarak yazılımcı tarafından da yapılabilir(implemente edilebilir). fakat framework'ün sunduğu birçok avantaj'da vardır.

__Eventuate__ axon'a alternatif bir frameworktür.

Axon'un JVM üzerinde çalışan bir sunucusu vardır. her uygulama sunucuya bağlanır. sunucunun içinde event bus mekanizması vardır. bu event bus, yazılımdaki event'lerimiz tutmaya yaramaktadır. Sunucu, event'leri görüntülemek için web GUI'de sunmaktadır.

Axon'da CQRS ve Saga opsiyonel iken; "event sourcing" kullanımı zorunludur. event sourcing metodları boş bırakılır ve her event sourcing işleminde agreegate objelerimiz JPA'ya basılırsa, "event sourcing" kullanılmamış varsayabiliriz. fakat böyle bir durumda sadece event base programming yapmış oluruz ve Axon kullanmamızın pek anlamı kalmaz.

Axon mikroservislerde veya monolitik uygulamalarda da kullanılabilmektedir.

Axon sunucusunun event bus'ında, Command Bus, Event Bus ve Query Bus sürekli çalışmaktadır.

Axon'da temel kod örneklerine bakalım:

```java
import org.axonframework.commandhandling.CommandHandler;
import org.axonframework.eventsourcing.EventSourcingHandler;
import org.axonframework.modelling.command.AggregateIdentifier;
​
import static org.axonframework.modelling.command.AggregateLifecycle.apply;
​
public class GiftCard {
​
    @AggregateIdentifier // bu değer id'nin bu aggregat için unique olduğunu belirtiyor. eğer birden fazla event/command çağrılırsa, "points" değeri aynı id'li tüm aggregate işlemleri için ortak olacaktır.
    private String id;
    private int points;
​
    @CommandHandler // CQRS'teki "command" e denk gelmektedir.
    //her command metodunun içinde bir den fazla event olabilir.
    // businnes logic katmanlarından (businnes logic bu class'ta diil. businnes login'ler buraları çağıracak) event çağıramayız. command çağırmak durumundayız. her command kendi içinde zaten birçok event'i gerekiyorsa çağıracaktır.
    // command metodları "decision-making/business logic" kodları içermelidir.
    public GiftCard(IssueCardCommand cmd) {
       
       // apply ile bir event çağrıyoruz. event'i çağırıyoruz. event'i çağırırken, parametreleri event CardIssuedEvent objesi içerisinde gönderiyoruz.
       apply(new CardIssuedEvent(cmd.getCardId(), cmd.getAmount()));
    }
​
    @EventSourcingHandler
    // CardIssuedEvent tetiklediğimiz event tamamlandığında çağrılacak metoddur.
    // bu metod içerisinde GiftCard objeminiz güncelleriz. GiftCard bizim database objemiz değildir. GiftCard şu anda Axon yapısının gerektirdiği bir sınıftır/yapıdır.
    // "state changes" lerin bulunduğu metodlar EventSourcingHandler metodlarıdır. state change'leri DB'deki ye yazılmaz. DB'ye yazacağımız metodlar farklı yerde.
    // "state changes" kesinlikle yukarıdaki command metodlarında yapılmamalı.
    // command metodları genelde state'i kontrol edip, event tetiklemelidir. eğer state te bir hata varsa, command metodu exception fırlatmalıdır.
    public void on(CardIssuedEvent evt) {

        // burada GiftCard sınıfının datalarını güncelleriz. bu şekilde bu sınıfta (GiftCard sınıfında) olan diğer metodlar(command metodlar) da güncel olarak isteği bilgileri okuyabilecektir. 
        id = evt.getCardId(); // bu işlemin en öncelikli yapılması lazımmki; diğer eventler aynı GiftCard property'lerini okuyabilsin.
        points = 0;
        //bu adımda db'deki GiftCardDB objemiz güncellenmemiştir. bu adım sadece GiftCard sınıfı (bu sınıf) içindeki dataları günceller. 
    }
​
    protected GiftCard() {
      // boş constructor Axon frameworkü tarafından zorunlu tutuluyor.
    }
}
```

```java
import org.axonframework.modelling.command.TargetAggregateIdentifier;
​
public class IssueCardCommand {
​
    @TargetAggregateIdentifier // bu değer, GiftCard objesindeki id'ye denk geldiğini gösteriyor. mutlaka bu olmalı.
    private final String cardId; 
    private final Integer amount;
​
    // constructor / getters / setters
}
```

# State storage
yukarıdaki kod örneğinde; "points" ve "id" değerleri storage'dir. storage'de tüm instance'ların değerleri mevcuttur. bu storage kısa süreli tutulması beklenen bir storage'dir. kalıcı olacak olan bilgiler zaten her event sonrası yada command sonrası db'ye yazılmalıdır. state storage'deki her değeri, o cammand'in çağırdığı her event okuyabilir. fakat dışarıdan kimse okuyamaz.

# event storage
her event'in kaydedildiği db'dir. Herhangi bir db kullanılabilir. fakat normal uygulamda çok fazla event olacağından (herşeyin modüler ve event yapısında olduğu düşünülürse), db'yi buna uygun şekilde seçmekte yarar var. AxonServer'ın kendi içinde buna spesifik geliştirilmiş bir API var. bu api dataları tutuyor ve yine select sorgusu atılabilmesini sağlıyor.

# Multi-entity Aggregates

Aggregate Root objemiz GiftCard olsun, onun altında bir çok transaction işlemi olsun. Bunu axon ile bu şekilde ifade ediyoruz:

```java
import org.axonframework.modelling.command.AggregateIdentifier;
import org.axonframework.modelling.command.AggregateMember;
import org.axonframework.modelling.command.EntityId;
​
public class GiftCard {
​
    @AggregateIdentifier
    private String id;
​
    @AggregateMember
    private List<GiftCardTransaction> transactions = new ArrayList<>();
​
    private int remainingValue;
​
    // command handlers, event sourcing handlers
}
​
public class GiftCardTransaction {
​
    @EntityId
    private String transactionId;

    private int transactionValue;
    private boolean reimbursed = false;
​
    public GiftCardTransaction(String transactionId, int transactionValue) {
        this.transactionId = transactionId;
        this.transactionValue = transactionValue;
    }
​
    public String getTransactionId() {
        return transactionId;
    }
​
    // command handlers, event sourcing handlers
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Eventual consistency (yada nihai tutarlılık)

bir client'ımız olsun. birçok sunucudan X bilgisini çekiyor olsun. her sunucu (aralarında senkronizasyon yavaş olduğu için) client'a farklı bilgi gönderiyor olabilir. Çünkü X datası henüz tüm sunucularda güncellenmemiştir. bu durum tutarlı değildir. Eventual consistency olan bir yapıda/sistemde/yazılımda bu çeşit bir hata söz konusu olamaz. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# lombok

lombok java için birçok ek özellik sağlayan altyapıdır. jar olarak classpath'e eklendiğinde build sırasında devreye girmektedir, bu şekilde özelliklerini devreye sokabilmektedir.

örneğin;
- getter-setter'ları bizim için oluşturmaktadır.
- builder oluşturmaktadır
- toString oluşturmaktadır
- equals fonksiyonlarını oluşturmaktadır

özelliklerin tüm listesi buradadır: https://projectlombok.org/features/all

IDE'nin derlemeden önce bazı şeyleri görebilmesi için lombok'un IDE eklentisinin yüklü olması gereklidir. IDE'de lombok-eklentisi yüklü ise aşağıdaki kodda hata almayız.

```xml
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.4</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

```java
public class UserIntegrationTest {
 
    @Test
    public void givenAnnotatedUser_thenHasGettersAndSetters() {
        User user = new User();
        user.setFirstName("Test");
        assertEquals(user.gerFirstName(), "Test");
    }
 
    @Getter @Setter
    class User {
        private String firstName;
    }
}
```

IDE eklentisi, direk kodları da generade ediyor. yani sadece eklenti olursa, dependency'ye ihtiyaç kalmadan ilgili kodları generate edebiliyor. bu özellik zaten IDE'lerin içinde var.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ArchUnit
java test kütüphanesidir. derlenen projedeki java sınıflarının mimarisini kontrol edip, mimari açıdan uygun olup olmadığını test edebilmemiz sağlar. 

örnek:
- interfacelerin isimleri şunlarla başlamalıdır
- implemetasyonların prefixi şu olmamalıdır
- standart stream'lere  (system.out gibi) hiçbir yerden erişilmemiş olmalıdır

örnek:

aşağıdaki örnek 3 farklı test vardır. büyük harfle yazılanlar ArchUnit içerisinde yüklü gelen default mimari testler.
3'üncü loggers_should_be_private_static_final isimli fonksiyon'u ise biz tanımladık.

bu testleri junit içerisinde çağırıp koşabiliriz.

```java
@AnalyzeClasses(packages = "com.tngtech.archunit.example.layers")
public class CodingRulesTest {

    @ArchTest
    private final ArchRule no_generic_exceptions = NO_CLASSES_SHOULD_THROW_GENERIC_EXCEPTIONS;

    @ArchTest
    private final ArchRule no_java_util_logging = NO_CLASSES_SHOULD_USE_JAVA_UTIL_LOGGING;

    @ArchTest
    private final ArchRule loggers_should_be_private_static_final =
            fields().that().haveRawType(Logger.class)
                    .should().bePrivate()
                    .andShould().beStatic()
                    .andShould().beFinal()
                    .because("we agreed on this convention");
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# IntelliJ IDEA

JetBrains (eski ismi: IntelliJ) firması tarafından geliştirilen IDE. 2 tür lisans ile dağıtılıyor. Ücretsiz versiyon kısıtılı özelliklere sahip ve açık kaynaklı. Ücretli versiyonda kapalı kaynak eklentiler mevcut.

Genel olarak IDE Indexing konusunda diğer IDE'lere göre çok başarılı. Arkaplanda anlık yazdıkça indexleme yapılıyor ve bu yüzden geliştiriciye daha çok seçenek sunabiliyor (otomatik tamamlama gibi). bunları yaparkende herşeyi thread açıyor ve bu sebeple diğer IDE'lere göre hiç takılmıyor.

# Android Studio

IntelliJ IDEA üzerine ek olarak; android geliştirme ortamı için eklentiler ile google tarafından dağıtılan IDE.

# Android Developer Tools (yada ADT) vs Andmore

Android plugin for Eclipse. It was developing by Google. Google now discontinued the project because they are developing plugins for IntelliJ Idea. Now the community fork the ADT project with new name: Andmore.

# project Structure of IDE's

birbirine denk gelen terimler aşağıdaki tabloda verilmiştir:

```
Eclipse       -> Workspace | Project

IntelliJ      -> Project   | Module

Visual Studio -> Solution  | Project

XCode         -> Workspace | Projects
```

- Android Studio, IntelliJ türevi olduğu için aynı mantıkta çalışmaktadır.

- IntelliJ bir pencerede birden fazla proje açmıyor. sadece 1 proje ve onun alt modülleri olabilir. IntelliJ aynı projeyi 2 farklı pencerde açılmasına da izin vermiyor. sadece sekmeler farklı sekmelerde açılabiliyor.

- IntelliJ'de eclipse'teki gibi perspektifler yoktur.

# maven/gradle/ide proje dizin yapıları hakkında
saf IDE'siz sadece gradle android projesi oluştursak bir gradle dosyası yeterli. gradle, maven gibi altprojeleri destekliyor. "android studio" proje dizininde gradle görürse alt modülleri daha iyi yönetebilir. bu sebeple react-native gibi komut satırı uygulamaları android studio'ya uygun android projesi yaratır. yine bu sebeple android kodlarımızın olduğu proje bir module olmaktadır. modülümüzün bir üst dizini (android studio için project dizini) kodsuz bir gradle projesidir. bu gradle'nin alt modülü bizim android projemizdir. eğer android'e bir library yazmak istersek onu da android ile aynı seviyede yeni bir modül olacak şekilde ayarlamalıyız.

# eclipse version history

| name     | release date   | version     |
|----------|----------------|-------------|
| Callisto | June 2006      | 3.2         |
| Europa   | June 2007      | 3.3         |
| Ganymede | June 2008      | 3.4         |
| Galileo  | June 2009      | 3.5         |
| Helios   | June 2010      | 3.6         |
| Indigo   | June 2011      | 3.7         |
| Juno     | June 2012      | 3.8 and 4.2 |
| Kepler   | June 2013      | 4.3         |
| Luna     | June 2014      | 4.4         |
| Mars     | June 2015      | 4.5         |
| Neon     | June 2016      | 4.6         |
| Oxygen   | June 2017      | 4.7         |
| Photon   | June 2018      | 4.8         |
| 2018-09  | September 2018 | 4.9         |
| 2018-12  | December 2018  | 4.10        |
| 2019-03  | March 2019     | 4.11        |
| 2019-06  | June 2019      | 4.12        |
| 2019-09  | September 2019 | 4.13        |
| 2019-12  | December 2019  | 4.14        |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CPU Scheduling algortithms (yada process Scheduling algortithms)

# time slice (yada quantum)
işlemin cpu'da kesintisiz olarak kalacağı minimum süredir.

# algortithms

  - ## Round-robin (yada RR)
    round kelime anlamı: tur

    robin kelime anlamı: özel bir kuş türü

    quantum süresi vardır. tüm işlemler kuyruğa alınır. ilk işlem kuantum süresinde cpu'da kalır ve kuyruğun en sonuna atılır ve yeni gelen process varsa yine kuyruğun sonuna atılır. quantum bittiğinden dolayı tekrar skuyrukta sırada bekleyen işlem, cpu'ya alınır. bu işlemde kuyruğun sonuna tılır ve yeni gelen işlem varsa kuyruğun sonuna atılır...

  - ## First-Come, First-Served (FCFS) Scheduling
    İlk gelen kesintisiz olarak cpu'da kalır ve işlemin tamamlanması beklenir.

  - ## Shortest-Job-Next (SJN) Scheduling (yada shortest job first yada SJF)
    her zaman o anda kısa olan process tamamlanana kdar cpu'ya girer.

  - ## Shortest Remaining Time
    SJN'ye benzer. fakat quantum süresi vardır. quantum süresi her tamamlandığında, en kısa olan process işleme alınır.

  - ## Priority Scheduling
    her işlemin bir önceliği vardır. kalan zaman, arrival time gibi değerlerin yanında priority değerine bakarak ta hangi process'in cpu'ya gireceği karar verilir. bunun birçok implementasyonu vardır.

  - ## Multiple-Level Queues Scheduling
    birden fazla cpu kuyruğu yaratılır. her kuyruğun kendi içinde farklı algoritması olabilir. ve her 2 kuyruktan işleme alınacak değer seçilirkende farklı algoritmalar uygulanabilir. yani her kuyruk kendi içinde hangi process'in işleme gideceğini belirler. daha sonra her kuyruktan sunulan/belirlenen işlemler arasındada bir sçeim yapılır ve cpu'ya tek bir işlem yollanır. bu sebeple bu algoritmanında birçok implementasyonu vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# false positive (yada type I error)vs false nagatif (yada type II error)
mecical testlerde, binary classification ve istatistik bilimlerinde kullanılan terimlerdir.

bir data incelenir (örnek: ocr ile bir karakter tespiti yapılmaya çalışılsın, yada hastalığın tanısı koyulama çalışılsın). inceleme sonucu X kararına varılır (örnek: ocr sonucu karakter 'A' olduğuna kanaat getirilsin, yada hastalığın zatüre olduğuna karar verilsin).

Eğer gerçek sonuç X ten farklı ise; bu duruma 'false positive' denir.

İnceleme sonuu 'X değil' kararına varılır (örnek: ocr sonucu 'A karakteri değil', yada 'hasta zatüre değil' kararına varılır).

Eğer gerçek sonuç X ise o zaman bu duruma 'false negatif' denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Appmon
__Dynatrace__ firması tarafından geliştirilen "Application Performance Monitoring" yazılımıdır. örneğin; java web uygulamamızı takip eder ve ona gelen istekleri gruplayıp, dashboard'larda detayları gösterir. stacktrace'e kadar analiz edilmesini sağlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# jhipster
app genrator command line tool (boilerplate) for spring boot microservices (netflix), Angular, React, Bootstrap, Maven/Gradle, Elastic Stack, Docker.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ramdisk
ram, ssd'lerdendahi çok daha hızlı. bir yazılım ram'de bellek ayırıp (bellekte sanal bir dosya sistemi yaratıp), bunu hard-drive mış gibi OS'a mount edebilir. tıpkı bir usb bellek gibi. işte bu ram'deki belleğe ramdisk denir.

piyasada bir çok ramdisk görevi gören uygulama mevcut.

linux'taki "mount" komutu ramdisk ihtiyacını giderebiliyor. örnek:

```sh
mkdir /dir_to_mount
mount -o size=16G -t tmpfs none /dir_to_mount
```

mount komutu ram'de 2 farklı sanal file system destekliyor: tmpfs ve ramfs(tmpfs'e göre daha eski)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CGI (yada Commond Gateway Interface)
server'da istekleri karşılayıp, istekteki URL'deki path'lere göre iligli uygulamaya isteği yönlendiren yazılımdır. ISS, javaee sunucuları gibi gelişmiş sunucular çıktığında beri, özel durumlar hariç kullanılmıyor.

CGI'nin RFC'si vardır. yani belli standartlardadır. protokol hakkında detaylar RFC'de belirtilmiştir.

CGI kullanan birçok yazılım mevcut: mod_perl, mod_php4, mod_fastcgi

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Bottleneck (yada darboğaz)
bu terim bilişim dünyasında bu tazr durumlarda kullanılmaktadır:
- network'e gelen istekler internet hızımız sebebi ile yavaşlaması. bu durumda network darboğaz edilir.
- bilgisyarda bir process cpu ve hdd aynı oranda hızlı olmadıklarından dolayı (örnek hdd yavaş ise), process'in yavaşlaması. bu durumda hdd darboğaz edilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# twelve-factor app

12factor.net'te yayımlanan, birçok contributer'ın desteği ile genel olarak tüm software-as-a-service yazılımlarda uyulması gereken 12 temel kural yer almaktadır. her kural 1-2 sayfa uzunluğunda açıklanmıştır. özetle:

- Codebase (Kod Tabanı): birkaç uygulama, aynı kodları içeriyorsa mutlaka o kodlar common lib yapılmalı.

- Dependencies (Bağımlılıklar): bağımlılıklar mutlaka hiyerarşik olarak tanımlanmalıdır

- Config (Yapılandırma): configler mutlaka codebase'den ayrı ve prod, test gibi ortamlar için ayrı gruplanmalıdırlar

- Backing Services (Destek Servisleri): dış kaynaklar/hizmetlere gidebilmek için sadece config'lerin değiştirilmesi yeterli olmalıdır. örneğin; farklı bir db'ye giderken codebase'de dğeişiklik yapılmamalıdır. bir config değişikliği yetmelidir.

- Build, Release, Run (Derleme, Sürüm, Çalıştırma): pipeline olmalıdır ve eski sürüme dönülebilmelidir

- Processes (Süreçler): stateless(durumsuz) ve shared-nothing(bir şey paylaşmayan) olmalıdırlar. veriler process'lerde diil, veritabanlrında saklanmalıdırlar.

- Port Binding (Port Bağlama)

- Concurrency (Eş Zamanlılık)

- Disposability (Kullanıma Hazır Olma Durumu): birden kesinti olma durumuna hazır olabilmelidir.

- Dev/Prod Parity (Geliştirme/Üretim Ortamlarının Eşitliği): dev ortamında kullanılan tool'lar ile prod/test ortamında kullanılan tool'lar mümkün oldukça aynı olmalıdır. devops kültürü mutlaka olmalıdır.

- Logs (Günlükler)

- Admin Processes (Yönetici Süreçleri): yazılım yönetimi işlemleri tool'lar aracılığı ile yapılmalıdır. ve sadece 1 kerelik yapılacak işlemler (örneğin bir sql işlemi), script haline getirilip tüm ortamlarda çalıştırılabilmelidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# metasearch engine
startpage.com ve searx projesi buna bir örnektir. diğer arama motorlarını kullanarak sonuçları yansıtan arama motorlarıdır. kendi veritabanlarını bulundurmazlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# apache hadoop

açık kaynaklı bir framework'tür. MapReduce mantığını temel alarak çalışır ve big-data'mız üzerinde işlem yapabilmemizi sağlar. merkezi servisimize istek gelir, hadoop java api'miz ile hadoop sorgumuzu yazarız ve data'ları çekmek için java-API her node'a istek atar. her node HDFS dosya sistemi ile datalarımızı tutmaktadır. her node kendi içerisinde bilgiyi bize (merkezi noktaya) döndürür. bu şekilde merkezi servisimizde yük diğer node'lara paylaştırılmış olur.

haddoop'un Mödülleri:

- common

  utilities like java API (lib)

- HDFS (yada Hadoop Distributed File System)

  veritabanının formatıdır. bu tek bir makinede olmak zorunda değildir. birçok farklı lokasyonlardaki birçok farklı makinede parçalı şekilde bulunabilir.

Hadoop kendi içerisinde Zookeeper dan yararlanmaktadır. Hadoop'u kaldırmadan önce Zookeeper'ı ayağa kaldırmak gerekir. Zookeeper key/value bir storage manager'dır. (fakat piyasada geliştiriciler, açık olan web servisleri Zookeeper storage'sinde tutup Zookeeper'ı bir "servis discovery" olarak ta kullanmaktadırlar.)

# MapReduce

map ve reduce olarak iyi ayrı işlemi temsil eder. map, sorgunun başlangıcında HDFS'den topladığımız her değeri döner ve bu değer üzerinde değişiklik yapmamızı sağlayan fonksiyondur. reduce işlemi ise, map sonunda elde ettiğimiz tüm değerleri alır ve sonunda bir değer döndürmemizi sağlayan fonksiyondur. bu iki fonksiyon biribirini tamamlamaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mesasuring maintainability

  - # line of codes (yada LOC yada source lines of code yada SLOC)
     
  - # cyclomatic complexity

    if içinde if olma oranıdır. bu oran yükseldikçe maintainability düşer.

  - # depth of inheritance tree (yada DIT)
  
    sınıfların birbirinden extend etme oranıdır. ne kadar çok üst sınfıtan extend edersek maintainability o kadar düşer.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Greenfield systems vs Brownfield systems
Greenfield projeler sıfırdan başlanılan projelerdir. Brownfield ise varolan sitemin güncellenmesi gibi durumları temsil eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# windows için paket yöneticileri

  - # Chocolatey
  windows için açık kaynaklı komut satırı paket yöneticisidir.

  programları standart olarak program files içerisine kurar. fakat bu dizin istenirse her program için ayrı ayrı override edilebilir. kurulan programlar, normal koşullarda windows dizinine dll atıyorsa, Chocolatey ile kurulursa da aynı şekilde dll atarlar. Chocolatey native setup/installer'ı çalıştırır. çalıştırırken yüklenecek dizinleri override eder.

  Burada birçok açıklama mevcut: https://chocolatey.org/docs/GettingStarted#terminology (archive date: 10/11/2019) 

  - # scoop
  Chocolatey'e alternatiftir.

  - # Ninite
  son kullanıcı internet sayfası üzerinden kurmak istediği program listesini seçiyor ve ninite'nin o onda o listeye göre oluşturduğu exe dosyasını indiriyor. bu exe ile windows makineye kurulum yapılıyor. uygulamalar program files içerisine normal setup dosyaları internetten indirilerek kuruluyor.

# windows installers

  - InstallShield
  - Nullsoft Scriptable Install System (yada NSIS): winamp'ı geliştiren Nullsoft firması tarafından geliştirilmiştir.
  - Windows Installer: uzantısı .msi'dir. microsoft'un resmi installer'ıdır. diğer installer'lar msi formatını kullanamaz. diğer installer'lar .exe formatını kullanmak zorundalar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# BCD code (yada Binary Coded Decimal code)
onluk sistemindeki bir sayının her basamağın ayrı ayrı ikilik tabana çevrilerek yazılmış halidir. 2 çeşit BCD vardır:

- Genişletilmiş (Uncompressed) BCD

Her basamak 1 byte ile temsil edilir. örnek:

```
Onluk Taban: 91
BCD: 0000 1001 0000 0001
```

9; 1001'a eşit olduğundan her zaman 1 byte'ın yarısı boşa gitmektedir.

- Sıkıştırılmış (Packed) BCD

Her basamak 4 bit ile temsil edilir.

```
Onluk Taban: 91
BCD: 1001 0001 (aradaki boşluk olmamalı. okunabilirliği arttırmak için boşluk bırakıldı.)
```

```
Onluk Taban: 123
BCD: 0000 0001 0010 (aradaki boşluk olmamalı. okunabilirliği arttırmak için boşluk bırakıldı.)

min 8 bit olarak kullanılan bir makinede yukarıdaki BCD değerini sol tarafına 4 bit koymak zorundayız. sonuçta bunu elde ederiz:
BCD: 0000 0000 0001 0010 (aradaki boşluk olmamalı. okunabilirliği arttırmak için boşluk bırakıldı.)
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# eşlik biti (yada parity bit yada check bit)
binary data kümemizin başına yada sonuna konular bir çeşit kontrol bitidir. data kümemizde bulunan toplam 1 sayısı tek sayı ise 1, çift sayı ise 0 olarak eşlik biti kaydedilir (tersi de olabilir: eğer tek sayı için 0 verildiyse, çift sayı 1 için 1 kullanılır). bu şekilde datanın değişip değişmediği güvence altına alınmış olur. genelde iki bilgisyar arası haberleşme protokollerinde bu yöntem kullanılır. örnek:

| kontrol edilen data | count of "1" bits | data including parity |
|---------------------|-------------------|-----------------------|
| 0000000             | 0                 | 00000000              |
| 1010001             | 3                 | 11010001              |
| 1101001             | 4                 | 01101001              |
| 1111111             | 7                 | 11111111              |

Parity biti 1 bit değişikliğe uğrarsa, hatayı tespit edebilmemizi sağlıyor. fakat 2 bit değişikliğe uğramış ise o zaman hatayı tespit edemeyiz. dolayısı ile parity önemsiz data kontrollerinde kullanılmalıdır çünkü %100 doğruluğu yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# media center
youtubeden video izleme, netowkrdeki birçok protokol ile paylaşılmış media dosyalarını direk açma, yayın protokollerini izleme, local media dosyalarını yönetme izleme gibi birçok media işlevini yerine getiren birçok mödülü olan uygulama.

# Kodi (yada eski ismi XBMC)
Cross platform, Açık kaynaklı media center uygulamasıdır.

# Windows Media Center
microsoft'un media center uygulamasıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Module (yada modül) vs Component (yada Bileşen)
- modül, bir bütünün içinde diğer elemanlarının çalışmasından bağımsız çalışabilen bir parça olarak tanımlanabilir. modül, load/unload edilebilen bir parçadır.
- Component tekrar kullanılabicek olan parçadır.
- modül tekrar kullanılabilir olma durumu söz konusu diildir.
- "Component-based software engineering" ve "Modular programming" teknikleri yukarıdaki bilgilere dayanmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# context (yada tr:kontekst yada bağlam)
kontekst kelime anlamı: ortam, çevre

birçok api'de context terimi geçer. context bulunanan platform hakkında data bilgisidir. örneklerden gidersek daha net anlaşılır:

- servlet'e bir request geldi. servlet service metodu çalıştı 2 adet parametre aldı: request, response. service metodu içinden Servlet sınıfının getContext metodu çağrılabilir. getContext bize asıl amacımız olan request ve response dışındaki tüm data'yı sağlar. getContext Servlet'e (Servlet bu senaryoda platformumuz oluyor) geçilmiş parametreleri döndürüyor.

- kuberneteste komut satırından işlem yaptığımızda asıl amacımız container ağa kaldırmak, silmektir... Kubernetes'te context değiştirdiğimizde çalışma ortamımızın (platformumuzun) ayarlarını değiştiririz.

- android'te context bir sınıftır. Bu sınıf service, activity gibi sınıfıların super class'ıdır. context en tepededir ve uygulama seviyesinde (platform seviyesinde) bilgileri/metodları içerir. Android platformundaki android.content.Context sınıfının içindeki yorumlara bakacak olursak:

```
/**
 * Interface to global information about an application environment.  This is
 * an abstract class whose implementation is provided by
 * the Android system.  It
 * allows access to application-specific resources and classes, as well as
 * up-calls for application-level operations such as launching activities,
 * broadcasting and receiving intents, etc.
 */
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# header file (yada copybook)
programla dillerinde genelde dosyanın tepesinde bazı kütüphaneler import edilir. eğer kütüphane import edilirse ilgili kütüphane dosyaları, ilgili dosyamızın içine kopyalanır. bu durumda kopyalanan bu dosyalar header dosyalarımız olur. C/C++ buna en iyi örnektir.

java c# gibi dillerde header file yoktur. çünkü sadece ilgili kütüphanenin referansı tutulur. bunlara da dynamic library symbols denir.

# symbol (yada sembol)
IDE'lerde "find symbol" olarak ta menulerde bu seçeneği görebiliriz. 

symbol bazı programlama dillerinde __identifier__ yada __atom__ olarak ta isimlendirilir.  

symbol programlama dillerinde: metod referansları, instance referansları ve sınıf referanslarıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# consul
kelime anlamı: konsolos

- açık kaynaklı discovery server'dır.
- Raft (kelime anlamı: sal) consensus (kelime anlamı: fikir birliği) algoritmasını kullanmaktadır.
- key/value storage'si ile configurasyon altyapısı da sunmaktadır.

# raft algortiması
raft algoritması node'ların aralarında nasıl anlaşmaya varacaklarını belirleyen protokol için kullanılmaktadır.

**Raf, Paxos** algoritmasına dayanır.

Consul mimarisinde, her programcık, consule agent'ını (kütüphanesini) bulundurmak zorundadır. Consul agent'lar consul server'lara bağlanır. sistemde bir çok consul server vardır. consul server'lar ise kendi içlerinde lider seçerek haberleşirler.

1 agent birden fazla consul server'a bağlı olabilir. bu şekilde birine birşey oldumu diğerinden bilgi almaya devam eder.

raf'ta her node (consul server) 3 farklı state'de bulunabilir:
- follower
- candidate (aday)
- leader

İlk başta her node follower'dır. eğer bir lider'den haber alamazlarza, kendilerini candidate olarak belirleyebilirler. candidate olan node, diğer node'lara vote için request yollar. Bu olaya **"leader election"** denmektedir.

Sistemde bir data güncellemesi yapacak olalım. x=1 yapalım. client lidere x=1 bilgisini yolluyor. bu güncelleme her node'a leader tarafından yollanıyor. eğer çoğunluk (majority) dönerse, lider tekrar herkese bu bilginini herkes tarafından güncellendiğini onayladığı mesajını yolluyor. erkese bu bilgiyi yolladığında client tarafa da datanın güncellendiğini bildiriyor. bu olaya **"Log Replication"** deniliyor. Lider tarafından herkese yollanan data güncelleme request'ine **Append Entry message** deniliyor.

  - ## some timeouts

       - ### election timeout
         raft algoritmasında lider seçemk için her follwer'ın candidate olması için random bir süre vardır. genelde 150ms ile 300ms arasında değişir. bu şekilde tüm sistem aynı anda candidate olmamış olur.

       - ### heartbeat timeout
         lider sürekli olarak heartbeat ile follower'ları kontrol eder ve hepsinden dönüş alır. eğer follower'lar heartbeat isteğini liderden almazlarsa, o zaman yeniden o follower'lar election'a gidecektir. heartbeat için follower tarafında da bir timeout vardır.

# majority of votes
lider seçilirken, candidate'in en az sistemdeki node sayısı + 1 kadar oy alması gerekir. eğer bu sayı daha düşük olursa o zaman farklı bir candiate'te kendini lider olarak kabul edebilir.

# network sorunları
node'lar biribiri ile haberleşirken, node'ların bir kısmının biribi ile iletişiminin kesildiğini düşünelim. böyle bir durumda her iki grup kendi içinde lider seçecektir. client hangi lidere data güncelleme bilgisi yollarsa, artık datası sadece o grupta güncelleniyor olacaktır. bir süre sonra 2 grubun network erişimi tekrar düzelir ve birbirleri ile iletişime geçerlerse, iki grubun lideri daha çok oy toplamış bir grubun lideri karşısında kendini follower'a çekecek ve kendi güncellemelerini roll-back edecektir. yeni güncellemeleri lider'den alacaktır.

# Ethereum and Bitcoin
sanal para sistemlerinde consensus raft veya paxos yada bunlardan türemiş teknikler kullanılmamaktadır. çünkü sanal paralarda lider seçimi söz konusu değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# OpenStack
NASA'nın da geliştirmekte olduğu, açık kaynaklı Bulut bilişim altyapı projesidir. bulut bilişim sunan bir firmada olması gereken tüm yazılımlar bu proje altında açık kaynaklı olarak geliştirilmektedir. projede öncelik IaaS'tır.

# OpenShift
Kubernetes'i gömülü olarak kullanan onun üzerine ek özellikler sunanan bir yazılımdır:

- servislerin yönetilmesi
- servislerin discover edilebilmesi
- log takibi
- servislerin birbiri ile konuşması
- dış dünyadan gelen isteklerin balancing edilmesi

OpenShift 'oc' komut satırı uygulaması yada web arayüzünden yönetilmektedir.

- ## minishift
komut satırı uygulamasıdır. local'de tek cluster openshift ayağa kaldırır.

- ## OKD (yada Origin Community Distribution yada eski adı:OpenShift Origin)
ücretsiz lisanslı openshift versiyonu.

- ## OpenShift Container Platform (yada eski adı:OpenShift Enterprise)
OKD'nin lisansı ücretli ve destek alınabilen versiyonudur.

- ## Red Hat OpenShift Online (yada RHOO) vs OpenShift Dedicated vs OpenShift.io
Ticari lisanslı sadece bulut makinelerde çalışabilen openshift hizmetleridir.

# Envoy
türkçe kelime anlamı: elçi

L7 proxy'dir ve load balancing gibi ek özellikler barındırır.

# L4 vs L7 proxy
Proxy'ler data iletiminde aracılık yaparlar. aracılık yaparken gelen giden datanın ne olduğu ile ilgilenilirken, L4 proxy'ler OSI katmanlarında 4üncü level seviyesinde data transferlerine manipülasyon yapabilirler. Yani UDP, TCP seviyesinde datayı tanırlar. TCP ile gönderilen datanın ne olduğu ile ilgilenemezler. Oysa L7 proxy'ler en üst seviye OSI katmanında, 7inci katman, application layer'da işlem yapabilirler.

Bazı durumlarda; L7 proxy; OSI 4 katmanında, L4 ise OSI 7 katmanında işlem yapması gerekebilir/yapabilir. Bir proxy yazılımının hangi tipte olduğunu tanımlarken en çok hangi katmandaki işlemlere öncelik verdiği değerlendirilir.

bir L7 proxy aşağıdaki özellikleri sunabilir:

- load balancing
  kendine gelen request'leri, önünde durduğu node'lara paylaştırır
- Health checking
  - Active
    önünde duruğu her node'a sürekli /healchecking gibi bir url ile istek atar. cevap alamazsa, o node'a requestleri yollamaz
  - Passive
    önünde durduğu node'ların client'lara verdiği cevapları inceleler. eğer cevaplarda terslik varsa, o node'u sağlıksız olarak set eder ve ona request yönlendirmez
- Service discovery
  kendini servis discovery'ye tanıtır
- Sticky sessions
  cookie'lere bakarak, kullanıcını aynı session'ı boyunca aynı node'a gitmesini sağlar.
- dos atakarının önüne geçmek için kalkan oluşturur
- rate limiting
  kullanının session'ına göre (state'ine göre) belli bir sürede yapabileceği request sayısı tanımlanır. eğer bu sayıyı geçerse kullanıcının engellenir.
- yönetim paneli

# Proxy türleri
- middle proxy
  klasik proxy sunucularıdır. gelen istekler ve döndürülen cevaplar bu proxy üzerinden geçer.
- Edge proxy
  api gateway olarakta adlandırılırlar.
- Embedded client library
  bir kütüphane olarak node'lara eklenir. bunun dezavantajı dilden bağımsız olamaması. "polyglot (yada multilingual)" kütüphaneler yazılması gerekir.
- Sidecar proxy
  kendi başına genelde aynı container içerisinde ayağa kaldırılan proxy'dir.

# Direct server return (yada DSR)
bu tarz proxy'ler dönen response'ları kendi üzerlerinden geçirmezler. response'lar direk client'a node tarafından yollanır.

# downstream vs upstream
proxy dünyasında; downstream requesti yollayan, upstream ise cevabı yollayan node'dur.

# Service Mesh
Mesh türkçe kelime anlamı: ağ

mikroservislerin artmasıyla birlikte bu tanım ortaya çıktı. servislerin yönetimi/ağın yönetimi sistem büyüdükçe daha zorlaşmaya başlıyor. bu ağı yönetebilmek için birçok çözüm sunuluyor. "Service Mesh" tanımı sıkça karşımıza çıkmaktadır. "service mesh" uygulanan bu çözümlere/altyapılara verilen genel bir terimdir.

sidecar proxy, service discovery, orchestration framework, load balancing, circiut breaker gibi altyapıları içeren çözümler sunulur.

(Sidecar pattern'i başka başlıkta anlatılıyor) Modern servise mesh çözümlerinde sidecar pattern'inden sıkça faydalanılıyor. modern çözümlerde, her pod/container'a bağlı bir sidecar proxy bulunuyor. her gelen giden request buradan geçiyor. o pod/container'a bağlı sidecar security, loggining gibi birçok görevi pod/container'da yüklü olan uygulama yerine yapıyor.

# Istio
kubernetes içerisinde oluşturduğumuz her pod içinde, istio (otomatik olarak) kendi container'larını açar. bu container'lar ile sidecar proxy pattern'ini baz alarak load balancing, logging gibi birçok servise mesh işlemimizi halleder.

'istioctl' komut satırı uygulamaıs ile yönetimi sağlar.

# Kubernetes (yada abb:K8s)
UCP, docker swarm ve docker compose'a alternatif olan ve daha fazla özelliği içeren açık kaynaklı yazılımdır. kubernetes containerları yönetmek çin kullanılır. container'ların ne ile yönetildiği ile ilgilenmez. yani Kubernetes docker yada rkt ile de çalışabilir. kaldırdığı container'ları istediğimiz herhangi bir container yazılımı ile kaldırabiliriz.

docker UCP'deki özelliklere ek olarak kubernetes'in sundukları:
- cross-application config server barındırır
- cross-application discovery server barındırır
- port yönlendirme yapar, bu şekilde istediğimiz container'ın istediğimiz portuna bağlanabiliriz
- scheduled cronjobs

- ## command line tools

   - ### kubectl
     kubernetes command line tool'un kısaltmasıdır.

   - ### minikube
     komut satırı uygulamasıdır. localde test amaçlı çalışmalar için geliştirilmiştir. local makinede sanal makina açmakta ve içerisinde tek node'lu kubernetes yürütmektedir.

     eğer local makinede docker desktop yüklü ise; sanal makinesizde direk kubernetes yürütebilmektedir çünkü kubernetes tool'ları docker desktop ile yüklü gelmektedir.

   - ### kubeadm
     komut satırı uygulamasıdır. kubernetes'in kendisini update edebilmemizi, cluster oluşturabilmemizi sağlıyor.

- ## components

     - ## Master Components
       Cluster'daki tüm node'ları ve sistemi yöneten modüllerin tümüdür. Aşağıdaki parçalardan oluşur:

        - ### kube-apiserver
          API'nin sağlanması için gereken module. api olarak servis sunuyor. buraya http isteği olarak yollanan emirleri kubelet'lere yolluyor. kube-apiserver master node içerisindedir.

        - ### etcd
          key-value veritabanıdır. bütün sistemin meta bilgilerinin yönetimi buradan yapılır.
        
        - ### kube-scheduler
          scheduler'ları düzenli olarak çalıştıran modüldür. aynı zamanda yeni oluşturulan POD'ları node'lara atayan modüldür.

        - ### kube-controller-manager
          node'ların ayakta olup olmadığını kontrol eder, açılan pod'ların doğru adet olduğunu kontrol eden... modüldür

     - ## node components
       node bir VM yada fiziksel makinedir. node components, her node içinde yüklü olan modüllerdir.

        - ### kubelet
          her node'da bir adet yüklüdür. bu yazılım kube-controller-manager'dan emirleri alır ve yerine getirir. o node üzerinde yeni pod'ların ayağa kaldırılması kapatılması gibi işlevleri yerine getiriyor.

        - ### kube-proxy
          her node'da bir adet yüklüdür. "kubernetes servis", "port yönlendirme" gibi networksel işlemlerinin altyapısının her node'da çalışmasını sağlayan yazılımdır. "network proxy" yazılımıdır.

        - ## Container Runtime
          her node'da kurulmuş olması gereken docker yada alternatifi container manager'ı.

- ## namespace, context, cluster

     - ### namespace
       her cluster'da birçok namespace olabilir. her namespace kendi içinde pod ve servisler bulundurur.

       tüm namespace'leri listele:
       > kubectl get namespaces

       yeni namepsace yarat:
       > kubectl create namespace dev

     - ### context
       tüm ayarların bütününe verilen isimdir. örneğin bir context yaratalım:
       > kubectl config set-context minidev --cluster=cluster1 --user=user1 --namespace=namespace1

       switch to context:
       > kubectl config use-context minidev

     - ### cluster
       kubernetes cluster altyapısını destekleyen platformdur. örneğin google cloud, amazon cloud bu yapıyı destekliyor. localden hangi cluster'a gideceğimizi komut satırından belirtmemiz gerekmektedir.

# Kubernetes Objects

- pod
- service
- volume
- namespace
- Controllers
  - ReplicaSet
  - Deployment
  - StatefulSet
  - DaemonSet
  - Job

# pod
kubernetes'in kullandığı en ufak birimdir. içerisinde birden fazla container olabilir.

her pod naetworkü ortak kullanıyor. istediği storage namepsacelerini ise ortak kullanabiliyor. pod bir container değil. pod container wrapper'ı. yani pod kendi içinde bir container dizini barındırmıyor. pod örneğin 2 container içeriyorsa, pod 2 container'ı docker ile ayağa kaldırıyor. 2 docker'ı yöneten yapıya pod deniliyor. pod'un kendisni bir container gibi düşünmemek gerekli. çünkü container terimi farklı kapıya çıkıyor. pod bir process olarak düşünülmeli. bu proses diğer açılan 2 docker proces'ini yönetiyor.

pod'a en yakın tanım docker-compose'dur. sadece belirtilen container'ları açmakla/yönetmekle ilgilenir. kendisi bir container değildir.

pod tanımının bulunduğu dosya örneği:

```yml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-example
spec:
  containers:
  - name: nginx
    image: nginx:stable-alpine
    ports:
    - containerPort: 80
    volumeMounts:
    - name: html
      mountPath: /usr/share/nginx/html
  - name: content
    image: alpine:latest
    volumeMounts:
    - name: html
      mountPath: /html
    command: ["/bin/sh", "-c"]
    args:
      - while true; do
          echo $(date)"<br />" >> /html/index.html;
          sleep 5;
        done
  volumes:
  - name: html
    emptyDir: {}
```

context'imizi komut satırından belirledikten sonra pod'umuzu yaratabiliriz:

> kubectl create -f multi-container-example.yaml

Yukarıda 1 pod içinde 2 adet container bulundurulmuştur. Mount şekli docker'dan biraz farklıdır. Mount tanımı container tanımının dışındadır (üst seviyesindedir). bu mounting'e ihtiyacı olan container'lar "mountPath" ile bu tanımı kendileri için kullanabilirler. Yukarıdaki html isimli volume her 2 container tarafından kullanılmış. böyle durumlarda aynı dosyalar 2 container tarafından da görülüp yazılabilmektedir.

Aşağıdaki komut ile varolan pod'ları listeleyebiliriz:

> kubectl get pods --show-labels

Loga bakmak için:

> kubectl logs -f pod-name --tail 200

Eğer bir pod içerisinde birden fazla container varsa, sadece ilgili container'ın loguna bakmak istiyorsak:

> kubectl logs -f pod-name --tail 200 -c container-name

get detail of running pod:

> kubectl describe pod pod-name

delete pod:

> kubectl delete pod pod-name
yada
> kubectl delete -f pod-manifest.yaml

# port-forward
pod manifes yml dosyamızda dışarıya açılacak portu belirtmezsek; sonradan komut satırında şu şekilde portu dışarıya açabiliriz:

> kubectl port-forward pod-name 8081:8080

# CNI (yada Container network interface)
host'lar ve container'lar arasındaki network iletişimini sağlayan kubernetes plugin'leridir. isteğe bağlı kullanılırlar. örnek pluginler: calico, flannel, weave net

temel olarak; CNI eklentisi, her makinede bir bridge (eth0, loopback gibi) açıyor. bu bridge üzerinde routing table configürasyonları ile diğer makinedeki container'lara direk olarak, container ip'leri ile istek atabilmemizi sağlıyor.

bu route table'ların birbirlerini görebildiği sanal ağın (ip kümesinin) tümüne __overlay network__ adı veriliyor.

# label and selector

pod veya service yml'mizin başında istediğimiz isimde label'lar kullanabiliriz. bu label'lar sayesinde daha sonra birçok sorgu, update gibi işlemleri belli pod veya servislerimize sağlamış olacağız.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-example
  labels:
    app: nginx
    environment: prod
...
...
```

# service
service selector'ler ile belirttiğimiz pod grubuna referans edecek olan sanal yapıdır. örneğin bir servise istek yaparsak aslında ona bağlı pod'lara istek yapmış oluruz gibi...

yml dosyamızın içerisinde "kind: service" yazmamız gereklidir.

# deployment
deployment içerisine biçok replica set ve pod barındırabilen bir yapıdır.

yml dosyamızın içerisinde "kind: deployment" yazmamız gereklidir.

# Replica Controller
istediğimiz pod sayısının ayakta kalmasını sağlar.

yml dosyamızın içerisinde "kind: ReplicationController" yazmamız gereklidir.

# Replica set
Replica Controller'ın alternatifidir. temelde aynı görevi görür.

yml dosyamızın içerisinde "kind: ReplicaSet" yazmamız gereklidir.

# desired state vs actual state
her kubernetes objesinin bir 2 state'i vardır. biri yazılımcının istediği durum, diğer ise şu andaki durumudur. örneğin  ReplicaSet ile 100 adet container ayağa kaldırmak istemiş olalım, fakat henüz sadece 50 tanesi ayakta olsun. budurumda "desired state":100, "actual state":50 dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# example algorithms

List of solutions:
- fibanacci (recursive + iterative)
- palindrome (recursive + iterative)
- Cycle detection - Floyd's Tortoise and Hare
- reverse list (recursive + iterative)

- ## fibanacci

- ### recursive

```java
public class App {
	
	public static void main(String[] args) {		
		if( fibonacci(0) == 0 &&
			fibonacci(1) == 1 &&
			fibonacci(2) == 1 &&
			fibonacci(3) == 2 &&
			fibonacci(4) == 3 &&
			fibonacci(5) == 5 &&
			fibonacci(6) == 8 ) {
			System.out.println("test passed.");
		}		
	}

	static int fibonacci(int n) {
		if (n <= 1)
			return n;
		else
			return fibonacci(n - 1) + fibonacci(n - 2);
	}
}
```

- ### non-recursive solution (iterative)

```java
public class App {
	
	public static void main(String[] args) {		
		if( fibonacci(0) == 0 &&
			fibonacci(1) == 1 &&
			fibonacci(2) == 1 &&
			fibonacci(3) == 2 &&
			fibonacci(4) == 3 &&
			fibonacci(5) == 5 &&
			fibonacci(6) == 8 ) {
			System.out.println("test passed.");
		}		
	}

	static int fibonacci(int n) {

		if (n == 0) {
			return 0;
		}
		if (n == 1) {
			return 1;
		}

		int previous = 0;
		int current = 1;

		for (int i = 2; i <= n; i++) {
			int newCurrent = previous + current;
			previous = current;
			current = newCurrent;
		}

		return current;
	}
}
```

- ## palindrome (yada tr:Palindrom)

tersten yazılışı aynı olan kelime, cümle veya sayılardır.

- ### recursive

```java
public class App {
	
	public static void main(String[] args) {				
		if( isPal("a")    &&
		    isPal("abba") && 
		    isPal("abcba") &&
		    !isPal("abcgba")) {
		        System.out.println("test passed.");
		}		
	}

	static boolean isPal(String s) {
	    if(s.length() < 2)
	        return true;
	    
	    if(s.charAt(0) == s.charAt(s.length()-1))
	        return isPal(   s.substring(1, s.length()-1)   );

	    else
	        return false;
	}
}
```

- ### non-recursive solution (iterative)

```java
public class App {
	
	public static void main(String[] args) {				
		if( isPal("a")     &&
		    isPal("abba")  && 
		    isPal("abcba") &&
		    !isPal("abcgba")) {
		        System.out.println("test passed.");
		}		
	}

	static boolean isPal(String s) {
		
		if(s.length() < 2)
	        return true;
		
		for (int i = 0; i < s.length(); i++) {

			if( ! ( s.charAt(i) == s.charAt(s.length()-i-1)  )) {
				return false;
			}
		}

		return true;
	}
}
```

- ## Cycle detection

- ### Floyd's Tortoise and Hare
	
  Floyd tarafından geliştirilen bu yöntemde kamlumba ve tavşan birer pointerdır.

	Aşağıdaki kod üzerinde Floyd  algoritması uygulanmış ve kaplumba=slow tavşan=fast olarak isimlendirilmiştir.

  Floyd algoritmasında, tavşan kaplumpağadan önce döngü içerisine girecektir ve döngü içinde birbirlerine muhakkak denk geleceklerdir. algoritmanın kodlaması çok basittir. algoritma çalışmasında ek belleğe ihtiyaç duyulmaz. sadece 2 pointer vardır.

  Aşağıda şekilde döngüsel linkedlist'imiz olduğunu varsayalım.
  - Çizgiler x uzunluğunda (x adet node olsun), 
  - eşittir işaretleri y uzunluğunda
  - yıldız z uzunluğunda

  Eğer tavşan ve kamplumbağa yıldızlı node'ların başında buluşuyorsa;
  - Distance travelled by slowPointer before meeting = x+y
  - Distance travelled by fastPointer before meeting = (x+y+z)+y = x + 2y + z

  Bu durumda;
    
  > 2 * dist(slowPointer) = dist(fastPointer)

  > x = z çıkmaktadır

  Slow ve fast buluştuğunda liste döngüsel demektir. Bunun sebebi yukarıdaki demklem değil. Yukarıdaki denklem ek olarak bizim döngüdeki döngünün başladığı noktayı bulmamızı şu şekilde sağlayacak: Eğer slowpointer'ı dizinin en başına koyarsak ve her iki pointer'ı da artık aynı hızda ilerletirsek, 2 pointer döngünün başladığı nokta olan kısımda (bizim şeklimizde eşittir işaretinin en başı) buluşacaklardır. çünkü x=z'dir. 

  Aşağıdaki java kodu sadece listenin döngüsel olup olmadığını tespit etmektedir.

```
    <...x...>                      <....y
________________======================  :
                *                    =  :
                *                    =  v
                *                    =
                **********************
                  <...z...>
```

```java
public class App {

	public static void main(String[] args) {
		Node ten = new Node("10");
		Node twenty = new Node("20");
		Node thirty = new Node("30");
		Node fourty = new Node("40");
		Node fifty = new Node("50");

		ten.next = twenty;
		twenty.next = thirty;
		thirty.next = fourty;
		fourty.next = fifty;
		
		// here is the cycle.
		// we point to first node.
		fifty.next = ten;

		if (isCircle(ten)) {
			System.out.println("test passed.");
		}
	}

	static boolean isCircle(Node first) {
    
		Node fast = first;
		Node slow = first;

		while(fast!= null && fast.next != null){
		  fast = fast.next.next;
		  slow = slow.next;
		  
		  if( slow == fast )
		     return true;
		}
		return false;
	}

	static class Node {
		Node next;
		String data;

		public Node(String data) {
			this.data = data;
		}
	}
}
```

- ## reverse list

- ### recursive

```java
public class App {

	public static void main(String[] args) {
		Node ten = new Node("10");
		Node twenty = new Node("20");
		Node thirty = new Node("30");
		Node fourty = new Node("40");
		Node fifty = new Node("50");

		ten.next = twenty;
		twenty.next = thirty;
		thirty.next = fourty;
		fourty.next = fifty;
		fifty.next = null;

		Node reversed = reverse(ten);
		if (reversed == fifty  &&
		    reversed.next == fourty &&
		    reversed.next.next == thirty &&
		    reversed.next.next.next == twenty && 
		    reversed.next.next.next.next == ten && 
		    reversed.next.next.next.next.next == null) {
			
			    System.out.println("test passed.");
		}
	}

	static Node reverse(Node node) {

		if (node.next == null) {
			return node;
		}

		Node newHead = reverse(node.next);

		node.next.next = node;
		node.next = null;
		
		return newHead;
	}

	static class Node {
		Node next;
		String data;

		public Node(String data) {
			this.data = data;
		}
	}
}
```

- ### non-recursive solution (iterative)
      Aşağıdaki algoritmada yeni bir liste oluşturmadan, varolan liste değiştirilerek liste tersine çevriliyor.
      Listede previous, current ve next elemenaları mevut. Liste sırası ile dönülüyor. Her döngüde current.next = prev yapılıyor. Daha sonrasında ise; prev, cur, next değerleri bir sonraki elemenlara referans edecek şekilde set ediliyor.

```java
public class App {

	public static void main(String[] args) {
		Node ten = new Node("10");
		Node twenty = new Node("20");
		Node thirty = new Node("30");
		Node fourty = new Node("40");
		Node fifty = new Node("50");

		ten.next = twenty;
		twenty.next = thirty;
		thirty.next = fourty;
		fourty.next = fifty;
		fifty.next = null;

		Node reversed = reverse(ten);
		if (reversed == fifty  &&
			reversed.next == fourty &&
			reversed.next.next == thirty &&
			reversed.next.next.next == twenty && 
			reversed.next.next.next.next == ten && 
			reversed.next.next.next.next.next == null) {
			
			    System.out.println("test passed.");
		}
	}

	static Node reverse(Node first) {
		Node prev = null; 
        Node current = first; 
        Node next = null; 
        while (current != null) { 
            next = current.next; 
            current.next = prev; 
            prev = current; 
            current = next; 
        } 
        first = prev; 
        return first;
	}

	static class Node {
		Node next;
		String data;

		public Node(String data) {
			this.data = data;
		}
	}
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# language server protocol (yada LSP)

Language server IDE'den bağımsız çalışan bir yazılımdır. ide (client) sunucuya (language server'a) bağlanır ve ona kodları yollar, kodları alan sunucu sürekli olarak client'e rapor verir. her ide için ayrı eklenti yazmaktansa, IDE'ler bu protokolü desteklediği sürece her dile destek verebilirler.

language server ile neredeyse herşey yapılabilir. örnek:  code completion, hover tooltips, jump-to-definition, find-references...

language server ile client'ın haberleştiği protokol LSP'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Google Cloud Platform

 - g suite
   - gmail
   - docs
   - keep
   - calendar
   - google
   - ...
 - Firebase : mobile uyglamaya (ios ve andorid) google firebase lib ekleniyor. lib google firebase servislerine bağlanıyor ve otomatik istatistik yolluyor.
   - Firebase Cloud Messaging (yada FCM): eski adı: Google Cloud Messaging (yada GCM) ilk kurulduğundaki ismi: Android Cloud to Device Messaging (yada Cloud to Device Messaging yada C2DM) 
 - compute
   - Compute Engine (remote VM)
   - App Engine (AWS Elastic Beanstalk alternatifi)
 - ...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# virtualbox içerisinde Resolution ayarlama
Çözünürlük ilk makina kuruldugunda küçük gelir. Arttirabilmek için "guest additions" eklentisi ziyaretçi makinaya kurulmalidir. guest restart edilir. Virtualbox --> görünüm --> "misafir ekranini otomatik yeniden boyutlandir" seçenegi, anlik olarak guest isletim sisteminin virtualbox penceresini kaplayacak sekilde genislemesini saglamaktadir.

# virt-manager içerisinde Resolution ayarlama
direk olarak masaüstü görüntü ayarları genişletilebilir. ek bir ayara gerek yok.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Virtualbox üzerinde dosya paylaşımı

- ISO file
dosyaları direk olarak mount edebiliyoruz. Bu sebeple bir dosyayi virtualmachine tasimak istedigimizde ISO dosyasi içerisine atmamiz yeterli olacaktir. ISO dosyası direk 7zip, Peazip gibi uygulamalarla oluşturulamıyor. Linux ve Windows için şu çözümler kullanılabilir:
- Windows üzerinde dosyalari ISO dosyasina atabilmek icin "cdrtfe" (portableapps.com formatinda da mevcut) uygulamasi kullanilabilir. "cdrtfe" açildiginda "options" butonuna basilir. "ISO image" bölümünden "create image only do not burn" enabled edilir.
- Linux üzerinde şu komut ile iso yaratılır: mkisofs -o /home/user/folder.iso /home/user/folder

- drag and drop
"guest additions" eklentisi ziyaretçi makinaya kurulmalidir. guest restart edilir. bu sekilde kopyalanan metinlerde, ziyaretçi-host arasinda paylasilabilir olmaktadir. Bu özelligi enable etmek için, General --> Advance kismindan enable edilmelidir.
Drag and drop ile klasör tasima islemlerinde sorun oldugu görüldü. Tek dosya tasimak gerekebilir.
Drag and drop ve kopyalama panosu paylasimi özellikleri virtualbox 5'inci sürümü öncesi stabil olmayan eklentilerle saglaniyordu. Fakat 5'inci sürüm ile birlikte stabil ve eklentisiz desteklenmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# hypertext
gerek aynı dökümanın içerisinde bir yere referans ederek, gerekse farklı bir dökümana işaret eden linkler içeren text dosyalarına/dökümanlarına verilen isimdir.

# hyperlink
"link" ile eşanlamlıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# nexus vs artifactory
birbirine alternatif maven, gradle, npm gibi birçok farklı altyapılar için merkezi dependency/artifact barındırırısıdır.
sonatype firması nexus'u, JFrog firması artifactory'yi geliştiriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# freemarker
açık kaynaklı java kütüphanesidir. template'leri model ile doldurup, çıktı yaratmaktadır.

IDE ve Text editorler'e kurulan "freemarker" eklentileri sadece syntax highlighting görevi görmektedirler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# remote desktop software comparison

# Remote Desktop Connection (yada RDC yada Remote Desktop yada (eski adı) Microsoft Terminal Services Client yada (eski adı) mstsc yada (eski adı) tsclient)
İsimleri çok genel bu yüzden karışıklıklara sebep olabilir. Microsoftun uzak masaüstü için kullandığı yazılımdır.

Client ve sunucu iletişiminde yine microsoft'un geliştirdiği Remote Desktop Protocol (RDP) kullanılmaktadır.

resmi mac, android, ios client versiyonları mevcuttur.

# Remmina
windows RDP'yi de destekleyen, linux için multi-protocol client.

# VNC
- Açık kaynaklı uzak masaüstü destekleyen bir protokoldür.
- Protokolü kullanan birden fazla ürün mevcuttur.
- VNC, teamviewer gibi ekran görüntüsünü paylaşmamaktadır. Sunucu tarafta sanal bir X session'u açmaktadır (bu __Xvnc__ oluyor). bu X session'ına client taraftan bağlanmayı sağlamaktadır.

# winvnc
VNC, x'e göre çalıştığından normal koşullarda ms windows ortamında çalışamaz. winvnc sadece ms windows için geliştirimiş bir vnc sunucu uygulamasıdır.

# x11vnc
standart VNC protokolü sadece uzak masaüstündeki sanal X11'e bağlanılabilmesini sağlıyor. fakat x11vnc; zaten açık olan X session'ına bağlanılmayı sağlıyor. x11vnc sadece server tarafındaki yazılımdır. herhangi bir vnc client'ta x11vnc'e bağlanabilir.

X11vnc ve vnc'nin yaptığını X zaten destekliyor. fakat vnc ve x11vnc bazı şeyleri disable ederek, network bağlantısında daha az data transferi üzerinden çalışmasını sağlıyor.

X11vnc projesinin geliştirilmesi 0.9.13 sürümünde durdu. daha sonra LibVNC github kullanıcısının, x11vnc reposunda forklanarak geliştirilmesi devam ettirilmektedir.

LibVNC kullanıcısının altındaki LibVNCServer ve LibVNCClient repoları cross-platform C kütüphaneleridir. Bu kütüphaneler ile vnc client veya vnc uygulaması geliştirilebilir.

# noVNC
VNC client using HTML5, WebSockets and Canvas.

# vnc kullanan uygulamalar ve diğer uygulamalar ile karşılaştırması:

| app name                                  | licence     | ms windows server | ms windows client | mac server | mac client | linux server | linux client | bsd server | bsd client | java client | android client | android server | ios client | ios server | chrome os client |
|-------------------------------------------|-------------|-------------------|-------------------|------------|------------|--------------|--------------|------------|------------|-------------|----------------|----------------|------------|------------|------------------|
| TightVNC                                  | GPL         | Yes               | Yes               | No         | Yes        | Yes          | Yes          | Yes        | Yes        | Yes         | Yes            | ?              | ?          | No         | ?                |
| TigerVNC                                  | GPL         | Yes               | Yes               | No         | Yes        | Yes          | Yes          | Yes        | Yes        | Yes         | No             | ?              | No         | No         | ?                |
| TurboVNC                                  | GPL         | No                | Yes               | No         | Yes        | Yes          | Yes          | Yes        | Yes        | Yes         | No             | ?              | No         | No         | ?                |
| UltraVNC                                  | GPL         | Yes               | Yes               | No         | No         | No           | No           | No         | No         | Yes         | No             | No             | No         | No         | ?                |
| X11vnc                                    | GPL         | No                | Yes               | Yes        | Yes        | Yes          | Yes          | ?          | Yes        | Yes         | ?              | ?              | ?          | No         | ?                |
| RealVNC Free                              | Proprietary | Yes               | Yes               | Yes        | Yes        | Yes          | Yes          | No         | Yes        | Yes         | Yes            | ?              | Yes        | No         | ?                |
| RealVNC Personal                          | Proprietary | Yes               | Yes               | Yes        | Yes        | Yes          | Yes          | No         | No         | Yes         | Yes            | ?              | Yes        | No         | ?                |
| RealVNC Enterprise                        | Proprietary | Yes               | Yes               | Yes        | Yes        | Yes          | Yes          | No         | No         | Yes         | Yes            | ?              | Yes        | No         | ?                |
| Remmina                                   | GPL         | No                | No                | No         | Yes        | No           | Yes          | No         | Yes        | No          | No             | ?              | No         | No         | ?                |
| Remote Desktop Services/Terminal Services | Proprietary | Yes               | Yes               | No         | Yes        | Yes          | Yes          | No         | Yes        | ?           | Yes            | ?              | Yes        | No         | ?                |
| SSH with X forwarding                     | GPL         | No                | Yes               | No         | Yes        | Yes          | Yes          | Yes        | Yes        | No          | Yes            | ?              | Yes        | No         | ?                |
| TeamViewer                                | Proprietary | Yes               | Yes               | Yes        | Yes        | Yes          | Yes          | No         | No         | Yes         | Yes            | Yes            | Yes        | No         | Yes              |
| Ammyy Admin                               | Proprietary | Yes               | Yes               | No         | No         | No           | No           | No         | No         | No          | No             | ?              | No         | No         | ?                |
| Apple Remote Desktop                      | Proprietary | No                | No                | Yes        | Yes        | ?            | No           | No         | No         | No          | No             | ?              | No         | No         | ?                |

# Teamviewer
mobile de bağlanılmasını sağlıyor.

birçok teamviewer uygulaması mevcut:
- Android markette "QuickSupport" uygulaması, mobile'e bağlantı yapılabilmesini sağlayan uygulama.
- "QS Add-on" prefixi ile başlayanlar QuickSupport uygulamasının yanına kurulan, her firma için ayrı ayrı logo ve hızlı bağlantı çeşitleri içeren uygulamalardır.
- Masaüstü versiyonlarda "QuickSupport" uygulaması kurulumsuz olarak başlayan ve sadece sunucu modunda açılan ayar sunmayan, iş yerleri müşterileri için hazırlanmış bir versiyondur.
- Android markette "Host" uygulaması android'e servis olarak kuruluyor. böylece uygulama açık değilken bile uzaktan bağlantıya izin veriyor.

# Chrome Remote Desktop
- masaüstünde chrome eklentisi olarak çalışıyor (sunucu modu dahi).
- mobile versiyon google chrome'a eklenti diil. android uygulaması.
- linux tarafında sunucu olması için bazı ayarlar yapmak gerekebiliyor.
- chromium tarayıcısında da çalışıyor.
- mobile'den masaüstü yönetiliyor. fakat mobil yönetilemiyor.
- google hesabı ile login zorunlu.

# Ammyy Admin
kullanılması tehlikeli derecede güvensiz bir uygulama.

ip'siz (id aracılığı ile) bağlanılabilmeyi sağlıyor.

windows harici sistemler için indirmesi ücretli.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cron
cron kelimesi yunanca xronos'tan gelmektedir.

"cron" unix tabanlı sistemlerde istenilen saatlerde çalışması istenen scriptleri çalıştıran yazılımın özel ismidir. ismi çok sıkça kullanıldığından; cronjob denildiğinde, herhangi bir platformda arkaplanda scheduled yürütülen job'lar anlamında kullanılır. oysa bu terimi diğer job'lar için kullanmak tamamen yanlıştır.

- cronjob
cron uygulaması birçok farklı scripti farklı zamanlarda çalıştırabilir. her başlatılan script birer cronjob'tır.

- crontab
"cron table" cron uygulamasının configurasyon dosyasıdır. bu dosya tablo gibidir. her satırda zaman ve o zamanda başlatılacak script yazmaktadır.

scheduler görevi gören birçok uygulama yazılmıştır. bazıları aynı veya benzer komut satırı ismiyle çağrılmaktadır. 

- tarih formatı

```
* * * * * *
| | | | | | 
| | | | | +-- Year              (range: 1900-3000)
| | | | +---- Day of the Week   (range: 1-7, 1 standing for Monday)
| | | +------ Month of the Year (range: 1-12)
| | +-------- Day of the Month  (range: 1-31)
| +---------- Hour              (range: 0-23)
+------------ Minute            (range: 0-59)
```

- özel karakterler

  - \*

    'her' anlamında kullanılır. örneğin, dakika sutununa yıldız koyarsak, her dakika anlamına gelir.

  - , 

    birden fazla değer koymak istediğimizde kullanılırız.

  - \-

    birden fazla değeri kullanmak için kullanırız. örnek: mon-fri, or 9-17 
  
  - */x

    'her n' anlamına kullanılır. örnek: her beş dakika için, dakika sutununa */5 yazılmalıdır.

- örnekler

```
* * * * * *                         Each minute

59 23 31 12 5 *                     One minute  before the end of year if the last day of the year is Friday
									
59 23 31 DEC Fri *                  Same as above (different notation)

45 17 7 6 * *                       Every  year, on June 7th at 17:45

45 17 7 6 * 2001,2002               Once a   year, on June 7th at 17:45, if the year is 2001 or 2002

0,15,30,45 0,6,12,18 1,15,31 * 1-5 *  At 00:00, 00:15, 00:30, 00:45, 06:00, 06:15, 06:30,
                                    06:45, 12:00, 12:15, 12:30, 12:45, 18:00, 18:15,
                                    18:30, 18:45, on 1st, 15th or  31st of each  month, but not on weekends

*/15 */6 1,15,31 * 1-5 *            Same as above (different notation)

0 12 * * 1-5 * (0 12 * * Mon-Fri *) At midday on weekdays

* * * 1,3,5,7,9,11 * *              Each minute in January,  March,  May, July, September, and November

1,2,3,5,20-25,30-35,59 23 31 12 * * On the  last day of year, at 23:01, 23:02, 23:03, 23:05,
                                    23:20, 23:21, 23:22, 23:23, 23:24, 23:25, 23:30,
                                    23:31, 23:32, 23:33, 23:34, 23:35, 23:59

0 9 1-7 * 1 *                       First Monday of each month, at 9 a.m.

0 0 1 * * *                         At midnight, on the first day of each month

* 0-11 * * *                        Each minute before midday

* * * 1,2,3 * *                     Each minute in January, February or March

* * * Jan,Feb,Mar * *               Same as above (different notation)

0 0 * * * *                         Daily at midnight

0 0 * * 3 *                         Each Wednesday at midnight
```

https://crontab.guru/ sayfası expression'a karşılık gelen zaman dilimlerini ekrana basıyor. buradan kontroller yapılabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Omnibox
Çok amaçlı adres çubuğudur. tarayıcılarda adres çubuğu sadece url'ye gitmek için değil, aynı zamanda arama yapma, matematik işlemleri yapma gibi birçok özellikte barındırır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# functional requirement

- yazılım/sistem ihtiyaç analizleri için kullanılan terimdir. 
- spesifik bir fonksiyonalite/özellik için yazılmış olması gereken ihtiyaca verilen sıfattır. örnek:
  - son kullanıcı formu yolla tuşuna ancak şifresini girdikten sonra basabilecektir.
  - ikinci sayfadan sonra direk dördüncü sayafaya geçiş mümkün olmalıdır.

# Non-functional requirement

bu ihtiyaçlar spesifik özellikler için belirtilmez. genel sisteme hitap eder. örnek:

- sistem 10.000 kişiyi destekleyecektir.
- yazılım güncel linux sistemlerini destekleyecektir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sistem Programcılığı
Daha çok işletim sistemi ile yada donanım bilgisi gerektiren yazılım dilleri ve kütüphaneleri kullanarak yazılım geliştirmedir. Bu sebeple; genelde üst seviyeli diller buna gruba dahil olmaz. Bu sebeple; genelde sürücü (driver) yazanlar bu gruba dahildir.

# sistem çağrıları (yada çekirdek çağrıları)
İşletim sistemi ile kullanıcı programları arasında tanımlı olan arayüz, işletim sistemi tarafından tanımlanan bir prosedürler kümesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# p2p (peer-to-peer yada eşlerarası)

genel bir terimdir. client'ların direk birbiri ile haberleşip transfer yaptığı, sunucunun aracılık yapmadığı protokollerdir. sunucu sadece client'ların birbirini görebilmelerini sağlamakla yükümlüdür. eğer client'lar biribinin network adreslerini biliyor ise, sunucuya hiç gerek kalmaz. 

BitTorrent p2p'ye en iyi implementasyon örneğidir.

# BitTorrent

BitTorrent protokolün ismidir. aynı zamanda BitTorrent firmasının geliştirdiği, BitTorrent yazılımı bu protokolü kullanan bir client'tır.

# torrent

paylaşımda (yada paylaşıma açık yada hazır) olan dosya yada dosyalar grubu için kullanılan terimdir.

# torrent file (yada METAINFO)

.torrent uzantılıdır.

file that contains metadata about files and folders to be distributed, and usually also a list of the network locations of trackers.

# Availability

Also known as "distributed copies". The number of full copies of a file. Each seed adds 1.0 to this number. eğer 1 kişide tam dosya ve 1 kişide yarım dosya varsa o dosya için Availability 1.5 olur.

# Seed (yada tohum)

seeder, dosyayı paylaşan toplam client sayısıdır.

# Leech (yada sülük)

Leecher dosyayı çeken toplam client sayısıdır.

# Peer (yada eş)

Seed ve Leech in toplamındır.

# Tracker

Torrentin bağlandığı sunucudur. Torrenti çeken Peerler ve Trackere dosya hakkında bilgi gönderirler. Diğer Peerler ise Trackere bağlanarak kimde hangi dosyanın hangi parçalarının olduğunu öğrenirler. Tracker üzerinden kesinlikle dosya transferi gerçekleşmez. sadece Kim nekadar dosya çekti ve ne kadar yükledi gibi İstatislik bilgilerinide barındırabilir.

# DHT (yada Distributed Hash Table)

tracker'dan bağımsız çalışabilmek gerektiğinde tracker gibi tüm istatistikleri toplamak gerekir. işte bu toplanan bilgilerin tümü DHT tablosunda toplanır.

# Ratio (yada oran)

torrent'in Upload işleminin Download işlemine oranıdır.

# Hit&run

Dosyayı çekip hemen paylaşımdan kaldırma işlemine ve kaldırana verilen isimdir. Torrent dünyasında kesinlikle sevilmezler.

# Choked

kelime anlamı: tıkanmış

diğer client'lar X client'ına dosyayı yollamıyorsa, X client'ı chocked olmuş olur.

# Lurker

dosyayı indiren ve paylaşan client'tır. fakat indirdiği dosya haricinde farklı bir dosya paylaşmayan client'tır.

# swarm

kelime anlamı: sürü

bir torrent'i paylaşan tüm client'lara verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# 3rd party software

terminolojik açıdan bakıldığında; developer ve müşteri 1 ve 2inci kişiler oluyor. 3 üncü kişiler ise 1 ve 2 dışında yazılım sağlayanlar oluyor. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# otonom (yada autonomous)
kendi kuraları/işleyişi olan. özerk sistem.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# tele

Yunanca "tele" (uzak, ırak) anlamına gelmektedir. bu kelime birçok bilişim kelimesi için ön ek olarak kullanılmaktadır. örnek: telecommunication, telegraf, telephone, television...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# awesome

- bir rozet (yada badge)'tir.

- "The awesome manifesto" yazılmıştır.

- en iyi kitaplar listesi, android open source uygulamalar listesi, c# için yazılmış ingilizce kitaplar listesi gibi koleksiyon sayfalarına bu etiketi her isteyen koyabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Input Form validasyonları

# Posta kodu (yada postal code)
Her ülkede farklı terminoloji kullanılmaktadır. bazı örnekler:

- amerika da ZIP Code
- hindistan da PIN Code (Postal Index Number'ın kısaltması)
- Brezilya da CEP
- İrlanda da Eircode

posta kodu'nun formatı ülkeden ülkeye değişiklik göstermektedir (bazı ülkelerde harf boşluk ve tire işareti dahi mevcuttur). bu sebeple; son kullanıcıya önce ülke seçimi yaptırılmalı ve posta kodu validasyonu devreye girmelidir.

türkiyede posta kodu mahalle'yi temsil etmektedir. fakat bazı ülkelerde, şehir/semt/mahalle kavramlarının seviyeleri farklı olduğundan, posta kodu farklı bir seviyeye denk gelebilir.

türkiyedeki seviyeler (büyükten küçüğe):
- il
- ilçe
- semt
- mahalle veya köy (aynı seviyedeler)

Bazı ülkeler, bazı posta kodlarını spesifik kurumların adresine veriyor. bunlar genelde devlet daireleri gibi kurumlar oluyor.

Genelde e-ticaret firmaları ülke bazlı kontrol yapıyor yada sadece karakter sayısını kontrol ediyor. adresin yanlış girilmesi sorumluluğu son kullanıcıya bırakılıyor.

# Administrative division (yada idari bölünme)
her ülke kendi içinde birçok alt bölge ayrmı içindedir. son kullanıcıya adres formu doldururken, eğer site uluslararası bir site ise (tüm dünyaya yada geniş bir alana hizmet veriyor ise) sadece ülke ve şehir bilgisi otomatik selectbox'tan tamamlanır. bir de her zaman son kullanıcıya adresi kendisi girmesi için textbox bırakılır. adres kullanıcının sorululuğundadır. çünkü her ülkeyinin tüm seviyelerinini otomatik selectbox'tan tamamlanması çok zordur. her ülkenin bilgisi dönem dönemde değişiyor. sürekli bu dataları güncel tutmak büyük maliyet. bazı kaynaklar bunları web servisi yada offline olarak sağlıyor, fakat tüm detayların doğru olduğundan hiçbir zaman %100 emin olamıyorsunuz.

eğer birkaç yada tek bir ülkeye hizmet veren bir yazılım geliştiriliyorsa, tüm seviyelerin kaynaklarını bulmak kolay oluyor. fakat tüm dünya için bu detaylar çok maliyetli.

state, region, province, city, country gibi terimlerin dahi karşılıkları her ülke dilinde olmayabiliyor.

tüm dünyanın veritabanı olsa dahi, her seviyenin isminin hangi dilde son kullanıya gösterileceği de önemli. türkçe olarak sitede gezip, adres formunda amerikayı seçtiğinizde her seviyenin türkçe çıkması gerekecektir. bunların kombinasyonu çok fazla.

sadece şehir/ülke kombinasyonu yeterlidir. Amazon gibi büyük firmalar bu şekilde yapıyorlarlar (yıl 2018).

# isim soyisim (full name)
- her ülkede farklı standartlar vardır.
- bazen nufusa yanlış yazılmış, fakat değiştirilmemiş karakterlerde çıkabiliyor. bu sebeple ülke bazlı dahi düzgün bir validasyon/regex mevcut değildir.
- yunan gibi alfabeler de hesaba katıldığında validasyon bulmak çok zor. bu sebeple bazı siteler validasyon yaparken sadece bazı karakterleri exclude ediyor. çünkü sadece kabul edilen karakterler çok fazla. 
- tüm karakterler kabul edildiğinde bu sefer işaret karakterleri de kabul edilmiş oluyor. bu durum yunanca gibi diller işin içine girince daha da zorlaşıyor.
- sayı(lar) içeren isimlerde mevcut.
- tek kural şu denebilir: isim ve soyisim en az birer karakterden oluşmak zorundadır. 
- standart olarak her zamanki gibi, XSS saldırıları için text'i denetlemek gerekir.

# yaş
- minimum yıl seçeneği çok eskiye dayanmalıdır. resmi olarak kayıtlarda ülkeye ölü bilgisi gelmeden o kişi ölü sayılamaz. ve sistemde kaydı açılması gerekiyor ise; bu kişinin doğum yılı çok eski tarih olabilir. e-devlet gibi sistemler düşünüldüğünde yaş validasyonunu yüksek tutmakta yarar var.

google, yahoo, apple gibi sitelerin kayıt sayfalarında 1865'ten itibaren doğum yılı validasyonları kabul ediliyor.

# telefon mobil numaraları
her ülke için değişmektedir.

https://github.com/google/libphonenumber projesi c++, java ve js için ortak repo altında kütüphane sunmaktadır. bu kütüphane ülke bazında validasyon sağlamaktadır. aynı standartları uygulayan diğer diller için "Third-party Ports" başlığında listelenmektedir: https://github.com/google/libphonenumber#third-party-ports

# email ve url
email ve url'ler global oldukları için çoğu yazılımcı tarafında elle regex tanımlanarak kontrol edilir. elle yazmak mantıklı, fakat ayrıntıya grilirse çok detay var:

- domainin uzantısı en az 2 karakter olmalı
- domainin uzantısı istenildiği kadar uzun olabilir ve sayı içerebilir
- domainin uzantısı sayı ile bitemez
- bazı URL bilgilerinde IP girilebiliyorsa, regex çok daha karışık olmaktadır (IP4 ve IP6 da unutulmamalı)
- emaillerde john@server.department.company.com gibi subdomain'ler olabilir

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# windows kitaplıkları

windows'un yeni sürümleri ile birlikte dosya yöneticisinde kitaplık özelliği geldi. bir kitaplık birden fazla dizini aynı klasörmüş gibi gösterebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# windows güncellemeleri

windows xp'den itibaren güncellemeleri toplu şekilde gruplandırarak "service pack" isminde ayrı setup'larda sunuyordu. tek tek güncelleme kurmak yerine bu tercih edilebilirdi.

windows 10 ile birlikte isimlendirme service pack olarak adlandırılmıyor. onun yerine piyasaya özel isimlerle sunuluyor. sırası ile 2018'e kadar aşağıdaki paketler sunuldu:

November Update

Anniversary Update

Creators Update

Fall Creators Update

April 2018 Update

October 2018 Update 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# type safe

programlama dilinin bir özelliğidir. eğer değişkenlerin tipleri belli edilmek zorunda ise o dil type-safedir. örneğin javada bir değişken oluşturulduğunda int, object, string olup olmadığı belirtilmek zorundadır bu yüzden java type-safe'dir. oysa javascript'te tüm değişkenler "var" ile belirtildiğinden type-safe değildir.

# Type Inference
tipin otomatik algılandığı dillerdir. bir objenin tipi dil tarafından otomatik algılanır ve ona göre tutulur. eğer ilerdeki kod satırlarında aynı referansa farklı bir tipte değer set etmeye çalışırsak runtime'da hata alırırız. kaynak: https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html başlık: "Type Safety and Type Inference" (archive date: 31/10/2019) 

# Static typing language vs Dynamically typing language
variable tiplerinin compile-time sırasında kontrol edildiği diller Statically typed dillerdir. tipler sadece run-time sırasında kontrol ediliyorsa Dynamically typed dildir.

her iki dilde de tip'ler vardır (type safety).

script dilleri genelde Dynamically typed'tır.

JS Dynamically typed'tır. fakat TypeScript Static typed'tır.

# static binding (yada early binding) vs dynamic binding (yada late binding).

Dog d1=new Dog();  --> static binding

Animal a=new Dog();  --> dynamic binding

# Compile Time Polymorphism vs Run Time Polymorphism

polimorfizm bir nesnenin farklı şekillerde davranabilmesidir. bu sebeple iki farklı polimorfizm gözlemlenebilir:

compile time --> Static binding veya Method overloading olan durumlarda geçerlidir

run time --> Dynamic binding veya Method overriding olan durumlarda geçerlidir

# Inheritance (yada kalıtım) vs polymorphism (yada polimorfizm)

kalıtım; sınıfların bir yada birden fazla yerden türeyebilme özelliğidir. polymorphism ise; türeyen sınıfların süper sınıfın metodlarını override edebilmesi yada bir metodun overloading yapabilmesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Bağlaşım (yada Coupling)

sınıfıların birbirine bağımlı olma durumlarıdır. X sınıfında değişiklik yapınca Y'de de değişiklik yapılması gerekiyor ise Y, X'ye bağlıdır.

# Gevşek bağlılık (yada Loosely Coupled) vs Sıkı bağlılık (yada Tightly Coupled)

sıkı bağlılık örnek:

```java
class Traveller {

    Car c = new Car();

    Public void startJourney() {

        c.move();

    }

}
```

gevşek bağlılığa örnek:

```java
class Traveller {

    Arac c; //burada injection şart

    Public void startJourney() {

        c.move();

    }

}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# debounce
bounce kelime anlamı: sıçramak
debounce kelime anlamı: sıçramayı engellemek

bir butona bastığımızda (yada bir event olduğunda) genelde bir metod tetiklenir.  eğer bu event üstüste sürekli gerçekleşirse her defasında bu eventi yapmak yerine 3 saniyede bir yapmayı tercih ederiz. işte bu yapacağımız yöntem ile sıçramayı engellemiş oluruz.

örnek:

> var debouncedMethod = API.debounce(myMethod, 3);

artık debouncedMethod'i üstüte çağırırsak max 3 saniyede bir execute edilecektir. yani üstüste çağırmalarımız ignore edilecek.

Yukarıda API bir kütüphaneyi temsilen yazıldı. örneğin lodash'te debounce metodu vardır. her proramlama dili için debounce sağlanabilir.

debounce kullanım alanına örnekler:

- web syafasındaki form için post butonu event'i

- GUI'mizin resize olduğundaki event. her snaiye tekrar render yerine, 2 saniyede 1 render etmek cpu'yu daha az yoracaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Automatic content recognition (yada ACR)

media yada belge dosyalarının otomatik tanınması teknolojisidir.

# acoustic fingerprinting

ses dosyalarının tanınması için tasarlanan teknolojilerdir. ACR'nin bir alt kümesidir.

# video fingerprinting

video dosyalarının tanınması için tasarlanan teknolojilerdir. ACR'nin bir alt kümesidir.

# Watermark (yada filigram)

fiziksel ve digital olarak ikiye bölünür.

fiziksel olan: örneğin paralarda sadece ışığa tutulduğunda görünen bir şekil vardır bu şekilde o paraya id atanabilir. fakat normal kullanımda kullanıcı bunu görmez/bilmez.

benzer şekilde digital dünyadada insan kulağının duymayacağı şekilde her ses dosyasına bir ses yerleştirilir. bu sesi ancak proramatik olarak tespit edebiliriz. bu şekilde o ses dosyasını unique yapmış oluruz.

# AcoustID

açık kaynaklı acoustic fingerprinting teknolojisidir.

# Chromaprint

AcoustID için client side C kütüphanesidir. AcoustID fingerprint'i üretir. AcoustID, projenin ismi olarak kullanılmaktadır.

# acoustic fingerprinting temel çalışma prensibi

her acoustic fingerprinting farklı teknikler kullanmaktadır. fakat temel olarak işleyi birçoğunda aynıdır.

bir ses dosyasının acustic fingerprint'i uzunca bir string'dir. MD5 ve SHA'nın tersine, ses dosyasındaki ufak değişiklikler accoustic fingerprint çıktısında ufak değişiklikler yaratmaktadır. tabi bu da tölere edilebilir bir durumdur. örneğin bir web servsine Fingerprint attığımızda en yakın fingerprint'ler benzerlik oranları ile dönmektedir: 

```
{

    results =     (

                {

            id = "3fb2e35c-c4d5-4360-aaff-8dad0aad05e9";

            score = "0.976727";

        },

                {

            id = "b262c597-64b3-42c4-94b0-e89b8c496d76";

            score = "0.9310389999999999";

        },

                {

            id = "49f64d53-de52-4f4b-a08d-8d78ca0001cf";

            score = "0.690862";

        }

    )

}
```

Fingerprint'in %100 benzeşmesi zaten istenmeyen bir durumdur.

Fingerprint hesaplamaları ses çıktısı üzerinden yapılır, ses dosyası formatı ile bağlantılı bir durum diildir.

Önrğin bir ses dosyası olsun. Her 2 saniyesinin sonundaki ses anından bir bilgi üretilsin. bu bilgi 2 sn aralıklarla tutulmuş oluyor. AYnı şarkının farklı bir ses kaydında yada bir konserde, orjinal şarkı kaydı ile aynı tempoda gidilmeyeceği için kontrol edilen şarkı 1'er saniye delay yapılarak tekrar kontrol edililiyor. 

Müzik veritabanları incelendiğinde (public ve ticari olanlarda) veritabanında aynı şarkının birden fazla fingerprint'i olduğu görülür. bunun sebebi şudur: orjinal kayıt sanatçının kayıt/media/prodüksyon şirketi tarafından kaydedilir. 1 fingerprint o zaman oluşturulur. bu şirket hesabı onaylanmamış hesap olabilir. bu gibi bir durumda  bir ses kaydından fingerprint oluşturulup o şarkıya yollayan biri olabilir. yada konser versiyonunu fingerprint olarak o şarkıya atayan olabilir. bazı insanlar ise; bir şarkının konser yada remix versiyonunu  şarkı oarak kaydetmektedir. Bazı şarkılar  albümlerin içinde de gelmektedir, yada bir film müziği DVD'si içerisinde sountrack hediye olarak gelmektedir. onları da  şarkı olarak kaydeden olmaktadır.

Eski klasik müziklerde iş karmaşıklaşmaktadır. Bir semfoniyi komple bir şarkı olarak kaydedenler mevcuttur. Semfoninin ilk 3 ile 5inci bölümünü bir şarkı olarak kaydedenler vardır. bir semfoninin 4üncü bölümünü içeren bir ses dosyasını Chromaprint'e verdiğimizde bize tüm olasılıkları döndürecektir.

# Gracenote

müzik veritabanını dışarıya ücretli API ile sunan bir cloud web servis firması.

# MusicBrainz

açık kaynaklı müzik veritabanı.

# beets

açık kaynaklı müzik arşivi düzenleme programıdır.

# Picard

açık kaynaklı GUI kullanan AcoustID ile MusicBrainz foundution'dan local müzik dosyalarlının tag'leyen uygulama.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Android Studio ve IntelliJ Idea için "build" vs "Make" vs "rebuild"

aşağıdaki işlemler project veya module için sunuluyor. project explorer'da ne seçili ise, aşağıdaki seçenekler seçili olan seviye (proje yada modül) için çalıştırılıyor.

- "android studio: Make" "intellij idea: build" ikisinin sadece menüdeki isimleri farklı. arkada aynı görevi işletiyorlar.

  compile + depend olan kodları/modülleri de compile eder. fakat sadece değişik yapılan sınıfları build eder. bu sebeple path, paket ismi, lib gibi değişikliklerde "rebuild" yapılmalıdır. "make" isminden de anlaşıldığı gibi executable yaratmak için kullanır. zaten bu sebeple depend olan projeleri de birlikte derler.

- clean

  her koşulda target dizinlerini siler.

- Rebuild

  her koşulda herşeyi tekrardan build eder. öncesinde clean'i çağırır. artifact oluşturmaz.

- compile

  sadece bir class yada paket(class grubu) seçili iken menüde görülen bir seçenektir. sadece ilgili(seçili) paketlerin/sınıfların içindeki kodları compile eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Semantic Versioning (yada Semver)

yazılım sürüm atlama kurallarıdır.

MAJOR.MINOR.PATCH şeklinde gider.

- Major: public API'lerimizde geri uyumlu olmayan geliştirmeler yapıldığında bu sürüm atlanır.

- Minor: public API'lere sadece eklemeler yapıldığında yada public API'ler değişmeden varolan davranışlar değiştiğinde bu sürüm atlanır

- Patch (yada tr:yama): public API'lerde değişiklik olmadan çıkılan bugfix'lerde bu atlanır

Bazı firmalar yukarıdaki kurallara uymasada bu kurallarla resmi makale mevcuttur.

Yukarıda bahsedilen public API bir çok katman olabilir: örneğin komut satırından kullanırken ki komut parametreleri, web tarayıcısı için eklentilere sunduğu API, veya her ikisi birden olabilir.

Sayısal artış matematiktekinin tersinedir. yani; 1.9.0 < 1.10.0 'dır.

0.y.z sürümünde public API ile sunulan şeyler pek stabil değildir. 1.0.0 ile artık production ortamına geçilmelidir ve public API kullanılabilir bir stabilitede olmalıdır.

# Romantic Versioning (yada RomVer)

HUMAN.MAJOR.MINOR şeklinde gider.

Semver daha çok son kullanıcısı teknik olan ve teknik işlerde kullanılan yazılımlarda kullanılmaktadır. Oysa romver daha son yazılım geliştirici olmayanlar için tasarlanmış yazılımlarda kulanılmaktadır.

Romverde Public API'nin yerini GUI almaktadır.

- Human: son kullanıcıyı ilgilendiren büyük değişikliklerde artar. örnek GUI.

- Major: eskisi ile aynı şekilde kullanılmayan özellikler içeriyorsa bu sürüm artar. örneğin menülerinin yerlerinin değişmesi. yani kullanım kılavuzunun değişmesi gerekeceği durumlarda bu sürüm artar. 

- Minor: GUI'de değişiklik olmadan çıkılan özellikler ve bugfix'ler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# atom

electron tabanlı text editor.

# atom ide

atom'a ekstra ide prefix/suffix'li eklentiler kurularak atom ide halini alıyor.

# Nuclide IDE

atom tabanlı fakat paketlerin kurulu geldiği ve silinemediği bir ide. react-native projeleri için geliştirilmiştir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jenkins

en üst seviyede "item"'lar tanımlarız. item'ların çeşitleri:

- project (yada eski adı:job): tanımlamarımızı yürütecek olan süreçtir.

- pipeline: build sürecinin her defasında dinamik olarak build edildiğinde tanımlabilen bir süreçtir. build'a başlamadan "Jenkinsfile" isimli dosyayı okur ve içindkeilere göre build yapar.

- folder: diğer item'ları bir diinde toplayabilmemizi sağlar.

- external job: jendkinsden bağımsız process'lerin takibi için yapılmış özel bir durum

- multibranch pipeline: bu bir plug-in'in getirdiği item'dır. default gelmez. çekilen kod'un her branch'i için ayrı ayrı Jenkinsfile'ı çalıştırır.

jenkins içinde bazı eklentiler varsayılan olarak yüklü gelir.

Her item "build" edilir.

Yeni bir item oluşturduğumuzda; bizden item'ın çeşidini seçmemiz istenir. buradaki çeşitler default mevcuttur bazıları ise jenkins'e eklediğimiz plug-in'ler tarafından sağlanır. Bazı item çeşitlerinin isminde Project keywordü yer alır. bu tamamen ismin bir parçasıdır. terminolojide project denilen bir kelime yoktur.

Bir item'ı seçtiğimizde ise yeni bir sayfada birçok ayar istenir. bu ayarlar gruplandırılışmıtır. komple bir grup bir jendkins eklentisi tarafından sağlanıyor olabilir. fakat default sunulan gruplarada eklentiler müdahele edebilir. eklentiler default grupların içine yeni form alanları ekleyebilir. bir form alanının sağ tarafında "?" simgesi vardır. ona tıkladığımızda o alan için kısa bilgi çıkar ve hangi eklentinin bu alanı buraya koyduğunu yazar. Eğer ? simgesi yoksa o alan default jenkins ile geliyordur.

Hangi grupların ve hangi form alanlarının gösterileceği, item'ın çeşidine göre değişir. item oluşturulduktan sonra item'ın çeşidini değiştiremeyiz.

# Agent

jenkins master'a, agent'lar bağlanır ve bu agent'larda taskları koşabiliriz.

# node

master ve agent'ların her birine verilen genel isimdir. bir node, agent yada master olabilir.

# Stage

pipeline (türkçe kelime anlamı: boru hattı) içindeki her iş grubuna verilen isimdir. örnek pipeline dosyamız ve içindeki stage'ler:

```js
pipeline {

    agent any 

    stages {

        stage('Build') { 

            steps {

                echo "build started"

                sh 'cd buil_dir'

            }

        }

        stage('Test') { 

            steps {

                sh 'javac soruce_dir'

            }

        }

        stage('Deploy') { 

            steps {

                sh 'docker push *file.jar'

            }

        }

    }

}
```

# step

pieline stage'sinin içindeki adımların her biri.

# Upstream

bir item diğer bir item'ı tetikleyebilir. tetikleyen item'a, tetiklenen item'ın upstream'i denir. tabi tetiklenen projede downstream oluyor.

# view

tüm item'ların listelendiği anasayfada item'lar gruplandırılabiliyor. her gruba "view" adı veriliyor. 

# workspace directory

her item için bir dizin vardır. her build aynı dizin üzerinde işlem yapar. eğer "workspace temizleme" aktif edilmezse eski build'deki dosyalar sürekli orada kalır.

# build directory

her build bittğinde metadalarını (log, baslangıç bitiş saat bilgisi, istatistik bilgisi ve artifact'ları) build dizini içine kopyalar.

# JENKINS_HOME

- config.xml

- plugins/

- workspace/

  - [itemName]

- jobs/

  - [itemName]

    - config.xml   (job configuration file)

    - latest       (symbolic link to the last successful build)

    - builds/

    - [buildId]/

Bazı jenkins sürümlerinde workspace dizini her JENKINS_HOME/jobs/[itemName]/'in altındadır.

# Pipeline types

iki çeşit Pipeline var:

- Scripted: Groovy dili ile yazılmış dosyadan okuyan pipeline'lardır.

- declarative: özel üst seviyeli bir dil (arkaplanda groovy kullanıyor). Scripted'a göre daha üst seviyeli olduğundan, Scripted'a göre daha kolay fakat daha az özellik sunuyor.

# jenkins'i docker ile kurup job içinde docker kullanma
"docker içinde docker" konusuna girmekteyiz. bunun için, container'ımız dışındaki dockerd'ye bağlanmamız gerekli. bunun için container'ımızı açarken bağlama işleminin bu şekilde yapabiliriz:

> docker container run -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock jenkins-docker

jenkins-docker içerisinde docker client'ın kurulu olması gerekli. artık jennkins joblarımızda, dockerd'ye komut yollayabiliriz.

"/var/run/docker.sock" bir unix socket dosyasıdır. dockerd sürekli buradan dinleme yapar ve buradan gelen komutları kabul eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# GitLab

Continous integration, bug, repository hizmetlerinin sunucusu yönetimi sunan bir web yazılımıdır.

- ## ana menüler

- İlk açıldığında "Project" ve "Groups" sekmeleri karşımıza çıkar. "groups" projelerin gruplandırılmış halini içerir. projects ise direk projeleri (gruplamadan) gösterir.

  her repository (yani proje), repository-url'sinin önüne grup ve sub-group adını otomatik prefix olarak gelmektedir.

- "Activity" sekmesinde tüm projelerdkei her komiti bir arada listeleyebiliyoruz.

- "Milestones" sekmesindeki listenin her elemanı bir task grubunu kapsaması için yaratılmıştır. örnek: OnlineÖdemeler ismini verdiğimiz milestonumuz olsun. bunun altına yine GitLab'den açacağımız taskları bağlarız. Bu şekilde taksların yüzde kaçının bittiğini, her task ile bağlantılı olan kaç komit atıldığını vs istatistiksel olarak takip edebiliriz. Milestone, jira'daki Spring kavramına çok benzemektedir.

- ## alt menüler

Her projenin altında o projeye ait wiki, issues, CI/CD(Continous development), Settings, Merge Request, repository sekmeleri bulunur. bu başlıkta sadece "issues" in altındakileri inceleyeceğiz:

issues sekmesinin altında;

- list: tüm issue'lar listelenir

- board: issue'ler grup halinde listelenir: kapanmışlar, backlog'dakiler vs..

- milestones: (yukarıda anlatılıyor)

- labels: taskalara/issue'lara atanmak için yaratılırlar. issue'lara keyword atamak için kullanılır. eksta bir özelliği yok.

# roller

sadece bunlar var: Guest , Reporter, Developer, Master, Owner

her rol için detaylı bilgi burada yazıyor: https://docs.gitlab.com/ee/user/permissions.html

temel olarak;

- Guest: yeni issue açabiliyor + kod hariç herşeyi sadece takip edebiliyor (wiki, diğer issue'lar, artifacts, job'lar) 

- Reporter: guest + issue assign + label management + merge request assign gibi daha yönetimsel işleri yapabiliyor + ek olarak proje kodunu da görebiliyor.

- Developer: reporter + milestone yönetimi + kod gönderme isteği + wiki yazma + CI/DI için sadece stop-start gibi yetkilere sahip

- Master: Developer + user management + CI/DI yazma

- Owner: Master + remove everything

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# bootstrap kelime anlamı

sözlük anlamı: çizme (ayakabı) atkısı (üst kısmıdır)

bilişim dünyasında farklı anlamda kullanılmaktadır. direk örnekler üzerinden gidersek; 

- web sitemizde bootstrapper index.html dosyamızdır.

- bootstrap loader bir sistemi başlatan yazılım yada yazılımın ilk kodlarıdır. bootstrap loader; web sitemizi başlatacak olan framework/sunucu yazılımızıdır.

Bootstrapping; terimi ise yukarıdaki 2 maddeden bağımsızdır. Bootstrapping; bir sistemi kendi sisteminizle geliştirmenizdir. örneğin; C programlama dili'nin yeni versiyonları yine C dili ile geliştirilmektedir. bu geliştirme sürecine bootstraping denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ITIL (yada Information Technology Infrastructure Library)

İngiltere Ticaret Bakanlığı tarafından geliştirilmiştir. ITIL kitaplar şeklinde iş dünyası için bilişim altyapısı standartları açıklamaktadır. BU stanardtalar dünyanın birçok firması tarafından özellikle uyulmakta ve takip edilmektedir. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Dependency hell

bu terim genelde Dependency'lerden kaynaklı sorunlar olduğunda kullanılır. bazen platfrom spesifik terimler kullanılır. örnek: jar hell, dll hell gibi.

Dependency hell birkaç çeşit formda karşımıza çıkabilir:

- Long chains of dependencies. muhtemel çözüm: microservisler. bir projeyi farklı executable paketlerle kullanılabilir halde ayırmak. belki portable uygulamacıklar sunmak.

- too many dependencies. muhtemel çözüm: microservisler. bir projeyi farklı executable paketlerle kullanılabilir halde ayırmak. belki portable uygulamacıklar sunmak.

- yürüteceğimiz bir programın bir bağımlılığı diğer yürüteceğimiz bir programın bağımlılığı ile sürüm çakışması yaşamaktadır. örnek: x proramı Dependency-A'yı max 3 sürümü kabul ederken, Y programı min 4 kabul etmektedir. muhtemel çözüm: container (örnek docker)

- cyclic dependencies: birbirine depend eden projeler. muhtemel çözüm: için projeleri birleştirmek yada ortak kodları başka üçünkü bir projeye taşımaktır.

# tree vs flat Dependency store management

nix ve yarn paket yöneticileri paketleri/bağımlılıkları bir dizinde tutar. buna store biçimine flat adı verilir. bu dizinde X paketi olsun. X paketinin bağımlılıkalrı X dizini içinde dğeildir. X ile aynı dizindedir. bu paket yönetim sistemi daha az yer kaplar. çünkü ortak bağımlılıklar ortak dizine referans eder.

tree'de ise her paket/Dependency kendi Dependency'lerini kendi içinde barındırır. ağaç şeklindedir. bu yöntem flat'e göre daha çok yer kaplar. fakat paketler taşınabilirdir. bir paketi başka sisteme kopyalayıp kullanabilirsiniz. çünkü herşey kendi içindedir. npm tree yöntemini kullanır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# declaration (yada beyan yada bildiri) vs definition (yada tanımlama)

fonksiyon/variable imlazalarının kodlandığı yerde declaration yapılmış olur. oysa bunlara değer atama, metodu implemente etme (body kısmını yazma) gibi kodlarının yapıldığı yerde definition yapılmış olur.

örnek:

> int PI; //declaration

> PI = 3; //definition

```java
int function topla(int a, int b); //declaration

topla = function(int a, int b){

      return a+b;
}
```

declaration ve definition aynı kod bloğunda da olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# truncate output

truncate türkçe kelime anlamı: kesmek, ayırmak.

bir komut satırı çıktısının sadece bir kısmını göstermek anlamında kullanılır. örneğin pi sayısını (3.123456789...) truncate edip göstereceksek 3.124 gibi yuvarlayamayız. onun yerine sadece bir kısmını göstermemiz gerekir: 3.123. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# accessibility (yada erişilebilirlik) destekleyen yazılımlar

kullanıcıların birçok çeşit engeli olabilir:

- renk körlüğü (yada Color blindness yada color vision deficiency yada gr:Αχρωματοψία)
  
  çeşitleri:
  - renkleri farklı görme: mavi yerine pembe gibi
  - sadece siyah beyaz görme
  - renkleri farklı yada doğru gören insanların bazıları renklerin farklı tonlarını algılayamamaktadırlar
  
  birçok insan renk körü olduğunu bile bilmiyor. bu sebeple renklere bağlı seçim yaptırırken dikkat edilmelidir. 

- hiç görememe

- duyamayanlar

- fareyi kullanamama

tüm engelliler klavyeyi kullanabiliyor.

dikkat edilmesi gereken bazı noktalar:

- html'de medya dosyalarına (video, resim, ses gibi) alt="media aciklamasi" atanmalıdır. göremeyenlere resimler gösterilmediğinde bu metin ekrana basılıyor, veya göremeyenler için bu metin okunuyor. bu metinlerde multi-language desteği olmalıdır.

- yazılımdaki tüm metinlerde kısaltmalar az ve hatta mümkünse hiç olmamalı. örnek 11 Feb 2017. Feb yerine February yazmak lazım. metin okuma programları kısaltmaları tanımlayamayabilir.

- standartlara uymak gerekli. örneğin html'de h1, h2, p gibi tagler çok önemli. sayfanın yapısı tarayıcı tarafından algılanabilmeli.

- metinler ve resimler net olmalı. ve ufak boyutta olmamalı. hiçbir obje ufak olmamalı. örneğin chechbox bile ufak olmamalı. karmaşık fontlar kullanılmamalı.

- linklerde title'ı mutlaka kullanmalıyız. örnek:

```html
<p>İşyerlerine başvuru için formu doldurmanız gerekli. <a href="form.html" title="İşyerine başvuru için tıklayın.">İşyerine başvuru için tıklayın.</a>  </p>
```

- form'da her eleman için "label" olmalı. form alanı doldururken buradaki bilgi ilişkili olduğu için engelli kişi için tanımlayıcı oluyor. örnek:

```html
<form>
    <label for="yourName">Your Name</label>
    <input name="yourName" id="yourName">
    <!-- etc. -->
```

- form elemanlarını mümkünse gruplamalıyız:

```html
<form action="somescript.php" >
    <fieldset>
        <legend>Name</legend>
        <p>First name <input name="firstName"></p>
        <p>Last name <input name="lastName"></p>
    </fieldset>
    <fieldset>
        <legend>Address</legend>
        <p>Address <textarea name="address"></textarea></p>
        <p>Postal code <input name="postcode"></p>
    </fieldset>
    <!-- etc. -->
```

- penceredeki elemanlara "Access Keys" atamaları yapılmalıdır.

- tema ve renkler olabildiğince flat olmalı. bu şekilde resimlerin tonlarını algılamayanlar için sorunlar ortadan kalkacaktır.

- sadece ses efekti ile uyarı yapıldığında, paralel olarak bildirim de yapılmalıdır.

- tarayıcılarda accesibility açıkken farklı css'ler kullanılabilir. bu şekilde çözüm üretilemeyen durumlarda, gerekirse bambaşka bir sayfa gösterilebilir. 

- bazen içeriklere hızlı erişim için ayrı ayrı linkler konulmalıdır. örnek:

```html
<header>
    <h1>The Heading</h1>
    <a href="#content">Skip to content</a>
</header> 

<nav>
    <!--loads of navigation stuff -->
</nav>

<section id="content">
    <!--lovely content -->
</section>
```

bunlar'ı css ile accesibility kapalı ise gösrmemeliyiz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# makefile

linuxta genelde program derlemek için önce "configure" komutu çağrılır. configure komutu o proje dizinindeki bir scirpt'in ismidir. root klasöründe olan bu script dosyasının görevi projenin derlenebilmesi için ön-hazırlık ve kontrollerin yapılmasıdır. örnek: işletim sisteminde projeyi derlemek için gerekli program var mı tespiti yapılmaktadır. örnek: javac (java compiler) var mı?

daha sonra "make" komutu çalıştırılır. make komutu bir program. örneğin "gnu make" açık kaynaklı bir program. işletim sisteminde kurulu olması gerekli. birçok make programı mevcut. make proramı varsayılan olarak projenin root'undaki "makefile" dosyasını okur ve bu dosya içindeki tanımlara göre projeyi derler. makefile dosyası programlama dilinden bağımsızdır. ant-script/gradle gibi tanımlar içerir.

en sondaki ise "make install" komutu çalıştırılır. bu komut derlenmiş olan projenin işletim sistemine kurulmasını sağlar. örneğin output dizinindeki bazı dosyaları user $PATH'ine kopyalar gibi... 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# handler vs listener

aralarında çok ince bir fark var. listener kaynağı dinler ve kaynak aksiyon aldığında handler'ı tetikler. handler ise event tetiklenince ne yapılacağına karar verir. şöyle düşünebiliriz:

```java
button.setOnClickListener( OnClickListener { 

    onClick() { 

        ...

    }

});
```

yukarıdaki kodda; OnClickListener bir listener, onclick metodu ise bir handlerdır. farklı bir örnek:

```java
button.addEventListener('click', function() {

   ...

});
```

yukarıdaki addEventListener metodu aslında bizim için kendi içinde listener oluşturuyor. bizden sadece handlerı parametre istiyor (anonym metodumuz - 2inci parametre).

Aslında; listener'da birşey tarafından tetikleniyor. bi bakıma listener'da bir handler olmuş oluyor. bu sebeple olaya nereden baktığımız önem kazanıyor. örneğin; bir java programında, proragmcı için listener; jvm'in işletim sistemi ile arasındaki kod/proram parçacığıdır. handler ise; programcının parametre yolladığı sınıftır. şimdi ise jvm açısından olaya bakalım: jvm için listener; işletim sisteminin event mekanizmasıdır, oysa handler jvm'in işletim sistemine event gerçekleştiğinde çalıştırması için gönderdiği kod/program parçacığıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# regular expression (yada regex yada reg-ex yada regexp yada rational expression)

arama yapabilmek için karakterlerden oluşan özel bir ifade dilidir.

birçok implementasyonu vardır. örnek: BRE (yada Basic Regular Expressions), ERE (yada Extended Regular Expressions), SRE (yada Simple Regular Expressions) 

# regex kullanımı
| regex          | what it returns                                                                                                                                                                                                                                            |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ^merhaba       | starts with "merhaba"                                                                                                                                                                                                                                      |
| merhaba yusuf$ | ends with "merhaba yusuf"                                                                                                                                                                                                                                  |
| merhaba        | contains "merhaba"                                                                                                                                                                                                                                         |
| ab#            | a, ac, abc, abb, abbc döndürebilir. a kesin var. b'den istenildiği kadar olabilir. # karakteri sadece bir önceki karakter için bir kuralı kapsar. eğer bir önceki karakter birden fazla karakter olacak ise; a(bc)# olarak parantez içinde gruplandırılır. |
| a(bc)#         | a, abc, abcbc, abce döndürebilir.                                                                                                                                                                                                                          |
| ab+            | # karakteri ile aynıdır. tek farkı en az 1 adet b olmalıdır. ab, abb, abc                                                                                                                                                                                  |
| ab?            | # karakteri ile aynıdır. tek farkı en fazla 1 adet b olmalıdır. a, ab, ac                                                                                                                                                                                  |
| a?b+$          | ab, b, bb, abb, abbb dolar ile bittiğini belirttiğimizden en sağa c gibi farklı karakterler gelemez                                                                                                                                                        |
| ab{2}          | {} # gibi sadece bir önceki karakter için geçerli bir kural belirtir. {2} 2 tane b olması gerektiğini belirtiyor. abb, abbc, abbc                                                                                                                          |
| ab{2,}         | en az 2 adet b olmalıdır. abbb, abb, abbc                                                                                                                                                                                                                  |
| ab{3,5}        | en az 3 en fazla 5 adet b olabilir. abbbc, abbb                                                                                                                                                                                                            |
| hi∣hello       | hi veya hello içerenler döner.                                                                                                                                                                                                                             |
| (b∣cd)ef       | bef yada cdef içeren textler döner                                                                                                                                                                                                                         |
| [ab]           | [] sadece bir karakteri temsil ediyor. (a∣b) ile aynı şeydir.                                                                                                                                                                                              |
| [a-d]          | [abcd] ve (a∣b∣c∣d) ile aynı şey.                                                                                                                                                                                                                          |
| [a-zA-Z]       | regex aksi belirtilmedikçe case sensitive'dir. bu sebeple büyük küçük tüm alfabeyi kapsatır.                                                                                                                                                               |
| [0-9]          | sadece rakam olduğunu belirtir.                                                                                                                                                                                                                            |
| [a-zA-E0-8]    | ufak karakterler tüm alfabe, büyük karakterlerde a'dan e'ye kadar olan karakterler, sayılarda ise 0'dan 8'e kadra olan karakterler                                                                                                                         |
| a.b            | . bir karakteri temsil ediyor. acb, abb                                                                                                                                                                                                                    |
| ..             | en az 2 karakter olan değerler döner                                                                                                                                                                                                                       |

özel karakterler kullanılacak ise öncesinde backslash "\" kullanılmalıdır.

# javascript'te regex

```js
var myRegexObject = /hello world/i;

var sonuc = "hello yusuf".search(myRegexObject);
```

/i yerine aşağıdakiler kullanılabilir:

- /i

Perform case-insensitive matching

- /g

Perform a global match (find all matches rather than stopping after the first match)

- /m

Perform multiline matching

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# synchronization (yada senkronizasyon yada eşleme)

eşgüdümleme kelimesi eş anlamlı değildir. eşgüdümleme koordine etme, işbirliği içinde hareket ettirme anlamına gelmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# string vs char[] for passwords (on java)

şifreler programda string değil char[] tutulması önerilir. bunun birkaç sebebi var:

- program exception olup kapanırsa işletim sistemi variable'ların dump'ını bi yere atar. daha sonra birileri bunu okursa, string değeri okunabiliken, char[]'in değeri okunamaz. çünkü char[]'in toString()'i yoktur.

- char[]'in toString()'i olmadığından; biri yankışlıkla log atarsa log("" + password); ekranda şifre görülmeyecektir. biri direk şifreyi ekrana basmaz fakat obje içinde obje olduğunda bazen taşınma işlemlerinde yanlışlıkla log basılmış olabilir.

- javax.swing kütüphanesinin JPasswordField sınıfı getText() metodu artık char[] dönüyor (eskiden string dönüyordu). bu sebepten böyle düzeltildi.

- Strings'ler immutable olduklarından referansları olmasa bile garbage collector temizleyene kadar ram'de tutulurlar. eğer uygulama hacklenirse ram'den okuma yapacak olan hacker bu değerleri görebilir. string ve char[], ram'den aynı şekilde okunur, fakat bu maddede bahsedilen, kullanıma ihityaç duyulmadığında, string'in memory'den silinmesinin zaman almasıdır. (kaynak: http://docs.oracle.com/javase/6/docs/technotes/guides/security/crypto/CryptoSpec.html#PBEEx )

.net'te System.Security namespace'si altında SecureString isimli bir sınıf vardır. Bu sınıf aynı br string gibi davranmakta fakat işimiz bittiğinde bu sınıfı Dispose() (kelime anlamı: elden çıkarmak) metodu ile yokedebiliyoruz. Aynı zamanda, burada string memory'de şifrelenmiş olarak saklanmaktadır. şifreleme görevini .net ortamı'ı yapmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Headless software

örnek: "headless java", "headless Linux"

bu terim o yazılımın GUI'siz çalıştığını belirtir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# wordpress vs joomla vs brutal

açık kaynaklı php tabanlı content management sistemleridir. başlıktaki sırası ile basitten daha gelişmiş'e doğru gider. daha basit kullanımı olan wordpress neredeyse hiçbir teknik bilgi gerektirmeden kullanılabiliyor. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# dosya sistemi (yada file system)

dosyaların bellekte tutulma formatıdır. örnek formatlar: ntfs, fat16, fa32, ReiserFS, ext# (ext4, ext3...), XFS

# iso9660

CD-ROM dosya sistemi

# journaling file system (günlükleme dosya sistemi yada JFS)

hiçbir dosyaya direk olarak yazmaz. önce geçici bir dosyaya (journal) yazar, herşey kesinleştiğinde/bittiğinde dosya referansını yeni dosya adresi ile günceller. bu tarz dosya sistemlerine JFS tabanlı dosya sistemleri denir.

# Journaled File System

IBM'in geliştirdiği JSF tabanlı dosya sistemi. özel isimdir. teknolojisi ile aynı ismi barındırıyor.

# disk birleştrime (yada disk difragment)

bir dosyanın fiziksel olarak yazıldığı noktalar bütünleşik olmayabiliyor. bu sebeple dosyalar arası boşluklar meydana geliyor ve tek dosya birçok parçalanma yaşayabiliyor. bu parçalanmalar dosya okuma yazma hızlarını da düşürüyor.  sebeple disk defragment yapan yazılımlar geliştirilmiştir.

ext4 gibi gelişmiş bazı dosya sistemlerinde okuma yazma yaparken akıllı algoritmalar sayesinde dosya bölünmeleri en aza indirgeniyor ve yeni dosyalarla boşluklar kapatılıyor. bu sebeple bazı dosya sistemlerinde disk birleştirme neredeyse hiç gerekmiyor.

# dosyaların silinmemesi

dosyalar dosya sisteminden silindiğinde, sadece dosya adresi silinir. bu sebeple hala dosyanın geri getirilme şansı vardır. bu sebeple bazı güvenlik yazılımları, silinene dosyaların üzerinden geçmek için, boş alanın tümünün üstüne yazar. boş alanı dolu olan bir bellekten dosyayı yazılımsal tekniklerle geri getirilme şansı yoktur. artık donanımsal/fiziksel çözümlerle eski dosyalar geri getirilebilir.

# inode

unix tarzı dosya sistemlerinde her dosya ve klasörün metadataları bir tabloda (inode table) tutulur. her inode bu tablonun her satırıdır. her inode (her satır) bir dosya yada klasörün meta data'larını tutar.

# hdd vs solid-state drive (yada SSD yada solid-state disk)

hdd mekanik ssd ise tamemen elektronik yapıdadır.

# hhd'nin fiziksel yapısı

- plak (yada platter): hdd üstüste takılmış yuvarlak parçalardan meydana gelir. genelde bir hdd kendi içinde birkaç plak bulundurur. 

- track: her plak içinde dairesel kayıt kısmına denir. 

- sektör (yada sector): her track kendi içinde ufak kısımlara bölünmüştür. en ufak birim sektördür ve genelde 512 byte'tır. 

- Cluster (yada Block): bir hdd'de sektör sayısı çok fazla olduğundan dosya sistemleri kendi içlerinde ardışık olan sektörleri gruplandırırlar. böylece sanal olarak en ufak kayıt birimi cluster olur. windows temelli sistemler cluster terimini kullanırken, posix'lerde block terimi kullanılır. bu birime "allocation unit"'te denir. cluster genelde 4 sektörü kapsar.

# lock files

bir program işlem yaptığı dosyayı kilitleyebilir. bu şekilde başkaları bu dosyay erişmeye çalıştığında işletim sistemi izin vermeyecektir. dosya sistemi ve işletim sisteminin sunduğu bir çok lock çeşidi vardır. her işletim sistemi farklı çeşit lock seviyeleri sunuyor. örnek: bazı filesystem'ler dosyayı kilitleyip, diğer yazılımlara read-only açabiliyor. bazı dosya sistemleri dosyanın bir kısmını lock'lamaya izin veriyor.

# "size on disk" vs "size"

sadece "size" denildiğinde; dosyanın byte olarak ne kadar veri içerdiği kastedilir. oysa aynı dosya için "size on disk" çok daha fazla yada az bir değere tekabül ediyor olabilir. 

harddisklerde en küçük kaydedilebilir birim "allocation unit"'tir. dosya sistemimizin "allocation unit" boyutu 4096kb olduğunu varsayalım. bizim dosyamız ise 512kb olsun. bu durumda bizim dosyamız 4096kb "size on disk" değerinde olacaktır. 

dosya sisteminin şifreleme yada sıkıştırma gibi özellikleri oluyor. bu özellikler sonucunda dosyanın "size on disk" boyutu, "size"'a göre artmış yada azalmış olabilir.

# sparse file

dosya sistemleri bu özelliği native destekliyor olabilir. bazı dosyalar örneğin 1 gb yer almaktadır. fakat bir yazılım henüz dosyanın tümüne yazmamıştır. dolayısı ile yazılmayan kısımlar kullanılmaz. bu kullanılmayan kısımlar sayesinde "size on disk" değeri "size" değerinden düşük olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# variable shadowing (yada değer gölgeleme)

programlama dünyasında kullanılan bir terimdir. bir değer scope'ta ve üst scope'ta aynı isimde olabilir. örnek:

```java
class MyClass {

    int myVal;
    void method(int myVal){

         //burada 2 farklı myVal mevcut
    }
}
```

her zaman üst scope'taki değere 'shaddowed' adı verilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Messaging System
 
  iki farklı şekilde yapılabilir:

  - Point to Point

    örnek: bir web browser'ın sunucuya yolladığı ajax isteği

  - Publish-Subscribe

    örnek: apache kafka, MQTT

# apache kafka

kafka açık kaynaklı bir broker'dır. publisher'lar buraya mesaj bırakır, consumer'lar ise buradan üye oldukları topic'lere göre mesajları okurlar. mesajlar okunduğunda, kafka broker'ı subscriber'lara onay bildirimi yollar. 

kafkanın producer ve consumer'lar için java api'si vardır. örnek:

- producer

```java
//props = config informations like kafka broker ip...
Producer<String, String> producer = new KafkaProducer<String, String>(props);

//we create a message here
final ProducerRecord<Long, String> record = new ProducerRecord<>("my-topic", "my-key", "my-value");

//we send the message
RecordMetadata metadata = producer.send(record).get();
```

- consumer

```java
//props = config informations like kafka broker ip...
KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(props);

//we should subscribe to get a message form that topic
consumer.subscribe("my-topic")

ConsumerRecords<String, String> records = con-sumer.poll(100);

//we loop one by one for all messages from that topic
for (ConsumerRecord<String, String> record : records)
         
         //we got the message
         System.out.print(record.offset(), record.key(), record.value());
```

- # Broker
    her apache kafka node'una denir.

- # kafka cluster
    birçok broker'ın bir arada çalıştığı ortama denir.

- # partition
    her broker'da birden fazla partition vardır. her partition'da birdena fazla topic olabilir, ve bir topic biden fazla partition'da yer alabilir.

    partition'lar kuyruk gibi düşünülebilir. bir kuyruktaki mesjaı hem tpic hemde partition adı ile çekebiliriz.

- # offset
    offset bilişimde: the distance to an element within a data object

    mesajın o partition'daki pozisyonudur. bu değer hiç değişmez.

- # replication factor
    bir partititon'a mesaj bıraktığımızda, o mesajın kaç adet broker'da node'da klonlanacağını belirtebiliyoruz. mesajımızı X node'unun A partition'una gönderelim. X node'u bunu altığında diğer node'ların ilgili partition'larına yollar. bu şekilde bir makinemiz down olduğunda, diğer makineler mesjaı saklamaya ve dağıtmaya devam edecektir.

# Event bus vs service bus vs message Queue

- service bus

  çoğu zaman "Enterprise service bus" diye geçiyor.

  birçok servis, tek bir servise (service bus'a) mesaj atar. mesjaları geldiği gibi sırayla işletme görevi artık service bus'tadır.

- message Queue

  service bus'ımızda birçok kuyruk olabilir. her kuyruk farklı amaçlıdır ve farklı id'si vardır.

  - enqueue
    verb. add item to queue.

  - dequeue
    verb. remove item to queue.

- event bus

  bir çeşit service bus çeşididir. event bus'a yolladığımız yer mesaj içerisinde bir de callback metodumuz vardır. bu callback metodları zamanı geldiğinde event bus tarafından tetiklenir.

- event bus için örnek framework'ler
  - Apache Kafka
  - rabbitmq
  - JMS

    JMS, JavaEE'nin bir spesifikasyonu. Birçok messaging altyapısı (broker) için implementasyonu mevcut.

    - Apache ActiveMQ

      "Artemis" kod adı ile ActiveMQ projesi paralelden geliştiriliyor. Artemis stabil olduğunda eski proje (artemis'in başlaması ile artık "Classic" olarak adlandırılıyor) sonlandırılacak.

    - Open Message Queue (yada OpenMQ yada Open MQ)

      Orcale tarafından geliştirilen, GlassFish içerisinde default olarak geliyor

    - OpenJMS

    - Oracle Advanced Queuing (yada AQ)

    - IBM MQ (yada eski adı:MQSeries yada eski adı:WebSphere MQ)

    - Rabbit mq (Pivotal Software tarafından geliştiriliyor)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# excel

# farklı dildeki ofis setlerinde formüller
excel dosyalarındaki formülller ofis dil yüklenmiş ofis paketlerinde sorunsuz açılabilmektedirler. çünkü formüller text olarak değil, referansları ile saklanmaktadırlar. dolayısı ile sadece ekrana basarken farklı dillerde gösterilmektedirler. fakat ekstra yüklenen eklentinin multi-language desteği yok ise, eklentinin sunduğu formüller doğal olarak multi-language olmayacaktır.

# $ işareti
excel'de $ işareti sutun yada satır adresinin önüne gelebilmektedir. örneğin; $A$2 yada $A2 yada A$2 olabilir. $ işaretinin olduğu bölüm (sutun yada satır), bulunduğu hücredeki formül kopyala yapıştır yapıldığında değişmemektedir. $ olmayan kısımlar her zaman değişebilir. örneğin bir satır aşağıya kopyalanan hücrenin içinde $ olmayan satır değerleri otomatik olarak 1 artar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# csv (yada Comma seperated value)

csv dsya formatının avantajı şu: txt formatı gibi her editorle açılabiliyor. aynı zaman excel ve alternatifi uygulamalar csv açarken her virgül arasını bir sutuna koyup açtığı için aynı dosya excel ile de sutunlara bölünmüş şekilde açılabiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# getter-setter

kullanım sebepleri:

private değişkenlere erişim/read/write sırasında

- yetki kontrolü yapılabilir

- set edilecek veri ise; verinin uygunluğu/validation'ı (null mı? gibi) kontrol edilebilir

- getter setter işlemlerinde loglama yapabiliriz

- debug ederken setter veya getter'a log atılması işimizi hızlandırabilir.

- immutability sağlayabiliriz (aynı objenin klonunu döndürürüz, setter'da ise işlem yaptırmayız.)

- Mocking, Serialization gibi kütüphanelerin hangi nesneleri alması gerektiğini belirtebiliriz. (bunu sadece anotation kullanarakta yapabiliriz)

- bizim sınıfımızdan extend etmiş sınıflar farklı davranmaları için esneklik yağı sağlamış oluruz.

- getter'a tüm paketlerden erişimi açabilir, setter'a ise sadece aynı pakettekilerin erişebilmesini sağlayabiliriz (public getName, protected setName)

get/set prefixleri gerçek getter-setter amaçlı kullanılmayan metodlarda kullanılmamalıdır. Bu sebeple get yerine örneğin; fetch, find gibi isimler verilmelidir.

# Accessor vs Mutator

programlama dünyasında; Accessors getter metodlarına verilen isim, mutators ise setter metodlarına verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Convention over configuration (yada coding by convention yada CoC)

örnek; jpa'da aksini belirtmedikce @entity ve @id'ye annotation'larina sahip tum pojo class'lar, attribute name'lerin ve class name'lerin birebir tablolarla ve column'larla eslestigi kabulu uzerine dayanir ki buna convention denir. yani özel olarka ek könfigürasyon belirtmediğimizce yapılan code'lamalar daha temiz ve daha sadece görünmektedir. fakat bunun dezavantajı şudur: codu okuyan başka birisinin kullandığımız framework/library'lere hakim olması gerekir. çünkü configler hep default'tur.

# Diamond Problem

```
  A

B  C

  D
```

Yukarıdaki elmas görünümlü şekil extend ağaçı olsun. A super class. B ve C, A'dan türemiş. D ise hem B hem C'den türemiş.

B ve C, A'nın X metodunu override etmiş olsun. D, A'nın X metodunu çalıştırmak isterse ne olacak? İŞte bu problme için birçok dil farklı  çözümler getirmiş durumdadır. 

# composition over inheritance

EFT ve Havale yapmak için sunucuya gönderdiğimiz iki ayrı nesnemiz olsun. bu nesnelerde tutar, karşı atarfın banka nosu, gibi ortak parametreleri var. Genelde yazılımcılar MoneyTransferCommonData objesi oluşturur ve içine amount, receiverNo gibi ortak değerleri atar. Daha sonra EFT ve Havale nesnelerini bundan türetir. Oysa "composition over inheritance" practice'si, bunun yerine  MoneyTransferCommonData objesini EFT ve Havale objelerinin içinde propertie olarak tutmamızı önerir. Bunun avantajları şudur:

- bazı diller birçok extend'e izin vermez. extend hakkımızı kullanmamış oluruz.

- ilgili sınıfımızı ilerde bir çok interface/abstract/sınıftan türetebiliriz. bu türetmeler sonucu (özellikle aynı isimdeki metodlar) debug işlemlerimiz çok zorlaşacaktır. oysa; myProperty.myMethod() çok daha okunaklı. sadece myMethod() bize mi ait super classtan mı geliyor diye sürekli bakmak gerekmektedir.

yani; "is a" ilişkisi yerine "has a" ilişkisi tercih edilmelidir.

# Halting problem (yada Sonlanma problemi)

bir programı başlatırken birçok parametre ile başlatabiliriz. her parametreye karşılık bu programın sonsuza dek döngüye girip girmeyeceğini yada sonlanacağının garantisini veremeyiz. daha doğrusu bunu programatik olarak kanıtlayacak/ispatlayacak programı yazamayız.

örneğin IDE ile kodumuzu formatlarken sonsuz döngüye girme ihtimalimiz var. Böyle bir durumda IDE nasıl davaranacak? O kodun formatlanırken IDE'nin sonsuz döngüye girip girmeyeceğini bilemiyoruz. Bunu bilemediğimizden IDE'ler belli bir süre sonra format işlemini durdurmaktadır. Çünkü henüz bir programın sonsuz döngüye girip girmeyeceğini belirleyen bir program yazılamamıştır.

Alan turing böyle bir programın yazılamayacağını kanıtlamıştır. Kanıt "proof by contradiction (çelişki ile kanıt)" yöntemini kullanmaktadır.

A programımıza i inputunu verseydik; A programımız sonlanırmıydı?

isHalting( A, i );  
  - returns;
    - true(durur)
    - false(durmaz) 

isHalting'in koduna eklemeler yapmak amaçlı metodu wrap edelim:
- true(durur) dönerse; wrapper metodumuz sürekli çalışmaya devam etsin (kendini sonsuz döngüye soksun).
- false(durmaz) dönerse; wrapper metodumuz true(durur) dönsün ve kapansın.

Bu wrapper metodumuza isHalting+ adını verelim.

Şimdi şu işlemi yapalım:

isHalting+( isHalting+, (A,i) ); 

Bu metod bize ne dönecek?
- isHalting+ eğer true(durur) dönerse; isHalting(isHalting+, (A,i) ) false dönmüş demektir. Oysa isHalting+ hiç false dönemiyordu. False durumunda kendini (isteyerek - bu kodu biz yukarıda kendimiz ekledik) sonsuz döngüye sokuyordu. Böyle bir durum söz konusu olamaz (çelişki durumu). Yani makinemiz bize yanlış bilgi vermiş oluyor.

- isHalting+ false(durmaz) dönmek yerine kendini songuz döngüye sokuyordu (bizim yazdığımız kod). eğer kendini sonsuz döngüye sokarsa; isHalting(isHalting+, (A,i) ) true dönmüş demektir. Burası olağan gibi geliyor. Fakat isHalting(isHalting+, (A,i) ) true dönmüş olması için isHalting+'ın çalıştırıldığında sonlanmış olması; bir return değerini döndürmüş olması gerekli. isHalting+ sadece isHalting false durumunda true dönüş yapabiliyordu. diğer durumda sonsuz döngüye giriyordu. isHalting false ise; isHalting(isHalting+, (A,i) ) true dönmüş olamaz. Burada da bir çelişki söz konusudur.
  

# Project Management Triangle (yada Triple Constraint yada Iron Triangle yada Project Triangle)

scope(kapsam), cost(maliyet), schedule(zaman) sabitlerinin köşeleri olduğu üçgendir. örneğin; maliyeti düşürürseniz diğerlerini arttırırsınız.

# Hofstadter yasası

Bir olay her zaman planlanan zamandan daha geç biter. Hofstadter yasasını hesaba katsan bile.

# Hollywood Principle

Hollywood'daki yapımcılar/yönetmenler önemli insanlardır (VIP - Very important person). onları aradığınızda ve bir teklif sunduğunuzda size bölye dönerler: "Don't call us, we'll call you".

Bu prensibe dayanan birçok yazılım Frameworkü mevcut. örnek: spring'in DI'ı bu prensibe dayalı çalışır: Bir java bena'i bir dependency'yi autowired ettiğinde, dependency'yi aramaz. spring o sınıfa dependency'yi getirir. "Don't call around for your dependencies, we'll give them to you when we need you."

# Brooks's law

bir projedeki kişi sayısı arttığında, projenin kısalma süresi gelen kişi sayısı ile doğru orantılı değildir. bu kişiler birbirleri ile iletişimde kaybedilen zaman artmaktadır. aynı zamanda birbirlerine olan aktarım süreleri de artmaktadır vs...

# law of demeter (yada LoD yada principle of least knowledge)

coupling'i azaltmak için temel bir prensiptir. her sınıf sadece kendi içindeki variable'ları çağırmalıdır.

# don't repeat yourself (yada DRY)

tekrar yazılan kodları ortak yere alma prensibidir.

# Unix Felsefesi (yada Unix philosophy)

Unix işletim sisteminin lider geliştiricilerinin deneyimine ve yaklaşımlarına dayalı yazılım geliştirme metodolojisidir. bu yöntemler genlde tüm POSIX'lerde tercih ediliyor.

genelde aşağıdaki metodolojilere dayanır:

- Write programs that do one thing and do it well.

- Write programs to work together.

- Write programs to handle text streams, because that is a universal interface.

# Secure by design

güvenlik açıklarını kökten çözüm bulan teknikler için kullanılan terimdir. güvenlik açıklarını spesifik açık bularak kapatmak yerine, kökten (mimarisel) çözüm bulunan yazılımlara/ortamlara "secure by design" denir. yani dizaynın kendisi zaten güvenlidir. örneğin;
- web tarayıcısı hacklenebilir. bunun için birçok güvenlik yöntemi ekleyebiliriz. fakat eğer tarayıcının kendisi, android üzerinde hiçbir yetki kullanmadan çalışır ise; uygulama hacklense bile hacker dış kaynaklara erişemeyecektir. tarayıcının kendisi istese dahi dış kaynaklara erişemeyecektir. işte bu tarz dizaynlara verilen genel bir teknik terimdir. docker veya sanal makinlerde uygulamalr açılarak güvenliğin sağlandığı birçok uygulama vardır.
- sanal paralar dayapısı da buna güzel bir örnektir. sanal paraları güvenli kılan faktör sadece kod güvenliği değildir. sistemin kendisi baştan iyi tasarlanmıştır. sanal para sahipleri kendi paralarının değerinin kaybolmaması için diğer sanal para sunucuları (node'ları) ile uyumlu çalışma durumundadırlar.

# SOLID prensipleri

object orianted için çıkmış 5 tane prensibi birarada bulunduran bir terimdir (baş harflerini birleştirilmiştir): SRP, OCP, LSP, ISP, DIP

- ## Bağımlılığın Ters Çevrilmesi Prensibi (yada Dependency Inversion Principle yada DIP)

  iki kuralı vardır:

  1. High-level modules should not depend on low-level modules. Both should depend on other high levels.

  2. Abstractions should not depend on details. Details should depend on abstractions.

  Yukarıdaki iki maddeden çıkaracağımız şudur: Üst seviyeli modüller/sınıflar, alt seviyeli sınıflara/modüllere bağımlı olmamalıdır. Alt seviyeli modüller üst seviyeli modüllere(modüllerin arayüzlerine) bağımlı olabilir.

  Örnek üzerinden gidelim; online ödemeler yapılacaktır. Birçok online ödememeiz mevcut. Shop abstract'ımız (burada üst seviyeli sınıf/modül oluyor) online ödemeleri bu şekilde kullanıyor olsun:

  ```java
  public abstract Shop {

    private TaksitliOdeme taksitliOdeme;
    private KrediKartiOdeme krediKartiOdeme;

    ....

    public ode(){

      if(taksitliOdeme){

          alert("Taksitli ödeme icin vade farkı alınacaktır");
          taksitliOdeme.ode();

      } else if(krediKartiOdeme){

          krediKartiOdeme.ode();

      }

  }
  ```

  DIP prensibi yukarıdaki durumun yerine tüm ödemeleri bir interfaceden extend edip, bu interfaceyi Shop sınıfına inject edip kullanmamızı öneriyor.

- ## Arayüzlerin Ayrımı Prensibi (yada ISP yada Interface Segregation Principle)

  Direk örnek üzerinden gidelim: Mesaj interfacemiz olsun. bunu implemente eden bir MesajImpl sınıfımız olsun. MesajImpl kendi içinde attahmentVideo, attahemtnPhoto gibi özellikler içericek. fakat her mesajda video olmayabilir. sadece foto olabilir. bu sebeple burada önerilen; birden fazla MesajImpl olsun ve her biri video için yada photo için spesifik geliştirilsin. (bir mesjda hem foto hem video olamayacağını varsayarak örnek verilmiştir). bu şekilde her implementasyon sadece tek bir işe odaklı yani; interfacenin metodlarını implemente etmeye odaklı olmalıdır. böylece kullanılmayan metodlar çıkarılmış olacaktır.

- ## Liskov'un Yerine Geçme Prensibi (yada Liskov Substitution Principle yada LSP)

  Temel sınıf bizim diğer sınıfları extend edeceğimiz base abstract yada normal classımız olsun. buna göre; temel sınıfımızda, onu implemente edecen sınıflara özel, fonksiyon yada field bulunmamalıdır.

- ## Single Responsibility Principle (yada SRP)

  Bir sınıfın yahut metodun tek bir sorumluluğu olmalıdır. eğer metodun/sınıfın değişmesi gerekiyorsa; muhtemelen ona yeni sorumluluklar mı eklenecek şüphesi yaratmalıdır.

- ## Open/closed principle (yada OCP)

  code/software should be open for extension, but closed for modification. yani; yeni eklemeler yapıldığında programda minimum değişiklik yeterli olabilmelidir. örneğin; online ödemeler. yeni bir ödeme geldiğinde yeni ödemeyi bir sınıftan implemente edip bir listeye eklememiz yeterli olabilmelidir.
  
  Farklı bir değişle; yeni geliştirme geldiğinde; varolan kodumuzu az editlemeliyiz fakat yeni sınıfılar yaratmalıyız. Bunu ne kadar iyi yapabiliyorsak, koduumuz OCP'ye o kadar yakındır.

# KISS (yada Keep it simple, Stupid)

herşeyi basit olmasına dayanan feslefedir.

# Iterative and Incremental development

sürekli olarak ufak geliştirmeler çıkarak yapılan geliştirme yöntemidir. bu şekilde riskler düşürülür, feature/branch merge'ler kolaylaşır.

# Abstraction principle

yazılımı abstract sınıflar kullanarak tekrarlanan kodları azaltma metodolojisidir.

Abstraction; karışık bir implementasyonu, kullanıcıdan veya programdan gizlemek için yeni bir katmanın geliştirilmesidir. Bunun sonucunda anlaşılması ve kullanılması daha kolay bir arayüzün ortaya çıkması beklenir.

# Big Design Up Front (yada BDUF)

yazılımın mimarisi oturtulmadan sayfaların/businnes logic'in geliştirmeye başlanmadığı yazılım geliştirme sürecidir.

# Yukarıdan aşağıya ve aşağıdan yukarıya tasarım (yada Top-down and bottom-up design)

yukarıdan aşağıda büyük resimden detaylara doğru inceleme anlamına gelir. bir işe sistemin en detaylı kısımlarından başlanıyor ise; aşağıdan yukarıya bir geliştirme süreci izleniyor demektir.

# design by contract

direk örnek üzerinden gidersek;

```java
int böl(int a, int b)
{
  contract.requires(b != 0);
  return a / b;
}
```

static kod analizinde derleyici bu kodu yakalayacaktır. işte bu tarz kodlara anlaşmalı kodlar deniliyor.

# Defensive design

kodun kendinğ koruduğu, böylece alacağı parametrelerle uygulamanın akışının engellenmediği yazılımlara denir. örneğin; ekleyeceğiniz pluginler hiçbir şeyilkde uygulamanızı durdurmamalı. gerekirse plugin kill edilmeli ve uygulama devam etmeli. başka bir örnek: getterlarda döndürülen listeler sadece read only olmalıdır. fakat javada referans döndüğünüz için biri bizim listemizimyObj.getList().add("new element"); diyip ekleme çıkarma ayapabilir. self defensive olup getterlardan objenin klonunu yada read only halini döndürebiliriz. 

# service contract

servislerimizi inject edip başka yerden kullandığımızda inject ettiğimiz şey, bir interface olması görüşüdür. hangi implementasyonun run-tme'de kullanılacağına framework/mimarimiz karar vermelidir. bunun avantajları:

- service'lerimiz interface'lere depend eder implemetasyonlara değil. bu kural farklı patternlerden gelmektedir.

- bazen servislerimizde protected yada public metodlarımız olabilir. bu metodları her zaman dışarıya dğeil, diğer internal sınıfılarımıza açma gereği duyabiliriz. bu sebeple API'miz interface'lerden okunmalıdır. böylece herkes bilirki API sadece interfacededir.

Sadece 1 implementasyonumuz olacaksa bile bunu yapmalıyız. çünkü:

- ilerde 2inci implementasyon gelirse, buna hazır olmuş oluruz.

- API'miz interface olursa dışarıdan okuyan kişi daha kolay ve net olarak hangi metodları çağırması gerektiğini okuyabilir.

Yukarıda bir API'den bahsedildi. API uygulamamızın dışarıya açtığı metodları temsil eder. fakat internal sınıflarımızda API kullanıyoruz demek pek doğru olmaz. Bu sebeple "contract (yada anlaşma)" denilmektedir. tabi buradan türeyen diğer terimlerde kullanılabilir: "interface contract", "service contract", "service interface"... Servislerimizin aldığı parametrelere "Message Contact", "data contract" da denilebilmektedir.

# YAGNI (yada you aren't gonna need it)

ihtiyaç olmayan özelliklerin projeye eklenmemesi prensibidir.

# over-engineering

over-engineering çözümler her zaman gerekenden fazla özellikleri desteklemek için uygulanır. bu aslında YAGNI ile neredeyse aynı kapıya çıkmaktadır.

# interoperability (yada Birlikte çalışabilirlik)
interop kelime anlamı: birlikte çalışma

genel bir terim olduğundan kullanılan domain'e göre farklı şeyi ifade edebilir. bazı örnekler:
- bir programlama dilinin interoperability karakteristiği; o dil ile yazılmış bir koddaki fonksiyonların, diğer diller tarafından yazılmış uygulamalar tarafından çağrılabilirliğini belirtir.
- diğer yazılımların kullandığı dosya formatına okuyup yazabilmekte, uygulamamızın interoperability karakteristiğini yansıtır.
- herkesin okuyabileceği ve yazabileceği bir dosya formatımız olursa, o dosya formatı için interoperability den bahsedilebilir.

İlk maddede belirtilen interoperability karakteristiğini şu maddelerle değerlendirebiliriz:
- dilin kullandığı primitive ve standart objeleri, diğer dillere pass edilebilirliği(uyumu) ve tersi
- diğer dillerden fonksiyonlar çağırdığındaki performansı ve tersi
- ve dahası...

- # Component Object Model (yada COM)
microsoft ürünlerinde kullanılabilen IPC için kullanılan, ortak data obje modelidir. dilden bağımsızdır. bu interoperability için diil IPC için kullanılmaktadır. COM; primitive tipler içeren bir standarttır. her dil bu kurallara uyar. bu şekilde haberleştiklerinde ortak değerler ile haberleşirler.

COM; IPC için kullanılır, fakat microsoft dilleri arasında interoperability yapmak gerektiğinde de bu obje tipleri kullanılabilmektedir.

- # Distributed Component Object Model (DCOM)
COM dan sonra çıkmış Remote procedure call'larda kullanılabilirlik eklenmiştir. bu sebeple Distributed prefixi eklenmiştir.

- # Object Linking & Embedding (yada OLE)
COM'a dayanan bir teknolojidir. Bir programdan bir dosyaya referans edebilmemizi sağlar. Örneğin, bir word dökümanında, başka bir excel dosyasının br parçasını gösterebiliriz. excel her güncellendiğinde otomatik olarak word'deki excel görüntüsüde güncellenecektir.

- # CORBA (yada Common Object Request Broker Architecture)
Dilden bağımsız geliştirilmiş, DCOM'a alternatif bir standarttır. microsoft hari birçok kuruluş bu standardı desteklemektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# magic string
yazılımcılar arasında "hardcode string" olarakta adlandırılır. enum, yada final static olmayan kod içine direk olarak yazılmış string'lere verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# method/function invoke vs call

ikiside kelime anlamı olarak aynı. resmi bir farklılık yok. fakat çoğu zaman "invoke" kelimesi otomatik olarak çalıştırılan metodlar için kullanılırken kullanılır. örneğin; sınıf oluşturduk, içindeki before ve constructor metodları otomatik çalıştırıldı. oysa "call" ibaresi daha çok elle bir fonksiyonu çağırdığımızda kullanılır.

# 'Calling a method' vs 'sending a message to a method'

programlama dilinin metod çağırma yöntemidir. programlama dilini kullanan yazılımcı için, dilin hangi yöntem ile metodu tetiklediğinin bir önemi yok. fakat dil geliştiricileri için bu konu önemli.

örneğin C 'Calling a method' yöntemini kullanır. burada iligli kod bir fonksiyonu çağırdığında, kod direk o çağrılan fonksiyonun içine gider. Objective-C ise 'sending a message to a method' yöntemi ile çalışır. Objective-C'de bir metod mesaja cevap vermeyebilir. yada farklı metodlara yönlendirme gibi durumlar olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Homebrew
Sadece macOS için yapılan açık kaynaklı paket yöneticisi projesinin ismi. komut satırından __"brew"__ olarak kullanılmaktadır.

Linux ve ms-windows için aynı temele dayanene yazılım paralelden ayrı geliştiriliyordu (linux için olna Linuxbrew projesiydi). daha sonra bu projelerin tümü birleştirildi.

Homebrew, nix ile çok benzer yapıdadır. fakat nix, özellikle sürüm yönetimi konusunda çok daha gelişmiştir.

# formulae
formulae paket kurulumu için gerekli tanımlamaları içeren metindir.

# Cask
Homebrew eklentisidir. GUI kullanan uygulamaların yönetimini sağlamaktadır.

ilk başlarda Homebrew'den bağımsız geliştiriliyordu. daha sonra Homebrew altında ortak geliştirmeye başlandı.

Cask sadece macOS'ta desteklenmektedir (yıl 2019).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Metot Aşırı Yükleme (yada Method Overload)

yazılım dünyasında kullanılan genel bir terimdir. her programlama dili desteklemez. bir metod aynı isimde aynı sınıfta/scope'ta olabilir. fakat aldığı parametrelerin sayısı veya sayısı aynı fakat tipi farklı olabilir. bu duruma verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# IBM DataPower Gateway

donanımdır. web servise gelen isteklerde menipülasyon yapabilmemizi sağlar, genel istekleri filtreleyebilmemizi sağlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# chrome extension vs chrome app

chrome app'e daha çok api açık durumda olduğundan daha çok özellik'ten yararlanabiliyor. + app'ler tarayıcıdan bağımsız bir pencerede tarayıcı penceresi başlatılmaden dahi başlatılabiliyor. burada amaç kullanıcıya daha çok native prgrammış havası yaratmaktır. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# pure vs Impure function

pure fonksiyonlar sadece bir hesaplama yapıp bir sonuç geri döndürürüler. uygulamanın içinde yada dış kaynaklarda hiçbir şeyi değiştirmezler. hiçbir şeyi değiştirmedikleri gibi hiçbirşey de okumazlar, okusalar bile bu değeri hiç kulanmamalılıdırlar. bu şekilde sonuç olarak; her zaman aynı girişe karşı aynı çıktıyı verirler. deterministictirler.

impure metodlar ise bu kurala uymayan tüm metodlar için geçerli bir sıfattır.

# side effect

bir fonskyionun kendi içindeki değerler haricinde dışarda bazı kaynaklarda değişiklik yapmasıdır. yani dış dünyaya etkisi olmasıdır. örneğin; dosyaya yazması. console.log() metodu dahi dış kaynakları kullanır. console'u milyon saniyesede bir de olsa durdurur ve o kaynağın başkası tarafından kullanılmasını engellemiş olur. databseden veri okumak bile side effect yaratır. okunan veri için dışa kanyaklarda soket açılır, belki o soketler başkası tarafından kullanılacaktı. database sistemi ayarları sebbei ile biri okduğunda başkasının o veriler üzerinde değişiklik yapmasını engelleyebilir. bu da bir etkidir (side effecttir).

# Referential transparency (yada referential opacity)

kodun bir satırında bir metod çağırdık. çağırdığımızı bu metodu kaldırıp yerine döndürdüğü değeri yağıştırdığımızıda, tüm yazılım yine aynı şekilde çalışmaya devam ediyorsa, bu metod için "Referential transparency" 'den bahsedilebilir demek oluyor. matematikte her fonksiyon için kesinlikle Referential transparency vardır. fakat yazılımda kesinlikle bu durum var diyemiyoruz. çünkü çağırdığımız metod pure bir metod değilse, "Referential transparency"'den bahsedilemez.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# C language

# Preprocessor directives

derleme öncesi komutlardır. derleme aşamasın başlamadan kod üzerinde manipülasyon sağlarlar. bazı direktifler:

- ## include

ilgili kod dosyasını, include yazdığımız satıra ekler. kodların kopyasını ekler, referans etmez.

```c
//iki farklı syntax vardır:

#Include <DosyaIsmi.H>

#Include "DosyaIsmi.H"
```

- ## define

sabit bir değer yada fonksiyon yerine alias kullanabilmemizi sağlar. bu şekilde __macro (yada makro)__ tanımlamış olururuz.

```c
#Define YARI_CAP 987
```

ile sabit değerler kullanılabilir. aynı zamanda fonksiyon da tanımlanabilir:

```c
#Define  buyuk(a,b)   (  (a>b) ? a:b  )
```

- ## undefine

define ile tekrar tanımlama yaparsak hata alırız. eğer değeri güncelleyecek ise önce undefined yapmak zorundayız.

```c
#Define X 1
#Undef X
#Define X 2
```

- ## If, Elif, Else, EndIf

```c
#If (sizeof ( int ) == 2)
#Define MAKINA "16 bitlik isletim sistemi"
#Elif (sizeof ( int ) == 1)
#Else
#Define MAKINA "unknown bitlik isletim sistemi"
#Endif

main() {
  printf(MAKINA);
}
```

- ## ifdef, ifndef

Eğer önceden define edilmemiş ise aşağısındaki if bloğunun çalışmasını sağlar.

```c
#Ifndef PI
#Define PI "3"
```

- ## error

derleyicinin hata vermesini ve durmasını sağlar

```c
#if (size of ( int ) = = 2 )
#Error bu program ( 16 bit'lik ) bu işletim sisteminde çalışmaz!
#Endif
```

# literals

some examples:

```c
85         /* decimal */
0213       /* octal */
0x4b       /* hexadecimal */
30         /* int */
30u        /* unsigned int */
30l        /* long */
30ul       /* unsigned long */
3.14159       /* Legal */
314159E-5L    /* Legal */
510E          /* Illegal: incomplete exponent */
210f          /* Illegal: no decimal or exponent */
.e55          /* Illegal: missing integer or fraction */
```

# bitwise operations

```c
unsigned int a = 60;	/* 60 = 0011 1100 */  
unsigned int b = 13;	/* 13 = 0000 1101 */
int c = 0;

c = a & b;       /* 12 = 0000 1100 */ 

c = a | b;       /* 61 = 0011 1101 */

c = a ^ b;       /* 49 = 0011 0001 */

c = ~a;          /*-61 = 1100 0011 */

c = a << 2;     /* 240 = 1111 0000 */

c = a >> 2;     /* 15 = 0000 1111 */


```

# sizeof

sizeof(3); ile byte cinsinden uzunluk alınabilir. burada 3 bir int olduğundan integer uzunluğu dönecektir.

benzer şekilde: sizeof(int); komutu bir integer biriminin uzunluğunu dönecektir.

# bellek adresi

&myValue şeklinde adres bilgisi alınır. bu logical adres'tir (başka başlıkta anlatılıyor).

Java'da bu adresi elde etmenin hiçbir mantığı yoktur. çünkü garbage collctor (yada JVM'in farklı bir özelliği) verimliliği arttırmak için yada herhangi farklı bir sebepten, bellekteki data'ların logical adreslerini değiştirebilir. bu sebeple bu değere güvenip işlem yapmamız yanlış olacaktır.

# pointer (yada gösterici)

myValue'nun başlangıç adresi de bir yerde tutulmaktadır. bu yere pointer denilmektedir.

örnek kullanım üzerinden incelersek;

```c
int *pointerSayi; //yıldız işareti; karakteri yönlendirme (yada indirection) operatörü olarak adlandırılır

int sayi = 99;

pointerSayi = &sayi; // bu satır işlendikten sonra; artık pointerımız sayının adresinin tutmaktadır.
```

# array

arrayler sırası ile memoryde aynı tipte veri tutmaktadır. memoryde aralarında hiçbir bilgi bulundurmaz.

```c
dizi;
```

ile

```c
&dizi[0];
```

aynı bilgiyi döndürür. bu C'de arrayler için geliştirilmiş bir syntax'tır.

arraylar üzerinde adresleri ile gezinebiliriz. adresler ile gezinirken pointerları kullanmamız gerekir. çünkü arraylarda pointer'ı 1 arttırdığımızda, array'in tipi kadar byte arttırır. örneğin integer tutan bir array'in pointer'ını arttırırsak;

```c
pointer = pointer + 5;
```

aşağıdaki ile aynı şeyi yapmış oluruz.

```c
pointer = pointer + 5*sizeof(int);
```

## variable-length array (yada VLA yada variable-sized array yada runtime-sized array)

C'den bağımsız diğer programlama dillerinde de olabilen bir özelliktir.

C'ye C99 ile bu özellik gelmiştir. C'de daha öncesinde array ancak şu 2 şekilde oluşturulabiliyordu:

- array-size'ı ancak define'da tanımlanan bir değer ile oluşturulabiliyordu
- malloc ile bellek alanı tahsis edilen bir array pointer'ı yaratılıyordu

C99 ile artık run-time'de oluşturulan bir integer değeri ile de dizi oluşturabiliyoruz.

C++'ta bu özellik yok (eski sürümlerinde yoktu. yeni sürümlerinde gelmiş olabilir). Fakat gcc derleyicisi bu özelliği destekleyecek şekilde bir yeteneği var. fakat c++ spesifikasyonlarında bu özellik desteklenmiyor.

__Flexible array member__ C99 ile gelen VLA'dan farklı bir özelliktir.

## fonksiyona parametre geçme

aşağıdaki 1 ve 3üncü yöntemlerde size'ı farklı bir parametrede almak gerekli.

```c
void myFunction(int *param) {
   .
   .
   .
}

void myFunction(int param[10]) {
   .
   .
   .
}

void myFunction(int param[]) {
   .
   .
   .
}
```

# bellekte yer ayırma

__Malloc__ bellekten yer ayırmamızı söyler işletim sistemine:

```c
int *dizi;

dizi = dizi=(int*) malloc(20*sizeof(int));
```

__calloc__ da aynı görevi görür. calloc, mallac'a ek olarak tüm yer ayrılan bölgeye gerçekten 0 değerini atar. bu şekilde işletim sisteminin memory'yi tahsis etmekten ziyade, o bölgeyi gerçektende fiziksel olarak hazır ettiğini garanti eder. tabi performans olarak daha yavaştır.

__free(dizi);__ şeklindeki satır ise dizi için tahsis edilmiş tüm alanı serbest bırakır.

__realloc__ ise önceden zaten tahsis edilmiş alana ek yeni alan tahsis eder:

```c
yeniAlan = (int*) realloc (eskiAlan,100*sizeof(int));
```

# goto

```c
one: 
for (i = 0; i < number; ++i)
{
    test += i;
    goto two;
}

two: 
if (test > 5) {
   //do something
}

```

# function(void) vs function()
function(void) ekstra parametre kesinlikle kabul etmezken (derleyici hata fırlatır), function() istenirse parametre ile çağrılabilir. fakat bu parametreler önemsizdir çünkü hiç kullanılmazlar.

# kod işleme sırası

```c
#include "main.h"

int main()
{
    function1("Hello");
}

int function1(char name)
{
    //do something
}
```

Yukarıdaki kod için bazı derleyiciler hata verir. C dilinde best practice metodun main'den önce tanımlanmasıdır:

```c
int function1(char name); //bu tanımlamaya "prototype declaration" denir. fonksiyonun prototipini tanımlamış oluruz. 
```

# string

string diye bir tip yoktur. C "hello" yazdığımızda otomatik olarak char array yaratır. ve sonuna null karakterini koyar. aşağıdaki değerler aynıdır fakat birinin sonunda null karakteri vardır:

```c
char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'}; // \0 null karakteri olarak adlandırılır

char greeting[] = "Hello";
```

# struct

```c
struct Book {
   char  title[50];
   int   book_id;
};

int main() {
   struct Book book1;
   book1.title = "hello";
}
```

isteğe bağlı instance'ler yaratılabilir:

```c
struct Book {
   char  title[50];
   int   book_id;
} book1, book2, book3={"book3 name", 3};
```

İsteğe bağlı isimsiz struct'ta yaratılabilir:

```c
struct {
   char  title[50];
   int   book_id;
} book1, book2, book3={"book3 name", 3};
```

Pointer'lar aracılığı ile çalışmamız gerektiğinde;

```c
struct Book *struct_pointer;
struct_pointer = &book1;
char[] myStr = struct_pointer->title;
```

Aynı şekilde ilk başta da instance'ların pointer olup olmayacağını belirleyebliriz:

```c
struct {
   char  title[50];
   int   book_id;
} book1, *book2, book3={"book3 name", 3};

//Yukarıda sadece book2 pointer'dır.
```

## stuct üzerinde bit bazında bellek ayırma

struct yapısında tüm değeri değilde, sadece kaç bit kullanacağımızı da belirtebiliyoruz:

```c
struct myStruc1 {
   unsigned int a : 1;
   unsigned int b : 1;
};
```

yukarıda a değerinde sadece 1 bitlik yer kullanacağımızı belirtmiş olduk. aynı şey b içinde geçerli.

sizeOf(myStruc1) = 4 (byte). fakat 32 tane "unsigned int" olsaydı yine 4 byte yer kaplayacaktı.

```c
struct {
   unsigned int age : 3;
} Age;

int main( ) {

   Age.age = 4;
   printf( "Sizeof( Age ) : %d\n", sizeof(Age) );
   printf( "Age.age : %d\n", Age.age );

   Age.age = 7;
   printf( "Age.age : %d\n", Age.age );

   Age.age = 8;
   printf( "Age.age : %d\n", Age.age );

   return 0;
}

/*
output:

Sizeof( Age ) : 4
Age.age : 4
Age.age : 7
Age.age : 0
*/

```

## Struct'ların bellekteki yerleri

C'de struct'lar memory'de ardarda kaydedilir. bu sebeple arada boşluklar oluşur. çünkü cpu'dan en az word uzunluğunda byte okunabilir. eğer scturct içindeki bir verinin boyutu word'den küçük ise, boşluk kalabilir.

örnek üzerinden gidelim:

32 bitlik bir cpu'da boyutlar byte cinsinden şu şekilde olur:

```c
struct structure1
{
   int num1;      //4
   
   int num2;      //4
 
   char char1;    //1
   char char2;    //1
                  //2 byte boş kalır
   
   float float1;  //4 bytes
};
 
struct structure2
{
   int num1;      //4
   
   char char1;    //1
                  //3 byte boş kalır
				  
   int num2;      //4
   
   char char2;    //1
                  //3 byte boş kalır
   
   float float1;  //4 byte
};
```

- structure1 total: 16 byte harcıyor. oysa beklenen 14 byte'tı. cpu word sebebi ile böyle oldu. 2 byte boşta kaldı.

- structure1 total: 20 byte harcıyor. oysa beklenen 14 byte'tı. cpu word sebebi ile böyle oldu. 6 byte boşta kaldı.

programın tepesine __#pragma pack(1)__ yazılırsa; C derleyicisi boşluklar olmayacak şekilde scturcture'yi oluşturuyor. bu durum programın bellekten daha az yer harcamasına sebep olurken, performans olarak dezavantaj sağlıyor. çünkü sıkıştırılan datalar birden fazla word'ün içine saklanıyor, buda her defasında cpu'dan daha fazla data okunmasına ve bu dataları parse edilmesine sebep oluyor.

Bu konu C'de "structure padding" yada "struct allign" olarak geçiyor.

Her derleyici ve cpu-tipi'ne göre farklı padding yöntemleri kullanılabilir.

Struct içindeki pointer'lar belli bir yer kaplar. örneğin 32 bit için 4 byte. Pointer'ın işaretlediği yerler struct'ın boyutuna dahil değildir. Fakat fixed-size array'lar pointer'lardna farklıdır. fixed-size Array içindeki datalar struct'ın içindedir.

Eğer sadece bir struct'taki padding'leri yoketmek istiyorsak şunu yazmalıyız:

```c
struct __attribute__((packed)) struct1 {
     
	 int data1;
    
};
```

Eğer allign boyutunu kendimiz belirlemek istiyorsak: 

```c
__attribute__((packed, aligned(4))) 
```

olarak set edebiliriz.

Eğer tüm programda allign değerini set etmek istiyorsak, programımızın tepesine #pragma pack(push, 4) yazmalıyız. Eğer sadece belli struct'lara bu allign değerini set etmek istiyorsak:

```c
#pragma pack(push, 4)

//Structure 1
......

//Structure 2
......

#pragma pack(pop)
```

# union
struct ile syntax'ı aynıdır. fark şudur: union'da union içindeki instance'tan sadece 1 eleman kullanılabilir. 

```c
union foo {
  int a;   // can't use both a and b at once
  char b;
};

struct bar {
  int a;   // can use both a and b simultaneously
  char b;
};

union foo x;
x.a = 3; // OK
x.b = 'c'; // bu beklediğimiz gibi çalışmaz. x.a = 'c' olarak çalışır. "union" yapılarında ilk atama nereye yapıldıysa o değer artık referans alınır.

// 'c' ascii tablosunda 99'a karşılık gelir.
// dolasyı ile artık bu eşitlik vardır: x.a = x.b = 01100011 (decimal:99)
```

Union'daki her değer bellekte aynı yerde tutulur. Dolayısı ile bir union'un kapladığı yer, içindeki elemanlara bağlıdır. içindeki elemanların en çok yer ayıranı kadar yer ayrılır. örneğin; foo union'umuz içinde char ve int var. char 1 byte. int daha büyük boyutlu olduğu için, foo union'u int boyutunda olacaktır.

Union genelde şu durumda kullanılır: sayısal bir değer birden fazla tip (primitive) ile farklı amaçla kullanılabilir. bu gibi durumlarda atamamızı yaptıktan sonra o değeri istediğimiz primitive tipte kullanabilir. örneğin "100" char[] olarakta, belki de özel string işlemlerimiz sırasında int olarak ihtiyacımız olacak.

# typedef

Aşağıdaki örnekte newType bir alias'tır. örneğin istediğimiz yerde "int" yerine bu alias'ımızı kullanabiliriz. benzer şekilde struc yada union'lara da alias ile ikinci isim takabiliriz. 

```c
typedef int newType;

void main (void)
{
  newType myVar;

  myVar = 9;

  printf("%d", myVar);
}
```

c'de tiplerin boyutları cihazdan cihaza değişeceği için, her cihazda farklı tip kullanılabilir. bunlar pre-compiler (#ifdef... gibi) belirlenebilir. bu sebeple kodun içerisinde; her tip için (en temel tipleri için dahi) önceden belirlenmiş alias'lar kullanılmalıdır.

- typdef kullanımlarına örnekler:

  - örnek 1:

    ```c
    typedef struct { 
        int x; 
    } X;
    ```

    bu örnekte;
    - C case-sensitive olduğundan x ve X birbirine karışmamaktadır. 
    - X alias'lı isimsiz bir struct yaratılmıştır. Bu durum tüm kod tek satıra indirgenince daha kolay anlaşılmaktadır:

      ```c
      typedef   struct{ int x; }   X;
      ```

    - c'ye yeni başlayanlar X'i değişken olması ile karıştırabiliyorlar. oysa X bir değişken değil, çünkü satırın en başında typedef kullanıldı.

  - örnek 2:

    ```c
    typedef struct A { 
        int x; 
    } A;
    ```

    bu örnekte;

    - örnek 1'deki struct, isimli olarak yaratıldı ve ismi ile alias'ı A olarak verildi.

    - Artık fonksiyonlarda bunu parametre olarak istediğimizde bu şekilde yazmamız yeterli olacaktır:

      ```c
      void myFunc( A param1, int param2)
      ```

      Oysa örnek 1'deki tanımı bu şekilde parametre olarak yazmak zorundaydık:

      ```c
      void myFunc( struct A param1, int param2)
      ```

# C standard library (yada libc yada ISO C library)

__ANSI C (yada ISO C yada Standard C)__ American National Standards Institute (ANSI) ve International Organization for Standardization tarafından geliştirilen C dili spesifikasyonudur. Birçok sürümü vardır. spesifikasyon sürümleri:

| kısaltma     | spesifikasyon resmi id                                                                       | resmi stabil yayım yılı | makro değeri __STDC_VERSION__ |
|--------------|----------------------------------------------------------------------------------------------|-------------------------|-------------------------------|
|              | Brian Kernighan and Dennis Ritchie published the first edition of The C Programming Language | 1978                    | not exist                     |
| C89          | ANSI X3.159-1989                                                                             | 1989                    | not exist                     |
| C90          | ISO/IEC 9899:1990                                                                            | 1990                    | not exist                     |
| C95          | ISO/IEC 9899/AMD1:1995                                                                       | 1995                    | not exist                     |
| C99          | ISO/IEC 9899:1999                                                                            | 1999                    | 199901L                       |
| C11          | ISO/IEC 9899:2011                                                                            | 2011                    | 201112L                       |
| C18 yada C17 | ISO/IEC 9899:2018                                                                            | 2018                    | 201710L                       |

__Libc__ ise American National Standards Institute (ANSI) ve International Organization for Standardization tarafından geliştirilen, piyasada kabul görmüş temel birçok işlevleri bulunduran C kütüphaneleridir.

libc'ye alternatif birçok kütüphane vardır. bazıları:

- __C POSIX library__ kütüphanesi libc'nin bir türevidir. çok büyük oranla libc ile uyumludur, ek olarak birçok farklı fonksiyon da içerir.
- __BSD libc__, implementations distributed under BSD operating systems
- __GNU C Library (glibc)__, used in GNU Hurd, GNU/kFreeBSD and Linux
- __Microsoft C run-time library__, part of Microsoft Visual C++
- __dietlibc__, an alternative small implementation of the C standard library (MMU-less)
- __μClibc__, a C standard library for embedded μClinux systems (MMU-less)
- __Newlib__, a C standard library for embedded systems (MMU-less)
- __klibc__, primarily for booting Linux systems
- __musl__, another lightweight C standard library implementation for Linux systems
- __Bionic__, originally developed by Google for the Android embedded system operating system, derived from BSD libc

# GCC (yada GNU Derleyici Koleksiyonu yada GNU Compiler Collection)

Açık kaynaklıdır ve birçok programlama dilleri için (C, C++, Objective-C, Fortran, Java, Ada, Go) derleyici içerir.

İlk versiyonu sadece C derleyicisi içeriyordu. Bu sebeple eski versiyonlarının ismi yine GCC idi, fakat açılımı __GNU C Compiler__ idi.

# g++
GCC'nin içindeki bir komut satırı uygulamasıdır. Sadece c++ Compiler'ıdır. gcc komutu ile yapılabileceklerin aynısını sağlar. fakat sadece c++ derlediği için, komutların c++ için ayrlanmış olması büyük kolaylıklar sağlamaktadır. 

# Bloodshed Dev-C++
Bloodshed firması tarafından geliştirilen sadece ms-windows üzerinde çalışan C ve C++ için IDE'dir. compiler olarak gcc veya onun türevlerini kullanmaktadır.

# turbo c
Borland firması tarafından geliştirilen IDE ve c/c++ derleyicisidir. ismi daha sonra __Turbo c++__ olmuştur. fakat proje sonlanmıştır. 

# Objective-C
bu şekilde de kısaltılır: ObjC yada Objective C yada Obj-C
Apple ürünlerinde kullanılan fakat platform bağımsız bir dildir. Nesneye yönelimlidir, fakat C dilinden türetilmiştir.

# standart kütüphaneler
bu kısım özellikle bir implementasyona göre yazılmadı.

- assert.h

  ```c
  scanf("%d", &a);
  assert(a >= 10);
  printf("Integer entered is %d\n", a);
  ```

- ctype.h
  - int isalnum(int c) - is alphanumeric
  - int isdigit(int c)
  - int toupper(int c)

- errno.h
  
  Bazı fonksiyonlar hata döndüklerinde genelde -1 dönerler. fakat aynı zamanda global errno değerine hata numarasını set ederler. bu integer değer errno.h içerisindedir.

  errno değeri thread-local'dir. bu sebeple multi-thread uygulamalarda her thread için farklı bir instance tutulur. bu sebeple excepiton kontrollerimizi buradan sorunsuzca yapabiliriz.

- limits.h
  
  aşağıdaki sabit değerleri barındırıyor
  - CHAR_BIT
  - LONG_MAX
  - LONG_MIN

- stdio.h

  dosya veya diğer stream'ler üzerine okuma yazma işlemleri yapabilmemizi sağlayan fonksiyonlar içerir
  - int fclose(FILE *stream)
  - int printf(const char *format, ...)

- stdlib.h

  Memory management, array sort, search gibi utility olarak gruplandırabileceğimiz fonksiyonlar içerir

  - void *calloc(size_t nitems, size_t size) - memory management fonksiyon
  - void exit(int status) - terminate the application
  - void *bsearch(const void *key, const void *base, size_t nitems, size_t size, int (*compar)(const void *, const void *)) - binary search
  - void qsort(void *base, size_t nitems, size_t size, int (*compar)(const void *, const void*)) - quick sort

- math.h

  - double tanh(double x)
  - double log(double x)

- complex.h

  matematikteki karmaşık sayılar üzerinde işlem yapmamızı sağlayan fonksiyonlar içerir

- stdint.h

  fixed-size variable'lar barındırır.

- inttypes.h

  stdint.h'i içinde zaten barındırıyor. bu kütüphane stdint içinde gelen fixed-size variable'ların printf, scanf gibi standart metodlarla çalışabilmeleri için formattırları barındırıyor. örnek: 
  
  > PRId64 = "ld"
  
  sabiti inttypes.h içerisindedir. bu değer ile standart c kütüphane metodu olan printf ile şunu yapabiliriz:

  ```c
  printf("%+"PRId64"\n", myInt64Var);
  ```

  C'de string birleştirmeyi derleyici bizim için yapıyor. dolayısı ile yukarıdaki printf metodu şuna tekabül ediyor:

  ```c
  printf("%+ld\n", myInt64Var);
  ```

- sysexits.h

  exit status integer'larını barındırıyor. konu başka başlıkta anlatılıyor.

- signal.h

  Sinyal handler fonksiyonunu ve sinyallerin integer değerlerini barındırıyor. konu başka başlıkta anlatılıyor.

# stdint.h

sabit boyutlar aşağıda tablodadır:

| Degisken | İşareti  | Bits | Bytes | Minimum Değer                      | Maksimum Değer                        |
|----------|----------|------|-------|------------------------------------|---------------------------------------|
| int8_t   | Signed   | 8    | 1     | −2^7 = −128                        | 2^7 − 1 = 127                         |
| uint8_t  | Unsigned | 8    | 1     | 0                                  | 2^8 − 1 = 255                         |
| int16_t  | Signed   | 16   | 2     | −2^15 = −32,768                    | 2^15 − 1 = 32,767                     |
| uint16_t | Unsigned | 16   | 2     | 0                                  | 2^16 − 1 = 65,535                     |
| int32_t  | Signed   | 32   | 4     | −2^31 = −2,147,483,648             | 2^31 − 1 = 2,147,483,647              |
| uint32_t | Unsigned | 32   | 4     | 0                                  | 2^32 − 1 = 4,294,967,295              |
| int64_t  | Signed   | 64   | 8     | −2^63 = −9,223,372,036,854,775,808 | 2^63 − 1 = 9,223,372,036,854,775,807  |
| uint64_t | Unsigned | 64   | 8     | 0                                  | 2^64 − 1 = 18,446,744,073,709,551,615 |

# hata yönetimi

c'de try-ctach tarzı bir mekanizma yoktur. eğer çağırdığımız bir fonksiyon -1 yada null dönerse errno değerini kontrol ederiz.

```c
#include <stdio.h>
#include <errno.h>
#include <string.h>

extern int errno ;

int main () {

   FILE * pf;
   int errnum;
   pf = fopen ("unexist.txt", "rb");
	
   if (pf == NULL) {
   
      errnum = errno;
      fprintf(stderr, "Value of errno: %d\n|", errno);
      perror("Error printed by perror");
      fprintf(stderr, "Error opening file: %s\n", strerror( errnum ));
   } else {
   
      fclose (pf);
   }
   
   return 0;
}

/*
output:

Value of errno: 2
Error printed by perror: No such file or directory
Error opening file: No such file or directory
*/
```

- perror aldığı string parametrenin yanına, o anda error no'ya denk gelen error string'i basıyor.

- strerror aldığı errorNo integer'ına denk gelen error String'inin pointer'ını dönüyor.

Sıfıra bölünme hatasında uygulama kendini kapatacatır. Bu sebeple önceden bölenin sıfır olup olmadığını manuel kontrol etmemiz gereklidir. Burada süreç şu şekilde işler:
- cpu işlemi gerçekleştiremez ve hata fırlatır
- OS bu hatayı yakalar ve yazılıma sinyal gönderir yada process kill edilir. bu OS'a kalmış bir durumdur. bazı OS'lar, processe SIGFPE (yada Signal Float Point Exception) sinyali yollar. bu sinyali handle edersek ve handler fonksiyonumuz execute edildikten sonra aynı satır (sıfıra bölünme satırı) tekrar işletilir. (handler bittiğinde neden devam ettiğini POSIX sinyalleri konusunda anlatılıyor)

C++'ta try/catch'lar vardır.

# static

bir değerin önüne static koyulursa o değer tüm program boyunca tekrar initialize edilmez. aşağıdaki örnekte "count" sadece 1 kere initialize olur.

```c
#include<stdio.h> 
int fun() 
{ 
  static int count = 0; 
  count++; 
  return count; 
} 
   
int main() 
{ 
  printf("%d ", fun()); 
  printf("%d ", fun()); 
  return 0; 
}

/*
output:

1 2
*/
```

# register

bir değerin önüne register keywordü koyulursa o değer RAM'de değil, cpu'nun register'larında saklanır. bu da o değere daha hızlı erişilmesini sağlar. fakat register'lar maliyetli fiziksel bellekler olduğundan, mümkün olduğunca az kullaılmalıdırlar.

Eğer register'da yer yok ise bu değer RAM'de oluşturulur ve hata fırlatmaz.

register'da saklanan değerlerin & operatürü ile adresini almamız mümkün diildir. çünkü ram adresleri yoktur.

# extern

extern bir değerin önüne atılırsa, o değer artık aynı isimde olan tüm değerlerle aynı yere referans edecektir. özellikle farklı kaynak dosyaları ile çalışıyorsak ve başka kaynak dosyanın global bir değişkenini değiştirmek istediğimizde extern iş görecektir.

diğer kaynak dosyasındaki değere referans eden değeri aynı isimde kendi kaynak dosyamızda oluştururuz. önüne extend koyarsak bu değer artık runtime sırasında aynı yere referans edecektir. "errno" değeri (konu başka başlıkta anlatılıyor) extern'dir ve extern'in kullanımı için çok iyi bir örnektir.

# auto

her değişkenin önüne eklenebilir fakat eklenmesede default identifier olduğu için etki etmeyecektir.

# const
sabit değerler yaratabilmek için kullanılır. java'daki final identifier'ına benzer.

# memory structure

```
High Addresses ---> .----------------------.
                    |      Environment     |  CommandLine Argument + Enviroment variables
                    |----------------------|
                    |                      |   variable are declared on the stack
                    |         STACK        |   
stack pointer ->    | - - - - - - - - - - -|
                    |           |          |
                    |           v          |
                    :                      :
                    .                      .   The stack grows down into unused space
                    .         Empty        .   while the heap grows up. 
                    .                      .
                    .                      .   (other memory maps do occur here, such 
                    .                      .    as dynamic libraries, and different memory
                    :                      :    allocate)
                    |           ^          |
                    |           |          |
                    | - - - - - - - - - - -|   Dynamic memory is declared on the heap
                    |          HEAP        |
                    |                      |
                    |----------------------|
                    |          BSS         |   Uninitialized data (BSS)
                    |----------------------|   
                    |          Data        |   Initialized data (DS)
                    |----------------------|
                    |          Text        |   Binary code
Low Addresses ----> '----------------------'  
```

C ile derlenmiş bir uygulama çalışırken, memory'deki görüntüsü yukarıdaki gibidir. bu parçaları incelersek:

- stack
  
  static olmayan değerler burada tutulur.

  her fonksiyonun bir stack bloğu vardır.

  LIFO mantığında çalışır. bu sebeple __stack pointer__ listenin tepesinin adresini tutar.

- heap

  memory allocation (alloc, realloc...) işlemleri ile buradan yer ayırtılır. buradaki değerler fonksiyonlardan bağımsızdır.

- BSS (yada Uninitialized data segment)

  static yada global olan fakat initialize olmamış değerler burada tutulur.

- DS (yada Initialized data segment)

  static veya global olan ve initialize olmuş değerler burada tutulur.

- Text

  kodumuzun binary hali burada tutulur. bu kısım read-only'dir. böylece yanlışlıkla buraya yazma gibi bir durum söz konusu olamaz.

# segmentation fault (yada segfault)
eğer memory'de erişmeye yetkimiz olmayan bir adrese erişmek istediğimizde bu hatayı alırız.

# neden fonksiyondan döndüreceğimiz değerleri memory allocation ile ayırlmalıyız

aşağıdaki örnekten gidelim:

```c
#include<stdio.h> 
int * createArray() { 
  int arr[10];
  return arr;
} 
   
int main() { 
  int * arr = createArray(); 
   
  arr[2] = 1; // bu satırda segfault hatası alırız
  
  return 0; 
}
```

Yukarıdaki kodda segfault alırız. çünkü; createArray içindeki arr değerinin adresini döndürdük fakat fonksiyon tamamlanınca, createArray __stack frame__'i bellekten siliniyor. bu sebeple oraya main fonksiyonumuzdan erişmeye kalkdığımızda hata alırız. bu sebeple createArray metodunda heap'ten ayıracağımız array'i döndürmeliyiz.

aynı durum alt metodlara dallandığımızda parametre gönderdiğimizde geçerli değildir. çünkü alt metoda dalışımızda, dalmadan önceki stack frame hala ramde kalmaya devam edecektir.

# fazlar (yada phases)
- preprocess
  
  derleme öncesi çalışır. başka başlıkta anlatılan keywordlerin (ifdef gibi) çalıştığı aşamadır. bu fazın çıktısı yine source koddur. fakat bir kısmı ifdef gibi tanımlar sebbei ile silinmiştir.

- compile

  kod makinenin anlayacağı dile çevriliyor. bu faz sonucunda __object code (yada object file)__ elde edilir.

  genelde __.o__ uzantılıdır. c'den türemiş bazı dillerde __.obj__ olarakta kullanılır.
  
  object code'da diğer kütüphaneler henüz linklenmediği için düzgün çalışamaz. linklenirse diğer kütüphaneler (metodlar vs..) tanımlı olacağı için sağlıklı çalışacaktır.

- link

  kütüphanelerin birleştirildiği/linklendiği fazdır. artık object code'umuz executable olmuştur. 

# enum

enum; enumerated'ın kısaltması olarak programlama dünyasına yansımıştır. enumerated; birer birer saymak anlamına gelmektedir.

```c
//tanımlama
enum state {
  working = 98,
  stoped = 99
};

//kullanımı
enum state personState;
personState = stoped;
```

İstenirse enum'lardaki gibi instance tanımlama satırında yapılabilir:

```c
//tanımlama
enum state {
  working = 98,
  stoped = 99
} personState;

//kullanımı
personState = stoped;
```

Eğer enum variable'larına değer verilmezse otomatik olarak 0, 1, 2 olarak set edilir. örnek:

```c
//tanımlama
enum state {
  working,  // working = 0
  stoped    // stoped = 1
} ;
```

# fork()
fork() fonksiyonu system call çağrısı yapar. yeni bir process yaratılır ve yeni proceess eski process ile aynı bellek değerlerine sahiptir. fork fonksiyonunun dönüş değeri yeni process'in ID'sidir.

# thread

```c
#include <pthread.h>
#include <unistd.h> // for sleep() only
#include <stdio.h> // for printf() only

void wait(void)
{
    sleep(2);
    printf("Done.\n");
}

int main(void)
{
    pthread_t thread;
    int err;

    err = pthread_create(&thread, NULL, wait, NULL);

    if (err)
    {
        printf("An error occured: %d", err);
        return 1;
    }

    printf("Waiting for the thread to end...\n");

    pthread_join(thread, NULL);

    printf("Thread ended.\n"); 

    return 0;
}
```

# mutex

```c
#include <pthread.h>
#include <unistd.h>
#include <stdio.h>

pthread_mutex_t lock;
int j;

void do_process()
{
    pthread_mutex_lock(&lock);
    int i = 0;

    j++;

    while(i < 5)
    {
        printf("%d", j);
        sleep(1);

        i++;
    }

    printf("...Done\n");

    pthread_mutex_unlock(&lock);
}

int main(void)
{
    int err;
    pthread_t t1, t2;

    if (pthread_mutex_init(&lock, NULL) != 0)
    {
        printf("Mutex initialization failed.\n");
        return 1;
    }

    j = 0;

    pthread_create(&t1, NULL, do_process, NULL);
    pthread_create(&t2, NULL, do_process, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# digital media container vs codec
container dosya formatına denk gelmektedir. bir dosya container'ı kendi içinde birden fazla ses, video, birden fazla subtitle, metadata (album ismi, şarkıcı ismi, album resimleri...) bulundurabilir. bazı container'lar sadece ses içerebilir. bazıları ise sadece resim içerebilir (resim dosyalarınında container'ı var).

codec ise container'ın içindeki sesin veya videonun standartıdır/formatıdır.

bazı container'lar birden fazla codec desteklemektedir.

bir dosya uzantısı, o dosyanın container'ından gelmektedir.

| container name                            | description                                                                                                                                                                                                            |
|-------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AIFF                                      | IFF file format, widely used on Mac OS platform                                                                                                                                                                        |
| WAV                                       | RIFF file format, widely used on Windows platform                                                                                                                                                                      |
| XMF                                       | Extensible Music Format                                                                                                                                                                                                |
| FITS                                      | Flexible Image Transport System - still images, raw data, and associated metadata.                                                                                                                                     |
| TIFF                                      | Tagged Image File Format - still images and associated metadata.                                                                                                                                                       |
| 3GP                                       | used by many mobile phones; based on the ISO base media file format                                                                                                                                                    |
| ASF                                       | container for Microsoft WMA and WMV, which today usually do not use a container                                                                                                                                        |
| AVI                                       | the standard Microsoft Windows container, also based on RIFF                                                                                                                                                           |
| DVR-MS                                    | "Microsoft Digital Video Recording", proprietary video container format developed by Microsoft based on ASF                                                                                                            |
| Flash Video (FLV, F4V)                    | container for video and audio from Adobe Systems                                                                                                                                                                       |
| IFF                                       | first platform-independent container format                                                                                                                                                                            |
| Matroska (MKV)                            | not limited to any coding format, as it can hold virtually anything; it is an open standard container format                                                                                                           |
| MJ2                                       | Motion JPEG 2000 file format, based on the ISO base media file format which is defined in MPEG-4 Part 12 and JPEG 2000 Part 12                                                                                         |
| QuickTime File Format                     | standard QuickTime video container from Apple Inc.                                                                                                                                                                     |
| MPEG program stream                       | standard container for MPEG-1 and MPEG-2 elementary streams on reasonably reliable media such as disks; used also on DVD-Video discs)                                                                                  |
| MPEG-2 transport stream ( a.k.a. MPEG-TS) | standard container for digital broadcasting and for transportation over unreliable media; used also on Blu-ray Disc video; typically contains multiple video and audio streams, and an electronic program guide        |
| MP4                                       | standard audio and video container for the MPEG-4 multimedia portfolio, based on the ISO base media file format defined in MPEG-4 Part 12 and JPEG 2000 Part 12) which in turn was based on the QuickTime file format. |
| Ogg                                       | standard container for Xiph.org audio formats Vorbis and Opus and video format Theora                                                                                                                                  |
| RM                                        | RealMedia; standard container for RealVideo and RealAudio                                                                                                                                                              |

# kayıpsız ses
__Pulse code modulation (yada PCM)__ kayıpsız olarak analog sesi, digital ortama kaydetme mantığıdır. bu bir format değildir. 

WAV formatı raw ses data'sını tutar. yani kayıpsızdır. Yani; PCM mantığındadır. PCM mantığında çalışan formatlar bellekten çok fazla yer kaplar. 5 mb'lik bir mp3 dosyası 500 mb'lik bir wav dosyasına denk gelmektedir.

FLAC gibi codec'ler kayıpsız sıkıştırma uygular. yani rar, zip tarzı sıkıştırma uygularlar. fakat sıkıştırılan dosya formatı belli olduğu için, sıkıştırma oranı çok yüksektir. örneğin; 500 mb WAV dosyası, kayıpsız şekilde sıkıştırılıp, 50 mb'lik bir flac dosyasına denk gelir.

Kayıplı sıkıştırmalar ise MP3 gibi dosyalardır. ses kalitesinden ödün vererek sıkıştırma uygularlar ve boyutları çok ufaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# BPM (yada Business Process Manager)

İş akışını yöteten otomasyonlara verilen genel isimdir.

Piyasadaki bazı BPM Platformları:

- IBM BPM (eskiden Lombardi firması tarafından geliştiriliyordu. IBM satın aldı.)

- Activiti (açık kaynaklı, Alfresco firmasın tarafından geliştiriliyor)

- camunda (açık kaynaklı)

BPM platformları; BPM Engine'ini, IDE'ler için eklentiler, programlama dilleri için engin'i kullanmayı sağlayan API'leri, desteklediği formatları (örnek: BPNM) ve daha bir çok şeyi içinde kapsamaktadır.

BPM platformlarının birçok avantajı vardır. İş akışını belirler ve iş akışı için client tarafa data yollar. Örneğin HTML yollayabilir, bu şekilde her client bunu parse edip ekranda ne göstereceğine karar verebilir. ATM, Mobil, masaüstü, web clientları tek bir iş akışı sunucusundan aynı endpoint'lerden yönetilebilir. Sadece client'a data yollamaz, aynı zamanda session ile ilişkilendirilip her user'ın hangi iş akışına girebileceğini, iş akışı ekranının client tarafta yanlışlıkla kapanması durumunda devam etmesini sağlayabilir. Benzer şekilde hangi user'ların hangi process'lerde ve hangi sayfada olduğunu monitör edip admin'lere sunabilir. istenildiğinde user'lar engellenebilir. performans ölçümü yapılmasını sağlayabilir.

# Business Process Model and Notation (veya BPMN)

İş akışını gösteren bir xml standartıdır. Sürümleri mevcuttur. 

bir bpmn dosyasının içinde birçok akış (yada flow) olablir.

her akış içerisinde bşrçok task olabilir. her task son kullanıcı çin birer sayfa gibi düşünülebilir. fakat öyle olmak zorunda diildir. client kendi için bir syafayı 2-3 sayfa içerisinde toplayıp sunucuya istekte bulunabilir. 

Gateway (geçiş) her tasktan taska giderken isteğe bağlı kullanılan karar mekanizmalarıdır. örnek diğer taska giderken, koşul koyulabilir. eğer koşula sağlanmıyorsa kullanıcı farklı bir taskta yönlendirilebilir.

flow, gateway, task gibi objeleri birçok türevi vardır.

bpm sürecini bir örnek ile açıklayalım: internetten kredi çekilebilsin. son kullanıcı banka sistemine login olur ve kredi sürecini başlatır. kullanıcıdan birçok bilgi alınıp son olarak onay tuşuna bastı. artık bu arada kredi henüz alınmadı. daha bankacının onay vermesi gerekmektedir. işte süreçte user 'a bir bildirim yapılıyor ve user daha ileri gidemiyor. ne zmana kredi çekmeye kalksa (yani süreci tamamlamak istese) "temsilcinizdenonay bekliyorsunuz" mesajını görüyor. artık bpm akışı bankacının akışına yönlenmiş durumda (bpmn formatında ok işareti). eğer bankacı kendi sürecini başlatırsa, ve kendi sürecinde olan kısmını tamamlarsa yine ok işareti müşterinini sürecinini içine ok işareti iel göstereceğinden, artık süreç müşteriden devam edecektir. müşteri artık kredi sayfasını açtığında karşısında raporu görecektir. rapor göstermede de sürecin (son) bir parçasıdır. eğer temsilci isterse kullanıcıyı başka bir akışa yönlendirip ek bilgi talebinde bulunabilirdi.  

sub-process, bir task gibi görünür aram referansı bir sub-process yani task grubudur. tekrar kullanılabilirlik sağlamak için kullanılırlar.

# camunda modeler 

İş akışlarını hazırlamaya yarayan masaüstü GUI yazılımıdır.

# kod örneği

aşağıdaki kod parçası Activity BPM Java kodudur:

```java
//engine'i devreye sokuyoruz

ProcessEngineConfiguration cfg = new StandaloneProcessEngineConfiguration()

        .setJdbcUsername("sa")

        .setJdbcPassword("");

    ProcessEngine processEngine = cfg.buildProcessEngine();

    String pName = processEngine.getName();

    String ver = ProcessEngine.VERSION;

//bir akış deploy ediyoruz

    RepositoryService repositoryService = processEngine.getRepositoryService();

    Deployment deployment = repositoryService.createDeployment()

        .addClasspathResource("moneytransfer.bpmn").deploy();

//processDefinition arama sonucunu içeriyor

    ProcessDefinition processDefinition = repositoryService.createProcessDefinitionQuery()

        .deploymentId(deployment.getId()).singleResult();

  

//money transfer akışının bir instance'ını başlatıyoruz

    RuntimeService runtimeService = processEngine.getRuntimeService();

    ProcessInstance processInstance = runtimeService

        .startProcessInstanceByKey("moneytransfer");

//bpm'in sunduğu bir çok servisi kullanmak için bir degere atıyoruz

    TaskService taskService = processEngine.getTaskService();

    FormService formService = processEngine.getFormService();

    HistoryService historyService = processEngine.getHistoryService();

//process dongusune girip tasklar cekiliyor ve her taksı içindeki data çekiliyor ve data service atılıyor

    while (processInstance != null && !processInstance.isEnded()) {

List<Task> tasks = taskService.createTaskQuery()

          .taskCandidateGroup("managers").list();

      System.out.println("Active outstanding tasks: [" + tasks.size() + "]");

      for (int i = 0; i < tasks.size(); i++) {

        Task task = tasks.get(i);

        System.out.println("Processing Task [" + task.getName() + "]");

        Map<String, Object> variables = new HashMap<String, Object>();

        FormData formData = formService.getTaskFormData(task.getId());

        for (FormProperty formProperty : formData.getFormProperties()) {

          if (StringFormType.class.isInstance(formProperty.getType())) {

            System.out.println(formProperty.getName() + "?");
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# swagger 

"SmartBear Software" tarafından geliştirilen REST API doc'u hazırlanması ve client/server API kodlarının otomatik oluşturulması amaçlı geliştirilen yazılım tool'larının bütünüdür.

# swagger definition file

rest API'lerin bilgilerinin swagger formatında yazıldığı JSON yada YAML dosyası.

# swagger editor

json dosyasını HTML'e çeviren (html API doc üreten) yazılımın adıdır. web arayüzlü bir tool'dur. yml'i verirsiniz, yan tarafta HTML sürümü görünür.

# Swagger Codegen

programlama dilinde yazılmış kodları yml dosyasına çeviren ve yml dosyasından server/client kodu hazırlayabilen yazılımır.

# Swagger UI

"swagger editor" 'ün arkaplanda kullandığı derleme yazılımının ismidir. swagger-ui sadece html/javascript'ten oluşan bir yazılımıdır. bu kodları sunucuya attığınızda, index.html'i üzerinden bir web sayfası açılır. bu sayfa sizden yml dosyasının url'sini ister. url'yi girdiğinizde, sayfa otomatik olarka o yml'yi çeker ve parse ederi. parse ettikten sonra, web syafasına REST API DOC'ları listeler. bu şekilde doc hazırlanmış olur.

# swagger parser
OpenAPI dosyasını alıp, onu parse edebilmemizi sağlayan java kütüphanesidir. örnek kullanım:

```java
OpenAPI openAPI = new OpenAPIV3Parser().read("./path/to/openapi.yaml");

//Read details of file from openAPI object 
```

# open API (yada public API)

üçüncü parti şirketlere API'ler hazırlanabiliyor. public olarak eirişilen API'ler. böylece üçüncü parti firmalar daha hızlı şekilde müşterilere entegrasyon sağlayabiliyorlar. bu keyword public API olduğunu tesmli ediyor. bu bir standart yada spesifikasyon değildir. 

# OpenAPI Specification (yada OAS yada eski adı:Swagger Specification)
swagger yml dosyasında bir standartı var. bu wsdl dosyasının benzeri bir yml dosyası. bu dosyanın formatını 3.0 sürümüne kadar swagger kendi belirliyordu. 3.0 ile OpenAPI olarakm yeni bir repo'da geliştirmeye başlandı. OpenAPI'nin ilk sürümü 3.0 ile başlamış oldu.

finans sektöründe kullanılan "open api" ile hiçbir bağlantısı yoktur. "OpenAPI Specification"'daki "open" kelimesi spesifikasyonun açık kaynaklı standartlara dayandığını temsil etmektedir. OAS sadece REST için bir spesifikasyondur.

OpenAPI repo'sunda eski sürümlerin doc'ları da yer almaktadır. onlar open api 2 diye yazılmış fakat swagger formatlarıdır.

# Springfox

java-spring projeleri için REST-DOC oluşturma parse kütüphanesidir. springfox bir interface sunuyor. Springfox kullanmak için springfox-swagger gibi implementasyonların projeye import edilmesi şarttır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# .net framework

sistemde yüklü olmalı ki, bu framework'ten yararlanan uygulama yürütülebilsin. aynı jvm'de olduğu gibi. .net, sadece c# değil, başka diller için de API'si mevcuttur. ".net" bir runtime'a ihtiyaç duyuyor. Aynı JVM gibi. bu runtime'ın ismi __Common Language Runtime (yada CLR)__. JVM bytecode'u çalıştırabilirken, ".net" __Microsoft intermediate language (yada MSIL)__'ı çalıştırmaktadır.

# .net core

.net'in sadece linux, mac ve windows'ta çalışılabilir bir türevidir. sadece windowsta çalışacak bir uygulama yazılacaksa .net tercih edilmelidir. ".net framework", .net core'a göre daha çok özellik içeriyor.

# .net standart

isim karışıklığına sebep oluyor fakat kendisi başlıca farklı bir kütüphane grubu. tüm platformlar (mobil/xamarin, desktop, core...) için ortak olan kütüphaneleri temsil ediyor.

# MONO

C# uygulamalarının windows harici sistemlerde derlenmesi ve çalıştırılması için gerekli tüm projeleri (IDE, api,...) kapsar. açık kaynaklı bir projedir.

Mono projesi .net'in açık kaynaklı  bir implementasyonunu içermektedir. fakat api'ler aynı olduğundan direk olarak uygulamaları derleyebilemkte ve yürütebilmektedir.

# MonoDevelop

mono projesi altında geliştirilen IDE.

# Xamarin

Mono projesi tabanlı bir framework. mobil platformlar için c#'ta yazılan uygulamaların diğer sistemlerde çalışabilmesini sağlıyor.

# ISS (yada Internet Information Services)

Microsoft'un web server'ı.

# ASP (yada Active Server Pages)

Microsoft'un jsf alternatifi framework'ü. 

# ASP.net

.net projesi'nin bir altkümesidir. .net'in sadece asp kümesini kapsar. 

# ASP.net core

.net core projesi'nin bir altkümesidir. .net core'un sadece asp kümesini kapsar. Bu yapı ile "ISS express" olarak adlandırılan portable ve windows harici sistemlerde çalışabilen bir ISS türevi geliştirilmiştir. böylece asp.net core ile cross platform sunucular çalıştırılabilmektedir.

# nuget

maven tarzı c# için dependency yöneticisi.

# steel toe

"spring cloud" mimarisindeki servislere entegrasyon sağlamak için hem sunucu hemde client kütüphanelerini içeren, c# için açık kaynaklı kütüphane.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Solr vs elastic search

Ikiside açık kaynaklı birbirine alternatif arama indexleyici ve döndüren yazılımlardır.

Solr'da field isimleri ve field type'lar baştan belirtilmek zorundadır. Yeni field eklemeye kalktığınızda bütün kayıtları tekrar Solr'a aktarmanız gerekiyor. elastic'te böyle bir durum yok. Çünkü elastic scheme barındırmıyor. No-sql-db gibi.

# elasticsearch

### index

normal veritabanındaki tablo'ya denk gelen kavramdır. en üst seviye bilgi grubudur. her index'e bir alias atılmak zorunda değildir. fakat atılması mutlaka önerilir. bir sistemde min 1 index olmalıdır. kaynak: https://www.elastic.co/guide/en/elasticsearch/reference/current/glossary.html (archive date: 26/12/2019)

### Mapping

normal veritabaındaki şemaya denk gelir.

### Document

normal veritabaındaki tablo içindeki kayda denk gelir.

### Type

Document'in tipini belirtir. user, tweet tipinde farklı document'ler olabilir.

### field

normal veritabanındaki her tablo içinde olan bir sutuna denk gelir. elastic no-sql olduğu için json tutar ve her field bir json propertie'dir.

### kayıt eklemek

elastic search önceden şema istemediğinden attığımız yeni kaydı direk kabul eder ve bundan arama yapılabilmesini kendisi sağlar. yeni kaydımızı Rest api'si ile ekleyelim:

> PUT ELASTIC_SEARCH_URL/customer/_doc/1

```json
{
  "name": "John Doe"
}
```

dönüşü:

```json
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 26,
  "_primary_term" : 4
}
```

şimdi kaydımızı çekelim:

> GET /customer/_doc/1

dönüşü:

```json
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 26,
  "_primary_term" : 4,
  "found" : true,
  "_source" : {
    "name": "John Doe"
  }
}
```

Birçok döküman indexlenecek ise, tek tek yerine performans açısından "_bulk" apisi tercih edilmelidir.

Birçok kayıt indexlemiş olalım. Şimdi bu kayıtlarımızı arayalım:

> GET /bank/_search

```json
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```

dönüşü:

```json
{
  "took" : 63, //how long it took Elasticsearch to run the query, in milliseconds
  "timed_out" : false, // request timeout or not.
  "_shards" : { // how many shards were searched and a breakdown of how many shards succeeded, failed, or were skipped
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
        "value": 1000,
        "relation": "eq"
    },
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, ...
    ]
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# exif (yada Exchangeable image file format)

Media dosyaları için kullanılan aynı dosyaya gömülen metada bilgilerinin formatıdır. "Exiftool" açık kaynaklı komut satırı programıdır. Bu program ile dosyalarının exif bilgileri okunup manipüle edilebilmektedir.

Exif bilgileri arasında birçok çeşit bilgi vardır. Örneğin; fotoğrafın çeken device bilgileri, fotoğrafın çekildiği saat, lokasyon...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Veri yapıları

# Linked-List vs Double-Linked-List vs Dairesel (Circle) Linked-List

Linked-List sadece bir sonraki elemanın adresini içeriyorken, double linked-list bir önceki elemanında adresini içeriyor. performans açısından avantaj sağlarken bellekten zarar etmektedir.

Dairesel Linked-List ise son elemanı listenin başındaki elemana refernas etmektedir. Dairesel Linked-List'ler isteğe bağlı double olabilirler.

# Stack (yada yığın yada yığıt yada çıkın)

Nesne ekleme ve çıkarmalarının sadece en üstten (top) yapıldığı veri yapısına denir. LIFO mantığı işler.

en üstteki elemanlara bu işlemler yapılabilir. push ile eleman eklenir, pop ile eleman çıkarılır, peek (yada top) ile eleman çekilir.

# Kuyruk (yada queue)

FIFO mantığında çalışır. Kuyruğun ara elemanlarına erişim yoktur. Son'a eklenir (enqueue) yada ilk eleman çıkarılır (dequeue).

# graf

Node'lar birbirlerine istedikleri gibi (döngüsel dahi) bağlanabilir. bi node 10 tane node'a, aynı graph'ta bir node, sadece 1 noda'a bağlı olabilir. 

bellekte tutulurken 2 şekilde tutulabilir:

- komşuluk matrisi: çok yer kaplar. fakat komşuluk tespit etme kısa sürer.

```
   A B C

A  0 1 0

B  1 1 1 

C  0 1 1
```

- komşuluk listesi: az yer kaplar. fakat komşuluk tespiti yavaştır. veri yapısı oluşturmak daha komplekstir.

A -> B

B -> C -> D

# ağaç (yada tree)

bir node'a gitmek ulaşabilmek için sadece 1 faklı yol olmalıdır. bazı tanımlar:

__Yaprak (yada Leaf)__: Çocuğu olmayan düğümdür

__Kardeş (yada Sibling)__: Ebeveyni aynı olan düğümlerdir.

__Ata (yada Ancestor)__: Bir d düğümünün ataları, d'den köke olan yol üzerindeki tüm düğümlerdir.

__Ayrıt (yada edge)__: düğümler arası ilişkiye verilen isimdir.

__descendant (yada torunlarıdır)__: bir düğümünü çocuklarına bağlı tüm alt düğümlerdir.

__level (yada düzey yada seviye)__: bir düğümün köke olan uzaklığıdır.

__height (yada yükseklik)__: herhangi bir x node'unun yüksekliği, torunları arasında ona en uzak olan yaprağa olan uzaklığıdır.

__derinlik (yada depth)__: bir ağacın derinliği ona en uzak olan yapara olan uzaklıktır.

## ağaç çeşitleri

   - ### Binary Tree (yada İkili Ağaç)
   
     İkili ağaçta bir düğümün __sol çocuk__ ve __sağ çocuk__ olmak üzere en fazla iki çocuğu olabilir

   - ### Full Binary Tree

     bir binary tree çeşididir. her düğümün aynı seviyede derinliği olmalıdır ve her düğümün mutlaka 2 çocuğu vardır. ağaç dengeli çizildiğinde her taraftan aynı boyutta görünür. örnek:

     ```
           4 
          / \
         /   \
        2     5 
       / \   / \
      1   3 4   6
     ```

   - ### Complete Binary Tree

     binary tree türevidir. yapraklar arasındaki deirnlik farkı en fazla 1'dir. her eklenen düğüm sol'dan sağa doğru eklenir. 1 derinlik bitmeden diğerine geçilmez. örnekler:

     ```
           4 
          / \
         /   \
        2     5 
       / \   / \
      1   3 4   

      
           4 
          / \
         /   \
        2     5 
       / \   / \
      1       
     
     ```

   - ### Height Balanced Binary Tree

     Her düğümünü sol alt ağacının yüksekliği ile sağ alt ağacının yüksekliği ile farkı en fazla 1'dir. örnekler:

     ```
           4 
          / \
         /   \
        2     5 
       / \   / \
      1   3     6

                        4 
                       / \
                      /   \
                     /     \
                    /       \
                  2           5 
                /   \       /   \
                                 6

                        1 
                       / \
                      /   \
                     /     \
                    /       \
                  1           1 
                /   \       /   \
               1                 1

                        1 
                       / \
                      /   \
                     /     \
                    /       \
                  1           1 
                /   \       /   \
               1           1     
                          /     
                          1
     ```

   - ### Yığın Ağacı (yada Heap tree yada öbek yada binary heap)

     bir binary tree türevidir. her düğüm, tüm alt düğümlerindenbüyük olan ağaçtır.

     herhangi bir ağacı, "yığın ağacı" kuralların uygun hale getirmek için yapılan işlemlere __yığınlamak (yada heapify)__ denir.

   - ### İkili Arama Ağacı (yada Binary Search Tree)

     binary tree çeşididir. her düğümün sol çocuğu sağ çocuğundan büyük olan ağaçtır. babanın alttakilerdenbüyük olup olmaması önemli diildir.

## ağaç içinde gezinme

   - Pre-order Traversal (root-left-right)
        - Kökü ziyaret et
        - Sol alt ağacı dolaş
        - Sağ alt ağacı dolaş
   - In-order Traversal (left-root-right)
        - Sol alt ağacı dolaş
        - Kökü ziyaret et
        - Sağ alt ağacı dolaş
   - Post-order Traversal (left-right-root)
        - Sol alt ağacı dolaş
        - Sağ alt ağacı dolaş
        - Kökü ziyaret et
    Level-order Traversal (Breadth-first)
        - Kökü ziyaret et
        - Soldan sağa ağacı dolaş

    örnekte incelersek:

    ```
                        F 
                       / \
                      /   \
                     /     \
                    /       \
                  B          G 
                /   \         \
               A     D         I
                    / \       /
                   C   E     H
    ```

    Pre-order   : F, B, A, D, C, E, G, I, H

    In-order    : A, B, C, D, E, F, G, H, I

    Post-order  : A, C, E, D, B, H, I, G, F

    Level-order : F, B, G, A, D, I, C, E, H

# Veri yapıları üzerinde yapılan işlemlerin algoritma değerlendirmeleri

| Data Structure     | (Average) Access | (Average) Search | (Average) Insertion | (Average) Deletion | (Worst) Access | (Worst) Search | (Worst) Insertion | (Worst) Deletion | (Worst) Space Complexity |
|--------------------|------------------|------------------|---------------------|--------------------|----------------|----------------|-------------------|------------------|--------------------------|
| Array              | Θ(1)             | Θ(n)             | Θ(n)                | Θ(n)               | O(1)           | O(n)           | O(n)              | O(n)             | O(n)                     |
| Stack              | Θ(n)             | Θ(n)             | Θ(1)                | Θ(1)               | O(n)           | O(n)           | O(1)              | O(1)             | O(n)                     |
| Queue              | Θ(n)             | Θ(n)             | Θ(1)                | Θ(1)               | O(n)           | O(n)           | O(1)              | O(1)             | O(n)                     |
| Singly-Linked List | Θ(n)             | Θ(n)             | Θ(1)                | Θ(1)               | O(n)           | O(n)           | O(1)              | O(1)             | O(n)                     |
| Doubly-Linked List | Θ(n)             | Θ(n)             | Θ(1)                | Θ(1)               | O(n)           | O(n)           | O(1)              | O(1)             | O(n)                     |
| Skip List          | Θ(log(n))        | Θ(log(n))        | Θ(log(n))           | Θ(log(n))          | O(n)           | O(n)           | O(n)              | O(n)             | O(n log(n))              |
| Hash Table         | N/A              | Θ(1)             | Θ(1)                | Θ(1)               | N/A            | O(n)           | O(n)              | O(n)             | O(n)                     |
| Binary Search Tree | Θ(log(n))        | Θ(log(n))        | Θ(log(n))           | Θ(log(n))          | O(n)           | O(n)           | O(n)              | O(n)             | O(n)                     |
| Cartesian Tree     | N/A              | Θ(log(n))        | Θ(log(n))           | Θ(log(n))          | N/A            | O(n)           | O(n)              | O(n)             | O(n)                     |
| B-Tree             | Θ(log(n))        | Θ(log(n))        | Θ(log(n))           | Θ(log(n))          | O(log(n))      | O(log(n))      | O(log(n))         | O(log(n))        | O(n)                     |
| Red-Black Tree     | Θ(log(n))        | Θ(log(n))        | Θ(log(n))           | Θ(log(n))          | O(log(n))      | O(log(n))      | O(log(n))         | O(log(n))        | O(n)                     |
| Splay Tree         | N/A              | Θ(log(n))        | Θ(log(n))           | Θ(log(n))          | N/A            | O(log(n))      | O(log(n))         | O(log(n))        | O(n)                     |
| AVL Tree           | Θ(log(n))        | Θ(log(n))        | Θ(log(n))           | Θ(log(n))          | O(log(n))      | O(log(n))      | O(log(n))         | O(log(n))        | O(n)                     |
| KD Tree            | Θ(log(n))        | Θ(log(n))        | Θ(log(n))           | Θ(log(n))          | O(n)           | O(n)           | O(n)              | O(n)             | O(n)                     |
# Expression Tree (yada İfade Ağaçları)

```
                        + 
                       / \
                      /   \
                     /     \
                    /       \
                   *         7
                 /   \
                +     c
               / \
              a   b
```

```
infix   : (a+b)*c + 7 (in-order-traversal uygulandığında bu sonuç elde edilir)

prefix  : +*+abc7     (Pre-order traversal)

postfix : ab+c*7+     (post-order traversal)
```

# Infix vs Prefix vs Postfix

Aritmetik işlemler 3*8+9 gibi derleyiciler tarafından çözümlenirken  bir sıralamaya koyulmaktadırlar. Infix vs Prefix vs Postfix, bu sıralama (notasyon) gösterimlerine verilen 3 alternatiftir. Bazı notasyonlara göre önce işaretler yer almaktadır gibi… Notasyonları gerçekleştirirken ve çözümlerken veri yapılarından yararlanılır.

# infix

matematiksel hesaplamalar (örnek: 3+(2*4) ) insanın okuyabileceği türden yazıldıklarında infix formatındadırlar. derleyiciler bu hespalamaları yapabilmek için bunları  bir formata çevirir. bunlardan bazı formatlar: Prefix notasyonu (yada polish notation), Postfix notasyonu (yada RPN yada reverse polish notation). Bu iki örnek notasyonda da paranteze ihtiyaç yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# algoritma performansı

## algoritma karmaşıklığı (yada algorithm Complexity)
time Complexity (performans) ve space Complexity (bellek kullanımı) gibi faktörlerin değerlendirmeleridir.

- O (yada Büyük O yada Big-Oh) - En kötü durum analizi

  - en kötü durum için algoritmanın içinde if/else ile ayrılan kısımlarda en yavaş olan kısım seçilir.

- Θ (Teta) - Ortalama durum analizi

- Ω (Omega) - En iyi durum analizi

- küçük-omega ve küçük-o - en iyi en kötü durumların kısmen tolere edilmiş analizleridir.

Hesaplamalar yapılırken, asimptotik üst sınır ve alt sınırlar kullanılır. (asimptot'lar başka başlıkta anlatılıyor)

## örnek notasyon gösterimi

f(x) = 6x^4 + 9x^4 + 80 = O(x^4) = Θ(x)

olarak ifade edilir. Bu gösterime göre; "O" bir fonksiyon değildir, sadece bir gösterimdir.

## büyüme oranları

1 < logn < n < n*logn < n^2 < 2^n < n!

| n      | (constant) 1 | (logarithmic) logn | (logarithmic) log_2 n | (linear) n | (logarithmic) N-log-n | (quadratic) n^2 | (cubic) n^3 | (exponential) 2^n |
|--------|--------------|--------------------|-----------------------|------------|-----------------------|-----------------|-------------|-------------------|
| 1      | 1            | 0                  | 0                     | 1          | 0                     | 1               | 1           | 2                 |
| 2      | 1            | 0.3                | 1                     | 2          | 0.6                   | 4               | 8           | 4                 |
| 3      | 1            | 0.4                | 1.5                   | 3          | 1.4                   | 9               | 27          | 8                 |
| 10     | 1            | 1                  | 3                     | 10         | 10                    | 100             | 1E+03       | 1024              |
| 100    | 1            | 2                  | 7                     | 100        | 200                   | 1E+04           | 1E+06       | 1E+30             |
| 1000   | 1            | 3                  | 10                    | 1E+03      | 3E+03                 | 1E+06           | 1E+09       | 1E+301            |
| 10000  | 1            | 4                  | 13                    | 1E+04      | 4E+04                 | 1E+08           | 1E+12       | too big!          |
| 100000 | 1            | 5                  | 17                    | 1E+05      | 5E+05                 | 1E+10           | 1E+15       | too big!          |
| 1E+6   | 1            | 6                  | 20                    | 1E+06      | 6E+06                 | 1E+12           | 1E+018      | too big!          |
| 1E+7   | 1            | 7                  | 23                    | 1E+07      | 7E+07                 | 1E+14           | 1E+021      | too big!          |
| 1E+8   | 1            | 8                  | 27                    | 1E+08      | 8E+08                 | 1E+016          | 1E+024      | too big!          |
| 1E+11  | 1            | 11                 | 37                    | 1E+11      | 1E+12                 | 1E+022          | 1E+033      | too big!          |
| 1E+25  | 1            | 25                 | 83                    | 1E+025     | 3E+26                 | 1E+050          | 1E+075      | too big!          |

# kodun hesaplanması

```java

// O(1)
print(hello)

// O(n)
for ( i = 0; i < n; i++) {
    
    print(hello)
}

// O(n2)
for ( i = 0; i < n; i++) {
    
    for ( i = 0; i < n; i++) {
    
        print(hello)
    }
}

// O(logn)
for ( i = 0; i < n; i=i*2) {
    
    print(hello)
}
```

Yukarıdaki kodun toplamı:
> O(1) + O(n) + O(n2) + O(logn) = 1 + n + n2 + logn = O(n2 + logn) = O(logn)

Yukarıdaki notasyonda O'nun içindeki değerler, hesaplamamızdaki hassasiyete kalmıştır. Eğer hassas hespalama yapılacak ise, yada düşük seviyelerdeki n değerleri için çalışılacaksa, hassasiyeti arttırıp O(n + n2 + logn) de yazabilirdik.

O(n2) = 0(n3) eşitliği de doğrudur. Fakat üst limiti en düşük tutmak daha yakın değerler verecektir.

# logn
- binary search algortimasından yola çıkalım. 16 adetlik bir dizimiz olsun ve worst case'e yakalanalım.
- elemanı bulamadığımız her adımda diziyi ikiye bölüyoruz.
- en kötü durumda 4 kere diziyi bölmemiz yetecek.
- bu durumda 16'yı dört kere 4'e bölüyoruz: 16 * (1/2)^4 = 1
- şimdi bu denklemi n elemanlı dizi için genelleyelim: n * (1/2)^k = 1.
- bu denklemde k benim kaç kere döngüye gireceğimin sayısı. biz bu sayıyı arıyoruz. bu sebeple denklemi çözersek; k = log_2 n (log 2 tabanında n)
- log_2 n asimptotu logn olduğundan binary search için karmaşıklık O(logn)'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# arama algoritmaları

- ## İkili Arama Algoritması (yada Binary Search Algorithm)

Sıralı bir dizide arama algoritmasıdır. __parçala fethet (divide and conquere)__ mantığı işler. aramaya dizinin ortasından başlanır. aranan değerden küçükse, küçük olan taraf'ta en ortaya bakılır. büyükse büyük olan yönde en ortaya bakılır.

- ## doğrusal arama Algoritması  (yada linear search Algorithm yada sıralı arama yada Sequential Search)

dizinin yapısı önemli değildir. baştan sona tek tek eleman karşılaştırılarak arama yapılır.

## Karşılaştırma tablosu:

| name   | best | average | worst | worst space |
|--------|------|---------|-------|-------------|
| binary | 1    | log n   | log n | 1           |
| lineer | 1    | n       | n     | 1           |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sıralama (yada Sorting) Algortitlamaları

- ## Kabarcık Sıralaması (yada Baloncuk sıralaması yada Bubble Sort)

Diziyi eleman sayısı kadar dolaşır. Her dolaşmada her elemanı sağ taraftaki ile kontrol edip yer değiştirtir. O(n^2)'dir.

- ## Seçerek Sıralama (yada Selection Sort)

Diziyi sürekli dolaşır. En ufak elemanı geçici bir dizinin başına atar. Daha sonraki dolaşımlarda tekrar en ufak elemanı bulur geçici dizideki 2inci elemana atar. O(n^2)'dir.

- ## Hızlı Sıralama Algoritması (yada Quick Sort Algorithm)

bir eleman seçilir. bu eleman pivot (türçe anlamları: en önemli nokta, püf nokta, çark etmek, eksen üzerinde dönmek) adı verilir. bu elemanın hangi eleman seçileceği hakkında birçok farklı görüş mevcuttur.

bu elemanın sağ tarafında ondan büyük elemanlar toplanır ve sol tarafında ise küçük elemanlar gruplanır. aynı işlem sağ ve sol taraf içinde yapılacaktır. böyle olunca en ufak parçalara (3 elemana düşünceye kadar) bölündüğünde tekrar birleştirdiklerinde dizi sıralanmış olacaktır.

- ## Birleştirme Sıralaması (Merge Sort)

Diziyi sürekli ikiye böler. her elemanı kendi içinde 2 şerli şekilde sıralar. SOnra bunları birleştirir. Birleştirirken de elemanların büyüklüklerini değerlendirir.

- ## Yığınlama Sıralaması (yada Heap Sort)

öncelikle sıralanması istenen dizi heap ağacı kurallarına uygun şekilde ağaç haline getirilir. heap ağacı hale getirilmesi kısadır. büyük olan üstte ufak olan altınd aşeklinde dallandırılacaktır. fakat ağaç bir dizi yapısı olmadığından henüz elimizde sıralı bir dizi yoktur. bunu dizi hale getirmek gerekmektedir. heap ağacının en üstündeki en üst sayı olacağından, sonuc kümesine en büyük rakam olarak kaydedilir. daha sonra ağaçın en üstteki node'u silerek ağaç tekrar heapfy işlemine sokulur. bu sefer ağacın en üstündeki değer sonuç kümemizin ikinci en büyük sayısı olmuş olur. bu şekilde tüm ağaç sıfırlandığında sonuç kümemizde sıralanmış bir dizi olacaktır.

## Algoritma karşılaştırma tablosu

| Algoritma adı         | best                                    | Average                 | Worst                                            | Memory                                                                                           | Stable                                                      | method                   | Other notes                                                                                                                                                                                                                                           |
|-----------------------|-----------------------------------------|-------------------------|--------------------------------------------------|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Quicksort             | n logn (variation is n)                 | n log ⁡ n               | n 2                                              | log ⁡ n on average, worst case space complexity is n; Sedgewick variation is log ⁡ n worst case. | Typical in-place sort is not stable; stable versions exist. | Partitioning             | Quicksort is usually done in-place with O(log n) stack space.                                                                                                                                                                                         |
| Merge sort            | n log ⁡ n                               | n log ⁡ n               | n log ⁡ n                                        | n (A hybrid block merge sort is O(1) mem.)                                                       | Yes                                                         | Merging                  | Highly parallelizable (up to O(log n) using the Three Hungarians' Algorithm or, more practically, Cole's parallel merge sort) for processing large amounts of data.                                                                                   |
| In-place merge sort   | —                                       | —                       | n log 2 ⁡ n (for hybrid is n log ⁡n)             | 1                                                                                                | Yes                                                         | Merging                  | Can be implemented as a stable sort based on stable in-place merging.                                                                                                                                                                                 |
| Heapsort              | n (If all keys are distinct, n log ⁡ n) | n log ⁡ n               | n log ⁡ n                                        | 1                                                                                                | No                                                          | Selection                |                                                                                                                                                                                                                                                       |
| Insertion sort        | n                                       | n 2                     | n 2                                              | 1                                                                                                | Yes                                                         | Insertion                | O(n + d), in the worst case over sequences that have d inversions.                                                                                                                                                                                    |
| Introsort             | n log ⁡ n                               | n log ⁡ n               | n log ⁡ n                                        | log ⁡ n                                                                                          | No                                                          | Partitioning & Selection | Used in several STL implementations.                                                                                                                                                                                                                  |
| Selection sort        | n 2                                     | n 2                     | n 2                                              | 1                                                                                                | No                                                          | Selection                | Stable with O ( n )  extra space or when using linked lists                                                                                                                                                                                           |
| Timsort               | n                                       | n log ⁡ n               | n log ⁡ n                                        | n                                                                                                | Yes                                                         | Insertion & Merging      | Makes n comparisons when the data is already sorted or reverse sorted.                                                                                                                                                                                |
| Cubesort              | n                                       | n log ⁡ n               | n log ⁡ n                                        | n                                                                                                | Yes                                                         | Insertion                | Makes n comparisons when the data is already sorted or reverse sorted.                                                                                                                                                                                |
| Shell sort            | n log ⁡ n                               | Depends on gap sequence | Depends on gap sequence (best known is n 4 / 3 ) | 1                                                                                                | No                                                          | Insertion                | Small code size, no use of call stack, reasonably fast, useful where memory is at a premium such as embedded and older mainframe applications. There is a worst case O ( n ( log ⁡ n ) 2 )  gap sequence but it loses O ( n log ⁡ n ) best case time. |
| Bubble sort           | n                                       | n 2                     | n 2                                              | 1                                                                                                | Yes                                                         | Exchanging               | Tiny code size.                                                                                                                                                                                                                                       |
| Binary tree sort      | n log ⁡ n                               | n log ⁡ n               | n log ⁡ n  (balanced)                            | n                                                                                                | Yes                                                         | Insertion                | When using a self-balancing binary search tree.                                                                                                                                                                                                       |
| Cycle sort            | n 2                                     | n 2                     | n 2                                              | 1                                                                                                | No                                                          | Insertion                | In-place with theoretically optimal number of writes.                                                                                                                                                                                                 |
| Library sort          | n                                       | n log ⁡ n               | n 2                                              | n                                                                                                | Yes                                                         | Insertion                |                                                                                                                                                                                                                                                       |
| Patience sorting      | n                                       | —                       | n log ⁡ n                                        | n                                                                                                | No                                                          | Insertion & Selection    | Finds all the longest increasing subsequences in O(n log n).                                                                                                                                                                                          |
| Smoothsort            | n                                       | n log ⁡ n               | n log ⁡ n                                        | 1                                                                                                | No                                                          | Selection                | An adaptive variant of heapsort based upon the Leonardo sequence rather than a traditional binary heap.                                                                                                                                               |
| Strand sort           | n                                       | n 2                     | n 2                                              | n                                                                                                | Yes                                                         | Selection                |                                                                                                                                                                                                                                                       |
| Tournament sort       | n log ⁡ n                               | n log ⁡ n               | n log ⁡ n                                        | n                                                                                                | No                                                          | Selection                | Variation of Heap Sort.                                                                                                                                                                                                                               |
| Cocktail sort         | n                                       | n 2                     | n 2                                              | 1                                                                                                | Yes                                                         | Exchanging               |                                                                                                                                                                                                                                                       |
| Comb sort             | n log ⁡ n                               | n 2                     | n 2                                              | 1                                                                                                | No                                                          | Exchanging               | Faster than bubble sort on average.                                                                                                                                                                                                                   |
| Gnome sort            | n                                       | n 2                     | n 2                                              | 1                                                                                                | Yes                                                         | Exchanging               | Tiny code size.                                                                                                                                                                                                                                       |
| UnShuffle Sort        | n                                       | kn                      | kn                                               | In-place for linked lists. n * sizeof(link) for array. n+1 for array?                            | No                                                          | Distribution and Merge   | No exchanges are performed. The parameter k is proportional to the entropy in the input. k = 1 for ordered or reverse ordered input.                                                                                                                  |
| Franceschini's method | —                                       | n log ⁡ n               | n log ⁡ n                                        | 1                                                                                                | Yes                                                         | ?                        |                                                                                                                                                                                                                                                       |
| Block sort            | n                                       | n log ⁡ n               | n log ⁡ n                                        | 1                                                                                                | Yes                                                         | Insertion & Merging      | Combine a block-based O ( n )  in-place merge algorithm with a bottom-up merge sort.                                                                                                                                                                  |
| Odd–even sort         | n                                       | n 2                     | n 2                                              | 1                                                                                                | Yes                                                         | Exchanging               | Can be run on parallel processors easily.                                                                                                                                                                                                             |
• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# protocol vs format vs codec (yada Çözücü)

format dosyaların yapısını(standartlarını) belirlerken, protokol communication yapılarını belirlemektedir.

codec terimi ise; "coder-decoder" ın birleşiminden gelir. siyanl veya digital olan datayı, bir formdan başka bir forma convert(decode/encode) eden yazılım/yöntem/araç'tır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# popup

açılan ufak pencrelerin tümüne verilen genel isimdir.

# Modal vs Dialog

ikiside aynı anlama gelmektedir. Pencerede kullanıcı için focus olunan bir popup açılır. Core HTML'in dialog özelliği mevcut (Önemli: Alert değil! İçine istediğimiz objeleri atabildiğimiz popup  var.). Fakat bootstrap gibi kütüphaneler kendi dialog'larını da yazmıştır.

# Popover vs Tooltip

ikiside aynı anlama geliyor. Genelde fare ile bir objenin üzerinde durulduğunda açıklaması için açılan popup'a verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# International Standard Name Identifier (yada ISNI)

birçok media tipinde verilere yada ürünlere id atamak için kullanilan sistemerin tümüdür (ailesidir). bazi medya/ürün tipleri su sekildedir:

- International Standard Book Number (yada ISBN) (sadece kitap)

- Serial Number (dergi, gazete, website, database, yayin, makale, blog)

- Text Code (metin çalismalari)

- Musical Work Code

- Music Number

- Recording Code (video veya ses kayitlari)

- Wine Number (şarap)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sistem Programcılığı

Daha çok işletim sistemi ile yada donanım bilgisi gerektiren yazılım dilleri ve kütüphaneleri kullanarak yazılım geliştirmedir. Bu sebeple; genelde üst seviyeli diller buna gruba dahil olmaz. Bu sebeple; genelde sürücü (driver) yazanlar bu gruba dahildir.

# sistem çağrıları (yada çekirdek çağrıları)

İşletim sistemi ile kullanıcı programları arasında tanımlı olan arayüz, işletim sistemi tarafından tanımlanan bir prosedürler kümesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Controller Area Network (yada CAN bus)

bir iletişim prtokolüdür. genelde araçlarda her birim arası haberleşme için tercih edilir. 

her birim ağı sürekli dinler ancak ağ boş ise mesajını tüm ağa yollar. her birim kendi alacağı mesajı önceden bilir ve kendisini ilgilendirmeyen mesajları dikkate almaz. haberleşme limitli max 1km uzunlukta kablo ve 1mb/sec hızında gerçekleşmektedir. çünkü sürekli ağı dinleyen birimler vardır. eğer ağın boş olduğunu düşünüp, birden fazla birim aynı anda mesaj yollamaya kalkarsa hata (çatışma - collision) meydana gelir. bu sebeple kablo uzunluğu ve veri boyutu sınırlı tutulur. bu şekilde ağ dinlemelerinde uzaktan gelen veriden habersiz olma gibi bir durum söz konusu olmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# file type detection

dosya formatını tanımak %100 olarak mümkün diildir. bazı uygulamalar her format için dosya kalıplrını (format yapısını-template'ini) inceliyor ve benzerlik oranına göre sonuç döndürüyor.

bazı dosya formatları genelde dosyanın en başına dosya formatını yazan birkaç bitlik bir yer bulundururlar. fakat bu bile bir dosyanın %100 txt olmayacağı anlamına gelmez.

# binary file

saf text olmayan bütün dosyalara verilen genel isimdir. anlamlı hale getirebilmek için basit yada komplex parse işlemleri yapılmalıdır.

pdf, word, zip, exe, bin dosyaları binary'dir. oysa json, xml, java, csv, sh dosyaları text dosyalarıdır. 

dosya sistemleri seviyesinde bir farklılık yoktur. sadece son kullanıcı için "text dosyası" ve "binary dosyası" olarak gruplandırılmışlardır.

bazı proramlama dilleri API'lerinde dosya okuma işlemlerinde binary olup olmadığı bilgisini isteyebilir. bunun birkaç sebebi var:

- eğer binary dosya açılırsa; satır sonu karakterlerini işletim sisteminin belirlediği default karaktere göre otomatik değiştirmez

- eğer binary açılırsa; end-of-file karakteri en sona otomatik koyulmaz.

- eğer binary açılırsa; write read metodları byte okur, oysa text açılırsa belli bir encoding'de string döner. 

her API farklı kuraller/tepkiler gösterebilir. bu sebeple API'yi incelemek gerekli. bazı kütüphaneler byte ve text için ayrı ayrı sınıf/metod sunuyor olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Steganografi (yada Steganography)

bilgiyi gizleme (önemli: şifreleme değil) bilimine verilen addır. Steganografi'nin şifrelemeye göre en büyük avantajı bilgiyi gören bir kimsenin gördüğü şeyin içinde önemli bir bilgi olduğunu farkedemiyor olmasıdır, böylece içinde bir bilgi aramaz. Bilişim dünyasında kullanılır. örnein; seste veya görüntüdeki küçük bozuklukları insan beyni farkedemediği için, kasıtlı olarak periyodik bozukluklar şeklinde dosyanın içine başka bir dosya saklanabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Cache (yada Önbellek)

yazılımsal ve donanımsal olarak ayrılmaktadır. donanımsal önbellekler; daha hızlı erişilen belleklere denir. örneğin "cpu cache". cpu cache cpu üzerindedir ve ram'den dahi daha hızlı erişilebilir. fakat maliywtli olduğundan çok çok ufak boyuttadır. cpu cache'in, özel amaçlı donanımlar hariç, yazılımcılar tarafından kullanması önerilmez. fakat klasik işletim sistemi bu kısmın editlenmesi için API sunmuş olabilir.

# Guava Cache example

Guava isimli kütüphanenin cache modülü için örnek kod:

```java
LoadingCache<String, Employee> employeeCache = 

 CacheBuilder.newBuilder()

    .maximumSize(100) 

    .expireAfterAccess(30, TimeUnit.MINUTES)

    .build(new CacheLoader<String, Employee>(){

    

       @Override

       public Employee load(String empId) throws Exception {

         

          return getFromDatabase(empId);

       } 

    });

 System.out.println(employeeCache.get("100"));

 //second invocation, data will be returned from cache

 System.out.println(employeeCache.get("100"));
```

görüldüğü gibi getFromDatabase yerine tekrar çağrıldığında ram'deki bellekten getiriyor sonucu. yazılımsal cache'lere en iyi örneklerden biri budur.

Bazı uygulamalar user-home dizininde bir klasörde cache isimli dizinler yaratmaktadır. uygulamalar kapandığında yada belli aralıklarla ram'deki bu cache datalarını dosyalara yazarlar. böylece uygulama restart edildiğinde, cache direk olarak RAM'e yüklenecektir. tekrardan cache'in dolması beklenmeyecektir.

# cache algorithms (yada cache replacement algorithms yada cache replacement policies)

bu tarz algoritmalar bir sıralı listeyi yönetmek için kulanılırlar. bu algoritmalar, hangi parçaların tutulacağı ve yeni parçalara yer açmak için hangi parçaların atılacağına karar vermek zorundadır.

 örnekler:

- First In First Out (yada FIFO)

- Last In First Out (yada LIFO)

- Least Recently Used (LRU): en eski kullanılan veri, yeni eleman geldiğinde ilk çıkarılır.

- Most Recently Used (MRU): en yeni kullanılan veri, yeni eleman geldiğinde ilk çıkarılır.

- Least Frequently Used (LFU): en az sıklıkta kullanılan veri, yeni eleman geldiğinde ilk çıkarılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# User Account Control (yada UAC)

(başka bir başlıkta da bu konu hakkında yazı mevcut)

# Squirrel

Atom text editorun kullandığı bir altyapıdır. bu yaızlım sayesinde uygulama program files'e değilde, user-home dizinine kurulmaktadır. bu şekilde, uygulama güncellendiğinde son kullanıcıdan yetki istememektedir. çünkü proram files'teki yazlım güncellemesi için son kullanıcıdna yetki istenmektedir. Squirrel aynı zamanda update işlemlerini kolaylaştıran ve API sunan bir altyapıdır.

# Chrome auto update

GOogle chrome Squirrel veya benzeri bir yapı kullanmamaktadır. Chrome kurulduğu işletim sistemine update işlemi için windows servisi eklemektedir. bu servis admin yetkisindedir. bu sebeple son kullanıcya soru sormadan program files'teki chrome'u güncelleyebilmektedir.

# ms-windows (arkaplan) servisleri kullanıcı kısıtlamaları ile yürütülüyor

windows servisleri kullanıcı bazlı çalışmaktadırlar. bu sebeple yetkileri kısıtlanabilir. bu kullanıcıların home klasörleri olmayabilir. çünkü sadece yetki kısıtlama amaçlı oluşturulmuşlardır. az yetkiliden çok yetkiliye örnek kullanıcılar; LocalService, NetworkService, LocalSystem.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# mmap
Memory map'in kısatlmasıdır.

posix system call'udur. bir dosyayı memory'de adresler. dosyanın her byte'ına RAM'den erişilebilir olur. fakat birçok nmap implementasyonu tüm dosyayı ram'e kopyalamaz. lazy loading yapar.

# genel *nix mimarisindeki memory terimleri

- resident memory

  resident türkçe kelime anlamı: oturan, sakin, yerleşmiş

  sadece RAM'deki memory.

- shared memory

  diğer process'lerle ortak kullanılan bellek boyutudur. Inter-process communication protokolleri aracılığı ile (veya benzeri yollar ile) kullanılan ortak memory'dir.

- Virtual Memory (önce "Sanal bellek (yada swap space yada takas alanı)" başlığı okunmalı)

  virtual memory; swap memory'den farklıdır.

  virtual memory = swap + resident memory + mmap

# "Gnome system monitor" ile gorulen proceess degerleri

- memory

  memory = resident - shared (çıkarma işlemi)

- opened files

  "__lsof (list of open files__ kısaltmasından gelir)" komutu çıktısının aynısıdır. 

  bu listede bir processe ait açık olan tüm dosyalar tutulur. dosyalar soket, pipe (process output), binary files gibi dosyalar olabilirler (unix-like'ta herşey dosya olması yapısı)

- memory map

  bir process'in virtual memory'si için her detayın (kaynağın) tutulduğu haritadır. mmap system call'u ile alakası yoktur. bellekteki tüm değerler (swap + resident memory + mmap)  yanyana tutulmaz. parça parça tutulur. bu sebeple bu tüm parçaların başlangıç ve bitiş adreslerine memory map haritasından bakabiliriz.

# htop komutu ile gösterilen bazı sutunlar

- PPID

Parent process id

- PGRP

Process group id. her process'in group id'si parent process'in group id'sidir. fakat eğer process'in kendisi alt process'ler başlatmış ise, yeni bir group id alır. aldığı group id, process id ile aynı atanır.

- NI (nice)

işlemciye giriş için öncelik seviyesi. numara büyüdükçe önceliği yükselir.

- PR (priority)

PR = 20 + NI. Nice -20 +20 aralıdğındaki değerini gösteryor. bu değer user tarafından değiştirilebilen  öncelik seviyeleridir. oysa linux'ta çok daha fazla değer vardır. tüm değerleri priority gösterirken, kullanıcı bazlı seviyeleri NI gösteriyor.

# top/htop shows different result than gnome system monitor

cpu farkının sebebi: gnome system monitor 8 cpu'muz varsa 8'i %100'e tamamlıyor. yani tüm cpu'lar tümüyle harcanırsa ancak %100 gösteriyor. fakat top yada htop komutunda bu değer %800 oluyor. bu sebeple genelde 8 cpu'lu bir makinede gnome system monitor 8 kat daha düşük değer gösteriyor. tabi gnome system monitor aynı zamanda bu değeri yuvarlıyor.

memory: memory değerleri de yuvarlanıyor.

# Windows 10 task manager işlem detayları sekmesinde olan bilgiler:

- PID: Process ID

- Status: process state

- Session ID: Login user ID

- CPU Time: How much time the process use the cpu (not: process yaşam süresi değil)

- Image Path name: Path of executable which started

- Command Line: Hangi komut ile bu programın başlatıldığını yazar. Örneğin "myservice -param1 -param2" gibi. myservice buada full path'de olabilir.

- Platform: 32 bit or 64 bit executable is rurnning

- Operating System Context: Hangi uyumluluk modunda çalıştırıldığını belirtir

- Description: İşletim sistemi belirtmiş ise uygulama ismi, belirtmemiş ise process adı yazar.

- data execution prevention: cpu ve işletim sisteminin desteklemesi gereken bir özelliktir. bu özellik sayesinde belirtilen uygulama, işletim sisteminin belirttiği bellek alanları dışında bir yere erişmesi engellenir. bu alan, window'un bu programa bu güvenliği uygulayıp uygulamadığını yazar.

- UAC Virtualization: UAC windows'ta yeni gelen bir teknoloji. Bu tkenoloji ile uygulamaların yetkileri kısıtlanmaktadır. Eski uygulamalar ile uyumluluk sağlayabilmek amaçlı bu özellik o uygulama için enable hale gelebilir. Bir uygulama  için bu özellik aktif ise, uygulama yetkisi olmayan bir yere yazmaya kalkdığında, yetki hatası almıyor fakat bu dizine yazdığı dosyalar aktarılıyor: C:\Users\username\AppData\Local\VirtualStore

- Base priority: İşlemciye giriş için öncelik seviyesidir.

- Handles: Windows uygulamanın açtığı bazı kaynakları tutmak/yönetmek Handle kavramından yararlanır. Soket, dosya, pencereler, pencere objeleri gibi… Bunların yazılımdaki "sınıf"'ları gibi düşünülebilir. Bunların toplam sayısı bu alanda yazmaktadır.

- Cycle: CPU hayat döngüsünün (fecth-decode-execute instruction) yüzde kaçının bu işleme ait olduğunu yazıyor.

- Private working set: İşlemin kullandığı toplam RAM değeri

- Peak working set: Uygulamanın yaşam süresi boyunca aldığı maximum "working set" değeri.

- Working set: Private working set + Shared working set

- Working set delta: "Working set" değerindeki anlık değişimin toplam değeri. Örnek: 300'den 350 ‘ye çıkmış ise o saniyede 50 değerini alır.

- Shared working set: Diğer process'lerle paylaşılabilen toplam "working set" değeri.

- Commit size: Toplam Sanal bellek değeri

- Page faults: Uygulama bellekten bir bilgiye erişmek istediğinde, önce RAM'e bakar. orda yoksa "sayfa hatası" alır. bu demek oluyorki, sanal belleğe (yani; hdd/ss'ye yani; ikincil belleğe) bakması gerekli. uygulama başladığında itibaren kaç kere bu hatayı aldığını gösterir.

- PD Delta: "Page faults"'un o andaki değişim değeri.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sanal bellek (yada swap space yada takas alanı)

sanal bellek terimi 3 farklı durum için eş anlamlı kullanılıyor:
- top komutunun manual'ında belirtildiği gibi 3 farklı memory var. bir çeşidi virtual memory olarak adlandırılmış. fakat bu bildiimiz swap dosyası alanı değil. http://man7.org/linux/man-pages/man1/top.1.html
- swap space
- her process , OS tarafından sanal bir bellekteymiş gibi çalıştırılabilir (bunun en gerçek örneği comtainer'lar içindeki uygulamalardır). bu belleklere sanal bellek denilir.

bu başlıkta sanal bellek; swap space'e eş anlamı olarak kullanılmıştır.

sanal bellek; ram'in yetmediği durumlarda kullanılan diğer bellek alanına (burada hdd/ssd oluyor) verilen isimdir. burada; RAM __birincil bellek__, hdd/ssd ise __ikincil bellek__ olarak isimlendirilir.

Sanal belleğin hdd/ssd'deki karşılığı bir dosya da olabilir yada bir partition'da olabilir. windows'ta her zaman bu bir dosyadır: C:\pagefile.sys

"Takas" ibaresi şuradan gelir: OS'lar ikincil bellek alanından data okuyacağı zaman onu önce RAM'e (birincil belleğe) kopyalar. oradan okur. bu işlem __takas yapma (yada swapping yada paging in yada faulting in)__ olarak adlandırılır. çok swapping yapmakta zaman kaybettirir. bu oranı OS'un iyi tutturması gerekir.

Bir programda üretilen/kullanılan adresler gerçek __fiziksel adresler__ değildir. Programda üretilen adreslere __"logical address (yada mantıksal adres)"__ denir. logical adresler, işletim sistemi seviyesinde veya donanımsal katmanda gerçek fiziksel adreslere çevrilmektedir.

sanal bellek yapısı kullanan sistemlerde "logical adress" yerine __virtual adress__ ibaresi de kullanılabilmektedir. 

# segmentation (yada segmentasyon)

OS'lar çalışan programlara ihtiyaçları kadar bellek atar ve bu bellekleri ardarda sıralayarak koyar. daha sonra bir process kapandığında onun kullandığı bellek kısmı silinir, fakat memory optimize edilmez. dolayısı ile memory'de arada boşluklar kalır. OS eğer boşluktan daha düşük memory isteyen bir uygulama gelirse, boşalan yeri dolduruyor. fakat %90 boşluğun tümünü doldurmayacağı için her zaman boşluklar kalıyor.

örnek:
```

   ******************
   *    100 MB      *  (A process için ayrılmış bellek alanı)
   ******************
   *                *
   *                *  (B process için ayrılmış bellek alanı)
   *    300 MB      *
   *                *
   ******************
   *    100 MB      *  (C process için ayrılmış bellek alanı)
   ******************


   B processi kapansın.


   ******************
   *    100 MB      *  (A process için ayrılmış bellek alanı)
   ******************
   *                *
   *                *  boş alan
   *    300 MB      *
   *                *
   ******************
   *    100 MB      *  (C process için ayrılmış bellek alanı)
   ******************


   250 mb isteyen E Processi gelsin.


   ******************
   *    100 MB      *  (A process için ayrılmış bellek alanı)
   ******************
   *    250 MB      *  (E process için ayrılmış bellek alanı)
   *                *  
   ******************
   *    50 MB       *  boş alan
   ******************
   *    100 MB      *  (C process için ayrılmış bellek alanı)
   ******************
```

Yukarıda görüldüğü gibi boş alanlar kalabiliyor. bu kalan kalıntılara __harici hafıza kırıntılarıdır (yada external fragments yada harici parçalar)__ denir. buna çözüm olarak __sayfalama (yada Paging)__ yaklaşımı kullanılıyor.

# paging (yada sayfalama)

fiziksel bellek alanı eşit büyüklükteki parçalara bölünür. Bunların her birine __sayfa (yada page)__ denir. Bu sayfalara karşılık gelen fiziksel bellek alanlarına ise __sayfa çerçevesi (yada page frame)__ denir.

sayfa ve çerçeve terimlerinin farklı olmasının sebebi şudur: bazı durumlarda birden fazla page, fziksel bellekteki aynı çerçeveye bakıyor olabilir. örnek durumlar:
- shared memory
- örneğin bir yazılımı 2 kere başlatalım. OS ve uygulama bunun tersini belirtmemişse, uygulamanın kendsi bellekte bir kere yer kaplar.
- common readed files
- bazen 1 page hiç pencereye bakmıyor bile olabiliyor: hiç initialize edilmemiş bir array olsun yada tümü null/0 olarak allocate edilmiş bir array olsun. bu gibi durumlarda OS bu frame'leri bayrak atıyor ve gerçek fiziksel bellekte karşılıklarını tutmuyor.

sadece "frame" terimi "page frame" yerine kullanılmamalı. bu yanlış. karışıklığa sebep oluyor. çünkü "frame" tek başına benzer anlamlar taşıyabiliyor.

Paging çözümünu, segmentasyona göre daha avantajlı fakat %100 verimli değil. 1 frame birden fazla uygulama tarafından kullanılamaz. bu sebeple en az PAGE_SIZE kadar process'lere alan tahsis edilir.

PAGE_SIZE=100 olsun. 1 process 210 mb isterse ona 100*3=300 mb'lik yer tahsis edilir. burada 90 mb boşuna harcanır. bu kalan kalıntılara __iç hafıza kırıntısı (yada internal fragments)__ denir.

PAGE_SIZE küçüldükçe memory'deki kırıntılar azalır fakat her page mapping genişleyeceği için performansta farklı dezavanjlar yaşanacaktır. OS bu oranı iyi tutturmalıdır.

# haritalama (yada mapping)

__page table__ OS'un kendi içinde tuttuğu bir tablodur. bu tablo ile __mapping__ yapılabilmesini sağlar. logical adres'leri fiziksel adreslere çevirir. örnek bir tablo;

| Sanal bellek mi gerçek bellek mi? | Page/segment'in başladığı gerçek fiziksel taban adresi |
|-----------------------------------|--------------------------------------------------------|
| gercek                            | 1000                                                   |
| gercek                            | 2000                                                   |
| sanal                             | 4000                                                   |

# sayfa hatası (yada page fault)

bu konu "Windows 10 task manager işlem detayları sekmesinde olan bilgiler" altında nalatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CAPTCHA (yada Completely Automated Public Turing test to tell Computers and Humans Apart)

puzzle yapma, matematik işlemi yapma, ekrandaki metni girme gibi birçok çeşidi vardır.

# reCAPTCHA

otomatik tanınması zor yazıların insan emeğiyle bilgisayar ortamında taranıp sayısallaştırılmasını sağlayan bir uygulamadır. Google tarafından kullanılarak; arşivlerin dijital ortama aktarılması için kullanılan reCAPTCHA, Google Books ve New York Times arşivinin dijital ortama aktarılmasında etkili olmuştur.

reCAPTCHA eskiden sadece kitaplardaki metinleri okuyabilmek için kullanılıyordu. fakat google projeyi aynı isimde farklı bir boyuta taşıdı. artık resimlerdeki objeler tanımlanarak son kullanıcı onaylanıyor. şimdilik sadece "street view" fotoğraflarındaki objeler son kullanıcıya soruluyor. yeni reCAPTCHA sisteminde; google, cookie'lere bakarak otomatik insan olup olmadığına karar veriyor. eğer cookie yoksa, o zaman resimleri kullanıcıya soruyor.

reCAPTCHA'nın 2 güzel yanı var: cookielere bakması ve insan tarafından fotoğrafların kolay algılanması. standart CAPTCHA'larda spam yazılımlar'daki ocr öylesine geliştiki, ocr'ler başarısız olsun diye çok zor metinler son kullanıcıya soruluyor. sn kulanıcı dahi bunları girmekte sıkıntı yaşıyor. bu da son kullanıcıyı sitedenuzaklaştırıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# NTFS long file names problem

NTFS 255 Uniceode karakter kabul ediyor. fakat eski dosya sistemleri etmiyor. bunun önüne geçmek için güncel işletim sistemleri her dosya için 2 farklı dosya ismi tutuyor. örneğin;

Microsoft.txt + MICROS~1.TXT

eski dosya sistemleri hem daha az karakterlerleri olmak zorunda, hemde büyük harf olmak zorunda gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# gradle

groovy syntax'ı ile build script yazmaya yarıyor (groovy syntax'ı başka başıkta anlatılmıştır). sadece jvm tabanlı projeler için değil, programlama dilinden bağımsızdır. şu anda çoğu eklentisi java tabanlı prjeler için yazdılığı için öyle bir algı oluşuştur.

# kotlin
gradle groovy'nin yanında kotlin syntax'ını da destekliyor. dosyanın kotlinle yazıldığını belirtmek için her gradle dosyasının sonuna kts eklemek gerekiyor. örnek:

> build.gradle.kts

# files

  - # build.gradle

  tüm işleyişin belirlendiği dosya

  - # gradle.properties

  properties formatında dosyasıdır. key-value şeklinde build.gradle tarafına parametre geçmek için kullanılır.

  - # settings.gradle

  tüm sub-modüller için geçerli olan key-value şeklindeki property'leri ve işleyişin de belirlenebebileceği dosyadır. sub-modüller içeren projede sadece 1 tane (root projede) olması gerekir alt projelerde olmamalıdır.

# lifecycle

  - initialization phase

    butada gradle sadece submodüllerin varolup olmadıklarını kontrol ediyor.

  - configuration phase
  
    __Gradle builds a Directed Acyclic Graph (DAG)__ algoritması kullanarak tasklarda döngü olup olmadığını temsil ediyor.

  - execution phase

    tüm build tasklarının çalıştırıldığı fazdır. ilk 2 faz IDE eklentileri tarafından default çağrılırken, bu fazın çağrılması için bir komut gerçekmektedir.

# tasks

gradle'da fazlar aslında basit yapıdadır. gradle tasklar üzerine kuruludur. task başka taskları çağırır. hangi taskların olacağına plugin'ler karar verir. java-plugin'i, bazı istisnalar dışında maven ile aynı isimlere sahip tasklar tanımlamıştır. bu şekilde maven'a alışmış geliştiriciler, gradle'daki taskları da çağırabilir.

build.gradle içinde;

```groovy
task hello {
   doLast {
      println 'tutorialspoint'
   }
}
```

taskımızı şu şekilde run ediyoruz:

> gradle –q hello

yada 

> gradlew hello

gradlewrapper otomatik olarak task çağırmaktadır.

"gradlew --help" ile "gradlew help" ayrı komutlardır.

eclipse'te gradle eklentisi varsa "gradle tasks" view'ı tüm gradle tasklarını listelemektedir. gradle taskları önce proje bazlı gruplandırılmakta daha alt seviyede ise 'task grup'ları bazında listelenmektedir. tasklarımızı gruplandırmak istersek; örnek hello taskımızı bir grup altına alalım:

build.gradle dosyamızın içine bunu eklememiz yeterli:

```groovy
hello.group = MY_GROUP_NAME
```

Gradle'da hiçbir eklenti yoksa "Help" ve grubu altında bir kaç task varsayılan olarak tanımlı gelir. bunlar gradle executable'ın içine gömülmüş tasklardır:

- build setup (task grup)

  - init (task)

    boş bir gradle proje oluşturur. bulunduğumuz dizinde, gradle.build dosyası vs oluşturur...

  - wrapper (task)

    gradle wrapper dosyalarını bulunduğumuz dizinde oluşturur.

- help (task group)

  - tasks (task)

    Displays the tasks runnable from root project

  - help

    displays help

  - (and others)
  
# task dependsOn

```groovy
task task4(dependsOn: [task1, task3]) << {

   println 'building task4'
}
```

artık task4 koştuğunda önce task1 sonra task3 koşacaktır. task1 task2 koşmuşsa (başka task tarafından) o zaman tekrar koşmaz.

# :my-sub-project1:myTask

bu şekilde altprojedeki bir taskı çağırmış oluyoruz. örnek komut:

> gradle :order-microservice:compile

yada asağıdaki aynı taskı çalıştırmaktadır:

> gradle order-microservice:compile

# defaultTasks

defaultTasks'lar eğer task ismi komut satırında hiç verilmemiş ise koşacak tasklardır. örnek:

```groovy
defaultTasks 'clean', 'run'

task clean {
    doLast {
        println 'Default Cleaning!'
    }
}

task run {
    doLast {
        println 'Default Running!'
    }
}

task other {
    doLast {
        println "I'm not a default task!"
    }
}
```

> grdale -q

komutu sadece clean ve run tasklarını çağıracaktır.

# java plugin

```groovy
apply plugin: 'java'

repositories {

   mavenCentral()
}

dependencies {

   compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'

   testCompile group: 'junit', name: 'junit', version: '4.+'

}
```

# java plugin dependency types

- Compile
- Runtime
- Test Compile
- Test Runtime

# sourceSet

java plugin'inin sunduğu bir konsepttir. sourceSet; birlikte derlenecek resource ve java kodlarının bulunduğu dizinledir. örneğin default tanımlı main sourceSet'i:

```groovy
sourceSets {

    main {

        java {

            srcDirs = ['src/main/java']

        }

        resources {

            srcDirs = ['src/main/resources']

        }

    }

}
```

gibi test source set'i de bulunur. isteğe bağlı ek source set'ler belirlenebilir. örnek: "integration-test" soruceSet gibi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# anotation vs xml (or any other format like json)

advantages of anotations:

- xml değiştirmek/okumak java koduna göre daha zordur (jpa da her entitity yi xml'de tanımladığınızı düşünün)

- bir sınıf hakkındaki tüm anotationlar (meta bilgiler) sınıfın içindedir.

- sınıf ismi değiştiğinde xml değiştirmek gerekmez

Advantages of xml files:

- anotation kulanımışsa; bazen hangi sınıfın görevini bilmiyorsak; onu tek tek dosyalarda arama yaparak anotasyonlarını bulmamız gerekir. bazen IDE'ler kolaylık sağlayabilir bu konuda. fakat xml'lerde tüm sistemi birkaç dosyada görebiliriz (bazen geçerli olan bir avantaj/durum).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# data binding

bir değeri bir objeye atama işleminin adı binding'tir. iki türlü binding vardır:

- one way binding

  bir degeri js tarafında modelde tutalım. bu degeri ekranda bir input'a yazdırdık. kullanıcı bu inputu değiştirdiğinde otomatik olarak model'deki değer değişmez. aradaki senkronizasyonu proramcı yapmak zorunda kalır. input değiştiğinde, tetiklenecek change metodunda programcı model'i değiştirmelidir.

- two way binding

  burada senkronizasyonu MVC kütüphanesinini kendisi yapar. örneğin; angular.js'te two way binding vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Electron

nodejs ve chromium altyapısı ile cross platform uygulama geliştirmeye yarıyor. "Atom text editor", "visual studio code" bu altyapıyı kullanıyor. İlk olarak Atom text editor için geliştirilmiş, daha sonra herkes kullanmaya başlamıştır. bu sebeple eski ismi: "Atom Shell"dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# eclipse IDE

Eclipse altyapısı ile birçok OSGI tabanlı uygulamalar oluşturulabilir. Bu uygulamalara "eclipse application (yada eclipse software)" deniliyor. Bu uygulamalardan bir tanesi Eclipse IDE ‘dir.

Eclipse IDE kendi içinde  versiyonlar ile dağıtılıyor: Java, JavaEE, Android gibi. Bunlara "paket veya product" adı veriliyor.

Her paket kendi içerisinde farklı plug-in'lerden oluşuyor.

Birçok plug-in toplanılarak, "feature" olarak gruplandırılıyor. Son kullanıcı feature silip ekleyebiliyor. Feature başka featurelere depend edebiliyor.

extension çok teknik seviyede kullanılan bir terim. extension; eclipse eklenti geliştiricileri tarafından sunulan her objenin/kaynağın (functionality, help content gibi...) ismidir.

- "eclipse java" paketinde yüklü gelen feature'ler:

  - "Maven Integration for Eclipse" (yada M2Eclipse yada m2e)

  - "Mylyn" - Jira gibi sistemlerden kullanıcıya taskı gösteren eklenti.

  - "Eclipse Web Tools Platform" paketinden sadece XML editor ve tool'ları

  - "Egit" - git integration. egit based on jgit (git java implementation)

  - "code recommenders"

  - gradle (feature ismi bilinmiyor)

- "eclipse javaee" içerisinde gelenler:

  - "eclipse java" versiyonu ile gelenler + aşağıdaki liste:

  - "Data Tools Platform" - Database management plugin.

  - "Java EE Developer Tools"

  - "Eclipse Web Tools Platform" (yada WTP)

  - Plug-in Development Environment: sadece eclipse paketi geliştirmek için gerekli altyapı.


Marketten indirebilenler:

WindowBuilder: GUI creator. pro version is exist.

# MyEclipse
eclipse IDE'den türetilmiş bir IDE.

# project facet
facet Eclipse'in sunduğu bir özelliktir. pojeye'nin property'lerini açtığımızda, birçok teknoloji ismi listelenir. hangilerini seçersek  proje için o teknolojilere özgü özellikler otomatik devreye girmektedir.

Örnek: ear facet'i devreye alındığında projenin claspath'ine otomatik olarak deployment descriptor eklenmektedir.

Bazı facet'ler biribirine depend ederken, bazıları ise aynı anda aktif edilememektedir.

IntelliJ'de "Add framework support" menüsünde facet ekleme vardır. aslında bakıldığında birçok IDE'de facet terimi olarak geçmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# nohup

posix için komut satırı yazılımı. aldığı parametreleri komut olarak arkaplanda çalıştırtır. böylece komut satırı kapatıldığında uygulama çalışmaya devam eder. nohup'a alternatfi yazılımlar da mevcuttur. ms-windows için "start" komutu aynı görevi görür.

# nohup vs disown vs &

  Aşağıdaki 3 örnekte 'commandX' yeni processe atanır ve shell yeni komut alacak şekilde kendini ayarlar.

  aşağıdaki her işlemde de commandX'e sterr, stdin gibi streamler bağlanır.

| command usage                    | standart input (stdin)                      | standard output (stdout) and standard error (stderr)                       | shell her prosesi hafızasında | SIGHUP sinyalini               |
|----------------------------------|---------------------------------------------|----------------------------------------------------------------------------|-------------------------------|--------------------------------|
| commandX param1 param2 &         | program buraya data yollarsa hata alacaktır | hala terminal'e bağlıdır, bu sbeple durduk yere ekranda output görebiliriz | process hafızada tutuluyor    | proces'e yollar                |
| commandX param1 param2 \| disown | program buraya data yollarsa hata alacaktır | program buraya data yollarsa hata alacaktır                                | process hafızadan silinir     | proces'e yollanmasını engeller |
| nohup commandX param1 param2     | kapatır ve process'e EOF sinyali gider      | redirects to nohup.out                                                     | process hafızada tutuluyor    | proces'e yollanmasını engeller |

shell'e bağlı job'ların listesini 'job' komutu ile listeyebiliriz.

  Bazen programlar disown yada nohup'la başlatmamıza rağmen shell kapandığında process'te kapanır. bunun sebepleri:

  - shell user session'ına bağlanmıştır. shell kapatılınca logout olur ve process kapanır.

  - shell kapandığında stin, stdout gibi streamler null olduğundan nullpointer bigi hatalar alan program hata fırlatarak beklenmedik durum gereği kapanır. bu sebeple bazıları stout'u > file.txt gibi bir dosyaya yönlendirmemizi önerir.

# posix sinyalleri

Özel bir IPC event'idir. posix standartları altında tanımlanmıştır. çalışan processlere veya bir thread'e özel birçok çeşit sinyal gönderilebilir. bu sinyaller 64 tanedir. C'de signal.h içerisinde tanımlıdırlar. Bazı sinyaller:

- SIGINT - 2 numaralı sinyal - komut satırında Ctrl+C tuşu aynı görevi işler. Bu yazılımcı tarafından handle edildiğinde gözardı edilebilir. yani program kapanmak zorunda değil.

- SIGKILL - 9 numaralı sinyal - Uygulamayı yazılımcı handle edemeden kapatır. Core dump dosyasını saklar.

- SIGSTOP - 18 numaralı sinyal - komut satırında Ctrl+Z tuşu aynı görevi işler - uygulamayı pause eder.

- SIGCONT - 19 numaralı sinyal - uygulamayı pause durumundan resume ile devam ettirir.

Bazı sinyaller piyasada kullanılmakta fakat POSIX standratlarında belirtilmemiştir.

Yazılımcı kod içerisinde bir listener aracılığı ile bu sinyalleri handle etmesi gerekir. C üzerinden örneklersek:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>

void sighandler(int);

int main () {

   //signal.h'taki bir fonksiyondur.
   //ilk parametresi: hangi sinyal için handle çalıştırılsın
   //ikinci paraetresi: handler fonksiyonumuz
   signal(SIGINT, sighandler);

   while(1) {
      printf("sleep for a second...\n");
      sleep(1); 
   }
   return(0);
}

void sighandler(int signum) {
   printf("Caught signal %d, coming out...\n", signum);
   exit(1);
}
```

Sinyaller processimize iletildiğinde, process thread'i beklemeye alınır ve handler'ımız execute edilir. handler bittiğinde main process devam eder. 

bir handler çalışırken, başka bir sinyal daha gelirse, önce ilk handler tamamlanır, sonra diğer handler için tekrar aynı handler devreye girer. 2inci handler bittiğinde main fonksiyonu çalışmaya devam eder. aşağıdaki kod ile bunun testini otuput kısmını inceleyerek görebiliriz. CTRL+C ile çalışan uygulamaya sinyal gönderildiğinde, uygulamanın çıktısı değişmektedir.

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>

void sighandler(int);

int main () {
   signal(SIGINT, sighandler);
   int mainLoopId = 0;
   while(1) {
      mainLoopId++;
      printf("main sleep -- mainLoopId=%d\n", mainLoopId);
      sleep(1); 
   }
   return(0);
}

int signalHandlerId = 0;

void sighandler(int signum) {
   signalHandlerId++;
   int loopId = 0;
   while(loopId<5) {
      printf("handler sleep -- signalHandlerId=%d loop=%d \n", signalHandlerId, loopId);
      sleep(1);
      loopId++;
   }
}
```

## Kimler ve hangi durumlarda sinyal alınıp/yollanabilir

- İşletim sitemi arkaplanda bu sinyalleri yazılımlara yollayabilir. örnek:
  - son kullanıcı işletim sistemini kapatmak için event açsın. bu durumda işletim sistemi uygulamaların tümüne kapanmaları için özel bir sinyal gönderir.

- Yeterli yetkisi olan son kullanıcı bir process'e sinyaller gönderebilir. örnek:
  - kill, signal komut satırı uygulamaları ile process-id'sini parametre olarak geçtiğimiz process'e, istediğimiz sinyali gönderebiliriz.

- yazılımcı kod içerisinden sinyal yollayabilir. örnek:
  - C'de; signal.h içindeki __int raise(int sig)__ fonksiyonu ile kendi işlemimize sinyal gönderebiliriz.
  - C'de; __int kill( pid_t pid, int sig )__ ile diğer process'lere sinyal gönderebiliriz.

# Core dump
Yazılım işletim sistemi tarafından zorla kapatılırsa memory'deki bilgisi ve diğer ek bilgiler bir dosyaya saklanır. bu şekilde daha sonra bu dosya analiz edilebilir.


# exit status (yada exit code)

program kapandığında bir sinyal döner. bu sinyale verilen addır. bir sayıdır.

- 0 başarılı bittiği anlamına gelir.

- pozitif sayılar daha çok yazılımcı tarafından yakalanmış/yönetilmiş hatalardır.

- negatif sayılar yazılımcının yakalamadığı/yakalayamadığı durumlarda döndürülür.

java'da; System.exit(int status) metodu ile statü dönülerek işlem sonlandırılır.

Bazı exit statüsler standarttır. örneğin bazı status'ler POSIX standartlarında belirtilmiştir. örnek:

code - açıklama

1 - programatic error example: 1/0 divide zero

2 - binary file format exmaple: missing keyword. for example calling a funciton which is not on classpath 

126 - Command invoked cannot execute. example: file is not executable

127 - command not found to execute

128 - exit code integer değilse o zaman bu hatayı verir.

130 - Script terminated by Control-C

?(sağ tarafı oku) - exit code integer fakat 0-255 arası değilse; işletim sistemi yazılımcının girdiği değeri yazilimci_exit_code%255 yapar ve bunu döndürür.

Yukarıdaki pozitif değerler yazılımcı tarafından fırlatılabilir. fakat bu hiç önerilmez. best practice değildir.

c ve c++ dillerinde standart kütüphanelerinin (sysexits.h) içinde asagidaki degerler vardır. fakat bunlar işletim sisteminin yada başka programlama dillerinin ve hatta diğer c kütüphanelerinin resmi olarak tanıdığı standratlar değildirler.

```js
#define EX_OK  0 /* successful termination */

#define EX__BASE 64 /* base value for error messages */

#define EX_USAGE 64 /* command line usage error */

#define EX_DATAERR 65 /* data format error */

#define EX_NOINPUT 66 /* cannot open input */

#define EX_NOUSER 67 /* addressee unknown */

#define EX_NOHOST 68 /* host name unknown */

#define EX_UNAVAILABLE 69 /* service unavailable */

#define EX_SOFTWARE 70 /* internal software error */

#define EX_OSERR 71 /* system error (e.g., can't fork) */

#define EX_OSFILE 72 /* critical OS file missing */

#define EX_CANTCREAT 73 /* can't create (user) output file */

#define EX_IOERR 74 /* input/output error */

#define EX_TEMPFAIL 75 /* temp failure; user is invited to retry */

#define EX_PROTOCOL 76 /* remote error in protocol */

#define EX_NOPERM 77 /* permission denied */

#define EX_CONFIG 78 /* configuration error */

#define EX__MAX 78 /* maximum listed value */
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# command line argumant passing types:

- POSIX like
  > myapp -myParam1 myValue1

- GNU like
  > myapp --my-param1 --my-param2=myValue2

- Java like

  Bu genelde D ve X olarak parameteleri gruplandırmak için yapılmaktadır.
  > myapp -Dmy.param1=myvalue1 -XmyParam1myValue1

- (no name for this)

  java-like gibi property1 ve property2 altında gruplayabilmek için kullanılır.
  > myapp --property1 myparam1=myvalue1 myparam2=myvalue2 --property2 myparam3=myvalue3
  

- (no name for this)

  tar -x -z -f myfile.tar.gz ile aynı olabilir. bu uygulamadan uygulama değişir.
  > tar -xzf myfile.tar.gz

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Test Double

stub, fake, dummy, mock gibi test için kullanılan datalara verilen genel bir terimdir.

# stub metod

bir metod daha yazılmamış ise (implemente edilmemiş ise) fakat başka yerlerden çağrılması gerekiyor ise metodun imzası hazırlanır fakat içinden null yada geçici bir değer dönülür. bu şekilde bu metodu kullanan diğer kodların geliştirmesi durmamış olur. böyle durumlarda bu metoda stub adı verilir.

# stub class

içinde stub metodları bulunan sınıftır.

# dummy data

genelde saece parametre yollamak(doldurmak) için kullanılan datalardır.

# mock data

bir yere yollandığında cevap olarak bir beklentimizin olduğu datalardır.

# fake data

asıl testimiz dışındaki kaynaklar için kullandığımız test datalarıdır. örneğin uygulamanın db'si ayağa kalkmazsa uygulama rest isteği hiç yapamaz. rest isteğini tets etmke istiyoruz. bu durumda database'yi fake yapabiliriz. database yerine memory'de tututal bir liste yapabiliriz. işte bu liste fake data oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ribbon

microsoft office 2007 ile birlikte getirdiği toolbar türevidir (componenet'tir). daha önce farklı yerlerde tabbed-toolbar mantığı kullanılmıştır. bu sebeple lisans problemleri yaşanmaktadır. Mirosofot aynı zamanda bu component'e "Fluent UI" adını da vermiştir.

# Metro UI

microsoft'un geliştirdiği arayüz standardıdır. Marka sorunları sebebi ile yeni isimleri mevcuttur: "Microsoft tasarım dili" yada "Modern UI". Windows 8 ile gelen yeni başlat menüsü tuşu ile açılan sayfada Metro UI kullanılmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Büyüklük/ölçü birimleri

# px (yada dot yada pixel yada Gözek yada picture element)

Görüntüdeki en ufak birimdir. Cihazdan cihaza boyutu değişir.

# sub-pixel

bu fiziksel bir kavramdır. tek bir pixeli oluşturmak için fiziksle olarak ihtiyaç duyulan birimlerdir. fakat yine en ufak görüntü birimi pixel kabul edilir. çünkü tek bir görüntü noktası (tek renk) olan en ufak birim pixel'dir.

# Inch (yada inç yada in)

çift tırnak işareti ile simgelenmektedir. 2,54 cm'e denk gelmektedir.

# PPI (yada pixer per inch)

dpi ile benzer bir kavramdır.

# dpi (yada dot per inch)

ppi'a benzerdir. ppi pixel bazında olduğu için her grafik çıktısı bu terimi kullanamaz. örneğin; ppi'ı sadece monitörler/ekranlar kullanır. printer çıktısında ppi'dan bahsedilemez. bu sebeple dpi terimine ihtiyaç vardır.

örneğin bir görüntü oluşturudğumuzda (photoshop ile) birimler dpi olabilir. dpi ile oluşturulan görnütüs o anda uygun olan pixel boyutuna çevrilecektir. fakat  cihazda  görünebilecektir. print etitğimizde  görünebilecektir. örnek belki photoshop, 1 dot = 1 pixel yapar, yada 1 dot 4 pixel yaparak ekranda gösteriyordur. işte burada o cihazın/yazılımın "dp" değeri önemlidir.

# dp (yada dip yada density (yoğunluk) independent pixels)

Standartlara göre değişen bir birimdir. örneğin; android'lerde 160dpi bir ekranda; 1dp = 1px'tir. 160 sayısı burada anroid standartıdır.  320dpi bir ekranda ise; 1dp 2 piksele eşit olacaktır.

# International System of Units (yada SI)

üslerin pozitif olduğu değeler:

```
deca  hecto  kilo  mega  giga  tera   peta   exa    zetta  yotta

da    h      k     M     G     T      P      E      Z      Y

10^1  10^2   10^3  10^6  10^9  10^12  10^15  10^18  10^21  10^24
```

üslerin eksi olduğu değerler:

```
deci  centi  milli  micro  nano  pico   femto  atto   zepto   yocto

d     c      m      μ      n     p      f      a      z       y

10^1  10^2   10^3   10^6   10^9  10^12  10^15  10^18  10^21   10^24
```

Birçok teknik farklı kavramlar için ek birimler türetilmiştir. örnek:

Frekans birimi; hertz (yada abb:Hz). s^−1 (saniyede salınım)

# uzunluk tanımı

Uzunluk hesaplamalarında metre referans olarak alınmaktadır.

1 metre, ışığın boşlukta 1/299.792.458 saniyede aldığı yol olarak tanımlanmıştır. 

# ağırlık tanımı

ağırlık hesaplamalarında gram referans olarak alınmaktadır.

Uluslararası kuruluşlarca Paris'teki Milletlerarası Ağırlıklar ve Ölçüler Bürosu'nda bulunan iridyum platinden yapılmış silindir şeklindeki cisim 1kg olarak kabul edilir.

Paristeki referansa en yakın tanım: gram, +4°C de 1 cm3 saf suyun kütlesidir.

Ağırlığı referans almak için piyasada şu yol izlenmektedir: dünyanın en yuvarlak nesnesi olan ve 2,15 x 10^25 adet silikon 28 atomuna sahip mükemmel küre şeklindeki bir cisim çok yüksek maliyetlere üretildi ve kopyalandı. kopyası dünyanın her tarafına dağıtıldı. herkes bu referansa göre ağırlığı tanımlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Big Endian vs Little Endian

Bir veri başka bir yere taşınırken verinin taşınma sırasını temsil eder. Big endian'da değeri yüksek kısım (byte) sağ tarafta olur. little'da ise tersi geçerlidir.
Bu sıralama bit bazında değildir. Bit bazında hangi sırada tutulduğu donanımın kendisi belirler. byte bazındaki sıra endian ile ilgili bir durumdur.

Big Endian piyasada "Motorola convention" yada "motorola style" byte ordering olarakta geçmektedir. çünkü mototora işlemcilerde hep big endian kullanılmaktadır. 

# bit
__binary digit__'ten türemiştir.

# charset (yada karakter seti) vs character encoding (yada karakter kodlaması)

charset, hangi karakterleri kullanılıp kullanılmayacağını belirtir. Örneğin; ABC kümesi bir charset olabilir. Fakat bunların binary bazında nasıl kaydedeği bilgisini charset içermez. binary bazında nasıl kaydedileceğini (tutulacağını) encoding kuralları belirler. A harfi bir encoding'de 8 bit olarak tutulurken, farklı bir encoding'de yine A harfi 16 bit olarak tutulmak zorunda kalabilir. Her encoding charset uzayının boyutuna göre ayarlandığından (formatlandığından/boyutlandırıldığından), belli bir charset'i destekliyor olur. Dolayısı ile bi yerde "charset=UTF-8" gibi bir ibare görülebilir. Bu ibare; UTF-8'in desteklediği charset'ler anlamına gelmektedir.

# ASCII (yada American Standard Code for Information Interchange)

Bir encoding standardıdır. Sadece ingilizce ve en temel karakterleri içerir. 7 bittir. 1 bir ekstra olarak parity biti olarak kullanılır. dolayısı ile 8 bit yer kaplar.

# Genişletilmiş ASCII (yada Extended ASCII)

7+1 bittir. ASCII yetersizliğinden ortaya çıkmıştır. ASCII ile ilk 7 biti aynı karakter setine referans ettiği için ASCII ile uyumludur.

# Extended ASCII (yada EASCII yada high ASCII)
8 bittir. parity biti içermez. ilk 7 biti ASCII ile aynıdır. diğer bitler ise farklı karakterler içerir. diğer bitler için bir standart yoktur. her işletim sistemi ve kurulan dil paketine göre değişiklik gösterir.

# ANSI (yada Windows-\*)

7+1 bittir. ASCII yetersizliğinden ortaya çıkmıştır. ASCII ile ilk 7 biti aynı karakter setine referans ettiği için ASCII ile uyumludur. Diğer 128 bit ISO-8859 standardının türevlerin değişiklik göstermektedir. Örneğin; Windows-1254; ASCII (ilk 127) + TR (diğer 127) içermektedir.

# ISO-8859

7+1 bittir. ASCII yetersizliğinden ortaya çıkmıştır. ASCII ile ilk 7 biti aynı karakter setine referans ettiği için ASCII ile uyumludur. Diğer 128 bit ISO-8859 standardının türevlerin değişiklik göstermektedir. Örneğin; ISO-8859-9; ASCII (ilk 127) + TR (diğer 127) içermektedir.

# Unicode Transformation Format (yada UTF)

Bir encoding standardıdır. kendi içinde UTF-8, UTF-16, UTF-32 gibi ayrımları vardır. Format sürekli yenilenmekte ve yeni versiyon çıkmaktadırlar.

utf karakter boyutu çok büyük olduğundan boş atanmamış karakter noktaları/kodları da vardır.

- UTF-8: karakter'e göre 1-4 byte arsında her karakter uzayabilir.  ilk 7 biti aynı karakter setine referans ettiği için ASCII ile uyumludur.

- UTF-16: karakter'e göre 2-4 byte arsında her karakter uzayabilir. ASCII ile uyumsuzdur. 

- UTF-32: her karakter 32 bittir. her karakter ayrı ayrı ayrıştırılabilir. ASCII ile uyumsuzdur. 

- UTF-1: artık kullanılmıyor ve standart değil

- UTF-7: birileri tarafından makalelerle yayınlanmıştır. fakat resmi bir UTF standardı değildir.

Yukarıdaki utf listesinden anlaşıldığı gibi; utf-8 dahi tüm karakterleri gösterebilme yeteneğine sahiptir (hatta boş alanı dahi vardır).

UTF ile ilgili birçok terime buradan ulaşılabilir: http://unicode.org/glossary/

# code unit

bir karakteri utf-8, utf-16 gibi encodinglerle byte haline çevirdiğimizde ortaya çıkan byte bilgisine verilen isimdir. örneğin; A karakterinin code point'i U+0031 olsun, code unit'i UTF-8 için 0031, utf-16 için 00010031 olabilir (değerler tamamen uydurmadır). 

# Unicode

UTF-8, 16 ve 32 bir encoding formatı iken, bu encodinglerin baz alındığı(desteklendiği) karakter-seti Unicode'dur. utf-8 dahi tüm unicode'u desteklemektedir.

Unicode kelimesi microsoft windows'un bazı yazılımlarında yanlış kullanılmaktadır. bu sebeple karıştırılmamasına dikkat edilmeli. 

# '€' karakterini utf-8 ile kodlayalım.

- € karakteri U+20AC kod noktasına tekabül eder

- 0010 0000 1010 1100 (binary) = 20AC (hexadecimal)

- U+20AC 2 byte'tır. fakat aralık Unicode standartlarında U+0800 ile U+FFFF arasında olduğu için 3 byte ile tutulmalıdır.

- 3 bayt olduğu için 3 tane 1 yanyana kulanılacaktır. 1'lerin bittiğini belirlemek için 1 adet sıfır sağ tarafa eklenecek. sonuç; 1110.

- 1110 4 karakter odluğu için 1 bayta tamamlanacak ve 0010 0000 1010 1100'nin en önemli 4 biti daha kullanılacatır. sonuc: 1110 0010

- daha sonraki kısmıda belirli algoritmalarla tamamlanıyor.

akışa bakıldığında; tüm metin için ortak bir yerde metadada saklanmadığı görülmektedir. yani her karakerin uzunluğu UTF-8 ve UTF-16 için karakterin başında bellidir. bu sebeple bir sonraki yada önceki karakterler ilgili karakterimizi boyutunu değiştirmemektedir.

# unicode karakter özellikleri

unicode standratlarında her karakterin bir meta bilgisi vardır. bu bilgi karakter kodundan diil, dökümantasyondan çıkartılmalıdır.

- genel kategori (yada general category)

  30'a yakın kategori vardır (yıl 2019)

  kategoriler virgülle ayrılmış iki kısımdan oluşmaktadır. ilk kısım major class, ikincisi ise subclass'ı temsil eder.
  
  örnek: "Number, decimal digits", "Number, letter", "Number, Others".

  bazı kategoriler:

  - Other, control
    
    kod: Cc
    
    açıklama: "start of text", "end of line" gibi karakterleri buradadır.

  - Letter, Lowercase
    
    kod: Ll

  - Letter, Uppercase

    kod: Lu


  Fakat her kategori %100 şekilde uymamaktadır. geriye uyumluluk için bazı istisnalara tölerans gösterilmiştir. (bazıları yeni sürümlerde düzeltiliyor)

- code point

  U+0001 şeklindeki gösterimin ismidir. burada U+ hiçbir anlamı yoktur. unicode'un verdiği özel bir prefix'tir. Aslında asıl değer 0001'dir ve bu değer unicode karakter listesindeki 2'inci elemana referans eder. 0001, hexadecimal formattadır.

- name

  büyük harflerle alfanumeric, boşluk ve tire işaretli içerebilir. tekildir fakat her karakter için set edilmiş bir değer olmayabilir.

- name alias

  Unicode version 2 ile birlikte 

- script

  (başka başlıkta ne olduğu anlatılıyor)

  200'e yakın script vardır.

- Bidirectional class

  "Unicode Bidirectional (bidi) Algorithm" için gerekli bilgileri verir. metnin sağdan sola mı soldan sağa mı, yoksa etkisi olup olmadığı gibi... Bu bilgiden 25'e yakın var (yıl 2019). bu sebeple algoritma karışık. aynı satırda birden fazla farklı dilden metin olursa ne yana yaslanacağı belli kurallara göre (algoritma ile) belirlenmektedir. Aynı satırda farklı yönlerde karaktermetin olması durumunda o text bloğuna Bi-directional text (yada BI-DI yada BIDI) denir.

- Lowercase/uppercase Character

  aynı karakterin büyük yada küçük versiyonununun code-point'i.

- Mirrored

  karakterin mirroru olup olmadığı bilgisi. bu bilgi sadece true/false şekilde tutuluyor. hangi karakter bu karakterin mirror'u olup olmadığı bilgisi tutulmamaktadır. örnek mirrered characterler: < > ( )

- Decomposition

  bir karakter birden fazla karakterin birleşimindenoluşuyorsa, hangi karakterlerin birleşimindenoluştuğu bilgisi. örnek: vurgu işaretli e' karakteri; e ve vurgu karakterinden oluşuyor. dolayısı ile şapka yada tonlama işaretlerinden herhangi birini almış her karakterin unicode'u varklıdır. örneğin yunancadaki omega işraeti; ω, tonlama aldığı zaman (ώ) farklı codepoint'e sahip farklı karakter oluyorlar.

# UTF mimarisinde karakterlerin sıralanması/gruplanması

- plane 

  plane kelime anlamı: tabaka

  tüm karakterler sıra bazlı gruplandırılmışlardır. kolay hespalama olsun diye yapılmıştır.

  örnek: 

  - plane 0

    aralık: U+0000 - U+FFFF

    adı: BMP (yada Basic Multilingual Plane yada 0 BPM yada plane 0)

  - plane 1

    adı: 1 SMP (yada Supplementary Multilingual Plane)

  - plane 2

    adı: 2 SIP (yada Supplementary Ideographic Plane)

  - plane 3-13

  - plane 14

    adı: 14 SSP (yada Supplement­ary Special-purpose Plane)

  - plane 15-16

  Yukarıdan görüldüğü gibi; U+FFFF - U+FFFFFF aralığı için plane'ler "supplementary" prefixini alıyor.

- block

  tüm karakterler plane'den daha ufak seviyede de gruplandırılmıştır. kolay hespalama olsun diye yapılmıştır. bazı block'lar:
  
  - U+0000 - U+007F

    ismi: Basic Latin

  - U+0080 - U+00FF

    ismi: Latin-1 Supplement 

  - U+0100 - U+017F

    ismi: Latin Extended-A 

  ...

  - U+0600 - U+06FF

    ismi: Arabic 

  ...

  - U+0750 - U+077F

    ismi: Arabic Supplement

  ...

  - U+08A0 - U+08FF

    ismi: Arabic Extended-A

  ...

# BOM (yada Byte Order Mark)

UTF'lerde (little/big endian) önemlidir. Bu sebeple standartlara göre opsiyonel olarak text'in en başına byte order bilgisi atanır. Bu bilgi ile text'in hangi order'da olduğu belirtilir. Text editorler menülerde encoding'leri bu şekilde gösterirler: UTF-16-LE, UTF-32-BE olarak kısaltılırlar. UTF-16-with-BOM 'da kullanılrı fakat LE mi BE mi belli değildir. bu yüzden bu kullanım tavsiye edilmez.

BOM'un uzunluğu belirsizdir. çünkü birçok kombinasyonu vardır. UTF-8, 16, 32, 7 (ve daha fazlası) ve bunların LE BE kombinasyonları vardır.

# EOF (yada End Of File)

EOF bir karakter değildir. Çoğu proramlama dilindeki dosya okuma yazma API'leri bunu karaktermiş gibi yansıtabilir. fakat karakter değildir. Böyle bir karaktere ihtiyaç yoktur, çünü tüm dosya sistemlerinde dosyaların boyutu bellidir.

# ETX (yada End Of Text) vs NUL

birer karakterdir. Eskiden teknolojilerde (telegraf gibi) kullanıldıklarından, yeni encoding standartlarında geriye uyumluluk olması açısından karakter olarak yer almaktadırlar.

# newline (yada line ending yada end of line yada EOL yada line break yada satır sonu)

birçok alternatifi vardır. Varsalyılan işletim sistemi karakter setlerine göre:

- POSIX: LF (line feed) --> \n

- Windows: CR+LF (yada CRLF) --> \n\r

- bazı sistemlerde sadece CR kullanılmakta, bazılarında ise LF+CR kullanılmaktadır.

açılımları:

CR: carriage return

LF: line feed

Atom text editor'de "invisible character" olarak nitelendirilen karakterler: boşluk, eof ve tab'dır. Atom, LF ve CRLF'yi  simge olarak göstermektedir.

Bunlar dışında karakter setlerinden birçok kontrol (özel) karakter bulunmaktadır. örnek:

- paragraf sonu

- backspace

- delete

- ctrl-z

Bu karakterler önyüzde bir şey göstermek için değil, belli işlemler yaptırmak için kullanılır. bu karakterler sadece dosyadan okununca işe yaraması için yapılmamıştır. örneğin ctrz-z karakterini C gibi bir dilinde printf ile ekrana bastığınızda CTRL-Z olan "geriye al" işlemini yapar. yada klavyede basılan bir tuşu uzak makinada çalıştırmak için bu tarz kontrol karakterleri işe yarayabilir. Bu tarz karakterler "control" karakteri olarak Unicode'da da kullanılmaktadır.

# unicode'daki bazı özel karakterler

unicode'da özel karakterler diye bir grup yoktur. Bu başlıkta boşluk, satır sonu gibi karakterler hakkında açıklamalar vardır.

```
LF:    Line Feed, U+000A

VT:    Vertical Tab, U+000B

FF:    Form Feed, U+000C (yeni bir sayfa oldugugunu printer'a belirtmek için kullanılır)

CR:    Carriage Return, U+000D

CR+LF: CR (U+000D) followed by LF (U+000A)

NEL:   Next Line, U+0085 (bazı ibm platformları bunu kullanıyor.)

LS:    Line Separator, U+2028

PS:    Paragraph Separator, U+2029
```

yukarıdaki bazı karakterlere simge atayabilmek için farklı karakterler ile ikon seti belirlenmiştir:

```
U+2424 (SYMBOL FOR NEWLINE, ␤)

U+23CE (RETURN SYMBOL, ⏎)

U+240D (SYMBOL FOR CARRIAGE RETURN, ␍)

U+240A (SYMBOL FOR LINE FEED, ␊)
```

unicode'da 20'ye yakın farklı space vardır. bazıları farklı platformlarda kullanıldığı için, bazılarının ise uzunlukları farklı.

Unicode'un amacı yeni bir karaketer standardı yaratmak değildir. varolan tüm karakterleri tutmaktır.

# URL encoding (yada percent encoding)

URL 'lerde boşluk ve bazı özel karakterler kabul edilemez. dolayısı ile bu karakterler için yeni bir encoding geliştirilmiştir. bazı karakterler % ve sonrasında 2 adet sayı olacak şekilde bir karakteri temsil etmektedirler.

# StringUtils

apache-commons-lang kütüphanesinin içinde olan bir sınıf. Commons-lang içinde CharUtils, RandomStringUtils(random string üretiyor) sınıfıları da mevcuttur.

# trim

türkçe kelime anlamı: düzeltmek

bu metod birçok kütüphane içinde mevcuttur. birden fazla boşluk yanyana ise onları tek bir boşluk haline getirir. eğer boşluk en sağda yada en sonda varsa onları kaldırır.

# karakter grupları

karakterler son kullanıcı açısından gruplanmıştır. fakat bu grupların bir resmiyeti yoktur. örneğin; mobil uygulamalarda sanal klavyede i tuşuna basılı tututlduğunda i'nin türevleri çıkar. örnek: 'ı' gibi. bu karakter grupları o sanal klavyenin kendi ayarıdır. böyle bir gruplama utf yada farklı karakter setlerinde tanımlanmamıştır. 

ingilizcedeki küçük 'i' ve türkçedeki küçük 'i' ortak karakterdir. toLowerCase() metodu işletim sisteminden default dili algılar ve ona göre uygun büyük harfe çevirir. toLowerCase() metodu parametre olarak java Locale'i alır.

# accent (yada aksan yada şive)

örnek: alman türkçesi. ana dili türkçe olmayan kişiler kullanır. bu terim "ağız" terimi ile aynı kapıya çıkıyor fakat farklı.

# lehçe (yada diyalekt yada Dialect)

örnek: azerbaycan türkçesi. resmi olarak lehçenin dilbilgisi farklıdır.

# ağız (yada subdialect)

örnek: karadeniz türkçesi.

# aksan işareti (yada aksan imi yada grave accent)

örnek: ằ v̀ gibi hem sessiz hem sesli harflerde birçok farklı şekilde bulunurlar.

StringUtils.stripAccents("abc"); metodu aldığı string'deki vurgu karakterlerini silip, yerine vurgusuz halini koymaktadır. örnek égğ --> egg

strip türkçe kileme anlamı: soymak, üstünü çıkarmak.

(diğer başlıklarda anlatıldığı üzere) Unicode metadalarında hangi karakterlerin üzerinde hangi aksan işareti olduğu bilgisi tutulmaktadır. yani; v̀ için, "v ve aksan işareti" olduğu Unicode veritabanında mevcuttur.

# java.util.Locale 

bu sınıf jre içinde gelir. içinde static olarak en sık kullanılan değerler hazır bulunur:

static public final Locale ENGLISH = createConstant("en", "");

static public final Locale UK = createConstant("en", "GB");

static public final Locale US = createConstant("en", "US");

Locale 3 parametre alır:

Locale(String language, String country, String variant). sağdaki iki parametre opsiyoneldir. eğer atılmazsa boş olarak kullanılır. yani locale.getVariant() boş döner.

Locale sınıfının aldığı üç parametre ağız/lehçe/aksan kombinasyonu değildir. dil, kullanıldığı ülke ve türevi olarak algılanır. örnek:

- "azerbaycan türkçesi", türkçe'nin türevi diildir. kendisi bir dildir. ve ülkesi azerbaycan'dır.

# alfabe (yada alphabet) 

bazı alfabeler:

- Latin alfabesi (ABCDEF...)

- Yunan (yada Greek) alfabesi

- Kiril (yada Cyrillic) Alfabesi (Rusya ve etrafında kullanılıyor)

- Ermenice (yada Armenian) alfabesi

- Gürcü (yada Georgian) alfabesi

- arap alfabesi

- İbrani alfabesi (yada Hebrew)

> japonya, çin ve kore'de kullanılan alfaberlerde birbirinden farklıdır.

> arapça temelli dillerde alfabe hep aynı. bazı karakterler bazı dillerde yok yada fazlası var fakat temelde aynı karakterler kullanılıyor. yani farklı ülkeye gittiğinizde kelimeleri okuyabiliyor fakat ne anlama geldiklerini bilemiyorsunuz.

> arapça temelli dillerde, bazı karakterler yanındaki karakterlere göre şeklini değiştirmektedir (yanındaki karakterlere bağlanmaktadırlar). bu sebeple font viewer'larda gördüğümüz font'lar ile cümle içinde gördüğümüz karakterler aynı olmayabilirler.

# Yazı sistemi (yada writing system yada script)

yazı sistemi; bir dilin yazı stilidir. örnek: script arapça, bu script'in baz alındığı birçok dil vardır: Iraqi, Lebanese, Egyptian... Arapça baz alınan dllerin %100 arapça yazı stili kurallarına uymayacağı zaten ortadadır.

bazı diller birden fazla yazım sistemini desteklemektedir. örnek: Korece (yada Korean), Japonca (yada Japanese). Yani bu dillerde aynı cümleler farklı yazı ile yazılabilmektedir.

Örnek: japonca'da aşağıdaki yazım sistemleri vardır:
- hiragana: japonca kökenli kelimeleri yazmak için kullanılır
- katakana: yabancı kökenli sözcüklerin yazmak için kullanılır

japonca'da aynı cümlede birden fazla yazım stili olabilir.

Romanji; japon yazı sistemindeki her karaktere ve heceye karşılık gelen latin karakterleri olur. japonca'da yazılacak cümle, aynı karakterlere denk gelen latin karakterleri ile yazılır. Romanji'de kendi içinde türevleri vardır.

# Traditional vs Simplified Chinese
Çince zor dil olduğundan her katakter daha basit bir görüntü haline getirilip, ortaya daha basit okunabilen karakterler yaratılmıştır. Bu yeni sisteme Simplified Chinese ismi verilmiştir. Simplified Chinese'de her karakter değişmemiştir.

Japonca da benzer durum yok, çünkü japonca Simplified Chinese'e göre dahi daha kolay anlaşılır bir görüntüsü var.

# numbers

  - ## digit

    türkçede matematik'teki "basamak", "hane" olarak çevrilir.

    13'lük sayıs siteminden rasteel bir sayı alalım: 111A

    en sonda olan A (10 sayısına denk gelmektedir) tek başına bir basamak hanedir.

  - ## Unicode terminolojisi

    Unicode karışıklığa sebep olmaması bazı termleri şu şekilde kullanmaktadır:

    - digit: only numerical digits

    - Arabic digits yada Arabic-Indic Digits: Arabic script'e ait rakamlar

    - Western digits yada Latin digits yada European Digits: 0-9 rakamları

    - Indic digits: hint temelli dillerde kullanılan rakamlardır.

  - ##  Doğu Arap rakamları (yada Eastern Arabic numerals yada Arabic–Indic numerals yada Arabic–Hindu numerals yada Arabic Eastern numerals yada Indo-Persian numerals)

    arapça tabanlı kullanılan bir sayı sistemidir. arap ülkeleri arasında da farklılık gösteriyor.

    arapça yazılım geliştirirken arapça sayı girişi olan input'larda arap ülkelerindeki kullanıcılar sadece "Doğu Arap rakamları" değil aynı anda klavye aracılığı ile "Arap rakamları"'na (0-9) geçiş yapabiliyor. bu durumda yazılımcıya ek iş yükü binmiş oluyor.

  - ## Arap rakamları (yada Hindu–Arabic numerals yada Indo-Arabic numerals yada Arabic numerals yada Western Arabic numerals yada Western numerals)

    0-9 kullanan rakam karakterleridir.

  - ## Roma rakamları (yada Roman numerals)

    Bazı kullanım alanları:

    - Kitapların başlangıçlarındaki sunuş ve önsöz gibi bölümlerde.
    - Aynı adlı padişah ve kralların ayrılmasında: II. Ahmet, XVI. Louis gibi.
    - Yüzyıl adlarında: XIX. yüzyıl gibi.
    - Tarihi olaylarda: II. Viyana Kuşatması, I. Dünya Savaşı gibi.

    Temel kurallar: 
    
    I = 1

    V = 5

    X = 10

    gibi daha birçok karakterden oluşur.

    sağına eklenenler toplanır, sola eklenenler çıkarılır. örnek:

    XIV = 14

    XXIV = 24

    karakterler üzerlerine düz çizgi de alabilirler. bu durumda o sayının 1000 katı temsil edilir.

  - ## çin ve arapça'da sayılar

    arapçada sayıların görünümü farklı fakat sistem olarak "Arap rakamları (0-9)" ile aynı.

    çince temelli dillerde de sayılar bölgeden bölgeye farklılıklar gösterir.
    
    çince'de ise yazım sistemi de farklı. örnek üzerinden gidersek; "Mandarin çincesi"nde her karakter bir sayıyı ifade etmiyor. örnek: 
    
    - __四十二 42 iken
    - 九十 90 'ı temsil ediyor. görüldüğü gibi sayı daha büyük olmasına rağmen daha az karakterle temsil ediliyor.

    Özellikle programatik olarak __sıralama__ istendiğinde çok zor durumlarla karşılaşılmaktadır.

# CJK

Chinese, Japanese ve Korean dilleri için ortak terimdir. Bazı dilbilimciler Vietnamese (yada vietnamca) dilinide bu gruba katarak, CJKV kısaltmasını kullanırlar.

CJK karmaşıklığını gidermek için (mappinglerin daha düzenli olması için) Unicode projesinde belli kişiler çalışmaktadır. bu çalışmalar "Han unification" olarak isimlendirilmektedir. 

# Çin

Çinde birçok farklı türemiş dil mevcuttur. Ülke resmi olarak __"Mandarin"__ isimli dili kullanmaktadır. "Mandarin" Çin'de en fazla kullanılan dildir. Cantonese ve Wu dilleri diğer en çok kullanılan diller arasındadır.

Sadece "Çince" denildiğinde "Mandarin" kastediliyordur. 

Mandarin, Cantonese arasında farklar: bazı karakterler, grammer yapısı, tonlamalar, kelime hazinesi. Mandarin metni okunduğunda, sadece Cantonese bilen biri bu cümleleri anlayamıyor.

Çinde yüzlerce dialect ve dil vardır.

__Classical Chinese (yada Literary Chinese)__ hala kullanılan ve İsa zamanına dayanan bir tarihi vardır.

# arapça
__Klasik arapça__ 7 ve 9 uncu yüzyıllar arası kullanılan arapçadır. Şu anda kullanılmıyor.

Çince gibi arapça'dan da türemiş birçok dil ve dialect vardır.

__Modern Standard Arabic (yada MSA yada Standard Arabic yada Literary Arabic)__ birçok ülke tarafından resmen tanınan bir arapça çeşididir. Günümüz sistemlerine uygun şekilde tasarlanmaktadır.

# alphanumeric (yada Alfanümerik)

Apache-commons-lang StringUtils sınıfına göre; sadece isAlpha():

- işaret ve boşluk karakteri içermemeli. yani sadece "latter" içermeli. letter bilgisi Unicode'un "general category" bilgisinden gelmektedir.

- sadece unicode karakteri içermeli

- sayı içermemeli

isAlphanumeric(); isAlpha'nın sayı kuralını ignore eder.

# fontlar

her font belli bir karakter setini desteklemektedir. Eğer ekranda gösterilmesi gereken karakter seti o sıra kullandığımız font'ta tanımlı diil ise; web tarayıcıda isek; web tarayıcının kendisi, native bir uygulamada isek; OS'un kendisi karar verip, görüntülenemeyen karakter için şunu yapacaktır:

- default bildiği font, yada farklı bir font ile o karakteri gösterecektir

- sadece o karakteri hiç göstermeyecektir (boşluk yada hiç)

- hata fırlatacaktır (bu hiç tercih edilmeyen yöntem. neredeyse hiçbir program böyle bir şey yapmıyor)

- fallback karakterini gösterecektir.

- böyle bir karakter gösterecektir: (bir diktörgen içinde sayılar ve harfler oluyor)

  ```
   _____
  | 1 8 |
  | 2 f |
   ¯¯¯¯¯
  ```

   Bunun anlamı bu karakter: 16'lık gösterimde 182f'dir.

# replacement character (yada missing glyph yada replacement glyph yada .notdef glyph)

- � (içinde soru işareti olan altıgen) karakteri simgesindedir.
- bu karakter utf kodlamasında hata alındığında, hata alınan karakter yerine bu karakterin konulması istenmektedir.
- code point'i U+FFFD'dir.
- Bu unicode'un bir standartıdır. bu sebeple her font'un fallback karakteri bu codepoint'e tekabül etmez. fakat her font dosyasında bir fallback karakteri olması beklenir. 
- Unicode'un unsopported character dökümanına bakıldığında: http://unicode.org/faq/unsup_char.html her fallback karakteri için "replacement character" kullanılmaması önerilmektedir. örneğin;
  - White_Space property içeren fakat tanımlanmamış karakterler yerine boşluk gösterilmeli
  - default-ignorable character'ler tanımlanmamışsa yerine hiçbir şey gösterilmemeli 

# tofu
kelime anlamı: soya peyniri
replacement character yazılımlar tarafından her zaman unicode'da belirtilen şekilde olmamaktadır. genelde boş kare yada içi noktalarla dolu kare işareti gösteirlmektedir. işte bu işarete yazılımcılar tofu demektedir. kelime anlamından gelmesi ona benzemesindendir.

# noto
no-tofu 'dan üretilen bir kelimedir.
google'ın geliştirdiği font ailesinin ismidir. unicode'da olan dillerin tümünün alfabesini karşılamak için tasarlanmıştır. alfabeler dışında ekstra karakterlerde vardır, fakat öncelik dilin alfabeleridir.

# glyph
bir karakterin yada herhangi bir olgunun, simgelenmiş halidir. bu şekilde metinlerde, duvarlarda o olgu gösterilmektedir. bazen bazı karakterlerin birden fazla glyph karşılığı olabilir. glyph'ler kültürden kültüre farklı anlamlara çıkabilmektedir.

bir font dosyasındaki her karakter, birer glyph'tir.

# yüzde işareti (yada percent sign)

% işareti ile temsil edilir. ingilizcede yüzde 5; 5% ile yazılırken, türkçede %5 ile yazılır.

‰ işareti "per mile" olarak isimlendiriliyor. fakat 1000'i temsil ediyor. "per mil" karakterindeki "mil" kelimesi uzunluk birimi olan mil'den (1 mil = 1 km değil! ve kara ve denizde farklı değeri vardır) gelmemektedir.

45% = 0.45 = 45/100 eşitliğini göstermek için kullanılır. + - işaretleri gibi her istenilen yere konulamaz. özel bir gösterim karakteri olduğu için bazı özel durumlarda kullanılır. örnek:

- 45% = 0.45 // tek başına kullanıldığında bu anlama geliyor.

- 200 − 10% = 180 //burada 10% ile; 200'ün yüzde 10'u alınmıştır. yani; 200-20=180.

- 200 − 10% + 3 //hatalı kullanım.

# dosyayı byte array olarak ekranda okuma

linux komut satırı uygulamaları ile yapılabilir. örnek olarak içeriği aşağıdaki gibi olan bir dosya için; 

123456789123456789

- 16'lık tabanda görebilmek için:

> hexdump myfile.txt

```
0000000 3231 3433 3635 3837 3139 3332 3534 3736

0000010 3938 3231 3433 3635 3837 3139 3332 3534

0000020 3736 3938 3231 3433 3635 3837 0a39     

000002e
```

- 8'lik tabanda görebilmek için:

```
0000000 031061 032063 033065 034067 030471 031462 032464 033466

0000020 034470 031061 032063 033065 034067 030471 031462 032464

0000040 033466 034470 031061 032063 033065 034067 005071

0000056
```

-- yukarıdaki çıktılarda ilk sutun aralıklardır. sadece output'u guplandırmak için komut satırı uygulamalasının kullandığı gurplama tekniğidir.

-- bir satırda sadece # karakteri var ise; bunun anlamı üstteki satır ile birebir aynı olduğudur. -v parametesi ile bu durum iptal edilebilir.

-- 8'lik tabanda output'ta gösterilen bir değer max 7 değerini alabilir. 7 için 3 bit'e ihtiyacımız var. 1 byte 8 bit olduğundan, 8lik tabanda 1 byte'ı göstermek için 3 karaktere ihtiyacımız var. ve en yüksek öncelikli karakterde gereksiz boş yer kalmaktadır. örnek: octal(177) = hex(7F) = binary(1 111 111). örnekte gözüktüğü gibi output'u en erimli görebilmek için 4 bite denk gelen 16'lık taban kullanılmalıdır. octal gösterimde 2'inci byte, örnekte 177'yi en az 1 arttırarak 200'a çıkaracaktır.

-- bazı text editörler satır sonu karakterlerini (örnek 0a) otomatik atarlar ve ekranda (son kullanıcıya - GUI'de) satır sonunu göstermeyebilirler.

-- bazı editörler BOM karakterini dosyanın başına otomatik atarlar.

# text dosyasında encodingi otomatik algılatma

text editorler dosyayı açtıklarında deneme yanılma yöntemi il doğru formatı bulmaya çalışırlar. örnek utf-16 mı diye sorguladıklarında, her karakter için bir karaktere ulaşılabiliyor ise o formatı geçerli kılarlar. fakat bu her zaman %100 doğru encoding'i bulabilme durumu söz konusu diildir.

# java char

java'da "char" 2 byte uzunluğundadır. unsigned 16 bit sayı olarak tutulur. bu sebeple unicode'daki U+FFFF üstü karakterler java'nın char tipi tarafından desteklenememektedir.

tanımlamak için: char myChar = '\u0003'; yazılır. Bu sebeple int'e cast işlemi sonucu "3" değerini alırız. 

Character sınıfı char tipini constructor'dan alır ve wrap eder. Character sınıfıda char'ı wrap ettiği için yine  U+FFFF üstü karakterleri destekleyemez.

Aşağıdaki gibi tanımlama yapabiliriz:

```java
char myChar = '\u0003';

Character myCharClass = new Character(myChar);

myCharClass.hashCode(); // int 3 değerini döndürür

myCharClass.compareTo(anotherCharacter); //eşitlerse 1 değillerse 0'ı döndürür. eşitlik unicode codepointi ile yapılır.
```

Character sınıfının metodları unicode'un her karakter için belirlediği property'leri de döndürmektedir. örnek: lowercase, unicodeblock... metod isimleri direk anlaşılmaktadır. tek istisna getType() metodudur ki bu "genel kategori" bilgisini döndürmektedir.

Char karakteri 16 bit olduğundan tüm unicode desteklenememektedir. daha sonraları char boyutu yükseltilmek istenmiş fakat bu köklü değişiklikler gerektirdiğinden string alternatifi farklı sınıfılar sunulmuş, ve Character sınıfına bazı destekleyici metodlar eklenmiştir. örnek:


charCount(int codePoint) metodu: codepoint'imizi int olarak gönderiyoruz. eğer U+FFFF 'ten büyükse (yani 16 bit'e sığmayacaksa) 2 dönüyor. diğer durumlarda 1 dönüyor.

boolean isSupplementaryCodePoint(int codePoint) metodu: charCount metodu ile ynı şeyi belirtmektedir fakat boolean ile dönüş yapar.

Character sınıfı içindeki birçok aynı isimde metod hem int (codepoint) yada char alacak şekilde ayarlanmıştır. char alan metodlar eskiden vardı, int alanlar ise sonradan geliştirildi. int almalarının sebebi codepoint'i U+FFFF üstü değerleri int olarak verebilmemizi sağlamaktadır.

Character.toCodePoint(char high, char low) --> char array gönderiyoruz. elimizdeki bir 2 ielemanlı bir char[]'in ilk ve ikinci elemanını buraya yolluyoruz. char[2] 32 bittir. buraya yolladığımız karakter br code-unit'tir. yani (int) char[2] bize codeunit vermelidir. bu metod ise bize onun code point'ini döndürmektedir. 

Character.toChars(int codePoint) --> codepoint'e karşılık char[2] yada char[1] döndürmektedir. döndürülen bu char[]; (int) char[2] bize codeunit verir.

Character.isSurrogatePair(char high, char low) --> toCodePoint metodu ile aynı parametreleri alıyor. toCodePoint'in iki elemanlı dizi dönüp dönmeyeceği bilgisini veriyor. 

# java String

String kendi içinde char[] tutar. her "char" kendi içinde Unicode codepoint'i ile tutulur. String'in mimarisini constructor'larına bakarak kavrayabiliriz: 

String constructor'u eğer String alıyorsa o zaman charset parametresi istemez. örnek: new String("abc"); çünkü yolladığımız string'i karakterlere böler ve char array'e atar. zaten her karakter artık bellidir. bu karakterlerin unicode'da tanımlı olması şarttır. zaten yolladığımız string'de bir java string'idir. bu işlem sonucu kopyalama işlemi gerçekleşir.

"new String(byteArray, "UTF-8");" ile byte-Array'ı char-array'e çevirmesi için karaketer seti bilgisini yollamamız gerekir. eğer yollamazsak default işletim sisteminin karakter-encoding'ini kullanacaktır. burada byteArray aslında UTF-8 encoding'inde olduğuu belirtiyoruz, ve constructor'a bize java String'i dönmesini istiyoruz. java string'inin ne tuttuğu önemli diil.

String constructor'u eğer unmappable karakter okursa bunu default/özel bir karakter ile replace eder ve bu şekilde char[] oluşturur.

String'in toString metodu dışarıya string'i verirken kendini return eder. bu string'i bir yere basmak için bir stream'e yollamak lazım. bu stream bunu alırken nasıl okuduğu önemlidir. stream, isterse karakter karakter okur, isterse byte byte okur.

"text".getBytes("UTF-8"); ile istediğimiz çeşit encoding'de byte array olarak text'i döndürür.

yukarıdan görüldüğü gibi; bizim için charset ancak şu durumlarda gerekiyor:
- string'den dışarı byte-array çıkarmak istediğimizde
- string'i byte-array ile initialize etmek istediğimizde

bunun dışıdan java kendi içinde nasıl tuttuğu önemli dğeil. her sürümde farklı şekilde tutuyor olabilir. emin olduğumuz bir şey var: jvm bizim için her karaktere tekabül eden codepoint'ler ile string'imizi tutuyor.


- örnek 1:

  yukarıda anlatıldığı gibi;
  - Java char'larda codepoint tutuyor
  - string ise char array tutuyor. 

  şimdi şöyle bir örnek üzerinden gidelim:

  sunucumuza CP1250 formatında request gelsin.

  HTTPServletRequest.getParameter("name") bize beklenmedik karakterler döndürüyor olsun. önce bu string'in bize gelmeden hangi aşamalardan geçtiğini trace edelim:

  - web server'a gelen istek servlet tarafından karşılanıyor.
  - servlet'e gelen istek byte array olarak karşılanır
  - arada filter'larımız olabilir
  - daha sonra getParameter("name") bize string olarak (ilgili alanı, sadece "name" kısmını) döndürür.

  getParameter bize hatalı döndü. çünkü java byte-array'i string'e çevirirken, byte-arrya'in CP1250 olduğunu bilmiyor ve farklı bir encoding'de okuyor. Doğal olarak; String'e yanlış çevriliyor. Örneğin en başında belirttiğimiz kurallar gereği artık string'imiz: byte-array barındırmıyor, Code-point barındırıyor. Dolayısı elimizde ile artık yanlış code-point'li karakterler var. Artık string'imizi CP1250'e çevirme gibi bir durumumuz yok. Bu sebeple şöyle bir çözüme gidilebilir:

  Filter oluşturulur:

  ```java
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {

    if (request.getCharacterEncoding() == null) { //Güncel HTTP standratlarında encoding bilgiside gönderilebiliyor.
      
        request.setCharacterEncoding("cp1250");
    }
    chain.doFilter(request, response);
  }
  ```

  Filter'da encoing set edilince artık, HTTPServletRequest.getParameter("name") metodu byte-array'den bizre string döndürürken cp1250 encoding'ini kullanacaktır.

  HTTPServletRequest.getParameter("name", "cp1250") gibi bir metod sunmuyor. bu sebeple yukarıdaki yöntemi kullanmak gerekli.

- örnek 2

  Console'dan karakter okuduğumuz zaman işletim sisteminini default encoding'i ile karakterler okunur. işlemi trace edersek:

  - console uygulaması input olarak karakteri byte'a çevirir.
  - console uygulaması byte'a çevirirken (özel ayar belirtilmemişse) işletim sisteminin default encoding'i ile karakterleri byte'a çevirir.
  - bu byte'ları array olarak programımıza yollar.
  - programımız stream'den (özel ayar belirtilmemişse) işletim sistemininin default encoding'i ile byte'ları string'e çevirir.
  
  Kod içerisinde (örneğin java) steram'in ecdoing'ini değiştirmek istersek şu kodu kullanabiliriz:

  ```java
  Scanner console = new Scanner(new InputStreamReader(System.in, "cp1250"));
  ```

# Modified UTF-8 (yada MUTF-8)

JVM'in kendi içindeki sınıflarda kulandığı UTF-8 türevi encoding'dir. Dışarıya çıktı/girdi alan sınıflarda kullanılmaz. Dolayısı ile uç durumlar haricinde yazılımcıyı ilgilendiren bir durum diildir.

MUTF-8 encoidng'inin kullanıldığı yerler:

- DataInput sınıfı içindeki String readUTF() MUTF-8 okur ve bize String döndürür.

- JVM serialize/deserialize yaparken MUTF-8 kullanır.

# Console'a basarken yaşanan karakter problemleri

Bir log attığımızda eclipse console'u veya aynı text'i dosyaya attığımızda farklı biçimlerde ekranda görünebilir. bu tarz sorunların sebebi her Reader'ın (console, text editor...) okuduğunda encoding'i bilmemesinden kendi default varsaydığı encoding'de olduğunu varsayıp okuyup ekranda göstermesidir.

# code page

unicode çıkmadan önce her işletim sistemi belli karakter kümelerini gruplayarak isimlendirmekteydi. bu gruplara code page deniliyor.

code page bir encoding değil, sadece karakter setidir. örneğin; sistem hangi karakter seti destekliyor dendiğinde, X code page'ini destekliyor denilirdi.

bir code page içerisinde, her karaktere tekabül eden bir sayı vardır. bu sayıya "code point" denir. GUI aracılığı ile bir textbox'a metin girerken, ALT+123 şeklinde bir tuşlama yazdığımızda, işletim sistemininin varsayılan code point'i üzerindeki 123'üncü sayıyı çağırmış oluyoruz. ALT+123'a "alt code" adı verilir.

çoğu zaman codepoint'ler 256 karakter uzunluğunda tutulur. bu şekilde ilk karakter ve son karakter, bir sayı ile temsil edilmek istendiğinde hepsinin byte aralığı sabit olur (1 byte). aslında bu bilgiyi byte olarak dosyaya yazdığımızda encoding görevi görmüş olacaktır. fakat code page encoding standartdı dwğildir, çünkü code point'in nasıl bellekte tutulacağının bilgisini içermez. byte olarak kaydetmek en mantıklısıdır ve çoğu text editor, bir dosyayı bir code page standartlarında kaydettiği zaman, byte olarak kaydeder. fakat bunu BCD'de kaydedebilir, yada farklı yöntemlerde tercih edebilir. bu yazılımın kendisine kalmıştır.

ms-windows kendi API'lerinde code page'lerin tümünü bu şekilde referanslıyor:

aşağıdaki tabloda utf-8'i de görmekteyiz. bunun sebebi; o code page'in barındırdığı tüm karakterler, utf-8 'in desteklediği tüm karakterlerdir.

aşağıda "mac" keywordü içeren satırlar mevcut. bunun sebebi; mac'te kaydedilmiş dosyaları, windows tarafında okuyabilmektedir.

kaynak: https://docs.microsoft.com/en-us/windows/desktop/Intl/code-page-identifiers (mayıs 2019)

| Identifier | .NET Name               | Additional information                                                                              |
|------------|-------------------------|-----------------------------------------------------------------------------------------------------|
| 037        | IBM037                  | IBM EBCDIC US-Canada                                                                                |
| 437        | IBM437                  | OEM United States                                                                                   |
| 500        | IBM500                  | IBM EBCDIC International                                                                            |
| 708        | ASMO-708                | Arabic (ASMO 708)                                                                                   |
| 709        |                         | Arabic (ASMO-449+, BCON V4)                                                                         |
| 710        |                         | Arabic - Transparent Arabic                                                                         |
| 720        | DOS-720                 | Arabic (Transparent ASMO); Arabic (DOS)                                                             |
| 737        | ibm737                  | OEM Greek (formerly 437G); Greek (DOS)                                                              |
| 775        | ibm775                  | OEM Baltic; Baltic (DOS)                                                                            |
| 850        | ibm850                  | OEM Multilingual Latin 1; Western European (DOS)                                                    |
| 852        | ibm852                  | OEM Latin 2; Central European (DOS)                                                                 |
| 855        | IBM855                  | OEM Cyrillic (primarily Russian)                                                                    |
| 857        | ibm857                  | OEM Turkish; Turkish (DOS)                                                                          |
| 858        | IBM00858                | OEM Multilingual Latin 1 + Euro symbol                                                              |
| 860        | IBM860                  | OEM Portuguese; Portuguese (DOS)                                                                    |
| 861        | ibm861                  | OEM Icelandic; Icelandic (DOS)                                                                      |
| 862        | DOS-862                 | OEM Hebrew; Hebrew (DOS)                                                                            |
| 863        | IBM863                  | OEM French Canadian; French Canadian (DOS)                                                          |
| 864        | IBM864                  | OEM Arabic; Arabic (864)                                                                            |
| 865        | IBM865                  | OEM Nordic; Nordic (DOS)                                                                            |
| 866        | cp866                   | OEM Russian; Cyrillic (DOS)                                                                         |
| 869        | ibm869                  | OEM Modern Greek; Greek, Modern (DOS)                                                               |
| 870        | IBM870                  | IBM EBCDIC Multilingual/ROECE (Latin 2); IBM EBCDIC Multilingual Latin 2                            |
| 874        | windows-874             | ANSI/OEM Thai (ISO 8859-11); Thai (Windows)                                                         |
| 875        | cp875                   | IBM EBCDIC Greek Modern                                                                             |
| 932        | shift_jis               | ANSI/OEM Japanese; Japanese (Shift-JIS)                                                             |
| 936        | gb2312                  | ANSI/OEM Simplified Chinese (PRC, Singapore); Chinese Simplified (GB2312)                           |
| 949        | ks_c_5601-1987          | ANSI/OEM Korean (Unified Hangul Code)                                                               |
| 950        | big5                    | ANSI/OEM Traditional Chinese (Taiwan; Hong Kong SAR, PRC); Chinese Traditional (Big5)               |
| 1026       | IBM1026                 | IBM EBCDIC Turkish (Latin 5)                                                                        |
| 1047       | IBM01047                | IBM EBCDIC Latin 1/Open System                                                                      |
| 1140       | IBM01140                | IBM EBCDIC US-Canada (037 + Euro symbol); IBM EBCDIC (US-Canada-Euro)                               |
| 1141       | IBM01141                | IBM EBCDIC Germany (20273 + Euro symbol); IBM EBCDIC (Germany-Euro)                                 |
| 1142       | IBM01142                | IBM EBCDIC Denmark-Norway (20277 + Euro symbol); IBM EBCDIC (Denmark-Norway-Euro)                   |
| 1143       | IBM01143                | IBM EBCDIC Finland-Sweden (20278 + Euro symbol); IBM EBCDIC (Finland-Sweden-Euro)                   |
| 1144       | IBM01144                | IBM EBCDIC Italy (20280 + Euro symbol); IBM EBCDIC (Italy-Euro)                                     |
| 1145       | IBM01145                | IBM EBCDIC Latin America-Spain (20284 + Euro symbol); IBM EBCDIC (Spain-Euro)                       |
| 1146       | IBM01146                | IBM EBCDIC United Kingdom (20285 + Euro symbol); IBM EBCDIC (UK-Euro)                               |
| 1147       | IBM01147                | IBM EBCDIC France (20297 + Euro symbol); IBM EBCDIC (France-Euro)                                   |
| 1148       | IBM01148                | IBM EBCDIC International (500 + Euro symbol); IBM EBCDIC (International-Euro)                       |
| 1149       | IBM01149                | IBM EBCDIC Icelandic (20871 + Euro symbol); IBM EBCDIC (Icelandic-Euro)                             |
| 1200       | utf-16                  | Unicode UTF-16, little endian byte order (BMP of ISO 10646); available only to managed applications |
| 1201       | unicodeFFFE             | Unicode UTF-16, big endian byte order; available only to managed applications                       |
| 1250       | windows-1250            | ANSI Central European; Central European (Windows)                                                   |
| 1251       | windows-1251            | ANSI Cyrillic; Cyrillic (Windows)                                                                   |
| 1252       | windows-1252            | ANSI Latin 1; Western European (Windows)                                                            |
| 1253       | windows-1253            | ANSI Greek; Greek (Windows)                                                                         |
| 1254       | windows-1254            | ANSI Turkish; Turkish (Windows)                                                                     |
| 1255       | windows-1255            | ANSI Hebrew; Hebrew (Windows)                                                                       |
| 1256       | windows-1256            | ANSI Arabic; Arabic (Windows)                                                                       |
| 1257       | windows-1257            | ANSI Baltic; Baltic (Windows)                                                                       |
| 1258       | windows-1258            | ANSI/OEM Vietnamese; Vietnamese (Windows)                                                           |
| 1361       | Johab                   | Korean (Johab)                                                                                      |
| 10000      | macintosh               | MAC Roman; Western European (Mac)                                                                   |
| 10001      | x-mac-japanese          | Japanese (Mac)                                                                                      |
| 10002      | x-mac-chinesetrad       | MAC Traditional Chinese (Big5); Chinese Traditional (Mac)                                           |
| 10003      | x-mac-korean            | Korean (Mac)                                                                                        |
| 10004      | x-mac-arabic            | Arabic (Mac)                                                                                        |
| 10005      | x-mac-hebrew            | Hebrew (Mac)                                                                                        |
| 10006      | x-mac-greek             | Greek (Mac)                                                                                         |
| 10007      | x-mac-cyrillic          | Cyrillic (Mac)                                                                                      |
| 10008      | x-mac-chinesesimp       | MAC Simplified Chinese (GB 2312); Chinese Simplified (Mac)                                          |
| 10010      | x-mac-romanian          | Romanian (Mac)                                                                                      |
| 10017      | x-mac-ukrainian         | Ukrainian (Mac)                                                                                     |
| 10021      | x-mac-thai              | Thai (Mac)                                                                                          |
| 10029      | x-mac-ce                | MAC Latin 2; Central European (Mac)                                                                 |
| 10079      | x-mac-icelandic         | Icelandic (Mac)                                                                                     |
| 10081      | x-mac-turkish           | Turkish (Mac)                                                                                       |
| 10082      | x-mac-croatian          | Croatian (Mac)                                                                                      |
| 12000      | utf-32                  | Unicode UTF-32, little endian byte order; available only to managed applications                    |
| 12001      | utf-32BE                | Unicode UTF-32, big endian byte order; available only to managed applications                       |
| 20000      | x-Chinese_CNS           | CNS Taiwan; Chinese Traditional (CNS)                                                               |
| 20001      | x-cp20001               | TCA Taiwan                                                                                          |
| 20002      | x_Chinese-Eten          | Eten Taiwan; Chinese Traditional (Eten)                                                             |
| 20003      | x-cp20003               | IBM5550 Taiwan                                                                                      |
| 20004      | x-cp20004               | TeleText Taiwan                                                                                     |
| 20005      | x-cp20005               | Wang Taiwan                                                                                         |
| 20105      | x-IA5                   | IA5 (IRV International Alphabet No. 5, 7-bit); Western European (IA5)                               |
| 20106      | x-IA5-German            | IA5 German (7-bit)                                                                                  |
| 20107      | x-IA5-Swedish           | IA5 Swedish (7-bit)                                                                                 |
| 20108      | x-IA5-Norwegian         | IA5 Norwegian (7-bit)                                                                               |
| 20127      | us-ascii                | US-ASCII (7-bit)                                                                                    |
| 20261      | x-cp20261               | T.61                                                                                                |
| 20269      | x-cp20269               | ISO 6937 Non-Spacing Accent                                                                         |
| 20273      | IBM273                  | IBM EBCDIC Germany                                                                                  |
| 20277      | IBM277                  | IBM EBCDIC Denmark-Norway                                                                           |
| 20278      | IBM278                  | IBM EBCDIC Finland-Sweden                                                                           |
| 20280      | IBM280                  | IBM EBCDIC Italy                                                                                    |
| 20284      | IBM284                  | IBM EBCDIC Latin America-Spain                                                                      |
| 20285      | IBM285                  | IBM EBCDIC United Kingdom                                                                           |
| 20290      | IBM290                  | IBM EBCDIC Japanese Katakana Extended                                                               |
| 20297      | IBM297                  | IBM EBCDIC France                                                                                   |
| 20420      | IBM420                  | IBM EBCDIC Arabic                                                                                   |
| 20423      | IBM423                  | IBM EBCDIC Greek                                                                                    |
| 20424      | IBM424                  | IBM EBCDIC Hebrew                                                                                   |
| 20833      | x-EBCDIC-KoreanExtended | IBM EBCDIC Korean Extended                                                                          |
| 20838      | IBM-Thai                | IBM EBCDIC Thai                                                                                     |
| 20866      | koi8-r                  | Russian (KOI8-R); Cyrillic (KOI8-R)                                                                 |
| 20871      | IBM871                  | IBM EBCDIC Icelandic                                                                                |
| 20880      | IBM880                  | IBM EBCDIC Cyrillic Russian                                                                         |
| 20905      | IBM905                  | IBM EBCDIC Turkish                                                                                  |
| 20924      | IBM00924                | IBM EBCDIC Latin 1/Open System (1047 + Euro symbol)                                                 |
| 20932      | EUC-JP                  | Japanese (JIS 0208-1990 and 0212-1990)                                                              |
| 20936      | x-cp20936               | Simplified Chinese (GB2312); Chinese Simplified (GB2312-80)                                         |
| 20949      | x-cp20949               | Korean Wansung                                                                                      |
| 21025      | cp1025                  | IBM EBCDIC Cyrillic Serbian-Bulgarian                                                               |
| 21027      |                         | (deprecated)                                                                                        |
| 21866      | koi8-u                  | Ukrainian (KOI8-U); Cyrillic (KOI8-U)                                                               |
| 28591      | iso-8859-1              | ISO 8859-1 Latin 1; Western European (ISO)                                                          |
| 28592      | iso-8859-2              | ISO 8859-2 Central European; Central European (ISO)                                                 |
| 28593      | iso-8859-3              | ISO 8859-3 Latin 3                                                                                  |
| 28594      | iso-8859-4              | ISO 8859-4 Baltic                                                                                   |
| 28595      | iso-8859-5              | ISO 8859-5 Cyrillic                                                                                 |
| 28596      | iso-8859-6              | ISO 8859-6 Arabic                                                                                   |
| 28597      | iso-8859-7              | ISO 8859-7 Greek                                                                                    |
| 28598      | iso-8859-8              | ISO 8859-8 Hebrew; Hebrew (ISO-Visual)                                                              |
| 28599      | iso-8859-9              | ISO 8859-9 Turkish                                                                                  |
| 28603      | iso-8859-13             | ISO 8859-13 Estonian                                                                                |
| 28605      | iso-8859-15             | ISO 8859-15 Latin 9                                                                                 |
| 29001      | x-Europa                | Europa 3                                                                                            |
| 38598      | iso-8859-8-i            | ISO 8859-8 Hebrew; Hebrew (ISO-Logical)                                                             |
| 50220      | iso-2022-jp             | ISO 2022 Japanese with no halfwidth Katakana; Japanese (JIS)                                        |
| 50221      | csISO2022JP             | ISO 2022 Japanese with halfwidth Katakana; Japanese (JIS-Allow 1 byte Kana)                         |
| 50222      | iso-2022-jp             | ISO 2022 Japanese JIS X 0201-1989; Japanese (JIS-Allow 1 byte Kana - SO/SI)                         |
| 50225      | iso-2022-kr             | ISO 2022 Korean                                                                                     |
| 50227      | x-cp50227               | ISO 2022 Simplified Chinese; Chinese Simplified (ISO 2022)                                          |
| 50229      |                         | ISO 2022 Traditional Chinese                                                                        |
| 50930      |                         | EBCDIC Japanese (Katakana) Extended                                                                 |
| 50931      |                         | EBCDIC US-Canada and Japanese                                                                       |
| 50933      |                         | EBCDIC Korean Extended and Korean                                                                   |
| 50935      |                         | EBCDIC Simplified Chinese Extended and Simplified Chinese                                           |
| 50936      |                         | EBCDIC Simplified Chinese                                                                           |
| 50937      |                         | EBCDIC US-Canada and Traditional Chinese                                                            |
| 50939      |                         | EBCDIC Japanese (Latin) Extended and Japanese                                                       |
| 51932      | euc-jp                  | EUC Japanese                                                                                        |
| 51936      | EUC-CN                  | EUC Simplified Chinese; Chinese Simplified (EUC)                                                    |
| 51949      | euc-kr                  | EUC Korean                                                                                          |
| 51950      |                         | EUC Traditional Chinese                                                                             |
| 52936      | hz-gb-2312              | HZ-GB2312 Simplified Chinese; Chinese Simplified (HZ)                                               |
| 54936      | GB18030                 | Windows XP and later: GB18030 Simplified Chinese (4 byte); Chinese Simplified (GB18030)             |
| 57002      | x-iscii-de              | ISCII Devanagari                                                                                    |
| 57003      | x-iscii-be              | ISCII Bangla                                                                                        |
| 57004      | x-iscii-ta              | ISCII Tamil                                                                                         |
| 57005      | x-iscii-te              | ISCII Telugu                                                                                        |
| 57006      | x-iscii-as              | ISCII Assamese                                                                                      |
| 57007      | x-iscii-or              | ISCII Odia                                                                                          |
| 57008      | x-iscii-ka              | ISCII Kannada                                                                                       |
| 57009      | x-iscii-ma              | ISCII Malayalam                                                                                     |
| 57010      | x-iscii-gu              | ISCII Gujarati                                                                                      |
| 57011      | x-iscii-pa              | ISCII Punjabi                                                                                       |
| 65000      | utf-7                   | Unicode (UTF-7)                                                                                     |
| 65001      | utf-8                   | Unicode (UTF-8)                                                                                     |

# GILT

Globalization, Internationalization, Localization and Translation'ın kısaltmasıdır.

yazılım geliştirme sürecinde dil desteği için kullanılan terimlerdir.

### Translation (yada T9N)

bir yazılımın text'lerinin başka bir dile çevrilme işlemidir.

### Localization (yada l10n)

yazılım geliştirme sürecinde, içeriklerin ve bilgisayar yazılımlarının belirli bir coğrafyaya ya da etnik topluluğa özgü pazar ya da coğrafi bölgede geçerli yerel dilsel ve kültürel özelliklere uyarlanmasıdır. dolayısı ile; T9N'u ve daha fazlasını kapsar.

### Internationalization (i18n)

bir yazılımın birden fazla dili göre destekleme işlemidir.

### Globalization (yada g11n)

i18n'i ve daha fazlaısnı kapsar. i18n olaya sadece metin bazında bakarken, g11n; kültürel açıdan da coğrafyalara uyumlu olbilmesini kapsar.

# a11y

accessibility özelliğini kazandırma işlemidir. GILT grubuna dahil değildir.

# i18n kütüphaneleri

örnek bir kütüphaneyi inceleyelim: jQuery.i18n

ilgili dildeki string karşılığını döndürüyor:

> $.i18n( 'message-key-sample1' ); 

#### Placeholder

```js
var message = "Welcome, $1";

$.i18n(message, 'Alice'); // This gives "Welcome, Alice" 
```

#### Plural (yada çoğul)

```js
var message = "Found $1 {{PLURAL:$1|result|results}}";

$.i18n(message, 1); // This gives "Found 1 result"

$.i18n(message, 4); // This gives "Found 4 results"
```

yada

```js
var message = 'Box has {{PLURAL:$1|one egg|$1 eggs|12=a dozen eggs}}.';

$.i18n(message, 4 ); // Gives "Box has 4 eggs."

$.i18n(message, 12 ); // Gives "Box has a dozen eggs."
```

Yuakarıdaki örnek ingilizce içindir. Bazı dillerde çoğul kelime olduğunda yanındaki kelimelerde hiçbir fark meydana gelmezken, bazı dillerde ise 2'den fazla form olabilmektedir.

Common Locale Data Repository (yada CLDR), Unicode ekibininin public olarak yayınladığı çoğul kelimelerin sayılara göre naısl değiştiğini açıklayan kurallar dökümantasyonudur. Hem dökümantasyon hem de parse edilebilmesi için XML olarak yayımlanır. Bu dökümantasyona göre parse eden program kütüphaneleri mevcuttur.

Yukarıdaki örnekte "{{PLURAL:$1|res" kod kısmı PRURAL yazdığı için 2 parametre alıyor. Burada CDRL parser ingilizce için 2 adet form olduğunu biliyor. bu sebeple 2 parametre alıyor. yani;

ingilizce için: var message = "Found $1 {{PLURAL:$1|result|results}}";

arapça için:  var message = "Found $1 {{PLURAL:$1|zero|one|two|few|many|other}}";

eğer arapça için şunu yazarsak: {{PLURAL:$1|A|B}} o zaman 0 için A, diğer tüm değerler için B kullanılacaktır.

Yuakrıdaki "var message" aslında dil dosyasındaki json text'inden gelmelidir. 

#### Gender (yada cinsiyet)

```js
var message = "$1 changed {{GENDER:$2|his|her}} profile picture";

$.i18n(message, 'Alice', 'female' ); // This gives "Alice changed her profile picture"

$.i18n(message, 'Bob', 'male' ); // This gives "Bob changed his profile picture"
```

#### Fallback

kütüphane init yapılırken ayarlanır. ğer bir tet'in karşılığını o andaki dilde bulamazsa, fallback olarak set edilen dil dosyasında arar. eğer onda da bulamazsa 2inci fallback dilini kontrol eder.

#### Message documentation

en.json, tr.json gibi dosyalar ile aynı dizinde qqq.json dosyası bulunması önerilir. bu dosya eun-time sırasında okunmaz/kullanılmaz. tranlator'ların okuması için bir dosyadır. key'ler yetersiz olduğunda translator bu dosyaya bakıp ne yazması gerektiğini çıkarır. örnek qqq.json dosyası:

```json
{

 "appname-title": "Application name",

 "appname-sub-title": "explanation of the application",

 "appname-about": "About this application text"

}
```

# Bir sayıyı metne çevirme yada metinden sayıyı çevirme

örnek: 

> 105 --> yüz beş

> 1208 -> bin iki yüz sekiz

bu işlemler i18n gibi kütüphanelerin görevi değildir. bu sebeple ekstra kütüphane kullanmak gerekiyor. 2018 yılı itibari ile hala bu işi yapan düzgün ve multi-language kütüphane mevcut değil.

bunu yapan kütüphaneleri genelde "number to word" terimleri ile arama yapmak gerekiyor.

# Tipoğrafik İşaretler

resmi dilin noktalama işaretleri dışında kalan karakterlerdir. aslında birer anlama gelen şekillerdir. @, &, # gibi karakterlerdir.

bir dilde tipografik olan karakter başka dilde tpografik olmayabilir.

# bazı karakterler

aşağıdaki karakterlerden UNICODE setinde birçok türev/benzeri vardır. fakat genelde aşağıdaki prefix/suffix'leri alırlar.

> ^

eng:caret (türkçede karşılığı yok)

aynı zamanda "düzeltme işareti (yada Circumflex)" olarak kullanılıyor. amacına göre değişiyor. karakterin üzerine şapka olarak geliyorsa düzeltme karakteri oluyor.

> –

dash (uzun çizgi)

tdk'da sadece hikayelerde konuşma cümlelelerinin başına koyulur. onun dışında hiçbir yerde kullanılmaz. fakat farklı dillerde farklı yerlerde kullanılabiliyor.

> -

Hyphen (yada Tire yada kısa çizgi)

tdk'da konuşma çizgisi dışında olan tüm çizgiler buna tekabül eder.

> ' '

Kesme işareti (yada apostrof yada apostrophe)

> ‘ '

Tek Tırnak İşareti

kesme işaretinden farklıdır.

> "

Denden (yada double quotation mark)

Right ve left olan iki farklı "double quotation mark" daha var.

tdk'da üstteki satırın aynısı kopyalamak için kullanılır.

> ′

Prime

matematik gibi bilimlerde kullanılır. örnek: 3° 5′ 30″

> †

Dagger (yada obelisk yada obelus yada hançer)

obelus kelime anlamı: hançer

obelus kelime anlamı: başvurma işareti

genelde footer'larda kullanılıyor. farklı font'lar arasında görünüşü çok farkediyor.

> •

Bullet (yada madde işareti)

bullet türkçe kelime anlamı: mermi

> …

ellipsis (yada üç nokta)

tek başına bir karakterdir.

> ‹  

Single left-pointing angle quotation mark  

> ›  

Single right-pointing angle quotation mark  

> !  

exclamation mark (yada ünlem işareti)

> /  

slash (yada Eğik çizgi)

birçok türevi vardır: Fraction slash, Division slash, Fullwidth solidus

> \ 

Backslash (yada ters eğik çizgi)

> ( ) 

Yay Ayraç (yada Round brackets yada parentheses)

> [ ] 

köşeli ayraç (yada Square brackets yada simply brackets)

> { } 

Kaşlı ayraç (yada Curly brackets yada braces)

> ¶ 

pilcrow

resmi olmayan isimlendirmeleri: paragraph mark, paragraph sign, paraph, alinea, blind P

> @ 

Asperand (yada kuyruklu a)

> ~ 

tilde

Benzerlik, yaklaşıklık, denklik anlamında kullanılır.

> | 

pipe (yada verti-bar yada vbar yada stick yada vertical line yada vertical slash yada vertical bar yada obelisk yada Sheffer stroke)

> \#

kare işareti (yada sayı işareti yada hashtag işareti yada hash yada pound sign yada  number sign)

Müzikte kullanılan diyez bundan farklıdır. diyez hafif kayıktır.

> ÷ 

obelus (yada bölü işareti)

> ; 

Semicolon (yada semi colon yada Noktalı virgül)

> & 

ampersand (yada ampersant)

> Æ

ismi: "ash". alfabe karakteri değildir. metinlerimn okunularını gösterirken kullanılan bir karakterdir.

# Font

#### Type Family
karakterlerinin yapısını belirleyen kuralları içerir. örnek: Helvetica 

#### Typeface (yada typeface family yada font family)
bir ‘type family''ye ek olarak; bold, italic olup olmaması bilgisini birlikte içerir. örnek: Helvetica Bold.

#### Font
Typeface'e ek olarak; boyut bilgilerini içerir. örnek: Helvetica Bold 12px

#### font-face
tipografi'de geçen bir terim değildir. sadece css için font keyword'üdür.

#### serif
her karakterin uç kısımlarına ufak bir çizgiler koyulur. bu çizgiler seriftir. sans-serif ise bu çizgiler olmadan ki karakterleri temsil ediyor.

#### opentype vs truetype vs PostScript
alternatif font dosya saklama formatlarıdır.

#### tipoğrafi
metin yazmada kullanılan özellikleri inceleyen sanat dalıdır.

#### ClearType
Microsoft'un ekranlarda tüm fontları daha ince göstermek için geliştirdiği teknoloji. tüm fontları farklı bir biçime sokuyor. fontlar tasarlanırken özellikle ClearType'a uygun şekilde tasarlanırsa, ClearType daha başarılı oluyor. ClearType genel olarak LCD ekranlarda daha smooth bir görüntü yaratıyor.

# NLP (yada Natural Language Processing yada Doğal Dil İşleme)

"doğal dil"'den kasıt insan konuşması, yani; düzensiz konuşmadır.

NLP yapay zeka ve dilbilim alt kategorisidir. bu dal ile her türlü dili sözlü veya yazılı olarak anlama işlevleri yerine getirilmeye çalışılır. bunun için yazılımlar geliştirilmiştir. bu yazılımların örnek bazı görevleri ve YSA'ya bunun neresinde ihtiyaç duyulduğu:

- sesi algılayıp yazıya çevirme
  
  sesi tam olarak algılanamadığında YSA ile cümlenin akışına göre tahminleme de yapılabilir

- metni okuma

  bir metin okunduğunda birçok kişi farklı tonlarda okuyor. bu tonların daha düzgün olması için YSA'dan yararlanılıyor. eğer YSA olmadan metin okutulursa robotun okuduğu çok belli oluyor.

- dilbilgisi hata denetimi

  sadece hata denetimi için YSA işlevlerinden yararlanılmaz, fakat yanlış karakterler yerine düzgün kelime önerilerinde YSA'dan yararlanılabilir

- cümleyi veririz, bize her kelimenin kökünü ve detayını verir. örnek: "ben eve gidiyorum" dönüş olarak: [ {kok: ev, aldıgı-ek:"e", tip: nesne, adet: tekil, sıra: 2, verb:false ... } ... ]

- farklı dile çeviri işlemleri

  YSA ile çevrilecek konular daha net bir şekilde algılanıp diğer dile çevrilmesi kolay olabilir)

- OCR

  ysa bu metinlerin tam okunamadığı zamanlar tahminlemede kullanılıyor)

- bir cümleyi kategorileme: örnek banka uygulamamız olsun. müşteri istediği işlemi söyleyecek/yada yazacak bizde o işlemin ne olduğunu anlayıp işlemi gerçekleştiricez. müşteririnin verdiği cümle, bizim işlem uzayımızıdaki hangi cümle ile aynı kategoride olduğunu tespit/tahmin edebiliyor. örneğin; eğer tahmin oranı sonucu %90 oranla A işlemi çıktıysa, direk müşteriyi A işlemi sayfasına yönlendiririz. oysa %70 oranla A çıktıysa, müşteriye tekrar ne yapmak istediğini  şekilde açıklamasını isteriz. 

  burada YSA tahminleme oranlamasında kullanılıyor

genelde her dil için ayrı NLP yazılımları geliştiriliyor, fakat bu ortak yazılımların olmayacağı/olamayacağı anlamına gelmiyor.

# zemberek

özellikle Türkçe olmak üzere Türk dilleri için geliştirilmiş açık kaynak kodlu bir doğal dil işleme java kütüphanesidir. openoffice ve libreoffice için türkçe yazım denetimi eklentisi bu kütüphaneyi kullanır ve eklentide aynı ismi taşımaktadır.

# Optical character recognition (yada optical character reader yada OCR)

resim'den metin algılama işlemidir.

Tesseract komut satırı uygulaması (açık kaynaklı) bu işi yapmaktadır. engine olarak libtesseract'ı kullanmaktadır.

# open source NLP yazılımları

açık kaynaklı kütüphaneler mevcut. fakat henüz (2018 yılı) son kullanıcıya sunulacak derecede başarılı diiller. hiçbir NLP konusunda ticari olmayan NLP özelliği mevcut diil.

# google speech Api

google'ın speeach api'si client olan her taraftan çağrılabilmektedir.

google api'si sesi gerçek input-stream'ler ile gerçek zamanlı da alabiliyor. yani baştan sonra tek dosya halinde ses atılmak zorunda değil.

google sesi sunucularına atıyor. kişi sesini gereksiz seslerden arındırma yeteneğine sahip. sesi sadece metine çevirmekle kalmıyor, aynı zamanda her kelimenin yüzde kaç doğrulukla metne çevrildiği de tespit ediliyor ve yapay sinir ağı tüm cümleyi devrik olmayacak şekilde doğrulamaya çalışıyor. yapay siir ağı olduğundan; yüzde yüz bir sonuç olmuyor. google api'sinin result'ı bir string listesi oluyor. her elemanında kişinin kurduğu tüm cümle oluyor (baştan sona konuşması).

listesinin ilk elemanı google'ın en doğru düşündüğü cümle. ikinci elemanı daha az ihtimalle doğru olan cümle. listenin elemanı sıfıra yaklaştıkça daha yanlış sonuçlara gidilmektedir. her string elemanının bir de güvenilirlik oranı mevcut. böylece yazılımcı eğer ilk eleman en az %90 güvenilir diilse, uygulamaya işlem yapma gibisinden davranış sergiletebilir.

google-translate'ten daha başarısız oranla dili de otomatik algılayabiliyor. daha başarısız çünkü alfabetik harfleri göremiyor. sadece okunuşuyla değerlendiriyor. bu sebeple otomatik dil tanıma özelliği sunulmuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# process states

- Created: yeni oluşturulmuş, fakat process hala tam olarak RAM'e alınmamıştır.

- Ready: CPU ya girmek için bekliyor.

- Running: CPUda işlem görüyor. Kernel mode ve User mode olarak çalışırlar. "User mode" kernel instructions'ları çağıramaz.

- Blocked: CPU ya girsede bir işe yaramayacak durumdadır, çünkü dış donanımlardan (kaynaklardan) cevap bekleniyordur.

- Terminated: işi bitmiş process. parent process "exit status"'u okumadığı için henüz , process bu statüde bekler. bu şekilde çok process olabileceğinden bunlara "zombie process" denir.

işletim sistemlerinin ek olarak birçok farklı statüleri olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# concurrent (eş zamanlı) vs paralel

bu iki kavram sürekli birbirinin aynısı gibi bilinir. fakat ufak bir fark var: paralel çalışan işlemler, birden fazla cpu ile olmalıdır. yani 2 paralel thread var ise, en az 2 cpu olmalıdır. 2 işlem ancak bu şekilde paralel yürütülebilir. ancak concurrent işlemler; paralel olmak zorunda diillerdir. 2 farklı işlem birbirindenbağımsız yürür. aynı cpu'ya ikişer saniye aralıklarla girerlerse yine concurrent çalıştıkları anlamına gelir. kısaca; concurrent, birden fazla birbirindenbağımsız işlem olduğu; paralel ile zamanlama açısından ikisinin aynı anda işletileceğini belirtir.

# işlemci sayısı ve thread sayısı ilişkisi

- gerçekte bir bilgisayar tek thread çalıştırabilir. bir bilgisyarın cpu'su 2 işlemcili ise 2 thread aynı anda çalıştırabilir. yani 2 işlemi pararlel yapabilir. 

- standart bir pc'de işletim sistemleri binlerce thread aynı anda açık tutar. bu thread'lerin hepsi sanaldır. aslında her biri teker teker (daha doğrusu cpu sayımız kadarı) cpu'ya girerler. fakat işletim sistemi, üzerinde çalışan yazılımlar durmasın/hata vermesin diye onlara thread açar fakat beklemeye alır.

# cpu core

bir cpu içinde birden fazla çekirdeğe (core'a) sahip olabilir. 2 olan: Dual core, 4 olan: Quad Core gibi...

# cpu socket (yada cpu yuvası yada cpu slot)

- isminden de anlaşıldığı gibi; anakart üzerinde bulunan, cpu'nun takılması için tasarlanmış fiziksel giriştir.

- bir anakartta birden fazla cpu slotu olabilir.

# Hyper-threading

cpu teknolojisi ismidir. bu teknoloji ile gerçekte x çekirdekli onlar bir sunucu daha fazla çekirdekliymiş gibi işletim sistemine gösteriliyor. yani bir nevi sanal cpu çekirdeği yaratılıyor. bu şekilde eğer çalıştırılan yazılımlar çok çekirdekli teknolojiye göre yazılmışlar ise; bir nebze kara geçilebiliyor. Çok basit anlamada; cpu'nun boşta kalabileceği sürelerde diğer thread'ler devreye gireceği için performans avantajı olduğu düşünülür.

gerçekte 4 core'umuz olsun. her 1 core 2 core gibi davransın. bu durumda işletim sistemi 8 adet "logical processor" olduğunu algılayacaktır.

# Virtual Processor vs logical processor

ikiside aynı anlamdadır. fakat Virtual sanallaştırma yaparken, sanal olacak işletim sistemine atadığımız cpu core sayısını belirtmek için kullanılıyor.

# intel vt-x vs amd-v

intel ve amd'nin donanım sanallaştırması için kullandığı teknolojlerin isimleri. bir donanımı sanal olan makinalara bölüştürmek için yazılımsal olarakta sanallaştırma yapılabiliyor, fakat donanım desteği olunca daha hızlı ve güvenlidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# wrapper sınfıları

tüm programlama dilleri için ortak bir kavramdır. bazen bazı değerlerin (nesnelerin) bir sınıfın içinde bulunmasını isteyebiliriz. bunun birçok sebebi olabilir. örneğin; javadaki collections'lar primitive değerler alamazlar. fakat elimizdeki primitive değerleri bir listede toplamak istiyoruz. böyle olunca bu değerleri geçici bir sınıfa atarız, ve bu sınıflardan oluşan bir collection yaratırız.

Integer box = new Integer (x); //BOXING

int y = box.intValue(); //UNBOXING

wrapper sınıfları bazen "holder" sınıfları olarakta geçmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# liferay
Liferay, bir portal platformudur ve Ücretsiz (Community Edition) ve Ücretli (Enterprise Edition) olmak üzere iki şekilde sunulmaktadır.
liferay java'da yazılmıştır. OSGI kullanarak modüler bir yapıdadır.
liferay bir content management system (cms - içerik yönetim sistemi , joomla, drupal, WordPress gibi ) 'dır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# QA (yada Quality Assurance yada kalite güvencesi)

teknik insanlardan oluşan bir ekibin test ettiği platformdur.

# UAT (yada User acceptance Testing)

müşterinin (veya ürününü kullanacak olan kişilerin) test ettiği platformdur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# zaman

zaman hesaplamalarında saniye referans olarak alınmaktadır.

saniyenin tanımı şu şekildedir: Uluslararası Birimler Sistemine göre saniye, en düşük enerji seviyesindeki Sezyum-133 atomunun iki hyperfine seviye arasındaki geçiş radyasyonunun 9.192.631.770 periyoduna karşılık gelen süredir.

Bir saniyeye denk gelen birçok farklı yöntem geliştirilmiştir. Bu sebeple her cihaz yıllar sonra ufak sapmalar meydana getirmektedir.

# TimeStamp (yada Zaman damgası)

Fiziksel olarak zaman damgası; mürekkep ile belge üzerine basılan tarihtir. bu istenilen bir formatta olabilir. örneğin; etrafı çerçeveli, sadece gün/ay/yıl olan, ek olarak saati içerebilir gibi. Aynı şekilde elektronik ortamda da TimeStamp kullanılmaktadır. Bu değer istenilen formatta olabilir ve bazen sadece hesaplama ile bulunacak bir sayı ile temsil edilir.

> örnek1: unix timestamp

> örnek2: Tue 01-01-2009 6:00

> örnek3: 07:38, 11 December 2012 (UTC))

# Unix-Epoch time (yada POSIX time yada Unix time yada seconds since the Epoch)
1 Ocak 1970 UTC+0 Grinviç tarihinin gece yarısından bu yana geçen zamanın saniyeler cinsinden değeridir. terminolojik olarak bakıldığında; herhangi bir formatta tarih gösterimi "timestamp" olduğundan, "Unix time" da bir çeşit 'timestamp'tir.

İlk olarak; 3 November 1971'deki "Unix Programmer's Manual (PDF) (1st ed.)"'inde tanımlanmıştır. kaynak: https://www.bell-labs.com/usr/dmr/www/pdfs/man22.pdf (archive date: 10/10/2019). İlk tanımlandığında 32 bit ve '00:00:00, 1 January 1971'den itibarenki saniyeyi temsil ediyordu. 

# gösterim

zaman dilimi dünyanın herhangi bir yerinde aynıdır. sadece bölgesel olarak gösterilmek istendiğinde UTC+2 gibi ibarelerle gösterilirler. örneğin; aynı anda oluşturulan iki java-calendar objesi olsun. biri amerika biride türkiye bölgesine göre ayarlı olsun. bu değerler sadece ekrana basılırken farklıdır, aslında kendi içlerinde tuttukları "zaman" değeri ikisinde de aynıdır. yani; bu 2 calendar objesinin "Unix time"'ını alsak aynı değeri alırız.

# GMT (yada Greenwich Mean Time)

Londra'nın Greenwich bölglesinden geçen sıfır meridyeninin referans alınarak yapılan saat dilimi sistemidir.

12 saat doğuda, 12 saat batıda parçalara bölünmüştür.

# Universal Time (yada UT)
birçok versiyonu vardır. bunlar sürüm gibi düşünülmemelidir. hepsi birbirine alternatiftir.
- UTC (yada Coordinated Universal Time)
- UT0
- UT1
- UT1R
- UT2

# Coordinated Universal Time (yada UTC yada Eş Güdümlü Evrensel Zaman)

- UTC'nin baş harflerinden kısaltma elde edilmemektedir. bunun sebebi tamamen siyasidir.

- UTC saniyeleri IS (ISO standart birimleri) standartlarına göre alır (atomik saniye). Oysa GMT astronomik değerlere göre saniyeyi baz alır. UTC bu sebeple daha güvenlidir. Fiziksel cisimlerin duruma göre değişmez. GMT'nin açılımı 'Greenwich Mean Time'dır. Buradaki 'Mean' ortalama anlamına gelir. Yani GMT de bir yıl güneşin ve dünyanın dönüşünü tamamlayacak ortalama en optimum düzeyde hesaplanmıştır ve bu şekilde kabul edilir. 

- GMT ve UTC arasında çok çok az bir zaman farkı vardır. Bu fark zaman geçtikçe artabilir yada azalabilir. Çok ilerdeki tarihlerde fark çok açılabilir. 2019 yılına kadar, aradaki fark maximum 1 saniye olmuştur. Bu saniyelerde "__artık saniye (yada leap second)__" uygulaması ile giderilmektedir. Leap second GMT'ye uygulanmaz. UTC'ye uygulanır. GMT'ye uyumlu olması için yapılan manuel değişikliktir. Bu değişiklik uluslararası birimler tarafından ortak karar alınarak yapılır.

# artık saniye (yada leap second)
çok hassas hesaplamalar sonucu uluslararası alınan karar ile bazı günlerin sonuna (örnek 31 aralık gecesi) 1 saniye eklenmektedir yani 59:00'dan 00:00'a gçeiş 2 saniye sürmektedir.

- İlk artık saniye 1972'de uygulanmıştır.
- Şu ana dek (yıl 2019) sadece Haziran ve Aralık (yılın sonu ve ortası) aylarında artık saniye uygulanmıştır.
- Bazı yıllarda hem aralık hem de haziran ayı artık saniye uygulanmıştır.
- https://en.wikipedia.org/wiki/Leap_second (archive date: 18/12/2019) sayfasının "Announced leap seconds to date" başlığında uygulanan tüm artık saniyeler listelenmektedir.

POSIX time'da bu saniye ignore edilir (yani saniye ekleme yada çıkarma yapılmaz). kaynak: https://pubs.opengroup.org/onlinepubs/9699919799/xrat/V4_xbd_chap04.html (archive date: 18/12/2019) "A.4.16 Seconds Since the Epoch" başlığı ilk paragraf.

# artık yıl (yada leap year)
miladi takvimde yaşanan bir durumdur.

dünya güneş etrafında 365.24 küsür günde dönüyor. miladi takvime göre; her yıl 365 kabul edilmiştir. 4 yılda bir kere 29 şubat kullanılmaktadır. 29 şubat olan yıla "artık yıl" denir. artık yılın 4 senede bir olmasının bazı istisnası vardır:

istisnaya bir örnek: 100'ün katlarından 400'e kalansız bölünenler artık yıl diildir. örnek: 1600 artık yıldır. 1700 diildir. (bunun sebebi 1 yılın 365.24 küsür olması. 0.25(1/4) değil).

# UTC mi GMT mi "Unix-Epoch time" mi kulanmak gerekir?

Örneğin, uzayda herhangi bir gezegende yada noktada, örneğin uydulardaki astronotların, GMT ile bilgi alışverişi yapması best bractice olmayacaktır. orda UTC kullanılmalıdır. Çünkü UTC atomik saniyeyi baz alır.

GMT'yi eledik. Unix-Epoch time ve UTC kullanmak ikiside doğrudur. UTC kullanılabiliyorsa tercih edilmelidir. sebepleri:
- debug işlemlerini çok hızlandıracaktır çünkü direk okunduğunda değerin ne olduğu human-readable'dır.
- Unix-Epoch saniye bazında zaman tutuyor. eğer saniyeden ufak bir bilgi (birim) tutmaya ihtiyacımız olursa sorun yaşayabiliriz. Ek not: Unix-Epoch time için bazı kütüphaneler saniyeden ufak birimleri desteklemek için noktadan sonrasında da karakter desteklemektedir (örnek: 123456789.123). Fakat bunun resmi bir standartı yoktur. Dolayısı ile kullanmamakta fayda var.

Not: Özellikle string yerine integer gibi değerlerle kullanılan formatlarda birçok farklı problem de yaşanabiliyor:
- Y2K bug: 2000 yılına girildiğinde, yıl değerinin sadece son iki hanesini tutan sistemler sorun yaşamıştır.
- Y2K38 bug: signed 32 bit integer ile tututal Unix-Epoch değerleri, "03:14:07 UTC on 19 January 2038" sonrasını gösteremeyecek, onun yerine sıfır değerine resetlenecek yada hata fırlatacaktır.
- diğerleri: https://en.wikipedia.org/wiki/Time_formatting_and_storage_bugs (archive date: 20/10/2019)

# güneş zamanı (yada solar time)

Güneş'in ve dünyanın konumunu temel alan zaman birimleridir. temel zaman birimi gündür. ingilizcede gün'e 'solar day' de denir.

# Yaz saati uygulaması

DÜnyanın bazı ülkelerinde uygulanırken bazılarında uygulanmamaktadır. Genelde avrupada ve amerikada kullanılıyor. Yaz saati uygulaması varken türkiye; kış ve yaz, UTC+3 ve UTC+2 arasında değiştirirdi. Her ülke kendi yasalarınca saatleri kendi belirlediği için, her ülkenin saati meridyene göre hesaplanamamaktadır.

# timezone

dünyanın birçok noktasında timezone'lar belirlenmiştir. genelde her ülkenin bir timzone noktası vardır. ve bu noktanın etrafındaki belli bir bölge aynı saati kullanır.

confrafya ve ticari sebeplerden birden fazla timezone noktası aynı ülkede olabilir. bu noktalar için saatin kaç olacağı da sadece meridyene göre belirlenmemektedir. ticari ve siyasi sebeplerden meridyenle alakasız saat farkları olabilir. aynı zamanda; yaz saati uygulaması gibi belli dönemler aynı noktadaki saatler farklılık gösterebilir.

not: unutulmamalıdır ki; yaz saati veya benzeri bir uygulama 100 yıl boyunca X noktasında aynı kuralla tekrar edilmemektedir. örneğin; türkiye +03:00 kuralına 2014 ile geçilmiştir. bundan önceki yıllarda oradaki timezon +02:00'dır.

Timezone'lar text olarak gösterilmek istendiğinde; KıtaAdı/ŞehirAdı formatı kullanılmaktadır. bu resmi bir format değildir. fakat genelde böyle kullanılır. Örneğin; Europe/Istanbul, America/Chicago. Bazı yerler için sadece direk ülke yada şehir ismi kullanıldığı da olur.

örneğin türkiye için 3 farklı gösterim ile karşılaşılabilir: Europe/Istanbul yada Asia/Istanbul yada Turkey

# özel timezone isimleri

Özellikle sık kullanılan timezone'lara bu şekilde eş anlamlı kelimeler atanmıştır. örnek: 

- UTC+2, "Eastern European Time (yada EET)" ile eş anlamlı kullanılır.

- "Eastern European Summer Time (yada EEST)" ise UTC+3 ile eş anlamlıdır.

Politik sebeplerden "Central Africa Time (yada CAT)" ve "South African Standard Time (yada SAST)" UTC+2 ile eş anlamlıdır.

Yani bir timezone'un birçok eş anlamlı ismi olabilir. genelde sık kullanılanlara bu tarz takma isimler atarlar. Bir ülke (yada bir bölge) belli bir zaman diliminde (örneğin kışın ve bahar mevsimleri) EET kullanır, fakat yaz mevsimine geçerken EEST kullanabilir. Yani; EET kendi içinde kış veya yaz olarak farklı saat dilimlerine geçiş yapmamaktadır.

# mevsim (yada season)

mevsimler programatik olarak bulunması gerektiğinde ay bazında değil gün bazında bulunduğu unutulmamalıdır. örneğin 23 Eylül öncesi kuzeyde yaz mevsimi iken, 23 eylül sonrası sonbahardır. aynı tarihlerde güney yarımküredeki ülkeler için zıt mevsimlerdir.

dünya güneşin etrafında 365 günde döner ve dönüşünü sürekli 23 derece açı ile döner. dönüşünün yarısı boyunca bir yarımküre güneşe dik bakarken (o yarımkürede yaz mevsimi olur), diğerinde güneş dik açı ile vurmadığı için daha soğuk olur.

```
__________________________________________________________________________________

Kuzey yarımküre                  |            |             | Güney yarımküre

(yada Northern hemisphere)       | start date | end date    | (yada Southern Hemisphere)

için                             |            |             | için

__________________________________________________________________________________

İlkbahar (yada Spring)            21 Mart      22 Haziran    Sonbahar

yaz (yada summer)                 22 Haziran   23 Eylül      Kış

sonbahar (yada autumn yada fall)  23 Eylül     22 Aralık     İlkbahar

kış (yada winter)                 22 Aralık    21 Mart       Yaz

__________________________________________________________________________________
```

# astronomical seasons vs meteorological seasons

İngiltere ve daha kuzeyinde, ırak ve daha güneyinde meteorological mevsim tarihleri kullanılmaktadır. yani mevsimler aynı fakat  başlangıç ve bitiş tarihleri vardır. meteorological mevsimler hava durumuna göre ayarlanmaktadır. oysa astronomical mevsimler dünyanın güneş etrafındaki dönüşüne göre ayarlanmaktadır. 

bazı ülkelerde mevsimler, o ülkelerin resmi dil kurumlarınca daha fazla sayıya bölünmüştür.

# Gregorian calendar (yada miladi takvim yada tr:Gregoryen takvimi)

milat: İsa'nın doğduğu gün'dür. bugünü başlangıç yılı ve 1 yılı 365 gün kabul eden takvimdir.

# Hicri takvim (yada Müslüman takvimi yada İslami takvim yada Hijri calendar)
Muhammed'in Mekke'den Medine'ye yolculuğunu başlangıç yılı (1. yıl) kabul eden ve 1 yılı 354 ya da 355 gün olan takvimdir.

# takvim
tüm zaman kavramlarını ele alan zaman sistemidir: gün, saat, yılın kaç ay olduğu, ay isimleri ve hangi ayın kaç gün olduğu, mevsim...

Gregorian takvimi en çok kullanılan takvim. fakat bazı ülkelerde hala diğer takvimler %50'ye yakın oranla kullanılmaktadır (istatisitk tarihi: 2017). örnek:

- Hicri takvim - orta doğu ülkeleri

- Hebrew calendar (yada Jewish calendar yada İbrani takvimi yada Yahudi takvimi yada Musevi takvimi) - israil

- Etiyopya takvimi (yada Ethiopian calendar) - Etiyopya ülkesinde kullnılan takvim

- Buddhist calendar - Thailand, Sri Lanka (ve onlara yakın bazı ülkeler)

- Hindu calendar (Indian national calendar yada Shalivahana Shaka calendar yada Saka calendar) - hindistan

- Hint takvimi - sadece eskiden Hinduizm'de kullanılan bir takvimdir.

- Jülyen takvimi (yada Julian calendar) - batı tarafından Miladi takvim'in kabulüne kadar (1582 yılı) kullanılan takvimdir.

Yukarıdaki takvim sistemleride kendi içlerinde yıllar içinde değişime uğramıştır.

# yılbaşı (yada new year's day)

her takvimde yılbaşı farklı gün olabilir. isa'nın doğduğu günün kesin olmaması sebebi ile, Jülyen takvimini kullanan ülkeler farklı tarihlerde isa'nın doğumunu kutluyordu. bazıları 25 aralığı doğduğu gün olarak sayıyor, ve 25 aralığı temsilen yılbaşı olarak sayıyorlardı. bazı bölgelerde ise bu 1 ocak'tı. daha sonra Gregorian takvimine geçildiğinde isanın doğuşu 25 aralık (noel), 1 ocak ise yılbaşı olarak belirlendi. 

# resmi tatil (yada public holiday yada legal holiday)

milli bayram (yada national holiday) yada din bayramları, başlıktakilerin birer altkümesi olarak ayrılır. başlıktaki terimler sadece kurumların kapalı olduğunu belirten terimlerdir.

programatik olarak bu bilgileri her ülke için elde etmek zordur. her ülkenin resmi tatilleri yaşanan olaylar sebebi ile değişebilmektedir. "Administrative division (yada idari bölünme)" başlığına benzer bir durum söz konusudur.

# Bank holiday

bu terim sadece ingiltere ve birkaç ülkede kullanılan bir terimdir. her ülkede ise farklı anlama gelmektedir. örneğin; ingiltere'de resmi tatilleri kapsarken, irlanda'da sadece banka'ların tatil olduğu günleri temsil eder.

# time vs date vs datetime

- time: saat/dakika/saniye gibi bilgileri içeriyor. - date: günün yıl içerisindeki yerini tutuyor
- datetime: hem date hemde time'ı içerir.

# java.sql.Date

java.util.Date'ten extend etmiştir. sql.Date time bilgisini içermez sadece date içerir.

# java.sql.Time 

java.util.Date'ten extend etmiştir. date bilgisi içermez fakat time içerir.

# java.sql.Timestamp

java.util.Date'ten extend etmiştir. java.util.date'e ekstradan nanosecond 'da içerir. java.util.date en ufak birimi millisecond'dır.

# java.util.Calendar

java.util.Date'e alternatif java 7 ile gelen takvim sınıfıdır. java.util.Calendar abstractır bu sebeple direk kullanılamaz. GregorianCalendar ise onu extends etmiştir.

# java.time.*

java.util.Calendar'e alternatif java 8 ile gelen takvim paketidir. java.util.date ve java.util.calendar geriye uyumluluk için hala java içerisinden kaldırılmamıştır.

# Joda Time

java için date ve calendar'ın eksiklikleri sebebi ile geliştirilen kütüphane. java 8 ile gelen java.time.# joda'nın forkudur. java 8 çıktığında joda projesi artık resmi olarak gereksiz olmuş ve geliştirilmeme kararı alınmıştır.

# java.time.*

__LocalDate__ sadece gün ay yıl tutuyor.

```java
// init metodları
LocalDate localDate = LocalDate.now();
LocalDate.of(2015, 02, 20);
LocalDate.parse("2015-02-20");

// diğer metodlar
LocalDate tomorrow = LocalDate.now().plusDays(1);

// ChronoUnit, java.time.* içindeki özel bir obje
LocalDate previousMonthSameDay = LocalDate.now().minus(1, ChronoUnit.MONTHS);

// Period, java.time.* içindeki özel bir obje.
// Period date birimleri ile ilgilenirken, Duration sınıfı time birimleri ile ilgilenir.
LocalDate finalDate = initialDate.plus(Period.ofDays(5));


DayOfWeek sunday = LocalDate.parse("2016-06-12").getDayOfWeek();

int twelve = LocalDate.parse("2016-06-12").getDayOfMonth();

boolean notBefore = LocalDate.parse("2016-06-12")
  .isBefore(LocalDate.parse("2016-06-11"));
```

__LocalTime__ sadece saat, dakika, saniye ve ek olarak nano seviyesinde bilgi tutuyor.

```java
// init metodları
LocalTime now = LocalTime.now();
LocalTime sixThirty = LocalTime.parse("06:30");

//diğer metodlar
LocalTime sevenThirty = LocalTime.parse("06:30").plus(1, ChronoUnit.HOURS);

LocalTime localtime = LocalTime.parse("00:01:02.5");
//prints: "seconds: 2 nano seconds: 500000000"
System.out.println("seconds: " + localtime.getSecond() + " nano seconds: " + localtime.getNano() );
```

Yukarıdaki kod'da "nano" tüm zamanın nano seviyesindeki karşılığını döndürmüyor.

__LocalDateTime__ date ve time'ı birlikte tutar. LocalTime ve LocalDate ile aynı isimde metodlar sunar.

__ZonedDateTime__; LocalDateTime'e ek olarak time zone bilgisini de tutar. zone bilgisi bölgesel bir bilgidir. Yani; kanunlar gereği bir conrafi bölgedeki saat dilimi değiştirildiğinde, ZonedDateTime değeri de otomatik değişecektir.

__OffsetDateTime__ ZonedDateTime ile aynıdır. tek farkı zone bilgisi değil, sadece offset bilgisi tutar. bu sebeple;kanunlar gereği bir coğrafi bölgedeki saat dilimi değiştirildiğinde OffsetDateTime sınıfının değeri etkilenmez. çünkü OffsetDateTime sadece "+03:00" gibi offset değeri tutar. oysa ZonedDateTime "Asia/Turkey" gibi bir değer tutar.

```java
ZoneId zoneId = ZoneId.of("Europe/Paris");
Set<String> allZoneIds = ZoneId.getAvailableZoneIds();

ZonedDateTime zz = ZonedDateTime.parse("2015-05-03T10:15:30.5+01:00[Europe/Paris]");
ZonedDateTime zonedDateTime = ZonedDateTime.of(localDateTime, zoneId); // LocalDateTime objesine timezone ekler.
```

__ZonedDateTime__ parse ve toString metodlarında ISO 8601 formatını kullanır. tek istisna opsiyonel olarak timezone'un isim olarak \[Europe/Paris] şekilde yazılabilmesidir.

# javax.xml.datatype.XMLGregorianCalendar

jre içinde bulunan bir interface. implementasonu da burda: 

> com.sun.org.apache.xerces.internal.jaxp.datatype.XMLGregorianCalendarImpl

W3C'ün XML şemalarında birçok farklı tip vardır (base64Binary, boolean..). bunların arasında zaman ile ilgili data tipleri de vardır. örnek:

gYearMonth, gYear, dateTime, time, date...

XML üzerinde örneklersek:

```xml
<myxlmroot>

 <TestgYearMonth>1999-02</TestgYearMonth>

 <testAnyelement>1</testAnyelement>

 <TestdateTime>2002-10-10T12:00:00-05:00</TestdateTime>

</myxlmroot>
```

Yukarıda 1999-02 stringinini hangi formata olacağı W3C tarafından belirlenmiştir. Eğer xsd dosyası bu xml elementinin bir W3C XML şeması olduğunu gösteriyorsa o zaman Java'daki XML to object parser'lar bunu Javadaki XMLGregorianCalendarImpl'a çeviririyor. Yukarıdaki xml şu java sınıfına döndürülüyor:

```java
public class Myxlmroot {

private XMLGregorianCalendar testgYearMonth;

private int testAnyelement;

private XMLGregorianCalendar testdateTime;

//getter setters

}
```

XMLGregorianCalendarImpl gYear, gYearMonth gibi tüm değerleri tek bir sınıftan okuyabilmemizi sağlıyor. getXMLSchemaType() metodu bize XMLGregorianCalendar'in hangi şemadan (gYearMonth, time)  xml'e çevrildiğini döndürüyor.

XMLGregorianCalendar kendi içinde date time işlemleri yapmıyor. zaten gün ekleme çıkarma gibi işlem yaptıran public metodlar sunmuyor. sadece set/get, birçok constructor ve birkaç utility metodu mevcut. XMLGregorianCalendar kendi içinde GregorianCalendar'ı kullanıyor (fakat ondan extend etmemiş).

# ISO 8601

date time format standardıdır. formatta esneklikler vardır. bazı değerler gösterilebilir yada gösterilmeyebilir.

- Gregorian calendar veya UTC kullanılır.

- TZD, time zone designator'ın kısaltmasıdır ve formatta +hh:mm seklinde gösterilir. sadece Z kullanılırsa +00:00 anlamına gelir. (Z harfi "Zulu time"'dan gelmektedir. Zulu kelimesi NATO'nun kendi içinde Z karakteri için kullandığı bir kodlamadır)

- T işareti ise date'in bittiği ve time'a geçilen kısma atılır.

- DD ve diğerleri her zaman çift hane olmak zorunda. 01 yerine 1 yazılamaz.

- AM/PM ayrımı yoktur. 24 saat özelliği kullanılır.

- örnekler;

  - YYYY-MM --> 1997-07

  - YYYY-MM-DDThh:mm:ssTZD --> 1997-07-16T19:20:30+01:00

  - YYYY-MM-DDThh:mm:ss.sTZD --> 1997-07-16T19:20:30.45+01:00

- mm ve MM ayırt edilebilmesi için küçük büyük olup olmadıkları önemlidir.

- eğer 's' karakteri var ise; en az 1 karakter olmak zorundadır. maximum kaç karakter olacağı standartlarda belirtilmemiştir. aslında 's' millisecond değildir. bu kısım yazılımcının kendisine kalmıştır.

- ISO 8601'de haftalar pazartesi ile başlar. senenin başında hafta ancak pazartesi ise başlar. yani 1inci hafta ilk pazartesi olan haftadır. örnek 1 ocak çarşamba günü ise; 1 ocak hala eski yılın son haftasıdır.

  haftalar da output olarak basılabilir. örnek: 2018-W08 Format: YYYY-Www

# SimpleDateFormatter

Java içinde gelen SimpleDateFormatter'ın kendine özgü formatı vardır (global anlamda bir standart değildir). 

- YYYY-MM-DDThh:mm:ss kabul etmez. T harfini ' tırnak içinde ister: YYYY-MM-DD'T'hh:mm:ss

- YYYY'yi büyük harf olduğundan kabul tanımamaktadır. küçük harf olması şarttır.

- MMMMM ay'ın kelime karşılığını yazıyor: Mart, July gibi.

- a, AM PM 'i ekrana basıyor.  

- küçük s second, büyük S millisecond anlamına geliyor. ss hep iki karakter olmak zorunda iken, SSS 3 karakter olmak zorundadır.

- yy yılın son 2 karakterini ekrana basacaktır. örneğin yıl 2012 ise; 12 basacaktır. 

- yyyyy; yıl 2012 ise ekrana 02012 basacaktır.

daha fazla detay: https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html

# millisecond (yada ms)

saniyenin 1000'de biri.

# salise

Saniyenin 1/60'ıdır. bu terimin ingilizce karşılığı yok.

# AM ve PM

Kısaltmalar Latince kelimelerden geliyor.

AM/PM sistemi, 24 saatlik sisteme(formata) alternatif olarak kullanılır.

AM = A.M = Ante meridiem = Before noon = before midday = öğleden önce = ö.ö. = gece yarısı(00:00) ve öğlene kadar(12:00) olan süre

PM = P.M = Post meridiem = After noon = after midday = öğleden sonra = ö.s. = öğleden gece yarısına kadar olan süre

# bazı terimlerin ingilizceleri

aşağıdaki terimlerin resmi saat aralıkları yoktur.

- Öğle (yada öğlen yada midday yada noon)

- afternoon (yada ikindi)

- akşam (yada evening)

- gece (yada night)

- gece yarısı (yada midnight)

# 0 yılı öncesi ve sonrası

CE = MS = M.S. = Milattan Sonra = Common Era = Current Era = Christian Era = İsa'dan Sonra = İS = İ.S. = Anno Domini (efendimizin yılı anlamına geliyor) = AD

BCE = MÖ = M.Ö. = Milattan Önce = Before Common Era = İsa'dan Önce = İÖ = İ.Ö. = BC = before Christ

# 00:00:00

time değerleri sıfır olduğunda o günün başlangıcını temsil etmektedir. 00:00 yerine 24:00 da resmi olarak kullanılabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# profiling

bu terim yazılım geliştirme dünyasında, çalışan uygulamayı runtime sırasında monitör etmek (bellek, cpu kullanımı gibi parametreleri) anlamında kullanılmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Telemetri (yada Uzölçüm yada Telemetry)

bir sistem ya da tesisin uzaktan kablo veya kablosuz olarak izlenmesi veya kontrol edilmesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •


# Macromedia
Adobe tarafından satın alınan şirket.

# Adobe photoshop
fixed size grafikler için kullanılan uygulama. Vektör grafik çizilemez.

# Adobe illustrator vs Inkspace
Vektör grafik'ler çizim için tasarlanmış birbirine alternatif uygulamalar.

# Adobe Fireworks
hem bitmap hemde vektörel grafik tasarlamak için geliştirilmiş yazılım. sadece web teknolojileri/uygulama temeası geliştirme gibi işler için tasarlanmıştır. bu sebeple photoshop yada illustrator gibi gelişmiş değildir. daha basit basit ve hızlıdır.

# Adobe AIR
masaüstü için web teknolojileri ile uygulama yazmaya yarayan altyapı. air ile yazılmış uygulamayı run etmek için bilgisyarda "AIR Runtime" yüklü olmalıdır.

# Adobe Shockwave Player
Adobe Director tarafından geliştirilen uygulamalarını oynatan playerdır. web tarayıcısına eklentidir. "Flash Player"'dan tamamen bağımsız ve ona alternatif bir teknolojidir. flash temelde video oynatmak için tasarlanmıştır. oysa "Shockwave Player" web uygulaması geliştirmek için tasarlanmıştır.

# Macromedia Flash Player (yada yeni adı: Adobe Flash Player)
Macromedia'nın geliştirdiği dönemlerde ismi "Shockwave Flash Player"'dı.

# Adobe Flex (yada yeni adı: Apache flex)
flash uygulaması geliştirmeye yarayan framework.

# Adobe Dreamweaver
web sayfası tasarlamak için geliştirilmiş IDE'dir. HTML, CSS gibi web dilleri ile önyüz geliştirmek için tasarlanmıştır.

# 3ds Max (yada eski adı: 3D Studio yada eski adı: 3D Studio Max)
Autodesk firması tarafından geliştirilen 3d ve 2d grafik geliştirme yazılımı.

# AutoCAD
Autodesk firması tarafından geliştirilen teknik resim çizme yazılımıdır.

# teknik resim
Teknik resim, bir şeyin nasıl çalıştığını veya üretildiğini anlamak üzere yapılan çizim. mühendislik gibi alanlarda özellikle tercih edilir.

# Bilgisayar Destekli Tasarım (yada CAD yada Computer-Aided Design)
digital ortamdan yararlanılarak yapılan teknik resim çizimidir

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# null vs nil
temelde aynı manaya gelselerde bazı programlama dillerinde çok ince farklılıkları olabiliyor.

bazı proramlama dillerinde Nil ve nil dahi birbirinden farklı da olabiliyor. büyük harfle başlayan "Nil" obje hali oluyor.

"nil" kelime anlamı: hiçbir şey, boş
"null" kelime anlamı: değersiz
"void" kelime anlamı: geçersiz

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# GUI design patterns

# responsive

ekran küçülüp büyüdükçe tüm nesneler kendi içinde büyür veya küçülür. ekrana sığmaya çalışır. ekranın boyutu, uç noktalardayken (eşik değerlerinde - aşırı büyük yada aşırı ufak olduğunda) "adaptive" tasarım kullanımına geçilebilir. yani 2 dizayn birlikte kullanılabilir.

# adaptive

sayfada her nesne fix boyuttadır. sayfa büyültüp küçültüldüğü zaman ekrandaki nesnelerin yeri değişmektedir. nesneler kendilerini ekrana sığacak şekilde yerlerini değiştirmektedirler.

# static

pencere içerisinde ekrna küçülsede büyülsede nesnelerin yeri ve büyüklüğü değişmemektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Heroku

Bulut hazır işletim sistemidir. Amaç; deploymentların hızlı şekilde yapılabilmesi için platform sağlamaktadır. Kodların derlenipr otomatik olarak derlenip deploy edilmesi, dışarıya açılması, makinelerin dışardan hazır API ile yönetilebilmesi gibi birçok ihtiyacı karşılayabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# robots.txt

dosyaya erişim domainin url'sinin root'undan olmalıdır. bu dosyada yazan adresler arama motoroları tarafından indexlenmemektedir. fakat buna uymayan robot'lar da mevcuttur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# QR (yada Quick Response) Code

2 boyutlu bir resimde kare kare siyah ve beyaz noktalar mevcut. BUnların her biri birer ikiliğe (byte) denk geliyor. Bu değerler resimden okunduğu (tarandığı) zaman, sonucunda uzunca bir string elde edilebiliyor. bu string değeri ister bir uel olabilir, ister bir metin olabilir...

# Barkod (yada Barcode)

Piyasada QR Code gibi birçok alternatif mevcut. tüm bu alternatiflere genel olarak barcode adı verilir. bu alternatiflerin bazı farkları:

- mesajın maximum boyutu (daha çok byte olabiliyor)

- okunana verilerin belli formatta okunması gerekiyor. yoksa anlam yüklenemez. örneğin resim mi, url mi, yoksa sadece text mi? burada desteklenen format tipleri.

- okunacak verinin şekli değişebiliyor (qr code 2 boyutlu matrix olması gibi)

# boyutuna göre barcode çeşitleri:

| 1D product | 1D industrial | 2D           |
|------------|---------------|--------------|
| UPC-A      | Code 39       | QR Code      |
| UPC-E      | Code 93       | Data Matrix  |
| EAN-8      | Code 128      | Aztec        |
| EAN-13     | Codabar       | PDF 417      |
|            | ITF           | MaxiCode     |
|            |               | RSS-14       |
|            |               | RSS-Expanded |

# ZXing ("Zebra Crossing")
açık kaynaklı java barcode okuma kütüphanesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Unix-like'ta herşey dosya

unix tabanlı sistemlerde tüm kaynaklar dosyalama mantığı ile çalışmaktadır. bu dosyalar UNIX-like'larda "özel dosyalar" olarak adlandırılırlar. komut satırından bir dizindeki dosyaları listelediğimizde, bir dosyanın özel olup olmadığını; yetki satırındaki ilk harfinden anlayabiliriz. birden fazla dosya tipi vardır:

- regular file : dosya sistemindeki normal özelliksiz bir dosya

- dizin : dosya sistemindeki klasör/dizin

- Symbolic link (yada symlink yada soft link) : windowstaki kısayol

- Named pipe : Inter-process iletişimi için gerekli boru hatları dosyaları

- Socket : Inter-process iletişimi soketleri. network soketi ile karıştırılmamalıdır. bazı kaynaklarda "UNIX domain socket"'i diye de geçmektedir.

- device : driver dosyası

- Door : sadece Solaris işletim sisteminde kullanılan IPC için gerekli bir dosya

# dosya vs API

unix tabanlı olmayan sistemlerde (örnek windows) her kaynağa işletim sistemin sunduğu API'ler aracılığı ile erişilebilir. Örneğin; bir stream'e veri atarken windows'ta MyAPIClass.writeValue("new value"); diye yazılırken, Linuxta herhangi bir outputstream ile ilgili dosyaya byte veri yollamak yeterlidir.

Stream olmasının en temel yararı stream kütüphanleri her dilde varolduğundan her dilden API ihtiyacı olmadan işlemleri gerçekleştirebilmemizi sağlar.

örneğin; linuxta komut satırında "command > output.txt" komutu command'ı çalıştırır ve komut satırı çıktısını output.txt'ye aktarır. unix sistemlerde 3 özel dosya vardır. şu işlevi görür : 

- "firefox > /dev/null" çalıştırırsan çıktılar hiçbir yere aktarılmaz. her yazım işlemi sonrası EOF döndürülür.

- /dev/random sürekli okumak için tasarlanmıştırç. sürekli random sayı döndürür.

- /dev/zero sürekli okumak için tasarlanmıştır. ASCII'deki 0x00 (null) değerini döndürür.

# maximum açık dosya limiti

*nix'lerde açık dosya sayısının bir limiti vardır.

bu limit root yetkisi var ise arttırılabilir. sadece özel olarak bir process için dosya sayısı limitlenebilir, aynı şekilde sadece user yada tüm OS bazında limitler belirlenebilir.

böyle bir limitin olmasının sebebi *nix'lerde herşeyin dosya olmasıdır: soket, process herşey bir dosya. dolayısı ile bir program kendisi çok bellekten harcarsa (örneğin durmadan integer array'ler oluşturursa) memory doldu hatası alacaktır. yani bu işletim sistemini etkilemez. fakat program durmadan soket açarsa, process açarsa, OS bunlara karşılık birer dosya oluşturur. bu durumda dosya sistemi bu durumdan etkilenecektir. dosya sistemi memory'd eolmadığı için ve fiziksel olarak bilgiler diskte kaydolacağı için bu durum bilgisayarı çökertebilir. bunu engellemek amaçlı dosya sayısı limitlenmektedir.

# fork-bomb
sürekli olarak process açıp sistemi sıkıntıya sokan programlardır. sürekli dosya açılması sıkıntı yaratır.

# file descriptor (yada dosya betimleyicisi)

her dosya kullanımında process içerisinde bu dosyanın adresi (programlama dilindeki obje/nesne) tutulur. bu adres file descriptor'tur. C dilinde bu bir integer'dır. Dosyalar sanal olarak  /prod/pid/fd (/prod/process-id/file-descriptor) gibi dosyalarda tutulur. örneğin; /prod/1001/48 dosyası bir doman soketine yada bir sürücüy denk geliyor olabilir. bu dosyalar gerçekten de o dizinde yaratılırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# StackExchange
çeşitli konularda soru-cevap sitelerini bir araya getiren site ağı. StackExchange, aralarında Matematik, İstatistik, Din, Astronomi, Bilgisayar programcılığı gibi belirli konularda özelleşmiş 130'un üzerinde soru-cevap sitelerini barındırmaktadır.
- bilgisayar proramcılığı (yazılım geliştirme) ile olan site Stack Overflow'dur.
- bilgisar kavramları ile ilgili tüm soruların olduğu site: http://superuser.com/'dur.
- ubuntu hakkındaki soruların sitesi: http://askubuntu.com/
- network/sistem soruların sitesi "Server Fault"'tır.
- StackExchange ile ilgili sorular: "örnek nerede bu soruyu sorabilirim", "neden konulara cevap yazamıyorum"... meta.stackexchange.com'da soruluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# PMP (yada Project Management Professional)
bu aynı zamanda bir sertifikasyondur. proje yönetimi konusunda en yaygın kabul gören sertifika özelliğine sahiptir. bu sertifika, merkezi abd'de bulunan proje yönetimi enstitüsü (yada project management institute yada PMI) tarafından verilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Cocoa vs Cocoa Touch
Cocoa Apple altyapılı ürünler için GUI ve diğer uygulamaların çalışması için kütüphane kümesidir. Cocoa Touch, kısmen Cocoa modülleri içeren ve sadece tv, saat, telefon gibi ürünler için gerekli kütüphaneleri içerir.

# CocoaPods
xcode projelerinde third party kütüphanelerin yönetimini sağlayan paket yöneticisidir. javadaki maven gibi. xcode projesi ilk açıldığında pod desteği olmaz. pod desteğini proje dizinine giderek "pod init" komutu ile veririr. daha sonra projenin içinde "podfile" isminde bir dosya oluşur. buranın için kütüphanelerimizi ekleriz. her ekleme sonrası komut satırından "pod install" komutunu çalıştırmak durumundayız.
pod yapısı, aynı xcode workspace'inin içine "Pods" isminde bir proje daha oluşturur. bu projede tüm third party kütüphaneler mevcuttur. asıl projemiz sadece pod projesine depened eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Breadcrumb
Bir uygulamada (web/desktop) kullanıcı bir ekrandan diğerine yönlnedirilmektedir. bazen içiçe sayfalar çok sayıda olabilmektedir. böyle durumlar için syafada nerede olduğunu gösteren ağaç-tarzı yapılara breadcrumb denilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Kurumsal kaynak planlaması (yada işletme kaynak planlaması yada Enterprise Resource Planning yada ERP)
işletmelerde mal ve hizmet üretimi için gereken işgücü, makine, malzeme gibi kaynakların verimli bir şekilde kullanılmasını sağlayan bütünleşik yönetim sistemlerine verilen genel addır.

ERP'nin alt modülleri:
- __CRM (yada customer relationship management)__: Satış programı, Pazarlama programı, Müşteri servisleri programı, teknik destek programı gibi görevleri içeren yazılımdır.
- İnsan kaynakları modülü
- muhasebe modülü
- ...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Standard streams

In computer programming, standard streams are preconnected input and output communication channels between a computer program and its environment when it begins execution. The three I/O connections are called standard input (stdin), standard output (stdout) and standard error (stderr). Originally I/O happened via a physically connected system console (input via keyboard, output via monitor), but standard streams abstract this. When a command is executed via an interactive shell, the streams are typically connected to the text terminal on which the shell is running, but can be changed with redirection, e.g. via a pipeline.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Luna

SafeNet firması tarafından geliştirilen, HSM (Hardware security Module)'dür. Yazılımsal API'si aracılığı ile network üzerinden aldığı bilgileri şirreleyip açabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jmeter

web servislerini test etmek (yük testi) için gerekli yazılım.

# JavaMelody

Java uygulamalarını (sunucu üzerinde olsada) runtime sırasında monitör etmek için kullanılan yazılım. java kütüphanesi olarak, incelenecek/run edilecek projenin içine eklenmelidir. Çlaışn bu kütüphane runtime sırasında bir port açıyor ve browser üzerinden istatistikleri sunuyor. Uygulamaya yapılan http requestler hakkında istatistikler, databaseye yapılan sql'ler hakkında birçok  istatistik mevcut.

# jvisualvm

sadece jdk paketinin içinde bulunan (bin dizininde) görsel bir uygulamana performans monitör etme yazılımıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SonarQube
Kod kalitesini arttırmak/incelemek amaçlı yazılımdır. Bu yazılım comitlenen kodları sürekli olarak incelemeye alıp, kodun durumu hakkında rapor/bilgilendirme yapmaktadır. Bazı algortimalar kod üzerinde tekrarlanan kısımları tesipt edebilir, potansiyel hataları bulabilir (runtime'a ihtiyaç duymadan direk kodu inceleyerek karar verebilir).

"__Sonar__" projenin eski adıdır. __SonarLint__ ise; Eclipse IDE için yazılan eklentiye verilen isimdir.

# jshint vs eslint vs jslint
javascript için potansiyel kod hatalarını ve düzenini sağlamak için kullanılan yazılımlardır.

- IDE'mizde ilgili paketin eklentisi var ise, root dizinimizde ilgili pkaetin config dosyasını ayar ve bu konfiglere göre kodumuzu düzeltir, yada uyarı verir vs...
- komut satırından çağrılarak, hatalar (sonuçlar) çıktı olarak görülebilir.

# eslint-plugin (yada eslint-parser) vs eslint-config
eslint için birden fazla parser (rule) çeşidi yazılabilir. bu parserlar pluginler tarafından yazılır. config'ler ise hangi parserların enable/diable/error/warning olarak gösterileceğini belirler (extend eder). örnek: airbnb-plugin-eslint, babel-plugin-eslint piyasada sık kullanılan yazılımlardır.

ilgili dizin içerisinde .eslintrc dosyasında configler (parser plugini yazılır, hangi rule'ler disbale edilecek belirtilir gibi) yazılır. IDE'deki ilgili plugin yada çalıştırılan komut satırı uygulaması alt dizinlerde bu kurallara göre hataları bulur.

IDE'lerin genelde kendi formatter'ları oluyor. bu formatter eklentileri, eslint'ten bağımsız çalışıyor. eslint'in kendi formatalama kısayol tuşu oluyor. burada bu durum tartışılmış: https://github.com/Microsoft/vscode-eslint/issues/417 (archive date: 14/11/2019)

Eslint'te hatalar ve formatlama farklı kavramlar. genellikle bu ikisi (bazı yazılımcılar tarafından) bazı durumlar için karıştırılabiliyor. bu sebeple eslint formatter'ı, (hataları gidermeyeceği için) formatlamamış olarak düşünülüyor. oysa eslint formatter'ı zaten hataları gidermez.

# lint (yada linter)
potansiyel kod hatalarını yakalamk ve düzenini sağlamak için kodun kontrol edilmesi/eden tool'a verilen genel isimdir.

# Code coverage
testler yazılırken, belirli metodlar çağrılmaktadır. yani belirli satırlar kodda çağrılmaktadır. code coverage yüzdelik cinsindendir. %100 olan kod'da, her satıra en az bir kere girilmiş demektir.

# static vs dinamik test

static test, asıl uygulamanın kodu yürütülmeden yapılan analizler/testlerdir. örneğin; kodun temiz olup olmaması, derlenip derlenmediği, dökümanlarının yazılıp yaızlmadığı gibi faktörlerin testini kapsar. dinamik testlerde ise; junit ve entegration testleri koşulur.

# entegration test vs unit test vs system test

unit test bir birimin kendi içinde çalışıp çalışılmadığının testidir. örneğin; login işlemi olup olmadığı. unit testlerde mock kütüphaneleri ile diğer sistemler mock'lanır. böylece diğer sistemler çalışmıyor olsa bile unit testler tamamlanır. ğer diğer sistemler mocklanmıyor ise o zaman bu entegration testi olur. diğer sistemlerle entegreli çalışıp çalışmadığı test edilmiş olur.

sistem testi ise tüm sistemin baştan sona testini ifade eder.

# happy path test (yada golden path test yada happy day scenario test)

uygulamanın işleyip işlemediğini test etmek için yapılacak en basit testi ifade eder. örneğin ufak bir update çıkıldı. tüm sistem testi çalıştırılacak zaman olmasın. happy path testi ile login olunur, hızlıca bir alışveriş yapılır ve happy test başarılı olmuştur.

# smoke test

smoke test happy path'in eş anlamlısı değildir, fakat aynı anlama gelmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# .htaccess

apache htpp server'ın kullandıgı bir dosyadır. bu doaya içerisinde basit belirtimler ile hangi dosyaların dışarıya açık, hangilerinin hangi ip'lere açık, 404 durumunda hangi dosyanın response olarak gideceği gibi bir çok özelliği ayarlanabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Endüstri 4.0 (yada 4. Sanayi Devrimi yada Industry 4.0 yada Industrie 4.0 yada the fourth industrial revolution)

Tarih boyunca endüstride birçok devrim yaşanmıştır. sırası ile:

1. su ve buhar gücünün daha verimi kullanan sistemler oluşturulması (1800 yılları)

2. üretim bandı tasarımı ve elektriğin seri üretimde kullanılmaya başlanması

3. mekanik ve elektronik teknolojilerin yerini dijital teknolojiye bırakmasına sebep olan programlanabilir makinelerin kullanılmaya başlanması (1950 yılları)

4. burada amaç; endüstrinin (dikkat: sadece fabrikanın değil) siber-fiziksel (tüm fiziksel sistemlerin) sanal ortamla iletişimde kalarak çalışmasıdır.

   programlanabilir embeeded cihazlar fabrikalarda belli firmalar tarafından dağıtılıp, sadece belirli işler için kullanılabiliyordu. bu sebeple çoğu yazılım özel-tasarım işletim sistemlerinde çalışıyordu. 4.0 ile yazılan yazılımlar daha modüler ve daha farklı amaçlarda kullanılabilmesi planlanıyor. Bunun için işletim sistemi yapılarının ortak olması gerekiyor. Ancak bu şekilde başkası sizin yazılımınıza depend olabilir yada fork edebilir. bu şekilde hem daha hızlı/kolay uygulama geliştirilebilecek, hemde tekrar kullanılabilir modüler uygulamalar artacak. burada işletim sistemli bu cihazlar iot cihazlarına denk gelmektedir. kolay ve hızlı uygulama geliştirme sayesinde ise; big data, yapay zeka gibi kavramlardan daha çok yararlanılabilecek. hatta bu durumdan sadece fabrikalar değil, o endüstriye bağlı internet siteleri etkilenecek. mesela e-ticaret sitesinde ürün stokları fabrikadaki stok sayısından otomatik gösterilecek. e-ticaret sitesindeki kullanıcıların isteğine bağlı renkte ürünler fabrikadan otomatik üretilecek ve teslim edilecek gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# İş zekası (yada Business Intelligence)

endüstride, ham veriyi anlamlı ve kullanışlı bilgiye dönüştüren teoriler, metodolojiler, süreçlerin, mimarilerin ve teknolojilerin bir kümesidir. iş zekası işinde çalışanlar; istatistik, veri madenciliği, iş/süreç analizi, raporlama gibi bir çok farklı şeyden yararlanarak şirket için kararların alınmasında rol oynarlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Daemon (yada Disk and Execution Monitor)
Daemon kelime anlamı: şeytan

Yazılım dünyasında bu terim arkaplanda çalışan uygulamalara verilmektedir. Direk olarka kullanıcı ile iletişimde değildir, fakat arkaplanda komut/request almak için beklemektedir. genelde uygulamanın isminin sonuna "d" harfi koyulur. örneğin; syslogd, işletim sisteminde system log'larını tutan arkaplan uygulamasıdır. sonundaki d harfi deamon'dan gelmektedir. windows'taki servisler birer Daemon örneğidirler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Internet Information Services (yada IIS)

Web sayfalarının yayınlanmasını ve web uygulamalarının çalışmasını sağlayan, istemcilerden HTTP ve FTP üzerinden gelen talepleri Microsoft Windows sunucu tabanlı işletim sistemlerinde karşılayan birimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sanal gerçekçilik (yada Virtual Reality yada VR)

Son kullanıcıyı tümüyle sanal bir ortama aktarıp, orada hareket etmesini sağlayan sistemlerdir.

# Artırılmış gerçeklik (yada Augmented Reality yada AR)

Gerçek fiziksel dünya son kullanıcıya zenginleştirilip yansıtılmasıdır. Örneğin bir gözlük aracılığı ile; son kullanıcı sokakta gezerken aslıdna olmayan bir insanı görebilir. SAdece o insan sanaldır. kalan herşey gerçektir.

# Mixed Reality (yada Karma Gerçeklik yada MR)

AR içide kullanılan sanal objeler gerçek dünyadaki objeler ile daha uyumludur ve onlarla etkileşimdedir. örneğin bir buzdolabı kapağı açıldığında içinden meyve dökülmesini MR ile yapabilirsiniz. fakat AR buna müsait değildir. AR direk sanal objeleri belli bir konuma yerleştirir. onlara son kullanıcı dokunarak temas eder. fakat MR, gerçek dünyadaki objelerin tipini, konumunu da algılayıp sanal nesneler ile etkileşimli olmasını sağlar.

# Holografi

Uzayda bir cismin varlığına ait bilgi bize genellikle ses veya ışık dalgaları halinde ulaşır. Holografi, cisimlerden gelen dal­galardaki bilgileri belirli bir şekilde depo edip, bu bilgide hiçbir kayıp olmadan tekrar ortaya çıkartmayı sağlayan bir tekniktir.

# Piyasadaki cihazlar

Microsoft - Hololens

Oculus/Facebook - Rift

Samsung - Gear VR

Google - Daydream

Sony - PlayStation VR

HTC/Valve - Vive

# Kinect

Microsoftun donanım cihazı. ufak bir yerde sabit duruyor. karşısındaki kişinin vucut yapısını uzaktan algılıyor. kullanıcının kıpırdamalarını (el, ayak vs) uzaktan algılıyor. bu şekilde oyun oynamayı sağlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# GNU Tasarısı

Richard Stallman tarafından fikir olarak ortaya atılmış bir akımdır. bu tasarı temel olarak özgür yazılım felsefelerini içerir. yazılımlar "özgür yazılım" olabilmesi için şart olan kurallardır.

- 0 numaralı özgürlük: Herhangi bir amaç için yazılımı çalıştırma özgürlüğü

- 1 numaralı özgürlük: Her ne istiyorsanız onu yaptırmak için programın nasıl çalıştığını öğrenmek ve onu değiştirme özgürlüğü

- 2 numaralı özgürlük: Kopyaları dağıtma özgürlüğü. Böylece komşunuza yardım edebilirsiniz.

- 3 numaralı özgürlük: Tüm toplumun yarar sağlayabileceği şekilde programı geliştirme ve geliştirdiklerinizi (ve genel olarak değiştirilmiş sürümlerini) yayınlama özgürlüğü.

# libre
free kelimesi ingilizcede hem ücretsiz hemde özgür anlamına gelmektedir. ücretsiz yazılım ile özgür yazılım arasında çok büyük farklar var. bu iki terimi ayırt etmek için özgür terimi yerine ispanyolcada özgür anlamına gelen "libre" terimi kullanılmaktadır.

# FOSS (yada F/OSS yada Free and open-source software) vs Free/Libre and Open Source Software (yada FLOSS yada F/LOSS)

sadece bir sınıflandırmadır. lisans değillerdir.

FOSS ve FLOSS arasındaki fark "libre" başlığında anlatılmıştır.

# GNU yazılımları

"GNU Tasarısı" nı baz alarak geliştirilen yazılımlardır.

# GNU

GNU; "GNU Tasarısı" nı baz alarak geliştirilen bir işletim sistemidir. İsmi çok yerde yanlış anlamda kullnılmaktadır. İsminin açılımı "GNU's Not Unix" (GNU Unix değildir) dir. Bu ismi almasındaki sebep de tasarımının Unix'e benzerken kendisinin özgür yazılım olması ve herhangi bir UNIX kodunu içermemesidir. Linux çıkana kadar geliştirilen işletim sistemi stabil bir sürüm çıkaramadı. Liuxun doğuşu ile geliştirilmesi durduruldu.

# GNU/Linux

Bu ibarenin kullanılmasının sebebi; Linux'un bünyeside GNU araçlarını barındırmasıdır.

# Copyleft (yada Telif feragatı)

Telif haklarının belirli bölümlerinden yazarı tarafından belirtilen şartlar altında feragat edilmiş olan bir esere işaret eder.

- işareti; 🄯 (daire içinde sola dönük "C" harfi)

# Copyright (yada Kopyalama hakkı yada telif hakkı yada röyalti yada royalty)

telif kelime anlamı: eseri yaratan kişinin, bu eserden doğan haklarının hepsi.

- bir kişi ya da kişilerin her türlü fikri emeği ile meydana getirdiği bilgi, düşünce, sanat eseri ve ürününün kullanılması ve kopyalanması ile ilgili hukuken sağlanan haklardır.

- copyleft'in zıttıdır.

- işareti; © (daire içinde "C" harfi)

# trademark (yada trade mark yada trade-mark yada tr:Alametifarika yada marka yada brand)
baskı yolu ile çoğaltılabilecek her türlü simge, isim, çizim gibi her türlü ifadelerdir.

™ yada (TM) işareti kullanılır. yasal olarak "registred" olursa ® yada (R) işareti kullanılır.

# tescilli (yada Registered)
tescil; Herhangi bir şeyi resmi olarak kaydetme, kütüğe geçirme'dir.

tescilsiz markalarda ticaret Kanunlarının "Haksız Rekabet" hükümlerince yinede korunur. fakat tescilli markaların korunmaları daha önceliklidir.

tescilli markalar, türkiyede zorunlu olunmasada, ® yada (R) işaretini markanın yanına eklemektedirler. bu işaret tüm dünyada kullanılmaktadır.

Ses veya melodi'ler baskı yolu ile çoğaltılamayacağı için tescil alınamayacağı düşünülebilir, fakat melodinin ve sözlerin notaya dökülmüş halinin patenti alınabileceğinden

# patent
ingilizce'de de aynıdır.

patent belli bir özellik için alınır. örneğin marka'nın patent'i alınamaz. bir markanın ancak tescili yapılabilir. markanın tescili türkiye'de "Türk Patent Enstitüsü"'ne yapılır. enstitüdeki patent ismi birçok kişide karışıklık yaratıyor. 

# freeware
ware kelime anlamı: mal

resmi olarak kullanılan bir terim değildir. ücretsiz kullanılabilen veya ücretsiz dağıtılabilen uygulamaların lisanslarının olduğu gruptur.

# shareware
belirtilen süre içinde (deneme süresi) kulanımı ücretsiz veya kısıtlı özelliklerle kulanımı ücretsiz olan uygulamaların lisanslarının olduğu gruptur.

# Digital Millennium Copyright Act (yada DMCA)
Amerika'nın digital hakları korumak için onayladığı yasanın özel ismidir. Aynı isimde develete bağlı olmayan bir kuruluş vardır (dmca.com).

# digital rights management (yada DRM)
Digital hakları koruyabilmek için kullanılan teknolojilere verilen genel isimdir. örneğin apple'ın sunduğu müzik servisinden indirilen müzikler kopyalanamıyor. bunu engellemek için apple'ın çalıştırdığı mekanizma DRM görevi görmektedir.

# Creative Commons (yada CC)

Hem firma, hemde bu firmanın yarattığı lisans'ların prefix'leridir. CC firmasının birçok lisansı vardır. bazıları:

- Creative Commons Attribution (yada CC-BY)
- Creative Commons Attribution-ShareAlike (yada CC-BY-SA)
- Creative Commons Attribution-NoDerivs (yada CC-BY-ND)

# EULA (yada End User License Agreement yada Software license agreement)

yazılım lisanslarına verilen genel isim.

# Piyasada kullanılan bazı lisanslar

detaylı karşılaştırma: 
https://choosealicense.com/appendix/

en çok kullanılan lisansların karşılaştırması:
https://choosealicense.com/licenses/

Yukarıdaki listedeki bazı özellikler:
- Commercial use
  
  Yazılımın ticari kullanıma açık olup olmadığı

- Distribution

  Yazılımın dağıtılıp dağıtılmayacağı. ticari olarak diil, kişinin tek olarak başkasına verebilmesi de bu durumu kapsar.

- Modification

  Yazılım üzerinde modifikasyon yapılıp yapılamayacağı iznidir

- Patent use

  Katkıda bulunanlarda hak sahibi (paten alabilme hakkı) olup olmadığını belirtir

- Disclose source

  Eğer modifiye edilmiş kodun derlenmiş hali piyasaya sürülürse, kodun açılması gerekir.

- License and copyright notice

  Projedeki lisans dosyası yeni projenin içerisinde bir yere konulmalıdır.

- Network use is distribution

  web tarayıcısı yada web servisi gibi hizmetlerden kodun derlenmiş hali piyasaya servis olarak açılırsa, kodun tamamamı da dağıtılmalıdır.

- Same license

  Yeni kod yine aynı lisans ile dağıtılmalıdır. (choosealicense.com çok detaya inmemiş. karşılaştırma tablosunda, bazı lisanslarda bu koşul işaretlenmiş fakat bazı durumlarda benzer lisanslarla da fork'a izin verilebilmektedir. mutlaka aynı lisans istemeyen lisanslarda var.)

- State changes

  Changes made to the code must be documented.

Yukarıdaki choosealicense.com karşılaştırma listelerinden bazı notlar:

- GNU AGPL (yada Affero GPL) vs GNU Genel Kamu Lisansı (yada GPL yada GNU GPL yada General Public Licence)
  
  ikisinin 3'üncü sürümleri ile ilgili not: 

  Tek farkı; AGPLv3 web'den sunulan bir hizmet olsada kodların tamamı açılmak zorundadır. Oysa GPLv3de network'ten açılan servislerde bu zorunluluk yoktur. fakat her iki lisans'ta da diğer tüm durumlarda kodun tamamı açılmalıdır.

- LGPL (Kısıtlı GPL yada Lesser GPL)

  GPL ile çok benzerdir. Tek fark: GPL'e ek olarak şu özgürlüğü sunar: LGPL ile patentlenen yazılım, istenirse özgür olmayan yazılımlar tarafından kullanılabilir. bu lisans genelde formatlarda ve eklentilerde kullanılmaktadır.

- MIT Lisansı

  "özgür yazılım" tanımına uyar. ve türetip kullanılan kodun ne yapılması gerektiği konusunda hiçbir kısıtlama sunmaz. istenirse yeni kod ticari kullanılabilir, yada tamamen kapatılabilir.

- BSD

  MIT Lisansı ile aynıdır. ek olarak şu madde vardır: kaynağı alınan kod referansı bir yerlerde belirtilmelidir.

- Apache License 2.0

  BSD ile çok benzerdir.

# farklı projeler için farklı lisanslar
font, data, media için ayrı ayrı uygun lisanslar mevcut. bu sebeple projelerin birden fazla lisansı olabiliyor. her lisans projenin ayrı ayrı kısımları için belirtiliyor.

projenin dökümantasyonu kod ile aynı sınıflandırılabilir. yani yazılım için geçerli lisanslar, dökümantasyon içinde kullanılabilir.

# lisanssız projeler
lisanssız her proje Copyleft'tir. fakat github gibi sitelerden yayınlanan kodlar bazı hakları esnetebilir. Bu sebeple lisansız projeleri kişisel kullanımda dahi kullanmamak gerekir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Statistical Package for the Social Sciences (SPSS)

İstatistik analiz yazılımıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Programming paradigm

paradigm türkçesi: paradigma

paradigma kelime anlamı: 1-örnek, 2-bir şeyin nasıl üretileceği konusunda örnek/model

proramlama dilinin özellikleridir: örneğin cephe yönelimli olması, nesneye yönelimli olması birer paradigmadır.

# multi-paradigm programming language

bazı programlama dillerinin bazı özellikleri birden fazla paradigmayı desteklemektedir.

# programlama dilleri jenerasyonları

- 1inci jenerasyon (1GL): 1 ve 0'lardan oluşan programlama dilleridir. 2inci dünya savaşında Nazi almanyasının şifreli mesajlaşma makinesi olan Enigma'yı çözebilmek için, Alen Turing, Turing makinesi ile 1940'larda ilk programlama mantığı ortaya atılmıştır. Fakat proramlama dilleri olmadığı için manyetik teypler panel aracılığı ile kapatılıp açılıyordu (switch/toogle). bu şekilde sadece 1 ve 0'lar ile işlemler yapılıyordu.

- 2inci jenerasyon (2GL): düşük seviyeli programlama dilleri. assembly. İlk 1950'lerde ortaya çıkmıştır.

- 3inci jenerasyon (3GL): yüksek seviyeli proramlama dilleri. C, C++, C#, Java

- 4inci jenerasyon (4GL): diğer jenerasyondakiler gibi bit ve byte kavramlarını yönetmekle değil, direk olarak verileri bütün olarak kullanabilmektedirler. örneğin tablo üzerinde direk işlem sağlanabilmekte, ve ekrana GUI raporu alınabilmektedir. Örnekler: SPSS, Unix shell, SQL.

- 5inci jenerasyon (5GL): programcının algoritma geliştirerek çözüm geliştirmesinin ötesinde, koşulları ve kısıtları bilgisayara verdiğinizde, bilgisayarın çözümü kendisinin bulmasına yönelik olarak tasarlanmaktadır. Bazı arastırmacılar, bu tarz dillerin jenerasyon grubu altında olamayacağını belirtmekte. çünkü sadece DSL olduğunu düşünmektedirler.

- Bir dil, birkaç jenerasyona birden uyabilir.

- Her jenerasyon ile donanımdan daha bağımsız uygulamalar yazılması sağlanır.

# object-orianted (yada nesneye yönelimli) vs object-based (yada nesne-tabanlı)

javascript nesneye yönelimli değildir. nesne kullanır bu sebeple nesne tabanlıdır. fakat nesneye yönelimli dillerin desteklemesi gerektiği özellikleri destekleyecek şekilde özenle geliştirilmez. örneğin javascriptte inheritance ve polimorfism yoktur. son çıkan javascript sürümlerinde sınıflar gelmiştir. fakat bu seferde encapsulation yoktur.

bu iki kavram piyasada sürekli biribir ile karıştırılmaktadır.

# Yordamsal programlama (yada Procedural programming)

kod tekrarı engellemek amaçlı (goto/jump gibi terimler yazmaya gerek kalmadan) programın fonksiyonlara bölünmesi ile yazılmayı destekleyen programlama dilleridir. yordam kelimesi; hem fonksiyonu hem de metodu kapsamaktadır.

# fonksiyonel programlama (yada functional programming)

fonksiyonel programlama genelde;

- rekursiv işlemlerin yapıldığı,

- pure fonksiyolar ile çalışıldığı

- fonksiyonlar fonksiyonlara parametre olarak geçilebildiği

dillerdir.

Burada "multi-paradigm programming language" baslıgında belirtildiği gibi her dilin birden fazla özelliği desteklediğini hatırlatmak gerekli. AYnı zamanda yordamsal, fonksiyonel, nesneye yönelimli terimleri programlama dilinin bir özelliği değildir. aslında bir yazım tarzıdır. daha farklı bir bakış açısıyla;

- fonksiyonel dil: devide and concur mantığı ile problemlerin çözüldüğü diller

- prosedürel dil: sıralı şekilde birbiri ardına işlem yapılarak bir işin gerçekleştirildiği diller

- nesneye yönelimli dil: gerçek hayattaki nesne yapıları oluşturularak bunlar üzerinde biribirinin aksiyonları çağrılarak yazılan uygulamalardır

tabikide eğer bir dilde nesne yapısı hiç yoksa nesneye yönelimli tarzda uygulama yazmak mümkün olmayacağı için bu terimler sanki dilin bir özelliğiymiş gibi anlatılıyor. oysa diil.

# Olaya dayalı programlama (yada olay güdümlü programlama yada event driven proramming)

GUI, donanım sinyalleri gibi olaylar sonrası aksiyon alan programlama dilleridir. aslında tüm programlama dilleri kütüphaneleri desteklediği sürece event bazlı olabilir. fakat genelde ağırlıklı olarak event'lere dayalı programlar için bu terim kullanılır. örneğin javascript. sadece son kullanıcı sayfada bir yere tıkladımı, yada sayfa yüklediğinde bir aksiyon alınır. onun dışındaki aksiyonlar nadirdir. zaman tetiklemeleri mevcuttur. belirli zamanda event tetiklenir. timeout metodları gibi...

örneğin komut satırı ile işlem yapan script'ler event bazlı değildir.

# method vs function

metod; bir objenin fonksiyonudur.

fonksiyon ise; sınıflardan bağımsızdır.

nesneye yönelimli dillerde, her fonksiyon birer metoddur. nesneye yönelimli olmayan dillerde ise hepsi fonksiyondur. istista olarak nesneye yönelimlilerdeki static metodlar vardır. bunlar sınıfın fonksiyonlarıdır fakat sınıfa bir bağlılıkları yoktur. bu sebeple her zaman tartışma konusu olmuşlardır.

bu tanımların farkı yazılımcılar arasında bilinmediğinden; çoğu zaman yanlış yerlerde kullanılırlar.

# encapsulation (yada kapsülleme)

Bir sınıf içeriğinin, onun üyelerini kullananlar tarafından bilinmesine gerek duymadan sadece metodun verdiği hizmetin gösterilmesi işlemidir. public private keywordleri bunu yapmamızı sağlar.

# aspect oriented programming (yada cephe yönelimli programlama yada AOP)
başka başlıkta bununla iligli şeyler yazıldı.

Bütün programlama yaklaşımlarında kodlar uzadıkça, kodların anlaşılabilirliği çok düşmekte, bazen de içinden çıkılmaz bir hal almaktadır.

aspect felsefesi ile ortak olan kod parçaları bir yerde toplanır. uygulamanın her hangi bir kısmında bu ortak metodları kullanarak kodu kısaltmış oluruz. aspect felsefesi budur. ascpect felsefesi uygularken kütüphane şart değildir. kendimiz elle metodları çağırabilir. fakat %90 kütüphane kullanılır. sebebini bir örnekle açıklayalım: programımız her metod çağırdığında log atsın istiyoruz. bu ortak log metodunu sürekli her metodun başına ekleyebiliriz. fakat bir kütüphane olsa bize bunu otomatik yapar. bu en belirgin ve basit örnektir. eklenecek kütüphanenin metodların başına/sonuna işlem ekleme (method-interceptors) yapabiliyor olması gerekir.

# Behaviour Driven Development (yada BDD)

TDD benzeri fakat X olursa şunu yap gibi terimlerle desteklenen test yazım felsefesidir.

Java'da JBhave frameworku (genelde) tercih edilmektedir.

given/when/then gibi terimler kullanılır. Givenda tanımlamalar yapılır, then de sonuçta beklenen şey yazılır, whende ise ne zaman bu sonucun gerçekleşeceği belirtilir. Bu belirtilmler DLS ile yazıldığından, konuşma dili ile yaızlan cümleller içerir. Bu BDD ‘nin bir avantajıdır.

# Test-driven development (yada TDD)

Önce test sonra kod yazıma gidilerek yapılan kodlama felsefesidir.

# declarative programming

decleration türkçe kelime anlamı: beyan etmek, bildirmek

dekleratif diller işin nasıl yapıldığına karışmaz. sadece ne yapılması istendiğini belirtir. örnek: SQL, CSS, HTML, Java'daki anotation'lar...

# imperative programming

imperative türkçe kelime anlamı: zorunlu, mecbur

declarative'in tersidir. nasıl olacağına tam olarak karışır ve sonucta ne istediği dilin kendisini ilgilendirmez.

# reactive (yada Rx yada tr:reaktif) programming

reactive türkçe kelime anlamı: duyarlı, tepkili.

reactive bir programlama paradigmasıdır.

çoğu yerde fonksiyonel programlama ile karıştırılır fakat farklı kavramlardır. fakat reactive programlamak için fonksiyonel programlama en idealidir.

a = b + c; dediğimizde b ve c toplanır sonucu a'ya atanır. peki b değişirse a değişir mi? normalde hayır. fakat reactive programlamada değişmesi bekleniyor. reactive programlama; data'ların değişimi ve bu değişimin etkileri/yayılımı'na dayanır.

Reactive programlama ile yapılan herşey diğer programlama paradigmaları kullanılarakta yapılır. burada amaç; development sürecine bir yön vermektir. şunlar ağırlıklı kullanılır:

- reactive'de event'lere subscribe işlemi çok sıkça kullanılır.

- data değişimleri de birer event olarak algılanır

- subscribe olurken Listener/handler sınıfı atmak yerine sadece ilgili metod atılır. listener sınıfı komple oluşturulmaz. kod fazlalığı olmaz. listener sınıfı yarattığımızda override etmek zorunda kaldığımız fakat kullanmadığımız metodlarda olur. onları da yazmak durumunda kalmayız.

- event'lerimiz non-blocking thread mekanizmasını destekliyor olur (başka yerde anlatılıyor)

- fonksiyonel dil / lamda / stream gibi teknolojilerden çok sıkça yararlanılır

- bazı listenerlarda hatayı yakalamk için try/catch mekanizması gereklidir. fakat reactivede hatalar onError metodları ile yakalanır.

- diğer paradigmalarda, databaseden veri çekerken tüm veriyi çekeriz. oysa reative'de 10 adet veri çekilir event tetiklenir. daha sonra diğer 10 adet daha çekilir ve event tetiklenir. amaç data geldikçe harekete geçmektir. eskisi gibi aldım yolladım kaydettim bitti mantığı yok. data aktıkça, sistem buna hazırdır (tepkilidir).

sonuc olarak Rx'e uygun yazarken, event başladığında yaptığınız işlemde side effectleri düşünmezsiniz. çünkü zaten her side effect'in subscriber'ı mevcuttur. onlar işlerini yapacaklardır. bir programda binlerce/milyarlarca side effect olduğunu düşünürseniz reactive çok ideal olacaktır. yani kod akışını takip etmek yerine, olayları takip etmeye çalışırız.

# reactive vs event driven

reactive event-drive'ın bir çeşididir. klasik event driven developement bir event gerçekleştiğinde devreye giren kodları barındırır, fakat react'ta event data'nın da değişmesini kapsar. yani data değiştiğinde event tetiklenir. klasik event driven sistemlerde data'nın değişmesi diye bir kavram yoktur.

# reactive system

büyük resme bakıldığında (mimarisel olarak) reactive bir sistem için aşağıdakiler şarttır ve bunlar www.reactivemanifesto.org'da belirtilmiştir:

- Responsive/Duyarlı: sistem her event'e cevap veriyor olmalı ve mümkün olduğunca hızlı cevap vermelidir. timeout gibi hata durumlarında da hata event'leri devreye girmelidir.

- Resilient/dirençli: sistem hatalar karşısında da responsive davranır 

- Elastic/esnek: ölçeklendirilebilir durumda olmalıdır. daha fazla kaynak üzerinde çalıştığında daha hızlı cevap verebilmelidir. 

- Message Driven/mesaj güdümlü: herşey asenkron mesajlaşma tekniği ile gerçekleşmelidir.

reactive programming, reactive sistem'in uygulanabilmesi için gerekli programlama paradigmasıdır.

# Reactive Streams

"Reactive Streams" reactive programlamaya uygun data çekme/işleme politikası için olan sınıf/sınıflardır. örnek: data çekerken tüm data yerine 10'ar 10'ar çekmemizi sağlayan api'ler react yapısına uygundur. dikkat edilirse 10'ar 10'ar çekme işlemi bir stream yapısıdır. bu sebeple "reactive stream" olarak adlandırılır.

Java 9 ile gelen reactive API'sinin ismi de "Reactive Streams"'dir. __java.util.concurrent.Flow.*__ paketi altındaki sınıfları içeriyor.

reactive-stream paketleri 1.0.2 öncesi org.reactivestream.* altındaydı. 1.0.2 ile birlikte jdk'ya dahil edildi ve __java.util.concurrent.Flow.*__ altına taşındı. kaynak: https://www.reactive-streams.org/announce-1.0.2

Aşağıdaki dependency her iki pkaetinde birbirine referans edebilmesini sağlıyor.

```xml
<dependency>
   <groupId>org.reactivestreams</groupId>
   <artifactId>reactive-streams-flow-adapters</artifactId>
  <version>1.0.2</version>
</dependency>
```

```java
public class MySubscriber implements java.util.concurrent.Flow.Subscriber<Employee> {

	private Subscription subscription;
	
	private int counter = 0;
	
	@Override
  //subscribe işlemi gerçekleştiğinde bu metod çalışır.
	public void onSubscribe(java.util.concurrent.Flow.Subscription subscription) {
		System.out.println("Subscribed");
		this.subscription = subscription;
		this.subscription.request(1); //requesting data from publisher
		System.out.println("requested 1 item");
	}

	@Override
	public void onNext(Employee item) {
		System.out.println("Processing Employee "+item);
		counter++;
		this.subscription.request(1);
	}

	@Override
	public void onError(Throwable e) {
		System.out.println("Some error happened");
		e.printStackTrace();
	}

	@Override
	public void onComplete() {
		System.out.println("All Processing Done");
	}

	public int getCounter() {
		return counter;
	}
}
```

```java
// Create Publisher
SubmissionPublisher<Employee> publisher = new SubmissionPublisher<>();

// Register Subscriber
MySubscriber subs = new MySubscriber();
publisher.subscribe(subs);

List<Employee> emps = EmpHelper.getEmps();

// Publish items
System.out.println("Publishing Items to Subscriber");
emps.stream().forEach(i -> publisher.submit(i));

// wait untill processing of all messages are over
while (emps.size() != subs.getCounter()) {
  Thread.sleep(10);
}

publisher.close();
```

# ReactiveX (yada Reactive Extensions)

tüm programlma dilleri için reactive API kütüphanesi oluşturmayı amaçlayan proje. 

# RxJava

ReactiveX java implementasyonu. Aynı zamanda Java 9 ile gelen "Reactive Streams" implementasyonudur.

# Reactor

Java 9 ile gelen "Reactive Streams" implementasyonudur.

Spring 5.0 ile birlikte, __spring-webflux__ paketi ile spring mvc'ye reactor ile reactif programlama desteği getirmiştir.

Reactor; Mono ve Flux objelerini sunar. Bu objeler, java 9 ile gelen 'reactive stream'in Publisher'ını implemente eder.

- # reactor.core.publisher.Flux

  is a publisher that produces 0 to N values. It could be unbounded. Operations that return multiple elements use this type.

- # reactor.core.publisher.Mono

  is a publisher that produces 0 to 1 value. Operations that return a single element use this type.

örnek basit bir kod:

```java
@RestController
public class GreetReactiveController {

    @GetMapping("/greetings")
    public Publisher<Greeting> greetingPublisher() {

        Flux<Greeting> greetingFlux = Flux.<Greeting>generate(sink -> sink.next(new Greeting("Hello"))).take(50);

        return greetingFlux;

    }
}
```

Temel olarak non-blocking olması için akış bu şekilde ilerliyor:

greetingFlux objesi özel bir webflux sınıfı. greetingPublisher metodu neredeyse başladığı gibi bitiyor. fakat greetingFlux objesi onsuccess yada onerror olunca spring webflux altyapısı servlet'ten cevabı önyüze dönüyor. onerror yada onsuccess olması uzun zaman da alabilirdi. fakat biz, greetingPublisher metodunun thread'ini kapatmış oluyoruz. dolayısı ile onerror veya onsuccess ne kadar geç tamamlanırsa tamamlansın, thread'imiz açık kalmamış olacaktır. 

# back-pressure (yada backpressure yada geri tepme yada back pressure)
publisher çok fazla data sağlar ve subscriber bu dataları işlerken yavaş ise, subscriber back-pressure yapmalıdır. yani; subscriber publisher'a bir şekilde daha az data yollamasını iletebilmelidir. bu yeteneğe back-pressure denir.

back-pressure sistemi yavaşlatacaktır fakat sistmein stabil çalışmasını sağlayacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# freedesktop.org (yada X Desktop Group yada XDG)
X window system üzerindeki uygulamalara katkıda bulunan ticari olmayan topluluğun ismidir. Gnome, KDE gibi projelerin büyük destekçilerindendir.

# XDG Base Directory Specification
XDG'nin hangi dizinlerde hangi dataların olacağı standartıdır.

OS üzerinde o anda $HOME user'ı gibi belli standartlar ile dizinler belirlenmiştir. Bu standartlara uyulmak zorunda değildir, fakat standart olduğu için birçok yazlım bu dizinleri tercih eder. örneğin;

$XDG_CONFIG_HOME, çoğunlukla HOME/.config'i işaret eder ve bu dizin sadece ayarları tutar. oysa $XDG_DATA_HOME, çoğunlukla $HOME/.local/share/ i işaret eder ve sadece dataları tutar: örneğin thumbnail gibi. bu standartlar sayesinde kullanıcı ayarlarını bir makinadan diğerine daha rahat taşıyabiliriz.

$XDG_CONFIG_DIRS bir dizin listesi tutar. $XDG_CONFIG_HOME'a ek olarak hangi dizinlerde config dosyalarını bulabileceğimizi tutar. örnek: /etc/xdg:$HOME/xdg

Specification eğer $XDG_CONFIG_HOME gibi değerler boş ise; default değerleri de belirtmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# MATLAB
"matrix laboratory" nin kısaltması olarak isimlendirilmiştir.
MathWorks tarafından geliştirilen sayısal hesaplama/analiz yazılımıdır. MATLAB isimli programlama dili yada diğer programlama dilleri ile çalışılabilir. Kullanıcıyı arayüz sunan ekranlar hazırlanabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DevOps

development ve operations kelimelerinin kısaltmalarının birleşiminden meydana gelmiştir. IT'deki tüm ekiplerin (yazılım geliştirme, destek...) birbiri ile ilişkili şekilde çalışması kültürüdür. Bu kültürün oluşabilmesi için operation kısmındaki birçok işin otomatize edilmesi gerekmektedir. İşte "devops enginneer" burada devreye giriyor.

operation kısmı şu birimlerden oluşmaktadır: sistem mühendisleri, sistem yöneticileri, sürüm mühendisleri, veritabanı yöneticileri (DBAs), network mühendisleri, güvenlik uzmanları vs...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Microsoft Azure (yada eski adı:Windows Azure)

Eski adı işletim sistemi sürümü gibi algılandığı için terminolojik kargaşa yaratabilir.

Microsoft'un bulut bilişim hizmetleri altında birçok hizmet vermektedir. örneğin; Windows kurulu makineler, yada windows üzerinde kurulu SQL Server yüklü hazır makineler sunulmaktadır. Amazon Web Services'a alternatiftir.

Bulut'ta yüklü Windows sürümleri özel bir Windows değildir. normal windowsa göre ufak eklemeler çıkarmalar yapılıyor olabilir. aynı durum Amazon Web Services'te böyledir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# path: canonical vs asolutite and relative

örnekler:

- absolute path: C:\XyzWs\\.\TEST.TXT  -> dosya sistmeinin root'undan alınan fakat noktalama işaretleri ile yönlendirmeler alabilen path

- canonical path: C:\XyzWs\test.txt  -> dosya sistmeinin root'undan alınan fakat noktalama işaretleri ile yönlendirmeler içermeyen path

- relative path: .\TEST.TXT -> dosya sisteminin başından değilde o anda runtime sırasında bulunan dizinden itibaren verilen path'tir.

  relative path için kullanılan işaretler:

  - ./images/image1 --> bulunduğumuz dizin altındaki images klasörü

  - ../images --> bulunduğumuz dizin üstündeki dizindikeri images klasörü

  - ../../images --> bulunduğumuz dizinin 2 üstündeki dizindeki images klasörü

  - ~/images --> user'ın home klasörünün içindeki images klasörünü temsil eder. bu relative path değildir. direk olarak root'tan verilmiş bir dizin olduğundan absolut path'tir. ~ işareti sadece bir kısaltmadır.

# Java File API
path : ..\\.\Java.txt

absolute path : C:\Users\WINDOWS 8\workspace\Demo\\..\\.\Java.txt

canonical path : C:\Users\WINDOWS 8\workspace\Java.txt

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Uniform Resource Name (yada URN)

örnek: 

- ISBN numarası: urn:isbn:096139210

- amazon servislerindeki bir kaynağın tanımı: arn:partition:service:region:account-id:resource

# URL (yada Uniform Resource Locator)

örnek:

- mailto:myemail@servise.com

# Uniform Resource Identifier (yada URI)

bir resource için unique id tanımıdır. URL ve URN bunun birer implementasyonudur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Library (yada kütüphane) vs Framework (yada yazılım iskeleti yada yazılım çerçevesi yada yazılım çatısı)

Library, API'lerini çağırarak işlem yaptığımız ve bazen dönüş aldığımız yazılımlardır. Framework'te ise durum tam tersidir. Framework kendi çalışır ve bizim uygulamamızdaki API'leri çağırır. Bizi kendi akışının bir parçasıymış (modülüymüş gibi) kullanır.

Örneğin; Libary; Log4J, ApacheCommonStringUtils verilebilir. Framework; Web uygulaması sistemi verilebilir: Spring, JavaEE gibi. Spring içerisinde bizim uygulamamız bir modül gibi kalıyor. Onun sınıflarını extend ederek akışlar oluşturuluyor.

# Toolkit vs SDK (Software Development Kit)

Bir kütüphaneyi yada bir framerowrk'ü kullanmak yada ondan yararlanmak için zorunlu/isteğe bağlı olarak kullanılan yazılımlardır. resmi tanım olmadıklarından; ikiside aynı anlama gelmektedir. fakat piyasada genel olarak sdk daha çok; başka bir işletim sistemi yada ortam(platform) üzerinde uygulamayı yürütmek için gerekli olan yazılımlar için kullanılamktadır. toolkit'ler ise SDK içinde yüklü gelebilirler ve daha çok opsiyonel kaynakları barındırırlar.

# yazılım (yada software) vs program vs appllication (yada app yada uygulama)

- program ;blgisayar biliminde makineye işletilmesi için verilen talimatlardır (kodlardır).

- yazılım; proramlar grubudur.

- app; son kullanıcı ile etkileşimde olması şart olan bir yazılım çeşididir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# yorumlayıcı (yada Interpreter)

bir yazılımdır. bu yazılım kaynak kodu direk olarak (yada bir aşama daha atlatılarak paketlenmiş/derlenmiş) halini yorumlar ve yorumladığı şeyleri yürütür. Örneğin; JVM, Javascript motorları birer yorumlayıcıdır.

# betik dili (yada script language yada scripting language)

bir programlama dili grubudur. script'lerin kaynak kodları direk makina koduna çevrilmez. yorumlayıcı koduna çevirilir.

örneğin; C'de makina koduna çevrilmiş bir kod elde ederiz. Bunu çalıştıran işletim sistemidir. Fakat javascript'te kod işletim sistemi tarafından çalıştırılmaz. tarayıcı tarafından çalıştırılır.

Yorumlayıcıya ihtiyaç olan her dil, script dilir.

Komut satırına yazdığımız scriptler (script'ler if blockları içerebilir…) buna en iyi örnektir. Yorumlayıcı olan komut satırı uygulaması kodu farklı  yorumlayabilir. Burada yorumlayanlar aynı zamanda komut satırından çağırdığımız uygulamalarda olabilir.

# bytecode (yada portable code yada p-code)
yorumlayıcı gerektiren dillerde yorumlayıcının okuduğu dosyadır.

# java bytecode
java kodları (.java uzantılı dosyalar) derlenir ve bytecode'a çevrilir. java için bytecode .class dosyalarıdır.
Scala ve Groovy'de Java diline alternatiftir ve derlendiklerinde bytecode'a çevrilirler. Yani JVM'de run edilmek için tasarlanmışlardır.

# Assembley
- En düşük seviyeli dildir.
- Makine kodu; 0 ve 1'lerin okunabilir hale getirilmesi için kullanılır.
- Windows (yada diğer işletim sistemleri) üzerinde çalışan .exe (yada diğer executable) uzantılı dosyalar, hem makine dilinden hemde işletim sisteminin anlayacağı kısımlar içerir. Bu sebeple; C'de yazılmış basit bir satırlık printf satırlık kod, linuxta derlenirse FreeBSD'de, mac'te yada windows'ta çalışmamaktadır. Executable formatı zaten her işletim sisteminini farklıdır.
- Aynı işletim sistemine sahip 2 işletim sistemi olsun: Debian, Suse. Debian üzerinde bir kod derledik. Eğer kod içerisinde Suse'de olmayan bir kütüphaneye link var ise, uygulama run edilir fakat runtime sırasında hata alınır.
- Compiler'ın yaptığı şey: kodları bir dosyaya aktarmak (executable yaratmak). Bu sebeple ms-windows için, linux üzerinde derleme işlemi yapılabilir. örnek bunu yapan compiler: mingw.

# JIT (Just-in-time) compile

ANında derleneme işlemi gelen kodun anında makine koduna (aslında hem işletim sisteminin hemde makine koduna birlikte) çevrilmesi şeklindeki yürütme tipine verilen isimdir. JIT programlama dilinin özelliği değildir, runtime'da kodu işleten programın özelliğidir. JVM, .net, Javascript engin'ler bu yöntemi kullanır.

# Ahead of time (AOT- Vaktinden önce) compiler

c, c++ dilleri direk olarak makine koduna çevrildiğinden, runtime sırasında derleme işlemine gerek kalmaz ve bu sebepten daha hızlı çalışır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# WebP

Google'nin geliştirdiği resim formatı. Daha çok web sayfalarında gösterilmesi için geliştirilmiştir. bu sebeple; hızlı yüklenme ve az boyutlu olacak şekilde tasarlanmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# .tar formatı

tarball adı verilen bu dosyalar sadece arşivlenmek amaçlı kullanılmaktadır. unix sistemlerinde ki dosyalar için, dosya sistemindeki bilgilerini de .tar dosyası içerisinde saklamaktadır. oysa zip gibi formatlarda sadece windows dosya sistemindeki bazı bilgiler saklanmaktadır. tar formatı sıkıştırılmadığından, genelde ek araçlarla sıkıştırılır ve dosya uzantısının yanına bir uzantı daha eklenir yada dosya uzantısı tamamen değiştirilir.

# iso formatı

iso formatı bazı dosya sistemlerinin partititon bilgileride tutmak için kullanılmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# windows işletim sistemi dizin yapısı

> RECYCLED 

> RECYCLER

> $Recycle.Bin

işletim sistemi versiyonuna göre ismi değişmektedir. bölümün root dizinindedir. çöp kutusu aslında burdadır. windows çöp kutusu buradaki tüm dosyaları tek klasördeymiş gibi gösterir.

> System Volume Information

NTFS dizinlerinde root'ta bulunur. bu dizin içerisinde system restore bilgilerini, hızlı arama (index) için bilgileri, kısayol ve link edilmiş tipteki dosyaların bilgileri vs gibi bilgileri tutar.

> Program files

"C:\Program files (x86)"" 32 bit uygulamaları barındırırken, 64 bit uygulamalar "C:\Program files" dizinindedir.

> C:\ProgramData

tüm kullanıcılar için ortak data saklayabileceği dizindir. uygulamalar alt klasörler yaratarak dosya saklayabilirler.

> C:\Documents and Settings\username

Microsoft Windows 2000, XP and 2003 için home dizini

> C:\Users\username 

Microsoft Windows Vista, 7, 8 and 10 için home dizini

> C:\Users\AllUsers

C:\ProgramData dizine kısayoldur.

> C:\Users\Default

windows yeni kullanıcı açtığında buradaki dosyaları yeni user'a aktarır.

> C:\Users\Public

Tüm kullanıcılara ve networkteki kullanıcılara açık olan dizindir. kolay dosya paylaşımı yapabilmek için kullanılmaktadır.

> C:\Users\UserName\AppData\Roaming

Microsoft user syncronizing feauture sync only these files.

> C:\Users\UserName\AppData\Local
ve
> C:\Users\UserName\AppData\LocalLow

app can store files here the files which will not sync with their windows-ms-account

> C:\Windows\System

16 bit dll dosyalarını bulunduruyor.

> C:\Windows\System32

32 bit dll dosyalarını bulunduruyor.

> C:\Windows\SysWOW64

64 bit dll dosyalarını bulunduruyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Rassal değişken (yada Random variable)
rastgele oluşturulan değişkendir.

# sözde rastsal (yada pseudo random number)
bilgisayar ortamında rastgele değişken oluşturulamayacığından, sözde rastsal sayılar oluşturulabilmektedir.

pseudorandom number generator (yada eng-kısaltma:PRNG yada deterministic random bit generator yada eng-kısaltma:DRBG) programları bu sayıları üretmek için kullanılır.

bilgisyarlar rastglele sayı üretebilmek için, her cihazda olan aşağıdaki 2 referanstan yararlanır:
- saat-tarih
- CPU çalışmaya başladığından itibaren kaç cycle geçmiş gösteren __Clock Sequence__

# Hardware random number generator (yada eng-kısaltma:HRNG yada true random number generators)
donanımlar aracılığı ile rastgele sayı üretilememektedir (yazılımda olduğu gibi). fakat donanımlar bazı sensörler ile çevreden bilgi alıp rastgele sayı üretmeye çalışabilirler. örneğin; ısı sensörü gibi. aslında burada da %100 rastgele sayı üretilmiş olmuyor. fakat önceden kestirilemeyen (unpredictability) sayı üretiliyor.

# rasgele sayı üretimi hakkında

- Teorik olarak; sadece hiç input almadan rastgele sayı üretebilen bir platform ancak bize gerçekten rastgele sayı üretebilir.

- işletim sistemleri klavye ve mouse gibi bazı kaynaklardan yararlanarak rastgele sayı veren API sunarlar. örneğin posix'lerde: /dev/random'dan yararlanılır.

- random.org sitesi bir web servis üzerinden rastgele sayı servisi sağlıyor. bu servis hava durumundan yararlanıyor.

# UUID (yada Universally unique identifier)

- https://www.ietf.org/rfc/rfc4122 (archive date: 02/11/2019)'da belirtilen standartlardır. her versiyon hakkında bilgi yine bu standart altında belirtilmiştir. bu standartta 5 adet version belirtilmiştir.

  - version 1 (Time Based)

    cihazdaki MAC adresi ve data-time (en ufak birimi nanosecond olacak şekilde) bilgisi ile UUID üretir. aynı bilgilerle aynı UUID tekrar üretilebileceğinden bazı durumlarda takip edilebilirliği sağlamak amaçlı bu kullanılmaktadır.

  - version 2 (DCE Security)

  - version 3 (Name Based)

    UUID oluşturmak için namespace(url gibi) ve name'in MD5'i kullanılır. örnek kod:

    ```java
    String source = namespace + name;
    byte[] bytes = source.getBytes("UTF-8");
    UUID uuid = UUID.nameUUIDFromBytes(bytes);// bizim için MD5 hash alıyor
    ```

  - version 4 (Random)

    sistem random number generate ederken, önceden belirli datalardan yararlanmaz. bu sebeple genelde bu tercih edilir.

  - version 5 (Name Based)

    Version 3 ile aynı. tek var MD5 değil, SHA-1 istiyor.

- version

  most significant 4 bits of "time" block.

- variant

  UUID'nin ilk 2 bitidir.

  UUID'nin layout'udur. hangi bitlerde, hangi bilginin yer aldığı standartıdır.
  
  rfc4122'de sadece bir variant anlatılıyor.

  ("x" önemi olmayan bit anlamına geliyor)

  | bit-0 | bit-1 | bit-2 | explanation                                            |
  |-------|-------|-------|--------------------------------------------------------|
  | 0     | x     | x     | Reserved, NCS backward compatibility                   |
  | 1     | 0     | x     | rfc4122'de anlatılan formattır                         |
  | 1     | 1     | 0     | Reserved, Microsoft Corporation backward compatibility |
  | 1     | 1     | 1     | Reserved for future definition.                        |

  Leach-Salz (rfc4122 yazarlarının soyadları) UUID'nin 2'inci variantının (digit olarak bakıldığında varyant değeri "1") takma adı olarak her tarafta kullanılmaktadır.

- java code of version vs variant

  ```java
  UUID uuid = UUID.randomUUID();
  int variant = uuid.variant();
  int version = uuid.version();
  ```
  
- Tekil bir stringdir. 128 bit'tir.

- sayı çok büyük olduğundan aynı sayının aynı uygulama içinde üretilmesi matematiksel olarak çok azdır. bu sebeple standart olarak kullanılır.

- örnek kullanım alanlar:
  - MAC addresler
  - dosya sistemlerinde partition id'leri
  - db'lerde id'ler

- 36 karakterdir ve arada tire koyularak (tireler sadece notasyondur, 128 bite dahil değildir) 5 grup olarak gösterilir:
  > 123e4567-e89b-12d3-a456-426655440000
    
  her grup bir data'yı temsil eder:

  - time
  - version
  - clock_seq_hi
  - clock_seq_lo
  - node

- sadece küçük harf karakterler geçerlidir.

- Uniform Resource Name (yada URN) gösterimi bu şekildedir:

  > urn:uuid:123e4567-e89b-12d3-a456-426655440000

- Nil UUID

  tümü sıfır olan UUID'dir.

# Globally unique identifier (yada GUID)

Microsoftun UUID implementasyonudur. UUID'den hiçbir farkı yoktur. sadece variant değeri farklıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ISO (yada International Organization for Standardization)

Teknik ve teknik olmayan konularda standartlar belirleyen komisyon. Sadece IEC'in (Uluslararası Elektroteknik Komisyonu) ilgilendiği konularda standart belirlemiyorlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ekonomi/finans/borsa/sanal para/muhasebe/online ödeme/bankacılık/sigortacılık için temel kavramlar

# sermaye (yada tr:kapital yada eng:capital)

bir amaç için ayrılmış kaynakların tümü (__anapara (yada resülmal)__ ve paraya çevrilebilir malların tamamı).

# KOBİ (yada Küçük ve Orta Büyüklükteki İşletme yada Small and medium-sized enterprise yada SME)

Avrupa birliği standartlarında geçen büyüklüğü maximum noktası belli olan ticari kuruluşlardır. Türkiye'de KOSGEB'e bağlılardır.

# anonim şirket (yada AŞ yada Joint-stock company) vs limited şirket (yada LTD yada Limited company)

anonim şirketler halka açılabilir (hisse senedi) ve tahvil çıkarabilir. limited'ların böyle bir şansı yok. her ülkenin yasası birçok detay belirlemektedir. fakat temel olarak asıl fark budur.

# Şirket (veya firma veya company)

tüzel kişiliktir (yada juridical person).

# nominal değer (yada stated value yada eng:nominal value yada par Value yada face Value)

nominal kelime anlamı: bi yere yazılı olan

herhangi bir iktisadi/ekonomik belgenin üzerinde yazılı olan değerdir. örneğin bir arazinin nominal değeri 100.000 TL'dir. halk arasında nominal yerine "kağıt üzerinde" terimi kullanılır. oysa gerçek (reel) değeri bundan daha az yada çok olabilir.

# sigorta (yada insurance)

önceden ödenen prim karşılığında, bir kimsenin ya da bir şeyin herhangi bir yönden ilerde karşılaşabileceği zararı gidermek için, bu işle uğraşan bir kuruluşla yapılan bağlantı sözleşmesi.

# Kara taşıtları sigortası (yada eng:kasko)

hem ingilizce hemde türkçede "kasko" olarak geçmektedir.

bir sigorta çeşididir. trafik sigortları bazı ülkelerde sadece kazada karşıdakinin masraflarını öder. bu sebeple kasko kazayı yapan kişinin masraflarını ödeyen sigorta çeşidi olarak ortaya atılmıştır. trafik dışında diğer sigorta türlerinde böyle bir bölünme zorunlu diildir.

# ipotek (yada hypothec)

borç karşılığında, ödemenin geri yapılmasına kadarki süre için borçlunun bir varlığı alacaklıya ipotek ettiririlir. ipotek bir çeşit teminattır. örneğin; bankadan konut kredisi alındığında banka eve ipotek koyar.

# Mortgage (yada mortgage loan yada Tutsat yada tutulu satış yada ipotekli satış yada rehinli satış)

ipotek her zaman olmak zorunda değildir. ipoteksiz de borç verilebilir. ipotekli tüm borç alma işlemlerine iktisatta Mortgage adı verilir.

# repo (yada repurchase agreement yada RP yada sale and repurchase agreement)

Mortgage üzerinden devam ederek anlatırsak; ipotek yapılan malın sahibi hala borçlu olan kişiydi. repo'da ise durum farklı. ipotek koymak yerine, bu malı borç veren kendi ismine almaktadır. fakat; bunu yaparken bir sözleşme imzalanmaktadır: eğer borç + faizi + vade farkı (anlaşama göre değişebilir) borç verene ödenir ise, borçlu bu malı sabit (o anda sözleşmede belirnen fiyata) alma yetkisine sahiptir. işte bu sözleşme repo'dur. borçlu repo yapmış oluyor, borç veren ise "ters repo" yapmış oluyor.

# Mikro ekonomi vs makro ekonomi

mikro ekonomi bireysel, tükecici ve alıcı piyasalarının ekonomisini incelerken, makro ekonomi global, ülke gibi daha geniş çapta piyasaların ekonomilerini inceler.

# İktisat (veya ekonomi) vs finans (veya finance)

finans para temelli yatırımların yönetimini ele alır. ekonomi ise kısıtlı kaynakların üretim, dağıtım ve paylaşım biçimlerini inceleyen daldır.

# kredi (yada credit)

borcu ödeme güvenilirliği olma durumudur. örnek: "piyasada kredisi var". kredi kelimesi aynı zamanda; para/malın bir vadede içinde geri alınmak üzere verilmesidir.

# Emtia (yada Commodity)

kelime anlamı olarak ticarette kullanılan mal anlamına gelir. dolayısı ile fiziksel varlıklardır. dünya üzerinde her mal emtia'dır. altın ve gümüş en çok kabul gören emtialardır. çünkü zehirli diildirler, yanmazlar, kolay tepkimeye girmezler... gümüşün hemen kararması altını ön sıraya çıkarmıştır.

bitcoin ve altcoinlerin birer emtia para olup olmadığı konusu net değildir. çünkü bitcoinde paranın karşılığı olarak teknoloji ve güvenilirlik yatmaktadır. bu onu emtia paraya benzetir. fakat nakit paradaki seri numara gibi bir unique'liği olduğundan ve zamanın geçmesi ile eskimesi/zarar görme durumu olmadığından emtia tanımına uymaz ve bu sebeple nakit para gibi işlem görmektedir.

"emtia" devlet yasalarında geçmektedir. örneğin; emtia paralardan kar edildiğinde çoğu devlet vergi toplar. fakat "para" bozdurup alımlarda kar edildiğinde devletler vergi toplamaz. dolayısı ile bir ülkede neyin emtia neyin para konumunda olduğu resmi olarak önemlidir. bir ülkede emtia olan varlık başka ülkelerin yasalarında "para" sınıfında olabilir. yada başka bir ülkenin yasasında emtia olarak geçen bir varlık başka ülkenin yasasında hiç tanımlı bile olmayabilir.

emtia kelimesi yasada geçsede, kelime anlamı ile emtia bir araba da olabilir.

# menkul kıymetler (yada tangible assets yada chattels)

- menkul kıymetler; TDK'da araba gibi taşınabilir değerler olarak belirtilmiştir. Hukuk'ta ise tahviller, Kâr-zarar ortaklığı belgeleri,  hisse senetleri gibi birçok değeri temsil etmektedir. Hangilerinin menkul kıymet olup olmayacağı o ülkenin yasasına belirtilmelidir.

- gayrimenkul kıymetler (yada Real estate); arazi, ev gibi değerleri kapsamaktadır. 

# döviz

türkiye'de sadece yabancı paraları temsil etmek için kullanılan kelimedir. türkiye dışında, yabancı ülke paralarını temsil etmek için "foreign Currency" terimi kullanılır.

# döviz işlemi (yada kambiyo işlemi yada currency exchange)

para birimleri arası çevirme işlemi.

# merkez bankası

her ülkede ülkenin döviz rezerv'ini tutmak ve para yönetimini yapmak gibi birçok  görevi olan, devletin belirlediği yasalardan yararlanarak hareket eden kurumdur. bankalara borç veren ve hatta bankalara paralarını yatırdıkları takdirde faiz verebilir. türkiyenin merkez bankası TCMB'dir.

banknote'ları sadece ülkelerin merkez bankaları basmaya yetkilidir. amerika merkez bankası karşılıksız banknote-dolar bastığı biliniyor. bu sebeple amerika kendi para birimi olan dolara güvenmez, bu sebeple amerika merkez bankası (ve diğer güçlü ülkeler) rezerv'lerinde yüksek mikatrda altın saklar.

# FED (yada Federal Rezerv Sistemi)

ABD'nin merkez bankasıdır.

# finansal enstrüman

finans sektöründe "enstrüman" terimi ticarette kullanılan değerler için kullanılır. örnek: dolar, altın, hisse senedi gibi.

# merkez bankasının piyasaya dolar yada TL sürmesi

piyasaları dengelemek için yapılan bir harekettir. MB piyasaya TL dağıtmak için elindeki TL haricindeki enstümanları ucuza satar. herkes alınca piyasada TL mikarı artmış olur.

# banknote (yada tr:banknot)

kağıt para

# coin (yada bozuk para yada madeni para)

halk arasında "demir para" diye geçkemktedir.

# eng:dollar (yada tr:dolar)

dünyada birçok farklı dolar isminde para birimi vardır (20'den fazla). çoğunun simgesi bile aynıdır. fakat birbirlerinden tamamen bağımsızdırlar. örnek: Amerikan doları  (ISO 4217 para birimi kodu: USD), Avustralya doları (ISO 4217 para birimi kodu: AUD)...

Dolarları birbirinden ayırt edebilmek için farklı kısaltmalar kullanılır: örneğin; avustralya doları için kısaltmalar: A$, $A, $AU, AU$

# tr:peni (yada eng:penny yada plural-eng:pence)

halk arasında kullanılan resmi olmayan bir terimdir. 1 dolar yada ingiliz strelin'in 100'de birine denk gelen paraya verilen takma isimdir.

# eng:Cent (yada tr:sent)

simgesi: ¢

dolar'ın 100'de birine verilen isimdir. tüm dolar çeşitleri (amerikan, avustralya doları) için kullanılır.

# bazı para birimleri

```
KOD  simge  İsim Türkçe       İsim İngilizce

JPY  ¥      Japon Yeni        Japanese Yen

CAD  C$     Kanada Doları     Canadian Dollar

CHF  SFr    İsviçre Frangı    Swiss Frank

GBP  £      İngiliz Sterlini  British pound (yada British Sterling yada British Sterlin)

CNY  ¥      Çin Yuanı         Chinese Yuan

DKK  DKr    Danimarka Kronu   Danish krone (yada Danish crown)

MXN  $      Meksika Pezosu    Mexican peso

JOD         Ürdün dinarı      Jordanian dinar
```

- Çin Yuanı ve Japon Yeni aynı para birimi simgesine sahiptirler.

- Meksika Pezosu, dolar ile aynı simgeye sahiptir. Bu sebeple bazı yerlede N$ olarak kullanılır.

- Dünyada sadece dolar değil, birçok para birimi aynı isimde bağımsız bir şekilde  ülkeler tarafından basılmaktadır. Bu sebeple her para biriminin önüne merkez bankasının bağlı olduğu ülke ismi de yazılmalıdırki karışıklık olmasın. örnek: GREENLAND, "danish krone'u" kullanırken, norveç kendi bağımsız kronunu kullanmaktadır.

# ISO 4217

Uluslararası para birimi ve emtia kodu standartıdır. USD, GBP gibi kodlar bu standrattadır. XAU Altın'ı temsil etmektedir.

# türk lirası

TRL ISO kodu ile 2005'e kadar kullanılmıştır. Daha sonra TRY kodu ile yeni bir ISO kodu yayınlanmıştır.

31 Ocak 2005 tarihinde liradan 6 sıfır atılarak Yeni Türk lirası (YTL) oluşturulmuştur. Yeni banknotlar basılmıştır. 1 Ocak 2009 tarihinde ise YTL yeniden TL olarak değiştirilmiştir. Bu işlem içinde yeni banknotlar basılmıştır. YTL ve TL terimleri ISO 4217'da yoktur.

# tr:lira (yada eng:lira yada plural-eng:lire)

lira kelimesi eskiden Roma'da ağırlık birimi olarak kullanılıyordu. Daha sonra  ülkeler günümüzde dahi para birimi olarak kullanmaktadır: Mısır lirası. Suriye lirası...

# emisyon (yada emission)

kelime anlamı: yayma, dışarı çıkarma

finans sektöründe devletçe piyasaya para, pay senedi, tahvil çıkarma. örneğin; her yeni para birimi basımı bir emisyondur.

xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

(xxx içindeki bu kısımdaki başlık sırası önemli)

# senet

finans dışındaki anlamı; dayanılacak belgedir. resmi olmak zorunda değildir.

iktisatta ise; bir kimsenin ödemeye ya da yapmaya borçlu olduğu şeyi göstermek üzere imzalayıp ilgiliye verdiği belgedir. her poliçe ve çek birer senet çeşididir.

# Poliçe (yada policy)

alacaklı tarafından, borçluya düzenletilen ve belirli bir paranın üçüncü bir şahsa ya da kendine (alacaklıya) ödenmesini bildiren bir belgedir. yani 3 taraf vardır: alacaklı (keşideci), borçlu ve muhattap.

Poliçe aynı zamanda sigorta firmalarının insanlarla(müşterilerle) yaptığı sözleşmelere verilen isimdir.

# çek (yada cheque yada check)

muhattap'ın banka olması durumundaki poliçe çeşididir.

# Bono (yada ingilizce:bond)

çek ile aynı amaçtadır. fakat sadece alacaklı ve borçlu vardır. muhattap yoktur.

# lehtar (yada beneficiary)

Çek veya poliçenin ödeneceği, belgenin üzerinde adı yazılı olan kişi.

xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# açığa satış (yada short sale yada shorting yada going short)
hisse senetleri aracı kuruluşlardadır. aracı kuruluşlar, bir müşterinin hisse senedini diğer müşteriye kiralayabilir ödünç verebilir. bu durumda; kiralayan müşteri günlük kira parası alır.

açığa satıştakki amaç; hisseleri yüksekten satıp, ucuza almaktır. örnek: 100 dolarlık bir hisse olsun. 50 dolara düşeceği haberini aldık. bu durumdan kar etmek istiyoruz. şu şekilde işlem yapıyoruz:

- 100 dalrlık, 1 adet, X firmasının hisessini aracı kurumdan ödünç aldık.
- bunu borsada 100 dolara saatık. (bu işlem açığa satış oluyor)
- daha sonra 50 dolara düşen hisse senedini 50 dolara piyasadan aldık.
- aldığımız 1 adet X firmasının hisse senedi ile aracı kuruma borcumuzu ödedik.

Burada 50 dolar kar etmiş olduk (aracı komisyona verilen hisseler düşülmedi).

# Portföy (yada portfolio)

bir kişinin ya da kuruluşun sahip olduğu ve kazanç için harcayacağı varlıkların tümünü ifade eder.

# envanter (yada Inventory)

Mal ve değerlere ait döküm. veya bir kuruluşun borçları ve elindekileri gösteren çizelge.

# tr:fon (yada eng:fund)

Belirli bir iş için gerektikçe harcanmak üzere ayrılıp işletilen para.

# yatırım fonu

yatırımcıdan alınan paralar birçok farklı hisse senedi ve  dövizler alınır. böylece yatırımcının zarar etme riski düşmüş olur. bu şekilde yapılan yatırımlara "yatırım fon"u denir.

# hisse senedi (yada stock certificate yada share certificate)

bir şirket devletten izin alarak ortaklarını arttırabilir yada azaltabilir. bu ortaklar hisse senedi aracılığı ile ortaklık oranlarını (şirket paylarını) ellerinde tutarlar.  hisse senedi sahipleri şirket içinde birçok yönetimsel hakka sahip olurlar.

# tahvil

bir firma borç almak istediğinde piyasaya hisse senedi satabilir. fakat bunu yapmayıp birilerinden borç almayı tercih edebilir. işte bu durumda şirket dışarıya tahvil dağıtır. tahviller bir nevi basit borç belgeleridir. vatandaş(yatırımcı) için hisse senedi daha karlı olabilir fakat daha risklidir. oysa tahvil daha garantidir fakat daha az kazandırır.

# borsa (yada Exchange yada organized market yada bourse)

belli kurallara göre talep edilen malların alınıp satıldığı piyasadır. bu piyasalar birbirindenbağımsızdır. yani birçok çeşit borsa vardır. örneğin; döviz borsası. menkul kıymetler borsası.

örneğin; "Borsa İstanbul (BİST)" eski adıyla da "İstanbul Menkul Kıymetler Borsası (İMKB)" bir menkul kıymet borsasıdır. BİST devletin "Sermaye Piyasası Kurulu (SPK)" birimi tarafından denetlenir. Devlet denetimi olduğundan, manipülasyonlar engellenmeye çalışılır ve halk/şirketler bu tarz borsalara bu sebepten güvenir. Oysa borsa kavramı devlet yönetiminde olmak zorunda değildir. Dolar, emtia, euro, altın aldığımız herhangi bir merci aslında kendi çağında bir borsadır. İstediği fiyata elinde olan varlıkları satabilir/alabilir. Bir firmadır. Tabi borsa firmaları, güven kazanmak için her zaman alım/satım değerlerini göz önünde bulundurur (arz/talep) ve piyasadaki diğer borsalarla orantılı fiyat sunmak zorundadır. eğer bunu yapmaz ise kimse zaten o firmayı tercih etmeyecektir. bir varlığın değeri arz/talebe göre belirlenir. örneğin borsaların elinde fazladan bitcoin varsa, bitcoini ucuza satarlar. fakat az bitcoin varsa borsaların elinde, bu bitcoinleri daha yüksek fiyattan satarlar. 

# likit (yada nakit yada Cash yada akça)

istenildiği anda harcama yapabilecek durumda olan finansal varlıklardır.

# ROI (yada return on investment)

yatırımın geri gönüş karıdır.

# katılma hesabı (yada Participation Account)

islam'da dini kurallar gereği faiz yanlıştır. bu sebeple bazı bankalar faizsiz hesap açar. faiz veremediği için farklı olarak; şirket ile fonlara ortak olunur ve şirketin elde edeceği kara ortak olunmuş olunur. faiz ile ortalama aynı getiriye sahip olacak şekilde ayarlanmaya çalışılır. fakat kara ortak olunduğundan önceden kar yüzdelikleri kesin/net değildir. 

# forex

forex'in kelime anlamı "foreign exhnage"'in kısaltmasından gelmektedir. fakat bu kelime sadece yabancı kurların ve emtia paraların çıkışından/azalmasından faydalanarak yatırımcılara sunulan farklı imkanları veren piyasaları temsil etmektedir. özellikle kaldıraç sistemi (yada leverage system); forexin temelinde yatan en ilgi çekici özelliktir. yatırımcı 1000 dolar ile 50000 dolar satıl almaktadır. doların her yükselşi olan 50000 dolar cinsindenverilmektedir. fakat doların inişi ile de 50000 dolar üzerinden kesinti yapılmaktadır. kaldıraç sitemi ile yatırımcılara kullanabileceği doları geçici bir süreliğine verip dolar ile para kazanması sağlanmaktadır.

# arbitraj (yada arbitrage)

bazen, bir para birimi yada emtia para farklı bir piyasalarda  ücretlerden alınıp satılmaktadır. böyle durumlarda bunun farkına varan insanlar elindeki X tipi parayı satıp, yine X tipi parayı daha ucuzdan almaktadır. böylece risksik bir para kazancı elde ederler. bu işleme arbitraj denir.  

# vade (yada maturity)

Bir işin yapılması veya bir borcun ödenmesi için tanınan süredir.

# mevduat (yada deposit)

bankalara ve benzeri kredi kurumlarına istenildiğinde ya da belli bir vade ya da ihbar süresi sonunda çekilmek üzere yatırılan paralardır. "vadeli mevduat" olduğunda alacaklı taraf ana parasını faizi ile birlikte alır.

halk arasında "faiz hesabı"" diye kullanılır.

# valör

Üzerinde anlaşma sağlanarak yapılmış bir işlemin yükümlülüklerinin, fiilen yerine getirileceği tarih. örnek: mevduat hesabı açalım. bu hesabın faizi en az bir gün sonra başlar. o gün hiç bir anlam ifade etmez. valör, yarının tarihidir.

# kıyı bankacılığı (yada Offshore bank)

Offshore kelime anlamı: kıyıdan uzak

hizmet verdiği ülkenin dışında kurulu olan bankadır. insanlar genelde gizlenmek amaçlı bu firmalardan yararlanıyor. genelde bu tarz firmalar vergisi düşük olan yada denetimi az olan ülkelerde kuruluyor.

# ekstre (yada extract)

hesap işlem özeti.

# fatura (yada invoice)

Satılan bir malın cinsini, miktarını ve fiyatını bildirmek için satıcının alıcıya verdiği belge.

# dekont (yada receipt)

Çekilen veya yatırılan para karşılığında bankanın verdiği belge.

# fiş (yada receipt)

mal/hizmet karşılığı ödenen paranın miktarını, vergilerini, alışverişin yapıldığı tarihi gösteren belge. fatura ile aynıdır. sadece belge standartları yasada farklıdır. fiş; ticari olmayan işlemlerde veya şirket-vatandaş arasında yapılan ve yasanın belirlediği düşük fiyatlı alışverişlerde kullanılabilir.

# makbuz (yada receipt)

Para/malın teslim alındığı gösteren belge. fatura'da alışverişe karşılık her iki taraf birbirine borçlanmaz. fakat makbuzda sadece bir şeyi teslim alırsınız ve karşıya hiçbir şey vermezsiniz.

makbuz ve dekont aynı şeyi belgelerler. fakat dekont sadece bankalar tarafından verilebilir.

# parite (yada parity yada çapraz kur)

iki ülke parasının karşılıklı değeri (birbirlerine oranı). örnek $/EURO paritesi: 3.1/4.2 gibi.

# cari hesap

cari kelime anlamı: yürürlükte olan, hala devam eden

ticari işlemlerde müşteri/iş yeri yada iş yeri/iş yeri arasında yapılan birçok işlemin tutulduğu hesaptır. birbirlerine yaptıkları transfer işlemleri cari hesapta kayıt altında tutulur. bazı bankalar bu iş için özel hesap tipi sunuyorlar.

# cari açık (yada dış ticaret açığı)

ülkenin dışsatımıyla dışalımı arasında oluşan açık. daha doğrusu; bir ülkenin ithal (yada imported) ettiği malların ihraç (yada exported) ettiği mallardan fazla olması durumudur. 

# bilanço

bir kuruluşun belli tarihler arası harcamalarını ve bu harcamalara karşılık elindeki değerleri gösteren tablodur. 

# Ayı Piyasası (yada Bearish Market)

Piyasalarda genel anlamda düşüş eğiliminin baskın olduğu zaman periyodlarını ifade eder.

# Boğa Piyasası (yada Bullish Market)

Piyasalarda genel anlamda çıkış eğiliminin baskın olduğu zaman periyodlarını ifade eder.

# Leaser (yada kiraya veren)

lease: kiralama anlamına geliyor.

# satınalma (yada devralma yada Acquisition)

bir malı satın alma işlemi.

# Broker
kelime anlamı: komisyoncudur.

finans sektöründe kullanılan anlamı: müşteriyi ekonomik yatırımlara yönlendiren ve bundan komisyon alan işçilere deniyor.

# Devalüasyon (yada devaluation)

para biriminin diğer para birimlerine karşı değerinin düşürülmesidir. bu düşürme işlemi bazen kötü yada iyi niyetle yapılıyor olabilir.

# Enflasyon (yada Inflation)

bir ülkede genel olarak ürünlerin satış fiyatının artmasıdır. 

# marj

Bir işlemin yapılabilmesi için gereken minimum teminat tutarıdır.

# ATM (yada automated teller machine)
teller'ın türkçe anlamı; bankalarda para alıp veren kişidir (veznedar)
Banka işlemleri için kullanılan donanımlardır. Üzerinde işletim sistemi kuruludur. ATM için geliştirimiş uygulama startup'ta otomatik açılır.

# borsa sitelerindeki satış/alış akışı ve kur rezervi yönetimi

- borsada örneğin dolar satın alacağız. aldığımız kur onaylandığında onay sayfasına gideriz. onay sayfasında belli bir süre tölerans tanınır. örneğin 10 dakika belirlediğimiz kurdna belirlediğimiz miktar doları satın alabiliriz. burda belli adet dolar miktarını belli bir kurdan "rezerv" etmiş oluruz.

- borsada borsa kurumu 1 doları 3 tl'den satıyor ise, istenilen adet kadar doları 3 tl'den satmaz. 100.000 adet doları 3 tl'den satıyordur. eğer birileri totalde bu sayıdan fazla örnek 140.000 $ satın almaya kalkarsa, ilk 100.000 dolar 3 tl'den, kalan diğer 40 bin dolar ise 3.1 tl'den satılır. bu şekilde borsa kurumu kendini güvenceye alır.

# fiyat endeksi (yada price index)

endek kelimesi türkçe kelime anlamı: 1. dizin  2. gösterge

bir emtipanın/paranın/malın fiyat endeksi piyasadaki değerinin göstergeleridir. örneğin bir grafikte yıllara göre doların fiyat endeksini görebliriz.

# derinlik tablosu (yada Order Book)

borsa sitelerinde herkes satış/alış emri verebilir. eğer belli bir fiyata gelirse şu kadar dolarımı sat gibi emirler... bu emirler public yada ücretli olarak borsa sitelerinde canlı olarak yayınlanır. alışlar ve satışlar talosu vardır. bu tabloların tümüne derinlik tablosu denir.

derinlik tablosunun resmi bir standardı olmaz. sutun bilgileri borsa sitesinden sitesine değişir.

# Card Associations (yada Kart Kuruluşları yada Payment Brands)
bankaların birbirlerinin pos'ları ile ödeme yapmalarını sağlayan ortak merkezi kuruluşlardır. X bankası kartı ile Y bankası pos'u ile işlem yapmak istendiğinde ortak bir merkezi sistemden onay işlemi gerekir. iki tarafın mutabakat yapması gerekir. işte bunu sağlayan firmalar kart kuruluşlarıdır.

Kart kuruluşlarının para barındırma gibi bir durumu yoktur. X ve Y bankası birbirine para transferi yapması gerekir. çünkü ödeme yapan müşterinin parası kartın bankasındadır ve acquirer bankaya aktarılması gerekir. bunun transferi/onayı için merkezi sistem kart Kuruluşlarıdır. bu işletim sürecine __"Exchange and Settlement" (yada Takas ve Hesaplaşma)__ denir.

card Kuruluşları her işlem için bankadan komsiyon alırlar.

bazı kart kuruluşları:

- Visa
- MasterCard
- American Express (yada abbr:Amex yada abbr:Amexco)
- Discover
- UnionPay (çin)
- JCB (yada Japan Credit Bureau)
- Diners Club International (yada DCI)
- Discover Card
- Troy

Eğer A bankası, X kart kuruluşunu desteklemiyorsa o banka üzerinden X kartı ile işlem yapamayız. Fakat X eğer A'nın desteklediği bir kart kuruluşu ile anlaşırsa işlem yaptırtabilir. örneğin troy'u destekleyen banka yurtdışında yoktur. BKM firması Discover ile anlaştı. artık troy Discover geçen her atm, her pos'ta desteklenmektedir.

Bir müşteri X kartı ile A bankası pos'undan ödeme yapsın. X bankası komisyon alır + A bankası komisyon alır + aradaki kart kuruluşu komisyon alır.

Aynı kart kuruluşuna bağlı X ve Y bankası olsun. X banka kartı ile Y bankasının ATM'sinden para çektiğimizde yine kart kuruluşu ve Y bankası X bankasından komisyon alır.

# Card Networks (yada kart şeması yada card scheme)
Kart kuruluşlarının sağladığı ağlara (servislere) denir.

Kart şemaları 2'ye ayrılır:
- three-party scheme (or closed scheme yada üç taraflı kart sistemi)
  
  acquirer bank ile issuer bank aynı olduğundaki duruma verilen isimdir. bu işlemlere __onus (yada on-us)__ denmektedir. 3 taraf vardır:
  - issuer bank = acquirer bank
  - card holder
  - merchant

- four-party scheme (or open scheme yada dört taraflı kart sistemi)
  
  acquirer bank ile issuer bank farklı olduğundaki duruma verilen isimdir. bu işlemlere __not onus (yada not on-us)__ denmektedir. 4 taraf vardır:
  - issuer bank
  - card holder
  - merchant
  - acquirer bank

# Maestro vs MasterCard

"MasterCard Inc" firmasının sadece banka (debit) kartları için sunduğu hizmettir. "MasterCard" hizmeti ise debit ve diğer tüm kartları kapsayacak şekilde tasarlanmıştır. Yani bir debit cartımız sadece MasterCard yada sadece Maestro etiketini taşıyabilir. Dolayısı ile ikisinin standartları farklılık göstermektedir.

# Electron vs Visa

Electron, Visa'nın banka (debit) kartlarına verilen genel isimdir. Aynı "Maestro vs MasterCard" konusundaki durum söz konusudur.

# troy

BKM firmasının (2014'lerde) çıkardığı Card Association hizmetidir.

# Card Group

- Credit card (yada Kredi Kartı)

- Debit card (yada Banka Kartı): "ATM kartı" olarak da bilinir. bankadaki hesabınıza ait kartlardır. Hesabınızda para bulunduğu müddetçe offline ve online'da alışveriş yapabilirsiniz. online alışverişlerde taksit seçeneği ve provizyon işlemi yoktur. direk para varsa kart içerisinde satış işlemi yapılabilir.

- Prepaid (yada Ön-ödemeli Kart): Debit card ile aynı özelliktedirler. Fakat bu kartların arkasında banka hesabı bulunmaz. (zaten bulunursa debit card olur). bu kartların yine bağlı olduğu kart kuruluşları vardır. örnek kart: ininal, paypal, papara, paycell.

# Ek kart

asıl karta bağlı olan birden fazla ek kart olabilir. ek kart genelde çocuklara ve eşlere çıkartılır, fakat ek kartı kullanacak kişinin akraba olması şart değildir. ek kart 16 yaş üzerine çıkarılabilir.

# Loyalty card (yada Sadakat kartı)

Bonus, Axess, World, Maximum örnektir. bunları merchant'lar bankalar ile anlaşarak son kullanıcılara verir.

# ödeme kartında bulunan kod

Kartın ön yada arka yüzündedir. hane sayısı farklı olabilir. her kart firması bu koda kendi ismini vermiştir. örnekler:

| adı                                                                              | kullanan kart markası           | kartın neresinde             |
|----------------------------------------------------------------------------------|---------------------------------|------------------------------|
| CID (yada Card Id yada Card Identification Number yada Card Identification Code) | Discover and  American Express  | four digits on front of card |
| CSC (yada Card Security Code)                                                    | debit cards of American Express | three digits on back of card |
| CVC2 (yada Card Validation Code)                                                 | MasterCard                      |                              |
| CVD (yada Card Verification Data)                                                | Discover                        |                              |
| CVE (yada Elo Verification Code)                                                 | Elo in Brazil                   |                              |
| CVN2 (yada Card Validation Number 2)                                             | China UnionPay                  |                              |
| CVV2 (yada Card Verification Value 2)                                            | Visa                            |                              |

Yukarıdaki örneklerde "2" suffix'ine sahip güvenlik kodları sürüm numarasıdır.

Bazı kodlar son kullanının görebileceği yerlerde değildir. örneğin CVV'nin 1'inci versiyonu kredi kartlarının üzerindeki manyetik bantta kayıtlıdır. dolayısı ile sadece cihazlar tarafından okunabilir.

# BIN (yada Bank Identification Number yada IIN yada Issuer Identification Number)

Eskiden BIN, günümüzde ise IIN olarak adlandırılan kartın ilk 6 hanesi. bu bilgileri içerir:

- Bank ID

  Bin numarasının ait olduğu bankayı gösterir

- Kart numarası uzunluğu

  Kart numarasının kaç hane olduğunu gösteren bildirgeçtir. Örneğin 15 ya da 16 haneli kart. (çünkü kart numaraarı sabit dğeildir)

- Check Digit Algoritması
  
  Kart üretimi sırasında check digit kullanıp kullanılmadığı. (çünkü bazı kart numaraları belli bir algortimaya uymaz. örneğin luhn kullanılmayabilir.)

- Kart Şeması
  
  Kartın hangi kartlı ödeme şemasında olduğu bilgisi. Örneğin Visa, MasterCard ,Troy , Amex

- Kart Tipi
  
  Kartın tipini gösterir. Örneğin kredi kartı,debit kart,prepaid kart

- Alt Kart Tipi

  Kartın klasik, gold, ticari kart vb gibi alt tiplerini gösterir

- Ürün Tipi
  
  Kartın hangi ürün tipinde basıldığı, örneğin standart kart,sanal kart olarak basılıp basılmadığı bilgisi yeralır.

BIN aralıkları örneğin 900000 ile 900100 arası yukarıdaki aynı kombinasyonlu kart tiplerine denk gelebilir. sadece 1 adet BIN bazen yetmeyebiliyor. bu sebeple aralık kullanılıyor.

# Universal Payment Identification Code (yada UPIC)
Sadece Amerika'da bankalarda kullanılan her hesap için özel bir ID'dir.

# POS (yada Point of Sale)

Üye işyerlerinin bankalar ile yaptığı anlaşmalar sonucu temin ettikleri elektronik ödeme alma cihazlarıdır.

# Sanal POS (yada Virtual POS yada VPOS)

Fiziksel POS cihazının sanal yani yazılımsal halidir. İnternet üzerinden yapılan ödemelerde üye işyerinin ödeme isteğini gönderip aldığı yazılımlardır (servislerdir). bunlar bankaların direk kendi API'leri de olabilirken, payment processor'lerde birer sanal postur. fakat gateway sanal pos diildir. gateway sadece sanal pos'lara erişimimizi kolaylaştıran aracıdır.

Hem sanal hemde fiziksel pos'larda; örneğin garanti kartı ile akbank pos'undan ödeme alınabilir. fakat akbank işyerinden komisyon keser.

pos yada vpos birer terminal'dir. her terminal birçok şirket çalışanı tarafından kullanılabildiği için, her çalışana birer user verilir. yani bir terminal altında birden fazla user olabilir.

# acquiring bank (yada acquirer)

Ticaret Bankası anlamına gelir. pos ile müşterilerden para almasını sağlayan sisteme sahip olan, ticaret yapmaya olanak sağlayan bankalara denir. POS sahibi bankaya denir. (tümü aynı kapıya çıkmaktadır)

# Issuer Bank

Kartı düzenleyen bankadır. kartın bankası.

# Satış terimleri

  - ## Ön Provizyon (yada Ön Otorizasyon yada Pre-authorization)

  Karttan para çekmeyen fakat ilgili meblağ için blok koyan işlem. Örneğin 100 TL'lik provizyon gerçekleştiğinde kartınızdan 100 TL çekilmiş olur, kart limitiniz 100 TL düşer, ancak ön otorizasyon yapıldığında ise para gerçekte çekilmez sadece bloklanır ve limitiniz 100 TL düşer. Kullanıcının yeterli limitinin olup olmadığı, güvenlik kontrolü, ön sipariş vb amaçlarla kullanılır. Ön otorizasyon işlemi başarılıysa daha sonradan post-otorizasyon ile para çekme işlemine döndürülebilir. Ön otorizasyon sonrası hiçbir işlem yapılmazsa, bankadan bankaya değişmekle birlikte kabaca 7-10 gün içinde tutar için koyulan blok ortadan kalkar ve kartın limiti tekrar eski haline döner.

  - ## Ön Provizyon kapama (yada Ön Otorizasyon kapama yada post-authorization)

  En basit anlatımla, karttan elektronik olarak para çekme işlemi olarak tarif edilebilir. Para çekme işleminin fiziksel POS veya sanal POS aracılığıyla bankaya iletildikten sonra bankanın olumlu cevap vermesiyle başarıyla sonuçlananmasıdır.

  post-authorization, sadece ön provizyon yapılmış bir siparişe uygulanabilir.

  - ## Sale (yada satış)
  ön otorizasyon ve post-authorization işleminin tek özel işlemde yapılmasıdır. arkaplanda bu işlem olurken ön otorizasyon ve ardından post-authorization yapılmaz. satış kendi başına özel bir işlemdir.

  - ## iptal (yada void) vs iade (yada rebate yada refund)
  iptal ancak gün sonu yapılmamışsa olurken, iade ancak gün sonu yapıldıktan sonra yapılabilir.

  - aşağıdaki işlemler satış işlemi değildir. fakat aynı servislere aynı protokol ile yapılabilen işlemlerdir:
  
    - ## credit (yada bağımsız iade)
    iptal işlemi ile aynıdır. fakat credit, postan yapılan bir işlem olmayabilir. herhangi bir işlem credit yapılabilir. örnek; müşteriden elle ödeme alındı. daha sonra bu işlem iptal edilmek istendi ve pos cihazından yapıldı. işte o zaman credit oluyor. 

    credit bazı bankalarca desteklenemeyebiliyor veya refund/void altında yapılıyor.

    - ## query
    bankalara üye iş yerleri yada fakrlı katmanlardaki servisler query atabilir. burada bonus para sorgulama gibi işlemler yapılabilir.

# Luhn Algorithm

Girilen kart numarasının doğruluğunu tespit etmek için kullanılan algoritma. Kart numarasının 1 hanesini bile yanlış yazdığınızda bu algoritmadan geçemezsiniz. Bankaya ödeme isteği gönderilmeden önce muhakkak Luhn algoritması kontrolü yapılmalı ve yanlış girilen kart numarası için gereksiz yere bankaya istekte bulunulmamalıdır.

fakat bazı kartlar bu algoritmaya göre tasarlanmaz. bu kartları BIN numaralarına göre algoritmaya dahil olup olmadığı bilgisi bulunabilir.

# pan
ödeme işlemlerinde web servisler arası haberleşmede sıkça kullanılan bir kısaltmadır. "primary account number" anlamına gelir. bu işlem yapılacak hesap numarası yada kart numarasını taşır.

# 3d ödemeler

  - ## 3D secure
    Ödeme işlemi için en son onay sayfasında, son kullanıcı bankanın kendi sitesine yönlendirilir ve bankanın ona sorduğu bilgileri girer. Bu bilgiler çoğunlukla sms ve güvenlik sorusudur. hangi bilgiler olacağı bir standart değildir.

    3D Secure bağımsız bir firma tarafından geliştirildi. İlk VISA, "__Verified by Visa__" ismi ile kullandı. Aynı özellik daha sonra diğer firmalarca şu isimlerde kullanılmaya başlandı:

    - American Express - SafeKey
    - MasterCard - SecureCode
    - JCI - J/Secure
    - Diners Club - ProtectBuy

    3D denmesinin sebebi "3 domain"'dan gelmektedir. 3d onayında, 3 bağımsız kuruluşun sunucularından yararlanılmaktadır: issuer (with Access Control Server), acquirer ve interoperability (which is directory server).

  - ## Merchant Plug-In (yada MPI)
    gateway'de yada üye işyerinde olan bir yazılım servisidir/modülüdür. bu yazılım 3d secure akışı olacaksa çalışır. __Bazı__ kart kuruluşları (bu gruba VISA, MasterCard dahildir) üye işyerlerince kendilerine direk istek gelmesini kabul etmiyor (Örneğin troy MPI kullanmıyor). Arada kart kuruluşlarından lisans almış MPI Provider (3üncü parti kuruluşlar) kullanılmasını şart koyuyor.

    sırası ile şu akışları işletmekten sorumludur:
    - Üye işyeri MPI'a tüm bilgileri yollar (ücret, kart no, üye işyeri acquirer'ın verdiği şifreler vs). çünkü artık MPI tüm işleri tamamlayacatır.
    - Kartın 3d işlemine tabi olup olmayacağını "VISA/MasterCard __directory server__"'a sorgular. Bunun için MPI, Directory server'a __VEReq (yada Verification request)__ mesajı atar.
    - Directory Server, card numarasından hareket ederek, kartın bankasının "__Access Control Server (yada ACS)__"'ına bu kartın 3d'ye tabi olup olamayacağını sorgular.
    - ACS Directory server'a dönüş yapar. Directory server ise bu cevabı MPI'd döndüdür. MPI'a gelen bu cevap  __VERes (yada Verification response)__ olarak isimlendirilir.
    - MPI'a gelen bu cevapta 3d olup olamayacağı ve eğer 3d varsa son kullanıcının yönlendirileceği ACS'nin URL'si bulunur.
    - MPI son kullanıcının web sayfasını ACS'ye post işlemi ile __Payer Authentication Request (yada PAReq)"__ payload'ı ile yönlendirir.
    - ACS sms gibi bilgileri son kullanıcıya sorar.
    - Doğru bilgiyi giren son kullancının web sayfası otomatik olarak post redirect işlemi ile MPI'a payloadından __Payer Authentication Response (yada PARes)__ olacak şekilde yönlendirilir.
    - Bu adımda artık ödeme sonuçlanmıştır.

    Birçok banka ACS'yi 3üncü parti olarak kullanmaktadır.

    __Modirum__, Nestpay tarafından kullanılan bir MPI providerıdır. Modirum firması aynı zamanda ACS ve DS yazılımı da geliştirmektedir.

    Payten firması kendi ACS'sini geliştirmiştir. Bu ürünü dışarıya paket olarak sunmaktadır. Türkiye'de her banka BKM'nin ACS'sini kullanırken, sadece akbank payten'in ACS'sini kullanmaktadır (yıl 2019).

  - ## Half 3d (yada merchant-attempted 3-D yada half-secure yada half secure)

    3D secure olarak başlayan fakat bazı sebeplerden dolayı 3D secure olmadan tamamlanan online ödeme işlemleri half 3d olarak adlandırılır.

    örnek sebepler:
    - Kullanıcı alışveriş yapıyor olsun. Üye işyeri sadece 3d destekliyor olsun. son kullanıcın kartı 3d desteklemiyor olsun. Eğer banka buna izin veriyor ise, o anda üye işyeri isterse 3d'siz işleme devam edebilir. Bu duruma half 3d denir. Bazı ülkelerde; böyle durumlarda fraud olma durumunda sorumluluk yasal olarak üye iş yerine geçer. müşteriyi kaybetmek istemeyen üye iş yerleri, riski göze alarak böyle bir çözüme gidebilir.

    - tüm aracılar ve sistemler 3d destekliyor olsun. acquirer kartın bankasına 3d için gitmeye çalıştığında, kartın bankasında anlamsız hata veya cevap alamasın. böye bir durumda acquirer bank 3d olmayan işlem olarak ödemeyi yeniden başlatabilir. issuer bank 3d'siz bu işleme onay verebilir. böyle durumlarda sorumluluk issuer bank'ındır. çünkü 3d platformunu desteklediğini belirtiyor ise; 3d servislerinin sorunsuz çalışıyor olması gerekir. buradaki denetlemeler directory server, kart markaları vs tarafından yapılmaktadır.

    bankalar, güvenlik sebebi ile genelde belli adet sayıda böyle işlemlere izin veriyorlar.

    kart sahibinin "3d secure sistemine kayıt olmama hakkı (opt out)" vardır. bu sebeple anlık bu şekilde işlemlere izin verilir.

    __full 3d__ ise 3d'nin başarılı tamamlandığı akışlara verilen isimdir.

  - ## Non-Secure işlem

    Resmi olarak tanımlı bir terim değildir. bu sebeple herkes farklı anlamda kullanmaktadır. şu anlamlara gelebilir:

    - 3D Secure olmayan ödemeler
    - MOTO olan ödemeler

  - ## 3D secure 2.0
    3D secure 2.0, 1.0 ile temelde benzer mantıkta çalışmaktadır.

    Birçok kart kuruluşunun desteklediği __EMVCo__ firması tarafından geliştirilmiştir. "__EMV® 3-D Secure__" ismi ile geliştirilmektedir.

    1.0'a ek olarak 2 temel fark var:
    - __risk-based-authentication (yada RBA)__ özelliği
      
      İş yeri 3d ekranına yönlendirirken, bankaya birçok bilgi daha gönderecek. Bu bilgiler doğrultusunda banka, yapay zeka yada benzeri bir sistemle son kullanıcıya güvenlik soruları soracak. hatta hiç şüphe çekecek bir durum yok ise, sms gibi hiçbir soru sormayıp direk doğrulayacak. sipariş bilgileri, fatura ve teslimat adresi, browser ve işletim sistemi bilgiler bankaya yollacak.

    - web dışındaki platformlara destek
      
      3d 1.0'da, sms onay sayfasına yönlenme işi redirectional-post işlemi ile gerçekleştirilmekteydi. dolayısı ile web dışındaki diğer platformlar 3d onayı almak istediklerinde webview kullanmak zorunda kalıyorlardı. artık 3d 2.0 ile web servisleri arasında haberleşme ile onay alınabileceğinden web teknolojilerine bağımlılıklar tamamen ortadan kalkmış oldu.

    1.0 'a göre bazı değişiklikler ve yeni terimler:
    - acquirer ve merchant tarafı (domaini) __3DS Requestor Environment__ olarak isimlendiriliyor. bu grubun içinde "3DS Server" ve "3DS Client" var.
    - MPI, __3DS Server__ olarak isimlendiriliyor
    - __3DS Client__ --> web uygulaması yada mobil uygulama yada herhangi bir uygulama
    - VEReq/VERes mesajlarının yeni isimleri vardır: AReq/ARes (Authentication Message)
    - PAReq/PARes mesajlarının yeni isimleri vardır: CReq/CRes (Challenge Message) + RReq/RRes (Results confirmation Message)

# EMV
EMVCo firması tarafından geliştirilen terminal(pos) ve kartın standartlarıdır. Her kart kuruluşu bu standarları kendi ayrı olarak bu isimlerle yayımlar:

- VIS – Visa
- Mastercard chip – Mastercard
- AEIPS – American Express
- UICS – China Union Pay
- J Smart – JCB
- D-PAS – Discover / Diners Club International
- Rupay – NPCI

# card not present transaction (yada CNP yada MO/TO yada Mail Order / Telephone Order yada MOTOEC)
kart sahibi ödeme işlemini internet sitesinden yapmak zorunda değildir. merchant şube yetkililerine ulaşarak ve bilgilerini onlara vererekte yapabilir. buna en iyi örnek call center'lardır. call center'lardan tuşlama ile bilgileri sesli yanıt sistemine aktarabilir. bu şekilde ödemesini tamamlar. bu tarz ödemelere CNP denir. 

# offline ödemeler
- pos cihazı bu şekilde ayarlanırsa ve kart önceden offline'a açık ise bu işlem gerçekleşebilir.
- genelde kartın offline işlem yapabilme limiti çok düşük tutulur
- kullanılm alanları:
  - çok sıra olan ve genelde düşük ücretli alışverişler: örnek restoranlar.
  - pos'un online olamadığı bölgelerde: örnek uçaklarda servis sırasında dağıtılan yemekleri satın alırken.
- bazen hükümet hangi sektördeki firmaların bu özelliği kullanabileceğini de belirleyebiliyor. çünkü bu özellik fraud'a çok elverişli.
- kartın çip'inde pin kaydedilmiş oluyor. bu pin'i pos cihazı müşteriye soruyor. eğer müşteri pin'ini internetten güncellerse, kartı herhangi bir atm'ye taktığında direk olarak kartın içindeki pin de otomatik güncellenir.
- pos cihazı online olduğu anda işlemler bankaya iletilir ve valide edilir.

# Günsonu (yada gün sonu yada end-of-day yada batch yada settlement)
kelime anlamlarınca farklı kapıya çıksalarda, ödeme sistemlerinde hepsi teknik olarak aynı anlamda kullanılmaktadır.

Gün içinde yapılan işlemlerin toplu olarak bankaya gönderilmesi ve onaylanması işlemidir. Günsonu işlemi genelde gece 23:00-24:00 aralığında yapılırken bazı iş yerleri birden fazla günsonu işlemi de yapabilirler. eğer gün sonu yapılmaz ise; hesap kesimi diğer güne kalır. bankalar, normalde paraları hemen müşterilerine (firmalara-pos shiplerine) yollar. fakat günsonu yaılmadıkça bu ödeme tarihleri ileriye sarkar. yani eski tarihli gün sonu alımı diye bir işlem olmaz. eğer 2 gündür günsonu alınamıyorsa, 2 gün sonra alınacak gün sonu 2 günki tüm işlemleri kapsar.

aslında tüm işlemler anlık olarak bankaya bildirilmektedir, fakat günsonu dediğimiz hesap, bir önceki gün sonu işleminden o ana kadar olan işlemlerin tekrar onaylanması için bir ek güvenlik prosedürüdür. teorik olarak hem banka tarafının hemde iş yeri tarafının her gün sonunda çıkan hesabı kontrol etmeleri gerekir.

gün sonu POS cihazları ile bankalar arasında yapılan mutabakat işlemidir. müşteriye(bankanın müşterisi: iş yeri) bir rapor print eder ve bankaya tüm raporu (gün sonunu) tekrar yollar.

gün sonu sadece banka ile müşteri arasında olan bir raporlamadır. banka dışında yapılan işlemleri kapsayamaz. devlet'in muhattap olduğu bir konu değildir.

# gerçek paranın hayat döngüsü

P bankası pos'undan K bankasına (kartın bankası) ait kart ile ödeme yapılsın.
- K bankası komisyonunu keserek (__takas komisyonu (yada interchange commission)__ olarak adlandırılır), P bankasına parayı yatırır.
- P bankası komisyon keserek, iş yerinin önceden tanımladığı hesaba parayı yollar.

Artık K bankası kartın sahibinden esktre tarihi gelince parayı isteyecektir.

Kart hamilinin ödemeyi kredi kartı yada debit kart ile ödemesi üye iş yerindeki bu akışı değiştirmez. Belki süreler değişebilir.

Yukarıdaki işlemlerin süreleri ülkelere/bankalara/anlaşmalara göre çok değişken olabilir.

Takas komsiyonu yüzdeleri türkiyede BKM ve rekabet kurulu tarafından belirlenir. Faklı ülkelerde farklı otoriteler bu görevi üstlenebilir. kaynak: https://bkm.com.tr/faydali-bilgiler/komisyon-ve-ucretler/bankalararasi-takas-komisyonlari/ (archive date: 10/12/2019) aynı sayfanın ingilizcesi: https://bkm.com.tr/en/useful-information/commissions-and-fees/interbank-interchange-commissions/ (archive date: 10/12/2019)

P bankası farklı ülkede ise; kur çevrim farkı zaten kart sahibine yansıtıldığı için ektra bir fark yansıtılmaz. Çünkü yurtdışı para birimine çevrilmiş para zaten bankanın elindedir. Bu para yurtdışındaki ilgili bankaya transfer edilir.

Üstelik bankalar para birimi çevrimi gereken işlemlerde kur çevrimindeki marjı normalden yüksek tutarlar.

# Yurtdışı alışverişlerde alınan döviz kur farkı için alınan komisyonlar

Direk örnek üzerinden gidersek; Amerikadaki bir alışveriş sitesine girdim. orada 100 dolar bir ürünü, garanti bankası hesabım ile satın aldım. bu durumda; o andaki garanti bankası dolar kuru üzerinden, benim kartımdan TL olarak düşülür.

Eğer ingiliz sterlini olan bir siteden 100 ingiliz sterlini olan bir ürünü satın alsaydım;

1- garanti bankası parayı dolara çeviricek komisyonu alır

2- web sitesi default-POS bankasının ingiliz sterlini ile dolar kuru arasındaki komisyonunu hesaplayıp benden komisyon alır.

bu 2 komisyonun toplamı total TL ücreti olarak yansıtılıp benim kartıma yansıtılır.

Bazı online siteler birçok farklı kurda ücret gösteriyor. örneğin ingiliz sitesi, bana hem TL hem de ingiliz sterlini gösterebilir. böyle durumlarda direk TL olduğu için garanti bankası hiçbir ek ödeme bana yansıtamaz. fakat ingiliz web sitesi garanti bankasından ücreti TL olarak alacağı için, ekranda TL ücretini göstermeden TL ücretine komiyonu zaten eklemiş şekilde göstermektedir.

# Taksit (yada installment yada instalment yada hire purchase)

her ülkedeki bankalar bu hizmeti sunmamaktadır.

İşyerine ödemeyi, kart sahibi yerine banka yapar. bankada işyerine ödemeyip peşin yada taksitli yapabilir. taksit; Kart sahibi kartından yapılan ödemenin bankaya ay ay ödenmesi özelliğidir. Banka, kart sahibinden parayı tek sefer yerine aylara bölünmüş olarak aldığı için, buradaki finansmanı genelde üye işyerine yıkar, üye işyeri de müşteriye. Örneğin bir ürünün peşin/tek çekim fiyatı 100 TL iken, 3 taksitli fiyatı 105 TL olabilir. Bu 5 tl'lik far "vade farkı" olarak adlandırılır.

# Peşin fiyatına taksit

taksitli ödemlerde vade farkının alınmayacağını gösterir.

# para puan vs bonus vs çip para

Bankalar kart üzerinden geçen işlemler için üye işyerlerinden komisyon alırlar. Örneğin bankanın, 100 TL'lik bir ödemeyi kart ile yaptığınızda tek çekim için üye işyerinden %2 = 2 TL komisyon aldığını düşünürsek bunun bir kısmını puan/bonus/chip para vb isimle kart sahibine hediye eder. Böylece siz kart ile alışveriş yaptıkça puan toplarsınız ve bu puanları bir sonraki alışverişinizde kullanabilirsiniz. Not: Bankaların komisyon oranları, üye işyerinin işlem hacmine ve tek çekim/taksit tutarına göre değişkenlik gösterir.

Genelde komisyonlar puan olarak müşteriye döndürülsede bu işin yasal zorunluluğu yada rajonu diildir. bir zamanlar bazı kuruluşlar bu komisyona karşılık gelen bir hediye olarak dile getirdi. fakat işin aslı puanlar komisyona bağlı değillerdir. puanlar bankanın yada hizmet satın aldığımız firmanın bir kampanyasıdır. 

örneğin THY para puan yerine mil kavramını kullanmaktadır.

# Merchant (yada Üye İşyeri)

Merchant: tüccar

Bankalar ile sözleşme yaparak, gerekli sorumlulukları yerine getirip fiziksel veya sanal POS sahibi olan gerçek veya tüzel kişi.

# Fraud (yada Sahtecilik)

Dolandırıcını (yada fraudster) başkasının kartı ile işlem yapmaya çalışmasıdır.

# tr:Kart Hamili (yada card holder)

hamil türkçe kelime. anlamı; 'elinde bulunduran'dır.

kart hamili, kartın üstünde ismi yazan kişidir.

# Chargeback (yada Ters İbraz)

İbraz kelime anlamı: birşeyi ortaya koymaktır.

herhangi bir sebep sunarak, işlemin ödememe işleminin yapılmaması yada geri alınması isteğidir. bu istek direk olarak, kart sahibi tarafndan kartın bankasına'ya yapılır. kartın bankası durumu, üye iş yerinin bankasına (acquirer) iletir. acquirer ise üye iş yerine iletir.

# soft chargeback

kart sahibi bankaya başvurur ve kartı üzerinden yapılmış bir işlem hakkında detaylı bilgi talep eder. bu sürece verilen isimdir.

# open banking (yada açık bankacılık)
bu yeni bir akımın ismidir. bankalar artık sadece üye iş yerlerine ödeme API'si değil, özellikle müşteri bilgilerini açtığı API'ler tasarlıyor. bu şekilde; farklı kurumlar müşterilere daha fazla seçenek sunabilecek.

# para transfer terimleri

eft, havale, virman kelimeleri her ülkede  anlama gelebilir. asagıda hem türkiye hemde türkiye dışında ne anlama geldikleri yazmaktadır.

  - ## EFT (yada Elektronik Fon transferi)

  türkiye içindeki bankalara farklı bankaya yapılan para gönderme işlemidir. türkiyede Ancak haftaiçi işlemler 08.30 - 17.30 arasında gerçekleştirilebilmektedir. fakat bazen bankalar istisna tanımaktadır. ve eft saatleri anlık olarak değiştirilebilmektedir. böyle bir esneklik mevcuttur.

  "Elektronik Fon Transfer Sistemi" Türkiye merkez bankasının geliştirdiği bir sistemdir. ve aradaki bu sistem kullanılarak bankalar arası transfer gerçekleşebiliriz.

  türkiye dışında: eft tüm para transferi çeşitlerini kapsamaktadır (bankalar arası, ülkeler arası...)

  - ## Havale

  türekiye içindeki bankalarda aynı bankanın farklı kişilere ait hesapları arasındaki para transferi olarak adlandırılıyor.

  - ## virman

  tdk'ya göre: para aktarımı

  türkiye'de finans terimi olarak kullandıldığında; bir kişinin ülke içinde aynı banka içindeki kendi hesapları arasında yaptığı para trasnferine verilen isimdir.

  - ## wire transfer

  virman hariç tüm transferi kapsar (havale, eft, uluslararası...). tam olarak türkçe karşılığı yoktur. domestic ve international olarak bölünebilir.

  - ## money order

  para transferi anlamına gelir. wire transfer + virman; yani tüm transferi kapsamaktadır. bankaya verilen para transferi talimatı anlamında kullanılır.

  - ## withdraw
  kelime anlamı: geri çekmek
  parayı hesaptan çekme işlemidir.

# Yazar kasa (yada Ödeme kaydedici cihaz)

bir firma, işletme ya da şirkette yapılan satış işlemlerinin kayıt edilmesi için tasarlanmış olan makinelere verilen isimdir. Yazar kasa sayesinde ürünü satın alan tüketici ve aynı zamanda ürünü satmakta olan üretici alışveriş tutarını görebilmektedirler. Yazar kasalar yapılan satışların toplamlarını da farklı yollarla kağıtlara basarak yansıtmaktadır. 

# Yazar Kasa Pos

Yazar kasa pos en kolay tanımı ile kullanılmakta olan yazar kasalar ile posların birleştirilmesi sonucu ortaya çıkan bir cihazdır. Yazarkasa pos sayesinde hem poslardan hem de yazar kasalardan ayrı ayrı işlem yapma derdi ortadan kalkmıştır. Bu sayede ödeme alınan ancak yazar kasadan fiş kesilmeyen alışverişlerin önüne geçilmiştir.

# BKM (yada Bankalararası Kart Merkezi)

BKM bir kurumdur. Altında birçok teknoloji hizmeti mevcuttur. bazı hizmetlere örnekler: "BKM Express", troy...

BKM Express ödemeler için hiçbir kart bilgisi girdirtmez. Direk olarak son kullanıcı BKM sitesine yönlendirilir. Burada ödemeyi yapar ve son durum bilgisi üye işyerine verilir ve kullanıcı üye iş yerinin sitesine yönlendirilir. Kart bilgileri önceden BKM sitesine kayıtlı olmalıdır yada o anda kaydedilir. BKM'de son kullanıcı tüm kart bilgilerinini bkm sistemine dahi girme gereği duymaz. BKM bilgileri otomatik bankalardan çekiyor ve tamamlıyor. örneğin son kullanıcının kart numarasının yarısını girmesi yeterli oluyor. bkm türkiyede devletin destekleiği resmi bir kurum olduğu için böyle bir tölerans tanınmış durumdadır.

bkm express ile ödeme yapmak için üye iş yerinin bkm express'i desteklemesi gerekmektedir.

Kullanıcı, BKM Express sayfasından kartı seçip ödemeye ilerlediği zaman, bkm express kartın bin numarasını üye iş yerinin sunucusuna yollar. cevap olarak üye iş yeri bu bin numaralı kartın hangi vpos'a yönlendirileceğini ve ilgili vpos'un şifresini ve client-id'sini bkm'ye cevap olarak yollar. bkm üye iş yerinin verdiği bu vpos'a istek yapar. dolayısı ile; her bankaya giden servisler ayrı ayrı bkm tarafından yazılmıştır. Türkiye'de çoğu bankanın sanal pos'u nestpay altyapısındadır. dolayısı ile BKM Express son adımlarda vpos'a giderken çoğu banka için nestpay'e gitmiş olur.

# Nestpay'in sunduğu ödeme modelleri
Nestpay yapılan üye işyeri ile anlaşmaya göre üye iş yerinden gelen istekleri yönlendiriyor.

  - ## 3d
    - kart bilgileri sayfası: üye iş yeri
    - 3d sayfası: kartın bankası

  - ## 3d pay
    - kart bilgileri sayfası: üye iş yeri
    - 3d sayfası: kartin bankasi

  - ## 3d pay hosting
    - kart bilgileri sayfası: nestpay
    - 3d sayfası: kartın bankası

  - ## pay hosting
    - kart bilgileri sayfası: üye iş yeri
    - 3d sayfası: 3d sayfası hiç yok

"3d" ve "3d pay" modellerinde 3d onayı sonrası akış değişiklik göstermektedir. "3d" modelde, 3d onayı sonrası ödeme tamamlanmaz. üye iş yerine istek yapılır. üye iş yeri onay dönderse ödeme tamamlanma bilgisi vpos sahibi bankaya (acquirer) iletilir. "3d pay" de ise 3d onayı sonrası işlem direk acquirer bankasına iletilir.

3d onayı ödemeyi tamamlamaz. 3d onayı sadece autahntication anlamında yapılır. "3d" model pre-auth ile karıştırılmamalı. tamamen farklı kavramlardır. bu durumu uygulayan çok fazla üye işyeri olmaz. fakat bazı üye iş yerleri kontrolü ve akışı daha çok kendi yönetmek istediğinde "3d" model kulanır.

# nestpay altyapısı
Nestpay VPOS yazılımıdır. Türkiye'deki çoğu bankanın (garanti ve yapı kredi dışında) kendi vpos yazılımları olmadığı için, nestpay'den bu servisi satın almaktadırlar.

nestpay gelen ödemeyi acquirer bankaya bildiriyor. fakat arkaplanda kart markalarına (örnek BKM, VISA...) ve issuer bankaya istek yapmıyor. Oralara isteği acquirer bankası yapıyor.

Nestpay ürününü türkiye dışında bazı firmalar paket olarak satın alıp, kendi ülkelerinde hizmete sunuyorlar. o ülkelerdeki bankalara ödeme için giden servisler nestpay tarafından yazılmaktadır.

Nestpay ürününü paket olarak satın alan bazı firmalar, kendi sunucuları kullanmıyor. sunucuda çalıştırma görevi ve sunucuların lokasyonu da nestpay tarafından belirlenebiliyor. 

Nestpay yazılımı vpos olduğunu belirttik. üye işyerleri, netspay vpos'a web servis isteği yaparak ödeme işlemini başlatıyor. nestpay bu api isteği ile uğraşmak istemeyen üye iş yerleri için HPP desteği de veriyor.

Nestpay alternatifi sistemler türkiye'de mevcut. Bu sistemler fiziksel pos cihazlarında kullanılıyor. İnternet üzerinden yapılan tüm ödemeler ise nestpay'den geçiyor (yıl 2019).

# MSU (yada Merchant Safe Unipay)
Payten firmasının, üye işyerleri için sunduğu bir hizmettir. Üye işyeri, son kullanıcıyı ödeme sayfası yerine MSU sayfasına yönlendirilir. burada artık MSU birçok ödeme çeşitlerini (alternatif, kredi kartı...) sunmaktadır.

MSU kullanan üye iş yerinin pos'u kendine ait olmalıdır.

MSU aynı zamanda kart saklama hizmeti de vermektedir. son kullanıcı yada üye işyeri birçok kart saklayarak daha sonra bu kart üzerinden işlem yapabilmektedirler.

# paratika
Payten firmasının, üye işyerleri için sunduğu bir hizmettir. MSU ile aynı hizmeti sağlar. fakat pos'lar paratika firmasına aittir. tüm işlemler paratika üzerindenyapılır. bu sebeple BDDK'ya bağlıdır. her bankadan pos almakla uğraşmak istemeyen firmalar, bu ürünü alırlarsa daha hızlı ödeme almaya başlarlar.

# iyzico
processor veya Gateway görevi gören servistir.

# PayU
processor veya Gateway veya vpos(türkiye'de diil fakat diğer ülkelerde bu hizmeti sunuyor) görevi gören servistir. MSU gibi ödeme sayfalarında alternatif ödemelere de yönlendirebiliyor. fakat kendisi bir alternatif ödeme diil.

# Hosted Payment Pages (yada HPP)
Üye iş yeri ödeme sayfasını tasarlamakla uğraşmak istemeyebilir. bu sayfalar ,(eğer destekleniyorsa) gateway yada processor tarafından sağlanabilir. bu tarz sayfalara HPP denir.

# Payment Gateway (yada ödeme geçicidi) vs payment processor

Bu 2 terim teknik olarak birbirinden ayrılsa da, kelime anlamlarınca aynı kapıya çıkmaktadır.

- Gateway: bankalara onaylatma işlemine tek tek gitmek yerine, hepsini yöneten bir servisten yararlanılabilir. bu servis gateway'dir. Bu firmalar BDDK'ya bağlı değillerdir. çünkü sadece yazılım altyapısı sunarlar.

- processor: gateway ile aynı görevi görür fakat bankaya gidilen hesap bilgileri processor'e aittir. BDDK terminolojisinde "Ödeme kuruluşu" olarak tanımlanmıştır ve BDDK lisansı almak zorundadır.

# PSP (yada Payment Service Provider yada Ödeme Servis Sağlayıcı yada Ödeme hizmet sağlayacı)

processor veya Gateway veya alternatif ödeme sunan hizmetlerin tümünün olduğu gruptur.

aynı zamanda __payment facilitator__ (facilitator kelime anlamı: kolaylaştırıcı) terimi de PSP yerine kullanılıyor.

# türkiyede'ki vpos yazılımları

| ürün adı                                              | ürünü sağlayan firma                                                                        |
|-------------------------------------------------------|---------------------------------------------------------------------------------------------|
| POSNET                                                | Yapı Kredi Bankası                                                                          |
| GVP (yada Garanti Virtual POS yada Garanti Sanal Pos) | Garanti Bankası                                                                             |
| NestPay                                               | Payten (eski adı: Nestpay Ödeme Hizmetleri A.Ş), firmanın bağlı olduğu grup firması: Asseco |

# EST
Asseco 2010'da ITD (yada İletişim Teknoloji Danışmanlık Ticaret A.Ş.) ve EST (yada Elektronik Sanal Ticaret Bilişim Hizmetleri A.Ş.) yi satın almıştır.

# Alternatif Ödeme yöntemleri
BKM Express, Google Pay, kapıda ödeme, para transferi ile ödeme, bitcoin ile ödeme gibi ödeme yöntemleridir. kart ile ödemeye ek olarak bazı özellik sundukları yada kart ile ödeme dışında olan ödeme yöntemleri, "alternatif" olarak değerlendirilirler.

# para transferi ile ödeme
bazı üye işyerleri para transferi ile ödeme sçeneğinde banka hesabı IBAN'ı yazıyorlar. buraya manuel olarak son kullanıcı transfer yapıyor. transferin açıklamasına göre üye işyeri çalışanları, paranın yatıp yatmadığını manuel kontrol ediyorlar.

bazı sistemler ise her banka ile ayrı ayrı entegrasyon yaparak, kulanıcıları banka hesaplarına login edip, otomatik para transferi yapmaları sağlanıyor. bu işlem sofort vs giropay vs ideal gibi alternatif ödeme sitemlerinin aynısı.

# PSD (yada Payment Services Directive yada Ödeme Hizmetleri Kanunu)

Avrupa birliği ödeme sistemleri standartıdır. PSP'ler içinde kurallar içermektedir. PSD1 ve PSD2 gibi sürümleri mevcuttur.

# Online cüzdan (yada Digital Wallet yada Dijital Cüzdan)

| Ürün adı                                                             | Hizmeti sunan firma |
|----------------------------------------------------------------------|---------------------|
| MasterPass                                                           | MasterCard          |
| VISA Checkout                                                        | VISA                |
| BKM Ekpress                                                          | BKM                 |
| MerchantSafe Unipay (yada MSU)                                       | Payten/Asseco       |
| Google Pay (yada eski adı:Pay with Google yada eski adı:Android Pay) | Google              |
| Apple Pay                                                            | Apple               |
| BKM Express                                                          | BKM                 |
| Samsung Pay                                                          | Samsung             |
| Microsoft Wallet                                                     | Microsoft           |

Online cüzdanlar kart bilgilerini kendi sunucularında tutar ve bunların yönetimi için GUI sağlarlar. Ödeme yapılmak istendiğinde eğer son kullanıcı online cüzdan seçerse, online cüzdan sunucusundan kartınını seçer. kart seçilince online cüzdan sitesi, bu kart için bir token üye iş yerine döner. üye iş yeri de normal ödeme sürecini başlatır. normal ödemede kart no, ccv gibi bilgiler payment gateway/processor'e yollanırken, burada token yollanması yeterlidir. eğer kullanılan payment gateway/processor token tipini destekliyorsa gidip bu token'a karşılık gelen kart bilgilerini üçüncü parti __Token Service Provider (yada TSP)__'dan alacaktır.

örnek service token servisleri:
- __Visa Token Service (yada VTS)__

Her online cüzdan servisi temelde yukarıdaki mantıkta çalışmaktadır fakat bazı ek özellikler sunan cüzdanlar da olabilir. örnek:
- BKM hem online cüzdandır fakat aynı zamanda payment gateway işini de görmektedir. kullanıcı kartı seçtiği anda ödemeyi de yapmaktadır.
- VISA Checkout üye iş yeri isterse direk kart bilgilerini de döndürebilirken, isterse token'da döndürebiliyor. her 2 seçeneği de sunuyor.
- MSU kendi içinde kendi token'larını kullanıyor. ödeme akışının devamındaki bir servisin MSU'ya özellikle destek vermesi şarttır.

Wallet'lar 2'ye ayrılır:
- __Pass-through Wallet__

  üye iş yerine token dönen servistir.

- __Staged Wallet__

  ödeme işlemini tamamlayan sistemlerdir. üye iş yeri açısından alternatif ödeme gibi gözüksede (çünkü kart üye iş yeri sitesinde kart girdirtmiyor), büyük resimden bakıldığında; kart bilgisini staged wallet'ın kendisi istiyor. bu sebeple alternatif ödeme yöntemi değildir.
  
  BKM Express, ödemeyi tamamladığı için Staged Wallet'a en iyi örnektir.

# GarantiPay
Merchant sayfasında GarantiPay ile ödeme seçildiğinde, web tarayıcı garanti'nin belirlediği sayfaya yönlendiriliyor. garanti'nin mobil uygulaması indirilmiş olması gerekiyor. Buradan sonra garanti sitesinden kart vs bilgileri seçiliyor ve ödeme tamamlanıyor. Daha sonra son kullanıcı tekrar Merchant'ın sitesine yönleniyor.

Yönlenilen Garanti sayfasında QR ile ödeme de mevcut. QR'ı mobil uygulamasından okutan kullanıcı direk ödeme işlemini hızlandırıyor. Web sayfası kullanıcının QR'ı okutup okutmadığını her saniye, garantinin servislerine HTTP isteği yaparak anlamaya çalışıyor.

# para transferi ile ödeme
Online ödemelerde sunulan bir özelliktir. Bu tamamen manuel yönetilir. Üye işyeri bir hesap numarası verir. Bu hesaba müşteri açıklamasında order-id (yada benzeri bir bilgiyi) girip transferi gerçekleştirir. Üye işyeri parayı aldığı zaman siparişin onayını manuel verir.

# MCC (yada Merchant Category Code)
ISO standartlarında numerik deerlerdir. örneğin hava yolları firması, otel firması bir işlem yaptığında MCC değerini ödeme kuruluşuna yollar. bunun birçok sebebi vardır: yasal kontroller, istatistik/analiz toplama, kampanya koşulları doğrultusunda hizmet/servis/indirim sunabilme...

# rfc4112
Electronic Commerce Modeling Language (yada ECML)'dir.

bir eticaret sitesi ödeme işlemi yapacağı zaman ödeme servisine yollacağı parametreler için formattır. her alanın içinde olacak bilginin tipi, boyutu gibi bilgiler de bu standarta dahildir.

buradan detaylı field'lar incelenebilir: https://tools.ietf.org/html/rfc4112

örnek bazı field'lar:

- Ecom_Merchant_Terminal_ID
- Ecom_Transaction_Settle_Date
- Ecom_UserData_BirthDate_Year
- Ecom_UserData_Country
- Ecom_UserData_Language
- Ecom_UserData_Gender
- Ecom_Loyalty_Card_Name
- Ecom_BillTo_Postal_Name_Prefix
- Ecom_ShipTo_Postal_Name_Prefix

# ISO 8583
Bankalar arası haberleşmede kullanılan protokoldür.

nestpay bankaların vpos'u olduğu için, diğer bankalarla nestpay haberleşir, bu sebeple iso8583 protokolünü nestpay yazılımı kullanılır.

Mesaj 3 temel alandan oluşur:
- MTI (yada Message Type Indicator)
  
  4 alandan oluşur. her alanın bir anlamı vardır:

  - versiyon

    0xxx — ISO 8583:1987

    1xxx — ISO 8583:1993

    2xxx — ISO 8583:2003

  - şu bilgilerden biri:

    x1xx — Otorizasyon Mesajı

    x2xx — Finansal mesajlar

    x4xx — Reverse mesajlar

    x5xx — Mütabakat mesajları gibi

  - şu bilgilerden biri:

    xx0x — Request mesajı

    xx1x — Response mesajı

    xx2x — Advice mesajı gibi

  - şu bilgilerdenbiri:

    xxx0 — Acquirer

    xxx2 — Issuer gibi

- Bitmap

  bitmap değerimiz 16'lık sistemde bu olsun: "1A 23 00 41 51 02 F1 10"

  parçalara bölersek:

  - 0x23 = 0010 0011 / 11, 15 ve 16. alanlar dolu

  - 0x00 = 0000 0000 / Burada dolu alan yok

  - 0x41 = 0100 0001 / 26 ve 32. alanlar dolu

  şeklinde devam eder.

  sonuçta buradan çıkardığımız şu field'lar doludur: 11, 15, 16, 26, 32...

- Data Elements

  yukarıda bitmap alanlarında dolu olan alanlar'ın data kısımları buradadır. bazı field'lar aşağıdaki gibidir:

  - Field 1: Bitmap alanının kendisidir.

  - Field 2: Primary Account Number(PAN), kart numarası bu alana yazılır. 19 haneli numeric alandır.

  - Field 3: Processing Code, işlemin ne işlemi olduğunu burdan anlarız. Örneğin bakiye sorgulama için farklı, para çekme için farklıdır. Bankaya gelen mesajlar bu alana bakılıp adreslenir işlemler yapılır ve işleme özel mesajlar üretilir. 6 haneli numeric alandır.

  - Field 4: Transaction Amount, işlem hangi ülkenin para birimiyle yapıldıysa bu alana yazılır. Numeric 12 haneli alandır.

  - Field 6: Cardholder Billing, kart sahibinin kuruyla hesaplanmış tutardır. Örneğin 5 dolarlık işlem için 19,5 tl (Kur 3.9) olarak yani 000000001950 değeri set edilir. Numeric 12 haneli alandır.

  - Field 7: Transmission date & time, işlemi başlatan tarafın işlemi başlattığı anın olduğu alandır. MMDDhhmmss formatındadır.

  - Field 11: STAN, işlemi trace edebilmek için verilen numaradır. Numeric 6 hanelidir.

  - Field 12: Time, hhmmss formatında işlemin saatidir.

  - Field 13: Date, MMDD formatında işlemin tarihidir.

  - Field 18: Merchant Type, mcc kodudur. Numeric 4 hanelidir.

  - Field 22: POS(Point of Service) Entry Mode. Nümerik 3 hanelidir. İlk 2 hanede işlemin nasıl yapıldığını son hanede de şifre kullanılıp kullanılmadığını alır.

    05 : Chipli kartla işlem
    09: E-Commerce işlemi
    90: Manyetik kartla işlem

    Şifre datası var ise sonuna 1 yok ise 0 alır.

  - Field 35: Track 2 datasıdır. İçinde kartın numarası, son kullanma tarihi ve cvv değerleri bulunur.

  - Field 37: Retrievel reference number, işlemi başlatan banka tarafından üretilen koddur. Alfanümerik 12 hanelidir.

  - Field 38: Authorization identification response, işlemin otorizasyon kodudur. Alfanümerik 12 hanelidir.

  - Field 39: Response Code, işlemin cevap kodudur. İşlemin onay alıp almadığı bu alandan kontrol edilir. Alfanümerik 2 hanelidir.

  - Field 41: Card acceptor terminal identification, işlemin yapıldığı terminalin IDsidir. 8 haneli alfanümeriktir.

  - Field 42: Card acceptor identification code, işemin yapıldığı terminalin kodudur.

  - Field 43: Card Acceptor Name/Location, işlemin yapıldığı terminalin yerinin olduğu alandır. ilk 23 hanesi adresi, 24 ile 36 karakterler arası şehri, 37 ve 38. karakterler eyalet kodunu, 39 ve 40. karakter ülke kodunu barındırı. 40 karakterli alfanümeriktir.

  - Field 45: Track 1 datasıdır.

  - Field 48: Additional Data, ek bilgi alanıdır. Additional alanlar işleme özel değerler alırlar.

  - Field 52: PIN Datasının olduğu alandır. PIN doğrulamayı bu alandan yapar.

  - Field 54: Additional Amount ek tutar alanıdır. İşeleme özel değer alır. Örneğin başka atmden bakiye sorgulamada issuerden müşterinin bakiyesine bu alana basması beklenir.

  - Field 55: ICC Data, kart chipli ise EMV Datası bu alana basılır.

  - Field 95: Replacement Amount, kısmi reverse işlemlerinde dolu gönderilir.

# PCI (yada Payment Card Industry)

kartlı ödemelerle ilgili tüm güvenlik kurallarını yayınlayan bir otorite.

# PCI-DSS (yada PCI - Data Security Standarts)

PCI'nin yayınladığı standart dökümanlarıdır.

# Bankacılık Düzenleme ve Denetleme Kurumu (yada BDDK)

türkiyedeki devlete bağlı resmi kurumdur.

# Kişisel Verileri Koruma Kurumu (yada KVKK)
Türkiyede resmi kanun çıkarmakla yükümlü kurumdur. Her ülkede farklı kurumlar tarafından benzer kanunlar çıkarılır. KVKK kanunlarında (tüm dünyada benzer kanunlar vardır) kişisel verileri içeren datalar geriye dönüşümü imkansız olcak şekilde kaydedilmeli yada silinmelidir. Dolayısı ile dikkat edilecek bazı hususlar:
- veritabanı tasarlandığında özellikle ilişkileri bozmayacak şekilde silme mümkün olmalı
- veritabanı backup'ları içerisinden de gerektiğinde data silinebilmelidir (bugün hassas olmayan data 5 yıl sonra hassas olduğu kanunlaştığında, o datayı backup'lardan silebilmeliyiz)
- loglardaki çıktılar pek kolay silinemediği için, ilerde hassas olabilecek bir data kesinlikle loglanmamalıdır

# OTP (yada One Time Password yada Tek Kullanımlık Şifre yada TKŞ)

İşlemlerin doğrulanması için tek seferlik kullanım amacıyla üretilen ve/veya gönderilen şifrelerdir. 3D Secure sürecinde bankadan cep telefonunuza gelen şifre veya internet bankacılığı kullanıcı girişlerinde üretilen anahtar şifreler bu uygulamaya örnektir.

# Uluslararası Banka Hesap Numarası (YADA IBAN)

Her ülkede uzunluğu değişebilir. Türkiyede sırası ile hane bilgileri şu şekildedir:

| hane sayısı | kullanılma amacı   |
|-------------|--------------------|
| 2           | TR                 |
| 2           | özel ayrılmış alan |
| 5           | banka kodu         |
| 1           | özel ayrılmış alan |
| 16          | hesap numarası     |

# Western Union

Uluslarası transferler gerçekleştirebilmek için kurulmuş bir firma. Ek hizmetlerde sunmaktadır. Bu banka işlemleri başka bankalar aracılığı ile de yapılabilmektedir. örneğin; garanti bankası aracılığı ile western union üzerinden direk bir para transferi işlemi yapmak mümkündür.

# moneygram

Western Union'a alternatif kuruluş.

# SWIFT (yada Society for Worldwide Interbank Financial Telecommunication)

direk olarak bankalar arası iletişim/mesajlaşma protokolüdür. bu protokol sayesinde banakalar birbirlerine mesaj atabilir. mesaj atarken birbirleri arasında elektronik para transfer ederler. swift standratları tüm bankalarca ortak bir kuruluş altında belirlenmektedir. her banka bu ortak kuruluşa üye olmak için belli bir ücret vermektedir. swift'in sunucuları da vardır. bu sunucular swift protokolleri ile banaklar haberleşirken bu mesajları denetler/loglar/valide eder.

swift ücreti dinamiktir ve işlem yaptığımız banka ve parayı transfer ettiğimiz bankaya göre değişiklik gösterir. örneğin ING uluslarası bir banka. türkeiyeden almanyadaki ing şubesine para gönderdiğimizde ING kendi içinde bu işi kolayca halleder. fakat farklı türkeiyede ING almanyada HSBC'ye para atacaksanız o zaman iş değişiyor. eğer ING'nşn almanyada şubesi yoksa, ING, almanyada şubesi olan farklı bir bankayı aracı olarak kullanıyor. işte bu aracılar bazen çok fazla olabiliyor. bu da swift komisyon ücretinin artmasına sebep oluyor. 

# BIC kodları (yada Bank Identifier Codes yada SWIFT Kodları)

SWITF işlemi yapmak için gerekli bilgilerden biridir. kod'un formatı aşağıdaki gibidir:

- İlk dört karakter - banka kodu

- Sonraki iki karakter - ISO ülke kodu

- Sonraki iki karakter - yerel kod

- (Eğer verilmişse) Son üç karakter ise - şube kodunu belirtir. (Merkez Şube için XXX kullanılır.)

# Sanal Kart

Sanal kart, gerçek bir karta bağlıdır. Ödeme yapılan işyerine kart bilgisi yerine sanal kart bilgisi verilir. fiziksel olarak varolmadığından, sadece temazsız işlemlerde (telefon, online...) işlemlerde kullanılabilir. Sanal kart gerçek kart üzerinden işlemi yapar. Sadece verilen kart no, son kullanma tarihi gerçek değildir. Sanal kartlar çabuk açılıp kaldırılabilir. Her işlem için bile farklı kart kullanılabilir.

# customer ID
bankalardan musteri ile ilgili herhangi bir bilgi merchant'a hiç döndürülmüyor. müşteri sürekli yeni kart tanımlatıp yeni üyelik açıp sitedeki denemelik dağıtılan ücretsiz servislerdensürekli olarak yararlanabilir.

son kullanıcyı isim soyisim ile de denetleyemeyiz, zira dünyada aynı isim ve soyisme sahip birçok kullanıcı mevcut.

# sofort vs giropay vs ideal

birbirlerine alternatif, birer alternatif ödeme sistemledir. bu sistemlerler her bankayla ayrı ayrı entegrasyon sağlamışlardır. ödeme sayfasında, son kulanıcı bankanın ismini seçer. bankasını seçtikten ve o bankaya login olduktan sonra ödeme işlemi tamamlanır. ödeme sistemi banka ile anlaşmalı olduğu için direk ödemeyi bankadan alır. daha sonra bankadan aldığı parayı, üye işyerine ay sonu (yada anlaşılan süre içinde) yollar.

paypal'dan farklı çalışmaktadırlar. (paypal farklı başlıkta anlatılıyor)

# paypal

- paypal bir alternatif ödemedir. paypal hesabı kartsız da açılabilir, daha sonrada da kart eklenebilir. herhangi tipte bir kart eklenebilir (kredi, debit, prepaid).

- paypal hesabınıza biri email adresinizi biliyorsa para yatırabilir. banka hesabınıza yatan parayı çekmek için paypal hesabınıza bağlandığınız para kartınızdaki default hesaba yollayabilirsiniz.

- yurtiçi ve yurtdışı para transferleri paypal hesapları arasında mümkündür. yurtışı işlemlerde paypal para kesmektedir.

- paypal hesabına kart tanımlanırken, paypal çok ufak bir para çekiyor. aynı zaman kart ekstresinde yada banka müşteri çağrı merkezinden alacağınız paypal özel şifresi ile bu parayı da geri alabiliyorsunuz. bu şekilde spam kartlar paypalda onaylatılamıyor.

- paypal ile alışverişte son kullanıcı bir ücret ödemezken, merchant bir ücret ödemektedir. merchant'ın ödediği ücret işlem hacmine ve paypal ile anlaşmasına göre değişmektedir.

- paypal hizmet sunduğu her ülke için ayrı entegrasyon yapılmıştır. parayı paypal son kullanıcıdan alır, üye işyerine eft yapar. paypal türkiye'deki paratika ile aynı sisteme sahip bir sistemdir. ek olarak bir çok hizmet daha sunar.

- paypalda her kullanıcının "paypal hesabı" vardır. bu bankalarla ilgili bir durum diildir. tamamen sanal bir hesaptır. paypal şirketinin bankakasındaki hesaba eft/havale yapılarak, bu sanal hesaba para yükleyebilir, daha sonra bu hesaptaki para ile alışveriş yapılabilir. bu hesaptan alışveriş yapılırken, paypal şirketinin banka hesabından, üye iş yerine para transferi yapar. son kullanıcıdan ise parayı şu 2 şekilde çekebilir:
  - kredi yada debit kartı ile alışveriş yapıyorsa; üye iş yerinin hizmet sunduğu o ülkede, paypayl kendi anlaşmalı olduğu banka'nın sanal pos'u üzerinden, (paypal'ın merchant olacak şekilde) kendisine ödeme yaptırır. yani son kullanıcı ay sonu ekstresinde paypal'a ödeme yapmış görünür.
  - eğer önceden sanal hesabındaki para ile alışveriş yapılıyorsa; paypal sanal heapatan parayı düşer.

# Temassız Kartlar (yada Contactless Cards)

fiziksel olarak temasız işlem yapabilen kartlardır. genellikle şifre girilmediğinden hızlı (pratik) işlem sağlar. pos cihazınında uyumlu olması gerekir. çalınma ihtimaline karşı temazsız yapılan işlemlerde kartın limiti genelde çok düşüktür.

# FinTech

Financial technology'nin kısaltması olarak sıkça kullanılan bir terimdir. finans teknolojisi sektörüne verilen kısaltma isimdir. birçok konferans, organizasyon bu terimi prefix olarak barındırır.

# tr:slip

ödeme belgesi.

# kartlarının arkasında bulunan imza kısmı

yasal olarak normalde süreç bu şekilde ilerlemelidir: kart ile işlem yapıldıktan sonra 2 adet slip çıkar. biri şirkette kalır diğer ise müşteride. şirkette kalacak slipe müşteri imza atmak zorundadır. İmzanın uyuşup uyuşmadığını kasiyer yapmak zorundadır.

günümüzde PIN istenen kartlarda slip üzerinde imza kısmı çıkmamaktadır. çünkü yasal olarak gerek yoktur. 

pin olmadan yapılan alışveriş kartlarında (örnek temazsız) işlem limiti düşük olduğundan özel yasa ile buna istisna gösterilebilmektedir.

# blockchain

sanal paralarda kullanılan fakat günümüzde diğer platformlarında kullanmaya başladığı bir kayıt(veritabanı) yönetim sistemidir.

Blockchain içerisinde tutulacak veri (blockhain felsefesi gereği), log, para transfer işlemleri kaydı, activity log gibi işlemler olmalıdır (continuously growing list of records). Geri silinmeyecek bilgiler olmalıdır.

sanal paralar üzerinden örnek vererek anlatırsak: herkeste ortak aynı bir veritabanı olmalı. tüm para transferleri bu veritabanına loglanmaktadır. bir kişi diğer bir kişiye para transfer edeceği zaman bu transfer bilgiler tüm herkes tarafından onaylanmalı ve herkesin veritabanında kayıt tutulmalıdır. bu sebeple merkezi olmayan bir sistemdir. daha sonra, yeni bir transfer gerçekleşeceği zaman eski trasnferin bilgilerinin tutulduğu satırların referansı ile yeni transfer bilgileri kriptolanarak kaydedilmektedir. bu sonsuz döngü bu şekilde işlemektedir. her kayıt bir öncekinin referansı olacağı için geçmişten kayıt silmek imkansız olmaktdır. çünkü veritabanı herkeste var. tüm düyadaki kayıtları herkesin makinesindensilmek imkansızdır. temel olarak blockhain yapısı/felsefesi budur.

blockhain dağıtık olmak zorunda değildir. sanal paralar dağıtık olarak kullanıyorlarki güvenlik daha sağlam olsun ve merkezi yönetim olmasın. çünkü Decentralized için ilk adım dağıtık sistem olmaktır.

blockhain için merkeziyetçilik diye bir nitelik olamaz. blockchain sadece veritabanının dataları nasıl tuttuğunun bir özelliğidir. merkeziyetçilik, ona datayı kaydeden yazılımlar/processler için geçerli bir niteliktir. 

blockhainde her kyıt bir önceki kaydın hash'ini tutuyor demiştik. burada her kayıt 1 kayıt olmak zorunda değil. örneğin; bitcoinde her 100 kayıt bir blokta tutuluyor, ve her blok bir önceki blokun hash'ini tutuyor.

sanal para dışında birçok kullanım alanı da vardır:

- oylama sistemi. herkesin oyları merkezi olmayan bir sistemden görülebilir olucak. bu şekilde kimse oy çalamayacak.

- sadece para trasnferi. varolan para birimlerini de transfer etmek için kullanılabilir.

- akıllı anlaşmalar (smart contracts). 2 şirket, yada 1 şirket bi insan, yada birçok şirket bir ortak anlaşma yapacak. bu ortak anlaşma karşılıklı olarka kendi private key'leri ile onaylanıp, blockhain'e programatik ifadeler içeren dosya olarak yazılabilir. bu dosyadaki programatik bilgiler zamanı geldiğinde, proramatik olarak otomatik olarak işleme alınabilir. 

- repository (like git). git'te benzer bir yapı kullanıyor. fakat git'te block'lar yok + tüm kayıtlar direk olarak biribinin ardına ekleniyor. ardı ardına eklenirken, eski kaydın sadece referansı (adresi) tutuluyor. eski kayıtta biri birşey değiştirebilir. git linkedlist list mantığında çalışıyor.

- haber yayın kaynakları. herkes kendi private key'i ile merkezi olmayan sisteme haber atabilecek. bu şekilde herkes haberin kimden geldiğini kesin olarak bilebilecek/güvenebilecek.

- noter işlemleri. iki taraf karşılıklı onayladığı dökümanları blockhain'e yollar. artık ikisinin onayladığı gerçeği kimse tarafından değiştirilemez. eğer bu sistem resmi olarak devlet tarafından tanınırsa, notere gereke kalmaz.

- lojistik takip. bir ilacın bir lokasyona yollanmak istediğini düşünelim. aracı olan her firma private key'lerle her aşamayı onayladığını varsayalım. erişimin hangi adımlarda nerde olduğunu adım adım takip edebiliriz. takibin özelliği her aşamanın garanti olması ve herkes tarafından onaylanarak yapılmasıdır. hiç bir aracı diğer networkte node'lardan onayı almadan bir sonraki aşamaya geçmeyecek. eğer onay alıp geçti ise geriye dönük kontrollerde ben bunu onaylatmamıştım diyemeyecek. bu işi blockchainsiz yaptığımızı düşünelim, yine bir takip sistemi yapabiliriz, fakat geriye dönük log'ların garantisi yoktur. sistemler hacklenebilir/değiştirilebilir.

# Genesis (yada köken)

blockhain veritabanındaki ilk veri kümesine verilen isimdir.

# Sanal para vs Cryptocurrency (yada kripto-para)

sanal para denilince akla digital kart'lara yüklenmiş (kredi kartındaki bonus paralar, sadece bir yemekhane için geliştirilmiş para kartı içindeki para) paralar'dır. fakat kripto-para bunun bir alt kümesidir. kripto-para özel olarak şifreleme tabanlı koruma sistemi ile kullanıma açılmış para çeşididir.

örneğin; dolar yada tl bir kripto-para değildir. çünkü korunmaları şifreleme ile sağlanmamaktadır. bankaya giriş yaptığımızda kullandığımız şifreler para sisteminin değil, bankanın kendi koruma sistemidir.

bitcoin gibi kripto-paralar internet ortamında sunucularla (yada p2p ortamları ile) ayakta tutulan para sistemleridir. Burada para; aktarım, güvenlik gibi konuların tümüyle ele alınır. Merkez bankası olmayan bir sistem ile ayakta tutulabilir.

Sanal paralar virgülden sonra 100-milyonuncu basamağa kadar birimlere ayrılabilme gibi birçok fiziksel paranın olmadığı özelliğe sahip olabilir. Fiziksel paralar gibi sanal paralarında farklı birimleri vardır. BUnlardan bazıları: BTC (Bitcoin) ve Litecoin'dir. Her para biriminin  özellikleri vardır. Para birimlerinin özellikleri aynı zamanda transfer edilebilmesi, güvenliği, log sistemi, paranın üretilme mekanizması, maximum ulaşabileceği sayı, yazılımların kalitesi, dökümantasyonu gibi birçok karakteristiği vardır. para birimi (yada currency) aslında yazılım kullandığı protokol ve bu protokolü destekleyen yazılımlar kümesi oluyor. merkezi sistem olmadığından aslında yazılım tarafının çok önemli olmuyor, çünkü herkes yazılımı override edebiliyor. fakat protokole uymadıkları zaman, o para birimini kullananlarla haberleşemiyorlar.

# bitcoin wallet (yada bitcoin cüzdan)

Wallet = adress + private-key + public-key

Bu verilerin bir uygulama (web, desktop...) tarafından yönetilebilmesi gerekir. Bu uygulama bitcoin client'tır. Eğer üçüncü parti bir web sitesini kullanırsak, o site bizim private key'imizi ve adresimizi biliyor olur.

public key'den adres bulabilen algoritma mevcut. fakat adresten public key bulunamaz.

her bir private key adres ve public key sadece birer adet olacak şekilde birbiri ile ilişkilendirilmiştir.

bitcoin uygulamasını kapatırken sadece kullanılan private keyleri ve adresleri yedek almak yeterli olacaktır.

# (bitcoin için) transaction işlemi

transaction işlemi için cüzdan sahibi, diğer tüm node'lara bu bilgiyi yollar:

> public_key + private_key_ile_imzala("yollayacağı ücret", "hangi adrese gideceği", ...)

network'teki node'lar sırası ile:
- public key ile imzalı olan data'yı açar.
- public_key'den adresi tespit eder.
- bu adreste yollanacak kadar para (bitcoin) varmı diye kontrol edilir
- eğer bilgiler uyuşuyorsa, işlemin kaydı blockhain'e kaydedilir ve sıradaki işlem kontolüne geçilir

Bitcoin'de block block kayıtlar biriktirilip hash alınıyor. Bu konu başka başlıkta anlatılıyor.

# (bitcoin için) key pool

ilk cüzdan açıldığında alınan 100 adet private ve public key'e verilen isim

# passphrase

türkçe karşılığı paroladır. fakat passphrase; parola amaçlı kullanılan; kelimlerden (human-readable / mnemonic phrase) oluşan uzun bir string anlamına gelmektedir.

# (bitcoin için) HD wallet (yada Hierarchical Deterministic wallet)

genelde kullanıcılar birden fazla cüzdan kullanır. bu sebeple HD çözümüne gidilmiştir. kullanıcı bir passphrase yazar. bunu yedeklemesi yeterli olacaktır. passphrase ile bitcoin uygulaması (hd wallet desteği olan uygulama) bir master key yaratır. son kullanıcı yeni bir adres istediğinde; uygulama bu key'in sonuna +1 ekler. bu yeni key artık yeni oluşturulacak cüzdanın private keyi olark kabul edilir ve yeni cüzdan açılır. +2 eklenince başka bir cüzdan daha açılır. son kullanıcı sadece passphrase ve kaç adet cüzdan oluşturduğunu bilmesi yeterli olacaktır.

HD cüzdan bitcoin protokolünün verdiği bir özellik değildir. onun önündeki yazılım katmanında son kullanıcıya sunulan bir kısayol işlemidir.

HD cüzdan kullananlar genelde işlem yapıp cüzdanı bir daha kullanmazlar. çünkü cüzdanlar çok hızlı/pratik oluşturulabiliyor. bu şekilde takibi zorlaştırıdğı için gizliliği arttırır.

# (bitcoin için) adresin otomatik değişmesi

online cüzdanlar yada bazı client uygulamalar cüzdandan bir işlem gerçekleştirince otomatik olarak adresi değiştirir. bu şekilde gizliliği arttırmış olur.

# (bitcoin için) seed

türkçe kelime anlamı: tohum.

Seed kelimesi HD wallet'larda başlangıç noktasını temsil eder. yani passphrase'e karşılık gelir.

# (bitcoin için) watch-only address

adress'ler yedeklenirken public key'i de saklamak iyi olur. public key ve adress bilgisi ile cüzdan takip edilebiliyor. dolayısı ile güvenmediğiniz bir bilgisyarda sadece public key'i bir porgrama import ederek takip edebiliyoruz. HD cüzdanlar için bir 'master public key' vardır. bununla tüm alt cüzdanlar takip edilebilir. fakat sadece bir tane alt-cüzdan takip edilmek isteniyor ise sadece o alt-cüzdanın public key'ini export etmek gereklidir.

# (bitcoin için) Multisignature wallet (yada multisig wallet)

bu özellik bitcoin tarafından native olarak desteklenir. M-of-N transaction'ların yapılabilmesi sağlanır. birden fazla kişinin cüzdan üzerinden işlem yapılabilmesi için onaylaması gerekir. bu kişilerin her birinde birer private key bulunur. kullanım senaryosu örneği; bir şirket elemanının şirket bitcoin özel hesabındna işlem yapması için onay isteyebilmesi gerekebilir. 

# (bitcoin için) two-factor authentication wallet

bu özellik sadece Electrum uygulamasında bulunuyor. multisig teknolojisine dayanıyor. üçünkü parti bir firmada bir anahtar daha tutuluyor. böylece son kullanıcı işlem yapmak istediği zaman o firmanında onayı gerekiyor.

# "Replace by Fee"

bu bir transaction özelliğidir ve transaction yapılmadan önce set edilmiş olmalıdır. bu özellik var ise; transaction onaylanmamış ise, kullanıcı "fee (yada ücret)" 'i arttırabilir dinamik olarak. bunun için electrumda preferences'tan transaction'ı yapmadan önce bu özelliği açmak gerekiyor.

mining yapan insanlar fee'yi fazla veren transaction'lara öncelik verirler.

transaction başlatılmadna önce istediğiniz miktarda fee verebilirsiniz. 

çok fazla uzun mem-pool'da kalan ödemeler bir süre sonra iptal edilir ve hesaba geri yansıtılır (transaction geçersiz sayılır).

# CPFP (yada child pays for parent)

az fee'li bir transaction düşünelim id'si A olsun. A hiçbir şekilde alıcıya ulaşamıyor. sürekli beklemede. alıcı bu parayı alabilmek için şu yolu izler: onaylanmamışta olsa kendi cüzdanı üzerinden bu paranın bir kısmı ile bir harcama yapmaya kalkar. bu yeni transaction'ın fee'sini yüksek tutar. miner'lar bu transaction'ı onaylamak isteyecek çünkü yüksek fee'si var. fakat bu adamın cüzdanındaki parayı harcayabilmesi için öncelikle diğer (child) işlemin onaylanması gerekmektedir. CPFP desteği olan miner'lar bu tarz transaction'ları görebiliyor ve destek veriyor. dolayısı ile önce child işlemi (id'si A olan işlemi) onaylamak zorunda kalıyor.

# Satoshi Nakamoto

bitcoin'in yaratıcısı olarak bilinen kişi/grupların kullandığı isimdir.  daha öncesinde blockhain ile ilgili çalışmalar vardı. fakat Satoshi makalesinde blockchain mantığını ilk defa proof-of-work özelliği ile birleştirmiştit. bu makalesinde bitcoin implementasyonuda ortaya çıkarmıştır.

# Satoshi client (yada bitcoin-qt yada bitcoin core)

Referans bitcoin client uygulamasıdır. Tüm blockhain zincirini indirebilmemizi sağlıyor. Piyasadaki diğer bazı client'lar tüm zinciri indirmeden de para transferi işlemleri yapabilmemiz sağlıyor. Bu zincirin localde olmasının tek avantajı: para transferi doğrulama işleminin sadece başkaları tarafından yapılmayacak olmasıdır. Yani local'deki zincirde de onaylanması gerekecek işlemin. 

# Satoshi

bitcoinin ufak bir miktarına verilen özel isim.

1 Satoshi = 0.00000001 ฿

0.001 ฿ = 1 mBTC

0.01  ฿ = 1 cBTC (bitcent)

# (bitcoin için) Simplified Payment Verification (yada SPV)

Tüm zinciri local'e indirmeden yapılan transfer işlemidir. Daha güvensizdir. BU transfer işleminin bir niteliğinin ismidir. her desktop uygulama spv desteklemeyebilir. bazıları zorunlu olark full-node olacak şekilde tasarlanmıştır.

# (bitcoin için) masaüstü uygulamaları

https://bitcoin.org/en/wallets/desktop/linux/ Sayfasında her client için güvenlik açığı yaratabilecek parametreler tek tek listelenmiş durumda.

-- Linux için uygulamalar:

```
isim            açık   full-node portable   güvenlik durumu
                kaynak required             

bitcoinknots    evet    evet     evet

bitcoin core    evet    evet     evet

BitGo           evet    XX        XX        key uzak suncuda oluşturuluyor. güvensiz.

Bither          evet    XX       evet       güncellenmiyor + arkasında community yok. güvensiz.

Electrum        evet    hayır    nix-app

GreenAddress    XX      XX        XX        key uzak suncuda oluşturuluyor. güvensiz.

ArcBit          XX      XX        XX        key uzak suncuda oluşturuluyor. güvensiz.

Armory          evet    evet     nix-app

mSIGNA          evet    XX        XX        arkasında community yok.

XX -> yazılmadı
```
 

# (bitcoin için) muhasebe defteri (yada ledger)

Her işlemin tutulduğu veritabanı zinciri. Blockhain tabanlı.

# (bitcoin için) Mining (yada madencilik)

Blockhain zinciri block'lardan oluşuyor. Bu bloklar kendi içinde belli adet transaction işlemi listesi barındırıyor. Yeni bir blok eklenmesi için birinin (bir client node'un) işlem yapması gerekmekte. Işte bu işlem karşılığında bitcoin hediye edilmekte. Bitcoin hediyesi node'un tanımladığı cüzdana para transferi olarak geçmekte.

Yeni bir blok oluşturmak için eski bloğun hash'ini almak gerekiyor. Tabi bu kolay olduğundan ekstra olarak; random bir sayı ile bir rakam tutturulmaya çalışılıyor (bunun zorluk derecesi her geçen gün artıyor). İlk bulan node bitcoin hediyelerini kendine transfer ediyor. Ve yeni bir blok oluşup zincire ekleniyor. Artık her para transfer işlemi bu bloğa kendini kaydediyor. Yani mining olmazsa hesaplama olmaz ve yeni blok zincirleri eklenmez. Hash alma işlemleri güçlü CPU istediği için mining; bir teşvik olarak sunulmaktadır.

hash alma işlemi zaman alıcı olduğundan, bu süre içerisinde yapılan trnsferler onaylanmadan bekletilmiş oluyor. para transferi tamamiyle onaylanmadan, gönderimi tamamlanmışpara transferleri her zaman risklidir.

# Proof of Work (yada PoW yada Proof-of-Work)

mining işlerinde kullanılan temel sistemin felsefesinin genel adıdır. bitcoin'de şu şekilde işliyor:

```java
String calculateHash() {

    String newHash = sha256("zincirin son bloğunun hash'i", date, data, randomString() );

    return newHash;
}

if( newHash.startsWith("000") ){

    // newHash is valid ! 

} else {
    calculateHash();
}
```

Yukarıdaki kodda; hash'in ilk 3 karakteri sıfır mı diye kontrol ediliyor. zorluk derecesi arttığında belki ilk 4 karakteri kontrol edilecek. gibi...bunun gibi birçok kontrol algoritması var.

mining yapanlar ancak deneme yanılma yöntemleri ile mining yapabiliyor. yani; 'Proof of Work' felsefesine göre; çalıştıklarını kanıtlamış oluyorlar.

bitcoin'de zorluk derecesi her geçen gün daha zorlaştırılıyor.

# nonce
kelime anlamı: şimdiki zaman

örnek kullanım: for the nonce --> 'şimdilik' anlamına geliyor 

nonce bilgisayar bilimlerinde "number used once"'ın kısaltmasıdır. yani bir kerelik kullanılacak sayı anlamına geliyor. örneğin; mining yaparken yukarıdaki örnekte randomString() yerine 'nonce' değeri kullanılır.

# mining pool
Madenciler güçlü bilgisayarlara ortak yatırım yapıyor. Bulunan bitcoinler ortak paylaştırılıyor. bu diğer para birimleri içinde geçerli bir durum.

# Equihash
Proof-of-Work (yada proof of work yada PoW) tabanlı bir mining algoritmasıdır. 

piyasada sadece firmalar ASIC alabiliyor ve halk alamıyor. bu sebeple piyasa hakimi sadece firmalar oluyor. bu da merkeziyetçi sistem demek oluyor.

mining yapan ASIC'ler maliyet olarak içlerinde memory bulundurmuyorlar. eğer memory bulundururlarsa gerçek bir ekran kartından farkları kalmıyor. bu sebelpe tüm piyasadaki ASIC'leri kullanılmaz hale getirecek yeni hash algoritmaları geliştirildi. bu algoritmalar bi nebze hızlı erişimli memory'ye ihtiyaç duyuyor. ASIC'lerde memory olmadığından artık işin içine halk ta ekran kartı alarak girebiliyor. işte bu tarz algortmalara Proof-of-Work tabanlı algoritmalar deniliyor. Equihash bir algoritma ismidir.

# proof of stake (yada PoS yada proof-of-stake)
stake kelime anlamı: menfaat, destek, çıkarcılık

pow'a alterntif bir metoddur. pos'ta elinizde bulunan coinlerin miktarı ve bu coinlerin yaşı (en son transfer edildiği tarih) önemlidir. bu değerler arttıkça kazandığınız ödül miktarı da artmaktadır.

Pow'a göre daha az makine gücü (elektrik gücü) istemektedir.

# Bizans General Problemi (yada Byzantine fault yada interactive consistency yada source congruency yada error avalanche yada Byzantine agreement problem yada Byzantine generals problem yada Byzantine failure)
yazılımlarda merkeziyetsiz sistemlerde haberleşmelerde kaynaklanan sıkıntılardır. bu sorunları çözen algoritmalara genel olarak "Consensus algoritmaları" denir.

Problemin ismi şuradan gelmektedir: 4 bizans generali farklı noktalarda bir şehri kuşatmış durumda. şehri ancak sabah her biri dört bir yandan saldırıya geçerek kazanabilecek. peki birbirlerine nasıl haber verecekler. bir haberci yollanır. fakat haberci yolda ölebilir. bu sebeple haberci mesajı ilettiğine dair, karşı generalden getireceği şifreli haber ile ancak mesajın iletildiğinden emin olabiliriz. şifreli mesaj dönmesi şart, çünkü habercide yalan söyleyebilir. peki generaller farklı mesaj gönderirse veya haberci için zaman yetmezse? peki ya diğer generaller farklı bir mesaj iletirse? peki ya hain generallerin yapacağı diğer aksiyonlar? işte bu sorunlar merkeziyetsiz sistemlerin iletişim problemleridir. aynı problemler yazılımlarda çözülmeye çalışılmaktadır.

# Karıştırma (yada Mixing yada Çamaşırhane yada Laundry) Servisleri

Bir servis bu işi yapabilir. Herkesten bitcoinleri aynı hesaba yollar, daha sonra herkese geri yollar. Sadece işlem karışıklığı yapılır ki takip zorlaşsın. Fakat bu servisin paraları geri iade etmemem durumu da söz konusu olabilir.

# %51 atağı vs double spend

%50'nin üzerinde onaylama gücüne sahip node'lar bir çok farklı kötü amaçlı işlem yapabilirler. buna bir örnek "double spend" olarak bilinen bir ataktır.

bu hack yönteminin maliyeti o parayı kazanmaktan daha fazladır. Ve bu maliyeti ortaya koyabilecek bir otoriter, tüm blochian platformunun sisteminin güvenliğinin riske girmesi durumunu göze alamaz. Çünkü bu kadar node'u çalıştıracak sisteme büyük para yatırmıştır, bu sistemle para üretmesi daha mantıklı olacaktır.

# double spend

Tüm node'ların yüzde %50 sine sahip bir otoriter sunucu, P bloğundaki public olan ana blockhain üzerinden kendisinin private key'ine ait cüzdanlardan alışveriş yapar. üstüne V adet yeni blok yaratılmasını bekler. Bu arada P noktasından itibaren kendisi blok oluşturmaya devam eder fakat bunu kimse ile paylaşmaz. Yeni transcationlar zaten public yayımlanıyor, bunları kendi zincirine eklemeye de devam eder. Piyasadaki işlemlere ekstradan kendisi de kendi ucuz cüzdanları ile gerekesiz işlemler yapar, ve son durum şu olur:

Public blockhain uzunluğu  : P + (spend money as normally) + V

Private blockchain uzunluğu: P + V + (send-resend money only to create new blocks)

Bu durumda artık private'ı public ile yer değiştirtir. tüm dünya %50'den daha az oylamaya sahip olacağı için bu hacker sunucunun dediği blockhain'i doğru olarak kabul edecektir. çünkü uzun olan ve valid olan blockchain kabul görmektedir. işlemler ortada yoktur. fakat örneğin; hacker kendine bu parayla buzdolabı çoktan almış olabilir. buzdolabı firması işlemin iptal olduğunu görecektir fakat buzdolabını teslim etmiş olacaktır.

double spend'i engellemenin en iyi yolu herkesin bir transaction'ın onaylandığını kabul etmesi için minimum blok sayısı arttırması önerilir.

# (bitcoin için) transfer onayı

bir para transferi tüm node'lar tarafından onaylanır. işlemin herkese gitmesi çok çabuk olur, fakat herkesin bunu onaylaması yani; bloğa işlemesi gerekmektedir (yeni bloğun oluşturulması ve birinini hediye bitcoini almış olması). bu işlem zaman alabiliyor. herkes onayladı, yeni blok oluştu. bu durumda, o transaction'a 1 confirmation geliyor. daha sonra dünyada birçok trasnfer oluyor. bunlarda yeni bloklar oluşturuyor. her blok sonrası bizim transfer +1 confirmation alıyor. yani daha garantilenmiş oluyor. çünkü blockhainde her eklenen blok zincirin kırılmasını/hacklenmesini daha da zorlaştırıyor. bu yüzden ne kadar çok onay alınırsa o kadar kesin işlem olmuş demek oluyor.

# (bitcoin için) cizdana/adrese bağlı para toplamı (balance) nerede tutuluyor

blockhain mantığında normalde tüm database baştan sonra kontrol edilmeli, ve ilgili adres üzerinden yapılmış alışverişler kontrol edilmelidir ve hesaplanmalıdır. bitcoinde de bu şekildedir.

unutulmamalıdırki; ilk para girişi birilerine mutlaka başkasından yapılıyor. yeni bitcoinler ise mining yapanlara hediye olarak transfer şeklinde geliyor.

# (bitcoin için) mempool (yada unconfirmed transactions pool)

blockhain'de bloğa işlenmemiş transaction'ların totalinin bulunduğu memory'dir.

# altcoin

alternative bitcoin'in kısaltmasıdır. bitcoin haricinde olan tüm kriptoparalar grubuna verilen ismidir. sadece halk dilinde kullanılan bir terim olduğundan; blockchain altyapısını kullanan ve içinde gömülü para modülü barındıran fakat çok farklı amaçla kullanılan sistemlerin (örneğin Ethereum) altcoin olup olmadığı tartışmaları ile karşılaşılabilir.

# (bitcoin için) transaction fee

para transfer ücretleri. bitcoin miner'larına hediye olarak verilmektedir.

# (bitcoin için) hashrate

belli bir süre içinde bir node tarafından onaylanan blok sayısı, o node'un hasrate'idir.

# (bitcoin için) input vs output

her transaction en az bir input ve bir output içerir. input yollanan paranın bitcoin protokolündeki formatıdır. output ise her paranın adrese girişi için gerekli formatıdır. bir input birden fazla adrese output olarak gidebilir. bu yüzden 1 input 2 output olabilir. örneğin ahmet tek bir input ile 5 btc'yi, ayse ve fatma'ya 2 ve 3 btc olarak yollamaktadır. benzer şekilde birçok input tek bir output yaratabilir.

# yeni bitcoinler nasıl üretiliyor

mining yapanlara transaction fee'lere ek olarak yeni hediye bitcoin transfer edilir.

# ASIC (yada Application Specific Integrated Circuit)

Sadece bir işi yapmak için tasarlanmış cihaz. örneğin; "ASIC mining"; son kullanıcıların kullandığı masaüstü bilgisayarların üzerine yazılım kurarak değilde, sadece para basma için cihazlar kulanılmasıdır. piyasada bir para birimi için ASIC yok ise; mining yapan kişiler normal son kullanıcıların kullandığı ekran kartlarını kullanmakta ve piyasada ekran kartı sıkıntısı yaşanmaktadır.

# Ponzi Oyunu (yada Ponzi Düzeni yada Ponzi Scheme)

yatırımcılara kendi paralarından geri dönenle veya sonraki yatırımcılardan gelen paralarla ödemenin yapıldığı bir dolandırıcılık yöntemidir. toplam kar edilen pra üyelere bölüştürülür. ortada bir ürün satılıp alınmadığından dolandırıcılık yöntemi olarka adlandırılır fakat bazı ülkelerde illegal bir durum teşkil etmeyebilir.

# Saadet zinciri (piramit zinciri yada pyramid scheme)

her yeni üye referans ile sisteme katılır. bu sistemde her üye olan kişi, referansına pra kazandırır. bu tarz kazanç sağlayan sistemlere verilen isimdir. dolandırıcılık olmak zorunda değildir.

# MMM

BU özel isimde bir şirket ponzi şeması mekanizması ile para kazanmıştır. Daha sonra firma "MMM GLobal" olarak isim değiştirmiştir.

# Ethereum (ETH) vs Ethreium Classic (yada ETC)

Ethereum'da çıkan güvenlik açığı sebebi ile para birimi ikiye bölündü ve eskisi ETC olarak adlandırıldı.

# ether

Ethereum'un kendi içinde kullandığı para birimi.

# Ethereum Virtual Machine (yada EVM)

Ethereum protokolünde akıllı kontraatlar vardır. bu kontraatlar programatik dilde text dosyasında, olabildiğince son kullanıcında anlayacağı dilde yazılır. bu kontraatlar blockhain'e atılır/kaydedilir. daha sonra kontraatlar gerçekten EVM içinde tüm dünyadaki node'lar tarafından işleme alınır. her işlem (kod satırı adımı) sonrası kontraatın son durumu blockhain'e atılır/kaydedilir. Ethereum kontraatında işlem başarılı olursa, yada başarısız olursa kimin hangi adresten ne kadarlık "ether" parası aktarılacağını belirten kodlarda mavcut. işte ether parası burada kullanılıyor. birilerinin işlem başlatmadan önce ether hesabında ether olması gerekmektedir.

# (Ethereum için) mining

bitcoinde olduğu gibi işlemler blockhainlere kaydediliyor. blokların birleştirilme işlemi mining'ciler tarafından yapılıyor. fakat contract'lar herkes tarafından işletiliyor.

# (Ethereum için) gas (yada gaz)

her contraat içindeki işlem belirli bir işlem gücü gerektiriyor. bu işlem gücüne verilen tkenik isim gas'tır. eğer kullanıcı contraat işlemini yaptıracaksa, hazırda contraatın gerektiridiği gaz miktarı kadar ether para birimi olmalıdır. bu gas'ların parası hesaptan alınıyor ve miner'lara hediye olarak veriliyor.

# (Ethereum için) ENS (yada Ethereum Name Service)

İnternet DNS mantığında; her adresi bir domaine bağlayabiliyorsunuz.

# (Ethereum için) fast vs full vs light 

bu mod çeşitleri ile ethereum client yazılımları run edilmektedir. full node tüm blockhaini indiriyor ve tüm işlmeleri baştan sona gerçekleştiriyor. fast mode; tüm blockhaini indiriyor ama sadece artık bu zaman zarfından sonraki contract'ları yürütmeye başlıyor. light mode hiç veritabanını indirmiyor.

Light mode sadece metadata indiriyor.  metadata boyutu 2017 sonları itibariyle ortalama 200 MB.

# Mist (yada Mist Browser yada Ethereum Wallet) vs Geth (yada go-ethereum)

Mist Ethereum için electron bazlı GUI uygulaması. geth ise Ethereum için komut satırı uygulaması. Mist arkaplanda "geth" 'i kullanarak işlemleri yapıyor. eğer istenirse farklı bir implementasyona da referans edip çalıştırılabilir.

# Ethereum classic

etheriumda bir DAO aracılığ ile fon toplanmıştı. Bu DAO içerisinde bir açıktan yararlanıldı ve bir hacker tüm fonları kendine aktardı. Hacker aslında Ethereum'u hacklemedi. DAO proramının içerisinde bir açık vardı. bundan yararlandılar. Ethereum projesi hackerların saldırdığı bu DAO oluşturuncaki kısımdan itibaren veritabanını klonlayarak hard fork yaptı. Bu hard forku kullanmayan ve hackelenen database ile devam eden proje ise "Etherium Classic" olarak adlandırıldı.

# Consensus

türkçe kelime anlamı:  konsensüs, Mutabakat, uzlaşma, fikir birliği

blockhain herkeste senkron veritabanı oluşturması için herkesin bribiri ile anlaşmış olması gerekmektedir. bu durum bir Mutabakat'ı zorunlu hale getiriyor. Mutabakat yapmayan gruplar kendileri birleşerek fork yaratabilirler.

# merkle kök değeri

bir bloğun hash'i alınıyor. bu hash'e verilen isimdir. bu hash henüz içinde bir önceki bloğun hash'ini bulundurmuyor.

# block header vs block body

block body; veritabanının data kısmını barındırmaktadır. block header ise aşağıdakilerden oluşur:

- timestamp

- bir önceki bloğun hash'i

- merkle kök değeri

# üst blok

bir önce yaratılmış bloğa verilen isimdir.

# en uzun zincir

blocklar üstüste eklenirken, bu bloğun tüm diğer node'lar tarafından onaylaması beklenir. ağdaki tıkanmalar, spam olarak onaylamayanlar, onaya geç dönüş yapanlar, gibi etkenlerden dolayı blockhain ağı bazen  zincirleme onaylandığı görülür. fakat protokol gereği her zaman en uzun olan zincir doğru olarak kabul edilir. "ikincil zincirler" ise daha kısa kalmış fakat bazı node'lar atarfından onaylanmış bloklardır. fakat ikincil zincirlerde artık geçersiz sayılacaktır. bir de orphan zincirler var. bunlar ise kimse tarafından onaylanamamış zincirlerdir.

# Bitcoin Cash

Bitcoin 1 agustos 2017'de hard forklandı. bitcoin cash isimli yeni fork olarak ortaya çıktı. para birimi kısaltması: BCH.

# SegWit vs SegWit2x

ikiside bitcoine gelen ayrı yazılım/protokol updateleridir.

SegWit güncellemesi bitcoinin büyük toplulukları topladı. SegWit'i beğenmeyenler "bitcoin cash" topluluğunu oluşturdu.

SegWit2x güncellemesi SegWit sonrası Kasım 2017 gibi gelmesi planlanıyordu. Fakat daha sonra iptal edildi.

# bitcoin gold

Bitcoin(SegWit dalından) 24 Ekim 2017'de hard forklandı. yeni para biriminin ismi bitcoin gold. para birimi kısaltması: BTG.

# Initial coin offering (yada ICO)

yeni bir coin protokolü/para birimi oluşturulmak istendiğinde bir maliyet söz konusudur + birilerinin elinde bu para biriminden olmalıdır ki para akışı meydana gelsin. bunun için (initial para için) bir fonlama başlatılır. eğer fonlama başarılı geçerse (bekllenen minimum para toplanırsa) bu para birimi oluşturulur ve herkese ilk başlangıçta o para birimi fonladıkları oranla dağıtılır. fonlamaların çoğu etherium gibi kontraatlardan yapılır. eğer para birimi canlıya geçerse kullanıcılara token verilmek zorunda kalınır. eğer proje canlıya geçmezse fonlamadan toplanan paralar geri iade edilir

para birimleri ilk çıktığında genel olarka ya tüm para birimlerini merkezi bir firma elinde tutar ve bunların satışından para kazanır. Dolayısı ile para biriminin artmasını onlarda desteklemektedir ve bunun için sürekli yazılım güncellerler. örneğin; ripple. Bazı firmalar ise yeni para birimi oluşturuğunda ise Genesis bloğunda belli miktarda para birimini kendilerinde tutarlar. BU para birimlerini satarak kendileri de kar ederler. Yazılımı geliştirmek için buradaki paraları satarak para kazanırlar.

# (bitcoin için) cold wallet vs hot wallet

istenirse private key'i hiç cüzdan uygulamasına vermeyebiliriz. transaction yapmak için output dosyasını oluştururur. sadece oluşturma aşaması için bir süreliğine uygulamaya veriririz. bu şekilde yükske güvenlik sağlanır. bu yönteme cold wallet adı verilir. diğer tüm yöntemler hot wallet'tır.

adres takip işlemleri zaten publickeyle yapılabiliyor.

# ripple

"ripple Labs" bir şirket. geliştirdikleri uygulama ile herhangi bir digital varlığı şirketler arasında transfer etmeye yarayan yazılımı sunmaktadırlar. transfer edilecek bu varlıklar istenirse XRP para birimi olabilir. XRP, Ripple networkü tarafından native desteklenen bir varlıktır. XRP isteğe bağlı kullanılan bir para birimidir.

"Ripple connect" temelinde blockhain'e benzer bir sistem bulunmaktadır.

# XRP

mining kavramı mevcut değil. bütün XRP'ler baştan oluşturulmuş. "Ripple Labs" bütün XRP'leri elinde bulunduruyor. Her XRP trasnferi sonrası, XRP'ler yokoluyor. tekrar transfer edilemiyorlar.

XRP, Ripple ağında bir token diyebiliriz. Ripple ağından transfer yapan bankalar kendi yada farklı tokenler tercih etmektedirler. Etherium da da benzer bir durum var. etheriuma entegre firmalar, etheriumda kendi tokenları ile bilgi alışverişi yapıp kayıt saklayabilirler. fakat etheriumda işlem yaptırma ücreti olarak Ether para birimi kullanmaları şarttır. XRP'de de transfer için XRP masrafı şarttır. Fakat transfer edilen bilgi bankanın kendi token'ı olabilir.

# litecoin

LTC para birimini kullanan sanal paradır. sadece bitcoinin bazı dezavantajlarını gidermek için ortaya çıkarılmıştır.

# Etherium alternatifi turing-complete sistemler

```
Proje Adı   Native Para Birimi
------------------------------
QTUM           ?

NEM            XEM

NEO            NEO

EOS            EOS
```

# Gizliliği arttırmak amaçlı kurulan para birimleri

```
Proje Adı                          Para Birimi
----------------------------------------------
Dash (yada eski adı: Darkcoin)       DASH

Zcash                                ZEC

Monero                               XMR

Bytecoin                             BCN
```

# Tether

Merkezi bir blockhain tabanlı para birimidir. Piyasada alış/satış ne olursa olsun para biriminin değerini sürekli 1 dolar olarak sabitlemiş durumdalar.bu sebeple herkes dolar kullanacağına tether kullanıyor. çünkü para çevrimlerdeki komisyonları ve transfer ücretleri gerçek dolara göre çok daha düşür. para birimi kısaltması: USDT.

# Stablecoin

tether gibi sabit değerden satılacağı idda edilen sanal-para birimlerine verilen isimdir.

# IOTA

proje ismi ile para birimi ismi aynı sanal paradır. asıl olarak iot cihazlarda kullanılması hedeflenen bir para birimi mekanizmasıdır. MIOTA, GIOTA gibi para birimleri kullanılır. Bunlar farklı para birimi diildir. birer metrictir. megabayt, gigabaytın ilk harflerini prefix olarak almıştır.

miner'lara gerek kalmadığı bir sistemdir. her node bir miner'dır. fakat 1 transaction yapabilmek için belirlenen sayıda transactionı onaylamanız beklenir. bu sebeple minerların işi parçalanmış olur ve ufak işlemler olduğu için cep telefonları dahi bu ufacık mining işini yapabilir hale gelmektedir. transaction sayısı arttıkça onaylama sayısıda arttığı için hiçbir zaman transaction süreleri yavaşlamayacaktır.

IOTA blockchain kullanmıyor. Onun yerine DAG (yada directed acyclic graph) veri yapısı kullanılır. DAG'ın boyutu sürekli büyüdüğünden, belli aralıklarla snapshot'ı alınarak sadece tüm işlemlerin özeti kalır ve artık herkes bu snapshot'ı kullanır. örneğin snapshot alındığında; eski transactionlar silinir, geriye sadece son durumda parası olan cüzdanlar kalır. 

# token vs coin

etherimum gibi turing-complete sistemlerde akıllı sistemler yürütülmektedir. bu sistemlerin bazılarıda para birimidir. bu tarz para birimlerine token denilir. herkese token dağıtılır. fakat para birimi artık kendi altyapısını kuracak (birileri tarafından sunucularda işlenecek) konuma geldiğinde o zaman token olmaktan çıkar ve herkese token'ı kadar coin verilerek coin sistemine dönüşür.

# Directed acyclic graph (DAG)

bir çeşit veri yapısı modelidir. Bazı sanal para birimlerinde blockhain yerine tercih edilir.

# dağıtık (yada Distributed) vs merkeziyetsiz (yada Decentralized)
merkeziyetsiz sistemler, dağıtık sistemlerin bir türevidir. merkeziyetsiz sistemlerin tek farkı, merkez bir node'un karar verme yetkisinin olmamasıdır. ancakk ve ancak tüm node'lar aynı kararı alırlarsa, tüm networkte o güncelleme yapılabilir.

eğer bir grup kendi içinde üncelleme yapar ve diğer node'lar bunu kabul etmezse fork oluşur.

# Hard fork vs soft fork

Hard fork gerçekleştiğinde gerçekleşen andan itibaren blockchain veri tabanına artık yeni yazılıma geçenler haricinde yazabilen ve okuyabilen olmuyor. Oysa soft fork'ta sadece yazabilenlerin yeni yazılıma güncelleme yapmaları gerekiyor. Soft fork'ta yeni blockların validation işlemi için yazılımı güncellemeye gerek yoktur.

# Oracle

Tükçe kelime anlamı: kahin.

Akıllı kontraat tabanlı sistemlerde oracle bir servistir. Bu servis dışarıdan herhangi bir bilgi sorgulanmasını gerçekleştirir. örneğin hava durumu 30 dereceyi geçerse etherium'daki bir akıllı kontraat bir yere para gönderecek olsun. hava durumu servisi burada "oracle" olarak adlandırılıyor.

bir sürü oracle servisi mevcuttur. örneğin; Oraclize.

# testnet

bitocin'in test sürümünün ismidir. protokol daha kolay mining yapar ve veritabanı test kayıtlarını içeririr. daha sonra diğer para birimleri de bu isimle test sürümlerini yayınlamıştır. bu sebeple sadece bitocine özgü bir kelime değildir.

# electrum ile testnet kullanımı

"electrum --testnet" komutu ile uygulama başlatılması yeterli.

# electrum hesap yönetimi

bitcoin protokolünde adreste olan tüm "transaction input"'lar tek bir "output" olarak gitmek zorundadır. yani adresimden kısmen para gönderiyim diye bir şey yok. size kaç parça gönderilmişse,o parçaları aynı büyüklükte yollamak zorundasınız. fakat karşı tarafa para atlırken birden fazla adrese yollanabilir. ama tek işlemde yollanmak zorundadır. bu bitcoinin bir kuralıdır.

electrumda seed üzerinden cüzdan açıldığında "receiving" ve "change" grup isimleri altında birçok cüzdan adresi görülür. biri bize transfer yapmak istediğinde herhangi bir adrese transfer yapılabilir. "request payment" kısmında electrum "receiving"'deki ilk sıradaki adresi gösterir. biri bize para yolladığıdnda ve electrumu açıp kapattığımızda artık "request payment" kısmında electrum "receiving"'deki 2'inci adresi gösterir. electurmun bunu böyle yapıyor 2 sebepten: 1- gizliliği arttırıyor. 2- her "transaction input" 1 adreste tutulmuş oluyor. harcarken 1 adres 1 transaction output olarak hesaplanacağı için hesaplaması (görülebilirliği) daha kolay olmaktadır.

Şimdi ise yollama işlemine bakalım. para herhangi bir hesaptan yollandığında eğer yollanan para; herhangi bir adresimizin total amount'u ile eşit değilse; karşı tarafa nasıl bu paranın bir skımını yollayacağız? işte burada electrum bizim için devreye giriyor. bir adrestei paranın yollanacağı kısmı yolluyor, o adreste kalan diğer kısmı ise aynı "transaction output" ile bizim "change adresses" listesindeki hesaplardan birine yolluyor. bu şekilde tek işlemde kısmı para göndermiş oluyoruz.  

# Lightning Network (yada LN yada Yıldırğım ağı)

bitcoinde transaction fee'lerinin artması ve transaction onaylanmalarının yavaşlaması üzerine LN geliştirilmiştir.

LN açık kaynaklı bir spesifikasyon. Birçok açık kaynaklı implementasyonu var. Bu sistem birçok para birimini destekleyebiliyor. bu sistem desteklediği para birimleri protokollerindenbağımsızca (off-chain) çalışıyor. bu sistem ile farklı bir blockhain üzerinde transaction yapılacağına dair onaylama gerçekleşiyor.

bitcoini baz alan bir LN özetle şu şekilde çalışıyor: belli bir miktar bitcoin'i X kişisi NL networküne taşıyor. artık NL networkündeki akıllı kontraatlar ile bu bitcoin yönetiliyor. Y kişisi Z kişisine bitcoin atmak istediğinde, X kişisi aracılığı ile bu işlemi gerçekleştiriyor. İşte buradaki X-Y-Z arasındaki işlem yoluna "ödeme kanalı (yada payment channel)" adı veriliyor. X kişisinde hazır yüklü olan para birimi Z ye aktarılıyor. NL son bitcoin durumunu bitcoin-blockhaine göndermiyor. tabi kanalı biri kapatıyorum diyene kadar. Yani NL X-Y-Z kanalı 1 ay işlem yapıp kapatılırsa, sadece 1 ay sonraki özet bilgi (son durum bilgisi) bitcoin blockhain'e aktarılıyor. böylece bitcoin blockhaini daha az şişiyor ve X-Y-Z arasındaki işlemler daha ucuza yapılmış oluyor. NL networkünde de blockchain işleme ve B aracısına para verme (fee) durumu olacak fakat şu andaki bitcoin ücretlerine ve hızına göre çok çok az olucak.

# Hyperledger

açık kaynaklı blockhain altyapısı kullanan ve blockchain ile network üzerinden anlaşma sağlayan framework altyapısı projesidir. bu proje tek başına kullanılamaz. bu projeye dayalı, birçok altproje mevcuttur. bunlardan biri seçilmek zorundadır. örnek altprojeler: Hyperledger Fabric, Hyperledger Burrow...

Hyperledger bir sistemi tümüyle ele alıyor: blockhain defterin doplanması, konsensüs kuralları/yönetimi, güvenlik ve yetki/kimlik yönetimi, network haberleşme protokolü gibi...

Hyperledger projesi içinde araçlar da mevcuttur. örnek: Hyperledger explorer (GUI ile kayıtları takip edebilmemizi sağlıyor)

Hyperledger alternatifi piyasada birçok proje mevcuttur.

# Blockchain 1.0 vs 2.0 vs 3.0

blockhian sadece terimi bir kayıt defteridir. fakat akıllı kontraatlar ile defterden daha fazlasını sunan blockhain temelli yazılımlar ortaya çıkmıştır. bu yazılımların çıkışı ile 2.0 terimi de insanlar arasında yayılmıştır. oysa veritabanı sisteminde bir değişiklik meydana gelmemiştir. aslında "blockchain platformu 2.0" ismi teorik olarak daha uygun olacaktı.

sıralama şu şekilde gitmektedir:
- 1.0 - cryptocoins
- 2.0 - smart contracts
- 3.0 - __DApp (yada decentralized application)__ birer uygulamadır. bu uygulamalar blockchain'e erişmek için smart contract'ları kullanırlar. DApp'lar web standartlarına uygun son kullanıcıya arayüz sunabilirler yada birer native uygulama olabilirler.

# DApp temel yapısı
Native bir uygulamadan yada web browser'ımızda çalışan açık kaynaklı bir kod son kullanıcıya sunulur.

Son kullanıcı bu kod aracılığı ile direk olarak kendi localinde yada 3üncü parti birinin sunduğu uzak makinadaki etherium-node'una bağlanır ve etherium nodun'da koşan kontrocat'lara istek atar.

Son kullanıcı localinde yürüttüğü native uygulama ise etherium-node'a bağlantı kolay olacaktır. Fakat son kullanıcı'yı web browser'ından bağlantırtmak daha dolambaçlı yapılmaktadır: Kullanıcı web tarayıcısına bir eklenti kurmaktadır. Eklenti web sayfasındaki local kodlar ile temasa geçmektedir. Önrke bir web tarayıcısı eklentisi: Metamask

Buradan anlaşılan; html, css, js kodları etherium node'ları tarafından paylaşılmamaktadır.

Etherium gibi yapılarda contract'lara istek attığımızda anında cevap alamayız. İşlemlerin ne zman tamamlanacağı kesin değildir. çünkü işlemleri dünyadan bir çok node gerçekleştirecek ve blockhain'e yazıp işlemi onaylaması gerekecek. dolayısı ile istekler yapılır, eventlere subscribe olunur.

Etherium üzerindeki akıllı kontraatlar, __solidity__ isimli dil ile yazılmış ve native olarak etherium-node'lar üzerinde koşmaktadır.

# Decentralised Autonomous Organisation (yada DAO yada decentralized autonomous corporation yada DAC)

bu terim; bir makalede ortaya atılmıştır. bu terim; kendi başına çalışan otonom bir sistemi temsil eder.

DAO bir açıdan bakıldığında; bitcoin, etherium gibi birçok sistemi içine almaktadır. çünkü bu sistemler de tek başlarına işleyebiliyor. fakat bitcoin, etherium gibi sistemler ne kadarda merkeziyetsiz olsalarda, birileri tarafından güncelleniyorlar. DAO kavramında insan elininin sadece başlangıçta olması beklenir.

DAO pratikte en yakın; Ethereum tarzı sistemlerde çalışan akıllı kontraatlara benzetilebilir. Çünkü pratikte gerçek bir işleyişin olması için, bir run-time'ın olması gerekli. run-time ortamı içinde etherium tarzı sistemlerdeki contraatlar uygun bir yapılardır.

# DAO vs Dapp
DAO tamamiyle autonomus olması beklenmektedir. yani; sisteme manuel mudahele gerektirmemelidir. %100 bunu yapan bir yazılım yapılamadı fakat insan elinin daha az değdiği birçok etherium-contract'ı geliştirildi ve işletildi.

Dapp ise insan elinin müdahelesi sonucunda işlem yapar.

DAO kavramı biraz felsefik/teoriktir.

__The Dao__ özel bir isimdir. simgesi: __Đ__. Bir grup yazılımcının etherium üzerinde çalıştırdıkları DAO'y averdikleri özel ismdir.

# merkezsiz borsa (yada decentralized exchange yada DEX)

p2p tabanlı protokollerle insanların paralarını birbirine transfer edebileceği yazılımlar (borsa yazılımları) geliştirildi. bu borsa yazılımları direk para sahibinin cep telefonunda yada bilgisyarında çaıştırılıyor. para sahibi uygulaması açık olduğu sürece parasını topluluğa açabiliyor veya başkasının parasını alabiliyor. herkes kendi teklifini sunabiliyor yada emir bırakabiliyor. sanal paraları bu sistemlerden nakit paraya çevirmek mümkün diil. nakit paraya çevirebilmek için nakit paranın servis sağlayıcısına erişim olması gerekir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# mantıksal analiz robotu (yada yapay zeka yada Artificial intelligence yada AI)

bir bilgisayarın, gerçek bir düşünebilen canlı gibi davranması yeteneğidir.

# yapay sinir ağı (YSA yada Artificial neural network yada ANN)

yapay zekanın varlığı için kendi kendini geliştirmelidir. YSA bu gelişim için kullanılan yapıdır. yani; ysa, yapay zeka için gerekli olan mimarinini adıdır. yapay zeka; için YSA kullanılmak zorunda değiliz. onun yerine eski oyunlarda olduğu gibi araba yarışı tasarlayabilir ve diğer yarışçıların da akıllı davranabilmesini sağlayabiliriz. 

# Makine öğrenimi (machine learning)

araştırmalarının odaklandığı konu; bilgisayarlara algılama ve bir data ile kendini geliştirme yeteneğini kazandırmaktır. yapay zeka her zaman bir veriden kendini geliştirmek zorunda değildir. eski oyunlarda akıllı olan bilgisayar yarışçıları datadan beslenmiyorlardı.

# derin öğrenme (deep learning)

makine öğrenmesinin bir türevidir. çok katmanlı YSA'ların kullanılmasıdır. çok katmanlı YSA'lar ile birlikte her level (katman) kendi içinde  ysa'lar barındırmaktadır. örnek: araba kullanan robot yapacağız. fonksiyonumuz araba kullanmak. fakat alt fonksiyonlarımız var: direksoynu yönetme, acil durumlarda arabayı tamir etme gibi... İşte bu katmanlar ayrı ayrı birleştirilmektedir.

çok katmanlı YSA'ların kullanılması özellikle görüntüden resim cisim/obje ayıklama/sınıflandırma için çok uygundur. grafik kartları (GPU) lar bu konularda özel çalışmalar yapmaktadırlar.

# Bulanık mantık (yada bulanık eseme yada puslu mantık yada Fuzzy logic)

Bulanık mantığın temeli bulanık küme ve alt kümelere dayanır. Klasik yaklaşımda bir varlık ya kümenin elemanıdır (1) ya da değildir (0). bulanık mantık ile bir varlık üyelik derecesine göre birden fazla kümenin elemanı olabilir. üyelik derecesi 0-1 arasındadır. örnek olarak sıcaklık derecesi verilebilir. belirli bir seviyedensonrasını sıcak belirli bir seviyedensonrasını soğuk olarak ele alırsak; ılık sıcaklıklar ve bazen yarı-sıcak ve ılık sıcaklıkların olduğu dereceler bulabiliriz.

# Sinaptik (yada Sinaptic)

Sinaps (yada Synapse) kelimesinde geliyor. Sinaps; nöronların (sinir hücrelerinin yada neuron) diğer nöronlara ya da kas veya salgı bezleri gibi nöron olmayan hücrelere mesaj iletmesine olanak tanıyan özelleşmiş bağlantı noktalarıdır.

# YSA yapısı

Girişler her hücreye (yada node yada düğüm) geldiğinde ağırlıklar (yada W yada Weight) ile çarpılır. Her girişten gelen aynı derecede önemsenmeyeceği için bu çarpım işlemi yapılır.

Bütün girişler çağrıldıktan sonra sinir içerisinde bir matematiksel işleme tabi tutulurlar. Bu işlem "toplama fonksiyonu" olarak adlandırılır. Burada isim karışıklığı olabilir. BU metod sadece toplama çarpımı değildir. Herhangi bir matematiksle işlem olabilir.

Çıkan çıktı ise en son yine bir matematiksel işlemden geçer. En sonki bu işlem çıkış için bir filtre görevi görür ve aktivasyon fonksiyonu ismini alır. Burada da isim karışıklığı olabilir. "aktivasyon fonksiyonu" matematikteki özel bir fonksiyona denk geliyor. Fakat YSA'da bu fonksiyon herhangi bir işlem olabilir.

# YSA Katmanlar (yada Layers)

YSA'nın ilk girdi aldığı hücreler ve son çıkışa giden hücreler arasında da hücreler olabilir. aradaki katmanlara (ilk ve son katmanlar hariç) gizli katmanlar (yada ara katmanlar) denir. Dolayısı ile gizli katmanlı olan bir ağ, en az 3 katmandan oluşuyordur.

çok katmanlı ysa en az 2 katmandan oluşur.

bir YSA en az 1 katmandan oluşabilir.

# Ağın Eğitilmesi

YSA'nın eğitilmesi için doğru olarak bilinen giriş ve çıkış değerlerinin elimizde olması gerekir. YSA'ya girdiler verilir ve YSA'nın verdiği çıktı ile gerçek çıktı karşılaştırılır. İşte bu hata payına göre, W değerleri her işlem sonrası değiştirilir. Bu eğitim şekline geri beslemeli (yada geri yayılım yada back propagation) eğitim denir. Birçok farklı eğitim şekli düşünülebilir.

Ağa ilk W değerleri ya rastgele yada belli referanslara dayanılarakta atanabilir.

Test süreçlerinde YSA'nın eğitilmemesi gereKmektedir.

# Hopfield Ağları (Hopfield Net)

Aşağıdai kurallara uyan sinir ağlarına verilen özel isimdir. Bunun gibi birçok ağ mevcuttur.

Tüm hüclerelerde sadece 1 ve 0 değerleri giriş çıkışlarda olmalıdır

Nöronlara gelen ağırlıklar aktivasyon fonksiyonuna gelmeden hemen önce toplanırlar

Nöronlarda aktivasyon fonksiyonu olmalı ve eşik değeri kullanılmalıdır

Katman kavramı yoktur. Tüm hücreler diğer tüm hücrelere bağlıdır. Yani aslında tek katmanlıdır diyebiliriz. Böylece tüm girişler (hücreler) aynı zamanda çıkışları verecektir. n girişli ve n çıkışlıdır. Her bir hücre çıkışı, bir sonraki iterasyonda diğer tüm hücrelerin girişine etki etmektedir.

# iterasyon vs epoch

epoch'un türkçe kelime anlamı: devir, çağ, dönem'dir.

iterasyon YSA'nın bir kere input alıp, bir kere çıktı vermesine kadar olan süreçtir.

YSA'lar eğitilirken bir eğitim seti (giriş ve çıkış seti) olmaktadır. BU tüm set ysa'ya verilir ve ysa kendini sonuçlara göre eğitir. Tüm eğitim seti dolalışdığında bir epoch süresi geçmiş sayılır. YSA eğitiminde bazen epoch sayısını (aynı eğitim seti için) birçok kere geçmek gerekebilir.

# Basamak (yada adım yada step yada staircase) fonksiyonu

Belirli aralıklarda sadece 1 sayıyı çıktı olarak veren fonksiyonlar grubudur. Örnek; "Heaviside step function", "signum", "Constant" bu kümenin içindedir.

# Sigmoid fonksiyonu (simge: S)

bu fonksiyon her girdi için 0 ile 1 arasında değer üretir.

f(x)= 1/1+e^(-1)

# Eşik Değer (treshold) fonksiyonları

bu bir fonksiyon grubudur. sigmoid ve basamak fonksiyonları grubun içindedir. eşik değerlere göre sonuç verirler.

# Birim fonksiyon (yada özdeşlik fonksiyonu yada özdeşlik gönderimi yada özdeşlik dönüşümü yada birim dönüşüm yada birim işlev yada unit)

f(x) = x

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# İşaretleme dili (yada etiketleme dili yada markup language)

Latex, HMTL, XML, XHTML gibi dillerdir. Bir dilin markup language olabilmesi için; parse edilip işlenmesi sonrası ortaya elektronik bir döküman çıkması gerekli. Bu dillerle bu dökümanın stili ve yapısı belirlenmektedir.

Bu sebeple Json bir markup language değildir. XML markup'tır, çünkü SVG gibi yapılarda bir döküman outputu verdiği görülür.

* Markdown
  
  Çok basit bir markup dilidir. çeşitleri:

  - Github Flavored Markdown (including AtomDoc)
  - R Markdown

  gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# OpenStreetMaps (OSM)

Açık kaynaklı dünya haritası veritabanıdır. forsquare gibi firmalar bu veritabınından yararlanır. sadece veritabanı sunduğundan, API'ler ancak üçüncü partiler tarafından yayınlanır.

Opesseamaps OSM'nin içindedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Dokuman yonetim sistemleri

| name       | comment                                                                                                                       |
|------------|-------------------------------------------------------------------------------------------------------------------------------|
| confluence | Atlasian'ın geliştirdiği wiki tool'u                                                                                          |
| MediaWiki  | wikipedianın altyapısını sağlayan wiki tool'u. bu wordpress gibi hazır bir yapı. direk wikipedia gibi site oluşturulabiliyor. |
| XWiki      | open source                                                                                                                   |

# CI sistemleri

- Bamboo

Atlasian şirketinin geliştirdiği CI (Continuous Integration) tool'u.

Agent dediğimiz uygulamalar, bamboo'ya bağlanıyor. Bambo bir task yapmak istediğinde, agent makineleri (agent'ın home klasörü ve her bir task için yaratılan workspace) üzerinde çalıştırıyor ve sonucu alıyor. Bambo bulunduğu makinede bir işlem yapmıyor. Herşey agent'larda yapılıyor. Bu agent'lar; amazon sunucusunda, docker'da yada aynı makinedeki birden fazla işletim sistemi user'ında olabilir.

Bmabodaki scriptler büyükten küçüğe şu şekilde gruplandırılıyor: Plan -> Stage -> Job > Task

- alternatifler: Jenkins, Hudson

# SCM sistemleri

- alternatifler:

| name                                                  | comment                                                 |
|-------------------------------------------------------|---------------------------------------------------------|
| Atlasian BitBucket                                    | Server                                                  |
| gitlab                                                | Server (ekstra CI özellikleri içeriyor)                 |
| EGit                                                  | Eclipse plugin (default installed)                      |
| Atlasian SourceTree                                   | Client Desktop                                          |
| SmartGit                                              | Client Desktop                                          |
| GitHub                                                | web site (Proprietary software)                         |
| git-gui                                               | Default Git's Client Desktop without history/log viewer |
| gitk                                                  | Default Git's Client Desktop only history/log viewer    |
| CA Harvest Software Change Manager (yada CCC/Harvest) | develop by microsoft                                    |
| Bazaar (yada Bazaar-NG)                               | Server                                                  |

# SVN sistemleri

| name        | comment                                                       |
|-------------|---------------------------------------------------------------|
| Subversive  | Eclipse plugin (default installed on old versions of Eclipse) |
| TortoiseSVN | Client Desktop                                                |

# SCM, CI gibi bir yazılımın tüm hayat döngüsünü sağlayan paket yazılımlar

- Sourceforge

- Launchpad

- Team Foundation Server

  Hem localde hemde Azure'de yayımlanan sunucu versiyonları mevcut. Kendi SCM protokolünü içermektedir. İstenirse "git" yazılımıda kullanılabilmektedir. TFS sadece SCM değil, aynı zamanda birçok işlevi CI, Proje yönetimi, test hayat döngüsü gibi de içermektedir. Bir yazılım için gerekli tüm hayat döngüsünü sağlayacak altyapıya sahiptir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# yaml (yada YAML Ain't Markup Language)

boşlukların, satır sonlarının önemli olduğu XML ve JSON'a alternatif bir formattır. .yml veya .yaml uzantıları ile dosyalarda tutulurlar.

json'a göre daha çok özelliğe sahiptir ve okunaklığı daha fazladır.

yaml editor kullanmadan editlemek çok zordur. ve tab karakteri kabul görmemektedir/kullanılmamaktadır.

yaml dökümanı isteğe bağlı olarak --- ilen başlayıp ... ile bitebilir. 

örnek yml:

```yml
# yaml supports comments, json does not

array: 

  - first element of array

  - second element of array

  

# string yazraken tırnak yada tırnak koymasakta olabilir

stringObject:

  'key': 'any string here'

  "key2": "any other string here"

  key3  : any other string here

#tipler otomatik algılanır. eger bir integer'ı string algılatmak istiyorsak; o zaman tırnak kullanmak zorundayız.

types:

  - 1

  - "1"

  - true

  - "true"

# yes ve no'lar true false olarak algılanıyor

booleanTypes:

  - no 

  - yes

  - NO 

paragraph: >

  my name

  

  is ahmet

# yada sonuna bir tire işareti de atılabilir content: |-

content: |

   my name

   

   is ahmet

# biraz farklı da olsa json tarzı bir formatta da veriler tutulabiliyor

ornekJson: {name: Martin Developer}
```

yukarıdaki yaml'a denk gelen json aşağıdaki gibidir:

```json
{

  "array": [

    "first element of array",

    "second element of array"

  ],

  "stringObject": {

    "key": "any string here",

    "key2": "any other string here",

    "key3": "any other string here"

  },

  "types": [

    1,

    "1",

    true,

    "true"

  ],

  "booleanTypes": [

    false,

    true,

    false

  ],

  "paragraph": "my name\nis ahmet\n",

  "content": "my name\n\nis ahmet\n",

  "ornekJson": {

    "name": "Martin Developer"

  }

}
```

# yaml format standartları

> YAML 1.2 (3rd Edition) - 2009-10-01-  http://yaml.org/spec/1.2/spec.html

> YAML 1.1 (2nd Edition) - 2005-01-18 - http://yaml.org/spec/1.1/

> YAML 1.0 (1st Edition) - 2004-01-29 - http://yaml.org/spec/1.0/

# json format standartları

> March 2017 - RFC 7493 - This document specifies I-JSON, short for "Internet JSON"

> March 2014 - RFC 7159 - draft of "I-JSON"

> March 2013 - RFC 7158 - draft of "I-JSON"

> October 2013 - ECMA-404 - JSON standard at ECMA

> July 2006 - RFC 4627 - first json public doc

Yukarıdaki standartlar az da olsa biribiri arasında fark ediyor. bazı parser/validatorlarda bu bilgi lazım olabilir.

__JSON5, jsonc, Hjson__ json'dan bağımsız fakat ondan türemiş farklı standartlardır.

# xml format standartları
W3C.org'da xml standartları belirlenmiştir. W3C.org da bir spesifikasyon bu süreçlerden geçer:

1. Working Draft (WD)
2. Last Call Working Draft
3. Candidate Recommendation (CR)
4. Proposed Recommendation (PR)
5. W3C Recommendation (REC)

http://www.w3.org/TR/xml/ URL'si her zaman son sürüme otomatik yönlendirir.

> 1.0 (W3C Recommendation of Fifth Edition) - http://www.w3.org/TR/2008/REC-xml-20081126/

> 1.0 (Proposed Edited Recommendation of Fifth Edition) - https://www.w3.org/TR/2008/PER-xml-20080205/

> 1.0 (Fourth Edition)- https://www.w3.org/TR/2006/REC-xml-20060816/

> ...

# xml (yada eXtensible Markup Language)

  - #### terminoloji:

Aşağıdaki tüm satır bir __element__'tir.
```xml
<tag attribute="attribute's value"> </tag>
```

  - #### ? karakteri

ilk satırda encoding ve xml-version'u bu şekilde belirtilebilir:

```xml
<?xml version="1.0" encoding="UTF-8"?> 
```

  - #### XML Namespace

```xml
<table>

  <tr>

    <td>Apples</td>

    <td>Bananas</td>

  </tr>

</table> 

<table>

  <name>African Coffee Table</name>

  <width>80</width>

  <length>120</length>

</table> 
```

yukarıdaki iki farklı xml tek bir xml'de birleştirmiş olalım. fakat isimlendirmeler benzediği/aynı olduğu için sorun olur. bunu çözmek için elemenlerin önüne prefix atılır.

```xml
<h:table>

  <h:tr>

    <h:td>Apples</h:td>

    <h:td>Bananas</h:td>

  </h:tr>

</h:table>

<f:table>

  <f:name>African Coffee Table</f:name>

  <f:width>80</f:width>

  <f:length>120</f:length>

</f:table> 
```

namespacelere isteğe bağlı "xmlns" ile id verilebilir. fakat genelde id yerine o namespace için dökümantasyon url'si yazılır. çünkü url'de genelde unique'tir. örnek:

```xml
<h:table xmlns:h="http://www.w3.org/TR/html4/">

  <h:tr>
```

yada şu şekilde:

```xml
<root xmlns:h="http://www.w3.org/TR/html4/">

   <h:table>
```

# XSL (yada Extensible Stylesheet Language)

XML ile ilgili tüm standratların ailesine verilen isimdir. Altında XSLT, XPath gibi standratlar bulunmaktadır.

# XSLT (yada XSL Transformations)

XML dökümanını, yine bir XML dosyasına dönüştürmek (farklı bir şema üzerine transfer etmek) için kullanılan XML tabanlı dildir. Yazılımcılar arasında XSL olarak adlandırılır, oysa sadece "XSL" farklı bir kavramdır.

örnek xslt asagidadır. bu xslt'yi xml dköümanını da vererek işletirsek; output olarak bir liste alırız.

xslt:
```xml
<xsl:stylesheet>

  <xsl:output method="text"/>

  <xsl:template match="/">

    Article - <xsl:value-of select="/Article/Title"/>

    Authors: <xsl:apply-templates select="/Article/Authors/Author"/>

  </xsl:template>

  <xsl:template match="Author">

    - <xsl:value-of select="." />

  </xsl:template>

</xsl:stylesheet>
```

xml:

```xml
<Article>

  <Title>My Article</Title>

  <Authors>

    <Author>Mr. Foo</Author>

    <Author>Mr. Bar</Author>

  </Authors>

  <Body>This is my article text.</Body>

</Article>
```

output:

```
Article - My Article

Authors:

- Mr. Foo

- Mr. Bar
```

output olarak aldığımız listenin "Authors" stringinin sağına ve soluna <div> atsaydık, saf liste yerine html çıktısı almış olacaktık.

# XPath

XML dökümanındaki bir verinin yerini gösermek için kullanılan path yapısı. ELimizde XML olsun. İçeirisnden aşağıdaki path'lerle açıklaması yazan bilgiler çekilebilir:

```
/items/philosphy[2] : Philosphy deki ikinci item

/items/*/dictionary : Bütün Sözlükler

/items/*/book[@id="12"]/@title : Id'si 12 olan kitabın title'ı

count(/items/sociology/book) : Sociology kitaplarının sayısı

/items/psychology[last()] : Son psychology kitabı

/items/*/[contains(@title,"Turk")] : İçerisinde Turk geçen bütün maddeler
```

# XQuery

SQL'e benzer şekilde; bir XML dökümanından veri çekebilmek için query dilidir. Örnek:

```xquery
for $x in doc("books.xml")/bookstore/book

where $x/price>30

order by $x/title

return $x/title
```

# XML Schema Definition (yada XSD) vs document type definition (yada DTD)

ikiside birbirindenbağımsız XML şeması belirtmek için yaratılmış dillerdir. XML içerisindeki elementlerin sırasını dahi belirtebilirsiniz.

# dtd

kurallara uymasını istediğimiz xml dosyasının en üstüne aşağıdaki satırı yerleştirmeliyiz.

dtd dosyası dışardan okunur:

```xml
<!DOCTYPE note SYSTEM "kurallar.dtd">

<note>

  <to>Tove</to>

</note>
```

yada dtd bilgileri xml'in içine gömülür:

```xml
<!DOCTYPE note [

<!ELEMENT note (to,from)>

<!ELEMENT to (#PCDATA)>

<!ELEMENT from (#PCDATA)>

]>

<note>

  <to>Tove</to>

  <from>Ayse</from>

</note>
```

yukarıda 2'inci satırda elementlerin sırası verilmiş. 3. satırda "to" elementinin PCDATA, yani text olacağını belirtmiş.

# xsd

xml dökümanının şemasını referans etmek için:

```xml
<note h:schemaLocation="https://www.w3schools.com note.xsd">

<h:to>Tove</to>
```

xsd dosyasında her zaman root'ta "xs:schema" bulunmak zorunda.

sağ tarafta açıklamaları ile örnek:

(asadaki xsd'yi düzgün okuyabilmek için ekranı büyüt)

```xml
<xs:schema>

<xs:element name="note">    Note isimli bir element olması gerek.

  <xs:complexType>            Bu elementin içinde birden fazla element olacak

    <xs:sequence>             Child elementler xsd'de yazan sıra ile olmalı. sequence yerine all kullanırsak, sıralı olmak zorunda olmazlar.

      <xs:element name="to" type="xs:string"/>        child element olarak "to" isimli element olucak ve degeri string olucak. örnek <to>Ahmet</to>

      <xs:element name="from" type="xs:string"/>      child element olarak "from" isimli element olucak ve degeri string olucak. örnek <from>Ahmet</from>

    </xs:sequence>

  </xs:complexType>

</xs:element>

</xs:schema>
```

-  type="xs:string" yerine birçok farklı tip yazılabilir: Integer, Boolean, Date, negativeInteger, unsignedByte gibi birçok  standart belirlenmiştir.

- <xs:element name="to" type="xs:string" fixed="red"/>  to elementinin içindeki değerin sadece "red" olabilir anlamına geliyor

- fixed="red" yerine default="red" de yazabilir. bu durumda bu değer xml'de belirtilmezse, bir parser tarafından okunduğunda "red" okumasını emreder.

- asagidaki simpleType complex gibi değildir. simple olduğu child elementin olmayacağı anlamına gelir.

```xml
<xs:element name="age">

  <xs:simpleType>

    <xs:restriction base="xs:integer">

      <xs:minInclusive value="0"/>

      <xs:maxInclusive value="120"/>

    </xs:restriction>

  </xs:simpleType>

</xs:element> 
```

restriction için farklı bir örnek:

```xml
<xs:element name="car">

  <xs:simpleType>

    <xs:restriction base="xs:string">

      <xs:enumeration value="Audi"/>

      <xs:enumeration value="Golf"/>

      <xs:enumeration value="BMW"/>

    </xs:restriction>

  </xs:simpleType>

</xs:element> 
```

restriction kendi içinde birçok farklı özellik daha barındırıyor.

xsd'lerde tipleri önceden define edebiliriz. örnek:

```xml
<xs:element name="product" type="prodtype"/>   Define ettik

<xs:complexType name="prodtype">      Burada kullandık

  <xs:attribute name="prodid" type="xs:positiveInteger"/>   Attribute isteğe bağlı (konudan bağımsız bir özellik)

</xs:complexType> 
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •


• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Systems development life cycle (yada SDLC yada application development life-cycle)

genel br terimdir. aşağıdakilerin kümesini kapsar.

# Agile (Çevik) proje yönetimi

bir proje (yazılım projesi olmak zorunda diil) yönetim (işleyiş) biçimidir.  Gereksinimleri açıkça belirli olmayan, değişime açık, karmaşık projelerin yönetimi için uygulanmaktadır. agile manifestosu şu şekildedir:

---- türkçe ----
```
Bizler daha iyi yazılım geliştirme yollarını

uygulayarak ve başkalarının da uygulamasına yardım ederek ortaya çıkartıyoruz.

Bu çalışmaların sonucunda:

Süreçler ve araçlardan ziyade bireyler ve etkileşimlere

Kapsamlı dökümantasyondan ziyade çalışan yazılıma

Sözleşme pazarlıklarından ziyade müşteri ile işbirliğine

Bir plana bağlı kalmaktan ziyade değişime karşılık vermeye

değer vermeye kanaat getirdik.

Özetle, sol taraftaki maddelerin değerini kabul etmekle birlikte,

sağ taraftaki maddeleri daha değerli bulmaktayız.
```

---- ingilizce ----
```
 We are uncovering better ways of developing

software by doing it and helping others do it.

Through this work we have come to value:

Individuals and interactions over processes and tools

Working software over comprehensive documentation

Customer collaboration over contract negotiation

Responding to change over following a plan

That is, while there is value in the items on

the right, we value the items on the left more.
```

# Scrum

Agile'ın bir türevidir. Sprint (kelime anlamı: sürat koşusu), backlog, daily meeting gibi kavramlar bunun içindedir. Sadece yazılım projelerinde uygulanmak için tasarlanmıştır. Scrum'ın türkçe kelime anlamı itişip kakışma anlamına gelir fakat özel türkçe ismi yoktur.

Scrum temel olarak agile'a sürecin nasıl işleyeceği ile ilgili detaylandırılmış şeklidir. Agile ise işin felsefik kısmıdır. agile'da yüzyüze iletişim olması gerektiği belirtiltilir; scrumda ise, müşteri ile birlikte çalışmak çok verimli olduğu belirtilir. Agile'da iterasyonlarlar bahsedilir, scrumda sprint ile detaylandırılmıştır. agile'da visualizing önemli denir, scrumda, scrum board'dan bahsedilir.

sprint içerisinde analistler her zmana yeni sprint belirmeden önce analzileri hazırlamış ve teknik ekibe yollamış olmalı. teknik ekip retro-toplantısı ile yeni sprint puanları verilmeden önceki arada bunları okumalıdır.

# Scrum çalışmalarında geçen kavramlar ve notlar

- scrum toplantıları: sadece dün ne yaptım, bugün ne yapacam ve engelim varmı toplantısıdır

- scrum master: sadece scrum sürecini ve teknik olmayan konularda yaşanan engelleri çözmek için ilgili kişidir. taskların içeriği değilde, scrum süreçinin işlediğinden emin olmalıdır.

- project manager (proje yöneticisi): bazı agile sistemlerde bu role varken, bu rolde bir kişi scrum'da yoktur. çünkü srum'da takım self-organized'dır.

- team leader (yada takım lideri): scrumda böyle bir role yoktur. team self organized'dir.

- product owner (yada PO yada ürün sahibi): tüm kararlardan/araştırmalardan sonra en son ürün için yapılacak taskları onaylayan, üründe hangi geliştirmeleri yapılacağnı söylemeye yetkili kişi. PO sürekli müşteri ile temas halindedir. müşterinin isteklerini anlar ve backlog'daki tasklara businnes value'ları set eder. teknik bilgisi olan kişi değildir. sadece businnes odalıdır.

- Businnes Value: iş önceliği. product owner'ın karar verdiği değer. product owner için iş ne kadar önemli.

- öğe (yada item): scrum dilinde, öğe jira'daki tasklara verilen isimdir. her öğe, techinacal task dahi olsa, bir şekilde PO'nun da anlayacağı bir çıktısı olmalıdır.

- Story points: estimation ve işin zorluk (komplekslik) değeri çarpımıdır. alabildiği değerler: Fibonacci-like formatındadır: 0, 0.5, 1, 2, 3, 5, 8, 13, 20, 40, 100. Daha yüksek değer verilmemelidir. eğer daha yüksek değer vermek gerekiyor ise; sub-tasklara bölünmelidir. bug veya teknik elemanların açtığı task'larda story point olmalıdır. sonuçta product owner onaylıyor ve bunlar sprint'te çalışılıyor.

- Estimation: işin bir kişi tarafından bitirilmesi süresi

- retrospective (retrospektif) toplantısı: retrospektif kelime anlamı: 'dünden bugüne', 'geriye dönük'. her sprint sonu takımın ve scrum master'ın toplanarak yağtığı başarısızlıkları ve başarıları konuştukları toplantı. burada hem teknik hemde scrum süreci hakkındaki değerlendirmeler yapılır. fakat ideal olan scrum/agile süreci hakkında konuşmaktır. fakat teknik konulara da girilmez diye kural yok. süreç ne kadar başarılı gitsede her süreçte geliştirme amaçlı şeyler konuşulmalıdır. aksi durumda iyiye gidiş olmaz.

- Grooming toplantıları: grooming kelime anlamı: prepare or train (someone) for a particular purpose or activity. ürün sahibinin ürün iş listesindeki maddeleri, geliştirme takımı ile tartışabilmek, onların fikirlerini alabilmek için düzenlediği gayri resmi toplantılardır.

- Planning poker (yada Scrum poker): ekipçe her taska süre verirken uygulanan bir tekniktir. herkes kapalı kağıtları ile her taska süre verir.

- spring demo (yada spring review): sprint'in sonunda 

# Kanban

Agile'ın bir türevidir. Sadece yazılım projelerinde uygulanmak için tasarlanmıştır.

Kaban, scrum ile karşılaştırıldığında;

kambanda; toplantılar ve belirli aktiviteler bulunmamaktadır. Fakat yaşanan darboğaz durumlara göre aksiyonlar alınmaktadır. Her projede, süreç kendini geliştirmektedir.

kambanda; Yazılımcıların estimation vermesi opsiyoneldir.

kambanda; Agile'daki gibi roller (scrummaster gibi) belirlenmemiştir.

Kambanda; iterasyon opsiyoneldir. Agile'da sprint (iterasyona denk geliyor) zorunludur.

# Extreme programming (yada XP)

Agile'ın bir türevidir. Sadece yazılım projelerinde uygulanmak için tasarlanmıştır. Bu metodda kaliteli kod ön plandadır. Gerektiğinde ufak/büyük refactoring'ler yapılmalıdır. Merkezde sürekli çalıştırılabilir kod olmalı ve ufak ufak parçalar halinde geliştirmeler yapılıp bu koda eklenmelidir. Testler kodda önce yazılmalıdır. Basit tasarım ve kodlama standardı gibi kavramlara önem verilir.

# WaterFall Model (Şelale modeli)

Kod baştan sona analize göre tasarlanır ve müşteriye teslim edilir. Günümüz şartlarına pek uygun değildir. Zira tüm yazılım detaylandırılması önceden uzun bir zaman ve maliyet alması bir yana, analiz sürecindede gelişmeler meydana gelmektedir. Bunları o süreçte yakalam ve ona göre analiz yapmak neredeyse imkansızdır. Aynı zamanda müşteri bazen ne istediğini tam olarak bilemez. Geliştirme sürecine müşteri katılır ve zaman ile gelişimini inceler ise (diğer modellerde olduğu gibi), ürün teslimi sonunda müşterinin beklenmedik şeylerle karşılaşması zorlaşır.

Tüm proje boyunca bazen proje önmeli süreçlere bölünebiliyor (scrum'daki sprint sonu gibi). bu noktalara waterfall modelinde; milestone (kilometre taşı, dönüm noktası) diye isimlendiriliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# plug-in (yada plugin yada add-in yada addin yada add-on yada addon yada extension yada uzantı yada eklenti)

Hepsinin kelime anlamı aynıdır. Yazılım dünyasında işlevlerin farklılığını belirten bir standart yoktur. Her yazılım, ona yazılacak eklentinin çeşidini kendi rastgele karar vererek isimlendirir. Örneğin firefox; eklentilere add-on derken, chrome extension diyor. fakat iki tarayıcıda adobe flash tipindeki eklentiler için plug-in kelimesini kullanıyor.

Uzantı kelimesi farklı anlamlara da gelebiliyor. Uzantı bir dosya formatının, dosyadaki kısa adıdır. Bazı yazılımların eklentilere uzantı demesinin sebebi de burda geliyor. Çünkü eklentilerin setup dosyalarının uzantıları, o yazılıma eklenti kurmaya yarıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# OS/2

IMB ile Microsoftun çok eskiden birlikte geliştirdikleri işletim sistemi. Daha sonra sadece IBM geliştirmelere devam etmiş, fakat sonrasında IBM'de geliştirmeyi tamamen kesmiştir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Web browser User Agent

user agent header'da otomatik platform tarafından gönderilir. sadece browser değil, her client user-agent atmalıdır.

formatı incelersek; Çoğu "mozilla/" ile başlar. ilk olarka bu isim verilmişti. daha sonra herkes devam ettirdi. yanındaki ilk numara format sürümüdür.

User agent içerisinde aşağıdakiler sürüm bilgileri ile yazar:

- tarayıcıda bazı kurulu eklentilerin listesini

- layout engine ismi

- tarayıcı ismi

- işletim sistemi ismi

- işletim sistmi çekirdek ismi

- windows için kurulu .net ve service pack sürümleri

User agent dışında javascript tarafında alınan bilgiler:

ekran çözünürlüğü, tarayıcı pencere boyutu, local saat, cookie ve jascriptin açık olmadığı bilgisi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Ajax (yada Asynchronous JavaScript and XML)

web sayfasında reloading yapmadan sunucuya xml istekleri yaparak sayfayı yenileme tekniğidir.

# XMLHttpRequest (yada XHR)

Core Javascript'te ajax yollamaya yarayan objenin ismidir.

# origin
kelime anlamı: menşei, köken

web dünyasında origin protokol, domain, host (subdomain olarakta adlandırılır), port dörtlüsünün bir arada temsil etmektedir. örneğin; iki URL değerimiz olsun. ikisi aynı origin olabilmesi için bu 4 bilgininde aynı olması şarttır.

path değeri önemsizdir.

Sadece Microsoft internet explorer'da:

- "Trust Zones" olarak tanımlı domain'lere
- port bilgisi farklı olan URL'lere

istisna tanınıp aynı origin'de oldukları varsayılır. bu her sürümde varmı bilmiyorum. kaynak: https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy (archive date: 03/12/2019)

# Cross origin domain policy

Web tarayıcılarının başka domainlere istek yapılmasını güvenlik için engellemektedirler. Cross origin domain policy bu kıstası kapsamaktadır. web sitesinin bunu aşmasının birkaç yolu vardır: JSONP, CORS, Adobe flash gibi eklentiler'dir.

# Cross-Origin Resource Sharing (yada CORS)

Modern tarayıcılar tarafından cross site scriptin desteklenmesi standartlarıdır. Farklı bir domain'e istek yapılacağı zaman tarayıcı anında (artık - cors destekleyen tarayıcılar) bloklama yapmaz. bunun yerine eğer;

- istek "basit istek" değil ise preflight isteği yapılıyor.

- istek "basit istek" ise isteğe direk izin veriliyor.

tarayıcı arkaplanda; web sayfasının istek yapmak istedikleri adrese, istek tipini ve web sayfasının o andaki adresi gibi bilgileri gönderir. bu istek "option" metodu tipinde bir XHR'dır ve ismi "Preflight"'tır. sunucu bu isteğe cevap olarak tarayıcıya asıl isteği gönderip gönderemeyeceği bilgisini verir. eğer sunucu izin verirse web sayfasının ajax işlemi gerçekleştirilir. burada bir nebzede olsa güvenlik sağlanmış olmaktadır. çünkü Preflight request'i web sayfası değil, tarayıcı tarafından hazırlanmaktadır. standartlar web sayfalarını tamamiyle serbest bırakmak yerine, bir nebze daha güvenli olacağı düşünülmüş bu taktiği uygulamaktadır. JSONP yönteminden daha modern bir yöntemdir.

# Basit istek

Beğer bir XHR isteği;

- GET, HEAD, POST ise

- Accept, Accept-Language, Content-Language, Content-Type mutlaka olmalı

- Tarayıcılarn default doldurduğu headerlar olabilir (örnek user-aget) 

Content-type header'ında sadece application/x-www-form-urlencoded, multipart/form-data, text/plain değerlerini alabilir.

Basit istek olmayanlar; put gibi metodlar veya custom header'lar içerenler örnek olarak verilebilir.

# json vs jsonp
ajax isteği ile ancak aynı domain'e istek yapıp cevap alabiliriz. farklı domainlere yapılan istekler tarayıcılar tarafından güvenlik sebebi ile engellenir.

jsonp formatı; json formatındaki bir değerin bir metod içerisinde çağrılacak şekilde formatlanmış halidir.

örneğin;

json:
> { isim: ahmet }

jsonp:
> metodName ( { isim: ahmet } )

HTML sayfasında include edilen kütüphanelerin (script, css vs) taglerindeki url'lerde cross domain'e izin verilir. İşte bu noktada jsonp ie bir açıktan yararlanılır:

JSonp ile istek yaparken HTTPRequest isteği isteği yapılmaz. Sayfaya script include edilir. Bu include edilecek değer ise sunucudan gelecek olan response'tur.

```js
var elm = document.createElement("script");

elm.setAttribute("type", "text/javascript");

elm.src = "http://example.com/jsonp";

document.body.appendChild(elm);
```

Burada dönen cevap bir metodu çağırır. Metod bizişm javascript tarafında tanımlı olduğundan ona veri olarak parametre gönderir. İşte tüm bunları simüle eden bir fonksiyon yapıp HTTPRequest-Ajax işlemi yaparmış gibi cross domain kuralını aşarak işlem yapabilir.

# jsonP vs cors
Cors web tarayıcıları tarafından önerilen bir yöntemdir.

jsonp'nin dezavantajları:

(tüm dezavantajlar \<src\> eklenip yapılmasından kaynaklıdır.)

- sadece get isteği yapılabilir
- excepiton handling mekanizması ajax request yönetimine göre daha kötüdür. sadece request'te hata olup olmadığını görebiliriz.
- headerları seçip yollayamayız yada gelene header'ları göremeyiz

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Critical section

Paralel execute edildiğinde dead-lock olabilecek bölgeye verilen özel isim.

# race condition

birden fazla thread'in olduğu yazılımlarda aynı kaynaklara eriştiklerinde bazen sorunlar olabiliyor. aynı anda datayı değiştirdiklerinde diğeri bunu görmüyor vs.. Fakat bu durum her test yapıldığında olmuyor. çok nadiren yada her zaman olabiliyor. bu tarz hatalarda kodun, race condition'ı sağlanmadığı söylenir.

# Mutex (yada Mutual Exclusion yada Karşılıklı dışlama) vs Semafor (yada Semaphore)

Semafor kelime anlamı: ulaşımda kullanılan işaret

sadece 1 işlemin o anda belirtilen kod bloğu içerisinden geçmesini sağlar. Oysa semaforlarda bu limit 1 den fazla olabilir. Semaforlarda örneğin en fazla 10 kişi sunucuya bağlanacak diğerleri sırada bekleyecek denilebilir.

# binary Semaphore vs counting Semaphore
binary; mutex gibi sadece 1 işlemin o anda belirtilen kod bloğu içerisinden geçmesini sağlar. counting Semaphore'da bu sayı birden fazladır.

binary Semaphore ve mutex aynı işi görür. fakat yapısal olarak farklıdırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JNI (yada Java Native Interface)

Java ile native kod'u çağırma ve native kod'dan javayı çağırma işlemlerinin yapılabilmesini sağlar. aşağıdaki örnekte görüldüğü gibi navite keywordu ile başka dilden metod çağırmaktayız.

```java
public class Native {

        static {

                System.loadLibrary("native_library");

        }

        public static native int sum(int x, int y);

        public static void main(String[] args) {

                System.out.println(sum(3, 5));

        }

}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Inter-process communication (yada IPC)

İşletim sistemleri üzerinde yürüyen işlemlerin birbirleri arasında haberleşme (uzak metod çağırma işlemi dahil) mekanizmasıdır.

# DBus (yada D-Bus)

Açık kaynaklı bir IPC kütüphanesidir. Birçok implementasonu ve default bir implementasyonu vardır.

# Unix-domain socket

IPC için uygulamalar birbirlerine veri trasnfer ederken; örneğin; localhost:7011 üzerinden de haberleşebilir. fakat bu best practice'lere uymaz. 3 sebepten;

- TCP/IP soket kuralları ile bu işin olması tercih edilmeyebilir

- 7011 portuna herkes istek yapabilir. bu güvensizdir.

- IPC metodları localhost'ta daha hızlı çalışır. localhost/loopback işlemlemleri zaman kaybettirebilir.

IPC işlmelerinde unix-like'larda "unix-domain soket" kullanılır.

# dbus

her yazılım kendi içinde dışarıya birçok path (servis grubu açabilir). her path tüm işletim sisteminde uniqeu olmalıdır. bu path'lerin her biine bus denir. örnek:

> /com/appname/account

yukarıda account servisi var. bu servis içinde birçok parametre alan metod olabilir.  

örnek java d-bus kodu:

```java
import org.freedesktop.dbus.DBusInterface;

public interface ReceiveInterface extends DBusInterface {

   public int echoMessage(String str); // "member" terminolojik adı.

}
```

main app code:

```java
try {

    DBusConnection conn = DBusConnection.getConnection(DBusConnection.SESSION);

    conn.requestBusName("com.myapp"); //bu tüm OS'ta unique olmalı. bu değeri d-bus kendi içinde kullanıyor.

    conn.exportObject("/com/myappname/accounts", new JavaReceive()); //we export a service here. JavaReceive burada "object" deniliyor.

} catch (DBusException e) {

    e.printStackTrace();
}
```

send signal:

```java
Message message = new Message("/remote/object/path", "MethodName", arg1, arg2);

Connection connection = getBusConnection();

connection.send(message);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Oyun moturu (game engine)

oyun geliştirmek için directX, opneGL gibi framework'lerin üzerine daha hazır API'ler sunarak, oyun yazılımını hızlandırmak amaçlı yazılmış kütüphanelerdir. Game engine'ler içerisinde audio engine, Physics engine, yapay zeka gibi API'ler bulundururlar.

# rendering engine vs game engine

Oyun içerisindeki modeller (insan, araba gibi) modelleri ve API'leri oyun motorları bulundurur. Fakat bunları derleme işlemleri için render engin'e ihtiyaç vardır. Render engin opengl, directX olabilir fakat embbed bir renderda içerebilir, yada belirli kısımları kendi derleyip bazı kısımları cross-rendered da tasarlanmış olabilir.

# Unreal vs Unity vs Source

Cross platform (çapraz platform) kapalı kaynak oyun motorlarıdır. Source Engine, Valve firması tarafından geliştirilmekte ve Counter-Strike, Half-life gibi oyunlarda kullanmıştır.

# Unity Web Player

Geliştirilimi durdurulmuş bir tarayıcı eklentisidir. BU eklenti ile Unity oyunları  tarayıcı üzerindeki sayfalardan direk oynanabilmektedir.

# Quake

Quake oyununda kulla ıldığı için ismi aynı olan, açık kaynaklı oyun motorudur.

# Render Engine

Önceden tasarlanmış modelleri işleyerek arayüze sunan uygulamadır. HTML rendering ve graphic rendering gibi birçok çeşit render sayılabilir.

# Blender

Açık kaynaklı 3d modelleme yapılması sunan tool'dur. "Blender Game Engine" de içermektedir. "Autodesk 3ds Max"'e alternatiftir.

# graphic engine

genel bir tanımdır. "Window system" yada opengl bu tanıma uymaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# kernel (yada çekirdek)

"işletim sistemi" ve "işletim sistemi çekirdeği" farklı kavramlardır. işletim sisteminin çekirdeği değişebilir, fakat kendisi aynı isimde sürüm çıkarmaya devam edebilir.

# UNIX

işletim sistemi çekirdeğidir.

# Darwin

Apple çok eskiden UNIX işletim sistemi kullanıyordu. Daha sonra lisans sorunları sebebi ile BSD çekirdeğini kullanmaya başladı. BSD'den türettikleri işletim sistemi çekirdeğine Darwin adı verildi.

# BSD (yada Berkeley Software Distribution)

İşletim sistemi çekirdeğidir. UNIX'ten çatallanmıştır.

# Linux

Linux, UNIX çekirdeğinden çatallanmamıştır.

# Solaris (yada eski adı: SunOS)

İşletim sistemi ve kernel ile aynı isimde dağıtılmaktadır. çekirdeği, UNIX'ten çatallanmıştır.

# Unix-like operating system (yada UN*X yada \*nix)

BSD, Linux, Unix ve türevlerini kapsayan işletim sistemi ailesidir. Resmi bir UNIX Spesifikasyonu var, fakat sertifikayı almamış bir çok Unix-like sistemlerde mevcuttur.

# POSIX (yada Portable Operating System Interface for Unix)

IEEE (yada Institute of Electrical and Electronics Engineers) tarafından belirlenen, tüm işletim sistemlerinde geçerli olabilecek bir standartlar ailesidir. Resmi olarak sertifika almamış bir çok POSIX destekli (yada kısmen POSIX destekli) işletim sistemleri vardır. Tüm standartları sağlamayan fakat kısmen standartlara uyan işletim sistemleri vardır. Bunların arasında MS Windows, BSD ve Linux vardır. Solaris ve MacOS, tamamiyle POSIX desteklidir.

POSIX standartları temelde şunlara benzer: İşletim sistmei üzerinde çalışan yazılımların birbirleri arasında yollayabildikleri sinyallarin tipleri, işletim sisteminin process ve thread yönetimi, tarih ve zamanlama konularında standartlar, desteklenen encoding'ler vs...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Window System

İşletim sisteminde GUI (Input Ouput - keyboard, mouse vs dahil) pencerelerin yönetimini sağlayan sistemdir. Tüm sistemi temsil eder. Hangi pencere açık, statüleri ve meta bilgileri burada tutulur. (Aşağıda anlatılan hem sunucu hem client tarafının tümünü temsil eder)

# X Window System (yada X yada (en çok kullanılan sürüm 11 olduğundan) X11)

Bir Window System'dır.

# Display Server (yada Window Server)

Display Server bir sunucudur. Display server pencere meta bilgilerini tutar. Bunları, window manager ile birlikte window system'ın client tarafına sunar. client tarafı birden fazla olabilir. Zira uzaktan aynı makineye bağlanan ve kullanan birileri olabilir. Burada pencerelerin GUI'lerinin dekorasyonu ve işlevlerini window manager sunmaktadır. Sunucu ve client aralarında Telnet kullanarak haberleşirler.

# X.Org Server

X Window System için sunucu tarafıdır (başka alternatiflerde piyasada mevcuttur). Bazı kaynaklarda kısaltılarak "X server" olarak kullanılıyor. Fakat "X Server" iki anlama da gelebilir: "X.Org Server" kısaltması, yada "X Window System" için sunucu tarafı. "X Window System" için birden fazla sunucu tarafı implementasyonu (yazılımı) olduğu için karışıklığa sebep olabilir.

# $HOME/.Xauthority dosyası

Bu dosya home'a sahip olan user'ın "x.org server"'a eriştiğinde kullandığı session'ın authantication token'ını saklar. eğer bu dosyaya yazma yetkisi yok ise o user görüntüyü alamaz. bu sebeple sudo ile açılan komutlarda bu sorun yaşanabiliyordu. yine bu sebeple gksudo komutu geliştirildi. 

# Xresources
varsayılan olarak ~/.Xresources veya ~/.Xdefaults dizinindeki tutulan property'lerdir. bu property'ler her program için spesifik yada genel olarak tüm pencerelerin yapısı (rengi, textlerin fontu...) hakkında bilgiler tutar.

# Mir vs Wayland

X çok eski bir altyapıya sahiptir. Bu sebeple X'e alternatif açık kaynaklı projeler geliştirilmiştir.

# Window Manager

Windows System üzerinde pencerelerin yönetimi (işlevlerini belirleyen: minimize, maximize, always on top, dekorasyonu gibi) sağlayan yazılımdır. tiplerine göre 4'e ayrılırlar.

- Compositing Window Manager (yada compositor)

  pencereler birbirleri üzerine binebilirler. ek olarak transparan yapabilme gibi birçok özleliğe sahiptirler çünkü her pencere objesini manipüle edebilirler.

  örnekler:

  Compton, sway, Compiz, KWin, Metacity, Mutter, Xfwm, Enlightenment, Xcompmgr, Gala, Muffin

- Stacking (yada floating)

  pencereler birbirleri üzerine binebilirler (bu durum overlapping olarak adlandırılır). pencereleri manipüle etme gibi bir durumları yoktur.

  örnekler:

  2bwm, aewm, AfterStep, amiwm, Blackbox, cwm, Fluxbox, FVWM, IceWM, JWM, Matchbox, mwm, Openbox, Sawfish, twm, Window Maker, eggwm, evilwm, Flwm

- Tiling

  pencereler birbirleri üzerine binemezler.

  örnekler:

  i3, Ion, ratpoison, wmii, StumpWM, larswm, Qtile, Herbstluftwm, Bspwm, EXWM, howm, Notion, Ratpoison, subtle, sway, way-cooler, WMFS, WMFS2

- dynamic tilling (yada dynamic)

  Tiling ve Stacking arasında geçiş yapabilen mod'ları mevcuttur.
 
  örnekler:

  - dwm: 2000 satırı geçmeyen sadece C ile yazılmış kod olacak şekilde tasarlanmaktadır. sadece temel görevleri yerine getirmek için tasarlanmıştır.

  - awesome: dwm türevidir. 2000 satırlık kod limiti bu fork'da aşılmıştır. aynı zamanda lua kullanır.

  - spectrwm, xmonad, catwm, echinus, FrankenWM, Qtile, wmii

# Masaüstü yöneticileridir (yada desktop manager)

EN üst seviyeli GUI ortamıdır. Masaüstlerinde sunulacak özellikleri (widget, notification system, simge panelleri, ayarlar sunan yazılımlar, hazır gelen yazılımlar gibi) belirlerler.

Bazı Windows Manager'lar desktop manager olmadan kullanılabilir. Fakat sadece windows system tek başına (window manager olmadan) son kullanıcı için bir işe yaramaz.

# desktop manager'lar:

- budgie: gnome 2'inci sürüme benzer, sıfırdan yazılmış

- KDE Plasma: en gelişmiş DM olma yolundadır

- Gnome: genel olarak kde'ye göre daha hafiftir

- Xfce: genel olarak gnome'a göre daha hafiftir

- LXDE: xfce'ye alternatiftir

- MATE: Gnome'un 2'inci sürümünden devam eden bir akımdır.

- Cinnamon: Gnome'un 3'üncü sürümünden devam eden bir akımdır. gnome shell'e alternatiftir. gnome için kabuktur.

- LXQt: LXDE'nin qt tabanlı türevidir.

- Budgie

- Sugar: cocuklar icin tasarlanan arayüzde çok kısıtılı seçenek sunan masaüstüdür

- Moksha: Enlightment projesinden türemiş, masaüstü yönetici olmaya odaklanmış bir projedir.

- lumina: hafif olmayı amaçlar

- Pantheon: elementary OS için yaratılan proje

- manokwari: gnome üzerine kurulu shell (unity gibi)

- Deepin DE (yada DDE)

# KDE Plasma vs KDE

KDE, projenin genel ismidir. KDE; "Plasma masaüstü yöneticisi", "KDE uygulamaları"" ve "KDE kütüphaneleri (frameworks)"" üçlüsünden oluşur. Sonuçta asıl amaç; masaüstü ortamı sunmak olduğu için; KDE; "K Desktop Environment" kelimelerinin kısaltmasından oluşmaktadır.

# KDE Neon

Ubuntu LTS bazlı KDE yüklü gelen, KDE takımı tarafından geliştirilen OS. Kubuntu LTS varken nedenböyle bir OS geliştiriliyor? Çünkü Ubuntu LTS versiynlarında stabilite gereği hiçbir uygulama (masaüstü ymneticiside dahil) her zaman güncellenmez. Güncellensede genelde sadece güvenlik paketleri güncellenir. KDE takımı ise sadece kendi repolarını update edecek şekilde ayarlamıştır Neon içerisinde.

# GNOME Shell vs Unity

Gnome 3 üzerine inşa edilmiş farklı masaüstü yöneticileridir. Unity, Ubuntu tarafından geliştirilmekte, Gnome Shell ise, Gnome takımı tarafından geliştirilmektedir. Bu yüzden gnome-shell, gnome projesinin varsayılan (yada referans) masaüstü olmaktadır. Gnome Shell'in eklenti altyapısı mevcuttur.

Gnome, 3'üncü sürümüne kadar tek başına bir masaüstü yöneticisi olarak paketlenip sunulmaktaydı. 3'üncü sürümü ile artık her topluluk/şirket bunun üzerine ek bir kabuk yazarak dağıtmak durumunda kalıyorlar (gnome shell, unity, cinnamnamon gibi).

Bazı işletim sistemleri, gnome 3'ü hala sade (eski haline benzer) olarak gösterebilmektedirler. bunun birkaç yöntemi var:

1- GNOME Flashback (yada GNOME Fallback): gnome 3 ile eski görüntüyü sağlamak için bazı modüller devre dışı bırakılıyor ve shell'dekinden farklı bir window manager kullanılıyor. + eski panel kullanılıyor.

2- GNOME Classic (yada GNOME Classic (no effects)):  gnome 3.8 sürümü ile resmi olarak; birden fazla eklenti ile eklentiler ile eski görünümü gnome shell üzerine verebiliyor.

Gnome 2'den 3e geçiş sürecinde oturum açma ekranlarında işletim sistemleri kendilerince isimlendirme kullandıkları için karışıklığa sebep olmaktadırlar.

# Ubuntu Gnome

Ubuntu'nun türevi. Masaüstü yöneticisi olarak sadece "Gnome Shell" yüklü gelmektedir.

Ubuntu 18.04 ile artık varsayılan masaüstü Gnome Shell'dir. Ekstra birkaç eklenti yüklü gelmektedir (örnek kısayol panel eklentisi). Bu sebeple Ubuntu Gnome projesine ihtiyaç kalmamış ve proje sonlanmıştır.

# Display manager (yada  login manager)

işletim sistemi login ekranında çıkan yazılımdır. masaüstü yöneticisin yada sadece pencere yöneticisini başlatmak için görevlidir. komut satırı yada GUI tabanlı olabilirler.

- Console

  - CDM
  - Console TDM
  - Ly
  - nodm

- Graphic

  - Entrance - An EFL based display manager
  - GDM - The GNOME display manager.
  - LightDM - A cross-desktop display manager, can use various front-ends written in any toolkit.
  - LXDM - The LXDE display manager. Can be used independent of the LXDE desktop environment.
  - MDM - used in Linux Mint, a fork of GDM 2.
  - SDDM - The QML-based display manager and successor to KDE4's kdm; recommended for Plasma 5 and LXQt.
  - SLiM - Lightweight and elegant graphical login solution. (discontinued)
  - XDM - The X display manager with support for XDMCP, and host chooser.

# X display manager

Sadece X session'u başlatan yazılımdır (display manager'dır).

# GTK (yada eski adı:GIMP Toolkit yada eski adı:GTK+) vs Qt vs Java Swing vs Java AWT

Widget toolkit'lerdir. Penrecelerdeki butonların ve diğer nesnelerin nasıl olacağına ve event'lere karar veren kütüphanelerdir. Window manager ekrana bir pencere açtığında bu kütüphanelerden yararlanır. Doğal olarak; default olarak her window manager'ın yanında bir widget toolkit olmalıdır. Bazı uygulamalar kendi içerisinde widget toolkitleri gömerler (java ile yazılmış bir uygulamanın swing'i kendi içerisinde bulundurması gibi, GIMP'in GTK'yı bulundurması gibi). Böyle durumlarda windows manager o uygulamayı, uygulamanın kendi widget toolkit'i ile açmak zorunda kalır. böyle durumlarda ekranda bazen sıkıntılar yola açabilir. çünkü her pencerede, bir çeşit toolkit aynı anda çalışıyor olur. bir yazılım, esnek tasarlanmış ise birden fazla toolkit ile başlatılabilir.

# Qt

açık kaynaklı bu proje, ağırlıklı olarak C++ üzerinden, QT widget'ları ile uygulama yazılmasını sağlar. Qt projesi, sadece widget toolkit değildir. Aynı zamanda bir IDE (pencere tasarlama modülü içeren) sunmaktadır. QT, sadece GUI için değil, aynı zamanda C++ için veritabanı, matematik gibi bir çok işlevi yerine getiren API'lerde sunmaktadır. Diğer diller C++ kodunu çağırarak yada C++ kodu diğer dilleri çağırarak runtime sırasında çağırarak yada derleme sırasında c++/qt binary'lerini kullanarak işlemleri yapmaktadırlar.

# Remote desktop using X

X sunucuları uzaktan bağlanılmak için gerekli altyapıya sahiptir. X sunucusuna birden fazla client aynı anda istek yapabilir. AYnı kullanıcı oturumu içerisinde dahi birden fazla client istek yapıp cevap alabilir. X sunucularından gelen bu istekleri uzak makinada X client karşılamak zorundadır. X client MS Windowsta olmadığı için uzak makine orada yapılamaz. Fakat Apple cihazlar için apple'ın kütüphane esnekliği ile open source X11 client'ı mevcuttur. Apple makinede x client kurulu ise uzak masaüstü yapılabilir. X org sadece masaüstünü kople değil, uygulama penceresi bazında da uzağa yansıtma yapabilir. Uzak masaüstü kendi masaüstünde uygulama açarmış gibi uygulamayı kullanabilir.

SSH üzerinden uzak masaüstü rahatlıkla yapılabilir. Komut satırından SSH ile username ile login olduktan sonra, komut satırından örneğin firefox yazılır ise, firefox ssh yapılan makinede açılacaktır.

X11'in bu özelliğini kullanmayan diğer uzak bağlantı uygulamaları (örneğin teamviewer), sadece ekranın yada bir bölümünün ekran görüntüsünü çekerek uzak makinaya aktarırlar. Oysa X11'deki olay direk olarak native bir çalışma sergilemektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# on-screen display (yada OSD)

bir yazılımın üzerinde açıldığı zaman ekranın her zaman önünde duran pencere yapısına verilen genel isimdir. örneğin; işletim sistemlerinde yada TV'lerde, ses açılıp arttırıldığında ekranda çıkan ses göstergesi OSD'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Opt-out vs Opt-in (yada Double Opt-in)

izinli pazarlama (reklam mail/sms'leri yapma) altındaki kavramlardır.

opt-out; kullanıcının onayı alınmadan reklam içerikli email gönderilebilir, ancak kullanıcı dilediğinde, çok kolay bir şekilde mail/sms listesinden çıkabilir, anlamına gelmektedir.

opt-in ise; kullanıcıya önceden izni alınmadan hiçbir reklam gönderilemez. kullanıcı reklamları kabul ederse, tekrardan istediği zaman çıkabilmesi gereklidir.

opt-in ve opt-out terimleri en çok reklam sms ve emailleri için kullanılsada, genel olarak herhangi bir 'hizmet/servis' içinde kullanılabilir. 

- opt ingilizcede 'tercih etmek' anlamına geliyor
- opt-out ingilizcede 'ayrılmak/vazgeçmek' anlamına geliyor

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Do Not Track

web siteleri kullanıcılardan istatistik toplar. web tarayıcıları http header'larına, kullanıcın bilgi toplanmasını istemediğini yada istediğine dair bir metadata ekler. sitede buna göre davranır. yasal süreçlerin getirmiş olduğu bir zorunluluktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# google chrome os vs chromium os

- embeeded adobe flash

- auto updater

- Name and logo tradememarks

- donanıma özgü performans paketleri içerir

- resmi destek google ve/veya donanım üreticisi tarafından verilir. chromium için topluluklar ile temasta olmak gerekir.

- chromium browser (look at: chromium browser vs google chrome browser )

- ve daha fazlası

# chromium browser vs google chrome browser

- default installed extensions

- embeeded auto updater (bu modül işletim sistemi versiyonu vs hakkında istatistik topluyor)

- embeeded adobe flash (linux paketinde de geliyor)

- Media codecs to support H.264, AAC and MP3 formats

- Name and logo tradememarks

- işletim sistemine kurulum sırasında bir ID oluşturuluyor ve google'a gönderiliyor.

- isteğe bağlı olarak birçok bilginin istatistik olarak google'a gönderilmesi seçeneği (chromium üzerinde bulunan diğer hizmetlerde de bilgi gönderiliyor. detaylar farklı başlıkta yazıyor)

- resmi destek google tarafından verilir. chromium için topluluklar ile temasta olmak gerekir.

# notlar

- "built-in PDF viewer" and "print preview" future added both browsers after chromium 47 version.

- chromium has no release tags inside code review system. there is no way to find stable versions. açık kaynağı destekleyen topluluklar, kodun stabil olduğunu düşündükleri bir noktadan, debian tabanlı sistemler için paket çıkmaktadırlar. şimdilik en stabil versiyonu bu.

- html ve javascript derleyicilerinde bir farklılık yok. fakat buna rağmen chromium ile teknik bir kaşrılaştırma yapmak mümkün diil, çünkü chromium için aynı noktadan paket çıkılmış versiyon bulmak pratikte imkansız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# javascript promise

promise keywordü daha çok kullanılsada farklı kütüphaneler, yada programlama dilleri farklı isimlerde kullanmaktadır: Future, delay, deferred

Javascriptte parametre olarak fonksiyon geçilebilmektedir. bu şekilde error, success gibi event'lerde o callabck'lerimiz çağrılmaktadırlar. fakat promise ile; parametre geçmek yerine, promise bize bir nesne (promise nesnesi) döner. biz bu promise nesnesinin success fail gibi metodlarına ilgili callback metodlarımızı ona yollarız. daha sonra promise metodumuzu run ederiz. aslında farklı yöntemlerle yapılabilecek bir iş. fakat promise yöntemi okunabilirliği arttırmaktadır, standart olmasını sağlamaktadır ve eğer native Promise kütüphanesinden yararlanıyorsak, asenkron thread işlemi yapabilmemizi sağlamaktadır.

promise aslında programlama dilinden bağımsız bir best practice'dir. ecmascript 6 ile Promise nesnesi native olarak gelmiştir. daha önceki ecmascript sürümlerinde Promise kütüphanelerle yapılmaktaydı. güncel ecmascript promise kullanımı şu şekildedir:

```js
var sendMessage = new Promise( (successCallback, errorCallback) => {

$.ajax({

      url: "www.google.com/request";

  

}).then(function(data) {

     successCallback(data);

        }).catch(function(data) {

             errorCallback(data);

        });

});

sendMessage.then(function(data) {

      

     //success code

}).catch(function(data) {

    //fail code

});
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Chromecast

Google'ın geliştirdiği bir cihaz. USB bellek boyutundaki ufak cihaz, USB girişi şekilde HDMI girişi mevcut. HDMI girişini direk televizyona bağlıyoruz. Bu şekilde televizyonda akıllı-TV özelliği olmasa bile, chromecastin içerisindeki işletim sistemini HDMI aracılığı ile kullanmaya başlıyoruz. Cihazın USB girişi de mevcut. BU girişi ile cihazı aynı zamanda paralelden elektrik alması için beslemek gerekiyor.

chromecast içerisindeki işletim sisteminde uygulama çalıştırılmıyor. chromecast ile aynı wi-fi'deki herhangi bir cihazın içerisindeki uygulama, chromecast'e istediği ekran görünütüsünü ve sesi aktarıyor. chromecast'e ekran görüntüsü atan yazılımlar: android, ios, chrome-os, chrome eklentisi ile google-chrome.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jasper Reports

Jasper report; raporun bir kere tasarlanması, çıktının ise dilediğiniz formatta (pdf, xml, csv, txt, vs.), kodda veya tasarımda değişiklik yapmadan üretilmesini sağlayan sistemdir.

Raporlar belli bir formatta hazırlanır. Bu raporlar dilediğimiz formatta runtime sırasında çıktıyı verirler.

JasperReports ve iReports'un geliştiricileri daha sonra birleşerek JasperSoft isimli profesyonel bir şirket kurdular ve şu an Jasper Reports konusunda danışmanlık veriyorlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ternary operator
ternary kelime anlamı: üçlü, üç parçadan oluşan

programlama dillerinin desteklediği bir syntaxtır. if bloklarını bu şekilde yazabilmemizi sağlar:

```js
var y = x === 4 ? `4'e eşit` : `4'e eşit değil`;
```

# unary operation
unary kelime anlamı: birli, sadece 1 parçadan oluşan

Sadece bir operand'a sahip olan operasyonlara unary operation adı verilir. 

```c
++x;
x--;
```

# unary function

Sadece bir argüman alan fonksiyonlara unary function adı verilir.

```js
var func = x => x + 1;
```

# n-ary function and operations
unary, binary, ternary şeklinde devam etmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •


# Amazon Elastic Compute Cloud (Amazon EC2)

Amazon firmasının, Amazon Web Services (AWS) isimli hizmet gruplarından bir tanesidir. Uzak masaüstü ile bağlantı yapılabilen sunucular (sadece makina) sunuyor. Bunlardan sistem gücüne göre saatlik ücret alıyor. Bu makineler üzerindeki HDD partitionlarının backup'ları anında alınabiliyor (Amazon Machine Image - AMI) ve tekrar kullanılabiliyor - public olarak dağıtılabiliyor.

Amazona ücret ödendikten sonra verilen kullanıcı ve şifre ile uzaktan Amazon API ‘leri kullanılarak programatik olarak yeni makine yaratıp kapatılabiliyor. Bu şekilde gereksiz makinalar kapatılarak ücretten kar edilmiş olunabiliyor.

# Regions and Availability Zones

Amazon belirli bölgeler arasında sunucularını birbirinden bağımsız tutmuş (maaliyet gibi sebeplerden). Bu sebeple bu bölgeler arasındaki makinelerdeki haberleşme normal internet üzerinden yürümekte, bu sebeple daha yavaş haberleşme olmaktadır. Backup alma işlemleri kısıtlı yürüyebilmektedir gibi.

# Elastic Beanstalk

AWS'nin bir alt hizmetidir. Heroku alternatifi bir sistemdir.

# amazon networks:

büyükten küçüğe doğru:

- region (asia vs…): maliyetten düşülmesi için amazon coğrafik konum olarak  yerlerde biribrindenbağımsız sistemler kurmuş. biz bunlar arasındaki sanal ortamlara erişmke istediğimizde, makinelerimizin public ip'lerimizi kullanmamız şart.

- vps (yada Virtual Private Cloud): Amazonun bulutta diğer network'lerden izole şekilde kullandırttığı network kümesidir. Amazon bulut makinaları subnet'lr altında gruplanır. Bu subnet'ler birbirine erişebilir. Fakat bu subnet'ler gruplandırıldığında VPC'ler altında toplanırlar ve birbirlerine erişemezler. VPC kısaltması birçok farklı ismin kısaltması olduğundan bazı yerlerde karışıklığa sebep olabilir.

- availibity zone

- subnet

# elastic ip

elastic ip kullanıcının makineye atadığı public ip'dir. makine yeniden oluşturulduğunda değişmez. "public ip"' ise her makine yeniden oluşturulduğunda değişir.

# security group

varsayılan olarak tüm makinelerin tü portları dışarıya kapalıdır. elle kullanıcı rule tanımlamalıdır. tanımlanan bu rule'lar gruplar altında toplanmaktadır. bu grubun ismi security group'tur.

# Network Interface

her makinada en az bir tane olmak üzere Network Interfaces attach olmuş olmalıdır. bu network kartı'dır. Bir makine yarattığımızda default Network Interface tanımlanır. bu interfaceyi direk başka bir cihaza atach edebiliriz. default ismi eht0'dır.

# device farm

amazonun mobil uygulamalr için test servisi. verilen testleri istenilen tüm cihazlarda koşmaktadır. sonuçları videolu şekilde kaydetmektedir. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DSL (Domain Spesific Language)

Kendi programlama dilinizi özel bir iş için geliştirdiğinize bu dil DSL olur. DSL sayesinde belirtilen syntax'ta ki yapılar, programlama diline çevrilir ve işlenir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# serverless

sunucu tarafta, businnes logic'i dışındaki işlerle ilgilenmemek için güvenlik, oturum yönetimi, ölçeklendirilebilirlik, kurulum konfigürasyonları, ağ yönetimi gibi konular için dışardan hazır hizmet alınması tercih edilmektedir. bu da servlerless mantığını ortaya koymuştur. isminden server olmama gibi anlaşılabilir, fakat olay bu değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Omniture vs Google Analitics vs CoreMetrics

Piyasada en çok tercih edilen istatistik toplama ve analiz etme sistemleri. Geliştirilen yazılımda, son kullanıcının yaptığı her aksiyon bu sitelere belirli formatlarda yollanıyor. BU siteler ise, bu bilgilerle ilgili istatistikleri yazılımcılara sunuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Xray

Test yönetimi sunan Jira eklentisi. ‘Test execution', ‘Test' gibi issue tipleri yaratıp bunları arayüzdendüzenlemeyi sunuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Selenium

web sayfaları testi için kullanılan yazılım paketleri.

# Selenium IDE:

Firefox eklentisi. Bu araç kullanılarak tarayıcı üzerinde elle yapılan işlemler script'lere dökülebilmektedir. IDE eklenti desteklemektedir.

# Selenium Client

Programlama dilleri için geliştirilen API. Test scriptleri herhangi bir dilde yazılıp, execute edilebilir.

# Selenium WebDriver

API üzerinden yapılan istekler web-driver tarafından execute edilir (tarayıcıda çalıştırılır). Bu yüzden webdriver browser spesific yazılmaktadır. Selnium Web driver'ına gelen istekler sadece Selenium API ile olmayabilir. Aynı komutları kim gönderirse, uygun cevap dönülmektedir.

# HtmlUnit

Selenium'un geliştirdiği simülasyon tarayıcısıdır.

# SeleniumGrid (yada Selenium Standalone Server)

birden fazla web driver olan test makinesi yönetimi içi kullanılan yapının genel ismidir. java ile yazılmış bir uygulama mevcut. bu uygulama ya sunucu yada node rolünde başlatılabilmektedir (ikiside tek executable). sunucu bir adet olmalıdır (seleniumHub). node'lar ise istenildiğikadar olabilir ve sunucuya attach (register) olmaları gereklidir. Tüm test istekleri Hub'a gelir. Hub ilgili yönlendirmeleri uygun node'a havale eder. Bu şekilde birçok test paralelden çalıştırılabilir.

bir selenium node'unun konfigürasyonlarına webdriver dizini verilmelidir. bu şekilde ilgili node, hangi tarayıcıların driver'lerini çalıştırabiliyorsa, o testleri üzerinde yapmaya hazır şekilde bekler.

# Selenium RC

Eski sürüm selenium'larda kullanılırdı. WebDriver yerine bu vardı.

# SeleniumGridScaler

selenium-grid'in eklenti altyapısı mevcut. pom.xml'e selenium-grid eklenirse, bu sınıflardan yararlanarak bir java projesi oluşturulursa bu java projesi artık bir eklenti olmuş olacaktır. selenium-grid başlatıldığında, SeleniumGridScaler'ın install edilmiş hali (jar) class-path'e verildiğinde SeleniumGridScaler'ın kodları da yürütülmektedir.

SeleniumGridScaler eklentisi ile amazon AMI'leri üzerinde node'lar sadece ihtiyaç olduğunda açık tutulurken, ihtiyaç olmadığında kapatılıyor. bu şekilde maliyet kazancı sağlıyor.

# RemoteWebDriver

webdriver'dan extend etmiş bir sınıftır. uzakta driver servlet'i var ise ona bağlanmak için kullanılır. örnek kodlardan daha kolay anlaşılabilir:

remote example:

```java
String Node = "http://localhost:4444/wd/hub";

DesiredCapabilities cap = DesiredCapabilities.chrome();

webDriver = new RemoteWebDriver(new URL(Node), cap);

local example:

System.setProperty("webdriver.chrome.driver", "/Users/murat/DEV/chromedriver");

webDriver = new ChromeDriver(); //chrome driver'de webdriver'dan extend etmiştir.
```

# jsoup
java html parser'dır. dosya içeriği url'den yada herhangi bir string olarak verilir, html'i parse etmiş şekilde okuyablmemizi sağlayan api'ler sunar. arkaplanda headless web browser çalıştırmaz. bu sebeple js motoru yoktur. bu sebeple sadece statik sayfalar parse edilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sanallaştırma kavramları

# Hypervisor (yada virtual machine monitor yada VMM)

sanal makine yönetimini yapan yazılımlara verilen gelen isimdir.

hypervisor run edildiği katmana göre 2'ye ayrılır:

#### Type-1 (yada native yada bare-metal hypervisors)

işletim sisteminin kurulu olmasını gerektirmez. direk donanım ile temasta olan ilk katmanda bulunur. Hypervisor üzerine bir den fazla işletim sistemi kurulur.

Örnek: Microsoft Hyper-V (linux'u da desteklemektedir), Xen, VMware ESXi (yada eski adı VMware ESX)

#### Type-2 (yada hosted hypervisors)

işletim sistemi üzerine yüklenirler ve sanal makine içindeki uygulamaları host donanımın anlayacağı dile çevirir.

örnek: virtualbox, QEmu (yada Quick Emulator)

# Donanım sanallaştırma (yada platform sanallaştırma)

kurulan işletim sistemlerine donanımları paylaştırma işlemidir. hypervisor'ler bu paylaştırma işlemini yönetir. hypervisor'ler donanımları paylaştırma işlemlerine göre 3'e bölünürler:

- Tam Sanallaştırma: tamamiyle sanal olarak bir donanım yaratılır ve işletim sistemlerine bu donanım kullandırtılır.

- Para-virtualization: gerçek donanımlar direk olarak işletim sistemlerine bağlanır. sanal donanım hiç yoktur.

- Kısmi Sanallaştırma: para ve tam arasında olan bir mekanizmadır. işletim sistemlerine bazı donanımlar sanal verilirken, bazı donanımlar sanal yaratılıp verilir.

# KVM (yada Kernel-based Virtual Machine)

özel bir isimdir. KVM linux modülüdür, hypervisor'dür. işletim sistemi modülü olduğu için type-1 veya 2 olduğu tartışılır. sanal makinede olan işlemleri host'un anlayacağı dile çevirmez. bu sebeple type-1 olarak algılanabilir. fakat çekirdek üzerine kurulu olduğundan type-2 gibi düşünülebilir çünkü tüm guest'ler host makinanın birer process'iymiş gibi çalışmaktadır.

# Parallels Desktop (yada Parallels Desktop for Mac)

mac için hypervisor.

# yazılım sanallaştırma

docker gibi uygulamalra verilen genel isimdir. uygulama çalıştırma seviyesinde olur, işletim sistemleri seviyesinde bir kavram değildir.

# intel VT-x vs AMD-v

Sanallaştırma teknolojileri için işlemcilerin yapmış olduğu yeni teknoloji isimleri.

# Nested virtualization

içiçe sanallaştırma yani; sanal makine içinde sanal makine çalıştırmaktır.

# VMware ThinApp

VMware'in uygulaması, yazılım Sanallaştırması yapar. İşletim sisteminde bir uygulamanın sanal olarak yürütülmesini sağlar. Varolan işletim sisteminin dosyalarını elleyemezler.

# Emülator (yada öykünücü) vs simulator (yada simülatör)

emülatör gerçek sistemi bir sandbox'ta çalıştırıp sunarken, simülatör gerçek sisteme en yakın (benzeri) modeli sandbox'ta sunmaya çalışır. örnekler:

- para-virtualization, emülatör yada simülatör değildir. çünkü donanımı direk olarak yazılıma verir ve üzerindeki sistemler sandbox'ta çalıştırılmaz. sadece işletim sistemleri birbirinden bağımsız (birbirini görme yetkisi olmadan) çalışır. 

- full ve partirial virtualization emülatördür. android sdk'sında gelen android sanal makineleri bu gruba girer. virtualbox bu gruba girer. guest sistemler gerçek setup (örneğin iso) dosyalarından kurulur.

- eğitimlerde kullanılan uçak yada araba oyunları birer simülatördür.

# VirtualBox OSE (yada VirtualBox open source edition)

Virtualbox 4üncü sürümden önce kapalı kaynak ve açık kaynaklı sürüm olarka dağıtılıyordu. 4 üncü sürüm ile birlikte ikisi birleştirildi. ve lisans gerektiren ek özellikler; kapalı kaynak eklentiler olarka indirilebiliyor.

# VMware

VMware'in birçok fakrli ürünü mevcut. Tüm ürünler kapalı kaynaktır.

# VMware Workstation (yada VMware Workstation Pro)

Sanal makina çalıştırıcısıdır. lisans gerektiren versiyon.

# VMware Workstation Player (yada VMware Player)

kisisel kullanim için ücretiz sunuluyor. VMware Workstation'dan farklı bir executable olup, eksik özellikler içeriyor.

# VMware Fusion

Mac'in altyapısı farklı olduğundan, mac versiyonu farklı build üretilmiştir.

# Gnome-boxes

gnome projesi altında dağıtılan sanal makina yöneticisi için GUI'dir. Arkaplanda kendi teknolojilerini kullanmaz. Sadece GUI ile sanal makine yönetim (oluşturma, silme, açma) işlerini kolaylaştırır.

# Virtual Machine Manager (yada virt-manager)

Genel bir terim gibi görünsede özel bir uygulama ismi. Gnome-boxes gibi sadece GUI'dir.

# LXD

Canonical firması tarafından geliştirilen LXC altyapısı üzerine kurulu, docker alternatifi sanallaştırma yazılımıdır.

# LXD vs Docker vs LXC

Docker genelde bir uygulamayı container üzerinde çalıştırmayı amaçlar ve bunun için altyapı sunar. LXD komple bir işletim sisteminin uygulamalarını (services, apps...) container üzerinde yürütmek için altyapı sunar. LXC, docker ve LXD'nin çekirdeğindeki yazılımdır (daha sonra docker LXC'yi değiştirerek libcontainer yapısına geçmiştir). LXC'nin son kullanıcı tarafından direk kullanılması zordur. Bu yüzden docker ve LXD gbi projeler ortaya çıkmıştır. 

# chroot
unix-like sistemlerde bir dizini root olarak belirleyip, o dizin içerisinde bir uygulamayı run etmeye yarayan programdır. run edilen uygulama o dizini root olarak görür ve üst dizinleri kullananamaz. bu durum run edilen uygulamayı hapse atmaya benzetilebilir. fakat pratikte; bir uygulama birçok kütüphaneye, dosyayı kullanır. böyle durumlar için uygulamayı run etmeden önce, istenilen dizinleri, chroot altındaki dizinlere mount etmek gerekir.

bir uygulamayı execute ederken, sanal dizin altında olabilmesini sağlayan bir komuttur. Chroot'un Sanallaştırma gibi bir amacı yoktur.

chrrot ile önce boş bir dizin yaratılır. artık burası koşucak uygulamanın root dizinidir. buralara artık bağımlılıkları da atmak gereklidir. sadece bağımlılıklar diil, posix'te herşey dosya olduğundan stream out in, gibi dosyaları da kaydetmek gereklidir. tüm bir işletim sistemini hazırlamak gereklidir. bu elle zor olduğundan genelde chroot türevi  hazır paket yazılımlardan yararlanılır. yada varolan sistemdeki dizinler direk olarak chroot'un root dizini altındaki yerlere mount edilir. bu şekilde örneğin sistemimizdeki /etc dizinini komple chroot içine bağlamış oluruz.

# Vagrant

Virtualbox, VMWare gibi uygulamaların içeirisinde olan makinaları, dcoker tarzı yönetimini sağlayabilmemizi sağlayan açık kaynaklı bir komut satırı uygulamasıdır.

Örneğin; Virtualbox üzerinde çalışan Windows-guest içerisinde komut çalıştırmamızı, disikin image'ini hazır indirmemizi, gibi işlemleri kolayca yapabilmemizi sağlar. BUnları yaparken VMWare ile de yapabilmemizi sağladığı için tek bir interface aracılığı ile yönetebilmemzi sağlar.

işletim sistemini hazır bulut imagelerden indirip açar, run edip, bir user tanımlar, bu user'ın içine ssh atar ve ssh komutu ile içine girip istediğimizi çalıştırabilmemizi sağlar.

# Rkt (yada Rocket)

açık kaynaklı docker alternatifi uygulama. avantajları:

- container'ların formatlarının açık kaynaklı olması, aynı container'ların Rkt haricinde başka uygulamalarda çalışabilmesini sağlamaktadır.

- docker monolitic (herşeyin gömülü olduğu tek paket) şeklinde dağıtılırken, Rkt daha modüler bir yapıdadır. (moby projesi ile bu artık değişiyor)

- docker deamon root çalışırken, rkt root user'ı ile çalışmak zorunda diildir. container içindeki uygulama root olarak çalışır ve işletim sistemi kernel'i host-OS ile aynıdır. dolayısı ile bir risk yaratıyor. docker daemon işleminin root olması bu riski arttırıyor. docker komut satırı işlemi yapıldığında, arkaplanda root çalışan docker daemon kendi altprocess'i olarak yeni bir container başlatıyor. yani altprocess'te root yetkisi ile çalışıyor.

# vagrant vs docker+rocket

karşılaştırılmaları doğru diil. biri vm container'ı, diğeri uygulama container'ı.

- vagrantın açılması dakikalar alırken, docker saniyeler alır.

- vagran fully portable'dır. fakat docker diildir. docker işletim sistmeini sanallaştırmıyor. varolan OS çekirdeğini kullanıyor. bu sebeple başka makinaya atılan docker container, OS çekirdeği farklı sürüm ise, OS çekirdeği farklı ise farklı tepkiler verebilir. Özellikle donanıma bağlı kodlarda çok büyük farklılıklar oluşabilir. OS çekirdeği aynı ise fakat donanım farklı ise yine container'lar farklı tepkiler verebilir.

# cgroups (yada control groups)

linux üzerinde proseslerin cpu, memory, network gibi kaynaklardan ne kadar öncelikli yararlanabileceği, yada limitli yararlanabilmeleri için yönetimi sağlayan yazılımdır.

# Linux namespaces

linux üzerinde proseslerin hangi kaynakları diğer prosesler ile ortak kullanıp kullanamayacağını belirleyen altyapıdır. örneğin;

- Mount: bir grup proses, bir dizini görebilirken diğer hiçbir proses göremeyebilir.

- Network: birden fazla network interface'si (eth, wifi..) birden fazla network namespace'sine bağlanır. network namespace'si iptable gibi rouing ayarlarını içerir.

- IPC: IPC namespaceleri vardır. bir ipc namespace'si içindeki diğer ipc'lere mesaj atamaz.

- PID: process id grubu oluşturuluyor. bu şekilde, X pid namespacesi'sindeki process'ler sadece brbirlerini görebiliyor.

- uts: domain ve host name'leri okurken farklı gruplar yaratabiliyoruz

- User: bir process başlatıldığında o process'in kendini X user'ı ve Y grubu altındaymış gibi sanmasını sağlayabiliyoruz.

- cgroup: bu kısıtlama yapmaya yarayan cgroup özelliği değildir. bu bir namepscace'tir. çalışacak process'in kendini  bir croup kısıtlaması altında olduğunu sanmasını sağlayabiliriz.

bazı kaynaklarda sadece 'namespace' olarak geçer, bu sebeple karışıklığa sebep olabilir, zira 'namespace' kelimesi birçok farklı alanda kullanılmaktadır.

/proc/<PROCESS_ID>/ns/ dizini altında o process'in tüm namespace'leri ayrı ayrı link dosyalarıdır: 

> /proc/<PROCESS_ID>/ns/ipc
> /proc/<PROCESS_ID>/ns/mnt

İşte bu dosyalar link olduğundan, örneğin X namespace'sine link eder. İşte o proceess'i Y namespace'sine yönlendirirsek o zaman container/sandbox yapmış oluruz. 

# sandbox vs container

conitaner içinde uygulamalar çalıştırabilmek için (yani yeni namepscae'ler yaratabilmek için) root yetkisine ihtiyaç vardır. fakat sandbox için root yetkisine gerek yoktur. örnek docker root yetkisi ister, fakat google chrome sandbox için user yetkisi yeterlidir.

# Flatpak (yada eski isim:xdg-app)

flatpak, sadece unix tabanlı sistemler için geliştirilmiştir. user-yetkisinin yeterli olduğu sandbox teknolojisine dayanıyor (teknoloji adı: Bubblewrap). 

# Bubblewrap vs firejail vs sandstorm

linux'ta user-namespace teknolojisi genişletilerek root yetkisi olmadan sandbox içinde uygulama çalıştırmaya yarayan birbirlerine alternatif teknolojilerdir.

# appImage (yada eski isim:klik yada eski isim:PortableLinuxApps)

tek bir dosya içinde tutuluyor. hiçbir paket yöneticine ihtiyaç barındırmıyor. direk executable'ın yürütülmesi yeterli oluyor.

# nix

nix sistemden bağımsız çalışan paketleme sisteminin ismidir. her uygulama root dizininde store olarak isimlendirilen bir dizinde saklanıyor. her user ise kendi path'inde istediği kısayolları ekliyor. nix bir paket yöneticisidir.

nix mac'te de native olarak çalışıyor. nix namespace teknolojilerini, build sırasında veya nix daemon veya nix komut satırı uygulaması ile işlem yaparken kullanıyor. fakat kurulumdan sonra uygulamalar namespace teknolojileri ile çalışmıyor. bu sebeple firejail gibi uygulamalar nix ile entegre çalışabilirken, snap ile çalışmamaktadır.

# NixOS

tüm uygulamaların nix paketleme yöntemi ile yönetildiği linux tabanlı işletim sistemi.

# Guix 

Nix'in sadece açık kaynaklı paketleri kabul eden türevi.

# nix komutlar
> nix-env -qaP | grep -i vlc

search ignoring case. the list is not same as official web site. her satır bir uygulamayı gösteriyor. ilk sutun program için "attribute name" ikinci ise "package name".

> nix-env -i vlc

install vlc by package name. install komutunun parametresi regex'tir. bu sebeple sadecd "vlc" dediğimizde "vlc" regex'ine uyan tek bir sonuç var ise, o paketi yükler.

> nix-env -iA vlc

install vlc by attribute name

> nix-env -q

list all installed pagakes

> nix-env -e vlc

removes the package

> --arg config '{ allowUnfree = true; }'

bu parametre install ve update package komutlarına geçilebilir. böylece lisanslı paketlerde indirilmeye açık olur.

> nix-channel --update nixpkgs
> nix-env -u '*'

update nix packages.

> nix-collect-garbage -d
when you remove a package with -e, it only removes the symbolic links from user profile. to remove completly the packages from "nix store" use above command.

(with -d parameter we remove also the rollback (old generation) packages)

# remove nix
- If you dont use nix daemon, stop it.

- it is enough to remove the path variables of nix from current OS user. the path file is auto added by nix-installer here:

  $HOME/.profile

  remove this line:

  > if [ -e /home/user_name/.nix-profile/etc/profile.d/nix.sh ]; then . /home/user_name/.nix-profile/etc/profile.d/nix.sh; fi # added by Nix installer

- Also run this command (optionally): "sudo rm -r /nix"

- Also (optionally - because they are only folders) "rm $HOME/.nix-channels $HOME/.nix-profile $HOME/.nix-defexpr"

# nix package kurulumda alınan priority hatası

bir paket kurarken priority hatası alınırsa;

önceden sistemde kurulmuş olan ve çakışan paketin (ekranda paket adı yazar) ismi PACKAGE_X olsun, şu komut çalıştırılmalıdır:

nix-env --set-flag priority 6 PACKAGE_X

Buradaki 6 sayısı hatada gösterilen sayısının bir fazlasıdır. Artık yeni paket kurulumu yapılabilir.

# wrapped vs unwrapped nix packages

ornegin firefox'un her iki prefix ile de paketi mevcut. wrapped olanı eklentilerin/patch'lerin kurulu oldugu pakettir. unwrapped ise saf halidir.

# firejail
bazı komutlar:

(diğer komutlar burada https://firejail.wordpress.com/features-3/man-firejail-profile/)

vlc komutunun sağında kalan her paraetre vlc'ye geçilir
> firejail vlc -file /videos/video.mp4

interneti devre dışı bırakmak için:
> firejail --net=none vlc

X'i farklı bir ekrandaymış gibi açar. bu sebeple firefoxu sanal bir penceredeymiş gibi görürürüz. açılan uygulama sistemden ekran görüntüsü alamayacaktır.
> firejail --x11 vlc

/root ve /home/user dizinleri artık tamamen boştur/sanaldır.
> firejail --private vlc

/home/user dizini artık /path1'dir.
> firejail --private=/path1 vlc

geçici oluşturulan /home/user dizinine path1'deki tüm dosyalar kopyalanır. sandbox kapandığında geçici olan home dizini silinir ve /path1 eskisi gibi kalır.
> firejail --private-home=/path1 vlc
> firejail --private-home=/path1/file1 vlc # sadece file1 geçici home dizinine kopyalanır

iki adet dns algılatırız
> firejail --dns=8.8.8.8 --dns=8.8.4.4 firefox

local network adreslerine erişememesi önceden hazır gelen nolocal.net dosyası
> firejail --netfilter=/etc/firejail/nolocal.net firefox

ip (ifconfig yerine kullanılıyor) komutunu sanal olarak bir network'e atadık. network bilgilerini (örnek mac adres) rastgele firejail tarafından üretiliyor. ip'yi elle verdiğimizi için rastgele üretilmedi. bu sanal network'un bağlı olduğu dış network eth1'dir. eth1 o sıra bilgisayarımızda ayakta olmalıdır.
> firejail --net=eth1 --ip=192.168.0.10 ip -c a

profil dosyasında komut satırında geçeceğimizi bir çok parametreyi bulundurabiliyoruz. bu şekilde komutlarımız daha kısa oluyor ve aynı profil dosyasını birçok  uygulama için kullanabiliyoruz.

eğer --noprofile parametresini vermezsek default olarak aynı executable isminde bir dosyayı firejail'in profilleri tuttuğu dizinde arar. eğer aynı isimde profil dosyası bulursa o profil dosyasını aktif eder.

--noprofile parametresi geçilmezse aynı zamanda şu da olur: default olarak bazı profiller otomatik devreye alınıyor. bu profil dosyaları okunduunda komut satırına log atılıyor.

> firejail --profile=/file.profile vlc

# Guix System Distribution (yada GuixSD)

Guix paket yöneticisi içeren işletim sistemi.

# portableapps.com

sadece windows için uygulamaların portable hale getirildiği projedir. portable uygulamayı çalıştırmak için paket yöneticisine ihtiyaç yoktur.

# snap

container içinde masaüstü/sunucu uygulamaların çalıştırılmasını sağlıyor.

# snappy

snap app manager

# Snapcraft

"snap formatı"'na uygun uygulama paketlemek için gerekli tool.

# snap commands

- snap find vlc --> finds apps on market

- snap install vlc

- snap install --beta vlc 

- snap install --devmode vlc --> snap uygulamasının bazı dizinleri (örnek /etc/) okumaya yetkisi yoktur. bunu aşmak için tüm dizinlere yetki veriyoruz. 

- snap remove vlc

- snap list --all -> lists installed apps

- snap refresh vlc --> updates vlc

- snap disable vlc --> vlc'yi silinmiş gibi yapar fakat dosyaları kalır. eğer arkaplan servisi varsa önce onu stop eder. enable'da ise servisi varsa başlatır.

- snap services start docker --> servisi başlatır. her snap app'te servis olmak zorunda değil. arkaplanda sürekli açık duran sistem servisleri snap tarafından destekleniyor. posix'lerde loglar /var/log dizini altında saklanırlar. snap'in servislerinin log'ları da buraya düşmektedir.

# containerized apps HOME folder

Tüm uygulamalar normalde $HOME idizini içerisine tüm config dosyalarını kaydederler. Fakat containerized app'lerde farklı durum vardır:

- snap: /home/<user_name>/snap/<snap_app_name>/<app_version>/

- portableapps.com: portableappname/Data/ dizinini kullanıyor.

- appimage: executable dosya ile aynı dizinde ve aynı isimde suffixi ".home" olan dizini kullanıyor. eğer yok ise normal sistemdeki $HOME dizinini kullanıyor.

- nix: normal uygulamalar gibi sistemdeki $HOME dizinini kullanıyor.

- flatpak: normal uygulamalar gibi sistemdeki $HOME dizinini kullanıyor.

- docker: container içindeki /root dizinini kullanıyor. 

snap uygulamaları home dizininde şöyle seçenkerlerde sunuyor:

- /home/<user_name>/snap/<snap_app_name>/<app_version>/ --> burası app_version sürümü için home dizini

- /home/<user_name>/snap/<snap_app_name>/common/ --> tüm sürümler için rtak olan home dizini

- /home/<user_name>/snap/<snap_app_name>/current/ --> bu bir link. yüklü olan (enable olan) sürümün home dizinine link ediyor.

Snap uygulamaları home dizini gibi işletim sistemi root dizinini altında /var/snap/ dizininde de her uygulama için read-write yetki olacak şekilde dizin yapısı kurmuş durumda. burası tüm OS user'ları için ortak config'leri barındırıyor.

# containerized apps dizin yapısı

- portableapps.com: /<APP-NAME\>/App/

- appimage: kendisi zaten tek bir (portable) dosya.

- docker: o anda docker runtime'ı, dizinleri contaner'a mount ediyor.

- nix: işletim sistemi root dizini altında tek bir dizinde tüm dosyaları toplanmış durumda:

```
--/nix/

------/store/

------------/<HASH_OF_THIS_FILE_OR_DIR>-<PACKAGE_NAME>-<APP_VERSION>/ --> Burası dizin de olabilir sadece bir dosyada. her dependency içinde kendi dependency'lerini barındırmıyor. tüm dependency'ler de nix/sotree/ içinde.

------/var/

----------/profiles/

-------------------/<PROFILE_NAME_1>/

-------------------/<PROFILE_NAME_2>/

------------------------------------/bin/
```

Her user bir profil'e bağlanır. Her profil ise kendi içinde, nix-store'da yüklü olan aynı uygulamanın farklı sürümlerine bağlanır. Bu sürümlerin komut satırı executable'ları /<PROFILE_NAME_2>/bin/ içinde yer alıyorlar.

Dolayısı ile OS içinde yeni bir user'ın $PATH'ine /<PROFILE_NAME_2>/bin/ dizinini eklemek yeterlidir. artık tüm uygulamalar kullanılabilir olur.

$HOME/.nix-profile --> bu dizin buraya link ediyor: /nix/var/nix/profiles/<PROFILE_NAME_X>

- snap

işletim sistemi root içerisinde birçok dosyayı barındırıyor. fakat ekstradan işletim sistemi ile entegreli çalıştığından her tarafta sembolik linkler ve config dosyaları da barındırıyor.

```
--/snap/

-------/bin/ >-> executables of installed apps

-------/<APP_NAME>/

------------------/<VERSION>/

------------------/current/ ->link to enabled version
```

# container vs VM

container işletim sistemi çekirdeği içermez. vm içerir.

# app container vs system container

resmi bir kavram farklılığı yok. fakat yazılımcılar arasında kulanılan terimlerdir. app container sadece bir uygulama içerir. system container ise birden fazla uygulama içerir. system container'ın birden fazla uygulama içermesinin sebebi; işletim sistemi çekirdğei haricinde tüm dependency'lerini (sadece kütüphane değil, process dependency'leri de) kendi container'ı içinde bulundurur.

# nix vs docker

- nix namespace'leri kullanmaz. programlar normal masaüstü uygulaması gibi açılır. nix ile açılan uygulamada namespace'leri ayırmak istersek önüne firejail gibi uygulamalar kullanmamız gereklidir.

- nix desktop uygulamalarda da kullanılabilması daha kolaydır.

- nix tamamen portable bir dizindedir. isteğe bağlı runtime servisi (daemon) açılabilir.

- nix update/değişiklik sonrası rollback işlemleri kolaydır. çünkü grupça tüm paketleri rollback yapabilir. git'teki gibi geri ileri alınabilir. docker'da bunu yapabilmek için docker'ın önünde kubernetes gibi yazılımlar kullanmak gerekir.

- dockerfile'de "apt-get update" işlemi yapıldığında paket indirme işleminin hangi paketleri indirdiği net değildir. oysa nix'te çok fazla paket detayına tek tek inilebilir. hem versiyon hemde sha hem de her paket için ayrı ayrı repo verilebilir.

- nixos sayesinde kernel modülü gerketiren uygulamalarda çalıştırılabilir. örnek virtualbox nixOS'ta sorunsuz çalışır.

# init vs systemd vs Upstart

işletim sistemi servis ve program yönetimi yazılımıdır. sistem başlangıcında ilk başlayan işlemdir. bu sebeple __init process__ olarakta adlandırılırlar. tüm servisler ve process'lerin yönetimini sağlar. process id'leri 1'dir.

"init" daha çok sıralı bir açılışa ağırlık verir. örneğin; update hizmeti, log servisine bağlı ise önce log servisinin başlatılması beklenir. hazır olunca update servisi başlar. oysa systemd'de işlemler paralel başlatılır. birbirlerine depend eden istekler beklemeye alınır. işlemler paralel başladığından CPU daha az boşta kalır ve daha hızlı açılış gözlenir.

systemd vs Upstart için komut satırı uygulamaları mevcut. bunların kullanım örnekleri aşağıdadır:

| Operation                          | Upstart Command                   | Systemd command                   |
|------------------------------------|-----------------------------------|-----------------------------------|
| Start service                      | start $job                        | systemctl start $unit             |
| Stop service                       | stop $job                         | systemctl stop $unit              |
| Restart service                    | restart $job                      | systemctl restart $unit           |
| See status of services             | initctl list                      | systemctl status                  |
| Check configuration is valid       | init-checkconf /tmp/foo.conf      | systemd-analyze verify $unitFile  |
| Show job environment               | initctl list-env                  | systemctl show-environment        |
| Set job environment variable       | initctl set-env foo=bar           | systemctl set-environment foo=bar |
| Remove job environment variable    | initctl unset-env foo             | systemctl unset-environment foo   |
| View job log                       | cat /var/log/upstart/$job.log     | sudo journalctl -u $unit          |
| tail -f job log                    | tail -f /var/log/upstart/$job.log | sudo journalctl -u $unit -f       |
| Show relationship between services | initctl2dot                       | systemctl list-dependencies --all |

Systemd servisleri başlatırken hangi servisleri başlatacağına target'lar aracılığı ile karar verir. örneğin sunucumuzda grafik arayüzü hiç kurulmamış ise, daha sonra gui kurarsak ve varsayılan olarak gui servislerinin açılmasını istiyorsak, default target'ı değiştirmeliyiz:

> systemctl set-default graphical.target

otuput:

```
Removed symlink /etc/systemd/system/default.target.
Created symlink from /etc/systemd/system/default.target to /usr/lib/systemd/system/graphical.target.
```

sistemde varolan target'ları görebilmek için:

> systemctl list-units --type=target

container'lar içinde çalışması için optimize edilmiş farklı alternatif init process'ler vardır. örnekler: __supervisord, runit, monit, tini/dumb-init, s6, tini+bash4.x__. özellikle side-car pattern'i ile birden fazla process aynı container'da çalıştırıldığından bu konu önemli olabiliyor. çoğu container yöneticisi tek process'li container'larda init process' oluşturma işini hallediyor. daha doğrusu direk ilgili process'imizi container'da açıyor. kaynak: https://ahmet.im/blog/minimal-init-process-for-containers/ (archive date: 19/10/2019) 

# Docker

linux'larda run edilen uygulamaların birbirinden habersiz çalışması, "Linux Containers (yada LXC yada runC)" isimli yapı ile zaten sağlanmaktadır. yürüyen uygulamalar birbirinden sadece port yada sadece dosya sistemlerini ortak yada bağımsız kullanmaları sağlanabilir. Varolan bu teknolojinin konfigürasyonu zor ve ayrıntılıdır. Docker bu yapıyı değiştirerek (libcontainer isimli proje ile) ve kolaylaştırarak kullanıcıların rahatça izole ortamlarda (container'larda) uygulama yürütebilmesini sağlamaktadır.

# linux dışındaki işletim sistemlerinde docker nasıl çalışıyor

docker sanal makine kurmamakta yada çalıştırmamaktadır. ve "container" yapısı sadece linux'ta vardır. fakat windows ve mac container yapısı desteklememektedir. docker uygulaması windowsa yada mac'e kurulduğunda, çalıştırılacak uygulamalar linux tabanlı olduğu için, docker kendi içinde bir sanal linux makinesi kurmaktadır (docker-machine aracılığı ile kuruyor. "Tiny Core Linux" türevi olan boot2docker isimli işletim sistemini çalıştırıyor). Bu makine üzerinde uygulamaları yürütmektedir. bu işlemleri docker harici bir firma boot2docker isimli uygulama ile yapmaktadır. ve arkaplanda sanal makine kurduğunu da önyüzde göstermemektedir. işletim sistemi boot2docker iken, bunu virtualboxu otomatik kurup yönetem uygulama boot2docker-cli'dir. artık docker-machine komut satırı uygulaması ile docker firması bu işi üstlenmektedir. bu sebeple boot2docker projesi artık geliştirilmiyor.

docker varolan işletim sisteminin çekirdeğini kullandığı için (ortada ikinci işletim sistemi olmadığı için), hypervisor gibi işlemci teknolojilerine bağımlılığı yoktur. en azından linux için bu şekilde. fakat windows ve mac 2016 yılı sonrası sürümleri itibari ile container yapısını desteklemeye başlamıştır. fakat teknik altyapıda donanıma bağımlılık vardır. hyper-v tarzı teknolojilere dayanmaktadırlar.

# Docker Daemon (yada docker engine)

Dcoker'ın containerları yönetebilmesini sağlayan yazılımıdır. libcontainer tabanlıdır. bir nevi hypervisor görevi görür.

# docker command line interface

docker uygulamasının bir client'ıdır. docker deamon'a erişir. docker deamon'a CLI'den erişmek zorunlu değildir. API kullanılarak bağımsız client'lar yazılabilir. erişim TCP socketleri ve HTTP istekleri üzerinden yapılabilmektedir.

# docker server vs docker client

her makinede ikisi yada sadece bir tanesi yüklü olabilir. server docker engine'dir. dockerları o makinede koşabilir. client ise user isteklerini docker server'a gönderebilme yeteneğine sahiptir.

# Docker Registery

internette bulunana hazır container repo hizmetlerinin genel adıdır.

# Docker Hub

Docker firmasının sunduğu resmi 'Docker Registery'sinin ismidir.

# Docker-compose

Yaml konfigürasyon dosyasını parametre olarak alır ve bu konfigürasyonlara göre birden fazla docker container'ını ayağa kaldırır. Kolay bir konfigürasyon dosyası olması ile ön plana çıkar. komut satırında bizim uzunca yazacağımız şeyleri kısaltmış olur.

docker-compose configürasyon dosyasının (yml) birçok sürümü vardır. dosyanın içinde en başa version belirtilmelidir. "version:3", "version 3.0" ile aynı anlama gelir. Tüm sürüm listesi buradadır: https://docs.docker.com/compose/compose-file/compose-versioning

"links" özelliği docker compose'da varken, stack'te yoktur. fakat stack'te olan bazı özellikler, docker-compose'da yoktur. 

```yml
links: # aşağıdaki servisler artık hostname üzerinden erişilebilir olur. bu servislerin network'leri mutlaka "links"i koyduğumuz container'ın networkü ile aynı olmalı.
      - db # service_name
      - db:database # service_name:alias
      - redis # service_name
```

# stack

swarm modülünden bağımsızdır. stack docker-compose'un gelişmişidir. swarm özelliği gibi docker-compose'un yapamadığı bir çok özelliği destekler.

v3 yml örnek dosya:

```yml
version: "3"

services:  #service tanımı baska bir başlıkta anlatılıyor

  web:  #docker servisinin adı. daha sonra bu ismi kullanarak docker-compose komutları ile ekrsta işlemler yapabileceğiz

    build: # service imageden değilde, dockerfile'dan oluşturulacaksa bu kısım eklenir

          dockerfile: /my/dir/dockerfile

          args:    # dockerfile içinde args varsa kullanılabilir

              PI = 3

    image: myusername/myrepo:mytag  #bu servis hangi imageden oluşturulacak. eğer bu varsa "build" property'si olmamalı.

    environment: # dockerfile içindekileri override edebilmek için kullanılır

             PROFILE: "docker"

             ANDROID_PATH: "/root/sdk/android"

    hostname: "web_domain_name" #diğer container'lar bu domain name ile bu container'a erişebilecek

    depends_on:  #bu conitanerlar baslamadna bu baslamaz

             - configserver

             - discoveryserver 

    

    deploy: 

      replicas: 5  #bu servise bağlı 5 tane container oluşacak

      resources:

        limits:

          cpus: "0.1"  #her container bu makinedeki tüm cpu'ların max %10'unu harcayabilir.

          memory: 50M  #her container max 50 mb ram harcayabilir

      restart_policy:

        condition: on-failure

      placement:

        constraints: [node.role == manager] # manager tipindeki node'larda bu container çalışacaktır. yada farklı seçenklerde var. 

                                            # örnek: [node.labels.layers == xxx] --> xxx label'ı verilmiş node'larda koşucak. 

    ports:

      - "80:80" #iceriden disariya olan port yönlendirme

    networks:

      - webnet #varolan hangi networke bagli olacak. bu özellik yeni geldi.

networks:

  webnet #bir network ismi
```

# cgroups ve linuxnamespace ayarları

(ne oldukları başka başlıkta anlatılıyor)

her container için ayrı ayrı ayar verilebilir. linuxnamespace'ler docker tarafından otomatik ayarlanıyor. oysa yukarıdaki docker compose örneğinde görüldüğü gibi cgroups'lar user tarafından configüre edilebiliyor (örnek: cpu 0.1 gibi).

# docker swarm

swarm türkçe kelime anlamı: sürü

dockerın içinde gelen bir modüldür. docker'da cluster/scale işlemlerini yapar. instance'ları(container'ları) scale ederek arttıran ve önlerine otomatik load balancer koyabilme yeteneklerine sahiptir. 

# Universal Control Plane (yada UCP)

birçok makinadaki docker-daemonlar tek bir noktadaki bu sunucu tarafından Web-GUI ile yönetilebiliyor/monitör edilebiliyor.

# ucp-agent (yada ucp node)

2 çeşittir:

- manager: web-ui'ı sunar ve worker'lara komut gönderir.

- worker: docker conitaner'larını çalıştırır.

manager özelliğine sahip bir node'da isterse container çalıştırır fakat bu pek tavsiye edilmez. sadece hızlı test için, deneme amaçlı kurulumlar yapılması için manager'ların container çalıştırması serbest bırakılmıştır.

manager ve worker node'ları aynı sistemde birden fazla olabilir.

# ucp bundle (yada ucp client bundle)

ucp panelden download edilir. biz zip dosyasıdır. içinde gerekli anahtarlar ve komut satırı scriptleri bulunur. bu scirptler kullanılarak direk docker yürklü bir makinadan ucp-web-ui-panelindyemiş gibi komut satırından işlem yapılabilmesini sağlar. 

# service

service bir conitaner instance'ı değil. örneğin yukarıda 1 service'e bağlı 5 instance oluşturduk. istersek hemen 10 tane daha ekleyebiliriz. servise yaptığımız ayar o servise bağlı tüm container'lara otomatik yapılmaktadır.

servis tanımımızı güncellediğimiz zaman o service'den türemiş tüm containerlar kapanır ve yeni tanımlama ile tekrar açılırlar.

# service types

Global service: her yeni worker node kelendiğinde bu servislerden birer instance otomatik o workderda açılmaktadır. firewall/antivirüs, monitor-agents gibi uygulamalarımız burada olabilir.

Replicated service:  İlgili servisten kaç adet instance'ın çalışacağına sizin karar verdiğiniz servis tipidir.

# docker machine

ek bir komut satırı uygulaması. bu uygulama ile yeni sanal işletim sistemleri kurabiliyoruz. bu şekilde container desteklemeyen işletim sistemlerinde, kolayca linux sanal makine yaratarak onun üzerinde docker çalıştırmamızı sağlamaktadır. aynı şekilde sadece localde değil, uzak bilgisayarlardaki sanal makineler üzerinde de çalışabilmemizi sağlamaktadır.  

docker machive windows ve linux'ta virtualbox kullanırken, mac'te HyperKit kullanmaktadır.

docker machine driver'ları sayesinde vmware, virtualbox, Microsoft Azure, Amazon Web Services gibi birçok  clodu makina sistemlerine veya sanal makine yöneticilerinde uzaktan işlem yapabiliyoruz.

# docker toolbox

docker engine + docker compose + docker machine + Kitematic uygulamasını bir arada sunan bir setup'tır.

windows ve mac native destek sağladıklarından beri artık bu pakete destek kaldırıldı. windows ve mac'e kurulumlarda "docker for windows (yada Docker Desktop for Windows)" veya "docker for mac (yada Docker Desktop for Mac)" kurulması öneriliyor.

docker for windows ve mac kubectl ve minikube yazılımını da içinde bulundurmaktadır.

# Kitematic

cross platfrom docker client GUI'sidir.

# docker tooling

Eclipse için docker client eklentisidir. dockerların içine komut yazabilmemizi de sağlıyor.

# docker container vs docker image vs docker file

docker file, docker image'i yaratmak için docker'ın anlayacağı tipte bilgiler/komutlar içeren dosyadır. bu dosya işlenerek (build işlemi) image image yaratılır.

docker image, docker file ile yaratılmış run edilmeye hazır sanal pakettir.

docker image run edildiğinde, docker container olur.

# docker file

örnek docker file:

```docker
FROM ubuntu # Docker container ilk açıldığında, boş bir dosya sistemi oluşturur. Burada bir işletim sistemi kullanmak gerekli. Bu şekilde temel kütüphaneler dosya sisteminde bulunmuş olur.

ARGS PI = 3 # container içinden bu değer okunamaz. komut satırından override edilebilir: "docker build --build-arg PI=4"

ENV ANDROID_HOME /my/dir # yada ENV ANDROID_HOME=/my/dir. Args ile aynı çağrılır. komut satırından override edilebilir: 'docker run -e "ANDROID_HOME=/my/another/dir"'

COPY /source/dir:/dest/dir #container dışındaki bir dizini yada dosyayı container içine kopyalar

ADD /source/dir:/dest/dir # copy ile aynıdır. ekstra olarak ADD; eğer source url ise onu indirir ve eğer source arşiv dosyası ise (tar, zip) onu açıp kopyalar.

RUN echo "hello number $some_variable_name" # yada "hello number ${some_variable_name}".

RUN apt-get install nodejs # sadece "docker build" komutu ile çağrılır. 

CMD ["/usr/bin/node", "/var/www/app.js"] # sadece "docker run" komutu ile çağrılır.

ENTRYPOINT ["/usr/bin/node", "/var/www/app.js1", "/var/www/app.js2"]   # CMD ile aynı görevi görür. tek farkı docker'ı run ederken, ek parametreler alabilir. "docker run myImage /var/www/app.js3" komutu ENTRYPOINT'e 3üncü parametreyi ekleyecetir.
```

# app installation with user password

example: 

> apt-get install wget

This command will ask "yes or no"? The only way to bypass this action is possible by apt-get feature:

> apt-get get "-y" 

parameter and check the user has the privilages to install the wget app. 

# userName/containerName vs containerName vs myhub.mysite.com:8081/myContainerName

Docker file'da her iki formatta da paket indirilebilir. userName olmadan indirilenler docker tarafından resmi olarak tag'lenmiş anlamına gelmektedirler.

"docker registry" açık kaynaklı yazılımları bulunmaktadır. self-hosted şekilde kendimize yaratabiliriz. böyle durumlar için, ip'si verilmiş makineden de image çekebilir.

# host os (yada Container OS)

docker deamon'u çalıştıran işletim sistemidir. bu herhangi bir kullandığımız son kullanıcı os'u olabilir. fakat bazı OS'lar sadece docker yürütmek için tasarlanmıştır. içinde gereksiz paketler yoktur.

örnek:
- Snappy Ubuntu Core
- CoreOS
- RancherOS
- Project Atomic
- Photon

# Snappy Ubuntu Core (yada Snappy Ubuntu yada Ubuntu Core)

Sadece snap uygulamaları barındıran minimal bir ubuntu türevidir. docker'ın snap paketi olduğundan sadece docker kurulu şekilde kullanılması tercih edilebilir. yada iot cihazlarında tercih edilmektedir.

# Base OS (yada base image os)

container içinde koşan işletim sistemidir. FROM komutu ile işletim sistemi baz alınırken, işletim sisteminin minimum olması beklenmektedir. bu durum; hem bellekten kazandırır hemde bağımlılıkların daha iyi belirlenmesini sağlar. buradaki işletim sistemleri tamamen farklı bir dizinde (chroot tarzı) saklanmaktadır. container'da run edilen uygulamalar sadece burayı görebilmektedirler. container içindeki uygulama, host işletim sisteminden sadece linux çekirdeğini ortak kullanmaktadır. onun dışında tüm kaynaklar mount edilen yeni dizin altındadır.

bazen "base os" terimi, "minimal os" terimi yerine yanlış kullanılmaktadır. ikisi farklı şeyler. minimal os, normal destop sistemler içinde olabilir. minimal os, sadece ek paketlerin olmadığı anlamına gelmektedir.

bazen de "container os" terimi "base os" ile karıştırılmaktadır.

linuxta direk sadece işletim sistemi çekirdeği kullanan bir uygulama çalıştırılırsa baseOS'a ihtiyacımız olmaz.

örnek: 
- alpine(özelliği ufak olması)
- ubuntu
- busybox(özelliği ufak olması)
- centos
- fedora
- nanoserver(windows native container'lar için)

# container user

container üzerinde çalışan uygulamalar root kullanıcı ile çalıştırılır. docker, runtime sırasında, root kullanıcısında komut yürütmemize izin verir.

# stop vs remove komutları

iki komutta container'ı durdurmaktadır. fakat remove komutu varolan state'i silmektedir. oysa stop komutu state'i korumaktadır. bu şekilde state istenirse farklı amaçlar için saklanabilir.

# attach vs exec komutu

docker içindeki root user'ına komut satırı üzerinden yönetimini sağlar. attach oldugumuzda yeni bir komut satırı oturumu açılmaz, onun yerine son çalıştırılan komututun çıktılarını görürürüz. eğer yeni bir komut satırı istiyor isek; "docker exec -i -t 878978547 bash command arg1 arg2" ile yeni bir bash açabiliriz. exec komutu en sağda yazan komutu container üzerinde çalıştırmamızı sağlar.

# docker build command

dockerfile'ın image olarak hazır hale getirilmesini sağlar. run edip anında ayağa kalkması için build olması şart. zira build sırasında docker için gerekli dosyalalar internetten indiriliyor.

# docker run command

build edilmiş bir image'yi run etmemize yarar.

-d parametresi eklenince container'ın id'sini döndürüp arkaplanda çalıştırır. -d verilmezse komut satırında işletim sisteminin system.out loglarını basar.

arkaplanda çalışan container'ın system.out çıktılarını "docker log" komutu ile de görebiliriz. 

# docker comit komutu

bu komut ile çalışmakta olan bir image, kopyalanıyor ve yeni bir image yaratılıyor. bu image'nin id'si ekrana basılıyor. image içindeki bilgiler tutulmuş oluyor. örneğin; mysql image'si run edildi, daha sonra üzerine manuel olarak wget kuruldu. bu image kopyalanıp saklanılması isteniyor ise; comit komutundan yararlanılmalıdır. aslında yeni bir isimde klon image oluşturulmuş oluyor. fakat comit yerine dockerfile yaratılması öneriliyor; çünkü image'in nasıl yaratıldığını daha net bilebiliriz.

# layer

container içinde yapılan her işlem yeni bir katman yaratıyor. örneğin layer değişmeden comit yapabilmemiz mümkün diil. çünkü değişen bir dosya olmayacaktır.

# docker diff command

"docker diff containerId" komutu verildiğinde o containerId'nin yaratıldığı image-layer'ı ile şu andaki layer arasındaki dosya sistemindeki farkları listeleyecektir.

# kill vs stop command

container'ların içindeki tüm uygulamalara terminate yada kill siyali gönderiyor. sinyaller başka başlıkta anlatılıyor.

# -p parameter

container'ı run ederken port numarası mappingi yapmak için kullanılır. örneğin;

> docker run -p 80:8080 imageId

BU şekilde host makinenin 80 portuna gelen istekler, docker container içindeki 8080 portuna yönlendirilecektir.

# -v parameter

-p paramtresi ile aynı formata kullanılıyor. fakat dizin bağlamak (mount etmek) için yapılıyor. örnek:

> docker run -v /home/host-user/dir:/home/docker-user/dir imageId

# docker inspect

inspect yanına yazıalna id, herhangi bir kaynağın id'si olabilir. örnek: volume id, image id, container id, network id, stack id gibi.. tüm detaylı json formatında bize döndürür.

# docker --link

docker container'ların birer ip si vardır. bu ip'ler biribirine erişebilmelidirler. bu yüzden host dosyasına dns kaydı otomatik atılabilir. örnek:

> docker run --link myOtherContainerId:aliasOnDnsHostFile imageId

myOtherContainerId ip'si 10.0.0.1 ise; yeni yaratılan container içindeki host dosyasına bakıldığında bu satır görülecektir:

> 10.0.0.1 aliasOnDnsHostFile

# docker hub'a image yollama

docker login komutu ile komut satırından docker'a login olunabilir. daha sonra herhangi bir image'ı push komutu ile docker hub'daki hesabımıza yollayabilir.

# repository vs registry

repository bir docker-image'sinin farklı sürümlerinin barındırıldığı sunucu yazılımıdır. tag ise docker-image'nin sürümüdür. repository kelimesi birçok farklı docker-imagesini barındırıyormuş gibi görünüyor, oysa aynı docker-image'inin farklı sürümlerini barındırıyor. örneğin: repo:jdk tag:8.

registry ise birçok farklı docker-image'sinin bulunduğu sunucu yazılımıdır. örnek "docker hub", docker Inc firmasının resmi registry'sidir ve default olarak docker client'ta tanımlı gelir.

# docker ce

community edition (free)

# docker ee

enterprise edition

# docker edge vs stable

edge is beta version.

# Docker Trusted Registry (yada DTR)

locale kurulabilen registry.

# docker collection

docker'ın ucp'sinin vermiş olduğu bir özellik. birçok node UCP'ye bağlı olabilir. her node'da hangi servisi koşacağı stactk-yml dosyalarında bellidir. fakat swarm-yml dosyaları her bilgiyi barındıramıyor. bu sebeple bazı bilgiler sadece ucp içinde kayıtlıdır. örnek:

- hangi volume'nin hangi node'da olacağı

- node'lara atanmış label bilgileri

- stack dosyaları haricinde yaratılmış service'ler

bunların yedeğini alabilmek için gurplama ihtiyacı vardır. işte bu gruplara "collection" denir. collection sadece bu bilgileri değil, tü sistemin bilgileri saklar. örneğin; stack-yml dosyaları da buna dahildir.

her collection, ucp tarafından ayrı ayrı dizinler içerisinde saklanır.

# moby

artık (2017 yılı ile) docker açık kaynaklı proje olarak moby projesi altında dağıtılıyor. moby projesi dockerCE ve EE olarak docker tarafından kapatılıp üzerinde bazı değişiklikler yapıp dağıtılmaktadır. moby projesi ile hedef docker'ı daha modüler hale getirmektir.

# union file system (yada unionfs)

docker bu dosya sistemini run edilen container'lar için kullanır. örnek: java kurduk ve uygulamamızı run ettik. java ve ubuntu-os otomatik kurulur. bu 2 container read-only'dir. run ettiğimiz java uygulaması birşeyler kaydetti. bu dosyalar ubuntu-os'un dosyalarını da değiştirmiş olabilir yada ek dosya atayabilir. böyle durumlar için dosya sistemi böyle işliyor: ubuntu-os ve java'nın olduğu bir dosya sistemi read-only (no:1) oluşturuluyor. daha sonra  bir dosya sistemine (no:2) java uygulamamız dosya yazıyor. eğer container içinden bir dosya okumaya kalkarsak; unionfs önce no:2 dosya sistemine bakıyor. eğer bir dosya silme bilgisi yada yeni dosya oluşturulmuş ise bu dosyayı bize dönüyor. eğer hiçbir bilgi bulamaz ise no:1 dosya sistemindeki dosyayı dönüyor. bu sistemde no:2 dosya sistemine no:!'in "branch"'i adı veriliyor. istediğimiz kadar branch oluşturabiliyoruz.

var olan ve container açıldıktan sonra düzenlenen bir dosya branch'e kopyalanır ve editlenir. tabi eğer taşınan bu dosya çok büyükse ilk kopyalama işlemi zaman alacaktır.

unionfs'e birçok alternatif var. bu alternatifleri docker engine'den seçebiliyoruz. bazı dosya sistemleri tüm dosyayı değilde, dosyayı belli bloklara bölüyor ve sadece değişen kısmın bloğunu taşıyor. bu tarz farklı yöntemler mevcut.

unionfs'in değişiklikleri branch'lere kopyalama özelliğine copy-on-write (yada cow yada implicit sharing yada shadowing) deniliyor.

# volume

bir container'a host bilgisyarımızdaki bir dizini mount edebiliriz. buna alternatif olarak sanal bölümler docker tarafından oluşturulabilir. bunlar host makina tarafından okunamazlar. mount ve volume karşılaştırılacak olursa asıl önemli avantaj: volume'ların docker tarafından moutn edilmesi olacaktır. host bilgisyarda motun edeceğimiz dizinin yetkileri, dosya sisteminin desteklenip desteklenmeyeceğini gibi sorunlarla karşılaşabiliriz. oysa volume'larda böyle bir sorun yok.

# docker temizlik

##### dangling images

türkçe kelime anlamı: askıda kalmak

örnek üzerinden direk gidersek. docker içinde jdk indirdik. os olarak fedorayı indirdi. fedora'yı indirince fedora ile "latest" isminde bir layer (image) oluştu. daha sonra jdk layerı oluştu. 1 ay sonra tkrar jdk'yı indirdik. bu sefer içindeki fedora güncellendi. ve yine "latest" tag'i ile. dolayısı ile eski "latest" tagli fedora artık "none" isminde kaydediliyor. bu tarz durumlar bazı layerları askıda bırakıyor. sadece askıda olan layerları temizleyen komut mevcut.

tag vermeden oluşturduğumuz her image dangling'dir.

##### docker system prune

prune türkçe kelime anlamı: budamak.

bu komut sırası ile şu komutları işletiyor:

- docker container prune

- docker image prune

- docker network prune

- docker volume prune

##### docker system prune -a

-a parametresi durmuş tüm kaynakların silinmesini sağlıyor. yani o sırada run edilenler haricindeki tüm kaynaklar siliniyor. hiçbir container run edilmezken çalıştırılırsa tüm kaynaklar silinir.

##### docker container prune

stop olmuş tüm container'ları siler.

##### docker image prune

dangling images'leri siler.

##### docker image prune -a

hiçbir container tarafından kullanılmayan imageleri siler.

##### dangling volume

bir volume conitaner run edildiğinde mount edilir. yani container ile ilişkilendirilir. eğer ilişkisiz bir volume var ise, bu volume dangling'dir. stop olmuş fakat silinmemiş container'lar hala volume'ler ile ilişkilidir.

##### docker volume prune

dangling volume'leri siler. bu komutun "-a" ile bir alternatifi yoktur. çünkü zaten container ile bağlantısı olmayan her volume dangling'dir.

##### docker network prune

volume ile aynı mantıktadır. bu komutun "-a" ile bir alternatifi yoktur. çünkü zaten container ile bağlantısı olmayan her volume dangling'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sanal makinelerde Disk formatlari

```
          native-support      support               açıklama

- VMDK    VMWare              Virtualbox, QEMU/KVM   

- VDI     virtualbox          QEMU/KVM              Genel olarak VDI ve WDMK arasinda önemli farklar mevcut degil.

- raw     QEMU/KVM                                  hiçbir özellik barındırmıyor. sanal diskte ne varsa direk burada binary olarak tutuluyor. format header dahi barındırmıyor. en iyi performans bunda.

- cow     QEMU/KVM                                  QEMU/KVM'nin sunduğu format.

- qcow    QEMU/KVM            VirtualBox            cow'un yeni sürümü.

- qcow2   QEMU/KVM                                  qcow'un yeni sürümü.

- vhdx    Hyper-V             VirtualBox            vhd'nin yeni sürümü. vhd microsoft haricindeki yazılımlara sorun olduğu söyleniyor.

- vhd     Hyper-V             VirtualBox

- cloop   QEMU/KVM                                  özel amaçlı kullanılan bir format.

- qed     QEMU/KVM            VirtualBox            qcow2'nin özellikleri silinerek performans kazandırılmaya çalışılıyor. fakat proje durduruldu.
```

# Virtualbox'ta Disk dosyasını klonlama

Ayni HDD'den ayni virtualbox üzerine eklenemiyor. HDD'nin UUID'sini degistirmek gereklidir. Bunun için asagidakimkomut kullanilmalidir. Asagidaki komut verilen hdd'ye rastgele UUID atamaktadir.

> "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" internalcommands sethduuid "C:\Users\Desktop\hdd_to_change_uuid.vmdk"

HDD format degisikligi ve yeni UUID ile hdd klonlama islemi VirtualBox'un grafik arayüzünden --> dosya --> sanal ortam yoneticisi araciligi ile yapilabilmektedir. fakat GUI'deki bu islem çok uzun sürmektedir.

# kvm performans

- qcow2 yerine raw formatı tercih edilebilir. fakat raw formatı ile kullanılan diskte snapshot yok.

- KVM'de çalışırken qcow2 bir image kullanmak performans açısından aşırı derecede farkediyor. bu sebeple aşağıdaki convert işlemi yapılabilir.

  > qemu-img convert -f vmdk -O qcow2 /dir/disk1.vdmk /dir/disk1.qcow2

- "show virtual hardware details" --> disk --> advanced options --> disk bus: SATA 

- "show virtual hardware details" --> disk --> advanced options --> Performans options --> Cache mode: none

- "show virtual hardware details" --> disk --> advanced options --> Performans options --> IO mode: native

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# gitweb

belirtilen dizinlerdeki git projelerinin web tarayıcısından açılmasını sağlayan bir web arayüz projesidir. bu arayüzden commitler, loglar, history'ler kolayca incelenebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CDN ( Content Delivery Network - İçerik Dağıtım Ağı )

CDN dünyanın bir çok yerine dağıtılmış sunucuların oluşturduğu bir alt yapıdır ve CDN'nin amacı ziyaretçilere, site içeriği en hızlı şekilde ulaştırmaktır. Bant genişlikleri, bölge bölge hız değerleri göz önüne alınarak dağıtım yapılır ve sunucular her tarafa uygun hızda iletim sağlamaya çalışır.

CDN hizmeti sunan birçok firma var. TÜm global düyayı takip edip sunucuları uygun yerlere koyuyorlar.

CDN hizmetine atılan dosyaları çekmek daha hızlı olacağından;

```xml
<script src="js/jquery-2.0.2.min.js"></script> 
```

yerine;

```xml
<script src="http://code.jquery.com/jquery-2.0.2.min.js"></script>
```

yazılması daha iyi olacaktır. Ziyaretçi önceden farklı siteden CDN li linki indişmiş ise tarayıcı cache'e dahi alacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# dos (disk operating system)

bir işletim sistemidir. birçok firma tarafından farklı türevleri çıkarılmıştır. freedos (open source), ms-dos gibi.

# ms-dos (microsoft dos)

ms-dos 1980 yıllarında çıkmıştır. microsoft, windows 2000 sürümü ile NT ailesine geçiş yapmıştır. NT, dos çekirdeğini ortadan tamamiyle kaldırmıştır. NT öncesi, komut satırı DOS'un arayüzüydü (her işletim sisteminde olduğu gibi). Dos komutları ile aynı görevi yapan ve benzer isimlere sahip olan komutlar, NT sonrası da vardır. bu sebeple birçok kişi NT sonrası işletim sistemlerinde hala Dos'un komut satırı olduğunu sanmaktadır.

# windows NT ailesi

Windows 3'üncü sürümü ile hem dos hem de NT tabanlı sistemleri pararlel geliştiriyor ve paketliyordu. NT tabanlılar sunucu ve kurumsal makinalara kuruluyor, dos tabanlıları ise ev kullanıcılarına veriyorlardı.

# Windows Me (yada Windows Millennium Edition)

Windows 98 İle Windows 2000 arasındaki sürümdür.

# Pocket PC

Microsoft'un geliştirdiği, cebe sığacak boyutta donanım cihazı. Üzerinde windows kurulu geliyor.

# windows insider

microsoft lisanslı windows ürünleri için isteyen kullanıcıları windowsun önceki sürümlerinin updatelerini alabilmelerini sağlamaya başladı. birçok microsoft ürünü için insider opsiyonu mevcut. insider seçeneği aktif olan kişilere aynı anda güncelleme gelmiyor. bazı kullanıcılara bazı updateler çok önceden getiriliyor. düzenli şekilde gruplama mevcut.

# PowerShell

Windows'un güncel sürümlerinde yüklü gelen yeni gelişmiş komut satırı programı.

# Microsoft Surface (yada Surface RT)
Microsoftun donanımını da geliştirdiği, windows işletim sistemi windows yüklü gelen tablet cihazlar serisinin ismidir.

İçinde windows rt vardır. windows rt, windows 8.x'in arm için türevidir. windows rt sadece windows store uygulamalarını açabiliyor. daha sonra mcrosoft geliştirmeyi kesti.

# MSDN
Microsoft'un sadece geliştiriciler için sunduğu web portal.

# TechNet
Microsoft'un tüm kullanıcılar için sunduğu web portal. örneğin visual studio ide ile ilgili indirmeler yada dökümanlar burada bulunmaz.

# RTM (yada released to manufacturers)
microsoft windows ürününü piyasaya sunmadan son halini bazı gruplara açar. bu gruplar özellikle akademik ve donanım üreticileridir. bu şekilde piyasaya windows dağıtılmadan hazırlıklar yapılır. bu dağıtılan sürüm RTM'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Gerçek Zamanlı İşletim Sistemleri (yada Real time operating system yada RTOS)

Üzerinde çalışan uygulamalar (servisler ve yazılımlar vs..)  gerçek zamanlıdır. yani belirli bir süre içerisinde belirli bir işe cevap vermek zorundalardır. bu durumun oluşabilmesi için; işletim sistemi gerektiğinde, kendisinden daha öncelikli işlemlere öncelik verebilmesi gerekmektedir. aynı zamanda procesler arası çalışmalarda öncelik yönetimininde, normal işletim sistemlerine göre daha çok seçenek sunmaktadır. bu tarz yönetimlere destek veren işletim sistemlerine RTOS denir. CPU önceliklendirilmesi özel olduğundan, bu işletim sistemlerinin sundukları API'ler, arkaplandaki servislerde buna göre yazılmıştır.

Genellikle RTOS'larda donanıma ve sürücülere bağımlılık yüksektir.

Örnekler: 
- Quantum Software Systems OS (yada QNX)
- Microsoft Windows CE
- RTLinux (yada Real-time Linux)

# monolithic kernel

linux'un kullandığı çekirdek tipidir. tüm sistem tek bir yapıda bulunur. bir hata olduğunda, yada içinden bir şeyin tekrar başlaması gerektiğinde komple sistem restartı gerekir. fakat daha stabildir.

# microkernel

unix bu tiptedir. sistem daha modüler yapıdadır. kernel modda çok az şey çalışır. bu sebeple bir parça (örnek driver) hata verdiğind eve restart istediğinde tüm sistem resatart edilmek zorunda diildir. daha az stabildir.

# hybrit kernel

windows'un NT ve sonrası kullandığı çekirdek yapı tipidir. NT öncesi monolitic kullanıyordu. bu tarz sistemlerde; bazı kısımlar çok bütünleşikken, bazıları hiç diildir (modülerdir).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# IaaS (yada Infrastructure As a Service yada Altyapı olarak servis)

uzaktaki sunucularda sizin adınıza sanal bir sistem (donanım) oluşturuluyor ve içine işletim sistemi kurulu şekilde hazır oluyor. (Örnek firma hizmeti: Amazon WS)

# PaaS (yada Platform As a Service)

Bu sınıf yapısında size bir servis havuzu sunuluyor ve siz bu servislerden ihtiyacınıza göre ekleme çıkarma yapabiliyorsunuz. Tabiki her bir servis platformu belirli limitlerden sonra ayrı ayrı (+) maliyet demek. Örneğin; çeşitli servisler mevcut (Maven, MySQL veritabanları gibi). (Örnek firma hizmeti: Windows Azure)

# SaaS (yada Software As a Service)

sunulan özel yazılımlar ile, lokalde hiçbir işgücü ve bakım maliyeti olmadan yazılımlardan istifade edebiliyorsunuz. örneğin; attlasian jira, gmail gibi.

# Mobile backend as a service (yada MBaaS yada backend as a service yada BaaS)

mobil uygulamalar için bulut sistemi hizmetleridir. örneğin notification yönetimi, app için analitik bilgi toplama işlemleri, crachs hakkında bilgi toplama servisleri, kolayca erişilebilir veritabanları. Örnek hizmet: Google Firebase.
