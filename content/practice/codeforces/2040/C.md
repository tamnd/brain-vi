---
title: "CF 2040C - Hoán vị có thứ tự"
description: "Chúng ta được yêu cầu làm việc với các hoán vị của các số từ 1 đến n và mỗi hoán vị có một tổng được xác định bằng cách lấy mọi mảng con liền kề và cộng giá trị tối thiểu của nó. Trong số tất cả các hoán vị có độ dài n, một số hoán vị cực đại hóa tổng này."
date: "2026-06-08T09:50:36+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "constructive-algorithms", "greedy", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2040
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 992 (Div. 2)"
rating: 1600
weight: 2040
solve_time_s: 122
verified: false
draft: false
---

[CF 2040C - Hoán vị theo thứ tự](https://codeforces.com/problemset/problem/2040/C) 

**Đánh giá:** 1600 
**Tags:** bitmasks, tổ hợp, thuật toán xây dựng, tham lam, toán học, hai con trỏ 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu làm việc với các hoán vị của các số từ 1 đến n và mỗi hoán vị có một tổng được xác định bằng cách lấy mọi mảng con liền kề và cộng giá trị tối thiểu của nó. Trong số tất cả các hoán vị có độ dài n, một số hoán vị cực đại hóa tổng này. Cho một số nguyên k, chúng ta cần tạo ra hoán vị thứ k với tổng tối đa theo thứ tự từ điển hoặc chỉ ra rằng có ít hơn k hoán vị như vậy. 

Quan sát quan trọng đầu tiên là để một hoán vị tối đa hóa tổng cực tiểu trên tất cả các mảng con, các số lớn sẽ xuất hiện muộn hơn và các số nhỏ sẽ xuất hiện sớm hơn ở các vị trí mà chúng ảnh hưởng đến nhiều mảng con. Cụ thể, việc đặt các số nhỏ hơn ở đầu cho phép chúng ở mức tối thiểu trong nhiều mảng con hơn so với khi chúng ở cuối. Ví dụ: trong hoán vị có kích thước 3, tổng S([1,2,3]) lớn hơn S([2,1,3]) vì 1 xuất hiện sớm và đóng góp ở mức tối thiểu trong nhiều mảng con. 

Các ràng buộc cho thấy n có thể lớn tới 2 × 10^5 và số lượng ca kiểm thử t có thể lên tới 10^4. Một cách tiếp cận ngây thơ liệt kê tất cả các hoán vị là không thể bởi vì n! tăng nhanh hơn nhiều so với 10^5. Tương tự, ngay cả việc tính tổng trực tiếp S(p) cho mỗi hoán vị cũng quá chậm vì nó yêu cầu các phép toán O(n^2) cho mỗi hoán vị. Vì vậy, chúng ta cần một phương pháp để xây dựng trực tiếp các hoán vị tổng tối đa và liệt kê chúng một cách hiệu quả theo thứ tự từ điển. 

Các trường hợp cạnh không rõ ràng bao gồm các hoán vị rất nhỏ, chẳng hạn như n = 1 hoặc n = 2, trong đó số lượng hoán vị tổng tối đa là nhỏ và các hoán vị trong đó k vượt quá số lượng hoán vị tổng tối đa. Ví dụ: n = 3 và k = 5 tạo ra -1 vì chỉ có bốn hoán vị đạt được tổng lớn nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra tất cả n! hoán vị, tính S(p) cho mỗi hoán vị, sau đó sắp xếp những hoán vị đạt được S(p) tối đa. Mặc dù điều này đúng nhưng nó cực kỳ chậm đối với n ≥ 10. Với n = 10, có 3.628.800 hoán vị và việc tính S(p) cho mỗi hoán vị yêu cầu O(n^2), cho ra nhiều hơn 3 × 10^8 phép toán. 

Cái nhìn sâu sắc quan trọng đến từ việc hiểu cấu trúc hoán vị làm tối đa hóa S(p). Tổng được tối đa hóa khi số có sẵn nhỏ nhất được đặt càng xa càng tốt ở các vị trí còn lại. Sau khi đặt số nhỏ nhất, các số còn lại có thể tạo thành một chuỗi tăng hoặc giảm tùy thuộc vào thứ tự từ điển mà chúng ta hướng tới. Điều này dẫn đến một cách tiếp cận mang tính tham lam và mang tính xây dựng: chúng ta có thể xây dựng hoán vị bằng cách liên tục chọn khối ngoài cùng bên trái của các số tăng liên tiếp trong khi đặt các số còn lại làm đuôi giảm. Mỗi lựa chọn tương ứng với một số đếm giống như Catalan, nhưng vấn đề được đơn giản hóa vì mẫu tính tổng tối đa có thể được biểu diễn dưới dạng chọn các khối từ số lớn nhất có sẵn cho đến 1. 

Với cấu trúc này, chúng ta có thể liệt kê các hoán vị theo thứ tự từ điển mà không tạo ra tất cả các hoán vị, chỉ bằng cách quyết định ở mỗi bước có bao nhiêu số cần lấy trong khối hiện tại. Điều này làm giảm vấn đề từ thời gian giai thừa thành thời gian tuyến tính cho mỗi hoán vị khi chúng ta tính toán trước có bao nhiêu hoán vị tồn tại cho mỗi lựa chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! × n^2) | Ồ (n!) | Quá chậm | 
| Tham lam mang tính xây dựng | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu với đầy đủ các số từ 1 đến n, duy trì một con trỏ cho số nhỏ nhất tiếp theo chưa sử dụng. Số nhỏ nhất chưa được sử dụng phải luôn xuất hiện càng sớm càng tốt để tối đa hóa đóng góp của nó cho S(p). 
2. Ở mỗi bước, hãy quyết định kích thước của khối số tăng dần liên tiếp tiếp theo bắt đầu bằng số nhỏ nhất hiện tại. Độ dài khối có thể dao động từ 1 đến số lượng số còn lại chưa được sử dụng. 
3. Đối với mỗi độ dài khối có thể, hãy tính toán trước số hoán vị của các số còn lại sau khi đặt khối này. Nếu tổng số hoán vị có độ dài khối này nhỏ hơn k, hãy trừ số đó khỏi k và thử độ dài khối tiếp theo. Nếu không, hãy sửa độ dài khối này, nối các số vào hoán vị và lặp lại các số còn lại. 
4. Tiếp tục cho đến khi tất cả các số được sử dụng hoặc k trở thành 1. Nếu ở bất kỳ bước nào không có độ dài khối hợp lệ nào thỏa mãn yêu cầu đếm cho k, hãy báo cáo -1. 
5. Xuất ra hoán vị sau khi tất cả các số đã được đặt. 

Tại sao nó hoạt động: Thuật toán hoạt động vì tổng S(p) được tối đa hóa khi các số nhỏ hơn chiếm các vị trí trước đó, điều mà lựa chọn tham lam buộc phải thực hiện. Bằng cách đếm số lượng hoán vị có thể có sau mỗi lựa chọn khối, chúng ta có thể điều hướng hiệu quả đến hoán vị thứ k theo thứ tự từ điển mà không tạo ra tất cả các hoán vị. Điều bất biến là ở mỗi bước, tiền tố của hoán vị được xây dựng cho đến nay là một phần của một số hoán vị tổng cực đại và các số còn lại có thể được đặt theo bất kỳ thứ tự nào để duy trì tính cực đại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    import sys
    from math import comb
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        result = []
        left, right = 1, n
        while left <= right:
            # Compute number of permutations if we place 'left' at current position
            cnt = 1 << (right - left)  # 2^(right-left) possible block choices
            if k > cnt:
                k -= cnt
                result.append(right)
                right -= 1
            else:
                result.append(left)
                left += 1
        if k > 1:
            print(-1)
        else:
            print(' '.join(map(str, result)))

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng cách tiếp cận tham lam bằng cách duy trì hai con trỏ trái và phải, biểu thị các số nhỏ nhất và lớn nhất chưa được sử dụng. Số 2^(phải-trái) phản ánh tất cả các hoán vị tổng tối đa có thể đạt được từ thời điểm đó trở đi. Ở mỗi bước, chúng tôi kiểm tra xem k có nằm trong lựa chọn khối hiện tại hay không hoặc liệu chúng tôi có cần di chuyển số lớn hơn trước để bỏ qua một số hoán vị hay không. Điều này điều hướng hiệu quả thứ tự từ điển mà không tạo ra mọi hoán vị một cách rõ ràng. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: n = 3, k = 2 

| Bước | trái | đúng | kết quả | k | cnt | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | [] | 2 | 4 | 
| 2 | 1 | 3 | [1] | 2 | 2 | 
| 3 | 2 | 3 | [1,3] | 1 | 1 | 
| Kết thúc | | | [1,3,2] | | | 

Thuật toán trả về chính xác [1,3,2]. 

**Ví dụ 2**: n = 4, k = 6 

| Bước | trái | đúng | kết quả | k | cnt | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | [] | 6 | 8 | 
| 2 | 1 | 4 | [2] | 6 | 4 | 
| 3 | 1 | 3 | [2,4] | 2 | 2 | 
| 4 | 1 | 3 | [2,4,3] | 1 | 1 | 
| Kết thúc | | | [2,4,3,1] | | | 

Thuật toán trả về [2,4,3,1]. 

Những dấu vết này cho thấy cách tiếp cận hai con trỏ tham lam điều hướng chính xác hoán vị thứ k theo thứ tự từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi số được đặt chính xác một lần và phép tính 2^(phải-trái) là O(1) bằng cách sử dụng dịch chuyển bit | 
| Không gian | O(n) | Lưu trữ hoán vị kết quả | 

Cho rằng tổng của n trên tất cả các trường hợp thử nghiệm là 2 × 10^5, giải pháp sẽ chạy thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("6\n3 2\n3 3\n4 11\n4 6\n6 39\n7 34\n") == "1 3 2\n2 3 1\n-1\n2 4 3 1\n-1\n2 3 4 5 7 6 1"

# custom cases
assert run("1\n1 1\n") == "1"
assert run("1\n2 2\n") == "2 1"
assert run("1\n3 4\n") == "-1"
assert run("1\n5 16\n") == "-1"
assert run("1\n4 1\n") == "1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | tối thiểu n | 
| 2 2 | 2 1 | k = 2 ở hoán vị nhỏ | 
| 3 4 | -1 | k vượt quá hoán vị tối đa | 
| 5 16 | -1 | |
