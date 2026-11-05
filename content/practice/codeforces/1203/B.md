---
title: "CF 1203B - Hình chữ nhật bằng nhau"
description: "Chúng tôi được tặng 4n gậy. Mỗi hình chữ nhật cần bốn que: hai que cho chiều dài một cạnh và hai que cho chiều dài cạnh kia. Mỗi cây gậy phải được sử dụng đúng một lần. Mục tiêu là chia tất cả các que thành đúng n hình chữ nhật sao cho mọi hình chữ nhật đều có diện tích bằng nhau."
date: "2026-06-11T23:42:19+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1203
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 579 (Div. 3)"
rating: 1200
weight: 1203
solve_time_s: 115
verified: true
draft: false
---

[CF 1203B - Hình chữ nhật bằng nhau](https://codeforces.com/problemset/problem/1203/B) 

**Đánh giá:** 1200 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`4n`gậy. Mỗi hình chữ nhật cần bốn que: hai que cho chiều dài một cạnh và hai que cho chiều dài cạnh kia. Mỗi cây gậy phải được sử dụng đúng một lần. 

Mục tiêu là chia tất cả các que thành chính xác`n`hình chữ nhật sao cho mọi hình chữ nhật đều có diện tích bằng nhau. Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định xem liệu cấu trúc đó có tồn tại hay không. 

Một hình chữ nhật có độ dài cạnh`a`Và`b`cần hai que dài`a`và hai que dài`b`. Điều đó ngay lập tức cho chúng ta biết rằng các cây gậy phải tạo thành các cặp có độ dài bằng nhau một cách tự nhiên. Nếu chiều dài thanh nào đó xuất hiện với số lần lẻ bên trong cấu trúc cặp bắt buộc thì việc tạo thành hình chữ nhật là không thể. 

Những hạn chế là nhỏ. Mỗi truy vấn chứa tối đa`4 · 100 = 400`gậy và có tối đa 500 truy vấn. Việc sắp xếp 400 số cực kỳ rẻ, vì vậy`O(n log n)`giải pháp cho mỗi truy vấn dễ dàng đủ nhanh. 

Phần khó không phải là dựng hình chữ nhật mà là chứng minh rằng tất cả các hình chữ nhật đều có thể có cùng diện tích. 

Hãy xem xét các cây gậy:```
1 1 2 2 3 3 6 6
```Một cách tiếp cận bất cẩn chỉ có thể xác minh rằng mỗi độ dài xuất hiện một số lần chẵn. Điều kiện đó được thỏa mãn, tuy nhiên hình chữ nhật duy nhất có thể có là`(1,6)`Và`(2,3)`, đều có diện tích`6`, vậy câu trả lời là`YES`. 

Bây giờ hãy xem xét:```
1 1 2 2 3 3 4 4
```Một lần nữa, mỗi độ dài xuất hiện một số lần chẵn. Các hình chữ nhật trở thành`(1,4)`Và`(2,3)`, diện tích của chúng đều là`4`Và`6`tương ứng. Câu trả lời là`NO`. Chỉ số lượng cặp là không đủ. 

Một sai lầm dễ mắc phải khác là tham lam ghép các cặp bằng nhau mà không kiểm tra khu vực toàn cầu. Ví dụ:```
1 1 1 1 2 2 2 2
```Chúng ta có thể xây dựng các hình chữ nhật`(1,1)`Và`(2,2)`, cho diện tích`1`Và`4`. Câu trả lời đúng là`NO`, mặc dù mỗi chiều dài thanh xuất hiện đúng bốn lần. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách để nhóm các que thành hình chữ nhật và sau đó kiểm tra xem mọi hình chữ nhật có cùng diện tích hay không. Điều này đúng vì nó xem xét mọi sự sắp xếp có thể. Thật không may, số lượng các nhóm có thể tăng lên một cách bùng nổ. Ngay cả đối với vài chục cây gậy, số lượng kết hợp trở nên vô cùng lớn, khiến phương pháp như vậy hoàn toàn không khả thi. 

Quan sát quan trọng đến từ việc phân loại các que. 

Giả sử tồn tại một giải pháp hợp lệ. Mỗi cạnh của mỗi hình chữ nhật phải được tạo thành bởi hai que bằng nhau. Sau khi sắp xếp, các que bằng nhau phải xếp cạnh nhau. Nếu ở vị trí nào đó`a[2i] != a[2i+1]`, một số mặt cần thiết không thể được hình thành và câu trả lời là ngay lập tức`NO`. 

Sau khi xác minh tất cả các cặp, hãy nghĩ về diện tích hình chữ nhật. Cho mảng đã sắp xếp là:```
a0 ≤ a1 ≤ ... ≤ a(4n-1)
```Trong cách sắp xếp hợp lệ, độ dài cạnh nhỏ nhất hiện có phải được ghép với độ dài cạnh lớn nhất hiện có. Nếu không, một số hình chữ nhật sẽ có diện tích nhỏ hơn và hình chữ nhật khác sẽ có diện tích lớn hơn. 

Điều này dẫn đến một cấu trúc rất mạnh mẽ. Sau khi sắp xếp, chiều dài các cạnh hình chữ nhật được hình thành từ các khối cặp:```
(a0,a1), (a2,a3), ... , (a(4n-2),a(4n-1))
```Cặp cạnh đầu tiên và cặp cạnh cuối cùng phải tạo thành một hình chữ nhật. Cặp cạnh thứ hai và cặp cạnh cuối cùng thứ hai phải tạo thành một hình chữ nhật khác, v.v. 

Nếu tất cả các hình chữ nhật có cùng diện tích thì```
a0 * a(4n-1)
=
a2 * a(4n-3)
=
a4 * a(4n-5)
= ...
```Kiểm tra tình trạng này sau khi sắp xếp sẽ giải quyết được hoàn toàn vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`4n`thanh độ dài và sắp xếp chúng. 
2. Xác minh rằng mọi cặp liên tiếp đều bằng nhau. 

Kiểm tra vị trí`(0,1)`,`(2,3)`,`(4,5)`, vân vân. Nếu bất kỳ cặp nào chứa các giá trị khác nhau thì không thể tạo thành một cạnh hình chữ nhật, do đó xuất ra`"NO"`. 
3. Tính diện tích mục tiêu bằng que nhỏ nhất và lớn nhất. 

Vì hình chữ nhật hợp lệ phải ghép cạnh nhỏ nhất với cạnh lớn nhất nên diện tích cần có là:```
area = a[0] * a[4n-1]
```4. Di chuyển vào trong từ cả hai đầu của mảng đã sắp xếp. 

Đối với mỗi ứng cử viên hình chữ nhật, hãy so sánh:```
a[l] * a[r]
```Ở đâu`l`bắt đầu lúc`0`và tăng lên bởi`2`, trong khi`r`bắt đầu lúc`4n-1`và giảm dần`2`. 
5. Nếu bất kỳ sản phẩm nào khác với khu vực mục tiêu, hãy xuất`"NO"`. 
6. Nếu tất cả kiểm tra cặp và tất cả kiểm tra khu vực thành công, xuất ra`"YES"`. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mỗi cạnh hình chữ nhật phải được tạo thành từ hai que liền kề bằng nhau. Nếu không, hai que bằng nhau sẽ phải được tách ra trong khi vẫn còn một số chiều dài không khớp giữa chúng, điều này là không thể trong một cặp hợp lệ. 

Khi các cặp cạnh đó đã được cố định, cách duy nhất để tất cả các diện tích hình chữ nhật vẫn bằng nhau là kết hợp cạnh nhỏ nhất có sẵn với cạnh lớn nhất có sẵn, cạnh nhỏ thứ hai với cạnh lớn thứ hai, v.v. Bất kỳ sự sắp xếp nào khác sẽ tạo ra một sản phẩm nhỏ hơn ở đâu đó và một sản phẩm lớn hơn ở nơi khác. 

Thuật toán kiểm tra chính xác các điều kiện cần thiết này. Họ cũng có đủ. Nếu mọi cặp liền kề đều bằng nhau và mọi tích của cặp cực trị đều có cùng giá trị, thì chúng ta có thể xây dựng rõ ràng các hình chữ nhật từ các cặp đó và mọi hình chữ nhật đều có diện tích chung cần thiết. Do đó thuật toán là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())

    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        ok = True

        for i in range(0, 4 * n, 2):
            if a[i] != a[i + 1]:
                ok = False
                break

        if not ok:
            print("NO")
            continue

        target = a[0] * a[-1]

        l = 0
        r = 4 * n - 1

        while l < r:
            if a[l] * a[r] != target:
                ok = False
                break

            l += 2
            r -= 2

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Phần đầu tiên sắp xếp các que. Sắp xếp sẽ hiển thị các cặp thanh bằng nhau phải trở thành các cạnh hình chữ nhật. 

Vòng lặp đầu tiên xác nhận cấu trúc cặp. Kiểm tra từng vị trí thứ hai là đủ vì một bên hợp lệ yêu cầu chính xác hai gậy bằng nhau. Nếu thậm chí một cặp không thành công thì không có sự sắp xếp nào có thể sử dụng chính xác tất cả các que. 

Biến`target`lưu trữ diện tích mà mọi hình chữ nhật phải đạt được. Việc sử dụng độ dài cạnh còn lại nhỏ nhất và lớn nhất không phải là tùy ý. Trong bất kỳ cách xây dựng hợp lệ nào, hai bên đó phải thuộc về nhau. 

Vòng lặp hai con trỏ đi vào trong qua mảng đã được sắp xếp. Con trỏ luôn nằm trên phần tử đầu tiên của cặp bên và phần tử cuối cùng của cặp bên. So sánh sản phẩm của họ với mục tiêu sẽ xác minh rằng mọi hình chữ nhật sẽ có cùng diện tích. 

Số nguyên Python xử lý thoải mái sản phẩm lớn nhất có thể,`10000 × 10000 = 10^8`, vì vậy tràn không phải là một mối quan tâm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
1 1 2 2 3 3 6 6
```Mảng được sắp xếp:```
1 1 2 2 3 3 6 6
```Xác thực cặp: 

| Cặp chỉ số | Giá trị | hợp lệ | 
| --- | --- | --- | 
| (0,1) | 1,1 | Có | 
| (2,3) | 2,2 | Có | 
| (4,5) | 3,3 | Có | 
| (6,7) | 6,6 | Có | 

Khu vực mục tiêu:```
1 × 6 = 6
```Kiểm tra khu vực: 

| tôi | r | Sản phẩm | Dự kiến ​​| 
| --- | --- | --- | --- | 
| 0 | 7 | 1×6=6 | 6 | 
| 2 | 5 | 2×3=6 | 6 | 

Tất cả các sản phẩm đều trùng khớp, vì vậy câu trả lời là:```
YES
```Ví dụ này thể hiện tính bất biến trung tâm: mọi tích cặp cực trị đều giống hệt nhau. 

### Ví dụ 2 

đầu vào:```
n = 2
1 1 2 2 3 3 4 4
```Mảng được sắp xếp:```
1 1 2 2 3 3 4 4
```Xác thực cặp: 

| Cặp chỉ số | Giá trị | hợp lệ | 
| --- | --- | --- | 
| (0,1) | 1,1 | Có | 
| (2,3) | 2,2 | Có | 
| (4,5) | 3,3 | Có | 
| (6,7) | 4,4 | Có | 

Khu vực mục tiêu:```
1 × 4 = 4
```Kiểm tra khu vực: 

| tôi | r | Sản phẩm | Dự kiến ​​| 
| --- | --- | --- | --- | 
| 0 | 7 | 1×4=4 | 4 | 
| 2 | 5 | 2×3=6 | 4 | 

Sản phẩm thứ hai khác với mục tiêu nên câu trả lời là:```
NO
```Điều này cho thấy tại sao chỉ kiểm tra tính chẵn lẻ tần số là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp`4n`gậy chiếm ưu thế về thời gian chạy | 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài mảng đầu vào | 

Với tối đa 400 que cho mỗi truy vấn, việc phân loại cực kỳ nhanh chóng. Ngay cả trên tất cả 500 truy vấn, tổng số công việc vẫn được thực hiện thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    q = int(input())
    ans = []

    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        ok = True

        for i in range(0, 4 * n, 2):
            if a[i] != a[i + 1]:
                ok = False
                break

        if ok:
            target = a[0] * a[-1]

            l = 0
            r = 4 * n - 1

            while l < r:
                if a[l] * a[r] != target:
                    ok = False
                    break
                l += 2
                r -= 2

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

# provided sample
assert run(
"""5
1
1 1 10 10
2
10 5 2 10 1 1 2 5
2
10 5 1 10 5 1 1 1
2
1 1 1 1 1 1 1 1
1
10000 10000 10000 10000
"""
) == """YES
YES
NO
YES
YES"""

# minimum size
assert run(
"""1
1
5 5 7 7
"""
) == "YES"

# pair mismatch
assert run(
"""1
1
1 1 1 2
"""
) == "NO"

# equal pairs but unequal areas
assert run(
"""1
2
1 1 2 2 3 3 4 4
"""
) == "NO"

# all equal lengths
assert run(
"""1
3
5 5 5 5 5 5 5 5 5 5 5 5
"""
) == "YES"

# larger valid construction
assert run(
"""1
3
1 1 2 2 2 2 4 4 4 4 8 8
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 5 7 7`| CÓ | Trường hợp hợp lệ nhỏ nhất có thể | 
|`1 1 1 2`| KHÔNG | Thiếu cặp bằng nhau | 
|`1 1 2 2 3 3 4 4`| KHÔNG | Có sự ghép đôi nhưng các khu vực khác nhau | 
| Mười hai que đều bằng 5 | CÓ | Mọi hình chữ nhật đều trở thành hình vuông | 
|`1 1 2 2 2 2 4 4 4 4 8 8`| CÓ | Nhiều hình chữ nhật có diện tích phù hợp | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
1 1 1 2
```Sau khi sắp xếp ta thu được:```
1 1 1 2
```Cặp đôi`(1,2)`không kiểm tra sự bình đẳng. Thuật toán xuất ra ngay lập tức`NO`. Một giải pháp chỉ đếm tần số một cách lỏng lẻo có thể cố gắng tạo thành một hình chữ nhật một cách không chính xác. 

Coi như:```
1
2
1 1 2 2 3 3 4 4
```Tất cả các cặp liền kề đều hợp lệ, do đó các cạnh hình chữ nhật có thể được tạo thành. Vùng mục tiêu trở thành`1 × 4 = 4`. Trong quá trình quét vào bên trong, sản phẩm thứ hai được`2 × 3 = 6`, điều này phá vỡ bất biến mà mọi diện tích hình chữ nhật phải khớp. Thuật toán xuất ra chính xác`NO`. 

Coi như:```
1
2
1 1 1 1 1 1 1 1
```Mọi cặp liền kề đều bằng nhau. Khu vực mục tiêu là`1 × 1 = 1`, và mọi sản phẩm hướng nội cũng`1`. Đầu ra của thuật toán`YES`, xử lý chính xác trường hợp mọi hình chữ nhật đều là hình vuông giống nhau. 

Coi như:```
1
2
1 1 2 2 3 3 6 6
```Tất cả các cặp đều hợp lệ. Khu vực mục tiêu là`1 × 6 = 6`. Các sản phẩm bên trong là`1 × 6 = 6`Và`2 × 3 = 6`. Mỗi hình chữ nhật có diện tích`6`, do đó kết quả đầu ra của thuật toán`YES`. Điều này khẳng định rằng tích bất biến cặp cực trị là cần thiết và đủ.
