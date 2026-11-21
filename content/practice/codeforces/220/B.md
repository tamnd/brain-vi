---
title: "CF 220B - Chú Voi Nhỏ Và Mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên dương và nhiều truy vấn phạm vi. Với mỗi truy vấn [l, r], chúng ta chỉ nhìn vào mảng con giữa các vị trí đó và đếm xem có bao nhiêu giá trị x thỏa mãn một điều kiện rất cụ thể: Bên trong mảng con đó, giá trị x xuất hiện đúng x lần."
date: "2026-06-04T02:00:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 220
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 136 (Div. 1)"
rating: 1800
weight: 220
solve_time_s: 118
verified: true
draft: false
---

[CF 220B - Chú voi nhỏ và mảng](https://codeforces.com/problemset/problem/220/B) 

**Đánh giá:** 1800 
**Tas:** thuật toán xây dựng, cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và nhiều truy vấn phạm vi. Đối với mỗi truy vấn`[l, r]`, chúng ta chỉ nhìn vào mảng con giữa các vị trí đó và đếm xem có bao nhiêu giá trị`x`thỏa mãn một điều kiện rất cụ thể: 

Bên trong mảng con đó, giá trị`x`xuất hiện chính xác`x`lần. 

Ví dụ, trong mảng con```
[3, 1, 2, 2, 3, 3, 7]
```tần số là:```
1 -> 1
2 -> 2
3 -> 3
7 -> 1
```Giá trị`1`,`2`, Và`3`mỗi cái xuất hiện chính xác nhiều lần bằng giá trị của chính chúng, vì vậy câu trả lời là`3`. 

Độ dài mảng và số lượng truy vấn đều lớn bằng`100000`. Một giải pháp đơn giản tính toán lại tần số cho mọi truy vấn sẽ bị loại trừ ngay lập tức. Thậm chí một`O(length of range)`giải pháp cho mỗi truy vấn sẽ yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất. 

Bản thân các giá trị có thể lớn bằng`10^9`. Điều này có nghĩa là chúng ta không thể xây dựng cấu trúc tần số được lập chỉ mục trực tiếp theo giá trị. Chúng tôi chỉ quan tâm đến các giá trị thực sự xuất hiện trong mảng. 

Một quan sát tinh tế là bất kỳ giá trị nào lớn hơn`n`không bao giờ có thể đóng góp vào một câu trả lời. Một giá trị phải xuất hiện chính xác`x`lần, nhưng không có tần số nào trong bất kỳ phạm vi nào có thể vượt quá`n`. Nếu như`x > n`, tần số phù hợp`x`là không thể. 

Coi như:```
n = 5
array = [100, 100, 100, 100, 100]
```Tần số của`100`là`5`, không`100`, vì vậy nó không bao giờ đóng góp. 

Một sai lầm dễ mắc phải khác là đếm số lần xuất hiện thay vì tính giá trị đủ điều kiện. Ví dụ:```
array = [2, 2]
query = [1, 2]
```Giá trị`2`xuất hiện đúng hai lần nên đáp án là:```
1
```không```
2
```bởi vì chúng tôi tính các giá trị thỏa mãn điều kiện chứ không phải vị trí. 

Cạm bẫy thứ ba xuất hiện khi tần số vượt qua giá trị mục tiêu trong khi mở rộng hoặc thu hẹp phạm vi. 

Giả sử tần số hiện tại của giá trị`3`thay đổi:```
2 -> 3 -> 4
```Khi nó đạt tới`3`, câu trả lời sẽ tăng thêm một. Khi nó trở thành`4`, câu trả lời sẽ giảm đi một. Việc quên một trong những quá trình chuyển đổi này sẽ gây ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Đối với một truy vấn`[l, r]`, chúng tôi quét toàn bộ mảng con, tính toán tần số bằng bản đồ băm, sau đó đếm xem có bao nhiêu giá trị thỏa mãn:```
frequency[value] == value
```Điều này đúng vì nó trực tiếp thực hiện định nghĩa. 

Thật không may, một truy vấn có thể bao gồm gần như toàn bộ mảng. Với`100000`truy vấn và phạm vi độ dài`100000`, công sẽ trở thành đại khái:```
100000 × 100000 = 10^10
```vượt xa giới hạn thời gian. 

Quan sát chính là các truy vấn hỏi về nhiều phạm vi chồng chéo. Các truy vấn liên tiếp thường chỉ khác nhau một vài phần tử. Tính toán lại tất cả các tần số từ đầu chất thải hoạt động. 

Đây chính xác là bối cảnh mà thuật toán của Mo tỏa sáng. Chúng tôi sắp xếp các truy vấn theo thứ tự đặc biệt để phạm vi hiện tại thay đổi dần dần. Trong khi chuyển từ truy vấn này sang truy vấn tiếp theo, chúng tôi chỉ thêm hoặc xóa một số phần tử nhỏ khỏi phạm vi hiện tại. 

Thách thức trở thành việc duy trì câu trả lời một cách linh hoạt khi tần số thay đổi. 

Giả sử giá trị`v`hiện có tần số`cnt[v]`. 

Trước khi tăng tần suất của nó, chúng tôi kiểm tra xem nó đã đóng góp hay chưa. Nếu như:```
cnt[v] == v
```thì việc loại bỏ sự bình đẳng đó phải làm giảm câu trả lời. 

Sau khi cập nhật tần số, chúng tôi kiểm tra lại. Nếu tần số mới bằng`v`, giá trị bắt đầu đóng góp và câu trả lời tăng lên. 

Mỗi thao tác thêm/xóa sẽ trở thành`O(1)`và việc đặt hàng của Mo chỉ đảm bảo về`O((n+m)√n)`điều chỉnh tổng phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu (Thuật toán Mo) | O((n+m)√n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và truy vấn. 
2. Thay thế mọi giá trị lớn hơn`n`với một giá trị không liên quan đặc biệt. 

Những giá trị như vậy không bao giờ có thể thỏa mãn`frequency = value`, vì vậy chúng ta không cần theo dõi chúng trong logic câu trả lời. 
3. Lưu trữ từng truy vấn cùng với chỉ mục gốc của nó. 
4. Chọn kích thước khối xấp xỉ bằng`√n`. 
5. Sắp xếp truy vấn theo thứ tự của Mo. 

Các truy vấn trong cùng khối bên trái được sắp xếp theo điểm cuối bên phải. Điều này giảm thiểu chuyển động của con trỏ. 
6. Duy trì phạm vi hiện tại`[curL, curR]`. 

Ban đầu phạm vi trống. 
7. Duy trì dải tần`cnt`.`cnt[x]`lưu trữ giá trị bao nhiêu lần`x`xuất hiện trong phạm vi hiện tại. 
8. Duy trì một biến`answer`. 

Giá trị này bằng số lượng giá trị có tần số hiện khớp với giá trị của chúng. 
9. Khi gia tăng giá trị`v`: 

Đầu tiên hãy kiểm tra xem`cnt[v] == v`. Nếu đúng thì giảm`answer`vì sự bình đẳng sắp bị phá vỡ. 

Tăng`cnt[v]`. 

Kiểm tra lại xem`cnt[v] == v`. Nếu đúng thì tăng`answer`. 
10. Khi xóa giá trị`v`: 

Đầu tiên hãy kiểm tra xem`cnt[v] == v`. Nếu đúng thì giảm`answer`. 

Giảm bớt`cnt[v]`. 

Kiểm tra lại xem`cnt[v] == v`. Nếu đúng thì tăng`answer`. 
11. Với mỗi truy vấn theo thứ tự Mo, di chuyển`curL`Và`curR`cho đến khi phạm vi hiện tại khớp với phạm vi truy vấn. 
12. Lưu trữ hiện tại`answer`là kết quả cho truy vấn đó. 
13. Xuất câu trả lời theo thứ tự truy vấn ban đầu. 

### Tại sao nó hoạt động 

Vào mọi thời điểm,`cnt[x]`bằng tần số của giá trị`x`trong phạm vi hiện tại được duy trì bởi thuật toán Mo. 

Biến`answer`đếm chính xác những giá trị thỏa mãn:```
cnt[x] = x
```Bất cứ khi nào tần số thay đổi, chỉ có một giá trị bị ảnh hưởng. Cập nhật thói quen thêm/xóa`answer`chính xác khi giá trị đó đi vào hoặc rời khỏi điều kiện đẳng thức. 

Vì thuật toán của Mo cuối cùng sẽ truy cập chính xác mọi phạm vi truy vấn và tần số được duy trì luôn chính xác cho phạm vi hiện tại nên câu trả lời được lưu trữ cho mỗi truy vấn chính xác là số giá trị xuất hiện một số lần bằng giá trị của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    # Values > n can never satisfy freq == value
    a = [x if x <= n else 0 for x in a]

    block = int(n ** 0.5) + 1

    queries = []
    for idx in range(m):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        queries.append((l, r, idx))

    queries.sort(key=lambda q: (q[0] // block, q[1]))

    cnt = [0] * (n + 1)
    answers = [0] * m
    current_answer = 0

    def add(pos):
        nonlocal current_answer
        v = a[pos]

        if v == 0:
            return

        if cnt[v] == v:
            current_answer -= 1

        cnt[v] += 1

        if cnt[v] == v:
            current_answer += 1

    def remove(pos):
        nonlocal current_answer
        v = a[pos]

        if v == 0:
            return

        if cnt[v] == v:
            current_answer -= 1

        cnt[v] -= 1

        if cnt[v] == v:
            current_answer += 1

    cur_l = 0
    cur_r = -1

    for l, r, idx in queries:
        while cur_l > l:
            cur_l -= 1
            add(cur_l)

        while cur_r < r:
            cur_r += 1
            add(cur_r)

        while cur_l < l:
            remove(cur_l)
            cur_l += 1

        while cur_r > r:
            remove(cur_r)
            cur_r -= 1

        answers[idx] = current_answer

    sys.stdout.write("\n".join(map(str, answers)))

if __name__ == "__main__":
    solve()
```Bước tiền xử lý đầu tiên thay thế tất cả các giá trị lớn hơn`n`với`0`. Những giá trị như vậy không bao giờ có thể đóng góp vì không có tần số nào có thể vượt quá`n`. Điều này tránh lãng phí thời gian duy trì các tần số không bao giờ có thể thỏa mãn điều kiện. 

các`cnt`mảng chỉ lưu trữ tần số cho các giá trị từ`1`ĐẾN`n`. Giá trị`0`được bỏ qua hoàn toàn. 

Phần tinh tế nhất là logic cập nhật. Khi tần số thay đổi, chúng ta phải loại bỏ phần đóng góp cũ của nó trước khi sửa đổi số đếm, sau đó thêm phần đóng góp mới của nó sau đó. Việc đảo ngược thứ tự đó sẽ dẫn đến từng lỗi một bất cứ khi nào tần số vượt qua giá trị mục tiêu của nó. 

Phạm vi được duy trì bởi thuật toán Mo là bao gồm cả hai đầu. Việc triển khai bắt đầu với một phạm vi trống`[0, -1]`, sau đó mở rộng hoặc thu nhỏ cho đến khi khớp với từng truy vấn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 2
3 1 2 2 3 3 7
1 7
3 4
```Các truy vấn theo thứ tự được sắp xếp sẽ khớp với thứ tự đầu vào. 

Đối với phạm vi`[1,7]`: 

| Giá trị | Tần số | Đóng góp? | 
| --- | --- | --- | 
| 1 | 1 | Có | 
| 2 | 2 | Có | 
| 3 | 3 | Có | 
| 7 | 1 | Không | 

Đáp án = 3. 

Đối với phạm vi`[3,4]`:```
[2, 2]
```| Giá trị | Tần số | Đóng góp? | 
| --- | --- | --- | 
| 2 | 2 | Có | 

Trả lời = 1. 

Đầu ra:```
3
1
```Ví dụ này thể hiện điều kiện cốt lõi. Một giá trị chỉ đóng góp khi tần số của nó bằng chính giá trị đó. 

### Ví dụ 2 

đầu vào:```
5 1
1 1 1 1 1
1 5
```Phạm vi hiện tại trở thành toàn bộ mảng. 

| Giá trị | Tần số | Đóng góp? | 
| --- | --- | --- | 
| 1 | 5 | Không | 

Trả lời:```
0
```Dấu vết cho thấy rằng việc khớp chính xác giá trị là quan trọng. Có tần số lớn hơn giá trị không được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m)√n) | Thuật toán của Mo thực hiện các chuyển động con trỏ O((n+m)√n), mỗi chuyển động được cập nhật trong O(1) | 
| Không gian | O(n + m) | Mảng tần số, lưu trữ truy vấn và lưu trữ câu trả lời | 

Với`n,m ≤ 100000`, chúng tôi có`√n ≈ 316`. Số lượng cập nhật thu được nằm trong giới hạn của sự cố Codeforces kéo dài 4 giây và mức sử dụng bộ nhớ ở mức dưới 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import math

    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    a = [x if x <= n else 0 for x in a]

    block = int(n ** 0.5) + 1

    queries = []
    for idx in range(m):
        l, r = map(int, input().split())
        queries.append((l - 1, r - 1, idx))

    queries.sort(key=lambda q: (q[0] // block, q[1]))

    cnt = [0] * (n + 1)
    ans = [0] * m
    cur_ans = 0

    def add(pos):
        nonlocal cur_ans
        v = a[pos]
        if v == 0:
            return
        if cnt[v] == v:
            cur_ans -= 1
        cnt[v] += 1
        if cnt[v] == v:
            cur_ans += 1

    def remove(pos):
        nonlocal cur_ans
        v = a[pos]
        if v == 0:
            return
        if cnt[v] == v:
            cur_ans -= 1
        cnt[v] -= 1
        if cnt[v] == v:
            cur_ans += 1

    lcur, rcur = 0, -1

    for l, r, idx in queries:
        while lcur > l:
            lcur -= 1
            add(lcur)

        while rcur < r:
            rcur += 1
            add(rcur)

        while lcur < l:
            remove(lcur)
            lcur += 1

        while rcur > r:
            remove(rcur)
            rcur -= 1

        ans[idx] = cur_ans

    return "\n".join(map(str, ans))

# provided sample
assert run(
"""7 2
3 1 2 2 3 3 7
1 7
3 4
"""
) == "3\n1"

# minimum size
assert run(
"""1 1
1
1 1
"""
) == "1"

# value larger than n
assert run(
"""3 1
100 100 100
1 3
"""
) == "0"

# all equal values
assert run(
"""5 1
2 2 2 2 2
1 5
"""
) == "0"

# exact frequency match
assert run(
"""4 1
2 2 1 3
1 2
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử đơn`[1]`|`1`| Hạn chế tối thiểu | 
| Tất cả giá trị`100`với`n=3`|`0`| Giá trị lớn hơn`n`không liên quan | 
|`[2,2,2,2,2]`|`0`| Tần số lớn hơn giá trị không được tính | 
| Phạm vi`[2,2]`|`1`| Khớp tần số chính xác | 
| Mẫu chính thức |`3,1`| Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét:```
3 1
100 100 100
1 3
```Từ`100 > n`, giá trị không bao giờ có thể xuất hiện chính xác`100`lần. Quá trình tiền xử lý chuyển đổi tất cả các mục thành`0`, quy trình cập nhật sẽ bỏ qua chúng và câu trả lời được duy trì vẫn còn`0`. 

Coi như:```
2 1
2 2
1 2
```Tần số của giá trị`2`chính xác là`2`. Trong quá trình chèn, số lượng thay đổi:```
0 -> 1 -> 2
```Khi nó đạt tới`2`, câu trả lời tăng lên một. Đầu ra cuối cùng là:```
1
```điều này đúng vì chỉ có giá trị`2`đủ điều kiện. 

Coi như:```
4 1
3 3 3 3
1 4
```Sự chuyển đổi tần số:```
0 -> 1 -> 2 -> 3 -> 4
```Ở tần số`3`, giá trị đóng góp. Ở tần số`4`, nó ngừng đóng góp. Logic cập nhật sẽ loại bỏ phần đóng góp trước khi tăng số lượng và chỉ khôi phục nó nếu sau đó sự bình đẳng vẫn được giữ nguyên. Câu trả lời cuối cùng là:```
0
```phù hợp chính xác với định nghĩa.
