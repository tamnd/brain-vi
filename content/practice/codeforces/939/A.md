---
title: "CF 939A - Tam giác tình yêu"
description: "Chúng ta có một cấu trúc có hướng trên $n$ nút, trong đó mỗi nút có chính xác một cạnh đi ra. Cụ thể, mặt phẳng $i$ trỏ tới mặt phẳng $fi$, nghĩa là nó “thích” chính xác một mặt phẳng khác. Tự vòng lặp rõ ràng không được phép, do đó không có nút nào trỏ đến chính nó."
date: "2026-06-17T02:35:48+07:00"
tags: ["codeforces", "competitive-programming", "graphs"]
categories: ["algorithms"]
codeforces_contest: 939
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 464 (Div. 2)"
rating: 800
weight: 939
solve_time_s: 68
verified: true
draft: false
---

[CF 939A - Tam giác tình yêu](https://codeforces.com/problemset/problem/939/A) 

**Đánh giá:** 800 
**Thẻ:** đồ thị 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một cấu trúc định hướng trên$n$các nút, trong đó mỗi nút có chính xác một cạnh đi ra. Cụ thể, mặt phẳng$i$trỏ vào mặt phẳng$f_i$, nghĩa là nó “thích” chính xác một mặt phẳng khác. Tự vòng lặp rõ ràng không được phép, do đó không có nút nào trỏ đến chính nó. 

Nhiệm vụ là xác định xem có tồn tại một chu trình có hướng có độ dài chính xác bằng ba hay không. Nói cách khác, chúng ta đang tìm kiếm ba mặt phẳng phân biệt$A, B, C$như vậy$A \to B$,$B \to C$, Và$C \to A$. 

Cấu trúc này là một đồ thị hàm số, vì mỗi nút đều có mức độ ngoài 1. Điều đó ngay lập tức ngụ ý rằng mọi thành phần được kết nối đều bao gồm một chu trình với các cây được định hướng đi vào nó. Tuy nhiên, chỉ có chu kỳ có độ dài 3 mới quan trọng ở đây. 

Ràng buộc$n \le 5000$đủ nhỏ để một$O(n^2)$hoặc thậm chí đơn giản$O(n)$quét với kiểm tra liên tục trên mỗi nút là đủ. Bất cứ điều gì khối hoặc liên quan đến việc truyền tải lặp đi lặp lại trên mỗi nút vẫn sẽ vượt qua, nhưng không cần thiết. 

Một trường hợp khó phát hiện xuất phát từ việc hiểu sai hướng truyền tải. Vì mỗi nút trỏ đến chính xác một nút, nhưng nhiều nút có thể trỏ đến cùng một mục tiêu, nên việc suy nghĩ theo các thuật ngữ vô hướng hoặc giả định tính đối xứng sẽ phá vỡ tính chính xác. 

Một trường hợp cạnh khác là giả định rằng bất kỳ chu kỳ nào cũng đủ điều kiện. Ví dụ: một chu kỳ có độ dài 2 như$1 \to 2 \to 1$không hợp lệ. Chỉ tính chiều dài 3 chu kỳ. 

Một ví dụ cụ thể về một cái bẫy: 

đầu vào:```
3
2 1 2
```Hình thức này$1 \to 2 \to 1$, Và$3 \to 2$. Có một chu kỳ nhưng có độ dài 2, vì vậy đầu ra đúng là:```
NO
```Phương pháp phát hiện chu kỳ đơn giản dừng lại ở “chu kỳ tồn tại” sẽ trả lời sai CÓ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi bộ ba được đặt hàng$(i, j, k)$và kiểm tra xem$i \to j$,$j \to k$, Và$k \to i$. Điều này có thể thực hiện đơn giản bằng cách tra cứu trực tiếp trong mảng$f$. Tuy nhiên, điều này đòi hỏi$O(n^3)$kiểm tra, vì có$n^3$có thể gấp ba lần và mỗi lần kiểm tra là thời gian không đổi. Với$n = 5000$, đây là theo thứ tự của$1.25 \times 10^{11}$kiểm tra, vượt xa thời hạn. 

Chúng ta có thể loại bỏ việc tìm kiếm không cần thiết bằng cách quan sát cấu trúc của biểu đồ. Vì mỗi nút có chính xác một cạnh đi ra, bắt đầu từ bất kỳ nút nào$i$, chỉ có một chuỗi có thể theo sau:$i \to f_i \to f_{f_i} \to \dots$. Nếu tồn tại 3 chu kỳ thì nó phải xuất hiện trong cấu trúc chuỗi xác định này. 

Điều này gợi ý kiểm tra trực tiếp trên mỗi nút. Đối với mỗi nút$i$, chúng ta có thể định nghĩa: 

-$a = f_i$-$b = f_a$-$c = f_b$Sau đó chúng ta chỉ cần xác minh xem$c = i$. Nếu vậy, chúng ta đã tìm được 3 chu trình bao gồm$i$,$a$, Và$b$. Bởi vì mỗi nút có chính xác một cạnh đi ra nên mỗi 3 chu kỳ tiềm năng sẽ được phát hiện bằng cách bắt đầu từ một trong các nút của nó. 

Điều này làm giảm vấn đề xuống$O(n)$séc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả gấp ba) |$O(n^3)$|$O(1)$| Quá chậm | 
| Kiểm tra 3 bước trực tiếp trên mỗi nút |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng$f$, điều chỉnh lập chỉ mục dựa trên 0 nếu cần. Điều này đảm bảo chúng ta có thể truy cập những người kế nhiệm trong thời gian không đổi. 
2. Đối với mỗi nút$i$, tính bước nhảy đầu tiên, thứ hai và thứ ba của nó:$a = f[i]$,$b = f[a]$, Và$c = f[b]$. Điều này mô phỏng các cạnh được định hướng chính xác ba lần. 
3. Kiểm tra xem$c == i$. Nếu điều này đúng, chúng ta đã đóng một chu trình có hướng có độ dài 3 bắt đầu và kết thúc tại$i$. 
4. Nếu có như vậy$i$thỏa mãn điều kiện thì xuất ngay YES. Việc thoát sớm này là hợp lệ vì sự tồn tại của một tam giác hợp lệ là đủ. 
5. Nếu không có nút nào tạo ra kết quả trả về 3 bước hợp lệ, xuất ra NO. 

### Tại sao nó hoạt động 

Mỗi nút có chính xác một cạnh đi ra, vì vậy mỗi chu kỳ có độ dài 3 phải được biểu diễn duy nhất bởi nút bắt đầu của nó. Nếu một chu kỳ$A \to B \to C \to A$tồn tại thì bắt đầu từ$A$chắc chắn sẽ tạo ra$f_A = B$,$f_B = C$, Và$f_C = A$, làm cho việc kiểm tra thành công. Vì tất cả các quá trình chuyển đổi đều mang tính xác định nên không thể bỏ sót chu kỳ nào và không thể xảy ra kết quả dương tính giả do điều kiện thực thi rõ ràng việc đóng sau đúng ba bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
f = list(map(int, input().split()))

# convert to 0-based indexing
for i in range(n):
    f[i] -= 1

for i in range(n):
    a = f[i]
    b = f[a]
    c = f[b]
    if c == i:
        print("YES")
        sys.exit()

print("NO")
```Chi tiết triển khai chính là chuyển đổi ngay lập tức sang lập chỉ mục dựa trên 0. Điều này tránh việc điều chỉnh -1 lặp đi lặp lại trong quá trình truyền tải và giữ cho việc truy cập mảng luôn rõ ràng. 

Một điểm quan trọng khác là thoát sớm. Khi tìm thấy một tam giác hợp lệ, chúng tôi chấm dứt ngay lập tức vì bài toán chỉ yêu cầu sự tồn tại chứ không yêu cầu liệt kê. 

Logic nhảy ba lần phản ánh trực tiếp điều kiện toán học$f(f(f(i))) = i$, đây là thuộc tính xác định của 3 chu trình trong đồ thị hàm số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 4 5 1 3
```Chúng tôi theo dõi từng nút: 

| tôi | a = f[i] | b = f[a] | c = f[b] | c == tôi | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 0 | CÓ | 

Tại$i = 0$, chúng tôi nhận được$0 \to 1 \to 3 \to 0$, tạo thành 3 chu kỳ hợp lệ. 

Điều này xác nhận thuật toán xác định chính xác một chu trình khi nó tồn tại. 

### Ví dụ 2 

đầu vào:```
3
2 1 2
```| tôi | a = f[i] | b = f[a] | c = f[b] | c == tôi | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | KHÔNG | 
| 1 | 0 | 1 | 0 | KHÔNG | 
| 2 | 1 | 0 | 1 | KHÔNG | 

Không có nút khởi đầu nào quay về chính nó theo đúng ba bước, do đó không tồn tại 3 chu kỳ. 

Điều này chứng tỏ rằng các chu kỳ ngắn hơn (độ dài 2 ở đây) không tạo ra kết quả dương tính giả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nút thực hiện chính xác ba lần tra cứu mảng và một lần so sánh | 
| Không gian |$O(n)$| Lưu trữ đồ thị hàm số trong một mảng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$n \le 5000$và tính toán là tuyến tính với hệ số không đổi rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    f = list(map(int, input().split()))
    for i in range(n):
        f[i] -= 1

    for i in range(n):
        a = f[i]
        b = f[a]
        c = f[b]
        if c == i:
            return "YES"
    return "NO"

# provided sample
assert run("5\n2 4 5 1 3\n") == "YES"

# no cycle of length 3
assert run("3\n2 1 2\n") == "NO"

# minimal n
assert run("2\n2 1\n") == "NO"

# simple 3-cycle
assert run("3\n2 3 1\n") == "YES"

# larger with embedded triangle
assert run("4\n2 3 1 2\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2 4 5 1 3 | CÓ | cơ bản hợp lệ 3 chu kỳ | 
| 3 2 1 2 | KHÔNG | chỉ tồn tại 2 chu kỳ | 
| 2 2 1 | KHÔNG | kích thước tối thiểu, không thể có hình tam giác | 
| 3 2 3 1 | CÓ | trực tiếp 3 chu kỳ | 
| 4 2 3 1 2 | CÓ | tam giác được nhúng trong biểu đồ lớn hơn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi biểu đồ chứa các chu trình nhưng không có chu trình nào có độ dài 3. Ví dụ: một chu trình 2 thuần túy như:```
3
2 1 2
```Bắt đầu từ mỗi nút, thuật toán tính toán chính xác ba lần chuyển đổi. Đối với nút 1:$1 \to 2 \to 1 \to 2$, không trở về 1, vì vậy nó bị từ chối một cách chính xác. Điều tương tự cũng xảy ra với tất cả các nút, vì vậy đầu ra là KHÔNG. 

Một trường hợp cạnh khác là khi nhiều nút nạp vào 3 chu kỳ hợp lệ. Thuật toán vẫn hoạt động vì nó không dựa vào khả năng tiếp cận hoặc khả năng kết nối mà chỉ dựa vào thành phần chức năng trực tiếp. Đối với bất kỳ nút nào trong chu trình, bước nhảy ba lần sẽ ở trong chu trình và quay trở lại chính xác, đảm bảo phát hiện ngay cả khi chu trình không bị cô lập.
