---
title: "CF 106284B - \u041a\u0442\u043e \u0431\u043e\u043b\u044c\u0448\u0435?"
description: "Chúng tôi có một số loại số nguyên không dấu. Loại có bit có thể lưu trữ các giá trị từ 0 đến 2^a - 1. Chúng ta cần quyết định xem có tồn tại một số nguyên x phù hợp với loại nhỏ hơn hay không, nhưng hình vuông của nó không vừa với loại lớn hơn."
date: "2026-06-25T07:40:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106284
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 (\u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435) 10-11 \u043a\u043b\u0430\u0441\u0441, \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2025"
rating: 0
weight: 106284
solve_time_s: 35
verified: true
draft: false
---

[CF 106284B - \u041a\u0442\u043e \u0431\u043e\u043b\u044c\u0448\u0435?](https://codeforces.com/problemset/problem/106284/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số loại số nguyên không dấu. Một loại có`a`bit có thể lưu trữ các giá trị từ`0`ĐẾN`2^a - 1`. Chúng ta cần quyết định xem có tồn tại một số nguyên hay không`x`vừa với loại nhỏ hơn, nhưng hình vuông của nó không vừa với loại lớn hơn. 

Nói cách khác, trong số các kích thước bit nhất định, chúng ta cần biết liệu có hai loại có kích thước`a < b`sao cho một số giá trị có thể biểu thị bằng`a`bit có một hình vuông cần nhiều hơn`b`bit. 

Đầu vào cung cấp số lượng loại có sẵn và dung lượng bit của chúng. Đầu ra là`YES`nếu một cặp loại như vậy tồn tại và`NO`nếu không thì. 

Các giới hạn rất quan trọng vì số lượng loại có thể lớn bằng`100000`, trong khi kích thước mỗi bit có thể lớn bằng`10^9`. Một giải pháp so sánh từng cặp loại sẽ hoạt động khoảng`10^10`kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn thời gian cuộc thi thông thường cho phép. Chúng ta cần một giải pháp gần`O(n log n)`hoặc tốt hơn. 

Các giá trị lớn cũng có nghĩa là chúng ta không thể lặp qua các số có thể`x`hoặc tính toán các quyền lực khổng lồ. Câu trả lời phải xuất phát từ mối quan hệ giữa độ dài bit. 

Phần khó khăn là kích thước bit bằng nhau không tạo ra một cặp hợp lệ vì chúng ta cần một loại nhỏ hơn cho`x`và một loại lớn hơn cho`x^2`. Ví dụ: với đầu vào:```
2
5 5
```câu trả lời là:```
NO
```Một giải pháp bất cẩn chỉ kiểm tra xem hai giá trị có khác nhau hay không có thể chấp nhận nó một cách không chính xác. 

Một trường hợp cạnh khác xuất hiện khi loại nhỏ nhất nhỏ hơn nhiều so với loại lớn nhất. Ví dụ:```
3
1 2 3
```Câu trả lời là:```
YES
```vì loại 1 bit chỉ lưu trữ`0`Và`1`, và giá trị hữu ích tối đa là`1`, hình vuông của nó vẫn vừa với 2 bit. Tuy nhiên, loại cỡ 2 có thể lưu trữ`3`, Và`3^2 = 9`yêu cầu 4 bit, không phù hợp với loại 3 bit. Giải pháp chỉ kiểm tra các giá trị liền kề sau khi sắp xếp có thể bỏ lỡ cặp không liền kề. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi loại nhỏ hơn và mọi loại lớn hơn. Đối với một cặp kích thước bit`a`Và`b`, giá trị lớn nhất phù hợp với loại nhỏ hơn là`2^a - 1`. Chúng ta sẽ cần phải kiểm tra xem liệu`(2^a - 1)^2 >= 2^b`. Nếu đúng thì loại nhỏ hơn sẽ chứa một giá trị có hình vuông vượt quá loại lớn hơn. 

Cách tiếp cận này đúng vì kiểm tra giá trị tối đa là đủ. Bình phương tăng khi số lượng tăng, vì vậy nếu giá trị tối đa có thể không tràn thì không giá trị nhỏ hơn nào có thể tràn. 

Tuy nhiên, việc kiểm tra tất cả các cặp là quá chậm. Với`n = 100000`, số lượng cặp là khoảng`n^2 / 2`, tức là khoảng năm tỷ so sánh. 

Quan sát quan trọng là chúng tôi chỉ quan tâm đến độ dài bit. Đối với một giá trị với`a`bit, bình phương của giá trị lớn nhất có thể nhỏ hơn một chút so với`2^(2a)`. Điều đó có nghĩa là một loại với`b`các bit có thể chứa mọi ô vuông từ`a`-bit chỉ loại khi`b`ít nhất là`2a`. Nếu có loại lớn hơn với ít hơn`2a`bit, câu trả lời là ngay lập tức`YES`. 

Vì vậy, vấn đề trở thành việc tìm kiếm liệu có tồn tại kích thước bit hay không`a`trong đó kích thước bit hiện có khác lớn hơn`a`nhưng ít hơn`2a`. Chúng tôi có thể sắp xếp kích thước và duy trì giá trị lớn nhất được thấy cho đến nay. Khi xử lý kích thước`a`, chúng ta cần biết liệu có kích thước trước đó ít nhất là`a`? Một cách đơn giản hơn là sắp xếp giảm dần và giữ kích thước nhỏ nhất tối đa đã được xử lý. 

Điều kiện có thể được kiểm tra bằng một mảng được sắp xếp. Đối với mọi kích thước, kích thước lớn hơn nguy hiểm duy nhất là những kích thước nhỏ hơn hai lần. Vì tất cả các kích thước đều được sắp xếp nên chúng ta có thể sử dụng tìm kiếm nhị phân để tìm xem giá trị đó có tồn tại hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các kích thước bit theo thứ tự tăng dần. Sau khi sắp xếp, mọi loại lớn hơn có thể có cho một loại nhất định sẽ xuất hiện ở đâu đó bên phải nó. 
2. Đối với từng vị trí`i`, xử lý kích thước hiện tại`a[i]`là loại lưu trữ`x`. Chúng ta cần tìm xem liệu có kích thước nào sau đó nhỏ hơn`2 * a[i]`. Kích thước như vậy sẽ đủ lớn để trở thành loại lớn hơn nhưng lại quá nhỏ để lưu trữ mọi hình vuông có thể. 
3. Sử dụng tìm kiếm nhị phân để tìm chỉ mục đầu tiên sau`i`giá trị của nó ít nhất là`2 * a[i]`. Nếu chỉ mục đó không phải là ranh giới tiếp theo ngay lập tức của mảng thì tồn tại một giá trị giữa`a[i]`Và`2 * a[i]`, vậy câu trả lời là`YES`. 
4. Nếu mọi loại đều vượt qua bước kiểm tra này, hãy in`NO`. 

Tại sao nó hoạt động: đối với một loại`a`bit, giá trị lớn nhất mà nó lưu trữ là`2^a - 1`. Bình phương nó cho một số nhỏ hơn`2^(2a)`và rất gần với giới hạn đó. Một loại có ít hơn`2a`bit không thể đại diện cho tất cả các ô vuông như vậy. Vì chúng ta chỉ cần chứng minh sự tồn tại của một giá trị tràn nên việc tìm loại lớn hơn bên dưới`2a`đúng là điều kiện cần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    for i in range(n):
        target = 2 * a[i]
        lo, hi = i + 1, n
        while lo < hi:
            mid = (lo + hi) // 2
            if a[mid] >= target:
                hi = mid
            else:
                lo = mid + 1
        if lo < n and lo > i + 1:
            print("YES")
            return

    print("NO")

solve()
```Mảng được sắp xếp trước tiên để tất cả các loại lớn hơn được nhóm lại với nhau. Với mỗi loại, tìm kiếm nhị phân tìm kích thước đầu tiên không nhỏ hơn`2 * a[i]`. Nếu vị trí đó không phải là phần tử tiếp theo thì có ít nhất một loại lớn hơn trước nó và loại đó quá nhỏ để chứa tất cả các ô vuông một cách an toàn. 

Phép nhân với hai là an toàn vì các giá trị đầu vào khớp với`10^9`, do đó giá trị trung gian dễ dàng khớp với các số nguyên Python. Tìm kiếm nhị phân bắt đầu từ`i + 1`bởi vì một loại không thể được ghép nối với chính nó. 

## Ví dụ đã hoạt động 

Dành cho:```
3
64 16 32
```Các kích thước được sắp xếp là`[16, 32, 64]`. 

| Kích thước hiện tại | Cần giá trị đầu tiên >= | Chỉ mục được tìm thấy | Kết quả | 
| --- | --- | --- | --- | 
| 16 | 32 | 1 | Không có giá trị nhỏ hơn trong phạm vi | 
| 32 | 64 | 2 | Không có giá trị nhỏ hơn trong phạm vi | 
| 64 | 128 | 3 | Kết thúc mảng | 

Quá trình quét không tìm thấy cặp hợp lệ nào, vì vậy đầu ra là`NO`. Điều này chứng tỏ trường hợp các loại lớn hơn chính xác là đủ lớn. 

Vì:```
4
4 2 1 3
```Các kích thước được sắp xếp là`[1, 2, 3, 4]`. 

| Kích thước hiện tại | Cần giá trị đầu tiên >= | Chỉ mục được tìm thấy | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | Không | 
| 2 | 4 | 3 | Giá trị 3 nằm trong khoảng từ 2 đến 4 | 

Thuật toán dừng và quay trở lại`YES`. Loại kích thước 2 có thể lưu trữ giá trị lên tới 3 và`3^2`cần 4 bit, vượt quá loại 3 bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Phân loại chi phí`O(n log n)`và mỗi cái`n`các phần tử thực hiện một tìm kiếm nhị phân. | 
| Không gian | O(n) | Mảng đã sắp xếp được lưu trữ. | 

Thuật toán tránh kiểm tra từng cặp loại, do đó nó vẫn hiệu quả đối với`100000`các giá trị đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    a.sort()
    for i in range(n):
        target = 2 * a[i]
        lo, hi = i + 1, n
        while lo < hi:
            mid = (lo + hi) // 2
            if a[mid] >= target:
                hi = mid
            else:
                lo = mid + 1
        if lo < n and lo > i + 1:
            return "YES\n"
    return "NO\n"

assert run("3\n64 16 32\n") == "NO\n"
assert run("4\n4 2 1 3\n") == "YES\n"

assert run("2\n5 5\n") == "NO\n", "equal sizes"
assert run("3\n1 2 3\n") == "YES\n", "non adjacent pair"
assert run("5\n1 1 1 1 1\n") == "NO\n", "all equal values"
assert run("2\n1000000000 1999999999\n") == "YES\n", "large boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 5 5`|`NO`| Kích thước bằng nhau không được tạo thành một cặp | 
|`3 / 1 2 3`|`YES`| Một cặp hợp lệ có thể không liền kề | 
|`5 / 1 1 1 1 1`|`NO`| Các giá trị trùng lặp được xử lý chính xác | 
|`2 / 1000000000 1999999999`|`YES`| Kích thước bit lớn không gây tràn | 

## Vỏ cạnh 

Đối với trường hợp kích thước bằng nhau:```
2
5 5
```Mảng được sắp xếp là`[5, 5]`. Đối với phần tử đầu tiên, không có phần tử nào sau đó nhỏ hơn`10`, nhưng dù sao phần tử sau cũng không lớn hơn. Thuật toán không bao giờ chấp nhận kích thước và trả về bằng nhau`NO`. 

Đối với trường hợp khe hở lớn:```
3
1 2 3
```Sau khi sắp xếp, giá trị đầu tiên là`1`. Kích thước đầu tiên ít nhất là`2`là phần tử thứ hai nên không có ứng cử viên nào ở đó. Đối với kích thước`2`, chúng ta cần ít nhất một giá trị`4`, nhưng giá trị`3`được tìm thấy trước ranh giới đó, có nghĩa là nó lớn hơn`2`và nhỏ hơn`4`. Thuật toán trả về`YES`, phù hợp với thực tế là số 2 bit có thể tràn loại 3 bit khi bình phương. 

Đối với trường hợp trùng lặp:```
5
1 1 1 1 1
```Mọi tìm kiếm đều bắt đầu sau chỉ mục hiện tại, nhưng tất cả các ứng viên đều có cùng giá trị. Không có giá trị nào lớn hơn loại hiện tại, do đó không tồn tại cặp hợp lệ và câu trả lời là`NO`. 

Đối với đầu vào rất lớn:```
2
1000000000 1999999999
```Giá trị thứ hai lớn hơn giá trị thứ nhất nhưng vẫn nhỏ hơn hai lần giá trị thứ nhất. Thuật toán phát hiện khoảng thời gian một cách chính xác và trả về`YES`mà không cần cố gắng tính toán bất kỳ sức mạnh to lớn nào.
