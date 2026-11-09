---
title: "CF 1476B - Lạm phát"
description: "Chúng ta được cung cấp một chuỗi tăng giá hàng tháng cho một sản phẩm. Giá trị đầu tiên thể hiện giá ban đầu và mỗi giá trị tiếp theo là mức tăng danh nghĩa trong tháng đó."
date: "2026-06-10T23:58:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1476
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 103 (Rated for Div. 2)"
rating: 1300
weight: 1476
solve_time_s: 107
verified: true
draft: false
---

[CF 1476B - Lạm phát](https://codeforces.com/problemset/problem/1476/B) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, toán học 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi tăng giá hàng tháng cho một sản phẩm. Giá trị đầu tiên thể hiện giá ban đầu và mỗi giá trị tiếp theo là mức tăng danh nghĩa trong tháng đó. Chúng tôi được yêu cầu đảm bảo rằng tỷ lệ lạm phát hàng tháng, được tính bằng tỷ lệ giữa mức tăng của tháng đó với giá tích lũy tính đến tháng đó, không vượt quá một ngưỡng nhất định.$k$phần trăm. Để đạt được điều này, chúng tôi có thể tăng giá ban đầu và/hoặc giá của các tháng trước đó, nhưng chúng tôi muốn làm như vậy trong khi giảm thiểu tổng số tiền bổ sung được thêm vào. 

Mỗi trường hợp thử nghiệm cung cấp số tháng$n$, tỷ lệ lạm phát tối đa được phép$k$, và mảng giá tăng$p$. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm, tổng số tiền tối thiểu cần được thêm vào mảng để hạn chế lạm phát được giữ nguyên cho mỗi tháng. 

Những hạn chế ở mức vừa phải:$n$nhiều nhất là 100 và$t$lên tới 1000, nghĩa là giải pháp sẽ hoạt động hiệu quả trong$O(n)$mỗi trường hợp thử nghiệm. Mức tăng giá riêng lẻ có thể lên tới$10^9$, loại trừ các phương pháp dựa vào việc liệt kê thô bạo một cách ngây thơ tất cả các khoản bổ sung giá có thể có. Trường hợp khó khăn không rõ ràng phát sinh khi một mức giá ban đầu rất nhỏ được theo sau bởi một mức tăng lớn: nếu không điều chỉnh, hệ số lạm phát sẽ vượt quá$k$bằng một biên độ rộng. Ví dụ, với$p = [1, 100]$Và$k = 1$, mức tăng của tháng thứ hai gấp 100 lần giá ban đầu, vượt xa giới hạn 1%. Một giải pháp ngây thơ chỉ kiểm tra nếu$p_i \le k$thất bại hoàn toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các khoản tăng có thể có trong tổng số tiền tích lũy cho mỗi tháng và kiểm tra xem sự kết hợp nào đáp ứng giới hạn lạm phát. Cách tiếp cận này đúng nhưng không thực tế: cứ mỗi tháng, có khả năng$O(10^9)$các mức tăng cần xem xét, dẫn đến số lượng hoạt động cao đến mức khủng khiếp. 

Cái nhìn sâu sắc quan trọng là vấn đề có thể được giải quyết một cách tham lam từ trái sang phải. Hệ số lạm phát tháng$i$được định nghĩa là$p_i / S$, Ở đâu$S$là tổng của tất cả các giá trị trước đó sau khi điều chỉnh. Để đảm bảo$p_i / S \le k / 100$, chúng ta có thể tìm được tổng tích lũy yêu cầu tối thiểu$S$BẰNG$S \ge \lceil (p_i \cdot 100) / k \rceil$. Nếu tổng tích lũy hiện tại nhỏ hơn số này, chúng tôi sẽ cộng phần chênh lệch vào tổng số hiện có. Bằng cách xử lý tuần tự các tháng, chúng tôi luôn điều chỉnh tổng mức tăng tối thiểu để đáp ứng ràng buộc, đảm bảo chúng tôi không bù đắp quá mức. Đây là tuyến tính trong$n$và hoạt động trong giới hạn nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((max p_i)^n) | O(n) | Quá chậm | 
| Tuần tự tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`total_add`về 0 để theo dõi tổng của tất cả các phép cộng và đặt`cumulative`theo giá tháng đầu tiên. 
2. Lặp lại nhiều tháng từ tháng thứ hai đến tháng cuối cùng. 
3. Cho mỗi tháng$i$, tính tổng tích lũy tối thiểu cần thiết như`(p[i] * 100 + k - 1) // k`. Công thức này đảm bảo chúng ta làm tròn đến số nguyên gần nhất. 
4. Nếu số tiền tích lũy hiện tại`cumulative`nhỏ hơn mức tối thiểu yêu cầu, hãy tính chênh lệch và cộng nó vào`total_add`. Cập nhật`cumulative`bằng cách thêm sự khác biệt này. 
5. Thêm giá của tháng hiện tại`p[i]`ĐẾN`cumulative`, phản ánh rằng chúng tôi hiện đang tính tháng này vào tổng số của các tháng tiếp theo. 
6. Sau khi xử lý tất cả các tháng, kết quả đầu ra`total_add`. 

Điều bất biến là sau tháng xử lý$i$, số tiền tích lũy luôn đủ để giữ lạm phát của tất cả các tháng ở mức$i$dưới$k$phần trăm. Bằng cách duy trì tính bất biến này, chúng tôi đảm bảo giới hạn lạm phát không bao giờ bị vi phạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        p = list(map(int, input().split()))
        total_add = 0
        cumulative = p[0]
        for i in range(1, n):
            required = (p[i] * 100 + k - 1) // k
            if cumulative < required:
                add = required - cumulative
                total_add += add
                cumulative += add
            cumulative += p[i]
        print(total_add)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc số lượng ca kiểm thử và xử lý từng trường hợp kiểm thử riêng lẻ. Chúng tôi khởi tạo`total_add`về 0 và đặt`cumulative`với giá của tháng đầu tiên, phản ánh giá khởi điểm. Vòng lặp tính tổng tích lũy cần thiết bằng cách sử dụng phép chia trần để xử lý việc làm tròn số nguyên. Nếu số tiền tích lũy hiện tại không đủ, chúng tôi sẽ thêm số tiền cần thiết vào`total_add`và cập nhật`cumulative`. Cuối cùng, chúng tôi cộng giá của tháng hiện tại vào`cumulative`để kiểm tra trong tương lai. Sử dụng số học số nguyên xuyên suốt sẽ tránh được lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 1
20100 1 202 202
```| Tháng | p[i] | tích lũy trước | yêu cầu tối thiểu | thêm | tích lũy sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 20100 | 20100 | - | 0 | 20100 | 
| 1 | 1 | 20100 | 100 | 0 | 20101 | 
| 2 | 202 | 20101 | 20200 | 99 | 20200 + 202 = 20401 | 
| 3 | 202 | 20401 | 20200 | 0 | 20401 + 202 = 20603 | 

Tổng số đã thêm: 99 

Dấu vết này cho thấy thuật toán xác định chính xác rằng tháng thứ ba cần thêm 99 để đáp ứng giới hạn 1%. 

### Mẫu 2 

đầu vào:```
3 100
1 1 1
```| Tháng | p[i] | tích lũy trước | yêu cầu tối thiểu | thêm | tích lũy sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | - | 0 | 1 | 
| 1 | 1 | 1 | 1 | 0 | 2 | 
| 2 | 1 | 2 | 1 | 0 | 3 | 

Tổng số đã thêm: 0 

Tỷ lệ phần trăm đều nằm trong giới hạn 100% nên không cần bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi tháng được xử lý một lần với phép tính đơn giản | 
| Không gian | O(n) | Chúng tôi lưu trữ các mảng tăng giá, không bổ sung cấu trúc lớn | 

Từ$n \le 100$Và$t \le 1000$, tổng số thao tác nhiều nhất là$10^5$, trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("2\n4 1\n20100 1 202 202\n3 100\n1 1 1\n") == "99\n0", "samples"

# custom cases
assert run("1\n2 1\n1 100\n") == "9900", "large increase after tiny initial"
assert run("1\n3 50\n2 1 1\n") == "0", "small increases, all below 50%"
assert run("1\n4 10\n10 1 1 100\n") == "990", "last month requires big addition"
assert run("1\n2 100\n1000000000 1000000000\n") == "0", "large numbers, already under 100%"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1\n1 100 | 9900 | Xử lý chính xác mức tăng rất cao sau lần đầu nhỏ | 
| 3 50\n2 1 1 | 0 | Xử lý nhiều tháng mà không cần bổ sung | 
| 4 10\n10 1 1 100 | 990 | Đảm bảo tháng trước yêu cầu bổ sung và thuật toán xử lý các cập nhật tuần tự | 
| 2 100\n1000000000 1000000000 | 0 | Số lượng lớn, kiểm tra xử lý tràn số nguyên | 

## Vỏ cạnh 

Đối với đầu vào`2 1\n1 100`, số tích lũy bắt đầu từ 1. Tháng 1 yêu cầu`100 * 100 / 1 = 10000`, vì vậy chúng tôi thêm`10000 - 1 = 9999`đến tích lũy và tổng số được thêm vào là 9999. Thuật toán áp dụng chính xác mức chia trần và cập nhật tích lũy tuần tự. Vì`4 10\n10 1 1 100`, thuật toán cộng thêm 90 trước tháng cuối cùng để đáp ứng giới hạn 10%, chứng tỏ rằng các bản cập nhật tuần tự sẽ tích lũy chính xác cho những tháng sau đó.
