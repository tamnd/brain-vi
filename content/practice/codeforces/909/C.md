---
title: "CF 909C - Thụt lề Python"
description: "Chúng ta được cung cấp một chuỗi các câu lệnh của chương trình. Mỗi câu lệnh là: f , một câu lệnh for có nội dung phải chứa ít nhất một câu lệnh ở một mức thụt đầu dòng sâu hơn. s , một câu lệnh đơn giản chiếm chính xác một dòng và không tạo khối mới."
date: "2026-06-12T10:26:25+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 909
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 455 (Div. 2)"
rating: 1800
weight: 909
solve_time_s: 180
verified: true
draft: false
---

[CF 909C - Thụt lề Python](https://codeforces.com/problemset/problem/909/C) 

**Đánh giá:** 1800 
**Thẻ:** dp 
**Thời gian giải:** 3 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các câu lệnh của chương trình. Mỗi tuyên bố là:`f`, Một`for`câu lệnh có nội dung phải chứa ít nhất một câu lệnh ở mức thụt đầu dòng sâu hơn.`s`, một câu lệnh đơn giản chiếm đúng một dòng và không tạo khối mới. 

Việc thụt lề đã bị xóa khỏi chương trình. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu bố cục thụt lề hợp lệ có thể tạo ra một chương trình giống Python đúng về mặt cú pháp. 

Chi tiết quan trọng là vết lõm xác định cấu trúc khối. Mọi`for`câu lệnh phải có phần thân không trống, do đó câu lệnh ngay sau câu lệnh`f`phải được thụt vào sâu hơn một cấp và thuộc về phần thân của nó. Tuy nhiên, sau một câu lệnh đơn giản, câu lệnh tiếp theo có thể giữ nguyên mức thụt lề hoặc lùi lại một lượng bất kỳ mà vẫn giữ cho chương trình hợp lệ. 

Số lượng lệnh nhiều nhất là 5000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào xây dựng rõ ràng tất cả các cây thụt lề có thể có. Ngay cả các thuật toán bậc hai cũng phải được thực hiện cẩn thận vì$5000^2 = 25$triệu hoạt động. Một giải pháp khối sẽ yêu cầu khoảng$1.25 \times 10^{11}$hoạt động và hoàn toàn không khả thi. 

Câu trả lời phải được tính theo modulo$10^9+7$. 

Một khía cạnh tinh tế của vấn đề là các lựa chọn thụt đầu dòng không được thực hiện tại`for`các tuyên bố. MỘT`for`buộc dòng tiếp theo phải sâu hơn một cấp. Sự tự do thực sự xuất hiện sau những câu lệnh đơn giản, bởi vì một câu lệnh đơn giản có thể chấm dứt một số vòng lặp hiện đang mở. 

Coi như:```
2
f
s
```Có chính xác một chương trình hợp lệ:```
for
    simple
```Câu lệnh đơn giản phải nằm bên trong thân vòng lặp. 

Một trường hợp thú vị khác là:```
3
f
s
s
```thứ hai`s`có thể vẫn ở trong vòng lặp hoặc đóng vòng lặp trước. 

Hai bố cục hợp lệ là:```
for
    simple
    simple
```Và```
for
    simple
simple
```Một giải pháp chỉ theo dõi vết lõm hiện tại và cố gắng giữ nó không thay đổi sẽ bỏ lỡ một trong những khả năng này. 

Trường hợp cạnh thứ ba là một chuỗi các vòng lặp:```
4
f
f
f
s
```Câu trả lời là 1. 

Mỗi vòng lặp yêu cầu vòng lặp kế tiếp ngay lập tức của nó phải ở bên trong phần thân của nó, do đó độ sâu lồng ghép hoàn toàn bị ép buộc:```
for
    for
        for
            simple
```Không có sự lựa chọn tồn tại. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xây dựng lại tất cả các mẫu thụt lề hợp lệ. Chúng tôi xử lý các câu lệnh từ trên xuống dưới, duy trì mức thụt lề hiện tại và bất cứ khi nào gặp một câu lệnh đơn giản, chúng tôi sẽ thử mọi mức độ thụt lề có thể có trước câu lệnh tiếp theo. 

Tìm kiếm mạnh mẽ này là chính xác vì mọi cấu hình thụt lề hợp lệ đều tương ứng với một chuỗi các quyết định thụt lề. Thật không may, số lượng khả năng tăng theo cấp số nhân. Ngay cả một chuỗi bao gồm hầu hết các câu lệnh đơn giản cũng có thể tạo ra hệ số phân nhánh rất lớn, khiến cho việc liệt kê toàn diện là không thể. 

Quan sát quan trọng là chỉ có độ sâu thụt hiện tại mới quan trọng. Lịch sử chính xác tạo ra chiều sâu đó là không liên quan. 

Điều này gợi ý lập trình động. 

Giả sử chúng ta xử lý từng câu lệnh một. Cho phép`dp[d]`đại diện cho số cách để đạt được độ sâu thụt đầu dòng`d`trước khi xử lý câu lệnh hiện tại. 

Bây giờ hãy xem xét tuyên bố tiếp theo bị ảnh hưởng như thế nào. 

Nếu tuyên bố hiện tại là`f`, thì câu lệnh tiếp theo phải sâu hơn một cấp. Mọi trạng thái ở độ sâu`d`chuyển tiếp theo chiều sâu`d+1`. 

Nếu tuyên bố hiện tại là`s`, thì trước câu lệnh tiếp theo, chúng ta có thể đóng 0 hoặc nhiều vòng lặp hiện đang mở. Bắt đầu từ độ sâu`d`, chúng ta có thể di chuyển đến bất kỳ độ sâu nào giữa`0`Và`d`. 

Quá trình chuyển đổi thứ hai này có vẻ tốn kém vì mỗi bang phân nhánh thành nhiều bang. Việc triển khai ngây thơ sẽ yêu cầu$O(n^2)$làm việc trên mỗi hàng và$O(n^3)$tổng thể. 

Cấu trúc của quá trình chuyển đổi đã cứu chúng ta. Đối với một tuyên bố đơn giản, mọi độ sâu đều nhận được sự đóng góp từ tất cả các độ sâu sâu hơn hoặc bằng nhau:$$new[d] = \sum_{k=d}^{n} dp[k]$$Đây chính xác là một khoản tiền hậu tố. Chúng ta có thể tính toán toàn bộ hàng DP theo thời gian tuyến tính. 

Vì có tối đa 5000 câu lệnh và chi phí cho mỗi bước$O(n)$, tổng độ phức tạp trở thành$O(n^2)$, thoải mái phù hợp với giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP tối ưu với tổng hậu tố | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`dp[d]`biểu thị số cách hợp lệ để ở độ sâu thụt lề`d`trước khi xử lý câu lệnh tiếp theo. 

Ban đầu, trước câu lệnh đầu tiên, chương trình bắt đầu ở độ sâu 0. 

1. Tạo một mảng`dp`kích thước`n + 1`và thiết lập`dp[0] = 1`. 
2. Xử lý các câu lệnh từ lệnh đầu tiên đến lệnh cuối cùng thứ hai. 

Chúng ta dừng lại ở lệnh cuối cùng thứ hai vì mỗi câu lệnh xác định cách đặt câu lệnh sau. 
3. Nếu câu lệnh hiện tại là`f`, tạo một hàng DP mới trong đó mọi trạng thái sẽ dịch chuyển sâu hơn một cấp. 

Đối với mọi độ sâu`d`, thêm vào`dp[d]`vào trong`new[d+1]`. 

Điều này mô hình hóa quy tắc rằng câu lệnh tiếp theo phải thuộc về thân vòng lặp. 
4. Nếu câu lệnh hiện tại là`s`, tính tổng hậu tố. 

Cho phép$$suffix[d] = dp[d] + suffix[d+1]$$Sau đó đặt$$new[d] = suffix[d]$$Mọi cấu hình ở độ sâu`k`có thể thụt lề ở bất kỳ độ sâu nào từ`0`bởi vì`k`, rất sâu`d`nhận được sự đóng góp từ tất cả các bang có chiều sâu ít nhất`d`. 
5. Thay thế`dp`với hàng mới được tính toán. 
6. Sau khi tất cả các chuyển đổi được xử lý, tính tổng tất cả các giá trị ở hàng DP cuối cùng. 

Tuyên bố cuối cùng được đảm bảo là`s`, vì vậy mọi độ sâu còn lại thể hiện một chương trình hoàn chỉnh hợp lệ. 
7. Xuất kết quả theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý câu lệnh`i`,`dp[d]`bằng số lượng phép gán thụt lề hợp lệ cho tiền tố kết thúc tại câu lệnh`i`, câu nói đó`i+1`được định vị ở độ sâu`d`. 

Đối với một`for`câu lệnh, cú pháp Python buộc câu lệnh tiếp theo phải sâu hơn chính xác một cấp, do đó chuyển mọi trạng thái từ`d`ĐẾN`d+1`là vừa cần vừa đủ. 

Đối với một câu lệnh đơn giản, câu lệnh tiếp theo có thể vẫn ở độ sâu hiện tại hoặc xuất hiện sau khi đóng bất kỳ số vòng lặp hoạt động nào. Một trạng thái ở độ sâu`k`do đó có thể chuyển sang mọi độ sâu`0..k`. Tính tổng hậu tố tổng hợp chính xác những khả năng này. 

Vì mỗi lựa chọn thụt lề hợp lệ tương ứng với một chuyển đổi DP và mỗi chuyển đổi DP tương ứng với một lựa chọn thụt lề hợp lệ, DP sẽ đếm tất cả các chương trình hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    cmds = [input().strip() for _ in range(n)]

    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(n - 1):
        ndp = [0] * (n + 1)

        if cmds[i] == 'f':
            for d in range(n):
                ndp[d + 1] = dp[d]
        else:
            suffix = 0
            for d in range(n, -1, -1):
                suffix = (suffix + dp[d]) % MOD
                ndp[d] = suffix

        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Mảng DP lưu trữ số lượng được lập chỉ mục theo độ sâu thụt đầu dòng. Chỉ có độ sâu lên tới`n`là cần thiết vì không có chương trình nào có thể trở nên sâu hơn số lượng câu lệnh. 

Đối với một`f`, quá trình chuyển đổi là một sự dịch chuyển thuần túy. Một lỗi phổ biến là cập nhật mảng tại chỗ. Làm như vậy sẽ trộn các trạng thái từ hàng hiện tại với các trạng thái của hàng tiếp theo. Sử dụng tươi`ndp`lảng tránh vấn đề đó. 

Đối với một`s`, mọi độ sâu đều nhận được sự đóng góp từ tất cả các độ sâu sâu hơn. Việc tính toán trực tiếp các khoản tiền này sẽ mất thời gian bậc hai cho mỗi lần chuyển đổi. Việc quét tổng hậu tố sẽ tích lũy chúng trong một lần từ phải sang trái. 

Vòng lặp chỉ chạy qua`n - 1`các tuyên bố. Đây là cố ý. Tuyên bố`i`xác định vị trí của câu lệnh`i + 1`, vì vậy lệnh cuối cùng không có chuyển tiếp đi. 

Cuối cùng, tất cả độ sâu còn lại đều là trạng thái kết thúc hợp lệ, do đó số lượng của chúng được tính tổng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
s
f
f
s
```Các trạng thái xử lý: 

| Bước | Tuyên bố | Độ sâu DP có giá trị khác 0 | 
| --- | --- | --- | 
| Bắt đầu | - | {0: 1} | 
| Sau s | chuyển tiếp hậu tố | {0: 1} | 
| Sau f | ca | {1: 1} | 
| Sau f | ca | {2: 1} | 

DP cuối cùng: 

| Độ sâu | Cách | 
| --- | --- | 
| 2 | 1 | 

Trả lời = 1. 

Chuỗi liên tiếp`f`câu lệnh buộc lồng nhau. Không có lựa chọn thụt lề nào xuất hiện. 

### Mẫu 2 

đầu vào:```
4
f
s
f
s
```Các trạng thái xử lý: 

| Bước | Tuyên bố | Độ sâu DP có giá trị khác 0 | 
| --- | --- | --- | 
| Bắt đầu | - | {0: 1} | 
| Sau f | ca | {1: 1} | 
| Sau s | hậu tố | {0: 1, 1: 1} | 
| Sau f | ca | {1: 1, 2: 1} | 

DP cuối cùng: 

| Độ sâu | Cách | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 

Trả lời = 2. 

Bước hậu tố sau câu lệnh đơn giản đầu tiên tạo ra hai khả năng. Chúng ta có thể ở trong vòng lặp hiện tại hoặc đóng nó trước khi bắt đầu vòng lặp tiếp theo.`for`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | có`n`Hàng DP, mỗi hàng được xử lý trong thời gian O(n) | 
| Không gian | O(n) | Chỉ các hàng DP hiện tại và tiếp theo mới được lưu trữ | 

Với$n \le 5000$, thuật toán thực hiện khoảng 25 triệu thao tác đơn giản, vừa vặn trong giới hạn nhất định trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    cmds = [input().strip() for _ in range(n)]

    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(n - 1):
        ndp = [0] * (n + 1)

        if cmds[i] == "f":
            for d in range(n):
                ndp[d + 1] = dp[d]
        else:
            suffix = 0
            for d in range(n, -1, -1):
                suffix = (suffix + dp[d]) % MOD
                ndp[d] = suffix

        dp = ndp

    return str(sum(dp) % MOD)

# sample 1
assert run("4\ns\nf\nf\ns\n") == "1"

# sample 2 from statement discussion
assert run("4\nf\ns\nf\ns\n") == "2"

# minimum size
assert run("1\ns\n") == "1"

# single loop
assert run("2\nf\ns\n") == "1"

# one loop followed by another simple statement
assert run("3\nf\ns\ns\n") == "2"

# fully forced nesting
assert run("5\nf\nf\nf\nf\ns\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 s`|`1`| Chương trình hợp lệ nhỏ nhất | 
|`f s`|`1`| Thân vòng lặp bắt buộc | 
|`f s s`|`2`| Căn lề tùy chọn sau câu lệnh đơn giản | 
|`f f f f s`|`1`| Làm tổ hoàn toàn bắt buộc | 
|`f s f s`|`2`| Phân nhánh được tạo bởi quá trình chuyển đổi tổng hậu tố | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
s
```DP bắt đầu với`dp[0] = 1`. Không có chuyển tiếp nào được xử lý vì không có câu lệnh sau. Tổng hợp hàng cuối cùng cho 1. Điều này đếm chính xác chương trình hợp lệ duy nhất bao gồm một câu lệnh đơn giản. 

Hãy xem xét một chuỗi các vòng lặp:```
4
f
f
f
s
```Các chuyển tiếp là:```
depth 0 -> depth 1
depth 1 -> depth 2
depth 2 -> depth 3
```Không có quá trình chuyển đổi tổng hậu tố nào xảy ra. DP cuối cùng chứa chính xác một trạng thái, vì vậy câu trả lời là 1. Mỗi vòng lặp phải chứa câu lệnh tiếp theo, buộc phải lồng hoàn toàn. 

Coi như:```
3
f
s
s
```Sau lần đầu tiên`f`, DP chỉ chứa độ sâu 1. Sau đó, câu lệnh đơn giản thực hiện chuyển đổi hậu tố và tạo các trạng thái ở độ sâu 0 và 1. Chúng thể hiện hai lựa chọn hợp pháp: đóng vòng lặp trước câu lệnh tiếp theo hoặc giữ câu lệnh tiếp theo bên trong vòng lặp. Thuật toán xuất ra 2, khớp với số lượng bố cục thụt lề hợp lệ thực tế. 

Những ví dụ này minh họa bất biến trung tâm:`f`tạo ra sự lồng ghép bắt buộc, trong khi`s`tạo ra tất cả các lựa chọn thụt lề hợp pháp thông qua quá trình chuyển đổi tổng hậu tố.
