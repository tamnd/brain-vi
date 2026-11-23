---
title: "CF 58B - Tiền xu"
description: "Chúng ta được cho giá trị của đồng xu có mệnh giá lớn nhất, n. Chúng ta phải xây dựng một chuỗi các giá trị đồng xu riêng biệt sao cho mọi đồng xu lớn hơn đều có thể chia hết cho mọi đồng xu nhỏ hơn. Trong số tất cả các chuỗi hợp lệ, chúng tôi muốn chuỗi chứa số lượng xu tối đa có thể."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 58
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 54 (Div. 2)"
rating: 1300
weight: 58
solve_time_s: 111
verified: false
draft: false
---

[CF 58B - Tiền xu](https://codeforces.com/problemset/problem/58/B) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được trao giá trị của đồng xu có mệnh giá lớn nhất,`n`. Chúng ta phải xây dựng một chuỗi các giá trị đồng xu riêng biệt sao cho mọi đồng xu lớn hơn đều có thể chia hết cho mọi đồng xu nhỏ hơn. Trong số tất cả các chuỗi hợp lệ, chúng tôi muốn chuỗi chứa số lượng xu tối đa có thể. 

Đầu ra phải liệt kê các mệnh giá từ lớn nhất đến nhỏ nhất. 

Điều kiện phân chia tạo ra một cấu trúc dây chuyền. Nếu đồng xu là:```
a1 > a2 > a3 > ...
```thì mọi giá trị trước đó phải chia hết cho mọi giá trị sau. Đặc biệt:```
a1 % a2 == 0
a2 % a3 == 0
a1 % a3 == 0
```và vân vân. 

Giới hạn chỉ là`10^6`, đủ nhỏ để phân chia thử nghiệm và các phép toán nhân tử lặp lại. Thậm chí một`O(sqrt(n))`thuật toán dễ dàng đủ nhanh trong vòng 2 giây. Mặt khác, việc tạo và kiểm tra tất cả các tập hợp con hoặc tất cả các chuỗi ước số sẽ bùng nổ về mặt tổ hợp và không cần thiết. 

Phần khó khăn là hiểu "số lượng xu tối đa" thực sự có nghĩa là gì. Một giải pháp bất cẩn có thể cố gắng nhảy trực tiếp từ`n`ĐẾN`1`, nhưng điều đó làm lãng phí cơ hội chèn các ước số trung gian. 

Ví dụ: 

đầu vào:```
12
```Một chuỗi hợp lệ là:```
12 1
```nhưng nó không phải là tối đa. Chúng ta có thể chèn thêm giá trị:```
12 6 3 1
```Một lỗi phổ biến khác là chia cho các ước số tùy ý thay vì chia cho thừa số nguyên tố nhỏ nhất có thể. Giả sử chúng ta bắt đầu từ`60`. 

Nếu chúng ta chia rẽ mạnh mẽ:```
60 -> 10 -> 1
```chúng tôi chỉ nhận được ba đồng xu. 

Nếu chúng ta loại bỏ từng yếu tố một:```
60 -> 30 -> 15 -> 5 -> 1
```chuỗi trở nên dài hơn. 

Trường hợp cạnh`n = 1`cũng dễ bị nhầm lẫn. Không có ước số dương nhỏ hơn để cộng, nên câu trả lời đơn giản là:```
1
```## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các ước số của`n`, sau đó tìm kiếm chuỗi chia hết dài nhất trong số chúng. Vì mọi cặp trong chuỗi phải chia chính xác nên chúng ta có thể sắp xếp các ước số và chạy lập trình động tương tự như dãy con tăng dài nhất. 

Điều này hoạt động vì mối quan hệ chia hết tạo thành một trật tự một phần. Nếu như`a`chia rẽ`b`, sau đó`a`có thể xuất hiện sau`b`trong trình tự. 

Vấn đề là cách tiếp cận này giải quyết được một vấn đề khó hơn nhiều so với mức cần thiết. Mặc dù`n ≤ 10^6`giữ cho số chia có thể quản lý được, việc xây dựng tất cả các chuỗi và chuyển tiếp vẫn là công việc không cần thiết. 

Quan sát quan trọng là việc tối đa hóa số lượng xu có nghĩa là chúng ta nên giảm số lượng càng chậm càng tốt. 

Giả sử đồng tiền hiện tại là`x`. Đồng xu tiếp theo phải là ước số chính xác của`x`. Để tối đa hóa số bước còn lại, chúng tôi muốn giá trị tiếp theo ở mức lớn nhất có thể trong khi vẫn nhỏ hơn`x`. 

Cách tốt nhất là chia cho thừa số nguyên tố nhỏ nhất của`x`. 

Ví dụ:```
60 -> 30
```tốt hơn là:```
60 -> 20
```bởi vì chỉ loại bỏ một thừa số nguyên tố sẽ bảo toàn được nhiều bước chia hết hơn sau này. 

Mỗi thao tác sẽ loại bỏ chính xác một thừa số nguyên tố khỏi hệ số của số đó. Vì một số có hệ số hóa```
n = p1^a1 * p2^a2 * ...
```chứa chính xác`a1 + a2 + ...`thừa số nguyên tố tính bội số, chuỗi dài nhất có thể sẽ loại bỏ chúng lần lượt cho đến khi đạt`1`. 

Vì vậy, thuật toán trở nên đơn giản: 

Bắt đầu từ`n`. Chia liên tục cho thừa số nguyên tố nhỏ nhất và in từng giá trị trung gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d²) trong đó d là số ước | O(d) | Phức tạp không cần thiết | 
| Tối ưu | O(sqrt(n)) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. In giá trị hiện tại của`n`vì dãy số phải bắt đầu từ mệnh giá lớn nhất. 
3. Trong khi`n`lớn hơn`1`, tìm ước số nhỏ nhất của`n`lớn hơn`1`. 

Chúng tôi tìm kiếm từ`2`hướng lên trên cho đến khi tìm được số chia. Ước số đầu tiên tìm được luôn là thừa số nguyên tố nhỏ nhất. 
4. Chia`n`bởi thừa số nguyên tố nhỏ nhất này. 

Điều này loại bỏ chính xác một thừa số nguyên tố khỏi số, giúp giảm chậm nhất có thể và tối đa hóa độ dài chuỗi còn lại. 
5. In giá trị mới của`n`. 
6. Lặp lại cho đến khi`n`trở thành`1`. 

Ở mỗi bước, giá trị mới chia cho giá trị trước đó vì nó có được bằng cách chia cho một hệ số nguyên. Vì khả năng chia hết có tính bắc cầu nên mọi đồng xu lớn hơn vẫn chia hết cho mọi đồng xu nhỏ hơn ở sau chuỗi. 

### Tại sao nó hoạt động 

Chuỗi dài nhất có được bằng cách loại bỏ từng thừa số nguyên tố một. 

Giả sử chúng ta có số hiện tại`x`. Mọi giá trị tiếp theo phải là:```
x / d
```Ở đâu`d`là số chia lớn hơn`1`. 

Nếu như`d`là hợp số, khi đó việc loại bỏ nó sẽ loại bỏ nhiều thừa số nguyên tố cùng một lúc. Điều đó rút ngắn chuỗi có thể vì vẫn còn ít ước số trung gian hơn. 

Phép chia cho thừa số nguyên tố nhỏ nhất sẽ loại bỏ chính xác một thừa số nguyên tố và giữ số tiếp theo càng lớn càng tốt. Việc lặp lại quá trình này đảm bảo rằng mỗi thừa số nguyên tố đóng góp thêm một đồng xu vào chuỗi, đó là độ dài tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

ans = []

while True:
    ans.append(str(n))

    if n == 1:
        break

    for d in range(2, int(n ** 0.5) + 1):
        if n % d == 0:
            n //= d
            break
    else:
        n = 1

print(" ".join(ans))
```Giải pháp duy trì mệnh giá hiện tại trong`n`và liên tục thêm nó vào chuỗi câu trả lời. 

Vòng lặp chỉ dừng sau khi in`1`, từ`1`cũng phải xuất hiện như đồng xu cuối cùng. 

Vòng lặp bên trong tìm kiếm ước số nhỏ nhất bắt đầu từ`2`. Ước số đầu tiên được tìm thấy tự động là thừa số nguyên tố nhỏ nhất. Chia cho nó sẽ loại bỏ chính xác một thừa số nguyên tố khỏi phép nhân tử. 

các`else`gắn liền với`for`vòng lặp là quan trọng. Nó chỉ chạy nếu không tìm thấy ước số. Điều đó có nghĩa là hiện tại`n`là nguyên tố. Số nguyên tố chỉ có một ước số thực sự là`1`, vì vậy bước tiếp theo phải trở thành`1`. 

Một lỗi phổ biến là quên in giá trị ban đầu trước khi bắt đầu chia. Một lỗi dễ xảy ra khác là dừng ngay lập tức khi`n`trở thành số nguyên tố, sẽ bỏ qua trận chung kết`1`. 

Giới hạn căn bậc hai giúp tìm kiếm số chia hiệu quả. Từ`n`co lại sau mỗi bước, tổng thời gian chạy vẫn rất nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
```| Hiện tại | Hệ số nguyên tố nhỏ nhất | Tiếp theo | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 10 | 2 | 5 | 10 | 
| 5 | nguyên tố | 1 | 10 5 | 
| 1 | - | - | 10 5 1 | 

Trình tự trở thành:```
10 5 1
```Dấu vết này cho thấy các số nguyên tố ngay lập tức chuyển sang`1`, vì chúng không có ước số trung gian. 

### Ví dụ 2 

đầu vào:```
72
```| Hiện tại | Hệ số nguyên tố nhỏ nhất | Tiếp theo | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 72 | 2 | 36 | 72 | 
| 36 | 2 | 18 | 72 36 | 
| 18 | 2 | 9 | 72 36 18 | 
| 9 | 3 | 3 | 72 36 18 9 | 
| 3 | nguyên tố | 1 | 72 36 18 9 3 | 
| 1 | - | - | 72 36 18 9 3 1 | 

Đầu ra cuối cùng:```
72 36 18 9 3 1
```Ví dụ này thể hiện ý tưởng tham lam cốt lõi. Thuật toán loại bỏ một thừa số nguyên tố tại một thời điểm, tạo ra số mệnh giá tối đa có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(n)) | Mỗi bước tìm kiếm các ước số lên tới sqrt(n hiện tại) và n giảm nhanh | 
| Không gian | O(1) không bao gồm đầu ra | Chỉ có một vài biến được sử dụng | 

Với`n ≤ 10^6`, điều này dễ dàng nằm trong giới hạn. Ngay cả trong trường hợp xấu nhất, việc kiểm tra số chia vẫn nhỏ vì số lượng giảm đi sau mỗi phép chia. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())

    ans = []

    while True:
        ans.append(str(n))

        if n == 1:
            break

        for d in range(2, int(n ** 0.5) + 1):
            if n % d == 0:
                n //= d
                break
        else:
            n = 1

    return " ".join(ans)

# provided sample
assert run("10\n") == "10 5 1", "sample 1"

# minimum value
assert run("1\n") == "1", "n = 1"

# prime number
assert run("13\n") == "13 1", "prime input"

# power of two
assert run("16\n") == "16 8 4 2 1", "repeated smallest factor"

# mixed prime factors
assert run("60\n") == "60 30 15 5 1", "multiple distinct factors"

# maximum bound style case
out = run("1000000\n")
assert out.startswith("1000000 "), "large input handled"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào nhỏ nhất có thể | 
|`13`|`13 1`| Số nguyên tố nhảy thẳng tới 1 | 
|`16`|`16 8 4 2 1`| Thừa số nguyên tố lặp lại | 
|`60`|`60 30 15 5 1`| Tham lam loại bỏ từng yếu tố một | 
|`1000000`| Chuỗi dài hợp lệ | Hiệu suất gần giới hạn trên | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất:```
1
```Thuật toán ngay lập tức nối thêm`1`đến câu trả lời và dừng lại. Không có tìm kiếm ước số nào xảy ra vì không có ước số dương nhỏ hơn. Đầu ra là chính xác:```
1
```Bây giờ hãy xem xét một số nguyên tố:```
13
```Tìm kiếm ước số kiểm tra tất cả các số nguyên từ`2`ĐẾN`sqrt(13)`và không tìm thấy gì các`for-else`kích hoạt nhánh, đặt giá trị tiếp theo thành`1`. 

Trình tự trở thành:```
13 1
```Điều này là tối ưu vì số nguyên tố không có ước nào khác ngoài`1`. 

Cuối cùng, hãy xem xét một số có nhiều ước số lặp lại:```
16
```Việc thực hiện diễn ra:```
16 -> 8 -> 4 -> 2 -> 1
```Mỗi bước chia cho`2`, mỗi lần chỉ loại bỏ một thừa số nguyên tố. Một giải pháp bất cẩn chia cho các yếu tố lớn hơn có thể tạo ra:```
16 -> 4 -> 1
```đó là hợp lệ nhưng không tối đa. Chiến lược tham lam bảo toàn chính xác chuỗi dài nhất có thể.
