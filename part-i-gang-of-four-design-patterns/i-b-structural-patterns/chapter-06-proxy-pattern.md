# Chapter 6: Proxy Pattern

{% hint style="warning" %}
LƯU Ý: CHƯƠNG NÀY ĐANG ĐƯỢC DỊCH
{% endhint %}

## GoF Definition

Cung cấp một thứ thay thế \(surrogate\) hoặc giữ chỗ \(placeholder\) cho một object khác để kiểm soát việc truy cập vào nó.

## Khái niệm

Một proxy về cơ bản là một thứ thay thế cho một object mà bạn muốn/dự định làm việc. Khi một Client giao tiếp với một proxy, nó sẽ nghĩ rằng nó đang làm việc trực tiếp với object thực sự. Bạn sẽ phải chơi với  kiểu thiết kế này bởi vì việc xử lý một đối tượng gốc \(original object\) không phải lúc nào cũng có thể. Điều này là do nhiều yếu tố như vấn đề bảo mật chẳng hạn. Do vậy, trong mẫu này, bạn có thể muốn sử dụng một class mà có thể thực hiện như một interface đến một thứ gì khác.

## Ví dụ thực tế

Thời sinh viên mình rất hay cúp tiết, lúc điểm danh thầy gọi tên mình, bạn thân của mình có thể sẽ giả giọng mình để điểm danh hộ😅, một người bạn tốt như vậy là một ví dụ điển hình cho mẫu proxy.

## Ví dụ chuyên ngành

An ATM implementation will hold proxy objects for bank information that exists on a remote server. In the real programming world, creating multiple instances of a complex object \(a heavy object\) is costly in general. So, whenever you can, you should create multiple proxy objects that can point to the original object. This mechanism can also help you to save the computer/system memory.

## Minh họa và giải thích

In this program, you are calling the DoSomeWork\(\) method of the proxy object that, in turn, calls the DoSomeWork\(\) method of an object of ConcreteSubject. When customers see the output, they think they have invoked the method from their intended object directly.

### Class Diagram

![H&#xEC;nh 6-1. Class diagram](../../.gitbook/assets/img-6-1.png)

### Directed Graph Document

![H&#xEC;nh 6-2. Directed Graph Document](../../.gitbook/assets/img-6-2.png)

### Solution Explorer View

Hình 6-3 shows the high-level structure of the parts of the program. \(Note that you could separate the proxy class into a different file. But since the parts are small in this example, I have put everything in a single file.\)

![H&#xEC;nh 6-3. Solution Explorer View](../../.gitbook/assets/img-6-3.png)

### Viết Code

```csharp
using System;

namespace ProxyPattern
{
    /// <summary>
    /// Abstract class Subject
    /// </summary>
    public abstract class Subject
    {
        public abstract void DoSomeWork();
    }

    /// <summary>
    /// ConcreteSubject class
    /// </summary>
    public class ConcreteSubject : Subject
    {
        public override void DoSomeWork()
        {
            Console.WriteLine("ConcreteSubject.DoSomeWork()");
        }
    }

    /// <summary>
    /// Proxy class
    /// </summary>
    public class Proxy : Subject
    {
        Subject cs;
        public override void DoSomeWork()
        {
            Console.WriteLine("Proxy call happening now...");

            //Lazy initialization:We'll not instantiate until the method is called
            if (cs == null)
            {
                cs = new ConcreteSubject();
            }

            cs.DoSomeWork();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Proxy Pattern Demo***\n");

            Proxy px = new Proxy();
            px.DoSomeWork();

            Console.ReadKey();
        }
    }
}
```

### Output

```text
***Proxy Pattern Demo***

Proxy call happening now...
ConcreteSubject.DoSomeWork()
```

## Hỏi Đáp

**What are the different types of proxies?**



**You could create the ConcreteSubject instance in the proxy class constructor as shown here:**



**But with this lazy instantiation process, you may create unnecessary objects in a multithreaded application. Is this understanding correct?**



**Can you give an example of a remote proxy?**



**When can you use a virtual proxy?**



**When can you use a protection proxy?**



Dưới đây là code đã chỉnh sửa:

```csharp
using System;
using System.Linq;//For Contains() method below

namespace ProxyPatternQAs
{
    /// <summary>
    /// Abstract class Subject
    /// </summary>
    public abstract class Subject
    {
        public abstract void DoSomeWork();
    }

    /// <summary>
    /// ConcreteSubject class
    /// </summary>
    public class ConcreteSubject : Subject
    {
        public override void DoSomeWork()
        {
            Console.WriteLine("ConcreteSubject.DoSomeWork()");
        }
    }

    /// <summary>
    /// Proxy class
    /// </summary>
    public class Proxy : Subject
    {
        Subject cs;
        string[] registeredUsers;
        string currentUser;

        public Proxy(string currentUser)
        {
            //Avoiding to instantiate inside the constructor
            //cs = new ConcreteSubject();
            //Registered users
            registeredUsers = new string[] { "Admin", "Rohit", "Sam" };
            this.currentUser = currentUser;
        }
        public override void DoSomeWork()
        {
            Console.WriteLine("\nProxy call happening now...");
            Console.WriteLine("{0} wants to invoke a proxy method.", 
                currentUser);

            if (registeredUsers.Contains(currentUser))
            {
                // Lazy initialization: We'll not instantiate until the
                // method is called
                if (cs == null)
                {
                    cs = new ConcreteSubject();
                }
                cs.DoSomeWork();
            }
            else
            {
                Console.WriteLine("Sorry {0}, you do not have access.",
               currentUser);
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Proxy Pattern Demo***\n");

            //Authorized user-Admin
            Proxy px1 = new Proxy("Admin");
            px1.DoSomeWork();

            //Unwanted User-Robin
            Proxy px2 = new Proxy("Robin");
            px2.DoSomeWork();

            Console.ReadKey();
        }
    }
}
```

Kết quả sau khi chỉnh sửa:

```text
***Proxy Pattern Demo***

Proxy call happening now...
Admin wants to invoke a proxy method.
ConcreteSubject.DoSomeWork()

Proxy call happening now...
Robin wants to invoke a proxy method.
Sorry Robin, you do not have access.
```

**It looks like proxies act like decorators. Is this understanding correct?**



## Tham khảo thêm

* [https://gpcoder.com/4644-huong-dan-java-design-pattern-proxy/](https://gpcoder.com/4644-huong-dan-java-design-pattern-proxy/)
* [https://www.stdio.vn/articles/design-pattern-proxy-pattern-601](https://www.stdio.vn/articles/design-pattern-proxy-pattern-601)
* [https://appstechviet.wordpress.com/2016/08/23/design-pattern-the-proxy-pattern/](https://appstechviet.wordpress.com/2016/08/23/design-pattern-the-proxy-pattern/)
* [https://nixforest.wordpress.com/2010/12/06/design-pattern-proxy/](https://nixforest.wordpress.com/2010/12/06/design-pattern-proxy/)

