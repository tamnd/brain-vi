---
title: "CF 315A - Sereja và Chai"
description: "Mỗi chai có hai thuộc tính. Giá trị a[i] là nhãn hiệu riêng của chai. Giá trị b[i] là nhãn hiệu của chai mà chai này có thể mở được. Một chai có thể được mở nếu tồn tại một số chai khác có khả năng mở phù hợp với nhãn hiệu của nó."
date: "2026-06-06T01:20:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 315
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 187 (Div. 2)"
rating: 1400
weight: 315
solve_time_s: 114
verified: true
draft: false
---

[CF 315A - Sereja và Chai](https://codeforces.com/problemset/problem/315/A) 

**Đánh giá:** 1400 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chai có hai thuộc tính. 

giá trị`a[i]`là thương hiệu riêng của chai. giá trị`b[i]`là nhãn hiệu của chai mà chai này có thể mở được. 

Một chai có thể được mở nếu tồn tại một số chai khác có khả năng mở phù hợp với nhãn hiệu của nó. Vì chai có thể được sử dụng dù chúng đã mở hay vẫn đóng nên chúng tôi chỉ quan tâm đến mối quan hệ tĩnh giữa các thương hiệu. Không có chuỗi hành động để mô phỏng. 

Đối với chai có nhãn hiệu`a[i]`, chúng ta cần kiểm tra xem có tồn tại một chai khác không`j`như vậy`b[j] = a[i]`. Nếu một cái chai như vậy tồn tại, cái chai`i`có thể được mở. Nếu không, nó không bao giờ có thể mở được. 

Nhiệm vụ là đếm xem có bao nhiêu chai không thể mở được bằng chai khác. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 chai. Ngay cả một thuật toán so sánh từng cặp chai cũng chỉ hoạt động`100 × 100 = 10,000`kiểm tra, đó là chuyện nhỏ trong thời hạn. Không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. 

Một chi tiết tinh tế là cái chai không thể tự mở được. Khi kiểm tra xem chai`i`có thể mở được, chúng ta chỉ phải xem xét những chai có chỉ số khác. 

Hãy xem xét ví dụ này:```
2
1 1
2 2
```Chai 1 có thể mở nhãn hiệu 1, nhưng khả năng đó thuộc về cùng một chai. Chai 2 hoạt động tương tự. Cả hai chai đều không thể mở được bằng chai khác nên đáp án là:```
2
```Việc triển khai bất cẩn cho phép tự khớp sẽ tạo ra kết quả không chính xác`0`. 

Một trường hợp quan trọng khác là khi nhiều chai có cùng nhãn hiệu.```
3
1 5
1 7
2 1
```Chai 3 có thể mở được nhãn hiệu 1 nên cả hai chai có nhãn hiệu 1 đều mở được. Bản thân chai 3 có nhãn hiệu 2 và không có chai nào mở được nhãn hiệu 2. Đáp án đúng là:```
1
```Việc triển khai chỉ kiểm tra các nhãn hiệu duy nhất thay vì các chai riêng lẻ có thể bị tính sai. 

Trường hợp thứ ba là khi cùng một nhãn hiệu có thể mở được nhiều chai.```
3
4 4
5 4
4 1
```Cả chai thứ nhất và chai thứ hai đều có thể mở được nhãn hiệu 4. Chai thứ ba có nhãn hiệu 4, chai thứ nhất có thể mở được. Mỗi chai có nhãn hiệu 4 phải được coi là có thể mở được một cách độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra từng chai riêng biệt. Đối với chai`i`, chúng tôi quét tất cả các chai khác và kiểm tra xem có chai nào`j`có`b[j] = a[i]`. Nếu chúng ta tìm thấy một cái chai như vậy, cái chai`i`có thể mở được. Nếu không, nó góp phần vào câu trả lời. 

Cách tiếp cận này đúng vì định nghĩa về khả năng mở được chỉ phụ thuộc vào sự tồn tại của một chai khác có nhãn hiệu mở đầu trùng với nhãn hiệu của chính chai đó. Bằng cách kiểm tra mọi chai có thể`j`, ta kiểm tra chính xác điều kiện mà bài toán yêu cầu. 

Đối với các ràng buộc lớn hơn, việc so sánh từng cặp có thể trở nên tốn kém. Với`n = 100`tuy nhiên, trường hợp xấu nhất chỉ là 10.000 phép so sánh, rất nhỏ. 

Người ta cũng có thể xây dựng một bộ chứa tất cả các nhãn hiệu mở đầu`b[i]`và kiểm tra tư cách thành viên. Sự phức tạp duy nhất là việc cấm tự mở. Vì các ràng buộc quá nhỏ nên giải pháp theo cặp đơn giản hơn và tránh được việc xử lý đặc biệt. 

Nhận xét quan trọng là trạng thái của một chai chỉ phụ thuộc vào việc nhãn hiệu của nó có xuất hiện trong khả năng mở của một số chai khác hay không. Điều này đương nhiên dẫn đến việc kiểm tra tất cả các cặp chai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(n²) | O(1) | Đã chấp nhận | 
| Cấu trúc theo dõi thương hiệu | O(n2) hoặc O(n) | O(n) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc`n`và lưu trữ tất cả các cặp`(a[i], b[i])`. 
2. Khởi tạo`answer = 0`. 
3. Cho mỗi chai`i`, ban đầu giả sử rằng nó không thể mở được. 
4. Quét tất cả các chai`j`. 
5. Bỏ qua vụ việc`i == j`, vì chai không thể tự mở được. 
6. Nếu`b[j] == a[i]`, sau đó đóng chai`i`có thể được mở bằng chai`j`. Đánh dấu nó là có thể mở được và ngừng kiểm tra các chai khác. 
7. Sau khi kiểm tra tất cả các chai có thể, nếu chai`i`vẫn không mở được, tăng dần`answer`. 
8. Sau khi xử lý từng chai, in`answer`. 

### Tại sao nó hoạt động 

Đối với một chai`i`, bài toán hỏi liệu có tồn tại một chai khác có khả năng mở bằng`a[i]`. Thuật toán kiểm tra mọi chai có thể`j ≠ i`. Nếu một chai như vậy tồn tại, nó sẽ được tìm thấy trong quá trình quét và đóng chai`i`được đánh dấu là có thể mở được. Nếu quá trình quét kết thúc mà không tìm thấy thì chai đó không tồn tại ở bất kỳ đâu trong đầu vào, vì vậy chai`i`không thể mở được. 

Vì lý do này được áp dụng độc lập cho mỗi chai nên số lượng cuối cùng chính xác là số chai không thể mở được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
bottles = [tuple(map(int, input().split())) for _ in range(n)]

answer = 0

for i in range(n):
    brand = bottles[i][0]
    openable = False

    for j in range(n):
        if i == j:
            continue

        if bottles[j][1] == brand:
            openable = True
            break

    if not openable:
        answer += 1

print(answer)
```Chương trình đầu tiên lưu trữ tất cả các mô tả về chai. Đối với mỗi chai, nó chiết xuất thương hiệu của nó`a[i]`và tìm kiếm một chai khác có nhãn hiệu mở đầu bằng giá trị đó. 

các`i == j`kiểm tra là chi tiết triển khai quan trọng nhất. Không có nó, một cái chai của riêng nó`b[i]`bằng`a[i]`sẽ được coi là có thể mở được một cách không chính xác. 

Vòng lặp bên trong dừng ngay sau khi tìm thấy phần mở hợp lệ. Điều này không làm thay đổi tính đúng đắn nhưng tránh được những so sánh không cần thiết. 

Không cần xử lý đặc biệt đối với các nhãn hiệu trùng lặp hoặc khả năng mở trùng lặp vì mỗi chai đều được kiểm tra riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1
2 2
3 3
4 4
```| Chai tôi | Thương hiệu a[i] | Đã tìm thấy dụng cụ mở phù hợp? | Số lượng không thể mở được | 
| --- | --- | --- | --- | 
| 1 | 1 | Không | 1 | 
| 2 | 2 | Không | 2 | 
| 3 | 3 | Không | 3 | 
| 4 | 4 | Không | 4 | 

Đầu ra:```
4
```Mỗi chai chỉ phù hợp với khả năng mở riêng của nó. Vì việc tự mở là không được phép nên không ai trong số chúng có thể mở được. 

### Ví dụ 2 

đầu vào:```
3
1 2
2 3
3 1
```| Chai tôi | Thương hiệu a[i] | Chai mở nó | Số lượng không thể mở được | 
| --- | --- | --- | --- | 
| 1 | 1 | Chai 3 | 0 | 
| 2 | 2 | Chai 1 | 0 | 
| 3 | 3 | Chai 2 | 0 | 

Đầu ra:```
0
```Các chai tạo thành một chu kỳ. Nhãn hiệu của mỗi chai xuất hiện dưới dạng khả năng mở của chai khác, vì vậy tất cả các chai đều có thể mở được. 

Ví dụ này chứng minh rằng các chai không cần phải mở theo bất kỳ thứ tự cụ thể nào. Vấn đề chỉ hỏi liệu dụng cụ mở có tồn tại hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Đối với mỗi chai, chúng tôi có thể quét tất cả các chai khác | 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài lưu trữ đầu vào | 

Với`n ≤ 100`, thuật toán thực hiện tối đa 10.000 so sánh cặp. Giá trị này thấp hơn nhiều so với giới hạn, khiến cho nghiệm bậc hai đơn giản là quá đủ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    bottles = [tuple(map(int, input().split())) for _ in range(n)]

    answer = 0

    for i in range(n):
        brand = bottles[i][0]
        openable = False

        for j in range(n):
            if i == j:
                continue

            if bottles[j][1] == brand:
                openable = True
                break

        if not openable:
            answer += 1

    return str(answer)

# provided sample
assert run("4\n1 1\n2 2\n3 3\n4 4\n") == "4", "sample 1"

# minimum size
assert run("1\n42 42\n") == "1", "single bottle cannot open itself"

# cyclic opening
assert run("3\n1 2\n2 3\n3 1\n") == "0", "all bottles openable"

# duplicate brands
assert run("3\n1 5\n1 7\n2 1\n") == "1", "two bottles opened by same opener"

# all bottles open same brand
assert run("4\n1 5\n2 5\n3 5\n4 5\n") == "4", "no bottle brand appears among opening capabilities"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42 42`|`1`| Không được phép tự mở | 
| Chu kỳ ba chai |`0`| Mọi chai đều có thể mở được | 
| Ví dụ về thương hiệu trùng lặp |`1`| Nhiều chai có thể chia sẻ một thương hiệu | 
| Tất cả khả năng mở bằng 5 |`4`| Không có dụng cụ mở phù hợp cho bất kỳ chai nào | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
42 42
```Thuật toán kiểm tra chai duy nhất. Vòng lặp bên trong bỏ qua việc tự so sánh vì`i == j`. Không tìm thấy phần mở hợp lệ nào, vì vậy câu trả lời trở thành`1`. Điều này phù hợp với quy luật chai không thể tự mở được. 

Hãy xem xét trường hợp có nhãn hiệu trùng lặp:```
3
1 5
1 7
2 1
```Đối với chai đầu tiên, thuật toán thấy rằng chai 3 có`b = 1`, vì vậy nó có thể mở được. Lý do tương tự cũng áp dụng cho chai thứ hai vì nó có cùng nhãn hiệu. Chai thứ ba có nhãn hiệu`2`và không có chai nào có khả năng mở được`2`, nên chỉ tính chai đó thôi. Câu trả lời là`1`. 

Hãy xem xét một chu kỳ:```
3
1 2
2 3
3 1
```Chai 1 được mở bằng chai 3, chai 2 được mở bằng chai 1 và chai 3 bằng chai 2. Mọi tìm kiếm đều tìm thấy dụng cụ mở phù hợp nên số lượng vẫn giữ nguyên`0`. Thuật toán xử lý chính xác các chuỗi và chu trình vì nó chỉ kiểm tra sự tồn tại chứ không phải chuỗi mở.
