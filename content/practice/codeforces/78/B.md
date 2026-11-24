---
title: "CF 78B - Trứng Phục Sinh"
description: "Chúng ta cần xây dựng một dãy màu tròn cho n quả trứng. Có chính xác bảy màu có sẵn: R, O, Y, G, B, I, V Hai điều kiện phải được đáp ứng đồng thời. Đầu tiên, mỗi màu phải xuất hiện ít nhất một lần."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 78
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 70 (Div. 2)"
rating: 1200
weight: 78
solve_time_s: 119
verified: false
draft: false
---

[CF 78B - Trứng Phục sinh](https://codeforces.com/problemset/problem/78/B) 

**Đánh giá:** 1200 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 59 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một chuỗi màu sắc hình tròn cho`n`trứng. Có chính xác bảy màu có sẵn:`R, O, Y, G, B, I, V`Hai điều kiện phải được giữ đồng thời. 

Đầu tiên, mỗi màu phải xuất hiện ít nhất một lần. 

Thứ hai, mỗi khối bốn quả trứng liên tiếp trên vòng tròn phải có bốn màu riêng biệt. Vì trứng tạo thành một vòng tròn nên trình tự sẽ bao bọc xung quanh. Những quả trứng cuối cùng và những quả trứng đầu tiên cũng là hàng xóm của nhau. 

Đầu vào chỉ là một số nguyên`n`, số lượng trứng Đầu ra là bất kỳ chuỗi màu có độ dài hợp lệ nào`n`. 

Những hạn chế là rất nhỏ.`n`nhiều nhất là`100`, do đó, ngay cả những cách tiếp cận khá kém hiệu quả cũng sẽ hoạt động thoải mái trong thời gian giới hạn. Về mặt lý thuyết, một giải pháp quay lui mạnh mẽ đối với tất cả các phép gán màu là có thể thực hiện được với giới hạn nhỏ như vậy, nhưng hệ số phân nhánh vẫn đủ lớn nên nó trở nên lộn xộn và không cần thiết. Vấn đề thực sự mang tính xây dựng, có một mô hình đơn giản luôn hoạt động. 

Phần khó khăn là điều kiện vòng tròn. Một chuỗi hoạt động tuyến tính có thể bị lỗi sau khi quấn quanh. Ví dụ:```
ROYGBIVR
```có vẻ ổn nếu chỉ được kiểm tra từ trái sang phải, nhưng ba ký tự cuối cùng cộng với dạng đầu tiên:```
IVRR
```lặp đi lặp lại`R`. 

Một lỗi phổ biến khác là sử dụng cả bảy màu một lần rồi lặp lại từ đầu:```
ROYGBIVROY...
```Điều này không thành công vì một số cửa sổ có bốn cửa sổ chứa các màu lặp lại gần điểm nối. 

Ví dụ, với`n = 8`:```
ROYGBIVR
```bốn quả trứng liên tiếp qua ranh giới là:```
V R R O
```Hai`R`s xuất hiện. 

Quan sát quan trọng là chúng ta không thực sự cần tất cả bảy màu trong mọi cửa sổ cục bộ. Chúng ta chỉ cần mỗi nhóm bốn quả trứng liên tiếp được phân biệt theo từng cặp. Hậu tố lặp lại được lựa chọn cẩn thận có thể đáp ứng điều kiện đó vô thời hạn. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử gán một trong bảy màu cho mỗi quả trứng trong khi kiểm tra xem chuỗi từng phần hiện tại có vi phạm quy tắc "bốn màu riêng biệt liên tiếp" hay không. Cuối cùng, nó cũng sẽ xác minh rằng tất cả bảy màu đã được sử dụng và các cửa sổ bao quanh hình tròn là hợp lệ. 

Điều này hiệu quả vì các ràng buộc rất nhỏ nhưng không gian tìm kiếm vẫn theo cấp số nhân. Trong trường hợp xấu nhất chúng ta khám phá đại khái`7^n`khả năng, lớn về mặt thiên văn ngay cả đối với người vừa phải`n`. Việc cắt tỉa có ích, nhưng cách tiếp cận này vẫn phức tạp một cách không cần thiết đối với một vấn đề có mục đích mang tính xây dựng. 

Cái nhìn sâu sắc thực sự đến từ việc nghiên cứu tình trạng của bốn quả trứng liên tiếp. Nếu chúng ta đã có một mẫu lặp lại hợp lệ trong đó mỗi khối bốn liền kề chứa các chữ cái khác nhau, thì việc lặp lại mẫu đó mãi mãi vẫn hợp lệ. 

Trình tự cầu vồng tiêu chuẩn:```
ROYGBIV
```đã thỏa mãn điều kiện rồi. Vấn đề xuất hiện khi chúng ta tiếp tục lặp lại từ đầu vì sự chồng chéo hình tròn tạo ra các bản sao. 

Thay vì lặp lại toàn bộ mẫu bảy chữ cái, chúng tôi chỉ lặp lại một hậu tố duy trì thuộc tính bốn khác biệt. Trình tự:```
GBIV
```có thể lặp lại mãi mãi bởi vì mỗi khối bốn liên tiếp bên trong sự lặp lại đó chính xác là một phép quay nào đó của các chữ cái riêng biệt. 

Vậy việc xây dựng trở thành: 

1. Bắt đầu với`"ROYGBIV"`để đảm bảo tất cả bảy màu xuất hiện. 
2. Đối với mỗi vị trí bổ sung, hãy thêm các ký tự theo chu kỳ từ`"GBIV"`. 

Ví dụ cho`n = 10`:```
ROYGBIVGBI
```Cứ bốn ký tự liên tiếp vẫn có sự khác biệt, bao gồm cả đường ranh giới hình tròn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(7^n) | O(n) | Về mặt khái niệm quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo chuỗi cơ sở:```
ROYGBIV
```Điều này ngay lập tức đảm bảo rằng mỗi màu sẽ xuất hiện ít nhất một lần. 

1. Tính xem cần thêm bao nhiêu ký tự.```
extra = n - 7
```1. Sử dụng mẫu lặp lại:```
GBIV
```Mẫu này được chọn vì mỗi khối bốn ký tự liên tiếp bên trong các bản sao lặp lại vẫn có sự khác biệt. 

1. Nối các ký tự từ`"GBIV"`theo chu kỳ cho đến khi tổng chiều dài trở thành`n`. 

Nếu như`extra = 5`, chúng tôi nối thêm:```
GBIVG
```1. In chuỗi cuối cùng. 

### Tại sao nó hoạt động 

Bảy ký tự đầu tiên chứa tất cả bảy màu đúng một lần, vì vậy điều kiện đầu tiên được thỏa mãn ngay lập tức. 

Hậu tố lặp lại`"GBIV"`có độ dài bốn và cả bốn ký tự đều khác biệt. Bất kỳ khối bốn chữ cái liên tiếp nào bên trong các bản sao lặp lại của mẫu này chỉ là sự dịch chuyển theo chu kỳ của bốn chữ cái đó, do đó không có sự lặp lại nào xuất hiện. 

Ranh giới giữa`"ROYGBIV"`và hậu tố được thêm vào cũng có tác dụng vì một số ký tự cuối cùng của cơ sở đã căn chỉnh một cách tự nhiên với`"GBIV"`. 

Kết quả là mỗi cửa sổ hình tròn của bốn quả trứng liên tiếp đều chứa bốn màu riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    base = "ROYGBIV"
    extra_pattern = "GBIV"
    
    ans = base
    
    for i in range(n - 7):
        ans += extra_pattern[i % 4]
    
    print(ans)

solve()
```Giải pháp bắt đầu với chuỗi bảy màu bắt buộc. Phần đó đã được khắc phục vì sự cố yêu cầu mọi màu phải xuất hiện ít nhất một lần. 

Các vị trí còn lại được lấp đầy bằng cách sử dụng`"GBIV"`theo chu kỳ. Hoạt động modulo xử lý việc gói bên trong mẫu bốn ký tự đó. 

Một chi tiết tinh tế là chúng tôi chỉ thêm vào sau bảy ký tự đầu tiên. Việc lặp lại toàn bộ cầu vồng bảy chữ sẽ phá vỡ tình trạng hình tròn gần ranh giới. 

Một chi tiết quan trọng khác là độ dài câu trả lời phải chính xác`n`. Vì vòng lặp chạy`n - 7`lần, độ dài cuối cùng là chính xác mà không cần kiểm tra thêm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
```Các bước thi công: 

| Bước | Câu trả lời hiện tại | Đã thêm nhân vật | 
| --- | --- | --- | 
| Ban đầu | ROYGBIV | - | 
| 1 | ROYGBIVG | G | 

Đầu ra cuối cùng:```
ROYGBIVG
```Ví dụ này cho thấy trường hợp nhỏ nhất trong đó một ký tự phụ phải được thêm vào. Đã thêm`G`tiếp tục cấu trúc lặp lại an toàn mà không phá vỡ bất kỳ cửa sổ bốn ký tự nào. 

### Ví dụ 2 

đầu vào:```
12
```Các bước thi công: 

| Bước | Câu trả lời hiện tại | Đã thêm nhân vật | 
| --- | --- | --- | 
| Ban đầu | ROYGBIV | - | 
| 1 | ROYGBIVG | G | 
| 2 | ROYGBIVGB | B | 
| 3 | ROYGBIVGBI | Tôi | 
| 4 | ROYGBIVGBIV | V | 
| 5 | ROYGBIVGBIVG | G | 

Đầu ra cuối cùng:```
ROYGBIVGBIVG
```Dấu vết này cho thấy sự lặp lại theo chu kỳ của`"GBIV"`. Mỗi nhóm bốn ký tự liên tiếp vẫn có tính phân biệt vì bản thân hậu tố là một chu kỳ gồm bốn ký tự gồm các chữ cái riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi nối thêm chính xác`n`nhân vật tổng thể | 
| Không gian | O(n) | Chuỗi đầu ra lưu trữ`n`nhân vật | 

Với`n ≤ 100`, giải pháp này dễ dàng thực hiện trong cả giới hạn thời gian và bộ nhớ. Việc triển khai chỉ thực hiện các thao tác chuỗi đơn giản và một vòng lặp đơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())
    
    base = "ROYGBIV"
    extra_pattern = "GBIV"
    
    ans = base
    
    for i in range(n - 7):
        ans += extra_pattern[i % 4]
    
    print(ans)

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    
    solve()
    
    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("8\n") == "ROYGBIVG", "sample 1"

# minimum size
assert run("7\n") == "ROYGBIV", "minimum n"

# one full extra cycle
assert run("11\n") == "ROYGBIVGBIV", "full GBIV repetition"

# off-by-one after cycle restart
assert run("12\n") == "ROYGBIVGBIVG", "cycle restart"

# maximum size length check
res = run("100\n")
assert len(res) == 100, "maximum size"

# all colors appear
for c in "ROYGBIV":
    assert c in res, "missing color"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`|`ROYGBIV`| Kích thước hợp lệ tối thiểu | 
|`8`|`ROYGBIVG`| Ký tự được thêm đầu tiên | 
|`11`|`ROYGBIVGBIV`| Chu kỳ hậu tố đầy đủ chính xác | 
|`12`|`ROYGBIVGBIVG`| Khởi động lại modulo đúng | 
|`100`| Độ dài 100 chuỗi hợp lệ | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào tối thiểu:```
7
```Các đầu ra thuật toán:```
ROYGBIV
```Mỗi màu xuất hiện một lần và mỗi nhóm bốn quả trứng liên tiếp đều chứa các màu riêng biệt vì tất cả các ký tự đều là duy nhất. 

Bây giờ hãy xem xét phần mở rộng không cần thiết đầu tiên:```
8
```Việc xây dựng trở thành:```
ROYGBIVG
```Kiểm tra cửa sổ tròn:```
ROYG
OYGB
YGBI
GBIV
BIVG
IVGR
VGRO
GROY
```Tất cả các cửa sổ bốn ký tự đều chứa các chữ cái riêng biệt. 

Một sự lặp lại ngây thơ như:```
ROYGBIVR
```sẽ thất bại vì cửa sổ tròn:```
IVRR
```chứa lặp đi lặp lại`R`. 

Một trường hợp tinh vi khác là khi mẫu được nối thêm bao quanh:```
12
```Đầu ra:```
ROYGBIVGBIVG
```Hậu tố lặp lại như sau:```
GBIVG
```Ngay cả sau khi khởi động lại lúc`G`, mỗi khối bốn ký tự liên tiếp vẫn là một hoán vị của`G, B, I, V`, do đó điều kiện tiếp tục được giữ.
