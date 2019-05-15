# Chapter 2: Prototype Pattern

## GoF Definition

Xác định các kiểu của các object muốn tạo ra bằng cách sử dụng một prototype \(khuôn mẫu, nguyên mẫu\), và tạo ra những object mới bằng cách sao chép prototype này.

## Khái niệm

Mẫu Prototype cung cấp một phương thức thay thế cho việc khởi tạo các đối tượng mới, bằng cách sao chép hoặc nhân bản một instance của một đối tượng hiện có. Điều này có thể giúp giảm thiểu chi phí.

## Ví dụ thực tế

Giả sử bạn chỉ có 1 bản sao chính của một tài liệu giá trị, bạn muốn thử thay đổi một vài thứ và cần xem xét ảnh hưởng của sự thay đổi. Trong trường hợp này, bạn có thể tạo ra một bản copy của tài liệu gốc và chỉnh sửa thay đổi trên đó.

## Ví dụ chuyên ngành

Cứ cho là bạn đã có một ứng dụng ổn định. Trong tương lai, bạn có thể muốn chỉnh sửa ứng dụng với một vài thay đổi nhỏ. Bạn phải bắt đầu với một bản copy của ứng dụng gốc, thay đổi nó, rồi phân tích thêm. Chắc là bạn sẽ không muốn viết lại ứng dụng hoàn toàn từ đầu để chỉ thử một vài thay đổi nhỏ xíu. Việc này sẽ chôm của bạn rất nhiều thời gian và tiền bạc, tôi cá luôn.

## Minh họa và giải thích

Trong ví dụ minh họa này, tôi sẽ làm theo structure \(cấu trúc, kiến trúc\) được trình bày trong hình 2-1 ngay dưới đây:

![H&#xEC;nh 2-1. V&#xED; d&#x1EE5; Prototype](../../.gitbook/assets/img-2-1.png)

Ở đây, `BasicCar`_\_là một prototype,_ `Nano` _và_ `Ford` _là concrete prototypes \(những khuôn mẫu cụ thể\), và chúng đã được implement phương thức_ `Clone()`_- là phương thức đã được định nghĩa trong_ `BasicCar.` _Hãy chú ý rằng trong ví dụ này tôi đã tạo một object_ `BasicCar` _với một vài \_price_ \(giá tiền\) mặt định. Sau đó tôi chỉnh sửa _price_ cho mỗi model. `Program.cs` được xem là`client`- là thứ được dùng để chạy chương trình chắc ai cũng biết rồi.

### Class Diagram

![H&#xEC;nh 2-2. Class diagram](../../.gitbook/assets/img-2-2.png)

### Directed Graph Document \(Đồ thị có hướng\)

![H&#xEC;nh 2-3. Directed Graph Document](../../.gitbook/assets/img-2-3.png)

### Solution Explorer View

![H&#xEC;nh 2-4. Solution Explorer View](../../.gitbook/assets/img-2-4.png)

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
Chú ý: Bạn có thể thấy sự khác biệt về price trên máy của bạn bởi vì chỗ này tôi generate một price ngẫu nhiên trong phương thức `SetPrice()` bên trong `BasicCar` class. Nhưng tôi đã đảm bảo rằng price của `Ford` sẽ lớn hơn `Nano`_._
{% endhint %}

## Q&A Session

#### **Lợi ích của việc dùng mẫu thiết kế Prototype?**

* Bạn có thể đưa thêm vào hoặc loại bỏ các product lúc runtime
* Trong một vài bối cảnh, bạn có thể tạo mới các instance với chi phí rẻ hơn
* Tập trung vào việc thay đổi, tùy biến hơn là lo lắng về sự phức tạo trong quá trình tạo ra một instance mới.
* Lúc viết code để thực thi ứng dụng thì không cần lo việc tạo ra object, chỉ cần copy object đã có.  

**Sử dụng mẫu thiết kế Prototype thì có những thử thách, khó khăn gì không?**

* Các lớp con phải thực hiện cơ chế nhân bản hoặc sao chép.
* Việc thực hiện cơ chế nhân bản có thể sẽ rất thử thách nếu như các object đang xem xét không hỗ trợ cơ chế sao chép hoặc nếu có các circular reference \(tham chiếu vòng - cái này phụ thuộc cái kia\)
* Trong ví dụ minh họa này, tôi đã sử dụng phương thức `MemberwiseClone()` , nó sẽ thi hành cơ chế `shallow copy` trong C\#. Đại khái, nó tạo một object rồi copy các field `nonstatic` của object hiện tại vào một object mới.

  MSDN cũng giải thích thêm về `MemberwiseClone`:

  `For a value type field, it performs a bit-by-bit copy, but for a reference type field, the references are copied but referred objects are not copied. So, the original object and the cloned object both refer to the same object. If you need a deep copy in your application, that can be expensive.`  
  Tham khảo thêm bài viết về shallow copy và deep copy: [http://thachleblog.com/shallow-copy-va-deep-copy/](http://thachleblog.com/shallow-copy-va-deep-copy/)

![](../../.gitbook/assets/img-2-shallowcopy-deepcopy.png)

**Bạn có thể demo một cách copy bằng constructor?**

```csharp
class Student
{
    int rollNo;
    string name;
    //Instance Constructor
    public Student(int rollNo, string name)
    {
        this.rollNo = rollNo;
        this.name = name;
    }
    //Copy Constructor
    public Student(Student student)
    {
        this.name = student.name;
        this.rollNo = student.rollNo;
    }
    public void DisplayDetails()
    {
        Console.WriteLine("Student name :{0}, Roll no: {1}",
        name, rollNo);
    }
}

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("***A simple copy constructor demo***\n");
        Student student1 = new Student(1, "John");
        Console.WriteLine("The details of student1 is as follows:");
        student1.DisplayDetails();
        Console.WriteLine("\nCopying student1 to student2 now");
        Student student2 = new Student(student1);
        Console.WriteLine("The details of student2 is as follows:");
        student2.DisplayDetails();
        Console.ReadKey();
    }
}
```

_Kết quả khi chạy chương trình:_

```text
***A simple copy constructor demo***

The details of student1 is as follows:
Student name :John, Roll no: 1

Copying student1 to student2 now
The details of student2 is as follows:
Student name :John, Roll no: 1
```

## Tham khảo thêm

* [https://kipalog.com/posts/Design-Pattern--Prototype-Pattern---C-](https://kipalog.com/posts/Design-Pattern--Prototype-Pattern---C-)
* [https://tndhuy.wordpress.com/2011/05/29/prototype/](https://tndhuy.wordpress.com/2011/05/29/prototype/)
* [https://www.dotnettricks.com/learn/designpatterns/prototype-design-pattern-dotnet](https://www.dotnettricks.com/learn/designpatterns/prototype-design-pattern-dotnet)

