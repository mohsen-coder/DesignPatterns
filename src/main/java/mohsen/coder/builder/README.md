[//]: # (<style>)

[//]: # (@import url&#40;'https://fonts.googleapis.com/css2?family=Roboto:wght@900&family=Vazirmatn:wght@200;300;500;900&display=swap'&#41;;)

[//]: # (</style>)

<div dir="rtl" style="font-family: 'Vazirmatn'; line-height: 2rem">

# دیزاین پترن Builder

این دیزاین پترن نیز مثل دیزاین پتر های Factory و Abstract Factory یک دیزاین پترن سازنده است.
این دیزاین پترن راهکاری برای حل برخی از مشکلات دیزاین پترن های Factory و Abstract Factory در زمانی که آبجکت اتریبیوت های زیادی داره ارائه شده است.

در دیزاین پترن های Factory و Abstract Factory وقتی آبجکت اتریبیوت های زیادی داشته باشه باعث ایجاد 3 مشکل میشه. مشکلاتی که ایجاد میکنه عبارتند از:
<ol>
  <li>	چون کلاینت باید آرگومان های زیادی رو برای کلاس Factory ارسال کنه و بیشتر آرگومان ها type یکسانی دارن، برای کلاینت سخته که به همون ترتیب مدنظر کلاس ارسال کنه و ممکنه کلاینت اشتباه کنه در ترتیب ارسال آرگومان ها و باعث بروز خطا بشه.</li>
  <li>بعضی از پارامتر ها ممکنه اختیاری (optional) باشن ولی در پترن Factory ما مجبور هستیم تمام پارامتر های اجباری و اختیاری رو ارسال کنیم. (پارامتر های اختیاری رو بصورت null ارسال میکنیم)</li>
  <li>اگر آبجکت بزرگ باشه و ساختنش پیچیده باشه باعث میشه ساخت کلاس Factory پیچیده، مشکل و گیج کننده بشه.</li>
</ol>

ما میتونیم این مشکلات رو به وسیله یک constructor که اتریبیوت های ضروری (require) و همچنین تعداد زیادی متد setter که اتریبیوت های اختیاری (optional) رو مقدار دهی کنه،  استفاده کنیم.
ولی خب مشکلی که این روش داره اینه چون ما اول آبجکت رو می سازیم و بعد اتریبیوت های اختیاریش رو مقداری دهی میکنیم، ممکنه آبجکت رو در وضعیت ناسازگاری قرار بده.

راهکار چیه؟ دیزاین پترن Builder

# دیزاین پترن Builder 

برای پیاده سازی این دیزاین پترن مراحل زیر رو طی میکنیم:

<ol>
<li>
در ابتدا ما به یک کلاس استاتیک درونی نیاز داریم و سپس تمام اتریبیوت های کلاس پدر رو در کلاس فرزند کپی میکنیم، سپس یک متد constructor در کلاس پدر ایجاد میکنیم که یک اتریبیوت از نوع کلاس فرزند دریافت میکنه و سپس اتریبیوت های کلاس پدر رو با مقادیر کلاس فرزند مقدار دهی میکنیم. درضمن اصول نامگذاری رو باید رعایت کنیم. مثلا اگر کلاس پدر اسمش Computer هست، اسم کلاس فرزند (کلاس درونی) باید ComputerBuilder باشه.
</li>
<li>
کلاس درونی باید یک متد public constructor برای مقدار دهی تمام اتریبیوت های اجباری (require) داشته باشه.
</li>
<li>
کلاس درونی باید متد هایی برای مقدار دهی اتریبیوت های اختیاری داشته باشه و در انتهای هر کدوم از اون متد ها همون آبجکت (this) رو return کنه.
</li>
<li>
مرحله آخر ایجاد یک متد build هست در اون یک آبجکت از نوع کلاس پدر ایجاد میکنیم و سپس آبجکت کلاس سازنده رو به عنوان پارامتر به متد constructor کلاس پدر ارسال میکنیم.
</li>
</ol>

نمونه کد:

<div dir="ltr">

```java

package mohsen.coder.builderdesignpattern;

public class Computer {
	
	//required parameters
	private String HDD;
	private String RAM;
	
	//optional parameters
	private boolean isGraphicsCardEnabled;
	private boolean isBluetoothEnabled;
	

	public String getHDD() {
		return HDD;
	}

	public String getRAM() {
		return RAM;
	}

	public boolean isGraphicsCardEnabled() {
		return isGraphicsCardEnabled;
	}

	public boolean isBluetoothEnabled() {
		return isBluetoothEnabled;
	}
	
	private Computer(ComputerBuilder builder) {
		this.HDD=builder.HDD;
		this.RAM=builder.RAM;
		this.isGraphicsCardEnabled=builder.isGraphicsCardEnabled;
		this.isBluetoothEnabled=builder.isBluetoothEnabled;
	}
	
	//Builder Class
	public static class ComputerBuilder{

		// required parameters
		private String HDD;
		private String RAM;

		// optional parameters
		private boolean isGraphicsCardEnabled;
		private boolean isBluetoothEnabled;
		
		public ComputerBuilder(String hdd, String ram){
			this.HDD=hdd;
			this.RAM=ram;
		}

		public ComputerBuilder setGraphicsCardEnabled(boolean isGraphicsCardEnabled) {
			this.isGraphicsCardEnabled = isGraphicsCardEnabled;
			return this;
		}

		public ComputerBuilder setBluetoothEnabled(boolean isBluetoothEnabled) {
			this.isBluetoothEnabled = isBluetoothEnabled;
			return this;
		}
		
		public Computer build(){
			return new Computer(this);
		}

	}

}

```

</div>


نکته: دقت کنید که کلاس Computer یک private constructor و فقط متد های getter داره. بنابراین تنها راهی که میتونیم یک آبجکت از نوع کلاس Computer داشته باشیم اینه اون رو توسط کلاس ComputerBuilder ایجاد کنیم.

در زیر یک نمونه کد جهت تست این دیزاین پترن هست که میتونید روی سیستم خودتون اجراش کنید.

<div dir="ltr">


```java

package mohsen.coder.test;

import mohsen.coder.builderdesignpattern.Computer;

public class TestBuilderPattern {

	public static void main(String[] args) {
		//Using builder to get the object in a single line of code and 
                //without any inconsistent state or arguments management issues		
		Computer comp = new Computer.ComputerBuilder(
				"500 GB", "2 GB").setBluetoothEnabled(true)
				.setGraphicsCardEnabled(true).build();
	}

}

```


</div>

</div>
