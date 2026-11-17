---
title: "CF 1983E - Tôi Yêu Bóng"
description: "Có hai loại bóng. K quả bóng đầu tiên là đặc biệt, n-k còn lại là bình thường. Mỗi quả bóng đều có một giá trị. Các quả bóng được loại bỏ từng cái một theo thứ tự ngẫu nhiên thống nhất. Người chơi hiện tại lấy quả bóng đã chọn và cộng giá trị của nó vào điểm của họ."
date: "2026-06-08T16:38:36+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1983
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 956 (Div. 2) and ByteRace 2024"
rating: 2300
weight: 1983
solve_time_s: 200
verified: true
draft: false
---

[CF 1983E - Tôi yêu những quả bóng](https://codeforces.com/problemset/problem/1983/E) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, toán, xác suất 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có hai loại bóng. đầu tiên`k`những quả bóng là đặc biệt, còn lại`n-k`là bình thường. Mỗi quả bóng đều có một giá trị. 

Các quả bóng được loại bỏ từng cái một theo thứ tự ngẫu nhiên thống nhất. Người chơi hiện tại lấy quả bóng đã chọn và cộng giá trị của nó vào điểm của họ. Quy luật thú vị là điều gì sẽ xảy ra tiếp theo. 

Nếu quả bóng được chọn là quả bóng bình thường thì lượt chơi sẽ chuyển cho người chơi khác. 

Nếu quả bóng được chọn là đặc biệt, người chơi đó sẽ ngay lập tức chơi lại. 

Alice bắt đầu. 

Chúng ta cần điểm cuối cùng dự kiến ​​của Alice và điểm cuối cùng dự kiến ​​của Bob. Vì câu trả lời là một số hữu tỉ nên chúng ta xuất nó theo modulo`10^9+7`sử dụng nghịch đảo mô-đun. 

Tổng số quả bóng trong tất cả các trường hợp thử nghiệm nhiều nhất là`5 · 10^5`. Bất kỳ thuật toán nào cố gắng liệt kê các hoán vị, trạng thái trò chơi hoặc phân bố xác suất trên các tập hợp con đều vô vọng. Thậm chí`O(n^2)`mỗi trường hợp thử nghiệm sẽ quá đắt trong trường hợp xấu nhất. Chúng ta cần một cái gì đó về cơ bản là tuyến tính về số lượng quả bóng. 

Mối nguy hiểm chính là giả định rằng mọi quả bóng đều có xác suất`1/2`thuộc về Alice. Điều đó là sai vì lượt chơi chỉ thay đổi khi một quả bóng thông thường được thực hiện. 

Coi như:```
n = 2, k = 1
special value = 10
ordinary value = 20
```Hai lệnh có thể xảy ra là:```
S N
```Alice nhận được cả hai quả bóng.```
N S
```Alice chỉ nhận được quả bóng bình thường. 

Số điểm mong đợi của Alice là`25`, không`(10+20)/2 = 15`. 

Một sai lầm dễ mắc phải khác là xử lý các quả bóng đặc biệt và bóng bình thường giống hệt nhau. Xác suất sở hữu của họ là khác nhau. 

Ví dụ:```
n = 3, k = 1
values = [100, 1, 1]
```Quả bóng đặc biệt có nhiều khả năng thuộc về Alice hơn quả bóng thông thường. Bất kỳ giải pháp nào tính toán một xác suất duy nhất cho mỗi quả bóng sẽ tạo ra kỳ vọng sai. 

Vụ án`k = n`cũng đặc biệt. Không có quả bóng thông thường nào nên lượt không bao giờ thay đổi và Alice nhận được mọi quả bóng với xác suất`1`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ tạo ra mọi thứ tự có thể có của các quả bóng. Đối với mỗi hoán vị, chúng ta có thể mô phỏng trò chơi, tính điểm kết quả và tính trung bình cộng của tất cả các hoán vị. 

Điều này đúng vì mọi lệnh loại bỏ đều có khả năng xảy ra như nhau. Vấn đề là kích thước của không gian trạng thái. có`n!`các đơn đặt hàng có thể. Ngay cả đối với`n = 15`, đó đã là hơn một nghìn tỷ hoán vị. 

Quan sát quan trọng là giá trị kỳ vọng là tuyến tính. 

Thay vì yêu cầu phân bổ toàn bộ điểm số, chúng ta có thể hỏi một câu hỏi nhỏ hơn nhiều: 

"Xác suất để một quả bóng cụ thể thuộc về Alice là bao nhiêu?" 

Nếu chúng ta biết xác suất đó thì quả bóng góp phần```
value × probability
```với số điểm mong đợi của Alice. 

Tất cả các quả bóng đặc biệt đều đối xứng. Tất cả các quả bóng thông thường đều đối xứng. Vì vậy, chúng ta chỉ cần hai xác suất:```
pS = probability a specific special ball belongs to Alice
pN = probability a specific ordinary ball belongs to Alice
```Một khi những điều này đã được biết,```
Alice =
pS × (sum of special values)
+
pN × (sum of ordinary values)
```và Bob nhận được giá trị kỳ vọng còn lại. 

Nhiệm vụ còn lại hoàn toàn là tổ hợp. 

Sự đơn giản hóa quan trọng là các quả bóng đặc biệt không bao giờ thay đổi lượt của nó. Chỉ những quả bóng thông thường mới gây ra sự luân phiên. Nếu chúng ta chỉ nhìn vào chuỗi các quả bóng thông thường, Alice sở hữu quả thứ nhất, Bob sở hữu quả thứ 2, Alice sở hữu quả thứ 3, v.v. 

Những quả bóng đặc biệt chỉ đơn thuần được nhét vào những khoảng trống do những quả bóng thông thường đó tạo ra. 

Điều này biến toàn bộ trò chơi thành việc đếm khoảng cách và thứ hạng, dẫn đến giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n! · n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`mỗi trường hợp thử nghiệm |`O(1)`thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để```
m = n - k
```là số quả bóng thông thường. 

### Xác suất của một quả bóng đặc biệt 

Hãy tưởng tượng loại bỏ tất cả các quả bóng đặc biệt và chỉ giữ lại`m`những quả bóng thông thường. 

Họ tạo ra chính xác`m + 1`khoảng trống:```
before first ordinary
between ordinary 1 and 2
between ordinary 2 and 3
...
after last ordinary
```Danh tính của người chơi thay đổi bất cứ khi nào một quả bóng bình thường được lấy. 

Vì vậy, quyền sở hữu các khoảng trống sẽ thay thế:```
Alice, Bob, Alice, Bob, ...
```Lấy một quả bóng đặc biệt. 

Vị trí tương đối của nó giữa các quả bóng thông thường chỉ được xác định bởi số lượng quả bóng bình thường xuất hiện trước nó. Con số đó có thể là bất kỳ giá trị nào từ`0`ĐẾN`m`. 

Theo tính đối xứng, tất cả`m + 1`các khả năng đều có khả năng xảy ra như nhau. 

Do đó quả bóng đặc biệt chọn một khoảng cách đồng đều giữa các`m + 1`những khoảng trống. 

Alice sở hữu```
ceil((m + 1) / 2)
```của những khoảng trống đó. 

Vì thế```
pS = ceil((m + 1) / 2) / (m + 1)
```### Xác suất của một quả bóng thường 

Hãy xem xét một quả bóng bình thường cụ thể. 

Trong số`m`những quả bóng thông thường, thứ hạng của nó có khả năng là bất kỳ giá trị nào từ`1`ĐẾN`m`. 

Quả bóng bình thường đầu tiên thuộc về Alice. 

Chiếc thứ 2 thuộc về Bob. 

Chiếc thứ 3 thuộc về Alice. 

Và vân vân. 

Alice sở hữu chính xác thứ hạng lẻ. 

Số hàng lẻ là```
ceil(m / 2)
```Như vậy```
pN = ceil(m / 2) / m
```khi`m > 0`. 

### Tính toán kỳ vọng 

hãy để```
special_sum = sum of first k values
ordinary_sum = sum of remaining values
```Sau đó```
Alice =
special_sum × pS
+
ordinary_sum × pN
```Tất cả số học được thực hiện modulo`10^9+7`. 

Bob nhận được giá trị mong đợi còn lại:```
Bob =
(total_sum - Alice)
```### Tại sao nó hoạt động 

Quyền sở hữu bất kỳ quả bóng nào chỉ phụ thuộc vào số lượng quả bóng thông thường xuất hiện trước nó. Những quả bóng đặc biệt không bao giờ làm thay đổi tính chẵn lẻ của lượt chơi. 

Đối với một quả bóng đặc biệt, đại lượng duy nhất có liên quan là khoảng cách giữa các quả bóng thông thường mà nó rơi vào. Mọi khoảng trống đều có khả năng xảy ra như nhau và quyền sở hữu các khoảng trống sẽ thay thế nhau. 

Đối với một quả bóng thông thường, đại lượng duy nhất có liên quan là thứ hạng của nó trong số các quả bóng thông thường. Mọi cấp bậc đều có khả năng như nhau và quyền sở hữu thay thế theo cấp bậc tương đương. 

Vì kỳ vọng là tuyến tính nên việc tính tổng giá trị nhân với xác suất sở hữu trên tất cả các quả bóng sẽ cho ra số điểm mong đợi chính xác. Tính đối xứng làm giảm việc tính toán xuống một xác suất cho tất cả các quả bóng đặc biệt và một xác suất cho tất cả các quả bóng thông thường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

t = int(input())

for _ in range(t):
    n, k = map(int, input().split())
    v = list(map(int, input().split()))

    m = n - k

    special_sum = sum(v[:k]) % MOD
    ordinary_sum = sum(v[k:]) % MOD

    if m == 0:
        alice = special_sum
        bob = 0
        print(alice, bob)
        continue

    pS = ((m + 2) // 2) * pow(m + 1, MOD - 2, MOD)
    pS %= MOD

    pN = ((m + 1) // 2) * pow(m, MOD - 2, MOD)
    pN %= MOD

    alice = (special_sum * pS + ordinary_sum * pN) % MOD

    total = (special_sum + ordinary_sum) % MOD
    bob = (total - alice) % MOD

    print(alice, bob)
```Bước đầu tiên tính toán tổng giá trị của tất cả các quả bóng đặc biệt và tất cả các quả bóng thông thường một cách riêng biệt. 

Khi không có quả bóng thông thường, lượt chơi không bao giờ thay đổi. Alice thu thập mọi thứ nên câu trả lời là ngay lập tức. 

Vì`m > 0`, mã đánh giá hai xác suất xuất phát ở trên. Phép chia theo số học modulo được thực hiện bằng định lý Fermat:```
x / y  ->  x * y^(MOD-2)
```Điểm kỳ vọng của Alice được tính trực tiếp từ tính tuyến tính của kỳ vọng. 

Điểm kỳ vọng của Bob có được bằng cách lấy tổng giá trị trừ đi kỳ vọng của Alice. Điều này tránh các phép tính xác suất trùng lặp và tự động duy trì tính bất biến mà tổng hai kỳ vọng bằng tổng giá trị. 

Trường hợp góc tinh tế duy nhất là`m = 0`, bởi vì`pN`sẽ yêu cầu chia cho số 0. Trường hợp đó được xử lý riêng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 5
k = 2
values = [10, 20, 5, 15, 25]
```Chúng tôi có:```
special_sum = 30
ordinary_sum = 45
m = 3
```| Số lượng | Giá trị | 
| --- | --- | 
| m | 3 | 
| pS | 2/4 = 1/2 | 
| pN | 2/3 | 
| Đóng góp đặc biệt | 30 × 1/2 = 15 | 
| Đóng góp thông thường | 45 × 2/3 = 30 | 
| Alice | 45 | 
| Bob | 30 | 

Câu trả lời là:```
45 30
```Ví dụ này cho thấy các quả bóng đặc biệt và thông thường có xác suất khác nhau. 

### Ví dụ tùy chỉnh```
n = 3
k = 1
values = [100, 1, 1]
```| Số lượng | Giá trị | 
| --- | --- | 
| m | 2 | 
| pS | 2/3 | 
| pN | 1/2 | 
| Đóng góp đặc biệt | 100 × 2/3 | 
| Đóng góp thông thường | 2 × 1/2 = 1 | 
| Alice | 203/3 | 
| Bob | 102 - 203/3 = 103/3 | 

Ví dụ này nêu bật lý do tại sao quả bóng đặc biệt không thể được coi là có xác suất`1/2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi quả bóng được truy cập một lần trong khi tính tổng | 
| Không gian |`O(1)`| Chỉ có một vài biến ngoài mảng đầu vào | 

Tổng số quả bóng trong tất cả các trường hợp thử nghiệm nhiều nhất là`5 · 10^5`, do đó quét tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ test case hiện tại. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        v = list(map(int, input().split()))

        m = n - k

        ss = sum(v[:k]) % MOD
        sn = sum(v[k:]) % MOD

        if m == 0:
            out.append(f"{ss} 0")
            continue

        pS = ((m + 2) // 2) * pow(m + 1, MOD - 2, MOD) % MOD
        pN = ((m + 1) // 2) * pow(m, MOD - 2, MOD) % MOD

        alice = (ss * pS + sn * pN) % MOD
        bob = (ss + sn - alice) % MOD

        out.append(f"{alice} {bob}")

    return "\n".join(out)

# sample
assert run(
"""1
5 2
10 20 5 15 25
"""
) == "45 30"

# minimum size
assert run(
"""1
1 1
7
"""
) == "7 0"

# all balls special
assert run(
"""1
4 4
1 2 3 4
"""
) == "10 0"

# one special, one ordinary
assert run(
"""1
2 1
10 20
"""
) == "25 5"

# all values equal
assert run(
"""1
5 2
1 1 1 1 1
"""
) == "3 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`|`7 0`| Ví dụ nhỏ nhất có thể | 
|`4 4 / 1 2 3 4`|`10 0`| Không có quả bóng thông thường | 
|`2 1 / 10 20`|`25 5`| Xác suất khác nhau cho các quả bóng đặc biệt và thông thường | 
|`5 2 / 1 1 1 1 1`|`3 2`| Số học đối xứng và kỳ vọng | 

## Vỏ cạnh 

### Tất cả các quả bóng đều đặc biệt 

đầu vào:```
1
4 4
1 2 3 4
```Không có quả bóng bình thường. Lượt đi không bao giờ thay đổi. 

Alice nhận mọi quả bóng:```
Alice = 10
Bob = 0
```Việc triển khai xử lý việc này trước khi tính toán bất kỳ nghịch đảo mô-đun nào. 

### Chính xác là một quả bóng bình thường 

đầu vào:```
1
2 1
10 20
```Có một khoảng trống trước quả bóng thông thường và một khoảng trống sau nó. 

Quả bóng đặc biệt rơi vào một trong hai khoảng trống với xác suất bằng nhau.```
pS = 1/2
pN = 1
```Kỳ vọng của Alice trở thành:```
10 × 1/2 + 20 × 1 = 25
```phù hợp với liệt kê trực tiếp. 

### Nhiều quả bóng đặc biệt, không ảnh hưởng đến tính chẵn lẻ 

đầu vào:```
1
5 4
10 10 10 10 100
```Chỉ có một quả bóng bình thường tồn tại. 

Tất cả bốn quả bóng đặc biệt đều có cùng xác suất:```
pS = 1/2
```bất kể có bao nhiêu quả bóng đặc biệt. Đạo hàm chỉ phụ thuộc vào số lượng bóng thông thường vì chỉ những quả bóng thông thường mới ảnh hưởng đến sự thay đổi lượt chơi. Đây chính xác là thuộc tính được sử dụng bởi thuật toán. 

### Số chẵn so với số lẻ của các quả bóng thông thường 

đầu vào:```
1
3 1
100 1 1
```Đây`m = 2`. 

Alice sở hữu hai trong ba khoảng trống, đưa ra```
pS = 2/3
```thay vì`1/2`. 

Một giải pháp giả định rằng mọi quả bóng đặc biệt đều có khả năng thuộc về một trong hai người chơi như nhau sẽ thất bại trong ví dụ này. Đối số đếm khoảng cách chính xác sẽ tạo ra xác suất lớn hơn.
