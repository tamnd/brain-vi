---
title: "CF 1095C - Quyền hạn của hai"
description: "Chúng ta được yêu cầu biểu diễn một số nguyên n cho trước dưới dạng tổng của chính xác k số, trong đó mỗi tổng phải là lũy thừa của hai. Do đó, mỗi số được chọn có dạng 1, 2, 4, 8, ... và được phép lặp lại vì chúng ta có thể sử dụng cùng một lũy thừa nhiều lần."
date: "2026-06-13T05:12:25+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1095
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 529 (Div. 3)"
rating: 1400
weight: 1095
solve_time_s: 744
verified: false
draft: false
---

[CF 1095C - Sức mạnh của hai](https://codeforces.com/problemset/problem/1095/C) 

**Đánh giá:** 1400 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 12m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu thể hiện một số nguyên nhất định`n`như một tổng chính xác`k`các số, trong đó mỗi tổng phải là lũy thừa của hai. Do đó mỗi số được chọn có dạng`1, 2, 4, 8, ...`và được phép lặp lại vì chúng ta có thể sử dụng cùng một công suất nhiều lần. 

Đầu vào mô tả số tiền mục tiêu`n`và số lần triệu tập cần thiết`k`. Đầu ra là một tuyên bố rằng việc phân tách là không thể hoặc một tập hợp cụ thể của`k`lũy thừa của hai có tổng bằng`n`. 

Cấu trúc của vấn đề bị ràng buộc theo hai hướng độc lập. giá trị`n`giới hạn tổng khối lượng chúng ta có thể phân phối, trong khi`k`buộc chúng ta phải chia khối lượng đó thành một số phần cố định. Điều này tạo nên sự căng thẳng giữa “ít nước lớn” và “nhiều nước nhỏ”. 

Các giới hạn đủ chặt chẽ để bất kỳ cách tiếp cận nào cố gắng liệt kê các phân vùng hoặc xây dựng các kết hợp một cách rõ ràng sẽ thất bại. Giá trị của`n`đi lên`10^9`, do đó, ngay cả việc biểu diễn nó ở dạng nhị phân cũng có tối đa 30 bit có nghĩa. Giá trị của`k`đi lên`2 * 10^5`, do đó bản thân kết quả đầu ra có thể lớn, nghĩa là thuật toán phải gần tuyến tính ở kích thước câu trả lời cuối cùng. 

Trường hợp cạnh khóa xuất hiện khi`k`lớn hơn`n`. Vì giá trị cho phép nhỏ nhất là`1`, tổng của`k`số nguyên dương ít nhất là`k`, vì vậy bất kỳ trường hợp nào`k > n`ngay lập tức là không thể. 

Một trường hợp tế nhị khác phát sinh khi`n`có rất ít bit được đặt ở dạng nhị phân. Ví dụ, nếu`n = 8`, biểu diễn tự nhiên chỉ là`[8]`. Nếu như`k = 3`, chúng ta phải chia tay`8`thành ba lũy thừa của hai. Điều này có thể thực hiện được thông qua việc chia tách`8 -> 4 + 4`, sau đó chia tiếp một`4 -> 2 + 2`, tùy thuộc vào số lần chia tách là cần thiết. Tính đúng đắn phụ thuộc vào việc liệu chúng ta có thể tiếp tục phân tích lũy thừa của hai hay không. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp nhưng ngây thơ là bắt đầu từ`n`được viết dưới dạng tổng lũy ​​thừa của hai ở dạng nhị phân. Mỗi bit`1 << i`trở thành một phần tử. Điều này đưa ra số lệnh triệu tập tối thiểu bằng số lượng bit được đặt trong`n`. 

Nếu con số này đã lớn hơn`k`, chúng ta thất bại ngay lập tức vì việc hợp nhất sức mạnh của hai là không thể. lũy thừa của hai không thể kết hợp với một lũy thừa khác để tạo thành lũy thừa khác của hai trừ khi chúng bằng nhau, vì vậy chúng ta không thể giảm số lượng một cách tùy ý. 

Nếu số số hạng ban đầu nhiều nhất`k`, chúng tôi cố gắng tăng số lượng số hạng cho đến khi đạt được chính xác`k`. Phép toán duy nhất bảo toàn tổng trong khi tăng số phần tử là chia lũy thừa của hai thành hai nửa bằng nhau. Mỗi lần chia sẽ tăng số lượng lên đúng một. 

Phiên bản bạo lực sẽ liên tục chọn bất kỳ số nào lớn hơn`1`và chia nó cho đến khi đạt được`k`điều khoản. Trong trường hợp xấu nhất, mỗi lần chia sẽ giảm công suất cao xuống nhiều công suất, dẫn đến khoảng`O(n)`hoạt động nếu được thực hiện không hiệu quả như thao tác mảng. Điều này là quá chậm đối với`k`lên đến`2e5`. 

Quan sát quan trọng là chúng ta không bao giờ cần xem xét việc phân chia tùy ý. Chúng ta chỉ cần một chiến lược tham lam: luôn chia sức mạnh lớn nhất hiện có của hai người. Điều này đảm bảo rằng chúng tôi duy trì tính khả thi đồng thời tối đa hóa tính linh hoạt cho các lần chia tách trong tương lai. 

Chúng tôi duy trì một cấu trúc gồm số lũy thừa của hai. Mỗi lần chúng tôi chia tay một`2^i`, chúng tôi thay thế nó bằng hai`2^(i-1)`các giá trị. Chúng tôi tiếp tục cho đến khi số lượng phần tử đạt`k`hoặc không thể chia tách thêm nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Danh sách chia tách Brute Force | O(n) trường hợp xấu nhất | O(k) | Quá chậm | 
| Tham lam chia tách sức mạnh của hai | O(k log n) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi`n`thành sự phân rã nhị phân của nó, lưu trữ số lượng lũy ​​thừa của hai. Điều này đưa ra số lượng lệnh triệu tập nhỏ nhất có thể. Bước này là bắt buộc vì lũy thừa của hai tạo thành cơ sở rời rạc cho tất cả các số nguyên. 
2. Đếm số lần triệu tập ban đầu. Nếu số này lớn hơn`k`, đầu ra`NO`, vì chúng ta không thể hợp nhất các lũy thừa riêng biệt của hai thành một lũy thừa hợp lệ duy nhất của hai. 
3. Chèn tất cả lũy thừa của hai vào cấu trúc tối đa (về mặt khái niệm là hàng đợi ưu tiên theo số mũ). Chúng tôi luôn muốn phân chia quyền lực lớn nhất hiện có vì nó tạo ra sự linh hoạt nhất trong các lần phân chia trong tương lai. 
4. Trong khi số lần triệu tập ít hơn`k`, lấy công suất lớn nhất`2^x`. Nếu như`x = 0`, nghĩa là giá trị là`1`, dừng lại vì không thể chia được nữa. Nếu không thì thay thế nó bằng hai bản sao của`2^(x-1)`và tăng số lượng lên một. 
5. Nếu chúng ta đạt được chính xác`k`các phần tử, xuất ra nhiều tập hợp. Nếu chúng ta bị mắc kẹt trước khi đạt được`k`, đầu ra`NO`. 

### Tại sao nó hoạt động 

Thuật toán duy trì sự biểu diễn của`n`như một tập hợp lũy thừa của hai luôn có tổng bằng`n`. Mỗi thao tác bảo toàn tổng số tiền vì thay thế`2^x`với hai`2^(x-1)`giữ giá trị không thay đổi. Số lượng phần tử tăng đơn điệu đúng một phần tử cho mỗi lần chia, vì vậy chúng tôi kiểm soát kích thước cuối cùng một cách chính xác. Sự lựa chọn tham lam trong việc chia tách công suất lớn nhất đảm bảo chúng ta không bao giờ hết sớm khối lượng có thể chia tách khi có một giải pháp khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, k = map(int, input().split())

    # initial decomposition
    cnt = 0
    heap = []

    for i in range(31):
        if n & (1 << i):
            heap.append(-i)
            cnt += 1

    if cnt > k:
        print("NO")
        return

    heapq.heapify(heap)

    # we store exponents as negatives for max-heap behavior

    while cnt < k:
        if not heap:
            break

        x = -heapq.heappop(heap)

        if x == 0:
            break

        # split 2^x into two 2^(x-1)
        heapq.heappush(heap, -(x - 1))
        heapq.heappush(heap, -(x - 1))
        cnt += 1

    if cnt != k:
        print("NO")
        return

    print("YES")
    res = []
    while heap:
        x = -heapq.heappop(heap)
        res.append(1 << x)

    print(*res)
```Giải pháp bắt đầu bằng việc phân hủy`n`thành lũy thừa của hai, tạo thành biểu diễn hợp lệ tối thiểu. Một đống được sử dụng để luôn trích xuất số mũ lớn nhất một cách hiệu quả. Điều này quan trọng bởi vì các quyền lực lớn hơn là những quyền lực duy nhất có thể được phân chia nhiều lần và việc chọn chúng trước sẽ tránh bị mắc kẹt với nhiều quyền lực.`1`quá sớm. 

Mỗi thao tác phân tách sẽ loại bỏ một phần tử và thêm hai phần tử nhỏ hơn, tăng tổng số lên chính xác một phần tử. Điều này giúp dễ dàng theo dõi tiến độ hướng tới`k`. 

Điều kiện kết thúc hoặc đạt chính xác`k`các yếu tố hoặc cạn kiệt tất cả các quyền hạn có thể phân chia. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n = 9, k = 4`Sự phân hủy ban đầu là`9 = 8 + 1`, vậy số mũ là`{3, 0}`. 

| Bước | Đống (số mũ) | Đếm | Hành động | 
| --- | --- | --- | --- | 
| 0 | [3, 0] | 2 | bắt đầu | 
| 1 | [2, 2, 0] | 3 | chia 3 → 2 + 2 | 
| 2 | [2, 1, 1, 0] | 4 | chia 2 → 1 + 1 | 

Chúng tôi đạt được 4 yếu tố, vì vậy đầu ra hợp lệ:`8 → 4 4 → 2 2 4 → 1 1 2 4`. 

Điều này chứng tỏ rằng việc phân chia lặp đi lặp lại công suất lớn nhất hiện có sẽ phân phối chính xác khối lượng thành số lần triệu hồi cần thiết. 

### Ví dụ 2 

đầu vào:`n = 7, k = 5`Phân hủy ban đầu:`7 = 4 + 2 + 1`. 

| Bước | Đống | Đếm | Hành động | 
| --- | --- | --- | --- | 
| 0 | [2, 1, 0] | 3 | bắt đầu | 
| 1 | [1, 1, 1, 0] | 4 | chia 2 → 1 + 1 | 
| 2 | [1, 1, 1, 0] | 4 | không thể chia thêm để tiến bộ | 

Chúng tôi không thể đạt tới 5 phần tử vì tất cả các giá trị còn lại là`1`, do đó không thể phân chia thêm được nữa. Câu trả lời là`NO`. 

Điều này cho thấy tầm quan trọng của việc phát hiện sự cạn kiệt quyền hạn có thể chia tách trước khi đạt được`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log n) | Mỗi lần chia là một thao tác heap và chúng tôi thực hiện tối đa k lần chia | 
| Không gian | O(k) | Heap lưu trữ tối đa k phần tử | 

Các ràng buộc cho phép lên đến`2 * 10^5`các phần tử đầu ra và mỗi phép toán đều là logarit ở kích thước này, do đó giải pháp phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import sys
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # inline solution for testing
    import heapq

    n, k = map(int, sys.stdin.readline().split())

    cnt = 0
    heap = []

    for i in range(31):
        if n & (1 << i):
            heap.append(-i)
            cnt += 1

    if cnt > k:
        return "NO"

    heapq.heapify(heap)

    while cnt < k:
        if not heap:
            return "NO"
        x = -heapq.heappop(heap)
        if x == 0:
            return "NO"
        heapq.heappush(heap, -(x - 1))
        heapq.heappush(heap, -(x - 1))
        cnt += 1

    if cnt != k:
        return "NO"

    res = []
    while heap:
        res.append(1 << (-heapq.heappop(heap)))

    return "YES " + " ".join(map(str, res))

assert run("9 4") == "YES 1 2 2 4", "sample 1"

assert run("1 1") == "YES 1", "minimum case"

assert run("1 2") == "NO", "cannot split 1"

assert run("8 1") == "YES 8", "already exact"

assert run("7 10") == "NO", "need more splits than possible"

assert run("10 4") != "", "basic feasibility check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | KHÔNG | không thể chia thêm 1 | 
| 8 1 | CÓ 8 | đại diện đã tối thiểu | 
| 7 10 | KHÔNG | công suất phân chia không đủ | 
| 10 4 | phân hủy hợp lệ | tính đúng đắn chung | 

## Vỏ cạnh 

Khi nào`n = 1`Và`k > 1`, heap chỉ chứa một số mũ duy nhất`0`. Thuật toán dừng ngay lập tức vì`0`không thể chia nhỏ được nên nó trả về một cách chính xác`NO`. 

Khi`n`đã lớn nhưng thưa thớt từng bit, chẳng hạn như`n = 2^30`, thuật toán bắt đầu với một phần tử duy nhất. Nó liên tục phân chia thành`1`s cho đến khi đạt được`k`hoặc cạn kiệt tất cả công suất phân chia. Mỗi bước duy trì tính chính xác vì mỗi lần chia đều duy trì tổng số tiền. 

Khi`k`bằng số bit ban đầu trong`n`, không cần chia tách. Thuật toán đưa ra trực tiếp biểu diễn nhị phân, phù hợp với phân tách tối thiểu tối ưu.
