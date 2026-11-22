---
title: "CF 31A - Sự tiến hóa của giun"
description: "Chúng tôi được cung cấp một loạt các chiều dài sâu. Mỗi vị trí đại diện cho một dạng sâu khác nhau và chúng ta cần tìm ba chỉ số riêng biệt sao cho độ dài tại một chỉ số bằng tổng độ dài ở hai chỉ số còn lại."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 31
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 31 (Div. 2, Codeforces format)"
rating: 1200
weight: 31
solve_time_s: 106
verified: true
draft: false
---
[CF 31A - Sự tiến hóa của giun](https://codeforces.com/problemset/problem/31/A) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt các chiều dài sâu. Mỗi vị trí đại diện cho một dạng sâu khác nhau và chúng ta cần tìm ba chỉ số riêng biệt sao cho độ dài tại một chỉ số bằng tổng độ dài ở hai chỉ số còn lại. 

Cụ thể hơn, chúng tôi muốn các chỉ số`i`,`j`, Và`k`Ở đâu:```
a[i] = a[j] + a[k]
```Tất cả các chỉ số phải khác nhau, nhưng bản thân các giá trị được phép lặp lại. Ví dụ: nếu mảng chứa hai vị trí khác nhau có giá trị`2`, cả hai đều có thể tham gia vào phương trình. 

Kích thước đầu vào rất nhỏ. Số lượng dạng sâu nhiều nhất`100`, và mỗi độ dài tối đa là`1000`. Với`n = 100`, thậm chí là một`O(n^3)`thuật toán chỉ thực hiện khoảng một triệu lượt kiểm tra, hoàn toàn an toàn trong giới hạn 2 giây trong Python. Điều này thay đổi cách chúng ta nghĩ về vấn đề. Chúng tôi không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. Việc thực hiện trực tiếp đã đủ nhanh. 

Phần khó khăn không phải là hiệu suất, mà là tính chính xác xung quanh các chỉ số riêng biệt. 

Hãy xem xét ví dụ này:```
3
2 1 1
```Câu trả lời đúng là:```
1 2 3
```Việc triển khai bất cẩn chỉ sử dụng các giá trị có thể vô tình sử dụng lại cùng một giá trị`1`hai lần từ cùng một vị trí. Vấn đề cho phép các giá trị bằng nhau, nhưng không cho phép các chỉ số bằng nhau. Chúng ta phải kiểm tra rõ ràng rằng cả ba vị trí đều khác nhau. 

Một lỗi dễ xảy ra khác khi tồn tại các giá trị trùng lặp:```
4
4 2 2 1
```Câu trả lời hợp lệ là:```
1 2 3
```Đây`4 = 2 + 2`, và hai`2`s đến từ các chỉ số khác nhau. Việc triển khai chỉ lưu trữ một chỉ mục cho mỗi giá trị trong bản đồ băm có thể kết luận không chính xác rằng không có giải pháp nào tồn tại. 

Cũng có trường hợp không tồn tại bộ ba hợp lệ:```
5
1 2 4 8 16
```Không có phần tử nào bằng tổng của hai phần tử khác nên kết quả đầu ra phải là`-1`. Một chương trình dừng quá sớm hoặc cho rằng giải pháp nào đó luôn tồn tại sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Chúng ta có thể thử mọi bộ ba chỉ số riêng biệt theo thứ tự`(i, j, k)`và kiểm tra xem:```
a[i] == a[j] + a[k]
```Điều này hoạt động vì các ràng buộc rất nhỏ. Với`n = 100`, số bộ ba nhiều nhất là:```
100 × 100 × 100 = 1,000,000
```Một triệu phép cộng và so sánh số nguyên đơn giản là chuyện nhỏ đối với Python. 

Phương pháp vũ phu cũng dễ lý giải. Vì nó kiểm tra rõ ràng mọi tổ hợp chỉ số hợp lệ có thể có nên nó không thể bỏ sót một giải pháp nào. Nếu tồn tại một bộ ba hợp lệ thì một trong các lần lặp sẽ gặp nó. 

Chúng tôi vẫn có thể nghĩ về cách cải thiện nó. Công việc lặp đi lặp lại xuất phát từ việc kiểm tra cả ba chỉ số một cách độc lập. Khi chúng tôi sửa hai chỉ số`j`Và`k`, giá trị mục tiêu`a[j] + a[k]`được biết đến ngay lập tức. Thay vì quét tất cả những gì có thể`i`, chúng ta có thể lưu trữ các giá trị trong bản đồ từ giá trị này đến chỉ mục khác và tra cứu xem tổng đó có tồn tại hay không. 

Điều đó dẫn đến một`O(n^2)`giải pháp. Đối với mỗi cặp`(j, k)`, chúng tôi tính toán:```
target = a[j] + a[k]
```Sau đó chúng tôi kiểm tra xem`target`tồn tại ở một nơi khác trong mảng tại một chỉ mục riêng biệt. 

Điều này có tác dụng vì bài toán yêu cầu mối quan hệ tổng và việc tra cứu theo giá trị sẽ rẻ hơn nhiều so với việc quét toàn bộ mảng nhiều lần. 

Mặc dù`O(n^3)`đã trôi qua một cách thoải mái rồi,`O(n^2)`phiên bản sạch hơn và thể hiện cấu trúc cơ bản của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng chiều dài sâu. 
2. Xây dựng một từ điển ánh xạ từng giá trị tới tất cả các chỉ mục nơi nó xuất hiện. 

Chúng tôi lưu trữ tất cả các chỉ mục thay vì chỉ một vì các giá trị trùng lặp rất quan trọng. Ví dụ,`4 = 2 + 2`yêu cầu hai vị trí khác nhau chứa`2`. 
3. Lặp lại từng cặp chỉ số`(j, k)`. 

Chúng đại diện cho hai con sâu có chiều dài được cộng lại với nhau. 
4. Bỏ qua các cặp ở đâu`j == k`. 

Vấn đề đòi hỏi ba dạng riêng biệt, do đó không thể sử dụng lại cùng một chỉ mục. 
5. Tính toán:```
target = a[j] + a[k]
```6. Kiểm tra xem`target`tồn tại trong từ điển. 

Nếu nó không tồn tại, không có giá trị`i`có thể thỏa mãn phương trình của cặp này. 
7. Đối với mọi chỉ mục được lưu trữ`i`tương ứng với`target`, kiểm tra xem`i`,`j`, Và`k`đều khác biệt. 

Điều này tránh các trường hợp không hợp lệ khi cùng một vị trí mảng tham gia nhiều lần. 
8. Ngay khi tìm thấy bộ ba hợp lệ, xuất các chỉ số ở dạng dựa trên 1 và kết thúc. 
9. Nếu hết các cặp mà không tìm được lời giải, hãy in`-1`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra một cách có hệ thống mọi cặp phụ lục có thể có`(j, k)`. Đối với mỗi cặp, nó tính toán giá trị chính xác cần thiết cho`a[i]`. Từ điển đảm bảo rằng nếu giá trị đó tồn tại ở bất kỳ đâu trong mảng, chúng ta có thể tìm thấy tất cả các vị trí khớp ngay lập tức. 

Vì mọi cặp hợp lệ đều được kiểm tra và mọi chỉ mục trùng khớp đều được xác minh về tính khác biệt nên thuật toán không thể bỏ sót câu trả lời đúng và không thể đưa ra câu trả lời không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    positions = {}

    for i, value in enumerate(a):
        if value not in positions:
            positions[value] = []
        positions[value].append(i)

    for j in range(n):
        for k in range(n):
            if j == k:
                continue

            target = a[j] + a[k]

            if target not in positions:
                continue

            for i in positions[target]:
                if i != j and i != k:
                    print(i + 1, j + 1, k + 1)
                    return

    print(-1)

solve()
```Phần đầu tiên của mã xây dựng một từ điển có tên`positions`. Mỗi khóa là một độ dài sâu và giá trị là danh sách tất cả các chỉ mục nơi độ dài đó xuất hiện. 

Sử dụng danh sách là chi tiết triển khai quan trọng. Nếu chúng tôi chỉ lưu trữ một chỉ mục cho mỗi giá trị, các trường hợp như`4 = 2 + 2`có thể thất bại vì cả hai`2`s phải đến từ các vị trí khác nhau. 

Các vòng lặp lồng nhau liệt kê từng cặp có thứ tự`(j, k)`. Các cặp có thứ tự được chấp nhận vì bài toán cho phép bất kỳ câu trả lời hợp lệ nào. Chúng tôi bỏ qua vụ án`j == k`ngay lập tức vì cùng một dạng sâu không thể được sử dụng hai lần. 

Đối với mỗi cặp, chúng tôi tính toán giá trị mục tiêu cần thiết và kiểm tra xem nó có tồn tại trong từ điển hay không. Nếu đúng như vậy, chúng tôi lặp qua tất cả các chỉ số ứng cử viên`i`chứa giá trị đó. 

Điều kiện cuối cùng:```
if i != j and i != k:
```đảm bảo cả ba chỉ số đều khác biệt. 

Đầu ra sử dụng`+1`bởi vì mảng Codeforces được lập chỉ mục từ`1`, trong khi Python sử dụng`0`lập chỉ mục dựa trên nội bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 5 7
```Dấu vết: 

| j | k | một[j] | một [k] | mục tiêu | Chỉ số phù hợp | hợp lệ gấp ba | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | 3 | [2] | vâng | 

Thuật toán ngay lập tức tìm thấy:```
a[2] = 3
a[0] + a[1] = 1 + 2 = 3
```Vì vậy, nó xuất ra:```
3 1 2
```Mọi hoán vị thỏa mãn phương trình đều được chấp nhận. 

Dấu vết này thể hiện ý tưởng tra cứu cốt lõi. Một khi cặp`(1, 2)`được cố định, giá trị yêu cầu trở thành xác định. 

### Ví dụ 2 

đầu vào:```
4
4 2 2 1
```Dấu vết: 

| j | k | một[j] | một [k] | mục tiêu | Chỉ số phù hợp | hợp lệ gấp ba | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 4 | [0] | vâng | 

Thuật toán tìm thấy:```
4 = 2 + 2
```sử dụng chỉ số`1`,`2`, Và`0`. 

Đầu ra:```
1 2 3
```Ví dụ này chứng minh tại sao việc lưu trữ tất cả các chỉ số lại quan trọng. hai`2`s đến từ các vị trí khác nhau mặc dù giá trị của chúng bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chúng tôi kiểm tra tất cả các cặp chỉ số có thứ tự | 
| Không gian | O(n) | Từ điển lưu trữ tất cả các chỉ số mảng | 

Với`n ≤ 100`, ngay cả các giải pháp khối cũng đủ nhanh. Giải pháp bậc hai chạy gần như ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    positions = {}

    for i, value in enumerate(a):
        if value not in positions:
            positions[value] = []
        positions[value].append(i)

    for j in range(n):
        for k in range(n):
            if j == k:
                continue

            target = a[j] + a[k]

            if target not in positions:
                continue

            for i in positions[target]:
                if i != j and i != k:
                    print(i + 1, j + 1, k + 1)
                    return

    print(-1)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("5\n1 2 3 5 7\n") in {
    "3 1 2",
    "3 2 1"
}, "sample 1"

# minimum size with solution
assert run("3\n2 1 1\n") in {
    "1 2 3",
    "1 3 2"
}, "minimum size"

# no valid triple
assert run("5\n1 2 4 8 16\n") == "-1", "no solution"

# duplicate values
assert run("4\n4 2 2 1\n") in {
    "1 2 3",
    "1 3 2"
}, "duplicate values"

# all equal values
assert run("5\n5 5 5 5 5\n") == "-1", "all equal without valid sum"

# boundary style case
large_case = "100\n" + " ".join(map(str, range(1, 101))) + "\n"
assert run(large_case) != "", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 2 1 1`| ba hợp lệ | Kích thước đầu vào hợp lệ tối thiểu | 
|`5 / 1 2 4 8 16`|`-1`| Xử lý đúng khi không có giải pháp | 
|`4 / 4 2 2 1`| ba hợp lệ | Giá trị trùng lặp với các chỉ số riêng biệt | 
|`5 / 5 5 5 5 5`|`-1`| Giá trị bằng nhau không tự động ngụ ý một giải pháp | 
|`1..100`| bất kỳ câu trả lời nào không trống | Xử lý kích thước hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
3
2 1 1
```Thuật toán lưu trữ:```
2 -> [0]
1 -> [1, 2]
```Khi kiểm tra chỉ số`(1, 2)`, mục tiêu trở thành`2`. Tra cứu từ điển trả về chỉ mục`0`, khác biệt với cả hai chỉ số bổ sung. Các đầu ra thuật toán:```
1 2 3
```Trường hợp này xác nhận rằng các giá trị bằng nhau được cho phép miễn là các chỉ số khác nhau. 

Bây giờ hãy xem xét:```
4
4 2 2 1
```Từ điển trở thành:```
4 -> [0]
2 -> [1, 2]
1 -> [3]
```Vì`(1, 2)`, mục tiêu là`4`. Thuật toán lấy chỉ mục`0`và xác minh rằng tất cả các chỉ số là khác biệt. 

Việc triển khai có lỗi chỉ lưu trữ một lần xuất hiện của mỗi giá trị có thể xử lý sai cấu trúc này, đặc biệt khi cần có các bản sao trong tổng. 

Cuối cùng, hãy xem xét một trường hợp không có giải pháp:```
5
1 2 4 8 16
```Mỗi tổng cặp được kiểm tra:```
1 + 2 = 3
1 + 4 = 5
2 + 4 = 6
...
```Không có giá trị nào trong số này tồn tại trong mảng. Các vòng lặp kết thúc mà không quay trở lại, do đó thuật toán sẽ in chính xác:```
-1
```Điều này xác nhận thuật toán không cho rằng giải pháp tồn tại và xác minh toàn diện tất cả các khả năng.
