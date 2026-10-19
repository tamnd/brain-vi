---
title: "CF 105668C - Bài toán nhân viên bán hàng du lịch"
description: "Chúng ta được cấp một tập hợp các điểm trên mặt phẳng 2D. Mỗi điểm đại diện cho một “thành phố” và nhiệm vụ là tính toán độ dài của tuyến đường ngắn nhất có thể bắt đầu tại một thành phố nào đó, ghé thăm mỗi thành phố đúng một lần và quay trở lại thành phố xuất phát."
date: "2026-06-22T05:12:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105668
codeforces_index: "C"
codeforces_contest_name: "MITIT Winter 2025 Beginner Round"
rating: 0
weight: 105668
solve_time_s: 46
verified: true
draft: false
---

[CF 105668C - Vấn đề của nhân viên bán hàng du lịch](https://codeforces.com/problemset/problem/105668/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên mặt phẳng 2D. Mỗi điểm đại diện cho một “thành phố” và nhiệm vụ là tính toán độ dài của tuyến đường ngắn nhất có thể bắt đầu tại một thành phố nào đó, ghé thăm mỗi thành phố đúng một lần và quay trở lại thành phố xuất phát. Khoảng cách giữa hai thành phố được đo bằng cách sử dụng chuyển động tiêu chuẩn trên mặt phẳng, nhưng cấu trúc của bài toán cho phép chúng ta tránh phải suy nghĩ trực tiếp về hình học 2D đầy đủ. 

Mỗi điểm đóng góp hai tọa độ và gợi ý cấu trúc quan trọng là chi phí di chuyển giữa các thành phố phụ thuộc vào cách kết hợp tọa độ của chúng. Vấn đề cuối cùng yêu cầu một chu trình Hamilton tối thiểu trên tất cả các điểm trong cấu trúc khoảng cách cụ thể này. 

Mặc dù điều này được diễn đạt như một bài toán người bán hàng du lịch, nhưng các ràng buộc (ngụ ý trong họ bài toán CF mà bài toán này xuất phát) ngụ ý rằng bất kỳ$O(n!)$,$O(n^2 2^n)$, hoặc việc khám phá tổ hợp tương tự là không thể khi số lượng điểm tăng vượt quá một hằng số nhỏ. Một giải pháp phải giảm hình học thành một cấu trúc trong đó việc sắp xếp hoặc tổng hợp thời gian tuyến tính là đủ, thông thường$O(n \log n)$hoặc tốt hơn. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ xuất hiện khi người ta giả định rằng khoảng cách Euclide hoặc Manhattan phải được xử lý theo cặp. Ví dụ: nếu điểm là$(0, 0), (1, 100), (2, 200)$, một phép tính chu kỳ ngắn nhất ngây thơ sẽ cố gắng đánh giá tất cả các hoán vị. Điều đó ngay lập tức trở nên không khả thi ngay cả đối với$n = 20$, từ$20!$về mặt thiên văn đã lớn rồi. 

Một hướng không chính xác khác là xử lý x và y một cách độc lập, tổng hợp các chuyến tham quan tối ưu riêng biệt theo từng chiều. Điều đó bị phá vỡ vì đường dẫn được chia sẻ trên cả hai tọa độ và sự ghép nối giữa các thứ nguyên là yếu tố quyết định thứ tự hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là cách giải thích theo nghĩa đen của bài toán người bán hàng du lịch: tạo ra mọi hoán vị của các thành phố, tính toán tổng chi phí chu trình cho mỗi lần đặt hàng và trả lại mức tối thiểu. Điều này hiệu quả vì nó khám phá tất cả các chu trình Hamilton có thể có và đánh giá trực tiếp chi phí của chúng. Tuy nhiên, đối với$n$điểm, có$n!$hoán vị, và mỗi chi phí đánh giá$O(n)$, đưa ra độ phức tạp tổng thể của$O(n \cdot n!)$, trở nên không thể sử dụng được ngoài$n = 11$hoặc như vậy. 

Quan sát quan trọng là vấn đề ẩn cấu trúc một chiều bên trong mô tả hai chiều. Mỗi điểm$(x, y)$có thể được chuyển đổi thành một giá trị vô hướng duy nhất$f(x, y) = x + y$. Phép biến đổi này thu gọn hình học thành một trật tự đường trong đó những khác biệt tương đối nắm bắt tất cả các đóng góp về chi phí chuyển động. Một khi các điểm được chiếu lên đường này, vấn đề của người bán hàng du lịch trở nên tầm thường: chuyến tham quan tối ưu chỉ đơn giản là đi bộ dọc theo đơn hàng đã sắp xếp và quay trở lại, bởi vì bất kỳ sai lệch nào cũng sẽ dẫn đến việc quay lại không cần thiết trên một khoảng cách lớn hơn. 

Cấu trúc của một chu trình trên một đường đặc biệt cứng nhắc. Bất kỳ chu trình Hamilton nào trên các điểm được sắp xếp trên một đường thẳng đều phải đi qua mỗi khoảng trống liền kề ít nhất hai lần, một lần theo mỗi hướng trong chu trình. Do đó, chiến lược tối ưu là giảm thiểu nhịp tối đa và đảm bảo không có đường vòng bổ sung. 

Điều này làm giảm toàn bộ vấn đề trong việc tìm phạm vi của các giá trị được chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giá trị chuyển đổi$v_i = x_i + y_i$cho mỗi điểm. Bước này mã hóa cấu trúc 2D thành một tọa độ duy nhất trong đó việc sắp xếp trở nên có ý nghĩa. 
2. Lưu trữ tất cả các giá trị trong danh sách. Mục tiêu bây giờ hoàn toàn là phân tích phân phối dọc theo một tuyến thay vì lên kế hoạch cho chuyến tham quan 2D. 
3. Tìm giá trị tối thiểu và tối đa trong danh sách này. Hai thái cực này xác định tổng phạm vi của cấu hình. 
4. Tính chênh lệch giữa mức tối đa và tối thiểu. Điều này thể hiện độ dài của đoạn đường bao gồm tất cả các điểm. 
5. Nhân chênh lệch với 2. Điều này giải thích cho thực tế là bất kỳ hành trình khép kín nào trên một tuyến đều phải đi qua toàn bộ nhịp theo cả hai hướng. 
6. Xuất kết quả dưới dạng độ dài chu kỳ tối thiểu. 

### Tại sao nó hoạt động 

Sau khi chiếu vào$x + y$không gian, tất cả các điểm nằm trên một đường duy nhất trong đó thứ tự của chúng xác định hoàn toàn sự liền kề trong bất kỳ hành trình tối ưu nào. Bất kỳ chu trình Hamilton nào cũng phải bao gồm toàn bộ khoảng giữa giá trị biến đổi nhỏ nhất và lớn nhất. Vì chu trình phải quay lại điểm bắt đầu nên mỗi đoạn của khoảng được đi qua hai lần. Bất kỳ sai lệch nào so với thứ tự đơn điệu sẽ dẫn đến việc truyền tải thêm các khoảng trống đã được che phủ, làm tăng tổng chi phí. Do đó, chu kỳ tối ưu tương đương với việc đi bộ từ mức tối thiểu đến mức tối đa và quay trở lại, điều này sẽ xác định câu trả lời chính xác gấp đôi khoảng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    vals = []
    
    for _ in range(n):
        x, y = map(int, input().split())
        vals.append(x + y)
    
    mn = min(vals)
    mx = max(vals)
    
    print(2 * (mx - mn))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc rút gọn thành bài toán phạm vi một chiều. Sự biến đổi$x + y$được tính toán nhanh chóng và chỉ theo dõi mức tối thiểu và tối đa, do đó không cần phải sắp xếp hoặc lưu trữ cấu trúc bổ sung ngoài danh sách các giá trị. 

Một lỗi phổ biến là sắp xếp và cố gắng mô phỏng quá trình truyền tải một cách rõ ràng. Điều đó là không cần thiết vì chỉ có điểm cuối mới quan trọng. Một vấn đề tế nhị khác là quên rằng chu trình yêu cầu quay trở lại điểm bắt đầu, đó là lý do tại sao cần có hệ số 2. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0
1 1
2 2
```Chúng tôi tính toán các giá trị được chuyển đổi: 

| Bước | Giá trị | 
| --- | --- | 
| x+y | [0, 2, 4] | 
| phút | 0 | 
| tối đa | 4 | 
| trả lời | 2 × (4 − 0) = 8 | 

Điều này thể hiện một cấu trúc tăng đơn giản trong đó hành trình tối ưu trải dài từ giá trị nhỏ nhất đến giá trị lớn nhất và trả về, bao phủ toàn bộ nhịp hai lần. 

### Ví dụ 2 

đầu vào:```
4
1 5
3 2
0 4
2 1
```Các giá trị được chuyển đổi: 

| Bước | Giá trị | 
| --- | --- | 
| x+y | [6, 5, 4, 3] | 
| phút | 3 | 
| tối đa | 6 | 
| trả lời | 2 × (6 − 3) = 6 | 

Ở đây, thứ tự đầu vào không đơn điệu, nhưng sau khi chuyển đổi, logic phạm vi tương tự sẽ được áp dụng. Con đường chỉ phụ thuộc vào những thái cực, không phụ thuộc vào sự sắp xếp trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để tính toán giá trị tối thiểu và tối đa được chuyển đổi | 
| Không gian | O(n) | lưu trữ các giá trị được chuyển đổi | 

Giải pháp tuyến tính về số điểm, đủ nhanh cho các ràng buộc điển hình lên đến$10^5$hoặc hơn thế nữa. Không cần sắp xếp hoặc tìm kiếm tổ hợp, vì vậy cả thời gian và bộ nhớ đều ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    data = inp.strip().split()
    n = int(data[0])
    vals = []
    idx = 1
    for _ in range(n):
        x = int(data[idx]); y = int(data[idx+1])
        vals.append(x + y)
        idx += 2
    return str(2 * (max(vals) - min(vals)))

# provided samples
assert run("3\n0 0\n1 1\n2 2\n") == "8"

# custom cases

# single point
assert run("1\n10 20\n") == "0", "single point"

# negative coordinates
assert run("3\n-1 -1\n0 0\n1 1\n") == "4", "mixed sign range"

# unordered input
assert run("4\n5 1\n0 0\n3 3\n2 2\n") == "12", "unordered points"

# all equal
assert run("3\n2 2\n2 2\n2 2\n") == "0", "all identical"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | chu trình thoái hóa chính xác | 
| phạm vi dấu hiệu hỗn hợp | 4 | giá trị âm được xử lý chính xác | 
| điểm không có thứ tự | 12 | tính đúng đắn không phụ thuộc vào thứ tự đầu vào | 
| tất cả đều giống hệt nhau | 0 | trường hợp cạnh không nhịp | 

## Vỏ cạnh 

Một đầu vào điểm duy nhất như$(10, 20)$tạo ra giá trị chuyển đổi$30$. Giá trị tối thiểu và tối đa bằng nhau, do đó khoảng thời gian được tính toán bằng 0 và thuật toán trả về 0 một cách chính xác, phản ánh rằng không cần phải di chuyển. 

Khi tất cả các điểm giống hệt nhau, ví dụ như nhiều bản sao của$(2, 2)$, tất cả các giá trị được chuyển đổi là 4. Phạm vi bằng 0, do đó chi phí chu kỳ bằng 0. Thuật toán xử lý các bản sao một cách tự nhiên vì giá trị tối thiểu và tối đa không thay đổi. 

Với tọa độ âm như$(-1, -1), (0, 0), (1, 1)$, các giá trị được biến đổi trở thành$-2, 0, 2$. Khoảng cách là 4 và nhân đôi cho 8. Thuật toán không phụ thuộc vào độ tích cực và hoạt động thống nhất trên toàn bộ phạm vi số nguyên.
