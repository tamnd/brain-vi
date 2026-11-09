---
title: "CF 1482C - Ngoại giao cơ bản"
description: "Chúng ta có m ngày. Vào mỗi ngày, có một nhóm bạn cụ thể và chúng ta phải chọn chính xác một người bạn sẵn có cho ngày đó. Nhiệm vụ phải đáp ứng một điều kiện công bằng. Đặt giới hạn = trần (m / 2). Không có người bạn nào có thể được chọn nhiều hơn số lần giới hạn trong tất cả các ngày."
date: "2026-06-10T23:26:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1482
codeforces_index: "C"
codeforces_contest_name: "\u0422\u0435\u0445\u043d\u043e\u043a\u0443\u0431\u043e\u043a 2021 - \u0424\u0438\u043d\u0430\u043b"
rating: 1600
weight: 1482
solve_time_s: 368
verified: false
draft: false
---

[CF 1482C - Ngoại giao cơ bản](https://codeforces.com/problemset/problem/1482/C) 

**Đánh giá:** 1600 
**Tags:** vũ lực, thuật toán xây dựng, tham lam, thực hiện 
**Thời gian giải:** 6 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`m`ngày. Vào mỗi ngày, có một nhóm bạn cụ thể và chúng ta phải chọn chính xác một người bạn sẵn có cho ngày đó. 

Nhiệm vụ phải đáp ứng một điều kiện công bằng. Cho phép`limit = ceil(m / 2)`. Không có người bạn nào có thể được chọn nhiều hơn`limit`lần trong tất cả các ngày. 

Đối với mỗi ngày, đầu vào sẽ cung cấp danh sách bạn bè có thể chơi vào ngày đó. Nhiệm vụ của chúng ta là xây dựng một bài tập hợp lệ hoặc xác định rằng không có bài tập nào tồn tại. 

Các ràng buộc là manh mối đầu tiên cho thấy giải pháp không thể phức tạp. Tổng số ngày trên tất cả các trường hợp thử nghiệm nhiều nhất là`100000`và tổng số mục khả dụng trên tất cả các danh sách nhiều nhất là`200000`. Điều này có nghĩa là chúng ta nên hướng tới một giải pháp chỉ xử lý từng danh sách sẵn có với số lần không đổi. Bất cứ điều gì theo cấp số nhân đều là không thể ngay lập tức, và ngay cả công việc bậc hai trong`m`sẽ vượt quá giới hạn. 

Phần khó khăn là các lựa chọn không độc lập. Việc chọn một người bạn hôm nay sẽ ảnh hưởng đến việc chúng ta có thể tiếp tục sử dụng người bạn đó vào ngày mai mà không vi phạm giới hạn tần suất tối đa hay không. 

Một vài trường hợp đáng chú ý. 

Hãy xem xét trường hợp mỗi ngày chỉ có một người bạn:```
m = 3

Day 1: {1}
Day 2: {1}
Day 3: {1}
```Nhiệm vụ khả thi duy nhất là bạn bè`1`ba lần. Từ`ceil(3/2)=2`, bạn ơi`1`sẽ được chọn nhiều hơn giới hạn cho phép. Câu trả lời đúng là`NO`. 

Một trường hợp tinh tế khác là khi một người bạn xuất hiện trong nhiều danh sách, nhưng những danh sách đó cũng chứa các lựa chọn thay thế:```
m = 5

{1}
{1,2}
{1,2}
{1,3}
{1,4}
```Chiến lược tham lam luôn chọn người bạn có sẵn đầu tiên sẽ chọn người bạn`1`năm lần và thất bại. Một nhiệm vụ hợp lệ tồn tại bằng cách chuyển hướng một số ngày linh hoạt cho những người bạn khác. 

Tình huống quan trọng cuối cùng là khi có chính xác một người bạn vượt quá giới hạn sau lần phân công ban đầu:```
m = 6

{1}
{1}
{1}
{1,2}
{1,3}
{1,4}
```Đây`limit = 3`. Bạn bè`1`bị ép buộc trong ba ngày đầu tiên và ban đầu sẽ được chọn trong cả sáu ngày. Chúng ta phải nhận ra rằng chỉ những ngày có nhiều lựa chọn mới có thể được chỉ định lại. Nếu có đủ những ngày như vậy thì câu trả lời là có thể. Nếu không thì không thể được. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử tất cả các bài tập có thể. Mỗi ngày, hãy chọn một người bạn có sẵn và tiếp tục đệ quy. Nếu một ngày có tới`n`ứng viên, không gian tìm kiếm trở nên gần như$$\prod_{i=1}^{m} k_i$$đó là số mũ của số ngày. Ngay cả khi chỉ có hai mươi ngày và hai lựa chọn mỗi ngày, đã có hơn một triệu bài tập cần kiểm tra. Với tối đa một trăm nghìn ngày, cách làm này hoàn toàn không khả thi. 

Quan sát quan trọng đến từ việc hỏi điều gì thực sự có thể gây ra thất bại. 

Giả sử ban đầu chúng ta chỉ định hàng ngày cho người bạn đầu tiên trong danh sách sẵn có của nó. Đếm xem mỗi người bạn được chọn bao nhiêu lần. 

Nếu không có người bạn nào vượt quá`ceil(m/2)`, chúng ta đã làm xong rồi. 

Bây giờ giả sử một người bạn nào đó`x`vượt quá giới hạn. Nhiều người bạn có thể vượt quá giới hạn cùng một lúc không? 

Không. Nếu hai người bạn khác nhau xuất hiện nhiều hơn`m/2`lần, tổng số lần xuất hiện của chúng sẽ vượt quá`m`, điều này là không thể vì chỉ có`m`ngày. 

Điều này đơn giản hóa đáng kể vấn đề. Nhiều nhất một người bạn có thể vi phạm hạn chế. 

Giả sử bạn bè`x`được chọn quá nhiều lần. Cách duy nhất để giảm`x`số lượng của là thay đổi ngày hiện được chỉ định cho`x`. Ngày có một người bạn duy nhất không thể thay đổi được. Những ngày có danh sách sẵn sàng chứa thêm bạn bè có thể được chỉ định lại cho người khác. 

Vì vậy, chiến lược trở thành: 

1. Ban đầu chỉ định mỗi ngày cho người bạn được liệt kê đầu tiên. 
2. Tìm xem một số người bạn`x`vượt quá giới hạn. 
3. Nếu không, hãy trả lời`YES`. 
4. Nếu không thì liên tục di chuyển ngày linh hoạt được giao cho`x`hướng tới một người bạn sẵn có khác cho đến khi`x`số lượng của giảm đến giới hạn. 
5. Nếu chúng tôi hết số ngày linh hoạt trước khi đạt đến giới hạn, hãy trả lời`NO`. 

Lý do điều này có tác dụng là vì chỉ một người bạn có thể gặp vấn đề và việc rời xa người bạn đó một ngày không bao giờ có thể tạo ra hành vi vi phạm mới vì số lượng người bạn của mọi người khác chỉ tăng từ giá trị tối đa là giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Đệ quy/trạng thái theo cấp số nhân | Quá chậm | 
| Tối ưu | O(Σkᵢ) | O(m + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`limit = (m + 1) // 2`. 
2. Đối với mỗi ngày, ban đầu hãy chọn người bạn đầu tiên được liệt kê trong danh sách sẵn có của ngày đó. 
3. Đếm xem mỗi người bạn hiện được chọn bao nhiêu lần. 
4. Tìm một người bạn`x`có số lượng vượt quá`limit`. 
5. Nếu không có người bạn đó, hãy in`YES`và nhiệm vụ hiện tại. 
6. Ngược lại, hãy tính```
excess = count[x] - limit
```Đây là số lượng lựa chọn phải được chuyển khỏi bạn bè`x`. 
7. Quét tất cả các ngày. 

Nếu một ngày hiện được chỉ định cho`x`và ngày đó có ít nhất hai người bạn sẵn sàng, hãy giao lại ngày đó cho bất kỳ người bạn sẵn sàng nào khác, ví dụ như người bạn thứ hai trong danh sách. 
8. Sau mỗi lần phân công lại, hãy giảm`excess`bởi một. 
9. Dừng lại một lần`excess`trở thành số không. 
10. Nếu`excess`vẫn dương tính sau khi kiểm tra cả ngày, in`NO`. 
11. Nếu không thì in`YES`và nhiệm vụ cuối cùng. 

### Tại sao nó hoạt động 

Nhiệm vụ ban đầu có thể vi phạm giới hạn đối với nhiều nhất một người bạn. Hãy để người bạn đó là`x`. 

Mọi giải pháp hợp lệ đều phải giảm số lần`x`được chọn chính xác`count[x] - limit`. Những ngày duy nhất có thể giúp ích là những ngày được giao cho`x`có nhiều lựa chọn sẵn có. Việc chỉ định lại bất kỳ ngày nào như vậy sẽ giảm`x`được tính bằng một. 

Không có sự chỉ định lại nào có thể tạo ra một vi phạm mới. Trước khi sửa đổi, mọi người bạn ngoại trừ`x`đã xuất hiện nhiều nhất`limit`lần. Trong quá trình này, chúng tôi tăng số lượng của chúng lên, nhưng tổng số lần tăng chính xác là số lượng mà chúng tôi tăng lên.`x`đã vượt quá giới hạn. Một lần`x`đạt đến giới hạn, mọi người bạn khác vẫn ở mức tối đa. 

Như vậy, nếu có đủ số ngày linh hoạt thì việc xây dựng sẽ thành công. Nếu không có đủ số ngày linh hoạt thì không có giải pháp nào có thể tồn tại vì yêu cầu cắt giảm`x`là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())

        days = []
        ans = []
        cnt = [0] * (n + 1)

        for _ in range(m):
            arr = list(map(int, input().split()))
            k = arr[0]
            friends = arr[1:]

            days.append(friends)

            chosen = friends[0]
            ans.append(chosen)
            cnt[chosen] += 1

        limit = (m + 1) // 2

        bad = -1
        for friend in range(1, n + 1):
            if cnt[friend] > limit:
                bad = friend
                break

        if bad == -1:
            print("YES")
            print(*ans)
            continue

        need = cnt[bad] - limit

        for i in range(m):
            if need == 0:
                break

            friends = days[i]

            if ans[i] == bad and len(friends) > 1:
                cnt[bad] -= 1
                ans[i] = friends[1]
                need -= 1

        if need > 0:
            print("NO")
        else:
            print("YES")
            print(*ans)

solve()
```Việc thực hiện tuân theo bằng chứng gần như trực tiếp. 

Người bạn đầu tiên trong mọi danh sách sẵn sàng sẽ trở thành nhiệm vụ tạm thời. Điều này quan trọng vì nó cho chúng ta một điểm khởi đầu cụ thể và đảm bảo rằng chỉ một người bạn mới có thể trở thành vấn đề. 

Sau khi đếm tần số, chúng tôi xác định được người bạn có số lượng vượt quá giới hạn. Nếu không có người bạn nào như vậy tồn tại thì nhiệm vụ hiện tại đã hợp lệ. 

Khi có một người bạn có vấn đề tồn tại, chúng tôi chỉ kiểm tra những ngày hiện được chỉ định cho người bạn đó. Những ngày chỉ có một tùy chọn sẵn có không thể giúp ích được vì việc thay đổi chúng là không thể. Để có một ngày linh hoạt, chỉ định người bạn thứ hai trong danh sách là đủ. Chúng ta không cần phải tìm kiếm người thay thế tốt nhất vì bằng chứng cho thấy việc giảm số lượng người bạn có vấn đề là điều duy nhất quan trọng. 

Biến`need`theo dõi chính xác có bao nhiêu lựa chọn nữa phải được di chuyển khỏi người bạn bị lạm dụng. Khi nó đạt đến 0, phép gán thỏa mãn giới hạn tần số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 6
1 1
2 1 2
3 1 2 3
4 1 2 3 4
2 2 3
1 3
```

`limit = 3`. 

Nhiệm vụ ban đầu: 

| Ngày | Có sẵn | Được chọn | 
| --- | --- | --- | 
| 1 | {1} | 1 | 
| 2 | {1,2} | 1 | 
| 3 | {1,2,3} | 1 | 
| 4 | {1,2,3,4} | 1 | 
| 5 | {2,3} | 2 | 
| 6 | {3} | 3 | 

Đếm: 

| Bạn | Đếm | 
| --- | --- | 
| 1 | 4 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 0 | 

Bạn bè`1`vượt quá giới hạn bởi`1`. 

Phân công lại ngày thứ 2 từ`1`ĐẾN`2`. 

| Ngày | Lựa Chọn Mới | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 2 | 
| 6 | 3 | 

Bạn bè`1`bây giờ xuất hiện chính xác`3`lần, vì vậy phép gán là hợp lệ. 

Ví dụ này cho thấy chỉ cần một điều chỉnh duy nhất mặc dù nhiều ngày liên quan đến bạn bè.`1`. 

### Ví dụ 2 

đầu vào:```
2 2
1 1
1 1
```

`limit = 1`. 

Nhiệm vụ ban đầu: 

| Ngày | Có sẵn | Được chọn | 
| --- | --- | --- | 
| 1 | {1} | 1 | 
| 2 | {1} | 1 | 

Đếm: 

| Bạn | Đếm | 
| --- | --- | 
| 1 | 2 | 

Bạn bè`1`vượt quá giới hạn bởi`1`. 

Không có ngày nào có nhiều hơn một người bạn nên không thể phân công lại được. 

Thuật toán kết thúc với`need = 1`và in`NO`. 

Điều này chứng tỏ tình huống mà tất cả các lựa chọn quá mức đều bị ép buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Σkᵢ) | Mỗi danh sách sẵn có được đọc một lần và mỗi ngày được quét tối đa một lần | 
| Không gian | O(m + n) | Lưu trữ danh sách phân công, số lượng và tình trạng sẵn sàng | 

Tổng số mục khả dụng trên tất cả các trường hợp thử nghiệm nhiều nhất là`200000`, do đó quá trình xử lý tuyến tính dễ dàng nằm gọn trong giới hạn hai giây. Việc sử dụng bộ nhớ cũng ở dưới mức giới hạn một cách thoải mái vì chúng tôi chỉ lưu trữ danh sách đầu vào và một số mảng phụ trợ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def run(inp: str) -> str:
    from collections import defaultdict

    data = inp.strip().split()
    p = 0
    t = int(data[p])
    p += 1

    out = []

    for _ in range(t):
        n = int(data[p]); p += 1
        m = int(data[p]); p += 1

        days = []
        ans = []
        cnt = [0] * (n + 1)

        for _ in range(m):
            k = int(data[p]); p += 1
            cur = list(map(int, data[p:p+k]))
            p += k

            days.append(cur)
            ans.append(cur[0])
            cnt[cur[0]] += 1

        limit = (m + 1) // 2

        bad = -1
        for i in range(1, n + 1):
            if cnt[i] > limit:
                bad = i
                break

        if bad == -1:
            out.append("YES")
            out.append(" ".join(map(str, ans)))
            continue

        need = cnt[bad] - limit

        for i in range(m):
            if need == 0:
                break
            if ans[i] == bad and len(days[i]) > 1:
                ans[i] = days[i][1]
                need -= 1

        if need:
            out.append("NO")
        else:
            out.append("YES")
            out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# minimum size
assert run("""1
1 1
1 1
""").startswith("YES")

# impossible because all days force same friend
assert run("""1
2 2
1 1
1 1
""") == "NO"

# already valid
assert run("""1
3 3
1 1
1 2
1 3
""").startswith("YES")

# one reassignment fixes everything
assert run("""1
4 6
1 1
2 1 2
3 1 2 3
4 1 2 3 4
2 2 3
1 3
""").startswith("YES")

# boundary: exactly ceil(m/2) appearances
assert run("""1
2 3
1 1
1 1
1 2
""").startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ngày độc thân, người bạn độc thân | CÓ | Kích thước đầu vào tối thiểu | 
| Hai ngày gượng ép cho cùng một người bạn | KHÔNG | Không thể vì không có ngày linh hoạt | 
| Bạn bè ép buộc khác nhau | CÓ | Đã thỏa mãn giới hạn | 
| Người bạn bị lạm dụng với các lựa chọn thay thế | CÓ | Logic gán lại | 
| Bạn bè đã sử dụng chính xác ceil(m/2) lần | CÓ | Xử lý ranh giới đúng | 

## Vỏ cạnh 

Xét trường hợp bắt buộc hoàn toàn:```
1
1 3
1 1
1 1
1 1
```Giới hạn là`2`. Bạn bè`1`được chọn ba lần. Mỗi ngày chỉ có một ứng cử viên nên thuật toán không tìm thấy ngày nào đủ điều kiện để chỉ định lại. Phần dư còn lại là dương và đáp án là`NO`. Bất kỳ giải pháp hợp lệ nào cũng cần giảm bớt bạn bè`1`số lượng của nó, điều đó là không thể. 

Hãy xem xét trường hợp người bạn bị sử dụng quá mức có thể được sửa chữa:```
1
3 5
1 1
2 1 2
2 1 2
2 1 3
2 1 3
```Giới hạn là`3`. Nhiệm vụ ban đầu chọn bạn`1`năm lần. Phần thừa bằng`2`. Thuật toán phân bổ lại hai ngày linh hoạt xa bạn bè`1`, giảm số lượng của nó xuống chính xác`3`. Việc chuyển nhượng kết quả là hợp lệ. 

Xét trường hợp biên:```
1
2 5
1 1
1 1
1 1
1 2
1 2
```Giới hạn là`3`. Bạn bè`1`đã xuất hiện đúng ba lần. Vì số lượng không lớn hơn giới hạn nên thuật toán không thực hiện sửa đổi nào và ngay lập tức đưa ra kết quả`YES`. Điều này tránh được sai lầm phổ biến là coi sự bình đẳng là một sự vi phạm. 

Cuối cùng, hãy xem xét:```
1
4 6
1 1
1 1
1 1
2 1 2
2 1 3
2 1 4
```Giới hạn là`3`. Bạn bè`1`ban đầu xuất hiện sáu lần, do đó số dư là`3`. Có chính xác ba ngày linh hoạt và việc phân công lại cả ba ngày sẽ làm giảm số lượng bạn bè`1`đến giới hạn. Thuật toán thành công, cho thấy mọi lần xuất hiện linh hoạt đều có thể được sử dụng độc lập để loại bỏ một đơn vị dư thừa.
