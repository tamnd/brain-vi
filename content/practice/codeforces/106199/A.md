---
title: "CF 106199A - \u041f\u0440\u043e\u0432\u0435\u0440\u043a\u0430 \u0441\u043e\u0441\u0442\u043e\u044f\u043d\u0438\u044f"
description: "Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu so sánh hai biểu thức được xây dựng từ nó. Biểu thức đầu tiên lấy ước chung lớn nhất của tất cả các phần tử mảng rồi áp dụng giai thừa cho kết quả đó."
date: "2026-06-19T18:33:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106199
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106199
solve_time_s: 56
verified: true
draft: false
---

[CF 106199A - \u041f\u0440\u043e\u0432\u0435\u0440\u043a\u0430 \u0441\u043e\u0441\u0442\u043e\u044f\u043d\u0438\u044f](https://codeforces.com/problemset/problem/106199/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu so sánh hai biểu thức được xây dựng từ nó. Biểu thức đầu tiên lấy ước chung lớn nhất của tất cả các phần tử mảng rồi áp dụng giai thừa cho kết quả đó. Thay vào đó, biểu thức thứ hai lấy giai thừa của từng phần tử trước rồi tính gcd của các giá trị giai thừa đó. Nhiệm vụ là quyết định xem hai giá trị này có luôn bằng nhau trong mảng đã cho hay không. 

Kích thước đầu vào có thể đạt tới 100000 phần tử và mỗi phần tử có thể lớn tới 10^9. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng tính toán giai thừa một cách rõ ràng, vì giai thừa phát triển cực kỳ nhanh và trở nên không khả thi ngay cả đối với đầu vào khiêm tốn. Ngay cả việc lưu trữ hoặc thao tác các giá trị giai thừa đầy đủ cũng không thể thực hiện được, vì vậy mọi giải pháp đều phải dựa vào các thuộc tính cấu trúc của gcd và giai thừa thay vì tính toán trực tiếp. 

Trường hợp cạnh tinh tế xuất hiện khi gcd của mảng là 1. Trong trường hợp đó, biểu thức bên trái trở thành 1! là 1. Biểu thức đúng cũng trở thành gcd của một danh sách các giai thừa và vì mọi giai thừa đều chia hết cho 1 nên kết quả cũng là 1. Vì vậy, các mảng có gcd bằng 1 luôn hợp lệ. 

Một tình huống thú vị hơn xảy ra khi gcd lớn hơn 1. Ví dụ: nếu tất cả các số là bội số của 2 nhưng một số không phải là bội số của lũy thừa cao hơn như 4 hoặc 8, thì hành vi giai thừa sẽ đưa ra các thừa số nguyên tố bổ sung không được phản ánh trong cấu trúc gcd ban đầu. Sự không phù hợp này là nguồn gốc của sự bất bình đẳng. 

Một trực giác phản ví dụ tối thiểu đã xuất hiện với các số nhỏ như 2 và 3. gcd của chúng là 1 nên đẳng thức giữ nguyên, nhưng khi tất cả các số đều có chung ước số lớn hơn 1, thì cấu trúc giai thừa sẽ khuếch đại số mũ nguyên tố theo cách có thể phá vỡ đẳng thức. 

## Phương pháp tiếp cận 

Một nỗ lực mạnh mẽ sẽ trực tiếp tính toán gcd của mảng, tính giai thừa của nó và cũng tính giai thừa cho từng phần tử rồi lấy gcd. Phần đầu thì ổn, nhưng phần thứ hai thì thất bại ngay vì thậm chí 20! đã vượt quá 10^18 và ở đây các giá trị lên tới 10^9, làm cho các giá trị giai thừa lớn về mặt thiên văn. Ngay cả khi sử dụng số nguyên lớn, việc tính toán 100000 giai thừa là không thể trong thời gian giới hạn. 

Quan sát quan trọng là cấu trúc giai thừa hoàn toàn được điều khiển bởi số mũ nguyên tố. Đối với số x!, số mũ của số nguyên tố p được xác định theo công thức Legendre. Khi chúng tôi lấy gcd của các giai thừa, chúng tôi đang lấy số mũ tối thiểu trên tất cả các tổng Legendre này một cách hiệu quả. Câu hỏi đặt ra là liệu việc áp dụng giai thừa trước hay sau gcd có bảo toàn được các mối quan hệ số mũ tối thiểu này hay không. 

Sự đơn giản hóa mang tính quyết định là hai biểu thức bằng nhau khi và chỉ khi gcd của mảng là 1. Nếu gcd là 1, cả hai bên đều đánh giá là 1 như đã giải thích trước đó. Nếu gcd lớn hơn 1 thì tất cả các phần tử đều có chung ít nhất một thừa số nguyên tố và việc khuếch đại giai thừa làm cho biểu thức thứ hai lớn hơn biểu thức thứ nhất về mặt cấu trúc số mũ, phá vỡ đẳng thức. 

Do đó, toàn bộ vấn đề được chuyển thành một phép tính gcd duy nhất trên mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (giai thừa + gcd) | Không thể (sự bùng nổ giai thừa) | O(n) + số nguyên lớn | Không sử dụng được | 
| Tối ưu (chỉ kiểm tra gcd) | O(n log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán

1. Tính gcd của tất cả các phần tử trong mảng. Giá trị này ghi lại số nguyên tối đa có thể chia đồng thời mọi phần tử, đây chính xác là thông tin cấu trúc duy nhất cần thiết. 
2. Kiểm tra xem gcd này có bằng 1 hay không. Đây là điểm quyết định duy nhất của bài toán, vì đẳng thức của các biểu thức phụ thuộc hoàn toàn vào việc có tồn tại ước chung không tầm thường hay không. 
3. Nếu gcd là 1, xuất ra CÓ. Nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Phép toán giai thừa bảo toàn tính chia hết nhưng khuếch đại số mũ nguyên tố theo cách phi tuyến tính. Khi tất cả các số đều là số nguyên tố cùng nhau (gcd bằng 1), không có thừa số nguyên tố nào buộc phải xuất hiện trong mọi phần tử và cả hai cấu trúc đều sụp đổ thành 1. Khi tồn tại một gcd không tầm thường, mỗi số đều chứa ít nhất một thừa số nguyên tố chung và các giai thừa đưa ra bội số bổ sung của các số nguyên tố này không được đồng bộ hóa trên toàn mảng, khiến gcd của các giai thừa vượt quá giai thừa của gcd. Sự không phù hợp về cấu trúc này đảm bảo sự bất bình đẳng bất cứ khi nào gcd lớn hơn 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    g = 0
    for x in a:
        g = gcd(g, x)
    
    if g == 1:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo sau việc giảm xuống một tính toán gcd duy nhất. Gcd được tích lũy lặp đi lặp lại, giúp tránh lưu trữ các kết quả trung gian hoặc thực hiện bất kỳ công việc tốn kém nào liên quan đến giai thừa. Sự so sánh cuối cùng là thời gian không đổi. 

Một chi tiết tinh tế là khởi tạo bộ tích lũy gcd bằng 0. Điều này an toàn vì gcd(0, x) = x, do đó, nó tạo hạt giống chính xác cho phép tính với phần tử mảng đầu tiên mà không cần viết hoa đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| Bước | Gcd hiện tại | 
| --- | --- | 
| bắt đầu | 0 | 
| sau 1 | 1 | 
| sau 2 | 1 | 
| sau 3 | 1 | 
| sau 4 | 1 | 
| sau 5 | 1 | 

gcd cuối cùng là 1 nên câu trả lời là CÓ. Điều này phù hợp với thực tế là không có số nào lớn hơn 1 chia tất cả các phần tử cùng một lúc và cả hai biểu thức đều thu gọn thành 1. 

### Ví dụ 2 

đầu vào:```
3
6 30 15
```| Bước | Gcd hiện tại | 
| --- | --- | 
| bắt đầu | 0 | 
| sau 6 giờ | 6 | 
| sau 30 | 6 | 
| sau 15 | 3 | 

gcd cuối cùng là 3 nên câu trả lời là KHÔNG. Ở đây tất cả các số đều có chung một thừa số lớn hơn 1 và việc khuếch đại giai thừa sẽ tạo ra sự không khớp giữa hai cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi thao tác gcd chạy theo thời gian logarit ở giá trị size | 
| Không gian | O(1) | Chỉ có một bộ tích lũy duy nhất được lưu trữ | 

Các ràng buộc cho phép tối đa 100000 số có giá trị lên tới 10^9, do đó, lượt gcd lặp lại có thể dễ dàng đủ nhanh trong vòng một giây. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    g = 0
    for x in a:
        g = gcd(g, x)
    
    return "YES" if g == 1 else "NO"

# provided samples
assert run("5\n1 2 3 4 5\n") == "YES"
assert run("3\n6 30 15\n") == "NO"

# custom cases
assert run("1\n1\n") == "YES", "single element 1"
assert run("1\n10\n") == "NO", "single element >1"
assert run("4\n2 4 8 16\n") == "NO", "all even numbers"
assert run("3\n2 3 5\n") == "YES", "pairwise coprime-ish"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | CÓ | cạnh phần tử đơn | 
| 10 | KHÔNG | phần tử không phải một | 
| 2 4 8 16 | KHÔNG | ước chung > 1 | 
| 2 3 5 | CÓ | gcd bằng 1 trường hợp | 

## Vỏ cạnh 

Khi mảng chứa một phần tử bằng 1, gcd là 1 và câu trả lời là CÓ, khớp cả hai biểu thức có giá trị bằng 1. Khi mảng chứa một phần tử lớn hơn 1, gcd bằng phần tử đó và câu trả lời trở thành KHÔNG vì khuếch đại giai thừa ở bên phải không thể khớp với cấu trúc bên trái. 

Đối với các mảng trong đó tất cả các phần tử có chung ước số như 2, gcd trở thành ít nhất là 2 và thuật toán xuất ra NO. Tính toán gcd trực tiếp nắm bắt điều kiện này mà không cần kiểm tra hành vi giai thừa.
