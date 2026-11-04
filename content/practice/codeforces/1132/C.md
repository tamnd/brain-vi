---
title: "CF 1132C - Sơn hàng rào"
description: "Chúng ta có một hàng rào có n phần được đánh số từ 1 đến n. Mỗi họa sĩ bao gồm một khoảng liên tục [li, ri]. Ban đầu tất cả q họa sĩ đều có sẵn, nhưng chúng tôi buộc phải loại bỏ chính xác hai trong số họ và giữ lại q - 2."
date: "2026-06-12T04:08:40+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1132
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 61 (Rated for Div. 2)"
rating: 1700
weight: 1132
solve_time_s: 122
verified: true
draft: false
---

[CF 1132C - Sơn hàng rào](https://codeforces.com/problemset/problem/1132/C) 

**Đánh giá:** 1700 
**Tags:** vũ lực 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng rào với`n`phần được đánh số từ`1`ĐẾN`n`. Mỗi họa sĩ bao gồm một khoảng thời gian liên tục`[l_i, r_i]`. Ban đầu tất cả`q`họa sĩ có sẵn, nhưng chúng tôi buộc phải loại bỏ chính xác hai người trong số họ và giữ lại những người còn lại`q - 2`. 

Phần hàng rào được sơn nếu có ít nhất một thợ sơn được giữ lại sơn phần đó. Mục đích là chọn ra hai họa sĩ cần loại bỏ sao cho tổng số phần được sơn càng lớn càng tốt. 

Đầu vào cho biết chiều dài hàng rào và khoảng cách được sơn bởi mỗi họa sĩ. Kết quả đầu ra là số phần hàng rào tối đa vẫn được sơn sau khi loại bỏ đúng hai thợ sơn. 

Các ràng buộc là điều đầu tiên thúc đẩy việc thiết kế giải pháp. Cả hai`n`Và`q`nhiều nhất là`5000`. Một cách tiếp cận mạnh mẽ nhằm thử mọi cặp họa sĩ đã bị loại bỏ đã yêu cầu khoảng$$\binom{5000}{2} \approx 12.5 \text{ million}$$cặp. 

Nghe có vẻ lớn nhưng có thể quản lý được. Vấn đề thực sự xuất hiện nếu chúng ta tính lại hàng rào sơn cho mỗi cặp. Làm thậm chí`O(n)`công việc cho mỗi cặp sẽ yêu cầu khoảng$$12.5 \cdot 10^6 \cdot 5000$$hoạt động hoàn toàn không thể thực hiện được. 

Cấu trúc của bài toán gợi ý rằng chúng ta nên tính toán trước thông tin về phạm vi bao phủ và sau đó đánh giá từng cặp bị loại bỏ theo thời gian không đổi hoặc logarit. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
5 3
1 5
1 5
1 5
```Loại bỏ hai họa sĩ bất kỳ vẫn để lại một họa sĩ phủ toàn bộ hàng rào, vì vậy câu trả lời là`5`. Một giải pháp chỉ tính không chính xác phạm vi phủ sóng duy nhất sẽ đánh giá thấp các phần được sơn còn lại. 

Một trường hợp quan trọng khác là:```
5 3
1 2
2 3
3 4
```Phần`2`được bao phủ bởi hai họa sĩ và phần`3`cũng được bao phủ bởi hai họa sĩ. Việc loại bỏ các cặp khác nhau sẽ ảnh hưởng đến các lớp phủ khác nhau. Chúng ta phải phân biệt cẩn thận các phần được đề cập đúng một lần và chính xác hai lần. 

Trường hợp tế nhị thứ ba là:```
5 3
1 1
3 3
5 5
```Loại bỏ hai họa sĩ bất kỳ chỉ để lại một phần được sơn. Phạm vi phủ sóng rất thưa thớt nên logic khoảng thời gian phải xử lý các khoảng trống một cách chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử từng cặp họa sĩ có thể loại bỏ được. 

Đối với một cặp cố định`(i, j)`, chúng ta có thể đánh dấu tất cả các phần được vẽ bởi các họa sĩ còn lại và đếm xem có bao nhiêu phần hàng rào vẫn còn được che phủ. Điều này đúng vì nó mô phỏng đúng nghĩa của vấn đề. 

Khó khăn là chi phí. có khoảng`12.5 million`cặp họa sĩ trong trường hợp xấu nhất. Phạm vi tính toán lại lên đến`5000`phần hàng rào cho mỗi cặp sẽ yêu cầu hàng chục tỷ thao tác. 

Quan sát quan trọng là chúng ta không cần phải xây dựng lại toàn bộ cấu hình vẽ cho từng cặp. Điều quan trọng là có bao nhiêu phần không được sơn sau khi loại bỏ hai họa sĩ. 

Đầu tiên, hãy tính xem mỗi phần hàng rào có bao nhiêu thợ sơn phủ lên nó. 

Cho phép`cover[x]`được tính như vậy. 

Giả sử chúng ta loại bỏ họa sĩ`i`Và`j`. 

Một phần biến mất khỏi bức tranh cuối cùng chỉ trong hai trường hợp. 

Nếu như`cover[x] = 1`, thì có đúng một họa sĩ phụ trách phần đó. Loại bỏ họa sĩ đó làm cho phần biến mất. 

Nếu như`cover[x] = 2`, thì có đúng hai họa sĩ che phần đó. Phần này chỉ biến mất khi cả hai họa sĩ đó bị xóa. 

Các phần có ít nhất ba lớp phủ vẫn được sơn bất kể hai họa sĩ nào đã bị loại bỏ. 

Điều này biến bài toán thành việc đếm tổn thất thay vì tính toán lại bức tranh cuối cùng. 

Cho phép:`total`= số phần được vẽ ban đầu bởi ít nhất một họa sĩ. 

Đối với mỗi họa sĩ`i`, định nghĩa:`one[i]`= số phần được phủ đúng một lần và được họa sĩ che phủ`i`. 

Những phần này biến mất bất cứ khi nào họa sĩ`i`được gỡ bỏ. 

Đối với mỗi cặp`(i, j)`, định nghĩa:`two[i][j]`= số phần được che phủ chính xác hai lần, đặc biệt là bởi các họa sĩ`i`Và`j`. 

Những phần này chỉ biến mất khi cả hai họa sĩ bị loại bỏ. 

Sau đó sau khi loại bỏ họa sĩ`i`Và`j`, các phần sơn còn lại bằng:$$total - one[i] - one[j] - two[i][j]$$Bây giờ nhiệm vụ trở thành tính toán`one`Và`two`một cách hiệu quả. 

Bởi vì các khoảng thời gian là liên tục và`n ≤ 5000`, chúng tôi có thể xử lý mọi vị trí hàng rào. Đối với các vị trí có phạm vi phủ sóng chính xác là một, chúng tôi xác định họa sĩ chịu trách nhiệm. Đối với các vị trí có phạm vi phủ sóng chính xác là hai, chúng tôi xác định cặp họa sĩ chịu trách nhiệm. 

Để truy vấn số lượng khoảng thời gian một cách hiệu quả, chúng tôi xây dựng tổng tiền tố trên các vị trí có phạm vi bao phủ bằng một và các vị trí có phạm vi bao phủ bằng hai. 

Sau đó, mỗi cặp họa sĩ có thể được đánh giá theo thời gian không đổi sau một vài truy vấn tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q²·n) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(nq + q2) | O(q²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng thời gian của họa sĩ. 
2. Xây dựng một mảng khác biệt trên hàng rào và tính toán`cover[x]`, số lượng họa sĩ vẽ từng phần. 
3. Đếm`total`, số đoạn hàng rào có`cover[x] > 0`. 
4. Tạo một mảng`cnt1[x]`Ở đâu`cnt1[x] = 1`nếu như`cover[x] = 1`, nếu không thì`0`. 
5. Tạo một mảng`cnt2[x]`Ở đâu`cnt2[x] = 1`nếu như`cover[x] = 2`, nếu không thì`0`. 
6. Xây dựng tổng tiền tố cho`cnt1`Và`cnt2`. 

Điều này cho phép chúng ta đếm xem có bao nhiêu vị trí có phạm vi bao phủ chính xác một hoặc chính xác hai vị trí nằm trong bất kỳ khoảng nào. 
7. Đối với mọi họa sĩ`i`, tính toán`one[i]`. 

Điều này bằng số lượng vị trí bên trong họa sĩ`i`khoảng thời gian có tổng phạm vi bao phủ chính xác là một. 
8. Dành cho mỗi cặp họa sĩ`(i, j)`, hãy tính có bao nhiêu vị trí được cả hai họa sĩ che phủ đúng hai lần. 

Vì vị trí được bao phủ chính xác hai lần và thuộc về cả hai họa sĩ phải nằm trong giao điểm của các khoảng của họ, nên chúng ta đếm các vị trí có`cover[x] = 2`bên trong ngã tư đó. 
9. Cho mỗi cặp`(i, j)`, tính toán$$total - one[i] - one[j] - two[i][j]$$và giữ giá trị lớn nhất. 
10. Xuất ra mức tối đa. 

### Tại sao nó hoạt động 

Một phần hàng rào chỉ có thể biến mất nếu tất cả các họa sĩ che phủ nó bị loại bỏ. 

Khi một phần có phạm vi bao phủ chính xác là một, việc loại bỏ họa sĩ duy nhất của nó là đủ để loại bỏ phần đó. Những phần như vậy góp phần`one[i]`. 

Khi một phần có phạm vi bao phủ chính xác là hai, cả hai họa sĩ chịu trách nhiệm phải bị loại bỏ đồng thời. Những phần như vậy góp phần`two[i][j]`. 

Khi một phần có ít nhất ba phạm vi phủ sóng, việc chỉ loại bỏ hai họa sĩ không bao giờ có thể loại bỏ tất cả phạm vi phủ sóng. 

Mỗi phần biến mất thuộc về chính xác một trong các danh mục này và không có phần nào được tính hai lần. Do đó,$$one[i] + one[j] + two[i][j]$$chính xác là số phần bị mất sau khi loại bỏ họa sĩ`i`Và`j`. Trừ đi sự mất mát này từ số lượng sơn ban đầu sẽ cho câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, q = map(int, input().split())

    segs = []
    diff = [0] * (n + 3)

    for _ in range(q):
        l, r = map(int, input().split())
        segs.append((l, r))
        diff[l] += 1
        diff[r + 1] -= 1

    cover = [0] * (n + 1)
    cur = 0
    total = 0

    for x in range(1, n + 1):
        cur += diff[x]
        cover[x] = cur
        if cur > 0:
            total += 1

    one_pos = [0] * (n + 1)
    two_pos = [0] * (n + 1)

    for x in range(1, n + 1):
        if cover[x] == 1:
            one_pos[x] = 1
        elif cover[x] == 2:
            two_pos[x] = 1

    pref1 = [0] * (n + 1)
    pref2 = [0] * (n + 1)

    for x in range(1, n + 1):
        pref1[x] = pref1[x - 1] + one_pos[x]
        pref2[x] = pref2[x - 1] + two_pos[x]

    one = [0] * q

    for i, (l, r) in enumerate(segs):
        one[i] = pref1[r] - pref1[l - 1]

    ans = 0

    for i in range(q):
        l1, r1 = segs[i]

        for j in range(i + 1, q):
            l2, r2 = segs[j]

            left = max(l1, l2)
            right = min(r1, r2)

            both = 0
            if left <= right:
                both = pref2[right] - pref2[left - 1]

            remaining = total - one[i] - one[j] - both
            if remaining > ans:
                ans = remaining

    print(ans)

if __name__ == "__main__":
    main()
```Giai đoạn đầu tiên tính toán số lượng vùng phủ sóng cho mỗi phần hàng rào bằng cách sử dụng một mảng khác biệt. Điều này tránh việc đánh dấu từng khoảng thời gian riêng lẻ và đưa ra tất cả số lượng phạm vi bảo hiểm trong`O(n + q)`thời gian. 

Các mảng`one_pos`Và`two_pos`xác định các vị trí có phạm vi bao phủ chính xác là một hoặc chính xác là hai. Tổng tiền tố sau đó cho phép truy vấn khoảng thời gian trong thời gian không đổi. 

giá trị`one[i]`có được bằng cách truy vấn số vị trí bao phủ-một bên trong họa sĩ`i`khoảng thời gian của. Mọi vị trí như vậy đều biến mất bất cứ khi nào họa sĩ`i`được gỡ bỏ. 

Đối với một cặp họa sĩ, chỉ có giao điểm của các khoảng thời gian của họ mới quan trọng. Phần có phạm vi bao phủ chính xác là hai chỉ biến mất nếu nó thuộc về cả hai họa sĩ, nghĩa là nó nằm trong giao lộ đó. Truy vấn số lượng vùng phủ sóng-hai vị trí trong giao điểm cho`two[i][j]`. 

Lỗi triển khai phổ biến nhất là quên rằng giao lộ có thể trống. Trong trường hợp đó`both`phải duy trì bằng không. 

Một lỗi dễ xảy ra khác xuất phát từ các truy vấn tổng tiền tố. sử dụng```
pref[r] - pref[l - 1]
```yêu cầu các mảng được lập chỉ mục từ`1`và một mục nhập tiền tố thứ 0 hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 5
1 4
4 5
5 6
6 7
3 5
```Số lượng bảo hiểm: 

| Vị trí | Bảo hiểm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 | 
| 5 | 3 | 
| 6 | 2 | 
| 7 | 1 | 

Như vậy`total = 7`. 

Số lượng bìa duy nhất: 

| Họa sĩ | Khoảng thời gian | một[i] | 
| --- | --- | --- | 
| 1 | [1,4] | 2 | 
| 2 | [4,5] | 0 | 
| 3 | [5,6] | 0 | 
| 4 | [6,7] | 1 | 
| 5 | [3,5] | 0 | 

Nếu loại bỏ họa sĩ 2 và 3, giao điểm của họ chỉ chứa vị trí 5 nhưng phạm vi bao phủ là 3, vì vậy`both = 0`. 

Còn lại: 

| Đã xóa cặp | Mất mát | Còn lại | 
| --- | --- | --- | 
| (2,3) | 0 | 7 | 
| (1,4) | 3 | 4 | 
| (1,5) | 3 | 4 | 

Tối đa là`7`. 

Ví dụ này cho thấy rằng việc loại bỏ các họa sĩ không có phần quan trọng duy nhất có thể khiến toàn bộ hàng rào được sơn. 

### Ví dụ 2 

đầu vào:```
5 3
1 3
2 4
3 5
```Số lượng bảo hiểm: 

| Vị trí | Bảo hiểm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 1 |`total = 5`. 

| Họa sĩ | một[i] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 

Đối với cặp`(1,2)`, giao điểm là`[2,3]`. 

Vị trí duy nhất`2`có phạm vi bảo hiểm chính xác là hai. 

| Cặp | một[i] | một[j] | cả hai | Còn lại | 
| --- | --- | --- | --- | --- | 
| (1,2) | 1 | 0 | 1 | 3 | 
| (1,3) | 1 | 1 | 0 | 3 | 
| (2,3) | 0 | 1 | 1 | 3 | 

Trả lời =`3`. 

Dấu vết này cho thấy cách bao phủ-hai vị trí chỉ được tính cho cặp họa sĩ cụ thể chịu trách nhiệm về chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q2) | Tính toán phạm vi bao phủ là tuyến tính, đánh giá cặp kiểm tra tất cả các cặp họa sĩ | 
| Không gian | O(n + q) | Mảng bảo hiểm, tổng tiền tố và lưu trữ theo khoảng thời gian | 

Với`q ≤ 5000`, thuật ngữ chiếm ưu thế là đại khái$$\binom{5000}{2} \approx 12.5 \text{ million}$$đánh giá cặp đôi. Mỗi đánh giá chỉ thực hiện một số phép tính số học và các truy vấn tổng tiền tố theo thời gian không đổi, phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, q = map(int, input().split())

    segs = []
    diff = [0] * (n + 3)

    for _ in range(q):
        l, r = map(int, input().split())
        segs.append((l, r))
        diff[l] += 1
        diff[r + 1] -= 1

    cover = [0] * (n + 1)
    cur = 0
    total = 0

    for i in range(1, n + 1):
        cur += diff[i]
        cover[i] = cur
        if cur:
            total += 1

    one_pos = [0] * (n + 1)
    two_pos = [0] * (n + 1)

    for i in range(1, n + 1):
        if cover[i] == 1:
            one_pos[i] = 1
        elif cover[i] == 2:
            two_pos[i] = 1

    pref1 = [0] * (n + 1)
    pref2 = [0] * (n + 1)

    for i in range(1, n + 1):
        pref1[i] = pref1[i - 1] + one_pos[i]
        pref2[i] = pref2[i - 1] + two_pos[i]

    one = [0] * q
    for i, (l, r) in enumerate(segs):
        one[i] = pref1[r] - pref1[l - 1]

    ans = 0

    for i in range(q):
        l1, r1 = segs[i]
        for j in range(i + 1, q):
            l2, r2 = segs[j]

            L = max(l1, l2)
            R = min(r1, r2)

            both = 0
            if L <= R:
                both = pref2[R] - pref2[L - 1]

            ans = max(ans, total - one[i] - one[j] - both)

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run(
"""7 5
1 4
4 5
5 6
6 7
3 5
"""
) == "7", "sample 1"

# minimum sizes
assert run(
"""3 3
1 1
2 2
3 3
"""
) == "1", "minimum q"

# all intervals equal
assert run(
"""5 3
1 5
1 5
1 5
"""
) == "5", "all equal intervals"

# sparse intervals
assert run(
"""5 3
1 1
3 3
5 5
"""
) == "1", "disjoint painters"

# off-by-one boundary coverage
assert run(
"""5 3
1 2
2 3
3 5
"""
) == "3", "endpoint intersections"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba họa sĩ đơn bào | 1 | Kích thước hợp lệ tối thiểu | 
| Tất cả các khoảng thời gian`[1,5]`| 5 | Chồng chéo nặng nề khắp nơi | 
| Ba khoảng rời rạc | 1 | Bảo hiểm thưa thớt | 
| Khoảng thời gian chạm điểm cuối | 3 | Đúng ranh giới giao lộ | 

## Vỏ cạnh 

Hãy xem xét:```
5 3
1 5
1 5
1 5
```Bảo hiểm là`3`ở mọi vị trí. Không có vị trí bảo hiểm-một hoặc bảo hiểm-hai. Mọi`one[i]`bằng`0`, mọi`two[i][j]`bằng`0`, Và$$5 - 0 - 0 - 0 = 5$$cho mỗi cặp. Thuật toán trả về chính xác`5`. 

Bây giờ hãy xem xét:```
5 3
1 1
3 3
5 5
```Bảo hiểm-một vị trí là chính xác`{1,3,5}`. Mỗi họa sĩ sở hữu một vị trí như vậy. Loại bỏ hai họa sĩ bất kỳ sẽ làm mất hai phần đã sơn, chỉ còn lại một phần. Thuật toán tính toán:$$3 - 1 - 1 = 1$$và trả về`1`. 

Cuối cùng hãy xem xét:```
5 3
1 2
2 3
3 4
```Số lượng bảo hiểm là`[1,2,2,1,0]`. Vị trí`2`Và`3`có phạm vi bảo hiểm chính xác là hai. Loại bỏ họa sĩ`1`Và`2`mất vị trí`1`từ vùng phủ sóng và vị trí độc đáo`2`từ phạm vi bảo hiểm kép. Thuật toán tính những tổn thất này một cách riêng biệt thông qua`one`Và`both`, tránh việc tính hai lần và cho ra kết quả chính xác.
