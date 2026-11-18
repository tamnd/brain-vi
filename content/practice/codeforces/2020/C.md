---
title: "CF 2020C - Cân bằng bit"
description: "Cho ba số nguyên không âm b, c và d. Chúng ta phải xây dựng một số nguyên không âm a sao cho $$(a mid b) - (a & c) = d$$ trong đó Nhiệm vụ không phải là tối ưu hóa một số giá trị của a. Bất kỳ a hợp lệ đều được chấp nhận. Nếu không có giá trị đó tồn tại, chúng ta in -1."
date: "2026-06-08T12:47:39+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "hashing", "implementation", "math", "schedules", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 2020
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 976 (Div. 2) and Divide By Zero 9.0"
rating: 1400
weight: 2020
solve_time_s: 130
verified: false
draft: false
---

[CF 2020C - Cân bằng bit](https://codeforces.com/problemset/problem/2020/C) 

**Đánh giá:** 1400 
**Thẻ:** bitmasks, băm, triển khai, toán học, lịch trình, tìm kiếm ternary 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên không âm`b`,`c`, Và`d`. Chúng ta phải xây dựng một số nguyên không âm`a`như vậy$$(a \mid b) - (a \& c) = d$$Ở đâu`|`là bitwise HOẶC và`&`là bit AND. 

Nhiệm vụ không phải là tối ưu hóa một số giá trị của`a`. Bất kỳ hợp lệ`a`được chấp nhận. Nếu không có giá trị như vậy tồn tại, chúng tôi in`-1`. 

Quan sát đầu tiên đến từ kích thước của các con số. Mỗi giá trị có thể lớn bằng$10^{18}$, có nghĩa là có tới khoảng 60 bit nhị phân có liên quan. Có thể có nhiều như$10^5$các trường hợp thử nghiệm, do đó, một giải pháp dành thậm chí vài nghìn thao tác cho mỗi trường hợp thử nghiệm là ổn, nhưng bất kỳ điều gì phụ thuộc vào độ lớn của bản thân các con số đều là không thể. 

Một tìm kiếm mạnh mẽ trên các giá trị có thể có của`a`ngay lập tức bị loại trừ. Phạm vi cho phép đạt$2^{61}$, đại khái là$2.3 \times 10^{18}$. 

Phần khó khăn là phép trừ. Nhiều bài toán theo bit có thể được giải độc lập từng bit một, nhưng phép trừ thường tạo ra sự vay mượn giữa các bit lân cận. Một giải pháp bất cẩn xử lý từng bit một cách độc lập mà không xem xét đến việc vay mượn nói chung sẽ không chính xác. 

Các trường hợp cạnh then chốt xuất phát từ sự kết hợp của các bit tạo ra mâu thuẫn. 

Coi như:```
b = 0, c = 0, d = 1
```Khi đó phương trình trở thành$$a - 0 = 1$$vậy câu trả lời là`a = 1`. 

Một giải pháp không tái tạo lại chính xác các bit của`a`sẽ bỏ lỡ trường hợp tầm thường này. 

Một trường hợp quan trọng khác là:```
b = 1, c = 0, d = 0
```Đối với bit 0,`(a|1)`luôn luôn là`1`, trong khi`(a&0)`luôn luôn là`0`. Vế trái bao giờ cũng ít nhất`1`, nên không có giải pháp nào tồn tại. Đầu ra đúng là`-1`. 

Nguồn lỗi thứ ba là quên rằng phép trừ có thể tạo ra khoản vay. 

Ví dụ:```
b = 0, c = 1, d = 1
```Nếu chúng ta chọn`a = 1`, sau đó 

(1|0) - (1&1) = 1 - 1 = 0 

không`1`. 

Mối quan hệ giữa các bit phụ thuộc vào khoản vay vào vị trí hiện tại, vì vậy chúng ta phải theo dõi nó một cách rõ ràng. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Hãy thử mọi giá trị có thể của`a`, đánh giá$$(a|b)-(a\&c)$$và kiểm tra xem nó có bằng không`d`. 

Điều này rõ ràng là đúng vì nó xem xét mọi ứng cử viên. Vấn đề là không gian tìm kiếm.`a`có thể lớn như$2^{61}$, vì vậy trường hợp xấu nhất đòi hỏi nhiều hơn$2 \times 10^{18}$đánh giá. Ngay cả với tốc độ một tỷ phép tính mỗi giây, điều đó sẽ mất hàng thập kỷ. 

Cấu trúc của biểu thức gợi ý cách tiếp cận theo bit. Cả OR và AND đều hoạt động độc lập trên mỗi bit. Hoạt động duy nhất ghép các vị trí bit khác nhau là phép trừ. 

Cho phép$$X = a|b$$Và$$Y = a\&c$$Phương trình trở thành$$X - Y = d.$$Khi thực hiện phép trừ nhị phân, thông tin duy nhất truyền từ bit này sang bit tiếp theo là thông tin mượn. Điều đó có nghĩa là chúng ta có thể xử lý các bit từ ít quan trọng nhất đến quan trọng nhất trong khi vẫn duy trì một trạng thái duy nhất: khoản vay đến. 

Đối với mỗi vị trí bit chúng ta biết: 

- một chút`b`- một chút`c`- một chút`d`- khoản vay hiện tại 

Điều chưa biết là một chút`a`. 

Đang thử cả hai khả năng (`0`hoặc`1`) là đủ. Đối với mỗi lựa chọn, chúng ta có thể tính toán các bit tương ứng của`X`Và`Y`, thực hiện một bước trừ nhị phân và kiểm tra xem bit được tạo có khớp với bit mục tiêu trong`d`. 

Điều này biến vấn đề thành một quá trình động nhỏ chỉ có hai trạng thái mượn,`0`Và`1`. Vì chúng tôi xử lý tối đa 61 bit nên công việc trên mỗi trường hợp thử nghiệm là không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{61})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(61)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`a = 0`Và`borrow = 0`. 
2. Xử lý vị trí bit từ`0`bởi vì`60`. 
3. Trích xuất các bit hiện tại:$$b_i,\ c_i,\ d_i$$từ`b`,`c`, Và`d`. 
4. Thử cả hai giá trị có thể có cho bit hiện tại của`a`, cụ thể là`0`Và`1`. 
5. Đối với một bit ứng cử viên`a_i`, tính:$$x_i = a_i \mid b_i$$Và$$y_i = a_i \& c_i$$6. Mô phỏng một bước trừ nhị phân:$$x_i - y_i - borrow$$Nếu giá trị không âm thì bit kết quả là giá trị đó và lần mượn tiếp theo là`0`. 

Nếu không thì thêm`2`đến giá trị và đặt khoản vay tiếp theo thành`1`. 
7. Kiểm tra xem bit kết quả có bằng không`d_i`. 

Nếu nó khớp, sự lựa chọn này của`a_i`phù hợp với phương trình. Lưu trữ bit trong câu trả lời, cập nhật khoản vay và tiếp tục đến vị trí tiếp theo. 
8. Nếu không`a_i = 0`cũng không`a_i = 1`tạo ra bit cần thiết, không có giải pháp nào tồn tại. đầu ra`-1`. 
9. Sau khi tất cả các bit được xử lý, hãy xác minh rằng khoản vay cuối cùng bằng 0. Số tiền vay còn lại có nghĩa là kết quả phép trừ không bằng`d`. 
10. Xuất ra giá trị được xây dựng của`a`. 

### Tại sao nó hoạt động 

Tại mỗi vị trí bit, phép trừ nhị phân chỉ phụ thuộc vào ba giá trị: bit hiện tại của số bị trừ, bit hiện tại của số bị trừ và bit mượn vào vị trí đó. 

Thuật toán liệt kê rõ ràng hai khả năng duy nhất cho bit chưa biết`a_i`. Bất cứ khi nào một lựa chọn tái tạo bit được yêu cầu của`d`, nó duy trì khả năng có một giải pháp toàn cục hợp lệ vì tất cả thông tin cần thiết ở các bit cao hơn được tóm tắt bằng khoản vay gửi đi. 

Khoản vay đóng vai trò là trạng thái hoàn chỉnh của quá trình trừ. Vì mỗi bit được xử lý khớp với bit tương ứng của`d`và số tiền vay cuối cùng bằng 0, số được xây dựng lại thỏa mãn$$(a|b)-(a\&c)=d.$$Nếu ở một vị trí nào đó không có sự lựa chọn`a_i`có thể tạo ra bit được yêu cầu thì không có sự tiếp tục hợp lệ nào tồn tại, do đó việc báo cáo`-1`là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        b, c, d = map(int, input().split())

        a = 0
        borrow = 0
        ok = True

        for i in range(61):
            bi = (b >> i) & 1
            ci = (c >> i) & 1
            di = (d >> i) & 1

            found = False

            for ai in (0, 1):
                xi = ai | bi
                yi = ai & ci

                cur = xi - yi - borrow

                if cur >= 0:
                    bit = cur
                    nb = 0
                else:
                    bit = cur + 2
                    nb = 1

                if bit == di:
                    found = True
                    borrow = nb

                    if ai:
                        a |= (1 << i)

                    break

            if not found:
                ok = False
                break

        if borrow:
            ok = False

        ans.append(str(a if ok else -1))

    sys.stdout.write("\n".join(ans))

solve()
```Giải pháp tuân theo quá trình trừ chính xác. 

Biến`borrow`lưu trữ khoản vay vào bit hiện tại. Đối với mỗi vị trí bit, chúng tôi kiểm tra cả hai giá trị có thể có của`a_i`. 

Các biểu thức```
xi = ai | bi
yi = ai & ci
```biểu diễn các bit tương ứng của`(a|b)`Và`(a&c)`. 

Bước trừ được thực hiện chính xác như số học nhị phân. Nếu giá trị cục bộ trở thành âm, chúng ta mượn bit tiếp theo, thêm`2`đến vị trí hiện tại và ghi lại khoản vay đi của`1`. 

Một điểm tinh tế là phạm vi xử lý. Bài toán đảm bảo rằng một câu trả lời hợp lệ nằm trong`[0, 2^61]`, vì vậy việc kiểm tra các bit`0..60`là đủ. Tất cả đầu vào đều ở bên dưới`2^60`và bất kỳ sự lan truyền mượn nào yêu cầu bit cao hơn đều được phát hiện bởi kết quả cuối cùng`borrow`kiểm tra. 

Một chi tiết tinh tế khác là chúng tôi ngay lập tức cam kết lựa chọn hợp lệ đầu tiên. Đối với mọi tiểu bang`(bit position, incoming borrow)`chỉ cần có nhiều nhất một phần tiếp theo. Khoản vay tóm tắt hoàn toàn tất cả các quyết định trước đó, do đó không cần phải quay lại bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
b = 2
c = 2
d = 2
```Biểu diễn nhị phân:```
b = 10
c = 10
d = 10
```| Chút | bi | ci | di | Vay vào | Được chọn ai | Vay | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 0 | 0 | 0 | 

Câu trả lời được xây dựng là:```
a = 0
```Kiểm tra:$$(0|2)-(0\&2)=2-0=2$$Ví dụ này cho thấy việc chọn`a_i = 0`đã có thể thỏa mãn phương trình mà không cần bất kỳ bit nào được đặt trong`a`. 

### Ví dụ 2 

đầu vào:```
b = 10
c = 2
d = 14
```Nhị phân:```
b = 1010
c = 0010
d = 1110
```| Chút | bi | ci | di | Vay vào | Được chọn ai | Vay | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 1 | 0 | 1 | 0 | 
| 3 | 1 | 0 | 1 | 0 | 1 | 0 | 

Giá trị được xây dựng là:```
a = 1100₂ = 12
```Xác minh:$$(12|10)=14$$

$$(12\&2)=0$$

$$14-0=14$$Dấu vết này cho thấy cách thuật toán xây dựng lại câu trả lời từng bit một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(61)$| Mỗi bit trong số 61 bit thử tối đa hai giá trị | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Vì 61 là một hằng số nên thời gian chạy thực tế là tuyến tính theo số lượng ca kiểm thử. Ngay cả với$10^5$các trường hợp thử nghiệm, tổng công việc chỉ có vài triệu thao tác cơ bản, dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        b, c, d = map(int, input().split())

        a = 0
        borrow = 0
        ok = True

        for i in range(61):
            bi = (b >> i) & 1
            ci = (c >> i) & 1
            di = (d >> i) & 1

            found = False

            for ai in (0, 1):
                xi = ai | bi
                yi = ai & ci

                cur = xi - yi - borrow

                if cur >= 0:
                    bit = cur
                    nb = 0
                else:
                    bit = cur + 2
                    nb = 1

                if bit == di:
                    found = True
                    borrow = nb

                    if ai:
                        a |= (1 << i)

                    break

            if not found:
                ok = False
                break

        if borrow:
            ok = False

        out.append(str(a if ok else -1))

    return "\n".join(out)

# provided samples
assert run("3\n2 2 2\n4 2 6\n10 2 14\n") == "0\n-1\n12"

# minimum values
assert run("1\n0 0 0\n") == "0"

# simple solvable case
assert run("1\n0 0 1\n") == "1"

# impossible case
assert run("1\n1 0 0\n") == "-1"

# large boundary value
assert run(f"1\n{(1<<60)-1} 0 {(1<<60)-1}\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`0`| Giá trị nhỏ nhất có thể | 
|`0 0 1`|`1`| Tái thiết một bit thiết lập | 
|`1 0 0`|`-1`| Phát hiện cấu hình không thể | 
|`(2^60-1) 0 (2^60-1)`|`0`| Xử lý đúng các giá trị rất lớn | 
| Đầu vào mẫu | Đầu ra mẫu | Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 0 0
`
```Tại bit 0 ta có:```
bi = 1
c
```
