---
title: "CF 102373E - Họa Tiết Ca Rô"
description: "Chúng ta có một bảng hình chữ nhật (n lần m) có các ô màu đen hoặc trắng. Sau khi thay đổi bất kỳ số lượng ô nào, các ô màu đen phải tạo thành một biểu đồ kết nối không trống, trong đó các ô có chung một cạnh liền kề nhau và biểu đồ đó không được chứa chu trình."
date: "2026-08-14T12:39:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "E"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 587
verified: false
draft: false
---

[CF 102373E - Họa tiết ca rô](https://codeforces.com/problemset/problem/102373/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng hình chữ nhật (n \times m) có các ô màu đen hoặc trắng. Sau khi thay đổi bất kỳ số lượng ô nào, các ô màu đen phải tạo thành một biểu đồ kết nối không trống, trong đó các ô có chung một cạnh liền kề nhau và biểu đồ đó không được chứa chu trình. Theo thuật ngữ đồ thị, tập hợp ô đen cuối cùng phải tạo ra chính xác một cây. 

Mỗi ô được thay đổi đóng góp một đơn vị vào chi phí, vì vậy nhiệm vụ là tìm một phép biến đổi khoảng cách Hamming tối thiểu của bảng gốc thành bảng có đồ thị ô đen là cây. 

Các kích thước được cố tình không đối xứng. Chiều cao có thể đạt tới (100), nhưng chiều rộng tối đa là (10). Do đó, một trạng thái mô tả mọi thứ liên quan đến một ranh giới ngang có thể phụ thuộc theo cấp số nhân vào (m), trong khi vẫn thực tế vì (m) chỉ là (10). Thuật toán có số mũ trong (n m) là không thể, vì có thể có (1000) ô, trong khi thuật toán có phần mũ chỉ phụ thuộc vào (m) là mục tiêu tự nhiên. 

Trường hợp cạnh đầu tiên là một bảng hoàn toàn màu trắng. Ví dụ,```
.
```không thể giữ nguyên vì đồ thị màu đen được yêu cầu phải không trống. Kết quả đúng là```
#
```với một sự thay đổi. Giải pháp chỉ kiểm tra kết nối và chu kỳ có thể vô tình chấp nhận biểu đồ trống. 

Trường hợp cạnh thứ hai là một khu rừng bị ngắt kết nối. Ví dụ,```
#.#
```đã không có chu trình nhưng hai ô đen của nó là những thành phần riêng biệt. Một sự thay đổi là cần thiết, và```
###
```là kết quả tối ưu hợp lệ. Chỉ kiểm tra các chu trình là không đủ vì đồ thị cuối cùng cũng phải có chính xác một thành phần. 

Trường hợp cạnh thứ ba là một chu trình không chứa hình vuông hoàn toàn đen (2 \times 2). Coi như```
###
#.#
###
```Tám ô ranh giới màu đen tạo thành một vòng quanh tâm màu trắng. Việc loại bỏ một ô ranh giới sẽ phá vỡ chu kỳ đó và mang lại sự thay đổi tối ưu. Phép kiểm tra chỉ tìm kiếm các ô vuông màu đen đầy đủ (2 \times 2) sẽ bỏ lỡ chu trình này. 

Cuối cùng, một ô màu đen trong bảng lớn hơn luôn là cây hợp lệ. Ví dụ, một bảng toàn màu trắng (100 \times 10) chỉ cần thay đổi một lần. Điều này rất hữu ích vì nó kiểm tra cả ranh giới chiều cao lớn và yêu cầu tồn tại ít nhất một ô đen. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liệt kê mọi màu cuối cùng có thể có. Có (2^{nm}) tập hợp con các ô có thể có màu đen. Đối với mỗi tập hợp con, chúng ta có thể xây dựng biểu đồ cảm ứng của nó, kiểm tra tính kết nối và tính không tuần hoàn cũng như tính toán khoảng cách của nó với đầu vào. Điều này đúng vì mọi mẫu cuối cùng có thể đều được xem xét rõ ràng, nhưng kết quả trong trường hợp xấu nhất của nó là (O(nm2^{nm})). Ở mức tối đa (nm=1000), đó là khoảng (1000\cdot2^{1000}) hoạt động, vượt xa mọi điều khả thi. 

Quan sát hữu ích là chúng ta không cần phải nhớ toàn bộ bảng đã được xử lý. Quét từng hàng bảng và trong mỗi hàng từ trái sang phải. Khi một ô nằm phía sau ranh giới quét, cách duy nhất mà thành phần màu đen của nó vẫn có thể tương tác với các ô chưa được xử lý là thông qua ranh giới hiện tại giữa các ô đã xử lý và chưa được xử lý. 

Do đó, đối với mỗi cột, chúng tôi ghi nhớ xem ô biên giới có màu đen hay không và nếu có màu đen thì nó thuộc về thành phần được kết nối nào. Hai vị trí biên có cùng nhãn thuộc về cùng một thành phần trong phần được xử lý của bảng. Bản thân các nhãn không có ý nghĩa nên chúng được chuẩn hóa, ví dụ ((4,4,0,7)) trở thành ((1,1,0,2)). 

Khi một ô đen mới được chèn vào, nó có tối đa hai ô lân cận đã được xử lý, ô lân cận bên trái và ô lân cận phía trên của nó. Nếu cả hai đều tồn tại và thuộc cùng một thành phần, việc thêm ô mới sẽ tạo ra một chu trình. Nếu chúng thuộc về các thành phần khác nhau thì ô mới sẽ hợp nhất các thành phần đó. Nếu không tồn tại, nó sẽ bắt đầu một thành phần mới. 

Kết nối yêu cầu một phần trạng thái bổ sung. Nếu một thành phần biến mất hoàn toàn khỏi biên giới thì không tế bào nào trong tương lai có thể chạm vào nó. Một thành phần như vậy được hoàn thành vĩnh viễn. Một giải pháp cuối cùng hợp lệ có thể có nhiều nhất một thành phần đã hoàn thành và sau khi nó hoàn thành thì không được chọn thêm ô đen nào nữa. Điều này cho phép chúng ta loại bỏ các giải pháp từng phần bị ngắt kết nối ngay lập tức thay vì mang theo các trạng thái vô dụng. 

Giá trị lập trình động của một trạng thái là số lần đổi màu tối thiểu cần thiết để đạt được trạng thái đó. Bởi vì các quyết định trong tương lai chỉ phụ thuộc vào trạng thái kết nối biên giới chứ không phụ thuộc vào lịch sử chính xác, nên chỉ giữ lại cách rẻ nhất để đến từng trạng thái là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm2^{nm})) | (O(nm)) | Quá chậm | 
| Biên giới DP | (O(nm^2S_m)) | (O(nS_m)) với việc tái thiết | Đã chấp nhận | 

Ở đây (S_m) biểu thị số lượng chữ ký biên giới có thể tiếp cận đối với chiều rộng (m). Vì (m\le10), đây là hằng số phụ thuộc độ rộng cho bài toán này. Các trạng thái có thể truy cập thực tế ít hơn nhiều so với tất cả các nhãn tùy ý vì các thành phần phát sinh từ một nhóm lưới phẳng. 

## Hướng dẫn thuật toán

1. Quét các ô theo thứ tự hàng lớn và duy trì đường biên chính xác (m). Vị trí (c) biểu thị thành phần màu đen đã được xử lý chạm vào ranh giới hiện tại trong cột (c). Số 0 có nghĩa là ô biên giới có màu trắng hoặc không có thành phần màu đen nào chạm vào ranh giới ở đó. 
2. Chuẩn hóa tất cả các nhãn thành phần sau mỗi lần chuyển đổi. Ví dụ: các trạng thái ((7,7,0,3)) và ((1,1,0,2)) mô tả chính xác cùng một kết nối, do đó chúng phải được lưu trữ ở cùng trạng thái DP. Nếu không chuẩn hóa, số lượng trạng thái tương đương sẽ tăng lên một cách không cần thiết. 
3. Đối với mỗi ô, hãy thử làm cho nó có màu trắng. Giá của nó là (0) nếu nó đã có màu trắng và (1) nếu ban đầu nó có màu đen. Thay thế vị trí biên giới bằng 0. Nếu điều này loại bỏ lần xuất hiện biên cuối cùng của một thành phần thì thành phần đó đã bị đóng vĩnh viễn. Chỉ giữ quá trình chuyển đổi này khi không có thành phần hoạt động nào khác, vì thành phần đóng sẽ không bao giờ có thể kết nối với bất kỳ thứ gì sau này. 
4. Thử làm ô màu đen. Giá của nó là (0) khi ô ban đầu có màu đen và (1) nếu ngược lại. Nếu tồn tại một thành phần đã hoàn thiện trước đó, hãy từ chối quá trình chuyển đổi này vì ô màu đen mới sẽ tạo ra một thành phần riêng biệt. 
5. Nhìn vào nhãn biên giới phía trên và bên trái. Nếu cả hai đều khác 0 và bằng nhau, ô mới sẽ nối hai đỉnh đã được kết nối của cùng một thành phần, do đó cạnh mới sẽ đóng một chu trình. Từ chối quá trình chuyển đổi. 
6. Nếu nhãn trên và nhãn bên trái có giá trị khác 0, hãy hợp nhất các thành phần của chúng và gán thành phần đã hợp nhất cho ô mới. Nếu có chính xác một ô, hãy gắn ô mới vào thành phần đó. Nếu không tồn tại, hãy tạo một thành phần mới. 
7. Sau khi xử lý tất cả các ô, chấp nhận trạng thái nếu có chính xác một thành phần đang hoạt động hoặc chính xác một thành phần đã hoàn thành trước đó. Trạng thái hoàn toàn trống rỗng bị từ chối vì cần có ít nhất một ô màu đen. 
8. Lưu trữ, đối với mỗi hàng, trạng thái tiền nhiệm và mặt nạ ô đen đã chọn bất cứ khi nào trạng thái có được chi phí tốt hơn. Sau khi tìm thấy trạng thái cuối cùng tối ưu, hãy lùi lại các bản ghi tiền nhiệm này để khôi phục mọi hàng của bảng đầu ra. 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi nhãn biên giới khác 0 biểu thị chính xác một thành phần được kết nối của các ô đen đã được xử lý cho đến nay và mọi thành phần đã xử lý không còn chạm vào biên giới đã bị đóng vĩnh viễn. Các cạnh duy nhất được đưa vào khi xử lý một ô là các cạnh của nó đối với các cạnh bên trái và phía trên đã được xử lý. Do đó, một chu trình có thể được tạo ra chính xác khi cả hai đều tồn tại và thuộc về cùng một thành phần, đó chính xác là sự chuyển đổi mà chúng tôi từ chối. 

Một thành phần biến mất khỏi biên giới sẽ không có cạnh đối với bất kỳ ô nào chưa được xử lý, vì vậy nó không bao giờ có thể kết nối với thành phần được tạo sau đó. Do đó, việc từ chối một quá trình chuyển đổi đóng một thành phần trong khi thành phần khác vẫn còn lại là cần thiết để đồ thị cuối cùng được kết nối. Cuối cùng, một thành phần còn lại có nghĩa là các ô màu đen được kết nối, trong khi mọi thao tác chèn được chấp nhận đều tránh tạo ra một chu trình. Do đó, đồ thị màu đen cuối cùng là một cái cây. 

Để tối ưu, mỗi trạng thái DP giữ số lượng thay đổi tối thiểu trong số tất cả các bảng một phần có cùng thông tin biên giới. Tất cả các khả năng trong tương lai chỉ phụ thuộc vào thông tin đó, vì vậy một cách đắt tiền hơn để đạt được trạng thái tương tự không bao giờ có thể dẫn đến câu trả lời cuối cùng tốt hơn. Do đó, trạng thái cuối cùng được chấp nhận với chi phí tối thiểu là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize(a):
    mp = {}
    nxt = 1
    for i, x in enumerate(a):
        if x:
            y = mp.get(x)
            if y is None:
                y = nxt
                mp[x] = y
                nxt += 1
            a[i] = y
    return tuple(a)

def solve_case(grid):
    n = len(grid)
    m = len(grid[0])

    start_state = (0,) * m
    start_key = (start_state, 0)

    # dp[(frontier, finished)] = minimum number of changes
    dp = {start_key: 0}

    # parents[r][state] = (previous_state, row_mask)
    parents = []

    INF = 10 ** 9

    for r in range(n):
        # value = (cost, state_before_this_row, row_mask)
        cur = {
            key: (cost, key, 0)
            for key, cost in dp.items()
        }

        for c in range(m):
            nxt = {}

            for (state, finished), (cost, prev_key, row_mask) in cur.items():
                old = state[c]
                left = state[c - 1] if c > 0 else 0
                up = old

                # Choose white.
                white_cost = cost + (grid[r][c] == '#')
                a = list(state)
                a[c] = 0

                new_finished = finished

                if old:
                    still_alive = old in a
                    if not still_alive:
                        # A component disappeared from the frontier.
                        # It is safe only if it is the only component.
                        if any(a):
                            still_alive = False
                            new_finished = -1
                        else:
                            new_finished = 1

                if new_finished != -1:
                    ns = normalize(a)
                    nk = (ns, new_finished)

                    if white_cost < nxt.get(nk, (INF, None, None))[0]:
                        nxt[nk] = (
                            white_cost,
                            prev_key,
                            row_mask
                        )

                # Choose black.
                if not finished:
                    # If left and up belong to the same component,
                    # the two edges from the new cell close a cycle.
                    if not (left and up and left == up):
                        a = list(state)

                        if up and left and up != left:
                            # Merge left's component into up's component.
                            for i in range(m):
                                if a[i] == left:
                                    a[i] = up
                            new_label = up
                        elif up:
                            new_label = up
                        elif left:
                            new_label = left
                        else:
                            new_label = max(a) + 1

                        a[c] = new_label
                        ns = normalize(a)

                        black_cost = cost + (grid[r][c] == '.')
                        nk = (ns, finished)
                        nmask = row_mask | (1 << c)

                        if black_cost < nxt.get(
                            nk, (INF, None, None)
                        )[0]:
                            nxt[nk] = (
                                black_cost,
                                prev_key,
                                nmask
                            )

            cur = nxt

        ndp = {}
        par = {}

        for key, (cost, prev_key, row_mask) in cur.items():
            ndp[key] = cost
            par[key] = (prev_key, row_mask)

        dp = ndp
        parents.append(par)

    best_key = None
    best_cost = INF

    for (state, finished), cost in dp.items():
        if finished:
            if cost < best_cost:
                best_cost = cost
                best_key = (state, finished)
        else:
            components = len({x for x in state if x})
            if components == 1 and cost < best_cost:
                best_cost = cost
                best_key = (state, finished)

    # A one-cell black tree always exists, so best_key must exist.
    row_masks = [0] * n

    key = best_key
    for r in range(n - 1, -1, -1):
        prev_key, mask = parents[r][key]
        row_masks[r] = mask
        key = prev_key

    answer = []
    for r in range(n):
        row = []
        for c in range(m):
            row.append('#' if (row_masks[r] >> c) & 1 else '.')
        answer.append(''.join(row))

    return answer

def main():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    answer = solve_case(grid)
    sys.stdout.write('\n'.join(answer))

if __name__ == "__main__":
    main()
```Khóa DP bao gồm bộ biên giới và`finished`lá cờ. Bộ dữ liệu chỉ chứa thông tin kết nối, không chứa số lượng ô đen hoặc tọa độ chính xác của các thành phần cũ, vì không ảnh hưởng đến quá trình chuyển đổi trong tương lai. 

Quá trình chuyển đổi màu trắng thay thế vị trí biên giới hiện tại bằng 0. Phần tinh vi nhất là phát hiện một thành phần biến mất. Nếu nhãn cũ vắng mặt sau đó và một nhãn khác 0 vẫn còn, biểu đồ một phần sẽ bị ngắt kết nối vĩnh viễn, do đó quá trình chuyển đổi đó sẽ bị loại bỏ. Nếu không còn thành phần nào khác thì thành phần đó vừa được hoàn thành và`finished`cờ ghi lại rằng không có ô đen nào trong tương lai có thể được thêm vào. 

Đối với quá trình chuyển đổi màu đen,`up`là giá trị cũ ở cột hiện tại và`left`là giá trị đã được cập nhật ở cột trước đó. Thứ tự này là cần thiết. Tại thời điểm ô ((r,c)) được xử lý, đây chính xác là các ô lân cận đã được xử lý. 

Bài kiểm tra`left and up and left == up`phát hiện mọi chu kỳ mới được tạo. Nếu cả hai hàng xóm đều thuộc cùng một thành phần thì ô mới sẽ cung cấp tuyến đường thứ hai giữa chúng. Nếu chúng thuộc các thành phần khác nhau, ô mới sẽ nối hai cây thành một cây lớn hơn một cách an toàn. 

Mặt nạ hàng được lưu trữ trong mỗi bản ghi gốc là đủ để tái cấu trúc vì tất cả các lựa chọn trong một hàng đều được thể hiện bằng mặt nạ đó. Bản thân DP chỉ cần trạng thái biên giới kết quả, trong khi bản ghi tiền nhiệm ghi nhớ hàng nào đã được chọn để lấy nó. 

Số nguyên Python không bị giới hạn, nhưng chi phí tối đa là (nm), do đó không có vấn đề tràn. Mặt nạ bit có nhiều nhất là 10 bit vì (m\le10). 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, một mẫu cuối cùng tối ưu là`##.`,`#.#`,`###`. Nó chỉ thay đổi ô trên cùng bên phải. Dấu vết sau đây sử dụng các nhãn thành phần chuẩn, trong đó các nhãn bằng nhau có nghĩa là các ô biên giới tương ứng được kết nối. 

| Hàng đã xử lý | Hàng đã chọn | Biên giới sau hàng | Đã hoàn thành | 
| --- | --- | --- | --- | 
| Bắt đầu | không |`(0,0,0)`| 0 | 
| 1 |`##.`|`(1,1,0)`| 0 | 
| 2 |`#.#`|`(1,0,2)`| 0 | 
| 3 |`###`|`(1,1,1)`| 0 | 
| Kết thúc |`##./#.#/###`| một thành phần | 0 | 

Sau hàng đầu tiên, hai ô màu đen tạo thành một thành phần. Ở hàng thứ hai, ô ở giữa có màu trắng nên nhóm đen bên trái và bên phải tạm thời tách biệt. Ở hàng cuối cùng, ô ở giữa nối hai thành phần khác nhau đó. Vì các nhãn sẽ khác nhau khi điều đó xảy ra nên không có chu trình nào được tạo ra. Vẫn còn bảy ô đen và sáu cạnh, nên kết quả là một cái cây. Chi phí là một. 

Đối với Mẫu 2, bản thân đầu ra mẫu có thể được sử dụng làm mẫu tối ưu được theo dõi. 

| Hàng đã xử lý | Hàng đã chọn | Biên giới sau hàng | Đã hoàn thành | 
| --- | --- | --- | --- | 
| Bắt đầu | không |`(0,0,0)`| 0 | 
| 1 |`##.`|`(1,1,0)`| 0 | 
| 2 |`.##`|`(0,1,1)`| 0 | 
| 3 |`#.#`|`(2,1,1)`| 0 | 
| 4 |`###`|`(1,1,1)`| 0 | 
| Kết thúc |`##./.##/#.#/###`| một thành phần | 0 | 

Hàng thứ ba tạm thời tạo ra hai thành phần. Ô đầu tiên của hàng thứ tư mở rộng thành phần bên trái và ô thứ hai lại mở rộng thành phần đó. Ô cuối cùng nhìn thấy hàng xóm bên trái từ một thành phần và hàng xóm phía trên của thành phần khác, vì vậy nó hợp nhất chúng thay vì tạo ra một chu trình. Có chính xác hai ô khác với đầu vào, khớp với mức tối ưu đã nêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm^2S_m)) | Mỗi ô (nm) xử lý mọi trạng thái biên có thể tiếp cận và chuẩn hóa cộng với chi phí hợp nhất thành phần (O(m)). | 
| Không gian | (O(nS_m)) | DP hiện tại sử dụng trạng thái (O(S_m)), trong khi các bản ghi tiền nhiệm của hàng sử dụng không gian (O(nS_m)) để tái cấu trúc. | 

Ở đây (S_m) chỉ phụ thuộc vào chiều rộng. Chiều rộng được giới hạn ở (10), trong khi chiều cao chỉ là (100), đây chính xác là chế độ mà lập trình động biên giới hữu ích. Thuật toán không bao giờ liệt kê các bảng hoàn chỉnh (2^{nm}). 

## Trường hợp thử nghiệm 

Các thử nghiệm bên dưới xác thực kết quả đầu ra về mặt cấu trúc thay vì so sánh văn bản chính xác, vì bài toán cho phép bất kỳ mẫu tối ưu nào. Trình kiểm tra xác minh rằng đầu ra là một cây và số lần đổi màu của nó bằng với mức tối ưu đã biết.```python
import sys
import io
from collections import deque

def normalize(a):
    mp = {}
    nxt = 1
    for i, x in enumerate(a):
        if x:
            y = mp.get(x)
            if y is None:
                y = nxt
                mp[x] = y
                nxt += 1
            a[i] = y
    return tuple(a)

def solve_case(grid):
    n = len(grid)
    m = len(grid[0])

    dp = {((0,) * m, 0): 0}
    parents = []
    INF = 10 ** 9

    for r in range(n):
        cur = {
            key: (cost, key, 0)
            for key, cost in dp.items()
        }

        for c in range(m):
            nxt = {}

            for (state, finished), (cost, prev, mask) in cur.items():
                old = state[c]
                left = state[c - 1] if c else 0
                up = old

                # White
                a = list(state)
                a[c] = 0
                nf = finished

                if old and old not in a:
                    if any(a):
                        nf = -1
                    else:
                        nf = 1

                if nf != -1:
                    ns = normalize(a)
                    key = (ns, nf)
                    value = cost + (grid[r][c] == '#')
                    if value < nxt.get(key, (INF, None, None))[0]:
                        nxt[key] = (value, prev, mask)

                # Black
                if not finished and not (
                    left and up and left == up
                ):
                    a = list(state)

                    if left and up and left != up:
                        for i in range(m):
                            if a[i] == left:
                                a[i] = up
                        label = up
                    elif up:
                        label = up
                    elif left:
                        label = left
                    else:
                        label = max(a) + 1

                    a[c] = label
                    ns = normalize(a)
                    key = (ns, finished)
                    value = cost + (grid[r][c] == '.')
                    nmask = mask | (1 << c)

                    if value < nxt.get(key, (INF, None, None))[0]:
                        nxt[key] = (value, prev, nmask)

            cur = nxt

        dp = {key: value[0] for key, value in cur.items()}
        parents.append({
            key: (value[1], value[2])
            for key, value in cur.items()
        })

    best = None
    best_cost = INF

    for (state, finished), cost in dp.items():
        if finished or len({x for x in state if x}) == 1:
            if cost < best_cost:
                best_cost = cost
                best = (state, finished)

    masks = [0] * n
    key = best

    for r in range(n - 1, -1, -1):
        key, masks[r] = parents[r][key]

    return [
        ''.join('#' if (masks[r] >> c) & 1 else '.'
                for c in range(len(grid[0])))
        for r in range(n)
    ]

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    ans = solve_case(grid)
    sys.stdin = old_stdin
    return '\n'.join(ans)

def is_tree(board):
    n = len(board)
    m = len(board[0])

    cells = [
        (r, c)
        for r in range(n)
        for c in range(m)
        if board[r][c] == '#'
    ]

    if not cells:
        return False

    seen = {cells[0]}
    q = deque([cells[0]])

    while q:
        r, c = q.popleft()
        for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            nr, nc = r + dr, c + dc
            if (
                0 <= nr < n and
                0 <= nc < m and
                board[nr][nc] == '#'
                and (nr, nc) not in seen
            ):
                seen.add((nr, nc))
                q.append((nr, nc))

    if len(seen) != len(cells):
        return False

    edges = 0
    for r, c in cells:
        if r + 1 < n and board[r + 1][c] == '#':
            edges += 1
        if c + 1 < m and board[r][c + 1] == '#':
            edges += 1

    return edges == len(cells) - 1

def check(inp, expected_cost):
    first = inp.splitlines()
    n, m = map(int, first[0].split())
    original = first[1:n + 1]

    output = run(inp)
    board = output.splitlines()

    assert len(board) == n
    assert all(len(row) == m for row in board)
    assert all(ch in '.#' for row in board for ch in row)
    assert is_tree(board)

    cost = sum(
        original[r][c] != board[r][c]
        for r in range(n)
        for c in range(m)
    )
    assert cost == expected_cost

# Provided samples
check(
    """3 3
###
#.#
###
""",
    1
)

check(
    """4 3
##.
.##
###
##.
""",
    2
)

check(
    """2 3
...
...
""",
    1
)

# Minimum-size input, already valid
check(
    """1 1
#
""",
    0
)

# Minimum-size input, empty black graph
check(
    """1 1
.
""",
    1
)

# Disconnected forest, one change is enough
check(
    """1 3
#.#
""",
    1
)

# Maximum-size board, one black cell is optimal
check(
    "100 10\n" + "\n".join(["." * 10] * 100) + "\n",
    1
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 x 1`chứa đựng`#`|`#`| Một ô đơn đã là một cây hợp lệ. | 
|`1 x 1`chứa đựng`.`|`#`| Biểu đồ màu đen phải không trống. | 
|`1 x 3`chứa đựng`#.#`| Bất kỳ cây nào có giá (1) | Kết nối phải được thực thi ngay cả khi không có chu kỳ. | 
|`100 x 10`chỉ chứa`.`| Bất kỳ ô đen nào | Kích thước tối đa và trường hợp trống thành đơn. | 

## Vỏ cạnh 

Đối với đầu vào một ô```
.
```trạng thái DP ban đầu là biên giới trống. Việc chọn màu trắng sẽ để trống nhưng trạng thái đó cuối cùng sẽ bị từ chối vì không có thành phần màu đen. Việc chọn màu đen sẽ tạo ra một thành phần và trạng thái cuối cùng chứa chính xác một thành phần. Chi phí của nó là một, vì vậy sản lượng là`#`. 

Đối với đầu vào một ô đã hợp lệ```
#
```chọn màu đen có chi phí bằng không. Đường biên chứa một thành phần và trạng thái cuối cùng được chấp nhận ngay lập tức. Việc chọn màu trắng sẽ tạo ra trạng thái trống hoàn thiện, trạng thái này bị từ chối vì không chứa ô đen. Thuật toán do đó trả về`#`với sự thay đổi bằng không. 

Vì```
#.#
```ô đen đầu tiên tạo thành phần (1), ô trắng ở giữa để thành phần (1) hoạt động và ô đen cuối cùng bắt đầu thành phần (2). Do đó, đường biên cuối cùng chứa hai thành phần, do đó bảng không thay đổi sẽ bị loại bỏ. Thay vào đó, DP có thể làm ô ở giữa thành màu đen, hợp nhất hai bên thành một đường dẫn hoặc xóa một trong hai điểm cuối. Cả hai lựa chọn đều tốn một chi phí, vì vậy lựa chọn tối ưu là một. 

Đối với chu kỳ```
###
#.#
###
```hai hàng đầu tiên có thể tạm thời chứa một số thành phần biên giới. Khi hàng cuối cùng đóng hình dạng, bất kỳ quá trình chuyển đổi nào kết nối hai ô biên giới đã thuộc cùng một thành phần sẽ bị từ chối dưới dạng một chu trình. Việc xóa một ô sẽ để lại một tập hợp không tuần hoàn được kết nối, do đó DP giữ lại giải pháp chi phí một. Vì bản thân bảng gốc có tính tuần hoàn nên không có thay đổi nào là tối ưu, chứng tỏ rằng một thay đổi là tối thiểu. 

Đối với bảng toàn màu trắng (100\times10), DP có thể giữ mọi ô màu trắng cho đến khi chọn được một ô màu đen. Điều đó tạo ra một thành phần đơn lẻ, vốn đã là một cây. Chi phí chính xác là một và không có giải pháp nào có thể có chi phí bằng 0 vì bảng không thay đổi không có ô đen. Điều này xử lý bảng lớn nhất có thể trong khi thực hiện điều kiện cây không trống.
