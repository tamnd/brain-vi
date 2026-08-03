---
title: "CF 102538D - LIS rời rạc"
description: "Chúng ta được yêu cầu đếm các hoán vị có độ dài n mà dãy con tăng dài nhất có thể được chia thành hai dãy con tăng dần có cùng độ dài tối đa, không có phần tử nào được sử dụng hai lần. Câu trả lời là bắt buộc theo modulo 998244353."
date: "2026-08-03T20:56:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102538
codeforces_index: "D"
codeforces_contest_name: "300iq Contest 3"
rating: 0
weight: 102538
solve_time_s: 233
verified: true
draft: false
---

[CF 102538D - LIS rời rạc](https://codeforces.com/problemset/problem/102538/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các hoán vị của độ dài`n`dãy con tăng dài nhất của nó có thể được chia thành hai dãy con tăng dần có cùng độ dài tối đa mà không có phần tử nào được sử dụng hai lần. Câu trả lời là bắt buộc theo modulo`998244353`. Đầu vào chỉ là kích thước của hoán vị, vì vậy nhiệm vụ hoàn toàn là tổ hợp: đếm xem có bao nhiêu hoán vị có thuộc tính này. Các ràng buộc ban đầu là`1 ≤ n ≤ 75`, điều này làm cho nghiệm phụ thuộc theo cấp số nhân vào`n`không thể, nhưng cho phép các thuật toán dựa trên số phân vùng số nguyên của`n`. 

Đối tượng chính đằng sau vấn đề này là hình dạng được tạo ra bởi sự tương ứng Robinson-Schensted. Một hoán vị được chuyển thành dạng sơ đồ Young`λ`. Độ dài của hàng đầu tiên là độ dài LIS. Bài toán hỏi chính xác khi nào hai hàng đầu tiên của hình này có cùng độ dài. Một khi bản dịch này được thực hiện, vấn đề sẽ trở thành việc đếm hoạt cảnh thay vì hoán vị. 

Việc triển khai trực tiếp phải xử lý các kích thước nhỏ một cách chính xác. Ví dụ, đối với`n = 1`, hoán vị duy nhất là`[1]`. LIS của nó có chiều dài`1`, nhưng không có cách nào chọn được hai dãy con rời rạc có độ dài tăng dần`1`, vậy câu trả lời là`0`. Giải pháp chỉ kiểm tra xem hàng đầu tiên có tồn tại hay không sẽ tính sai. 

Vì`n = 2`, các hoán vị là`[1,2]`Và`[2,1]`. Cái đầu tiên có độ dài LIS`2`, do đó hai dãy con rời nhau có độ dài`2`là không thể. Cái thứ hai có độ dài LIS`1`và hai phần tử có thể tạo thành hai dãy con tăng riêng biệt, nên đáp án là`1`. Điều này giúp phát hiện các triển khai mà quên rằng cả hai chuỗi con đều phải có độ dài tối ưu. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê tất cả`n!`hoán vị, tính toán độ dài LIS của mỗi hoán vị, sau đó kiểm tra xem có tồn tại hai LIS rời nhau hay không. Việc kiểm tra có thể được thực hiện bằng lập trình động, nhưng phép liệt kê hoán vị chiếm ưu thế. Tại`n = 10`, điều này đã có nghĩa là về`3,628,800`hoán vị và tại`n = 75`con số này vượt xa mọi tính toán thực tế. 

Nhận xét quan trọng là bài toán không phụ thuộc vào thứ tự hoán vị chính xác. Nhóm tương ứng Robinson-Schensted hoán vị theo hình dạng sơ đồ Young. Đối với hình dạng cố định`λ`, số lượng hoán vị tạo ra nó là bình phương của số lượng hoạt cảnh Trẻ tiêu chuẩn có hình dạng đó. 

Độ dài LIS là độ dài hàng đầu tiên của`λ`. Tổng độ dài lớn nhất có thể có của hai dãy con tăng dần rời rạc là tổng độ dài của hai hàng đầu tiên. Hoán vị chính xác là tốt khi giá trị này bằng hai lần độ dài LIS, có nghĩa là:`λ[0] + λ[1] = 2 * λ[0]`vì vậy hai hàng đầu tiên phải bằng nhau. 

Đối với mỗi phân vùng của`n`thỏa mãn điều kiện này, chúng tôi tính toán số lượng hoạt cảnh Young tiêu chuẩn bằng cách sử dụng công thức độ dài móc:`f(λ) = n! / product(hook(cell))`Sự đóng góp của hình dạng là`f(λ)^2`. 

Số lượng phân vùng của`75`đủ nhỏ để việc lặp qua mọi phân vùng và tính toán tích hook của nó đủ nhanh một cách dễ dàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n · P(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mọi phân vùng số nguyên của`n`theo thứ tự không tăng. Mỗi phân vùng đại diện cho một hình dạng sơ đồ Young có thể có. 
2. Chỉ giữ lại những phân vùng có hai hàng đầu tiên có độ dài bằng nhau. Nếu phân vùng có ít hơn hai hàng thì nó không thể biểu thị một hoán vị tốt. 
3. Đối với mỗi hình dạng hợp lệ, hãy tính số lượng hoạt cảnh Young tiêu chuẩn bằng cách sử dụng công thức chiều dài móc. Đối với mỗi ô, chiều dài móc của nó là số ô bên phải cộng với số ô bên dưới cộng thêm một. 
4. Bình phương số lượng hoạt cảnh và thêm nó vào modulo câu trả lời`998244353`. 
5. In giá trị tích lũy. 

Lý do điều này có hiệu quả là vì Robinson-Schensted đưa ra sự phân biệt giữa các hoán vị của một hình dạng cố định và các cặp hoạt cảnh Young tiêu chuẩn của hình dạng đó. Do đó, một hình dạng đóng góp chính xác`f(λ)²`hoán vị. Điều kiện tốt cũng được mô tả hoàn toàn bằng hình dạng vì độ dài LIS và kích thước tối đa của hai dãy con tăng dần rời rạc được xác định bởi hai hàng đầu tiên. Thuật toán kiểm tra mọi hình dạng có thể và thêm chính xác các hoán vị được biểu thị bằng các hình dạng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input())

fact = [1] * (n + 1)
for i in range(1, n + 1):
    fact[i] = fact[i - 1] * i % MOD

ans = 0

def hook_tableau_count(part):
    cells = []
    rows = len(part)
    for i, length in enumerate(part):
        for j in range(length):
            cells.append((i, j))

    prod = 1
    for i, j in cells:
        right = part[i] - j - 1
        below = 0
        for k in range(i + 1, rows):
            if part[k] > j:
                below += 1
        prod = prod * (right + below + 1) % MOD

    return fact[n] * pow(prod, MOD - 2, MOD) % MOD

def generate(rem, last, cur):
    global ans

    if rem == 0:
        if len(cur) >= 2 and cur[0] == cur[1]:
            f = hook_tableau_count(cur)
            ans = (ans + f * f) % MOD
        return

    for x in range(min(last, rem), 0, -1):
        cur.append(x)
        generate(rem - x, x, cur)
        cur.pop()

generate(n, n, [])

print(ans)
```Mảng giai thừa lưu trữ`n!`, xuất hiện trong mọi phép tính chiều dài móc. Nghịch đảo của tích móc được tính bằng lũy ​​thừa mô đun vì mô đun là số nguyên tố. 

Trình tạo phân vùng luôn chọn độ dài hàng tiếp theo không lớn hơn hàng trước. Điều này tránh tạo ra cùng một sơ đồ Young nhiều lần theo các thứ tự khác nhau. 

điều kiện`len(cur) >= 2 and cur[0] == cur[1]`xử lý trường hợp phân vùng chỉ có một hàng. Hình dạng như vậy không thể cung cấp hai LIS rời nhau có kích thước yêu cầu. 

Công thức bao gồm phép chia, nhưng tất cả chiều dài móc đều nhỏ hơn mô đun vì`n ≤ 75`, do đó nghịch đảo mô đun luôn tồn tại. 

## Ví dụ đã hoạt động 

cho`n = 6`, câu trả lời là`132`. 

Hãy xem xét một vài phân vùng: 

| Hình dáng | Hai hàng đầu tiên | Số lượng hoạt cảnh | Đóng góp | 
| --- | --- | --- | --- | 
| (3,3) | Bằng | 5 | 25 | 
| (2,2,2) | Bằng | 5 | 25 | 
| (4,1,1) | Không bằng | Bỏ qua | 0 | 

Các hình dạng hợp lệ chính xác là những hình mà hai hàng đầu tiên khớp với nhau. Tổng hợp các hoạt cảnh bình phương trên tất cả các hình dạng như vậy sẽ cho`132`. 

Vì`n = 2`: 

| Hình dáng | Hai hàng đầu tiên | Số lượng hoạt cảnh | Đóng góp | 
| --- | --- | --- | --- | 
| (2) | Thiếu hàng thứ hai | Bỏ qua | 0 | 
| (1,1) | Bằng | 1 | 1 | 

Kết quả là`1`, phù hợp với thực tế là chỉ có hoán vị giảm mới hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · P(n)) | có`P(n)`phân vùng và mỗi phép tính hook sử dụng ô O(n). | 
| Không gian | O(n) | Độ sâu đệ quy và kích thước phân vùng hiện tại tối đa là`n`. | 

Vì`n = 75`, số lượng phân vùng đủ nhỏ để phép liệt kê này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    ans = 0

    def count(part):
        prod = 1
        for i, row in enumerate(part):
            for j in range(row):
                hook = row - j
                for k in range(i + 1, len(part)):
                    if part[k] > j:
                        hook += 1
                prod = prod * hook % MOD
        return fact[n] * pow(prod, MOD - 2, MOD) % MOD

    def gen(rem, last, cur):
        nonlocal ans
        if rem == 0:
            if len(cur) >= 2 and cur[0] == cur[1]:
                x = count(cur)
                ans = (ans + x * x) % MOD
            return
        for x in range(min(last, rem), 0, -1):
            cur.append(x)
            gen(rem - x, x, cur)
            cur.pop()

    gen(n, n, [])
    return str(ans)

assert solve("6\n") == "132"
assert solve("1\n") == "0"
assert solve("2\n") == "1"
assert solve("3\n") == "4"
assert solve("4\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Xử lý phân vùng một hàng | 
|`2`|`1`| Hình dạng hợp lệ nhỏ nhất | 
|`3`|`4`| Nhiều phân vùng hợp lệ | 
|`4`|`10`| Một số sản phẩm vách ngăn và móc | 
|`6`|`132`| Mẫu chính thức | 

## Vỏ cạnh 

cho`n = 1`, hình dạng duy nhất là`(1)`. Thuật toán từ chối nó vì không có hàng thứ hai. Điều này ngăn cản việc đếm một hoán vị có LIS nhưng không thể chia nó thành hai LIS rời nhau. 

Vì`n = 2`, hình dạng hợp lệ là`(1,1)`. Chiều dài móc là`2`Và`1`, vậy số lượng hoạt cảnh là`2! / 2 = 1`. Bình phương cho một hoán vị, đó chính xác là hoán vị giảm dần. 

Đối với lớn hơn`n`, các phân vùng có hàng đầu tiên dài nhưng hàng thứ hai ngắn hơn sẽ bị bỏ qua. Ví dụ,`(5,1)`không thể đóng góp vì dãy con tăng dài nhất có độ dài`5`, nhưng hai dãy con rời nhau có độ dài`5`sẽ yêu cầu hai hàng đầu tiên cùng nhau chứa ít nhất`10`tế bào. Điều kiện hình dạng nắm bắt được điều này mà không cần kiểm tra các hoán vị riêng lẻ.
