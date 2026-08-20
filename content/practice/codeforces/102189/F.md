---
title: "CF 102189F - \u0421\u0438\u0433\u043d\u0430\u0442\u0443\u0440\u0430"
description: "Chúng ta được cung cấp độ dài của các khối liên tiếp mà một chuỗi phải có. Nếu chữ ký là (s1,s2,ldots,sn), thì chuỗi cuối cùng phải chứa một khối gồm (s1) giá trị bằng nhau, theo sau là một giá trị khác được lặp lại (s2) lần, sau đó là một giá trị khác khác được lặp lại (s3) lần…"
date: "2026-08-19T06:20:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102189
codeforces_index: "F"
codeforces_contest_name: "12-\u0439 \u043e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0442\u0443\u0440\u043d\u0438\u0440 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0432 \u0410\u0431\u0430\u043a\u0430\u043d\u0435"
rating: 0
weight: 102189
solve_time_s: 94
verified: true
draft: false
---

[CF 102189F - \u0421\u0438\u0433\u043d\u0430\u0442\u0443\u0440\u0430](https://codeforces.com/problemset/problem/102189/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp độ dài của các khối liên tiếp mà một chuỗi phải có. Nếu chữ ký là (s_1,s_2,\ldots,s_n), thì chuỗi cuối cùng phải chứa một khối (s_1) giá trị bằng nhau, theo sau là một giá trị khác được lặp lại (s_2) lần, sau đó là một giá trị khác khác được lặp lại (s_3), v.v. Bản thân các giá trị là số nguyên dương và nhiệm vụ là chọn chúng sao cho tổng của tất cả các phần tử càng nhỏ càng tốt. 

Ví dụ: đối với chữ ký ([2,1,3,1]), các giá trị khối có thể là (1,2,1,2), đưa ra chuỗi ([1,1,2,1,1,1,2]). Bốn giá trị khối chỉ cần khác với các giá trị lân cận của chúng. Các khối không liền kề có thể sử dụng cùng một giá trị. 

Có thể có tối đa (10^5) khối, trong khi tổng chiều dài của chúng tối đa là (10^6). Vì bản thân đầu ra có thể chứa (10^6) số nguyên nên thuật toán (O(n)) về cơ bản là mục tiêu tự nhiên. Các thuật toán có hệ số bậc hai trong (n) sẽ yêu cầu khoảng (10^{10}) phép tính trong trường hợp xấu nhất, vượt xa giới hạn thời gian dự kiến. Thực tế là tổng độ dài chữ ký được giới hạn bởi (10^6) cũng có nghĩa là việc xây dựng câu trả lời một cách rõ ràng là khả thi, nhưng chúng ta nên tránh thực hiện nhiều hơn một lượng công việc không đổi cho mỗi phần tử đầu ra. 

Trường hợp cạnh đầu tiên là chữ ký chỉ chứa một khối. Đối với đầu vào (n=1), (s=[5]), câu trả lời là năm bản sao của (1). Không có khối lân cận nào buộc giá trị lớn hơn, vì vậy việc sử dụng (2), (3) hoặc bất kỳ giá trị lớn hơn nào sẽ chỉ làm tăng tổng. 

Trường hợp cạnh thứ hai là khi giá trị nhỏ nhất cho khối hiện tại không nhất thiết là lựa chọn tốt nhất cục bộ. Đối với (s=[1,3]), việc chọn các giá trị khối (1,2) sẽ cho tổng (1+3\cdot2=7), trong khi chọn (2,1) sẽ cho tổng (2+3=5). Vì vậy, quy tắc tham lam luôn bắt đầu bằng (1) là sai. 

Trường hợp cạnh thứ ba xảy ra khi nhiều khối liên tiếp có cùng độ dài. Đối với (s=[2,2,2]), các giá trị khối tối ưu là (1,2,1), tạo ra ([1,1,2,2,1,1]). Phép gán (1,2,3) hợp lệ nhưng tổng của nó lớn hơn. Việc xây dựng chỉ yêu cầu các giá trị khối liền kề khác nhau, do đó việc xen kẽ hai giá trị nhỏ nhất là đủ. 

Cuối cùng, đầu ra chứa các giá trị lặp lại chứ không phải chữ ký. Với (s=[1,3]), xuất ra`2 1 1 1`đúng vì chữ ký của nó là ([1,3]). Một lỗi triển khai phổ biến là giải chính xác giá trị của từng khối nhưng lại quên mở rộng giá trị đó theo độ dài khối của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ gán một số nguyên dương cho mỗi khối, kiểm tra xem các phép gán lân cận có khác nhau không, mở rộng các khối và tính tổng kết quả. Vì tổng độ dài chữ ký tối đa là (10^6), nên chỉ cần xem xét các giá trị từ (1) đến (10^6), vì không có câu trả lời tối ưu nào cần giá trị lớn hơn tổng độ dài đầu ra. Ngay cả với giới hạn nhân tạo này, vẫn có thể có ((10^6)^n) bài tập. Ở mức tối đa (n=10^5), tức là (10^{600000}) ứng viên, vì vậy việc tìm kiếm toàn diện là hoàn toàn không khả thi. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn có thể quan sát thấy rằng chỉ những giá trị nhỏ mới có khả năng quan trọng và thử một số giá trị có thể có cho mỗi khối. Điều này vẫn có hệ số phân nhánh cấp số nhân ở mọi vị trí, vì vậy nó vẫn theo cấp số nhân. Câu hỏi thực sự là tại sao các giá trị lớn hơn (2) không bao giờ phải xuất hiện. 

Hãy xem xét trạng thái lập trình động cho khối có giá trị được chọn là (x). Giả sử rằng, đối với khối trước đó, chi phí tối thiểu có thể đạt được đạt được bằng giá trị (1). Khi đó việc chọn (2) cho khối hiện tại là hợp pháp và tốn ít chi phí hơn so với việc chọn bất kỳ (x\ge3) nào. Thay vào đó, nếu đạt được mức tối thiểu cho khối trước đó bằng giá trị (2), thì việc chọn (1) cho khối hiện tại là hợp pháp và chi phí thấp hơn mọi (x\ge3). Bằng quy nạp, sau mỗi khối, trạng thái tốt nhất có thể được biểu diễn bằng một trong các giá trị (1) và (2). Giá trị ít nhất (3) luôn bị chi phối bởi một trong hai lựa chọn đó. 

Khi chỉ còn lại (1) và (2), bài toán trở nên đặc biệt đơn giản. Các khối liền kề phải có các giá trị khác nhau nên sau khi chọn giá trị của khối đầu tiên thì mọi giá trị sau đều bị ép buộc. Chỉ có hai mẫu có thể: 

[ 
1,2,1,2,\ldots 
] 

và 

[ 
2,1,2,1,\ldots 
] 

Chúng ta có thể tính chi phí của cả hai mẫu và chọn mẫu nhỏ hơn. Do đó, việc tìm kiếm mạnh mẽ trên các giá trị tùy ý đã thu gọn thành hai ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^{6n})) ứng viên | (O(n)) | Quá chậm | 
| DP hai mẫu | (O(n+\sum s_i)) | (O(n+\sum s_i)) cho đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chữ ký (s_1,s_2,\ldots,s_n). Mỗi (s_i) cho chúng ta biết có bao nhiêu bản sao của giá trị được gán cho khối (i) phải được viết. 
2. Tính chi phí để khối thứ nhất có giá trị (1) và chi phí để khối đó có giá trị (2). Đây là (s_1) và (2s_1). 
3. Với mỗi khối tiếp theo (i), hoán đổi các giá trị được sử dụng bởi hai trạng thái trước đó. Nếu khối trước được sử dụng (1), khối hiện tại phải sử dụng (2). Nếu khối trước được sử dụng (2), khối hiện tại phải sử dụng (1). Giá của một khối có chiều dài (s_i) là chiều dài của nó nhân với giá trị đã chọn. 
4. Giữ nguyên tổng chi phí của cả hai mẫu xen kẽ. Cuối cùng chọn mẫu có tổng chi phí nhỏ hơn. 
5. Mở rộng mẫu đã chọn. Nếu giá trị khối được chọn là (1), hãy thêm (s_i) bản sao của`1`; nếu là (2), hãy thêm (s_i) bản sao của`2`. Tổng số số nguyên được thêm vào chính xác là (\sum s_i), tối đa là (10^6). 

Tại sao nó hoạt động có thể được nêu như một bất biến. Sau khối xử lý (i), hai chi phí duy trì chính xác là chi phí tối thiểu giữa các giải pháp sử dụng hai mẫu xen kẽ có thể có kết thúc bằng giá trị (1) và (2). Bất kỳ giải pháp tối ưu nào cũng có thể bị giới hạn ở các giá trị (1) và (2), bởi vì bất cứ khi nào trạng thái có giá trị ít nhất (3) có thể là tối ưu, thì một trong (1) hoặc (2) có sẵn từ trạng thái tối ưu trước đó và có đóng góp nhỏ hơn rất nhiều cho độ dài khối dương hiện tại. Với các giá trị (1) và (2), ràng buộc đường dẫn buộc một trong chính xác hai mẫu xen kẽ. Do đó, so sánh hai mẫu đó sẽ tìm thấy mức tối thiểu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = list(map(int, input().split()))

    # cost1: pattern 1,2,1,2,...
    # cost2: pattern 2,1,2,1,...
    cost1 = 0
    cost2 = 0

    for i, length in enumerate(s):
        if i % 2 == 0:
            cost1 += length
            cost2 += 2 * length
        else:
            cost1 += 2 * length
            cost2 += length

    first = 1 if cost1 <= cost2 else 2

    ans = []
    value = first

    for length in s:
        ans.extend([str(value)] * length)
        value = 3 - value

    sys.stdout.write(" ".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính cả hai tổng số có thể có mà không cần xây dựng chuỗi đầy đủ. Đối với chỉ mục dựa trên số 0 chẵn, mẫu đầu tiên sử dụng giá trị (1) và mẫu thứ hai sử dụng (2); đối với chỉ số lẻ, vai trò của chúng bị đảo ngược. 

Việc so sánh sử dụng`<=`, vì vậy các mối quan hệ luôn chọn mẫu đầu tiên. Mọi mức tối thiểu đều được chấp nhận, do đó quy tắc ràng buộc cụ thể không ảnh hưởng đến tính chính xác. 

Vòng lặp thứ hai tái tạo lại chính xác mẫu đã chọn. biểu hiện`3 - value`công tắc`1`ĐẾN`2`Và`2`ĐẾN`1`, tránh một điều kiện riêng biệt. 

các`ans`list chứa các chuỗi vì việc nối các chuỗi ở cuối hiệu quả hơn đáng kể so với việc nối nhiều lần với một chuỗi Python lớn. Kích thước của nó được giới hạn bởi tổng chiều dài chữ ký, tối đa là (10^6). 

Số nguyên Python không bị tràn và tổng lớn nhất có thể tối đa là (2\cdot10^6) đối với giải pháp hai giá trị được xây dựng, vì vậy số học số nguyên ở đây không quan trọng. Việc sử dụng bộ nhớ chiếm ưu thế là chính đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chữ ký mẫu là (s=[1,2,2]). Hai mô hình có thể có chi phí sau đây. 

| Chặn | Chiều dài | Giá trị mẫu 1 | Giá mẫu 1 | Giá trị mẫu 2 | Giá mẫu 2 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 2 | 2 | 
| 2 | 2 | 2 | 4 | 1 | 2 | 
| 3 | 2 | 1 | 2 | 2 | 4 | 
| Tổng cộng | 5 | | 7 | | 8 | 

Mẫu 1 rẻ hơn nên giá trị khối là (1,2,1). Mở rộng chúng theo chữ ký mang lại`1 2 2 1 1`, nhưng mẫu được cung cấp sử dụng chữ ký`1 2 3`, mang lại`1 2 2 1 1 1`. Đối với đầu vào mẫu thực tế (s=[1,2,3]), phép tính là: 

| Chặn | Chiều dài | Giá trị mẫu 1 | Giá mẫu 1 | Giá trị mẫu 2 | Giá mẫu 2 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 2 | 2 | 
| 2 | 2 | 2 | 4 | 1 | 2 | 
| 3 | 3 | 1 | 3 | 2 | 6 | 
| Tổng cộng | 6 | | 8 | | 10 | 

Mẫu đầu tiên thắng, đưa ra`1 2 2 1 1 1`. Các khối của nó có độ dài (1,2,3), khớp chính xác với chữ ký. 

### Mẫu 2 

Hãy xem xét (s=[1,3]). Ví dụ này mắc lỗi luôn bắt đầu bằng giá trị (1). 

| Chặn | Chiều dài | Giá trị mẫu 1 | Giá mẫu 1 | Giá trị mẫu 2 | Giá mẫu 2 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 2 | 2 | 
| 2 | 3 | 2 | 6 | 1 | 3 | 
| Tổng cộng | 4 | | 7 | | 5 | 

Mẫu thứ hai tốt hơn nên câu trả lời là`2 1 1 1`. Khối đầu tiên của nó có một`2`và khối thứ hai của nó có ba`1`S. Tổng là (5), nhỏ hơn (7) thu được khi bắt đầu bằng`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+\sum s_i)) | Một lượt tính toán hai chi phí và một lượt mở rộng đầu ra | 
| Không gian | (O(\sum s_i)) | Câu trả lời rõ ràng chứa chính xác (\sum s_i) số nguyên | 

Các ràng buộc đưa ra (n\le10^5) và (\sum s_i\le10^6). Thuật toán chỉ thực hiện một lượng số học không đổi cho mỗi phần tử chữ ký và sau đó ghi tối đa (10^6) giá trị đầu ra, do đó nó phù hợp thoải mái trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    n = int(input())
    s = list(map(int, input().split()))

    cost1 = 0
    cost2 = 0

    for i, length in enumerate(s):
        if i % 2 == 0:
            cost1 += length
            cost2 += 2 * length
        else:
            cost1 += 2 * length
            cost2 += length

    first = 1 if cost1 <= cost2 else 2

    ans = []
    value = first

    for length in s:
        ans.extend([str(value)] * length)
        value = 3 - value

    sys.stdout.write(" ".join(ans))

input = sys.stdin.readline

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    stream = io.StringIO(inp)
    sys.stdin = stream
    input = stream.readline

    old_stdout = sys.stdout
    output = io.StringIO()
    sys.stdout = output

    try:
        solve()
        return output.getvalue()
    finally:
        sys.stdin = old_stdin
        input = old_input
        sys.stdout = old_stdout

def signature(arr):
    if not arr:
        return []

    result = []
    current = arr[0]
    count = 1

    for x in arr[1:]:
        if x == current:
            count += 1
        else:
            result.append(count)
            current = x
            count = 1

    result.append(count)
    return result

# Provided sample
assert run("3\n1 2 3\n") == "1 2 2 1 1 1", "sample 1"

# Minimum-size input
assert run("1\n1\n") == "1", "minimum size"

# A greedy-start counterexample
assert run("2\n1 3\n") == "2 1 1 1", "starting with 1 is not always optimal"

# All blocks have the same length
assert run("3\n2 2 2\n") == "1 1 2 2 1 1", "equal block lengths"

# Boundary case with a large single block
assert run("1\n1000000\n") == " ".join(["1"] * 1000000), "maximum output length"

# Maximum number of blocks
large = " ".join(["1"] * 100000)
out = run("100000\n" + large + "\n")
arr = list(map(int, out.split()))

assert len(arr) == 100000
assert signature(arr) == [1] * 100000
assert sum(arr) == 150000
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Xử lý đầu vào và khối đơn tối thiểu | 
|`2 / 1 3`|`2 1 1 1`| Chọn đúng giá trị bắt đầu tốt hơn | 
|`3 / 2 2 2`|`1 1 2 2 1 1`| Chiều dài khối lân cận xen kẽ và bằng nhau | 
|`1 / 1000000`| Một triệu`1`s | Kích thước đầu ra tối đa và logic mở rộng | 
|`100000 / 1 1 ... 1`| luân phiên`1 2 1 2 ...`| Số khối tối đa và xử lý ranh giới | 

## Vỏ cạnh 

Đối với một khối duy nhất, chẳng hạn như`1 / 5`, không có hạn chế kề cận nào cả. Thuật toán so sánh hai mẫu danh nghĩa, nhưng mẫu 1 có chi phí (5) trong khi mẫu 2 có chi phí (10) nên nó chọn giá trị`1`và đầu ra`1 1 1 1 1`. Chữ ký vẫn còn`[5]`. 

Đối với ví dụ phản chứng tham lam địa phương`2 / 1 3`, mẫu đầu tiên có giá trị (1,2) và chi phí (1+6=7). Mẫu thứ hai có các giá trị (2,1) và giá trị (2+3=5), do đó thuật toán chọn mẫu thứ hai và đưa ra kết quả`2 1 1 1`. Điều này chứng tỏ tại sao khối đầu tiên không thể được gán giá trị nhỏ nhất có thể mà không xem xét các khối lân cận của nó. 

Đối với độ dài khối lặp đi lặp lại,`3 / 2 2 2`, hai mẫu có chi phí (2+4+2=8) và (4+2+4=10). Mẫu đầu tiên được chọn, đưa ra`1 1 2 2 1 1`. Khối ở giữa khác với cả hai khối lân cận, trong khi khối thứ nhất và khối thứ ba có thể sử dụng cùng một giá trị một cách an toàn vì chúng không liền kề nhau. 

Đối với chiều dài đầu ra tối đa,`1 / 1000000`, câu trả lời bao gồm một triệu bản sao của`1`. Thuật toán không bao giờ tạo ra các giá trị thay thế không cần thiết và kích thước đầu ra chính xác là mức tối đa cho phép. Vòng lặp mở rộng thực hiện một thao tác nối thêm cho mỗi phần tử đầu ra, do đó công việc của nó là tuyến tính ở kích thước đầu ra không thể tránh khỏi. 

Để có số khối tối đa, lấy (100000) khối có độ dài (1). Mỗi khối lân cận phải khác nhau, do đó trình tự tối ưu xen kẽ giữa`1`Và`2`. Có hai chuỗi xen kẽ có thể xảy ra và vì có số khối chẵn nên mẫu bắt đầu bằng`1`có tổng số tiền (50000\cdot1+50000\cdot2=150000). Thuật toán tính toán chính xác lựa chọn này mà không cần xử lý đặc biệt nào đối với ranh giới giữa khối đầu tiên và khối cuối cùng, vì chỉ các khối liên tiếp bị ràng buộc.
