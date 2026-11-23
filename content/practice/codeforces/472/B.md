---
title: "CF 472B - Hướng dẫn thiết kế: Học hỏi từ cuộc sống"
description: "Chúng tôi có thang máy bắt đầu từ tầng một. Mọi người đều đang đợi ở tầng đó và muốn đến một tầng đích cụ thể. Thang máy có thể chở tối đa k người cùng một lúc. Di chuyển giữa các tầng tốn thời gian bằng độ chênh lệch tầng."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 472
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 270"
rating: 1300
weight: 472
solve_time_s: 104
verified: true
draft: false
---

[CF 472B - Hướng dẫn thiết kế: Học hỏi từ cuộc sống](https://codeforces.com/problemset/problem/472/B) 

**Đánh giá:** 1300 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có thang máy bắt đầu từ tầng một. Mọi người đều đang đợi ở tầng đó và muốn đến một tầng đích cụ thể. Thang máy có thể chở tối đa`k`mọi người cùng một lúc. 

Di chuyển giữa các tầng tốn thời gian bằng độ chênh lệch tầng. Vào và ra khỏi thang máy là miễn phí. Sau khi chở mọi người xong, thang máy phải quay trở lại tầng một. 

Nhiệm vụ là tìm tổng thời gian đi lại nhỏ nhất. 

Điều đầu tiên cần chú ý là người đi xuống các tầng thấp hơn có thể được thả xuống trong khi chở người lên tầng cao hơn. Nếu thang máy đã hướng tới tầng 10, việc thả ai đó ở tầng 4 trên đường đi sẽ không mất thêm chi phí nào. Vì lý do này, chi phí thực tế của một chuyến đi thang máy chỉ phụ thuộc vào tầng cao nhất đạt được trong chuyến đi đó. 

Các ràng buộc đủ nhỏ để cho phép sắp xếp. Cả hai`n`Và`k`nhiều nhất là 2000, vì vậy một`O(n log n)`giải pháp dễ dàng đủ nhanh. Thậm chí`O(n²)`sẽ phù hợp, nhưng có một quan sát đơn giản hơn nhiều dẫn trực tiếp đến nghiệm tham lam tối ưu. 

Một sai lầm phổ biến là nghĩ rằng mọi người nên được nhóm theo các tầng đích gần nhau. Coi như:```
4 2
2 2 100 100
```Một chiến lược tồi có thể đưa một người lên tầng 100 cùng với một người lên tầng 2. Điều đó tạo ra hai chuyến đi lên tầng 100. Giải pháp tối ưu là đưa cả hai hành khách ở tầng 100 lại với nhau, chỉ cần một chuyến đi lên tầng cao nhất. 

Một trường hợp tế nhị khác là khi`k >= n`.```
3 5
2 3 10
```Tất cả hành khách có thể đi xe cùng nhau. Thang máy đi lên tầng 10 và quay trở lại. Câu trả lời là`2 * (10 - 1) = 18`. Bất kỳ giải pháp nào luôn giả định nhiều chuyến đi sẽ bị tính quá mức. 

Trường hợp cạnh thứ ba xảy ra khi nhiều người có cùng đích đến.```
5 2
7 7 7 7 7
```Thang máy phải làm`ceil(5 / 2) = 3`chuyến đi, mỗi chuyến lên tới tầng 7. Câu trả lời là`3 * 2 * (7 - 1) = 36`. 

Điều quan trọng là phải hiểu rằng mỗi chuyến đi chỉ bị tính phí cho tầng đích tối đa. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xác định hành khách nào nên đi cùng nhau. Vì hành khách có thể được chia thành các nhóm theo nhiều cách khác nhau nên số lượng các khả năng có thể tăng theo cấp số nhân. Ngay cả đối với vài chục người, việc tìm kiếm toàn diện cũng trở nên bất khả thi. 

Lý do vũ lực là đúng về mặt khái niệm là vì một khi đã biết nhóm, chi phí của mỗi chuyến đi sẽ dễ dàng được tính toán. Nếu một nhóm gồm những hành khách có tầng đến cao nhất là`F`, chuyến đi đó có chi phí:```
(F - 1) + (F - 1) = 2(F - 1)
```Thử thách là tìm ra nhóm tốt nhất. 

Nhận xét quan trọng là chi phí của một chuyến đi chỉ phụ thuộc vào tầng lớn nhất trong số hành khách. Những người khác trong chuyến đi đó thực sự miễn phí vì họ có thể được đưa xuống trên đường đi. 

Giả sử chúng ta sắp xếp các tầng đích theo thứ tự giảm dần:```
f1 >= f2 >= f3 >= ...
```Điểm đến cao nhất phải xuất hiện trong một chuyến đi nào đó và sẽ quyết định chi phí của chuyến đi đó. Kể từ khi thêm vào`k - 1`các điểm đến nhỏ hơn trong cùng một chuyến đi không làm tăng số tầng tối đa, chúng ta nên luôn lấp đầy chuyến đi đó với càng nhiều hành khách còn lại càng tốt. 

Sau khi gán giá trị lớn nhất`k`điểm đến cho một chuyến đi, tầng lớn nhất chưa được chỉ định tiếp theo sẽ trở thành mức tối đa của một chuyến đi khác và logic tương tự sẽ lặp lại. 

Như vậy, sau khi sắp xếp theo thứ tự giảm dần, hành khách tại các vị trí:```
0, k, 2k, 3k, ...
```trở thành tầng tối đa của các chuyến đi thang máy. 

Mỗi tầng như vậy góp phần:```
2 * (floor - 1)
```để trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(1) bổ sung (không bao gồm sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`k`, và tất cả các tầng đích. 
2. Sắp xếp các tầng đích theo thứ tự giảm dần. 
3. Khởi tạo câu trả lời về 0. 
4. Bắt đầu từ chỉ mục`0`, nhảy qua`k`các vị trí mỗi lần. 
5. Mỗi tầng được chọn là điểm đến cao nhất trong một chuyến đi thang máy. 
6. Thêm`2 * (floor - 1)`để trả lời. 

Thang máy bắt đầu từ tầng 1, lên đến tầng cao nhất và cuối cùng quay trở lại tầng 1. 
7. Xuất ra câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Sau khi sắp xếp theo thứ tự giảm dần, điểm đến lớn nhất còn lại luôn quyết định chi phí cho chuyến đi tiếp theo. Vì hành khách có điểm đến nhỏ hơn có thể được vận chuyển cùng nhau mà không cần tăng số tầng tối đa nên mỗi chuyến đi nên chứa hành khách lớn nhất còn lại cộng thêm tối đa`k - 1`hành khách bổ sung. 

Sự lựa chọn tham lam này không thể làm tổn hại đến các chuyến đi trong tương lai vì việc loại bỏ các điểm đến nhỏ hơn khỏi các nhóm sau không bao giờ làm tăng số tầng tối đa của bất kỳ chuyến đi nào. Kết quả là nhóm tối ưu chính xác là phân vùng được hình thành bởi các khối có kích thước liên tiếp`k`theo thứ tự giảm dần. Phần tử đầu tiên của mỗi khối xác định chi phí của chuyến đi đó, đó là lý do tại sao tính tổng các tầng theo chỉ số`0, k, 2k, ...`đưa ra tổng thời gian tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    floors = list(map(int, input().split()))

    floors.sort(reverse=True)

    ans = 0
    for i in range(0, n, k):
        ans += 2 * (floors[i] - 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các tầng đích từ lớn nhất đến nhỏ nhất. Sau khi sắp xếp, mỗi khối`k`hành khách liên tiếp tương ứng với một chuyến đi thang máy. 

Hành khách đầu tiên trong mỗi khối có điểm đến cao nhất trong số những người được chỉ định cho chuyến đi đó. Vì thang máy phải di chuyển từ tầng 1 đến tầng đó rồi quay lại tầng 1 nên chuyến đi góp phần`2 * (floor - 1)`. 

Vòng lặp:```
for i in range(0, n, k):
```thăm chính xác hành khách đầu tiên của mỗi khối. Đó là những tầng duy nhất ảnh hưởng đến câu trả lời. 

Một điểm tinh tế là việc sử dụng`floor - 1`còn hơn là`floor`. Thang máy bắt đầu từ tầng 1 nên khi đến tầng`f`yêu cầu`f - 1`giây. 

Một chi tiết khác là xử lý các nhóm cuối cùng chưa hoàn chỉnh. Nếu ít hơn`k`hành khách ở lại, họ vẫn tạo thành một chuyến đi mà chi phí được xác định bởi tầng lớn nhất trong số đó. Vòng lặp xử lý việc này một cách tự nhiên vì phần tử đầu tiên của khối cuối cùng vẫn được truy cập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
2 3 4
```Sau khi sắp xếp:```
[4, 3, 2]
```| Chỉ mục | Tầng | Đã chọn làm chuyến đi tối đa? | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | Có | 2 × (4 - 1) = 6 | 6 | 
| 1 | 3 | Không | 0 | 6 | 
| 2 | 2 | Có | 2 × (2 - 1) = 2 | 8 | 

Câu trả lời cuối cùng:```
8
```Chuyến đầu tiên chở hành khách lên tầng 4 và 3. Chuyến thứ hai chở hành khách lên tầng 2. Chỉ có điểm đến lớn nhất trong mỗi chuyến đi là quan trọng. 

### Ví dụ 2 

đầu vào:```
5 2
7 7 7 7 7
```Sau khi sắp xếp:```
[7, 7, 7, 7, 7]
```| Chỉ mục | Tầng | Đã chọn làm chuyến đi tối đa? | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | Có | 12 | 12 | 
| 1 | 7 | Không | 0 | 12 | 
| 2 | 7 | Có | 12 | 24 | 
| 3 | 7 | Không | 0 | 24 | 
| 4 | 7 | Có | 12 | 36 | 

Câu trả lời cuối cùng:```
36
```Ví dụ này cho thấy các điểm đến giống hệt nhau chỉ tạo ra các chuyến đi lặp đi lặp lại khi công suất thang máy không đủ để chở tất cả mọi người cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) thêm | Ngoài mảng đầu vào và sắp xếp lưu trữ | 

Với`n ≤ 2000`, việc phân loại cực kỳ rẻ. Giải pháp chỉ thực hiện một lần quét tuyến tính bổ sung sau khi sắp xếp và phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, k = map(int, input().split())
    floors = list(map(int, input().split()))

    floors.sort(reverse=True)

    ans = 0
    for i in range(0, n, k):
        ans += 2 * (floors[i] - 1)

    return str(ans)

# provided sample
assert run("3 2\n2 3 4\n") == "8"

# minimum size
assert run("1 1\n2\n") == "2"

# k >= n, everyone in one trip
assert run("3 5\n2 3 10\n") == "18"

# all equal values
assert run("5 2\n7 7 7 7 7\n") == "36"

# boundary grouping case
assert run("4 2\n2 2 100 100\n") == "200"

# exact multiple of k
assert run("6 3\n2 3 4 5 6 7\n") == "22"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 2`|`2`| Trường hợp hợp lệ nhỏ nhất | 
|`3 5 / 2 3 10`|`18`| Công suất lớn hơn dân số | 
|`5 2 / 7 7 7 7 7`|`36`| Tầng giống hệt lặp đi lặp lại | 
|`4 2 / 2 2 100 100`|`200`| Phân nhóm đúng các tầng lớn | 
|`6 3 / 2 3 4 5 6 7`|`22`| Phân vùng khối chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
3 5
2 3 10
```Sau khi sắp xếp ta được:```
[10, 3, 2]
```Chỉ mục`0`được chọn vì`k = 5`. Câu trả lời trở thành:```
2 * (10 - 1) = 18
```Tất cả hành khách đều đi cùng nhau, đó là điều tối ưu. 

Coi như:```
5 2
7 7 7 7 7
```Thứ tự sắp xếp không thay đổi. Các chỉ số được chọn là`0`,`2`, Và`4`. Mỗi người đóng góp`12`, sản xuất`36`. Điều này giải thích chính xác cho ba chuyến đi cần thiết. 

Coi như:```
4 2
2 2 100 100
```Thứ tự sắp xếp:```
[100, 100, 2, 2]
```Các chỉ số được chọn là`0`Và`2`.```
2 * (100 - 1) + 2 * (2 - 1)
= 198 + 2
= 200
```Thuật toán tự động xếp cả 2 hành khách ở tầng 100 vào cùng một chuyến đi. Bất kỳ sự sắp xếp nào chia cắt họ thành các chuyến đi khác nhau sẽ phải trả gấp đôi chi phí cho tầng 100 và còn tệ hơn.
