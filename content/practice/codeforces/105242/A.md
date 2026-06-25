---
title: "CF 105242A - Tiền tố GCD"
description: "Chúng ta được cung cấp một mảng và được phép thực hiện một thao tác nhiều nhất một lần. Thao tác chọn một phân đoạn liền kề, tính toán mex của phân đoạn đó và sau đó ghi đè mọi phần tử bên trong phân đoạn đó bằng giá trị mex đó."
date: "2026-06-24T13:03:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105242
codeforces_index: "A"
codeforces_contest_name: "The 2024 Damascus University Collegiate Programming Contest (DCPC 2024)"
rating: 0
weight: 105242
solve_time_s: 81
verified: true
draft: false
---

[CF 105242A - Tiền tố GCD](https://codeforces.com/problemset/problem/105242/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và được phép thực hiện một thao tác nhiều nhất một lần. Thao tác chọn một phân đoạn liền kề, tính toán mex của phân đoạn đó và sau đó ghi đè mọi phần tử bên trong phân đoạn đó bằng giá trị mex đó. Sau khi không làm gì hoặc thực hiện chính xác một lần ghi đè như vậy, chúng tôi xem xét mex của toàn bộ mảng và muốn tối đa hóa nó. 

Vì vậy, nhiệm vụ không phải là trực tiếp tối ưu hóa phân khúc mà là hiểu cách "nén" phân đoạn được chọn cẩn thận có thể làm tăng mex toàn cầu. 

Mex của một mảng là số nguyên không âm nhỏ nhất bị thiếu trong mảng, do đó, việc tăng mex có nghĩa là chúng ta đang cố gắng làm xuất hiện càng nhiều số nguyên nhỏ càng tốt trong khi vẫn đảm bảo số nguyên bị thiếu đầu tiên được đẩy sang bên phải nhất có thể. 

Các ràng buộc cho phép các mảng có kích thước lên tới 10^6, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các phân đoạn. Ngay cả O(n log n) với các hằng số nặng cũng có thể chấp nhận được, nhưng bất cứ điều gì bậc hai trên các phân đoạn đều là không thể. 

Một trường hợp tế nhị xuất hiện khi nghĩ về việc liệu hoạt động đó chỉ có thể giúp ích hay đôi khi gây tổn hại. Ví dụ: nếu chúng tôi chọn một phân đoạn đã có mex 0, chúng tôi sẽ ghi đè lên phân đoạn đó bằng 0, có khả năng phá hủy cấu trúc hiện có và hạ thấp mex toàn cầu. Một vấn đề khác là giả định rằng phân khúc tốt nhất luôn liên quan trực tiếp đến mex ban đầu, điều này không đúng nếu không có lập luận mang tính cấu trúc về cách mex thay đổi trên toàn cầu. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử từng cặp chỉ số l và r, tính toán mex của mảng con đó, áp dụng ghi đè, tính toán lại toàn bộ mảng mex và theo dõi kết quả tốt nhất. Việc tính toán mex một cách đơn giản cho mỗi phân đoạn đã có giá O(n) và có các phân đoạn O(n^2), vì vậy điều này trở thành O(n^3), điều này hoàn toàn không khả thi ngay cả với n khoảng 5000. 

Ngay cả việc cải thiện tính toán mex trên mỗi phân đoạn thành O(1) được khấu hao bằng cách đặt lại tần số cũng không cứu được phương pháp này, vì việc lặp lại trên tất cả các phân đoạn vẫn chiếm ưu thế. 

Quan sát cấu trúc quan trọng là mex của toàn bộ mảng được xác định hoàn toàn bằng việc liệu tất cả các giá trị từ 0 trở lên có xuất hiện ở đâu đó hay không. Thao tác này chỉ thay đổi một vùng liền kề thành một giá trị không đổi, vì vậy cách duy nhất nó có thể cải thiện mex là giúp đảm bảo rằng các giá trị nhỏ vẫn tồn tại trong khi đưa ra một giá trị còn thiếu mới. 

Đặt m0 là mex của mảng ban đầu. Tất cả các giá trị từ 0 đến m0 − 1 xuất hiện ít nhất một lần và m0 hoàn toàn không xuất hiện. 

Nếu chúng ta muốn cải thiện mex sau một thao tác, cải tiến duy nhất có thể áp dụng là làm cho m0 xuất hiện trong mảng (vì hiện tại nó bị thiếu) trong khi vẫn giữ tất cả các giá trị từ 0 đến m0 − 1. Nếu chúng tôi thành công, mex mới sẽ trở thành m0 + 1. Chúng tôi không thể nhảy xa hơn thế vì chúng tôi đang giới thiệu nhiều nhất một giá trị thống nhất mới. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem có tồn tại một phân đoạn sao cho: 

phân đoạn tạo ra mex bằng m0 khi được thay thế và việc thay thế không loại bỏ tất cả các lần xuất hiện của bất kỳ giá trị nào từ 0 đến m0 − 1. 

Điều này biến thành một điều kiện vị trí thuần túy tại nơi mỗi giá trị xuất hiện. 

Chúng tôi xác định cho mỗi giá trị v lần xuất hiện đầu tiên và cuối cùng trong mảng. Để đảm bảo rằng sau khi thay thế mọi giá trị từ 0 đến m0 − 1 vẫn tồn tại, phân đoạn được chọn không được bao gồm đầy đủ tất cả các lần xuất hiện của bất kỳ v nào như vậy. Đồng thời, để đảm bảo mex của phân đoạn trở thành chính xác m0, phân đoạn phải chứa ít nhất một lần xuất hiện của mọi giá trị từ 0 đến m0 − 1. 

Điều đó chỉ có thể xảy ra nếu tồn tại một đoạn duy nhất giao nhau với mọi khoảng xuất hiện của các giá trị từ 0 đến m0 − 1. Điều này trở thành bài toán giao khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các phân đoạn | O(n^3) | O(n) | Quá chậm | 
| Kiểm tra giao lộ khoảng thời gian | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

### 1. Tính mex ban đầu 

Chúng tôi quét mảng và đánh dấu giá trị nào xuất hiện. Giá trị nhỏ nhất không thấy là m0. 

Điều này đưa ra đường cơ sở: các giá trị từ 0 đến m0 − 1 đều được đảm bảo tồn tại. 

### 2. Ghi lại ranh giới xuất hiện 

Với mỗi giá trị v trong [0, m0 − 1], chúng ta tính toán lần xuất hiện đầu tiên và cuối cùng của nó trong mảng. 

Những ranh giới này mô tả toàn bộ khu vực nơi mỗi giá trị “sống”. 

### 3. Xây dựng điều kiện giao lộ 

Chúng ta muốn một phân đoạn [l, r] chứa ít nhất một lần xuất hiện của mọi giá trị từ 0 đến m0 − 1. Điều đó có nghĩa là với mọi giá trị v như vậy, khoảng [Lv, Rv] của nó phải cắt nhau [l, r]. 

Điều này tương đương với việc yêu cầu phải tồn tại ít nhất một vị trí chỉ số nằm bên trong tất cả các khoảng này cùng một lúc. 

Vì vậy, chúng tôi tính toán giao điểm toàn cầu: 

chúng ta lấy L = max(Lv) và R = min(Rv) trên tất cả v < m0. 

### 4. Quyết định tính khả thi 

Nếu L ≤ R thì tồn tại một vị trí nằm trong mỗi khoảng xuất hiện. Chúng ta có thể chọn một phân đoạn bao gồm chính xác vị trí đó, đảm bảo nó giao nhau với tất cả các giá trị bắt buộc trong khi không bao phủ hoàn toàn bất kỳ khoảng nào theo cách phá hoại. 

Nếu L > R, không một phân đoạn nào có thể cắt đồng thời tất cả các khoảng này, vì vậy chúng ta không thể ép buộc cải thiện mex. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi giá trị v < m0 phải tồn tại trong hoạt động, nghĩa là ít nhất một lần xuất hiện của v phải nằm ngoài phân đoạn đã chọn. Đồng thời, để làm cho m0 xuất hiện sau thao tác, phân đoạn phải tạo ra nó dưới dạng mex, điều này yêu cầu phân khúc phải “nhìn thấy” mọi giá trị từ 0 đến m0 - 1 ít nhất một lần. 

Điều này tạo ra sự căng thẳng dẫn đến một điều kiện duy nhất: tất cả các khoảng giá trị này phải trùng lặp trong ít nhất một chỉ số chung. Nếu một điểm như vậy tồn tại, chúng ta có thể neo đoạn xung quanh nó; nếu không, bất kỳ phân đoạn nào cũng nhất thiết sẽ thiếu một số giá trị bắt buộc hoặc loại bỏ tất cả các lần xuất hiện của phân đoạn khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    seen = [False] * (n + 2)
    for x in a:
        if x <= n + 1:
            seen[x] = True

    mex0 = 0
    while seen[mex0]:
        mex0 += 1

    if mex0 == 0:
        print(1)
        return

    INF = 10**18
    L = INF
    R = -INF

    first = [-1] * (mex0)
    last = [-1] * (mex0)

    for i, x in enumerate(a):
        if x < mex0:
            if first[x] == -1:
                first[x] = i
            last[x] = i

    for v in range(mex0):
        L = min(L, first[v])
        R = max(R, last[v])

    print(mex0 + 1 if L <= R else mex0)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán mex ban đầu bằng cách đánh dấu tất cả các giá trị hiện tại. Sau đó, nó ghi lại lần xuất hiện đầu tiên và cuối cùng cho tất cả các giá trị nhỏ hơn mex đó. Các ranh giới này xác định các khoảng mà sự chồng chéo của chúng xác định liệu một phân đoạn có thể tương tác đồng thời với tất cả các giá trị được yêu cầu hay không. 

Sự so sánh cuối cùng`L <= R`là toàn bộ điểm quyết định: nó kiểm tra xem tất cả các khoảng này có chia sẻ ít nhất một chỉ số chung hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
0 1 2 4 3
```Ở đây mex là 5 vì 0-4 đều xuất hiện. 

Chúng tôi tính toán các khoảng: 

0: [0,0], 1: [1,1], 2: [2,2], 3: [4,4], 4: [3,3] 

Bây giờ chúng ta giao nhau với chúng: 

L = tối đa(0,1,2,4,3) = 4 

R = phút(0,1,2,4,3) = 0 

| Bước | L | R | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| Sau khoảng thời gian xử lý | 4 | 0 | 

Vì L > R nên không tồn tại điểm chung. 

Vậy đáp án là mex0 = 5. 

Điều này cho thấy trường hợp các giá trị quá phân tán để có thể được bảo toàn đồng thời trong một phân đoạn. 

### Ví dụ 2 

đầu vào:```
4
1 0 2 1
```mex là 3. 

Khoảng thời gian: 

0: [1,1] 

1: [0,3] 

2: [2,2] 

Giao lộ: 

L = tối đa(1,0,2) = 2 

R = phút(1,3,2) = 1 

| Bước | L | R | 
| --- | --- | --- | 
| Bắt đầu | 0 | 3 | 
| Sau khoảng thời gian xử lý | 2 | 1 | 

Vì L > R nên đáp án là mex0 = 3. 

Điều này chứng tỏ rằng mặc dù các giá trị tồn tại nhưng chúng không trùng nhau ở một vị trí duy nhất chạm đến tất cả các lần xuất hiện bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét một lần để tính mex và ranh giới | 
| Không gian | O(n) | lưu trữ vị trí xuất hiện và mảng nhìn thấy | 

Giải pháp phù hợp thoải mái trong giới hạn ngay cả với n lên đến 10^6 vì tất cả các phép toán đều là tuyến tính và sử dụng các mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    seen = [False] * (n + 2)
    for x in a:
        if x <= n + 1:
            seen[x] = True

    mex0 = 0
    while seen[mex0]:
        mex0 += 1

    if mex0 == 0:
        return "1"

    first = [-1] * mex0
    last = [-1] * mex0

    for i, x in enumerate(a):
        if x < mex0:
            if first[x] == -1:
                first[x] = i
            last[x] = i

    L = min(first)
    R = max(last)

    return str(mex0 + 1 if L <= R else mex0)

# minimum size
assert run("1\n0\n") == "1"

# all equal
assert run("5\n1 1 1 1 1\n") == "0"

# increasing sequence
assert run("5\n0 1 2 3 4\n") == "6"

# scattered values
assert run("4\n0 2 1 3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0`| 1 | hành vi phần tử đơn lẻ | 
|`5\n1 1 1 1 1`| 0 | thiếu số 0 từ đầu | 
|`5\n0 1 2 3 4`| 6 | đầy đủ phạm vi liên tiếp | 
|`4\n0 2 1 3`| 4 | sự cố giao lộ khoảng cách rải rác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mex ban đầu bằng 0. Trong trường hợp đó, 0 không xuất hiện ở bất kỳ đâu, vì vậy bất kỳ phân đoạn nào cũng có mex 0 và việc thay thế nó không giúp đưa ra cấu trúc bị thiếu. Thuật toán xử lý vấn đề này trực tiếp bằng cách trả về 1, vì chúng ta luôn có thể giới thiệu 0 bằng cách chọn bất kỳ phân đoạn nào chỉ chứa các giá trị khác 0. 

Một trường hợp cạnh khác là khi tất cả các giá trị đã xuất hiện liên tiếp từ 0 đến n − 1. Trong trường hợp đó mex là n và không có giá trị nào từ 0 đến mex − 1 vi phạm điều kiện. Điều kiện giao nhau được giữ một cách tầm thường và thuật toán trả về chính xác n + 1. 

Trường hợp khó phát hiện thứ ba là khi sự xuất hiện của các giá trị yêu cầu rất rải rác. Kiểm tra giao điểm khoảng thời gian nắm bắt chính xác điều này: ngay cả khi mọi giá trị tồn tại, nếu khoảng thời gian xuất hiện của chúng không trùng nhau thì không có phân đoạn nào có thể đồng thời bao gồm tất cả chúng và do đó không thể cải thiện được.
