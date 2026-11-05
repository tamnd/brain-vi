---
title: "CF 1183G - Hộp Kẹo (bản cứng)"
description: "Mỗi kẹo thuộc về một loại. Với mỗi loại, chúng ta biết có bao nhiêu viên kẹo thuộc loại đó tồn tại và trong số những viên kẹo đó có bao nhiêu viên kẹo có f = 1. Chúng ta muốn xây dựng một món quà bằng cách chọn một số viên kẹo."
date: "2026-06-13T11:40:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1183
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 570 (Div. 3)"
rating: 2000
weight: 1183
solve_time_s: 271
verified: true
draft: false
---

[CF 1183G - Candy Box (phiên bản cứng)](https://codeforces.com/problemset/problem/1183/G) 

**Xếp hạng:** 2000 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi kẹo thuộc về một loại. Với mỗi loại, chúng ta biết có bao nhiêu loại kẹo thuộc loại đó và trong số đó có bao nhiêu loại`f = 1`. 

Chúng tôi muốn xây dựng một món quà bằng cách chọn một số loại kẹo. Nếu chúng ta lấy kẹo từ nhiều loại thì số lượng kẹo lấy từ mỗi loại được chọn phải phân biệt theo từng cặp. Ví dụ: cấm sử dụng 5 viên kẹo một loại và 5 viên kẹo loại khác. 

Việc tối ưu hóa là từ điển. 

Đầu tiên, hãy tối đa hóa tổng số kẹo trong quà tặng. 

Trong số tất cả các quà tặng có kích thước tối đa, hãy tối đa hóa số lượng kẹo được chọn`f = 1`. 

Một quan sát quan trọng là danh tính kẹo riêng lẻ chỉ quan trọng thông qua hai số liệu thống kê cho mỗi loại:`cnt[type]`= tổng số kẹo loại đó.`good[type]`= kẹo loại đó với`f = 1`. 

Nếu chúng ta quyết định lấy`k`kẹo từ một loại, đóng góp tốt nhất có thể cho mục tiêu thứ hai là`min(k, good[type])`, bởi vì chúng ta luôn có thể chọn tất cả kẹo tốt có sẵn trước và lấp đầy phần còn lại bằng kẹo xấu nếu cần. 

Tổng của tất cả`n`trên các truy vấn nhiều nhất là`2 · 10^5`. Điều đó ngay lập tức loại trừ mọi thứ bậc hai về số lượng loại. Chúng tôi cần đại khái`O(n log n)`cho mỗi truy vấn hoặc tốt hơn. 

Phần tinh tế là mục tiêu đầu tiên chỉ phụ thuộc vào tần suất loại, trong khi mục tiêu thứ hai phụ thuộc vào số lượng kẹo ngon mà mỗi loại chứa. Một giải pháp tham lam tối đa hóa số kẹo tốt trước tiên có thể phá hủy kích thước quà tặng tối đa có thể. 

Coi như:```
Type A: cnt=5, good=0
Type B: cnt=5, good=5
```Tổng kích thước quà tặng tối đa có được bằng cách đếm`(5,4)`tổng cộng`9`. 

Chiến lược tham lam thích loại có nhiều kẹo ngon có thể chỉ định`5`gõ B và bỏ qua loại A, chỉ lấy`5`kẹo, điều này không tối ưu. 

Một sai lầm dễ mắc phải khác là cho rằng nếu một loại có tần số`c`, sau đó bất cứ khi nào chúng tôi gán kích thước`k ≤ c`, sự đóng góp cho kẹo tốt chính xác là`good[type]`. Điều đó là sai. 

Ví dụ:```
Type A: cnt=10, good=8
```Nếu chúng ta chỉ định kích thước`k=3`, chúng tôi chỉ có thể đóng góp`3`kẹo ngon thì không`8`. 

Đóng góp đúng là`min(k, good[type])`. 

Trường hợp cạnh thứ ba xuất hiện khi nhiều loại có cùng tần số. 

Ví dụ:```
cnt = [3,3,3]
```Các kích thước được chỉ định riêng biệt không thể`(3,3,3)`. 

Nhiệm vụ tốt nhất là`(3,2,1)`với tổng kích thước`6`. 

Bất kỳ triển khai nào xử lý tần số một cách độc lập sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực là quyết định, đối với mỗi loại, cần lấy bao nhiêu viên kẹo. Nếu có`m`loại, mỗi loại có thể đóng góp bất kỳ số tiền nào từ`0`theo tần số của nó. Sau đó, chúng tôi cần kiểm tra xem tất cả các số được chọn có khác biệt hay không và tính toán cả hai mục tiêu. 

Ngay cả đối với`m = 50`, không gian tìm kiếm này mang tính thiên văn. Với tối đa`2 · 10^5`kẹo nói chung, vũ lực là hoàn toàn không khả thi. 

Quan sát quan trọng là chỉ có tần số mới quan trọng đối với mục tiêu đầu tiên. 

Giả sử tần số loại là:```
c1, c2, c3, ...
```Ta muốn chọn các số nguyên dương phân biệt```
x1, x2, x3, ...
```như vậy```
0 ≤ xi ≤ ci
```và tổng của tất cả`xi`được tối đa hóa. 

Đây là vấn đề tham lam tương tự như phiên bản dễ dàng. Nếu chúng ta xử lý các tần số có thể từ lớn nhất đến nhỏ nhất thì bất cứ khi nào một số loại có thể hỗ trợ cùng một kích thước, chúng ta nên chỉ định kích thước đó cho chính xác một loại. Sau khi ấn định kích thước`s`, kích thước được chỉ định tiếp theo phải hoàn toàn nhỏ hơn`s`. 

Một cách thuận tiện để thực hiện điều này là nhóm các loại theo tần số và quét tần số xuống dưới. Ở tần số`f`, tất cả các loại có số lượng bằng`f`trở nên có sẵn. Trong số tất cả các loại hiện có, chúng tôi chỉ định kích thước`f`đến một trong số họ. 

Đối với phiên bản dễ dàng, loại nào có sẵn sẽ nhận được kích thước không quan trọng`f`. 

Đối với phiên bản cứng, điều này quan trọng vì mục tiêu thứ hai phụ thuộc vào loại đã chọn. 

Giả sử chúng ta hiện đang chỉ định kích thước`f`. 

Bất kỳ loại có sẵn có tần số ít nhất`f`, vì vậy nó có thể nhận được kích thước này một cách hợp pháp. 

Nếu một loại có`good = g`, sau đó gán kích thước`f`đóng góp```
min(f, g)
```kẹo tốt. 

Vì mục tiêu đầu tiên đã được cố định bằng cách gán kích thước`f`, chúng ta nên cung cấp kích thước này cho loại tối đa hóa có sẵn`min(f, g)`. 

Điều này biến vấn đề thành việc quét hàng đợi ưu tiên. 

Khi tần số giảm từ`n`ĐẾN`1`, chúng tôi duy trì tất cả các loại có tổng số lượng ít nhất bằng tần số hiện tại. Trong đó ta chọn loại có giá trị lớn nhất`min(f, good)`. 

Mỗi loại vào heap một lần và rời khỏi heap một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(m) | Quá chậm | 
| Tham lam tối ưu + Heap | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Nén thông tin theo loại 

Với mỗi loại hãy tính:```
cnt[type]
good[type]
```Ở đâu`good[type]`đếm kẹo với`f = 1`. 

Chỉ những giá trị này mới quan trọng sau đó. 

### 2. Nhóm các loại theo tần suất 

Đối với mọi loại hiện có, hãy đặt nó`good[type]`giá trị vào một nhóm tương ứng với tần số của nó:```
bucket[cnt[type]]
```Khi quá trình quét đạt đến tần số`f`, tất cả các loại trong`bucket[f]`trở nên có sẵn. 

### 3. Tần số quét từ lớn đến nhỏ 

Tần số quá trình:```
f = n, n-1, ..., 1
```Trước khi xử lý tần số`f`, chèn vào hàng ưu tiên tất cả các loại có tổng số bằng`f`. 

Tại thời điểm này, mọi loại trong hàng đợi đều có tần suất ít nhất`f`, vì vậy bất kỳ ai trong số họ cũng có thể nhận được kích thước bài tập`f`. 

### 4. Chọn loại phù hợp với kích thước`f`Nếu hàng đợi không trống, hãy chỉ định kích thước`f`thành đúng một loại có sẵn. 

Nhiệm vụ này góp phần:```
f
```với tổng số lượng quà tặng. 

Đối với mục tiêu thứ hai, một loại có`good = g`đóng góp:```
min(f, g)
```kẹo tốt. 

Trong số tất cả các loại có sẵn, hãy chọn loại tối đa hóa giá trị này. 

Loại bỏ loại đó khỏi hàng đợi. 

### 5. Tích lũy câu trả lời 

Thêm:```
f
```với kích thước quà tặng tối đa. 

Thêm vào:```
min(f, g)
```đến số lượng kẹo tốt tối đa. 

Tiếp tục cho đến khi tần số`1`. 

### Tại sao nó hoạt động 

Khi quét ở tần số`f`, mọi loại trong tập hoạt động đều có thể nhận được kích thước bài tập một cách hợp pháp`f`, vì tần số ban đầu của nó ít nhất là`f`. 

Đối số tham lam phiên bản dễ dàng cho thấy rằng việc gán kích thước`f`bất cứ khi nào có thể là cần thiết để tối đa hóa tổng số kẹo. Việc bỏ qua nhiệm vụ như vậy không bao giờ có thể được bù đắp sau này vì tất cả các kích thước trong tương lai đều nhỏ hơn. 

Một lần kích thước`f`là cố định, mọi loại hoạt động đều mang lại sự đóng góp như nhau`f`tới mục tiêu đầu tiên. Sự khác biệt duy nhất còn lại là sự đóng góp thứ yếu`min(f, good)`. 

Chọn tối đa hóa loại hoạt động`min(f, good)`không thể làm tổn hại đến mục tiêu đầu tiên và tối ưu cục bộ cho mục tiêu thứ hai. Vì mỗi tần số được chỉ định chính xác một lần và các phép gán trong tương lai sử dụng kích thước nhỏ hơn hoàn toàn nên đối số trao đổi này áp dụng độc lập ở mỗi bước. Việc thay thế loại đã chọn bằng bất kỳ loại hoạt động nào khác sẽ không làm tăng mục tiêu phụ. 

Do đó, thuật toán đồng thời tối đa hóa tổng kích thước quà tặng và, trong số tất cả các giải pháp như vậy, tối đa hóa số lượng kẹo được chọn với`f = 1`. 

## Giải pháp Python```python
import sys
from collections import defaultdict
import heapq

input = sys.stdin.readline

def solve():
    q = int(input())
    ans = []

    for _ in range(q):
        n = int(input())

        cnt = defaultdict(int)
        good = defaultdict(int)

        for _ in range(n):
            a, f = map(int, input().split())
            cnt[a] += 1
            good[a] += f

        buckets = [[] for _ in range(n + 1)]

        for t in cnt:
            buckets[cnt[t]].append(good[t])

        heap = []
        total_candies = 0
        total_good = 0

        for freq in range(n, 0, -1):
            for g in buckets[freq]:
                heapq.heappush(heap, -g)

            if heap:
                g = -heapq.heappop(heap)

                total_candies += freq
                total_good += min(freq, g)

        ans.append(f"{total_candies} {total_good}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên chuyển đổi dữ liệu kẹo thô thành số liệu thống kê theo từng loại. Sau đó, mỗi loại được biểu thị bằng hai số: tổng tần số và số kẹo ngon.`buckets[f]`lưu trữ tất cả các loại có tần số bằng`f`. Trong quá trình quét xuống, chèn`buckets[f]`vào heap làm cho mọi loại có tần số ít nhất`f`có sẵn. 

Các cửa hàng đống`good`tính. Python cung cấp vùng heap tối thiểu, vì vậy các giá trị âm được sử dụng để mô phỏng vùng heap tối đa. 

Khi xử lý tần số`freq`, loại bỏ lớn nhất`good`giá trị tương ứng với việc chọn loại tối đa hóa`min(freq, good)`. Vì mọi loại hoạt động đều đã hỗ trợ kích thước`freq`, đây chính xác là sự lựa chọn tham lam mà chứng minh yêu cầu. 

Một chi tiết triển khai dễ bị bỏ sót là vùng heap chỉ lưu trữ`good`, không phải tần số. Một loại được chèn đúng một lần, tại thời điểm tần số thực của nó có sẵn. Sau khi chèn, nó vẫn đủ điều kiện cho tất cả các kích thước bài tập nhỏ hơn, do đó không cần thêm thông tin. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1
1 1
2 1
2 1
```Thống kê loại: 

| Loại | cnt | tốt | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 2 | 2 | 

Quét: 

| tần số | chèn giá trị tốt | đống trước khi chọn | đã chọn g | tổng số kẹo | tổng số tốt | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2, 2 | [2,2] | 2 | 2 | 2 | 
| 1 | không | [2] | 2 | 3 | 3 | 

Kết quả:```
3 3
```Các kích thước được chỉ định là`(2,1)`. Sự khác biệt được tôn trọng và tất cả các loại kẹo được chọn đều tốt. 

### Ví dụ 2 

đầu vào:```
9
2 0
2 0
4 1
4 1
4 1
7 0
7 1
7 0
7 1
```Thống kê loại: 

| Loại | cnt | tốt | 
| --- | --- | --- | 
| 2 | 2 | 0 | 
| 4 | 3 | 3 | 
| 7 | 4 | 2 | 

Quét: 

| tần số | chèn vào | đống trước khi chọn | đã chọn g | kẹo | tốt | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 2 | [2] | 2 | 4 | 2 | 
| 3 | 3 | [3] | 3 | 7 | 5 | 
| 2 | 0 | [0] | 0 | 9 | 5 | 
| 1 | không | trống | - | 9 | 5 | 

Kết quả:```
9 5
```Các kích thước được chỉ định là`(4,3,2)`, cho tổng kích thước tối đa`9`. Trong số tất cả các nhiệm vụ như vậy, số lượng kẹo tốt tối đa là`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi loại vào và ra khỏi heap một lần | 
| Không gian | O(n) | Các nhóm, bản đồ tần số và vùng lưu trữ dữ liệu O(n) | 

Tổng số kẹo trên tất cả các truy vấn nhiều nhất là`2 · 10^5`. MỘT`O(n log n)`giải pháp hoạt động thoải mái trong thời gian giới hạn và mức sử dụng bộ nhớ tuyến tính thấp hơn nhiều so với bộ nhớ khả dụng. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import defaultdict
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    q = int(input())
    out = []

    for _ in range(q):
        n = int(input())

        cnt = defaultdict(int)
        good = defaultdict(int)

        for _ in range(n):
            a, f = map(int, input().split())
            cnt[a] += 1
            good[a] += f

        buckets = [[] for _ in range(n + 1)]

        for t in cnt:
            buckets[cnt[t]].append(good[t])

        heap = []
        s1 = 0
        s2 = 0

        for freq in range(n, 0, -1):
            for g in buckets[freq]:
                heapq.heappush(heap, -g)

            if heap:
                g = -heapq.heappop(heap)
                s1 += freq
                s2 += min(freq, g)

        out.append(f"{s1} {s2}")

    return "\n".join(out)

# provided sample
assert run(
"""3
8
1 0
4 1
2 0
4 1
5 1
6 1
3 0
2 0
4
1 1
1 1
2 1
2 1
9
2 0
2 0
4 1
4 1
4 1
7 0
7 1
7 0
7 1
"""
) == """3 3
3 3
9 5"""

# minimum size
assert run(
"""1
1
1 0
"""
) == "1 0"

# all candies same type
assert run(
"""1
5
1 1
1 1
1 0
1 0
1 0
"""
) == "5 2"

# equal frequencies requiring decreasing assignments
assert run(
"""1
6
1 1
1 1
2 1
2 1
3 1
3 1
"""
) == "3 3"

# choose type with larger good count
assert run(
"""1
10
1 1
1 1
1 1
1 1
1 1
2 1
2 0
2 0
2 0
2 0
"""
) == "9 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một loại kẹo, một loại |`1 0`| Ranh giới tối thiểu | 
| Tất cả các loại kẹo cùng loại |`5 2`| Phân bổ tần số đơn | 
| Ba loại có tần số bằng nhau 2 |`3 3`| Số lượng riêng biệt`(2,1)`| 
| Tần số 5 và 5 với số lượng tốt khác nhau |`9 5`| Mục tiêu tham lam thứ cấp | 

## Vỏ cạnh 

Hãy xem xét:```
1
6
1 1
1 1
2 1
2 1
3 1
3 1
```Mọi tần số đều bằng`2`. Một giải pháp bất cẩn có thể lấy cả ba loại với kích thước`2`, thu được`6`, vi phạm yêu cầu về tính khác biệt. Quá trình quét sẽ chèn cả ba loại khi`freq=2`, chọn một trong số chúng cho kích thước`2`, thì tại`freq=1`chọn kích thước khác`1`. Câu trả lời cuối cùng là`3 3`. 

Coi như:```
1
10
1 1
1 1
1 1
1 1
1 1
2 1
2 0
2 0
2 0
2 0
```Cả hai loại đều có tần số`5`, nhưng người ta còn có nhiều kẹo ngon hơn. Tổng kích thước tối đa được cố định ở`5 + 4 = 9`. Khi gán kích thước`5`, heap chọn loại có`good=5`, đóng góp 5 viên kẹo ngon. Loại thứ hai nhận kích thước`4`, không đóng góp thêm kẹo tốt nào. Câu trả lời là`9 5`. 

Coi như:```
1
5
1 1
1 1
1 1
1 1
1 1
```Chỉ có một loại tồn tại. Heap nhận được một mục nhập duy nhất với tần suất`5`. Việc quét chỉ định kích thước`5`một lần và không có gì sau đó. Câu trả lời là`5 5`, xác nhận rằng các tần số không sử dụng không cần gán. 

Coi như:```
1
4
1 0
1 0
2 1
2 1
```Kích thước quà tặng tối đa là`3`, thu được bằng bài tập`(2,1)`. Thuật toán gán kích thước`2`đến loại có hai viên kẹo ngon, đóng góp`2`. Loại còn lại nhận size`1`, đóng góp`0`. Câu trả lời trở thành`3 2`, đây là món quà tốt nhất có thể trong số tất cả các món quà có kích thước tối đa.
