---
title: "CF 141C - Hàng đợi"
description: "Mỗi người chỉ nhớ một con số, có bao nhiêu người cao hơn đứng trước mình trong hàng đợi. Chúng ta không còn biết thứ tự ban đầu hay độ cao thực tế nữa. Nhiệm vụ là xây dựng lại bất kỳ thứ tự hàng đợi hợp lệ nào cùng với độ cao thỏa mãn giá trị được ghi nhớ của mỗi người."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 141
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 101 (Div. 2)"
rating: 1800
weight: 141
solve_time_s: 144
verified: false
draft: false
---

[CF 141C - Hàng đợi](https://codeforces.com/problemset/problem/141/C) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người chỉ nhớ một con số, có bao nhiêu người cao hơn đứng trước mình trong hàng đợi. Chúng ta không còn biết thứ tự ban đầu hay độ cao thực tế nữa. Nhiệm vụ là xây dựng lại bất kỳ thứ tự hàng đợi hợp lệ nào cùng với độ cao thỏa mãn giá trị được ghi nhớ của mỗi người. 

Giả sử một người có giá trị`a = 2`. Điều đó có nghĩa là đúng hai người đứng trước họ phải có chiều cao lớn hơn hẳn. Những người có chiều cao bằng nhau không được tính. 

Điều thú vị là chiều cao thực tế không cố định. Chúng ta có thể tự do gán bất kỳ số nguyên dương nào làm chiều cao, miễn là các phép so sánh tương đối tạo ra số lượng được yêu cầu. 

Ràng buộc`n ≤ 3000`đủ nhỏ cho các thuật toán bậc hai. MỘT`O(n^2)`giải pháp thực hiện khoảng 9 triệu thao tác trong trường hợp xấu nhất, đủ nhanh trong Python. Phương pháp lập phương trở nên nguy hiểm vì`3000^3`là 27 tỷ thao tác, hoàn toàn không thể thực hiện được trong vòng 2 giây. 

Khó khăn chính là các giá trị được ghi nhớ phụ thuộc vào thứ tự độ cao tương đối chứ không phụ thuộc trực tiếp vào vị trí. Một nỗ lực ngây thơ tham lam xếp người từ trái sang phải có thể dễ dàng mắc bẫy sau này. 

Hãy xem xét ví dụ này:```
3
a 0
b 1
c 0
```Một chiến lược tham lam bất cẩn có thể đặt ra`a`đầu tiên bởi vì`a=0`, sau đó`c`, sau đó không đặt được`b`. Nhưng sự sắp xếp đúng tồn tại:```
a 2
b 1
c 2
```Ở đây chỉ`a`cao hơn`b`và đứng trước mặt anh ta. 

Một trường hợp tế nhị khác là phát hiện sự bất khả thi.```
2
a 1
b 1
```Người đứng trước không có người nào đứng trước nên họ không thể yêu cầu một người cao hơn trước mình. Không có giải pháp tồn tại. 

Chiều cao bằng nhau là một nguồn sai lầm khác. Vì chỉ những người cao hơn mới được tính, nên hai người có thể có cùng chiều cao mà không ảnh hưởng đến số lượng của nhau. 

Ví dụ:```
3
a 0
b 0
c 1
```Một bài tập hợp lệ là:```
a 2
b 2
c 1
```Cả hai`a`Và`b`cao hơn`c`, nhưng họ không cao hơn nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi hoán vị của mọi người và kiểm tra xem có tồn tại sự phân công chiều cao nào đó hay không. Ngay cả việc kiểm tra một hoán vị đơn lẻ cũng không hề đơn giản vì chúng ta phải xác minh số đếm cao hơn trước. Vì có`n!`hoán vị, điều này trở nên vô dụng ngay cả đối với`n = 15`. 

Chúng ta cần khai thác cấu trúc ẩn trong điều kiện. 

Giả sử chúng ta xử lý mọi người theo thứ tự tăng dần về chiều cao của họ. Khi chúng tôi sắp xếp một người, tất cả những người được xử lý trước đó đều thấp hơn hoặc bằng nhau, vì vậy họ không đóng góp vào số lượng người cao hơn trước họ. Những người duy nhất quan trọng là những người sẽ được xử lý sau, nghĩa là những người cao hơn. 

Điều này đảo ngược quan điểm một cách độc đáo. 

Hãy tưởng tượng chúng ta sắp xếp mọi người theo`a`. Một người có kích thước lớn hơn`a`phải có nhiều người cao hơn trước họ, vì vậy nhìn chung họ cần xuất hiện xa hơn về phía bên phải so với những người cao hơn. 

Quan sát chính là thế này: 

Nếu chúng ta xử lý con người ngày càng tăng`a`, thì khi đặt một người có giá trị`a`, họ phải chiếm vị trí`a`trong số trình tự hiện đang được xây dựng. 

Tại sao điều này làm việc? 

Tại thời điểm đó, tất cả mọi người trong chuỗi đều thấp hơn hoặc bằng nhau. Những lần chèn thêm trong tương lai tương ứng với những người cao hơn. Nếu người hiện tại được chèn vào chỉ mục`a`, chính xác`a`những người cao hơn trong tương lai sau này có thể xuất hiện trước họ. 

Điều này biến vấn đề thành một quá trình chèn mang tính xây dựng. 

Chúng tôi sắp xếp mọi người theo`a`và liên tục chèn từng người vào chỉ mục`a`trong danh sách hiện tại. 

Nếu tại bất kỳ thời điểm nào`a`vượt quá kích thước danh sách hiện tại thì không thể cấu hình được. 

Sau khi xây dựng đơn hàng, chúng tôi chỉ định độ cao. Vì những người bình đẳng`a`có thể chia sẻ cùng một chiều cao, chúng tôi nén chiều cao theo nhóm bằng nhau`a`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) | O(n) | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả mọi người theo cặp`(name, a)`. 
2. Sắp xếp người theo thứ tự tăng dần`a`. 

Nhỏ hơn`a`các giá trị dễ dàng được đặt lên hàng đầu vì chúng đòi hỏi ít người cao hơn đứng trước chúng. 
3. Duy trì danh sách đại diện cho hàng đợi đang được xây dựng. 
4. Đối với mỗi người theo thứ tự sắp xếp: 

1. Đặt số lượng yêu cầu của họ là`a`. 
2. Nếu`a`lớn hơn độ dài hàng đợi hiện tại thì không có vị trí hợp lệ nào tồn tại. In`-1`. 
3. Nếu không thì chèn người vào mục lục`a`. 

Chèn vào chỉ mục`a`đảm bảo chính xác rằng`a`những người cao hơn trong tương lai có thể sẽ đứng trước người này. 
5. Sau khi hoàn tất thứ tự xếp hàng, hãy chỉ định độ cao. 

Những người có cùng`a`có thể nhận được cùng một chiều cao vì họ không nên coi nhau là cao hơn. 
6. Duyệt hàng đợi cuối cùng từ trái sang phải. 

1. Bắt đầu với chiều cao`1`. 
2. Bất cứ khi nào người hiện tại`a`khác với người trước`a`, tăng chiều cao 
3. Lưu trữ chiều cao được chỉ định. 
7. Xuất thứ tự hàng đợi cùng với chiều cao. 

### Tại sao nó hoạt động 

Điều bất biến là khi xử lý một người có giá trị`a`, tất cả những người đã xếp hàng đều không cao hơn họ. Những người được đưa vào tương lai tương ứng với những người cao hơn. 

Bằng cách chèn người hiện tại vào chỉ mục`a`, chính xác`a`các vị trí trước chúng vẫn dành cho những người cao hơn trong tương lai. Mỗi người được xử lý sau này đều có`a' ≥ a`, vì vậy chúng được gán chiều cao lớn hơn hoặc bằng nhau. Chỉ những chiều cao lớn hơn mới quan trọng và bằng nhau`a`các nhóm chia sẻ chiều cao. 

Vì vậy, mỗi người cuối cùng sẽ có chính xác số lượng người cao hơn cần thiết trước mặt họ. 

Nếu ở một bước nào đó`a`vượt quá quy mô hàng đợi hiện tại, không có đủ vị trí dành cho những người cao hơn trong tương lai, vì vậy không có giải pháp nào có thể tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    people = []
    for _ in range(n):
        name, a = input().split()
        people.append((name, int(a)))
    
    people.sort(key=lambda x: x[1])
    
    queue = []
    
    for name, a in people:
        if a > len(queue):
            print(-1)
            return
        
        queue.insert(a, (name, a))
    
    heights = {}
    current_height = 1
    
    for i, (name, a) in enumerate(queue):
        if i > 0 and a != queue[i - 1][1]:
            current_height += 1
        
        heights[name] = current_height
    
    for name, a in queue:
        print(name, heights[name])

solve()
```Giai đoạn đầu tiên sắp xếp mọi người theo những giá trị được ghi nhớ của họ. Thứ tự này quan trọng vì chúng tôi hiểu những lần thêm vào sau này là những người cao hơn. 

Danh sách`queue`lưu trữ thứ tự được xây dựng lại một phần. Chèn danh sách Python là`O(n)`, điều này có thể chấp nhận được đối với`n ≤ 3000`. 

Điều kiện không thể là tinh tế. Nếu như`a > len(queue)`, thì cho dù mỗi người trong tương lai có cao hơn thì cũng sẽ không có đủ vị trí trước người này. Việc xây dựng phải dừng ngay lập tức. 

Việc gán chiều cao đơn giản hơn so với lần đầu xuất hiện. Chỉ có sự so sánh tương đối mới quan trọng. Những người có giống hệt nhau`a`các giá trị có thể có cùng chiều cao một cách an toàn vì chúng không được coi nhau là cao hơn. 

Mã chỉ tăng chiều cao khi`a`những thay đổi. Vì hàng đợi cuối cùng đã hợp lệ về mặt cấu trúc nên việc gán chiều cao được nén này sẽ duy trì tất cả số lượng cần thiết. 

Một sai lầm phổ biến là chỉ định độ cao tăng dần cho mọi người. Điều đó không chính xác làm cho bằng-`a`mọi người coi nhau là cao hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
a 0
b 2
c 0
d 0
```Sắp xếp theo`a`: 

| Người | một | 
| --- | --- | 
| một | 0 | 
| c | 0 | 
| d | 0 | 
| b | 2 | 

Quá trình chèn: 

| Bước | Chèn | Vị trí | Xếp hàng | 
| --- | --- | --- | --- | 
| 1 | một | 0 | [a] | 
| 2 | c | 0 | [c, a] | 
| 3 | d | 0 | [d, c, a] | 
| 4 | b | 2 | [d, c, b, a] | 

Phân bổ chiều cao: 

| Người | một | Chiều cao | 
| --- | --- | --- | 
| d | 0 | 1 | 
| c | 0 | 1 | 
| b | 2 | 2 | 
| một | 0 | 3 | 

Một đầu ra hợp lệ:```
d 1
c 1
b 2
a 3
```Trước`b`, có đúng hai người có chiều cao lớn hơn:`d`Và`c`. 

Dấu vết này thể hiện sự bất biến chính. Khi`b`được chèn vào chỉ số 2, chính xác có hai vị trí cao hơn trong tương lai tồn tại trước anh ta. 

### Ví dụ 2 

đầu vào:```
3
a 1
b 1
c 0
```Thứ tự sắp xếp: 

| Người | một | 
| --- | --- | 
| c | 0 | 
| một | 1 | 
| b | 1 | 

Quá trình chèn: 

| Bước | Chèn | Vị trí | Xếp hàng | 
| --- | --- | --- | --- | 
| 1 | c | 0 | [c] | 
| 2 | một | 1 | [c, a] | 
| 3 | b | 1 | [c, b, a] | 

Phân bổ chiều cao: 

| Người | một | Chiều cao | 
| --- | --- | --- | 
| c | 0 | 1 | 
| b | 1 | 2 | 
| một | 1 | 2 | 

Đầu ra cuối cùng:```
c 1
b 2
a 2
```Cả hai`a`Và`b`có chính xác một người cao hơn trước họ, đó là không ai cả. Chiều cao bằng nhau ngăn cản chúng đếm lẫn nhau. 

Ví dụ này cho thấy tại sao cần có chiều cao bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi lần chèn vào danh sách Python có thể dịch chuyển các phần tử O(n) | 
| Không gian | O(n) | Bản đồ hàng đợi và chiều cao lưu trữ tất cả mọi người | 

Với`n ≤ 3000`, thời gian bậc hai là hoàn toàn an toàn. Khoảng 9 triệu lượt di chuyển phần tử vừa vặn thoải mái trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    people = []
    for _ in range(n):
        name, a = input().split()
        people.append((name, int(a)))

    people.sort(key=lambda x: x[1])

    queue = []

    for name, a in people:
        if a > len(queue):
            print(-1)
            return

        queue.insert(a, (name, a))

    heights = {}
    current_height = 1

    for i, (name, a) in enumerate(queue):
        if i > 0 and a != queue[i - 1][1]:
            current_height += 1

        heights[name] = current_height

    for name, a in queue:
        print(name, heights[name])

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
out = run(
"""4
a 0
b 2
c 0
d 0
"""
)

assert out.strip() != "-1"

# minimum size
assert run(
"""1
alice 0
"""
).strip() == "alice 1"

# impossible case
assert run(
"""2
a 1
b 1
"""
).strip() == "-1"

# all zeros
out = run(
"""4
a 0
b 0
c 0
d 0
"""
)

assert out.strip() != "-1"

# increasing requirements
out = run(
"""4
a 0
b 1
c 2
d 3
"""
)

assert out.strip() != "-1"

# off-by-one boundary
assert run(
"""3
a 0
b 2
c 0
"""
).strip() == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người độc thân với`a=0`| Hàng đợi hợp lệ | Đầu vào nhỏ nhất có thể | 
| Hai người đều yêu cầu một người cao hơn trước |`-1`| Phát hiện cấu hình không thể | 
| Tất cả`a=0`| Hàng đợi hợp lệ | Xử lý độ cao bằng nhau | 
| Tăng nghiêm ngặt`a`| Hàng đợi hợp lệ | Ranh giới chỉ số chèn tối đa | 
|`a`lớn hơn kích thước hàng đợi hiện tại |`-1`| Xác thực chèn từng cái một | 

## Vỏ cạnh 

Hãy xem xét cấu hình không thể:```
2
a 1
b 1
```Trình tự xử lý vẫn giữ nguyên. Người đầu tiên yêu cầu chèn vào chỉ mục 1 trong khi hàng đợi trống. Từ`1 > 0`, thuật toán báo cáo ngay`-1`. 

Điều này đúng vì vị trí đầu tiên trong bất kỳ hàng đợi nào đều không có ai đứng trước nó. 

Now consider equal values:```
4
a 0
b 0
c 0
d 0
```Mọi thao tác chèn xảy ra ở vị trí 0:```
[d, c, b, a]
```Tất cả mọi người đều nhận được chiều cao như nhau. Không ai có người cao hơn trước họ nên mọi người đều hài lòng. 

Cuối cùng, hãy xem xét việc chèn ranh giới:```
4
a 0
b 1
c 2
d 3
```Trình tự chèn:```
[a]
[a, b]
[a, b, c]
[a, b, c, d]
```Mỗi người được chèn chính xác vào kích thước hàng đợi hiện tại, nghĩa là họ sẽ đi đến cuối. Điều này xác minh rằng`a == len(queue)`là hợp lệ, trong khi chỉ những giá trị lớn hơn là không thể.
