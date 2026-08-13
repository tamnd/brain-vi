---
title: "CF 102373J - Biến đổi"
description: "Chúng ta có hai hoán vị của cùng một nhóm bạn. Hoán vị đầu tiên a là thứ tự hiện tại và hoán vị thứ hai b là thứ tự bắt buộc. Một thao tác duy nhất sẽ chọn bất kỳ nhóm bạn bè nào không trống."
date: "2026-08-12T23:22:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "J"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 546
verified: false
draft: false
---

[CF 102373J - Biến đổi](https://codeforces.com/problemset/problem/102373/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 6 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hoán vị của cùng một nhóm bạn. Hoán vị đầu tiên`a`là thứ tự hiện tại và hoán vị thứ hai`b`là thứ tự cần thiết. 

Một thao tác duy nhất sẽ chọn bất kỳ nhóm bạn bè nào không trống. Những người bạn được chọn sẽ bị xóa khỏi vị trí hiện tại của họ, thứ tự tương đối của họ bị đảo ngược và trình tự kết quả được đặt ở phía trước. Những người bạn không được chọn giữ nguyên thứ tự tương đối của họ. 

Ví dụ: nếu trình tự hiện tại là`1 2 3 4 5`và chúng tôi chọn`{2, 4}`, những người bạn được chọn sẽ xuất hiện dưới dạng`2, 4`, vì vậy sau khi đảo ngược chúng và di chuyển chúng lên phía trước, chúng ta thu được`4 2 1 3 5`. 

Chúng tôi không cần số lượng hoạt động tối thiểu. Chúng ta chỉ cần một số chuỗi tối đa 15 thao tác thay đổi`a`vào trong`b`. Đầu ra chứa số thao tác theo sau là tập hợp số bạn bè được chọn trong mỗi thao tác. 

giá trị`n`nhiều nhất là 10000. Con số này đủ lớn để việc cố gắng hoán vị một cách rõ ràng là không thể, vì có`n!`các đơn đặt hàng có thể. Ngay cả việc xem xét mọi tập hợp con cho một thao tác cũng đã mang lại`2^n - 1`những khả năng vô vọng ở`n = 10000`. Phần hữu ích của các ràng buộc là giới hạn 15 thao tác. Từ`2^14 = 16384 > 10000`, việc xây dựng dựa trên 14 quyết định nhị phân là đủ. 

Có hai trường hợp tế nhị rất dễ xử lý sai. Đầu tiên, khi`n = 1`, chỉ có một hoán vị có thể xảy ra. Ví dụ,```text
1
1
1
```đã đáp ứng được mục tiêu nên kết quả đầu ra chính xác chỉ đơn giản là`0`. Một công trình tính toán mù quáng`ceil(log2(n))`và sau đó giả sử có ít nhất một thao tác tồn tại có thể vô tình tạo ra một thao tác trống không hợp lệ. 

Thứ hai, các hoán vị đầu vào được đảm bảo chứa các giá trị riêng biệt. Vì vậy, một bài kiểm tra như```text
3
1 1 2
1 2 1
```hoàn toàn không phải là một trường hợp thử nghiệm hợp lệ. Không cần phải xử lý các giá trị lặp lại và bất kỳ cách xây dựng nào dựa vào việc mỗi người bạn có một vị trí mục tiêu duy nhất đều được chứng minh bằng đầu vào. 

Một trường hợp ranh giới khác là lũy thừa của hai. Vì```text
4
4 3 2 1
1 2 3 4
```chúng ta cần chính xác hai bit để phân biệt cả bốn vị trí mục tiêu. Cấu trúc sử dụng bốn mã hai bit riêng biệt, do đó không cần thêm thao tác nào chỉ vì`n`đạt đến dung lượng của không gian mã. 

Đối với một số không có sức mạnh của hai, chẳng hạn như`n = 5`, chúng tôi sử dụng năm mã đầu tiên theo thứ tự phù hợp của tất cả tám mã ba bit. Các mã không được sử dụng đơn giản là không bao giờ thuộc về bất kỳ người bạn nào, vì vậy chúng không ảnh hưởng đến hoán vị kết quả. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng tìm kiếm thông qua các hoạt động có thể. có`2^n - 1`các tập con khác rỗng có thể được chọn trong một thao tác. Áp dụng một ứng cử viên cho một chi phí chuỗi`O(n)`nếu trình tự được xây dựng lại một cách rõ ràng, do đó thậm chí kiểm tra tất cả các chi phí hoạt động đầu tiên có thể có`O(n 2^n)`. Tìm kiếm sâu nhiều cấp độ còn tệ hơn nhiều, với hệ số phân nhánh khoảng`2^n`. Việc tìm kiếm đường đi ngắn nhất trên các hoán vị cũng không thể thực hiện được vì không gian trạng thái có`n!`tiểu bang. 

Lực lượng vũ phu hoạt động về mặt khái niệm vì mọi động thái hợp pháp đều được thể hiện rõ ràng giữa các tập hợp con đó. Sự thất bại của nó hoàn toàn là do số lượng lớn các tập hợp con có thể có và dẫn đến các hoán vị. 

Quan sát quan trọng là một hoạt động có thể được mô tả bằng cách sử dụng một quyết định nhị phân duy nhất cho mỗi người bạn. Cho mỗi người bạn biết một chút liệu người bạn đó có tham gia hoạt động hay không. Những người bạn đã chọn sẽ được di chuyển trước tất cả những người bạn không được chọn và phần được chọn sẽ bị đảo ngược. 

Giả sử chúng ta thực hiện một số thao tác và gán cho mỗi người bạn một mã nhị phân bao gồm các bit tham gia của nó. Thứ tự cuối cùng được xác định bởi các mã đó. Điều thú vị là thứ tự của các mã không phải là thứ tự nhị phân thông thường. Bởi vì mọi nhóm được chọn đều bị đảo ngược nên thứ tự mã tự nhiên là thứ tự mã Gray được phản ánh. 

Vì`k`hoạt động, có`2^k`mã nhị phân có thể. Nếu tất cả bạn bè nhận được các mã khác nhau, thứ tự ban đầu của họ sẽ không còn hiệu lực. Chúng ta chỉ cần gán mã để chuỗi thao tác tạo ra thứ tự mục tiêu mong muốn. 

Cho phép`G_k`là chuỗi mã Gray phản ánh nhị phân thông thường. Đối với ba bit nó là`000, 001, 011, 010, 110, 111, 101, 100`. 

Thứ tự được tạo ra bởi các hoạt động của chúng tôi là đảo ngược của trình tự này:`100, 101, 111, 110, 010, 011, 001, 000`. 

Có công thức trực tiếp cho mã tại vị trí đích`r`:`code[r] = gray(2^k - 1 - r)`,

Ở đâu`gray(x) = x XOR (x >> 1)`. 

Chúng tôi gán các mã này cho bạn bè theo thứ tự chúng xuất hiện trong`b`. Sau đó hoạt động`i`chọn chính xác những người bạn có mã được gán có bit`i`bộ. 

Việc xây dựng chỉ cần`ceil(log2 n)`hoạt động. Từ`n <= 10000`, nhiều nhất là 14, thoải mái dưới giới hạn yêu cầu là 15. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu |`O(n 2^n)`chỉ dành cho một cấp độ tìm kiếm |`O(n)`mỗi tiểu bang | Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n log n)`trong cách biểu diễn đơn giản | Đã chấp nhận | 

Việc triển khai thực tế chỉ có thể sử dụng`O(n)`bộ nhớ bổ sung vì mọi mã đều được tạo khi cần, do đó mức sử dụng không gian thực tế thậm chí còn nhỏ hơn giới hạn chung của bảng. 

## Hướng dẫn thuật toán 

1. Nếu`a`đã bằng rồi`b`, xuất ra các hoạt động bằng 0. Điều này không bắt buộc để đảm bảo tính chính xác của cách xây dựng chung, nhưng nó đưa ra câu trả lời và xử lý đơn giản nhất có thể`n = 1`một cách tự nhiên. 

2. Chọn cái nhỏ nhất`k`như vậy`2^k >= n`. Có đủ sự khác biệt`k`-bit mã để cung cấp cho mỗi vị trí mục tiêu một mã duy nhất. Bởi vì`n <= 10000`, chúng tôi luôn có`k <= 14`. 

3. Đánh số các vị trí hoán vị mong muốn từ`0`bởi vì`n - 1`. Đối với vị trí`r`, tính toán`x = 2^k - 1 - r`rồi gán mã`x XOR (x >> 1)`. 

Đây chính xác là những điều đầu tiên`n`mã của thứ tự mã Gray phản ánh nhị phân ngược. 

4. Đối với mỗi bit`i`từ`0`bởi vì`k - 1`, xây dựng một hoạt động. Quét hoán vị đích`b`. Nếu mã được giao của`b[r]`có chút`i`đặt, đặt bạn bè`b[r]`đi vào hoạt động`i`. 

Thứ tự in số bạn bè đã chọn không quan trọng. Bản thân hoạt động sẽ khôi phục thứ tự tương đối hiện tại của chúng trước khi đảo ngược chúng. 

5. Xuất tất cả`k`các phép toán theo thứ tự bit tăng dần, từ bit`0`cắn`k - 1`. 

Thứ tự của các hoạt động này là cần thiết. Hoạt động cuối cùng có ảnh hưởng lớn nhất đến nhóm cuối cùng và sự đảo ngược đệ quy do các hoạt động gây ra chính xác là thứ mang lại thứ tự mã Gray đảo ngược. 

### Tại sao nó hoạt động 

Hãy xem xét tất cả`2^k`những mã có thể Sau thao tác đầu tiên, mã hóa bằng bit`0`bằng`1`di chuyển lên phía trước theo thứ tự ngược lại, trong khi mã có bit`0`bằng`0`ở lại sau đó. Sau thao tác tiếp theo, quá trình tương tự diễn ra theo bit`1`, với phần được chọn bị đảo ngược. Tiếp tục theo cách này sẽ cho chuỗi đệ quy`C_k = 1 + reverse(C_{k-1}), 0 + C_{k-1}`khi bit cao được ghi đầu tiên. Trình tự cơ sở là`C_1 = [1, 0]`. 

Mã Gray phản ánh tiêu chuẩn thỏa mãn`G_k = 0 + G_{k-1}, 1 + reverse(G_{k-1})`. 

Đảo ngược danh tính này mang lại`reverse(G_k) = 1 + G_{k-1}, 0 + reverse(G_{k-1})`, 

đó chính xác là trình tự được tạo ra bởi các hoạt động của chúng tôi. Do đó thứ tự cuối cùng của các mã riêng biệt là`reverse(G_k)`. 

Chúng tôi chỉ định đầu tiên`n`mã của chuỗi này cho các phần tử của`b`theo thứ tự mục tiêu. Do đó, sau mọi thao tác, bạn bè xuất hiện đúng theo thứ tự`b`. Vì mỗi mã được gán là khác nhau nên không có cặp bạn nào cần thứ tự tương đối ban đầu của nó để phá vỡ mối ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_operations(a, b):
    n = len(a)

    if a == b:
        return []

    k = 0
    while (1 << k) < n:
        k += 1

    operations = []

    # code[r] is the r-th code in reverse Gray-code order.
    # We only need the codes for the n target positions.
    codes = [0] * n
    full = (1 << k) - 1

    for r in range(n):
        x = full - r
        codes[r] = x ^ (x >> 1)

    for bit in range(k):
        mask = 1 << bit
        selected = []

        for r in range(n):
            if codes[r] & mask:
                selected.append(b[r])

        if selected:
            operations.append(selected)

    return operations

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    operations = build_operations(a, b)

    print(len(operations))
    for op in operations:
        print(len(op), *op)

if __name__ == "__main__":
    solve()
```các`build_operations`đầu tiên hàm xử lý trường hợp đã được sắp xếp. Điều này tránh tạo ra các hoạt động không cần thiết và cũng có nghĩa là`n = 1`ngay lập tức tạo ra hoạt động bằng không. 

Tính toán vòng lặp`k`tìm số bit nhỏ nhất có khả năng biểu diễn ít nhất`n`những giá trị khác nhau. Vì`n = 10000`, nó dừng lại ở`k = 14`bởi vì`2^13 = 8192`trong khi đó là quá nhỏ`2^14 = 16384`là đủ. 

biểu hiện```python
x ^ (x >> 1)
```là sự chuyển đổi mã nhị phân sang mã xám tiêu chuẩn. Chúng tôi sử dụng`full - r`còn hơn là`r`bởi vì thứ tự được yêu cầu là thứ tự mã Gray ngược. 

các`codes`mảng lưu trữ một số nguyên cho mỗi vị trí mục tiêu. Từ`b[r]`là người bạn phải chiếm giữ vị trí mục tiêu`r`, mã được lưu trữ tại`r`thuộc về`b[r]`. 

Vòng lặp lồng nhau cuối cùng xây dựng các hoạt động thực tế. Một người bạn được chọn trong hoạt động`bit`chính xác khi mã của nó có tập bit đó. Hoạt động không yêu cầu chúng ta biết vị trí hiện tại của người bạn, đây là lợi thế chính khi triển khai công trình. 

Không có vấn đề tràn số nguyên trong Python. Ngay cả trong các ngôn ngữ có số nguyên có chiều rộng cố định, giá trị lớn nhất ở đây là bên dưới`2^14`, vì vậy số nguyên 32 bit thông thường là quá đủ. 

Số bạn bè được in có thể xuất hiện theo bất kỳ thứ tự nào vì câu lệnh chỉ yêu cầu tập hợp con. Thẩm phán xây dựng lại dãy con đã chọn bằng cách sử dụng hoán vị hiện tại, sau đó đảo ngược nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```text
5
5 4 3 2 1
3 4 5 1 2
```Chúng tôi cần`k = 3`bởi vì`2^2 < 5 <= 2^3`. Chuỗi mã Gray ngược cho ba bit bắt đầu`100, 101, 111, 110, 010, ...`vì vậy các vị trí mục tiêu nhận được các mã sau. 

| Vị trí mục tiêu | Bạn | Mã | Bit 0 | Bit 1 | Bit 2 | 
|---:|---:|---:|---:|---:|---:| 
| 0 | 3 |`100`| 0 | 0 | 1 | 
| 1 | 4 |`101`| 1 | 0 | 1 | 
| 2 | 5 |`111`| 1 | 1 | 1 | 
| 3 | 1 |`110`| 0 | 1 | 1 | 
| 4 | 2 |`010`| 0 | 1 | 0 | 

Do đó, các hoạt động được 

| Hoạt động | Bạn bè đã chọn | Thứ tự hiện tại sau khi hoạt động | 
|---:|---|---| 
| 1 |`4 5`|`5 4 3 2 1`được chuyển đổi theo bit 0 | 
| 2 |`5 1 2`| chuyển đổi theo bit 1 | 
| 3 |`3 4 5 1`|`3 4 5 1 2`| 

Việc xây dựng không cần đến các thứ tự trung gian chính xác vì đối số mã chứng minh thứ tự cuối cùng. Điểm quan trọng là thứ tự mã cuối cùng là`100, 101, 111, 110, 010`, 

ánh xạ tới`3, 4, 5, 1, 2`. 

Đầu ra mẫu sử dụng bốn thao tác, nhưng không cần giảm thiểu số lượng. Việc xây dựng hợp lệ khác bằng cách sử dụng ba thao tác là hoàn toàn có thể chấp nhận được. 

### Mẫu 2 

Đầu vào là```text
7
3 4 7 6 2 5 1
2 6 3 4 5 7 1
```Lại`k = 3`. Bảy mã Gray đảo ngược đầu tiên là`100, 101, 111, 110, 010, 011, 001`. 

| Vị trí mục tiêu | Bạn | Mã | 
|---:|---:|---:| 
| 0 | 2 |`100`| 
| 1 | 6 |`101`| 
| 2 | 3 |`111`| 
| 3 | 4 |`110`| 
| 4 | 5 |`010`| 
| 5 | 7 |`011`| 
| 6 | 1 |`001`| 

Các bộ đã chọn có được bằng cách xem xét từng cột bit. 

| Hoạt động | Bạn bè đã chọn | 
|---:|---| 
| 1 |`6 3 7 1`| 
| 2 |`3 4 5 7`| 
| 3 |`2 6 3 4`| 

Bắt đầu từ`3 4 7 6 2 5 1`, 

hoạt động đầu tiên mang lại`1 6 7 3 4 2 5`. 

Hoạt động thứ hai mang lại`5 4 3 7 1 6 2`. 

Hoạt động thứ ba mang lại`2 6 3 4 5 7 1`. 

Kết quả chính xác là hoán vị được yêu cầu. Mẫu sử dụng một chuỗi ba thao tác khác, cũng hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian |`O(n log n)`| có`k = O(log n)`hoạt động và mọi hoạt động sẽ quét`n`vị trí mục tiêu | 
| Không gian |`O(n)`| Mảng mã và các thao tác đầu ra chứa tối đa`O(n log n)`số nguyên ở dạng biểu diễn tệ nhất, trong khi việc triển khai có thể lưu trữ trực tiếp các tập hợp đã chọn | 

Vì`n <= 10000`,`k <= 14`, do đó công trình thực hiện tối đa khoảng 140000 lần kiểm tra vị trí cơ bản. Điều này dễ dàng nằm trong giới hạn 2 giây nhất định và số lượng thao tác hoàn toàn dưới mức tối đa được yêu cầu là 15. 

## Trường hợp thử nghiệm 

Đầu ra của một vấn đề mang tính xây dựng không phải là duy nhất, vì vậy việc kiểm tra đầu ra theo một chuỗi cố định sẽ không chính xác. Khai thác thử nghiệm sau đây phân tích cú pháp các hoạt động được tạo ra, mô phỏng chúng và xác minh rằng hoán vị cuối cùng bằng`b`. Nó cũng kiểm tra xem mọi thao tác có trống không, mỗi người bạn được chọn tối đa một lần cho mỗi thao tác và không quá 15 thao tác được in.```python
import sys
import io

def build_operations(a, b):
    n = len(a)

    if a == b:
        return []

    k = 0
    while (1 << k) < n:
        k += 1

    full = (1 << k) - 1
    codes = [0] * n

    for r in range(n):
        x = full - r
        codes[r] = x ^ (x >> 1)

    operations = []

    for bit in range(k):
        mask = 1 << bit
        selected = []

        for r in range(n):
            if codes[r] & mask:
                selected.append(b[r])

        if selected:
            operations.append(selected)

    return operations

def solve_string(inp):
    data = io.StringIO(inp)

    n = int(data.readline())
    a = list(map(int, data.readline().split()))
    b = list(map(int, data.readline().split()))

    operations = build_operations(a, b)

    out = [str(len(operations))]
    for op in operations:
        out.append("{} {}".format(len(op), " ".join(map(str, op))))

    return "\n".join(out) + "\n"

def run(inp: str) -> str:
    return solve_string(inp)

def validate(inp: str):
    lines = inp.strip().splitlines()
    n = int(lines[0])
    a = list(map(int, lines[1].split()))
    b = list(map(int, lines[2].split()))

    output = run(inp)
    tokens = output.split()
    ptr = 0

    k = int(tokens[ptr])
    ptr += 1

    assert 0 <= k <= 15

    current = a[:]

    for _ in range(k):
        c = int(tokens[ptr])
        ptr += 1

        assert 1 <= c <= n

        chosen = list(map(int, tokens[ptr:ptr + c]))
        ptr += c

        assert len(chosen) == c
        assert len(set(chosen)) == c
        assert all(1 <= x <= n for x in chosen)

        chosen_set = set(chosen)

        selected = []
        remaining = []

        for x in current:
            if x in chosen_set:
                selected.append(x)
            else:
                remaining.append(x)

        current = selected[::-1] + remaining

    assert ptr == len(tokens)
    assert current == b

sample1 = """\
5
5 4 3 2 1
3 4 5 1 2
"""

sample2 = """\
7
3 4 7 6 2 5 1
2 6 3 4 5 7 1
"""

validate(sample1)
validate(sample2)

custom1 = """\
1
1
1
"""
validate(custom1)

custom2 = """\
2
2 1
1 2
"""
validate(custom2)

custom3 = """\
8
8 7 6 5 4 3 2 1
1 2 3 4 5 6 7 8
"""
validate(custom3)

n = 10000
custom4 = "{}\n{}\n{}\n".format(
    n,
    " ".join(map(str, range(n, 0, -1))),
    " ".join(map(str, range(1, n + 1)))
)
validate(custom4)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`1 / 1 / 1`| Bất kỳ đầu ra hợp lệ nào với`0`hoạt động | Kích thước tối thiểu và xử lý không hoạt động | 
|`2 / 2 1 / 1 2`| Bất kỳ đầu ra hợp lệ nào có nhiều nhất`15`hoạt động | Không gian mã nhị phân không cần thiết nhỏ nhất | 
|`8 / 8 7 6 5 4 3 2 1 / 1 2 3 4 5 6 7 8`| Bất kỳ đầu ra hợp lệ nào | Ranh giới lũy thừa hai chính xác, trong đó mọi mã ba bit đều có sẵn | 
|`10000 / 10000 ... 1 / 1 ... 10000`| Bất kỳ đầu ra hợp lệ nào có nhiều nhất`15`hoạt động | Tối đa`n`, hiệu suất và`k = 14`ranh giới | 

Trường hợp "tất cả các giá trị bằng nhau" được yêu cầu không thể là một thử nghiệm hợp lệ vì vấn đề yêu cầu rõ ràng cả hai mảng phải là hoán vị, do đó mọi giá trị xảy ra chính xác một lần. Thay vào đó, trình xác thực sẽ kiểm tra tính duy nhất bên trong mỗi thao tác, nhằm phát hiện các lỗi triển khai mà các giá trị đầu vào lặp lại sẽ lộ ra. 

## Vỏ cạnh 

cho`n = 1`, hoán vị duy nhất có thể là`[1]`. Từ`a`Và`b`cả hai đều phải chứa giá trị duy nhất, chúng bằng nhau và`build_operations`ngay lập tức trả về một danh sách trống. Đầu ra là`0`, điều này hợp lệ vì không cần thực hiện thao tác nào. 

Vì`n = 2`, một chút là đủ. Trình tự mã Gray ngược là`[1, 0]`. Nếu mục tiêu là`[1, 2]`, bạn ơi`1`nhận được mã`1`và người bạn`2`nhận được mã`0`. Thao tác duy nhất chọn bạn bè`1`, tạo ra thứ tự mục tiêu bất kể hoán vị ban đầu. 

Vì`n = 4`, chuỗi mã Gray đảo ngược hai bit là`[2, 3, 1, 0]`, tương ứng với chuỗi nhị phân`10, 11, 01, 00`. Cả bốn mã đều khác biệt nên việc xây dựng công trình chính xác ở giới hạn công suất. Không có mã không sử dụng phải lo lắng. 

Vì`n = 5`, cần có ba bit. Các mã được chỉ định là`111, 110, 101, 100, 000`theo thứ tự Gray đảo ngược thích hợp sau khi áp dụng công thức. Ba mã còn lại không được sử dụng. Chúng không ảnh hưởng đến bất kỳ hoạt động nào vì không có người bạn nào sở hữu chúng. 

Vì`n = 10000`,`k = 14`bởi vì`2^13 = 8192`là không đủ trong khi`2^14 = 16384`là đủ. Mỗi người bạn nhận được một mã 14 bit duy nhất, do đó việc xây dựng chỉ cần 14 thao tác, để lại một thao tác ký quỹ dưới giới hạn 15. 

Lỗi thường gặp nhất là sử dụng`gray(r)`thay vì`gray(2^k - 1 - r)`. Cái trước tạo ra thứ tự mã Gray thông thường, trong khi các phép toán tạo ra thứ tự ngược lại. Việc trừ đi một từ`2^k`cũng là điều cần thiết. sử dụng`2^k - r`sẽ tạo ra một giá trị nằm ngoài dự định`k`-phạm vi bit cho vị trí đầu tiên. 

Một lỗi phổ biến khác là áp dụng các thao tác từ bit cao nhất đến bit thấp nhất. Việc xây dựng dựa trên các hoạt động được thực hiện theo thứ tự bit tăng dần. Hoạt động cuối cùng đảo ngược các nhóm đã chọn được tạo bởi tất cả các hoạt động trước đó và sự đảo ngược đệ quy đó chính xác là những gì tạo ra cấu trúc mã Gray được phản ánh. 

Cuối cùng, hoán vị ban đầu`a`không xuất hiện trong quá trình xây dựng mã sau khi các thao tác đã được chọn. Đây là cố ý. Tất cả các mã được chỉ định đều khác biệt và trình tự thao tác buộc mỗi cặp bạn bè phải tuân theo thứ tự tương đối được xác định bởi mã của họ. Thứ tự tương đối ban đầu có thể ảnh hưởng đến các hoán vị trung gian, nhưng xét cho cùng thì nó không thể ảnh hưởng đến thứ tự cuối cùng`k`hoạt động. 
:::
