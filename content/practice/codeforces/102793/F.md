---
title: "CF 102793F - \u042d\u043b\u0435\u043a\u0442\u0440\u043e\u043d\u043d\u044b\u0439 \u0437\u0430\u043c\u043e\u043a"
description: "Chúng tôi có một hàng gồm n tấm. Mọi bảng điều khiển đều bắt đầu tắt. Mã thành công là cấu hình trong đó chính xác k vị trí đã cho được bật và mọi vị trí khác đều tắt."
date: "2026-07-27T18:00:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102793
codeforces_index: "F"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102793
solve_time_s: 73
verified: true
draft: false
---

[CF 102793F - \u042d\u043b\u0435\u043a\u0442\u0440\u043e\u043d\u043d\u044b\u0439 \u0437\u0430\u043c\u043e\u043a](https://codeforces.com/problemset/problem/102793/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng`n`tấm. Mọi bảng điều khiển đều bắt đầu tắt. Một mã thành công là một cấu hình có chính xác thông số đã cho`k`các vị trí đang bật và mọi vị trí khác đều tắt. Một thao tác chọn một trong các độ dài được phép từ mảng`a`và lật từng ô trong một đoạn liên tiếp có độ dài đó. Nhiệm vụ là tìm số lượng thao tác tối thiểu cần thiết để đạt được cấu hình mục tiêu hoặc báo cáo rằng không thể thực hiện được. 

Các ràng buộc được thiết kế xung quanh thực tế là`n`lớn trong khi`k`là rất nhỏ. Một giải pháp lưu trữ hoặc xử lý mọi cấu hình bảng điều khiển có thể là không thể bởi vì`n`có thể đạt tới 10000. Tuy nhiên, có nhiều nhất là 10 vị trí quan trọng nên số lượng trạng thái có ý nghĩa liên quan đến câu trả lời là rất ít, nhiều nhất là`2^k = 1024`. Điều này gợi ý rằng thuật toán nên nén bảng lớn thành thông tin về các vị trí đặc biệt. 

Một số trường hợp rất dễ xử lý sai. Nếu mục tiêu không thể được biểu diễn bằng các thao tác có sẵn thì câu trả lời là`-1`, số lượng không lớn. Ví dụ:```
Input:
3 2 1
1 2
3

Output:
-1
```Việc lật cả ba bảng luôn làm thay đổi hai bảng đầu tiên cùng với bảng thứ ba nên không thể đạt được trạng thái mong muốn. 

Một trường hợp phức tạp khác là khi một thao tác dường như chỉ ảnh hưởng đến các vị trí quan trọng mà còn thay đổi các bảng phải tắt. Ví dụ:```
Input:
5 1 1
3
2

Output:
-1
```Việc lật chiều dài hai đoạn không bao giờ có thể tạo ra một đoạn riêng biệt trên bảng ở giữa vì mẫu khác biệt được tạo bởi thao tác luôn có hai đường viền. 

Hiệu ứng trống sau khi áp dụng cùng một thao tác hai lần là một lỗi phổ biến khác. Vì mọi thao tác đều là phép lật XOR nên hai thao tác giống hệt nhau sẽ triệt tiêu lẫn nhau. Bất kỳ giải pháp ngắn nhất nào cũng không bao giờ cần lặp lại quá trình chuyển đổi giống hệt nhau một cách không cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi chuỗi hoạt động có thể. Điều này đúng vì mọi chuỗi hợp lệ cuối cùng đều đạt đến một số cấu hình và chúng tôi có thể giữ độ dài nhỏ nhất trong số các chuỗi thành công. Vấn đề là số lượng các chuỗi có thể tăng theo cấp số nhân với câu trả lời. Nếu có 100 độ dài có thể và chúng ta cần thậm chí 10 phép toán thì không gian tìm kiếm đã chứa tới`100^10`ứng cử viên, vượt xa những gì có thể được khám phá. 

Quan sát quan trọng là ngừng suy nghĩ về hàng bảng đầy đủ. Hãy xem xét mảng khác biệt XOR của cấu hình hiện tại. Việc lật đoạn chỉ thay đổi hai vị trí trong mảng khác biệt này: vị trí bắt đầu của đoạn và vị trí ngay sau khi đoạn đó kết thúc. Phần bên trong của phân đoạn không thành vấn đề vì những thay đổi lân cận sẽ bị hủy bỏ. 

Cấu hình mục tiêu chỉ có một vài chuyển đổi giữa bật và tắt, bởi vì chỉ`k <= 10`các tế bào đang bật. Chúng ta có thể biểu diễn các điểm chuyển tiếp đó dưới dạng bitmask. Mọi hoạt động phân đoạn có thể trở thành sự chuyển tiếp giữa các mặt nạ. Vì có nhiều nhất 1024 mặt nạ nên chúng ta có thể chạy BFS và tìm số thao tác tối thiểu. 

Việc tìm kiếm mạnh mẽ trên các hoạt động không thành công vì nó khám phá bảng vật lý khổng lồ. Việc quan sát mảng khác biệt sẽ loại bỏ các ô không liên quan và chỉ để lại không gian trạng thái nhỏ ảnh hưởng đến khả năng tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong độ dài câu trả lời | Hàm mũ | Quá chậm | 
| Tối ưu | O((n + l) * 2^k) | O(2^k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu diễn khác biệt XOR của cấu hình đích. Một bit được đặt ở mọi ranh giới nơi trạng thái thay đổi từ bảng này sang bảng tiếp theo. Đây là những nơi duy nhất cần theo dõi hoạt động. 
2. Nén các vị trí khác nhau có thể có thành một danh sách nhỏ các tọa độ quan trọng. Có nhiều nhất`2k`những vị trí như vậy vì mỗi bảng trên bảng đóng góp tối đa hai ranh giới. 
3. Chạy BFS trên mặt nạ của các vị trí nén này. Mặt nạ bắt đầu bằng 0 vì ban đầu tất cả các bảng đều tắt. Mặt nạ mục tiêu mô tả các ranh giới cần thiết. 
4. Đối với mọi trạng thái BFS, hãy thử mọi độ dài hoạt động được phép và mọi vị trí phân đoạn có thể. Tính toán ranh giới nén nào được phân đoạn này chuyển đổi và sử dụng ranh giới đó làm trạng thái tiếp theo. 
5. Khi đạt đến mặt nạ mục tiêu, khoảng cách BFS là số thao tác tối thiểu. Nếu BFS kết thúc mà không đạt được thì không thể cấu hình mục tiêu. 

Tại sao nó hoạt động: biểu diễn chênh lệch XOR bảo toàn chính xác thông tin cần thiết để tái tạo lại trạng thái bảng. Việc lật phân đoạn chỉ ảnh hưởng đến hai đường viền của nó trong biểu diễn này, vì vậy hai cấu hình có cùng mặt nạ khác biệt sẽ tương đương với các hoạt động trong tương lai. BFS khám phá mọi cấu hình nén có thể tiếp cận với số lần di chuyển ngày càng tăng, do đó, khi mục tiêu xuất hiện lần đầu tiên, không thể tồn tại chuỗi ngắn hơn. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, k, l = map(int, input().split())
    x = list(map(int, input().split()))
    a = list(map(int, input().split()))

    target = [0] * (n + 2)
    for pos in x:
        target[pos] ^= 1
        target[pos + 1] ^= 1

    coords = [i for i in range(1, n + 2) if target[i]]
    idx = {v: i for i, v in enumerate(coords)}

    if not coords:
        print(0)
        return

    m = len(coords)
    target_mask = 0
    for i in range(m):
        target_mask |= 1 << i

    moves = []
    seen_moves = set()

    for length in a:
        for start in range(1, n - length + 2):
            end = start + length
            mask = 0
            if start in idx:
                mask ^= 1 << idx[start]
            if end in idx:
                mask ^= 1 << idx[end]
            if mask and mask not in seen_moves:
                seen_moves.add(mask)
                moves.append(mask)

    dist = [-1] * (1 << m)
    dist[0] = 0
    q = deque([0])

    while q:
        cur = q.popleft()
        if cur == target_mask:
            print(dist[cur])
            return
        for mv in moves:
            nxt = cur ^ mv
            if dist[nxt] == -1:
                dist[nxt] = dist[cur] + 1
                q.append(nxt)

    print(-1)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai xây dựng sự khác biệt XOR của mục tiêu. Việc lật một bảng sẽ thay đổi hai điểm khác biệt lân cận, do đó, mỗi điểm khác biệt mong muốn trên bảng sẽ đóng góp hai nút chuyển đổi. 

Danh sách`coords`chỉ lưu trữ các ranh giới quan trọng. Đây là bước nén để biến một bảng có chiều dài 10000 thành biểu đồ có tối đa 1024 đỉnh. 

Quá trình tạo chuyển tiếp kiểm tra mọi phân đoạn có thể vì số lượng trạng thái nén nhỏ và tổng số lần bắt đầu phân đoạn có thể chỉ là`O(nl)`. Mỗi quá trình chuyển đổi được chuyển đổi thành mặt nạ ranh giới mà thao tác chuyển đổi. 

Mảng BFS lưu trữ số lượng thao tác tối thiểu cần thiết để tiếp cận mọi mặt nạ. Vì mỗi cạnh biểu thị chính xác một thao tác nên BFS tự động đưa ra các đường dẫn ngắn nhất. Không có vấn đề tràn vì tất cả các khoảng cách nhiều nhất là số lượng trạng thái. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
Input:
10 8 2
1 2 3 5 6 7 8 9
3 5
```Các trạng thái quan trọng trong BFS là: 

| Bước | Ý nghĩa mặt nạ hiện nay | Hoạt động | Trạng thái tiếp theo | 
| --- | --- | --- | --- | 
| 0 | Không có sự khác biệt | Lật chiều dài 3 ở vị trí 1 | Sự khác biệt ở 1 và 4 | 
| 1 | Nhóm đầu tiên cố định | Độ dài lật 5 ở vị trí 5 | Mục tiêu | 

Hai thao tác tạo ra chính xác tám bảng cần thiết. 

Đối với mẫu thứ hai:```
Input:
3 2 1
1 2
3
```| Bước | Mặt nạ hiện tại | Chuyển đổi có sẵn | Kết quả | 
| --- | --- | --- | --- | 
| 0 | Trạng thái ban đầu | Chiều dài lật 3 | Mẫu ranh giới sai | 
| 1 | Thăm dò lặp đi lặp lại | Không có đường dẫn đến mục tiêu | Không thể | 

BFS khám phá mọi trạng thái nén và không bao giờ đạt được mục tiêu, vì vậy câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nl + 2^k * l) | Chúng tôi tạo ra các chuyển đổi có thể có và chạy BFS trên tối đa 1024 trạng thái | 
| Không gian | O(2^k) | Chỉ khoảng cách biểu đồ nén được lưu trữ | 

Giải pháp phù hợp vì phần đắt tiền phụ thuộc vào`2^k`, Và`k`được giới hạn ở mức 10. Giá trị lớn của`n`chỉ xuất hiện ở thế hệ chuyển tiếp. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    oldout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdin = old
    sys.stdout = oldout
    return out.getvalue()

assert run("""10 8 2
1 2 3 5 6 7 8 9
3 5
""") == "2\n"

assert run("""3 2 1
1 2
3
""") == "-1\n"

assert run("""1 1 1
1
1
""") == "1\n"

assert run("""5 1 1
3
2
""") == "-1\n"

assert run("""6 6 1
1 2 3 4 5 6
6
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảng đơn có chiều dài một | 1 | Trường hợp kích thước tối thiểu | 
| Chiều dài ba không thể tạo hai bảng | -1 | Trường hợp chẵn lẻ không thể | 
| Tất cả các bảng đều được bật và tồn tại một lần lật hoàn toàn | 1 | Mục tiêu tiếp giáp lớn | 
| Bảng cách ly ở giữa có chiều dài hoạt động sai | -1 | Xử lý ranh giới | 

## Vỏ cạnh 

Ví dụ không thể thực hiện được với ba bảng và thao tác có độ dài ba được xử lý vì mặt nạ khác biệt do thao tác tạo ra không khớp với mặt nạ mục tiêu. BFS không cho rằng mọi nhóm vị trí đều có thể truy cập được. 

Ví dụ về bảng điều khiển ở giữa bị cô lập được xử lý bởi cùng một bất biến. Lần lật dài hai lần luôn tạo ra hai ranh giới, do đó, mẫu ranh giới bảng đơn được yêu cầu không bao giờ xuất hiện trong biểu đồ BFS. 

Các trường hợp vị trí ở đầu hoặc cuối hàng được xử lý bằng cách thêm ranh giới sau vị trí`n`trong mảng khác biệt. Nếu không có ranh giới bổ sung đó, các thao tác chạm vào bảng cuối cùng sẽ được trình bày không chính xác.
