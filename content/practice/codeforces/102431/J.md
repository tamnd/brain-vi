---
title: "CF 102431J - Bộ đệm giao thức tương thích với dây"
description: "Thông báo protobuf là một chuỗi các trường được mã hóa. Tên trường không bao giờ xuất hiện trên dây. Thứ xác định một trường là thẻ số của nó và loại dây cho bộ giải mã biết có bao nhiêu byte thuộc về trường đó."
date: "2026-08-09T00:07:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "J"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 842
verified: true
draft: false
---

[CF 102431J - Bộ đệm giao thức tương thích với dây](https://codeforces.com/problemset/problem/102431/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thông báo protobuf là một chuỗi các trường được mã hóa. Tên trường không bao giờ xuất hiện trên dây. Thứ xác định một trường là thẻ số của nó và loại dây cho bộ giải mã biết có bao nhiêu byte thuộc về trường đó. Trong vấn đề đơn giản hóa này, một`double`sử dụng dây loại 1, trong khi cả một`string`và một tin nhắn nhúng sử dụng loại dây 2. 

Để hai bộ mô tả thông báo tương thích với định dạng dây, chúng phải chấp nhận chính xác các loại chuỗi trường được xê-ri hóa giống nhau. Điều đó ngay lập tức mang lại cho chúng tôi một số yêu cầu địa phương. Trường có thẻ cụ thể phải tồn tại trong cả hai tin nhắn. Quy tắc của nó phải phù hợp, bởi vì`required`,`optional`, Và`repeated`cho phép số lần xuất hiện khác nhau. Loại cấp dây của nó cũng phải phù hợp. MỘT`double`không thể thay thế bằng một chuỗi hoặc một tin nhắn và một chuỗi không thể được thay thế bằng một tin nhắn nhúng chỉ vì cả hai đều sử dụng loại dây 2. 

Đối với các tin nhắn được nhúng, có thêm một điều kiện nữa. Giả sử cả hai thông báo đều có trường tùy chọn được đánh số 3 có loại là một thông báo khác. Hai tin nhắn bên ngoài chỉ tương thích khi hai loại tin nhắn lồng nhau tương thích với nhau. Điều này tạo ra một biểu đồ đệ quy về các yêu cầu tương thích. Biểu đồ có thể chứa các chu trình, như được hiển thị bằng thông báo chứa trường tùy chọn thuộc loại riêng của nó. 

Bộ mô tả được đưa ra dưới dạng một số dòng văn bản, theo sau là tối đa 50.000 truy vấn. Có tối đa 1.000 loại tin nhắn và mỗi tin nhắn chứa tối đa 16 trường. Số lượng tin nhắn nhỏ là hạn chế chính. Nó cho phép chúng tôi dành thời gian gần như bậc hai cho số lượng tin nhắn, nhưng số lượng truy vấn lớn có nghĩa là chúng tôi không thể thực hiện một phép so sánh đệ quy mới tốn kém cho mỗi truy vấn. 

Một cách hữu ích để xem tin nhắn là xem một nút đồ thị có hướng được gắn nhãn. Mỗi trường là một cạnh đi được gắn nhãn theo thẻ, quy tắc và loại cấp độ dây của nó. Đối với các trường nguyên thủy, cạnh kết thúc ở loại đầu cuối cố định, chẳng hạn như`string`hoặc`double`; đối với một trường tin nhắn, nó kết thúc ở một nút tin nhắn khác. Hai nút thông báo tương thích chính xác khi các cạnh được gắn nhãn đầu ra của chúng khớp nhau và các cạnh thông báo tương ứng dẫn đến các nút tương thích. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xem xét các tên trường khác nhau có cùng một thẻ:```
message A {
optional string first = 1 ;
}
message B {
optional string second = 1 ;
}
2
A B
```Đầu ra đúng là:```
Wire-format compatible.
```Một giải pháp bất cẩn so sánh tên trường sẽ từ chối cặp này, nhưng tên trường không được tuần tự hóa. 

Bây giờ hãy xem xét cùng một tên trường với các thẻ khác nhau:```
message A {
optional string value = 1 ;
}
message B {
optional string value = 2 ;
}
1
A B
```Đầu ra đúng là:```
Wire-format incompatible.
```Bộ giải mã tra cứu các trường bằng thẻ số, vì vậy các tên giống nhau không giúp ích được gì. 

Quy tắc trường cũng có vấn đề:```
message A {
optional string value = 1 ;
}
message B {
repeated string value = 1 ;
}
1
A B
```Đầu ra đúng là:```
Wire-format incompatible.
```Trường lặp lại có thể xuất hiện nhiều lần, trong khi trường tùy chọn có thể xuất hiện nhiều nhất một lần. Bộ giải mã không thể hiểu mọi phiên bản hợp lệ của một lược đồ này là phiên bản hợp lệ của lược đồ kia. 

Cuối cùng, các tin nhắn nhúng không thể đơn giản được coi là chuỗi vì cả hai đều sử dụng loại dây 2:```
message Empty {
}
message Holder {
optional Empty value = 1 ;
}
message Text {
optional string value = 1 ;
}
1
Holder Text
```Đầu ra đúng là:```
Wire-format incompatible.
```Một chuỗi có thể chứa dữ liệu UTF-8 hợp lệ tùy ý, trong khi tải trọng của`Empty`phải được đăng nhiều kỳ`Empty`tin nhắn, bị hạn chế hơn nhiều. 

Giới hạn 1.000 tin nhắn và 16 trường trên mỗi tin nhắn làm cho thuật toán tiền xử lý (O(M^2F)) trở nên thực tế, trong đó (M) là số lượng tin nhắn và (F) là số lượng trường tối đa. Ở kích thước tối đa, con số này là khoảng 16 triệu thao tác cấp trường cho mỗi cấu trúc vượt qua sàng lọc, thay vì thực hiện công việc tương đương riêng biệt cho 50.000 truy vấn. Giới hạn 50.000 truy vấn chính xác là điều quy định việc giải quyết từng truy vấn một cách độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là so sánh đệ quy hai loại thông báo. Trước tiên hãy so sánh các tập hợp số trường, quy tắc và kiểu nguyên thủy của chúng. Bất cứ khi nào cả hai bên đều chứa trường thông báo, hãy so sánh đệ quy hai loại thông báo được tham chiếu. Bảng ghi nhớ cho các cặp thông báo sẽ ngăn chặn đệ quy vô hạn khi bộ mô tả chứa các chu trình. 

Điều này đúng vì tính tương thích của tin nhắn hoàn toàn được xác định bởi tính tương thích của các trường tương ứng. Tuy nhiên, trạng thái so sánh như vậy là một cặp loại thông báo, do đó có thể có tới (M^2) trạng thái khác nhau. Với (M=1000), một truy vấn khó có thể truy cập tới một triệu cặp tin nhắn. Với tối đa 16 trường trên mỗi cặp, điều đó có thể yêu cầu khoảng 16 triệu so sánh trường. Việc lặp lại điều này cho 50.000 truy vấn sẽ gây ra trường hợp xấu nhất là khoảng 800 tỷ lượt kiểm tra trường. 

Phương pháp brute-force hoạt động vì mối quan hệ tương thích là đệ quy, nhưng nó không thành công vì cùng một cặp loại thông báo có thể được khám phá lại một cách độc lập cho nhiều truy vấn. Quan sát mở ra giải pháp nhanh hơn là tính tương thích là mối quan hệ tương đương được xác định bởi vùng lân cận được gắn nhãn đầy đủ của mọi thông báo. Chúng ta có thể tính toán tất cả các lớp tương đương một lần. 

Bắt đầu bằng cách đưa các tin nhắn vào các lớp theo cấu trúc cấp dây trực tiếp của chúng. Một trường đóng góp thẻ, quy tắc của nó và liệu giá trị của nó có phải là một`double`, Một`string`, hoặc một tin nhắn khác. Đối với trường có giá trị thông báo, danh tính của mục tiêu tạm thời bị bỏ qua. 

Sau đó liên tục tinh chỉnh các lớp học. Khi một trường trỏ đến một thông báo khác, hãy thay thế thông báo đích bằng số lớp hiện tại của nó. Hai thông báo chỉ ở cùng một lớp nếu các trường của chúng có các thẻ và quy tắc giống nhau, các kiểu nguyên thủy giống hệt nhau và các trường thông báo tương ứng của chúng trỏ đến cùng các lớp hiện tại. 

Quá trình này là sàng lọc phân vùng. Mỗi lần lặp lại chỉ có thể tách các lớp, không bao giờ hợp nhất hai tin nhắn đã được tách ra. Chỉ có (M) thông báo, vì vậy sau tối đa (M-1) sàng lọc nghiêm ngặt, phân vùng phải ổn định. Tại thời điểm đó, hai tin nhắn có cùng một lớp khi chúng tương thích với định dạng dây. Các định nghĩa tuần hoàn được xử lý một cách tự nhiên vì mỗi lần lặp chỉ sử dụng các lớp được tính toán trong lần lặp trước đó. 

Với 1.000 thông báo và nhiều nhất là 16 trường mỗi thông báo, ngay cả việc triển khai sàng lọc đơn giản cũng đủ nhanh. Sau khi tiền xử lý, mọi truy vấn chỉ là sự so sánh của hai mã định danh lớp số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(QM^2F)) trường hợp xấu nhất với ghi nhớ cho mỗi truy vấn | (O(M^2)) mỗi truy vấn | Quá chậm | 
| Tối ưu | (O(M^2F)) trường hợp xấu nhất | (O(MF)) | Đã chấp nhận | 

Ở đây (M) là số lượng tin nhắn, (F) là số trường tối đa trong một tin nhắn và (Q) là số lượng truy vấn. 

## Hướng dẫn thuật toán 

1. Phân tích từng tin nhắn và gán cho nó một ID số nguyên. Đối với mỗi trường, hãy lưu trữ thẻ, quy tắc và loại của trường đó. Nếu loại là một tin nhắn khác, hãy lưu ID tin nhắn được tham chiếu. 
2. Trình bày mỗi tin nhắn dưới dạng các trường được sắp xếp theo thẻ. Bộ mô tả nguồn có thể liệt kê các trường theo bất kỳ thứ tự nào, nhưng thứ tự trường không có ý nghĩa trong việc tuần tự hóa protobuf, vì vậy việc sắp xếp theo thẻ sẽ cho chúng ta một thứ tự cục bộ chuẩn. 
3. Cung cấp cho mọi tin nhắn cùng một lớp ban đầu. Lúc đầu, chúng tôi cố tình bỏ qua danh tính của các loại tin nhắn lồng nhau. Tinh chỉnh đầu tiên sẽ phân biệt các thông báo bằng cách sử dụng mọi thứ có thể được xác định cục bộ. 
4. Xây dựng chữ ký cho mọi tin nhắn bằng các lớp hiện tại. Đối với trường nguyên thủy, chữ ký chứa thẻ, quy tắc và loại nguyên thủy. Đối với trường thông báo, nó chứa thẻ, quy tắc, điểm đánh dấu cho biết đó là thông báo và lớp hiện tại của mục tiêu. 
5. Gán các ID số nguyên bằng nhau cho các chữ ký bằng nhau. Những ID này tạo thành phân vùng mới của tin nhắn. Nếu hai thông báo có chữ ký khác nhau thì chúng không thể tương thích vì một số hành vi của dây ở cấp trường khác nhau. Nếu họ có cùng chữ ký, họ vẫn là ứng cử viên cho khả năng tương thích. 
6. Lặp lại việc xây dựng chữ ký cho đến khi nhiệm vụ của lớp ngừng thay đổi. Mỗi sàng lọc sẽ tách ít nhất một cặp bằng nhau trước đó hoặc đạt đến một điểm cố định. Vì chỉ có (M) thông báo nên có thể có nhiều nhất (M-1) sàng lọc nghiêm ngặt. 
7. Đối với mỗi truy vấn, hãy tra cứu ID lớp của hai tên thông báo của nó. ID bằng nhau có nghĩa là các tin nhắn có hành vi dây được xác định đệ quy giống hệt nhau, vì vậy hãy in`Wire-format compatible.`Nếu không thì in`Wire-format incompatible.`### Tại sao nó hoạt động 

Điều bất biến là sau mỗi lần sàng lọc, các thông báo ở các lớp khác nhau không thể tương thích với định dạng dây. Sự khác biệt về thẻ, quy tắc, loại dây nguyên thủy hoặc lớp hiện tại của thông báo lồng nhau sẽ tạo ra sự khác biệt cụ thể về dữ liệu được tuần tự hóa mà hai bộ mô tả có thể chấp nhận. 

Khi quá trình ổn định, mọi cặp thông báo trong cùng một lớp đều có các trường khớp và mọi trường thông báo lồng nhau tương ứng đều trỏ đến cùng một lớp ổn định. Do đó, hai thông báo thỏa mãn chính xác các điều kiện tương thích đệ quy giống nhau. Ngược lại, hai thông báo tương thích không bao giờ có thể được phân tách bằng một sàng lọc vì các trường tương thích phải có cùng thẻ, quy tắc, loại nguyên thủy hoặc mục tiêu lồng nhau tương thích. Do đó, các lớp ổn định chính xác là các lớp tương thích định dạng dây. 

Việc sử dụng các lớp lặp trước đó là điều làm cho chu trình trở nên an toàn. Ví dụ, nếu`A`chứa một tùy chọn`A`, chữ ký của nó đề cập đến lớp của`A`từ lần lặp trước. Nó không yêu cầu mở rộng đệ quy tin nhắn mãi mãi. Một nhóm đệ quy lẫn nhau như`B -> C -> B`do đó có thể xếp vào cùng một lớp khi cấu trúc dây quan sát được của chúng tương đương nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    tokens = []

    for _ in range(n):
        tokens.extend(input().split())

    pos = 0
    messages = []
    name_to_id = {}

    while pos < len(tokens):
        assert tokens[pos] == "message"
        pos += 1

        name = tokens[pos]
        pos += 1

        mid = len(messages)
        name_to_id[name] = mid
        messages.append({
            "name": name,
            "fields": []
        })

        assert tokens[pos] == "{"
        pos += 1

        while tokens[pos] != "}":
            label = tokens[pos]
            typ = tokens[pos + 1]
            field_name = tokens[pos + 2]
            assert tokens[pos + 3] == "="
            tag = int(tokens[pos + 4])
            assert tokens[pos + 5] == ";"
            pos += 6

            messages[mid]["fields"].append(
                [tag, label, typ]
            )

        pos += 1

    # Resolve message type names to integer IDs.
    for msg in messages:
        fields = []
        for tag, label, typ in msg["fields"]:
            if typ == "double":
                fields.append((tag, label, 0, -1))
            elif typ == "string":
                fields.append((tag, label, 1, -1))
            else:
                fields.append((tag, label, 2, name_to_id[typ]))

        fields.sort(key=lambda x: x[0])
        msg["fields"] = fields

    m = len(messages)

    # Initially all messages are in one class.
    cls = [0] * m

    while True:
        signatures = []

        for msg in messages:
            sig = []

            for tag, label, kind, target in msg["fields"]:
                if kind == 2:
                    sig.append((tag, label, kind, cls[target]))
                else:
                    sig.append((tag, label, kind))

            signatures.append(tuple(sig))

        ids = {}
        new_cls = [0] * m

        for i, sig in enumerate(signatures):
            if sig not in ids:
                ids[sig] = len(ids)
            new_cls[i] = ids[sig]

        if new_cls == cls:
            break

        cls = new_cls

    q = int(input())
    out = []

    for _ in range(q):
        a, b = input().split()

        if cls[name_to_id[a]] == cls[name_to_id[b]]:
            out.append("Wire-format compatible.")
        else:
            out.append("Wire-format incompatible.")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Trước tiên, trình phân tích cú pháp sẽ đọc tất cả các mã thông báo mô tả từ mã đầu tiên.`n`dòng. Điều này thuận tiện vì dấu ngoặc nhọn, dấu chấm phẩy, tên trường và tên loại đã được phân tách bằng dấu cách theo định dạng đầu vào. Do đó, một trường luôn chiếm đúng sáu mã thông báo sau khi nhãn của nó bắt đầu. 

Biểu diễn đầu tiên giữ tên loại thông báo dưới dạng chuỗi để tất cả tên thông báo có thể được đăng ký trước khi giải quyết các tham chiếu. Sau khi phân tích cú pháp hoàn tất, mọi tham chiếu tin nhắn sẽ được chuyển đổi thành ID số nguyên. Điều này tránh việc tra cứu từ điển trong vòng lặp sàng lọc. 

Các trường được sắp xếp theo thẻ. Điều này là cần thiết vì thứ tự mô tả không ảnh hưởng đến định dạng dây. Nếu không sắp xếp, hai tin nhắn giống hệt nhau có thể nhận được chữ ký khác nhau chỉ vì các khai báo của chúng xuất hiện theo thứ tự khác nhau. 

số nguyên`kind`phân biệt ba loại giá trị trường có thể.`0`đại diện cho`double`,`1`đại diện cho`string`, Và`2`đại diện cho một tin nhắn được nhúng. Trường thông báo bao gồm lớp mục tiêu hiện tại của nó, trong khi các trường nguyên thủy không có lớp mục tiêu. 

Vòng lặp sàng lọc bắt đầu với mọi thông báo trong lớp 0. Trên mỗi lần lặp, mọi thông báo đều nhận được một chữ ký hoàn chỉnh mô tả cấu trúc có thể quan sát được hiện tại của nó. Chữ ký bằng nhau nhận được ID lớp bằng nhau. Nếu mảng lớp mới giống hệt mảng trước đó thì phân vùng đã đạt đến điểm cố định. 

Sự so sánh`new_cls == cls`an toàn vì ID lớp được chỉ định một cách xác định bằng cách quét các tin nhắn theo thứ tự ID. Nếu phân vùng không thay đổi, chữ ký tương ứng cũng không thay đổi, do đó lần lặp khác không thể tinh chỉnh bất cứ điều gì. 

Không có vấn đề tràn số nguyên trong Python. Số trường có thể lớn tới 536.870.911, có thể dễ dàng xử lý bằng số nguyên Python. Số trường tối đa chỉ là 16 nên mỗi chữ ký vẫn nhỏ. 

Giai đoạn truy vấn có chủ ý là tầm thường. Tất cả công việc đệ quy đã được thực hiện trong quá trình tiền xử lý, vì vậy mỗi truy vấn trong số tối đa 50.000 truy vấn đều mất thời gian không đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các cấu trúc tin nhắn có liên quan là:```
Test1:       tag 1, optional, string
Test2:       tag 1, optional, string
Test3:       tag 2, optional, string
Test4:       tag 1, required, string
StringMessage: tag 1, optional, string
Test5:       tag 1, optional, message StringMessage
```Trạng thái sàng lọc có thể được tóm tắt như sau. ID lớp số chính xác phụ thuộc vào thứ tự chữ ký được gán, nhưng sự bình đẳng giữa các ID mới là điều quan trọng. 

| Lặp lại | Tin nhắn | Hình chữ ký | Lớp | 
| --- | --- | --- | --- | 
| Ban đầu | Kiểm tra1 | tất cả các tin nhắn ban đầu bằng nhau | 0 | 
| Ban đầu | Kiểm tra2 | tất cả các tin nhắn ban đầu bằng nhau | 0 | 
| Ban đầu | Kiểm tra3 | tất cả các tin nhắn ban đầu bằng nhau | 0 | 
| Ban đầu | Kiểm tra4 | tất cả các tin nhắn ban đầu bằng nhau | 0 | 
| Ban đầu | StringMessage | tất cả các tin nhắn ban đầu bằng nhau | 0 | 
| Ban đầu | Kiểm tra5 | tất cả các tin nhắn ban đầu bằng nhau | 0 | 
| 1 | Kiểm tra1 |`(1, optional, string)`| 0 | 
| 1 | Kiểm tra2 |`(1, optional, string)`| 0 | 
| 1 | Kiểm tra3 |`(2, optional, string)`| 1 | 
| 1 | Kiểm tra4 |`(1, required, string)`| 2 | 
| 1 | StringMessage |`(1, optional, string)`| 0 | 
| 1 | Kiểm tra5 |`(1, optional, message, class(Test1))`| 3 | 
| 2 | Kiểm tra1 | không thay đổi | 0 | 
| 2 | Kiểm tra2 | không thay đổi | 0 | 
| 2 | Kiểm tra3 | không thay đổi | 1 | 
| 2 | Kiểm tra4 | không thay đổi | 2 | 
| 2 | StringMessage | không thay đổi | 0 | 
| 2 | Kiểm tra5 | không thay đổi | 3 | 

Do đó, các truy vấn đưa ra`Test1`Và`Test2`cùng một lớp, trong khi`Test3`,`Test4`, Và`Test5`mỗi cái khác nhau. Truy vấn đầu tiên tương thích ngay cả khi tên trường khác nhau vì tên không bao giờ xuất hiện trong chữ ký. 

### Mẫu 2 

Ở đây cấu trúc lồng nhau là:```
A -> B -> C
D -> E
C and E are empty
```Quá trình sàng lọc diễn ra như sau. 

| Lặp lại | Tin nhắn | Chữ ký trường | Lớp | 
| --- | --- | --- | --- | 
| 1 | A |`(1, optional, message, 0)`| 0 | 
| 1 | B |`(1, optional, message, 0)`| 0 | 
| 1 | C | trống | 1 | 
| 1 | D |`(1, optional, message, 0)`| 0 | 
| 1 | E | trống | 1 | 
| 2 | A |`(1, optional, message, 0)`| 0 | 
| 2 | B |`(1, optional, message, 1)`| 1 | 
| 2 | C | trống | 2 | 
| 2 | D |`(1, optional, message, 1)`| 1 | 
| 2 | E | trống | 2 | 
| 3 | A |`(1, optional, message, 1)`| 0 | 
| 3 | B |`(1, optional, message, 2)`| 1 | 
| 3 | C | trống | 2 | 
| 3 | D |`(1, optional, message, 2)`| 1 | 
| 3 | E | trống | 2 |`B`Và`D`cuối cùng nhận được cùng một lớp vì cả hai đều chứa cùng một trường thông báo tùy chọn có mục tiêu là một thông báo trống.`A`khác biệt vì nó được lồng vào nhau`B`không tương đương với tin nhắn trống. 

Điều này đưa ra hai câu trả lời được yêu cầu:```
B D
```tương thích, trong khi```
A D
```không tương thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(M^2F + Q)) | Tối đa (M-1) các sàng lọc nghiêm ngặt, mỗi lần quét (M) thông báo và nhiều nhất là các trường (F), theo sau là các truy vấn có thời gian không đổi | 
| Không gian | (O(MF)) | Bộ mô tả và một chữ ký cho mỗi tin nhắn chứa tối đa các mục nhập trường (MF) | 

Với (M \le 1000) và (F \le 16), giới hạn tiền xử lý nhiều nhất là ở mức 16 triệu mục nhập trường trên tất cả các lần lặp sàng lọc có thể có. Giai đoạn truy vấn xử lý 50.000 truy vấn chỉ với hai lần tra cứu mảng cho mỗi truy vấn, do đó số lượng truy vấn lớn không làm thay đổi chi phí tiền xử lý tiệm cận. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_text(inp: str) -> str:
    data = inp.split()
    p = 0

    n = int(data[p])
    p += 1

    messages = []
    name_to_id = {}

    for _ in range(n):
        assert data[p] == "message"
        p += 1

        name = data[p]
        p += 1

        mid = len(messages)
        name_to_id[name] = mid
        messages.append([])

        assert data[p] == "{"
        p += 1

        while data[p] != "}":
            label = data[p]
            typ = data[p + 1]
            p += 2

            p += 1  # field name

            assert data[p] == "="
            p += 1

            tag = int(data[p])
            p += 1

            assert data[p] == ";"
            p += 1

            messages[mid].append((tag, label, typ))

        p += 1

    for i in range(len(messages)):
        converted = []

        for tag, label, typ in messages[i]:
            if typ == "double":
                converted.append((tag, label, 0, -1))
            elif typ == "string":
                converted.append((tag, label, 1, -1))
            else:
                converted.append((tag, label, 2, name_to_id[typ]))

        converted.sort()
        messages[i] = converted

    m = len(messages)
    cls = [0] * m

    while True:
        groups = {}
        new_cls = [0] * m

        for i in range(m):
            sig = []

            for tag, label, kind, target in messages[i]:
                if kind == 2:
                    sig.append((tag, label, kind, cls[target]))
                else:
                    sig.append((tag, label, kind))

            sig = tuple(sig)

            if sig not in groups:
                groups[sig] = len(groups)

            new_cls[i] = groups[sig]

        if new_cls == cls:
            break

        cls = new_cls

    q = int(data[p])
    p += 1

    ans = []

    for _ in range(q):
        a = data[p]
        b = data[p + 1]
        p += 2

        if cls[name_to_id[a]] == cls[name_to_id[b]]:
            ans.append("Wire-format compatible.")
        else:
            ans.append("Wire-format incompatible.")

    return "\n".join(ans)

def run(inp: str) -> str:
    return solve_text(inp)

sample1 = """18
message Test1 {
optional string field = 1 ;
}
message Test2 {
optional string field_string = 1 ;
}
message Test3 {
optional string field = 2 ;
}
message Test4 {
required string field = 1 ;
}
message StringMessage {
optional string field = 1 ;
}
message Test5 {
optional StringMessage field = 1 ;
}
4
Test1 Test2
Test1 Test3
Test1 Test4
Test1 Test5
"""

assert run(sample1) == """Wire-format compatible.
Wire-format incompatible.
Wire-format incompatible.
Wire-format incompatible.""", "sample 1"

sample2 = """5
message A { optional B nest = 1 ; }
message B { optional C nest = 1 ; }
message C { }
message D { optional E nest = 1 ; }
message E { }
2
B D
A D
"""

assert run(sample2) == """Wire-format compatible.
Wire-format incompatible.""", "sample 2"

sample3 = """1
message A { }
1
A A
"""

assert run(sample3) == """Wire-format compatible.""", "minimum empty message"

sample4 = """3
message A {
optional string x = 536870911 ;
}
message B {
optional string y = 536870911 ;
}
message C {
optional string z = 536870910 ;
}
3
A B
A C
B C
"""

assert run(sample4) == """Wire-format compatible.
Wire-format incompatible.
Wire-format incompatible.""", "maximum field number"

sample5 = """2
message A {
repeated string a = 1 ;
repeated string b = 2 ;
repeated string c = 3 ;
repeated string d = 4 ;
}
message B {
repeated string x = 1 ;
repeated string y = 2 ;
repeated string z = 3 ;
repeated string w = 4 ;
}
2
A B
A A
"""

assert run(sample5) == """Wire-format compatible.
Wire-format compatible.""", "all matching repeated fields"

# A larger generated case, close to the maximum number of messages.
parts = ["1000"]
for i in range(1000):
    parts.append(
        f"message M{i} {{ optional string value = 1 ; }}"
    )
parts.append("3")
parts.append("M0 M999")
parts.append("M123 M456")
parts.append("M0 M0")

large_input = "\n".join(parts) + "\n"

assert run(large_input) == """Wire-format compatible.
Wire-format compatible.
Wire-format compatible.""", "large descriptor"

# Recursive cycle case.
cycle_input = """3
message A { optional A next = 1 ; }
message B { optional C next = 1 ; }
message C { optional B next = 1 ; }
3
A B
A C
B C
"""

assert run(cycle_input) == """Wire-format compatible.
Wire-format compatible.
Wire-format compatible.""", "recursive cycles"
```Trường hợp kích thước tối thiểu kiểm tra xem phần thân mô tả trống có tạo ra một thông báo thông thường có chữ ký trống và thông báo đó luôn tương thích với chính nó hay không. 

Trường hợp thẻ tối đa kiểm tra xem số trường hợp pháp lớn nhất có được coi là số nguyên thông thường hay không và việc thay đổi nó bằng một sẽ thay đổi nhận dạng trường cấp dây. 

Trường hợp lặp lại tất cả sẽ kiểm tra xem tên trường có bị bỏ qua không và các quy tắc lặp lại vẫn tương thích khi các khai báo sử dụng các tên hoàn toàn khác nhau. 

Trường hợp 1.000 tin nhắn được tạo sẽ thực hiện kích thước số lượng tin nhắn lớn nhất và xác minh rằng quá trình tiền xử lý tạo ra một lớp cho tất cả các tin nhắn có cấu trúc giống hệt nhau. 

Trường hợp chu trình đệ quy kiểm tra lý do chính của việc sử dụng tinh chỉnh lặp thay vì mở rộng đệ quy đơn giản. Thuật toán đạt đến một phân vùng ổn định mặc dù không có sự mở rộng hữu hạn của các định nghĩa thông báo lồng nhau. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`message A { }`, truy vấn`A A`| Tương thích | Tin nhắn trống và mô tả tối thiểu | 
| Thẻ`536870911`so với`536870910`| Tương thích rồi không tương thích | Số trường pháp lý tối đa và độ nhạy thẻ | 
| Hai tin nhắn có bốn trường chuỗi lặp lại | Tương thích | Tên trường bị bỏ qua và các quy tắc lặp lại được giữ nguyên | 
| 1.000 thông điệp có cấu trúc giống nhau | Tất cả các cặp truy vấn đều tương thích | Số lượng tin nhắn tối đa và tiền xử lý | 
| Ba tin nhắn đệ quy lẫn nhau | Cả ba cặp đều tương thích | Tham chiếu tin nhắn theo chu kỳ | 

## Vỏ cạnh 

Các tên trường khác nhau được xử lý bằng cách loại trừ tên trường khỏi mỗi chữ ký. Vì```
message A { optional string first = 1 ; }
message B { optional string second = 1 ; }
```cả hai tin nhắn đều tạo ra chữ ký giống hệt nhau,`(1, optional, string)`, thế là họ vào cùng một lớp. Thuật toán tuân theo chính xác định dạng dây thay vì tên cấp nguồn của bộ mô tả. 

Các thẻ khác nhau được xử lý trực tiếp bởi phần tử đầu tiên của mỗi chữ ký trường. Vì```
message A { optional string value = 1 ; }
message B { optional string value = 2 ; }
```các chữ ký là`(1, optional, string)`Và`(2, optional, string)`. Chúng bị tách rời trong lần sàng lọc đầu tiên và không bao giờ có thể tương thích sau này. 

Các quy tắc khác nhau được hiển thị tương tự mà không cần nhìn vào bên trong các tin nhắn lồng nhau. Một trường tùy chọn đóng góp`optional`vào chữ ký của nó, trong khi một trường lặp lại góp phần`repeated`. Do đó```
message A { optional string value = 1 ; }
message B { repeated string value = 1 ; }
```được tách ra ngay lập tức. Điều này tránh được lỗi phổ biến là chỉ kiểm tra thẻ và loại dây mà quên mất tính đa dạng. 

Cả chuỗi và tin nhắn nhúng đều sử dụng loại dây 2, nhưng chữ ký vẫn giữ được sự khác biệt giữa`string`Và`message`. Trường thông báo cũng mang lớp hiện tại của mục tiêu lồng nhau. Vì vậy, một tuyên bố như```
message Empty { }
message Holder { optional Empty value = 1 ; }
message Text { optional string value = 1 ; }
```cho`Holder`trường có giá trị thông báo và`Text`một trường có giá trị chuỗi, do đó chúng nhận được các lớp khác nhau mặc dù số loại dây của chúng đều là 2. 

Các định nghĩa đệ quy được xử lý mà không cần đệ quy trong quá trình triển khai Python. Vì```
message A { optional A next = 1 ; }
```sự sàng lọc đầu tiên nhìn thấy hình dạng chung`(1, optional, message, 0)`. Các lần lặp tiếp theo tiếp tục thấy cùng một lớp mục tiêu, do đó phân vùng sẽ ổn định. Thuật toán không bao giờ cố gắng xây dựng một biểu diễn lồng nhau vô hạn của`A`. 

Các định nghĩa đệ quy lẫn nhau hoạt động vì cùng một lý do. TRONG```
message B { optional C next = 1 ; }
message C { optional B next = 1 ; }
```cả hai mục tiêu ban đầu thuộc cùng một lớp. Do đó, chữ ký của họ vẫn bằng nhau và cặp đôi này vẫn ở bên nhau qua mọi lần sàng lọc. Điểm cố định nắm bắt trực tiếp sự tương đương đệ quy thay vì yêu cầu trường hợp phát hiện chu kỳ đặc biệt.
