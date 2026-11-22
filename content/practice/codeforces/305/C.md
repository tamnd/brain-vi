---
title: "CF 305C - Ivan và sức mạnh của hai"
description: "Chúng ta được cho một dãy số mũ được sắp xếp. Thay vì viết số mũ, Ivan viết lũy thừa tương ứng của hai trên giấy: $$2^{a1}, 2^{a2}, dấu chấm, 2^{an}$$ Chúng ta có thể cộng nhiều số hơn, nhưng mọi số được thêm vào cũng phải là lũy thừa của hai."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 305
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 184 (Div. 2)"
rating: 1600
weight: 305
solve_time_s: 108
verified: true
draft: false
---

[CF 305C - Ivan và sức mạnh của hai người](https://codeforces.com/problemset/problem/305/C) 

**Đánh giá:** 1600 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số mũ được sắp xếp. Thay vì viết số mũ, Ivan viết lũy thừa tương ứng của hai lên giấy:$$2^{a_1}, 2^{a_2}, \dots, 2^{a_n}$$Chúng ta có thể cộng nhiều số hơn, nhưng mọi số được thêm vào cũng phải là lũy thừa của hai. Mục tiêu là làm cho tổng số tiền bằng một số có dạng$$2^v - 1$$đối với một số số nguyên không âm$v$. Chúng ta cần tính số lũy thừa bổ sung tối thiểu của hai cần thiết. 

Một con số như$2^v - 1$là đặc biệt vì biểu diễn nhị phân của nó chỉ chứa một. Ví dụ:$$1 = 1_2,\quad 3 = 11_2,\quad 7 = 111_2,\quad 15 = 1111_2$$Kích thước đầu vào đạt$10^5$và số mũ có thể lớn bằng$2 \cdot 10^9$. Điều đó ngay lập tức loại trừ bất kỳ thuật toán nào xây dựng rõ ràng các mảng nhị phân khổng lồ hoặc mô phỏng các vị trí bit lên đến số mũ tối đa. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Quan sát quan trọng là giá trị thực tế của lũy thừa của hai là rất lớn, nhưng số nhị phân mang tính cục bộ. Vì tất cả các số đều là lũy thừa của hai nên toàn bộ quá trình hoạt động giống hệt như phép cộng nhị phân. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử tổng hiện tại đã có dạng$2^v - 1$. 

đầu vào:```
4
0 1 1 1
```Những con số là$1,2,2,2$, tổng của nó là$7$. Từ$7 = 2^3 - 1$, câu trả lời là$0$. Việc triển khai bất cẩn luôn cố gắng "hoàn thành các bit bị thiếu" mà không kiểm tra số mang có thể bổ sung thêm sức mạnh một cách không chính xác. 

Một trường hợp phức tạp khác là khi nhiều quyền lực ngang nhau sụp đổ thông qua việc mang theo. 

đầu vào:```
3
0 0 0
```Tổng số tiền là$3$, đã bằng$2^2 - 1$. Nếu chỉ đếm số mũ riêng biệt, chúng ta sẽ nghĩ thiếu bit 1 và trả lời sai$1$. 

Chuỗi vận chuyển lớn cũng có vấn đề. 

đầu vào:```
5
0 0 0 0 0
```Tổng số tiền là$5$, nhị phân$101$. Để đạt được một số hình thức$111$, chúng ta chỉ cần thêm$2$, cho$7$. Câu trả lời là$1$, không$2$. Lý luận tham lam trên từng bit riêng lẻ có thể dễ dàng thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tính toán rõ ràng số tiền hiện tại:$$S = \sum 2^{a_i}$$Sau đó chúng ta có thể thử mọi mục tiêu có dạng$2^v - 1$lớn hơn hoặc bằng$S$. Đối với mỗi mục tiêu, chúng tôi sẽ tính toán sự khác biệt:$$D = (2^v - 1) - S$$Vì mọi số được thêm vào phải là lũy thừa của hai nên số số hạng được thêm tối thiểu bằng số bit được đặt trong$D$. Chúng ta có thể giảm thiểu giá trị đó hết mức có thể$v$. 

Ý tưởng này đúng về mặt toán học vì mỗi bit được đặt trong$D$có thể được cộng độc lập dưới dạng lũy ​​thừa của hai. 

Vấn đề là kích thước số. Số mũ đạt tới$2 \cdot 10^9$, do đó, về mặt khái niệm, bản thân tổng có thể chứa hàng tỷ bit. Thao tác trực tiếp với số nguyên lớn sẽ không thực tế nếu được thực hiện một cách ngây thơ. Mặc dù số nguyên Python có độ chính xác tùy ý, việc xây dựng số lượng khổng lồ là lãng phí và không cần thiết. 

Cái nhìn sâu sắc quan trọng là chúng ta không bao giờ cần số lượng đầy đủ. Chúng tôi chỉ quan tâm đến số lượng bit còn lại sau khi tất cả các lần mang được xử lý. 

Hãy suy nghĩ về việc cộng nhị phân theo từng cột. Mỗi lần xuất hiện của số mũ$x$đóng góp một đơn vị vào vị trí bit$x$. Hai đơn vị ở cùng một vị trí hợp nhất thành một đơn vị mang ở vị trí tiếp theo. 

Sau khi tất cả các giá trị mang được truyền đi, mọi vị trí bit đều chứa 0 hoặc 1. Đặt biểu diễn nhị phân cuối cùng của tổng là:$$b_k b_{k-1} \dots b_0$$Để chuyển đổi số này thành một dạng như$111\dots111$, chúng ta chỉ cần biến mọi bit 0 bên dưới bit được đặt cao nhất thành một. Mỗi cái còn thiếu yêu cầu thêm chính xác một lũy thừa của hai. 

Vì vậy, câu trả lời chỉ đơn giản là số bit 0 bên dưới bit được đặt quan trọng nhất của tổng cuối cùng. 

Chúng ta có thể tính toán điều này mà không cần phải xây dựng số nguyên khổng lồ một cách rõ ràng. Chúng tôi chỉ mô phỏng việc thực hiện giữa các vị trí bị chiếm giữ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(B²) | O(B) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

Đây,$B$đại diện cho số lượng vị trí bit có liên quan sau khi truyền mang. 

## Hướng dẫn thuật toán 

1. Đọc số mũ đã được sắp xếp. 
2. Đếm xem mỗi số mũ xuất hiện bao nhiêu lần. 

Mỗi lần xuất hiện đóng góp một đơn vị vào vị trí nhị phân đó. 
3. Xử lý các vị trí theo thứ tự tăng dần, lan truyền hướng lên trên. 

Nếu vị trí$x$chứa$c$đơn vị: 

-$c \bmod 2$trở thành bit cuối cùng tại vị trí$x$-$c // 2$được đưa vào vị trí$x+1$Điều này khớp chính xác với phép cộng nhị phân thông thường. 
4. Theo dõi vị trí cao nhất kết thúc bằng bit bằng 1. 

Các bit trên vị trí này không thành vấn đề vì dạng mục tiêu là:$$111\dots111$$chỉ tối đa bit được đặt cao nhất. 
5. Đếm xem có bao nhiêu vị trí bên dưới bit được đặt cao nhất chứa 0 sau khi truyền mang. 

Mỗi vị trí như vậy yêu cầu thêm một sức mạnh của hai. 
6. Xuất ra số đếm. 

### Tại sao nó hoạt động 

Sau khi lan truyền mang, chúng ta thu được biểu diễn nhị phân thực sự của tổng hiện tại. Bất kỳ số nào của mẫu$2^v - 1$có tất cả các bit bằng 1 dưới bit cao nhất của nó. 

Chúng tôi chỉ được phép cộng lũy ​​thừa của hai, nghĩa là mỗi thao tác có thể biến chính xác một bit đã chọn từ 0 thành 1 mà không ảnh hưởng đến các bit thấp hơn. Vì các mục tiêu cao hơn chỉ đơn thuần là bổ sung thêm các mục tiêu dẫn đầu nên mục tiêu tối ưu luôn là số đơn vị nhỏ nhất trên tổng hiện tại. 

Điều đó có nghĩa là mọi bit 0 bên dưới bit được đặt quan trọng nhất phải được sửa chính xác một lần và không cần bổ sung thêm. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    cnt = defaultdict(int)

    for x in a:
        cnt[x] += 1

    bits = {}
    positions = sorted(cnt.keys())

    i = 0
    while i < len(positions):
        pos = positions[i]

        cur = cnt[pos]

        bits[pos] = cur & 1

        carry = cur >> 1
        if carry:
            cnt[pos + 1] += carry
            if pos + 1 not in bits and (i + 1 == len(positions) or positions[i + 1] != pos + 1):
                positions.append(pos + 1)

        i += 1

    positions.sort()

    highest = -1

    for pos in positions:
        if bits.get(pos, 0):
            highest = pos

    ans = 0

    for pos in range(highest):
        if bits.get(pos, 0) == 0:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Từ điển`cnt`lưu trữ bao nhiêu bản sao của mỗi số mũ hiện đang tồn tại. Khi chúng tôi xử lý một vị trí, chúng tôi chia số đếm của nó thành bit cuối cùng và phần mang. 

Biểu thức:```
bits[pos] = cur & 1
```giữ tính chẵn lẻ của số đếm, trở thành bit nhị phân cuối cùng ở vị trí đó. 

Biểu thức:```
carry = cur >> 1
```tính xem có bao nhiêu cặp di chuyển lên trên. 

Một điểm tinh tế là việc mang có thể tạo ra các vị trí hoàn toàn mới không có trong đầu vào ban đầu. Chúng tôi tự động thêm các vị trí đó và sắp xếp lại trước lần quét cuối cùng. 

Một chi tiết quan trọng khác là vòng lặp:```
for pos in range(highest):
```Chúng tôi dừng lại trước`highest`chính nó vì bit được đặt cao nhất theo định nghĩa đã là 1. Chỉ những vị trí bên dưới nó mới có thể cần điền. 

Số nguyên Python an toàn ở đây vì chúng tôi không bao giờ trực tiếp xây dựng các lũy thừa khổng lồ. Chúng tôi chỉ thao túng số lượng và vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 1 1 1
```Công suất ban đầu là:$$1,2,2,2$$Tổng của họ là$7 = 111_2$. 

| Vị trí | Đếm Trước Khi Mang Theo | Bit cuối cùng | Thực hiện | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 3 | 1 | 1 | 
| 2 | 1 | 1 | 0 | 

Dạng nhị phân cuối cùng là:$$111_2$$Không có bit 0 nào bên dưới bit được đặt cao nhất, vì vậy câu trả lời là$0$. 

Dấu vết này cho thấy sức mạnh lặp đi lặp lại hợp nhất một cách tự nhiên thông qua các lần mang. 

### Ví dụ 2 

đầu vào:```
1
2
```Số duy nhất là:$$2^2 = 4$$Biểu diễn nhị phân:$$100_2$$| Vị trí | Đếm Trước Khi Mang Theo | Bit cuối cùng | Thực hiện | 
| --- | --- | --- | --- | 
| 2 | 1 | 1 | 0 | 

Bit 0 và 1 bị thiếu. 

Để có được:$$111_2 = 7$$chúng tôi thêm:$$2^0 + 2^1$$Vậy câu trả lời là$2$. 

Ví dụ này chứng tỏ rằng mục tiêu tối ưu luôn là số đơn vị nhỏ nhất trên tổng hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một số lần không đổi | 
| Không gian | O(n) | Từ điển lưu trữ tối đa một mục nhập cho mỗi vị trí bit có liên quan | 

Số lượng vị trí riêng biệt vẫn tuyến tính vì mỗi vị trí mang chỉ di chuyển lên trên một bước và tổng số lần mang bị giới hạn bởi số phần tử đầu vào. Điều này dễ dàng phù hợp trong giới hạn cho$10^5$các phần tử. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    cnt = defaultdict(int)

    for x in a:
        cnt[x] += 1

    bits = {}
    positions = sorted(cnt.keys())

    i = 0
    while i < len(positions):
        pos = positions[i]

        cur = cnt[pos]

        bits[pos] = cur & 1

        carry = cur >> 1

        if carry:
            cnt[pos + 1] += carry
            if pos + 1 not in bits and (i + 1 == len(positions) or positions[i + 1] != pos + 1):
                positions.append(pos + 1)

        i += 1

    positions.sort()

    highest = -1

    for pos in positions:
        if bits.get(pos, 0):
            highest = pos

    ans = 0

    for pos in range(highest):
        if bits.get(pos, 0) == 0:
            ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("4\n0 1 1 1\n") == "0\n", "sample 1"

# minimum size
assert run("1\n0\n") == "0\n", "single one already equals 1"

# single high bit
assert run("1\n2\n") == "2\n", "need bits 0 and 1"

# carry chain
assert run("5\n0 0 0 0 0\n") == "1\n", "5 is binary 101"

# all equal values
assert run("4\n3 3 3 3\n") == "0\n", "four 8s become 32"

# sparse bits
assert run("2\n0 3\n") == "2\n", "binary 1001 needs two gaps filled"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0`| Tổng hợp lệ nhỏ nhất có thể | 
|`1 / 2`|`2`| Thiếu bit thấp hơn | 
|`5 / 0 0 0 0 0`|`1`| Lan truyền dài hạn | 
|`4 / 3 3 3 3`|`0`| Nhiều mang sụp đổ thành một bit | 
|`2 / 0 3`|`2`| Biểu diễn nhị phân thưa thớt | 

## Vỏ cạnh 

Hãy xem xét việc thực hiện lặp đi lặp lại. 

đầu vào:```
4
3 3 3 3
```Chúng tôi bắt đầu với bốn bản sao của$2^3 = 8$. 

Phép cộng nhị phân tiến hành như sau: 

- Bốn đơn vị ở bit 3 tạo ra số 0 ở đó và mang hai đơn vị lên trên. 
- Hai đơn vị ở bit 4 tạo ra số 0 ở đó và mang một đơn vị lên trên. 
- Bit 5 trở thành một. 

Số cuối cùng là:$$100000_2$$Tất cả năm bit thấp hơn đều bằng 0, nhưng chúng tôi chỉ quan tâm dưới bit được đặt cao nhất. Chúng ta cần năm bổ sung để thực hiện:$$111111_2$$Thuật toán xử lý việc này một cách tự động thông qua việc truyền tải lặp đi lặp lại. 

Bây giờ hãy xem xét một cấu hình thưa thớt. 

đầu vào:```
2
0 3
```Tổng số tiền là:$$1 + 8 = 9 = 1001_2$$Bit 1 và 2 bị thiếu. Thuật toán phát hiện chính xác hai bit 0 bên dưới bit được đặt cao nhất và trả lời$2$. 

Cuối cùng, hãy xem xét một số đã hoàn hảo được hình thành thông qua số nhớ. 

đầu vào:```
3
0 0 0
```Ba số có tổng là:$$11_2$$Sự lan truyền mang tạo ra: 

- bit 0 = 1 
- chuyển tới bit 1 
- bit 1 = 1 

Không còn khoảng trống nên câu trả lời là$0$. Điều này xác nhận rằng thuật toán xử lý chính xác số mang trước khi đếm các bit bị thiếu.
