---
title: "CF 106160H - Nhớ nhà"
description: "Chúng ta có hai chuỗi chữ thường có độ dài bằng nhau, S và T. Một thao tác chọn hậu tố S và giá trị k trong khoảng từ 1 đến 25. Mỗi ký tự trong hậu tố đó được dịch chuyển về phía trước bởi k vị trí trong bảng chữ cái, với khoảng bao quanh từ z trở lại a."
date: "2026-06-25T11:13:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "H"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 39
verified: true
draft: false
---

[CF 106160H - Nhớ nhà](https://codeforces.com/problemset/problem/106160/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi chữ thường có độ dài bằng nhau,`S`Và`T`. 

Một thao tác chọn một hậu tố của`S`và một giá trị`k`trong khoảng từ 1 đến 25. Mỗi ký tự trong hậu tố đó được dịch chuyển về phía trước bởi`k`các vị trí trong bảng chữ cái, với sự bao bọc từ`z`quay lại`a`. 

Nhiệm vụ là biến đổi`S`vào trong`T`sử dụng càng ít thao tác càng tốt. 

Một cách hữu ích để suy nghĩ về vấn đề là quên đi các chữ cái thực tế và chỉ theo dõi mỗi vị trí phải được dịch chuyển bao nhiêu. Đối với mỗi chỉ số`i`, định nghĩa$$d_i = (T_i - S_i) \bmod 26$$Đây là sự thay đổi tổng thể vị trí đó`i`phải nhận. 

Độ dài chuỗi có thể lớn bằng`2 · 10^5`, do đó, bất kỳ thuật toán nào xem xét tất cả các cặp vị trí hoặc mô phỏng các hoạt động theo từng ký tự sẽ quá chậm. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính. 

Phần khó khăn là một thao tác ảnh hưởng đến một hậu tố chứ không phải một vị trí. Những thay đổi được áp dụng trước đó tiếp tục ảnh hưởng đến tất cả các vị trí sau này. 

Hãy xem xét một ví dụ nhỏ:```
S = aaa
T = bbb
```Mọi vị trí đều cần sự thay đổi`1`, Vì thế```
d = [1, 1, 1]
```Một thao tác trên toàn bộ chuỗi với`k = 1`là đủ. 

Một cách tiếp cận bất cẩn có thể đếm ba vị trí có độ dịch chuyển khác 0 và câu trả lời`3`, điều đó không đúng. 

Một trường hợp thú vị khác là:```
S = aaa
T = aba
```Sau đó```
d = [0, 1, 0]
```Vị trí 2 phải tăng một ca, trong khi vị trí 3 phải quay về 0. Vì các phép toán hậu tố lan truyền sang phải nên chúng ta cần:```
+1 starting at position 2
+25 starting at position 3
```Câu trả lời đúng là`2`. 

Điều này minh họa rằng điều quan trọng không phải là giá trị`d_i`bản thân họ mà là cách họ thay đổi giữa các vị trí lân cận. 

## Phương pháp tiếp cận 

Quan điểm brute-force là xây dựng các hoạt động một cách rõ ràng. 

Giả sử chúng ta xử lý các vị trí từ trái sang phải. Tại mỗi chỉ số, chúng tôi so sánh ca tích lũy cho đến nay với ca cần thiết tại vị trí đó, sau đó thêm các thao tác để khắc phục sự khác biệt. Điều này đã gợi ý về cấu trúc chính xác, nhưng nếu được thực hiện bằng cách cập nhật thực sự các hậu tố, mỗi thao tác có thể chạm vào`O(n)`các vị trí. Trong trường hợp xấu nhất trở thành`O(n^2)`, quá chậm đối với`n = 2 · 10^5`. 

Quan sát quan trọng là thao tác hậu tố không làm thay đổi sự khác biệt giữa hai vị trí bên trong hậu tố đó. Nó chỉ thay đổi điểm bắt đầu của hậu tố. 

Cho phép$$d_i = (T_i - S_i) \bmod 26$$và xác định$$d_0 = 0.$$Giả sử một hoạt động bắt đầu ở vị trí`i`và dịch chuyển theo`k`. Sau đó nó tăng lên mỗi`d_j`vì`j ≥ i`qua`k`, có nghĩa là nơi duy nhất mà "đạo hàm rời rạc" thay đổi là giữa các vị trí`i-1`Và`i`. 

Định nghĩa$$\Delta_i = (d_i - d_{i-1}) \bmod 26.$$Một sự thay đổi`k`bắt đầu ở vị trí`i`đóng góp chính xác`k`ĐẾN`Δ_i`và không nơi nào khác. 

Điều đó có nghĩa là mỗi vị trí hoàn toàn độc lập trong mảng sai phân. 

Nếu như`Δ_i = 0`, không cần thao tác ở đó. 

Nếu như`Δ_i ≠ 0`, một thao tác duy nhất bắt đầu tại vị trí`i`với`k = Δ_i`tạo ra chính xác sự thay đổi cần thiết. 

Vì một thao tác có thể nhận ra bất kỳ giá trị nào từ 1 đến 25, nên mọi giá trị khác 0`Δ_i`chi phí chính xác cho một hoạt động. 

Câu trả lời đơn giản là số lượng chỉ số có$$(d_i - d_{i-1}) \bmod 26 \ne 0.$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hậu tố Brute Force | O(n²) | O(n) | Quá chậm | 
| Quan sát mảng khác biệt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`S`, Và`T`. 
2. Đối với mọi vị trí`i`, tính toán$$d_i = (T_i - S_i) \bmod 26.$$Đây là tổng số ca cần thiết ở vị trí đó. 
3. Hãy để`prev = 0`, đại diện cho`d_0`. 
4. Di chuyển vị trí từ trái sang phải. 
5. Tính toán$$\Delta = (d_i - prev) \bmod 26.$$Đây là số ca mới phải bắt đầu chính xác ở vị trí này. 
6. Nếu`Δ ≠ 0`, tăng câu trả lời lên một. 

Một thao tác hậu tố duy nhất với shift`Δ`có thể tạo ra sự thay đổi này. 
7. Đặt`prev = d_i`và tiếp tục. 
8. Xuất số đếm cuối cùng. 

### Tại sao nó hoạt động 

Mọi thao tác hậu tố đều ảnh hưởng đến tất cả các vị trí từ điểm bắt đầu trở đi. Trong dãy ca cần thiết`d`, thao tác như vậy chỉ thay đổi một ranh giới: sự chuyển đổi giữa các vị trí`i-1`Và`i`. 

số lượng$$\Delta_i = (d_i - d_{i-1}) \bmod 26$$ghi lại chính xác số ca phải bắt đầu ở vị trí`i`. 

Khác không`Δ_i`không thể được tạo ra bởi các hoạt động bắt đầu ở nơi khác, vì vậy cần có ít nhất một hoạt động. Ngược lại, một thao tác với shift`Δ_i`tạo ra chính xác yêu cầu đó. 

Như vậy mọi khác 0`Δ_i`đóng góp chính xác một thao tác và mọi thao tác bằng 0`Δ_i`không đóng góp gì Việc đếm các khác biệt khác 0 sẽ cho số phép toán tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()
t = input().strip()

ans = 0
prev = 0

for i in range(n):
    cur = (ord(t[i]) - ord(s[i])) % 26
    delta = (cur - prev) % 26
    if delta != 0:
        ans += 1
    prev = cur

print(ans)
```Biến`cur`lưu trữ tổng số ca cần thiết cho vị trí hiện tại. 

Biến`prev`lưu trữ sự thay đổi cần thiết của vị trí trước đó. Máy tính```
(cur - prev) % 26
```đưa ra giá trị mảng khác biệt`Δ`. 

Bất cứ khi nào giá trị đó khác 0, chúng ta cần chính xác một thao tác bắt đầu từ vị trí hiện tại. 

Hoạt động modulo là cần thiết. Ví dụ: chuyển từ ca bắt buộc`1`đến ca cần thiết`0`tương ứng với```
0 - 1 = -1
```nên được hiểu là`25`trong số học modulo 26. Việc quên modulo sẽ coi quá trình chuyển đổi này là phủ định một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
S = aaaaaaaaa
T = aaabbbaaa
```Ca làm việc bắt buộc:```
d = [0,0,0,1,1,1,0,0,0]
```| Vị trí | d[i] | Trước d | Δ | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 0 | 
| 3 | 0 | 0 | 0 | 0 | 
| 4 | 1 | 0 | 1 | 1 | 
| 5 | 1 | 1 | 0 | 1 | 
| 6 | 1 | 1 | 0 | 1 | 
| 7 | 0 | 1 | 25 | 2 | 
| 8 | 0 | 0 | 0 | 2 | 
| 9 | 0 | 0 | 0 | 2 | 

Câu trả lời cuối cùng:`2`. 

Thao tác đầu tiên bắt đầu vùng dịch chuyển và thao tác thứ hai sẽ hủy nó sau đó. 

### Ví dụ 2```
S = aaa
T = bbb
```Ca làm việc bắt buộc:```
d = [1,1,1]
```| Vị trí | d[i] | Trước d | Δ | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 1 | 1 | 0 | 1 | 
| 3 | 1 | 1 | 0 | 1 | 

Câu trả lời cuối cùng:`1`. 

Một thao tác trên toàn bộ chuỗi là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua dây | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với`n ≤ 2 · 10^5`, quét tuyến tính dễ dàng đủ nhanh trong giới hạn thời gian và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()
    t = input().strip()

    ans = 0
    prev = 0

    for i in range(n):
        cur = (ord(t[i]) - ord(s[i])) % 26
        delta = (cur - prev) % 26
        if delta != 0:
            ans += 1
        prev = cur

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided samples
assert run("4\naaaa\naaaa\n") == "0\n"
assert run("9\naaaaaaaaa\naaabbbaaa\n") == "2\n"

# minimum size
assert run("1\na\nb\n") == "1\n"

# all positions need same shift
assert run("3\naaa\nbbb\n") == "1\n"

# alternating requirements
assert run("3\naaa\naba\n") == "2\n"

# wraparound case
assert run("2\nba\nab\n") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a -> b`|`1`| Trường hợp không tầm thường nhỏ nhất | 
|`aaa -> bbb`|`1`| Một thao tác có thể ảnh hưởng đến nhiều vị trí | 
|`aaa -> aba`|`2`| Cần cả hoạt động bắt đầu và hủy bỏ | 
|`ba -> ab`|`2`| Xử lý modulo-26 đúng | 

## Vỏ cạnh 

Hãy xem xét:```
3
aaa
aba
```Ca làm việc bắt buộc là:```
[0, 1, 0]
```Thuật toán tính toán:```
Δ = [0, 1, 25]
```Cả hai mục khác 0 đều được tính, đưa ra câu trả lời`2`. 

Một thao tác bắt đầu một`+1`shift ở vị trí 2. Một thao tác khác bắt đầu`+25`dịch chuyển ở vị trí 3, hủy bỏ hiệu ứng đối với phần còn lại của chuỗi. 

Bây giờ hãy xem xét:```
3
aaa
bbb
```Ca làm việc bắt buộc:```
[1, 1, 1]
```Sự khác biệt:```
Δ = [1, 0, 0]
```Chỉ tồn tại một mục khác 0 nên câu trả lời là`1`. Điều này nắm bắt chính xác rằng một thao tác hậu tố duy nhất trên toàn bộ chuỗi sẽ ảnh hưởng đồng thời đến cả ba vị trí. 

Cuối cùng, hãy xem xét một quá trình chuyển đổi bao quanh:```
2
ba
ab
```Ca làm việc bắt buộc:```
[25, 1]
```Sự khác biệt:```
Δ1 = 25
Δ2 = (1 - 25) mod 26 = 2
```Cả hai đều khác 0 nên đáp án là`2`. Số học modulo xử lý chính xác quá trình chuyển đổi qua ranh giới bảng chữ cái.
