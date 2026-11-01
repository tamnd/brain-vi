---
title: "CF 106475I - \u0411\u043b\u0438\u043d\u0447\u0438\u043a \u0438 \u0440\u0438\u0441\u043e\u0432\u0430\u043d\u0438\u0435"
description: "Chúng tôi có n màu bút chì. Nếu chúng ta vẽ con cá xi với màu i, số ngôi sao nhận được từ màu này sẽ phụ thuộc vào số lượng con cá có cùng màu đó. Con cá màu đầu tiên tôi cho ai sao, con thứ hai cho ai - 2, con thứ ba cho ai - 4, v.v."
date: "2026-06-25T08:52:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106475
codeforces_index: "I"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0412\u041a\u041e\u0428\u041f.Junior 2026"
rating: 0
weight: 106475
solve_time_s: 36
verified: true
draft: false
---

[CF 106475I - \u0411\u043b\u0438\u043d\u0447\u0438\u043a \u0438 \u0440\u0438\u0441\u043e\u0432\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/106475/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`màu bút chì. Nếu chúng ta vẽ`x_i`cá có màu sắc`i`, số sao nhận được từ màu này phụ thuộc vào số lượng cá có màu đó. Con cá màu đầu tiên`i`cho`a_i`ngôi sao, thứ hai cho`a_i - 2`, người thứ ba cho`a_i - 4`, vân vân. Một màu chỉ có thể được sử dụng cho đến khi điểm của một cá thể không còn giá trị, có nghĩa là số lượng cá có màu đó không thể vượt quá`a_i + 1`. 

Nhiệm vụ là chọn chính xác`m`tổng số cá, phân bổ chúng theo các màu sao cho tổng số sao nhận được càng lớn càng tốt. Chúng ta phải xuất ra cả điểm tối đa và một phân phối hợp lệ. 

Những ràng buộc buộc chúng ta phải tránh bất cứ điều gì tỷ lệ thuận với`m`. Giá trị của`m`có thể đạt được`10^9`, vì vậy việc mô phỏng mọi con cá hoặc mọi lựa chọn có thể là không thể. Thậm chí`O(m)`sẽ yêu cầu hàng tỷ hoạt động. Số lượng màu sắc chỉ có`2 * 10^5`, vì vậy giải pháp dự định phải hoạt động gần với`O(n log A)`Ở đâu`A`là phạm vi của các giá trị sao có thể có. 

Phần khó khăn là cách phân phối tốt nhất không chỉ đơn giản là "sử dụng màu sắc có kích thước lớn nhất".`a_i`đầu tiên". Một màu sẽ trở nên kém giá trị hơn khi có nhiều cá được gán cho nó hơn. Ví dụ: nếu`a = [5]`Và`m = 3`, ba con cá cho`5`,`3`, Và`1`sao, không phải ba lần`5`. 

Một giải pháp bất cẩn cũng có thể thất bại với số lượng lớn yêu cầu. Ví dụ:```
Input
1 2
1
```Màu sắc duy nhất mang lại giá trị`1, -1, ...`. Đầu ra đúng là:```
0
0 2
```Hai con cá cùng nhau cho`1 + (-1) = 0`. Một giải pháp tham lam chỉ có những đóng góp tích cực có thể tạo ra một con cá, nhưng chính xác`m`cá phải được rút ra. 

Một trường hợp khác là khi nhiều màu có giá trị cận biên bằng nhau:```
Input
3 9
1 2 3
```Đầu ra đúng là:```
0
2 3 4
```Có thể tồn tại các phân phối tối ưu khác nhau. Thuật toán phải xử lý các mối quan hệ mà không giả định một câu trả lời duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét từng loại cá có thể bổ sung. Đối với màu sắc`i`, trình tự tăng là:```
a_i, a_i - 2, a_i - 4, ..., -a_i
```có`a_i + 1`cá có thể có màu này. Nếu chúng ta tạo ra mọi lợi ích có thể và chọn mức lớn nhất`m`, nghiệm sẽ đúng vì mỗi lựa chọn cá đều đóng góp chính xác một giá trị cận biên. Tuy nhiên,`a_i`có thể lớn như`10^9`, vì vậy chuỗi này có thể chứa hàng tỷ giá trị. Việc mở rộng tất cả các chuỗi là không thể. 

Quan sát quan trọng là các chuỗi này là các cấp số cộng được sắp xếp. Chúng ta không cần phải tạo ra chúng. Chúng ta chỉ cần biết có bao nhiêu giá trị ít nhất là một ngưỡng nào đó. 

Giả sử chúng ta chọn một giá trị`x`. Đối với một màu, số lượng tăng biên ít nhất`x`có thể được tìm thấy về mặt toán học. Nếu chúng ta có thể đếm được có bao nhiêu mức tăng vượt quá ngưỡng ở tất cả các màu thì tìm kiếm nhị phân cho phép chúng ta tìm giá trị ngưỡng trong số giá trị lớn nhất`m`lợi ích. 

Sau khi tìm được ngưỡng`t`, mọi mức tăng lớn hơn`t`phải được thực hiện và chỉ một số lợi ích tương đương với`t`có thể cần thiết để đạt được chính xác`m`cá. Bởi vì tất cả mức tăng của một màu tạo thành một chuỗi giảm dần, mức tăng được chọn cho mỗi màu luôn tạo thành một tiền tố, cho phép chúng ta tái tạo lại số lượng cá của mỗi màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng(a_i)) | O(tổng(a_i)) | Quá chậm vì giá trị có thể đạt tới 10^9 | 
| Tối ưu | O(n log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi màu, hãy xem số lượng cá có thể đóng góp theo trình tự giảm dần:`a_i, a_i - 2, a_i - 4, ... , -a_i`. 

Đáp án là tổng lớn nhất`m`giá trị trong số tất cả các chuỗi này. 
2. Triển khai hàm đếm xem có bao nhiêu giá trị trong tất cả các chuỗi ít nhất là một số cho trước`x`. 

Đối với một màu, nếu`x > a_i`, không có giá trị nào như vậy. Nếu như`x <= -a_i`, tất cả`a_i + 1`các giá trị đủ điều kiện. Ngược lại thì giải:`a_i - 2k >= x`lớn nhất có thể`k`. 
3. Tìm kiếm nhị phân ngưỡng lớn nhất`t`sao cho có ít nhất`m`giá trị cận biên lớn hơn hoặc bằng`t`. 

Ngưỡng này là giá trị của đóng góp cận biên được chọn cuối cùng. 
4. Tính toán tất cả các khoản đóng góp lớn hơn`t`. Những đóng góp này phải được chọn vì có ít hơn`m`đóng góp vượt quá ngưỡng. 
5. Cộng đủ số tiền đóng góp bằng`t`để đạt được chính xác`m`cá. 

Đối với mỗi màu, trước tiên hãy chỉ định tất cả các con cá có đóng góp lớn hơn`t`. Sau đó phân phối số cá còn lại cho các màu có mức đóng góp tiếp theo bằng`t`. 
6. Tính điểm cuối cùng bằng tổng của tất cả các đóng góp cận biên đã chọn và đưa ra số lượng cá được gán cho mỗi màu. 

Tại sao nó hoạt động: sự đóng góp cận biên của mỗi con cá bổ sung có màu cố định chỉ giảm đi, do đó, bất kỳ lựa chọn tối ưu nào từ một màu đều phải có tiền tố trong chuỗi của nó. Toàn bộ vấn đề trở thành việc lựa chọn lớn nhất`m`các phần tử từ dãy đã được sắp xếp. Ngưỡng được tìm thấy bằng tìm kiếm nhị phân sẽ tách các giá trị được chọn khỏi các giá trị không được chọn. Tất cả các giá trị trên ngưỡng là bắt buộc và việc điền vào các vị trí còn lại bằng các giá trị bằng ngưỡng không thể làm giảm câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    def count_ge(x):
        res = 0
        for v in a:
            if x > v:
                continue
            if x <= -v:
                res += v + 1
            else:
                res += (v - x) // 2 + 1
        return res

    def sum_ge(x):
        res = 0
        for v in a:
            if x > v:
                continue
            if x <= -v:
                c = v + 1
            else:
                c = (v - x) // 2 + 1
            last = v - 2 * (c - 1)
            res += c * (v + last) // 2
        return res

    lo = -10**9 - 1
    hi = 10**9 + 1
    while lo + 1 < hi:
        mid = (lo + hi) // 2
        if count_ge(mid) >= m:
            lo = mid
        else:
            hi = mid

    threshold = lo

    ans = sum_ge(threshold + 1)
    used = [0] * n
    taken = 0

    for i, v in enumerate(a):
        if threshold + 1 > v:
            continue
        if threshold + 1 <= -v:
            c = v + 1
        else:
            c = (v - (threshold + 1)) // 2 + 1
        used[i] = c
        taken += c

    need = m - taken

    for i, v in enumerate(a):
        if need == 0:
            break
        nxt = v - 2 * used[i]
        if nxt == threshold:
            used[i] += 1
            need -= 1
            ans += threshold

    print(ans)
    print(*used)

if __name__ == "__main__":
    solve()
```các`count_ge`chức năng là cốt lõi của tìm kiếm nhị phân. Nó không bao giờ xây dựng các chuỗi vì mỗi màu có thể được tính bằng các công thức cấp số cộng. 

các`sum_ge`hàm sử dụng logic đếm tương tự nhưng cũng tính tổng cấp số cộng đã chọn. Giá trị đầu tiên luôn là`a_i`và giá trị được chọn cuối cùng được tìm thấy bằng cách trừ`2`cho mỗi bước. 

Sau khi tìm thấy ngưỡng, giai đoạn tái thiết sẽ thực hiện mọi đóng góp vượt quá ngưỡng đó. Các vị trí còn lại đều phải có giá trị chính xác bằng ngưỡng nên việc kiểm tra phần đóng góp chưa sử dụng tiếp theo của mỗi màu là đủ. Thứ tự trình tự đảm bảo rằng việc thêm một con cá nữa luôn là giá trị cận biên có sẵn tiếp theo. 

Tất cả số học được thực hiện bằng số nguyên Python, vì vậy các tích lớn liên quan đến phép tính tổng sẽ không bị tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 8
3 1 4 1 5
```Trình tự cận biên là:```
Color 1: 3 1 -1 -3
Color 2: 1 -1
Color 3: 4 2 0 -2 -4
Color 4: 1 -1
Color 5: 5 3 1 -1 -3 -5
```Tám giá trị lớn nhất là:```
5, 4, 3, 3, 2, 1, 1, 1
```| Bước | Ngưỡng | Cá được chọn | Điểm | 
| --- | --- | --- | --- | 
| Ban đầu | không | tất cả các chuỗi | 0 | 
| Chọn các giá trị trên ngưỡng | 2 | Màu 5 được 2, Màu 3 được 2 | 17 | 
| Thêm các giá trị bằng điểm cắt | 1 | Thêm ba màu nhận cá | 20 | 

Phân phối đầu ra có thể là:```
1 1 2 1 3
```Dấu vết cho thấy các giá trị cận biên bằng nhau có thể được gán cho bất kỳ màu tương thích nào. 

### Ví dụ 2 

đầu vào:```
3 8
3 2 2
```Trình tự là:```
Color 1: 3 1 -1 -3
Color 2: 2 0 -2
Color 3: 2 0 -2
```| Bước | Ngưỡng | Cá được chọn | Điểm | 
| --- | --- | --- | --- | 
| Tìm kiếm nhị phân | 0 | Đếm giá trị >= 0 là đủ | 11 | 
| Lấy các giá trị trên 0 | 1 | 3, 2, 2, 1 | 8 | 
| Điền vào các số 0 còn lại | 0 | Thêm bốn đóng góp bằng không | 8 | 

Một phân phối hợp lệ là:```
3 3 2
```Điểm quan trọng là đôi khi cần có những đóng góp bằng 0 và âm vì hình ảnh phải chứa chính xác`m`cá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Tìm kiếm nhị phân kiểm tra khoảng 31 ngưỡng và mỗi lần kiểm tra sẽ quét tất cả các màu | 
| Không gian | O(n) | Chỉ mảng đầu vào và số lượng kết quả được lưu trữ | 

Phạm vi giá trị là khoảng hai tỷ, do đó tìm kiếm nhị phân chỉ cần khoảng 31 lần lặp. Với`n = 2 * 10^5`, điều này mang lại khoảng sáu triệu phép tính, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

# provided samples
assert run("""5 8
3 1 4 1 5
""").split()[0] == "20"

assert run("""3 8
3 2 2
""").split()[0] == "5"

# custom cases
assert run("""1 1
10
""").split()[0] == "10"

assert run("""1 2
1
""").split()[0] == "0"

assert run("""3 9
1 2 3
""").split()[0] == "0"

assert run("""5 14
2 2 3 2 2
""").split()[0] == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 10`|`10`| Kích thước tối thiểu và giá trị cận biên đầu tiên | 
|`1 2 / 1`|`0`| Giá trị cận biên âm đôi khi được yêu cầu | 
|`3 9 / 1 2 3`|`0`| Sử dụng tối đa năng lực và xử lý ranh giới | 
|`5 14 / 2 2 3 2 2`|`5`| Nhiều giá trị bằng nhau và tái thiết ngưỡng | 

## Vỏ cạnh 

Đối với trường hợp:```
1 2
1
```trình tự duy nhất là:```
1, -1
```Tìm kiếm nhị phân tìm thấy ngưỡng`-1`. Thuật toán lấy giá trị trên`-1`, đó là`1`, sau đó thêm một giá trị bằng`-1`. Điểm cuối cùng là`0`, và phân phối hợp lệ duy nhất là hai con cá cùng màu. 

Vì:```
3 9
1 2 3
```năng lực là`2`,`3`, Và`4`, vì vậy tất cả cá phải được sử dụng. Các giá trị cận biên được chọn là:```
1, -1
2, 0, -2
3, 1, -1, -3
```Tổng của họ bằng không. Thuật toán đạt được điều này một cách tự nhiên vì ngưỡng trở thành mức đóng góp cần thiết thấp nhất và nó vẫn tôn trọng số lượng cá chính xác. 

Đối với các màu có cùng giá trị, chẳng hạn như:```
5 14
2 2 3 2 2
```nhiều đóng góp cận biên giống hệt nhau. Tìm kiếm nhị phân chỉ xác định ranh giới giá trị chứ không xác định màu chính xác. Trong quá trình tái thiết, bất kỳ màu nào có đóng góp tiếp theo cần thiết đều được chấp nhận, đó là lý do tại sao phương pháp này có thể tạo ra bất kỳ phân phối tối ưu nào.
