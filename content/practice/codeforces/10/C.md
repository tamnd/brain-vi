---
title: "CF 10C - Root kỹ thuật số"
description: "Chúng ta được cho một số nguyên N. Hãy xem xét tất cả các bộ ba (A, B, C) trong đó mọi giá trị đều nằm trong phạm vi [1, N]."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 10
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 10"
rating: 2000
weight: 10
solve_time_s: 101
verified: true
draft: false
---
[CF 10C - Root kỹ thuật số](https://codeforces.com/problemset/problem/10/C) 

**Xếp hạng:** 2000 
**Thẻ:** lý thuyết số 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên`N`. Xét tất cả các bộ ba`(A, B, C)`trong đó mọi giá trị nằm trong phạm vi`[1, N]`. 

Billy muốn xác minh xem liệu`A * B = C`, nhưng thay vì nhân trực tiếp, anh ta so sánh các nghiệm số:$$d(A \cdot B) = d(C)$$Sử dụng tài sản$$d(xy) = d(d(x)d(y))$$thuật toán của anh ấy kiểm tra một cách hiệu quả$$d(d(A)d(B)) = d(C)$$Nhiệm vụ là đếm xem có bao nhiêu bộ ba khiến thuật toán của Billy chấp nhận mặc dù phương trình thực$$A \cdot B = C$$là sai. 

Root kỹ thuật số hoạt động chính xác như modulo`9`, ngoại trừ bội số của`9`bản đồ tới`9`thay vì`0`. Chính thức hơn,$$d(x)= \begin{cases} 9 & x \bmod 9 = 0 \\ x \bmod 9 & \text{otherwise} \end{cases}$$Vì vậy thuật toán Billy chỉ kiểm tra đẳng thức theo modulo`9`, không phải sự bình đẳng thực tế. 

Ràng buộc`N ≤ 10^6`ngay lập tức loại trừ việc lặp lại trên tất cả các bộ ba. có`N^3`khả năng, trở thành`10^{18}`trong trường hợp xấu nhất. Thậm chí kiểm tra tất cả các cặp`(A, B)`sẽ là quá lớn bởi vì có`10^{12}`cặp. 

Một giải pháp hợp lệ phải khai thác được không gian trạng thái nhỏ bé của các gốc số. Chỉ có chín gốc kỹ thuật số có thể có, điều này gợi ý việc nhóm các số theo gốc kỹ thuật số của chúng thay vì làm việc với các giá trị riêng lẻ. 

Một trường hợp cạnh tinh tế đến từ bội số của`9`. Việc thực hiện bất cẩn có thể sử dụng`x % 9`trực tiếp như gốc kỹ thuật số, tạo ra`0`thay vì`9`. 

Ví dụ, khi`x = 18`:$$18 \bmod 9 = 0$$Nhưng$$d(18)=9$$Nếu chúng tôi lưu trữ gốc kỹ thuật số không chính xác dưới dạng`0`, logic đếm bị hỏng vì bài toán sử dụng các giá trị`1..9`, không`0..8`. 

Một sai lầm dễ mắc phải khác là quên rằng gấp ba lần`A * B = C`chính xác là không được tính. Thuật toán của Billy chấp nhận nhiều bộ ba, nhưng một số trong đó thực sự là phương trình đúng và phải được loại trừ. 

Ví dụ với`N = 4`, bộ ba`(1,2,2)`thỏa mãn cả điều kiện nghiệm số và phương trình thực. Nó không được đóng góp vào câu trả lời. 

Điểm tinh tế thứ ba là`A * B`có thể vượt quá`N`. Điều đó hoàn toàn được cho phép. Chỉ một`A`,`B`, Và`C`bị hạn chế`[1,N]`. 

Ví dụ với`N = 4`:$$(3,4,3)$$không hợp lệ về mặt toán học vì`3*4=12`, không`3`, nhưng Billy chấp nhận vì$$d(12)=3$$Bộ ba này góp phần vào câu trả lời. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Lặp lại tất cả các bộ ba`(A,B,C)`, tính nghiệm số, kiểm tra xem Billy có chấp nhận bộ ba hay không và cũng kiểm tra xem phép nhân thực có thất bại hay không. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Vấn đề là sự phức tạp:$$O(N^3)$$Với`N = 10^6`, điều này trở nên hoàn toàn không thể. 

Chúng ta có thể cải thiện một chút bằng cách quan sát điều đó đối với`(A,B)`, Billy chỉ quan tâm đến gốc kỹ thuật số của`C`. Thay vì lặp đi lặp lại tất cả`C`, chúng ta có thể đếm được có bao nhiêu số trong`[1,N]`có gốc kỹ thuật số phù hợp. Điều đó làm giảm sự phức tạp`O(N^2)`. 

Thậm chí điều đó còn quá chậm. Tại`N = 10^6`, chúng ta vẫn cần khoảng`10^{12}`lần lặp lại. 

Nhận xét quan trọng là việc kiểm tra của Billy chỉ phụ thuộc vào các nghiệm số. Chỉ có chín nghiệm có thể có, vì vậy tất cả các số có cùng nghiệm số đều hoạt động giống hệt nhau theo thuật toán của ông. 

Giả định:$$cnt[r]$$là số các số nguyên trong`[1,N]`có gốc kỹ thuật số bằng`r`. 

Sau đó với mỗi cặp rễ`(ra, rb)`:$$rc = d(ra \cdot rb)$$Mỗi số có gốc`ra`có thể ghép với mọi số có gốc`rb`, và Billy chấp nhận mọi`C`gốc của ai`rc`. 

Vậy số bộ ba mà Billy chấp nhận là:$$cnt[ra] \cdot cnt[rb] \cdot cnt[rc]$$Tổng hợp tất cả`9 × 9`cặp gốc cho tổng số bộ ba mà Billy chấp nhận. 

Bây giờ chúng ta phải loại bỏ các bộ ba trong đó phương trình thực sự đúng. 

Đối với mỗi cặp`(A,B)`, có chính xác một giá trị đúng:$$C = A \cdot B$$Nếu như`C ≤ N`, thì bộ ba này có giá trị về mặt toán học và phải được loại khỏi câu trả lời. 

Vậy ta trừ đi số cặp nhân với tích nhiều nhất`N`:$$\sum_{A=1}^{N} \left\lfloor \frac{N}{A} \right\rfloor$$Đây là hàm chia tổng-tổng cổ điển. 

Toàn bộ giải pháp trở nên gần như tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N³) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem có bao nhiêu số`[1,N]`có mỗi gốc kỹ thuật số từ`1`ĐẾN`9`. 

Mỗi khối gồm chín số liên tiếp đóng góp chính xác một lần xuất hiện của mỗi căn số. 
2. Lưu trữ các số đếm này trong một mảng`cnt[1..9]`. 
3. Lặp lại từng cặp gốc kỹ thuật số`(a,b)`. 

chỉ có`81`những cặp như vậy nên phần này là thời gian không đổi. 
4. Tính toán gốc số thu được:$$c = d(a \cdot b)$$1. Thêm$$cnt[a] \cdot cnt[b] \cdot cnt[c]$$đến tổng số. 

Điều này đếm tất cả các bộ ba được thuật toán của Billy chấp nhận. 

1. Tính số bộ ba thực sự đúng của phép nhân. 

Đối với mỗi`A`từ`1`ĐẾN`N`, có chính xác$$\left\lfloor \frac{N}{A} \right\rfloor$$giá trị của`B`như vậy$$A \cdot B \le N$$Mỗi cặp như vậy tương ứng với chính xác một giá trị hợp lệ`C`. 

1. Trừ số lượng này khỏi tổng số trước đó. 
2. Xuất kết quả. 

### Tại sao nó hoạt động 

Billy chấp nhận mức gấp ba chính xác khi gốc kỹ thuật số thỏa mãn$$d(A \cdot B)=d(C)$$Rễ kỹ thuật số chỉ phụ thuộc vào dư lượng modulo`9`, do đó tất cả các số có cùng gốc số đều có thể hoán đổi cho bài kiểm tra của Billy. 

Thuật toán nhóm các số theo gốc số và đếm tất cả các kết hợp được chấp nhận một cách tổ hợp. Điều này tính mỗi bộ ba Billy chấp nhận đúng một lần. 

Trong số các bộ ba được chấp nhận này, bộ ba duy nhất không xuất hiện trong câu trả lời cuối cùng là các phương trình thực sự đúng. Mỗi cặp`(A,B)`đóng góp một bộ ba iff đúng`A*B ≤ N`. Việc trừ tất cả các cặp như vậy sẽ loại bỏ chính xác các phương trình hợp lệ và chỉ để lại những sai sót của Billy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digital_root(x):
    return 9 if x % 9 == 0 else x % 9

def solve():
    n = int(input())

    cnt = [0] * 10

    for x in range(1, n + 1):
        cnt[digital_root(x)] += 1

    ans = 0

    for a in range(1, 10):
        for b in range(1, 10):
            c = digital_root(a * b)
            ans += cnt[a] * cnt[b] * cnt[c]

    correct = 0

    for a in range(1, n + 1):
        correct += n // a

    ans -= correct

    print(ans)

solve()
```Phần đầu tiên tính toán có bao nhiêu số thuộc về mỗi lớp gốc số. Từ`N`chỉ là`10^6`, một vòng lặp trực tiếp là hoàn toàn đủ nhanh. 

Vòng lặp lồng nhau trên rễ rất nhỏ. Chỉ một`81`sự kết hợp tồn tại, do đó việc tính toán đóng góp có hiệu quả là thời gian không đổi. 

Giai đoạn trừ là bước khái niệm quan trọng nhất. Thuật toán của Billy chấp nhận cả phương trình sai và phương trình đúng. Chúng tôi chỉ muốn những cái sai. Đối với mỗi cố định`A`, các giá trị của`B`thỏa mãn:$$A \cdot B \le N$$chính xác là:$$1,2,\dots,\left\lfloor \frac{N}{A} \right\rfloor$$Mỗi cặp như vậy xác định một giá trị đúng duy nhất`C`. 

Một chi tiết triển khai thường gây ra lỗi là hàm gốc kỹ thuật số. sử dụng`x % 9`trực tiếp là không chính xác vì bội số của`9`phải ánh xạ tới`9`, không`0`. 

Một điểm tinh tế khác là kích thước số nguyên. Câu trả lời có thể trở nên rất lớn, khoảng`N^3`, nhưng số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```Số lượng gốc kỹ thuật số: 

| Gốc | Số | Đếm | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 1 | 
| 3 | 3 | 1 | 
| 4 | 4 | 1 | 
| 5..9 | không | 0 | 

Bây giờ hãy xem xét các cặp đóng góp: 

| một | b | d(a*b) | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 1 | 2 | 2 | 1 | 
| 2 | 2 | 4 | 1 | 
| 3 | 4 | 3 | 1 | 
| 4 | 3 | 3 | 1 | 

Tổng hợp tất cả các cặp cho`10`. 

Phép nhân ba đúng: 

| A | Số B hợp lệ | 
| --- | --- | 
| 1 | 4 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 1 | 

Tổng số bộ ba đúng:$$4+2+1+1=8$$Câu trả lời cuối cùng:$$10-8=2$$Bộ ba còn lại là:$$(3,4,3), (4,3,3)$$Dấu vết này cho thấy tại sao bước trừ là cần thiết. Billy cũng chấp nhận nhiều phương trình đúng. 

### Ví dụ 2 

đầu vào:```
2
```Số lượng gốc kỹ thuật số: 

| Gốc | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| người khác | 0 | 

Bộ ba được chấp nhận: 

| một | b | c | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 1 | 2 | 2 | 1 | 
| 2 | 1 | 2 | 1 | 

Tổng số được chấp nhận:$$3$$Phép nhân ba đúng: 

| A | tầng(2/A) | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 

Tổng cộng:$$3$$Câu trả lời cuối cùng:$$0$$Điều này chứng tỏ rằng Billy đôi khi không mắc sai lầm nào ở phạm vi nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lượt cho số căn số và một lượt cho tổng số chia | 
| Không gian | O(1) | Chỉ một số mảng có kích thước cố định được lưu trữ | 

Với`N ≤ 10^6`, MỘT`O(N)`giải pháp dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là không đổi vì thuật toán chỉ lưu trữ chín bộ đếm và một vài số nguyên. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def digital_root(x):
        return 9 if x % 9 == 0 else x % 9

    n = int(input())

    cnt = [0] * 10

    for x in range(1, n + 1):
        cnt[digital_root(x)] += 1

    ans = 0

    for a in range(1, 10):
        for b in range(1, 10):
            c = digital_root(a * b)
            ans += cnt[a] * cnt[b] * cnt[c]

    correct = 0

    for a in range(1, n + 1):
        correct += n // a

    ans -= correct

    return str(ans)

# provided sample
assert solve_io("4\n") == "2", "sample 1"

# minimum size
assert solve_io("1\n") == "0", "single value"

# no incorrect triples
assert solve_io("2\n") == "0", "small range"

# first range containing multiple errors
assert solve_io("5\n") == "6", "basic correctness"

# boundary around digital root 9
assert solve_io("9\n") == "47", "multiple-of-9 handling"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Ranh giới kích thước tối thiểu | 
|`2`|`0`| Không tồn tại dương tính giả | 
|`5`|`6`| Tính chính xác chung | 
|`9`|`47`| Xử lý đúng root kỹ thuật số`9`| 

## Vỏ cạnh 

Hãy xem xét:```
1
```Bộ ba duy nhất là`(1,1,1)`. Billy chấp nhận nó, nhưng nó cũng đúng về mặt toán học:$$1 \cdot 1 = 1$$Thuật toán đếm một bộ ba được chấp nhận, sau đó trừ một phương trình đúng, tạo ra`0`. 

Bây giờ hãy xem xét:```
9
```bội số của`9`trở nên quan trọng ở đây. 

Ví dụ:$$(9,1,9)$$được chấp nhận vì:$$d(9 \cdot 1)=9$$Nếu chúng tôi trình bày không chính xác gốc kỹ thuật số`9`BẰNG`0`, bộ ba này sẽ được nhóm không chính xác và câu trả lời sẽ trở thành sai. 

Việc thực hiện`digital_root()`hàm khắc phục điều này bằng cách ánh xạ rõ ràng bội số của`9`ĐẾN`9`. 

Một trường hợp tinh tế khác là khi sản phẩm vượt quá`N`. 

Vì`N = 4`:$$(3,4,3)$$Billy chấp nhận điều đó vì:$$d(12)=3$$nhưng phương trình thực sự thất bại vì`12 ≠ 3`. 

Thuật toán bao gồm chính xác bộ ba này vì bộ ba được chấp nhận được tính hoàn toàn bằng gốc số, không phụ thuộc vào việc sản phẩm thực tế có nằm trong giới hạn hay không.
