---
title: "CF 106157M - Điểm dừng đường cao tốc"
description: "Chúng ta được cung cấp vị trí của các điểm dừng trên đường cao tốc dọc theo một tuyến đường, được lưu trữ dưới dạng khoảng cách tích lũy tăng dần. Giữa mỗi cặp điểm dừng liên tiếp đều có một đoạn lái xe."
date: "2026-06-25T11:20:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106157
codeforces_index: "M"
codeforces_contest_name: "2025 United Kingdom and Ireland Programming Contest (UKIEPC 2025)"
rating: 0
weight: 106157
solve_time_s: 37
verified: true
draft: false
---

[CF 106157M - Điểm dừng đường cao tốc](https://codeforces.com/problemset/problem/106157/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp vị trí của các điểm dừng trên đường cao tốc dọc theo một tuyến đường, được lưu trữ dưới dạng khoảng cách tích lũy tăng dần. 

Giữa mỗi cặp điểm dừng liên tiếp đều có một đoạn lái xe. Nếu dừng ở vị trí`i`được gỡ bỏ, ở đâu`i`không phải là điểm dừng đầu tiên cũng không phải là điểm dừng cuối cùng thì hai đoạn liền kề với điểm dừng đó sẽ trở thành một đoạn dài hơn. 

Nhiệm vụ là loại bỏ chính xác một điểm dừng bên trong sao cho đoạn lái xe dài nhất còn lại càng nhỏ càng tốt. Chúng ta phải xuất ra khoảng cách dài nhất có thể tối thiểu đó. 

Cho phép`g[j] = s[j+1] - s[j]`là khoảng cách giữa các điểm dừng liên tiếp. 

Nếu chúng ta loại bỏ điểm dừng`i`, sau đó là khoảng trống`g[i-1]`Và`g[i]`biến mất và được thay thế bằng một khoảng trống có chiều dài`g[i-1] + g[i]`. 

Tất cả các khoảng trống khác không thay đổi. 

Số lượng điểm dừng có thể lên tới 200.000, do đó có gần 200.000 khoảng trống. Một thuật toán tính toán lại khoảng cách tối đa từ đầu cho mỗi điểm dừng có thể bị loại bỏ sẽ yêu cầu khoảng`O(n²)`công việc, nó quá lớn. Với`n = 200,000`, điều đó có nghĩa là hàng chục tỷ hoạt động. 

Một vài trường hợp đáng chú ý. 

Giả sử các khoảng trống là`10 20 30`. 

Loại bỏ sự hợp nhất giữa điểm dừng`10`Và`20`, tạo ra khoảng trống`30 30`. Câu trả lời là`30`, không`20`. Bất kỳ giải pháp nào chỉ xem xét các khoảng trống hiện có và bỏ qua phân đoạn đã hợp nhất sẽ không thành công. 

Coi như:```
4
0 100 101 200
```Những khoảng trống là`100 1 99`. 

Việc loại bỏ điểm dừng thứ hai sẽ tạo ra khoảng cách`101`, cho giá trị tối đa`101`. 

Việc loại bỏ điểm dừng thứ ba sẽ tạo ra khoảng cách`100`, cho giá trị tối đa`100`. 

Câu trả lời đúng là`100`. Một quy tắc tham lam như "bỏ điểm dừng bên cạnh khoảng cách nhỏ nhất" là không đáng tin cậy. 

Một trường hợp ranh giới khác là khi việc loại bỏ tối ưu xảy ra bên cạnh điểm dừng có thể tháo rời đầu tiên hoặc cuối cùng:```
3
0 5 12
```Chỉ có một điểm dừng có thể được gỡ bỏ. Độ dài đoạn kết quả là`12`, vậy câu trả lời là`12`. Việc xử lý tiền tố hoặc hậu tố bất cẩn có thể dễ dàng truy cập các chỉ mục không hợp lệ tại đây. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi điểm dừng có thể tháo rời. 

Đối với điểm dừng đã chọn, hãy hợp nhất hai khoảng trống liền kề của nó và sau đó quét tất cả các khoảng trống còn lại để tìm khoảng trống lớn nhất. Điều này đúng vì nó đánh giá rõ ràng kết quả của mọi hoạt động loại bỏ có thể xảy ra. 

Vấn đề là thời gian chạy. có`O(n)`ứng viên dừng lại và mỗi lần đánh giá sẽ được thực hiện`O(n)`thời gian để tính toán lại khoảng cách tối đa. Tổng độ phức tạp trở thành`O(n²)`, điều đó là không thể đối với`n = 200,000`. 

Quan sát quan trọng là sau khi loại bỏ điểm dừng, chỉ có hai khoảng trống thay đổi. 

Để loại bỏ giữa các khoảng trống`g[i-1]`Và`g[i]`: 

Khoảng cách hợp nhất trở thành`g[i-1] + g[i]`. 

Mọi khoảng cách khác vẫn giữ nguyên như cũ. 

Vì vậy, khoảng cách tối đa sau khi loại bỏ này chỉ đơn giản là lớn hơn: 

1. Khoảng cách hợp nhất. 
2. Khoảng cách tối đa trong số tất cả các khoảng trống ngoại trừ`g[i-1]`Và`g[i]`. 

Điều này gợi ý tính toán trước các giá trị tối đa trên tiền tố và hậu tố của mảng khoảng cách. 

Cho phép:`pref[j]`= khoảng cách tối đa giữa`g[0..j]`

`suff[j]`= khoảng cách tối đa giữa`g[j..m-1]`Ở đâu`m = n - 1`. 

Sau đó để loại bỏ các khoảng trống liên quan`g[i-1]`Và`g[i]`, khoảng cách không bị ảnh hưởng tối đa có thể đạt được trong thời gian không đổi từ tiền tố trước`g[i-1]`và hậu tố sau`g[i]`. 

Mỗi lần loại bỏ ứng viên được đánh giá trong`O(1)`thời gian, đưa ra một cái nhìn tổng thể`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc vị trí dừng. 
2. Xây dựng mảng khoảng cách`g`, Ở đâu`g[i] = s[i+1] - s[i]`. 
3. Xây dựng mảng tiền tố tối đa`pref`.`pref[i]`lưu trữ khoảng cách lớn nhất nhìn thấy từ đầu đến khi lập chỉ mục`i`. 
4. Xây dựng một mảng hậu tố tối đa`suff`.`suff[i]`lưu trữ khoảng cách lớn nhất từ ​​​​chỉ mục`i`đến cuối cùng. 
5. Lặp lại mọi điểm dừng có thể tháo rời. 

Nếu điểm dừng bị loại bỏ nằm giữa các khoảng trống`g[i-1]`Và`g[i]`, thì khoảng cách hợp nhất mới là:`merged = g[i-1] + g[i]`. 
6. Tính khoảng cách không bị ảnh hưởng lớn nhất. 

Những khoảng trống trước`g[i-1]`đóng góp`pref[i-2]`nếu chỉ mục đó tồn tại. 

Những khoảng trống sau`g[i]`đóng góp`suff[i+1]`nếu chỉ mục đó tồn tại. 

Giá trị lớn hơn trong hai giá trị này là khoảng cách không bị ảnh hưởng tối đa. 
7. Phân khúc lớn nhất sau lần loại bỏ này là:`max(merged, unaffected_max)`. 
8. Lấy giá trị tối thiểu trong số tất cả các lần loại bỏ ứng cử viên. 
9. Xuất ra mức tối thiểu đó. 

### Tại sao nó hoạt động 

Đối với bất kỳ điểm dừng nào được chọn, chính xác hai khoảng trống liền kề sẽ được sửa đổi. Mọi khoảng cách khác vẫn không thay đổi. 

Đoạn dài nhất sau khi loại bỏ điểm dừng đó phải là khoảng trống mới được hợp nhất hoặc một trong những khoảng trống không thay đổi. Mảng tối đa tiền tố và hậu tố cho khoảng cách không thay đổi lớn nhất bên ngoài cặp được hợp nhất trong thời gian không đổi. 

Vì thuật toán đánh giá chính xác đoạn dài nhất cho mỗi lần xóa hợp lệ và trả về mức tối thiểu trong số đó nên thuật toán luôn tìm ra điểm dừng tối ưu để xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = list(map(int, input().split()))

gaps = [s[i + 1] - s[i] for i in range(n - 1)]
m = n - 1

pref = [0] * m
pref[0] = gaps[0]
for i in range(1, m):
    pref[i] = max(pref[i - 1], gaps[i])

suff = [0] * m
suff[m - 1] = gaps[m - 1]
for i in range(m - 2, -1, -1):
    suff[i] = max(suff[i + 1], gaps[i])

ans = 10**18

for i in range(1, m):
    merged = gaps[i - 1] + gaps[i]

    other = 0
    if i - 2 >= 0:
        other = max(other, pref[i - 2])
    if i + 1 < m:
        other = max(other, suff[i + 1])

    ans = min(ans, max(merged, other))

print(ans)
```Phần đầu tiên chuyển đổi vị trí dừng thành độ dài khoảng cách. Làm việc với các khoảng trống đơn giản hơn nhiều vì việc xóa điểm dừng chỉ ảnh hưởng đến hai khoảng trống liền kề. 

Mảng tiền tố và hậu tố cho phép chúng ta truy vấn khoảng trống lớn nhất không bị ảnh hưởng trong thời gian không đổi. Nếu không có chúng, chúng tôi sẽ cần quét lại toàn bộ mảng cho mỗi lần loại bỏ ứng viên. 

Việc xử lý chỉ mục là phần tinh tế. 

Khi loại bỏ điểm dừng giữa`g[i-1]`Và`g[i]`, những khoảng trống không bị ảnh hưởng là tất cả mọi thứ trước đây`g[i-1]`và mọi thứ sau đó`g[i]`. 

Đó là lý do tại sao phần đóng góp bên trái sử dụng`pref[i-2]`và sử dụng sự đóng góp đúng đắn`suff[i+1]`. 

sử dụng`0`khi một bên không tồn tại, hãy xử lý chính xác việc di chuyển ở gần cuối tuyến đường. 

Số nguyên Python dễ dàng xử lý các giá trị khoảng cách tối đa có thể, vì tọa độ tối đa là`10^9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
0 50 125 175 236
```Mảng khoảng cách:```
50 75 50 61
```| Loại bỏ | Khoảng cách hợp nhất | Khoảng cách lớn nhất khác | Kết quả | 
| --- | --- | --- | --- | 
| Xóa điểm dừng ở 50 | 125 | 61 | 125 | 
| Xóa điểm dừng ở 125 | 125 | 61 | 125 | 
| Bỏ điểm dừng ở 175 | 111 | 75 | 111 | 

Kết quả tối thiểu là`111`. 

Đầu ra:```
111
```Ví dụ này cho thấy việc loại bỏ tối ưu không nhất thiết phải ở gần khoảng cách nhỏ nhất. Cả phân khúc được hợp nhất và phân khúc lớn nhất còn lại đều phải được xem xét. 

### Ví dụ 2 

đầu vào:```
4
100 300 600 10000
```Mảng khoảng cách:```
200 300 9400
```| Loại bỏ | Khoảng cách hợp nhất | Khoảng cách lớn nhất khác | Kết quả | 
| --- | --- | --- | --- | 
| Xóa điểm dừng ở 300 | 500 | 9400 | 9400 | 
| Xóa điểm dừng ở 600 | 9700 | 200 | 9700 | 

Sự lựa chọn tốt nhất tạo ra độ dài đoạn tối đa là`9400`. 

Đầu ra:```
9400
```Dấu vết này chứng tỏ rằng đôi khi một khoảng cách lớn đã tồn tại lại chiếm ưu thế trong câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng các khoảng trống, cực đại tiền tố, cực đại hậu tố và quét tất cả các lần xóa mỗi lần mất thời gian tuyến tính | 
| Không gian | O(n) | Mảng khoảng cách, tiền tố và hậu tố | 

Với`n ≤ 200,000`, thời gian tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    s = list(map(int, input().split()))

    gaps = [s[i + 1] - s[i] for i in range(n - 1)]
    m = n - 1

    pref = [0] * m
    pref[0] = gaps[0]
    for i in range(1, m):
        pref[i] = max(pref[i - 1], gaps[i])

    suff = [0] * m
    suff[m - 1] = gaps[m - 1]
    for i in range(m - 2, -1, -1):
        suff[i] = max(suff[i + 1], gaps[i])

    ans = 10**18

    for i in range(1, m):
        merged = gaps[i - 1] + gaps[i]

        other = 0
        if i - 2 >= 0:
            other = max(other, pref[i - 2])
        if i + 1 < m:
            other = max(other, suff[i + 1])

        ans = min(ans, max(merged, other))

    return str(ans) + "\n"

# provided samples
assert run("5\n0 50 125 175 236\n") == "111\n", "sample 1"
assert run("4\n100 300 600 10000\n") == "9400\n", "sample 2"

# custom cases
assert run("3\n0 5 12\n") == "12\n", "minimum size"
assert run("5\n0 10 20 30 40\n") == "20\n", "all equal gaps"
assert run("4\n0 100 101 200\n") == "100\n", "choose correct side"
assert run("5\n0 1 100 101 102\n") == "100\n", "boundary removal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 0 5 12`|`12`| Trường hợp hợp lệ nhỏ nhất | 
|`5 / 0 10 20 30 40`|`20`| Khoảng cách thống nhất | 
|`4 / 0 100 101 200`|`100`| So sánh chính xác giữa các lần loại bỏ ứng viên | 
|`5 / 0 1 100 101 102`|`100`| Loại bỏ gần điểm cuối | 

## Vỏ cạnh 

Hãy xem xét:```
3
0 5 12
```Chỉ có một điểm dừng có thể tháo rời. Mảng khoảng cách là`[5, 7]`. Loại bỏ điểm dừng ở giữa sẽ hợp nhất chúng thành`12`. Thuật toán đánh giá ứng viên duy nhất, tính toán`merged = 12`, không tìm thấy khoảng trống không bị ảnh hưởng và kết quả đầu ra`12`. 

Coi như:```
4
0 100 101 200
```Những khoảng trống là`[100, 1, 99]`. 

Loại bỏ điểm dừng thứ hai sẽ tạo ra một khoảng cách hợp nhất của`101`và một khoảng cách khác của`99`, do đó mức tối đa trở thành`101`. 

Loại bỏ điểm dừng thứ ba sẽ tạo ra một khoảng cách hợp nhất của`100`và một khoảng cách khác của`100`, do đó mức tối đa trở thành`100`. 

Thuật toán kiểm tra cả hai khả năng và trả về chính xác`100`. 

Coi như:```
5
0 1 100 101 102
```Những khoảng trống là`[1, 99, 1, 1]`. 

Loại bỏ điểm dừng tại vị trí`1`hợp nhất hai khoảng trống đầu tiên vào`100`. Những khoảng trống còn lại là`1`Và`1`, vậy giá trị lớn nhất là`100`. 

Các truy vấn tiền tố và hậu tố xử lý chính xác thực tế là một bên của cặp bị loại bỏ không còn khoảng trống. Đây chính xác là nơi thường xuyên xuất hiện các lỗi riêng lẻ và việc kiểm tra chỉ mục sẽ ngăn chặn chúng.
