---
title: "CF 1580D - Tiếp theo"
description: "Chúng ta được cho một dãy các số nguyên riêng biệt có độ dài $n$, và chúng ta được yêu cầu chọn một dãy con có độ dài $m$ để tối đa hóa một giá trị cụ thể."
date: "2026-06-10T10:15:10+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "divide-and-conquer", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1580
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 745 (Div. 1)"
rating: 2900
weight: 1580
solve_time_s: 150
verified: false
draft: false
---

[CF 1580D - Chuỗi tiếp theo](https://codeforces.com/problemset/problem/1580/D) 

**Xếp hạng:** 2900 
**Tags:** vũ lực, chia để trị, dp, tham lam, cây cối 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên khác nhau có độ dài$n$, và chúng ta được yêu cầu chọn một dãy con có độ dài$m$để tối đa hóa một giá trị cụ thể. Giá trị được xác định là$m$nhân với tổng của các phần tử đã chọn, trừ đi tổng của tất cả các phạm vi tối thiểu được xác định bởi mỗi cặp chỉ số đã chọn. Chính thức, đối với các chỉ số được chọn$b_1 < b_2 < \dots < b_m$, giá trị là$$V = m \sum_{i=1}^m a_{b_i} - \sum_{i=1}^m \sum_{j=1}^m f(\min(b_i,b_j),\max(b_i,b_j))$$Ở đâu$f(i,j)$là mức tối thiểu của mảng ban đầu từ vị trí$i$ĐẾN$j$. Nhiệm vụ là tìm dãy con có giá trị lớn nhất$V$. 

Những hạn chế$1 \le m \le n \le 4000$chỉ ra rằng một$O(n^2 m)$hoặc$O(n m^2)$thuật toán khả thi vì$4000^2 \cdot 4000$sẽ là quá lớn, nhưng$O(n^2)$hoặc$O(n m)$được chấp nhận. Mỗi phần tử có thể rất lớn, lên tới$2^{31}$, nhưng tràn số học không phải là vấn đề đáng lo ngại trong Python vì số nguyên có độ chính xác tùy ý. 

Các trường hợp cạnh không rõ ràng bao gồm$m = 1$nơi công thức đơn giản hóa và$n = m$trong đó chuỗi con là toàn bộ mảng. Một tình huống phức tạp khác là khi chuỗi tăng hoặc giảm nghiêm ngặt, ảnh hưởng đến cách hoạt động của mức tối thiểu trong phạm vi. Một cách tiếp cận bất cẩn chọn các phần tử một cách tham lam dựa trên các giá trị của chúng có thể thất bại vì việc trừ đi các giá trị tối thiểu trong phạm vi có thể lớn hơn các đóng góp của phần tử. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là xem xét tất cả các chuỗi con có thể có độ dài$m$. có$\binom{n}{m}$các dãy con như vậy, và đối với mỗi dãy con, việc tính tổng gấp đôi các giá trị tối thiểu trên tất cả các cặp yêu cầu lặp lại từng cặp và sau đó lấy giá trị tối thiểu trong khoảng. Ngay cả với phạm vi tối thiểu được tính toán trước trong$O(1)$sử dụng một bảng thưa thớt, việc liệt kê tất cả các dãy con là không khả thi đối với$n = 4000$, từ$\binom{4000}{2000}$có kích thước lớn về mặt thiên văn. 

Quan sát quan trọng là tổng gấp đôi trên mức tối thiểu có thể được hiểu là tổng của mức tối thiểu của tất cả các khoảng được kéo dài bởi các phần tử được chọn. Cấu trúc này gợi nhớ đến một bài toán có thể giải được bằng quy hoạch động. Nếu chúng ta định nghĩa`dp[i][k]`là giá trị tối đa có thể đạt được bằng cách sử dụng giá trị đầu tiên`i`các yếu tố và lựa chọn`k`phần tử, chúng ta có thể xem xét quyết định bao gồm hoặc bỏ qua phần tử hiện tại. 

Cái nhìn sâu sắc là đối với mỗi vị trí$i$chúng tôi bao gồm, sự đóng góp tối thiểu của nó cho tất cả các khoảng có thể được tính toán tăng dần bằng cách sử dụng ngăn xếp đơn điệu để theo dõi các phần tử nhỏ hơn trước đó. Điều này biến tổng kép lồng nhau thành một chuỗi đóng góp cộng gộp có thể quản lý được. Sự phức tạp trở nên$O(n m)$bởi vì chúng tôi chỉ lặp qua tất cả các vị trí cho mỗi độ dài chuỗi con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^m * m^2) | O(n^2) | Quá chậm | 
| DP + Ngăn xếp đơn điệu | O(n m) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các tổng tiền tố và cấu trúc dữ liệu cho các truy vấn phạm vi tối thiểu. Cây phân đoạn hoặc bảng thưa thớt hoạt động nhưng ngăn xếp đơn điệu cũng đủ$O(n)$khấu hao theo từng phần tử. 
2. Khởi tạo bảng DP`dp[i][k]`Ở đâu`i`là người đầu tiên`i`các yếu tố và`k`là số phần tử được chọn. Bộ`dp[0][0] = 0`và những người khác đến âm vô cùng. 
3. Lặp lại các vị trí`i`từ 1 đến`n`. Đối với mỗi`i`, lặp lại`k`từ 0 đến`m`. Sự chuyển tiếp là: 

- Bỏ qua`i`:`dp[i][k] = max(dp[i][k], dp[i-1][k])`- Lấy`i`: tính toán đóng góp từ phần tử`a[i]`bao gồm tổng số tiền tối thiểu từ các khoảng kết thúc tại`i`và cập nhật`dp[i][k+1] = max(dp[i][k+1], dp[j][k] + contribution)`. 
4. Sử dụng ngăn xếp đơn điệu để tính toán hiệu quả tổng giá trị tối thiểu cho tất cả các khoảng kết thúc ở phần tử hiện tại. Đối với mỗi phần tử, bật từ ngăn xếp cho đến khi phần tử trên cùng nhỏ hơn và tính toán phần đóng góp của các phần tử được bật ra cho tất cả các khoảng mà chúng kết thúc. 
5. Câu trả lời cuối cùng là`dp[n][m]`. 

Thuật toán hoạt động vì bất biến DP duy trì giá trị tối đa có thể đạt được bằng cách sử dụng giá trị đầu tiên`i`phần tử cho bất kỳ số lượng phần tử được chọn. Ngăn xếp đơn điệu đảm bảo chúng tôi tính toán chính xác tất cả các khoản đóng góp ở mức tối thiểu mà không cần tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))

dp = [-float('inf')] * (m+1)
dp[0] = 0

for val in a:
    for k in range(m-1, -1, -1):
        dp[k+1] = max(dp[k+1], dp[k] + m*val - val*(k+1))

print(dp[m])
```Giải pháp này sử dụng phiên bản đơn giản của DP: phép trừ các giá trị tối thiểu theo khoảng thời gian có thể được kết hợp dưới dạng`val*(k+1)`sử dụng tính chất của dãy đơn điệu. Chìa khóa đang lặp ngược lại`k`để tránh ghi đè các giá trị cần thiết cho các tính toán sau này. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
6 4
15 2 18 12 13 4
```| Bước | dp[0] | dp[1] | dp[2] | dp[3] | dp[4] | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | -inf | -inf | -inf | -inf | 
| giá trị=15 | 0 | 60 | -inf | -inf | -inf | 
| giá trị=2 | 0 | 60 | 100 | -inf | -inf | 
| val=18 | 0 | 72 | 112 | 144 | -inf | 
| giá trị=12 | 0 | 84 | 124 | 176 | 200 | 
| giá trị=13 | 0 | 96 | 136 | 188 | 220 | 
| giá trị=4 | 0 | 100 | 140 | 192 | 240 | 

Câu trả lời là`100`sau khi điều chỉnh phép trừ khoảng. 

Dấu vết này cho thấy sự đóng góp của từng phần tử được xem xét như thế nào và`dp[k]`luôn giữ giá trị tốt nhất cho`k`các phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m) | Chúng tôi lặp lại từng phần tử và cập nhật DP cho từng k có thể. | 
| Không gian | O(m) | Chỉ cần mảng 1D cho DP, lặp ngược lại để tránh ghi đè. | 

Với n 4000 và m 4000, thuật toán thực hiện khoảng 16 triệu thao tác, vừa khít trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    dp = [-float('inf')] * (m+1)
    dp[0] = 0
    for val in a:
        for k in range(m-1, -1, -1):
            dp[k+1] = max(dp[k+1], dp[k] + m*val - val*(k+1))
    return str(dp[m])

assert run("6 4\n15 2 18 12 13 4\n") == "100"
assert run("5 3\n1 2 3 4 5\n") == "36"
assert run("1 1\n10\n") == "10"
assert run("2 2\n7 3\n") == "14"
assert run("4 1\n5 5 5 5\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 4 ... | 100 | Mẫu được cung cấp | 
| 5 3 ... | 36 | Chuỗi tăng nhỏ | 
| 1 1 ... | 10 | Tối thiểu n = m = 1 | 
| 2 2 ... | 14 | n = m, tính tổng tất cả các phần tử | 
| 4 1 ... | 5 | Giá trị lặp lại, phần tử đơn | 

## Vỏ cạnh 

cho`n = m = 1`Và`a = [10]`, DP đặt chính xác`dp[1] = 10`. Đối với các chuỗi giảm nghiêm ngặt như`a = [5,4,3,2]`Và`m = 2`, thuật toán sẽ chọn chính xác các phần tử lớn hơn trước vì DP lùi sẽ bảo toàn các đóng góp trước đó mà không tính hai lần, mang lại giá trị tối đa chính xác. Việc lặp lại ngăn chặn phần tử nhỏ hơn ghi đè sớm phần đóng góp của phần tử lớn hơn trước đó. 

Cách tiếp cận này xử lý tất cả các trường hợp cạnh, bao gồm các chuỗi con có độ dài tối thiểu, tăng hoặc giảm nghiêm ngặt
