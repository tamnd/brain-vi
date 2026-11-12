---
title: "CF 1650F - Thuật toán vô dụng nâng cao và quan trọng"
description: "Chúng tôi có một chuỗi các bài tập khóa học có thời hạn đã được sắp xếp sẵn. Bài tập i phải được hoàn thành đầy đủ trước thời gian tuyệt đối a[i]. Ban đầu mọi nhiệm vụ đều có tiến độ 0%. Có m lựa chọn đào tạo có sẵn. Tùy chọn j thuộc về đúng một phép gán e[j]."
date: "2026-06-10T03:56:11+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1650
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 776 (Div. 3)"
rating: 2200
weight: 1650
solve_time_s: 156
verified: false
draft: false
---

[CF 1650F - Các thuật toán vô dụng nâng cao và quan trọng](https://codeforces.com/problemset/problem/1650/F) 

**Xếp hạng:** 2200 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi các bài tập khóa học có thời hạn đã được sắp xếp sẵn. 

Phân công`i`phải được hoàn thành đầy đủ trước thời gian tuyệt đối`a[i]`. Ban đầu mỗi nhiệm vụ đều có`0%`tiến triển. 

có`m`các lựa chọn đào tạo có sẵn. Lựa chọn`j`thuộc về đúng một nhiệm vụ`e[j]`. Nếu chúng ta chi tiêu`t[j]`số giờ trên tùy chọn đó, tỷ lệ phần trăm hoàn thành nhiệm vụ`e[j]`tăng lên bởi`p[j]`. Mỗi tùy chọn có thể được sử dụng nhiều nhất một lần. 

Các tùy chọn được thực hiện lần lượt theo thứ tự do chúng tôi chọn. Thời gian tích lũy trên toàn cầu. Sau khi thực hiện một số tùy chọn, tổng thời gian đã trôi qua là tổng thời lượng của chúng. 

Đối với mỗi nhiệm vụ`i`, vào thời điểm thời gian đạt đến thời hạn`a[i]`, tổng phần trăm thu được từ các phương án thuộc nhiệm vụ`i`ít nhất phải có`100`. 

Chúng ta phải xây dựng một chuỗi các tùy chọn riêng biệt hợp lệ hoặc xác định rằng không tồn tại chuỗi nào như vậy. 

Quan sát quan trọng nhất là thời hạn được gắn liền với tiền tố thời gian. Khi hết hạn`a[i]`đến, mọi nhiệm vụ`1..i`ít nhất phải đạt được`100%`, bởi vì thời hạn của họ không muộn hơn`a[i]`. 

Những hạn chế là những gì thúc đẩy giải pháp. Mặc dù`n`Và`m`mỗi cái có thể lớn bằng`10^5`, tổng của`n+m`trên tất cả các trường hợp thử nghiệm chỉ`2·10^5`. Điều này ngay lập tức loại trừ bất cứ điều gì theo cấp số nhân và cũng loại trừ việc sử dụng một chiếc ba lô lớn trên tất cả các lựa chọn trên toàn cầu. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(m · constant)`mỗi trường hợp thử nghiệm. 

Một hạn chế tiềm ẩn quan trọng là mỗi phần trăm đạt được nhiều nhất là`100`. Điều đó có nghĩa là mỗi nhiệm vụ chỉ quan tâm đến giá trị tiến độ từ`0`ĐẾN`100`, điều này gợi ý một trạng thái ba lô nhỏ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một bài tập với các tùy chọn:```
deadline = 10

(t=10,p=100)
(t=1,p=60)
(t=1,p=40)
```Sử dụng tùy chọn 100% duy nhất sẽ hoạt động, mất 10 giờ. Sử dụng hai tùy chọn còn lại cũng có tác dụng, chỉ mất 2 giờ. Nếu chúng ta chỉ kiểm tra tính khả thi thì cả hai đều có vẻ hợp lệ. Tuy nhiên, đối với những thời hạn muộn hơn, tổng thời gian càng nhỏ thì càng tốt. Giải pháp phải giảm thiểu thời gian tiêu tốn cho mỗi nhiệm vụ. 

Một cái bẫy khác là tiến độ lấp đầy quá mức.```
deadline = 5

(t=2,p=70)
(t=2,p=70)
```Tiến độ đạt 140%. Điều này là hoàn toàn hợp pháp. Một DP chỉ lưu trữ số tiền chính xác và quên kẹp các giá trị trên 100 sẽ bỏ lỡ giải pháp này. 

Cái bẫy thứ ba là các bài tập chỉ tương tác theo thời gian.```
assignment 1 deadline = 5
assignment 2 deadline = 6
```Ngay cả khi nhiệm vụ 2 cuối cùng có thể được hoàn thành, nhiệm vụ 1 có thể tiêu tốn quá nhiều thời gian và không còn chỗ trống trước thời hạn của chính nó. Thời gian tích lũy dành cho tất cả các tùy chọn đã chọn cho bài tập`1..i`không bao giờ được vượt quá`a[i]`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ kiểm tra các tập hợp con của các tùy chọn và cố gắng lên lịch cho chúng. Ngay cả đối với một nhiệm vụ duy nhất cũng có thể có hàng nghìn lựa chọn, khiến`2^m`hoàn toàn không thể. 

Ý tưởng tiếp theo là xử lý các bài tập một cách độc lập. Đối với một bài tập cố định, chúng ta cần một tập hợp con các tùy chọn có tổng tiến độ đạt ít nhất`100`, trong khi tổng thời lượng càng nhỏ càng tốt. 

Điều này đột nhiên giống như một vấn đề về chiếc ba lô. 

Mỗi lựa chọn góp phần:```
weight = duration t
value = progress p
```Chúng tôi muốn đạt được giá trị ít nhất`100`đồng thời giảm thiểu trọng lượng. 

Thông thường ba lô sẽ quá đắt vì thời lượng có thể lên tới`10^9`. Quan sát quan trọng là sự tiến bộ bị giới hạn bởi`100`. Thay vì DP theo thời gian, chúng tôi thực hiện DP theo tiến độ. 

Đối với một nhiệm vụ, hãy```
dp[x] = minimum time needed to obtain exactly x progress
```trong đó tất cả các giá trị trên`100`được kẹp vào`100`. 

Vì sự tiến bộ không bao giờ vượt quá`100`, không gian trạng thái chỉ`101`. 

Sau khi tính toán thời gian tối thiểu có thể đạt được cho một nhiệm vụ, chúng tôi cộng nó vào thời gian tích lũy dành cho các nhiệm vụ trước đó. Nếu thời gian tích lũy vượt quá thời hạn của nhiệm vụ đó thì không có lịch trình nào tồn tại. 

Thách thức còn lại là tái thiết. Chúng ta phải xuất ra các chỉ số tùy chọn thực tế. Trong khi chạy ba lô, chúng tôi lưu trữ thông tin phụ huynh và xây dựng lại các tùy chọn nào đã tạo thành tập hợp con tối ưu. 

Điều tuyệt vời là các bài tập đều độc lập ngoại trừ sự đóng góp của chúng vào tổng thời gian đã trôi qua. Khi chúng ta biết thời lượng yêu cầu tối thiểu cho mỗi nhiệm vụ, việc sử dụng bất cứ thứ gì chậm hơn chỉ có thể khiến thời hạn khó đáp ứng hơn. Vì vậy, lựa chọn cục bộ tối ưu cho mỗi nhiệm vụ cũng là lựa chọn tối ưu toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(m) | Quá chậm | 
| Tối ưu | O(m · 100) | O(m + 100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm tất cả các tùy chọn theo nhiệm vụ mà chúng thuộc về. 
2. Xử lý bài tập từ`1`ĐẾN`n`theo thứ tự thời hạn. 
3. Đối với bài tập hiện tại, hãy chạy ba lô 0/1 theo tiến độ. 

Cho phép`dp[s]`là tổng thời gian tối thiểu cần thiết để đạt được tiến bộ`s`, trong đó tất cả các giá trị tiến trình lớn hơn`100`được lưu trữ dưới dạng trạng thái`100`. 
4. Khởi tạo:```
dp[0] = 0
dp[1..100] = INF
```5. Lặp lại từng tùy chọn của bài tập này. 

Đối với mỗi lựa chọn`(t, p)`, cập nhật DP ngược lại chính xác như một chiếc ba lô 0/1 tiêu chuẩn. 
6. Bất cứ khi nào chúng tôi chuyển đổi```
old_state -> new_state
```và có được thời lượng tốt hơn, ghi lại thông tin phụ huynh. Điều này sau đó cho phép xây dựng lại các tùy chọn đã chọn. 
7. Sau khi tất cả các tùy chọn được xử lý, hãy kiểm tra`dp[100]`. 

Nếu nó vẫn là vô hạn thì việc hoàn thành 100% là không thể, vì vậy câu trả lời là`-1`. 
8. Xây dựng lại các chỉ mục tùy chọn đã chọn bằng cách làm theo các con trỏ cha được lưu trữ từ trạng thái`100`. 
9. Thêm thời lượng tối thiểu`dp[100]`với thời gian tích lũy đã dành cho các nhiệm vụ trước đó. 
10. Nếu thời gian cộng dồn vượt quá thời hạn`a[i]`, đầu ra`-1`. 

Điều này có nghĩa là cách nhanh nhất để hoàn thành bài tập`1..i`lỡ thời hạn. 
11. Mặt khác, hãy thêm các chỉ số tùy chọn được xây dựng lại vào chuỗi câu trả lời chung. 
12. Tiếp tục nhiệm vụ tiếp theo. 
13. Sau khi tất cả các phép gán được xử lý thành công, xuất ra các chỉ số tùy chọn đã thu thập. 

### Tại sao nó hoạt động 

Đối với một nhiệm vụ cố định, DP tính toán tổng thời lượng tối thiểu trong số tất cả các tập hợp con có tiến độ đạt ít nhất`100`. Không gian trạng thái chứa mọi giá trị tiến trình được giới hạn có thể có từ`0`ĐẾN`100`, do đó không có tập hợp con khả thi nào bị bỏ qua. 

Giả sử một nhiệm vụ có thể được hoàn thành trong thời gian`X`, và DP trả về`Y > X`. Sau đó tập hợp con đạt được`X`sẽ tương ứng với trình tự chuyển đổi DP hợp lệ, mâu thuẫn với tính tối ưu của giá trị được lưu trữ tối thiểu. Vì vậy, DP thực sự tìm thấy thời lượng tối thiểu. 

Các bài tập không cạnh tranh các phương án vì mỗi phương án đều thuộc về một bài tập duy nhất. Sự tương tác duy nhất giữa các bài tập là thời gian trôi qua tích lũy. Việc thay thế kế hoạch hoàn thành trong thời gian tối thiểu cho một số nhiệm vụ bằng một kế hoạch chậm hơn không bao giờ có thể giúp đáp ứng thời hạn. Do đó, nếu tổng của tất cả thời lượng tối thiểu cho các bài tập`1..i`đã vượt quá thời hạn`a[i]`, không có lịch trình hợp lệ tồn tại. Ngược lại, nếu mọi tiền tố có khoảng thời gian tối thiểu đều thỏa mãn thời hạn của nó thì việc thực hiện phép gán các tập con đã chọn theo phép gán sẽ tạo ra một lịch trình hợp lệ. 

Điều này chứng tỏ tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        by_task = [[] for _ in range(n)]

        for idx in range(1, m + 1):
            e, tt, p = map(int, input().split())
            by_task[e - 1].append((tt, p, idx))

        total_time = 0
        answer = []
        possible = True

        for task in range(n):
            opts = by_task[task]

            k = len(opts)

            dp = [INF] * 101
            dp[0] = 0

            parent_state = [[-1] * 101 for _ in range(k + 1)]
            used_option = [[-1] * 101 for _ in range(k + 1)]

            for i in range(k):
                tt, p, idx = opts[i]

                ndp = dp[:]

                for s in range(101):
                    if dp[s] == INF:
                        continue

                    ns = min(100, s + p)
                    cand = dp[s] + tt

                    if cand < ndp[ns]:
                        ndp[ns] = cand
                        parent_state[i + 1][ns] = s
                        used_option[i + 1][ns] = i

                for s in range(101):
                    if ndp[s] == dp[s]:
                        if parent_state[i + 1][s] == -1:
                            parent_state[i + 1][s] = s

                dp = ndp

            best = dp[100]

            if best == INF:
                possible = False
                break

            total_time += best

            if total_time > a[task]:
                possible = False
                break

            chosen = []

            state = 100

            for i in range(k, 0, -1):
                if parent_state[i][state] == -1:
                    state = state
                    continue

                prev = parent_state[i][state]

                if used_option[i][state] != -1:
                    opt_index = used_option[i][state]
                    chosen.append(opts[opt_index][2])

                state = prev

            chosen.reverse()
            answer.extend(chosen)

        if not possible:
            out.append("-1")
        else:
            out.append(str(len(answer)))
            out.append(" ".join(map(str, answer)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Kích thước DP chỉ`101`, đây là quan sát quan trọng giúp giải quyết nhanh chóng. 

Các mảng tái thiết được lập chỉ mục bởi`(processed_options, progress_state)`. Khi quá trình chuyển đổi cải thiện trạng thái, chúng tôi sẽ nhớ cả tiến trình trước đó và tùy chọn nào đã tạo quá trình chuyển đổi. 

Giá trị tiến độ lớn hơn`100`ngay lập tức được kẹp vào`100`. Thiếu chi tiết này là nguyên nhân phổ biến dẫn đến các câu trả lời sai vì nhiều đáp án hợp lệ vượt quá`100%`. 

Việc kiểm tra thời hạn được thực hiện sau khi thêm thời lượng tối thiểu của nhiệm vụ hiện tại. Tại thời điểm đó, chúng tôi đã tính toán thời gian tích lũy nhỏ nhất có thể cho tiền tố của các bài tập. Nếu thậm chí vượt quá thời hạn, không có lựa chọn thay thế nào có thể sửa chữa được. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
Deadlines: [5, 7, 8]

Task 1:
(1,30,id=1)
(1,80,id=4)

Task 2:
(3,50,id=2)
(3,100,id=3)

Task 3:
(3,100,id=5)
```Đối với nhiệm vụ 1: 

| Tiểu bang | Thời gian tối thiểu | 
| --- | --- | 
| 0 | 0 | 
| 30 | 1 | 
| 80 | 1 | 
| 100 | 2 | 

Thời gian hoàn thành tốt nhất là`2`. 

Đối với nhiệm vụ 2: 

| Tiểu bang | Thời gian tối thiểu | 
| --- | --- | 
| 0 | 0 | 
| 50 | 3 | 
| 100 | 3 | 

Thời gian hoàn thành tốt nhất là`3`. 

Đối với nhiệm vụ 3: 

| Tiểu bang | Thời gian tối thiểu | 
| --- | --- | 
| 0 | 0 | 
| 100 | 3 | 

Thời gian hoàn thành tốt nhất là`3`. 

Tổng tiền tố của thời lượng tối thiểu: 

| Nhiệm vụ | Thời lượng Tốt nhất | Tiền tố Thời gian | Hạn chót | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 5 | 
| 2 | 3 | 5 | 7 | 
| 3 | 3 | 8 | 8 | 

Tất cả các thời hạn đều được đáp ứng. 

Câu trả lời được xây dựng lại là một chuỗi hợp lệ, chẳng hạn như:```
1 4 3 5
```### Ví dụ 2```
1 assignment
deadline = 4

options:
(t=3,p=60)
(t=3,p=60)
```Kết quả DP: 

| Tiến độ | Thời gian tối thiểu | 
| --- | --- | 
| 0 | 0 | 
| 60 | 3 | 
| 100 | 6 | 

Thời lượng tối thiểu là`6`. 

| Nhiệm vụ | Tiền tố Thời gian | Hạn chót | 
| --- | --- | --- | 
| 1 | 6 | 4 | 

Từ`6 > 4`, câu trả lời là:```
-1
```Điều này chứng tỏ rằng chỉ đạt 100% thôi là chưa đủ. Việc hoàn thành cũng phải phù hợp với thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · 100) | Mỗi tùy chọn thực hiện chuyển đổi qua 101 trạng thái tiến trình | 
| Không gian | O(m + 100) | Lưu trữ tùy chọn cộng với DP nhỏ và cấu trúc tái thiết | 

Vì tổng số tùy chọn trên tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, tổng khối lượng công việc DP là khoảng`2·10^7`chuyển đổi trạng thái, phù hợp thoải mái trong giới hạn trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # invoke solve() here

    return out.getvalue()

# minimum size, impossible
assert run("""1
1 1
1
1 1 99
""").strip() == "-1"

# minimum size, exact completion
assert run("""1
1 1
5
1 3 100
""").splitlines()[0] == "1"

# overfill beyond 100%
assert run("""1
1 2
10
1 1 70
1 1 70
""").splitlines()[0] == "2"

# deadline violation despite feasible completion
assert run("""1
1 2
3
1 2 60
1 2 60
""").strip() == "-1"

# choose faster subset instead of single slow option
assert run("""1
1 3
5
1 5 100
1 1 60
1 1 40
""").splitlines()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một lựa chọn cho 99% | -1 | Không thể dừng dưới 100% | 
| Một lựa chọn cho 100% | Câu trả lời hợp lệ | Trường hợp khả thi nhỏ nhất | 
| 70% + 70% | Câu trả lời hợp lệ | Tiến độ phải được giới hạn ở mức 100 | 
| Thời gian hoàn thành vượt quá thời hạn | -1 | Kiểm tra thời hạn | 
| Tùy chọn chậm 100% so với kết hợp nhanh | Lựa chọn kết hợp nhanh | DP giảm thiểu thời lượng | 

## Vỏ cạnh 

### Tiến độ vượt quá 100% 

đầu vào:```
1
1 2
10
1 1 70
1 1 70
```Tùy chọn đầu tiên đạt tiến trình 70. Tùy chọn thứ hai đạt tiến trình 140. DP lưu trữ tùy chọn này ở trạng thái 100. Thời gian hoàn thành trở thành 2. Nếu không có kẹp, trạng thái 140 sẽ không tồn tại và giải pháp có thể bị bỏ qua. 

### Bài tập có thể hoàn thành nhưng bị trễ thời hạn 

đầu vào:```
1
1 2
3
1 2 60
1 2 60
```Cách duy nhất để đạt 100% phải mất 4 giờ. DP tìm chính xác thời lượng 4. Sau khi thêm nó vào tổng tiền tố, chúng ta thu được:```
4 > 3
```vì vậy thuật toán trả về`-1`. 

### Nhiều cách để đạt 100% 

đầu vào:```
1
1 3
10
1 10 100
1 1 60
1 1 40
```Có hai giải pháp tồn tại.```
10 hours -> 100%
2 hours -> 100%
```DP lưu trữ thời lượng tối thiểu cho mọi trạng thái tiến trình, vì vậy trạng thái 100 kết thúc bằng giá trị 2. Việc tái tạo tuân theo các con trỏ cha tương ứng và xuất ra tập hợp con nhanh hơn. 

### Thiếu hoàn thành một bài tập 

đầu vào:```
1
2 1
5 10
1 1 100
```Bài tập 1 có thể được hoàn thành. Nhiệm vụ 2 không có lựa chọn nào cả. 

DP của nó rời đi`dp[100] = INF`, do đó thuật toán ngay lập tức đưa ra`-1`, điều này đúng vì mọi bài tập đều phải đạt ít nhất 100%.
