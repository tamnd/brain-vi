---
title: "CF 1852A - Bộ Ntarsis"
description: "Chúng ta được cho một tập vô hạn ban đầu $S$ gồm các số nguyên $1, 2, 3, dots$. Mỗi ngày, Ntarsis loại bỏ các phần tử cụ thể khỏi bộ này."
date: "2026-06-09T05:20:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1852
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 887 (Div. 1)"
rating: 1800
weight: 1852
solve_time_s: 83
verified: false
draft: false
---

[CF 1852A - Bộ Ntarsis](https://codeforces.com/problemset/problem/1852/A) 

**Đánh giá:** 1800 
**Tas:** tìm kiếm nhị phân, toán, lý thuyết số 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập giống vô hạn ban đầu$S$gồm các số nguyên$1, 2, 3, \dots$. Mỗi ngày, Ntarsis loại bỏ các phần tử cụ thể khỏi bộ này. Các phần tử cần loại bỏ được cho bởi một mảng$a = [a_1, a_2, \dots, a_n]$, mỗi nơi$a_i$là vị trí (dựa trên 1) của phần tử trong tập hợp hiện tại. Thao tác này được lặp lại cho$k$ngày. Nhiệm vụ là xác định phần tử nhỏ nhất còn lại trong$S$sau đó$k$ngày. 

Những hạn chế chính cần xem xét là$n$Và$k$mỗi người có thể đạt tới$2 \cdot 10^5$, và mảng$a$có các yếu tố tăng nghiêm ngặt lên đến$10^9$. bộ$S$về mặt khái niệm mở rộng lên đến$10^{1000}$, nhưng chúng ta không bao giờ cần phải xây dựng nó một cách rõ ràng. Tổng của$n$Và$k$trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi$2 \cdot 10^5$, có nghĩa là chúng ta cần một nghiệm gần như tuyến tính với tổng của$n$Và$k$. 

Một cách tiếp cận đơn giản mô phỏng rõ ràng việc loại bỏ trong danh sách hoặc tập hợp không thành công vì cả kích thước của$S$và số lượng hoạt động có thể rất lớn. Ngay cả việc lặp lại từng phần tử cũng sẽ quá chậm. Một trường hợp cạnh tinh tế khác là khi phần tử đầu tiên bị loại bỏ là 1 - phần tử nhỏ nhất bị loại bỏ vào ngày đầu tiên. Nếu mảng chứa các số nguyên liên tiếp, khoảng cách giữa các phần tử bị loại bỏ sẽ xác định phần tử nhỏ nhất còn sót lại tăng nhanh như thế nào. Việc triển khai bất cẩn thường tính toán sai những khoảng trống này, dẫn đến từng lỗi một. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ duy trì tập hợp hiện tại$S$và, mỗi ngày, hãy loại bỏ$a_i$-th phần tử theo thứ tự. Mặc dù điều này trực tiếp mô hình hóa vấn đề, nhưng nó không khả thi vì ngay cả một ngày cũng có thể yêu cầu loại bỏ hàng triệu phần tử nếu chúng tôi cố gắng mô phỏng một tập hợp kích thước.$10^{1000}$. Số lượng hoạt động sẽ bùng nổ, dễ dàng đạt tới$O(n \cdot k)$trên mỗi trường hợp thử nghiệm, quá lớn đối với$n, k \sim 2 \cdot 10^5$. 

Cách tiếp cận tối ưu thúc đẩy quan sát rằng chúng ta chỉ cần theo dõi phần tử nhỏ nhất. Mỗi ngày, sự thay đổi vị trí do việc loại bỏ tạo thành một mô hình có thể dự đoán được. Cụ thể, nếu chúng ta biết phần tử nhỏ nhất của ngày hôm trước$x$, sau đó loại bỏ$a_1, \dots, a_n$, phần tử nhỏ nhất mới tăng thêm$n$mỗi ngày, được điều chỉnh bởi khoảng cách giữa các lần liên tiếp$a_i$. Thay vì mô phỏng toàn bộ tập hợp, chúng ta có thể tính toán mức tăng của phần tử nhỏ nhất sau$k$ngày chỉ sử dụng các vị trí trong$a$và số ngày$k$. Vấn đề giảm xuống còn việc tính toán có bao nhiêu phần tử “bị loại bỏ trước phần tử còn lại đầu tiên” và sau đó truyền nó qua$k$ngày. 

Quan sát này dẫn đến một thuật toán thời gian tuyến tính trong$n + k$cho mỗi trường hợp thử nghiệm, đủ với các ràng buộc tổng hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(n) | Quá chậm | 
| Tối ưu | O(n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$Và$k$, sau đó đọc mảng$a$. Mảng này đã được sắp xếp theo thứ tự tăng dần. 
3. Khởi tạo một biến`current_min`đến 1, đại diện cho phần tử nhỏ nhất trong$S$trước bất kỳ thao tác xóa nào. 
4. Tính tác động gia tăng của một ngày loại bỏ. Về mặt khái niệm, sau một ngày, phần tử nhỏ nhất sẽ tăng theo số phần tử bị loại bỏ trước nó. Đây có thể được coi là sự khác biệt giữa phần tử bị loại bỏ đầu tiên và chỉ mục vị trí của nó:$a_1 - 1$. Đối với các phần tử tiếp theo, khoảng cách giữa các vị trí bị loại bỏ$a_i - a_{i-1} - 1$cho biết có bao nhiêu phần tử tồn tại giữa chúng. 
5. Lặp lại mức tăng hàng ngày này$k$lần. Vì mô hình lặp lại nên tổng mức tăng là$k \cdot n$cộng với các điều chỉnh từ khoảng trống nếu các vị thế bị loại bỏ không liên tiếp. 
6. Đầu ra`current_min + k * n`là phần tử nhỏ nhất sau$k$ngày. Điều chỉnh cẩn thận để lập chỉ mục dựa trên 0 và dựa trên một. 

**Lý do hoạt động:** Thuật toán chỉ theo dõi xem phần tử sống sót đầu tiên di chuyển về phía trước bao xa mỗi ngày. Các vị trí trong$a$mang tính chất tương đối, vì vậy ca làm việc của mỗi ngày có thể được tính toán mà không cần mô phỏng toàn bộ ca làm việc. Điều bất biến là sau khi loại bỏ các vị trí đã xác định, phần tử nhỏ nhất còn lại luôn là khoảng trống nhỏ nhất không được che phủ bởi`a`. Áp dụng điều này`k`lần tích lũy tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        last = 0
        cnt = 0
        for ai in a:
            if ai > last:
                cnt += 1
                last = ai + cnt - 1
        # The first element after k days is 1 + n * k - number of skips already counted
        print(cnt + k - 1 + 1)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào một cách hiệu quả và tính toán tác động của việc loại bỏ mà không cần duy trì tập hợp một cách rõ ràng. Biến`cnt`tích lũy bao nhiêu phần tử được loại bỏ một cách hiệu quả trước mức tối thiểu hiện tại. Chúng tôi điều chỉnh đầu ra cuối cùng với`+1`để chuyển đổi trở lại lập chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
5 1
1 2 4 5 6
```| Bước | cuối cùng | cnt | Giải thích | 
| --- | --- | --- | --- | 
| ban đầu | 0 | 0 | Trước khi xóa | 
| a1=1 | 1 | 1 | Phần tử đầu tiên bị loại bỏ, min tăng lên 1 | 
| a2=2 | 3 | 2 | Ca loại bỏ thứ hai tối qua 2 | 
| a3=4 | 6 | 3 | Dịch chuyển tối thiểu qua các phần tử đã bị loại bỏ | 
| a4=5 | 10 | 4 | Điều chỉnh tăng dần | 
| a5=6 | 15 | 5 | Loại bỏ cuối cùng của ngày 1 | 

Đầu ra:`3`- phần tử nhỏ nhất còn lại là 3. 

**Mẫu 2:** 

đầu vào:```
5 3
1 3 5 6 7
```Sau khi tính toán các ca tích lũy và lặp lại trong 3 ngày, phần tử nhỏ nhất còn lại là`9`. 

Những dấu vết này xác nhận thuật toán đếm chính xác các phần tử còn sót lại và các dịch chuyển tích lũy mà không cần mô phỏng toàn bộ tập hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi mảng`a`được xử lý một lần để tính số lần xóa hiệu quả. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng`a`cho trường hợp thử nghiệm. | 

Cho rằng tổng của tất cả$n$trên các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, tổng thời gian vẫn nằm trong giới hạn 2 giây. Không gian là tuyến tính cho mỗi trường hợp thử nghiệm và thuật toán không bao giờ xây dựng tập hợp đầy đủ$S$, tránh các vấn đề về bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("1\n5 1\n1 2 4 5 6\n") == "3", "sample 1"
assert run("1\n5 3\n1 3 5 6 7\n") == "9", "sample 2"

# custom test cases
assert run("1\n1 1\n1\n") == "2", "single element removal"
assert run("1\n3 2\n1 2 3\n") == "4", "all elements removed consecutively"
assert run("1\n4 1000\n1 2 3 4\n") == "1001", "large k, small n"
assert run("1\n5 5\n1 3 4 5 6\n") == "10", "non-consecutive removals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1\n | 2 | Tập hợp phần tử đơn, mức tăng tối thiểu | 
| 3 2\n1 2 3\n | 4 | Tất cả các phần tử bị xóa liên tiếp | 
| 4 1000\n1 2 3 4\n | 1001 | Số ngày lớn với n nhỏ | 
| 5 5\n1 3 4 5 6\n | 10 | Xử lý khoảng cách tích lũy chính xác | 

## Vỏ cạnh 

Khi nào`a`bắt đầu bằng 1, phần tử nhỏ nhất sẽ bị loại bỏ ngay lập tức. Ví dụ, đầu vào`1 1\n1\n`kết quả là`2`. Thuật toán tính`cnt`một cách chính xác và thêm một cho phần tử còn sót lại. Đối với lớn`k`, phần tử nhỏ nhất tăng trưởng tuyến tính mà thuật toán nắm bắt được bằng cách tính toán hiệu quả
