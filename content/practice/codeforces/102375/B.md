---
title: "CF 102375B - \u0411\u043e\u043b\u044c\u0448\u0438\u0435 \u043f\u0435\u0440\u0435\u043c\u0435\u043d\u044b"
description: "Chúng tôi có (N) các thành phố được gắn nhãn và phải xây dựng một biểu đồ vô hướng được kết nối bằng cách sử dụng chính xác (N-1) các kết nối hàng không riêng biệt. Vì một đồ thị liên thông trên (N) đỉnh có chính xác (N-1) cạnh là một cây, nên vấn đề thực sự là về các cây được gắn nhãn."
date: "2026-08-14T13:00:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "B"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 588
verified: false
draft: false
---

[CF 102375B - \u0411\u043e\u043b\u044c\u0448\u0438\u0435 \u043f\u0435\u0440\u0435\u043c\u0435\u043d\u044b](https://codeforces.com/problemset/problem/102375/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (N) các thành phố được gắn nhãn và phải xây dựng một biểu đồ vô hướng được kết nối bằng cách sử dụng chính xác (N-1) các kết nối hàng không riêng biệt. Vì một đồ thị liên thông trên (N) đỉnh có chính xác (N-1) cạnh là một cây, nên vấn đề thực sự là về các cây được gắn nhãn. 

Khả năng tiếp cận của một thành phố chỉ đơn giản là mức độ của nó, vì vậy chúng tôi muốn xem xét những cây có mức độ đỉnh tối đa càng lớn càng tốt. Nhiệm vụ là đếm xem có bao nhiêu cây được dán nhãn khác nhau đạt được mức độ tối đa có thể đó. 

Đầu vào chứa một số nguyên (N), với (2 \le N \le 10^9). Giới hạn trên là manh mối chính: việc xây dựng đồ thị, liệt kê các đỉnh hoặc thậm chí chạy một vòng lặp tỷ lệ với (N) là không cần thiết và sẽ không phù hợp. Câu trả lời phải đến từ quan sát cấu trúc và được tính toán trong thời gian không đổi. 

Trường hợp nhỏ nhất đáng được quan tâm đặc biệt. Với (N=2), có chính xác một cây có thể, bao gồm một cạnh duy nhất giữa hai thành phố. Hai đỉnh của nó đều có bậc (1), nên bậc lớn nhất là (1) và đáp án là (1). Một công thức chỉ trả về (N) sẽ cho kết quả (2) không chính xác. 

Với (N=3), mức tối đa có thể là (2). Mỗi cây trên ba đỉnh được dán nhãn là một đường đi và mỗi cây như vậy có một đỉnh bậc (2). Có ba lựa chọn cho đỉnh trung tâm đó, vì vậy câu trả lời là (3), phù hợp với mẫu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể liệt kê mọi tập hợp cạnh (N-1) có thể có từ các cặp thành phố (\binom N2) có thể có, kiểm tra xem biểu đồ kết quả có được kết nối hay không, tính toán tất cả các độ và giữ cho cây có độ tối đa lớn nhất. Số tập cạnh ứng cử viên chính xác là 

[ 
\binom{\binom N2}{N-1}, 
] 

vì vậy điều này trở nên vô vọng ngay cả đối với (N) khá nhỏ. Một lực lượng vũ phu thay thế dựa trên công thức của Cayley có thể liệt kê tất cả các cây được gắn nhãn (N^{N-2}), nhưng con số đó vẫn lớn theo cấp số nhân. Với (N) được phép đạt tới (10^9), cả hai cách diễn giải đều hoàn toàn không sử dụng được. 

Quan sát quan trọng là một đỉnh có thể được kết nối trực tiếp với hầu hết tất cả các thành phố (N-1) khác. Do đó không có cây nào có thể có bậc tối đa lớn hơn (N-1). Giới hạn trên này có thể đạt được: chọn một thành phố và kết nối nó với mọi thành phố khác. Đồ thị kết quả là một ngôi sao, được kết nối và chứa chính xác (N-1) cạnh. 

Với (N\ge3), mọi cây đạt bậc tối đa (N-1) phải chính xác là một ngôi sao như vậy. Nếu một số đỉnh có bậc (N-1), thì nó liền kề với mọi đỉnh khác, đã sử dụng tất cả (N-1) cạnh có sẵn. Không có cạnh bổ sung nào có thể tồn tại vì cây có chính xác (N-1) cạnh. Do đó, lựa chọn duy nhất là thành phố nào là trung tâm, cho (N) cây khác nhau. 

Trường hợp (N=2) là ngoại lệ duy nhất. Cả hai đỉnh đều liên tiếp với cạnh duy nhất, do đó việc chọn một trong hai đỉnh làm tâm giả định sẽ không tạo ra một cây khác. Chỉ có một công trình riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O\left(\binom{\binom N2}{N-1}\cdot N\right)) | (O(N^2)) | Quá chậm | 
| Tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N). Vì câu trả lời chỉ phụ thuộc vào (N=2) hay (N\ge3), nên không cần phải xây dựng bất kỳ đồ thị nào. 
2. Nếu (N=2), xuất ra (1). Chỉ có một cạnh có thể có và do đó chỉ có một cây có thể. 
3. Nếu (N\ge3), xuất ra (N). Mức độ lớn nhất có thể là (N-1) và một cây đạt đến mức đó một cách chính xác khi một thành phố được chọn được kết nối với mọi thành phố khác. Mỗi (N) thành phố có thể độc lập trở thành trung tâm duy nhất đó.

Bất biến quan trọng là cây có đỉnh bậc (N-1) đã sử dụng tất cả các cạnh (N-1) mà cây yêu cầu. Do đó, cây như vậy phải là một ngôi sao và mỗi cây tối ưu được xác định duy nhất bởi tâm của nó khi (N\ge3). Trường hợp đặc biệt (N=2) không có tâm duy nhất, đó là lý do tại sao nó phải được xử lý riêng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n == 2:
    print(1)
else:
    print(n)
```Dòng đầu tiên ghi số thành phố. điều kiện`n == 2`xử lý trường hợp duy nhất trong đó số lượng tâm có thể không bằng số lượng cây tối ưu riêng biệt. 

Với mọi (n\ge3), chương trình sẽ in`n`, bởi vì có chính xác một ngôi sao tối ưu cho mỗi lựa chọn tâm của nó. Không có vòng lặp trên các thành phố và không có cấu trúc dữ liệu biểu đồ, do đó việc triển khai vẫn duy trì thời gian không đổi ngay cả khi (N=10^9). 

Số nguyên Python dễ dàng biểu thị câu trả lời vì câu trả lời tối đa là (10^9), do đó không có vấn đề tràn. Việc so sánh phải sử dụng`n == 2`, không`n <= 2`, mặc dù đầu vào đảm bảo (N\ge2). 

## Ví dụ đã hoạt động 

### Mẫu 1: (N=3) 

Có ba sự lựa chọn có thể có cho trung tâm. Việc chọn thành phố (1), (2) hoặc (3) sẽ tạo ra ba ngôi sao được gắn nhãn khác nhau. 

| Bước | (N) | Tình trạng | Trả lời | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 3 | (N=2) là sai | | 
| Chọn công thức | 3 | (N\ge3) | 3 | 
| Đầu ra | 3 | | 3 | 

Kết quả là (3), chính xác như trong mẫu. Điều này cũng chứng tỏ tại sao trung tâm lại xác định duy nhất cây tối ưu khi có ít nhất ba thành phố. 

### Ví dụ tùy chỉnh: (N=2) 

Chỉ có một kết nối duy nhất có thể, giữa các thành phố (1) và (2). 

| Bước | (N) | Tình trạng | Trả lời | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 2 | (N=2) là đúng | | 
| Xử lý trường hợp đặc biệt | 2 | Cây độc đáo | 1 | 
| Đầu ra | 2 | | 1 | 

Dấu vết này thực hiện trường hợp ranh giới duy nhất trong đó việc trả về (N) sẽ không chính xác. Mặc dù một trong hai điểm cuối có thể được gọi là tâm, nhưng cả hai lựa chọn đều mô tả cùng một cạnh, do đó chỉ có một cấu trúc riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Chỉ cần một phép so sánh và một thao tác đầu ra. | 
| Không gian | (O(1)) | Thuật toán chỉ lưu trữ số nguyên đầu vào. | 

Giải pháp hằng số thời gian đặc biệt thích hợp cho (N\le10^9). Thuật toán không bao giờ phụ thuộc vào số lượng thành phố thông qua phép lặp hoặc xây dựng đồ thị, do đó giới hạn trên không có tác động thực tế đến thời gian thực hiện hoặc mức sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    if n == 2:
        print(1)
    else:
        print(n)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided sample
assert run("3\n") == "3", "sample 1"

# minimum input
assert run("2\n") == "1", "minimum N"

# smallest non-special case
assert run("3\n") == "3", "first case with a unique center"

# boundary near a large value
assert run("999999999\n") == "999999999", "large boundary"

# maximum input
assert run("1000000000\n") == "1000000000", "maximum N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`3`| Cung cấp mẫu và đếm sao thông thường | 
|`2`|`1`| Đầu vào tối thiểu và trường hợp đặc biệt | 
|`3`|`3`| Ranh giới giữa trường hợp đặc biệt và công thức tổng quát | 
|`999999999`|`999999999`| Giá trị lớn không bị tràn hoặc lặp lại | 
|`1000000000`|`1000000000`| Đầu vào tối đa được phép | 

Không có trường hợp "giá trị hoàn toàn bằng nhau" có ý nghĩa vì đầu vào chứa một số nguyên duy nhất thay vì một mảng hoặc tập hợp các giá trị. Tương tự có liên quan là kiểm tra các lệnh gọi lặp lại với các giá trị khác nhau mà bộ kiểm tra ở trên thực hiện thông qua một số đầu vào giá trị đơn độc lập. 

## Vỏ cạnh 

Với (N=2), đầu vào chính xác là`2`. Cây duy nhất có thể chứa cạnh ((1,2)), nên bậc tối đa là (1). Thuật toán đi vào nhánh đặc biệt và xuất ra`1`. Việc thực hiện bất cẩn`print(n)`sẽ xuất ra`2`, đếm hiệu quả cùng một cây hai lần bằng cách coi hai điểm cuối của nó là các tâm khác nhau. 

Với (N=3), đầu vào là`3`. Mức độ lớn nhất có thể là (2=N-1). Bất kỳ cây tối ưu nào cũng phải chứa một đỉnh liền kề với cả hai đỉnh còn lại và có đúng ba lựa chọn cho đỉnh đó. Thuật toán lấy nhánh chung và kết quả đầu ra`3`. Điều này xác nhận rằng việc chuyển từ trường hợp đặc biệt sang công thức (N) xảy ra chính xác tại (N=3). 

Để có đầu vào tối đa,`1000000000`, việc xây dựng ngay cả danh sách tất cả các thành phố cũng đã yêu cầu bộ nhớ không cần thiết và việc liệt kê các cạnh là không thể. Thuật toán chỉ thực hiện một phép so sánh và in ra`1000000000`. Kết quả xuất phát trực tiếp từ thực tế là mỗi thành phố trong số (10^9) có thể đóng vai trò là trung tâm của một ngôi sao tối ưu riêng biệt. 

Đối số cấu trúc cũng loại trừ các trường hợp đồ thị ẩn. Nếu cây tối ưu cho (N\ge3) có bậc tối đa (N-1) thì tâm của nó nằm liền kề với mọi thành phố khác. Các cạnh liên quan (N-1) đó đã chiếm toàn bộ tập cạnh của cây, do đó không thể có một cạnh khác giữa hai thành phố không phải là trung tâm. Do đó, không có công trình tối ưu nào bị bỏ sót và không có công trình không phải sao nào được tính.
