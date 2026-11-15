---
title: "CF 1840F - Súng ray"
description: "Chúng ta di chuyển trên một lưới tọa độ từ (0, 0) đến (n, m). Mỗi giây chúng ta có thể tăng tọa độ đầu tiên thêm một, tăng tọa độ thứ hai thêm một hoặc giữ nguyên vị trí hiện tại. Vị trí sau hành động là vị trí được kiểm tra đối với tất cả các phát súng Railgun được bắn vào giây đó."
date: "2026-06-09T06:26:27+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1840
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 878 (Div. 3)"
rating: 2200
weight: 1840
solve_time_s: 114
verified: true
draft: false
---

[CF 1840F - Súng điện từ](https://codeforces.com/problemset/problem/1840/F) 

**Xếp hạng:** 2200 
**Tags:** bạo lực, dfs và tương tự, dp, đồ thị 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi di chuyển trên một lưới tọa độ từ`(0, 0)`ĐẾN`(n, m)`. 

Mỗi giây chúng ta có thể tăng tọa độ đầu tiên thêm một, tăng tọa độ thứ hai thêm một hoặc giữ nguyên vị trí hiện tại. Vị trí sau hành động là vị trí được kiểm tra đối với tất cả các phát súng Railgun được bắn vào giây đó. 

Ảnh chụp ngang có tọa độ`x`giết chết mọi vị trí có tọa độ đầu tiên bằng`x`. Ảnh chụp dọc có tọa độ`y`giết chết mọi vị trí có tọa độ thứ hai bằng`y`. 

Mục tiêu là đạt`(n, m)`càng sớm càng tốt mà không bao giờ phải đứng trên ô bị trúng đạn vào thời điểm tương ứng. 

Quan sát đầu tiên xuất phát từ cấu trúc của chuyển động. Nếu chúng ta hiện đang ở`(i, j)`và đã chờ đợi chính xác`k`lần cho đến nay thì thời gian hiện tại được xác định hoàn toàn:$$t = i + j + k$$Mỗi bước di chuyển theo chiều dọc đều tăng`i`, mỗi lần di chuyển ngang đều tăng`j`, và mỗi lần chờ đợi đều tăng lên`k`. 

Các ràng buộc trông khác thường. Lưới có thể chứa tới`10^4`tổng số ô trên tất cả các trường hợp thử nghiệm, đủ lớn để trạng thái trên mỗi ô lưới là ổn. Số lần bắn nhiều nhất`100`, nó rất nhỏ. Thời gian chụp có thể lớn như`10^9`, vì vậy mọi giải pháp mô phỏng rõ ràng thời gian đều không thể thực hiện được. 

Câu hỏi quan trọng là chúng ta cần xem xét bao nhiêu sự chờ đợi. Vì chỉ có`r ≤ 100`những cú đánh, một con đường tối ưu không bao giờ cần nhiều hơn`r`chờ đợi. Điều này làm giảm không gian trạng thái từ hàng tỷ khoảnh khắc thời gian xuống chỉ còn khoảng`100`các lớp. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử một cảnh quay xảy ra vào một thời điểm quan trọng:```
1 1
1
1000000000 1 0
```Câu trả lời vẫn là`2`. Một giải pháp cố gắng xây dựng dòng thời gian với thời gian quay lớn nhất sẽ rất chậm. 

Một trường hợp tinh vi khác đang chờ đợi trong phòng giam.```
1 1
1
1 1 0
```Vào thời điểm`1`, hàng ngang`0`bị bắn. Chúng ta không thể ở lại`(0,0)`trong giây đầu tiên. Cách an toàn duy nhất là rời đi ngay lập tức. Một giải pháp chỉ kiểm tra các ô sau khi di chuyển mà bỏ qua việc chờ đợi sẽ cho phép đứng đó một cách không chính xác. 

Trường hợp thứ ba xảy ra khi toàn bộ khu vực được bao phủ đồng thời.```
3 3
6
2 1 0
2 1 1
2 1 2
2 2 0
2 2 1
2 2 2
```Vào thời điểm`2`, mọi vị trí có thể tiếp cận đều bị phá hủy. Không có con đường nào tồn tại nên câu trả lời là`-1`. Những quyết định tham lam của địa phương không thể phát hiện ra điều này; chúng ta cần tính toán khả năng tiếp cận toàn cầu. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực là coi một quốc gia như`(x, y, time)`và chạy BFS. Từ mỗi trạng thái chúng ta có thể di chuyển xuống, di chuyển sang phải hoặc chờ đợi. Cách tiếp cận này đúng vì tất cả các hành động đều tốn một giây và BFS tìm thấy hành động đến sớm nhất. 

Vấn đề là chiều thời gian. Thời gian bắn đạt`10^9`, do đó, ngay cả việc lưu trữ các trạng thái có thể truy cập theo thời gian cũng không thể thực hiện được. 

Quan sát quan trọng là thời gian không độc lập. Nếu chúng ta biết vị trí`(x, y)`và bao nhiêu lần chờ đợi`k`đã được sử dụng thì thời gian hiện tại chính xác là$$x + y + k.$$Điều này thay thế tọa độ thời gian khổng lồ bằng tọa độ chờ rất nhỏ. 

Tại sao chúng ta có thể hạn chế`k`nhiều nhất là`r`? Mọi sự chờ đợi thêm chỉ tồn tại để tránh bị bắn. Có nhiều nhất`r`tổng số lần chụp, vì vậy hãy xem xét nhiều hơn`r`sự chờ đợi không bao giờ có thể cải thiện được một giải pháp tối ưu. Đây là lực nén trung tâm giúp giải quyết được vấn đề. 

Bây giờ chúng tôi xây dựng DP trên các tiểu bang`(x, y, k)`. 

một trạng thái có nghĩa là: 

“Chúng ta có thể đứng ở vị trí`(x, y)`sau khi sử dụng chính xác`k`chờ đã, trong khi sống sót sau tất cả các phát bắn cho đến nay?" 

Từ`(x, y, k)`chúng ta có thể đến từ:`(x-1, y, k)`bằng cách di chuyển theo chiều dọc.`(x, y-1, k)`bằng cách di chuyển theo chiều ngang.`(x, y, k-1)`bằng cách chờ đợi một giây. 

Điều kiện bổ sung duy nhất là bản thân trạng thái hiện tại không được bị giết vào thời điểm đó.$$t = x + y + k.$$Số lượng các trạng thái chỉ là$$(n+1)(m+1)(r+1),$$đó là khoảng một triệu trong trường hợp thử nghiệm tồi tệ nhất có thể xảy ra, trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên`(x,y,time)`| Không thể do thời gian lên tới`10^9`| Không thể | Quá chậm | 
| DP bật`(x,y,waits)`|`O(nmr)`|`O(nmr)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng ba chiều`free[x][y][k]`.`free[x][y][k]`cho biết liệu nhà nước`(x, y, k)`là an toàn. 
2. Với mỗi phát bắn, hãy đánh dấu tất cả các trạng thái sẽ bị tiêu diệt bởi phát bắn đó. 

Nếu cú ​​đánh ngang trúng hàng`x = coord`vào thời điểm đó`t`, thì mọi trạng thái đều thỏa mãn$$x = coord,\quad t = x + y + k$$bị cấm. 

Sắp xếp lại mang lại$$k = t - coord - y.$$Đối với mỗi cột`y`, nếu điều này`k`nằm ở`[0,r]`, đánh dấu trạng thái là không an toàn. 
3. Xử lý ảnh dọc tương tự. 

Đối với một cột bắn vào`y = coord`và thời gian`t`:$$k = t - coord - x.$$Mọi trạng thái hợp lệ khớp với phương trình đó đều trở nên không an toàn. 
4. Tạo mảng DP`dp[x][y][k]`.`dp[x][y][k] = True`có nghĩa là trạng thái có thể truy cập được và an toàn. 
5. Khởi tạo`dp[0][0][0] = True`. 
6. Lặp lại tất cả các trạng thái theo thứ tự tăng dần`x`,`y`, Và`k`. 

Nếu trạng thái không an toàn, hãy bỏ qua nó. 

Nếu không, hãy tiếp cận nó từ: 

-`(x-1, y, k)`-`(x, y-1, k)`-`(x, y, k-1)`bất cứ khi nào những trạng thái đó có thể truy cập được. 
7. Kiểm tra tất cả các trạng thái`(n, m, k)`. 

Nếu có thể đến được thì thời gian đến là$$n + m + k.$$Chọn thời gian nhỏ nhất như vậy. 
8. Nếu không tồn tại trạng thái đầu cuối có thể truy cập, đầu ra`-1`. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[x][y][k]`đúng chính xác khi tồn tại một chuỗi hành động hợp lệ đạt đến vị trí`(x,y)`sau khi sử dụng`k`chờ đợi và sống sót sau mỗi phát bắn gặp phải. 

Mọi hành vi pháp lý đều thay đổi đúng một trong ba đại lượng`x`,`y`, hoặc`k`bởi một, do đó mọi đường đi hợp lệ đều tương ứng với một chuỗi chuyển tiếp DP. Ngược lại, mỗi lần chuyển đổi DP đại diện cho một hành động pháp lý. Vì các trạng thái không an toàn sẽ bị loại bỏ trước nên mọi trạng thái DP có thể tiếp cận đều tương ứng với trạng thái trò chơi có thể sống sót. 

Vì thời gian được xác định duy nhất bởi`x+y+k`, mọi ràng buộc về lượt bắn đều được kiểm tra vào đúng thời điểm. Do đó, DP mô tả chính xác tất cả các đường dẫn hợp lệ và giá trị có thể truy cập tối thiểu của`n+m+k`là thời gian đến sớm nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())
        r = int(input())

        free = [[[True] * (r + 1) for _ in range(m + 1)]
                for _ in range(n + 1)]

        for _ in range(r):
            tt, d, coord = map(int, input().split())

            if d == 1:
                x = coord
                if 0 <= x <= n:
                    for y in range(m + 1):
                        k = tt - x - y
                        if 0 <= k <= r:
                            free[x][y][k] = False
            else:
                y = coord
                if 0 <= y <= m:
                    for x in range(n + 1):
                        k = tt - y - x
                        if 0 <= k <= r:
                            free[x][y][k] = False

        dp = [[[False] * (r + 1) for _ in range(m + 1)]
              for _ in range(n + 1)]

        dp[0][0][0] = free[0][0][0]

        for x in range(n + 1):
            for y in range(m + 1):
                for k in range(r + 1):
                    if x == 0 and y == 0 and k == 0:
                        continue

                    if not free[x][y][k]:
                        continue

                    ok = False

                    if x > 0 and dp[x - 1][y][k]:
                        ok = True
                    if y > 0 and dp[x][y - 1][k]:
                        ok = True
                    if k > 0 and dp[x][y][k - 1]:
                        ok = True

                    dp[x][y][k] = ok

        ans = -1

        for k in range(r + 1):
            if dp[n][m][k]:
                ans = n + m + k
                break

        print(ans)

solve()
```Giai đoạn tiền xử lý chuyển đổi thông tin bắn trực tiếp sang trạng thái DP bị cấm. Đây là phần thanh lịch nhất của giải pháp. Thay vì hỏi liệu một trạng thái có bị bắn trúng hay không, chúng tôi đảo ngược phương trình và đánh dấu trực tiếp mọi trạng thái sẽ bị tiêu diệt. 

DP sử dụng chính xác ba hành động có sẵn trong trò chơi. Di chuyển thay đổi theo chiều dọc`x`, di chuyển thay đổi theo chiều ngang`y`, và chờ thay đổi`k`. 

Một sai lầm dễ mắc phải là quên rằng chờ đợi là một quá trình chuyển đổi thực sự. Nếu không có sự chuyển đổi từ`(x,y,k-1)`ĐẾN`(x,y,k)`, thuật toán sẽ không bao giờ khám phá ra những đường đi cố tình trì hoãn chính chúng. 

Một điểm tinh tế khác là việc khởi tạo. Nếu như`(0,0,0)`bản thân nó bị cấm, trạng thái bắt đầu phải được coi là không thể truy cập được. 

Câu trả lời cuối cùng là giá trị nhỏ nhất có thể đạt được của`n+m+k`, đó chính xác là thời gian đến. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
4
1 2 0
2 2 1
3 2 2
4 1 1
```Các trạng thái có thể truy cập có liên quan: 

| Vị trí | Đợi k | Thời gian | 
| --- | --- | --- | 
| (0,0) | 0 | 0 | 
| (0,1) | 0 | 1 | 
| (0,2) | 0 | 2 | 
| (0,3) | 0 | 3 | 
| (0,3) | 1 | 4 | 
| (1,3) | 1 | 5 | 

Việc di chuyển đến`(1,3)`vào thời điểm đó`4`sẽ gây tử vong vì hàng`1`bị bắn vào thời điểm`4`. Chờ một giây sẽ thay đổi thời gian đến thành`5`, làm cho con đường trở nên an toàn. 

Trả lời:`5`. 

Dấu vết này chứng tỏ tại sao việc chờ đợi phải là một phần của không gian trạng thái. Đường đi hình học ngắn nhất có chiều dài`4`, nhưng sự sống còn cần thêm một giây. 

### Ví dụ 2 

đầu vào:```
3 3
6
2 1 0
2 1 1
2 1 2
2 2 0
2 2 1
2 2 2
```Vào thời điểm`2`, mọi trạng thái có hàng`0..2`hoặc cột`0..2`bị phá hủy. 

| Thời gian | Các trạng thái có thể tiếp cận an toàn | 
| --- | --- | 
| 0 | (0,0) | 
| 1 | (1,0), (0,1), (0,0) sau khi chờ | 
| 2 | không | 

Vì mọi trạng thái có thể có thể đạt được theo thời gian`2`bị cấm, DP không thể truyền bá thêm. 

Trả lời:`-1`. 

Ví dụ này xác nhận rằng DP xử lý chính xác các ràng buộc đồng thời loại bỏ tất cả các đường dẫn có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nmr)`| Một trạng thái DP trên mỗi`(x,y,k)`| 
| Không gian |`O(nmr)`| Cửa hàng`free`Và`dp`mảng | 

Từ`r ≤ 100`và tổng giá trị của`n·m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^4`, tổng số tiểu bang là khoảng`10^6`. Điều này thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    out = []

    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())
        r = int(input())

        free = [[[True] * (r + 1) for _ in range(m + 1)]
                for _ in range(n + 1)]

        for _ in range(r):
            tt, d, coord = map(int, input().split())

            if d == 1:
                for y in range(m + 1):
                    k = tt - coord - y
                    if 0 <= k <= r:
                        free[coord][y][k] = False
            else:
                for x in range(n + 1):
                    k = tt - coord - x
                    if 0 <= k <= r:
                        free[x][coord][k] = False

        dp = [[[False] * (r + 1) for _ in range(m + 1)]
              for _ in range(n + 1)]

        dp[0][0][0] = free[0][0][0]

        for x in range(n + 1):
            for y in range(m + 1):
                for k in range(r + 1):
                    if x == 0 and y == 0 and k == 0:
                        continue
                    if not free[x][y][k]:
                        continue

                    dp[x][y][k] = (
                        (x > 0 and dp[x - 1][y][k]) or
                        (y > 0 and dp[x][y - 1][k]) or
                        (k > 0 and dp[x][y][k - 1])
                    )

        ans = -1
        for k in range(r + 1):
            if dp[n][m][k]:
                ans = n + m + k
                break

        out.append(str(ans))

    return "\n".join(out)

# sample
assert run("""1
1 3
4
1 2 0
2 2 1
3 2 2
4 1 1
""") == "5"

# minimum grid
assert run("""1
1 1
1
100 1 0
""") == "2"

# must move immediately
assert run("""1
1 1
1
1 1 0
""") == "2"

# impossible wall of shots
assert run("""1
3 3
6
2 1 0
2 1 1
2 1 2
2 2 0
2 2 1
2 2 2
""") == "-1"

# destination requires one wait
assert run("""1
1 1
1
2 1 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1x1`, bắn muộn không liên quan |`2`| Số lần bắn lớn không thành vấn đề | 
| Bắn vào hàng bắt đầu vào thời điểm 1 |`2`| Phải di chuyển thay vì chờ đợi | 
| Bảo hiểm đồng thời |`-1`| Toàn cầu không thể | 
| Bắn vào thời điểm đích |`3`| Chờ đợi có thể trì hoãn việc đến nơi một cách an toàn | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
1
1000000000 1 0
```Cảnh quay xảy ra vượt xa mọi thời điểm hợp lý. Trong quá trình tiền xử lý, giá trị tính toán của`k`rất lớn và rơi ra ngoài`[0,r]`, do đó không có trạng thái DP nào bị đánh dấu là bị cấm. DP tìm đường đi có độ dài bình thường`2`và kết quả đầu ra:```
2
```Bây giờ hãy xem xét:```
1
1 1
1
1 1 0
```Vào thời điểm`1`, mọi vị trí có hàng`0`bị phá hủy. Nhà nước`(0,0,1)`bị cấm nên việc chờ đợi ngay lập tức là không thể. Thay vào đó, DP đạt được`(1,0,0)`sau một giây và sau đó`(1,1,0)`sau hai giây. Câu trả lời là:```
2
```Cuối cùng:```
1
3 3
6
2 1 0
2 1 1
2 1 2
2 2 0
2 2 1
2 2 2
```Mỗi trạng thái có thể truy cập vào thời điểm đó`2`được đánh dấu là bị cấm. Không có quá trình chuyển đổi DP nào có thể vượt qua lớp đó. Đích không bao giờ có thể truy cập được và thuật toán trả về chính xác:```
-1
```Những ví dụ này bao gồm ba lỗi triển khai phổ biến nhất: xử lý thời gian bắn lớn, xử lý thời gian chờ không chính xác và không lập mô hình các hạn chế gây chết người đồng thời.
