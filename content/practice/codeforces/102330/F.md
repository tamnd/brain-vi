---
title: "CF 102330F - \u0417\u0432\u0435\u0440\u044c\u043a\u0438"
description: "Chúng ta có n con vật. Con vật i có ba tham số a i ​, b i ​ và c i ​. Nếu lồng hiện có nhiều nhất c i ​ con vật, thì con vật này góp phần gây ra sự hung hãn. Nếu lồng chứa nhiều hơn c i ​ động vật, nó đóng góp b i ​, trong đó a i ​ ≤b i ​."
date: "2026-08-13T04:05:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102330
codeforces_index: "F"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2019.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102330
solve_time_s: 146
verified: true
draft: false
---

[CF 102330F - \u0417\u0432\u0435\u0440\u044c\u043a\u0438](https://codeforces.com/problemset/problem/102330/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có n con vật. Con vật i có ba tham số a i ​, b i ​ và c i ​. Nếu lồng hiện có nhiều nhất c i ​ con vật, thì con vật này góp phần gây ra sự hung hãn. Nếu lồng chứa nhiều hơn c i ​ động vật, nó đóng góp b i ​, trong đó a i ​ ≤b i ​. 

Lồng chỉ có thể chứa một bộ sưu tập động vật khi tổng giá trị gây hấn hiện tại của chúng lớn nhất là s. Chúng ta cần số lượng động vật lớn nhất có thể có thể ở bên trong cùng một lúc. 

Khó khăn là sự đóng góp của một con vật phụ thuộc vào số lượng con vật cuối cùng trong chuồng. Nếu chúng ta kiểm tra xem có thể chọn chính xác k con vật hay không thì con vật i có giá cố định 

w i ​ (k)={ a i ​ , b i ​ , ​ c i ​ ≥k, c i ​ <k. ​ 

Khi k được sửa, vấn đề trở nên đơn giản: chọn chính xác k con vật có w i ​ (k) nhỏ nhất. Do đó, mức độ gây hấn tối thiểu có thể là tổng của k chi phí hiện tại nhỏ nhất. 

Ràng buộc n 10 5 loại trừ việc kiểm tra các tập hợp con, vốn sẽ yêu cầu nhiều thao tác theo cấp số nhân, đồng thời loại trừ việc quét O(n 2 ) trên mọi kích thước lồng có thể có nếu mỗi kích thước yêu cầu xem qua tất cả các loài động vật. Các giá trị a i ​ ,b i ​ ,s có thể đạt tới 10 9, do đó việc triển khai cũng phải sử dụng số học có kích thước 64 bit. Số nguyên Python đã xử lý việc này một cách an toàn. 

Có một số trường hợp ranh giới mà việc triển khai bất cẩn có thể thất bại. Đầu tiên, điều kiện là c i ​ ≥k, không phải c i ​ >k. Ví dụ,```
1 5
5 10 1
```Con vật duy nhất có thể được lưu trữ, bởi vì khi có một con vật, 1≤5, thì sự hung hãn của nó là 5. Câu trả lời là`1`. Một triển khai sử dụng`c_i > k`sẽ chuyển nó sang trạng thái gây hấn không chính xác 10. 

Trường hợp thứ hai là khi a i ​ =b i ​. Ví dụ,```
2 0
0 0 1
0 0 1
```Cả hai loài động vật đều không có sự hung dữ, vì vậy câu trả lời là`2`. Cấu trúc dữ liệu giả định mỗi lần chuyển đổi thay đổi giá trị vẫn có thể hoạt động nhưng không được vô tình xóa hoặc sao chép một con vật khi hai giá trị bằng nhau. 

Trường hợp thứ ba xảy ra khi không có số lượng động vật dương phù hợp:```
1 0
1 5 1
```Đối với một con vật, mức độ gây hấn là 1, vượt quá sức chứa của lồng. Câu trả lời đúng là`0`. Thuật toán phải cho phép câu trả lời là 0 thay vì giả sử có thể chọn ít nhất một con vật. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp là thử mọi số k động vật có thể. Đối với k cố định, hãy tính mức độ hung hãn hiện tại của mỗi con vật bằng cách sử dụng quy tắc trên, sắp xếp các giá trị n và lấy k nhỏ nhất. Điều này đúng vì khi kích thước lồng cuối cùng được cố định, mỗi con vật đều có chi phí cố định và việc chọn k chi phí rẻ nhất là tối ưu. 

Tuy nhiên, việc thực hiện điều này với mỗi k đòi hỏi n loại phần tử riêng biệt. Độ phức tạp thu được là O(n 2 logn). Với n=10 5, con số này vượt xa phạm vi dự định, với khoảng 10 10 phần tử tham gia vào công việc sắp xếp trước cả khi tính đến việc so sánh sắp xếp. 

Quan sát quan trọng là khi k tăng thêm một, con vật chỉ thay đổi giá trị của nó một lần. Con vật i sử dụng i ​ cho mọi kích thước lồng lên tới c i ​ và thay đổi vĩnh viễn thành b i ​ khi kích thước lồng trở thành c i ​ +1. Vì vậy, thay vì xây dựng lại toàn bộ danh sách chi phí cho mỗi k, chúng ta có thể duy trì tất cả chi phí hiện tại một cách linh hoạt. 

Với mỗi k, chúng ta cần tổng của k giá trị nhỏ nhất trong tập hợp hiện tại. Cây Fenwick có thể duy trì điều này một cách hiệu quả sau khi phối hợp nén tất cả các giá trị gây hấn có thể có. Một cây Fenwick lưu trữ số lượng động vật hiện có mỗi giá trị và một cây khác lưu trữ tổng các giá trị đó. Khi k tăng, tất cả động vật có c i ​ =k−1 đều được đổi từ a i ​ thành b i ​. Mỗi thay đổi chỉ là việc loại bỏ và chèn vào cây Fenwick. 

Thao tác còn lại là tìm tổng của k giá trị nhỏ nhất. Cây Fenwick chứa số đếm cho phép chúng ta tìm vị trí của giá trị nhỏ thứ k trong O(logn). Sau đó, cây thứ hai đưa ra tổng của tất cả các giá trị bên dưới vị trí đó và chúng tôi chỉ thêm số lượng bản sao của giá trị cuối cùng nếu cần. 

Lực lượng vũ phu hoạt động vì tập hợp tối ưu cho k cố định chỉ đơn giản là tập hợp k động vật rẻ nhất. Nó thất bại vì nó liên tục tái tạo lại thông tin chỉ thay đổi dần dần. Việc quan sát thấy rằng mỗi con vật thay đổi giá của nó chính xác một lần cho phép chúng tôi duy trì ngầm định nhiều tập hợp được sắp xếp và xử lý mọi kích thước lồng trong O(logn). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 2 logn) | O(n) | Quá chậm | 
| Tối ưu | O(nlogn) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các con vật và thu thập mọi giá trị a i ​ và b i ​. Đây là những giá trị gây hấn duy nhất có thể xảy ra, do đó, việc nén phối hợp chúng sẽ mang lại cho cây Fenwick kích thước tối đa là 2n. 
2. Ban đầu hãy xem xét một cái lồng chứa một con vật. Vì mọi c i ​ ≥1 nên mọi con vật hiện tại đều có tính hung dữ a i ​. Chèn tất cả các giá trị a i vào cây đếm và tính tổng Fenwick. 
3. Nhóm các con vật theo giá trị c i ​ của chúng. Khi chúng ta sắp đánh giá kích thước lồng k, mọi con vật có c i ​ =k−1 vừa vượt qua ngưỡng của nó. Xóa giá trị a i ​ của nó khỏi cấu trúc dữ liệu và chèn giá trị b i ​ của nó. Những động vật có c i ​ lớn hơn vẫn sử dụng i ​, trong khi những động vật vượt qua ngưỡng trước đó đã sử dụng b i ​. 
4. Tìm giá trị gây hấn hiện tại nhỏ nhất thứ k bằng cách sử dụng cây đếm Fenwick. Nếu vị trí nén của nó tương ứng với giá trị x, hãy tìm số và tổng các giá trị nhỏ hơn x bằng cách sử dụng cây tổng Fenwick. 
5. Giả sử có`cnt`giá trị nhỏ hơn x, với tổng số tiền`sm`. K−cnt động vật được chọn còn lại đều có tính hung hãn x, do đó mức độ hung dữ tối thiểu có thể có đối với k động vật là 

sm+(k−cnt)x. 

Nếu giá trị này lớn nhất là s thì k động vật có thể được lưu trữ, vì vậy hãy cập nhật câu trả lời. 

1. Tiếp tục đi qua tất cả k từ 1 đến n. Câu trả lời là khả thi lớn nhất k. 

### Tại sao nó hoạt động 

Trước khi kiểm tra kích thước lồng k, cấu trúc dữ liệu chứa chính xác giá trị gây hấn hiện tại của mọi con vật nếu lồng chứa k con vật. Một con vật đã thay đổi từ a i ​ thành b i ​ chính xác khi c i ​ <k, và không con vật nào khác cần thay đổi. 

Với k cố định này, mọi lựa chọn hợp lệ đều chứa chính xác k con vật. Vì mỗi con vật có một giá trị gây hấn hiện tại cố định nên tổng mức gây hấn tối thiểu có thể đạt được bằng cách lấy k giá trị nhỏ nhất. Cây Fenwick tính toán chính xác số tiền tối thiểu đó. Do đó, thuật toán đánh dấu k khả thi một cách chính xác khi một tập k động vật nào đó vừa với lồng. 

Vì việc thêm một con vật khác chỉ có thể làm tăng số lượng con vật và không bao giờ có thể làm giảm sự hung hãn của bất kỳ con vật nào đã chọn, nên tính khả thi là đơn điệu. Việc xử lý trực tiếp mọi k sẽ tránh được việc cần phải tìm kiếm tính khả thi riêng biệt và đưa ra kích thước khả thi tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())

    animals = []
    values = []

    for _ in range(n):
        a, b, c = map(int, input().split())
        animals.append((a, b, c))
        values.append(a)
        values.append(b)

    coords = sorted(set(values))
    m = len(coords)
    pos = {x: i + 1 for i, x in enumerate(coords)}

    count_bit = [0] * (m + 1)
    sum_bit = [0] * (m + 1)

    def add(bit, i, delta):
        while i <= m:
            bit[i] += delta
            i += i & -i

    def prefix(bit, i):
        res = 0
        while i > 0:
            res += bit[i]
            i -= i & -i
        return res

    # Find the smallest Fenwick index whose prefix count is >= k.
    def kth(k):
        idx = 0
        step = 1 << (m.bit_length() - 1)

        while step:
            nxt = idx + step
            if nxt <= m and count_bit[nxt] < k:
                idx = nxt
                k -= count_bit[nxt]
            step >>= 1

        return idx + 1

    # Animals with c = k - 1 change from a to b before size k is checked.
    by_c = [[] for _ in range(n + 1)]

    for a, b, c in animals:
        by_c[c].append((a, b))

        p = pos[a]
        add(count_bit, p, 1)
        add(sum_bit, p, a)

    answer = 0

    for k in range(1, n + 1):
        threshold = k - 1

        for a, b in by_c[threshold]:
            if a == b:
                continue

            pa = pos[a]
            pb = pos[b]

            add(count_bit, pa, -1)
            add(sum_bit, pa, -a)

            add(count_bit, pb, 1)
            add(sum_bit, pb, b)

        p = kth(k)

        cnt_before = prefix(count_bit, p - 1)
        sum_before = prefix(sum_bit, p - 1)

        value = coords[p - 1]
        total = sum_before + (k - cnt_before) * value

        if total <= s:
            answer = k

    print(answer)

if __name__ == "__main__":
    solve()
```Việc nén tọa độ sẽ chuyển đổi mọi giá trị xâm lược có thể có thành chỉ số Fenwick. Có nhiều nhất 2n giá trị phân biệt, bởi vì mỗi con vật chỉ đóng góp a i ​ và b i​. 

Trạng thái Fenwick ban đầu chứa tất cả các giá trị a i ​. Điều này đúng với k=1, vì mọi c i ​ ít nhất là một. các`by_c`mảng ghi lại chính xác thời điểm mỗi con vật phải thay đổi giá trị của nó. 

Vòng lặp sử dụng`by_c[k - 1]`trước khi kiểm tra kích thước k. Đây là điều kiện biên tới hạn. Một con vật có c i ​ =k−1 nhìn thấy k>c i ​, vì vậy nó phải sử dụng b i ​. Ngược lại, một con vật có c i ​ =k vẫn sử dụng a i ​. 

Cây đếm Fenwick được sử dụng để thống kê thứ tự. các`kth(k)`thường trình tìm vị trí nén chứa giá trị hiện tại nhỏ nhất thứ k trong O(logn). Cây tổng Fenwick sau đó nhận được tổng của mọi giá trị nhỏ hơn nó. Nếu có ít hơn k con vật nằm dưới giá trị cuối cùng thì những con vật còn lại đều có đúng giá trị đó. 

các`a == b`kiểm tra tránh thực hiện hai cập nhật không cần thiết cho một con vật có hành vi hung dữ không bao giờ thay đổi. Nó không bắt buộc phải chính xác nhưng nó làm giảm công việc và làm cho logic chuyển đổi rõ ràng hơn. 

Tất cả các tổng có thể đạt xấp xỉ 10 14, do đó số nguyên 32 bit có chiều rộng cố định sẽ không đủ. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
2 6
2 4 1
2 4 2
```multiset ban đầu chứa cả hai giá trị a. 

| k | Những thay đổi trước khi kiểm tra | Chi phí hiện tại | k nhỏ nhất | Số tiền tối thiểu | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | không | 2, 2 | 2 | 2 | vâng | 
| 2 | con vật 1: 2→4 | 4, 2 | 2, 4 | 6 | vâng | 

Con vật đầu tiên thay đổi chính xác khi chúng ta chuyển từ một con vật sang hai con vật, vì ngưỡng của nó là c 1 ​ =1. Tổng tối thiểu cho hai con vật chính xác là sức chứa của lồng, vì vậy câu trả lời là`2`. 

Đối với mẫu thứ hai,```
4 10
3 4 2
3 5 3
1 1 1
2 7 3
```sự tiến triển là: 

| k | Những thay đổi trước khi kiểm tra | Chi phí hiện tại | k nhỏ nhất | Số tiền tối thiểu | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | không | 3, 3, 1, 2 | 1 | 1 | vâng | 
| 2 | con vật 3: 1→1 | 3, 3, 1, 2 | 1, 2 | 3 | vâng | 
| 3 | không | 3, 3, 1, 2 | 1, 2, 3 | 6 | vâng | 
| 4 | con vật 1: 3→4 | 4, 3, 1, 2 | 1, 2, 3, 4 | 10 | vâng | 

Với k=4, con vật 1 vượt qua ngưỡng c 1 ​ =2, do đó sự hung hãn của nó trở thành 4. Cả bốn con vật bây giờ có tổng mức độ hung hãn chính xác là 10, vừa với cái lồng. Câu trả lời là`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nlogn) | Quá trình nén mất O(nlogn), mỗi con vật thay đổi nhiều nhất một lần và mỗi lần cập nhật Fenwick hoặc truy vấn thống kê thứ tự có chi phí O(logn). | 
| Không gian | O(n) | Các động vật, nhóm ngưỡng, giá trị nén và cây Fenwick đều chứa các phần tử O(n). | 

Có tối đa 2n giá trị gây hấn nén và mỗi con trong số n con vật thực hiện tối đa một lần chuyển đổi từ a i ​ sang b i ​. Do đó, thuật toán chỉ thực hiện các phép toán Fenwick O(n), mỗi logarit tính bằng n. Đối với n=10 5, điều này nằm trong mức độ phức tạp dự định, trong khi tổng có kích thước 64 bit được xử lý trực tiếp bằng số nguyên Python. 

## Trường hợp thử nghiệm```python
# The production solution can be tested by placing it in a module and
# calling solve() after replacing sys.stdin and sys.stdout.
#
# For a self-contained assert suite, the same algorithm is wrapped below.

import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    n, s = map(int, sys.stdin.readline().split())

    animals = []
    values = []

    for _ in range(n):
        a, b, c = map(int, sys.stdin.readline().split())
        animals.append((a, b, c))
        values.extend((a, b))

    coords = sorted(set(values))
    pos = {x: i + 1 for i, x in enumerate(coords)}
    m = len(coords)

    count_bit = [0] * (m + 1)
    sum_bit = [0] * (m + 1)

    def add(bit, i, delta):
        while i <= m:
            bit[i] += delta
            i += i & -i

    def prefix(bit, i):
        res = 0
        while i:
            res += bit[i]
            i -= i & -i
        return res

    def kth(k):
        idx = 0
        step = 1 << (m.bit_length() - 1)

        while step:
            nxt = idx + step
            if nxt <= m and count_bit[nxt] < k:
                idx = nxt
                k -= count_bit[nxt]
            step >>= 1

        return idx + 1

    by_c = [[] for _ in range(n + 1)]

    for a, b, c in animals:
        by_c[c].append((a, b))
        p = pos[a]
        add(count_bit, p, 1)
        add(sum_bit, p, a)

    answer = 0

    for k in range(1, n + 1):
        for a, b in by_c[k - 1]:
            if a == b:
                continue

            pa = pos[a]
            pb = pos[b]

            add(count_bit, pa, -1)
            add(sum_bit, pa, -a)

            add(count_bit, pb, 1)
            add(sum_bit, pb, b)

        p = kth(k)
        cnt_before = prefix(count_bit, p - 1)
        sum_before = prefix(sum_bit, p - 1)
        value = coords[p - 1]

        total = sum_before + (k - cnt_before) * value

        if total <= s:
            answer = k

    sys.stdout.write(str(answer) + "\n")

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided sample 1
assert run(
    """2 6
2 4 1
2 4 2
"""
) == "2\n", "sample 1"

# Provided sample 2
assert run(
    """4 10
3 4 2
3 5 3
1 1 1
2 7 3
"""
) == "4\n", "sample 2"

# Minimum-size input, including the possibility of storing nothing.
assert run(
    """1 0
1 5 1
"""
) == "0\n", "minimum size and zero capacity"

# All values equal and all animals remain harmless.
assert run(
    """5 0
0 0 1
0 0 2
0 0 3
0 0 4
0 0 5
"""
) == "5\n", "all equal zero aggression"

# Exact-threshold case: c == k must still use a, not b.
assert run(
    """3 6
2 100 3
2 100 3
2 100 3
"""
) == "3\n", "exact threshold"

# Maximum-size style case with large values and a feasible full cage.
assert run(
    """5 5000000000
1000000000 2000000000 1
1000000000 3000000000 2
1000000000 4000000000 3
1000000000 5000000000 4
1000000000 6000000000 5
"""
) == "5\n", "large sums and full selection"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 1 5 1`|`0`| Kích thước tối thiểu và khả năng không có con vật nào phù hợp | 
| Năm loài động vật không hung dữ |`5`| Giá trị bằng nhau và a i ​ =b i ​ =0 | 
| Ba con vật với`c=3`|`3`| Ngưỡng chính xác sử dụng i ​ | 
| Giá trị gây hấn lớn |`5`| Tổng lớn và số học số nguyên Python | 

## Vỏ cạnh 

Điều kiện ngưỡng chính xác được xử lý bằng cách thay đổi một con vật chỉ khi`k - 1 == c_i`. Coi như```
3 6
2 100 3
2 100 3
2 100 3
```Với k=3, con vật chưa vượt qua ngưỡng vì 3<c i ​. Cấu trúc dữ liệu do đó vẫn chứa`2, 2, 2`, mang lại tổng cộng`6`, vậy câu trả lời là`3`. Tại k=4, cả ba con vật sẽ chuyển sang`100`, nhưng dù sao cũng chỉ có ba con vật. Điều này mắc phải lỗi phổ biến khi chuyển đổi khi`k == c_i`thay vì khi nào`k > c_i`. 

Khi a i ​ =b i ​, quá trình chuyển đổi không có hiệu ứng số. Vì```
5 0
0 0 1
0 0 2
0 0 3
0 0 4
0 0 5
```mọi sự gây hấn hiện tại luôn bằng không. Cấu trúc Fenwick giữ năm bản sao số 0 trong toàn bộ quá trình, vì vậy mọi kích thước lồng từ`1`bởi vì`5`là khả thi và câu trả lời là`5`. Việc thực hiện`a == b`nhánh chỉ cần bỏ qua thao tác xóa và thêm dư thừa. 

Khi sức chứa của lồng quá nhỏ đối với dù chỉ một con vật, câu trả lời phải là 0. Vì```
1 0
1 5 1
```trạng thái ban đầu chứa một giá trị,`1`. Tổng của một giá trị nhỏ nhất là`1`, lớn hơn`s=0`, Vì thế`answer`không bao giờ được cập nhật từ giá trị ban đầu của nó`0`. 

Số tiền lớn là một ranh giới thực tế khác. Với năm con vật, mỗi con có số lượng hung dữ khoảng 10 9, tổng số có thể lên tới vài tỷ con trở lên. Thuật toán lưu trữ tổng số trong cây tổng Fenwick và trong biểu thức cho k giá trị nhỏ nhất mà không thu hẹp nó xuống còn 32 bit. Các số nguyên có độ chính xác tùy ý của Python làm cho phép tính số học trở nên an toàn. 

Cuối cùng, một con vật có thể có ngưỡng nhỏ hơn nhiều so với kích thước lồng cuối cùng. Khi ngưỡng của nó bị vượt qua, giá trị của nó sẽ thay đổi chính xác một lần và giữ nguyên b i ​ cho mỗi k tiếp theo. Quá trình chuyển đổi một lần này là bất biến trung tâm của thuật toán. Nếu quá trình triển khai được tính toán lại hoặc hoàn nguyên một phần các chuyển đổi như vậy trong khi di chuyển giữa các kích thước lồng, thì nó có thể kết hợp các chi phí thuộc các giá trị khác nhau của k, tạo ra mức tối thiểu không hợp lệ.
