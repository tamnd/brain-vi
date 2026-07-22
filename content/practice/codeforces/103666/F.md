---
title: "CF 103666F - \u041c\u0430\u0448\u0430 \u0438 \u043c\u0430\u0442\u0440\u0451\u0448\u043a\u0438"
description: "Chúng tôi được tặng một bộ sưu tập búp bê matryoshka, mỗi con đều có kích thước bằng số. Một con búp bê chỉ có thể được đặt bên trong một con búp bê khác nếu kích thước của nó nhỏ hơn hoàn toàn. Mỗi con búp bê có thể chứa trực tiếp nhiều nhất một con búp bê khác, vì vậy cấu trúc mà chúng tôi xây dựng là một chuỗi chứ không phải là cấu trúc phân nhánh."
date: "2026-07-03T02:29:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103666
codeforces_index: "F"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2016"
rating: 0
weight: 103666
solve_time_s: 41
verified: true
draft: false
---

[CF 103666F - \u041c\u0430\u0448\u0430 \u0438 \u043c\u0430\u0442\u0440\u0451\u0448\u043a\u0438](https://codeforces.com/problemset/problem/103666/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập búp bê matryoshka, mỗi con đều có kích thước bằng số. Một con búp bê chỉ có thể được đặt bên trong một con búp bê khác nếu kích thước của nó nhỏ hơn hoàn toàn. Mỗi con búp bê có thể chứa trực tiếp nhiều nhất một con búp bê khác, vì vậy cấu trúc mà chúng tôi xây dựng là một chuỗi chứ không phải là cấu trúc phân nhánh. 

Nhiệm vụ là loại bỏ một số con búp bê để những con còn lại có thể được sắp xếp thành một chuỗi lồng duy nhất, trong đó mỗi con búp bê sẽ nằm gọn trong con tiếp theo. Chúng tôi muốn tối đa hóa số lượng búp bê còn lại trong một chuỗi như vậy. 

Nói lại, đây là yêu cầu chuỗi số tăng dần nghiêm ngặt dài nhất, trong đó chúng tôi được phép sắp xếp lại các phần tử đã chọn một cách tùy ý vì chúng tôi có thể chọn bất kỳ thứ tự lồng nào miễn là kích thước tăng nghiêm ngặt dọc theo chuỗi. 

Kích thước đầu vào lên tới 1000 búp bê và kích thước lên tới 10000. Điều này ngay lập tức gợi ý rằng giải pháp O(n^2) có thể chấp nhận được, trong khi bất kỳ hình khối nào cũng sẽ vượt qua nhưng không cần thiết. Giải pháp O(n log n) cũng có thể thực hiện được nhưng không bắt buộc. 

Một điểm tinh tế là chúng ta không bị hạn chế trong việc duy trì trật tự ban đầu. Nếu ai đó coi nhầm dữ liệu đầu vào là một dãy và cố gắng tìm dãy con tăng dài nhất, thì họ có thể đếm thiếu. Ở đây chúng tôi có thể tự do sắp xếp lại, vì vậy chỉ có nhiều giá trị mới quan trọng. 

Trường hợp cạnh thứ hai phát sinh từ các kích thước bằng nhau. Vì việc lồng nhau đòi hỏi sự bất bình đẳng nghiêm ngặt nên các phần tử bằng nhau không thể được đặt liên tiếp. Ví dụ, đầu vào`3 3 3 3`nên sản xuất`1`, không`4`, bởi vì chỉ một trong số chúng có thể được chọn trong bất kỳ chuỗi tăng nghiêm ngặt nào. 

## Phương pháp tiếp cận 

Một cách giải thích ngây thơ là nghĩ đến việc xây dựng một chuỗi bằng cách thử mọi thứ tự có thể có của những con búp bê đã chọn và kiểm tra xem liệu nó có tạo thành một dãy tăng nghiêm ngặt hợp lệ hay không. Điều này nhanh chóng trở nên phức tạp theo giai thừa vì chúng ta sẽ hoán vị các tập hợp con và xác minh các ràng buộc, điều này là không thể nếu vượt quá n rất nhỏ. 

Một ý tưởng mạnh mẽ khác là thử mọi tập hợp con và sau đó kiểm tra xem liệu nó có thể được sắp xếp thành một dãy tăng dần hay không. Điều đó giúp giảm bớt vấn đề trong việc kiểm tra xem tất cả các giá trị trong tập hợp con có khác biệt hay không, vì việc sắp xếp bất kỳ tập hợp con nào sẽ mang lại một cách lồng hợp lệ khi và chỉ khi không tồn tại bản sao. Quan sát này đã đơn giản hóa vấn đề một cách đáng kể. 

Cái nhìn sâu sắc quan trọng là thứ tự là không liên quan. Khi chúng tôi sắp xếp tất cả các kích thước, mọi cách lồng hợp lệ đều tương ứng với việc chọn một dãy con có giá trị tăng dần. Vì việc sắp xếp loại bỏ các mối lo ngại về hoán vị nên chúng ta chỉ cần đếm xem chúng ta có thể xâu chuỗi bao nhiêu giá trị riêng biệt, nhưng chúng ta cũng phải xem xét bội số một cách chính xác trong khung DP tổng quát hơn. 

Một cách rõ ràng để xem nó là sắp xếp mảng và tính toán chuỗi con tăng nghiêm ngặt dài nhất. Vì việc sắp xếp phá hủy cấu trúc ban đầu nên LIS trở nên tầm thường: các giá trị bằng nhau không thể mở rộng chuỗi và các giá trị tăng dần luôn có thể mở rộng. Trong một mảng được sắp xếp, điều tốt nhất chúng ta có thể làm là chọn một đại diện từ mỗi giá trị riêng biệt, nhưng điều này không phải lúc nào cũng đủ nếu chúng ta diễn giải LIS không chính xác. Cách giải thích đúng vẫn là LIS trên mảng ban đầu, nhưng vì chúng ta có thể tự do sắp xếp lại nên chúng ta có thể sắp xếp trước và giảm vấn đề thành nhóm các giá trị bằng nhau. 

Do đó, giải pháp giảm xuống việc đếm số lần chúng ta có thể chọn các giá trị sao cho mỗi giá trị tiếp theo lớn hơn hoàn toàn, tương đương với số lượng giá trị riêng biệt trong nhiều tập hợp. 

Tuy nhiên, có một góc nhìn cẩn thận hơn: nếu chúng ta sắp xếp và nén các bản sao, chuỗi dài nhất chính xác là số lượng giá trị duy nhất. 

Lực lượng vũ phu hoạt động bằng cách khám phá tất cả các tập hợp con và hoán vị, nhưng không thành công vì nó lặp lại các bước kiểm tra tính khả thi tương tự theo cấp số nhân. Quan sát cho thấy rằng chỉ sắp xếp theo giá trị mới làm giảm vấn đề trùng lặp sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tập hợp con và hoán vị Brute Force | O(n! · n) | O(n) | Quá chậm | 
| Sắp xếp + đếm các giá trị riêng biệt | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách kích cỡ búp bê. 
2. Sắp xếp danh sách theo thứ tự không giảm. Việc sắp xếp đảm bảo rằng bất kỳ chuỗi lồng hợp lệ nào cũng phải xuất hiện dưới dạng một dãy con tăng dần theo thứ tự này. 
3. Khởi tạo bộ đếm cho câu trả lời. Bắt đầu bằng cách đếm phần tử đầu tiên nếu nó tồn tại. 
4. Quét qua mảng đã sắp xếp từ trái sang phải. 
5. Mỗi lần chúng ta thấy một giá trị lớn hơn giá trị trước đó, hãy tăng bộ đếm. Bước này đảm bảo chúng tôi chỉ mở rộng chuỗi lồng khi có sẵn một con búp bê lớn hơn. 
6. Giá trị bộ đếm cuối cùng là số lượng búp bê tối đa có thể lồng vào nhau. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ chuỗi lồng hợp lệ nào cũng phải tương ứng với việc chọn một chuỗi các chỉ mục có giá trị tăng dần. Vì các giá trị bằng nhau không thể liền kề trong một chuỗi như vậy nên mỗi giá trị riêng biệt có thể đóng góp nhiều nhất một phần tử. Ngược lại, việc chọn một đại diện từ mỗi giá trị riêng biệt theo thứ tự tăng dần luôn tạo thành một chuỗi lồng hợp lệ. Điều này chứng tỏ rằng giải pháp tối ưu chính xác là số lượng giá trị riêng biệt trong mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

if n == 0:
    print(0)
    sys.exit()

cnt = 1
for i in range(1, n):
    if a[i] > a[i - 1]:
        cnt += 1

print(cnt)
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho tất cả các giá trị bằng nhau trở nên liền kề. Chúng tôi khởi tạo câu trả lời là 1 vì một mảng không trống luôn cho phép ít nhất một con búp bê trong chuỗi. Sau đó, chúng tôi quét tuyến tính và chỉ tăng câu trả lời khi gặp một giá trị lớn hơn giá trị trước đó. Điều này đảm bảo rằng các bản sao sẽ bị bỏ qua vì chúng không thể mở rộng chuỗi tăng dần. 

Một lỗi phổ biến ở đây là sử dụng`>=`thay vì`>`, điều này sẽ cho phép các kích thước bằng nhau mở rộng chuỗi một cách không chính xác. Một vấn đề tế nhị khác là quên trường hợp đầu vào trống, mặc dù các ràng buộc đảm bảo ít nhất một phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 1 2 1 3
```Mảng được sắp xếp:```
1 1 2 2 3
```| tôi | một [tôi] | trước | a[i] > trước đó | cnt | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | bắt đầu | 1 | 
| 1 | 1 | 1 | không | 1 | 
| 2 | 2 | 1 | vâng | 2 | 
| 3 | 2 | 2 | không | 2 | 
| 4 | 3 | 2 | vâng | 3 | 

Điều này cho thấy rằng chỉ những chuyển đổi riêng biệt mới đóng góp, xác nhận rằng các bản sao không làm tăng độ dài lồng nhau. 

### Ví dụ 2 

đầu vào:```
6
4 4 4 3 3 2
```Mảng được sắp xếp:```
2 3 3 4 4 4
```| tôi | một [tôi] | trước | a[i] > trước đó | cnt | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | - | bắt đầu | 1 | 
| 1 | 3 | 2 | vâng | 2 | 
| 2 | 3 | 3 | không | 2 | 
| 3 | 4 | 3 | vâng | 3 | 
| 4 | 4 | 4 | không | 3 | 
| 5 | 4 | 4 | không | 3 | 

Dấu vết xác nhận rằng các kích thước lặp lại chỉ đóng góp một lần và các bước nhảy tăng dần sẽ xác định chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | bị chi phối bởi việc sắp xếp | 
| Không gian | O(1) phụ trợ | chỉ có một vài quầy ngoài việc lưu trữ đầu vào | 

Các ràng buộc n 1000 làm cho việc này nhanh chóng một cách thoải mái. Ngay cả với nhiều trường hợp thử nghiệm, giải pháp vẫn nằm trong giới hạn do kích thước đầu vào nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    return _sys.stdout.getvalue()

def solve(inp: str) -> str:
    import sys
    from io import StringIO
    sys.stdin = StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    cnt = 1 if n > 0 else 0
    for i in range(1, n):
        if a[i] > a[i - 1]:
            cnt += 1
    return str(cnt)

# provided samples (illustrative; actual sample outputs not fully specified in statement text)
assert solve("5\n2 1 2 1 3\n") == "3"
assert solve("6\n4 4 4 3 3 2\n") == "3"

# custom cases
assert solve("1\n10\n") == "1", "single element"
assert solve("4\n1 1 1 1\n") == "1", "all equal"
assert solve("5\n5 4 3 2 1\n") == "5", "strictly decreasing becomes full chain after sort"
assert solve("6\n1 2 2 3 3 4\n") == "4", "mixed duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp tối thiểu | 
| tất cả đều bình đẳng | 1 | xử lý bất bình đẳng nghiêm ngặt | 
| dãy giảm dần | 5 | sắp xếp đúng đắn | 
| trùng lặp hỗn hợp | 4 | nén trùng lặp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị đều bằng nhau. Đối với đầu vào:```
4
7 7 7 7
```Sắp xếp mang lại trình tự tương tự. Thuật toán bắt đầu với cnt = 1 và không có phần tử nào sau này lớn hơn phần tử trước đó, do đó kết quả vẫn bằng 1. Điều này phù hợp với thực tế là chỉ có thể chọn một con búp bê. 

Một trường hợp khác là giảm nghiêm ngặt đầu vào:```
5
5 4 3 2 1
```Sau khi sắp xếp:```
1 2 3 4 5
```Mỗi bước đều tăng cnt, tạo ra 5. Điều này xác nhận rằng việc sắp xếp lại là hoàn toàn được phép và thứ tự ban đầu là không liên quan. 

Trường hợp tinh tế cuối cùng là các bản sao hỗn hợp:```
6
1 2 2 3 3 4
```Sắp xếp không thay đổi nó. Thuật toán đếm các lần chuyển đổi 1→2, 2→3, 3→4, mang lại 4. Điều này chứng tỏ rằng các bản sao không bao giờ đóng góp thêm độ sâu lồng nhau và chỉ làm tăng đáng kể vấn đề.
