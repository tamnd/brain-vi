---
title: "CF 104619L - Vị trí, Vị trí, Vị trí"
description: "Chúng ta được cung cấp một tập hợp các điểm trên lưới 2D, mỗi điểm đại diện cho một vị trí như ngôi nhà hoặc căn hộ. Chúng ta phải chọn một điểm mới $(x, y)$ nơi sẽ xây dựng một trạm sạc."
date: "2026-06-29T17:28:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104619
codeforces_index: "L"
codeforces_contest_name: "2023 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 104619
solve_time_s: 61
verified: true
draft: false
---

[CF 104619L - Vị trí, Vị trí, Vị trí](https://codeforces.com/problemset/problem/104619/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm trên lưới 2D, mỗi điểm đại diện cho một vị trí như ngôi nhà hoặc căn hộ. Chúng ta phải chọn một điểm mới$(x, y)$nơi một trạm sạc sẽ được xây dựng. Chất lượng của một vị trí ứng cử viên được đo bằng tổng khoảng cách Manhattan từ vị trí đó đến mọi điểm nhất định. Khoảng cách Manhattan giữa hai điểm là khoảng cách ngang cộng với khoảng cách dọc. 

Nhiệm vụ là tìm một vị trí giảm thiểu tổng khoảng cách này. Nếu nhiều địa điểm đạt được số tiền tối thiểu như nhau, chúng ta phải chọn địa điểm có tổng nhỏ nhất$x$. Nếu vẫn hòa thì chọn cái nhỏ nhất$y$. 

Kích thước đầu vào tăng lên$n = 100000$và tọa độ được giới hạn trong$[-100000, 100000]$. Điều này ngay lập tức loại trừ mọi phương pháp đánh giá tất cả các vị trí ứng cử viên trong lưới hoặc tính toán khoảng cách từ mọi điểm có thể. Một lực lượng mạnh mẽ đối với các vị trí ứng cử viên sẽ là quá lớn vì ngay cả phạm vi tọa độ cũng mang lại$200001^2$những khả năng, điều không thể thực hiện được. 

Ý tưởng ngây thơ thứ hai là kiểm tra tất cả các điểm đầu vào như các vị trí cơ sở tiềm năng. Điều đó làm giảm khả năng ứng viên$n$, nhưng mỗi lần đánh giá vẫn tốn kém$O(n)$, dẫn đến$O(n^2)$tổng công việc. Với$n = 10^5$, điều này trở thành$10^{10}$tính toán khoảng cách, quá chậm. 

Trường hợp cạnh tinh vi phát sinh khi nhiều điểm có chung cấu trúc trung tuyến tối ưu. Ví dụ: nếu tất cả các điểm nằm trên một đường thẳng đứng$x = 0$, thì điểm bất kỳ với$x = 0$và trung vị$y$các giá trị là tối ưu. Việc triển khai bất cẩn tính toán các trung vị một cách độc lập nhưng không thực thi liên kết một cách nhất quán có thể trả về các tọa độ hợp lệ khác nhau, nhưng vấn đề đòi hỏi phải có sự lựa chọn xác định. 

## Phương pháp tiếp cận 

Quan sát quan trọng là khoảng cách Manhattan tách thành những đóng góp độc lập từ$x$Và$y$:$$|x - x_i| + |y - y_i| = |x - x_i| + |y - y_i|$$Vậy tổng chi phí sẽ là:$$\sum |x - x_i| + \sum |y - y_i|$$Sự phân rã này rất quan trọng vì nó có nghĩa là chúng ta có thể tối ưu hóa$x$Và$y$một cách độc lập. 

Đầu tiên chúng ta xem xét phiên bản 1D: chọn một giá trị$x$giảm thiểu$\sum |x - x_i|$. Một đặc tính nổi tiếng của độ lệch tuyệt đối là bất kỳ trung vị nào của nhiều tập hợp đều giảm thiểu tổng này. Theo trực giác, việc di chuyển điểm đã chọn sang trái hoặc phải sẽ tăng khoảng cách lên ít nhất bằng số điểm mà nó giảm đi. 

Như vậy, tối ưu$x$là trung tuyến của$x_i$các giá trị và tương tự là giá trị tối ưu$y$là trung tuyến của$y_i$các giá trị. 

Cách tiếp cận bạo lực sẽ đánh giá mọi ứng viên$x$Và$y$, tính lại tổng mỗi lần, tính giá thành$O(n)$mỗi đánh giá. Điều đó dẫn đến$O(n^2)$tổng thể phức tạp. Thông tin chi tiết trung bình thu gọn toàn bộ không gian tìm kiếm thành một phép tính xác định duy nhất bằng cách sử dụng tính năng sắp xếp. 

Việc bẻ cà vạt đòi hỏi sự cẩn thận. Khi$n$chẵn thì có nhiều số trung vị. Nguyên tắc “giảm thiểu$x$, sau đó giảm thiểu$y$” có nghĩa là chọn chỉ số trung vị thấp hơn (phần tử ở giữa bên trái theo thứ tự được sắp xếp), bởi vì nó tạo ra tọa độ hợp lệ nhỏ nhất trong số các giải pháp tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu (trung vị) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đối xử$x$Và$y$độc lập vì hàm chi phí được phân tách rõ ràng. 

1. Đọc tất cả các điểm và lưu trữ chúng$x$Và$y$tọa độ riêng biệt. Sự phân tách này là cần thiết vì chúng ta sẽ sắp xếp từng chiều một cách độc lập. 
2. Sắp xếp danh sách tất cả$x$-tọa độ. Việc sắp xếp là bắt buộc vì giá trị trung bình được xác định theo thống kê thứ tự, không phải giá trị thô. 
3. Chọn số trung vị$x$-điều phối. Nếu như$n$thật kỳ quặc, đây là phần tử ở giữa. Nếu như$n$chẵn, ta chọn chỉ số trung bình dưới để thỏa mãn yêu cầu tối thiểu hóa$x$trong số tất cả các giải pháp tối ưu. 
4. Lặp lại quy trình tương tự cho$y$-tọa độ, trích trung vị$y$. 
5. Đầu ra$(x, y)$như câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào tính chất là đối với bất kỳ tập hợp số thực nào, hàm$f(t) = \sum |t - a_i|$được giảm thiểu tại bất kỳ trung vị nào của tập hợp. Nếu chúng ta di chuyển$t$về bên trái của dải phân cách, nhiều điểm nằm bên phải hơn bên trái, làm tăng tổng khoảng cách; nếu chúng ta di chuyển sang phải, điều ngược lại sẽ xảy ra. Vì mục tiêu được chia thành các tổng độc lập trên$x$Và$y$, việc chọn các trung vị tối ưu cho từng tọa độ đồng thời giảm thiểu mục tiêu Manhattan 2D đầy đủ. Quy tắc ràng buộc được thỏa mãn bằng cách chọn chỉ số trung bình thấp hơn, đảm bảo tọa độ tối thiểu về mặt từ điển trong số tất cả các giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    xs = []
    ys = []
    
    for _ in range(n):
        x, y = map(int, input().split())
        xs.append(x)
        ys.append(y)
    
    xs.sort()
    ys.sort()
    
    # lower median for tie-breaking
    x_ans = xs[(n - 1) // 2]
    y_ans = ys[(n - 1) // 2]
    
    print(x_ans, y_ans)

if __name__ == "__main__":
    solve()
```Giải pháp chia đầu vào thành hai mảng để có thể áp dụng cách sắp xếp một cách độc lập. Việc sắp xếp từng mảng đảm bảo chúng ta có thể truy cập trực tiếp vào trung vị trong thời gian không đổi sau khi xử lý trước. chỉ số$(n - 1) // 2$được chọn cụ thể để thực thi quy tắc ràng buộc, đảm bảo rằng khi tồn tại nhiều đường trung vị tối ưu, chúng tôi sẽ chọn tọa độ khả thi nhỏ nhất. 

Một lỗi triển khai phổ biến là sử dụng`n // 2`một cách mù quáng mà không xem xét các trường hợp có quy mô chẵn và các ràng buộc về từ điển. sử dụng`(n - 1) // 2`tránh vấn đề đó bằng cách luôn chọn trung vị bên trái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 2
2 3
1 0
```Đã sắp xếp$x$: [0, 1, 2] 

Đã sắp xếp$y$: [0, 2, 3] 

| Bước | xs | vâng | đã chọn x | đã chọn y | 
| --- | --- | --- | --- | --- | 
| được sắp xếp | [0,1,2] | [0,2,3] | - | - | 
| trung vị | - | - | 1 | 2 | 

Các phần tử trung vị là$x = 1$,$y = 2$. Điểm này giảm thiểu tổng khoảng cách Manhattan. 

Điều này khẳng định đặc tính của phần tử ở giữa là cân bằng khoảng cách ở cả hai bên. 

### Ví dụ 2 

đầu vào:```
2
0 0
2 2
```Đã sắp xếp$x$: [0, 2] 

Đã sắp xếp$y$: [0, 2] 

| Bước | xs | vâng | đã chọn x | đã chọn y | 
| --- | --- | --- | --- | --- | 
| được sắp xếp | [0,2] | [0,2] | - | - | 
| quy tắc trung vị | - | - | 0 | 0 | 

Ở đây cả hai đường trung tuyến đều không phải là duy nhất. Quy tắc chọn trung vị thấp hơn, đưa ra$(0, 0)$, là tối ưu và thỏa mãn sự ràng buộc. 

Điều này chứng tỏ các đầu vào có kích thước đồng đều yêu cầu lựa chọn trung vị nhất quán như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp mảng x và y chiếm ưu thế | 
| Không gian |$O(n)$| lưu trữ mảng tọa độ | 

Các ràng buộc cho phép lên đến$10^5$điểm và việc phân loại hai lần ở thang đo này nằm trong giới hạn. Thuật toán sử dụng bộ nhớ bổ sung tuyến tính, có thể chấp nhận được dưới giới hạn 2048 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    input = _sys.stdin.readline

    n = int(input())
    xs = []
    ys = []
    for _ in range(n):
        x, y = map(int, input().split())
        xs.append(x)
        ys.append(y)

    xs.sort()
    ys.sort()
    return f"{xs[(n-1)//2]} {ys[(n-1)//2]}"

# provided sample 1
assert run("""3
0 2
2 3
1 0
""") == "1 2"

# provided sample 2
assert run("""2
0 0
2 2
""") == "0 0"

# all same point
assert run("""3
5 5
5 5
5 5
""") == "5 5"

# vertical line
assert run("""4
0 1
0 2
0 3
0 4
""") == "0 2"

# horizontal line
assert run("""4
1 0
2 0
3 0
4 0
""") == "2 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mọi điểm bằng nhau | cùng điểm | ổn định và trung vị tầm thường | 
| đường thẳng đứng | (0, trung vị y) | sự độc lập của x và y | 
| đường ngang | (trung vị x, 0) | hành vi đối xứng | 

## Vỏ cạnh 

Trường hợp cạnh chính là chẵn$n$, nơi tồn tại nhiều đường trung bình và các vấn đề liên quan đến ràng buộc. 

Coi như:```
2
0 0
2 2
```Đã sắp xếp$x = [0, 2]$, được sắp xếp$y = [0, 2]$. Thuật toán chọn chỉ số$(2 - 1) // 2 = 0$, cho$x = 0$,$y = 0$. Thay vào đó, nếu chúng ta chọn trung vị trên, chúng ta sẽ nhận được$(2, 2)$, có cùng tổng chi phí nhưng vi phạm yêu cầu giảm thiểu$x$, sau đó$y$. Lựa chọn trung bình thấp hơn sẽ thực thi điểm tối ưu nhỏ nhất về mặt từ điển. 

Một trường hợp khác là khi tất cả các điểm đều giống hệt nhau:```
3
7 7
7 7
7 7
```Cả hai mảng được sắp xếp đều không đổi, do đó, bất kỳ chỉ số trung vị nào cũng mang lại 7. Thuật toán trả về$(7, 7)$một cách nhất quán, phù hợp với giải pháp tối ưu khả thi duy nhất.
