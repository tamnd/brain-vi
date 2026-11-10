---
title: "CF 1535B - Sắp xếp lại mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và có thể hoán vị nó theo cách chúng ta muốn. Sau khi chọn thứ tự, một cặp vị trí $(i,j)$ với $i<j$ được gọi là tốt nếu $$gcd(ai, 2aj) 1.$$ Nhiệm vụ là tối đa hóa số lượng các cặp tốt. Đầu vào chứa nhiều trường hợp thử nghiệm."
date: "2026-06-10T15:40:23+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1535
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 110 (Rated for Div. 2)"
rating: 900
weight: 1535
solve_time_s: 239
verified: true
draft: false
---

[CF 1535B - Định dạng lại mảng](https://codeforces.com/problemset/problem/1535/B) 

**Xếp hạng:** 900 
**Tags:** sức mạnh vũ phu, lòng tham, toán học, lý thuyết số, sắp xếp 
**Thời gian giải:** 3 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và có thể hoán vị nó theo cách chúng ta muốn. Sau khi chọn thứ tự, một cặp vị trí$(i,j)$với$i<j$được gọi là tốt nếu$$\gcd(a_i, 2a_j) > 1.$$Nhiệm vụ là tối đa hóa số lượng các cặp tốt. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng và phải xuất ra số lượng cặp tốt nhất có thể sau khi sắp xếp lại. 

Ràng buộc quan trọng nhất là tổng kích thước của tất cả các mảng. Mặc dù có thể có tới 1000 ca kiểm thử nhưng tổng của tất cả$n$giá trị nhiều nhất là 2000. Điều đó thay đổi hoàn toàn bức tranh phức tạp. MỘT$O(n^2)$thuật toán hoàn toàn an toàn vì$$2000^2 = 4 \cdot 10^6,$$rất nhỏ trong giới hạn 2 giây. Không cần lý thuyết số nâng cao hoặc cấu trúc dữ liệu phức tạp. 

Một điểm tinh tế là điều kiện liên quan đến$2a_j$, không$a_j$. Điều này làm cho mọi số chẵn đều vô cùng có giá trị. Ví dụ: 

đầu vào:```
1
2
2 3
```Nếu chúng ta đặt mảng như$[2,3]$, sau đó$$\gcd(2, 2\cdot3)=\gcd(2,6)=2,$$vậy là cặp đôi này tốt. 

Một giải pháp bất cẩn kiểm tra$\gcd(a_i,a_j)$thay vào đó sẽ kết luận sai rằng$$\gcd(2,3)=1.$$Một sai lầm dễ mắc phải khác là cho rằng chỉ có các cặp số chẵn mới quan trọng. 

đầu vào:```
1
3
2 3 5
```Đặt hàng nó như$[2,3,5]$cho$$\gcd(2,6)=2,\qquad \gcd(2,10)=2.$$Cả hai cặp có số chẵn đứng đầu đều tốt. 

Trường hợp cạnh cuối cùng xảy ra khi tất cả các số đều là số lẻ. 

đầu vào:```
1
3
1 3 5
```Không sắp xếp lại sẽ tạo ra phần tử chẵn đầu tiên. Câu trả lời hoàn toàn xuất phát từ việc kiểm tra trực tiếp điều kiện gcd giữa các số lẻ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo mọi hoán vị có thể, đếm xem có bao nhiêu cặp thỏa mãn điều kiện và giữ mức tối đa. 

Điều này hiệu quả vì định nghĩa về một cặp tốt rất dễ đánh giá. Thật không may, số lượng hoán vị là$n!$. Ngay cả đối với$n=15$, điều đó đã rất lớn rồi. Từ$n$có thể đạt tới 2000 thì cách làm này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng xuất phát từ việc xem xét điều kiện$$\gcd(a_i,2a_j)>1.$$Giả định$a_i$là chẵn. Sau đó$2a_j$cũng là số chẵn nên cả hai số đều chia hết cho$2$. Vì thế$$\gcd(a_i,2a_j)\ge 2.$$Điều đó có nghĩa là mọi cặp có phần tử đầu tiên chẵn sẽ tự động tốt. 

Điều này cho thấy một chiến lược tham lam. Chúng tôi muốn càng nhiều chỉ mục càng tốt để chứa các số chẵn trước số lẻ. Nếu tất cả các số chẵn được chuyển lên phía trước thì mọi cặp có phần tử đầu tiên là một trong những số chẵn đó sẽ tự động trở thành số tốt. 

Sau khi xếp tất cả các số chẵn lên trước, các cặp số còn lại là các cặp số lẻ. Đối với những cặp đó, chúng tôi chỉ cần kiểm tra trực tiếp điều kiện gcd. 

Khi mảng được sắp xếp lại theo cách này, việc đếm câu trả lời trở nên dễ dàng. Chúng tôi kiểm tra từng cặp$(i,j)$và đếm những người thỏa mãn$$\gcd(a_i,2a_j)>1.$$Từ$n\le 2000$, MỘT$O(n^2)$quét cặp là hoàn toàn chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n!\cdot n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp lại tham lam + Kiểm tra cặp |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mảng thành số chẵn và số lẻ. 
2. Xây dựng một thứ tự mới bằng cách đặt tất cả các số chẵn trước, sau đó là tất cả các số lẻ. 
3. Khởi tạo câu trả lời về 0. 
4. Với mỗi cặp chỉ số$(i,j)$với$i<j$, tính toán$$\gcd(a_i,2a_j).$$1. Nếu gcd lớn hơn$1$, tăng câu trả lời. 
2. Xuất số đếm cuối cùng. 

Lý do bước 2 là tối ưu là vì mọi số chẵn đặt trước sẽ tự động tạo ra các cặp tốt với mọi phần tử sau. Di chuyển một số chẵn lên phía trước chỉ có thể làm tăng số lượng các cặp như vậy chứ không bao giờ làm giảm chúng. 

### Tại sao nó hoạt động 

Xét mọi giá trị chẵn$x$. Đối với mọi giá trị sau này$y$,$$\gcd(x,2y)\ge 2.$$Do đó, mọi cặp có phần tử đầu tiên chẵn đều được đảm bảo là tốt. 

Để tối đa hóa số lượng cặp tốt được đảm bảo, mọi số chẵn phải xuất hiện trước mỗi số lẻ. Bất kỳ đơn đặt hàng nào để lại số lẻ trước số chẵn sẽ lãng phí một cặp hàng hóa tự động tiềm năng. Sau khi di chuyển tất cả các số chẵn về phía trước, số lượng các cặp tốt được đảm bảo sẽ được tối đa hóa. 

Trong số các cặp lẻ-lẻ còn lại, không có sự đơn giản hóa cấu trúc nào nữa. Chúng ta phải đánh giá trực tiếp điều kiện gcd. Do đó, thứ tự được mô tả theo sau là phép đếm trực tiếp sẽ tạo ra câu trả lời tối đa có thể. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    even = []
    odd = []

    for x in a:
        if x % 2 == 0:
            even.append(x)
        else:
            odd.append(x)

    a = even + odd

    ans = 0

    for i in range(n):
        for j in range(i + 1, n):
            if gcd(a[i], 2 * a[j]) > 1:
                ans += 1

    print(ans)
```Phần đầu tiên phân tách các giá trị chẵn và lẻ. Điều này thực hiện thứ tự tham lam. Thứ tự tương đối bên trong nhóm chẵn và bên trong nhóm lẻ không quan trọng. 

Sau khi xây dựng mảng được sắp xếp lại, các vòng lặp lồng nhau sẽ kiểm tra từng cặp chính xác một lần. Điều kiện trong câu lệnh được kiểm tra theo nghĩa đen, sử dụng công cụ tích hợp sẵn của Python`gcd`. 

Không có vấn đề tràn tồn tại bởi vì$$a_i \le 10^5,$$Vì thế$$2a_j \le 2\cdot 10^5.$$Vòng lặp lồng nhau chạy tối đa khoảng hai triệu kiểm tra cặp trên tất cả các trường hợp thử nghiệm, dễ dàng phù hợp với giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 6 5 3
```Sau khi sắp xếp lại:```
6 3 5 3
```| Cặp | gcd(ai, 2aj) | Tốt? | 
| --- | --- | --- | 
| (6,3) | gcd(6,6)=6 | Có | 
| (6,5) | gcd(6,10)=2 | Có | 
| (6,3) | gcd(6,6)=6 | Có | 
| (3,5) | gcd(3,10)=1 | Không | 
| (3,3) | gcd(3,6)=3 | Có | 
| (5,3) | gcd(5,6)=1 | Không | 

Tổng số cặp tốt = 4. 

Ví dụ này cho thấy tại sao việc đặt số chẵn đầu tiên lại có lợi. Mỗi cặp bắt đầu bằng 6 đều tự động tốt. 

### Ví dụ 2 

đầu vào:```
5
1 4 2 4 1
```Sau khi sắp xếp lại:```
4 2 4 1 1
```| Cặp | gcd(ai, 2aj) | Tốt? | 
| --- | --- | --- | 
| (4,2) | 4 | Có | 
| (4,4) | 4 | Có | 
| (4,1) | 2 | Có | 
| (4,1) | 2 | Có | 
| (2,4) | 2 | Có | 
| (2,1) | 2 | Có | 
| (2,1) | 2 | Có | 
| (4,1) | 2 | Có | 
| (4,1) | 2 | Có | 
| (1,1) | 1 | Không | 

Câu trả lời là 9. 

Ví dụ này chứng minh rằng mọi cặp bắt đầu bằng số chẵn đều tự động đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Kiểm tra từng cặp một lần sau khi sắp xếp lại | 
| Không gian |$O(n)$| Lưu trữ mảng được sắp xếp lại | 

Bởi vì tổng của tất cả$n$giá trị nhiều nhất là 2000, tổng số cặp kiểm tra bị giới hạn bởi khoảng bốn triệu. Điều này thoải mái phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        even = [x for x in a if x % 2 == 0]
        odd = [x for x in a if x % 2 == 1]
        a = even + odd

        ans = 0
        for i in range(n):
            for j in range(i + 1, n):
                if gcd(a[i], 2 * a[j]) > 1:
                    ans += 1

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

assert run("3\n4\n3 6 5 3\n2\n1 7\n5\n1 4 2 4 1\n") == "4\n0\n9", "sample"

assert run("1\n2\n2 4\n") == "1", "all even"
assert run("1\n2\n1 1\n") == "0", "all odd"
assert run("1\n3\n2 3 5\n") == "2", "single even in front"
assert run("1\n4\n2 2 2 2\n") == "6", "all pairs good"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 4`|`1`| Trường hợp chẵn nhỏ nhất không tầm thường | 
|`1 1`|`0`| Gcd không bao giờ vượt quá 1 | 
|`2 3 5`|`2`| Hiệu ứng tham lam ngay từ đầu | 
|`2 2 2 2`|`6`| Mỗi cặp đều trở nên tốt | 

## Vỏ cạnh 

Hãy xem xét một mảng bao gồm toàn bộ số chẵn:```
1
4
2 4 6 8
```Mọi cặp đều thỏa mãn điều kiện vì phần tử đầu tiên của mỗi cặp là số chẵn. Thuật toán đặt tất cả các số ở phía trước, sau đó đếm$$\binom{4}{2}=6$$cặp tốt. 

Hãy xem xét một mảng bao gồm toàn số lẻ:```
1
3
1 3 5
```Mảng được sắp xếp lại không thay đổi. Thuật toán kiểm tra trực tiếp từng cặp:$$\gcd(1,6)=1,\quad \gcd(1,10)=1,\quad \gcd(3,10)=1.$$Câu trả lời là 0. 

Xét một hỗn hợp chỉ có một số chẵn:```
1
3
3 2 5
```Thuật toán sắp xếp lại thành```
2 3 5
```Các cặp là$$(2,3),\ (2,5),\ (3,5).$$Hai cái đầu tiên là tốt vì phần tử dẫn đầu là chẵn. Cái cuối cùng thì không. Câu trả lời là 2, là số tối đa.
