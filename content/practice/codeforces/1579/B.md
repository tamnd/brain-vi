---
title: "CF 1579B - Sắp xếp dịch chuyển"
description: "Chúng tôi được cung cấp một mảng và một hoạt động đặc biệt. Thay vì hoán đổi các phần tử riêng lẻ, chúng ta có thể chọn bất kỳ đoạn liền kề nào và xoay đoạn đó sang trái một lượng bất kỳ. Sau khi xoay, các phần tử vẫn nằm trong cùng một khoảng, chỉ có thứ tự của chúng thay đổi theo chu kỳ."
date: "2026-06-10T10:20:45+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1579
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 744 (Div. 3)"
rating: 1100
weight: 1579
solve_time_s: 130
verified: false
draft: false
---

[CF 1579B - Sắp xếp dịch chuyển](https://codeforces.com/problemset/problem/1579/B) 

**Đánh giá:** 1100 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và một hoạt động đặc biệt. Thay vì hoán đổi các phần tử riêng lẻ, chúng ta có thể chọn bất kỳ đoạn liền kề nào và xoay đoạn đó sang trái một lượng bất kỳ. Sau khi xoay, các phần tử vẫn nằm trong cùng một khoảng, chỉ có thứ tự của chúng thay đổi theo chu kỳ. 

Nhiệm vụ là chuyển đổi mảng thành thứ tự không giảm. Chúng tôi không phải giảm thiểu số lượng hoạt động. Bất kỳ trình tự nào sử dụng tối đa$n$luân chuyển được chấp nhận. 

Mỗi trường hợp thử nghiệm chứa một mảng. Đối với mọi trường hợp thử nghiệm, chúng ta phải xuất ra số lần xoay mà chúng ta thực hiện và sau đó mô tả mỗi phép quay theo điểm cuối bên trái, điểm cuối bên phải và số lượng dịch chuyển của nó. 

Những hạn chế là rất nhỏ. Độ dài mảng tối đa là 50 và có tối đa 1000 trường hợp thử nghiệm. Thậm chí một$O(n^3)$giải pháp chỉ thực hiện khoảng$50^3=125000$hoạt động cho mỗi trường hợp thử nghiệm, đủ nhanh. Không cần cấu trúc dữ liệu phức tạp. 

Một điểm tinh tế là sự hiện diện của các giá trị trùng lặp. Giả sử mảng là```
3
2 1 1
```Phiên bản được sắp xếp là```
1 1 2
```Khi cố định vị trí 0, chúng ta phải di chuyển lần xuất hiện đầu tiên không được sử dụng của giá trị 1, không phải giá trị tùy ý. Nếu không, chúng ta có thể làm xáo trộn vị trí tương đối của các giá trị bằng nhau và các vị trí sau này trở nên khó khớp hơn. 

Một trường hợp góc khác xảy ra khi phần tử chính xác đã được đặt đúng chỗ. Ví dụ```
4
1 2 3 4
```Câu trả lời không được chứa hoạt động nào. Việc triển khai bất cẩn vẫn có thể tạo ra các phép quay vô ích với độ dịch chuyển bằng 0, nhưng vấn đề đòi hỏi số lượng dịch chuyển phải dương. 

Mảng chứa các phần tử bằng nhau cũng cần được chú ý. Vì```
4
5 5 5 5
```mảng đã được sắp xếp, vì vậy câu trả lời đúng lại là không có phép tính nào. Tìm kiếm giá trị phù hợp đầu tiên và xoay vòng một cách không cần thiết sẽ chỉ làm phức tạp thêm mọi thứ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tìm kiếm trong tất cả các chuỗi phép quay có thể có cho đến khi một mảng được sắp xếp xuất hiện. Vì mỗi khoảng thời gian và mỗi số ca đều có sẵn nên số lượng khả năng sẽ tăng lên một cách bùng nổ. Ngay cả đối với$n=50$, cách tiếp cận này là vô vọng. 

Quan sát hữu ích là chúng ta không cần tối ưu hóa số lượng thao tác. Chúng tôi chỉ cần sử dụng tối đa một số trình tự$n$các phép quay. 

Hãy suy nghĩ về sắp xếp chèn. Tại vị trí$i$, một khi vị trí$0\ldots i-1$đã đúng rồi, chúng ta chỉ cần đưa phần tử chiếm vị trí$i$vào nơi đó. Nếu phần tử đó hiện đang ở vị trí$j$, chúng ta có thể chọn đoạn$[i,j]$và xoay nó sang trái$j-i$. 

Giả sử đoạn đó là```
[x1, x2, ..., xk, target]
```Xoay trái bởi$k$sản xuất```
[target, x1, x2, ..., xk]
```Chính xác phần tử mong muốn sẽ di chuyển đến vị trí$i$, trong khi tất cả các vị trí cố định trước đó vẫn không bị ảnh hưởng. Vì mỗi vị trí cần nhiều nhất một vòng quay nên chúng tôi thực hiện nhiều nhất$n-1$hoạt động. 

Cách tiếp cận bạo lực hoạt động vì các phép quay đủ biểu cảm để đạt được bất kỳ hoán vị nào, nhưng nó không thành công vì không gian tìm kiếm rất lớn. Quan sát thấy rằng một phép quay được chọn cẩn thận có thể đặt một vị trí vĩnh viễn sẽ giảm vấn đề xuống một quy trình giống như sắp xếp chèn đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một bản sao đã được sắp xếp của mảng. 
2. Xử lý các vị trí từ trái qua phải. 
3. Đối với vị trí hiện tại$i$, tìm vị trí đầu tiên$j\ge i$có giá trị bằng giá trị sẽ xuất hiện ở vị trí$i$trong mảng đã sắp xếp. 
4. Nếu$j=i$, phần tử chính xác đã có sẵn ở đó, vì vậy hãy tiếp tục. 
5. Ngược lại hãy xem xét phân đoạn$[i,j]$. Xoay đoạn này sang trái$j-i$. 
6. Sau khi quay, phần tử ban đầu ở vị trí$j$di chuyển đến vị trí$i$, trong khi tất cả các phần tử giữa$i$Và$j-1$chuyển một vị trí sang phải. 
7. Ghi lại thao tác này và tiếp tục với vị trí tiếp theo. 

### Tại sao nó hoạt động 

Bất biến là trước khi xử lý vị trí$i$, tiền tố$0\ldots i-1$đã khớp với mảng được sắp xếp. 

Khi chúng tôi xác định vị trí giá trị cần thiết tại vị trí$j$và xoay đoạn$[i,j]$, chỉ các vị trí từ$i$ĐẾN$j$thay đổi. Vị trí trước đây$i$không bị ảnh hưởng nên tính bất biến được bảo toàn. Giá trị mong muốn di chuyển chính xác vào vị trí$i$, tạo tiền tố$0\ldots i$Chính xác. 

Bằng quy nạp, sau khi kết thúc tất cả các vị trí, mọi chỉ số đều chứa cùng một giá trị với mảng đã được sắp xếp, do đó toàn bộ mảng được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        target = sorted(a)
        ops = []

        for i in range(n):
            j = i
            while j < n and a[j] != target[i]:
                j += 1

            if j == i:
                continue

            ops.append((i + 1, j + 1, j - i))

            val = a[j]
            for k in range(j, i, -1):
                a[k] = a[k - 1]
            a[i] = val

        out.append(str(len(ops)))
        for l, r, d in ops:
            out.append(f"{l} {r} {d}")

    sys.stdout.write("\n".join(out))

solve()
```Thuật toán đầu tiên xây dựng mảng mục tiêu đã sắp xếp. Điều này cho chúng ta biết giá trị nào sẽ chiếm từng vị trí. 

Đối với mọi chỉ mục, chúng tôi tìm kiếm ở bên phải cho đến khi tìm thấy lần xuất hiện đầu tiên của giá trị cần thiết. Việc sử dụng lần xuất hiện đầu tiên rất quan trọng khi có sự trùng lặp. Việc chọn một bản sao sau vẫn có tác dụng trong nhiều trường hợp, nhưng việc chọn bản sao sớm nhất sẽ bắt chước cách sắp xếp chèn một cách tự nhiên và giữ cho số lượng thao tác ở mức nhỏ. 

Các tham số hoạt động sử dụng chỉ mục dựa trên 1 vì đó là những gì câu lệnh yêu cầu. 

Bản cập nhật mảng thực tế xứng đáng được quan tâm. Đoạn quay$[i,j]$để lại bởi$j-i$tương đương với việc lấy phần tử`a[j]`, đặt nó ở vị trí`i`và dịch chuyển các phần tử giữa chúng sang phải một bước. Việc thực hiện trực tiếp điều này sẽ tránh tạo ra các lát cắt tạm thời và giữ cho logic trong suốt. 

Bởi vì mỗi vị trí kích hoạt tối đa một thao tác nên số lượng thao tác không bao giờ vượt quá$n-1$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:```
2 4 1 3
```Mục tiêu được sắp xếp:```
1 2 3 4
```| tôi | Giá trị mục tiêu | tìm thấy j | Mảng sau thao tác | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | [1,2,4,3] | (1,3,2) | 
| 1 | 2 | 1 | [1,2,4,3] | không | 
| 2 | 3 | 3 | [1,2,3,4] | (3,4,1) | 
| 3 | 4 | 3 | [1,2,3,4] | không | 

Vòng quay đầu tiên di chuyển 1 vào phía trước. Vòng quay không cần thiết thứ hai cố định vị trí 2. Các vị trí cố định trước đó không bao giờ bị xáo trộn. 

### Ví dụ 2 

Mảng đầu vào:```
2 5 1 4 3
```Mục tiêu được sắp xếp:```
1 2 3 4 5
```| tôi | Giá trị mục tiêu | tìm thấy j | Mảng sau thao tác | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | [1,2,5,4,3] | (1,3,2) | 
| 1 | 2 | 1 | [1,2,5,4,3] | không | 
| 2 | 3 | 4 | [1,2,3,5,4] | (3,5,2) | 
| 3 | 4 | 4 | [1,2,3,4,5] | (4,5,1) | 
| 4 | 5 | 4 | [1,2,3,4,5] | không | 

Ví dụ này cho thấy một thao tác sẽ cố định vĩnh viễn một vị trí mới. Sau khi xử lý chỉ số 2, tiền tố`[1,2,3]`đã là cuối cùng rồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi vị trí thực hiện tìm kiếm tuyến tính và dịch chuyển tuyến tính | 
| Không gian | O(n) | Chỉ bản sao đã sắp xếp và danh sách thao tác mới được lưu trữ | 

Từ$n\le50$, ngay cả thuật toán bậc hai cũng cực kỳ nhanh. Việc sử dụng bộ nhớ không đáng kể so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        target = sorted(a)
        ops = []

        for i in range(n):
            j = i
            while j < n and a[j] != target[i]:
                j += 1

            if j == i:
                continue

            ops.append((i + 1, j + 1, j - i))

            val = a[j]
            for k in range(j, i, -1):
                a[k] = a[k - 1]
            a[i] = val

        ans.append(str(len(ops)))
        for x in ops:
            ans.append("{} {} {}".format(*x))

    return "\n".join(ans)

# minimum size
assert run("1\n2\n2 1\n") == "1\n1 2 1"

# already sorted
assert run("1\n4\n1 2 3 4\n") == "0"

# all equal values
assert run("1\n4\n5 5 5 5\n") == "0"

# duplicates
assert run("1\n3\n2 1 1\n") == "1\n1 2 1"

# off-by-one case
assert run("1\n5\n5 1 2 3 4\n") == "1\n1 5 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`| một thao tác | Kích thước tối thiểu | 
|`1 2 3 4`| hoạt động bằng không | Đã sắp xếp mảng | 
|`5 5 5 5`| hoạt động bằng không | Giá trị bằng nhau | 
|`2 1 1`| một thao tác | Xử lý trùng lặp | 
|`5 1 2 3 4`| thay đổi bốn | Chỉ số ranh giới | 

## Vỏ cạnh 

Xem xét các giá trị trùng lặp:```
1
3
2 1 1
```Mảng được sắp xếp là`[1,1,2]`. 

Tại vị trí 0, chúng ta cần số 1. Lần xuất hiện đầu tiên xuất hiện ở chỉ số 1. Đoạn quay`[0,1]`cho```
[1,2,1]
```Vị trí 0 bây giờ là chính xác. Ở vị trí 1, chúng ta lại cần số 1, số này đã có sẵn ở chỉ mục 2. Một phép xoay khác sẽ sắp xếp mảng, nhưng vì chỉ mục 1 hiện chứa 2 nên thuật toán sẽ tự động thực hiện thao tác đó nếu cần. Sự trùng lặp không gây ra sự mơ hồ vì chúng tôi luôn tìm kiếm từ chỉ mục hiện tại trở đi. 

Bây giờ hãy xem xét một mảng đã được sắp xếp:```
1
4
1 2 3 4
```Đối với mọi vị trí, việc tìm kiếm sẽ tìm thấy ngay`j=i`. Không có hoạt động nào được ghi lại. Đầu ra chứa các phép toán bằng 0, thỏa mãn các yêu cầu của bài toán. 

Cuối cùng xem xét tất cả các giá trị bằng nhau:```
1
4
7 7 7 7
```Bản sao được sắp xếp giống hệt với mảng ban đầu. Mọi tìm kiếm đều dừng ngay lập tức, không có phép quay nào được thực hiện và thuật toán đưa ra chính xác các hoạt động bằng 0. Việc triển khai bất cẩn làm xoay các phân đoạn một cách mù quáng sẽ vi phạm yêu cầu số lượng dịch chuyển phải dương.
