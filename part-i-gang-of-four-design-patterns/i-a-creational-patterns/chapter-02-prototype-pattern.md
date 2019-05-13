# Chapter 2: Prototype Pattern

## GoF Definition

Xác định các kiểu của các object muốn tạo ra bằng cách sử dụng một prototype \(khuôn mẫu, nguyên mẫu\), và tạo ra những object mới bằng cách sao chép prototype này.

## Khái niệm

Mẫu Prototype cung cấp một phương thức thay thế cho việc khởi tạo các đối tượng mới, bằng cách sao chép hoặc nhân bản một instance của một đối tượng hiện có. Điều này có thể giúp giảm thiểu chi phí khi tạo một instance mới bằng cách sử dụng khái niệm này.

## Ví dụ thực tế

Giả sử bạn chỉ có 1 bản sao chính của một tài liệu giá trị, bạn muốn thử thay đổi một vài thứ và cần xem xét ảnh hưởng của sự thay đổi. Trong trường hợp này, bạn có thể tạo ra một bản copy của tài liệu gốc và chỉnh sửa thay đổi trên đó.

## Ví dụ chuyên ngành

Cứ cho là bạn đã có một ứng dụng ổn định. Trong tương lai, bạn có thể muốn chỉnh sửa ứng dụng với một vài thay đổi nhỏ. Bạn phải bắt đầu với một bản copy của ứng dụng gốc, thay đổi nó, rồi phân tích thêm. Chắc là bạn sẽ không muốn viết lại ứng dụng hoàn toàn từ đầu để chỉ thử một vài thay đổi nhỏ xíu. Việc này sẽ chôm của bạn rất nhiều thời gian và tiền bạc, tôi cá luôn.

## Minh họa và giải thích

Trong ví dụ minh họa này, tôi sẽ làm theo structure \(cấu trúc, kiến trúc\) được trình bày trong hình 2-1 ngày bên dưới:

![H&#xEC;nh 2-1. V&#xED; d&#x1EE5; Prototype](../../.gitbook/assets/image%20%283%29.png)

Ở đây, _`BasicCar`_là một prototype, _`Nano`_ và _`Ford`_ là concrete prototypes \(những khuôn mẫu cụ thể\), và chúng đã được implement phương thức _`Clone()`_- là phương thức đã được định nghĩa trong _`BasicCar.`_ Hãy chú ý rằng trong ví dụ này tôi đã tạo một object _`BasicCar`_ với một vài price \(giá tiền\) mặt định. Sau đó tôi chỉnh sửa price cho mỗi model. _`Program.cs`_ là thứ được dùng để chạy chương trình chắc ai cũng biết rồi.

### Class Diagram

![H&#xEC;nh 2-2. Class diagram](../../.gitbook/assets/image%20%284%29.png)

### Directed Graph Document \(Đồ thị có hướng\)

![H&#xEC;nh 2-3. Directed Graph Document](../../.gitbook/assets/image%20%285%29.png)

### Solution Explorer View

![H&#xEC;nh 2-4. Solution Explorer View](../../.gitbook/assets/image%20%286%29.png)

### Giờ code đến rồi

```csharp
//BasicCar.cs
using System;
namespace PrototypePattern
{
    public abstract class BasicCar
    {
        public string ModelName { get; set; }
        public int Price { get; set; }
        public static int SetPrice()
        {
            int price = 0;
            Random r = new Random();
            int p = r.Next(200000, 500000);
            price = p;
            return price;
        }
        public abstract BasicCar Clone();
    }
}

//Nano.cs
using System;
namespace PrototypePattern
{
    public class Nano : BasicCar
    {
        public Nano(string m)
        {
            ModelName = m;
        }
        public override BasicCar Clone()
        {
            return (Nano)this.MemberwiseClone();//shallow Clone
        }
    }
}

//Ford.cs
using System;
namespace PrototypePattern
{
    public class Ford : BasicCar
    {
        public Ford(string m)
        {
            ModelName = m;
        }
        public override BasicCar Clone()
        {
            return (Ford)this.MemberwiseClone();
        }
    }
}

//Client
using System;
namespace PrototypePattern
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Prototype Pattern Demo***\n");
            //Base or Original Copy
            BasicCar nano_base = new Nano("Green Nano") { Price = 100000 };
            BasicCar ford_base = new Ford("Ford Yellow") { Price = 500000 };

            BasicCar bc1;
            //Nano
            bc1 = nano_base.Clone();
            bc1.Price = nano_base.Price + BasicCar.SetPrice();
            Console.WriteLine("Car is: {0}, and it's price is Rs. {1}", 
                                bc1.ModelName,
                                bc1.Price);

            //Ford
            bc1 = ford_base.Clone();
            bc1.Price = ford_base.Price + BasicCar.SetPrice();
            Console.WriteLine("Car is: {0}, and it's price is Rs. {1}", 
                                bc1.ModelName, 
                                bc1.Price);
            Console.ReadLine();
        }
    }
}
```

### Output

```text
***Prototype Pattern Demo***
Car is: Green Nano, and it's price is Rs. 486026
Car is: Ford Yellow, and it's price is Rs. 886026
```

{% hint style="info" %}
Chú ý: Bạn có thể thấy sự khác biệt về price trên máy của bạn bởi vì chỗ này tôi generate một price  ngẫu nhiên trong phương thức _`SetPrice()`_ bên trong _`BasicCar`_ class. Nhưng tôi đã đảm bảo rằng price của _`Ford`_ sẽ lớn hơn _`Nano`._
{% endhint %}

## Q&A Session

1. **What are the advantages of using the Prototype design pattern?**

   • You can include or discard products at runtime.

   • In some contexts, you can create new instances with a cheaper cost.

   • You can focus on the key activities rather than focusing on complicated instance creation processes.

   • Clients can ignore the complex creation process for objects and instead clone or copy objects.  

2. **What are the challenges associated with using the Prototype design pattern?**
   * Each subclass must implement the cloning or copying mechanism.
   * Implementing the cloning mechanism can be challenging if the objects under consideration do not support copying or if there are circular references. 
   * In this example, I have used the MemberwiseClone\(\) member that performs a shallow copy in C\#. Actually, it creates an object and then copies the nonstatic fields of the current object into the newly created object. MSDN further says that for a value type field, it performs a bit-by-bit copy, but for a reference type field, the references are copied but referred objects are not copied. So, the original object and the cloned object both refer to the same object. If you need a deep copy in your application, that can be expensive.



{% hint style="info" %}
Still translating...
{% endhint %}





