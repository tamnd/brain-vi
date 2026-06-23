---
title: "CF 105327E - Bí ẩn của hộp đựng trang sức"
description: "Chúng ta có một lưới các số nguyên $N nhân N$ biểu thị một hộp trang sức hình vuông. Mỗi ô chứa một số lượng ngọc trai riêng biệt và trong cấu hình chính xác như dự kiến, các giá trị sẽ tăng dần từ trái sang phải dọc theo mỗi hàng và cũng tăng theo đúng từ trên xuống dưới…"
date: "2026-06-22T14:10:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105327
codeforces_index: "E"
codeforces_contest_name: "2024-2025 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 105327
solve_time_s: 321
verified: false
draft: false
---

[CF 105327E - Bí ẩn của hộp đựng trang sức](https://codeforces.com/problemset/problem/105327/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$N \times N$lưới các số nguyên đại diện cho một hộp trang sức hình vuông. Mỗi ô chứa một số lượng ngọc trai riêng biệt và trong cấu hình chính xác dự kiến, các giá trị sẽ tăng dần từ trái sang phải dọc theo mỗi hàng và cũng tăng đúng từ trên xuống dưới dọc theo mỗi cột. 

Lưới thực tế mà chúng tôi nhận được có thể không ở hướng ban đầu đó. Thay vào đó, nó có thể đã được xoay theo chiều kim đồng hồ 0, 90, 180 hoặc 270 độ và chúng ta đang thấy cấu hình kết quả. Nhiệm vụ của chúng ta là xác định cần bao nhiêu vòng quay ngược chiều kim đồng hồ 90 độ để đưa lưới trở lại hướng ban đầu, với giả định rằng cấu hình ban đầu thỏa mãn thuộc tính tăng theo hàng và theo cột. 

Hạn chế chính đó là$N \le 50$, giúp giữ cho lưới đủ nhỏ để chúng ta có thể thực hiện các phép tính bậc hai và thậm chí nhiều phép biến đổi đầy đủ của ma trận. Bất kỳ giải pháp nào liên tục quét hoặc chuyển đổi toàn bộ lưới với số lần không đổi vẫn dễ dàng nằm trong giới hạn. Điều này loại trừ bất cứ điều gì phức tạp hơn$O(N^2)$cho mỗi cấu hình đã kiểm tra, nhưng vẫn còn chỗ để kiểm tra rõ ràng cả bốn góc quay. 

Một điểm tinh tế là chúng ta không được cung cấp trực tiếp lưới ban đầu. Chúng tôi chỉ thấy một phiên bản xoay. Hướng ban đầu được định nghĩa ngầm là một trong bốn phép quay thỏa mãn các ràng buộc về tính đơn điệu. Do đó, đầu ra không được tính bằng cách khớp với tham chiếu bên ngoài mà bằng cách xác định phép quay nào tạo ra lưới tăng hợp lệ. 

Các trường hợp cạnh chủ yếu là về tính đối xứng và kích thước nhỏ. Ví dụ, nếu tất cả các phép quay vô tình thỏa mãn điều kiện đơn điệu do một$N$, chúng ta vẫn phải chọn số vòng quay nhỏ nhất. 

Hãy xem xét một cách đơn giản$2 \times 2$trường hợp: 

đầu vào:```
2
1 2
3 4
```Giá trị này đã tăng theo cả hai hướng, vì vậy câu trả lời đúng là 0. Một cách tiếp cận bất cẩn cho rằng đầu vào luôn bị xoay khỏi dạng hợp lệ có thể trả về một giá trị khác 0 không chính xác. 

Một trường hợp khác là khi ma trận chính xác là phiên bản xoay của ma trận hợp lệ: 

đầu vào:```
2
2 4
1 3
```Cấu hình này là phép quay 270 độ theo chiều kim đồng hồ (hoặc 90 độ ngược chiều kim đồng hồ) của ma trận được sắp xếp, vì vậy câu trả lời là 3. Một lỗi chỉ so sánh cấu trúc một phần, chẳng hạn như chỉ sắp xếp hàng mà không kiểm tra cột, sẽ phân loại sai những trường hợp như vậy. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là mô phỏng tất cả bốn hướng có thể có của lưới. Đối với mỗi hướng, chúng tôi kiểm tra xem lưới có thỏa mãn điều kiện mỗi hàng tăng dần từ trái sang phải và mọi cột tăng đúng từ trên xuống dưới hay không. Nếu chính xác một hướng là hợp lệ, chúng tôi sẽ trả về số vòng quay của nó. 

Cách tiếp cận bạo lực rất đơn giản: chúng tôi xoay ma trận một cách rõ ràng hoặc tính toán tọa độ xoay một cách tương đương một cách nhanh chóng và xác thực từng phiên bản. Mỗi xác nhận yêu cầu quét tất cả$N^2$các ô và kiểm tra mối quan hệ liền kề trong hàng và cột. Vì chúng ta thử thực hiện 4 phép quay nên tổng công là$4 \cdot O(N^2)$, trong trường hợp xấu nhất là về$4 \cdot 2500 = 10000$hoạt động. Điều này là tầm thường trong thời hạn. 

Không cần cấu trúc tổ hợp sâu hơn vì không gian bài toán chỉ có bốn trạng thái. Quan sát quan trọng là phép quay là một tập hợp hữu hạn khép kín các phép biến đổi, do đó việc kiểm tra toàn diện quỹ đạo là đủ và tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(4N^2)$|$O(N^2)$| Đã chấp nhận | 
| Tối ưu |$O(4N^2)$|$O(N^2)$| Đã chấp nhận | 

Cái gọi là giải pháp tối ưu giống hệt với giải pháp vũ phu trong cấu trúc, bởi vì ràng buộc đã giới hạn không gian trạng thái thành bốn khả năng. 

## Hướng dẫn thuật toán 

Chúng tôi coi lưới đầu vào là hướng hiện tại và kiểm tra xem cần bao nhiêu vòng quay ngược chiều kim đồng hồ để đạt được cấu hình đơn điệu hợp lệ. 

1. Đọc ma trận vào bộ nhớ. Chúng tôi giữ nguyên nó để có thể sử dụng lại làm trạng thái cơ sở cho tất cả các phép quay. Điều này tránh tích lũy các lỗi xoay từ các phép biến đổi lặp đi lặp lại. 
2. Đối với mỗi lần luân chuyển ứng viên$k = 0, 1, 2, 3$, chúng tôi giải thích ma trận sẽ trông như thế nào sau$k$các phép quay ngược chiều kim đồng hồ. Thay vì xoay ma trận một cách vật lý, chúng tôi tính toán các chỉ số theo yêu cầu bằng cách sử dụng ánh xạ từ tọa độ được quay trở lại ma trận ban đầu. 
3. Đối với mỗi$k$, chúng tôi xác minh xem lưới xoay ngụ ý có hợp lệ hay không. Chúng tôi kiểm tra từng hàng để đảm bảo mỗi phần tử hoàn toàn nhỏ hơn phần tử tiếp theo và chúng tôi kiểm tra từng cột để đảm bảo mỗi phần tử đều nhỏ hơn phần tử bên dưới nó. Nếu có bất kỳ vi phạm nào xảy ra, cấu hình không hợp lệ và chúng ta chuyển sang phần tiếp theo$k$. 
4. Đầu tiên$k$vượt qua cả kiểm tra hàng và cột sẽ được trả về dưới dạng câu trả lời. 

Lý do chúng ta có thể dừng ngay ở lần hợp lệ đầu tiên$k$là vấn đề đảm bảo cấu hình ban đầu chính xác là một trong bốn phép quay, do đó chính xác là một$k$sẽ thỏa mãn điều kiện đơn điệu. 

### Tại sao nó hoạt động 

Bốn phép quay tạo thành một phân vùng hoàn chỉnh của tất cả các hướng có thể có của hình vuông đã cho. Lưới ban đầu được biết là thỏa mãn tính đơn điệu nghiêm ngặt ở cả hai trục. Việc xoay lưới như vậy sẽ bảo toàn cấu trúc trật tự tương đối nhưng thay đổi sự liên kết của nó với các trục tọa độ. Do đó, chính xác một vòng quay sẽ căn chỉnh thứ tự nội tại với các hướng trục theo cách khôi phục tính đơn điệu theo hàng và theo cột. Vì chúng ta kiểm tra toàn bộ bốn trạng thái và chỉ chấp nhận các cấu hình đơn điệu hợp lệ nên chúng ta phải tìm ra hướng đúng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get(n, a, r, c, k):
    if k == 0:
        return a[r][c]
    if k == 1:  # 90 CCW
        return a[c][n - 1 - r]
    if k == 2:
        return a[n - 1 - r][n - 1 - c]
    # k == 3
    return a[n - 1 - c][r]

def valid(n, a, k):
    for i in range(n):
        for j in range(n - 1):
            if get(n, a, i, j, k) >= get(n, a, i, j + 1, k):
                return False
    for j in range(n):
        for i in range(n - 1):
            if get(n, a, i, j, k) >= get(n, a, i + 1, j, k):
                return False
    return True

def main():
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]
    
    for k in range(4):
        if valid(n, a, k):
            print(k)
            return

if __name__ == "__main__":
    main()
```Ý tưởng cốt lõi trong quá trình triển khai là tránh xoay ma trận về mặt vật lý. Thay vào đó, mỗi phép quay được biểu diễn dưới dạng một phép biến đổi chỉ số trong thời gian không đổi. Điều này ngăn chặn việc sao chép bộ nhớ không cần thiết và giữ cho giải pháp luôn sạch sẽ. 

Việc kiểm tra tính hợp lệ được chia thành quét hàng và quét cột. Mỗi phép so sánh sử dụng`get`hàm trừu tượng hóa logic xoay. Lỗi phổ biến nhất trong quá trình triển khai là ánh xạ chỉ mục không chính xác cho các phép quay, đặc biệt là đối với 90 và 270 độ. Ánh xạ được sử dụng ở đây tuân theo các phép biến đổi hình học tiêu chuẩn của tọa độ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
15 9 7 3
16 14 10 4
20 17 11 6
25 22 19 12
```Chúng tôi kiểm tra từng vòng quay: 

| k | Hàng/Col đơn điệu hợp lệ | Lý do | 
| --- | --- | --- | 
| 0 | Không | hàng đang giảm | 
| 1 | Có | cả hàng và cột đều tăng mạnh | 
| 2 | Không | đặt hàng nghỉ theo cả hai chiều | 
| 3 | Không | cột đơn điệu không thành công | 

Cấu hình hợp lệ đầu tiên là tại$k = 1$, vì vậy đầu ra là 1. 

Điều này cho thấy hướng ban đầu không phải là đầu vào nhất định mà là góc quay 90 độ ngược chiều kim đồng hồ. 

### Mẫu 2 

đầu vào:```
3
300 250 150
280 200 140
240 190 130
```| k | Hàng/Col đơn điệu hợp lệ | Lý do | 
| --- | --- | --- | 
| 0 | Không | hàng và cột giảm | 
| 1 | Không | thứ tự hỗn hợp, cột vẫn không hợp lệ | 
| 2 | Có | cả hai trục đều tăng nghiêm ngặt | 
| 3 | Không | vi phạm trật tự cột | 

Ở đây, hướng chính xác đạt được sau hai lần quay ngược chiều kim đồng hồ, do đó đầu ra là 2. 

Ví dụ này nhấn mạnh rằng cả hai ràng buộc hàng và cột phải được kiểm tra cùng nhau, vì cấu trúc một phần có thể được sắp xếp trong khi thứ tự 2D đầy đủ vẫn không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(4N^2)$| Mỗi vòng quay trong số bốn vòng quay được xác thực bằng cách quét tất cả các hàng và cột | 
| Không gian |$O(N^2)$| Lưu trữ ma trận đầu vào | 

Những hạn chế$N \le 50$đảm bảo rằng$N^2 = 2500$, do đó ngay cả hệ số không đổi bằng 4 vẫn không đáng kể. Giải pháp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    def get(n, a, r, c, k):
        if k == 0:
            return a[r][c]
        if k == 1:
            return a[c][n - 1 - r]
        if k == 2:
            return a[n - 1 - r][n - 1 - c]
        return a[n - 1 - c][r]

    def valid(n, a, k):
        for i in range(n):
            for j in range(n - 1):
                if get(n, a, i, j, k) >= get(n, a, i, j + 1, k):
                    return False
        for j in range(n):
            for i in range(n - 1):
                if get(n, a, i, j, k) >= get(n, a, i + 1, j, k):
                    return False
        return True

    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]

    for k in range(4):
        if valid(n, a, k):
            print(k)
            return

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""4
15 9 7 3
16 14 10 4
20 17 11 6
25 22 19 12
""") == "1"

assert run("""3
300 250 150
280 200 140
240 190 130
""") == "2"

assert run("""2
2 4
1 3
""") == "3"

# custom cases

# already correct
assert run("""2
1 2
3 4
""") == "0"

# 180 rotation case
assert run("""2
4 3
2 1
""") == "2"

# 3x3 identity increasing
assert run("""3
1 2 3
4 5 6
7 8 9
""") == "0"

# minimum size edge
assert run("""2
1 3
2 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ma trận đã được sắp xếp | 0 | độ chính xác định hướng cơ sở | 
| đảo ngược 2x2 | 2 | xử lý 180 độ | 
| tăng 3x3 | 0 | lưới đơn điệu chung | 
| trường hợp hoán đổi nhỏ | 3 | độ chính xác xoay ranh giới | 

## Vỏ cạnh 

Một mức tối thiểu$2 \times 2$lưới là nhạy cảm nhất với ánh xạ xoay không chính xác. Hãy xem xét: 

đầu vào:```
2
1 3
2 4
```Lưới này đã hợp lệ theo hướng tiêu chuẩn, vì vậy câu trả lời đúng là 0. Nếu ánh xạ chỉ số xoay cho 90 hoặc 270 độ bị tắt bởi một lần hoán đổi tọa độ, trường hợp này sẽ xuất hiện không hợp lệ trong một vòng quay khác, tạo ra câu trả lời sai. Thuật toán kiểm tra tất cả các phép quay một cách độc lập, do đó, ngay cả khi một ánh xạ sai, các ánh xạ khác sẽ không can thiệp và tính chính xác hoàn toàn phụ thuộc vào các phép biến đổi tọa độ chính xác. 

Một trường hợp cạnh khác là lưới đảo ngược hoàn hảo: 

đầu vào:```
2
4 3
2 1
```Vì$k=0$, thứ tự hàng không thành công ngay lập tức vì 4 > 3 ở hàng đầu tiên. Vì$k=1$, ánh xạ tạo ra một cấu hình không đơn điệu. Vì$k=2$, ma trận tăng dần theo cả hai hướng nên thuật toán dừng ở đó. Vì$k=3$, nó lại vi phạm tính đơn điệu. Việc kiểm tra từng bước đảm bảo rằng chỉ xoay vòng chính xác mới tồn tại được xác thực, xác nhận rằng việc từ chối sớm các trạng thái không hợp lệ sẽ không bỏ qua giải pháp chính xác.
