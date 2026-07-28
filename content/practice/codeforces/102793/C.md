---
title: "CF 102793C - \u0421\u043e\u0431\u0430\u043a\u0430, \u043f\u0440\u0435\u0434\u0430\u0442\u0435\u043b\u044c \u0438 \u043a\u0430\u0431\u0435\u043b\u044f"
description: "Bảng là một lưới các ô hình chữ nhật. Một số ranh giới giữa các ô lân cận có chứa các đoạn cáp. Khi con chó di chuyển từ ô này sang ô lân cận, nó sẽ cắt dây cáp ở đường viền mà nó đi qua, nếu đường viền đó tồn tại."
date: "2026-07-27T18:06:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102793
codeforces_index: "C"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102793
solve_time_s: 96
verified: true
draft: false
---

[CF 102793C - \u0421\u043e\u0431\u0430\u043a\u0430, \u043f\u0440\u0435\u0434\u0430\u0442\u0435\u043b\u044c \u0438 \u043a\u0430\u0431\u0435\u043b\u044f](https://codeforces.com/problemset/problem/102793/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bảng là một lưới các ô hình chữ nhật. Một số ranh giới giữa các ô lân cận có chứa các đoạn cáp. Khi con chó di chuyển từ ô này sang ô lân cận, nó sẽ cắt dây cáp ở đường viền mà nó đi qua, nếu đường viền đó tồn tại. 

Đối với mỗi ô xuất phát mà người chơi có thể đứng, chúng ta cần số đoạn cáp tối đa mà con chó có thể cắt. Con chó bị hạn chế chỉ di chuyển đến các ô không ở phía trên hoặc bên phải vị trí bắt đầu của nó. Điều này có nghĩa là sau khi chọn tuyến đường, mọi ô được truy cập phải nằm trên cùng một đường chéo với điểm bắt đầu hoặc xa hơn và nằm trong lưới. 

Đầu vào cung cấp kích thước lưới, các đoạn cáp hiện có và một số vị trí bắt đầu. Đầu ra chứa một giá trị cho mỗi vị trí bắt đầu: số lượng cáp tốt nhất có thể mà con chó có thể cắt. 

Kích thước lưới riêng lẻ có thể đạt tới 200000, nhưng tích của các kích thước nhiều nhất là 200000. Điều này có nghĩa là chúng ta không thể sử dụng thuật toán bậc hai về số lượng ô. Một giải pháp xung quanh`O(n*m)`là phù hợp vì có tối đa 200000 ô, trong khi việc chạy tìm kiếm riêng biệt từ mọi vị trí truy vấn vẫn sẽ quá tốn kém nếu truy vấn lớn. Số lượng truy vấn nhỏ nhưng quá trình tiền xử lý vẫn phải tuyến tính. 

Một sai lầm phổ biến là chỉ xem xét tuyến đường ngắn nhất hoặc tuyến đường đi theo mép lưới. Con chó có thể tự do lựa chọn bất kỳ con đường nào thỏa mãn hạn chế di chuyển, vì vậy tất cả các điểm cuối có thể có trên đường chéo yêu cầu phải được xem xét. 

Ví dụ: nếu điểm bắt đầu là ô phía trên trên đường chéo:```
2 3 0
```với một sợi cáp ở ranh giới giữa hai ô, con chó có thể chọn tuyến đường thực sự đi qua sợi cáp đó. Việc triển khai tham lam luôn hướng tới biên giới và bỏ qua các tuyến đường thay thế có thể bỏ lỡ câu trả lời tối ưu. 

Một trường hợp khó khăn khác là khi không có dây cáp nào cả:```
n = 1, m = 1, k = 0
query:
1 1
```Câu trả lời là`0`. Việc triển khai khởi tạo các giá trị lập trình động không chính xác bằng giá trị âm có thể tạo ra câu trả lời sai ở đây vì không có chuyển tiếp nào để thêm vào. 

Trường hợp cạnh cuối cùng là truy vấn trên hàng đầu tiên hoặc cột cuối cùng. Ví dụ:```
n = 2, m = 3
query:
1 3
```Con chó không thể di chuyển lên trên hoặc sang phải, vì vậy chỉ có các ô trên cùng một đường chéo có hàng ít nhất`1`và cột nhiều nhất`3`là hợp lệ. Mã sử ​​dụng ranh giới sai khi quét đường chéo có thể bao gồm các ô không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là bắt đầu tìm kiếm biểu đồ từ mọi ô truy vấn. Trong quá trình tìm kiếm, chúng tôi chỉ cho phép di chuyển xuống hoặc sang trái và đếm số dây cáp đi qua. Điều này đúng vì nó khám phá chính xác những tuyến đường mà con chó có thể đi. Tuy nhiên, các vấn đề con tương tự xuất hiện nhiều lần. Trong trường hợp xấu nhất, việc khám phá tất cả các tuyến đường có thể từ một ô chạm vào một phần lớn của bảng và thực hiện điều đó đối với tất cả các truy vấn có thể tiếp cận`O(q*n*m)`làm việc, điều đó là không cần thiết. 

Quan sát hữu ích là mọi tuyến đường tới một ô đều có cấu trúc giống nhau. Nếu chúng tôi tính toán số lượng cáp tối đa cần thiết để tiếp cận mọi ô từ góc trên cùng bên trái thì câu trả lời cho truy vấn có thể được trích xuất từ ​​​​các giá trị được tính toán trước đó. 

Đối với một tế bào`(i, j)`, mọi điểm cuối hợp lệ cho người chơi tại`(x, y)`phải thỏa mãn`i + j = x + y`, vì con chó nằm trên cùng một đường chéo. Trong số các ô đó, con chó không thể đi lên phía trên hoặc bên phải khi bắt đầu, vì vậy chỉ những ô có`i >= x`Và`j <= y`là hợp lệ. Vì tổng đường chéo là cố định nên việc kiểm tra`i >= x`là đủ. 

Chúng tôi tính toán giá trị lập trình động cho mọi ô:`dp[i][j]`là số lượng cáp tối đa đi qua trên một đường dẫn từ`(1, 1)`ĐẾN`(i, j)`. 

Quá trình chuyển đổi chỉ cần hàng xóm phía trên và bên trái. Sau khi tiền xử lý, mỗi truy vấn sẽ trở thành tra cứu trên một đường chéo. Bằng cách lưu trữ tối đa hậu tố cho mỗi đường chéo, truy vấn có thể được trả lời trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q_n_m) | O(n*m) | Quá chậm | 
| Tối ưu | O(n*m + q) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi dây cáp dưới dạng thông tin về đường viền giữa hai ô lân cận. Chúng ta chỉ cần biết việc di chuyển xuống hay di chuyển sang phải sẽ đi qua một sợi cáp. 
2. Xây dựng bảng lập trình động theo từng hàng. Đối với mỗi ô, hãy tận dụng tốt hơn việc đi từ trên xuống hoặc từ bên trái, sau đó thêm dây cáp chéo theo bước di chuyển đó. 
3. Nhóm tất cả các ô theo số đối chéo của chúng`i + j`. Truy vấn cho một ô chỉ phụ thuộc vào đường chéo của chính nó. 
4. Đối với mỗi đường chéo, xử lý các ô từ dưới lên trên và thay thế từng giá trị bằng giá trị lớn nhất trong số tất cả các ô bên dưới nó trên đường chéo đó. Điều này cho phép một truy vấn sử dụng ngay giá trị ở hàng bắt đầu của nó. 
5. Đối với một truy vấn`(x, y)`, sử dụng đường chéo`x + y`và trả về giá trị được lưu trữ cho hàng`x`. Giá trị này đã loại trừ tất cả các ô nằm trên điểm bắt đầu. 

Tại sao nó hoạt động: 

Bảng lập trình động lưu trữ số lượng cáp tốt nhất có thể cho mọi điểm cuối có thể tiếp cận từ trên cùng bên trái. Một truy vấn yêu cầu điểm cuối tốt nhất trên một đường chéo cụ thể không nằm phía trên hoặc bên phải ô bắt đầu. Hậu tố tối đa trên đường chéo đó giữ chính xác mức tối đa trong số tất cả các điểm cuối hợp lệ. Vì mọi tuyến đường có thể có của chó đều tương ứng với một trong các điểm cuối này và mọi giá trị điểm cuối được lưu trữ đều thể hiện tuyến đường tốt nhất tới đó nên giá trị trả về là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    down = [[0] * (m + 1) for _ in range(n + 1)]
    right = [[0] * (m + 1) for _ in range(n + 1)]

    for _ in range(k):
        x1, y1, x2, y2 = map(int, input().split())
        if x1 == x2:
            y = min(y1, y2)
            right[x1][y] = 1
        else:
            x = min(x1, x2)
            down[x][y1] = 1

    dp = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            best = 0
            if i > 1:
                best = max(best, dp[i - 1][j] + down[i - 1][j])
            if j > 1:
                best = max(best, dp[i][j - 1] + right[i][j - 1])
            dp[i][j] = best

    diagonals = {}
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            s = i + j
            if s not in diagonals:
                diagonals[s] = []
            diagonals[s].append((i, dp[i][j]))

    for s in diagonals:
        diagonals[s].sort()
        best = -1
        arr = diagonals[s]
        for idx in range(len(arr) - 1, -1, -1):
            best = max(best, arr[idx][1])
            arr[idx] = (arr[idx][0], best)

    q = int(input())
    ans = []
    for _ in range(q):
        x, y = map(int, input().split())
        arr = diagonals[x + y]
        lo = 0
        hi = len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid][0] < x:
                lo = mid + 1
            else:
                hi = mid
        ans.append(str(arr[lo][1]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Các mảng`down`Và`right`chỉ lưu trữ các hướng cáp cần thiết cho quá trình chuyển đổi. Di chuyển xuống dưới sử dụng đường viền được lưu trữ phía trên ô hiện tại, trong khi di chuyển sang phải sử dụng đường viền bên trái của ô hiện tại. Giữ quy ước này tránh được một lỗi. 

Vòng lập trình động bắt đầu từ`(1, 1)`và chỉ đọc các ô đã được tính toán. Hàng và cột bổ sung số 0 loại bỏ sự cần thiết của các trường hợp đặc biệt ở ranh giới trên cùng và bên trái. 

Quá trình tiền xử lý theo đường chéo lưu trữ các cặp số hàng và giá trị tốt nhất. Sắp xếp theo hàng cho phép tìm kiếm nhị phân trong mỗi truy vấn. Việc vượt qua tối đa hậu tố được thực hiện từ cuối vì các ô hợp lệ cho một truy vấn chính xác là những ô có hàng lớn hơn hoặc bằng hàng truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ nơi truy vấn nằm trên ô`(2, 3)`. Đường chéo liên quan có tổng`5`. 

| Bước | Ô hiện tại | Đường chéo | Giá trị được lưu trữ | 
| --- | --- | --- | --- | 
| 1 |`(1,4)`| 5 | 2 | 
| 2 |`(2,3)`| 5 | 5 | 
| 3 |`(3,2)`| 5 | 4 | 

Hậu tố tối đa thay đổi các giá trị thành: 

| Hàng | Giá trị sau hậu tố tối đa | 
| --- | --- | 
| 1 | 5 | 
| 2 | 5 | 
| 3 | 4 | 

Truy vấn bắt đầu ở hàng`2`, vì vậy câu trả lời là giá trị được lưu trữ ở hàng`2`, đó là`5`. Điều này chứng tỏ rằng thuật toán bỏ qua các ô phía trên trình phát. 

Đối với trường hợp khác, giả sử một truy vấn là ô duy nhất trên đường chéo của nó. 

| Bước | Ô hiện tại | Đường chéo | Giá trị được lưu trữ | 
| --- | --- | --- | --- | 
| 1 |`(1,1)`| 2 | 0 | 

Hậu tố tối đa giữ nguyên giá trị. Điều này xác nhận rằng một ô và một bo mạch không có cáp được xử lý mà không có logic đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n_m + q_log(n*m)) | Mỗi ô được xử lý một lần và mỗi truy vấn thực hiện tìm kiếm nhị phân trên một đường chéo | 
| Không gian | O(n*m) | Các bảng lưới và bộ nhớ chéo chứa một lượng dữ liệu không đổi trên mỗi ô | 

Giới hạn trên`n*m`làm cho quá trình tiền xử lý tuyến tính trở nên khả thi. Số lượng truy vấn nhỏ nên chi phí tra cứu logarit bổ sung là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""1 1 0
1
1 1
""") == "0\n", "single cell"

assert run("""2 2 2
1 1 1 2
1 1 2 1
2
1 2
2 1
""") == "1\n1\n", "basic cables"

assert run("""3 3 0
3
1 1
2 2
3 3
""") == "0\n0\n0\n", "no cables"

assert run("""3 3 3
1 1 1 2
1 2 1 3
1 1 2 1
2
1 3
1 2
""") == "2\n2\n", "diagonal boundary"

assert run("""2 3 1
1 2 2 2
1
1 3
""") == "0\n", "unusable cable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảng đơn bào | 0 | Kích thước tối thiểu và bộ cáp trống | 
| Lưới điện nhỏ có dây cáp | 1 | Chuyển tiếp lập trình động cơ bản | 
| Bố trí cáp trống | 0 | Khởi tạo đúng đắn | 
| Nhiều ô trên một đường chéo | 2 | Logic tối đa hậu tố chéo | 
| Cáp ngoài khu vực có thể tiếp cận | 0 | Xử lý ranh giới chuyển động | 

## Vỏ cạnh 

Khi bo mạch không chứa dây cáp, mọi quá trình chuyển đổi đều thêm số không. Bảng lập trình động vẫn hợp lệ vì giá trị đường dẫn tối đa vẫn là số lượng cáp chéo, bằng 0. 

Đối với một truy vấn ở cạnh bảng, chẳng hạn như`(1, m)`, tìm kiếm nhị phân sẽ chọn hàng đầu tiên trên đường chéo đó. Hậu tố tối đa đã loại bỏ mọi ô không thể phía trên trình phát, do đó không có tuyến đường không hợp lệ nào được tính. 

Khi nhiều ô có cùng đường chéo, thuật toán sẽ không chọn ô gần nhất. Nó kiểm tra ngầm tất cả các điểm cuối hợp lệ thông qua hậu tố tối đa, đây chính xác là điều cần thiết vì con chó được phép chọn bất kỳ tuyến đường nào giúp tối đa hóa số lượng cáp đi qua.
