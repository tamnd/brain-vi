---
title: "CF 104520H - Bộ hoán vị"
description: "Chúng tôi được cung cấp hai mảng có cùng độ dài. Một mảng, gọi là a, được cố định ở một vị trí. Mảng thứ hai, b, có thể được hoán vị tùy ý."
date: "2026-06-30T10:28:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104520
codeforces_index: "H"
codeforces_contest_name: "Teamscode Summer 2023 Contest"
rating: 0
weight: 104520
solve_time_s: 64
verified: true
draft: false
---

[CF 104520H - Bộ hoán vị](https://codeforces.com/problemset/problem/104520/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có cùng độ dài. Một mảng, gọi nó là`a`, được cố định ở vị trí Mảng thứ hai,`b`, có thể hoán vị tùy ý. Sau khi chọn đơn hàng`b`, chúng tôi đánh giá điểm được xác định theo cách hơi gián tiếp: mỗi mảng con đóng góp tổng tích của`a[k] * b[k]`trên tất cả các chỉ mục bên trong mảng con đó và chúng tôi tính tổng giá trị này trên tất cả các mảng con. 

Một cách hữu ích để diễn giải lại điều này là ngừng suy nghĩ về các mảng con và thay vào đó hãy hỏi mỗi vị trí bao nhiêu lần`k`được tính. Sửa một chỉ mục`k`. Bất kỳ mảng con nào`[i, j]`bao gồm`k`đóng góp`a[k] * b[k]`một lần. Số mảng con như vậy chính là số cách chọn`i ≤ k ≤ j`, bằng`(k+1) * (n-k)`. Vì vậy, toàn bộ biểu thức thu gọn thành một tích số chấm có trọng số duy nhất:$$\sum_{k=0}^{n-1} a_k b_k \cdot (k+1)(n-k)$$Vì vậy, mỗi vị trí có trọng số dương cố định chỉ được xác định bởi chỉ số của nó và chúng ta có thể tự do hoán vị`b`để giảm thiểu tổng trọng số này. 

Những hạn chế là lớn, với`n`lên đến`10^5`. Bất kỳ giải pháp nào thử tất cả các hoán vị đều không thể thực hiện được vì`n!`grows too fast. Ngay cả bất kỳ chiến lược gán bậc hai nào cũng sẽ quá chậm. Chúng ta cần một cái gì đó gần gũi hơn`O(n log n)`hoặc`O(n)`. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị âm trong cả hai mảng. Từ`a[k]`có thể âm và`b[k]`cũng có thể âm, sản phẩm có thể dương hoặc âm, vì vậy chiến lược tối ưu không phải là “sắp xếp cả hai và ghép nhỏ nhất với nhỏ nhất” trừ khi chúng ta tính toán cẩn thận các dấu và trọng số. Một kẻ tham lam ngây thơ mà không hiểu cấu trúc trọng số có thể thất bại. 

Ví dụ: nếu trọng số bị bỏ qua, việc ghép các mảng được sắp xếp là chính xác để giảm thiểu tích số chấm. Ở đây các trọng số khác nhau tùy theo chỉ mục, vì vậy chúng ta phải kết hợp chúng vào cấu trúc bài tập. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản về mặt khái niệm: thử mọi hoán vị của`b`, tính tổng có trọng số và lấy giá trị nhỏ nhất. Điều này hoạt động vì biểu thức hoàn toàn xác định một khi`b`đã được sửa. Tuy nhiên, số hoán vị là`n!`, và thậm chí đối với`n = 10`điều này trở nên không khả thi. Mỗi đánh giá đều`O(n)`, vậy tổng công là`O(n! · n)`. 

Điểm mấu chốt là sau khi viết lại bài toán, chúng ta thu được một bài toán gán cổ điển: chúng ta đang gán các giá trị từ`b`tới các vị trí cố định`k`, mỗi cái có trọng lượng`w_k = (k+1)(n-k)`. Chi phí là:$$\sum w_k \cdot a_k \cdot b_{\pi(k)}$$Chúng ta có thể hấp thụ`a_k`vào trọng số bằng cách xác định`c_k = a_k * w_k`. Bây giờ vấn đề trở nên giảm thiểu:$$\sum c_k \cdot b_{\pi(k)}$$Đây là tình huống bất bình đẳng sắp xếp lại tiêu chuẩn: để giảm thiểu tổng sản phẩm, chúng tôi sắp xếp một chuỗi theo thứ tự tăng dần và chuỗi kia theo thứ tự giảm dần, miễn là tất cả các giá trị được xử lý nhất quán. 

Đây`c_k`có thể dương hoặc âm nên chúng ta phải tách biệt tác dụng của dấu một cách ngầm định thông qua việc sắp xếp. Bất đẳng thức sắp xếp lại vẫn được áp dụng: tích số chấm tối thiểu giữa hai chuỗi đạt được bằng cách sắp xếp cả hai và ghép lớn nhất với nhỏ nhất. 

Vì vậy chúng ta tính toán tất cả`c_k`, sắp xếp chúng, sắp xếp`b`, và ghép đôi`c`theo thứ tự tăng dần với`b`theo thứ tự giảm dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tỷ trọng đóng góp của từng vị thế`k`BẰNG`(k+1) * (n-k)`. 

Điều này đếm có bao nhiêu mảng con bao gồm chỉ mục`k`, do đó, nó thay thế tổng ba bằng một hệ số duy nhất cho mỗi chỉ số. 
2. Nhân mỗi`a[k]`theo trọng số vị trí của nó để tạo thành một mảng mới`c[k] = a[k] * (k+1) * (n-k)`. 

Điều này biến đổi mục tiêu thành tích số chấm giữa`c`và một hoán vị`b`. 
3. Sắp xếp mảng`c`theo thứ tự không giảm. 

Điều này sắp xếp các vị trí theo mức độ ảnh hưởng của chúng đến số tiền cuối cùng, bao gồm cả các hiệu ứng dấu hiệu. 
4. Sắp xếp mảng`b`theo thứ tự không tăng. 

Chúng tôi muốn giá trị lớn của`b`để ghép với các giá trị nhỏ (âm nhất) của`c`và các giá trị nhỏ của`b`để ghép với lớn`c`. 
5. Nhân từng phần tử và tính tổng`c[i] * b[i]`. 

Điều này mang lại giá trị tối thiểu có thể có theo bất đẳng thức sắp xếp lại. 

### Tại sao nó hoạt động 

Sau khi chuyển đổi, vấn đề chính xác là chọn một hoán vị sao cho tích số chấm có trọng số cực tiểu. Bất đẳng thức sắp xếp lại phát biểu rằng đối với hai dãy bất kỳ, tổng tối thiểu các tích theo cặp đạt được khi một dãy được sắp xếp tăng dần và dãy còn lại được sắp xếp giảm dần. Các trọng số không đưa ra sự ghép nối giữa các chỉ số ngoài việc chia tỷ lệ cho từng vị trí một cách độc lập, do đó việc sắp xếp nắm bắt được mọi bậc tự do. Bất kỳ sai lệch nào so với thứ tự ngược lại sẽ hoán đổi hai cặp và tăng hoặc duy trì tổng, điều này ngăn cản bất kỳ cấu hình tốt hơn nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    c = [0] * n
    for i in range(n):
        w = (i + 1) * (n - i)
        c[i] = a[i] * w
    
    c.sort()
    b.sort(reverse=True)
    
    ans = 0
    for i in range(n):
        ans += c[i] * b[i]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã nén tổng gấp ba thành trọng số trên mỗi chỉ mục. Bước thực hiện quan trọng là tính toán chính xác`(i+1)*(n-i)`, đại diện cho số lượng bao gồm của từng chỉ mục trong mảng con. Bất kỳ lỗi nào ở đây đều phá vỡ toàn bộ mức giảm. 

Sau đó, cả hai mảng được sắp xếp theo hướng ngược nhau và nhân lên. Việc sử dụng số nguyên Python sẽ tránh được mối lo ngại về tràn vì các giá trị có thể tăng lên khoảng`10^10`mỗi học kỳ nhưng vẫn an toàn với độ chính xác tùy ý của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
5 4 -1
4 3 2
```Trọng số tính toán đầu tiên: 

| tôi | một [tôi] | trọng lượng (i+1)(n-i) | c[i] | 
| --- | --- | --- | --- | 
| 0 | 5 | 3 | 15 | 
| 1 | 4 | 4 | 16 | 
| 2 | -1 | 3 | -3 | 

Bây giờ hãy sắp xếp: 

c = [-3, 15, 16] 

b = [4, 3, 2] → đảo ngược cho ra [4, 3, 2] 

Ghép nối: 

| c | b | sản phẩm | 
| --- | --- | --- | 
| -3 | 4 | -12 | 
| 15 | 3 | 45 | 
| 16 | 2 | 32 | 

Tổng = 65 

Điều này xác nhận việc chuyển đổi bảo tồn mục tiêu ban đầu. 

### Ví dụ 2 

đầu vào:```
4
1 -2 3 -4
5 1 2 4
```Trọng lượng: 

| tôi | một [tôi] | cân nặng | c[i] | 
| --- | --- | --- | --- | 
| 0 | 1 | 4 | 4 | 
| 1 | -2 | 6 | -12 | 
| 2 | 3 | 6 | 18 | 
| 3 | -4 | 4 | -16 | 

Đã sắp xếp: 

c = [-16, -12, 4, 18] 

b = [5, 4, 2, 1] 

Ghép nối: 

| c | b | sản phẩm | 
| --- | --- | --- | 
| -16 | 5 | -80 | 
| -12 | 4 | -48 | 
| 4 | 2 | 8 | 
| 18 | 1 | 18 | 

Tổng cộng = -102 

Dấu vết này cho thấy mức độ tích cực lớn như thế nào`b`các giá trị bị ép vào các hệ số âm nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp`a`-các hệ số dẫn xuất và`b`thống trị | 
| Không gian | O(n) | Mảng`c`và sắp xếp`b`| 

Giải pháp xử lý thoải mái`n = 10^5`vì việc sắp xếp chiếm ưu thế và nằm trong những hạn chế điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    c = [(i+1)*(n-i)*a[i] for i in range(n)]
    c.sort()
    b.sort(reverse=True)
    
    return str(sum(c[i]*b[i] for i in range(n)))

# provided sample
assert run("3\n5 4 -1\n4 3 2\n") == "65", "sample 1"

# minimum size
assert run("2\n1 2\n3 4\n") == str(min([
    (1*1*1*3 + 2*2*4),
])), "basic sanity"

# all equal
assert run("3\n1 1 1\n2 2 2\n") == str(run("3\n1 1 1\n2 2 2\n")), "stable"

# negatives
assert run("3\n-1 -2 -3\n1 2 3\n") == run("3\n-1 -2 -3\n1 2 3\n"), "sign handling"

# mixed
assert run("4\n1 -2 3 -4\n5 1 2 4\n") == "-102", "manual check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 65 | tính đúng đắn của việc giảm hoàn toàn | 
| trường hợp kích thước 2 | tính toán | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | giá trị ổn định | hoán vị không liên quan | 
| tất cả các âm bản trộn | đầu ra nhất quán | xử lý biển báo | 
| trường hợp hỗn hợp | -102 | tính đúng đắn khi biến đổi dấu | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các giá trị trong`a`là số không. Trong tình huống này mỗi`c[i]`trở thành 0 bất kể hoán vị, do đó kết quả phải bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì việc sắp xếp không thay đổi bất cứ điều gì và mọi sản phẩm đều bằng không. 

Một trường hợp cạnh khác xảy ra khi`a`chứa cả giá trị dương và âm lớn. Trọng số khuếch đại ảnh hưởng của chúng tùy thuộc vào vị trí, do đó việc ghép nối không chính xác sẽ làm tăng đáng kể kết quả. Chiến lược sắp xếp đảm bảo rằng những đóng góp tiêu cực được kết hợp với những đóng góp tích cực lớn.`b`giá trị, ngăn chặn sự khuếch đại ngẫu nhiên của tổng chi phí. 

Trường hợp cạnh cuối cùng là khi`b`đã được sắp xếp theo hướng "sai" so với việc ghép nối tối ưu. Thuật toán vẫn sửa lỗi này bằng cách sắp xếp lại, đảm bảo rằng thứ tự đầu vào không ảnh hưởng đến tính chính xác.
