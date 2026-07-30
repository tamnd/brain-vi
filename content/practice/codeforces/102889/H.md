---
title: "CF 102889H - \u5b9d\u53ef\u68a6\u4e0e\u5206\u652f\u8fdb\u5316"
description: "Có n loài Pokémon. Loài 1 là gốc của họ tiến hóa và mọi loài khác đều có chính xác một loài bố mẹ mà nó tiến hóa từ đó. Điều này tạo ra một cây có gốc trong đó việc chuyển từ cha mẹ sang con cái tượng trưng cho một bước tiến hóa."
date: "2026-07-25T12:27:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102889
codeforces_index: "H"
codeforces_contest_name: "The 15-th Beihang University Collegiate Programming Contest (BCPC 2020) - Final"
rating: 0
weight: 102889
solve_time_s: 44
verified: true
draft: false
---

[CF 102889H - \u5b9d\u53ef\u68a6\u4e0e\u5206\u652f\u8fdb\u5316](https://codeforces.com/problemset/problem/102889/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

có`n`Loài Pokémon. Giống loài`1`là gốc rễ của họ tiến hóa và mọi loài khác đều có chính xác một loài bố mẹ mà nó tiến hóa từ đó. Điều này tạo ra một cây có gốc trong đó việc chuyển từ cha mẹ sang con cái tượng trưng cho một bước tiến hóa. 

Sân có chứa`m`Pokémon được sắp xếp theo thứ tự cố định từ trái sang phải. Chúng tôi muốn chọn càng nhiều trong số chúng càng tốt trong khi vẫn giữ nguyên thứ tự ban đầu. Nếu một Pokémon được chọn xuất hiện trước một Pokémon khác, thì Pokémon thứ hai phải là hậu duệ của Pokémon đầu tiên trong cây tiến hóa. Mục đích là tìm độ dài lớn nhất của dãy con đó. 

Thứ tự trình tự và cấu trúc cây tương tác với nhau. Pokémon được chọn không nhất thiết phải là con trực tiếp của Pokémon trước đó. Cho phép thực hiện bất kỳ bước tiến hóa nào, vì vậy loài được chọn tiếp theo chỉ cần nằm ở đâu đó bên trong cây con của loài trước đó. 

Các ràng buộc này làm cho giải pháp quy hoạch động bậc hai không thể thực hiện được. Cả số lượng loài và số lượng Pokémon trong sân đều có thể đạt tới`5 * 10^5`, do đó một thuật toán thực hiện`O(nm)`hoặc thậm chí`O(m sqrt(n))`các hoạt động sẽ không phù hợp với giới hạn thời gian một giây. Chúng ta cần gần tuyến tính hoặc`O((n+m) log n)`công việc. 

Một sai lầm phổ biến là coi đây là bài toán dãy con tăng dài nhất thông thường. Mối quan hệ giữa cây không dựa trên nhãn số nên so sánh số loài cho kết quả vô nghĩa. Một trường hợp tế nhị khác là Pokémon được chọn tiếp theo phải là hậu duệ chính thống chứ không phải cùng loài. Ví dụ:```
2 2
1
1 1
```Câu trả lời đúng là`1`. Chọn cả hai Pokémon sẽ yêu cầu loài thứ hai`1`tiến hóa từ loài đầu tiên`1`, nhưng không có sự tiến hóa nào xảy ra. 

Một trường hợp khác là loài gốc. Nó không có tổ tiên nên sự xuất hiện đầu tiên của gốc không thể mở rộng bất kỳ chuỗi nào trước đó. Ví dụ:```
3 3
1 1
2 1 3
```Câu trả lời đúng là`2`, sử dụng loài`1`theo sau là loài`3`. Giải pháp vô tình coi thư mục gốc là có thư mục gốc có thể tạo ra các chuyển tiếp không hợp lệ. 

Các loài lặp đi lặp lại cũng cần được chăm sóc. Ví dụ:```
3 4
1 2
2 2 3 3
```Câu trả lời đúng là`2`. Hai bản sao của loài`2`không thể xâu chuỗi lại với nhau, bởi vì một loài không tiến hóa thành chính nó. Chuỗi tốt nhất là loài`2`theo sau là loài`3`. 

## Phương pháp tiếp cận 

Ý tưởng lập trình động trực tiếp là xử lý sân từ trái sang phải. Đối với mọi Pokémon ở vị trí`i`, định nghĩa`dp[i]`là chuỗi con hợp lệ dài nhất kết thúc ở Pokémon này. Chúng ta có thể tính toán nó bằng cách nhìn vào mọi vị trí trước đó`j`và kiểm tra xem`a[j]`là tổ tiên của`a[i]`. Nếu đúng như vậy, chúng ta có thể mở rộng chuỗi đó. 

Cách tiếp cận này đúng vì mọi lựa chọn trước đó đều được xem xét. Tuy nhiên, nó còn quá chậm. Trong trường hợp xấu nhất, có`5 * 10^5`Pokémon và việc kiểm tra tất cả các vị trí trước đó yêu cầu khoảng`2.5 * 10^11`so sánh, đó là điều không thể. 

Quan sát quan trọng là thông tin duy nhất chúng ta cần từ các Pokémon trước đây là độ dài chuỗi tốt nhất trong số tổ tiên của loài hiện tại. Khi chúng tôi xử lý xong một Pokémon thuộc loài`x`, chúng tôi thêm độ dài chuỗi của nó làm câu trả lời ứng viên cho con cháu tương lai của`x`. Các truy vấn trong tương lai yêu cầu giá trị tối đa được lưu trữ trên đường dẫn từ gốc đến nút. 

Điều này biến vấn đề thành hai phép toán cây. Chúng tôi cần truy vấn giá trị tối đa trên đường dẫn từ nút gốc đến nút, loại trừ chính nút đó và chúng tôi cần cập nhật một nút có giá trị lớn hơn. Sự phân hủy ánh sáng mạnh sẽ phá vỡ mọi đường dẫn của cây thành một số ít các đoạn liền kề. Sau đó, cây phân đoạn theo thứ tự nặng-nhẹ có thể trả lời và cập nhật các phân đoạn này một cách hiệu quả. 

Giải pháp brute-force hoạt động vì nó kiểm tra rõ ràng tất cả các phiên bản trước đó nhưng không thành công vì có quá nhiều. Quan sát tối đa đường đi của cây nén tất cả các đường đi trước có liên quan vào một số phạm vi nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(m) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây tiến hóa từ thông tin gốc. Lưu trữ mọi phần tử con để sau này cây có thể được duyệt qua để phân hủy nặng-nhẹ. 
2. Tính kích thước của mỗi cây con và xác định nút con nặng của mỗi nút. Đứa trẻ nặng nhất là đứa trẻ có cây con lớn nhất. Theo sau các phần tử nặng giữ các đường dẫn quan trọng lại với nhau, điều này làm giảm mọi đường dẫn từ nút gốc đến nút chỉ còn một số phần logarit. 
3. Chỉ định mỗi nút một vị trí theo thứ tự nặng-nhẹ. Mỗi chuỗi nặng trở thành một phân đoạn liên tục theo thứ tự này, do đó, một truy vấn đường dẫn có thể được chuyển thành các truy vấn phạm vi cây phân đoạn. 
4. Xử lý Pokémon trong sân từ trái sang phải. Đối với loài hiện tại`x`, truy vấn giá trị được lưu trữ tối đa trên đường dẫn từ gốc đến`parent[x]`. Điều này mang lại chuỗi tốt nhất trước đó có thể phát triển thành`x`. 
5. Đặt độ dài chuỗi hiện tại thành một cộng với kết quả truy vấn đó. Sau đó cập nhật vị trí của`x`trong cây phân đoạn có giá trị này. Pokémon tương lai là hậu duệ của`x`bây giờ có thể sử dụng chuỗi này. 
6. Theo dõi giá trị tối đa được tạo ra trong quá trình quét và xuất nó sau khi tất cả Pokémon đã được xử lý. 

Tại sao truy vấn dừng lại ở`parent[x]`là điều cần thiết. Loài hiện tại không thể được sử dụng làm tổ tiên của chính nó vì quá trình chuyển đổi đòi hỏi ít nhất một bước tiến hóa. 

Tính đúng đắn xuất phát từ tính bất biến sau khi xử lý giá trị đầu tiên`i`Pokémon, mỗi nút cây lưu trữ độ dài chuỗi con tốt nhất trong số các Pokémon đã được xử lý của chính xác loài đó. Do đó, việc truy vấn tổ tiên của loài hiện tại sẽ trả về chính xác Pokémon tốt nhất có thể có trước đó. Bản cập nhật duy trì tính bất biến này và mọi vị trí kết thúc có thể đều được xem xét, vì vậy giá trị được lưu trữ tối đa là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    children = [[] for _ in range(n + 1)]
    parent = [0] * (n + 1)
    parent[1] = 0

    if n > 1:
        arr = list(map(int, input().split()))
        for i, p in enumerate(arr, 2):
            parent[i] = p
            children[p].append(i)

    a = list(map(int, input().split()))

    depth = [0] * (n + 1)
    order = [1]
    for x in order:
        for y in children[x]:
            depth[y] = depth[x] + 1
            order.append(y)

    size = [1] * (n + 1)
    heavy = [0] * (n + 1)
    for x in reversed(order):
        best_size = 0
        for y in children[x]:
            size[x] += size[y]
            if size[y] > best_size:
                best_size = size[y]
                heavy[x] = y

    head = [0] * (n + 1)
    pos = [0] * (n + 1)
    rev = [0] * (n + 1)
    cur = 0

    stack = [(1, 1)]
    while stack:
        x, h = stack.pop()
        while x:
            head[x] = h
            cur += 1
            pos[x] = cur
            rev[cur] = x

            for y in children[x]:
                if y != heavy[x]:
                    stack.append((y, y))

            x = heavy[x]

    size_seg = 1
    while size_seg < n:
        size_seg *= 2
    seg = [0] * (size_seg * 2)

    def update(i, val):
        i += size_seg - 1
        if seg[i] >= val:
            return
        seg[i] = val
        i //= 2
        while i:
            nv = seg[i * 2] if seg[i * 2] >= seg[i * 2 + 1] else seg[i * 2 + 1]
            if seg[i] == nv:
                break
            seg[i] = nv
            i //= 2

    def query(l, r):
        if l > r:
            return 0
        l += size_seg - 1
        r += size_seg - 1
        ans = 0
        while l <= r:
            if l & 1:
                if seg[l] > ans:
                    ans = seg[l]
                l += 1
            if not (r & 1):
                if seg[r] > ans:
                    ans = seg[r]
                r -= 1
            l //= 2
            r //= 2
        return ans

    def path_query(x):
        ans = 0
        while head[x] != head[1]:
            val = query(pos[head[x]], pos[x])
            if val > ans:
                ans = val
            x = parent[head[x]]
        val = query(pos[1], pos[x])
        if val > ans:
            ans = val
        return ans

    answer = 0
    for x in a:
        best = 0
        if x != 1:
            best = path_query(parent[x])
        dp = best + 1
        if dp > answer:
            answer = dp
        update(pos[x], dp)

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã xây dựng cây và tính toán phân tách ánh sáng nặng. Thứ tự duyệt được lưu trữ lặp đi lặp lại vì cây có thể chứa`5 * 10^5`các nút và DFS đệ quy có thể vượt quá giới hạn đệ quy của Python. 

các`size`Và`heavy`mảng xác định con nào thuộc cùng một chuỗi nặng. Quá trình phân rã gán cho mỗi nút một vị trí sao cho mỗi chuỗi nặng trở thành một khoảng liên tục. 

Cây phân đoạn lưu trữ độ dài chuỗi con tốt nhất kết thúc ở mỗi loài. Hoạt động cập nhật sử dụng`max`bởi vì những Pokémon cùng loài sau này chỉ có thể nâng cao giá trị dành cho con cháu. 

các`path_query`hàm trả lời các truy vấn tổ tiên. Trước khi truy vấn, vòng lặp chính sẽ chuyển sang`parent[x]`, loại bỏ các loài hiện tại khỏi việc xem xét. Điều này ngăn chặn sự chuyển đổi không hợp lệ giữa các loài bằng nhau. 

Tất cả các giá trị nhiều nhất`m`, vì vậy số nguyên Python dễ dàng xử lý chúng. Các hoạt động lặp lại của cây phân đoạn tránh được các vấn đề về độ sâu đệ quy và duy trì việc triển khai trong giới hạn yêu cầu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
n = 4, m = 5
parents: 1 1 2
sequence: 1 2 2 3 4
```Cây tiến hóa có`1`với tư cách là cha mẹ của`2`Và`3`, Và`2`với tư cách là cha mẹ của`4`. 

| Vị trí | Loài | Giá trị tổ tiên tốt nhất | dp hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 2 | 
| 3 | 2 | 1 | 2 | 2 | 
| 4 | 3 | 1 | 2 | 2 | 
| 5 | 4 | 2 | 3 | 3 | 

Hai lần xuất hiện của loài`2`không thể mở rộng lẫn nhau. Chuỗi tốt nhất là`1 -> 2 -> 4`, cho chiều dài`3`. 

Đối với mẫu thứ hai:```
n = 6, m = 6
parents: 1 2 3 4 5
sequence: 1 2 3 4 5 6
```Cây là một chuỗi duy nhất. 

| Vị trí | Loài | Giá trị tổ tiên tốt nhất | dp hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 2 | 
| 3 | 3 | 2 | 3 | 3 | 
| 4 | 4 | 3 | 4 | 4 | 
| 5 | 5 | 4 | 5 | 5 | 
| 6 | 6 | 5 | 6 | 6 | 

Mọi Pokémon đều có thể theo sau loài trước vì mỗi loài đều là hậu duệ của loài trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Sự phân tách mức độ nhẹ là tuyến tính và mỗi truy vấn hoặc cập nhật chạm tới nhiều nút cây phân đoạn theo logarit. | 
| Không gian | O(n) | Mảng cây, mảng phân rã và cây phân đoạn đều sử dụng bộ nhớ tuyến tính. | 

Đầu vào lớn nhất có thể chứa nửa triệu loài và nửa triệu Pokémon. Giải pháp chỉ thực hiện công việc logarit cho mỗi Pokémon sau giai đoạn tiền xử lý tuyến tính, phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())

    children = [[] for _ in range(n + 1)]
    parent = [0] * (n + 1)
    if n > 1:
        for i, p in enumerate(map(int, input().split()), 2):
            parent[i] = p
            children[p].append(i)

    a = list(map(int, input().split()))

    depth = [0] * (n + 1)
    order = [1]
    for x in order:
        for y in children[x]:
            depth[y] = depth[x] + 1
            order.append(y)

    size = [1] * (n + 1)
    heavy = [0] * (n + 1)
    for x in reversed(order):
        best = 0
        for y in children[x]:
            size[x] += size[y]
            if size[y] > best:
                best = size[y]
                heavy[x] = y

    head = [0] * (n + 1)
    pos = [0] * (n + 1)
    cur = 0
    stack = [(1, 1)]
    while stack:
        x, h = stack.pop()
        while x:
            cur += 1
            head[x] = h
            pos[x] = cur
            for y in children[x]:
                if y != heavy[x]:
                    stack.append((y, y))
            x = heavy[x]

    s = 1
    while s < n:
        s *= 2
    seg = [0] * (2 * s)

    def update(i, v):
        i += s - 1
        seg[i] = max(seg[i], v)
        i //= 2
        while i:
            seg[i] = max(seg[2 * i], seg[2 * i + 1])
            i //= 2

    def query(l, r):
        if l > r:
            return 0
        l += s - 1
        r += s - 1
        res = 0
        while l <= r:
            if l & 1:
                res = max(res, seg[l])
                l += 1
            if not r & 1:
                res = max(res, seg[r])
                r -= 1
            l //= 2
            r //= 2
        return res

    def get(x):
        res = 0
        while head[x] != head[1]:
            res = max(res, query(pos[head[x]], pos[x]))
            x = parent[head[x]]
        return max(res, query(pos[1], pos[x]))

    ans = 0
    for x in a:
        dp = 1
        if x != 1:
            dp = get(parent[x]) + 1
        ans = max(ans, dp)
        update(pos[x], dp)
    return str(ans) + "\n"

assert solve_data("""4 5
1 1 2
1 2 2 3 4
""") == "3\n", "sample 1"

assert solve_data("""6 6
1 2 3 4 5
1 2 3 4 5 6
""") == "6\n", "sample 2"

assert solve_data("""2 2
1
1 1
""") == "1\n", "same species cannot chain"

assert solve_data("""3 3
1 1
2 1 3
""") == "2\n", "root handling"

assert solve_data("""3 4
1 2
2 2 3 3
""") == "2\n", "duplicate species handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên | 3 | Hành vi phân nhánh cơ bản của cây | 
| Mẫu thứ hai | 6 | Tiến hóa chuỗi đơn dài | 
| Hai loài rễ | 1 | Các loài bằng nhau không thể liên tiếp trong một chuỗi | 
| Gốc trộn với con cháu | 2 | Root không có tổ tiên trước đó | 
| Loài trung gian lặp đi lặp lại | 2 | Ngăn chặn những sai lầm khi tự chuyển đổi | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên với các loài gốc lặp lại:```
2 2
1
1 1
```Khi chế biến loài đầu tiên`1`, cây không có kết quả truy vấn tổ tiên, vì vậy giá trị được lưu trữ là`1`. Khi chế biến loài thứ hai`1`, thuật toán vẫn chỉ truy vấn tổ tiên thích hợp. Vì không có nên nó tạo ra một chuỗi có độ dài khác`1`thay vì mở rộng không chính xác gốc trước đó. Câu trả lời vẫn còn`1`. 

Trường hợp cạnh thứ hai với gốc là điểm bắt đầu:```
3 3
1 1
2 1 3
```Loài đầu tiên`2`truy vấn cha mẹ của nó`1`và nhận số 0, tạo ra một chuỗi có độ dài`1`. Loài tiếp theo`1`cũng không có tổ tiên và tạo ra chiều dài`1`. Loài cuối cùng`3`truy vấn cha mẹ của nó`1`, nơi sự xuất hiện gốc trước đó được lưu trữ, do đó nó tạo ra độ dài`2`. Thuật toán tìm đúng chuỗi`1 -> 3`. 

Trường hợp cạnh thứ ba với các loài trung gian lặp đi lặp lại:```
3 4
1 2
2 2 3 3
```Loài đầu tiên`2`chiều dài cửa hàng`1`. Loài thứ hai`2`không thể đọc giá trị đó vì truy vấn dừng ở phần gốc của nó, tức là loài`1`, do đó nó cũng có chiều dài`1`. Khi loài`3`xuất hiện, nó có thể sử dụng giá trị được lưu trữ từ các loài`2`, tạo ra chiều dài`2`. Kết quả phù hợp với quy luật tiến hóa cần thiết.
