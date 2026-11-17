---
title: "CF 2000A - Nhiệm vụ chính"
description: "Chúng ta được cung cấp một danh sách các số nguyên được viết trên bảng và một trong số chúng có thể là biểu diễn sai của một số có dạng $10^x$ trong đó $x ge 2$. Lỗi xảy ra do ký hiệu lũy thừa '^' bị mất."
date: "2026-06-08T14:11:27+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 2000
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 966 (Div. 3)"
rating: 800
weight: 2000
solve_time_s: 157
verified: true
draft: false
---

[CF 2000A - Nhiệm vụ chính](https://codeforces.com/problemset/problem/2000/A) 

**Đánh giá:** 800 
**Tags:** triển khai, toán học, chuỗi 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên được viết trên bảng và một trong số chúng có thể là biểu diễn sai của một số có dạng$10^x$Ở đâu$x \ge 2$. Lỗi xảy ra do ký hiệu lũy thừa '^' bị mất. Ví dụ,$10^5$có thể đã được viết là$105$Và$10^{19}$BẰNG$1019$. Nhiệm vụ của chúng ta là xác định, đối với mỗi số trên bảng, liệu đó có phải là phiên bản sai của lũy thừa mười như vậy hay không. Nếu có thể, chúng tôi in "CÓ"; nếu không, chúng tôi in "KHÔNG". 

Kích thước đầu vào cho phép tối đa 10.000 số nguyên, mỗi số không vượt quá 10.000. Giới hạn trên này nhỏ, điều này ngay lập tức gợi ý rằng chúng ta có thể tính toán trước tất cả các số hợp lệ tiềm năng và thực hiện kiểm tra thành viên đơn giản cho mỗi số nguyên, thay vì thực hiện các phép biến đổi số học phức tạp. Sự tinh tế của vấn đề nằm ở việc diễn giải chính xác những gì được coi là sai lạc.$10^x$: số phải bắt đầu bằng 10 và sau đó có các chữ số của số mũ được nối trực tiếp, không được nối bất kỳ chữ số tùy ý nào. Ví dụ: 1010 có thể tương ứng với$10^{10}$, nhưng 100 hoặc 1023 thì không thể. 

Các trường hợp khó khăn có thể gặp phải một giải pháp đơn giản bao gồm các số như 100 (tức là$10^2$) - từ$x \ge 2$, sự sai lệch có thể tạo ra 102 chứ không phải 100. Tương tự, những con số như 1002 cần được giải thích cẩn thận: nó đại diện cho$10^2$với '2' được thêm làm số mũ, nhưng bản thân 1002 không tương ứng với dạng sai hợp lệ vì số mũ cần phải là 2 nhưng các chữ số không khớp. Những con số nhỏ, những con số có nhiều số 0 đứng đầu và những con số gần giới hạn 10.000 là những cạm bẫy tiềm ẩn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng lại$10^x$cho tất cả$x \ge 2$, chuyển đổi chúng thành chuỗi, mô phỏng lỗi bằng cách xóa ký hiệu '^', sau đó so sánh từng số bị lỗi với danh sách đầu vào. Mặc dù điều này khả thi đối với một số nguyên duy nhất, nhưng thực hiện điều này với 10.000 số nguyên và số mũ rất lớn sẽ không hiệu quả. Cụ thể là kiểm tra tất cả$x$chẳng hạn, tối đa 1.000.000 là không cần thiết vì số nguyên của chúng ta bị giới hạn bởi 10.000. 

Quan sát quan trọng là số nguyên tối đa trên bảng là 10.000, có nghĩa là số lớn nhất$10^x$có thể tạo ra một số bị hỏng trong phạm vi này là$10^2 = 100$lên tới$10^4 = 10000$. Tổng quát hơn, nếu chúng ta liệt kê$x$từ 2 trở lên và tạo thành các số bị hỏng bằng cách ghép "10" và các chữ số của$x$, chúng ta có thể tính toán trước tất cả các số hợp lệ có thể nằm trong phạm vi từ 1 đến 10.000. Sau đó, với mỗi số đầu vào, chúng ta chỉ cần kiểm tra xem nó có tồn tại trong tập hợp được tính toán trước này hay không. Điều này làm giảm vấn đề xuống mức tra cứu theo thời gian liên tục trên mỗi số và nhanh chóng một cách tầm thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đánh dấu từng x cho mỗi a) | O(t * max_x) | O(1) | Quá chậm nếu max_x lớn | 
| Tính toán trước các số bị hỏng hợp lệ | O(max_x + t) | O(max_x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập hợp trống để lưu trữ tất cả các số có thể là dạng sai của$10^x$với$x \ge 2$và số kết quả ≤ 10.000. Chúng tôi sử dụng một bộ để kiểm tra tư cách thành viên O(1). 
2. Lặp lại$x$bắt đầu từ 2, chuyển đổi$x$thành một chuỗi và nối nó với "10" để mô phỏng số bị hỏng. Chuyển đổi chuỗi nối này trở lại thành số nguyên. 
3. Nếu số nguyên kết quả lớn hơn 10.000, hãy ngắt vòng lặp, vì số nguyên lớn hơn$x$chỉ tạo ra số lớn hơn, nằm ngoài giới hạn đầu vào. 
4. Thêm từng số nguyên hợp lệ vào tập hợp. 
5. Đối với mỗi số nguyên đầu vào, hãy kiểm tra xem nó có tồn tại trong tập hợp được tính toán trước hay không. Nếu có, hãy in "CÓ"; nếu không thì in "KHÔNG". 

Điều này có tác dụng vì sai số mang tính quyết định: số hợp lệ phải luôn bắt đầu bằng "10", theo sau là các chữ số đại diện cho số mũ. Bằng cách tính toán trước tất cả các khả năng trong giới hạn đầu vào, chúng tôi đảm bảo tính chính xác và tránh các phép tính không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
numbers = [int(input()) for _ in range(t)]

valid_set = set()
x = 2
while True:
    corrupted = int("10" + str(x))
    if corrupted > 10000:
        break
    valid_set.add(corrupted)
    x += 1

for a in numbers:
    if a in valid_set:
        print("YES")
    else:
        print("NO")
```Đầu tiên chúng ta đọc tất cả thông tin đầu vào vào một danh sách. Vòng lặp tiền tính toán xây dựng tập hợp tất cả các số bị hỏng hợp lệ bằng cách ghép "10" với mỗi số mũ bắt đầu từ 2 cho đến khi số kết quả vượt quá 10.000. Trong vòng lặp cuối cùng, chúng tôi tận dụng tra cứu O(1) của tập hợp để xác định tư cách thành viên một cách hiệu quả. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

đầu vào:```
7
100
1010
101
105
2033
1019
1002
```Bộ được tính toán trước: {102, 103, 104, 105, 106, 107, 108, 109, 1010, 1011, 1012, 1013, 1014, 1015, 1016, 1017, 1018, 1019, 1020, 1021, 1022, 1023, 1024} 

| Nhập một | Đã sẵn sàng chưa? | Đầu ra | 
| --- | --- | --- | 
| 100 | Không | KHÔNG | 
| 1010 | Có | CÓ | 
| 101 | Không | KHÔNG | 
| 105 | Có | CÓ | 
| 2033 | Không | KHÔNG | 
| 1019 | Có | CÓ | 
| 1002 | Không | KHÔNG | 

Điều này xác nhận thuật toán xác định chính xác các số bị hỏng hợp lệ và bỏ qua các số khác. 

Một ví dụ khác: 

đầu vào:```
3
102
103
1100
```| Nhập một | Đã sẵn sàng chưa? | Đầu ra | 
| --- | --- | --- | 
| 102 | Có | CÓ | 
| 103 | Có | CÓ | 
| 1100 | Không | KHÔNG | 

Điều này chứng tỏ tính đúng đắn của giới hạn dưới của số mũ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t + max_x) | Vòng lặp tiền tính toán trên x cho đến khi bị hỏng > 10000; kiểm tra t số là O(1) mỗi số | 
| Không gian | O(max_x) | Set lưu trữ tất cả các số bị hỏng hợp lệ; max_x ≈ 100 | 

Với t 10^4 và số nguyên ≤ 10^4, việc tính toán trước tối đa vài chục số và kiểm tra tư cách thành viên cực kỳ nhanh chóng. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    numbers = [int(input()) for _ in range(t)]
    
    valid_set = set()
    x = 2
    while True:
        corrupted = int("10" + str(x))
        if corrupted > 10000:
            break
        valid_set.add(corrupted)
        x += 1
    
    for a in numbers:
        if a in valid_set:
            print("YES")
        else:
            print("NO")
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided sample
assert run("7\n100\n1010\n101\n105\n2033\n1019\n1002\n") == "NO\nYES\nNO\nYES\nNO\nYES\nNO", "Sample 1"

# Custom tests
assert run("3\n102\n103\n1100\n") == "YES\nYES\nNO", "Lower boundary exponents"
assert run("5\n100\n101\n104\n109\n1015\n") == "NO\nNO\nYES\nYES\nYES", "Mix of valid and invalid"
assert run("1\n10000\n") == "NO", "Maximum value edge"
assert run("1\n1010\n") == "YES", "Direct match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n102\n103\n1100 | CÓ\nCÓ\nKHÔNG | Số mũ giới hạn dưới | 
| 5\n100\n101\n104\n109\n1015 | KHÔNG\nKHÔNG\nCÓ\nCÓ\nCÓ | Kết hợp hợp lệ và không hợp lệ | 
| 1\n10000 | KHÔNG | Số ranh giới trên, bên ngoài các biểu mẫu bị hỏng hợp lệ | 
| 1\n1010 | CÓ | Trực tiếp đấu với sức mạnh mười phần | 

## Vỏ cạnh 

Đối với số mũ nhỏ nhất có thể, x = 2, số bị hỏng là 102. Thuật toán bao gồm 102 trong bộ, đảm bảo rằng đầu vào '102' được xác định chính xác là "CÓ". Đối với các số ngay trên giới hạn 10.000, chẳng hạn như 10002, vòng lặp tính toán trước sẽ dừng trước khi đưa chúng vào, do đó thuật toán trả về chính xác "KHÔNG". Tra cứu dựa trên tập hợp đảm bảo rằng các số lặp lại hoặc đầu vào không có thứ tự không ảnh hưởng đến tính chính xác. Sự hình thành xác định của các số bị hỏng đảm bảo không có số hợp lệ nào bị bỏ qua.
