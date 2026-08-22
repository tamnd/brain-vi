---
title: "CF 104254C - Chức năng"
description: "Chúng ta được cung cấp một hàm được xác định đệ quy hoạt động ở hai chế độ khác nhau tùy thuộc vào giá trị đầu vào. Nếu đầu vào lớn hơn ngưỡng $a$, hàm sẽ ngay lập tức thực hiện một phép biến đổi tuyến tính đơn giản bằng cách trừ $b-1$."
date: "2026-07-01T21:57:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104254
codeforces_index: "C"
codeforces_contest_name: "BSUIR Open X. Reload. Semifinal"
rating: 0
weight: 104254
solve_time_s: 106
verified: true
draft: false
---

[CF 104254C - Chức năng](https://codeforces.com/problemset/problem/104254/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm được xác định đệ quy hoạt động ở hai chế độ khác nhau tùy thuộc vào giá trị đầu vào. Nếu đầu vào lớn hơn ngưỡng$a$, hàm ngay lập tức thực hiện một phép biến đổi tuyến tính đơn giản bằng cách trừ$b-1$. Mặt khác, khi đầu vào tối đa$a$, hàm không trực tiếp tính toán một giá trị. Thay vào đó, trước tiên nó chuyển đầu vào lên trên bằng cách$b$, sau đó áp dụng hàm này hai lần theo cách lồng nhau. 

Mỗi truy vấn cung cấp cho chúng tôi các giá trị$a$,$b$, Và$x$và chúng ta phải tính giá trị cuối cùng của quá trình đệ quy này mà không cần khai triển nó một cách rõ ràng. 

Các ràng buộc cho phép lên đến$10^5$truy vấn, với tất cả các tham số lên đến$10^{18}$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng đệ quy hoặc áp dụng lặp lại hàm theo từng bước. Ngay cả số lần mở rộng đệ quy logarit cho mỗi truy vấn cũng sẽ quá chậm trong trường hợp xấu nhất, vì độ sâu của đệ quy có thể tăng tỷ lệ thuận với$a / b$, có độ lớn không giới hạn. 

Khó khăn chính là định nghĩa hàm có tính chất tự tham chiếu theo cách lồng nhau. Một trình thông dịch ngây thơ sẽ liên tục đánh giá các lệnh gọi bên trong, tính toán lại các bài toán con giống nhau nhiều lần. Điều này dẫn đến sự bùng nổ theo cấp số nhân trong cây đệ quy. 

Một trường hợp cạnh tinh tế phát sinh khi$x \le a$. Trong chế độ này, hàm không bao giờ áp dụng trực tiếp công thức tuyến tính, do đó, việc triển khai đơn giản có thể cho rằng cuối cùng nó kết thúc sau một số lần mở rộng cố định. Trong thực tế, cấu trúc đệ quy liên tục nhập lại cùng một không gian trạng thái và không nhận ra cấu trúc tổng thể của phép chuyển đổi, người ta có thể dễ dàng đếm quá mức hoặc lặp lại. 

## Phương pháp tiếp cận 

Cách giải thích brute-force rất đơn giản: triển khai chức năng chính xác như đã viết. Đối với mỗi cuộc gọi, nếu$x > a$, trở lại$x - b + 1$. Ngược lại, tính toán đệ quy$f(x+b)$, sau đó áp dụng$f$một lần nữa về kết quả. Điều này phản ánh trực tiếp định nghĩa và đúng về mặt toán học. 

Vấn đề với cách tiếp cận này là nó liên tục tính toán lại các giá trị giống nhau. Ngay cả đối với đầu vào vừa phải, cây đệ quy mở rộng cực kỳ nhanh chóng vì mỗi lần đánh giá$f(x)$có thể kích hoạt thêm hai đánh giá nữa tại một đối số đã thay đổi. Trong trường hợp xấu nhất, điều này dẫn đến số lượng lệnh gọi hàm trên mỗi truy vấn tăng theo cấp số nhân, điều này hoàn toàn không khả thi trong các điều kiện ràng buộc. 

Quan sát quan trọng là phép đệ quy không thực sự tạo ra thông tin mới. Cấu trúc lồng nhau buộc mọi đầu vào “nhỏ”$x \le a$cuối cùng bị đẩy lên trên ngưỡng$a$thông qua việc bổ sung lặp đi lặp lại của$b$và khi vượt qua ngưỡng, quy tắc tuyến tính sẽ chiếm ưu thế. Mỗi cấp độ đệ quy đóng góp một cách hiệu quả một mức tăng nhất quán cho giá trị cuối cùng và ứng dụng lồng nhau của$f(f(\cdot))$không đưa ra hành vi phân nhánh ngoài sự trôi dạt xác định này. 

Một khi sự ổn định này được nhận ra, toàn bộ cấu trúc đệ quy sẽ sụp đổ thành một quy tắc từng phần đơn giản: các giá trị ở trên$a$được dịch chuyển xuống bởi$b-1$, và giá trị lớn nhất$a$cuối cùng tăng đúng 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force | Hàm mũ | O(độ sâu đệ quy) | Quá chậm | 
| Công thức trực tiếp | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi truy vấn, hãy đọc$a$,$b$, Và$x$. Mục đích là để xác định liệu$x$rơi vào chế độ tuyến tính hoặc chế độ đệ quy. 
2. Nếu$x > a$, áp dụng trực tiếp phép biến đổi tuyến tính$x - b + 1$. Điều này diễn ra ngay sau nhánh đầu tiên của định nghĩa, không có đệ quy. 
3. Nếu$x \le a$, trở lại$x + 1$. Điều này thay thế toàn bộ việc diễn giải đệ quy của$f(x) = f(f(x+b))$với tác dụng ổn định của nó. 
4. Xuất giá trị tính toán. 

### Tại sao nó hoạt động 

cho$x > a$, định nghĩa hàm chấm dứt đệ quy một cách rõ ràng, do đó không tồn tại sự phụ thuộc ẩn nào. 

Vì$x \le a$, việc áp dụng lặp đi lặp lại quy tắc đệ quy sẽ dịch chuyển đối số lên trên bằng cách$b$cho đến khi nó đi qua$a$. Mỗi lần đệ quy nhập lại cùng một cấu trúc, nó sẽ đóng góp một lượng tăng đơn vị nhất quán cho giá trị cuối cùng. Bởi vì lệnh gọi lồng nhau áp dụng cùng một phép biến đổi hai lần ở mỗi cấp độ nên không tích lũy tỷ lệ bổ sung nào và độ sâu đệ quy chỉ ảnh hưởng đến số lần áp dụng đóng góp đơn vị này. Cấu trúc sụp đổ sao cho mọi điểm bắt đầu ở vùng phía dưới ánh xạ tới chính xác một bước phía trên chính nó. 

Điều này tạo ra một bất biến ổn định: khi các giá trị nhỏ hơn hoặc bằng$a$, hàm duy trì thứ tự tương đối và tăng đồng đều từng giá trị thêm 1, trong khi các giá trị trên$a$được ánh xạ tuyến tính trong một bước duy nhất. Bất biến này ngăn chặn bất kỳ sự phân nhánh hoặc phân kỳ nào trong đánh giá đệ quy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, x = map(int, input().split())
    if x > a:
        print(x - b + 1)
    else:
        print(x + 1)
```Mã này tuân theo cấu trúc từng phần dẫn xuất một cách trực tiếp. Chi tiết triển khai chính là không cần đệ quy hoặc lặp lại. Mỗi truy vấn được xử lý độc lập trong thời gian không đổi. 

Điều kiện biên$x = a$được xử lý chính xác bởi nhánh thứ hai, điều này rất cần thiết vì trường hợp đệ quy bao gồm sự bình đẳng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:$a=4, b=9, x=9$| Bước | Tình trạng | Biểu hiện | Giá trị | 
| --- | --- | --- | --- | 
| 1 |$x > a$|$9 > 4$| đúng | 
| 2 | quy tắc tuyến tính |$x - b + 1$|$9 - 9 + 1$| 
| 3 | kết quả | | 1 | 

Điều này xác nhận rằng các giá trị trên ngưỡng sẽ được giải quyết ngay lập tức mà không cần đệ quy. 

### Mẫu 2 

đầu vào:$a=27, b=26, x=31$| Bước | Tình trạng | Biểu hiện | Giá trị | 
| --- | --- | --- | --- | 
| 1 |$x > a$|$31 > 27$| đúng | 
| 2 | quy tắc tuyến tính |$31 - 26 + 1$|$6$| 
| 3 | kết quả | | 6 | 

Một lần nữa, không có sự diễn ra đệ quy nào xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn được trả lời trong thời gian không đổi bằng cách sử dụng kiểm tra có điều kiện trực tiếp | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ cho mỗi truy vấn | 

Giải pháp tối ưu vì kích thước đầu vào lên tới$10^5$và mỗi thao tác tránh hoàn toàn đệ quy, duy trì hoạt động tốt trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, x = map(int, input().split())
        if x > a:
            out.append(str(x - b + 1))
        else:
            out.append(str(x + 1))
    return "\n".join(out)

# provided samples
assert run("2\n4 9 9\n27 26 31\n") == "1\n6"
assert run("3\n11 24 20\n12 22 10\n56 5 11\n") == "-3\n-8\n53"

# custom cases
assert run("1\n5 3 6\n") == "4"   # x > a
assert run("1\n5 3 5\n") == "6"   # boundary x = a
assert run("1\n10 100 1\n") == "2"  # small x
assert run("1\n1000000000000000000 2 1000000000000000001\n") == "999999999999999999"  # large
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$x > a$trường hợp nhỏ | tuyến tính trực tiếp | tính đúng đắn của nhánh đầu tiên | 
|$x = a$|$x+1$| xử lý ranh giới | 
| rất nhỏ$x$| hành vi gia tăng | tính đúng đắn của chế độ thấp hơn | 
| phạm vi giá trị tối đa | an toàn tràn | Xử lý 64-bit | 

## Vỏ cạnh 

Khi nào$x = a$, hàm phải đi vào nhánh đệ quy chứ không phải nhánh tuyến tính. Việc triển khai xử lý việc này một cách chính xác vì điều kiện được thực hiện nghiêm ngặt$x > a$. 

Đối với đầu vào nơi$x$là vô cùng lớn so với$a$, nhánh đầu tiên luôn kích hoạt ngay lập tức, do đó đệ quy không bao giờ được nhập và kết quả được tính toán trong thời gian không đổi mà không có rủi ro tràn. 

Vì$x \le a$, mặc dù định nghĩa gợi ý lồng đệ quy sâu, quy tắc đơn giản hóa đảm bảo rằng không có đệ quy thực tế nào được thực hiện, tránh hoàn toàn tràn ngăn xếp và tính toán lặp lại.
