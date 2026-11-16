---
title: "CF 1878C - Vasilije ở Cacak"
description: "Bài toán hỏi liệu chúng ta có thể chọn chính xác k số nguyên riêng biệt từ tập hợp {1, 2, ..., n} sao cho tổng của chúng bằng x hay không. Mỗi trường hợp thử nghiệm cung cấp ba số nguyên: n, giới hạn trên của các số chúng ta có thể chọn; k, số phần tử phải chọn; và x, tổng mục tiêu."
date: "2026-06-08T22:50:18+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1878
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 900 (Div. 3)"
rating: 900
weight: 1878
solve_time_s: 87
verified: true
draft: false
---

[CF 1878C - Vasilije ở Cacak](https://codeforces.com/problemset/problem/1878/C) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề hỏi liệu chúng ta có thể chọn chính xác`k`các số nguyên khác nhau từ tập hợp`{1, 2, ..., n}`sao cho tổng của chúng bằng`x`. Mỗi trường hợp thử nghiệm cung cấp ba số nguyên:`n`, giới hạn trên của các số chúng ta có thể chọn;`k`, số phần tử chúng ta phải chọn; Và`x`, tổng mục tiêu. Đầu ra phải là "CÓ" nếu có thể lựa chọn như vậy và "KHÔNG" nếu ngược lại. 

Những ràng buộc chỉ ra rằng`n`có thể lớn tới 200.000 và`x`có thể lên tới 40 tỷ, với tối đa 10.000 ca kiểm thử. Một nỗ lực mạnh mẽ để liệt kê tất cả các kết hợp sẽ không khả thi vì số lượng tập hợp con phần tử k tăng lên theo kiểu tổ hợp với`n`Và`k`. Do đó, chúng tôi cần một giải pháp hoạt động theo thời gian không đổi hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp khó khăn dễ bị bỏ qua bao gồm các tình huống trong đó tổng nhỏ hơn tổng tối thiểu có thể có của`k`phần tử hoặc lớn hơn tổng tối đa có thể. Ví dụ, nếu`n = 5`,`k = 3`, Và`x = 3`, tổng nhỏ nhất của ba số phân biệt là`1 + 2 + 3 = 6`, vì vậy câu trả lời phải là "KHÔNG". Tương tự, nếu`x`vượt quá tổng của`k`số lớn nhất trong`{1..n}`, điều đó là không thể. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ là tạo ra tất cả các kết hợp của`k`số giữa`1`Và`n`và kiểm tra xem có bất kỳ cái nào trong số chúng có tổng bằng không`x`. Điều này đúng nhưng cực kỳ chậm: số lượng kết hợp là`C(n, k)`, cho giá trị lớn nhất của`n`Và`k`có thể vượt quá 10^150, vượt xa khả năng tính toán. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng tổng tối thiểu của`k`các số phân biệt luôn là tổng của số đầu tiên`k`số tự nhiên,`1 + 2 + ... + k = k*(k+1)/2`. Tương tự, tổng tối đa là tổng của số lớn nhất`k`số,`n + (n-1) + ... + (n-k+1) = k*(2n - k + 1)/2`. Nếu như`x`nhỏ hơn số tiền tối thiểu hoặc lớn hơn số tiền tối đa thì câu trả lời ngay lập tức là “KHÔNG”. Ngược lại có thể chọn số ở giữa để lấy chính xác`x`. Điều này mang lại khả năng kiểm tra liên tục cho từng trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n,k)) | O(k) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`k`, Và`x`. 
2. Tính tổng tối thiểu có thể có của`k`số phân biệt như`min_sum = k*(k+1)//2`. Tổng này sử dụng số đầu tiên`k`số nguyên dương. 
3. Tính số tiền tối đa có thể có như sau`max_sum = k*(2*n - k + 1)//2`. Tổng này sử dụng số tiền lớn nhất`k`số nguyên từ`1`ĐẾN`n`. 
4. So sánh`x`với`min_sum`Và`max_sum`. Nếu như`x`nằm ngoài khoảng`[min_sum, max_sum]`, xuất ra "KHÔNG". 
5. Ngược lại, xuất ra "CÓ". 

Tại sao nó hoạt động: Bất kỳ số tiền nào bên ngoài`[min_sum, max_sum]`không thể thu được vì các phần tử khác nhau và bị giới hạn bởi 1 và`n`. Bất kỳ số tiền nào trong khoảng đều có thể đạt được bằng cách bắt đầu với số đầu tiên`k`số lượng và tăng dần các yếu tố để tiếp cận`x`không vượt quá`n`. Thuật toán tận dụng thuộc tính này mà không cần xây dựng trình tự một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, k, x = map(int, input().split())
        min_sum = k * (k + 1) // 2
        max_sum = k * (2 * n - k + 1) // 2
        if x < min_sum or x > max_sum:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    main()
```Mã đọc số lượng trường hợp kiểm thử và sau đó xử lý từng trường hợp bằng cách tính tổng tối thiểu và tối đa cho`k`các phần tử. Việc kiểm tra so sánh được thực hiện bằng số nguyên, tránh tràn bằng cách sử dụng số học có độ chính xác tùy ý của Python. Điều này tránh được bất kỳ cạm bẫy nào với số lượng lớn`x`giá trị lên tới 4*10^10. Trường hợp cạnh với`k = 1`hoặc`k = n`được xử lý tự động vì các công thức vẫn tạo ra số tiền tối thiểu và tối đa chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`5 3 10`| Bước | tổng tối thiểu | max_sum | x | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Tính toán | 6 | 12 | 10 | CÓ | 

Giải thích: Tổng 10 nằm trong khoảng từ 6 đến 12 nên điều đó là có thể. 

Đầu vào mẫu 2:`5 3 3`| Bước | tổng tối thiểu | max_sum | x | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Tính toán | 6 | 12 | 3 | KHÔNG | 

Giải thích: 3 nằm dưới tổng nhỏ nhất của 3 phần tử nên không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính và so sánh số học được thực hiện cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài giá trị đầu vào | 

Với tối đa 10^4 trường hợp thử nghiệm, tổng độ phức tạp về thời gian là O(t), vừa vặn trong 1 giây ngay cả với Python. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# provided samples
assert run("""12
5 3 10
5 3 3
10 10 55
6 5 20
2 1 26
187856 87856 2609202300
200000 190000 19000000000
28 5 2004
2 2 2006
9 6 40
47202 32455 613407217
185977 145541 15770805980
""") == """YES
NO
YES
YES
NO
NO
YES
NO
NO
NO
YES
YES""", "sample 1"

# custom cases
assert run("1\n1 1 1\n") == "YES", "single element"
assert run("1\n1 1 2\n") == "NO", "single element too large"
assert run("1\n10 10 55\n") == "YES", "full sum of all elements"
assert run("1\n10 10 56\n") == "NO", "full sum exceeded"
assert run("1\n5 2 5\n") == "YES", "middle sum achievable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | CÓ | Đầu vào kích thước tối thiểu | 
| 1 1 2 | KHÔNG | Vượt quá phần tử đơn`n`| 
| 10 10 55 | CÓ | Tổng lựa chọn kích thước tối đa | 
| 10 10 56 | KHÔNG | Lựa chọn kích thước tối đa vượt quá tổng | 
| 5 2 5 | CÓ | Số tiền có thể đạt được trong phạm vi | 

## Vỏ cạnh 

cho`n = 1, k = 1, x = 1`,`min_sum`Và`max_sum`cả hai đều bằng 1, do đó thuật toán đưa ra "CÓ" chính xác. Vì`n = 1, k = 1, x = 2`,`x`vượt quá`max_sum = 1`, vì vậy nó xuất ra "KHÔNG". Vì`k = n`, tổng sẽ tự động kiểm tra xem`x`bằng tổng của tất cả các số. Lớn`n`Và`k`với kích thước lớn`x`được xử lý chính xác do số nguyên Python. Điều này đảm bảo tất cả các trường hợp cạnh được xử lý đồng nhất.
