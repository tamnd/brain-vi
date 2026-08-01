---
title: "CF 102620H - Tảng băng trôi"
description: "Bài toán mô tả một tập hợp các tảng băng trôi, trong đó mỗi tảng băng trôi được biểu diễn dưới dạng một đa giác đơn giản thông qua tọa độ các điểm biên của nó. Nhiệm vụ là tính tổng diện tích được bao phủ bởi tất cả các tảng băng trôi và chỉ xuất ra phần nguyên của diện tích đó."
date: "2026-07-31T03:31:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102620
codeforces_index: "H"
codeforces_contest_name: "mBIT Standard June 2020"
rating: 0
weight: 102620
solve_time_s: 97
verified: true
draft: false
---

[CF 102620H - Tảng băng trôi](https://codeforces.com/problemset/problem/102620/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bài toán mô tả một tập hợp các tảng băng trôi, trong đó mỗi tảng băng trôi được biểu diễn dưới dạng một đa giác đơn giản thông qua tọa độ các điểm biên của nó. Nhiệm vụ là tính tổng diện tích được bao phủ bởi tất cả các tảng băng trôi và chỉ xuất ra phần nguyên của diện tích đó. Các đa giác không chồng lên nhau, vì vậy các khu vực riêng lẻ của chúng có thể được cộng lại với nhau một cách đơn giản. Đầu vào chứa một số mô tả đa giác độc lập và đầu ra là tổng diện tích của chúng được làm tròn xuống. 

Số lượng đa giác có thể lên tới 1000 và mỗi đa giác có tối đa 50 đỉnh. Điều này có nghĩa là tổng số đỉnh nhiều nhất là 50000, do đó, một thuật toán xử lý mỗi đỉnh với số lần không đổi là đủ nhanh. Phương pháp so sánh từng cặp cạnh của mọi đa giác vẫn có tác dụng đối với các giới hạn này trong một số trường hợp, nhưng không cần thiết. Cách tiếp cận dự định phải tuyến tính về số đỉnh vì hình học có công thức trực tiếp. 

Các trường hợp cạnh chính đến từ hướng đa giác và diện tích phân số. Một đa giác có thể được liệt kê theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, do đó, việc áp dụng trực tiếp công thức diện tích đã ký mà không quan tâm đến dấu có thể tạo ra kết quả âm. Ví dụ:```
1
4
0 0
0 1
1 1
1 0
```Đầu ra đúng là:```
1
```Việc thực hiện bất cẩn có thể tính toán diện tích đã ký của`-1`và in sai một giá trị âm. 

Một trường hợp khác là khi diện tích thực không phải là số nguyên. Ví dụ:```
1
3
0 0
1 0
0 1
```Diện tích thực tế là`0.5`, vì vậy đầu ra cần thiết là:```
0
```Câu trả lời không được làm tròn đến số nguyên gần nhất. Phần phân số phải luôn được loại bỏ. 

Một lỗi phổ biến cuối cùng là quên rằng đỉnh cuối cùng kết nối ngược lại với đỉnh đầu tiên. Đối với đầu vào này:```
1
4
0 0
2 0
2 2
0 2
```đầu ra là:```
4
```Việc bỏ qua cạnh đóng sẽ để lại một đa giác không hoàn chỉnh và đưa ra vùng sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là chia mỗi đa giác thành nhiều phần nhỏ hoặc cố gắng mô phỏng phần bên trong của hình trên một lưới. Điều này là không cần thiết vì tọa độ rất chính xác và ranh giới đa giác đã chứa đủ thông tin. Giải pháp dựa trên lưới sẽ phụ thuộc vào phạm vi tọa độ, có thể lên tới một triệu, khiến số lượng ô có thể có quá lớn. 

Quan sát quan trọng là diện tích đa giác có thể được tính trực tiếp từ ranh giới của nó bằng công thức dây giày. Mỗi cặp đỉnh liên tiếp đóng góp một giá trị có dấu đại diện cho diện tích của một tam giác được tạo thành từ gốc tọa độ. Khi tất cả các đóng góp được tính tổng, các vùng được ký chồng chéo sẽ hủy bỏ một cách chính xác, để lại chính xác diện tích của đa giác. 

Phương pháp vũ lực hoạt động vì nó cố gắng tái tạo lại bên trong tảng băng trôi, nhưng nó bỏ qua thực tế là ranh giới đã mô tả hoàn toàn hình dạng. Nhận xét rằng diện tích chỉ phụ thuộc vào các cạnh có thứ tự cho phép chúng ta rút gọn bài toán thành một lần duyệt trên tất cả các đỉnh. 

Đối với đa giác có đỉnh`(x1, y1), (x2, y2), ... , (xp, yp)`, diện tích kép có dấu là:$$S = \sum_{i=1}^{p}(x_i y_{i+1} - x_{i+1} y_i)$$trong đó đỉnh tiếp theo sau đỉnh cuối cùng là đỉnh đầu tiên. Diện tích thực tế là`abs(S) / 2`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(diện tích phạm vi tọa độ) | O(1) | Quá chậm | 
| Công thức dây giày | O(tổng số đỉnh) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đa giác. Đối với mỗi đa giác, hãy lưu trữ đỉnh đầu tiên vì cạnh cuối cùng phải kết nối trở lại với nó. 
2. Đi qua mọi đỉnh và cộng tích chéo giữa đỉnh hiện tại và đỉnh tiếp theo. Sự đóng góp là`x1*y2 - x2*y1`. 
3. Sau khi xử lý tất cả các cạnh, lấy giá trị tuyệt đối của giá trị tích lũy vì thứ tự đầu vào có thể mô tả đa giác theo một trong hai hướng. 
4. Cộng diện tích nhân đôi vào tổng toàn cầu. Giữ mọi thứ được nhân đôi sẽ tránh được lỗi dấu phẩy động trong khi xử lý hình học. 
5. Sau khi tất cả các đa giác được xử lý, chia tổng diện tích nhân đôi cho hai bằng phép chia số nguyên. Điều này sẽ tự động loại bỏ phần phân số theo yêu cầu. 

Tại sao nó hoạt động: 

Công thức dây giày dựa trên việc phân tách đa giác thành các hình tam giác có dấu. Nếu các đỉnh được sắp xếp ngược chiều kim đồng hồ thì tất cả các hình tam giác đều đóng góp tích cực. Nếu chúng được sắp xếp theo chiều kim đồng hồ thì mọi đóng góp đều âm. Lấy giá trị tuyệt đối sẽ khôi phục diện tích thực gấp đôi. Vì các đa giác không chồng lên nhau nên việc cộng diện tích gấp đôi của tất cả các đa giác sẽ mang lại chính xác gấp đôi tổng diện tích được bao phủ. Chia số nguyên cho hai sẽ cho giá trị sàn yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    total2 = 0

    for _ in range(n):
        p = int(input())
        points = []

        for _ in range(p):
            x, y = map(int, input().split())
            points.append((x, y))

        area2 = 0
        for i in range(p):
            x1, y1 = points[i]
            x2, y2 = points[(i + 1) % p]
            area2 += x1 * y2 - x2 * y1

        total2 += abs(area2)

    print(total2 // 2)

if __name__ == "__main__":
    solve()
```Đầu vào được xử lý đa giác theo đa giác vì chỉ cần có ranh giới hiện tại. Danh sách các điểm được lưu trữ tạm thời để đỉnh cuối cùng có thể được kết nối trở lại đỉnh đầu tiên. 

biểu thức`(i + 1) % p`xử lý cạnh đóng. Nếu không có phép toán modulo, cạnh cuối cùng của mọi đa giác sẽ bị thiếu. 

Việc tính toán sử dụng diện tích nhân đôi thay vì giá trị dấu phẩy động. Tọa độ có thể lớn tới một triệu, do đó tích chéo có thể lớn, nhưng số nguyên Python tự động mở rộng để giữ các giá trị cần thiết. Giá trị tuyệt đối được áp dụng cho mỗi đa giác trước khi thêm vào câu trả lời cuối cùng vì mỗi đa giác có thể có hướng riêng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1
4
0 0
1 0
1 1
0 1
```| Bước | Cạnh hiện tại | Đóng góp | Tích lũy gấp đôi diện tích | 
| --- | --- | --- | --- | 
| 1 | (0,0) đến (1,0) | 0 | 0 | 
| 2 | (1,0) đến (1,1) | 1 | 1 | 
| 3 | (1,1) đến (0,1) | 1 | 2 | 
| 4 | (0,1) đến (0,0) | 0 | 2 | 

Diện tích nhân đôi là`2`, vậy diện tích thực là`1`. Điều này chứng tỏ trường hợp ngược chiều kim đồng hồ bình thường. 

Đối với mẫu thứ hai:```
2
5
98 35
79 90
21 90
2 36
50 0
3
0 0
20 0
0 20
```Hai đa giác tạo ra: 

| Đa giác | Diện tích nhân đôi | Khu vực | 
| --- | --- | --- | 
| Lầu Năm Góc | 11801 | 5900,5 | 
| Tam giác | 400 | 200 | 

Tổng diện tích nhân đôi là`12201`. Chia cho hai được`6100`, phù hợp với hoạt động sàn yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V) | Mỗi đỉnh từ mọi đa giác được xử lý một lần | 
| Không gian | O(P) | Chỉ các đỉnh của đa giác hiện tại được lưu trữ | 

Tổng số đỉnh tối đa đủ nhỏ để quét tuyến tính dễ dàng nằm gọn trong giới hạn. Thuật toán chỉ thực hiện các phép tính số học đơn giản và tránh mọi sự phụ thuộc vào độ lớn tọa độ. 

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

assert run("""1
4
0 0
1 0
1 1
0 1
""") == "1\n", "sample 1"

assert run("""2
5
98 35
79 90
21 90
2 36
50 0
3
0 0
20 0
0 20
""") == "6100\n", "sample 2"

assert run("""1
3
0 0
1 0
0 1
""") == "0\n", "half area"

assert run("""1
4
0 0
0 1
1 1
1 0
""") == "1\n", "clockwise polygon"

assert run("""1
4
0 0
1000000 0
1000000 1000000
0 1000000
""") == "1000000000000\n", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn vị vuông | 1 | Tính dây giày cơ bản | 
| Lầu Năm Góc cộng với tam giác | 6100 | Nhiều đa giác và sàn | 
| Tam giác vuông | 0 | Xử lý diện tích phân số | 
| Hình vuông theo chiều kim đồng hồ | 1 | Định hướng độc lập | 
| Hình vuông lớn | 1000000000000 | Số học số nguyên lớn | 

## Vỏ cạnh 

Đa giác theo chiều kim đồng hồ được xử lý bằng cách lấy giá trị tuyệt đối của vùng đã ký. Vì:```
1
4
0 0
0 1
1 1
1 0
```diện tích tích lũy gấp đôi là`-2`. Thuật toán chuyển đổi nó thành`2`, sau đó chia cho hai để có được`1`. 

Một đa giác có diện tích phân số giữ giá trị gấp đôi cho đến thao tác cuối cùng. Vì:```
1
3
0 0
1 0
0 1
```diện tích nhân đôi là`1`. Thuật toán tính toán`1 // 2`, sản xuất`0`, phù hợp với hành vi sàn được yêu cầu. 

Cạnh đóng được bao gồm bằng cách sử dụng đỉnh đầu tiên sau đỉnh cuối cùng. Vì:```
1
4
0 0
2 0
2 2
0 2
```đóng góp cuối cùng từ`(0,2)`quay lại`(0,0)`được bao gồm, cho diện tích gấp đôi`8`và câu trả lời cuối cùng`4`.
