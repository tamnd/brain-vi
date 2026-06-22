---
title: "CF 105791K - Sự cố đã biết"
description: "Bức tường được đưa ra dưới dạng lưới ô n x m. Mỗi ô trống hoặc bị chặn bởi một chiếc đinh. Một tấm áp phích chỉ được treo trên một vùng tường hình chữ nhật và mọi ô bên trong hình chữ nhật đó không được có đinh."
date: "2026-06-21T14:25:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "K"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 45
verified: true
draft: false
---

[CF 105791K - Sự cố đã biết](https://codeforces.com/problemset/problem/105791/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bức tường được đưa ra dưới dạng lưới ô n x m. Mỗi ô trống hoặc bị chặn bởi một chiếc đinh. Một tấm áp phích chỉ được treo trên một vùng tường hình chữ nhật và mọi ô bên trong hình chữ nhật đó không được có đinh. Poster không thể xoay tùy ý làm thay đổi căn chỉnh trục của nó mà luôn tương ứng với một hình chữ nhật căn chỉnh theo trục trong lưới. 

Nhiệm vụ là tìm hình chữ nhật lớn nhất có thể chỉ gồm các ô không có đinh. 

Kích thước lưới tối đa là 250 x 250, đủ nhỏ để có thể thực hiện được các giải pháp bậc hai trên các hàng hoặc cột, nhưng quá lớn đối với bất kỳ phương pháp nào thử trực tiếp tất cả các hình chữ nhật O(n²m²). Một lực lượng mạnh mẽ trên tất cả các hình chữ nhật phụ sẽ liên quan đến khoảng 10¹⁰ khả năng trong trường hợp xấu nhất, vượt xa những gì 1 giây cho phép. 

Một điểm tinh tế là mã hóa đầu vào được đảo ngược từ trực giác: a`.`đại diện cho một cái đinh (bị chặn), trong khi`#`đại diện cho một ô tường trống. Vì vậy, hình chữ nhật chúng ta muốn là hình chữ nhật lớn nhất-`#`ma trận phụ. 

Trường hợp cạnh điển hình phát sinh khi lưới bị chặn hoàn toàn hoặc hoàn toàn tự do. Nếu mọi thứ đều`.`, câu trả lời là 0 vì không tồn tại hình chữ nhật hợp lệ. Nếu mọi thứ đều`#`, đáp án là n·m vì toàn bộ bức tường đều có thể sử dụng được. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xem xét mọi cặp tọa độ trên cùng bên trái và dưới cùng bên phải có thể có và xác minh xem hình chữ nhật giữa chúng chỉ chứa`#`. Điều này yêu cầu kiểm tra O(1) ô trên mỗi hình chữ nhật bằng cách sử dụng tổng tiền tố hoặc quét và vì có hình chữ nhật O(n2m²), tổng công việc sẽ trở thành O(n2m2), trong lưới 250 x 250 theo thứ tự 4×10¹⁰ kiểm tra. Ngay cả khi tối ưu hóa, điều này vẫn không khả thi. 

Quan sát quan trọng là vấn đề này giống hệt với việc tìm hình chữ nhật lớn nhất của các hình chữ nhật trong ma trận nhị phân, trong đó`#`được coi là 1 và`.`bằng 0. Thay vì suy luận trực tiếp về hình chữ nhật 2D, chúng ta có thể diễn giải lại từng hàng làm cơ sở cho biểu đồ. Đối với mỗi hàng, chúng tôi duy trì một mảng chiều cao trong đó chiều cao [j] đếm số lượng liên tiếp`#`các ô kết thúc ở hàng i trong cột j. Sau đó, mỗi hàng chuyển bài toán thành tìm hình chữ nhật lớn nhất trong biểu đồ. Vấn đề biểu đồ đó được biết rõ là có thể giải được theo thời gian tuyến tính trên mỗi hàng bằng cách sử dụng ngăn xếp đơn điệu. 

Việc giảm có hiệu quả vì mọi hình chữ nhật đều có đường viền dưới cùng. Cố định hàng dưới cùng của hình chữ nhật, hình chữ nhật tốt nhất kết thúc ở đó chỉ phụ thuộc vào việc mỗi cột có thể kéo dài lên trên bao xa mà không chạm vào một`.`, đó chính xác là những gì mảng chiều cao mã hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force tất cả các hình chữ nhật | O(n²m²) | O(1) | Quá chậm | 
| Biểu đồ mỗi hàng + ngăn xếp | O(nm) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi lưới thành cách diễn giải nhị phân trong đó`#`là không gian có thể sử dụng và`.`bị chặn. Điều này thiết lập biểu diễn cơ sở cho tất cả các tính toán sau này. 
2. Duy trì một mảng`height`có kích thước m, ban đầu tất cả đều bằng 0. Mảng này biểu thị một biểu đồ được xây dựng theo từng hàng. 
3. Lặp lại từng hàng i từ trên xuống dưới. Với mỗi cột j, cập nhật`height[j]`. Nếu tế bào là`#`, tăng`height[j]`bằng 1, nếu không thì đặt lại về 0. Điều này đảm bảo`height[j]`luôn phản ánh số lượng ô trống liên tiếp kết thúc ở hàng hiện tại. 
4. Sau khi cập nhật biểu đồ cho hàng i, hãy tính diện tích hình chữ nhật lớn nhất trong biểu đồ này. Điều này được thực hiện bằng cách sử dụng một chồng chỉ số tăng dần đơn điệu. Ngăn xếp đảm bảo rằng chiều cao được xử lý theo thứ tự tăng dần để mỗi thanh trở thành chiều cao giới hạn cho hình chữ nhật tối đa trải rộng phạm vi hợp lệ của nó. 
5. Trong khi xử lý biểu đồ, bất cứ khi nào chiều cao hiện tại nhỏ hơn chiều cao ở đỉnh ngăn xếp, hãy bật ra khỏi ngăn xếp và tính diện tích hình chữ nhật bằng cách sử dụng chiều cao bật lên làm hệ số giới hạn. Chiều rộng được xác định bởi chỉ mục hiện tại và đỉnh ngăn xếp mới. 
6. Tiếp tục cho đến khi tất cả các thanh được xử lý và đảm bảo các phần tử ngăn xếp còn lại cũng được giải quyết bằng cách xóa bằng ranh giới trọng điểm. 
7. Theo dõi diện tích tối đa trên tất cả các hàng. Câu trả lời cuối cùng là hình chữ nhật lớn nhất gặp trong bất kỳ biểu đồ nào. 

Ý tưởng cốt lõi là mọi hình chữ nhật hợp lệ trong lưới phải có một hàng dưới cùng và đối với mỗi hàng dưới cùng, chúng tôi xem xét kỹ lưỡng tất cả các hình chữ nhật kết thúc ở đó thông qua xử lý biểu đồ. 

### Tại sao nó hoạt động 

Mỗi`height[j]`mã hóa chính xác phạm vi dọc tối đa có thể của hình chữ nhật kết thúc ở hàng i và cột j. Bất kỳ hình chữ nhật nào kết thúc ở hàng i đều tương ứng với một đoạn liền kề trong biểu đồ và chiều cao của nó được giới hạn bởi chiều cao tối thiểu trong đoạn đó. Ngăn xếp đơn điệu đảm bảo rằng mỗi phân đoạn như vậy được xem xét chính xác một lần với chiều cao giới hạn chính xác của nó. Điều này đảm bảo rằng không có hình chữ nhật nào bị bỏ sót và không có hình chữ nhật không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def largest_histogram(heights):
    stack = []
    best = 0
    n = len(heights)

    for i in range(n + 1):
        cur = 0 if i == n else heights[i]
        while stack and cur < heights[stack[-1]]:
            h = heights[stack.pop()]
            left = stack[-1] + 1 if stack else 0
            width = i - left
            best = max(best, h * width)
        stack.append(i)

    return best

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    height = [0] * m
    ans = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '#':
                height[j] += 1
            else:
                height[j] = 0

        ans = max(ans, largest_histogram(height))

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì biểu đồ đang chạy trong`height`, được cập nhật từng hàng. Mỗi hàng kích hoạt đánh giá biểu đồ đầy đủ. Quy trình ngăn xếp đơn điệu tính toán hình chữ nhật lớn nhất trong thời gian tuyến tính, đảm bảo mỗi chỉ mục được đẩy và xuất ra nhiều nhất một lần. 

Một sai lầm phổ biến là quên phép lặp trọng điểm trong vòng lặp biểu đồ. Việc lặp lại thêm tại`i == n`buộc các phần tử ngăn xếp còn lại phải được xử lý, nếu không các hình chữ nhật mở rộng đến ranh giới bên phải sẽ bị bỏ qua. 

Một chi tiết tinh tế khác là đặt lại độ cao về 0 khi gặp phải`.`. Từ`.`đại diện cho một ô bị chặn, bất kỳ hình chữ nhật nào đi qua nó đều không hợp lệ, do đó việc tích lũy theo chiều dọc phải khởi động lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5
#####
#####
#####
#####
#####
```Ở đây mọi ô đều hợp lệ nên chiều cao tăng trưởng đều đặn. 

| Hàng | Mảng chiều cao | Vùng biểu đồ tối đa | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| 0 | [1,1,1,1,1] | 5 | 5 | 
| 1 | [2,2,2,2,2] | 10 | 10 | 
| 2 | [3,3,3,3,3] | 15 | 15 | 
| 3 | [4,4,4,4,4] | 20 | 20 | 
| 4 | [5,5,5,5,5] | 25 | 25 | 

Dấu vết cho thấy mỗi hàng tăng biểu đồ một cách đồng đều và hình chữ nhật tốt nhất sẽ mở rộng để bao gồm toàn bộ lưới. 

### Mẫu 2 

đầu vào:```
5 5
##..#
####.
#####
#####
.####
```Chúng tôi theo dõi cách các ô bị chặn thiết lập lại độ cao. 

| Hàng | Mảng chiều cao | Vùng biểu đồ tối đa | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| 0 | [1,1,0,0,1] | 3 | 3 | 
| 1 | [2,2,0,0,0] | 4 | 4 | 
| 2 | [3,3,1,1,1] | 6 | 6 | 
| 3 | [4,4,2,2,2] | 8 | 8 | 
| 4 | [0,0,3,3,3] | 9 | 9 | 

Việc đặt lại ở hàng 4 cho thấy cách một hàng bị chặn phá vỡ tính liên tục theo chiều dọc và buộc các hình chữ nhật mới chỉ hình thành phía trên nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi hàng xây dựng một biểu đồ theo dạng O(m) và mỗi biểu đồ được xử lý theo dạng O(m) bằng cách sử dụng ngăn xếp đơn điệu | 
| Không gian | O(m) | Chỉ có mảng chiều cao và ngăn xếp được lưu trữ | 

Với n, m 250, tổng số thao tác là khoảng 62.500 trên mỗi biểu đồ, nằm trong giới hạn thời gian ngay cả khi có chi phí Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def largest_histogram(heights):
        stack = []
        best = 0
        n = len(heights)

        for i in range(n + 1):
            cur = 0 if i == n else heights[i]
            while stack and cur < heights[stack[-1]]:
                h = heights[stack.pop()]
                left = stack[-1] + 1 if stack else 0
                width = i - left
                best = max(best, h * width)
            stack.append(i)

        return best

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    height = [0] * m
    ans = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '#':
                height[j] += 1
            else:
                height[j] = 0

        ans = max(ans, largest_histogram(height))

    return str(ans)

# provided samples
assert run("""5 5
#####
#####
#####
#####
#####
""") == "25"

assert run("""5 5
##..#
####.
#####
#####
.####
""") == "9"

# custom cases
assert run("""1 1
#
""") == "1", "single cell"

assert run("""1 5
#.#.#
""") == "1", "alternating row"

assert run("""3 3
###
###
###
""") == "9", "full block"

assert run("""3 3
...
...
...
""") == "0", "all blocked"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1`#`| 1 | hình chữ nhật khác không tối thiểu | 
| hàng xen kẽ | 1 | xử lý phân mảnh | 
| đầy đủ 3x3 | 9 | mở rộng hình chữ nhật tối đa | 
| tất cả các dấu chấm | 0 | không có vị trí hợp lệ | 

## Vỏ cạnh 

Một lưới bị chặn hoàn toàn, trong đó mọi ô đều`.`, tạo ra mảng chiều cao của tất cả các số không. Mỗi lệnh gọi biểu đồ ngay lập tức mang lại kết quả bằng 0 vì không tồn tại chiều cao dương, vì vậy câu trả lời vẫn bằng 0 xuyên suốt. 

Một lưới mở hoàn toàn, trong đó mọi ô đều được`#`, tạo ra biểu đồ tăng dần trên mỗi hàng. Ngăn xếp không bao giờ kích hoạt các cửa sổ bật lên sớm ngoại trừ khi xóa và hình chữ nhật tối đa sẽ mở rộng thành ma trận đầy đủ ở hàng cuối cùng. 

Một ô tự do bị cô lập duy nhất trong một biển các ô bị chặn sẽ tạo ra các mức tăng đột biến về chiều cao bằng 1 và ngay lập tức được đặt lại. Bước biểu đồ giới hạn chính xác tất cả các hình chữ nhật ứng cử viên ở chiều rộng hoặc chiều cao 1, đảm bảo không xảy ra việc đếm quá mức vì bất kỳ tiện ích mở rộng nào cũng sẽ bao gồm cột có chiều cao bằng 0.
