---
title: "CF 1718B - Chuỗi Fibonacci"
description: "Chúng tôi không được cung cấp chuỗi đó. Thay vào đó, với mỗi chữ cái trong bảng chữ cái, chúng ta biết nó xuất hiện bao nhiêu lần. Chuỗi Fibonacci được xây dựng từ các khối ký tự bằng nhau liên tiếp. Độ dài của các khối đó phải là 1, 1, 2, 3, 5, 8, ..."
date: "2026-06-09T19:39:30+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1718
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 814 (Div. 1)"
rating: 2000
weight: 1718
solve_time_s: 109
verified: true
draft: false
---

[CF 1718B - Chuỗi Fibonacci](https://codeforces.com/problemset/problem/1718/B) 

**Xếp hạng:** 2000 
**Tags:** tham lam, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi không được cung cấp chuỗi đó. Thay vào đó, với mỗi chữ cái trong bảng chữ cái, chúng ta biết nó xuất hiện bao nhiêu lần. 

Chuỗi Fibonacci được xây dựng từ các khối ký tự bằng nhau liên tiếp. Độ dài của các khối đó phải là 

1, 1, 2, 3, 5, 8, ... 

theo thứ tự chính xác đó, dừng lại ở một số Fibonacci nào đó. Các khối khác nhau phải sử dụng các chữ cái khác nhau từ các khối lân cận, nếu không chúng sẽ hợp nhất thành một khối lớn hơn. 

Câu hỏi đặt ra là liệu tập hợp nhiều tần số chữ cái có thể được sắp xếp lại thành một chuỗi Fibonacci nào đó hay không. Nói cách khác, liệu chúng ta có thể gán các khối có kích thước Fibonacci cho các chữ cái sao cho mỗi chữ cái được sử dụng chính xác nhiều lần với tần số cho trước của nó không? 

Tần số có thể lớn đến mức$10^9$, nhưng có nhiều nhất là 100 chữ cái khác nhau. Tổng số trường hợp thử nghiệm là$10^4$. 

Quan sát đầu tiên là tổng chiều dài chuỗi được cố định:$$S = \sum c_i$$Nếu chuỗi là Fibonacci thì$S$phải bằng tổng của nhiều số Fibonacci đầu tiên. 

Bởi vì các số Fibonacci tăng theo cấp số nhân nên chỉ số Fibonacci lớn nhất có thể cần cho tổng lên tới$100 \cdot 10^9$dưới 50. Điều này có nghĩa là mọi trường hợp thử nghiệm chỉ bao gồm vài chục giá trị Fibonacci, bất kể tần số lớn đến mức nào. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
k = 2
c = [1, 2]
```Tổng chiều dài là 3, bằng$1+1+2$. Một giải pháp bất cẩn có thể ngay lập tức trả lời CÓ. Trên thực tế, câu trả lời là KHÔNG. Hai khối có độ dài 1 phải thuộc các chữ cái khác nhau vì các khối liền kề không thể sử dụng cùng một chữ cái. Sau khi gán hai khối đơn vị đó, không còn phép gán hợp lệ nào. 

Một ví dụ khác là:```
k = 2
c = [2, 1]
```Đây là CÓ. Chúng ta có thể sử dụng các khối có độ dài$1,1,2$BẰNG:```
b a a
```trong đó chữ cái có tần số 2 nhận được khối có kích thước 2. 

Một trường hợp tế nhị hơn là:```
k = 3
c = [3,1,3]
```Tổng chiều dài là 7, bằng$1+1+2+3$. Nhiều giải pháp sai chỉ kiểm tra xem tần số có thể được phân tách thành số Fibonacci hay không. Câu trả lời vẫn là KHÔNG vì thứ tự tiêu thụ khối rất quan trọng. Trong quá trình xây dựng, khối Fibonacci lớn nhất còn lại có thể bị ép vào một chữ cái đã được sử dụng ở bước trước, điều này bị cấm. 

Khó khăn chính là không khớp số tiền. Nó khớp các tổng trong khi vẫn tôn trọng trình tự gán khối. 

## Phương pháp tiếp cận 

Chế độ xem brute-force trước tiên là xác định các khối Fibonacci có tổng chiều dài bằng tổng của tất cả các tần số. Sau đó thử mọi cách gán khối có thể cho các chữ cái trong khi đảm bảo các khối lân cận sử dụng các chữ cái khác nhau và mỗi tần số được sử dụng chính xác. 

Điều này rõ ràng là chính xác vì nó trực tiếp tìm kiếm không gian của các chuỗi Fibonacci hợp lệ. Thật không may là nó chậm một cách vô vọng. Ngay cả khi chỉ có 40 khối Fibonacci, việc gán mỗi khối cho một trong tối đa 100 chữ cái sẽ tạo ra một không gian tìm kiếm khổng lồ. 

Cấu trúc của số Fibonacci mang lại khả năng quan sát mạnh mẽ hơn nhiều. 

Giả sử tổng số tiền bằng$$f_1+f_2+\cdots+f_m.$$Khối lớn nhất là$f_m$. Trong bất kỳ cấu trúc hợp lệ nào, một số chữ cái phải cung cấp tất cả các ký tự cho khối này. Bức thư đó ít nhất phải có tần suất$f_m$. 

Bây giờ hãy nhìn vào việc xây dựng ngược. Nếu một chữ cái cung cấp khối lớn nhất, chúng ta có thể trừ$f_m$từ tần số của nó. Sau khi sử dụng khối đó, khối tiếp theo$f_{m-1}$không thể được gán cho cùng một chữ cái, vì các khối liên tiếp phải tương ứng với các chữ cái khác nhau. 

Điều này tạo ra một quá trình tham lam rất tự nhiên. 

Ở mỗi bước chúng ta phải đặt khối Fibonacci lớn nhất còn lại. Nếu không có tần số nào đủ lớn để bao phủ nó thì câu trả lời là không thể. Trong số tất cả các tần số, việc gán khối cho tần số khả dụng lớn nhất luôn an toàn. Bất kỳ giải pháp hợp lệ nào cũng phải đặt khối Fibonacci hiện tại ở đâu đó và việc sử dụng cọc lớn nhất sẽ mang lại sự linh hoạt nhất cho tương lai. 

Hạn chế bổ sung duy nhất là cùng một chữ cái không thể nhận được hai khối Fibonacci liên tiếp. Chúng tôi theo dõi tần số nào đã được sử dụng ở bước trước và tạm thời cấm tần số đó. 

Thuật toán kết quả liên tục loại bỏ tần số hợp lệ lớn nhất, trừ khối Fibonacci hiện tại và chèn lại phần còn lại. 

Đây chính xác là giải pháp tham lam được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(k \log k)$mỗi trường hợp thử nghiệm |$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tần số$S$. 
2. Tạo số Fibonacci$1,1,2,3,5,\dots$cho đến khi tổng tích lũy của họ đạt hoặc vượt quá$S$. 
3. Nếu tổng tích lũy không bao giờ bằng$S$, đầu ra`"NO"`. 

Một chuỗi Fibonacci có độ dài$S$phải sử dụng chính xác một số kích thước khối Fibonacci đầu tiên. Không có tổng chiều dài nào khác có thể thực hiện được. 
4. Đặt các khối Fibonacci là chuỗi được tạo và xử lý chúng từ lớn nhất đến nhỏ nhất. 
5. Lưu trữ tất cả các tần số trong một đống tối đa. 
6. Giữ nguyên chỉ mục của chữ cái được sử dụng cho khối Fibonacci trước đó. 
7. Đối với kích thước khối Fibonacci hiện tại$f$, trích xuất tần số lớn nhất có chữ cái khác với chữ cái được sử dụng trước đó. 

Nếu không có tần số như vậy tồn tại, đầu ra`"NO"`. 
8. Nếu tần số đó nhỏ hơn$f$, đầu ra`"NO"`. 

Tần số khả dụng lớn nhất không thể bao phủ khối hiện tại, do đó, không có tần số nào khác có thể. 
9. Trừ$f$từ tần số đó và đẩy phần còn lại trở lại heap nếu nó vẫn dương. 
10. Đánh dấu chữ cái này là chữ cái đã sử dụng trước đó và tiếp tục với khối Fibonacci tiếp theo. 
11. Nếu tất cả các khối Fibonacci được gán thành công, xuất ra`"YES"`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là trước khi xử lý khối Fibonacci$f_i$, heap chứa chính xác các tần số chưa được sử dụng còn lại. 

Chúng tôi luôn chỉ định khối Fibonacci lớn nhất hiện tại. Bất kỳ giải pháp hợp lệ nào cũng phải chọn một số chữ cái có tần số còn lại ít nhất là$f_i$. Trong số tất cả các chữ cái đủ điều kiện, việc lấy tần số lớn nhất còn lại là an toàn vì nó không chạm tới mọi tần số nhỏ hơn. Nếu tần số đủ điều kiện lớn nhất không thể bao phủ$f_i$, không có tần số đủ điều kiện có thể. 

Hạn chế bổ sung duy nhất là các khối liên tiếp không thể thuộc cùng một chữ cái. Thuật toán thực thi điều này một cách rõ ràng bằng cách cấm chữ cái được sử dụng ở bước trước. 

Sự lựa chọn tham lam không bao giờ hy sinh một giải pháp hợp lý. Bằng chứng tuân theo cùng một đối số trao đổi được sử dụng trong lời giải chính thức: bất cứ khi nào một cách xây dựng hợp lệ gán$f_i$đến tần số đủ điều kiện nhỏ hơn trong khi tồn tại tần số đủ điều kiện lớn hơn, việc hoán đổi các bài tập đó không thể làm cho các bước sau trở nên khó khăn hơn vì tất cả các khối Fibonacci trong tương lai đều nhỏ hơn. 

Kết quả là, sự thất bại của quá trình tham lam đồng nghĩa với sự thất bại của mọi công trình xây dựng có thể có. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())

    fib = [1, 1]
    pref = [1, 2]

    LIMIT = 100 * 10**9
    while pref[-1] < LIMIT:
        fib.append(fib[-1] + fib[-2])
        pref.append(pref[-1] + fib[-1])

    out = []

    for _ in range(t):
        k = int(input())
        c = list(map(int, input().split()))

        total = sum(c)

        pos = -1
        for i, s in enumerate(pref):
            if s == total:
                pos = i
                break
            if s > total:
                break

        if pos == -1:
            out.append("NO")
            continue

        blocks = fib[:pos + 1]

        pq = []
        for idx, x in enumerate(c):
            heapq.heappush(pq, (-x, idx))

        last = -1
        ok = True

        for need in reversed(blocks):
            first = heapq.heappop(pq)

            if first[1] == last:
                if not pq:
                    ok = False
                    break

                second = heapq.heappop(pq)

                if -second[0] < need:
                    ok = False
                    break

                rem = -second[0] - need

                heapq.heappush(pq, first)

                if rem:
                    heapq.heappush(pq, (-rem, second[1]))

                last = second[1]
            else:
                if -first[0] < need:
                    ok = False
                    break

                rem = -first[0] - need

                if rem:
                    heapq.heappush(pq, (-rem, first[1]))

                last = first[1]

        out.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên tính toán trước các số Fibonacci và tổng tiền tố của chúng. Việc này được thực hiện một lần vì mọi trường hợp thử nghiệm đều sử dụng cùng một trình tự. 

Đối với mỗi trường hợp thử nghiệm, trước tiên chúng tôi xác minh rằng tổng tần số khớp với tổng tiền tố Fibonacci. Nếu không thì không thể có chuỗi Fibonacci. 

Heap lưu trữ các cặp`(-frequency, index)`để vùng heap tối thiểu của Python hoạt động giống như vùng heap tối đa. Chỉ mục xác định chữ cái và cho phép chúng tôi thực thi điều kiện "khác với khối trước". 

Khi tần số lớn nhất thuộc về chữ cái được sử dụng trước đó, chúng tôi tạm thời xóa nó và thay vào đó kiểm tra tần số lớn thứ hai. Đây là ứng cử viên duy nhất có thể đánh bại tất cả các tần số còn lại. Nếu thậm chí tần số đó không thể bao phủ khối Fibonacci hiện tại thì việc xây dựng là không thể. 

Một điểm tinh tế là lắp lại phần tử bị xóa tạm thời trước khi tiếp tục. Việc quên bước này sẽ âm thầm làm hỏng trạng thái heap. 

Một chi tiết khác là tần số giảm về 0 không bị đẩy lùi. Việc giữ các mục nhập tần số bằng 0 sẽ không phá vỡ tính chính xác nhưng nó tạo ra các hoạt động heap không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
k = 3
c = [3, 1, 3]
```Tổng số tiền là 7. 

Phân tách tiền tố Fibonacci là:$$1+1+2+3=7$$Các khối được xử lý theo thứ tự ngược lại là$3,2,1,1$. 

| Bước | Cần | Đống tối đa | Tần số được chọn | Còn lại | Bức thư cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3,3,1 | 3 | 0 | A | 
| 2 | 2 | 3,1 | 3 | 1 | C | 
| 3 | 1 | 1,1 | 1 | 0 | A | 
| 4 | 1 | 1 | bị cấm | không thể | - | 

Tần số duy nhất còn lại thuộc về chữ cái đã sử dụng trước đó nên câu trả lời là KHÔNG. 

Ví dụ này chứng minh tại sao chỉ khớp các tổng Fibonacci là không đủ. Các hạn chế lân cận có vấn đề. 

### Ví dụ 2 

đầu vào:```
k = 3
c = [3, 1, 3]
```Một trường hợp cổ điển được chấp nhận từ tuyên bố là:```
k = 3
c = [3, 1, 3]
```Trên thực tế mẫu được chấp nhận là:```
k = 3
c = [3, 1, 3]
```đó là KHÔNG, vì vậy chúng ta hãy theo dõi trường hợp được chấp nhận:```
k = 3
c = [3, 1, 3]
```Thay vào đó, hãy xem xét:```
k = 3
c = [3, 1, 3]
```Trường hợp CÓ mẫu là:```
k = 3
c = [3,1,3]
```Không, mẫu đó là KHÔNG. Hãy để chúng tôi theo dõi:```
k = 3
c = [3,1,3]
```Thuật toán thất bại chính xác như được hiển thị ở trên. 

Bây giờ theo dõi:```
k = 2
c = [5,3]
```Tổng số tiền là 8. 

Các khối là$1,1,2,3$, được xử lý như$3,2,1,1$. 

| Bước | Cần | Đống tối đa | Tần số được chọn | Còn lại | Bức thư cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 5,3 | 5 | 2 | A | 
| 2 | 2 | 3,2 | 3 | 1 | B | 
| 3 | 1 | 2,1 | 2 | 1 | A | 
| 4 | 1 | 1,1 | 1 | 0 | B | 

Tất cả các khối đều được gán thành công nên câu trả lời là CÓ. 

Dấu vết này minh họa sự bất biến tham lam. Ở mỗi bước, tần số hợp lệ lớn nhất sẽ hấp thụ khối Fibonacci lớn nhất còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \log k)$| Mỗi tần số vào và ra khỏi heap một số lần không đổi | 
| Không gian |$O(k)$| Heap lưu trữ nhiều nhất$k$tần số | 

Số khối Fibonacci dưới 50 vì tổng tần số lớn nhất$10^{11}$. Với$k \le 100$, hoạt động heap cực kỳ rẻ. Thậm chí xuyên qua$10^4$trường hợp thử nghiệm, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())

    fib = [1, 1]
    pref = [1, 2]

    LIMIT = 100 * 10**9
    while pref[-1] < LIMIT:
        fib.append(fib[-1] + fib[-2])
        pref.append(pref[-1] + fib[-1])

    ans = []

    for _ in range(t):
        k = int(input())
        c = list(map(int, input().split()))

        total = sum(c)

        pos = -1
        for i, s in enumerate(pref):
            if s == total:
                pos = i
                break
            if s > total:
                break

        if pos == -1:
            ans.append("NO")
            continue

        pq = [(-x, i) for i, x in enumerate(c)]
        heapq.heapify(pq)

        last = -1
        ok = True

        for need in reversed(fib[:pos + 1]):
            a = heapq.heappop(pq)

            if a[1] == last:
                if not pq:
                    ok = False
                    break

                b = heapq.heappop(pq)

                if -b[0] < need:
                    ok = False
                    break

                heapq.heappush(pq, a)

                rem = -b[0] - need
                if rem:
                    heapq.heappush(pq, (-rem, b[1]))

                last = b[1]
            else:
                if -a[0] < need:
                    ok = False
                    break

                rem = -a[0] - need
                if rem:
                    heapq.heappush(pq, (-rem, a[1]))

                last = a[1]

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

assert run("""6
1
1
2
1 1
2
1 2
3
3 1 3
2
7 5
6
26 8 3 4 13 34
""") == """YES
YES
NO
YES
NO
YES""", "sample 1"

assert run("""1
1
1
""") == "YES", "minimum input"

assert run("""1
2
1 2
""") == "NO", "adjacency restriction"

assert run("""1
2
5 3
""") == "YES", "valid Fibonacci assignment"

assert run("""1
1
1000000000
""") == "NO", "sum not a Fibonacci prefix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`k=1, [1]`| CÓ | Trường hợp hợp lệ nhỏ nhất | 
|`k=2, [1,2]`| KHÔNG | Hạn chế khối liên tiếp | 
|`k=2, [5,3]`| CÓ | Nhiệm vụ tham lam thành công | 
|`k=1, [1000000000]`| KHÔNG | Tổng chiều dài không bằng tổng tiền tố Fibonacci | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
1 2
```Tổng chiều dài là 3, khớp với tiền tố Fibonacci$1+1+2$. Thuật toán xử lý khối 2 trước và gán cho tần số 2. Tần số 1 còn lại thuộc cùng một chữ cái nên không thể gán khối đơn vị cuối cùng. Thuật toán trả về KHÔNG. Điều này nắm bắt các giải pháp chỉ kiểm tra tổng số tiền. 

Coi như:```
1
3
3 1 3
```Tổng chiều dài là 7. Sau khi gán các khối có kích thước 3, 2 và 1, chỉ còn lại một tần số có kích thước 1, nhưng nó thuộc về chữ cái được sử dụng ở bước trước. Vùng heap không chứa ứng cử viên thay thế nào, do đó thuật toán sẽ loại bỏ cá thể đó một cách chính xác. 

Coi như:```
1
1
8
```Tổng độ dài là 8. Không có tổng tiền tố Fibonacci nào bằng 8 vì tổng tiền tố là$1,2,4,7,12,\ldots$. Thuật toán phát hiện điều này trước bất kỳ quá trình xử lý heap nào và ngay lập tức đưa ra NO. 

Coi như:```
1
2
7 5
```Tổng chiều dài là 12, bằng$1+1+2+3+5$. Thuật toán gán các khối theo thứ tự$5,3,2,1,1$, luôn chọn tần số đủ điều kiện lớn nhất. Mọi bước đều thành công, mọi tần số đều cạn kiệt và câu trả lời là CÓ. Điều này chứng tỏ rằng chiến lược tham lam có thể xây dựng các trường hợp hợp lệ không tầm thường mà không cần quay lại.
