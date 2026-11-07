---
title: "CF 131E - Một nhiệm vụ khác với Nữ hoàng"
description: "Chúng ta được giao vị trí quân hậu trên bàn cờ. Quân hậu tấn công theo tám hướng: trái, phải, trên, dưới và bốn đường chéo. Nữ hoàng không tấn công mọi nữ hoàng theo một hướng, chỉ có người đầu tiên gặp dọc theo tia đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 131
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 95 (Div. 2)"
rating: 1700
weight: 131
solve_time_s: 158
verified: true
draft: false
---

[CF 131E - Một nhiệm vụ khác với Nữ hoàng](https://codeforces.com/problemset/problem/131/E) 

**Đánh giá:** 1700 
**Thẻ:** sắp xếp 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao vị trí quân hậu trên bàn cờ. Quân hậu tấn công theo tám hướng: trái, phải, trên, dưới và bốn đường chéo. Nữ hoàng không tấn công mọi nữ hoàng theo một hướng, chỉ có người đầu tiên gặp dọc theo tia đó. 

Với mỗi quân hậu, chúng ta phải đếm xem nó tấn công trực tiếp bao nhiêu quân hậu khác. Giá trị đó nằm trong khoảng từ 0 đến 8 vì chỉ có tám hướng có thể. Sau khi tính toán con số này cho mỗi quân hậu, chúng ta xây dựng một mảng trong đó`t[i]`chính xác là số quân hậu tấn công`i`những hoa hậu khác. 

Kích thước bàn cờ và số quân hậu đều lớn bằng`10^5`. Một giải pháp bậc hai ngay lập tức bị loại trừ. Việc kiểm tra từng cặp quân hậu sẽ yêu cầu khoảng`10^10`so sánh trong trường hợp xấu nhất, vượt xa những gì phù hợp trong hai giây. Giải pháp phải ở gần`O(m log m)`hoặc tốt hơn. 

Phần khó chính là câu “chỉ có nữ hoàng đầu tiên đi theo hướng này”. Một cách triển khai đơn giản có thể đếm mọi quân hậu chia sẻ cùng một hàng, cột hoặc đường chéo. Điều đó sẽ sai. 

Hãy xem xét ví dụ này:```
5 3
2 1
2 3
2 5
```Các nữ hoàng đều ở cùng một hàng. 

Số lần tấn công chính xác là: 

-`(2,1)`tấn công`(2,3)`-`(2,3)`tấn công cả hai người hàng xóm 
-`(2,5)`tấn công`(2,3)`Vậy số đếm là`[1,2,1]`, và đầu ra là:```
0 2 1 0 0 0 0 0 0
```Một giải pháp bất cẩn khi đếm mọi quân hậu trong cùng một hàng sẽ đưa ra số đếm không chính xác`[2,2,2]`. 

Xử lý theo đường chéo là một nguồn lỗi phổ biến khác. Hai quân hậu nằm trên cùng một đường chéo chỉ khi một trong hai quân hậu`r - c`hoặc`r + c`trận đấu. 

Ví dụ:```
5 2
1 5
3 3
```Những nữ hoàng này nằm trên cùng một`/`đường chéo vì:```
1 + 5 = 6
3 + 3 = 6
```Một giải pháp chỉ kiểm tra`r - c`sẽ bỏ lỡ cuộc tấn công này. 

Một trường hợp tinh tế khác xảy ra khi một số quân hậu có chung một dòng nhưng không ở cạnh nhau sau khi sắp xếp.```
8 4
4 1
4 3
4 5
4 8
```Chỉ có những nữ hoàng lân cận dọc hàng mới tấn công lẫn nhau. Các nữ hoàng ở cột`1`Và`5`không tương tác vì`(4,3)`chặn đường đi. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi quân hậu, hãy quét tất cả các quân hậu khác và kiểm tra xem chúng có nằm ở một trong tám hướng hay không. Nếu họ làm vậy, hãy xác định xem có nữ hoàng nào khác chặn đường hay không. Điều này hoạt động hợp lý vì chuyển động cờ vua mang tính hình học và dễ kiểm tra bằng số học tọa độ. 

Vấn đề là chi phí. có`m`quân hậu, vì vậy việc kiểm tra tất cả các cặp đã mất nhiều thời gian`O(m^2)`. Với`m = 10^5`, điều đó trở thành`10^10`kiểm tra cặp trước khi xem xét việc chặn logic. 

Cấu trúc của phong trào nữ hoàng mang lại một cách tiếp cận tốt hơn nhiều. Một nữ hoàng chỉ tương tác với những chia sẻ của nữ hoàng: 

- cùng một hàng 
- cùng một cột 
- giống nhau`r - c`đường chéo 
- giống nhau`r + c`đường chéo 

Bên trong bất kỳ hàng nào trong số này, chỉ những quân hậu liền kề sau khi sắp xếp mới có thể tấn công lẫn nhau. Nếu hai quân hậu có một quân hậu khác ở giữa chúng trên cùng một hàng, thì quân hậu ở gần hơn sẽ chặn quân hậu ở xa hơn. 

Sự quan sát đó thay đổi hoàn toàn vấn đề. Thay vì so sánh từng cặp, chúng ta nhóm quân hậu theo dòng: 

- nhóm hàng theo`r`- nhóm cột theo`c`- nhóm đường chéo bởi`r - c`- nhóm chống đường chéo bởi`r + c`Trong mỗi nhóm, chúng tôi sắp xếp các vị trí dọc theo dòng. Sau đó, mỗi cặp liền kề đóng góp chính xác một đòn tấn công theo mỗi hướng. 

Ví dụ: nếu một hàng chứa quân hậu ở các cột:```
1, 4, 7
```sau đó:

-`1`tấn công`4`-`4`tấn công cả hai`1`Và`7`-`7`tấn công`4`Cặp đôi`(1,7)`không bao giờ tương tác trực tiếp. 

Mỗi quân hậu tham gia vào tối đa hai mối quan hệ liền kề trên mỗi loại đường, do đó toàn bộ quá trình tính toán sẽ trở nên hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(1) | Quá chậm | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vị trí quân hậu và gán cho mỗi quân hậu một chỉ mục. 

Chúng ta cần lưu trữ ổn định vì số lần tấn công cuối cùng thuộc về từng quân hậu cụ thể. 
2. Tạo bốn bản đồ băm. 

Mỗi bản đồ nhóm các nữ hoàng chia sẻ một đường tấn công: 

- theo hàng`r`- cột theo`c`- các đường chéo chính bằng`r - c`- chống đường chéo bằng`r + c`3. Xếp từng quân hậu vào cả bốn nhóm. 

Mỗi mục được lưu trữ chứa đủ thông tin để sắp xếp các quân hậu dọc theo dòng đó và khôi phục chỉ số quân hậu sau đó. 
4. Đối với mỗi nhóm hàng, hãy sắp xếp theo cột. 

Các quân hậu liền kề sau khi sắp xếp chính là các quân hậu nhìn thấy nhau theo chiều ngang. 
5. Đối với mỗi cặp liền kề trong hàng đã sắp xếp, hãy tăng số lần tấn công của cả hai quân hậu. 

Nếu quân hậu`A`Và`B`liên tiếp theo thứ tự sắp xếp, không có quân hậu nào nằm giữa chúng nên chúng tấn công trực tiếp lẫn nhau. 
6. Lặp lại quy trình tương tự cho các nhóm cột. 

Ở đây chúng tôi sắp xếp theo hàng vì chuyển động theo chiều dọc. 
7. Lặp lại cho`r - c`các nhóm đường chéo. 

Sắp xếp theo hàng là đủ vì hàng và cột tăng cùng nhau dọc theo đường chéo này. 
8. Lặp lại cho`r + c`các nhóm đường chéo. 

Một lần nữa, sắp xếp theo hàng sẽ cho ra thứ tự đúng dọc theo dòng. 
9. Sau khi tất cả bốn cấu trúc được xử lý, mỗi quân hậu có tổng số quân hậu bị tấn công. 
10. Xây dựng một mảng`ans[0...8]`. 

Đối với mỗi nữ hoàng có số lần tấn công`k`, tăng`ans[k]`. 
11. In mảng tần số. 

### Tại sao nó hoạt động 

Bên trong bất kỳ hàng, cột hoặc đường chéo nào, quân hậu chỉ tấn công những người hàng xóm gần nhất của chúng theo mỗi hướng. Việc sắp xếp các quân hậu dọc theo một đường sẽ hiển thị chính xác những hàng xóm gần nhất đó là các phần tử liền kề. 

Mỗi mối quan hệ tấn công trực tiếp xuất hiện một lần dưới dạng cặp liền kề trong đúng một nhóm dòng. Không có đòn tấn công nào bị bỏ lỡ vì mọi đòn tấn công hợp lệ đều nằm trên một trong bốn loại đường. Không có đòn tấn công bổ sung nào được thêm vào vì các quân hậu không liền kề luôn có một quân hậu khác chặn đường. 

Vì chúng tôi tăng cả hai điểm cuối của mỗi cặp liền kề nên mỗi quân hậu sẽ tích lũy chính xác số lượng quân hậu mà nó tấn công. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def process(groups, cnt):
    for arr in groups.values():
        arr.sort()

        for i in range(len(arr) - 1):
            _, a = arr[i]
            _, b = arr[i + 1]

            cnt[a] += 1
            cnt[b] += 1

def solve():
    n, m = map(int, input().split())

    rows = defaultdict(list)
    cols = defaultdict(list)
    diag1 = defaultdict(list)   # r - c
    diag2 = defaultdict(list)   # r + c

    cnt = [0] * m

    for idx in range(m):
        r, c = map(int, input().split())

        rows[r].append((c, idx))
        cols[c].append((r, idx))
        diag1[r - c].append((r, idx))
        diag2[r + c].append((r, idx))

    process(rows, cnt)
    process(cols, cnt)
    process(diag1, cnt)
    process(diag2, cnt)

    ans = [0] * 9

    for x in cnt:
        ans[x] += 1

    print(*ans)

solve()
```Giải pháp xoay quanh`process`chức năng. Mỗi nhóm có các quân hậu chia sẻ một đường tấn công. Các bộ dữ liệu được lưu trữ dưới dạng:```
(position_along_line, queen_index)
```Sau khi sắp xếp, các nữ hoàng liên tiếp trở thành hàng xóm trực tiếp dọc theo dòng đó. Đối với mỗi cặp liền kề, cả hai quân hậu đều có được một đòn tấn công. 

Logic tương tự áp dụng cho các hàng, cột và cả hai đường chéo vì tất cả chuyển động của quân hậu đều là tuyến tính. Sự khác biệt duy nhất là cách các nhóm được thành lập. 

sử dụng`r - c`xác định một hướng chéo vì giá trị này không đổi dọc theo`\`đường chéo. sử dụng`r + c`xác định hướng chéo khác vì giá trị đó không đổi dọc theo`/`đường chéo. 

Một chi tiết triển khai tinh tế là tăng cả hai điểm cuối của mỗi cặp liền kề. Các cuộc tấn công của nữ hoàng ở đây là đối xứng. Nếu như`A`thấy`B`, sau đó`B`cũng thấy`A`từ hướng ngược lại. 

Một lỗi dễ mắc phải khác là sắp xếp các đường chéo không chính xác. Dọc theo một đường chéo cố định, việc sắp xếp theo hàng cũng tự động sắp xếp theo cột, vì vậy chỉ cần một tọa độ là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 4
4 3
4 8
6 5
1 6
```### Xử lý hàng 

| Hàng | Nữ hoàng được sắp xếp | Đã thêm các cuộc tấn công | 
| --- | --- | --- | 
| 4 | (3), (8) | cả +1 | 
| 6 | (5) | không | 
| 1 | (6) | không | 

Số lượng hiện tại: 

| Nữ hoàng | Đếm | 
| --- | --- | 
| (4,3) | 1 | 
| (4,8) | 1 | 
| (6,5) | 0 | 
| (1,6) | 0 | 

### Xử lý đường chéo 

Nữ hoàng`(4,8)`Và`(1,6)`chia sẻ`r + c = 12`. 

| Đường chéo | Nữ hoàng được sắp xếp | Đã thêm các cuộc tấn công | 
| --- | --- | --- | 
| 12 | (1,6), (4,8) | cả +1 | 

Số đếm cuối cùng: 

| Nữ hoàng | Tổng cộng | 
| --- | --- | 
| (4,3) | 1 | 
| (4,8) | 2 | 
| (6,5) | 0 | 
| (1,6) | 1 | 

Mảng tần số:```
0 3 0 1 0 0 0 0 0
```Ví dụ này cho thấy các cuộc tấn công có thể đến từ các loại đường khác nhau cùng một lúc. 

### Ví dụ 2 

đầu vào:```
5 4
3 1
3 3
3 5
1 3
```### Xử lý hàng 

| Hàng | Nữ hoàng được sắp xếp | Đã thêm các cuộc tấn công | 
| --- | --- | --- | 
| 3 | (1), (3), (5) | hàng xóm tấn công | 

Đếm sau hàng: 

| Nữ hoàng | Đếm | 
| --- | --- | 
| (3,1) | 1 | 
| (3,3) | 2 | 
| (3,5) | 1 | 
| (1,3) | 0 | 

### Xử lý cột 

Cột`3`chứa`(1,3)`Và`(3,3)`. 

| Cột | Nữ hoàng được sắp xếp | Đã thêm các cuộc tấn công | 
| --- | --- | --- | 
| 3 | (1), (3) | cả +1 | 

Số đếm cuối cùng: 

| Nữ hoàng | Tổng cộng | 
| --- | --- | 
| (3,1) | 1 | 
| (3,3) | 3 | 
| (3,5) | 1 | 
| (1,3) | 1 | 

Đầu ra:```
0 3 0 1 0 0 0 0 0
```Dấu vết này chứng tỏ tại sao chỉ có các nữ hoàng liền kề mới quan trọng.`(3,1)`không tấn công`(3,5)`bởi vì`(3,3)`chặn đường đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Mỗi nữ hoàng tham gia phân loại trong bốn nhóm | 
| Không gian | O(m) | Bản đồ băm và bộ đếm tấn công lưu trữ tất cả quân hậu một lần trên mỗi cấu trúc | 

Tổng lượng dữ liệu được lưu trữ là tuyến tính theo số lượng quân hậu. Sắp xếp chiếm ưu thế trong thời gian chạy. Vì tổng kích thước của tất cả các nhóm là`m`, tổng chi phí phân loại vẫn còn`O(m log m)`, dễ dàng phù hợp với giới hạn của`10^5`nữ hoàng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def process(groups, cnt):
        for arr in groups.values():
            arr.sort()

            for i in range(len(arr) - 1):
                _, a = arr[i]
                _, b = arr[i + 1]

                cnt[a] += 1
                cnt[b] += 1

    n, m = map(int, input().split())

    rows = defaultdict(list)
    cols = defaultdict(list)
    diag1 = defaultdict(list)
    diag2 = defaultdict(list)

    cnt = [0] * m

    for idx in range(m):
        r, c = map(int, input().split())

        rows[r].append((c, idx))
        cols[c].append((r, idx))
        diag1[r - c].append((r, idx))
        diag2[r + c].append((r, idx))

    process(rows, cnt)
    process(cols, cnt)
    process(diag1, cnt)
    process(diag2, cnt)

    ans = [0] * 9

    for x in cnt:
        ans[x] += 1

    return " ".join(map(str, ans))

# provided sample
assert run(
"""8 4
4 3
4 8
6 5
1 6
"""
) == "0 3 0 1 0 0 0 0 0"

# single queen
assert run(
"""1 1
1 1
"""
) == "1 0 0 0 0 0 0 0 0"

# three queens in one row
assert run(
"""5 3
2 1
2 3
2 5
"""
) == "0 2 1 0 0 0 0 0 0"

# diagonal chain
assert run(
"""5 4
1 1
2 2
3 3
4 4
"""
) == "0 2 2 0 0 0 0 0 0"

# isolated queens
assert run(
"""10 4
1 1
2 4
5 2
9 8
"""
) == "4 0 0 0 0 0 0 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nữ hoàng độc thân |`1 0 0 0 0 0 0 0 0`| Kích thước đầu vào tối thiểu | 
| Ba nữ hoàng liên tiếp |`0 2 1 0 0 0 0 0 0`| Chặn giữa các nữ hoàng không liền kề | 
| Chuỗi chéo |`0 2 2 0 0 0 0 0 0`| Nhóm đường chéo đúng | 
| Nữ hoàng biệt lập |`4 0 0 0 0 0 0 0 0`| Không có cuộc tấn công tình cờ | 

## Vỏ cạnh 

Xem xét nhiều quân hậu trên cùng một dòng có tính năng chặn:```
5 3
2 1
2 3
2 5
```Nhóm hàng trở thành:```
[(1,A), (3,B), (5,C)]
```Thuật toán xử lý các cặp liền kề: 

-`A-B`-`B-C`Nó không bao giờ kết nối`A-C`bởi vì chúng không liền kề sau khi sắp xếp. Số lần tấn công kết quả là:```
A = 1
B = 2
C = 1
```cái nào đúng. 

Bây giờ hãy xem xét các cuộc tấn công chống đường chéo:```
5 2
1 5
3 3
```Cả hai nữ hoàng đều thỏa mãn:```
r + c = 6
```Vì vậy họ nhập giống nhau`diag2`nhóm. Sau khi sắp xếp, chúng ở cạnh nhau và cả hai đều nhận được một đòn tấn công. Đầu ra cuối cùng là:```
0 2 0 0 0 0 0 0 0
```Điều này xác nhận rằng cả hai hướng chéo đều được xử lý độc lập. 

Cuối cùng, hãy xem xét các con hậu có chung đường chéo nhưng bị chặn:```
6 3
1 1
3 3
5 5
```Nhóm đường chéo được sắp xếp là:```
[(1,A), (3,B), (5,C)]
```Chỉ các cặp liền kề mới đóng góp: 

-`A-B`-`B-C`các nữ hoàng`A`Và`C`không bao giờ tấn công lẫn nhau bởi vì`B`chặn đường đi. Số đếm trở thành:```
1 2 1
```phù hợp chính xác với chuyển động cờ vua thực tế.
