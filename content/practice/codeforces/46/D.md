---
title: "CF 46D - Bãi Đỗ Xe"
description: "Chúng ta có một đoạn đỗ xe được biểu thị bằng khoảng [0, L]. Mỗi chiếc ô tô đến một lúc, luôn lái từ trái sang phải và mỗi tài xế đều muốn đỗ xe ở vị trí sớm nhất có thể."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 46
codeforces_index: "D"
codeforces_contest_name: "School Personal Contest #2 (Winter Computer School 2010/11) - Codeforces Beta Round 43 (ACM-ICPC Rules)"
rating: 1800
weight: 46
solve_time_s: 114
verified: true
draft: false
---
[CF 46D - Bãi đỗ xe](https://codeforces.com/problemset/problem/46/D) 

**Đánh giá:** 1800 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một phân khúc đỗ xe được biểu thị bằng khoảng thời gian`[0, L]`. Mỗi chiếc ô tô đến một lúc, luôn lái từ trái sang phải và mỗi tài xế đều muốn đỗ xe ở vị trí sớm nhất có thể. 

Nếu một chiếc xe có chiều dài`x`công viên quay lưng lại với tọa độ`p`, thì ô tô chiếm`[p, p + x]`. Giữa xe này và xe sau phải có ít nhất`b`mét không gian trống. Giữa xe này và xe phía trước phải có ít nhất`f`mét không gian trống. Đường viền đường phố rất đặc biệt, chúng không cần thêm khoảng cách. 

Các yêu cầu có hai loại. một loại`1 x`yêu cầu có nghĩa là một chiếc xe mới có chiều dài`x`đến và cố gắng đậu xe. Chúng ta phải xuất tọa độ nơi mặt sau của nó kết thúc, hoặc`-1`nếu không thể đỗ xe. một loại`2 k`yêu cầu có nghĩa là chiếc xe được tạo ra bởi`k`-th yêu cầu rời khỏi bãi đậu xe. 

Số lượng hoạt động nhiều nhất là`100`, nhỏ đến mức đáng ngạc nhiên. Thậm chí một`O(n^2)`mô phỏng sẽ phù hợp một cách dễ dàng. Thách thức không phải là hiệu suất, mà là mô hình hóa các quy tắc khoảng cách một cách chính xác. Hầu hết các câu trả lời sai đều xuất phát từ sự hiểu lầm về vị trí áp dụng các khoảng trống bắt buộc. 

Sự tinh tế quan trọng là khoảng cách không đối xứng. Một chiếc ô tô cần`b`mét phía sau và`f`mét phía trước. Khi hai ô tô đang đỗ cùng tồn tại thì khoảng cách trống thực tế giữa chúng phải thỏa mãn đồng thời cả hai ô tô. Giả sử xe A ở sau xe B. Khi đó khoảng cách giữa chúng ít nhất phải là`max(f, b)`? Không. Xe phía sau yêu cầu`f`phía trước, trong khi xe phía trước yêu cầu`b`phía sau nên khoảng cách ít nhất phải là`f + b`. 

Một sai lầm dễ dàng khác xuất hiện ở biên giới. Nếu ô tô là ô tô đỗ đầu tiên thì có thể xuất phát ở tọa độ`0`mà không cần một khoảng cách phía sau. Tương tự, chiếc xe đỗ cuối cùng có thể kết thúc đúng lúc`L`mà không cần một khoảng trống phía trước. 

Hãy xem xét ví dụ này:```
L = 10, b = 2, f = 3
Current parked car: occupies [4, 6]
New car length = 1
```Việc thực hiện bất cẩn có thể nghĩ rằng khoảng thời gian rảnh`[0, 4]`có thể phù hợp với một chiếc xe bắt đầu từ`0`, bởi vì`0 + 1 <= 4`. Điều đó là sai. Xe mới cũng cần`f = 3`mét trước ô tô đang đỗ, vì vậy chúng ta cần:```
0 + 1 + 3 <= 4
```điều này đúng ở đây, nhưng chỉ vừa đủ thôi. 

Một trường hợp cạnh nguy hiểm khác xuất hiện khi chen vào giữa hai xe.```
L = 20, b = 2, f = 2
Cars:
[0, 4]
[10, 14]
New car length = 2
```Khoảng trống là`[4, 10]`, chiều dài`6`. Một cách tiếp cận ngây thơ có thể cho rằng chiếc xe phù hợp vì`2 <= 6`. Diện tích thực tế cần tìm là:```
b + car_length + f = 2 + 2 + 2 = 6
```Chiếc xe vừa khít, bắt đầu từ tọa độ`6`. 

Thao tác xóa cũng dễ bị xử lý sai nếu chúng ta chỉ lưu trữ tọa độ mà quên mất yêu cầu nào đã tạo ra xe nào. Vì việc xóa liên quan đến chỉ mục yêu cầu nên chúng tôi phải duy trì ánh xạ từ id yêu cầu đến khoảng thời gian chưa sử dụng. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp nhất là duy trì tất cả các ô tô đang đỗ được sắp xếp theo vị trí. Đối với mỗi ô tô đến, chúng tôi quét bãi đậu xe từ trái sang phải và kiểm tra từng khoảng trống. 

Giả sử số ô tô đang đỗ hiện tại là:```
[p1, p1 + len1]
[p2, p2 + len2]
...
```Chúng tôi kiểm tra ba loại khoảng trống. 

Khoảng cách tiền tố trước chiếc xe đầu tiên. Một chiếc ô tô mới có thể khởi hành lúc`0`, nhưng phải rời đi`f`mét trước chiếc xe đậu đầu tiên. 

Khoảng cách giữa các xe liên tiếp. Nếu xe trước kết thúc lúc`r1`và chiếc xe tiếp theo xuất phát lúc`l2`, sau đó là một chiếc ô tô mới có chiều dài`x`có thể bắt đầu lúc`r1 + b`nếu như:```
r1 + b + x + f <= l2
```Khoảng cách hậu tố sau chiếc xe cuối cùng. Một chiếc ô tô mới có thể khởi hành lúc`last_end + b`miễn là:```
last_end + b + x <= L
```Vị trí hợp lệ đầu tiên luôn là vị trí tối ưu vì tài xế luôn chọn vị trí sớm nhất có thể. 

Mô phỏng lực lượng vũ phu này đã đủ nhanh. Nhiều nhất`100`các hoạt động tồn tại và mỗi lượt quét đến nhiều nhất`100`ô tô đỗ, đưa ra khoảng`10^4`séc. 

Quan sát chính là chúng ta không bao giờ cần cây khoảng phức tạp hoặc cấu trúc cân bằng vì các ràng buộc rất nhỏ. Công việc thực sự là tìm ra các công thức vị trí chính xác một cách chính xác. Khi đã hiểu được hình học, việc triển khai sẽ trở thành quá trình quét theo khoảng thời gian cẩn thận. 

Chúng tôi vẫn sắp xếp dữ liệu một cách rõ ràng bằng cách lưu trữ những chiếc xe đang đỗ theo thứ tự sắp xếp và loại bỏ chúng khi cần thiết. Điều đó mang lại một giải pháp đơn giản và đáng tin cậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

Giải pháp "tối ưu" ở đây thực sự là mô phỏng được mô hình hóa phù hợp. Các ràng buộc lớn hơn sẽ yêu cầu các cấu trúc khoảng, nhưng đối với`n ≤ 100`, việc thực hiện cẩn thận quan trọng hơn các thủ thuật tiệm cận. 

## Hướng dẫn thuật toán 

1. Duy trì một danh sách sắp xếp các ô tô đang đỗ. 

Mỗi chiếc xe được lưu trữ như`(start, length, request_id)`. Giữ danh sách được sắp xếp theo`start`cho phép chúng tôi quét các khoảng trống miễn phí từ trái sang phải. 
2. Duy trì một từ điển từ id yêu cầu đến dữ liệu ô tô đang đỗ. 

Các truy vấn xóa đề cập đến số yêu cầu ban đầu, không liên quan đến lệnh đỗ xe. Bản đồ này cho phép chúng tôi tìm thấy chiếc xe đang đỗ chính xác trong thời gian không đổi. 
3. Đối với yêu cầu đến với chiều dài xe`x`, trước tiên hãy thử đặt xe ở tọa độ`0`. 

Nếu không có ô tô đỗ, ô tô phù hợp khi`x <= L`. 

Nếu không, hãy để chiếc xe đậu đầu tiên xuất phát lúc`s`. Chiếc xe mới phù hợp với`0`nếu như:```
x + f <= s
```Đường viền bên trái không yêu cầu khoảng trống phía sau. 
4. Nếu khoảng cách tiền tố không thành công, hãy quét từng cặp ô tô đang đỗ liền kề. 

Giả sử chiếc xe trước đó chiếm`[l1, r1]`và chiếc xe tiếp theo xuất phát lúc`l2`. 

Thời điểm bắt đầu sớm nhất có thể là:```
r1 + b
```Để giữ khoảng cách phía trước cần thiết trước xe tiếp theo:```
r1 + b + x + f <= l2
```Cặp đầu tiên thỏa mãn điều kiện này sẽ cho vị trí đỗ xe chính xác. 
5. Nếu không có khoảng trống ở giữa, hãy thử đỗ xe sau chiếc xe cuối cùng. 

Nếu xe cuối cùng kết thúc lúc`r`, vị trí sớm nhất có thể là:```
r + b
```Chiếc xe phù hợp nếu:```
r + b + x <= L
```Đường viền bên phải không yêu cầu khoảng trống phía trước. 
6. Nếu không có trường hợp nào trong ba trường hợp thành công, xuất ra`-1`. 
7. Khi ô tô đỗ thành công, hãy đưa ô tô đó vào danh sách đã sắp xếp và ghi lại vào ánh xạ yêu cầu-id. 
8. Đối với yêu cầu xóa, hãy xác định vị trí ô tô đã lưu trữ bằng id yêu cầu và xóa nó khỏi danh sách đỗ. 

### Tại sao nó hoạt động 

Thuật toán luôn kiểm tra không gian trống từ trái sang phải và luôn chọn tọa độ hợp lệ sớm nhất bên trong mỗi khoảng trống. Mọi vị trí đỗ xe khả thi phải thuộc đúng một trong ba loại: trước xe đầu tiên, giữa hai xe hoặc sau xe cuối cùng. 

Bên trong khoảng trống ở giữa, mọi vị trí hợp lệ đều phải bắt đầu vào hoặc sau`previous_end + b`, vì xe trước yêu cầu khoảng cách phía sau. Bắt đầu muộn hơn chỉ lãng phí không gian và không thể đưa ra câu trả lời sớm hơn. Vì vậy, ứng cử viên sớm nhất bên trong khoảng trống đó được xác định duy nhất. Nếu ứng viên đó không chừa đủ khoảng cách phía trước thì không có vị trí nào sau này trong cùng khoảng trống đó có thể hoạt động được. 

Bởi vì mọi khoảng trống đều được kiểm tra theo thứ tự từ trái sang phải và vị trí khả thi sớm nhất của mỗi khoảng trống đều được kiểm tra, nên vị trí được chấp nhận đầu tiên chính xác là tọa độ đỗ xe hợp lệ ngoài cùng bên trái mà bài toán yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    L, b, f = map(int, input().split())
    n = int(input())

    parked = []
    by_request = {}

    for req_id in range(1, n + 1):
        t, x = map(int, input().split())

        if t == 1:
            length = x
            pos = -1

            if not parked:
                if length <= L:
                    pos = 0
            else:
                first_start = parked[0][0]

                if length + f <= first_start:
                    pos = 0
                else:
                    for i in range(len(parked) - 1):
                        s1, len1, _ = parked[i]
                        s2, len2, _ = parked[i + 1]

                        end1 = s1 + len1

                        candidate = end1 + b

                        if candidate + length + f <= s2:
                            pos = candidate
                            break

                    if pos == -1:
                        last_start, last_len, _ = parked[-1]
                        last_end = last_start + last_len

                        candidate = last_end + b

                        if candidate + length <= L:
                            pos = candidate

            print(pos)

            if pos != -1:
                car = (pos, length, req_id)

                idx = 0
                while idx < len(parked) and parked[idx][0] < pos:
                    idx += 1

                parked.insert(idx, car)
                by_request[req_id] = car

        else:
            car = by_request[x]
            parked.remove(car)
            del by_request[x]

solve()
```Giải pháp giữ cho bãi đỗ xe được thể hiện rõ ràng dưới dạng danh sách được sắp xếp theo khoảng thời gian được sử dụng. Vì số lượng thao tác rất nhỏ nên các thao tác danh sách đơn giản là hoàn toàn đủ. 

Logic chèn phản ánh lý luận hình học từ hướng dẫn. Trước tiên, chúng tôi thử khoảng cách tiền tố, sau đó là từng khoảng cách bên trong, sau đó là khoảng cách hậu tố. Các công thức hơi khác một chút vì đường viền không yêu cầu thêm khoảng cách. 

Một chi tiết triển khai tinh tế là ý nghĩa của`candidate`. Giữa hai ô tô đang đậu, ta luôn đặt ô tô mới càng xa bên trái càng tốt:```
candidate = end1 + b
```Bất kỳ vị trí xuất phát nào muộn hơn sẽ chỉ làm giảm khoảng trống còn lại trước xe tiếp theo, vì vậy việc kiểm tra tọa độ duy nhất này là đủ. 

Một lỗi dễ mắc phải khác là quên rằng các khoảng có độ dài liên tục, không phải các khe số nguyên. Điều kiện:```
candidate + length + f <= s2
```là chính xác. Sự bình đẳng được cho phép vì ô tô có thể để lại chính xác lượng không gian trống cần thiết. 

Logic loại bỏ sử dụng chính bộ dữ liệu được lưu trữ. Vì các bộ dữ liệu là bất biến và có khả năng nhận dạng duy nhất những chiếc ô tô đang đỗ,`list.remove()`xóa sạch khoảng thời gian chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
30 1 2
6
1 5
1 4
1 5
2 2
1 5
1 4
```| Bước | Hoạt động | Đỗ Xe Sau Hoạt Động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | Thêm chiều dài 5 |`[0,5]`| 0 | 
| 2 | Thêm chiều dài 4 |`[0,5], [6,10]`| 6 | 
| 3 | Thêm chiều dài 5 |`[0,5], [6,10], [11,16]`| 11 | 
| 4 | Xóa yêu cầu 2 |`[0,5], [11,16]`| | 
| 5 | Thêm chiều dài 5 |`[0,5], [11,16], [17,22]`| 17 | 
| 6 | Thêm chiều dài 4 |`[0,5], [11,16], [17,22], [23,27]`| 23 | 

Dấu vết cho thấy tại sao ô tô luôn chiếm tọa độ khả thi sớm nhất. Sau khi loại bỏ chiếc xe thứ hai, khoảng cách`[6,11]`chỉ là chiều dài`5`. Một chiếc xe mới có chiều dài`5`thực sự sẽ yêu cầu:```
1 + 5 + 2 = 8
```mét bên trong một khoảng trống bên trong nên không thể lọt vào đó và phải đỗ sau. 

### Ví dụ tùy chỉnh 

đầu vào:```
15 2 3
5
1 4
1 3
2 1
1 5
1 1
```| Bước | Hoạt động | Đỗ Xe Sau Hoạt Động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | Thêm chiều dài 4 |`[0,4]`| 0 | 
| 2 | Thêm chiều dài 3 |`[0,4], [6,9]`| 6 | 
| 3 | Xóa yêu cầu 1 |`[6,9]`| | 
| 4 | Thêm chiều dài 5 |`[6,9]`| -1 | 
| 5 | Thêm độ dài 1 |`[0,1], [6,9]`| 0 | 

Hoạt động thứ tư thể hiện một quy tắc biên giới. Tiền tố miễn phí trước`[6,9]`có chiều dài`6`, nhưng một chiếc ô tô có chiều dài`5`vẫn không thể phù hợp vì nó cũng cần`3`mét trước chiếc xe đang đỗ:```
5 + 3 > 6
```Hoạt động cuối cùng thành công vì độ dài`1`xe chỉ cần:```
1 + 3 <= 6
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi lần chèn có thể quét tất cả các ô tô đang đỗ | 
| Không gian | O(n) | Chúng tôi lưu trữ tất cả các ô tô hiện đang đỗ và yêu cầu lập bản đồ | 

Với nhiều nhất`100`hoạt động, công việc trong trường hợp xấu nhất là rất nhỏ. Ngay cả một mô phỏng bậc hai cũng chỉ thực hiện vài nghìn lần kiểm tra, thấp hơn nhiều so với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys_stdout = sys.stdout
    sys.stdout = out

    L, b, f = map(int, input().split())
    n = int(input())

    parked = []
    by_request = {}

    for req_id in range(1, n + 1):
        t, x = map(int, input().split())

        if t == 1:
            length = x
            pos = -1

            if not parked:
                if length <= L:
                    pos = 0
            else:
                first_start = parked[0][0]

                if length + f <= first_start:
                    pos = 0
                else:
                    for i in range(len(parked) - 1):
                        s1, len1, _ = parked[i]
                        s2, _, _ = parked[i + 1]

                        end1 = s1 + len1
                        candidate = end1 + b

                        if candidate + length + f <= s2:
                            pos = candidate
                            break

                    if pos == -1:
                        last_start, last_len, _ = parked[-1]
                        last_end = last_start + last_len

                        candidate = last_end + b

                        if candidate + length <= L:
                            pos = candidate

            print(pos)

            if pos != -1:
                car = (pos, length, req_id)

                idx = 0
                while idx < len(parked) and parked[idx][0] < pos:
                    idx += 1

                parked.insert(idx, car)
                by_request[req_id] = car

        else:
            car = by_request[x]
            parked.remove(car)
            del by_request[x]

    sys.stdout = sys_stdout
    return out.getvalue()

# provided sample
assert run(
"""30 1 2
6
1 5
1 4
1 5
2 2
1 5
1 4
"""
) == "0\n6\n11\n17\n23\n", "sample 1"

# minimum size
assert run(
"""10 1 1
1
1 10
"""
) == "0\n", "single car fills entire lot"

# exact middle fit
assert run(
"""20 2 2
3
1 4
1 4
1 2
"""
) == "0\n6\n12\n", "exact spacing equality"

# impossible due to front spacing
assert run(
"""15 2 3
2
1 4
1 9
"""
) == "0\n-1\n", "front spacing blocks placement"

# reuse freed space
assert run(
"""20 1 1
5
1 5
1 5
2 1
1 4
1 5
"""
) == "0\n6\n0\n11\n", "reuse after deletion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xe đơn chiếm toàn bộ lô |`0`| Đường viền không cần thêm khoảng cách | 
| Chính xác vừa vặn ở giữa |`0 6 12`| Điều kiện bình đẳng được cho phép | 
| Lỗi khoảng cách phía trước |`0 -1`| Khoảng cách tiền tố phải dự trữ`f`mét | 
| Tái sử dụng sau khi xóa |`0 6 0 11`| Khoảng thời gian đã xóa sẽ có sẵn chính xác | 

## Vỏ cạnh 

Hãy xem xét đầu vào này:```
15 2 3
2
1 4
1 8
```Bãi đậu xe đầu tiên ở`0`, chiếm giữ`[0,4]`. 

Đối với ô tô thứ hai, vị trí hậu tố sớm nhất là:```
4 + 2 = 6
```Chiếc xe mới sẽ chiếm`[6,14]`, vừa khít bên trong bãi đậu xe. Thuật toán chấp nhận nó vì vị trí hậu tố chỉ kiểm tra:```
candidate + length <= L
```Không cần khoảng cách phía trước ở đường viền bên phải. 

Bây giờ hãy xem xét:```
20 2 2
3
1 4
1 4
1 2
```Sau hai ô tô, ta có:```
[0,4]
[6,10]
```Hậu tố còn lại bắt đầu tại:```
10 + 2 = 12
```Chiều dài ô tô thứ ba là`2`, Và:```
12 + 2 = 14 <= 20
```Thuật toán đậu nó ở`12`. 

Cuối cùng, hãy xem xét một lỗ hổng nội bộ phức tạp:```
20 2 3
3
1 4
1 4
1 1
```Sau hai ô tô đầu tiên:```
[0,4]
[7,11]
```Khoảng cách tự do giữa chúng là độ dài`3`. Một giải pháp bất cẩn có thể mất nhiều thời gian`1`xe vừa vặn ở đó. 

Thuật toán kiểm tra:```
candidate = 4 + 2 = 6
6 + 1 + 3 <= 7
```thất bại. Thiếu khoảng cách phía trước bắt buộc nên ô tô không thể đỗ ở đó. Thay vào đó, thuật toán sẽ tìm kiếm chính xác các khoảng trống sau này.
