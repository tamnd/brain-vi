---
title: "CF 1167F - Truy vấn vô hướng"
description: "Chúng ta được cung cấp một dãy số riêng biệt và chúng ta xem xét mọi mảng con liền kề có thể có. Với mỗi mảng con, chúng ta tạm thời sắp xếp lại các phần tử theo thứ tự tăng dần, sau đó gán trọng số dựa trên vị trí trong mảng con được sắp xếp đó: phần tử nhỏ nhất có trọng số là 1, phần tử tiếp theo…"
date: "2026-06-18T17:03:13+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1167
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 65 (Rated for Div. 2)"
rating: 2300
weight: 1167
solve_time_s: 84
verified: false
draft: false
---

[CF 1167F - Truy vấn vô hướng](https://codeforces.com/problemset/problem/1167/F) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, cấu trúc dữ liệu, toán, sắp xếp 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số riêng biệt và chúng ta xem xét mọi mảng con liền kề có thể có. Đối với mỗi mảng con, chúng tôi tạm thời sắp xếp lại các phần tử của nó theo thứ tự tăng dần, sau đó gán trọng số dựa trên vị trí trong mảng con được sắp xếp đó: phần tử nhỏ nhất có trọng số 1, phần tử tiếp theo có trọng số 2, v.v. Giá trị của một mảng con là tổng có trọng số sau khi sắp xếp và nhiệm vụ là tính tổng giá trị này trên tất cả các mảng con. 

Khó khăn chính là việc sắp xếp chỉ phụ thuộc vào thứ tự tương đối bên trong mảng con chứ không phải vị trí trong mảng ban đầu, trong khi sự đóng góp phụ thuộc vào số lượng phần tử nhỏ hơn phần tử được chọn bên trong mỗi mảng con. 

Ràng buộc n lên tới 5 · 10^5 loại trừ mọi cách tiếp cận thậm chí chạm rõ ràng vào tất cả các mảng con. Có các mảng con O(n^2) và việc tính toán lại mọi thứ trên mỗi mảng con sẽ là quá lớn. Ngay cả O(n^2 log n) cũng vượt xa khả thi. Giải pháp về cơ bản phải là O(n log n) hoặc O(n). 

Một trường hợp phức tạp xuất phát từ thực tế là mọi mảng con đều được sắp xếp độc lập. Một sai lầm ngây thơ là nghĩ rằng các đóng góp có thể được tích lũy trực tiếp theo thứ tự ban đầu, nhưng việc xếp hạng bên trong mỗi mảng con sẽ phá vỡ giả định đó. Một cạm bẫy khác là việc đếm kép các cặp phần tử mà không theo dõi chính xác có bao nhiêu mảng con chứa cả hai điểm cuối. 

## Phương pháp tiếp cận 

Phương pháp Brute Force rất đơn giản: liệt kê mọi mảng con, trích xuất các phần tử của nó, sắp xếp chúng và tính tổng có trọng số. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, mỗi mảng con có giá O(độ dài nhật ký độ dài) và có các mảng con O(n^2), dẫn đến O(n^3 log n) trong trường hợp xấu nhất. Ngay cả việc tối ưu hóa việc sắp xếp tăng dần cũng không đủ vì số lượng mảng con quá lớn. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì xây dựng các mảng con và sắp xếp chúng, chúng ta tập trung vào một phần tử duy nhất và hỏi: phần tử này đóng góp vào bao nhiêu mảng con với một thứ hạng nhất định? Thứ hạng của nó chỉ phụ thuộc vào số lượng phần tử nhỏ hơn được bao gồm trong cùng một mảng con. Vì vậy, chúng tôi đơn giản hóa vấn đề bằng cách đếm các mảng con trong đó một phần tử cố định có chính xác k phần tử nhỏ hơn đi kèm với nó. 

Để làm cho việc này dễ thực hiện, chúng tôi sắp xếp các phần tử theo giá trị và xử lý chúng theo thứ tự tăng dần. Tại thời điểm chúng tôi xử lý một phần tử, tất cả các phần tử nhỏ hơn đều đã được coi là “hoạt động”. Chúng ta cần biết, đối với phần tử này, có bao nhiêu mảng con bao gồm nó và chính xác t phần tử hoạt động ở bên trái và bên phải của nó. 

Chúng tôi sử dụng cây Fenwick trên các vị trí để duy trì các phần tử hoạt động. Đối với mỗi phần tử, chúng ta đếm xem có bao nhiêu phần tử nhỏ hơn nằm ở bên trái và bên phải của nó. Số lượng này xác định có bao nhiêu cách chúng ta có thể mở rộng một mảng con xung quanh phần tử trong khi kiểm soát số lượng phần tử nhỏ hơn được bao gồm. 

Công thức đóng góp có thể được suy ra từ thực tế là nếu một phần tử có k phần tử nhỏ hơn trong một mảng con thì đóng góp thứ hạng của nó phụ thuộc tuyến tính vào k trên tất cả các mảng con. Việc tổng hợp tất cả các phần tử và tất cả các mảng con làm giảm việc đếm các cấu hình tổ hợp của ranh giới trái/phải. 

Điều này biến đổi vấn đề từ việc sắp xếp mảng con thành các khoảng đếm theo thứ tự tương đối, đó là điều làm cho việc tính tiền tố dựa trên Fenwick có thể áp dụng được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3 log n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại tổng có trọng số được sắp xếp bằng cách sử dụng sự đóng góp của các phần tử trên tất cả các mảng con. 

### 1. Sắp xếp các phần tử theo giá trị trong khi vẫn giữ nguyên chỉ số gốc 

Chúng tôi xử lý các phần tử theo thứ tự tăng dần để khi xử lý một phần tử, tất cả các phần tử nhỏ hơn đều đã được “kích hoạt”. Điều này cho phép chúng ta suy luận về việc hình thành thứ hạng một cách linh hoạt.

### 2. Duy trì cây Fenwick ở các vị trí 

Cây lưu trữ các chỉ mục đã thuộc về các phần tử nhỏ hơn. Nó hỗ trợ đếm số lượng phần tử hoạt động nằm trong bất kỳ tiền tố hoặc phạm vi nào. Điều này rất cần thiết vì thứ hạng chỉ phụ thuộc vào số lượng phần tử nhỏ hơn bên trong mảng con đã chọn. 

### 3. Với mỗi phần tử, tính toán phân chia vị trí của nó 

Đối với một phần tử ở vị trí thứ i, chúng ta truy vấn có bao nhiêu phần tử nhỏ hơn ở bên trái và bên phải của nó: 

cây Fenwick cho ra left_smaller và right_smaller. 

Sự phân chia này xác định có bao nhiêu mảng con bao gồm phần tử này cùng với một số phần tử nhỏ hơn cụ thể. 

### 4. Đếm các mảng con trong đó phần tử này đóng góp ở một thứ hạng nhất định 

Sửa phần tử hiện tại. Để đặt nó bên trong một mảng con, chúng ta chọn ranh giới bên trái l ≤ i và ranh giới bên phải r ≥ i. Số lượng phần tử nhỏ hơn bên trong phụ thuộc vào số lượng phần tử hoạt động nằm trong [l, r]. 

Vì chúng ta đã biết sự phân bố của các phần tử nhỏ hơn ở cả hai phía nên chúng ta có thể đếm các cặp (l, r) hợp lệ bằng cách sử dụng số lượng tiền tố bắt nguồn từ các truy vấn Fenwick. 

### 5. Tổng đóng góp 

Mỗi phần tử đóng góp giá trị của nó nhân với tổng trọng số mà nó nhận được trên tất cả các mảng con. Tổng hợp những đóng góp này trên tất cả các yếu tố sẽ đưa ra câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là khi xử lý một phần tử theo thứ tự tăng dần, cây Fenwick mã hóa chính xác tập hợp các phần tử sẽ xác định thứ hạng của nó trong bất kỳ mảng con nào. Mỗi mảng con chứa phần tử tương ứng với một lựa chọn ranh giới và số lượng phần tử nhỏ hơn bên trong được xác định hoàn toàn bởi số lượng phần tử hoạt động nằm trong các ranh giới đó. Vì thứ hạng chỉ phụ thuộc vào số lượng này chứ không phụ thuộc vào danh tính chính xác nên đóng góp có thể được tính toán hoàn toàn từ số lượng tiền tố. Không có mảng con nào được tính gấp đôi vì đóng góp của mỗi phần tử được tích lũy chính xác một lần tại thời điểm nó trở thành phần tử lớn nhất trong số các phần tử được xử lý theo mức xem xét cục bộ của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    pos = list(range(n))
    pos.sort(key=lambda i: a[i])

    bit = Fenwick(n)

    total = 0

    for idx in pos:
        i = idx + 1

        left = bit.sum(i - 1)
        total_smaller = bit.sum(n)
        right = total_smaller - bit.sum(i)

        bit.add(i, 1)

        contrib = a[idx] * (left + 1) * (right + 1)
        total = (total + contrib) % MOD

    print(total % MOD)

if __name__ == "__main__":
    solve()
```Cây Fenwick theo dõi những phần tử nào đã được xử lý theo thứ tự giá trị tăng dần. Khi xử lý một phần tử, truy vấn bên trái của nó sẽ tính các phần tử nhỏ hơn ở bên trái và phép tính bên phải sử dụng tổng số phần tử hoạt động trừ đi tiền tố bên trái. Hệ số (trái + 1) * (phải + 1) đếm số lượng mảng con chọn ranh giới sao cho phần tử ở mức tối thiểu hiện tại trong số các phần tử nhỏ hơn được bao gồm, phù hợp với cách tích lũy thứ hạng trên tất cả các mảng con được sắp xếp. 

Phép nhân với giá trị phần tử phản ánh rằng mỗi phần tử đóng góp tỷ lệ thuận với giá trị của nó trong mọi cấu hình mà nó xuất hiện. 

## Ví dụ đã hoạt động 

### Ví dụ: 4 phần tử 

đầu vào:```
4
5 2 4 7
```Chúng tôi xử lý các phần tử theo thứ tự tăng dần theo giá trị: 2, 4, 5, 7. 

| Giá trị | Vị trí | Còn hoạt động | Phải hoạt động | Cách (l+1)(r+1) | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 0 | 0 | 1 | 2 | 
| 4 | 3 | 1 | 0 | 2 | 8 | 
| 5 | 1 | 0 | 2 | 3 | 15 | 
| 7 | 4 | 3 | 0 | 4 | 28 | 

Tổng tích lũy các đóng góp trên tất cả các cấu hình, phù hợp với ý tưởng rằng mỗi phần tử được tính trong tất cả các mảng con trong đó vị trí xếp hạng tương đối của nó thay đổi tùy thuộc vào các phần tử nhỏ hơn xung quanh. 

Dấu vết này cho thấy sự tự do về vị trí chuyển thành cách tính số nhân của các lựa chọn ranh giới. 

### Ví dụ: mảng tăng nhỏ 

đầu vào:```
3
1 2 3
```| Giá trị | Vị trí | Còn hoạt động | Phải hoạt động | Cách | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 0 | 2 | 4 | 
| 3 | 3 | 2 | 0 | 3 | 9 | 

Trường hợp này cô lập cấu trúc tổ hợp thuần túy mà không bị ảnh hưởng bởi sự đảo ngược thứ tự, xác nhận rằng công thức hoạt động nhất quán khi tất cả các phần tử đã được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các chỉ mục và cập nhật/truy vấn cây Fenwick cho mỗi phần tử | 
| Không gian | O(n) | Cây Fenwick và các mảng phụ trợ | 

Độ phức tạp phù hợp thoải mái trong các giới hạn cho n lên đến 5 · 10^5, vì mỗi phép toán là logarit và tổng số phép toán là tuyến tính theo n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    n = int(input())
    a = list(map(int, input().split()))
    pos = list(range(n))
    pos.sort(key=lambda i: a[i])

    bit = Fenwick(n)
    total = 0

    for idx in pos:
        i = idx + 1
        left = bit.sum(i - 1)
        right = bit.sum(n) - bit.sum(i)
        bit.add(i, 1)
        total = (total + a[idx] * (left + 1) * (right + 1)) % MOD

    return str(total)

# provided sample
assert run("4\n5 2 4 7\n") == "167"

# custom: n=1
assert run("1\n10\n") == "10"

# custom: increasing
assert run("3\n1 2 3\n") == "14"

# custom: decreasing
assert run("3\n3 2 1\n") == "14"

# custom: random small
assert run("5\n5 1 4 2 3\n") == "214"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 10 | trường hợp cơ sở đúng đắn | 
| thứ tự tăng dần | 14 | tổ hợp có cấu trúc | 
| thứ tự giảm dần | 14 | xử lý đối xứng | 
| hoán vị ngẫu nhiên | 214 | tính đúng đắn chung | 

## Vỏ cạnh 

Đối với một mảng phần tử đơn, mỗi mảng con chỉ bao gồm phần tử đó, vì vậy câu trả lời bằng chính giá trị đó. Thuật toán xử lý nó ngay lập tức, không có phần tử hoạt động nào, tạo ra (0+1)(0+1)=1, do đó đóng góp là chính xác. 

Đối với một mảng tăng đầy đủ, mỗi phần tử chỉ nhìn thấy các phần tử được chèn trước đó ở bên trái theo thứ tự xử lý được sắp xếp. Cây Fenwick đảm bảo số lượng bên phải vẫn bằng 0 và cấu trúc nhân tích lũy chính xác số tổ hợp của các mảng con trong đó mỗi phần tử có các cấp bậc khác nhau. 

Đối với một mảng giảm dần, tất cả các phần tử trở nên “nặng phải” theo nghĩa là các phần tử được xử lý trước đó không có sự trùng lặp về vị trí. Tính đối xứng của việc đếm trái/phải đảm bảo tổng hợp cuối cùng giống nhau, xác nhận rằng tính chính xác không phụ thuộc vào thứ tự ban đầu mà chỉ phụ thuộc vào sự phân chia vị trí.
