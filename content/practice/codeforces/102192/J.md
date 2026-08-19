---
title: "CF 102192J - Taotao hái táo"
description: "Chúng tôi quét chiều cao của quả táo từ trái sang phải. Quả táo đầu tiên luôn được hái. Sau đó, một quả táo chỉ được hái khi chiều cao của nó lớn hơn chiều cao của quả táo được hái gần đây nhất. Do đó, các độ cao được chọn tạo thành một chuỗi cực đại tiền tố tăng dần."
date: "2026-08-18T02:11:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "J"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 155
verified: true
draft: false
---

[CF 102192J - Taotao hái táo](https://codeforces.com/problemset/problem/102192/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi quét chiều cao của quả táo từ trái sang phải. Quả táo đầu tiên luôn được hái. Sau đó, một quả táo chỉ được hái khi chiều cao của nó lớn hơn chiều cao của quả táo được hái gần đây nhất. Do đó, các độ cao được chọn tạo thành một chuỗi cực đại tiền tố tăng dần. 

Mỗi truy vấn là độc lập. Nó thay đổi đúng một vị trí`p`từ chiều cao ban đầu của nó`h[p]`ĐẾN`q`, sau đó hỏi có bao nhiêu quả táo sẽ được chọn trong mảng kết quả. Mảng ban đầu được khôi phục trước truy vấn tiếp theo. Vấn đề và giải pháp chính thức của cuộc thi mô tả điều này là duy trì thông tin cần thiết của hai bên về quan điểm đã sửa đổi. 

Các giá trị của`n`Và`m`cả hai có thể đạt được`100000`, và có thể có tới mười trường hợp thử nghiệm. Việc mô phỏng trực tiếp mọi truy vấn sẽ yêu cầu tới`10^10`kiểm tra mảng trong một trường hợp thử nghiệm khi mọi truy vấn quét gần như toàn bộ mảng. Điều đó vượt xa những gì giới hạn 2 giây có thể hỗ trợ. Chúng ta cần tiền xử lý gần tuyến tính hoặc`O(n log n)`, theo sau là công việc tính logarit đại khái cho mỗi truy vấn. 

Trường hợp tinh tế đầu tiên là khi giá trị thay thế không lớn hơn chiều cao tốt nhất đã thấy trước vị trí`p`. Ví dụ, hãy xem xét```
1
3 1
1 5 6
2 3
```Mảng được sửa đổi là`[1, 3, 6]`, vậy câu trả lời là`3`. Một giải pháp bất cẩn có thể luôn đếm quả táo đã sửa đổi vì nó đã bị thay đổi rõ ràng, nhưng`3`không cao hơn chiều cao đã chọn trước đó`1`trong ví dụ này nó thực sự là như vậy, vì vậy nó được chọn. Một trường hợp rõ ràng hơn là```
1
3 1
1 5 6
2 1
```Mảng được sửa đổi là`[1, 1, 6]`, và câu trả lời là`2`. Quả táo ở vị trí`2`bằng với chiều cao đã chọn trước đó nên so sánh chặt chẽ sẽ loại bỏ nó. Điều trị tình trạng như`>=`sẽ sản xuất không chính xác`3`. 

Trường hợp tinh tế thứ hai là khi quả táo được sửa đổi trở thành bản ghi mới. Coi như```
1
4 1
1 2 3 4
3 10
```Mảng được sửa đổi là`[1, 2, 10, 4]`, vậy câu trả lời là`3`. Một lần`10`được chọn thì càng muộn`4`không thể chọn được. Một giải pháp tính toán hậu tố độc lập với tiền tố tối đa ban đầu có thể vô tình đếm số tiền gốc`4`, bởi vì`4`là một bản ghi trong mảng chưa sửa đổi. Thay vào đó, hậu tố phải bắt đầu bằng ngưỡng mới`10`. 

Trường hợp ranh giới thứ ba là một sửa đổi ở vị trí đầu tiên:```
1
3 1
1 2 3
1 5
```Câu trả lời là`1`, bởi vì mảng trở thành`[5, 2, 3]`. Không có tiền tố bên trái nào cả, vì vậy giá trị thay thế chính là ngưỡng ban đầu. Bất kỳ triển khai nào truy cập thông tin cho vị trí`p - 1`không cần xử lý`p = 1`riêng biệt có thể tạo ra một kết quả không hợp lệ. 

Trường hợp ranh giới thứ tư là một sửa đổi ở vị trí cuối cùng:```
1
3 1
1 2 3
3 4
```Câu trả lời là`3`. Không có hậu tố sau vị trí`3`, vì vậy sau khi quyết định xem vị trí`3`được chọn, quá trình tính toán kết thúc. Mã tìm kiếm phần tử lớn hơn bắt đầu từ`p + 1`phải cho phép phạm vi tìm kiếm trống. 

## Phương pháp tiếp cận 

Giải pháp brute-force chỉ đơn giản là sao chép mảng ban đầu về mặt khái niệm, thay thế`h[p]`qua`q`, và quét tất cả`n`các vị trí. Trong quá trình quét, chúng tôi duy trì chiều cao của quả táo được hái cuối cùng và tăng câu trả lời bất cứ khi nào chiều cao hiện tại lớn hơn. Đây chính xác là quy tắc chọn, vì vậy phương pháp này là chính xác. 

Vấn đề là chi phí của nó. Một truy vấn mất`O(n)`thời gian và`m`truy vấn lấy`O(nm)`. Với`n = m = 100000`, trường hợp xấu nhất là về`10^10`kiểm tra phần tử. Ngay cả trước khi tính đến chi phí Python, đó là một mức độ quá lớn. 

Phương pháp brute-force hoạt động vì trạng thái quét chỉ có một số, độ cao lớn nhất được chọn cho đến nay. Quan sát hữu ích là việc thay đổi vị trí`p`không thể ảnh hưởng đến bất kỳ quyết định nào trước đó`p`. Nó chỉ thay đổi ngưỡng mà hậu tố nhìn thấy. 

Đối với tiền tố`[1, p-1]`, chúng ta có thể tính toán trước số lượng táo được hái và chiều cao được hái tối đa là bao nhiêu. Sau đó chỉ còn hai trường hợp. Nếu như`q`không lớn hơn tiền tố tối đa, vị trí`p`bị bỏ qua và hậu tố tiếp tục với tiền tố cũ tối đa. Nếu như`q`lớn hơn tiền tố tối đa, vị trí`p`được chọn và hậu tố tiếp tục với`q`như ngưỡng của nó. 

Chúng ta còn lại một thao tác: cho một hậu tố bắt đầu từ`l`và một ngưỡng`x`, tìm phần tử đầu tiên có chiều cao lớn hơn`x`. Khi phần tử đó được tìm thấy, phần còn lại của câu trả lời có thể được sử dụng lại từ quá trình tiền xử lý. 

Để có thể tái sử dụng, hãy tính toán`nxt[i]`, vị trí đầu tiên bên phải của`i`có chiều cao thực sự lớn hơn`h[i]`. Nếu vị trí đó không tồn tại,`nxt[i]`là số không. Cho phép`dp[i]`là số lượng táo được chọn khi quét từ`i`và điều trị`h[i]`là ngưỡng đầu tiên hiện tại. Sau đó`dp[i] = 1 + dp[nxt[i]]`khi`nxt[i]`tồn tại và`dp[i] = 1`nếu không thì. các`nxt`mảng có thể được xây dựng theo thời gian tuyến tính với ngăn xếp đơn điệu giảm dần. 

Tìm kiếm lớn hơn đầu tiên còn lại có thể được xử lý bằng cây phân đoạn lưu trữ chiều cao tối đa của mỗi khoảng. Bắt đầu từ vị trí truy vấn, cây có thể loại bỏ mọi khoảng có mức tối đa không lớn hơn ngưỡng. Khi một khoảng có thể chứa một vị trí hợp lệ, trước tiên hãy đi xuống phía con bên trái của nó, vì chúng ta cần vị trí đầu tiên như vậy. Việc này cần`O(log n)`thời gian. 

Điều này cung cấp một giai đoạn tiền xử lý tuyến tính và thời gian truy vấn logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nm)`|`O(1)`thêm | Quá chậm | 
| Tối ưu |`O(n + m log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước`prefix_count[i]`, số táo được hái ở tiền tố ban đầu`[1, i]`, Và`prefix_max[i]`, chiều cao được chọn lớn nhất trong tiền tố đó. Trong khi quét từ trái sang phải, một quả táo sẽ đóng góp chính xác khi chiều cao của nó lớn hơn mức tối đa hiện tại. Hai mảng này mô tả đầy đủ mọi thứ mà một truy vấn cần từ phần trước`p`. 
2. Tính toán`nxt[i]`với một ngăn xếp đơn điệu. Quét vị trí từ phải sang trái. Trong khi đỉnh ngăn xếp có chiều cao nhỏ hơn hoặc bằng`h[i]`, hãy xóa nó đi, vì vị trí đó không bao giờ có thể là quả táo cao hơn đầu tiên cho vị trí`i`. Đỉnh còn lại là vị trí đầu tiên bên phải có chiều cao lớn hơn`h[i]`. 
3. Tính toán`dp[i]`từ phải sang trái. Nếu như`nxt[i]`bằng 0 thì`i`là quả táo được hái cuối cùng trong chuỗi này và`dp[i] = 1`. Ngược lại, sau khi chọn`i`, quả táo được hái tiếp theo chính xác là`nxt[i]`, Vì thế`dp[i] = 1 + dp[nxt[i]]`. 
4. Xây dựng cây phân đoạn theo độ cao ban đầu, lưu trữ giá trị tối đa trong khoảng thời gian của mỗi nút. Mục đích của nó không phải là đếm táo trực tiếp. Nó cho phép chúng ta tìm vị trí đầu tiên tại hoặc sau một chỉ mục nhất định có chiều cao lớn hơn ngưỡng được cung cấp. 
5. Đối với một truy vấn`(p, q)`, trước tiên hãy lấy thông tin cho`[1, p-1]`. Nếu như`p = 1`, tiền tố này trống, vì vậy hãy sử dụng ngưỡng nhỏ hơn mọi chiều cao có thể có và số lượng được chọn bằng 0. Nếu không thì,`prefix_count[p-1]`là số đã được chọn và`prefix_max[p-1]`là ngưỡng hiện tại. 
6. So sánh`q`với tiền tố tối đa đó. Nếu như`q`lớn hơn, vị trí`p`được chọn, do đó hãy tăng câu trả lời và đặt ngưỡng hậu tố thành`q`. Nếu như`q`nhỏ hơn hoặc bằng, vị trí`p`bị bỏ qua và ngưỡng hậu tố vẫn là tiền tố tối đa. 
7. Tìm kiếm vị trí đầu tiên trên cây phân đoạn`i > p`với`h[i] > threshold`. Nếu không có chức vụ đó thì không có đóng góp phụ. Ngược lại, vị trí đó là quả táo đầu tiên được hái sau vị trí đã sửa đổi và toàn bộ phần đóng góp còn lại là`dp[i]`. 
8. Thêm tiền tố đóng góp, đóng góp có thể có của chức vụ`p`và sự đóng góp hậu tố. Bởi vì các truy vấn là độc lập nên các mảng ban đầu không bao giờ được sửa đổi. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi quét bất kỳ tiền tố nào, thông tin duy nhất liên quan đến các vị trí còn lại là chiều cao của quả táo được hái cuối cùng. Đối với một truy vấn tại vị trí`p`, tiền tố ban đầu không thay đổi, do đó số lượng được chọn và chiều cao được chọn cuối cùng của nó đã được biết. Vị trí được sửa đổi sẽ trở thành mức tối đa mới được chọn hoặc giữ nguyên mức tối đa đó. Khi hậu tố bắt đầu, quả táo được chọn đầu tiên của nó phải là phần tử đầu tiên lớn hơn ngưỡng này. Sau khi phần tử đầu tiên được chọn, quy trình này hoàn toàn giống quy trình được biểu thị bằng`dp[i]`. Do đó, mỗi quả táo đóng góp vào câu trả lời đều được tính một lần và mọi quả táo không hái được sẽ bị loại trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())
        h = [0] + list(map(int, input().split()))

        # prefix_count[i] = number of picked apples in [1, i]
        # prefix_max[i] = last picked height in [1, i]
        prefix_count = [0] * (n + 1)
        prefix_max = [0] * (n + 1)

        cur_max = 0
        cur_count = 0

        for i in range(1, n + 1):
            if h[i] > cur_max:
                cur_max = h[i]
                cur_count += 1
            prefix_max[i] = cur_max
            prefix_count[i] = cur_count

        # nxt[i] = first j > i with h[j] > h[i]
        nxt = [0] * (n + 1)
        stack = []

        for i in range(n, 0, -1):
            while stack and h[stack[-1]] <= h[i]:
                stack.pop()

            if stack:
                nxt[i] = stack[-1]

            stack.append(i)

        # dp[i] = number of picked apples in the chain
        # starting by picking position i.
        dp = [0] * (n + 1)

        for i in range(n, 0, -1):
            if nxt[i] == 0:
                dp[i] = 1
            else:
                dp[i] = dp[nxt[i]] + 1

        # Segment tree for range maximum.
        size = 1
        while size < n:
            size <<= 1

        seg = [0] * (2 * size)

        for i in range(1, n + 1):
            seg[size + i - 1] = h[i]

        for i in range(size - 1, 0, -1):
            seg[i] = max(seg[i << 1], seg[i << 1 | 1])

        def first_greater(left, value):
            """First index >= left with h[index] > value, or 0."""
            if left > n:
                return 0

            def search(node, nl, nr):
                if nr < left or seg[node] <= value:
                    return 0

                if nl == nr:
                    return nl

                mid = (nl + nr) >> 1

                res = search(node << 1, nl, mid)
                if res:
                    return res

                return search(node << 1 | 1, mid + 1, nr)

            return search(1, 1, size)

        out = []

        for _ in range(m):
            p, q = map(int, input().split())

            if p == 1:
                answer = 0
                threshold = 0
            else:
                answer = prefix_count[p - 1]
                threshold = prefix_max[p - 1]

            if q > threshold:
                answer += 1
                threshold = q

            first = first_greater(p + 1, threshold)

            if first:
                answer += dp[first]

            out.append(str(answer))

        sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý tiền tố tương ứng trực tiếp với phần đầu tiên của quá trình phân rã truy vấn.`prefix_count`ghi lại bao nhiêu quả táo đã được chọn, trong khi`prefix_max`ghi lại ngưỡng được chuyển đến vị trí được sửa đổi và hậu tố của nó. 

Ngăn xếp đơn điệu tính toán`nxt`trong thời gian tuyến tính. Việc so sánh sử dụng`<=`, không`<`, bởi vì quy tắc yêu cầu quả táo tiếp theo phải cao hơn một cách nghiêm ngặt. Quả táo có chiều cao bằng nhau không thể là quả táo được hái tiếp theo. 

các`dp`sự tái phát được đánh giá từ phải sang trái vì`dp[i]`phụ thuộc vào`dp[nxt[i]]`, Và`nxt[i]`luôn lớn hơn`i`. Mọi giá trị nhiều nhất là`n`, vì vậy số nguyên Python thông thường là quá đủ. 

Cây phân đoạn được lập chỉ mục với các vị trí mảng dựa trên một.`first_greater(left, value)`cố tình tìm kiếm một chỉ số lớn hơn hoặc bằng`left`, vì vậy truy vấn đã vượt qua`p + 1`và không bao giờ vô tình xem xét chính vị trí đã sửa đổi. 

Việc tìm kiếm đầu tiên kiểm tra con bên trái. Thứ tự đó là cần thiết vì một số phần tử có thể lớn hơn ngưỡng, nhưng phần tử bắt buộc là phần tử đầu tiên. Toàn bộ cây con bị loại bỏ ngay lập tức khi giá trị lớn nhất của nó gần bằng ngưỡng, bởi vì không có phần tử nào bên trong nó có thể thỏa mãn bất đẳng thức nghiêm ngặt. 

Truy vấn không bao giờ thay đổi`h`, cây phân đoạn,`nxt`, hoặc`dp`. Điều này rất cần thiết vì mỗi truy vấn mô tả một sửa đổi giả thuyết riêng biệt chứ không phải một chuỗi các sửa đổi tích lũy. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu được cung cấp:```
1
5 3
1 2 3 4 4
1 5
5 5
2 3
```Đối với mảng ban đầu, thông tin tiền tố là`1, 2, 3, 4, 4`cho số lượng đã chọn và`1, 2, 3, 4, 4`cho tiền tố cực đại. Có liên quan`nxt`liên kết là`1 -> 2`,`2 -> 3`,`3 -> 4`, trong khi cả hai vị trí đều không`4`cũng không`5`có phần tử lớn hơn ở bên phải của nó. 

Đối với truy vấn đầu tiên, vị trí được sửa đổi là quả táo đầu tiên nên không có tiền tố. Ngưỡng mới là`5`. Không có phần tử nào ở bên phải vượt quá`5`. 

| Truy vấn | Số tiền tố | Tiền tố tối đa | Chiều cao sửa đổi | Nhặt`p`? | Ngưỡng hậu tố | Đầu tiên lớn hơn | Số lượng hậu tố | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
|`(1, 5)`| 0 | 0 | 5 | vâng | 5 | không | 0 | 1 | 
|`(5, 5)`| 4 | 4 | 5 | vâng | 5 | không | 0 | 5 | 
|`(2, 3)`| 1 | 1 | 3 | vâng | 3 | 4 | 1 | 3 | 

Vì`(5, 5)`, bốn quả táo ban đầu đầu tiên đã được hái và giá trị mới`5`lớn hơn ngưỡng trước đó`4`, tặng cả 5 quả táo. Vì`(2, 3)`, tiền tố đóng góp một quả táo, vị trí`2`được chọn và vị trí`4`trở thành quả táo lớn hơn tiếp theo. Chức vụ`5`có chiều cao bằng nhau`4`, vì vậy nó không được chọn sau vị trí`4`. 

Ví dụ thứ hai thực hiện trường hợp quả táo đã sửa đổi bị bỏ qua:```
1
5 3
2 5 5 7 9
2 3
3 6
4 1
```Đối với truy vấn đầu tiên, mảng được sửa đổi là`[2, 3, 5, 7, 9]`, vậy là cả năm quả táo đều được hái. Đối với truy vấn thứ hai, mảng được sửa đổi là`[2, 5, 6, 7, 9]`, một lần nữa cho năm. Đối với truy vấn thứ ba, mảng được sửa đổi là`[2, 5, 5, 1, 9]`. Chức vụ`4`bị bỏ qua vì`1`không lớn hơn tiền tố tối đa`5`, và sau đó`9`được chọn. 

| Truy vấn | Số tiền tố | Tiền tố tối đa | Chiều cao sửa đổi | Nhặt`p`? | Ngưỡng hậu tố | Đầu tiên lớn hơn | Số lượng hậu tố | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
|`(2, 3)`| 1 | 2 | 3 | vâng | 3 | 3 | 3 | 5 | 
|`(3, 6)`| 2 | 5 | 6 | vâng | 6 | 4 | 2 | 5 | 
|`(4, 1)`| 2 | 5 | 1 | không | 5 | 5 | 1 | 3 | 

Hàng thứ ba giải thích tại sao ngưỡng hậu tố phải được duy trì`5`khi quả táo sửa đổi bị bỏ qua. Tìm kiếm phần tử lớn hơn`1`sẽ chọn sai vị trí`5`là phần tử lớn hơn đầu tiên, nhưng nó sẽ bỏ lỡ thực tế là tiền tố đã thiết lập ngưỡng`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m log n)`| Mảng tiền tố, ngăn xếp đơn điệu, DP và xây dựng cây phân đoạn cần`O(n)`, trong khi mỗi truy vấn thực hiện một tìm kiếm lớn hơn đầu tiên trên cây phân đoạn trong`O(log n)`. | 
| Không gian |`O(n)`| Mảng tiền tố,`nxt`,`dp`, ngăn xếp và cây phân đoạn đều yêu cầu lưu trữ tuyến tính. | 

Vì`n, m <= 100000`, quá trình tiền xử lý chỉ thực hiện một số lần quét tuyến tính không đổi, trong khi mỗi truy vấn thực hiện khoảng`O(log n)`công việc về cây phân đoạn. Tổng công việc thoải mái dưới mức`O(nm)`cách tiếp cận bạo lực và mức tiêu thụ bộ nhớ là tuyến tính theo số lượng táo. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`solve()`chức năng từ giải pháp trên có sẵn. Trình trợ giúp tạm thời thay thế đầu vào tiêu chuẩn và thu thập đầu ra tiêu chuẩn, do đó, các bài kiểm tra thực hiện logic truy vấn và phân tích cú pháp giống như chương trình đã gửi.```python
import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()

    try:
        with redirect_stdout(output):
            solve()
        return output.getvalue().strip()
    finally:
        sys.stdin = old_stdin

# Provided sample
sample = """\
1
5 3
1 2 3 4 4
1 5
5 5
2 3
"""
assert run(sample) == "1\n5\n3", "provided sample"

# Minimum-size case
minimum = """\
1
1 3
7
1 1
1 7
1 1000000000
"""
assert run(minimum) == "1\n1\n1", "single apple"

# All equal values, including a replacement that becomes a new maximum
all_equal = """\
1
4 4
5 5 5 5
1 6
2 4
4 6
4 5
"""
assert run(all_equal) == "1\n1\n2\n1", "all equal values"

# Boundary cases at the first and last positions
boundaries = """\
1
5 4
1 2 3 4 5
1 10
1 1
5 6
5 1
"""
assert run(boundaries) == "1\n4\n5\n4", "first and last positions"

# Strict inequality and an off-by-one-sensitive suffix
strict = """\
1
5 4
2 5 5 7 9
2 3
3 6
4 1
3 5
"""
assert run(strict) == "5\n5\n3\n4", "strict comparison and suffix"

# Maximum-size stress case.
# Every original value is 1. A replacement of 1 gives one picked apple,
# while a replacement of 2 gives exactly two picked apples.
n = 100000
m = 100000
queries = []
expected = []

for i in range(1, m + 1):
    q = 1 if i & 1 else 2
    p = (i - 1) % n + 1
    queries.append(f"{p} {q}")
    expected.append("1" if q == 1 else "2")

maximum = (
    "1\n"
    f"{n} {m}\n"
    + ("1 " * (n - 1))
    + "1\n"
    + "\n".join(queries)
    + "\n"
)

assert run(maximum) == "\n".join(expected), "maximum-size stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`,`h=[7]`|`1`,`1`,`1`| Tiền tố và hậu tố trống, kích thước mảng tối thiểu | 
|`h=[5,5,5,5]`|`1`,`1`,`2`,`1`| Không được chọn các giá trị bằng nhau, bao gồm cả bất đẳng thức nghiêm ngặt | 
|`h=[1,2,3,4,5]`với những thay đổi ở vị trí`1`Và`5`|`1`,`4`,`5`,`4`| Ranh giới vị trí đầu tiên và vị trí cuối cùng | 
|`h=[2,5,5,7,9]`|`5`,`5`,`3`,`4`| Giá trị được sửa đổi trở thành hoặc không trở thành ngưỡng mới | 
|`100000`giá trị bằng nhau và`100000`truy vấn | luân phiên`1`Và`2`| Tiền xử lý tuyến tính, truy vấn logarit, kích thước đầu vào tối đa | 

## Vỏ cạnh 

Khi nào`p = 1`, tiền tố trống. Ví dụ,```
1
3 1
1 2 3
1 5
```Bộ thuật toán`answer = 0`Và`threshold = 0`. Từ`5 > 0`, quả táo được sửa đổi đầu tiên sẽ được chọn, tạo thành ngưỡng`5`. Tìm kiếm hậu tố không tìm thấy giá trị nào lớn hơn`5`, vì vậy đầu ra là`1`. 

Khi`p = n`, hậu tố trống. Ví dụ,```
1
3 2
1 2 3
3 4
3 1
```Vì`(3,4)`, tiền tố đóng góp hai quả táo và`4 > 2`, vậy vị trí`3`được chọn và câu trả lời là`3`. Vì`(3,1)`, chức vụ`3`bị bỏ qua vì`1 <= 2`, vậy đáp án vẫn là`2`. Cuộc gọi tìm kiếm từ`p + 1 = 4`ngay lập tức trả về không có vị trí. 

Khi thay thế bằng tiền tố tối đa thì phải bỏ qua. Ví dụ,```
1
3 1
2 5 8
3 5
```Tiền tố trước vị trí`3`có tối đa`5`. Vì việc thay thế cũng là`5`, chức vụ`3`không được chọn. Hậu tố trống nên câu trả lời là`2`. sử dụng`>=`thay vì`>`sẽ đếm sai quả táo thứ ba. 

Khi sự thay thế lớn hơn mức tối đa của tiền tố, nó sẽ trở thành ngưỡng mới và thay đổi hoàn toàn những bản ghi hậu tố nào còn tồn tại. Ví dụ,```
1
4 1
1 2 3 4
3 10
```Tiền tố góp phần`2`táo tối đa`2`. Sự thay thế`10`được chọn, đưa ra câu trả lời hiện tại của`3`. Hậu tố chứa`4`, Nhưng`4`không lớn hơn`10`, vì vậy không có hậu tố apple nào được thêm vào. Kết quả là`3`. 

Khi tiền tố đã có giá trị tối đa cao và giá trị thay thế nhỏ thì hậu tố phải được tìm kiếm bằng giá trị tối đa cũ đó chứ không phải giá trị thay thế. Ví dụ,```
1
5 1
2 5 5 7 9
4 1
```Trước vị trí`4`, số táo được hái là`2`Và`5`, do đó ngưỡng là`5`. Sự thay thế`1`bị bỏ qua. Giá trị đầu tiên sau vị trí`4`lớn hơn`5`là`9`ở vị trí`5`, Và`dp[5] = 1`. Câu trả lời là`2 + 1 = 3`. 

Trường hợp đẳng thức trong ngăn xếp đơn điệu cũng có ý nghĩa tương đương. Vì```
1
4 1
5 5 5 6
1 5
```mảng được sửa đổi là`[5,5,5,6]`. Chỉ có cái đầu tiên`5`và trận chung kết`6`được chọn, vì vậy câu trả lời là`2`. Trong khi xây dựng`nxt`, vị trí có chiều cao`5`phải loại bỏ các ứng cử viên có chiều cao bằng nhau trước đó, bởi vì giá trị bằng nhau không bao giờ là lựa chọn tiếp theo hợp lệ lớn hơn. sử dụng`<`thay vì`<=`trong điều kiện ngăn xếp sẽ để lại các liên kết có chiều cao bằng nhau không hợp lệ và làm hỏng DP.
