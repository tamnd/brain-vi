---
title: "CF 1393C - Pinkie Pie Ăn Bánh Patty"
description: "Chúng ta được cung cấp nhiều tập hợp các loại bánh được biểu thị bằng số nguyên. Số nguyên bằng nhau có nghĩa là phần điền bằng nhau. Chúng ta có thể chọn bất kỳ thứ tự nào để ăn chúng. Đối với mỗi lần nhân xuất hiện nhiều lần, chúng ta có thể xem xét khoảng cách giữa các lần xuất hiện liên tiếp trong trình tự ăn uống."
date: "2026-06-11T10:03:54+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1393
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 662 (Div. 2)"
rating: 1700
weight: 1393
solve_time_s: 662
verified: true
draft: false
---

[CF 1393C - Pinkie Pie ăn bánh ngọt](https://codeforces.com/problemset/problem/1393/C) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, toán học, sắp xếp 
**Thời gian giải:** 11m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập hợp các loại bánh được biểu thị bằng số nguyên. Số nguyên bằng nhau có nghĩa là phần điền bằng nhau. Chúng ta có thể chọn bất kỳ thứ tự nào để ăn chúng. 

Đối với mỗi lần nhân xuất hiện nhiều lần, chúng ta có thể xem xét khoảng cách giữa các lần xuất hiện liên tiếp trong trình tự ăn uống. Khoảng cách được định nghĩa là số lượng bánh patty nằm giữa chúng. Ví dụ: trong trình tự:```
1 2 3 1
```hai lần đổ đầy`1`có khoảng cách`2`. 

Mục tiêu của chúng ta là sắp xếp tất cả các miếng bánh sao cho khoảng cách nhỏ nhất giữa tất cả các nhân bằng nhau càng lớn càng tốt. Chúng ta phải xuất ra khoảng cách tối thiểu tối đa có thể đạt được đó. 

Số lượng ca kiểm thử nhiều nhất là 100, nhưng hạn chế quan trọng là tổng số lượng bánh ngọt trong tất cả các ca kiểm thử nhiều nhất là 100.`10^5`. Điều này ngay lập tức gợi ý rằng các giải pháp xung quanh`O(n log n)`mỗi trường hợp thử nghiệm đều có thể chấp nhận được, trong khi mọi thứ bậc hai đều không thể thực hiện được. Một công trình mô phỏng nhiều lần việc sắp xếp lại sẽ quá chậm nếu nó thực hiện công việc tỷ lệ với thời gian trả lời`n`. 

Một số trường hợp khó khăn rất dễ bị đánh giá sai. 

Coi như:```
3
3 3 3
```Thứ tự duy nhất có thể là:```
3 3 3
```Khoảng cách là`0`Và`0`, vậy đáp án là:```
0
```Một công thức đơn giản chỉ dựa trên số lượng miếng trám riêng biệt sẽ thất bại ở đây vì chỉ có một miếng trám duy nhất. 

Coi như:```
6
1 1 1 2 2 3
```Việc làm đầy thường xuyên nhất xuất hiện ba lần. Người ta có thể cố gắng trải đều ba bản sao:```
1 2 1 3 1 2
```Khoảng cách tối thiểu chỉ`1`. Câu trả lời phụ thuộc vào số lượng phần tử có thể được đặt vào các khoảng trống được tạo bởi giá trị thường xuyên nhất, chứ không chỉ phụ thuộc vào tần số. 

Một trường hợp tế nhị khác là:```
6
2 5 2 3 1 4
```Câu trả lời mẫu là`4`. Vì giá trị`2`xuất hiện hai lần, chúng ta có thể đặt tất cả bốn phần tử còn lại vào giữa chúng:```
2 5 3 1 4 2
```Khoảng cách trở thành`4`. Điều này cho thấy khoảng cách có thể lớn hơn nhiều so với tần số của giá trị trùng lặp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra các hoán vị của nhiều tập hợp, tính toán khoảng cách tối thiểu giữa các phần điền bằng nhau trong mỗi hoán vị và lấy giá trị tốt nhất. Điều này đúng vì nó trực tiếp tìm kiếm toàn bộ không gian giải pháp. Thật không may, ngay cả đối với`n = 15`, hiện đã có hơn`10^12`hoán vị. Giới hạn thực tế là`10^5`, nên việc tìm kiếm toàn diện là hoàn toàn không thể. 

Quan sát quan trọng là chỉ có chất trám có tần số tối đa mới quan trọng. 

Giả sử tần số lớn nhất là`mx`. Cho phép`cnt`là số lượng chất trám xảy ra chính xác`mx`lần. 

Hãy tưởng tượng đặt tất cả các lần xuất hiện của những lần trám răng thường xuyên nhất đó lên đầu tiên. Vì mỗi chất trám đó xuất hiện`mx`lần, họ tạo ra`mx - 1`khoảng cách nội bộ giữa các bản sao liên tiếp. 

Ví dụ: nếu hai miếng trám xuất hiện bốn lần:```
A A A A
B B B B
```sau đó có`3`các lớp khoảng trống phải ngăn cách các bản sao liên tiếp. 

Mọi loại bánh patty khác chỉ có thể được sử dụng để lấp đầy những khoảng trống này. Cho phép```
rest = n - mx * cnt
```là số lượng bánh patty có tần số hoàn toàn nhỏ hơn mức tối đa. 

Những cái này`rest`các phần tử có thể được phân phối giữa các`mx - 1`các nhóm khoảng cách. Nếu mỗi khoảng trống nhận được```
rest // (mx - 1)
```các phần tử và một số khoảng trống nhận được một phần tử bổ sung thì kích thước khoảng trống nhỏ nhất sẽ trở thành chính xác```
rest // (mx - 1)
```Sau khi tính đến thực tế là có`cnt`các chất trám tần số tối đa có cùng cấu trúc, công thức cuối cùng trở thành```
(rest // (mx - 1)) + cnt - 1
```Đây là giá trị được chứng minh trong giải pháp chính thức. 

Toàn bộ vấn đề giảm xuống việc đếm tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần suất mỗi lần đổ đầy. 
2. Tìm tần số cực đại`mx`. 
3. Đếm chính xác có bao nhiêu miếng trám xuất hiện`mx`lần. Gọi giá trị này`cnt`. 
4. Tính số bánh còn lại:```
rest = n - mx * cnt
```5. Nếu`mx = 1`, mỗi miếng trám đều là duy nhất. Vấn đề đảm bảo có ít nhất một bản sao, nhưng việc xử lý trường hợp này sẽ làm cho công thức hoàn chỉnh. Câu trả lời sẽ là`n - 1`. 
6. Ngược lại tính:```
answer = rest // (mx - 1) + cnt - 1
```7. Xuất ra câu trả lời. 

Lý do bước 6 có hiệu quả là vì`mx - 1`các lớp phân tách bắt buộc được tạo ra bởi các chất trám thường xuyên nhất là nút cổ chai. Tất cả các phần tử còn lại phải được phân bổ giữa các lớp đó. Thương số cho chúng ta biết mỗi lớp có thể nhận được bao nhiêu chất độn và`cnt - 1`chiếm các phần lấp đầy tần số tối đa bổ sung chiếm các vị trí bên trong mỗi lớp. 

### Tại sao nó hoạt động 

Hãy để các miếng trám có tần suất tối đa là màu chủ đạo. Mọi sự sắp xếp hợp lệ đều phải phân tách các lần xuất hiện liên tiếp của mỗi màu chủ đạo bằng một số phần tử. Vì mỗi màu chủ đạo đều xuất hiện`mx`đôi khi, có chính xác`mx - 1`khoảng trống cần điền. 

Tổng cung các yếu tố không chiếm ưu thế là`rest`. Phân phối chúng càng đồng đều càng tốt để tối đa hóa khoảng thời gian nhỏ nhất. Không có sự sắp xếp nào có thể làm cho mọi khoảng cách lớn hơn`rest // (mx - 1)`, vì chỉ có`rest`các yếu tố phụ có sẵn. 

Khi có nhiều màu chủ đạo, chúng có thể xen kẽ với nhau. Điều này góp phần hiệu quả`cnt - 1`thêm vị trí cho mỗi khoảng thời gian. Giá trị thu được vừa có thể đạt được vừa tối ưu, mang lại:```
rest // (mx - 1) + cnt - 1
```## Giải pháp Python```python
import sys
from collections import Counter

input = sys.stdin.readline

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = Counter(a)

        mx = max(freq.values())
        cnt = sum(1 for v in freq.values() if v == mx)

        rest = n - mx * cnt

        answer = rest // (mx - 1) + cnt - 1
        ans.append(str(answer))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo công thức toán học trực tiếp.`Counter`tính toán tất cả các tần số trong thời gian tuyến tính. Biến`mx`lưu trữ tần số lớn nhất. Biến`cnt`đếm xem có bao nhiêu miếng trám đạt được tần suất đó. 

biểu thức```
rest = n - mx * cnt
```loại bỏ tất cả các lần xuất hiện của chất trám có tần số tối đa. Những gì còn lại chính xác là những yếu tố có sẵn để lấp đầy những khoảng trống. 

Công thức cuối cùng sử dụng phép chia số nguyên. Điều này quan trọng vì chúng tôi đang phân phối các phần tử phụ trên`mx - 1`khoảng trống và chỉ các nhóm hoàn chỉnh mới đóng góp vào kích thước khoảng cách tối thiểu. 

Tất cả các phép tính đều phù hợp thoải mái với số nguyên Python tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7
1 7 1 6 4 4 6
```Bảng tần số: 

| Điền | Tần số | 
| --- | --- | 
| 1 | 2 | 
| 4 | 2 | 
| 6 | 2 | 
| 7 | 1 | 

Bây giờ tính toán các biến. 

| Biến | Giá trị | 
| --- | --- | 
| n | 7 | 
| mx | 2 | 
| cnt | 3 | 
| nghỉ ngơi | 1 | 
| trả lời | 1 // 1 + 3 - 1 = 3 | 

Đầu ra:```
3
```Ba trám buộc cho tần số tối đa. Chúng có thể được xen kẽ và phần tử duy nhất còn lại đóng góp thêm một phần phân tách. 

### Mẫu 2 

đầu vào:```
8
1 1 4 6 4 6 4 7
```Bảng tần số: 

| Điền | Tần số | 
| --- | --- | 
| 1 | 2 | 
| 4 | 3 | 
| 6 | 2 | 
| 7 | 1 | 

Tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| n | 8 | 
| mx | 3 | 
| cnt | 1 | 
| nghỉ ngơi | 5 | 
| trả lời | 5 // 2 + 1 - 1 = 2 | 

Đầu ra:```
2
```Việc làm đầy`4`chi phối sự sắp xếp. Năm phần tử còn lại phải được phân bổ qua hai khoảng trống giữa ba bản sao của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tần số đếm và tần số quét | 
| Không gian | O(n) | Bản đồ tần suất trong trường hợp xấu nhất | 

Vì tổng số bánh ngọt trong tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`, xử lý tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Bảng tần số cũng nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = Counter(a)
        mx = max(freq.values())
        cnt = sum(1 for v in freq.values() if v == mx)

        rest = n - mx * cnt
        out.append(str(rest // (mx - 1) + cnt - 1))

    return "\n".join(out)

# provided samples
assert run("""4
7
1 7 1 6 4 4 6
8
1 1 4 6 4 6 4 7
3
3 3 3
6
2 5 2 3 1 4
""") == """3
2
0
4"""

# all equal
assert run("""1
5
7 7 7 7 7
""") == "0"

# two values tied for maximum frequency
assert run("""1
6
1 1 1 2 2 2
""") == "1"

# one dominant value
assert run("""1
6
1 1 1 2 3 4
""") == "1"

# smallest valid duplicated case
assert run("""1
2
5 5
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 7 7 7 7`|`0`| Tất cả các yếu tố giống hệt nhau | 
|`1 1 1 2 2 2`|`1`| Nhiều chất trám tần số tối đa | 
|`1 1 1 2 3 4`|`1`| Điền đơn chiếm ưu thế | 
|`5 5`|`0`| Mảng trùng lặp nhỏ nhất có thể | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
5 5 5
```Đây`mx = 3`,`cnt = 1`, Và`rest = 0`. Công thức cho:```
0 // 2 + 1 - 1 = 0
```Không có sự sắp xếp lại nào có thể tạo ra khoảng cách tích cực vì mọi vị trí đều chứa đựng sự lấp đầy giống nhau. 

Coi như:```
1
6
1 1 1 2 2 2
```Chúng tôi có`mx = 3`,`cnt = 2`, Và`rest = 0`. 

Công thức cho:```
0 // 2 + 2 - 1 = 1
```Một sự sắp xếp như:```
1 2 1 2 1 2
```đạt được khoảng cách tối thiểu`1`và không thể có giá trị lớn hơn vì không có miếng trám bổ sung nào để mở rộng khoảng trống. 

Coi như:```
1
6
2 5 2 3 1 4
```Chúng tôi có`mx = 2`,`cnt = 1`, Và`rest = 4`. 

Công thức cho:```
4 // 1 + 1 - 1 = 4
```Sự sắp xếp:```
2 5 3 1 4 2
```đặt tất cả bốn chiếc bánh patty còn lại vào giữa hai bản sao của`2`, tạo ra khoảng cách`4`, phù hợp với câu trả lời tối ưu.
