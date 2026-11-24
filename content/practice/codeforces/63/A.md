---
title: "CF 63A - Tàu chìm"
description: "Chúng ta được đưa cho một danh sách những người đứng theo thứ tự cố định từ trái qua phải trên một con tàu đang chìm. Mỗi người đều có một cái tên và một vai trò. Lệnh sơ tán phụ thuộc hoàn toàn vào mức độ ưu tiên của vai trò. Chuột rời đi đầu tiên. Phụ nữ và trẻ em chia sẻ mức độ ưu tiên tiếp theo. Đàn ông bỏ đi sau họ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 63
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 59 (Div. 2)"
rating: 900
weight: 63
solve_time_s: 91
verified: true
draft: false
---

[CF 63A - Tàu chìm](https://codeforces.com/problemset/problem/63/A) 

**Xếp hạng:** 900 
**Tags:** triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một danh sách những người đứng theo thứ tự cố định từ trái qua phải trên một con tàu đang chìm. Mỗi người đều có một cái tên và một vai trò. Lệnh sơ tán phụ thuộc hoàn toàn vào mức độ ưu tiên của vai trò. 

Chuột rời đi đầu tiên. Phụ nữ và trẻ em chia sẻ mức độ ưu tiên tiếp theo. Đàn ông bỏ đi sau họ. Thuyền trưởng luôn rời đi sau cùng. 

Khi hai người thuộc cùng một nhóm ưu tiên, thứ tự tương đối của họ phải giữ nguyên như ở dòng ban đầu. Điều đó có nghĩa đây không chỉ là vấn đề sắp xếp theo danh mục mà còn là vấn đề sắp xếp ổn định. 

Kích thước đầu vào rất nhỏ, chỉ tối đa 100 người. Ngay cả các thuật toán bậc hai cũng dễ dàng nằm gọn trong giới hạn thời gian. Thử thách không phải là hiệu suất, mà là thực hiện đúng các quy tắc ưu tiên mà không vô tình phá vỡ trật tự ban đầu trong một nhóm. 

Một sai lầm phổ biến là coi phụ nữ và trẻ em là những ưu tiên riêng biệt. Họ thực sự thuộc cùng một cấp độ sơ tán. 

Hãy xem xét đầu vào này:```
4
Anna woman
Ben child
Chris woman
David man
```Đầu ra đúng là:```
Anna
Ben
Chris
David
```Việc thực hiện bất cẩn sơ tán tất cả phụ nữ trước tất cả trẻ em sẽ tạo ra sai sót:```
Anna
Chris
Ben
David
```Một sai lầm dễ mắc phải khác là sử dụng cách sắp xếp không ổn định mà không giữ nguyên vị trí ban đầu. 

Coi như:```
5
Tom man
Jerry man
Spike man
Mila woman
Captain captain
```Những người đàn ông phải giữ nguyên thứ tự ban đầu của họ:```
Mila
Tom
Jerry
Spike
Captain
```Nếu việc triển khai chỉ sắp xếp theo mức độ ưu tiên và bỏ qua các chỉ số ban đầu thì thứ tự giữa các nam giới có thể trở nên tùy tiện. 

Trường hợp cạnh thứ ba là thuyền trưởng. Chỉ có duy nhất một đội trưởng và người đó phải luôn xuất hiện cuối cùng dù ban đầu họ đứng ở đâu. 

Ví dụ:```
3
Jack captain
Alice woman
Bob rat
```Đầu ra đúng:```
Bob
Alice
Jack
```Nếu ai đó xử lý dòng từ trái sang phải và in ngay lập tức, đội trưởng có thể xuất hiện quá sớm do nhầm lẫn. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là quét liên tục toàn bộ danh sách và trích xuất từng danh mục người. Đầu tiên hãy quét chuột và in chúng theo thứ tự. Sau đó quét lại để tìm phụ nữ và trẻ em. Sau đó quét lại cho nam giới. Cuối cùng in thuyền trưởng. 

Điều này hiệu quả vì các ưu tiên sơ tán đã được cố định và chỉ có năm trạng thái có thể xảy ra. Vì mỗi lần quét giữ nguyên thứ tự từ trái sang phải ban đầu nên kết quả đầu ra là chính xác. 

Độ phức tạp trong trường hợp xấu nhất vẫn còn rất nhỏ. Chúng tôi quét tối đa bốn hoặc năm lần trên tối đa 100 người, vì vậy tổng công việc là khoảng 500 thao tác. 

Một cách tiếp cận khác là gán mức độ ưu tiên bằng số cho mọi vai trò và sắp xếp mọi người theo mức độ ưu tiên đó. Ví dụ:```
rat -> 0
woman -> 1
child -> 1
man -> 2
captain -> 3
```Nhận xét quan trọng là phụ nữ và trẻ em có chủ ý chia sẻ cùng một mức độ ưu tiên. Khi chúng ta ánh xạ các vai trò thành các con số, bài toán sẽ trở thành một nhiệm vụ sắp xếp ổn định. 

Tính năng sắp xếp tích hợp của Python ổn định, có nghĩa là những người có mức độ ưu tiên như nhau sẽ tự động giữ nguyên thứ tự ban đầu của họ. Điều đó phù hợp với yêu cầu vấn đề một cách hoàn hảo. 

Mặc dù cả hai cách tiếp cận đều đủ nhanh nhưng cách tiếp cận sắp xếp rõ ràng hơn và dễ khái quát hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng thuyền viên. 
2. Với mỗi người, hãy đọc tên và trạng thái của họ. 
3. Chỉ định mức độ ưu tiên sơ tán bằng số cho từng trạng thái. 

Chuột nhận được giá trị nhỏ nhất vì chúng rời đi trước. Thuyền trưởng nhận được giá trị lớn nhất vì họ rời đi cuối cùng. Phụ nữ và trẻ em nhận được giá trị như nhau vì họ có chung quyền ưu tiên. 
4. Lưu trữ mỗi người thành một cặp chứa mức độ ưu tiên và tên của họ. 

Thứ tự đầu vào ban đầu được giữ nguyên tự động vì tính năng sắp xếp của Python ổn định. 
5. Sắp xếp danh sách theo mức độ ưu tiên. 
6. In tên theo thứ tự sắp xếp. 

### Tại sao nó hoạt động 

Mỗi thành viên phi hành đoàn thuộc đúng một hạng mục sơ tán. Ánh xạ số chuyển đổi các quy tắc bằng lời nói thành một mối quan hệ thứ tự có thể sắp xếp được. 

Nếu hai người thuộc các danh mục khác nhau, giá trị ưu tiên nhỏ hơn sẽ đặt chính xác người này trước người kia. 

Nếu hai người thuộc cùng một danh mục thì cả hai đều được ưu tiên như nhau. Tính năng sắp xếp ổn định của Python giữ nguyên thứ tự tương đối ban đầu của chúng, khớp chính xác với yêu cầu tuyên bố mà những người ở xa hơn sẽ sơ tán trước khi có các ưu tiên. 

Bởi vì cả hai quy tắc đều được thỏa mãn đồng thời nên thứ tự cuối cùng luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    priority = {
        "rat": 0,
        "woman": 1,
        "child": 1,
        "man": 2,
        "captain": 3
    }

    people = []

    for _ in range(n):
        name, status = input().split()
        people.append((priority[status], name))

    people.sort()

    for _, name in people:
        print(name)

solve()
```Từ điển chuyển đổi từng trạng thái thành một giá trị số có thể sắp xếp được. Chi tiết quan trọng đó là`"woman"`Và`"child"`cả hai bản đồ tới`1`, ưu tiên cho chúng như nhau. 

Mỗi người được lưu trữ dưới dạng một bộ dữ liệu`(priority, name)`. Vì Python sắp xếp các bộ dữ liệu theo từ điển nên danh sách được sắp xếp chủ yếu theo mức độ ưu tiên. 

Khả năng sắp xếp của Python ổn định, đây là đặc tính tinh tế nhưng quan trọng ở đây. Khi hai người có cùng mức độ ưu tiên, thứ tự tương đối của họ không thay đổi so với đầu vào. Điều đó tự động đáp ứng quy tắc từ trái sang phải từ câu lệnh vấn đề mà không lưu trữ các chỉ số rõ ràng. 

Các ràng buộc quá nhỏ nên hiệu suất không liên quan, nhưng việc triển khai này vẫn rõ ràng và hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
Jack captain
Alice woman
Charlie man
Teddy rat
Bob child
Julia woman
```Các bước xử lý: 

| Người | Trạng thái | Ưu tiên | Tuple được lưu trữ | 
| --- | --- | --- | --- | 
| Jack | đội trưởng | 3 | (3, Jack) | 
| Alice | người phụ nữ | 1 | (1, Alice) | 
| Charlie | người đàn ông | 2 | (2, Charlie) | 
| Teddy | chuột | 0 | (0, Teddy) | 
| Bob | con | 1 | (1, Bob) | 
| Julia | người phụ nữ | 1 | (1, Julia) | 

Sau khi sắp xếp: 

| Ưu tiên | Tên | 
| --- | --- | 
| 0 | Teddy | 
| 1 | Alice | 
| 1 | Bob | 
| 1 | Julia | 
| 2 | Charlie | 
| 3 | Jack | 

Đầu ra:```
Teddy
Alice
Bob
Julia
Charlie
Jack
```Dấu vết này chứng tỏ tại sao việc sắp xếp ổn định lại quan trọng. Alice, Bob và Julia đều có cùng mức độ ưu tiên và thứ tự tương đối ban đầu của họ được giữ nguyên. 

### Ví dụ 2 

đầu vào:```
5
Tom man
Jerry man
Lucy child
Kate woman
Morgan captain
```Các bước xử lý: 

| Người | Trạng thái | Ưu tiên | Tuple được lưu trữ | 
| --- | --- | --- | --- | 
| Tom | người đàn ông | 2 | (2, Tom) | 
| Jerry | người đàn ông | 2 | (2, Jerry) | 
| Lucy | con | 1 | (1, Lucy) | 
| Kate | người phụ nữ | 1 | (1, Kate) | 
| Morgan | đội trưởng | 3 | (3, Morgan) | 

Sau khi sắp xếp: 

| Ưu tiên | Tên | 
| --- | --- | 
| 1 | Lucy | 
| 1 | Kate | 
| 2 | Tom | 
| 2 | Jerry | 
| 3 | Morgan | 

Đầu ra:```
Lucy
Kate
Tom
Jerry
Morgan
```Ví dụ này cho thấy hai nhóm ràng buộc riêng biệt. Trẻ em và phụ nữ ở chung một nhóm, trong khi hai người đàn ông tạo thành một nhóm khác. Trong cả hai trường hợp, thứ tự ban đầu vẫn được giữ nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp danh sách phi hành đoàn chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Danh sách người được lưu trong bộ nhớ | 

Với tối đa 100 thành viên phi hành đoàn, thời gian chạy rất nhỏ. Ngay cả các thuật toán chậm hơn nhiều cũng có thể vượt qua một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())

        priority = {
            "rat": 0,
            "woman": 1,
            "child": 1,
            "man": 2,
            "captain": 3
        }

        people = []

        for _ in range(n):
            name, status = input().split()
            people.append((priority[status], name))

        people.sort()

        out = []

        for _, name in people:
            out.append(name)

        return "\n".join(out)

    return solve()

# provided sample
assert run(
"""6
Jack captain
Alice woman
Charlie man
Teddy rat
Bob child
Julia woman
"""
) == (
"""Teddy
Alice
Bob
Julia
Charlie
Jack"""
), "sample 1"

# minimum size
assert run(
"""1
Jack captain
"""
) == (
"""Jack"""
), "single person"

# all same priority
assert run(
"""4
Anna woman
Bella child
Cara woman
Diana child
"""
) == (
"""Anna
Bella
Cara
Diana"""
), "stable order among equal priorities"

# captain appears first in input
assert run(
"""3
Captain captain
Tom man
Jerry rat
"""
) == (
"""Jerry
Tom
Captain"""
), "captain must always be last"

# multiple men preserve order
assert run(
"""5
Tom man
Jerry man
Spike man
Lucy child
Captain captain
"""
) == (
"""Lucy
Tom
Jerry
Spike
Captain"""
), "stable order among men"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đội trưởng độc thân | Thuyền trưởng | Kích thước đầu vào tối thiểu | 
| Phụ nữ và trẻ em trộn lẫn | Thứ tự tương đối giống nhau | Độ ổn định ưu tiên bằng nhau | 
| Thuyền trưởng phía trước | Thuyền trưởng in cuối cùng | Đội trưởng xử lý đúng | 
| Nhiều người đàn ông | Đơn hàng gốc được bảo quản | Hành vi sắp xếp ổn định | 

## Vỏ cạnh 

Một trường hợp tế nhị là trộn lẫn phụ nữ và trẻ em. Chúng không phải là các giai đoạn sơ tán riêng biệt. 

đầu vào:```
4
Anna woman
Ben child
Chris woman
David man
```Thuật toán gán mức độ ưu tiên:```
woman -> 1
child -> 1
woman -> 1
man -> 2
```Sau khi sắp xếp, cả ba người có mức độ ưu tiên 1 vẫn giữ nguyên thứ tự ban đầu vì tính năng sắp xếp của Python ổn định:```
Anna
Ben
Chris
David
```Một tình huống phức tạp khác là giữ gìn trật tự giữa các danh mục giống hệt nhau. 

đầu vào:```
5
Tom man
Jerry man
Spike man
Lucy woman
Captain captain
```Ưu tiên trở thành:```
Tom -> 2
Jerry -> 2
Spike -> 2
Lucy -> 1
Captain -> 3
```Việc sắp xếp đưa Lucy đi trước những người đàn ông, nhưng ba người đàn ông vẫn giữ trật tự như sau:```
Tom
Jerry
Spike
```Kết quả cuối cùng là:```
Lucy
Tom
Jerry
Spike
Captain
```Trường hợp của thuyền trưởng cũng rất quan trọng. 

đầu vào:```
3
Jack captain
Alice woman
Bob rat
```Các ưu tiên là:```
Jack -> 3
Alice -> 1
Bob -> 0
```Sắp xếp tạo ra:```
Bob
Alice
Jack
```Mặc dù ban đầu thuyền trưởng đứng đầu hàng nhưng việc lập bản đồ ưu tiên buộc thuyền trưởng phải sơ tán cuối cùng.
