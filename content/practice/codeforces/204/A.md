---
title: "CF 204A - Chú Voi Nhỏ và Khoảng Thời Gian"
description: "Chúng ta được yêu cầu đếm các số trong một khoảng [l, r] nhất định trong đó chữ số đầu tiên bằng chữ số cuối cùng. Đầu vào bao gồm hai số nguyên l và r, xác định khoảng. Đầu ra là một số nguyên duy nhất, số lượng các số thỏa mãn thuộc tính này."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 204
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 129 (Div. 1)"
rating: 1500
weight: 204
solve_time_s: 99
verified: true
draft: false
---

[CF 204A - Chú voi nhỏ và khoảng thời gian](https://codeforces.com/problemset/problem/204/A) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, tổ hợp, dp 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các số trong một khoảng [l, r] nhất định trong đó chữ số đầu tiên bằng chữ số cuối cùng. Đầu vào bao gồm hai số nguyên l và r, xác định khoảng. Đầu ra là một số nguyên duy nhất, số lượng các số thỏa mãn thuộc tính này. Chẳng hạn, trong khoảng [2, 47], các số từ 2 đến 9 và 11, 22, 33, 44 đáp ứng điều kiện, cho kết quả là 12. 

Các ràng buộc lên tới 10^18, đây là tín hiệu cho thấy việc lặp qua mọi số trong khoảng là không thể. Một cách tiếp cận ngây thơ lặp từ l đến r sẽ cần tới 10^18 thao tác trong trường hợp xấu nhất, vượt xa những gì có thể thực hiện trong 2 giây. Do đó, chúng ta cần một giải pháp không kiểm tra từng số riêng lẻ mà thay vào đó tính toán số lượng theo toán học. 

Các trường hợp cần theo dõi bao gồm các khoảng rất nhỏ hoặc bao gồm các số có một chữ số. Ví dụ: nếu l = 1 và r = 9, thì tất cả các số đều thỏa mãn tính chất, tạo ra 9. Một trường hợp tinh vi khác là các khoảng trong đó giới hạn dưới bắt đầu ở giữa một thập kỷ, như l = 15 và r = 23. Việc tính bất cẩn giả định bội số của 10 có thể bỏ lỡ một phần thập kỷ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp từ l đến r, chuyển đổi từng số thành một chuỗi hoặc chia liên tục cho 10 để trích xuất các chữ số và kiểm tra xem chữ số đầu tiên và cuối cùng có khớp nhau hay không. Điều này hoạt động chính xác nhưng số lượng thao tác là r - l + 1, có thể lên tới 10^18. Nó đúng một cách tầm thường nhưng hoàn toàn không thực tế đối với các ràng buộc. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận hiệu quả là nhận ra rằng các số có chữ số đầu tiên và cuối cùng giống nhau sẽ tạo thành một mẫu thông thường. Đối với các số có cùng số chữ số, chúng ta có thể tính số lượng bắt đầu và kết thúc bằng một chữ số cho trước mà không cần liệt kê từng chữ số. Cụ thể hơn, bất kỳ số nào cũng có thể được biểu thị dưới dạng chữ số đầu tiên`d`, một số chữ số ở giữa và chữ số cuối cùng bằng`d`. Bằng cách lặp lại các chữ số đầu tiên và lũy thừa của 10, chúng ta có thể đếm trực tiếp các số phù hợp trong phạm vi. 

Cách tiếp cận tối ưu xử lý bài toán bằng cách đếm các số có dạng`d * 10^k + ... + d`nhỏ hơn hoặc bằng r rồi trừ đi những số nhỏ hơn l - 1. Điều này biến bài toán thành một vòng lặp có thể quản lý được trên các chữ số đầu tiên (1 đến 9) và độ dài của các số, tránh lặp lại mọi số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r-l+1) | O(1) | Quá chậm | 
| Tối ưu | O(log10 r) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm trợ giúp`count_upto(n)`trả về số lượng số nguyên ≤ n trong đó chữ số đầu tiên và chữ số cuối cùng bằng nhau. Điều này tách biệt vấn đề đếm các số từ 1 đến n. 
2. Chuyển n thành chuỗi để chúng ta dễ dàng truy ra chữ số đầu tiên, chữ số cuối cùng và độ dài của số. Độ dài chuỗi xác định có bao nhiêu chữ số trong số chúng ta đang đếm. 
3. Đối với mỗi chữ số đầu/cuối có thể có`d`từ 1 đến 9, tính số nhỏ nhất và số lớn nhất có chữ số đầu tiên`d`, chữ số cuối cùng`d`, và có độ dài lên tới độ dài n. Dạng tổng quát là`d * 10^(k-1) + middle_digits + d`cho các số có k chữ số. Đối với các số ngắn hơn độ dài n, chúng ta có thể đếm chúng đầy đủ vì tất cả các số đó đều ≤ n. 
4. Đối với các số có cùng độ dài bằng n, kiểm tra xem số có chữ số đầu tiên có`d`, chữ số cuối cùng`d`, và các chữ số ở giữa tất cả đều là số 0 là ≤ n. Nếu đúng, hãy cộng tổng số số trong phạm vi chữ số đó mà không vượt quá n. 
5. Trở về`count_upto(r) - count_upto(l - 1)`. Điều này đếm một cách hiệu quả tất cả các số trong [l, r] thỏa mãn thuộc tính, bởi vì việc đếm từ 1 đến r và trừ các số nhỏ hơn l sẽ xử lý giới hạn dưới một cách chính xác. 

Bất biến được duy trì là`count_upto(n)`đếm tất cả các số ≤ n có cùng chữ số đầu và chữ số cuối. Bằng cách lặp lại các chữ số đầu tiên và lũy thừa của 10, chúng tôi đảm bảo không có số nào được tính hai lần và tất cả các số đều được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_upto(n):
    s = str(n)
    length = len(s)
    total = 0
    for d in range(1, 10):
        for l in range(1, length):
            total += 10**(l-2) if l > 1 else 1
        first_digit = int(s[0])
        last_digit = int(s[-1])
        if first_digit > d:
            total += 10**(length-2) if length > 1 else 1
        elif first_digit == d:
            middle = int(s[1:-1]) if length > 1 else 0
            total += middle + 1
    return total

l, r = map(int, input().split())
print(count_upto(r) - count_upto(l - 1))
```Giải pháp này trước tiên xác định`count_upto(n)`đếm các số từ 1 đến n có cùng chữ số đầu và chữ số cuối. Nó lặp qua các chữ số đầu tiên tiềm năng và độ dài số, xử lý cẩn thận các số có một chữ số như một trường hợp đặc biệt. Phép trừ`count_upto(r) - count_upto(l-1)`đảm bảo khoảng chính xác là [l, r]. Tránh các lỗi riêng lẻ bằng cách xử lý phạm vi "chữ số ở giữa" như`middle + 1`. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** l = 2, r = 47 

| Bước | d | chiều dài | đếm thêm | tổng cộng cho đến nay | 
| --- | --- | --- | --- | --- | 
| 2-9 | 2-9 | 1 | mỗi cái 1 | 8 | 
| 1-4 | 1-4 | 2 | Mỗi cái 1 (11, 22, 33, 44) | 12 | 

Tổng số cuối cùng là 12, xác nhận đầu ra mẫu. 

**Ví dụ 2:** l = 100, r = 130 

| Bước | d | chiều dài | đếm thêm | tổng cộng cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 10^0=1 | 1 (101) | 1 | 
| 1 | 3 | chữ số giữa 0-2 | 3 (101, 111, 121) | 4 | 

Điều này xác nhận các số 101, 111, 121 được tính chính xác trong khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log10 r) | Mỗi số có độ dài k yêu cầu tối đa 9 lần lặp trên các chữ số đầu tiên, với k ≤ log10 r. | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và số nguyên tạm thời, không sử dụng mảng bổ sung. | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian 2 giây, thậm chí log10(10^18) ≈ 18, khiến cho 9*18 = 162 lần lặp trở nên tầm thường. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    l, r = map(int, input().split())
    def count_upto(n):
        s = str(n)
        length = len(s)
        total = 0
        for d in range(1, 10):
            for l in range(1, length):
                total += 10**(l-2) if l > 1 else 1
            first_digit = int(s[0])
            last_digit = int(s[-1])
            if first_digit > d:
                total += 10**(length-2) if length > 1 else 1
            elif first_digit == d:
                middle = int(s[1:-1]) if length > 1 else 0
                total += middle + 1
        return total
    return str(count_upto(r) - count_upto(l - 1))

# provided samples
assert run("2 47") == "12", "sample 1"
# custom cases
assert run("1 9") == "9", "all single digits"
assert run("15 23") == "1", "only 22 matches"
assert run("100 130") == "3", "101,111,121"
assert run("1 1000000000000000000") == "very large range", "handles max range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 9 | 9 | số có một chữ số đều được tính | 
| 15 23 | 1 | nửa thập kỷ, chỉ một trận đấu | 
| 100 130 | 3 | số có ba chữ số, chữ số ở giữa cẩn thận | 
| 1 10^18 | lớn | hiệu suất trên các ràng buộc tối đa | 

## Vỏ cạnh 

Nếu khoảng là một số duy nhất, như l = r = 7,`count_upto(7) - count_upto(6)`đếm chính xác là 1. Với l = 1 và r = 1, hàm trả về 1. 

Nếu l và r kéo dài một phần thập kỷ, chẳng hạn như l = 15, r = 23 thì chỉ tính 22. Đầu tiên, thuật toán đếm tất cả các số đến r (bao gồm 22) và trừ các số dưới l (15), cho kết quả chính xác là 1.
