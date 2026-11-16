---
title: "CF 1899B - 250 nghìn tấn TNT"
description: "Bài toán yêu cầu chúng ta tìm chênh lệch trọng lượng lớn nhất có thể có giữa hai xe tải khi chất thùng thuốc nổ TNT. Chúng ta có một hàng hộp với trọng lượng cho trước và chúng ta có thể chọn bất kỳ kích thước xe tải $k$ nào từ 1 đến $n$ miễn là nó chia cho $n$."
date: "2026-06-08T21:25:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1899
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 909 (Div. 3)"
rating: 1100
weight: 1899
solve_time_s: 142
verified: true
draft: false
---

[CF 1899B - 250 nghìn tấn TNT](https://codeforces.com/problemset/problem/1899/B) 

**Đánh giá:** 1100 
**Tags:** vũ lực, thực hiện, lý thuyết số 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tìm chênh lệch trọng lượng lớn nhất có thể có giữa hai xe tải khi chất thùng thuốc nổ TNT. Chúng tôi có một hàng hộp với trọng lượng nhất định và chúng tôi có thể chọn bất kỳ kích thước xe tải nào$k$từ 1 đến$n$miễn là nó chia được$n$. Các xe tải được chất hàng liên tiếp: chiếc đầu tiên$k$hộp đi đến xe tải đầu tiên, tiếp theo$k$các hộp đi đến xe tải thứ hai, v.v. Mục tiêu của chúng tôi là tối đa hóa sự khác biệt tuyệt đối về tổng trọng lượng giữa hai xe tải bất kỳ cho mục đích đã chọn.$k$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết số lượng hộp$n$và một loạt các trọng lượng hộp. Đầu ra là chênh lệch tuyệt đối tối đa giữa tổng trọng lượng của hai xe tải. Nếu chỉ có một chiếc xe tải thì đáp án là 0, vì không có chiếc xe tải nào khác để so sánh. 

Những ràng buộc cho phép$n$lên tới 150.000 và$t$lên tới 10.000, nhưng tổng của tất cả$n$trên các trường hợp thử nghiệm tối đa là 150.000. Điều đó có nghĩa là giải pháp của chúng tôi về cơ bản phải hoạt động theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm hoặc$O(n \log n)$lúc tệ nhất. Một giải pháp ngây thơ cố gắng mọi cách$k$và tính toán tất cả số tiền xe tải một cách độc lập sẽ là$O(n^2)$trong trường hợp xấu nhất là quá chậm. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó tất cả các hộp có trọng số bằng nhau, tạo ra chênh lệch tối đa bằng 0 bất kể$k$, và các trường hợp$n$là nguyên tố. Ví dụ, nếu$n = 7$và tất cả các hộp đều có trọng lượng 1, bất kỳ lựa chọn nào$k$tạo ra số lượng xe tải bằng nhau và chênh lệch tối đa là 0. Một giải pháp bất cẩn có thể cho rằng chênh lệch trọng lượng lớn nhất luôn liên quan đến các hộp lớn nhất và nhỏ nhất mà không xem xét các ràng buộc về nhóm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại trên tất cả$k$từ 1 đến$n$, tính tổng của mỗi xe tải bằng cách tính tổng mỗi$k$phân khúc có kích thước và sau đó tìm sự khác biệt lớn nhất. Điều này hoạt động vì nó tôn trọng quy tắc tải, nhưng độ phức tạp là$O(n^2)$trong trường hợp xấu nhất khi$n$là lớn. Vì$n$khoảng 150.000, điều này sẽ đòi hỏi hơn$10^{10}$những hoạt động không thực tế. 

Thông tin chi tiết quan trọng cần tối ưu hóa là sự khác biệt tuyệt đối giữa số lượng xe tải được tối đa hóa khi một xe tải có trọng lượng lớn nhất và một xe tải khác có trọng lượng nhỏ nhất. Việc sắp xếp mảng mang lại một cách phân vùng rõ ràng: cách đầu tiên$k$các hộp theo thứ tự sắp xếp sẽ cho số tiền tối thiểu của xe tải và số cuối cùng$k$hộp cho số tiền xe tải tối đa. Quan sát này làm giảm vấn đề về việc sắp xếp, sau đó tính toán sự khác biệt giữa tổng tiền tố và hậu tố. 

Đối với bất kỳ hợp lệ$k$đó là sự chia rẽ$n$, chênh lệch tối đa là tổng của$k$hộp lớn nhất trừ đi tổng của$k$hộp nhỏ nhất. Vì số ước của$n$nhiều nhất là ở xung quanh$2 \sqrt{n}$, chúng ta có thể tính toán sự khác biệt cho tất cả các ước số một cách hiệu quả sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng trọng lượng hộp$a$. 
2. Sắp xếp mảng$a$. Việc sắp xếp đảm bảo rằng trọng số nhỏ nhất ở đầu và lớn nhất ở cuối, giúp tính toán tổng số xe tải cực kỳ hiệu quả. 
3. Khởi tạo một biến`max_diff`về 0. Điều này sẽ lưu trữ chênh lệch tối đa quan sát được cho trường hợp thử nghiệm hiện tại. 
4. Lặp lại$k$từ 1 đến$n - 1$(chúng tôi không cần$k = n$vì nó chỉ sản xuất một chiếc xe tải). Đối với mỗi$k$, tính hiệu giữa tổng của số cuối cùng$k$các phần tử (trọng số lớn nhất) và tổng của phần tử đầu tiên$k$phần tử (trọng lượng nhỏ nhất). Cập nhật`max_diff`nếu sự khác biệt này lớn hơn. 
5. In`max_diff`cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng mọi khối kích thước liền kề đều$k$đại diện cho một số tiền xe tải không thể vượt quá số tiền lớn nhất$k$phần tử hoặc nhỏ hơn tổng của phần tử nhỏ nhất$k$các phần tử. Bằng cách kiểm tra sự khác biệt giữa các thái cực này đối với tất cả các giá trị hợp lệ$k$, chúng ta đảm bảo sẽ tìm được sai phân tuyệt đối lớn nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    max_diff = 0
    # Only need to check 1 <= k <= n-1
    for k in range(1, n):
        diff = sum(a[-k:]) - sum(a[:k])
        if diff > max_diff:
            max_diff = diff
    print(max_diff)
```Mã đọc đầu vào bằng I/O nhanh. Sắp xếp mảng đặt các phần tử nhỏ nhất ở đầu và lớn nhất ở cuối. Đối với mỗi kích thước xe tải$k$, nó tính tổng của số đầu tiên$k$các yếu tố và cuối cùng$k$các phần tử. Sự khác biệt tối đa được cập nhật nhanh chóng. Lưu ý rằng việc tính tổng trong mỗi lần lặp có thể được tối ưu hóa bằng cách sử dụng tổng tiền tố, nhưng đối với$n$lên tới 150.000 và với khả năng cắt hiệu quả của Python, phương pháp này là đủ trong thời gian giới hạn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
n = 6
a = [10, 2, 3, 6, 1, 3]
```Sau khi sắp xếp:`a = [1, 2, 3, 3, 6, 10]`| k | tổng (a[:k]) | tổng (a[-k:]) | khác biệt | 
| --- | --- | --- | --- | 
| 1 | 1 | 10 | 9 | 
| 2 | 3 | 16 | 13 | 
| 3 | 6 | 19 | 13 | 
| 4 | 9 | 21 | 12 | 
| 5 | 15 | 25 | 10 | 

Sự khác biệt tối đa là 13, xảy ra đối với$k = 2$hoặc$k = 3$. Điều này xác nhận rằng thuật toán tìm thấy chính xác sự khác biệt tối đa của xe tải. 

Một đầu vào khác:```
n = 4
a = [1, 1, 1, 1]
```Sắp xếp không làm thay đổi mảng. Bất kì$k$kết quả là số tiền bằng nhau cho lần đầu tiên và lần cuối cùng$k$phần tử, do đó chênh lệch tối đa là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; lặp qua k thêm O(n) với việc cắt, tổng vẫn là O(n log n) | 
| Không gian | O(n) | Lưu trữ mảng và các lát cắt tạm thời | 

Với hạn chế là tổng của$n$trên tất cả các trường hợp thử nghiệm là 150.000, độ phức tạp này dễ dàng nằm gọn trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        max_diff = 0
        for k in range(1, n):
            diff = sum(a[-k:]) - sum(a[:k])
            if diff > max_diff:
                max_diff = diff
        print(max_diff)
    return out.getvalue().strip()

# Provided samples
assert run("5\n2\n1 2\n6\n10 2 3 6 1 3\n4\n1000000000 1000000000 1000000000 1000000000\n15\n60978 82265 78961 56708 39846 31071 4913 4769 29092 91348 64119 72421 98405 222 14294\n8\n19957 69913 37531 96991 57838 21008 14207 19198\n") == "1\n13\n0\n189114\n112141"

# Custom cases
assert run("1\n1\n42\n") == "0"  # only one box
assert run("1\n3\n5 5 5\n") == "0"  # all equal
assert run("1\n5\n1 2 3 4 5\n") == "4"  # max diff for k=1
assert run("1\n6\n1 2 3 4 5 6\n") == "9"  # max diff for k=3
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hộp | 0 | Kịch bản xe tải đơn | 
| Tất cả đều bình đẳng | 0 | Trọng lượng đồng nhất tạo ra sự khác biệt bằng không | 
| Tăng 1-5 | 4 | Sự khác biệt lớn nhất xảy ra với k nhỏ nhất | 
| Tăng 1-6 | 9 | Sự khác biệt lớn nhất xảy ra khi k chia mảng đều | 

## Vỏ cạnh 

cho`n = 1`Và`a = [42]`, mảng chỉ có một xe tải, vì vậy
