---
title: "CF 102591I - \u0413\u0440\u043e\u043c\u043a\u043e\u0441\u0442\u044c \u0434\u0438\u043d\u0430\u043c\u0438\u043a\u0430"
description: "Âm lượng loa của máy tính hiện được đặt thành X và chúng tôi muốn thay đổi nó thành Y. Mỗi giây chúng tôi có thể thực hiện chính xác một thao tác. Một thao tác tăng hoặc giảm âm lượng thêm 1 hoặc Z."
date: "2026-07-31T06:26:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102591
codeforces_index: "I"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041c\u0423\u0418\u0422 \u043f\u043e \u0441\u043f\u043e\u0440\u0442\u0438\u0432\u043d\u043e\u043c\u0443 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2020. \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0442\u0443\u0440."
rating: 0
weight: 102591
solve_time_s: 356
verified: true
draft: false
---

[CF 102591I - \u0413\u0440\u043e\u043c\u043a\u043e\u0441\u0442\u044c \u0434\u0438\u043d\u0430\u043c\u0438\u043a\u0430](https://codeforces.com/problemset/problem/102591/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Âm lượng loa của máy tính hiện được đặt thành`X`, và chúng tôi muốn thay đổi nó thành`Y`. Mỗi giây chúng ta có thể thực hiện chính xác một thao tác. Một thao tác tăng hoặc giảm âm lượng bằng cách`1`hoặc bởi`Z`. Âm lượng phải luôn nằm trong phạm vi hợp lệ từ`0`ĐẾN`100`, bao gồm các giá trị trung gian đạt được sau mỗi thao tác. Nhiệm vụ là tính số giây tối thiểu cần thiết để đạt được âm lượng mục tiêu. 

Những hạn chế là cực kỳ nhỏ. Mỗi tập có thể có là một số nguyên nằm giữa`0`Và`100`, do đó toàn bộ không gian trạng thái chỉ chứa`101`đỉnh. Một biểu đồ nhỏ như vậy cho phép chúng ta khám phá trực tiếp mọi trạng thái có thể tiếp cận. Ngay cả khi mỗi trạng thái tạo ra bốn lần chuyển đổi, tổng khối lượng công việc sẽ vẫn ở mức dưới một nghìn thao tác. Bất kỳ thuật toán đồ thị nào như Tìm kiếm theo chiều rộng đều phù hợp thoải mái trong giới hạn. 

Một số trường hợp biên rất dễ bị bỏ sót khi cố gắng rút ra một công thức toán học thay vì tìm kiếm trong không gian trạng thái. 

Nếu như`Z = 0`, bước chuyển lớn không làm gì cả. Ví dụ:```
Input:
5 8 0
```Câu trả lời đúng là`3`, bởi vì chỉ`+1`thay đổi âm lượng. Một công thức dựa trên việc chia cho`Z`sẽ thất bại vì phép chia cho số 0 không được xác định. 

Một trường hợp tế nhị khác xuất hiện khi việc tạm thời rời xa mục tiêu lại có lợi. Coi như:```
Input:
1 4 5
```Câu trả lời là`3`. Một trình tự tối ưu là`1 -> 0 -> 5 -> 4`. Một chiến lược tham lam chỉ hướng tới mục tiêu sẽ không bao giờ phát hiện ra con đường này. 

Các giá trị biên cũng quan trọng. Ví dụ:```
Input:
100 99 10
```Câu trả lời là`1`. Từ khối lượng`100`chúng ta không thể tăng thêm`10`, vì các trạng thái trung gian phải nằm trong phạm vi hợp lệ. Bất kỳ việc triển khai nào cũng phải từ chối các bước di chuyển rời khỏi khoảng thời gian`[0, 100]`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xem mọi tập có thể có dưới dạng trạng thái. Từ một tập chúng ta có thể thử bốn thao tác: cộng`1`, trừ`1`, thêm vào`Z`, và trừ`Z`. Mỗi thao tác tốn một giây, vì vậy mọi cạnh đều có trọng số bằng nhau. Chạy Tìm kiếm theo chiều rộng đầu tiên từ các trạng thái truy cập khối lượng bắt đầu theo thứ tự tăng dần về thời gian cần thiết. Lần đầu tiên chúng tôi đạt được mục tiêu, chúng tôi đã tìm thấy số giây tối thiểu. 

Ngay cả việc tìm kiếm vũ phu cũng đã nhanh chóng vì chỉ có`101`tiểu bang. Trong trường hợp xấu nhất, Tìm kiếm theo chiều rộng xử lý mỗi trạng thái một lần và kiểm tra bốn lần chuyển tiếp đi, dẫn đến khoảng`404`kiểm tra chuyển tiếp. 

Việc cố gắng xây dựng một công thức dạng đóng khó hơn nhiều so với lần đầu tiên nó xuất hiện. Khả năng di chuyển theo cả hai hướng, hạn chế mọi âm lượng trung gian đều ở bên trong`[0, 100]`, và trường hợp đặc biệt`Z = 0`tạo ra nhiều trường hợp góc. Việc coi bài toán là bài toán có đường đi ngắn nhất sẽ loại bỏ tất cả những rắc rối này. Tìm kiếm theo chiều rộng xử lý một cách tự nhiên các đường vòng, ranh giới và các hoạt động lặp đi lặp lại mà không yêu cầu lý do riêng cho từng tình huống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force, Bề rộng Tìm kiếm đầu tiên trên tất cả các tiểu bang | O(101) | O(101) | Đã chấp nhận | 
| Tối ưu, theo chiều rộng Tìm kiếm đầu tiên trên tất cả các tiểu bang | O(101) | O(101) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng khoảng cách có kích thước`101`và khởi tạo mọi giá trị thành`-1`. Mảng này ghi lại số giây tối thiểu cần thiết để đạt được mỗi tập. 
2. Đặt khoảng cách âm lượng bắt đầu`X`ĐẾN`0`và đẩy`X`vào hàng đợi. Tìm kiếm theo chiều rộng luôn mở rộng các trạng thái theo thứ tự khoảng cách tăng dần. 
3. Liên tục xóa âm lượng phía trước khỏi hàng đợi. 
4. Tạo bốn tập tiếp theo có thể bằng cách áp dụng`+1`,`-1`,`+Z`, Và`-Z`. 
5. Bỏ qua mọi tập được tạo nằm ngoài khoảng`[0, 100]`, bởi vì việc di chuyển như vậy là không được phép. 
6. Nếu một tập hợp lệ chưa được truy cập trước đó, hãy chỉ định khoảng cách của nó là khoảng cách hiện tại cộng với một và đẩy nó vào hàng đợi. Lần truy cập đầu tiên luôn tối ưu vì Tìm kiếm theo chiều rộng sẽ khám phá các đường dẫn ngắn hơn trước các đường dẫn dài hơn. 
7. Tiếp tục cho đến khi hàng đợi trống hoặc đã đạt đến khối lượng mục tiêu. 
8. Xuất ra khoảng cách đã ghi cho âm lượng`Y`. 

### Tại sao nó hoạt động 

Tìm kiếm theo chiều rộng đầu tiên duy trì tính bất biến rằng mọi trạng thái bị xóa khỏi hàng đợi đều có khoảng cách tối thiểu có thể so với trạng thái bắt đầu. Vì mỗi thao tác được phép tốn chính xác một giây nên tất cả các cạnh của đồ thị đều có trọng số bằng nhau. Khi một trạng thái mới được phát hiện, nó sẽ đạt được thông qua chuỗi hoạt động ngắn nhất có thể. Mọi trình tự nhấn nút hợp lệ đều tương ứng với một đường dẫn trong biểu đồ này và mọi đường dẫn biểu đồ tương ứng với một chuỗi thao tác hợp pháp. Vì Tìm kiếm theo chiều rộng tính toán các đường đi ngắn nhất trong biểu đồ không có trọng số nên khoảng cách được ghi cho`Y`chính xác là thời gian tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    x, y, z = map(int, input().split())

    dist = [-1] * 101
    dist[x] = 0

    q = deque([x])

    while q:
        cur = q.popleft()
        if cur == y:
            break

        for nxt in (cur + 1, cur - 1, cur + z, cur - z):
            if 0 <= nxt <= 100 and dist[nxt] == -1:
                dist[nxt] = dist[cur] + 1
                q.append(nxt)

    print(dist[y])

if __name__ == "__main__":
    solve()
```Mảng khoảng cách phục vụ hai mục đích. Nó lưu trữ thời gian ngắn nhất được biết đến và cũng đánh dấu xem một tập đã được truy cập hay chưa. Điều này tránh việc xử lý cùng một trạng thái nhiều lần. 

Mỗi lần lặp lại xem xét chính xác bốn nước đi ứng cử viên. Việc kiểm tra ranh giới diễn ra trước khi truy cập mảng khoảng cách sao cho các khối lượng không hợp lệ, chẳng hạn như`-1`hoặc`101`được bỏ qua một cách an toàn. 

Việc thoát sớm khi`cur == y`là tùy chọn nhưng tránh được những công việc không cần thiết sau khi đã tìm được câu trả lời tối ưu. 

Việc thực hiện cũng xử lý`Z = 0`một cách tự nhiên. Trong trường hợp đó sự chuyển tiếp`cur + z`Và`cur - z`cả hai đều bằng nhau`cur`, đã được truy cập nên không có vòng lặp vô hạn nào xảy ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
0 10 3
```| Bước | Khối lượng hiện tại | Mới Đạt | Khoảng cách | 
| --- | --- | --- | --- | 
| 1 | 0 | 1, 3 | 1 | 
| 2 | 3 | 6 | 2 | 
| 3 | 6 | 9 | 3 | 
| 4 | 9 | 10 | 4 | 

Trình tự ngắn nhất là`0 -> 3 -> 6 -> 9 -> 10`. Tìm kiếm đầu tiên theo chiều rộng đạt đến số lượng`10`sau bốn thao tác, khớp với câu trả lời mẫu. 

### Mẫu 2 

đầu vào:```
10 0 3
```| Bước | Khối lượng hiện tại | Mới Đạt | Khoảng cách | 
| --- | --- | --- | --- | 
| 1 | 10 | 9, 7, 13 | 1 | 
| 2 | 7 | 6, 4 | 2 | 
| 3 | 4 | 3, 1 | 3 | 
| 4 | 1 | 0 | 4 | 

Một con đường ngắn nhất là`10 -> 7 -> 4 -> 1 -> 0`. Thuật toán tìm thấy mục tiêu sau bốn giây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(101) | Mỗi tập được xử lý tối đa một lần, với bốn lần chuyển đổi được kiểm tra từ mỗi trạng thái. | 
| Không gian | O(101) | Mảng khoảng cách và hàng đợi lưu trữ tối đa 101 trạng thái. | 

Vì chỉ có`101`khối lượng có thể, thời gian chạy là không đổi một cách hiệu quả. Giải pháp dễ dàng đáp ứng mọi giới hạn cuộc thi hợp lý. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solve():
    input = sys.stdin.readline
    x, y, z = map(int, input().split())

    dist = [-1] * 101
    dist[x] = 0
    q = deque([x])

    while q:
        cur = q.popleft()
        if cur == y:
            break
        for nxt in (cur + 1, cur - 1, cur + z, cur - z):
            if 0 <= nxt <= 100 and dist[nxt] == -1:
                dist[nxt] = dist[cur] + 1
                q.append(nxt)

    print(dist[y])

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue()

assert run("0 10 3\n") == "4\n", "sample 1"
assert run("10 0 3\n") == "4\n", "sample 2"
assert run("0 0 5\n") == "0\n", "already at target"
assert run("5 8 0\n") == "3\n", "Z equals zero"
assert run("100 99 10\n") == "1\n", "upper boundary"
assert run("1 4 5\n") == "3\n", "temporary detour"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 5`|`0`| Khối lượng ban đầu đã bằng mục tiêu. | 
|`5 8 0`|`3`| Nước đi lớn có độ dài bằng không. | 
|`100 99 10`|`1`| Di chuyển ra ngoài phạm vi hợp lệ sẽ bị từ chối. | 
|`1 4 5`|`3`| Con đường ngắn nhất có thể tạm thời di chuyển ra khỏi mục tiêu. | 

## Vỏ cạnh 

Khi nào`Z = 0`, biểu đồ chứa các vòng lặp tự tạo bởi`+Z`Và`-Z`. Coi như:```
Input:
5 8 0
```Việc tìm kiếm bắt đầu từ`5`. Các nước đi được tạo ra bằng cách cộng hoặc trừ số 0 sẽ quay trở lại`5`, đã được truy cập nên chúng bị bỏ qua. Tìm kiếm đầu tiên theo chiều rộng tiếp tục thông qua`6`,`7`, Và`8`, đưa ra câu trả lời đúng`3`. 

Khi cần đi đường vòng, Tìm kiếm theo chiều rộng vẫn thành công vì nó khám phá tất cả các trạng thái ở khoảng cách hiện tại trước khi chuyển sang các con đường dài hơn. Đối với đầu vào```
1 4 5
```việc tìm kiếm đạt tới`0`sau một bước thì`5`sau hai bước, và cuối cùng`4`sau ba bước. Một thuật toán tham lam luôn làm giảm sự khác biệt tuyệt đối đối với mục tiêu sẽ không bao giờ xem xét chuỗi này. 

Khi khối lượng hiện tại đã đạt đến giới hạn tối đa, những động thái bất hợp pháp sẽ bị loại bỏ ngay lập tức. Vì```
100 99 10
```sự chuyển đổi sang`110`bị bỏ qua vì nó nằm ngoài phạm vi hợp lệ. Việc di chuyển hợp pháp còn lại tới`99`đạt được mục tiêu trong một giây, đó là câu trả lời tối ưu.
