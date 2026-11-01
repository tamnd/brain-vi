---
title: "CF 106439O - Phân chia GCD tối ưu"
description: "Chúng ta được cho một mảng các số nguyên dương. Vị trí phân chia chia nó thành phần bên trái và phần bên phải. Việc phân chia được coi là tốt nếu chúng ta có thể làm cho ước số chung lớn nhất của hai phần bằng nhau sau khi thay đổi nhiều nhất một phần tử mảng."
date: "2026-06-25T09:33:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106439
codeforces_index: "O"
codeforces_contest_name: "Insomnia-26"
rating: 0
weight: 106439
solve_time_s: 44
verified: true
draft: false
---

[CF 106439O - Phân chia GCD tối ưu](https://codeforces.com/problemset/problem/106439/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Vị trí phân chia chia nó thành phần bên trái và phần bên phải. Việc phân chia được coi là tốt nếu chúng ta có thể làm cho ước số chung lớn nhất của hai phần bằng nhau sau khi thay đổi nhiều nhất một phần tử mảng. Phần tử đã thay đổi có thể ở bất kỳ đâu trong mảng và quyết định được đưa ra riêng cho từng phần phân chia có thể xảy ra. 

Đối với mỗi phép chia, câu hỏi không yêu cầu chúng ta tìm giá trị cuối cùng của GCD. Chúng ta chỉ cần biết liệu có thể đạt được một số giá trị chung hay không bằng cách sửa đổi một bên của phần phân chia trong khi vẫn giữ nguyên phía bên kia. 

Độ dài mảng có thể đạt tới$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm. Điều này loại trừ việc kiểm tra từng phần phân chia bằng cách quét hai bên, việc này sẽ tốn kém$O(n^2)$. Chúng ta cần một giải pháp gần tuyến tính hoặc$O(n \log n)$. Bản thân các giá trị có thể lớn bằng$10^9$, do đó các phép toán dựa trên tính chia hết và GCD phải hiệu quả. 

Những trường hợp phức tạp xuất phát từ việc được phép sửa đổi một lần. Sự phân chia trong đó các GCD hiện tại khác nhau không hẳn là xấu. Ví dụ:```
Input:
1
2
6 10

Output:
1
```Sự phân chia duy nhất là giữa hai con số. GCD bên trái là 6 và GCD bên phải là 10. Bằng cách thay đổi phần tử đầu tiên từ 6 thành 10, cả hai bên đều có GCD 10 nên việc phân chia là tốt. Một giải pháp chỉ so sánh tiền tố và hậu tố hiện tại của GCD sẽ từ chối nó một cách không chính xác. 

Một trường hợp cạnh khác là một cạnh chứa nhiều phần tử trong đó chỉ có một phần tử ngăn cản GCD mục tiêu.```
Input:
1
3
6 10 15

Output:
2
```Đối với lần chia đầu tiên, bên trái có GCD 6 và bên phải có GCD 5. Thay đổi 6 thành 5 thì cả hai bên có GCD 5. Đối với lần chia thứ hai, bên trái có GCD 2 và bên phải có GCD 15, đổi 15 thành 2 là được. Điều quan trọng là một bên có thể được sửa chữa nếu tất cả trừ một phần tử đã hỗ trợ GCD mong muốn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi phép chia và tính toán xem liệu một sửa đổi có thể làm cho hai GCD bằng nhau hay không. Để phân chia, chúng ta có thể tính toán các GCD hiện tại và sau đó thử loại bỏ từng phần tử một lần để xem những GCD nào có thể có sau khi sửa đổi. Điều này đúng vì việc thay đổi một phần tử tương đương với việc quyết định rằng tất cả các phần tử khác phải chia hết cho GCD cuối cùng. Tuy nhiên, thực hiện việc này cho mỗi lần phân tách sẽ yêu cầu quét liên tục các phần lớn của mảng. Trong trường hợp xấu nhất, nó trở thành$O(n^2)$, quá chậm. 

Quan sát quan trọng là để một phân đoạn có thể chia hết cho một số giá trị mục tiêu$g$sau khi thay đổi một phần tử, mọi phần tử ngoại trừ có thể một phần tử phải chia hết cho$g$. Chúng ta không cần biết chính xác giá trị mới của phần tử được sửa đổi vì chúng ta luôn có thể thay thế nó bằng$g$. 

Vì vậy, để phân chia với GCD bên trái$L$và GCD phải$R$, chỉ có ba khả năng. Sự phân chia đã hợp lệ nếu$L = R$. Ngược lại, chúng ta có thể thay đổi một phần tử ở bên trái để GCD của nó trở thành$R$hoặc thay đổi một phần tử ở bên phải để GCD của nó trở thành$L$. Việc kiểm tra này tập trung vào việc hỏi liệu một phạm vi có chứa tối đa một số không chia hết cho một giá trị nhất định hay không. 

Để trả lời nhanh chóng các truy vấn về tính chia hết đó, chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút lưu trữ GCD trong khoảng của nó. Nếu GCD của nút chia hết cho giá trị đích thì toàn bộ khoảng thời gian là hợp lệ và có thể bỏ qua. Ngược lại, chúng ta đi xuống cho đến khi tìm thấy một số phần tử vi phạm điều kiện. Vì chúng tôi dừng lại ngay khi tìm thấy hai phần tử xấu nên mọi truy vấn đều nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính GCD của mọi tiền tố và mọi hậu tố. Giá trị tiền tố tại vị trí$i$đưa ra GCD hiện tại của phía bên trái của phép chia$i$và giá trị hậu tố cho biết GCD hiện tại của vế phải. 
2. Xây dựng cây phân đoạn lưu trữ GCD của mỗi khoảng. Cây cho phép chúng ta nhanh chóng tìm xem liệu một phạm vi có nhiều hơn một phần tử không chia hết cho một mục tiêu nhất định hay không. 
3. Với mỗi vị trí được chia, gọi GCD bên trái là$L$và GCD bên phải là$R$. 
4. Nếu$L = R$, hãy tính phần tách này ngay lập tức vì không cần sửa đổi. 
5. Ngược lại, kiểm tra xem phần bên trái có thể đổi thành GCD không$R$. Điều này đúng khi phần bên trái có nhiều nhất một phần tử không chia hết cho$R$. 
6. Nếu không thành công, hãy kiểm tra xem phần bên phải có thể đổi thành GCD không$L$. Đây là tình trạng tương tự ở phía bên phải. 
7. Đếm phần tách nếu kiểm tra sửa đổi thành công. 

Điều bất biến đằng sau thuật toán là một phân đoạn có thể được chuyển đổi để có GCD$g$với một sửa đổi chính xác khi có nhiều nhất một phần tử trong phân đoạn đó không chia hết cho$g$. Tất cả các phần tử không thay đổi đều phải đóng góp vào GCD cuối cùng và một phần tử được sửa đổi có thể được gán bất kỳ giá trị nào để hoàn thành GCD được yêu cầu. Cây phân đoạn kiểm tra điều kiện này mà không cần truy cập từng phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build(node, left, right, arr, tree):
    if left == right:
        tree[node] = arr[left]
        return
    mid = (left + right) // 2
    build(node * 2, left, mid, arr, tree)
    build(node * 2 + 1, mid + 1, right, arr, tree)
    tree[node] = gcd(tree[node * 2], tree[node * 2 + 1])

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def count_bad(node, left, right, ql, qr, target, tree):
    if qr < left or right < ql:
        return 0
    if ql <= left and right <= qr and tree[node] % target == 0:
        return 0
    if left == right:
        return 1
    mid = (left + right) // 2
    result = count_bad(node * 2, left, mid, ql, qr, target, tree)
    if result > 1:
        return result
    result += count_bad(node * 2 + 1, mid + 1, right, ql, qr, target, tree)
    return result

def solve_case(arr):
    n = len(arr)

    pref = [0] * n
    suff = [0] * n

    for i in range(n):
        pref[i] = gcd(pref[i - 1], arr[i]) if i else arr[i]

    for i in range(n - 1, -1, -1):
        suff[i] = gcd(suff[i + 1], arr[i]) if i + 1 < n else arr[i]

    tree = [0] * (4 * n)
    build(1, 0, n - 1, arr, tree)

    ans = 0

    for i in range(n - 1):
        left_g = pref[i]
        right_g = suff[i + 1]

        if left_g == right_g:
            ans += 1
            continue

        if count_bad(1, 0, n - 1, 0, i, right_g, tree) <= 1:
            ans += 1
            continue

        if count_bad(1, 0, n - 1, i + 1, n - 1, left_g, tree) <= 1:
            ans += 1

    return ans

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        ans.append(str(solve_case(arr)))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Mảng tiền tố và hậu tố lưu trữ các giá trị GCD được sử dụng làm mục tiêu tự nhiên cho phía đối diện của mỗi phần tách. Nếu hai giá trị đã khớp nhau thì câu trả lời sẽ có ngay lập tức. 

Cây phân đoạn lưu trữ các GCD khoảng thời gian. Trong quá trình truy vấn, toàn bộ nút có thể bị loại bỏ khi GCD được lưu trữ của nó chia hết cho mục tiêu vì mọi phần tử trong khoảng đó cũng phải chia hết cho mục tiêu. Khi một nút không thể bị loại bỏ, việc tìm kiếm tiếp tục đi xuống cho đến khi tìm thấy các phần tử phá vỡ điều kiện chia hết. 

Truy vấn dừng sau khi tìm thấy hai phần tử không hợp lệ vì thuật toán chỉ cần biết số lượng bằng 0, một hay ít nhất là hai. Điều này tránh việc truyền tải không cần thiết và giữ độ phức tạp trong giới hạn. 

## Ví dụ đã hoạt động 

Mẫu 1:```
Input:
1
6
12 6 18 3 9 15
```| Chia | GCD trái | Đúng GCD | Kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 12 | 3 | Đổi trái thành 3 | Tốt | 
| 2 | 6 | 3 | Đổi trái thành 3 | Tốt | 
| 3 | 6 | 3 | Đổi trái thành 3 | Tốt | 
| 4 | 3 | 3 | Đã bình đẳng | Tốt | 
| 5 | 3 | 15 | Đổi quyền thành 3 | Tốt | 

Ví dụ chứng minh rằng nhiều phần tách hợp lệ không có GCD gốc bằng nhau. Điều kiện sửa chữa dựa trên một phần tử không thể phân chia là điều cho phép chúng. 

Mẫu 2:```
Input:
1
8
4 8 16 2 10 5 20 25
```| Chia | GCD trái | Đúng GCD | Kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | Đúng là đã có GCD 1 | Tốt | 
| 2 | 4 | 1 | Đúng là đã có GCD 1 | Tốt | 
| 3 | 4 | 5 | Quá nhiều giá trị không tương thích | Xấu | 
| 4 | 2 | 5 | Quá nhiều giá trị không tương thích | Xấu | 
| 5 | 2 | 5 | Quá nhiều giá trị không tương thích | Xấu | 
| 6 | 1 | 5 | Đổi quyền thành 1 | Tốt | 
| 7 | 1 | 25 | Đổi quyền thành 1 | Tốt | 

Dấu vết cho thấy tại sao thuật toán chỉ cần kiểm tra hai giá trị GCD hiện tại. Bất kỳ GCD cuối cùng nào khác có thể sẽ phải chia một trong các giá trị này và đã được truy vấn tính chia hết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tách thực hiện một số lượng truy vấn cây phân đoạn không đổi, mỗi truy vấn lấy O(log n). | 
| Không gian | O(n) | Mảng tiền tố, mảng hậu tố và cây phân đoạn đều sử dụng bộ nhớ tuyến tính. | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, vì vậy một$O(n \log n)$giải pháp dễ dàng phù hợp với thời hạn. Việc triển khai GCD lặp đi lặp lại của Python giữ cho các thừa số không đổi ở mức nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def solve_case(arr):
    n = len(arr)
    pref = [0] * n
    suff = [0] * n

    for i in range(n):
        pref[i] = gcd(pref[i - 1], arr[i]) if i else arr[i]

    for i in range(n - 1, -1, -1):
        suff[i] = gcd(suff[i + 1], arr[i]) if i + 1 < n else arr[i]

    tree = [0] * (4 * n)

    def build(node, l, r):
        if l == r:
            tree[node] = arr[l]
        else:
            m = (l + r) // 2
            build(node * 2, l, m)
            build(node * 2 + 1, m + 1, r)
            tree[node] = gcd(tree[node * 2], tree[node * 2 + 1])

    def bad(node, l, r, ql, qr, x):
        if qr < l or r < ql:
            return 0
        if ql <= l and r <= qr and tree[node] % x == 0:
            return 0
        if l == r:
            return 1
        m = (l + r) // 2
        res = bad(node * 2, l, m, ql, qr, x)
        if res > 1:
            return res
        return res + bad(node * 2 + 1, m + 1, r, ql, qr, x)

    build(1, 0, n - 1)

    ans = 0
    for i in range(n - 1):
        a, b = pref[i], suff[i + 1]
        if a == b or bad(1, 0, n - 1, 0, i, b) <= 1 or bad(1, 0, n - 1, i + 1, n - 1, a) <= 1:
            ans += 1
    return ans

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    t = data[0]
    idx = 1
    out = []
    for _ in range(t):
        n = data[idx]
        idx += 1
        arr = data[idx:idx+n]
        idx += n
        out.append(str(solve_case(arr)))
    return "\n".join(out)

assert run("""2
6
12 6 18 3 9 15
8
4 8 16 2 10 5 20 25
""") == """5
5"""

assert run("""1
2
6 10
""") == "1"

assert run("""1
5
7 7 7 7 7
""") == "4"

assert run("""1
2
1 1
""") == "1"

assert run("""1
5
2 3 5 7 11
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 12 6 18 3 9 15`|`5`| Cung cấp hành vi mẫu | 
|`6 10`|`1`| Một sự phân chia yêu cầu một sửa đổi | 
|`7 7 7 7 7`|`4`| Các giá trị hoàn toàn bằng nhau và đã khớp với GCD | 
|`1 1`|`1`| Đầu vào kích thước tối thiểu | 
|`2 3 5 7 11`|`4`| Nhiều giá trị GCD không tương thích và sự phân chia ranh giới | 

## Vỏ cạnh 

Đối với trường hợp các GCD hiện tại khác nhau nhưng một phần tử có thể sửa chữa sự phân chia:```
Input:
1
2
6 10
```Tiền tố GCD là 6 và hậu tố GCD là 10. Đoạn bên trái có một phần tử không chia hết cho 10, điều này được phép vì phần tử đó có thể thay thế được. Truy vấn cây phân đoạn trả về một phần tử không hợp lệ, do đó phần tách được tính. 

Đối với trường hợp toàn bộ một cạnh chứa một số phần tử không đáp ứng được yêu cầu chia hết:```
Input:
1
5
2 3 5 7 11
```Hãy xem xét phép phân chia trong đó một bên có mục tiêu GCD 3. Cây phân đoạn kiểm tra bên đó và tìm thấy nhiều số không chia hết cho 3. Vì hai hoặc nhiều phần tử cần phải thay đổi nên phép phân tách bị từ chối. Thuật toán chỉ chấp nhận một bên khi số lượng vi phạm nhiều nhất là một.
