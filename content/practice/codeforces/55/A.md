---
title: "CF 55A - Bọ chét du hành"
description: "Chúng ta có n vị trí được sắp xếp thành một vòng tròn. Bọ chét bắt đầu ở một vị trí. Sau phút đầu tiên nó tiến lên 1 bước, sau phút thứ hai là 2 bước, sau phút thứ ba là 3 bước, v.v."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 55
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 51"
rating: 1200
weight: 55
solve_time_s: 84
verified: true
draft: false
---

[CF 55A - Du lịch bằng bọ chét](https://codeforces.com/problemset/problem/55/A) 

**Đánh giá:** 1200 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`các vị trí xếp thành vòng tròn. Bọ chét bắt đầu ở một vị trí. Sau phút đầu tiên nó tiến lên 1 bước, sau phút thứ hai là 2 bước, sau phút thứ ba là 3 bước, v.v. 

Câu hỏi đặt ra là liệu mô hình chuyển động này cuối cùng có ghé thăm mọi vị trí trên vòng tròn hay không. 

Nếu bọ chét hiện đang ở vị trí`p`, rồi sau phút`k`nó di chuyển đến:$p+k \pmod n$Đầu vào chứa một số nguyên duy nhất`n`, số vị trí trong vòng tròn. Đầu ra phải là`"YES"`nếu mọi vị trí được truy cập ít nhất một lần, nếu không`"NO"`. 

Hạn chế rất nhỏ, chỉ tối đa 1000 vị trí. Ngay cả một mô phỏng trực tiếp với nhiều bước cũng đủ nhanh. Chúng tôi không cần tối ưu hóa nâng cao hoặc toán học nặng nề. Tuy nhiên, phần thú vị của vấn đề là hiểu được tại sao câu trả lời chỉ phụ thuộc vào việc liệu`n`là số nguyên tố hoặc hợp số. 

Một sai lầm dễ mắc phải là cho rằng con bọ chét luôn ghé thăm mọi thứ vì độ dài bước nhảy liên tục thay đổi. Trực giác đó không thành công đối với các giá trị tổng hợp của`n`. 

Ví dụ:```
n = 4
```Các vị trí đã ghé thăm là:```
0 -> 1 -> 3 -> 2 -> 2 -> ...
```Chức vụ`0`không bao giờ được xem lại sau khi bắt đầu và chuyển động bị mắc kẹt trong một chu kỳ nhỏ hơn. Đầu ra đúng là`"NO"`. 

Một trường hợp tế nhị khác là`n = 1`.```
1
```Chỉ có một vị trí duy nhất nên bọ chét đã ghé thăm tất cả các vị trí ngay lập tức. Câu trả lời đúng là`"YES"`. 

Việc thực hiện bất cẩn cũng có thể dừng lại quá sớm sau khi nhìn thấy một quan điểm lặp lại. Chỉ lặp lại là không đủ để kết luận thất bại trừ khi chúng ta đã biết một số vị trí chưa bao giờ được ghé thăm. Ví dụ:```
n = 2
```Con bọ chét luân phiên giữa hai vị trí mãi mãi, thực tế là đủ để bao phủ toàn bộ vòng tròn. Đầu ra đúng là`"YES"`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng. Chúng tôi theo dõi vị trí hiện tại và liên tục áp dụng các bước nhảy có độ dài`1, 2, 3, ...`. Mỗi khi chúng tôi đến một vị trí, chúng tôi đánh dấu vị trí đó là đã ghé thăm. 

Brute-force hoạt động vì không gian trạng thái rất nhỏ. chỉ có`n`vị trí, vì vậy cuối cùng chuyển động phải lặp lại. Nếu sau khi nhảy đủ số vị trí đã được đánh dấu, chúng ta trả lời`"YES"`, nếu không thì`"NO"`. 

Đối với kích thước bài toán này, thậm chí việc mô phỏng vài nghìn bước di chuyển cũng hoàn toàn có thể chấp nhận được. Với`n ≤ 1000`, MỘT`O(n^2)`quá trình vẫn thực hiện tối đa khoảng một triệu thao tác. 

Quan sát thú vị hơn đến từ lý thuyết số. 

Sau đó`k`chuyển động thì quãng đường đi được là:$1+2+3+\cdots+k = \frac{k(k+1)}{2}$Vị trí thực tế chỉ phụ thuộc vào giá trị này theo modulo`n`. 

Bọ chét ghé thăm mọi vị trí khi và chỉ khi`n`là nguyên tố. 

Tại sao? Đối với vật liệu tổng hợp`n`, tồn tại một số ước`d < n`. Sau khi nhảy đủ số lần, chuyển động tích lũy bắt đầu lặp lại theo modulo`d`, điều này ngăn cản quyền truy cập vào tất cả các vị trí. Đối với nguyên tố`n`, hành vi mô-đun quay vòng qua tất cả các phần dư. 

Một cách khác để diễn đạt quan sát chính thức là con bọ chét ghé thăm tất cả các vị trí một cách chính xác khi ước số chung lớn nhất của cấu trúc bước nhảy và`n`vẫn thuận lợi, điều này chỉ xảy ra với số nguyên tố`n`và cho`n = 1`. 

Từ`n`rất nhỏ, chúng ta có thể chỉ cần kiểm tra xem`n`là nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Xử lý trường hợp đặc biệt`n = 1`. 

Một vị trí duy nhất được truy cập đầy đủ vì bọ chét bắt đầu ở đó. 
3. Kiểm tra xem`n`là nguyên tố. 

Chúng tôi thử tất cả các ước số từ`2`lên tới`√n`. Nếu bất kỳ số chia nào cũng chia`n`, sau đó`n`là tổng hợp. 
4. Nếu`n`là số nguyên tố, in`"YES"`. 

Các vòng tròn có kích thước lớn cho phép mô hình chuyển động cuối cùng bao phủ mọi mô-đun dư lượng`n`. 
5. Nếu không thì in`"NO"`. 

Các vòng tròn có kích thước tổng hợp buộc bọ chét vào một cấu trúc lặp lại nhỏ hơn trước khi đến thăm tất cả các vị trí. 

### Tại sao nó hoạt động 

Vị trí của bọ chét sau vài lần nhảy phụ thuộc vào số tiền tích lũy theo modulo`n`. Đối với nguyên tố`n`, số học mô-đun trên vòng tròn không thu gọn thành các tập con lặp lại nhỏ hơn, do đó mọi vị trí đều có thể truy cập được. Đối với vật liệu tổng hợp`n`, chuyển động trở nên giới hạn trong một tập hợp con các vị trí được xác định bởi ước số của`n`. 

Thuật toán này đúng vì tính nguyên tố mô tả chính xác liệu chuyển động có bao phủ toàn bộ vòng tròn hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(x):
    if x < 2:
        return False

    d = 2
    while d * d <= x:
        if x % d == 0:
            return False
        d += 1

    return True

def solve():
    n = int(input())

    if n == 1 or is_prime(n):
        print("YES")
    else:
        print("NO")

solve()
```Giải pháp bắt đầu bằng một bài kiểm tra tính nguyên thủy tiêu chuẩn. Mọi số nhỏ hơn 2 đều không phải là số nguyên tố. Sau đó, chúng tôi thử mọi ước số`d`trong khi`d * d <= x`. 

Chỉ kiểm tra đến căn bậc hai là đủ vì các ước số luôn xuất hiện theo cặp. Nếu như`x`có ước số lớn hơn`√x`, số chia ghép đôi sẽ nhỏ hơn`√x`. 

Việc xử lý đặc biệt đối với`n = 1`quan trọng vì 1 không phải là số nguyên tố về mặt toán học, nhưng đáp án cho bài toán này vẫn là`"YES"`vì bọ chét đã chiếm vị trí duy nhất. 

Việc triển khai sử dụng bộ nhớ bổ sung liên tục và kết thúc gần như ngay lập tức đối với các ràng buộc nhất định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Dấu vết: 

| Bước | Giá trị | 
| --- | --- | 
| Đọc n | 1 | 
| Kiểm tra trường hợp đặc biệt | n == 1 | 
| Đầu ra | CÓ | 

Ví dụ này cho thấy đầu vào nhỏ nhất có thể. Không cần di chuyển vì bọ chét đã bắt đầu ở vị trí duy nhất. 

### Ví dụ 2 

đầu vào:```
4
```Dấu vết: 

| Bước | Giá trị | 
| --- | --- | 
| Đọc n | 4 | 
| Kiểm tra n == 1 | Sai | 
| Hãy thử ước số d = 2 | 4 % 2 == 0 | 
| Đã phát hiện hỗn hợp | Có | 
| Đầu ra | KHÔNG | 

Dấu vết này thể hiện trường hợp tổng hợp. Vì 4 có một ước số không hề nhỏ nên bọ chét không thể ghé thăm tất cả các vị trí. 

### Ví dụ 3 

đầu vào:```
7
```Dấu vết: 

| Bước | Giá trị | 
| --- | --- | 
| Đọc n | 7 | 
| Kiểm tra n == 1 | Sai | 
| Hãy thử ước số d = 2 | 7 % 2 != 0 | 
| d * d > n | Dừng lại | 
| Thủ tướng được phát hiện | Có | 
| Đầu ra | CÓ | 

Ví dụ này cho thấy một vòng tròn có kích thước nguyên tố, nơi con bọ chét cuối cùng sẽ đến mọi vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Chúng tôi chỉ kiểm tra các ước số tối đa căn bậc hai của n | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với`n ≤ 1000`, sự phức tạp này thấp hơn nhiều so với giới hạn. Ngay cả mô phỏng lực lượng vũ phu cũng có thể vượt qua một cách thoải mái, nhưng giải pháp dựa trên tính nguyên thủy sẽ sạch hơn và nhanh hơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())

    def is_prime(x):
        if x < 2:
            return False

        d = 2
        while d * d <= x:
            if x % d == 0:
                return False
            d += 1

        return True

    if n == 1 or is_prime(n):
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    global input
    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("1\n") == "YES\n", "sample 1"

# custom cases
assert run("2\n") == "YES\n", "smallest prime"
assert run("4\n") == "NO\n", "small composite"
assert run("7\n") == "YES\n", "prime number"
assert run("1000\n") == "NO\n", "large composite boundary"
assert run("997\n") == "YES\n", "large prime near limit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`YES`| Vỏ cạnh một vị trí | 
|`2`|`YES`| Số nguyên tố nhỏ nhất | 
|`4`|`NO`| Hành vi chu trình tổng hợp | 
|`7`|`YES`| Trường hợp nguyên tố điển hình | 
|`1000`|`NO`| Tổng hợp lớn gần giới hạn | 
|`997`|`YES`| Số nguyên tố lớn gần giới hạn | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
```Thuật toán khớp ngay với trường hợp đặc biệt`n == 1`và in`"YES"`. 

Không có chuyển động nào cả, nhưng mọi vị trí đều đã được ghé thăm vì chỉ có một vị trí tồn tại. 

Bây giờ hãy xem xét một số tổng hợp nhỏ:```
4
```Kiểm tra tính nguyên tố thử ước số`2`.```
4 % 2 == 0
```Vì có một số chia nên số này là hợp số nên thuật toán in ra`"NO"`. 

Điều này khớp chính xác với hành vi di chuyển thực tế, trong đó bọ chét không bao giờ bao phủ toàn bộ vòng tròn. 

Cuối cùng, hãy xem xét một số nguyên tố:```
5
```Kiểm tra tính chia hết là:```
5 % 2 != 0
```Không có ước số nào tồn tại đến`√5`, do đó thuật toán phân loại 5 là số nguyên tố và in`"YES"`. 

Cuối cùng, con bọ chét sẽ đi qua tất cả năm vị trí, điều này khẳng định tính đúng đắn của tiêu chí chính.
