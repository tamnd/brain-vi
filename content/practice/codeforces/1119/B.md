---
title: "CF 1119B - Alyona và chiếc tủ lạnh hẹp"
description: "Tủ lạnh có chiều cao h và đúng hai cột. Kệ có thể được lắp đặt ở mọi độ cao, nghĩa là chúng ta có thể thoải mái chia tủ lạnh thành các ngăn nằm ngang với chiều cao tùy ý. Mỗi chai chiếm một cột và có chiều cao a[i]."
date: "2026-06-12T04:28:46+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "flows", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1119
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 2"
rating: 1300
weight: 1119
solve_time_s: 114
verified: true
draft: false
---

[CF 1119B - Alyona và chiếc tủ lạnh hẹp](https://codeforces.com/problemset/problem/1119/B) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, dòng chảy, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tủ lạnh có chiều cao`h`và đúng hai cột. Kệ có thể được lắp đặt ở mọi độ cao, nghĩa là chúng ta có thể thoải mái chia tủ lạnh thành các ngăn nằm ngang với chiều cao tùy ý. Mỗi chai chiếm một cột và có chiều cao`a[i]`. 

Nhiệm vụ không phải là chọn bất kỳ tập hợp con chai nào. Chúng ta phải xem xét các chai theo thứ tự và tìm chai lớn nhất`k`như vậy chai`1..k`tất cả đều có thể được đặt bên trong tủ lạnh cùng một lúc. 

Quan sát chính là mọi ngăn được tạo bởi các kệ đều có chiều rộng bằng 2. Tối đa hai chai có thể chiếm cùng một mặt phẳng thẳng đứng vì tủ lạnh chỉ có hai cột. Nếu hai chai dùng chung một ngăn thì chiều cao của ngăn ít nhất phải cao hơn chiều cao của hai chai. Vì kệ có thể được đặt ở bất cứ đâu nên chỉ có chiều cao cần thiết mới quan trọng. 

Số lượng chai nhiều nhất là`1000`. Điều này đủ nhỏ để ngay cả một`O(n² log n)`giải pháp hoàn toàn có thể chấp nhận được. Chiều cao của tủ lạnh có thể lớn tới`10^9`, nhưng giá trị độ cao chỉ được sử dụng trong so sánh số học nên giới hạn lớn không gây khó khăn gì. 

Một lỗi phổ biến là cộng chiều cao của tất cả các chai. Ví dụ:```
3 7
2 3 5
```Câu trả lời là`3`, mặc dù`2 + 3 + 5 = 10 > 7`. 

Lý do là các chai có thể đứng cạnh nhau. Chiều cao ghép nối`(5,3)`chỉ sử dụng`5`đơn vị không gian thẳng đứng và chai`2`sử dụng cái khác`2`, với tổng số`7`. 

Một sai lầm dễ mắc phải khác là ghép các chai một cách tùy tiện thay vì tối ưu.```
4 8
1 4 4 4
```Sau khi sắp xếp chúng ta có được`[1,4,4,4]`. 

Ghép nối`(1,4)`Và`(4,4)`yêu cầu`4 + 4 = 8`, phù hợp. 

Một sự sắp xếp bất cẩn như`(1)`+`(4)`+`(4,4)`yêu cầu`1 + 4 + 4 = 9`, không phù hợp. 

Việc kiểm tra tính khả thi phải luôn sử dụng chiến lược ghép đôi tốt nhất có thể. 

Trường hợp tinh vi thứ ba xảy ra khi số chai là số lẻ.```
3 6
1 2 4
```Chiều cao được sắp xếp là`[1,2,4]`. 

Sự sắp xếp tối ưu chỉ sử dụng chai lớn nhất và ghép nối hai chai còn lại. Chiều cao yêu cầu là`4 + 1 = 5`, không`4 + 2 = 6`. Xử lý số lẻ không chính xác thường tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách`k`từ`1`ĐẾN`n`và xác định xem chai`1..k`phù hợp. Để kiểm tra tính khả thi, chúng ta có thể liệt kê tất cả các cặp chai có thể ghép vào các ngăn có hai cột và tính chiều cao yêu cầu tối thiểu. Điều này rõ ràng là đúng vì nó xem xét mọi cách sắp xếp, nhưng số lượng cặp đôi tăng theo cấp số nhân và trở nên hoàn toàn không thực tế. 

Cấu trúc của tủ lạnh cho khả năng quan sát chắc chắn hơn rất nhiều. Vì mỗi tầng có chiều rộng là 2 nên mỗi ngăn đều đóng góp chiều cao tối đa trong số các chai được đặt trong đó. Để giảm thiểu tổng chiều cao sử dụng, chúng ta nên đặt những chai cao nhất vào các ngăn riêng biệt khi cần thiết và ghép những chai nhỏ hơn với chúng. 

Giả sử chúng ta muốn kiểm tra xem lần đầu tiên`k`chai phù hợp. Lấy chiều cao của chúng và sắp xếp chúng. 

Với số chai chẵn:```
b0 ≤ b1 ≤ b2 ≤ b3 ≤ ... ≤ b(k-1)
```Sự sắp xếp tối ưu ghép các chai lân cận từ trên xuống:```
(b(k-2), b(k-1))
(b(k-4), b(k-3))
...
```Chiều cao cần thiết trở thành:```
b(k-1) + b(k-3) + ...
```Đối với số chai lẻ, chai nhỏ nhất chỉ chiếm một ngăn, các chai còn lại được ghép tương tự. Chiều cao yêu cầu một lần nữa đạt được bằng cách tính tổng mọi phần tử thứ hai từ cuối mảng đã sắp xếp. 

Điều này đưa ra một thử nghiệm khả thi hiệu quả. 

Nếu lần đầu tiên`k`chai vừa khít thì bất kỳ tiền tố nhỏ hơn nào cũng phù hợp vì việc loại bỏ chai không thể khiến việc đặt chai trở nên khó khăn hơn. Tính đơn điệu đó cho phép tìm kiếm nhị phân trên`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log2 n) | O(n) | Đã chấp nhận | 

## Phương pháp tiếp cận 

Điều kiện khả thi là đơn điệu. Nếu chai`1..k`vừa vặn, sau đó đóng chai`1..(k-1)`cũng phù hợp. 

Điều đó ngay lập tức gợi ý tìm kiếm nhị phân cho câu trả lời. 

Đối với một ứng cử viên`k`, chúng tôi trích xuất đầu tiên`k`chiều cao, sắp xếp chúng và tính chiều cao tủ lạnh tối thiểu cần thiết. 

Sau khi phân loại, chiến lược tối ưu là ghép các chai sao cho chai lớn nhất trong mỗi cặp đóng góp vào tổng số. Chai lớn nhất phải đóng góp toàn bộ chiều cao của nó bất kể nó được ghép nối với thứ gì. Người đóng góp lớn thứ hai phải càng nhỏ càng tốt, điều này đạt được bằng cách ghép các chai lớn lại với nhau. 

Do đó, chiều cao yêu cầu chỉ đơn giản là tổng của các phần tử tại các vị trí:```
last, last-2, last-4, ...
```trong mảng đã sắp xếp. 

Nếu số tiền này không vượt quá`h`, tiền tố là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log2 n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm kiếm câu trả lời nhị phân`k`trong phạm vi`[1, n]`. 
2. Đối với giá trị ứng viên`k`, lấy cái đầu tiên`k`chiều cao chai. 
3. Sắp xếp các độ cao này theo thứ tự không giảm. 
4. Bắt đầu từ phần tử lớn nhất, mỗi lần di chuyển sang trái hai vị trí và cộng các chiều cao đó vào tổng hiện có. 
5. Tổng số này bằng chiều cao tủ lạnh tối thiểu cần thiết cho những`k`chai. 
6. Nếu tổng lớn nhất`h`, đánh dấu`k`khả thi và tiếp tục tìm kiếm ở bên phải. 
7. Nếu không thì tìm kiếm bên trái. 
8. Giá trị khả thi lớn nhất được tìm thấy trong quá trình tìm kiếm nhị phân là câu trả lời. 

### Tại sao nó hoạt động 

Sau khi phân loại, mỗi ngăn sẽ đóng góp chiều cao của chai cao nhất. Để giảm thiểu tổng lượng đóng góp, các chai lớn nhất nên được ghép nối với nhau. Khi đó tập hợp các chiều cao ngăn trở thành chính xác các phần tử tại các vị trí`last, last-2, last-4, ...`. 

Bất kỳ sự ghép đôi nào khác sẽ buộc một số chai lớn hơn trở thành ngăn tối đa của một ngăn bổ sung và không thể giảm tổng chiều cao cần thiết. Do đó, kiểm tra tính khả thi sẽ tính toán chiều cao tối thiểu thực sự cần thiết cho tiền tố đã chọn. 

Bởi vì tính khả thi là đơn điệu đối với`k`, tìm kiếm nhị phân sẽ tìm chính xác tiền tố hợp lệ lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_fit(k, a, h):
    b = sorted(a[:k])

    need = 0
    i = k - 1

    while i >= 0:
        need += b[i]
        i -= 2

    return need <= h

def solve():
    n, h = map(int, input().split())
    a = list(map(int, input().split()))

    lo, hi = 1, n
    ans = 1

    while lo <= hi:
        mid = (lo + hi) // 2

        if can_fit(mid, a, h):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Hàm trợ giúp thực hiện kiểm tra tính khả thi. Nó sắp xếp đầu tiên`k`chai và tính chiều cao yêu cầu tối thiểu bằng cách tính tổng mọi phần tử thứ hai từ cuối. 

Tìm kiếm nhị phân duy trì tính bất biến rằng tất cả các câu trả lời khả thi đều nằm ở phía bên trái của giá trị không khả thi đầu tiên. Khi một ứng viên làm việc, chúng tôi ghi lại và tìm kiếm tiền tố lớn hơn. Khi thất bại, chúng tôi tìm kiếm trong số các tiền tố nhỏ hơn. 

Một lỗi triển khai phổ biến là tính tổng các cặp liền kề một cách rõ ràng. Điều đó hoạt động nhưng giới thiệu logic lập chỉ mục bổ sung. Tổng hợp các phần tử tại các vị trí`k-1, k-3, k-5, ...`trực tiếp đơn giản hơn và tương ứng chính xác với chiều cao của ngăn. 

Một điểm tinh tế khác là`ans`có thể bắt đầu an toàn lúc`1`. Tuyên bố đảm bảo rằng ít nhất một chai luôn phù hợp vì chiều cao tối đa của mỗi chai là`h`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 7
2 3 5 4 1
```Kiểm tra tìm kiếm nhị phân: 

| k | Tiền tố được sắp xếp | Chiều cao tổng hợp | Chiều cao yêu cầu | Khả thi | 
| --- | --- | --- | --- | --- | 
| 3 | [2,3,5] | 5 + 2 | 7 | Có | 
| 4 | [2,3,4,5] | 5 + 3 | 8 | Không | 

Giá trị khả thi lớn nhất là`3`. 

Dấu vết này cho thấy trường hợp có kích thước lẻ. Chai lớn nhất góp phần`5`, còn hai bình còn lại gộp lại chỉ đóng góp`2`. 

### Ví dụ 2 

đầu vào:```
4 8
1 4 4 4
```| k | Tiền tố được sắp xếp | Chiều cao tổng hợp | Chiều cao yêu cầu | Khả thi | 
| --- | --- | --- | --- | --- | 
| 2 | [1,4] | 4 | Có | | 
| 3 | [1,4,4] | 4 + 1 = 5 | Có | | 
| 4 | [1,4,4,4] | 4 + 4 = 8 | Có | | 

Trả lời:```
4
```Ví dụ này chứng minh tại sao việc ghép các chai lớn lại với nhau là tối ưu. Chiều cao yêu cầu trở nên chính xác`8`, phù hợp với tủ lạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log2 n) | Tìm kiếm nhị phân thực hiện kiểm tra O(log n), mỗi kiểm tra sắp xếp tối đa n phần tử | 
| Không gian | O(n) | Mảng được sắp xếp tạm thời để kiểm tra tính khả thi | 

Với`n ≤ 1000`, công việc trong trường hợp xấu nhất là đại khái`1000 × log²(1000)`, thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    n, h = map(int, input().split())
    a = list(map(int, input().split())

    def can_fit(k):
        b = sorted(a[:k])
        need = 0
        i = k - 1
        while i >= 0:
            need += b[i]
            i -= 2
        return need <= h

    lo, hi = 1, n
    ans = 1

    while lo <= hi:
        mid = (lo + hi) // 2
        if can_fit(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

def run(inp: str) -> str:
    global input
    input = io.StringIO(inp).readline

    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old
    return out.getvalue().strip()

# provided sample
assert run("5 7\n2 3 5 4 1\n") == "3", "sample 1"

# minimum size
assert run("1 5\n5\n") == "1", "single bottle"

# all equal
assert run("4 6\n3 3 3 3\n") == "4", "all equal heights"

# odd count boundary
assert run("3 6\n1 2 4\n") == "3", "odd number of bottles"

# answer not equal to n
assert run("5 5\n5 5 5 5 5\n") == "2", "largest valid prefix"

# larger prefix barely fits
assert run("4 8\n1 4 4 4\n") == "4", "exact height match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 / 5`|`1`| Kích thước đầu vào tối thiểu | 
|`4 6 / 3 3 3 3`|`4`| Mọi chiều cao đều bằng nhau | 
|`3 6 / 1 2 4`|`3`| Xử lý tiền tố có kích thước lẻ | 
|`5 5 / 5 5 5 5 5`|`2`| Ranh giới tìm kiếm nhị phân | 
|`4 8 / 1 4 4 4`|`4`| Vừa vặn chính xác với chiều cao của tủ lạnh | 

## Vỏ cạnh 

###Số chai lẻ 

đầu vào:```
3 6
1 2 4
```Tiền tố được sắp xếp:```
[1, 2, 4]
```Thuật toán tính tổng các vị trí`2`Và`0`, cho:```
4 + 1 = 5
```Từ`5 ≤ 6`, cả ba chai đều vừa. Việc triển khai không chính xác có thể tổng hợp`4 + 2 = 6`bằng cách ghép nối không chính xác, tính năng này vẫn hoạt động ở đây nhưng không thành công trên các đầu vào tương tự. 

### Kết hợp chiều cao chính xác 

đầu vào:```
4 8
1 4 4 4
```Đã sắp xếp:```
[1, 4, 4, 4]
```Chiều cao yêu cầu:```
4 + 4 = 8
```Thuật toán chấp nhận đẳng thức vì điều kiện là`need <= h`. Sử dụng bất đẳng thức nghiêm ngặt sẽ bác bỏ trường hợp này một cách không chính xác. 

### Nhiều chai cao 

đầu vào:```
5 5
5 5 5 5 5
```Vì`k = 2`:```
need = 5
```Khả thi. 

Vì`k = 3`:```
need = 5 + 5 = 10
```Không khả thi. 

Tìm kiếm nhị phân trả về chính xác`2`. Trường hợp này xác minh rằng thuộc tính đơn điệu được giữ nguyên và việc tìm kiếm dừng ở tiền tố khả thi lớn nhất. 

###Chai nhỏ kết hợp với chai lớn 

đầu vào:```
4 7
1 1 1 7
```Đã sắp xếp:```
[1, 1, 1, 7]
```Chiều cao yêu cầu:```
7 + 1 = 8
```Câu trả lời không tự động`4`chỉ vì ba chai rất nhỏ. Chai cao nhất chiếm ưu thế trong ngăn của nó và thuật toán nắm bắt điều đó bằng cách tính tổng mọi phần tử thứ hai từ cuối.
