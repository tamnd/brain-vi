---
title: "CF 106106E - \u0420\u0438\u043a \u0438 \u041c\u043e\u0440\u0442\u0438, \u0430 \u0442\u0430\u043a\u0436\u0435 \u0437\u043b\u043e\u0439 \u0442\u0430\u043c\u043e\u0436\u0435\u043d\u043d\u0438\u043a"
description: "Chúng ta phải quyết định xem liệu có thể điền vào một bảng n × m tất cả các số nguyên từ 1 đến n·m đúng một lần để thỏa mãn hai điều kiện hay không."
date: "2026-06-25T11:41:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106106
codeforces_index: "E"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u042e\u043d\u0438\u043e\u0440\u044b 2024"
rating: 0
weight: 106106
solve_time_s: 47
verified: true
draft: false
---

[CF 106106E - \u0420\u0438\u043a \u0438 \u041c\u043e\u0440\u0442\u0438, \u0430 \u0442\u0430\u043a\u0436\u0435 \u0437\u043b\u043e\u0439 \u0442\u0430\u043c\u043e\u0436\u0435\u043d\u043d\u0438\u043a](https://codeforces.com/problemset/problem/106106/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta phải quyết định liệu có thể điền vào một`n × m`bảng có tất cả các số nguyên từ`1`ĐẾN`n·m`đúng một lần sao cho có đủ hai điều kiện. 

Với mỗi cặp hàng khác nhau, nếu nhân tất cả các số ở hàng thứ nhất và tất cả các số ở hàng thứ hai thì hai tích đó phải có ước chung lớn hơn`1`. 

Yêu cầu tương tự áp dụng cho mọi cặp cột khác nhau. 

Đầu vào chỉ chứa kích thước của bảng. Chúng ta không cần xây dựng bảng, chỉ cần xác định xem bảng đó có tồn tại hay không. 

Các giá trị của`n`Và`m`có thể lớn như`10^9`, điều này ngay lập tức cho chúng ta biết rằng mọi giải pháp tùy thuộc vào các ô thực tế của bảng đều không thể thực hiện được. Chúng ta không thể xây dựng bảng, lặp lại các phần tử của nó hoặc thực hiện bất kỳ phép tính nào tỷ lệ với`n·m`. Câu trả lời phải đến từ một đặc tính toán học thuần túy. 

Các trường hợp cạnh nguy hiểm nhất là bảng một chiều. 

Coi như:```
n = 1
m = 2
```Bảng phải chứa các số`1`Và`2`. Mỗi cột chứa đúng một số, vậy tích của một cột là`1`và cái còn lại là`2`. gcd của họ là`1`, vi phạm yêu cầu. Câu trả lời đúng là`NO`. 

Một ví dụ đối xứng là:```
n = 2
m = 1
```Các sản phẩm hàng là`1`Và`2`, một lần nữa nguyên tố cùng nhau. Câu trả lời đúng là`NO`. 

Trường hợp hợp lệ nhỏ nhất là:```
n = 1
m = 1
```Không có cặp hàng phân biệt và không có cặp cột phân biệt. Mọi điều kiện đều đúng nên câu trả lời đúng là`YES`. 

Một giải pháp bất cẩn in`YES`bất cứ khi nào có ít nhất một chiều bằng`1`sẽ thất bại trên cả hai`1 × 2`Và`2 × 1`. 

## Phương pháp tiếp cận 

Một cách suy nghĩ mạnh mẽ là thử xây dựng một bảng rồi kiểm tra tất cả các sản phẩm hàng và cột. Ngay cả đối với kích thước vừa phải, điều này trở nên rất lớn, bởi vì số lượng vật liệu trám có thể là`(n·m)!`. Với kích thước đạt`10^9`, hướng đi này hoàn toàn vô vọng. 

Quan sát quan trọng là các giá trị thực tế bên trong bảng có ý nghĩa ít hơn nhiều so với số`1`. 

Nếu một chiều bằng`1`và cái kia lớn hơn`1`, thì một hàng hoặc một cột chỉ gồm một ô. Vì mọi số từ`1`ĐẾN`n·m`phải xuất hiện đúng một lần thì một trong các sản phẩm hàng hoặc cột đó chính xác là`1`. Gcd của`1`với bất kỳ số nguyên dương nào khác luôn luôn là`1`, do đó điều kiện ngay lập tức thất bại. 

Phần thú vị là cho thấy rằng mọi bảng có ít nhất hai hàng và ít nhất hai cột đều khả thi. 

Giả định`n > 1`Và`m > 1`. Nếu mọi tích của hàng đều chẵn thì tích của hai hàng bất kỳ đều có chung ước số`2`. Tương tự, nếu tích mỗi cột đều chẵn thì tích của hai cột bất kỳ cũng có ước số`2`. 

Vì vậy, toàn bộ vấn đề quy về việc đặt các số chẵn sao cho mỗi hàng và mỗi cột nhận được ít nhất một số chẵn. 

có`⌊nm/2⌋`số chẵn giữa`1, 2, ..., nm`. Cho tất cả`n, m > 1`,```
⌊nm/2⌋ ≥ max(n, m)
```cung cấp đủ số chẵn để bao gồm mọi hàng và mọi cột. Sau khi đặt các số chẵn đó một cách thích hợp, tất cả các ô còn lại có thể được điền tùy ý các số còn lại. 

Do đó, mọi bảng có cả hai chiều đều lớn hơn`1`là có thể. 

Đặc tính cuối cùng cực kỳ đơn giản:```
YES  if n = 1 and m = 1
YES  if n > 1 and m > 1
NO   otherwise
```| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đặc tính toán học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Nếu`n = 1`Và`m = 1`, in`"YES"`. 

Không có cặp hàng hoặc cột riêng biệt nên tất cả các yêu cầu đều được tự động đáp ứng. 
3. Nếu chính xác một trong`n`hoặc`m`bằng`1`, in`"NO"`. 

số`1`phải chiếm một hàng hoặc cột nào đó, tạo ra một sản phẩm bằng`1`, là đồng nguyên tố với mọi sản phẩm khác. 
4. Nếu không, hãy in`"YES"`. 

Khi cả hai chiều ít nhất`2`, chúng ta có thể phân phối các số chẵn sao cho mỗi hàng và mỗi cột đều chứa một số. Khi đó mọi tích hàng và mọi tích cột đều chia hết cho`2`. 

### Tại sao nó hoạt động 

Khi một chiều là`1`và cái kia lớn hơn`1`, một số tích hàng hoặc cột bằng chính xác`1`. Bất kỳ cặp nào liên quan đến sản phẩm đó đều có gcd`1`, nên không thể thỏa mãn yêu cầu. 

Khi cả hai chiều đều vượt quá`1`, tồn tại đủ số chẵn để đặt ít nhất một số chẵn ở mỗi hàng và mỗi cột. Mỗi sản phẩm hàng sẽ chia hết cho`2`và mỗi cột tích sẽ chia hết cho`2`. Hai tích bất kỳ đều có chung ước số`2`, vì vậy tất cả các gcd cần thiết đều lớn hơn`1`. 

Hai trường hợp này bao gồm tất cả các đầu vào có thể có, làm cho việc mô tả đặc tính được hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
m = int(input())

if (n == 1 and m == 1) or (n > 1 and m > 1):
    print("YES")
else:
    print("NO")
```Việc thực hiện phản ánh trực tiếp kết quả toán học. 

Điều kiện đầu tiên xử lý đặc biệt`1 × 1`bàn. Trường hợp này hợp lệ vì không có cặp hàng hoặc cột nào để kiểm tra. 

Điều kiện thứ hai xử lý tất cả các bảng có cả hai chiều ít nhất là hai. Bằng chứng tồn tại được mô tả ở trên đảm bảo rằng luôn có thể tạo ra sự sắp xếp phù hợp. 

Mọi trường hợp còn lại đều có đúng một chiều bằng`1`, điều này là không thể vì số`1`tạo ra một tích hàng hoặc cột bằng`1`. 

Không cần số học lớn hơn so sánh đơn giản, vì vậy những giới hạn rất lớn về`n`Và`m`không liên quan. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2
```| n | m | Điều kiện phù hợp | Trả lời | 
| --- | --- | --- | --- | 
| 2 | 2 | n > 1 và m > 1 | CÓ | 

Một sự sắp xếp hợp lệ tồn tại. Ví dụ:```
1 2
4 3
```Hàng sản phẩm là`2`Và`12`, gcd của ai là`2`. Sản phẩm cột là`4`Và`6`, gcd của ai là`2`. 

### Ví dụ 2 

đầu vào:```
2
1
```| n | m | Điều kiện phù hợp | Trả lời | 
| --- | --- | --- | --- | 
| 2 | 1 | Chính xác một chiều bằng 1 | KHÔNG | 

Bảng phải chứa:```
1
2
```Các sản phẩm hàng là`1`Và`2`. gcd của họ là`1`, vì vậy yêu cầu không thành công. 

Ví dụ này chứng tỏ tại sao mọi bảng một chiều không cần thiết đều không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số so sánh được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp chạy trong thời gian không đổi bất kể kích thước lớn như thế nào`n`Và`m`là, dễ dàng phù hợp với các giới hạn lên đến`10^9`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    m = int(sys.stdin.readline())

    if (n == 1 and m == 1) or (n > 1 and m > 1):
        return "YES\n"
    return "NO\n"

# provided samples
assert run("2\n2\n") == "YES\n", "sample 1"
assert run("2\n1\n") == "NO\n", "sample 2"

# custom cases
assert run("1\n1\n") == "YES\n", "single cell"
assert run("1\n5\n") == "NO\n", "single row"
assert run("5\n1\n") == "NO\n", "single column"
assert run("1000000000\n1000000000\n") == "YES\n", "maximum dimensions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`YES`| Điều kiện trống | 
|`1 5`|`NO`| Hàng một chiều | 
|`5 1`|`NO`| Cột một chiều | 
|`10^9 10^9`|`YES`| Kích thước lớn nhất có thể | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
2
```Các số được sử dụng là`1`Và`2`. Mỗi cột chứa chính xác một giá trị, vì vậy các sản phẩm trong cột là`1`Và`2`. gcd của họ bằng`1`, vi phạm điều kiện Thuật toán rơi vào nhánh "chính xác một chiều bằng 1" và in chính xác`NO`. 

Bây giờ hãy xem xét:```
2
1
```Các sản phẩm hàng là`1`Và`2`, một lần nữa cho gcd`1`. Thuật toán lại in`NO`. 

Cuối cùng:```
1
1
```Không có cặp hàng và không có cặp cột. Mọi yêu cầu đều tự động đúng. Thuật toán nhận dạng trường hợp đặc biệt này và in ra`YES`.
