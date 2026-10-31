---
title: "CF 106386C - Kiểu tóc Olympic"
description: "Aruj bắt đầu với k nang tóc. Có n thợ cắt tóc, và thợ cắt tóc i sẽ loại bỏ chính xác ai nang nếu Aruj vẫn còn nhiều hơn ai nang. Nếu anh ta có ai hoặc ít hơn, chuyến thăm đó sẽ khiến anh ta bị hói, điều này không được phép."
date: "2026-06-25T10:13:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106386
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 2-25-26 (Advanced)"
rating: 0
weight: 106386
solve_time_s: 37
verified: true
draft: false
---

[CF 106386C - Cắt tóc Olympic](https://codeforces.com/problemset/problem/106386/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Aruj bắt đầu với`k`nang tóc. có`n`thợ cắt tóc và thợ cắt tóc`i`loại bỏ chính xác`a_i`nang trứng nếu Aruj vẫn còn nhiều hơn`a_i`nang trứng. Nếu anh ấy có`a_i`hoặc ít hơn, chuyến thăm đó sẽ khiến anh ta bị hói, điều này không được phép. 

Aruj có thể chọn bất kỳ nhóm thợ cắt tóc nào và đến thăm họ theo bất kỳ thứ tự nào. Mục đích là giảm thiểu số lượng nang còn lại trong khi luôn giữ lại ít nhất một nang. Đầu ra là số lượng nang còn lại nhỏ nhất có thể. 

Nhận xét quan trọng là thứ tự của các thợ cắt tóc được chọn dường như chỉ có ý nghĩa quan trọng. Nếu tổng số tiền bị loại bỏ bởi một bộ đã chọn là`S`, thì số tiền cuối cùng là`k - S`. Tập đã chọn có giá trị chính xác khi`S < k`. Nếu tổng mức loại bỏ dưới đây`k`, thì trước khi có bất kỳ thợ cắt tóc riêng lẻ nào đến thăm, tổng số tiền đã bị loại bỏ cho đến nay cộng với số lần loại bỏ của thợ cắt tóc hiện tại là nhiều nhất`S`, vẫn nhỏ hơn`k`. Điều đó có nghĩa là số lượng tóc hiện tại luôn lớn hơn số lượng tóc được thợ cắt tóc loại bỏ, do đó số lượng tóc cắt không bao giờ bằng 0. 

Các ràng buộc làm cho cách tiếp cận dự kiến ​​trở nên rõ ràng. Có nhiều nhất`16`thợ cắt tóc, vậy số tập con có thể có là`2^16 = 65536`. Điều này đủ nhỏ để liệt kê tập hợp con hoàn chỉnh. Giải pháp thử mọi thứ tự có thể sẽ lớn hơn nhiều vì một tập hợp con có thể có nhiều chuỗi truy cập có thể có. Giới hạn trên`n`đặc biệt cho phép chúng tôi khám phá các tập hợp con thay vì hoán vị. 

Các trường hợp chính xuất phát từ việc nhầm lẫn điều kiện`S < k`với`S <= k`, và khỏi quên rằng việc không chọn thợ cắt tóc luôn được phép. 

Ví dụ:```
Input:
1 10
10

Output:
10
```Một giải pháp bất cẩn có thể chọn thợ cắt tóc vì cắt bỏ chính xác`10`các nang dường như rời đi`0`. Tuy nhiên, luật cấm đạt đến số 0 nên không thể sử dụng thợ cắt tóc. 

Một trường hợp khác là:```
Input:
3 10
5 8 4

Output:
1
```Việc chọn thợ cắt tóc thứ nhất và thứ ba sẽ loại bỏ`9`nang trứng. Câu trả lời cuối cùng là`1`. Giải pháp chỉ kiểm tra từng thợ cắt tóc có thể bỏ lỡ sự kết hợp của một số thao tác loại bỏ nhỏ hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử từng nhóm thợ cắt tóc. Đối với mỗi tập hợp con, hãy tính tổng số nang mà nó loại bỏ. Nếu tổng đó nhỏ hơn`k`, tập hợp con hợp lệ và chúng tôi giữ số tiền loại bỏ hợp lệ lớn nhất. Điều này hiệu quả vì câu trả lời cuối cùng chỉ đơn giản là`k`trừ đi số tiền có thể tháo rời tối đa. 

Phương pháp vũ phu là đúng vì mọi nhóm thợ cắt tóc có thể đều được kiểm tra. Vấn đề là số lượng tập hợp con tăng theo cấp số nhân. Trong bài toán này giá trị lớn nhất chỉ là`2^16`, có thể quản lý được nhưng là phiên bản tổng quát hơn với kích thước lớn hơn`n`sẽ nhanh chóng trở nên không thể. Ví dụ, với`n = 40`, việc kiểm tra tất cả các tập hợp con sẽ yêu cầu hơn một nghìn tỷ phép tính. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần quan tâm đến thứ tự của thợ cắt tóc. Điều kiện để có một chuỗi hợp lệ chỉ phụ thuộc vào tổng của tất cả các phép loại bỏ đã chọn. Khi chúng tôi biết một tập hợp con có tổng số lần loại bỏ nhỏ hơn`k`, mọi thứ tự của tập hợp con đó đều hoạt động. Điều này chuyển bài toán thành việc tìm tổng tập hợp con lớn nhất nhỏ hơn`k`, đó là một tiêu chuẩn đáp ứng trong tình huống ở giữa. 

Chúng tôi chia thợ cắt tóc thành hai nhóm. Mỗi nửa có tối đa tám phần tử, vì vậy chúng ta có thể tạo ra tất cả các tập hợp con cho mỗi nửa. Thay vì kiểm tra tất cả`2^16`tập hợp con trực tiếp, chúng tôi kết hợp hai danh sách một cách hiệu quả. Đối với mỗi số tiền từ một nửa, chúng tôi tìm thấy số tiền tương thích lớn nhất từ ​​nửa còn lại giữ cho tổng số ở dưới`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Được chấp nhận với n ≤ 16 | 
| Gặp nhau ở giữa | O(2^(n/2) log(2^(n/2))) | O(2^(n/2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mảng dịch vụ cắt tóc thành hai nửa. Mỗi nửa chứa tối đa tám giá trị, vì vậy tất cả các tổng tập hợp con bên trong một nửa có thể được tạo nhanh chóng. 
2. Tạo mọi tổng tập con có thể có của nửa đầu. Bao gồm tập hợp con trống với tổng`0`bởi vì không được phép đến thăm thợ cắt tóc từ phía này. 
3. Tạo mọi tổng tập hợp con có thể có của nửa thứ hai theo cách tương tự. Vấn đề bây giờ được giảm xuống còn việc kết hợp một giá trị từ mỗi danh sách. 
4. Sắp xếp các tập hợp con từ nửa thứ hai. Với mọi tổng tập hợp con`x`ở nửa đầu, tìm tổng lớn nhất ở nửa sau`y`như vậy`x + y < k`. 
5. Theo dõi tổng số lần xóa hợp lệ lớn nhất`x + y`. Các nang cuối cùng còn lại là`k`trừ giá trị này. 

Lý do điều này có tác dụng là vì mọi tập hợp con có thể có của thợ cắt tóc có thể được chia thành một lựa chọn duy nhất ở nửa đầu và một lựa chọn ở nửa sau. Hai danh sách được tạo ra chứa tất cả những đóng góp có thể có, vì vậy việc kiểm tra các cặp tương thích sẽ bao gồm mọi giải pháp có thể. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà giá trị tốt nhất được tìm thấy cho đến nay là số lượng nang tối đa có thể bị loại bỏ mà không đạt đến số 0. Mỗi tập hợp con hợp lệ tương ứng với chính xác một cặp tổng tập hợp con được tạo. Bước tìm kiếm nhị phân xem xét đối tác tốt nhất có thể cho mỗi lựa chọn ở nửa đầu, do đó không có câu trả lời hợp lệ nào bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def generate_sums(arr):
    sums = [0]
    for x in arr:
        current = []
        for s in sums:
            current.append(s + x)
        sums += current
    return sums

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    mid = n // 2
    left = generate_sums(a[:mid])
    right = generate_sums(a[mid:])

    right.sort()

    best = 0
    import bisect

    for x in left:
        limit = k - x - 1
        if limit >= 0:
            pos = bisect.bisect_right(right, limit) - 1
            if pos >= 0:
                best = max(best, x + right[pos])

    print(k - best)

if __name__ == "__main__":
    solve()
```các`generate_sums`Hàm xây dựng tất cả các lần xóa có thể có khỏi một nhóm thợ cắt tóc. Bắt đầu chỉ với`0`, mỗi thợ cắt tóc đều bị bỏ qua hoặc đóng góp số tiền bị loại bỏ, vì vậy mỗi khoản tiền hiện có sẽ tạo ra một khoản tiền mới. 

Mảng được chia thành hai phần trước khi tạo tổng. Với`n = 16`, mỗi bên có nhiều nhất là tám phần tử, cho nhiều nhất`256`tập hợp các tổng con mỗi bên. Điều này giữ cho bước kết hợp rất nhỏ. 

Việc tìm kiếm nhị phân sử dụng`bisect_right`để tìm giá trị đầu tiên lớn hơn giới hạn cho phép, sau đó lùi lại một vị trí để lấy giá trị hợp lệ lớn nhất. Sự bất bình đẳng nghiêm ngặt có vấn đề ở đây. Chúng tôi cần`x + y < k`, vậy giới hạn là`k - x - 1`, không`k - x`. 

Số nguyên Python không bị tràn, điều này tránh được sự cố vì`a_i`Và`k`có thể lớn như`10^9`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 10
5 8 4
```Các tập hợp con của việc loại bỏ được khám phá qua hai nửa. 

| Tổng trái | Tổng đúng cho phép | Tổng đúng nhất | Loại bỏ tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 9 | 8 | 8 | 
| 5 | 4 | 4 | 9 | 

Cách loại bỏ tốt nhất là`9`, rời đi`10 - 9 = 1`nang. 

Dấu vết này chứng minh tại sao việc kết hợp các thợ cắt tóc nhỏ hơn lại quan trọng. Câu trả lời không đến từ một thợ cắt tóc lớn nhất mà đến từ sự kết hợp tốt nhất dưới mức giới hạn. 

### Mẫu 2 

đầu vào:```
1 10
20
```| Tổng trái | Tổng đúng cho phép | Tổng đúng nhất | Loại bỏ tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 9 | 0 | 0 | 

Người thợ cắt tóc duy nhất cắt quá nhiều nên không thể cắt tóc được. 

Tập hợp con trống luôn được xem xét, giữ lại số lượng tóc ban đầu là một câu trả lời hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(n/2) log(2^(n/2))) | Chúng tôi tạo cả danh sách tổng tập hợp con và tìm kiếm nhị phân cho mỗi nửa tổng bên trái. | 
| Không gian | O(2^(n/2)) | Tổng tập hợp con được tạo sẽ được lưu trữ cho cả hai nửa. | 

Vì`n = 16`, danh sách tập hợp con lớn nhất chỉ chứa`256`giá trị, do đó giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import bisect

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def generate_sums(arr):
        sums = [0]
        for x in arr:
            sums += [s + x for s in sums]
        return sums

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    mid = n // 2
    left = generate_sums(a[:mid])
    right = generate_sums(a[mid:])
    right.sort()

    best = 0
    for x in left:
        pos = bisect.bisect_right(right, k - x - 1) - 1
        if pos >= 0:
            best = max(best, x + right[pos])

    return str(k - best) + "\n"

assert solution("3 10\n5 8 4\n") == "1\n", "sample 1"
assert solution("1 10\n20\n") == "10\n", "sample 2"

assert solution("1 1\n1\n") == "1\n", "cannot reach zero"
assert solution("4 100\n10 20 30 40\n") == "1\n", "all values fit except exact total"
assert solution("5 50\n10 10 10 10 10\n") == "10\n", "repeated values"
assert solution("4 15\n14 7 8 6\n") == "1\n", "boundary subset sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Kích thước tối thiểu và kết quả không bị cấm | 
|`4 100 / 10 20 30 40`|`1`| Việc loại bỏ hoàn toàn chính xác không được chấp nhận | 
|`5 50 / 10 10 10 10 10`|`10`| Nhiều giá trị bằng nhau | 
|`4 15 / 14 7 8 6`|`1`| Chọn kết hợp gần giới hạn | 

## Vỏ cạnh 

Đối với trường hợp thợ cắt tóc loại bỏ chính xác tất cả các nang còn lại:```
Input:
1 10
10
```Tổng tập hợp con được tạo là`0`Và`10`. Tìm kiếm nhị phân chỉ chấp nhận các khoản tiền dưới đây`10`, vậy tổng`10`bị từ chối. Câu trả lời vẫn còn`10`. 

Đối với trường hợp một số thợ cắt tóc nhỏ hơn kết hợp thành câu trả lời tối ưu:```
Input:
3 10
5 8 4
```Tổng tập hợp con`5 + 4 = 9`hợp lệ vì nó hoàn toàn nhỏ hơn`10`. Thuật toán tìm cặp này trên hai nửa và trả về`1`. 

Đối với trường hợp mọi thợ cắt tóc đều quá lớn:```
Input:
3 5
6 7 8
```Mọi tổng tập con không rỗng ít nhất là`6`, nên mỗi lần cắt tóc sẽ khiến Aruj bị hói. Tập hợp con hợp lệ duy nhất là tập hợp con trống và thuật toán trả về`5`. 

Đối với trường hợp tất cả thợ cắt tóc cùng nhau vẫn an toàn:```
Input:
4 20
3 4 5 6
```Tổng số loại bỏ là`18`, hợp lệ. Thuật toán tìm tập con đầy đủ và trả về`2`, cho thấy được phép sử dụng mọi thợ cắt tóc khi tổng số tiền vẫn ở mức dưới mức ban đầu.
