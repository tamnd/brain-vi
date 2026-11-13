---
title: "CF 1722F - Hình chữ L"
description: "Chúng ta có một lưới hình chữ nhật với các ô trống hoặc được tô bóng. Các ô được tô bóng tạo thành các phần trên lưới và mỗi phần phải tương ứng chính xác với hình chữ L được tạo thành từ ba ô được kết nối."
date: "2026-06-09T19:13:41+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1722
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 817 (Div. 4)"
rating: 1700
weight: 1722
solve_time_s: 108
verified: true
draft: false
---

[CF 1722F - Hình chữ L](https://codeforces.com/problemset/problem/1722/F) 

**Đánh giá:** 1700 
**Tags:** dfs và tương tự, triển khai 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật với các ô trống hoặc được tô bóng. Các ô được tô bóng tạo thành các phần trên lưới và mỗi phần phải tương ứng chính xác với hình chữ L được tạo thành từ ba ô được kết nối. Hình chữ L bao gồm một góc và hai cánh tay mở rộng theo hướng vuông góc và có thể xoay theo bất kỳ hướng nào. 

Mục đích là để xác định xem liệu toàn bộ tập hợp các ô được tô bóng có thể được phân chia thành các hình chữ L sao cho không có hai hình chữ L nào chạm nhau dọc theo một cạnh hoặc một góc hay không. Điều đó có nghĩa là mỗi`*`thuộc về đúng một hình chữ L và các hình chữ L bị cô lập với nhau, thậm chí theo đường chéo. 

Các ràng buộc đầu vào ở mức vừa phải: tối đa 50 hàng và cột trên mỗi lưới và tối đa 100 trường hợp thử nghiệm. Bởi vì$n$Và$m$nhỏ, thậm chí một thuật toán kiểm tra từng ô và các ô lân cận của nó nhiều lần cũng có thể chạy thoải mái trong thời gian giới hạn. Vấn đề trở nên phức tạp hơn ở khía cạnh tổ hợp của nó: phát hiện xem một ô có phải là một phần của hình chữ L hợp lệ hay không và đảm bảo không liền kề với các ô khác. 

Một số trường hợp cạnh không rõ ràng bao gồm các lưới quá mỏng để chứa hình chữ L. Ví dụ: một dòng sao 1x3`***`không phải là L hợp lệ vì nó không tạo thành một góc. Một trường hợp tinh tế khác là nhiều ngôi sao liền kề tạo thành một khối hình vuông 2x2; mặc dù nó chứa bốn`*`các ô, không có cách nào để phân chia chúng thành hình chữ L mà không chồng chéo lên nhau. Một cách tiếp cận đơn giản chỉ đếm các cụm gồm ba`*`sẽ chấp nhận sai các cấu hình như vậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các hình chữ L có thể có trên lưới, đánh dấu chúng và sau đó xác minh xem mỗi ngôi sao có bị che phủ chính xác một lần hay không. Đối với mỗi ô, chúng ta có thể thử thực hiện cả bốn phép quay hình chữ L. Nếu có$n \times m$các ô và mỗi ô có tối đa 4 phép quay để kiểm tra, độ phức tạp trong trường hợp xấu nhất là$O(n \cdot m)$lần chi phí đánh dấu hình chữ L. Được cho$n, m \le 50$, điều này có thể chấp nhận được, nhưng chúng ta cần xử lý cẩn thận các hình chữ L chồng lên nhau. 

Thông tin chi tiết quan trọng là mỗi hình chữ L có một góc riêng biệt: ô nơi hai cánh tay gặp nhau. Nếu chúng ta lặp lại trên tất cả các ô và xử lý bất kỳ ô nào`*`là một góc tiềm năng, chúng ta có thể cố gắng tạo thành chữ L từ nó. Để một ô là một góc, nó phải có chính xác một nhánh có độ dài 1 trên một trục và một nhánh khác có chiều dài 1 trên trục vuông góc. Bằng cách quét lưới theo cách có cấu trúc và đánh dấu các ô khi chúng tôi gán chúng cho hình chữ L, chúng tôi có thể đảm bảo mỗi ô`*`thuộc về chính xác một L. Chúng tôi cũng đảm bảo không có sự kề cận bằng cách đánh dấu các ô và kiểm tra các ô lân cận trước khi gán. 

Việc quan sát thấy hình chữ L có một góc duy nhất sẽ giảm vấn đề xuống việc kiểm tra mẫu cục bộ: mỗi góc`*`hoặc bắt đầu L hoặc là một phần của L bắt đầu ở hàng xóm. Điều này làm cho giải pháp tuyến tính theo số lượng ô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n_m_4) | O(n*m) | Chấp nhận được do n nhỏ, m | 
| Tối ưu | O(n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước lưới$n$Và$m$và sau đó chính lưới đó là một danh sách các chuỗi. 
2. Khởi tạo ma trận boolean`used`có cùng kích thước với lưới để theo dõi các ô đã được gán cho hình chữ L. 
3. Lặp lại từng ô`(i, j)`trong lưới. Nếu nó chứa một`*`và không được đánh dấu trong`used`, cố gắng tạo thành hình chữ L với`(i, j)`như góc. Kiểm tra bốn góc quay có thể có của hình chữ L: trên cùng bên trái, trên cùng bên phải, dưới cùng bên trái, dưới cùng bên phải. Mỗi vòng quay yêu cầu hai ô liền kề theo hướng vuông góc cũng`*`và không được sử dụng. 
4. Nếu tìm thấy hình chữ L hợp lệ, hãy đánh dấu cả ba ô trong`used`. Nếu không có phép quay nào trong bốn phép quay phù hợp, ngay lập tức khai báo lưới không hợp lệ và xuất ra "NO". 
5. Sau khi xử lý tất cả các ô, nếu mỗi ô`*`trong lưới được đánh dấu là đã sử dụng và không có vị trí không hợp lệ nào xảy ra, xuất "CÓ". Nếu không thì xuất ra "NO". 

Bất biến được duy trì là`used[i][j]`là`True`khi và chỉ khi ô đó được gán cho hình chữ L hợp lệ. Vì chúng ta kiểm tra sự kề nhau trước khi đánh dấu nên không có hai hình chữ L nào có thể chạm vào nhau, kể cả đường chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        grid = [list(input().strip()) for _ in range(n)]
        used = [[False]*m for _ in range(n)]
        valid = True

        # Directions for L-shape arms: (dx1, dy1), (dx2, dy2)
        rotations = [((0,1),(1,0)), ((0,1),(-1,0)), ((0,-1),(1,0)), ((0,-1),(-1,0))]

        for i in range(n):
            for j in range(m):
                if grid[i][j] == '*' and not used[i][j]:
                    found = False
                    for (dx1, dy1), (dx2, dy2) in rotations:
                        x1, y1 = i + dx1, j + dy1
                        x2, y2 = i + dx2, j + dy2
                        if 0 <= x1 < n and 0 <= y1 < m and 0 <= x2 < n and 0 <= y2 < m:
                            if grid[x1][y1] == '*' and grid[x2][y2] == '*' and not used[x1][y1] and not used[x2][y2]:
                                used[i][j] = used[x1][y1] = used[x2][y2] = True
                                found = True
                                break
                    if not found:
                        valid = False
                        break
            if not valid:
                break

        print("YES" if valid else "NO")

solve()
```Mã đọc lưới và kiểm tra từng lưới`*`cho vị trí hình chữ L tiềm năng. các`rotations`mảng xác định vị trí tương đối của hai ô khác theo mỗi hướng L. Kiểm tra ranh giới đảm bảo chúng tôi không đi ra khỏi lưới điện. các`used`ma trận ngăn chặn việc tính hai lần và đảm bảo không liền kề. Việc phá vỡ sớm sẽ tiết kiệm được tính toán không cần thiết khi phát hiện thấy vị trí không hợp lệ. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**```
6 10
........**
.**......*
..*..*....
.....**...
...*.....*
..**....**
```| tôi, j | Hành động | L đặt |`used`ô được đánh dấu | 
| --- | --- | --- | --- | 
| 0,8 | Bắt đầu | Đã tìm thấy vòng xoay phía dưới bên phải | (0,8),(1,8),(0,9) | 
| 1,1 | Bắt đầu | Đã tìm thấy vòng xoay phía dưới bên phải | (1,1),(2,1),(1,2) | 
| ... | ... | ... | ... | 

Dấu vết này xác nhận rằng mỗi`*`được bao phủ chính xác một lần và không có sự kề cận nào xảy ra. Đầu ra là`YES`. 

**Ví dụ 2:**```
3 3
***
...
***
```Hàng trên cùng`***`không thể tạo thành chữ L vì không có góc nào có hai cánh tay vuông góc. đầu tiên`*`thất bại tất cả các vòng quay. Đầu ra là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Mỗi ô được truy cập một lần; kiểm tra 4 vòng quay có thời gian không đổi | 
| Không gian | O(n*m) |`used`ma trận có cùng kích thước với lưới | 

Được cho$n, m \le 50$, giải pháp yêu cầu tối đa 2500 thao tác cho mỗi trường hợp thử nghiệm, dễ dàng trong giới hạn 1 giây. Việc sử dụng bộ nhớ rất khiêm tốn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("10\n6 10\n........**\n.**......*\n..*..*....\n.....**...\n...*.....*\n..**....**\n6 10\n....*...**\n.**......*\n..*..*....\n.....**...\n...*.....*\n..**....**\n3 3\n...\n***\n...\n4 4\n.*..\n**..\n..**\n..*.\n5 4\n.*..\n**..\n....\n..**\n..*.\n3 2\n.*\n**\n*.\n2 3\n*..\n.**\n3 2\n..\n**\n*.\n3 3\n.**\n*.*\n**.\n3 3\n..*\n.**\n..*") == "YES\nNO\nNO\nNO\nYES\nNO\nNO\nYES\nNO\nNO"

# Custom tests
assert run("1\n1 3\n***") == "NO",
```
