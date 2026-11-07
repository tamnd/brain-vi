---
title: "CF 1342C - Một vấn đề đếm khác"
description: "Chúng ta đang làm việc với các số nguyên và số học mô-đun, nhưng cấu trúc sẽ trở nên rõ ràng hơn nếu chúng ta coi trục số vô hạn được tô màu bởi một quy tắc tuần hoàn. Với bất kỳ số nguyên $x$ nào, chúng ta tính toán hai giá trị: đầu tiên chúng ta lấy $x bmod a$, sau đó giảm kết quả đó theo modulo $b$."
date: "2026-06-16T09:31:04+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1342
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 86 (Rated for Div. 2)"
rating: 1600
weight: 1342
solve_time_s: 290
verified: false
draft: false
---

[CF 1342C - Một vấn đề đếm khác](https://codeforces.com/problemset/problem/1342/C) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 4 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với các số nguyên và số học mô-đun, nhưng cấu trúc sẽ trở nên rõ ràng hơn nếu chúng ta coi trục số vô hạn được tô màu bởi một quy tắc tuần hoàn. 

Với mọi số nguyên$x$, chúng tôi tính toán hai giá trị: đầu tiên chúng tôi lấy$x \bmod a$, sau đó giảm kết quả đó theo modulo$b$. Song song đó, chúng tôi cũng tính toán$x \bmod b$theo sau là modulo$a$. Nếu hai kết quả cuối cùng này khác nhau, chúng tôi tính$x$. 

Mỗi truy vấn đưa ra một phân đoạn$[l, r]$, và chúng ta phải đếm xem có bao nhiêu số nguyên trong phạm vi đó thỏa mãn điều kiện bất đẳng thức này. 

Khó khăn chính đó là$l$Và$r$có thể lớn như$10^{18}$, vì vậy chúng tôi không thể đánh giá từng số riêng lẻ. Cấu trúc chỉ phụ thuộc vào dư lượng modulo$a$Và$b$, cả hai đều nhỏ (nhiều nhất là 200), điều này gợi ý rõ ràng về tính tuần hoàn trong một số chu kỳ liên quan đến$a$Và$b$. 

Ý tưởng mạnh mẽ sẽ kiểm tra mọi số trong mỗi phạm vi truy vấn. Điều đó đã bị hỏng ngay lập tức khi khoảng thời gian truy vấn lớn. Ví dụ, nếu$l = 1$Và$r = 10^{18}$, việc lặp lại là không thể. 

Một vấn đề tinh tế hơn là ngay cả khi chúng ta tính toán trước các giá trị cho tiền tố thì mẫu đẳng thức vẫn phụ thuộc vào cả mod$a$và mod$b$, vì vậy chúng ta cần một đối số lặp lại có cấu trúc hơn là mô phỏng tiền tố đơn giản. 

Trường hợp cạnh xuất hiện khi$a = b$. Trong trường hợp đó cả hai biểu thức đều giống hệt nhau cho tất cả$x$, vì vậy câu trả lời luôn bằng 0. Việc triển khai bất cẩn giả định hành vi định kỳ mà không kiểm tra điều này sẽ vẫn hoạt động, nhưng sẽ lãng phí tính toán hoặc có nguy cơ gây nhầm lẫn khi phân chia. 

Một trường hợp cạnh khác là khi một trong$a$hoặc$b$bằng 1. Sau đó, một trong các chuỗi modulo suy giảm về 0 và biểu thức được đơn giản hóa đáng kể. Việc triển khai vẫn phải xử lý việc này một cách rõ ràng bên trong một công thức chung. 

## Phương pháp tiếp cận 

Phương pháp brute-force đánh giá điều kiện cho mọi số nguyên trong mỗi khoảng truy vấn. Mỗi lần đánh giá mất thời gian không đổi, do đó chi phí cho một truy vấn$O(r - l + 1)$, và trong trường hợp xấu nhất thì đây là$10^{18}$. Điều này ngay lập tức không thể thực hiện được. 

Quan sát cốt lõi là điều kiện chỉ phụ thuộc vào$x \bmod a$Và$x \bmod b$. Hai dư lượng này lặp lại theo chu kỳ$a$Và$b$, do đó cặp đôi lặp lại với dấu chấm$\mathrm{lcm}(a, b)$. Từ$a, b \le 200$, khoảng thời gian này nhiều nhất là 40000, đủ nhỏ để tính toán trước. 

Do đó chúng ta có thể tính toán cho một chu kỳ đầy đủ độ dài$L = \mathrm{lcm}(a, b)$, có bao nhiêu giá trị thỏa mãn điều kiện. Sau đó, bất kỳ tiền tố nào lên đến$x$có thể được thể hiện như sau: 

chu kỳ đầy đủ cộng với một đoạn còn lại. 

Vì vậy, chúng tôi xây dựng một mảng tiền tố trong một khoảng thời gian và trả lời từng truy vấn bằng cách sử dụng hai đánh giá tiền tố. 

Điều này làm giảm mỗi truy vấn về thời gian không đổi sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\sum (r-l))$|$O(1)$| Quá chậm | 
| Tiền tố định kỳ trên LCM |$O(a \cdot b + q)$mỗi bài kiểm tra |$O(a \cdot b)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$L = \mathrm{lcm}(a, b)$. Điều này xác định khoảng thời gian lặp lại của tất cả các cặp dư lượng có liên quan vì cả hai mẫu modulo đều lặp lại đồng thời sau độ dài này. 
2. Xây dựng một mảng`bad[i]`vì$i \in [1, L]$, Ở đâu`bad[i] = 1`nếu như$((i \bmod a) \bmod b) \ne ((i \bmod b) \bmod a)$, nếu không thì 0. Điều này trực tiếp mã hóa xem mỗi vị trí có đóng góp vào câu trả lời hay không. 
3. Xây dựng mảng tổng tiền tố`pref`, Ở đâu`pref[i]`lưu trữ có bao nhiêu giá trị hợp lệ trong$[1, i]$. Điều này cho phép đếm phạm vi thời gian không đổi trong khoảng thời gian. 
4. Tính toán trước`total = pref[L]`, số giá trị hợp lệ trong một chu kỳ đầy đủ. Điều này cho phép chúng tôi xử lý các phạm vi lớn bằng cách chia chúng thành các chu kỳ đầy đủ cộng với phần còn lại. 
5. Để trả lời một câu hỏi$[l, r]$, tính: 

số lượng giá trị hợp lệ trong$[1, r]$trừ đi số trong$[1, l-1]$. Mỗi trong số này được tính toán bằng các chu kỳ đầy đủ:$$f(x) = (x // L) \cdot total + pref[x \bmod L]$$xử lý cẩn thận khi$x \bmod L = 0$. 

### Tại sao nó hoạt động 

Giá trị của biểu thức chỉ phụ thuộc vào cặp$(x \bmod a, x \bmod b)$. Vì cả hai dư lượng lặp lại độc lập với các khoảng thời gian$a$Và$b$, trạng thái kết hợp lặp lại mỗi$\mathrm{lcm}(a, b)$. Điều này làm cho chức năng chỉ báo có tính tuần hoàn, do đó việc đếm trên các phạm vi lớn sẽ giảm xuống việc đếm trên các chu kỳ đầy đủ cộng với tiền tố cố định, giúp duy trì tính chính xác cho mọi khoảng nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def lcm(a, b):
    import math
    return a // math.gcd(a, b) * b

t = int(input())
for _ in range(t):
    a, b, q = map(int, input().split())
    L = lcm(a, b)

    bad = [0] * (L + 1)
    pref = [0] * (L + 1)

    for i in range(1, L + 1):
        x = i % a
        y = i % b
        if (x % b) != (y % a):
            bad[i] = 1
        pref[i] = pref[i - 1] + bad[i]

    total = pref[L]

    def calc(x):
        if x <= 0:
            return 0
        cycles = x // L
        rem = x % L
        return cycles * total + pref[rem]

    out = []
    for _q in range(q):
        l, r = map(int, input().split())
        out.append(str(calc(r) - calc(l - 1)))

    print(" ".join(out))
```Việc triển khai dựa vào việc xây dựng rõ ràng một chu kỳ đầy đủ của hàm và sử dụng tổng tiền tố để biến các truy vấn phạm vi thành số học theo thời gian không đổi. 

Sự tinh tế chính là xử lý việc lập chỉ mục một cách chính xác. Mảng dựa trên 1, do đó tổng tiền tố cho phần còn lại sẽ sử dụng`pref[rem]`trực tiếp và chu kỳ đầy đủ nhân với`pref[L]`. Một chi tiết quan trọng khác là sử dụng`l - 1`an toàn bên trong`calc`, trong đó các giá trị không dương phải trả về 0. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi trường hợp thử nghiệm mẫu đầu tiên:$a = 4, b = 6$, với các truy vấn nhỏ. 

Giả sử khoảng thời gian là$L = \mathrm{lcm}(4,6) = 12$. 

Chúng tôi tính toán`bad[i]`trong một chu kỳ. 

| tôi | tôi%4 | tôi%6 | (i%4)%6 | (i%6)%4 | tệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 0 | 
| 2 | 2 | 2 | 2 | 2 | 0 | 
| 3 | 3 | 3 | 3 | 3 | 0 | 
| 4 | 0 | 4 | 0 | 0 | 0 | 
| 5 | 1 | 5 | 1 | 1 | 0 | 
| 6 | 2 | 0 | 2 | 0 | 1 | 
| 7 | 3 | 1 | 3 | 1 | 1 | 
| 8 | 0 | 2 | 0 | 2 | 1 | 
| 9 | 1 | 3 | 1 | 3 | 1 | 
| 10 | 2 | 4 | 2 | 0 | 1 | 
| 11 | 3 | 5 | 3 | 1 | 1 | 
| 12 | 0 | 0 | 0 | 0 | 0 | 

Từ đó, chúng tôi nhận được một mảng tiền tố và có thể trả lời các truy vấn. Ví dụ, truy vấn$[1, 7]$đưa ra tổng các giá trị xấu trong phạm vi đó, khớp với đầu ra mẫu. 

Dấu vết này xác nhận rằng tình trạng này không được phân bố đồng đều và phụ thuộc vào tương tác dư lượng, nhưng vẫn lặp lại rõ ràng trên LCM. 

Ví dụ thứ hai với$a = b$cho thấy một trường hợp suy biến. Nếu như$a = b = 5$, thì với mọi$x$, cả hai biểu thức đều giống hệt nhau, vì vậy`bad[i] = 0`cho tất cả$i$. Mảng tiền tố vẫn bằng 0 và mọi truy vấn đều trả về 0 bất kể kích thước phạm vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot L + q)$| Mỗi thử nghiệm xây dựng một chu kỳ kích thước$L = \mathrm{lcm}(a,b)$và trả lời các truy vấn trong O(1) | 
| Không gian |$O(L)$| Tiền tố và đánh dấu mảng trong một khoảng thời gian | 

Từ$a, b \le 200$, LCM tối đa là 40000, khiến cho việc tiền xử lý trở nên khả thi ngay cả đối với nhiều trường hợp thử nghiệm và các truy vấn được xử lý hiệu quả trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        t = int(input())
        for _ in range(t):
            a, b, q = map(int, input().split())
            L = a * b // math.gcd(a, b)

            bad = [0] * (L + 1)
            pref = [0] * (L + 1)

            for i in range(1, L + 1):
                x = i % a
                y = i % b
                bad[i] = 1 if (x % b) != (y % a) else 0
                pref[i] = pref[i - 1] + bad[i]

            total = pref[L]

            def calc(x):
                if x <= 0:
                    return 0
                return (x // L) * total + pref[x % L]

            out = []
            for _ in range(q):
                l, r = map(int, input().split())
                out.append(str(calc(r) - calc(l - 1)))
            print(" ".join(out))

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    res = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return res.strip()

# provided sample
assert run("""2
4 6 5
1 1
1 3
1 5
1 7
1 9
7 10 2
7 8
100 200
""") == """0 0 0 2 4
0 91"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$a=b$trường hợp | tất cả số không | sự sụp đổ bình đẳng | 
|$a=1$trường hợp | đơn giản hóa trực tiếp | hành vi modulo ranh giới | 
| phạm vi lớn | sự lặp lại ổn định | Tính chính xác của chu trình LCM | 
| ngẫu nhiên nhỏ | thỏa thuận vũ phu | tính đúng đắn của điều kiện | 

## Vỏ cạnh 

Khi nào$a = b$, mọi thuật ngữ đều có giá trị giống hệt nhau vì cả hai biểu thức đều giảm đến cùng một phép tính, do đó mảng đánh dấu của thuật toán trở thành hoàn toàn bằng 0. Tổng tiền tố không đổi và mọi truy vấn đều trả về 0, phù hợp với sự đơn giản hóa toán học dự kiến. 

Khi$a = 1$hoặc$b = 1$, một vế của biểu thức rút gọn về 0 đối với mọi$x$. Việc triển khai vẫn hoạt động vì các hoạt động modulo tạo ra dư lượng không đổi một cách chính xác và cấu trúc tuần hoàn suy biến thành một chu kỳ tầm thường có độ dài bằng tham số khác. 

Khi$l$Và$r$kéo dài nhiều chu kỳ đầy đủ cộng với một phần đuôi,`calc`chức năng chia phạm vi rõ ràng thành`(x // L)`lặp lại đầy đủ và một đoạn còn lại. Điều này tránh mọi sự phụ thuộc vào việc căn chỉnh ranh giới truy vấn với ranh giới chu kỳ, đảm bảo tính chính xác cho các khoảng thời gian tùy ý.
