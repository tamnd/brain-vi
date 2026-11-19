---
title: "CF 2092A - Kamilka và Cừu"
description: "Chúng tôi được đưa cho một danh sách các con cừu, mỗi con có một vẻ đẹp khác nhau. Kamilka có thể cho mỗi con cừu ăn thêm một số bó cỏ như nhau, tăng hiệu quả tất cả các cấp độ vẻ đẹp với cùng một số nguyên $d ge 0$."
date: "2026-06-08T05:41:54+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2092
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1014 (Div. 2)"
rating: 800
weight: 2092
solve_time_s: 77
verified: true
draft: false
---

[CF 2092A - Kamilka và chú cừu](https://codeforces.com/problemset/problem/2092/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học, lý thuyết số, sắp xếp 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một danh sách các con cừu, mỗi con có một vẻ đẹp khác nhau. Kamilka có thể cho mỗi con cừu ăn thêm một số bó cỏ như nhau, tăng hiệu quả tất cả các cấp độ sắc đẹp theo cùng một số nguyên$d \ge 0$. Sau lần cho ăn này, cô ấy phải chọn chính xác hai con cừu và niềm vui mà cô ấy nhận được là ước số chung lớn nhất (GCD) về mức độ đẹp đẽ của chúng. Nhiệm vụ là tối đa hóa GCD này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định$n$, số lượng cừu, tiếp theo là$n$số nguyên riêng biệt biểu thị mức độ vẻ đẹp. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm - GCD tối đa có thể đạt được. 

Các hạn chế ở mức vừa phải:$n$có thể lên tới 100, mức độ đẹp có thể cao tới$10^9$, và có tới 500 trường hợp thử nghiệm. Bởi vì$n$nhỏ, thuật toán với$O(n^2)$thời gian cho mỗi trường hợp thử nghiệm là khả thi, nhưng những nỗ lực mạnh mẽ để kiểm tra mọi$d$lên đến$10^9$là không thể. 

Các trường hợp quan trọng bao gồm các tình huống chỉ có hai con cừu tồn tại, điều này buộc chúng ta phải xem xét cẩn thận sự khác biệt giữa chúng. Một trường hợp khác là khi mức độ vẻ đẹp đã ở mức cao nhất; thêm đồng phục$d$có thể là cách duy nhất để tăng GCD. Ví dụ, đối với đầu vào`2\n1 3\n`, đang chọn$d=1$tạo ra GCD 2, đó là mức tối ưu. Một cách tiếp cận ngây thơ mà bỏ qua khả năng thêm đồng phục$d$sẽ xuất sai 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: với mỗi khả năng$d$, tính toán tất cả các mức độ đẹp thu được và kiểm tra GCD của từng cặp. Điều này đúng vì nó kiểm tra tất cả các cách cho ăn khả thi, nhưng nó thất bại hoàn toàn trong thực tế vì$d$có thể rất lớn, lên tới$10^9$, làm cho nó không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là xem xét cách GCD hoạt động dưới phép cộng đều. Hãy để hai con cừu được chọn có mức độ đẹp$x$Và$y$. Sau khi thêm$d$, giá trị của chúng trở thành$x+d$Và$y+d$, và GCD của cả hai là$\gcd(x+d, y+d)$. Theo các thuộc tính của GCD, điều này đơn giản hóa thành$\gcd(x+d, y+d) = \gcd(x - y, y + d)$. Điều này có nghĩa là GCD tối đa có thể đạt được luôn là ước số của một số chênh lệch$y - x$. 

Vì vậy, thay vì lặp đi lặp lại$d$, chúng ta có thể lặp lại tất cả các cặp cừu và tính toán sự khác biệt về mức độ đẹp của chúng. Đối với mỗi sự khác biệt$diff = y - x$, mọi ước của$diff$là GCD tối đa ứng cử viên. Sau đó, với mỗi ước số ứng viên, chúng ta kiểm tra xem có tồn tại một$d$sao cho việc cộng nó vào tất cả các con cừu sẽ tạo ra hai số chia hết cho số chia. Bởi vì chúng ta luôn có thể thay đổi$d$để làm cho số lớn hơn có thể chia hết cho số chia, thì luôn có thể đạt được ước số lớn nhất của bất kỳ sự khác biệt nào giữa hai con cừu. Do đó, chiến lược tối ưu quy về việc tìm ra sự khác biệt lớn nhất giữa hai con cừu bất kỳ và lấy các ước của nó. 

Điều này dẫn đến một cách đơn giản và hiệu quả$O(n^2)$thuật toán: lặp lại tất cả các cặp cừu, tính toán sự khác biệt của chúng và ghi lại sự khác biệt lớn nhất. Ước số lớn nhất của chênh lệch tối đa đó là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * max(a_i)) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi trường hợp thử nghiệm, hãy đọc số cừu$n$và mức độ vẻ đẹp của họ$a$. 
2. Khởi tạo một biến`max_beauty`đến mức độ vẻ đẹp tối đa và`min_beauty`đến mức tối thiểu. Sự khác biệt`max_beauty - min_beauty`đưa ra khoảng cách tối đa giữa hai con cừu bất kỳ. 
3. Niềm vui tối đa mà Kamilka có thể đạt được chính là sự khác biệt này vì chúng ta có thể lựa chọn$d = max_beauty - max(a_i)$để dịch chuyển số và làm cho GCD bằng hiệu. 
4. In giá trị này làm câu trả lời cho ca kiểm thử. 

Tại sao nó hoạt động: thêm tương tự$d$cho tất cả các số không làm thay đổi sự khác biệt giữa các số. GCD của hai số sau khi cộng$d$luôn có thể được tối đa hóa bằng cách lấy hiệu giữa số lớn nhất và số nhỏ nhất. Bởi vì sự khác biệt về mức độ vẻ đẹp quyết định tất cả các GCD có thể có, nên mức tối đa toàn cầu chính là sự khác biệt lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    max_a = max(a)
    min_a = min(a)
    print(max_a - min_a)
```Giải pháp này đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`, trích xuất số lượng cừu và mức độ đẹp của chúng, tính toán mức độ đẹp tối đa và tối thiểu và đưa ra sự khác biệt. Điều này có tác dụng vì sự khác biệt thu được GCD tối đa có thể đạt được sau khi thêm một giá trị phù hợp$d$. Giải pháp tránh các tính toán không cần thiết và hoạt động trong mọi ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 3
```| Bước | một | tối đa(a) | phút(a) | tối đa - phút | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,3] | 3 | 1 | 2 | 

Đầu ra: 2 

Giải thích: thêm$d=1$đưa ra số 2 và 4, có GCD là 2, khớp với hiệu. 

### Ví dụ 2 

đầu vào:```
5 4 3 2 1
```| Bước | một | tối đa(a) | phút(a) | tối đa - phút | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [5,4,3,2,1] | 5 | 1 | 4 | 

Đầu ra: 4 

Giải thích: cho ăn$d=0$và chọn 1 và 5 sẽ cho GCD 4, là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chỉ cần tính toán tối đa và tối thiểu trên n phần tử | 
| Không gian | O(n) | Lưu giữ đẳng cấp sắc đẹp | 

Từ$n \le 100$Và$t \le 500$, tổng số thao tác là khoảng 50.000, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res.append(str(max(a)-min(a)))
    return "\n".join(res)

# Provided samples
assert run("4\n2\n1 3\n5\n5 4 3 2 1\n3\n5 6 7\n3\n1 11 10\n") == "2\n4\n2\n10"

# Custom cases
assert run("1\n2\n1000000000 1\n") == "999999999", "large difference"
assert run("1\n3\n7 7 7\n") == "0", "all equal"
assert run("1\n2\n1 2\n") == "1", "minimum size"
assert run("1\n4\n10 1 2 3\n") == "9", "non-sorted input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1000000000 1`| 999999999 | Chênh lệch tối đa gần giới hạn trên | 
|`3\n7 7 7`| 0 | Mọi con cừu đều có vẻ đẹp như nhau | 
|`2\n1 2`| 1 | Đầu vào kích thước tối thiểu | 
|`4\n10 1 2 3`| 9 | Thuật toán hoạt động trên đầu vào chưa được sắp xếp | 

## Vỏ cạnh 

Nếu tất cả các con cừu đều có vẻ đẹp giống nhau thì sự khác biệt là bằng không. Ví dụ, đầu vào`3\n7 7 7`tạo ra đầu ra`0`. Thuật toán xác định chính xác điều này bởi vì`max(a) - min(a)`là 0. 

Nếu chỉ có hai con cừu, hãy nhập`2\n1 2`, sự khác biệt là 1. Thêm$d$không tăng GCD vượt quá 1 vì các số liên tiếp; thuật toán xử lý việc này bằng cách tính toán`max(a)-min(a)`trực tiếp, sản xuất 1. 

Nếu mức độ vẻ đẹp trải rộng trên toàn bộ phạm vi, ví dụ:`2\n1 1000000000`, thuật toán tính toán`1000000000 - 1 = 999999999`hiệu quả mà không bị tràn, vì số nguyên Python xử lý các giá trị lớn tùy ý. 

Giải pháp này đơn giản, hiệu quả và tận dụng trực tiếp đặc tính toán học mà việc tối đa hóa GCD trong phép cộng đồng đều được xác định bởi sai phân lớn nhất.
