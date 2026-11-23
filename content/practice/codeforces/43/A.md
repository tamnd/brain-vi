---
title: "CF 43A - Bóng đá"
description: "Chúng ta được cung cấp chuỗi bàn thắng được ghi trong một trận bóng đá. Mỗi dòng sau dòng đầu tiên chứa tên của đội đã ghi một bàn thắng. Nhiệm vụ là xác định xem đội nào ghi được nhiều bàn thắng hơn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 43
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 42 (Div. 2)"
rating: 1000
weight: 43
solve_time_s: 89
verified: true
draft: false
---
[CF 43A - Bóng đá](https://codeforces.com/problemset/problem/43/A) 

**Đánh giá:** 1000 
**Thẻ:** chuỗi 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp chuỗi bàn thắng được ghi trong một trận bóng đá. Mỗi dòng sau dòng đầu tiên chứa tên của đội đã ghi một bàn thắng. Nhiệm vụ là xác định xem đội nào ghi được nhiều bàn thắng hơn. 

Dữ liệu đầu vào đảm bảo rằng có nhiều nhất hai tên đội riêng biệt xuất hiện và trận đấu không bao giờ kết thúc với tỷ số hòa. Điều đó có nghĩa là một đội luôn có số lượng bàn thắng lớn hơn đội kia. 

Những hạn chế là rất nhỏ. Số lượng bàn thắng nhiều nhất là 100 và mỗi tên đội có độ dài tối đa là 10. Ngay cả một giải pháp kém hiệu quả là so sánh mọi bàn thắng với mọi bàn thắng khác vẫn sẽ phù hợp thoải mái trong thời hạn. Điều này cho phép chúng tôi tự do tập trung vào sự rõ ràng thay vì tối ưu hóa mạnh mẽ. 

Thử thách chính không phải là hiệu suất mà là tính toán chính xác số bàn thắng cho mỗi đội. 

Một sai lầm dễ mắc phải là cho rằng luôn có đúng hai đội. Coi như:```
1
ABC
```Câu trả lời đúng là:```
ABC
```Việc thực hiện bất cẩn mà ngay lập tức đọc được hai tên riêng biệt sẽ thất bại vì chỉ có một đội xuất hiện. 

Một vấn đề nhỏ khác là cập nhật số lượng không chính xác khi cùng một đội xuất hiện nhiều lần. Ví dụ:```
5
A
B
A
A
B
```Đầu ra đúng là:```
A
```Nếu chúng ta ghi đè số đếm thay vì tăng chúng thì kết quả cuối cùng sẽ sai. 

Một cạm bẫy khác đến từ việc chỉ so sánh các mục tiêu liền kề. Coi như:```
4
A
B
B
A
```Tỷ số cuối cùng được hòa ở đây, điều mà bài toán nói là sẽ không bao giờ xảy ra, nhưng ví dụ này cho thấy tại sao sự liền kề không cho chúng ta biết gì về người chiến thắng. Chúng ta phải đếm tất cả các lần xuất hiện trên toàn cầu. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là kiểm tra từng tên đội và đếm xem nó xuất hiện bao nhiêu lần trong danh sách đầy đủ. Đối với mỗi mục tiêu, chúng tôi quét lại toàn bộ mảng và tính toán tần số của nó. 

Nếu có`n`mục tiêu, điều này thực hiện`n * n`so sánh trong trường hợp xấu nhất Với`n = 100`, đó chỉ là 10.000 thao tác, điều này hoàn toàn có thể chấp nhận được ở đây. Cách tiếp cận bạo lực là đúng vì đội chiến thắng chính xác là đội có tần suất lớn nhất. 

Sự kém hiệu quả xuất phát từ việc tính toán lại cùng một số lượng nhiều lần. Nếu như`"A"`xuất hiện 50 lần, mỗi lần gặp đều vẫn kể lại hết 100 mục`"A"`. 

Nhận xét quan trọng là vấn đề này về cơ bản là vấn đề đếm tần số. Thay vì quét danh sách nhiều lần, chúng tôi có thể duy trì số lượng hoạt động của mỗi nhóm trong khi đọc thông tin đầu vào. 

Ở đây, bản đồ băm, được triển khai dưới dạng từ điển Python, là lý tưởng. Mỗi khi chúng tôi đọc tên nhóm, chúng tôi sẽ tăng số lượng được lưu trữ lên một. Sau khi xử lý tất cả các bàn thắng, từ điển sẽ chứa số bàn thắng chính xác mà mỗi đội ghi được. 

Vì có nhiều nhất là hai đội nên việc tìm ra đội chiến thắng sau đó là chuyện nhỏ. Chúng tôi chỉ cần chọn đội có số lượng lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(k) | Đã chấp nhận | 

Đây,`k`là số đội khác nhau, tối đa là 2. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số bàn thắng được ghi trong trận đấu. 
2. Tạo một từ điển trống có tên`count`. 

Từ điển ánh xạ tên đội theo số bàn thắng mà đội đó ghi được. 
3. Lặp lại`n`lần: 

1. Đọc tên đội. 
2. Tăng tần suất của nó trong từ điển. 

Nếu đội chưa xuất hiện trước đó, trước tiên hãy đặt số đếm của đội đó về 0. 
4. Sau khi xử lý tất cả các mục tiêu, lặp lại các mục từ điển và theo dõi nhóm có số lượng tối đa. 
5. In nhóm với tần suất cao nhất. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau khi xử lý dữ liệu đầu tiên`i`bàn thắng, từ điển sẽ lưu trữ chính xác số bàn thắng mà mỗi đội ghi được trong đó`i`mục nhập. 

Mỗi mục tiêu mới cập nhật chính xác số đếm của một đội, do đó, tính bất biến vẫn đúng trong suốt quá trình. 

Cuối cùng, tất cả các bàn thắng đã được xử lý, nghĩa là từ điển chứa tỷ số cuối cùng của mỗi đội. Vì bài toán đảm bảo không có kết quả hòa nên đội nào có số điểm lớn nhất sẽ là đội chiến thắng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    count = {}
    
    for _ in range(n):
        team = input().strip()
        count[team] = count.get(team, 0) + 1
    
    winner = ""
    best = 0
    
    for team, goals in count.items():
        if goals > best:
            best = goals
            winner = team
    
    print(winner)

solve()
```Giải pháp bắt đầu bằng cách đọc số lượng bàn thắng. Một cuốn từ điển có tên`count`lưu trữ số lần mỗi đội xuất hiện. 

Biểu thức:```
count.get(team, 0) + 1
```là quan trọng vì nó xử lý cả hai trường hợp một cách rõ ràng. Nếu nhóm đã tồn tại trong từ điển, chúng tôi sẽ tăng giá trị hiện tại của nhóm đó. Nếu không thì,`get`trả lại`0`, và đội bắt đầu với một mục tiêu. 

Sau khi đếm tất cả các mục tiêu, chúng ta quét từ điển một lần nữa để tìm tần số tối đa. Vì bài toán đảm bảo một người chiến thắng duy nhất nên chúng ta không bao giờ cần đến logic xử lý ràng buộc. 

sử dụng`strip()`khi đọc tên nhóm sẽ tránh được các ký tự dòng mới vô tình trở thành một phần của chuỗi. Quên điều này là lỗi thường gặp trong các vấn đề về chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
ABC
```Dấu vết: 

| Bước | Đội đọc | đếm từ điển | Người chiến thắng hiện tại | 
| --- | --- | --- | --- | 
| 1 | ABC | {"ABC": 1} | ABC | 

Đầu ra:```
ABC
```Ví dụ này thể hiện đầu vào hợp lệ nhỏ nhất. Chỉ có một đội xuất hiện nên tự động thắng. 

### Ví dụ 2 

đầu vào:```
5
A
B
A
A
B
```Dấu vết: 

| Bước | Đội đọc | đếm từ điển | Lãnh đạo hiện tại | 
| --- | --- | --- | --- | 
| 1 | A | {"A": 1} | A | 
| 2 | B | {"A": 1, "B": 1} | Cà vạt | 
| 3 | A | {"A": 2, "B": 1} | A | 
| 4 | A | {"A": 3, "B": 1} | A | 
| 5 | B | {"A": 3, "B": 2} | A | 

Đầu ra:```
A
```Dấu vết này cho thấy từ điển luôn phản ánh chính xác số điểm sau mỗi bàn thắng được xử lý. Mặc dù đội`B`cuối cùng lại ghi bàn, đội`A`vẫn kết thúc với nhiều bàn thắng hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mục tiêu được xử lý một lần | 
| Không gian | O(k) | Từ điển lưu trữ một mục nhập cho mỗi nhóm riêng biệt | 

Từ`n ≤ 100`, nghiệm dễ dàng nằm trong giới hạn. Từ điển chứa nhiều nhất hai mục nên việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    count = {}

    for _ in range(n):
        team = input().strip()
        count[team] = count.get(team, 0) + 1

    winner = ""
    best = 0

    for team, goals in count.items():
        if goals > best:
            best = goals
            winner = team

    print(winner)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("1\nABC\n") == "ABC\n", "sample 1"

# minimum size input
assert run("1\nTEAM\n") == "TEAM\n", "single team"

# alternating goals
assert run("5\nA\nB\nA\nB\nA\n") == "A\n", "alternating sequence"

# all goals by same team
assert run("4\nX\nX\nX\nX\n") == "X\n", "all equal"

# long names
assert run("3\nBARCELONA\nREALMADRID\nBARCELONA\n") == "BARCELONA\n", "string handling"

# winner decided late
assert run("7\nA\nB\nB\nA\nA\nB\nA\n") == "A\n", "late lead"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / TEAM`|`TEAM`| Đầu vào hợp lệ tối thiểu | 
| Mục tiêu A/B xen kẽ |`A`| Đếm tích lũy đúng | 
| Tất cả bàn thắng của một đội |`X`| Xử lý nhóm riêng biệt duy nhất | 
| Tên viết hoa dài |`BARCELONA`| Xử lý chuỗi thích hợp | 
| Người chiến thắng được quyết định gần cuối |`A`| So sánh cuối cùng đúng | 

## Vỏ cạnh 

Xét trường hợp chỉ có một đội xuất hiện:```
1
ABC
```Dấu vết thực hiện: 

1. Đọc`"ABC"`. 
2. Cửa hàng`count["ABC"] = 1`. 
3. Từ điển chỉ có một mục. 
4. Bài dự thi đó được chọn là người chiến thắng. 

Thuật toán in chính xác:```
ABC
```Không cần xử lý trường hợp đặc biệt nào vì từ điển hỗ trợ một cách tự nhiên cho một nhóm duy nhất. 

Bây giờ hãy xem xét các cập nhật lặp lại cho cùng một nhóm:```
5
A
B
A
A
B
```Từ điển phát triển như sau:```
{"A": 1}
{"A": 1, "B": 1}
{"A": 2, "B": 1}
{"A": 3, "B": 1}
{"A": 3, "B": 2}
```Thuật toán không bao giờ ghi đè số đếm không chính xác vì mỗi lần xuất hiện đều tăng giá trị trước đó. 

Cuối cùng, hãy xem xét trường hợp người chiến thắng thay đổi trong quá trình xử lý:```
7
A
B
B
A
A
B
A
```Các nhà lãnh đạo trung gian dao động, nhưng thuật toán không dựa vào các nhà lãnh đạo tạm thời. Nó chỉ sử dụng tần số cuối cùng:```
A -> 4
B -> 3
```Đầu ra cuối cùng là:```
A
```Điều này xác nhận rằng việc đếm toàn cầu trên toàn bộ đầu vào là chiến lược đúng đắn.
