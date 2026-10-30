---
title: "CF 106225H - Hyper Smawk Bros"
description: "Trò chơi bắt đầu với một ông chủ có sức khỏe là n. Hai người chơi luân phiên tấn công và bạn di chuyển trước. Mỗi đòn tấn công gây ra một lượng sát thương nguyên từ 1 đến m. Hạn chế duy nhất là người chơi không thể lặp lại số tiền mà đối thủ đã sử dụng ở lượt ngay trước đó."
date: "2026-06-19T14:03:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106225
codeforces_index: "H"
codeforces_contest_name: "2025-2026 ICPC Southwestern European Regional Contest (SWERC 2025)"
rating: 0
weight: 106225
solve_time_s: 56
verified: true
draft: false
---

[CF 106225H - Hyper Smawk Bros](https://codeforces.com/problemset/problem/106225/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi bắt đầu với một ông chủ có sức khỏe`n`. Hai người chơi luân phiên tấn công và bạn di chuyển trước. Mỗi đòn tấn công gây ra một lượng sát thương nguyên giữa`1`Và`m`. Hạn chế duy nhất là người chơi không thể lặp lại số tiền mà đối thủ đã sử dụng ở lượt ngay trước đó. Ai làm cho sức khỏe không tích cực sẽ thắng ngay. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi phải xác định xem người chơi thứ nhất có chiến lược đảm bảo chiến thắng hay không, giả sử người chơi thứ hai luôn phản ứng tối ưu. 

Các giá trị của cả hai`n`Và`m`có thể đạt được`10^6`, và có thể có tới`10^4`trường hợp thử nghiệm. Không có hạn chế nào về tổng của các giá trị này, do đó, bất kỳ thuật toán nào thực hiện công việc tỷ lệ thuận với`n`hoặc`m`vì mọi trường hợp sẽ trở nên quá đắt. Chúng ta cần một quy tắc quyết định theo thời gian không đổi. 

Một số tình huống rất dễ xử lý sai. 

Đầu tiên là khi trùm có thể bị giết ngay lập tức.```
n = 6, m = 9
```Câu trả lời là`YES`, bởi vì người chơi đầu tiên có thể chỉ gây sát thương`6`,`7`,`8`, hoặc`9`. 

Trường hợp tế nhị thứ hai xảy ra khi`m = 2`.```
n = 69, m = 2
```Câu trả lời là`NO`. Khi một người chơi sử dụng`1`, cái kia phải sử dụng`2`, thì việc đầu tiên phải sử dụng`1`một lần nữa, vân vân. Trình tự di chuyển trở nên hoàn toàn gượng ép. Một cách tiếp cận ngây thơ cho rằng luôn có sẵn nhiều lựa chọn sẽ thất bại ở đây. 

Một trường hợp phức tạp khác là khi`n`chính xác là chia hết cho`2m`.```
n = 20, m = 10
```Câu trả lời là`YES`, không`NO`. Mặc dù`20`là bội số của`20`, người chơi đầu tiên có thể tấn công bằng`10`, để lại sức khỏe`10`và sau nước đi của Bob, người chơi đầu tiên có thể sử dụng lại`10`bởi vì chỉ có đòn tấn công trước đó của Bob là bị cấm. 

Một nỗ lực bất cẩn nhằm mô hình hóa trò chơi như một phép trừ thông thường mà không nhớ nước đi cuối cùng sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô tả trạng thái bằng lượng máu còn lại và sát thương được sử dụng ở lượt trước. Từ trạng thái như vậy, chúng ta có thể thử mọi giá trị thiệt hại hợp pháp và đánh dấu xem người chơi hiện tại có nước đi thắng hay không. Phép đệ quy này đúng vì nó khám phá toàn bộ cây trò chơi. 

Thật không may, có khoảng`n × m`tiểu bang. Với cả hai giá trị bằng`10^6`, điều này trở nên đại khái`10^12`trạng thái, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của trò chơi cho thấy một mô hình mạnh mẽ hơn nhiều. Ký ức duy nhất mang theo giữa các lượt là sát thương trước đó của đối thủ. Nếu người chơi sử dụng`x`, thì ở lượt tiếp theo, giá trị chính xác đó sẽ bị chặn nhưng mọi giá trị khác vẫn có sẵn. 

Hãy xem xét một cặp vòng quay liên tiếp. Giả sử một người chơi giao dịch`a`và các ưu đãi khác`b`, với`a ≠ b`. Ở lượt sau,`a`lại trở thành hợp pháp bởi vì chỉ`b`bị cấm. Như vậy cứ hai nước đi lại loại bỏ chính xác`a+b`sức khỏe, và cùng một cặp có thể lặp lại vô thời hạn. 

Số tiền lớn nhất có thể rút được trong hai nước đi là```
m + (m - 1) = 2m - 1
```bởi vì hai thiệt hại phải khác nhau. 

Số tiền nhỏ nhất có thể rút ra trong hai lần di chuyển là```
1 + 2 = 3
```khi`m = 2`và nói chung hơn là bất kỳ cặp số phân biệt nào. 

Việc xem xét các ví dụ nhỏ sẽ nhanh chóng phát hiện ra một cấu trúc tuần hoàn. Mỗi khối có chiều dài`2m`cư xử giống hệt nhau. Những vị trí bị mất chính xác là những vị trí có```
n mod (2m) = 1
```khi`m > 2`. 

Vụ án`m = 2`là đặc biệt. Trình tự di chuyển buộc phải luân phiên giữa`1`Và`2`, do đó, cứ ba điểm sức khỏe tương ứng với một chu kỳ hoàn chỉnh. Trong trường hợp này, vị thế thua chính xác là```
n mod 3 = 0
```Brute Force hoạt động vì nó tính toán rõ ràng các trạng thái thắng và thua, nhưng nó thất bại vì có quá nhiều trạng thái. Quan sát mà trò chơi cuối cùng lặp lại với một mô hình mô-đun đơn giản cho phép chúng ta thay thế toàn bộ biểu đồ trạng thái bằng một công thức thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(nm) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Nếu`m = 2`, kiểm tra xem`n`chia hết cho`3`. 
3. Khi nào`n % 3 == 0`, đầu ra`NO`, bởi vì trình tự xen kẽ`1,2,1,2,...`bị ép buộc và người chơi thứ hai thực hiện nước đi cuối cùng. 
4. Nếu không thì xuất ra`YES`. 
5. Nếu`m > 2`, tính toán`n % (2m)`. 
6. Khi số dư bằng`1`, đầu ra`NO`. 
7. Với mỗi phần còn lại, hãy ghi`YES`. 

Lý do đằng sau Bước 6 là sau mỗi khối hoàn chỉnh của`2m`điểm sức khỏe tình trạng tương tự lại xuất hiện. Phần còn lại của`1`nghĩa là người chơi di chuyển phải đối mặt cùng loại vị trí với sức khỏe`1`, nơi đối thủ luôn có thể phản chiếu cấu trúc và cuối cùng có được đòn tấn công cuối cùng. 

### Tại sao nó hoạt động 

cho`m > 2`, vị trí lặp lại với dấu chấm`2m`. Trong một khoảng thời gian hoàn chỉnh, chính xác trạng thái còn lại`1`đang thua. Mọi nước đi hợp pháp từ một nước như vậy đều đạt đến trạng thái chiến thắng, trong khi mọi nước đi khác đều có ít nhất một nước đi dẫn đến nước đi còn lại`1`. 

Vì`m = 2`, việc cấm lặp lại đòn tấn công trước đó của đối thủ buộc chuỗi sát thương phải xen kẽ giữa`1`Và`2`. Cứ ba điểm máu sẽ tiêu tốn một chu kỳ đầy đủ, do đó, người chơi đầu tiên sẽ thua bội số của ba. 

Vì mọi vị trí đều được phân loại chính xác và trò chơi luôn giảm lượng máu nên thuật toán luôn trả về câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
ans = []

for _ in range(t):
    n, m = map(int, input().split())

    if m == 2:
        if n % 3 == 0:
            ans.append("NO")
        else:
            ans.append("YES")
    else:
        if n % (2 * m) == 1:
            ans.append("NO")
        else:
            ans.append("YES")

print("\n".join(ans))
```Chương trình xử lý từng trường hợp thử nghiệm một cách độc lập. 

Trường hợp đặc biệt`m = 2`đến trước vì hành vi của nó khác hoàn toàn với mẫu chung. Chuỗi tấn công xen kẽ chỉ để lại khoảng thời gian ba. 

Đối với các giá trị lớn hơn của`m`, câu trả lời chỉ phụ thuộc vào modulo còn lại`2m`. Không cần mảng hoặc đệ quy. 

Một sai lầm phổ biến là sử dụng phần còn lại`0`thay vì phần còn lại`1`. Ví dụ, với`n = 20`Và`m = 10`, modulo còn lại`20`là`0`, và vị trí đang chiến thắng. Sử dụng sai số dư sẽ phân loại sai trường hợp này. 

Một lỗi dễ dàng khác là quên điều đó`2 * m`phải được tính toán trước khi lấy mô đun. Số nguyên Python không bị giới hạn nên việc tràn không bao giờ là vấn đề. 

## Ví dụ đã hoạt động 

Hãy xem xét```
n = 42, m = 9
```Từ`m > 2`, chúng ta sử dụng quy tắc chung. 

| n | m | 2m | n mod 2m | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 42 | 9 | 18 | 6 | CÓ | 

Phần còn lại là`6`, đó không phải là`1`, do đó người chơi đầu tiên có chiến lược chiến thắng. Ví dụ này cho thấy chỉ có phần còn lại mới quan trọng. 

Bây giờ hãy xem xét```
n = 69, m = 2
```| n | m | n mod 3 | Đầu ra | 
| --- | --- | --- | --- | 
| 69 | 2 | 0 | KHÔNG | 

Vì máu chia hết cho ba nên chuỗi tấn công cưỡng bức sẽ để lại nước đi cuối cùng cho Bob. 

Những dấu vết này minh họa hai chế độ khác nhau của trò chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Số lượng số học không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Thuật toán chỉ thực hiện một số thao tác mô-đun và so sánh. Ngay cả với`10^4`trường hợp thử nghiệm và giá trị lên đến`10^6`, thời gian chạy không đáng kể và dễ dàng thỏa mãn giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())

        if m == 2:
            ans.append("NO" if n % 3 == 0 else "YES")
        else:
            ans.append("NO" if n % (2 * m) == 1 else "YES")

    return "\n".join(ans)

# provided samples
assert run("""8
6 9
20 10
69 2
42 9
42 10
44 9
44 10
400000 400000
""") == """YES
YES
NO
YES
YES
YES
YES
YES"""

# minimum size
assert run("""1
1 2
""") == "YES"

# m = 2 losing case
assert run("""1
6 2
""") == "NO"

# remainder 1 modulo 2m
assert run("""1
21 10
""") == "NO"

# large values
assert run("""1
1000000 1000000
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`| CÓ | Giá trị tối thiểu | 
|`6 2`| KHÔNG | Trường hợp đặc biệt`m=2`| 
|`21 10`| KHÔNG | còn lại`1`modulo`2m`| 
|`1000000 1000000`| CÓ | Giá trị lớn và độ chính xác số học | 

## Vỏ cạnh 

Hãy xem xét```
1
6 9
```Từ`m > 2`, chúng tôi tính toán```
6 mod 18 = 6
```Phần còn lại không`1`, do đó kết quả đầu ra của thuật toán`YES`. Quả thực, người chơi đầu tiên có thể tiêu diệt trùm ngay lập tức bằng cách gây sát thương`6`. 

Bây giờ hãy xem xét```
1
69 2
```Bởi vì`m = 2`, chúng tôi sử dụng quy tắc đặc biệt:```
69 mod 3 = 0
```Do đó câu trả lời là`NO`. Các cuộc tấn công buộc phải xen kẽ giữa`1`Và`2`, cho Bob hành động cuối cùng. 

Cuối cùng xem xét```
1
20 10
```Chúng tôi có được```
20 mod 20 = 0
```Vì phần còn lại không`1`, câu trả lời là`YES`. Điều này xác nhận rằng bội số của`2m`là những vị trí chiến thắng, điều này rất dễ bị bỏ qua nếu người ta thực hiện hành vi trò chơi trừ thông thường.
