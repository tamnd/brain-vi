---
title: "CF 103666B - \u0422\u0440\u043e\u0439\u043d\u043e\u0439 \u0424\u0438\u0431\u043e\u043d\u0430\u0447\u0447\u0438"
description: "Chúng ta có một dãy giống Fibonacci trong đó hai số hạng đầu tiên được cố định là $F1 = 1$ và $F2 = 2$, và mỗi số hạng sau đó là tổng của hai số hạng trước đó. Điều này tạo ra một chuỗi số nguyên vô hạn xác định."
date: "2026-07-02T21:06:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103666
codeforces_index: "B"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2016"
rating: 0
weight: 103666
solve_time_s: 51
verified: true
draft: false
---

[CF 103666B - \u0422\u0440\u043e\u0439\u043d\u043e\u0439 \u0424\u0438\u0431\u043e\u043d\u0430\u0447\u0447\u0438](https://codeforces.com/problemset/problem/103666/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy giống Fibonacci trong đó hai số hạng đầu tiên được cố định là$F_1 = 1$Và$F_2 = 2$, và mọi số hạng sau đều bằng tổng của hai số hạng trước đó. Điều này tạo ra một chuỗi số nguyên vô hạn xác định. Nhiệm vụ không phải là tính toán trực tiếp các giá trị lớn mà tập trung vào một đoạn của chuỗi này được lập chỉ mục từ$L$ĐẾN$R$và đếm xem có bao nhiêu số Fibonacci được lập chỉ mục chia hết cho 3. 

Đầu vào cho hai số nguyên$L$Và$R$, mô tả một khối chỉ số liền kề trong dãy Fibonacci. Đầu ra là một số nguyên biểu thị có bao nhiêu số Fibonacci trong phạm vi chỉ số đó có số dư bằng 0 khi chia cho 3. 

Những ràng buộc cho phép$L, R \le 10^5$, điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào tính toán lại các giá trị Fibonacci một cách nguyên bản cho đến từng điểm cuối truy vấn đều ổn về mặt số lần lặp thô, kể từ khi tạo$10^5$Số Fibonacci là tầm thường. Tuy nhiên, vấn đề thực sự không phải là tự tính toán các số Fibonacci mà là kiểm tra tính chia hết và nhận biết cấu trúc trong thuộc tính đó một cách hiệu quả. 

Một cạm bẫy nhỏ xuất hiện khi cố gắng tính trực tiếp các giá trị Fibonacci lớn. Mặc dù$n \le 10^5$, các số Fibonacci tăng theo cấp số nhân và vượt quá phạm vi số nguyên tiêu chuẩn rất nhanh. Việc triển khai đơn giản bằng cách sử dụng số nguyên lớn vẫn có thể thực hiện được bằng Python, nhưng không cần thiết. Quan trọng hơn, việc tính toán lại các giá trị đầy đủ là một nỗ lực lãng phí vì chúng ta chỉ quan tâm đến khả năng chia hết cho 3, điều này chỉ phụ thuộc vào chuỗi modulo 3. 

Không có trường hợp khó khăn nào phát sinh từ định dạng hoặc phạm vi đầu vào, nhưng có một trường hợp khái niệm: nếu ai đó tính toán trực tiếp các giá trị Fibonacci và sau đó kiểm tra khả năng chia hết, họ có thể cho rằng Python xử lý nó một cách an toàn. Đúng vậy, nhưng nó che giấu sự thật rằng cấu trúc của bài toán cho phép giải pháp thời gian không đổi cho mỗi chỉ mục sau khi tiền xử lý. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tạo ra từng số Fibonacci từ$F_1$lên đến$F_R$, kiểm tra từng giá trị và đếm xem có bao nhiêu trong phạm vi$[L, R]$đều chia hết cho 3. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Chi phí là tuyến tính trong$R$, với mỗi bước thực hiện phép cộng và kiểm tra modulo. Trong Python điều này vẫn khả thi đối với$10^5$điều khoản, nhưng nó đang làm công việc không cần thiết. 

Quan sát quan trọng là chúng ta không bao giờ cần các giá trị Fibonacci đầy đủ. Chúng ta chỉ cần chúng theo modulo 3. Sau khi giảm hàm truy hồi theo modulo 3, chúng ta sẽ có một máy trạng thái nhỏ: mỗi số hạng chỉ phụ thuộc vào hai dư lượng theo modulo 3 trước đó. Vì mỗi dư lượng nằm trong$\{0,1,2\}$, toàn bộ quá trình cuối cùng phải lặp lại. Trên thực tế, chuỗi này có cấu trúc tuần hoàn rất ngắn, do đó mẫu chia hết cho 3 trở thành tuần hoàn trong chỉ số. 

Một khi chúng ta biết mẫu này, vấn đề sẽ giảm xuống việc đếm xem có bao nhiêu chỉ số trong$[L, R]$rơi vào các vị trí có dư lượng Fibonacci bằng 0. Điều này có thể được trả lời bằng cách tính toán trước chu kỳ đầu tiên và sử dụng phép tính số học hoặc bằng cách xây dựng tổng tiền tố theo mẫu tuần hoàn lên đến$10^5$. 

Một cách tiếp cận đơn giản và an toàn không kém là tính toán trước chuỗi dư lượng lên đến$R$, xây dựng tổng tiền tố của các vị trí trong đó$F_i \bmod 3 = 0$và trả lời truy vấn trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Fibonacci Brute Force + kiểm tra |$O(R)$|$O(1)$| Đã chấp nhận | 
| Modulo + tổng tiền tố |$O(R)$tiền xử lý,$O(1)$truy vấn |$O(R)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng dãy Fibonacci chỉ modulo 3, vì khả năng chia hết chỉ phụ thuộc vào số dư. 

1. Tính một mảng`f[i]`lưu trữ$F_i \bmod 3$cho tất cả$i \le R$. Mỗi giá trị được tính toán bằng cách sử dụng phép lặp$f[i] = (f[i-1] + f[i-2]) \bmod 3$. Điều này giữ cho số lượng nhỏ và tránh tràn hoàn toàn. 
2. Xây dựng mảng tổng tiền tố`pref[i]`Ở đâu`pref[i]`đếm xem có bao nhiêu chỉ số$1 \le j \le i$thỏa mãn$f[j] = 0$. Điều này chuyển đổi vấn đề thành tính phạm vi, đó là lý do tại sao tổng tiền tố lại hữu ích ở đây. 
3. Trả lời câu hỏi$[L, R]$bằng cách tính toán`pref[R] - pref[L-1]`. Điều này hiệu quả vì tổng tiền tố mã hóa tần số tích lũy, do đó phép trừ sẽ tách biệt chính xác phạm vi mà chúng ta quan tâm. 

Tại sao nó hoạt động: Fibonacci lặp lại modulo 3 xác định một chuỗi xác định của các dư lượng, do đó mọi chỉ số đều có một giá trị cố định trước. Mảng tiền tố lưu trữ số lượng chính xác các vị trí thỏa mãn điều kiện chia hết cho bất kỳ chỉ mục nào. Vì chuỗi được tính toán trước hoàn toàn mà không có phép tính gần đúng, nên việc trừ các giá trị tiền tố sẽ tách biệt chính xác số phần tử hợp lệ trong bất kỳ khoảng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

L = int(input().strip())
R = int(input().strip())

n = R

f = [0] * (n + 1)
pref = [0] * (n + 1)

if n >= 1:
    f[1] = 1 % 3
    pref[1] = 1 if f[1] == 0 else 0

if n >= 2:
    f[2] = 2 % 3
    pref[2] = pref[1] + (1 if f[2] == 0 else 0)

for i in range(3, n + 1):
    f[i] = (f[i - 1] + f[i - 2]) % 3
    pref[i] = pref[i - 1] + (1 if f[i] == 0 else 0)

ans = pref[R] - (pref[L - 1] if L > 1 else 0)
print(ans)
```Việc triển khai trực tiếp phản ánh sự lặp lại nhưng ngay lập tức giảm mọi giá trị Fibonacci modulo 3, ngăn chặn sự tăng trưởng không cần thiết. Mảng tiền tố được cập nhật trong cùng một vòng lặp để tránh vượt qua lần thứ hai. 

Một chi tiết tinh tế là việc xử lý$L = 1$, từ`pref[0]`không tồn tại trong mảng. Điều này được xử lý rõ ràng bằng cách coi nó là số không. 

## Ví dụ đã hoạt động 

Hãy xem xét ví dụ ở đâu$L = 3$Và$R = 7$. 

Ta tính trình tự: 

| tôi | F_i | F_i mod 3 | số tiền tố | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 2 | 0 | 
| 3 | 3 | 0 | 1 | 
| 4 | 5 | 2 | 1 | 
| 5 | 8 | 2 | 1 | 
| 6 | 13 | 1 | 1 | 
| 7 | 21 | 0 | 2 | 

Kết quả truy vấn là$pref[7] - pref[2] = 2 - 0 = 2$. 

Dấu vết này cho thấy rằng một khi dư lượng được theo dõi, vấn đề sẽ giảm xuống còn việc đếm đơn giản. 

Bây giờ hãy xem xét$L = 1, R = 5$: 

| tôi | F_i mod 3 | tiền tố | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| 3 | 0 | 1 | 
| 4 | 2 | 1 | 
| 5 | 2 | 1 | 

Câu trả lời là$pref[5] - pref[0] = 1$, xác nhận rằng chỉ có một số Fibonacci trong phạm vi này chia hết cho 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(R)$| Mỗi giá trị tiền tố và dư lượng Fibonacci được tính một lần | 
| Không gian |$O(R)$| Mảng lưu trữ giá trị lên đến chỉ mục$R$| 

Ràng buộc$R \le 10^5$làm cho giải pháp này nhanh chóng một cách thoải mái, vì cả mức sử dụng bộ nhớ và thời gian đều tuyến tính và nhỏ về mặt tuyệt đối. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return __import__('builtins').print.__self__  # placeholder to avoid execution issues

# Since full harness is not executable here, illustrative asserts:

# provided sample
# assert run("3\n7\n") == "2"

# edge: smallest range
# assert run("1\n1\n") == "0"

# edge: first divisible case
# assert run("3\n3\n") == "1"

# small range
# assert run("1\n6\n") == "1"

# larger range
# assert run("1\n10\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | phần tử đơn không chia hết | 
| 3 3 | 1 | số hạng Fibonacci chia hết đầu tiên | 
| 1 6 | 1 | độ chính xác phạm vi nhỏ | 
| 1 10 | 2 | nhiều giai đoạn mẫu | 

## Vỏ cạnh 

Vụ án$L = 1$là điều kiện biên cấu trúc duy nhất vì nó buộc phải xử lý chỉ mục tiền tố bị thiếu. Ví dụ: với đầu vào:```
1
1
```Chúng tôi chỉ tính toán$F_1 = 1$, không chia hết cho 3, vì vậy câu trả lời đúng là 0. Thuật toán xử lý điều này bằng cách xử lý`pref[0]`bằng 0 một cách rõ ràng. 

Đối với trường hợp thứ hai:```
2
3
```Chúng tôi có$F_2 = 2$,$F_3 = 3$. Chỉ có chỉ số 3 đóng góp nên câu trả lời là 1. Phép trừ tiền tố`pref[3] - pref[1]`cách ly chính xác chỉ mục hợp lệ duy nhất này mà không cần logic trường hợp đặc biệt ngoài việc xử lý ranh giới.
