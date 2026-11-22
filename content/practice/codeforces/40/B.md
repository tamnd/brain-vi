---
title: "CF 40B - Sơn lại"
description: "Chúng ta bắt đầu với một bàn cờ n × m. Ô trên cùng bên trái có màu đen nên màu sắc xen kẽ giống hệt bàn cờ thông thường. Chỉ những ô màu đen ban đầu mới tham gia vào quá trình sơn lại."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 40
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 39"
rating: 1600
weight: 40
solve_time_s: 104
verified: true
draft: false
---
[CF 40B - Sơn lại](https://codeforces.com/problemset/problem/40/B) 

**Đánh giá:** 1600 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một`n × m`bàn cờ. Ô trên cùng bên trái có màu đen nên màu sắc xen kẽ giống hệt bàn cờ thông thường. Chỉ những ô màu đen ban đầu mới tham gia vào quá trình sơn lại. 

Vào phút thứ`0`, mọi ô màu đen đều nhận được màu`0`. Sau đó, vào phút`i`, chúng tôi sơn lại mọi ô màu đen có bốn đường chéo lân cận đã có màu`i - 1`. Tất cả các lần vẽ lại trong cùng một phút đều xảy ra đồng thời. 

Một ô có thể được sơn lại nhiều lần trong quá trình này. Nhiệm vụ là đếm chính xác có bao nhiêu ô được sơn lại`x`lần. 

Kích thước bảng tối đa là`5000 × 5000`, có nghĩa là tổng số ô có thể đạt tới`25,000,000`. Bất kỳ mô phỏng nào trên toàn bộ bảng cho nhiều lần lặp đều không thể thực hiện được. Ngay cả một quy trình kiểu BFS toàn lưới duy nhất cũng sẽ đắt tiền trong Python ở quy mô này. giá trị`x`có thể lớn như`10^9`, điều này hoàn toàn loại trừ việc mô phỏng từng phút. 

Thách thức thực sự là hiểu được quy tắc sơn lại thực sự có ý nghĩa gì về mặt hình học. 

Trường hợp cạnh tinh tế xuất hiện khi một trong các kích thước bằng`1`. 

Ví dụ:```
1 5
1
```Câu trả lời đúng là:```
3
```Ban đầu có ba ô màu đen và không có ô nào có bốn ô cạnh nhau theo đường chéo, vì vậy quá trình dừng ngay lập tức. Mỗi ô đó được sơn đúng một lần, trong một phút`0`. 

Việc triển khai bất cẩn có thể cố gắng giữ các lớp thu nhỏ vô thời hạn và vô tình tạo ra các kích thước âm hoặc đếm các ô không tồn tại. 

Một tình huống khó khăn khác là khi`x`lớn hơn mọi số lần sơn lại có thể. 

Ví dụ:```
3 3
2
```Câu trả lời đúng là:```
0
```Ô màu đen ở giữa được sơn hai lần, nhưng các ô màu đen ở góc chỉ được sơn một lần. Không có tế bào nào tồn tại đủ lâu để sơn lại lần thứ ba. 

Một mô phỏng ngây thơ có thể nghĩ sai rằng việc sơn lại sẽ tiếp tục mãi mãi vì tuyên bố có nội dung "ad infinitum", nhưng sau đủ các bước, quy trình sẽ ổn định và không còn ô nào đủ điều kiện nữa. 

Các bảng có kích thước chẵn cũng cần được xử lý cẩn thận vì số lượng ô màu đen không đối xứng. 

Ví dụ:```
2 3
1
```Các ô màu đen là:```
B W B
W B W
```Chỉ có ba tế bào tham gia. Câu trả lời là`3`, không`4`. 

Cấu trúc chẵn lẻ của bàn cờ đóng vai trò quan trọng trong suốt quá trình giải quyết. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng quá trình từng phút một. 

Chúng tôi có thể lưu trữ số lần sơn lại hiện tại của mỗi ô màu đen và quét bảng liên tục. Trong mỗi lần lặp lại, chúng tôi kiểm tra xem tất cả bốn đường chéo lân cận có được sơn lại ở phút trước hay không. Nếu có, ô hiện tại cũng được sơn lại. 

Mô phỏng này đúng vì nó tuân theo chính xác phát biểu. Vấn đề là chi phí. Số lượng lớp có thể lớn tới mức`min(n, m) / 2`và mỗi lần lặp sẽ quét toàn bộ bảng. Với kích thước lên tới`5000`, điều này sẽ trở thành hàng chục hoặc hàng trăm triệu thao tác, điều này vốn đã gây khó chịu trong Python. Quan trọng hơn, bản thân bo mạch có thể chứa 25 triệu ô, do đó, ngay cả việc lưu trữ các cấu trúc phụ trợ lớn cũng trở nên đắt đỏ. 

Quan sát quan trọng là việc sơn lại hoạt động giống hệt như bong tróc các lớp ở đường viền. 

Một ô màu đen chỉ tồn tại được sau một lần sơn lại nếu tất cả bốn ô lân cận theo đường chéo tồn tại và cũng tồn tại ở bước trước đó. Điều đó có nghĩa là ô không được nằm gần bất kỳ đường viền nào. Sau một lần sơn lại, lớp đường chéo ngoài cùng sẽ biến mất. Sau một lần sơn lại, lớp tiếp theo sẽ biến mất, v.v. 

Vì vậy số lần ô màu đen được sơn lại chỉ phụ thuộc vào khoảng cách đường chéo tối thiểu của nó với đường viền. 

Thay vì mô phỏng quá trình, chúng ta có thể đếm trực tiếp có bao nhiêu ô đen thuộc mỗi lớp đường chéo. 

Giả sử một ô tồn tại thông qua việc sơn lại`x - 1`. Thế thì ít nhất phải`x - 1`bước ra khỏi mọi biên giới, cả theo chiều dọc và chiều ngang. Loại bỏ các lớp này để lại một hình chữ nhật bên trong:```
(n - 2(x - 1)) × (m - 2(x - 1))
```Mỗi ô màu đen bên trong hình chữ nhật đó ít nhất được sơn lại`x`lần. 

Nếu chúng ta biểu thị bằng`f(k)`số ô màu đen được sơn lại ít nhất`k`thì câu trả lời chúng ta cần là:```
f(x) - f(x + 1)
```bởi vì đó chính xác là những tế bào tồn tại để sơn lại`x - 1`nhưng không vượt quá. 

Đếm các ô màu đen bên trong một hình chữ nhật thật dễ dàng. Vì góc trên bên trái có màu đen nên số lượng ô màu đen trong một`a × b`hình chữ nhật là:```
(a * b + 1) // 2
```khi được tính với cùng một căn chỉnh chẵn lẻ. 

Điều này làm giảm toàn bộ vấn đề thành một vài phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm · phút(n, m)) | O(nm) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`x`. 
2. Xác định hàm`count_black(a, b)`trả về số lượng ô đen trong một`a × b`hình chữ nhật có góc trên bên trái màu đen. 

Công thức là:```
(a * b + 1) // 2
```bởi vì các ô màu đen và trắng xen kẽ nhau, và màu đen sẽ nhận được ô phụ khi vùng đó là số lẻ. 
3. Hãy để`k = x - 1`. 

Một ô được sơn lại chính xác`x`thời gian phải tồn tại qua`x - 1`bước đi thu hẹp lại. 
4. Tính các kích thước của hình chữ nhật tồn tại ít nhất`x`sơn lại:```
a1 = n - 2k
b1 = m - 2k
```Nếu một trong hai chiều không dương thì không có tế bào nào tồn tại lâu như vậy. 
5. Tính các kích thước của hình chữ nhật tồn tại ít nhất`x + 1`sơn lại:```
a2 = n - 2(k + 1)
b2 = m - 2(k + 1)
```6. Tính toán:```
at_least_x = count_black(a1, b1)
at_least_x_plus_1 = count_black(a2, b2)
```coi hình chữ nhật có kích thước không dương là chứa 0 ô. 
7. Câu trả lời cuối cùng là:```
at_least_x - at_least_x_plus_1
```### Tại sao nó hoạt động 

Một ô màu đen chỉ có thể được sơn lại nếu cả bốn ô lân cận theo đường chéo cũng tồn tại ở bước trước đó. Điều kiện đó loại bỏ chính xác một lớp đường chéo khỏi mọi đường viền trong mỗi lần lặp. 

Sau đó`k`lặp đi lặp lại, các ô còn sót lại chính xác là các ô màu đen bên trong hình chữ nhật thu được bằng cách loại bỏ`k`hàng và cột từ bốn phía. 

Vì thế`f(k)`đếm chính xác các ô được sơn lại ít nhất`k`lần. Trừ các lớp liên tiếp cô lập các ô được sơn lại chính xác`x`lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_black(a, b):
    if a <= 0 or b <= 0:
        return 0
    return (a * b + 1) // 2

def solve():
    n, m = map(int, input().split())
    x = int(input())

    k = x - 1

    at_least_x = count_black(n - 2 * k, m - 2 * k)
    at_least_x_plus_1 = count_black(n - 2 * (k + 1), m - 2 * (k + 1))

    print(at_least_x - at_least_x_plus_1)

solve()
```Hàm trợ giúp xử lý cốt lõi hình học của vấn đề. Nếu hình chữ nhật còn lại có kích thước không dương thì không có ô nào tồn tại qua nhiều lớp đó, do đó hàm trả về 0 ngay lập tức. 

biểu hiện`(a * b + 1) // 2`đếm các ô màu đen theo mẫu bàn cờ bắt đầu bằng màu đen ở góc trên bên trái. Điều này áp dụng cho cả vùng chẵn và vùng lẻ. 

Biến`k = x - 1`biểu thị số lượng thao tác thu nhỏ mà một ô phải tồn tại trước khi có thể sơn lại chính xác`x`lần. 

Việc thực hiện tránh mô phỏng hoàn toàn. Không cần mảng, BFS hoặc đệ quy. Mọi thứ đều xuất phát từ cách giải thích hình học của các lớp đường chéo. 

Một lỗi dễ gây nhầm lẫn là “vẽ chính xác`x`lần” với “sống sót`x`vòng”. Một ô được sơn chính xác`x`thời gian chỉ tồn tại qua vòng`x - 1`, đó là lý do tại sao mã sử dụng`k = x - 1`. 

Một vấn đề phổ biến khác xuất hiện khi tính toán lớp tiếp theo. Chúng tôi trừ`2 * (k + 1)`bởi vì mỗi lần sơn lại bổ sung sẽ loại bỏ một hàng và một cột ở mọi phía. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1
```Chúng tôi tính toán: 

| Biến | Giá trị | 
| --- | --- | 
|`k`|`0`| 
|`a1, b1`|`3, 3`| 
|`a2, b2`|`1, 1`| 
|`at_least_x`|`5`| 
|`at_least_x_plus_1`|`1`| 
| Trả lời |`4`| 

Ô màu đen ở giữa tồn tại đủ lâu để được sơn lại hai lần, vì vậy nó không được tính trong số các ô được sơn lại đúng một lần. Bốn ô góc đen còn lại tạo thành câu trả lời. 

### Ví dụ 2 

đầu vào:```
5 5
2
```Chúng tôi tính toán: 

| Biến | Giá trị | 
| --- | --- | 
|`k`|`1`| 
|`a1, b1`|`3, 3`| 
|`a2, b2`|`1, 1`| 
|`at_least_x`|`5`| 
|`at_least_x_plus_1`|`1`| 
| Trả lời |`4`| 

Lớp đường chéo bên ngoài biến mất sau lần sơn lại đầu tiên. Bên trong phần còn lại`3 × 3`khu vực, bốn ô màu đen biến mất sau lần sơn lại thứ hai, trong khi phần trung tâm tồn tại lâu hơn sau một lần sơn lại. 

Ví dụ này xác nhận việc giải thích lớp một cách trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc là cực kỳ lớn đối với các giải pháp dựa trên mô phỏng, nhưng cách tiếp cận này không bao giờ phụ thuộc vào kích thước bảng ngoại trừ thông qua các công thức số học. Nó thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    def count_black(a, b):
        if a <= 0 or b <= 0:
            return 0
        return (a * b + 1) // 2

    n, m = map(int, input().split())
    x = int(input())

    k = x - 1

    at_least_x = count_black(n - 2 * k, m - 2 * k)
    at_least_x_plus_1 = count_black(n - 2 * (k + 1), m - 2 * (k + 1))

    print(at_least_x - at_least_x_plus_1)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("3 3\n1\n") == "4", "sample 1"

# minimum board
assert run("1 1\n1\n") == "1", "single cell"

# no further repaint possible
assert run("1 5\n2\n") == "0", "thin board"

# center survives longer
assert run("5 5\n3\n") == "1", "only center survives"

# large dimensions with impossible x
assert run("5000 5000\n3000\n") == "0", "x too large"

# rectangular board
assert run("2 3\n1\n") == "3", "parity handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Bảng nhỏ nhất có thể | 
|`1 5 / 2`|`0`| Không có ô nào có bốn ô lân cận theo đường chéo | 
|`5 5 / 3`|`1`| Lớp trung tâm sâu nhất còn sót lại | 
|`5000 5000 / 3000`|`0`| Số lượng sơn lại lớn không thể | 
|`2 3 / 1`|`3`| Bàn cờ chẵn lẻ đúng | 

## Vỏ cạnh 

Hãy xem xét bảng một hàng:```
1 5
1
```Thuật toán tính toán:```
k = 0
a1 = 1
b1 = 5
a2 = -1
b2 = 3
```Vì thế:```
at_least_x = 3
at_least_x_plus_1 = 0
```Câu trả lời là`3`. 

Điều này đúng vì mỗi ô màu đen được sơn một lần mỗi phút`0`, nhưng không có ô nào có bốn ô lân cận theo đường chéo, nên việc sơn lại không bao giờ tiếp tục. 

Bây giờ hãy xem xét:```
3 3
2
```Chúng tôi tính toán:```
k = 1
a1 = 1
b1 = 1
a2 = -1
b2 = -1
```Vì thế:```
at_least_x = 1
at_least_x_plus_1 = 0
```Câu trả lời là`1`. 

Chỉ có ô trung tâm tồn tại đủ lâu để được sơn lại hai lần. Các ô màu đen ở góc biến mất sau khi lớp đầu tiên co lại. 

Cuối cùng, hãy xem xét một bảng có kích thước chẵn:```
2 3
1
```Hình chữ nhật còn lại cho`x = 1`là bảng đầy đủ:```
2 × 3
```Số ô đen là:```
(2 * 3 + 1) // 2 = 3
```Điều này xác nhận rằng công thức đếm bàn cờ xử lý các kích thước chẵn lẻ hỗn hợp một cách chính xác.
