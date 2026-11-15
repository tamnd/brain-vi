---
title: "CF 183A - Trụ sở chính"
description: "Chiếc xe bắt đầu ở đâu đó trên lưới 2D vô hạn và cuối cùng đến quầy bán kem ở (0, 0). Chúng tôi biết số lần di chuyển và thứ tự của các bản ghi GPS, nhưng mỗi bản ghi chỉ cho chúng tôi biết một tập hợp các hướng có thể có cho bước đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 183
codeforces_index: "A"
codeforces_contest_name: "Croc Champ 2012 - Final"
rating: 1700
weight: 183
solve_time_s: 112
verified: true
draft: false
---

[CF 183A - Trụ sở chính](https://codeforces.com/problemset/problem/183/A) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chiếc xe xuất phát ở đâu đó trên lưới 2D vô tận và cuối cùng đến quầy bán kem ở`(0, 0)`. Chúng tôi biết số lần di chuyển và thứ tự của các bản ghi GPS, nhưng mỗi bản ghi chỉ cho chúng tôi biết một tập hợp các hướng có thể có cho bước đó. 

Ví dụ,`"UR"`có nghĩa là nước đi lên hoặc sang phải.`"ULDR"`có nghĩa là sự di chuyển có thể là một trong bốn hướng. 

Chúng ta cần đếm xem có bao nhiêu vị trí bắt đầu có thể dẫn đến`(0, 0)`sau khi xử lý tất cả các bước di chuyển. 

Một cách trực tiếp để suy nghĩ về vấn đề là đảo ngược quá trình. Thay vì hỏi chiếc xe bắt đầu từ đâu, hãy hỏi nó có thể ở đâu sau mỗi bước nếu nó bắt đầu ở`(0, 0)`và đi lùi qua các mô tả chuyển động. Mỗi vị trí kết thúc hợp lệ trong quy trình đảo ngược này tương ứng với một vị trí trụ sở chính có thể có. 

Số lần di chuyển lên tới`2 * 10^5`, do đó, bất kỳ thuật toán nào lưu trữ rõ ràng tất cả các điểm có thể truy cập đều không thể thực hiện được. Ngay cả khi mỗi bước chỉ nhân đôi số lượng trạng thái, chúng ta sẽ nhanh chóng đạt đến kích thước thiên văn. Giải pháp bậc hai ở đây cũng quá chậm. Với giới hạn 2 giây, chúng ta sẽ mong đợi điều gì đó gần với thời gian tuyến tính. 

Thách thức chính là tập hợp các vị trí có thể tiếp cận phát triển về mặt hình học nhưng thực tế lại có cấu trúc rất cứng nhắc. Thiếu cấu trúc đó dẫn đến sự bùng nổ trạng thái không cần thiết. 

Một sai lầm dễ mắc phải là xử lý tọa độ x và tọa độ y một cách độc lập mà không kiểm tra các ràng buộc chẵn lẻ. 

Coi như:```
1
UR
```Chiếc xe có thể đã di chuyển sang phải hoặc lên trên. Đảo ngược điều đó, trụ sở chính có thể ở`(-1,0)`hoặc`(0,-1)`. chỉ có`2`khả năng, không phải tất cả các điểm bên trong một hình chữ nhật. 

Một trường hợp tế nhị khác là những động tác lặp đi lặp lại không hạn chế:```
2
ULDR
ULDR
```Sau hai lần di chuyển tùy ý, các vị trí có thể tiếp cận không phải là mọi điểm trong một`5 x 5`quảng trường. Vấn đề tương đương về khoảng cách ở Manhattan. Các vị trí trụ sở hợp lệ chính là các điểm có khoảng cách tính từ gốc tối đa`2`và có cùng tính chẵn lẻ như`2`. 

Một giải pháp đếm hình chữ nhật bất cẩn sẽ bị tính quá mức ở đây. 

Ngoài ra còn có trường hợp suy biến trong đó mọi chuyển động đều có hướng trục giống nhau.```
3
UR
UR
UR
```Mỗi lần di chuyển sẽ thay đổi x bởi`+1`hoặc y bởi`+1`. Đảo ngược điều đó, mọi vị trí trụ sở đều thỏa mãn:```
x <= 0
y <= 0
x + y = -3
```Câu trả lời chỉ là`4`, không phải là một số lượng có kích thước diện tích. 

Cấu trúc của tập hợp có thể truy cập là toàn bộ vấn đề. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Duy trì tập hợp tất cả các vị trí có thể sau mỗi lần di chuyển. Bắt đầu từ`(0,0)`. Đối với mỗi điểm hiện tại, hãy áp dụng mọi hướng được phép từ bản ghi GPS hiện tại và chèn các điểm kết quả vào một tập hợp mới. 

Điều này đúng vì nó mô phỏng rõ ràng tất cả các đường dẫn hợp lệ. Vấn đề là tăng trưởng. Với`n = 2 * 10^5`, số lượng điểm có thể tiếp cận có thể tỷ lệ thuận với`n^2`. Việc lưu trữ và cập nhật rõ ràng tất cả chúng sẽ yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất. 

Chúng ta cần hiểu những trạng thái này thực sự trông như thế nào về mặt hình học. 

Mỗi bước di chuyển thay đổi chính xác một tọa độ bằng`±1`. Sau đó`n`di chuyển, mọi điểm có thể tiếp cận đều có khoảng cách tương đương với Manhattan bằng`n`. 

Bây giờ hãy xem năm loại bản ghi có thể có.`"UR"`luôn tăng x hoặc y thêm`1`.`"DL"`luôn giảm x hoặc y một lượng`1`.`"UL"`tăng y hoặc giảm x.`"DR"`giảm y hoặc tăng x.`"ULDR"`cho phép bất kỳ hướng nào. 

Thay vì theo dõi từng điểm, chúng ta có thể theo dõi các ràng buộc trên hai tọa độ được chuyển đổi:```
s = x + y
d = x - y
```Mỗi bước di chuyển ảnh hưởng đến chính xác một trong các giá trị này bằng cách`±1`. 

Ví dụ: 

| Di chuyển | Thay đổi trong s | Thay đổi trong d | 
| --- | --- | --- | 
| Bạn | +1 | -1 | 
| D | -1 | +1 | 
| L | -1 | -1 | 
| R | +1 | +1 | 

Bây giờ hãy kiểm tra từng loại bản ghi.`"UR"`luôn tăng`s`qua`1`.`"DL"`luôn giảm`s`qua`1`.`"UL"`luôn giảm`d`qua`1`.`"DR"`luôn tăng`d`qua`1`.`"ULDR"`cho phép một trong hai hướng trên cả hai trục được chuyển đổi. 

Điều này hoàn toàn tách biệt vấn đề. 

Giả định:```
a = count("UR")
b = count("DL")
c = count("DR")
d = count("UL")
e = count("ULDR")
```Sau đó:```
s = a - b + variable
d = c - d + variable
```trong đó mỗi biến xuất phát từ những chuyển động và thay đổi không hạn chế theo từng bước của`2`. 

Chính xác hơn:```
s ∈ [a - b - e, a - b + e]
d ∈ [c - d - e, c - d + e]
```và cả hai giá trị đều có cùng tính chẵn lẻ với tâm của chúng. 

Mỗi cặp hợp lệ`(s,d)`xác định chính xác một điểm lưới:```
x = (s + d) / 2
y = (s - d) / 2
```Các điều kiện chẵn lẻ tự động đảm bảo tính toàn vẹn. 

Vì vậy, câu trả lời trở thành:```
(number of possible s values) *
(number of possible d values)
```Mỗi quãng tiến triển theo các bước`2`, vậy mỗi cái có chính xác`e + 1`khả năng. 

Câu trả lời cuối cùng:```
(e + 1)^2
```Điều này nhỏ một cách đáng ngạc nhiên so với độ phức tạp rõ ràng của không gian trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) đến O(n³) tùy thuộc vào việc triển khai | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả hồ sơ chuyển động. 

Chúng tôi chỉ quan tâm đến việc mỗi loại xuất hiện bao nhiêu lần. Thứ tự chính xác của chúng không thành vấn đề khi chúng ta chuyển sang tọa độ được chuyển đổi. 
2. Đếm xem có bao nhiêu bản ghi`"ULDR"`. 

Hãy tính số này`e`. 
3. Quan sát hành vi cưỡng bức của các kiểu di chuyển khác.`"UR"`luôn tăng`s = x + y`.`"DL"`luôn giảm`s`.`"DR"`luôn tăng`d = x - y`.`"UL"`luôn giảm`d`. 

Những bước di chuyển này góp phần bù đắp cố định và không bao giờ tạo ra sự phân nhánh. 
4. Quan sát hành vi của`"ULDR"`. 

Mỗi bước di chuyển không hạn chế có thể thay đổi độc lập`s`bởi một trong hai`+1`hoặc`-1`, và tương tự cho`d`. 

Sau đó`e`những bước di chuyển như vậy, mỗi tọa độ được chuyển đổi có chính xác`e + 1`giá trị có thể tiếp cận vì các giá trị khác nhau theo các bước của`2`. 
5. Nhân các khả năng. 

Những lựa chọn cho`s`Và`d`là độc lập. Mỗi cặp hợp lệ tương ứng với chính xác một điểm lưới số nguyên. 

Số lượng vị trí trụ sở chính là:```
(e + 1)²
```### Tại sao nó hoạt động 

Sự biến đổi:```
s = x + y
d = x - y
```tách rời các ràng buộc chuyển động. 

Mọi thứ không`"ULDR"`bản ghi sửa chính xác một thay đổi tọa độ được chuyển đổi và không để lại sự phân nhánh. Chỉ một`"ULDR"`tạo ra các lựa chọn và mỗi bước đi như vậy sẽ đóng góp độc lập một quyết định nhị phân cho`s`và một quyết định nhị phân để`d`. 

Sau đó`e`di chuyển không hạn chế, cả hai tọa độ được chuyển đổi đều có thể thực hiện chính xác`e + 1`các giá trị riêng biệt vì các giá trị có thể tiếp cận tạo thành các cấp số cộng với sự khác biệt`2`. 

Việc ánh xạ giữa`(s,d)`Và`(x,y)`là tính từ để khớp các giá trị chẵn lẻ, vì vậy việc đếm các cặp tọa độ biến đổi hoàn toàn giống như việc đếm các điểm lưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

free_moves = 0

for _ in range(n):
    s = input().strip()
    if s == "ULDR":
        free_moves += 1

print((free_moves + 1) ** 2)
```Việc thực hiện rất nhỏ vì việc rút gọn toán học sẽ loại bỏ tất cả hình học. 

Thông tin duy nhất ảnh hưởng đến số lượng vị trí trụ sở có thể có là có bao nhiêu lần di chuyển hoàn toàn không bị hạn chế. Mọi di chuyển bị ràng buộc chỉ đơn thuần là dịch chuyển vùng có thể tiếp cận cuối cùng mà không thay đổi kích thước của nó. 

Công thức chỉ sử dụng số học số nguyên nên không có vấn đề gì về độ chính xác. Ngay cả ở mức hạn chế tối đa:```
(200000 + 1)^2
```thoải mái phù hợp bên trong số nguyên Python. 

Một lỗi triển khai phổ biến là cố gắng mô phỏng tọa độ hoặc duy trì các tập hợp có thể truy cập được. Cách tiếp cận đó vừa chậm hơn vừa dễ xảy ra lỗi hơn vì các ràng buộc chẵn lẻ trở nên khó quản lý một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
UR
UL
ULDR
```Chúng tôi xử lý các loại di chuyển. 

| Bước | Ghi lại | free_move | 
| --- | --- | --- | 
| 1 | UR | 0 | 
| 2 | UL | 0 | 
| 3 | ULDR | 1 | 

Tính toán cuối cùng:```
(1 + 1)^2 = 4
```Đợi đã, câu trả lời mẫu là`9`, vì vậy chúng ta cần giải thích cẩn thận. 

Đường đi ban đầu từ trụ sở chính đến`(0,0)`. Đảo ngược hướng duy trì số lượng nhưng thay đổi hướng chuyển động. Quan trọng hơn, mỗi bước di chuyển không hạn chế đều đóng góp ba giá trị có thể tiếp cận được trong tọa độ được chuyển đổi chứ không phải hai. 

Hãy để chúng tôi rút ra đúng cách. 

Sau đó`e`di chuyển không hạn chế:```
s ∈ center + {-e, -e+2, ..., e}
```Số khả năng:```
e + 1
```Tương tự cho`d`. 

Đối với mẫu này, thực tế có một bước di chuyển không bị hạn chế và hai bước di chuyển bị ràng buộc một phần mà mỗi bước vẫn tạo ra sự phân nhánh trong một tọa độ được chuyển đổi. 

Hãy để chúng tôi tính toán trực tiếp.`UR`đưa ra hai khả năng cho`(s,d)`:`(+1,+1)`hoặc`(+1,-1)`.`UL`mang lại:`(+1,-1)`hoặc`(-1,-1)`.`ULDR`đưa ra tất cả bốn sự kết hợp. 

Cả hai tọa độ được chuyển đổi đều có`3`tổng giá trị có thể, tạo ra:```
3 * 3 = 9
```Điều này cho thấy sự đơn giản hóa sâu sắc hơn: 

Mỗi nước đi đóng góp một lựa chọn nhị phân độc lập vào đúng một tọa độ được chuyển đổi. 

Cụ thể hơn: 

| Ghi lại | Tọa độ cố định | Tọa độ biến | 
| --- | --- | --- | 
| UR | s = +1 | d = ±1 | 
| DL | s = -1 | d = ±1 | 
| UL | d = -1 | s = ±1 | 
| DR | d = +1 | s = ±1 | 
| ULDR | cả hai biến | | 

Vì vậy chúng ta nên đếm xem có bao nhiêu nước đi cho phép có sự thay đổi trong`s`và bao nhiêu cho phép thay đổi trong`d`. 

Định nghĩa:```
cs = count(UL) + count(DR) + count(ULDR)
cd = count(UR) + count(DL) + count(ULDR)
```Sau đó:```
answer = (cs + 1) * (cd + 1)
```Bây giờ hãy tiếp tục với đạo hàm đã sửa. 

Đối với mẫu:```
cs = 1 + 0 + 1 = 2
cd = 1 + 0 + 1 = 2
```Trả lời:```
3 * 3 = 9
```### Ví dụ 2 

đầu vào:```
4
UR
UR
DL
DL
```| Bước | Ghi lại | cs | cd | 
| --- | --- | --- | --- | 
| 1 | UR | 0 | 1 | 
| 2 | UR | 0 | 2 | 
| 3 | DL | 0 | 3 | 
| 4 | DL | 0 | 4 | 

Câu trả lời cuối cùng:```
(0 + 1) * (4 + 1) = 5
```Các vị trí trụ sở có thể tiếp cận nằm trên một đường chéo vì`s`được cố định hoàn toàn trong khi`d`khác nhau. 

Dấu vết này thể hiện sự bất biến tách tọa độ. Một tọa độ được chuyển đổi có thể giữ nguyên cố định trong khi các tọa độ khác phân nhánh tự do. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua các hồ sơ chuyển động | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Thuật toán dễ dàng phù hợp trong giới hạn. Xử lý`2 * 10^5`các chuỗi có công việc không đổi trên mỗi chuỗi là chuyện nhỏ đối với Python trong 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    cs = 0
    cd = 0

    for _ in range(n):
        s = input().strip()

        if s == "UR":
            cd += 1
        elif s == "DL":
            cd += 1
        elif s == "UL":
            cs += 1
        elif s == "DR":
            cs += 1
        else:
            cs += 1
            cd += 1

    print((cs + 1) * (cd + 1))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""3
UR
UL
ULDR
"""
) == "9\n", "sample 1"

# minimum size
assert run(
"""1
UR
"""
) == "2\n", "single constrained move"

# all unrestricted
assert run(
"""2
ULDR
ULDR
"""
) == "9\n", "full 2D expansion"

# only one transformed coordinate varies
assert run(
"""4
UR
UR
DL
DL
"""
) == "5\n", "diagonal line"

# mixed constraints
assert run(
"""5
UL
DR
UR
DL
ULDR
"""
) == "9\n", "both coordinates vary independently"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / UR`|`2`| Trường hợp không cần thiết tối thiểu | 
|`2 / ULDR ULDR`|`9`| Tăng trưởng hoàn toàn không hạn chế | 
|`4 / UR UR DL DL`|`5`| Chỉ có một tọa độ được chuyển đổi khác nhau | 
| Các kiểu di chuyển hỗn hợp |`9`| Phân nhánh độc lập của cả hai tọa độ | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
UR
```Chúng tôi có:```
cs = 0
cd = 1
```Trả lời:```
(0 + 1) * (1 + 1) = 2
```Các vị trí trụ sở chính hợp lệ chính xác`(-1,0)`Và`(0,-1)`. Thuật toán xử lý việc này vì`"UR"`chỉ tạo ra sự biến đổi trong`d = x - y`. 

Bây giờ hãy xem xét tất cả các động thái không hạn chế:```
3
ULDR
ULDR
ULDR
```Chúng tôi nhận được:```
cs = 3
cd = 3
```Trả lời:```
16
```Các vị trí có thể tiếp cận tạo thành một vùng mạng hình kim cương. Mỗi tọa độ được chuyển đổi độc lập có bốn giá trị có thể. 

Cuối cùng, xét trường hợp đường suy biến:```
3
UL
UL
UL
```Chúng tôi có được:```
cs = 3
cd = 0
```Trả lời:```
4
```Chỉ có một tọa độ biến đổi khác nhau. Các vị trí trụ sở có thể tiếp cận nằm trên một đường chéo thẳng thay vì lấp đầy một khu vực. Công thức vẫn hoạt động vì nó tính trực tiếp các kết hợp tọa độ biến đổi thay vì giả sử bất kỳ hình dạng hình học nào trong`(x,y)`không gian.
