---
title: "CF 102348E - Sơn Hàng Rào"
description: "Chúng ta có một hàng gồm (n) tấm ván hàng rào và (m) màu sắc. Màu (i) có sẵn cho chính xác (ai) tấm ván và các giá trị tổng bằng (n), vì vậy mọi đơn vị sơn phải được sử dụng."
date: "2026-08-14T11:51:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "E"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 844
verified: true
draft: false
---

[CF 102348E - Sơn hàng rào](https://codeforces.com/problemset/problem/102348/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng gồm (n) tấm ván hàng rào và (m) màu sắc. Màu (i) có sẵn cho chính xác (a_i) tấm ván và các giá trị tổng bằng (n), vì vậy mọi đơn vị sơn phải được sử dụng. Chúng ta cần hoán vị các lần xuất hiện màu này dọc theo hàng rào sao cho không có đường chạy liền kề tối đa nào của một màu có chiều dài lớn hơn (k). 

Đầu ra là một mảng màu có độ dài-(n) như vậy, sử dụng chính xác mọi màu (a_i) lần hoặc (-1) nếu không có sự sắp xếp hợp lệ nào. 

Ràng buộc (n \le 2\cdot 10^5) loại trừ mọi thứ khám phá một phần lớn các hoán vị có thể có. Ngay cả (O(n^2)) cũng có nghĩa là xung quanh các hoạt động (4\cdot10^{10}) trong trường hợp xấu nhất. Chúng ta cần một giải pháp cơ bản tuyến tính hoặc (O(n\log n)). Hàng đợi ưu tiên là phù hợp vì mọi vị trí đều có thể được quyết định bằng cách lấy màu có số lượng còn lại lớn nhất, đồng thời tạm thời loại trừ màu đã đạt đến giới hạn chạy. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. Đầu tiên, một màu có thể chính xác ở ranh giới khả thi. Ví dụ,```
6 2 3
5 1
```là có thể, với`1 1 1 2 1 1`. Dòng màu 1 có độ dài chính xác (3), do đó, việc từ chối các dòng có độ dài (k) thay vì các dòng lớn hơn (k) sẽ in không chính xác (-1). 

Trường hợp thứ hai là khi màu lớn nhất quá lớn mặc dù có nhiều màu khác tồn tại. Ví dụ,```
8 2 3
7 1
```là không thể. Bảy bản sao màu 1 yêu cầu ít nhất ba lần chạy riêng biệt, nhưng tấm ván đơn màu 2 chỉ có thể phân tách hai ranh giới. Việc triển khai tham lam chỉ đơn giản là bắt đầu đặt màu lớn nhất mà không kiểm tra tính khả thi cuối cùng có thể gặp khó khăn và cần xử lý tình huống đó một cách chính xác. 

Đầu vào nhỏ nhất có thể cũng đặc biệt:```
1 1 1
1
```Câu trả lời hợp lệ duy nhất là`1`. Không có màu trước đó và không có khả năng vi phạm lần chạy, vì vậy việc khởi tạo không được cho rằng câu trả lời đã chứa bảng trước đó. 

Cuối cùng, (k) có thể lớn hơn mọi lần chạy hữu ích. Ví dụ,```
5 2 5
4 1
```có giá trị như`1 1 1 1 2`. Khi (k\ge n), hạn chế chạy thực sự không liên quan, do đó thuật toán không được ép buộc những thay đổi màu không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp coi hàng rào là một vấn đề hoán vị. Ở mỗi tấm ván, chúng tôi thử mọi màu có số lượng còn lại là dương, tiếp tục đệ quy và từ chối một nhánh ngay khi số lần chạy hiện tại của nó vượt quá (k). Điều này đúng vì mọi màu có thể cuối cùng đều được xem xét và màu hợp lệ được chấp nhận. 

Vấn đề là số lượng màu có thể có. Ngay cả khi bỏ qua các bội số cố định, vẫn có (m^n) chuỗi (n) màu. Việc kiểm tra một chuỗi hoàn chỉnh mất (O(n)), do đó, một tìm kiếm toàn diện đơn giản có thể mất (O(nm^n)) thời gian. Với (m=n=2\cdot10^5), giới hạn trong trường hợp xấu nhất này gần như là (O(n^{n+1})), điều này hoàn toàn không khả thi. 

Quan sát hữu ích là chúng ta không bao giờ quan tâm đến danh tính của những vị trí chưa được lấp đầy. Ở mỗi bước, điều quan trọng là còn lại bao nhiêu bản sao của mỗi màu, màu nào được sử dụng cuối cùng và thời gian chạy hiện tại là bao lâu. Trong số các màu có sẵn, màu có số lượng còn lại lớn nhất là màu nguy hiểm nhất. Nếu chúng ta không sử dụng nó trong khi sử dụng ít màu hơn, các bản sao còn lại của nó sẽ khó đặt hơn sau này. 

Điều đó dẫn đến một hàng đợi ưu tiên. Chúng tôi luôn lấy màu có số lượng còn lại lớn nhất. Nếu màu đó khác với màu trước thì chúng ta có thể sử dụng ngay. Nếu nó có cùng màu và lượt chạy hiện tại đã đạt đến (k), chúng tôi tạm thời lấy màu lớn thứ hai để thay thế. Sau khi sử dụng một màu một lần, số lượng còn lại của màu đó sẽ giảm đi và nó sẽ được trả về vùng nhớ heap nếu vẫn còn bản sao. 

Ngoài ra còn có một điều kiện khả thi đơn giản. Giả sử màu (c) xảy ra (A) lần. Vì mỗi lần chạy (c) chứa nhiều nhất (k) bản sao nên chúng ta cần ít nhất (\lceil A/k\rceil) lần chạy riêng biệt của (c). Giữa các lần chạy phải có ít nhất (\lceil A/k\rceil-1) tấm ván có màu khác. Như vậy, 

[ 
\left\lceil\frac{A}{k}\right\rceil \le n-A+1, 
] 

tương đương với 

[ 
A \le k(n-A+1). 
] 

Màu khó nhất là màu có (A) lớn nhất, vì vậy chỉ cần kiểm tra mức tối đa (a_i) là đủ. Điều này đưa ra một bài kiểm tra tính không thể ngay lập tức trước khi xây dựng câu trả lời. 

Brute-force hoạt động vì nó khám phá mọi thứ tự có thể có, nhưng không thành công vì có nhiều thứ tự theo cấp số nhân. Nhận xét rằng chỉ có màu lớn nhất còn lại có nguy cơ không thể đặt được cho phép chúng ta đưa ra từng quyết định một cách tham lam với một đống tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm^n)) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(n\log m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng màu và tìm số lượng lớn nhất (A). Nếu (A>k(n-A+1)), in ra (-1). Chỉ riêng màu lớn nhất đã cần nhiều đường chạy riêng biệt hơn những tấm ván khác có thể cung cấp. 
2. Đặt mọi màu có số lượng còn lại vào một đống tối đa. của Python`heapq`là một đống tối thiểu, vì vậy chúng tôi lưu trữ số âm. 
3. Giữ`last`, màu sắc được sử dụng trên tấm ván trước đó, và`run`, độ dài của lần chạy liên tiếp hiện tại của nó. Ban đầu không có màu trước đó, vì vậy`last = -1`Và`run = 0`. 
4. Tại mọi vị trí, loại bỏ màu có số lượng còn lại lớn nhất khỏi đống. 
5. Nếu màu đó khác với`last`, sử dụng nó. Độ dài chạy mới trở thành (1). 
6. Nếu nó bằng`last`Và`run < k`, sử dụng lại. Nên tiếp tục giữ nguyên màu vì nó có số lượng còn lại lớn nhất và lượt chạy hiện tại vẫn còn chỗ. 
7. Nếu nó bằng`last`Và`run = k`, bây giờ nó không thể được sử dụng. Xóa màu lớn nhất tiếp theo khỏi heap và thay vào đó sử dụng màu đó. Trả lại màu bị chặn cho heap không thay đổi. Nếu không có màu thứ hai thì việc xây dựng không thể tiếp tục. 
8. Giảm số lượng còn lại của màu đã chọn. Nếu nó vẫn còn bản sao, hãy chèn nó trở lại heap. 
9. Lặp lại cho đến khi tất cả (n) tấm ván được chỉ định. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi tiền tố được xây dựng, vùng heap chứa chính xác các bản sao chưa sử dụng của mọi màu, trong khi tiền tố đã đáp ứng giới hạn độ dài chạy. Bất cứ khi nào màu còn lại thường xuyên nhất được phép tiếp tục, việc sử dụng nó là an toàn vì việc trì hoãn một màu có số lượng còn lại nhỏ hơn không thể làm cho màu nhỏ hơn đó khó đặt hơn. Bất cứ khi nào lượt chạy hiện tại đạt đến (k), việc tiếp tục bị cấm, vì vậy bất kỳ sự tiếp tục hợp lệ nào cũng phải chọn một màu khác. Việc lựa chọn phương án thay thế lớn nhất hiện có sẽ bảo toàn được nguồn tài nguyên còn lại bị hạn chế nhất. 

Sự bất bình đẳng về tính khả thi đảm bảo rằng màu lớn nhất có đủ các tấm ván không khớp để phân tách tất cả các lần chạy cần thiết của nó. Sự lựa chọn tham lam luôn chỉ sử dụng những màu tách biệt đó khi buộc phải chuyển đổi, thay vì lãng phí chúng trong khi màu hiện tại vẫn có thể tiếp tục hợp pháp. Do đó, nếu điều kiện khả thi ban đầu được giữ nguyên, việc xây dựng vùng heap có thể sử dụng tất cả các bản sao mà không bị kẹt. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    mx = max(a)

    if mx > k * (n - mx + 1):
        print(-1)
        return

    heap = []
    for color, count in enumerate(a, 1):
        heapq.heappush(heap, (-count, color))

    ans = []
    last = -1
    run = 0

    for _ in range(n):
        neg_count, color = heapq.heappop(heap)
        count = -neg_count

        if color == last and run == k:
            if not heap:
                print(-1)
                return

            neg_count2, color2 = heapq.heappop(heap)
            count2 = -neg_count2

            heapq.heappush(heap, (-count, color))

            color = color2
            count = count2
            run = 1
        else:
            if color == last:
                run += 1
            else:
                run = 1

        ans.append(color)
        count -= 1

        if count > 0:
            heapq.heappush(heap, (-count, color))

        last = color

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc kiểm tra tính khả thi chỉ sử dụng số lượng tối đa. Đối với một màu có (A) bản sao, cần phải chạy ít nhất (\lceil A/k\rceil) và (n-A) các bảng khác cung cấp tối đa (n-A+1) vị trí chạy có thể có. Vì bất đẳng thức trở nên khó khăn hơn khi (A) tăng lên nên việc kiểm tra số lượng tối đa sẽ bao trùm mọi màu. 

Heap lưu trữ các cặp`(-count, color)`sao cho giá trị heap nhỏ nhất tương ứng với số lượng lớn nhất còn lại. Chỉ số màu được lưu trữ rõ ràng vì hai màu có thể có cùng số lượng và vẫn cần phải phân biệt được. 

Chi nhánh đặc biệt nơi`color == last and run == k`là điều kiện biên quan trọng. Màu hiện tại đã chiếm chính xác (k) vị trí liên tiếp, do đó, việc sử dụng màu này thêm một lần nữa sẽ tạo ra một chuỗi có độ dài (k+1). Chúng tôi tạm thời loại bỏ nó, chọn màu tốt nhất tiếp theo và đặt màu bị chặn trở lại không thay đổi. 

Khi một màu đã chọn còn lại một bản sao, việc giảm số lượng của nó sẽ tạo ra 0 và đơn giản là nó không bị đẩy lùi. Vì đầu vào đảm bảo rằng tổng số lần đếm là (n), nên cần có chính xác (n) lựa chọn thành công. 

Số nguyên Python không bị tràn và tích lớn nhất trong thử nghiệm tính khả thi tối đa là (n^2), khoảng (4\cdot10^{10}), mà Python xử lý trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 2 1
2 3
```Ở đây (k=1), vì vậy các màu bằng nhau có thể không bao giờ ở cạnh nhau. Số lớn nhất là (3) và 

[ 
3 \le 1(5-3+1)=3, 
] 

vì vậy trường hợp này chính xác ở ranh giới khả thi. 

| Vị trí | Đống trước khi lựa chọn | Cuối cùng | Chạy | Màu được chọn | Số lượng đã chọn còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`(3,2), (2,1)`| không | 0 | 2 | 2 | 
| 2 |`(2,1), (2,2)`| 2 | 1 | 1 | 1 | 
| 3 |`(2,2), (1,1)`| 1 | 1 | 2 | 1 | 
| 4 |`(1,1), (1,2)`| 2 | 1 | 1 | 0 | 
| 5 |`(1,2)`| 1 | 1 | 2 | 0 | 

Màu kết quả là`2 1 2 1 2`. Vì (k=1) nên mỗi bước đều buộc phải chuyển màu, và bất đẳng thức khả thi cho ta biết màu 2 có đủ ván ngăn cách. 

### Mẫu 2 

Đầu vào là```
8 2 3
1 7
```Màu lớn nhất có (A=7) bản sao. Số lần chạy cần thiết của nó là 

[ 
\left\lceil\frac{7}{3}\right\rceil=3. 
] 

Nhưng chỉ có duy nhất một tấm ván màu kia nên tối đa có thể tách được hai vạch màu 1. Tương đương, 

[ 
7 > 3(8-7+1)=6. 
] 

Thuật toán từ chối phiên bản trước khi xây dựng bất cứ thứ gì. 

| Giá trị | Tiểu bang | 
| --- | --- | 
| (n) | 8 | 
| (k) | 3 | 
| Số lượng lớn nhất (A) | 7 | 
| Công suất tách tối đa có thể | (3(8-7+1)=6) | 
| Khả thi? | Không | 
| Đầu ra |`-1`| 

Điều này chứng tỏ tại sao việc kiểm tra tính khả thi phải sử dụng`n - A + 1`, thay vì chỉ đếm xem có bao nhiêu màu khác tồn tại. Một tấm ván khác tạo ra tối đa hai nhóm màu chủ đạo riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log m)) | Mỗi ván gây ra một số lượng thao tác heap không đổi, mỗi lần thực hiện (O(\log m)). | 
| Không gian | (O(n+m)) | Câu trả lời chứa (n) màu và đống chứa tối đa (m) mục màu. | 

Với (n\le2\cdot10^5), thuật toán chỉ thực hiện khối lượng công việc theo logarit trên mỗi tấm ván. Việc sử dụng bộ nhớ là tuyến tính nên nó phù hợp thoải mái với giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Vì đầu ra hợp lệ không phải là duy nhất nên bộ khai thác thử nghiệm không được so sánh các đầu ra thành công với một chuỗi cố định. Thay vào đó, nó kiểm tra xem đầu ra có đúng số lượng ván hay không, sử dụng mọi màu theo số lần yêu cầu và không bao giờ tạo ra một lần chạy dài hơn (k). Đối với những trường hợp không thể, một cách chính xác`-1`so sánh là phù hợp.```python
import sys
import io
import heapq

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    mx = max(a)

    if mx > k * (n - mx + 1):
        print(-1)
        return

    heap = []
    for color, count in enumerate(a, 1):
        heapq.heappush(heap, (-count, color))

    ans = []
    last = -1
    run = 0

    for _ in range(n):
        neg_count, color = heapq.heappop(heap)
        count = -neg_count

        if color == last and run == k:
            if not heap:
                print(-1)
                return

            neg_count2, color2 = heapq.heappop(heap)
            count2 = -neg_count2

            heapq.heappush(heap, (-count, color))

            color = color2
            count = count2
            run = 1
        else:
            if color == last:
                run += 1
            else:
                run = 1

        ans.append(color)
        count -= 1

        if count > 0:
            heapq.heappush(heap, (-count, color))

        last = color

    print(*ans)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = sys.stdin.readline

def validate(inp: str, out: str):
    data = list(map(int, inp.split()))
    n, m, k = data[0], data[1], data[2]
    a = data[3:3 + m]

    assert out != "-1"

    ans = list(map(int, out.split()))
    assert len(ans) == n

    cnt = [0] * (m + 1)

    last = -1
    run = 0

    for color in ans:
        assert 1 <= color <= m
        cnt[color] += 1

        if color == last:
            run += 1
        else:
            last = color
            run = 1

        assert run <= k

    for color in range(1, m + 1):
        assert cnt[color] == a[color - 1]

# Provided samples
sample1 = """\
5 2 1
2 3
"""
out = run(sample1)
validate(sample1, out)

sample2 = """\
8 2 3
1 7
"""
assert run(sample2) == "-1", "sample 2"

sample3 = """\
10 3 2
5 2 3
"""
out = run(sample3)
validate(sample3, out)

# Minimum-size input
case1 = """\
1 1 1
1
"""
assert run(case1) == "1", "minimum-size case"

# Exact feasibility boundary
case2 = """\
6 2 3
5 1
"""
out = run(case2)
validate(case2, out)

# All counts equal
case3 = """\
12 3 4
4 4 4
"""
out = run(case3)
validate(case3, out)

# Maximum-size input
case4 = "200000 2 100000\n100000 100000\n"
out = run(case4)
validate(case4, out)

# Just beyond the feasibility boundary
case5 = """\
8 2 3
7 1
"""
assert run(case5) == "-1", "impossible boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1`|`1`| Kích thước tối thiểu và khởi tạo không có màu trước đó | 
|`6 2 3 / 5 1`| Bất kỳ màu hợp lệ nào, chẳng hạn như`1 1 1 2 1 1`| Ranh giới chính xác nơi cho phép chạy chiều dài (k) | 
|`12 3 4 / 4 4 4`| Bất kỳ màu hợp lệ nào | Tần số màu bằng nhau và mối quan hệ trong heap | 
|`200000 2 100000 / 100000 100000`| Bất kỳ màu hợp lệ nào | Tối đa (n), hoạt động heap lớn và chạy chính xác tại (k) | 
|`8 2 3 / 7 1`|`-1`| Tính khả thi bất bình đẳng và màu sắc chủ đạo không thể | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
1 1 1
1
```số lượng tối đa là (1) và kiểm tra tính khả thi cho (1\le1(1-1+1)). Đống chỉ chứa màu 1, được chọn một lần. Vì không có màu trước đó nên quá trình chạy bắt đầu ở (1) và kết quả đầu ra chính xác là`1`. 

Đối với trường hợp biên chính xác```
6 2 3
5 1
```màu chủ đạo có năm bản sao. Nó cần hai lần chạy vì (\lceil5/3\rceil=2) và một bản sao duy nhất của màu 2 là đủ để tách chúng ra. Cấu trúc tham lam lấy màu 1 ba lần, chuyển sang màu 2 khi chạy đến (3), sau đó lấy màu 1 hai lần. Kết quả là`1 1 1 2 1 1`, lần chạy dài nhất của nó có độ dài chính xác (3). 

Đối với tần số bằng nhau,```
12 3 4
4 4 4
```mọi màu đều có mức độ ưu tiên như nhau trong heap. Chỉ số màu của heap phá vỡ các ràng buộc một cách nhất quán, do đó cấu trúc có thể tạo ra bốn bản sao của một màu, tiếp theo là bốn bản sao của màu khác và bốn bản sao của màu thứ ba. Mỗi lần chạy có độ dài (4), chính xác là mức tối đa cho phép. 

Đối với ranh giới không thể,```
8 2 3
7 1
```màu chủ đạo cần ít nhất ba lần chạy, trong khi một tấm ván khác có thể tách tối đa hai lần chạy như vậy. Bất đẳng thức trở thành (7\le6), sai, do đó thuật toán in ra`-1`ngay lập tức. Không cần xây dựng một phần và không có rủi ro báo cáo tiền tố không thể hoàn thành. 

Trường hợp (k=1) được xử lý theo logic tương tự. Mỗi khi màu trước đó ở trên cùng của đống,`run == k`đã đúng nên thuật toán phải chọn một màu khác. Điều kiện khả thi giảm xuống còn (A\le n-A+1), yêu cầu quen thuộc là màu thường xuyên nhất phải có đủ các yếu tố khác để phân tách tất cả các bản sao của nó.
