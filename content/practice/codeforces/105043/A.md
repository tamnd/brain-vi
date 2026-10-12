---
title: "CF 105043A - \u041f\u0435\u0442\u044f \u0438 \u0441\u0442\u0440\u0430\u043d\u043d\u044b\u0435 \u0437\u0430\u043f\u0440\u043e\u0441\u044b"
description: "Chúng ta được cấp một số $n$. Hãy tưởng tượng viết tất cả các số nguyên từ $1$ đến $n$ trên một tờ giấy. Hai phép toán độc lập được thực hiện trên tập hợp này: mọi số chia hết cho 2 được đánh dấu bằng một màu và mọi số chia hết cho 3 được đánh dấu bằng một màu khác."
date: "2026-06-28T01:31:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105043
codeforces_index: "A"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u041d\u0422\u041e: \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0446\u0438\u043e\u043d\u043d\u0430\u044f \u0431\u0435\u0437\u043e\u043f\u0430\u0441\u043d\u043e\u0441\u0442\u044c. \u0421\u0435\u043a\u0446\u0438\u044f - \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0430"
rating: 0
weight: 105043
solve_time_s: 74
verified: false
draft: false
---

[CF 105043A - \u041f\u0435\u0442\u044f \u0438 \u0441\u0442\u0440\u0430\u043d\u043d\u044b\u0435 \u0437\u0430\u043f\u0440\u043e\u0441\u044b](https://codeforces.com/problemset/problem/105043/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n$. Hãy tưởng tượng viết tất cả các số nguyên từ$1$ĐẾN$n$trên một tờ giấy. Hai phép toán độc lập được thực hiện trên tập hợp này: mọi số chia hết cho 2 được đánh dấu bằng một màu và mọi số chia hết cho 3 được đánh dấu bằng một màu khác. Một số có thể nhận được một điểm, hai điểm hoặc không có điểm nào. 

Nhiệm vụ là đếm xem có bao nhiêu số nhận được đúng một điểm. Nói cách khác, chúng ta muốn số nguyên trong$[1, n]$chia hết cho 2 hoặc 3 nhưng không chia hết cho cả hai. 

Ràng buộc$n \le 10^9$ngay lập tức loại trừ mọi cách tiếp cận lặp lại trên tất cả các số. Quét tuyến tính sẽ yêu cầu tới một tỷ thao tác, tốc độ này quá chậm trong Python trong giới hạn Codeforces thông thường. Bất kỳ giải pháp khả thi nào cũng phải tính toán câu trả lời bằng cách sử dụng các tính chất số học về tính chia hết. 

Trường hợp cạnh tinh vi phát sinh khi một số chia hết cho cả 2 và 3. Ví dụ: tại$n = 6$, số 6 không nên được tính ngay cả khi nó chia hết cho cả 2 và 3. Một cách tiếp cận ngây thơ chỉ đơn giản là đếm bội số của 2 và bội số của 3 một cách độc lập sẽ vượt quá số đó trừ khi được sửa. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi lặp lại tất cả các số nguyên từ 1 đến$n$, kiểm tra xem mỗi số có chia hết cho 2 hay 3 hay không và đảm bảo nó không chia hết cho cả hai. Điều này đúng vì nó mã hóa trực tiếp điều kiện. Tuy nhiên, nó thực hiện$n$kiểm tra, điều này trở nên không khả thi khi$n$đạt tới$10^9$. 

Quan sát quan trọng là sự chia hết cho 2 và 3 tạo ra một cấu trúc tuần hoàn. Bội số của 2 xuất hiện mỗi 2 số, bội số của 3 xuất hiện mỗi 3 số và các số chia hết cho cả hai xuất hiện mọi$\mathrm{lcm}(2,3) = 6$. Điều này cho phép chúng ta thay thế phép lặp bằng cách đếm cấp số cộng. 

Ta tính ba đại lượng: bao nhiêu số chia hết cho 2, bao nhiêu số chia hết cho 3, bao nhiêu số chia hết cho cả hai. Sau đó, chúng tôi áp dụng loại trừ bao gồm để đếm những số chia hết cho chính xác một trong hai điều kiện. 

Biểu thức cuối cùng trở thành:$$\#(2 \text{ only}) + \#(3 \text{ only}) = (A + B - 2C)$$Ở đâu$A = \lfloor n/2 \rfloor$,$B = \lfloor n/3 \rfloor$, Và$C = \lfloor n/6 \rfloor$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$A = \lfloor n/2 \rfloor$, số các số nguyên chia hết cho 2 trong$[1, n]$. Điều này đếm tất cả các số sẽ nhận được màu đầu tiên. 
2. Tính toán$B = \lfloor n/3 \rfloor$, số các số nguyên chia hết cho 3 trong$[1, n]$. Điều này đếm tất cả các số sẽ nhận được màu thứ hai. 
3. Tính toán$C = \lfloor n/6 \rfloor$, số các số nguyên chia hết cho cả 2 và 3. Đây chính xác là những số đạt cả hai điểm, vì$\mathrm{lcm}(2,3) = 6$. 
4. Tính đáp án cuối cùng là$A + B - 2C$. Mỗi số chia hết cho cả hai được tính hai lần trong$A + B$, nhưng chúng tôi muốn loại trừ chúng hoàn toàn khỏi tập hợp “chính xác một điểm”, vì vậy chúng tôi trừ chúng hai lần. 

### Tại sao nó hoạt động 

Mỗi số nguyên trong$[1, n]$rơi vào đúng một trong bốn loại riêng biệt: không chia hết cho 2 và 3, chỉ chia hết cho 2, chỉ chia hết cho 3 hoặc chia hết cho cả hai. biểu hiện$A + B - 2C$gán giá trị 1 cho các phần tử trong danh mục “chỉ 2” và “chỉ 3” và giá trị 0 cho tất cả các phần tử khác. Căn chỉnh này đảm bảo tổng được tính toán khớp chính xác với số lượng có đúng một dấu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

A = n // 2
B = n // 3
C = n // 6

print(A + B - 2 * C)
```Mã trực tiếp thực hiện công thức bao gồm-loại trừ có nguồn gốc ở trên. Phép chia số nguyên an toàn vì chúng ta đang đếm các bội số đầy đủ. Việc tính toán có thời gian không đổi và chỉ sử dụng một vài biến số nguyên. 

Một lỗi phổ biến là quên trừ hai lần phần trùng nhau. Chỉ trừ một lần sẽ tính các số chia hết cho cả 2 và 3 như thể chúng đóng góp một điểm hợp lệ, nhưng chúng sẽ đóng góp bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 5$| Bước | A = ⌊n/2⌋ | B = ⌊n/3⌋ | C = ⌊n/6⌋ | Biểu hiện | 
| --- | --- | --- | --- | --- | 
| Giá trị | 2 | 1 | 0 | A + B - 2C = 3 | 

Các số là 1, 2, 3, 4, 5. Các số hợp lệ là 2 (chỉ chia hết cho 2), 3 (chỉ chia hết cho 3) và 4 (chỉ chia hết cho 2). Điều đó mang lại 3, phù hợp với tính toán. 

Điều này xác nhận rằng công thức đếm chính xác các số có đúng một điều kiện ước số được kích hoạt. 

### Ví dụ 2:$n = 8$| Bước | A = ⌊n/2⌋ | B = ⌊n/3⌋ | C = ⌊n/6⌋ | Biểu hiện | 
| --- | --- | --- | --- | --- | 
| Giá trị | 4 | 2 | 1 | A + B - 2C = 4 | 

Các số từ 1 đến 8. Các số hợp lệ là 2, 3, 4, 8. Số 6 bị loại vì nó được tính hai lần trong A và B nhưng được sửa lại bằng cách trừ đi 2C. 

Dấu vết cho thấy việc loại bỏ chồng chéo là cần thiết như thế nào để tránh đếm các số chia hết cho cả hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học cố định trên số nguyên | 
| Không gian |$O(1)$| Số lượng biến không đổi bất kể kích thước đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó tránh được việc lặp lại trong phạm vi lên tới$n$. Ngay cả đối với$n = 10^9$, chỉ có một số phép chia và phép cộng số nguyên được thực hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    A = n // 2
    B = n // 3
    C = n // 6
    return str(A + B - 2 * C)

def run(inp: str) -> str:
    return solve(inp)

# provided samples
assert run("5\n") == "3"
assert run("8\n") == "4"

# custom cases
assert run("1\n") == "0", "minimum size"
assert run("2\n") == "1", "only one multiple of 2"
assert run("3\n") == "1", "only one multiple of 3"
assert run("6\n") == "2", "boundary overlap case"
assert run("12\n") == "6", "regular periodic structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | ranh giới tối thiểu | 
| 2 | 1 | bội số nhỏ nhất của 2 | 
| 3 | 1 | bội số nhỏ nhất của 3 | 
| 6 | 2 | xử lý chồng chéo | 
| 12 | 6 | tính đúng đắn định kỳ | 

## Vỏ cạnh 

### Trường hợp$n = 1$đầu vào:```
1
```Đây$A = 0$,$B = 0$,$C = 0$. Thuật toán xuất ra 0. Số duy nhất hiện diện là 1, không chia hết cho 2 hay 3 nên không được tính. Công thức trả về đúng 0. 

### Trường hợp$n = 6$đầu vào:```
6
```Chúng tôi tính toán$A = 3$,$B = 2$,$C = 1$. Kết quả là$3 + 2 - 2 = 3$. Các số đóng góp là 2, 3 và 4. Số 6 bị loại vì nó thuộc lớp chồng chéo và bị loại bỏ hoàn toàn bằng cách trừ đi hai lần phần đóng góp của nó. Điều này xác nhận việc xử lý chính xác các bội số được chia sẻ.
