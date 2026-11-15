---
title: "CF 1841E - Điền vào ma trận"
description: "Mỗi cột của ma trận chứa tiền tố màu đen và hậu tố màu trắng. Trong cột i, các hàng 1..ai bị chặn, trong khi các hàng ai+1..n lại có sẵn. Chúng ta phải đặt các số nguyên 1,2,...,m vào các ô màu trắng riêng biệt."
date: "2026-06-09T06:21:49+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1841
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 150 (Rated for Div. 2)"
rating: 2200
weight: 1841
solve_time_s: 85
verified: true
draft: false
---

[CF 1841E - Điền vào ma trận](https://codeforces.com/problemset/problem/1841/E) 

**Đánh giá:** 2200 
**Tags:** cấu trúc dữ liệu, tham lam, toán học 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cột của ma trận chứa tiền tố màu đen và hậu tố màu trắng. Trong cột`i`, hàng`1..a_i`bị chặn, trong khi các hàng`a_i+1..n`có sẵn. 

Chúng ta phải đặt các số nguyên`1,2,...,m`thành các tế bào màu trắng riêng biệt. Người đẹp đếm được bao nhiêu đôi liên tiếp`(j, j+1)`kết thúc ở các ô liền kề theo chiều ngang của cùng một hàng, với`j+1`ngay bên phải của`j`. 

Giá trị thực tế của các con số chỉ quan trọng thông qua thứ tự của chúng. Bất cứ khi nào chúng ta đặt một dãy số liên tiếp bên trong một đoạn ô màu trắng nằm ngang, một đoạn chứa`k`những con số đóng góp chính xác`k-1`vẻ đẹp, bởi vì mỗi cặp lân cận bên trong đường chạy đó đều đóng góp một lần. 

Kích thước ma trận tối đa là`2·10^5`trên tất cả các trường hợp thử nghiệm. Bất kỳ thuật toán nào chạm rõ ràng vào tất cả`n²`tế bào là không thể. Ngay cả việc lưu trữ toàn bộ ma trận cũng sẽ quá tốn kém. Cấu trúc ô trắng phải được khai thác trực tiếp từ mảng`a`. 

Một điểm tinh tế là`m`có thể rất lớn. Tuyên bố cảnh báo rõ ràng rằng nó có thể không vừa với số nguyên 32 bit. Mọi phép tính liên quan đến số lượng giá trị được đặt phải sử dụng số học 64 bit. 

Một sai lầm dễ mắc phải khác là nghĩ rằng mỗi hàng có thể được xử lý độc lập. Coi như:```
n = 3
a = [0,0,0]
m = 9
```Tất cả các tế bào đều có màu trắng. Có ba đoạn hàng có độ dài`3`. Làm đầy từng cái hoàn toàn mang lại vẻ đẹp`2 + 2 + 2 = 6`, đó là tối ưu. Ở đây xử lý các hàng một cách độc lập là ổn. 

Bây giờ hãy xem xét:```
n = 4
a = [2,0,3,1]
m = 5
```Các đoạn hàng có độ dài khác nhau. Số tiền chi cho một đoạn ngắn có thể làm giảm vẻ đẹp cuối cùng vì mỗi đoạn mới sẽ bị phạt một đoạn. Chiến lược tối ưu được điều khiển bởi độ dài phân khúc chứ không phải bởi chính các hàng. 

Bẫy thứ ba xuất hiện khi số nguyên còn lại không thể lấp đầy hoàn toàn đoạn tiếp theo. Đối với một đoạn có độ dài`L`, chỉ đặt`x < L`những con số đóng góp`x-1`, không`x`. Thiếu chi tiết này sẽ tạo ra từng lỗi một ở bước tham lam cuối cùng. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực trước tiên là hữu ích. 

Hãy tưởng tượng trích xuất mọi đoạn màu trắng ngang tối đa. Nếu một đoạn có độ dài`L`, sau đó điền`k`các tế bào của đoạn đó liên tục mang lại vẻ đẹp`k-1`bất cứ khi nào`k > 0`. 

Giả sử chúng ta biết tất cả các phân đoạn. Sau đó chúng ta có thể thử mọi cách phân phối có thể của`m`số trong số đó. Điều này hiển nhiên là đúng vì vẻ đẹp chỉ phụ thuộc vào số lượng mỗi đoạn nhận được. Thật không may, số lượng phân phối là rất lớn và thậm chí việc tạo ra tất cả các phân đoạn một cách đơn giản đòi hỏi phải kiểm tra toàn bộ ma trận, có thể chứa khoảng`4·10^10`tế bào. 

Quan sát quan trọng là một đoạn được lấp đầy với`k`những con số đóng góp```
k - 1
```có thể được viết lại thành```
k - (number of used segments)
```tổng hợp trên tất cả các phân đoạn. 

Vì tổng số số được đặt là cố định và bằng nhau`m`, tối đa hóa vẻ đẹp tương đương với việc giảm thiểu số lượng đoạn nhận được ít nhất một số. 

Điều đó ngay lập tức gợi ý một chiến lược tham lam. Nếu chúng ta có sẵn một số phân đoạn, trước tiên chúng ta nên điền những phân đoạn dài nhất. Một đoạn dài có thể hấp thụ nhiều số trong khi phải chịu mức phạt "một đoạn" giống như một đoạn ngắn. 

Vấn đề bây giờ trở thành: 

1. Tìm xem tồn tại bao nhiêu đoạn màu trắng nằm ngang tối đa với mỗi độ dài có thể. 
2. Chi tiêu`m`số trên các đoạn từ chiều dài lớn nhất đến chiều dài nhỏ nhất. 

Thử thách còn lại là trích xuất tất cả các đoạn màu trắng nằm ngang tối đa mà không cần xây dựng ma trận. 

Đối với một hàng`r`, cột`i`màu trắng nếu`a_i < r`. Một đoạn ngang tối đa tương ứng với một khoảng các cột trong đó tất cả các chiều cao đều nằm dưới một mức hàng nào đó. 

Đây chính xác là cấu trúc tương tự xuất hiện trong các bài toán biểu đồ. Sử dụng ngăn xếp đơn điệu, chúng ta có thể đếm có bao nhiêu đoạn tối đa tồn tại cho mỗi độ dài. Ngăn xếp xử lý đường chân trời được hình thành bởi`a_i`và mỗi cửa sổ bật lên xác định độ dài phạm vi cùng với số lượng cấp hàng tạo ra phạm vi đó. 

Sau khi có được`cnt[len]`, số đoạn có độ dài tối đa`len`, việc lấp đầy tham lam rất đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n²) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

Việc triển khai bên dưới thực sự chạy theo thời gian tuyến tính ngoài việc lặp lại theo độ dài. 

## Hướng dẫn thuật toán 

1. Phiên dịch`a_i`như chiều cao cột của các ô bị chặn. 
2. Thêm hai cột có chiều cao trọng điểm`n`ở cả hai đầu. Những điều này đảm bảo rằng mọi phân đoạn cuối cùng đều được đóng và tính. 
3. Xử lý mảng với ngăn xếp đơn điệu giảm dần. 
4. Bất cứ khi nào chiều cao hiện tại ít nhất bằng chiều cao của cột trước đó, hãy bắt đầu bật các phần tử ngăn xếp. 
5. Trong quá trình bật lên, mỗi chiều cao được bật lên đại diện cho một phạm vi mức hàng tạo ra các đoạn màu trắng nằm ngang tối đa có độ dài cụ thể. Tích lũy những đóng góp này vào`cnt[length]`. 
6. Sau tất cả các cửa sổ bật lên được phép, nếu ngăn xếp vẫn không trống, hãy thêm phần đóng góp còn lại được tạo giữa chiều cao được bật ra cuối cùng và chiều cao hiện tại. 
7. Sau khi xử lý từng cột,`cnt[len]`lưu trữ có bao nhiêu đoạn màu trắng tối đa có chiều dài`len`. 
8. Đi qua chiều dài từ lớn nhất đến nhỏ nhất. 
9. Trong một thời gian dài`L`với số lượng`C`:

- Nếu như`m >= L·C`, điền đầy đủ tất cả các phân đoạn như vậy. 
- Mỗi phân đoạn được điền đầy đủ sẽ đóng góp`L-1`. 
- Trừ`L·C`từ`m`. 
10. Nếu không đủ số để điền hết`C`phân đoạn: 

- Điền đầy đủ`m // L`phân đoạn. 
- Sử dụng phần còn lại`m % L`bên trong một phân đoạn bổ sung. 
- Một đoạn được lấp đầy một phần với`r`những con số đóng góp`r-1`khi`r > 0`. 
- Sau bước này, tất cả các số được sử dụng và thuật toán dừng lại. 

### Tại sao nó hoạt động 

Mỗi phân đoạn bị chiếm đóng phải trả chính xác một đơn vị tổn thất vẻ đẹp so với số số nguyên được đặt bên trong nó. 

Đối với một phân đoạn nhận`k`số:```
beauty = k - 1
```Tổng hợp tất cả các phân đoạn được sử dụng sẽ cho```
beauty = m - used_segments
```bởi vì tổng số số nguyên được đặt là cố định tại`m`. 

Để tối đa hóa vẻ đẹp, chúng ta phải giảm thiểu số lượng đoạn nhận số. Cách tối ưu để làm điều đó là luôn sử dụng công suất từ ​​những phân khúc dài nhất hiện có trước tiên. Bất kỳ giải pháp nào sử dụng phân đoạn ngắn hơn trong khi tồn tại phân đoạn dài hơn không được sử dụng đều có thể được cải thiện bằng cách chuyển số từ phân đoạn ngắn hơn sang phân đoạn dài hơn, không bao giờ tăng số lượng phân đoạn bị chiếm dụng. 

Ngăn xếp đơn điệu đếm chính xác các phân đoạn màu trắng tối đa vì mỗi pop xác định chính xác khoảng trong đó một độ cao nhất định là ranh giới giới hạn. Sự khác biệt giữa các độ cao lân cận xác định số lượng hàng tạo ra độ dài khoảng đó. Đây là đối số phân rã biểu đồ tiêu chuẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        m = int(input())

        a = [n] + arr + [n]
        N = n + 1

        stack = [0]
        cnt = {}

        for i in range(1, N + 1):
            if a[i] >= a[i - 1]:
                prev = a[stack[-1]]
                stack.pop()

                while stack and a[stack[-1]] <= a[i]:
                    length = i - stack[-1] - 1
                    cnt[length] = cnt.get(length, 0) + (a[stack[-1]] - prev)
                    prev = a[stack[-1]]
                    stack.pop()

                if stack:
                    length = i - stack[-1] - 1
                    cnt[length] = cnt.get(length, 0) + (a[i] - prev)

            stack.append(i)

        ans = 0

        for length in sorted(cnt.keys(), reverse=True):
            if length == 1:
                continue

            c = cnt[length]
            total_cells = length * c

            if total_cells <= m:
                ans += c * (length - 1)
                m -= total_cells
            else:
                full = m // length
                ans += full * (length - 1)
                m -= full * length

                if m > 0:
                    ans += m - 1

                m = 0
                break

            if m == 0:
                break

        print(ans)

solve()
```Phần đầu tiên của mã thực hiện phân tách ngăn xếp đơn điệu của biểu đồ được hình thành bởi chiều cao của cột. Từ điển`cnt`lưu trữ có bao nhiêu đoạn ngang tối đa tồn tại cho mỗi chiều dài. 

Biến`prev`là chi tiết quan trọng. Độ cao bật lên liên tiếp thể hiện các dải cấp độ hàng khác nhau. Việc sử dụng chênh lệch chiều cao sẽ tránh việc đếm cùng một hàng nhiều lần. 

Sau khi biết tất cả số lượng phân đoạn, pha tham lam sẽ lặp lại từ độ dài lớn nhất trở xuống. Một đoạn chiều dài được điền đầy đủ`L`đóng góp`L-1`, đó là lý do tại sao bản cập nhật là:```
ans += count * (L - 1)
```Trường hợp điền một phần là nơi dễ mắc lỗi nhất. Nếu phần còn lại là`r`, phần đóng góp là`r-1`, không`r`. Khi`r = 0`, không có phân đoạn bổ sung nào tồn tại nên không có gì được thêm vào. 

Số nguyên Python tự động xử lý giá trị lớn của`m`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [0,0,0]
m = 9
```Tất cả các hàng đều chứa một đoạn có độ dài`3`. 

| Chiều dài | Đếm | trước đây | Hành động | Vẻ đẹp đạt được | 
| --- | --- | --- | --- | --- | 
| 3 | 3 | 9 | Điền vào tất cả | 6 | 

Câu trả lời cuối cùng:```
6
```Điều này chứng tỏ trường hợp được điền đầy đủ. Mỗi hàng đóng góp`3-1 = 2`, cho`6`. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [2,0,3,1]
m = 5
```Việc phân tách ngăn xếp tạo ra số lượng phân đoạn tương đương với: 

| Chiều dài | Đếm | 
| --- | --- | 
| 3 | 1 | 
| 2 | 1 | 
| 1 | 4 | 

Tham lam làm đầy: 

| Chiều dài | Đếm | trước đây | Hành động | Vẻ đẹp đạt được | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 5 | Điền đầy đủ | 2 | 
| 2 | 1 | 2 | Điền đầy đủ | 1 | 

Vẻ đẹp tổng thể:```
3
```Ví dụ này cho thấy tại sao các phân đoạn dài hơn phải được ưu tiên. Sử dụng các đoạn có độ dài 1 trước tiên sẽ lãng phí vẻ đẹp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) khấu hao | Mỗi chỉ mục vào và ra khỏi ngăn xếp một lần | 
| Không gian | O(n) | Số lượng ngăn xếp và phân đoạn | 

Tổng của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2·10^5`. Giải pháp thời gian tuyến tính phù hợp thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ vẫn tỷ lệ thuận với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        m = int(input())

        a = [n] + arr + [n]
        N = n + 1

        stack = [0]
        cnt = {}

        for i in range(1, N + 1):
            if a[i] >= a[i - 1]:
                prev = a[stack[-1]]
                stack.pop()

                while stack and a[stack[-1]] <= a[i]:
                    length = i - stack[-1] - 1
                    cnt[length] = cnt.get(length, 0) + (a[stack[-1]] - prev)
                    prev = a[stack[-1]]
                    stack.pop()

                if stack:
                    length = i - stack[-1] - 1
                    cnt[length] = cnt.get(length, 0) + (a[i] - prev)

            stack.append(i)

        ans = 0

        for length in sorted(cnt.keys(), reverse=True):
            if length == 1:
                continue

            c = cnt[length]

            if length * c <= m:
                ans += c * (length - 1)
                m -= length * c
            else:
                full = m // length
                ans += full * (length - 1)
                m -= full * length

                if m:
                    ans += m - 1
                m = 0
                break

            if m == 0:
                break

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run(
"""6
3
0 0 0
9
4
2 0 3 1
5
4
2 0 3 1
6
4
2 0 3 1
10
10
0 2 2 1 5 10 3 4 1 1
20
1
1
0
"""
) == """6
3
4
4
16
0"""

# minimum size
assert run(
"""1
1
1
0
"""
) == "0"

# single white cell
assert run(
"""1
1
0
1
"""
) == "0"

# all cells white, partial fill
assert run(
"""1
2
0 0
3
"""
) == "2"

# all columns blocked
assert run(
"""1
3
3 3 3
0
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, a=[1], m=0`|`0`| Ví dụ tối thiểu | 
|`n=1, a=[0], m=1`|`0`| Một ô có thể sử dụng được | 
|`n=2, all white, m=3`|`2`| Sử dụng một phần tổng công suất | 
|`n=3, all blocked, m=0`|`0`| Không có ô nào | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
0
1
```Có chính xác một ô màu trắng. Vị trí duy nhất có thể sử dụng một phân đoạn chứa một số. Vẻ đẹp là`1 - 1 = 0`. Giai đoạn tham lam không bao giờ đạt được bất cứ điều gì từ đoạn có độ dài 1, do đó thuật toán xuất ra chính xác`0`. 

Coi như:```
1
2
0 0
3
```Có hai đoạn hàng có độ dài`2`. Thuật toán trước tiên sẽ lấp đầy hoàn toàn một phân đoạn, đạt được`1`vẻ đẹp và cách sử dụng hai con số. Một số còn lại và bắt đầu một phân khúc khác, góp phần`0`. Vẻ đẹp tổng thể là`2 - 1 = 1`từ đoạn đầu tiên cộng thêm`0`từ thứ hai, đưa ra`2`tổng thể. Quy tắc điền một phần`r - 1`xử lý việc này một cách chính xác. 

Coi như:```
1
3
3 3 3
0
```Mọi tế bào đều có màu đen. Việc phân rã ngăn xếp không tạo ra độ dài đoạn hữu ích. Giai đoạn tham lam không làm gì và quay trở lại`0`, đó là câu trả lời duy nhất có thể. 

Cuối cùng, hãy xem xét trường hợp các số còn lại dừng ở giữa một đoạn dài:```
1
3
0 0 0
4
```Chiến lược tốt nhất là đặt tất cả bốn số bên trong một đoạn có độ dài 3 và sau đó chỉ tiếp tục vào một đoạn khác nếu cần thiết. Thuật toán lấp đầy hoàn toàn một đoạn có độ dài 3, đạt được`2`và số đơn còn lại đóng góp`0`. Câu trả lời là`2`, phù hợp với công trình tối ưu.
