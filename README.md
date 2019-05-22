# Design Patterns in C\# - Tiếng Việt

{% hint style="warning" %}
LƯU Ý  
TÀI LIỆU NÀY CHƯA HOÀN THÀNH, VẪN ĐANG DỊCH SẤP MẶT
{% endhint %}

_**Gitbook**_:       [https://minhhungit.gitbook.io/designpatternsincsharptiengviet](https://minhhungit.gitbook.io/designpatternsincsharptiengviet)  
_**Repository**_:  [https://github.com/minhhungit/designpatternsincsharptiengviet](https://github.com/minhhungit/designpatternsincsharptiengviet)



Project này là bản dịch các Design Patterns \(mẫu thiết kế\) dựa trên khung sườn của cuốn sách [**Design Patterns in C\#**](https://www.apress.com/gp/book/9781484236390) - tác giả **Vaskaran Sarcar**, với mong muốn giúp các bạn developer .NET Việt Nam \(hoặc thậm chí ngôn ngữ khác, nước khác ^^ \) tiếp cận dễ dàng hơn các mẫu thiết kế.

> **Lưu ý:** việc dịch dựa trên sách **Design Patterns in C\#** này không có nghĩa nội dung được dịch hoàn toàn 100%, _**sách cũng chỉ được sử dụng với mục đích tham khảo**_, người dịch có thể thêm thắt, lượt bỏ, đưa thêm ví dụ, viện dẫn các nguồn, bài viết khác nhau nhằm mục đích giải thích cho người đọc hiểu được lý do tại sao mẫu thiết kế được tạo ra, làm thế nào để sử dụng cũng như nên sử dụng trong những trường hợp cụ thể nào.

Mọi đóng góp dù lớn hay nhỏ cũng đều được hoan nghênh.

Để tham gia đóng góp bản dịch cho các mẫu thiết kế, các bạn chỉ cần làm theo các bước sau:

* Đăng ký chương mà bạn muốn dịch [tại đây](https://github.com/minhhungit/designpatternsincsharptiengviet/issues/1) nhằm tránh trùng lặp \(nhiều người cùng dịch\)
* Fork repo này
* Update  file .md create và pull request để merge

## Quy ước đóng góp

Vì project này chỉ mới được bắt đầu, chúng ta sẽ tập trung vào các chương chưa dịch thay vì chỉnh sửa các /chương đã dịch xong \(tuy vậy, nếu có sai sót gì nghiêm trọng ở các chương đã dịch, rất hoan nghênh sự đóng góp của các bạn\). Để bắt đầu dịch một chương mới, vui lòng thực hiện theo quy ước sau:

* Chỉ dịch một chương cho mỗi PR \(pull request\)
* Trước khi bắt tay vào dịch một chương, hãy kiểm tra xem chương đó có ai đã hoặc đang dịch hay không, xem [tại đây](https://github.com/minhhungit/designpatternsincsharptiengviet/issues/1).
* Tạo một branch mới từ master với tên là chương bạn muốn dịch, ví dụ `chuong-02-abc-xyz`
* Tạo một thay đổi nhỏ để submit PR vào master, với tên PR là \[WIP\] Chương 02: ABC XYZ \(WIP nghĩa là Work In Progress - đang dịch\)
* Tiến hành dịch dưới local và push commit vào PR này
* Sau khi phần dịch đã hoàn tất, xóa cụm \[WIP\] khỏi tên PR và tag một hay vài thành viên chính vào PR

## Lưu ý khi dịch

Do sự khác biệt về ngôn ngữ và những đặc thù của tiếng Việt, sẽ có những khó khăn nhất định khi dịch. Vì vậy, có một số lưu ý như sau:

* Không nhất thiết phải dịch toàn bộ từ hoặc cụm từ sang tiếng Việt, nhất là những từ chuyên ngành \(framework, file, component…\) Đối với những từ dịch được nhưng nghe lạ tai trong tiếng Việt, hãy dùng quy ước tiếng Anh \(nghĩa tiếng Việt\) cho lần đầu tiên, và dùng tiếng Anh từ đó về sau. Ví dụ:

  > Singleton pattern \(mẫu thiết kế Singleton\) là một trong những mẫu thiết kế đơn giản nhất. Singleton pattern được xếp vào nhóm Creational patterns \(Nhóm Khởi tạo\), cung cấp một trong những cách tốt nhất để tạo object...

* Đối với những từ hoặc cụm từ hoàn toàn không có từ ngữ tương đương trong tiếng Việt, hãy thông báo để chúng ta cùng thảo luận.
* Không nhất thiết phải dịch 1:1. Đôi khi chúng ta nên thêm hoặc bớt vài từ hoặc thậm chí là cả một câu để bản dịch được tự nhiên hơn, miễn là truyền tải được đúng và đủ ý.

## Tiến Trình

Các chương đã dịch xong

* [Chapter 1: Singleton Pattern](https://minhhungit.gitbook.io/designpatternsincsharptiengviet/part-i-gang-of-four-design-patterns/i-a-creational-patterns/chapter-01-singleton-pattern)
* [Chapter 2: Prototype Pattern](https://minhhungit.gitbook.io/designpatternsincsharptiengviet/part-i-gang-of-four-design-patterns/i-a-creational-patterns/chapter-02-prototype-pattern)
* [Chapter 4: Factory Method Pattern](https://minhhungit.gitbook.io/designpatternsincsharptiengviet/part-i-gang-of-four-design-patterns/i-a-creational-patterns/chapter-04-factory-method-pattern)
* [Chapter 5: Abstract Factory Pattern](https://minhhungit.gitbook.io/designpatternsincsharptiengviet/part-i-gang-of-four-design-patterns/i-a-creational-patterns/chapter-05-abstract-factory-pattern)
* [Chapter 6: Proxy Pattern](https://minhhungit.gitbook.io/designpatternsincsharptiengviet/part-i-gang-of-four-design-patterns/i-b-structural-patterns/chapter-06-proxy-pattern)
* [Chapter 24: Simple Factory Pattern](https://minhhungit.gitbook.io/designpatternsincsharptiengviet/part-ii-additional-design-patterns/chapter-24-simple-factory-pattern)

Chi tiết hơn các bác có thể check [tại đây](https://github.com/minhhungit/designpatternsincsharptiengviet/issues/1#issue-442682612)

## Lưu ý trước khi đọc

Nhớ là tài liệu này chỉ cố gắng mô tả các design pattern ở mức độ nhập môn, sơ khai và cơ bản nhất. Việc đi từ đơn giản đến phức tạp sẽ có nhiều lợi ích và phù hợp cho những bạn mới, còn đối với những bạn đã có ít nhiều hiểu biết về design patterns, tài liệu này có thể sẽ quá đơn giản, hoặc thậm chí theo bạn là thiếu sót, do vậy, trường hợp này bạn sẽ cần phải tự nghiên cứu, tìm hiểu thêm từ các nguồn khác. Thêm nữa, vì là bản dịch nên sẽ có một số khác biệt về ngôn ngữ, nên tài liệu này cũng cố gắng đưa vào một số đường link để bạn tìm hiểu thêm cũng như đối chiếu với các bản dịch khác, từ đó giúp bạn hiểu rõ hơn, bạn sẽ định hình được, đi đúng hướng và ... lương cao hơn 😘 

## Thành viên

> Đang cập nhật...

