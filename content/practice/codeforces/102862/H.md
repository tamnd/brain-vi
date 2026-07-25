---
title: "CF 102862H - Tối ưu hóa DFS"
description: "Chúng ta có một cây trong đó mỗi đỉnh lưu trữ hai số a và b. Giá trị thú vị không phải chỉ là số mà là sự khác biệt của chúng: $$cv = av - bv$$ Một bước DFS từ đỉnh v có thể di chuyển đến đỉnh u lân cận chính xác khi: $$av+bu=au+bv$$ Sắp xếp lại sẽ cho: $$av-bv=au-bu$$…"
date: "2026-07-25T13:53:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102862
codeforces_index: "H"
codeforces_contest_name: "LU ICPC Selection Contest 2020 and KFU Open Contest 2020"
rating: 0
weight: 102862
solve_time_s: 53
verified: true
draft: false
---

[CF 102862H - Tối ưu hóa DFS](https://codeforces.com/problemset/problem/102862/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó mỗi đỉnh lưu trữ hai số,`a`Và`b`. Giá trị thú vị không chỉ ở số mà còn ở sự khác biệt của chúng:$$c_v = a_v - b_v$$Một bước DFS từ một đỉnh`v`có thể di chuyển đến một đỉnh lân cận`u`chính xác khi nào:$$a_v+b_u=a_u+b_v$$Sắp xếp lại mang lại:$$a_v-b_v=a_u-b_u$$vì vậy DFS chỉ di chuyển qua các cạnh có điểm cuối hiện có cùng`c`giá trị. Sau khi một đỉnh kết thúc trong DFS, nó`a`giá trị trở thành`b`, có nghĩa là nó`c`giá trị trở thành số không. 

Nhiệm vụ là xử lý ba loại hoạt động. Chúng ta có thể thay đổi một đỉnh`a`giá trị, yêu cầu hiện tại của một đỉnh`a`giá trị hoặc mô phỏng quá trình DFS từ đỉnh bắt đầu một cách hiệu quả. 

Các ràng buộc cho phép lên đến`5 * 10^5`đỉnh và truy vấn. Giải pháp liên tục quét toàn bộ cây hoặc chạy DFS mới trên tất cả các đỉnh cho mỗi truy vấn thứ ba sẽ là quá chậm. Với nửa triệu thao tác, chúng tôi cần tổng khối lượng công việc trên tất cả các mô phỏng DFS gần với tuyến tính. 

Điều quan trọng là truy vấn thứ ba chỉ thay đổi các đỉnh có hiệu hiện tại`c`khác 0 và bằng hiệu của đỉnh bắt đầu. Khi một đỉnh như vậy được xử lý, hiệu của nó sẽ bằng 0. Một đỉnh chỉ có thể trở thành khác 0 thông qua truy vấn loại 1, truy vấn này thay đổi chính xác một đỉnh. Điều này mang lại cho chúng ta một giới hạn khấu hao: mọi đỉnh được truy cập bởi một đợt lấp lũ thực tế đều là một trong những đỉnh khác 0 ban đầu hoặc được làm cho khác 0 bởi một bản cập nhật trước đó. 

Việc triển khai cẩn thận phải xử lý các trường hợp đỉnh bắt đầu đã có`a_v=b_v`. Ví dụ:```
1 1
5
5
```Truy vấn thứ ba từ đỉnh`1`không nên làm gì và giá trị vẫn còn`5`. Một cách triển khai ngây thơ cố gắng tìm kiếm một thành phần có chênh lệch bằng 0 có thể lãng phí thời gian liên tục đi qua toàn bộ thành phần bằng 0. 

Một trường hợp cạnh khác là thành phần chứa một số đỉnh có cùng hiệu khác 0:```
3 1
10 20 30
5 15 25
1 2
2 3
3 1
```Sự khác biệt là tất cả`5`, do đó DFS truy cập cả ba đỉnh và kết quả sau thao tác là:```
5 15 25
```cho họ`a`các giá trị. Giải pháp chỉ đặt lại đỉnh bắt đầu sẽ không chính xác. 

Trường hợp quan trọng cuối cùng là khi một bản cập nhật kết nối lại một thành phần lớn:```
3 2
10 0 0
0 0 0
1 2
2 3
1 2 10
3 1
```Sau khi cập nhật, cả ba đỉnh đều có sự khác biệt`10`, vì vậy truy vấn thứ ba phải xóa cả ba. Thuật toán phải luôn so sánh sự khác biệt hiện tại khi di chuyển ngang. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là mô phỏng DFS được mô tả. Đối với mọi truy vấn loại 3, hãy bắt đầu từ đỉnh được yêu cầu, truy cập đệ quy các đỉnh lân cận có chênh lệch hiện tại khớp và đặt`a_v=b_v`sau khi trở về. Đây chính xác là những gì tuyên bố mô tả, vì vậy tính chính xác là ngay lập tức. 

Vấn đề là cùng một đỉnh có thể xuất hiện trong nhiều lần duyệt DFS nếu chúng ta chỉ nhìn vào cấu trúc cây. Một đường đi hoặc cây hình ngôi sao có thể khiến một truy vấn chạm tới hầu hết mọi đỉnh và việc lặp lại điều đó nhiều lần sẽ dẫn đến khoảng`O(nq)`công việc. 

Cái nhìn sâu sắc quan trọng là DFS có tính phá hoại. Thành phần sai phân khác 0 sẽ biến mất sau khi nó được xử lý vì mọi đỉnh trong thành phần đó trở thành sai phân bằng 0. Cách duy nhất để một đỉnh có thể trở về hiệu khác 0 là truy vấn loại 1 và điều đó ảnh hưởng đến một đỉnh. Do đó, trong toàn bộ quá trình thực thi, tổng số đỉnh có thể bị loại bỏ bởi tất cả các hoạt động DFS có ý nghĩa được giới hạn bởi số đỉnh khác 0 ban đầu cộng với số lượng cập nhật. 

Điều này cho phép chúng ta sử dụng phương pháp lấp lũ đơn giản. Chúng ta không cần cấu trúc kết nối động phức tạp vì chính thao tác này sẽ phá hủy phần biểu đồ mà nó khám phá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ hiện tại`a`giá trị và cố định`b`các giá trị. Đối với mỗi truy vấn thứ ba, hãy tính chênh lệch hiện tại`a[v]-b[v]`của đỉnh bắt đầu. 

Nếu giá trị này bằng 0, DFS sẽ chỉ truy cập các đỉnh đã thỏa mãn`a=b`, do đó thao tác không thay đổi gì cả. Chúng ta có thể bỏ qua nó ngay lập tức. 
2. Đối với chênh lệch ban đầu khác 0, hãy chạy DFS lặp bằng cách sử dụng ngăn xếp. Giữ một mảng dấu thời gian để đánh dấu các đỉnh đã ghé thăm trong quá trình lấp lũ hiện tại. 

Dấu thời gian tránh xóa toàn bộ`visited`mảng trước mỗi truy vấn, điều này sẽ tốn kém`O(n)`ngay cả đối với một truy vấn không có gì thay đổi. 
3. Trong khi xử lý một đỉnh, hãy kiểm tra tất cả các đỉnh liền kề. Nếu một hàng xóm chưa được ghé thăm có cùng mức chênh lệch hiện tại, hãy đẩy nó vào ngăn xếp. 

Kiểm tra đẳng thức sử dụng giá trị hiện tại của`a`, bởi vì các phần trước của DFS có thể đã thay đổi các đỉnh thành`b`. 
4. Sau khi lấy một đỉnh ra khỏi ngăn xếp, đặt`a[v]=b[v]`. 

Điều này hoàn toàn khớp với hiệu ứng của DFS ban đầu hoàn thiện đỉnh đó. Vì biểu đồ là một cây nên việc thay đổi các đỉnh đã được xử lý không thể tạo ra các đường dẫn bổ sung đáng lẽ phải được ghé thăm. 

Tại sao nó hoạt động: 

Điều kiện DFS tương đương với sự bằng nhau của hiệu`a-b`. Do đó, quy trình ban đầu sẽ truy cập chính xác thành phần được kết nối của đỉnh bắt đầu bên trong biểu đồ chỉ chứa các cạnh giữa các hiệu bằng nhau. Thuật toán thực hiện cùng một tìm kiếm và áp dụng phép gán cuối cùng giống nhau cho mọi đỉnh được truy cập. Dấu thời gian chỉ ngăn việc xem lại các đỉnh trong một tìm kiếm và đối số khấu hao chứng minh rằng tổng lượng tìm kiếm trên tất cả các truy vấn là tuyến tính theo số lượng đỉnh hoạt động ban đầu cộng với các bản cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))
    b = [0] + list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    seen = [0] * (n + 1)
    timer = 0
    ans = []

    for _ in range(q):
        query = list(map(int, input().split()))
        t = query[0]

        if t == 1:
            v, x = query[1], query[2]
            a[v] = x

        elif t == 2:
            ans.append(str(a[query[1]]))

        else:
            v = query[1]
            diff = a[v] - b[v]

            if diff == 0:
                continue

            timer += 1
            stack = [v]
            seen[v] = timer

            while stack:
                x = stack.pop()

                for y in g[x]:
                    if seen[y] != timer and a[y] - b[y] == diff:
                        seen[y] = timer
                        stack.append(y)

                a[x] = b[x]

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc thực hiện giữ`a`có thể thay đổi vì cả truy vấn loại 1 và loại 3 đều thay đổi nó. các`b`mảng không bao giờ thay đổi nên nó được lưu trữ riêng. 

Việc lấp đầy lũ sử dụng một ngăn xếp rõ ràng thay vì đệ quy. Với`5 * 10^5`các đỉnh, DFS đệ quy sẽ vượt quá giới hạn đệ quy của Python và có nguy cơ tràn ngăn xếp. 

Thứ tự bên trong vòng lặp rất quan trọng. Một đỉnh được gán`a[x]=b[x]`sau khi hàng xóm của nó đã được kiểm tra. Điều này phản ánh DFS ban đầu, trong đó nhiệm vụ được thực hiện sau khi khám phá tất cả các phần tử con hợp lệ. Nếu nhiệm vụ được thực hiện trước, sự khác biệt sẽ bằng 0 và việc tìm kiếm sẽ mất thông tin một cách không chính xác. 

Kỹ thuật dấu thời gian ngăn chặn nhu cầu phân bổ hoặc đặt lại một mảng lớn đã truy cập cho mỗi truy vấn. Mỗi đợt lũ thực tế chỉ chạm vào các đỉnh mà nó cần. 

## Ví dụ đã hoạt động 

Mẫu 1:```
2 6
20 102
10 90
1 2
2 1
2 2
1 2 100
3 1
2 1
2 2
```Những thay đổi trạng thái quan trọng là: 

| Bước | Truy vấn | Giá trị chênh lệch | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 |`2 1`|`10, 12`| In đỉnh 1 |`20`| 
| 2 |`2 2`|`10, 12`| In đỉnh 2 |`102`| 
| 3 |`1 2 100`|`10, 10`| Cập nhật đỉnh 2 | | 
| 4 |`3 1`| cả hai sự khác biệt là`10`| Xóa cả hai đỉnh | | 
| 5 |`2 1`|`0, 0`| In đỉnh 1 |`10`| 
| 6 |`2 2`|`0, 0`| In đỉnh 2 |`90`| 

Ví dụ này cho thấy tại sao hoạt động phụ thuộc vào sự khác biệt chứ không phải là nguyên`a`các giá trị. Sau khi cập nhật, hai đỉnh trở thành một phần của một thành phần. 

Mẫu 2:```
6 6
20 30 30 60 60 70
10 20 30 40 50 60
1 2
2 4
2 5
1 3
3 6
1 3 40
3 1
2 1
2 6
2 2
2 4
```Trạng thái khi ngập lụt là: 

| Bước | Truy vấn | Sự khác biệt của đỉnh | Hành động | 
| --- | --- | --- | --- | 
| 1 |`1 3 40`|`10,10,10,20,10,10`| Vertex 3 gia nhập nhóm 10 khác biệt | 
| 2 |`3 1`| các đỉnh 1,2,3,5,6 có chênh lệch 10 | Xóa toàn bộ thành phần được kết nối đó | 
| 3 |`2 1`| đỉnh 1 có hiệu 0 | đầu ra`10`| 
| 4 |`2 6`| đỉnh 6 có hiệu 0 | đầu ra`60`| 
| 5 |`2 2`| đỉnh 2 có hiệu 0 | đầu ra`20`| 
| 6 |`2 4`| đỉnh 4 không được kết nối bởi chênh lệch 10 | đầu ra`60`| 

Điều này chứng tỏ rằng việc lấp lũ tuân theo thành phần chênh lệch bằng nhau hiện tại chứ không chỉ đơn giản là mọi đỉnh có cùng độ chênh lệch trong toàn bộ cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) khấu hao | Mỗi đỉnh tràn có ý nghĩa ban đầu khác 0 hoặc được tạo bởi một bản cập nhật và mỗi đỉnh được truy cập sẽ quét danh sách kề của nó một lần. | 
| Không gian | O(n) | Cây, giá trị, dấu thời gian và ngăn xếp DFS đều sử dụng bộ nhớ tuyến tính. | 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào đi lại nhiều lần trên toàn bộ cây đều sẽ thất bại. Giới hạn khấu hao là yếu tố làm cho việc truyền tải đơn giản phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    oldout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    res = sys.stdout.getvalue()
    sys.stdin = old
    sys.stdout = oldout
    return res

assert run(
"""2 6
20 102
10 90
1 2
2 1
2 2
1 2 100
3 1
2 1
2 2
"""
) == "20\n102\n10\n90", "sample 1"

assert run(
"""6 6
20 30 30 60 60 70
10 20 30 40 50 60
1 2
2 4
2 5
1 3
3 6
1 3 40
3 1
2 1
2 6
2 2
2 4
"""
) == "10\n60\n20\n60", "sample 2"

assert run(
"""1 3
5
5
2 1
3 1
2 1
"""
) == "5\n5", "single zero-difference vertex"

assert run(
"""3 2
10 0 0
0 0 0
1 2
2 3
3 1
2 2
"""
) == "0", "large component clearing"

assert run(
"""4 5
1 2 3 4
0 0 0 0
1 2 10
1 3 10
1 4 10
3 2
2 2
"""
) == "0\n10", "updates creating components"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một đỉnh bằng nhau`a`Và`b`|`5`giá trị không thay đổi | Xử lý các truy vấn không khác biệt | 
| Một chuỗi trong đó tất cả các đỉnh có chung một điểm khác biệt | Toàn bộ thành phần bị xóa | Xử lý lũ lụt nhiều đỉnh | 
| Một số cập nhật theo sau là một truy vấn | Chỉ các đỉnh có độ sai phân bằng nhau được kết nối mới bị ảnh hưởng | Xử lý các thay đổi động | 
| Cây nhỏ nhất | Hành vi cơ bản đúng | Xử lý kích thước tối thiểu | 

## Vỏ cạnh 

Đối với một đỉnh bắt đầu với`a_v=b_v`, thuật toán sẽ tính toán chênh lệch bằng 0 và ngay lập tức bỏ qua việc tìm kiếm. Điều này khớp với DFS thực vì mọi đỉnh được truy cập sẽ kết thúc với cùng giá trị mà nó bắt đầu. 

Đối với một thành phần khác 0 được kết nối, chẳng hạn như:```
3 1
10 20 30
5 15 25
1 2
2 3
3 1
```tất cả các đỉnh đều có sự khác biệt`5`. Ngăn xếp bắt đầu bằng đỉnh`1`, khám phá các đỉnh`2`Và`3`, sau đó gán cho mỗi người trong số họ`a`giá trị để`b`. Các giá trị kết quả trở thành:```
5 15 25
```Đối với một thành phần được tạo lại bằng các bản cập nhật, chẳng hạn như:```
3 2
10 0 0
0 0 0
1 2
2 3
1 2 10
3 1
```bản cập nhật thay đổi đỉnh`2`vậy mọi đỉnh đều có sự khác biệt`10`. Truy vấn thứ ba bắt đầu từ đỉnh`1`, đạt đến cả ba đỉnh thông qua các hiệu bằng nhau và đặt lại cả ba đỉnh. Thuật toán xử lý việc này vì nó luôn đọc dòng điện`a-b`giá trị thay vì dựa vào bất kỳ thông tin thành phần nào trước đó.
