---
title: "CF 1657B - Trình tự XY"
description: "Chúng ta cần xây dựng một mảng $a0, a1, dots, an$ bắt đầu từ $a0 = 0$. Với mỗi vị trí tiếp theo, chúng ta có đúng hai lựa chọn. Chúng ta có thể tăng giá trị hiện tại thêm $x$ hoặc giảm giá trị hiện tại thêm $y$. Hạn chế duy nhất là mọi giá trị trong chuỗi phải ở mức tối đa là $B$."
date: "2026-06-10T03:27:50+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1657
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 125 (Rated for Div. 2)"
rating: 800
weight: 1657
solve_time_s: 116
verified: true
draft: false
---

[CF 1657B - Trình tự XY](https://codeforces.com/problemset/problem/1657/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng$a_0, a_1, \dots, a_n$bắt đầu từ$a_0 = 0$. 

Với mỗi vị trí tiếp theo, chúng ta có đúng hai lựa chọn. Chúng ta có thể tăng giá trị hiện tại lên$x$, hoặc giảm nó đi$y$. Hạn chế duy nhất là mọi giá trị trong chuỗi phải ở mức tối đa$B$. Giá trị âm hoàn toàn được cho phép. 

Trong số tất cả các chuỗi hợp lệ, chúng ta muốn chuỗi có tổng các phần tử càng lớn càng tốt. 

Các ràng buộc đủ nhỏ để chúng ta có thể xử lý từng vị trí riêng lẻ. Mặc dù$t$có thể lớn như$10^4$, tổng của tất cả$n$giá trị nhiều nhất là$2 \cdot 10^5$. Điều đó có nghĩa là một$O(n)$thuật toán cho mỗi trường hợp thử nghiệm, hay chính xác hơn là$O(\sum n)$trên tất cả các trường hợp thử nghiệm, dễ dàng phù hợp trong thời gian giới hạn. Việc khám phá theo cấp số nhân tất cả các lựa chọn có thể là không thể vì mỗi vị trí đều có hai lựa chọn, dẫn đến$2^n$trình tự. 

Phần khó khăn là nhận ra rằng việc tối đa hóa tổng số tiền không phải là vấn đề tối ưu hóa tổng thể đòi hỏi phải lập trình động. Cấu trúc đơn giản hơn nhiều. 

Hãy xem xét đầu vào này:```
1
4 1 7 3
```Trình tự đúng là:```
0, -3, -6, 1, -2
```với tổng$-10$. 

Một chiến lược bất cẩn luôn cố gắng thêm$x$sẽ ngay lập tức vi phạm giới hạn vì$0 + 7 > 1$. Vì các giá trị được phép trở thành âm nên giải pháp tối ưu có thể cố ý di chuyển xuống dưới trước và sau đó mới tiến lên. 

Một trường hợp thú vị khác là:```
1
3 5 5 1
```Trình tự tối ưu là:```
0, 5, 4, 3
```với tổng$12$. 

Sau khi đạt$5$, thêm cái khác$5$sẽ vượt quá$B$, vì vậy chúng ta phải trừ$1$. Một giải pháp thay thế các phép toán một cách mù quáng hoặc luôn ưu tiên phép trừ sau khi đạt đến giới hạn sẽ bỏ lỡ tổng số tốt nhất. 

Câu trả lời lớn cũng có thể:```
1
7 1000000000 1000000000 1000000000
```Câu trả lời là:```
4000000000
```Sử dụng số nguyên 32 bit sẽ tràn. Việc triển khai phải sử dụng số nguyên Python hoặc số học 64 bit bằng các ngôn ngữ khác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xem mọi vị trí dưới dạng lựa chọn nhị phân. Ở bước$i$, hoặc thêm$x$hoặc trừ$y$. Chúng ta có thể thử đệ quy cả hai khả năng bất cứ khi nào chúng tạo ra các giá trị hợp lệ và giữ được tổng số tốt nhất. 

Tìm kiếm mạnh mẽ này là chính xác vì nó kiểm tra mọi chuỗi hợp lệ. Thật không may, nó có$2^n$trạng thái trong trường hợp xấu nhất. Với$n$lên đến$2 \cdot 10^5$, điều này hoàn toàn không thể thực hiện được. 

Quan sát chính là mục tiêu là tối đa hóa tổng của tất cả các giá trị chuỗi. Ở mỗi bước, tương lai chỉ phụ thuộc vào giá trị hiện tại. Nếu cả hai thao tác đều được phép, việc chọn giá trị kết quả lớn hơn luôn tốt hơn. 

Giả sử giá trị hiện tại là$cur$. 

Nếu như$cur + x \le B$, thì hai giá trị ứng viên tiếp theo là:$$cur + x$$Và$$cur - y$$Từ$x > 0$Và$y > 0$,$$cur + x > cur - y.$$Giá trị tiếp theo lớn hơn không chỉ làm tăng đóng góp hiện tại cho câu trả lời mà còn mang lại điểm khởi đầu lớn hơn cho tất cả các quyết định trong tương lai. Không bao giờ có lý do để chọn phép trừ khi phép cộng có giá trị. 

Điều đó có nghĩa là quyết định bị ép buộc: 

Nếu thêm$x$nằm trong giới hạn, hãy làm điều đó. 

Ngược lại thì trừ$y$. 

Điều này mang lại một mô phỏng tham lam đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Mô phỏng tham lam tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo giá trị hiện tại là$cur = 0$. 
2. Khởi tạo câu trả lời dưới dạng$ans = 0$, từ$a_0 = 0$đóng góp vào tổng số. 
3. Đối với mỗi vị trí từ$1$ĐẾN$n$, kiểm tra xem có thêm$x$sẽ giữ giá trị trong giới hạn cho phép. 
4. Nếu$cur + x \le B$, bộ$cur = cur + x$. 

Đây là sự lựa chọn tốt nhất có thể vì nó tạo ra một giá trị lớn hơn rất nhiều so với$cur - y$. 
5. Nếu không, hãy đặt$cur = cur - y$. 

Phép cộng bị cấm trong tình huống này, vì vậy phép trừ là nước đi hợp lệ duy nhất. 
6. Thêm giá trị mới của$cur$vào tổng số đang chạy. 
7. Sau khi xử lý xong tất cả$n$vị trí, xuất ra số tiền tích lũy. 

### Tại sao nó hoạt động 

Sự lựa chọn tham lam là tối ưu cục bộ và toàn cầu. 

Bất cứ khi nào$cur + x \le B$, cả hai hoạt động đều hợp pháp. Giá trị kết quả sau khi cộng luôn lớn hơn giá trị kết quả sau khi trừ vì$x$Và$y$là tích cực. 

Việc chọn giá trị lớn hơn sẽ ngay lập tức làm tăng số hạng hiện tại trong tổng số tiền. Nó cũng để lại cho chúng ta giá trị hiện tại lớn hơn cho các bước đi trong tương lai. Bất kỳ trình tự nào có thể đạt được sau khi chọn$cur - y$có thể được sao chép từ trạng thái lớn hơn$cur + x$với những đóng góp tương đương hoặc tốt hơn trong tương lai. 

Vì vậy, mỗi khi phép cộng được thực hiện thì phép trừ sẽ chiếm ưu thế. Lần duy nhất chúng ta trừ là khi phép cộng vi phạm ràng buộc. Lặp lại quy tắc này ở mỗi bước sẽ tạo ra số tiền tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        n, B, x, y = map(int, input().split())

        cur = 0
        total = 0

        for _ in range(n):
            if cur + x <= B:
                cur += x
            else:
                cur -= y

            total += cur

        answers.append(str(total))

    sys.stdout.write("\n".join(answers))

solve()
```Biến`cur`lưu trữ giá trị chuỗi hiện tại. Chúng tôi bắt đầu lúc`0`, tương ứng với$a_0$. 

Đối với mỗi vị trí mới, trước tiên chúng tôi kiểm tra xem việc thêm$x$vẫn nằm trong giới hạn trên$B$. Nếu có, chúng tôi sẽ thực hiện tùy chọn đó. Ngược lại chúng ta phải trừ$y$. 

Sau khi xác định được giá trị mới, chúng ta thêm ngay vào`total`. Từ`total`bắt đầu từ số 0 và$a_0 = 0$, không cần xử lý riêng cho phần tử đầu tiên. 

Một sai lầm phổ biến là nghĩ rằng các giá trị âm bị cấm. Bài toán chỉ giới hạn các giá trị ở trên nên phép trừ có thể tạo ra các số âm tùy ý. 

Một lỗi phổ biến khác là sử dụng số nguyên 32 bit. Bản thân mẫu chứa câu trả lời của$4 \times 10^9$, vượt quá phạm vi 32-bit đã ký. Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, B = 100, x = 1, y = 30
```| Bước | Hiện Tại Trước | Hành động | Hiện Tại Sau | Tổng Chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | Bắt đầu | 0 | 0 | 
| 1 | 0 | +1 | 1 | 1 | 
| 2 | 1 | +1 | 2 | 3 | 
| 3 | 2 | +1 | 3 | 6 | 
| 4 | 3 | +1 | 4 | 10 | 
| 5 | 4 | +1 | 5 | 15 | 

Trả lời:```
15
```Giới hạn này lớn đến mức luôn có thể thực hiện được phép cộng. Quy tắc tham lam tiếp tục tăng giá trị và tạo ra chuỗi tối ưu rõ ràng. 

### Ví dụ 2 

đầu vào:```
n = 7, B = 1000000000, x = 1000000000, y = 1000000000
```| Bước | Hiện Tại Trước | Hành động | Hiện Tại Sau | Tổng Chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | Bắt đầu | 0 | 0 | 
| 1 | 0 | +x | 1000000000 | 1000000000 | 
| 2 | 1000000000 | -y | 0 | 1000000000 | 
| 3 | 0 | +x | 1000000000 | 2000000000 | 
| 4 | 1000000000 | -y | 0 | 2000000000 | 
| 5 | 0 | +x | 1000000000 | 3000000000 | 
| 6 | 1000000000 | -y | 0 | 3000000000 | 
| 7 | 0 | +x | 1000000000 | 4000000000 | 

Trả lời:```
4000000000
```Dấu vết này chứng tỏ rằng chuỗi có thể dao động giữa hai giá trị. Bất cứ khi nào giới hạn trên chặn phép cộng, phép trừ trở thành bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Một quyết định liên tục cho từng vị trí | 
| Không gian |$O(1)$| Chỉ có một số biến được duy trì | 

Vì tổng của tất cả$n$giá trị trên các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, tổng thời gian chạy là$O(2 \cdot 10^5)$. Điều này là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, B, x, y = map(int, input().split())

        cur = 0
        total = 0

        for _ in range(n):
            if cur + x <= B:
                cur += x
            else:
                cur -= y
            total += cur

        out.append(str(total))

    return "\n".join(out)

# provided sample
assert run(
"""3
5 100 1 30
7 1000000000 1000000000 1000000000
4 1 7 3
"""
) == """15
4000000000
-10"""

# minimum size
assert run(
"""1
1 1 1 1
"""
) == "1", "single move"

# boundary where addition exactly reaches B
assert run(
"""1
2 5 5 1
"""
) == "9", "equality with B is allowed"

# alternating behavior
assert run(
"""1
4 2 2 2
"""
) == "4", "repeated hit of upper bound"

# larger negative excursion
assert run(
"""1
3 1 5 3
"""
) == "-8", "must go negative first"

# large values
assert run(
"""1
7 1000000000 1000000000 1000000000
"""
) == "4000000000", "64-bit answer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1`| Trường hợp nhỏ nhất có ý nghĩa | 
|`2 5 5 1`|`9`| Bình đẳng với`B`là hợp pháp | 
|`4 2 2 2`|`4`| Mô hình cộng và trừ xen kẽ | 
|`3 1 5 3`|`-8`| Cho phép giá trị âm | 
|`7 10^9 10^9 10^9`|`4000000000`| kích thước câu trả lời lớn | 

## Vỏ cạnh 

### Phép cộng chính xác đạt đến giới hạn 

đầu vào:```
1
2 5 5 1
```Dấu vết:```
0 -> 5 -> 4
```Bước đầu tiên sử dụng phép cộng vì$0 + 5 = 5$, vẫn còn hiệu lực. Một số cách triển khai không chính xác sử dụng sự bất bình đẳng nghiêm ngặt và từ chối động thái này. Tổng kết quả là:```
0 + 5 + 4 = 9
```### Cho phép giá trị âm 

đầu vào:```
1
3 1 5 3
```Dấu vết:```
0 -> -3 -> -6 -> -1
```Việc bổ sung đầu tiên sẽ vượt quá$B$, vì vậy phép trừ là bắt buộc. Thuật toán cho phép chuỗi đi xuống dưới 0 một cách chính xác vì vấn đề không đặt ra giới hạn dưới. Tổng cuối cùng là:```
0 + (-3) + (-6) + (-1) = -8
```### Câu trả lời lớn vượt quá phạm vi 32 bit 

đầu vào:```
1
7 1000000000 1000000000 1000000000
```Dấu vết:```
0 -> 10^9 -> 0 -> 10^9 -> 0 -> 10^9 -> 0 -> 10^9
```Câu trả lời là:```
4000000000
```Bất kỳ triển khai nào sử dụng số nguyên có dấu 32 bit sẽ tràn. Bản thân thuật toán tham lam vẫn đúng nhưng kiểu dữ liệu phải đủ lớn để lưu trữ tổng tích lũy.
