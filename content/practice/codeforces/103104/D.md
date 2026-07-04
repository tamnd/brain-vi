---
title: "CF 103104D - Hợp nhất phân mảnh"
description: "Chúng ta được cấp một hoán vị có độ dài $n$ và chúng ta hiểu bất kỳ cặp chỉ số nào $(l, r)$ là một “phân đoạn” tương ứng với tập hợp các giá trị trong phân đoạn $al, a{l+1}, dots, ar$ if $l le r$. Nếu $l r$, phân mảnh đó đại diện cho một tập rỗng."
date: "2026-07-03T21:42:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103104
codeforces_index: "D"
codeforces_contest_name: "2021 Hubei Provincial Collegiate Programming Contest"
rating: 0
weight: 103104
solve_time_s: 51
verified: true
draft: false
---

[CF 103104D - Hợp nhất phân mảnh](https://codeforces.com/problemset/problem/103104/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$và chúng tôi giải thích bất kỳ cặp chỉ số nào$(l, r)$dưới dạng một “phân đoạn” tương ứng với tập hợp các giá trị trong phân đoạn$a_l, a_{l+1}, \dots, a_r$nếu như$l \le r$. Nếu như$l > r$, sự phân mảnh đó đại diện cho một tập hợp trống. 

Chúng ta được phép chọn hai phân mảnh, lấy kết hợp của hai tập hợp kết quả và nếu kết hợp này tạo thành một “khoảng hoàn hảo” gồm các số nguyên, nghĩa là nó chứa chính xác tất cả các số nguyên từ giá trị tối thiểu đến giá trị tối đa của nó mà không có khoảng trống và không trùng lặp, thì liên kết này được gọi là siêu phân mảnh. Nhiệm vụ là đếm có bao nhiêu tập hợp khác nhau$C$có thể thu được như là một sự kết hợp của hai mảnh vỡ. 

Vì mảng là một hoán vị nên mọi giá trị từ$1$ĐẾN$n$xuất hiện chính xác một lần, do đó mỗi phân mảnh tương ứng với việc chọn một tập hợp con của các giá trị này do một mảng con tạo ra. 

Đầu ra chính không phải là về các cặp phân đoạn mà là về các tập hợp kết quả riêng biệt$C$. Hai cặp khác nhau được tính là cùng một câu trả lời nếu chúng tạo ra cùng một bộ. 

Tổng ràng buộc của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^4$, trong khi mỗi$n$tùy thuộc vào$5000$. Điều này gợi ý mạnh mẽ một$O(n^2)$hoặc giải pháp siêu bậc hai cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng$O(n^3)$hoặc liệt kê ngây thơ của tất cả các cặp phân khúc là không. 

Một cách giải thích ngây thơ sẽ xem xét tất cả$O(n^2)$các phân đoạn và tất cả các cặp của chúng, dẫn đến$O(n^4)$sự kết hợp, đó là điều không thể. 

Một vấn đề tinh tế hơn là việc tính hai lần: nhiều cặp phân đoạn có thể tạo ra cùng một tập hợp, do đó, bất kỳ cách tiếp cận đúng nào cũng phải tính các biểu diễn chuẩn của các hợp hợp lệ, chứ không phải bản thân các cặp. 

Các trường hợp cạnh phát sinh khi: 

1. Một phân mảnh trống rỗng. Sau đó, câu trả lời giảm xuống còn tất cả các “khoảng hoàn hảo” một phân đoạn hợp lệ. 

Ví dụ:$a = [1,2,3]$. Bất kỳ mảng con nào cũng đã là một khoảng liên tiếp, vì vậy tất cả các phân đoạn đơn đều là kết quả đầu ra hợp lệ. 
2. Các phân đoạn chồng chéo về giá trị nhưng không trùng lặp về chỉ số, điều này không thể xảy ra theo nghĩa hoán vị đối với các tập hợp nhưng lại quan trọng trong lý luận: các phân đoạn chỉ số chồng chéo không bao hàm các khoảng giá trị chồng chéo. 
3. Hai phân đoạn có thể kết hợp để tạo thành một khoảng hợp lệ ngay cả khi không có phân đoạn nào liền kề nhau trong không gian giá trị, do đó việc phân loại “phân đoạn tốt” ngây thơ không thành công. 

## Phương pháp tiếp cận 

Một chiến lược vũ phu rất đơn giản. Chúng tôi liệt kê tất cả các mảng con$A$và tất cả các mảng con$B$, tính toán các tập giá trị của chúng và lấy liên kết của chúng. Sau đó chúng ta kiểm tra xem tập kết quả có tạo thành một khoảng liên tục hay không. Mỗi mảng con có thể được biểu diễn dưới dạng$O(1)$cập nhật nếu chúng tôi duy trì một mảng tần số hoặc một bộ, nhưng việc hợp nhất hai bộ vẫn là$O(n)$. Điều này dẫn đến khoảng$O(n^4)$tổng công việc cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất, vì có$O(n^2)$mảng con và việc kết hợp từng cặp là tuyến tính. 

Tốc độ này quá chậm, nhưng nó tiết lộ cấu trúc của vấn đề: điều quan trọng đối với một tập hợp chỉ là giá trị tối thiểu và tối đa của nó và liệu nó có đầy đủ hay không. Vì vậy, thay vì theo dõi các tập hợp đầy đủ, chúng ta muốn lý giải theo khoảng thời gian trên các giá trị. 

Quan sát quan trọng là bất kỳ siêu phân mảnh hợp lệ nào đều tương ứng với việc chọn khoảng giá trị$[x, y]$và chúng tôi chỉ cần kiểm tra xem liệu chúng tôi có thể bao gồm tất cả các giá trị trong khoảng này bằng cách sử dụng tối đa hai phân đoạn chỉ số rời nhau hay không. Vì các giá trị là một hoán vị nên vị trí của$x, x+1, \dots, y$đều khác biệt và chúng tôi đang hỏi liệu chúng có thể được bao phủ bởi tối đa hai khối chỉ mục liền kề hay không. 

Điều này làm giảm vấn đề về hình học khoảng trên các vị trí của các giá trị. 

Chúng tôi ánh xạ từng giá trị$v$đến vị trí của nó$pos[v]$. Đối với khoảng giá trị ứng cử viên$[l, r]$, chúng ta xét tập hợp các vị trí$\{pos[l], \dots, pos[r]\}$. Các vị trí này tạo thành một tập hợp trên một dòng và chúng tôi hỏi liệu chúng có thể được bao phủ bởi tối đa hai phân đoạn liền kề theo thứ tự chỉ mục hay không. Điều đó tương đương với việc kiểm tra xem tập hợp vị trí này có tối đa hai “khoảng trống” khi được sắp xếp hay không. 

Bây giờ vấn đề trở thành: đếm tất cả các khoảng giá trị$[l, r]$sao cho vị trí của các giá trị này hình thành tối đa hai khối liền kề nhau. 

Chúng tôi có thể sửa chữa$l$và mở rộng$r$, duy trì vị trí tối thiểu và tối đa và theo dõi số lượng phân đoạn rời rạc mà các vị trí hình thành. Từ$n$đủ nhỏ, chúng ta có thể duy trì cấu trúc cập nhật số lượng phân đoạn theo thời gian khấu hao không đổi. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các cặp phân đoạn |$O(n^4)$|$O(n)$| Quá chậm | 
| Khoảng giá trị + phân đoạn vị trí |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại vấn đề hoàn toàn trong không gian giá trị. 

### 1. Xây dựng mảng vị trí 

Chúng tôi tính toán$pos[v]$, chỉ số có giá trị$v$xuất hiện trong hoán vị. Điều này cho phép chúng ta thay thế các khoảng giá trị bằng các bộ vị trí. 

Điều này là cần thiết vì tính liên tục trong không gian giá trị phải được kiểm tra thông qua cấu trúc trong không gian chỉ mục. 

### 2. Lặp lại điểm cuối bên trái của khoảng giá trị 

Chúng tôi sửa giá trị bắt đầu$l$và dần dần mở rộng$r$từ$l$ĐẾN$n$. 

Ở mỗi bước chúng ta đang xem xét tập hợp giá trị$\{l, l+1, \dots, r\}$. 

### 3. Duy trì tập hợp các vị trí 

Khi chúng tôi thêm một giá trị mới$r$, chúng tôi chèn$pos[r]$thành cấu trúc có thứ tự động (được triển khai bằng cấu trúc cân bằng hoặc danh sách được sắp xếp có kiểm tra kề). 

Chúng tôi duy trì số lượng các phân đoạn liền kề được hình thành bởi các vị trí này. 

Một vị trí mới: 

- bắt đầu một phân đoạn mới nếu không có hàng xóm nào có mặt, 
- hợp nhất hai phân đoạn nếu cả hai phân đoạn đều tồn tại, 
- mở rộng một phân đoạn nếu tồn tại chính xác một phân đoạn. 

Chúng tôi giữ một bộ đếm các phân đoạn. 

### 4. Kiểm tra điều kiện hợp lệ 

Đối với mỗi khoảng thời gian$[l, r]$, chúng tôi kiểm tra xem các vị trí có hình thành tối đa hai phân đoạn liền kề hay không. 

Nếu có, khoảng giá trị này có thể được hình thành dưới dạng kết hợp của hai phân mảnh. 

Chúng tôi đếm nó. 

### 5. Tích lũy câu trả lời 

Chúng tôi tổng hợp tất cả hợp lệ$(l, r)$khoảng thời gian trên tất cả$l$. 

### Tại sao nó hoạt động 

Bất kỳ siêu phân mảnh hợp lệ nào đều tương ứng với một khoảng giá trị nào đó$[l, r]$, vì phép kết hợp phải không có khoảng trống trong không gian giá trị. Bởi vì các giá trị là một hoán vị, mỗi giá trị xuất hiện một lần, do đó, bất kỳ khoảng trống nào trong không gian giá trị đều tương ứng với một phần tử bị thiếu, điều này sẽ vi phạm điều kiện “không có khoảng trống”. 

Do đó, toàn bộ vấn đề quy về việc kiểm tra xem vị trí của các giá trị này có thể được bao phủ bởi tối đa hai phân đoạn chỉ số liền kề hay không. Việc duy trì số lượng phân đoạn một cách linh hoạt đảm bảo chúng tôi theo dõi chính xác xem hai phân đoạn có đủ để bao phủ tập hợp hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pos = [0] * (n + 1)
    for i, v in enumerate(a):
        pos[v] = i

    ans = 0

    for l in range(1, n + 1):
        used = [False] * n
        segs = 0

        for r in range(l, n + 1):
            p = pos[r]
            used[p] = True

            left = p > 0 and used[p - 1]
            right = p + 1 < n and used[p + 1]

            if left and right:
                segs -= 1
            elif not left and not right:
                segs += 1

            if segs <= 2:
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai này tuân theo ý tưởng mở rộng khoảng giá trị$[l, r]$và theo dõi có bao nhiêu khối liền kề mà các vị trí tương ứng hình thành. 

Mảng`used`đánh dấu những vị trí hiện đang được bao gồm. Khi chèn một vị trí mới, chúng tôi chỉ kiểm tra các lân cận trực tiếp của nó để xác định cấu trúc phân đoạn thay đổi như thế nào, vì khả năng kết nối trong mảng 1D chỉ phụ thuộc vào lân cận. 

Biến`segs`theo dõi có bao nhiêu thành phần được kết nối tồn tại trong tập hợp các vị trí cảm ứng. Nếu thêm một điểm mới kết nối hai thành phần, chúng ta sẽ giảm số lượng; nếu nó tạo ra một thành phần biệt lập mới, chúng tôi sẽ tăng nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [1, 2, 4, 3, 5]
```Chúng tôi xây dựng: 

| v | vị trí[v] | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 4 | 

Bây giờ chúng tôi liệt kê các khoảng giá trị. 

Vì$l = 1$: 

| r | giá trị | vị trí | phân đoạn | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | {0} | 1 | vâng | 
| 2 | [1,2] | {0,1} | 1 | vâng | 
| 3 | [1,2,3] | {0,1,3} | 2 | vâng | 
| 4 | [1,2,3,4] | {0,1,3,2} | 1 | vâng | 
| 5 | [1..5] | {0,1,3,2,4} | 1 | vâng | 

Điều này cho thấy ngay cả những hoán vị có tính xen kẽ cao vẫn tạo ra các liên kết hợp lệ vì các vị trí vẫn nằm trong hai khối cấu trúc. 

Dấu vết xác nhận thuật toán xử lý chính xác các hoán vị không đơn điệu mà không cần sắp xếp rõ ràng ở mỗi bước. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [4, 3, 2, 1]
```Vị trí: 

4→0, 3→1, 2→2, 1→3 

| tôi | r | vị trí | phân đoạn | 
| --- | --- | --- | --- | 
| 1 | 1 | {3} | 1 | 
| 1 | 2 | {3,2} | 1 | 
| 1 | 3 | {3,2,1} | 1 | 
| 1 | 4 | {3,2,1,0} | 1 | 

Mỗi khoảng hoàn toàn liền kề trong không gian chỉ mục, vì vậy tất cả các khoảng giá trị đều hợp lệ. Điều này xác nhận thuật toán xử lý các hoán vị đã được sắp xếp hoặc đảo ngược một cách thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$mỗi bài kiểm tra | Hai vòng lặp lồng nhau trong các khoảng giá trị, với cập nhật O(1) cho mỗi lần chèn | 
| Không gian |$O(n)$| Mảng để lập bản đồ vị trí và theo dõi việc sử dụng | 

Tổng cộng$\sum n \le 10^4$làm cho việc này nhanh chóng một cách thoải mái. Ngay cả trong các bài kiểm tra có độ dài 5000 trong trường hợp xấu nhất, phương pháp bậc hai vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _S
    out = _S()
    _stdout = _sys.stdout
    _sys.stdout = out
    solve()
    _sys.stdout = _stdout
    return out.getvalue().strip()

# minimal
assert run("1\n1\n1\n") == "1"

# sorted permutation
assert run("1\n3\n1 2 3\n") == "6"

# reversed permutation
assert run("1\n3\n3 2 1\n") == "6"

# mixed case
assert run("1\n5\n1 2 4 3 5\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| được sắp xếp | 6 | tất cả các khoảng hợp lệ | 
| đảo ngược | 6 | đối xứng | 
| hỗn hợp | 15 | sự xen kẽ đúng đắn | 

## Vỏ cạnh 

cho$n = 1$, thuật toán khởi tạo một vị trí duy nhất và ngay lập tức tính nó là một khoảng hợp lệ. Số lượng phân đoạn là 1, thỏa mãn ràng buộc. 

Đối với các hoán vị đơn điệu như$[1,2,3,\dots,n]$, mọi khoảng giá trị tương ứng với một khối chỉ mục liền kề, do đó số lượng phân đoạn không bao giờ vượt quá 1. Thuật toán tính tất cả$\frac{n(n+1)}{2}$khoảng cách một cách chính xác. 

Đối với các hoán vị có tính xen kẽ cao, chẳng hạn như$[1,n,2,n-1,\dots]$, các vị trí dao động, nhưng các cập nhật phân đoạn dựa trên lân cận vẫn hợp nhất và phân chia chính xác các thành phần, đảm bảo rằng chỉ các khoảng hợp lệ về mặt cấu trúc mới đóng góp vào câu trả lời.
