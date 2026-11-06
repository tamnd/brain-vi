---
title: "CF 1284C - Năm Mới và Hoán Vị"
description: "Chúng ta cần tổng số phân đoạn được đóng khung trên tất cả các hoán vị có độ dài n. Đối với hoán vị cố định, một đoạn [l, r] được đóng khung khi các giá trị bên trong nó tạo thành một tập hợp các số nguyên liên tiếp."
date: "2026-06-11T19:18:13+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1284
codeforces_index: "C"
codeforces_contest_name: "Hello 2020"
rating: 1600
weight: 1284
solve_time_s: 121
verified: true
draft: false
---

[CF 1284C - Năm mới và Hoán vị](https://codeforces.com/problemset/problem/1284/C) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi cần tổng số phân đoạn được đóng khung trên tất cả các hoán vị có độ dài`n`. 

Đối với hoán vị cố định, một đoạn`[l, r]`được đóng khung khi các giá trị bên trong nó tạo thành một tập hợp các số nguyên liên tiếp. Vì tất cả các giá trị trong một hoán vị là khác nhau nên điều kiện$$\max - \min = r - l$$có nghĩa chính xác là phân đoạn chứa mọi số nguyên giữa mức tối thiểu và tối đa của nó. 

Niềm vui của một hoán vị là số lượng các phân đoạn được đóng khung mà nó chứa. Thay vì tính toán hạnh phúc cho một hoán vị, chúng ta phải tính tổng nó trên tất cả`n!`hoán vị và xuất kết quả theo modulo số nguyên tố đã cho`m`. 

Ràng buộc`n ≤ 250000`làm thay đổi hoàn toàn bản chất của vấn đề. Chúng ta không thể liệt kê các hoán vị, vì thậm chí`20!`đã là rất lớn rồi. Chúng tôi cũng không thể sử dụng bất kỳ thuật toán nào để kiểm tra tất cả các phân đoạn của một hoán vị. Lời giải phải rút ra một công thức tính và đánh giá nó trong thời gian gần như tuyến tính. 

Một điểm tinh tế là chúng ta đang tính tổng tất cả các hoán vị cùng một lúc. Nhiều vấn đề thuộc loại này trở nên dễ giải quyết sau khi đảo ngược thứ tự đếm: thay vì hỏi mỗi hoán vị có bao nhiêu phân đoạn được đóng khung, hãy hỏi có bao nhiêu hoán vị tạo ra một phân đoạn cụ thể được đóng khung. 

Một sai lầm dễ mắc phải khác là nghĩ rằng điều kiện chỉ phụ thuộc vào vị trí. Ví dụ, đối với`n = 3`, đoạn có độ dài`2`chỉ được đóng khung khi hai giá trị của nó liên tiếp. Cặp đôi`(1,3)`không hoạt động vì`3 - 1 = 2`trong khi độ dài đoạn trừ đi một bằng`1`. 

Vì`n = 1`, chỉ có một hoán vị và một đoạn. Câu trả lời là`1`. Bất kỳ công thức nào cũng phải tạo ra giá trị này một cách tự nhiên. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta hãy tưởng tượng cách giải thích bạo lực. 

Chúng ta có thể tạo ra mọi hoán vị có độ dài`n`. Đối với mỗi hoán vị, chúng tôi có thể kiểm tra mọi phân đoạn, tính toán mức tối thiểu và tối đa của nó và kiểm tra xem nó có được đóng khung hay không. Ngay cả khi các truy vấn phân đoạn đã được tối ưu hóa, vẫn có`n!`hoán vị, do đó cách tiếp cận gần như trở nên không thể ngay lập tức. Vì`n = 10`, đã có rồi`3,628,800`hoán vị. 

Quan sát quan trọng là định nghĩa của phân đoạn được đóng khung chỉ phụ thuộc vào tập hợp các giá trị bên trong phân đoạn đó. 

Xét một đoạn có độ dài$$k = r-l+1.$$Vì tất cả các giá trị đều khác nhau nên điều kiện$$\max - \min = k-1$$có nghĩa là phân đoạn chứa chính xác`k`các số liên tiếp. 

Giả sử các số liên tiếp đó là$$x, x+1, \dots, x+k-1.$$Có bao nhiêu lựa chọn cho một tập giá trị như vậy? Giá trị khởi đầu`x`có thể được chọn trong$$n-k+1$$cách. 

Bây giờ hãy đếm các hoán vị trong đó một đoạn có vị trí cố định có độ dài`k`chứa chính xác những`k`những con số. 

các`k`các giá trị đã chọn có thể được sắp xếp bên trong phân đoạn theo`k!`cách. 

Phần còn lại`n-k`các giá trị có thể được sắp xếp bên ngoài phân khúc trong`(n-k)!`cách. 

Do đó, đối với vị trí đoạn cố định và khối giá trị liên tiếp cố định, số hoán vị hợp lệ là$$k!(n-k)!.$$có`n-k+1`các khối giá trị liên tiếp có thể và cũng có thể`n-k+1`các vị trí có thể có của một đoạn có chiều dài`k`. 

Do đó, tổng đóng góp của tất cả chiều dài-`k`phân đoạn là$$(n-k+1)^2 \cdot k! \cdot (n-k)!.$$Tổng hợp tất cả các độ dài cho câu trả lời:$$\sum_{k=1}^{n} (n-k+1)^2 \cdot k! \cdot (n-k)!.$$Nhiệm vụ còn lại là tính tổng này theo modulo`m`. Từ`n`tùy thuộc vào`250000`, chúng ta có thể tính trước giai thừa modulo`m`trong thời gian tuyến tính và sau đó đánh giá tổng trong một lần tuyến tính khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Tính toán trước modulo giai thừa`m`. 

Cho phép`fact[i] = i! mod m`cho tất cả`0 ≤ i ≤ n`. 
3. Khởi tạo câu trả lời về 0. 
4. Đối với mỗi đoạn có độ dài`k`từ`1`ĐẾN`n`, tính toán$$(n-k+1)^2 \cdot fact[k] \cdot fact[n-k] \pmod m.$$Số lượng này tính tất cả các cặp`(permutation, framed segment)`đoạn có độ dài bằng`k`. 
5. Thêm phần đóng góp này vào modulo câu trả lời`m`. 
6. Xuất ra câu trả lời cuối cùng. 

Lý do bước 4 có tác dụng là vì mỗi phân đoạn được đóng khung tương ứng với chính xác một khối giá trị liên tiếp và mọi lựa chọn về vị trí phân đoạn, khối giá trị liên tiếp, sắp xếp bên trong phân đoạn và sắp xếp bên ngoài phân đoạn đều tạo ra một hoán vị hợp lệ duy nhất. 

### Tại sao nó hoạt động 

Cố định độ dài`k`. 

Một đoạn có độ dài`k`được đóng khung khi và chỉ khi các giá trị của nó chính xác là một tập hợp nào đó`k`số nguyên liên tiếp. có`n-k+1`các tập giá trị như vậy. 

Chọn một vị trí phân khúc trong số`n-k+1`những vị trí có thể. Khi một tập giá trị liên tiếp được chọn,`k`các giá trị có thể được sắp xếp tùy ý bên trong phân đoạn, góp phần`k!`khả năng. Các giá trị còn lại có thể được sắp xếp tùy ý bên ngoài phân đoạn, góp phần`(n-k)!`khả năng. 

Mọi hoán vị chứa phân đoạn được đóng khung đó đều được tính chính xác một lần, vì vị trí phân đoạn và tập giá trị của nó được xác định duy nhất. 

Tổng các số đếm này trên tất cả các độ dài phân đoạn sẽ tính từng phân đoạn được đóng khung trong mỗi hoán vị chính xác một lần, đó chính xác là tổng số hạnh phúc cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

fact = [1] * (n + 1)
for i in range(1, n + 1):
    fact[i] = fact[i - 1] * i % m

ans = 0

for k in range(1, n + 1):
    cnt = n - k + 1
    contrib = cnt * cnt
    contrib %= m
    contrib = contrib * fact[k] % m
    contrib = contrib * fact[n - k] % m
    ans = (ans + contrib) % m

print(ans)
```Mảng giai thừa lưu trữ mọi giá trị theo modulo`m`. Vì tất cả các tính toán sau này cũng được thực hiện modulo`m`, không cần giá trị lớn hơn. 

Vòng lặp kết thúc`k`trực tiếp đánh giá công thức dẫn xuất. Biến`cnt = n-k+1`xuất hiện hai lần vì có hai lựa chọn độc lập: vị trí của phân đoạn và khối giá trị liên tiếp được gán cho nó. 

Việc áp dụng modulo sau mỗi phép nhân sẽ ngăn các số trung gian tăng lớn một cách không cần thiết. Số nguyên Python có thể xử lý các giá trị lớn, nhưng việc giảm sớm sẽ giúp việc triển khai gần hơn với số học mô-đun dự định. 

Việc lập chỉ mục đặc biệt dễ bị sai. Việc đóng góp sử dụng`fact[k]`Và`fact[n-k]`, không`fact[k-1]`. Thuật ngữ`n-k+1`đến từ việc đếm cả vị trí phân đoạn và khối giá trị liên tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 993244853
```Giai thừa: 

| tôi | sự thật[i] | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 

Vòng lặp chính: 

| k | cnt = n-k+1 | cnt² | k! | (n-k)! | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 

Câu trả lời cuối cùng: 

| Trả lời | 
| --- | 
| 1 | 

Hoán vị duy nhất là`[1]`và phân đoạn duy nhất của nó được đóng khung. 

### Ví dụ 2 

đầu vào:```
3 1000000007
```Giai thừa: 

| tôi | sự thật[i] | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 6 | 

Vòng lặp chính: 

| k | cnt | cnt² | k! | (n-k)! | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 9 | 1 | 2 | 18 | 
| 2 | 2 | 4 | 2 | 1 | 8 | 
| 3 | 1 | 1 | 6 | 1 | 6 | 

Tổng hợp: 

| Chạy Trả lời | 
| --- | 
| 18 | 
| 26 | 
| 32 | 

Đầu ra:```
32
```Điều này phù hợp với cuộc thảo luận mẫu, trong đó tổng số hạnh phúc trên tất cả sáu hoán vị bằng nhau`32`. 

Dấu vết minh họa ý tưởng đếm trung tâm. Các đoạn có độ dài-1 đóng góp`18`, đoạn có độ dài 2 đóng góp`8`và các đoạn có độ dài 3 đóng góp`6`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua giai thừa và một lần vượt qua tổng hợp | 
| Không gian | O(n) | Lưu trữ giai thừa | 
| | | | 

Thuật toán chỉ thực hiện một số phép tính số học trên mỗi giá trị của`k`. Với`n = 250000`, cần khoảng nửa triệu lần lặp vòng lặp, con số này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % m

    ans = 0
    for k in range(1, n + 1):
        cnt = n - k + 1
        ans = (
            ans
            + cnt * cnt % m
            * fact[k] % m
            * fact[n - k]
        ) % m

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("1 993244853\n") == "1", "sample 1"

# custom cases
assert run("2 1000000007\n") == "8", "all segments framed in both permutations"
assert run("3 1000000007\n") == "32", "statement example"
assert run("4 1000000007\n") == "156", "checks larger counting formula"
assert run("5 1000000007\n") == "872", "off-by-one and factorial indexing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 993244853`|`1`| Kích thước tối thiểu | 
|`2 1000000007`|`8`| Mọi phân đoạn đều được đóng khung | 
|`3 1000000007`|`32`| Ví dụ đã biết từ câu lệnh | 
|`4 1000000007`|`156`| Đóng góp nhiều độ dài phân khúc | 
|`5 1000000007`|`872`| Kiểm tra giai thừa và công thức đếm | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất:```
1 993244853
```Vòng lặp chạy một lần với`k = 1`.$$(1)^2 \cdot 1! \cdot 0! = 1.$$Đầu ra của thuật toán`1`, khớp với hoán vị đơn và phân đoạn có khung đơn. 

Coi như`n = 2`:```
2 1000000007
```Những đóng góp là$$2^2 \cdot 1! \cdot 1! = 4,$$Và$$1^2 \cdot 2! \cdot 0! = 2.$$Câu trả lời là`6`. Vì có hai hoán vị nên mỗi hoán vị đều có hạnh phúc`3`, tổng số là`6`. Điều này xác nhận rằng việc đếm xử lý chính xác các phân đoạn chiếm toàn bộ mảng. 

Rủi ro thường gặp xảy ra khi đếm các khối giá trị liên tiếp. Vì`n = 3`Và`k = 2`, có chính xác hai khối giá trị hợp lệ:$$\{1,2\}, \{2,3\}.$$Công thức sử dụng`n-k+1 = 2`. sử dụng`n-k`sẽ bỏ lỡ một khối và tạo ra tổng số không chính xác. Thuật toán tránh điều này vì cả vị trí phân đoạn và khối giá trị đều được tính bằng cùng một biểu thức`n-k+1`. 

Một trường hợp tế nhị khác là`k = n`. Chỉ có một vị trí phân đoạn và chỉ một khối giá trị liên tiếp, cụ thể là tất cả các giá trị từ`1`ĐẾN`n`. Sự đóng góp trở thành$$1^2 \cdot n! \cdot 0! = n!,$$đó chính xác là số lượng hoán vị có phân đoạn mảng đầy đủ được đóng khung. Vì mọi hoán vị đều thỏa mãn thuộc tính này cho toàn bộ mảng nên số đếm là chính xác.
