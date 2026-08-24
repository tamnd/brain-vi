---
title: "CF 102163L - Đề thi Hóa học"
description: "Bài thi của mỗi học sinh được mã hóa dưới dạng một số nguyên dương. Biểu diễn nhị phân của số nguyên đó mô tả các câu trả lời Đúng và Sai của học sinh: một bit được đặt đại diện cho một câu trả lời Đúng, trong khi một bit không được đặt đại diện cho một câu trả lời Sai."
date: "2026-08-23T08:37:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "L"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 1546
verified: true
draft: false
---

[CF 102163L - Kỳ thi Hóa học](https://codeforces.com/problemset/problem/102163/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài thi của mỗi học sinh được mã hóa dưới dạng một số nguyên dương. Biểu diễn nhị phân của số nguyên đó mô tả các câu trả lời Đúng và Sai của học sinh: một bit được đặt đại diện cho một câu trả lời Đúng, trong khi một bit không được đặt đại diện cho một câu trả lời Sai. Vì mọi câu trả lời trong bài kiểm tra đều đúng nên điểm của học sinh chính xác là số bit được đặt trong biểu diễn nhị phân của số của họ. 

Ví dụ, số`13`là`1101`ở dạng nhị phân. Nó chứa ba bit được đặt, do đó học sinh tương ứng sẽ ghi điểm`3`. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng`N`bài thi được mã hóa đó và phải xuất ra số đếm hoặc số bit cố định của từng phần tử theo cùng một thứ tự. 

Giá trị của`N`có thể đạt được`10^5`, do đó, một cách tiếp cận so sánh từng cặp học sinh hoặc thực hiện công việc tương ứng với`N^2`, ngay lập tức không phù hợp. Bản thân các giá trị nhiều nhất là`10^9`, có nghĩa là mỗi số có tối đa 30 chữ số nhị phân. Độ rộng bit cố định nhỏ đó là lý do chính khiến quét tuyến tính là đủ. Với giới hạn 1 giây, chúng ta nên nhắm tới khoảng`O(N)`hoạt động trên mỗi trường hợp thử nghiệm, chỉ với một lượng nhỏ xử lý không đổi trên mỗi số. 

Có một số trường hợp việc triển khai có thể gặp trục trặc nếu nó giả sử biểu diễn nhị phân có một hình dạng cụ thể. Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
1
```Đầu ra đúng là:```
1
```số`1`có chính xác một bit được đặt. Việc triển khai vô tình bắt đầu kiểm tra các bit từ vị trí 1 thay vì vị trí 0 sẽ tạo ra số 0 không chính xác. 

Một trường hợp biên khác là lũy thừa của hai:```
1
3
2 4 8
```Đầu ra đúng là:```
1 1 1
```Mọi lũy thừa của hai đều có chính xác một bit được đặt. Việc triển khai bất cẩn đếm số chữ số nhị phân thay vì số bit đã đặt sẽ trả về`2 3 4`, đó là số lượng sai. 

Một giá trị cũng có thể chứa các bit và số 0 liền kề giữa chúng:```
1
3
3 5 10
```Các dạng nhị phân là`11`,`101`, Và`1010`, vì vậy kết quả đúng là:```
2 2 2
```Chỉ kiểm tra bit được đặt cao nhất hoặc thấp nhất sẽ bỏ lỡ một số câu trả lời đúng. Toàn bộ mẫu bit phải được tính. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chuyển đổi mọi số nguyên thành nhị phân và kiểm tra tất cả các bit của nó, tăng dần câu trả lời bất cứ khi nào có một bit.`1`. Điều này hoàn toàn đúng vì mọi`1`trong biểu diễn nhị phân tương ứng với một câu hỏi đúng. 

Vì mọi`A_i`nhiều nhất là`10^9`, nó có nhiều nhất là 30 bit. Như vậy cách tiếp cận này mất`O(30N)`, điều này đơn giản hóa thành`O(N)`vì 30 là một hằng số. Trong Python, cách triển khai rõ ràng nhất là sử dụng phép toán số nguyên tích hợp`bit_count()`, thực hiện chính xác thao tác đếm số này mà không yêu cầu chúng ta thao tác thủ công các bit. 

Vòng đếm bit thủ công cũng có thể được bắt nguồn từ các nguyên tắc đầu tiên. biểu thức`x & 1`cho chúng tôi biết liệu bit thấp nhất có được đặt hay không và`x >> 1`loại bỏ bit đó. Lặp lại điều này cho đến khi`x`trở thành số 0 mỗi bit được đặt. Có một quan sát thao tác bit thậm chí còn tốt hơn:`x & (x - 1)`loại bỏ bit được đặt thấp nhất khỏi`x`. Do đó, việc áp dụng lặp đi lặp lại thao tác đó sẽ thực hiện chính xác một lần lặp trên mỗi bit đã đặt thay vì một lần lặp trên mỗi chữ số nhị phân. 

Việc giải thích bạo lực rất hữu ích để hiểu lý do tại sao câu trả lời lại là số lượng phổ biến, nhưng không cần thiết phải có`O(N^2)`thuật toán nào cả. Cấu trúc của mỗi bản ghi học sinh độc lập với mọi bản ghi khác. Chúng tôi có thể xử lý từng số nguyên riêng biệt và biểu diễn giới hạn 30 bit có nghĩa là ngay cả việc quét thủ công cũng đủ nhanh. của Python`int.bit_count()`trực tiếp đưa ra kết quả tương tự và là cách thực hiện tối ưu đơn giản nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét bit thủ công | O(30N) = O(N) | O(1) phụ trợ | Đã chấp nhận | 
| Số lượng tích hợp | O(N) | O(1) phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm là độc lập, do đó, quy trình tương tự có thể được áp dụng riêng biệt. 
2. Đọc`N`và`N`đề thi được mã hóa. Chúng ta chỉ cần giá trị của mỗi số nguyên vì điểm số phụ thuộc hoàn toàn vào biểu diễn nhị phân của chính nó. 
3. Với mọi số nguyên`x`, tính toán`x.bit_count()`. Điều này trả về số lượng bit đã đặt trong biểu diễn nhị phân của`x`, chính xác là số câu trả lời đúng được mã hóa bởi số nguyên đó. 
4. Lưu trữ số đếm kết quả theo cùng thứ tự với dữ liệu đầu vào và in chúng dưới dạng một dòng cách nhau bằng dấu cách. Việc giữ nguyên thứ tự rất quan trọng vì mỗi điểm đầu ra tương ứng với học sinh ở cùng vị trí trong mảng đầu vào. 

Tại sao nó hoạt động: đối với mỗi học sinh, mỗi vị trí nhị phân đại diện cho một câu hỏi và một tập hợp bit đại diện cho một câu trả lời đúng. các`bit_count()`phép toán đếm chính xác các bit đã đặt đó, vì vậy giá trị được tạo ra chính xác là điểm của học sinh đó. Vì học sinh được xử lý độc lập và theo thứ tự đầu vào nên mọi vị trí đầu ra đều nhận được điểm chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = [x.bit_count() for x in a]
        print(*ans)

if __name__ == "__main__":
    solve()
```Chương trình đầu tiên đọc`T`, sau đó xử lý từng trường hợp thử nghiệm một cách độc lập. Mảng được đọc dưới dạng số nguyên và việc hiểu danh sách được áp dụng`bit_count()`đến từng hồ sơ học sinh. 

Việc sử dụng`bit_count()`là cố ý. Bài toán yêu cầu số lượng`1`bit, không phải độ dài nhị phân hoặc vị trí của bit được đặt cao nhất. Số nguyên Python không có hành vi tràn chiều rộng cố định của các ngôn ngữ như C++, vì vậy các giá trị lên tới`10^9`được xử lý trực tiếp mà không cần bất kỳ loại đặc biệt nào. 

Cũng không có vấn đề gì riêng biệt bởi vì`bit_count()`xử lý chính xác vị trí bit 0. Ví dụ,`1`được biểu thị bằng bit thiết lập đơn ở vị trí 0, do đó số lượng của nó là`1`. 

Đầu ra được sản xuất với`print(*ans)`, chèn khoảng trắng giữa tất cả các điểm và tránh việc tạo chuỗi có định dạng lớn theo cách thủ công. Đầu vào sử dụng`sys.stdin.readline`, đáp ứng mẫu I/O nhanh được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với trường hợp thử nghiệm đầu tiên của mẫu, mảng đầu vào là`1 2 3 4 5`. Thuật toán xử lý từng số một cách độc lập. 

| Giá trị sinh viên | Biểu diễn nhị phân | Số bit đặt | Đầu ra | 
| --- | --- | --- | --- | 
| 1 |`1`| 1 | 1 | 
| 2 |`10`| 1 | 1 | 
| 3 |`11`| 2 | 2 | 
| 4 |`100`| 1 | 1 | 
| 5 |`101`| 2 | 2 | 

Dòng kết quả là`1 1 2 1 2`. Dấu vết này thể hiện tính bất biến trung tâm: điểm số chính xác là số lượng`1`bit, bất kể các bit đó xuất hiện ở đâu. 

Đối với trường hợp thử nghiệm thứ hai, đầu vào là`2 4 8 16`. 

| Giá trị sinh viên | Biểu diễn nhị phân | Số bit đặt | Đầu ra | 
| --- | --- | --- | --- | 
| 2 |`10`| 1 | 1 | 
| 4 |`100`| 1 | 1 | 
| 8 |`1000`| 1 | 1 | 
| 16 |`10000`| 1 | 1 | 

Đầu ra là`1 1 1 1`. Đây là một mẫu ranh giới hữu ích vì mọi giá trị đều là lũy thừa của hai. Chính xác một vị trí nhị phân được đặt trong mỗi trường hợp, mặc dù số chữ số nhị phân tiếp tục tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi trong số`N`các giá trị được xử lý một lần và mỗi giá trị có tối đa 30 bit. | 
| Không gian | O(N) | Mảng đầu vào và danh sách đầu ra chứa`N`số nguyên. | 

Trong tất cả các trường hợp thử nghiệm, công việc là tuyến tính trên tổng số học sinh. Với tối đa 30 bit liên quan trên mỗi số nguyên và`N <= 10^5`mỗi trường hợp thử nghiệm, điều này thoải mái phù hợp với giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với 256 MB đối với kích thước đầu vào nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(*(x.bit_count() for x in a))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample = """\
2
5
1 2 3 4 5
4
2 4 8 16
"""
assert run(sample) == "1 1 2 1 2\n1 1 1 1\n", "sample"

# Minimum-size input
assert run("""\
1
1
1
""") == "1\n", "minimum size"

# All values equal
assert run("""\
1
5
7 7 7 7 7
""") == "3 3 3 3 3\n", "all equal values"

# Boundary powers of two
assert run("""\
1
6
1 2 4 8 16 536870912
""") == "1 1 1 1 1 1\n", "powers of two"

# Mixed bit patterns, including the largest allowed value
assert run("""\
1
5
3 5 10 15 1000000000
""") == "2 2 2 4 13\n", "mixed patterns and upper boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`1`| Đầu vào tối thiểu và vị trí bit bằng 0 | 
|`5 / 7 7 7 7 7`|`3 3 3 3 3`| Các giá trị giống hệt nhau lặp đi lặp lại | 
|`1 2 4 8 16 536870912`| Sáu cái | Quyền hạn của hai vị trí bit tăng dần | 
|`3 5 10 15 1000000000`|`2 2 2 4 13`| Các mẫu nhị phân hỗn hợp và`10^9`ranh giới trên | 

## Vỏ cạnh 

Giá trị nhỏ nhất có thể là`1`, được biểu diễn dưới dạng nhị phân`1`. Đối với đầu vào```
1
1
1
```

`bit_count()`trả lại`1`, vì vậy đầu ra là```
1
```Không có bit hàng đầu bị thiếu để giải thích. Các số 0 đứng đầu không liên quan vì chúng không thể biểu thị các câu trả lời đúng. 

Sức mạnh của hai là một nguồn sai lầm phổ biến khác. Vì```
1
3
2 4 8
```các dạng nhị phân là`10`,`100`, Và`1000`. Mỗi cái chứa một bit được đặt, do đó thuật toán trả về```
1 1 1
```Số chữ số nhị phân thay đổi nhưng điểm số thì không. Điều này phân biệt việc đếm các bit tập hợp với việc đếm độ dài nhị phân. 

Các giá trị có nhiều bit tập hợp riêng biệt sẽ được xử lý mà không có trường hợp đặc biệt. Vì```
1
3
3 5 10
```các biểu diễn nhị phân là`11`,`101`, Và`1010`. Mỗi cái chứa hai bit được đặt, cho```
2 2 2
```Thuật toán không phụ thuộc vào độ kề hoặc vị trí của các bit đã đặt, vì vậy các số 0 giữa các câu trả lời đúng không có hiệu lực. 

Cuối cùng, giá trị được phép lớn nhất là`10^9`. Biểu diễn nhị phân của nó chứa 13 bit tập hợp, vì vậy```
1
1
1000000000
```sản xuất```
13
```Giá trị nằm trong phạm vi số nguyên của Python và`bit_count()`trực tiếp xử lý nó. Không cần xử lý đặc biệt ở ranh giới số.
