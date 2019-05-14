# Chapter 4: Factory Method Pattern

## GoF Definition

Định nghĩa một giao diện \(interface\) cho việc tạo một đối tượng, nhưng để các lớp con quyết định lớp nào sẽ được tạo.

{% hint style="info" %}
**Note:** Để hiểu mẫu thiết kế này, tôi đề nghị bạn tham khảo [Chapter 24: Simple Factory pattern](../../part-ii-additional-design-patterns/chapter-24-simple-factory-pattern.md). Mẫu thiết kế _`Simple Factory`_ không thuộc các mẫu thiết kế của GOF, cho nên việc thảo luận về mẫu _`Simple Factory`_ sẽ được để trong phần 2 của tài liệu này. Mẫu _`Factory Method`_ sẽ rõ nghĩa hơn nếu bạn đã hiểu điểm mạnh điểm yếu của mẫu _`Simple Factory`_ 
{% endhint %}

## Khái niệm

Tốt hơn là mô tả khái niệm của mẫu này thông qua ví dụ, nhưng đại khái có thể hiểu như vầy: 

> Factory Method định nghĩa một method cho việc tạo đối tượng, và các lớp con hoặc là kế thừa hoặc là override để chỉ rõ đối tượng nào sẽ được tạo và tạo như thế nào.

## Ví dụ thực tế

Trong một nhà hàng, dựa vào sở thích, khẩu vị, yêu cầu của từng khách hàng mà đầu bếp sẽ thay đổi nguyên liệu/hương vị của món ăn để phục vụ thực khách đó.

## Ví dụ chuyên ngành

Trong một ứng dụng, rất có thể bạn sử dụng nhiều loại database khác nhau cho người dùng khác nhau, ví dụ như một người thì dùng Oracle, người khác bạn lại dùng Sql Server. Bất cứ khi nào bạn cần thêm data vào database, bạn cần tạo ra hoặc là một _`SqlConnection`_ hoặc là một _`OracleConnection`_. Nếu bạn đặt code của mình vào câu lệnh _`if...else...`_ \(hoặc _`switch`_\), bạn sẽ phải viết rất nhiều code lặp đi lặp lại, điều này không dễ để maintain \(bảo trì\). Giả sử sau này bạn cần làm việc với một kiểu database khác ví dụ MySql, lúc đó sẽ là thảm họa. Mẫu thiết kế _`Factory Method`_ này sẽ giúp bạn giải quyết kiểu vấn đề như vậy. Ở đây chúng ta sẽ dùng một lớp trừu tượng \(_`IAnimalFactory`_ 😂 \) để định nghĩa một kiến trúc đơn giản. Theo định nghĩa, quá trình khởi tạo sẽ được thực hiện thông qua các lớp con được dẫn xuất từ lớp trừu tượng này.

## Minh họa và giải thích

### Class Diagram

![H&#xEC;nh 4-1. Class diagram](../../.gitbook/assets/img-4-1.png)

### Directed Graph Document

![H&#xEC;nh 4-2. Directed Graph Document](../../.gitbook/assets/img-4-2.png)

### Solution Explorer View

![H&#xEC;nh 4.3. Solution Explorer View](../../.gitbook/assets/img-4-3.png)

### Viết code thôi!

```csharp
using System;
namespace FactoryMethodPattern
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
            Console.WriteLine("Dogs prefer barking...\n");
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
            Console.WriteLine("Tigers prefer hunting...\n");
        }
    }

    public abstract class IAnimalFactory
    {
        //Remember the GoF definition which says "....Factory method lets a class
        //defer instantiation to subclasses." Following method will create a Tiger
        //or Dog But at this point it does not know whether it will get a Dog or a
        //Tiger. It will be decided by the subclasses i.e.DogFactory or TigerFactory.
        //So, the following method is acting like a factory (of creation).
        public abstract IAnimal CreateAnimal();
    }

    public class DogFactory : IAnimalFactory
    {
        public override IAnimal CreateAnimal()
        {
            //Creating a Dog
            return new Dog();
        }
    }

    public class TigerFactory : IAnimalFactory
    {
        public override IAnimal CreateAnimal()
        {
            //Creating a Tiger
            return new Tiger();
        }
    }

    class Client
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***Factory Pattern Demo***\n");
            // Creating a Tiger Factory
            IAnimalFactory tigerFactory = new TigerFactory();
            // Creating a tiger using the Factory Method
            IAnimal aTiger = tigerFactory.CreateAnimal();
            aTiger.Speak();
            aTiger.Action();
            // Creating a DogFactory
            IAnimalFactory dogFactory = new DogFactory();
            // Creating a dog using the Factory Method
            IAnimal aDog = dogFactory.CreateAnimal();
            aDog.Speak();
            aDog.Action();
            Console.ReadKey();
        }
    }
}
```

### Output

```text
***Factory Pattern Demo***

Tiger says: Halum.
Tigers prefer hunting...

Dog says: Bow-Wow.
Dogs prefer barking...
```

### Thực hiện sửa đổi

Trong lần sửa đổi này, chúng ta sẽ làm cho đoạn code tăng tính linh hoạt. Chú ý là, class _`IAnimalFactory`_ là một lớp trừu tượng \(abstract\), do đó bạn có thể tận dụng mọi lợi thế của nó. Giả sử rằng bạn muốn một lớp con tuân theo qui tắc mà lớp cha \( hoặc _`base class`_ - lớp cơ sở \) đã qui định. Tôi đã thử code cho tình huống như vậy, dưới đây là những ý chính:

* Chỉ _`AnimalFactory`_ được sửa đổi, nói cách khác, tôi đang đề cập đến một phương thức mới gọi là _`MakeAnimal()`_ .

  ```csharp
  //Modifying the IAnimalFactory class.
  public abstract class IAnimalFactory
  {
      public IAnimal MakeAnimal()
      {
          Console.WriteLine("\n IAnimalFactory.MakeAnimal() " +
                      "-You cannot ignore parent rules.");
         
          /*
          Tới thời điểm này, nó chả biết khi nào sẽ là một con Dog hay một con Tiger.
          Nó sẽ được quyết định bởi các lớp con, ví dụ DogFactory hoặc TigerFactory. 
          Nhưng nó biết là nó sẽ thực hiện 2 hành động Speak() và Action()
          */

          IAnimal animal = CreateAnimal();
          animal.Speak();
          animal.Action();
        
          return animal;
      }

      //So, the following method is acting like a factory
      //(of creation).
      public abstract IAnimal CreateAnimal();
  }
  ```

* Tôi cũng thay đổi một chú code client \( _`Main()`_ method \):

  ```csharp
  class Client
  {
      static void Main(string[] args)
      {
          Console.WriteLine("***Beautification to Factory Pattern Demo * **\n");

          // Creating a tiger using the Factory Method
          IAnimalFactory tigerFactory = new TigerFactory();
          IAnimal aTiger = tigerFactory.MakeAnimal();

          //IAnimal aTiger = tigerFactory.CreateAnimal();
          //aTiger.Speak();
          //aTiger.Action();

          // Creating a dog using the Factory Method
          IAnimalFactory dogFactory = new DogFactory();
          IAnimal aDog = dogFactory.MakeAnimal();

          //IAnimal aDog = dogFactory.CreateAnimal();
          //aDog.Speak();
          //aDog.Action();

          Console.ReadKey();
      }
  }
  ```

### Kết quả sau khi sửa đổi

```text
***Beautification to Factory Pattern Demo***

AnimalFactory.MakeAnimal()-You cannot ignore parent rules.
Tiger says: Halum.
Tigers prefer hunting...

AnimalFactory.MakeAnimal()-You cannot ignore parent rules.
Dog says: Bow-Wow.
Dogs prefer barking...
```

### Phân tích

Chú ý rằng trong mỗi trường hợp bạn sẽ thấy cảnh bảo như sau:_`"…You cannot ignore parent rules."`_  
**Giải thích**: Ý chỗ này nó muốn nói các lớp con sẽ luôn chạy những dòng code trong  _`IAnimalFactory.MakeAnimal()`_, tức là sẽ gọi lệnh _`animal.Speak()`_ và _`animal.Action()`_

## Hỏi & Đáp

**Tại sao tách phương thức CreateAnimal\(\) ra khỏi client \(Main.cs\)?**

**Lợi ích của việc sử dụng mẫu Factory Method?**

**Sử dụng mẫu này có những thử thách/khó khăn gì không?**

**Tôi thấy rằng mẫu Factory Method này đang hỗ trợ 2 phân lớp song song, cách hiểu này có đúng không?**

**Bạn nên luôn luôn chỉ định một factory method bằng từ khóa** _**`abstract`**_ **để các lớp con có thể hoàn thiện nó, đúng không?**

**Tôi vẫn thấy rằng mẫu** _**`Factory Method`**_ **vẫn chả có gì khác biệt nhiều so với mẫu** _**`Simple Factory`**_**, đúng chứ hỉ?**

{% hint style="warning" %}
Còn tiếp...
{% endhint %}





