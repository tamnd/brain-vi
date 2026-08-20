---
title: "CF 102180B - \u041f\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u0435 \u0433\u0430\u0440\u0434\u0435\u0440\u043e\u0431\u0430"
description: "Katya kiểm tra (n) áo phông theo một thứ tự cố định. Mỗi chiếc áo phông có một mã nhận dạng mẫu (ai). Khi cô ấy nhìn thấy một mã nhận dạng lần đầu tiên, cô ấy sẽ mua chiếc áo phông đó. Mỗi chiếc áo phông sau này có cùng số nhận dạng sẽ bị bỏ qua."
date: "2026-08-19T06:45:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102180
codeforces_index: "B"
codeforces_contest_name: "MSPU Training Contest 2018-2019"
rating: 0
weight: 102180
solve_time_s: 69
verified: true
draft: false
---

[CF 102180B - \u041f\u043e\u043f\u043e\u043b\u043d\u0435\u043d\u0438\u0435 \u0433\u0430\u0440\u0434\u0435\u0440\u043e\u0431\u0430](https://codeforces.com/problemset/problem/102180/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Katya kiểm tra (n) áo phông theo một thứ tự cố định. Mỗi chiếc áo phông có một mã nhận dạng mẫu (a_i). Khi cô ấy nhìn thấy một mã nhận dạng lần đầu tiên, cô ấy sẽ mua chiếc áo phông đó. Mỗi chiếc áo phông sau này có cùng số nhận dạng sẽ bị bỏ qua. Nhiệm vụ là tạo ra một mảng gồm (n) giá trị nhị phân, trong đó vị trí (i) chứa (1) chính xác khi áo phông ở vị trí (i) là lần xuất hiện đầu tiên của mã định danh của nó. 

Đầu vào chứa số lượng áo phông và sau đó là mã nhận dạng của chúng theo thứ tự xem. Các mã định danh có thể lớn bằng (10^9), vì vậy chúng không thể được coi là các chỉ số mảng nhỏ mà không có các giả định bổ sung. Số lượng áo phông nhiều nhất là (5000), đủ nhỏ để giải pháp bậc hai có thể khả thi ở một số ngôn ngữ, nhưng giải pháp dự định là tuyến tính và cũng đơn giản như vậy. 

Việc triển khai đơn giản có thể so sánh từng mã định danh với mọi mã định danh trước đó. Với (n=5000), điều đó có thể yêu cầu so sánh khoảng (n(n-1)/2 = 12{,}497{,}500) trong trường hợp xấu nhất. Số tiền đó không lớn nhưng không cần thiết và bộ băm đưa ra bài kiểm tra tư cách thành viên trực tiếp với thời gian trung bình không đổi. 

Trường hợp cạnh đầu tiên là một dãy chỉ chứa một chiếc áo phông. Đối với đầu vào`1`theo sau là`7`, câu trả lời là`1`, bởi vì không có sự xuất hiện trước đó để phù hợp với nó. Việc triển khai khởi tạo mọi câu trả lời về 0 và chỉ đánh dấu các giá trị sau khi phát hiện một bản sao sẽ tạo ra số 0 không chính xác. 

Một trường hợp đặc biệt khác là các số nhận dạng lặp lại ngay cạnh nhau. Đối với đầu vào`4`với số nhận dạng`9 9 9 9`, đầu ra đúng là`1 0 0 0`. đầu tiên`9`được mua và mỗi lần tiếp theo`9`đã gặp phải rồi. Việc triển khai bất cẩn chỉ so sánh với phần tử trước đó sẽ có tác dụng ở đây, nhưng lý do đó là không đủ đối với các bản sao không liên tiếp. 

Ví dụ, với đầu vào`5`và số nhận dạng`1 2 1 3 2`, đầu ra đúng là`1 1 0 1 0`. Phần tử thứ ba lặp lại phần tử đầu tiên, trong khi phần tử thứ năm lặp lại phần tử thứ hai. Cách tiếp cận chỉ kiểm tra mã định danh ngay trước đó sẽ đánh dấu không chính xác vị trí thứ ba và thứ năm là mới. 

Bản thân giá trị định danh cũng cần được quan tâm. Một đầu vào như`3`với số nhận dạng`1 1000000000 1`sản xuất`1 1 0`. Giá trị (10^9) hoàn toàn hợp lệ, nhưng nó phải được lưu trữ dưới dạng số nguyên thông thường và không được sử dụng làm chỉ mục trong một mảng lớn. 

## Phương pháp tiếp cận 

Giải pháp vũ phu trực tiếp tuân theo định nghĩa theo nghĩa đen. Đối với mỗi chiếc áo phông, hãy quét tất cả các vị trí trước nó và kiểm tra xem mã nhận dạng của nó đã xuất hiện chưa. Nếu không tìm thấy mã định danh bằng nhau, xuất ra`1`; nếu không thì xuất ra`0`. Điều này đúng vì mã định danh là mới chính xác khi không có mã định danh tương đương ở vị trí trước đó. 

Vấn đề với cách tiếp cận này là việc tìm kiếm lặp đi lặp lại. Đối với chiếc áo phông cuối cùng, chúng ta có thể kiểm tra tất cả (n-1) vị trí trước đó, đối với vị trí trước đó cho đến (n-2), v.v. Trong trường hợp xấu nhất tổng số phép so sánh là 

[ 
0+1+2+\dots+(n-1)=\frac{n(n-1)}2. 
] 

Đối với (n=5000), đây là so sánh (12{,}497{,}500). Nó vẫn có thể được chấp nhận trong điều kiện triển khai rộng rãi và giới hạn thời gian, nhưng nó mang tính bậc hai và không có quy mô mạnh mẽ như phương pháp dự định. 

Quan sát quan trọng là thông tin duy nhất chúng ta cần từ quá khứ là thông tin nhận dạng nào đã xuất hiện. Chúng tôi không quan tâm sự việc trước đó xảy ra ở đâu và nó đã xảy ra bao nhiêu lần. Một bộ đại diện chính xác cho thông tin này. Khi xử lý (a_i), tư cách thành viên trong nhóm sẽ trả lời câu hỏi "chúng ta đã thấy mô hình này trước đây chưa?" trong thời gian trung bình không đổi. Nếu câu trả lời là không, chúng tôi xuất ra`1`và ngay lập tức chèn mã định danh vào bộ. Nếu câu trả lời là có, chúng tôi xuất ra`0`. 

Phương pháp brute-force hoạt động vì nó tìm kiếm toàn bộ tiền tố đã được xử lý để xây dựng lại tập hợp các mã định danh đã thấy cho đến nay. Nhận xét rằng tập hợp này là phần có liên quan duy nhất của tiền tố cho phép chúng tôi duy trì nó một cách rõ ràng và thay thế toàn bộ quá trình quét bằng một lần tra cứu bảng băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(1)) thêm | Được chấp nhận cho những ràng buộc này, nhưng chậm không cần thiết | 
| Tối ưu | (O(n)) trung bình | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một tập hợp trống có tên`seen`. Nó sẽ chứa mọi mã định danh mô hình gặp phải trước vị trí hiện tại. 
2. Tạo một mảng kết quả trống. Chúng tôi sẽ thêm chính xác một giá trị cho mỗi chiếc áo phông, giữ nguyên thứ tự ban đầu. 
3. Xử lý các mã định danh từ trái sang phải. Đối với mã định danh hiện tại`x`, trước tiên hãy kiểm tra xem`x`đã ở trong rồi`seen`. Bộ này thể hiện chính xác những đặc điểm nhận dạng mà Katya đã gặp trước đó, vì vậy bài kiểm tra tư cách thành viên này sẽ trực tiếp trả lời xem liệu cô ấy có nên mua chiếc áo phông hiện tại hay không. 
4. Nếu`x`không có trong`seen`, nối thêm`1`đến kết quả và chèn`x`vào bộ. Việc chèn phải diễn ra ngay lập tức vì bất kỳ lần xuất hiện nào sau đó của cùng một mô hình đều phải được nhận dạng là trùng lặp. 
5. Nếu`x`đã ở trong rồi`seen`, nối thêm`0`đến kết quả và giữ nguyên tập hợp. Việc nhìn thấy lại mô hình đó không đưa ra bất kỳ thông tin mới nào. 
6. Sau khi tất cả (n) mã định danh đã được xử lý, hãy in kết quả, phân tách bằng dấu cách. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của đầu vào, bất biến là`seen`chứa chính xác các mã định danh riêng biệt xuất hiện trong tiền tố đó. Ban đầu tiền tố trống nên bất biến được giữ nguyên. Khi một mã định danh mới được xử lý, nó sẽ không có trong`seen`, do đó, thuật toán đầu ra`1`và chèn nó, bảo toàn tính bất biến. Khi một mã định danh lặp lại được xử lý thì nó đã có mặt rồi, do đó thuật toán đưa ra`0`và giữ nguyên tập hợp, đồng thời bảo toàn tính bất biến. 

Do đó, tại mọi vị trí, thuật toán đưa ra`1`chính xác khi mã định danh hiện tại chưa xuất hiện trước đó. Đó chính xác là quy tắc mua hàng bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    seen = set()
    answer = []

    for x in a:
        if x in seen:
            answer.append(0)
        else:
            answer.append(1)
            seen.add(x)

    print(*answer)

if __name__ == "__main__":
    solve()
```Hai dòng đầu tiên của`solve`đọc số lượng áo phông và trình tự nhận dạng đầy đủ. Vì câu lệnh đảm bảo rằng dòng thứ hai chứa tất cả (n) mã định danh, nên một`split()`là đủ. 

các`seen`set tương ứng trực tiếp với bước 1 của hướng dẫn. Trong vòng lặp, tư cách thành viên được kiểm tra trước khi chèn. Thứ tự này quan trọng. Nếu mã được chèn vào`x`trước khi kiểm tra tư cách thành viên, mọi số nhận dạng dường như đã được nhìn thấy và mọi câu trả lời sẽ trở thành`0`. 

Lần xuất hiện đầu tiên đi vào`else`nhánh, nhận được câu trả lời`1`, và được thêm vào`seen`. Những lần xuất hiện sau đó đi vào`if`chi nhánh và nhận`0`. Không có số học chỉ mục trong thuật toán, do đó không có ranh giới riêng biệt nào để quản lý. 

Số nguyên Python xử lý trực tiếp các giá trị lên tới (10^9), do đó mã định danh tối đa không yêu cầu xử lý đặc biệt. Tập hợp này lưu trữ tối đa (n) số nguyên riêng biệt và đầu ra chứa chính xác (n) giá trị. 

Bài toán chỉ có một ca kiểm thử nên không có vòng lặp ca kiểm thử bên ngoài.`sys.stdin.readline`được sử dụng theo yêu cầu, trong khi`print(*answer)`tạo ra đầu ra được phân tách bằng dấu cách cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mã định danh đầu vào là`1 2 3`. Mỗi mã định danh đều mới khi nó xuất hiện. 

| Vị trí | Hiện hành`x`|`x in seen`| Hành động |`seen`sau bước | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | Không | Mua và chèn 1 |`{1}`| 1 | 
| 2 | 2 | Không | Mua và chèn 2 |`{1, 2}`| 1 | 
| 3 | 3 | Không | Mua và chèn 3 |`{1, 2, 3}`| 1 | 

Kết quả đầu ra là`1 1 1`. Dấu vết này thể hiện trạng thái ban đầu và trường hợp mỗi chiếc áo phông có một mẫu riêng biệt. 

### Mẫu 2 

Mã định danh đầu vào là`1 2 1 2 3`. Các mẫu lặp lại áo phông thứ ba và thứ tư đã gặp phải. 

| Vị trí | Hiện hành`x`|`x in seen`| Hành động |`seen`sau bước | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | Không | Mua và chèn 1 |`{1}`| 1 | 
| 2 | 2 | Không | Mua và chèn 2 |`{1, 2}`| 1 | 
| 3 | 1 | Có | Bỏ qua |`{1, 2}`| 0 | 
| 4 | 2 | Có | Bỏ qua |`{1, 2}`| 0 | 
| 5 | 3 | Không | Mua và chèn 3 |`{1, 2, 3}`| 1 | 

Kết quả đầu ra là`1 1 0 0 1`. Dấu vết giải thích tại sao tập hợp phải nhớ tất cả các mã định danh trước đó, thay vì chỉ mã định danh ngay trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) trung bình | Mỗi mã định danh được kiểm tra và, trong lần xuất hiện đầu tiên, được chèn vào bộ băm | 
| Không gian | (O(n)) | Tập hợp có thể chứa tối đa (n) mã định danh riêng biệt và đầu ra cũng chứa (n) giá trị | 

Với (n\le 5000), nghiệm tuyến tính chỉ thực hiện vài nghìn phép tính trên bảng băm. Việc sử dụng bộ nhớ cũng nhỏ so với giới hạn 256 MB. Giới hạn định danh của (10^9) không ảnh hưởng đến độ phức tạp vì Python lưu trữ mỗi mã định danh dưới dạng một giá trị nguyên thay vì phân bổ một mảng được lập chỉ mục theo giá trị đó. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    seen = set()
    answer = []

    for x in a:
        if x in seen:
            answer.append(0)
        else:
            answer.append(1)
            seen.add(x)

    print(*answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("3\n1 2 3\n") == "1 1 1", "sample 1"

# Provided sample 2
assert run("5\n1 2 1 2 3\n") == "1 1 0 0 1", "sample 2"

# Provided sample 3
assert run("4\n9 9 9 9\n") == "1 0 0 0", "sample 3"

# Minimum-size input
assert run("1\n42\n") == "1", "single T-shirt"

# Maximum-size input, all identifiers distinct
n = 5000
maximum_distinct = list(range(1, n + 1))
expected = " ".join(["1"] * n)
assert run(f"{n}\n{' '.join(map(str, maximum_distinct))}\n") == expected, \
    "maximum-size distinct input"

# Maximum identifier value and non-consecutive duplicates
assert run("6\n1000000000 1 2 1000000000 2 1\n") == "1 1 1 0 0 0", \
    "identifier boundary"

# Duplicates separated by other identifiers
assert run("7\n5 8 5 9 8 10 5\n") == "1 1 0 1 0 1 0", \
    "non-consecutive duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`|`1`| Đầu vào có kích thước tối thiểu và xử lý lần xuất hiện đầu tiên | 
|`5000 / 1 2 ... 5000`| 5000 cái | Tối đa (n) và tất cả các số nhận dạng riêng biệt | 
|`6 / 1000000000 1 2 1000000000 2 1`|`1 1 1 0 0 0`| Giá trị nhận dạng tối đa và các mô hình lặp lại | 
|`7 / 5 8 5 9 8 10 5`|`1 1 0 1 0 1 0`| Các bản sao được phân tách bằng một số mã định danh khác | 

## Vỏ cạnh 

###Một chiếc áo thun đơn 

Đối với đầu vào```
1
7
```bộ này bắt đầu trống. Dấu hiệu nhận dạng duy nhất,`7`, không có trong tập hợp, do đó thuật toán sẽ thêm vào`1`và chèn`7`. Đầu ra cuối cùng là```
1
```Không có áo phông trước nên lần đầu tiên phải mua luôn. 

### Tất cả số nhận dạng đều bằng nhau 

Đối với đầu vào```
4
9 9 9 9
```cái đầu tiên`9`vắng mặt ở`seen`, do đó, thuật toán đầu ra`1`và chèn nó. Mỗi trong số ba tiếp theo`9`s đã có trong bộ này nên mỗi người sẽ sản xuất`0`. Đầu ra cuối cùng là```
1 0 0 0
```Bộ này không bao giờ cần chứa nhiều bản sao của cùng một mã định danh. Đây chính xác là lý do tại sao một tập hợp là sự thể hiện tự nhiên của lịch sử liên quan. 

### Lặp lại không liền kề 

Đối với đầu vào```
5
1 2 1 3 2
```tập hợp phát triển như`{1}`,`{1, 2}`,`{1, 2}`,`{1, 2, 3}`,`{1, 2, 3}`. Đầu ra tương ứng là```
1 1 0 1 0
```Vị trí thứ ba bị từ chối vì`1`xuất hiện ở vị trí 1, mặc dù vị trí 2 chứa mã định danh khác. Vị trí thứ năm bị từ chối vì`2`xuất hiện ở vị trí 2. Đây là trường hợp bộc lộ chiến lược không chính xác chỉ dựa trên việc so sánh các phần tử lân cận. 

### Số nhận dạng tối đa 

Đối với đầu vào```
3
1 1000000000 1
```hai mã định danh đầu tiên là mới nên chúng tạo ra`1 1`. trận chung kết`1`đã có trong bộ và sản xuất`0`. Đầu ra là```
1 1 0
```giá trị`1000000000`không yêu cầu xử lý đặc biệt. Nó được lưu trữ dưới dạng số nguyên thông thường trong tập hợp và độ lớn của nó không ảnh hưởng đến thời gian chạy của thuật toán.
