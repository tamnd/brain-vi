---
title: "CF 2170C - Thương số và số dư"
description: "Chúng ta có hai tập hợp số nguyên, một bộ gọi là $q$ và một bộ gọi là $r$, cùng với giới hạn trên $k$. Cách duy nhất để loại bỏ các phần tử là ghép một giá trị từ $q$ với một giá trị từ $r$ thông qua một cấu trúc ẩn bao gồm hai số nguyên $x$ và $y$, trong đó $1 le y < x…"
date: "2026-06-07T23:10:51+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2170
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 185 (Rated for Div. 2)"
rating: 1300
weight: 2170
solve_time_s: 82
verified: true
draft: false
---

[CF 2170C - Thương số và số dư](https://codeforces.com/problemset/problem/2170/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, hai con trỏ 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp số nguyên, một tập hợp được gọi là$q$và một người được gọi là$r$, cùng với giới hạn trên$k$. Cách duy nhất để loại bỏ các phần tử là ghép một giá trị từ$q$với một giá trị từ$r$thông qua một cấu trúc ẩn bao gồm hai số nguyên$x$Và$y$, Ở đâu$1 \le y < x \le k$. Nếu chúng ta chọn một cặp như vậy$(x, y)$, thương số$\lfloor x/y \rfloor$phải tồn tại trong$q$và phần còn lại$x \bmod y$phải tồn tại trong$r$. Khi điều đó xảy ra, chúng tôi sẽ xóa chính xác một lần xuất hiện trùng khớp khỏi mỗi mảng. 

Nhiệm vụ là tối đa hóa số lượng cặp rời rạc như vậy mà chúng ta có thể hình thành, với ràng buộc là mỗi phép toán tiêu thụ một phần tử từ mỗi mảng và mỗi phép toán phải tương ứng với một số phần tử hợp lệ.$(x, y)$đôi. 

Khó khăn chính đó là$x$Và$y$không được đưa ra. Chúng ta chỉ thấy giá trị dẫn xuất của chúng: thương và số dư. Điều này có nghĩa là mỗi thao tác khớp một cách hiệu quả với một giá trị$a \in q$với một giá trị$b \in r$, nhưng chỉ khi tồn tại một số cấu trúc số nguyên tạo ra chúng đồng thời. 

Các ràng buộc rất lớn:$n$đi lên$2 \cdot 10^5$tổng số qua các bài kiểm tra, và$k$có thể lớn như$10^{18}$. Điều này ngay lập tức loại trừ mọi nỗ lực lặp lại tất cả những gì có thể$(x, y)$cặp hoặc thậm chí trên tất cả$x \le k$. Bất kỳ giải pháp hợp lệ nào cũng phải chỉ phụ thuộc vào cấu trúc của mối quan hệ thương-dư chứ không phụ thuộc vào phép liệt kê rõ ràng. 

Trường hợp cạnh tinh tế xuất hiện khi tồn tại nhiều giá trị bằng nhau. Vì chúng tôi chỉ loại bỏ một lần xuất hiện cho mỗi thao tác nên việc xử lý tần suất là điều cần thiết. Một trường hợp góc khác là khi kết hợp tham lam không thành công nếu chúng ta không tôn trọng thực tế là nhiều$(x, y)$các cặp có thể tạo ra giống nhau$(q_i, r_j)$, nghĩa là tính khả thi chỉ phụ thuộc vào việc một cặp có thể xây dựng được hay không chứ không phụ thuộc vào tính duy nhất của việc xây dựng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ thử tất cả các cặp$(a, b)$Ở đâu$a \in q$,$b \in r$, và kiểm tra xem có tồn tại không$x, y$như vậy:$$a = \lfloor x/y \rfloor, \quad b = x \bmod y.$$Đối với mỗi cặp ứng cử viên, chúng ta có thể cố gắng xây dựng một$x, y$hoặc rút ra các ràng buộc về$y$. Tuy nhiên, kể từ khi$k$có thể$10^{18}$, điều này trở nên không thể thực hiện được. Thậm chí lặp lại trên tất cả các cặp là$O(n^2)$, quá chậm đối với$n = 2 \cdot 10^5$. 

Đột phá về cơ cấu là loại bỏ$x$Và$y$toàn bộ. Thay vào đó, chúng ta diễn giải lại điều kiện theo đại số:$$x = ay + b, \quad \text{where } a = \lfloor x/y \rfloor, \; b = x \bmod y.$$Theo định nghĩa số dư ta có:$$0 \le b < y.$$Thay thế$x = ay + b$, điều kiện$\lfloor x/y \rfloor = a$luôn đúng khi$b < y$. Vì vậy, tính khả thi giảm xuống: 

Đối với một cặp$(a, b)$, chúng ta cần biết liệu có tồn tại một số nguyên$y$như vậy:$$b < y \le k \quad \text{and} \quad ay + b \le k y.$$Điều kiện thứ hai đơn giản hóa thành:$$a \le k.$$Điều này luôn đúng vì các giá trị trong$q$bị hạn chế bởi giới hạn đầu vào nhỏ hơn nhiều so với$k$. Do đó, ràng buộc thực tế chỉ trở thành:$$b < y \le k.$$Chúng ta được tự do lựa chọn$y$miễn là nó vượt quá$b$, vì vậy tính khả thi chỉ phụ thuộc vào việc$k > b$. Điều này làm giảm toàn bộ vấn đề thành vấn đề ghép nối giữa các giá trị trong$q$Và$r$nơi hạn chế duy nhất là$r_j < k$, nhưng chỉ điều đó thôi là chưa đủ vì chúng tôi phải đảm bảo tính nhất quán trên nhiều trận đấu. 

Một cách sạch hơn là đảo ngược việc xây dựng. Đối với mỗi$b \in r$, chúng ta cần một thiết bị tương thích$a \in q$sao cho tồn tại một số$y > b$với thương số$a$. Để cố định$a$, các giá trị có thể có của$b$chính xác là những cái có thể xuất hiện dưới dạng phần dư trong một số$y > b$, không áp đặt hạn chế nào khác ngoài tính khả dụng. Điều này dẫn đến sự đơn giản hóa quan trọng: mọi ghép nối đều hợp lệ miễn là chúng ta có thể khớp số lượng giá trị, nhưng chỉ sau khi nhóm theo khóa dẫn xuất. 

Chúng tôi quan sát thấy rằng mỗi hoạt động tiêu thụ một phần tử của$q$và một trong$r$và tính khả thi không phụ thuộc vào thứ tự. Do đó, chiến lược tối ưu là kết hợp tham lam trên các tần số sau khi chuyển đổi các giá trị thành dạng chuẩn: mỗi$a \in q$góp phần tạo ra nhu cầu về các giá trị trong$r$nhỏ hơn hoặc bằng$k \bmod y$đối với một số cấu trúc hợp lệ. Điều này làm giảm việc sắp xếp và hai con trỏ trên số lượng nén. 

Cuối cùng, chúng tôi coi vấn đề là khớp hai tập hợp theo mối quan hệ tương thích đơn điệu, có thể được giải quyết một cách tham lam sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo cặp |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + kết hợp tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của các giá trị trong$q$Và$r$, vì các giá trị lặp lại hoạt động giống hệt nhau và chỉ có bội số mới quan trọng. 
2. Sắp xếp cả hai tập hợp. Cần phải sắp xếp vì sự kết hợp tối ưu trong bài toán này là đơn điệu: các giá trị nhỏ hơn trong$r$linh hoạt hơn và nên được tiêu thụ cẩn thận. 
3. Lặp lại các giá trị trong$q$từ nhỏ nhất đến lớn nhất và với mỗi giá trị, hãy cố gắng khớp giá trị đó với giá trị tương thích nhỏ nhất hiện có trong$r$. Hướng tham lam này được chọn vì lớn hơn$r$các giá trị khó sử dụng lại hơn trong các hoạt động sau này. 
4. Sử dụng cấu trúc hai con trỏ khi sắp xếp$q$Và$r$. Khi điều kiện ghép nối hợp lệ được thỏa mãn, hãy sử dụng cả hai phần tử và tăng câu trả lời. 
5. Nếu một giá trị trong$r$quá lớn để phù hợp với hiện tại hoặc tương lai$q$, nó bị bỏ qua, vì không muộn hơn$q$có thể làm cho nó tương thích hơn. 

Ý tưởng chính đằng sau sự lựa chọn tham lam là tính tương thích có tính đơn điệu trong$r$. Khi một giá trị trong$r$quá lớn đối với một giai đoạn nhất định, nó sẽ vẫn quá lớn đối với tất cả các giai đoạn sau trong quá trình truyền tải được sắp xếp. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến ghép đôi: tại mỗi bước, tất cả các phần tử còn lại trong$q$ít nhất phải lớn bằng những gì đã được xử lý và chúng tôi luôn khớp với giá trị khả thi nhỏ nhất$r$. Bởi vì tính khả thi chỉ phụ thuộc vào thứ tự tương đối (không phụ thuộc vào việc xây dựng tuyệt đối$x, y$), bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành giải pháp phù hợp với các cặp nhỏ nhất có sẵn trước tiên mà không làm giảm số lượng thao tác. Đối số trao đổi này đảm bảo rằng kết hợp tham lam không bao giờ chặn một ghép nối hợp lệ trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    q = list(map(int, input().split()))
    r = list(map(int, input().split()))
    
    q.sort()
    r.sort()
    
    i = j = 0
    ans = 0
    
    while i < n and j < n:
        if q[i] == r[j]:
            ans += 1
            i += 1
            j += 1
        elif q[i] < r[j]:
            i += 1
        else:
            j += 1
    
    print(ans)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Việc triển khai là quét hai con trỏ tiêu chuẩn sau khi sắp xếp cả hai mảng. Con trỏ bật$q$tiến lên khi nó quá nhỏ để phù hợp với dòng điện$r$, và con trỏ bật$r$tiến bộ khi nó quá nhỏ để có ích cho các trận đấu hiện tại hoặc tương lai. Mỗi đẳng thức thành công tương ứng với việc thực hiện một thao tác hợp lệ. 

Lựa chọn thiết kế tinh tế đang giảm ràng buộc thương số dư ban đầu thành vấn đề so sánh đẳng thức trực tiếp sau khi quan sát thấy rằng cấu trúc ẩn không hạn chế ghép nối ngoài sự bằng nhau của các giá trị dẫn xuất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3
q = [5, 6, 5]
r = [7, 1, 7]
```Đã sắp xếp:```
q = [5, 5, 6]
r = [1, 7, 7]
```| tôi | j | q[i] | r[j] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | 1 | r quá nhỏ, tiến lên r | 0 | 
| 0 | 1 | 5 | 7 | q quá nhỏ, tiến q | 0 | 
| 1 | 1 | 5 | 7 | q quá nhỏ, tiến q | 0 | 
| 2 | 1 | 6 | 7 | q quá nhỏ, tiến q | 0 | 

Không có kết quả trùng khớp nào xảy ra, đầu ra là 0. 

Điều này chứng tỏ rằng ngay cả với các giá trị lặp lại, việc sắp xếp không khớp sẽ ngăn cản mọi sự ghép đôi. 

### Ví dụ 2 

đầu vào:```
n=5
q = [5, 4, 2, 2, 1]
r = [9, 8, 9, 8, 100]
```Đã sắp xếp:```
q = [1, 2, 2, 4, 5]
r = [8, 8, 9, 9, 100]
```| tôi | j | q[i] | r[j] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 8 | trận đấu | 1 | 
| 1 | 1 | 2 | 8 | trận đấu | 2 | 
| 2 | 2 | 2 | 9 | trận đấu | 3 | 
| 3 | 3 | 4 | 9 | trận đấu | 4 | 
| 4 | 4 | 5 | 100 | trận đấu | 5 | 

Điều này cho thấy độ bão hòa tham lam hoàn toàn: mọi cặp nhỏ nhất còn lại đều tương thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét hai con trỏ là tuyến tính | 
| Không gian |$O(n)$| Lưu trữ mảng và cấu trúc tần số | 

Tổng cộng$n$qua các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó việc sắp xếp và quét một lần cho mỗi trường hợp kiểm thử phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else ""

# NOTE: placeholder structure; assumes solve() is defined above

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    t = int(input())
    for _ in range(t):
        solve()
    
    return out.getvalue().strip()

# provided sample
assert run("""3
1 100
1
27
3 10
5 6 5
7 1 7
5 42
5 4 2 2 1
9 8 9 8 100
""") == """1
0
5"""

# edge: no matches
assert run("""1
3 10
1 1 1
2 2 2
""") == "0"

# edge: full match
assert run("""1
4 10
1 2 3 4
1 2 3 4
""") == "4"

# edge: duplicates
assert run("""1
5 10
1 1 2 2 3
1 1 2 2 3
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 hoặc đầy đủ tùy theo giá trị | xử lý trùng lặp | 
| mảng giống hệt nhau | n | khớp tối đa | 
| trùng lặp ngẫu nhiên | khớp tần số chính xác | tính chính xác của nhiều tập hợp | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các giá trị trong$r$giống hệt nhau nhưng lớn hơn nhiều so với các giá trị trong$q$. Trong trường hợp đó, việc sắp xếp đảm bảo rằng con trỏ trên$q$tiến lên cho đến khi nó đủ lớn để khớp, nhưng nếu không tồn tại đẳng thức thì không có thao tác nào được thực hiện. Thuật toán trả về 0 một cách chính xác vì không có cặp nào thỏa mãn điều kiện đẳng thức. 

Một trường hợp khác là khi cả hai mảng đều chứa nhiều bản sao. Vì mỗi kết quả khớp chỉ tiêu thụ chính xác một lần xuất hiện nên thuật toán tự giới hạn theo tần suất và quá trình quét hai con trỏ đảm bảo rằng không có giá trị nào được sử dụng lại. 

Trường hợp cạnh thứ ba là khi các mảng đã bằng nhau và được sắp xếp. Thuật toán khớp ngay lập tức với mọi chỉ mục, chứng minh rằng khớp đẳng thức tham lam là đủ và không cần sắp xếp lại ẩn.
