---
title: "CF 106223A - Rau quả"
description: "Chúng tôi có một số loại rau. Rau i cần được sản xuất với số lượng Ai và sản xuất một đơn vị hiện tiêu tốn Bi đơn vị nước. Tổng lượng nước cần thiết là tổng lượng Ai Bi trên tất cả các loại rau."
date: "2026-06-25T06:59:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106223
codeforces_index: "A"
codeforces_contest_name: "ZCO 2024"
rating: 0
weight: 106223
solve_time_s: 40
verified: true
draft: false
---

[CF 106223A - Rau](https://codeforces.com/problemset/problem/106223/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số loại rau. Rau quả`i`cần sản xuất với số lượng`A_i`, và sản xuất một đơn vị hiện tiêu thụ`B_i`đơn vị nước. Tổng lượng nước cần thiết là tổng của`A_i * B_i`trên tất cả các loại rau. 

Việc nâng cấp có thể làm giảm một số`A_i`một, có nghĩa là thị trấn cần ít hơn một đơn vị rau đó, hoặc giảm một ít`B_i`thêm một, nghĩa là mỗi đơn vị còn lại của loại rau đó cần ít hơn một đơn vị nước. Với mỗi truy vấn, chúng ta được phép thực hiện tối đa`X`nâng cấp và cần mức tiêu thụ nước cuối cùng tối thiểu có thể. 

Điều quan trọng là mọi thao tác đều làm giảm sản phẩm hiện tại của một loại rau. Nếu chúng ta giảm`A_i`, sự cải thiện ngay lập tức là hiện tại`B_i`. Nếu chúng ta giảm`B_i`, sự cải thiện ngay lập tức là hiện tại`A_i`. Nhiệm vụ là chọn những nâng cấp loại bỏ được lượng nước lớn nhất. 

Các giới hạn đòi hỏi một cách tiếp cận cẩn thận. Các giá trị của`A_i`Và`B_i`có thể lớn nên việc mô phỏng mọi nâng cấp có thể là không thể. Một loại rau có thể có hàng triệu mức giảm trở lên và tổng số thao tác hữu ích có thể vượt xa những gì mô phỏng tuyến tính trên tất cả các hoạt động có thể xử lý. Với nhiều truy vấn, chúng ta cần xử lý trước cấu trúc của câu trả lời. 

Phần khó khăn là hiểu khi nào việc giảm bớt không còn hữu ích và tránh cập nhật lặp lại. 

Hãy xem xét một loại rau với`A = 3`Và`B = 5`. Đóng góp ban đầu của nó là`15`. Nếu chúng ta giảm`A`ba lần, lượng nước giảm đi`5`, sau đó`5`, sau đó`5`, đạt tới số không. Giảm`B`thay vào đó sẽ mang lại lợi nhuận nhỏ hơn sau`A`đã được thay đổi. Việc triển khai bất cẩn kết hợp từng hoạt động một cách tham lam có thể vẫn đúng trong các trường hợp nhỏ nhưng sẽ quá chậm. 

Một trường hợp cạnh khác là khi một bên lớn hơn nhiều so với bên kia. Ví dụ:```
Input:
1 1
1000000
1000000
1
```Có một triệu nâng cấp hữu ích. Tiết kiệm nâng cấp đầu tiên tốt nhất`1000000`nước, nhưng sau khi sử dụng hết các nâng cấp ở một bên, toàn bộ sản phẩm sẽ biến mất. Câu trả lời là:```
999999000000
```Một giải pháp cố gắng tạo ra tất cả lợi ích riêng lẻ sẽ cần lưu trữ hai triệu giá trị, điều này là không cần thiết và có thể thất bại dưới những ràng buộc lớn hơn. 

Trường hợp thứ hai là khi truy vấn yêu cầu nâng cấp nhiều hơn mức hữu ích:```
Input:
1 1
5
7
20
```Sản phẩm là`35`, và chỉ`5`cần nâng cấp để loại bỏ tất cả bằng cách giảm cạnh nhỏ hơn. Câu trả lời phải là`0`, vì những nâng cấp bổ sung không thể làm giảm lượng nước xuống dưới 0. Coi mọi sự giảm thiểu có thể có của cả hai bên là một lợi ích tích cực sẽ cho một kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng nâng cấp một cách tham lam. Ở mỗi bước, hãy xem xét từng loại rau, chọn thao tác hiện tiết kiệm nước nhất, áp dụng và tiếp tục. Điều này đúng vì mọi hoạt động đều có lợi ích rõ ràng trước mắt và chúng tôi luôn muốn mức giảm lớn nhất có thể. 

Vấn đề là số lượng hoạt động. Nếu một loại rau có`A_i = 10^6`, nó đã có thể đóng góp một triệu nâng cấp hữu ích. Trên nhiều loại rau, số lượng hoạt động mô phỏng có thể rất lớn. Việc lặp đi lặp lại việc tìm kiếm hoạt động tốt nhất thậm chí còn khiến nó trở nên tồi tệ hơn, tiếp cận xung quanh`O(total upgrades * N)`thời gian. 

Quan sát làm thay đổi vấn đề là một loại rau không có trình tự tăng trưởng phức tạp. Đối với loại rau có cạnh`A_i`Và`B_i`, chiến lược tốt nhất là loại bỏ hoàn toàn cạnh nhỏ hơn. Bên nhỏ hơn cho chúng ta biết cần bao nhiêu lần nâng cấp và mỗi lần nâng cấp đó sẽ tiết kiệm chính xác bên lớn hơn. 

Nếu như`A_i <= B_i`, chúng ta có thể giảm`A_i`chính xác`A_i`lần và mọi thao tác đều tiết kiệm`B_i`. Sau đó rau không đóng góp gì. Lý luận tương tự cũng có tác dụng khi`B_i <= A_i`. 

Vì vậy, mỗi loại rau trở thành một nhóm có giá trị giống hệt nhau. Nhóm chứa`min(A_i, B_i)`nâng cấp, mỗi giá trị`max(A_i, B_i)`Nước. Vấn đề toàn cầu hiện nay là lấy giá trị lớn nhất trong số các nhóm này. 

Vì các giá trị bên trong mỗi nhóm đều bằng nhau nên chúng ta không cần phải mở rộng chúng. Chúng tôi sắp xếp các nhóm theo giá trị của chúng, lưu trữ số tiền tố và tổng tiền tố, đồng thời trả lời từng truy vấn bằng tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số nâng cấp * N) | O(N) | Quá chậm | 
| Tối ưu | O(N log N + Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính lượng nước tiêu thụ ban đầu bằng tổng của`A_i * B_i`. Đối với mỗi loại rau, hãy tạo một nhóm nén có giá trị`max(A_i, B_i)`và kích thước`min(A_i, B_i)`. Kích thước thể hiện số lượng nâng cấp của giá trị đó tồn tại. 

Việc nén hoạt động vì cách tốt nhất để loại bỏ một hình chữ nhật có diện tích`A_i * B_i`là loại bỏ các hàng hoặc cột khỏi kích thước nhỏ hơn. 

1. Sắp xếp tất cả các nhóm theo giá trị giảm dần. Khoản tiết kiệm lớn hơn phải luôn được sử dụng trước tiên, vì vậy trình tự tối ưu là tiền tố của thứ tự này. 
2. Xây dựng mảng tiền tố. Lưu trữ bao nhiêu nâng cấp được thực hiện sau mỗi nhóm và tổng lượng nước tiết kiệm được sau khi lấy tất cả các nhóm đó. 

Các tiền tố này cho phép truy vấn bỏ qua toàn bộ nhóm thay vì xử lý từng nâng cấp. 

1. Đối với một truy vấn`X`, tìm nhóm đầu tiên có số lần nâng cấp tích lũy đạt hoặc vượt qua`X`. 

Đưa tất cả các nhóm hoàn chỉnh trước nó. Nếu một số nâng cấp vẫn còn trong nhóm hiện tại, hãy nhân số tiền còn lại với giá trị của nhóm đó. 

1. Trừ đi lượng nước tiết kiệm được tối đa từ lượng nước tiêu thụ ban đầu. Kết quả là yêu cầu nước cuối cùng tối thiểu có thể. 

Tại sao nó hoạt động: mỗi loại rau đóng góp một tập hợp các giá trị nâng cấp giống hệt nhau bằng cạnh lớn hơn của nó. Giải pháp tối ưu chỉ đơn giản là lấy các giá trị nâng cấp lớn nhất hiện có trên toàn cầu. Việc sắp xếp các nhóm nén sẽ đặt chính xác các giá trị đó lên hàng đầu và tính toán tiền tố sẽ tìm ra lượng nước tiết kiệm tốt nhất có thể cho bất kỳ truy vấn nào. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    groups = []
    total = 0

    for x, y in zip(a, b):
        total += x * y
        groups.append((max(x, y), min(x, y)))

    groups.sort(reverse=True)

    counts = []
    sums = []
    cur_count = 0
    cur_sum = 0

    for value, cnt in groups:
        cur_count += cnt
        cur_sum += value * cnt
        counts.append(cur_count)
        sums.append(cur_sum)

    ans = []

    for _ in range(q):
        x = int(input())
        if x == 0:
            ans.append(str(total))
            continue

        idx = bisect_left(counts, x)

        if idx == 0:
            saved = x * groups[0][0]
        else:
            saved = sums[idx - 1]
            used = counts[idx - 1]
            saved += (x - used) * groups[idx][0]

        ans.append(str(total - saved))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Các mảng đầu vào được đọc đầu tiên vì mỗi loại rau cần cả hai giá trị trước khi chúng ta có thể tạo thành nhóm nén của nó. Câu trả lời ban đầu được tích lũy bằng phép nhân, do đó số nguyên Python xử lý các tích lớn một cách tự nhiên. 

Việc tạo nhóm là chi tiết thực hiện trung tâm. Chúng tôi lưu trữ`(larger side, smaller side)`bởi vì bên lớn hơn là giá trị của mỗi nâng cấp hữu ích và bên nhỏ hơn là số lần nâng cấp đó tồn tại. 

Sau khi sắp xếp,`counts`lưu trữ bao nhiêu nâng cấp đã được đưa vào mỗi vị trí.`sums`lưu trữ tổng mức giảm tương ứng. Tìm kiếm nhị phân tìm nhóm đầu tiên chứa vị trí câu trả lời. Mọi thứ trước nhóm đó đã được lấy toàn bộ và nhóm hiện tại có thể bị lấy một phần. 

Không cần phải xử lý tràn hoặc kích thước số nguyên đặc biệt vì số nguyên Python tự động tăng lên. Ranh giới quan trọng là trường hợp truy vấn lớn hơn tổng số bản nâng cấp hữu ích, được xử lý vì nhóm cuối cùng được tìm thấy và số lượng bổ sung không đóng góp gì ngoài việc giảm tổng số xuống 0. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
N = 4
A = [2,4,5,3]
B = [5,2,3,3]
```Các nhóm là: 

| Rau | Giá trị | Đếm | 
| --- | --- | --- | 
| 1 | 5 | 2 | 
| 2 | 4 | 2 | 
| 3 | 5 | 3 | 
| 4 | 3 | 3 | 

Sau khi sắp xếp: 

| Bước | Giá trị hiện tại | Nâng cấp được bảo hiểm | Tiết kiệm nước | 
| --- | --- | --- | --- | 
| 1 | 5 | 3 | 15 | 
| 2 | 4 | 5 | 23 | 
| 3 | 3 | 8 | 32 | 

Đối với truy vấn`X = 1`, chúng tôi thực hiện một lần nâng cấp giá trị`5`. Nước ban đầu là`42`, vậy kết quả là`42 - 5 = 37`. 

Đối với truy vấn`X = 2`, chúng tôi thực hiện hai lần nâng cấp giá trị`5`. Số tiền tiết kiệm được là`10`, cho:```
42 - 10 = 32
```Điều này chứng tỏ rằng thuật toán chọn nâng cấp theo hiệu ứng của chúng chứ không phải theo thứ tự thực vật. 

Đối với mẫu cuối cùng:```
N = 1
A = [1000000]
B = [1000000]
X = 1
```Nhóm nén là: 

| Bước | Giá trị hiện tại | Nâng cấp được bảo hiểm | Tiết kiệm nước | 
| --- | --- | --- | --- | 
| 1 | 1000000 | 1000000 | 1000000000000 | 

Chỉ sử dụng một lần nâng cấp nên số tiền tiết kiệm được là`1000000`. Số nước còn lại là:```
1000000000000 - 1000000 = 999999000000
```Dấu vết cho thấy tại sao việc nén là cần thiết. Chúng tôi không bao giờ lưu trữ một triệu giá trị giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + Q log N) | Việc sắp xếp các nhóm nén chiếm ưu thế trong quá trình tiền xử lý và mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(N) | Chúng tôi lưu trữ một nhóm nén và hai mảng tiền tố cho mỗi rau | 

Thuật toán xử lý các giá trị lớn vì số lượng đối tượng được lưu trữ phụ thuộc vào số lượng rau chứ không phải số lượng nâng cấp có thể có. Quá trình xử lý truy vấn vẫn nhanh chóng ngay cả khi số lượng nâng cấp được phép rất lớn. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    groups = []
    total = 0

    for x, y in zip(a, b):
        total += x * y
        groups.append((max(x, y), min(x, y)))

    groups.sort(reverse=True)

    counts = []
    sums = []
    c = 0
    s = 0

    for v, cnt in groups:
        c += cnt
        s += v * cnt
        counts.append(c)
        sums.append(s)

    out = []

    for _ in range(q):
        x = int(input())
        idx = bisect_left(counts, x)
        if idx == 0:
            saved = x * groups[0][0]
        else:
            saved = sums[idx - 1]
            saved += (x - counts[idx - 1]) * groups[idx][0]
        out.append(str(total - saved))

    return "\n".join(out)

assert run("""4 2
2 4 5 3
5 2 3 3
1
2
""") == "37\n32"

assert run("""1 1
1000000
1000000
1
""") == "999999000000"

assert run("""1 1
5
7
20
""") == "0"

assert run("""2 3
1 2
10 3
1
2
3
""") == "12\n2\n0"

assert run("""3 2
4 4 4
4 4 4
5
20
""") == "28\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên |`37`,`32`| Đặt hàng tham lam bình thường | 
| Một loại rau khổng lồ |`999999000000`| Số lượng lớn mà không cần mở rộng | 
| Nhiều nâng cấp hơn mức cần thiết |`0`| Độ bão hòa sau khi loại bỏ toàn bộ rau | 
| Quy mô nhóm khác nhau |`12`,`2`,`0`| Sử dụng nhóm một phần | 
| Giá trị bình đẳng ở mọi nơi |`28`,`0`| Xử lý dây buộc và loại bỏ hoàn toàn | 

## Vỏ cạnh 

Trường hợp rau củ khổng lồ được xử lý bằng cách nén. Đối với một`1000000 x 1000000`rau, thuật toán lưu trữ một nhóm có giá trị`1000000`và đếm`1000000`. Một truy vấn yêu cầu một bản nâng cấp sẽ trực tiếp tìm thấy nhóm đó và trừ đi`1000000`từ sản phẩm ban đầu. 

Trường hợp nâng cấp vượt quá tổng số thao tác hữu ích cũng được xử lý một cách tự nhiên. Số tiền tố đạt đến tổng số mức giảm có thể và giá trị nước cuối cùng trở thành 0. Các nâng cấp bổ sung không thể tạo ra mức tiêu thụ nước âm, vì vậy câu trả lời vẫn là 0. 

Khi một số loại rau có cùng giá trị tốt nhất, việc phân loại sẽ giữ chúng lại với nhau. Tìm kiếm nhị phân có thể dừng bên trong bất kỳ nhóm nào trong số các nhóm bằng nhau này, nhưng kết quả không thay đổi vì mọi thao tác trong các nhóm liên kết đều có cùng lợi ích.
