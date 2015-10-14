## Android Programming The Big Nerd Ranch Guide

[supported markup formats](#markups)

[Bölüm 1](#bolum_1)

[Bölüm 2](#bölüm_2)

[Bölüm 3](#bölüm_3)

[Bölüm 4](#bölüm_4)

[Bölüm 5](#bölüm_5)

[Bölüm 6](#bölüm_6)

[Bölüm 7](#bölüm_7)

#Markups

1.1 User Interface
------------------
* ADT 21'den itibaren layout dosyalarının başına ` <?xml version="1.0" encoding="utf-8"?>
  `
  yazmak gerekmiyor.

* `TextView, Button, RelativeLayout, ImageView` bunların herbiri **widget** olarak adlandırılıyor.

* Her widget ya **View** sınıfının doğrudan bir örneğidir ya da onun altsınıflarının(**TextView** ya da **Button** gibi) bir örneğidir.

* Her bir widget'a karşılık **XML elementi** yazılır. Elementin adı widget'ın türüdür. Her element **XML attributeları** kümesine sahiptir. Attribute'ler widgetları özelliklerini belirlemek için kullanılır.

1.2 The view hierarchy
------------------

* Layout'un kök elemanı(root element) aşağıdaki gibi Andoid resource xml isim uzayını belirtmelidir.
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    ...
</LinearLayout>
```
* **LinearLayout**, **View**'ın alt sınıfı olan **ViewGroup** sınıfından türemiştir. **ViewGroup** içinde widgetlar bulunan ve onları düzenleyen bir widget'tır.

* **ViewGroup**'un diğer alt sınıfları **FrameLayout, TableLayout** ve
**RelativeLayout**dır.


1.3 Widget attributes
------------------

**android:layout_width** ve **android:layout_height**
android:layout_width ve android:layout_height attributeları her tip widget için gereklidir. `match_parent` ya da `wrap_content` özelliklerini alabilir.

> `fill_parent` isimli attribute API Level 8'den itibaren önerilmemektedir ve `match_parent` olarak yeniden isimlendirilmiştir. 

**match_parent :** Görünümü kendi ebeveyni kadar büyük olsun

**wrap_content :** Görünümü içerdiği şey ne kadarsa büyüklüğü o kadar olsun

**padding :** TextView için TextView'ın içindeki text ile arasına boşluk koyar.

**android:orientation :** LinearLayout için `vertical` ya da `horizontal` değerlerini alabilir. `vertical` ilk çocuk en yukarı yerleşir. `horizontal` da ise ilk çocuk en sola yerleşir.
> Arapça gibi dillerde `android:orientation="horizontal"` durumunda  ilk çocuk en sağa yerleşir. 

**android:text :** Button, TextView gibi widgetlarda bulunur. Gösterilecek yazıyı tutar.

1.4 Layout XML'den View Nesnelerine
--
* **onCreate(Bundle)** metodu Activity altsınıfının örneği yaratıldığı zaman çağrılır. Activity yaratıldığında yöneteceği bir user interface gerekir. Activity'nin user interface'i alması için

 ```java
public void setContentView(int layoutResID)
```

  metodu çağrılır. Bu metod id olarak aldığı layout'ı **inflate**(hava basmak, şişirmek) eder ve ekrana yerleştirir. Bir layout **inflate** edildiğinde layout dosyasındaki her widget tanımlı attributeları ile oluşturulur.

1.5 Resource lar and resource ID leri
--
* Layout bir resource dur. Uygulamamızın kod olmayan parçalarına **resource(kaynak)** diyoruz.

* Uygulamamızdaki kaynaklar `res/` klaörü altında bulunur. Örneğin uygulamamız içindeki string'ler `res/values/` altında `strings.xml` dosyasında tutulur. Layout'lar `res/layout` klasörü altındadır.

* Kod içinden kaynaklara erişmek için *resource Id*'leri kullanırız. Uygulamamız içindeki kaynakların id'leri `R.java` isimli dosya da oluşturulur. `R.java` dosyasının genel görünümü şu şekildedir.

R.java
 ```java
 public final class R {
   public static final class attr {

   }
   public static final class drawable {
      public static final int ic_launcher=0x7f020000;
   }
   public static final class id {
      public static final int menu_settings=0x7f070003;
   }
   public static final class layout {
      public static final int activity_quiz=0x7f030000;
   }
   public static final class menu {
      public static final int activity_quiz=0x7f060000;
   }
   public static final class string {
      public static final int app_name=0x7f040000;
      public static final int false_button=0x7f040003;
      public static final int menu_settings=0x7f040006;
      public static final int question_text=0x7f040001;
      public static final int true_button=0x7f040002;
   }
   ...
 }
 ```

> Android oluşturduğunuz her layout ve string için otomatik olarak id oluştutur. Ancak layout dosyalarınız içindeki widget'lar için otomatik id oluşturulmaz. Eğer kod içinden erişmek istediğiniz bir widget mevcutsa buna id yi biz kendimiz vermeliyiz.

* Bir widget'a resource id, `android:id` attribute'una değer verilerek atanmış olur. Bunu yaptığımızda `R` sınıfının statik iç sınıfı olan `id` sınıfında bizim verdiğimiz isimle widget'a bir referans adresini belirtmek üzere hexadecimal formatta bir tamsayı değeri oluşturulur.

```xml
<Button
 android:id="@+id/my_button"
 ...
```

> \+ yazmamızın sebebi daha önce olmayan bir değişken yaratmak istememiz sebebiyledir. stringler için + işareti kullanmadan
@string/my_string şeklinde bir tanımlama yapıyorduk çünkü strings.xml de my_string adında bir string olduğunu biliyoruz ve sadece ona bir referansta bulunuyoruz.

[1.6 Google Android İsimlendirme Standartları](http://source.android.com/source/code-style.html#follow-field-naming-conventions)

* Non-public, non-static field names start with m.
* Static field names start with s.
* Other fields start with a lower case letter.
* Public static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

1.7 Widget'ları bağlamak(wiring up)
--
İnflate edilmiş bir widget nesnesine referans elde etmek için
```java
public View findViewById(int id)
```
activity metodu çağrılır. Ve sonrasında tıklandığında çalışacak metod için isimsiz sınıf oluşturulup onClick metodu çalıştırılır.

Android uygulamaları olay yönetimlidir(event-driven). Uygulama başlar ve bir olay bekler. Örneğin kullanıcının bir butona basması gibi. Event'a cevap olarak yaratılan nesneye listener denir. Listener, event için listener interfaceyi uygulayan bir nesnedir.  

```java
mMyButton = (Button)findViewById(R.id.mybutton);
mMyButton.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
      // do something
    }
});
```

Toast mesajı
```java
public static Toast makeText(Context context, int resId, int duration)
```

Context parametresi genellikle Activity nesnesidir. (**Activity**, **Context** sınıfından türetilmiştir.)
Context nesnesi string id'yi bulup kullanabilmek için gereklidir.
