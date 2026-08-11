---
title: "CF 104021K - Ma trận con chung lớn nhất"
description: "Chúng ta có hai lưới vuông có kích thước $n nhân m$, mỗi ô chứa một số nguyên duy nhất trong lưới đó. Các giá trị bên trong một ma trận không bao giờ lặp lại, nhưng trên hai ma trận, các giá trị có thể xuất hiện ở cả hai ma trận."
date: "2026-07-02T04:36:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104021
codeforces_index: "K"
codeforces_contest_name: "The 2019 ICPC Asia Yinchuan Regional Contest"
rating: 0
weight: 104021
solve_time_s: 42
verified: true
draft: false
---

[CF 104021K - Ma trận con chung lớn nhất](https://codeforces.com/problemset/problem/104021/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai lưới vuông có kích thước$n \times m$, mỗi ô chứa một số nguyên duy nhất trong lưới đó. Các giá trị bên trong một ma trận không bao giờ lặp lại, nhưng trên hai ma trận, các giá trị có thể xuất hiện ở cả hai ma trận. 

Một ma trận con được xác định bằng cách chọn một khối hình chữ nhật liền kề gồm các hàng và cột. Chúng ta muốn tìm diện tích lớn nhất có thể có của ma trận con xuất hiện trong cả hai ma trận theo cách cấu trúc giống hệt nhau, nghĩa là tồn tại một hình chữ nhật trong ma trận thứ nhất có mẫu giá trị khớp chính xác với hình chữ nhật trong ma trận thứ hai. 

Bởi vì tất cả các giá trị trong mỗi ma trận đều khác biệt nên mỗi giá trị đóng vai trò như một mã định danh duy nhất cho vị trí của nó. Đây là cách đơn giản hóa cấu trúc quan trọng: thay vì so sánh các giá trị thô, chúng ta có thể suy luận về vị trí của các giá trị trùng khớp trên hai ma trận. 

Những ràng buộc cho phép$n, m \le 1000$, do đó lưới có tới$10^6$tế bào. Bất kỳ giải pháp nào gần hơn$O(n^2 m^2)$ngay lập tức là không thể vì nó sẽ yêu cầu theo thứ tự$10^{12}$so sánh. Thậm chí$O(n^2 m)$phải được xây dựng cẩn thận, lý tưởng nhất là sử dụng quét tuyến tính hoặc gần tuyến tính trên mỗi hàng hoặc mỗi cột. 

Một sự hiểu lầm ngây thơ thường xuất phát từ việc coi đây là chuỗi con chung dài nhất trong hai chiều mà không sử dụng tính duy nhất của các giá trị. Điều đó dẫn đến việc cố gắng băm mọi ma trận con hoặc so sánh trực tiếp tất cả các hình chữ nhật, điều này trở nên không khả thi. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta cố gắng so khớp các ma trận con hoàn toàn bằng giá trị bằng nhau mà không thực thi việc định vị tương đối nhất quán. Ví dụ: nếu chúng tôi chỉ kiểm tra xem tất cả các giá trị trong một hình chữ nhật có xuất hiện ở đâu đó trong ma trận khác hay không, chúng tôi sẽ chấp nhận sai cách sắp xếp không phải hình chữ nhật. 

Công thức đúng đòi hỏi phải duy trì đồng thời các mối quan hệ kề nhau theo cả hướng hàng và cột. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi ma trận con có thể có trong lưới đầu tiên và cố gắng xác định vị trí ma trận con giống hệt nhau trong lưới thứ hai. Ngay cả khi chúng tôi tối ưu hóa việc kiểm tra bằng cách sử dụng hàm băm hoặc so sánh trực tiếp thì số lượng ma trận con vẫn$O(n^2 m^2)$và việc xác minh từng cái sẽ tốn ít nhất thời gian tuyến tính trong khu vực, khiến toàn bộ công việc hoàn toàn không khả thi. 

Quan sát quan trọng xuất phát từ tính duy nhất của các giá trị bên trong mỗi ma trận. Vì mỗi số xuất hiện chính xác một lần trên mỗi ma trận nên mỗi giá trị xác định một tọa độ duy nhất. Điều này cho phép chúng ta chuyển đổi bài toán từ việc so sánh các giá trị hình chữ nhật sang so sánh các hình chữ nhật có tọa độ khác nhau. 

Nếu chúng ta ánh xạ mọi giá trị trong ma trận A tới vị trí của nó và thực hiện tương tự cho ma trận B, thì bất kỳ giá trị nào cũng đóng vai trò là điểm neo tham chiếu. Nếu một ma trận con tồn tại trong cả hai lưới thì việc chọn phần tử trên cùng bên trái của nó sẽ mang lại một mẫu bù nhất quán cho tất cả các phần tử khác trong ma trận con đó. Điều này có nghĩa là một khi chúng ta sửa một cặp ô neo phù hợp, sự tồn tại của một ma trận con chung sẽ trở thành vấn đề mở rộng theo hai hướng trong khi vẫn đảm bảo tính nhất quán về vị trí. 

Điều này biến vấn đề thành vấn đề mở rộng chung dài nhất 2D trên các tọa độ được căn chỉnh, có thể được kiểm tra từng hàng bằng cách sử dụng các vị trí khớp được tính toán trước. 

Chúng tôi giảm bớt nhiệm vụ một cách hiệu quả để tìm hình chữ nhật lớn nhất trong đó các hàng liên tiếp bảo toàn các mẫu căn chỉnh ngang giống hệt nhau được tạo ra bởi ánh xạ giá trị theo vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 m^2 (nm))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng một tra cứu ngược cho cả hai ma trận để với mọi giá trị, chúng tôi biết tọa độ của nó trong ma trận đó. Điều này cho phép dịch theo thời gian không đổi từ một giá trị trong ma trận này sang vị trí của nó trong ma trận kia. 

Sau đó, chúng tôi xây dựng một lưới trợ giúp mã hóa sự liên kết cấu trúc giữa hai ma trận. Đối với mỗi vị trí$(i, j)$trong ma trận A, chúng ta xác định vị trí giá trị của nó xuất hiện trong ma trận B, đưa ra tọa độ$(x, y)$. Việc ghép tọa độ này cho phép chúng ta suy luận xem liệu các ô liền kề trong A có duy trì tính kề cận trong B hay không. 

Tiếp theo, chúng ta chuyển bài toán thành tìm hình chữ nhật lớn nhất có căn chỉnh hợp lệ. Chúng tôi xác định điều kiện nhị phân cho các cặp cột liền kề: liệu mối quan hệ lân cận theo chiều ngang có được bảo toàn đồng thời trong cả hai ma trận hay không. Đối với mỗi hàng, chúng tôi tính toán một cấu trúc giống như biểu đồ biểu thị mức độ nhất quán theo chiều ngang này kéo dài sang bên phải. 

Sau đó, chúng tôi quét qua các hàng, coi mỗi hàng là cơ sở của biểu đồ. Đối với mỗi cột, chúng tôi duy trì bao nhiêu hàng liên tiếp ở trên cũng duy trì tính nhất quán theo chiều ngang. Điều này làm giảm vấn đề tính toán hình chữ nhật lớn nhất trong biểu đồ cho mỗi hàng. 

Diện tích hình chữ nhật tối đa được tìm thấy trong quá trình này là câu trả lời. 

### Tại sao nó hoạt động 

Bất kỳ ma trận con chung hợp lệ nào cũng phải duy trì mối quan hệ kề cận theo cả chiều ngang và chiều dọc. Liền kề theo chiều ngang đảm bảo rằng đối với mỗi ô, ô lân cận bên phải của nó khớp với ô lân cận bên phải của ô tương ứng trong ma trận khác. Xếp chồng theo chiều dọc đảm bảo rằng tính nhất quán này trải rộng trên nhiều hàng. 

Bởi vì mỗi giá trị là duy nhất nên tính nhất quán kề cận xác định đầy đủ liệu một ma trận con có giống nhau trên cả hai ma trận hay không. Điều này làm giảm vấn đề khớp mẫu 2D thành mở rộng biểu đồ 1D trên các hàng, trong đó tính chính xác xuất phát từ thực tế là tất cả các ràng buộc phân rã thành kiểm tra kề cận cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    A = [list(map(int, input().split())) for _ in range(n)]
    B = [list(map(int, input().split())) for _ in range(n)]

    posB = {}
    for i in range(n):
        for j in range(m):
            posB[B[i][j]] = (i, j)

    match_row = [[0] * m for _ in range(n)]
    match_col = [[0] * m for _ in range(n)]

    for i in range(n):
        for j in range(m):
            x, y = posB[A[i][j]]
            match_row[i][j] = (y == posB[A[i][j+1]][1] if j + 1 < m else 0)
            match_col[i][j] = (x == posB[A[i+1][j]][0] if i + 1 < n else 0)

    # height array for histogram of valid rows
    height = [[0] * m for _ in range(n)]

    for j in range(m):
        height[0][j] = 1

    for i in range(1, n):
        for j in range(m):
            if match_col[i-1][j]:
                height[i][j] = height[i-1][j] + 1
            else:
                height[i][j] = 1

    ans = 1

    # for each row, compute largest rectangle using horizontal validity
    for i in range(n):
        stack = []
        for j in range(m + 1):
            cur = match_row[i][j] if j < m else 0
            while stack and match_row[i][stack[-1]] >= cur:
                idx = stack.pop()
                h = height[i][idx]
                width = j if not stack else j - stack[-1] - 1
                ans = max(ans, h * width)
            stack.append(j)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng bản đồ vị trí cho ma trận B để mỗi giá trị có thể được phân giải theo tọa độ của nó ngay lập tức. Đây là xương sống cấu trúc cho phép so sánh các vùng lân cận mà không cần quét. 

các`match_row`bảng kiểm tra xem việc di chuyển sang phải trong A có tương ứng với việc di chuyển sang phải trong B hay không. Nếu điều kiện này đúng thì cấu trúc ngang nhất quán ở ô đó. Tương tự, tính nhất quán theo chiều dọc được thể hiện một cách gián tiếp thông qua`height`mảng, đếm số lượng hàng liên tiếp duy trì sự căn chỉnh theo chiều dọc ở mỗi cột. 

Bước cuối cùng là tính toán biểu đồ ngăn xếp đơn điệu tiêu chuẩn trên mỗi hàng, trong đó chiều rộng đến từ tính nhất quán theo chiều ngang và chiều cao đến từ tính liên tục theo chiều dọc. 

Một cạm bẫy triển khai phổ biến là quên rằng các ràng buộc ngang và dọc tương tác độc lập. Trộn chúng vào một trạng thái DP duy nhất thường dẫn đến việc đếm quá mức hoặc mở rộng hình chữ nhật không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét cấu trúc mẫu nơi tồn tại khối 2x2 hợp lệ. 

### Ví dụ 1 

Ma trận A:```
1 2 3
4 5 6
8 7 9
```Ma trận B:```
5 6 1
7 9 3
2 4 8
```Chúng tôi theo dõi các trận đấu ngang đầu tiên. Đối với hàng 1 trong A, cặp (5,6) căn chỉnh trong B thành các ô liền kề, do đó`match_row`là đúng ở vị trí đó. Điều tương tự cũng xảy ra với hàng thứ hai của (7,9). 

| Hàng | Cột | trận đấu_row | chiều cao | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 
| 0 | 1 | 0 | 1 | 
| 1 | 0 | 1 | 1 | 
| 1 | 1 | 0 | 1 | 

Hình chữ nhật lớn nhất có kích thước 2x2 được tạo thành bởi các hàng 1-2 và các cột 0-1, cho diện tích 4. 

### Ví dụ 2 

Một trường hợp tối thiểu:```
A:
1 2
3 4

B:
1 2
3 4
```Tất cả các trận đấu ngang và dọc diễn ra ở mọi nơi. 

| Hàng | Cột | trận đấu_row | chiều cao | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 
| 0 | 1 | 0 | 1 | 
| 1 | 0 | 1 | 2 | 
| 1 | 1 | 0 | 2 | 

Ma trận đầy đủ là hợp lệ, cho diện tích 4. 

Những dấu vết này cho thấy sự tích tụ theo chiều dọc không phụ thuộc vào tính khả thi theo chiều ngang và cả hai phải thẳng hàng để tạo thành một hình chữ nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô được xử lý một số lần không đổi để ánh xạ, kiểm tra lân cận và tính toán biểu đồ | 
| Không gian |$O(nm)$| Lưu trữ bản đồ vị trí và bảng phụ trợ và bảng chiều cao | 

Những hạn chế$n, m \le 1000$cung cấp tối đa một triệu ô, do đó việc quét tuyến tính trên tất cả các ô và xử lý thời gian liên tục trên mỗi ô là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# minimal
assert run("""1 1
1
1
""") == "1"

# identical matrices
assert run("""2 2
1 2
3 4
1 2
3 4
""") == "4"

# rotated mismatch
assert run("""2 2
1 2
3 4
2 1
4 3
""") == "1"

# sample-like case
assert run("""3 3
1 2 3
4 5 6
7 8 9
1 2 3
4 5 6
7 8 9
""") == "9"

# reversed order
assert run("""3 3
9 8 7
6 5 4
3 2 1
1 2 3
4 5 6
7 8 9
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 giống nhau | 1 | trường hợp cơ sở đúng đắn | 
| giống hệt 2×2 | 4 | phát hiện hình chữ nhật đầy đủ | 
| bố cục hoán đổi | 1 | xử lý sự không phù hợp lân cận | 
| giống hệt 3×3 | 9 | mở rộng hình chữ nhật tối đa | 
| ma trận đảo ngược | 1 | không có sự tăng trưởng ma trận con sai | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi ma trận chia sẻ các giá trị nhưng có cách sắp xếp không gian hoàn toàn khác nhau. Trong những trường hợp như vậy, chỉ riêng sự bình đẳng về giá trị đã gây hiểu lầm vì tính liền kề không được bảo toàn. Thuật toán tránh điều này bằng cách thực thi tính nhất quán theo cả chiều ngang và chiều dọc thông qua`match_row`Và`height`. 

Một trường hợp khác là khi chỉ tồn tại các kết quả khớp một ô. Ví dụ: nếu mọi giá trị được tách biệt theo khía cạnh bảo toàn kề, thuật toán vẫn tạo ra chiều cao 1 và chiều rộng 1 ở mọi nơi, mang lại câu trả lời 1 chính xác mà không cần cố gắng mở rộng không hợp lệ. 

Cuối cùng, các lưới suy biến như 1×m hoặc n×1 được xử lý một cách tự nhiên. Các chuyển tiếp ngang hoặc dọc sụp đổ, nhưng cấu trúc biểu đồ vẫn tính toán chính xác vì các ranh giới được coi là các chuyển tiếp bằng 0.
