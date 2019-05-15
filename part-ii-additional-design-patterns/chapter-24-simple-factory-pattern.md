# Chapter 24: Simple Factory Pattern

{% hint style="warning" %}
**LƯU Ý:**  
CHƯƠNG NÀY VẪN CHƯA DỊCH XONG
{% endhint %}

## Định nghĩa

Tạo ra các đối tượng mà không để lộ việc tạo ra đối tượng đó như thế nào khi sử dụng

## Khái niệm

Trong lập trình hướng đối tượng, một factory là một thứ object mà có thể tạo ra những object khác. Một factory có thể được dùng/được gọi ra bằng nhiều cách khác nhau nhưng thường xuyên nhất là gọi một phương thức mà có thể trả về những object với các nguyên mẫu khác nhau. Bất kỳ subroutine \(chương trình con\) nào mà giúp chúng ta tạo ra những object mới này thì có thể được xem như là một factory. Quan trọng nhất, nó sẽ giúp bạn trừu tượng hóa việc tạo ra object.

## Ví dụ trong thực tế

Trong một nhà hàng ở Nam Ấn Độ, khi bạn order một dĩa Biryani yêu thích, người phục vụ có thể hỏi bạn có muốn Biryani của bạn có thêm hoặc bớt gia vị. Dựa trên sự yêu cầu của bạn, đầu bếp sẽ điều chỉnh cho phù hợp.

## Ví dụ chuyên ngành

Mẫu Simple Factory rất phổ biến trong các phần mềm ứng dụng, nhưng trước khi đi sâu, bạn nên chú ý những thứ sau đây:

* Mẫu Simple Factory không được xem là mẫu thiết kế tiêu chuẩn trong cuốn sách nổi tiếng GoF, nhưng cách tiếp cận thì khá phổ biến với bất kỳ ứng dụng nào bạn viết nếu bạn muốn tách những đoạn code thay đổi nhiều so với phần code không thay đổi.
* Mẫu Simple Factory được coi là hình thức đơn giản nhất của mẫu Factory Method \(và mẫu Abstract Factory\). Vì vậy, bạn có thể giả sử rằng bất kỳ ứng dụng nào tuân theo mẫu Factory Method hoặc mẫu Abstract Factory đều bắt nguồn từ khái niệm về các mục tiêu thiết kế mẫu Simple Factory.

Chúng ta sẽ thảo luận về mẫu này bằng một trường hợp phổ biến. Chiến thôi.

## Minh họa và giải thích

Có một vài đặc điểm quan trọng dưới đây:

* Trong ví dụ này, bạn đang làm việc với 2 loài vật: Dog và Tiger. Quá trình tạo ra object sẽ phụ thuộc vào input của user
* Bạn có thể giả định rằng loài nào cũng có thể nói \(speak\) và thích làm gì đó \(action\)
* Trong client code, bạn sẽ thấy dòng này: `preferredType = simpleFactory.CreateAnimal();`
* Tôi đã viết code tạo object ở chỗ khác, cụ thể là trong một factory class. Với cách này, bạn sẽ không phải sử dụng từ khóa _`new`_ trong code client
* Bạn có thể cũng sẽ thấy rằng tôi đã tách code có thể thay đổi từ code ít có khả năng thay đổi. Cơ chế này giúp bạn loại bỏ sự liên kết chặt chẽ trong hệ thống \(làm sao được vậy? Đọc thêm phần Hỏi Đáp\)

{% hint style="info" %}
**Lưu ý:**  
Ở một số nơi, bạn có thể thấy biến thể của mẫu này trong đó các object được tạo thông qua một method có tham số kiểu như _`preferType = simpleFactory.CreateAnimal ("Tiger")`_ và trong đó class factory của bạn không kế thừa từ một abstract class hay interface.
{% endhint %}

### Class Diagram

![H&#xEC;nh  24-1. Class diagram](../.gitbook/assets/img-24-1.png)

### Directed Graph Document

![H&#xEC;nh 24-2. Directed Graph Document](../.gitbook/assets/img-24-2.png)

### Solution Explorer View

![H&#xEC;nh 24-3. Solution Explorer View](../.gitbook/assets/img-24-3.png)

### Viết Code

```csharp
using System;

namespace SimpleFactoryPattern
{
    public interface IAnimal
    {
        void Speak();
        void Action();
    }

    public class Dog : IAnimal
    {
        public void Speak()
        {
            Console.WriteLine("Dog says: Bow-Wow.");
        }
        public void Action()
        {
            Console.WriteLine("Dogs prefer barking...");
        }
    }

    public class Tiger : IAnimal
    {
        public void Speak()
        {
            Console.WriteLine("Tiger says: Halum.");
        }
        public void Action()
        {
            Console.WriteLine("Tigers prefer hunting...");
        }
    }

    public abstract class ISimpleFactory
    {
        public abstract IAnimal CreateAnimal();
    }

    public class SimpleFactory : ISimpleFactory
    {
        public override IAnimal CreateAnimal()
        {
            IAnimal intendedAnimal = null;
            Console.WriteLine("Enter your choice(0 for Dog, 1 for Tiger)");
            string b1 = Console.ReadLine();
            int input;
            if (int.TryParse(b1, out input))
            {
                Console.WriteLine("You have entered {0}", input);
                switch (input)
                {
                    case 0:
                        intendedAnimal = new Dog();
                        break;
                    case 1:
                        intendedAnimal = new Tiger();
                        break;
                    default:
                        Console.WriteLine("You must enter either 0 or 1");
                        //We'll throw a runtime exception for any other choices.
                        throw new ApplicationException(
                            String.Format(
                                " Unknown Animal cannot be instantiated"));
                }
            }
            return intendedAnimal;
        }
    }

    //A client is interested to get an animal who can 
    //speak and perform an action.
    class Client
    {
        static void Main(string[] args)
        {
            Console.WriteLine("*** Simple Factory Pattern Demo***\n");

            IAnimal preferredType = null;
            ISimpleFactory simpleFactory = new SimpleFactory();

            #region The code region that will vary based on users preference 
            preferredType = simpleFactory.CreateAnimal();
            #endregion

            #region The codes that do not change frequently
            preferredType.Speak();
            preferredType.Action();
            #endregion

            Console.ReadKey();
        }
    }
}
```

### Output

Trường hợp 1, user nhập giá trị 0

```text
***Simple Factory Pattern Demo***
Enter your choice( 0 for Dog, 1 for Tiger)
0
You have entered 0
Dog says: Bow-Wow.
Dogs prefer barking...
```

Trường hợp 2, user nhập giá trị 1

```text
***Simple Factory Pattern Demo***
Enter your choice(0 for Dog, 1 for Tiger)
1
You have entered 1
Tiger says: Halum.
Tigers prefer hunting...
```

Trường hợp 3, user nhập giá trị 3

```text
***Simple Factory Pattern Demo***
Enter your choice(0 for Dog, 1 for Tiger)
3
You have entered 3
You must enter either 0 or 1
And you will receive he folllowing Exception:"Unknown Animal cannot be
instantiated"
```

![H&#xEC;nh  24-4. D&#xED;nh exception do gi&#xE1; tr&#x1ECB; &#x111;&#x1EA7;u v&#xE0;o kh&#xF4;ng h&#x1EE3;p l&#x1EC7;](../.gitbook/assets/img-24-4.png)

## Hỏi Đáp

**Trong ví dụ này,  tui thấy là các client đang ủy thác việc tạo đối tượng thông qua mẫu Simple Factory. Nhưng thay vì như vậy, chúng có thể tạo object trực tiếp với keyword** _**`new`**_**. Đúng không?**



**Có khó khăn gì với mẫu thiết kế này hem?**



**Có thể nào bỏ qua** _**`ISimpleFactory`**_ **trong ví dụ trên?**



**Có thể tạo một static class factory?**



