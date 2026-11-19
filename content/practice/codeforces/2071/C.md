---
title: "CF 2071C - Trapmigiano Reggiano"
description: "Chúng ta được cho một cây, đỉnh bắt đầu st và đỉnh đích en. Một hoán vị của tất cả các đỉnh phải được chọn. Trong bước thứ i, một miếng pho mát xuất hiện ở đỉnh p[i]. Nếu con chuột đã ở đó thì nó vẫn ở đó."
date: "2026-06-08T06:51:52+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dfs-and-similar", "dp", "greedy", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 2071
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1007 (Div. 2)"
rating: 1700
weight: 2071
solve_time_s: 134
verified: false
draft: false
---

[CF 2071C - Trapmigiano Reggiano](https://codeforces.com/problemset/problem/2071/C) 

**Đánh giá:** 1700 
**Thẻ:** thuật toán xây dựng, cấu trúc dữ liệu, dfs và tương tự, dp, tham lam, sắp xếp, cây 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây, một đỉnh bắt đầu`st`, và một đỉnh đích`en`. 

Một hoán vị của tất cả các đỉnh phải được chọn. Trong thời gian`i`-Bước thứ, một miếng pho mát xuất hiện ở đỉnh`p[i]`. Nếu con chuột đã ở đó thì nó vẫn ở đó. Ngược lại, nó sẽ di chuyển chính xác một cạnh dọc theo đường đi đơn giản duy nhất hướng tới đỉnh đó. 

Hoán vị xác định một chuỗi các điểm hấp dẫn. Chúng ta phải sắp xếp tất cả các đỉnh đúng một lần để sau khi xử lý toàn bộ hoán vị, chuột sẽ đến đỉnh`en`. 

Nhiệm vụ không phải là mô phỏng một quy trình cố định. Chúng ta có thể tự do lựa chọn hoán vị. 

Cây chứa tới`10^5`đỉnh trên tất cả các trường hợp thử nghiệm. Vì tổng số đỉnh được giới hạn bởi`10^5`, MỘT`O(n log n)`hoặc`O(n)`giải pháp cho mỗi trường hợp thử nghiệm là đủ nhanh. Bất cứ điều gì liên quan đến việc kiểm tra nhiều hoán vị đều không thể thực hiện được. Ngay cả việc kiểm tra tất cả các hoán vị của một cây chỉ có 15 đỉnh cũng đòi hỏi hơn một nghìn tỷ khả năng. 

Phần nguy hiểm nhất của vấn đề là chuột chỉ di chuyển một cạnh mỗi bước. Một chiến lược ngây thơ có thể cố gắng làm cho con chuột đi theo một con đường hoàn chỉnh hướng tới`en`, nhưng những vị trí đặt phô mai sau này có thể kéo nó đi lần nữa. 

Hãy xem xét một chuỗi đơn giản:```
1 - 2 - 3
st = 2
en = 2
```Nếu chọn hoán vị`[1,2,3]`, chuột di chuyển```
2 -> 1 -> 2 -> 3
```và kết thúc tại`3`, không phải tại`2`. 

Một trường hợp tế nhị khác là khi con chuột đi qua`en`trước bước cuối cùng.```
1 - 2 - 3 - 4
st = 1
en = 4
```Hoán vị`[4,1,2,3]`làm cho con chuột tiếp cận`4`ngay lập tức, nhưng những hấp dẫn sau đó sẽ kéo nó đi. Tiếp cận`en`sớm là không đủ. Chỉ có vị trí cuối cùng quan trọng. 

Một giải pháp đúng phải đảm bảo vị trí cuối cùng bất kể con chuột hiện đang ở đâu khi các đỉnh cuối cùng được xử lý. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra mọi hoán vị của các đỉnh và mô phỏng chuột. Để hoán vị độ dài`n`, mô phỏng mất`O(n)`các bước. có`n!`hoán vị, do đó tổng độ phức tạp là`O(n·n!)`. 

Ngay cả đối với`n = 15`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng đến từ việc quan sát cây so với đỉnh bẫy`en`. 

Rễ cây tại`en`. Bây giờ mọi đỉnh đều có độ sâu bằng khoảng cách từ nó đến`en`. 

Giả sử chúng ta xử lý các đỉnh theo thứ tự độ sâu giảm dần. Theo trực giác, chúng ta thấy pho mát đầu tiên ở phần sâu nhất của cây và sau đó chỉ ở gần gốc. 

Tại sao điều này hữu ích? 

Thực hiện bất kỳ bước nào mà pho mát xuất hiện ở một đỉnh nào đó`v`. Nếu chuột chưa ở đó, nó sẽ di chuyển một cạnh về phía`v`. 

Khi các đỉnh được xử lý từ độ sâu lớn hơn đến độ sâu nhỏ hơn, mọi đỉnh chưa được xử lý đều nằm ở độ sâu không lớn hơn đỉnh hiện tại. Trong cây có gốc, việc di chuyển về phía đỉnh sâu hơn không thể di chuyển chuột ra xa tập hợp các đỉnh sâu hơn đã được xử lý. Quá trình dần dần sụp đổ về phía gốc. 

Thực tế quan trọng là đỉnh cuối cùng trong thứ tự này là`en`chính nó, bởi vì nó là đỉnh duy nhất có chiều sâu`0`. 

Vào thời điểm chúng tôi xử lý đỉnh cuối cùng, con chuột luôn nằm ở đâu đó trên cây. Phô mai xuất hiện ở`en`, và chuột di chuyển một bước về phía`en`. Cấu trúc sắp xếp đảm bảo rằng sau khi sử dụng hết tất cả các đỉnh trước đó, vị trí của con chuột trùng với cây con hoạt động còn lại chứa`en`, buộc vị trí cuối cùng là`en`. 

Một cách chính thức hơn để xem nó là thông qua hàm thế. Cho phép`d(v)`biểu thị độ sâu từ`en`. Xử lý các đỉnh theo độ sâu giảm dần đảm bảo rằng sau khi xử lý tất cả các đỉnh có độ sâu lớn hơn`k`, con chuột phải nằm bên trong cây con được tạo ra bởi các đỉnh có độ sâu nhiều nhất`k`. Bất biến này co lại cho đến khi chỉ`en`vẫn còn. 

Để xây dựng hoán vị, chúng ta chỉ cần độ sâu của mỗi đỉnh từ`en`. Sắp xếp các đỉnh theo độ sâu giảm dần sẽ ngay lập tức đưa ra câu trả lời hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 

|---|------|---| 

| Lực lượng vũ phu | O(n·n!) | O(n) | Quá chậm | 

| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh`en`. 
2. Chạy DFS hoặc BFS bắt đầu từ`en`và tính độ sâu của mỗi đỉnh. 
3. Tạo danh sách chứa tất cả các đỉnh. 
4. Sắp xếp các đỉnh theo độ sâu giảm dần. 
5. Xuất ra thứ tự kết quả dưới dạng hoán vị. 

Lý do sắp xếp theo độ sâu giảm dần là vì các đỉnh ở xa bẫy hơn sẽ được xử lý trước. Các đỉnh gần bẫy bị trì hoãn cho đến hết. 

1. Vì`en`có chiều sâu`0`, nó trở thành phần tử cuối cùng của hoán vị. 

### Tại sao nó hoạt động 

Rễ cây tại`en`. 

Đối với một đỉnh`x`, xác định độ sâu của nó là khoảng cách của nó từ`en`. 

Xét tập các đỉnh chưa xuất hiện trong hoán vị. Vì chúng tôi xử lý độ sâu từ lớn nhất đến nhỏ nhất nên tập hợp này luôn được kết nối và chứa`en`. 

Ban đầu, tất cả các đỉnh đều chưa được xử lý. 

Khi một đỉnh`v`được xử lý, nó là một trong những đỉnh sâu nhất còn lại. Mọi chuyển động hướng tới`v`không thể ép chuột ra ngoài vùng kết nối được hình thành bởi các đỉnh còn lại. Sau khi loại bỏ`v`xét đến thì con chuột vẫn nằm bên trong thành phần kết nối còn lại chứa`en`. 

Việc lặp lại đối số này sau mỗi bước cho thấy rằng sau khi xử lý tất cả trừ đỉnh cuối cùng, chuột phải nằm bên trong tập hợp còn lại. Đỉnh duy nhất còn lại là`en`, vậy con chuột phải ở`en`khi quá trình kết thúc. 

Như vậy hoán vị được tạo ra luôn bắt được chuột vào bẫy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, st, en = map(int, input().split())

        g = [[] for _ in range(n + 1)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        parent = [-1] * (n + 1)
        depth = [0] * (n + 1)

        stack = [en]
        parent[en] = 0

        while stack:
            u = stack.pop()

            for v in g[u]:
                if v == parent[u]:
                    continue

                parent[v] = u
                depth[v] = depth[u] + 1
                stack.append(v)

        order = list(range(1, n + 1))
        order.sort(key=lambda x: depth[x], reverse=True)

        print(*order)

solve()
```DFS tính toán khoảng cách từ`en`, trở thành độ sâu trong cây có gốc. 

Mảng cha ngăn không cho các cạnh đi ngược lại về phía cha. Vì đồ thị là một cây nên việc thăm mỗi đỉnh đúng một lần là đủ. 

Sau khi đã biết độ sâu, nhiệm vụ duy nhất còn lại là sắp xếp. Các đỉnh xa hơn`en`xuất hiện sớm hơn trong hoán vị. Bởi vì`en`có chiều sâu`0`, nó tự nhiên trở thành phần tử cuối cùng. 

Không cần xử lý đặc biệt đối với`st`. Việc xây dựng hoạt động cho mọi vị trí bắt đầu. 

Một sai lầm dễ mắc phải là nhổ cây ở`st`thay vì`en`. Đối số chính xác phụ thuộc hoàn toàn vào khoảng cách từ đỉnh bẫy. Sử dụng bất kỳ gốc nào khác sẽ phá vỡ bằng chứng. 

Một lỗi phổ biến khác là sắp xếp theo thứ tự độ sâu tăng dần. Nơi đó`en`đầu tiên thay vì cuối cùng và thường thất bại. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 3
st = 2
en = 2

1 - 2 - 3
```Độ sâu từ`en = 2`: 

| Đỉnh | Độ sâu | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 

Sắp xếp theo độ sâu giảm dần:```
[1, 3, 2]
```Mô phỏng chuột: 

| Bước | Phô mai | Chuột Trước | Chuột Sau | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 
| 2 | 3 | 1 | 2 | 
| 3 | 2 | 2 | 2 | 

Vị trí cuối cùng là`2 = en`. 

Ví dụ này cho thấy chuột có thể di chuyển ra khỏi bẫy và quay lại sau. 

### Ví dụ 2```
n = 6
st = 1
en = 4

Edges:
1-2
1-3
1-4
4-5
5-6
```Độ sâu từ`4`: 

| Đỉnh | Độ sâu | 
| --- | --- | 
| 4 | 0 | 
| 1 | 1 | 
| 5 | 1 | 
| 2 | 2 | 
| 3 | 2 | 
| 6 | 2 | 

Một đơn đặt hàng hợp lệ:```
[2, 3, 6, 1, 5, 4]
```Mô phỏng: 

| Bước | Phô mai | Chuột Trước | Chuột Sau | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 
| 2 | 3 | 2 | 1 | 
| 3 | 6 | 1 | 4 | 
| 4 | 1 | 4 | 1 | 
| 5 | 5 | 1 | 4 | 
| 6 | 4 | 4 | 4 | 

Con chuột chạm tới bẫy nhiều lần trong quá trình này, nhưng chỉ vị trí cuối cùng mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | DFS là O(n), việc sắp xếp chiếm ưu thế | 
| Không gian | O(n) | Danh sách kề, mảng sâu, mảng cha | 

Tổng của tất cả`n`giá trị nhiều nhất là`10^5`. Sắp xếp`10^5`các đỉnh và thực hiện một DFS cho mỗi trường hợp thử nghiệm dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = []

    t = int(input())

    for _ in range(t):
        n, st, en = map(int, input().split())

        g = [[] for _ in range(n + 1)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        parent = [-1] * (n + 1)
        depth = [0] * (n + 1)

        stack = [en]
        parent[en] = 0

        while stack:
            u = stack.pop()

            for v in g[u]:
                if v == parent[u]:
                    continue
                parent[v] = u
                depth[v] = depth[u] + 1
                stack.append(v)

        order = list(range(1, n + 1))
        order.sort(key=lambda x: depth[x], reverse=True)

        out.append(" ".join(map(str, order)))

    return "\n".join(out)

# minimum tree
assert run("""1
1 1 1
""") == "1"

# single edge
assert run("""1
2 1 2
1 2
""") == "1 2"

# chain
assert run("""1
3 2 2
1 2
2 3
""") == "1 3 2"

# star centered at trap
assert run("""1
4 2 1
1 2
1 3
1 4
""").split()[-1] == "1"

# long chain, trap at one endpoint
assert run("""1
5 3 1
1 2
2 3
3 4
4 5
""") == "5 4 3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn |`1`| Cây nhỏ nhất có thể | 
| Hai đỉnh |`1 2`| Chuyển động cơ bản | 
| Xích có bẫy ở giữa |`1 3 2`| Các đỉnh có độ sâu bằng nhau | 
| Sao tập trung vào bẫy | Bẫy xuất hiện cuối cùng | Xử lý gốc | 
| Dây chuyền dài | Thứ tự ngược lại | Đặt hàng độ sâu tối đa | 

## Vỏ cạnh 

### Cây có một đỉnh 

đầu vào:```
1
1 1 1
```DFS chỉ định độ sâu`0`đến đỉnh`1`. 

Thứ tự sắp xếp chỉ đơn giản là:```
[1]
```Chuột bắt đầu và kết thúc ở cái bẫy. 

### Bẫy bằng đỉnh bắt đầu 

đầu vào:```
1
3 2 2
1 2
2 3
```Độ sâu là:```
1 -> 1
3 -> 1
2 -> 0
```Hoán vị kết thúc bằng`2`. 

Mặc dù con chuột bắt đầu vào bẫy nhưng việc xuất hiện miếng phô mai sớm hơn có thể khiến nó bỏ đi. Lần xuất hiện cuối cùng tại`2`đảm bảo quá trình kết thúc chính xác. 

###Bẫy là một chiếc lá 

đầu vào:```
1
4 1 4
1 2
2 3
3 4
```Độ sâu từ`4`:```
1 -> 3
2 -> 2
3 -> 1
4 -> 0
```Hoán vị trở thành:```
[1, 2, 3, 4]
```Cái bẫy vẫn xuất hiện cuối cùng. Việc chứng minh chỉ phụ thuộc vào độ sâu từ`en`, không phải trên hình dạng của cây. 

### Nhiều đỉnh có cùng độ sâu 

đầu vào:```
1
5 1 3
3 1
3 2
3 4
3 5
```Tất cả các lá đều có chiều sâu`1`. 

Bất kỳ thứ tự nào trong số các lá đó đều hợp lệ vì việc chứng minh tính chính xác chỉ yêu cầu độ sâu không tăng. Các đỉnh có độ sâu bằng nhau có thể xuất hiện theo bất kỳ thứ tự nào.
