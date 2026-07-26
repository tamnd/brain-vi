---
title: "CF 102870I - Hình dạng bất thường của gấu trúc Orz"
description: "Bài toán đưa ra các đỉnh của một đa giác không đều theo thứ tự chúng xuất hiện xung quanh ranh giới của nó. Mỗi đỉnh có tọa độ nguyên. Nhiệm vụ là tính diện tích chính xác được bao quanh bởi đa giác này và in kết quả."
date: "2026-07-25T13:18:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102870
codeforces_index: "I"
codeforces_contest_name: "2020-2021 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102870
solve_time_s: 49
verified: true
draft: false
---

[CF 102870I - Hình dạng bất thường của gấu trúc Orz](https://codeforces.com/problemset/problem/102870/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra các đỉnh của một đa giác không đều theo thứ tự chúng xuất hiện xung quanh ranh giới của nó. Mỗi đỉnh có tọa độ nguyên. Nhiệm vụ là tính diện tích chính xác được bao quanh bởi đa giác này và in kết quả. Đầu vào không mô tả hình dạng lưới hoặc tập hợp các ô. Đó là một ranh giới hình học, do đó thứ tự của các điểm rất quan trọng vì mọi điểm đều được kết nối với điểm tiếp theo, với điểm cuối cùng được kết nối trở lại điểm đầu tiên. 

Số lượng đỉnh đủ nhỏ để giải pháp O(n) là hướng dự định. Bất kỳ thuật toán nào thử từng cặp đỉnh, dựng nhiều hình tam giác hoặc thực hiện nhiều lần các phép kiểm tra hình học sẽ tạo thêm công việc không cần thiết. Mặc dù các giá trị tọa độ có thể lớn, nhưng các đỉnh của đa giác là số nguyên, do đó diện tích có thể được biểu diễn chính xác bằng số học số nguyên trước khi chia cuối cùng cho hai. 

Một lỗi phổ biến là sử dụng hình học dấu phẩy động ngay từ đầu. Các tọa độ lớn có thể làm cho việc tích lũy dấu phẩy động mất đi độ chính xác, đặc biệt khi nhiều tích chéo dương và âm triệt tiêu lẫn nhau. Một sai lầm khác là quên rằng đa giác có thể có diện tích phân số tận cùng bằng 0,5. 

Xét một tam giác có các đỉnh:```
3
0 0
1 0
0 1
```Đầu ra đúng là:```
0.5
```Việc triển khai chỉ lưu trữ khu vực dưới dạng số nguyên sẽ xuất ra không chính xác`0`. 

Một trường hợp cạnh khác là đa giác có các điểm được liệt kê theo chiều kim đồng hồ thay vì ngược chiều kim đồng hồ:```
4
0 0
0 1
1 1
1 0
```Vùng đã ký trở nên âm. Diện tích hình học thực tế là dương nên phải lấy giá trị tuyệt đối trước khi định dạng câu trả lời. 

Trường hợp thứ ba là đa giác suy biến:```
3
0 0
1 1
2 2
```Tất cả các điểm đều nằm trên cùng một đường thẳng nên diện tích bao quanh là`0`. Việc triển khai dây giày đúng cách sẽ xử lý được vấn đề này một cách tự nhiên vì tất cả các đóng góp của sản phẩm chéo đều bị hủy. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là chia đa giác thành các hình tam giác. Người ta có thể chọn đỉnh đầu tiên làm điểm cố định và tính diện tích của mọi tam giác tạo bởi đỉnh đó và hai đỉnh lân cận. Điều này đúng vì bất kỳ đa giác đơn giản nào cũng có thể được chia thành các hình tam giác. Tuy nhiên, việc triển khai bất cẩn có thể liên tục tính toán các mối quan hệ hình học hoặc tìm kiếm các đường chéo hợp lệ, tạo ra sự phức tạp không cần thiết. 

Quan sát quan trọng là diện tích đa giác không yêu cầu xây dựng các hình tam giác một cách rõ ràng. Công thức dây giày trực tiếp chuyển đổi ranh giới thành tổng các tích chéo. Đối với mọi cạnh từ`(x_i, y_i)`ĐẾN`(x_{i+1}, y_{i+1})`, chúng tôi thêm`x_i * y_{i+1} - x_{i+1} * y_i`. Giá trị tuyệt đối cuối cùng của tổng này gấp đôi diện tích đa giác. 

Công thức này hoạt động vì mỗi cạnh đều đóng góp diện tích có dấu của một tam giác có gốc tọa độ. Khi tất cả các cạnh biên được kết hợp, các vùng bên ngoài sẽ bị hủy bỏ và chỉ còn lại phần bên trong của đa giác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng tam giác Brute Force | O(n²) | O(1) | Quá chậm và không cần thiết | 
| Công thức dây giày | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các đỉnh đa giác theo thứ tự đã cho. Thứ tự phải được giữ nguyên vì các cạnh của đa giác được xác định bởi các đỉnh liên tiếp. 
2. Lặp lại qua từng đỉnh và ghép nó với đỉnh tiếp theo trong chu trình. Đối với đỉnh cuối cùng, đỉnh tiếp theo là đỉnh đầu tiên. Thêm đóng góp sản phẩm chéo`x_i * y_next - x_next * y_i`đến một tổng số đang chạy. 
3. Lấy giá trị tuyệt đối của số tiền tích lũy. Dấu hiệu chỉ cho biết các đỉnh được liệt kê theo chiều kim đồng hồ hay ngược chiều kim đồng hồ. 
4. Định dạng kết quả bằng cách chia diện tích nhân đôi cho hai. Nếu giá trị là số lẻ thì khu vực đó có`.5`phần phân số. 

Tại sao nó hoạt động: Tổng dây giày chính xác gấp đôi diện tích có dấu của một đa giác. Mỗi cạnh có hướng đóng góp một diện tích tam giác so với gốc tọa độ. Việc thêm tất cả những đóng góp đã ký này sẽ loại bỏ các khu vực bên ngoài đa giác và giữ lại phần bên trong. Việc lấy giá trị tuyệt đối sẽ loại bỏ sự phụ thuộc vào hướng truyền tải, do đó thuật toán luôn trả về diện tích hình học thực. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return

    it = iter(data)
    n = int(next(it))

    points = []
    for _ in range(n):
        x = int(next(it))
        y = int(next(it))
        points.append((x, y))

    area2 = 0

    for i in range(n):
        x1, y1 = points[i]
        x2, y2 = points[(i + 1) % n]
        area2 += x1 * y2 - x2 * y1

    area2 = abs(area2)

    if area2 % 2 == 0:
        print(area2 // 2)
    else:
        print(str(area2 // 2) + ".5")

if __name__ == "__main__":
    solve()
```Phần đầu vào lưu trữ các đỉnh vì công thức dây giày yêu cầu thứ tự tuần hoàn. Việc giữ các điểm trong danh sách cũng làm cho cạnh bao quanh từ đỉnh cuối cùng trở lại đỉnh đầu tiên trở nên đơn giản và tránh các lỗi biên. 

Vòng lặp chính tính toán diện tích đã ký nhân đôi. Số nguyên Python không bị tràn, điều này rất hữu ích ở đây vì phép nhân tọa độ có thể lớn hơn nhiều so với phạm vi tọa độ ban đầu. 

Logic đầu ra tránh hoàn toàn dấu phẩy động. Vì giá trị được tính bằng hai lần diện tích nên giá trị chẵn biểu thị diện tích nguyên và giá trị lẻ biểu thị diện tích kết thúc bằng`.5`. 

## Ví dụ đã hoạt động 

Hãy xem xét đa giác:```
4
0 0
2 0
2 2
0 2
```| Bước | Cạnh hiện tại | Đóng góp | Khu vực chạy2 | 
| --- | --- | --- | --- | 
| 1 | (0,0) đến (2,0) | 0 | 0 | 
| 2 | (2,0) đến (2,2) | 4 | 4 | 
| 3 | (2,2) đến (0,2) | 4 | 8 | 
| 4 | (0,2) đến (0,0) | 0 | 8 | 

Diện tích nhân đôi cuối cùng là`8`, vậy diện tích đa giác là`4`. Điều này thể hiện trường hợp ngược chiều kim đồng hồ thông thường trong đó mọi đóng góp đều dễ diễn giải. 

Hãy xem xét đa giác:```
3
0 0
1 0
0 1
```| Bước | Cạnh hiện tại | Đóng góp | Khu vực chạy2 | 
| --- | --- | --- | --- | 
| 1 | (0,0) đến (1,0) | 0 | 0 | 
| 2 | (1,0) đến (0,1) | 1 | 1 | 
| 3 | (0,1) đến (0,0) | 0 | 1 | 

Diện tích nhân đôi là`1`, vậy câu trả lời là`0.5`. Điều này cho thấy tại sao giải pháp giữ diện tích nhân đôi và chỉ định dạng phân số ở cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh tham gia vào một phép tính tích chéo | 
| Không gian | O(n) | Các đỉnh được lưu trữ để duy trì trật tự tuần hoàn | 

Thời gian chạy tuyến tính dễ dàng phù hợp với các ràng buộc dự định vì thuật toán chỉ thực hiện một lượng số học không đổi trên mỗi đỉnh. Việc sử dụng bộ nhớ cũng nhỏ vì dữ liệu được lưu trữ duy nhất là ranh giới đa giác. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# helper solution copy
def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return

    it = iter(data)
    n = int(next(it))
    points = [(int(next(it)), int(next(it))) for _ in range(n)]

    area2 = 0
    for i in range(n):
        x1, y1 = points[i]
        x2, y2 = points[(i + 1) % n]
        area2 += x1 * y2 - x2 * y1

    area2 = abs(area2)

    if area2 % 2 == 0:
        print(area2 // 2)
    else:
        print(str(area2 // 2) + ".5")

assert run("""3
0 0
1 0
0 1
""") == "0.5\n", "triangle fraction"

assert run("""4
0 0
2 0
2 2
0 2
""") == "4\n", "square"

assert run("""3
0 0
1 1
2 2
""") == "0\n", "degenerate polygon"

assert run("""4
0 0
0 1
1 1
1 0
""") == "1\n", "clockwise ordering"

assert run("""5
0 0
4 0
5 2
2 4
0 3
""") == "15\n", "irregular polygon"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác có đỉnh`(0,0),(1,0),(0,1)`| 0,5 | Xử lý diện tích phân số | 
| Hình vuông có cạnh dài 2 | 4 | Tính đa giác chuẩn | 
| Ba điểm thẳng hàng | 0 | Trường hợp thoái hóa | 
| Hình vuông theo chiều kim đồng hồ | 1 | Xử lý giá trị tuyệt đối | 
| Đa giác năm cạnh không đều | 15 | Tính toán dây giày chung | 

## Vỏ cạnh 

Đối với trường hợp tam giác phân số:```
3
0 0
1 0
0 1
```Thuật toán tính diện tích gấp đôi của`1`. Vì nó là số lẻ nên nó in`0.5`. Không cần tính toán dấu phẩy động. 

Đối với thứ tự đỉnh đảo ngược:```
4
0 0
0 1
1 1
1 0
```Tổng dây giày trở nên âm vì đường đi theo chiều kim đồng hồ. Giá trị tuyệt đối thay đổi diện tích nhân đôi từ`-2`ĐẾN`2`, đưa ra câu trả lời đúng`1`. 

Đối với đa giác suy biến:```
3
0 0
1 1
2 2
```Sự đóng góp sản phẩm chéo triệt tiêu lẫn nhau. Diện tích nhân đôi cuối cùng là`0`, do đó thuật toán in chính xác`0`. 

Đối với tọa độ lớn, mọi phép nhân được thực hiện bằng số học số nguyên. Python tự động xử lý độ chính xác cần thiết, do đó không có lỗi làm tròn nào có thể xuất hiện từ các phép tính trung gian. 

Tôi cũng có thể cung cấp một phiên bản biên tập ngắn hơn theo phong cách cuộc thi nếu bạn muốn một phiên bản gần giống với những gì sẽ xuất hiện trên trang hướng dẫn Codeforces.
