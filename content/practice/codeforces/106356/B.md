---
title: "CF 106356B - Tổng hợp dễ dàng"
description: "Chúng ta được cho một số số nguyên tố. Với mỗi số nguyên tố $p$, chúng ta có thể đặt một hoặc nhiều chữ số thập phân trước biểu diễn thập phân của nó. Số kết quả là sự kết hợp của tiền tố đã chọn và $p$."
date: "2026-06-19T14:56:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106356
codeforces_index: "B"
codeforces_contest_name: "Replay of BUET IUPC 2026, Powered By Phitron"
rating: 0
weight: 106356
solve_time_s: 52
verified: true
draft: false
---

[CF 106356B - Tổng hợp dễ dàng](https://codeforces.com/problemset/problem/106356/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số số nguyên tố. Đối với mỗi số nguyên tố$p$, chúng ta có thể đặt một hoặc nhiều chữ số thập phân trước biểu diễn thập phân của nó. Số kết quả là sự nối của tiền tố đã chọn và$p$. 

Mục tiêu của chúng tôi là tạo ra một tiền tố làm cho số kết quả là tổng hợp, đồng thời sử dụng càng ít chữ số được thêm vào đầu càng tốt. Nếu một số tiền tố có độ dài tối thiểu hoạt động thì bất kỳ tiền tố nào trong số đó đều được chấp nhận. 

Quan sát chính xuất phát từ những hạn chế về$p$. Mỗi số đầu vào là số nguyên tố từ 10 đến$10^8$, Vì thế$p$không bao giờ bằng 2, 3 hoặc 5. Đặc biệt,$p \bmod 3$luôn là 1 hoặc 2. 

Một cách tiếp cận bất cẩn có thể thử nhiều tiền tố và kiểm tra tính nguyên tố của số kết quả. Điều đó sẽ hiệu quả với những đầu vào nhỏ, nhưng nó hoàn toàn bỏ qua cấu trúc của vấn đề. 

Coi như$p = 11$. 

Nếu chúng ta thêm chữ số 1 vào trước, chúng ta sẽ có 111. Vì$111 = 3 \times 37$, kết quả là tổng hợp. 

Câu trả lời chỉ sử dụng một chữ số được thêm vào trước, vì vậy nó đã tối ưu. 

Một ví dụ khác là$p = 13$. 

Thêm 2 vào trước sẽ được 213, và$213 = 3 \times 71$. 

Một lần nữa, một chữ số là đủ. 

Trường hợp cạnh quan trọng nhất là nhận ra rằng không được phép có chữ số 0 được thêm vào trước. Mặc dù số ban đầu là số nguyên tố nhưng chúng ta phải thêm ít nhất một chữ số vào trước. Do đó, độ dài câu trả lời tối thiểu có thể là 1, không phải 0. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử tất cả các tiền tố một chữ số, sau đó là tất cả các tiền tố hai chữ số, v.v. cho đến khi tìm thấy số tổng hợp. Đối với mỗi ứng cử viên, nó sẽ xây dựng số được nối và chạy kiểm tra tính nguyên tố. 

Điều này có hiệu quả vì cuối cùng một số tiền tố phải tạo ra một số tổng hợp. Vấn đề là việc kiểm tra tính nguyên thủy sẽ được lặp lại nhiều lần trong tối đa$10^5$trường hợp thử nghiệm. Mặc dù số kết quả không quá lớn nhưng việc thực hiện kiểm tra tính nguyên tố lặp đi lặp lại là không cần thiết. 

Quan sát quan trọng là chúng ta thực sự không cần phải tìm kiếm. 

Cho phép$k$là số chữ số của$p$. Nếu chúng ta thêm vào trước một chữ số$d$, số mới là$$N = d \cdot 10^k + p.$$Từ$10 \equiv 1 \pmod 3$,$$10^k \equiv 1 \pmod 3.$$Như vậy$$N \equiv d + p \pmod 3.$$Bởi vì$p$là số nguyên tố lớn hơn 3,$p \bmod 3$là 1 hoặc 2. 

Nếu$p \bmod 3 = 1$, chọn$d = 2$. 

Sau đó$$N \equiv 2 + 1 \equiv 0 \pmod 3.$$Nếu như$p \bmod 3 = 2$, chọn$d = 1$. 

Sau đó$$N \equiv 1 + 2 \equiv 0 \pmod 3.$$Số kết quả chia hết cho 3 và lớn hơn 3 nên phải là hợp số. 

Vì một chữ số luôn hoạt động nên số chữ số được thêm vào trước tối thiểu luôn là 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Phụ thuộc vào các thử nghiệm nguyên thủy lặp đi lặp lại | O(1) | Chậm một cách không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên tố$p$. 
2. Tính toán$p \bmod 3$. 
3. Nếu$p \bmod 3 = 1$, xuất ra chữ số`2`. 

Số liền nhau sẽ chia hết cho 3 vì$2 + 1 \equiv 0 \pmod 3$. 
4. Nếu không,$p \bmod 3 = 2$, do đó xuất ra chữ số`1`. 

Số liền nhau sẽ chia hết cho 3 vì$1 + 2 \equiv 0 \pmod 3$. 

### Tại sao nó hoạt động 

hãy để$k$là số chữ số của$p$. Sau khi thêm chữ số vào trước$d$, số mới là$$N = d \cdot 10^k + p.$$Từ$10^k \equiv 1 \pmod 3$,$$N \equiv d + p \pmod 3.$$Sự lựa chọn của chúng tôi$d$đảm bảo$d + p \equiv 0 \pmod 3$. Kể từ đây$N$chia hết cho 3. 

Bởi vì$N$chứa ít nhất hai chữ số và lớn hơn 3, chia hết cho 3 ngụ ý rằng$N$là tổng hợp. 

Tiền tố một chữ số luôn tồn tại và không có giải pháp nào có thể sử dụng ít hơn một chữ số vì không được phép thêm chữ số 0 vào trước. Vì vậy, câu trả lời được đưa ra luôn là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    p = int(input())

    if p % 3 == 1:
        print(2)
    else:
        print(1)
```Dung dịch chỉ sử dụng phần cặn của$p$mô-đun 3. 

Khi nào$p \equiv 1 \pmod 3$, chữ số`2`làm cho số được nối chia hết cho 3. 

Khi nào$p \equiv 2 \pmod 3$, chữ số`1`cũng làm như vậy. 

Không cần thiết phải xây dựng số cuối cùng. Số chữ số của$p$không bao giờ cần phải tính toán vì sự đồng đẳng$$10^k \equiv 1 \pmod 3$$giữ cho mọi$k$. 

Quá trình triển khai diễn ra trong thời gian không đổi cho mỗi trường hợp thử nghiệm và tránh tất cả thử nghiệm nguyên thủy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
11
```| Biến | Giá trị | 
| --- | --- | 
| p | 11 | 
| p% 3 | 2 | 
| chữ số đã chọn | 1 | 

Số được xây dựng sẽ là 111. 

| Kiểm tra | Giá trị | 
| --- | --- | 
| 111 % 3 | 0 | 
| Tổng hợp? | Có | 

Ví dụ này cho thấy trường hợp$p \equiv 2 \pmod 3$. Một chữ số duy nhất ngay lập tức tạo ra một số tổng hợp. 

### Ví dụ 2 

đầu vào:```
1
13
```| Biến | Giá trị | 
| --- | --- | 
| p | 13 | 
| p% 3 | 1 | 
| chữ số đã chọn | 2 | 

Số được xây dựng sẽ là 213. 

| Kiểm tra | Giá trị | 
| --- | --- | 
| 213 % 3 | 0 | 
| Tổng hợp? | Có | 

Ví dụ này cho thấy trường hợp$p \equiv 1 \pmod 3$. Đối số tương tự hoạt động với một chữ số khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có một phép toán modulo và một phép so sánh | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ | 

Ngay cả với$10^5$trường hợp thử nghiệm, tổng công việc là rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        p = int(input())
        ans.append("2" if p % 3 == 1 else "1")

    return "\n".join(ans) + "\n"

# basic examples
assert run("1\n11\n") == "1\n"
assert run("1\n13\n") == "2\n"

# smallest valid prime
assert run("1\n11\n") == "1\n"

# p % 3 = 1
assert run("1\n31\n") == "2\n"

# p % 3 = 2
assert run("1\n17\n") == "1\n"

# multiple test cases
assert run("4\n11\n13\n17\n31\n") == "1\n2\n1\n2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`11`|`1`| Trường hợp ở đâu$p \equiv 2 \pmod 3$| 
|`13`|`2`| Trường hợp ở đâu$p \equiv 1 \pmod 3$| 
|`17`|`1`| Một dư lượng-2 nguyên tố khác | 
|`31`|`2`| Một dư lượng-1 nguyên tố khác | 
| Nhiều đầu vào | Câu trả lời thay thế | Xử lý đúng nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Xét số nguyên tố hợp lệ nhỏ nhất:```
1
11
```Thuật toán tính toán$11 \bmod 3 = 2$và đầu ra`1`. Số kết quả là 111, chia hết cho 3. Vì sử dụng một chữ số nên câu trả lời là tối ưu. 

Xét một số nguyên tố với$p \equiv 1 \pmod 3$:```
1
97
```Đầu ra của thuật toán`2`. Số được xây dựng là 297. Vì$297 = 3 \times 99$, nó là hợp chất. Không thể có tiền tố ngắn hơn vì phải thêm vào trước ít nhất một chữ số. 

Xét một số nguyên tố lớn gần giới hạn trên:```
1
99999989
```Chỉ một$p \bmod 3$vấn đề. Thuật toán vẫn thực hiện một phép toán modulo đơn và đưa ra`1`hoặc`2`. Kích thước của số nguyên tố không ảnh hưởng đến thời gian chạy và không đổi.
