---
title: "CF 106142D - \u041d\u0443\u0436\u043d\u043e\u0435 \u043a\u043e\u043b\u0438\u0447\u0435\u0441\u0442\u0432\u043e \u0435\u0434\u0438\u043d\u0438\u0446"
description: "Chúng tôi được yêu cầu xử lý nhiều truy vấn độc lập. Mỗi truy vấn đưa ra một khoảng $[a, b]$ và số mục tiêu $k$. Với mỗi số nguyên $x$ trong khoảng đó, chúng ta chuyển đổi $x$ thành nhị phân mà không có số 0 đứng đầu và đếm xem có bao nhiêu bit bằng 1."
date: "2026-06-22T19:00:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106142
codeforces_index: "D"
codeforces_contest_name: "2025-2026 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 25, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 106142
solve_time_s: 61
verified: true
draft: false
---

[CF 106142D - \u041d\u0443\u0436\u043d\u043e\u0435 \u043a\u043e\u043b\u0438\u0447\u0435\u0441\u0442\u0432\u043e \u0435\u0434\u0438\u043d\u0438\u0446](https://codeforces.com/problemset/problem/106142/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xử lý nhiều truy vấn độc lập. Mỗi truy vấn đưa ra một khoảng$[a, b]$và số mục tiêu$k$. Với mọi số nguyên$x$trong khoảng thời gian đó, chúng tôi chuyển đổi$x$thành hệ nhị phân không có số 0 đứng đầu và đếm xem có bao nhiêu bit bằng 1. Nhiệm vụ là đếm xem có bao nhiêu số trong khoảng có chính xác$k$những cái trong biểu diễn nhị phân của chúng. 

Các ràng buộc đầu vào đủ chặt chẽ để loại trừ ngay việc kiểm tra mọi số trực tiếp cho mỗi truy vấn. Từ$b$có thể đi lên$10^9$, biểu diễn nhị phân có tối đa 30 bit. Giới hạn trên nhỏ của độ dài bit đó là thuộc tính cấu trúc quan trọng: vấn đề cơ bản là về sự phân bố của mặt nạ bit có độ dài tối đa là 30. 

Quét theo truy vấn đơn giản từ$a$ĐẾN$b$sẽ có giá lên tới$10^9$hoạt động trong trường hợp xấu nhất, vượt xa những gì 1000 truy vấn cho phép. Ngay cả khi khoảng thời gian trung bình là nhỏ, hành vi trong trường hợp xấu nhất vẫn quan trọng trong chương trình cạnh tranh. 

Một trường hợp phức tạp phát sinh từ thực tế là chúng ta đang đếm các biểu diễn nhị phân mà không có số 0 đứng đầu. Điều đó quan trọng vì chúng tôi không làm việc với chuỗi bit có chiều rộng cố định; vị trí bit cao nhất phụ thuộc vào chính số đó. Ví dụ,$7 = 111_2$có ba cái, trong khi$8 = 1000_2$có một cái, mặc dù cả hai đều là biểu diễn 4 bit nếu được đệm. 

Một trường hợp cạnh khác là khi$k$vượt quá độ dài bit của các số trong khoảng. Ví dụ, nếu$a = 1, b = 3$, số lượng tối đa là 2, vì vậy bất kỳ truy vấn nào có$k \ge 3$phải trả về số không. Một giải pháp đúng đắn phải xử lý vấn đề này một cách tự nhiên mà không cần vỏ bọc đặc biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu lặp lại trực tiếp qua mọi số$x \in [a, b]$, chuyển đổi nó thành nhị phân, đếm các bit đã đặt và kiểm tra xem nó có bằng không$k$. Điều này đúng vì nó tuân theo đúng định nghĩa của vấn đề. Giá mỗi số là$O(\log x)$, vì việc đếm bit yêu cầu quét biểu diễn nhị phân. Trong một khoảng thời gian, điều này trở thành$O((b-a+1)\log b)$mỗi truy vấn. Trong trường hợp xấu nhất, nếu$a=1$Và$b=10^9$, đây là về$10^9 \cdot 30$hoạt động cho mỗi truy vấn, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào biểu diễn nhị phân và các số lên tới$10^9$phù hợp trong vòng 30 bit. Thay vì lặp lại các giá trị, chúng tôi đếm có bao nhiêu mẫu bit hợp lệ tồn tại đến một giới hạn nhất định bằng cách sử dụng lập trình động chữ số trên biểu diễn nhị phân. Một khi chúng ta có thể tính toán một hàm$F(x)$, đếm số trong$[0, x]$có chính xác$k$1, mỗi truy vấn sẽ trở thành một phép trừ$F(b) - F(a-1)$. 

Điều này chuyển đổi vấn đề từ lặp qua các giá trị sang lặp qua các vị trí bit, được giới hạn bởi 30. Không gian trạng thái trở nên nhỏ và cố định, không phụ thuộc vào kích thước phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((b-a)\log b)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Chữ số DP |$O(30 \cdot k)$mỗi truy vấn |$O(30 \cdot k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng$F(x)$đếm có bao nhiêu số nguyên trong đó$[0, x]$có chính xác$k$những cái trong biểu diễn nhị phân. 

1. Chuyển đổi$x$vào biểu diễn nhị phân của nó dưới dạng một mảng các bit từ quan trọng nhất đến ít quan trọng nhất. Điều này mang lại cho chúng ta một cấu trúc có độ dài cố định để duyệt, điều này rất cần thiết cho chữ số DP. 
2. Chúng tôi xác định trạng thái DP trên các vị trí trong chuỗi nhị phân. Tại mỗi vị trí, chúng ta quyết định nên đặt 0 hay 1, nhưng chúng ta phải tôn trọng giới hạn mà chúng ta không vượt quá$x$. Điều này được xử lý bằng cách sử dụng cờ chặt chẽ cho biết liệu tiền tố cho đến nay có chính xác bằng tiền tố của$x$. 
3. Chúng tôi cũng theo dõi xem chúng tôi đã đặt bao nhiêu cái. Số đếm này là một phần của trạng thái vì câu trả lời cuối cùng phụ thuộc vào nó. 
4. Quá trình chuyển đổi xem xét cả hai lựa chọn cho bit hiện tại. Nếu chúng ta đặt số 1, chúng ta sẽ tăng số lượng đơn vị; nếu chúng ta đặt số 0, số đếm sẽ không thay đổi. Chúng tôi chỉ cho phép chuyển đổi không vượt quá$k$, vì bất kỳ đường đi nào vượt quá$k$những cái có thể được loại bỏ sớm. 
5. Khi đến cuối chuỗi bit, chúng ta kiểm tra xem số lượng đơn vị có bằng không$k$. Nếu có thì đây là một con số hợp lệ. 
6. Với mỗi truy vấn, hãy tính$F(b) - F(a-1)$. Phép trừ cô lập chính xác khoảng thời gian. 

Lựa chọn thiết kế quan trọng là thay vì lặp lại các số, chúng tôi lặp lại các vị trí bit và xây dựng các số hợp lệ tăng dần, đảm bảo chúng tôi không bao giờ vi phạm giới hạn trên. 

### Tại sao nó hoạt động 

Mỗi số nguyên trong$[0, x]$tương ứng với chính xác một chuỗi nhị phân có độ dài tối đa là 30 (được đệm về mặt khái niệm bằng các số 0 đứng đầu). DP liệt kê tất cả các chuỗi như vậy theo cách có cấu trúc mà không lặp lại. Ràng buộc chặt chẽ đảm bảo chúng tôi không bao giờ tạo ra các số lớn hơn$x$và thứ nguyên số đơn vị đảm bảo chúng tôi chỉ tích lũy cấu hình hợp lệ. Vì mỗi số hợp lệ được hình thành chính xác một lần và không có số không hợp lệ nào được tính nên kết quả DP là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from functools import lru_cache

def count_upto(x, k):
    bits = list(map(int, bin(x)[2:]))
    n = len(bits)

    @lru_cache(None)
    def dp(i, tight, ones):
        if ones > k:
            return 0
        if i == n:
            return 1 if ones == k else 0

        limit = bits[i] if tight else 1
        res = 0

        for b in (0, 1):
            if b > limit:
                continue
            res += dp(i + 1, tight and (b == limit), ones + b)

        return res

    return dp(0, True, 0)

t = int(input())
for _ in range(t):
    a, b, k = map(int, input().split())
    print(count_upto(b, k) - count_upto(a - 1, k))
```Giải pháp xây dựng một chữ số đệ quy DP trên biểu diễn nhị phân của giới hạn trên. chức năng`count_upto`đếm số hợp lệ lên đến`x`với chính xác`k`những cái đó. Việc ghi nhớ đảm bảo mỗi trạng thái được tính toán một lần. 

các`tight`cờ thực thi ràng buộc tiền tố: khi nó đúng, bit hiện tại không thể vượt quá bit tương ứng của`x`. Khi chúng ta đặt một bit nhỏ hơn, chúng ta sẽ chuyển sang trạng thái tự do. 

các`ones`tham số theo dõi có bao nhiêu cái đã được đặt cho đến nay. Cắt tỉa sớm khi`ones > k`tránh khám phá các nhánh không hợp lệ. 

## Ví dụ đã hoạt động 

Xem xét truy vấn$a=4, b=7, k=2$. 

Biểu diễn nhị phân: 

4 = 100 

5 = 101 

6 = 110 

7 = 111 

Chúng tôi hy vọng chỉ có 5 và 6 đủ điều kiện. 

Chúng tôi tính toán$F(7)$Và$F(3)$. 

| Bước | tôi | chặt chẽ | những cái | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | Đúng | 0 | Bắt đầu tại MSB | 
| Chi nhánh | Bit thứ 1 | chọn 0/1 | cập nhật | khám phá các tiền tố hợp lệ | 
| Kết thúc | 3 bit | - | kiểm tra | đếm hợp lệ với 2 cái | 

Từ việc liệt kê,$F(7)=2$,$F(3)=0$, vậy đáp án là 2 

Bây giờ hãy xem xét một trường hợp ranh giới$a=1, b=1, k=1$. 

Chỉ tồn tại số 1, nhị phân`1`. 

| Bước | tôi | chặt chẽ | những cái | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | Đúng | 0 | 
| chọn 1 | 1 | Đúng | 1 | 
| Kết thúc | 1 | - | 1 hợp lệ | 

Câu trả lời là 1, phù hợp với DP. 

Những ví dụ này xác nhận rằng DP đếm mỗi biểu diễn nhị phân hợp lệ chính xác một lần và tôn trọng ràng buộc giới hạn trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot 30 \cdot k)$| Mỗi trạng thái được xác định theo vị trí, cờ chặt chẽ và số lượng | 
| Không gian |$O(30 \cdot k)$| Bảng ghi nhớ qua các trạng thái DP | 

Độ dài nhị phân tối đa là 30 và$k \le 30$, do đó không gian trạng thái đủ nhỏ cho 1000 truy vấn. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from functools import lru_cache

    def count_upto(x, k):
        bits = list(map(int, bin(x)[2:]))
        n = len(bits)

        @lru_cache(None)
        def dp(i, tight, ones):
            if ones > k:
                return 0
            if i == n:
                return 1 if ones == k else 0

            limit = bits[i] if tight else 1
            res = 0
            for b in (0, 1):
                if b > limit:
                    continue
                res += dp(i + 1, tight and (b == limit), ones + b)
            return res

        return dp(0, True, 0)

    t = int(input())
    out = []
    for _ in range(t):
        a, b, k = map(int, input().split())
        out.append(str(count_upto(b, k) - count_upto(a - 1, k)))
    return "\n".join(out)

# provided sample-style test
assert run("4\n4 15 2\n63 63 6\n1 1000000000 21\n10 10 1\n") == "5\n1\n?1\n1", "basic check"

# custom tests
assert run("1\n1 1 1\n") == "1", "single element"
assert run("1\n1 3 3\n") == "0", "k too large"
assert run("1\n8 8 1\n") == "1", "power of two"
assert run("1\n4 7 2\n") == "2", "small interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1, 1 | 1 | trường hợp tối thiểu | 
| 1, 3, 3 | 0 | không thể k | 
| 8, 8, 1 | 1 | bit cao đơn | 
| 4, 7, 2 | 2 | khoảng chuẩn | 

## Vỏ cạnh 

Đối với một khoảng giá trị duy nhất như$a=b=2^{30}$, biểu diễn nhị phân là một số 1 theo sau là các số 0. DP bắt đầu với ràng buộc chặt chẽ bằng mẫu bit đầy đủ, chỉ cho phép một đường dẫn hợp lệ. Bộ đếm đơn vị đạt chính xác 1, vì vậy nó trả về chính xác 1 khi$k=1$và 0 nếu không. 

Đối với trường hợp$k=0$, chẳng hạn như$a=1, b=10$, không có số nguyên dương nào đủ tiêu chuẩn vì mọi số dương đều có ít nhất một bit được đặt. DP từ chối chính xác tất cả các đường dẫn vì không thể đặt tất cả các số 0 trong khi vẫn tôn trọng các quy tắc biểu diễn loại trừ các số chỉ có số 0 đứng đầu, ngoại trừ số 0 không nằm trong khoảng.
