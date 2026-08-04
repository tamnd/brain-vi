---
title: "CF 102625E - Kế hoạch của nhà độc tài cho ngày Valentine!"
description: "Vấn đề mô tả một đường kéo dài từ điểm bắt đầu Ruby ở tọa độ 0 về phía Cổng chính. Các lính canh đứng ở tọa độ cố định và mỗi lính canh chỉ hoạt động trong một khoảng thời gian nhất định."
date: "2026-08-03T15:19:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "E"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 55
verified: true
draft: false
---

[CF 102625E - Kế hoạch của nhà độc tài cho ngày lễ tình nhân!](https://codeforces.com/problemset/problem/102625/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả một đường kéo dài từ điểm bắt đầu Ruby tại tọa độ`0`hướng về Cổng Chính. Các lính canh đứng ở tọa độ cố định và mỗi lính canh chỉ hoạt động trong một khoảng thời gian nhất định. Các cặp đôi bắt đầu đi bộ từ Ruby với thời gian xuất phát là số nguyên khác nhau và di chuyển một đơn vị khoảng cách mỗi giây. Đối với mỗi cặp đôi, chúng ta cần xác định khoảng cách họ đi bộ trước khi gặp người bảo vệ đầu tiên, hoặc báo cáo`-1`nếu họ đến đích mà không gặp ai. 

Một người bảo vệ ở tọa độ`X`đang hoạt động trong khoảng thời gian`[L-z, R-z]`, Ở đâu`z`là một giá trị dương nhỏ nhỏ hơn`0.1`. Một cặp đôi bắt đầu từ lúc nào đó`T`chính xác sau đó đã đến được chỗ người bảo vệ này`X`giây, tại thời điểm`T + X`. Điều kiện để bị bắt là:`L - z <= T + X <= R - z`Sắp xếp lại:`L - X - z <= T <= R - X - z`Các giá trị đầu vào là số nguyên, trong khi`z`là một phần dương rất nhỏ. Điều này thay đổi ranh giới số nguyên. Số nguyên hợp lệ nhỏ nhất`T`là`L - X`, bởi vì`L - X`vẫn lớn hơn`L - X - z`. Số nguyên hợp lệ lớn nhất`T`là`R - X - 1`, bởi vì`R - X`đã quá lớn so với phân số`z`. 

Vì vậy, mỗi người bảo vệ có thể được xem như đang tạo ra một khoảng thời gian:`[L - X, R - X - 1]`Trong mỗi thời gian bắt đầu nguyên trong khoảng thời gian này, người bảo vệ đó có thể bắt được cặp đôi. 

Giới hạn cho phép lên tới`200000`lính canh và`200000`các cặp đôi. Bất kỳ cách tiếp cận nào để kiểm tra từng cặp đôi với từng người bảo vệ sẽ yêu cầu khoảng`4 * 10^10`hoạt động vượt xa những gì một vài giây cho phép. Chúng ta cần một giải pháp gần`O((M+N) log M)`. 

Một lỗi phổ biến là quên mất tác dụng của phân số`z`. Ví dụ:```
1 1
0 1 5
5
```Người bảo vệ hoạt động từ`-0.1`ĐẾN`0.9`. Cặp đôi đạt được sự phối hợp`5`vào thời điểm đó`10`, vậy câu trả lời là`-1`. Một chuyển đổi sai sử dụng`[L-X, R-X]`sẽ bao gồm không chính xác thời gian bắt đầu`-5`chỉ trong các tình huống khác và có thể thay đổi ranh giới một. 

Một trường hợp phức tạp khác là một bộ bảo vệ có điểm cuối bên phải đạt được chính xác mà không cần dịch chuyển phân số:```
1 1
10 20 5
15
```Cặp đôi đến gặp người bảo vệ đúng lúc`20`. Khoảng thời gian hoạt động ban đầu kết thúc lúc`19.9`, nên người bảo vệ không còn hiện diện nữa. Đầu ra đúng là:```
-1
```Xử lý khoảng thời gian được đóng bằng các điểm cuối số nguyên sẽ xuất ra không chính xác`5`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng từng cặp đôi một cách độc lập. Vào mỗi thời điểm bắt đầu, chúng tôi kiểm tra từng người bảo vệ và xem liệu cặp đôi có tiếp cận được người bảo vệ đó khi nó đang hoạt động hay không. Nếu có nhiều lính canh trùng nhau, chúng ta giữ tọa độ nhỏ nhất vì cặp đôi dừng lại ở lính canh đầu tiên gặp phải. Điều này đúng vì nó trực tiếp mô phỏng chuyển động, nhưng trường hợp xấu nhất sẽ thực hiện`M * N`séc. Với cả hai giá trị bằng`200000`, điều này trở thành khoảng bốn mươi tỷ so sánh. 

Quan sát hữu ích là các truy vấn đã được sắp xếp theo thời gian bắt đầu. Thay vì hỏi từng người bảo vệ về từng cặp đôi, chúng ta có thể lướt qua thời gian một lần. Mỗi người bảo vệ sẽ hoạt động vào thời điểm`L-X`và ngừng hoạt động sau một thời gian`R-X-1`. Trong khi xử lý các cặp theo thứ tự tăng dần của`T`, chúng ta chỉ cần biết tọa độ nhỏ nhất trong số các lính canh hiện đang hoạt động. 

Một hàng đợi ưu tiên là đủ cho việc này. Khi một người bảo vệ bắt đầu hoạt động, chúng tôi chèn tọa độ và thời gian kết thúc của nó. Trước khi trả lời một truy vấn, chúng tôi loại bỏ tất cả các bộ bảo vệ có thời gian kết thúc nhỏ hơn thời gian bắt đầu hiện tại. Tọa độ tối thiểu còn lại trong heap là người bảo vệ đầu tiên mà cặp đôi sẽ gặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(MN) | O(1) | Quá chậm | 
| Tối ưu | O((M+N) log M) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi lần bảo vệ thành một khoảng thời gian. Đối với người bảo vệ tại tọa độ`X`, lưu trữ rằng nó đang hoạt động trong thời gian bắt đầu từ`L-X`ĐẾN`R-X-1`. Tọa độ`X`được lưu trữ cùng với khoảng thời gian vì đó là giá trị chúng ta cần xuất ra. 
2. Sắp xếp tất cả các khoảng bảo vệ được chuyển đổi theo thời gian bắt đầu của chúng. Sắp xếp rất hữu ích vì các cặp cũng đến theo thứ tự tăng dần, do đó, các vệ sĩ có thể được thêm chính xác khi chúng trở nên phù hợp. 
3. Xử lý các cặp từ thời điểm bắt đầu nhỏ nhất đến lớn nhất. Trước khi trả lời một cặp, hãy thêm mọi người bảo vệ có khoảng thời gian chuyển đổi bắt đầu không muộn hơn thời điểm hiện tại. Đây là những người bảo vệ mới duy nhất có thể ảnh hưởng đến cặp đôi này hoặc cặp đôi sau này. 
4. Loại bỏ các vệ sĩ khỏi hàng ưu tiên khi thời gian kết thúc của họ nhỏ hơn thời gian bắt đầu của cặp đôi hiện tại. Những người bảo vệ như vậy đã ngừng hoạt động. 
5. Nếu heap trống, không người bảo vệ nào có thể bắt được cặp đôi, vì vậy đầu ra`-1`. Ngược lại, tọa độ nhỏ nhất trong heap là đối tượng bảo vệ đầu tiên gặp phải. 

Tại sao nó hoạt động: 

Tại bất kỳ thời điểm truy vấn`T`, heap chứa chính xác các bộ bảo vệ có khoảng thời gian được chuyển đổi bao gồm`T`. Mọi bảo vệ không có trong đống đều bắt đầu muộn hơn hoặc đã hết hạn. Trong số tất cả những người bảo vệ đang hoạt động, cặp đôi này đạt đến tọa độ nhỏ nhất trước tiên, do đó lấy mức tối thiểu`X`đưa ra câu trả lời đúng 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    data = input().split()
    if not data:
        return

    M, N = map(int, data)

    guards = []
    for _ in range(M):
        L, R, X = map(int, input().split())
        guards.append((L - X, R - X - 1, X))

    guards.sort()

    queries = []
    for _ in range(N):
        queries.append(int(input()))

    heap = []
    ans = []
    idx = 0

    for t in queries:
        while idx < M and guards[idx][0] <= t:
            start, end, x = guards[idx]
            heapq.heappush(heap, (x, end))
            idx += 1

        while heap and heap[0][1] < t:
            heapq.heappop(heap)

        if heap:
            ans.append(str(heap[0][0]))
        else:
            ans.append("-1")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc chuyển đổi bảo vệ là phần tế nhị nhất trong quá trình thực hiện. Thời điểm kết thúc phải là`R-X-1`, không`R-X`, bởi vì khoảng thời gian ban đầu kết thúc một chút trước thời gian nguyên đó do giá trị dương`z`. 

Các cửa hàng heap`(coordinate, ending_time)`. Tọa độ là ưu tiên hàng đầu vì chúng ta cần người bảo vệ gần nhất. Thời gian kết thúc được giữ lại để các phần bảo vệ đã hết hạn có thể được gỡ bỏ sau này. 

Các truy vấn đã được sắp xếp trong đầu vào nên không cần sắp xếp thêm. Con trỏ`idx`chỉ di chuyển về phía trước, nghĩa là mỗi người bảo vệ vào đống một lần và rời khỏi nó nhiều nhất một lần. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
4 6
1 3 2
7 13 10
18 20 13
3 4 2
0
1
2
3
5
8
```Khoảng bảo vệ được biến đổi là: 

| Tọa độ | Thời gian bắt đầu | Thời gian kết thúc | 
| --- | --- | --- | 
| 2 | -1 | 0 | 
| 10 | -3 | 2 | 
| 13 | 5 | 6 | 
| 2 | 1 | 1 | 

Xử lý một số truy vấn đầu tiên: 

| Thời gian truy vấn | Đã thêm bảo vệ | Đã loại bỏ bảo vệ | Đống tối thiểu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | tọa độ 2,10 | không | 2 | 2 | 
| 1 | tọa độ 2 | không | 2 | 2 | 
| 2 | không | hết tọa độ 2 ở cuối 1 | 10 | 10 | 
| 3 | không | hết tọa độ 10 | trống | -1 | 

Dấu vết cho thấy đống này luôn chỉ chứa những người bảo vệ hiện có thể bắt được cặp đôi. Tọa độ tối thiểu giữa chúng trùng với điểm va chạm vật lý đầu tiên. 

Một ví dụ nhỏ thứ hai:```
2 3
5 9 3
20 30 10
0
2
10
```Khoảng thời gian được chuyển đổi: 

| Tọa độ | Bắt đầu | Kết thúc | 
| --- | --- | --- | 
| 3 | 2 | 5 | 
| 10 | 10 | 19 | 

Xử lý: 

| Thời gian truy vấn | Bảo vệ tích cực | Trả lời | 
| --- | --- | --- | 
| 0 | không | -1 | 
| 2 | tọa độ 3 | 3 | 
| 10 | tọa độ 10 | 10 | 

Điều này kiểm tra ranh giới kích hoạt chính xác và cho thấy lý do tại sao sự dịch chuyển phân số lại quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((M+N) log M) | Mỗi bộ bảo vệ được chèn và xóa một lần và mỗi thao tác trên heap tiêu tốn thời gian logarit. | 
| Không gian | O(M) | Hàng đợi ưu tiên lưu trữ các bảo vệ đang hoạt động. | 

Với`200000`lính canh và`200000`cặp đôi, phương pháp quét giữ số lượng thao tác trong phạm vi yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        M, N = map(int, input().split())
        guards = []
        for _ in range(M):
            L, R, X = map(int, input().split())
            guards.append((L - X, R - X - 1, X))
        guards.sort()

        heap = []
        idx = 0
        res = []

        for _ in range(N):
            t = int(input())
            while idx < M and guards[idx][0] <= t:
                heapq.heappush(heap, (guards[idx][2], guards[idx][1]))
                idx += 1
            while heap and heap[0][1] < t:
                heapq.heappop(heap)
            res.append(str(heap[0][0]) if heap else "-1")

        return "\n".join(res)

    out = solve()
    sys.stdin = old
    return out

assert run("""4 6
1 3 2
7 13 10
18 20 13
3 4 2
0
1
2
3
5
8
""") == "2\n2\n10\n-1\n13\n-1"

assert run("""1 1
0 1 5
0
""") == "-1"

assert run("""2 2
1 10 3
1 10 7
4
5
""") == "3\n3"

assert run("""1 3
10 20 5
5
10
15
""") == "-1\n5\n-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảo vệ duy nhất bên ngoài tất cả các truy vấn |`-1`| Xử lý không có trường hợp bảo vệ hoạt động | 
| Bảo vệ chồng chéo |`3, 3`| Xác nhận chọn người bảo vệ gần nhất | 
| Kích hoạt và hết hạn ranh giới |`-1, 5, -1`| Kiểm tra phân số`z`chuyển đổi | 

## Vỏ cạnh 

Người canh gác kết thúc đúng lúc một cặp đôi đến không được bắt họ. Đầu vào:```
1 1
10 20 5
15
```đưa ra một khoảng biến đổi`[5,14]`. Thời gian truy vấn là`15`, nằm ngoài khoảng thời gian, do đó heap loại bỏ phần bảo vệ trước khi trả lời và trả về`-1`. 

Phải bao gồm một người bảo vệ bắt đầu chính xác khi một cặp đôi có thể tiếp cận nó. Vì:```
1 1
3 4 2
1
```khoảng biến đổi là`[1,1]`. Bảo vệ được chèn vào trước thời gian xử lý`1`, vẫn hoạt động và câu trả lời là`2`. 

Nhiều vệ sĩ cùng tọa độ cũng được xử lý một cách tự nhiên. Đống có thể chứa một số mục có tọa độ bằng nhau và việc trả về giá trị tối thiểu vẫn cho khoảng cách dừng chính xác. Việc đảm bảo đầu vào rằng các khoảng ở cùng tọa độ không trùng nhau sẽ ngăn các trạng thái đồng thời xung đột gây ra sự mơ hồ.
