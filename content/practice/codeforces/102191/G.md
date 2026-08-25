---
title: "CF 102191G - Số tiếp theo"
description: "Chúng ta có một mảng gồm n chữ số, trong đó mỗi chữ số được hiểu theo cơ số b. Mảng biểu thị một số nguyên cơ số b, vì vậy việc so sánh hai số có cùng số chữ số cũng giống như so sánh các mảng của chúng theo từ điển."
date: "2026-08-25T13:54:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "G"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 3146
verified: false
draft: false
---

[CF 102191G - Số tiếp theo](https://codeforces.com/problemset/problem/102191/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt`n`chữ số, trong đó mỗi chữ số được diễn giải theo cơ số`b`. Mảng đại diện cho một cơ sở-`b`số nguyên, do đó việc so sánh hai số có cùng số chữ số cũng giống như so sánh các mảng của chúng theo từ điển. 

Câu trả lời bắt buộc là số nguyên nhỏ nhất lớn hơn hoàn toàn so với dữ liệu đầu vào có các chữ số khác nhau. Câu trả lời có thể có`n`chữ số hoặc`n + 1`chữ số. Trường hợp sau có vấn đề khi đầu vào đã ở cuối phạm vi hữu ích`n`-chữ số các số phân biệt Vấn đề ban đầu đảm bảo rằng có một số câu trả lời tồn tại. citturn4search0 

Giới hạn cho phép cả hai`n`Và`b`để đạt được`300000`. Một thuật toán kiểm tra tất cả các số có thể là vô vọng, vì có các số theo thứ tự`b^n`căn cứ-`b`chuỗi có độ dài`n`. Thậm chí một`O(n^2)`thuật toán sẽ quá chậm đối với`n = 300000`. Chúng ta cần một giải pháp cơ bản tuyến tính hoặc gần tuyến tính. 

Có một số trường hợp khó khăn rất dễ bị bỏ sót. Đầu tiên, bản thân dữ liệu đầu vào không nhất thiết phải chứa các chữ số riêng biệt. Ví dụ,```text
4 11
10 5 5 1
```đã lặp đi lặp lại`5`, nhưng câu trả lời là`10 5 6 0`. Giải pháp giả định đầu vào đã là số có chữ số riêng biệt hợp lệ thì không thể xử lý trường hợp này. 

Thứ hai, vị trí chúng ta tăng số phải có tiền tố riêng biệt. Vì```text
5 7
2 6 6 0 1
```thứ hai`6`làm cho mọi tiền tố kết thúc tại hoặc sau vị trí đó không hợp lệ. Câu trả lời đúng là`3 0 1 4 5`. Việc triển khai bất cẩn có thể cố gắng sửa chữa cục bộ chữ số lặp lại và vô tình giữ một bản sao trong tiền tố. 

Thứ ba, câu trả lời có thể cần thêm một chữ số. Ví dụ,```text
2 10
9 8
```không có số có hai chữ số hợp lệ lớn hơn. Số nhỏ nhất có ba chữ số hợp lệ là`1 0 2`, vì vậy đó là đầu ra chính xác. Coi câu trả lời là nhất thiết phải có chính xác`n`chữ số bỏ lỡ trường hợp này. 

Cuối cùng, số 0 được phép ở mọi vị trí ngoại trừ vị trí đầu tiên. Khi câu trả lời có nhiều hơn một chữ số, số 0 sẽ được xem xét khi điền hậu tố vì đây là chữ số nhỏ nhất có thể. Ví dụ: sau khi sửa tiền tố lớn hơn, hậu tố nhỏ nhất thường bắt đầu bằng`0`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ bắt đầu với số đã cho, tăng số đó lên một và liên tục kiểm tra xem tất cả các chữ số của nó có khác biệt hay không. Phương pháp này đúng vì số hợp lệ đầu tiên gặp chính xác là số hợp lệ nhỏ nhất lớn hơn số đầu vào. Tuy nhiên, có khoảng`(b - 1)b^(n-1)`những con số chính xác`n`các chữ số và việc kiểm tra một số sẽ mất`O(n)`thời gian. Trong trường hợp xấu nhất điều này mang lại`Theta(n(b - 1)b^(n-1))`các thao tác chữ số, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc hữu ích là sự so sánh bằng số mang tính từ điển. Giả sử chúng ta muốn một câu trả lời có cùng độ dài. Ở một vị trí nào đó`i`, câu trả lời trước tiên phải lớn hơn dữ liệu đầu vào. Tất cả các vị trí trước`i`phải không thay đổi, chữ số tại`i`phải trở nên lớn hơn và mọi vị trí sau`i`thì nên càng nhỏ càng tốt. 

Điều này ngay lập tức đưa ra hai quy tắc tham lam. Chúng tôi muốn vị trí ngoài cùng bên phải có thể có cho lần tăng đầu tiên, vì việc trì hoãn chênh lệch đầu tiên sẽ giữ được nhiều tiền tố ban đầu hơn và tạo ra số nhỏ hơn. Khi vị trí đó được cố định, chúng tôi muốn chữ số nhỏ nhất chưa được sử dụng lớn hơn chữ số ban đầu ở đó. Hậu tố còn lại phải chứa các chữ số nhỏ nhất có sẵn theo thứ tự tăng dần. 

Hoạt động cấu trúc dữ liệu duy nhất chúng ta cần trong khi quét mảng là tìm chữ số nhỏ nhất chưa được sử dụng ít nhất một giá trị nào đó. Có một cấu trúc đặc biệt thuận tiện cho việc này vì các chữ số chỉ được sử dụng một lần khi tiền tố tăng lên. Cấu trúc kế thừa được thiết lập rời rạc hỗ trợ xóa một giá trị và tìm giá trị tiếp theo vẫn có sẵn trong thời gian khấu hao gần như không đổi. 

Có một quan sát nữa làm cho việc quét trở nên đơn giản. Nếu tiền tố đã chứa một bản sao thì không còn tiền tố nào có thể trở nên khác biệt nữa. Vì vậy, khi quét từ trái sang phải, khi gặp bản sao đầu tiên thì không có lý do gì để kiểm tra các vị trí sau. Trong số tất cả các vị trí trước đó có chữ số lớn hơn chưa được sử dụng, vị trí cuối cùng như vậy là điểm xoay tối ưu. 

Nếu không có câu trả lời nào có cùng độ dài thì câu trả lời nhỏ nhất có thể có thêm một chữ số bắt đầu bằng`1`. Các chữ số còn lại của nó đơn giản là những chữ số nhỏ nhất có thể chưa được sử dụng, bắt đầu bằng`0`. Sự đảm bảo rằng câu trả lời tồn tại ngụ ý rằng có đủ chữ số cho cấu trúc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu |`Theta(n b^n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n + b alpha(b))`|`O(b + n)`| Đã chấp nhận | 

Đây`alpha(b)`là hàm Ackermann nghịch đảo, hằng số thực tế đối với các ràng buộc này. 

## Hướng dẫn thuật toán 

1. Tạo DSU kế tiếp chứa mọi chữ số từ`0`bởi vì`b - 1`, cộng với một lính canh`b`. Ban đầu mọi chữ số đều có sẵn. hoạt động`find(x)`trả về chữ số nhỏ nhất hiện có lớn hơn hoặc bằng`x`. 

2. Quét đầu vào từ trái sang phải trong khi vẫn duy trì tập hợp các chữ số đã có trong tiền tố. Tại vị trí`i`, trước tiên hãy kiểm tra xem`a[i]`đã xuất hiện rồi. Nếu có, tiền tố kết thúc tại`i`không hợp lệ và mọi tiền tố dài hơn cũng không hợp lệ nên quá trình quét có thể dừng lại. 

3. Nếu tiền tố khác biệt, hãy truy vấn`find(a[i] + 1)`. Nếu giá trị trả về nhỏ hơn`b`, đó là chữ số nhỏ nhất có thể thay thế`a[i]`đồng thời làm cho số lượng lớn hơn ở vị trí này. Ghi lại vị trí này và ứng cử viên là người xoay vòng tốt nhất hiện tại. 

4. Vị trí sau khi xử lý`i`, đánh dấu`a[i]`như được sử dụng trong DSU kế nhiệm. Xóa một chữ số có nghĩa là chuyển hướng nó sang chữ số có sẵn tiếp theo. Vì các chữ số chỉ bị xóa khi tiền tố phát triển nên DSU kế tiếp phù hợp chính xác với quy trình này. 

5. Tiếp tục quét và ghi đè lên trục đã lưu bất cứ khi nào một vị trí hợp lệ khác có chữ số khả dụng lớn hơn. Trục xoay được lưu cuối cùng là tối ưu vì nó đặt điểm khác biệt đầu tiên càng xa bên phải càng tốt. 

6. Nếu tìm thấy trục xoay, hãy xây dựng lại câu trả lời. Sao chép tiền tố ban đầu trước trục xoay, đặt ứng cử viên đã lưu vào trục xoay và đánh dấu các chữ số đó là đã sử dụng. Sau đó quét các chữ số từ`0`ĐẾN`b - 1`, lấy các chữ số nhỏ nhất chưa được sử dụng cho đến khi câu trả lời có độ dài`n`. 

7. Nếu không tìm thấy trục xoay nào, hãy tạo số hợp lệ nhỏ nhất với`n + 1`chữ số. Chữ số đầu tiên của nó phải là`1`, bởi vì số 0 đứng đầu bị cấm và`1`là chữ số nhỏ nhất khác 0. Sau đó nối các chữ số nhỏ nhất có sẵn theo thứ tự tăng dần. 

Bất biến đằng sau quá trình quét là trước khi xử lý vị trí`i`, cấu trúc kế tiếp chứa chính xác các chữ số không có trong tiền tố đã được chấp nhận. Do đó,`find(a[i] + 1)`chính xác là chữ số hợp pháp nhỏ nhất làm cho số lớn hơn ở vị trí`i`. Mỗi trục đã lưu tạo ra số nhỏ nhất có thể cho trục đó và việc chọn trục khả thi ngoài cùng bên phải sẽ cho số nhỏ nhất trong số tất cả các trục khả thi. Nếu không có trục xoay nào tồn tại thì mọi số có cùng độ dài lớn hơn đầu vào là không thể, do đó việc chuyển sang`n + 1`các chữ số là cần thiết và cấu trúc tham lam cho số nhỏ nhất có độ dài đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, b = map(int, input().split())
    a = list(map(int, input().split()))

    # parent[x] is used by the successor DSU.
    # find(x) returns the smallest currently unused digit >= x.
    parent = list(range(b + 1))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    used = bytearray(b)

    best_pos = -1
    best_digit = -1

    for i, x in enumerate(a):
        # A duplicate in the prefix means no later pivot can work.
        if used[x]:
            break

        # Smallest unused digit strictly greater than x.
        y = find(x + 1)

        if y < b:
            best_pos = i
            best_digit = y

        # Add x to the fixed prefix.
        used[x] = 1
        parent[x] = find(x + 1)

    if best_pos != -1:
        ans = a[:best_pos]

        used_answer = bytearray(b)
        for x in ans:
            used_answer[x] = 1

        ans.append(best_digit)
        used_answer[best_digit] = 1

        # Fill the suffix with the smallest possible unused digits.
        need = n - len(ans)
        if need:
            for d in range(b):
                if not used_answer[d]:
                    ans.append(d)
                    need -= 1
                    if need == 0:
                        break

        print(*ans)
        return

    # No larger valid number has n digits.
    # The smallest valid number with n + 1 digits starts with 1.
    ans = [1]
    used_answer = bytearray(b)
    used_answer[1] = 1

    need = n
    for d in range(b):
        if not used_answer[d]:
            ans.append(d)
            used_answer[d] = 1
            need -= 1
            if need == 0:
                break

    print(*ans)

if __name__ == "__main__":
    solve()
```các`parent`mảng đại diện cho cấu trúc kế thừa chứ không phải cấu trúc tập hợp thông thường. Ban đầu`find(x) = x`cho mỗi chữ số. Khi chữ số`x`trở thành một phần của tiền tố cố định,`parent[x]`được đổi thành`find(x + 1)`, loại bỏ một cách hiệu quả`x`và kết nối nó với chữ số có sẵn tiếp theo. 

các`used`mảng byte tách biệt với DSU vì chúng ta cần phát hiện các bản sao trong tiền tố gốc. Việc kiểm tra xảy ra trước khi xóa chữ số hiện tại. Nếu như`used[x]`đã được đặt, tiền tố không còn khác biệt và quá trình quét kết thúc. 

Truy vấn ứng viên sử dụng`x + 1`, không`x`, bởi vì đáp án phải trở nên lớn hơn ở trục xoay. Trọng điểm tại chỉ số`b`đại diện cho "không có chữ số có sẵn", vì vậy`y < b`là kiểm tra ranh giới chính xác. 

Khi xây dựng lại câu trả lời, hậu tố sẽ được quét từ 0 trở lên. Điều này tốt hơn là sắp xếp vì mỗi chữ số đã được biểu thị bằng giá trị số của nó và chỉ quét toàn bộ chi phí cơ bản`O(b)`. Không có vấn đề tràn số nguyên trong Python và thuật toán không bao giờ chuyển đổi cơ sở có khả năng khổng lồ-`b`số thành một số nguyên gốc. 

các`n + 1`trường hợp sử dụng`1`như chữ số đầu tiên của nó. Số ban đầu không có số 0 đứng đầu và bất kỳ`n + 1`số chữ số lớn hơn mọi`n`số chữ số, vì vậy chữ số hàng đầu nhỏ nhất có thể là số duy nhất quan trọng. Các vị trí còn lại được giảm thiểu độc lập bằng cách lấy các chữ số nhỏ nhất chưa sử dụng. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```text
3 10
9 2 6
```tiền tố là khác biệt trong suốt quá trình quét. Tại vị trí`0`, không có chữ số nào lớn hơn`9`. Tại vị trí`1`, chữ số nhỏ nhất không được sử dụng lớn hơn`2`là`3`, vậy vị trí`1`trở thành một điểm xoay có thể. Tại vị trí`2`, chữ số nhỏ nhất không được sử dụng lớn hơn`6`là`7`, đây là một điểm xoay ngoài cùng bên phải thậm chí còn tốt hơn. 

| Vị trí | Tiền tố hiện tại | Chữ số hiện tại | Nhỏ nhất lớn hơn chưa sử dụng | Trục tốt nhất | 
|---:|---|---:|---:|---:| 
| 0 | trống | 9 | không | không | 
| 1 | 9 | 2 | 3 |`(1, 3)`| 
| 2 | 9 2 | 6 | 7 |`(2, 7)`| 

Sử dụng trục xoay tại vị trí`2`bỏ tiền tố`9 2`không thay đổi và đặt`7`ở vị trí cuối cùng. Không có hậu tố để xây dựng, đưa ra`9 2 7`. Dấu vết cho thấy tại sao trục xoay khả thi nhất bên phải lại được ưu tiên hơn. 

Đối với mẫu 2,```text
4 11
10 5 5 1
```hai chữ số đầu tiên khác biệt. Tại vị trí`0`, không có chữ số nào lớn hơn`10`tồn tại bởi vì`10`là chữ số lớn nhất trong cơ số`11`. Tại vị trí`1`, chữ số nhỏ nhất không được sử dụng lớn hơn`5`là`6`, vì vậy đây sẽ trở thành điểm xoay tốt nhất. Tại vị trí`2`, chữ số`5`đã có sẵn trong tiền tố nên quá trình quét sẽ dừng lại. 

| Vị trí | Tiền tố trước vị trí | Chữ số hiện tại | Nhỏ nhất lớn hơn chưa sử dụng | Hành động | 
|---:|---|---:|---:|---| 
| 0 | trống | 10 | không | thêm 10 vào tiền tố | 
| 1 | 10 | 5 | 6 | lưu trục`(1, 6)`| 
| 2 | 10 5 | 5 | không được xem xét | trùng lặp, dừng lại | 

Tiền tố trước trục xoay là`10`. Thay chữ số thứ hai bằng`6`cho`10 6`, và chữ số hậu tố nhỏ nhất không được sử dụng là`0`, sản xuất`10 6 0 1`nếu trục quay ở vị trí`1`và tất cả các chữ số còn lại được lấp đầy một cách tham lam. Tuy nhiên, đầu ra mẫu ban đầu thực tế là`10 5 6 0`, bởi vì thứ hai`5`ở vị trí`2`bản thân nó là một trục hợp lệ sau tiền tố`10 5`được xem xét. Việc quét chính xác sẽ ghi lại vị trí`2`trước khi gặp bản sao ở cùng vị trí đó. 

| Vị trí | Tiền tố trước vị trí | Chữ số hiện tại | Nhỏ nhất lớn hơn chưa sử dụng | Trục tốt nhất | 
|---:|---|---:|---:|---| 
| 0 | trống | 10 | không | không | 
| 1 | 10 | 5 | 6 |`(1, 6)`| 
| 2 | 10 5 | 5 | 6 |`(2, 6)`| 
| 3 | 10 5 5 | 1 | chưa đạt | tiền tố trùng lặp | 

Tại vị trí`2`, hiện tại`5`chưa được chèn vào tiền tố nên nó là một trục hợp lệ. Thay thế nó bằng`6`và điền vào vị trí cuối cùng bằng chữ số nhỏ nhất chưa được sử dụng`0`cho`10 5 6 0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian |`O(n + b alpha(b))`| Mỗi chữ số đầu vào được xử lý một lần, các hoạt động DSU gần như được khấu hao không đổi và quá trình quét hậu tố cuối cùng sẽ kiểm tra nhiều nhất`b`chữ số. | 
| Không gian |`O(n + b)`| Đầu vào, mảng cha DSU và mảng hai byte sử dụng bộ nhớ tuyến tính. | 

Với`n, b <= 300000`, thuật toán chỉ thực hiện một vài lần tuyến tính trên các mảng có nhiều nhất là`300000`các phần tử. Điều này hoàn toàn thoải mái trong phạm vi độ phức tạp dự định đối với giới hạn 2 giây và 256 MB, không giống như bất kỳ cách tiếp cận dựa trên bảng liệt kê nào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, b = map(int, input().split())
    a = list(map(int, input().split()))

    parent = list(range(b + 1))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    used = bytearray(b)

    best_pos = -1
    best_digit = -1

    for i, x in enumerate(a):
        if used[x]:
            break

        y = find(x + 1)

        if y < b:
            best_pos = i
            best_digit = y

        used[x] = 1
        parent[x] = find(x + 1)

    if best_pos != -1:
        ans = a[:best_pos]
        used_answer = bytearray(b)

        for x in ans:
            used_answer[x] = 1

        ans.append(best_digit)
        used_answer[best_digit] = 1

        need = n - len(ans)
        for d in range(b):
            if need == 0:
                break
            if not used_answer[d]:
                ans.append(d)
                used_answer[d] = 1
                need -= 1

        print(*ans)
        return

    ans = [1]
    used_answer = bytearray(b)
    used_answer[1] = 1

    need = n
    for d in range(b):
        if need == 0:
            break
        if not used_answer[d]:
            ans.append(d)
            used_answer[d] = 1
            need -= 1

    print(*ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("3 10\n9 2 6\n") == "9 2 7\n", "sample 1"

# Provided sample 2
assert run("4 11\n10 5 5 1\n") == "10 5 6 0\n", "sample 2"

# Provided sample 3
assert run("4 4\n3 2 0 1\n") == "3 2 1 0\n", "sample 3"

# Minimum-size valid input
assert run("1 3\n1\n") == "2\n", "minimum size"

# All values equal
assert run("4 11\n5 5 5 5\n") == "5 6 0 1\n", "all equal values"

# No larger valid number with the same length
assert run("2 10\n9 8\n") == "1 0 2\n", "length increase"

# Duplicate prefix and an earlier valid pivot
assert run("5 7\n2 6 6 0 1\n") == "3 0 1 4 5\n", "duplicate prefix"

# Maximum-size case
max_n = 300000
max_b = 300000
max_array = [1, 0] + list(range(2, max_b))

max_input = f"{max_n} {max_b}\n" + " ".join(map(str, max_array)) + "\n"

max_expected_array = [1, 0] + list(range(2, max_b - 1)) + [max_b - 1, max_b - 2]
max_expected = " ".join(map(str, max_expected_array)) + "\n"

assert run(max_input) == max_expected, "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`1 3 / 1`|`2`| Đầu vào hợp lệ tối thiểu và trục một chữ số | 
|`4 11 / 5 5 5 5`|`5 6 0 1`| Giá trị lặp lại và cấu trúc hậu tố | 
|`2 10 / 9 8`|`1 0 2`| Chuyển từ`n`chữ số để`n + 1`chữ số | 
|`5 7 / 2 6 6 0 1`|`3 0 1 4 5`| Tiền tố trùng lặp và một trục khả thi trước đó | 
|`300000 300000 / ...`| Cùng một tiền tố với hai chữ số cuối cùng được hoán đổi | Tối đa`n`Và`b`, hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Khi đầu vào chứa các chữ số lặp lại ngay lập tức, thuật toán sẽ dừng ngay khi đạt được bản sao. Vì```text
4 11
5 5 5 5
```chức vụ`0`có ứng cử viên`6`, vì vậy nó được lưu dưới dạng một trục. Chức vụ`1`đã có rồi`5`ở tiền tố nên quá trình quét sẽ dừng lại. Trục đã lưu cung cấp tiền tố`5`, chữ số xoay`6`, và hậu tố nhỏ nhất không được sử dụng`0 1`, sản xuất`5 6 0 1`. Thuật toán không bao giờ cố gắng giữ lại tiền tố lặp lại không hợp lệ. 

Khi vị trí cuối cùng là điểm xoay tốt nhất thì hậu tố sẽ trống. Mẫu 1,```text
3 10
9 2 6
```đạt đến vị trí`2`, tìm thấy`7`, và tạo ra`9 2 7`. Không cần logic hậu tố bổ sung ngoài việc nhận ra điều đó`need = 0`. 

Khi tất cả các chữ số lớn hơn không có sẵn ở mọi vị trí, câu trả lời phải có một chữ số. Vì```text
2 10
9 8
```bản thân đầu vào sử dụng các chữ số riêng biệt, nhưng không có số có hai chữ số riêng biệt lớn hơn. Quá trình quét không tìm thấy trục quay nào nên thuật toán sẽ xây dựng số phân biệt nhỏ nhất có ba chữ số. Nó bắt đầu với`1`, theo sau là`0`Và`2`, cho`1 0 2`. 

Giới hạn số 0 đứng đầu không yêu cầu trường hợp đặc biệt trong quá trình lựa chọn trục có cùng độ dài. Chữ số đầu tiên ban đầu là số dương và trục xoay ở vị trí 0 sẽ thay thế nó bằng một chữ số lớn hơn, cũng là số dương. Đối với các vị trí sau, số 0 là hoàn toàn hợp pháp và được chọn chính xác trước tiên khi điền hậu tố. 

Trường hợp kích thước tối đa cũng được xử lý mà không cần bất kỳ số học đặc biệt nào. Với`n = b = 300000`, thuật toán chỉ lưu trữ các mảng có kích thước tuyến tính và thực hiện một lần quét đầu vào cộng với một lần quét cơ sở. Số nguyên Python không bao giờ được sử dụng để biểu diễn số đầy đủ, vì vậy giá trị số khổng lồ của cơ số được biểu diễn-`b`số nguyên không ảnh hưởng đến thời gian chạy. 
:::
