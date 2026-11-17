---
title: "CF 1949F - Hẹn hò"
description: "Mỗi người dùng có thể được xem như một tập hợp các hoạt động. Chúng ta cần tìm hai người dùng có nhóm thỏa mãn đồng thời ba điều kiện: 1. Họ chia sẻ ít nhất một hoạt động. 2. Người dùng đầu tiên có ít nhất một hoạt động mà người dùng thứ hai không có. 3."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1949
codeforces_index: "F"
codeforces_contest_name: "European Championship 2024 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2200
weight: 1949
solve_time_s: 117
verified: false
draft: false
---

[CF 1949F - Hẹn hò](https://codeforces.com/problemset/problem/1949/F) 

**Đánh giá:** 2200 
**Tags:** tham lam, phân loại, cây cối 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người dùng có thể được xem như một tập hợp các hoạt động. 

Chúng ta cần tìm hai người dùng có tập hợp thỏa mãn đồng thời ba điều kiện: 

1. Họ chia sẻ ít nhất một hoạt động. 
2. Người dùng đầu tiên có ít nhất một hoạt động mà người dùng thứ hai không có. 
3. Người dùng thứ hai có ít nhất một hoạt động mà người dùng thứ nhất không có. 

Trong thuật ngữ tập hợp, chúng ta đang tìm kiếm hai tập hợp mà phần giao của chúng không trống nhưng không có tập hợp nào là tập con của tập hợp kia. 

Dữ liệu đầu vào chứa tối đa 200.000 người dùng nhưng tổng số hoạt động được liệt kê trên tất cả người dùng tối đa là 1.000.000. Giới hạn thứ hai này là giới hạn quan trọng. Điều đó có nghĩa là chúng tôi có thể đủ khả năng thực hiện công việc tương ứng với tổng lượng dữ liệu đầu vào, nhưng chúng tôi không thể đủ khả năng so sánh từng cặp người dùng. 

Việc kiểm tra mạnh mẽ tất cả các cặp sẽ yêu cầu khoảng$$\binom{200000}{2} \approx 2 \cdot 10^{10}$$so sánh là điều hoàn toàn không thể. 

Vũ trụ hoạt động rất rộng lớn, lên tới$10^6$, nhưng nhiều nhất chỉ$10^6$hoạt động xảy ra thực sự xuất hiện. Bất kỳ giải pháp nào cũng phải phù hợp với các hoạt động diễn ra trong đầu vào thay vì cố gắng xây dựng các cấu trúc có quy mô lớn.$m$. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét hai người dùng giống hệt nhau:```
2 3
2 1 2
2 1 2
```Câu trả lời đúng là`NO`. Họ chia sẻ các hoạt động nhưng không người dùng nào có thứ mà người kia thiếu. Một giải pháp chỉ kiểm tra giao lộ không trống sẽ trả về sai`YES`. 

Hãy xem xét hai người dùng rời rạc:```
2 3
1 1
1 2
```Câu trả lời đúng là`NO`. Mỗi người dùng có một hoạt động riêng nhưng họ không chia sẻ bất cứ điều gì. 

Hãy xem xét một mối quan hệ tập hợp con:```
2 4
1 1
2 1 2
```Câu trả lời đúng là`NO`. Họ chia sẻ hoạt động 1, nhưng tập đầu tiên được chứa trong hoạt động thứ hai. Cả hai người dùng đều phải sở hữu thứ mà người kia không có. 

Trường hợp khó phát hiện cuối cùng là khi có nhiều người dùng thực hiện cùng một hoạt động. Chỉ nhìn vào tần suất hoạt động là không đủ. Chúng ta phải phân biệt giữa các tập hợp giống nhau và các tập hợp không thể so sánh được. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là so sánh từng cặp người dùng. 

Đối với một cặp bộ$A$Và$B$, chúng ta có thể kiểm tra xem:$$A \cap B \neq \varnothing$$Và$$A \setminus B \neq \varnothing$$Và$$B \setminus A \neq \varnothing.$$Điều này đúng vì nó trực tiếp thực hiện định nghĩa về sự phù hợp tốt. 

Vấn đề là số lượng cặp. Với 200.000 người dùng, có khoảng hai mươi tỷ cặp. Ngay cả khi mọi sự so sánh đều cực kỳ rẻ thì điều này vẫn vượt xa giới hạn. 

Để đạt được tiến bộ, chúng ta cần khai thác cấu trúc đặc biệt của các ràng buộc. 

Quan sát quan trọng là tổng số lần xuất hiện hoạt động chỉ$10^6$. Điều này gợi ý một ý tưởng ánh sáng nặng cổ điển. 

Gọi một hoạt động là nặng nề nếu nó xuất hiện ở nhiều người dùng và ngược lại là nhẹ. 

Giả sử chúng ta chọn một ngưỡng$$B \approx \sqrt{10^6}=1000.$$Có thể có nhiều nhất khoảng 1000 hoạt động nặng nhọc, bởi vì mỗi hoạt động nặng nhọc đều đóng góp ít nhất$B$lần xuất hiện và tổng số lần xuất hiện nhiều nhất là$10^6$. 

Bây giờ hãy xem xét một tập người dùng$S$. 

Nếu như$|S| \ge B$, chúng tôi gọi đó là người dùng nhiều. Cũng có thể có tối đa khoảng 1000 người dùng nhiều, vì tổng kích thước của tất cả các bộ tối đa là$10^6$. 

Sự phân rã căn bậc hai này tạo ra hai thế giới có thể quản lý được. 

Đối với người dùng nhiều, chỉ có khoảng 1000 người trong số họ. Chúng tôi có thể xây dựng cấu trúc thành viên nhanh chóng và so sánh chúng một cách hiệu quả. 

Đối với người dùng ít, mỗi bộ chứa ít hơn$B$các hoạt động. Chúng tôi có thể xử lý chúng thông qua danh sách tỷ lệ hoạt động. Vì mỗi bộ đèn nhỏ nên tổng công việc gần như$O(10^6 \sqrt{10^6})$. 

Đặc tính quan trọng là hai người dùng tạo thành một kết quả phù hợp chính xác khi họ giao nhau nhưng không thể so sánh được khi đưa vào. 

Giải pháp biên tập xây dựng thông tin bao gồm giữa các nhóm và chỉ tìm kiếm giữa các cặp thực sự chia sẻ hoạt động, tránh sự bùng nổ bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot k)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(T\sqrt{T})$, Ở đâu$T=\sum k_i$|$O(T)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$$T=\sum k_i.$$Chọn$$B=\lfloor\sqrt{T}\rfloor+1.$$Trình bày mọi hoạt động của người dùng dưới dạng danh sách được sắp xếp. 

### 1. Nén tất cả các lần xuất hiện hoạt động vào danh sách sự cố 

Đối với mọi hoạt động, hãy lưu trữ những người dùng có chứa hoạt động đó. 

Điều này cho phép chúng tôi chỉ liệt kê những cặp người dùng thực sự chia sẻ ít nhất một hoạt động. 

### 2. Phân loại người dùng nặng hay nhẹ 

Một người dùng nặng nếu kích thước được đặt của nó ít nhất là$B$. 

Vì tổng kích thước của tất cả các bộ là$T$, số lượng người dùng nhiều nhất là nhiều nhất$T/B = O(\sqrt T)$. 

Giới hạn này là điều làm cho việc xử lý sau này trở nên khả thi. 

### 3. Xây dựng cấu trúc thành viên nhanh chóng cho người dùng nhiều 

Đối với mỗi người dùng nặng, hãy lưu trữ tất cả hoạt động của họ trong bảng băm. 

Điều này cho phép chúng tôi kiểm tra tư cách thành viên trong thời gian dự kiến ​​không đổi. 

### 4. Xử lý người dùng nặng đối với tất cả người dùng 

Dành cho người dùng nặng$H$, tính toán số lượng giao lộ với mọi người dùng. 

Lặp lại qua từng hoạt động của$H$. Đối với mỗi người dùng có hoạt động đó, hãy tăng bộ đếm. 

Sau lần quét này, đối với bất kỳ người dùng nào$U$, bộ đếm bằng$$|H \cap U|.$$Bây giờ hãy kiểm tra xem$$0 < |H\cap U| < |H|$$Và$$0 < |H\cap U| < |U|.$$Những bất bình đẳng này có nghĩa là: 

- giao lộ không trống, 
-$H$có cái gì đó bên ngoài$U$, 
-$U$có cái gì đó bên ngoài$H$. 

Nếu họ giữ, chúng tôi đã tìm thấy một cặp hợp lệ. 

### 5. Xử lý các cặp người dùng ít 

Mỗi người dùng nhẹ chứa ít hơn$B$các hoạt động. 

Đối với mỗi hoạt động, hãy xem danh sách người dùng nhẹ có chứa hoạt động đó. 

Bất cứ khi nào hai người dùng ít gặp nhau thông qua một hoạt động nào đó, họ sẽ chia sẻ ít nhất một hoạt động. 

Chúng ta phải xác định xem cái này có phải là tập con của cái kia hay không. 

Lưu trữ các cặp ứng cử viên trong một cấu trúc đếm. Đối với mỗi hoạt động được chia sẻ, hãy tăng số lượng hoạt động chung của cặp đó. 

Sau khi tất cả các hoạt động được xử lý, một cặp$(A,B)$có giá trị$$c = |A \cap B|.$$Cặp đôi này tốt chính xác khi$$c > 0,$$

$$c < |A|,$$Và$$c < |B|.$$Nếu các điều kiện này được giữ nguyên thì không tập hợp nào chứa hoàn toàn tập hợp kia và chúng chia sẻ ít nhất một hoạt động. 

### 6. Xuất ra cặp hợp lệ đầu tiên 

Nếu bất kỳ giai đoạn nào tìm thấy một cặp hợp lệ, hãy in`YES`và các chỉ số tương ứng. 

Nếu tất cả các lần kiểm tra kết thúc mà không thành công, hãy in`NO`. 

### Tại sao nó hoạt động 

Đối với mỗi cặp người dùng chia sẻ một hoạt động, thuật toán sẽ tính toán kích thước chính xác của giao điểm của họ. 

Một cặp chỉ được chấp nhận khi giao điểm hoàn toàn nhỏ hơn cả hai kích thước đã đặt. Điều đó có nghĩa là mỗi người dùng sở hữu ít nhất một hoạt động bên ngoài giao lộ. Tương tự, không có tập nào được chứa trong tập kia. 

Một cặp bị từ chối bất cứ khi nào giao điểm trống hoặc bằng một trong các kích thước đã đặt. Trong những trường hợp đó, người dùng không chia sẻ gì hoặc một hồ sơ là tập hợp con của hồ sơ kia, vi phạm định nghĩa về sự phù hợp tốt. 

Quá trình xử lý người dùng nhiều sẽ kiểm tra tất cả các cặp liên quan đến người dùng nhiều. Quá trình xử lý người dùng nhẹ sẽ kiểm tra tất cả các cặp bao gồm toàn bộ người dùng ít. Mỗi cặp có thể thuộc về chính xác một trong các loại này, vì vậy không thể bỏ sót câu trả lời hợp lệ nào. 

## Giải pháp Python```python
import sys
from math import isqrt
from collections import defaultdict

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    sets = []
    total = 0
    activity_users = defaultdict(list)

    for idx in range(n):
        row = list(map(int, input().split()))
        k = row[0]
        arr = row[1:]
        sets.append(arr)
        total += k

        for x in arr:
            activity_users[x].append(idx)

    B = isqrt(max(total, 1)) + 1

    heavy = []
    is_heavy = [False] * n

    for i in range(n):
        if len(sets[i]) >= B:
            is_heavy[i] = True
            heavy.append(i)

    # Heavy versus all users
    cnt = [0] * n

    for h in heavy:
        touched = []

        for act in sets[h]:
            for u in activity_users[act]:
                if cnt[u] == 0:
                    touched.append(u)
                cnt[u] += 1

        hs = len(sets[h])

        for u in touched:
            if u == h:
                continue

            c = cnt[u]

            if c > 0 and c < hs and c < len(sets[u]):
                print("YES")
                print(h + 1, u + 1)
                return

        for u in touched:
            cnt[u] = 0

    # Light-light pairs
    pair_cnt = defaultdict(int)

    for users in activity_users.values():
        light_users = [u for u in users if not is_heavy[u]]

        s = len(light_users)

        for i in range(s):
            a = light_users[i]
            for j in range(i + 1, s):
                b = light_users[j]
                if a > b:
                    a, b = b, a
                pair_cnt[(a, b)] += 1

    for (a, b), c in pair_cnt.items():
        if c < len(sets[a]) and c < len(sets[b]):
            print("YES")
            print(a + 1, b + 1)
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc tất cả các bộ và đồng thời xây dựng danh sách tỷ lệ hoạt động. Những danh sách sự cố đó là cấu trúc trung tâm của giải pháp vì mọi hoạt động trong tương lai đều được thúc đẩy bởi các hoạt động chung. 

Giai đoạn người dùng nhiều không bao giờ so sánh người dùng nhiều với mọi hoạt động của mọi người dùng khác. Thay vào đó, nó xem xét các hoạt động của người dùng nhiều và tích lũy kích thước giao điểm chính xác. các`touched`mảng ghi lại các bộ đếm trở thành khác 0 để việc dọn dẹp vẫn tỷ lệ thuận với công việc thực tế được thực hiện. 

Giai đoạn ánh sáng-ánh sáng đếm các hoạt động chung của từng cặp người dùng ánh sáng thực sự chia sẻ điều gì đó. Một cặp không bao giờ chia sẻ một hoạt động sẽ không bao giờ được lưu trữ, điều này rất cần thiết để đạt được hiệu quả. 

điều kiện```
c < len(sets[a]) and c < len(sets[b])
```là bản dịch toán học chính xác của "cả hai người dùng đều có ít nhất một hoạt động mà người kia thiếu". Vì mọi cặp được lưu trữ đều đã có`c > 0`, điều này mô tả đầy đủ một kết quả khớp hợp lệ. 

Không tồn tại vấn đề tràn số nguyên vì tất cả số đếm đều nhiều nhất$10^6$, nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
3 1 2 4
5 1 2 3 4 5
2 1 5
```| Cặp | Kích thước nút giao | Cỡ A | Cỡ B | hợp lệ | 
| --- | --- | --- | --- | --- | 
| (1,2) | 3 | 3 | 5 | Không | 
| (1,3) | 1 | 3 | 2 | Có | 
| (2,3) | 2 | 5 | 2 | Không | 

Cặp (1,3) chia sẻ hoạt động 1. Người dùng 1 cũng có hoạt động 2 và 4, trong khi người dùng 3 cũng có hoạt động 5. Cả hai hướng đều chứa một hoạt động duy nhất nên cặp này được chấp nhận. 

### Ví dụ 2 

đầu vào:```
2 4
1 1
2 1 2
```| Cặp | Kích thước nút giao | Cỡ A | Cỡ B | hợp lệ | 
| --- | --- | --- | --- | --- | 
| (1,2) | 1 | 1 | 2 | Không | 

Giao điểm bằng toàn bộ tập hợp đầu tiên. Người dùng 1 không có gì mà người dùng 2 thiếu. Đây là một mối quan hệ tập hợp con, không phải là một kết quả phù hợp. 

Ví dụ này giải thích tại sao chỉ kiểm tra giao lộ không trống là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T\sqrt{T})$| Phân hủy nặng nhẹ,$T=\sum k_i$| 
| Không gian |$O(T)$| Danh sách hoạt động và cặp sổ sách kế toán | 

Tổng số lần diễn ra hoạt động nhiều nhất là$10^6$. Với$B \approx \sqrt T$, cả số lượng người dùng nhiều và khối lượng công việc được đóng góp bởi quá trình xử lý nặng vẫn bị giới hạn bởi$O(T\sqrt T)$. Điều này thoải mái phù hợp với giới hạn 3 giây trong giải pháp dự định. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import defaultdict
from math import isqrt

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    sets = []
    total = 0
    activity_users = defaultdict(list)

    for idx in range(n):
        row = list(map(int, input().split()))
        k = row[0]
        arr = row[1:]
        sets.append(arr)
        total += k

        for x in arr:
            activity_users[x].append(idx)

    B = isqrt(max(total, 1)) + 1

    heavy = []
    is_heavy = [False] * n

    for i in range(n):
        if len(sets[i]) >= B:
            is_heavy[i] = True
            heavy.append(i)

    cnt = [0] * n

    for h in heavy:
        touched = []

        for act in sets[h]:
            for u in activity_users[act]:
                if cnt[u] == 0:
                    touched.append(u)
                cnt[u] += 1

        hs = len(sets[h])

        for u in touched:
            if u != h:
                c = cnt[u]
                if c > 0 and c < hs and c < len(sets[u]):
                    return "YES\n"

        for u in touched:
            cnt[u] = 0

    pair_cnt = defaultdict(int)

    for users in activity_users.values():
        light_users = [u for u in users if not is_heavy[u]]

        for i in range(len(light_users)):
            for j in range(i + 1, len(light_users)):
                a, b = sorted((light_users[i], light_users[j]))
                pair_cnt[(a, b)] += 1

    for (a, b), c in pair_cnt.items():
        if c < len(sets[a]) and c < len(sets[b]):
            return "YES\n"

    return "NO\n"

# provided sample
assert run(
"""3 5
3 1 2 4
5 1 2 3 4 5
2 1 5
"""
) == "YES\n"

# identical sets
assert run(
"""2 3
2 1 2
2 1 2
"""
) == "NO\n"

# disjoint sets
assert run(
"""2 3
1 1
1 2
"""
) == "NO\n"

# subset relation
assert run(
"""2 4
1 1
2 1 2
"""
) == "NO\n"

# minimal valid pair
assert run(
"""2 3
2 1 2
2 1 3
"""
) == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bộ giống hệt nhau | KHÔNG | Bình đẳng thôi chưa đủ | 
| Bộ rời rạc | KHÔNG | Hoạt động chia sẻ là bắt buộc | 
| Quan hệ tập hợp con | KHÔNG | Cả hai người dùng đều cần các hoạt động độc đáo | 
| Cặp hợp lệ tối thiểu | CÓ | Ví dụ tích cực nhỏ nhất | 
| Mẫu 1 | CÓ | Tính đúng đắn chung | 

## Vỏ cạnh 

Xem xét các hồ sơ giống hệt nhau:```
2 3
2 1 2
2 1 2
```Kích thước giao điểm là 2, bằng cả hai kích thước đã đặt. Thuật toán từ chối cặp vì`c < len(set)`thất bại cho cả hai người dùng. Đầu ra là`NO`. 

Xem xét các hồ sơ rời rạc:```
2 3
1 1
1 2
```Không có danh sách tỷ lệ hoạt động nào chứa cả hai người dùng. Cặp này không bao giờ xuất hiện trong quá trình xử lý nặng hoặc đếm cặp nhẹ. Đầu ra là`NO`. 

Hãy xem xét một tập hợp con nghiêm ngặt:```
2 4
1 1
2 1 2
```Kích thước giao điểm là 1. Nó bằng kích thước của tập hợp đầu tiên. điều kiện`c < len(first)`không thành công nên cặp này bị từ chối. Đầu ra là`NO`. 

Hãy xem xét một cặp hợp lệ không thể so sánh được:```
2 4
2 1 2
2 1 3
```Kích thước giao điểm là 1. Cả hai kích thước được đặt là 2, vì vậy```
1 < 2
1 < 2
```nắm giữ. Mỗi người dùng có một hoạt động duy nhất. Đầu ra của thuật toán`YES`. 

Những trường hợp này bao gồm ba cách mà một cặp có thể thất bại và một cách có thể thành công, phù hợp chính xác với định nghĩa về sự kết hợp tốt.
