---
title: "CF 476B - Dreamoon và WiFi"
description: "Drazil gửi một chuỗi lệnh di chuyển. Mỗi dấu '+' di chuyển Dreamoon sang phải một bước và mỗi dấu '-' di chuyển sang trái một bước. Chúng ta biết chuỗi lệnh gốc và cả chuỗi mà Dreamoon nhận được. Chuỗi nhận được có thể chứa '?"
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 476
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 272 (Div. 2)"
rating: 1300
weight: 476
solve_time_s: 95
verified: true
draft: false
---

[CF 476B - Dreamoon và WiFi](https://codeforces.com/problemset/problem/476/B) 

**Đánh giá:** 1300 
**Tags:** bitmasks, lực lượng vũ phu, tổ hợp, dp, toán học, xác suất 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Drazil gửi một chuỗi lệnh di chuyển. Mỗi`'+'`di chuyển Dreamoon một bước sang phải và mỗi bước`'-'`dịch sang trái một bước. 

Chúng ta biết chuỗi lệnh gốc và cả chuỗi mà Dreamoon nhận được. Chuỗi nhận được có thể chứa`'?'`các ký tự, đại diện cho các lệnh không thể được nhận dạng. Đối với mọi`'?'`, Dreamoon độc lập lựa chọn`'+'`hoặc`'-'`với xác suất bằng nhau. 

Nhiệm vụ là tính xác suất để Dreamoon về đích ở đúng vị trí mà anh ấy đã đạt được nếu làm theo chuỗi lệnh ban đầu. 

Các chuỗi có độ dài tối đa là 10. Ràng buộc đó thay đổi hoàn toàn bản chất của vấn đề. Ngay cả khi mọi ký tự trong chuỗi nhận được đều`'?'`, chỉ có$2^{10} = 1024$những cách giải thích có thể. Việc liệt kê mọi khả năng là chuyện nhỏ trong thời gian giới hạn. 

Thách thức chính không phải là hiệu quả mà là tính xác suất một cách chính xác. Mỗi nhiệm vụ của`'?'`các ký tự có khả năng xảy ra như nhau vì mỗi lệnh chưa biết được xác định bằng cách tung đồng xu công bằng độc lập. 

Một lỗi phổ biến là so sánh các chuỗi theo từng ký tự thay vì so sánh vị trí cuối cùng. Ví dụ:```
s1 = "++--"
s2 = "+?+-"
```Các chuỗi lệnh khác nhau vẫn có thể tạo ra cùng một vị trí cuối cùng. Điểm đến quan trọng chứ không phải trình tự di chuyển chính xác. 

Một sai lầm dễ mắc phải khác là quên rằng mọi nhiệm vụ đều có khả năng xảy ra như nhau. Coi như:```
s1 = "+"
s2 = "?"
```Có hai kết quả có thể xảy ra,`'+'`Và`'-'`. Chỉ có một người đến được vị trí mục tiêu nên câu trả lời là`0.5`, không`1`. 

Trường hợp cạnh thứ ba xảy ra khi không thể tiếp cận được vị trí mục tiêu bất kể`'?'`ký tự được chọn:```
s1 = "+++"
s2 = "?--"
```Vị trí mục tiêu là`+3`. Chuỗi nhận được chỉ có thể kết thúc ở vị trí`-1`hoặc`-3`. Xác suất đúng là`0`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. 

Đầu tiên hãy tính toán điểm đến mà Drazil dự định. Sau đó xem xét mọi khả năng thay thế của`'?'`ký tự trong chuỗi nhận được. Đối với mỗi nhiệm vụ, hãy mô phỏng chuỗi chuyển động thu được và tính toán vị trí cuối cùng của nó. Đếm xem có bao nhiêu nhiệm vụ đạt được vị trí mục tiêu. 

Nếu có`k`lệnh không xác định, có chính xác`2^k`bài tập. Vì độ dài chuỗi không bao giờ vượt quá 10 nên trường hợp xấu nhất chỉ là 1024 phép gán. Ngay cả một tìm kiếm đệ quy đơn giản cũng cực kỳ nhanh chóng. 

Lý do mà cách tiếp cận bạo lực này có tác dụng là vì mỗi phép gán tương ứng với một kết quả có khả năng xảy ra như nhau khi tung đồng xu. Nếu như`good`nhiệm vụ đạt được mục tiêu và có`2^k`tổng số bài tập, xác suất mong muốn là:$$\frac{good}{2^k}$$Ngoài ra còn có một quan sát tổ hợp. Thay vì liệt kê các bài tập, người ta có thể tính toán có bao nhiêu`'+'`cần có sự lựa chọn giữa các vị trí chưa biết và sử dụng hệ số nhị thức. Điều đó hiệu quả vì chỉ có sự dịch chuyển cuối cùng mới quan trọng. 

Đối với vấn đề này, ràng buộc nhỏ làm cho việc liệt kê đơn giản hơn và dễ thực hiện chính xác hơn. Nó đã đủ tối ưu rồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(2^k \cdot n) | O(k) | Đã chấp nhận | 
| Đếm tổ hợp | O(n) | O(1) | Đã chấp nhận | 

Đây`k`là số lượng`'?'`nhân vật và`n ≤ 10`. 

## Hướng dẫn thuật toán 

1. Tính vị trí đích do chuỗi gốc tạo ra. 

Đối xử`'+'`BẰNG`+1`Và`'-'`BẰNG`-1`, sau đó tổng hợp tất cả các chuyển động. 
2. Đếm xem có bao nhiêu`'?'`các ký tự xuất hiện trong chuỗi nhận được. 

Những vị trí này là những phần không chắc chắn duy nhất của quá trình. 
3. Sử dụng tìm kiếm theo chiều sâu để tạo ra mọi khả năng thay thế các lệnh chưa biết. 

Tại mỗi`'?'`, phân nhánh thành hai khả năng:`'+'`Và`'-'`. 
4. Khi tất cả các vị trí đã được xử lý, hãy tính vị trí cuối cùng thu được. 

Nếu nó bằng vị trí mục tiêu, hãy tăng số lượng nhiệm vụ thành công. 
5. Theo dõi tổng số bài tập đã khám phá. 

Mỗi lá của đệ quy tương ứng với một kết quả có khả năng xảy ra như nhau. 
6. Đầu ra:$$\frac{\text{successful assignments}}{\text{total assignments}}$$### Tại sao nó hoạt động 

Mỗi`'?'`đại diện cho việc tung đồng xu công bằng độc lập. Việc thay thế tất cả các vị trí chưa biết sẽ tạo ra chính xác không gian mẫu của các kết quả có thể xảy ra. Đệ quy truy cập mọi kết quả một lần và chỉ một lần. 

Đối với mọi kết quả, chúng tôi kiểm tra xem vị trí cuối cùng của nó có bằng vị trí mục tiêu hay không. Số kết quả thành công chia cho tổng số kết quả chính xác là định nghĩa về xác suất khi tất cả các kết quả đều có khả năng xảy ra như nhau. Vì mỗi lần gán các lệnh chưa biết đều có xác suất$(1/2)^k$, đếm các bài tập thuận lợi là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s1 = input().strip()
s2 = input().strip()

target = 0
for ch in s1:
    target += 1 if ch == '+' else -1

good = 0
total = 0

def dfs(idx, pos):
    global good, total

    if idx == len(s2):
        global target
        total += 1
        if pos == target:
            good += 1
        return

    if s2[idx] == '+':
        dfs(idx + 1, pos + 1)
    elif s2[idx] == '-':
        dfs(idx + 1, pos - 1)
    else:
        dfs(idx + 1, pos + 1)
        dfs(idx + 1, pos - 1)

dfs(0, 0)

print("{:.12f}".format(good / total))
```Phần đầu tiên tính toán đích đến mà Drazil dự định. 

Hàm đệ quy xử lý chuỗi nhận được từ trái sang phải. tham số`pos`lưu trữ vị trí đạt được sau khi xử lý tiền tố kết thúc tại chỉ mục`idx - 1`. 

Đối với các lệnh đã biết chỉ có một phần tiếp theo. Vì`'?'`có hai lần tiếp tục, tương ứng với hai kết quả có khả năng xảy ra như nhau khi tung đồng xu. 

Khi đệ quy đến cuối chuỗi, một phép gán hoàn chỉnh đã được xây dựng. Chúng tôi tăng tổng số và kiểm tra xem vị trí kết quả có khớp với mục tiêu hay không. 

Câu trả lời là tỷ lệ các bài tập thành công trên tất cả các bài tập. Vì số lượng bài tập nhiều nhất là 1024 nên đệ quy hoàn toàn an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
++-+-
+-+-+
```Vị trí mục tiêu: 

| Nhân vật | Vị trí chạy | 
| --- | --- | 
| + | 1 | 
| + | 2 | 
| - | 1 | 
| + | 2 | 
| - | 1 | 

Mục tiêu = 1 

Chuỗi nhận được: 

| Nhân vật | Vị trí chạy | 
| --- | --- | 
| + | 1 | 
| - | 0 | 
| + | 1 | 
| - | 0 | 
| + | 1 | 

Vị trí cuối cùng = 1. 

Không có lệnh nào chưa biết nên chỉ có một kết quả có thể xảy ra. Nó đạt được mục tiêu, mang lại xác suất`1/1 = 1`. 

### Mẫu 2 

đầu vào:```
+-+-
+-??
```Vị trí mục tiêu: 

| Nhân vật | Vị trí chạy | 
| --- | --- | 
| + | 1 | 
| - | 0 | 
| + | 1 | 
| - | 0 | 

Mục tiêu = 0 

Nhiệm vụ có thể có của hai lệnh chưa biết: 

| Bài tập | Vị trí cuối cùng | Thành công | 
| --- | --- | --- | 
| +-++ | 2 | Không | 
| +-+- | 0 | Có | 
| +--+ | 0 | Có | 
| +--- | -2 | Không | 

Chúng ta có 2 nhiệm vụ thành công trên tổng số 4 nhiệm vụ.$$\frac{2}{4}=0.5$$Ví dụ này cho thấy tại sao việc đếm các vị trí cuối cùng lại là điều quan trọng. Hai nhiệm vụ khác nhau đạt đến cùng một đích và cả hai đều góp phần tạo ra xác suất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^k · n) | Liệt kê từng nhiệm vụ của`k`các lệnh không xác định và xử lý một chuỗi có độ dài`n`| 
| Không gian | O(k) | Độ sâu đệ quy bằng số ký tự được xử lý | 

Từ`n ≤ 10`, số lượng bài tập tối đa chỉ là 1024. Thuật toán chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s1 = input().strip()
    s2 = input().strip()

    target = sum(1 if c == '+' else -1 for c in s1)

    good = 0
    total = 0

    def dfs(idx, pos):
        nonlocal good, total

        if idx == len(s2):
            total += 1
            if pos == target:
                good += 1
            return

        if s2[idx] == '+':
            dfs(idx + 1, pos + 1)
        elif s2[idx] == '-':
            dfs(idx + 1, pos - 1)
        else:
            dfs(idx + 1, pos + 1)
            dfs(idx + 1, pos - 1)

    dfs(0, 0)
    return "{:.12f}".format(good / total)

# sample 1
assert run("++-+-\n+-+-+\n") == "1.000000000000"

# sample 2 from statement explanation
assert run("+-+-\n+-??\n") == "0.500000000000"

# minimum length
assert run("+\n+\n") == "1.000000000000"

# one unknown command
assert run("+\n?\n") == "0.500000000000"

# impossible target
assert run("+++\n?--\n") == "0.000000000000"

# all unknowns, target position 0
assert run("+-\n??\n") == "0.500000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`+\n+\n`|`1.000000000000`| Ví dụ nhỏ nhất có thể | 
|`+\n?\n`|`0.500000000000`| Tung đồng xu đơn | 
|`+++\n?--\n`|`0.000000000000`| Vị trí mục tiêu không thể tiếp cận | 
|`+-\n??\n`|`0.500000000000`| Nhiều nhiệm vụ đạt mục tiêu | 

## Vỏ cạnh 

Hãy xem xét:```
+
?
```Vị trí mục tiêu là`+1`. Lệnh không xác định có thể trở thành`'+'`hoặc`'-'`. Đệ quy tạo ra chính xác hai phép gán. Một người đạt đến vị trí`+1`, cái còn lại đạt tới`-1`. Thuật toán trả về`1/2 = 0.5`. 

Coi như:```
+++
?--
```Vị trí mục tiêu là`+3`. Các vị trí cuối cùng duy nhất có thể có từ chuỗi nhận được là`-1`Và`-3`. Trong quá trình liệt kê, không có nhiệm vụ nào phù hợp với mục tiêu. Số lượng thành công vẫn bằng 0, vì vậy câu trả lời là`0.0`. 

Coi như:```
+-
??
```Vị trí mục tiêu là`0`. Đệ quy khám phá bốn bài tập. Hai đầu ở vị trí`0`, một lúc`+2`, và một tại`-2`. Thuật toán đếm hai phép gán và kết quả đầu ra thành công`2/4 = 0.5`. 

Những ví dụ này bao gồm các tình huống phức tạp trong đó xác suất là phân số, trong đó mục tiêu là không thể và trong đó nhiều nhiệm vụ riêng biệt dẫn đến cùng một đích. Phương pháp đếm xử lý tất cả chúng một cách tự nhiên.
