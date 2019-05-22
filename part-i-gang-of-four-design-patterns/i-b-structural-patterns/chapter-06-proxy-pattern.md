# Chapter 6: Proxy Pattern

{% hint style="warning" %}
LƯU Ý: CHƯƠNG NÀY ĐANG ĐƯỢC DỊCH
{% endhint %}

## GoF Definition

Cung cấp một thứ thay thế \(surrogate\) hoặc giữ chỗ giùm \(placeholder\) cho một object khác để kiểm soát việc truy cập vào nó.

## Khái niệm

Một proxy về cơ bản là một thứ thay thế cho một object mà bạn muốn làm việc hoặc dự định làm việc. Khi một Client giao tiếp với một proxy, nó sẽ nghĩ rằng nó đang làm việc trực tiếp với object thực sự. Bạn sẽ phải chơi với  kiểu thiết kế này bởi vì việc xử lý một đối tượng gốc \(original object\) không phải lúc nào cũng có thể. Điều này là do nhiều yếu tố như vấn đề bảo mật chẳng hạn.

## Ví dụ thực tế

Thời sinh viên mình rất hay cúp tiết, lúc điểm danh thầy gọi tên mình, bạn thân của mình có thể sẽ giả giọng mình để điểm danh hộ😅, một người bạn tốt như vậy là một ví dụ điển hình cho mẫu proxy.

## Ví dụ chuyên ngành

Một máy ATM thường sẽ có các proxy object để lấy thông của ngân hàng \(nằm trên một remote server\). Trong thế giới lập trình, thực tế, chi phí cho việc tạo ra nhiều instance của một object phức tạp, cồng kềnh nói chung khá đắt đỏ. Do vậy, bất cứ khi nào có thể, bạn nên tạo nhiều proxy object trỏ về original object. Cơ chế này có thể giúp bạn tiết kiệm bộ nhớ của hệ thống

## Minh họa và giải thích

Trong chương trình này, bạn đang gọi phương thức _`DoSomeWork()`_ của proxy object, từ đây, proxy object sẽ gọi tiếp phương thức _`DoSomeWork()`_ của ConcreteSubject object. Khi người dùng nhìn thấy output, họ sẽ nghĩ rằng họ đang gọi trực tiếp phương thức _`DoSomeWork()`_ của ConcreteSubject object.

### Class Diagram

![H&#xEC;nh 6-1. Class diagram](../../.gitbook/assets/img-6-1.png)

### Directed Graph Document

![H&#xEC;nh 6-2. Directed Graph Document](../../.gitbook/assets/img-6-2.png)

### Solution Explorer View

Hình 6-3 hiển thị high-level structure của các thành phần trong chương trình. \(Chú ý là bạn có thể tách proxy class ra một file riêng. Nhưng để cho gọn gàng dễ nhìn, tôi đã đặt mọi thứ vào một file duy nhất\)

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

**Có các loại proxy nào?**

Có một vài kiểu proxy phổ biến:

* Remote proxy: Kiểu proxy này có thể trỏ tới một đối tượng nằm ở một nơi khác.
* Virtual proxy: Kiểu proxy này thường được sử dụng để thực hiện tối ưu hóa, khi có nhiều lượt truy xuất vào một đối tượng cấu trúc phức tạp, cồng kềnh, chứa dữ liệu lớn \(hình ảnh, video,..\) trường hợp này chúng ta sẽ tạo ra một _`proxy`_ để đại diện cho đối tượng đó. Đối tượng chỉ được tạo ở lần truy xuất đầu tiên sau đó những lần truy xuất tiếp theo chỉ cần tái sử dụng lại mà không cần khởi tạo để tránh trường hợp sao chép ra nhiều đối tượng, vì vậy sẽ tiếp kiệm được nhiều tài nguyên.
* Protection proxy: Proxy dạng này sẽ xử lý những các quyền truy cập khác nhau.
* Smart reference proxy: Thực hiện các kiểm soát các hoạt động bổ sung khi đối tượng được truy cập bởi Client. Một ví dụ là thường proxy sẽ đếm số lượt tham chiếu đến object ở một thời điểm cụ thể.

> Còn rất nhiều kiểu proxy khác, như Monitor Proxy, Firewall Proxy, Cache Proxy, Synchronization Proxy, Copy-On-Write Proxy... mấy bác tự tìm hiểu thêm

**Tôi có thể create một instance của ConcreteSubject trong constructor của proxy class như dưới đây, đúng không:**

```csharp
/// <summary>
/// Proxy class
/// </summary>
public class Proxy : Subject
{
    Subject cs;

    public Proxy()
    {
        //Instantiating inside the constructor
        cs = new ConcreteSubject();
    }
    
    public override void DoSomeWork()
    {
        Console.WriteLine("Proxy call happening now..");
        cs.DoSomeWork();
    }
}
```

Uh, bạn có thể làm vậy. Nhưng nếu bạn làm theo thiết kế này, bất cứ khi nào bạn khởi tạo một proxy object, bạn cũng cần phải khởi tạo một đối tượng của lớp ConcreteSubject. Do đó, quá trình này có thể sẽ tạo ra các đối tượng không cần thiết.

**Nhưng với lazy instantiation, bạn có thể tạo ra các đối tượng không cần thiết trong ứng dụng đa luồng, đúng không?**

Đúng là vậy, nhưng trong tài liệu này do cố tình chỉ đưa ra các mình họa đơn giản nên tôi đã bỏ qua phần đó. Trong phần thảo luận về mẫu Singleton trong Chương 1, chúng ta đã phân tích một số phương pháp để xử lý trong môi trường đa luồng, bạn luôn có thể tham khảo cho những trường hợp kiểu này. Ví dụ, trong trường hợp cụ thể này, bạn có thể sử dụng Smart Proxy để đảm bảo rằng một đối tượng cụ thể nào đó bị khóa \(lock\) trước khi bạn cấp quyền truy cập cho nó.

**Bạn hãy cho một ví dụ về Remote Proxy?**

Suppose you want to call a method of an object but the object is running in a different address space \(for example, in a different location or on a different computer\). How can you proceed? With the help of remote proxies, you can call the method on the proxy object, which in turn will forward the call to the actual object that is running on the remote machine. This type of need can be realized through different well-known mechanisms such as ASP. NET, CORBA, or Java’s RMI. In C\# applications, you can exercise a similar mechanism with WCF \(.NET Framework version 3.0 onward\) or .NET web services/remoting \(mainly used in earlier versions\).

![H&#xEC;nh 6-4: Diagram c&#x1EE7;a m&#x1ED9;t Remote Proxy &#x111;&#x1A1;n gi&#x1EA3;n](../../.gitbook/assets/img-6-4.png)

**Khi nào thì sử dụng Virtual Proxy?**

Sử dụng Virtual Proxy để tránh việc load một tấm ảnh cưc lớn nhiều lần.

**Khi nào thì sử dụng Protection Proxy?**

In an organization, the security team can implement a protection proxy to block Internet access to specific web sites. Consider the following example, which is basically a modified version of the Proxy pattern implementation described earlier. For simplicity, let’s assume you have only three registered users who can exercise the proxy method DoSomeWork\(\). If any other user \(say Robin\) tries to invoke the method, the system will reject those attempts. When the system rejects this kind of unwanted access, there is no point in making a proxy object. So, if you avoid instantiating an object of ConcreteSubject in the proxy class constructor, you can easily avoid creating objects unnecessarily. Now let’s go through the modified implementation

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

**Có vẻ như Proxy hoạt động giống Decorator, phải hem?**

A protection proxy might be implemented like a decorator, but you should not forget the intent of a proxy. Decorators focus on adding responsibilities, but proxies focus on controlling the access to an object. Proxies differ from each other through their types and implementations. So, if you can remember their purposes, in most cases you will be able to clearly distinguish proxies from decorators.

## Tham khảo thêm

* [https://gpcoder.com/4644-huong-dan-java-design-pattern-proxy/](https://gpcoder.com/4644-huong-dan-java-design-pattern-proxy/)
* [https://www.stdio.vn/articles/design-pattern-proxy-pattern-601](https://www.stdio.vn/articles/design-pattern-proxy-pattern-601)
* [https://appstechviet.wordpress.com/2016/08/23/design-pattern-the-proxy-pattern/](https://appstechviet.wordpress.com/2016/08/23/design-pattern-the-proxy-pattern/)
* [https://nixforest.wordpress.com/2010/12/06/design-pattern-proxy/](https://nixforest.wordpress.com/2010/12/06/design-pattern-proxy/)

