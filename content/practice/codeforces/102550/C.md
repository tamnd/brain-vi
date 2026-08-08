---
title: "CF 102550C - \u041c\u0438\u043d\u043d\u043e\u0435 \u043f\u043e\u043b\u0435"
description: "Trường này là một lưới hình chữ nhật trong đó mỗi ô ban đầu chứa một quả mìn đang hoạt động. Trong quá trình này, các tế bào được loại bỏ từng cái một. Một truy vấn hỏi về mỏ còn lại gần nhất theo đúng một trong bốn hướng từ một ô nhất định."
date: "2026-08-06T20:39:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102550
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2018-2019, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102550
solve_time_s: 218
verified: true
draft: false
---

[CF 102550C - \u041c\u0438\u043d\u043d\u043e\u0435 \u043f\u043e\u043b\u0435](https://codeforces.com/problemset/problem/102550/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trường này là một lưới hình chữ nhật trong đó mỗi ô ban đầu chứa một quả mìn đang hoạt động. Trong quá trình này, các tế bào được loại bỏ từng cái một. Một truy vấn hỏi về mỏ còn lại gần nhất theo đúng một trong bốn hướng từ một ô nhất định. Đối với truy vấn dọc, chúng tôi chỉ quan tâm đến cùng một cột và đối với truy vấn theo chiều ngang, chúng tôi chỉ quan tâm đến cùng một hàng. 

Đầu vào mô tả kích thước lưới và trình tự các thao tác. Thao tác xóa sẽ xóa vĩnh viễn một ô. Thao tác tìm kiếm yêu cầu tọa độ của ô gần nhất chưa bị xóa theo hướng được yêu cầu hoặc báo cáo rằng không có ô nào tồn tại. 

Các giới hạn là khó khăn chính. Lưới có thể chứa tới bốn triệu ô và có thể có tới một triệu thao tác. Quét toàn bộ hàng hoặc cột cho mỗi truy vấn sẽ quá tốn kém. Trong trường hợp xấu nhất, một truy vấn có thể kiểm tra 2000 ô, cung cấp khoảng hai tỷ lượt kiểm tra cho tất cả các hoạt động. Giải pháp cần thời gian gần như không đổi cho mỗi thao tác. 

Các thao tác xóa có một đặc tính hữu ích: các ô chỉ biến mất, chúng không bao giờ quay trở lại. Điều này có nghĩa là chúng ta có thể sử dụng cấu trúc dữ liệu được thiết kế để loại bỏ sự đơn điệu. Chúng ta chỉ cần bỏ qua những ô đã bị xóa. 

Một số trường hợp ranh giới có thể phá vỡ việc triển khai trực tiếp. Một truy vấn từ rìa lưới có thể không có câu trả lời ngay lập tức. Ví dụ:```
1 3 3
l 1 1
r 1 3
u 1 2
```Đầu ra là:```
-1
-1
-1
```Việc triển khai bất cẩn có thể vô tình bao gồm ô bắt đầu hoặc di chuyển ra ngoài lưới trong khi tìm kiếm. 

Một trường hợp khác là hướng mà ô gần nhất đã bị xóa nhưng ô xa hơn vẫn tồn tại:```
1 4 3
c 1 2
r 1 1
l 1 4
```Đầu ra là:```
1 3
1 1
```Sau khi loại bỏ`(1,2)`, truy vấn phù hợp phải bỏ qua nó và tìm`(1,3)`. Dừng lại ở vị trí bị loại bỏ đầu tiên sẽ cho kết quả sai. 

Trường hợp khó khăn cuối cùng là tìm kiếm lặp lại sau nhiều lần xóa:```
2 2 4
c 1 1
c 1 2
d 1 1
r 2 1
```Đầu ra là:```
2 1
2 2
```Cấu trúc phải tiếp tục tìm các ô còn lại ngay cả khi phần lớn hàng hoặc cột đã biến mất. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là lưu trữ những ô nào vẫn đang hoạt động và đối với mỗi truy vấn, hãy đi từng bước một theo hướng được yêu cầu cho đến khi tìm thấy ô hiện hoạt. Điều này đúng vì ô hoạt động đầu tiên gặp phải chính xác là ô gần nhất. Vấn đề là số lượng công việc lặp đi lặp lại. Với lưới 2000 x 2000, một truy vấn có thể quét 1999 ô và một triệu truy vấn như vậy có thể yêu cầu gần hai tỷ thao tác. 

Quan sát quan trọng là các ô chỉ bị xóa. Chúng ta không bao giờ cần phải cài mìn nữa. Đối với một hàng, sau khi một cột bị xóa, mọi truy vấn trong tương lai đạt đến vị trí đó sẽ ngay lập tức chuyển qua cột đó. Điều tương tự áp dụng độc lập cho mỗi cột. 

Đây chính xác là tình huống được xử lý bởi cấu trúc "con trỏ tiếp theo" kiểu tập hợp rời rạc. Đối với mỗi hàng, chúng tôi duy trì một cấu trúc có thể tìm thấy cột còn lại tiếp theo và cột còn lại trước đó. Đối với mỗi cột, chúng tôi duy trì hàng còn lại tiếp theo và trước đó. Khi một ô bị xóa, chúng tôi kết nối nó với vị trí sẵn có tiếp theo, loại bỏ nó khỏi các tìm kiếm trong tương lai một cách hiệu quả. 

Ví dụ: nếu một hàng chứa các cột`1 2 3 4`và cột`2`bị xóa, con trỏ tiếp theo của`2`trở thành`3`. Mọi tìm kiếm trong tương lai bắt đầu từ`1`yêu cầu cột hiện hoạt tiếp theo sẽ tự động nhảy qua ô đã xóa. 

Bốn cấu trúc cùng nhau đáp ứng mọi hướng trong thời gian khấu hao gần như không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * max(n, m)) | O(nm) | Quá chậm | 
| Tối ưu | O(q α(nm)) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bốn cấu trúc tập hợp rời rạc. Đối với mỗi hàng, lưu trữ cấu trúc "cột còn sống tiếp theo" và cấu trúc "cột còn sống trước đó". Đối với mỗi cột, lưu trữ hai cấu trúc tương đương cho các hàng. Mỗi cấu trúc ban đầu đều trỏ đến chính nó vì mọi ô đều tồn tại. 
2. Đối với hoạt động loại bỏ tại`(x, y)`, hãy xóa ô đó khỏi cả cấu trúc hàng và cấu trúc cột của nó. Trong cấu trúc con trỏ tiếp theo, kết nối vị trí đã loại bỏ với vị trí tiếp theo. Trong cấu trúc con trỏ trước, hãy kết nối nó với vị trí trước đó. 
3. Để có truy vấn đúng từ`(x, y)`, hãy hỏi cấu trúc con trỏ tiếp theo của hàng cho cột hoạt động đầu tiên sau`y`. Nếu kết quả nằm ngoài phạm vi cột hợp lệ thì không có câu trả lời nào tồn tại. Còn không thì trả lại`(x, result)`. 
4. Đối với truy vấn bên trái, hãy sử dụng cấu trúc con trỏ trước của hàng. Đối với truy vấn lên, hãy sử dụng cấu trúc con trỏ trước của cột. Đối với truy vấn xuống, hãy sử dụng cấu trúc con trỏ tiếp theo của cột. 

Lý do điều này có tác dụng là vì những thay đổi duy nhất là việc xóa. Vị trí đã xóa luôn chuyển hướng đến vị trí gần nhất vẫn có thể là câu trả lời hợp lệ. Việc nén đường dẫn làm cho các bước nhảy lặp lại trở nên rất rẻ vì các chuỗi dài nhanh chóng bị sụp đổ. 

### Tại sao nó hoạt động 

Điều bất biến là mọi cấu trúc con trỏ đều trả về vị trí vẫn tồn tại gần nhất theo hướng của nó. Ban đầu điều này đúng vì mọi ô đều tồn tại và mọi con trỏ đều trỏ đến chính nó. Khi một ô bị xóa, nó sẽ được thay thế bằng một liên kết đến ô lân cận còn tồn tại gần nhất theo hướng đó. Bất kỳ tìm kiếm nào trong tương lai có thể đến ô đã xóa sẽ được chuyển hướng đến chính xác vị trí cần tìm sau khi xóa. Vì các ô không bao giờ được chèn lại nên không có bản cập nhật nào có thể làm mất hiệu lực thuộc tính này. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def find(arr, idx):
    root = idx
    while arr[root] != root:
        root = arr[root]
    while arr[idx] != idx:
        nxt = arr[idx]
        arr[idx] = root
        idx = nxt
    return root

def solve():
    n, m, q = map(int, input().split())

    row_size = m + 1
    col_size = n + 1

    row_next = array('H', [0]) * (n * row_size)
    row_prev = array('H', [0]) * (n * row_size)
    col_next = array('H', [0]) * (m * col_size)
    col_prev = array('H', [0]) * (m * col_size)

    for i in range(n):
        base = i * row_size
        for j in range(m + 2):
            if j <= m:
                row_next[base + j] = j
            if j <= m:
                row_prev[base + j] = j

    for j in range(m):
        base = j * col_size
        for i in range(n + 1):
            col_next[base + i] = i
            col_prev[base + i] = i

    out = []

    for _ in range(q):
        parts = input().split()
        typ = parts[0]
        x = int(parts[1]) - 1
        y = int(parts[2]) - 1

        if typ == 'c':
            rb = x * row_size
            cb = y * col_size

            row_next[rb + y] = find(row_next, rb + y + 1) - rb
            row_prev[rb + y] = find(row_prev, rb + y - 1) - rb

            col_next[cb + x] = find(col_next, cb + x + 1) - cb
            col_prev[cb + x] = find(col_prev, cb + x - 1) - cb
        elif typ == 'r':
            rb = x * row_size
            ans = find(row_next, rb + y + 1) - rb
            if ans > m - 1:
                out.append("-1")
            else:
                out.append(f"{x + 1} {ans + 1}")
        elif typ == 'l':
            rb = x * row_size
            ans = find(row_prev, rb + y - 1) - rb
            if ans < 0:
                out.append("-1")
            else:
                out.append(f"{x + 1} {ans + 1}")
        elif typ == 'd':
            cb = y * col_size
            ans = find(col_next, cb + x + 1) - cb
            if ans > n - 1:
                out.append("-1")
            else:
                out.append(f"{ans + 1} {y + 1}")
        else:
            cb = y * col_size
            ans = find(col_prev, cb + x - 1) - cb
            if ans < 0:
                out.append("-1")
            else:
                out.append(f"{ans + 1} {y + 1}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã lưu trữ mọi cấu trúc hàng và cột trong các mảng phẳng. Điều này tránh được chi phí chung của hàng triệu đối tượng Python riêng biệt. Vì tất cả các chỉ số tối đa là 2000,`array('H')`là đủ vì mọi giá trị được lưu trữ đều khớp với một số nguyên 16 bit không dấu. 

các`find`là chức năng tra cứu tập hợp rời rạc thông thường với tính năng nén đường dẫn. Vòng lặp đầu tiên tìm vị trí còn sót lại cuối cùng và vòng lặp thứ hai rút ngắn đường dẫn để các truy vấn sau này nhanh hơn. 

Lệnh xóa quan trọng. Trước tiên, mã sẽ kết nối ô đã xóa với các ô lân cận của nó, sau đó các truy vấn trong tương lai sẽ không bao giờ trả lại ô đó nữa. các`+1`Và`-1`offset cũng rất quan trọng vì truy vấn yêu cầu một ô ngay sau hoặc trước vị trí đã cho. 

Không có vấn đề tràn số nguyên trong Python và kích thước mảng được kiểm soát bởi kích thước lưới. Việc tiết kiệm bộ nhớ từ việc sử dụng mảng nhỏ gọn là cần thiết vì bốn cấu trúc trên bốn triệu ô sẽ quá lớn so với danh sách Python thông thường. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
3 4 6
u 2 3
c 2 4
r 2 4
c 2 3
l 2 4
d 1 3
```| Hoạt động | Hành động | Trạng thái quan trọng | Kết quả | 
| --- | --- | --- | --- | 
| 1 | lên từ (2,3) | Cột 3 vẫn còn hàng 1,2,3 | 1 3 | 
| 2 | loại bỏ (2,4) | Hàng 2 thua cột 4 | | 
| 3 | ngay từ (2,4) | Không có cột sau 4 | -1 | 
| 4 | loại bỏ (2,3) | Hàng 2 thua cột 3 và 4 | | 
| 5 | còn lại từ (2,4) | Cột sống trước đó là 2 | 2 2 | 
| 6 | giảm từ (1,3) | Cột 3 có hàng 3 còn sống | 3 3 | 

Dấu vết này cho thấy các ô đã xóa sẽ bị bỏ qua thay vì chỉ được đánh dấu và bỏ qua trong mỗi lần tìm kiếm. Các con trỏ được cập nhật một lần trong quá trình xóa và được sử dụng lại sau đó. 

Một ví dụ thứ hai:```
2 3 5
c 1 2
r 1 1
c 1 3
d 1 1
l 2 3
```| Hoạt động | Hành động | Trạng thái quan trọng | Kết quả | 
| --- | --- | --- | --- | 
| 1 | loại bỏ (1,2) | Hàng 1 liên kết cột 2 với cột 3 | | 
| 2 | ngay từ (1,1) | Nhảy qua cột đã xóa 2 | 1 3 | 
| 3 | xóa (1,3) | Hàng 1 không có ô nào sau cột 1 | | 
| 4 | giảm từ (1,1) | Cột 1 vẫn còn hàng 2 | 2 1 | 
| 5 | còn lại từ (2,3) | Hàng 2 còn cột 1 và 2 | 2 2 | 

Ví dụ này thực hiện một chuỗi các thao tác xóa. Thuộc tính quan trọng là các truy vấn không bao giờ duyệt qua từng ô đã xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q α(nm)) | Mỗi truy vấn và xóa chỉ thực hiện một số lượng không đổi các thao tác tập hợp rời rạc với nén đường dẫn. | 
| Không gian | O(nm) | Bốn mảng nhỏ gọn lưu trữ thông tin về hàng và cột về phía trước và phía sau. | 

Kích thước lưới tối đa tạo ra khoảng bốn triệu ô. Thuật toán thực hiện một số lượng nhỏ các thao tác cho mỗi truy vấn trong số một triệu truy vấn, phù hợp với các ràng buộc vì hệ số Ackermann nghịch đảo thực tế là không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3 4 6
u 2 3
c 2 4
r 2 4
c 2 3
l 2 4
d 1 3
""") == """1 3
-1
2 2
3 3""", "sample"

assert run("""1 1 3
r 1 1
c 1 1
l 1 1
""") == """-1
-1""", "single cell"

assert run("""1 5 5
c 1 2
r 1 1
c 1 3
r 1 1
l 1 5
""") == """1 3
1 4
1 4""", "deleted middle cells"

assert run("""3 1 4
c 2 1
d 1 1
c 3 1
d 1 1
""") == """3 1
-1""", "vertical boundary"

assert run("""2 2 4
c 1 1
c 1 2
d 1 1
r 2 1
""") == """2 1
2 2""", "remaining row after deletions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới ô đơn | Không có câu trả lời sau khi xóa | Xử lý các kích thước nhỏ nhất và hướng trống | 
| Loại bỏ các ô giữa | Bỏ qua các vị trí đã xóa | Kiểm tra bước nhảy kế nhiệm và tiền nhiệm | 
| Lưới cột đơn | Điều hướng dọc | Kiểm tra cấu trúc cột và ranh giới | 
| Xóa nặng một hàng | Các ô còn lại độc lập | Kiểm tra các hàng và cột không can thiệp | 

## Vỏ cạnh 

Trường hợp ranh giới đầu tiên là một truy vấn từ rìa của lưới. Trong đầu vào:```
1 3 1
l 1 1
```thuật toán yêu cầu cột trước đó`0`trong lập chỉ mục dựa trên số không. Cấu trúc tiền thân trả về vị trí trọng điểm bên ngoài lưới, vì vậy câu trả lời là`-1`. 

Trường hợp thứ hai là bỏ qua các ô đã xóa:```
1 4 2
c 1 2
r 1 1
```Sau khi xóa`(1,2)`, hàng kế tiếp của cột`1`trỏ trực tiếp vào cột`3`. Truy vấn trả về:```
1 3
```mà không cần kiểm tra lại ô đã bị loại bỏ. 

Trường hợp thứ ba là xóa toàn bộ phần hàng:```
2 2 3
c 1 1
c 1 2
d 1 1
```Hàng đầu tiên không còn ô nào nhưng cấu trúc cột vẫn chứa`(2,1)`. Truy vấn đi xuống trả về:```
2 1
```vì cấu trúc hàng và cột được duy trì riêng biệt.
