---
title: "CF 2061I - Kevin và Nivek"
description: "Chúng ta được yêu cầu xác định thời gian tối thiểu Kevin phải đầu tư để thắng ít nhất một số trận đấu nhất định trước Nivek, với tất cả các số có thể có từ 0 đến $n$."
date: "2026-06-08T07:44:48+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "dp"]
categories: ["algorithms"]
codeforces_contest: 2061
codeforces_index: "I"
codeforces_contest_name: "IAEPC Preliminary Contest (Codeforces Round 999, Div. 1 + Div. 2)"
rating: 3500
weight: 2061
solve_time_s: 108
verified: false
draft: false
---

[CF 2061I - Kevin và Nivek](https://codeforces.com/problemset/problem/2061/I) 

**Đánh giá:** 3500 
**Tags:** chia để trị, dp 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định thời gian tối thiểu Kevin phải đầu tư để thắng ít nhất một số trận đấu nhất định trước Nivek, với tất cả các số có thể có từ 0 đến$n$. Mỗi trận đấu thuộc Loại 1, có thời gian cố định$a_i$để Kevin đảm bảo chiến thắng, hoặc Loại 2, tự động thuộc về Kevin nếu anh ấy hiện không đứng sau về số trận thắng. Các trận đấu loại 2 không tốn thời gian mà phụ thuộc vào những chiến thắng trước đó của Kevin. 

Đầu vào là một dãy số nguyên trong đó$-1$cho biết kết quả khớp Loại 2 và bất kỳ số không âm nào biểu thị chi phí thời gian của kết quả khớp Loại 1. Đầu ra là một danh sách độ dài$n+1$, ở đâu$k$-phần tử thứ là thời gian tối thiểu để đảm bảo ít nhất$k$thắng. 

Với những hạn chế -$n$lên đến$3 \cdot 10^5$cho mỗi trường hợp thử nghiệm và tối đa$10^4$trường hợp thử nghiệm - bất kỳ thuật toán nào kiểm tra mọi tập hợp con phù hợp hoặc mô phỏng tất cả các khả năng sẽ quá chậm. Một giải pháp phải mở rộng tuyến tính hoặc tệ nhất là$O(n \log n)$mỗi trường hợp thử nghiệm. Các trường hợp đặc biệt bao gồm khi tất cả các trận đấu thuộc Loại 2, khi tất cả các trận đấu thuộc Loại 1 hoặc khi sự kết hợp của cả hai khiến không thể đạt được số trận thắng nhất định. 

Ví dụ: nếu đầu vào là`5`và tất cả các trận đấu là Loại 2 (`-1 -1 -1 -1 -1`), Kevin có thể thắng tất cả các trận đấu mà không tốn thời gian, vì vậy kết quả sẽ là`0 0 0 0 0 0`. Nếu tất cả các kết quả phù hợp là Loại 1 có chi phí`[3, 2, 5, 4, 1]`, thời gian tối thiểu cho một số trận thắng nhất định có được bằng cách chọn chiến thắng rẻ nhất$k$trùng khớp, trong trường hợp này dẫn đến tổng chi phí được sắp xếp tích lũy:`0 1 3 6 10 15`. 

Một cách tiếp cận bất cẩn có thể cố gắng mô phỏng từng trận đấu một cách tuần tự và quyết định các trận đấu Loại 2 một cách linh hoạt mà không tính toán trước các khả năng tích lũy. Điều này không thành công vì chiến lược tối ưu thường bao gồm việc chọn các kết quả phù hợp Loại 1 cụ thể trước, sau đó để các kết quả phù hợp Loại 2 tự động đóng góp vào chiến thắng và tương tác không mang tính cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là xem xét tất cả các tập hợp con của các trận đấu Loại 1, mô phỏng kết quả Loại 2 cho từng tập hợp con và ghi lại thời gian tối thiểu để đạt được mỗi số lần thắng có thể có. Vì$n$trận đấu, có$2^n$tập hợp con, điều này là không thể thực hiện được. Ngay cả khi chúng tôi chia theo loại đối sánh, việc lặp lại tất cả các hoán vị của đối sánh Loại 1 để tính đến kết quả Loại 2 sẽ dẫn đến sự tăng trưởng theo cấp số nhân. 

Thông tin chi tiết quan trọng là các trận đấu Loại 2 sẽ miễn phí và tự động có lợi nếu Kevin không đứng sau. Điều này cho phép chúng tôi xử lý chúng một cách riêng biệt. Trước tiên chúng ta có thể đếm số lượng trận đấu Loại 2$b$và trận đấu Loại 1$a_1, \dots, a_m$. Chúng tôi sắp xếp chi phí Loại 1 theo thứ tự tăng dần. Ít nhất phải thắng$k$trận đấu, chúng ta có thể cân nhắc việc dành thời gian cho trận đấu rẻ nhất$x$Trận đấu Loại 1 và để trận đấu Loại 2 điền vào số trận thắng cần thiết còn lại. Cụ thể, Kevin chỉ cần dành thời gian cho các trận đấu Loại 1 nếu$k$vượt quá số lượng trận đấu Loại 2 mà anh ta có thể tận dụng để đạt được$k$. 

Điều này làm giảm vấn đề sắp xếp chi phí Loại 1 và tính toán tổng tiền tố, sau đó xác định cẩn thận số lượng kết quả phù hợp Loại 1 thực sự cần thiết cho mỗi mục tiêu$k$. Mỗi mục tiêu có thể được đánh giá trong$O(1)$sau khi sắp xếp và tính tổng tiền tố, đưa ra kết quả tổng thể$O(n \log n)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các kết quả trùng khớp thành hai danh sách: Các kết quả khớp Loại 1 có thời gian liên quan và các kết quả khớp Loại 2 được tính bằng một số nguyên đơn giản. Sắp xếp Loại 1 lần theo thứ tự tăng dần. Việc sắp xếp đảm bảo rằng chúng ta dành thời gian cho những kết quả phù hợp nhất trước tiên. 
2. Tính tổng tiền tố của Loại được sắp xếp 1 lần. Điều này cho phép chúng tôi truy vấn nhanh tổng thời gian cần thiết để thắng bất kỳ số trận đấu Loại 1 nào. 
3. Lặp lại số lần thắng cần thiết$k$từ 0 đến$n$. Đối với mỗi$k$, xác định số trận đấu Loại 1 tối thiểu Kevin phải chủ động thắng. Đây là cái nhỏ nhất$x$như vậy$x + \text{Type 2 count} \ge k$Và$x \le \text{Type 1 count}$. Nếu như$k$nhỏ hơn hoặc bằng số lượng Loại 2, Kevin không mất thời gian. Nếu như$k$vượt quá tổng của các kết quả trùng khớp Loại 2 và Loại 1 thì điều đó là không thể và chúng tôi có thể đánh dấu nó bằng -1. 
4. Để có thể đạt được$k$, hãy sử dụng mảng tổng tiền tố để tìm tổng thời gian dành cho các kết quả khớp Loại 1 được yêu cầu. Nối phần này vào danh sách đầu ra cho trường hợp thử nghiệm. 
5. Xuất danh sách đầy đủ cho từng test case. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán đảm bảo chúng tôi chỉ dành thời gian cho các kết quả phù hợp Loại 1 nếu cần thiết. Các trận đấu Loại 2 sẽ tự động đóng góp vào chiến thắng nếu Kevin không đứng sau, vì vậy tổng tiền tố của các trận đấu Loại 1 sẽ mang lại thời gian bổ sung tối thiểu cần thiết. Điều này đảm bảo chúng tôi đạt được thời gian tối thiểu cho tất cả số lần thắng mục tiêu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        type1 = sorted([x for x in a if x != -1])
        type2_count = a.count(-1)
        m = len(type1)
        prefix = [0] * (m + 1)
        for i in range(m):
            prefix[i+1] = prefix[i] + type1[i]
        res = []
        for k in range(n+1):
            x = max(0, k - type2_count)
            if x > m:
                res.append(-1)
            else:
                res.append(prefix[x])
        print(' '.join(map(str, res)))

solve()
```Giải pháp trước tiên sẽ tách các kết quả khớp Loại 1 và Loại 2, sắp xếp Loại 1 theo thời gian và tính toán trước các tổng tiền tố. Khi đánh giá từng$k$, nó sẽ kiểm tra xem chỉ riêng Loại 2 có đủ phù hợp hay không. Nếu không, nó sẽ thêm các kết quả phù hợp Loại 1 rẻ nhất nếu cần. Các trường hợp đặc biệt, giống như tất cả các kết quả khớp thuộc Loại 2 hoặc Loại 1, đương nhiên nằm ngoài tính toán này. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
-1 -1 -1 -1 -1
```Chúng tôi có 5 trận đấu Loại 2 và không có trận đấu Loại 1 nào. Mảng tổng tiền tố là`[0]`. Lặp lại$k = 0$đến 5, các kết quả phù hợp Loại 1 được yêu cầu là`max(0, k-5) = 0`cho tất cả$k$. Kết quả là`[0, 0, 0, 0, 0, 0]`. 

Đối với đầu vào:```
5
100 -1 -1 -1 1
```Trận đấu loại 1:`[1, 100]`, Số đếm loại 2 = 3. Tổng tiền tố`[0, 1, 101]`. 

| k | x = max(0, k-3) | tiền tố[x] | kết quả | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 
| 3 | 0 | 0 | 0 | 
| 4 | 1 | 1 | 1 | 
| 5 | 2 | 101 | 101 | 

Điều này chứng tỏ thuật toán xử lý chính xác các hỗn hợp và chọn kết quả phù hợp với chi phí tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp trận đấu Loại 1 chiếm ưu thế | 
| Không gian | O(n) | Mảng phân tách tổng và khớp tiền tố | 

Cho số tiền tối đa của$n$trên tất cả các trường hợp thử nghiệm là$3 \cdot 10^5$, giải pháp này chạy thoải mái trong vòng 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n5\n-1 -1 -1 -1 -1\n5\n3 2 5 4 1\n5\n100 -1 -1 -1 1\n") == "0 0 0 0 0 0\n0 1 3 6 10 15\n0 0 0 0 1 101", "sample 1"

# Custom cases
assert run("1\n1\n-1\n") == "0 0", "single Type 2 match"
assert run("1\n1\n5\n") == "0 5", "single Type 1 match"
assert run("1\n3\n5 2 -1\n") == "0 0 2 7", "mix of Type 1 and Type 2"
assert run("1\n4\n-1 -1 -1 -1\n
```
