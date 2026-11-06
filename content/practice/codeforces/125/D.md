---
title: "CF 125D - Hai cấp tiến"
description: "Chúng ta được cho một dãy các số nguyên riêng biệt theo một thứ tự cố định. Mỗi phần tử phải được gán cho đúng một trong hai dãy con. Bên trong mỗi dãy con, thứ tự ban đầu phải được giữ nguyên. Mục đích là làm cho cả hai dãy con đều là cấp số cộng."
date: "2026-06-02T16:23:26+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 125
codeforces_index: "D"
codeforces_contest_name: "Codeforces Testing Round 2"
rating: 2200
weight: 125
solve_time_s: 144
verified: false
draft: false
---

[CF 125D - Hai tiến trình](https://codeforces.com/problemset/problem/125/D) 

**Đánh giá:** 2200 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên riêng biệt theo một thứ tự cố định. Mỗi phần tử phải được gán cho đúng một trong hai dãy con. Bên trong mỗi dãy con, thứ tự ban đầu phải được giữ nguyên. 

Mục đích là làm cho cả hai dãy con đều là cấp số cộng. Tiến trình có độ dài 1 hoặc 2 luôn hợp lệ. Không được phép cấp tiến có độ dài 0, vì vậy cả hai chuỗi con kết quả phải chứa ít nhất một phần tử. 

Độ dài chuỗi lên tới 30000. Bất kỳ cách tiếp cận nào thử nhiều phép gán phần tử khác nhau sẽ ngay lập tức trở thành không thể. Thậm chí kiểm tra tất cả$2^n$các phân vùng lớn một cách vô lý. Giải pháp phải khai thác các đặc tính cấu trúc mạnh mẽ của cấp số cộng và chạy trong thời gian gần như tuyến tính. 

Phần tinh tế nhất của vấn đề là các cấp số cộng được xác định theo thứ tự chứ không phải bằng cách sắp xếp. Ví dụ:```
1 5 3
```Trình tự tiếp theo`1 3`là số học, nhưng`1 5 3`thì không. Bất kỳ giải pháp nào sắp xếp mảng trước tiên đều giải quyết được một vấn đề khác. 

Một sai lầm dễ mắc phải khác là cho rằng khi chúng ta đoán một cấp số nhân thì mọi giá trị trùng khớp đều phải thuộc về cấp số đó. Coi như:```
1 2 3 -2 -7
```Cả hai điều sau đây đều hợp lệ:```
1 2 3
-2 -7
```Và```
1 2
3 -2 -7
```Thuật toán phải cho phép sự mơ hồ này. 

Cạm bẫy thứ ba xuất hiện khi một tiến trình gần như đúng và chỉ phần tử được chọn cuối cùng của nó bị đặt sai vị trí. Ví dụ:```
4 1 2 7 3 10
```Nếu chúng ta xây dựng sự tiến triển`1 2 3`, mọi thứ đều hoạt động. Đối với các dự đoán khác, các phần tử còn lại có thể không phải là số học cho đến khi chúng ta di chuyển phần tử được chọn cuối cùng từ cấp số này sang cấp số khác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi phân vùng của chuỗi thành hai chuỗi con và kiểm tra xem cả hai có phải là số học hay không. Điều này đúng vì nó trực tiếp kiểm tra mọi câu trả lời có thể. Thật không may là có$2^n$phân vùng. Ngay cả đối với$n=50$điều này đã vô vọng rồi, và đây$n$đạt 30000. 

Quan sát quan trọng đến từ ba yếu tố đầu tiên. 

Khi ba phần tử được phân bố thành hai dãy con, theo nguyên lý chuồng bồ câu, ít nhất hai trong số chúng thuộc cùng một cấp số nhân. Hai yếu tố đó quyết định sự khác biệt chung của sự tiến triển đó. 

Trong số ba vị trí đầu tiên chỉ có ba cặp có thể xảy ra:```
(1,2), (1,3), (2,3)
```Vì vậy, chỉ có ba điểm khác biệt của ứng cử viên đáng được xem xét. 

Giả sử chúng ta chọn một trong các cặp này và cho rằng nó thuộc về cấp số A. Sự khác biệt của nó là cố định. Khi chênh lệch được cố định, toàn bộ tiến trình A được xác định: bắt đầu từ giá trị được chọn đầu tiên, giá trị tiếp theo phải chính xác`start + d`, sau đó`start + 2d`, vân vân. 

Chúng tôi quét trình tự từ trái sang phải. Bất cứ khi nào chúng tôi gặp giá trị mong đợi tiếp theo của A, chúng tôi sẽ đặt nó vào A. Mọi thứ khác tạm thời được đặt vào B. 

Sau cấu trúc này, B đã là một cấp số cộng hoặc không. 

Thủ thuật còn lại là phần duy nhất không rõ ràng. Nếu B không phải là số học, chúng ta thử di chuyển phần tử cuối cùng được chọn cho A vào B và kiểm tra lại. Nếu vẫn không thành công thì sự khác biệt về ứng cử viên này không thể tạo ra câu trả lời hợp lệ. 

Tại sao chỉ di chuyển phần tử cuối cùng là đủ? Giả sử tồn tại một số giải pháp cho sự khác biệt ứng cử viên này. Cấu trúc tham lam luôn lấy mọi giá trị kỳ vọng sẵn có của A. Giá trị duy nhất có thể được "trả lại" một cách an toàn cho B là giá trị cuối cùng được lấy. Nếu việc di chuyển phần tử cuối cùng đó không sửa được B, thì việc di chuyển nhiều phần tử hơn cũng không thể giúp ích được gì. Giải pháp Codeforces ban đầu dựa vào thuộc tính này và chỉ kiểm tra hai cấu hình này. 

Vì chỉ có ba điểm khác nhau của ứng viên và mỗi lần kiểm tra là tuyến tính nên toàn bộ thuật toán là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`n = 2`, xuất ra hai phần tử dưới dạng hai cấp số phần tử đơn. 
2. Xét ba cặp trong ba vị trí đầu tiên:`(1,2)`,`(1,3)`,`(2,3)`. 
3. Đối với một cặp được chọn`(l,r)`, cho phép:`d = a[r] - a[l]`. 
4. Xây dựng tiến trình A một cách tham lam. 

Bắt đầu với giá trị mong đợi`a[l]`. 

Quét trình tự từ trái sang phải. 

Bất cứ khi nào phần tử hiện tại bằng giá trị mong đợi, hãy đặt nó vào A và tăng giá trị mong đợi lên`d`. 
5. Mọi phần tử không được A lấy sẽ được đặt vào B. 
6. Kiểm tra xem B có phải là cấp số cộng hay không. 

Độ dài 1 và độ dài 2 luôn được chấp nhận. 
7. Nếu B hợp lệ, xuất A và B. 
8. Nếu không, hãy loại bỏ phần tử cuối cùng được chèn vào A, di chuyển nó vào B và kiểm tra lại B. 
9. Nếu B hợp lệ, xuất ra A và B đã sửa đổi. 
10. Nếu không có lần kiểm tra nào thành công, hãy thử cặp ứng cử viên tiếp theo. 
11. Nếu cả ba cặp ứng cử viên đều thất bại, hãy in`"No solution"`. 

### Tại sao nó hoạt động 

Trong số ba phần tử đầu tiên, một số cặp phải thuộc cùng một tiến trình trong bất kỳ phân vùng hợp lệ nào. Cặp đó xác định sự khác biệt của ứng cử viên, vì vậy một trong ba lần thử sử dụng sự khác biệt chính xác. 

Đối với một sai phân cố định, cấu trúc tham lam phục hồi cấp số cộng tối đa phù hợp với sai phân đó. Mỗi khi giá trị mong đợi xuất hiện, việc bỏ qua nó sẽ vĩnh viễn ngăn số hạng đó xuất hiện trong lũy ​​tiến sau này vì tất cả các số đều khác nhau. 

Sự lựa chọn quá tham lam duy nhất có thể là phần tử được chọn cuối cùng. Việc di chuyển phần tử cuối cùng này sang tiến trình khác sẽ bao gồm cấu hình hợp lệ còn lại. Nếu cả phân vùng ban đầu và phân vùng thu được bằng cách di chuyển phần tử được chọn cuối cùng đều không mang lại cấp số cộng B, thì không tồn tại phân vùng nào sử dụng chênh lệch đó. Do đó, việc thử ba điểm khác biệt ứng cử viên là đủ để tìm ra giải pháp bất cứ khi nào có một giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_ap(v):
    m = len(v)
    if m == 0:
        return False
    if m <= 2:
        return True

    d = v[1] - v[0]
    for i in range(2, m):
        if v[i] - v[i - 1] != d:
            return False
    return True

def try_pair(a, l, r):
    n = len(a)

    d = a[r] - a[l]
    expected = a[l]

    used = [False] * n
    A = []
    last_idx = -1

    for i in range(n):
        if a[i] == expected:
            used[i] = True
            A.append(a[i])
            expected += d
            last_idx = i

    B = [a[i] for i in range(n) if not used[i]]

    if is_ap(B):
        return A, B

    A.pop()
    used[last_idx] = False

    B = [a[i] for i in range(n) if not used[i]]

    if is_ap(B):
        return A, B

    return None

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 2:
        print(a[0])
        print(a[1])
        return

    pairs = [(0, 1), (0, 2), (1, 2)]

    for l, r in pairs:
        res = try_pair(a, l, r)
        if res is not None:
            A, B = res

            if len(A) == 0 or len(B) == 0:
                continue

            print(*A)
            print(*B)
            return

    print("No solution")

if __name__ == "__main__":
    solve()
```chức năng`is_ap`thực hiện định nghĩa chính xác của một cấp số cộng. Độ dài 1 và 2 tự động hợp lệ, trong khi độ dài 0 bị từ chối vì bài toán yêu cầu cả hai chuỗi con đều không trống. 

chức năng`try_pair`thực hiện một cuộc kiểm tra ứng viên. Sự khác biệt được xác định bởi cặp được chọn trong số ba phần tử đầu tiên. Trong quá trình quét, biến`expected`lưu trữ giá trị tiếp theo phải xuất hiện trong tiến trình A. 

Mảng`used`ghi lại các vị trí đã được giao cho A. Tái tạo B từ`used`đơn giản hơn và ít xảy ra lỗi hơn so với việc cố gắng duy trì cả hai chuỗi con trong quá trình quét. 

Một chi tiết tinh tế đang được lưu trữ`last_idx`. Nếu phiên bản đầu tiên bị lỗi, chỉ phần tử được chọn cuối cùng bị xóa khỏi A. Tính toán lại B từ phiên bản đã cập nhật`used`mảng đảm bảo rằng thứ tự tương đối vẫn đúng. 

Tất cả số học đều sử dụng số nguyên Python, vì vậy việc tràn không phải là vấn đề đáng lo ngại mặc dù các giá trị có thể đạt tới$10^8$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
4 1 2 7 3 10
```Đang thử cặp`(1,2)`trong lập chỉ mục dựa trên 1, các giá trị`(4,1)`. 

| Vị trí | Giá trị | Dự kiến ​​ở A | Hành động | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | A | 
| 2 | 1 | 1 | A | 
| 3 | 2 | -2 | B | 
| 4 | 7 | -2 | B | 
| 5 | 3 | -2 | B | 
| 6 | 10 | -2 | B |`B = [2, 7, 3, 10]`, không phải số học. 

Đang thử cặp`(2,3)`, giá trị`(1,2)`. 

| Vị trí | Giá trị | Dự kiến ​​ở A | Hành động | 
| --- | --- | --- | --- | 
| 1 | 4 | 1 | B | 
| 2 | 1 | 1 | A | 
| 3 | 2 | 2 | A | 
| 4 | 7 | 3 | B | 
| 5 | 3 | 3 | A | 
| 6 | 10 | 4 | B |`A = [1,2,3]`

`B = [4,7,10]`Cả hai đều là số học, vì vậy chúng tôi xuất chúng. 

Ví dụ này cho thấy cách tìm lại sự khác biệt chính xác từ một trong ba cặp ứng cử viên. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 -2 -7
```Đang thử cặp`(1,2)`. 

| Vị trí | Giá trị | Dự kiến ​​ở A | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | A | 
| 2 | 2 | 2 | A | 
| 3 | 3 | 3 | A | 
| 4 | -2 | 4 | B | 
| 5 | -7 | 4 | B | 

Chúng tôi có được:```
A = [1,2,3]
B = [-2,-7]
```Cả hai dãy con đều là số học. 

Ví dụ này minh họa rằng tiến trình có độ dài-2 luôn hợp lệ, điều này rất cần thiết cho nhiều phân vùng thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Ba lần quét tuyến tính, mỗi lần kiểm tra sự khác biệt của ứng viên | 
| Không gian |$O(n)$| Lưu trữ các chuỗi con và mảng đánh dấu | 

Độ dài chuỗi chỉ là 30000, do đó, một vài đường truyền tuyến tính có thể dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng khiêm tốn vì chúng tôi chỉ lưu trữ một số mảng có kích thước$n$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    def solve():
        input = sys.stdin.readline

        def is_ap(v):
            if len(v) == 0:
                return False
            if len(v) <= 2:
                return True
            d = v[1] - v[0]
            for i in range(2, len(v)):
                if v[i] - v[i - 1] != d:
                    return False
            return True

        def try_pair(a, l, r):
            d = a[r] - a[l]
            expected = a[l]

            used = [False] * len(a)
            A = []
            last = -1

            for i, x in enumerate(a):
                if x == expected:
                    used[i] = True
                    A.append(x)
                    expected += d
                    last = i

            B = [a[i] for i in range(len(a)) if not used[i]]
            if is_ap(B):
                return A, B

            A.pop()
            used[last] = False
            B = [a[i] for i in range(len(a)) if not used[i]]

            if is_ap(B):
                return A, B

            return None

        n = int(input())
        a = list(map(int, input().split()))

        if n == 2:
            print(a[0])
            print(a[1])
            return

        for l, r in [(0, 1), (0, 2), (1, 2)]:
            res = try_pair(a, l, r)
            if res:
                A, B = res
                if A and B:
                    print(*A)
                    print(*B)
                    return

        print("No solution")

    solve()

    sys.stdout = old_stdout
    return out.getvalue()

# sample 1
assert run("6\n4 1 2 7 3 10\n") != "No solution\n"

# sample 2
assert run("5\n1 2 3 -2 -7\n") != "No solution\n"

# minimum size
assert run("2\n5 9\n") == "5\n9\n"

# already two interleaved progressions
assert run("6\n1 10 3 20 5 30\n") != "No solution\n"

# impossible case
assert run("5\n1 2 4 8 16\n") == "No solution\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 5 9`| Hai tiến trình đơn lẻ | Kích thước tối thiểu | 
|`1 10 3 20 5 30`| Bất kỳ phân vùng hợp lệ nào | Tiến trình xen kẽ | 
|`1 2 4 8 16`|`No solution`| Trường hợp tiêu cực | 
| Đầu vào mẫu | Phân vùng hợp lệ | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Hãy xem xét:```
2
5 9
```Vấn đề vẫn yêu cầu hai tiến trình không trống. Thuật toán ngay lập tức đưa ra mỗi phần tử dưới dạng một tiến trình đơn lẻ. Độ dài 1 là số học, vì vậy điều này hợp lệ. 

Coi như:```
5
1 2 3 -2 -7
```phần tử`3`có thể thuộc về một trong hai sự tiến triển. Sự xây dựng tham lam đặt nó vào sự tiến triển với sự khác biệt`1`, sản xuất:```
1 2 3
-2 -7
```Lời giải vẫn hợp lệ vì cấp số cộng được xác định bởi giá trị kỳ vọng tiếp theo của chúng. 

Coi như:```
6
4 1 2 7 3 10
```Một số khác biệt về ứng cử viên lúc đầu không thành công. Thuật toán kiểm tra cả phân vùng tham lam trực tiếp và phiên bản mà phần tử được chọn cuối cùng được chuyển sang tiến trình khác. Điều này ngăn việc từ chối các cấu hình hợp lệ do một lựa chọn quá tham lam gây ra. 

Cuối cùng, hãy xem xét một đầu vào không thể, chẳng hạn như:```
5
1 2 4 8 16
```Tất cả ba sự khác biệt của ứng cử viên đều được kiểm tra. Không ai trong số chúng tạo ra hai chuỗi số học, ngay cả sau khi di chuyển phần tử được chọn cuối cùng. Thuật toán in chính xác:```
No solution
```
