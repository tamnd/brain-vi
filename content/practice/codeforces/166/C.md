---
title: "CF 166C - Trung vị"
description: "Chúng ta bắt đầu với một mảng các số nguyên và giá trị đích x. Chúng ta có thể thêm bất kỳ số lượng số nguyên bổ sung nào vào mảng và chúng ta muốn trung vị của mảng cuối cùng trở thành chính xác x. Nhiệm vụ là tính toán số lượng phần tử được thêm vào nhỏ nhất cần thiết để thực hiện điều đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 166
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 113 (Div. 2)"
rating: 1500
weight: 166
solve_time_s: 97
verified: true
draft: false
---

[CF 166C - Trung vị](https://codeforces.com/problemset/problem/166/C) 

**Đánh giá:** 1500 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng các số nguyên và một giá trị đích`x`. Chúng tôi có thể thêm bất kỳ số lượng số nguyên bổ sung nào vào mảng và chúng tôi muốn trung vị của mảng cuối cùng trở nên chính xác`x`. Nhiệm vụ là tính toán số lượng phần tử được thêm vào nhỏ nhất cần thiết để thực hiện điều đó. 

Định nghĩa trung vị ở đây tuân theo quy ước của Codeforces. Sau khi sắp xếp mảng, trung vị là phần tử ở vị trí`⌊(n + 1) / 2⌋`sử dụng chỉ mục dựa trên 1. Đối với một mảng có độ dài chẵn, điều này có nghĩa là phần tử ở giữa bên trái được chọn. 

Các ràng buộc đủ nhỏ để việc sắp xếp lặp đi lặp lại là hoàn toàn có thể chấp nhận được. Độ dài mảng ban đầu tối đa là`500`và mọi phần tử được thêm vào chỉ tăng kích thước một chút. Thậm chí một`O(n^2 log n)`giải pháp sẽ trôi qua một cách thoải mái. Thách thức không phải là hiệu suất, mà là hiểu chính xác những gì điều kiện tạo ra`x`số trung vị và cách đạt được chúng với ít lần chèn nhất. 

Trường hợp tinh tế đầu tiên xuất hiện khi`x`hoàn toàn không tồn tại trong mảng. 

Ví dụ:```
n = 3, x = 10
array = [20, 30, 40]
```Câu trả lời là không`0`, mặc dù chúng ta có thể tưởng tượng ra sự thay đổi vị trí bằng cách nào đó. Nếu như`10`vắng mặt, số trung vị không bao giờ có thể trở thành`10`, vì số trung vị phải là một phần tử mảng thực tế sau khi sắp xếp. Chúng ta phải chèn ít nhất một`10`. 

Một sai lầm dễ mắc phải khác là do hiểu nhầm phần tử ở giữa nào được sử dụng cho độ dài chẵn. 

Ví dụ:```
n = 4, x = 4
array = [1, 4, 5, 6]
```Thứ tự sắp xếp là:```
[1, 4, 5, 6]
```Vị trí trung gian là`⌊(4 + 1) / 2⌋ = 2`, vậy số trung vị là`4`, không`5`. Việc triển khai bất cẩn sử dụng phần tử ở giữa bên phải sẽ tạo ra câu trả lời sai. 

Trường hợp cạnh tinh tế hơn xảy ra khi`x`đã tồn tại nhưng có quá nhiều phần tử nhỏ hơn trước nó. 

Ví dụ:```
n = 5, x = 4
array = [1, 2, 3, 4, 100]
```Mảng được sắp xếp là:```
[1, 2, 3, 4, 100]
```Trung vị là`3`, không`4`. Chúng tôi không thể khắc phục điều này bằng cách chèn số nhỏ hơn`4`, bởi vì điều đó chỉ đẩy`4`thậm chí xa hơn bên phải. Sự bổ sung hữu ích duy nhất là những con số lớn hơn hoặc bằng`4`, làm dịch chuyển vị trí trung bình về phía sự xuất hiện của`4`. 

Kịch bản ngược lại cũng có vấn đề. 

Ví dụ:```
n = 5, x = 10
array = [10, 20, 30, 40, 50]
```Trung vị đã có rồi`30`. Ở đây, việc chèn số lượng lớn không giúp ích được gì. Chúng ta cần những con số nhỏ hơn để kéo vị trí trung tuyến sang trái cho đến khi nó chạm tới`10`. 

Những tác động định hướng này là quan sát chính đằng sau giải pháp. 

## Phương pháp tiếp cận 

Ở đây, cách tiếp cận bạo lực là điều tự nhiên một cách đáng ngạc nhiên. Chúng ta có thể liên tục kiểm tra xem trung vị hiện tại có bằng không`x`. Nếu không, chúng tôi chèn một số được lựa chọn cẩn thận và thử lại. 

Phiên bản đơn giản nhất hoạt động như thế này. Sắp xếp mảng. Nếu trung vị nhỏ hơn`x`, chèn cái khác`x`. Nếu trung vị lớn hơn`x`, cũng chèn một cái khác`x`. Tiếp tục cho đến khi trung vị trở thành`x`. 

Điều này đúng vì mỗi lần chèn`x`làm tăng số lần xuất hiện của`x`và dần dần kéo đường trung tuyến về phía nó. Vì các ràng buộc rất nhỏ nên thậm chí việc sắp xếp liên tục sau mỗi lần chèn cũng đủ nhanh. 

Điểm yếu của ý tưởng bạo lực này là ở tính khái niệm hơn là tính toán. Nó không giải thích lý do tại sao quy trình hoạt động hoặc điều kiện nào thực sự xác định thành công. Chúng ta cần một đặc tính rõ ràng hơn. 

Quan sát quan trọng là sau khi phân loại, một số sự xuất hiện của`x`phải chiếm vị trí trung gian. Giả sử chúng ta sắp xếp mảng và xem xét lần xuất hiện đầu tiên của`x`. 

Nếu sự xuất hiện đó nằm ở bên trái của vị trí trung vị thì có quá nhiều phần tử lớn hơn`x`. Chúng ta phải cộng các số nhỏ hơn để dịch chuyển chỉ số trung vị sang trái so với`x`. 

Nếu sự xuất hiện đó nằm ở bên phải vị trí trung vị thì có quá nhiều phần tử nhỏ hơn`x`. Chúng ta phải cộng các số lớn hơn để dịch chuyển chỉ số trung vị sang phải so với`x`. 

Một cách sạch hơn để thực hiện điều này thậm chí còn đơn giản hơn. Chúng tôi tiếp tục chèn`x`chính nó cho đến khi đường trung tuyến trở thành`x`. 

Tại sao điều này làm việc? Thêm`x`không bao giờ thay đổi thứ tự tương đối giữa các giá trị nhỏ hơn và lớn hơn. Nó chỉ tăng khối có giá trị bằng nhau. Cuối cùng, chỉ số trung vị phải nằm trong khối đó. 

Vì cần tối đa vài trăm lần chèn nên việc sắp xếp lặp đi lặp lại là hoàn toàn có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k · n log n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(k · n log n) | O(n) | Đã chấp nhận | 

Đây,`k`là số phần tử được chèn vào. Dưới những ràng buộc nhất định,`k`vẫn còn nhỏ. 

## Hướng dẫn thuật toán 

1. Đọc mảng và chèn một bản sao của`x`ngay lập tức nếu mảng chưa chứa nó. 

Bước này là bắt buộc vì số trung vị phải bằng một phần tử mảng thực tế. 
2. Sắp xếp mảng. 

Giá trị trung vị chỉ phụ thuộc vào thứ tự sắp xếp, vì vậy mọi kiểm tra phải diễn ra sau khi sắp xếp. 
3. Tính chỉ số trung vị là`(len(array) - 1) // 2`. 

Đây là phiên bản dựa trên số 0 của định nghĩa vấn đề. 
4. Kiểm tra xem phần tử trung vị có bằng không`x`. 

Nếu có, hãy dừng lại ngay lập tức. Số lần chèn hiện tại là tối thiểu vì chúng tôi chỉ chèn khi cần thiết. 
5. Nếu số trung vị nhỏ hơn`x`, nối thêm cái khác`x`. 

Điều này làm tăng số phần tử lớn hơn hoặc bằng`x`, đẩy giá trị trung bình lên trên. 
6. Nếu số trung vị lớn hơn`x`, cũng nối thêm một cái khác`x`. 

Điều này làm tăng số phần tử nhỏ hơn hoặc bằng`x`, đẩy giá trị trung bình xuống dưới. 
7. Lặp lại từ bước 2 cho đến khi số trung vị trở thành`x`. 

### Tại sao nó hoạt động 

Mỗi lần chèn thêm một bản sao khác của`x`, mở rộng khối liền kề của`x`các giá trị theo thứ tự sắp xếp. Vị trí trung vị thay đổi khi mảng phát triển, nhưng khối`x`giá trị cũng tăng lên. Cuối cùng, chỉ số trung bình sẽ nằm trong khối đó. 

Vì mỗi thao tác đều làm tăng số lượng`x`đúng một, không có chuỗi phép tính nào nhỏ hơn có thể làm cho số trung vị trở thành`x`trước đó. Thời điểm đầu tiên trung vị bằng`x`nhất thiết là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, x = map(int, input().split())
arr = list(map(int, input().split()))

added = 0

while True:
    arr.sort()

    median = arr[(len(arr) - 1) // 2]

    if median == x:
        print(added)
        break

    arr.append(x)
    added += 1
```Việc thực hiện được cố ý nhỏ gọn vì ý tưởng cơ bản rất đơn giản. 

Vòng lặp luôn bắt đầu bằng việc sắp xếp mảng. Vì số trung vị phụ thuộc vào thứ tự sắp xếp nên việc kiểm tra trước khi sắp xếp sẽ vô nghĩa. 

Chỉ số trung bình sử dụng:```
(len(arr) - 1) // 2
```Điều này phù hợp với định nghĩa ở giữa bên trái của vấn đề với độ dài chẵn. sử dụng`len(arr) // 2`sẽ chọn sai phần tử ở giữa bên phải và thất bại trong nhiều trường hợp. 

Bất cứ khi nào trung vị không`x`, chúng tôi nối thêm một cái khác`x`. Không cần phân biệt số trung vị quá nhỏ hay quá lớn. Thêm`x`giúp theo cả hai hướng vì nó mở rộng khối có giá trị bằng nhau. 

Biến`added`theo dõi số lượng chúng tôi đã chèn. Vì mỗi lần lặp vòng lặp sẽ chèn chính xác một giá trị nên trạng thái thành công đầu tiên sẽ tự động ở mức tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 10
10 20 30
```Mảng ban đầu:```
[10, 20, 30]
```| Bước | Mảng được sắp xếp | Chỉ số trung bình | Giá trị trung bình | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [10, 20, 30] | 1 | 20 | nối thêm 10 | 
| 2 | [10, 10, 20, 30] | 1 | 10 | dừng lại | 

Trả lời:```
1
```Ví dụ này cho thấy cách thêm một lần xuất hiện của`x`mở rộng phạm vi của nó theo thứ tự được sắp xếp cho đến khi chỉ số trung vị rơi vào nó. 

### Ví dụ 2 

đầu vào:```
5 4
1 2 3 4 100
```Mảng ban đầu:```
[1, 2, 3, 4, 100]
```| Bước | Mảng được sắp xếp | Chỉ số trung bình | Giá trị trung bình | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 2, 3, 4, 100] | 2 | 3 | nối thêm 4 | 
| 2 | [1, 2, 3, 4, 4, 100] | 2 | 3 | nối thêm 4 | 
| 3 | [1, 2, 3, 4, 4, 4, 100] | 3 | 4 | dừng lại | 

Trả lời:```
2
```Dấu vết này cho thấy trường hợp có quá nhiều phần tử nhỏ hơn ban đầu đẩy`x`ở bên phải vị trí trung gian. Việc chèn lặp đi lặp lại dần dần di chuyển chỉ số trung vị vào khối`4`S. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · n log n) | Mảng được sắp xếp sau mỗi lần chèn | 
| Không gian | O(n) | Bản thân mảng lưu trữ tất cả các phần tử | 

Với`n ≤ 500`, việc sắp xếp lặp đi lặp lại là đủ nhanh. Ngay cả khi chúng tôi chèn hàng trăm phần tử, tổng khối lượng công việc sẽ vẫn rất nhỏ so với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, x = map(int, input().split())
    arr = list(map(int, input().split()))

    added = 0

    while True:
        arr.sort()

        median = arr[(len(arr) - 1) // 2]

        if median == x:
            return str(added)

        arr.append(x)
        added += 1

# provided sample
assert run("3 10\n10 20 30\n") == "1", "sample 1"

# already correct
assert run("5 3\n1 2 3 4 5\n") == "0", "already median"

# x absent initially
assert run("3 5\n1 2 3\n") == "4", "x missing"

# single element
assert run("1 7\n7\n") == "0", "minimum size"

# all equal but wrong value
assert run("4 10\n1 1 1 1\n") == "5", "all values smaller"

# even length left-middle check
assert run("4 4\n1 4 5 6\n") == "0", "left middle median"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 3 / 1 2 3 4 5`|`0`| Trung bình đã đúng | 
|`3 5 / 1 2 3`|`4`| Mất tích`x`trước tiên phải được chèn | 
|`1 7 / 7`|`0`| Mảng kích thước tối thiểu | 
|`4 10 / 1 1 1 1`|`5`| Cần nhiều phần chèn | 
|`4 4 / 1 4 5 6`|`0`| Đúng định nghĩa giữa trái | 

## Vỏ cạnh 

Hãy xem xét trường hợp`x`vắng mặt. 

đầu vào:```
3 5
1 2 3
```Thực thi:```
[1, 2, 3] -> median = 2
append 5

[1, 2, 3, 5] -> median = 2
append 5

[1, 2, 3, 5, 5] -> median = 3
append 5

[1, 2, 3, 5, 5, 5] -> median = 3
append 5

[1, 2, 3, 5, 5, 5, 5] -> median = 5
```Đầu ra:```
4
```Thuật toán thành công vì mỗi lần chèn sẽ mở rộng khối`5`s cho đến khi chỉ số trung bình đạt đến nó. 

Bây giờ hãy xem xét định nghĩa độ dài chẵn. 

đầu vào:```
4 4
1 4 5 6
```Mảng được sắp xếp:```
[1, 4, 5, 6]
```Chỉ số trung bình:```
(4 - 1) // 2 = 1
```Giá trị trung bình:```
4
```Đầu ra:```
0
```Triển khai sai khi sử dụng chỉ mục`len(arr) // 2`sẽ chọn`5`thay vào đó và tiếp tục chèn các phần tử không chính xác. 

Cuối cùng, hãy xem xét một trường hợp trong đó`x`nằm xa về bên trái. 

đầu vào:```
5 10
10 20 30 40 50
```Thực thi:```
[10, 20, 30, 40, 50] -> median = 30
append 10

[10, 10, 20, 30, 40, 50] -> median = 20
append 10

[10, 10, 10, 20, 30, 40, 50] -> median = 20
append 10

[10, 10, 10, 10, 20, 30, 40, 50] -> median = 10
```Đầu ra:```
3
```Thêm bản sao của`10`dịch chuyển vị trí trung bình sang trái so với các giá trị lớn hơn cho đến khi nó nằm trong`10`khối.
