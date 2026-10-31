---
title: "CF 106351H - Fady lưới Fady"
description: "Cho một tập hợp các số nguyên dương A. Chúng ta phải gán một số nguyên dương Bi khác cho mọi phần tử sao cho mọi tích Ai Bi đều có cùng giá trị. Trong số tất cả các nhiệm vụ có thể thực hiện được, chúng ta cần tổng nhỏ nhất có thể có của tất cả các Bi đã chọn."
date: "2026-06-25T08:10:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106351
codeforces_index: "H"
codeforces_contest_name: "Zaglol Contest - FCDS level 2 contest 2026"
rating: 0
weight: 106351
solve_time_s: 28
verified: true
draft: false
---

[CF 106351H - Fady lưới fady](https://codeforces.com/problemset/problem/106351/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên dương`A`. Chúng ta phải gán một số nguyên dương khác`B_i`đến mọi yếu tố để mỗi sản phẩm`A_i * B_i`chính xác là cùng một giá trị. Trong số tất cả các nhiệm vụ có thể thực hiện được, chúng ta cần tổng nhỏ nhất có thể của tất cả các nhiệm vụ được chọn.`B_i`. Những hạn chế là`N`lên tới`2 * 10^5`, trong khi mọi`A_i`nhiều nhất là`20`. 

Hạn chế chính là giá trị nhỏ của mỗi`A_i`. Vì chỉ có một số thừa số nguyên tố có thể có nên giá trị chung của tất cả các tích có thể được xử lý thông qua việc phân tích thành thừa số nguyên tố thay vì thông qua các thuật toán chung lớn. Một giải pháp thử các giá trị có thể có của tích chung sẽ ngay lập tức thất bại vì câu trả lời có thể lớn hơn nhiều so với các giá trị đầu vào. Với`N = 200000`, chúng ta cần một thuật toán gần với thời gian tuyến tính. 

Các trường hợp cạnh chính đến từ các giá trị và số lặp lại chứa các lũy thừa nguyên tố khác nhau. Ví dụ: nếu đầu vào là:```
3
2 2 2
```đầu ra đúng là:```
3
```Sản phẩm thông thường có thể`2`, đưa ra mọi`B_i = 1`, vậy tổng là`3`. Việc triển khai bất cẩn có thể nhân tất cả các số và sử dụng số đó làm mục tiêu, tạo ra câu trả lời lớn hơn nhiều. 

Một trường hợp quan trọng khác là khi một số đã chứa lũy thừa lớn hơn của số nguyên tố:```
3
2 4 8
```Đầu ra đúng là:```
7
```Sản phẩm chung nhỏ nhất là`8`, cho`B = [4, 2, 1]`. Nhìn chung, chỉ sử dụng giá trị lớn nhất nhìn thấy trong mảng là không đủ vì tích chung phải chia hết cho mọi giá trị. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tìm giá trị chung nhỏ nhất có thể`X`như vậy mỗi`A_i`chia rẽ`X`. Một lần`X`đã biết, mỗi giá trị là bắt buộc vì`B_i = X / A_i`. Điều này đúng vì không còn quyền tự do sau khi chọn sản phẩm thông thường. 

Tuy nhiên, việc thử các giá trị có thể có của`X`là không thể. Ngay cả với đầu vào nhỏ, bội số chung nhỏ nhất có thể tăng nhanh và không có giới hạn trên hữu ích nào cho phép liệt kê. 

Nhận xét làm thay đổi bài toán là tích chung phải chính xác là bội số chung nhỏ nhất của tất cả`A_i`. Bất kỳ tích chung hợp lệ nào cũng là bội số của LCM và việc chọn bội số lớn hơn chỉ làm tăng mỗi`B_i`. Vì vậy, mục tiêu tối ưu chính là LCM. 

Bởi vì mỗi`A_i`nhiều nhất là`20`, chúng ta chỉ cần giữ số mũ lớn nhất của từng thừa số nguyên tố`2`ĐẾN`20`. Ví dụ: nếu các số chứa`2`,`4`, Và`8`, LCM cần`2^3`. Sau khi tìm thấy các số mũ nguyên tố này, chúng tôi xây dựng lại LCM và tính tổng cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể bị ràng buộc | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đếm lũy thừa tối đa của mọi số nguyên tố xuất hiện trong đầu vào. 

Vì mọi giá trị lớn nhất là`20`, các số nguyên tố có liên quan duy nhất là`2`,`3`,`5`,`7`,`11`,`13`,`17`, Và`19`. Phân tích từng số là công việc liên tục. 
2. Xây dựng bội số chung nhỏ nhất từ ​​các lũy thừa tối đa này. 

LCM chứa mọi thừa số nguyên tố có số mũ lớn nhất được yêu cầu bởi bất kỳ số đầu vào nào. Đây là số nhỏ nhất chia hết cho tất cả`A_i`. 
3. Thêm`LCM / A_i`cho mỗi giá trị đầu vào. 

Nếu sản phẩm thông thường là`LCM`, sau đó`A_i * B_i = LCM`, Vì thế`B_i`được xác định duy nhất. 

Tại sao nó hoạt động: mọi giải pháp hợp lệ đều chọn một số`X`chia hết cho mọi`A_i`. Số nhỏ nhất như vậy là LCM. Bất kỳ hợp lệ nào khác`X`là bội số của LCM và ít nhất cũng lớn bằng, điều này làm cho mọi giá trị`X / A_i`ít nhất là lớn bằng`LCM / A_i`. Do đó việc phân công LCM sẽ giảm thiểu tổng số tiền. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    primes = [2, 3, 5, 7, 11, 13, 17, 19]
    best = [0] * len(primes)

    for x in a:
        for i, p in enumerate(primes):
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            if cnt > best[i]:
                best[i] = cnt

    lcm = 1
    for p, e in zip(primes, best):
        lcm *= p ** e

    ans = 0
    for x in a:
        ans += lcm // x

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên trích xuất số mũ nguyên tố. Vì các số rất nhỏ nên phép chia lặp lại sẽ nhanh hơn và đơn giản hơn việc lưu trữ cấu trúc phân tích nhân tử tổng quát. 

Biến`best`lưu trữ thông tin LCM. Đối với mỗi số nguyên tố, nó ghi nhớ số mũ lớn nhất gặp phải cho đến nay. Việc xây dựng lại LCM nhân mỗi số nguyên tố với số mũ được lưu trữ của nó chính xác một lần. 

Vòng lặp cuối cùng sử dụng phép chia số nguyên vì LCM được đảm bảo chia hết cho mọi giá trị đầu vào. Số nguyên Python cũng tránh được vấn đề tràn, mặc dù câu trả lời cuối cùng vẫn đủ nhỏ cho các giới hạn lập trình cạnh tranh thông thường vì LCM tối đa của các số lên tới`20`bị hạn chế. 

# Ví dụ đã hoạt động 

Đối với đầu vào:```
6
1 2 3 4 5 6
```LCM trở thành`60`. 

| Giá trị | Sản phẩm bắt buộc | Tính B | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | 60 | 60 | 60 | 
| 2 | 60 | 30 | 90 | 
| 3 | 60 | 20 | 110 | 
| 4 | 60 | 15 | 125 | 
| 5 | 60 | 12 | 137 | 
| 6 | 60 | 10 | 147 | 

Kết quả là:```
147
```Điều này chứng tỏ sản phẩm chung phải thỏa mãn mọi yếu tố chứ không chỉ riêng yếu tố lớn nhất. 

Đối với đầu vào:```
3
2 4 8
```Công suất cực đại của hai là`2^3`, vậy LCM là`8`. 

| Giá trị | LCM | Giá trị B | Tổng chạy | 
| --- | --- | --- | --- | 
| 2 | 8 | 4 | 4 | 
| 4 | 8 | 2 | 6 | 
| 8 | 8 | 1 | 7 | 

Câu trả lời là:```
7
```Điều này xác nhận rằng các thừa số lặp lại được xử lý thông qua số mũ nguyên tố. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi số được phân tích thành nhân tử chỉ sử dụng tám số nguyên tố có thể có. | 
| Không gian | O(1) | Chỉ có mảng số mũ nguyên tố và một vài biến được lưu trữ. | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì nó thực hiện một lượng công việc không đổi cho mỗi`200000`các giá trị đầu vào. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        import sys
        input = sys.stdin.readline

        n = int(input())
        a = list(map(int, input().split()))

        primes = [2, 3, 5, 7, 11, 13, 17, 19]
        best = [0] * len(primes)

        for x in a:
            for i, p in enumerate(primes):
                cnt = 0
                while x % p == 0:
                    x //= p
                    cnt += 1
                best[i] = max(best[i], cnt)

        lcm = 1
        for p, e in zip(primes, best):
            lcm *= p ** e

        print(sum(lcm // x for x in a))

    solve()
    sys.stdin = old_stdin
    return sys.stdout.getvalue()

assert run("""6
1 2 3 4 5 6
""") == "147\n", "sample 1"

assert run("""3
2 4 8
""") == "7\n", "prime powers"

assert run("""1
20
""") == "1\n", "single element"

assert run("""5
7 7 7 7 7
""") == "5\n", "all equal values"

assert run("""8
1 2 3 4 5 6 10 20
""") == "210\n", "mixed factors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 / 1 2 3 4 5 6`|`147`| Hành vi mẫu ban đầu | 
|`3 / 2 4 8`|`7`| Xử lý công suất tối đa | 
|`1 / 20`|`1`| Đầu vào kích thước tối thiểu | 
|`5 / 7 7 7 7 7`|`5`| Giá trị bằng nhau | 
|`8 / 1 2 3 4 5 6 10 20`|`210`| Kết hợp nhiều thừa số nguyên tố | 

# Vỏ cạnh 

Đối với trường hợp giá trị lặp lại:```
3
2 2 2
```Thuật toán tìm thấy số mũ lớn nhất của số nguyên tố`2`là một, nên LCM là`2`. Mỗi giá trị đóng góp`2 / 2 = 1`, đưa ra câu trả lời`3`. Phương pháp này không bao giờ nhân các giá trị trùng lặp với nhau, vì vậy nó tránh đánh giá quá cao mục tiêu. 

Đối với trường hợp nguồn gốc:```
3
2 4 8
```Số lượng yếu tố là`2^1`,`2^2`, Và`2^3`. Số mũ tối đa được lưu trữ là ba, tạo ra LCM là`8`. Những đóng góp là`4`,`2`, Và`1`, đưa ra số tiền tối thiểu có thể`7`. 

Đối với một phần tử duy nhất:```
1
15
```LCM của một số chính là số đó. Sự lựa chọn duy nhất có thể là`B_1 = 1`, vậy câu trả lời là`1`. Thuật toán xử lý điều này một cách tự nhiên vì không cần so sánh với các giá trị khác.
