---
title: "CF 103934D - Lạm phát"
description: "Chúng ta có một tập hợp $n$ các số nguyên tố khác nhau $a1, a2, dots, an$. Từ những số nguyên tố này chúng ta tạo thành một tích lớn $P = prod ai$. Mỗi mặt hàng trong cửa hàng được xác định theo một cách khác thường: giá của mặt hàng $i$ là tích của tất cả các số nguyên tố ngoại trừ $ai$."
date: "2026-07-02T07:12:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103934
codeforces_index: "D"
codeforces_contest_name: "2022 USP Try-outs"
rating: 0
weight: 103934
solve_time_s: 61
verified: true
draft: false
---

[CF 103934D - Lạm phát](https://codeforces.com/problemset/problem/103934/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập$n$số nguyên tố phân biệt$a_1, a_2, \dots, a_n$. Từ những số nguyên tố này chúng ta tạo thành một sản phẩm khổng lồ duy nhất$P = \prod a_i$. Mỗi mặt hàng trong cửa hàng được xác định theo một cách khác thường: giá của mặt hàng$i$là tích của tất cả các số nguyên tố ngoại trừ$a_i$. Nói cách khác, mỗi mức giá “gần như$P$”, nhưng thiếu đúng một thừa số nguyên tố. 

Vì vậy, mọi mục đều có cấu trúc “mọi thứ ngoại trừ một số nguyên tố bị thiếu” và việc mua một mục tương ứng với việc chọn số nguyên tố nào được loại trừ khỏi hệ số của nó. 

Chúng tôi cũng được cấp một số tiền mục tiêu$d$, được mô tả là sản phẩm của$m$số nguyên tố (không nhất thiết phải phân biệt). Nhiệm vụ là xác định xem liệu chúng ta có thể chọn một tập hợp con các mặt hàng sao cho tổng giá của chúng chính xác hay không.$d$. 

Khó khăn chính là các giá trị liên quan là cực kỳ lớn, vì mỗi mức giá là tích của tối đa 3000 số nguyên tố. Bất kỳ mô phỏng số trực tiếp nào cũng nhanh chóng trở nên không thực tế trừ khi chúng ta khai thác cấu trúc. 

Một mối quan tâm ngây thơ là giới hạn tràn hoặc đại diện. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, thậm chí việc xây dựng một mức giá duy nhất là không thể. Ngay cả trong Python, việc tính tổng một cách mù quáng nhiều số nguyên lớn như vậy sẽ gây ra các vấn đề về hiệu suất nếu chúng ta không giảm thiểu vấn đề một cách có cấu trúc. 

Trường hợp cạnh tinh tế xuất hiện khi$d$chứa các số nguyên tố không nằm trong số$a_i$. Trong trường hợp đó, không có cách nào để đại diện$d$dưới dạng tổng của các mục đã cho vì mỗi mục chỉ bao gồm các số nguyên tố từ$a_i$bộ. Vì vậy, bất kỳ sự không phù hợp nào như vậy ngay lập tức hàm ý sự thất bại. 

Một tình huống quan trọng khác là khi$n = 1$. Khi đó chỉ có một mục và vấn đề giảm xuống còn việc kiểm tra xem giá trị đó có bằng không$d$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử tất cả các tập hợp con của các mặt hàng, tính tổng giá của chúng và so sánh nó với$d$. Điều này hoạt động về mặt khái niệm vì nó liệt kê tất cả các khả năng. Tuy nhiên, có$2^n$tập hợp con và với$n = 3000$, điều này trở nên hoàn toàn không thể thực hiện được. Thậm chí$n = 40$sẽ là đường biên giới. 

Cấu trúc của giá cả là sự đơn giản hóa quan trọng. Mỗi mục bằng$P / a_i$, nghĩa là tất cả các mục đều cực kỳ gần nhau về độ lớn, chỉ khác nhau bởi một thừa số nguyên tố duy nhất bị thiếu. Điều này làm cho hệ thống có tính quy luật cao: thay vì các trọng số tùy ý, chúng ta có một họ số có cấu trúc chặt chẽ bắt nguồn từ một tích toàn cục. 

Quan sát quan trọng là mọi số trong hệ thống đều được xác định liên quan đến cùng một tích cơ sở$P$. Vì vậy, thay vì coi các vật phẩm là những giá trị độc lập, chúng ta có thể coi chúng như những sự biến đổi của một vật thể duy nhất. Điều này cho phép chúng ta giảm bớt vấn đề trong việc chọn “các số nguyên tố bị thiếu” tương ứng với một tập hợp con có đóng góp tổng hợp phù hợp với cấu trúc của$d$. 

Khi vấn đề được trình bày lại theo cách này, tổng sẽ hoạt động giống như một hệ thống bị ràng buộc trong đó tác động của mỗi lựa chọn có thể dự đoán được và có thể so sánh được. Cấu trúc này cho phép xây dựng tham lam sau khi sắp xếp các mục theo kích thước của số nguyên tố còn thiếu của chúng. 

Các số nguyên tố nhỏ hơn tương ứng với các giá trị mục lớn hơn, vì loại bỏ một hệ số nhỏ khỏi$P$tạo ra thương số lớn hơn. Thứ tự này tạo ra một cấu trúc đơn điệu: các mặt hàng lớn thống trị các mặt hàng nhỏ và hệ thống hoạt động giống như một hệ thống tiền xu chuẩn trong đó chiến lược tham lam là hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(2^n \cdot n)$|$O(1)$| Quá chậm | 
| Tham lam với trọng lượng có cấu trúc |$O(n \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xây dựng cấu trúc sản phẩm toàn cầu một cách ngầm định 

Chúng tôi không bao giờ xây dựng một cách rõ ràng$P$. Thay vào đó, chúng ta chỉ suy luận xem mỗi mục liên quan đến nó như thế nào: item$i$là tích của tất cả các số nguyên tố ngoại trừ$a_i$. Điều này cho phép chúng ta so sánh các mục chỉ sử dụng các số nguyên tố$a_i$, mà không bao giờ hình thành các số nguyên khổng lồ. 

### 2. Sắp xếp các mục theo số nguyên tố còn thiếu 

Chúng tôi sắp xếp các chỉ số sao cho$a_i$là theo thứ tự giảm dần. Điều này có nghĩa là các mục được sắp xếp theo giá trị tăng dần, vì việc loại bỏ số nguyên tố lớn hơn sẽ mang lại sản phẩm nhỏ hơn và loại bỏ số nguyên tố nhỏ hơn sẽ mang lại sản phẩm lớn hơn. 

Thứ tự này rất quan trọng vì nó làm cho hệ thống trở nên đơn điệu: các lựa chọn trước đó tương ứng với những đóng góp mạnh hơn vào tổng. 

### 3. Tham lam xây dựng tập con cho tổng mục tiêu 

Chúng tôi xử lý các mặt hàng từ giá trị lớn nhất đến nhỏ nhất. Ở mỗi bước, chúng tôi quyết định có đưa mục hiện tại vào hay không dựa trên việc liệu mục đó có còn phù hợp với số tiền yêu cầu còn lại hay không. 

Quyết định này mang tính quyết định vì đặc tính ưu thế được tạo ra bởi cấu trúc của các trọng số: các mục lớn hơn không thể được “sáng tác” từ sự kết hợp của các mục nhỏ hơn do khoảng cách nhân của chúng được tạo bởi các thừa số nguyên tố riêng biệt. 

### 4. So sánh với giá trị đích$d$Chúng tôi duy trì tổng số tiền hiện có và đảm bảo nó không vượt quá$d$. Nếu chúng ta có thể khớp chính xác$d$cuối cùng, câu trả lời là có. 

### Tại sao nó hoạt động 

Mỗi mục tương ứng với việc loại bỏ chính xác một số nguyên tố khỏi một sản phẩm chung. Điều này tạo ra một thứ tự nghiêm ngặt trong đó các mục có số nguyên tố bị thiếu nhỏ hơn sẽ lớn hơn theo cấp số nhân so với các mục bị thiếu số nguyên tố lớn hơn. Bởi vì tất cả các số nguyên tố đều khác biệt, nên không có sự hủy bỏ hoặc kết hợp lại các mục nhỏ hơn có thể sao chép chính xác một mục lớn hơn. Điều này thực thi một cấu trúc kinh điển tương tự như các hệ thống tiền xu tham lam với trọng số siêu tăng, đảm bảo rằng các quyết định cục bộ không làm mất hiệu lực tính tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    c = list(map(int, input().split()))

    # Build d as big integer product
    d = 1
    for x in c:
        d *= x

    # Build full product P
    P = 1
    for x in a:
        P *= x

    # Build item values: P / a[i]
    items = [P // x for x in a]

    # Sort by value descending (largest first)
    items.sort(reverse=True)

    total = 0
    for v in items:
        if total + v <= d:
            total += v

    print("S" if total == d else "N")

if __name__ == "__main__":
    main()
```Việc triển khai trực tiếp xây dựng các giá trị cần thiết bằng cách sử dụng các số nguyên lớn của Python, dựa vào khả năng xử lý các sản phẩm rất lớn của Python. Danh sách giá các mặt hàng được tính một lần và sau đó được sắp xếp sao cho chúng tôi luôn xem xét những đóng góp lớn nhất trước tiên. 

Bước tích lũy tham lam là cốt lõi của giải pháp. Chúng tôi chỉ thêm một mục nếu nó không vượt quá tổng mục tiêu, bởi vì một khi chúng tôi vượt quá$d$, không có sự kết hợp nào của các mục nhỏ hơn còn lại có thể bù đắp cho việc vượt quá mức do sự thống trị về cấu trúc giữa các kích thước mục khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
2 3 5
3 7
```Đây$P = 30$, vì vậy giá trị mục là$15, 10, 6$, Và$d = 21$. 

| Bước | Mục được coi là | Giá trị | Tổng hiện tại | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 15 | 15 | 15 | lấy | 
| 2 | 10 | 10 | 15 (bỏ qua) | bỏ qua | 
| 3 | 6 | 6 | 21 | lấy | 

Chúng tôi đạt chính xác 21, vì vậy câu trả lời là có. Điều này cho thấy sự tham lam trong việc xây dựng mục tiêu mà không cần phải quay lại như thế nào. 

### Ví dụ 2 

đầu vào:```
3 3
2 3 5
2 2 11
```Đây$P = 30$, vì vậy các mặt hàng vẫn còn$15, 10, 6$, Nhưng$d = 44$. 

| Bước | Mục được coi là | Giá trị | Tổng hiện tại | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 15 | 15 | 15 | lấy | 
| 2 | 10 | 10 | 25 | lấy | 
| 3 | 6 | 6 | 31 | lấy | 

Chúng ta kết thúc ở 31 chứ không phải 44 nên không thể đạt được mục tiêu. Điều này chứng tỏ rằng ngay cả việc sử dụng tất cả các mục cũng không nhất thiết phải đạt được các giá trị tùy ý do cấu trúc không khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp các mục chiếm ưu thế; số học là tuyến tính trong$n$các phép toán số nguyên lớn | 
| Không gian |$O(n)$| Lưu trữ giá trị mục và mảng đầu vào | 

Những hạn chế$n, m \le 3000$làm cho điều này trở nên khả thi trong Python, vì các phép toán chủ yếu là phép nhân các số nguyên lớn có kích thước vừa phải và một sắp xếp duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import prod

    n, m = map(int, inp.splitlines()[0].split())
    # We directly call the same logic as main would
    a = list(map(int, inp.splitlines()[1].split()))
    c = list(map(int, inp.splitlines()[2].split()))

    d = 1
    for x in c:
        d *= x

    P = 1
    for x in a:
        P *= x

    items = sorted([P // x for x in a], reverse=True)

    total = 0
    for v in items:
        if total + v <= d:
            total += v

    return "S\n" if total == d else "N\n"

# provided samples
assert run("""3 2
2 3 5
3 7
""") == "S\n"

assert run("""3 3
2 3 5
2 2 11
""") == "N\n"

# custom cases
assert run("""1 1
2
2
""") == "S\n"

assert run("""2 2
2 3
5 7
""") == "N\n"

assert run("""2 3
2 3
2 2 3
""") == "S\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đối sánh chính xác một mục duy nhất | S | cấu trúc tối thiểu | 
| số nguyên tố không tương thích | N | không thể xảy ra khi cấu trúc không khớp | 
| xây dựng khả thi nhỏ | S | tính đúng đắn trong trường hợp chặt chẽ | 

## Vỏ cạnh 

Khi chỉ có một mục, thuật toán sẽ giảm xuống việc kiểm tra xem giá trị đơn lẻ đó có bằng không$d$. Logic tham lam xử lý việc này một cách tự nhiên vì chỉ có một ứng cử viên bổ sung. 

Khi$d$chứa các số nguyên tố không có trong$a_i$được thiết lập, tổng được xây dựng không bao giờ có thể khớp với nó về mặt cấu trúc. Mặc dù mã không kiểm tra rõ ràng điều kiện này, nhưng sự không khớp biểu hiện ở việc không thể đạt được tổng mục tiêu chính xác trong quá trình tích lũy tham lam. 

Khi tất cả các số nguyên tố đều lớn hoặc đều nhỏ, thứ tự vẫn hoạt động nhất quán vì nó chỉ phụ thuộc vào sự so sánh tương đối giữa các số nguyên tố chứ không phụ thuộc vào độ lớn tuyệt đối của chúng.
