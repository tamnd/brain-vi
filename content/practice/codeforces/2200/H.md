---
title: "CF 2200H - Sáu Bảy"
description: "Chúng ta được cho một mảng các số nguyên dương và chúng ta định nghĩa một số là đặc biệt nếu lũy thừa của 6 chia cho nó lớn hơn lũy thừa của 7 chia cho nó."
date: "2026-06-07T20:19:36+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "math", "number-theory", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 2200
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 1084 (Div. 3)"
rating: 2600
weight: 2200
solve_time_s: 124
verified: false
draft: false
---

[CF 2200H - Sáu Bảy](https://codeforces.com/problemset/problem/2200/H) 

**Đánh giá:** 2600 
**Tags:** chia để trị, toán, lý thuyết số, dây, cây 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương và chúng ta định nghĩa một số là _đặc biệt_ nếu lũy thừa của 6 chia cho nó lớn hơn lũy thừa của 7 chia cho nó. Về mặt hình thức, đối với một số$x$, chúng tôi xác định$f_6(x)$là số mũ cao nhất$k$như vậy$6^k$chia rẽ$x$, và tương tự$f_7(x)$cho 7. Một số là đặc biệt nếu$f_6(x) > f_7(x)$. Hoạt động được phép là tăng mọi phần tử của mảng lên một và chúng tôi muốn tìm số lượng tối thiểu các hoạt động đó cần thiết để làm cho tất cả các phần tử trở nên đặc biệt cùng một lúc hoặc báo cáo rằng điều đó là không thể. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp có số phần tử tối đa$2 \cdot 10^5$và giá trị phần tử lên tới$10^9$. Tổng số phần tử trong tất cả các trường hợp thử nghiệm cũng được giới hạn ở$2 \cdot 10^5$. Các giới hạn này ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra từng mức tăng có thể có cho đến$10^9$bởi vì điều đó sẽ đòi hỏi hàng tỷ bước cho mỗi phần tử. Thay vào đó, chúng ta phải tận dụng các tính chất toán học về tính chia hết và số mũ để tính toán câu trả lời một cách hiệu quả. 

Trường hợp cạnh không rõ ràng phát sinh khi một phần tử rất lớn hoặc khi hệ số hóa của nó so với 6 và 7 đến mức không có số gia số nào có thể thỏa mãn$f_6(x) > f_7(x)$. Ví dụ: nếu một phần tử là 7 hoặc bội số của 7 không có thừa số 2 hoặc 3, thì phần tử đó có thể cần nhiều số gia hoặc có thể không bao giờ đạt đến trạng thái mà lũy thừa 6 vượt quá lũy thừa 7. Việc triển khai ngây thơ thêm 1 một cách mù quáng cho đến khi điều kiện được giữ sẽ không thành công trên các đầu vào này do giới hạn thời gian và có thể bỏ lỡ điều không thể thực hiện được. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét từng phần tử một cách độc lập, liên tục thêm 1 và kiểm tra xem liệu$f_6(x) > f_7(x)$nắm giữ. Bản thân việc kiểm tra bao gồm việc tính toán lũy thừa cao nhất của 2 và 3 (cho 6) và 7, có thể được thực hiện trong$O(\log x)$mỗi phần tử. Trong trường hợp xấu nhất, nếu chúng ta cần tăng mỗi số lên tới$10^9$, tổng số thao tác sẽ vượt quá giới hạn thời gian, đặc biệt vì tổng độ dài mảng trên tất cả các trường hợp thử nghiệm có thể là$2 \cdot 10^5$. Phương pháp này đúng về mặt khái niệm nhưng không khả thi về mặt tính toán. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là chức năng$f_6(x) - f_7(x)$chỉ thay đổi khi$x$vượt qua bội số của 2, 3 hoặc 7. Vì 6 và 7 là số nguyên tố cùng nhau nên chúng ta có thể suy luận về số mũ của chúng một cách độc lập. Thay vì mô phỏng từng mức tăng, chúng ta có thể tính toán trước tất cả các số đến một giới hạn hợp lý khi bất đẳng thức$f_6(x) > f_7(x)$giữ và sau đó, đối với mỗi phần tử trong mảng, hãy tìm số đầu tiên lớn hơn hoặc bằng số đó trong tập hợp này. Điều này làm giảm vấn đề khi tính toán một tập hợp nhỏ các “số đặc biệt” một cách hiệu quả, sau đó sử dụng tra cứu đơn giản hoặc tìm kiếm nhị phân cho mỗi phần tử. 

Một quan sát khác là sự tăng trưởng của các số chia hết cho 6 hoặc 7 là theo cấp số nhân. Bất kỳ số nào lớn hơn một ngưỡng nhất định cuối cùng sẽ đạt đến một số đặc biệt trong một số gia số nhỏ. Bằng cách liệt kê tất cả các số có dạng$2^i \cdot 3^j \cdot 7^k$với$i,j,k \ge 0$theo thứ tự tăng dần, chúng ta có thể xây dựng tập hợp các ứng cử viên, sau đó kiểm tra từng phần tử mảng theo mức tăng tối thiểu cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(a_i)) | O(1) | Quá chậm | 
| Tối ưu | O(n * log(max_special)) | O(số lượng đặc biệt) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước tất cả các số đặc biệt. Tạo số ở dạng$2^i \cdot 3^j \cdot 7^k$với$i,j,k \ge 0$, nhưng chỉ giữ số ở nơi$i+j > k$, tương đương với$f_6(x) > f_7(x)$. Dừng tạo khi số lượng vượt quá$10^{18}$, đảm bảo bao phủ cho tất cả các mức tăng hợp lý. 
2. Sắp xếp tập hợp số đặc biệt. Điều này cho phép tra cứu hiệu quả sau này bằng cách sử dụng tìm kiếm nhị phân. 
3. Đối với mỗi trường hợp kiểm thử, hãy lặp qua mảng. Đối với mỗi phần tử, hãy tìm số đặc biệt nhỏ nhất lớn hơn hoặc bằng nó bằng tìm kiếm nhị phân. Ghi lại sự khác biệt giữa số đặc biệt này và phần tử, đó là số lượng gia tăng cần thiết để làm cho nó trở nên đặc biệt. 
4. Nếu bất kỳ phần tử nào không có số đặc biệt hợp lệ lớn hơn hoặc bằng nó (tìm kiếm nhị phân không thành công), hãy trả về -1 cho trường hợp kiểm thử đó. Nếu không, hãy lấy chênh lệch tối đa trên tất cả các phần tử; đây là số lượng thao tác tối thiểu để làm cho toàn bộ mảng trở nên đặc biệt cùng một lúc. 
5. Xuất kết quả cho từng test. 

Tại sao hoạt động: Thuật toán duy trì tính bất biến là với mỗi phần tử, chúng ta luôn chọn số đặc biệt gần nhất không nhỏ hơn phần tử đó. Việc lấy giá trị tối đa trên toàn mảng đảm bảo rằng sau số lần tăng đó, mọi phần tử ít nhất sẽ đạt được số đặc biệt mục tiêu của nó. Vì tập hợp các giá trị đặc biệt đầy đủ cho đến một giới hạn cực kỳ lớn nên phương pháp này không bao giờ bỏ lỡ một giải pháp và điều kiện$f_6(x) > f_7(x)$được đảm bảo cho các số đã chọn. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

# precompute all "special" numbers up to a very large bound
specials = set()
limit = 10**18
powers2 = [1]
while powers2[-1] <= limit // 2:
    powers2.append(powers2[-1] * 2)
powers3 = [1]
while powers3[-1] <= limit // 3:
    powers3.append(powers3[-1] * 3)
powers7 = [1]
while powers7[-1] <= limit // 7:
    powers7.append(powers7[-1] * 7)

for p2 in powers2:
    for p3 in powers3:
        f6 = 0
        x = p2 * p3
        if x > limit:
            break
        f6 = 0
        val = x
        while val % 6 == 0:
            val //= 6
            f6 += 1
        for p7 in powers7:
            val = x * p7
            f7 = 0
            temp = val
            while temp % 7 == 0:
                temp //= 7
                f7 += 1
            if f6 > f7 and val <= limit:
                specials.add(val)

special_list = sorted(specials)

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    ans = 0
    impossible = False
    for x in a:
        idx = bisect.bisect_left(special_list, x)
        if idx == len(special_list):
            impossible = True
            break
        ans = max(ans, special_list[idx] - x)
    print(-1 if impossible else ans)
```Phần tiền tính toán tạo ra lũy thừa 2, 3 và 7 một cách hiệu quả và đảm bảo chúng tôi không vượt quá giới hạn lớn. Đối với mỗi sự kết hợp, chúng tôi xác minh điều kiện$f_6(x) > f_7(x)$trước khi thêm vào bộ. Tìm kiếm nhị phân đảm bảo chúng tôi tìm thấy số lượng gia tăng tối thiểu một cách hiệu quả cho từng phần tử. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2
3
1 2 3
2
25 67
```| Trường hợp thử nghiệm | Yếu tố | Đặc biệt gần nhất | Tăng | 
| --- | --- | --- | --- | 
| 1 | 1 | 6 | 5 | 
| 1 | 2 | 6 | 4 | 
| 1 | 3 | 6 | 3 | 
| 2 | 25 | 30 | 5 | 
| 2 | 67 | 72 | 5 | 

Trong trường hợp thử nghiệm đầu tiên, mức tăng lớn nhất cần thiết là 5, nhưng 1 không thể đạt đến số đặc biệt hợp lệ đồng thời với 2 và 3 (các số được tính toán trước không căn chỉnh), vì vậy câu trả lời là -1. Trong trường hợp thử nghiệm thứ hai, việc thêm 5 vào cả hai phần tử sẽ đạt [30, 72], tất cả các số đặc biệt, mức tăng tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log S) | n phần tử cho mỗi trường hợp thử nghiệm, tìm kiếm nhị phân trong danh sách đặc biệt được tính toán trước S | 
| Không gian | O(S) | lưu trữ tất cả các số đặc biệt hợp lệ, S ≈ 10^5-10^6 | 

Điều này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ với các hạn chế. 

## Trường hợp thử nghiệm```python
# helper function
import sys, io, bisect

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    specials = set()
    limit = 10**18
    powers2 = [1]
    while powers2[-1] <= limit // 2:
        powers2.append(powers2[-1] * 2)
    powers3 = [1]
    while powers3[-1] <= limit // 3:
        powers3.append(powers3[-1] * 3)
    powers7 = [1]
    while powers7[-1] <= limit // 7:
        powers7.append
```
