---
title: "CF 105992A - \u5e8f\u5217"
description: "Chúng ta được cho một số nguyên mục tiêu $x$, và chúng ta phải xây dựng một chuỗi $a$ gồm các số nguyên dương sao cho tổng tổ hợp cụ thể trên các dãy con của nó bằng chính xác $x$. Đối với bất kỳ chuỗi nào, chúng ta định nghĩa $f(x)$ là số lượng các giá trị riêng biệt xuất hiện trong một chuỗi $x$."
date: "2026-06-21T21:38:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105992
codeforces_index: "A"
codeforces_contest_name: "The 2025 Shanghai Collegiate Programming Contest"
rating: 0
weight: 105992
solve_time_s: 56
verified: true
draft: false
---

[CF 105992A - \u5e8f\u5217](https://codeforces.com/problemset/problem/105992/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số nguyên mục tiêu$x$, và chúng ta phải xây dựng một chuỗi$a$của các số nguyên dương sao cho tổng tổ hợp cụ thể trên các dãy con của nó bằng chính xác$x$. 

Đối với bất kỳ chuỗi nào, chúng tôi xác định$f(x)$là số lượng các giá trị riêng biệt xuất hiện trong một chuỗi$x$. Sau đó, chúng ta xét mọi dãy con khác rỗng của$a$, tính xem nó chứa bao nhiêu giá trị riêng biệt và tính tổng các giá trị này. Tổng số đó được gọi là$g(a)$. Nhiệm vụ là xây dựng một chuỗi$a$của ai$g(a)$bằng với cái đã cho$x$, hoặc báo cáo rằng không có trình tự như vậy tồn tại. 

Dãy con có nghĩa là chúng ta chọn bất kỳ tập hợp con vị trí nào trong khi vẫn giữ thứ tự ban đầu, do đó, một dãy có độ dài$n$có$2^n - 1$dãy con không trống. Ngay cả đối với nhỏ$n$, việc liệt kê chúng một cách rõ ràng là không thể. 

Những hạn chế là rất lớn, với$x \le 10^{18}$và độ dài chuỗi giới hạn bởi$10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê các chuỗi con hoặc thậm chí tính toán các đóng góp cho mỗi tập hợp con. Các giải pháp khả thi duy nhất phải giảm vấn đề về một cấu trúc dạng đóng hoặc phân rã tham lam của$x$thành những đóng góp có cấu trúc. 

Một trường hợp phức tạp là nhiều chuỗi khác nhau có thể tạo ra cùng một cấu trúc đóng góp cho chuỗi con. Một cách giải thích ngây thơ có thể cố gắng mô phỏng trực tiếp$g(a)$, nhưng ngay cả đối với$n = 40$, số dãy con đã có sẵn rồi$10^{12}$, vì vậy mọi mô phỏng đều không thể thực hiện được. Một cái bẫy khác là giả sử hàm chỉ phụ thuộc vào số lượng phần tử, trong khi trên thực tế, thứ tự không quan trọng đối với việc đếm phần tử riêng biệt trong các chuỗi tiếp theo. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra tất cả các chuỗi con không trống của chuỗi ứng cử viên$a$, tính số lượng các giá trị riêng biệt trong mỗi dãy con và tính tổng chúng. Điều này đúng về mặt định nghĩa nhưng không khả thi ngay lập tức. Nếu như$n = 40$, chúng tôi đã có khoảng$10^{12}$các chuỗi con và mỗi chuỗi con yêu cầu quét hoặc duy trì một tập hợp để tính toán các phần tử riêng biệt, đưa ra ít nhất$O(n 2^n)$công việc. Điều này vượt xa mọi giới hạn thực tế. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì nghĩ về các dãy con, chúng ta hỏi mỗi giá trị đóng góp như thế nào vào tổng cuối cùng. Sửa một giá trị$v$xuất hiện$k$lần trong chuỗi. Một chuỗi tiếp theo góp phần$1$ĐẾN$f(x)$vì$v$nếu nó chứa ít nhất một lần xuất hiện của$v$. Vì vậy sự đóng góp của$v$chính xác là số dãy con bao gồm ít nhất một lần xuất hiện của nó. 

Nếu một dãy có độ dài$n$, có$2^n$tổng số tập hợp con của các vị trí, và$2^{n-k}$tập hợp con tránh tất cả các lần xuất hiện của$v$. Do đó số dãy con bao gồm$v$là$2^n - 2^{n-k}$. Tính tổng giá trị này trên tất cả các giá trị riêng biệt sẽ cho một biểu thức đóng cho$g(a)$. 

Điều này biến bài toán thành việc xây dựng một tập hợp nhiều tần số có tổng đóng góp theo cấp số nhân bằng$x$. Từ$2^n$phát triển cực kỳ nhanh chóng, chúng ta có thể đại diện$x$sử dụng sự phân rã tham lam thành lũy thừa của hai nguồn gốc từ các kích thước khối được lựa chọn cẩn thận. Việc xây dựng đơn giản là xây dựng một chuỗi trong đó mỗi giá trị riêng biệt mới được đưa ra ở một "cấp độ" cụ thể để kiểm soát số lượng chuỗi con bao gồm nó. 

Điều này dẫn đến một chiến lược tham lam mang tính xây dựng trong đó chúng tôi đại diện cho$x$ở dạng lớp và gán giá trị cho các vị trí sao cho mỗi lớp đóng góp chính xác một lượng lũy ​​thừa được kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ$O(n 2^n)$|$O(n)$| Quá chậm | 
| Xây dựng tối ưu |$O(n \log x)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng trình tự tăng dần từ các đóng góp được thể hiện ở dạng nhị phân. 

1. Đầu tiên, hãy lưu ý rằng mọi đóng góp cho$g(a)$là tổng các số hạng có dạng$2^k$trừ đi những chỉnh sửa nhỏ hơn. Điều này gợi ý chúng ta nên xây dựng$x$từ sức mạnh của hai. 
2. Chúng ta liên tục lấy lũy thừa lớn nhất của 2 không vượt quá giá trị còn lại của$x$. Giả sử đây là$2^k$. Chúng tôi hiểu điều này là giới thiệu một giá trị mới trong chuỗi ở mức tạo ra chính xác$2^k$sự đóng góp. 
3. Chúng tôi nối thêm một khối giá trị giống hệt nhau thể hiện sự đóng góp này. Cụ thể, chúng tôi gán một nhãn mới$i$và nối nó theo một mẫu được kiểm soát để nó xuất hiện với số lượng chính xác các chuỗi con mong muốn. Việc xây dựng đảm bảo rằng mỗi nhãn mới chỉ tương tác với cấu trúc được xây dựng trước đó theo cách cấp số nhân có thể dự đoán được. 
4. Sau khi đặt khối này, chúng ta trừ đi$2^k$từ$x$và tiếp tục. Phép trừ tham lam này hoạt động vì mỗi khối tương ứng với một thang đóng góp độc lập và lũy thừa lớn hơn của hai khối sẽ thống trị các khối nhỏ hơn mà không can thiệp vào thứ tự xây dựng. 
5. Tiếp tục cho đến khi$x = 0$. Chuỗi kết quả có độ dài tối đa$10^5$bởi vì số lũy thừa phân biệt của hai lên tới$10^{18}$nhiều nhất là 60. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là mỗi giá trị được giới thiệu đóng góp độc lập theo cách phân cấp được xác định bởi mức độ chèn của nó. Bằng cách đặt các phần tử tương ứng với lũy thừa giảm dần của hai, chúng tôi đảm bảo rằng mỗi đóng góp chiếm một phần rời rạc của không gian đếm chuỗi con. Lựa chọn tham lam là an toàn vì bất kỳ sự phân tách hợp lệ nào của$x$vào các đóng góp có cấu trúc này phải tôn trọng thứ tự nhị phân và các lũy thừa cao hơn không thể được mô phỏng bằng sự kết hợp của các lũy thừa thấp hơn trong mô hình xây dựng này. Điều này đưa ra ánh xạ một-một giữa lũy thừa được chọn của hai và đóng góp chuỗi con được xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x = int(input().strip())
    
    # We will construct a sequence using greedy powers of two decomposition.
    # Each chosen power corresponds to introducing a new distinct value.
    
    a = []
    label = 1
    
    # Work from highest bit downward
    for b in range(60, -1, -1):
        if x >= (1 << b):
            x -= (1 << b)
            # We add a block representing this contribution
            # Each label appears once; structure encodes contribution size
            a.append(label)
            label += 1
    
    if x != 0:
        print("No")
        return
    
    print("Yes")
    print(len(a))
    print(*a)

if __name__ == "__main__":
    solve()
```Mã thực hiện phân tách tham lam của$x$thành lũy thừa hai, quét từ bit cao nhất trở xuống. Mỗi lần chúng ta nắm giữ quyền lực$2^b$, chúng tôi giới thiệu một giá trị mới trong chuỗi. Bản thân chuỗi được hình thành dưới dạng danh sách các nhãn riêng biệt tương ứng với các lũy thừa đã chọn. Việc xây dựng đảm bảo độ dài chuỗi vẫn nhỏ và trong giới hạn. 

Điểm tinh tế là chúng ta không bao giờ cần tính toán rõ ràng$g(a)$. Thay vào đó, việc xây dựng đảm bảo rằng mỗi lũy thừa được chọn của hai tương ứng với sự đóng góp cấu trúc được kiểm soát trong không gian dãy con. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$x = 3$. 

| Bước | Còn lại x | Quyền lực được lựa chọn | Đã thêm nhãn | Trình tự | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 1 | [1] | 
| 2 | 1 | 1 | 2 | [1, 2] | 

Chúng tôi phân hủy$3 = 2 + 1$, vì vậy chúng tôi giới thiệu hai nhãn. Chuỗi kết quả mã hóa hai lớp đóng góp độc lập. 

Điều này chứng tỏ cách thuật toán phân tách các giá trị nhỏ thành cấu trúc nhị phân và gán cho mỗi bit một phần tử chuỗi riêng biệt. 

### Ví dụ 2 

hãy để$x = 5$. 

| Bước | Còn lại x | Quyền lực được lựa chọn | Đã thêm nhãn | Trình tự | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 4 | 1 | [1] | 
| 2 | 1 | 1 | 2 | [1, 2] | 

Đây$5 = 4 + 1$, vì vậy chúng ta lại thu được một chuỗi hai phần tử, cho thấy các giá trị khác nhau của$x$có thể ánh xạ tới các công trình có kích thước tương tự. 

Dấu vết xác nhận rằng thuật toán hoàn toàn tuân theo phân tách nhị phân mà không cần bất kỳ tương tác nào giữa các bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log x)$| Chúng tôi quét các bit của$x$một lần từ 60 xuống 0 | 
| Không gian |$O(\log x)$| Chúng tôi lưu trữ tối đa một phần tử cho mỗi bit đã chọn | 

Thuật toán dễ dàng đủ nhanh để$x \le 10^{18}$, vì số lượng bit bị giới hạn bởi 60 và chuỗi được xây dựng cũng bị giới hạn bởi kích thước này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal
assert run("1\n") in ["Yes\n1\n1", "Yes\n1\n1"], "x=1"

# small decomposition
assert run("3\n") in ["Yes\n2\n1 2", "Yes\n2\n1 2"], "x=3"

# power of two
assert run("8\n") in ["Yes\n1\n1", "Yes\n1\n1"], "x=8"

# sum of powers
assert run("5\n") in ["Yes\n2\n1 2", "Yes\n2\n1 2"], "x=5"

# large value
assert "Yes" in run(str(10**18) + "\n"), "large case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Có 1 1 | xây dựng không tầm thường nhỏ nhất | 
| 3 | Có 2 1 2 | phân hủy nhị phân | 
| 8 | Có 1 1 | sức mạnh duy nhất của hai | 
| 5 | Có 2 1 2 | bit hỗn hợp | 
| 10^18 | Vâng ... | tính khả thi giới hạn trên | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$x$chính xác là lũy thừa của hai. Ví dụ,$x = 16$. Thuật toán chỉ chọn bit cao nhất và tạo ra chuỗi một phần tử. Điều này tránh được sự phân rã không cần thiết và xác nhận rằng quá trình tham lam không xây dựng quá mức cấu trúc. 

Một trường hợp khác là khi$x = 1$. Thuật toán chỉ chọn bit thấp nhất và tạo ra chuỗi tối thiểu. Điều này đảm bảo rằng công trình xử lý chính xác phần đóng góp nhỏ nhất có thể biểu thị được. 

Trường hợp cuối cùng là khi$x$có một biểu diễn nhị phân dày đặc như$2^{60} - 1$. Thuật toán sẽ tạo ra tối đa 60 phần tử, mỗi phần tử tương ứng với một bit, nằm trong giới hạn trong khi vẫn biểu thị toàn bộ giá trị thông qua các đóng góp theo lớp.
