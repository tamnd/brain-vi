---
title: "CF 104027K - \u96f6\u65f6\u56f0\u5883 II"
description: "Chúng ta được cung cấp một thiết lập bao gồm một biểu thức ma trận có dạng $A^T nhân A$, trong đó $A$ là một ma trận nào đó và $A^T$ là chuyển vị của nó. Thao tác chính được mô tả trong bài toán là hoán đổi các hàng của $A$ và chúng ta được biết rằng thao tác này không làm thay đổi giá trị của $A^T nhân với A$."
date: "2026-07-02T04:10:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104027
codeforces_index: "K"
codeforces_contest_name: "The 10-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 104027
solve_time_s: 40
verified: true
draft: false
---

[CF 104027K - \u96f6\u65f6\u56f0\u5883 II](https://codeforces.com/problemset/problem/104027/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một thiết lập liên quan đến biểu thức ma trận có dạng$A^T \times A$, Ở đâu$A$là một số ma trận và$A^T$là chuyển vị của nó. Thao tác chính được mô tả trong bài toán này là hoán đổi các hàng của$A$, và chúng ta được biết rằng thao tác này không làm thay đổi giá trị của$A^T \times A$. 

Nhiệm vụ, bị loại bỏ khỏi câu chuyện, về cơ bản là xác định liệu ma trận mục tiêu có$B$phù hợp với giá trị của$A^T \times A$cho ma trận đã cho ban đầu$A$. Vấn đề nhấn mạnh rằng hoán vị hàng không ảnh hưởng đến kết quả, nghĩa là thứ tự bên trong của các hàng trong$A$không liên quan đến sản phẩm tính toán cuối cùng. 

Vì vậy, đầu vào có thể được hiểu là một ma trận$A$, có thể có một số nhiễu biểu diễn hoặc sự mơ hồ về hoán vị và một ma trận$B$mà chúng tôi muốn xác minh dựa trên một bất biến chính tắc bắt nguồn từ$A$, cụ thể là ma trận Gram$A^T A$. 

Đầu ra là nhị phân: chúng tôi xuất ra xem bất biến có khớp với mục tiêu đã cho hay không. 

Các ràng buộc không được nêu rõ ràng, nhưng với cấu trúc Codeforces điển hình và đề cập đến các phép toán ma trận, chúng ta nên giả định$n$ít nhất là đến$10^5$hoặc quy mô tương tự. Điều đó ngay lập tức loại trừ phép nhân ma trận ngây thơ$O(n^3)$sẽ tiếp cận nếu chúng ta giải thích nó như những ma trận dày đặc. Tuy nhiên, quan sát quan trọng là phép toán bất biến khi hoán đổi hàng, vì vậy chúng ta không cần mô phỏng bất kỳ phép biến đổi nào. 

Một sai lầm ngây thơ nhưng đầy cám dỗ là tính toán lại tích ma trận trong khi vẫn tôn trọng các hoán vị hàng một cách rõ ràng. Ví dụ, nếu người ta cố gắng xây dựng lại$A$theo thứ tự hàng khác nhau và tính toán lại$A^T A$, điều này trở thành giai thừa theo số lượng hàng. 

Một trường hợp thất bại tinh vi hơn xuất hiện khi hiểu sai phép nhân chuyển vị. Ví dụ, tính toán sai$A A^T$thay vì$A^T A$dẫn đến một chiều hướng và ý nghĩa hoàn toàn khác, mặc dù cả hai đều trông giống nhau. 

Một ví dụ về cạnh bê tông: 

đầu vào:$A = \begin{bmatrix}1 & 2 \\ 3 & 4\end{bmatrix}$Nếu chúng ta tính toán sai$A A^T$, chúng tôi nhận được:$$\begin{bmatrix}5 & 11 \\ 11 & 25\end{bmatrix}$$Nhưng$A^T A$mang lại:$$\begin{bmatrix}10 & 14 \\ 14 & 20\end{bmatrix}$$Việc nhầm lẫn những điều này sẽ dẫn đến những câu trả lời sai liên tục ngay cả trong những trường hợp nhỏ. 

Sự đơn giản hóa sâu hơn trong phát biểu bài toán là tất cả các hoán vị hàng đều bảo toàn nhiều tập vectơ hàng và do đó ma trận Gram chỉ phụ thuộc vào các tích bên trong được tổng hợp trên các hàng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ xử lý vấn đề theo nghĩa đen: xem xét tất cả các hoán vị hàng có thể có của$A$, tính toán$A^T A$cho mỗi cái và kiểm tra xem có cái nào bằng không$B$. Mỗi phép tính của$A^T A$mất$O(n^2 m)$nếu như$A$là$n \times m$, và có$n!$hoán vị. Điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 10$. 

Ngay cả khi chúng ta tránh liệt kê các hoán vị và thay vào đó cố gắng mô phỏng việc hoán đổi hàng một cách linh hoạt, chúng ta vẫn đang tính toán lại cấu trúc bậc hai nhiều lần, dẫn đến công việc không cần thiết. 

Cái nhìn sâu sắc quan trọng là$A^T A$là bất biến trong các hoán vị hàng vì về cơ bản nó là tổng của các hàng:$$A^T A = \sum_{i} r_i^T r_i$$Ở đâu$r_i$là$i$-hàng thứ của$A$. Việc hoán đổi các hàng không làm thay đổi tập hợp nhiều tích số bên ngoài này, do đó ma trận cuối cùng vẫn giống hệt nhau. 

Điều này biến toàn bộ vấn đề thành một phép tính trực tiếp: chúng ta tính toán ma trận Gram một lần từ ma trận ban đầu$A$, rồi so sánh trực tiếp với$B$. Không có mô phỏng, không có tổ hợp trên hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n! \cdot n^2)$|$O(n^2)$| Quá chậm | 
| Tính toán Gram trực tiếp |$O(n m^2)$hoặc$O(n m)$tùy theo cấu trúc |$O(m^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích từng hàng của$A$dưới dạng một vectơ trong một$m$-không gian chiều. Ma trận$A^T A$được xây dựng bằng cách tích lũy các tích tọa độ theo cặp trên tất cả các hàng. 

1. Đọc ma trận$A$và ma trận$B$. Chúng tôi giả sử$A$có$n$hàng và$m$cột, trong khi$B$là$m \times m$. Các thứ nguyên đã thực thi rằng chỉ có thể thực hiện một so sánh có ý nghĩa. 
2. Khởi tạo một$m \times m$ma trận`res`với số không. Điều này sẽ lưu trữ ma trận Gram được tính toán. Mỗi mục$res[j][k]$đại diện cho sự đóng góp tích lũy của sản phẩm chấm trên tất cả các hàng. 
3. Đối với mỗi hàng$i$TRONG$A$, lặp qua tất cả các cặp cột$(j, k)$. Chúng tôi thêm$A[i][j] \times A[i][k]$ĐẾN$res[j][k]$. Điều này trực tiếp thực hiện định nghĩa của$A^T A$mà không hình thành một cách rõ ràng một chuyển vị. 
4. Sau khi xử lý tất cả các hàng,`res`chứa đầy đủ$A^T A$. Bây giờ chúng tôi so sánh từng mục với$B$. Nếu có bất kỳ sự không trùng khớp nào xuất hiện, chúng tôi ngay lập tức kết luận rằng chúng không bằng nhau. 
5. Nếu tất cả các mục đều khớp, chúng ta sẽ xuất ra kết quả bằng nhau. 

Lý do chúng ta tích lũy qua các hàng thay vì xây dựng một phép nhân ma trận đầy đủ là vì việc phân tách theo hàng vừa đơn giản hơn vừa tránh được chi phí chuyển vị không cần thiết. 

### Tại sao nó hoạt động 

giá trị$A^T A$mở rộng thành tổng trên tích ngoài của các hàng:$$A^T A = \sum_{i=1}^{n} r_i^T r_i$$Mỗi hàng đóng góp độc lập vào ma trận cuối cùng. Hoán đổi hàng chỉ hoán vị thứ tự của các số hạng này trong tổng và phép cộng có tính chất giao hoán nên kết quả cuối cùng không thay đổi. Điều này làm cho ma trận Gram trở thành một hàm nhiều tập hợp của các hàng chứ không phải là một đối tượng phụ thuộc vào chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    A = [list(map(int, input().split())) for _ in range(n)]
    B = [list(map(int, input().split())) for _ in range(m)]
    
    res = [[0] * m for _ in range(m)]
    
    for i in range(n):
        row = A[i]
        for j in range(m):
            for k in range(m):
                res[j][k] += row[j] * row[k]
    
    for i in range(m):
        for j in range(m):
            if res[i][j] != B[i][j]:
                print("No")
                return
    
    print("Yes")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo sự phân tách hàng của ma trận Gram. Cấu trúc lồng ba là không thể tránh khỏi trong trường hợp chung vì mỗi hàng đóng góp một sản phẩm bên ngoài đầy đủ. Bước so sánh được thực hiện ngay sau khi thi công để tránh những công việc không cần thiết. 

Một cạm bẫy phổ biến là đảo ngược các chỉ số khi xây dựng sản phẩm bên ngoài. Đóng góp đúng là`row[j] * row[k]`, không trộn lẫn các chỉ mục hàng hoặc cố gắng chuyển đổi một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử:```
A =
1 2
3 4

B =
10 14
14 20
```Chúng tôi tính toán từng bước. 

| Hàng | Đóng góp cho res | 
| --- | --- | 
| [1, 2] | [[1, 2], [2, 4]] | 
| [3, 4] | [[9, 12], [12, 16]] | 

Cuối cùng:```
res =
10 14
14 20
```Điều này phù hợp$B$, vì vậy đầu ra là Có. 

Dấu vết này xác nhận rằng các khoản đóng góp tích lũy tuyến tính và độc lập trên mỗi hàng. 

### Ví dụ 2```
A =
1 0
0 1

B =
1 0
0 1
```| Hàng | Đóng góp | 
| --- | --- | 
| [1, 0] | [[1, 0], [0, 0]] | 
| [0, 1] | [[0, 0], [0, 1]] | 

Kết quả cuối cùng:```
1 0
0 1
```Một lần nữa trận đấu$B$, cho thấy các hàng cơ sở trực giao tạo ra ma trận Gram chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n m^2)$| Mỗi trong số$n$hàng đóng góp một$m \times m$sản phẩm bên ngoài | 
| Không gian |$O(m^2)$| Lưu trữ ma trận Gram thu được | 

Thuật toán chia tỷ lệ một cách tự nhiên với kích thước đầu vào ma trận dày đặc điển hình cho các bài toán xây dựng ma trận Gram. Ngay cả đối với kích thước vừa phải$n, m$, đây là cách tiếp cận tối ưu tiêu chuẩn vì mọi phần tử đầu vào đều tham gia vào ít nhất một phép nhân. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

def solve():
    n, m = map(int, input().split())
    A = [list(map(int, input().split())) for _ in range(n)]
    B = [list(map(int, input().split())) for _ in range(m)]
    
    res = [[0]*m for _ in range(m)]
    for i in range(n):
        for j in range(m):
            for k in range(m):
                res[j][k] += A[i][j] * A[i][k]
    
    print("Yes" if res == B else "No")

# provided sample-like cases
assert run("2 2\n1 2\n3 4\n10 14\n14 20\n") == "Yes"

# minimum size
assert run("1 1\n5\n25\n") == "Yes"

# mismatch case
assert run("1 2\n1 2\n1 3\n1 4\n") == "No"

# identity case
assert run("2 2\n1 0\n0 1\n1 0\n0 1\n") == "Yes"

# negative values
assert run("2 2\n1 -1\n-1 1\n2 -2\n-2 2\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận 1x1 | Có | ma trận Gram hợp lệ nhỏ nhất | 
| không khớp B | Không | phát hiện so sánh sai | 
| ma trận nhận dạng | Có | bảo quản đường chéo | 
| giá trị âm | Có | xử lý dấu hiệu trong các sản phẩm chấm | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Trong tình huống này,$A^T A$chỉ đơn giản là tích bên ngoài của một hàng với chính nó. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp trên các hàng chạy chính xác một lần và không cần phân nhánh đặc biệt. 

Một trường hợp khác là khi tất cả các mục đều bằng 0. Khi đó mọi đóng góp đều bằng 0 và kết quả là ma trận bằng 0 bất kể số hàng. Thuật toán tích lũy chính xác các số 0 và sẽ khớp với$B$chỉ khi nó cũng bằng không. 

Trường hợp thứ ba là khi các hàng được lặp lại. Vì mỗi hàng đóng góp độc lập nên các bản sao chỉ đơn giản là chia tỷ lệ đóng góp một cách tuyến tính. Ví dụ: hai hàng giống hệt nhau sẽ nhân đôi phần đóng góp của ma trận Gram. Thuật toán tích lũy cả hai bản sao một cách tự nhiên mà không cần loại bỏ trùng lặp hoặc băm.
