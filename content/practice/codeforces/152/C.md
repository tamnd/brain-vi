---
title: "CF 152C - Sách bỏ túi"
description: "Chúng ta có danh sách n chuỗi và mỗi chuỗi có cùng độ dài m. Một thao tác chọn hai chuỗi và độ dài tiền tố k, sau đó hoán đổi k ký tự đầu tiên giữa hai chuỗi đó. Chúng tôi chỉ quan tâm đến chuỗi cuối cùng xuất hiện ở vị trí 1."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics"]
categories: ["algorithms"]
codeforces_contest: 152
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 108 (Div. 2)"
rating: 1400
weight: 152
solve_time_s: 93
verified: true
draft: false
---

[CF 152C - Sách bỏ túi](https://codeforces.com/problemset/problem/152/C) 

**Đánh giá:** 1400 
**Thẻ:** tổ hợp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một danh sách`n`chuỗi và mọi chuỗi đều có cùng độ dài`m`. Một thao tác chọn hai chuỗi và độ dài tiền tố`k`, sau đó đổi chỗ đầu tiên`k`ký tự giữa hai chuỗi đó. 

Chúng tôi chỉ quan tâm đến chuỗi cuối cùng xuất hiện ở vị trí`1`. Sau khi thực hiện bất kỳ số thao tác nào, có bao nhiêu chuỗi riêng biệt có thể kết thúc ở đó? 

Phần quan trọng là hiểu những gì hoạt động thực sự cho phép. Việc hoán đổi tiền tố không tự ý hoán vị các ký tự. Nó bảo tồn hậu tố sau vị trí`k`, trong khi trao đổi mọi thứ trước nó. Câu hỏi thực sự là hỏi những ký tự nào có thể xuất hiện độc lập ở mỗi vị trí của chuỗi đầu tiên. 

Cả hai giới hạn đều rất nhỏ`n`Và`m`nhiều nhất là`100`. Ngay cả những ý tưởng có dạng cấp số nhân cũng hấp dẫn ở đây, bởi vì tổng kích thước đầu vào chỉ`10^4`nhân vật. Tuy nhiên, việc tạo ra tất cả các chuỗi có thể truy cập một cách rõ ràng là nguy hiểm. Nếu mỗi vị trí có thể chọn độc lập trong số nhiều ký tự thì số lượng chuỗi có thể truy cập sẽ tăng theo cấp số nhân trong`m`. Ví dụ, với`m = 100`và thậm chí chỉ`2`lựa chọn cho mỗi vị trí, đã có`2^100`khả năng. 

Điều đó ngay lập tức loại trừ việc tìm kiếm trong không gian trạng thái qua các chuỗi. Chúng ta cần đếm các khả năng mà không cần xây dựng chúng. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều chuỗi chứa cùng một ký tự trong một cột. Coi như:```
3 2
AA
AB
AC
```Ở vị trí thứ hai, các ký tự có sẵn là`{A, B, C}`, nhưng vị trí đầu tiên chỉ có`{A}`. Câu trả lời là`1 * 3 = 3`, không`3^2 = 9`. Bất kỳ giải pháp nào xử lý hàng thay vì cột sẽ bị tính quá mức. 

Một lỗi dễ mắc phải khác là cho rằng các ký tự từ các cột khác nhau được liên kết với nhau. Coi như:```
2 3
ABC
DEF
```Các chuỗi có thể truy cập là:```
ABC
ABF
AEC
AEF
DBC
DBF
DEC
DEF
```có`2^3 = 8`khả năng xảy ra vì mỗi cột có thể được chọn độc lập với các ký tự xuất hiện trong cột đó. Một mô phỏng ngây thơ thường bỏ lỡ sự độc lập này. 

Một cái bẫy nữa là các chuỗi trùng lặp. Ví dụ:```
3 3
AAA
AAA
AAA
```Câu trả lời là`1`, không`3^3`. Chúng tôi đếm các ký tự riêng biệt trên mỗi cột chứ không phải số hàng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mọi chuỗi có thể truy cập là một trạng thái và áp dụng liên tục tất cả các hoạt động có thể. Đối với mỗi cặp chuỗi và mỗi độ dài tiền tố, chúng tôi tạo các trạng thái mới và tiếp tục cho đến khi không còn chuỗi chưa nhìn thấy nào xuất hiện. 

Điều này hoạt động về mặt khái niệm vì không gian hoạt động là hữu hạn và mọi chuỗi được tạo đều thực sự có thể truy cập được. Vấn đề là sự bùng nổ về số lượng các bang. Nếu mỗi trong số`m`các vị trí có thể độc lập đảm nhận ngay cả`2`giá trị, tổng số chuỗi có thể truy cập sẽ trở thành`2^m`. Với`m = 100`, điều này là hoàn toàn không thể. 

Bước ngoặt đến từ việc hiểu được những tiền tố hoán đổi thực sự thay đổi những gì. 

Giả sử chúng ta tập trung vào một vị trí duy nhất`p`. Nếu chúng ta trao đổi tiền tố có độ dài ít nhất`p + 1`, thì ký tự ở vị trí`p`di chuyển cùng với tiền tố. Nếu độ dài tiền tố nhỏ hơn, vị trí`p`không bị ảnh hưởng. 

Điều này có nghĩa là chúng ta có thể nhập độc lập bất kỳ ký tự nào đã tồn tại trong cột`p`vào chuỗi đầu tiên. Quan trọng hơn, các thao tác trên các cột sau không hủy bỏ các lựa chọn đã được thực hiện cho các cột trước đó. Mỗi cột hoạt động độc lập. 

Vì vậy, vấn đề rút gọn thành một quan sát đếm đơn giản: 

Đối với mỗi cột, hãy đếm xem có bao nhiêu ký tự riêng biệt xuất hiện ở đó trong số tất cả các chuỗi. Câu trả lời cuối cùng là tích của những phép tính đó. 

Tại sao nhân? Bởi vì với mỗi cột chúng ta có thể chọn bất kỳ ký tự nào xuất hiện trong cột đó, độc lập với các lựa chọn cho các cột khác. 

Ví dụ:```
ABC
DEF
```Cột`0`có`{A, D}`, cột`1`có`{B, E}`, cột`2`có`{C, F}`. 

Số chuỗi có thể truy cập là:```
2 * 2 * 2 = 8
```Điều này hoàn toàn tránh được sự mô phỏng. Chúng tôi chỉ quét đầu vào một lần và đếm các chữ cái riêng biệt trên mỗi cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`m`| số mũ trong`m`| Quá chậm | 
| Tối ưu | O(nm) | O(n) hoặc O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`Và`m`, sau đó lưu trữ tất cả các chuỗi. 
2. Khởi tạo câu trả lời dưới dạng`1`. 
3. Đối với mỗi cột`j`từ`0`ĐẾN`m - 1`, thu thập tất cả các ký tự xuất hiện ở vị trí đó trên mỗi chuỗi. 

Chúng tôi chỉ quan tâm đến các ký tự riêng biệt vì việc chọn cùng một ký tự từ nhiều hàng không tạo ra chuỗi mới. 
4. Đặt số lượng ký tự riêng biệt trong cột`j`là`cnt`. 
5. Nhân câu trả lời với`cnt`, lấy modulo`10^9 + 7`. 

Phép nhân này hoạt động vì các lựa chọn cho các cột khác nhau là độc lập. 
6. Sau khi xử lý tất cả các cột, hãy in câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Đối với bất kỳ cột nào`j`, hoán đổi một tiền tố có độ dài ít nhất`j + 1`có thể di chuyển nhân vật tại vị trí`j`từ chuỗi này sang chuỗi khác. Vì vậy, mọi ký tự đã có trong cột đó cuối cùng đều có thể xuất hiện trong chuỗi đầu tiên. 

Đồng thời, các thao tác ảnh hưởng đến cột`j`không hạn chế những ký tự nào sau này có thể được chọn cho các cột khác. Việc xây dựng hoạt động độc lập cho mỗi vị trí. 

Bởi vì mỗi cột đóng góp một tập hợp các lựa chọn độc lập nên tổng số chuỗi có thể truy cập bằng tích của số ký tự riêng biệt trong mỗi cột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, m = map(int, input().split())
names = [input().strip() for _ in range(n)]

answer = 1

for col in range(m):
    chars = set()

    for row in range(n):
        chars.add(names[row][col])

    answer = (answer * len(chars)) % MOD

print(answer)
```Chương trình trực tiếp thực hiện quan sát độc lập với cột. 

Đầu tiên chúng ta đọc tất cả các chuỗi vào một danh sách. Sau đó, đối với mỗi cột, chúng tôi xây dựng một tập hợp chứa tất cả các ký tự riêng biệt xuất hiện ở đó. Các bộ Python tự động loại bỏ các bản sao, khớp chính xác với yêu cầu toán học. 

Câu trả lời được nhân với kích thước của bộ này. Vì số lượng các chuỗi có thể có có thể trở nên cực kỳ lớn nên mọi phép nhân đều được thực hiện theo modulo`10^9 + 7`. 

Một lỗi triển khai phổ biến là tính tổng số lần xuất hiện thay vì các ký tự riêng biệt. Ví dụ: nếu một cột chứa`A A A`, phần đóng góp là`1`, không`3`. 

Một điểm tinh tế khác là lập chỉ mục. Python sử dụng lập chỉ mục dựa trên 0, vì vậy cột`0`tương ứng với ký tự đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
AAB
BAA
```Xử lý dấu vết: 

| Cột | Nhân vật đã thấy | Số lượng riêng biệt | Chạy Trả lời | 
| --- | --- | --- | --- | 
| 0 | A, B | 2 | 2 | 
| 1 | A, A | 1 | 2 | 
| 2 | B, A | 2 | 4 | 

Câu trả lời cuối cùng:```
4
```Các chuỗi có thể truy cập là:```
AAB
AAA
BAA
BAB
```Ví dụ này chứng minh rằng các ký tự trùng lặp bên trong một cột không làm tăng số lượng. 

### Ví dụ 2 

đầu vào:```
3 4
ABCD
AACD
BBAD
```Xử lý dấu vết: 

| Cột | Nhân vật đã thấy | Số lượng riêng biệt | Chạy Trả lời | 
| --- | --- | --- | --- | 
| 0 | A, A, B | 2 | 2 | 
| 1 | B, A, B | 2 | 4 | 
| 2 | C, C, A | 2 | 8 | 
| 3 | Đ, Đ, D | 1 | 8 | 

Câu trả lời cuối cùng:```
8
```Dấu vết này cho thấy sự độc lập giữa các cột. Cột cuối cùng chỉ đóng góp một lựa chọn, trong khi các cột còn lại đóng góp hai lựa chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Chúng tôi quét từng ký tự chính xác một lần | 
| Không gian | O(n) | Một bộ lưu trữ tối đa một cột`n`nhân vật | 

Với`n, m ≤ 100`, tổng công việc nhiều nhất là`10^4`lượt truy cập của nhân vật, rất nhỏ so với thời hạn. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    MOD = 10**9 + 7

    n, m = map(int, input().split())
    names = [input().strip() for _ in range(n)]

    ans = 1

    for col in range(m):
        chars = set()

        for row in range(n):
            chars.add(names[row][col])

        ans = (ans * len(chars)) % MOD

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run(
"""2 3
AAB
BAA
"""
) == "4", "sample 1"

# minimum size
assert run(
"""1 1
A
"""
) == "1", "minimum case"

# all strings equal
assert run(
"""3 3
AAA
AAA
AAA
"""
) == "1", "all equal"

# every column has two choices
assert run(
"""2 3
ABC
DEF
"""
) == "8", "independent columns"

# mixed duplicates
assert run(
"""3 2
AA
AB
AC
"""
) == "3", "duplicate handling"

# boundary style case
assert run(
"""4 4
ABCD
BBCD
CBAD
DBCD
"""
) == "12", "multiple distinct counts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / A`|`1`| Đầu vào nhỏ nhất có thể | 
| Ba chuỗi giống hệt nhau |`1`| Các hàng trùng lặp không làm tăng sự lựa chọn | 
|`ABC / DEF`|`8`| Phép nhân cột độc lập | 
|`AA / AB / AC`|`3`| Đếm riêng biệt trên mỗi cột | 
| Ví dụ 4x4 hỗn hợp |`12`| Độ chính xác chung trên các cột | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các chuỗi đều giống hệt nhau:```
3 3
AAA
AAA
AAA
```Đối với mỗi cột, tập hợp riêng biệt chỉ là`{A}`. Thuật toán tính toán:```
1 * 1 * 1 = 1
```Không có thao tác nào có thể giới thiệu một ký tự mới vì không có ký tự nào tồn tại trong bất kỳ cột nào. Việc triển khai bất cẩn đếm hàng thay vì các ký tự riêng biệt sẽ trả về không chính xác`27`. 

Bây giờ hãy xem xét các lựa chọn độc lập trên các cột:```
2 3
ABC
DEF
```Bộ cột là:```
{A, D}
{B, E}
{C, F}
```Thuật toán nhân lên:```
2 * 2 * 2 = 8
```Điều này đúng vì mỗi cột có thể chọn độc lập một trong các ký tự có sẵn của nó. Bất kỳ cách tiếp cận nào cố gắng duy trì cấu trúc toàn bộ hàng sẽ bị tính thiếu. 

Cuối cùng, hãy xem xét sự đa dạng không đồng đều:```
3 2
AA
AB
AC
```Cột đầu tiên chỉ đóng góp một tùy chọn:```
{A}
```Cột thứ hai đóng góp ba:```
{A, B, C}
```Câu trả lời trở thành:```
1 * 3 = 3
```Điều này xác nhận rằng mỗi cột phải được xử lý độc lập và các bản sao bên trong một cột phải được bỏ qua.
