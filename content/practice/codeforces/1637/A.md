---
title: "CF 1637A - Phân loại linh kiện"
description: "Chúng tôi có một mảng và chúng tôi buộc phải thực hiện chính xác một thao tác. Chúng ta chọn vị trí chia mảng thành hai phần không trống. Sau đó chúng ta sắp xếp phần bên trái và phần bên phải một cách độc lập."
date: "2026-06-10T04:34:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1637
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 19"
rating: 800
weight: 1637
solve_time_s: 83
verified: true
draft: false
---

[CF 1637A - Phân loại các bộ phận](https://codeforces.com/problemset/problem/1637/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, phân loại 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng và chúng tôi buộc phải thực hiện chính xác một thao tác. Chúng ta chọn vị trí chia mảng thành hai phần không trống. Sau đó chúng ta sắp xếp phần bên trái và phần bên phải một cách độc lập. Câu hỏi đặt ra là liệu có tồn tại sự phân chia nào đó sao cho toàn bộ mảng cuối cùng vẫn chưa được sắp xếp hay không. 

Với mỗi test case, chúng ta phải trả lời`"YES"`nếu ít nhất một phần tách tạo ra một mảng chưa được sắp xếp và`"NO"`nếu mọi sự phân chia có thể tạo ra một mảng được sắp xếp trên toàn cầu. 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm chỉ là$10^4$, do đó, ngay cả các thuật toán bậc hai cũng có thể nằm gọn trong giới hạn. Điều đó có nghĩa là hiệu suất không thực sự là thách thức ở đây. Khó khăn nằm ở việc nhận ra cấu trúc ẩn giấu bên trong hoạt động. 

Một sai lầm dễ mắc phải là nghĩ rằng việc sắp xếp cả hai phần riêng biệt luôn tạo ra một mảng được sắp xếp. Coi như```
3
2 2 1
```Việc chọn phần tách sau phần tử đầu tiên sẽ cho```
[2] + [2,1]
```Sau khi sắp xếp từng bên:```
[2] + [1,2]
```trở thành```
[2,1,2]
```và không được sắp xếp. Câu trả lời đúng là`"YES"`. 

Một tình huống khác có thể đánh lừa một giải pháp bất cẩn là một mảng đã được sắp xếp:```
5
1 2 2 4 4
```Bất kể chúng ta chia ở đâu, phần tử lớn nhất ở bên trái không bao giờ vượt quá phần tử nhỏ nhất ở bên phải. Mọi mảng kết quả vẫn được sắp xếp, vì vậy câu trả lời là`"NO"`. 

Bản sao cũng đáng được chú ý. Ví dụ,```
4
1 3 3 2
```Việc chia tách sau phần tử thứ ba sẽ cho```
[1,3,3] + [2]
```Sau khi sắp xếp, không có gì thay đổi:```
[1,3,3,2]
```vẫn chưa được sắp xếp. Các giá trị bằng nhau là vô hại, chỉ có sự đảo ngược nghiêm ngặt mới quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi cách phân chia có thể. Đối với mỗi vị trí, chúng tôi sắp xếp tiền tố và hậu tố một cách độc lập, nối chúng và kiểm tra xem toàn bộ mảng có được sắp xếp hay không. có$n-1$các điểm phân chia có thể có và mỗi lần thử thực hiện hai loại và một kiểm tra tuyến tính. Trường hợp xấu nhất là đại khái$O(n^2 \log n)$. Với$n\le10^4$, điều này vẫn sẽ vượt qua, nhưng nó thực hiện nhiều công việc hơn mức cần thiết. 

Lý do mà vũ lực có tác dụng là sau khi sắp xếp từng mảnh, nơi duy nhất có thể xảy ra tình trạng hỗn loạn là ở ranh giới giữa hai mảnh. Bên trong mỗi phần, mọi thứ đã được sắp xếp sẵn. 

Giả sử chúng ta chia theo vị trí$i$. Sau khi sắp xếp, phần bên trái kết thúc bằng phần tử tối đa và phần bên phải bắt đầu bằng phần tử tối thiểu. Toàn bộ mảng được sắp xếp chính xác khi$$\max(\text{left}) \le \min(\text{right})$$Nếu thay vào đó$$\max(\text{left}) > \min(\text{right}),$$thì hai phần tử biên này tạo ra sự đảo ngược và mảng cuối cùng không được sắp xếp. 

Bây giờ hãy quan sát điều gì đó thậm chí còn đơn giản hơn. Nếu mảng ban đầu chứa bất kỳ sự đảo ngược nào$a_i>a_j$với$i<j$, sau đó việc phân chia giữa các vị trí đó sẽ đặt giá trị lớn hơn ở đâu đó ở nửa bên trái và giá trị nhỏ hơn ở đâu đó ở nửa bên phải. Sau khi sắp xếp nội bộ, nửa bên trái vẫn chứa giá trị lớn hơn đó như một phần của mức tối đa và nửa bên phải vẫn chứa giá trị nhỏ hơn như một phần của mức tối thiểu, do đó ranh giới vẫn bị đảo ngược. 

Điều đó có nghĩa là câu trả lời là`"YES"`chính xác khi mảng ban đầu không được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Quét các cặp liền kề từ trái qua phải vì mảng không giảm phải thỏa mãn$a_i\le a_{i+1}$ở khắp mọi nơi. 
3. Nếu một số cặp liền kề thỏa mãn$a_i>a_{i+1}$, mảng đã chứa đảo ngược. đầu ra`"YES"`. 

Một phép đảo ngược liền kề là đủ vì việc chọn phần phân chia giữa hai phần tử đó làm cho cực đại bên trái lớn hơn cực tiểu bên phải. 

1. Nếu không tồn tại cặp như vậy thì mảng đã được sắp xếp theo thứ tự không giảm. đầu ra`"NO"`. 

### Tại sao nó hoạt động 

Mảng được sắp xếp vẫn được sắp xếp sau khi sắp xếp bất kỳ tiền tố và hậu tố nào một cách độc lập. Ngược lại, nếu mảng ban đầu chứa đảo ngược thì cũng phải tồn tại đảo ngược liền kề. Việc phân chia giữa hai phần tử đó để lại giá trị lớn hơn ở nửa bên trái và giá trị nhỏ hơn ở nửa bên phải. Việc sắp xếp từng bên không thể loại bỏ sự đảo ngược xuyên biên giới này. Do đó hoạt động tạo ra một mảng chưa được sắp xếp. Hai sự thật này hoàn toàn mô tả câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    ok = False
    for i in range(n - 1):
        if a[i] > a[i + 1]:
            ok = True
            break

    print("YES" if ok else "NO")
```Vòng lặp trên các phần tử liền kề là đủ vì mọi mảng chưa được sắp xếp đều chứa ít nhất một phép đảo ngược liền kề. Khi tìm thấy cặp như vậy, chúng ta có thể dừng ngay lập tức vì chỉ cần một nhân chứng là đủ. 

Việc triển khai tránh việc sắp xếp không cần thiết và chỉ sử dụng bộ nhớ bổ sung không đổi. Điều kiện biên rất đơn giản vì vòng lặp chạy từ`0`ĐẾN`n-2`, làm`a[i+1]`luôn luôn hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu```
3
2 2 1
```| tôi | một [tôi] | a[i+1] | a[i] > a[i+1]? | được | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | Không | Sai | 
| 1 | 2 | 1 | Có | Đúng | 

Thuật toán tìm phép nghịch đảo liền kề ở vị trí 2 và 3 nên đáp án là`"YES"`. 

Ví dụ này cho thấy rằng mặc dù hầu hết mảng đều được sắp xếp theo thứ tự nhưng chỉ cần đảo ngược một lần là đủ. 

Coi như```
5
1 2 2 4 4
```| tôi | một [tôi] | a[i+1] | a[i] > a[i+1]? | được | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | Không | Sai | 
| 1 | 2 | 2 | Không | Sai | 
| 2 | 2 | 4 | Không | Sai | 
| 3 | 4 | 4 | Không | Sai | 

Không có sự đảo ngược liền kề nào xuất hiện nên câu trả lời là`"NO"`. 

Điều này xác nhận rằng các giá trị trùng lặp không gây rắc rối. Chỉ có một mức giảm nghiêm ngặt mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua mảng | 
| Không gian | O(1) | Chỉ một biến boolean được sử dụng | 

Vì tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$10^4$, nghiệm tuyến tính chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ok = False
        for i in range(n - 1):
            if a[i] > a[i + 1]:
                ok = True
                break

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run(
"""3
3
2 2 1
4
3 1 2 1
5
1 2 2 4 4
"""
) == """YES
YES
NO"""

# minimum size, increasing
assert run(
"""1
2
1 2
"""
) == "NO", "already sorted"

# minimum size, decreasing
assert run(
"""1
2
2 1
"""
) == "YES", "single inversion"

# all equal values
assert run(
"""1
5
7 7 7 7 7
"""
) == "NO", "duplicates are allowed"

# inversion near the end
assert run(
"""1
5
1 2 3 5 4
"""
) == "YES", "boundary inversion"

# maximum-size style case
assert run(
"1\n10000\n" + " ".join(map(str, range(1, 10001))) + "\n"
) == "NO", "large sorted array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,2]`| KHÔNG | Kích thước tối thiểu, đã được sắp xếp | 
|`[2,1]`| CÓ | Kích thước tối thiểu có đảo ngược | 
|`[7,7,7,7,7]`| KHÔNG | Giá trị bằng nhau là hợp lệ | 
|`[1,2,3,5,4]`| CÓ | Đảo ngược gần ranh giới | 
|`1,2,...,10000`| KHÔNG | Hiệu suất đầu vào lớn | 

## Vỏ cạnh 

Hãy xem xét```
1
2
2 1
```Quá trình quét sẽ kiểm tra cặp liền kề duy nhất và tìm thấy`2 > 1`. Đầu ra của thuật toán`"YES"`. Sự phân chia giữa hai phần tử tạo ra`[2] + [1]`, vẫn chưa được sắp xếp. 

Coi như```
1
5
1 2 2 4 4
```Mọi cặp liền kề đều thỏa mãn điều kiện không giảm. Thuật toán đi đến cuối cùng mà không tìm thấy sự đảo ngược và xuất ra`"NO"`. Bất kỳ sự phân chia nào cũng duy trì trật tự toàn cầu vì phần tử tối đa ở bên trái không bao giờ vượt quá phần tử tối thiểu ở bên phải. 

Coi như```
1
4
1 3 3 2
```Quá trình quét đạt đến cặp`(3,2)`và phát hiện sự đảo ngược. Câu trả lời là`"YES"`. Chia sau vị trí thứ ba cho```
[1,3,3] + [2]
```Việc sắp xếp từng phần không thay đổi gì và mảng cuối cùng vẫn giữ nguyên```
[1,3,3,2]
```không được sắp xếp. Ví dụ này cho thấy các giá trị trùng lặp không ảnh hưởng đến đối số. Chỉ có một mức giảm nghiêm ngặt mới quan trọng.
