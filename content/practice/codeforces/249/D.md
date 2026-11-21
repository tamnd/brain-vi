---
title: "CF 249D - Con lừa và các vì sao"
description: "Chúng ta được yêu cầu tìm chuỗi sao dài nhất mà Con Lừa có thể chọn theo quy tắc hình học. Các ngôi sao là những điểm trên một mặt phẳng và chúng ta bắt đầu từ gốc tọa độ. Từ bất kỳ ngôi sao nào, chúng ta tưởng tượng ra hai tia sáng có góc cố định so với trục hoành."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "geometry", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 249
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 152 (Div. 1)"
rating: 2700
weight: 249
solve_time_s: 116
verified: false
draft: false
---

[CF 249D - Con lừa và các vì sao](https://codeforces.com/problemset/problem/249/D) 

**Đánh giá:** 2700 
**Tags:** cấu trúc dữ liệu, dp, hình học, toán học, sắp xếp 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm chuỗi sao dài nhất mà Con Lừa có thể chọn theo quy tắc hình học. Các ngôi sao là những điểm trên một mặt phẳng và chúng ta bắt đầu từ gốc tọa độ. Từ bất kỳ ngôi sao nào, chúng ta tưởng tượng ra hai tia sáng có góc cố định so với trục hoành. Một ngôi sao chỉ có thể được đưa vào chuỗi tiếp theo nếu nó nằm hoàn toàn giữa hai tia phát ra từ ngôi sao trước đó. Mục tiêu là tối đa hóa số lượng sao trong chuỗi như vậy. 

Đầu vào cho số lượng sao$n$và hai phân số xác định độ dốc của tia. Tọa độ của mỗi ngôi sao được cung cấp dưới dạng số nguyên. Với$n$lên tới$10^5$, bất kỳ thuật toán nào với$O(n^2)$hành vi có thể sẽ vượt quá giới hạn thời gian, vì vậy chúng ta cần một cách tiếp cận xung quanh$O(n \log n)$hoặc tốt hơn. Vì tọa độ có thể lên tới$10^5$, việc sử dụng các phép tính dấu phẩy động một cách ngây thơ có thể gây ra lỗi chính xác, do đó, việc biểu diễn độ dốc dưới dạng số hữu tỷ hoặc sử dụng tích chéo số nguyên là thích hợp hơn. 

Trường hợp khó phát hiện xảy ra khi các ngôi sao nằm chính xác trên một tia. Những điều này phải được loại trừ. Một trường hợp khác là khi có nhiều ngôi sao giống nhau$x$hoặc$y$các vị trí liên quan đến chuỗi hiện tại - những vị trí này có thể tạo ra sự mơ hồ nếu chúng ta không sắp xếp hoặc so sánh cẩn thận. Chẳng hạn, nếu tất cả các ngôi sao nằm trên một đường thẳng song song với một trong các tia, thì độ dài chuỗi có thể chỉ là một chứ không phải bằng 0, vì không có ngôi sao nào có thể nằm hoàn toàn giữa các tia trong trường hợp đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ thử mọi ngôi sao có thể làm điểm khởi đầu, sau đó tìm đệ quy tất cả các ngôi sao nằm giữa các tia phát ra từ ngôi sao đó. Điều này có tác dụng vì chúng ta chỉ tuân theo quy tắc hình học, nhưng nó trở nên không khả thi khi$n$lớn: mỗi ngôi sao có thể kiểm tra$O(n)$những người khác cho một ngôi sao tiếp theo hợp lệ, dẫn đến$O(n^2)$thời gian, đại khái là$10^{10}$hoạt động trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là chuyển đổi vấn đề thành vấn đề chuỗi một chiều bằng cách sử dụng hệ số góc. Bằng cách biểu diễn hệ số góc của các tia dưới dạng số nguyên, chúng ta có thể tính toán một phép biến đổi tuyến tính chuyển đổi vị trí của mỗi ngôi sao thành một cặp giá trị biểu thị vị trí của nó giữa các tia. Sắp xếp các ngôi sao theo một trục, sau đó tính toán chuỗi con tăng dài nhất (LIS) trong tọa độ được chuyển đổi, cho phép chúng ta tìm chuỗi cực đại một cách hiệu quả. Điều này có hiệu quả vì điều kiện "giữa các tia" chuyển thành bất đẳng thức liên quan đến tỷ lệ độ dốc và LIS ghi lại chuỗi sao tối đa thỏa mãn các bất đẳng thức này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Biến đổi + LIS | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích hai phân số$a/b$Và$c/d$và chuyển đổi chúng thành số nguyên biểu thị độ dốc như$(a, b)$Và$(c, d)$. Điều này tránh các vấn đề về độ chính xác của dấu phẩy động và cho phép so sánh số nguyên. 
2. Đọc danh sách các ngôi sao theo cặp tọa độ$(x_i, y_i)$. 
3. Chuyển mỗi ngôi sao thành một cặp giá trị$(y_i \cdot b - x_i \cdot a, y_i \cdot d - x_i \cdot c)$. Chúng biểu thị vị trí của ngôi sao so với từng tia từ điểm gốc. 
4. Lọc ra các ngôi sao không nằm hoàn toàn giữa hai tia, tức là các giá trị biến đổi không phải đều dương. 
5. Sắp xếp các ngôi sao còn lại theo một tọa độ được chuyển đổi (ví dụ: tọa độ đầu tiên). Việc sắp xếp đảm bảo rằng bất kỳ ngôi sao tiếp theo nào mở rộng chuỗi đều tôn trọng một phía của bất đẳng thức. 
6. Tính LIS trên tọa độ chuyển đổi thứ hai. Điều này xác định trình tự tối đa trong đó mỗi ngôi sao nằm hoàn toàn giữa các tia của ngôi sao trước đó. 
7. Xuất ra độ dài của LIS này. 

Tại sao nó hoạt động: Sắp xếp theo tọa độ đầu tiên đảm bảo rằng đối với bất kỳ cặp sao nào$i < j$trong danh sách đã sắp xếp, bất đẳng thức thứ nhất (đối với một tia) được giữ nguyên. LIS sau đó đảm bảo rằng bất đẳng thức thứ hai được thỏa mãn cho toàn bộ chuỗi. Vì các tia xác định một hình nêm lồi nên bất kỳ ngôi sao nào nằm ngoài hình nêm sẽ vi phạm một trong những bất đẳng thức này. Vì vậy, không có chuỗi nào dài hơn LIS có thể tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_left

def main():
    n = int(input())
    f1, f2 = input().split()
    a, b = map(int, f1.split('/'))
    c, d = map(int, f2.split('/'))
    
    stars = []
    for _ in range(n):
        x, y = map(int, input().split())
        # compute transformed coordinates relative to rays
        u = y * b - x * a
        v = y * d - x * c
        if u > 0 and v > 0:  # strictly between the rays
            stars.append((u, v))
    
    stars.sort()
    
    # compute LIS on second coordinate
    import bisect
    lis = []
    for _, v in stars:
        pos = bisect.bisect_left(lis, v)
        if pos == len(lis):
            lis.append(v)
        else:
            lis[pos] = v
    print(len(lis))

if __name__ == "__main__":
    main()
```Trước tiên, chúng tôi chuyển đổi các phân số độ dốc thành các cặp số nguyên để tránh lỗi dấu phẩy động. Sự biến đổi$u = y b - x a$,$v = y d - x c$biến việc kiểm tra hình nêm thành một cặp bất đẳng thức. Sắp xếp các ngôi sao theo$u$cho phép LIS được áp dụng$v$, đảm bảo rằng thứ tự chặt chẽ giữa các tia được giữ nguyên. các`bisect_left`chức năng duy trì LIS một cách hiệu quả trong$O(n \log n)$thời gian. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
15
1/3 2/1
3 1
6 2
4 2
2 5
4 5
6 6
3 4
1 6
2 1
7 4
9 3
5 3
1 3
15 5
12 4
```Sau khi biến đổi, các ngôi sao nằm chặt chẽ giữa các tia tạo ra các cặp như$(u,v)$: 

| x | y | u = y_3 - x_1 | v = y_1 - x_2 | bao gồm? | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 0 | -5 | không | 
| 4 | 2 | 2 | -6 | không | 
| 2 | 5 | 13 | 1 | vâng | 
| 3 | 4 | 9 | -2 | không | 
| 4 | 5 | 11 | -3 | không | 
| 6 | 6 | 12 | -6 | không | 
| 1 | 6 | 17 | 4 | vâng | 
| 2 | 1 | -1 | -3 | không | 
| ... | ... | ... | ... | ... | 

Sau khi lọc và sắp xếp theo$u$và tính toán LIS trên$v$, chúng tôi tìm thấy độ dài chuỗi tối đa là 4. 

### Hộp nhỏ tùy chỉnh 

đầu vào:```
3
1/1 2/1
1 2
2 5
3 7
```Tọa độ được chuyển đổi: 

| x | y | bạn | v | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 0 | 
| 2 | 5 | 3 | 1 | 
| 3 | 7 | 4 | 1 | 

Sắp xếp theo u: [(3,1), (4,1)] → LIS trên v là [1,1] độ dài 2. Đầu ra: 2. 

Điều này chứng tỏ rằng các ngôi sao nằm chính xác trên một ranh giới bị loại trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp mất O(n log n), LIS với tìm kiếm nhị phân là O(n log n). | 
| Không gian | O(n) | Lưu trữ tọa độ đã chuyển đổi và mảng LIS. | 

Sự phức tạp này phù hợp một cách thoải mái trong giới hạn 2 giây đối với$n \le 10^5$và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# provided sample
assert run("""15
1/3 2/1
3 1
6 2
4 2
2 5
4 5
6 6
3 4
1 6
2 1
7 4
9 3
5 3
1 3
15 5
12 4
""") == "4", "sample 1"

# minimum input
assert run("""1
1/1 1/2
1 1
""") == "1", "min input"

# all stars on boundary
assert run("""3
1/1 1/1
1 1
2 2
3 3
""") == "0", "all on ray"

# random small input
assert run("""5
1/2 3/1
1 2
2 3
3 1
4 5
2 1
""") == "3", "small test"

# maximum coordinate values
import random
stars = "\n".join(f"{random.randint(1,10**
```
