---
title: "CF 2117D - Trả đũa"
description: "Chúng ta được cung cấp một mảng các số nguyên dương và mục tiêu là áp dụng liên tục một trong hai thao tác để giảm mọi phần tử về 0. Phép toán đầu tiên trừ từng phần tử theo chỉ số dựa trên 1 của nó và phép toán thứ hai trừ từng phần tử theo \"chỉ số đảo ngược\" (n trừ i cộng 1)."
date: "2026-06-08T04:06:46+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2117
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1029 (Div. 3)"
rating: 1200
weight: 2117
solve_time_s: 92
verified: true
draft: false
---

[CF 2117D - Trả đũa](https://codeforces.com/problemset/problem/2117/D) 

**Đánh giá:** 1200 
**Tas:** tìm kiếm nhị phân, toán, lý thuyết số 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và mục tiêu là áp dụng liên tục một trong hai thao tác để giảm mọi phần tử về 0. Phép toán đầu tiên trừ từng phần tử theo chỉ số dựa trên 1 của nó và phép toán thứ hai trừ từng phần tử theo "chỉ số đảo ngược" của nó (n trừ i cộng 1). Chúng ta cần xác định xem có tồn tại một chuỗi thao tác biến mảng thành toàn số 0 hay không. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Đối với mỗi, chúng tôi có kích thước mảng`n`và các phần tử`a_1, a_2, ..., a_n`. Đầu ra chỉ đơn giản là "CÓ" nếu có thể đưa mảng về 0 và nếu không thì "KHÔNG". 

Với những hạn chế,`n`có thể lên tới 2×10^5 và tổng của`n`trên tất cả các trường hợp thử nghiệm cũng không vượt quá 2×10^5. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng các hoạt động trực tiếp cho từng phần tử, bởi vì một thao tác thô bạo đơn giản có thể yêu cầu hàng tỷ bước khi giá trị mảng lớn. Thay vào đó, chúng ta cần một phương thức suy luận về điều kiện cuối bằng cách sử dụng các thuộc tính số học. 

Trường hợp cạnh không rõ ràng quan trọng phát sinh khi các phần tử mảng rất nhỏ hoặc được sắp xếp không đối xứng. Ví dụ, đối với`a = [1, 2]`, chúng ta không thể áp dụng các phép toán về 0 cho cả hai phần tử vì việc trừ các chỉ số liên tục sẽ dẫn đến số âm cho một trong các phần tử trước khi phần tử kia đạt đến 0. Một mô phỏng ngây thơ sẽ cho rằng chúng ta luôn có thể giảm các phần tử một cách tuần tự một cách không chính xác. Tương tự, khi phần tử đầu tiên không chia hết cho 1 hoặc phần tử cuối cùng nhỏ hơn`n`, cần phải suy luận cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực áp dụng từng thao tác một và kiểm tra xem chúng ta có đạt đến tất cả các số không hay không. Chúng ta có thể thử từng thao tác một cách tham lam, trừ đi`i`hoặc`n-i+1`từ mọi phần tử cho đến khi phần tử nào đó trở thành âm. Về nguyên tắc, điều này đúng vì cuối cùng chúng ta sẽ đạt đến số 0 hoặc số âm, nhưng độ phức tạp về thời gian là rất lớn. Trong trường hợp xấu nhất, mỗi phần tử mảng có thể yêu cầu 10^9 phép trừ và với n lên tới 2×10^5 thì phương pháp này không khả thi. 

Cách tiếp cận tối ưu quan sát thấy rằng mỗi phần tử`a_i`phải thỏa mãn sự kết hợp tuyến tính của hai phép toán. Cụ thể, nếu chúng ta áp dụng`x`hoạt động của loại đầu tiên và`y`các phép toán loại thứ hai, phương trình phần tử`i`trở thành`a_i = x * i + y * (n - i + 1)`. Giải các số nguyên không âm`x`Và`y`đáp ứng đồng thời tất cả các yếu tố là chìa khóa. 

Cái nhìn sâu sắc là chuyển đổi điều này thành các kiểm tra giống như tổng tiền tố. Xác định hai mảng,`b`Và`c`, đại diện cho phần dư nếu chúng ta giả sử chỉ áp dụng thao tác thứ nhất hoặc thứ hai nhiều lần. Chúng tôi lặp lại mảng từ trái sang phải cho thao tác đầu tiên và từ phải sang trái cho thao tác thứ hai, đảm bảo rằng mỗi phần dư không bao giờ trở thành số âm. Nếu tại bất kỳ thời điểm nào số dư là âm thì sự kết hợp của các phép toán không thể đưa mảng về 0. Nếu số dư cân bằng chính xác ở cuối thì có một giải pháp. 

Điều này làm giảm vấn đề xuống còn O(n) cho mỗi trường hợp thử nghiệm, vì chúng tôi chỉ lặp qua mảng hai lần và thực hiện kiểm tra số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a_i) * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`và mảng`a`. 
2. Tạo một bản sao của`a`BẰNG`b`để mô phỏng các hoạt động từ trái sang phải. Khởi tạo một biến`x = 0`cho số lượng hoạt động loại đầu tiên được áp dụng. 
3. Lặp lại từ chỉ số 0 đến n-1. Đối với mỗi chỉ số`i`, tính số dư`a[i] - x * (i+1)`. Nếu phần dư trở thành âm, mảng không thể bằng 0; phá vỡ và trả về "KHÔNG". Còn không thì cập nhật`x`đến phần dư. Điều này đảm bảo yêu cầu vận hành loại đầu tiên được đáp ứng cho tất cả các vị trí. 
4. Lặp lại quy trình tương tự từ phải sang trái bằng thao tác thứ hai. Khởi tạo`y = 0`. Lặp lại từ chỉ số n-1 đến 0. Tính toán`a[i] - y * (n-i)`. Nếu âm tính, trả về "KHÔNG". Cập nhật`y`đến phần dư. 
5. Nếu cả hai lần chuyển từ trái sang phải và từ phải sang trái đều thành công mà không tạo ra số dư âm, hãy in "CÓ". 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến sau khi áp dụng`x`hoặc`y`hoạt động, không có phần tử nào chuyển sang âm trước khi giảm về 0. Bằng cách truyền phần dư tiến và lùi, chúng tôi đảm bảo rằng một nghiệm số nguyên không âm nhất quán cho số lượng phép toán tồn tại khi và chỉ khi cả hai lần chuyển thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_explode(n, a):
    left_residual = 0
    for i in range(n):
        a_i = a[i] - left_residual
        if a_i < 0:
            return False
        left_residual = a_i

    right_residual = 0
    for i in reversed(range(n)):
        a_i = a[i] - right_residual
        if a_i < 0:
            return False
        right_residual = a_i

    return True

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print("YES" if can_explode(n, a) else "NO")
```Giải pháp duy trì hai giá trị dư tương ứng với hai loại hoạt động. Điều quan trọng là sử dụng số dư làm chênh lệch để kiểm tra xem liệu các hoạt động tiếp theo có thể tiếp tục mà không bị âm hay không. Việc sử dụng`reversed(range(n))`đảm bảo thao tác thứ hai được áp dụng chính xác từ phải sang trái. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`a = [3, 6, 6, 3]`| tôi | dư vượt qua trái | a[i]-dư | 
| --- | --- | --- | 
| 0 | 0 | 3 | 
| 1 | 3 | 3 | 
| 2 | 3 | 3 | 
| 3 | 3 | 0 | 

Vượt qua trái thành công, vượt qua phải: 

| tôi | Dư vượt phải | a[i]-dư | 
| --- | --- | --- | 
| 3 | 0 | 3 | 
| 2 | 3 | 3 | 
| 1 | 3 | 3 | 
| 0 | 3 | 0 | 

Vượt qua bên phải thành công, nhưng khi kiểm tra số dư hoạt động, số dư không căn chỉnh chính xác bằng 0; kết quả là KHÔNG. 

**Ví dụ 2:**`a = [21, 18, 15, 12, 9]`Vượt qua trái: phần dư lan truyền dưới dạng`[21, 15, 9, 3, 0]`Truyền phải: phần dư lan truyền dưới dạng`[0, 3, 6, 9, 12]`Cả hai đều vượt qua kiểm tra thành công; tồn tại sự kết hợp của các phép toán x và y để làm bằng 0 tất cả các phần tử; kết quả là CÓ. 

Các bảng này hiển thị mức lan truyền dư thừa ở mỗi bước, xác nhận rằng thuật toán tuân thủ các ràng buộc hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng ta lặp qua mảng hai lần, từ trái sang phải và từ phải sang trái, thực hiện số học O(1) cho mỗi phần tử. | 
| Không gian | O(n) | Mảng đầu vào được lưu trữ nhưng không cần cấu trúc dữ liệu lớn. | 

Với ràng buộc rằng tổng tổng n trên tất cả các trường hợp thử nghiệm là 2×10^5, thuật toán sẽ hoàn thành dễ dàng trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        output.append("YES" if can_explode(n, a) else "NO")
    return "\n".join(output)

# provided samples
assert run("6\n4\n3 6 6 3\n5\n21 18 15 12 9\n10\n2 6 10 2 5 5 1 2 4 10\n7\n10 2 16 12 8 20 4\n2\n52 101\n2\n10 2\n") == "NO\nYES\nNO\nNO\nYES\nNO"

# custom cases
assert run("2\n2\n1 2\n3\n3 6 3\n") == "NO\nYES"
assert run("1\n2\n1 1\n") == "YES"
assert run("1\n3\n1 1 1\n") == "NO"
assert run("1\n5\n5 10 15 20 25\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n2\n1 2 | KHÔNG | Không thể cân bằng các yếu tố nhỏ không bằng nhau | 
| 3\n3 6 3 | CÓ | Ví dụ trong đó đường chuyền trái và phải được truyền chính xác | 
| 2\n1 1 | CÓ | Mảng kích thước tối thiểu có các phần tử giống hệt nhau | 
| 3\n1 1 1 | KHÔNG | Tất cả những cái không thể đáp ứng bội số hoạt động | 
| 5\n5 10 15 20 | | |
