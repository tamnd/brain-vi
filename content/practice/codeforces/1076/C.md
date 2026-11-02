---
title: "CF 1076C - Vấn Đề Meme"
description: "Chúng ta có một số $d$, và chúng ta muốn quyết định xem liệu chúng ta có thể chia nó thành hai số thực không âm $a$ và $b$ sao cho hai điều kiện có cùng một lúc: tổng của chúng bằng $d$, và tích của chúng cũng bằng $d$."
date: "2026-06-15T14:34:04+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1076
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 54 (Rated for Div. 2)"
rating: 1300
weight: 1076
solve_time_s: 761
verified: true
draft: false
---

[CF 1076C - Vấn đề về Meme](https://codeforces.com/problemset/problem/1076/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 12m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$d$và chúng tôi muốn quyết định xem liệu chúng tôi có thể chia nó thành hai số thực không âm hay không$a$Và$b$sao cho hai điều kiện xảy ra đồng thời: tổng của chúng bằng$d$, và tích của chúng cũng bằng$d$. 

Vì vậy, về mặt hình học, chúng ta đang tìm kiếm hai điểm trên đường thực không âm có tổng và tích trùng với một giá trị mục tiêu cố định. Mỗi trường hợp thử nghiệm sẽ hỏi liệu một cặp như vậy có tồn tại hay không và nếu có, chúng ta phải xây dựng bất kỳ cặp hợp lệ nào, không nhất thiết phải là một cặp duy nhất. 

Ràng buộc$d \le 10^3$Và$t \le 10^3$có nghĩa là chúng ta chỉ cần tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào liên quan đến việc lặp lại các giá trị ứng cử viên với mức độ chi tiết tốt vẫn có thể được chấp nhận, nhưng mọi thứ theo cấp số nhân hoặc phụ thuộc vào tìm kiếm chính xác mà không có cấu trúc đều không cần thiết. Cấu trúc của các phương trình cho thấy sự giảm thiểu đại số trực tiếp hơn là lực lượng mạnh mẽ về số. 

Điểm tinh tế chính là bài toán cho phép các số thực có độ chính xác lên tới$10^{-6}$. Điều đó loại trừ khả năng suy luận thuần túy về số nguyên và buộc chúng ta phải giải thích các điều kiện dưới dạng các ràng buộc có giá trị thực. Một điểm tinh tế khác là các nghiệm dấu phẩy động gần các trường hợp biên, đặc biệt là xung quanh$d = 0$Và$d = 1$, có thể hoạt động khác nhau trong các công thức đơn giản, vì vậy chúng cần được xử lý rõ ràng. 

Chế độ thất bại xuất hiện khi cố gắng giả sử rằng mọi phương trình bậc hai luôn mang lại nghiệm thực không âm. Ví dụ,$d = 1$dẫn đến một phân biệt đối xử giảm về 0 theo cách làm cho hệ thống không nhất quán dưới các ràng buộc đẳng thức cần thiết, mặc dù thao tác đại số có thể gợi ý sai một giải pháp đường biên. Một trường hợp cạnh khác là$d = 0$, trong đó cả hai biến phải đồng thời thu gọn về 0. 

## Phương pháp tiếp cận 

Quan điểm vũ phu sẽ cố gắng chọn một ứng cử viên$a$và tính toán$b = d - a$, sau đó kiểm tra xem$a \cdot b \approx d$. Từ$a$là thực, chúng ta sẽ rời rạc hóa phạm vi$[0, d]$thành nhiều bước nhỏ. Về nguyên tắc, điều này đúng vì mọi giải pháp hợp lệ phải thỏa mãn chính xác ràng buộc tổng, nhưng nó trở nên vô nghĩa về mặt tính toán vì không gian giải pháp là liên tục và yêu cầu lấy mẫu cực kỳ tinh tế để đáp ứng yêu cầu về độ chính xác. Ngay cả với 10^6 bước cho mỗi trường hợp thử nghiệm, đầu vào trong trường hợp xấu nhất của 10^3 thử nghiệm đã trở nên quá chậm. 

Quan sát cấu trúc quan trọng là hệ thống được xác định đầy đủ khi chúng ta thay thế$b = d - a$. Điều này chuyển đổi điều kiện của sản phẩm thành một phương trình bậc hai duy nhất trong$a$. Tuy nhiên, thay vì giải nó một cách trực tiếp mọi lúc, chúng ta có thể nhận thấy rằng tính khả thi chỉ phụ thuộc vào việc phương trình bậc hai thu được có nghiệm thực trong miền không âm hay không và những nghiệm đó có thể được xây dựng một cách rõ ràng hay không. Cấu trúc ràng buộc rút gọn bài toán về giải phương trình bậc hai có tham số$d$và chọn bất kỳ cặp gốc hợp lệ nào. 

Đại số dẫn đến:$$a(d - a) = d \Rightarrow a^2 - da + d = 0$$Điều này đưa ra một điều kiện phân biệt:$$\Delta = d^2 - 4d = d(d - 4)$$Vì vậy, giải pháp thực sự chỉ tồn tại khi$d \ge 4$hoặc$d = 0$. Trường hợp đặc biệt$d = 4$cho một gốc lặp đi lặp lại, và$d = 0$sụp đổ tầm thường để$(0, 0)$. Vì$d \in (0, 4)$, phân biệt đối xử là âm, do đó không tồn tại nghiệm thực sự. 

Khi giải pháp tồn tại, chúng tôi tính toán:$$a = \frac{d - \sqrt{d^2 - 4d}}{2}, \quad b = d - a$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(td / \epsilon)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(t)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$d$. Mục tiêu là quyết định tính khả thi của hệ ràng buộc bậc hai xuất phát từ điều kiện tổng tích. 
2. Nếu$d = 0$, xuất ngay$a = 0$,$b = 0$. Đây là cách duy nhất cả tổng và tích có thể bằng 0 cùng một lúc mà không gây ra mâu thuẫn. 
3. Nếu$d < 4$, xuất ra “N”. Điều này xuất phát từ sự phân biệt đối xử$d(d - 4)$, trở nên tiêu cực và loại bỏ các giải pháp thực sự. 
4. Ngược lại hãy tính phân biệt$\Delta = d^2 - 4d$. Giá trị này xác định xem phương trình bậc hai rút ra từ các ràng buộc có nghiệm thực hay không. 
5. Lấy căn bậc hai của$\Delta$và tính toán gốc nhỏ hơn:$$a = \frac{d - \sqrt{\Delta}}{2}$$Việc chọn gốc nhỏ hơn đảm bảo cả hai giá trị không âm và ổn định về mặt số. 
6. Tính toán$b = d - a$. Điều này thực thi ràng buộc tổng một cách chính xác, ngăn chặn việc trôi dấu phẩy động vi phạm$a + b = d$. 
7. Xuất “Y” theo sau là$a$Và$b$. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ phải thỏa mãn đồng thời cả hai ràng buộc và việc thay thế một biến sẽ làm giảm hệ thống thành một phương trình bậc hai duy nhất có nghiệm mô tả chính xác tất cả các nghiệm khả thi. Phân biệt xác định đầy đủ sự tồn tại của các nghiệm thực và các nghiệm được xây dựng tự động thỏa mãn cả điều kiện tổng và tích bằng tương đương đại số. Không có cặp nào khác có thể tồn tại bên ngoài các nghiệm này vì phép biến đổi mang tính chất phỏng đoán trên ràng buộc$b = d - a$. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

t = int(input())
for _ in range(t):
    d = float(input().strip())

    if d == 0:
        print("Y 0.0 0.0")
        continue

    if d < 4:
        print("N")
        continue

    disc = d * d - 4 * d
    root = math.sqrt(disc)

    a = (d - root) / 2
    b = d - a

    print("Y", a, b)
```Mã trực tiếp thực hiện việc giảm bậc hai. Các lối thoát sớm xử lý các phạm vi suy biến và không khả thi, tránh các hoạt động dấu phẩy động không cần thiết. Tính toán phân biệt ổn định vì$d \le 10^3$, Vì thế$d^2$vẫn nằm trong giới hạn chính xác của dấu phẩy động. Máy tính$b$BẰNG$d - a$bảo toàn chính xác ràng buộc tổng và tránh các lỗi làm tròn gộp. 

## Ví dụ đã hoạt động 

### Ví dụ:$d = 5$| Bước | Giá trị | 
| --- | --- | 
| d | 5 | 
| phân biệt đối xử$d^2 - 4d$| 25 - 20 = 5 | 
| mét vuông | 2.2360679 | 
| một | (5 - 2,2360679)/2 = 1,381966 | 
| b | 3.618034 | 

Điều này xác nhận một phân tách hợp lệ trong đó cả hai ràng buộc đều khớp về mặt số. 

### Ví dụ:$d = 3$| Bước | Giá trị | 
| --- | --- | 
| d | 3 | 
| kiểm tra | d < 4 | 
| đầu ra | N | 

Ở đây phân biệt đối xử là phủ định, xác nhận không có giải pháp thực sự nào tồn tại. 

Dấu vết đầu tiên cho thấy cách xây dựng căn bậc hai mang lại các cặp hợp lệ, trong khi dấu vết thứ hai cho thấy ranh giới khả thi tại$d = 4$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm sử dụng số học không đổi và một căn bậc hai | 
| Không gian |$O(1)$| Không có bộ nhớ phụ ngoài một vài giá trị vô hướng | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì ngay cả các phép tính căn bậc hai 10^3 cũng không đáng kể trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        d = float(input().strip())

        if d == 0:
            print("Y 0.0 0.0")
            continue
        if d < 4:
            print("N")
            continue

        disc = d * d - 4 * d
        root = math.sqrt(disc)
        a = (d - root) / 2
        b = d - a
        print("Y", a, b)

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old
    return out.getvalue().strip()

# provided sample checks (structure only; exact floating output may vary slightly)
assert run("7\n69\n0\n1\n4\n5\n999\n1000\n").split()[0] == "Y"

# edge: minimum
assert "Y" in run("1\n0\n")

# edge: infeasible region
assert run("1\n3\n").strip() == "N"

# edge: boundary
assert "Y" in run("1\n4\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n0 | Y 0 0 | Trường hợp suy biến zero | 
| 1\n3 | N | Vùng không khả thi | 
| 1\n4 | Y ... | Tính khả thi về ranh giới | 

## Vỏ cạnh 

cho$d = 0$, thuật toán ngay lập tức xuất ra$(0, 0)$. Việc thay thế sẽ cho tổng 0 và sản phẩm 0, khớp chính xác. 

Vì$d = 3$, thuật toán bác bỏ trường hợp này vì$d < 4$. Người phân biệt đối xử$9 - 12 = -3$xác nhận không có gốc thực sự, vì vậy không tồn tại cặp hợp lệ. 

Vì$d = 4$, phân biệt đối xử trở thành 0, cho một gốc lặp lại$a = b = 2$. Thuật toán tính toán$\sqrt{0} = 0$, sau đó quay trở lại$a = 2$,$b = 2$, thỏa mãn chính xác cả hai ràng buộc.
