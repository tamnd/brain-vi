---
title: "CF 315B - Sereja và mảng"
description: "Chúng ta có một mảng các số nguyên và một chuỗi các phép tính. Một thao tác thay đổi một vị trí thành một giá trị mới. Một thao tác khác thêm cùng một số vào mọi phần tử trong mảng. Thao tác thứ ba yêu cầu giá trị hiện tại tại một vị trí cụ thể."
date: "2026-06-06T01:22:32+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 315
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 187 (Div. 2)"
rating: 1200
weight: 315
solve_time_s: 129
verified: true
draft: false
---

[CF 315B - Sereja và Mảng](https://codeforces.com/problemset/problem/315/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng các số nguyên và một chuỗi các phép tính. 

Một thao tác thay đổi một vị trí thành một giá trị mới. Một thao tác khác thêm cùng một số vào mọi phần tử trong mảng. Thao tác thứ ba yêu cầu giá trị hiện tại tại một vị trí cụ thể. 

Thách thức đến từ hoạt động bổ sung toàn cầu. Việc triển khai trực tiếp sẽ cập nhật tất cả`n`các phần tử mỗi khi một hoạt động như vậy xuất hiện. Với cả hai`n`và số lượng hoạt động`m`đạt`100000`, cách tiếp cận đó trở nên quá tốn kém. 

Đầu vào đầu tiên cung cấp mảng ban đầu. Sau đó nó cung cấp`m`hoạt động. Đối với mọi thao tác truy vấn, chúng ta phải in giá trị hiện được lưu trữ tại chỉ mục được yêu cầu sau khi xem xét tất cả các cập nhật xảy ra trước đó. 

Các ràng buộc gợi ý rõ ràng rằng mọi thao tác phải được xử lý trong thời gian không đổi hoặc thời gian logarit. Một giải pháp chạm vào toàn bộ mảng trong quá trình bổ sung toàn cục sẽ yêu cầu tối đa:$$100000 \times 100000 = 10^{10}$$cập nhật phần tử trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong một giây. 

Một số tình huống tế nhị có thể phá vỡ việc thực hiện bất cẩn. 

Coi như:```
3 3
1 2 3
2 5
1 1 10
3 1
```Sau khi thêm 5 vào mọi phần tử, mảng logic sẽ trở thành:```
6 7 8
```Sau đó vị trí 1 được gán giá trị 10. 

Câu trả lời là:```
10
```Một lỗi phổ biến là lưu trữ vật lý giá trị cũ cộng với các phần bổ sung tích lũy và sau đó thêm lại mức tăng chung trong các truy vấn, tạo ra 15 thay vì 10. 

Một trường hợp phức tạp khác là nhiều lần bổ sung toàn cầu.```
2 4
1 1
2 3
2 4
3 1
```Câu trả lời đúng là:```
8
```Hai sự bổ sung tích lũy. Thay vào đó, bất kỳ giải pháp nào chỉ ghi nhớ phần bổ sung gần đây nhất sẽ trả về 5. 

Một kịch bản quan trọng hơn là chỉ định sau một số lần bổ sung và sau đó áp dụng nhiều lần bổ sung hơn.```
1 4
10
2 5
1 1 7
2 2
3 1
```Mảng phát triển như sau:```
10
15
7
9
```Đầu ra đúng là:```
9
```Nhiệm vụ đặt lại phần tử về chính xác 7 tại thời điểm đó, bất kể các bổ sung trước đó. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là luôn duy trì các giá trị mảng thực tế. 

Đối với thao tác loại 1, hãy cập nhật một phần tử. 

Đối với thao tác loại 2, lặp qua toàn bộ mảng và thêm giá trị được yêu cầu vào mọi vị trí. 

Đối với thao tác loại 3, hãy in phần tử được yêu cầu. 

Phương pháp này rất dễ hiểu vì mảng được lưu trữ luôn khớp với mảng thực. Thật không may, mỗi chi phí bổ sung toàn cầu`O(n)`. Nếu tất cả`m`hoạt động là loại 2, tổng độ phức tạp trở thành`O(nm)`, có thể đạt tới`10^10`cập nhật. 

Quan sát quan trọng là mọi phép cộng toàn cục đều ảnh hưởng như nhau đến tất cả các phần tử. 

Thay vì sửa đổi ngay toàn bộ mảng, chúng ta có thể giữ một biến duy nhất`add`đại diện cho tổng số tiền được thêm vào mọi phần tử cho đến nay. 

Giả định`add = 17`. Khi đó giá trị thực của một phần tử là:```
stored_value + 17
```Một truy vấn trở nên dễ dàng. Chúng tôi chỉ đơn giản trả về giá trị được lưu trữ cộng với`add`. 

Thử thách duy nhất là các hoạt động phân công. 

Nếu chúng ta thực thi:```
a[v] = x
```trong khi`add = 17`, chúng tôi cần các truy vấn trong tương lai để trả về chính xác`x`vào thời điểm đó. 

Vì các truy vấn luôn thêm`add`quay lại sau, chúng tôi lưu trữ:```
a[v] = x - add
```Sau đó:```
stored + add = (x - add) + add = x
```đó chính xác là những gì chúng tôi muốn 

Điều này biến mọi hoạt động thành thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng ban đầu. 
2. Tạo một biến`add = 0`. 

Biến này lưu trữ tổng giá trị gia tăng trên toàn cầu bởi tất cả các hoạt động loại 2 được xử lý cho đến nay. 
3. Đối với thao tác loại 1`(v, x)`, cửa hàng:```
a[v] = x - add
```Giá trị được lưu trữ được điều chỉnh sao cho sau khi thêm lại giá trị bù chung hiện tại, giá trị logic sẽ trở thành chính xác`x`. 
4. Đối với thao tác loại 2`(y)`, cập nhật:```
add += y
```Không có phần tử mảng nào được chạm vào. 
5. Đối với thao tác loại 3`(q)`, đầu ra:```
a[q] + add
```Giá trị được lưu trữ cộng với phần bù tích lũy bằng giá trị logic hiện tại. 
6. Tiếp tục cho đến khi tất cả các thao tác được xử lý. 

### Tại sao nó hoạt động 

Bất biến trung tâm là:```
real_value(i) = stored_value(i) + add
```Ban đầu`add = 0`, vậy bất biến là đúng. 

Khi một sự bổ sung toàn cầu xảy ra, chúng tôi tăng`add`. Giá trị thực của mọi phần tử đều tăng cùng một lượng, do đó bất biến vẫn hợp lệ. 

Khi một bài tập đặt một phần tử thành`x`, chúng tôi lưu trữ`x - add`. Giá trị thực khi đó trở thành:```
(x - add) + add = x
```do đó vị trí được chỉ định ngay lập tức nhận được giá trị chính xác trong khi tất cả các vị trí khác không thay đổi. 

Vì mọi thao tác đều bảo toàn tính bất biến nên mọi truy vấn đều trả về giá trị hiện tại chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))

add = 0
ans = []

for _ in range(m):
    op = list(map(int, input().split()))

    if op[0] == 1:
        v, x = op[1], op[2]
        a[v - 1] = x - add

    elif op[0] == 2:
        y = op[1]
        add += y

    else:
        q = op[1]
        ans.append(str(a[q - 1] + add))

sys.stdout.write("\n".join(ans))
```Mảng`a`không lưu trữ các giá trị thực tế. Thay vào đó, nó lưu trữ các giá trị liên quan đến offset toàn cục hiện tại. 

Biến`add`tích lũy mọi hoạt động loại 2. Đây là phần lười biếng của giải pháp. Thay vì cập nhật tất cả các vị trí, chúng tôi ghi nhớ số lượng cần thêm vào mỗi phần tử. 

Nhiệm vụ đòi hỏi sự chăm sóc đặc biệt. Nếu chúng ta chỉ đơn giản lưu trữ`x`, các truy vấn sau này sẽ thêm`add`lại và cho kết quả sai. Lưu trữ`x - add`bù đắp cho sự bù đắp hiện tại. 

Mảng sử dụng lập chỉ mục dựa trên 0 trong khi đầu vào sử dụng lập chỉ mục dựa trên một. Mỗi quyền truy cập vị trí phải trừ đi một. Thiếu chuyển đổi này là lỗi triển khai phổ biến nhất. 

Số nguyên Python tự động xử lý các giá trị lớn hơn 32 bit, do đó không có vấn đề tràn phát sinh ngay cả sau nhiều lần bổ sung. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 11
1 2 3 4 5 6 7 8 9 10
3 2
3 9
2 10
3 1
3 10
1 1 10
2 10
2 10
3 1
3 10
3 9
```| Hoạt động | thêm | Giá trị được lưu trữ có liên quan | Đầu ra | 
| --- | --- | --- | --- | 
| Trạng thái ban đầu | 0 | a[1]=1 | | 
| 3 2 | 0 | a[2]=2 | 2 | 
| 3 9 | 0 | a[9]=9 | 9 | 
| 2 10 | 10 | | | 
| 3 1 | 10 | a[1]=1 | 11 | 
| 3 10 | 10 | a[10]=10 | 20 | 
| 1 1 10 | 10 | a[1]=0 | | 
| 2 10 | 20 | | | 
| 2 10 | 30 | | | 
| 3 1 | 30 | a[1]=0 | 30 | 
| 3 10 | 30 | a[10]=10 | 40 | 
| 3 9 | 30 | a[9]=9 | 39 | 

Đầu ra:```
2
9
11
20
30
40
39
```Dấu vết này cho thấy lý do tại sao bài tập lưu trữ các giá trị được điều chỉnh. Sau đó`1 1 10`, vị trí số 1 cửa hàng`0`, không`10`. Việc thêm lại phần bù hiện tại sau đó sẽ tạo lại giá trị chính xác. 

### Ví dụ tùy chỉnh 

đầu vào:```
1 4
10
2 5
1 1 7
2 2
3 1
```| Hoạt động | thêm | được lưu trữ [1] | Giá trị thực | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | 10 | 10 | 
| 2 5 | 5 | 10 | 15 | 
| 1 1 7 | 5 | 2 | 7 | 
| 2 2 | 7 | 2 | 9 | 
| 3 1 | 7 | 2 | 9 | 

Đầu ra:```
9
```Ví dụ này chứng tỏ rằng một nhiệm vụ sẽ ghi đè hoàn toàn những bổ sung trước đó cho vị trí đó trong khi vẫn cho phép những bổ sung trong tương lai ảnh hưởng đến nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Đọc mảng tốn O(n), mỗi thao tác tốn O(1) | 
| Không gian | O(n) | Danh sách mảng và câu trả lời được lưu trữ | 

Giải pháp thực hiện một lượng công việc không đổi cho mọi hoạt động. Với nhiều nhất`100000`các yếu tố và`100000`hoạt động, tổng khối lượng công việc dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    add = 0
    ans = []

    for _ in range(m):
        op = list(map(int, input().split()))

        if op[0] == 1:
            v, x = op[1], op[2]
            a[v - 1] = x - add
        elif op[0] == 2:
            add += op[1]
        else:
            ans.append(str(a[op[1] - 1] + add))

    return "\n".join(ans)

# provided sample
assert run(
"""10 11
1 2 3 4 5 6 7 8 9 10
3 2
3 9
2 10
3 1
3 10
1 1 10
2 10
2 10
3 1
3 10
3 9
"""
) == "2\n9\n11\n20\n30\n40\n39"

# minimum size
assert run(
"""1 1
5
3 1
"""
) == "5"

# assignment after global addition
assert run(
"""1 3
10
2 5
1 1 7
3 1
"""
) == "7"

# multiple global additions
assert run(
"""2 3
1 1
2 3
2 4
3 1
"""
) == "8"

# off-by-one index check
assert run(
"""3 3
1 2 3
1 3 10
3 3
3 1
"""
) == "10\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn phần tử đơn | 5 | Giới hạn tối thiểu | 
| Bài tập sau khi bổ sung | 7 | Bồi thường bù đắp chính xác | 
| Nhiều bổ sung | 8 | Tích lũy bù đắp lười biếng | 
| Cập nhật vị trí cuối cùng | 10, 1 | Chuyển đổi dựa trên một sang không dựa trên | 

## Vỏ cạnh 

### Chuyển nhượng sau các lần bổ sung toàn cầu trước đó 

đầu vào:```
1 3
10
2 5
1 1 7
3 1
```Sau khi bổ sung,`add = 5`. Các cửa hàng nhiệm vụ:```
7 - 5 = 2
```Truy vấn trả về:```
2 + 5 = 7
```đó chính xác là giá trị cần thiết. 

### Nhiều lần bổ sung liên tiếp 

đầu vào:```
1 4
1
2 3
2 4
2 5
3 1
```Thuật toán không bao giờ chạm vào mảng. Nó chỉ cập nhật:```
add = 12
```Truy vấn trả về:```
1 + 12 = 13
```khớp với giá trị mảng thực sự. 

### Bài tập theo sau là những bổ sung khác 

đầu vào:```
1 4
10
2 5
1 1 7
2 2
3 1
```Khi nhiệm vụ xảy ra,`add = 5`, do đó giá trị được lưu trữ trở thành`2`. 

Sau một lần bổ sung khác,`add = 7`. 

Truy vấn trả về:```
2 + 7 = 9
```phản ánh chính xác rằng phần tử đã được đặt lại thành 7 và sau đó tăng thêm 2. 

### Vị trí đầu tiên và cuối cùng 

đầu vào:```
3 4
1 2 3
1 1 10
1 3 20
3 1
3 3
```Thuật toán truy cập các vị trí bằng cách sử dụng`index - 1`. 

Vị trí ánh xạ 1 tới chỉ mục mảng 0 và vị trí 3 ánh xạ tới chỉ mục mảng 2. 

Các kết quả đầu ra là:```
10
20
```xác nhận rằng việc chuyển đổi lập chỉ mục được xử lý chính xác ở cả hai ranh giới.
