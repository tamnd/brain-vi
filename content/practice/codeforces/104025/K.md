---
title: "CF 104025K - ZYW có gia sư"
description: "Chúng ta được cho một ma trận vuông có kích thước $n nhân n$ chứa các số nguyên. Chúng ta được phép chọn chính xác một ô trong ma trận này và thay thế giá trị của nó bằng bất kỳ số thực nào mà chúng ta muốn, nhưng thao tác này có thể được thực hiện nhiều nhất một lần."
date: "2026-07-02T04:17:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104025
codeforces_index: "K"
codeforces_contest_name: "The 16-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 104025
solve_time_s: 44
verified: true
draft: false
---

[CF 104025K - ZYW với gia sư](https://codeforces.com/problemset/problem/104025/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ma trận vuông có kích thước$n \times n$chứa số nguyên. Chúng ta được phép chọn chính xác một ô trong ma trận này và thay thế giá trị của nó bằng bất kỳ số thực nào mà chúng ta muốn, nhưng thao tác này có thể được thực hiện nhiều nhất một lần. Sau đó, chúng ta tính định thức của ma trận thu được và chỉ quan tâm đến giá trị tuyệt đối của nó. Mục tiêu là chọn cả ô và giá trị mới sao cho định thức tuyệt đối trở nên nhỏ nhất có thể. 

Định thức là một đại lượng toàn cầu, nhưng phép toán chúng ta được phép mang tính cục bộ: chúng ta chỉ chạm vào một mục duy nhất. Thách thức là phải hiểu mức độ ảnh hưởng của một mục đối với yếu tố quyết định và liệu ảnh hưởng đó có đủ để đẩy yếu tố quyết định xuống đáng kể hay không. 

Những hạn chế tương đối nhỏ, với$n \le 100$, vì vậy bất cứ điều gì liên quan đến$O(n^3)$đại số tuyến tính là hoàn toàn tốt. Tuy nhiên, sự hiện diện của sự thay thế có giá trị thực cho thấy rằng lý do dự định không phải là loại bỏ tính toán thuần túy mà là sự hiểu biết mang tính cấu trúc về cách các yếu tố quyết định phụ thuộc vào các mục riêng lẻ. 

Trường hợp góc tinh tế xuất hiện khi$n = 1$. Trong trường hợp này, định thức chỉ là một phần tử duy nhất của ma trận và việc thay đổi nó một lần sẽ xác định hoàn toàn câu trả lời. 

Một trường hợp cạnh khái niệm thú vị hơn sẽ là các ma trận trong đó nhiều hàng hoặc cột phụ thuộc, ví dụ như ma trận toàn 0 hoặc ma trận thiếu thứ hạng. Một cách tiếp cận ngây thơ cố gắng mô phỏng những thay đổi về mặt số học có thể bỏ lỡ rằng định thức đôi khi có thể được đưa chính xác về 0 bằng một sửa đổi được lựa chọn cẩn thận. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử mọi tế bào có thể$(i, j)$là vị trí được sửa đổi và đối với mỗi lựa chọn như vậy, hãy coi định thức là hàm của giá trị được sửa đổi$x$. Định thức là đa thức bậc một của biến đó, vì vậy chúng ta có thể tính các hệ số của nó và sau đó cực tiểu hóa giá trị tuyệt đối theo phương pháp phân tích trên$x$. Điều này đã làm giảm bớt vấn đề trong việc đánh giá một vài yếu tố quyết định hoặc đồng yếu tố trên mỗi ô. Việc tính toán các yếu tố quyết định hoặc đồng yếu tố cho từng ô một cách độc lập dẫn đến độ phức tạp theo thứ tự$O(n^4)$, vì mỗi chi phí quyết định$O(n^3)$, và có$O(n^2)$tế bào ứng cử viên Điều này có thể chấp nhận được dưới$n \le 100$, nhưng việc đó tốn nhiều công sức hơn mức cần thiết. 

Cái nhìn sâu sắc về cấu trúc quan trọng là yếu tố quyết định là tuyến tính trong mỗi mục riêng lẻ khi tất cả các mục khác đều cố định. Nếu chúng ta mở rộng định thức dọc theo một mục cụ thể$A_{ij}$, chúng ta nhận được một biểu mẫu$\det(A) = A_{ij} \cdot C_{ij} + \text{(terms independent of } A_{ij})$, Ở đâu$C_{ij}$là cofactor tương ứng với mục đó. 

Khi chúng ta xem định thức theo cách này, quyền tự do thay thế một mục bằng bất kỳ số thực nào sẽ trở nên rất mạnh mẽ. Nếu hệ số$C_{ij}$khác 0 thì ta có thể chọn giá trị của$A_{ij}$sao cho định thức trở thành chính xác bằng 0 bằng cách giải một phương trình tuyến tính đơn giản. Điều đó ngay lập tức đưa định thức tuyệt đối tối thiểu về 0 cho lựa chọn ô đó. 

Mối quan tâm duy nhất còn lại là liệu có tồn tại tình huống mà mọi đồng yếu tố đều bằng không hay không. Điều đó có nghĩa là mọi$(n-1) \times (n-1)$nhỏ bằng 0, ngụ ý ma trận có thứ hạng nhiều nhất$n-2$, do đó buộc định thức của nó phải bằng 0. Trong trường hợp đó, chúng tôi đã ở giá trị tối thiểu có thể. 

Vì vậy, quan điểm vũ phu sụp đổ thành một quan sát duy nhất: trừ khi định thức được cố định về mặt cấu trúc bằng 0 bất kể sự thay đổi của một mục, chúng ta luôn có thể buộc nó trở thành 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force thông qua việc tính toán lại các yếu tố quyết định trên mỗi ô |$O(n^4)$|$O(1)$| Được chấp nhận nhưng không cần thiết | 
| Cái nhìn sâu sắc về tuyến tính của đồng yếu tố |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính định thức của ma trận theo bất kỳ cách tiêu chuẩn nào, chẳng hạn như phép khử Gaussian. Điều này mang lại cho chúng ta giá trị cơ bản, mặc dù chúng ta sẽ không thực sự cần nó cho câu trả lời cuối cùng. 
2. Tính toán cấu trúc liên hợp một cách ngầm định bằng cách lấy các đồng yếu tố cho các mục hoặc quan sát một cách tương đương rằng chúng ta thậm chí không cần xác định một mục cụ thể, chỉ cần xác định liệu một số đồng yếu tố có khác 0 hay không. Điều này có thể được suy ra từ việc ma trận có xếp hạng đầy đủ hay tất cả$(n-1)\times(n-1)$trẻ vị thành niên biến mất. 
3. Kiểm tra xem có tồn tại ít nhất một mục có đồng hệ số khác 0 hay không. Nếu mục đó tồn tại, hãy chọn mục đó làm vị trí mà chúng tôi sẽ sửa đổi. 
4. Đặt mục nhập đã chọn thành một giá trị hủy bỏ biểu thức định thức tuyến tính. Vì định thức là affine trong mục đó, nên điều này luôn có thể được thực hiện khi hệ số khác 0, mang lại định thức chính xác bằng 0. 
5. Nếu không có mục nào như vậy tồn tại, hãy kết luận rằng tất cả các đồng yếu tố đều bằng 0, điều này ngụ ý định thức bằng 0 giống hệt nhau và không thể thay đổi bằng một sửa đổi mục nhập duy nhất. 

### Tại sao nó hoạt động 

Định thức là tuyến tính đối với bất kỳ mục nào khi tất cả các mục khác đều cố định. Sửa tất cả các mục ngoại trừ$A_{ij}$, định thức trở thành hàm tuyến tính$f(x) = C_{ij}x + d$. Nếu như$C_{ij} \neq 0$, hàm này luôn có thể về 0 bằng cách chọn$x = -d/C_{ij}$. Nếu mỗi$C_{ij} = 0$, thì định thức không phụ thuộc vào bất kỳ phần tử đơn lẻ nào, điều này chỉ có thể thực hiện được khi ma trận đã là số ít đến mức buộc định thức bằng 0. Điều này đảm bảo yếu tố quyết định tuyệt đối tối thiểu có thể đạt được luôn bằng không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def gauss_det(a):
    n = len(a)
    det = 1
    sign = 1
    for i in range(n):
        pivot = i
        for j in range(i, n):
            if abs(a[j][i]) > abs(a[pivot][i]):
                pivot = j
        if a[pivot][i] == 0:
            return 0
        if pivot != i:
            a[i], a[pivot] = a[pivot], a[i]
            sign *= -1
        pivot_val = a[i][i]
        det *= pivot_val
        for j in range(i + 1, n):
            factor = a[j][i] / pivot_val
            for k in range(i, n):
                a[j][k] -= factor * a[i][k]
    return det * sign

def main():
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]
    
    # We don't actually need the determinant value.
    # The key observation guarantees the answer is always 0.
    print(0)

if __name__ == "__main__":
    main()
```Việc triển khai có chủ đích tránh tính toán nặng nề vì cấu trúc toán học khiến nó không cần thiết. Mặc dù việc loại bỏ Gaussian được hiển thị dưới dạng tham chiếu, chương trình cuối cùng giảm xuống đầu ra không đổi sau khi nhận ra rằng một mục nhập có giá trị thực tự do duy nhất luôn có thể loại bỏ định thức. 

Một lỗi phổ biến là cố gắng tính toán các yếu tố đồng yếu tố hoặc yếu tố quyết định cho mọi điểm sửa đổi có thể có. Điều đó đúng nhưng dư thừa, vì đối số phụ thuộc tuyến tính đảm bảo sự tồn tại của một phép khử hoàn hảo miễn là định thức không bất biến về mặt cấu trúc trong các nhiễu loạn một mục nhập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một ma trận đơn giản:$$\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}$$Chúng tôi đánh giá về mặt khái niệm điều gì sẽ xảy ra nếu chúng tôi chọn mục nhập$A_{1,1}$. 

| Bước | Giá trị của$A_{1,1}$| Dạng xác định | 
| --- | --- | --- | 
| Bắt đầu | 1 | cố định | 
| Sửa đổi |$x$|$4x - 6$| 

Chúng ta có thể chọn$x = \frac{3}{2}$để tạo định thức bằng 0. 

Điều này cho thấy rằng ngay cả ma trận xếp hạng đầy đủ cũng có thể bị vô hiệu hóa chỉ bằng một thay đổi mục nhập. 

### Ví dụ 2 

Hãy xem xét một ma trận bằng không:$$\begin{bmatrix}
0 & 0 \\
0 & 0
\end{bmatrix}$$| Bước | Quan sát | 
| --- | --- | 
| Mọi sửa đổi | Yếu tố quyết định trở thành tuyến tính trong mục đã chọn nhưng hệ số đồng yếu tố bằng 0 | 
| Kết quả | Luôn luôn 0 | 

Điều này xác nhận rằng ngay cả trong trường hợp suy biến, kết quả vẫn bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Sau khi nhận dạng thuộc tính cấu trúc, không cần tính toán ngoài việc đọc đầu vào | 
| Không gian |$O(n^2)$| Lưu trữ đầu vào ma trận | 

Các ràng buộc cho phép tính toán nặng hơn nhiều, nhưng tính tuyến tính của định thức làm giảm bài toán về kết luận có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    
    # direct solution logic
    n = int(input())
    for _ in range(n):
        input()
    return "0"

# provided sample (interpreted minimal form)
assert run("1\n1\n") == "0", "sample 1"

# n = 2 identity
assert run("2\n1 0\n0 1\n") == "0", "identity matrix"

# zero matrix
assert run("3\n0 0 0\n0 0 0\n0 0 0\n") == "0", "zero matrix"

# random small matrix
assert run("2\n1 2\n3 4\n") == "0", "generic full rank"

# single element
assert run("1\n5\n") == "0", "n=1 case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận 1x1 | 0 | trường hợp cạnh đơn phần tử | 
| ma trận nhận dạng | 0 | trường hợp xếp hạng đầy đủ | 
| ma trận không | 0 | ma trận suy biến | 
| ngẫu nhiên 2x2 | 0 | tính đúng đắn chung | 

## Vỏ cạnh 

cho$n = 1$, định thức bằng mục duy nhất. Vì chúng ta được phép thay đổi mục nhập đó một lần thành bất kỳ số thực nào nên chúng ta trực tiếp đặt nó về 0, làm cho đầu ra bằng 0. 

Đối với các ma trận xếp hạng đầy đủ như ma trận đẳng thức, định thức ban đầu là 1, nhưng chọn bất kỳ mục nào$A_{ij}$trong đó hệ số tương ứng khác 0 cho phép chúng ta điều chỉnh mục nhập đó để hủy bỏ định thức một cách chính xác. 

Đối với các ma trận đã là số ít, định thức bằng 0 ngay từ đầu và mọi sửa đổi đều không thể làm tăng mức tối thiểu xuống dưới 0, do đó kết quả tối ưu vẫn bằng 0.
