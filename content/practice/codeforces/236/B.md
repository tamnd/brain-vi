---
title: "CF 236B - Thử thách số dễ"
description: "Chúng ta cần tính tổng số ước của mọi tích i j k, trong đó i nằm trong khoảng từ 1 đến a, j nằm trong khoảng từ 1 đến b và k nằm trong khoảng từ 1 đến c. Với mỗi bộ ba (i, j, k), chúng ta tính d(i j k), trong đó d(x) là số ước dương của x."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 236
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 146 (Div. 2)"
rating: 1300
weight: 236
solve_time_s: 105
verified: true
draft: false
---

[CF 236B - Thử thách số dễ](https://codeforces.com/problemset/problem/236/B) 

**Đánh giá:** 1300 
**Tags:** thực hiện, lý thuyết số 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta cần tính tổng số ước của mỗi tích`i * j * k`, Ở đâu`i`dao động từ`1`ĐẾN`a`,`j`dao động từ`1`ĐẾN`b`, Và`k`dao động từ`1`ĐẾN`c`. 

Đối với mỗi bộ ba`(i, j, k)`, chúng tôi đánh giá`d(i * j * k)`, Ở đâu`d(x)`có nghĩa là số ước dương của`x`. Câu trả lời cuối cùng là tổng của tất cả các số chia này, được tính theo modulo`2^30 = 1073741824`. 

Các ràng buộc đủ nhỏ để việc lặp qua tất cả các bộ ba là hoàn toàn hợp lý. Vì mỗi biến nhiều nhất là`100`, tổng số bộ ba nhiều nhất là:$$100 \times 100 \times 100 = 10^6$$Một triệu lần lặp là ổn trong Python trong giới hạn 2 giây. Thách thức thực sự là làm thế nào chúng ta tính được số chia cho mỗi sản phẩm. 

Sản phẩm tối đa có thể là:$$100 \times 100 \times 100 = 10^6$$Một thủ tục đếm số chia đơn giản để kiểm tra tất cả các số nguyên từ`1`ĐẾN`n`sẽ yêu cầu khoảng`10^6`hoạt động cho mỗi truy vấn trong trường hợp xấu nhất. Lặp lại điều đó cho một triệu sản phẩm khác sẽ bùng nổ`10^{12}`hoạt động vượt xa giới hạn. 

Quan sát quan trọng là nhiều sản phẩm lặp lại. Ví dụ,`2 * 3 * 4`Và`1 * 6 * 4`cả hai đều sản xuất`24`. Nếu chúng ta tính số chia cho`24`một lần và lưu trữ nó, mọi lần xuất hiện trong tương lai sẽ trở thành tra cứu O(1). 

Ngoài ra còn có những trường hợp phức tạp có thể lặng lẽ phá vỡ việc triển khai bất cẩn. 

Hãy xem xét đầu vào nhỏ nhất:```
1 1 1
```Sản phẩm duy nhất là`1`, Và`d(1) = 1`. Một số cách triển khai số chia vô tình trả về`0`vì`1`bởi vì họ cho rằng mọi số đều có thừa số nguyên tố. 

Một lỗi dễ xảy ra khác khi đếm các ước số bằng phép lặp căn bậc hai. Ví dụ:```
1 1 4
```Các sản phẩm là`1, 2, 3, 4`, số ước của nó là`1, 2, 2, 3`. 

Nếu chúng ta đếm các ước số bằng cách lặp đến`sqrt(n)`và thêm`2`đối với mỗi cặp ước số, chúng ta phải xử lý các bình phương hoàn hảo một cách cẩn thận. Vì`4`, cặp số chia`(2,2)`chỉ nên đóng góp một lần. Việc quên điều này sẽ tạo ra`4`thay vì`3`. 

Một cạm bẫy khác là tính lại số chia mỗi khi cùng một sản phẩm xuất hiện. Với đầu vào:```
100 100 100
```nhiều sản phẩm lặp lại nhiều. Một giải pháp không ghi nhớ vẫn thực hiện hàng triệu phép tính tốn kém về số chia và có nguy cơ bị hết thời gian. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là lặp qua mỗi bộ ba`(i, j, k)`, tính tích`x = i * j * k`, sau đó tính toán`d(x)`bằng cách kiểm tra mọi số nguyên từ`1`ĐẾN`x`. 

Điều này hoạt động hợp lý vì định nghĩa của ước số rất đơn giản: mọi phép chia số nguyên`x`đóng góp một vào số lượng. Vấn đề là quy mô. Có tới một triệu sản phẩm và mỗi phép tính ước số có thể quét tới một triệu số. Trường hợp xấu nhất trở thành đại khái:$$10^6 \times 10^6 = 10^{12}$$đó là điều vô vọng. 

Chúng ta có thể cải thiện việc đếm số chia bằng cách sử dụng thuộc tính căn bậc hai. Số chia đi theo cặp. Nếu như`d`chia rẽ`n`, sau đó`n/d`cũng chia`n`. Chúng ta chỉ cần kiểm tra những con số lên đến`sqrt(n)`. 

Ví dụ, ước số của`36`xuất hiện dưới dạng:$$(1,36), (2,18), (3,12), (4,9), (6,6)$$Chỉ kiểm tra tối đa`6`là đủ. 

Điều này làm giảm một phép tính chia từ`O(n)`ĐẾN`O(sqrt(n))`. Vì sản phẩm lớn nhất là`10^6`, mỗi số chia bây giờ có giá khoảng`1000`lần lặp lại. Trên một triệu bộ ba, điều này trở thành khoảng`10^9`hoạt động, vẫn còn quá chậm trong Python. 

Quan sát chính là số lượng sản phẩm riêng biệt bị hạn chế. Mỗi sản phẩm nằm giữa`1`Và`10^6`. Thay vì tính toán lại số chia nhiều lần, chúng tôi lưu chúng vào bộ nhớ đệm. 

Khi chúng ta gặp một tích số lần đầu tiên, chúng ta tính toán số chia của nó bằng phép lặp căn bậc hai và lưu trữ kết quả. Mỗi lần xuất hiện trong tương lai sẽ trở thành một lần tra cứu liên tục. 

Điều này biến công việc lặp đi lặp lại tốn kém thành hiệu ứng tiền xử lý một lần. Vì số chia đếm cho các số lên đến`10^6`đủ rẻ khi chỉ được thực hiện một lần cho mỗi giá trị riêng biệt, giải pháp sẽ dễ dàng vượt qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(abc × n) | O(1) | Quá chậm | 
| Tối ưu | O(abc + U√U) | O(U) | Đã chấp nhận | 

Đây,`U`là sản phẩm tối đa có thể, nhiều nhất`10^6`. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`,`b`, Và`c`. 
2. Tạo một từ điển hoặc mảng để ghi nhớ số chia. 

Cùng một sản phẩm xuất hiện nhiều lần nên việc lưu vào bộ nhớ đệm sẽ tránh được công việc lặp lại. 
3. Lặp lại tất cả các bộ ba`(i, j, k)`. 

Vì mỗi giới hạn nhiều nhất là`100`, tổng số lần lặp có thể quản lý được. 
4. Tính tích:$$x = i \times j \times k$$5. Nếu`x`chưa được xử lý trước đó, hãy tính số ước của nó. 

Lặp lại`d`từ`1`ĐẾN`⌊√x⌋`. 

Nếu như`d`chia rẽ`x`, thì: 

- thêm`2`nếu như`d`Và`x/d`khác nhau 
- thêm`1`nếu như`d*d == x`Điều này đếm chính xác các cặp số chia mà không cần đếm gấp đôi các ô vuông hoàn hảo. 
6. Lưu trữ số chia cho`x`trong bộ đệm. 
7. Thêm số ước được lưu trong bộ nhớ đệm vào câu trả lời đang chạy. 
8. Sau khi tất cả các bộ ba được xử lý, hãy in kết quả theo modulo`1073741824`. 

### Tại sao nó hoạt động 

Đối với mỗi bộ ba`(i, j, k)`, thuật toán tính toán chính xác giá trị mà bài toán yêu cầu, cụ thể là`d(i*j*k)`. 

Quy trình đếm số chia là đúng vì mọi ước số bên dưới`sqrt(n)`tương ứng với một số chia ghép ở trên`sqrt(n)`. Các hình vuông hoàn hảo đóng góp một ước số không ghép đôi ở chính căn bậc hai. 

Ghi nhớ không thay đổi tính chính xác. Nó chỉ tránh việc tính toán lại giá trị cho các sản phẩm đã thấy trước đó. Mọi tra cứu đều trả về cùng một số ước số có thể được tính toán lại theo cách thủ công. 

Vì mỗi bộ ba được xử lý chính xác một lần và đóng góp chính xác số ước của nó nên tổng cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1073741824

a, b, c = map(int, input().split())

cache = {}
answer = 0

for i in range(1, a + 1):
    for j in range(1, b + 1):
        for k in range(1, c + 1):
            x = i * j * k

            if x not in cache:
                divisors = 0
                d = 1

                while d * d <= x:
                    if x % d == 0:
                        if d * d == x:
                            divisors += 1
                        else:
                            divisors += 2
                    d += 1

                cache[x] = divisors

            answer += cache[x]

print(answer % MOD)
```Ba vòng lặp lồng nhau liệt kê mỗi bộ ba hợp lệ chính xác một lần. Vì giới hạn chỉ là`100`, một triệu lần lặp có thể chấp nhận được. 

Bộ đệm lưu trữ số chia được khóa theo giá trị sản phẩm. Đây là sự tối ưu hóa quan trọng. Nếu không có nó, số ước tương tự sẽ được tính lại nhiều lần. 

Vòng đếm số chia sử dụng phương pháp căn bậc hai. điều kiện`d * d <= x`đảm bảo chúng tôi kiểm tra tất cả các cặp số chia có thể. 

Nhánh vuông hoàn hảo rất dễ mắc sai lầm. Ví dụ, khi`x = 36`, số chia`6`cặp với chính nó. Chúng ta nên đếm nó một lần, không phải hai lần. 

Modulo chỉ được áp dụng ở cuối. Số nguyên Python không bị tràn nên tổng trung gian sẽ an toàn. Áp dụng modulo trong quá trình tích lũy cũng sẽ có tác dụng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2
```| tôi | j | k | Sản phẩm | Ước số | Tổng Chạy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 
| 1 | 1 | 2 | 2 | 2 | 3 | 
| 1 | 2 | 1 | 2 | 2 | 5 | 
| 1 | 2 | 2 | 4 | 3 | 8 | 
| 2 | 1 | 1 | 2 | 2 | 10 | 
| 2 | 1 | 2 | 4 | 3 | 13 | 
| 2 | 2 | 1 | 4 | 3 | 16 | 
| 2 | 2 | 2 | 8 | 4 | 20 | 

Câu trả lời cuối cùng:```
20
```Ví dụ này hiển thị rõ ràng các sản phẩm lặp lại. giá trị`2`xuất hiện ba lần và`4`xuất hiện ba lần. Việc ghi nhớ tránh việc tính toán lại số chia của chúng nhiều lần. 

### Ví dụ 2 

đầu vào:```
1 2 3
```| tôi | j | k | Sản phẩm | Ước số | Tổng Chạy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 
| 1 | 1 | 2 | 2 | 2 | 3 | 
| 1 | 1 | 3 | 3 | 2 | 5 | 
| 1 | 2 | 1 | 2 | 2 | 7 | 
| 1 | 2 | 2 | 4 | 3 | 10 | 
| 1 | 2 | 3 | 6 | 4 | 14 | 

Câu trả lời cuối cùng:```
14
```Dấu vết này thể hiện cả tích lặp và tích không vuông với cặp số chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(abc + U√U) | Lặp lại ba lần cộng với một phép tính chia cho mỗi sản phẩm riêng biệt | 
| Không gian | O(U) | Bộ đệm có thể lưu trữ số chia cho tất cả các sản phẩm lên đến`10^6`| 

Số bộ ba lớn nhất có thể có là`10^6`, hoàn toàn có thể quản lý được bằng Python. Bộ đệm đếm số chia ngăn chặn các phép tính tốn kém lặp đi lặp lại, giữ cho thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1073741824

def solve():
    input = sys.stdin.readline

    a, b, c = map(int, input().split())

    cache = {}
    answer = 0

    for i in range(1, a + 1):
        for j in range(1, b + 1):
            for k in range(1, c + 1):
                x = i * j * k

                if x not in cache:
                    divisors = 0
                    d = 1

                    while d * d <= x:
                        if x % d == 0:
                            if d * d == x:
                                divisors += 1
                            else:
                                divisors += 2
                        d += 1

                    cache[x] = divisors

                answer += cache[x]

    print(answer % MOD)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("2 2 2\n") == "20", "sample 1"

# minimum input
assert run("1 1 1\n") == "1", "minimum case"

# small asymmetric case
assert run("1 2 3\n") == "14", "mixed ranges"

# perfect square products appear
assert run("1 1 4\n") == "8", "perfect square divisor counting"

# all equal small values
assert run("2 2 1\n") == "8", "duplicate products"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Xử lý đúng số chia cho`1`| 
|`1 2 3`|`14`| Phạm vi hỗn hợp và sản phẩm lặp đi lặp lại | 
|`1 1 4`|`8`| Đếm số chia bình phương hoàn hảo | 
|`2 2 1`|`8`| Sản phẩm trùng lặp và tái sử dụng bộ đệm | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 1 1
```Thuật toán chỉ xử lý một bộ ba: 

| Sản phẩm | Ước số | 
| --- | --- | 
| 1 | 1 | 

Vòng chia số chạy với`d = 1`. Từ`1 * 1 == 1`, thuật toán chỉ cộng đúng một ước số thay vì hai. 

Câu trả lời cuối cùng:```
1
```Bây giờ hãy xem xét một trường hợp hình vuông hoàn hảo:```
1 1 4
```Các sản phẩm là`1, 2, 3, 4`. 

Vì`4`, vòng chia số hoạt động như sau: 

| d | d*d <= 4 | 4% d == 0 | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | Có | Có | +2 | 
| 2 | Có | Có | +1 | 

Tổng số ước sẽ trở thành`3`, tương ứng với`{1,2,4}`. 

Việc thực hiện bất cẩn luôn bổ sung thêm`2`đối với các cặp số chia sẽ trả về không chính xác`4`. 

Cuối cùng, hãy xem xét các sản phẩm lặp lại:```
2 2 2
```Sản phẩm`4`xuất hiện ba lần. Lần xuất hiện đầu tiên tính số chia của nó và lưu trữ:```
cache[4] = 3
```Hai lần xuất hiện tiếp theo ngay lập tức sử dụng lại giá trị được lưu trong bộ nhớ cache. Điều này tránh việc quét căn bậc hai lặp đi lặp lại và giữ thời gian chạy đủ nhanh cho các đầu vào lớn nhất.
