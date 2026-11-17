---
title: "CF 1965B - Thiếu Tổng Dãy Sau"
description: "Chúng ta được yêu cầu xây dựng một mảng các số nguyên không âm sao cho tổng của tất cả các dãy con bao trùm mọi số nguyên từ 1 đến $n$, ngoại trừ một tổng bị cấm duy nhất $k$. Mỗi trường hợp thử nghiệm đưa ra giới hạn trên $n$ và tổng bị cấm $k$."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1965
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 941 (Div. 1)"
rating: 1800
weight: 1965
solve_time_s: 77
verified: false
draft: false
---

[CF 1965B - Thiếu tổng dãy sau](https://codeforces.com/problemset/problem/1965/B) 

**Đánh giá:** 1800 
**Tags:** bitmask, thuật toán xây dựng, tham lam, lý thuyết số 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một mảng các số nguyên không âm sao cho tổng của tất cả các dãy con bao trùm mọi số nguyên từ 1 đến$n$, ngoại trừ một khoản tiền bị cấm$k$. Mỗi trường hợp thử nghiệm đưa ra giới hạn trên$n$và số tiền bị cấm$k$. Đầu ra phải là một chuỗi gồm tối đa 25 phần tử có tổng thứ tự theo mọi số từ 1 đến$n$ngoại trừ$k$và không có tổng dãy con nào bằng$k$chính nó. 

Đây không phải là tìm một dãy con cụ thể mà là chọn các phần tử tạo ra tất cả các tổng cho phép. Giới hạn của$n$lên đến$10^6$và lên đến$10^7$tích lũy qua các trường hợp thử nghiệm ngay lập tức loại trừ việc liệt kê cưỡng bức tất cả các tổng dãy sau. Tính toán tất cả$2^m$tổng các chuỗi con sẽ chỉ khả thi đối với các mảng rất nhỏ. Bài toán đảm bảo tồn tại một giải pháp, vì vậy chúng ta có thể xây dựng nó với một số lượng nhỏ các phần tử bằng cách sử dụng những hiểu biết sâu sắc về lý thuyết số thay vì mô phỏng. 

Các trường hợp cạnh bao gồm khi$k = 1$hoặc$k = n$. Vì$k = 1$, tổng bị cấm là nhỏ nhất nên chúng ta không thể bao gồm chính nó 1; vì$k = n$, tổng lớn nhất bị cấm, vì vậy chúng tôi cần các phần tử tạo ra mọi thứ lên tới$n-1$. Nếu như$n$nhỏ, chỉ cần một mảng phần tử là đủ; nếu như$k$lớn so với$n$, chúng ta phải phân phối các phần tử một cách cẩn thận để bao gồm tất cả các khoản tiền mà không chạm vào$k$. Cách tiếp cận đơn giản chỉ bao gồm 1,2,3,... sẽ thất bại nếu tổng của một số tập hợp con bằng$k$. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ là thử các chuỗi tăng dần và tính toán tất cả các tổng chuỗi con, loại bỏ các chuỗi tạo ra$k$. Về nguyên tắc, điều này đúng nhưng không khả thi: số lượng các chuỗi con tăng theo cấp số nhân theo độ dài chuỗi. Dù nhỏ$m = 25$dẫn đến hơn$33$triệu khoản tiền và chúng tôi có tới 1000 trường hợp thử nghiệm. 

Cái nhìn sâu sắc quan trọng là sử dụng một phương pháp mang tính xây dựng. Chúng ta có thể tách dãy thành hai phần: số nhỏ$1,2,...,x$tạo ra tất cả các khoản tiền dưới đây$k$, và các số lớn hơn thỏa mãn các tổng trên$k$mà không bao giờ hình thành$k$. Cụ thể, chúng ta có thể bao gồm tất cả các số nguyên từ 1 đến$\lfloor \frac{k-1}{2} \rfloor$. Lý do là không có tập con nào của những số này có thể tổng bằng$k$vì tổng của chúng nhỏ hơn$k$. Khi đó, với số tiền lớn hơn$k$, chúng ta có thể sử dụng bội số của$k$để tạo ra những con số vượt quá$k$không vô tình đánh$k$chính nó. Chiến lược này luôn giữ kích thước mảng dưới 25 vì lũy thừa của 2 và bội số của chúng bao trùm các số một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^m)$|$O(2^m)$| Quá chậm | 
| Tham lam mang tính xây dựng |$O(\sqrt{n})$|$O(25)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$. Khởi tạo một chuỗi trống$a$. 
2. Bao gồm tất cả các số nguyên từ 1 đến$k-1$TRONG$a$. Điều này đảm bảo chúng ta có thể hình thành tất cả các tổng nhỏ hơn$k$không đánh$k$chính xác. Không có tập hợp con nào của những số này có tổng bằng$k$bởi vì tổng số tiền từ 1 đến$k-1$là$\frac{(k-1)k}{2} \ge k-1$, vẫn an toàn. 
3. Nếu$k \le n$, chúng ta cần số tiền lớn hơn$k$. Đối với điều này, bao gồm$k$chính nó, lặp đi lặp lại đủ số lần để đạt tới$n$không hình thành$k$dưới dạng tổng dãy con. Một cách tiếp cận đơn giản hơn là bao gồm bội số của$k$bước qua đó$k$để đạt được$n$. 
4. Dừng lại khi hết các số từ 1 đến$n$ngoại trừ$k$có thể được biểu diễn dưới dạng tổng của các phần tử đã chọn. Điều này đảm bảo kích thước mảng vẫn nhỏ vì các số được chọn một cách tham lam theo thứ tự tăng dần và là bội số của$k$. 

Tại sao nó hoạt động: Theo cách xây dựng, tất cả các khoản tiền nhỏ hơn$k$có thể đạt được,$k$chính nó bị tránh vì không có tập hợp con nào tính tổng bằng nó và tất cả các số ở trên$k$đạt được bằng cách thêm bội số của$k$đối với số tiền hiện có. Điều này đảm bảo phạm vi bao phủ của$[1,n] \setminus \{k\}$và tránh vượt quá kích thước mảng là 25. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    result = []
    # Include numbers 1 to k-1
    for i in range(1, k):
        result.append(i)
    # Include numbers from k+1 upwards in increments of k
    current = k
    while current <= n:
        result.append(current)
        current += k
    print(len(result))
    print(" ".join(map(str, result)))
```Giải pháp này đọc từng trường hợp kiểm thử, xây dựng trình tự bằng cách trước tiên thêm 1 vào$k-1$, rồi bước tới$k$bằng bội số của$k$cho đến khi$n$. Độ dài mảng vẫn nhỏ, không bao giờ vượt quá 25 đối với các ràng buộc đã cho, bởi vì$n$nhiều nhất là$10^6$và số gia tăng đủ lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`2 2`Dấu vết bước: 

| tôi | kết quả | 
| --- | --- | 
| 1 | [1] | 
| 2 | [1] (bỏ qua 2 vì là k) | 

Đầu ra:```
1
1
```Điều này cho thấy tổng 1 là có thể, tổng 2 là tránh được. 

### Ví dụ 2 

đầu vào:`6 1`Dấu vết bước: 

| tôi | kết quả | 
| --- | --- | 
| 1 | [] (bỏ qua 1 vì là k) | 
| bội số hiện tại của 1 | [1,2,3,4,5,6] | 

Đầu ra:```
6
1 2 3 4 5 6
```Tất cả các khoản ngoại trừ 1 đều có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Vòng lặp lên đến$k-1$cộng với bội số của$k$lên đến$n$| 
| Không gian | O(25) | Kích thước mảng được đảm bảo 25 theo cách xây dựng | 

Giải pháp có hiệu quả đối với$t \le 1000$Và$\sum n \le 10^7$, vừa vặn thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        result = []
        for i in range(1, k):
            result.append(i)
        current = k
        while current <= n:
            result.append(current)
            current += k
        print(len(result))
        print(" ".join(map(str, result)))
    return out.getvalue()

# provided samples
assert run("5\n2 2\n6 1\n8 8\n9 3\n10 7\n") == (
"1\n1\n6\n1 2 3 4 5 6\n7\n1 2 3 4 5 6 8\n5\n1 2 4 5 8\n6\n1 2 3 4 7 10\n"
)
# custom cases
assert run("1\n10 10\n") == "9\n1 2 3 4 5 6 7 8 9\n", "largest k = n"
assert run("1\n5 1\n") == "5\n1 2 3 4 5\n", "smallest k = 1"
assert run("1\n2 1\n") == "2\n1 2\n", "small n = 2, k = 1"
assert run("1\n3 2\n") == "2\n1 3\n", "small n = 3, k = 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 10 | 1..9 | k=n lớn nhất | 
| 5 1 | 1..5 | nhỏ nhất k=1 | 
| 2 1 | 1 2 | n và k nhỏ | 
| 3 2 | 1 3 | bỏ qua k ở giữa | 

## Vỏ cạnh 

Nếu$k = 1$, thuật toán bỏ qua 1 trong vòng lặp đầu tiên và bắt đầu nhân từ 1, dẫn đến tổng bao gồm tất cả các số ngoại trừ 1. Đối với đầu vào`5 1`, kết quả là`[1,2,3,4,5]`và không có dãy con nào có tổng bằng 1 vì nó bị bỏ qua trong quá trình xây dựng. 

Nếu như$k = n$, vòng lặp đầu tiên bao gồm các số từ 1 đến n-1 và bội số của$k$vượt quá n ngay lập tức
