<div dir="rtl">

# آموزش دیزاین پترن Abstract Factory

این دیزاین پترن مشابه دیزاین پترن Factory هست با این تفاوت که در دیزاین پترن Factory ما در Factory Class بر اساس ورودی های مختلف در بلاک های if-else خروجی های متفاوتی رو برای کلاینت ارسال می کردیم ولی در این دیزاین پترن دیگه از بلاک های if-else برای ارسال خروجی استفاده نمی کنیم. در این دیزاین پترن به ازای هر sub-class یک Factory Class داریم که به ما کمک می کنن تا بر اساس ورودی های کاربر object مورد نظر رو به سمت کلاینت ارسال کنیم.

## نحوه پیاده سازی دیزاین پترن Abstract Factory

مثل دیزاین پترن Factory در ابتدا کلاس های Computer, PC و Server رو می سازیم.


<div dir="ltr">

```java

package mohsen.coder.abstractfactory;

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


<div dir="ltr">

```java

package mohsen.coder.abstractfactory;

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

package mohsen.coder.abstractfactory;


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

### ایجاد Factory Class برای هرکدام از subclass ها

در این مرحله ما باید برای هر کدوم از subclass ها یک Factory Class بسازیم. ولی قبل از ساخت کلاس های Factory باید یک اینترفیس ایجاد کنیم و اون اینترفیس رو روی کلاس های Factory پیاده سازی کنیم.

<div dir="ltr">

```java

package mohsen.coder.abstractfactory;


public interface ComputerAbstractFactory {
    //note: this method should be public in implementation
    Computer createComputer(); 

}
```

</div>

حالا باید این اینترفیس رو در کلاس های Factory subclass پیاده کنیم

<div dir="ltr">

```java

package mohsen.coder.abstractfactory;


public class PCFactory implements ComputerAbstractFactory {

    private String ram;
    private String hdd;
    private String cpu;

    public PCFactory(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public Computer createComputer() {
        return new PC(ram,hdd,cpu);
    }

}
```

</div>

<div dir="ltr">

```java

package mohsen.coder.abstractfactory;


public class ServerFactory implements ComputerAbstractFactory {

    private String ram;
    private String hdd;
    private String cpu;

    public ServerFactory(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }

    @Override
    public Computer createComputer() {
        return new Server(ram,hdd,cpu);
    }

}

```

</div>

در این مرحله باید یک کلاس Factory دیگه ایجاد کنیم که بر اساس ورودی کلاینت object مورد نظر رو ایجاد کنه و برای کلاینت ارسال کنه.

<div dir="ltr">

```java

package mohsen.coder.abstractfactory;


public class ComputerFactory {

    public static Computer getComputer(ComputerAbstractFactory factory){
        return factory.createComputer();
    }
}
```

</div>


در کلاس بالا متد getComputer یک ورودی از نوع ComputerAbstractFactory (همون اینترفیسی که روی کلاس های Factory subclass پیاده کردیم) میگیره و یک object از نوع Computer به کلاینت میده.

در زیر میتونید کد تست این دیزاین پترن رو ببینید.

<div dir="ltr">

```java

package mohsen.coder.abstractfactory;


public class TestAbstractFactory {
    
    public static void main(String[] args) {
        Computer pc = ComputerFactory.getComputer(new PCFactory("2 GB", "500 GB", "2.4 GHz"));
        Computer server = ComputerFactory.getComputer(new ServerFactory("16 GB", "1 TB", "2.9 GHz"));
        System.out.println("AbstractFactory PC Config::" + pc);
        System.out.println("AbstractFactory Server Config::" + server);
    }
    
}

```

</div>

</div>