---
title: "CF 228B - Hai Bàn"
description: "Chúng ta có hai ma trận nhị phân. Một sự thay đổi (x, y) có nghĩa là ô (i, j) của ma trận thứ nhất được so sánh với ô (i ​​+ x, j + y) của ma trận thứ hai. Đối với một ca cố định, hệ số chồng chéo là tổng tích số của các ô chồng chéo."
date: "2026-06-04T08:59:51+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 228
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 141 (Div. 2)"
rating: 1400
weight: 228
solve_time_s: 95
verified: true
draft: false
---

[CF 228B - Hai bảng](https://codeforces.com/problemset/problem/228/B) 

**Đánh giá:** 1400 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai ma trận nhị phân. một ca`(x, y)`có nghĩa là tế bào đó`(i, j)`của ma trận đầu tiên được so sánh với ô`(i + x, j + y)`của ma trận thứ hai. 

Đối với một ca cố định, hệ số chồng chéo là tổng tích số của các ô chồng chéo. Vì tất cả các giá trị đều`0`hoặc`1`, một sản phẩm góp phần`1`chính xác khi cả hai ô chồng lên nhau đều chứa`1`. 

Một cách khác để xem vấn đề là chỉ nhìn vào các vị trí chứa`1`. Đối với một sự dịch chuyển nhất định, hệ số chồng chéo chỉ đơn giản là số cặp`1`các ô, mỗi ô trong mỗi bảng, sẽ được căn chỉnh sau khi áp dụng sự thay đổi đó. 

Chúng ta phải tìm bất kỳ sự thay đổi nào làm tối đa hóa số lượng này. 

Kích thước của cả hai ma trận lớn nhất là`50 × 50`. Điều đó có nghĩa là mỗi ma trận chứa nhiều nhất`2500`tế bào. Một giải pháp kiểm tra mọi ca có thể xảy ra và mọi cặp ô sẽ thực hiện hàng chục triệu thao tác. Con số đó không lớn nhưng có một quan sát rõ ràng hơn nhiều dẫn trực tiếp đến giải pháp mong muốn. 

Thực tế là các ma trận chỉ chứa`0`Và`1`là thuộc tính cấu trúc quan trọng. Chỉ những vị trí chứa`1`vấn đề. Vì mỗi ma trận được đảm bảo chứa ít nhất một`1`, chúng ta có thể tập trung hoàn toàn vào những vị trí đó. 

Một sai lầm phổ biến là chỉ nghĩ đến những khu vực chồng chéo. Sự dịch chuyển tối ưu có thể đặt hầu hết ma trận này ra ngoài ma trận kia. Ví dụ:```
A:
1

B:
1
```Mỗi ca đều tạo ra yếu tố chồng chéo`1`hoặc`0`. Bản thân các giá trị dịch chuyển không bị hạn chế bởi kích thước ma trận. 

Một lỗi dễ mắc phải khác là tính toán các dịch chuyển sai hướng. Nếu một`1`trong ma trận đầu tiên là tại`(r1, c1)`và một`1`trong ma trận thứ hai là tại`(r2, c2)`, sự dịch chuyển sắp xếp chúng là:```
x = r2 - r1
y = c2 - c1
```Đảo ngược phép trừ tạo ra sự dịch chuyển ngược lại và đếm các cách sắp xếp sai. 

Trường hợp khó phát hiện cuối cùng xảy ra khi nhiều ca đạt được sự chồng chéo tối đa như nhau. Câu lệnh cho phép bất kỳ câu trả lời tối ưu nào, vì vậy chúng ta chỉ cần nhớ một ca có số lượng lớn nhất. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi ca có thể. Vì các ma trận nhiều nhất là`50 × 50`, sự dịch chuyển hàng dao động trong khoảng từ`-49`ĐẾN`49`và việc dịch chuyển cột cũng thực hiện tương tự. Đối với mỗi ca, chúng tôi có thể quét tất cả các ô và đếm các cặp ô chồng lên nhau. 

Cách tiếp cận này đúng vì nó đánh giá đúng yếu tố chồng chéo cho mỗi ca ứng viên. Vấn đề là hiệu quả. có khoảng`100 × 100 = 10,000`những ca làm việc có thể xảy ra và mỗi ca có thể yêu cầu quét tới`2500`tế bào. Chi phí trong trường hợp xấu nhất là khoảng 25 triệu lượt kiểm tra tế bào. 

Quan sát quan trọng là chỉ những tế bào chứa`1`đóng góp vào yếu tố chồng chéo. 

Giả sử một`1`trong ma trận đầu tiên nằm ở`(ra, ca)`và một`1`trong ma trận thứ hai nằm ở`(rb, cb)`. Hai ô này được căn chỉnh theo đúng một ca:```
(rb - ra, cb - ca)
```Nếu nhiều cặp số một tạo ra cùng một sự dịch chuyển thì tất cả các cặp đó sẽ được căn chỉnh đồng thời theo sự dịch chuyển đó. 

Điều này biến vấn đề thành một vấn đề đếm. Với mỗi cặp bao gồm một`1`từ ma trận đầu tiên và một`1`từ ma trận thứ hai, tính toán độ dịch chuyển để căn chỉnh chúng và đếm số lần mỗi ca xuất hiện. 

Sự dịch chuyển có tần số lớn nhất chính xác là sự dịch chuyển tạo ra số lượng cặp đơn vị chồng chéo tối đa. 

Vì mỗi ma trận chứa nhiều nhất`2500`những cái đó, có nhiều nhất`2500 × 2500 = 6.25`triệu đôi. Điều đó vừa vặn một cách thoải mái trong giới hạn của Python. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(na·ma·nb·mb) qua tất cả các ca và lần quét | O(1) | Được chấp nhận nhưng kém thanh lịch | 
| Tối ưu | O(A·B) trong đó A và B là số đơn vị | O(A·B) trong trường hợp xấu nhất | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cả hai ma trận. 
2. Trích xuất tọa độ của mọi ô chứa`1`trong ma trận đầu tiên. 
3. Trích xuất tọa độ của mọi ô chứa`1`trong ma trận thứ hai. 
4. Tạo bản đồ băm từ ca`(x, y)`theo tần số của nó. 
5. Đối với mọi`1`tế bào`(ra, ca)`trong ma trận đầu tiên và mọi`1`tế bào`(rb, cb)`trong ma trận thứ hai, tính:```
x = rb - ra
y = cb - ca
```Tăng tần số dịch chuyển`(x, y)`. 

Sự thay đổi này là bản dịch duy nhất căn chỉnh hai ô đó. 
6. Theo dõi sự dịch chuyển có tần số lớn nhất. 
7. Xuất ca đó. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ ca cố định nào`(x, y)`. 

một cặp`1`các ô góp phần vào yếu tố chồng chéo theo sự thay đổi này khi và chỉ khi:```
rb = ra + x
cb = ca + y
```Sắp xếp lại mang lại:```
x = rb - ra
y = cb - ca
```Vì vậy, mỗi cặp đóng góp tương ứng với chính xác một lần xuất hiện của ca`(x, y)`trong quá trình đếm của chúng tôi. 

Ngược lại, mỗi cặp đều được tính cho ca`(x, y)`trở nên phù hợp theo sự thay đổi đó và đóng góp`1`đến yếu tố chồng chéo. 

Do đó, tần số được lưu trữ cho một ca sẽ chính xác bằng hệ số chồng lấp của nó. Tối đa hóa tần số sẽ tối đa hóa hệ số chồng chéo, điều này chứng tỏ tính đúng đắn. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    na, ma = map(int, input().split())

    ones_a = []
    for i in range(na):
        row = input().strip()
        for j, ch in enumerate(row):
            if ch == '1':
                ones_a.append((i, j))

    nb, mb = map(int, input().split())

    ones_b = []
    for i in range(nb):
        row = input().strip()
        for j, ch in enumerate(row):
            if ch == '1':
                ones_b.append((i, j))

    cnt = defaultdict(int)

    best_shift = (0, 0)
    best_count = -1

    for ra, ca in ones_a:
        for rb, cb in ones_b:
            shift = (rb - ra, cb - ca)
            cnt[shift] += 1

            if cnt[shift] > best_count:
                best_count = cnt[shift]
                best_shift = shift

    print(best_shift[0], best_shift[1])

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã trích xuất tất cả các vị trí có chứa`1`. Mọi ô khác đều không liên quan vì tích có số 0 không bao giờ đóng góp vào hệ số chồng chéo. 

Vòng lặp lồng nhau liệt kê từng cặp`1`tế bào. Đối với mỗi cặp, chúng tôi tính toán độ dịch chuyển duy nhất để căn chỉnh chúng. Từ điển đếm có bao nhiêu cặp bầu cho mỗi ca. 

Thời điểm số ca làm việc vượt quá mức tối đa hiện tại, chúng tôi sẽ lưu trữ ca đó làm câu trả lời hay nhất. Vì câu lệnh chấp nhận bất kỳ sự dịch chuyển tối ưu nào nên không cần logic ràng buộc. 

Việc sử dụng tọa độ dựa trên 0 thay vì tọa độ dựa trên một không ảnh hưởng đến các dịch chuyển được tính toán. Cả hai điểm cuối của phép trừ đều được dịch chuyển bởi cùng một hằng số, do đó hiệu vẫn giữ nguyên. 

Không có vấn đề tràn tồn tại vì sự khác biệt về tọa độ tối đa là khoảng`50`về độ lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
01
10
00
2 3
001
111
```Vị trí của những người:```
A: (0,1), (1,0)
B: (0,2), (1,0), (1,1), (1,2)
```| Một tế bào | Tế bào B | Thay đổi | 
| --- | --- | --- | 
| (0,1) | (0,2) | (0,1) | 
| (0,1) | (1,0) | (1,-1) | 
| (0,1) | (1,1) | (1,0) | 
| (0,1) | (1,2) | (1,1) | 
| (1,0) | (0,2) | (-1,2) | 
| (1,0) | (1,0) | (0,0) | 
| (1,0) | (1,1) | (0,1) | 
| (1,0) | (1,2) | (0,2) | 

Bảng tần số: 

| Thay đổi | Đếm | 
| --- | --- | 
| (0,1) | 2 | 
| tất cả những người khác | 1 | 

Tần số tối đa là`2`, Vì thế`(0,1)`là một câu trả lời tối ưu. 

Ví dụ này thể hiện ý tưởng trung tâm: câu trả lời xuất hiện trực tiếp từ việc đếm các ca giống hệt nhau. 

### Ví dụ 2```
1 1
1
1 1
1
```Vị trí:```
A: (0,0)
B: (0,0)
```| Một tế bào | Tế bào B | Thay đổi | 
| --- | --- | --- | 
| (0,0) | (0,0) | (0,0) | 

Bảng tần số: 

| Thay đổi | Đếm | 
| --- | --- | 
| (0,0) | 1 | 

Đầu ra:```
0 0
```Điều này cho thấy đầu vào hợp lệ nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(A·B) | A và B là số đơn vị trong hai ma trận | 
| Không gian | O(A·B) trường hợp xấu nhất | Các ca riêng biệt được lưu trữ trong bản đồ băm | 

Trong trường hợp xấu nhất, cả hai ma trận đều chứa đầy ma trận 1, vì vậy`A = B = 2500`. Thuật toán thực hiện`6.25 × 10^6`đánh giá theo cặp, phù hợp thoải mái trong thời gian giới hạn. Số lượng các dịch chuyển riêng biệt thực sự bị giới hạn bởi các phạm vi tọa độ, khiến cho việc sử dụng bộ nhớ rất ít trong thực tế. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    na, ma = map(int, input().split())

    ones_a = []
    for i in range(na):
        row = input().strip()
        for j, ch in enumerate(row):
            if ch == '1':
                ones_a.append((i, j))

    nb, mb = map(int, input().split())

    ones_b = []
    for i in range(nb):
        row = input().strip()
        for j, ch in enumerate(row):
            if ch == '1':
                ones_b.append((i, j))

    cnt = defaultdict(int)

    best_shift = (0, 0)
    best_count = -1

    for ra, ca in ones_a:
        for rb, cb in ones_b:
            shift = (rb - ra, cb - ca)
            cnt[shift] += 1

            if cnt[shift] > best_count:
                best_count = cnt[shift]
                best_shift = shift

    return f"{best_shift[0]} {best_shift[1]}"

# provided sample
assert run(
"""3 2
01
10
00
2 3
001
111
"""
) == "0 1"

# minimum size
assert run(
"""1 1
1
1 1
1
"""
) == "0 0"

# unique non-zero shift
assert run(
"""2 2
10
00
2 2
00
01
"""
) == "1 1"

# all cells are ones
out = run(
"""2 2
11
11
2 2
11
11
"""
)
assert out == "0 0"

# boundary shift
assert run(
"""1 2
10
2 2
00
01
"""
) == "1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận đơn bào |`0 0`| Đầu vào hợp lệ tối thiểu | 
| Một góc căn chỉnh với góc đối diện |`1 1`| Chuyển hướng đúng | 
| Tất cả các ô bằng một |`0 0`| Tích lũy số lượng chồng chéo lớn | 
| Căn chỉnh từng cạnh |`1 1`| Sự khác biệt tọa độ ranh giới | 

## Vỏ cạnh 

Xét hai ma trận, mỗi ma trận chứa một`1`:```
1 1
1
1 1
1
```Cặp duy nhất tạo ra sự dịch chuyển`(0,0)`. Tần số của sự dịch chuyển đó là`1`, do đó thuật toán trả về`(0,0)`. Không cần tính toán diện tích chồng lấp. 

Hãy xem xét trường hợp độ dịch chuyển tối ưu di chuyển một ma trận gần như ra ngoài ma trận kia:```
1 2
10
2 2
00
01
```Vị trí của những người đó là`(0,0)`Và`(1,1)`. Sự thay đổi liên kết của họ là`(1,1)`. Thuật toán đếm cặp này và trả về`(1,1)`mặc dù hầu hết các ô đều nằm ngoài vùng chồng lấp. Điều này đúng vì hệ số chồng chéo chỉ phụ thuộc vào hệ số trùng lặp. 

Hãy xem xét nhiều câu trả lời tối ưu:```
1 2
11
1 2
11
```Cả hai ca`(0,0)`và một số người khác có thể nhận được số lượng trong quá trình xử lý, nhưng`(0,0)`đạt được hệ số chồng chéo tối đa của`2`. Nếu một ca khác được gắn với mức tối đa trong một ví dụ khác, thuật toán sẽ trả về bất kỳ giá trị nào đạt mức tối đa trước. Tuyên bố rõ ràng cho phép bất kỳ câu trả lời tối ưu nào, vì vậy hành vi này là hợp lệ. 

Cuối cùng, hãy xem xét lỗi ký hiệu phổ biến. Nếu một`1`xuất hiện tại`(0,0)`trong ma trận đầu tiên và`(1,1)`trong ma trận thứ hai, sự dịch chuyển căn chỉnh là:```
(1 - 0, 1 - 0) = (1,1)
```sử dụng`(0 - 1, 0 - 1)`sẽ sản xuất`(-1,-1)`, làm di chuyển ma trận thứ hai theo hướng sai. Thuật toán tính toán liên tục`rb - ra`Và`cb - ca`, tránh lỗi này.
