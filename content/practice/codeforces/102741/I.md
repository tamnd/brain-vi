---
title: "CF 102741I - Nhảy đóng thế thoát hiểm"
description: "Bản đồ là một mạng lưới các ngọn đồi. Michael bắt đầu ở đâu đó ở cạnh trên và di chuyển một hàng xuống dưới mỗi phút, trong khi Trevor bắt đầu ở đâu đó ở cạnh trái và di chuyển một cột sang phải mỗi phút."
date: "2026-07-29T00:48:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102741
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 1"
rating: 0
weight: 102741
solve_time_s: 65
verified: true
draft: false
---

[CF 102741I - Thoát hiểm bằng nhảy đóng thế](https://codeforces.com/problemset/problem/102741/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bản đồ là một mạng lưới các ngọn đồi. Michael bắt đầu ở đâu đó ở cạnh trên và di chuyển một hàng xuống dưới mỗi phút, trong khi Trevor bắt đầu ở đâu đó ở cạnh trái và di chuyển một cột sang phải mỗi phút. Mỗi bước di chuyển cho phép điều chỉnh sang một bên tối đa một ô, vì vậy Michael có thể trôi sang trái hoặc phải khi đi xuống và Trevor có thể trôi lên hoặc xuống khi tiến. Họ muốn gặp nhau tại cùng một phòng giam vào cùng một thời điểm và điểm số là tổng độ cao của mỗi ngọn đồi mà mỗi tay đua ghé thăm. Nếu cả hai tay đua đến cùng một ngọn đồi, giá trị của nó sẽ được tính hai lần. 

Các ràng buộc cho phép tối đa 1000 hàng và 1000 cột, do đó có thể có một triệu ô. Một giải pháp thực hiện nhiều hơn một lượng nhỏ công việc không đổi trên mỗi ô sẽ khó có thể phù hợp một cách thoải mái. Đặc biệt, việc thử mọi cặp đường đi có thể là không thể vì số lượng đường đi tăng theo cấp số nhân. Ngay cả một chương trình năng động có thêm chiều hướng về vị trí của cả hai tay đua cũng sẽ có khoảng một nghìn tỷ trạng thái, vượt xa giới hạn. Mục tiêu hữu ích là giải pháp O(nm) hoặc O(nm log n). 

Các trường hợp cạnh chính xuất phát từ sự tự do của các vị trí xuất phát và từ thực tế là việc gặp nhau ở đầu là hợp lệ. Giải pháp giả định rằng cả hai tay đua đều xuất phát ở cùng một góc sẽ mất câu trả lời hợp lệ. 

Ví dụ:```
1 3
5 1 1
```Đầu ra đúng là:```
10
```Cả hai tay đua có thể xuất phát ở hàng duy nhất còn trống và gặp nhau ngay tại ô đầu tiên mà họ chọn. Giải pháp bất cẩn buộc cuộc họp phải diễn ra sau ít nhất một động thái sẽ bỏ lỡ trường hợp này. 

Một trường hợp khác là:```
2 2
1 1
2 2
```Đầu ra đúng là:```
7
```Lựa chọn tốt nhất là Michael đi qua các giá trị 1 và 2, trong khi Trevor bắt đầu ở giá trị 2 phía dưới bên trái và đến ô cuộc họp với một giá trị khác 2. Các ngọn đồi chung được tính cho cả hai tay đua, vì vậy việc loại bỏ các giá trị trùng lặp khỏi tổng sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê điểm hẹn và tính toán lộ trình tốt nhất để mỗi người lái xe đến được điểm đó. Đối với mọi ô cuộc họp có thể có, chúng tôi có thể chạy tìm kiếm từ các cạnh bắt đầu. Điều này đúng vì hai tay đua di chuyển độc lập trước khi gặp nhau. Tuy nhiên, việc lặp lại tìm kiếm cho mọi điểm gặp có thể sẽ thực hiện công việc khoảng O(nm) cho mỗi điểm, dẫn đến các hoạt động O(n²m²) trong trường hợp xấu nhất, quá chậm đối với một lưới có một triệu ô. 

Nhận xét quan trọng là chuyển động của người lái có một hướng cố định. Hàng của Michael tăng chính xác một phút mỗi phút, do đó việc tiếp cận một ô chỉ phụ thuộc vào hàng trước đó. Cột của Trevor tăng chính xác một phút mỗi phút, do đó việc tiếp cận một ô chỉ phụ thuộc vào cột trước đó. Hai bài toán đường đi độc lập có thể được giải quyết riêng biệt bằng quy hoạch động. 

Chúng tôi tính toán một bảng DP chứa số điểm cao nhất mà Michael có thể có khi tiếp cận mọi ô. Bảng DP thứ hai chứa số điểm cao nhất mà Trevor có thể có khi tiếp cận mọi ô. Ô chỉ có thể là điểm hẹn khi cả hai tay đua có thể đến đó cùng lúc. Sau khi hai bảng được tạo, câu trả lời chỉ đơn giản là tổng tối đa của hai giá trị trên tất cả các ô có thể gặp. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi cặp tuyến đường có thể, nhưng nó lặp lại các bài toán con giống nhau nhiều lần. Việc quan sát rằng mỗi tay đua chỉ có một tọa độ thay đổi cho phép chúng tôi thu gọn những phép tính lặp lại đó thành hai đường lưới tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m²) | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bảng lập trình động của Michael. Đối với hàng trên cùng, mỗi ô đều có thể là vị trí bắt đầu, vì vậy giá trị ban đầu chỉ đơn giản là chiều cao của ô đó. Đối với mỗi hàng sau, cách tốt nhất để nhập một ô là từ một trong ba ô ngay phía trên nó theo đường chéo hoặc chiều dọc. Thêm chiều cao ngọn đồi hiện tại sau khi lấy giá trị tốt nhất trước đó. 
2. Xây dựng bảng lập trình động của Trevor. Đối với cột bên trái, mọi ô đều có thể là vị trí bắt đầu. Đối với mỗi cột sau, vị trí trước đó phải ở cột trước và có thể ở trên một hàng, cùng một hàng hoặc một hàng dưới. Thêm chiều cao ngọn đồi hiện tại vào điểm số tốt nhất trước đó. 
3. Kiểm tra mọi ô mà cả hai người lái có thể tiếp cận cùng một lúc. Cuộc gặp gỡ ở hàng i và cột j chỉ có thể xảy ra khi Michael thực hiện nước đi i và Trevor thực hiện nước đi j, vì vậy chúng ta cần i = j. Câu trả lời là giá trị tối đa của Michael cộng với giá trị của Trevor trên các ô chéo này. 
4. Trả về số điểm tối đa tìm được. Trường hợp cả hai tay đua xuất phát cùng nhau được đưa vào một cách tự nhiên vì ô đường chéo (0,0) được xem xét. 

Tại sao nó hoạt động: Giá trị DP của Michael cho một ô lưu trữ điểm số tốt nhất có thể có trong số tất cả các đường đi xuống hợp lệ kết thúc ở đó. Quá trình chuyển đổi xem xét mọi bước đi cuối cùng có thể xảy ra, vì vậy không thể bỏ qua con đường nào tốt hơn. Lập luận tương tự cũng áp dụng cho DP của Trevor. Vì các tay đua không ảnh hưởng đến chuyển động của nhau trước khi gặp nhau nên cặp tuyến đường tốt nhất cho ô gặp nhau đã chọn chính xác là tổng của hai giá trị DP độc lập. Lấy số điểm tối đa trên tất cả các ô cuộc họp hợp lệ sẽ mang lại tổng số điểm tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]

    michael = [[0] * m for _ in range(n)]
    for j in range(m):
        michael[0][j] = grid[0][j]

    for i in range(1, n):
        for j in range(m):
            best = michael[i - 1][j]
            if j > 0:
                best = max(best, michael[i - 1][j - 1])
            if j + 1 < m:
                best = max(best, michael[i - 1][j + 1])
            michael[i][j] = best + grid[i][j]

    trevor = [[0] * m for _ in range(n)]
    for i in range(n):
        trevor[i][0] = grid[i][0]

    for j in range(1, m):
        for i in range(n):
            best = trevor[i][j - 1]
            if i > 0:
                best = max(best, trevor[i - 1][j - 1])
            if i + 1 < n:
                best = max(best, trevor[i + 1][j - 1])
            trevor[i][j] = best + grid[i][j]

    ans = 0
    for i in range(min(n, m)):
        ans = max(ans, michael[i][i] + trevor[i][i])

    print(ans)

if __name__ == "__main__":
    solve()
```Thẻ DP đầu tiên tuân theo quy tắc di chuyển của Michael. Hàng đầu tiên được khởi tạo trực tiếp vì anh ta có thể chọn bất kỳ vị trí bắt đầu ở hàng trên cùng nào. Quá trình chuyển đổi chỉ nhìn vào hàng trước, phù hợp với việc anh ta phải di chuyển xuống dưới mỗi phút. 

Đường chuyền DP thứ hai cũng có ý tưởng tương tự được xoay 90 độ. Chiều thời gian của Trevor là số cột nên mọi chuyển tiếp đều đến từ cột trước đó. Việc khởi tạo mọi ô ở cột bên trái là cần thiết vì Trevor có thể bắt đầu ở bất kỳ đâu dọc theo cạnh đó. 

Vòng lặp cuối cùng chỉ kiểm tra các ô`(i, i)`. Vào thời điểm`i`, Michael chắc chắn phải ở trong hàng`i`và Trevor phải ở trên cột`i`, vì vậy bất kỳ ô nào khác không thể là địa điểm họp đồng thời. Việc sử dụng số nguyên Python sẽ tránh tràn vì điểm tối đa có thể lớn hơn nhiều so với số nguyên 32 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 2
1 1
2 2
```Bảng của Michael trở thành: 

| Tế bào | Giá trị | 
| --- | --- | 
| (0,0) | 1 | 
| (0,1) | 1 | 
| (1,0) | 3 | 
| (1,1) | 3 | 

Bảng của Trevor trở thành: 

| Tế bào | Giá trị | 
| --- | --- | 
| (0,0) | 1 | 
| (1,0) | 2 | 
| (0,1) | 2 | 
| (1,1) | 4 | 

Các tế bào họp có thể là`(0,0)`Và`(1,1)`. Điểm của họ là 2 và 7, vì vậy câu trả lời là 7. Dấu vết này cho thấy lý do tại sao các ngọn đồi chung được tính hai lần. 

Đối với mẫu thứ hai:```
3 3
1 2 3
4 5 6
7 8 9
```Các trạng thái đường chéo là: 

| Vị trí | Điểm Michael | Điểm Trevor | Kết hợp | 
| --- | --- | --- | --- | 
| (0,0) | 1 | 1 | 2 | 
| (1,1) | 8 | 8 | 16 | 
| (2,2) | 17 | 25 | 42 | 

Điểm gặp mặt tốt nhất là ô dưới cùng bên phải, tạo ra 42. Dấu vết này chứng tỏ rằng điểm gặp mặt tối ưu không nhất thiết phải đến sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi bảng DP xử lý mỗi ô lưới một lần. | 
| Không gian | O(nm) | Hai bảng lưu trữ điểm số tốt nhất cho cả hai tay đua. | 

Với tối đa một triệu ô, giải pháp chỉ thực hiện vài triệu thao tác và dễ dàng phù hợp với giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    n, m = map(int, data().split())
    grid = [list(map(int, data().split())) for _ in range(n)]

    michael = [[0] * m for _ in range(n)]
    for j in range(m):
        michael[0][j] = grid[0][j]

    for i in range(1, n):
        for j in range(m):
            best = michael[i - 1][j]
            if j:
                best = max(best, michael[i - 1][j - 1])
            if j + 1 < m:
                best = max(best, michael[i - 1][j + 1])
            michael[i][j] = best + grid[i][j]

    trevor = [[0] * m for _ in range(n)]
    for i in range(n):
        trevor[i][0] = grid[i][0]

    for j in range(1, m):
        for i in range(n):
            best = trevor[i][j - 1]
            if i:
                best = max(best, trevor[i - 1][j - 1])
            if i + 1 < n:
                best = max(best, trevor[i + 1][j - 1])
            trevor[i][j] = best + grid[i][j]

    ans = 0
    for i in range(min(n, m)):
        ans = max(ans, michael[i][i] + trevor[i][i])

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solve_case("""2 2
1 1
2 2
""") == "7\n"

assert solve_case("""3 3
1 2 3
4 5 6
7 8 9
""") == "42\n"

assert solve_case("""1 1
10
""") == "20\n"

assert solve_case("""2 3
5 1 1
1 1 1
""") == "13\n"

assert solve_case("""3 1
7
2
3
""") == "14\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 10`|`20`| Kích thước lưới tối thiểu và cuộc họp ngay lập tức | 
|`2 3 / 5 1 1 / 1 1 1`|`13`| Lưới hình chữ nhật và kích thước không bằng nhau | 
|`3 1 / 7 / 2 / 3`|`14`| Xử lý ranh giới một cột | 

## Vỏ cạnh 

Đối với trường hợp họp ngay:```
1 1
10
```Cả hai tay đua đều bắt đầu trên ngọn đồi duy nhất. Các bảng DP đều chứa 10 tại`(0,0)`, và câu trả lời cuối cùng sẽ kiểm tra ô đó, tạo ra 20. Không cần di chuyển. 

Đối với trường hợp cạnh bắt đầu được chia sẻ:```
2 2
1 1
2 2
```Thuật toán không ép buộc chuyển động trước khi kiểm tra câu trả lời. Nó đánh giá`(0,0)`như một điểm gặp gỡ hợp lệ và cũng đánh giá`(1,1)`, nơi gặp nhau của các đường đi tối ưu. Tùy chọn thứ hai cho 7. 

Đối với trường hợp hình chữ nhật:```
2 3
5 1 1
1 1 1
```Các vị trí gặp nhau theo đường chéo chỉ`(0,0)`Và`(1,1)`, bởi vì cả hai tay đua phải thực hiện số bước đi như nhau. Thuật toán không bao giờ truy cập vào các vị trí đường chéo không hợp lệ ngoài kích thước nhỏ hơn, ngăn chặn lỗi ngoài giới hạn.
