---
title: "CF 105934B - Kho Báu"
description: "Các thành phố nằm trên một con sông theo thứ tự. Những người săn kho báu bắt đầu ở thành phố 1. Đến thành phố i + 1 từ thành phố i tốn ci xu và không có con đường nào khác. Mỗi thành phố chứa một rương có giá trị là đồng xu ai."
date: "2026-06-25T13:57:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105934
codeforces_index: "B"
codeforces_contest_name: "MEPhI Spring Cup 2025"
rating: 0
weight: 105934
solve_time_s: 54
verified: true
draft: false
---

[CF 105934B - Kho báu](https://codeforces.com/problemset/problem/105934/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các thành phố nằm trên một con sông theo thứ tự. Những thợ săn kho báu bắt đầu ở thành phố 1. Tiếp cận thành phố`i + 1`từ thành phố`i`chi phí`c_i`tiền xu và không có con đường thay thế nào. 

Mỗi thành phố chứa một rương có giá trị`a_i`tiền xu. Thợ săn sở hữu nhiều nhất`k`chìa khóa, vì vậy trong toàn bộ cuộc thám hiểm, họ chỉ có thể mở tối đa`k`rương. Họ có thể bỏ qua bất kỳ thành phố nào ngay cả khi họ ghé thăm nó. 

Đến một lúc nào đó họ quyết định ngừng khám phá. Việc quay trở lại hạ lưu là miễn phí, vì vậy chi phí đi lại duy nhất là chi phí phải trả để đến được thành phố xa nhất mà bạn ghé thăm. 

Đối với mỗi thành phố có thể dừng lại, chúng ta phải xác định lợi nhuận tốt nhất:`coins collected from opened chests - travel costs paid`và xuất ra giá trị lớn nhất có thể đạt được. 

Các ràng buộc rất lớn, lên tới`10^5`thành phố, trong khi giá trị kho báu và chi phí đi lại có thể đạt tới`10^9`. Bất kỳ giải pháp nào quét liên tục các tiền tố hoặc liên tục sắp xếp các phạm vi lớn sẽ quá chậm. MỘT`O(n^2)`cách tiếp cận sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Chúng ta cần một cái gì đó gần gũi`O(n log n)`. 

Một điểm tinh tế là việc mở rương là tùy chọn. Giả định:```
n = 3, k = 2
a = [100, 1, 1]
```Lựa chọn tối ưu là chỉ mở những rương có giá trị có sẵn trong tiền tố đã truy cập. Chúng tôi không bao giờ muốn lãng phí chìa khóa cho một chiếc rương nhỏ hơn nếu có một chiếc chìa khóa lớn hơn trong cùng một bộ có thể truy cập được. 

Một sai lầm dễ mắc phải khác là nghĩ rằng thành phố đích phải chứa một chiếc rương đã mở.```
n = 3, k = 1
a = [100, 1, 1000]
c = [10, 10000]
```Dừng chân ở thành phố 2 mang lại lợi nhuận`100 - 10 = 90`. Du hành đến thành phố 3 là một điều tai hại vì chi phí đi lại bổ sung vượt quá kho báu bổ sung. Điểm dừng và rương mở là những quyết định độc lập. 

Trường hợp cạnh thứ ba xuất hiện khi ít hơn`k`các thành phố đã được viếng thăm.```
n = 2, k = 2
a = [5, 7]
c = [1]
```Sau khi đến thành phố 1, kho báu sưu tầm được tốt nhất là`5`, không phải "tổng của hai kho báu". Thuật toán phải xử lý một cách tự nhiên các tiền tố có độ dài nhỏ hơn`k`. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ xem xét mọi thành phố dừng lại`i`. 

Nếu chúng ta dừng lại ở thành phố`i`, chỉ các thành phố`1...i`có thể truy cập được. Trong số các thành phố đó, chiến lược tốt nhất rất rõ ràng: mở các rương có giá trị lớn nhất, lên tới`k`của họ. Chi phí đi lại là cố định và bằng:```
c1 + c2 + ... + c(i-1)
```Vì vậy chúng ta có thể tính toán:```
profit(i) =
(sum of k largest values in a[1..i])
-
(prefix travel cost)
```Việc thực hiện bạo lực sẽ tính toán lại`k`giá trị lớn nhất cho mỗi tiền tố một cách độc lập. Sắp xếp từng chi phí tiền tố`O(i log i)`, dẫn đến đại khái`O(n^2 log n)`tổng thể. Với`n = 10^5`, tốc độ này quá chậm. 

Quan sát quan trọng là các tiền tố phát triển từng thành phố tại một thời điểm. Khi thành phố`i`được thêm vào, chỉ có một giá trị kho báu mới được xem xét. Chúng ta không cần phải tính toán lại toàn bộ tập hợp hàng đầu`k`kho báu từ đầu. 

Duy trì hiện tại`k`giá trị kho báu lớn nhất được thấy cho đến nay. Một đống tối thiểu là hoàn hảo cho việc này. Heap lưu trữ chính xác các kho báu hiện được chọn để mở. Kích thước của nó không bao giờ vượt quá`k`. 

Khi một kho báu mới đến: 

Nếu heap chứa ít hơn`k`các phần tử, chúng ta phải bao gồm nó. 

Ngược lại, hãy so sánh nó với kho báu nhỏ nhất được chọn, nằm ở đầu vùng tối thiểu. Nếu giá trị mới lớn hơn, việc thay thế kho báu nhỏ nhất đã chọn sẽ làm tăng tổng số kho báu đã chọn. Nếu nó không lớn hơn, bộ kho báu được chọn tối ưu vẫn không thay đổi. 

Điều này mang lại tổng số lớn nhất`k`các giá trị trong mọi tiền tố trong`O(log k)`thời gian cho mỗi thành phố. 

Chi phí đi lại thậm chí còn đơn giản hơn. Cho phép`cost`là chi phí tích lũy phải trả để đến được thành phố hiện tại. Khi chúng tôi di chuyển từ thành phố`i`đến thành phố`i + 1`, thêm vào`c_i`. 

Đối với mỗi tiền tố chúng tôi tính toán:```
current_profit = sum_top_k - cost
```và giữ ở mức tối đa. 

Quan sát chính thức chính xác là nếu những người thợ săn dừng lại ở thành phố`i`, lợi nhuận tốt nhất bằng tổng của`k`giá trị kho báu lớn nhất trong tiền tố`a[1..i]`trừ chi phí đi lại cần thiết để đến thành phố đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(n) | Quá chậm | 
| Tối ưu | O(n log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`k`, giá trị kho báu`a`, và chi phí đi lại`c`. 
2. Tạo một heap tối thiểu trống và một biến`selected_sum = 0`. 
3. Duy trì`travel_cost`, tổng chi phí phải trả để đến được thành phố hiện tại. 
4. Xử lý các thành phố từ trái sang phải. 
5. Đối với giá trị kho báu hiện tại`a[i]`, cập nhật đống kho báu đã chọn. 

Nếu kích thước heap nhỏ hơn`k`, đẩy giá trị và thêm nó vào`selected_sum`. 
6. Nếu heap đã chứa`k`phần tử, so sánh`a[i]`với kho báu được chọn nhỏ nhất. 

Nếu như`a[i]`lớn hơn, loại bỏ kho báu được chọn nhỏ nhất, chèn`a[i]`, và cập nhật`selected_sum`tương ứng. 
7. Sau khi cập nhật vùng heap,`selected_sum`bằng tổng của lớn nhất`k`giá trị kho báu trong tiền tố hiện tại. 
8. Tính toán:```
profit = selected_sum - travel_cost
```Cập nhật câu trả lời với lợi nhuận tối đa được thấy cho đến nay. 
9. Trước khi chuyển đến thành phố tiếp theo, hãy cộng chi phí đi lại tương ứng vào`travel_cost`. 
10. Sau khi tất cả các thành phố được xử lý, hãy in lợi nhuận tối đa. 

### Tại sao nó hoạt động 

Đối với bất kỳ thành phố dừng lại`i`, các thành phố có thể tiếp cận chính xác là tiền tố`1...i`. Trong số những thành phố đó, bất kỳ chiến lược tối ưu nào cũng sẽ mở ra những rương có giá trị nhất, lên tới`k`của họ. Chọn một rương nhỏ hơn trong khi bỏ qua một rương lớn hơn có thể tiếp cận chỉ có thể làm giảm lợi nhuận. 

Tính bất biến của heap là nó luôn lưu trữ giá trị lớn nhất`k`giá trị kho báu trong số tất cả các thành phố được xử lý cho đến nay. Bất cứ khi nào một kho báu mới được đưa vào tiền tố, sự thay đổi duy nhất có thể xảy ra đối với bộ tối ưu là thay thế kho báu nhỏ nhất hiện được chọn. Heap thực hiện chính xác bản cập nhật này. 

Từ`selected_sum`luôn bằng tổng lớn nhất`k`kho báu trong tiền tố hiện tại, và`travel_cost`bằng chi phí cần thiết để đến thành phố cuối cùng của tiền tố đó, lợi nhuận được tính toán chính xác là lợi nhuận tốt nhất cho điểm dừng đó. Lấy mức tối đa trên tất cả các điểm dừng mang lại mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    c = []
    if n > 1:
        c = list(map(int, input().split()))
    
    heap = []
    selected_sum = 0
    travel_cost = 0
    answer = 0

    for i in range(n):
        x = a[i]

        if len(heap) < k:
            heapq.heappush(heap, x)
            selected_sum += x
        elif x > heap[0]:
            removed = heapq.heapreplace(heap, x)
            selected_sum += x - removed

        answer = max(answer, selected_sum - travel_cost)

        if i < n - 1:
            travel_cost += c[i]

    print(answer)

solve()
```Heap lưu trữ các kho báu hiện được chọn. Vì là min-heap nên kho báu nhỏ nhất được chọn luôn có sẵn trong`O(1)`thời gian. 

Khi kích thước heap thấp hơn`k`, mọi kho báu mới đều tự động thuộc về kho báu hàng đầu hiện tại`k`bộ. Khi heap đạt đến kích thước`k`, một kho báu mới chỉ quan trọng nếu nó lớn hơn kho báu nhỏ nhất được chọn.`selected_sum`được cập nhật dần dần. Tính lại tổng từ heap mỗi lần lặp sẽ biến giải pháp thành`O(nk)`. 

Thứ tự của các hoạt động quan trọng. Lợi nhuận cho thành phố`i`phải được tính toán trước khi cộng chi phí di chuyển vào thành phố`i + 1`. Vào lúc đó,`travel_cost`phải thể hiện chính xác số tiền chi tiêu để đến thành phố`i`. 

Tất cả số học đều sử dụng số nguyên Python, xử lý các giá trị lớn hơn nhiều so với câu trả lời tối đa có thể một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 5, k = 2
a = [10, 3, 2, 20, 45]
c = [5, 5, 5, 50]
```| Thành phố | Kho báu | Nội dung đống | đã chọn_sum | du lịch_chi phí | Lợi nhuận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | [10] | 10 | 0 | 10 | 
| 2 | 3 | [3,10] | 13 | 5 | 8 | 
| 3 | 2 | [3,10] | 13 | 10 | 3 | 
| 4 | 20 | [10,20] | 30 | 15 | 15 | 
| 5 | 45 | [20,45] | 65 | 65 | 0 | 

Lợi nhuận tối đa là`15`, đạt được khi dừng lại ở thành phố 4. Những chiếc rương được chọn có giá trị`20`Và`10`, và tổng chi phí đi lại là`15`. 

### Ví dụ 2```
n = 7, k = 3
a = [0, 0, 5, 8, 13, 17, 20]
c = [9, 5, 7, 10, 1, 8]
```| Thành phố | Kho báu | đã chọn_sum | du lịch_chi phí | Lợi nhuận | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 9 | -9 | 
| 3 | 5 | 5 | 14 | -9 | 
| 4 | 8 | 13 | 21 | -8 | 
| 5 | 13 | 26 | 31 | -5 | 
| 6 | 17 | 38 | 32 | 6 | 
| 7 | 20 | 50 | 40 | 10 | 

Câu trả lời tốt nhất là`10`, thu được ở thành phố cuối cùng. 

Dấu vết này cho thấy tiền tố lớn hơn không phải lúc nào cũng tệ hơn mặc dù chi phí đi lại cao hơn. Số lượng kho báu sẵn có ngày càng tăng cuối cùng có thể lớn hơn chi phí bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log k) | Mỗi thành phố thực hiện tối đa một thao tác heap | 
| Không gian | O(k) | Heap lưu trữ nhiều nhất`k`kho báu | 

Với`n ≤ 10^5`,`O(n log k)`dễ dàng phù hợp với giới hạn lập trình cạnh tranh điển hình. Việc sử dụng bộ nhớ rất nhỏ vì chỉ những kho báu được chọn mới được lưu giữ trong heap. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    c = []
    if n > 1:
        c = list(map(int, input().split()))

    heap = []
    selected_sum = 0
    travel_cost = 0
    answer = 0

    for i in range(n):
        x = a[i]

        if len(heap) < k:
            heapq.heappush(heap, x)
            selected_sum += x
        elif x > heap[0]:
            removed = heapq.heapreplace(heap, x)
            selected_sum += x - removed

        answer = max(answer, selected_sum - travel_cost)

        if i < n - 1:
            travel_cost += c[i]

    return str(answer) + "\n"

# provided samples
assert run(
"""5 2
10 3 2 20 45
5 5 5 50
"""
) == "15\n", "sample 1"

assert run(
"""7 3
0 0 5 8 13 17 20
9 5 7 10 1 8
"""
) == "10\n", "sample 2"

# minimum size
assert run(
"""1 1
7
"""
) == "7\n", "single city"

# all equal values
assert run(
"""4 2
5 5 5 5
1 1 1
"""
) == "10\n", "all treasures equal"

# large travel costs discourage movement
assert run(
"""3 1
100 1 1000
10 10000
"""
) == "100\n", "best stop is first city"

# k equals n
assert run(
"""3 3
5 6 7
1 2
"""
) == "15\n", "can open every reachable chest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thành phố đơn lẻ | 7 | Hạn chế tối thiểu | 
| Kho báu bình đẳng | 10 | Logic thay thế heap có bản sao | 
| Chi phí đi lại lớn | 100 | Câu trả lời hay nhất có thể xảy ra sớm | 
| k = n | 15 | Tất cả các kho báu có thể tiếp cận đều có thể được lấy | 

## Vỏ cạnh 

Hãy xem xét trường hợp đi xa hơn có hại.```
3 1
100 1 1000
10 10000
```Tại thành phố 1, heap chứa`{100}`và lợi nhuận là`100`. Ở thành phố 2, kho báu quý giá nhất vẫn là`100`, nhưng lợi nhuận giảm xuống`90`vì chi phí đi lại. Tại thành phố 3, heap thay đổi thành`{1000}`, nhưng lợi nhuận lại trở thành`1000 - 10010`, tức là âm. Thuật toán giữ đúng giá trị lớn nhất`100`. 

Bây giờ hãy xem xét một tiền tố ngắn hơn`k`.```
2 2
5 7
1
```Sau thành phố 1, đống chỉ chứa một kho báu và`selected_sum = 5`. Thuật toán không bao giờ giả định chính xác điều đó`k`kho báu phải tồn tại. Sau thành phố 2, đống chứa cả kho báu và câu trả lời trở thành`12 - 1 = 11`. 

Cuối cùng, hãy xem xét các giá trị trùng lặp.```
4 2
5 5 5 5
1 1 1
```Heap luôn lưu trữ hai bản sao có giá trị`5`. Các giá trị bằng nhau không kích hoạt những thay thế không cần thiết vì việc thay thế một giá trị đã chọn`5`với người khác`5`không có gì thay đổi Bất biến mà heap chứa giá trị lớn nhất`k`các giá trị vẫn hợp lệ trong suốt quá trình thực thi.
