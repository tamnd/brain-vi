---
title: "CF 106088A - \u041f\u0440\u043e\u0441\u043c\u043e\u0442\u0440 \u0441\u0435\u0440\u0438\u0430\u043b\u0430"
description: "Chúng ta có một chuỗi các tập có thời lượng t1, t2, ..., tn. Kolya xem chúng theo thứ tự và dành chính xác m đơn vị thời gian mỗi ngày, ngoại trừ có thể vào ngày cuối cùng khi loạt phim có thể kết thúc trước khi anh ấy đạt được m."
date: "2026-06-19T21:51:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106088
codeforces_index: "A"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u0432\u0442\u043e\u0440\u043e\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 106088
solve_time_s: 50
verified: true
draft: false
---

[CF 106088A - \u041f\u0440\u043e\u0441\u043c\u043e\u0442\u0440 \u0441\u0435\u0440\u0438\u0430\u043b\u0430](https://codeforces.com/problemset/problem/106088/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi các tập có thời lượng`t1, t2, ..., tn`. Kolya xem chúng theo thứ tự và chi tiêu chính xác`m`đơn vị thời gian mỗi ngày, ngoại trừ có thể vào ngày cuối cùng khi chuỗi thời gian có thể kết thúc trước khi anh ta đến nơi.`m`. 

Một ngày được gọi là thành công nếu phiên xem có độ dài chính xác`m`kết thúc ở phần cuối chính xác của một số tập phim. Nếu buổi tập kết thúc vào giữa một tập thì ngày đó không thành công. Ngày cuối cùng thật đặc biệt: nếu ít hơn`m`số lượt xem vẫn còn, ngày đó không được tính ngay cả khi bộ phim kết thúc đúng vào cuối tập. 

Đầu vào cung cấp thời lượng tập và giới hạn xem hàng ngày. Chúng ta phải đếm xem có bao nhiêu ngày thành công. 

Quan sát quan trọng là tiến trình xem chỉ phụ thuộc vào tổng thời gian đã xem. Sau một ngày Kolya đã xem`m`đơn vị, sau hai ngày trọn vẹn`2m`, sau ba ngày trọn vẹn`3m`, vân vân. Một ngày được coi là thành công khi một trong những thời gian xem tích lũy này trùng với thời điểm kết thúc một tập phim. 

Những hạn chế là nhỏ về mặt`n`, chỉ tối đa 1000, nhưng độ dài tập và`m`có thể đạt được`10^9`. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào nâng cao từng đơn vị thời gian tại một thời điểm. Chúng ta cần một cách tiếp cận dựa trên tổng tích lũy thay vì đơn vị thời gian riêng lẻ. 

Một điểm tinh tế là việc điều trị một phần ngày cuối cùng. Giả sử tổng thời lượng của tất cả các tập là`S`. Ngày xem đầy đủ chỉ tương ứng với thời gian`m, 2m, 3m, ...`hoàn toàn nằm trong quá trình xem. Nếu một tập phim kết thúc vào thời điểm`S`Và`S`không đạt được sau một ngày dài`m`, nó không được tính. 

Coi như:```
1 5
3
```Toàn bộ series chỉ kéo dài 3 đơn vị. Kolya xem nó chỉ trong một phần ngày. Câu trả lời đúng là`0`, không`1`. 

Một sai lầm dễ mắc phải khác là tính số kết thúc của các tập xảy ra trong một ngày thay vì chính xác ở ranh giới của ngày đó. 

Ví dụ:```
2 3
1 2
```Tập đầu tiên kết thúc sau 1 đơn vị, nhưng ngày đầu tiên kết thúc sau 3 đơn vị. Câu trả lời đúng là`1`vì ranh giới ngày ở thời điểm thứ 3 trùng với cuối tập 2. Đếm hết từng tập sẽ sai. 

Trường hợp cạnh thứ ba xảy ra khi tổng thời lượng là bội số chính xác của`m`. 

Ví dụ:```
2 2
1 3
```Tổng thời lượng là 4. Có hai ngày trọn vẹn. Ngày 1 kết thúc ở thời điểm 2, tức là ở giữa tập 2. Ngày 2 kết thúc ở thời điểm 4, đúng vào lúc kết thúc bộ phim và cũng là cuối tập. Câu trả lời là`1`. 

Ngày cuối cùng vẫn được tính vì Kolya đã xem chính xác`m`đơn vị vào ngày hôm đó. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng cách nhìn của Kolya hàng ngày. Mỗi ngày, chúng tôi sẽ xác định nơi anh ấy dừng lại, có thể chuyển qua một số tập. Từ`n`chỉ là 1000, thậm chí liên tục thăng tiến qua các tập phim cũng có thể chấp nhận được. Chúng tôi có thể duy trì tập hiện tại và vị trí bên trong nó và mô phỏng hàng ngày cho đến khi bộ truyện kết thúc. 

Mô phỏng lực lượng vũ phu là chính xác vì nó tuân theo chính xác quá trình xem. Vấn đề là nó phức tạp không cần thiết. Trạng thái thực tế của quá trình được xác định hoàn toàn bởi tổng thời gian đã xem. 

Quan sát quan trọng là phần kết thúc của tập phim xảy ra ở tổng thời lượng tích lũy của tập phim. Nếu chúng ta định nghĩa```
pref[i] = t1 + t2 + ... + ti
```rồi tập phim`i`kết thúc đúng lúc`pref[i]`. 

Một ngày thành công xảy ra khi thời điểm kết thúc một ngày xem trọn vẹn trùng với thời điểm kết thúc một tập phim. Ngày xem đầy đủ đôi khi kết thúc```
m, 2m, 3m, ...
```lên đến tổng thời lượng của loạt phim. 

Vì vậy, chúng ta chỉ cần đếm thời gian kết thúc tập là bội số của`m`. 

Còn một điều kiện nữa. Phần ngày cuối cùng không được tính. Một tập phim kết thúc vào thời điểm`pref[i]`chỉ đóng góp nếu đạt được mục tiêu vào cuối ngày. Điều đó có nghĩa`pref[i]`phải chia hết cho`m`. 

Không có gì khác quan trọng. Chúng tôi không cần mô phỏng các đoạn tạm dừng hoặc chuyển tiếp tập phim. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n + số ngày) | O(1) | Được chấp nhận nhưng phức tạp không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Khởi tạo thời lượng tích lũy đang chạy`prefix = 0`. 
3. Khởi tạo câu trả lời`ans = 0`. 
4. Xử lý các tập theo thứ tự. 
5. Thêm thời lượng tập hiện tại vào`prefix`. 
6. Kiểm tra xem`prefix % m == 0`. 

Nếu thời gian tích lũy chia hết cho`m`, thì phần cuối của tập này trùng với thời điểm kết thúc một ngày xem trọn vẹn nào đó. 
7. Nếu điều kiện đúng thì tăng`ans`. 
8. Sau khi tất cả các tập được xử lý, xuất ra`ans`. 

Tại sao điều này tự động xử lý phần ngày cuối cùng? Bởi vì một phần ngày không bao giờ kết thúc vào thời điểm là bội số của`m`. Chỉ số ngày xem đầy đủ mới tương ứng với thời gian đã xem tích lũy`k·m`. Do đó, đếm phần kết thúc của tập ở bội số của`m`đếm chính xác những ngày thành công và bỏ qua những ngày chưa hoàn thành cuối cùng. 

### Tại sao nó hoạt động 

hãy để`pref[i]`là thời điểm tập phim`i`kết thúc. 

Sau đó`k`xem đầy đủ ngày, Kolya đã xem chính xác`k·m`đơn vị thời gian. Một ngày thành công khi thời điểm kết thúc của nó trùng với thời điểm kết thúc một tập phim. Điều đó có nghĩa là phải tồn tại một tập`i`như vậy```
pref[i] = k·m
```đối với một số nguyên`k`. 

Điều kiện này tương đương với```
pref[i] % m = 0
```Mỗi ngày thành công tương ứng với đúng một thời gian kết thúc tập phim chia hết cho`m`và mỗi thời điểm kết thúc tập phim như vậy tương ứng với một ngày thành công. Thuật toán tính chính xác những khoảnh khắc này nên kết quả là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
t = list(map(int, input().split()))

prefix = 0
ans = 0

for x in t:
    prefix += x
    if prefix % m == 0:
        ans += 1

print(ans)
```Giải pháp duy trì thời lượng tích lũy của tất cả các tập được xử lý cho đến nay. 

Bất cứ khi nào thời lượng tích lũy chia hết cho`m`, điều đó có nghĩa là tập hiện tại kết thúc chính xác vào cuối một ngày xem đầy đủ nào đó. Chúng tôi tăng câu trả lời ngay lập tức. 

Không có cấu trúc dữ liệu bổ sung được yêu cầu. Tổng tích lũy có thể lớn tới mức`1000 × 10^9 = 10^12`, dễ dàng phù hợp với số nguyên Python. 

Một lỗi triển khai phổ biến là cố gắng lý giải về từng ngày và vị trí tập. Điều kiện chia hết đã nắm bắt được toàn bộ quá trình. Một sai lầm khác là lo lắng về nửa ngày cuối cùng. Vì chỉ có bội số của`m`được tính, những ngày không đầy đủ sẽ tự động bị loại trừ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
1 1 1 1
```| Tập | Thời lượng | Tiền tố Tổng | Tiền tố % 2 | Đếm Ngày Thành Công | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 0 | 1 | 
| 3 | 1 | 3 | 1 | 1 | 
| 4 | 1 | 4 | 0 | 2 | 

Đầu ra:```
2
```Tập 2 kết thúc ở thời điểm 2 và tập 4 kết thúc ở thời điểm 4. Cả hai thời điểm đều là bội số chính xác của thời gian xem hàng ngày nên cả hai đều tương ứng với những ngày thành công. 

### Mẫu 2 

đầu vào:```
4 3
1 1 2 1
```| Tập | Thời lượng | Tiền tố Tổng | Tiền tố % 3 | Đếm Ngày Thành Công | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 2 | 0 | 
| 3 | 2 | 4 | 1 | 0 | 
| 4 | 1 | 5 | 2 | 0 | 

Đầu ra:```
0
```Không có thời gian kết thúc tập nào chia hết cho 3. Ngày đầu tiên kết thúc ở giữa tập 3 và thời gian xem còn lại tạo thành một phần ngày cuối cùng, không được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần xem qua tất cả các tập | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`n ≤ 1000`, điều này chạy thoải mái trong giới hạn. Thuật toán chỉ thực hiện các phép tính số học đơn giản và sử dụng bộ nhớ bổ sung không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())
    t = list(map(int, input().split()))

    prefix = 0
    ans = 0

    for x in t:
        prefix += x
        if prefix % m == 0:
            ans += 1

    return str(ans)

# provided samples
assert run("4 2\n1 1 1 1\n") == "2", "sample 1"
assert run("4 3\n1 1 2 1\n") == "0", "sample 2"

# minimum size
assert run("1 1\n1\n") == "1", "single episode exactly one full day"

# partial final day
assert run("1 5\n3\n") == "0", "only partial day"

# all equal values
assert run("5 2\n2 2 2 2 2\n") == "5", "every episode end is successful"

# off-by-one style check
assert run("2 2\n1 3\n") == "1", "only final episode end matches"

# large values
assert run("3 1000000000\n1000000000 1000000000 1000000000\n") == "3", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`1 5 / 3`|`0`| Một phần ngày cuối cùng không được tính | 
|`5 2 / 2 2 2 2 2`|`5`| Mỗi phần cuối của tập trùng khớp với một ranh giới trong ngày | 
|`2 2 / 1 3`|`1`| Xử lý đúng đắn khi chỉ có tập cuối đủ điều kiện | 
| Lớn`10^9`giá trị |`3`| Số học số lớn | 

## Vỏ cạnh 

Xem xét đầu vào```
1 5
3
```Thời gian kết thúc tập tích lũy là 3. Vì`3 % 5 != 0`, thuật toán không đếm nó và trả về`0`. Điều này phù hợp với quy luật rằng một phần ngày cuối cùng không thành công. 

Bây giờ hãy xem xét```
2 3
1 2
```Tổng tích lũy là 1 và 3. Chỉ có 3 chia hết cho 3 nên đáp án là`1`. 

Dấu vết:```
prefix = 1 -> not counted
prefix = 3 -> counted
```Thuật toán tính ranh giới ngày chứ không chỉ tính sự tồn tại của phần kết của tập. 

Cuối cùng, hãy xem xét```
2 2
1 3
```Tổng tích lũy là 1 và 4.```
1 % 2 = 1
4 % 2 = 0
```Chỉ có tập thứ hai đóng góp, tạo ra câu trả lời`1`. Ngày đầu tiên kết thúc trong tập 2, trong khi ngày thứ hai kết thúc đúng lúc bộ phim kết thúc. Thuật toán tự động nắm bắt sự khác biệt này thông qua việc chia hết thời gian kết thúc tập tích lũy.
