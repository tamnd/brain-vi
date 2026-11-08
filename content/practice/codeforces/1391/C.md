---
title: "CF 1391C - Hoán vị tuần hoàn"
description: "Chúng ta được cho một hoán vị của các số 1...n. Từ hoán vị đó, một biểu đồ được xây dựng trên các chỉ số của mảng. Đối với mọi vị trí i, chúng ta kết nối nó với phần tử lớn hơn gần nhất ở bên trái và phần tử lớn hơn gần nhất ở bên phải, bất cứ khi nào các phần tử đó tồn tại."
date: "2026-06-11T10:17:16+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 1391
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 663 (Div. 2)"
rating: 1500
weight: 1391
solve_time_s: 89
verified: true
draft: false
---

[CF 1391C - Hoán vị tuần hoàn ](https://codeforces.com/problemset/problem/1391/C) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, dp, đồ thị, toán 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số`1...n`. Từ hoán vị đó, một biểu đồ được xây dựng trên các chỉ số của mảng. 

Đối với mọi vị trí`i`, chúng ta kết nối nó với phần tử lớn hơn gần nhất ở bên trái và phần tử lớn hơn gần nhất ở bên phải, bất cứ khi nào các phần tử đó tồn tại. Đồ thị là vô hướng. 

Một hoán vị được gọi là tuần hoàn nếu đồ thị thu được chứa ít nhất một chu trình đơn. Nhiệm vụ không phải là xây dựng biểu đồ cho một hoán vị cụ thể. Thay vào đó, chúng tôi chỉ được cung cấp`n`, và chúng ta phải đếm có bao nhiêu hoán vị của độ dài`n`tạo ra một đồ thị chứa một chu trình. 

Câu trả lời phải được báo cáo modulo`10^9 + 7`. 

Hạn chế là quan sát quan trọng. Đây`n`có thể lớn như`10^6`. Giá trị lớn này ngay lập tức loại trừ bất kỳ cách tiếp cận nào gây ra các hoán vị riêng lẻ. có`n!`hoán vị, và thậm chí tạo ra chúng là không thể. Chúng ta cần một công thức tính toán thuần túy có giá trị được tính theo thời gian tuyến tính hoặc gần tuyến tính. 

Việc xây dựng biểu đồ có vẻ phức tạp, nhưng ràng buộc gợi ý rõ ràng rằng câu trả lời cuối cùng phải phụ thuộc vào đặc tính cấu trúc của tất cả các biểu đồ như vậy thay vì kiểm tra từng hoán vị. 

Một sai lầm phổ biến là cho rằng đồ thị có thể chứa nhiều loại chu trình khác nhau và chúng ta phải đếm chúng một cách trực tiếp. Ví dụ, đối với`n = 4`, có`24`hoán vị và chu trình kiểm tra thủ công có vẻ khả thi. Vì`n = 10^6`, viễn cảnh đó là vô vọng. 

Một bẫy dễ dàng khác là đếm các hoán vị có đồ thị được kết nối. Tính kết nối không giống như việc chứa đựng một chu trình. Một cây được kết nối và không theo chu kỳ. Sự khác biệt hóa ra chính xác là điều mở ra giải pháp. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra mọi hoán vị, xây dựng biểu đồ và chạy thuật toán biểu đồ như DFS để phát hiện các chu trình. 

Đối với một hoán vị đơn, việc tìm phần tử lớn hơn gần nhất ở mỗi bên có thể được thực hiện bằng cách sử dụng ngăn xếp đơn điệu trong`O(n)`. Phát hiện chu kỳ cũng`O(n)`. Do đó, một hoán vị có thể được xử lý trong thời gian tuyến tính. 

Vấn đề là số lượng hoán vị. có`n!`của họ. Ngay cả đối với`n = 15`, chuyện này đã là về rồi`1.3 × 10^12`khả năng. Cách tiếp cận vũ phu gần như không thể sử dụng được ngay lập tức. 

Sự đột phá đến từ việc hiểu được biểu đồ được tạo ra bởi công trình này. 

Biểu đồ này là biểu đồ cây Descartes nổi tiếng. Nếu chúng ta lấy phần tử lớn nhất của hoán vị thì nó sẽ trở thành nghiệm. Theo cách đệ quy, các phần tử bên trái của nó tạo thành cây con bên trái và các phần tử bên phải của nó tạo thành cây con bên phải. 

Thuộc tính quan trọng là biểu đồ được xây dựng trong câu lệnh luôn được kết nối. Mọi nút ngoại trừ phần tử tối đa đều có ít nhất một kết nối với phần tử lớn hơn, do đó, mọi đỉnh cuối cùng đều có thể đạt đến mức tối đa toàn cục. 

Bây giờ hãy xem xét số cạnh. 

Mỗi vị trí có thể đóng góp tối đa một cạnh cho phần tử lớn hơn gần nhất ở bên trái và một cạnh cho phần tử lớn hơn gần nhất ở bên phải. Phân tích sâu hơn về cấu trúc này cho thấy đồ thị kết quả có chính xác`n - 1`các cạnh khi và chỉ khi hoán vị tương ứng với cây Descartes không có chu kỳ phân nhánh. Trong thực tế, biểu đồ là một cây chính xác khi phần tử lớn nhất nằm ở một trong các đầu của hoán vị và theo đệ quy, cùng một thuộc tính cho cả hai phần còn lại. 

Điều kiện đệ quy đó đặc trưng cho các hoán vị có thể đạt được bằng cách liên tục đặt giá trị lớn nhất còn lại hiện tại ở đầu bên trái hoặc đầu bên phải. 

Có bao nhiêu hoán vị như vậy tồn tại? 

Địa điểm`n`Đầu tiên. Nó phải ở một đầu. 

Sau khi loại bỏ`n`, điều kiện tương tự phải được giữ cho`n-1`. 

Với mỗi giá trị từ`n`xuống`2`, chúng tôi độc lập chọn xem nó đi về đầu bên trái hay đầu bên phải của chuỗi còn lại. 

Điều đó mang lại chính xác`2^(n-1)`hoán vị có đồ thị là một cây. 

Vì mọi hoán vị đều tạo ra một biểu đồ liên thông, nên một biểu đồ có tính tuần hoàn chính xác khi nó **không** là một cây. 

Tổng số hoán vị là`n!`. 

Kể từ đây:$$\text{cyclic permutations} = n! - 2^{\,n-1}$$Tất cả các tính toán được thực hiện modulo`10^9+7`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`. 
2. Tính toán`n! mod M`, Ở đâu`M = 10^9 + 7`. 
3. Tính toán`2^(n-1) mod M`. 
4. Trừ giá trị thứ hai từ modulo đầu tiên`M`. 
5. Xuất kết quả. 

Phần không tầm thường duy nhất là đối số đếm. Khi chúng ta biết rằng đồ thị không tuần hoàn chính xác là các hoán vị cuối cực đại đệ quy, việc đếm chúng sẽ trở thành lũy thừa đơn giản của hai. 

### Tại sao nó hoạt động 

Mọi biểu đồ được tạo ra bởi công trình đều được kết nối. Một biểu đồ được kết nối trên`n`các đỉnh là không tuần hoàn khi và chỉ nếu nó chứa chính xác`n-1`các cạnh, nghĩa là nó là một cái cây. 

Các hoán vị tạo ra cây chính xác là những hoán vị mà phần tử lớn nhất hiện tại luôn xuất hiện ở một trong các đầu của khoảng hiện tại. Việc loại bỏ các phần chia tối đa không có gì và điều kiện tương tự phải được giữ đệ quy đối với các phần tử còn lại. 

Xây dựng hoán vị như vậy từ lớn nhất đến nhỏ nhất, mỗi giá trị có đúng hai lựa chọn, đầu bên trái hoặc đầu bên phải. có`n-1`quyết định độc lập, mang lại`2^(n-1)`hoán vị tạo cây. 

Vì có`n!`tổng số hoán vị, số tạo ra ít nhất một chu kỳ là$$n! - 2^{n-1}.$$## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n = int(input())

fact = 1
for i in range(2, n + 1):
    fact = (fact * i) % MOD

trees = pow(2, n - 1, MOD)

print((fact - trees) % MOD)
```Việc thực hiện trực tiếp theo công thức tính. 

Vòng lặp đầu tiên tính toán`n!`modulo`10^9+7`. Từ`n ≤ 10^6`, một triệu phép nhân mô-đun dễ dàng đủ nhanh. 

Tính toán lũy thừa mô-đun tích hợp của Python`2^(n-1)`TRONG`O(log n)`thời gian. 

Phép trừ được thực hiện modulo`MOD`. biểu thức```
(fact - trees) % MOD
```xử lý chính xác các giá trị trung gian âm. 

Không sử dụng đệ quy, điều này tránh được các vấn đề về độ sâu đệ quy đối với đầu vào lớn nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```Tính hai đại lượng đó. 

| Bước | Giá trị | 
| --- | --- | 
|`4!`| 24 | 
|`2^(4-1)`| 8 | 
| Trả lời | 24 - 8 = 16 | 

Đầu ra:```
16
```Điều này phù hợp với mẫu. 

Dấu vết thể hiện nguyên tắc đếm trung tâm: tất cả 24 hoán vị được phân chia thành 8 hoán vị tạo cây và 16 hoán vị tuần hoàn. 

### Ví dụ 2 

đầu vào:```
3
```| Bước | Giá trị | 
| --- | --- | 
|`3!`| 6 | 
|`2^(3-1)`| 4 | 
| Trả lời | 6 - 4 = 2 | 

Đầu ra:```
2
```Bốn hoán vị không tuần hoàn chính xác là những hoán vị thu được bằng cách liên tục đặt giá trị lớn nhất còn lại ở hai đầu:`[1,2,3]`,`[2,1,3]`,`[3,1,2]`,`[3,2,1]`. 

Hai hoán vị còn lại tạo ra chu trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Máy tính`n!`yêu cầu một phép nhân cho mỗi số nguyên từ`2`ĐẾN`n`| 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`n ≤ 10^6`, một triệu phép nhân mô-đun vừa vặn thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ là không đổi và không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())

    fact = 1
    for i in range(2, n + 1):
        fact = (fact * i) % MOD

    trees = pow(2, n - 1, MOD)

    return str((fact - trees) % MOD)

# provided sample
assert run("4\n") == "16", "sample"

# minimum valid n
assert run("3\n") == "2", "minimum size"

# small manual case
assert run("5\n") == "104", "5! - 2^4 = 120 - 16"

# another small manual case
assert run("6\n") == "688", "720 - 32"

# large boundary case, just verify execution and range
ans = int(run("1000000\n"))
assert 0 <= ans < MOD
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`2`| Đầu vào hợp lệ nhỏ nhất | 
|`4`|`16`| Mẫu chính thức | 
|`5`|`104`| Tính toán giai thừa và công suất đúng | 
|`6`|`688`| Xác minh thủ công bổ sung | 
|`1000000`| Giá trị modulo hợp lệ | Hiệu suất ở mức hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào hợp lệ nhỏ nhất:```
3
```Thuật toán tính toán$$3! - 2^2 = 6 - 4 = 2.$$Đối số đếm vẫn được áp dụng. Có chính xác bốn hoán vị tạo cây và hai hoán vị tuần hoàn. Không cần xử lý đặc biệt. 

Hãy xem xét trường hợp phép trừ sẽ trở thành modulo âm`MOD`. Đối với các giá trị lớn hơn của`n`, cả hai`n! mod MOD`Và`2^{n-1} mod MOD`là số dư chứ không phải số ban đầu. Điều đó hoàn toàn có thể xảy ra$$(n! \bmod MOD) < (2^{n-1} \bmod MOD).$$sử dụng```
(fact - trees) % MOD
```tự động chuyển đổi kết quả thành lớp dư lượng chính xác, tránh lỗi số học mô-đun phổ biến. 

Hãy xem xét đầu vào tối đa:```
1000000
```Thuật toán thực hiện một triệu phép nhân mô-đun và một phép lũy thừa mô-đun. Không có mảng tỷ lệ thuận với`n`được phân bổ và không sử dụng đệ quy. Giải pháp vẫn thoải mái trong cả giới hạn bộ nhớ và thời gian.
