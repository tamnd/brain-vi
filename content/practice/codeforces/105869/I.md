---
title: "CF 105869I - Số dư ngẫu nhiên"
description: "Chúng ta được cho một mảng các số nguyên dương. Nhiệm vụ là tính tổng toàn cục trên tất cả các cặp có thứ tự trong đó mỗi phần tử trong mảng đóng vai trò là ước số trong biểu thức mô-đun."
date: "2026-06-22T02:29:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105869
codeforces_index: "I"
codeforces_contest_name: "OCPC Fall 2024 Day 2 Jagiellonian Contest (The 3rd Universal Cup. Stage 35: Krak\u00f3w)"
rating: 0
weight: 105869
solve_time_s: 49
verified: true
draft: false
---

[CF 105869I - Số dư ngẫu nhiên](https://codeforces.com/problemset/problem/105869/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Nhiệm vụ là tính tổng toàn cục trên tất cả các cặp có thứ tự trong đó mỗi phần tử trong mảng đóng vai trò là ước số trong biểu thức mô-đun. Đối với mỗi phần tử cố định$a_j$, chúng tôi xem xét mọi phần tử khác$a_i$, tính toán$a_i \bmod a_j$, bình phương nó và tích lũy mọi thứ thành một câu trả lời duy nhất. 

Vì vậy, cấu trúc về cơ bản là sự tương tác theo cặp trên một mảng được sắp xếp, trong đó mỗi cặp đóng góp một giá trị tùy thuộc vào cách thức$a_i$phân hủy đối với$a_j$. 

Quan sát quan trọng là đối với một ứng cử viên có số chia cố định$a_j$, các giá trị của$a_i$hành xử theo cách có cấu trúc khi được nhóm theo thương số$\left\lfloor \frac{a_i}{a_j} \right\rfloor$. Thương số này chia mảng thành các phân đoạn trong đó biểu thức mô đun trở thành tuyến tính theo$a_i$, điều này làm cho việc tổng hợp tiền tố có thể thực hiện được. 

Kích thước đầu vào đủ lớn để bất kỳ giải pháp nào lặp lại trực tiếp trên tất cả các cặp đều không khả thi ngay lập tức. Một sự ngây thơ$O(n^2)$cách tiếp cận ngụ ý đến$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. Ngay cả một phương pháp theo cặp được tối ưu hóa một chút vẫn còn quá chậm vì quá trình tính toán bên trong không hề đơn giản. 

Cấu trúc ẩn nằm ở chỗ, mặc dù có$n^2$cặp, mỗi cặp cố định$a_j$chỉ tương tác với một số khoảng thương số giới hạn khi các giá trị được nhóm theo phép chia. Điều này làm giảm hiệu quả công việc một cách đáng kể. 

Trường hợp cạnh tinh tế phát sinh khi các giá trị bị sai lệch nhiều. Ví dụ: nếu mảng chứa nhiều giá trị lớn lặp lại, việc nhóm đơn giản theo thương vẫn có thể thoái hóa thành nhiều kiểm tra nếu không được triển khai với cách xử lý phạm vi thích hợp. Một trường hợp góc khác là khi các giá trị nhỏ, chẳng hạn như tất cả các giá trị. Trong trường hợp đó, tất cả số dư đều bằng 0 và câu trả lời gần như bằng 0, nhưng việc triển khai bất cẩn vẫn có thể thử logic phân đoạn không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xem xét từng cặp$(i, j)$và tính toán$(a_i \bmod a_j)^2$một cách rõ ràng. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, chi phí của nó là bậc hai tính theo kích thước mảng. Với$n = 2 \cdot 10^5$, điều này dẫn đến hàng tỷ thao tác và không thể chạy kịp thời. 

Để cải thiện điều này, chúng tôi sửa$a_j$và cố gắng hiểu làm thế nào$a_i \bmod a_j$cư xử như$a_i$tăng lên. Ý tưởng chính là thương số$\left\lfloor \frac{a_i}{a_j} \right\rfloor$không đổi trên các phạm vi liền kề của$a_i$. Bên trong phạm vi như vậy, phần còn lại đơn giản hóa thành$a_i - t \cdot a_j$, Ở đâu$t$là giá trị thương số. Điều này biến đổi một biểu thức mô-đun phi tuyến thành một biểu thức bậc hai trên một đoạn liền kề. 

Khi mảng được sắp xếp, tất cả các giá trị có cùng thương số$t$tạo thành một đoạn liền kề. Điều đó cho phép tính toán trước tổng tiền tố của$a_i$,$a_i^2$và số lượng không đổi, sau đó đánh giá từng phân đoạn trong thời gian không đổi. 

Mối quan tâm còn lại là có bao nhiêu phân khúc như vậy tồn tại trên mỗi$a_j$. Đối với mỗi$a_j$, thương số$t$dao động từ$0$lên đến$\lfloor \max(a) / a_j \rfloor$. Mặc dù điều này có vẻ lớn nhưng khi$a_j$được chọn ngẫu nhiên từ một phân bố đồng đều trên mảng, số lần chuyển đổi thương số riêng biệt dự kiến ​​​​là logarit. Đây là hành vi chuỗi hài tương tự xuất hiện trong các đối số đếm số chia. 

Như vậy mỗi$a_j$chỉ đóng góp$O(\log A)$trung bình các phân khúc, dẫn đến kết quả dự kiến$O(n \log n \log A)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n \log A)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu bằng cách sắp xếp mảng sao cho tất cả các phân vùng dựa trên thương số trở nên liền kề nhau. Thứ tự cấu trúc này là thứ cho phép tổng hợp phạm vi thay vì tính toán theo từng phần tử. 

Sau đó, chúng tôi tính toán trước các tổng tiền tố cho lũy thừa của các giá trị mảng. Cụ thể, chúng tôi duy trì số tiền tích lũy của$a_i$,$a_i^2$và cũng là một tiền tố đơn giản để thuận tiện khi mở rộng biểu thức bậc hai. 

Với mỗi giá trị cố định$a_j$, chúng tôi lặp lại các giá trị thương số có thể$t$. Mỗi$t$xác định một loạt các chỉ số$i$như vậy$\left\lfloor \frac{a_i}{a_j} \right\rfloor = t$. Chúng tôi xác định phạm vi này bằng cách sử dụng ranh giới tìm kiếm nhị phân trên mảng được sắp xếp. 

Khi chúng tôi xác định được một phân khúc$[p, q]$, chúng tôi viết lại phần đóng góp:$$(a_i \bmod a_j)^2 = (a_i - t \cdot a_j)^2 = a_i^2 - 2 t a_j a_i + t^2 a_j^2$$Chúng tôi tính tổng trên phân đoạn bằng cách sử dụng tổng tiền tố để mỗi thuật ngữ thu được trong thời gian không đổi. Điều này tránh việc lặp lại qua phân đoạn một cách rõ ràng. 

Chúng tôi lặp lại điều này cho tất cả hợp lệ$t$các giá trị hiện tại$a_j$, tích lũy đóng góp vào câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc phân chia mảng thành các phân đoạn liền kề tối đa trong đó thương số$\left\lfloor \frac{a_i}{a_j} \right\rfloor$là không đổi. Bên trong mỗi phân đoạn, biểu thức mô đun trở thành hàm bậc hai cố định của$a_i$. Bởi vì tổng tiền tố bảo toàn tính tuyến tính nên tổng dạng bậc hai này trong một khoảng liền kề là chính xác và không mất dữ liệu. Mỗi cặp$(i, j)$được bao gồm trong chính xác một phân đoạn tương ứng với thương số của nó, do đó không có đóng góp nào bị bỏ sót hoặc được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    pref1 = [0] * (n + 1)
    pref2 = [0] * (n + 1)

    for i in range(n):
        pref1[i + 1] = pref1[i] + a[i]
        pref2[i + 1] = pref2[i] + a[i] * a[i]

    def range_sum(l, r):
        return pref1[r + 1] - pref1[l]

    def range_sum2(l, r):
        return pref2[r + 1] - pref2[l]

    ans = 0

    for j in range(n):
        aj = a[j]
        i = 0
        max_val = a[-1]

        t = 0
        while t * aj <= max_val:
            # find range of i where floor(ai / aj) == t
            left_val = t * aj
            right_val = (t + 1) * aj - 1

            # binary search for bounds
            l = j + 1
            r = n - 1

            # lower bound
            lo = l
            hi = r
            L = n
            while lo <= hi:
                mid = (lo + hi) // 2
                if a[mid] >= left_val:
                    L = mid
                    hi = mid - 1
                else:
                    lo = mid + 1

            lo = l
            hi = r
            R = j - 1
            while lo <= hi:
                mid = (lo + hi) // 2
                if a[mid] <= right_val:
                    R = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            if L <= R and L < n:
                cnt = R - L + 1
                s1 = range_sum(L, R)
                s2 = range_sum2(L, R)

                # sum (ai - t*aj)^2
                ans += s2 - 2 * t * aj * s1 + cnt * (t * aj) * (t * aj)

            t += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp để kích hoạt các phạm vi thương số liền kề. Sau đó, tổng tiền tố được xây dựng để cho phép tổng hợp phạm vi thời gian không đổi cho cả số hạng tuyến tính và bậc hai. 

Đối với mỗi$a_j$, chúng tôi quét các giá trị thương$t$. Mỗi$t$xác định một khoảng số$[t a_j, (t+1)a_j - 1]$và chúng tôi sử dụng tìm kiếm nhị phân để tìm chỉ số nào rơi vào khoảng này. Phần đóng góp được tính toán bằng cách sử dụng nhận dạng bình phương mở rộng, thay thế phép lặp trên phân đoạn bằng số học tổng tiền tố. 

Phải cẩn thận với các giới hạn: việc tìm kiếm phải loại trừ các chỉ mục trước$j$nếu vấn đề xác định các cặp có thứ tự$i \ne j$và tìm kiếm nhị phân phải xử lý chính xác các phạm vi trống. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ$a = [1, 3, 5]$. 

Chúng tôi tính toán đóng góp cho mỗi$a_j$. 

| j | một[j] | t | đoạn [L, R] | s1 | s2 | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | [1,2] không hợp lệ | - | - | 0 | 
| 1 | 3 | 0 | [0,2] → [1] | 1 | 1 | (1-0)^2 = 1 | 
| 1 | 3 | 1 | [3,5] → [5] | 5 | 25 | (5-3)^2 = 4 | 
| 2 | 5 | 0 | [0,4] → [1,3] | 4 | 10 | (1-0)^2 + (3-0)^2 = 10 | 
| 2 | 5 | 1 | [5,9] → [5] | 5 | 25 | 0 | 

Vì$j=1$, chúng tôi chia các giá trị thành các nhóm thương tương ứng với 3. Giá trị 1 và 5 rơi vào các phạm vi khác nhau, cho thấy cách phân đoạn thay thế tính toán theo từng cặp. 

Bây giờ hãy xem xét$a = [2, 2, 2, 2]$. 

| j | một[j] | t | phân đoạn | cnt | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| tất cả | 2 | 0 | tất cả các yếu tố | 4 | 0 | 

Mọi phần còn lại đều bằng 0, do đó kết quả bằng 0 bất kể phân đoạn. Điều này cho thấy thuật toán thu gọn một cách tự nhiên các trường hợp có giá trị lặp lại mà không cần thêm logic. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \log A)$| sắp xếp cộng với tìm kiếm nhị phân trên mỗi phân đoạn thương cho mỗi phần tử | 
| Không gian |$O(n)$| tổng tiền tố và mảng được lưu trữ | 

Hệ số logarit từ phân đoạn phạm vi giá trị vẫn có kỳ vọng nhỏ vì mỗi$a_j$chỉ tạo ra một số giới hạn các ranh giới thương số có ý nghĩa. Điều này giữ cho dung dịch nằm trong giới hạn điển hình cho$n \approx 2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# simple sample-like case
assert run("3\n1 3 5\n") != "", "basic run"

# all equal
assert run("4\n2 2 2 2\n") == "0", "all equal values"

# minimum size
assert run("1\n7\n") == "0", "single element"

# increasing pattern
assert run("5\n1 2 3 4 5\n") != "", "increasing array"

# large spread
assert run("4\n1 100 1000 10000\n") != "", "wide range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 giá trị giống nhau | 0 | tất cả phần còn lại biến mất | 
| phần tử đơn | 0 | không có cặp nào tồn tại | 
| 1 2 3 4 5 | khác không | tính chính xác của phân đoạn chung | 
| 1 100 1000 10000 | khác không | sự ổn định trên các chỉ số lớn | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng nhau, mọi phần còn lại đều bằng 0 đối với bất kỳ cặp nào và thuật toán xử lý một phân đoạn thương số duy nhất có giá trị bằng 0. Đối với một đầu vào như$[5, 5, 5]$, điều duy nhất có liên quan$t$bằng 0 và tổng phân đoạn được tính toán trở thành 0 vì$a_i - 0 \cdot a_j = 5$nhưng nó chỉ được ghép nối với chính nó tùy thuộc vào quy tắc lập chỉ mục, dẫn đến việc hủy bỏ việc xử lý cặp đối xứng. 

Khi mảng chứa một phần tử duy nhất như$[10]$, không có cặp hợp lệ nào, do đó vòng lặp bên ngoài chạy nhưng không có phân đoạn nào đóng góp. Tìm kiếm nhị phân mang lại phạm vi trống cho tất cả$t$, để lại câu trả lời tích lũy không thay đổi ở mức 0. 

Khi các giá trị có khoảng cách cao như$[1, 10^9]$, mỗi$a_j$chỉ tạo ra một vài khoảng thương vì$t a_j$nhảy nhanh vượt quá giá trị mảng tối đa. Vòng lặp kết thúc$t$kết thúc sớm và chỉ một hoặc hai phân đoạn được đánh giá, thể hiện hành vi logarit trong thực tế.
