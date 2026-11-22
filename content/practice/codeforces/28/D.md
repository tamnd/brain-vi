---
title: "CF 28D - Đừng sợ, DraviDe rất tốt bụng"
description: "Chúng tôi có một chuỗi xe tải cố định. Đối với mỗi chiếc xe tải, chúng tôi biết giá trị của nó, số người bên trong nó và hai hạn chế về nỗi sợ hãi."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "hashing"]
categories: ["algorithms"]
codeforces_contest: 28
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 28 (Codeforces format)"
rating: 2400
weight: 28
solve_time_s: 100
verified: true
draft: false
---
[CF 28D - Đừng sợ, DravDe rất tốt bụng](https://codeforces.com/problemset/problem/28/D) 

**Đánh giá:** 2400 
**Tas:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, băm 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi xe tải cố định. Đối với mỗi chiếc xe tải, chúng tôi biết giá trị của nó, số người bên trong nó và hai hạn chế về nỗi sợ hãi. 

Nếu một chiếc xe tải sống sót và đi vào đường hầm thì trong số tất cả các xe tải sống sót: 

- tổng số người trước đó phải bằng nhau`l`- tổng số người đứng sau nó phải bằng nhau`r`Chúng tôi có thể xóa bất kỳ tập hợp con xe tải nào, nhưng thứ tự tương đối của các xe tải còn lại không thể thay đổi. Mục tiêu của chúng tôi là tối đa hóa tổng giá trị của những chiếc xe tải còn sót lại. 

Chi tiết quan trọng là những hạn chế được thể hiện ở khía cạnh số người sống sót chứ không phải ở vị trí ban đầu. Nếu đoàn xe sống sót có xe tải chở hành khách:`c[a1], c[a2], ..., c[ak]`sau đó cho mỗi xe tải được chọn`ai`: 

-`c[a1] + ... + c[a(i-1)] = l[ai]`-`c[a(i+1)] + ... + c[ak] = r[ai]`Cộng cả hai phương trình sẽ được:`total_people = l[i] + c[i] + r[i]`Đây là quan sát cấu trúc đầu tiên. Mỗi chiếc xe tải sống sót phải thống nhất về tổng số người trong đoàn xe cuối cùng. 

Các ràng buộc đủ lớn để lập trình động bậc hai là không thể. Với`n = 10^5`, MỘT`O(n^2)`số lần chuyển tiếp sẽ đạt khoảng`10^10`hoạt động vượt xa giới hạn. Chúng ta cần một cái gì đó gần gũi`O(n log n)`. 

Phần khó khăn là các điều kiện mang tính toàn cầu. Một chiếc xe tải chỉ hợp lệ nếu tổng số người trước và sau nó khớp chính xác. Việc triển khai ngây thơ để kiểm tra xe tải một cách độc lập sẽ thất bại vì việc chọn một xe tải sẽ thay đổi tổng tiền tố mà mỗi xe tải sau này nhìn thấy. 

Hãy xem xét ví dụ này:```
3
5 2 0 2
5 2 2 0
100 1 0 0
```Chỉ riêng xe tải 3 có giá trị với tổng số người`1`. 

Xe tải 1 và 2 cùng nhau có giá trị với tổng số người`4`. 

Câu trả lời đúng chỉ là xe tải 3, vì giá trị`100`lớn hơn`10`. 

Một chiến lược tham lam luôn mở rộng một chuỗi tương thích sẽ giữ xe tải 1 và 2 một cách không chính xác. 

Một trường hợp tinh tế khác xuất hiện khi một số xe tải có cùng yêu cầu về tiền tố.```
4
10 1 0 2
1 1 0 2
10 1 1 1
10 1 2 0
```Xe tải 2 không bao giờ có thể cùng tồn tại với xe tải 3 vì cả hai đều yêu cầu các tiền tố không tương thích trong cùng một tổng. Một DP bất cẩn chỉ kiểm tra khả năng tương thích cục bộ có thể vô tình xâu chuỗi chúng lại với nhau. 

Ngoài ra còn có trường hợp không thể có hai xe tải cùng tồn tại.```
2
5 1 0 0
7 2 0 0
```Mỗi xe tải riêng lẻ tạo thành một đoàn xe hợp lệ. Đáp án chỉ nên chọn xe số 2 thôi. 

Điều này có nghĩa là DP đương nhiên phải cho phép bắt đầu một đoàn xe mới từ bất kỳ xe tải nào. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi tập hợp con xe tải, giữ trật tự, tính toán tổng tiền tố còn sót lại và xác minh mọi ràng buộc. Điều này hiệu quả vì định nghĩa về tính hợp lệ là rõ ràng, nhưng ngay cả`2^40`đã vô vọng rồi, và đây`n`đạt tới`10^5`. 

Một DP vũ phu thực tế hơn sẽ thử tất cả những người tiền nhiệm. Giả sử chúng ta xử lý xe tải từ trái sang phải và xác định:`dp[i] = best value of a valid convoy ending at i`Để chuyển từ`j`ĐẾN`i`, chúng ta sẽ cần: 

- cả hai xe tải đều thuộc cùng một tổng dân số 
- những người trước đây`i`bình đẳng với mọi người trước đây`j`cộng thêm`c[j]`Điều này đã gợi ý một cấu trúc chuỗi. Nếu xe tải`i`có yêu cầu tiền tố`l[i]`, thì tiền thân của nó phải thỏa mãn:`l[j] + c[j] = l[i]`và cả hai đều phải chia sẻ như nhau:`T = l + c + r`DP bậc hai kiểm tra tất cả các xe tải trước đó`j`cho mọi`i`. Điều đó mang lại`O(n^2)`chuyển tiếp vẫn chậm quá. 

Quan sát quan trọng là mọi chuyển đổi chỉ phụ thuộc vào hai giá trị: 

- tổng số đoàn xe`T`- dân số tiền tố cần thiết`l`Giả sử chúng ta biết đoàn xe tốt nhất với tổng chi phí cố định`T`kết thúc với dân số tiền tố`x`. Sau đó, một chiếc xe tải với: 

- tổng cộng`T`- tiền tố`l = x + c_prev`có thể gia hạn nó ngay lập tức. 

Điều này biến vấn đề thành trạng thái DP thưa thớt được lập chỉ mục bởi`(T, l)`. 

Đối với mỗi xe tải: 

- tính toán`T = l + c + r`- bất kỳ người tiền nhiệm nào cũng phải có điều tương tự`T`- tiền tố kết thúc của tiền tố trước phải bằng`l`Vì vậy, chúng tôi duy trì:`best[T][prefix] = best achievable state`Mỗi trạng thái lưu trữ giá trị tối đa và thông tin tái thiết. 

Sử dụng bản đồ băm cho kết quả trung bình`O(1)`chuyển tiếp, do đó toàn bộ giải pháp trở thành tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(n²) | O(n) | Quá chậm | 
| Hash-DP tối ưu | O(n) trung bình | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xe xử lý từ trái sang phải. 

Trật tự rất quan trọng vì trật tự đoàn xe không thể thay đổi. Bất kỳ tiền thân hợp lệ nào của xe tải`i`phải xuất hiện sớm hơn trong đầu vào. 
2. Với mỗi xe tải, hãy tính:```
T = l[i] + c[i] + r[i]
```Tất cả các xe tải trong cùng một đoàn xe còn sống sót phải chia sẻ tổng dân số chính xác này. 
3. Duy trì bản đồ băm cho các trạng thái DP. 

Đối với mỗi tổng số`T`, chúng tôi lưu trữ một bản đồ băm khác được lập chỉ mục theo quần thể tiền tố. 

Một trạng thái đại diện cho:```
best[T][p]
```nghĩa là đoàn xe tốt nhất có tổng dân số là`T`và dân số tiền tố hiện tại của nó bằng`p`. 
4. Giải thích ý nghĩa của tiền tố một cách cẩn thận. 

Nếu một đoàn xe kết thúc ở xe tải`j`, thì sau khi lấy nó, tổng số người tích lũy trong đoàn trở thành:```
l[j] + c[j]
```Điều đó trở thành tiền tố được yêu cầu bởi chiếc xe tải tương thích tiếp theo. 
5. Đối với xe tải`i`, hãy tìm trạng thái tiền thân:```
best[T][l[i]]
```bởi vì bất kỳ đoàn xe nào trước đó được kéo dài bằng xe tải`i`phải chứa chính xác`l[i]`mọi người. 
6. Nếu trạng thái đó tồn tại, hãy mở rộng nó:```
new_value = previous_value + v[i]
```Nếu không thì xe tải`i`chỉ có thể bắt đầu một đoàn xe mới nếu`l[i] == 0`. 

Một đoàn xe khởi đầu bằng xe tải`i`không có người trước nó. 
7. Đoàn xe mới kết thúc với tiền tố tích lũy:```
l[i] + c[i]
```Cập nhật:```
best[T][l[i] + c[i]]
```nếu giá trị mới tốt hơn. 
8. Lưu trữ con trỏ gốc trong quá trình cập nhật. 

Để tái thiết, chúng tôi giữ: 

- trạng thái trước đó 
- chỉ số xe tải trước 
9. Trong số tất cả các trạng thái có tiền tố tích lũy bằng`T`, chọn giá trị lớn nhất 

Một đoàn xe chỉ hoàn thành khi tổng số người tích lũy bằng tổng số đoàn xe. 

### Tại sao nó hoạt động 

Mỗi đoàn xe còn sống sót xác định một chuỗi các quần thể tiền tố tăng dần nghiêm ngặt:```
0,
c[a1],
c[a1]+c[a2],
...
```Dành cho xe tải`ai`, những người đứng trước nó bằng chính xác dân số tích lũy trước khi thêm hành khách của chính nó. Giá trị đó là`l[ai]`. 

trạng thái DP`best[T][p]`đại diện cho đoàn xe từng phần tối ưu với tổng dân số`p`đã được tích lũy bên trong một đoàn xe mà tổng số cuối cùng phải là`T`. 

Việc chuyển đổi là hợp pháp chính xác khi xe tải tiếp theo yêu cầu`l[i] = p`. Vì tất cả các xe tải trong đoàn đều có chung tổng`T`, mọi chuỗi được xây dựng đều tự động thỏa mãn cả điều kiện sợ trước và sau. 

Không có đoàn xe hợp lệ nào bị bỏ sót vì mỗi đoàn xe có thể được xây dựng lại một cách duy nhất bằng cách tuân theo tổng tiền tố tăng dần của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    trucks = [None]

    for _ in range(n):
        v, c, l, r = map(int, input().split())
        trucks.append((v, c, l, r))

    # dp[T][prefix] = (best_value, state_id)
    dp = {}

    # state:
    # value, previous_state, truck_index, T, prefix
    states = [None]

    best_final = -1
    best_state = -1

    for i in range(1, n + 1):
        v, c, l, r = trucks[i]

        T = l + c + r

        if T not in dp:
            dp[T] = {}

        cur_map = dp[T]

        best_val = -1
        prev_state = -1

        if l == 0:
            best_val = v

        if l in cur_map:
            prev_val, st_id = cur_map[l]

            if prev_val + v > best_val:
                best_val = prev_val + v
                prev_state = st_id

        if best_val == -1:
            continue

        new_prefix = l + c

        new_state_id = len(states)
        states.append((best_val, prev_state, i, T, new_prefix))

        if (
            new_prefix not in cur_map
            or best_val > cur_map[new_prefix][0]
        ):
            cur_map[new_prefix] = (best_val, new_state_id)

        if new_prefix == T and best_val > best_final:
            best_final = best_val
            best_state = new_state_id

    ans = []

    while best_state != -1:
        value, prev_state, truck_idx, T, prefix = states[best_state]
        ans.append(truck_idx)
        best_state = prev_state

    ans.reverse()

    print(len(ans))
    print(*ans)

solve()
```Ý tưởng trung tâm trong việc thực hiện là việc giải thích trạng thái DP. 

Đối với tổng dân số cố định`T`, chìa khóa xác định tiến độ bên trong đoàn xe là dân số tích lũy cho đến nay. Sau khi chọn xe tải có thông số`(c, l)`, dân số tích lũy trở thành`l + c`. 

Bước chuyển tiếp:```
if l in cur_map:
```có nghĩa là chúng tôi đã xây dựng một đoàn xe hợp lệ chứa chính xác`l`mọi người. Xe tải`i`hợp pháp có thể đến tiếp theo bởi vì sự hạn chế nỗi sợ hãi của nó đòi hỏi chính xác`l`người trước nó. 

Việc khởi tạo:```
if l == 0:
    best_val = v
```cho phép một đoàn xe xuất phát từ chiếc xe tải này. 

Một điểm tinh tế là chúng tôi cập nhật trạng thái cho`new_prefix = l + c`, không dành cho`l`. Trạng thái mới mô tả đoàn xe sau khi đưa chiếc xe tải hiện tại vào. 

Một lỗi dễ mắc phải khác là quên rằng nhiều đường dẫn có thể dẫn đến cùng một`(T, prefix)`tình trạng. Chúng tôi chỉ giữ lại giá trị tối đa vì các chuyển đổi trong tương lai chỉ phụ thuộc vào hai số này. Bất kỳ trạng thái yếu hơn nào cũng không bao giờ có thể trở thành trạng thái tối ưu sau này. 

Việc xây dựng lại lưu trữ ID trạng thái gốc thay vì sao chép mảng chỉ mục. Điều này giữ cho bộ nhớ tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 0 3
1 1 1 2
1 1 2 1
1 1 3 0
2 1 3 0
```Tất cả các xe tải đều có:```
T = 4
```| Xe tải | tôi | c | r | Trạng thái cần thiết | Tiền tố mới | Giá trị tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 3 | bắt đầu | 1 | 1 | 
| 2 | 1 | 1 | 2 | tiền tố 1 | 2 | 2 | 
| 3 | 2 | 1 | 1 | tiền tố 2 | 3 | 3 | 
| 4 | 3 | 1 | 0 | tiền tố 3 | 4 | 4 | 
| 5 | 3 | 1 | 0 | tiền tố 3 | 4 | 5 | 

Xe tải 5 tạo ra giá trị cuối cùng tốt hơn xe tải 4, do đó đoàn xe tối ưu trở thành:```
1 2 3 5
```Dấu vết này thể hiện ý nghĩa của trạng thái DP. Mỗi xe tải mở rộng đoàn xe có dân số tích lũy bằng với tiền tố bắt buộc của nó. 

### Ví dụ 2 

đầu vào:```
3
5 2 0 2
5 2 2 0
100 1 0 0
```| Xe tải | T | Trạng thái cần thiết | Tiền tố mới | Giá trị tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | bắt đầu | 2 | 5 | 
| 2 | 4 | tiền tố 2 | 4 | 10 | 
| 3 | 1 | bắt đầu | 1 | 100 | 

Hai tổng số riêng biệt tồn tại. 

Vì`T = 4`, đoàn xe tốt nhất có giá trị`10`. 

Vì`T = 1`, đoàn xe tải đơn có giá trị`100`. 

Thuật toán so sánh chính xác các đoàn xe đã hoàn thành trên tất cả các tổng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trung bình | Mỗi xe tải thực hiện một số hoạt động bản đồ băm không đổi | 
| Không gian | O(n) | Một trạng thái DP cho mỗi lần chuyển đổi thành công | 

Giải pháp dễ dàng phù hợp với giới hạn. Với`10^5`xe tải, xử lý tuyến tính với bản đồ băm cũng nằm trong giới hạn thời gian 2 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    sys.stdout = out

    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())

        trucks = [None]

        for _ in range(n):
            v, c, l, r = map(int, input().split())
            trucks.append((v, c, l, r))

        dp = {}
        states = [None]

        best_final = -1
        best_state = -1

        for i in range(1, n + 1):
            v, c, l, r = trucks[i]

            T = l + c + r

            if T not in dp:
                dp[T] = {}

            cur_map = dp[T]

            best_val = -1
            prev_state = -1

            if l == 0:
                best_val = v

            if l in cur_map:
                prev_val, st_id = cur_map[l]

                if prev_val + v > best_val:
                    best_val = prev_val + v
                    prev_state = st_id

            if best_val == -1:
                continue

            new_prefix = l + c

            new_state_id = len(states)
            states.append((best_val, prev_state, i))

            if (
                new_prefix not in cur_map
                or best_val > cur_map[new_prefix][0]
            ):
                cur_map[new_prefix] = (best_val, new_state_id)

            if new_prefix == T and best_val > best_final:
                best_final = best_val
                best_state = new_state_id

        ans = []

        while best_state != -1:
            value, prev_state, truck_idx = states[best_state]
            ans.append(truck_idx)
            best_state = prev_state

        ans.reverse()

        print(len(ans))
        print(*ans)

    solve()

    return out.getvalue().strip()

# provided sample
assert run(
"""5
1 1 0 3
1 1 1 2
1 1 2 1
1 1 3 0
2 1 3 0
"""
) == "4\n1 2 3 5", "sample 1"

# minimum size
assert run(
"""1
10 5 0 0
"""
) == "1\n1", "single truck"

# no compatible pairs
assert run(
"""2
5 1 0 0
7 2 0 0
"""
) == "1\n2", "choose larger single truck"

# chain of three
assert run(
"""3
3 1 0 2
4 1 1 1
5 1 2 0
"""
) == "3\n1 2 3", "full chain"

# competing totals
assert run(
"""3
5 2 0 2
5 2 2 0
100 1 0 0
"""
) == "1\n3", "better separate convoy"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xe tải hợp lệ duy nhất |`1 1`| Khởi tạo trường hợp cơ sở | 
| Hai chiếc xe tải không tương thích | Chỉ xe tải có giá trị lớn hơn | Khởi động đoàn xe mới đúng cách | 
| Chuỗi tương thích đầy đủ | Tất cả các xe tải được chọn | Chuyển tiếp DP | 
| Tổng số cạnh tranh | Tổng số tốt nhất được chọn trên toàn cầu | Tách bằng`T`| 

## Vỏ cạnh 

Hãy xem xét trường hợp mỗi chiếc xe tải đều có giá trị riêng lẻ nhưng không tương thích với tất cả những chiếc xe tải khác.```
2
5 1 0 0
7 2 0 0
```Xe tải 1 tạo trạng thái:```
T = 1
prefix = 1
value = 5
```Xe tải 2 tạo ra:```
T = 2
prefix = 2
value = 7
```Không thể mở rộng cái kia vì tổng số khác nhau. Thuật toán giữ cả hai đoàn xe đã hoàn thành và cuối cùng chọn giá trị lớn hơn, chỉ tạo ra xe tải 2. 

Bây giờ hãy xem xét các yêu cầu tiền tố không tương thích.```
4
10 1 0 2
1 1 0 2
10 1 1 1
10 1 2 0
```Cả xe tải 1 và xe tải 2 đều tạo ra các trạng thái với:```
T = 3
prefix = 1
```DP chỉ giữ chiếc tốt hơn, xe tải 1 có giá trị`10`. 

Xe tải 3 mở rộng trạng thái đó:```
value = 20
prefix = 2
```Xe tải 4 lại kéo dài:```
value = 30
prefix = 3
```Trạng thái yếu hơn thông qua xe tải 2 không bao giờ tồn tại vì những chuyển đổi trong tương lai chỉ phụ thuộc vào`(T, prefix)`. Giữ giá trị tối đa là đủ. 

Cuối cùng, hãy xem xét nhiều đoàn xe đã hoàn thành với tổng số khác nhau.```
3
5 2 0 2
5 2 2 0
100 1 0 0
```Thuật toán xây dựng độc lập: 

- đoàn xe`{1,2}`có giá trị`10`- đoàn xe`{3}`có giá trị`100`Vì các trạng thái đã hoàn thành được so sánh trên toàn cầu nên câu trả lời đúng chỉ là xe tải 3.
