---
title: "CF 114A - Cifera"
description: "Chúng ta được cho hai số nguyên k và l. Nhiệm vụ là xác định liệu l có thể được viết dưới dạng lũy ​​thừa chính xác của k hay không. Nói cách khác, chúng ta cần kiểm tra xem có tồn tại số nguyên không âm n sao cho: $l = k^n$ Nếu n như vậy tồn tại, chúng ta in \"YES\" và cũng in tầm quan trọng của…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 114
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 86 (Div. 2 Only)"
rating: 1000
weight: 114
solve_time_s: 118
verified: true
draft: false
---

[CF 114A - Cifera](https://codeforces.com/problemset/problem/114/A) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`k`Và`l`. Nhiệm vụ là xác định xem liệu`l`có thể được viết dưới dạng lũy ​​thừa chính xác của`k`. 

Nói cách khác, chúng ta cần kiểm tra xem có tồn tại số nguyên không âm hay không`n`như vậy:$l = k^n$Nếu như vậy`n`tồn tại, chúng tôi in`"YES"`và cũng in ra tầm quan trọng của con số, chính xác là`n - 1`trong thuật ngữ của tuyên bố. Ví dụ,`k^1`không có lần xuất hiện nào của`"la"`,`k^2`có một lần xuất hiện, v.v. 

Các ràng buộc đủ nhỏ để chúng ta không cần toán học nâng cao hoặc logarit. Cả hai số đều nằm trong số nguyên có dấu 32 bit, do đó phép nhân hoặc chia lặp lại là hoàn toàn khả thi. Vì quyền hạn của`k`tăng theo cấp số nhân, số bước trước khi vượt quá`l`nhiều nhất là khoảng 31 khi`k >= 2`. 

Mối nguy hiểm chính là xử lý số mũ một cách chính xác. Việc thực hiện bất cẩn có thể dễ dàng in sai giá trị quan trọng vì câu lệnh đếm số lượng`"la"`các từ chứ không phải bản thân số mũ. 

Hãy xem xét ví dụ này: 

đầu vào:```
5
25
```Đây:$25 = 5^2$Số mũ là`2`, nhưng điều quan trọng là`1`, bởi vì`"petricium la petricium"`chứa một`"la"`. 

Một trường hợp cạnh tinh vi khác xuất hiện khi`l`không chia hết cho`k`trong quá trình này. 

đầu vào:```
3
10
```Một vòng lặp đơn giản giữ lũy thừa nhân có thể vượt quá hoặc sử dụng logarit dấu phẩy động và gặp phải các vấn đề về độ chính xác. Câu trả lời đúng là`"NO"`bởi vì không có lũy thừa nguyên của`3`bằng`10`. 

Một trường hợp quan trọng nữa là khi`l = k`. 

đầu vào:```
7
7
```Điều này tương ứng với:$7 = 7^1$Câu trả lời nên là:```
YES
0
```bởi vì tầm quan trọng được tính`"la"`từ, và không có từ nào trong sức mạnh đầu tiên. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Bắt đầu từ`1`, nhân nhiều lần với`k`và kiểm tra xem liệu cuối cùng chúng ta có đạt được`l`. 

Ví dụ:```
1 -> k -> k^2 -> k^3 -> ...
```Điều này có hiệu quả vì mọi số hợp lệ trong chuỗi đều được tạo theo thứ tự tăng dần. Thời điểm giá trị hiện tại vượt quá`l`, chúng tôi biết`l`là không thể. 

Mặc dù cách tiếp cận này đã đủ nhanh đối với các ràng buộc đã cho, vẫn có một cách thậm chí còn rõ ràng hơn để suy nghĩ về vấn đề. Thay vì xây dựng quyền lực hướng lên trên, chúng ta có thể giảm bớt`l`đi xuống. 

Nếu như`l`thực sự là sức mạnh của`k`, sau đó chia cho`k`liên tục cuối cùng phải đạt được`1`. 

Ví dụ:$125 \to 25 \to 5 \to 1$Mỗi phép chia thành công sẽ loại bỏ một yếu tố của`k`. Nếu vào một thời điểm nào đó`l`không chia hết cho`k`, thì việc biểu diễn là không thể. 

Phương pháp dựa trên phép chia này đơn giản hơn vì nó không bao giờ có nguy cơ tràn và cung cấp cho chúng ta số mũ một cách tự nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(logₖ l) | O(1) | Đã chấp nhận | 
| Tối ưu | O(logₖ l) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`k`Và`l`. 
2. Khởi tạo bộ đếm`cnt = 0`. Bộ đếm này sẽ theo dõi số lần chúng ta chia thành công cho`k`. 
3. Trong khi`l`chia hết cho`k`, chia`l`qua`k`và tăng dần`cnt`. 

Mỗi bộ phận loại bỏ một lũy thừa của`k`từ số. 
4. Sau khi vòng lặp kết thúc, hãy kiểm tra xem`l == 1`. 

Nếu chúng tôi đạt được`1`, thì số ban đầu chính xác là lũy thừa của`k`. 
5. Nếu`l == 1`, in`"YES"`và in`cnt - 1`. 

Bản thân số mũ là`cnt`, nhưng vấn đề yêu cầu số lượng`"la"`từ, tức là ít hơn một từ. 
6. Nếu không, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến rằng sau mỗi lần lặp, giá trị hiện tại của`l`bằng số ban đầu chia cho`k^cnt`. 

Nếu số ban đầu thực sự là:$l = k^n$sau đó chính xác`n`sự phân chia chúng ta phải có được`1`. 

Nếu tại một thời điểm nào đó việc phân chia là không thể bởi vì`l % k != 0`thì ít nhất một thừa số nguyên tố không thuộc`k`, do đó số không thể là lũy thừa thuần túy của`k`. 

Bởi vì vòng lặp loại bỏ chính xác một yếu tố của`k`mỗi lần lặp,`cnt`trở thành số mũ và`cnt - 1`trở thành tầm quan trọng cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    l = int(input())

    cnt = 0

    while l % k == 0:
        l //= k
        cnt += 1

    if l == 1:
        print("YES")
        print(cnt - 1)
    else:
        print("NO")

solve()
```Giải pháp liên tục loại bỏ các yếu tố của`k`từ`l`. 

Điều kiện vòng lặp là chi tiết chính:```
while l % k == 0:
```Chúng ta chỉ chia khi phép chia chính xác. Điều này ngăn chặn các trạng thái không hợp lệ trong đó phép chia số nguyên sẽ âm thầm loại bỏ thông tin. 

Biến`cnt`lưu trữ bao nhiêu lần phân chia thành công. Nếu số ban đầu là`k^n`, thì vòng lặp chạy chính xác`n`lần. 

Phép trừ cuối cùng:```
cnt - 1
```rất dễ bỏ lỡ. Bài toán không yêu cầu số mũ. Nó hỏi số lượng`"la"`các từ nối trong cách biểu diễn cụm từ. 

Không có vấn đề tràn tồn tại vì phép chia chỉ làm giảm giá trị của`l`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
25
```Dấu vết: 

| Hiện tại tôi | l %k | cnt | 
| --- | --- | --- | 
| 25 | 0 | 0 | 
| 5 | 0 | 1 | 
| 1 | dừng lại | 2 | 

Trạng thái cuối cùng:```
l = 1
cnt = 2
```Đầu ra:```
YES
1
```Điều này chứng tỏ rằng thuật toán xác định chính xác:$25 = 5^2$và chuyển đổi số mũ`2`trở nên quan trọng`1`. 

### Ví dụ 2 

đầu vào:```
3
10
```Dấu vết: 

| Hiện tại tôi | l %k | cnt | 
| --- | --- | --- | 
| 10 | 1 | 0 | 

Vòng lặp không bao giờ bắt đầu vì`10`không chia hết cho`3`. 

Trạng thái cuối cùng:```
l = 10
```Đầu ra:```
NO
```Ví dụ này cho thấy tại sao khả năng chia hết là tiêu chí chính xác. Một số không chia hết cho`k`không thể chỉ chứa các yếu tố của`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(logₖ l) | Mỗi lần lặp lại chia`l`qua`k`| 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Từ`l`giảm theo cấp số nhân với mỗi lần chia, vòng lặp thực hiện tối đa khoảng 31 lần đối với số nguyên 32 bit. Điều này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    k = int(input())
    l = int(input())

    cnt = 0

    while l % k == 0:
        l //= k
        cnt += 1

    if l == 1:
        print("YES")
        print(cnt - 1)
    else:
        print("NO")

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("5\n25\n") == "YES\n1\n", "sample 1"

# minimum valid power
assert run("2\n2\n") == "YES\n0\n", "k^1 should have importance 0"

# not a power
assert run("3\n10\n") == "NO\n", "10 is not a power of 3"

# larger exact power
assert run("2\n1024\n") == "YES\n9\n", "1024 = 2^10"

# boundary style case
assert run("2147483647\n2147483647\n") == "YES\n0\n", "largest value equal to k"

# close but invalid
assert run("4\n63\n") == "NO\n", "63 is not a power of 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`|`YES 0`| Trường hợp số mũ nhỏ nhất | 
|`3 10`|`NO`| Số không chia hết | 
|`2 1024`|`YES 9`| Nhiều bộ phận thành công | 
|`2147483647 2147483647`|`YES 0`| Trường hợp giá trị bằng nhau hợp lệ lớn nhất | 
|`4 63`|`NO`| Phòng chống dương tính giả gần công suất | 

## Vỏ cạnh 

Xét trường hợp số mũ chính xác`1`. 

đầu vào:```
7
7
```Thực thi: 

| Hiện tại tôi | Hành động | cnt | 
| --- | --- | --- | 
| 7 | chia cho 7 | 1 | 
| 1 | dừng lại | 1 | 

Thuật toán in:```
YES
0
```Điều này xử lý chính xác sự khác biệt giữa số mũ và tầm quan trọng. 

Bây giờ hãy xem xét một số có một phần ảnh hưởng đến`k`nhưng không hoàn toàn. 

đầu vào:```
6
72
```Thực thi: 

| Hiện tại tôi | Hành động | cnt | 
| --- | --- | --- | 
| 72 | chia cho 6 | 1 | 
| 12 | chia cho 6 | 2 | 
| 2 | dừng lại | 2 | 

Giá trị còn lại là`2`, không`1`, vậy câu trả lời là`"NO"`. 

Điều này xác nhận tính bất biến rằng lũy ​​thừa hợp lệ phải giảm hoàn toàn về`1`. 

Cuối cùng, hãy xem xét một lũy thừa có giá trị rất lớn. 

đầu vào:```
2
1073741824
```Điều này bằng:$1073741824 = 2^{30}$Vòng lặp thực hiện chính xác 30 phép chia và in ra:```
YES
29
```Số lần lặp logarit giữ cho giải pháp cực kỳ nhanh ngay cả khi ở gần ranh giới ràng buộc trên.
