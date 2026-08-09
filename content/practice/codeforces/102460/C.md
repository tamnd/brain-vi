---
title: "CF 102460C - Chúng đều là số nguyên phải không?"
description: "Chúng ta có một mảng được sắp xếp gồm các số nguyên dương (A), với (3 le n le 50). Chúng ta phải quyết định xem mọi lựa chọn của ba vị trí phân biệt (i,j,k) có thỏa mãn [ frac{A[i]-A[j]}{A[k]} trong mathbb Z hay không."
date: "2026-08-09T18:34:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "C"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 414
verified: true
draft: false
---

[CF 102460C - Tất cả đều là số nguyên?](https://codeforces.com/problemset/problem/102460/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 54 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng được sắp xếp gồm các số nguyên dương (A), với (3 \le n \le 50). Chúng ta phải quyết định xem mọi lựa chọn của ba vị trí riêng biệt (i,j,k) có thỏa mãn 

[ 
\frac{A[i]-A[j]}{A[k]} \in \mathbb Z. 
] 

Vì mọi (A[k]) đều dương nên điều này cũng giống như hỏi liệu (A[k]) có chia hết (A[i]-A[j]) hay không. Thứ tự của (i) và (j) không quan trọng đối với khả năng chia hết vì nếu một số chia hết (x-y) thì nó cũng chia hết (y-x). 

Đầu ra cần thiết là`yes`nếu điều kiện đúng cho mọi bộ ba chỉ số riêng biệt có thể có và`no`ngay khi một bộ ba hợp lệ vi phạm nó. 

Mảng chứa tối đa 50 phần tử, do đó, ngay cả phép liệt kê (O(n^3)) cũng chỉ thực hiện 

[ 
50\cdot49\cdot48=117600 
] 

kiểm tra trong trường hợp xấu nhất. Đó là rất nhỏ đối với giới hạn 2 giây, vì vậy việc triển khai vũ lực theo nghĩa đen đã đủ nhanh. Giới hạn trên (A[i]\le100) thậm chí không cần thiết cho giải pháp cơ bản. Tuy nhiên, nó làm rõ rằng số học số nguyên là hoàn toàn an toàn và không cần đến các kỹ thuật số lớn. 

Có một số chi tiết có thể khiến việc triển khai có vẻ hợp lý trở nên không chính xác. Đầu tiên, ba chỉ số phải khác biệt. Ví dụ, với`1 2 4`, bộ ba hợp lệ duy nhất sử dụng cả ba vị trí và kết quả là`no`bởi vì 

[ 
\frac{1-2}{4}=-\frac14 
] 

không phải là số nguyên. Việc triển khai vô tình cho phép chỉ số của mẫu số trùng với một trong các chỉ số của tử số có thể kiểm tra các biểu thức không phải là một phần của vấn đề. 

Thứ hai, tử số là một sự khác biệt, không phải là giá trị mảng riêng lẻ. Vì`2 4 6`, mẫu số`6`phải chia (2-4=-2), nhưng không phải vậy, nên câu trả lời là`no`. Việc kiểm tra xem cả hai giá trị tử số có chia hết riêng lẻ cho mẫu số hay không sẽ giải quyết được một vấn đề khác. 

Thứ ba, tử số có thể âm. Ví dụ, trong`1 1 4`, chênh lệch có thể là (1-4=-3) nếu mẫu số là một phần tử khác, do đó việc triển khai phải kiểm tra tính chia hết thay vì giả sử tử số không âm. của Python`%`toán tử xử lý chính xác các số nguyên âm để kiểm tra tính chia hết vì (x % d=0) chính xác khi (d) chia (x). 

Cuối cùng, các giá trị lặp lại là hoàn toàn hợp pháp. Vì`1 1 1 1 4`, mọi sự khác biệt giữa bốn`1`s bằng 0 và số 0 chia hết cho mọi số nguyên dương. Câu trả lời đúng là`yes`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất tuân theo định nghĩa toán học. Liệt kê từng bộ ba có thứ tự của các chỉ số riêng biệt (i,j,k), tính toán (A[i]-A[j]) và kiểm tra xem nó có chia hết cho (A[k]) hay không. Nếu bất kỳ kiểm tra nào không thành công, câu trả lời sẽ có ngay lập tức`no`; nếu tất cả các bước kiểm tra đều đạt, câu trả lời là`yes`. 

Có chính xác (n(n-1)(n-2)) bộ ba có thứ tự. Ở mức tối đa (n=50), đó chỉ là 117600 kiểm tra chia hết. Vì vậy, mặc dù (O(n^3)) sẽ trở nên kém hấp dẫn đối với các mảng lớn hơn nhiều, nhưng nó hoàn toàn có thể chấp nhận được đối với các ràng buộc thực tế của vấn đề này. 

Có một quan sát hữu ích giúp giảm công xuống còn (O(n^2)). Cố định vị trí mẫu số (k). Điều kiện nói rằng với mọi cặp vị trí (i,j) khác với (k), 

[ 
A[i]\equiv A[j]\pmod{A[k]}. 
] 

Nói cách khác, sau khi loại bỏ (A[k]), mọi giá trị mảng còn lại phải có cùng modulo dư (A[k]). 

Chúng ta không cần phải so sánh từng cặp để thiết lập tính chất đó. Chọn một vị trí tham chiếu (r\ne k). Nếu mọi vị trí khác (j\ne k,r) thỏa mãn 

[ 
A[j]\equiv A[r]\pmod{A[k]}, 
] 

thì hai giá trị bất kỳ có cùng số dư nên hiệu của chúng chia hết cho (A[k]). Như vậy, với mỗi (k), một phần tử tham chiếu là đủ. 

Lực lượng vũ phu hoạt động vì nó trực tiếp kiểm tra mọi bộ ba được yêu cầu, nhưng về cơ bản nó lặp lại cùng một thông tin mô-đun nhiều lần. Quan sát rằng tất cả các phần tử không phải (A[k]) phải thuộc về một lớp dư lượng modulo (A[k]) cho phép chúng ta thay thế tất cả các so sánh theo cặp bằng các so sánh với một tham chiếu. 

Phiên bản bậc hai không cần thiết cho (n\le50) đã cho, nhưng nó mang lại một biểu thức rõ ràng hơn về toán học cơ bản và thang đo tốt hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(1)) | Đã chấp nhận | 
| Kiểm tra lớp dư lượng | (O(n^2)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và mảng (A). Chúng ta chỉ cần bản thân các giá trị và thuật toán không yêu cầu việc sắp xếp đã cho. 
2. Coi mỗi vị trí mảng (k) là vị trí mẫu số. Chúng ta phải xác minh rằng mọi cặp giá trị tại các vị trí khác với (k) đều có hiệu chia hết cho (A[k]). 
3. Chọn vị trí tham chiếu (r) khác với (k). Chúng ta có thể sử dụng vị trí`0`trừ khi (k=0), trong trường hợp đó chúng ta sử dụng vị trí`1`. Bởi vì (n\ge3) nên vị trí tham chiếu như vậy luôn tồn tại. 
4. Đối với mọi vị trí khác (j), bỏ qua (k) và (r), hãy kiểm tra xem 
[ 
(A[j]-A[r])\bmod A[k]=0. 
] 
Điều này nói lên rằng (A[j]) và giá trị tham chiếu có cùng modulo dư (A[k]). 
5. Nếu chênh lệch không chia hết cho (A[k]), in ngay`no`. Cặp thất bại (j,r), cùng với mẫu số (k), là bộ ba hợp lệ của các vị trí phân biệt, do đó điều kiện ban đầu không thể giữ được. 
6. Nếu mọi vị trí (k) đều đạt, hãy in`yes`. Đối với mỗi mẫu số, tất cả các giá trị khác đều bằng modulo của mẫu số đó, do đó mọi hiệu giữa hai trong số chúng đều chia hết cho nó. 

### Tại sao nó hoạt động 

Cố định bất kỳ vị trí mẫu số (k). Thuật toán đảm bảo rằng mọi giá trị khác (A[j]) có cùng modulo dư (A[k]) như giá trị tham chiếu (A[r]). Do đó, với hai vị trí bất kỳ (i,j\ne k), 

[ 
A[i]\equiv A[r]\equiv A[j]\pmod{A[k]}. 
] 

Trừ các đồng đẳng sẽ cho 

[ 
A[i]-A[j]\equiv0\pmod{A[k]}, 
] 

do đó (A[k]) chia hết (A[i]-A[j]). Do đó mọi bộ ba có mẫu số là (A[k]) đều hợp lệ. Vì thuật toán thực hiện đối số này với mọi (k), nên mọi bộ ba được phép đều hợp lệ. Ngược lại, nếu thuật toán tìm thấy hai giá trị có số dư khác nhau đối với một số (k), thì hiệu của chúng không chia hết cho (A[k]), tạo ra một bộ ba không hợp lệ rõ ràng. Do đó, thuật toán trả về`yes`chính xác khi điều kiện yêu cầu được thoả mãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    for k in range(n):
        # Choose any index different from k as the reference.
        ref = 0 if k != 0 else 1

        for j in range(n):
            if j == k or j == ref:
                continue

            if (a[j] - a[ref]) % a[k] != 0:
                print("no")
                return

    print("yes")

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài cố định mẫu số (A[k]). Khi (k) được sửa, bài toán sẽ trở thành một phát biểu về số dư của tất cả các giá trị khác theo modulo (A[k]). 

các`ref`lựa chọn đảm bảo rằng chỉ số tham chiếu khác với chỉ số mẫu số. Nếu như`k`không bằng 0, chỉ số`0`hoạt động. Nếu như`k`bằng 0, chỉ số`1`hoạt động. Vì (n\ge3), chỉ số`1`luôn tồn tại. 

Vòng lặp bên trong bỏ qua cả hai`k`Và`ref`. Mỗi vị trí còn lại đại diện cho một giá trị phải có cùng số dư như`a[ref]`modulo`a[k]`. 

biểu thức`(a[j] - a[ref]) % a[k]`thích hợp hơn là thực hiện phép chia và kiểm tra xem kết quả có phải là số nguyên hay không. Nó chỉ sử dụng số học số nguyên và kiểm tra trực tiếp tính chia hết. Số nguyên Python cũng tránh được mọi lo ngại về tràn, mặc dù các giá trị đã cho quá nhỏ nên số nguyên máy thông thường đã đủ. 

Thuật toán trả về ngay sau khi tìm thấy mâu thuẫn. Điều này là an toàn vì thuộc tính bắt buộc là phổ biến: một bộ ba không hợp lệ là đủ để xác định rằng câu trả lời là`no`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
5
1 1 1 1 4
```Hãy xem xét từng vị trí mẫu số có thể. Bảng sau đây trình bày các bước kiểm tra cần thiết. Tham chiếu được chọn theo thuật toán. 

| (k) | (A[k]) | Tham khảo | Giá trị so sánh với tham chiếu | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1, 1, 4 | tất cả sự khác biệt chia hết cho 1 | 
| 1 | 1 | 0 | 1, 1, 4 | tất cả sự khác biệt chia hết cho 1 | 
| 2 | 1 | 0 | 1, 1, 4 | tất cả sự khác biệt chia hết cho 1 | 
| 3 | 1 | 0 | 1, 1, 4 | tất cả sự khác biệt chia hết cho 1 | 
| 4 | 4 | 0 | 1, 1, 1 | tất cả sự khác biệt là 0 | 

Đối với (k=0,1,2,3), mẫu số là 1, chia hết mọi số nguyên. Với (k=4), bốn giá trị còn lại đều bằng 1, do đó mọi hiệu có thể có đều bằng 0 và 0 chia hết cho 4. 

Do đó, thuật toán đi đến kết thúc mà không tìm thấy mâu thuẫn và in ra:```
yes
```Ví dụ này giải thích tại sao các giá trị trùng lặp phải được xử lý bình thường. Không có yêu cầu nào về việc bản thân các giá trị được chọn phải khác biệt, chỉ có vị trí của chúng. 

### Mẫu 2 

Đầu vào là:```
5
1 2 4 8 16
```Mẫu số đầu tiên là (A[0]=1), do đó nó tự động chuyển. Khi (k=1), mẫu số là (2) và tham chiếu là (A[0]=1). 

| (k) | (A[k]) | Giá trị tham khảo | Giá trị hiện tại | Sự khác biệt | Chia được? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 4 | 2 | vâng | 
| 0 | 1 | 2 | 8 | 6 | vâng | 
| 0 | 1 | 2 | 16 | 14 | vâng | 
| 1 | 2 | 1 | 4 | 3 | không | 

Tại (k=1), các giá trị nằm ngoài vị trí mẫu số bao gồm 1 và 4. Hiệu của chúng là (4-1=3), không chia hết cho 2. Do đó bộ ba có giá trị (1,4,2) vi phạm điều kiện. 

Thuật toán dừng ngay lập tức và in:```
no
```Dấu vết này cho thấy tại sao việc kiểm tra các loại dư lượng là đủ. Khi hai giá trị có số dư khác nhau theo mẫu số, hiệu của chúng không thể chia hết cho mẫu số đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Đối với mỗi (n) vị trí mẫu số, tối đa (n-2) vị trí khác được kiểm tra. | 
| Không gian | (O(1)) phụ trợ | Ngoài mảng đầu vào, thuật toán chỉ lưu trữ một số lượng chỉ số và giá trị không đổi. | 

Với (n\le50), thuật toán bậc hai thực hiện ít hơn 2500 lần kiểm tra mô-đun, thấp hơn nhiều so với ngân sách thời gian có sẵn. Bản thân mảng đầu vào chỉ chứa 50 số nguyên nên việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    for k in range(n):
        ref = 0 if k != 0 else 1

        for j in range(n):
            if j == k or j == ref:
                continue

            if (a[j] - a[ref]) % a[k] != 0:
                return "no\n"

    return "yes\n"

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample 1
assert run("""\
5
1 1 1 1 4
""") == "yes\n", "sample 1"

# Provided sample 2
assert run("""\
5
1 2 4 8 16
""") == "no\n", "sample 2"

# Minimum-size input, all equal
assert run("""\
3
7 7 7
""") == "yes\n", "minimum size and all equal"

# Minimum-size input with a failing triple
assert run("""\
3
1 2 3
""") == "no\n", "minimum size failure"

# Boundary values with duplicates
assert run("""\
3
1 1 100
""") == "yes\n", "maximum value with duplicate small values"

# Maximum-size input, all equal
assert run("50\n" + "100 " * 49 + "100\n") == "yes\n", \
    "maximum size and all equal"

# A nontrivial valid case
assert run("""\
4
2 2 5 5
""") == "yes\n", "equal groups"

# Catches the difference-vs-value mistake
assert run("""\
3
2 4 6
""") == "no\n", "difference is not divisible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 7 7 7`|`yes`| Kích thước tối thiểu và thực tế là chênh lệch bằng 0 có thể chia hết cho mọi mẫu số dương | 
|`3 / 1 2 3`|`no`| Mảng nhỏ nhất có thể chứa bộ ba không hợp lệ | 
|`3 / 1 1 100`|`yes`| Vị trí trùng lặp và giá trị tối đa được phép | 
| 50 bản sao của`100`|`yes`| Tối đa (n), giá trị lặp lại và hiệu suất | 
|`4 / 2 2 5 5`|`yes`| Các nhóm giá trị khác nhau trong đó mọi chênh lệch bắt buộc đều bằng 0 | 
|`3 / 2 4 6`|`no`| Khẳng định rằng tử số phải là một hiệu chứ không phải kiểm tra tính chia hết riêng lẻ | 

## Vỏ cạnh 

Mảng tối thiểu có thể có ba vị trí. Vì`1 2 3`, chỉ có một lựa chọn không có thứ tự cho ba vị trí, mặc dù biểu thức cho phép tử số có cả hai thứ tự. Lấy mẫu số 3 và tử số (1-2=-1) cho (-1/3), đây không phải là số nguyên. Thuật toán cuối cùng xem xét (k=2), so sánh các giá trị 1 và 2 theo modulo 3 và thấy rằng hiệu của chúng không chia hết cho 3. Nó in ra`no`. 

Đối với một mảng chứa tất cả các giá trị bằng nhau, chẳng hạn như`7 7 7`, mọi hiệu của tử số đều bằng không. Vì (0) chia hết cho mọi số nguyên dương nên mọi biểu thức có thể có đều là số nguyên. Thuật toán chọn bất kỳ giá trị tham chiếu nào và mọi so sánh đều tạo ra chênh lệch bằng 0, do đó, nó sẽ in`yes`. 

Mẫu số bằng 1 luôn vô hại. Ví dụ, trong`1 2 5`, bất cứ khi nào vị trí mẫu số chứa 1, mọi hiệu đều chia hết cho 1. Thuật toán không cần trường hợp đặc biệt cho trường hợp này vì phép toán modulo thông thường đã cho kết quả bằng 0 đối với mọi số nguyên modulo 1. 

Giá trị được phép lớn nhất cũng không cần xử lý đặc biệt. TRONG`1 1 100`, khi 100 là mẫu số, chỉ có hai giá trị còn lại đều bằng 1, do đó hiệu của chúng bằng 0 và điều kiện đạt. Khi 1 là mẫu số, mọi hiệu sẽ tự động chia hết cho 1. Kết quả cuối cùng là`yes`. 

Những khác biệt tiêu cực được xử lý trực tiếp. TRONG`2 4 6`, xét 2 và 4 với mẫu số 6 tạo ra (2-4=-2). Giá trị (-2) không chia hết cho 6 nên đáp án là`no`. Biểu thức Python`(-2) % 6`là 4 chứ không phải (-2), nhưng nó khác 0, đó chính là điều mà bài kiểm tra tính chia hết cần. 

Các giá trị lặp lại phải được coi là các vị trí riêng biệt, không được thu gọn thành một tập hợp. TRONG`2 2 5 5`, khi mẫu số là 5 thì hai số 2 còn lại có hiệu bằng 0. Khi mẫu số là 2 thì hai số 5 còn lại có hiệu bằng 0. Mọi bộ ba được yêu cầu đều hợp lệ, vì vậy câu trả lời là`yes`. Việc loại bỏ các bản sao trước khi kiểm tra sẽ thay đổi tập hợp các vị trí có sẵn và có thể phá hủy cấu trúc làm cho phiên bản gốc hợp lệ.
