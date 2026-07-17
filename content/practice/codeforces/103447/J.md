---
title: "CF 103447J - Tối thiểu cục bộ"
description: "Chúng ta được cho một lưới số hình chữ nhật. Đối với mỗi ô, chúng tôi xem xét tất cả các giá trị nằm trong cùng một hàng hoặc trong cùng một cột với ô đó, bao gồm cả chính ô đó. Trong số tất cả các giá trị đó, chúng tôi kiểm tra xem giá trị của ô hiện tại có nhỏ nhất hay không."
date: "2026-07-03T07:32:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103447
codeforces_index: "J"
codeforces_contest_name: "The 2021 China Collegiate Programming Contest (Harbin)"
rating: 0
weight: 103447
solve_time_s: 40
verified: true
draft: false
---

[CF 103447J - Mức tối thiểu cục bộ](https://codeforces.com/problemset/problem/103447/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới số hình chữ nhật. Đối với mỗi ô, chúng tôi xem xét tất cả các giá trị nằm trong cùng một hàng hoặc trong cùng một cột với ô đó, bao gồm cả chính ô đó. Trong số tất cả các giá trị đó, chúng tôi kiểm tra xem giá trị của ô hiện tại có nhỏ nhất hay không. Nhiệm vụ là đếm xem có bao nhiêu ô thỏa mãn điều kiện này. 

Một cách khác để nghĩ về điều này là mỗi ô đều xác định một “chữ thập” được tạo thành từ hàng và cột của nó. Chúng tôi lấy giá trị tối thiểu bên trong chữ thập đó và chúng tôi muốn đếm xem có bao nhiêu ô bằng giá trị tối thiểu đó. 

Kích thước lưới có thể lớn tới 1000 x 1000, cung cấp tới một triệu ô. Bất kỳ giải pháp nào kiểm tra từng ô bằng cách quét trực tiếp toàn bộ hàng và cột của nó sẽ có khả năng thực hiện khoảng 2n hoặc 2m công việc cho mỗi ô, dẫn đến khoảng 10^12 thao tác trong trường hợp xấu nhất. Điều đó vượt xa những gì phù hợp với giới hạn một giây. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị tối thiểu lặp lại bên trong một hàng hoặc cột. Ví dụ, nếu một hàng là`1 2 1`, cả hai ô chứa`1`là những ứng cử viên xét từ góc độ hàng, nhưng một trong số chúng có thể thất bại khi xem xét cột của nó. 

Một tình huống phức tạp khác là khi mức tối thiểu của hàng và mức tối thiểu của cột không đồng nhất. Một ô có thể là giá trị tối thiểu trong hàng của nó nhưng không phải là giá trị tối thiểu trong cột của nó hoặc ngược lại, vì vậy chúng ta phải luôn xem xét hợp nhất cả hai ràng buộc một cách chính xác. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là đơn giản. Đối với mỗi ô`(i, j)`, chúng tôi quét toàn bộ hàng`i`để tìm giá trị nhỏ nhất và chúng tôi cũng quét toàn bộ cột`j`để tìm giá trị nhỏ nhất. Sau đó, chúng tôi so sánh giá trị ô với giá trị tối thiểu của hai kết quả này. Nếu trùng khớp thì chúng tôi tính. 

Điều này đúng vì mức tối thiểu trên phần kết hợp của hàng và cột chỉ đơn giản là mức tối thiểu của mức tối thiểu của hàng và mức tối thiểu của cột. Tuy nhiên, việc tính toán lại các giá trị cực tiểu này cho mỗi ô là lãng phí. Mỗi lần quét hàng tốn O(m), mỗi lần quét cột tốn O(n) và thực hiện điều này cho tất cả n·m ô sẽ dẫn đến O(n·m·(n+m)), trong trường hợp xấu nhất là khoảng 2×10^9 đến 2×10^12 thao tác tùy thuộc vào hằng số, quá chậm. 

Quan sát quan trọng là cực tiểu của hàng và cực tiểu của cột không phụ thuộc vào ô truy vấn. Chúng chỉ phụ thuộc vào cấu trúc lưới. Vì vậy, chúng ta có thể tính toán trước giá trị tối thiểu cho mỗi hàng và mỗi cột một lần. Sau đó, mỗi ô có thể được kiểm tra theo thời gian không đổi bằng cách so sánh nó với giá trị nhỏ hơn của giá trị tối thiểu hàng và giá trị tối thiểu cột của nó. 

Điều này làm giảm vấn đề xuống còn một lần truyền qua lưới sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m·(n+m)) | O(1) | Quá chậm | 
| Tối ưu | O(n·m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán hai mảng phụ trợ, một mảng lưu trữ giá trị tối thiểu của mỗi hàng và một mảng lưu trữ giá trị tối thiểu của mỗi cột. 

1. Khởi tạo một mảng`rowMin`kích thước`n`, chứa đầy những giá trị rất lớn. Điều này sẽ lưu trữ giá trị nhỏ nhất được thấy trong mỗi hàng. 
2. Khởi tạo một mảng`colMin`kích thước`m`, cũng chứa đầy những giá trị rất lớn. Điều này sẽ lưu trữ giá trị nhỏ nhất được thấy trong mỗi cột. 
3. Quét toàn bộ ma trận một lần. Đối với mỗi ô`(i, j)`, cập nhật`rowMin[i] = min(rowMin[i], a[i][j])`Và`colMin[j] = min(colMin[j], a[i][j])`. Bước này xây dựng giá trị cực tiểu chính xác cho mỗi hàng và cột theo thời gian tuyến tính trên tất cả các ô. 
4. Sau khi tiền xử lý, quét lại ma trận. Đối với mỗi ô`(i, j)`, tính toán`t = min(rowMin[i], colMin[j])`. Nếu như`a[i][j] == t`, tăng câu trả lời. 
5. Xuất số đếm cuối cùng. 

Lý do chúng tôi lấy`min(rowMin[i], colMin[j])`đó là sự kết hợp của các giá trị trong hàng`i`và cột`j`có giá trị nhỏ nhất trong hàng hoặc trong cột. Không có nguồn nào khác cho giá trị nhỏ hơn, vì mọi phần tử đều đã được tính vào một trong hai bộ này. 

### Tại sao nó hoạt động 

Đối với bất kỳ ô nào`(i, j)`, mọi giá trị được xét trong chữ thập của nó đều thuộc về hàng`i`hoặc cột`j`. Mức tối thiểu trên liên kết đó phải là phần tử nhỏ nhất trong hàng`i`hoặc phần tử nhỏ nhất trong cột`j`, tùy theo giá trị nào nhỏ hơn. Bước tiền xử lý đảm bảo các giá trị này là chính xác trên toàn cầu. Vì vậy, so sánh`a[i][j]`với`min(rowMin[i], colMin[j])`kiểm tra chính xác xem ô có đạt được mức tối thiểu trong đường chéo của nó hay không và không có ô nào có thể được phân loại sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    
    INF = 10**18
    rowMin = [INF] * n
    colMin = [INF] * m

    for i in range(n):
        for j in range(m):
            v = a[i][j]
            if v < rowMin[i]:
                rowMin[i] = v
            if v < colMin[j]:
                colMin[j] = v

    ans = 0
    for i in range(n):
        for j in range(m):
            if a[i][j] == min(rowMin[i], colMin[j]):
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc lưới vào bộ nhớ, điều này là cần thiết vì chúng ta cần hai lần truyền dữ liệu. Bước đầu tiên xây dựng các cực tiểu hàng và cột một cách độc lập. Điều này tránh việc quét lặp lại sau này. 

Lần thứ hai thực hiện kiểm tra tình trạng thực tế. biểu thức`min(rowMin[i], colMin[j])`được tính theo O(1), do đó mỗi ô đóng góp một công không đổi. Đây là bước thay thế việc tìm kiếm brute-force tốn kém. 

Một lỗi triển khai phổ biến là cố gắng tính lại giá trị cực tiểu của hàng hoặc cột bên trong vòng lặp thứ hai. Điều đó sẽ âm thầm giới thiệu lại hệ số n hoặc m và phá vỡ hiệu suất. 

## Ví dụ đã hoạt động 

Hãy xem xét ma trận mẫu:```
3 3
1 5 9
4 3 7
2 6 2
```Đầu tiên chúng ta tính cực tiểu của hàng và cột. 

| Bước | Hàng/Col | Chỉ mục | Giá trị | Trạng thái cập nhật | 
| --- | --- | --- | --- | --- | 
| quét | hàngMin | 0 | 1 | [1, inf, inf] | 
| quét | hàngMin | 1 | 3 | [1, 3, inf] | 
| quét | hàngMin | 2 | 2 | [1, 3, 2] | 
| quét | colMin | 0 | 1 | [1, inf, inf] | 
| quét | colMin | 1 | 5 | [1, 3, inf] | 
| quét | colMin | 2 | 9 | [1, 3, 2] | 

Bây giờ đánh giá từng ô: 

| Tế bào | Giá trị | min(rowMin[i], colMin[j]) | Cuộc thi đấu? | 
| --- | --- | --- | --- | 
| (0,0) | 1 | 1 | vâng | 
| (0,1) | 5 | 3 | không | 
| (0,2) | 9 | 2 | không | 
| (1,0) | 4 | 1 | không | 
| (1,1) | 3 | 3 | vâng | 
| (1,2) | 7 | 2 | không | 
| (2,0) | 2 | 1 | không | 
| (2,1) | 6 | 3 | không | 
| (2,2) | 2 | 2 | vâng | 

Câu trả lời là 3. 

Dấu vết này cho thấy các ràng buộc hàng và cột tương tác độc lập như thế nào. Mỗi ô đủ điều kiện khớp chính xác với giá trị tốt nhất có thể đạt được trong dấu thập của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Mỗi ô được xử lý một số lần không đổi trong hai lần | 
| Không gian | O(n+m) | Lưu trữ cực tiểu hàng và cực tiểu cột | 

Kích thước lưới tối đa là một triệu ô, do đó, việc quét tuyến tính trên tất cả các mục được thực hiện thoải mái trong giới hạn thời gian. Dung lượng bộ nhớ bị chi phối bởi việc lưu trữ ma trận và hai mảng phụ, nằm dưới giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    
    INF = 10**18
    rowMin = [INF] * n
    colMin = [INF] * m

    for i in range(n):
        for j in range(m):
            v = a[i][j]
            rowMin[i] = min(rowMin[i], v)
            colMin[j] = min(colMin[j], v)

    ans = 0
    for i in range(n):
        for j in range(m):
            if a[i][j] == min(rowMin[i], colMin[j]):
                ans += 1

    return str(ans)

# provided sample
assert run("""3 3
1 5 9
4 3 7
2 6 2
""") == "3"

# minimum size
assert run("""1 1
5
""") == "1"

# all equal
assert run("""2 3
7 7 7
7 7 7
""") == "6"

# row dominance
assert run("""2 2
1 100
100 100
""") == "2"

# column dominance
assert run("""2 2
1 2
100 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 1 | độ đúng cơ sở | 
| tất cả ma trận bằng nhau | 6 | mọi ô đều đủ điều kiện | 
| ma trận nặng hàng | 2 | hàng tối thiểu chiếm ưu thế | 
| ma trận nặng cột | 2 | cột cực tiểu chiếm ưu thế | 

## Vỏ cạnh 

Lưới 1×1 là ranh giới đơn giản nhất. Hàng và cột là cùng một phần tử, vì vậy cả hai cực tiểu đều có giá trị bằng nhau và phải tính từng ô riêng lẻ. Thuật toán khởi tạo`rowMin[0]`Và`colMin[0]`đến giá trị đó và bước kiểm tra cuối cùng sẽ được thực hiện ngay lập tức. 

Trong một ma trận thống nhất trong đó mọi mục nhập đều giống hệt nhau, mọi giá trị tối thiểu của hàng và giá trị tối thiểu của cột đều có cùng giá trị. Đối với bất kỳ ô nào`(i, j)`,`min(rowMin[i], colMin[j])`bằng giá trị ô, vì vậy tất cả các mục đều được tính. Lượt thứ hai tăng chính xác cho mọi vị trí mà không có ngoại lệ. 

Trong trường hợp một hàng chứa một giá trị rất nhỏ trong khi các cột thì không, giá trị tối thiểu của hàng sẽ chiếm ưu thế trong hầu hết các phép so sánh. Thuật toán vẫn hoạt động vì nó luôn tính toán lại giá trị cực tiểu của hàng và cột toàn cục chính xác trước khi đánh giá, đảm bảo không phụ thuộc vào cấu trúc cục bộ hoặc thứ tự truyền tải.
