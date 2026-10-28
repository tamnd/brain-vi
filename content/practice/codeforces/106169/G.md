---
title: "CF 106169G - Lời Bí Mật"
description: "Chúng ta được cung cấp một từ điển gồm các từ bí mật và một chuỗi văn bản. Văn bản chứa các chữ cái viết thường và ký tự đặc biệt ?. MỘT ? có thể đại diện cho bất kỳ chữ cái viết thường nào. Nhiệm vụ là đếm xem có bao nhiêu cách khác nhau để chia toàn bộ văn bản thành các từ trong từ điển."
date: "2026-06-25T11:09:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106169
codeforces_index: "G"
codeforces_contest_name: "2025-2026 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 106169
solve_time_s: 45
verified: true
draft: false
---

[CF 106169G - Từ bí mật](https://codeforces.com/problemset/problem/106169/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một từ điển gồm các từ bí mật và một chuỗi văn bản. Văn bản chứa chữ thường và ký tự đặc biệt`?`. 

MỘT`?`có thể đại diện cho bất kỳ chữ cái viết thường nào. Nhiệm vụ là đếm xem có bao nhiêu cách khác nhau để chia toàn bộ văn bản thành các từ trong từ điển. 

Một từ trong từ điển khớp với một đoạn văn bản nếu chúng có cùng độ dài và mọi vị trí đều thỏa mãn một trong các điều kiện sau: 

- ký tự văn bản bằng ký tự từ; 
- ký tự văn bản là`?`, nghĩa là nó có thể đại diện cho ký tự từ đó. 

Mỗi từ trong từ điển có thể được sử dụng nhiều lần. Mỗi ký tự của văn bản phải thuộc về chính xác một từ đã chọn. Chúng ta cần số lượng phân tách hợp lệ modulo`10^9 + 7`. 

Những hạn chế tuy nhỏ nhưng rất gợi ý. Có tối đa 100 từ, mỗi từ có độ dài tối đa là 10 và độ dài văn bản tối đa là 100. Một giải pháp xung quanh`O(|T| * n * maxLen)`là hoàn toàn an toàn. Thậm chí`100 * 100 * 10 = 100000`so sánh nhân vật là rất nhỏ. 

Sự tinh tế chính là một`?`không chỉ tạo ra một từ phù hợp. Nó có thể khớp với một số từ trong từ điển khác nhau và mỗi lựa chọn phù hợp sẽ đóng góp riêng cho câu trả lời. 

Hãy xem xét mẫu đầu tiên:```
Words: a, b, c, d, ab, bc, cd
Text: ab?d
```Nhân vật thứ ba có thể hành xử như`a`,`b`,`c`, hoặc`d`, tạo ra một số phân hủy riêng biệt. Câu trả lời là 11 chứ không phải 4. 

Một sai lầm dễ mắc phải khác là quên rằng các từ khác nhau có cùng độ dài có thể khớp với cùng một chuỗi con. 

Ví dụ:```
Words:
ab
ac

Text:
a?
```Cả hai từ đều phù hợp. Câu trả lời là:```
2
```Một DP boolean chỉ ghi lại liệu một vị trí có thể truy cập được hay không sẽ trả về 1 không chính xác. 

Trường hợp cạnh thứ ba là khi toàn bộ văn bản bao gồm các dấu chấm hỏi. 

Ví dụ:```
Words:
a
b

Text:
??
```Vị trí thứ nhất có 2 lựa chọn và vị trí thứ hai có 2 lựa chọn nên đáp án là:```
4
```Đây là một trong những mẫu chính thức. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là quay lui đệ quy. Bắt đầu từ đầu văn bản, thử từng từ trong từ điển khớp với vị trí hiện tại và xử lý đệ quy phần còn lại của chuỗi. Mỗi lần hoàn thành thành công đều đóng góp một phần vào câu trả lời. 

Cách tiếp cận này đúng vì mọi phân tách hợp lệ đều tương ứng với chính xác một đường dẫn trong cây đệ quy. Vấn đề là hậu tố tương tự được tính toán lại nhiều lần. Trong trường hợp xấu nhất, chẳng hạn như một văn bản chứa đầy`?`ký tự và nhiều từ phù hợp, số lượng trạng thái đệ quy tăng theo cấp số nhân. 

Công việc lặp đi lặp lại gợi ý lập trình động. 

Cho phép`dp[i]`biểu thị số cách phân tách tiền tố kết thúc trước vị trí`i`. Sau đó, mỗi từ phù hợp sẽ chuyển từ vị trí trước sang vị trí sau. 

Giả sử một từ có độ dài`L`khớp với chuỗi con kết thúc tại vị trí`i`. Sau đó, mọi phân tách hợp lệ của tiền tố trước từ đó có thể được mở rộng bằng từ này. Điều này mang lại:```
dp[i] += dp[i - L]
```Nhiệm vụ duy nhất còn lại là kiểm tra một cách hiệu quả xem một từ có khớp với một đoạn văn bản hay không. Vì độ dài từ tối đa là 10 nên việc so sánh đơn giản từng ký tự đã đủ nhanh. 

Lực lượng vũ phu khám phá sự phân hủy trực tiếp. Giải pháp lập trình động đếm tất cả các phân tách của mỗi tiền tố một lần và sử dụng lại các kết quả đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O( | T | 
| DP tối ưu | O( | T | × n × maxLen) | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ trong từ điển. 
2. Hãy để`m`là độ dài của văn bản và tạo một mảng`dp`chiều dài`m + 1`. 
3. Đặt`dp[0] = 1`. 

Điều này đại diện cho tiền tố trống. Có chính xác một cách để phân hủy không có gì. 
4. Đối với mọi vị trí`i`từ`0`ĐẾN`m - 1`, tra cứu từng từ trong từ điển. 
5. Để từ hiện tại có độ dài`L`. 

Nếu như`i + L > m`, từ không thể bắt đầu ở vị trí`i`, vì vậy hãy bỏ qua nó. 
6. So sánh từ với đoạn văn bản`T[i : i + L]`. 

Đối với mọi vị trí`j`, trận đấu thành công nếu một trong hai`T[i + j] == '?'`hoặc`T[i + j] == word[j]`. 
7. Nếu toàn bộ từ khớp, hãy thêm:```
dp[i + L] += dp[i]
```và lấy kết quả modulo`10^9 + 7`. 

Mỗi lần phân hủy đạt đến vị trí`i`có thể được mở rộng với từ phù hợp này. 
8. Sau khi xử lý tất cả các vị trí và từ, xuất ra`dp[m]`. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[i]`luôn bằng số lượng phân tách hợp lệ của phân tách đầu tiên`i`các ký tự của văn bản. 

Ban đầu điều này đúng vì tiền tố trống có chính xác một phân tách. 

Bất cứ khi nào một từ khớp với vị trí bắt đầu`i`, mọi phân tách được tính bởi`dp[i]`có thể được mở rộng bởi từ đó, tạo ra sự phân tách độ dài`i + L`. Quá trình chuyển đổi thêm chính xác những phân tách mới đó và không có gì khác. 

Mọi phân tách hợp lệ đều có một từ cuối cùng duy nhất. Khi từ cuối cùng đó được xử lý, quá trình phân tách được tính chính xác một lần thông qua tiền tố trước nó. Bởi vì mọi phân tách được tạo ra một lần và mọi phân tách được tạo ra đều hợp lệ,`dp[m]`tương đương với câu trả lời mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

n = int(input())
words = [input().strip() for _ in range(n)]
text = input().strip()

m = len(text)

dp = [0] * (m + 1)
dp[0] = 1

for i in range(m):
    if dp[i] == 0:
        continue

    for word in words:
        L = len(word)

        if i + L > m:
            continue

        ok = True
        for j in range(L):
            if text[i + j] != '?' and text[i + j] != word[j]:
                ok = False
                break

        if ok:
            dp[i + L] = (dp[i + L] + dp[i]) % MOD

print(dp[m])
```Mảng DP lưu trữ số tiền tố. Chức vụ`i`đại diện cho lần đầu tiên`i`các ký tự của văn bản, giúp cho quá trình chuyển tiếp trở nên rõ ràng và tránh các lỗi sai sót một. 

Việc kiểm tra phù hợp được thực hiện trực tiếp. Vì độ dài mỗi từ tối đa là 10 nên không cần băm, thử hoặc xử lý trước chuỗi. 

các`if dp[i] == 0`không cần phải kiểm tra tính chính xác nhưng nó tránh được những nỗ lực so khớp không cần thiết từ các trạng thái không thể truy cập được. 

Phép toán modulo được áp dụng sau mỗi phép cộng vì số lượng phân tách có thể tăng rất nhanh khi có nhiều`?`các nhân vật có mặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
a
b
c
d
ab
bc
cd
ab?d
```Cho phép`dp[i]`biểu thị số cách xếp thứ nhất`i`nhân vật. 

| Vị trí | Các từ phù hợp bắt đầu từ đây | Cập nhật DP | 
| --- | --- | --- | 
| 0 | một, ab | dp[1]+=1, dp[2]+=1 | 
| 1 | b, bc | dp[2]+=1, dp[3]+=1 | 
| 2 | a, b, c, d, cd | dp[3]+=2, dp[4]+=2, dp[4]+=2, dp[4]+=2, dp[4]+=2 | 
| 3 | d | dp[4]+=5 | 

DP cuối cùng: 

| tôi | 0 | 1 | 2 | 3 | 4 | 
| --- | --- | --- | --- | --- | --- | 
| dp[i] | 1 | 1 | 2 | 3 | 11 | 

Trả lời:```
11
```Ví dụ này cho thấy một đơn`?`có thể đồng thời ghép nhiều từ khác nhau, mỗi từ góp phần phân tách bổ sung. 

### Ví dụ 2 

đầu vào:```
2
a
b
??
```| Vị trí | Từ phù hợp | Cập nhật DP | 
| --- | --- | --- | 
| 0 | một, b | dp[1]=2 | 
| 1 | một, b | dp[2]=4 | 

DP cuối cùng: 

| tôi | 0 | 1 | 2 | 
| --- | --- | --- | --- | 
| dp[i] | 1 | 2 | 4 | 

Trả lời:```
4
```Điều này chứng tỏ rằng mỗi`?`phân nhánh độc lập thành nhiều lựa chọn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | T | 
| Không gian | O( | T | 

Với`|T| ≤ 100`,`n ≤ 100`, Và`maxLen ≤ 10`, trường hợp xấu nhất là so sánh khoảng 100.000 ký tự, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    MOD = 10 ** 9 + 7

    n = int(input())
    words = [input().strip() for _ in range(n)]
    text = input().strip()

    m = len(text)
    dp = [0] * (m + 1)
    dp[0] = 1

    for i in range(m):
        if dp[i] == 0:
            continue

        for word in words:
            L = len(word)

            if i + L > m:
                continue

            ok = True
            for j in range(L):
                if text[i + j] != '?' and text[i + j] != word[j]:
                    ok = False
                    break

            if ok:
                dp[i + L] = (dp[i + L] + dp[i]) % MOD

    return str(dp[m]) + "\n"

# provided samples
assert run(
"""7
a
b
c
d
ab
bc
cd
ab?d
"""
) == "11\n", "sample 1"

assert run(
"""2
a
b
??
"""
) == "4\n", "sample 2"

# minimum size
assert run(
"""1
a
a
"""
) == "1\n", "single exact match"

# impossible decomposition
assert run(
"""1
ab
a
"""
) == "0\n", "cannot cover whole text"

# multiple words match same segment
assert run(
"""2
ab
ac
a?
"""
) == "2\n", "two matching words"

# overlapping decomposition choices
assert run(
"""3
a
aa
aaa
aaa
"""
) == "4\n", "different partition lengths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một từ`a`, chữ`a`| 1 | Trường hợp hợp lệ nhỏ nhất | 
| Một từ`ab`, chữ`a`| 0 | Bảo hiểm không thể | 
| Từ`ab`,`ac`, chữ`a?`| 2 | Nhiều từ phù hợp với cùng một phân đoạn | 
| Từ`a`,`aa`,`aaa`, chữ`aaa`| 4 | Nhiều cấu trúc phân vùng | 

## Vỏ cạnh 

Hãy xem xét:```
2
ab
ac
a?
```Ở vị trí 0, cả hai từ đều khớp nhau vì`?`có thể đại diện cho một trong hai`b`hoặc`c`. 

Thuật toán thực hiện cả hai lần chuyển đổi:```
dp[2] += dp[0]
dp[2] += dp[0]
```cho đi`dp[2] = 2`. Một giải pháp chỉ kiểm tra xem một số từ khớp có tạo ra 1 không chính xác hay không. 

Hãy xem xét:```
2
a
b
??
```Ở ký tự đầu tiên, cả hai từ đều khớp nhau. Điều tương tự cũng xảy ra ở ký tự thứ hai. 

DP phát triển như sau:```
dp = [1, 2, 4]
```và trả về 4. Điều này đếm chính xác mọi phép gán chữ cái được ngụ ý trong các từ đã chọn. 

Coi như:```
1
ab
a
```Từ này dài hơn văn bản còn lại. Quá trình chuyển đổi bị bỏ qua vì`i + L > m`. Không có trạng thái nào đi đến cuối cùng, vì vậy`dp[m]`vẫn bằng 0, cho biết chính xác rằng văn bản không thể phân tách thành các từ trong từ điển.
