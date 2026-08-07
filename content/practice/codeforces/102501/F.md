---
title: "CF 102501F - Tảng băng trôi"
description: "Chỉnh sửa Chúng ta được cung cấp một số tảng băng trôi, trong đó mỗi tảng băng trôi được mô tả bằng danh sách có thứ tự các điểm trên đường viền của nó. Các điểm tạo thành một đa giác đơn giản, có nghĩa là đường viền không bao giờ tự vượt qua."
date: "2026-08-06T18:51:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "F"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 65
verified: true
draft: false
---

[CF 102501F - Tảng băng trôi](https://codeforces.com/problemset/problem/102501/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
Chỉnh sửa 

#Hiểu vấn đề 

Chúng ta được cho một số tảng băng trôi, trong đó mỗi tảng băng trôi được mô tả bằng danh sách có thứ tự các điểm trên đường biên của nó. Các điểm tạo thành một đa giác đơn giản, có nghĩa là đường viền không bao giờ tự vượt qua. Nhiệm vụ là tìm diện tích bề mặt tổng hợp của tất cả các đa giác và in phần nguyên của diện tích đó, nghĩa là làm tròn xuống. 

Kích thước đầu vào được thiết kế dựa trên thực tế là tổng lượng dữ liệu hình học nhỏ. Có thể có tới 1000 đa giác và mỗi đa giác có tối đa 50 đỉnh, do đó tổng số đỉnh tối đa là 50000. Điều này loại trừ các thuật toán so sánh nhiều lần mọi cặp đỉnh hoặc quét một lưới tọa độ lớn. Giải pháp dựa trên lưới là không thể vì tọa độ có thể lớn tới 10^6, tạo ra diện tích 10^12 ô cho một hướng. Giải pháp dự định cần xử lý mỗi đỉnh một số lần không đổi. 

Các trường hợp cạnh chính xuất phát từ cách thể hiện các khu vực đa giác. Một đa giác có thể được liệt kê theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ và cả hai mô tả đều thể hiện cùng một tảng băng trôi vật lý. Việc thực hiện bất cẩn theo một hướng có thể tạo ra câu trả lời tiêu cực. 

Ví dụ:```
1
4
0 0
0 1
1 1
1 0
```Đầu ra đúng là:```
1
```Các điểm được tính theo chiều kim đồng hồ nên diện tích được đánh dấu từ công thức dây giày là âm. Lấy giá trị mà không áp dụng giá trị tuyệt đối sẽ tạo ra kết quả âm không chính xác. 

Một trường hợp cạnh khác là đa giác có diện tích không phải là số nguyên.```
1
3
0 0
1 0
0 1
```Đầu ra đúng là:```
0
```Tam giác có diện tích 0,5 và thao tác cần thực hiện là loại bỏ phần phân số. Làm tròn đến số nguyên gần nhất sẽ cho một câu trả lời khác, vì vậy việc triển khai phải giữ đúng diện tích gấp đôi. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là cố gắng đo trực tiếp bên trong của mỗi đa giác. Một phương pháp khả thi là tạo một lưới gồm tất cả các tọa độ bên trong hình chữ nhật bao quanh và kiểm tra từng ô. Điều này hoạt động vì tọa độ xác định một mặt phẳng, nhưng nó ngay lập tức thất bại đối với vấn đề này. Một đa giác trải dài từ 0 đến 10^6 ở cả hai chiều sẽ yêu cầu kiểm tra khoảng 10^12 ô, vượt xa thời gian cho phép. 

Một quan sát hình học tốt hơn là diện tích đa giác không phụ thuộc vào hình dạng riêng của phần bên trong nó. Các điểm biên giới chứa đủ thông tin. Bằng cách nối mọi đỉnh với một điểm gốc cố định, đa giác có thể được chia thành các hình tam giác. Phần đóng góp diện tích có dấu của mỗi cặp đỉnh liên tiếp được tính bằng cách sử dụng tích chéo: 

[ 
x_i y_{i+1} - y_i x_{i+1} 
] 

Tổng của những đóng góp này gấp đôi diện tích có dấu của đa giác. Dấu hiệu chỉ cho chúng ta biết hướng sắp xếp của các đỉnh, do đó, lấy giá trị tuyệt đối sẽ có diện tích thực tế tăng gấp đôi. 

Phương pháp tiếp cận bạo lực hoạt động hiệu quả vì nó mô hình hóa trực tiếp khu vực được bao phủ bởi băng, nhưng nó bỏ qua lượng thông tin nhỏ hơn nhiều được lưu trữ trong ranh giới. Việc quan sát thấy diện tích đa giác có thể được phục hồi từ các điểm biên giúp giảm vấn đề từ việc phụ thuộc vào kích thước tọa độ xuống chỉ phụ thuộc vào số đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(diện tích khung giới hạn) | O(diện tích khung giới hạn) | Quá chậm | 
| Công thức dây giày | O(tổng số đỉnh) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mọi đa giác và lưu trữ các đỉnh của nó theo thứ tự. Thứ tự quan trọng vì công thức dây giày sử dụng các cặp điểm lân cận xung quanh đường biên. 
2. Với mọi cạnh tính từ đỉnh`i`đến đỉnh`i + 1`, thêm phần đóng góp của sản phẩm chéo`x_i * y_{i+1} - y_i * x_{i+1}`đến diện tích được ký gấp đôi của đa giác. 
3. Lấy giá trị tuyệt đối của giá trị tích lũy cho đa giác. Điều này loại bỏ ảnh hưởng của việc các đỉnh được cho theo chiều kim đồng hồ hay ngược chiều kim đồng hồ. 
4. Cộng diện tích nhân đôi của mỗi đa giác vào tổng diện tích toàn cầu. Các tảng băng trôi không chồng lên nhau nên diện tích của chúng có thể được tóm tắt một cách đơn giản. 
5. Chia diện tích nhân đôi cuối cùng cho hai bằng phép chia số nguyên. Điều này mang lại giá trị cần thiết khi loại bỏ phần phân số. 

Tại sao nó hoạt động: 

Công thức dây giày được suy ra bằng cách tính tổng diện tích có dấu của các hình tam giác được hình thành bởi các cạnh đa giác liên tiếp và gốc tọa độ. Mỗi tam giác bên trong đa giác đóng góp chính xác một lần, trong khi các vùng bên ngoài được tạo bởi gốc tọa độ sẽ triệt tiêu các dấu đối diện. Vì đa giác đơn giản nên kết quả chính xác là diện tích có dấu của đa giác. Việc lấy giá trị tuyệt đối sẽ cho diện tích thực và tính tổng các diện tích nhân đôi sẽ duy trì độ chính xác cho đến lần cắt cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    total_twice = 0

    for _ in range(n):
        p = int(input())
        points = []
        for _ in range(p):
            x, y = map(int, input().split())
            points.append((x, y))

        twice = 0
        for i in range(p):
            x1, y1 = points[i]
            x2, y2 = points[(i + 1) % p]
            twice += x1 * y2 - y1 * x2

        total_twice += abs(twice)

    print(total_twice // 2)

if __name__ == "__main__":
    solve()
```Đầu vào được xử lý đa giác theo đa giác vì mỗi tảng băng trôi là độc lập. Danh sách các đỉnh được lưu trữ tạm thời vì đỉnh cuối cùng phải kết nối trở lại đỉnh đầu tiên khi tính cạnh cuối cùng. 

Vòng lặp trên các cạnh sử dụng`(i + 1) % p`để xử lý cạnh đóng của đa giác. Nếu không có phép toán modulo, cạnh từ điểm cuối cùng trở lại điểm đầu tiên sẽ bị bỏ sót, đây là nguyên nhân phổ biến dẫn đến các câu trả lời sai. 

Việc tính toán được thực hiện với diện tích nhân đôi thay vì số dấu phẩy động. Vì tất cả tọa độ đều là số nguyên nên mọi đóng góp của dây giày cũng là số nguyên. Việc giữ chính xác giá trị này sẽ tránh được các lỗi chính xác và làm cho phép toán tầng cuối cùng chỉ là một phép chia số nguyên. 

Số nguyên Python tự động tăng vượt quá kích thước máy cố định, điều này rất hữu ích ở đây vì tổng tích số chéo có thể vào khoảng 10^16. 

## Ví dụ đã hoạt động 

Đối với hình vuông đơn vị:```
1
4
0 0
1 0
1 1
0 1
```Việc tính toán là: 

| Cạnh | Đóng góp chéo sản phẩm | Diện tích chạy tăng gấp đôi | 
| --- | --- | --- | 
| (0,0) đến (1,0) | 0 | 0 | 
| (1,0) đến (1,1) | 1 | 1 | 
| (1,1) đến (0,1) | 1 | 2 | 
| (0,1) đến (0,0) | 0 | 2 | 

Diện tích nhân đôi là 2, do đó diện tích thực là 1. Ví dụ xác nhận rằng thuật toán xử lý một đa giác cơ bản và phép chia cuối cùng một cách chính xác. 

Đối với tam giác vuông:```
1
3
0 0
20 0
0 20
```Việc tính toán là: 

| Cạnh | Đóng góp chéo sản phẩm | Diện tích chạy tăng gấp đôi | 
| --- | --- | --- | 
| (0,0) đến (20,0) | 0 | 0 | 
| (20,0) đến (0,20) | 400 | 400 | 
| (0,20) đến (0,0) | 0 | 400 | 

Diện tích nhân đôi là 400, cho diện tích 200. Ví dụ cho thấy cách phương pháp xử lý các hình không phải hình chữ nhật mà không cần kiểm tra bên trong đa giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V) | Mỗi đỉnh được sử dụng trong một tích chéo, trong đó V là tổng số đỉnh trên tất cả các tảng băng trôi. | 
| Không gian | O(P) | Chỉ các đỉnh của đa giác hiện tại được lưu trữ. | 

Số đỉnh tối đa là 50000, do đó nghiệm tuyến tính chỉ thực hiện một số lượng nhỏ các phép toán số học và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(data):
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)
    solve()
    sys.stdin = old_stdin

def run(inp: str) -> str:
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve_data(inp)
    result = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return result

assert run("""1
4
0 0
1 0
1 1
0 1
""") == "1\n", "sample style square"

assert run("""2
3
0 0
20 0
0 20
4
0 0
2 0
2 2
0 2
""") == "202\n", "multiple polygons"

assert run("""1
3
0 0
1 0
0 1
""") == "0\n", "fractional area"

assert run("""1
4
0 0
0 1000000
1000000 1000000
1000000 0
""") == "1000000000000\n", "large coordinates"

assert run("""1
3
0 0
0 5
5 0
""") == "12\n", "clockwise orientation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn vị vuông | 1 | Tính toán dây giày cơ bản | 
| Hai đa giác | 202 | Tích lũy diện tích độc lập | 
| Nửa đơn vị tam giác | 0 | Cắt đúng diện tích phân đoạn | 
| Hình vuông lớn | 1000000000000 | Xử lý tọa độ lớn | 
| Tam giác theo chiều kim đồng hồ | 12 | Định hướng độc lập | 

## Vỏ cạnh 

Một đa giác theo chiều kim đồng hồ tạo ra tảng băng vật lý giống như một đa giác ngược chiều kim đồng hồ, nhưng tổng dây giày thay đổi dấu. Đối với đầu vào:```
1
4
0 0
0 1
1 1
1 0
```diện tích tích lũy gấp đôi là`-2`. Thuật toán chuyển đổi nó thành`2`trước khi thêm nó vào câu trả lời, tạo ra kết quả chính xác`1`. 

Một đa giác có diện tích phân số không được làm tròn. Vì:```
1
3
0 0
1 0
0 1
```diện tích nhân đôi là`1`, do đó phép chia số nguyên cho`1 // 2 = 0`. Thuật toán tuân theo hành vi sàn được yêu cầu một cách chính xác. 

Cạnh cuối cùng của đa giác là một trường hợp tinh tế khác. Vì:```
1
3
0 0
2 0
0 2
```cạnh cuối cùng là từ`(0,2)`quay lại`(0,0)`. Phép toán modulo bao gồm cạnh này, tạo ra diện tích gấp đôi`4`và đầu ra`2`. Việc bỏ qua cạnh này sẽ cho kết quả sai mặc dù tất cả các điểm được liệt kê đều đã được xử lý.
