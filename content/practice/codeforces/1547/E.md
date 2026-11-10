---
title: "CF 1547E - Máy Điều Hòa Không Khí"
description: "Chúng ta có một dải một chiều có độ dài $n$. Một số vị trí trên dải này chứa các máy điều hòa không khí, mỗi vị trí cố định ở một tọa độ xác định và mỗi vị trí tạo ra nhiệt độ cơ bản riêng."
date: "2026-06-14T19:58:16+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "implementation", "shortest-paths", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1547
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 731 (Div. 3)"
rating: 1500
weight: 1547
solve_time_s: 465
verified: false
draft: false
---

[CF 1547E - Máy điều hòa không khí](https://codeforces.com/problemset/problem/1547/E) 

**Đánh giá:** 1500 
**Thẻ:** cấu trúc dữ liệu, dp, triển khai, đường dẫn ngắn nhất, sắp xếp, hai con trỏ 
**Thời gian giải:** 7 phút 45 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dải chiều dài một chiều$n$. Một số vị trí trên dải này chứa các máy điều hòa không khí, mỗi vị trí cố định ở một tọa độ xác định và mỗi vị trí tạo ra nhiệt độ cơ bản riêng. Mỗi ô khác thừa hưởng một nhiệt độ được xác định bởi khoảng cách của nó đến tất cả các máy điều hòa không khí: từ mỗi máy điều hòa, phần đóng góp cho một ô là nhiệt độ cơ bản cộng với khoảng cách tuyệt đối đến ô đó và nhiệt độ cuối cùng là mức tối thiểu của tất cả những đóng góp đó. 

Vì vậy, mỗi máy điều hòa hoạt động giống như một nguồn “tỏa” nhiệt độ tăng tuyến tính khi chúng ta di chuyển ra xa nó. Chúng tôi muốn, đối với mọi vị trí trên đường, giá trị nhỏ nhất trong số tất cả các hàm tăng trưởng tuyến tính này. 

Kích thước đầu vào cho phép lên đến$3 \cdot 10^5$tổng số ô trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng tính toán câu trả lời cho từng ô bằng cách quét tất cả các bộ điều hòa một cách độc lập sẽ có hiệu suất lên tới$O(nk)$các hoạt động trong một trường hợp thử nghiệm, thoái hóa thành$9 \cdot 10^{10}$trong trường hợp xấu nhất và không khả thi. Điều này ngay lập tức loại trừ lực lượng vũ phu trên mỗi tế bào. 

Một chi tiết cấu trúc quan trọng là mỗi máy điều hòa không khí đóng góp một chức năng hình chữ V$t_i + |a_i - x|$. Câu trả lời cuối cùng là đường bao dưới của các hình chữ V này. 

Một trường hợp thất bại phổ biến đối với lối suy luận ngây thơ là chỉ tính toán không chính xác các máy điều hòa không khí gần nhất. Ví dụ: nếu hai bộ điều hòa ở vị trí 1 và 100 có nhiệt độ tương tự nhau, thì một ô ở giữa bị ảnh hưởng bởi cả hai và mức tối thiểu không nhất thiết phải là từ ô gần nhất trong khoảng cách thô khi nhiệt độ cơ bản khác nhau đáng kể. 

Một trường hợp phức tạp khác là khi một máy điều hòa ở xa nhưng có nhiệt độ cơ bản nhỏ hơn nhiều. Cách tiếp cận chỉ có vị trí gần nhất tham lam không thành công ở đó vì nó bỏ qua các khoảng lệch dọc. 

## Phương pháp tiếp cận 

Phương pháp brute-force đánh giá từng tế bào một cách độc lập. Đối với một vị trí cố định$i$, nó kiểm tra tất cả các điều hòa và tính toán$t_j + |a_j - i|$, lấy mức tối thiểu. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Chi phí đến từ việc lặp lại tính toán này cho tất cả$n$tế bào, cho$O(nk)$các thao tác trên mỗi trường hợp kiểm thử trong trường hợp xấu nhất, quá chậm khi cả hai$n$Và$k$lớn. 

Quan sát quan trọng là mỗi điều kiện xác định một hàm trên dòng và câu trả lời là mức tối thiểu của các hàm này tại mọi điểm. Thay vì coi đây là các truy vấn độc lập, chúng tôi diễn giải lại nó như một bài toán đường đi ngắn nhất nhiều nguồn trên biểu đồ đường: mỗi điều hòa là một nguồn có khoảng cách ban đầu$t_i$và di chuyển sang trái hoặc phải sẽ tăng chi phí thêm 1 mỗi bước. Đây chính xác là đường dẫn ngắn nhất trên biểu đồ trong đó mọi nút kết nối với các nút lân cận có trọng số là 1. 

Cấu trúc này cho phép lan truyền giống như BFS hoặc Dijkstra đa nguồn. Vì tất cả các trọng số của cạnh là 1, nên chúng ta có thể khởi tạo tất cả các nguồn cùng một lúc và truyền ra ngoài theo thứ tự khoảng cách tăng dần bằng cách sử dụng logic hàng đợi ưu tiên hoặc deque. Sự đơn giản hóa tối ưu là điều này trở thành “đường dẫn ngắn nhất đa nguồn trên một đường” cổ điển, giúp giảm xuống mức quét tuyến tính sau khi sắp xếp các nguồn. 

Chúng tôi xử lý từ trái sang phải và từ phải sang trái vì mỗi hướng sẽ tính toán độc lập mức đóng góp tốt nhất từ ​​các bộ điều hòa ở phía đó. Câu trả lời cuối cùng là mức tối thiểu của hai lần quét này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$|$O(1)$| Quá chậm | 
| Tuyên truyền hai chiều |$O(n + k)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi đường này là thứ mà ảnh hưởng lan ra bên ngoài từ mỗi chất điều hòa và chúng tôi tính toán các giá trị tối thiểu riêng biệt từ cả hai hướng. 

1. Khởi tạo một mảng`ans`kích thước$n$với vô cùng. Điều này thể hiện nhiệt độ tốt nhất chưa được biết đến cho đến nay. 
2. Đối với mỗi máy điều hòa tại vị trí$a_i$, bộ`ans[a_i] = t_i`. Điều này mã hóa thực tế rằng tại nguồn, chi phí chính xác là nhiệt độ cơ bản của nó. 
3. Quét từ trái sang phải. Duy trì giá trị tốt nhất có thể đến từ phía bên trái. Tại vị trí$i$, ảnh hưởng tốt nhất còn lại là bắt đầu sử dụng dầu xả mới ở$i$hoặc kéo dài mức tốt nhất trước đó lên +1. Chúng tôi cập nhật`ans[i] = min(ans[i], ans[i-1] + 1)`. Bước này đảm bảo chúng ta nắm bắt được tác dụng của tất cả các chất điều hòa ở bên trái lan sang bên phải. 
4. Quét đối xứng từ phải sang trái. Duy trì sự lan truyền từ phía bên phải. Tại vị trí$i$, cập nhật`ans[i] = min(ans[i], ans[i+1] + 1)`. Điều này đảm bảo các bộ điều hòa bên phải tác động sang bên trái một cách chính xác. 
5. Xuất kết quả cuối cùng`ans`mảng. 

Lý do mỗi lần quét sử dụng quá trình chuyển đổi +1 đơn giản là vì việc di chuyển một ô sẽ tăng khoảng cách lên đúng 1, do đó, mỗi bước sẽ mô hình hóa chính xác việc mở rộng khoảng cách tuyệt đối. 

### Tại sao nó hoạt động 

Mỗi bộ điều hòa xác định một trường khoảng cách mở rộng đồng đều theo cả hai hướng. Đường từ trái sang phải tính toán mức tối thiểu trên tất cả các đường đi qua một chuỗi các bước di chuyển phải bắt đầu từ bất kỳ nguồn nào và đường chuyền từ phải sang trái cũng thực hiện tương tự đối với các bước di chuyển trái. Vì bất kỳ đường dẫn tối ưu nào từ nguồn đến ô đều đơn điệu theo một hướng sau khi rời khỏi nguồn, nên nó phải được ghi lại hoàn toàn bằng một trong hai lần quét này. Lấy mức tối thiểu của cả hai hướng sẽ tái tạo lại đường bao đầy đủ của tất cả$t_i + |a_i - x|$chức năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    INF = 10**18

    for _ in range(q):
        input()
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        t = list(map(int, input().split()))

        ans = [INF] * n

        for i in range(k):
            ans[a[i] - 1] = t[i]

        for i in range(1, n):
            ans[i] = min(ans[i], ans[i - 1] + 1)

        for i in range(n - 2, -1, -1):
            ans[i] = min(ans[i], ans[i + 1] + 1)

        print(*ans)

if __name__ == "__main__":
    solve()
```Bước khởi tạo đặt tất cả các máy điều hòa không khí trực tiếp vào mảng. Việc quét từ trái sang phải mô hình chính xác mức độ lan truyền ảnh hưởng tăng dần từng bước. Việc quét từ phải sang trái sẽ khắc phục các trường hợp nguồn tốt nhất nằm ở bên phải. Việc sử dụng một lượng lớn`INF`đảm bảo các ô không được khởi tạo không lấn át giá trị cực tiểu một cách không chính xác. 

Một lỗi triển khai phổ biến là quên chuyển đổi vị trí sang lập chỉ mục dựa trên số 0. Một vấn đề tế nhị khác là ghi đè thay vì lấy mức tối thiểu trong quá trình truyền bá, điều này sẽ loại bỏ các đường dẫn tốt hơn một cách không chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một trường hợp nhỏ:$n=6$, điều hòa ở vị trí 2 và 5 với nhiệt độ 14 và 16. 

Trạng thái ban đầu sau khi đặt: 

| tôi | 1 | 2 | 3 | 4 | 5 | 6 | 
| --- | --- | --- | --- | --- | --- | --- | 
| trả lời | thông tin | 14 | thông tin | thông tin | 16 | thông tin | 

Sau khi quét từ trái sang phải: 

| tôi | 1 | 2 | 3 | 4 | 5 | 6 | 
| --- | --- | --- | --- | --- | --- | --- | 
| trả lời | thông tin | 14 | 15 | 16 | 16 | 17 | 

Sau khi quét từ phải sang trái: 

| tôi | 1 | 2 | 3 | 4 | 5 | 6 | 
| --- | --- | --- | --- | --- | --- | --- | 
| trả lời | 15 | 14 | 15 | 16 | 16 | 17 | 

Điều này xác nhận mức độ ảnh hưởng của cả hai bên hợp nhất để tạo thành đường bao tối thiểu chính xác. 

Ví dụ thứ hai là máy điều hòa đơn ở vị trí 4 có giá trị 10 in$n=7$. Việc truyền lan tạo ra sự gia tăng tuyến tính đối xứng ra bên ngoài, xác nhận rằng cả hai lần quét đều tái tạo chính xác hành vi khoảng cách tuyệt đối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi lần quét thăm mỗi ô một lần, cộng thêm$O(k)$khởi tạo | 
| Không gian |$O(n)$| Mảng lưu trữ các giá trị tốt nhất hiện tại cho từng vị trí | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng của$n$, phù hợp thoải mái trong giới hạn của$3 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    q = int(input())
    INF = 10**18
    out_lines = []

    for _ in range(q):
        input()
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        t = list(map(int, input().split()))

        ans = [INF] * n

        for i in range(k):
            ans[a[i] - 1] = t[i]

        for i in range(1, n):
            ans[i] = min(ans[i], ans[i - 1] + 1)

        for i in range(n - 2, -1, -1):
            ans[i] = min(ans[i], ans[i + 1] + 1)

        out_lines.append(" ".join(map(str, ans)))

    return "\n".join(out_lines)

assert run("""5

6 2
2 5
14 16

10 1
7
30

5 5
3 1 4 2 5
3 1 4 2 5

7 1
1
1000000000

6 3
6 1 3
5 5 5
""") == """15 14 15 16 16 17
36 35 34 33 32 31 30 31 32 33
1 2 3 4 5
1000000000 1000000001 1000000002 1000000003 1000000004 1000000005 1000000006
5 6 5 6 6 5"""

assert run("""1

1 1
1
10
""") == "10"

assert run("""1

5 1
3
7
""") == "9 8 7 8 9"

assert run("""1

6 2
1 6
5 1
""") == "5 2 3 4 5 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 10 | trường hợp ranh giới tối thiểu | 
| điều hòa trung tâm đơn | lan truyền đối xứng | tính đúng đắn của cả hai lần quét | 
| thái cực đối lập | ảnh hưởng hỗn hợp | hành vi phong bì đúng | 

## Vỏ cạnh 

Một trường hợp điều hòa đơn tối thiểu như$n=1, k=1$với vị trí 1 trực tiếp khởi tạo mảng và không có sự lan truyền nào làm thay đổi nó, xác nhận tính chính xác của các đầu vào suy biến. 

Ví dụ: trường hợp có một dầu xả ở phía bên trái$n=5$,$a_1=1$, cho thấy chỉ việc quét từ trái sang phải mới quan trọng như thế nào. Thuật toán bắt đầu với ans = [t, inf, inf, inf, inf], sau đó lan truyền đến [t, t+1, t+2, t+3, t+4], khớp chính xác với định nghĩa khi khoảng cách tăng thêm một trên mỗi bước.
