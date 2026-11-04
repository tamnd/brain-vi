---
title: "CF 1144C - Hai chuỗi xáo trộn"
description: "Chúng ta được cấp một tập hợp các số nguyên được tạo bằng cách lấy hai dãy, một dãy tăng chặt và một dãy giảm chặt chẽ, hợp nhất tất cả các phần tử của chúng lại với nhau, sau đó xáo trộn kết quả. Thứ tự ban đầu bên trong mảng đã hợp nhất bị mất."
date: "2026-06-12T03:31:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1144
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 550 (Div. 3)"
rating: 1000
weight: 1144
solve_time_s: 108
verified: false
draft: false
---

[CF 1144C - Hai chuỗi xáo trộn](https://codeforces.com/problemset/problem/1144/C) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên được tạo bằng cách lấy hai dãy, một dãy tăng chặt và một dãy giảm chặt chẽ, hợp nhất tất cả các phần tử của chúng lại với nhau, sau đó xáo trộn kết quả. 

Thứ tự ban đầu bên trong mảng đã hợp nhất bị mất. Chúng ta chỉ biết những giá trị đã xuất hiện. Nhiệm vụ của chúng ta là xây dựng lại bất kỳ cặp chuỗi hợp lệ nào có các phần tử kết hợp khớp chính xác với các số đã cho. 

Yêu cầu quan trọng là dãy tăng không được chứa các giá trị trùng lặp và dãy giảm cũng không được chứa các giá trị trùng lặp. Vì tập hợp cuối cùng xuất phát từ sự kết hợp của hai chuỗi này nên mỗi lần xuất hiện của một giá trị phải được gán cho một trong hai chuỗi. 

Kích thước đầu vào đạt$2 \cdot 10^5$. Bất kỳ giải pháp nào thử nhiều bài tập hoặc tìm kiếm khác nhau thông qua các khả năng sẽ bị loại trừ ngay lập tức. Chúng ta cần một cái gì đó gần gũi$O(n \log n)$, đó là mục tiêu tự nhiên khi có liên quan đến việc sắp xếp. 

Một vài trường hợp đáng chú ý. 

Coi như:```
3
5 5 5
```Giá trị 5 xuất hiện ba lần. Một bản sao có thể xếp vào dãy tăng dần và một bản sao vào dãy giảm dần, nhưng không có chỗ nào để đặt bản sao thứ ba. Vì cả hai chuỗi phải duy trì nghiêm ngặt nên không giá trị nào có thể xuất hiện nhiều lần trong cùng một chuỗi. Câu trả lời đúng là:```
NO
```Một giải pháp bất cẩn chỉ kiểm tra xem có tồn tại các bản sao hay không sẽ bỏ lỡ tần số lớn hơn hai là không thể. 

Coi như:```
4
1 2 3 4
```Mỗi giá trị xuất hiện một lần. Cấu trúc đúng là đặt tất cả các giá trị vào dãy tăng dần và để trống dãy giảm dần:```
YES
4
1 2 3 4
0
```Một số triển khai buộc mọi giá trị vào cả hai chuỗi một cách không cần thiết và không thành công ở các giá trị duy nhất. 

Một trường hợp tế nhị khác là:```
4
1 1 2 2
```Đã có câu trả lời hợp lệ:```
Increasing: 1 2
Decreasing: 2 1
```Dãy số giảm dần phải được in theo thứ tự giảm dần. Nếu chúng ta chỉ thu thập các giá trị trùng lặp theo thứ tự tăng dần, chúng ta sẽ xuất ra`1 2`, không giảm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử gán mọi lần xuất hiện theo trình tự tăng hoặc trình tự giảm, sau đó kiểm tra xem liệu cả hai trình tự có thể được sắp xếp để đáp ứng các yêu cầu sắp xếp nghiêm ngặt hay không. 

Nếu một giá trị xuất hiện$k$Đôi khi, mỗi bản sao đều có các lựa chọn gán. Ngay cả đối với đầu vào vừa phải, điều này sẽ trở thành cấp số nhân. Với$n = 2 \cdot 10^5$, số lượng bài tập có thể thực hiện được là hoàn toàn không thể thực hiện được. 

Cấu trúc của vấn đề mang lại một quan sát mạnh mẽ hơn nhiều. 

Vì cả hai chuỗi đều nghiêm ngặt nên một giá trị có thể xuất hiện nhiều nhất một lần trong mỗi chuỗi. Điều đó có nghĩa là bất kỳ giá trị nào cũng có thể xuất hiện tối đa hai lần về tổng thể. Khi tần số nào đó vượt quá hai thì câu trả lời là không thể. 

Giả sử một giá trị xuất hiện đúng một lần. Nó có thể được đặt một cách an toàn vào dãy tăng dần. 

Giả sử một giá trị xuất hiện chính xác hai lần. Một bản sao phải theo thứ tự tăng dần và bản sao còn lại phải theo thứ tự giảm dần. Không có sự thay thế. 

Điều này hoàn toàn quyết định nhiệm vụ. 

Sau khi sắp xếp các giá trị riêng biệt, chúng ta đặt một bản sao của mỗi giá trị riêng biệt vào dãy tăng dần. Điều này tự động tạo ra một dãy tăng dần. 

Mỗi giá trị xuất hiện hai lần sẽ đóng góp thêm một bản sao vào chuỗi giảm dần. Nếu chúng ta lưu trữ các giá trị trùng lặp đó và cuối cùng đảo ngược thứ tự sắp xếp của chúng thì kết quả sẽ giảm dần. 

Toàn bộ vấn đề giảm xuống việc đếm tần số và sắp xếp các giá trị riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số và đếm tần số của mỗi giá trị. 
2. Nếu bất kỳ giá trị nào xuất hiện nhiều hơn hai lần, hãy in`"NO"`và dừng lại. 

Một giá trị có thể xuất hiện nhiều nhất một lần trong dãy tăng dần và nhiều nhất một lần trong dãy giảm dần, vì vậy tần số ba hoặc nhiều hơn là không thể. 
3. Sắp xếp tất cả các giá trị riêng biệt theo thứ tự tăng dần. 
4. Xây dựng dãy tăng dần bằng cách lấy mỗi giá trị phân biệt đúng một lần. 

Vì các giá trị được sắp xếp và phân biệt nên dãy tăng dần. 
5. Với mỗi giá trị có tần số chính xác bằng 2, hãy thêm một bản sao vào danh sách thứ hai. 

Đây là những bản sao bổ sung không thể tồn tại theo trình tự tăng dần. 
6. Đảo ngược danh sách thứ hai này. 

Các giá trị trùng lặp được thu thập theo thứ tự tăng dần. Đảo ngược chúng tạo ra một dãy giảm dần. 
7. In`"YES"`và xuất ra cả hai chuỗi. 

### Tại sao nó hoạt động 

Mọi giá trị có tần số bằng một phải xuất hiện chính xác theo một chuỗi và việc đặt nó vào chuỗi tăng dần không bao giờ tạo ra xung đột. 

Mỗi giá trị có tần số 2 phải đóng góp một bản sao cho mỗi chuỗi. Đặt một bản sao vào chuỗi tăng dần và một bản sao vào chuỗi giảm dần là phép gán hợp lệ duy nhất có thể. 

Chuỗi tăng dần chứa mỗi giá trị riêng biệt một lần, theo thứ tự tăng dần, do đó, nó tăng dần. 

Chuỗi giảm dần chứa chính xác các giá trị trùng lặp, mỗi giá trị xuất hiện một lần, theo thứ tự giảm dần, do đó nó giảm dần. 

Vì mỗi lần xuất hiện được chỉ định chính xác một lần và tần số lớn hơn hai bị loại bỏ nên các chuỗi được tạo sẽ tạo thành một phân vùng hợp lệ của nhiều tập hợp ban đầu bất cứ khi nào có giải pháp. 

## Giải pháp Python```python
import sys
from collections import Counter

input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

freq = Counter(a)

for cnt in freq.values():
    if cnt > 2:
        print("NO")
        sys.exit()

inc = []
dec = []

for x in sorted(freq):
    inc.append(x)
    if freq[x] == 2:
        dec.append(x)

dec.reverse()

print("YES")
print(len(inc))
if inc:
    print(*inc)
else:
    print()

print(len(dec))
if dec:
    print(*dec)
else:
    print()
```Bảng tần số là cấu trúc dữ liệu trung tâm. Nó ngay lập tức phát hiện các trường hợp không thể xảy ra khi một giá trị xuất hiện nhiều hơn hai lần. 

Chuỗi tăng dần được xây dựng từ các giá trị riêng biệt được sắp xếp. Việc sử dụng các giá trị riêng biệt đảm bảo tính nghiêm ngặt một cách tự động. 

Chuỗi giảm dần chứa chính xác những giá trị xuất hiện hai lần. Thứ tự là quan trọng. Việc thu thập các bản sao trong quá trình truyền tải tăng dần sẽ đưa ra một danh sách ngày càng tăng, vì vậy chúng tôi đảo ngược danh sách đó trước khi in. 

Trình tự trống được xử lý rõ ràng. Vấn đề cho phép chúng, vì vậy khi một chuỗi có độ dài bằng 0, chúng tôi vẫn in dòng yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
7 2 7 3 3 1 4
```Bảng tần số: 

| Giá trị | Tần số | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 1 | 
| 7 | 2 | 

Xử lý: 

| Giá trị hiện tại | Tần số | inc | tháng 12 trước khi đảo ngược | 
| --- | --- | --- | --- | 
| 1 | 1 | [1] | [] | 
| 2 | 1 | [1,2] | [] | 
| 3 | 2 | [1,2,3] | [3] | 
| 4 | 1 | [1,2,3,4] | [3] | 
| 7 | 2 | [1,2,3,4,7] | [3,7] | 

Sau khi đảo ngược: 

| inc | tháng mười hai | 
| --- | --- | 
| [1,2,3,4,7] | [7,3] | 

Đầu ra:```
YES
5
1 2 3 4 7
2
7 3
```Điều này thể hiện ý chính. Mỗi giá trị riêng biệt xuất hiện một lần trong chuỗi tăng dần, trong khi bản sao thứ hai của các giá trị trùng lặp sẽ chuyển sang chuỗi giảm dần. 

### Ví dụ 2 

đầu vào:```
4
1 1 2 2
```Bảng tần số: 

| Giá trị | Tần số | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 

Xử lý: 

| Giá trị hiện tại | Tần số | inc | tháng 12 trước khi đảo ngược | 
| --- | --- | --- | --- | 
| 1 | 2 | [1] | [1] | 
| 2 | 2 | [1,2] | [1,2] | 

Sau khi đảo ngược: 

| inc | tháng mười hai | 
| --- | --- | 
| [1,2] | [2,1] | 

Đầu ra:```
YES
2
1 2
2
2 1
```Ví dụ này cho thấy tại sao thao tác ngược lại là cần thiết. Không có nó, dãy giảm sẽ không thực sự giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Đếm tần số là$O(n)$, việc sắp xếp các giá trị riêng biệt chiếm ưu thế | 
| Không gian |$O(n)$| Bảng tần số và chuỗi đầu ra | 

Đầu vào lớn nhất chứa$2 \cdot 10^5$những con số. Việc sắp xếp nhiều giá trị đó dễ dàng nằm trong giới hạn và mức sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    freq = Counter(a)

    for cnt in freq.values():
        if cnt > 2:
            return "NO"

    inc = []
    dec = []

    for x in sorted(freq):
        inc.append(x)
        if freq[x] == 2:
            dec.append(x)

    dec.reverse()

    out = []
    out.append("YES")
    out.append(str(len(inc)))
    out.append(" ".join(map(str, inc)))
    out.append(str(len(dec)))
    out.append(" ".join(map(str, dec)))

    return "\n".join(out)

# sample
assert run("7\n7 2 7 3 3 1 4\n").startswith("YES")

# minimum size
assert run("1\n5\n") == "YES\n1\n5\n0\n"

# all equal, impossible
assert run("3\n7 7 7\n") == "NO"

# duplicates exactly twice
assert run("4\n1 1 2 2\n") == "YES\n2\n1 2\n2\n2 1"

# all distinct
assert run("5\n1 2 3 4 5\n") == "YES\n5\n1 2 3 4 5\n0\n"

# boundary style case
assert run("2\n0 200000\n") == "YES\n2\n0 200000\n0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`| Giải pháp đơn phần tử | Kích thước tối thiểu | 
|`7 7 7`|`NO`| Tần số lớn hơn hai | 
|`1 1 2 2`| Chia hợp lệ | Xử lý trùng lặp | 
|`1 2 3 4 5`| Chuỗi giảm dần trống | Tất cả các giá trị duy nhất | 
|`0 200000`| Chia hợp lệ | Phạm vi giá trị cực cao | 

## Vỏ cạnh 

Hãy xem xét:```
3
5 5 5
```Tần số của 5 là 3. Trong quá trình quét tần số, thuật toán ngay lập tức phát hiện số lượng lớn hơn hai và in:```
NO
```Điều này đúng vì hai chuỗi nghiêm ngặt có thể chứa tối đa hai bản sao của bất kỳ giá trị nào. 

Coi như:```
4
1 2 3 4
```Tất cả các tần số đều bằng nhau. 

Cấu trúc thuật toán:```
inc = [1, 2, 3, 4]
dec = []
```Chuỗi giảm dần trống, vấn đề này cho phép rõ ràng. 

Coi như:```
4
1 1 2 2
```Thuật toán xây dựng:```
inc = [1, 2]
dec before reverse = [1, 2]
```Sau khi đảo ngược:```
dec = [2, 1]
```Cả hai chuỗi đều nghiêm ngặt và mỗi lần xuất hiện từ nhiều tập hợp ban đầu được sử dụng chính xác một lần. 

Coi như:```
5
1 1 2 3 3
```Thuật toán tạo ra:```
inc = [1, 2, 3]
dec = [3, 1]
```Trình tự tăng tiếp tục tăng nghiêm ngặt, trình tự giảm tiếp tục giảm nghiêm ngặt và tất cả năm lần xuất hiện ban đầu đều được tính đến. Điều này xác nhận rằng các giá trị trùng lặp có thể cùng tồn tại an toàn miễn là không có giá trị nào xuất hiện quá hai lần.
