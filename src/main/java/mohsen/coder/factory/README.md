<div dir="rtl">

# آموزش دیزاین پترن Factory

از این دیزاین پترن زمانی استفاده می کنیم که یک کلاس پدر و چندین کلاس فرزند داریم و باید بر اساس ورودی که دریافت می کنیم یک نمونه (instance) از کلاس فرزند ایجاد کنیم و به سمت کلاینت ارسال کنیم.

این پترن وظیفه ساخت نمونه های مورد نیاز کلاینت رو به عهده میگیره و کلاینت مجبور نیست خودش نمونه ها رو بسازه.

 ## نحوه پیاده سازی پترن

در این دیزاین پترن ما به یک super class (من میگم کلاس پدر) که میتونه یک اینترفیس یا abstract class یا یک کلاس معمولی باشه نیاز داریم.

برای مثال من یک super class به اسم Computer ایجاد میکنم.

<div dir="ltr">

```java

package mohsen.coder.factory;

public abstract class Computer {

    public abstract String getRAM();
    public abstract String getHDD();
    public abstract String getCPU();

    @Override
    public String toString(){
        return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
    }
}

```

</div>

بعد از ساخت super class ما نیاز به چند sub-class (کلاس فرزند) داریم.

در این مثال من دو کلاس به اسم های PC و Server ایجاد میکنم.




<div dir="ltr">

```java

package mohsen.coder.factory;

public class PC extends Computer {

    private String ram;
    private String hdd;
    private String cpu;

    public PC(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }

    @Override
    public String getHDD() {
        return this.hdd;
    }

    @Override
    public String getCPU() {
        return this.cpu;
    }

}

```

</div>



<div dir="ltr">

```java

package mohsen.coder.factory;

public class Server extends Computer {

    private String ram;
    private String hdd;
    private String cpu;

    public Server(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }

    @Override
    public String getHDD() {
        return this.hdd;
    }

    @Override
    public String getCPU() {
        return this.cpu;
    }

}

```

</div>


پس از ساخت sub-classes ما نیاز به یک Factory Class داریم که نمونه های مورد نیاز کلاینت رو بسازه.


<div dir="ltr">

```java

package mohsen.coder.factory;

public class ComputerFactory {

    public static Computer getComputer(String type, String ram, String hdd, String cpu) {
        if ("PC".equalsIgnoreCase(type)) return new PC(ram, hdd, cpu);
        else if ("Server".equalsIgnoreCase(type)) return new Server(ram, hdd, cpu);

        return null;
    }
}

```

</div>

همونطور که می بینید متد getComputer بر اساس ورودی که دریافت میکنه نمونه موردنظر رو ایجاد میکنه.

مزایای استفاده از دیزاین پترن Factory:

<ol>

<li>
باعث عدم وابستگی کلاس ها به همدیگه میشه.
</li>
<li>
باعث میشه جزئیات ساخت آبجکت از کلاینت مخفی بمونه.
</li>
<li>
باعث میشه کد ما قابلیت توسعه بهتری داشته باشه.
</li>

</ol>


</div>