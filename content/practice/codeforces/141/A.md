---
title: "CF 141A - Truyện cười vui nhộn"
description: "Chúng ta được cấp ba chuỗi chữ hoa. Hai chuỗi đầu tiên là những cái tên được viết trên cửa, và chuỗi thứ ba là chồng chữ cái được tìm thấy vào sáng hôm sau sau khi ai đó trộn lẫn mọi thứ lại với nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 141
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 101 (Div. 2)"
rating: 800
weight: 141
solve_time_s: 91
verified: true
draft: false
---

[CF 141A - Trò đùa thú vị](https://codeforces.com/problemset/problem/141/A) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp ba chuỗi chữ hoa. Hai chuỗi đầu tiên là những cái tên được viết trên cửa, và chuỗi thứ ba là chồng chữ cái được tìm thấy vào sáng hôm sau sau khi ai đó trộn lẫn mọi thứ lại với nhau. 

Nhiệm vụ là kiểm tra xem chuỗi thứ ba có chứa các chữ cái giống hệt như hai chuỗi đầu tiên cộng lại hay không. Mỗi nhân vật đều quan trọng, kể cả số lần nó xuất hiện. Nếu tên kết hợp chứa ba`'A'`các ký tự thì cọc cũng phải chứa đúng ba ký tự`'A'`nhân vật. Không được phép thiếu chữ cái nào và không được phép thêm chữ cái nào. 

Độ dài tối đa của mỗi chuỗi chỉ là 100, do đó, ngay cả những hoạt động khá kém hiệu quả cũng hoàn toàn an toàn. Một giải pháp bậc hai vẫn sẽ chạy ngay lập tức. Điều này cho phép chúng tôi tự do lựa chọn cách triển khai rõ ràng nhất thay vì lo lắng về việc tối ưu hóa vi mô. 

Mối nguy hiểm chính là quên rằng tần số ký tự quan trọng chứ không chỉ sự tồn tại của ký tự. Một giải pháp bất cẩn chỉ có thể kiểm tra xem liệu mọi ký tự riêng biệt có xuất hiện ở đâu đó trong đống hay không. 

Hãy xem xét đầu vào này:```
AAB
C
ABAC
```Đầu ra đúng là:```
YES
```Tên kết hợp chứa hai`'A'`, một`'B'`, và một`'C'`. Cọc chứa số lượng chính xác như nhau. 

Bây giờ hãy xem trường hợp này:```
AAB
C
ABC
```Đầu ra đúng là:```
NO
```Kiểm tra dựa trên sự hiện diện ngây thơ sẽ chấp nhận điều này một cách không chính xác vì tất cả các chữ cái cần thiết đều xuất hiện ít nhất một lần. Vấn đề là cái đó`'A'`bị thiếu. 

Một sai lầm dễ mắc phải khác là quên từ chối những lá thư thừa.```
ABC
DEF
ABCDEFX
```Đầu ra đúng là:```
NO
```Tất cả các chữ cái bắt buộc đều có mặt, nhưng phần bổ sung`'X'`làm cho cọc không hợp lệ. 

Trường hợp cạnh cuối cùng là khi độ dài đã không đồng ý.```
AB
CD
ABC
```Đầu ra đúng là:```
NO
```Tên kết hợp cần bốn chữ cái, nhưng chồng chỉ có ba. Lời giải đúng phải loại bỏ ngay hoặc phát hiện sự không phù hợp thông qua việc đếm. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra mọi hoán vị có thể có của các tên kết hợp và kiểm tra xem một trong số chúng có bằng chuỗi cọc hay không. Điều này có tác dụng vì bài toán thực sự hỏi liệu chồng dữ liệu có thể được sắp xếp lại thành các chữ cái gốc hay không. 

Nếu chiều dài kết hợp là`n`, có`n!`hoán vị. Ngay cả đối với`n = 20`, điều này trở nên lớn đến mức không thể tưởng tượng được và giới hạn thực tế có thể đạt tới 200 ký tự. Thuật toán thời gian giai thừa là hoàn toàn không khả thi. 

Điều quan trọng cần lưu ý là thứ tự các ký tự không hề quan trọng chút nào. Cọc có thể xáo trộn tùy ý. Hai chuỗi tương đương nhau khi được sắp xếp lại tùy ý khi và chỉ khi mọi ký tự xuất hiện cùng số lần trong cả hai. 

Điều đó làm giảm vấn đề so sánh tần số ký tự. 

Có hai cách phổ biến để làm điều này. Một là đếm ký tự bằng cách sử dụng mảng hoặc bản đồ băm. Cách khác là sắp xếp cả hai chuỗi và so sánh kết quả được sắp xếp. Vì kích thước đầu vào rất nhỏ nên cả hai cách tiếp cận đều được chấp nhận một cách thoải mái. 

Một giải pháp dựa trên sắp xếp đặc biệt ngắn gọn. Chúng ta nối hai chuỗi đầu tiên, sắp xếp kết quả, sắp xếp chuỗi chồng và so sánh trực tiếp. Nếu các phiên bản được sắp xếp khớp nhau thì các tập ký tự sẽ giống hệt nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp và so sánh | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tên khách, tên chủ nhà, xâu cọc. 
2. Nối hai chuỗi đầu tiên vì chúng cùng nhau đại diện cho tất cả các chữ cái nên tồn tại. 
3. Sắp xếp chuỗi được nối. Sắp xếp các nhóm chữ cái giống nhau lại với nhau, giúp việc so sánh tần số trở nên đơn giản. 
4. Sắp xếp chuỗi cọc vì lý do tương tự. 
5. So sánh hai chuỗi đã được sắp xếp. 

Nếu chúng giống hệt nhau thì mỗi ký tự xuất hiện cùng số lần trong cả hai chuỗi, vì vậy hãy in`"YES"`. 
6. Nếu không, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Việc sắp xếp sẽ chuyển đổi từng chuỗi thành dạng chuẩn trong đó các tập hợp ký tự giống hệt nhau trở thành các chuỗi giống hệt nhau. Nếu hai chuỗi chứa chính xác các ký tự giống nhau có cùng tần số thì các phiên bản được sắp xếp của chúng phải khớp với từng ký tự. Nếu thậm chí một số ký tự khác nhau thì các chuỗi được sắp xếp sẽ khác nhau ở đâu đó. Điều này đảm bảo việc so sánh là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

guest = input().strip()
host = input().strip()
pile = input().strip()

combined = guest + host

if sorted(combined) == sorted(pile):
    print("YES")
else:
    print("NO")
```Bước đầu tiên đọc ba chuỗi đầu vào và xóa các ký tự dòng mới ở cuối bằng cách sử dụng`strip()`. 

Biến`combined`lưu trữ tất cả các chữ cái tồn tại nếu không ai thêm hoặc xóa bất cứ thứ gì. 

Cuộc gọi đến`sorted()`chuyển đổi từng chuỗi thành một danh sách các ký tự được sắp xếp. Python so sánh các danh sách theo từ điển, do đó sự bình đẳng chỉ thành công khi cả hai danh sách đều chứa chính xác các ký tự giống nhau trong cùng một số lượng. 

Một chi tiết tinh tế là chỉ riêng việc sắp xếp đã tự động xử lý các điểm không khớp về độ dài. Nếu một chuỗi có thêm hoặc thiếu chữ cái thì danh sách được sắp xếp không thể bằng nhau. 

Quá trình triển khai diễn ra trong thời gian ngắn vì tính năng sắp xếp tích hợp của Python xử lý tất cả các công việc nặng nhọc một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
SANTACLAUS
DEDMOROZ
SANTAMOROZDEDCLAUS
```| Bước | Giá trị | 
| --- | --- | 
| Khách |`SANTACLAUS`| 
| Máy chủ |`DEDMOROZ`| 
| Kết hợp |`SANTACLAUSDEDMOROZ`| 
| Cọc |`SANTAMOROZDEDCLAUS`| 
| Sắp xếp Kết hợp |`AACDDELMNOORRSSTTUZ`| 
| Cọc được sắp xếp |`AACDDELMNOORRSSTTUZ`| 
| Kết quả |`YES`| 

Dấu vết này cho thấy sự bất biến trung tâm của thuật toán. Sau khi loại bỏ thứ tự thông qua việc sắp xếp, cả hai chuỗi sẽ trở nên giống hệt nhau vì chúng chứa tần số ký tự giống hệt nhau. 

### Ví dụ 2 

đầu vào:```
AAB
C
ABC
```| Bước | Giá trị | 
| --- | --- | 
| Khách |`AAB`| 
| Máy chủ |`C`| 
| Kết hợp |`AABC`| 
| Cọc |`ABC`| 
| Sắp xếp Kết hợp |`AABC`| 
| Cọc được sắp xếp |`ABC`| 
| Kết quả |`NO`| 

Ví dụ này chứng minh tại sao tần số lại quan trọng. Đống dữ liệu chứa tất cả các chữ cái riêng biệt được yêu cầu, nhưng chỉ có một`'A'`thay vì hai. Việc sắp xếp sẽ bộc lộ sự không phù hợp ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp thống trị thời gian chạy | 
| Không gian | O(n) | Danh sách ký tự được sắp xếp yêu cầu bộ nhớ bổ sung | 

Đây,`n`là tổng số ký tự có liên quan. Vì độ dài tối đa có thể chỉ là 200 nên thời gian chạy rất nhỏ so với giới hạn. Giải pháp dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    guest = input().strip()
    host = input().strip()
    pile = input().strip()

    combined = guest + host

    if sorted(combined) == sorted(pile):
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run(
    "SANTACLAUS\nDEDMOROZ\nSANTAMOROZDEDCLAUS\n"
) == "YES\n", "sample 1"

# missing repeated character
assert run(
    "AAB\nC\nABC\n"
) == "NO\n", "missing repeated character"

# extra character
assert run(
    "ABC\nDEF\nABCDEFX\n"
) == "NO\n", "extra character"

# minimum size valid
assert run(
    "A\nB\nAB\n"
) == "YES\n", "minimum valid case"

# all characters equal
assert run(
    "AAA\nAAAA\nAAAAAAA\n"
) == "YES\n", "all equal characters"

# maximum-style boundary case
assert run(
    "A" * 100 + "\n" +
    "B" * 100 + "\n" +
    "A" * 100 + "B" * 100 + "\n"
) == "YES\n", "large boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`AAB / C / ABC`|`NO`| Phát hiện các ký tự trùng lặp bị thiếu | 
|`ABC / DEF / ABCDEFX`|`NO`| Từ chối thêm thư | 
|`A / B / AB`|`YES`| Đầu vào hợp lệ nhỏ nhất | 
|`AAA / AAAA / AAAAAAA`|`YES`| Xử lý các ký tự giống hệt nhau lặp đi lặp lại | 
| 100`'A'`và 100`'B'`|`YES`| Đầu vào có kích thước ranh giới | 

## Vỏ cạnh 

Hãy xem xét trường hợp không khớp ký tự lặp lại:```
AAB
C
ABC
```Thuật toán nối hai chuỗi đầu tiên thành`"AABC"`. Sau khi sắp xếp, ta được:```
AABC
```Đống sắp xếp để:```
ABC
```Các chuỗi khác nhau bởi vì một`'A'`bị thiếu. Thuật toán in chính xác`"NO"`. 

Bây giờ hãy xem xét trường hợp thêm chữ cái:```
ABC
DEF
ABCDEFX
```Chuỗi kết hợp sắp xếp thành:```
ABCDEF
```Đống sắp xếp để:```
ABCDEFX
```phần bổ sung`'X'`vẫn hiển thị sau khi sắp xếp, do đó việc so sánh không thành công và câu trả lời là`"NO"`. 

Cuối cùng, hãy xem xét sự không khớp về độ dài thuần túy:```
AB
CD
ABC
```Các tên kết hợp chứa bốn ký tự, trong khi chồng tên chỉ chứa ba ký tự. Sau khi sắp xếp:```
Combined: ABCD
Pile:     ABC
```Các chuỗi được sắp xếp không thể khớp nhau vì độ dài của chúng khác nhau. Thuật toán tự động từ chối đầu vào mà không cần bất kỳ logic trường hợp đặc biệt nào.
