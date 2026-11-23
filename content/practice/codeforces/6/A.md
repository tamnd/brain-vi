---
title: "CF 6A - Tam giác"
description: "Chúng ta có bốn thanh dài và chúng ta phải chọn chính xác ba trong số chúng. Tùy thuộc vào mối quan hệ giữa ba độ dài đó, có ba kết quả có thể xảy ra."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry"]
categories: ["algorithms"]
codeforces_contest: 6
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 6 (Div. 2 Only)"
rating: 900
weight: 6
solve_time_s: 78
verified: true
draft: false
---
[CF 6A - Tam giác](https://codeforces.com/problemset/problem/6/A) 

**Xếp hạng:** 900 
**Tags:** lực lượng vũ phu, hình học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn thanh dài và chúng ta phải chọn chính xác ba trong số chúng. Tùy thuộc vào mối quan hệ giữa ba độ dài đó, có ba kết quả có thể xảy ra. 

Nếu các que được chọn có thể tạo thành một hình tam giác có diện tích dương, chúng ta sẽ in`TRIANGLE`. Về mặt hình học, điều này có nghĩa là tổng của hai cạnh nhỏ hơn hoàn toàn lớn hơn cạnh lớn nhất. 

Nếu không thể có một tam giác thích hợp, chúng ta vẫn kiểm tra xem các que có thể nằm trên một đường thẳng hay không. Điều đó xảy ra khi tổng của hai cạnh nhỏ bằng chính xác cạnh lớn nhất. Trong trường hợp đó chúng tôi in`SEGMENT`. 

Nếu không có điều kiện nào có thể xảy ra đối với bất kỳ sự lựa chọn nào trong ba que tính, chúng ta sẽ in`IMPOSSIBLE`. 

Kích thước đầu vào rất nhỏ. Chúng tôi chỉ có bốn số, vì vậy có chính xác bốn bộ ba khác nhau để kiểm tra. Ngay cả một giải pháp mạnh mẽ để kiểm tra mọi sự kết hợp cũng sẽ chạy ngay lập tức. Với những ràng buộc nhỏ như vậy, thách thức không phải là tối ưu hóa mà là xử lý chính xác các điều kiện hình học. 

Sai lầm phổ biến nhất là nhầm lẫn giữa bất đẳng thức tam giác chặt và không chặt. Một tam giác thích hợp yêu cầu:$$a + b > c$$sau khi sắp xếp ba cạnh sao cho$c$là lớn nhất. 

Một tam giác suy biến mà bài toán gọi là một đoạn, yêu cầu:$$a + b = c$$sử dụng`>=`thay vì`>`sẽ phân loại không chính xác các đoạn thành hình tam giác. 

Hãy xem xét đầu vào:```
1 2 3 10
```Những cây gậy`1 2 3`tạo thành một đoạn thẳng vì`1 + 2 = 3`. Đầu ra đúng là:```
SEGMENT
```Việc thực hiện bất cẩn bằng cách sử dụng`>=`sẽ in sai`TRIANGLE`. 

Một sai lầm dễ mắc phải khác là dừng quá sớm sau khi chỉ kiểm tra một bộ ba. Ví dụ:```
1 1 2 3
```Bộ ba`1 1 2`chỉ là một phân đoạn, nhưng`1 2 3`là không thể. Vì không có tam giác riêng nào tồn tại nên câu trả lời đúng vẫn là:```
SEGMENT
```Chúng ta phải xem xét tất cả bốn bộ ba có thể có trước khi quyết định. 

Cũng có trường hợp ngược lại:```
4 2 1 3
```Bộ ba`1 2 3`chỉ là một phân đoạn, nhưng`2 3 4`tạo thành một tam giác hợp lệ vì`2 + 3 > 4`. Câu trả lời đúng là:```
TRIANGLE
```Một triển khai in ngay lập tức`SEGMENT`sau khi nhìn thấy một trường hợp thoái hóa sẽ là sai lầm. Hình tam giác phù hợp luôn được ưu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là tạo ra mỗi bộ ba cây gậy từ bốn cây gậy có sẵn. Chỉ có bốn kết hợp như vậy nên chúng ta có thể đơn giản kiểm tra từng kết hợp một cách độc lập. 

Đối với mỗi bộ ba, chúng tôi sắp xếp ba độ dài. Sau khi sắp xếp, chúng ta chỉ cần so sánh tổng của hai giá trị nhỏ hơn với giá trị lớn nhất. 

Nếu như:$$a + b > c$$thì chúng ta đã biết câu trả lời phải là`TRIANGLE`, bởi vì một tam giác thích hợp có mức độ ưu tiên cao hơn mọi kết quả khác. 

Thay vào đó, nếu đẳng thức giữ nguyên:$$a + b = c$$thì chúng ta nhớ rằng một đoạn có thể xảy ra, nhưng chúng ta vẫn tiếp tục kiểm tra các bộ ba khác phòng trường hợp một tam giác thực xuất hiện sau đó. 

Giải pháp vũ phu đã hoàn toàn được chấp nhận vì số lượng thao tác không đổi. Chúng tôi chỉ kiểm tra bốn bộ ba, mỗi bộ bao gồm một loại ba số rất nhỏ. 

Không có tối ưu hóa tiệm cận có ý nghĩa nào ngoài điều này. Điều quan trọng cần lưu ý là kích thước đầu vào là cố định và cực kỳ nhỏ nên giải pháp sạch nhất cũng là giải pháp tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các bộ ba | O(1) | O(1) | Đã chấp nhận | 
| Kiểm tra dựa trên quan sát được tối ưu hóa | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn chiều dài thanh thành một mảng. 
2. Tạo mọi sự kết hợp của ba que từ bốn que có sẵn. Vì chỉ có bốn que nên sẽ tạo ra chính xác bốn bộ ba. 
3. Với mỗi bộ ba, hãy sắp xếp ba độ dài theo thứ tự không giảm. 
4. Hãy để các bên được sắp xếp`a`,`b`, Và`c`, Ở đâu`c`là cạnh lớn nhất. 
5. Nếu`a + b > c`, in ngay`TRIANGLE`và dừng lại. Một tam giác hợp lệ có mức độ ưu tiên cao nhất nên không cần kiểm tra thêm nữa. 
6. Nếu`a + b == c`, hãy nhớ rằng tồn tại một tam giác suy biến. Đừng dừng lại vì bộ ba khác vẫn có thể tạo thành một tam giác đúng. 
7. Sau khi kiểm tra tất cả các bộ ba, hãy in`SEGMENT`nếu có ít nhất một tam giác suy biến được tìm thấy. 
8. Nếu không thì in`IMPOSSIBLE`. 

### Tại sao nó hoạt động 

Việc sắp xếp từng bộ ba làm giảm điều kiện của tam giác thành một so sánh duy nhất liên quan đến cạnh lớn nhất. Đối với ba mặt được sắp xếp bất kỳ`a ≤ b ≤ c`, bất đẳng thức tam giác chỉ cần kiểm tra xem`a + b`lớn hơn`c`. 

Thuật toán kiểm tra mọi lựa chọn có thể có của ba thanh, do đó nó không thể bỏ lỡ cấu hình hợp lệ. Từ`TRIANGLE`có mức độ ưu tiên cao hơn`SEGMENT`, ngay lập tức quay lại sau khi tìm được tam giác thích hợp là đúng. Nếu không tồn tại tam giác riêng nhưng có ít nhất một trường hợp đẳng thức xuất hiện thì cách phân loại đúng là`SEGMENT`. Nếu cả hai điều kiện đều không xảy ra thì không thể hình thành bất kỳ loại tam giác nào. 

## Giải pháp Python```python
import sys
from itertools import combinations

input = sys.stdin.readline

sticks = list(map(int, input().split()))

segment = False

for triple in combinations(sticks, 3):
    a, b, c = sorted(triple)

    if a + b > c:
        print("TRIANGLE")
        sys.exit()

    if a + b == c:
        segment = True

if segment:
    print("SEGMENT")
else:
    print("IMPOSSIBLE")
```Giải pháp bắt đầu bằng cách đọc chiều dài bốn thanh. Vì chúng ta cần mọi nhóm ba que có thể có,`itertools.combinations`là cách sạch nhất để tạo ra chúng. 

Mỗi bộ ba được sắp xếp trước khi kiểm tra điều kiện của tam giác. Bước này rất cần thiết vì việc so sánh chỉ hoạt động chính xác khi`c`là cạnh lớn nhất. Việc quên sắp xếp có thể tạo ra kết quả không chính xác. 

Điều kiện đầu tiên kiểm tra một tam giác thích hợp bằng cách sử dụng bất đẳng thức nghiêm ngặt. Thời điểm bộ ba như vậy được tìm thấy, chương trình sẽ in`TRIANGLE`và thoát ra ngay lập tức. Không có bộ ba nào muộn hơn có thể tạo ra câu trả lời có mức độ ưu tiên cao hơn. 

Điều kiện thứ hai kiểm tra sự bình đẳng. Thay vì in ngay lập tức, chúng tôi lưu kết quả vào`segment`lá cờ. Điều này tránh được sai lầm trước đó khi bộ ba sau vẫn có thể tạo thành một tam giác đúng. 

Cuối cùng, nếu không tìm thấy hình tam giác nào nhưng tồn tại ít nhất một cấu hình phân đoạn, chương trình sẽ in`SEGMENT`. Nếu không thì câu trả lời là`IMPOSSIBLE`. 

Các con số cực kỳ nhỏ nên việc tràn số nguyên là không thể trong Python. Việc thực hiện chủ yếu là về việc sắp xếp đúng thứ tự điều kiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2 1 3
```| Ba | Sắp xếp ba | Kiểm tra | Kết quả | 
| --- | --- | --- | --- | 
| (4,2,1) | (1,2,4) | 1 + 2 < 4 | Không thể | 
| (4,2,3) | (2,3,4) | 2 + 3 > 4 | Tìm thấy tam giác | 

Thuật toán dừng ngay sau khi tìm thấy`(2,3,4)`bởi vì một tam giác thích hợp có quyền ưu tiên hơn mọi kết quả khác. Câu trả lời cuối cùng là:```
TRIANGLE
```Dấu vết này chứng tỏ tại sao chúng ta không thể dừng lại sau khi nhìn thấy một bộ ba thất bại. Các lựa chọn gậy khác nhau có thể hoạt động khác nhau. 

### Ví dụ 2 

đầu vào:```
1 2 3 10
```| Ba | Sắp xếp ba | Kiểm tra | Kết quả | 
| --- | --- | --- | --- | 
| (1,2,3) | (1,2,3) | 1 + 2 = 3 | Phân đoạn | 
| (1,2,10) | (1,2,10) | 1 + 2 < 10 | Không thể | 
| (1,3,10) | (1,3,10) | 1 + 3 < 10 | Không thể | 
| (2,3,10) | (2,3,10) | 2 + 3 < 10 | Không thể | 

Không có tam giác riêng nào tồn tại, nhưng tồn tại một trường hợp suy biến. Câu trả lời trở thành:```
SEGMENT
```Ví dụ này khẳng định tầm quan trọng của việc phân biệt rõ ràng sự bất bình đẳng với sự bình đẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có bốn bộ ba được kiểm tra | 
| Không gian | O(1) | Sử dụng một vài biến bất kể kích thước đầu vào | 

Chương trình thực hiện một lượng công việc không đổi vì kích thước đầu vào được cố định ở bốn số. Thời gian chạy và mức sử dụng bộ nhớ thực sự tức thời so với giới hạn của sự cố. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import combinations

def solve():
    input = sys.stdin.readline

    sticks = list(map(int, input().split()))

    segment = False

    for triple in combinations(sticks, 3):
        a, b, c = sorted(triple)

        if a + b > c:
            print("TRIANGLE")
            return

        if a + b == c:
            segment = True

    if segment:
        print("SEGMENT")
    else:
        print("IMPOSSIBLE")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("4 2 1 3\n") == "TRIANGLE", "sample 1"

# custom cases
assert run("1 2 3 10\n") == "SEGMENT", "degenerate triangle exists"
assert run("1 1 3 5\n") == "IMPOSSIBLE", "no valid combination"
assert run("5 5 5 5\n") == "TRIANGLE", "all equal sticks"
assert run("100 1 1 2\n") == "SEGMENT", "boundary equality case"
assert run("2 3 4 9\n") == "TRIANGLE", "one valid triple among failures"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 10`|`SEGMENT`| Bình đẳng không được tính là hình tam giác | 
|`1 1 3 5`|`IMPOSSIBLE`| Không có bộ ba nào thỏa mãn mọi điều kiện | 
|`5 5 5 5`|`TRIANGLE`| Tam giác vuông có các cạnh bằng nhau | 
|`100 1 1 2`|`SEGMENT`| Mất cân bằng lớn với một trường hợp suy biến | 
|`2 3 4 9`|`TRIANGLE`| Sau này gấp ba vẫn có thể thành công | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 2 3 10
```Thuật toán kiểm tra`(1,2,3)`Đầu tiên. Sau khi sắp xếp, chúng tôi nhận được`1 2 3`, và kể từ đó`1 + 2 = 3`, cái`segment`cờ trở thành`True`. 

Các bộ ba còn lại đều không đúng bất đẳng thức. Vì không tìm thấy tam giác thích hợp nên kết quả cuối cùng là:```
SEGMENT
```Trường hợp này khẳng định rằng đẳng thức phải được xử lý tách biệt khỏi một tam giác thực. 

Bây giờ hãy xem xét:```
4 2 1 3
```Bộ ba`(1,2,3)`chỉ tạo thành một phân đoạn, nhưng sau đó thuật toán sẽ kiểm tra`(2,3,4)`. Từ`2 + 3 > 4`, nó in ngay:```
TRIANGLE
```Điều này chứng tỏ tại sao chúng ta không thể dừng lại sau khi nhìn thấy một tam giác suy biến. Sự kết hợp sau này vẫn có thể tạo thành một sự kết hợp thích hợp. 

Cuối cùng, hãy xem xét:```
1 1 3 5
```Mỗi bộ ba đều thất bại với điều kiện:$$a + b \ge c$$Thuật toán không bao giờ đặt`segment`cờ và không bao giờ tìm thấy một hình tam giác thích hợp. Câu trả lời cuối cùng là:```
IMPOSSIBLE
```Điều này xác minh rằng thuật toán loại bỏ chính xác các cấu hình trong đó ngay cả sự sắp xếp theo đường thẳng cũng không thể được hình thành.
