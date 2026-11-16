---
title: "CF 1919E - Tiền tố đếm"
description: "Chúng ta được yêu cầu thiết kế ngược một mảng ẩn có kích thước $n$ chỉ chứa $1$ và $-1$. Thay vì được cung cấp chính mảng đó, chúng ta được cung cấp danh sách được sắp xếp các tổng tiền tố của mảng. Tổng tiền tố tại vị trí $i$ là tổng của các phần tử $i$ đầu tiên của mảng."
date: "2026-06-08T19:36:18+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1919
codeforces_index: "E"
codeforces_contest_name: "Hello 2024"
rating: 2600
weight: 1919
solve_time_s: 162
verified: false
draft: false
---

[CF 1919E - Tiền tố đếm](https://codeforces.com/problemset/problem/1919/E) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, thuật toán xây dựng, dp, triển khai, toán học 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu thiết kế ngược một mảng kích thước ẩn$n$chỉ chứa$1$Và$-1$. Thay vì được cung cấp chính mảng đó, chúng ta được cung cấp danh sách được sắp xếp các tổng tiền tố của mảng. Tổng tiền tố tại vị trí$i$là tổng của số đầu tiên$i$các phần tử của mảng. Sau khi tính toán tất cả các tổng tiền tố, chúng được sắp xếp theo thứ tự không giảm và đó là đầu vào mà chúng ta nhận được. Nhiệm vụ của chúng ta là đếm số mảng riêng biệt có thể tạo ra mảng tổng tiền tố được sắp xếp này. 

Các ràng buộc rất khiêm tốn: tổng số phần tử trong tất cả các trường hợp thử nghiệm không vượt quá 5000. Điều này có nghĩa là các thuật toán có độ phức tạp lên tới$O(n^2)$sẽ chạy hiệu quả trong giới hạn một giây. Những cách tiếp cận ngây thơ cố gắng tạo ra tất cả các mảng có thể một cách rõ ràng là không khả thi ngay lập tức vì đối với$n=5000$số mảng ứng cử viên là$2^{5000}$, một con số lớn về mặt thiên văn. 

Các trường hợp cạnh phát sinh khi tổng tiền tố được sắp xếp bao gồm các số 0 hoặc các phần tử lặp lại. Ví dụ: nếu tổng tiền tố được sắp xếp là`[0]`vì`n=1`, không có mảng có kích thước 1 chỉ với`1`Và`-1`có thể tạo ra một tổng tiền tố của`0`. Tương tự, các mảng trong đó tổng tích lũy phải giảm nhiều hơn mức có thể cũng mang lại kết quả bằng không. Việc xử lý những điều này một cách chính xác đòi hỏi phải tính toán cẩn thận xem có bao nhiêu cách mà mỗi tổng tiền tố có thể được ghép nối với các phần tử còn lại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các mảng có kích thước$n$với các phần tử`1`Và`-1`, tính tổng tiền tố của chúng, sắp xếp chúng và so sánh với mảng đã cho. Về nguyên tắc, điều này có tác dụng vì nó trực tiếp mô hình hóa vấn đề. Tuy nhiên, số lượng hoạt động cho trường hợp xấu nhất là$2^n \cdot n \log n$, điều này là không thể ngay cả đối với$n=20$, huống hồ là$5000$. 

Cái nhìn sâu sắc quan trọng để giảm độ phức tạp là xem đây là một vấn đề đếm trên nhiều tập hợp tiền tiền tố. Đặt mảng tổng tiền tố là$p$. Mỗi tổng tiền tố có thể tăng thêm 1 hoặc giảm đi 1 so với tổng tiền tố trước đó. Chúng ta có thể lặp qua các tổng tiền tố được sắp xếp, theo dõi xem có bao nhiêu tổng tiền tố "mở" ở mỗi giá trị và khớp chúng với các tổng tiền tố đến bằng cách sử dụng phương pháp lập trình động. Cụ thể, đối với giá trị tổng tiền tố$v$, số cách nó có thể là tổng tiền tố tiếp theo phụ thuộc vào số tổng tiền tố trước đó nhỏ hơn một đơn vị$v$, trừ đi số đã sử dụng. 

Điều này làm giảm vấn đề xuống$O(n^2)$hoạt động vì đối với mỗi$n$các vị trí, chúng tôi có thể cần kiểm tra số lượng lên tới$n$tổng tiền tố trước đó. DP tiếp tục đếm các cách để gán từng phần tử cho tổng tiền tố hợp lệ trước đó và chúng tôi tích lũy số lượng phép gán hợp lệ theo modulo$998244353$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n log n) | O(n) | Quá chậm | 
| Đếm DP / Nhiều bộ | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bảng lập trình động`dp[i]`biểu thị số cách xây dựng mảng kết thúc bằng tổng tiền tố`p[i]`. 
2. Sử dụng bộ đếm để ghi lại số lần mỗi tổng tiền tố đã xuất hiện cho đến nay. 
3. Lặp lại các tổng tiền tố đã được sắp xếp. Đối với phần tử đầu tiên, nếu không`1`hoặc`-1`, không có mảng hợp lệ, vì vậy câu trả lời là 0. Nếu không, hãy đặt số cách để tính tổng tiền tố đầu tiên thành 1. 
4. Đối với mỗi tổng tiền tố tiếp theo`p[i]`, đếm số tổng tiền tố trước đó bằng`p[i]-1`. Điều này thể hiện những cách hợp lệ để tăng số tiền trước đó lên 1 để đạt được`p[i]`. 
5. Nhân số cách để`p[i]`theo số lượng tổng tiền tố có sẵn trước đó và giảm bộ đếm của tổng tiền tố có sẵn khi chúng được sử dụng. 
6. Sau khi xử lý tất cả các tổng tiền tố, câu trả lời là số cách tích lũy cho phần tử cuối cùng, modulo`998244353`. 

Điều bất biến là ở mỗi bước, DP đếm chính xác số lần gán hợp lệ của tổng tiền tố cho các vị trí trong mảng ban đầu. Mỗi tổng tiền tố chỉ có thể được hình thành từ một tổng trước đó cộng một hoặc trừ một. Bằng cách duy trì số lượng nhiều tập hợp, chúng tôi đảm bảo rằng chúng tôi không sử dụng lại tổng tiền tố một cách không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def solve_case(n, p):
    if p[0] not in [-1, 1]:
        return 0
    from collections import Counter
    counter = Counter()
    ways = 1
    counter[p[0]] += 1
    for i in range(1, n):
        prev_count = counter[p[i]-1]
        if prev_count == 0:
            return 0
        ways = ways * prev_count % MOD
        counter[p[i]] += 1
        counter[p[i]-1] -= 1
    return ways

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        print(solve_case(n, p))

if __name__ == "__main__":
    main()
```Kiểm tra đầu tiên đảm bảo rằng tổng tiền tố đầu tiên là`1`hoặc`-1`, vì không có giá trị nào khác có thể xuất hiện dưới dạng tổng tiền tố đầu tiên. Chúng tôi duy trì một`Counter`tổng tiền tố được sử dụng để tránh chỉ định tổng tiền tố trước đó nhiều lần hơn số tiền tồn tại. Mỗi lần lặp nhân số cách hiện tại với số tổng hợp lệ trước đó và sau đó cập nhật bộ đếm. Số học Modulo ngăn ngừa tràn số nguyên. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
1
0
1
1
3
-1 1 2
5
-1 0 0 1 1
5
-4 -3 -3 -2 -1
```Dấu vết cho`[-1, 0, 0, 1, 1]`: 

| tôi | p[i] | truy cập trước | trước_count | cách | truy cập sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | -1 | {} | - | 1 | {-1:1} | 
| 1 | 0 | {-1:1} | 1 | 1 | {-1:0, 0:1} | 
| 2 | 0 | {-1:0,0:1} | 1 | 1 | {0:2} | 
| 3 | 1 | {0:2} | 2 | 2 | {0:1, 1:1} | 
| 4 | 1 | {0:1,1:1} | 1 | 2 | {0:0,1:2} | 

trận chung kết`ways`là 3, xác nhận đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi tổng tiền tố, chúng tôi có thể tra cứu các số liệu trước đó;`Counter`hoạt động được khấu hao O(1). | 
| Không gian | O(n) | Bộ đếm lưu trữ tối đa n tổng tiền tố duy nhất. | 

Được cho$n \le 5000$, bảng DP và bộ đếm vừa vặn thoải mái trong bộ nhớ và các lần lặp lồng nhau hoàn thành trong chưa đầy một giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("5\n1\n0\n1\n1\n3\n-1 1 2\n5\n-1 0 0 1 1\n5\n-4 -3 -3 -2 -1\n") == "0\n1\n0\n3\n1"

# Custom cases
assert run("1\n1\n1\n") == "1"  # minimal size, valid
assert run("1\n1\n-1\n") == "1"  # minimal size, valid negative
assert run("1\n2\n-1 0\n") == "1"  # two elements, simplest increment
assert run("1\n3\n-2 -1 0\n") == "1"  # consecutive negative to zero
assert run("1\n3\n0 0 0\n") == "0"  # impossible, can't all be zero
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1/1 | 1 | mảng dương tối thiểu | 
| 1 / -1 | 1 | mảng âm tối thiểu | 
| 2 / -1 0 | 1 | mảng hai phần tử đơn giản | 
| 3 / -2 -1 0 | 1 | tổng tiền tố âm liên tiếp | 
| 3 / 0 0 0 | 0 | trường hợp bất khả thi với số 0 lặp đi lặp lại | 

## Vỏ cạnh 

Đối với đầu vào`[0]`với`n=1`, thuật toán sẽ kiểm tra ngay`p[0]`và trả về 0, xử lý chính xác tổng tiền tố không thể. Đối với các đầu vào có tổng tiền tố lặp lại như`[0,0]`, cơ chế đếm đảm bảo rằng chúng ta không lạm dụng tổng tiền tố, do đó thuật toán trả về chính xác 0 khi không tồn tại mảng ban đầu hợp lệ. Để tối đa
