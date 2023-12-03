<div dir="rtl">

# آموزش دیزاین پترن Adapter 

این دیزاین پترن، یک دیزاین پترن ساختاری هست که از اون برای ارتباط دوتا اینترفیس که با هم ارتباطی ندارن استفاده میشه.
به آبجکتی که باعث ارتباط دو اینترفیس نامرتبط میشه Adapter میگن.

یکی از مثال هایی که در دنیای واقعی میتونیم بزنیم آداپتور موبایل هست. آداپتور موبایل برق 220 ولت رو دریافت میکنه و ولتاژ برق رو به اندازه مورد نیاز گوشی موبایل (مثلا 3 ولت) تبدیل میکنه.

در این آموزش قراره یک Multi-Adapter باهم بسازیم تا نحوه کار این دیزاین پترن رو یاد بگیرید.

در ابتدا ما به دو کلاس Volt و Socket نیاز داریم.

کلاس Volt برای اندازه گیری ولتاژ

کلاس Socket (همون پریز برق) ولتاژ 220 ولت تولید میکنه.

<div dir="ltr">

```java

package mohsen.coder.adapter;


public class Volt {

    private int volts;

    public Volt(int v){
        this.volts=v;
    }

    public int getVolts() {
        return volts;
    }

    public void setVolts(int volts) {
        this.volts = volts;
    }

}

```

</div>



<div dir="ltr">

```java

package mohsen.coder.adapter;


public class Socket {

    public Volt getVolt(){
        return new Volt(220);
    }
}

```

</div>


حالا ما میخوایم یک آداپتور درست کنیم که ولتاژ های 3، 12 و 220 ولت برای ما تولید کنه. برای اینکار در ابتدا باید اینترفیس آداپتور مون رو ایجاد کنیم.



<div dir="ltr">

```java

package mohsen.coder.adapter;


public interface SocketAdapter {

    Volt get220Volt();

    Volt get12Volt();

    Volt get3Volt();
}

```

</div>


دوتا روش برای پیاده سازی آداپتور مون وجود داره:

<ol>

<li>
Class Adapter - در این روش ما از ارث بری استفاده می کنیم.
</li>

<li>
Object Adapter - در این روش ما از روش کامپوزیشن استفاده میکنیم.
</li>

</ol>



## Adapter Design Pattern - Class Adapter

<div dir="ltr">

```java

package mohsen.coder.adapter;


//Using inheritance for adapter pattern
public class SocketClassAdapterImpl extends Socket implements SocketAdapter{

    @Override
    public Volt get220Volt() {
        return getVolt();
    }

    @Override
    public Volt get12Volt() {
        Volt v = getVolt();
        return convertVolt(v,10);
    }

    @Override
    public Volt get3Volt() {
        Volt v = getVolt();
        return convertVolt(v,40);
    }

    private Volt convertVolt(Volt v, int i) {
        return new Volt(v.getVolts() / i);
    }

}

```

</div>


## Adapter Design Pattern - Object Adapter Implementation



<div dir="ltr">

```java

package mohsen.coder.adapter;


public class SocketObjectAdapterImpl implements SocketAdapter{

    //Using Composition for adapter pattern
    private Socket sock = new Socket();

    @Override
    public Volt get220Volt() {
        return sock.getVolt();
    }

    @Override
    public Volt get12Volt() {
        Volt v= sock.getVolt();
        return convertVolt(v,10);
    }

    @Override
    public Volt get3Volt() {
        Volt v= sock.getVolt();
        return convertVolt(v,40);
    }

    private Volt convertVolt(Volt v, int i) {
        return new Volt(v.getVolts()/i);
    }
}

```

</div>


همونطور که می بینید دو روش تقریبا یکسان هستن و در هر دو روش اینترفیس SocketAdapter پیاده سازی شده.

نکته: ما میتونیم بجای اینترفیس از Abstract class هم استفاده کنیم.


در ادامه کد تست این دیزاین پترن رو مشاهده می کنید.



<div dir="ltr">

```java

package mohsen.coder.adapter;


public class AdapterPatternTest {

    public static void main(String[] args) {

        testClassAdapter();
        testObjectAdapter();
    }

    private static void testObjectAdapter() {
        SocketAdapter sockAdapter = new SocketObjectAdapterImpl();
        Volt v3 = getVolt(sockAdapter,3);
        Volt v12 = getVolt(sockAdapter,12);
        Volt v220 = getVolt(sockAdapter,220);
        System.out.println("v3 volts using Object Adapter="+v3.getVolts());
        System.out.println("v12 volts using Object Adapter="+v12.getVolts());
        System.out.println("v220 volts using Object Adapter="+v220.getVolts());
    }

    private static void testClassAdapter() {
        SocketAdapter sockAdapter = new SocketClassAdapterImpl();
        Volt v3 = getVolt(sockAdapter,3);
        Volt v12 = getVolt(sockAdapter,12);
        Volt v220 = getVolt(sockAdapter,220);
        System.out.println("v3 volts using Class Adapter="+v3.getVolts());
        System.out.println("v12 volts using Class Adapter="+v12.getVolts());
        System.out.println("v220 volts using Class Adapter="+v220.getVolts());
    }

    private static Volt getVolt(SocketAdapter sockAdapter, int i) {
        switch (i){
            case 3: return sockAdapter.get3Volt();
            case 12: return sockAdapter.get12Volt();
            case 220: return sockAdapter.get220Volt();
            default: return sockAdapter.get220Volt();
        }
    }
}

```

</div>

</div>