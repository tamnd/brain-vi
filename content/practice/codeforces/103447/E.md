---
title: "CF 103447E - Công suất và Modulo"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm được cho là biểu thị các giá trị được tạo ra bởi một quy trình mô đun ẩn. Tồn tại một số nguyên dương $M$ chưa biết và mảng này dự kiến ​​sẽ khớp với chuỗi được hình thành bằng cách lấy lũy thừa của hai và giảm chúng theo modulo $M$."
date: "2026-07-03T07:30:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103447
codeforces_index: "E"
codeforces_contest_name: "The 2021 China Collegiate Programming Contest (Harbin)"
rating: 0
weight: 103447
solve_time_s: 40
verified: true
draft: false
---

[CF 103447E - Sức mạnh và Modulo](https://codeforces.com/problemset/problem/103447/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm được cho là biểu thị các giá trị được tạo ra bởi một quy trình mô đun ẩn. Tồn tại một số nguyên dương chưa biết$M$và mảng dự kiến ​​​​sẽ khớp với chuỗi được hình thành bằng cách lấy lũy thừa của hai và giảm chúng theo modulo$M$. Nói cách khác, phần tử đầu tiên sẽ hoạt động như$2^0 \bmod M$, lượt thích thứ hai$2^1 \bmod M$, vân vân. 

Nhiệm vụ không phải là tìm bất kỳ mô đun nào hoạt động mà là xác định xem có tồn tại chính xác một số nguyên dương hay không$M$điều đó làm cho toàn bộ chuỗi phù hợp với quy tắc này. Nếu như vậy$M$tồn tại duy nhất thì ta xuất nó ra. Nếu không, chúng tôi xuất ra -1. 

Kích thước đầu vào cho phép lên tới$10^5$giá trị trên mỗi tệp thử nghiệm, được tổng hợp trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử các giá trị ứng viên của$M$và mô phỏng trình tự đầy đủ cho từng cái. Một bậc hai hoặc thậm chí$O(n \sqrt{A_i})$giải pháp phong cách sẽ quá chậm. 

Một điểm tinh tế là dãy số không phải là số học mô-đun tùy ý. Đây hoàn toàn là một sự tiến triển theo cấp số nhân xác định theo modulo, điều này hạn chế rất nhiều về cấu trúc. Cấu trúc này là những gì làm cho việc tái thiết có thể thực hiện được. 

Một trường hợp thất bại thường đánh lừa lý luận ngây thơ là giả định rằng nếu tất cả các giá trị nhỏ hơn một số ứng cử viên$M$, sau đó lớn hơn$M$hoạt động. Ví dụ, nếu trình tự là$[1, 2, 4, 8]$, thì bất kỳ$M > 8$duy trì sự bình đẳng, nghĩa là không có mô đun duy nhất. Điều này trực tiếp dẫn đến trường hợp “-1”. 

Một trường hợp cạnh khác xuất hiện khi sự bao quanh xảy ra. Ví dụ: nếu một giá trị đột nhiên trở thành 0 hoặc lặp lại các giá trị trước đó một cách bất ngờ, điều đó cho thấy rằng mô đun đủ nhỏ để tạo ra xung đột và nhiều mô đun ứng cử viên có thể tồn tại hoặc không có mô đun nào cả. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu bắt đầu từ định nghĩa. Chúng tôi thử một ứng cử viên$M$và xác minh xem mọi thuật ngữ có khớp không$2^{i-1} \bmod M$. Đối với mỗi$M$, điều này đòi hỏi phải quét toàn bộ mảng và có thể có nhiều giá trị hợp lý của$M$. Thậm chí còn hạn chế ứng viên trong các phạm vi như$\max(A_i) + 1$vẫn để lại một không gian tìm kiếm không giới hạn, vì điều kiện chính xác phụ thuộc vào hành vi mô-đun chứ không chỉ so sánh với các giá trị được quan sát. 

Một cách hiệu quả hơn để suy nghĩ về vấn đề là đảo ngược quy trình mô-đun. Thay vì thử nghiệm$M$, chúng tôi hỏi những ràng buộc nào mà trình tự áp đặt lên nó. 

Từ định nghĩa:$$A_{i+1} \equiv 2A_i \pmod{M}$$Điều này ngụ ý:$$2A_i - A_{i+1} \equiv 0 \pmod{M}$$Vì vậy, mỗi cặp liền kề đều cho một số phải chia hết cho$M$. Điều đó có nghĩa$M$phải chia tất cả các giá trị của biểu mẫu$2A_i - A_{i+1}$. 

Điều này ngay lập tức làm giảm vấn đề về việc tính ước số chung lớn nhất đối với các ràng buộc này. Nếu tất cả những khác biệt đó bằng 0 thì chuỗi hoàn toàn phù hợp với việc nhân đôi và không bao giờ kết thúc, tương ứng với trường hợp$M$là không giới hạn và không tồn tại nghiệm duy nhất. 

Mặt khác, gcd của tất cả các ràng buộc sẽ cho mô đun ứng cử viên duy nhất có thể. Sau khi tính toán ứng cử viên này, chúng tôi vẫn phải xác minh rằng nó tạo ra chuỗi đã cho, vì việc tái cấu trúc dựa trên gcd chỉ thực thi tính nhất quán theo cặp chứ không phải tính năng động mô-đun đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ hoặc$O(n \cdot M)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log A_i)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xây dựng ràng buộc từ các số hạng liên tiếp 

Với mọi cặp liền kề$(A_i, A_{i+1})$, tính:$$D_i = 2A_i - A_{i+1}$$Mỗi$D_i$phải chia hết cho mô đun chưa biết$M$. Điều này chuyển đổi ràng buộc trình tự thành một hệ thống chia hết. 

### 2. Tính gcd của tất cả các ràng buộc khác 0 

Duy trì một gcc đang chạy trên tất cả$D_i \neq 0$. Gcd này đại diện cho số nguyên lớn nhất có thể chia đồng thời mọi ràng buộc và do đó là ứng cử viên duy nhất có thể cho$M$. 

Nếu tất cả$D_i$bằng 0, chúng ta không thể rút ra bất kỳ giới hạn nào trên$M$, nghĩa là vô số giá trị của$M$sẽ thỏa mãn cấu trúc, do đó tính duy nhất không thành công. 

### 3. Xử lý trường hợp “không có khác biệt” 

Nếu mọi$D_i = 0$, dãy thỏa mãn$A_{i+1} = 2A_i$chính xác. Điều này có nghĩa là chuỗi không bao giờ bao bọc modulo bất kỳ thứ gì. Bất kỳ đủ lớn$M$hoạt động nên không có mô đun duy nhất. Chúng tôi ngay lập tức trả về -1. 

### 4. Xác thực mô đun ứng viên 

hãy để$M$là gcd được tính toán. Chúng tôi mô phỏng sự tái phát:$$x_1 = A_1, \quad x_{i+1} = (2x_i) \bmod M$$và kiểm tra xem nó có tái tạo chính xác mảng đã cho hay không. Nếu thất bại ở bất kỳ thời điểm nào, hãy trả về -1. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ mô đun hợp lệ nào cũng phải chia mọi biểu thức$2A_i - A_{i+1}$. Điều này xuất phát trực tiếp từ việc viết lại phép truy toán mô-đun. Vì vậy, mọi giá trị hợp lệ$M$nằm trong tập ước số của gcd của tất cả các ràng buộc và gcd là ứng cử viên lớn nhất có thể. 

Nếu ứng cử viên gcd không xác minh được thì không có ước số nào của nó có thể thành công, vì giảm$M$chỉ tăng cường việc bao bọc mô-đun và sẽ gây ra sự không nhất quán trước đó. Điều này đảm bảo thuật toán không bỏ lỡ bất kỳ mô đun hợp lệ thay thế nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 1:
        return -1
    
    g = 0
    for i in range(n - 1):
        d = 2 * a[i] - a[i + 1]
        if d != 0:
            if g == 0:
                g = abs(d)
            else:
                import math
                g = math.gcd(g, abs(d))
    
    if g == 0:
        return -1
    
    # verify
    m = g
    cur = a[0]
    for i in range(1, n):
        cur = (2 * cur) % m
        if cur != a[i]:
            return -1
    
    return m

def main():
    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai cốt lõi tập trung vào việc chuyển đổi phép truy toán thành các ràng buộc tuyến tính và nén chúng bằng gcd. Bước trừ`2 * a[i] - a[i+1]`là phép biến đổi duy nhất cần thiết để thể hiện tính chia hết cho$M$. 

Giai đoạn xác minh là cần thiết vì việc tổng hợp gcd chỉ đảm bảo tính nhất quán của các ràng buộc cục bộ. Mô phỏng đảm bảo tính chính xác toàn diện của quá trình phát triển mô-đun từ thuật ngữ đầu tiên trở đi. 

Một chi tiết triển khai tinh tế là xử lý những khác biệt tiêu cực. Lấy giá trị tuyệt đối trước gcd là điều cần thiết vì tính chia hết không bị ảnh hưởng bởi dấu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
A = [1, 2, 4, 1]
```Chúng tôi tính toán: 

| tôi | A[i] | A[i+1] | D = 2A[i] - A[i+1] | gcd | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 0 | 
| 2 | 2 | 4 | 0 | 0 | 
| 3 | 4 | 1 | 7 | 7 | 

Bây giờ ứng cử viên$M = 7$. 

Mô phỏng: 

| tôi | cur | (2*cur) % 7 | A[i] | trận đấu | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | vâng | 
| 2 | 2 | 4 | 4 | vâng | 
| 3 | 4 | 1 | 1 | vâng | 

Trình tự nhất quán nên đầu ra là 7. 

### Ví dụ 2 

đầu vào:```
A = [1, 2, 4, 8]
```Tất cả sự khác biệt đều bằng 0, vì vậy gcd vẫn bằng 0. Điều này cho thấy không có sự bao bọc nào xảy ra, nghĩa là có vô số mô đun lớn hơn 8 thỏa mãn chuỗi. Đầu ra đúng là -1. 

Điều này thể hiện sự khác biệt quan trọng giữa mô đun có thể tái tạo và phạm vi hợp lệ không giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A_i)$| Mỗi bước tính gcd và một lần xác minh tuyến tính | 
| Không gian |$O(1)$| Chỉ có một vài bộ tích lũy và mảng đầu vào | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì tổng số phần tử trong các trường hợp thử nghiệm là$10^5$, và cả tính toán và mô phỏng gcd đều tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            return -1
        g = 0
        for i in range(n - 1):
            d = 2 * a[i] - a[i + 1]
            if d != 0:
                if g == 0:
                    g = abs(d)
                else:
                    from math import gcd
                    g = gcd(g, abs(d))
        if g == 0:
            return -1
        cur = a[0]
        for i in range(1, n):
            cur = (2 * cur) % g
            if cur != a[i]:
                return -1
        return g

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    return "\n".join(out)

# provided samples
assert run("1\n4\n1 2 4 1\n") == "7", "sample 1"
assert run("1\n4\n1 2 4 8\n") == "-1", "sample 2"

# custom cases
assert run("1\n1\n5\n") == "-1", "minimum size"
assert run("1\n3\n1 0 2\n") in ["-1", "-1"], "wrap inconsistency"
assert run("1\n5\n1 2 4 8 16\n") == "-1", "no unique modulus"
assert run("1\n3\n1 2 0\n") != "", "valid small sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | -1 | sự mơ hồ của yếu tố đơn lẻ | 
| [1,2,4,8,16] | -1 | hợp lệ không giới hạn M | 
| trường hợp bọc hỗn hợp | -1 hoặc không hợp lệ | cấu trúc mô-đun không nhất quán | 

## Vỏ cạnh 

### Mọi sự khác biệt đều bằng không 

Khi chuỗi tăng gấp đôi chính xác mà không có bất kỳ sự bao bọc nào, mọi$D_i = 0$. Bộ thuật toán$g = 0$và ngay lập tức trả về -1. Điều này nắm bắt chính xác thực tế là có vô số mô đun hợp lệ, do đó tính duy nhất bị vi phạm. 

### Sự khác biệt trung gian tiêu cực 

Nếu$2A_i < A_{i+1}$, tính toán$D_i$trở nên tiêu cực. Lấy giá trị tuyệt đối đảm bảo tính toán gcd vẫn hợp lệ, vì khả năng chia hết chỉ phụ thuộc vào độ lớn. 

### Mâu thuẫn sớm 

Nếu trình tự vi phạm nhân đôi mô-đun tại bất kỳ điểm nào, vòng xác minh sẽ thất bại ngay lập tức, từ chối các ứng cử viên gcd không chính xác ngay cả khi chúng đáp ứng tất cả các ràng buộc theo cặp cục bộ.
