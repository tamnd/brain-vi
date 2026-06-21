---
title: "CF 106B - Lựa chọn Laptop"
description: "Mỗi máy tính xách tay có bốn giá trị: tốc độ xử lý, kích thước RAM, kích thước ổ cứng và giá cả. Một máy tính xách tay được coi là lỗi thời nếu tồn tại một máy tính xách tay khác tốt hơn hoàn toàn về cả ba đặc tính kỹ thuật cùng một lúc."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 106
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 82 (Div. 2)"
rating: 1000
weight: 106
solve_time_s: 123
verified: true
draft: false
---

[CF 106B - Chọn máy tính xách tay](https://codeforces.com/problemset/problem/106/B) 

**Đánh giá:** 1000 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi máy tính xách tay có bốn giá trị: tốc độ xử lý, kích thước RAM, kích thước ổ cứng và giá cả. Một máy tính xách tay được coi là lỗi thời nếu tồn tại một máy tính xách tay khác tốt hơn hoàn toàn về cả ba đặc tính kỹ thuật cùng một lúc. Giá cả không quan trọng trong việc quyết định liệu một chiếc máy tính xách tay có lỗi thời hay không. 

Sau khi loại bỏ toàn bộ laptop lỗi thời, chúng ta phải chọn laptop còn lại có giá thành tối thiểu và in mục lục gốc của nó. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 laptop nên ngay cả một thuật toán so sánh từng cặp laptop cũng hoàn toàn an toàn. Một so sánh cặp đầy đủ thực hiện tối đa$100^2 = 10{,}000$kiểm tra, việc này không quan trọng trong giới hạn thời gian 2 giây. 

Khó khăn chính không phải là hiệu suất, mà là diễn giải chính xác điều kiện thống trị. Một máy tính xách tay chỉ trở nên lỗi thời nếu một máy tính xách tay khác có tốc độ lớn hơn, RAM lớn hơn và ổ cứng HDD lớn hơn cùng một lúc. 

Một sai lầm dễ mắc phải là coi "lớn hơn hoặc bằng" là đủ. Hãy xem xét đầu vào này:```
2
2000 1024 100 300
2000 2048 200 250
```Máy tính xách tay thứ hai tốt hơn về RAM và HDD, nhưng tốc độ xử lý tương đương, không lớn hơn nhiều. Không có máy tính xách tay đã lỗi thời. Câu trả lời đúng là:```
2
```Việc thực hiện bất cẩn bằng cách sử dụng`>=`sẽ loại bỏ sai máy tính xách tay đầu tiên. 

Một lỗi phổ biến khác là quên rằng giá cả không liên quan khi kiểm tra máy tính xách tay lỗi thời. Coi như:```
2
3000 2048 300 900
2500 1024 200 100
```Chiếc máy tính xách tay thứ hai đã lỗi thời mặc dù nó rẻ hơn nhiều. Câu trả lời đúng là:```
1
```Một số triển khai nhầm lẫn giữ máy tính xách tay rẻ hơn bất kể thông số kỹ thuật. 

Ngoài ra còn có một trường hợp tế nhị là một số máy tính xách tay không thể so sánh được. Ví dụ:```
3
3000 1024 500 400
2500 4096 300 350
3200 2048 200 300
```Không cái nào lấn át cái kia vì mỗi cái đều thua ít nhất một thuộc tính. Cả 3 đều là ứng cử viên nên chúng ta chỉ cần chọn cái rẻ nhất là laptop 3. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra mọi máy tính xách tay và hỏi xem liệu máy tính xách tay khác có vượt trội hơn nó hay không. Dành cho máy tính xách tay`i`, chúng tôi lặp lại trên tất cả các máy tính xách tay`j`. Nếu máy tính xách tay`j`có tốc độ, RAM và ổ cứng cao hơn hẳn, sau đó là máy tính xách tay`i`đã lỗi thời và có thể bị loại bỏ. 

Sau khi xác định tất cả các máy tính xách tay không lỗi thời, chúng tôi quét chúng và chọn ra chiếc có giá tối thiểu. 

Phương pháp vũ phu này đã đủ nhanh rồi. Với tối đa 100 laptop thì tổng số so sánh chỉ là 10.000. Mỗi so sánh kiểm tra ba số nguyên, do đó thời gian chạy thực sự là tức thời. 

Không cần phải sắp xếp, cấu trúc dữ liệu nâng cao hoặc thủ thuật tối ưu hóa vì ràng buộc là nhỏ một cách có chủ ý. Quan sát quan trọng là bài toán hỏi về sự thống trị theo cặp trên chính xác ba thuộc tính. Vì mọi máy tính xách tay đều có thể tương tác với mọi máy tính xách tay khác nên so sánh hoàn chỉnh là giải pháp đơn giản và rõ ràng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(1) | Đã chấp nhận | 

Đối với bài toán này, giải pháp brute-force cũng là giải pháp thực tế tối ưu. 

## Hướng dẫn thuật toán 

1. Đọc tất cả máy tính xách tay và lưu trữ các thuộc tính của chúng cùng với chỉ mục dựa trên 1 ban đầu của chúng. 
2. Dành cho mọi laptop`i`, ban đầu giả sử rằng nó không lỗi thời. 
3. So sánh laptop`i`chống lại mọi máy tính xách tay khác`j`. 
4. Nếu máy tính xách tay`j`có tốc độ cao hơn, RAM lớn hơn và ổ cứng lớn hơn, đánh dấu máy tính xách tay`i`như đã lỗi thời. 

Điều này phù hợp với định nghĩa chính xác từ báo cáo vấn đề. Giá trị bằng nhau không được tính. 
5. Sau tất cả các so sánh, chỉ giữ lại những máy tính xách tay chưa bao giờ bị đánh dấu là lỗi thời. 
6. Trong số các máy tính xách tay còn lại, hãy chọn chiếc có giá thành tối thiểu. 
7. In chỉ mục gốc của nó. 

### Tại sao nó hoạt động 

Một máy tính xách tay hợp lệ khi và chỉ khi không có máy tính xách tay nào khác vượt trội hơn nó về cả ba thông số kỹ thuật. Thuật toán kiểm tra tình trạng này một cách toàn diện cho mọi máy tính xách tay. Vì mọi cặp thống trị có thể đều được thử nghiệm nên không có máy tính xách tay lỗi thời nào có thể vượt qua bước lọc. 

Sau khi lọc, vấn đề giảm xuống còn việc chọn máy tính xách tay có chi phí tối thiểu trong số tất cả các ứng viên hợp lệ. Vì thuật toán quét mọi máy tính xách tay còn sót lại chính xác một lần nên nó luôn tìm ra mức tối thiểu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    laptops = []

    for i in range(n):
        speed, ram, hdd, cost = map(int, input().split())
        laptops.append((speed, ram, hdd, cost, i + 1))

    best_cost = float('inf')
    best_index = -1

    for i in range(n):
        s1, r1, h1, c1, idx1 = laptops[i]

        outdated = False

        for j in range(n):
            if i == j:
                continue

            s2, r2, h2, c2, idx2 = laptops[j]

            if s2 > s1 and r2 > r1 and h2 > h1:
                outdated = True
                break

        if not outdated and c1 < best_cost:
            best_cost = c1
            best_index = idx1

    print(best_index)

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ mọi máy tính xách tay cùng với chỉ mục ban đầu của nó vì đầu ra yêu cầu vị trí trong đầu vào chứ không phải dữ liệu máy tính xách tay. 

Các vòng lặp lồng nhau thực hiện kiểm tra ưu thế một cách trực tiếp. Đối với mỗi máy tính xách tay, chúng tôi tìm kiếm một máy tính xách tay khác tốt hơn hoàn toàn về cả ba đặc điểm. các`break`rất quan trọng vì một khi máy tính xách tay được cho là đã lỗi thời thì việc so sánh thêm là không cần thiết. 

Các toán tử so sánh nghiêm ngặt là chi tiết triển khai quan trọng nhất. Thay thế`>`với`>=`làm thay đổi ý nghĩa của bài toán và tạo ra kết quả không chính xác khi các thuộc tính bằng nhau. 

Bước lựa chọn cuối cùng chỉ xem xét những máy tính xách tay đã vượt qua bài kiểm tra lỗi thời. Vì tất cả các mức giá đều khác nhau nên máy tính xách tay giá tối thiểu là duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2100 512 150 200
2000 2048 240 350
2300 1024 200 320
2500 2048 80 300
2000 512 180 150
```| Máy tính xách tay | Thống trị? | Lý do | Chi phí ứng viên | 
| --- | --- | --- | --- | 
| 1 | Có | Laptop 3 tốt hơn ở cả ba thông số kỹ thuật | Đã xóa | 
| 2 | Không | Không có máy tính xách tay nào có thể đánh bại nó ở mọi nơi | 350 | 
| 3 | Không | Không có máy tính xách tay nào có thể đánh bại nó ở mọi nơi | 320 | 
| 4 | Không | HDD thấp nhưng không có laptop nào vượt qua được cả 3 | 300 | 
| 5 | Có | Laptop 3 thống trị nó | Đã xóa | 

Trong số các laptop còn lại, rẻ nhất là laptop 4 với giá 300. 

Đầu ra:```
4
```Ví dụ này cho thấy rằng một máy tính xách tay có ổ cứng HDD yếu hơn vẫn có thể tồn tại nếu không có ai thực sự giỏi hơn ở mọi danh mục cùng một lúc. 

### Ví dụ 2 

đầu vào:```
3
3000 1024 500 400
2500 4096 300 350
3200 2048 200 300
```| Máy tính xách tay | Thống trị? | Tại sao không thống trị | Chi phí ứng viên | 
| --- | --- | --- | --- | 
| 1 | Không | Ổ cứng cao nhất | 400 | 
| 2 | Không | RAM cao nhất | 350 | 
| 3 | Không | Tốc độ cao nhất | 300 | 

Không có máy tính xách tay nào thống trị máy tính xách tay khác vì mỗi máy tính đều chiến thắng ở một hạng mục nào đó. 

Laptop hợp lệ rẻ nhất là laptop 3. 

Đầu ra:```
3
```Dấu vết này chứng tỏ rằng sự thống trị đòi hỏi sự vượt trội ở cả ba thông số kỹ thuật cùng một lúc chứ không chỉ hai trong ba thông số kỹ thuật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mọi máy tính xách tay đều được so sánh với mọi máy tính xách tay khác | 
| Không gian | O(1) | Chỉ một số biến ngoài mảng đầu vào được sử dụng | 

Với$n \le 100$, nghiệm bậc hai thực hiện tối đa 10.000 phép so sánh, con số này rất nhỏ đối với phần cứng hiện đại. Việc sử dụng bộ nhớ cũng không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    laptops = []

    for i in range(n):
        speed, ram, hdd, cost = map(int, input().split())
        laptops.append((speed, ram, hdd, cost, i + 1))

    best_cost = float('inf')
    best_index = -1

    for i in range(n):
        s1, r1, h1, c1, idx1 = laptops[i]

        outdated = False

        for j in range(n):
            if i == j:
                continue

            s2, r2, h2, c2, idx2 = laptops[j]

            if s2 > s1 and r2 > r1 and h2 > h1:
                outdated = True
                break

        if not outdated and c1 < best_cost:
            best_cost = c1
            best_index = idx1

    print(best_index)

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
    "5\n"
    "2100 512 150 200\n"
    "2000 2048 240 350\n"
    "2300 1024 200 320\n"
    "2500 2048 80 300\n"
    "2000 512 180 150\n"
) == "4", "sample 1"

# minimum size
assert run(
    "1\n"
    "3000 2048 500 700\n"
) == "1", "single laptop"

# equal attribute edge case
assert run(
    "2\n"
    "2000 1024 100 300\n"
    "2000 2048 200 250\n"
) == "2", "equal speed should not dominate"

# cheaper outdated laptop
assert run(
    "2\n"
    "3000 2048 300 900\n"
    "2500 1024 200 100\n"
) == "1", "price does not affect dominance"

# incomparable laptops
assert run(
    "3\n"
    "3000 1024 500 400\n"
    "2500 4096 300 350\n"
    "3200 2048 200 300\n"
) == "3", "choose cheapest among non-dominated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Máy tính xách tay đơn | 1 | Đầu vào kích thước tối thiểu | 
| Trường hợp tốc độ bằng nhau | 2 | Xử lý bất bình đẳng nghiêm ngặt | 
| Laptop lỗi thời giá rẻ | 1 | Giá bị bỏ qua trong quá trình kiểm tra sự thống trị | 
| Máy tính xách tay có một không hai | 3 | Xử lý đúng ưu thế cục bộ | 

## Vỏ cạnh 

Xét lại trường hợp đẳng thức:```
2
2000 1024 100 300
2000 2048 200 250
```Laptop 2 có RAM và HDD tốt hơn nhưng tốc độ xử lý ngang nhau. Thuật toán kiểm tra:```
2000 > 2000
```Điều này là sai nên laptop 1 không bị đánh dấu là lỗi thời. Cả hai máy tính xách tay đều tồn tại và chiếc rẻ hơn, máy tính xách tay 2, được chọn. Đầu ra là:```
2
```Bây giờ hãy xem xét một chiếc máy tính xách tay giá rẻ nhưng đã lỗi thời:```
2
3000 2048 300 900
2500 1024 200 100
```Khi so sánh laptop 2 với laptop 1:```
3000 > 2500
2048 > 1024
300 > 200
```Cả ba điều kiện đều đúng nên laptop 2 đã lỗi thời dù giá rẻ hơn. Chỉ có laptop 1 còn hiệu lực nên đáp án là:```
1
```Cuối cùng, hãy xem xét những chiếc máy tính xách tay có một không hai:```
3
3000 1024 500 400
2500 4096 300 350
3200 2048 200 300
```Mỗi máy tính xách tay thua ít nhất một hạng mục trong mỗi lần so sánh. Không có gì được đánh dấu là lỗi thời. Sau đó, thuật toán chỉ cần chọn mức giá tối thiểu trong số cả ba ứng cử viên, đó là máy tính xách tay 3.
