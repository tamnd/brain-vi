---
title: "CF 2124A - Xóa loạn trí"
description: "Chúng ta được cung cấp một mảng và có thể xóa bất kỳ số lượng phần tử nào trong khi vẫn giữ nguyên thứ tự tương đối của các phần tử còn lại. Sau khi xóa, chuỗi còn lại phải không trống."
date: "2026-06-08T03:31:30+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2124
codeforces_index: "A"
codeforces_contest_name: "EPIC Institute of Technology Round Summer 2025 (Codeforces Round 1036, Div. 1 + Div. 2)"
rating: 800
weight: 2124
solve_time_s: 129
verified: false
draft: false
---

[CF 2124A - Xóa sai trật tự](https://codeforces.com/problemset/problem/2124/A) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và có thể xóa bất kỳ số lượng phần tử nào trong khi vẫn giữ nguyên thứ tự tương đối của các phần tử còn lại. Sau khi xóa, chuỗi còn lại phải không trống. 

Một dãy được gọi là xáo trộn nếu sau khi sắp xếp một bản sao của nó theo thứ tự không giảm, mọi vị trí đều thay đổi giá trị. Nói cách khác, nếu mảng còn lại là`b`và phiên bản được sắp xếp của nó là`c`, thì chúng ta cần`b[i] != c[i]`cho mọi chỉ số. 

Nhiệm vụ không phải là tối đa hóa hay giảm thiểu bất cứ điều gì. Chúng ta chỉ cần xác định liệu một số dãy con của mảng ban đầu có tạo thành một trật tự hay không và nếu có thì xuất ra bất kỳ dãy con nào như vậy. 

Những hạn chế là rất nhỏ. Mỗi test case có tối đa 100 phần tử và có tối đa 100 test case. Ngay cả các thuật toán có độ phức tạp bậc hai hoặc bậc ba cũng hoàn toàn an toàn. Điều này có nghĩa là chúng ta nên tập trung vào việc tìm kiếm một quan sát cấu trúc đơn giản hơn là lo lắng về việc tối ưu hóa nặng nề. 

Khó khăn chính là hiểu được loại chuỗi nào có thể trở thành một sự xáo trộn. 

Hãy xem xét mảng`[1, 1, 1]`. Mọi dãy con không trống chỉ gồm một dãy. Sau khi sắp xếp, nó không thay đổi nên không có sự xáo trộn. 

Coi như`[2, 1]`. Toàn bộ mảng trở thành`[1, 2]`sau khi sắp xếp. Cả hai vị trí đều thay đổi, vì vậy câu trả lời là CÓ. 

Một trường hợp tế nhị là`[2, 2, 3]`. Thứ tự sắp xếp đã có rồi`[2, 2, 3]`. Xóa`3`cho`[2, 2]`, vẫn không thay đổi sau khi sắp xếp. Xóa một trong những`2`s cho`[2, 3]`, thứ tự sắp xếp của nó cũng là`[2, 3]`. Mọi dãy con không trống đều thất bại nên câu trả lời là KHÔNG. 

Một cách tiếp cận bất cẩn có thể cố gắng sử dụng toàn bộ mảng bất cứ khi nào nó chưa được sắp xếp. Điều đó không thành công ở các đầu vào như`[3, 1, 2]`. Toàn bộ mảng không bị xáo trộn vì sau khi sắp xếp chúng ta nhận được`[1, 2, 3]`và phần tử ở giữa trở thành`2`, khớp với vị trí được sắp xếp của nó. Chúng ta phải xác minh mọi vị trí chứ không chỉ kiểm tra xem mảng có khác với phiên bản được sắp xếp của nó hay không. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là kiểm tra mọi dãy con không rỗng. Đối với mỗi chuỗi con, hãy sắp xếp một bản sao và kiểm tra xem mọi vị trí có khác nhau không. Điều này rõ ràng là đúng vì nó kiểm tra mọi câu trả lời có thể. 

Vấn đề là một mảng có độ dài`n`có`2^n - 1`dãy con không trống. Ngay cả đối với`n = 100`, điều này là hoàn toàn không thể. 

Quan sát quan trọng đến từ việc xem xét phần tử nhỏ nhất. 

Cho phép`mn`là giá trị nhỏ nhất trong chuỗi hiện tại. Sau khi sắp xếp, tất cả các lần xuất hiện của`mn`phải xuất hiện ngay từ đầu. Nếu một vị trí đã chứa`mn`và vẫn nằm trong số các vị trí đầu tiên sau khi sắp xếp, vị trí đó không bao giờ khác với giá trị được sắp xếp của nó. 

Giả sử chúng ta chọn tất cả các phần tử của một dãy con nào đó và sắp xếp chúng. Nếu một phần tử đã ở cùng vị trí với thứ tự được sắp xếp thì điều kiện lệch trật tự sẽ không thành công. 

Một cách đơn giản hơn nhiều để suy nghĩ về vấn đề là sắp xếp mảng ban đầu và so sánh nó với thứ tự ban đầu. Mỗi chỉ số nơi`a[i] != sorted_a[i]`là "tốt", bởi vì phần tử đó đã khác với thứ mà chuỗi được sắp xếp sẽ đặt ở đó. 

Mỗi chỉ số nơi`a[i] == sorted_a[i]`là “xấu”, vì nếu chúng ta giữ quan điểm đó thì ngay lập tức vi phạm điều kiện loạn trí. 

Điều này gợi ý việc lấy chính xác các vị trí mà mảng ban đầu khác với phiên bản được sắp xếp của nó. Hãy để những phần tử đó tạo thành một dãy con`d`. 

Đối với mỗi vị trí được giữ lại, giá trị ban đầu và giá trị được sắp xếp là khác nhau. Multiset được sắp xếp của`d`chính xác là các giá trị tương ứng được lấy từ mảng đã sắp xếp. Do đó mọi vị trí của`d`khác với vị trí của nó trong phiên bản được sắp xếp của nó, làm cho`d`một sự loạn trí hợp lệ. 

Nếu không có vị trí nào như vậy tồn tại thì mảng đã bằng phiên bản được sắp xếp của nó. Trong trường hợp đó, mọi phần tử đều khớp với vị trí được sắp xếp của nó và không thể hình thành chuỗi con hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng`a`. 
2. Tạo bản sao đã được sắp xếp`b = sorted(a)`. 
3. Tìm mọi chỉ mục`i`như vậy`a[i] != b[i]`. 

Đây chính xác là những quan điểm đã không đồng ý với sự sắp xếp đã được sắp xếp. 
4. Thu thập các giá trị tương ứng`a[i]`vào một mảng mới`ans`. 
5. Nếu`ans`trống, in`NO`. 

Điều này có nghĩa là mảng ban đầu đã giống hệt với phiên bản được sắp xếp của nó, do đó không tồn tại chuỗi xáo trộn nào. 
6. Nếu không thì in`YES`, chiều dài của`ans`, và các phần tử của`ans`. 

### Tại sao nó hoạt động 

hãy để`S`là tập hợp các chỉ số trong đó`a[i] != b[i]`, Ở đâu`b`là phiên bản được sắp xếp của`a`. 

Chúng tôi xây dựng`ans`sử dụng chính xác những vị trí đó. 

Phiên bản được sắp xếp của`ans`thu được bằng cách lấy các phần tử`b[i]`cho cùng chỉ số`i ∈ S`. Điều này xuất phát từ thực tế là`b`là sự sắp xếp được sắp xếp toàn cầu của cùng một multiset. 

Với mỗi vị trí được giữ lại, chúng ta có`a[i] != b[i]`bằng cách xây dựng. Như vậy mỗi vị trí của`ans`khác với vị trí tương ứng trong phiên bản được sắp xếp của nó. 

Kể từ đây`ans`là một sự loạn trí. 

Nếu như`S`thì trống rỗng`a = b`, nghĩa là toàn bộ mảng đã được sắp xếp. Mọi dãy con của mảng đã được sắp xếp cũng được sắp xếp, do đó một số vị trí sẽ luôn trùng với phiên bản đã sắp xếp của nó. Không có sự loạn trí nào có thể tồn tại. 

Điều này chứng tỏ tính đúng đắn và đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        b = sorted(a)

        ans = []
        for i in range(n):
            if a[i] != b[i]:
                ans.append(a[i])

        if not ans:
            print("NO")
        else:
            print("YES")
            print(len(ans))
            print(*ans)

solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng. Bản sao được sắp xếp thể hiện sự sắp xếp mục tiêu dựa vào đó điều kiện lệch lạc được đo lường. 

Vòng lặp thu thập chính xác các chỉ số trong đó giá trị ban đầu khác với giá trị trong mảng được sắp xếp. Đây là những vị trí duy nhất có thể tham gia vào một câu trả lời hợp lệ. 

Nếu không có vị trí nào như vậy tồn tại thì mảng đã được sắp xếp và không tồn tại dãy con hợp lệ. 

Mặt khác, các phần tử được thu thập sẽ tạo thành một sự xáo trộn hợp lệ. Việc thực hiện trực tiếp xuất ra chúng. 

Sai lầm phổ biến nhất là cố gắng giữ các chỉ số ở nơi`a[i] == b[i]`. Những vị trí như vậy ngay lập tức vi phạm yêu cầu sắp xếp lại vì giá trị đã khớp với vị trí được sắp xếp của nó. 

Một lỗi dễ mắc phải khác là chỉ kiểm tra xem toàn bộ mảng có khác với phiên bản được sắp xếp của nó hay không. Một sự loạn trí đòi hỏi mọi vị trí đều phải khác nhau chứ không phải chỉ có ít nhất một vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
4 5 5 2 4
```Bản sao được sắp xếp:```
2 4 4 5 5
```| Chỉ mục | một [tôi] | đã sắp xếp[i] | Giữ? | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 2 | Có | [4] | 
| 1 | 5 | 4 | Có | [4, 5] | 
| 2 | 5 | 4 | Có | [4, 5, 5] | 
| 3 | 2 | 5 | Có | [4, 5, 5, 2] | 
| 4 | 4 | 5 | Có | [4, 5, 5, 2, 4] | 

Chuỗi kết quả là toàn bộ mảng. Phiên bản được sắp xếp của nó là`[2,4,4,5,5]`, và mọi vị trí đều khác nhau. 

### Ví dụ 2 

đầu vào:```
3
2 2 3
```Bản sao được sắp xếp:```
2 2 3
```| Chỉ mục | một [tôi] | đã sắp xếp[i] | Giữ? | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | Không | [] | 
| 1 | 2 | 2 | Không | [] | 
| 2 | 3 | 3 | Không | [] | 

Không có vị trí nào khác với mảng đã được sắp xếp, vì vậy`ans`vẫn còn trống. 

Thuật toán in`NO`. Điều này chứng tỏ trường hợp mảng ban đầu đã được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Sắp xếp mảng sao chép và trả lời | 

Với`n ≤ 100`, sự phức tạp này thấp hơn nhiều so với giới hạn. Ngay cả trên tất cả các trường hợp thử nghiệm, khối lượng công việc vẫn rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        b = sorted(a)

        ans = [a[i] for i in range(n) if a[i] != b[i]]

        if not ans:
            print("NO")
        else:
            print("YES")
            print(len(ans))
            print(*ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# sample 1
assert run(
"""3
3
2 2 3
5
4 5 5 2 4
1
1
"""
) == (
"""NO
YES
5
4 5 5 2 4
NO
"""
)

# minimum size
assert run(
"""1
1
1
"""
) == (
"""NO
"""
)

# simple derangement
assert run(
"""1
2
2 1
"""
) == (
"""YES
2
2 1
"""
)

# all equal
assert run(
"""1
5
3 3 3 3 3
"""
) == (
"""NO
"""
)

# already sorted
assert run(
"""1
4
1 2 3 4
"""
) == (
"""NO
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1]`| KHÔNG | Mảng nhỏ nhất có thể | 
|`[2,1]`| CÓ | Trường hợp tích cực nhỏ nhất | 
|`[3,3,3,3,3]`| KHÔNG | Tất cả các giá trị bằng nhau | 
|`[1,2,3,4]`| KHÔNG | Đã sắp xếp mảng | 
|`[4,5,5,2,4]`| CÓ | Trường hợp hỗn hợp điển hình có trùng lặp | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
1
```Bản sao được sắp xếp giống hệt nhau. Không có chỉ số nào thỏa mãn`a[i] != b[i]`, vì vậy mảng câu trả lời trống và thuật toán in ra`NO`. Một dãy khác trống không thể tồn tại vì dãy con duy nhất có sẵn là`[1]`. 

Coi như:```
1
5
3 3 3 3 3
```Một lần nữa bản sao được sắp xếp giống hệt nhau. Mỗi dãy con bao gồm toàn bộ số ba và việc sắp xếp không bao giờ thay đổi bất cứ điều gì. Thuật toán không tìm thấy chính xác vị trí và bản in khác nhau`NO`. 

Coi như:```
1
2
2 1
```Bản sao được sắp xếp là`[1,2]`. Cả hai vị trí đều khác nhau nên thuật toán giữ nguyên cả hai phần tử. Mảng kết quả`[2,1]`đã sắp xếp phiên bản`[1,2]`, và mọi vị trí đều khác nhau. Thuật toán in`YES`. 

Coi như:```
1
3
3 1 2
```Bản sao được sắp xếp là`[1,2,3]`. Cả ba vị trí đều khác nhau nên toàn bộ mảng được chọn. So sánh`[3,1,2]`với`[1,2,3]`, mọi vị trí đều khác nhau, làm cho nó trở thành một sự xáo trộn hợp lệ. Thuật toán xuất ra toàn bộ mảng.
