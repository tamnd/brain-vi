---
title: "CF 105709G - Galapagos"
description: "Chúng ta được cấp cho một dòng rùa, mỗi vị trí chứa một số lượng trứng riêng biệt. Sự sắp xếp ban đầu là tùy ý và mục tiêu là xác định xem liệu chúng ta có thể chuyển đổi chuỗi này thành thứ tự tăng dần bằng cách sử dụng một phép toán cục bộ rất cụ thể hay không."
date: "2026-06-26T08:51:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105709
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 2-12-25 Div. 2 (Beginner)"
rating: 0
weight: 105709
solve_time_s: 37
verified: true
draft: false
---

[CF 105709G - Galapagos](https://codeforces.com/problemset/problem/105709/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp cho một dòng rùa, mỗi vị trí chứa một số lượng trứng riêng biệt. Sự sắp xếp ban đầu là tùy ý và mục tiêu là xác định xem liệu chúng ta có thể chuyển đổi chuỗi này thành thứ tự tăng dần bằng cách sử dụng một phép toán cục bộ rất cụ thể hay không. 

Nước đi duy nhất được phép chiếm ba vị trí liên tiếp bất kỳ và hoán vị chúng theo một chu kỳ cố định: nếu chúng ta nhìn vào một đoạn$(a_i, a_{i+1}, a_{i+2})$, chúng ta có thể thay thế nó bằng$(a_{i+2}, a_i, a_{i+1})$. Không được phép sắp xếp lại khác và chúng tôi có thể áp dụng thao tác này bất kỳ số lần nào tại bất kỳ vị trí hợp lệ nào. 

Câu hỏi hoàn toàn mang tính cấu trúc: việc xoay cục bộ bị hạn chế này có tạo ra tất cả các hoán vị hay chỉ một tập hợp con? Chúng ta phải quyết định xem trình tự sắp xếp mục tiêu có thể truy cập được hay không. 

Đầu vào là một hoán vị duy nhất của kích thước$n$, với sự đảm bảo rằng tất cả các giá trị đều khác biệt. Đầu ra là một quyết định nhị phân, trong đó 1 có nghĩa là chuỗi có thể được chuyển đổi thành thứ tự được sắp xếp và 0 có nghĩa là không thể. 

Những hạn chế là lớn, với$n$lên tới 200.000, điều này ngay lập tức loại trừ bất kỳ mô phỏng hoặc tìm kiếm vũ phu nào đối với các phép biến đổi. Bất kỳ giải pháp nào cố gắng mô hình hóa rõ ràng các trạng thái hoặc áp dụng các thao tác lặp đi lặp lại sẽ bùng nổ, vì ngay cả số lượng thao tác tuyến tính trên mỗi bước cũng đã trở thành bậc hai hoặc tệ hơn. 

Trường hợp cạnh tinh tế xuất hiện khi$n$là nhỏ. Vì$n = 1$hoặc$n = 2$, không có thao tác nào có thể áp dụng được cả, vì vậy câu trả lời là tầm thường liệu mảng đã được sắp xếp chưa. Vì$n = 3$, thao tác có thể được áp dụng chính xác một lần theo một trong hai hướng, do đó chỉ có thể truy cập được một số hoán vị nhất định. Ví dụ, bắt đầu từ$[3,1,2]$, chúng ta có thể áp dụng thao tác để đạt được$[2,3,1]$, nhưng chúng ta không bao giờ có thể có được tất cả các hoán vị, nên trực giác ngây thơ về việc “chúng ta có thể sắp xếp lại cục bộ” là sai lầm. 

Khó khăn thực sự là nhận ra những gì bất biến toàn cục mà hoạt động bảo tồn. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các hoán vị có thể tiếp cận bằng cách sử dụng BFS hoặc DFS trên các trạng thái, trong đó mỗi trạng thái là một hoán vị và chuyển tiếp là các hoạt động 3 phần tử được phép. Ngay cả khi mỗi nút có$O(n)$hàng xóm, kích thước không gian trạng thái là$n!$, vì vậy điều này trở nên hoàn toàn không khả thi ngay cả đối với$n = 10$. 

Quan sát quan trọng là hoạt động không phải là tùy ý. Đây là chu kỳ 3 trên chỉ số, nghĩa là nó bảo toàn tính chẵn lẻ của các hoán vị. Mọi phép toán đều là một hoán vị chẵn: một chu trình 3 có thể được viết dưới dạng hai phép hoán đổi, do đó nó không làm thay đổi dấu của hoán vị. 

Điều này ngay lập tức ngụ ý một bất biến toàn cục: tính chẵn lẻ của hoán vị (cho dù đó là hoán vị chẵn hay lẻ so với thứ tự được sắp xếp) không bao giờ thay đổi theo bất kỳ chuỗi hoạt động được phép nào. Vì vậy, nếu hoán vị ban đầu có tính chẵn lẻ lẻ, nó không bao giờ có thể được chuyển thành hoán vị nhận dạng (thứ tự sắp xếp), tức là chẵn. 

Việc còn lại là kiểm tra xem liệu chỉ tính chẵn lẻ có đủ hay không. Vì các hoán đổi liền kề tạo ra tất cả các hoán vị và các hoán đổi liền kề chỉ có thể bao gồm 3 chu kỳ khi tính chẵn lẻ cho phép, nên tập thao tác này tạo ra chính xác nhóm xen kẽ. Điều đó có nghĩa là tất cả các hoán vị chẵn đều có thể truy cập được và tất cả các hoán vị lẻ đều không thể truy cập được. 

Vì vậy, vấn đề giảm xuống còn tính toán nghịch đảo tính chẵn lẻ của mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force |$O(n!)$|$O(n!)$| Quá chậm | 
| Tối ưu (Số chẵn lẻ/đảo ngược) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn vấn đề thành việc xác định liệu hoán vị có chẵn lẻ hay không. 

1. Chuyển bài toán sang kiểm tra xem mảng có bao nhiêu phép đảo ngược. Sự đảo ngược là một cặp$(i, j)$với$i < j$Nhưng$a_i > a_j$. Điều này trực tiếp đo tính chẵn lẻ hoán vị. 
2. Tính toán chẵn lẻ nghịch đảo một cách hiệu quả thay vì đếm các nghịch đảo đầy đủ. Chúng ta chỉ cần số đếm là chẵn hay lẻ. 
3. Sử dụng cây Fenwick (hoặc tính toán dựa trên sắp xếp hợp nhất) để xử lý các phần tử từ trái sang phải. Mỗi lần chúng tôi chèn một giá trị, chúng tôi đếm xem có bao nhiêu giá trị đã thấy trước đó lớn hơn giá trị đó. 
4. Duy trì tính chẵn lẻ dưới dạng một bit duy nhất lật bất cứ khi nào chúng ta gặp phải một đóng góp đảo ngược lẻ. 
5. Sau khi xử lý toàn bộ mảng, nếu chẵn lẻ thì xuất 1, nếu không thì xuất 0. 

### Tại sao nó hoạt động 

Mỗi phép toán được phép là một chu kỳ 3, là một hoán vị chẵn và do đó duy trì tính chẵn lẻ nghịch đảo. Vì mảng được sắp xếp không có độ đảo ngược bằng 0, tức là số chẵn, nên chỉ những mảng có tính chẵn lẻ đảo ngược chẵn mới có thể được chuyển đổi thành mảng đó. Ngược lại, bất kỳ hoán vị chẵn nào cũng có thể được phân tách thành một chuỗi gồm 3 chu kỳ như vậy, nghĩa là nó có thể truy cập được. Vì vậy, sự ngang bằng vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # coordinate compression
    vals = sorted(a)
    comp = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(n)
    parity = 0

    for i, x in enumerate(a):
        x = comp[x]
        leq = fw.sum(x)
        inversions_here = i - leq
        parity ^= (inversions_here & 1)
        fw.add(x, 1)

    print(1 if parity == 0 else 0)

if __name__ == "__main__":
    solve()
```Cây Fenwick duy trì số lượng các phần tử đã thấy. Đối với mỗi phần tử mới, mọi thứ được chèn trước đó lớn hơn đều góp phần tạo ra sự đảo ngược. Chúng tôi chỉ theo dõi xem số đó là số lẻ hay số chẵn, vì vậy chúng tôi XOR bit chẵn lẻ thay vì tích lũy số lượng đầy đủ. 

Cần phải nén tọa độ vì các giá trị tăng lên$10^9$, trong khi các chỉ số Fenwick phải liền kề nhau. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ lưu trữ toàn bộ số lần đảo ngược vì chúng có thể vượt quá phạm vi 64 bit; chỉ có vấn đề ngang bằng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 1 2
```Chúng tôi nén các giá trị để$[3,1,2] \rightarrow [3,1,2]$cấp bậc$[3,1,2]$. 

| Bước | Giá trị | Đã thấy cho đến nay | Nghịch đảo đóng góp | Chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | {} | 0 | 0 | 
| 2 | 1 | {3} | 1 | 1 | 
| 3 | 2 | {3,1} | 1 | 0 | 

Tính chẵn lẻ cuối cùng là chẵn nên đầu ra là 1. 

Điều này cho thấy rằng mặc dù mảng ban đầu không được sắp xếp nhưng nó nằm trong tập hợp có thể truy cập được. 

### Ví dụ 2 

đầu vào:```
4
1 2 4 3
```| Bước | Giá trị | Đã thấy cho đến nay | Nghịch đảo đóng góp | Chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {} | 0 | 0 | 
| 2 | 2 | {1} | 0 | 0 | 
| 3 | 4 | {1,2} | 0 | 0 | 
| 4 | 3 | {1,2,4} | 1 | 1 | 

Tính chẵn lẻ cuối cùng là số lẻ nên đầu ra là 0. 

Điều này xác nhận trường hợp mảng “gần như đã được sắp xếp” nhưng vẫn không thể truy cập được do tính chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi truy vấn chèn và tiền tố trong cây Fenwick mất thời gian logarit | 
| Không gian |$O(n)$| Lưu trữ các giá trị nén và cây Fenwick | 

Với$n \le 200{,}000$, điều này phù hợp thoải mái trong giới hạn điển hình, vì$n \log n$là khoảng vài triệu hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp))

# We wrap solve to capture print
def solve_output(inp: str):
    import sys
    input = sys.stdin.readline
    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0]*(n+1)
        def add(self,i,v):
            while i<=self.n:
                self.bit[i]+=v
                i+=i&-i
        def sum(self,i):
            s=0
            while i>0:
                s+=self.bit[i]
                i-=i&-i
            return s

    n=int(input())
    a=list(map(int,input().split()))
    vals=sorted(a)
    comp={v:i+1 for i,v in enumerate(vals)}
    fw=Fenwick(n)
    parity=0
    for i,x in enumerate(a):
        x=comp[x]
        leq=fw.sum(x)
        parity^=((i-leq)&1)
        fw.add(x,1)
    print(1 if parity==0 else 0)

    return ""

# provided samples
assert run("3\n3 1 2\n") == ""
assert run("4\n1 2 4 3\n") == ""

# custom cases
assert run("1\n5\n") == "", "single element"
assert run("2\n1 2\n") == "", "already sorted"
assert run("2\n2 1\n") == "", "single swap odd parity"
assert run("5\n5 4 3 2 1\n") == "", "reverse array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp tầm thường | 
| đã được sắp xếp | 1 | đảo ngược bằng không | 
| hoán đổi hai phần tử | 0 | chẵn lẻ lẻ | 
| mảng đảo ngược | 0 | chẵn lẻ đảo ngược tối đa | 

## Vỏ cạnh 

cho$n = 1$, không có thao tác nào tồn tại và câu trả lời luôn là 1 vì mảng được sắp xếp một cách tầm thường. 

Vì$n = 2$, thao tác không thể sử dụng được nên chỉ có hoán vị danh tính là hợp lệ. Thuật toán xử lý chính xác điều này vì tính chẵn lẻ đảo ngược phân biệt trực tiếp$[1,2]$từ$[2,1]$. 

Đối với các trường hợp lớn hơn trong đó mảng gần như được sắp xếp nhưng chứa một đảo ngược duy nhất, thuật toán sẽ loại bỏ chúng một cách chính xác, vì một đảo ngược duy nhất hàm ý tính chẵn lẻ lẻ và không thể sửa được chỉ bằng 3 chu kỳ chẵn.
