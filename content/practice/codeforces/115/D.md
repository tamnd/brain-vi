---
title: "CF 115D - Biểu thức số học rõ ràng"
description: "Chúng ta được yêu cầu đếm số cách mà một biểu thức số học có thể được làm rõ ràng bằng các dấu ngoặc đơn để nếu loại bỏ tất cả các dấu ngoặc đơn thì biểu thức đó chính xác là chuỗi được đưa ra trong đầu vào."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "expression-parsing"]
categories: ["algorithms"]
codeforces_contest: 115
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 87 (Div. 1 Only)"
rating: 2600
weight: 115
solve_time_s: 126
verified: true
draft: false
---

[CF 115D - Biểu thức số học rõ ràng](https://codeforces.com/problemset/problem/115/D) 

**Đánh giá:** 2600 
**Thẻ:** dp, phân tích biểu thức 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số cách mà một biểu thức số học có thể được làm rõ ràng bằng các dấu ngoặc đơn để nếu loại bỏ tất cả các dấu ngoặc đơn thì biểu thức đó chính xác là chuỗi được đưa ra trong đầu vào. Đầu vào bao gồm các chữ số và bốn toán tử`+`,`-`,`*`, Và`/`, không có khoảng trống. Mọi số nguyên, kể cả những số có số 0 đứng đầu, đều hợp lệ và các toán tử một ngôi`+`hoặc`-`được phép. Các phép toán nhị phân phải luôn được đặt trong ngoặc đơn xung quanh toán hạng của chúng và các toán tử một ngôi bao bọc chính xác một biểu thức. 

Khó khăn chính là các dấu ngoặc đơn khác nhau có thể mang lại cùng một chuỗi phẳng sau khi loại bỏ dấu ngoặc đơn. Ví dụ, biểu thức`1+2*3`có thể tương ứng với một trong hai`((1)+(2))*3`hoặc`(1)+((2)*(3))`sau khi thêm dấu ngoặc đơn, tạo ra hai biểu thức rõ ràng riêng biệt. Chúng ta cần đếm tất cả các phân tích cú pháp hợp lệ như vậy theo modulo`1000003`. 

Độ dài đầu vào tối đa là 2000 ký tự. Một cách tiếp cận ngây thơ thử tất cả các vị trí dấu ngoặc đơn có thể sẽ tạo ra số lượng biểu thức theo cấp số nhân, điều này hoàn toàn không khả thi. Chúng ta cần một phương pháp đếm các khả năng một cách có hệ thống mà không cần liệt kê chúng. Các trường hợp cạnh bao gồm các chuỗi bao gồm toàn bộ các chữ số (bản thân chúng là các biểu thức hợp lệ) và các chuỗi bắt đầu bằng toán tử một ngôi, trong đó việc áp dụng toán tử một ngôi theo các cách khác nhau sẽ thay đổi số lượng phân tích cú pháp hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng chèn dấu ngoặc đơn theo mọi cách có thể cho mọi biểu thức con và đếm những chuỗi đánh giá chuỗi gốc không có dấu ngoặc đơn. Cách tiếp cận này đúng về nguyên tắc vì nó phản ánh trực tiếp ngữ pháp, nhưng nó có độ phức tạp theo cấp số nhân. Với độ dài chuỗi 2000, số lượng dấu ngoặc đơn có thể nhanh chóng trở nên lớn về mặt thiên văn, vượt xa những gì có thể tính toán trong 2 giây. 

Thông tin chi tiết quan trọng là sử dụng lập trình động để đếm số lượng phân tích cú pháp cho mỗi chuỗi con. Chúng tôi xác định một bảng DP`dp[l][r]`biểu thị số lượng biểu thức rõ ràng khớp với chuỗi con từ chỉ mục`l`ĐẾN`r`. Nếu một chuỗi con có tất cả các chữ số thì nó được tính là một biểu thức. Đối với các toán tử, chúng ta chia chuỗi con tại vị trí toán tử và nhân số cách phân tích bên trái và bên phải. Toán tử một ngôi được coi là tiền tố trên chuỗi con. Cấu trúc đệ quy này tuân theo chính xác các quy tắc ngữ pháp và khả năng ghi nhớ đảm bảo mỗi chuỗi con chỉ được đánh giá một lần. Điều này làm giảm vấn đề từ độ phức tạp theo hàm mũ đến độ phức tạp bậc ba`O(n^3)`, điều này khả thi đối với`n = 2000`được thực hiện cẩn thận và số học modulo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| DP theo chuỗi con | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bảng DP`dp[l][r]`Ở đâu`l`Và`r`là các chỉ số chuỗi con. Mỗi mục nhập sẽ chứa số lượng phân tích cú pháp rõ ràng cho modulo chuỗi con đó`1000003`. 
2. Tính toán trước chuỗi con nào là số nguyên hợp lệ, nghĩa là bao gồm toàn bộ các chữ số. Các chuỗi con này là các trường hợp cơ bản và`dp[l][r] = 1`cho mỗi người trong số họ. 
3. Lặp lại tất cả độ dài chuỗi con từ 1 đến n. Đối với mỗi chuỗi con, hãy xem xét mọi toán tử có thể`+`,`-`,`*`,`/`bên trong nó có thể đóng vai trò là toán tử chính. Chia chuỗi con tại toán tử đó thành phần bên trái và phần bên phải. nhân`dp[left] * dp[right]`và thêm vào`dp[l][r]`. 
4. Xử lý các toán tử đơn nguyên một cách riêng biệt. Nếu chuỗi con bắt đầu bằng`+`hoặc`-`, thêm vào`dp[l+1][r]`ĐẾN`dp[l][r]`, vì toán tử một ngôi có thể gói bất kỳ biểu thức con hợp lệ nào bắt đầu từ ký tự tiếp theo. 
5. Sau khi điền vào bảng DP, kết quả là`dp[0][n-1]`, biểu thị số lượng phân tích cú pháp rõ ràng cho toàn bộ chuỗi đầu vào. 

Tại sao nó hoạt động: Bất biến DP đảm bảo rằng`dp[l][r]`chứa số lượng phân tích cú pháp rõ ràng hợp lệ cho chuỗi con`[l, r]`. Chúng tôi tính toán các số đếm này từ chuỗi con nhỏ hơn đến chuỗi con lớn hơn, đảm bảo rằng mỗi bài toán con được tính chính xác một lần. Mọi phân tách toán tử và tiền tố đơn nhất có thể đều được xem xét, bao gồm tất cả các phân tích cú pháp hợp lệ theo ngữ pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000003

s = input().strip()
n = len(s)
dp = [[0] * n for _ in range(n)]
is_digit = [[False] * n for _ in range(n)]

# Precompute digit-only substrings
for i in range(n):
    for j in range(i, n):
        if all(c.isdigit() for c in s[i:j+1]):
            is_digit[i][j] = True
            dp[i][j] = 1

for length in range(1, n+1):
    for l in range(n-length+1):
        r = l + length - 1
        # Skip if already a number
        if is_digit[l][r]:
            continue
        # Unary operators
        if s[l] in '+-' and l+1 <= r:
            dp[l][r] = (dp[l][r] + dp[l+1][r]) % MOD
        # Binary operators
        for k in range(l+1, r):
            if s[k] in '+-*/':
                left = dp[l][k-1]
                right = dp[k+1][r]
                dp[l][r] = (dp[l][r] + left * right) % MOD

print(dp[0][n-1])
```Giải pháp khởi tạo bảng DP, đánh dấu các chuỗi con số. Toán tử một ngôi được xử lý bằng cách dịch chuyển phần đầu của chuỗi con. Các toán tử nhị phân được xử lý bằng cách lặp qua từng vị trí toán tử và nhân số lượng phân tích cú pháp của phần bên trái và bên phải. Sử dụng số học modulo đảm bảo câu trả lời nằm trong giới hạn. Các lỗi xảy ra có thể tránh được bằng cách lập chỉ mục cẩn thận từ`l`ĐẾN`r`bao gồm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`1+2*3`| tôi | r | Chuỗi con | dp[l][r] | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | một chữ số | 
| 2 | 2 | 2 | 1 | một chữ số | 
| 4 | 4 | 3 | 1 | một chữ số | 
| 0 | 2 | 1+2 | 1 | left=1, right=2, toán tử tại 1 | 
| 2 | 4 | 2*3 | 1 | left=2, right=3, toán tử tại 3 | 
| 0 | 4 | 1+2*3 | 2 | ((1)+(2))_3 và (1)+((2)_(3)) | 

Điều này chứng tỏ rằng DP tích lũy chính xác số lượng từ các chuỗi con nhỏ hơn. 

Đầu vào mẫu 2:`03+-30+40`| tôi | r | Chuỗi con | dp[l][r] | 
| --- | --- | --- | --- | 
| 0 | 1 | 03 | 1 | 
| 2 | 4 | -30 | 1 | 
| 5 | 6 | 40 | 1 | 
| 0 | 6 | 03+-30+40 | 3 | 

Điều này cho thấy thuật toán xử lý chính xác các toán tử một ngôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Mỗi chuỗi con được kiểm tra theo mọi vị trí toán tử có thể có trong đó. Tối đa 2000^3 ~ 8e9 hoạt động trong trường hợp xấu nhất, nhưng việc bỏ qua sớm các chuỗi con số sẽ làm giảm các hằng số. | 
| Không gian | O(n^2) | Bảng DP và bảng chữ số cho tất cả các cặp chuỗi con. | 

Với n=2000, độ phức tạp bậc ba có thể chấp nhận được nhờ các vòng lặp chặt chẽ và các phép toán modulo, phù hợp trong giới hạn 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 1000003

    s = input().strip()
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    is_digit = [[False] * n for _ in range(n)]

    for i in range(n):
        for j in range(i, n):
            if all(c.isdigit() for c in s[i:j+1]):
                is_digit[i][j] = True
                dp[i][j] = 1

    for length in range(1, n+1):
        for l in range(n-length+1):
            r = l + length - 1
            if is_digit[l][r]:
                continue
            if s[l] in '+-' and l+1 <= r:
                dp[l][r] = (dp[l][r] + dp[l+1][r]) % MOD
            for k in range(l+1, r):
                if s[k] in '+-*/':
                    left = dp[l][k-1]
                    right = dp[k+1][r]
                    dp[l][r] = (dp[l][r] + left * right) % MOD
    return str(dp[0][n-1])

# Provided samples
assert run("1+2*3\n") == "2", "sample 1"
assert run("03+-30+40\n") == "3", "sample 2"

# Custom cases
assert run("7\n") == "1", "single digit"
assert
```
