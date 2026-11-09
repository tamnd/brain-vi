---
title: "CF 1461D - Chia và tóm tắt"
description: "Chúng tôi bắt đầu với một mảng và liên tục áp dụng một quy tắc phân tách rất cụ thể. Đối với bất kỳ mảng hiện tại nào, hãy tính $$mid = leftlfloor frac{min + max}{2} rightrfloor$$ Tất cả các phần tử có giá trị tối đa nằm ở dạng giữa của một nhóm và tất cả các phần tử có giá trị lớn hơn dạng giữa của nhóm kia."
date: "2026-06-11T02:22:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "divide-and-conquer", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1461
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 689 (Div. 2, based on Zed Code Competition)"
rating: 1600
weight: 1461
solve_time_s: 126
verified: true
draft: false
---

[CF 1461D - Chia và tóm tắt](https://codeforces.com/problemset/problem/1461/D) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, phân chia và chinh phục, triển khai, sắp xếp 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng và liên tục áp dụng một quy tắc phân tách rất cụ thể. 

Đối với bất kỳ mảng hiện tại nào, hãy tính$$mid = \left\lfloor \frac{\min + \max}{2} \right\rfloor$$Tất cả các phần tử có giá trị lớn nhất`mid`tạo thành một nhóm và tất cả các phần tử có giá trị lớn hơn`mid`hình thành cái khác. Sau khi chia, chúng tôi giữ lại đúng một trong hai nhóm và loại bỏ nhóm còn lại. 

Đối với mỗi giá trị truy vấn`s`, chúng ta phải quyết định xem có tồn tại chuỗi các lựa chọn như vậy tạo ra một mảng có tổng phần tử chính xác hay không`s`. 

Quan sát quan trọng nhất là sự phân chia chỉ phụ thuộc vào giá trị chứ không phụ thuộc vào vị trí. Thứ tự tương đối được giữ nguyên, nhưng sau khi sắp xếp mảng, mỗi phần tách sẽ trở thành phần tách của một phạm vi giá trị liền kề. Điều đó biến quá trình này thành một phân vùng đệ quy của mảng đã được sắp xếp. 

Các ràng buộc đủ lớn nên chúng tôi không thể mô phỏng mọi truy vấn một cách độc lập. Trên tất cả các trường hợp thử nghiệm, cả tổng số phần tử mảng và tổng số truy vấn đều cao nhất`10^5`. Bất kỳ giải pháp xung quanh`O(nq)`sẽ quá chậm vì nó có thể yêu cầu khoảng`10^{10}`hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần gũi`O(n log n)`tiền xử lý và sau đó xử lý truy vấn rất nhanh. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét một mảng trong đó tất cả các giá trị đều bằng nhau:```
[5, 5, 5, 5]
```Tính toán`mid`cũng là`5`, vì vậy mọi phần tử đều nằm ở phần bên trái và phần bên phải trống. Không thể chia tách thêm nữa. Số tiền duy nhất có thể đạt được là`20`. Việc triển khai đệ quy bất cẩn có thể lặp lại mãi mãi vì phạm vi không bao giờ co lại. 

Coi như:```
[1, 2]
```Tổng số tiền`3`có thể đạt được mà không cần thực hiện bất kỳ sự phân chia nào. Một số giải pháp chỉ ghi lại số tiền sau khi chia tách và quên bao gồm chính phân khúc hiện tại. Sau đó truy vấn`3`sẽ trả về sai "Không". 

Một trường hợp tế nhị khác là:```
[1, 1, 100]
```Sự phân chia đầu tiên tách ra`[1,1]`Và`[100]`. Số tiền có thể đạt được là`102`,`2`, Và`100`. Truy vấn`101`phải bị từ chối ngay cả khi nó nằm giữa số tiền có thể đạt được. Vấn đề không phải là yêu cầu các tập hợp con tùy ý. Chỉ có số tiền được tạo ra bởi quá trình phân chia đệ quy mới quan trọng. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu là tuân theo một cách rõ ràng mọi chuỗi lựa chọn có thể có. Từ mỗi mảng hiện tại, chúng ta có thể giữ lại phần bên trái hoặc phần bên phải, tạo ra cây đệ quy nhị phân. Mỗi mảng có thể truy cập đều đóng góp một khoản tiền có thể đạt được. 

Điều này đúng vì nó khớp trực tiếp với định nghĩa của quy trình. Thật không may, việc đại diện cho các mảng và phân vùng lại chúng nhiều lần rất tốn kém. Trong trường hợp xấu nhất, chúng tôi liên tục quét các mảng lớn và tạo ra nhiều bản sao. Với tối đa`10^5`xét về tổng thể, cách tiếp cận này nhanh chóng trở nên không thực tế. 

Quan sát quan trọng là sau khi sắp xếp, mọi trạng thái đệ quy đều tương ứng với một phân đoạn liền kề của mảng đã được sắp xếp. 

Giả sử đoạn mảng được sắp xếp là`a[l...r]`. Ngưỡng phân chia là$$mid = \left\lfloor \frac{a[l] + a[r]}{2} \right\rfloor$$Vì phân đoạn đã được sắp xếp nên tất cả các giá trị`<= mid`tạo thành tiền tố của phân đoạn và tất cả các giá trị`> mid`tạo thành một hậu tố. Vị trí phân chia có thể được tìm thấy bằng tìm kiếm nhị phân. 

Bây giờ chúng ta không còn cần phải theo dõi các mảng thực tế nữa. Một trạng thái được mô tả đầy đủ bằng khoảng chỉ số`[l, r]`. 

Mỗi phân đoạn như vậy có một tổng được xác định rõ ràng. Bất kỳ phân đoạn nào xuất hiện trong quá trình đệ quy này đều tương ứng với một mảng có thể đạt được, vì vậy chúng ta có thể tính toán trước tất cả các tổng có thể đạt được và lưu trữ chúng trong một tập hợp. 

Khi đã biết tất cả số tiền có thể đạt được, mỗi truy vấn sẽ trở thành một bài kiểm tra tư cách thành viên đơn giản. 

Việc đệ quy hiệu quả vì mỗi phép chia đều chia phạm vi giá trị. Một phân đoạn chỉ được xử lý một lần và tìm kiếm nhị phân sẽ xác định điểm phân vùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo số lần chia | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng. 

Sau khi sắp xếp, mỗi lần phân chia tương ứng với việc tách một đoạn liền kề thành hai đoạn con liền kề. 
2. Xây dựng mảng tổng tiền tố. 

Điều này cho phép tổng của bất kỳ phân đoạn nào`[l, r]`được tính toán trong thời gian không đổi. 
3. Bắt đầu quy trình đệ quy trên toàn bộ phân đoạn mảng đã được sắp xếp. 

Phân đoạn ban đầu đại diện cho mảng ban đầu, có tổng luôn có thể đạt được. 
4. Đối với một đoạn`[l, r]`, tính tổng của nó và chèn nó vào một tập hợp. 

Mỗi phân đoạn đệ quy tương ứng với một mảng hợp lệ có thể đạt được thông qua các hoạt động được phép. 
5. Nếu`a[l] == a[r]`, dừng lại. 

Tất cả các giá trị trong phân khúc đều giống hệt nhau. Bất kỳ sự phân chia nào nữa sẽ đặt mọi phần tử về cùng một phía, do đó đệ quy phải chấm dứt. 
6. Tính toán$$mid = \left\lfloor \frac{a[l] + a[r]}{2} \right\rfloor$$7. Sử dụng tìm kiếm nhị phân để tìm vị trí đầu tiên có giá trị lớn hơn`mid`. 

Điều này xác định ranh giới giữa các nhóm bên trái và bên phải chính xác như được xác định bởi vấn đề. 
8. Nếu ranh giới không thực sự chia đoạn, hãy dừng lại. 

Điều này ngăn cản việc đệ quy vô ích khi một bên trống. 
9. Xử lý đệ quy đoạn bên trái. 
10. Xử lý đệ quy đúng đoạn. 
11. Sau khi quá trình tiền xử lý kết thúc, hãy trả lời mọi truy vấn bằng cách kiểm tra xem giá trị của nó có tồn tại trong tập hợp các tổng có thể đạt được hay không. 

### Tại sao nó hoạt động 

Đệ quy phản ánh chính xác hoạt động được phép. 

Đối với bất kỳ phân đoạn đệ quy nào`[l, r]`, các giá trị bên trong nó chính xác là các phần tử của mảng nào đó có thể xuất hiện trong quá trình này. Do đó, tổng phân khúc có thể đạt được và phải được ghi lại. 

Ngược lại, mọi sự phân chia hợp pháp trong quy trình ban đầu đều phân chia các phần tử theo ngưỡng`mid`. Trong một phân đoạn được sắp xếp, phân vùng đó chính xác là phần phân chia tìm kiếm nhị phân được thuật toán sử dụng. Đệ quy khám phá cả hai phân đoạn kết quả, do đó mọi mảng có thể xuất hiện đều được biểu diễn. 

Do đó, tập hợp được xây dựng bởi đệ quy chứa tất cả các tổng có thể đạt được và chỉ các tổng có thể đạt được. Việc trả lời truy vấn giảm bớt việc kiểm tra tư cách thành viên trong tập hợp đó. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n, q = map(int, input().split())
        a = sorted(map(int, input().split()))

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        possible = set()

        def dfs(l, r):
            seg_sum = pref[r + 1] - pref[l]
            possible.add(seg_sum)

            if a[l] == a[r]:
                return

            mid = (a[l] + a[r]) // 2
            p = bisect_right(a, mid, l, r + 1)

            if p == l or p == r + 1:
                return

            dfs(l, p - 1)
            dfs(p, r)

        dfs(0, n - 1)

        for _ in range(q):
            s = int(input())
            answers.append("Yes" if s in possible else "No")

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Bước đầu tiên sắp xếp mảng vì quy tắc phân tách chỉ phụ thuộc vào các giá trị. Sau khi được sắp xếp, mọi trạng thái đệ quy sẽ trở thành một phạm vi chỉ mục liền kề. 

Mảng tổng tiền tố cho phép tính tổng phân đoạn theo thời gian không đổi. Nếu không có nó, mỗi lệnh gọi đệ quy sẽ cần quét toàn bộ phân đoạn của nó, điều này làm tăng độ phức tạp lên đáng kể. 

các`possible`thiết lập các cửa hàng mỗi số tiền có thể đạt được. Ngay sau khi một phân đoạn được truy cập, tổng của nó sẽ được chèn vào vì bản thân phân đoạn đó đại diện cho một mảng có thể thu được hợp lệ. 

Điều kiện dừng`a[l] == a[r]`là rất quan trọng. Khi tất cả các giá trị đều bằng nhau thì không tồn tại sự phân chia có ý nghĩa. Bỏ qua kiểm tra này có thể tạo ra đệ quy vô hạn.`bisect_right`tìm thấy giá trị đầu tiên lớn hơn`mid`, khớp chính xác với định nghĩa vấn đề. Giá trị bằng`mid`thuộc về phần bên trái. 

Séc```
if p == l or p == r + 1:
    return
```bảo vệ chống lại các phân vùng trống. Sự phân chia như vậy sẽ không làm giảm phân khúc. 

Cuối cùng, mỗi truy vấn được trả lời trong thời gian dự kiến ​​không đổi bằng cách sử dụng tư cách thành viên đã đặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng:```
[1, 2, 3, 4, 5]
```Mảng được sắp xếp không thay đổi. 

| Phân đoạn | Tối thiểu | Tối đa | Giữa | Tổng hợp | Chia | 
| --- | --- | --- | --- | --- | --- | 
| [1,2,3,4,5] | 1 | 5 | 3 | 15 | [1,2,3] và [4,5] | 
| [1,2,3] | 1 | 3 | 2 | 6 | [1,2] và [3] | 
| [1,2] | 1 | 2 | 1 | 3 | [1] và [2] | 
| [1] | 1 | 1 | - | 1 | dừng lại | 
| [2] | 2 | 2 | - | 2 | dừng lại | 
| [3] | 3 | 3 | - | 3 | dừng lại | 
| [4,5] | 4 | 5 | 4 | 9 | [4] và [5] | 
| [4] | 4 | 4 | - | 4 | dừng lại | 
| [5] | 5 | 5 | - | 5 | dừng lại | 

Số tiền có thể đạt được là:```
{1,2,3,4,5,6,9,15}
```Truy vấn`9`trả về "Có", trong khi truy vấn`8`trả về "Không". 

Ví dụ này cho thấy chỉ có tổng tương ứng với các phân đoạn đệ quy là hợp lệ. 

### Ví dụ 2 

Mảng:```
[3, 1, 3, 1, 3]
```Sau khi sắp xếp:```
[1, 1, 3, 3, 3]
```| Phân đoạn | Tối thiểu | Tối đa | Giữa | Tổng hợp | Chia | 
| --- | --- | --- | --- | --- | --- | 
| [1,1,3,3,3] | 1 | 3 | 2 | 11 | [1,1] và [3,3,3] | 
| [1,1] | 1 | 1 | - | 2 | dừng lại | 
| [3,3,3] | 3 | 3 | - | 9 | dừng lại | 

Số tiền có thể đạt được là:```
{11, 2, 9}
```Truy vấn`3`trả về "Không" mặc dù giá trị`3`tồn tại trong mảng. 

Điều này chứng tỏ rằng nhiệm vụ không phải là chọn các tập hợp con tùy ý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q) | Việc sắp xếp chiếm ưu thế trong quá trình tiền xử lý, đệ quy sử dụng tìm kiếm nhị phân trên các phân đoạn được tạo | 
| Không gian | O(n) | Tổng tiền tố, trạng thái đệ quy và tập hợp tổng có thể đạt được | 

Tổng số tiền của`n`và tổng số tiền`q`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`. MỘT`O(n log n + q)`giải pháp dễ dàng phù hợp trong thời hạn, và`O(n)`bộ nhớ thoải mái dưới mức giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_right

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        a = sorted(map(int, input().split()))

        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        possible = set()

        def dfs(l, r):
            possible.add(pref[r + 1] - pref[l])

            if a[l] == a[r]:
                return

            mid = (a[l] + a[r]) // 2
            p = bisect_right(a, mid, l, r + 1)

            if p == l or p == r + 1:
                return

            dfs(l, p - 1)
            dfs(p, r)

        dfs(0, n - 1)

        for _ in range(q):
            s = int(input())
            out.append("Yes" if s in possible else "No")

    return "\n".join(out)

# provided sample
assert run(
"""2
5 5
1 2 3 4 5
1
8
9
12
6
5 5
3 1 3 1 3
1
2
3
9
11
"""
) == """Yes
No
Yes
No
Yes
No
Yes
No
Yes
Yes"""

# minimum size
assert run(
"""1
1 3
7
7
1
14
"""
) == """Yes
No
No"""

# all equal values
assert run(
"""1
4 3
5 5 5 5
20
5
10
"""
) == """Yes
No
No"""

# simple split
assert run(
"""1
2 3
1 2
1
2
3
"""
) == """Yes
Yes
Yes"""

# uneven values
assert run(
"""1
3 4
1 1 100
2
100
102
101
"""
) == """Yes
Yes
Yes
No"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | Chỉ có thể đạt được tổng số tiền | Trường hợp đệ quy cơ sở | 
| Tất cả các giá trị bằng nhau | Không chia tách thêm | Ngăn chặn đệ quy vô hạn | 
|`[1,2]`| Có thể đạt được tổng 1, 2 và 3 | Xử lý đúng tổng phân khúc | 
|`[1,1,100]`| 101 bị từ chối | Không phải là bài toán tổng con tùy ý | 

## Vỏ cạnh 

### Tất cả các giá trị đều giống nhau 

đầu vào:```
1
4 2
5 5 5 5
20
10
```Phân đoạn được sắp xếp có`a[l] = a[r] = 5`. 

Thuật toán ghi tổng`20`và ngay lập tức dừng đệ quy. Tập hợp có thể đạt được trở thành:```
{20}
```Truy vấn`20`trả về "Có" và truy vấn`10`trả về "Không". Điều này phù hợp với quy trình vì mỗi lần phân chia sẽ đặt tất cả các phần tử vào cùng một nhóm. 

### Tổng số tiền không chia 

đầu vào:```
1
2 1
1 2
3
```Bản thân mảng ban đầu là một mảng có thể thu được hợp lệ vì không được phép thực hiện bất kỳ thao tác nào. 

Thuật toán ghi lại tổng phân đoạn gốc trước khi thực hiện bất kỳ phép chia nào, do đó`3`được chèn vào tập hợp ngay lập tức. 

Câu trả lời là chính xác`"Yes"`. 

### Truy vấn giữa các tổng có thể đạt được 

đầu vào:```
1
1 4
1 1 100
101
```Đệ quy tạo ra các khoản tiền:```
102, 2, 100
```Không có phân đoạn nào có tổng`101`. 

Thuật toán trả lời`"No"`bởi vì tư cách thành viên được kiểm tra dựa trên tập hợp chính xác các tổng có thể đạt được, không dựa trên bất kỳ tiêu chí khoảng hoặc tổng tập hợp con nào. 

### Giá trị bằng ngưỡng phân chia 

đầu vào:```
1
3 2
2 2 3
4
3
```Đây:```
mid = (2 + 3) // 2 = 2
```Cả hai lần xuất hiện của`2`phải thuộc đoạn bên trái.`bisect_right`tìm thấy phần tử đầu tiên lớn hơn`2`, sản xuất các phân đoạn`[2,2]`Và`[3]`. 

Số tiền có thể đạt được trở thành:```
7, 4, 3
```Truy vấn`4`Và`3`cả hai đều trở lại`"Yes"`. Điều này xác nhận rằng các giá trị bằng`mid`được xử lý chính xác.
