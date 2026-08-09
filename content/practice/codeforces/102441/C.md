---
title: "CF 102441C - Tổng một phần"
description: "Chúng ta bắt đầu với ma trận nhị phân (n lần m) (A0). Một thao tác sẽ thay thế mọi ô bằng tính chẵn lẻ của hình chữ nhật từ góc trên bên trái đến ô đó."
date: "2026-08-09T01:36:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "C"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 297
verified: true
draft: false
---

[CF 102441C - Tổng một phần](https://codeforces.com/problemset/problem/102441/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 57 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với ma trận nhị phân (n \times m) (A_0). Một thao tác sẽ thay thế mọi ô bằng tính chẵn lẻ của hình chữ nhật từ góc trên bên trái đến ô đó. Việc áp dụng phép toán nhiều lần sẽ tạo ra (A_1,A_2,\ldots) và chúng ta cần số phép toán dương nhỏ nhất mà sau đó ma trận hiện tại lại trở thành chính xác (A_0). Vấn đề chính thức đưa ra (1 \le n,m \le 10^6) và (nm\le 10^6), với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Ràng buộc sản phẩm là hạn chế chính. Chúng ta có thể đủ khả năng truyền tuyến tính trên tất cả các ô (nm), nhưng chúng ta không thể mô phỏng nhiều phép biến đổi của toàn bộ ma trận. Một phép biến đổi duy nhất đã có giá (\Theta(nm)) và câu trả lời có thể ở khoảng (10^6), do đó, việc xây dựng ma trận nhiều lần sẽ vượt xa giới hạn. Giải pháp phải xử lý đầu vào về cơ bản một lần. 

Có một số trường hợp việc triển khai chỉ dựa trên kích thước ma trận sẽ cho kết quả sai. Ví dụ, đối với ma trận toàn 0,```
2 3
000
000
```câu trả lời đúng là (1), bởi vì một phép tính tổng một phần không làm thay đổi ma trận. Một giải pháp luôn tìm kiếm lũy thừa của hai đủ lớn để bao phủ các kích thước có thể trả về không chính xác (4). 

Ma trận một chiều có thể có chu kỳ nhỏ hơn nhiều so với độ dài của nó gợi ý. Vì```
1 4
0001
```câu trả lời đúng là (1). Ô khác 0 duy nhất đã ở ngoài cùng bên phải, vì vậy việc lấy tổng tiền tố modulo (2) không làm thay đổi hàng. Lời giải bất cẩn cho rằng đáp án ít nhất phải bằng (m) sẽ trả về (4). 

Vị trí của ô khác 0 đầu tiên cũng quan trọng theo hướng khác. Vì```
1 4
0100
```câu trả lời đúng là (4). Ô đầu tiên chứa (1) là cột (2) và lũy thừa nhỏ hơn của 2 không đẩy ranh giới cần thiết đủ xa về bên phải. Chỉ nhìn vào sự tồn tại của (1) mà không xét đến vị trí của nó sẽ làm mất đi chính xác thông tin quyết định đáp án. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng (A_1), sau đó (A_2), v.v., cho đến khi một trong số chúng bằng (A_0). Phép biến đổi tổng tiền tố hai chiều có thể được thực hiện trong thời gian (O(nm)) bằng cách duy trì tiền tố hàng hiện tại, tiền tố cột và đóng góp đường chéo trước đó, tất cả theo modulo (2). Phương pháp này đúng vì nó tính toán chính xác định nghĩa của phép toán. 

Vấn đề là số lần chuyển đổi. Xét toán tử tổng tiền tố một chiều trên vectơ có độ dài (d). Theo modulo (2), bậc của nó là lũy thừa của hai và trở thành đơn vị khi lũy thừa đó đạt ít nhất (d). Nếu (d=10^6), lũy thừa tiếp theo của 2 là (2^{20}=1,048,576). Do đó, một mô phỏng lực lượng vũ phu có thể yêu cầu khoảng (1.048.576) lần truyền ma trận đầy đủ. Với (nm=10^6), đó là khoảng (1,05\times10^{12}) thao tác ô trong trường hợp xấu nhất. 

Quan sát loại bỏ hệ số lớn này xuất phát từ việc xem tổng tiền tố dưới dạng toán tử tuyến tính trên trường có hai phần tử. Giả sử (S) là toán tử dịch chuyển mọi phần tử xuống một vị trí và chèn số 0 vào đầu. Tổng tiền tố là 

[ 
P=I+S+S^2+\cdots. 
] 

Đối với lũy thừa hai (q=2^t), đặc tính (2) cho 

[ 
P^q=(I+S)^q=I+S^q. 
] 

Tất cả các hệ số nhị thức trung gian đều biến mất modulo (2). Danh tính tương tự giữ độc lập cho các hàng và cột. 

Phép biến đổi hai chiều hoàn chỉnh có thể tách rời được, do đó sau các phép toán (q), 

[ 
A_q=(I+R^q)A_0(I+C^q), 
] 

trong đó (R) dịch chuyển hàng và (C) dịch chuyển cột. Do đó, đối với mỗi tế bào, 

A[i,j] 
\oplus A[i-q,j] 
\oplus A[i,j-q] 
\oplus A[i-q,j-q], 
] 

trong đó một chỉ mục bên ngoài ma trận đóng góp bằng không. 

Có một thực tế cấu trúc quan trọng khác. Chu kỳ của bất kỳ ma trận cụ thể nào cũng phải là lũy thừa của hai, bởi vì phép biến đổi hoàn toàn có bậc lũy thừa hai. Vì vậy, thay vì kiểm tra mọi (k) có thể, chúng ta chỉ cần hiểu lũy thừa của hai. 

Giả sử (q<\min(n,m)) và (A_q=A). Đối với (i>q) và (j\le q), các số hạng dịch chuyển liên quan đến (j-q) biến mất, cho ra (A[i-q,j]=0). Với (i>q,j>q), hãy xác định 

[ 
B[i,j]=A[i,j]\oplus A[i-q,j]. 
] 

Đẳng thức (A_q=A) cho 

[ 
B[i,j]=B[i,j-q]. 
] 

Điều kiện biên trước đó nói rằng các cột (q) đầu tiên của (B) bằng 0, do đó đẳng thức lan truyền trên mọi cột và (B) hoàn toàn bằng 0. Do đó mỗi hàng lặp lại với dấu chấm (q). 

Vì các cột (q) đầu tiên bằng 0 ở các hàng bên dưới và các hàng lặp lại với dấu chấm (q), nên các cột đó thực sự bằng 0 ở mọi nơi. Điều kiện biên đối xứng nói rằng các cột (m-q) đầu tiên của các hàng (q) đầu tiên bằng 0. Bởi vì (q<m), hai vùng đó bao gồm mọi cột của hàng (q) đầu tiên. Do đó, các hàng (q) đầu tiên bằng 0 và tính tuần hoàn của hàng làm cho toàn bộ ma trận bằng 0. 

Vì vậy, ma trận khác 0 chỉ có thể trả về sau lũy thừa hai (q) thỏa mãn 

[ 
q\ge \min(n,m). 
] 

Điều này để lại một điều kiện một chiều rất đơn giản. 

Nếu (n\le m), mọi (q) liên quan đều thỏa mãn (q\ge n), thì độ dịch hàng (R^q) đã bằng không. Chỉ còn lại chuyển đổi cột. Ma trận không thay đổi chính xác khi mọi cột từ (1) đến (m-q) đều bằng 0. Nếu cột đầu tiên chứa (1) là (c), thì điều này tương đương với 

[ 
m-q<c, 
] 

hoặc 

[ 
q\ge m-c+1. 
] 

Do đó lũy thừa cần thiết của hai ít nhất là lũy thừa nhỏ nhất của hai 

[ 
\max(n,m-c+1). 
] 

Trường hợp (m<n) là đối xứng. Chúng ta tìm hàng đầu tiên chứa a (1), giả sử (r), và lũy thừa cần tìm của 2 ít nhất là lũy thừa nhỏ nhất của 2 

[ 
\max(m,n-r+1). 
] 

Phương pháp brute-force tuân theo định nghĩa theo nghĩa đen, nhưng sự đồng nhất lũy thừa hai biến toàn bộ vấn đề thành việc tìm ra chỉ một vị trí biên.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm\cdot 2^{\lceil\log_2\max(n,m)\rceil})) | (O(nm)) | Quá chậm | 
| Tối ưu | (O(nm)) | (O(1)) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước ma trận và xác định kích thước nào nhỏ hơn. Chúng ta sẽ chỉ cần thông tin dọc theo chiều lớn hơn, bởi vì chiều nhỏ hơn cho chúng ta biết lũy thừa hai chu kỳ đầu tiên có thể có. 
2. Nếu (n\le m), quét từng hàng và ghi lại cột nhỏ nhất chứa a (1). Gọi cột này là (c). Chúng ta không cần lưu trữ ma trận vì điều kiện cuối cùng chỉ phụ thuộc vào cột khác 0 sớm nhất này. 
3. Nếu không có (1), ghi (1). Ma trận 0 được cố định bởi mọi phép toán tổng tiền tố. 
4. Với (n\le m), tính ngưỡng số nhỏ nhất 

[ 
L=\max(n,m-c+1). 
] 

Bất kỳ khoảng thời gian hợp lệ nào cũng phải có lũy thừa của ít nhất hai (n) và cột khác 0 đầu tiên cũng yêu cầu thêm (q\ge m-c+1). 

1. Nếu (m<n), thực hiện quét đối xứng cho hàng đầu tiên (r) chứa a (1), sau đó đặt 

[ 
L=\max(m,n-r+1). 
] 

1. Bắt đầu từ (1), liên tục nhân đôi ứng viên cho đến khi đạt ít nhất (L). Giá trị thu được là câu trả lời vì mọi chu kỳ khác 0 có thể đều là lũy thừa của hai. 

### Tại sao nó hoạt động 

Bất biến đằng sau thuật toán là mọi thời gian trả về khác 0 đều là lũy thừa của hai. Đối với lũy thừa hai (q), phép toán tiền tố gấp (q) chỉ là nhận dạng cộng với sự dịch chuyển theo (q) trong mỗi chiều. Nếu (q<\min(n,m)), các số hạng biên buộc mọi hàng và cột phải lặp lại đồng thời buộc các vùng biên về 0, điều này chỉ có thể thực hiện được đối với ma trận 0. Do đó mọi ma trận khác 0 đều cần (q\ge\min(n,m)). 

Một lần (q\ge n), khi (n\le m), sự dịch chuyển hàng biến mất hoàn toàn. Thay đổi duy nhất còn lại là so sánh mọi ô với vị trí ô (q) ở bên trái của nó. Ma trận không thay đổi chính xác khi tất cả các cột trước (c) đủ xa ranh giới đó, cho ra (q\ge m-c+1). Đối số tương tự với các thẻ điều khiển hàng (m<n). Do đó, lấy lũy thừa nhỏ nhất của hai thỏa mãn cả hai giới hạn dưới sẽ là giá trị hợp lệ tối thiểu (k). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    first = None

    if n <= m:
        for _ in range(n):
            row = input().strip()
            pos = row.find('1')
            if pos != -1:
                col = pos + 1
                if first is None or col < first:
                    first = col

        if first is None:
            print(1)
            return

        need = max(n, m - first + 1)

    else:
        for i in range(1, n + 1):
            row = input().strip()
            if first is None and '1' in row:
                first = i

        if first is None:
            print(1)
            return

        need = max(m, n - first + 1)

    ans = 1
    while ans < need:
        ans <<= 1

    print(ans)

if __name__ == "__main__":
    solve()
```Dữ liệu đầu vào được xử lý theo từng hàng, do đó chương trình không bao giờ cấp phát ma trận (n\times m). Khi (n\le m),`row.find('1')`trực tiếp đưa ra cột khác 0 đầu tiên của hàng hiện tại. Lấy giá trị tối thiểu trên tất cả các hàng sẽ cho cột đầu tiên chứa bất kỳ (1) nào. 

Khi (m<n), chỉ có hàng đầu tiên chứa (1) quan trọng, do đó mã sẽ ghi lại chỉ mục hàng hiện tại lần đầu tiên`'1' in row`thành công. Các hàng còn lại vẫn phải được đọc vì chúng là một phần của luồng đầu vào. 

Các biểu thức`m - first + 1`Và`n - first + 1`là các khoảng cách ranh giới được suy ra ở trên. các`+1`là điều cần thiết. Nếu (1) đầu tiên nằm ở cột cuối cùng thì (m-c+1=1), cho phép đúng dấu chấm (1). Bỏ qua`+1`sẽ tạo ra không chính xác số 0 làm ngưỡng. 

Không có vấn đề tràn số nguyên trong Python và trên thực tế, câu trả lời nhiều nhất là lũy thừa tiếp theo của hai số trên (10^6), cụ thể là (1.048.576). Do đó, vòng lặp nhân đôi chỉ thực hiện khoảng 20 lần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
1 1
1
```Ở đây (n\le m) và cột khác 0 đầu tiên là (c=1). 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 1 | 
| (m) | 1 | 
| Cột khác 0 đầu tiên (c) | 1 | 
| (mc+1) | 1 | 
| Ngưỡng (L) | 1 | 
| lũy thừa nhỏ nhất của hai (\ge L) | 1 | 

Câu trả lời là (1). Ô đơn chứa (1) và tổng tiền tố của nó vẫn là (1). 

### Mẫu 2 

Đầu vào là```
4 2
00
01
10
11
```Bây giờ (m<n), vì vậy chúng ta tìm hàng đầu tiên chứa (1). Đó là hàng (2). 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 4 | 
| (m) | 2 | 
| Hàng khác 0 đầu tiên (r) | 2 | 
| (n-r+1) | 3 | 
| Ngưỡng (L) | 3 | 
| Ứng viên (1) | Quá nhỏ | 
| Ứng viên (2) | Quá nhỏ | 
| Ứng viên (4) | hợp lệ | 

Câu trả lời là (4), phù hợp với mẫu chính thức. 

Dấu vết cũng cho thấy tại sao chỉ sử dụng chiều nhỏ hơn là không đủ. Kích thước nhỏ hơn là (m=2), nhưng hàng khác 0 đầu tiên đủ cao để (q=2) vẫn không đạt đến ranh giới yêu cầu. Sức mạnh tiếp theo của hai, (4), là khoảng thời gian hợp lệ đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm)) | Mỗi ô đầu vào được đọc một lần và mỗi hàng sử dụng tìm kiếm chuỗi tuyến tính. | 
| Không gian | (O(1)) thêm | Chỉ các thứ nguyên, vị trí khác 0 đầu tiên và một vài số nguyên được lưu trữ. | 

Ràng buộc (nm\le10^6) có nghĩa là ma trận hoàn chỉnh có thể được quét thoải mái một lần. Thuật toán không thực hiện các phép biến đổi ma trận lặp lại và không lưu trữ ma trận, do đó, nó phù hợp thoải mái trong giới hạn 1 giây và 256 MB đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    first = None

    if n <= m:
        for _ in range(n):
            row = input().strip()
            pos = row.find('1')
            if pos != -1:
                col = pos + 1
                if first is None or col < first:
                    first = col

        if first is None:
            ans = 1
        else:
            need = max(n, m - first + 1)
            ans = 1
            while ans < need:
                ans <<= 1
    else:
        for i in range(1, n + 1):
            row = input().strip()
            if first is None and '1' in row:
                first = i

        if first is None:
            ans = 1
        else:
            need = max(m, n - first + 1)
            ans = 1
            while ans < need:
                ans <<= 1

    sys.stdin = old_stdin
    return str(ans)

# Provided sample 1
assert solve("""\
1 1
1
""") == "1", "sample 1"

# Provided sample 2
assert solve("""\
4 2
00
01
10
11
""") == "4", "sample 2"

# Minimum-size zero matrix
assert solve("""\
1 1
0
""") == "1", "zero matrix must have period 1"

# One-dimensional boundary case
assert solve("""\
1 4
0001
""") == "1", "last-column 1 is already fixed"

# One-dimensional nontrivial boundary
assert solve("""\
1 4
0100
""") == "4", "first 1 in column 2 requires period 4"

# Maximum-area square, all values equal to 1
case = "1000 1000\n" + ("1" * 1000 + "\n") * 1000
assert solve(case) == "1024", "maximum-area all-one matrix"

# Row-oriented boundary case
assert solve("""\
4 1
1
0
0
0
""") == "4", "top-row 1 requires period 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`| 1 | Kích thước tối thiểu và ma trận hoàn toàn bằng 0 | 
|`1 4 / 0001`| 1 | Một ma trận khác 0 có thể có chu kỳ nhỏ hơn chiều lớn hơn của nó | 
|`1 4 / 0100`| 4 | Xử lý từng cột một của cột khác 0 đầu tiên | 
|`1000 x 1000`tất cả những cái | 1024 | Diện tích tối đa cho phép và làm tròn lũy thừa hai | 
|`4 1 / 1,0,0,0`| 4 | Trường hợp dựa trên hàng đối xứng | 

## Vỏ cạnh 

Ma trận toàn 0 là trường hợp ngoại lệ đối với phát biểu rằng ma trận khác 0 cần ít nhất một khoảng thời gian (\min(n,m)). Vì```
2 3
000
000
```không có cột hoặc hàng đầu tiên khác 0. Thuật toán phát hiện điều này và ngay lập tức trả về (1). Áp dụng phép toán tiền tố cho số 0 sẽ tạo ra số 0, do đó, khoảng thời gian dương tối thiểu thực sự là (1). 

Ma trận (1\times m) không có tương tác hàng nào cả. Coi như```
1 4
0001
```Cột khác 0 đầu tiên là (4), vì vậy 

[ 
L=\max(1,4-4+1)=1. 
] 

Câu trả lời là (1). Tổng tiền tố của`0001`modulo (2) vẫn còn`0001`. 

Bây giờ hãy xem xét```
1 4
0100
```Cột khác 0 đầu tiên là (2), cho 

[ 
L=\max(1,4-2+1)=3. 
] 

Các lũy thừa của hai số dưới đây (3) là (1) và (2), nên đáp án là (4). Phần (+1) bổ sung trong (m-c+1) là yếu tố làm cho phép tính ranh giới này trở nên chính xác. 

Lý luận tương tự hoạt động theo chiều dọc. Vì```
4 1
1
0
0
0
```hàng khác 0 đầu tiên là (1), vì vậy 

[ 
L=\max(1,4-1+1)=4. 
] 

Câu trả lời là (4). Một (1) ở đầu ma trận một cột được tích lũy nhiều lần xuống dưới, do đó nó không trở về vị trí ban đầu cho đến khi độ dài dịch chuyển đạt đến toàn bộ chiều. 

Cuối cùng, hãy xem xét một ma trận khác 0 có kích thước của cả hai đều đủ lớn để lũy thừa nhỏ của 2 có vẻ hấp dẫn. Đối với bất kỳ (q<\min(n,m)), lũy thừa hai sẽ tạo ra tính tuần hoàn theo (q), trong khi các ô biên buộc các dải ban đầu tương ứng về 0. Hai thuộc tính đó lan truyền cho đến khi mọi ô đều bằng 0. Do đó, một ma trận khác 0 không thể có (q) là chu kỳ của nó. Đây là lý do tại sao thuật toán không bao giờ cần kiểm tra lũy thừa của hai bên dưới chiều nhỏ hơn. 

Nếu bạn muốn, tôi cũng có thể cung cấp một phiên bản biên tập cuộc thi ngắn hơn của cùng một giải pháp hoặc một bằng chứng nặng về đại số hơn về đẳng thức lũy thừa hai.
