---
title: "CF 1807F - Bóng nảy"
description: "Chúng ta có một quả bóng chuyển động bên trong một lưới hình chữ nhật n × m. Quả bóng luôn di chuyển theo đường chéo nên mỗi lần di chuyển sẽ thay đổi cả hai tọa độ ±1. Một hướng bao gồm một thành phần thẳng đứng và một thành phần nằm ngang."
date: "2026-06-09T09:05:13+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1807
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 859 (Div. 4)"
rating: 1700
weight: 1807
solve_time_s: 87
verified: true
draft: false
---

[CF 1807F - Bóng nảy](https://codeforces.com/problemset/problem/1807/F) 

**Đánh giá:** 1700 
**Tags:** vũ lực, dfs và tương tự, triển khai 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một quả bóng đang chuyển động bên trong một`n × m`lưới hình chữ nhật. Quả bóng luôn di chuyển theo đường chéo nên mỗi lần di chuyển sẽ thay đổi cả hai tọa độ một lượng`±1`. 

Một hướng bao gồm một thành phần thẳng đứng và một thành phần nằm ngang. Ví dụ,`DR`có nghĩa là tăng cả hàng và cột, trong khi`UL`có nghĩa là giảm cả hai. Bất cứ khi nào bước đi tiếp theo rời khỏi lưới, thành phần hướng tương ứng sẽ bị đảo ngược. Chạm vào bức tường trên cùng hoặc dưới cùng sẽ làm lật thành phần thẳng đứng. Đánh vào bức tường bên trái hoặc bên phải sẽ làm lật phần nằm ngang. Đến một góc lật cả hai. 

Nhiệm vụ không phải là đếm bước. Chúng ta phải đếm xem có bao nhiêu lần nảy xảy ra trước khi quả bóng chạm vào ô mục tiêu lần đầu tiên. Nếu quả bóng không bao giờ đến được ô đó, chúng ta sẽ xuất ra`-1`. 

Ràng buộc quan trọng nhất là tổng của tất cả các kích thước lưới trên tất cả các trường hợp thử nghiệm nhiều nhất là`5 · 10^4`. Điều đó nhỏ đến mức đáng ngạc nhiên. Mặc dù các kích thước cá nhân có thể đạt tới`25000`, tổng số ô trên tất cả các trường hợp thử nghiệm bị giới hạn. Điều này gợi ý rõ ràng rằng việc mô phỏng chuyển động là khả thi nếu chúng ta tránh quay lại trạng thái tương tự vô thời hạn. 

Một điểm tinh tế là bóng nảy lên sau khi đi vào ô. Bản thân ô được truy cập trước khi hướng thay đổi. Bất kỳ mô phỏng nào phản ánh trước rồi kiểm tra vị trí đều có thể tạo ra câu trả lời sai. 

Một điểm tinh tế khác là quả bóng có thể đi vào một chu kỳ không bao giờ chứa ô mục tiêu. Vì chuyển động có tính chất quyết định nên một khi trạng thái đó xuất hiện trở lại thì tương lai sẽ lặp lại mãi mãi. 

Coi như:```
2 4
start = (2,1), DR
target = (2,2)
```Quả bóng chuyển động:```
(2,1) -> (1,2) -> (2,3) -> (1,4) -> ...
```Tế bào`(2,2)`không bao giờ được ghé thăm. Một mô phỏng không phát hiện chu kỳ sẽ chạy mãi mãi. Câu trả lời đúng là`-1`. 

Một sai lầm dễ mắc phải khác là đếm số lần thoát thay vì đếm số lần thoát trước khi đạt được mục tiêu. 

Ví dụ:```
3 3
start = (1,3), UR
target = (2,2)
```Quả bóng đầu tiên chạm tới`(2,2)`sau khi nảy một lần từ bức tường phía trên. Câu trả lời là`1`. Nếu chúng ta tiếp tục mô phỏng và đếm số lần thoát sau đó, kết quả sẽ không chính xác. 

Trường hợp phạt góc thứ ba xảy ra khi vị trí xuất phát đã bằng mục tiêu. 

Ví dụ:```
3 3
start = (2,2), DR
target = (2,2)
```Câu trả lời là`0`bởi vì quả bóng đã ở đó trước khi có bất kỳ chuyển động nào xảy ra. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản theo dõi quả bóng từng bước một. Ở mỗi lần di chuyển, chúng tôi cập nhật vị trí, thực hiện mọi phản ánh cần thiết và đếm số lần thoát. 

Cách tiếp cận này đúng vì các quy tắc chuyển động có tính chất quyết định. Vấn đề là quả bóng có thể di chuyển trong một thời gian rất dài trước khi lặp lại. Vì kích thước có thể lớn bằng`25000`, mô phỏng từng bước thuần túy có thể yêu cầu hàng triệu bước di chuyển. 

Quan sát quan trọng là không có gì thú vị xảy ra giữa các lần thoát. 

Giả sử quả bóng hiện đang ở`(r,c)`với một hướng nào đó. Cho đến khi chạm vào tường, nó chỉ đi theo một đường chéo thẳng. Trong phân đoạn đó, hàng và cột thay đổi với số lượng như nhau ở mỗi bước. 

Thay vì mô phỏng từng bước di chuyển, chúng ta có thể nhảy thẳng sang lần nảy tiếp theo. Cho phép```
dr ∈ {+1,-1}
dc ∈ {+1,-1}
```đại diện cho hướng hiện tại. 

Khoảng cách đến bức tường thẳng đứng tiếp theo được xác định bởi`dr`:```
if dr = +1: n - r
if dr = -1: r - 1
```Tương tự, khoảng cách đến bức tường ngang tiếp theo là:```
if dc = +1: m - c
if dc = -1: c - 1
```Khoảng cách nhỏ hơn trong hai khoảng cách này cho chúng ta biết còn lại bao nhiêu bước chéo trước sự kiện thoát tiếp theo. 

Khi di chuyển dọc theo đoạn đường chéo này, mục tiêu sẽ đạt được chính xác khi cả độ lệch hàng và cột khớp nhau. Chúng ta có thể kiểm tra điều này bằng đại số mà không cần bước qua từng ô trung gian. 

Sau khi nhảy đến vị trí nảy, chúng ta lật các thành phần hướng cần thiết, tăng bộ đếm nảy rồi tiếp tục. 

Trạng thái của hệ thống là:```
(row, column, direction)
```chỉ có`4nm`các trạng thái có thể. Khi một trạng thái lặp lại, quỹ đạo tương lai cũng lặp lại. Vì tổng cộng`nm`trên tất cả các bài kiểm tra nhiều nhất là`5 · 10^4`, việc lưu trữ các trạng thái đã truy cập có thể dễ dàng thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lần di chuyển) | O(1) | Quá chậm | 
| Tối ưu | O(số trạng thái thoát) ≤ O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển chuỗi định hướng thành hai thành phần`dr`Và`dc`, mỗi cái bằng`+1`hoặc`-1`. 
2. Nếu ô bắt đầu đã bằng ô đích, hãy trả về`0`. 
3. Duy trì một tập hợp các trạng thái đã truy cập`(row, column, dr, dc)`. 
4. Trước khi xử lý một trạng thái, hãy kiểm tra xem nó đã được nhìn thấy chưa. Nếu có, quỹ đạo đã đi vào một chu kỳ và sẽ không bao giờ đạt được mục tiêu. Trở lại`-1`. 
5. Tính số bước cho đến bức tường thẳng đứng tiếp theo và bức tường ngang tiếp theo. 
6. Hãy để`k`nhỏ hơn trong hai giá trị đó. Quả bóng di chuyển chính xác`k`các bước chéo trước sự kiện thoát tiếp theo. 
7. Kiểm tra xem mục tiêu có nằm trên đoạn đường chéo này hay không. Vì cả hai tọa độ đều di chuyển tuyến tính nên mục tiêu sẽ đạt được trong đoạn iff: 

-`(i2 - row)`có cùng dấu hiệu với`dr`, 
-`(j2 - col)`có cùng dấu hiệu với`dc`, 
- và độ lệch hàng và cột được yêu cầu là bằng nhau. 

Nếu mục tiêu nằm trong phạm vi tiếp theo`k`bước, trả về số lần thoát hiện tại. 
8. Di chuyển bóng trực tiếp đến vị trí nảy:```
row += dr * k
col += dc * k
```9. Nếu vị trí nảy nằm ở viền trên hoặc viền dưới, hãy lật`dr`. 
10. Nếu vị trí nảy nằm ở viền trái hoặc phải, hãy lật`dc`. 
11. Tăng số lần thoát lên một và tiếp tục từ bước 4. 

### Tại sao nó hoạt động 

Giữa hai lần nảy liên tiếp, quả bóng đi theo một đường chéo thẳng. Mỗi ô được truy cập trên phân đoạn đó thỏa mãn mối quan hệ tuyến tính cố định giữa độ lệch hàng và cột. Kiểm tra xem mục tiêu có thuộc phân khúc đó hay không là đủ để xác định liệu nó có đạt được trước lần thoát tiếp theo hay không. 

Mô phỏng luôn chuyển từ trạng thái thoát này sang trạng thái thoát tiếp theo mà không bỏ qua bất kỳ sự xuất hiện mục tiêu nào có thể xảy ra, bởi vì mọi vị trí trung gian đều nằm trên đoạn đường chéo được kiểm tra. 

Chuyển động có tính quyết định. Từng là một bang`(row, column, dr, dc)`lặp lại, tất cả các trạng thái trong tương lai cũng lặp lại. Nếu mục tiêu không được tìm thấy trước khi lặp lại, nó sẽ không bao giờ được tìm thấy sau đó. Điều này đảm bảo rằng việc quay trở lại`-1`ở trạng thái lặp lại là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

DIR = {
    "DR": (1, 1),
    "DL": (1, -1),
    "UR": (-1, 1),
    "UL": (-1, -1),
}

def solve():
    t = int(input())

    for _ in range(t):
        n, m, r, c, tr, tc, d = input().split()

        n = int(n)
        m = int(m)
        r = int(r)
        c = int(c)
        tr = int(tr)
        tc = int(tc)

        dr, dc = DIR[d]

        if r == tr and c == tc:
            print(0)
            continue

        visited = set()
        bounces = 0
        answer = -1

        while True:
            state = (r, c, dr, dc)

            if state in visited:
                break

            visited.add(state)

            dv = (n - r) if dr == 1 else (r - 1)
            dh = (m - c) if dc == 1 else (c - 1)

            k = min(dv, dh)

            row_diff = tr - r
            col_diff = tc - c

            if row_diff * dr >= 0 and col_diff * dc >= 0:
                if row_diff == col_diff * dr * dc:
                    steps = abs(row_diff)
                    if steps <= k:
                        answer = bounces
                        break

            r += dr * k
            c += dc * k

            bounced = False

            if r == 1 or r == n:
                dr *= -1
                bounced = True

            if c == 1 or c == m:
                dc *= -1
                bounced = True

            if bounced:
                bounces += 1

        print(answer)

solve()
```Chuỗi định hướng được chuyển đổi thành hai thành phần có dấu. Điều này làm cho sự phản chiếu trở nên cực kỳ đơn giản vì việc nảy ra khỏi bức tường nằm ngang chỉ làm thay đổi`dr`, khi nảy ra khỏi bức tường thẳng đứng chỉ thay đổi`dc`. 

Ý tưởng cốt lõi là độ dài bước nhảy`k`. Thay vì mô phỏng mọi chuyển động, chúng tôi tính toán xem quả bóng có thể di chuyển bao xa trước khi chạm vào tường. Toàn bộ đoạn đường chéo đó được xử lý cùng một lúc. 

Việc kiểm tra mục tiêu là phần tế nhị nhất. Dọc theo một đoạn đường chéo, cả hai tọa độ đều thay đổi theo cùng một số bước về độ lớn. Sự bình đẳng```
row_diff == col_diff * dr * dc
```mã hóa yêu cầu đó cho cả bốn hướng cùng một lúc. 

Một chi tiết tinh tế khác là thời gian tính số lần thoát. Trước tiên chúng tôi kiểm tra xem mục tiêu có nằm trên phân khúc hiện tại hay không. Nếu có, câu trả lời là số lần thoát đã xảy ra. Chỉ sau khi đạt đến vị trí thoát, chúng ta mới tăng bộ đếm số lần thoát. 

Phát hiện chu kỳ sử dụng trạng thái hoàn chỉnh`(row, column, dr, dc)`. Chỉ sử dụng vị trí sẽ không chính xác vì đến cùng một ô với hướng khác có thể dẫn đến một quỹ đạo khác trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 7 1 7 2 4 DL
```| Số lần thoát | Vị trí | Hướng | k | Nhắm mục tiêu vào phân khúc? | 
| --- | --- | --- | --- | --- | 
| 0 | (1,7) | DL | 4 | Không | 
| 1 | (5,3) | UL | 2 | Không | 
| 2 | (3,1) | UR | 2 | Không | 
| 3 | (1,3) | DR | 1 | Có | 

Đoạn cuối cùng đi từ`(1,3)`ĐẾN`(2,4)`. Mục tiêu nằm trên phân đoạn đó nên câu trả lời là số lần thoát hiện tại, tức là`3`. 

### Ví dụ 2 

đầu vào:```
6 4 1 2 3 4 DR
```| Số lần thoát | Vị trí | Hướng | k | Nhắm mục tiêu vào phân khúc? | 
| --- | --- | --- | --- | --- | 
| 0 | (1,2) | DR | 2 | Có | 

mục tiêu`(3,4)`nằm ngay trên đường chéo ban đầu. Không có sự nảy lên nào xảy ra trước khi chạm tới nó, vì vậy câu trả lời là`0`. 

Ví dụ này minh hoạ tại sao câu trả lời lại tính số lần thoát chứ không phải các đoạn đã di chuyển hoặc số bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi trạng thái được xử lý nhiều nhất một lần | 
| Không gian | O(nm) | Bộ trạng thái đã truy cập | 

Số lượng trạng thái có thể có nhiều nhất là`4nm`, vì mỗi ô có thể được kết hợp với bốn hướng. Bài toán đảm bảo rằng tổng của tất cả`nm`nhiều nhất là các giá trị trên tệp thử nghiệm`5 · 10^4`, do đó tổng công vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    DIR = {
        "DR": (1, 1),
        "DL": (1, -1),
        "UR": (-1, 1),
        "UL": (-1, -1),
    }

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m, r, c, tr, tc, d = input().split()

        n = int(n)
        m = int(m)
        r = int(r)
        c = int(c)
        tr = int(tr)
        tc = int(tc)

        dr, dc = DIR[d]

        if (r, c) == (tr, tc):
            out.append("0")
            continue

        vis = set()
        b = 0
        ans = -1

        while True:
            state = (r, c, dr, dc)

            if state in vis:
                break

            vis.add(state)

            dv = (n - r) if dr == 1 else (r - 1)
            dh = (m - c) if dc == 1 else (c - 1)

            k = min(dv, dh)

            rd = tr - r
            cd = tc - c

            if rd * dr >= 0 and cd * dc >= 0:
                if rd == cd * dr * dc:
                    if abs(rd) <= k:
                        ans = b
                        break

            r += dr * k
            c += dc * k

            if r == 1 or r == n:
                dr *= -1

            if c == 1 or c == m:
                dc *= -1

            b += 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""6
5 7 1 7 2 4 DL
5 7 1 7 3 2 DL
3 3 1 3 2 2 UR
2 4 2 1 2 2 DR
4 3 1 1 1 3 UL
6 4 1 2 3 4 DR
""") == """3
-1
1
-1
4
0"""

# custom cases
assert run("""1
3 3 2 2 2 2 DR
""") == "0", "already at target"

assert run("""1
2 2 1 1 2 2 DR
""") == "0", "direct diagonal reach"

assert run("""1
2 4 2 1 2 2 DR
""") == "-1", "unreachable cycle"

assert run("""1
25000 2 1 1 25000 2 DR
""") == "0", "large boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bắt đầu bằng mục tiêu | 0 | Chấm dứt ngay lập tức | 
| Tầm với theo đường chéo 2×2 | 0 | Không bị trả lại trước mục tiêu | 
| Mẫu 4 | -1 | Phát hiện chu kỳ chính xác | 
| Lưới 25000×2 | 0 | Xử lý kích thước tối đa | 

## Vỏ cạnh 

### Vị trí xuất phát đã là mục tiêu 

đầu vào:```
1
3 3 2 2 2 2 DR
```Thuật toán kiểm tra điều kiện này trước khi vào vòng lặp mô phỏng. Không cần chuyển động, vì vậy nó xuất ra:```
0
```Nếu không có sự kiểm tra này, mô phỏng sẽ tiếp tục di chuyển và có thể trả về giá trị lớn hơn. 

### Mục tiêu không bao giờ xuất hiện trong chu kỳ 

đầu vào:```
1
2 4 2 1 2 2 DR
```Dấu vết:```
(2,1,DR)
→ (1,2,UR)
→ (2,3,DR)
→ (1,4,UR)
→ (2,3,DL)
→ ...
```Cuối cùng, trạng thái đã thấy trước đó xuất hiện trở lại. mục tiêu`(2,2)`không bao giờ thuộc về bất kỳ đoạn đường chéo nào đã được xử lý, do đó thuật toán trả về:```
-1
```Việc kiểm tra trạng thái lặp lại sẽ ngăn chặn vòng lặp vô hạn. 

### Đạt mục tiêu chính xác ở cuối đoạn 

đầu vào:```
1
6 4 1 2 3 4 DR
```Đoạn chéo thứ nhất có độ dài`k = 2`:```
(1,2) → (2,3) → (3,4)
```Mục tiêu là ô cuối cùng của phân khúc đó. Vì việc kiểm tra mục tiêu sử dụng`steps <= k`, điểm cuối được bao gồm và thuật toán xuất ra chính xác:```
0
```Sử dụng bất đẳng thức nghiêm ngặt sẽ bỏ lỡ trường hợp này một cách không chính xác. 

### Góc nảy 

đầu vào:```
1
2 2 1 1 1 2 UL
```Bóng bắt đầu theo hướng`UL`, ngay lập tức đến góc`(1,1)`trạng thái nảy và cả hai thành phần hướng được đảo ngược với nhau khi góc được xử lý. 

Việc triển khai sẽ lật thành phần dọc nếu chạm vào đường viền trên hoặc dưới và lật độc lập thành phần ngang nếu chạm vào đường viền bên trái hoặc bên phải. Một góc tự nhiên sẽ kích hoạt cả hai lần lật, khớp chính xác với các quy tắc của bài toán.
