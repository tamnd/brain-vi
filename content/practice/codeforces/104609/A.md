---
title: "CF 104609A - Những con ong bận rộn"
description: "Chúng ta được cung cấp một tập hợp các vị trí trong một lưới lục giác vô hạn, trong đó mỗi vị trí chứa một nhóm ong thợ. Chuyển động xảy ra dọc theo các cạnh chung của các ô hex và chi phí giữa hai ô là số lần di chuyển tối thiểu cần thiết để di chuyển giữa chúng."
date: "2026-06-30T02:45:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104609
codeforces_index: "A"
codeforces_contest_name: "Udmurt SU + Izhevsk STU Contest 2012"
rating: 0
weight: 104609
solve_time_s: 54
verified: true
draft: false
---

[CF 104609A - Những con ong bận rộn](https://codeforces.com/problemset/problem/104609/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các vị trí trong một lưới lục giác vô hạn, trong đó mỗi vị trí chứa một nhóm ong thợ. Chuyển động xảy ra dọc theo các cạnh chung của các ô hex và chi phí giữa hai ô là số lần di chuyển tối thiểu cần thiết để di chuyển giữa chúng. Chúng ta phải chọn một ô cho nữ hoàng sao cho tổng khoảng cách từ ô đó đến tất cả các vị trí công nhân là nhỏ nhất. 

Đầu vào chỉ đơn giản là danh sách N ô hex bị chiếm giữ. Đầu ra là tọa độ một ô giúp giảm thiểu tổng khoảng cách di chuyển từ tất cả công nhân đến vị trí của quân hậu. 

Ràng buộc N lên tới 100.000 ngay lập tức loại trừ bất kỳ giải pháp nào thử mọi ô có thể hoặc tính toán tất cả các khoảng cách theo cặp. Ngay cả việc đánh giá một vị trí ứng cử viên đối với tất cả công nhân cũng là O(N), do đó, một tìm kiếm đơn giản trên tất cả các ứng viên sẽ dẫn đến O(N^2). Chúng ta cần một cấu trúc trong đó mục tiêu phân tách rõ ràng thành các thành phần độc lập. 

Một khó khăn nhỏ là đây là lưới lục giác, không phải lưới Manhattan tiêu chuẩn. Một cách tiếp cận bất cẩn có thể cố gắng xử lý tọa độ một cách độc lập như x và y mà không tính đến trục ẩn thứ ba trong hình học hex. Ví dụ: hai điểm trông có vẻ được căn chỉnh theo đường chéo trong cách diễn giải lưới vuông thực tế có thể có khoảng cách thực khác nhau trong hệ thống hex. Một sai lầm tiềm ẩn khác là giả định rằng tọa độ trung bình hoạt động trực tiếp như trong hình học Euclide, điều này không hợp lệ ở đây. 

## Phương pháp tiếp cận 

Một ý tưởng táo bạo là thử mọi tế bào công nhân làm ứng cử viên cho vị trí nữ hoàng. Đối với mỗi ứng viên, chúng tôi tính tổng khoảng cách hex tới tất cả các công nhân. Vì việc tính toán tổng yêu cầu tính toán khoảng cách O(N) và có N ứng cử viên, điều này dẫn đến các phép toán O(N^2), quá chậm đối với 10^5 điểm. 

Quan sát quan trọng là tọa độ lưới hex có thể được chuyển đổi thành hệ tọa độ khối 3D trong đó khoảng cách trở thành khoảng cách Manhattan theo ba chiều. Mỗi ô hex (x, y) có thể được ánh xạ vào (x, y, z) với ràng buộc x + y + z = 0, điển hình là z = -x - y. Theo biểu diễn này, khoảng cách giữa hai ô hex trở thành chênh lệch tuyệt đối tối đa giữa ba tọa độ, tương đương với cấu trúc khoảng cách Manhattan qua ba trục. 

Khi bài toán được biểu diễn bằng tọa độ khối, nhiệm vụ sẽ trở thành cực tiểu hóa tổng khoảng cách dọc theo các trục bị ràng buộc. Áp dụng kết quả tiêu chuẩn từ tối ưu hóa 1D: tổng độ lệch tuyệt đối được giảm thiểu ở mức trung bình. Mặc dù khoảng cách hex đầy đủ sử dụng cấu trúc tối đa, nhưng nó có thể được phân tách thành ba hình chiếu tuyến tính và giải pháp tối ưu sẽ căn chỉnh với các đường trung bình trong mỗi trục theo ràng buộc. 

Vì vậy, thay vì tìm kiếm trên lưới, chúng tôi chuyển đổi tất cả các điểm thành tọa độ khối, tính toán các trung vị dọc theo x và y và tái tạo lại z một cách ngầm định. Bất kỳ tọa độ nào thỏa mãn ràng buộc đều mang lại một ô tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Tối ưu (khối + trung vị) | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi từng tọa độ hex đầu vào (x, y) thành tọa độ khối bằng cách coi nó là (x, y, z) trong đó z được lấy từ z = -x - y. Phép chuyển đổi này bảo toàn khoảng cách theo cách giúp chúng tối ưu hóa dễ dàng hơn. 
2. Lưu trữ tất cả giá trị x và tất cả giá trị y trong các mảng riêng biệt. Chúng tôi không cần z một cách rõ ràng vì nó được xác định bởi ràng buộc và sẽ tự động căn chỉnh khi x và y được cố định. 
3. Sắp xếp cả hai mảng một cách độc lập. Việc sắp xếp là cần thiết vì trung vị là điểm giảm thiểu tổng độ lệch tuyệt đối trong một chiều. 
4. Chọn trung vị của mảng x làm tọa độ x ứng viên. Nếu N chẵn thì một trong hai phần tử ở giữa đều hoạt động vì cả hai đều giảm thiểu độ lệch tuyệt đối như nhau. 
5. Chọn trung vị của mảng y tương tự. 
6. Tính z ngầm là -x - y nếu cần để xác minh, nhưng đầu ra chỉ yêu cầu tọa độ hex 2D ban đầu, vì vậy chúng tôi xuất trực tiếp (x, y) đã chọn. 
7. Trả tọa độ này về vị trí của quân hậu. 

Lý do chúng ta có thể xử lý x và y một cách độc lập là vì cấu trúc khoảng cách hex, sau khi được chuyển đổi thành tọa độ khối, sẽ phân tách thành các đóng góp cộng dọc theo các trục và mỗi trục được giảm thiểu độc lập bởi đường trung tuyến của nó. 

### Tại sao nó hoạt động 

Trong một chiều, hàm tổng |xi - c| được giảm thiểu khi c là trung vị của các giá trị xi. Trong tọa độ khối, mỗi chuyển động góp phần tạo ra những thay đổi dọc theo các trục bị ràng buộc và tổng chi phí hoạt động giống như sự kết hợp của các độ lệch tuyệt đối trên các trục này. Bất kỳ sai lệch nào so với giá trị trung bình ở cả hai tọa độ đều làm tăng tổng chi phí vì nó làm tăng sự mất cân bằng ở ít nhất một phía của phân phối. Điều này đảm bảo rằng việc chọn các trung vị sẽ mang lại một điểm tối ưu toàn cục trong không gian được chuyển đổi, tương ứng với một ô lục giác tối ưu trong lưới ban đầu. 

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
    
    x_ans = xs[n // 2]
    y_ans = ys[n // 2]
    
    print(x_ans, y_ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách tọa độ thành hai mảng độc lập và sử dụng cách sắp xếp để trích xuất trung vị. Chi tiết triển khai chính là sử dụng n // 2, xử lý chính xác cả trường hợp lẻ và trường hợp chẵn vì mọi trung vị ở phạm vi trung tâm đều hợp lệ. 

Một lỗi phổ biến là cố gắng tính tọa độ trung bình thay vì lấy trung vị. Điều đó hoạt động trong khoảng cách Euclide bình phương nhưng không thành công với các mục tiêu khoảng cách tuyệt đối. Một sai lầm khác là cố gắng mô phỏng trực tiếp khoảng cách hex, điều này không cần thiết và quá chậm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 0
4 4
0 2
```Chúng tôi theo dõi tọa độ được sắp xếp và lựa chọn. 

| Bước | xs | vâng | đã chọn x | đã chọn y | 
| --- | --- | --- | --- | --- | 
| ban đầu | [3, 4, 0] | [0, 4, 2] | - | - | 
| được sắp xếp | [0, 3, 4] | [0, 2, 4] | - | - | 
| trung vị | - | - | 3 | 2 | 

Đầu ra:```
3 2
```Điều này cho thấy giải pháp tự nhiên chọn xu hướng trung tâm của cả hai trục một cách độc lập, cân bằng khoảng cách đến tất cả các điểm. 

### Ví dụ 2 

đầu vào:```
4
0 -2
0 0
0 2
2 0
```| Bước | xs | vâng | đã chọn x | đã chọn y | 
| --- | --- | --- | --- | --- | 
| ban đầu | [0, 0, 0, 2] | [-2, 0, 2, 0] | - | - | 
| được sắp xếp | [0, 0, 0, 2] | [-2, 0, 0, 2] | - | - | 
| trung vị | - | - | 0 | 0 | 

Đầu ra:```
0 0
```Trường hợp này thể hiện việc xử lý các bản phân phối trùng lặp và đối xứng, trong đó tồn tại nhiều giải pháp tối ưu và trung vị vẫn xác định chính xác một trung tâm hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | sắp xếp mảng x và y chiếm ưu thế | 
| Không gian | O(N) | lưu trữ tọa độ | 

Các ràng buộc cho phép lên tới 100.000 điểm và việc sắp xếp ở thang đo này nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và ổn định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    n = int(input())
    xs, ys = [], []
    for _ in range(n):
        x, y = map(int, input().split())
        xs.append(x)
        ys.append(y)
    
    xs.sort()
    ys.sort()
    
    x_ans = xs[n // 2]
    y_ans = ys[n // 2]
    
    return f"{x_ans} {y_ans}"

# provided samples
assert run("""3
3 0
4 4
0 2
""") == "3 2"

assert run("""4
0 -2
0 0
0 2
2 0
""") == "0 0"

# minimum size
assert run("""1
5 7
""") == "5 7"

# all equal x
assert run("""3
10 1
10 5
10 9
""") == "10 5"

# symmetric case
assert run("""5
-2 0
-1 0
0 0
1 0
2 0
""") == "0 0"

# negative coordinates
assert run("""3
-5 -5
-1 -2
-3 -4
""") in ["-3 -4", "-3 -2"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | cùng điểm | trường hợp cơ sở | 
| cùng giá trị x | trung vị đúng y | thoái hóa dọc | 
| đường đối xứng | trung tâm | đối xứng trung tuyến | 
| âm bản hỗn hợp | xử lý ổn định | độ bền của ký hiệu tọa độ | 

## Vỏ cạnh 

Đối với một công nhân, vị trí tối ưu gần như là cùng một ô đó vì bất kỳ chuyển động nào cũng làm tăng khoảng cách. Quy tắc trung vị vẫn trả về phần tử duy nhất trong cả hai mảng, do đó thuật toán xử lý phần tử đó một cách tự nhiên mà không cần viết hoa đặc biệt. 

Đối với trường hợp tất cả công nhân nằm trên cùng một đường thẳng đứng hoặc nằm ngang, bài toán sẽ giảm xuống thành bài toán trung vị 1D. Thuật toán giảm trục chính xác một cách độc lập trong khi vẫn giữ cố định trục kia nên vẫn tạo ra điểm cân bằng chính xác. 

Với N chẵn, có hai trung vị hợp lệ. Cả hai đều có thể chấp nhận được vì việc dịch chuyển trong khoảng trung vị không làm thay đổi tổng độ lệch tuyệt đối. Việc sử dụng n // 2 của thuật toán ngầm chọn một trong số chúng, đủ cho yêu cầu về độ chính xác.
