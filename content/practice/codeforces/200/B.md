---
title: "CF 200B - Đồ uống"
description: "Chúng tôi được cho uống nhiều đồ uống, và mỗi đồ uống đều chứa một ít nước cam. Vasya trộn một lượng bằng nhau mỗi đồ uống vào một loại cocktail duy nhất. Câu hỏi rất đơn giản: sau khi trộn chúng, bao nhiêu phần trăm trong ly cocktail cuối cùng là nước cam?"
date: "2026-06-03T16:24:54+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 200
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 126 (Div. 2)"
rating: 800
weight: 200
solve_time_s: 128
verified: false
draft: false
---

[CF 200B - Đồ uống](https://codeforces.com/problemset/problem/200/B) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cho uống nhiều đồ uống, và mỗi đồ uống đều chứa một ít nước cam. Vasya trộn một lượng bằng nhau mỗi đồ uống vào một loại cocktail duy nhất. 

Câu hỏi rất đơn giản: sau khi trộn chúng, bao nhiêu phần trăm trong ly cocktail cuối cùng là nước cam? 

Dữ liệu đầu vào chứa số lượng đồ uống và phần trăm nước cam của mỗi đồ uống. Kết quả đầu ra là phần trăm nước cam trong hỗn hợp. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 100 đồ uống nên bất kỳ thuật toán nào xử lý từng phần trăm một lần sẽ kết thúc ngay lập tức. Ngay cả một mô phỏng phức tạp không cần thiết vẫn sẽ vừa vặn thoải mái trong giới hạn. Điều này có nghĩa là thách thức không phải là hiệu quả thuật toán mà là hiểu chính xác cách trộn số lượng bằng nhau ảnh hưởng đến tỷ lệ phần trăm. 

Một sai lầm phổ biến là nghĩ về khối lượng thực tế và mô phỏng chúng. Vấn đề không cho chúng ta biết mỗi lần uống bao nhiêu ml. May mắn thay, giá trị đó không thành vấn đề vì mỗi lần uống đều có cùng một lượng. Tỷ lệ phần trăm cuối cùng chỉ đơn giản là mức trung bình của các tỷ lệ phần trăm riêng lẻ. 

Một trường hợp tinh tế khác xảy ra khi một số đồ uống không chứa nước cam. 

đầu vào:```
2
0 100
```Đầu ra:```
50
```Việc triển khai bất cẩn bỏ qua giá trị 0 hoặc chia cho số lượng sai có thể tạo ra kết quả không chính xác. Hỗn hợp này chứa một lượng bằng nhau giữa đồ uống nước cam nguyên chất và đồ uống không có nước cam, vì vậy mức trung bình chính xác là 50%. 

Một trường hợp khác là khi mỗi đồ uống đều chứa 0% nước cam. 

đầu vào:```
3
0 0 0
```Đầu ra:```
0
```Câu trả lời vẫn hợp lệ vì tính trung bình các số 0 vẫn cho kết quả bằng 0. 

Tương tự, nếu mỗi đồ uống có chứa 100% nước cam: 

đầu vào:```
4
100 100 100 100
```Đầu ra:```
100
```Hỗn hợp vẫn là nước cam nguyên chất. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp mang tính bạo lực là cho rằng Vasya uống một lượng nào đó, chẳng hạn như 100 ml, từ mỗi lần uống rượu. Chúng ta có thể tính xem mỗi đồ uống có bao nhiêu ml nước cam, tính tổng các lượng đó, tính tổng thể tích của ly cocktail và cuối cùng là chia. 

Đối với đồ uống có tỷ lệ phần trăm`p`, lấy 100 ml đóng góp`p`ml nước cam. Sau khi chế biến tất cả đồ uống, tỷ lệ nước cam là$$\frac{\text{total orange juice}}{\text{total volume}} \times 100.$$Cách tiếp cận này đúng vì nó tuân theo chính xác quy trình trộn vật lý. Độ phức tạp thời gian của nó là$O(n)$, vì mỗi đồ uống được xử lý một lần. 

Nhìn kỹ vào công thức sẽ thấy điều gì đó đơn giản hơn. Nếu cùng một lượng được lấy từ mỗi lần uống, số tiền đó sẽ xuất hiện ở cả tử số và mẫu số và bị loại bỏ. Những gì còn lại chỉ là giá trị trung bình số học của tất cả các tỷ lệ phần trăm:$$\frac{p_1 + p_2 + \cdots + p_n}{n}.$$Vấn đề ngay lập tức trở thành một nhiệm vụ trung bình. Chúng ta chỉ cần tổng của tất cả các tỷ lệ phần trăm rồi chia cho số lượng đồ uống. 

Mô phỏng lực lượng vũ phu đã chạy đủ nhanh cho các ràng buộc nhất định, nhưng quan sát trung bình sẽ loại bỏ các phép tính không cần thiết và thể hiện trực tiếp toán học cơ bản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số lượng đồ uống. 
2. Đọc`n`các giá trị phần trăm. 
3. Tính tổng của tất cả các tỷ lệ phần trăm. 
4. Chia tổng cho`n`để có được tỷ lệ phần trăm trung bình. 
5. In kết quả dưới dạng số dấu phẩy động. 

Quan sát toán học duy nhất là các phần bằng nhau được lấy từ mỗi đồ uống. Vì mỗi đồ uống đóng góp một thể tích như nhau nên nồng độ cuối cùng bằng giá trị trung bình của nồng độ ban đầu. 

### Tại sao nó hoạt động 

Giả sử Vasya lấy`x`ml từ mỗi đồ uống. 

Uống`i`đóng góp$$x \cdot \frac{p_i}{100}$$ml nước cam nguyên chất. 

Tổng số nước cam là$$x \cdot \frac{p_1+p_2+\cdots+p_n}{100}.$$Tổng khối lượng cocktail là$$nx.$$Tỷ lệ phần trăm cuối cùng là$$\frac{x \cdot \frac{p_1+p_2+\cdots+p_n}{100}}{nx}\times100 = \frac{p_1+p_2+\cdots+p_n}{n}.$$Thuật toán tính toán chính xác đại lượng này nên luôn trả về tỷ lệ phần trăm nước cam chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
p = list(map(int, input().split()))

answer = sum(p) / n
print(answer)
```Dòng đầu tiên ghi số lượng đồ uống. Dòng thứ hai đọc tất cả tỷ lệ phần trăm vào một danh sách. 

biểu hiện`sum(p)`tính toán tổng phần trăm đóng góp từ tất cả đồ uống. Chia cho`n`đưa ra giá trị trung bình số học, chính xác là nồng độ của nước cam trong ly cocktail cuối cùng. 

Python tự động thực hiện phép chia dấu phẩy động khi sử dụng`/`, do đó kết quả được in với độ chính xác đủ để đạt được mức dung sai lỗi yêu cầu. 

Không có điều kiện biên phức tạp. Các giá trị có thể nằm trong khoảng từ 0 đến 100 và tổng tối đa chỉ là 10.000, thấp hơn nhiều so với bất kỳ giới hạn số nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
50 50 100
```| Bước | Tỷ lệ hiện tại | Tổng Chạy | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| Đọc 50 | 50 | 50 | 
| Đọc 50 | 50 | 100 | 
| Đọc 100 | 100 | 200 | 

Tính toán cuối cùng:$$200 / 3 = 66.666666666667$$Đầu ra:```
66.666666666667
```Ví dụ này cho thấy nồng độ cuối cùng chỉ đơn giản là giá trị trung bình của tất cả các tỷ lệ phần trăm. 

### Ví dụ 2 

đầu vào:```
2
0 100
```| Bước | Tỷ lệ hiện tại | Tổng Chạy | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| Đọc 0 | 0 | 0 | 
| Đọc 100 | 100 | 100 | 

Tính toán cuối cùng:$$100 / 2 = 50$$Đầu ra:```
50
```Ví dụ này chứng minh rằng đồ uống có 0% nước cam được xử lý một cách tự nhiên bằng công thức tính trung bình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần trăm được xử lý một lần trong khi tính tổng | 
| Không gian | O(1) | Chỉ cần một vài biến ngoài dữ liệu đầu vào | 

Với tối đa 100 đồ uống, thuật toán chỉ thực hiện một lượng công việc rất nhỏ. Cả thời gian và mức sử dụng bộ nhớ đều nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    print(sum(p) / n)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert abs(float(run("3\n50 50 100\n")) - 66.666666666667) < 1e-6

# minimum size
assert abs(float(run("1\n0\n")) - 0.0) < 1e-6

# all equal values
assert abs(float(run("4\n25 25 25 25\n")) - 25.0) < 1e-6

# all maximum values
assert abs(float(run("5\n100 100 100 100 100\n")) - 100.0) < 1e-6

# mixture of extremes
assert abs(float(run("2\n0 100\n")) - 50.0) < 1e-6

# maximum n
assert abs(float(run("100\n" + " ".join(["100"] * 100) + "\n")) - 100.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0`| Kích thước đầu vào tối thiểu | 
|`4 / 25 25 25 25`|`25`| Tất cả các giá trị bằng nhau | 
|`5 / 100 100 100 100 100`|`100`| Ranh giới phần trăm trên | 
|`2 / 0 100`|`50`| Pha trộn thái cực | 
|`100`uống tất cả`100`|`100`| Kích thước đầu vào tối đa | 

## Vỏ cạnh 

### Uống một lần 

đầu vào:```
1
73
```Dấu vết: 

Tổng các phần trăm là`73`. Chia cho`1`cho`73`. 

Đầu ra:```
73
```Chỉ với một ly, ly cocktail giống hệt với ly ban đầu. 

### Tất cả tỷ lệ phần trăm đều bằng 0 

đầu vào:```
3
0 0 0
```Dấu vết: 

Tổng số tiền còn lại`0`.$$0 / 3 = 0$$Đầu ra:```
0
```Thuật toán xử lý chính xác việc thiếu nước cam. 

### Tất cả tỷ lệ phần trăm là một trăm 

đầu vào:```
4
100 100 100 100
```Dấu vết: 

Tổng số tiền là`400`.$$400 / 4 = 100$$Đầu ra:```
100
```Hỗn hợp đồ uống nước cam nguyên chất vẫn là nước cam nguyên chất. 

### Hỗn hợp các giá trị cực trị 

đầu vào:```
2
0 100
```Dấu vết: 

Tổng số tiền là`100`.$$100 / 2 = 50$$Đầu ra:```
50
```Điều này xác nhận rằng tỷ lệ phần trăm trung bình mô hình chính xác việc pha trộn lượng đồ uống bằng nhau.
