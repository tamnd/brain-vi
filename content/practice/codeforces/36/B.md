---
title: "CF 36B - Fractal"
description: "Chúng ta bắt đầu với mẫu n × n bao gồm các ô đen (*) và các ô trắng (.). Mẫu này hoạt động giống như một mẫu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 36
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 36"
rating: 1600
weight: 36
solve_time_s: 112
verified: true
draft: false
---
[CF 36B - Fractal](https://codeforces.com/problemset/problem/36/B) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một`n × n`mô hình bao gồm các ô màu đen (`*`) và tế bào trắng (`.`). Mẫu này hoạt động giống như một mẫu. 

Fractal được xây dựng đệ quy. Ở cấp độ đầu tiên, toàn bộ canvas được chia thành`n × n`các khối và mọi khối tương ứng với một ô màu đen trong mẫu sẽ trở thành màu đen hoàn toàn. Hiện tại, các ô mẫu màu trắng vẫn chưa được quyết định. 

Ở cấp độ tiếp theo, mỗi khối vẫn trắng lại được chia thành`n × n`các khối nhỏ hơn và cùng một mẫu được áp dụng bên trong nó. Các tế bào đen ngừng mở rộng mãi mãi, trong khi các tế bào trắng tiếp tục đệ quy. 

Sau khi thực hiện chính xác quá trình này`k`nhiều lần, chúng ta phải in bức ảnh cuối cùng. Kích thước của nó là`n^k × n^k`. 

Các giới hạn rất nhỏ về mặt số lượng, nhưng bản thân đầu ra cuối cùng có thể trở nên lớn. Từ`n ≤ 3`Và`k ≤ 5`, lưới lớn nhất có thể có kích thước`3^5 = 243`, do đó đầu ra chứa tối đa khoảng sáu mươi nghìn ô. Điều đó có nghĩa là ngay cả các thuật toán chạm vào từng ô cuối cùng nhiều lần cũng hoàn toàn an toàn. 

Thách thức thực sự là hiểu cách thức đệ quy xác định liệu một ô cụ thể có màu đen hay trắng. 

Một sai lầm phổ biến là nghĩ rằng mọi cấp độ đều ghi đè lên các màu trước đó. Đó không phải là cách quá trình hoạt động. Khi một vùng trở thành màu đen, quá trình đệ quy sẽ dừng ở đó vĩnh viễn. 

Hãy xem xét đầu vào này:```
2 2
*.
..
```Ở cấp độ đầu tiên, góc phần tư trên cùng bên trái trở thành màu đen hoàn toàn. Không có gì bên trong nó được chia nhỏ lại. Việc triển khai bất cẩn có thể tiếp tục lặp lại trong góc phần tư đó và tạo không chính xác các ô màu trắng bên trong một vùng đáng lẽ phải có màu đen hoàn toàn. 

Một trường hợp tinh tế khác xuất hiện khi mẫu chỉ chứa một ô màu trắng.```
2 3
**
*.
```Chỉ có nhánh dưới cùng bên phải tiếp tục chia nhỏ. Mọi khu vực khác sẽ trở thành màu đen ngay lập tức. Nếu việc triển khai mở rộng tất cả các ô một cách mù quáng theo từng bước thì việc đó sẽ lãng phí công việc và có thể vô tình ghi đè lên các vùng màu đen cố định. 

Trường hợp nhỏ nhất có thể cũng đáng được kiểm tra:```
2 1
..
.*
```Ở đây câu trả lời chính xác là mẫu ban đầu. Không có sự mở rộng đệ quy nào xảy ra vì chúng ta chỉ thực hiện một cấp độ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp nhất phản ánh tuyên bố theo nghĩa đen. 

Chúng ta bắt đầu với một hình vuông màu trắng. Ở mỗi bước, mọi hình vuông màu trắng hiện tại sẽ được thay thế bằng một`n × n`bản sao của mẫu. Hình vuông màu đen vẫn có màu đen đặc mãi mãi. 

Một cách để thực hiện điều này là phân chia đệ quy. Mọi vùng đều trở nên đen hoàn toàn ngay lập tức hoặc bị chia nhỏ lại. Vì hình ảnh cuối cùng chứa nhiều nhất`243 × 243`cell, cách tiếp cận này đã đủ nhanh rồi. 

Vấn đề là việc quản lý rõ ràng các vùng đệ quy trở nên khó khăn. Chúng ta cần tọa độ, kích thước, điều kiện dừng và xử lý cẩn thận các vùng vốn đã có màu đen. 

Quan sát quan trọng là mọi ô cuối cùng đều có thể được phân tích độc lập. 

Lấy một tế bào`(x, y)`trong lưới cuối cùng. Ở mọi cấp độ đệ quy, mẫu sẽ quyết định xem vị trí đó rơi vào ô con màu đen hay ô con màu trắng. Tọa độ ở mỗi cấp chỉ đơn giản là các chữ số của`x`Và`y`trong căn cứ`n`. 

Giả sử ở một mức độ nào đó chúng ta kiểm tra:```
template[x % n][y % n]
```Nếu vị trí này có màu đen thì toàn bộ vùng tổ tiên trở thành màu đen ở độ sâu đệ quy đó, do đó ô cuối cùng cũng phải có màu đen. Chúng ta có thể dừng lại ngay lập tức. 

Nếu nó màu trắng, đệ quy tiếp tục đi lên:```
x //= n
y //= n
```Chúng tôi lặp lại điều này cho tất cả`k`cấp độ. Nếu không có cấp nào rơi vào ô mẫu màu đen thì ô cuối cùng vẫn có màu trắng. 

Điều này chuyển đổi vấn đề từ việc xây dựng vùng đệ quy thành vấn đề kiểm tra chữ số trên mỗi ô đơn giản. 

Cấu trúc đệ quy brute-force hoạt động hiệu quả vì kích thước đầu ra nhỏ, nhưng việc diễn giải dựa trên tọa độ sẽ rõ ràng hơn và dễ suy luận hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng đệ quy Brute Force | O(n^(2k) * k) | O(n^(2k)) | Đã chấp nhận | 
| Kiểm tra tọa độ tối ưu | O(n^(2k) * k) | O(1) thêm | Đã chấp nhận | 

Độ phức tạp tiệm cận trông giống nhau vì bản thân đầu ra đã có`n^(2k)`tế bào. Sự cải tiến đến từ sự đơn giản và hệ số hằng số thấp hơn. 

## Hướng dẫn thuật toán 

1. Đọc lưới mẫu và tính kích thước cuối cùng`size = n^k`. 
2. Lặp lại từng ô cuối cùng`(i, j)`trong`size × size`lưới đầu ra. 
3. Đối với ô hiện tại, sao chép tọa độ của nó vào các biến tạm thời:```
x = i
y = j
```4. Lặp lại chính xác`k`lần. 

Ở mỗi bước, hãy kiểm tra:```
template[x % n][y % n]
```Những phần còn lại này cho chúng ta biết ô mẫu nào kiểm soát mức đệ quy hiện tại. 
5. Nếu ô mẫu đó có màu đen (`*`), đánh dấu ô cuối cùng màu đen ngay lập tức và ngừng kiểm tra các mức sâu hơn. 

Điều này có hiệu quả vì các vùng màu đen không bao giờ được chia nhỏ hơn nữa. 
6. Ngược lại chia tọa độ:```
x //= n
y //= n
```Điều này chuyển sang cấp độ đệ quy gốc. 
7. Nếu tất cả`k`các mức vẫn trắng, ô cuối cùng có màu trắng. 
8. In các hàng đã tạo. 

### Tại sao nó hoạt động 

Mỗi tọa độ trong hình ảnh cuối cùng tương ứng với một đường đi qua cây phân chia đệ quy. 

Cơ sở ít quan trọng nhất-`n`các chữ số xác định cấp độ phân chia sâu nhất, các chữ số tiếp theo xác định cấp độ chính, v.v. 

Một ô sẽ trở thành màu đen nếu bất kỳ cấp độ nào của đường dẫn này nằm ở vị trí mẫu màu đen. Khi điều đó xảy ra, quá trình đệ quy sẽ dừng lại và toàn bộ vùng con cháu sẽ mãi mãi có màu đen. 

Nếu mọi cấp độ đều nằm trên một ô mẫu màu trắng thì đệ quy vẫn tồn tại`k`bước và ô vẫn trắng. 

Thuật toán kiểm tra chính xác các điều kiện này để mỗi ô đầu ra đều nhận được màu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
pattern = [input().strip() for _ in range(n)]

size = n ** k
answer = []

for i in range(size):
    row = []

    for j in range(size):
        x = i
        y = j
        black = False

        for _ in range(k):
            if pattern[x % n][y % n] == '*':
                black = True
                break

            x //= n
            y //= n

        row.append('*' if black else '.')

    answer.append(''.join(row))

sys.stdout.write('\n'.join(answer))
```Hai vòng lặp bên ngoài tạo ra mọi vị trí trong hình ảnh cuối cùng. 

Đối với mỗi ô, các biến`x`Và`y`được giảm đi nhiều lần bằng cách sử dụng phép chia số nguyên. Điều này có hiệu quả đi lên thông qua các cấp độ đệ quy. 

Biểu thức:```
pattern[x % n][y % n]
```trích xuất cơ sở hiện tại-`n`cặp chữ số. Những chữ số đó xác định khối mẫu nào kiểm soát cấp độ hiện tại. 

Sớm`break`là quan trọng. Khi một ô mẫu màu đen xuất hiện, các cấp độ sâu hơn không còn quan trọng nữa vì quá trình đệ quy sẽ dừng vĩnh viễn ở đó. 

Một chi tiết triển khai tinh tế là thứ tự thực hiện. Ta phải kiểm tra số dư hiện tại trước khi chia cho`n`. Đảo ngược thứ tự sẽ bỏ qua mức đệ quy sâu nhất và tạo ra các mẫu bị dịch chuyển. 

Một chi tiết khác là lặp lại chính xác`k`lần. Kể cả nếu`x`Và`y`trở về 0 sớm, mức đệ quy cao hơn vẫn tương ứng với vị trí mẫu`(0, 0)`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
.*
..
```Mẫu cho biết chỉ khối trên cùng bên phải mới có màu đen. 

Kích thước cuối cùng:```
2^3 = 8
```Hãy xem xét một số ô: 

| Tế bào`(i,j)`| Cấp 1`(x%n,y%n)`| Cấp 2 | Cấp 3 | Kết quả | 
| --- | --- | --- | --- | --- | 
|`(0,0)`|`(0,0)=.`|`(0,0)=.`|`(0,0)=.`|`.`| 
|`(0,1)`|`(0,1)=*`| dừng lại | dừng lại |`*`| 
|`(2,2)`|`(0,0)=.`|`(1,1)=.`|`(0,0)=.`|`.`| 
|`(3,4)`|`(1,0)=.`|`(1,0)=.`|`(0,1)=*`|`*`| 

Điều này chứng tỏ rằng một quyết định đen có thể xảy ra ở bất kỳ độ sâu đệ quy nào. Thuật toán tiếp tục kiểm tra các cấp độ cho đến khi màu đen xuất hiện hoặc tất cả các cấp độ vẫn giữ nguyên màu trắng. 

### Ví dụ 2 

đầu vào:```
2 2
*.
..
```Kích thước cuối cùng:```
4
```Theo dõi một số ô: 

| Tế bào`(i,j)`| Cấp 1 | Cấp 2 | Kết quả | 
| --- | --- | --- | --- | 
|`(0,0)`|`(0,0)=*`| dừng lại |`*`| 
|`(1,1)`|`(1,1)=.`|`(0,0)=*`|`*`| 
|`(2,2)`|`(0,0)=*`| dừng lại |`*`| 
|`(3,3)`|`(1,1)=.`|`(1,1)=.`|`.`| 

Quan sát quan trọng ở đây là các vùng màu đen dừng đệ quy ngay lập tức. Mặc dù`(0,0)`sẽ ánh xạ tới các cấp độ sâu hơn, cấp độ đầu tiên đã sửa nó thành màu đen mãi mãi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^(2k) * k) | Chúng tôi kiểm tra mọi ô đầu ra và kiểm tra nhiều nhất`k`mức đệ quy | 
| Không gian | O(n^(2k)) | Bản thân lưới đầu ra chi phối việc sử dụng bộ nhớ | 

Lưới lớn nhất chứa`243 × 243 = 59049`tế bào. Đối với mỗi ô, chúng tôi thực hiện tối đa năm lần kiểm tra. Điều này là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())
    pattern = [input().strip() for _ in range(n)]

    size = n ** k
    ans = []

    for i in range(size):
        row = []

        for j in range(size):
            x = i
            y = j
            black = False

            for _ in range(k):
                if pattern[x % n][y % n] == '*':
                    black = True
                    break

                x //= n
                y //= n

            row.append('*' if black else '.')

        ans.append(''.join(row))

    return '\n'.join(ans)

# provided sample
assert run(
"""2 3
.*
..
"""
) == (
""".*******
..******
.*.*****
....****
.***.***
..**..**
.*.*.*.*
........"""
), "sample 1"

# minimum size with k = 1
assert run(
"""2 1
..
.*
"""
) == (
"""..
.*"""
), "k = 1 should reproduce template"

# only one recursive white branch
assert run(
"""2 2
**
*.
"""
) == (
"""****
****
****
***."""
), "single surviving white path"

# all white template
assert run(
"""2 3
..
..
"""
) == (
"""........
........
........
........
........
........
........
........"""
), "everything stays white"

# off-by-one coordinate checks
assert run(
"""2 2
.*
..
"""
) == (
""".***
..**
.*.*
...."""
), "base-n digit traversal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`k = 1`trường hợp | Mẫu gốc | Không đệ quy không cần thiết | 
| Nhánh đơn màu trắng | Chủ yếu là lưới đen | Vùng đen dừng đệ quy | 
| Mẫu toàn màu trắng | Đầu ra hoàn toàn trắng | Không có sự lan truyền màu đen ngẫu nhiên | 
| Trường hợp 2 cấp độ hỗn hợp | Fractal có cấu trúc | Căn cứ chính xác-`n`phối hợp xử lý | 

## Vỏ cạnh 

Hãy xem xét trường hợp đệ quy chỉ tồn tại qua một đường dẫn:```
2 3
**
*.
```Ở mọi cấp độ, ba góc phần tư ngay lập tức trở thành màu đen vĩnh viễn. Chỉ có góc phần tư phía dưới bên phải tiếp tục chia nhỏ. 

Lấy di động`(7,7)`. 

Ở mỗi cấp độ:```
(7 % 2, 7 % 2) = (1,1) -> white
(3 % 2, 3 % 2) = (1,1) -> white
(1 % 2, 1 % 2) = (1,1) -> white
```Không có mức độ màu đen nào xuất hiện nên ô vẫn có màu trắng. 

Bây giờ lấy`(6,7)`:```
(0,1) -> black
```Thuật toán dừng ngay lập tức và đánh dấu ô màu đen. 

Điều này khẳng định quy luật rằng một tổ tiên da đen duy nhất sẽ thống trị mọi cấp độ sâu hơn. 

Bây giờ hãy xem xét độ sâu đệ quy nhỏ nhất:```
2 1
..
.*
```Thuật toán kiểm tra chính xác một cấp độ cho mỗi ô. 

Vì`(1,1)`:```
(1 % 2, 1 % 2) = (1,1) -> black
```Vì`(0,1)`:```
(0,1) -> white
```Đầu ra khớp chính xác với mẫu, xác nhận rằng quá trình triển khai xử lý`k = 1`một cách chính xác mà không cần chia nhỏ thêm. 

Cuối cùng, hãy xem xét một mẫu toàn màu trắng:```
2 2
..
..
```Mọi kiểm tra cấp độ luôn nằm trên một ô mẫu màu trắng. Vì không có mức đệ quy nào tạo ra màu đen nên toàn bộ hình ảnh cuối cùng vẫn có màu trắng. 

Điều này phát hiện các hoạt động triển khai vô tình khởi tạo các ô có màu đen hoặc không đặt lại trạng thái giữa các lần lặp.
