---
title: "CF 102284M - \u0422\u0440\u0438\u0441\u043a\u0430\u0439\u0434\u0435\u043a\u0430\u0444\u043e\u0431\u0438\u044f"
description: "Chúng ta cần làm việc với dãy tăng dần của tất cả các số nguyên dương có thừa số nguyên tố thuộc tập cố định {2, 3, 5, 7, 11}. Những số như vậy có thể được viết chính xác là [ 2^a3^b5^c7^d11^e, ] trong đó tất cả năm số mũ đều là số nguyên không âm."
date: "2026-08-13T09:04:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "M"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 155
verified: false
draft: false
---

[CF 102284M - \u0422\u0440\u0438\u0441\u043a\u0430\u0439\u0434\u0435\u043a\u0430\u0444\u 043e\u0431\u0438\u044f](https://codeforces.com/problemset/problem/102284/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần làm việc với dãy tăng dần của tất cả các số nguyên dương có thừa số nguyên tố thuộc tập cố định`{2, 3, 5, 7, 11}`. Những số như vậy có thể được viết chính xác như 

[ 
2^a3^b5^c7^d11^e, 
] 

trong đó tất cả năm số mũ đều là số nguyên không âm. số`1`được đưa vào vì nó không có thừa số nguyên tố. 

Đầu vào chứa`n`Và`k`. đầu tiên`n`các phần tử của chuỗi được coi là đã bị bỏ qua và chúng ta phải in thông tin sau`k`các yếu tố, cụ thể là`a_{n+1}`bởi vì`a_{n+k}`. Từ`n + k <= 200000`, chúng tôi không bao giờ cần nhiều hơn 200000 phần tử chuỗi đầu tiên. 

Khó khăn chính là trình tự thưa thớt. Mặc dù chúng ta chỉ cần 200000 phần tử, nhưng phần tử thứ 200000 lớn hơn nhiều so với 200000, do đó, việc quét mọi số nguyên thông thường đến giá trị đó là lãng phí. Các giới hạn được thiết kế cho một thuật toán có công việc phụ thuộc vào số lượng phần tử chuỗi được tạo ra, thay vì vào giá trị số của các phần tử đó. MỘT`O((n+k) log(n+k))`giải pháp dễ dàng hợp lý, trong khi một thuật toán tỷ lệ thuận với giá trị lớn nhất mà chúng ta phải kiểm tra có thể yêu cầu hàng trăm triệu hoặc hàng tỷ lần lặp. 

Có một số bẫy lập chỉ mục và sao chép. Đầu tiên,`n = 0`có nghĩa là câu trả lời bắt đầu bằng`1`. Ví dụ, đầu vào`0 1`phải sản xuất`1`. Một triển khai khởi tạo mảng được tạo bằng`1`nhưng sau đó bắt đầu đọc từ chỉ mục`n + 1`sai có thể bỏ qua. 

Cái bẫy thứ hai là các sản phẩm khác nhau có thể đại diện cho cùng một số. Ví dụ,`6`là cả hai`2 * 3`Và`3 * 2`. Đối với đầu vào`5 1`, đầu ra đúng là`6`. Việc triển khai chỉ tiến bộ một trình tạo khi một số sản phẩm ứng cử viên bằng mức tối thiểu sẽ chèn`6`hai lần và thay đổi mỗi câu trả lời sau. 

Bẫy thứ ba là sự khác biệt giữa mảng dựa trên số 0 được sử dụng trong quá trình triển khai và chuỗi toán học dựa trên một. Đối với đầu vào`12 1`, câu trả lời đúng là`14`, bởi vì mười hai phần tử đầu tiên kết thúc tại`12`. sử dụng`dp[n]`vì giá trị được yêu cầu đầu tiên thay vào đó sẽ trả về`14`chỉ khi việc lập chỉ mục mảng được xử lý một cách nhất quán, trong khi việc trộn các vị trí dựa trên một và dựa trên 0 mới có thể âm thầm trả về`12`hoặc`15`. 

Cuối cùng,`k`có thể nhỏ như một ngay cả khi`n`đang ở gần mức tối đa. Ví dụ,`199999 1`là hợp lệ bởi vì`n + k = 200000`. Thuật toán phải tạo chính xác 200000 phần tử bên trong và chỉ in phần tử cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra các số nguyên dương theo thứ tự tăng dần và kiểm tra xem mỗi số có thừa số nguyên tố nào lớn hơn không`11`. Một bài kiểm tra đơn giản liên tục chia ứng viên thành`2`,`3`,`5`,`7`, Và`11`, sau đó kiểm tra xem giá trị còn lại có phải là`1`. Mọi số được chấp nhận sẽ được thêm vào cho đến khi chúng tôi tạo ra`n+k`các phần tử. 

Điều này đúng vì sau khi loại bỏ mọi thừa số có thể có khỏi tập hợp được phép, một số sẽ trơn tru chính xác khi không còn lại gì. Vấn đề là kích thước của khoảng thời gian tìm kiếm. Nếu phần tử trình tự được yêu cầu cuối cùng là`a_{n+k}`, phương pháp brute-force kiểm tra mọi số nguyên từ`1`bởi vì`a_{n+k}`. Với năm bài kiểm tra tính chia hết cho mỗi số nguyên, nó thực hiện gần đúng`5 * a_{n+k}`kiểm tra khả năng phân chia trong việc thực hiện đơn giản. số lượng`a_{n+k}`lớn hơn rất nhiều so với`n+k`, do đó thuật toán dành gần như toàn bộ thời gian để loại bỏ những số không bao giờ là ứng cử viên cho câu trả lời. 

Quan sát cấu trúc hữu ích là mọi số hợp lệ đều có thể thu được từ một số hợp lệ khác bằng cách nhân với một trong năm số nguyên tố được phép. Bắt đầu từ`1`, nhân với`2`,`3`,`5`,`7`, hoặc`11`luôn giữ chúng ta ở trong trình tự. Quan trọng hơn, nếu trình tự đã được biết đến`a_i`, thì mọi phần tử tiếp theo có thể có phải là một trong 

[ 
2a_j,\quad 3a_j,\quad 5a_j,\quad 7a_j,\quad 11a_j 
] 

đối với một số chỉ mục trước đó`j`. 

Đối với mỗi số nguyên tố, chúng ta có thể giữ một con trỏ tới phần tử chuỗi đầu tiên có tích với số nguyên tố đó chưa được xem xét. Giá trị nhỏ nhất trong năm sản phẩm hiện tại là giá trị chuỗi tiếp theo. Khi một số tích bằng giá trị đó thì tất cả các con trỏ tương ứng phải tiến lên. Đây chính là ý tưởng tạo đơn điệu tương tự được sử dụng cho các dãy số xấu xí cổ điển, kéo dài từ ba số nguyên tố đến năm. 

Con trỏ không bao giờ di chuyển lùi. Mỗi con trỏ tiến lên nhiều nhất`n+k`lần và mỗi phần tử được tạo chỉ yêu cầu năm lần so sánh ứng cử viên. Điều đó thay đổi chi phí từ việc phụ thuộc vào kích thước bằng số của câu trả lời sang phụ thuộc trực tiếp vào số lượng phần tử chuỗi mà chúng ta cần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(a_{n+k})`kiểm tra tính chia hết |`O(n+k)`| Quá chậm | 
| Tối ưu |`O(n+k)`|`O(n+k)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cửa hàng`1`là phần tử chuỗi đầu tiên. Nó hợp lệ vì hệ số nguyên tố của nó trống. 
2. Tạo năm con trỏ, mỗi con trỏ đại diện cho một số nguyên tố`2`,`3`,`5`,`7`, Và`11`. Ban đầu tất cả các con trỏ đều`0`, nghĩa là mọi máy phát điện đều bắt đầu từ`1`. 
3. Đối với vị trí dãy tiếp theo, hãy tính năm ứng viên`dp[p2] * 2`,`dp[p3] * 3`,`dp[p5] * 5`,`dp[p7] * 7`, Và`dp[p11] * 11`. Ứng viên nhỏ nhất là số hợp lệ nhỏ nhất chưa được tạo ra. 
4. Thêm mức tối thiểu đó vào`dp`. Vì mọi ứng cử viên là một số hợp lệ và mọi số hợp lệ lớn hơn phần tử cuối cùng hiện tại có thể được biểu diễn dưới dạng số hợp lệ trước đó nhân với một trong năm số nguyên tố, nên ứng cử viên tối thiểu phải là phần tử chuỗi tiếp theo. 
5. Đối với mọi số nguyên tố có ứng cử viên bằng giá trị mới được tạo, hãy tăng con trỏ của nó lên một. Tất cả các con trỏ trùng khớp phải di chuyển, vì nếu không, giá trị tương tự sẽ xuất hiện lại ở lần lặp sau. 
6. Tiếp tục cho đến khi`n+k`các phần tử đã được tạo ra. Câu trả lời được yêu cầu chiếm vị trí dựa trên số 0`n`bởi vì`n+k-1`, do đó đầu ra`dp[n:n+k]`. 

Bất biến chính là sau khi tạo`dp[i]`, mảng chứa chính xác phần đầu tiên`i+1`các phần tử của dãy cần thiết theo thứ tự tăng dần. Với mọi số nguyên tố cho phép`p`, con trỏ của nó xác định chỉ mục đầu tiên có tích bằng`p`vẫn lớn hơn giá trị được tạo cuối cùng. Do đó, năm tích hiện tại đại diện cho các ứng viên nhỏ nhất chưa được đề cập trong cả năm họ phép nhân. Lấy mức tối thiểu của họ không thể bỏ qua một số hợp lệ. Việc nâng cao mọi con trỏ tạo ra mức tối thiểu sẽ ngăn ngừa sự trùng lặp. Bằng quy nạp, mọi phần tử được tạo ra chính xác là phần tử tiếp theo của dãy toán học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_terms(n, k):
    total = n + k
    primes = (2, 3, 5, 7, 11)

    dp = [1]
    ptr = [0] * 5

    while len(dp) < total:
        candidates = [
            dp[ptr[i]] * primes[i]
            for i in range(5)
        ]

        nxt = min(candidates)
        dp.append(nxt)

        for i in range(5):
            if candidates[i] == nxt:
                ptr[i] += 1

    return dp[n:n + k]

def solve():
    n, k = map(int, input().split())
    print(*get_terms(n, k))

if __name__ == "__main__":
    solve()
```các`dp`mảng lưu trữ chuỗi theo thứ tự tăng dần, bắt đầu bằng`1`. Năm mục của`ptr`là các máy phát điện chuyển động. Ở mỗi lần lặp, mã xây dựng chính xác năm ứng cử viên, chọn mức tối thiểu của chúng và sau đó nâng cao mọi con trỏ chịu trách nhiệm về mức tối thiểu đó. 

Việc kiểm tra tính bằng nhau ở vòng lặp cuối cùng là cần thiết. Giả sử các ứng cử viên hiện tại bao gồm`6`từ cả hai`2 * 3`Và`3 * 2`. Nếu chỉ có một con trỏ di chuyển,`6`sẽ vẫn là ứng cử viên của trình tạo khác và sẽ được thêm lại. Việc thúc đẩy tất cả các ứng cử viên bình đẳng sẽ loại bỏ mọi biểu thị của giá trị vừa được sử dụng. 

Điều kiện dừng sử dụng`len(dp) < total`, chính xác như vậy`n+k`các giá trị tuần tự được xây dựng. Điều này tránh được lỗi xảy ra trong khoảng thời gian được yêu cầu. Vì chuỗi toán học dựa trên một nhưng danh sách Python dựa trên 0,`a_{n+1}`tương ứng với`dp[n]`, đó chính xác là lý do tại sao lát cắt bắt đầu lúc`n`. 

Số nguyên Python có độ chính xác tùy ý, do đó nhân với`11`không thể tràn. Trong ngôn ngữ có số nguyên có chiều rộng cố định, việc triển khai tương ứng phải sử dụng loại đủ lớn cho giá trị được tạo lớn nhất. 

Năm con trỏ chỉ di chuyển về phía trước. Trong toàn bộ quá trình chạy, mỗi con trỏ tiến lên nhiều nhất`n+k`nhiều lần, do đó việc bảo trì con trỏ góp phần làm công việc tuyến tính. Năm phép tính ứng cử viên cho mỗi phần tử được tạo là công việc không đổi. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`0 13`. Chúng ta cần mười ba phần tử trình tự đầu tiên, vì vậy việc tạo bắt đầu từ`1`và tiếp tục cho đến khi mười ba giá trị được lưu trữ. 

| Bước | Giá trị được tạo ra | Tiền tố trình tự | 
| --- | --- | --- | 
| 1 | 1 |`1`| 
| 2 | 2 |`1 2`| 
| 3 | 3 |`1 2 3`| 
| 4 | 4 |`1 2 3 4`| 
| 5 | 5 |`1 2 3 4 5`| 
| 6 | 6 |`1 2 3 4 5 6`| 
| 7 | 7 |`1 2 3 4 5 6 7`| 
| 8 | 8 |`1 2 3 4 5 6 7 8`| 
| 9 | 9 |`1 2 3 4 5 6 7 8 9`| 
| 10 | 10 |`1 2 3 4 5 6 7 8 9 10`| 
| 11 | 11 |`1 2 3 4 5 6 7 8 9 10 11`| 
| 12 | 12 |`1 2 3 4 5 6 7 8 9 10 11 12`| 
| 13 | 14 |`1 2 3 4 5 6 7 8 9 10 11 12 14`| 

Sự chuyển đổi thú vị là từ`12`ĐẾN`14`. số`13`không thể được tạo ra bằng cách nhân một phần tử chuỗi trước đó với một trong các`2`,`3`,`5`,`7`, hoặc`11`, bởi vì`13`chính nó là thừa số nguyên tố bị cấm. Ứng cử viên có sẵn tiếp theo là`14 = 2 * 7`. 

Đối với Mẫu 2, 16 phần tử đầu tiên bị bỏ qua, do đó giá trị được yêu cầu đầu tiên là`a_17 = 20`. Phần có liên quan của chuỗi được tạo là: 

| Chỉ số trình tự | Giá trị được tạo ra | Vị trí yêu cầu | 
| --- | --- | --- | 
| 17 | 20 | 1 | 
| 18 | 21 | 2 | 
| 19 | 22 | 3 | 
| 20 | 24 | 4 | 
| 21 | 25 | 5 | 
| 22 | 27 | 6 | 
| 23 | 28 | 7 | 
| 24 | 30 | 8 | 
| 25 | 32 | 9 | 
| 26 | 33 | 10 | 
| 27 | 35 | 11 | 
| 28 | 36 | 12 | 
| 29 | 40 | 13 | 
| 30 | 42 | 14 | 
| 31 | 44 | 15 | 
| 32 | 45 | 16 | 

Ví dụ, sau`36`, ứng cử viên tiếp theo bao gồm`40 = 2 * 20`,`42 = 2 * 21`hoặc`3 * 14`và các giá trị lớn hơn từ các trình tạo khác. Tối thiểu là`40`, do đó nó trở thành phần tử chuỗi tiếp theo. Sự biểu diễn trùng lặp của`42`cũng minh họa tại sao tất cả các con trỏ có ứng cử viên bằng mức tối thiểu đã chọn phải cùng tiến lên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n+k)`| Năm ứng cử viên có thời gian không đổi được đánh giá cho từng phần tử được tạo và tất cả năm con trỏ chỉ di chuyển về phía trước. | 
| Không gian |`O(n+k)`| Trình tự được tạo chứa chính xác`n+k`giá trị, cộng với năm con trỏ. | 

Độ dài chuỗi yêu cầu tối đa chỉ là 200000, do đó thuật toán tối ưu thực hiện một lượng công việc không đổi nhỏ cho mỗi phần tử được yêu cầu. Nó không phụ thuộc vào giá trị thứ 200000 lớn đến mức nào, đây là thuộc tính làm cho nó phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

input = sys.stdin.readline

def get_terms(n, k):
    total = n + k
    primes = (2, 3, 5, 7, 11)

    dp = [1]
    ptr = [0] * 5

    while len(dp) < total:
        candidates = [
            dp[ptr[i]] * primes[i]
            for i in range(5)
        ]

        nxt = min(candidates)
        dp.append(nxt)

        for i in range(5):
            if candidates[i] == nxt:
                ptr[i] += 1

    return dp[n:n + k]

def solve():
    n, k = map(int, input().split())
    print(*get_terms(n, k))

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

def reference_heap(count):
    primes = (2, 3, 5, 7, 11)
    heap = [1]
    seen = {1}
    result = []

    while len(result) < count:
        x = heapq.heappop(heap)
        result.append(x)

        for p in primes:
            y = x * p
            if y not in seen:
                seen.add(y)
                heapq.heappush(heap, y)

    return result

# Provided samples
assert run("0 13") == (
    "1 2 3 4 5 6 7 8 9 10 11 12 14"
), "sample 1"

assert run("16 16") == (
    "20 21 22 24 25 27 28 30 32 33 35 36 40 42 44 45"
), "sample 2"

# Minimum input
assert run("0 1") == "1", "must include 1"

# Consecutive early values
assert run("1 5") == "2 3 4 5 6", "basic indexing"

# Equal candidate products: 6 can be produced as 2*3 and 3*2
assert run("5 1") == "6", "duplicate candidate handling"

# Boundary around 12, 14 and 15
assert run("12 5") == "14 15 16 18 20", "off-by-one boundary"

# Maximum-size input
expected = reference_heap(200000)[199999]
assert run("199999 1") == str(expected), "maximum n+k boundary"

# Large output length
answer = run("190000 10000").split()
assert len(answer) == 10000, "maximum k"
assert all(int(answer[i]) < int(answer[i + 1])
           for i in range(len(answer) - 1)), "strictly increasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 1`|`1`| Trình tự bắt đầu bằng`1`. | 
|`1 5`|`2 3 4 5 6`| Lập chỉ mục cơ bản dựa trên một đến không. | 
|`5 1`|`6`| Nhiều máy phát điện tạo ra cùng một giá trị. | 
|`12 5`|`14 15 16 18 20`| Số nguyên tố bị thiếu`13`và một ranh giới chỉ mục. | 
|`199999 1`| Giá trị thứ 200000 được tạo độc lập | Tối đa cho phép`n+k`. | 
|`190000 10000`| 10000 giá trị tăng dần | Tối đa`k`và chiều dài đầu ra. | 

Xác nhận kích thước tối đa sử dụng triển khai tham chiếu dựa trên heap thay vì chính thuật toán con trỏ. Điều đó mang lại cho bài kiểm tra một cách khác về mặt cấu trúc để tạo ra giá trị mong đợi và làm cho việc kiểm tra ranh giới trở nên hữu ích hơn. 

## Vỏ cạnh 

Đối với đầu vào`0 1`, thuật toán khởi tạo`dp`với`[1]`. Vì tổng số được yêu cầu đã là một nên vòng lặp tạo không chạy và`dp[0:1]`cho`1`. Điều này nắm bắt các triển khai vô tình bắt đầu tạo từ`2`. 

Đối với đầu vào`5 1`, năm giá trị đầu tiên là`1, 2, 3, 4, 5`. Các ứng cử viên hiện tại cho giá trị tiếp theo bao gồm`2 * 3 = 6`Và`3 * 2 = 6`. Tối thiểu là`6`, và cả`2`con trỏ và`3`con trỏ được nâng cao. Ở lần lặp tiếp theo, không có trình tạo nào đề xuất`6`một lần nữa, vì vậy kết quả đầu ra là chính xác`6`. Đây là trường hợp xử lý trùng lặp trung tâm. 

Đối với đầu vào`12 1`, tiền tố được tạo là`1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12`. Các ứng cử viên tiếp theo bao gồm`14 = 2 * 7`, trong khi`13`không có cách xây dựng hợp lệ vì`13`không phải là một trong những thừa số nguyên tố được phép. Thuật toán nối thêm`14`và xuất nó. Điều này nắm bắt cả ranh giới thiếu nguyên tố và lỗi sai sót trong việc chọn`a_{n+1}`. 

Đối với đầu vào`199999 1`, thuật toán tạo chính xác 200000 phần tử và sau đó trả về phần tử ở chỉ số dựa trên 0`199999`. Nó không tạo ra giá trị thứ 200001 không cần thiết. Điều này thực hiện ranh giới trên của`n+k`và xác nhận rằng việc triển khai tỷ lệ theo số lượng phần tử chuỗi được yêu cầu thay vì theo độ lớn bằng số của câu trả lời cuối cùng.
