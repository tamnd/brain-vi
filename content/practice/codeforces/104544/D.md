---
title: "CF 104544D - Thêm vài đô la"
description: "Chúng ta được yêu cầu quyết định xem Yazan có thể đặt hai món ăn với giá rẻ như thế nào, một món cho mỗi người bạn, với hai điều kiện ràng buộc. Món đầu tiên có giá a và ít nhất phải bằng x. Món thứ hai có giá b và được gắn với tổng hóa đơn: ít nhất phải bằng y% tổng giá a + b."
date: "2026-06-30T09:02:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104544
codeforces_index: "D"
codeforces_contest_name: "Aleppo Collegiate Programming Contest 2023 V.2"
rating: 0
weight: 104544
solve_time_s: 91
verified: false
draft: false
---

[CF 104544D - Để kiếm thêm vài đô la](https://codeforces.com/problemset/problem/104544/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu quyết định xem Yazan có thể đặt hai món ăn với giá rẻ như thế nào, một món cho mỗi người bạn, với hai điều kiện ràng buộc. 

Món đầu tiên có giá`a`và ít nhất phải có`x`. Món thứ hai có giá`b`và được gắn với tổng hóa đơn: ít nhất phải bằng`y%`của chi phí kết hợp`a + b`. 

Vì vậy, cấu trúc là hình tròn. Giá trị của`b`phụ thuộc vào tổng số, nhưng tổng số phụ thuộc vào`b`. Mục đích là chọn số nguyên`a`Và`b`thỏa mãn cả hai điều kiện đồng thời giảm thiểu`a + b`. 

Đầu vào cung cấp nhiều trường hợp kiểm thử độc lập, mỗi trường hợp có giới hạn`x`Và`y`. Đối với mỗi trường hợp thử nghiệm, chúng ta phải đưa ra tổng chi phí tối thiểu có thể hoặc`-1`nếu không có cặp hợp lệ tồn tại. 

Khó khăn chính là hạn chế tự tham chiếu về`b`. Một nỗ lực ngây thơ sẽ thử tất cả các cặp`(a, b)`lên đến giới hạn nào đó, nhưng không có giới hạn trên hữu hạn rõ ràng cho cả hai biến. Tuy nhiên, vì cả hai ràng buộc này chỉ làm tăng chi phí khi giá trị tăng lên, nên mọi giải pháp tối ưu sẽ tồn tại ở các giá trị khả thi nhỏ nhất, điều này cho thấy rõ ràng rằng chúng ta không nên khám phá phạm vi lớn. 

Trường hợp cạnh tinh tế xuất hiện khi`y = 100`. Trong trường hợp đó, ràng buộc thứ hai trở thành`b ≥ a + b`, lực nào`0 ≥ a`, không thể được vì`a ≥ x ≥ 1`. Vì vậy, mọi trường hợp thử nghiệm với`y = 100`ngay lập tức là không thể thực hiện được. 

Một cạm bẫy tiềm ẩn khác là xử lý giới hạn phần trăm không chính xác như`b ≥ (y/100) * a`, bỏ qua rằng tỷ lệ phần trăm là tổng số, không chỉ món ăn đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các cặp số nguyên`(a, b)`lên tới giới hạn tối đa nào đó, kiểm tra xem cả hai ràng buộc có giữ nguyên hay không và theo dõi tổng tối thiểu. Mặc dù đúng nhưng điều này không được xác định rõ ràng về mặt tính toán vì không có giới hạn trên tự nhiên trên`a`Và`b`và ngay cả khi chúng tôi áp đặt một giá trị như 10^5 thì nó sẽ quá chậm ở O(N^2). 

Quan sát quan trọng là ràng buộc thứ hai có thể được viết lại để loại bỏ sự phụ thuộc vòng tròn. Bắt đầu từ:`b ≥ y/100 * (a + b)`chúng tôi sắp xếp lại:`100b ≥ y(a + b)`

`100b ≥ ya + yb`

`(100 - y)b ≥ ya`Nếu như`y < 100`, điều này trở thành:`b ≥ (y * a) / (100 - y)`Vì vậy để cố định`a`, hợp lệ nhỏ nhất`b`được xác định trực tiếp. Điều này loại bỏ hoàn toàn chu kỳ phụ thuộc. 

Bây giờ vấn đề trở nên giảm thiểu:`a + b(a) = a + ceil(y * a / (100 - y))`Vì cả hai số hạng đều tăng tuyến tính với`a`, tổng tăng đơn điệu khi`a`tăng lên. Vì vậy, giải pháp tối ưu luôn sử dụng kích thước nhỏ nhất có thể.`a`, đó là`a = x`. 

Điều này làm giảm toàn bộ vấn đề thành tính toán trực tiếp, ngoại trừ trường hợp đặc biệt`y = 100`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực tàn bạo (a, b) | O(N²) hoặc không giới hạn | O(1) | Quá chậm / Không xác định | 
| Giảm công thức | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Phương pháp tối ưu 

1. Kiểm tra xem`y == 100`. Nếu vậy thì xuất ngay`-1`. Điều này là do lực cản`b ≥ a + b`, điều này là không thể đối với bất kỳ tích cực nào`a`. 
2. Ngược lại hãy tính giá trị hợp lệ tối thiểu`a`, điều đó luôn luôn là`a = x`. Tăng dần`a`chỉ làm tăng cả tổng chi phí và yêu cầu`b`, vì vậy không có lựa chọn nào lớn hơn có thể cải thiện câu trả lời. 
3. Viết lại ràng buộc thành giới hạn dưới trực tiếp cho`b`:`b ≥ (y * a) / (100 - y)`4. Tính số nguyên nhỏ nhất`b`thỏa mãn giới hạn này bằng cách sử dụng phép chia trần. 
5. Đầu ra`a + b`. 

### Tại sao nó hoạt động 

Việc chuyển đổi loại bỏ vòng lặp phụ thuộc giữa`a`,`b`, và tổng số tiền. Đối với bất kỳ cố định`a`, ràng buộc xác định duy nhất khả năng nhỏ nhất khả thi`b`. Vì cả hai`a`Và`b(a)`là các hàm không giảm của`a`, hàm mục tiêu`a + b(a)`cũng không giảm nên mức tối thiểu của nó phải xảy ra ở mức nhỏ nhất khả thi`a`. Điều này đảm bảo rằng việc hạn chế sự chú ý đến`a = x`không loại trừ giải pháp tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, y = map(int, input().split())
    
    if y == 100:
        print(-1)
        continue
    
    a = x
    numerator = y * a
    denominator = 100 - y
    
    b = (numerator + denominator - 1) // denominator
    print(a + b)
```Mã trực tiếp theo công thức dẫn xuất. Sự phân nhánh duy nhất là`y == 100`trường hợp, xử lý điều kiện không thể. 

Bộ phận trần`(numerator + denominator - 1) // denominator`là rất quan trọng; sử dụng số học dấu phẩy động sẽ có nguy cơ xảy ra lỗi chính xác, đặc biệt vì các ràng buộc chặt chẽ và cần có độ chính xác số nguyên chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
x = 2, y = 40
```Chúng tôi tính toán`a = 2`. 

| Bước | một | tử số = y·a | mẫu số | b = trần(n/d) | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | - | - | - | - | 
| tính toán | 2 | 80 | 60 | 2 | 4 | 

Đây`b = ceil(80/60) = 2`, vậy tổng chi phí là`4`. 

Điều này chứng tỏ ràng buộc thứ hai ràng buộc chặt chẽ và buộc`b`tỷ lệ thuận với`a`. 

### Ví dụ 2 

đầu vào:```
x = 1, y = 10
```| Bước | một | tử số | mẫu số | b | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | - | - | - | - | 
| tính toán | 1 | 10 | 90 | 1 | 2 | 

chúng tôi nhận được`b = ceil(10/90) = 1`, tổng cộng`2`. 

Điều này cho thấy khi`y`nhỏ, giới hạn phần trăm yếu và cả hai món ăn đều có thể ở mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được giải bằng một số phép tính số học không đổi | 
| Không gian | O(1) | Chỉ một số số nguyên được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì`t ≤ 100`và mỗi trường hợp là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        x, y = map(int, input().split())
        if y == 100:
            out.append("-1")
            continue
        a = x
        b = (y * a + (100 - y) - 1) // (100 - y)
        out.append(str(a + b))
    return "\n".join(out)

# provided samples
assert run("5\n2 40\n4 60\n2 100\n3 50\n1 10\n") == "4\n10\n-1\n6\n2"

# custom cases
assert run("1\n1 99\n") == "100", "high percentage edge"
assert run("1\n100 1\n") == "101", "large x small y"
assert run("1\n5 100\n") == "-1", "impossible case"
assert run("1\n10 50\n") == "20", "balanced ratio"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 99`|`100`| phần trăm gần như không thể ép lớn b | 
|`100 1`|`101`| chi phí cơ bản lớn chiếm ưu thế | 
|`5 100`|`-1`| trường hợp ràng buộc không khả thi | 
|`10 50`|`20`| tỷ lệ trung điểm đối xứng | 

## Vỏ cạnh 

### Trường hợp`y = 100`đầu vào:```
x = 5, y = 100
```Ràng buộc trở thành:`b ≥ a + b`Trừ`b`cho`0 ≥ a`, điều này mâu thuẫn`a ≥ 5`. Thuật toán ngay lập tức trở lại`-1`trước bất kỳ tính toán nào. 

### Bé nhỏ`y`giá trị 

đầu vào:```
x = 1, y = 1
```Ở đây giới hạn phần trăm là cực kỳ yếu. Thuật toán tính toán:`b = ceil(1 / 99) = 1`, vậy tổng số là`2`. Tối thiểu`a`sự lựa chọn vẫn tối ưu vì tăng`a`chỉ làm xấu đi cả tử số và tổng số. 

### Lớn`x`đầu vào:```
x = 100, y = 50
```Thuật toán sửa lỗi`a = 100`, sau đó tính`b = ceil(5000/50) = 100`, cho tổng cộng`200`. Bất kỳ lớn hơn`a`sẽ chỉ tăng cả hai giá trị theo tỷ lệ, xác nhận rằng lựa chọn biên luôn tối ưu.
