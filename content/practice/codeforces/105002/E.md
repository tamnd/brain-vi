---
title: "CF 105002E - \u041c\u0435\u0433\u0430\u041f\u043e\u043a\u0435\u0440"
description: "Chúng ta được cung cấp một chuỗi các thẻ được sắp xếp theo một thứ tự cố định. Mỗi thẻ mang một giá trị nguyên từ 1 đến m. Chúng tôi được phép xóa bất kỳ tập hợp con thẻ nào, nhưng chúng tôi không thể sắp xếp lại các thẻ còn lại."
date: "2026-06-28T03:18:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105002
codeforces_index: "E"
codeforces_contest_name: "vkoshp.letovo 2022"
rating: 0
weight: 105002
solve_time_s: 66
verified: true
draft: false
---

[CF 105002E - \u041c\u0435\u0433\u0430\u041f\u043e\u043a\u0435\u0440](https://codeforces.com/problemset/problem/105002/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thẻ được sắp xếp theo một thứ tự cố định. Mỗi thẻ mang một giá trị nguyên từ 1 đến m. Chúng tôi được phép xóa bất kỳ tập hợp con thẻ nào, nhưng chúng tôi không thể sắp xếp lại các thẻ còn lại. Sau khi xóa, chúng tôi muốn chuỗi còn lại tạo thành một chuỗi tăng dần một cách nghiêm ngặt, nghĩa là mỗi cặp liền kề khác nhau chính xác một. 

Vì vậy, nhiệm vụ là chọn một dãy con trông giống như x, x+1, x+2, v.v., trong khi vẫn giữ nguyên thứ tự ban đầu và tối đa hóa số lượng thẻ còn lại. 

Điểm mấu chốt là chúng tôi không chọn các thẻ tùy ý, chúng tôi đang chọn một dãy con hoạt động giống như một dãy số nguyên liên tục. 

Ràng buộc n 100000 loại trừ bất kỳ giải pháp nào thử tất cả các chuỗi con hoặc thậm chí thử mọi điểm bắt đầu và mở rộng nó một cách tham lam bằng các lần quét lồng nhau. Bất cứ điều gì bậc hai hoặc tệ hơn sẽ hết thời gian. Chúng ta cần một cái gì đó gần tuyến tính hoặc n log n. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị lặp lại hoặc khi có nhiều chuỗi ứng cử viên chồng chéo. Ví dụ: nếu mảng là 1 2 1 2 3, chúng ta có thể nghĩ rằng chúng ta có thể mở rộng nhiều chuỗi, nhưng chỉ các chuỗi con tôn trọng thứ tự và chuyển đổi +1 chính xác mới quan trọng, vì vậy chúng ta phải theo dõi cẩn thận tính khả thi thay vì chỉ tần suất. 

Một trường hợp phức tạp khác là khi chuỗi tốt nhất không được căn chỉnh với lần xuất hiện đầu tiên của giá trị bắt đầu. Ví dụ: trong 2 3 2 1 4, bắt đầu từ 2 cái đầu tiên sẽ tệ hơn so với bắt đầu từ 2 cái sau vì xây dựng một đường chạy liên tiếp dài hơn. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là thử mọi vị trí bắt đầu i, coi a[i] là điểm bắt đầu của chuỗi và tham lam quét sang bên phải để thu thập a[i], a[i]+1, a[i]+2, v.v. Đối với mỗi lần bắt đầu, chúng tôi sẽ duy trì một con trỏ và liên tục quét về phía trước để tìm giá trị bắt buộc tiếp theo. Điều này hoạt động chính xác bởi vì bất kỳ chuỗi con hợp lệ nào cũng phải chọn các phần tử theo thứ tự và việc tham lam tận dụng sự xuất hiện sớm nhất có thể sẽ duy trì tính khả thi. 

Tuy nhiên, phương pháp này liên tục quét mảng. Trong trường hợp xấu nhất, với mỗi n vị trí bắt đầu, chúng ta có thể quét tối đa n phần tử, cho kết quả O(n2). Với n lên tới 100000, đây là khoảng 10¹⁰ hoạt động, điều này không khả thi. 

Quan sát cấu trúc là khi chúng ta cố định giá trị bắt đầu x, vấn đề sẽ giảm xuống việc liên tục tìm lần xuất hiện tiếp theo của x+1 sau một vị trí đã chọn. Nếu chúng ta có thể trả lời nhanh chóng “sự xuất hiện tiếp theo của giá trị v sau chỉ số p”, thì chúng ta có thể mô phỏng từng chuỗi một cách hiệu quả. Điều này gợi ý vị trí tiền xử lý của từng giá trị và sử dụng tìm kiếm nhị phân. 

Đối với mỗi giá trị, chúng tôi lưu trữ một danh sách các chỉ mục được sắp xếp ở nơi nó xuất hiện. Sau đó, đối với vị trí bắt đầu nhất định i có giá trị x, chúng ta tiến về phía trước bằng cách liên tục nhảy đến lần xuất hiện đầu tiên của x+1 xuất hiện sau chỉ mục hiện tại của chúng ta. Mỗi bước nhảy tốn O(log k) thông qua tìm kiếm nhị phân và chúng tôi thực hiện tối đa m bước, nhưng trên thực tế chỉ dọc theo các giá trị liên tiếp hiện có. 

Điều này làm giảm vấn đề chỉ quét các chuỗi khả thi thay vì lặp đi lặp lại toàn bộ mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Danh sách vị trí + tìm kiếm nhị phân | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ánh xạ từ mỗi giá trị v tới danh sách các chỉ mục nơi v xuất hiện trong mảng. 

Điều này cho phép chúng tôi nhanh chóng xác định vị trí xuất hiện của bất kỳ giá trị nào theo thứ tự được sắp xếp. 
2. Lặp lại mọi chỉ mục i trong mảng và coi nó như một điểm khởi đầu tiềm năng. 

Chúng tôi cố gắng xây dựng chuỗi con liên tiếp hợp lệ dài nhất bắt đầu từ a[i]. 
3. Gọi giá trị hiện tại là a[i] và vị trí hiện tại là i. Khởi tạo độ dài thành 1. 
4. Nhiều lần cố gắng mở rộng chuỗi tới giá trị hiện tại + 1.

Chúng tôi tìm kiếm trong danh sách các vị trí cho giá trị đó và tìm chỉ mục nhỏ nhất lớn hơn vị trí hiện tại bằng cách sử dụng tìm kiếm nhị phân. 
5. Nếu chỉ mục đó tồn tại, hãy di chuyển đến chỉ mục đó, tăng độ dài chuỗi và tiếp tục. 

Nếu nó không tồn tại thì chuỗi dừng lại. 
6. Theo dõi độ dài xích tối đa trên tất cả các vị trí bắt đầu. 

Mỗi bước đảm bảo chúng tôi luôn tôn trọng thứ tự ban đầu trong khi chỉ chọn các giá trị liên tiếp một cách nghiêm ngặt. 

### Tại sao nó hoạt động 

Đối với bất kỳ giải pháp hợp lệ nào, chuỗi con phải được neo ở một số lần xuất hiện bắt đầu của giá trị v. Từ thời điểm đó trở đi, mọi phần tử tiếp theo phải là lần xuất hiện đầu tiên của v+1 sau lần chọn trước đó; lựa chọn bất cứ điều gì sau này chỉ làm giảm các lựa chọn trong tương lai. Do đó, việc nhảy tham lam đến lần xuất hiện tiếp theo hợp lệ sớm nhất không làm giảm tính tối ưu. Vì mọi chỉ số bắt đầu đều được xem xét nên chúng tôi không bỏ lỡ bất kỳ chuỗi nào có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_right

n, m = map(int, input().split())
a = list(map(int, input().split()))

pos = [[] for _ in range(m + 2)]
for i, v in enumerate(a):
    pos[v].append(i)

ans = 1

for i in range(n):
    v = a[i]
    cur_idx = i
    length = 1

    while v + 1 <= m:
        lst = pos[v + 1]
        if not lst:
            break
        j = bisect_right(lst, cur_idx)
        if j == len(lst):
            break
        cur_idx = lst[j]
        v += 1
        length += 1

    if length > ans:
        ans = length

print(ans)
```Giải pháp dựa trên các vị trí tiền xử lý để mọi truy vấn “bước tiếp theo” trở thành tìm kiếm nhị phân. Vòng lặp bên ngoài thử mọi chỉ số bắt đầu có thể, bởi vì chuỗi tốt nhất có thể bắt đầu khi xuất hiện bất kỳ giá trị nào. Vòng lặp bên trong đi tiếp qua các giá trị liên tiếp, luôn nhảy đến lần xuất hiện sớm nhất có thể. 

Việc sử dụng bisect_right là cần thiết vì chúng ta phải đảm bảo chuyển động tiến lên phía trước một cách nghiêm ngặt trong các chỉ số; sự bình đẳng không được phép vì chúng ta không thể sử dụng lại cùng một thẻ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4
2 3 2 1 4
```Chúng tôi xây dựng danh sách vị trí: 

| Giá trị | Vị trí | 
| --- | --- | 
| 1 | [3] | 
| 2 | [0, 2] | 
| 3 | [1] | 
| 4 | [4] | 

Bây giờ chúng ta theo dõi những điểm khởi đầu quan trọng. 

Bắt đầu từ chỉ số 0 (giá trị 2): 

| Bước | Giá trị | Chỉ số hiện tại | Đã tìm thấy vị trí tiếp theo | Chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | bắt đầu | 1 | 
| 2 | 3 | 0 | 1 | 2 | 
| 3 | 4 | 1 | 4 | 3 | 
| 4 | 5 | 4 | dừng lại | 3 | 

Tốt nhất từ đầu này là 3. 

Bắt đầu từ chỉ số 3 (giá trị 1): 

| Bước | Giá trị | Chỉ số hiện tại | Đã tìm thấy vị trí tiếp theo | Chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | bắt đầu | 1 | 
| 2 | 2 | 3 | không xảy ra sau 3 | 1 | 

Vì vậy, tối đa vẫn là 3. 

Điều này xác nhận rằng chuỗi tối ưu là 2 → 3 → 4. 

### Mẫu 2 

đầu vào:```
7 10
1 2 3 4 7 8 9
```Danh sách vị trí: 

| Giá trị | Vị trí | 
| --- | --- | 
| 1 | [0] | 
| 2 | [1] | 
| 3 | [2] | 
| 4 | [3] | 
| 7 | [4] | 
| 8 | [5] | 
| 9 | [6] | 

Bắt đầu từ 1: 

| Bước | Giá trị | Chỉ mục | Tiếp theo | Chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | bắt đầu | 1 | 
| 2 | 2 | 0 | 1 | 2 | 
| 3 | 3 | 1 | 2 | 3 | 
| 4 | 4 | 2 | 3 | 4 | 
| 5 | 5 | 3 | dừng lại | 4 | 

Bắt đầu từ số 7 thì chuỗi có độ dài ngắn hơn là 3. Vậy câu trả lời là 4. 

Điều này cho thấy thuật toán xử lý chính xác các khoảng trống về giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi chỉ mục có thể là điểm bắt đầu và mỗi bước sử dụng tìm kiếm nhị phân trên danh sách vị trí | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách chỉ mục cho tất cả các giá trị xuất hiện trong mảng | 

Các ràng buộc cho phép tối đa 100000 phần tử, do đó, giải pháp n log n nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính về kích thước mảng và số lần xuất hiện giá trị, điều này cũng dễ dàng phù hợp. 

## Trường hợp thử nghiệm```python
import sys, io
from bisect import bisect_right

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    pos = [[] for _ in range(m + 2)]
    for i, v in enumerate(a):
        pos[v].append(i)

    ans = 1

    for i in range(n):
        v = a[i]
        cur = i
        length = 1
        while v + 1 <= m:
            lst = pos[v + 1]
            if not lst:
                break
            j = bisect_right(lst, cur)
            if j == len(lst):
                break
            cur = lst[j]
            v += 1
            length += 1
        ans = max(ans, length)

    return str(ans)

# provided samples
assert solve("5 4\n2 3 2 1 4\n") == "3"
assert solve("7 10\n1 2 3 4 7 8 9\n") == "4"
assert solve("3 8\n3 2 1\n") == "1"

# custom cases
assert solve("1 5\n3\n") == "1", "single element"
assert solve("5 5\n1 1 1 1 1\n") == "1", "all equal"
assert solve("6 6\n1 3 2 4 5 6\n") == "4", "interleaving but valid chain"
assert solve("8 8\n8 7 6 5 4 3 2 1\n") == "1", "strictly decreasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | độ chính xác kích thước tối thiểu | 
| tất cả đều bình đẳng | 1 | không thể chuyển đổi liên tiếp | 
| chuỗi xen kẽ nhưng hợp lệ | 4 | xây dựng nhạy cảm với trật tự | 
| giảm nghiêm ngặt | 1 | thứ tự tồi tệ nhất cho chuỗi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tồn tại nhiều lần xuất hiện của một giá trị cần thiết, nhưng chỉ những lần xuất hiện sau mới cho phép tiếp tục. Ví dụ: trong 1 2 1 2 3, bắt đầu từ 2 số đầu tiên có thể dẫn đến ngõ cụt, trong khi bắt đầu từ số 2 thứ hai cho phép đạt đến 3. Thuật toán xử lý điều này vì mọi lần xuất hiện đều được coi là điểm bắt đầu và tìm kiếm nhị phân luôn chọn lần xuất hiện tiếp theo hợp lệ đầu tiên sau vị trí hiện tại. 

Một trường hợp khác là khi các giá trị bị thiếu ở giữa chuỗi. Nếu 1 và 3 tồn tại nhưng 2 không tồn tại thì không thể hình thành chuỗi dài hơn 1. Trong trường hợp này, danh sách vị trí cho 2 trống và thuật toán ngay lập tức dừng phần mở rộng, ngăn chặn chính xác việc xâu chuỗi sai qua các khoảng trống. 

Trường hợp tinh tế cuối cùng là các giá trị lặp lại được nhóm theo thứ tự, chẳng hạn như 1 1 1 2 2 3. Thuật toán luôn nhảy về phía trước trong không gian chỉ mục, do đó, nó tránh sử dụng lại các bản sao trước đó và vẫn tìm thấy chuỗi tối đa có thể đạt được 1 → 2 → 3 với độ dài 3.
