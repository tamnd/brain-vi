---
title: "CF 105873C - Dấu hiệu mới của Celayas"
description: "Trong vấn đề này, chúng ta có hai chuỗi. Cái đầu tiên là văn bản hiện được in trên bảng hiệu và cái thứ hai là văn bản sẽ xuất hiện."
date: "2026-06-25T14:26:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105873
codeforces_index: "C"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105873
solve_time_s: 51
verified: true
draft: false
---

[CF 105873C - Dấu hiệu mới của Celayas](https://codeforces.com/problemset/problem/105873/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong vấn đề này, chúng ta có hai chuỗi. Cái đầu tiên là văn bản hiện được in trên bảng hiệu và cái thứ hai là văn bản sẽ xuất hiện. Việc điều chỉnh duy nhất được phép là một thao tác đơn lẻ chọn một phần liên tục của chuỗi hiện tại, cắt nó thành hai phần liên tiếp và hoán đổi hai phần đó. Nhiệm vụ là quyết định xem có thể đạt được chuỗi mục tiêu hay không và nếu có thì xuất ra một lựa chọn hợp lệ về phân đoạn và vị trí phân chia. 

Thao tác này có thể được xem như lấy một chuỗi con và xoay nó sang trái. Nếu khoảng được chọn là từ`l`ĐẾN`r`, và cuối cùng`k`các ký tự được chuyển lên phía trước, phần giữa thay đổi từ:`A[l:r+1]`vào trong:`A[r-k+1:r+1] + A[l:r-k+1]`Mọi thứ bên ngoài khoảng thời gian đó vẫn giữ nguyên như cũ. 

Chiều dài của chuỗi có thể đạt tới`100000`, vì vậy việc thử mọi khoảng và mọi phép chia có thể là không khả thi. có khoảng`n^3`các hoạt động có thể thực hiện được nếu được thực hiện trực tiếp, sẽ xoay quanh`10^15`kiểm tra trong trường hợp xấu nhất. Chúng ta cần một giải pháp gần tuyến tính hoặc`n log n`. 

Điều đầu tiên cần quan sát là các vị trí nằm ngoài khoảng được sửa đổi phải chính xác. Điều này cho chúng ta khoảng thời gian nhỏ nhất có thể đã thay đổi: từ vị trí đầu tiên nơi`A`Và`B`khác nhau đến vị trí cuối cùng nơi chúng khác nhau. 

Một lỗi phổ biến là để trống phần được di chuyển hoặc toàn bộ khoảng đã chọn. Thao tác này yêu cầu cả hai phần bên trong khoảng đã chọn đều hợp lệ, do đó điểm phân tách phải để lại ít nhất một ký tự trong phần đầu tiên. Ví dụ: nếu:```
A = ABC
B = CAB
```câu trả lời là có thể với:```
0 2 1
```nhưng phép chia di chuyển cả ba ký tự sẽ không phải là thao tác hợp lệ. 

Một trường hợp cạnh khác là khi các chuỗi đã bằng nhau. Câu trả lời đúng vẫn là`Yes`, bởi vì không được phép thực hiện thay đổi hiệu quả. Vì:```
A = AAA
B = AAA
```đầu ra có thể mô tả độ dài một khoảng với`k = 0`. Một giải pháp chỉ tìm kiếm những thông tin không khớp sẽ từ chối trường hợp này một cách không chính xác. 

Trường hợp phức tạp cuối cùng xuất hiện khi khoảng thay đổi chứa các ký tự khớp xung quanh những khác biệt thực tế. Vì:```
A = AXXB
B = AXBX
```hoạt động phải bao gồm các vị trí xung quanh nếu vòng quay cần chúng. Chỉ kiểm tra xem các ký tự không khớp có tạo thành hoán vị hay không là chưa đủ, vì vị trí của chúng sau khi xoay rất quan trọng. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là thử mọi khoảng thời gian có thể và mọi điểm phân chia có thể. Với mỗi lựa chọn, chúng ta xây dựng chuỗi kết quả và so sánh nó với chuỗi đích. Điều này đúng vì mọi hoạt động có thể đều được xem xét. Tuy nhiên, có`O(n^2)`khoảng thời gian và lên đến`O(n)`chia nhỏ cho mỗi khoảng thời gian, đưa ra`O(n^3)`thời gian. Với`n = 100000`, điều này vượt xa những gì có thể chạy. 

Quan sát hữu ích là thao tác chỉ xoay một chuỗi con. Đầu tiên, tiền tố trước phần được thay đổi và hậu tố sau phần đó phải khớp nhau. Điều này có nghĩa là khu vực thú vị duy nhất nằm giữa điểm không khớp đầu tiên và cuối cùng. 

Giả sử khoảng tối thiểu này là`[l, r]`. Trong khoảng này, chúng ta cần kiểm tra xem chuỗi con đích có phải là chuỗi xoay của chuỗi con gốc hay không. Một phép quay có thể được tìm thấy một cách hiệu quả bằng cách đặt chuỗi con ban đầu hai lần liên tiếp. Bất kỳ phép quay nào của một chuỗi có độ dài`m`xuất hiện dưới dạng chuỗi con của chuỗi nhân đôi có độ dài`m`. 

Vì vậy, chúng tôi rút gọn vấn đề thành một thao tác khớp mẫu đơn. Chúng tôi tìm kiếm`B[l:r+1]`bên trong`(A[l:r+1] + A[l:r+1])`. Nếu nó bắt đầu ở vị trí`d`, sau đó chuỗi con được xoay sang trái`d`nhân vật. Tham số đầu ra được yêu cầu là`k = length - d`, bởi vì`k`là số ký tự được di chuyển từ cuối lên trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm chỉ số đầu tiên`l`Ở đâu`A[l]`Và`B[l]`khác nhau và chỉ số cuối cùng`r`chúng khác nhau ở chỗ nào. Nếu không có chỉ mục đó, hãy xuất ra bất kỳ thao tác không thay đổi hợp lệ nào. 
2. Lấy chuỗi con`A[l:r+1]`và chuỗi con đích tương ứng`B[l:r+1]`. Đây là những phần duy nhất có thể khác sau khi phẫu thuật. Nếu thao tác ban đầu tồn tại thì chuỗi con đích này phải là chuỗi xoay của chuỗi con ban đầu. 
3. Tạo chuỗi nhân đôi`A[l:r+1] + A[l:r+1]`và tìm kiếm`B[l:r+1]`bên trong nó. Sử dụng KMP để tìm kiếm vẫn tuyến tính. 
4. Nếu mẫu xuất hiện bắt đầu từ vị trí`d`, chuỗi con được xoay sang trái bởi`d`các vị trí. Số tiền được chuyển từ cuối là`k = length - d`. đầu ra`l`,`r`, Và`k`. 
5. Nếu không tìm thấy mẫu thì không có thao tác hợp lệ nào tồn tại, vì vậy đầu ra`No`. 

Tại sao nó hoạt động: không thể sửa đổi khoảng ngoài khoảng không khớp đầu tiên và cuối cùng vì các ký tự đó đã khớp và việc xoay sẽ chỉ ảnh hưởng đến khoảng đã chọn. Bên trong khoảng còn lại, phép biến đổi duy nhất có thể xảy ra là sự dịch chuyển theo chu kỳ. Tìm kiếm chuỗi kép kiểm tra chính xác xem sự dịch chuyển theo chu kỳ như vậy có tồn tại hay không và sự dịch chuyển được trả về sẽ trực tiếp xác định sự phân tách được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def kmp_search(text, pattern):
    m = len(pattern)
    if m == 0:
        return 0

    pi = [0] * m
    j = 0
    for i in range(1, m):
        while j and pattern[i] != pattern[j]:
            j = pi[j - 1]
        if pattern[i] == pattern[j]:
            j += 1
        pi[i] = j

    j = 0
    for i, c in enumerate(text):
        while j and c != pattern[j]:
            j = pi[j - 1]
        if c == pattern[j]:
            j += 1
        if j == m:
            return i - m + 1
    return -1

def solve():
    a = input().strip()
    b = input().strip()
    n = len(a)

    l = 0
    while l < n and a[l] == b[l]:
        l += 1

    if l == n:
        print("Yes")
        print(0, 0, 0)
        return

    r = n - 1
    while a[r] == b[r]:
        r -= 1

    s = a[l:r + 1]
    t = b[l:r + 1]
    m = len(s)

    pos = kmp_search(s + s, t)

    if pos == -1 or pos >= m:
        print("No")
        return

    k = m - pos
    if k == m:
        k = 0

    print("Yes")
    print(l, r, k)

solve()
```Đầu tiên, mã sẽ cô lập vùng duy nhất có thể khác nhau. Điều này ngăn chặn những công việc không cần thiết đối với tiền tố và hậu tố đã đúng. 

Hàm KMP xây dựng mảng tiền tố cho chuỗi con đích và tìm lần xuất hiện đầu tiên của nó trong chuỗi con nguồn nhân đôi. Chỉ tìm kiếm chuỗi con nhân đôi là đủ vì mọi vòng quay hợp lệ đều bắt đầu ở đâu đó bên trong bản sao đầu tiên. 

Việc chuyển đổi từ vị trí tìm thấy sang`k`là phần tinh tế. Vị trí trùng khớp là số ký tự được di chuyển từ trước ra sau. Bài toán yêu cầu di chuyển số từ sau ra trước, do đó các giá trị bổ sung cho nhau. 

Trường hợp đẳng thức được xử lý riêng vì khoảng không khớp không tồn tại. In ấn`k = 0`tránh mọi sự phân chia không hợp lệ. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
A = ABC
B = ACB
```thuật toán hoạt động như sau. 

| Bước | tôi | r | chuỗi con A | chuỗi con B | vị trí phù hợp | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 2 | BC | CB | 1 | Có | 
| Đầu ra | 1 | 2 | B | C | k = 1 | 1 2 1 | 

Khoảng thời gian là`BC`. Xoay nó sang trái một cái sẽ cho`CB`, tạo ra chuỗi mục tiêu. 

Một ví dụ khác:```
A = ABCDE
B = ACBDE
```| Bước | tôi | r | chuỗi con A | chuỗi con B | vị trí phù hợp | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 2 | BC | CB | 1 | Có | 
| Đầu ra | 1 | 2 | BC | CB | k = 1 | 1 2 1 | 

Dấu vết chứng tỏ rằng chỉ phần giữa đã thay đổi mới được xem xét. Các ký tự xung quanh bị bỏ qua vì chúng đã được sửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc tìm kiếm sự không khớp và chạy KMP đều mất thời gian tuyến tính | 
| Không gian | O(n) | Mảng tiền tố và chuỗi con nhân đôi sử dụng bộ nhớ tuyến tính | 

Độ dài chuỗi tối đa là`100000`, do đó, thuật toán tuyến tính dễ dàng phù hợp với giới hạn lập trình cạnh tranh thông thường về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def kmp_search(text, pattern):
        m = len(pattern)
        pi = [0] * m
        j = 0
        for i in range(1, m):
            while j and pattern[i] != pattern[j]:
                j = pi[j - 1]
            if pattern[i] == pattern[j]:
                j += 1
            pi[i] = j
        j = 0
        for i, c in enumerate(text):
            while j and c != pattern[j]:
                j = pi[j - 1]
            if c == pattern[j]:
                j += 1
            if j == m:
                return i - m + 1
        return -1

    a = input().strip()
    b = input().strip()

    n = len(a)
    l = 0
    while l < n and a[l] == b[l]:
        l += 1

    if l == n:
        return "Yes\n0 0 0\n"

    r = n - 1
    while a[r] == b[r]:
        r -= 1

    s = a[l:r + 1]
    t = b[l:r + 1]
    pos = kmp_search(s + s, t)

    if pos == -1 or pos >= len(s):
        return "No\n"

    k = len(s) - pos
    if k == len(s):
        k = 0
    return f"Yes\n{l} {r} {k}\n"

assert solve_case("ABC\nACB\n") == "Yes\n1 2 1\n"
assert solve_case("AAA\nAAA\n") == "Yes\n0 0 0\n"
assert solve_case("ABCDE\nACBDE\n") == "Yes\n1 2 1\n"
assert solve_case("ABCD\nABDC\n") == "Yes\n2 3 1\n"
assert solve_case("ABC\nBAC\n") == "Yes\n0 1 1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`AAA / AAA`| Có | chuỗi đã đúng | 
|`ABCDE / ACBDE`| Có | vòng quay nội bộ nhỏ | 
|`ABCD / ABDC`| Có | phong trào bên hậu tố | 
|`ABC / BAC`| Có | phong trào tiền tố | 

## Vỏ cạnh 

Đối với trường hợp chuỗi bằng nhau:```
A = AAA
B = AAA
```thuật toán không tìm thấy sự không phù hợp. Nó ngay lập tức trả về một hoạt động không hoạt động. Điều này hoạt động vì số lượng thao tác được phép bằng 0 hoặc một, do đó không thể thay đổi gì được chấp nhận. 

Đối với trường hợp phần bị thay đổi là toàn bộ vùng có ý nghĩa:```
A = ABC
B = CAB
```khoảng thời gian không khớp trở thành`[0,2]`. Chuỗi nhân đôi là`ABCABC`, Và`CAB`xuất hiện bắt đầu từ chỉ mục`2`. Thuật toán chuyển đổi điều này thành`k = 1`, đưa ra một phép quay hợp lệ. 

Đối với trường hợp không phải là phép quay:```
A = ABCD
B = ACBD
```chuỗi con ở giữa là`ABCD`so với`ACBD`. Tìm kiếm`ACBD`TRONG`ABCDABCD`không thành công, do đó thuật toán sẽ loại bỏ chính xác phép biến đổi. 

Đối với trường hợp ranh giới chỉ có hai ký tự:```
A = AB
B = BA
```khoảng có độ dài bằng hai. Chuỗi nhân đôi là`ABAB`, Và`BA`xuất hiện ở chỉ mục một. Đầu ra của thuật toán`k = 1`, đây là phép chia hợp lệ duy nhất.
