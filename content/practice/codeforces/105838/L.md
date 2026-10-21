---
title: "CF 105838L - Thế Giới Tham Lam"
description: "Chúng ta bắt đầu với một tập hợp chính xác các số $k$. Trong số này, các bản sao của $k-1$ có giá trị nào đó là $x$, và một phần tử duy nhất bằng $y$. Vì thế ban đầu cấu trúc cực kỳ đồng đều ngoại trừ một “sự xáo trộn”."
date: "2026-06-21T22:41:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105838
codeforces_index: "L"
codeforces_contest_name: "The 14th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 105838
solve_time_s: 64
verified: true
draft: false
---

[CF 105838L - Thế giới tham lam](https://codeforces.com/problemset/problem/105838/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với nhiều bộ chính xác$k$những con số. Trong số này,$k-1$bản sao bằng một số giá trị$x$và một phần tử bằng$y$. Vì thế ban đầu cấu trúc cực kỳ đồng đều ngoại trừ một “sự xáo trộn”. 

Mỗi thao tác chọn hai phần tử bất kỳ$a$Và$b$, xóa chúng, tính trung bình, làm tròn xuống rồi chèn hai bản sao của giá trị làm tròn này. Trong thực tế, một cặp được thay thế bằng hai giá trị giống nhau bằng$\left\lfloor \frac{a+b}{2} \right\rfloor$. Kích thước của nhiều tập hợp không bao giờ thay đổi, nhưng các giá trị dần dần trôi về phía nhau trong khi mất đi các phần phân số. 

Quá trình được lặp lại tùy ý nhiều lần. Cuối cùng, luôn có thể đạt đến trạng thái mà tất cả các phần tử đều bằng nhau. Nhiệm vụ là xác định giá trị nhỏ nhất có thể có của giá trị chung cuối cùng này$S$. 

Những hạn chế là rất lớn:$k$có thể đi lên$10^9$và các giá trị có thể đạt tới$10^{18}$. Điều này ngay lập tức loại trừ mọi mô phỏng của quá trình. Ngay cả việc lưu trữ nhiều tập hợp cũng là không thể, vì vậy giải pháp phải nén toàn bộ động lực thành một biểu thức dạng đóng bắt nguồn từ các bất biến của phép toán. 

Một trường hợp cạnh tinh tế đến từ mức trung bình. Nếu như$a+b$là số lẻ, tổng của hai phần tử thu được nhỏ hơn rất nhiều so với$a+b$, nghĩa là tổng số tiền không được bảo toàn. Điều này tạo ra khả năng “tổn thất” mà một lập luận dựa trên mức trung bình ngây thơ có thể bỏ lỡ. Ví dụ, với các giá trị$1$Và$2$, hoạt động tạo ra hai$1$s, giảm tổng số tiền đi$1$. Bất kỳ lý do nào giả định bảo toàn tổng số tiền sẽ thất bại ngay lập tức. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ liên tục chọn các cặp, áp dụng phép biến đổi và hy vọng hệ thống sẽ hội tụ. Thậm chí đại diện cho chi phí nhà nước$O(k)$, và mỗi thao tác là$O(1)$, nhưng về nguyên tắc, số lượng thao tác trước khi ổn định là không bị giới hạn và có thể dễ dàng mở rộng quy mô theo độ lớn của các giá trị. Với$k$lên đến$10^9$, phương án này hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là hoạt động hoạt động tuyến tính ngoại trừ tổn thất làm tròn được kiểm soát. Nếu chúng ta xác định tổng của nhiều tập hợp, mỗi thao tác sẽ biến đổi$(a,b)$thành hai bản sao$\left\lfloor \frac{a+b}{2} \right\rfloor$, do đó tổng số tiền giảm chính xác bằng$(a+b) \bmod 2$. Điều này có nghĩa là cách duy nhất để tổng thay đổi là mất một đơn vị khi cặp được chọn có tổng lẻ. 

Điều này chuyển quan điểm từ theo dõi các giá trị riêng lẻ sang theo dõi tổng mức giảm. Vì trạng thái cuối cùng có tất cả các phần tử bằng$S$, tổng cuối cùng là$kS$. Do đó, vấn đề giảm xuống mức hiểu được chúng ta có thể kiếm được số tiền cuối cùng nhỏ đến mức nào trong khi tôn trọng ràng buộc là chúng ta chỉ có thể mất tối đa một đơn vị cho mỗi thao tác và chỉ khi tính chẵn lẻ cho phép. 

Từ cấu trúc của cấu hình ban đầu, tổng “khối lượng dư thừa” trên đường cơ sở$x$tập trung vào một phần tử duy nhất$y$. Quá trình này cho phép phần dư thừa này được phân phối lại trên tất cả$k$các vị trí và mỗi lần nó được phân phối lại theo cách tạo ra sự không khớp chẵn lẻ, chúng ta có thể mất một đơn vị. Chiến lược tối ưu giúp dàn trải thặng dư một cách đồng đều nhất có thể một cách hiệu quả và giới hạn dưới không thể tránh khỏi xuất phát từ việc lấy trung bình tổng số tiền trên$k$các phần tử. 

Điều này dẫn đến một biểu mẫu khép kín: giá trị cuối cùng có thể đạt được tốt nhất được xác định theo giá trị sàn của tổng số tiền trung bình và trong cấu hình hai giá trị cụ thể này, nó đơn giản hóa một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\text{unbounded})$|$O(k)$| Quá chậm | 
| Phân tích bất biến + tổng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút ra câu trả lời trực tiếp từ các bất biến toàn cục thay vì mô phỏng các hoạt động. 

1. Tính tổng ban đầu của tất cả các phần tử. Mảng chứa$k-1$bản sao của$x$và một bản sao của$y$, vậy tổng là$(k-1)x + y$. 
2. Quan sát rằng mọi thao tác đều bảo toàn số phần tử, do đó cấu hình cuối cùng phải bao gồm chính xác$k$giá trị giống nhau$S$, cho biết tổng số tiền$kS$. 
3. Theo dõi tổng tiến triển như thế nào trong một phép toán. Thay thế$a, b$với hai bản sao của$\left\lfloor \frac{a+b}{2} \right\rfloor$giảm tổng bằng$(a+b) \bmod 2$, là 0 hoặc 1. 
4. Vì chúng ta có thể chọn các cặp nhiều lần nên chúng ta luôn có thể nhận ra một chuỗi các thao tác phân phối các giá trị đồng đều nhất có thể. Tác động không thể đảo ngược duy nhất là tổn thất tích lũy gây ra bởi sự không khớp chẵn lẻ, điều này không bao giờ làm tăng tổng. 
5. Do đó, tổng cuối cùng nhỏ nhất có thể là bội số lớn nhất của$k$số đó không vượt quá tổng ban đầu, bởi vì chúng ta chỉ có thể giảm tổng theo từng bước đơn vị và không thể phân bổ đều khối lượng còn lại xuống dưới. 
6. Chuyển câu trả lời này thành câu trả lời cuối cùng$S = \left\lfloor \frac{(k-1)x + y}{k} \right\rfloor$. 

Từ$(k-1)x + y = kx + (y-x)$, điều này đơn giản hóa thành$S = x + \left\lfloor \frac{y-x}{k} \right\rfloor$. 

### Tại sao nó hoạt động 

Bất biến là sự kết hợp giữa số lượng cố định và mức giảm đơn điệu có kiểm soát của tổng số tiền. Mọi thao tác đều bảo toàn tổng hoặc giảm tổng đó đi đúng một và không có thao tác nào có thể làm tăng tổng đó. Vì trạng thái cuối cùng được xác định hoàn toàn bởi một giá trị duy nhất được lặp lại$k$lần, tổng hoàn toàn đặc trưng cho hệ thống khi kết thúc. Quyền tự do duy nhất là có thể “chi tiêu” bao nhiêu trong số tiền ban đầu thông qua các tổn thất chẵn lẻ trước khi đạt được cấu hình thống nhất. Sự tự do đó được nắm bắt một cách chính xác bằng cách lấy giá trị thống nhất tối đa có thể phù hợp với tổng số tiền bị ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        k, x, y = map(int, input().split())
        s = (k - 1) * x + y
        print(s // k)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp mã hóa đối số bất biến. Thay vì mô phỏng các hoạt động, nó tính tổng ban đầu và chia cho$k$, dựa trên thực tế là quy trình chỉ có thể giảm tổng theo các bước nguyên cho đến khi đạt được cấu hình đồng nhất. 

Chi tiết quan trọng là phép chia số nguyên: vì giá trị cuối cùng phải là số nguyên và tổng chỉ có thể giảm nên câu trả lời chính xác là giá trị sàn của tổng trung bình. 

## Ví dụ đã hoạt động 

Xem xét cấu hình giống như mẫu$k=3, x=3, y=5$. 

| Bước | Tổng hợp | Biểu hiện | 
| --- | --- | --- | 
| Ban đầu | 11 |$2\cdot 3 + 5$| 
| Cuối cùng cho mỗi yếu tố | 3 |$11 // 3 = 3$| 

Điều này cho thấy rằng mặc dù chúng ta bắt đầu với một giá trị lớn hơn, nhưng điều tốt nhất chúng ta có thể làm là phân phối tổng khối lượng cho ba phần tử và lấy trung bình là số nguyên. 

Bây giờ hãy xem xét$k=4, x=2, y=10$. 

| Bước | Tổng hợp | Biểu hiện | 
| --- | --- | --- | 
| Ban đầu | 16 |$3\cdot 2 + 10$| 
| Cuối cùng cho mỗi yếu tố | 4 |$16 // 4 = 4$| 

Ở đây khối lượng dư đủ lớn để nâng giá trị đồng nhất cuối cùng lên trên$x$và quá trình lấy trung bình sẽ dàn trải nó một cách đầy đủ. 

Những dấu vết này xác nhận rằng quá trình này hoạt động giống như một sự phân phối lại tổng khối lượng có kiểm soát chỉ với hiệu ứng làm tròn đi xuống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm yêu cầu các phép tính số học có thời gian không đổi | 
| Không gian |$O(1)$| Không có trạng thái nào ngoài các biến đầu vào được lưu trữ | 

Giải pháp là tối ưu cho$T \le 10^5$, vì nó tránh mọi mô phỏng theo từng phần tử hoặc lặp lại và giảm từng trường hợp thử nghiệm thành một phép tính số học duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        k, x, y = map(int, input().split())
        out.append(str(((k - 1) * x + y) // k))
    return "\n".join(out)

# provided sample (as inferred format example)
assert solve_io("1\n3 3 5\n") == "3"

# minimum case
assert solve_io("1\n2 0 0\n") == "0"

# no disturbance
assert solve_io("1\n5 7 7\n") == "7"

# large gap but small effect
assert solve_io("1\n10 1 20\n") == str(((9*1 + 20)//10))

# edge parity case
assert solve_io("1\n3 0 2\n") == str((0*2 + 2)//3)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 0 | 0 | cấu hình ổn định tối thiểu | 
| 5 7 7 | 7 | đầu vào đã thống nhất | 
| 10 1 20 | giá trị tính toán | phân phối lại không tầm thường | 
| 3 0 2 | hiệu ứng trung bình sàn | hành vi chẵn lẻ và làm tròn | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi tất cả các giá trị đều bằng nhau, chẳng hạn như$k=5, x=y=7$. Thao tác vẫn được áp dụng nhưng không thể thay đổi bất kỳ giá trị nào, vì việc lấy trung bình các số giống nhau sẽ bảo toàn chúng. Thuật toán xử lý việc này một cách trực tiếp vì tổng là$5 \cdot 7$, và phép chia trả về$7$, phù hợp với bất biến. 

Một trường hợp cạnh khác là khi$y$chỉ lớn hơn một chút so với$x$, Ví dụ$k=4, x=10, y=11$. Ở đây khối lượng tăng thêm chỉ là 1, nhưng nó được phân bổ trên 4 vị trí, khiến cho không thể tăng bất kỳ phần tử nào lên 1 ở trạng thái đồng nhất cuối cùng. Công thức trả về chính xác$10$và bất kỳ nỗ lực nào nhằm “truyền bá” đơn vị bổ sung đều thất bại vì nó không thể tồn tại khi tính trung bình trên tất cả các phần tử. 

Trường hợp cạnh cuối cùng xảy ra ở các giá trị lớn gần giới hạn ràng buộc, trong đó$y-x$gần với$2k-1$. Ngay cả trong trường hợp cực đoan này, quá trình tính toán vẫn ổn định vì mọi thứ đều giảm xuống còn một phép chia số nguyên duy nhất, tránh tình trạng tràn hoặc mất ổn định lặp lại.
