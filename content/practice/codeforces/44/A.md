---
title: "CF 44A - Mùa hè Ấn Độ"
description: "Mỗi chiếc lá được mô tả bằng hai chuỗi: loài cây và màu lá. Alyona chỉ giữ một chiếc lá nếu cô ấy chưa có chiếc lá khác có cùng cặp giá trị. Nhiệm vụ chỉ đơn giản là đếm xem có bao nhiêu sự kết hợp (loài, màu sắc) riêng biệt xuất hiện trong đầu vào."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 44
codeforces_index: "A"
codeforces_contest_name: "School Team Contest 2 (Winter Computer School 2010/11)"
rating: 900
weight: 44
solve_time_s: 83
verified: true
draft: false
---

[CF 44A - Mùa hè Ấn Độ](https://codeforces.com/problemset/problem/44/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chiếc lá được mô tả bằng hai chuỗi: loài cây và màu lá. Alyona chỉ giữ một chiếc lá nếu cô ấy chưa có chiếc lá khác có cùng cặp giá trị. 

Nhiệm vụ chỉ đơn giản là đếm xem có bao nhiêu`(species, color)`sự kết hợp xuất hiện trong đầu vào. Nếu cùng một cặp xuất hiện nhiều lần thì chỉ được tính một lần. 

Những hạn chế là rất nhỏ. Có tối đa 100 lá và mỗi chuỗi có độ dài tối đa là 10. Ngay cả một$O(n^2)$giải pháp dựa trên so sánh sẽ dễ dàng phù hợp trong thời hạn vì trường hợp xấu nhất sẽ chỉ thực hiện khoảng 10.000 so sánh. Điều này có nghĩa là vấn đề nằm ở việc triển khai đúng hơn là tối ưu hóa thuật toán. 

Nguồn gốc chính của sai lầm là do hiểu nhầm những gì được coi là trùng lặp. Hai chiếc lá chỉ được coi là giống hệt nhau nếu cả loài và màu sắc đều giống nhau. 

Hãy xem xét ví dụ này:```
3
birch yellow
birch green
birch yellow
```Câu trả lời đúng là:```
2
```Việc thực hiện bất cẩn chỉ theo dõi các loài sẽ trả về sai`1`. 

Một lỗi dễ mắc phải khác là đếm màu sắc độc lập với loài.```
3
maple red
birch red
oak red
```Câu trả lời đúng là:```
3
```Cả ba chiếc lá đều có màu giống nhau nhưng chúng đến từ những cây khác nhau. 

Trường hợp cạnh cuối cùng là khi mọi lá đều giống hệt nhau.```
4
oak green
oak green
oak green
oak green
```Câu trả lời đúng là:```
1
```Thuật toán phải tránh đếm các cặp lặp lại nhiều lần. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Chúng tôi có thể xử lý từng lá một và duy trì danh sách các lá duy nhất được tìm thấy cho đến nay. Đối với mỗi lá mới, chúng tôi quét danh sách và kiểm tra xem có giống nhau không`(species, color)`cặp đã tồn tại. Nếu không, chúng tôi thêm nó. 

Điều này có hiệu quả vì vấn đề chỉ hỏi liệu một cặp đã xuất hiện trước đó hay chưa. Vì có tối đa 100 lá nên trường hợp xấu nhất sẽ thực hiện khoảng:$$1 + 2 + 3 + \dots + 99 \approx 5000$$so sánh cặp, điều đó là hoàn toàn tốt. 

Hạn chế của phương pháp brute-force là nó quét liên tục các phần tử đã thấy trước đó. Nếu kích thước đầu vào lớn hơn nhiều thì việc tìm kiếm lặp đi lặp lại này sẽ trở nên tốn kém. 

Quan sát quan trọng là mô tả lá hoạt động tự nhiên giống như một khóa duy nhất. Bộ Python được thiết kế chính xác cho mục đích này. Một bộ sẽ tự động loại bỏ các bản sao, vì vậy nếu chúng ta chèn mọi`(species, color)`ghép thành một bộ, kích thước bộ cuối cùng chính xác là số lượng lá riêng biệt. 

Điều này thay đổi tìm kiếm tuyến tính lặp đi lặp lại thành trung bình$O(1)$các thao tác chèn và tra cứu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Đã chấp nhận | 
| Tối ưu |$O(n)$trung bình |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số lượng lá 
2. Tạo một tập hợp trống có tên`seen`. 

Bộ này sẽ lưu trữ các cặp`(species, color)`. Bộ tự động bỏ qua các bản sao. 
3. Lặp lại`n`lần: 

Đọc các loài và chuỗi màu từ đầu vào. 
4. Chèn cặp`(species, color)`vào bộ. 

Nếu cặp đã tồn tại thì tập hợp này không thay đổi. Nếu là mới, bộ này sẽ tăng thêm một. 
5. Sau khi xử lý hết các lá, in kích thước của bộ. 

Số phần tử trong tập hợp chính xác bằng số lượng mô tả lá riêng biệt. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến`seen`chứa mọi thứ độc đáo`(species, color)`cặp gặp phải cho đến nay. 

Bất cứ khi nào một lá mới được xử lý, việc chèn nó vào tập hợp sẽ giữ nguyên tính bất biến này. Các cặp trùng lặp không tạo thêm mục nhập, trong khi các cặp mới được thêm đúng một lần. 

Sau khi tất cả các lá được xử lý, mỗi mô tả lá riêng biệt sẽ xuất hiện chính xác một lần trong tập hợp. Do đó, kích thước của bộ này là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    seen = set()
    
    for _ in range(n):
        species, color = input().split()
        seen.add((species, color))
    
    print(len(seen))

solve()
```Giải pháp tuân theo thuật toán trực tiếp. 

bộ`seen`lưu trữ các bộ dữ liệu`(species, color)`. Các bộ dữ liệu có thể băm được trong Python nên chúng có thể được chèn vào một tập hợp một cách an toàn và hiệu quả. 

Dòng:```
seen.add((species, color))
```là cốt lõi của giải pháp. Nếu cặp này đã tồn tại thì không có gì thay đổi. Điều này tránh việc kiểm tra trùng lặp thủ công. 

sử dụng`input().split()`an toàn vì mỗi dòng luôn chứa chính xác hai chuỗi cách nhau bằng dấu cách. 

Không có điều kiện biên khó khăn trong việc thực hiện. Ngay cả khi tất cả các lá giống hệt nhau hoặc tất cả đều là duy nhất, bộ này vẫn tự động hoạt động chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
birch yellow
maple red
birch yellow
maple yellow
maple green
```| Bước | Lá Hiện Tại | Đặt nội dung | Đặt kích thước | 
| --- | --- | --- | --- | 
| 1 |`(birch, yellow)`|`{(birch, yellow)}`| 1 | 
| 2 |`(maple, red)`|`{(birch, yellow), (maple, red)}`| 2 | 
| 3 |`(birch, yellow)`|`{(birch, yellow), (maple, red)}`| 2 | 
| 4 |`(maple, yellow)`|`{(birch, yellow), (maple, red), (maple, yellow)}`| 3 | 
| 5 |`(maple, green)`|`{(birch, yellow), (maple, red), (maple, yellow), (maple, green)}`| 4 | 

Đầu ra cuối cùng:```
4
```Dấu vết này cho thấy việc chèn trùng lặp không làm thay đổi tập hợp như thế nào. 

### Ví dụ 2 

đầu vào:```
4
oak green
oak green
oak green
oak green
```| Bước | Lá Hiện Tại | Đặt nội dung | Đặt kích thước | 
| --- | --- | --- | --- | 
| 1 |`(oak, green)`|`{(oak, green)}`| 1 | 
| 2 |`(oak, green)`|`{(oak, green)}`| 1 | 
| 3 |`(oak, green)`|`{(oak, green)}`| 1 | 
| 4 |`(oak, green)`|`{(oak, green)}`| 1 | 

Đầu ra cuối cùng:```
1
```Ví dụ này chứng minh rằng các lá giống hệt nhau lặp đi lặp lại chỉ được tính một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$trung bình | Mỗi bộ chèn là trung bình$O(1)$| 
| Không gian |$O(n)$| Bộ này có thể lưu trữ tất cả các lá nếu tất cả đều là duy nhất | 

Với$n \le 100$, giải pháp dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng rất nhỏ vì chỉ lưu trữ tối đa 100 cặp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    
    seen = set()
    
    for _ in range(n):
        species, color = input().split()
        seen.add((species, color))
    
    print(len(seen))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""5
birch yellow
maple red
birch yellow
maple yellow
maple green
"""
) == "4\n", "sample 1"

# minimum input size
assert run(
"""1
oak red
"""
) == "1\n", "minimum case"

# all leaves identical
assert run(
"""4
oak green
oak green
oak green
oak green
"""
) == "1\n", "all equal"

# same color but different species
assert run(
"""3
oak red
birch red
maple red
"""
) == "3\n", "species matters"

# same species but different colors
assert run(
"""4
oak red
oak green
oak yellow
oak red
"""
) == "3\n", "color matters"

# maximum distinct small-style case
inp = "100\n"
for i in range(100):
    inp += f"tree{i} color{i}\n"

assert run(inp) == "100\n", "many unique pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lá đơn |`1`| Kích thước đầu vào tối thiểu | 
| Tất cả các lá giống hệt nhau |`1`| Duplicate removal |
 | Cùng màu, khác loài |`3`| Loài là một phần của chìa khóa | 
| Cùng loài, khác màu |`3`| Màu sắc là một phần của chìa khóa | 
| 100 chiếc lá độc đáo |`100`| Xử lý các mục riêng biệt tối đa | 

## Vỏ cạnh 

Hãy xem xét trường hợp chỉ có loài phù hợp:```
3
oak red
oak green
oak red
```Thuật toán xử lý các cặp theo thứ tự. 

Sau khi đọc`(oak, red)`, tập hợp chứa một phần tử. 

Sau khi đọc`(oak, green)`, cặp này khác nhau vì màu sắc thay đổi nên kích thước cài đặt trở thành hai. 

trận chung kết`(oak, red)`đã tồn tại trong tập hợp, vì vậy kích thước vẫn là hai. 

Đầu ra là:```
2
```Điều này xác nhận rằng thuật toán so sánh cả hai trường với nhau. 

Bây giờ hãy xem xét những chiếc lá có màu giống hệt nhau nhưng khác loài:```
3
birch yellow
maple yellow
oak yellow
```Mỗi lần chèn sẽ tạo ra một cặp mới vì mỗi lần các loài lại khác nhau. 

Bộ này phát triển như sau:```
{(birch, yellow)}
{(birch, yellow), (maple, yellow)}
{(birch, yellow), (maple, yellow), (oak, yellow)}
```Đầu ra trở thành:```
3
```Điều này ngăn ngừa sai lầm khi chỉ coi màu sắc là đặc điểm nhận dạng. 

Cuối cùng, hãy xem xét trường hợp trùng lặp hoàn toàn:```
5
pine green
pine green
pine green
pine green
pine green
```Lần chèn đầu tiên thêm`(pine, green)`đến trường quay. Mỗi lần chèn sau sẽ bị bỏ qua vì cặp này đã tồn tại. 

Kích thước đặt cuối cùng là:```
1
```Điều này xác nhận rằng các bản sao được tính chính xác một lần.
