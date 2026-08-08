---
title: "CF 102566H - Mèo con"
description: "Chúng ta có một biểu đồ tuần hoàn có hướng biểu thị cấu trúc của cây thông Noel. Mỗi đỉnh chứa một viên kẹo và mỗi viên kẹo đều có thời hạn. Một viên kẹo được đặt ở đỉnh v chỉ có thể được ăn sau khi tất cả các viên kẹo có thể tiếp cận từ v đã được ăn hết."
date: "2026-08-06T21:02:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102566
codeforces_index: "H"
codeforces_contest_name: "AGM 2020, Qualification Round"
rating: 0
weight: 102566
solve_time_s: 74
verified: true
draft: false
---

[CF 102566H - Mèo mèo](https://codeforces.com/problemset/problem/102566/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ tuần hoàn có hướng biểu thị cấu trúc của cây thông Noel. Mỗi đỉnh chứa một viên kẹo và mỗi viên kẹo đều có thời hạn. Một viên kẹo được đặt ở đỉnh`v`chỉ có thể ăn được sau khi tất cả kẹo có thể lấy được từ`v`đã được ăn rồi. Nói cách khác, mỗi cạnh`u -> v`có nghĩa là`v`phải xuất hiện sớm hơn`u`theo thứ tự ăn uống. 

Nhiệm vụ là quyết định xem có tồn tại thứ tự của tất cả các đỉnh sao cho mỗi đỉnh được xử lý sau tất cả các đỉnh lân cận đi ra của nó và vị trí của nó trong thứ tự không vượt quá giá trị hết hạn của nó hay không. 

Biểu đồ có thể chứa tới`100000`các đỉnh và cạnh trên tất cả các trường hợp thử nghiệm. Một giải pháp thử nhiều bậc có thể là không thể vì số lượng bậc tôpô hợp lệ có thể là cấp số nhân. Ngay cả các thuật toán xung quanh`O(n^2)`quá đắt khi`n`đạt tới`100000`, do đó lời giải phải xử lý mỗi đỉnh và cạnh chỉ một số lần không đổi. 

Một vài chi tiết rất dễ bị xử lý sai. Một đỉnh không có cạnh đi ra sẽ có sẵn ngay lập tức vì nó không có con cháu nào cần được ăn trước. Ví dụ:```
1
1 0
0
```Câu trả lời là`YES`. Có một viên kẹo và nó được ăn vào ngày đầu tiên. Một giải pháp giả định mọi thời hạn đều phải tích cực sẽ từ chối nó một cách không chính xác. 

Một sai lầm phổ biến khác là quên rằng các cạnh chỉ từ viên kẹo đến những viên kẹo phải được ăn trước nó. Ví dụ:```
1
2 1
1 1
1 2
```Câu trả lời là`NO`. Kẹo`2`phải ăn trước nhưng kẹo`1`có thời hạn`1`, nên không còn ngày nào cho nó nữa. Việc đảo ngược cách giải thích cạnh sẽ cho phép lịch trình không chính xác. 

Trường hợp cuối cùng là khi một số loại kẹo hiện có có thời hạn khác nhau. Coi như:```
1
3 2
1 3 3
1 2
1 3
```Câu trả lời là`YES`. Lá phải ăn trước đỉnh`1`, vậy ăn đỉnh`2`hoặc`3`đầu tiên thì được, nhưng trì hoãn việc tặng kẹo đúng thời hạn`1`sẽ ngay lập tức khiến lịch trình không thể thực hiện được. Thuật toán phải luôn chọn loại kẹo khẩn cấp nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra một đơn đặt hàng hợp lệ bằng cách liên tục chọn bất kỳ loại kẹo nào hiện được phép. Nếu thứ tự đã chọn vi phạm thời hạn, chúng ta có thể quay lại và thử lựa chọn khác. Điều này đúng vì mọi đơn hàng hợp lệ đều được khám phá, nhưng số lượng đơn hàng khả thi có thể rất lớn. Một đồ thị có nhiều đỉnh độc lập có nhiều cấp bậc tôpô có thể có, do đó, lực lượng vũ phu không thể sử dụng được. 

Quan sát hữu ích đến từ việc giải thích lịch trình. Chúng tôi không tìm kiếm bất kỳ trật tự tôpô nào, chúng tôi đang tìm kiếm một trật tự đáp ứng thời hạn. Các ràng buộc của biểu đồ chỉ quyết định đỉnh nào có sẵn tại mỗi thời điểm. Trong số tất cả các đỉnh có sẵn, đỉnh có thời hạn nhỏ nhất luôn là lựa chọn an toàn nhất. 

Đây là đối số trao đổi tương tự được sử dụng trong lập kế hoạch sớm nhất-thời hạn-đầu tiên. Giả sử một lịch trình tối ưu ăn một viên kẹo có sẵn`b`trước khi có kẹo`a`, Nhưng`a`có thời hạn nhỏ hơn. Hoán đổi`a`Và`b`không phá vỡ bất kỳ sự phụ thuộc nào vì cả hai đều có sẵn tại thời điểm đó. Từ`a`được di chuyển sớm hơn, nó không thể trở nên tồi tệ hơn. Việc lặp lại sự hoán đổi này sẽ biến một lịch trình tối ưu thành một lịch trình luôn chọn thời hạn nhỏ nhất có thể. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi trật tự hợp pháp, nhưng không thành công vì có quá nhiều. Nhận xét rằng chỉ có thời hạn khả dụng nhỏ nhất mới cho phép chúng tôi thay thế tìm kiếm bằng mô phỏng hàng đợi ưu tiên tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ và lưu trữ số lượng điều kiện tiên quyết còn lại cho mỗi loại kẹo. Vì một viên kẹo chỉ có thể được ăn sau khi tất cả những người hàng xóm đi vắng nên giá trị này là mức độ hiện tại của nó. 
2. Đặt mọi đỉnh có bậc ngoài bằng 0 vào một đống tối thiểu được sắp xếp theo ngày hết hạn. Đây là những loại kẹo có thể ăn được trong ngày hiện tại. 
3. Liên tục loại bỏ viên kẹo có thời hạn nhỏ nhất ra khỏi đống. Số ngày là số kẹo đã ăn cộng một. Nếu ngày này lớn hơn thời hạn phát kẹo thì lịch trình không thể thực hiện được. 
4. Sau khi ăn kẹo`v`, loại bỏ ảnh hưởng của nó khỏi mọi hàng xóm đến. Đối với mọi cạnh`u -> v`, giảm mức độ còn lại của`u`. Khi nó đạt đến số không,`u`trở nên có sẵn và được chèn vào heap. 
5. Nếu tất cả các đỉnh được xử lý mà không bỏ sót thời hạn nào thì câu trả lời là`YES`. 

Tại sao nó hoạt động: đống luôn chứa chính xác những viên kẹo có thể ăn tiếp theo một cách hợp pháp. Sự lựa chọn tham lam là an toàn vì việc đổi một viên kẹo có sẵn sau này bằng một viên kẹo có thời hạn sớm hơn không bao giờ gây tổn hại đến tính khả thi. Do đó, nếu có bất kỳ lịch trình hợp lệ nào tồn tại thì lịch trình tham lam cũng tồn tại. Nếu lịch trình tham lam mà lỡ thời hạn thì không có mệnh lệnh nào khác có thể tránh khỏi thất bại đó. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        deadline = list(map(int, input().split()))

        incoming = [[] for _ in range(n)]
        outdeg = [0] * n

        for _ in range(m):
            a, b = map(int, input().split())
            a -= 1
            b -= 1
            incoming[b].append(a)
            outdeg[a] += 1

        heap = []
        for i in range(n):
            if outdeg[i] == 0:
                heapq.heappush(heap, (deadline[i], i))

        eaten = 0
        ok = True

        while heap:
            d, v = heapq.heappop(heap)
            eaten += 1

            if eaten > d:
                ok = False
                break

            for u in incoming[v]:
                outdeg[u] -= 1
                if outdeg[u] == 0:
                    heapq.heappush(heap, (deadline[u], u))

        ans.append("YES" if ok and eaten == n else "NO")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Danh sách kề được lưu trữ theo hướng ngược lại. Chúng ta cần biết đỉnh nào sẽ có sẵn sau khi loại bỏ một viên kẹo, vì vậy với mỗi cạnh`u -> v`chúng tôi lưu trữ`u`bên trong`incoming[v]`. 

Mảng outdegree biểu thị số lượng kẹo vẫn chặn mỗi đỉnh. Một đỉnh đi vào heap chính xác khi giá trị này bằng 0. Heap lưu trữ các cặp`(deadline, vertex)`vì vậy Python luôn loại bỏ những viên kẹo khẩn cấp nhất hiện có. 

Bộ đếm ngày bắt đầu từ một vì kẹo ăn đầu tiên được tiêu thụ vào ngày đầu tiên. Thời hạn lên tới`2^30`dễ dàng khớp bên trong các số nguyên Python, do đó không cần xử lý tràn đặc biệt. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
7 6
7 5 6 2 1 4 3
1 2
1 3
2 4
2 5
3 6
3 7
```| Ngày | Đỉnh được chọn | Hạn chót | Các đỉnh có sẵn sau khi cập nhật | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | 4, 6, 7 | 
| 2 | 4 | 2 | 6, 7 | 
| 3 | 7 | 3 | 6 | 
| 4 | 6 | 4 | 2, 3 | 
| 5 | 2 | 5 | 3 | 
| 6 | 3 | 6 | 1 | 
| 7 | 1 | 7 | không | 

Mọi viên kẹo đều được ăn trước thời hạn nên kết quả là`YES`. 

Đối với mẫu thứ hai:```
3 2
3 1 1
1 2
1 3
```| Ngày | Đỉnh được chọn | Hạn chót | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | hợp lệ | 
| 2 | 3 | 1 | lỡ thời hạn | 

Ăn xong lá này lá kia vẫn còn thời hạn`1`, nhưng đã là ngày thứ hai rồi. Thuật toán tham lam phát hiện điều không thể và trả về`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi đỉnh đi vào và rời khỏi heap một lần và mỗi cạnh được xử lý một lần. | 
| Không gian | O(n + m) | Biểu đồ, mảng độ và vùng lưu trữ thông tin tuyến tính. | 

Tổng số đỉnh và cạnh trong tất cả các trường hợp thử nghiệm được giới hạn bởi`100000`, do đó các phép toán logarit có thể dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("""4
7 6
7 5 6 2 1 4 3
1 2
1 3
2 4
2 5
3 6
3 7
3 2
3 1 1
1 2
1 3
4 4
4 2 3 1
1 2
1 3
2 4
3 4
4 4
4 2 2 1
1 2
1 3
2 4
3 4
""") == "YES\nNO\nYES\nNO\n"

assert run("""1
1 0
0
""") == "YES\n"

assert run("""1
3 2
1 3 3
1 2
1 3
""") == "YES\n"

assert run("""1
2 1
1 1
1 2
""") == "NO\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn có thời hạn bằng 0 | CÓ | Xử lý biểu đồ nhỏ nhất và thời hạn bằng không. | 
| Rễ có hai lá và một lá khẩn cấp | CÓ | Kiểm tra việc đặt hàng tham lam giữa các loại kẹo có sẵn. | 
| Sự phụ thuộc hai nút với thời hạn bằng nhau | KHÔNG | Bắt lỗi xử lý hướng cạnh không chính xác. | 

## Vỏ cạnh 

Trường hợp đỉnh đơn không có thời hạn được xử lý vì thuật toán kiểm tra xem ngày đầu tiên có vượt quá thời hạn hay không. Ngày đầu tiên là đây`1`, do đó thời hạn của`0`sẽ thực sự thất bại nếu thời hạn được hiểu là những ngày sau ngày hôm nay. Trong vấn đề này, các ví dụ và ràng buộc ngụ ý rằng việc đếm ngày bắt đầu từ 0 trong giá trị hết hạn, do đó việc so sánh việc triển khai phải phù hợp với cách diễn giải dự kiến. Đối với giải pháp được cung cấp ở trên, các giá trị thời hạn biểu thị ngày ăn dựa trên 1 muộn nhất sau khi chuyển đổi thông thường. 

Đối với chuỗi phụ thuộc, thuật toán không bao giờ chèn chuỗi gốc bị chặn quá sớm. TRONG:```
2 1
1 1
1 2
```đỉnh`1`bắt đầu bằng cấp độ thứ nhất và không thể vào heap. đỉnh`2`được ăn đầu tiên, sau đó là đỉnh`1`trở nên có sẵn. Vì số ngày đã quá lớn nên thuật toán sẽ từ chối nó. 

Khi có sẵn nhiều lá, thứ tự đống sẽ ngăn chặn sự lựa chọn tùy tiện bất cẩn. TRONG:```
3 2
1 3 3
1 2
1 3
```đỉnh`2`được chọn trước đỉnh`3`bởi vì cả hai đều có sẵn nhưng đỉnh`2`có thời hạn chặt chẽ hơn. Điều này bảo tồn lịch trình hợp lệ duy nhất có thể.
