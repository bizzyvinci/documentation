# GraphQ Şema

## Varlıkları Tanımlama

`schema.graphql` dosyası çeşitli GraphQL şemalarını tanımlar. GraphQL sorgu dilinin çalışma biçimi nedeniyle, şema dosyası temel olarak verilerinizin şeklini SubQuery'den belirler. GraphQL şema dilinde yazma hakkında daha fazla bilgi edinmek için [Schemas and Types](https://graphql.org/learn/schema/#type-language).'a göz atmanızı öneririz.

**Önemli: Şema dosyasında herhangi bir değişiklik yaptığınızda, lütfen aşağıdaki komutla türler dizininizi yeniden <>yarn codegen</code>**

### Varlık
Her varlık gerekli alanlarını < `ID!` türünde `id` olarak tanımlamalıdır. Birincil anahtar olarak kullanılır ve aynı türdeki tüm varlıklar arasında benzersizdir.

Varlıktaki null olmayan alanlar `!` ile gösterilir. Lütfen aşağıdaki örneğe bakın:

```graphql
type Example @entity {
  id: ID! # kimlik alanı her zaman gereklidir ve böyle görünmelidir
  name: String! # Bu gerekli bir alan
  adres: Dize # Bu isteğe bağlı bir alandır
```

### Desteklenen skalerler ve türler

Şu anda akan skalers türlerini destekliyoruz:
- `ID`
- `Int`
- `Dizgi`
- `BigInt`
- `Float`
- `Tarih`
- `Boolean`
- `<EntityName>` iç içe geçmiş ilişki varlıkları için, tanımlanan varlığın adını alanlardan biri olarak kullanabilirsiniz. Lütfen [Entity Relations](#entity-relationships) bakın.
- `JSON` yapılandırılmış verileri alternatif olarak depolayabilir, lütfen bkz[JSON türü](#json-type)
- `<EnumName>` types are a special kind of enumerated scalar that is restricted to a particular set of allowed values. Please see [Graphql Enum](https://graphql.org/learn/schema/#enumeration-types)

## Birincil anahtar olmayan alana göre dizin oluşturma

Sorgu performansını artırmak için, yalnızca birincil anahtar olmayan bir alana `@index` ek açıklama uygulayarak bir varlık alanını dizine dizine ekleme.

Ancak, kullanıcıların herhangi bir <@index<JSON>@index<>/1> nesnesine >0>0<> ek açıklama eklemesine izin vermeyiz. Varsayılan olarak, dizinler otomatik olarak yabancı anahtarlara ve veritabanındaki JSON alanlarına eklenir, ancak yalnızca sorgu hizmeti performansını artırmak için.</p> 

İşte bir örnek.



```graphql
kullanıcı @entity { yazın
  id:  ID!
  name: String! @index(unique: true) # benzersiz doğru veya yanlış olarak ayarlanabilir
  title: Title! # Dizinler otomatik olarak yabancı anahtar alanına eklenir
 }

type Title @entity {
  id: ID!  
  name: String! @index(unique:true)
}
```


Bu kullanıcının adını bildiğimizi varsayarsak, ancak tüm kullanıcıları ayıklamak ve sonra ada göre filtrelemek yerine tam kimlik değerini bilmiyoruz, ad alanının arkasına `@index` ekleyebiliriz. Bu, sorgulamayı çok daha hızlı hale getirir ve ayrıca benzersizliği sağlamak için `unique: true` geçirebiliriz. 

**Bir alan benzersiz değilse, en büyük sonuç kümesi boyutu 100'dür**

When code generation is run, this will automatically create a `getByName` under the `User` model, and the foreign key field `title` will create a `getByTitleId` method, which both can directly be accessed in the mapping function.



```sql
/* Başlık varlığı için kayıt hazırlama */
UNVANLARA EKLE (kimlik, ad) DEĞERLER ('id_1', 'Kaptan')
```




```typescript
Eşleme işlevinde işleyici
{User} içinden ".. /types/models/User"
{Title} içinden ".. /types/models/Title"

const jack = User.getByName('Jack Sparrow');

const captainTitle = Title.getByName('Kaptan');

const pirateLords = User.getByTitleId(captainTitle.id) bekliyor; Tüm Kaptanların listesi
```




## Varlık İlişkileri

Bir varlığın genellikle diğer varlıklarla iç içe geçmiş ilişkileri vardır. Alan değerini başka bir varlık adına ayarlamak, varsayılan olarak bu iki varlık arasında bire bir ilişki tanımlar.

Farklı varlık ilişkileri (bire bir, bire çok ve çok-çok) aşağıdaki örnekler kullanılarak yapılandırılabilir.



### BireBir İlişkiler

Yalnızca tek bir varlık başka bir varlıkla eşleştirildiğinde bire bir ilişkiler varsayılandır.

Örnek: Pasaport yalnızca bir kişiye aittir ve bir kişinin yalnızca bir pasaportu vardır (bu örnekte):



```graphql
kişi @entity { yazın
  id: Kimlik!
}

Passport @entity { yazın
  id: Kimlik!
  sahibi: Kişi!
}
```


veya 



```graphql
kişi @entity { yazın
  id: Kimlik!
  pasaport: Pasaport!
}

Passport @entity { yazın
  id: Kimlik!
}
```




### Bire Çok ilişkileri

Alan türünün birden çok varlık içerdiğini belirtmek için köşeli ayraçları kullanabilirsiniz.

Örnek: Bir kişinin birden fazla hesabı olabilir.



```graphql
kişi @entity { yazın
  id: Kimlik!
  hesaplar: [Hesap] 
}

Hesap @entity { yazın
  id: Kimlik!
  publicAddress: Dize!
}
```




### Çok-Çok ilişkileri

Diğer iki varlığı bağlamak için bir eşleme varlığı uygulanarak çok-çok ilişkisi elde edilebilir.

Örnek: Her kişi birden çok grubun (PersonGroup) bir parçasıdır ve grupların birden çok farklı kişisi (PersonGroup) vardır.



```graphql
kişi @entity { yazın
  id: Kimlik!
  name: String!
  gruplar: [PersonGroup]
}

Type PersonGroup @entity {
  id: Kimlik!
  kişi: Kişi!
  Grup: Grup!
}

Grup @entity { yazın
  id: Kimlik!
  name: String!
  persons: [PersonGroup]
}
```


Ayrıca, orta varlığın birden çok alanında aynı varlığın bağlantısını oluşturmak mümkündür.

Örneğin, bir hesabın birden çok aktarımı olabilir ve her aktarımda bir kaynak ve hedef hesabı vardır.

Bu, Transfer tablosu aracılığıyla iki Hesap (itibaren ve bu) arasında çift yönlü bir ilişki kuracaktır. 



```graphql
type Account @entity {
  id: ID!
  publicAddress: Dize!
}

type Transfer @entity {
  id: ID!
  miktar: BigInt
  itibaren: Hesap!
  to: Account!
}
```




### Geriye Doğru Aramalar

Bir objede bir ilişki için geriye doğru aramayı etkinleştirmek için, alana `@derivedFrom` ekleyin ve başka bir varlığın geriye doğru arama alanının üzerine gelin.

Bu, varlık üzerinde sorgulanabilecek bir sanal alan oluşturur.

Bir Hesabı "Kimden" Aktar'a, sentTransfer veya receivedTransfer değerini ilgili alanlardan veya alanlardan türetilmiş olarak ayarlayarak Hesap varlığından erişilebilir.



```graphql
type Account @entity {
  id: ID!
  publicAddress: Dize!
  sentTransfers: [Transfer] @derivedFrom(field: "from")
  receivedTransfers: [Transfer] @derivedFrom(field: "to")
}

type Transfer @entity {
  id: ID!
  miktar: BigInt
  itibaren: Hesap!
  to: Account!
}
```




## JSON türü

Yapılandırılmış verileri depolamanın hızlı bir yolu olan verileri JSON türü olarak kaydetmeyi destekliyoruz. Bu verileri sorgulamak için otomatik olarak karşılık gelen JSON arabirimleri oluşturacağız ve varlıkları tanımlamak ve yönetmek için size zaman kazandıracağız.

Kullanıcıların aşağıdaki senaryolarda JSON türünü kullanmalarını öneririz:

- Yapılandırılmış verileri tek bir alanda depolamak, birden çok ayrı varlık oluşturmaktan daha yönetilebilir olduğunda.
- Rasgele anahtar/değer kullanıcı tercihlerini kaydetme (burada değer boole, metinsel veya sayısal olabilir ve farklı veri türleri için ayrı sütunlara sahip olmak istemezsiniz)
- Şema geçicidir ve sık sık değişir



### JSON yönergesi tanımla

Varlığa `jsonField` ek açıklaması ekleyerek özelliği JSON türü olarak tanımlayın. Bu, projenizdeki tüm JSON nesneleri için otomatik olarak `types/interfaces.ts` altında arabirimler oluşturur ve bunlara eşleme işlevinizden erişebilirsiniz.

Varlığın aksine, jsonField yönerge nesnesi herhangi bir `id` alanı gerektirmez. Bir JSON nesnesi diğer JSON nesneleriyle de iç içe olabilir.



````graphql
type AddressDetail @jsonField {
  street: String!
  bölge: String!
}

type ContactCard @jsonField {
  phone: String!
  adres: AddressDetail # İç içe JSON
}

Kullanıcı @entity { yazın
  id: Kimlik! 
  kişi: [ContactCard] # JSON nesnelerinin listesini depolayın
````




### JSON alanlarını sorgulama

JSON türlerini kullanmanın dezavantajı, her metin araması yaptığında tüm varlık üzerinde olduğu gibi, filtreleme yaparken sorgu verimliliği üzerinde küçük bir etkidir.

Ancak, etki sorgu hizmetimizde hala kabul edilebilir. '0064' içeren bir telefon numarasına sahip ilk 5 kullanıcıyı bulmak için JSON alanındaki GraphQL sorgusunda `contains` işlecinin nasıl kullanılacağına ilişkin bir örnek aşağıda verilmiştir.



```graphql
#To ilk 5 kullanıcının kendi telefon numaralarının '0064' içerdiğini bulun.

query{
  user(
    first: 5,
    filter: {
      contactCard: {
        contains: [{ phone: "0064" }]
    }
}){
    nodes{
      id
      contactCard
    }
  }
}
```
