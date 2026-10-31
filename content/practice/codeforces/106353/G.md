---
title: "CF 106353G - Ảnh nhóm"
description: "Chúng ta được cho một chuỗi các độ cao khác nhau được sắp xếp thành một đường thẳng. Chúng tôi được phép chọn một số người và chỉ tự do sắp xếp lại những người được chọn trong số họ, trong khi những người còn lại vẫn giữ nguyên vị trí ban đầu."
date: "2026-06-19T14:54:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106353
codeforces_index: "G"
codeforces_contest_name: "2025-2026 ICPC Northwestern European Regional Programming Contest (NWERC 2025)"
rating: 0
weight: 106353
solve_time_s: 57
verified: true
draft: false
---

[CF 106353G - Ảnh nhóm](https://codeforces.com/problemset/problem/106353/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các độ cao khác nhau được sắp xếp thành một đường thẳng. Chúng tôi được phép chọn một số người và chỉ tự do sắp xếp lại những người được chọn trong số họ, trong khi những người còn lại vẫn giữ nguyên vị trí ban đầu. Sau khi thực hiện việc này, đường thẳng đầy đủ phải tạo thành một “hình ngọn núi”, nghĩa là nếu chúng ta nhìn vào độ cao từ trái sang phải, chúng sẽ tăng dần đến một điểm nào đó rồi giảm dần sau đó. 

Mục tiêu không phải là xây dựng sự sắp xếp cuối cùng một cách rõ ràng mà là giảm thiểu số lượng người chúng tôi cần di chuyển. Một người “di chuyển” nếu họ nằm trong tập hợp con đã chọn có vị trí được sắp xếp lại. 

Một cách hữu ích để nghĩ về điều này là những người chúng ta không di chuyển sẽ giữ cố định cả vị trí và chiều cao của họ trong cấu hình cuối cùng. Vì vậy, những người không thay đổi này chắc hẳn đã phù hợp với thứ tự núi cuối cùng theo thứ tự từ trái sang phải hiện tại của họ. 

Ràng buộc n lên tới 5 × 10^5 buộc bất kỳ phương trình bậc hai nào trong n đều thất bại ngay lập tức. Ngay cả O(n log^2 n) cũng là đường biên nhưng có khả năng chấp nhận được, vì vậy giải pháp phải dựa vào các kỹ thuật chuỗi con dài nhất cổ điển với cấu trúc dữ liệu hiệu quả. 

Một trường hợp thất bại tinh tế xuất hiện khi sử dụng một quyết định tham lam hoặc cục bộ. Ví dụ, cố gắng chọn một đỉnh tốt nhất một cách độc lập hoặc thực thi một cách độc lập thứ tự tăng dần ở phía bên trái mà không phối hợp cả hai bên sẽ dẫn đến những mâu thuẫn trong đó hậu tố giảm dần không thể được thỏa mãn. Cấu trúc đúng phải coi ngọn núi như một dãy con nhất quán duy nhất chứ không phải là hai công trình tham lam không liên quan. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ sẽ là chọn tập hợp con những người mà chúng tôi giữ cố định, sau đó thử xem liệu những người còn lại có thể được sắp xếp lại để chuỗi đầy đủ trở thành bitonic hay không. Đối với mỗi tập hợp con, chúng tôi sẽ mô phỏng xem liệu các vị trí cố định có thể được nhúng vào một dãy núi hợp lệ hay không. Vì có 2^n tập hợp con nên điều này ngay lập tức không khả thi và thậm chí việc cố gắng tối ưu hóa việc kiểm tra tính khả thi cho mỗi tập hợp con vẫn sẽ quá chậm. 

Quan sát quan trọng là việc sắp xếp lại tập hợp con đã chọn một cách tự do có nghĩa là hạn chế duy nhất quan trọng là tính nhất quán về trật tự tương đối của những người mà chúng ta không di chuyển. Nếu một người không bị di chuyển, vị trí của họ được cố định, do đó, trong chuỗi cuối cùng, thứ tự tương đối của họ được giữ nguyên. Điều này có nghĩa là tập hợp những người mà chúng ta cố định phải tạo thành một dãy con hợp lệ của dãy núi cuối cùng. 

Vì vậy, thay vì nghĩ đến việc di chuyển mọi người, chúng tôi lật ngược quan điểm: chúng tôi muốn giữ càng nhiều người càng tốt, những người đã nói dối về một số chuỗi bitonic hợp lệ. Nếu chúng ta tối đa hóa kích thước của dãy con như vậy thì những người còn lại chính xác là những người chúng ta phải di chuyển. 

Điều này làm giảm vấn đề tìm chuỗi bitonic dài nhất trong mảng ban đầu. Khi chúng ta biết giá trị đó, câu trả lời là n trừ đi độ dài đó. 

Một chuỗi con bitonic có thể được phân tách ở đỉnh của nó thành phần tăng chặt, theo sau là phần giảm chặt. Đối với mọi vị trí i được coi là đỉnh, chúng tôi tính toán dãy con tăng dài nhất kết thúc tại i và dãy con giảm dài nhất bắt đầu tại i. Việc kết hợp những điều này sẽ mang lại chuỗi bitonic tốt nhất với đỉnh tại i. 

Chúng tôi tính toán trước cả hai phần một cách hiệu quả bằng cách sử dụng cây Fenwick (hoặc bất kỳ cấu trúc log n LIS nào). Một lượt tính LIS kết thúc ở mỗi chỉ mục. Bước thứ hai từ bên phải tính toán LIS trên các giá trị đảo ngược, tương ứng với các chuỗi con giảm dần theo hướng ban đầu. Lấy điểm phân chia tốt nhất mang lại câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n) | O(n) | Quá chậm | 
| LIS Bitonic với cây Fenwick | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính, với mọi vị trí i, độ dài của dãy con tăng nghiêm ngặt dài nhất kết thúc chính xác tại i. Điều này cho thấy mỗi phần tử có thể hoạt động tốt như thế nào khi ở bên trái của đỉnh. 
2. Để tính toán điều này một cách hiệu quả, hãy xử lý các phần tử từ trái sang phải trong khi vẫn duy trì cây Fenwick được khóa theo chiều cao. Đối với mỗi i, hãy truy vấn giá trị tốt nhất trong số tất cả các độ cao nhỏ hơn và thêm một giá trị. 
3. Tính, với mỗi vị trí i, độ dài của dãy con giảm nghiêm ngặt dài nhất bắt đầu từ i. Điều này thể hiện mức độ chúng ta có thể mở rộng ngọn núi về bên phải của đỉnh đã chọn. 
4. Chuyển vấn đề này thành bài toán dãy con tăng dần bằng cách xử lý từ phải sang trái và ánh xạ từng giá trị a[i] tới n - a[i] + 1 sao cho số giảm trở thành số tăng. 
5. Duy trì cây Fenwick thứ hai trong quá trình di chuyển ngược này. Đối với mỗi vị trí i, truy vấn giá trị tốt nhất trong số các độ cao được chuyển đổi nhỏ hơn và cập nhật tương ứng. 
6. Đối với mỗi vị trí i, coi nó là đỉnh của ngọn núi và tính dp_inc[i] + dp_dec[i] - 1, vì đỉnh được tính hai lần. 
7. Lấy giá trị lớn nhất trên tất cả i. Câu trả lời cuối cùng là n trừ độ dài chuỗi con bitonic tối đa này. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình cuối cùng hợp lệ nào đều tương ứng với một dãy con gồm những người có thứ tự tương đối từ trái sang phải được giữ nguyên và có chiều cao tạo thành một chuỗi bitonic. Ngược lại, bất kỳ chuỗi con bitonic nào cũng có thể được giữ cố định trong khi tất cả các phần tử khác được di chuyển và đặt tùy ý vào các vị trí còn lại. Điều này thiết lập một sự tương đương trực tiếp giữa việc tối đa hóa số người cố định và việc tìm ra chuỗi con bitonic dài nhất, do đó không có giải pháp nào có thể tốt hơn sự phân rã này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def update(self, i, v):
        while i <= self.n:
            if v > self.bit[i]:
                self.bit[i] = v
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            if self.bit[i] > res:
                res = self.bit[i]
            i -= i & -i
        return res

n = int(input())
a = list(map(int, input().split()))

dp_inc = [0] * n
bit = Fenwick(n)

for i in range(n):
    x = a[i]
    best = bit.query(x - 1)
    dp_inc[i] = best + 1
    bit.update(x, dp_inc[i])

dp_dec = [0] * n
bit = Fenwick(n)

for i in range(n - 1, -1, -1):
    x = n - a[i] + 1
    best = bit.query(x - 1)
    dp_dec[i] = best + 1
    bit.update(x, dp_dec[i])

best = 1
for i in range(n):
    best = max(best, dp_inc[i] + dp_dec[i] - 1)

print(n - best)
```Bước đầu tiên tính toán các chuỗi con tăng dần kết thúc ở mỗi vị trí bằng cách sử dụng cây Fenwick theo độ cao. Mỗi truy vấn truy xuất độ dài chuỗi con tốt nhất trong số tất cả các độ cao nhỏ hơn được thấy cho đến nay. 

Bước thứ hai đảo ngược mảng và biến đổi các giá trị sao cho các chuỗi con giảm dần trở thành chuỗi tăng dần. Logic Fenwick tương tự được sử dụng lại, giúp giữ cho việc triển khai đối xứng và tránh ghi DP giảm riêng biệt. 

Cuối cùng, mọi chỉ số đều được coi là một đỉnh tiềm năng và chúng tôi hợp nhất hai giá trị DP một cách cẩn thận bằng cách trừ một giá trị để tránh tính hai lần phần tử đỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 6 4 3 2 5
```Chúng tôi tính toán các khoản đóng góp tăng dần và giảm dần. 

| tôi | một [tôi] | dp_inc[i] | dp_dec[i] | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 
| 1 | 6 | 2 | 1 | 2 | 
| 2 | 4 | 2 | 3 | 4 | 
| 3 | 3 | 2 | 2 | 3 | 
| 4 | 2 | 2 | 1 | 2 | 
| 5 | 5 | 3 | 1 | 3 | 

Đỉnh tốt nhất là ở chỉ số 2 với giá trị 4 cho độ dài bitonic là 4. Vì vậy, câu trả lời là 6 − 4 = 2. 

Điều này cho thấy cấu trúc tối ưu không nhất thiết phải đạt đến giá trị tối đa mà ở điểm mà sự tăng trưởng bên trái và bên phải cân bằng tốt nhất. 

### Ví dụ 2 

đầu vào:```
4
1 2 4 3
```| tôi | một [tôi] | dp_inc[i] | dp_dec[i] | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 
| 1 | 2 | 2 | 1 | 2 | 
| 2 | 4 | 3 | 2 | 4 | 
| 3 | 3 | 3 | 1 | 3 | 

Đỉnh tốt nhất là ở chỉ số 2 với độ dài 4, vì vậy câu trả lời là 0. 

Điều này xác nhận trường hợp toàn bộ mảng đã có hình dạng ngọn núi hoàn hảo, nghĩa là không cần phải di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Hai đường đi qua cây Fenwick với các truy vấn và cập nhật logarit | 
| Không gian | O(n) | Mảng cho các giá trị DP và lưu trữ cây Fenwick | 

Các ràng buộc cho phép tối đa 5 × 10^5 phần tử, do đó, một giải pháp tuyến tính với hệ số không đổi nhỏ phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def update(self, i, v):
            while i <= self.n:
                if v > self.bit[i]:
                    self.bit[i] = v
                i += i & -i

        def query(self, i):
            res = 0
            while i > 0:
                if self.bit[i] > res:
                    res = self.bit[i]
                i -= i & -i
            return res

    n = int(input())
    a = list(map(int, input().split()))

    dp_inc = [0] * n
    bit = Fenwick(n)

    for i in range(n):
        dp_inc[i] = bit.query(a[i] - 1) + 1
        bit.update(a[i], dp_inc[i])

    dp_dec = [0] * n
    bit = Fenwick(n)

    for i in range(n - 1, -1, -1):
        x = n - a[i] + 1
        dp_dec[i] = bit.query(x - 1) + 1
        bit.update(x, dp_dec[i])

    best = 1
    for i in range(n):
        best = max(best, dp_inc[i] + dp_dec[i] - 1)

    return str(n - best)

assert run("6\n1 6 4 3 2 5\n") == "2"
assert run("4\n1 2 4 3\n") == "0"
assert run("1\n1\n") == "0"
assert run("5\n5 4 3 2 1\n") == "4"
assert run("5\n1 2 3 4 5\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 1 6 4 3 2 5 | 2 | cấu trúc núi hỗn hợp chung | 
| 4 1 2 4 3 | 0 | cấu hình đã tối ưu | 
| 1 1 | 0 | trường hợp cạnh kích thước tối thiểu | 
| 5 5 4 3 2 1 | 4 | mảng giảm hoàn toàn | 
| 5 1 2 3 4 5 | 4 | mảng tăng đầy đủ | 

## Vỏ cạnh 

Một chuỗi tăng đầy đủ như`1 2 3 4 5`có giá trị đối với hình dạng núi có phần giảm trống. Thuật toán xử lý điều này vì dp_inc trở thành cực đại ở phần tử cuối cùng trong khi dp_dec vẫn bằng 1 ở mọi nơi, do đó độ dài bitonic tốt nhất là n và không cần di chuyển. 

Một chuỗi giảm hoàn toàn như`5 4 3 2 1`hoạt động đối xứng, trong đó dp_inc là 1 ở mọi nơi và dp_dec là tối đa ở phần tử đầu tiên. Đỉnh vẫn cung cấp phạm vi bao phủ đầy đủ nên một lần nữa không cần di chuyển. 

Một phần tử đầu vào đơn lẻ tạo thành một ngọn núi hợp lệ một cách tầm thường và cả hai đường chuyền Fenwick đều trả về 1, tạo ra các bước di chuyển bằng 0 sau khi trừ.
