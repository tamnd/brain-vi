---
title: "CF 106100A - Giảm thiểu sự đảo ngược với tiền tố được xác định trước"
description: "Chúng ta có m phần tử đầu tiên của hoán vị số 1...n. Những yếu tố này là cố định và không thể thay đổi. N - m vị trí còn lại phải được điền chính xác bằng các số không xuất hiện trong tiền tố cố định."
date: "2026-06-25T11:51:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106100
codeforces_index: "A"
codeforces_contest_name: "International MathCoding Narxoz open olympiad 2025"
rating: 0
weight: 106100
solve_time_s: 46
verified: true
draft: false
---

[CF 106100A - Giảm thiểu sự đảo ngược với tiền tố được xác định trước](https://codeforces.com/problemset/problem/106100/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao cái đầu tiên`m`các phần tử của hoán vị số`1...n`. Những yếu tố này là cố định và không thể thay đổi. 

Phần còn lại`n - m`các vị trí phải được điền chính xác bằng các số không xuất hiện trong tiền tố cố định. Sau khi điền vào các vị trí đó, toàn bộ mảng phải là một hoán vị hợp lệ chứa mọi số từ`1`ĐẾN`n`đúng một lần. 

Trong số tất cả các cách hợp lệ để hoàn thành phép hoán vị, chúng ta muốn cách có số lần đảo ngược nhỏ nhất có thể. 

Sự đảo ngược là một cặp vị trí`(i, j)`với`i < j`Và`p[i] > p[j]`. 

Những ràng buộc cho phép`n`lên tới`10^5`. Bất kỳ thuật toán nào tính toán rõ ràng số lần đảo ngược cho mỗi lần hoàn thành có thể xảy ra đều cực kỳ tốn kém. Ngay cả việc kiểm tra tất cả các hoán vị của các số còn lại cũng sẽ yêu cầu`(n-m)!`những khả năng gần như ngay lập tức trở thành không thể. Chúng tôi cần một công trình trực tiếp trong khoảng`O(n log n)`thời gian hoặc tốt hơn. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Giả sử tiền tố cố định đã tăng lên:```
n = 5
m = 4
prefix = [1, 2, 3, 4]
```Số duy nhất còn thiếu là`5`, vậy đáp án phải là:```
5
```Một trường hợp thú vị khác là:```
n = 4
m = 2
prefix = [4, 1]
```Các số còn thiếu là`{2, 3}`. 

Nếu chúng ta đặt chúng như`[3, 2]`, bản thân hậu tố đã chứa một phép đảo ngược. 

Nếu chúng ta đặt chúng như`[2, 3]`, hậu tố không chứa đảo ngữ. 

Các phép đảo ngược liên quan đến tiền tố cố định giống hệt nhau ở cả hai cách hoàn thành, do đó cách sắp xếp thứ hai là tối ưu. 

Một cách tiếp cận bất cẩn có thể cố gắng làm cho các phần tử tiền tố lớn hơn tương tác tốt hơn với hậu tố, nhưng những sự đảo ngược chéo đó chỉ được xác định bởi giá trị nào bị thiếu chứ không phải theo thứ tự đặt các giá trị bị thiếu đó. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra mọi hoán vị của các số bị thiếu, nối nó vào tiền tố cố định, đếm số lần đảo ngược trong hoán vị kết quả và giữ lại số tốt nhất. 

Điều này đúng vì nó kiểm tra mọi lần hoàn thành hợp lệ. Vấn đề là số lần hoàn thành. Nếu có`k = n-m`số còn thiếu thì có`k!`khả năng. Ngay cả đối với`k = 15`, điều này đã vượt xa giới hạn thực tế. 

Quan sát quan trọng là chia nghịch đảo thành hai nhóm. 

Nhóm đầu tiên bao gồm các đảo ngữ hoàn toàn bên trong hậu tố. Những điều này phụ thuộc vào thứ tự chúng ta chọn cho các số còn thiếu. 

Nhóm thứ hai bao gồm các đảo ngược trong đó phần tử bên trái nằm ở tiền tố cố định và phần tử bên phải nằm ở hậu tố. 

Hãy xem xét một giá trị tiền tố cố định`x`. Mọi số còn thiếu đều nhỏ hơn`x`tạo ra sự đảo ngược với`x`và mọi số còn thiếu lớn hơn`x`không. Việc đếm chỉ phụ thuộc vào tập hợp số còn thiếu chứ không phụ thuộc vào thứ tự của chúng. Cho dù chúng ta sắp xếp hậu tố như thế nào thì số lần đảo ngược tiền tố-hậu tố vẫn không thay đổi. 

Điều đó có nghĩa là phần duy nhất chúng ta có thể tác động là số lần đảo ngược bên trong hậu tố đó. 

Trong số một tập hợp các số riêng biệt, số lần đảo ngược tối thiểu có thể bằng 0, đạt được bằng cách sắp xếp chúng theo thứ tự tăng dần. 

Vì vậy, sự hoàn thành tối ưu chỉ đơn giản là những con số còn thiếu được sắp xếp tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-m)! · n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) hoặc O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Đọc tiền tố cố định. 
3. Đánh dấu mọi số đã xuất hiện ở tiền tố. 
4. Quét số từ`1`ĐẾN`n`. 
5. Mọi số không được đánh dấu đều là số còn thiếu và phải thuộc hậu tố. 
6. In tất cả các số còn thiếu theo thứ tự tăng dần. 

Lý do bước 6 là tối ưu là vì số lượng đảo ngược tiền tố-hậu tố được cố định bất kể thứ tự hậu tố, trong khi thứ tự tăng dần khiến bản thân hậu tố không có số lần đảo ngược, đây là giá trị nhỏ nhất có thể. 

### Tại sao nó hoạt động 

Tập hợp số bị thiếu hoàn toàn được xác định bởi tiền tố cố định. 

Đối với mọi phần tử tiền tố`p[i]`, số phần tử hậu tố nhỏ hơn`p[i]`được sửa vì hậu tố luôn chứa chính xác cùng một bộ giá trị bị thiếu. Việc sắp xếp lại các giá trị đó không thể thay đổi số lượng chúng nhỏ hơn`p[i]`. 

Vì vậy, mọi nghịch đảo liên quan đến ít nhất một phần tử tiền tố đều đóng góp một lượng không đổi vào câu trả lời. 

Những đảo ngữ duy nhất còn lại là những đảo ngữ được hình thành bởi các cặp hoàn toàn bên trong hậu tố. Một dãy số riêng biệt có nghịch đảo bằng 0 khi và chỉ khi nó tăng nghiêm ngặt. Vì số 0 là số lần đảo ngược tối thiểu có thể có nên việc sắp xếp các số còn thiếu ngày càng tạo ra sự hoàn thành tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
prefix = list(map(int, input().split()))

used = [False] * (n + 1)

for x in prefix:
    used[x] = True

ans = []
for x in range(1, n + 1):
    if not used[x]:
        ans.append(str(x))

print(" ".join(ans))
```Việc thực hiện tuân theo bằng chứng trực tiếp. 

các`used`mảng ghi lại những giá trị đã xuất hiện trong tiền tố cố định. Vì tiền tố chứa các giá trị riêng biệt từ một hoán vị nên mỗi giá trị được đánh dấu nhiều nhất một lần. 

Sau đó, một lần quét từ`1`ĐẾN`n`thu thập tất cả các số không được sử dụng. Vì quá trình quét được thực hiện theo thứ tự tăng dần nên hậu tố kết quả đã được sắp xếp. 

Không cần đếm ngược. Bằng chứng cho thấy các số còn thiếu đã được sắp xếp luôn tối ưu. 

Một điểm tinh tế là chúng tôi chỉ xuất ra hậu tố chứ không xuất ra toàn bộ hoán vị. Vấn đề yêu cầu các giá trị chiếm vị trí`m+1`bởi vì`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 7
5 1 2 9 3 4 6
```Số còn thiếu là`{7, 8, 10}`. 

| Số hiện tại | Được sử dụng trong tiền tố? | Đã thêm vào câu trả lời | 
| --- | --- | --- | 
| 1 | Có | Không | 
| 2 | Có | Không | 
| 3 | Có | Không | 
| 4 | Có | Không | 
| 5 | Có | Không | 
| 6 | Có | Không | 
| 7 | Không | Có | 
| 8 | Không | Có | 
| 9 | Có | Không | 
| 10 | Không | Có | 

Đầu ra:```
7 8 10
```Ví dụ này cho thấy rằng khi đã biết tập hợp còn thiếu, việc sắp xếp nó theo thứ tự tăng dần sẽ ngay lập tức cho hậu tố tối ưu. 

### Ví dụ 2 

đầu vào:```
5 4
1 2 3 4
```Số còn thiếu là`{5}`. 

| Số hiện tại | Được sử dụng trong tiền tố? | Đã thêm vào câu trả lời | 
| --- | --- | --- | 
| 1 | Có | Không | 
| 2 | Có | Không | 
| 3 | Có | Không | 
| 4 | Có | Không | 
| 5 | Không | Có | 

Đầu ra:```
5
```Điều này thể hiện kích thước hậu tố nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để đánh dấu các giá trị đã sử dụng và một lượt để thu thập các giá trị còn thiếu | 
| Không gian | O(n) | Mảng Boolean có kích thước`n+1`| 

Với`n ≤ 10^5`, thời gian tuyến tính và bộ nhớ tuyến tính dễ dàng nằm trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    prefix = list(map(int, input().split()))

    used = [False] * (n + 1)

    for x in prefix:
        used[x] = True

    ans = []
    for x in range(1, n + 1):
        if not used[x]:
            ans.append(str(x))

    print(" ".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided samples
assert run("10 7\n5 1 2 9 3 4 6\n") == "7 8 10", "sample 1"
assert run("5 4\n1 2 3 4\n") == "5", "sample 2"

# custom cases
assert run("2 1\n2\n") == "1", "minimum valid size"
assert run("4 2\n4 1\n") == "2 3", "sorted missing values"
assert run("6 5\n6 5 4 3 2\n") == "1", "single missing value"
assert run("8 3\n2 4 6\n") == "1 3 5 7 8", "multiple gaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 2`|`1`| Kích thước đầu vào hợp lệ tối thiểu | 
|`4 2 / 4 1`|`2 3`| Số bị thiếu phải được sắp xếp | 
|`6 5 / 6 5 4 3 2`|`1`| Hậu tố đơn phần tử | 
|`8 3 / 2 4 6`|`1 3 5 7 8`| Nhiều khoảng bị thiếu | 

## Vỏ cạnh 

Hãy xem xét:```
4 2
4 1
```Các số còn thiếu là`{2, 3}`. 

Các đầu ra thuật toán:```
2 3
```Hoán vị hoàn thành là:```
4 1 2 3
```Bất kỳ thứ tự nào khác của hậu tố, chẳng hạn như`3 2`, giới thiệu một sự đảo ngược bổ sung bên trong hậu tố. Đảo ngược tiền tố-hậu tố không thay đổi vì tập hợp bị thiếu vẫn còn`{2,3}`. Thuật toán chọn chính xác sự sắp xếp với đảo ngược hậu tố bằng 0. 

Bây giờ hãy xem xét:```
5 4
1 2 3 4
```Giá trị còn thiếu duy nhất là`5`. 

Quá trình quét từ`1`ĐẾN`5`tìm thấy chính xác một số chưa được đánh dấu và đưa ra kết quả:```
5
```Không có quyền tự do lựa chọn và thuật toán xử lý trường hợp ranh giới này một cách tự nhiên. 

Cuối cùng, hãy xem xét:```
7 3
7 6 5
```Số còn thiếu là:```
1 2 3 4
```Thuật toán xuất chúng theo thứ tự tăng dần. 

Mặc dù tiền tố chứa các giá trị lớn tạo ra nhiều đảo ngược với hậu tố, nhưng những đảo ngược đó là không thể tránh khỏi vì mọi giá trị bị thiếu đều nhỏ hơn mỗi giá trị tiền tố. Sắp xếp lại hậu tố không thể làm giảm số lượng đó. Các phép đảo ngược có thể kiểm soát duy nhất nằm bên trong hậu tố và thứ tự tăng dần sẽ giảm thiểu chúng về 0.
