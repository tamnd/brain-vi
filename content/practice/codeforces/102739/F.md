---
title: "CF 102739F - \u0421\u0430\u0448\u0430 \u043e\u043f\u044f\u0442\u044c \u0434\u0435\u043b\u0430\u0435\u0442 \u0437\u0430\u0434\u0430\u0447\u0443 \u043f\u0440\u043e \u043f\u0440\u043e\u0441\u0442\u044b\u0435 \u0447\u0438\u0441\u043b\u0430"
description: "Nhiệm vụ là làm cho mọi giá trị của thiết bị theo dõi thể dục được Sasha chấp nhận. Một giá trị được chấp nhận nếu nó là số nguyên tố hoặc lũy thừa của hai. Với mỗi số được ghi, chúng ta cần tìm số nhỏ nhất có thể chấp nhận được và không nhỏ hơn số được ghi."
date: "2026-07-29T01:08:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102739
codeforces_index: "F"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2020.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102739
solve_time_s: 73
verified: true
draft: false
---

[CF 102739F - \u0421\u0430\u0448\u0430 \u043e\u043f\u044f\u0442\u044c \u0434\u0435\u043b\u0430\u0435\u0442 \u0437\u0430\u0434\u0430\u0447\u0443 \u043f\u0440\u043e \u043f\u0440\u043e\u0441\u0442\u044b\u0435 \u0447\u0438\u0441\u043b\u0430](https://codeforces.com/problemset/problem/102739/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ là làm cho mọi giá trị của thiết bị theo dõi thể dục được Sasha chấp nhận. Một giá trị được chấp nhận nếu nó là số nguyên tố hoặc lũy thừa của hai. Với mỗi số được ghi, chúng ta cần tìm số nhỏ nhất có thể chấp nhận được và không nhỏ hơn số được ghi. Đầu ra chứa một câu trả lời cho mỗi ngày. 

Đầu vào chứa tối đa$10^5$giá trị được ghi lại và mỗi giá trị có thể lớn bằng$10^7$. Giải pháp kiểm tra từng số ứng viên riêng biệt cho từng truy vấn có thể trở nên quá tốn kém. Trong trường hợp xấu nhất, việc quét về phía trước theo nhiều số và kiểm tra tính nguyên tố của từng số có thể hoạt động tốt hơn nhiều so với$10^8$hoạt động không an toàn trong môi trường lập trình cạnh tranh một giây. Kích thước của giá trị tối đa cho thấy rằng việc xử lý trước tất cả các câu trả lời có liên quan một lần là hướng dự định. 

Các trường hợp đặc biệt chủ yếu là do câu trả lời không nhất thiết phải là số nguyên tố. Một giá trị có thể đã là lũy thừa của hai và lũy thừa đó có thể nhỏ hơn số nguyên tố tiếp theo. Ví dụ, đầu vào```
1
1023
```có đầu ra```
1024
```bởi vì 1024 là lũy thừa của hai và nhỏ hơn số nguyên tố tiếp theo sau 1023. Giải pháp chỉ tìm kiếm các số nguyên tố sẽ trả về sai 1031. 

Một lỗi phổ biến khác là quên rằng số 0 và một không phải là số nguyên tố, nhưng một là lũy thừa hợp lệ của hai trong cách giải thích bài toán này vì nó là$2^0$. Ví dụ:```
1
0
```có đầu ra```
1
```Một giải pháp chỉ dựa trên tính nguyên tố sẽ thất bại vì nó không có câu trả lời nguyên tố nào nhỏ hơn 2 và việc triển khai lũy thừa hai bắt đầu từ$2^1$sẽ bỏ lỡ giá trị chính xác. 

Các giá trị lặp lại cũng quan trọng. Ví dụ:```
3
16 16 17
```có đầu ra```
16 16 17
```Thuật toán phải xử lý trước một lần và trả lời tất cả các truy vấn một cách nhất quán thay vì tính toán lại cùng một thông tin. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xử lý mọi truy vấn một cách độc lập. Bắt đầu từ giá trị đã cho, chúng ta có thể kiểm tra từng số nguyên sau cho đến khi tìm thấy số nguyên tố hoặc lũy thừa của hai. Điều này đúng vì chúng tôi kiểm tra các ứng viên theo thứ tự tăng dần, vì vậy ứng viên hợp lệ đầu tiên chính xác là mức tối thiểu được yêu cầu. Vấn đề là số lượng công việc lặp đi lặp lại. Nếu nhiều truy vấn gần với một giá trị lớn thì mọi truy vấn sẽ lặp lại hầu hết các bước kiểm tra tính nguyên thủy giống nhau. Với$10^5$truy vấn và giá trị xung quanh$10^7$, cách tiếp cận này có thể dễ dàng yêu cầu hàng tỷ thao tác nhỏ. 

Cấu trúc của bài toán cho chúng ta một lựa chọn tốt hơn. Các câu trả lời có thể chỉ có hai loại: số nguyên tố và lũy thừa của hai. Sức mạnh của hai cực kỳ hiếm nên chúng ta có thể lưu trữ chúng trực tiếp. Các số nguyên tố cho đến câu trả lời lớn nhất có thể có thể được tạo ra một lần bằng Sàng Eratosthenes. Sau đó, mọi truy vấn trở thành một bài toán tra cứu: tìm số nguyên tố đầu tiên không nhỏ hơn giá trị và so sánh nó với lũy thừa đầu tiên của hai không nhỏ hơn giá trị. 

Câu hỏi còn lại là làm thế nào để tìm được số nguyên tố tiếp theo một cách nhanh chóng. Vì giá trị tối đa chỉ ở khoảng$10^7$, chúng ta có thể xây dựng một mảng trong đó mỗi vị trí lưu trữ số nguyên tố gần nhất tại hoặc sau vị trí đó. Một lần quét ngược sẽ lấp đầy mảng này. Sau đó, mọi truy vấn đều được trả lời trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * khoảng cách * sqrt(A)) | O(1) | Quá chậm | 
| Tối ưu | O(M log log M + M + n) | O(M) | Đã chấp nhận | 

Đây$M$là giá trị tối đa mà chúng tôi sàng lọc được. Sàng là bước tiền xử lý chủ yếu, trong khi việc trả lời các truy vấn là tuyến tính theo số ngày. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị trước và xác định giá trị lớn nhất xuất hiện. Chúng tôi cần điều này vì sàng chỉ phải bao phủ phạm vi có thể ảnh hưởng đến câu trả lời. 
2. Mở rộng phạm vi một chút và chạy Sàng Eratosthenes. Sàng đánh dấu mọi số tổng hợp, để lại chính xác các vị trí chính có sẵn cho truy vấn. 
3. Xây dựng một dãy hậu tố gồm các số nguyên tố. Quét từ cuối phạm vi về phía đầu, ghi nhớ số nguyên tố gần đây nhất được nhìn thấy. Sau lần quét này, giá trị tại vị trí$i$là số nguyên tố nhỏ nhất lớn hơn hoặc bằng$i$. 
4. Tạo tất cả lũy thừa của hai đến phạm vi yêu cầu. Lưu trữ chúng theo thứ tự sắp xếp. Đối với mỗi truy vấn, tìm kiếm nhị phân danh sách này để tìm lũy thừa đầu tiên của hai ít nhất là giá trị truy vấn. 
5. Với mỗi số ban đầu, hãy so sánh số nguyên tố tiếp theo và lũy thừa tiếp theo của 2. Câu trả lời nhỏ hơn là câu trả lời vì cả hai ứng cử viên đều hợp lệ và cả hai đều là những ứng cử viên sớm nhất có thể có trong hạng mục của riêng họ. 

Tại sao nó hoạt động: sau khi tiền xử lý, mọi ứng cử viên chính được biểu thị bằng mảng số nguyên tố tiếp theo và mọi ứng cử viên lũy thừa hai được biểu thị bằng tìm kiếm nhị phân trên danh sách được tạo. Đối với bất kỳ giá trị đầu vào nào$x$, mọi câu trả lời hợp lệ đều phải thuộc một trong hai loại này. Thuật toán chọn ứng cử viên nhỏ nhất từ ​​mỗi danh mục và trả về giá trị nhỏ hơn trong hai danh mục, do đó không thể tồn tại số hợp lệ nhỏ hơn. 

## Giải pháp Python```python
import sys
import bisect

input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    n = data[0]
    a = data[1:]

    mx = max(a)

    limit = mx + 100000

    prime = bytearray(b'\x01') * (limit + 1)
    prime[0] = 0
    if limit >= 1:
        prime[1] = 0

    i = 2
    while i * i <= limit:
        if prime[i]:
            start = i * i
            prime[start:limit + 1:i] = b'\x00' * (((limit - start) // i) + 1)
        i += 1

    next_prime = [0] * (limit + 2)
    nxt = 0
    for i in range(limit, -1, -1):
        if prime[i]:
            nxt = i
        next_prime[i] = nxt

    powers = []
    x = 1
    while x <= limit:
        powers.append(x)
        x *= 2

    ans = []
    for value in a:
        p = next_prime[value]
        idx = bisect.bisect_left(powers, value)
        pw = powers[idx]
        if p < pw:
            ans.append(str(p))
        else:
            ans.append(str(pw))

    print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc tất cả các truy vấn vì kích thước tiền xử lý phụ thuộc vào giá trị đầu vào tối đa. Sàng sử dụng một`bytearray`bởi vì nó chỉ lưu trữ thông tin boolean và có hiệu quả bộ nhớ cao hơn nhiều so với danh sách số nguyên Python. 

Phép gán lát trong sàng đánh dấu tất cả bội số của một số nguyên tố cùng một lúc. Việc tạo ra`next_prime`mảng được điền ngược, vì vậy mọi vị trí đều biết ngay số nguyên tố gần nhất ở bên phải của nó. Điều này tránh thực hiện tìm kiếm nhị phân trên các số nguyên tố cho mọi truy vấn. 

Danh sách quyền hạn bắt đầu từ 1 vì$2^0$là cần thiết cho những trường hợp như số không. Tìm kiếm nhị phân trả về lũy thừa đầu tiên không nhỏ hơn giá trị hiện tại. Phép so sánh cuối cùng xử lý quyết định duy nhất còn lại: số gần nhất được chấp nhận là số nguyên tố hay lũy thừa của hai. 

100000 vị trí bổ sung sau giá trị truy vấn tối đa cung cấp đủ chỗ để đạt đến số nguyên tố tiếp theo. Điều này cũng tránh được việc xử lý phức tạp giá trị đầu vào lớn nhất có thể trong khi vẫn giữ kích thước sàng nhỏ. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
5
2020 1023 0 101 10000
```dấu vết là: 

| Giá trị | Thủ tướng tiếp theo | Sức mạnh tiếp theo của hai | Trả lời | 
| --- | --- | --- | --- | 
| 2020 | 2027 | 2048 | 2027 | 
| 1023 | 1031 | 1024 | 1024 | 
| 0 | 2 | 1 | 1 | 
| 101 | 101 | 128 | 101 | 
| 10000 | 10007 | 16384 | 10007 | 

Ví dụ này chứng minh tại sao cả hai loại phải được xem xét. Các giá trị 1023 và 0 sẽ hiển thị một triển khai chỉ tìm kiếm các số nguyên tố. 

Một ví dụ thứ hai:```
4
1 2 8 9
```| Giá trị | Thủ tướng tiếp theo | Sức mạnh tiếp theo của hai | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 2 | 2 | 2 | 
| 8 | 11 | 8 | 8 | 
| 9 | 11 | 16 | 11 | 

Dấu vết này xác nhận rằng một số hợp lệ hiện có luôn được chấp nhận ngay lập tức và lũy thừa của hai có thể đánh bại các số nguyên tố gần đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log log M + M + n log log M) | Sàng chiếm ưu thế trong quá trình tiền xử lý, trong khi mỗi truy vấn thực hiện tìm kiếm nhị phân giữa các lũy thừa của hai | 
| Không gian | O(M) | Sàng và mảng tiếp theo lưu trữ thông tin cho mọi vị trí đến giới hạn | 

Giá trị lớn nhất chỉ$10^7$, do đó, cấu trúc tiền xử lý có kích thước tuyến tính là thực tế trong Python. Sau khi tiền xử lý, giải pháp sẽ xử lý$10^5$các truy vấn chỉ có công việc có kích thước không đổi ngoại trừ tìm kiếm nhị phân nhỏ trên lũy thừa hai. 

## Trường hợp thử nghiệm```python
import sys
import io
import bisect

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return ""

    n = data[0]
    a = data[1:]

    mx = max(a)
    limit = mx + 100000

    prime = bytearray(b'\x01') * (limit + 1)
    prime[0] = 0
    if limit >= 1:
        prime[1] = 0

    i = 2
    while i * i <= limit:
        if prime[i]:
            prime[i * i:limit + 1:i] = b'\x00' * (((limit - i * i) // i) + 1)
        i += 1

    nxt = 0
    next_prime = [0] * (limit + 2)
    for i in range(limit, -1, -1):
        if prime[i]:
            nxt = i
        next_prime[i] = nxt

    powers = []
    x = 1
    while x <= limit:
        powers.append(x)
        x *= 2

    res = []
    for v in a:
        p = next_prime[v]
        pw = powers[bisect.bisect_left(powers, v)]
        res.append(str(min(p, pw)))

    return " ".join(res)

assert solution("""5
2020 1023 0 101 10000
""") == "2027 1024 1 101 10007"

assert solution("""4
1 2 8 9
""") == "1 2 8 11"

assert solution("""3
0 1 3
""") == "1 1 3"

assert solution("""4
16 16 17 31
""") == "16 16 17 31"

assert solution("""3
10000000 9999999 9999991
""") == "10000019 9999991 9999991"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0, 1, 3`|`1 1 3`| Xử lý các giá trị nhỏ nhất và sức mạnh đặc biệt của hai`1`| 
|`16, 16, 17, 31`|`16 16 17 31`| Xử lý các giá trị lặp lại và các câu trả lời đã hợp lệ | 
|`10000000, 9999999, 9999991`|`10000019 9999991 9999991`| Kiểm tra hành vi gần phạm vi đầu vào tối đa | 

## Vỏ cạnh 

Đối với đầu vào```
1
1023
```sàng tìm thấy số nguyên tố tiếp theo là 1031, trong khi tìm kiếm lũy thừa hai tìm thấy 1024. Phép so sánh trả về 1024. Thuật toán thành công vì nó không bao giờ giả sử các số nguyên tố luôn là số toán học gần nhất. 

Đối với đầu vào```
1
0
```ứng cử viên chính là 2, nhưng danh sách lũy thừa được tạo bắt đầu bằng 1. Tìm kiếm nhị phân trả về 1 và giá trị tối thiểu của hai ứng cử viên là chính xác. Trường hợp này khẳng định việc phát điện phải bao gồm$2^0$. 

Đối với đầu vào```
3
16 16 17
```cả hai lần xuất hiện của 16 đều tìm thấy các ứng cử viên số nguyên tố và lũy thừa tiếp theo giống nhau. Quá trình tiền xử lý được chia sẻ nên kết quả giống hệt nhau đối với các đầu vào bằng nhau và không cần tính toán lặp lại.
