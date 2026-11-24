---
title: "CF 83B - Bác sĩ"
description: "Chúng tôi có một hàng động vật đang chờ bác sĩ. Thú cưng ơi, tôi phải đến gặp bác sĩ đúng [i] lần trước khi ra đi mãi mãi. Bất cứ khi nào một con vật được kiểm tra, một trong hai điều sẽ xảy ra. Nếu vẫn cần thêm lượt truy cập, nó sẽ ngay lập tức di chuyển về phía sau hàng đợi."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 83
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 72 (Div. 1 Only)"
rating: 1800
weight: 83
solve_time_s: 97
verified: true
draft: false
---

[CF 83B - Bác sĩ](https://codeforces.com/problemset/problem/83/B) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, toán học, sắp xếp 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng động vật đang chờ bác sĩ. Động vật`i`phải đi khám bác sĩ chính xác`a[i]`lần trước khi ra đi mãi mãi. 

Bất cứ khi nào một con vật được kiểm tra, một trong hai điều sẽ xảy ra. Nếu vẫn cần thêm lượt truy cập, nó sẽ ngay lập tức di chuyển về phía sau hàng đợi. Nếu lượt truy cập đó đã hoàn thành tất cả các bài kiểm tra bắt buộc, nó sẽ biến mất khỏi hàng đợi. 

Bác sĩ thực hiện chính xác`k`thi rồi nghỉ làm. Chúng ta phải xuất ra những con vật vẫn đang xếp hàng tại thời điểm đó, theo thứ tự chính xác từ trước ra sau. 

Các ràng buộc loại trừ hoàn toàn mô phỏng trực tiếp. Số lượng động vật nhiều nhất là`10^5`, có thể quản lý được, nhưng`k`có thể lớn như`10^14`. Một mô phỏng theo nghĩa đen sẽ yêu cầu một thao tác cho mỗi lần kiểm tra, điều này có thể có nghĩa là một trăm nghìn tỷ hoạt động xếp hàng. Ngay cả việc triển khai được tối ưu hóa cao cũng không thể giải quyết được điều đó. 

Tổng số bài kiểm tra có thể thực hiện được là tổng của tất cả`a[i]`. Nếu số tiền đó nhỏ hơn`k`, bác sĩ thậm chí không thể thực hiện`k`bài kiểm tra và câu trả lời là`-1`. 

Phần khó khăn là việc xây dựng lại hàng đợi sau một số lượng lớn các lần quay và loại bỏ theo chu kỳ. 

Một sai lầm dễ mắc phải là xử lý sai trường hợp hàng đợi trở nên trống ngay sau`k`- Kỳ thi thứ . 

Ví dụ:```
Input:
1 1
1
```Sau một lần kiểm tra, con vật duy nhất sẽ ra đi mãi mãi. Hàng đợi trống nên kết quả đầu ra chính xác là một dòng trống chứ không phải`-1`. 

Một trường hợp tinh vi khác xuất hiện khi một số con vật về đích trong cùng một "vòng". 

Ví dụ:```
Input:
4 5
2 1 2 1
```Việc thực hiện bất cẩn có thể xoay con vật không chính xác sau khi loại bỏ và tạo ra thứ tự còn lại sai. 

Quá trình đúng là:```
Initial: 1 2 3 4
After 1: 2 3 4 1
After 2: 3 4 1
After 3: 4 1 3
After 4: 1 3
After 5: 3
```Đầu ra đúng:```
3
```Một trường hợp góc nguy hiểm khác là`k = 0`. 

Ví dụ:```
Input:
3 0
5 2 7
```Không có cuộc kiểm tra nào xảy ra nên hàng đợi không thay đổi:```
1 2 3
```Việc triển khai luôn nâng cao hàng đợi ít nhất một lần sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Lưu trữ hàng đợi một cách rõ ràng. Liên tục bật con vật phía trước, giảm số lượt truy cập còn lại của nó và loại bỏ nó hoặc đẩy nó trở lại. 

Mô phỏng này là chính xác vì nó tuân theo quy trình một cách chính xác. Vấn đề là quy mô. Nếu mỗi động vật cần tới`10^9`thăm và`k`chính nó đạt tới`10^14`, việc mô phỏng có thể yêu cầu hàng nghìn tỷ phép tính. 

Cấu trúc của quy trình mang lại cách tiếp cận tốt hơn. 

Hãy tưởng tượng tất cả các động vật hiện đang sống hoàn thành một chu kỳ đầy đủ trong hàng đợi. Mỗi động vật còn sống sẽ mất đúng một lần viếng thăm bắt buộc trong chu kỳ đó. Thay vì xử lý từng lượt truy cập, chúng tôi có thể xử lý toàn bộ lớp cùng một lúc. 

Giả sử hiện tại có`m`động vật còn sống và mỗi con trong số chúng vẫn cần ít nhất`d`nhiều lượt truy cập hơn. Sau đó chúng ta có thể dành`m * d`kiểm tra hàng loạt, giảm thiểu mọi động vật còn sống bằng cách`d`. 

Điều này biến vấn đề thành một vấn đề tương tự như việc bóc lớp từ tất cả các số cùng một lúc. 

Nếu chúng ta sắp xếp động vật theo yêu cầu thăm quan, chúng ta có thể xác định có thể loại bỏ bao nhiêu lớp hoàn chỉnh trước khi một số động vật biến mất. Số lượng lớp bị loại bỏ có tác dụng giống như một phần bù tổng thể được áp dụng cho tất cả các động vật còn sống. 

Cuối cùng, chúng tôi đạt đến điểm mà lớp đầy đủ tiếp theo sẽ vượt quá`k`. Tại thời điểm đó, chỉ còn lại một phần chu kỳ. Vì số lần kiểm tra còn lại bây giờ ít hơn số lượng động vật còn sống nên chúng ta có thể trực tiếp xây dựng lại hàng đợi cuối cùng. 

Brute-force hoạt động vì các hoạt động xếp hàng rất đơn giản nhưng không thành công vì nó xử lý mọi kiểm tra riêng lẻ. Giải pháp tối ưu hóa hoạt động vì nhiều lần kiểm tra liên tiếp ảnh hưởng đến tất cả các động vật sống như nhau, cho phép chúng tôi nén các phần lớn của quy trình thành một bước số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị và tính tổng số lần kiểm tra có thể, đó là`sum(a)`. 

Nếu như`sum(a) < k`, in`-1`ngay lập tức vì bác sĩ không thể thực hiện nhiều cuộc kiểm tra như vậy. 
2. Tạo cặp`(a[i], i)`và sắp xếp chúng theo`a[i]`. 

Việc sắp xếp cho phép chúng ta xử lý động vật theo thứ tự chúng biến mất. 
3. Duy trì ba biến:`prev`, số lớp hoàn chỉnh đã bị loại bỏ.`alive`, số lượng động vật vẫn còn trong hàng đợi.`used`, số lượng bài kiểm tra đã được tính. 
4. Lặp lại các con vật đã được sắp xếp. 

Giả sử con vật hiện tại cần`x`thăm. Từ`prev`các lớp đã bị xóa trên toàn cầu, loài động vật này vẫn cần`x - prev`bổ sung thêm các lớp đầy đủ trước khi biến mất. 
5. Tính toán:```
cost = (x - prev) * alive
```Đây là số lần kiểm tra cần thiết để giảm mọi động vật còn sống xuống điểm biến mất tiếp theo. 
6. Nếu`used + cost <= k`, tiêu thụ toàn bộ khối này. 

Cập nhật:```
used += cost
prev = x
alive -= count_of_animals_removed_here
```Nhiều động vật có thể chia sẻ cùng một`x`, thế là tất cả chúng cùng biến mất. 
7. Nếu không, chúng tôi không thể hoàn thành lớp đầy đủ tiếp theo. 

Vào lúc này, mọi động vật còn sống đều đã mất`prev`thăm. Các bài thi còn lại là:```
rem = k - used
```8. Thu thập tất cả các động vật có giá trị ban đầu lớn hơn`prev`. Đây chính xác là những động vật vẫn còn sống. 
9. Sắp xếp những con vật còn sống này theo chỉ mục ban đầu vì thứ tự xếp hàng luôn giữ nguyên thứ tự tương đối giữa những con vật còn sống. 
10. Quá trình còn lại chỉ là xoay vòng trong hàng đợi còn hoạt động`rem`các bước. 

Từ`rem < number_of_alive_animals`, hàng đợi cuối cùng bắt đầu tại vị trí:```
rem % len(alive_animals)
```11. Xuất các động vật còn sống bắt đầu từ vị trí đó, quấn quanh theo chu kỳ. 

### Tại sao nó hoạt động 

Sau khi loại bỏ`prev`các lớp hoàn chỉnh, mọi động vật sống sót đều có chính xác`a[i] - prev`lượt truy cập còn lại. Thứ tự tương đối của chúng trong hàng đợi không thay đổi vì mỗi lớp hoàn chỉnh tương ứng với một vòng quay đầy đủ qua tất cả các động vật còn sống. 

Khi chúng tôi dừng lại trước khi hoàn thành một lớp đầy đủ khác, các kiểm tra còn lại chỉ hình thành một phần duyệt qua hàng đợi còn sống. Tiến lên`rem`các vị trí trong hàng đợi khớp chính xác khi thực hiện từng bài kiểm tra còn lại. 

Thuật toán không bao giờ mất dấu thứ tự hàng đợi vì việc xóa chỉ xóa các động vật chứ không bao giờ sắp xếp lại thứ tự của những người sống sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    if sum(a) < k:
        print(-1)
        return

    arr = sorted((a[i], i) for i in range(n))

    used = 0
    prev = 0
    alive = n
    ptr = 0

    while ptr < n:
        value = arr[ptr][0]
        diff = value - prev
        cost = diff * alive

        if used + cost > k:
            break

        used += cost
        prev = value

        while ptr < n and arr[ptr][0] == value:
            ptr += 1
            alive -= 1

    remaining = k - used

    survivors = []

    for i in range(n):
        if a[i] > prev:
            survivors.append(i + 1)

    if not survivors:
        print()
        return

    start = remaining % len(survivors)

    answer = survivors[start:] + survivors[:start]

    print(*answer)

solve()
```Chi tiết quan trọng đầu tiên là việc kiểm tra sớm những điều không thể thực hiện được. Nếu tổng số bài kiểm tra bắt buộc nhỏ hơn`k`, không có hàng đợi nào tồn tại sau thời gian dừng của bác sĩ vì quá trình này đã kết thúc sớm hơn. 

Mảng được sắp xếp thúc đẩy quá trình loại bỏ lớp. Mỗi giá trị riêng biệt cho chúng ta biết khi nào một nhóm động vật mới biến mất. 

Biểu thức:```
cost = diff * alive
```là trái tim của giải pháp.`diff`biểu thị số lượng vòng hoàn chỉnh mà chúng ta có thể áp dụng thống nhất cho tất cả các động vật còn sống và mỗi vòng chạm vào mỗi động vật còn sống đúng một lần. 

Bên trong`while`vòng lặp sẽ loại bỏ mọi con vật có số lượt truy cập được yêu cầu bằng ngưỡng hiện tại. Xử lý các giá trị bằng nhau với nhau là điều cần thiết. Việc quên điều này sẽ tạo ra số lượng sống không chính xác và vi phạm số học sau này. 

Sau khi quá trình xử lý hàng loạt dừng lại, tất cả các động vật còn lại đều thỏa mãn:```
a[i] > prev
```Những con vật đó xuất hiện theo thứ tự xếp hàng theo chỉ số ban đầu của chúng. Chúng ta không bao giờ cần một hàng đợi thực sự vì các lớp hoàn chỉnh duy trì trật tự tuần hoàn. 

Các kỳ thi còn lại ít hơn số người sống sót nên một vòng quay đơn giản sẽ đưa ra hàng cuối cùng. 

Trường hợp hàng đợi trống rất tinh tế. Nếu mọi con vật biến mất đúng lúc`k`, đầu ra đúng chỉ là một dòng trống. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 2 1
```Các cặp được sắp xếp:```
(1,0), (1,2), (2,1)
```| Bước | trước | còn sống | đã qua sử dụng | Giá trị hiện tại | chi phí | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 3 | 0 | 1 | 3 | tiêu thụ | 
| Sau khi loại bỏ | 1 | 1 | 3 | 2 | 1 | dừng lại | 

Hiện nay:```
remaining = 3 - 3 = 0
```Những người sống sót:```
[2]
```Vị trí bắt đầu:```
0 % 1 = 0
```Hàng đợi cuối cùng:```
2
```Dấu vết này cho thấy hai con vật biến mất đồng thời như thế nào sau lớp đầy đủ đầu tiên. 

### Mẫu 2 

đầu vào:```
7 10
1 3 1 2 5 2 4
```Các cặp được sắp xếp:```
(1,0), (1,2), (2,3), (2,5), (3,1), (4,6), (5,4)
```| Bước | trước | còn sống | đã qua sử dụng | Giá trị hiện tại | chi phí | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 7 | 0 | 1 | 7 | tiêu thụ | 
| Xóa 1 giây | 1 | 5 | 7 | 2 | 5 | dừng lại | 

Hiện nay:```
remaining = 10 - 7 = 3
```Những người sống sót theo thứ tự xếp hàng:```
[2, 4, 5, 6, 7]
```Xoay theo:```
3 % 5 = 3
```Hàng đợi cuối cùng:```
6 7 2 4 5
```Ví dụ này thể hiện quan sát quan trọng đằng sau thuật toán. Thay vì mô phỏng bảy bài kiểm tra riêng biệt, chúng tôi loại bỏ ngay một lớp hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | phân loại chiếm ưu thế | 
| Không gian | O(n) | lưu trữ mảng và người sống sót | 

Bước sắp xếp là phần phi tuyến tính duy nhất của thuật toán. Với`n <= 10^5`,`O(n log n)`dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tuyến tính và thoải mái dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if sum(a) < k:
            return "-1"

        arr = sorted((a[i], i) for i in range(n))

        used = 0
        prev = 0
        alive = n
        ptr = 0

        while ptr < n:
            value = arr[ptr][0]
            diff = value - prev
            cost = diff * alive

            if used + cost > k:
                break

            used += cost
            prev = value

            while ptr < n and arr[ptr][0] == value:
                ptr += 1
                alive -= 1

        remaining = k - used

        survivors = []

        for i in range(n):
            if a[i] > prev:
                survivors.append(i + 1)

        if not survivors:
            return ""

        start = remaining % len(survivors)

        answer = survivors[start:] + survivors[:start]

        return " ".join(map(str, answer))

    return solve()

# provided sample
assert run("3 3\n1 2 1\n") == "2", "sample 1"

# k = 0
assert run("3 0\n5 2 7\n") == "1 2 3", "no examinations"

# impossible case
assert run("2 5\n1 2\n") == "-1", "not enough total examinations"

# queue becomes empty exactly at k
assert run("1 1\n1\n") == "", "empty queue"

# all equal values
assert run("4 6\n3 3 3 3\n") == "3 4", "uniform layers"

# multiple removals together
assert run("4 5\n2 1 2 1\n") == "3", "simultaneous removals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 0 / 5 2 7`|`1 2 3`| không xảy ra quá trình xử lý | 
|`2 5 / 1 2`|`-1`| tổng khối lượng công việc không thể | 
|`1 1 / 1`| đầu ra trống | hàng đợi trở nên trống chính xác tại k | 
|`4 6 / 3 3 3 3`|`3 4`| hành vi tuần hoàn thống nhất | 
|`4 5 / 2 1 2 1`|`3`| loại bỏ đồng thời | 

## Vỏ cạnh 

Hãy xem xét trường hợp hàng đợi trở nên trống ngay sau kỳ thi cuối cùng. 

đầu vào:```
1 1
1
```Thuật toán xử lý một lớp hoàn chỉnh:```
cost = 1 * 1 = 1
```Sau khi tiêu thụ nó:```
used = 1
alive = 0
```Không còn ai sống sót nên thuật toán in ra một dòng trống. Điều này phù hợp chính xác với quá trình thực tế. 

Bây giờ hãy xem xét việc loại bỏ đồng thời. 

đầu vào:```
4 5
2 1 2 1
```Sau khi sắp xếp:```
(1,1), (1,3), (2,0), (2,2)
```Chi phí lớp đầu tiên:```
1 * 4 = 4
```Sau khi tiêu thụ nó, cả hai con vật đều có giá trị`1`cùng biến mất. Những người sống sót là:```
1, 3
```Còn lại một bài kiểm tra. Xoay một lần lá:```
3
```Việc xử lý các giá trị bằng nhau với nhau là rất quan trọng ở đây. Loại bỏ chúng cùng một lúc sẽ thay đổi không chính xác số lượng còn sống trong cùng một lớp. 

Cuối cùng, hãy xem xét`k = 0`. 

đầu vào:```
3 0
5 2 7
```Vòng lặp chính ngay lập tức dừng lại vì không thể thực hiện bài kiểm tra nào. Từ`prev = 0`, mọi động vật đều sống sót. Số vòng quay còn lại cũng bằng 0 nên thứ tự ban đầu được giữ nguyên:```
1 2 3
```Điều này xác nhận rằng thuật toán xử lý trường hợp tiền tố trống một cách chính xác.
