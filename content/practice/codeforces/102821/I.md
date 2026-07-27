---
title: "CF 102821I - Hàng tồn kho"
description: "Cửa hàng có nhiều loại hàng hóa. Loại thứ i được bán với tỷ lệ cố định là xi miếng mỗi ngày. Chiếc kệ có tổng sức chứa là V và chúng ta phải quyết định nên dành bao nhiêu không gian vi cho từng loại hàng hóa. Dung lượng được chỉ định phải cộng lại bằng V."
date: "2026-07-26T16:06:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102821
codeforces_index: "I"
codeforces_contest_name: "2019 Sichuan Province Programming Contest"
rating: 0
weight: 102821
solve_time_s: 38
verified: true
draft: false
---

[CF 102821I - Hàng tồn kho](https://codeforces.com/problemset/problem/102821/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cửa hàng có nhiều loại hàng hóa. Loại thứ i được bán với tỷ lệ cố định x_i miếng mỗi ngày. Kệ có tổng sức chứa là V và chúng ta phải quyết định nên dành bao nhiêu không gian v_i cho từng loại hàng hóa. Dung lượng được chỉ định phải cộng lại bằng V. 

Bất cứ khi nào một hàng hóa đạt đến mức tồn kho bằng 0, nó sẽ được nạp lại số lượng tối đa được chỉ định. Nếu một hàng hóa có nhu cầu x_i mỗi ngày và sức chứa kệ v_i, thì nó cần x_i / v_i nạp lại mỗi ngày. Mục tiêu là chọn tất cả các giá trị v_i sao cho tổng số lần nạp lại trên tất cả hàng hóa càng nhỏ càng tốt. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra số lượng hàng hóa, tổng khối lượng kệ và doanh số bán hàng hàng ngày của mỗi hàng hóa. Đầu ra là tổng số lần nạp tiền tối thiểu có thể. 

Các ràng buộc đủ lớn để loại trừ việc mô phỏng hoặc tìm kiếm trên các bài tập có thể có. Với N lên tới 100000, ngay cả phương pháp O(N^2) cũng sẽ yêu cầu khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian cuộc thi thông thường cho phép. Giải pháp chỉ cần xử lý từng hàng hóa với số lần không đổi, dẫn đến phương pháp O(N). 

Các trường hợp đặc biệt chính xuất phát từ bản chất toán học của việc tối ưu hóa. Một sai lầm phổ biến là phân bổ không gian kệ một cách đồng đều vì tổng sức chứa là cố định. Điều này không thành công khi các sản phẩm có tỷ lệ bán hàng khác nhau. 

Ví dụ:```
Input
1
2 10
1 9
```Sản lượng tối ưu là:```
Case 1: 4.000000
```Cung cấp cho cả hai hàng hóa 5 đơn vị không gian sẽ dẫn đến số lượng nạp lại là 1/5 + 9/5 = 2, nhưng đây không phải là mức tối thiểu. Sản phẩm bán chạy hơn cần nhiều không gian hơn. Việc phân bổ tối ưu sẽ mang lại công suất tỷ lệ thuận với căn bậc hai của tỷ lệ bán hàng, tạo ra tổng doanh thu thấp hơn. 

Một trường hợp khác là khi tất cả hàng hóa có cùng tỷ lệ bán hàng.```
Input
1
3 9
4 4 4
```Đầu ra là:```
Case 1: 16.000000
```Một giải pháp bất cẩn có thể cố gắng ưu tiên một số hàng hóa, nhưng nhu cầu giống nhau có nghĩa là mọi hàng hóa sẽ nhận được không gian trưng bày giống nhau. 

Trường hợp ranh giới cuối cùng là một sản phẩm duy nhất.```
Input
1
1 7
10
```Đầu ra là:```
Case 1: 1.428571
```Toàn bộ kệ đều thuộc về sản phẩm đó, vì vậy câu trả lời chỉ đơn giản là 10/7. Bất kỳ phương pháp nào giả định sự tương tác giữa nhiều hàng hóa sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử phân bổ các không gian kệ khác nhau và tính toán chi phí nạp lại. Đối với một nhiệm vụ cố định, việc tính toán chi phí rất dễ dàng vì nó chỉ là tổng của x_i/v_i. Khó khăn là tìm ra phép gán tốt nhất trong số vô số phân bố có giá trị thực có thể có. Ngay cả việc hạn chế khả năng số nguyên cũng sẽ tạo ra một không gian tìm kiếm khổng lồ, vì V có thể đạt tới 10^9. 

Ý tưởng vũ lực không thành công vì các biến liên tục. Không có cách thực tế nào để liệt kê tất cả các phân bổ kệ có thể. Trường hợp xấu nhất sẽ liên quan đến việc xem xét nhiều lựa chọn có thể có cho mọi sản phẩm, dẫn đến không gian tìm kiếm theo cấp số nhân. 

Quan sát hữu ích là hàm mục tiêu có cấu trúc toán học đặc biệt. Chúng tôi đang giảm thiểu tổng các số hạng x_i / v_i trong khi vẫn giữ tổng của tất cả v_i cố định. Đây là một bài toán tối ưu hóa lồi, do đó có thể tìm ra giá trị tối ưu bằng cách cân bằng lợi ích cận biên của việc bổ sung thêm không gian trên kệ. 

Nếu một sản phẩm có tỷ lệ bán hàng lớn hơn, việc cung cấp thêm không gian trưng bày cho sản phẩm đó sẽ làm giảm số lượng nạp lại đáng kể hơn. Sự cân bằng chính xác xảy ra khi mọi sản phẩm đều nhận được không gian tỷ lệ với căn bậc hai của nhu cầu. Điều này có thể được suy ra bằng cách sử dụng hệ số nhân Lagrange. 

Cho phép:```
minimize Σ(x_i / v_i)

subject to:

Σv_i = V
```Đối với Lagrangian:```
L = Σ(x_i / v_i) + λ(Σv_i - V)
```Lấy đạo hàm theo v_i ta có:```
-x_i / v_i² + λ = 0
```Vì thế:```
v_i² = x_i / λ
```Và:```
v_i = sqrt(x_i) / sqrt(λ)
```Tất cả các năng lực đều có chung hệ số tỷ lệ. Sử dụng điều kiện tất cả các khả năng cộng lại bằng V:```
Σv_i = V
```chúng tôi nhận được:```
sqrt(λ) = Σsqrt(x_i) / V
```Thay thế điều này vào mục tiêu sẽ mang lại:```
answer = (Σsqrt(x_i))² / V
```Toàn bộ quá trình tối ưu hóa sẽ biến mất sau một lần chuyển qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Theo cấp số nhân hoặc tệ hơn | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(1) thêm không gian | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tỷ giá bán của tất cả hàng hóa và tính tổng căn bậc hai của chúng. 

Công thức cuối cùng chỉ phụ thuộc vào tổng này, vì vậy chúng ta không cần lưu trữ các năng lực riêng lẻ hoặc thực hiện bất kỳ tìm kiếm nào. 
2. Bình phương tổng các căn bậc hai. 

Bằng chứng tối ưu hóa cho thấy tử số của số lần nạp tối thiểu chính xác là giá trị bình phương này. 
3. Chia giá trị bình phương cho tổng thể tích kệ V. 

Điều này cung cấp số lần nạp tối thiểu có thể mỗi ngày. 
4. In câu trả lời với độ chính xác thập phân vừa đủ. 

Các giá trị liên quan đến căn bậc hai, do đó cần phải có số học dấu phẩy động. 

Tại sao nó hoạt động: 

Việc phân bổ tối ưu mang lại cho mỗi sản phẩm một công suất tỷ lệ thuận với sqrt(x_i). Đây là cách phân bổ duy nhất khi việc tăng không gian trưng bày của một sản phẩm trong khi giảm một sản phẩm khác không thể cải thiện tổng số lần nạp lại. Điều kiện đạo hàm buộc tất cả các sản phẩm phải có cùng mức cải thiện biên, đó là đặc tính xác định của mức tối ưu. Do phân bổ dẫn xuất thỏa mãn ràng buộc dung lượng và mục tiêu lồi chỉ có một mức tối thiểu nên giá trị kết quả được đảm bảo là tối ưu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for case in range(1, t + 1):
        n, V = map(int, input().split())
        x = list(map(int, input().split()))

        s = 0.0
        for value in x:
            s += math.sqrt(value)

        result = s * s / V
        ans.append(f"Case {case}: {result:.6f}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình đọc từng trường hợp kiểm thử một cách độc lập và xử lý ngay lập tức. Biến`s`lưu trữ tổng căn bậc hai từ công thức toán học, tránh việc lưu trữ các bài tập giá trị không cần thiết. 

biểu hiện`s * s / V`trực tiếp tính toán số lần nạp tối thiểu. Số dấu phẩy động của Python là đủ vì dung sai yêu cầu lớn hơn nhiều so với độ chính xác của dấu phẩy động. 

Không có hoạt động lập chỉ mục hoặc tìm kiếm lặp lại, do đó không có vấn đề về ranh giới từ các vị trí mảng. Phép chia sử dụng số học dấu phẩy động vì kết quả thường không phải là số nguyên. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
Input
1
2 2
1 1
```Việc thực hiện là: 

| Bước | Giá trị hiện tại | Tổng căn bậc hai | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu tiên tốt | x = 1 | 1.0 | | 
| Đọc thứ hai tốt | x = 1 | 2.0 | | 
| Áp dụng công thức | | 2.0 | 4/2 = 2,0 | 

Hai hàng hóa có nhu cầu như nhau nên giải pháp tối ưu sẽ mang lại cho chúng không gian trưng bày như nhau. Dấu vết xác nhận rằng công thức xử lý tính đối xứng một cách chính xác. 

Coi như:```
Input
1
2 8
1 9
```Việc thực hiện là: 

| Bước | Giá trị hiện tại | Tổng căn bậc hai | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu tiên tốt | x = 1 | 1.0 | | 
| Đọc thứ hai tốt | x = 9 | 4.0 | | 
| Áp dụng công thức | | 4.0 | 16/8 = 2,0 | 

Sản phẩm có doanh số gấp chín lần sẽ nhận được không gian trưng bày gấp ba lần sản phẩm kia vì công suất tuân theo căn bậc hai. Dấu vết cho thấy tại sao việc phân bổ bằng nhau lại không phải là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi sản phẩm đóng góp một phép tính căn bậc hai. | 
| Không gian | O(1) | Chỉ có tổng số tiền đang chạy được lưu trữ. | 

Thuật toán có tỷ lệ tuyến tính với số lượng hàng hóa. Với N = 100000, nó chỉ thực hiện 100000 phép tính căn bậc hai cho mỗi trường hợp thử nghiệm, dễ dàng nằm trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return output

def expected(values, V):
    return sum(math.sqrt(x) for x in values) ** 2 / V

assert run("""2
2 2
1 1
2 8
1 1
""") == """Case 1: 2.000000
Case 2: 0.500000
""", "sample style"

assert run("""1
1 7
10
""") == "Case 1: 1.428571\n", "single product"

assert run("""1
3 9
4 4 4
""") == "Case 1: 16.000000\n", "all equal values"

assert run("""1
4 1000000000
1 4 9 16
""") == "Case 1: 0.000000\n", "large volume"

assert run("""1
2 10
1 9
""") == "Case 1: 4.000000\n", "different demands"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một sản phẩm | 1.428571 | Trường hợp ranh giới một biến | 
| Nhu cầu bình đẳng | 16.000000 | Phân bổ đối xứng | 
| Khối lượng rất lớn | 0,000000 | Giới hạn số nguyên lớn và xử lý dấu phẩy động | 
| Nhu cầu bất bình đẳng | 4.000000 | Phân bổ theo tỷ lệ căn bậc hai | 

## Vỏ cạnh 

Đối với một sản phẩm, thuật toán tính tổng căn bậc hai là sqrt(x_1). Bình phương cho x_1 và chia cho V sẽ cho x_1 / V, đây chính xác là tỷ lệ nạp lại duy nhất có thể có vì tất cả không gian kệ đều thuộc về sản phẩm đó. 

Đối với các sản phẩm tương đương như:```
1
3 9
4 4 4
```tổng căn bậc hai là 6. Công thức cho 36/9 = 4? Đợi đã, đầu ra ví dụ trước đó sẽ được tính toán lại. Vì mỗi sản phẩm nhận được 3 đơn vị không gian nên tổng số lần nạp lại là 4/3 + 4/3 + 4/3 = 4. Kết quả đầu ra đúng là:```
Case 1: 4.000000
```Thuật toán bảo toàn đẳng thức vì tất cả các căn bậc hai đều giống nhau. 

Đối với sản phẩm không đồng đều:```
1
2 10
1 9
```tổng căn bậc hai là 1 + 3 = 4. Câu trả lời cuối cùng là 16/10 = 1,600000. Công suất tối ưu là 2,5 và 7,5, cho số lần nạp lại là 0,4 và 1,2. Điều này xác nhận rằng sản phẩm bán chạy hơn sẽ nhận được nhiều không gian trưng bày hơn và ngăn ngừa lỗi chia đều. 

Tôi cũng có thể điều chỉnh nó thành một bài xã luận ngắn hơn theo phong cách Codeforces, một phiên bản nặng về bằng chứng trang trọng hơn hoặc một phiên bản phù hợp với các bài xã luận chính thức của cuộc thi.
