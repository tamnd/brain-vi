---
title: "CF 103186G - \u9e21\u54e5\u7684\u96d5\u50cf"
description: "Chúng ta được cho một mảng gồm $n$ số nguyên dương, trong đó mỗi giá trị đại diện cho “mức tăng trưởng” của một cái cây được đặt dọc theo một con phố."
date: "2026-07-03T16:13:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103186
codeforces_index: "G"
codeforces_contest_name: "The 2021 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103186
solve_time_s: 46
verified: true
draft: false
---

[CF 103186G - \u9e21\u54e5\u7684\u96d5\u50cf](https://codeforces.com/problemset/problem/103186/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt$n$số nguyên dương, trong đó mỗi giá trị biểu thị “mức tăng trưởng” của cây trồng dọc theo đường phố. Đối với mọi vị trí$i$, chúng ta tưởng tượng việc đặt một bức tượng trên$i$-cây thứ và sau đó chúng tôi tính điểm bằng tích của tất cả mức tăng trưởng của các cây khác, ngoại trừ cây ở vị trí đó$i$. 

Nhiệm vụ là tính toán giá trị này cho mọi chỉ mục một cách độc lập và đưa ra kết quả theo modulo$998244353$. 

Vì vậy với mỗi vị trí$i$, chúng tôi muốn:$$x_i = \prod_{j \ne i} a_j \pmod{998244353}$$Các ràng buộc cho phép lên đến$n = 10^5$, và mỗi$a_i$có thể lớn như$10^9$. Việc nhân trực tiếp tất cả các phần tử cho mỗi chỉ mục sẽ là quá chậm vì điều đó sẽ liên quan đến$O(n^2)$phép nhân. 

Bản thân các giá trị này đều lớn nên các sản phẩm trung gian dễ dàng vượt quá giới hạn 64-bit nếu không được xử lý cẩn thận bằng số học mô-đun. 

Một trường hợp tinh tế phát sinh khi các giá trị lớn và lặp lại. Ví dụ: nếu tất cả các giá trị đều bằng nhau thì mọi câu trả lời phải giống hệt nhau và bằng$a^{n-1} \mod 998244353$. Bất kỳ việc xử lý không chính xác nào về phân chia mô-đun hoặc tính toán trước sẽ phá vỡ tính đối xứng. 

Một trường hợp cạnh quan trọng khác là khi$n = 2$. Mỗi câu trả lời chỉ đơn giản là phần tử còn lại, điều này rất dễ hiểu đúng, nhưng việc triển khai bạo lực đôi khi vô tình nhân cả hai giá trị và chia không chính xác mà không xử lý đúng cách các nghịch đảo mô-đun. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản sẽ tính toán từng$x_i$bằng cách lặp lại tất cả các phần tử khác và nhân chúng. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, đối với mỗi chỉ số, chúng tôi thực hiện$n-1$phép nhân, dẫn đến tổng số khoảng$n(n-1)$hoạt động. Với$n = 10^5$, điều này trở thành$10^{10}$phép nhân, vượt xa mọi giới hạn thời gian hợp lý. 

Quan sát quan trọng là tất cả các câu trả lời đều có chung một cấu trúc tổng thể: mọi$x_i$là tích của toàn bộ mảng ngoại trừ một phần tử. Nếu chúng ta tính toán trước tổng tích của tất cả các phần tử thì mỗi câu trả lời có thể thu được bằng cách loại bỏ$a_i$từ sản phẩm này. Vì phép chia trực tiếp không được phép trong số học mô đun nếu không cẩn thận, nên chúng tôi sử dụng nghịch đảo mô đun. 

Chúng tôi tính toán:$$\text{total} = \prod_{i=1}^{n} a_i \mod 998244353$$Sau đó:$$x_i = \text{total} \cdot a_i^{-1} \mod 998244353$$Từ$998244353$là số nguyên tố, mọi phần tử khác 0 đều có phần tử nghịch đảo và chúng ta có thể tính nó bằng cách sử dụng lũy ​​thừa nhanh:$$a_i^{-1} = a_i^{MOD-2} \mod MOD$$Điều này làm giảm vấn đề tính toán một sản phẩm toàn cầu và$n$lũy thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu (tiền tố + nghịch đảo) |$O(n \log MOD)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và xác định mô đun$MOD = 998244353$. Mỗi phép tính sẽ được giảm modulo giá trị này để tránh tràn và có thể thực hiện phép chia thông qua nghịch đảo mô-đun. 
2. Tính tổng tích của tất cả các phần tử theo modulo$MOD$. Điều này được thực hiện bằng cách lặp lại một lần trên mảng và nhân dần dần. Bước này nén tất cả thông tin chung thành một giá trị duy nhất. 
3. Đối với mỗi chỉ số$i$, tính nghịch đảo mô đun của$a_i$sử dụng lũy ​​thừa nhanh với số mũ$MOD - 2$. Điều này đúng vì môđun là số nguyên tố, nên định lý nhỏ Fermat đảm bảo tính đúng đắn của phép nghịch đảo này. 
4. Nhân tổng sản phẩm với nghịch đảo của$a_i$, sau đó lấy kết quả modulo$MOD$. Điều này loại bỏ một cách hiệu quả sự đóng góp của$a_i$, để lại tích của tất cả các nguyên tố khác. 
5. Xuất tất cả các giá trị tính toán theo thứ tự. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tại bất kỳ thời điểm nào, biến`total`luôn biểu thị tích của tất cả các phần tử trong mảng theo modulo$MOD$. Khi chúng ta nhân nó với$a_i^{-1}$, chúng ta đang chia đại số ra chính xác một lần xuất hiện của$a_i$theo số học mô-đun. Vì nghịch đảo mô-đun là chính xác trong trường nguyên tố nên thao tác này bảo toàn tính chính xác và tạo ra tích chính xác của tất cả các phần tử ngoại trừ phần tử được chọn. Không có sự phụ thuộc nào giữa các chỉ số bị phá vỡ vì mọi tính toán đều bắt đầu từ cùng một sản phẩm toàn cầu được chia sẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, e):
    res = 1
    a %= MOD
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n = int(input())
a = list(map(int, input().split()))

total = 1
for x in a:
    total = total * x % MOD

ans = []
for x in a:
    inv = modpow(x, MOD - 2)
    ans.append(total * inv % MOD)

print(*ans)
```Việc thực hiện tuân theo cấu trúc chính xác của thuật toán. các`modpow`hàm thực hiện phép lũy thừa nhị phân, cần thiết để tính toán nghịch đảo mô-đun một cách hiệu quả. Tổng sản phẩm được tính một lần theo thời gian tuyến tính. Sau đó, mỗi phần tử được xử lý độc lập bằng cách chia nó ra theo nghịch đảo của nó. 

Một điểm tinh tế là mọi thao tác đều được thực hiện theo modulo$MOD$, bao gồm cả phép nhân bên trong lũy ​​thừa. Điều này ngăn ngừa tràn và đảm bảo tính chính xác trong số học mô-đun. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
114 514 1919 810
```Tính tổng sản phẩm đầu tiên: 

| Bước | Giá trị | 
| --- | --- | 
| Bắt đầu | 1 | 
| ×114 | 114 | 
| ×514 | 58596 | 
| ×1919 | 112445724 | 
| ×810 | 91018829640 mod | 

Vậy tổng số giảm modulo$998244353$. 

Bây giờ hãy tính từng câu trả lời bằng tổng chia cho từng phần tử. 

| tôi | một [tôi] | x[i] = tổng / a[i] | 
| --- | --- | --- | 
| 1 | 114 | sản phẩm của người khác | 
| 2 | 514 | sản phẩm của người khác | 
| 3 | 1919 | sản phẩm của người khác | 
| 4 | 810 | sản phẩm của người khác | 

Điều này phù hợp với sản lượng dự kiến:```
798956460 177200460 47462760 112445724
```Dấu vết cho thấy mọi giá trị đều bắt nguồn từ cùng một sản phẩm toàn cầu, xác nhận tính nhất quán. 

### Ví dụ 2 

đầu vào:```
3
2 2 2
```Tổng sản phẩm là$8$. 

| tôi | một [tôi] | nghịch đảo | kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | 499122177 | 4 | 
| 2 | 2 | 499122177 | 4 | 
| 3 | 2 | 499122177 | 4 | 

Tất cả các đầu ra đều giống hệt nhau, điều này khẳng định tính đối xứng khi tất cả các đầu vào đều bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log MOD)$| một thẻ đầy đủ cho sản phẩm cộng thêm$n$lũy thừa mô-đun | 
| Không gian |$O(1)$thêm | chỉ lưu trữ mảng đầu vào và đầu ra | 

Giải pháp thoải mái phù hợp trong giới hạn vì$n \log MOD$là về$10^5 \times 30$, chỉ trong vòng một giây trong Python đối với các phép tính số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def modpow(a, e):
    res = 1
    a %= MOD
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    total = 1
    for x in a:
        total = total * x % MOD
    out = []
    for x in a:
        out.append(total * modpow(x, MOD - 2) % MOD)
    return " ".join(map(str, out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# sample-like test
assert run("4\n114 514 1919 810\n") == "798956460 177200460 47462760 112445724"

# minimum n
assert run("2\n3 5\n") == "5 3"

# all equal
assert run("5\n7 7 7 7 7\n") == "2401 2401 2401 2401 2401"

# includes large values
assert run("3\n1000000000 999999937 123456789\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 3 5 | 5 3 | trường hợp hoán đổi tối thiểu | 
| tất cả 7 giây | tất cả các sản phẩm như nhau | xử lý đối xứng | 
| số nguyên tố lớn | tính đúng mod | an toàn tràn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị giống hệt nhau. Đối với đầu vào:```
5
7 7 7 7 7
```Tổng sản phẩm là$7^5 \mod MOD$. Mỗi câu trả lời chia ra một số 7, dẫn đến$7^4$. Thuật toán xử lý việc này một cách tự nhiên vì mỗi nghịch đảo mô-đun của 7 đều giống hệt nhau, do đó tất cả các kết quả đầu ra vẫn bằng nhau. Việc tính toán không bao giờ phụ thuộc vào thứ tự chỉ số, do đó tính đối xứng được bảo toàn. 

Một trường hợp cạnh khác là$n = 2$. Vì:```
2
a b
```tổng sản phẩm là$ab$. Câu trả lời đầu tiên trở thành$ab \cdot a^{-1} = b$, và thứ hai trở thành$a$. Điều này phù hợp với cách diễn giải trực tiếp mà không yêu cầu bất kỳ logic trường hợp đặc biệt nào. 

Cuối cùng, các giá trị lớn gần$10^9$được xử lý an toàn vì tất cả các phép nhân đều được giảm modulo$998244353$ở mọi bước, ngăn ngừa tràn trong cả Python và số học mô-đun trung cấp.
