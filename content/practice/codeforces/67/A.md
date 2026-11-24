---
title: "CF 67A - Giáo viên bán phần"
description: "Chúng ta có một hàng học sinh, và với mỗi cặp liền kề, chúng ta chỉ biết thứ tự tương đối về điểm của họ. Nếu ký tự quan hệ giữa vị trí i và i + 1 là: - L thì sinh viên i phải nhận nhiều cà phê hơn sinh viên i + 1 - R thì sinh viên i + 1 phải nhận…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 67
codeforces_index: "A"
codeforces_contest_name: "Manthan 2011"
rating: 1800
weight: 67
solve_time_s: 126
verified: true
draft: false
---

[CF 67A - Giáo viên bán phần](https://codeforces.com/problemset/problem/67/A) 

**Đánh giá:** 1800 
**Tags:** dp, đồ thị, tham lam, thực hiện 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng học sinh, và với mỗi cặp liền kề, chúng ta chỉ biết thứ tự tương đối về điểm của họ. Nếu mối quan hệ giữa các vị trí`i`Và`i + 1`là:

-`L`, sau đó là sinh viên`i`phải nhận được nhiều cà phê hơn sinh viên`i + 1`-`R`, sau đó là sinh viên`i + 1`phải nhận được nhiều cà phê hơn sinh viên`i`-`=`, thì cả hai học sinh phải nhận được số lượng kẹo như nhau 

Mỗi học sinh phải nhận được ít nhất một quả kẹo bơ cứng, và trong số tất cả các bài tập thỏa mãn các quan hệ, chúng ta muốn bài tập có tổng số tiền tối thiểu. 

Đầu vào không phải là điểm thực tế. Thay vào đó, nó trực tiếp đưa ra những ràng buộc giữa các sinh viên lân cận. Nếu chuỗi quan hệ có độ dài`n - 1`, thì ký tự`s[i]`mô tả mối quan hệ giữa các vị trí`i`Và`i + 1`. 

Ràng buộc`n ≤ 1000`là khá nhỏ. Thậm chí một`O(n^2)`giải pháp sẽ trôi qua một cách thoải mái vì trường hợp xấu nhất là khoảng một triệu thao tác. Điều đó có nghĩa là chúng ta không cần các cấu trúc dữ liệu hoặc thuật toán đồ thị phức tạp với mức độ tối ưu hóa cao. Thách thức là lý luận về cách thức mà sự bất bình đẳng cục bộ tương tác trên toàn cầu. 

Phần khó khăn là việc thay đổi một vị trí có thể buộc các vị trí lân cận phải cập nhật liên tục. Một sự lựa chọn tham lam ngây thơ được thực hiện quá sớm có thể phá vỡ những ràng buộc sau này. 

Hãy xem xét ví dụ này:```
4
RRR
```Các hạn chế là:```
a1 < a2 < a3 < a4
```Phép gán hợp lệ tối thiểu là:```
1 2 3 4
```Một kẻ tham lam từ trái sang phải bất cẩn luôn đưa ra giá trị hợp lệ nhỏ nhất hiện tại có thể gán:```
1 2 1 2
```Mối quan hệ thứ ba thất bại vì`a2 < a3`không còn đúng nữa. 

Một trường hợp nguy hiểm khác là chuỗi giảm dài:```
5
LLLL
```Các ràng buộc trở thành:```
a1 > a2 > a3 > a4 > a5
```Phép gán tối thiểu đúng là:```
5 4 3 2 1
```Nếu chúng ta khởi tạo mọi thứ thành`1`và chỉ khắc phục các vi phạm cục bộ một lần, có thể chúng ta sẽ dừng quá sớm trước khi ảnh hưởng lan rộng đến tận bên trái. 

Sự bình đẳng cũng quan trọng vì chúng kết nối các vị trí thành các nhóm phải chia sẻ cùng một giá trị. 

Ví dụ:```
5
R==L
```Các mối quan hệ là:```
a1 < a2 = a3 = a4 > a5
```Nhiệm vụ tối thiểu là:```
1 2 2 2 1
```Nếu sự bình đẳng được coi là “không hạn chế” thay vì “cùng một giá trị”, chúng ta có thể tạo ra sai:```
1 2 1 2 1
```vi phạm`a2 = a3`. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là bắt đầu mỗi học sinh bằng một viên kẹo bơ cứng và liên tục xem xét các mối quan hệ. Bất cứ khi nào một ràng buộc bị vi phạm, hãy tăng một bên cho đến khi mối quan hệ trở nên hợp lệ. 

Ví dụ, nếu chúng ta thấy`R`ở vị trí`i`và hiện tại`a[i] >= a[i+1]`, chúng tôi tăng`a[i+1]`ĐẾN`a[i] + 1`. Tương tự, đối với`L`, chúng ta tăng vế trái khi cần thiết. Vì`=`, chúng tôi đồng bộ hóa cả hai giá trị. 

Quá trình này cuối cùng sẽ hội tụ vì các giá trị chỉ tăng lên và mỗi bản cập nhật đều khắc phục ít nhất một vi phạm. Nhiệm vụ cuối cùng là tối thiểu vì chúng tôi chỉ tăng giá trị khi bị ép buộc. 

Vấn đề là các bản cập nhật có thể được lan truyền nhiều lần trên mảng. Trong trường hợp xấu nhất, mỗi lần quét chỉ sửa được một vị trí, dẫn đến gần như`O(n^2)`hoạt động. 

Nhận xét quan trọng là mọi mối quan hệ chỉ phụ thuộc vào các vị trí lân cận. Yêu cầu ngày càng tăng theo một hướng. 

Đối với một chuỗi như:```
RRRR
```mỗi vị trí phụ thuộc vào hàng xóm bên trái. Các giá trị tối thiểu có thể được xây dựng từ trái sang phải. 

Vì:```
LLLL
```sự phụ thuộc chảy từ phải sang trái. 

Điều này gợi ý việc chia vấn đề thành hai hướng đi. 

Chúng tôi duy trì một mảng được khởi tạo bằng các mảng. 

Trong quá trình chuyển từ trái sang phải, chúng tôi xử lý tất cả`R`quan hệ. Nếu vị trí`i + 1`phải lớn hơn vị trí`i`, thì lựa chọn hợp lệ nhỏ nhất là:```
a[i+1] = a[i] + 1
```Trong quá trình chuyển từ phải sang trái, chúng tôi xử lý tất cả`L`quan hệ. Nếu vị trí`i`phải vượt quá vị trí`i + 1`, thì lựa chọn hợp lệ nhỏ nhất là:```
a[i] = max(a[i], a[i+1] + 1)
```Sự bình đẳng có thể được xử lý bằng cách sao chép các giá trị một cách thích hợp trong cả hai lần chuyển. 

Cái nhìn sâu sắc quan trọng là mọi ràng buộc chỉ đẩy các giá trị lên đến mức cần thiết tối thiểu. Khi cả hai sự phụ thuộc định hướng được xử lý, tất cả các bất đẳng thức đều được giữ nguyên đồng thời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`a`chiều dài`n`và khởi tạo mọi giá trị thành`1`. 

Mỗi học sinh phải nhận được ít nhất một quả kẹo bơ cứng, vì vậy`1`là điểm bắt đầu nhỏ nhất có thể. 
2. Di chuyển từ trái sang phải. 

Với mọi quan hệ ở vị trí`i`:

- Nếu như`s[i] == 'R'`, sau đó là sinh viên`i + 1`phải nhận nhiều hơn sinh viên`i`. 
- Bộ:```
a[i+1] = a[i] + 1
```- Nếu như`s[i] == '='`, thì cả hai học sinh phải có cùng giá trị. 
- Bộ:```
a[i+1] = a[i]
```Thẻ này giải quyết tất cả các ràng buộc mà phần phụ thuộc của nó di chuyển từ trái sang phải. 
3. Di chuyển từ phải sang trái. 

Với mọi quan hệ ở vị trí`i`:

- Nếu như`s[i] == 'L'`, sau đó là sinh viên`i`phải nhận nhiều hơn sinh viên`i + 1`. 
- Bộ:```
a[i] = max(a[i], a[i+1] + 1)
```- Nếu như`s[i] == '='`, thì cả hai học sinh phải khớp lại. 
- Bộ:```
a[i] = max(a[i], a[i+1])
a[i+1] = a[i]
```các`max`là cần thiết vì việc chuyển từ trái sang phải có thể đã được gán giá trị lớn hơn do các ràng buộc trước đó. 
4. In mảng kết quả. 

### Tại sao nó hoạt động 

Chuyển từ trái sang phải tính toán các giá trị tối thiểu cần thiết để đáp ứng mọi sự phụ thuộc ngày càng tăng của biểu mẫu`a[i] < a[i+1]`. Chuyển từ phải sang trái tính toán các giá trị tối thiểu cần thiết cho mỗi lần phụ thuộc giảm dần của biểu mẫu`a[i] > a[i+1]`. 

Không có bước nào làm tăng giá trị một cách không cần thiết. Mỗi bản cập nhật đặt một vị trí thành số nhỏ nhất có khả năng đáp ứng ràng buộc cục bộ trong khi vẫn duy trì các yêu cầu đã thiết lập trước đó. 

Sau cả hai lần duyệt, mọi quan hệ liền kề đều được thỏa mãn. Vì tất cả các giá trị bắt đầu ở giá trị tối thiểu có thể`1`và chỉ tăng lên khi bị ép buộc bởi một ràng buộc, tổng số tiền là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    a = [1] * n

    for i in range(n - 1):
        if s[i] == 'R':
            a[i + 1] = a[i] + 1
        elif s[i] == '=':
            a[i + 1] = a[i]

    for i in range(n - 2, -1, -1):
        if s[i] == 'L':
            a[i] = max(a[i], a[i + 1] + 1)
        elif s[i] == '=':
            val = max(a[i], a[i + 1])
            a[i] = val
            a[i + 1] = val

    print(*a)

solve()
```Giải pháp bắt đầu bằng việc giao cho mỗi học sinh một viên kẹo bơ cứng. Điều này đảm bảo trạng thái cơ sở pháp lý tối thiểu. 

Bước đầu tiên xử lý các phần phụ thuộc ở bên phải. Khi chúng tôi nhìn thấy`R`, học sinh bên phải phải lớn hơn hẳn, vì vậy chúng ta chỉ định nhiều hơn bên trái một đơn vị. Khi chúng tôi nhìn thấy`=`, chúng ta sao chép cùng một giá trị vì đẳng thức phải giữ chính xác. 

Đường chuyền thứ hai xử lý các phụ thuộc sang trái. Vì`L`, học sinh bên trái phải nhiều hơn học sinh bên phải ít nhất một. Chúng tôi sử dụng`max`bởi vì phía bên trái có thể đã đủ lớn so với các ràng buộc trước đó. 

Việc xử lý bình đẳng trong quá trình truyền ngược rất tinh tế. Giả sử một bên trở nên lớn hơn do có một dây xích khác. Bình đẳng có nghĩa là cả hai bên phải chia sẻ giá trị lớn hơn. Nếu chúng ta chỉ cập nhật một hướng, sự bình đẳng có thể bị phá vỡ. 

Vòng lặp:```
for i in range(n - 2, -1, -1):
```cũng dễ mắc sai lầm. Chúng tôi bắt đầu từ`n - 2`bởi vì mối quan hệ`s[i]`kết nối các vị trí`i`Và`i + 1`. 

Không tồn tại mối lo ngại về tràn số nguyên vì`n ≤ 1000`, do đó giá trị lớn nhất có thể có nhiều nhất là`1000`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
LRLR
```Quan hệ:```
a1 > a2 < a3 > a4 < a5
```Trạng thái ban đầu: 

| Bước | Mảng | 
| --- | --- | 
| Bắt đầu | 1 1 1 1 1 | 

Đường chuyền từ trái sang phải: 

| tôi | Quan hệ | Hành động | Mảng | 
| --- | --- | --- | --- | 
| 0 | L | không có gì | 1 1 1 1 1 | 
| 1 | R | a[2] = a[1] + 1 | 1 1 2 1 1 | 
| 2 | L | không có gì | 1 1 2 1 1 | 
| 3 | R | a[4] = a[3] + 1 | 1 1 2 1 2 | 

Đường chuyền từ phải sang trái: 

| tôi | Quan hệ | Hành động | Mảng | 
| --- | --- | --- | --- | 
| 3 | R | không có gì | 1 1 2 1 2 | 
| 2 | L | a[2] = max(2, 2) | 1 1 2 1 2 | 
| 1 | R | không có gì | 1 1 2 1 2 | 
| 0 | L | a[0] = max(1, 2) | 2 1 2 1 2 | 

Đầu ra cuối cùng:```
2 1 2 1 2
```Ví dụ này cho thấy hai đường chuyền bổ sung cho nhau như thế nào. Việc di chuyển từ trái sang phải xử lý các chuỗi tăng dần, trong khi việc di chuyển ngược lại sửa chữa các chuỗi giảm dần. 

### Ví dụ 2 

đầu vào:```
5
R==L
```Quan hệ:```
a1 < a2 = a3 = a4 > a5
```Trạng thái ban đầu: 

| Bước | Mảng | 
| --- | --- | 
| Bắt đầu | 1 1 1 1 1 | 

Đường chuyền từ trái sang phải: 

| tôi | Quan hệ | Hành động | Mảng | 
| --- | --- | --- | --- | 
| 0 | R | a[1] = 2 | 1 2 1 1 1 | 
| 1 | = | a[2] = a[1] | 1 2 2 1 1 | 
| 2 | = | a[3] = a[2] | 1 2 2 2 1 | 
| 3 | L | không có gì | 1 2 2 2 1 | 

Đường chuyền từ phải sang trái: 

| tôi | Quan hệ | Hành động | Mảng | 
| --- | --- | --- | --- | 
| 3 | L | a[3] = max(2, 2) | 1 2 2 2 1 | 
| 2 | = | đồng bộ hóa | 1 2 2 2 1 | 
| 1 | = | đồng bộ hóa | 1 2 2 2 1 | 
| 0 | R | không có gì | 1 2 2 2 1 | 

Đầu ra cuối cùng:```
1 2 2 2 1
```Dấu vết này chứng tỏ rằng các mối quan hệ bình đẳng tạo thành các khối kết nối có giá trị phải được đồng bộ hóa ngay cả sau khi cập nhật sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai đường tuyến tính trên mảng | 
| Không gian | O(n) | Mảng câu trả lời lưu trữ một giá trị cho mỗi học sinh | 

Với`n ≤ 1000`, giải pháp này dễ dàng nằm trong giới hạn. Thậm chí một`O(n²)`phương pháp sẽ thành công, nhưng giải pháp tuyến tính sạch hơn và có quy mô tự nhiên cho các đầu vào lớn hơn nhiều. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    a = [1] * n

    for i in range(n - 1):
        if s[i] == 'R':
            a[i + 1] = a[i] + 1
        elif s[i] == '=':
            a[i + 1] = a[i]

    for i in range(n - 2, -1, -1):
        if s[i] == 'L':
            a[i] = max(a[i], a[i + 1] + 1)
        elif s[i] == '=':
            val = max(a[i], a[i + 1])
            a[i] = val
            a[i + 1] = val

    print(*a)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("5\nLRLR\n") == "2 1 2 1 2", "sample 1"

# minimum size
assert run("2\nR\n") == "1 2", "minimum increasing case"

# all equal
assert run("5\n====\n") == "1 1 1 1 1", "all equal values"

# strictly decreasing
assert run("5\nLLLL\n") == "5 4 3 2 1", "long decreasing chain"

# mixed constraints
assert run("5\nR==L\n") == "1 2 2 2 1", "equality block with boundaries"

# alternating pattern
assert run("6\nLRLRL\n") == "2 1 2 1 2 1", "alternating inequalities"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / R`|`1 2`| Đầu vào hợp lệ nhỏ nhất | 
|`5 / ==== `|`1 1 1 1 1`| Tuyên truyền bình đẳng | 
|`5 / LLLL`|`5 4 3 2 1`| Chuỗi phụ thuộc ngược dài | 
|`5 / R==L`|`1 2 2 2 1`| Bình đẳng tương tác với bất bình đẳng | 
|`6 / LRLRL`|`2 1 2 1 2 1`| Luân phiên cực tiểu và cực đại cục bộ | 

## Vỏ cạnh 

Chuỗi giảm dài là cách dễ nhất để phá vỡ giải pháp một chiều. 

đầu vào:```
5
LLLL
```Sau khi vượt qua từ trái sang phải, mảng vẫn là:```
1 1 1 1 1
```Quá trình vượt qua ngược xử lý các ràng buộc từ cuối:```
a[3] = 2
a[2] = 3
a[1] = 4
a[0] = 5
```Kết quả cuối cùng:```
5 4 3 2 1
```Điều này cho thấy tại sao việc truyền từ phải sang trái là cần thiết. Mỗi bản cập nhật phụ thuộc vào giá trị đã đúng ở bên phải của nó. 

Chuỗi bình đẳng yêu cầu đồng bộ hóa ở cả hai lượt. 

đầu vào:```
4
===
```Lần đầu tiên sao chép các giá trị trên toàn bộ chuỗi:```
1 1 1 1
```Đường chuyền thứ hai giữ cho chúng được đồng bộ hóa. Vì không có bất đẳng thức nào buộc phải tăng lên nên phép gán hợp lệ tối thiểu vẫn là tất cả. 

Các mối quan hệ bình đẳng và bất bình đẳng hỗn hợp có thể tạo ra những hiệu ứng lan truyền tinh vi. 

đầu vào:```
5
R==L
```Đường chuyền chuyển tiếp xây dựng:```
1 2 2 2 1
```Đường chuyền ngược kiểm tra trận chung kết`L`quan hệ và xác nhận rằng`2 > 1`đã nắm giữ rồi. Đồng bộ hóa bình đẳng bảo tồn khối giữa. 

Việc triển khai có lỗi chỉ sao chép các đẳng thức theo một hướng có thể vô tình tạo ra:```
1 2 2 1 1
```vi phạm`a3 = a4`. 

Các ràng buộc xen kẽ kiểm tra xem thuật toán có tránh được sự gia tăng không cần thiết hay không. 

đầu vào:```
6
LRLRL
```Phép gán hợp lệ tối thiểu là:```
2 1 2 1 2 1
```Thuật toán không bao giờ tăng giá trị vượt quá những gì mỗi mối quan hệ cục bộ yêu cầu. Thuộc tính tối thiểu đó chính xác là lý do tại sao bắt đầu từ số một và chỉ tăng khi bị buộc hoạt động chính xác.
