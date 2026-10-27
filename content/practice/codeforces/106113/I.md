---
title: "CF 106113I - Cristales M\u00e1gicos"
description: "Chúng ta có tinh thể N. Mỗi tinh thể thuộc về một họ được xác định bằng tên màu và có giá trị công suất. Chúng ta phải chọn chính xác K tinh thể, với hạn chế là không có hai tinh thể được chọn nào có thể đến từ cùng một họ. Mục tiêu là tối đa hóa tổng sức mạnh của họ."
date: "2026-06-25T11:39:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106113
codeforces_index: "I"
codeforces_contest_name: "Coding Cup TecNM 2025"
rating: 0
weight: 106113
solve_time_s: 51
verified: true
draft: false
---

[CF 106113I - Cristales M\u00e1gicos](https://codeforces.com/problemset/problem/106113/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`N`tinh thể. Mỗi tinh thể thuộc về một họ được xác định bằng tên màu và có giá trị công suất. 

Chúng ta phải chọn chính xác`K`tinh thể, với hạn chế là không có hai tinh thể được chọn nào có thể đến từ cùng một họ. Mục tiêu là tối đa hóa tổng sức mạnh của họ. 

Nếu điều đó là không thể vì có ít hơn`K`các gia đình riêng biệt, chúng tôi in`IMPOSIBLE`. 

Nếu một số lựa chọn đạt được tổng công suất tối đa như nhau, chúng ta phải xuất ra một lựa chọn có danh sách các chỉ số tinh thể được sắp xếp nhỏ nhất về mặt từ điển. 

Kích thước đầu vào là thách thức chính. Có thể có tới`10^6`tinh thể và tên họ có thể là những chuỗi dài. Bất kỳ giải pháp nào so sánh tất cả các cặp tinh thể hoặc sắp xếp liên tục các bộ sưu tập lớn sẽ quá chậm. Thuật toán về cơ bản phải xử lý mỗi tinh thể một lần và chỉ giữ lại thông tin tổng hợp cho mỗi họ. 

Một chi tiết tinh tế là sự ràng buộc về mặt từ điển. Chỉ phát huy tối đa sức mạnh thôi là chưa đủ. Hai lựa chọn tối ưu khác nhau có thể có tổng công suất như nhau và chúng ta phải chọn cẩn thận các chỉ số tạo ra chuỗi được sắp xếp theo từ điển nhỏ nhất. 

Hãy xem xét ví dụ này:```
4 1
RED 10
RED 10
BLUE 10
GREEN 5
```Công suất tối đa là`10`. 

Câu trả lời tối ưu có thể có là chọn chỉ mục`1`,`2`, hoặc`3`. 

Đầu ra đúng là chỉ mục`1`, bởi vì`[1]`về mặt từ điển nhỏ hơn`[2]`hoặc`[3]`. 

Một trường hợp quan trọng khác xuất hiện khi một số gia đình có sức mạnh tốt nhất như nhau và cạnh tranh để giành lấy những vị trí cuối cùng còn trống.```
5 2
A 10
B 10
C 10
D 5
E 1
```Bất kỳ hai trong số`A`,`B`, Và`C`đưa ra số tiền tối đa như nhau`20`. 

Câu trả lời đúng sử dụng chỉ số`[1, 2]`, không`[1, 3]`hoặc`[2, 3]`, bởi vì`[1, 2]`là nhỏ nhất về mặt từ điển. 

Trường hợp cạnh cuối cùng là khi một họ chứa nhiều tinh thể có cùng công suất tối đa.```
4 2
RED 7
RED 7
BLUE 5
GREEN 1
```Dành cho gia đình`RED`, cả hai chỉ số`1`Và`2`đạt được sự đóng góp tương tự. Chọn chỉ mục`1`ít nhất về mặt từ điển luôn tốt như việc chọn chỉ mục`2`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi tập hợp con của`K`tinh thể, kiểm tra xem tất cả các họ có khác biệt hay không, tính toán tổng công suất và giữ lại lựa chọn hợp lệ tốt nhất. Ngay cả đối với`N = 50`, điều này đã là vô vọng vì số lượng tập hợp con tăng theo cấp số nhân. Với`N = 10^6`, cách tiếp cận như vậy là hoàn toàn không thể. 

Quan sát đầu tiên là, đối với bất kỳ họ nào, chỉ có tinh thể mạnh nhất của nó mới có thể xuất hiện trong một giải pháp tối ưu. Nếu một gia đình có quyền lực`3`,`8`, Và`12`, chọn bất cứ thứ gì ngoại trừ tinh thể có sức mạnh`12`chỉ có thể làm giảm tổng công suất. 

Điều này cho phép chúng tôi nén mọi họ thành một giá trị duy nhất:`V = maximum power in that family`. 

Vẫn còn một điều phức tạp. Một họ có thể chứa nhiều tinh thể có sức mạnh tương đương`V`. Vì tất cả chúng đều đóng góp số tiền như nhau nên chúng ta nên giữ chỉ số nhỏ nhất trong số chúng. Bất kỳ chỉ mục lớn hơn nào cũng chỉ có thể làm cho câu trả lời cuối cùng trở nên tồi tệ hơn về mặt từ điển. 

Sau lần nén này, mọi gia đình sẽ trở thành:```
(best_power, best_index)
```Bây giờ vấn đề thay đổi thành: 

Chọn chính xác`K`các gia đình. 

Mỗi gia đình được chọn đều đóng góp`best_power`. 

Trong số tất cả các lựa chọn có công suất tối đa, hãy trả về danh sách các chỉ mục đã chọn được sắp xếp nhỏ nhất theo từ điển. 

Để tối đa hóa sức mạnh, chúng ta chỉ cần thực hiện`K`quyền lực lớn nhất của gia đình. 

Sự mơ hồ duy nhất xảy ra khi một số gia đình chia sẻ nguồn điện bị cắt. 

Giả định:```
powers = [15, 12, 12, 12, 8]
K = 3
```Gia đình có quyền lực`15`phải được chọn. 

Gia đình có quyền lực`8`không bao giờ có thể được chọn. 

Trong số ba gia đình có quyền lực`12`, chúng ta cần thêm hai lựa chọn nữa. 

Vì tất cả các họ này đóng góp sức mạnh như nhau nên quy tắc từ điển trở thành yếu tố quyết định. Chúng ta nên chọn các họ có chỉ số đại diện nhỏ nhất. 

Điều đó mang lại một chiến lược rõ ràng: 

1. Tính công suất cực đại và chỉ số nhỏ nhất đạt được của mỗi họ. 
2. Sắp xếp gia đình theo sức mạnh. 
3. Xác định công suất cắt. 
4. Chọn tất cả các họ vượt quá giới hạn. 
5. Trong số các họ bằng điểm giới hạn, hãy chọn càng nhiều càng tốt, ưu tiên các chỉ số đại diện nhỏ hơn. 
6. Sắp xếp các chỉ số kết quả và in chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(N + F log F) | O(F) | Đã chấp nhận | 

Đây`F`là số họ riêng biệt. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các tinh thể và nhóm chúng theo tên họ. 
2. Đối với mỗi gia đình, hãy duy trì hai giá trị: 

- Sức mạnh tối đa được thấy cho đến nay. 
- Chỉ số nhỏ nhất có lũy thừa bằng giá trị lớn nhất đó. 
3. Sau khi xử lý tất cả các tinh thể, hãy để`F`là số họ riêng biệt. 
4. Nếu`F < K`, in`IMPOSIBLE`bởi vì việc chọn một tinh thể từ mỗi họ vẫn không cung cấp đủ tinh thể. 
5. Biến mỗi gia đình thành một cặp`(power, index)`Ở đâu:

-`power`là quyền lực tối đa của gia đình. 
-`index`là chỉ số nhỏ nhất đạt được sức mạnh đó. 
6. Sắp xếp các cặp này theo lũy thừa giảm dần. 
7. Hãy để`T`là sức mạnh của`K`-gia đình thứ theo thứ tự này. Đây là công suất cắt. 
8. Chọn mọi gia đình có thế lực lớn hơn`T`. Việc đưa chúng vào là bắt buộc vì việc thay thế bất kỳ điều nào trong số chúng sẽ làm giảm tổng công suất. 
9. Đếm xem vẫn cần thêm bao nhiêu gia đình nữa. 
10. Tập hợp tất cả các gia đình có sức mạnh tương đương`T`. 
11. Sắp xếp các họ ngưỡng đó theo chỉ số đại diện của chúng và lấy chính xác số lượng yêu cầu. 
12. Tập hợp tất cả các chỉ số đã chọn, sắp xếp chúng tăng dần và in chúng cùng với tổng công suất. 

### Tại sao nó hoạt động 

Mỗi gia đình đóng góp độc lập vào tổng sức mạnh. Việc thay thế một tinh thể đã chọn bằng một tinh thể khác từ cùng một họ không bao giờ có thể cải thiện tổng vượt quá công suất tối đa của họ, vì vậy mọi giải pháp tối ưu đều phải sử dụng một tinh thể có công suất tối đa từ mỗi họ đã chọn. 

Sau khi giảm mỗi gia đình tới mức đóng góp tối đa có thể đạt được, việc tối đa hóa tổng công suất sẽ tương đương với việc chọn`K`quyền lực lớn nhất của gia đình. Bất kỳ họ nào nằm trên điểm cắt phải thuộc mọi giải pháp tối ưu, trong khi bất kỳ họ nào nằm dưới điểm cắt không bao giờ có thể thuộc về một giải pháp tối ưu. 

Sự tự do duy nhất đến từ những gia đình ở đúng thời điểm bị cắt điện. Vì tất cả các gia đình như vậy đóng góp số tiền như nhau nên tổng sức mạnh không thay đổi bất kể họ được chọn. Để có được danh sách chỉ mục được sắp xếp nhỏ nhất về mặt từ điển, chúng tôi chọn các chỉ mục đại diện nhỏ nhất trong số các họ liên kết đó. Bất kỳ lựa chọn nào khác sẽ thay thế chỉ mục nhỏ hơn bằng chỉ mục lớn hơn và làm cho chuỗi được sắp xếp lớn hơn về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    families = {}

    for idx in range(1, n + 1):
        color, power = input().split()
        power = int(power)

        if color not in families:
            families[color] = [power, idx]
        else:
            best_power, best_idx = families[color]

            if power > best_power:
                families[color] = [power, idx]
            elif power == best_power and idx < best_idx:
                families[color][1] = idx

    if len(families) < k:
        print("IMPOSIBLE")
        return

    items = [(p, idx) for p, idx in families.values()]
    items.sort(key=lambda x: -x[0])

    threshold = items[k - 1][0]

    chosen_indices = []
    total_power = 0

    equal_group = []

    for power, idx in items:
        if power > threshold:
            chosen_indices.append(idx)
            total_power += power
        elif power == threshold:
            equal_group.append((idx, power))

    need = k - len(chosen_indices)

    equal_group.sort()

    for idx, power in equal_group[:need]:
        chosen_indices.append(idx)
        total_power += power

    chosen_indices.sort()

    print(total_power)
    print(*chosen_indices)

solve()
```Từ điển chỉ lưu trữ những viên pha lê tốt nhất cho mỗi gia đình. Khi tìm thấy nguồn điện lớn hơn, hồ sơ gia đình sẽ được thay thế. Khi công suất tối đa tương tự xuất hiện trở lại, chỉ số nhỏ hơn được giữ nguyên. 

Sau khi tất cả các họ được nén, thuật toán không bao giờ cần đến các tinh thể ban đầu nữa. Danh sách họ đã được sắp xếp xác định công suất cắt. Các họ trên điểm giới hạn là thành viên cố định của mọi giải pháp tối ưu, trong khi các họ bị ràng buộc được xử lý riêng. 

Nhóm ngưỡng được sắp xếp theo chỉ mục vì quy tắc ràng buộc phụ thuộc vào thứ tự từ điển cuối cùng của các chỉ mục đã chọn, không phụ thuộc vào họ hoặc quyền hạn. 

Số nguyên Python xử lý tổng công suất một cách an toàn vì tổng tối đa có thể là`10^6 × 10^9 = 10^15`, nằm trong phạm vi số nguyên có độ chính xác tùy ý của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
ROJO 5
AZUL 3
ROJO 4
VERDE 6
AZUL 2
```Nén gia đình tạo ra: 

| Gia đình | Sức mạnh tốt nhất | Chỉ số đại diện | 
| --- | --- | --- | 
| ROJO | 5 | 1 | 
| AZUL | 3 | 2 | 
| XÁC MINH | 6 | 4 | 

Sắp xếp theo sức mạnh: 

| Quyền lực | Chỉ mục | 
| --- | --- | 
| 6 | 4 | 
| 5 | 1 | 
| 3 | 2 |`K = 3`, vì vậy tất cả các họ đều được chọn. 

| Bước | Chỉ số được chọn | Tổng công suất | 
| --- | --- | --- | 
| Chọn XÁC MINH | [4] | 6 | 
| Chọn ROJO | [4,1] | 11 | 
| Chọn AZUL | [4,1,2] | 14 | 

Chỉ số được sắp xếp:```
1 2 4
```Đầu ra:```
14
1 2 4
```Ví dụ này cho thấy trường hợp cơ bản trong đó mỗi họ riêng biệt phải được chọn. 

### Ví dụ 2 

đầu vào:```
5 2
A 10
B 10
C 10
D 5
E 1
```Gia đình nén: 

| Gia đình | Quyền lực | Chỉ mục | 
| --- | --- | --- | 
| A | 10 | 1 | 
| B | 10 | 2 | 
| C | 10 | 3 | 
| D | 5 | 4 | 
| E | 1 | 5 | 

Công suất cắt là`10`. 

Ba gia đình hòa nhau ở điểm giới hạn, nhưng chỉ có thể chọn hai gia đình. 

| Danh mục ứng viên | Đã chọn? | 
| --- | --- | 
| 1 | Có | 
| 2 | Có | 
| 3 | Không | 

Chỉ số cuối cùng:```
1 2
```Điều này chứng tỏ sự ràng buộc về mặt từ vựng được giải quyết như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + F log F) | Một lần lướt qua tất cả các tinh thể, sau đó sắp xếp các họ riêng biệt | 
| Không gian | O(F) | Một bản ghi cho mỗi họ riêng biệt |`F`nhiều nhất là`N`, vậy trường hợp xấu nhất là`O(N log N)`. Với`N = 10^6`, điều này đủ hiệu quả cho các giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    data = io.StringIO(inp)
    input = data.readline

    n, k = map(int, input().split())

    families = {}

    for idx in range(1, n + 1):
        color, power = input().split()
        power = int(power)

        if color not in families:
            families[color] = [power, idx]
        else:
            bp, bi = families[color]
            if power > bp:
                families[color] = [power, idx]
            elif power == bp and idx < bi:
                families[color][1] = idx

    out = []

    if len(families) < k:
        out.append("IMPOSIBLE")
        return "\n".join(out)

    items = [(p, idx) for p, idx in families.values()]
    items.sort(key=lambda x: -x[0])

    threshold = items[k - 1][0]

    chosen = []
    total = 0
    eq = []

    for p, idx in items:
        if p > threshold:
            chosen.append(idx)
            total += p
        elif p == threshold:
            eq.append((idx, p))

    need = k - len(chosen)

    eq.sort()

    for idx, p in eq[:need]:
        chosen.append(idx)
        total += p

    chosen.sort()

    out.append(str(total))
    out.append(" ".join(map(str, chosen)))
    return "\n".join(out)

# provided samples
assert run(
"""5 3
ROJO 5
AZUL 3
ROJO 4
VERDE 6
AZUL 2
"""
) == "14\n1 2 4"

assert run(
"""4 3
AMARILLO 4
GRIS 5
AMARILLO 6
GRIS 7
"""
) == "IMPOSIBLE"

# minimum size with one family
assert run(
"""3 1
A 5
A 7
A 6
"""
) == "7\n2"

# equal maximum power inside a family
assert run(
"""4 2
A 10
A 10
B 5
C 1
"""
) == "15\n1 3"

# cutoff tie between families
assert run(
"""5 2
A 10
B 10
C 10
D 5
E 1
"""
) == "20\n1 2"

# all families distinct
assert run(
"""4 4
A 1
B 2
C 3
D 4
"""
) == "10\n1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một gia đình, K=1 | Pha lê tốt nhất được chọn | Tính đúng đắn cơ bản | 
| Cực đại bằng nhau trong một họ | Chỉ số nhỏ nhất được giữ lại | Xử lý ràng buộc trong nội bộ gia đình | 
| Cà vạt cắt ba chiều | Chỉ số nhỏ nhất được chọn | Quy tắc từ điển học | 
| K bằng số họ | Tất cả các gia đình được chọn | Điều kiện biên | 
| Các họ riêng biệt nhỏ hơn K | KHÔNG THỂ | Kiểm tra tính khả thi | 

## Vỏ cạnh 

Hãy xem xét:```
4 3
AMARILLO 4
GRIS 5
AMARILLO 6
GRIS 7
```Chỉ có hai gia đình tồn tại:`AMARILLO`Và`GRIS`. 

Thuật toán tính toán:```
F = 2
K = 3
```Từ`F < K`, nó in ngay:```
IMPOSIBLE
```Không cần xử lý thêm. 

Bây giờ hãy xem xét:```
4 2
RED 7
RED 7
BLUE 5
GREEN 1
```Nén gia đình mang lại: 

| Gia đình | Sức mạnh tốt nhất | Chỉ mục | 
| --- | --- | --- | 
| ĐỎ | 7 | 1 | 
| XANH | 5 | 3 | 
| XANH | 1 | 4 | 

Mặc dù chỉ số`2`có sức mạnh tương tự như chỉ số`1`, chỉ số đại diện trở thành`1`. 

Các gia đình được chọn là`RED`Và`BLUE`. 

Đầu ra:```
12
1 3
```Yêu cầu về từ điển được thỏa mãn một cách tự động vì chỉ mục nhỏ nhất đạt được mức tối đa của họ được lưu trữ trong quá trình tiền xử lý. 

Cuối cùng, hãy xem xét:```
5 2
A 10
B 10
C 10
D 5
E 1
```Công suất cắt là`10`. Nhóm ngưỡng chứa các chỉ số`[1,2,3]`. 

Thuật toán cần hai trong số đó, vì vậy nó cần hai giá trị nhỏ nhất:```
[1,2]
```Bất kỳ lựa chọn nào khác sẽ tạo ra một chuỗi được sắp xếp lớn hơn về mặt từ điển, đồng thời mang lại tổng công suất như nhau.
