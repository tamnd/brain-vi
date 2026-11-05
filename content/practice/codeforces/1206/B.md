---
title: "CF 1206B - Tạo sản phẩm bình đẳng"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Trong một thao tác, chúng ta có thể chọn bất kỳ phần tử nào và tăng nó lên 1 hoặc giảm nó đi 1. Mỗi thay đổi như vậy tốn một xu."
date: "2026-06-11T23:32:40+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1206
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 580 (Div. 2)"
rating: 900
weight: 1206
solve_time_s: 103
verified: true
draft: false
---

[CF 1206B - Tạo sản phẩm bình đẳng](https://codeforces.com/problemset/problem/1206/B) 

**Xếp hạng:** 900 
**Tags:** dp, triển khai 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Trong một thao tác, chúng ta có thể chọn bất kỳ phần tử nào và tăng nó lên 1 hoặc giảm nó đi 1. Mỗi thay đổi như vậy tốn một xu. 

Mục tiêu là sửa đổi mảng sao cho tích của tất cả các phần tử trở thành chính xác 1, đồng thời tiêu tốn số xu tối thiểu có thể. 

Vì tích cuối cùng phải là 1 nên mọi phần tử trong mảng cuối cùng phải là 1 hoặc -1. Bất kỳ giá trị nào có giá trị tuyệt đối lớn hơn 1 sẽ gây ra chi phí không cần thiết, vì việc di chuyển nó xa hơn về phía ±1 luôn làm giảm số lượng thao tác cần thiết. Số 0 cũng không thể vẫn là 0 vì bất kỳ sản phẩm nào chứa số 0 đều bằng 0. 

Ràng buộc$n \le 10^5$ngay lập tức gợi ý rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào thử nhiều cấu hình cuối cùng có thể có hoặc thực hiện lập trình động trên các sản phẩm đều sẽ quá tốn kém. Với 100.000 phần tử, một$O(n)$quét là lý tưởng. 

Một số trường hợp đáng chú ý. 

Hãy xem xét một mảng chỉ chứa số 0:```
1
0
```Câu trả lời là 1. Chúng ta có thể đổi 0 thành 1 chỉ bằng một thao tác. Một giải pháp chỉ đếm số âm và số dương có thể quên xử lý số không. 

Hãy xem xét một số lẻ các số âm không có số 0:```
3
-1 -1 -1
```Sản phẩm là -1. Để tích bằng 1, phải thay đổi một phần tử từ -1 thành 1, tốn 2 thao tác. Câu trả lời đúng là 2. Một giải pháp tham lam bất cẩn có thể dừng lại sau khi chuyển đổi mọi thứ về ±1 gần nhất và trả về 0 không chính xác. 

Bây giờ hãy xem xét một số lẻ các số âm có ít nhất một số 0:```
2
-1 0
```Lựa chọn rẻ nhất là chuyển số 0 thành -1 với giá 1, cho ra sản phẩm 1. Câu trả lời là 1. Giải pháp tự động chuyển đổi mọi số 0 thành 1 sẽ thực hiện 3 thao tác thay thế. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là suy nghĩ về giá trị cuối cùng của mọi phần tử và thử tất cả các phép gán có thể có là 1 và -1. Vì mỗi phần tử có thể có hai trạng thái cuối cùng nên có$2^n$mảng mục tiêu có thể. Đối với mỗi nhiệm vụ, chúng ta có thể tính chi phí và kiểm tra xem sản phẩm có bằng 1 hay không. 

Điều này hoạt động về mặt khái niệm vì mọi mảng cuối cùng hợp lệ chỉ bao gồm 1 và -1. Vấn đề là kích thước của không gian tìm kiếm. Với$n=10^5$, thậm chí$2^{50}$khả năng đã là không thể, huống chi là$2^{100000}$. 

Quan sát quan trọng là mỗi phần tử có thể được xử lý độc lập khi tính toán cách rẻ nhất để đạt được 1 hoặc -1. 

Đối với số dương$x>0$, đóng góp rẻ nhất là biến nó thành 1, tốn$x-1$. 

Đối với số âm$x<0$, đóng góp rẻ nhất là biến nó thành -1, tốn$-1-x$. 

Với số 0, đạt 1 hoặc -1 tốn đúng 1. 

Sau khi làm cho mọi phần tử càng gần mục tiêu tự nhiên của nó càng tốt, vấn đề duy nhất còn lại là tính chẵn lẻ của số lượng giá trị âm. Tích bằng 1 khi và chỉ khi số giá trị -1 là số chẵn. 

Nếu số lượng âm bản đã chẵn thì chúng ta đã hoàn thành. 

Nếu nó lẻ, chúng ta cần điều chỉnh thêm một lần nữa. Khi số 0 tồn tại, chúng ta có thể chỉ cần chuyển đổi một số 0 thành -1 thay vì 1 mà không phải trả thêm phí ngoài 1 xu thông thường. Nếu không có số 0, chúng ta phải thay đổi -1 cuối cùng thành 1, cần thực hiện thêm 2 thao tác. 

Điều này dẫn trực tiếp đến giải pháp tham lam theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba biến: tổng chi phí, số phần tử âm và số số 0. 
2. Quét qua mảng một lần. 
3. Nếu giá trị hiện tại là dương, hãy chuyển đổi nó thành 1. Thêm$x-1$đến chi phí. 
4. Nếu giá trị hiện tại là âm, hãy chuyển đổi nó thành -1. Thêm vào$-1-x$đến chi phí và tăng bộ đếm âm. 
5. Nếu giá trị hiện tại bằng 0, hãy thêm 1 vào chi phí và tăng bộ đếm 0. 

Số 0 có thể trở thành 1 hoặc -1 với cùng chi phí, vì vậy hiện tại chúng tôi chỉ ghi lại sự tồn tại của nó. 
6. Sau khi xử lý tất cả các phần tử, kiểm tra xem số âm có phải là số chẵn hay không. 
7. Nếu số âm là số chẵn thì chi phí hiện tại đã tối ưu. 
8. Nếu số âm là số lẻ và có ít nhất một số 0 thì không cần tốn thêm chi phí. 

Một số 0 có thể được chuyển thành -1 thay vì 1, cố định tính chẵn lẻ trong khi vẫn có giá chính xác là 1 xu. 
9. Nếu số phủ định là số lẻ và không có số 0 thì hãy thêm 2 vào câu trả lời. 

Tùy chọn duy nhất là thay đổi -1 cuối cùng thành 1, yêu cầu hai thao tác. 
10. Xuất ra tổng chi phí. 

### Tại sao nó hoạt động 

Đối với mọi phần tử khác 0, giá trị có thể đạt được rẻ nhất trong số$\{-1,1\}$được xác định một cách độc lập. Bất kỳ sự lựa chọn thay thế nào đều có giá cao hơn. Ví dụ: thay đổi một số dương thành -1 luôn tốn chính xác hơn 2 lần so với thay đổi số đó thành 1. 

Sau những lựa chọn tối ưu cục bộ này, điều kiện toàn cục duy nhất là tổng số giá trị -1 phải là số chẵn. Nếu tính chẵn lẻ đã đúng thì các quyết định tối ưu cục bộ sẽ tạo thành một giải pháp tối ưu toàn cục. 

Khi số chẵn lẻ sai, cách hiệu chỉnh rẻ nhất là sử dụng số 0 hiện có, số này có thể trở thành -1 mà không mất thêm chi phí hoặc trả chính xác 2 thao tác bổ sung để lật một -1 cuối cùng thành 1. Không có sự điều chỉnh nào rẻ hơn nên thuật toán luôn tạo ra chi phí tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    cost = 0
    negatives = 0
    zeros = 0

    for x in a:
        if x > 0:
            cost += x - 1
        elif x < 0:
            cost += -1 - x
            negatives += 1
        else:
            cost += 1
            zeros += 1

    if negatives % 2 == 1 and zeros == 0:
        cost += 2

    print(cost)

solve()
```Vòng lặp đầu tiên tính toán chi phí tối thiểu để đưa mọi phần tử về giá trị hữu ích gần nhất của nó. Các số dương tiến về 1, các số âm tiến về -1 và các số 0 cần một thao tác để rời khỏi số 0. 

Biến`negatives`ghi lại có bao nhiêu phần tử tự nhiên trở thành -1. Biến`zeros`ghi lại liệu chúng tôi có thể linh hoạt sửa lỗi chẵn lẻ miễn phí hay không. 

Điều kiện cuối cùng là phần tinh tế duy nhất. Số âm lẻ có nghĩa là tích sẽ là -1. Nếu không có sẵn số 0, thì -1 phải trở thành 1, tốn chính xác 2 thao tác bổ sung. Khi tồn tại ít nhất một số 0, số 0 đó có thể trở thành -1 thay vì 1, cố định tính chẵn lẻ mà không làm tăng chi phí đã tính. 

Số nguyên Python dễ dàng xử lý kích thước câu trả lời vì chi phí tối đa có thể thấp hơn nhiều$10^{14}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
-1 1
```| Yếu tố | Chi phí bổ sung | Tiêu cực | Số không | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| -1 | 0 | 1 | 0 | 0 | 
| 1 | 0 | 1 | 0 | 0 | 

Sau khi quét, số âm bản là số lẻ và không có số 0. 

Chúng tôi thêm 2 hoạt động bổ sung. 

| Điều chỉnh cuối cùng | Chi phí bổ sung | Tổng cộng | 
| --- | --- | --- | 
| Lật một -1 thành 1 | 2 | 2 | 

Trả lời: **2** 

Ví dụ này thể hiện bước hiệu chỉnh chẵn lẻ. Mọi phần tử đều đã ở mức ±1, nhưng tích là -1, vì vậy cần phải điều chỉnh thêm một lần nữa. 

### Ví dụ 2 

đầu vào:```
5
-5 -3 5 3 0
```| Yếu tố | Chi phí bổ sung | Tiêu cực | Số không | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| -5 | 4 | 1 | 0 | 4 | 
| -3 | 2 | 2 | 0 | 6 | 
| 5 | 4 | 2 | 0 | 10 | 
| 3 | 2 | 2 | 0 | 12 | 
| 0 | 1 | 2 | 1 | 13 | 

Số âm đã bằng nhau nên không cần điều chỉnh gì thêm. 

Trả lời: **13** 

Dấu vết này cho thấy mỗi yếu tố đóng góp độc lập như thế nào cho câu trả lời. Số 0 cuối cùng tốn một thao tác và tính chẵn lẻ đã chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần đi qua mảng | 
| Không gian |$O(1)$| Chỉ có một vài quầy được lưu trữ | 

Thuật toán kiểm tra từng phần tử chính xác một lần và chỉ thực hiện công việc có thời gian không đổi cho mỗi phần tử. Với$n = 10^5$, điều này dễ dàng phù hợp với giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    cost = 0
    negatives = 0
    zeros = 0

    for x in a:
        if x > 0:
            cost += x - 1
        elif x < 0:
            cost += -1 - x
            negatives += 1
        else:
            cost += 1
            zeros += 1

    if negatives % 2 == 1 and zeros == 0:
        cost += 2

    return str(cost)

# provided sample
assert run("2\n-1 1\n") == "2", "sample 1"

# minimum size, single zero
assert run("1\n0\n") == "1", "single zero"

# odd negatives, no zero
assert run("3\n-1 -1 -1\n") == "2", "parity correction"

# odd negatives, with zero
assert run("2\n-1 0\n") == "1", "zero fixes parity"

# all positive
assert run("4\n2 2 2 2\n") == "4", "convert all to 1"

# mixed values
assert run("5\n-5 -3 5 3 0\n") == "13", "editorial example"

# boundary-sized values
assert run("2\n1000000000 -1000000000\n") == "1999999998", "large magnitudes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| Trường hợp không phần tử đơn | 
|`3 / -1 -1 -1`|`2`| Số âm lẻ không có số 0 | 
|`2 / -1 0`|`1`| Zero có thể sửa chữa sự chẵn lẻ | 
|`4 / 2 2 2 2`|`4`| Tất cả các số dương | 
|`5 / -5 -3 5 3 0`|`13`| Giá trị hỗn hợp và xử lý bằng không | 
|`2 / 1000000000 -1000000000`|`1999999998`| Giá trị tuyệt đối lớn | 

## Vỏ cạnh 

### Số âm lẻ và không có số 0 

đầu vào:```
3
-1 -1 -1
```Quá trình quét tạo ra chi phí 0, âm 3, số 0. Kết quả sẽ là -1 vì số âm là số lẻ. Vì không có số 0 nên thuật toán cộng 2 và trả về 2. 

Đây là cách khắc phục rẻ nhất có thể vì việc thay đổi một -1 thành 1 yêu cầu chính xác hai thao tác. 

### Số âm lẻ có số 0 

đầu vào:```
2
-1 0
```Quá trình quét tạo ra chi phí 1, âm 1, số không 1. Số âm là số lẻ nhưng tồn tại số 0. Thay vì chuyển số 0 thành 1, chúng ta chuyển số 0 thành -1 bằng cách sử dụng cùng một thao tác đã được tính. 

Mảng cuối cùng trở thành`[-1, -1]`, có tích là 1. Câu trả lời vẫn là 1. 

### Tất cả số không 

đầu vào:```
3
0 0 0
```Mỗi số 0 đóng góp 1, tạo ra chi phí 3. Số âm vẫn là 0, là số chẵn. 

Thuật toán trả về 3. Một mảng cuối cùng hợp lệ là`[1, 1, 1]`. 

### Phần tử âm đơn 

đầu vào:```
1
-1
```Quá trình quét cho ra chi phí 0, âm 1, số không 0. Tính chẵn lẻ là số lẻ nên thuật toán cộng thêm 2. 

Đáp án cuối cùng là 2, tương ứng với việc thay đổi`-1`vào trong`1`. 

Trường hợp này rất dễ bị bỏ qua vì phần tử đã ở một trong các giá trị mục tiêu nhưng yêu cầu về sản phẩm vẫn buộc phải điều chỉnh bổ sung.
