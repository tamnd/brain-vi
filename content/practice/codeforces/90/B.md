---
title: "CF 90B - Ô chữ Châu Phi"
description: "Chúng ta được cung cấp một mạng lưới nhỏ các chữ cái viết thường. Một ô chỉ tồn tại nếu chữ cái của nó là duy nhất cả trong hàng và bên trong cột của nó. Nếu ký tự tương tự xuất hiện ở nơi khác trong cùng hàng thì ô đó sẽ bị xóa."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 90
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 74 (Div. 2 Only)"
rating: 1100
weight: 90
solve_time_s: 104
verified: true
draft: false
---

[CF 90B - Ô chữ châu Phi](https://codeforces.com/problemset/problem/90/B) 

**Đánh giá:** 1100 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng lưới nhỏ các chữ cái viết thường. Một ô chỉ tồn tại nếu chữ cái của nó là duy nhất cả trong hàng và bên trong cột của nó. Nếu ký tự tương tự xuất hiện ở nơi khác trong cùng hàng thì ô đó sẽ bị xóa. Điều tương tự cũng xảy ra nếu ký tự xuất hiện ở một nơi khác trong cùng một cột. 

Sau khi loại bỏ đồng thời tất cả các ô không hợp lệ, chúng ta đọc các chữ cái còn lại theo hàng, từ trên xuống dưới và từ trái sang phải rồi ghép chúng thành đáp án cuối cùng. 

Kích thước lưới tối đa là 100 × 100 nên tổng số ô nhiều nhất là 10.000. Ngay cả các thuật toán kiểm tra toàn bộ hàng và cột cho mỗi ô ở đây vẫn đủ nhanh, bởi vì các thao tác 10.000 × 200 chỉ là khoảng hai triệu so sánh. Điều đó có nghĩa là chúng ta không cần các cấu trúc dữ liệu nâng cao hoặc các thủ thuật tiền xử lý. 

Thách thức chính không phải là hiệu suất mà là việc triển khai chính xác điều kiện lọc. 

Một lỗi phổ biến là chỉ kiểm tra hàng hoặc chỉ cột. Một tế bào chỉ tồn tại nếu cả hai điều kiện được giữ đồng thời. 

Hãy xem xét ví dụ này:```
2 2
aa
bc
```hai`a`các ô không hợp lệ vì chúng lặp lại bên trong hàng đầu tiên. 

Đầu ra đúng là:```
bc
```Một sai lầm dễ mắc phải khác là xóa từng chữ cái một thay vì đồng thời. Vấn đề cho biết tất cả các chữ cái lặp lại đều bị gạch bỏ cùng một lúc, vì vậy việc xóa một ô không thể làm cho ô khác đột nhiên trở thành duy nhất. 

Ví dụ:```
3 1
a
a
a
```Cả ba ô đều không hợp lệ vì cột chứa các ký tự lặp lại`a`. 

Về mặt logic, đầu ra đúng là một chuỗi trống, nhưng câu lệnh đảm bảo có ít nhất một ký tự còn sót lại trong các trường hợp kiểm thử hợp lệ. 

Một trường hợp tinh tế hơn là khi một chữ cái là duy nhất ở hàng của nó nhưng được lặp lại ở cột của nó.```
2 2
ab
cb
```hai`b`các ô phải được loại bỏ vì chúng có chung một cột. 

Đầu ra đúng là:```
ac
```Việc triển khai bất cẩn chỉ kiểm tra các hàng sẽ giữ sai cả hai`b`tế bào. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu tuân theo định nghĩa trực tiếp. Đối với mỗi ô, hãy quét toàn bộ hàng để xem liệu chữ cái tương tự có xuất hiện ở nơi nào khác hay không. Sau đó quét toàn bộ cột để tìm tình trạng tương tự. Nếu cả hai lần quét đều không tìm thấy ký tự bằng nhau khác, hãy thêm ô vào câu trả lời. 

Điều này hoạt động vì lưới nhỏ. Giả sử lưới có kích thước 100 × 100. Đối với mỗi ô trong số 10.000 ô, chúng tôi quét tối đa 100 vị trí hàng và 100 vị trí cột. Điều đó mang lại khoảng hai triệu so sánh, điều này hoàn toàn ổn trong thời gian giới hạn. 

Cách tiếp cận vũ phu đã diễn ra một cách thoải mái, do đó không cần phải tối ưu hóa phức tạp. Tuy nhiên, chúng ta có thể tổ chức logic rõ ràng hơn bằng cách đếm tần số trước. 

Quan sát quan trọng là một ô tồn tại chính xác khi ký tự của nó xuất hiện một lần trong hàng và một lần trong cột. Thay vì quét lại hàng và cột liên tục, chúng ta có thể tính toán trước tần số ký tự cho mỗi hàng và mỗi cột. 

Đối với mỗi hàng, hãy đếm số lần mỗi chữ cái xuất hiện. Làm tương tự cho mỗi cột. Sau đó một tế bào`(i, j)`sống sót nếu:```
row_count[i][grid[i][j]] == 1
and
col_count[j][grid[i][j]] == 1
```Điều này biến việc quét lặp đi lặp lại thành kiểm tra liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × m × (n + m)) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n×m) | O(n × 26 + m × 26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới thành danh sách các chuỗi. 

Chúng tôi cần quyền truy cập ngẫu nhiên vào mọi ô vì mỗi ký tự phải được kiểm tra theo tần số hàng và cột của nó. 
2. Xây dựng bảng tần số cho các hàng. 

Đối với mỗi hàng, hãy đếm số lần mỗi chữ cái viết thường xuất hiện. Vì chữ cái chỉ`'a'`ĐẾN`'z'`, một mảng có kích thước 26 là đủ. 
3. Xây dựng bảng tần số cho cột. 

Đối với mỗi cột, hãy đếm số lần mỗi chữ cái viết thường xuất hiện trong cột đó. 
4. Duyệt từng ô`(i, j)`theo thứ tự hàng lớn. 

Thứ tự duyệt này khớp với thứ tự đầu ra được yêu cầu, do đó các ký tự còn sót lại có thể được thêm trực tiếp vào chuỗi câu trả lời. 
5. Đối với nhân vật hiện tại`c`, hãy kiểm tra xem nó có xuất hiện đúng một lần trong hàng và đúng một lần trong cột hay không. 

Nếu cả hai đều bằng 1 thì giữ nguyên ký tự. Nếu không hãy loại bỏ nó. 
6. In các ký tự còn sót lại được nối. 

### Tại sao nó hoạt động 

Thuật toán mã hóa trực tiếp điều kiện từ câu lệnh vấn đề. Một ký tự chỉ tồn tại khi không có ký tự ngang bằng nào khác tồn tại trong hàng của nó và không có ký tự ngang bằng nào khác tồn tại trong cột của nó. 

Bảng tần số hàng cho chúng ta biết chính xác có bao nhiêu chữ cái bằng nhau xuất hiện trong hàng đó. Bảng tần số cột thực hiện tương tự cho cột. Nếu cả hai số đếm đều bằng 1 thì ô hiện tại là duy nhất theo cả hai hướng và phải được giữ nguyên. Nếu một trong hai số lớn hơn 1, thì có một số ký tự trùng khớp và ô phải bị gạch bỏ. 

Bởi vì mỗi ô được kiểm tra độc lập với tần số ban đầu nên tất cả việc loại bỏ đều diễn ra đồng thời, chính xác theo yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    row_count = [[0] * 26 for _ in range(n)]
    col_count = [[0] * 26 for _ in range(m)]

    # Count frequencies in rows
    for i in range(n):
        for j in range(m):
            idx = ord(grid[i][j]) - ord('a')
            row_count[i][idx] += 1

    # Count frequencies in columns
    for j in range(m):
        for i in range(n):
            idx = ord(grid[i][j]) - ord('a')
            col_count[j][idx] += 1

    ans = []

    # Collect surviving characters
    for i in range(n):
        for j in range(m):
            idx = ord(grid[i][j]) - ord('a')

            if row_count[i][idx] == 1 and col_count[j][idx] == 1:
                ans.append(grid[i][j])

    print("".join(ans))

solve()
```Phần đầu tiên đọc lưới và chuẩn bị các mảng tần số. Sử dụng mảng có độ dài 26 đơn giản và nhanh hơn so với từ điển vì bảng chữ cái đã cố định. 

Vòng đếm hàng ghi lại số lần mỗi ký tự xuất hiện trong mỗi hàng. Vòng đếm cột thực hiện tương tự với các cột. Việc phân chia các bước này giúp logic đơn giản và tránh trộn lẫn hai khái niệm độc lập. 

Việc truyền tải cuối cùng tuân theo thứ tự hàng chính vì đầu ra phải giữ nguyên thứ tự đọc ban đầu. Chi tiết này rất dễ bị bỏ qua. Sắp xếp các ký tự còn sót lại hoặc lưu trữ chúng trong cấu trúc khác sẽ tạo ra câu trả lời sai. 

Điều kiện:```
row_count[i][idx] == 1 and col_count[j][idx] == 1
```là bản dịch chính xác của quy tắc vấn đề. Cả hai kiểm tra đều được yêu cầu. sử dụng`or`ở đây sẽ giữ không chính xác các ô duy nhất chỉ theo một hướng. 

Việc triển khai không có vấn đề về ranh giới vì mọi vòng lặp đều nằm trong kích thước lưới và các chỉ số ký tự luôn nằm trong khoảng từ 0 đến 25. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
cba
bcd
cbc
```Tần số hàng: 

| Hàng | Tần số | 
| --- | --- | 
|`cba`| c:1 b:1 a:1 | 
|`bcd`| b:1 c:1 d:1 | 
|`cbc`| c:2 b:1 | 

Tần số cột: 

| Cột | Nhân vật | Tần số | 
| --- | --- | --- | 
| 0 | c b c | c:2 b:1 | 
| 1 | b c b | b:2 c:1 | 
| 2 | a d c | a:1 d:1 c:1 | 

Kiểm tra tế bào: 

| Tế bào | Nhân vật | Hàng độc đáo | Cột Duy Nhất | Giữ | 
| --- | --- | --- | --- | --- | 
| (0,0) | c | Có | Không | Không | 
| (0,1) | b | Có | Không | Không | 
| (0,2) | một | Có | Có | Có | 
| (1,0) | b | Có | Có | Có | 
| (1,1) | c | Có | Có | Có | 
| (1,2) | d | Có | Có | Có | 
| (2,0) | c | Không | Không | Không | 
| (2,1) | b | Có | Không | Không | 
| (2,2) | c | Không | Có | Không | 

Câu trả lời cuối cùng:```
abcd
```Dấu vết này cho thấy tính duy nhất phải được giữ đồng thời ở cả hai chiều. 

### Ví dụ 2 

đầu vào:```
2 2
ab
cb
```Tần số hàng: 

| Hàng | Tần số | 
| --- | --- | 
|`ab`| a:1 b:1 | 
|`cb`| c:1 b:1 | 

Tần số cột: 

| Cột | Nhân vật | Tần số | 
| --- | --- | --- | 
| 0 | một c | a:1 c:1 | 
| 1 | b b | b:2 | 

Kiểm tra tế bào: 

| Tế bào | Nhân vật | Hàng độc đáo | Cột Duy Nhất | Giữ | 
| --- | --- | --- | --- | --- | 
| (0,0) | một | Có | Có | Có | 
| (0,1) | b | Có | Không | Không | 
| (1,0) | c | Có | Có | Có | 
| (1,1) | b | Có | Không | Không | 

Câu trả lời cuối cùng:```
ac
```Ví dụ này chứng tỏ tại sao chỉ kiểm tra các hàng là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n×m) | Mỗi ô được xử lý một số lần không đổi | 
| Không gian | O(n × 26 + m × 26) | Bảng tần số cho hàng và cột | 

Với tối đa 10.000 ô, giải pháp dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ rất nhỏ vì chỉ lưu trữ các mảng tần số cho chữ thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        grid = [input().strip() for _ in range(n)]

        row_count = [[0] * 26 for _ in range(n)]
        col_count = [[0] * 26 for _ in range(m)]

        for i in range(n):
            for j in range(m):
                idx = ord(grid[i][j]) - ord('a')
                row_count[i][idx] += 1

        for j in range(m):
            for i in range(n):
                idx = ord(grid[i][j]) - ord('a')
                col_count[j][idx] += 1

        ans = []

        for i in range(n):
            for j in range(m):
                idx = ord(grid[i][j]) - ord('a')

                if row_count[i][idx] == 1 and col_count[j][idx] == 1:
                    ans.append(grid[i][j])

        return "".join(ans)

    return solve()

# provided sample
assert run(
"""3 3
cba
bcd
cbc
"""
) == "abcd", "sample 1"

# minimum size
assert run(
"""1 1
a
"""
) == "a", "single cell"

# repeated row
assert run(
"""1 4
abca
"""
) == "bc", "row duplicates"

# repeated column
assert run(
"""4 1
a
b
a
c
"""
) == "bc", "column duplicates"

# all unique
assert run(
"""2 3
abc
def
"""
) == "abcdef", "all survive"

# mixed duplicates
assert run(
"""2 2
ab
cb
"""
) == "ac", "column duplicate removal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`lưới |`a`| Đầu vào hợp lệ tối thiểu | 
|`abca`|`bc`| Xử lý trùng lặp hàng | 
| Cột lặp lại đơn |`bc`| Xử lý trùng lặp cột | 
| Tất cả lưới độc đáo |`abcdef`| Mọi tế bào đều tồn tại | 
| Trường hợp trùng lặp hỗn hợp |`ac`| Cả điều kiện hàng và cột cùng nhau | 

## Vỏ cạnh 

Hãy xem xét một hàng có các chữ cái lặp lại:```
1 4
abca
```Tần số hàng là:```
a:2, b:1, c:1
```Tất cả tần số cột là 1 vì chỉ có một hàng. 

Thuật toán loại bỏ cả hai`a`các ô vì số hàng của chúng lớn hơn 1. Các chữ cái còn lại là`b`Và`c`, do đó đầu ra trở thành:```
bc
```Điều này xác nhận rằng các bản sao trong cùng một hàng sẽ vô hiệu hóa mọi lần xuất hiện trùng khớp. 

Bây giờ hãy xem xét trường hợp trùng lặp cột:```
4 1
a
b
a
c
```Tần số cột là:```
a:2, b:1, c:1
```Mỗi hàng chỉ chứa một ký tự nên tần số của hàng đều bằng 1. 

Thuật toán loại bỏ cả hai`a`các ô vì số cột của chúng lớn hơn 1. Kết quả đầu ra trở thành:```
bc
```Điều này xác minh rằng chỉ cần lặp lại cột là đủ để loại bỏ một ô. 

Cuối cùng, hãy xem xét trường hợp tồn tại sự trùng lặp theo cả hai hướng:```
3 3
aaa
aba
aaa
```Trung tâm`b`là duy nhất trong hàng và cột của nó, trong khi mọi`a`lặp lại nhiều lần. 

Thuật toán chỉ giữ lại ô trung tâm, tạo ra:```
b
```Điều này chứng tỏ rằng các lần kiểm tra là độc lập và cách ly chính xác các tế bào còn sống.
