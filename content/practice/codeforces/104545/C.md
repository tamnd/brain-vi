---
title: "CF 104545C - Giải lao cà phê"
description: "Chúng ta có một lưới nhị phân nhỏ biểu thị một bảng đồ ăn nhẹ, trong đó mỗi ô là số 1 (quả bóng phô mai) hoặc số 0 (coxinha)."
date: "2026-06-30T08:56:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104545
codeforces_index: "C"
codeforces_contest_name: "VIII MaratonUSP Freshman Contest"
rating: 0
weight: 104545
solve_time_s: 54
verified: true
draft: false
---

[CF 104545C - Nghỉ giải lao](https://codeforces.com/problemset/problem/104545/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới nhị phân nhỏ biểu thị một bảng đồ ăn nhẹ, trong đó mỗi ô là số 1 (quả bóng phô mai) hoặc số 0 (coxinha). Nhiệm vụ là chọn chính xác một lưới con hình chữ nhật thẳng hàng với trục và thu thập tất cả đồ ăn nhẹ bên trong nó, với hạn chế là hình chữ nhật không được chứa bất kỳ coxinhas nào cả. Nói cách khác, mọi ô được chọn bên trong hình chữ nhật phải là 1. 

Mục tiêu là tối đa hóa số lượng pho mát được thu thập, tương đương với việc tối đa hóa diện tích của một hình chữ nhật con bao gồm toàn bộ 1 giây. 

Ràng buộc n × m ≤ 400 là đầu mối cấu trúc quan trọng. Ngay cả trong trường hợp xấu nhất, đây là một lưới rất nhỏ, cho phép các giải pháp bậc hai hoặc kém hơn một chút trên mỗi ô mà không có nguy cơ vượt quá giới hạn thời gian. Tuy nhiên, nó cũng gợi ý rằng giải pháp dự định không phải là giải pháp mạnh mẽ đối với tất cả các hình chữ nhật, vì điều đó vẫn liên quan đến việc kiểm tra nhiều ma trận con nhiều lần, dẫn đến chi phí không cần thiết và việc triển khai khó khăn. 

Một cách tiếp cận đơn giản liệt kê mọi hình chữ nhật có thể có và xác minh xem liệu tất cả những hình chữ nhật đó có thể âm thầm thất bại về hiệu suất hoặc độ phức tạp khi triển khai hay không. Ví dụ: ngay cả trên lưới 20 × 20, có khoảng 10^8 hình chữ nhật và việc kiểm tra từng hình sẽ tốn thêm công sức. 

Một trường hợp thất bại tinh tế hơn đối với việc triển khai đơn giản xuất phát từ việc xác thực hình chữ nhật không chính xác. Giả sử chúng ta chọn một hình chữ nhật và chỉ kiểm tra các góc của nó hoặc quét một phần: 

đầu vào:```
3 3
1 1 1
1 0 1
1 1 1
```Người kiểm tra bất cẩn có thể xác thực một hình chữ nhật bao trùm toàn bộ lưới bằng cách chỉ kiểm tra các đường viền và kết luận nó hợp lệ, trả về sai 9. Câu trả lời đúng là 4, đến từ bất kỳ ô vuông phụ 2 × 2 nào tránh được số 0 ở giữa. 

Điều này cho thấy tính chính xác phụ thuộc vào cấu trúc toàn cục bên trong hình chữ nhật chứ không chỉ các giá trị biên. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê tất cả các hình chữ nhật con có thể được xác định bởi hàng trên cùng, hàng dưới cùng, cột bên trái và cột bên phải và đối với mỗi hình chữ nhật con, hãy quét từng ô để kiểm tra xem nó có chứa số 0 hay không. Nếu không, hãy tính diện tích của nó và cập nhật câu trả lời. 

Điều này hiệu quả vì nó trực tiếp thực thi định nghĩa của vấn đề. Tuy nhiên, chi phí của nó tăng quá nhanh. Có các hình chữ nhật O(n^2 m^2) và mỗi lần xác minh có chi phí O(nm) trong trường hợp xấu nhất, dẫn đến giới hạn O(n^3 m^3) về mặt lý thuyết. Ngay cả với n × m ≤ 400, tốc độ này vẫn quá chậm trong thực tế. 

Quan sát quan trọng là chúng ta thực sự không cần phải tính lại tính hợp lệ của mọi hình chữ nhật từ đầu. Thay vào đó, chúng ta có thể sử dụng lại cấu trúc trên các hàng. Nếu cố định hàng dưới cùng, chúng ta có thể nén lưới thành biểu đồ trong đó mỗi cột lưu trữ bao nhiêu số 1 liên tiếp kết thúc ở hàng đó. Bất kỳ hình chữ nhật tổng hợp nào kết thúc ở hàng đó sẽ trở thành một đoạn liền kề trong biểu đồ này. Vấn đề sau đó trở thành hình chữ nhật lớn nhất cổ điển trong biểu đồ, được giải quyết hiệu quả bằng ngăn xếp đơn điệu. 

Điều này biến việc tìm kiếm hình chữ nhật 2D thành một chuỗi các bài toán 1D, mỗi bài có thể giải được theo thời gian tuyến tính trên mỗi hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^2 · nm) | O(1) | Quá chậm | 
| Biểu đồ DP + Stack | O(nm) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng hàng lưới, duy trì một mảng biểu thị độ cao biểu đồ của các hàng liên tiếp. 

1. Khởi tạo một mảng`height`có kích thước m với số không. Mảng này biểu thị, đối với mỗi cột, có bao nhiêu cột liên tiếp chúng ta đã thấy kết thúc ở hàng hiện tại. 
2. Đối với mỗi hàng, hãy cập nhật biểu đồ bằng cách quét các cột. Nếu ô hiện tại là 1, hãy tăng`height[j]`bằng 1, nếu không thì đặt lại về 0. Bước này chuyển đổi ràng buộc 2D thành biểu diễn 1D của các đường chạy dọc. 
3. Để cập nhật`height`mảng, tính hình chữ nhật lớn nhất trong biểu đồ. Điều này được thực hiện bằng cách sử dụng một chồng chỉ số tăng dần đơn điệu. Khi chúng tôi gặp chiều cao nhỏ hơn đỉnh ngăn xếp, chúng tôi liên tục bật lên và tính toán các khu vực bằng cách sử dụng chiều cao đã bật lên làm chiều cao giới hạn và chỉ mục hiện tại làm ranh giới bên phải. 
4. Theo dõi diện tích tối đa nhìn thấy trên tất cả các hàng. 
5. Sau khi xử lý tất cả các hàng, xuất ra vùng được ghi tối đa. 

Bước suy luận quan trọng là mọi hình chữ nhật hợp lệ đều có một hàng dưới cùng duy nhất. Bằng cách coi mỗi hàng là ranh giới đáy tiềm năng, chúng tôi đảm bảo rằng mọi hình chữ nhật được xem xét chính xác một lần trong giai đoạn biểu đồ. 

### Tại sao nó hoạt động 

Tại bất kỳ hàng r nào,`height[c]`mã hóa chiều cao tối đa có thể có của hình chữ nhật kết thúc ở r và kéo dài lên trên cột c mà không bị gián đoạn. Bất kỳ hình chữ nhật tổng hợp nào có hàng dưới cùng r đều tương ứng với một đoạn cột liền kề trong đó tất cả các chiều cao ít nhất bằng chiều cao hình chữ nhật. Công thức biểu đồ nắm bắt chính xác những hạn chế này, do đó hình chữ nhật lớn nhất trong biểu đồ chính xác là hình chữ nhật tốt nhất kết thúc ở hàng r. Vì mọi hình chữ nhật đều có một số hàng dưới cùng nên việc quét tất cả các hàng sẽ bao gồm tất cả các khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def largest_histogram(heights):
    stack = []
    best = 0
    heights.append(0)
    
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            left = stack[-1] if stack else -1
            width = i - left - 1
            best = max(best, height * width)
        stack.append(i)
    
    heights.pop()
    return best

def solve():
    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]
    
    height = [0] * m
    answer = 0
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                height[j] += 1
            else:
                height[j] = 0
        
        answer = max(answer, largest_histogram(height))
    
    print(answer)

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì một biểu đồ cuộn`height`tích lũy những cái liên tiếp theo chiều dọc. Hàm trợ giúp tính toán hình chữ nhật tốt nhất theo thời gian tuyến tính trên mỗi hàng bằng cách sử dụng ngăn xếp tăng chiều cao. Số 0 trọng điểm được thêm vào biểu đồ đảm bảo rằng tất cả các phần tử ngăn xếp còn lại sẽ bị xóa khi kết thúc tính toán. 

Chi tiết triển khai tinh tế duy nhất là đặt lại chiều cao của cột về 0 ngay lập tức khi gặp ô số 0, vì bất kỳ hình chữ nhật nào bao gồm ô đó sẽ trở nên không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
1 1
1 1
```Sự phát triển và tính toán biểu đồ: 

| Hàng | Mảng chiều cao | Khu vực biểu đồ tốt nhất | 
| --- | --- | --- | 
| 1 | [1, 1] | 2 | 
| 2 | [2, 2] | 4 | 

Hàng thứ hai tạo ra biểu đồ trong đó cả hai cột đều đạt chiều cao 2, tạo thành hình chữ nhật 2×2. 

Điều này xác nhận rằng sự tích lũy theo chiều dọc chụp chính xác các hình chữ nhật trải dài trên nhiều hàng. 

### Ví dụ 2 

đầu vào:```
3 3
1 0 1
0 1 1
0 1 1
```| Hàng | Mảng chiều cao | Khu vực biểu đồ tốt nhất | 
| --- | --- | --- | 
| 1 | [1, 0, 1] | 1 | 
| 2 | [0, 1, 2] | 2 | 
| 3 | [0, 2, 3] | 4 | 

Ở hàng cuối cùng, cột 2 và 3 tạo thành một cầu thang có chiều cao ngày càng tăng và biểu đồ xác định hình chữ nhật 2 × 2 tất cả. 

Điều này chứng tỏ phương pháp này tích lũy tính liên tục theo chiều dọc một cách tự nhiên và chuyển nó thành sự mở rộng chiều rộng theo chiều ngang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi hàng được xử lý theo O(m) và tính toán biểu đồ là tuyến tính do các hoạt động ngăn xếp đơn điệu | 
| Không gian | O(m) | Chỉ có mảng chiều cao và ngăn xếp được lưu trữ | 

Với n × m 400, giải pháp này chạy cực kỳ nhanh, nằm trong giới hạn ngay cả khi có chi phí sử dụng Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("2 2\n1 1\n1 1\n") == "4"
assert run("3 3\n1 0 1\n0 1 1\n0 1 1\n") == "4"

# single cell
assert run("1 1\n1\n") == "1"

# all zeros
assert run("2 3\n0 0 0\n0 0 0\n") == "0"

# all ones larger rectangle
assert run("2 3\n1 1 1\n1 1 1\n") == "6"

# mixed pattern
assert run("3 4\n1 1 0 1\n1 1 1 1\n1 0 1 0\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 1 | trường hợp tối thiểu | 
| tất cả số không | 0 | không có hình chữ nhật hợp lệ | 
| đầy đủ | 6 | độ chính xác đầy đủ của hình chữ nhật | 
| lưới hỗn hợp | 4 | xử lý gián đoạn | 

## Vỏ cạnh 

Lưới hoàn toàn bằng 0 kiểm tra xem thuật toán có đặt lại đúng độ cao biểu đồ hay không thay vì tích lũy các giá trị cũ. Khi mỗi hàng bằng 0, mảng chiều cao vẫn bằng 0 và biểu đồ luôn trả về 0, tạo ra câu trả lời đúng. 

Lưới một hàng làm giảm vấn đề thành biểu đồ thuần túy. Thuật toán tính toán chính xác đoạn liền kề dài nhất trong số 1, vì tất cả các độ cao đều là 0 hoặc 1. 

Một lưới đầy đủ sẽ kiểm tra sự tích lũy tối đa. Mỗi hàng tăng tất cả các giá trị biểu đồ và hàng cuối cùng tạo ra một biểu đồ trong đó toàn bộ chiều rộng có thể sử dụng được với chiều cao tối đa, mang lại n × m như mong đợi.
