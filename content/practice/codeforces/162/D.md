---
title: "CF 162D - Xóa chữ số"
description: "Chúng ta được cung cấp một chuỗi chứa các ký tự ASCII có thể in được. Một số ký tự đó có thể là các chữ số từ '0' đến '9'. Nhiệm vụ là loại bỏ từng chữ số và in các ký tự còn lại theo thứ tự ban đầu."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "D"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 1900
weight: 162
solve_time_s: 75
verified: true
draft: false
---

[CF 162D - Xóa chữ số](https://codeforces.com/problemset/problem/162/D) 

**Xếp hạng:** 1900 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chứa các ký tự ASCII có thể in được. Một số ký tự đó có thể là chữ số từ`'0'`ĐẾN`'9'`. Nhiệm vụ là loại bỏ từng chữ số và in các ký tự còn lại theo thứ tự ban đầu. 

Tuyên bố đề cập rằng các ký tự sẽ dịch chuyển sang trái sau khi xóa, nhưng chi tiết đó chỉ mô tả cách hoạt động của việc xóa chuỗi về mặt khái niệm. Chúng tôi thực sự không cần phải mô phỏng việc xóa đi lặp lại. Kết quả cuối cùng chỉ đơn giản là dãy ký tự không có chữ số. 

Độ dài đầu vào tối đa là 100 ký tự, cực kỳ nhỏ. Ngay cả những cách tiếp cận không hiệu quả cũng sẽ phù hợp một cách thoải mái trong giới hạn. Một giải pháp bậc hai với việc xóa nhiều lần sẽ thực hiện tối đa khoảng 10.000 thao tác ký tự, điều này là không đáng kể đối với giới hạn 3 giây. Tuy nhiên, giải pháp sạch nhất sẽ quét chuỗi một lần và xây dựng câu trả lời trực tiếp. 

Có một số trường hợp nguy hiểm có thể lặng lẽ phá vỡ việc triển khai bất cẩn. 

Một chuỗi chỉ chứa các chữ số sẽ tạo ra một dòng trống. 

đầu vào:```
12345
```Đầu ra đúng:```

```Việc triển khai có lỗi có thể vô tình in các khoảng trắng hoặc không in được dòng mới. 

Các chữ số có thể xuất hiện ở bất cứ đâu, không chỉ ở cuối. 

đầu vào:```
a1b2c3
```Đầu ra đúng:```
abc
```Giải pháp sai chỉ loại bỏ chữ số đầu tiên hoặc dừng sớm sẽ không thành công ở đây. 

Chuỗi có thể chứa các ký hiệu không phải là chữ cái hoặc chữ số. 

đầu vào:```
VK-Cup-2012!
```Đầu ra đúng:```
VK-Cup-!
```Dấu gạch ngang và dấu chấm than phải ở lại. Chỉ các ký tự số được loại bỏ. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là liên tục tìm kiếm một chữ số và xóa nó khỏi chuỗi. Vì các chuỗi là bất biến trong Python nên mỗi lần xóa sẽ tạo ra một chuỗi mới, do đó việc xóa một ký tự sẽ tốn thời gian tuyến tính. Nếu chuỗi có độ dài`n`và chúng tôi loại bỏ từng chữ số một, tổng độ phức tạp sẽ trở thành`O(n^2)`trong trường hợp xấu nhất. 

Đối với vấn đề này, ngay cả cách tiếp cận đó cũng đủ nhanh vì`n ≤ 100`. Tuy nhiên, nó làm công việc không cần thiết. Mỗi lần xóa sẽ dịch chuyển các ký tự sang trái, có nghĩa là các ký tự giống nhau có thể được sao chép nhiều lần. 

Quan sát tốt hơn là chúng ta không bao giờ cần phải xóa các ký tự về mặt vật lý. Câu trả lời cuối cùng bao gồm chính xác các ký tự không phải là chữ số. Thay vì sửa đổi chuỗi gốc, chúng ta có thể quét chuỗi đó một lần từ trái sang phải và nối từng ký tự không có chữ số vào kết quả. 

Điều này làm giảm công việc xuống còn một lần truyền qua chuỗi. Mỗi ký tự được kiểm tra một lần và được thêm vào nhiều nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Tạo một danh sách trống để lưu trữ các ký tự mà chúng ta muốn giữ lại. 
3. Lặp lại từng ký tự trong chuỗi. 
4. Đối với mỗi ký tự, hãy kiểm tra xem đó có phải là chữ số hay không. 

Trong Python,`ch.isdigit()`trả lại`True`cho các ký tự số và`False`nếu không thì. 
5. Nếu ký tự không phải là chữ số, hãy thêm nó vào danh sách kết quả. 

Chúng tôi giữ nguyên tất cả các chữ cái, dấu chấm câu và ký hiệu. 
6. Sau khi xử lý toàn bộ chuỗi, nối các ký tự đã thu thập được thành một chuỗi cuối cùng. 
7. In kết quả. 

### Tại sao nó hoạt động 

Thuật toán xử lý mỗi ký tự chính xác một lần. Trong quá trình quét, nó giữ nguyên thứ tự tương đối của tất cả các ký tự không có chữ số vì chúng được thêm vào theo cùng thứ tự xuất hiện trong đầu vào. Mọi chữ số đều bị bỏ qua, do đó không có chữ số nào có thể xuất hiện ở đầu ra. Vì mỗi ký tự đều bị bỏ qua hoặc nối thêm chính xác một lần nên chuỗi được tạo ra chính xác là chuỗi gốc đã loại bỏ tất cả các chữ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

result = []

for ch in s:
    if not ch.isdigit():
        result.append(ch)

print("".join(result))
```Chương trình bắt đầu bằng cách đọc chuỗi đầu vào. sử dụng`strip()`xóa dòng mới ở cuối khỏi đầu vào. 

các`result`list lưu trữ tất cả các ký tự tồn tại trong bước lọc. Sử dụng danh sách sẽ hiệu quả vì việc thêm các ký tự sẽ`O(1)`, trong khi việc nối các chuỗi liên tục sẽ tạo ra nhiều chuỗi tạm thời. 

Vòng lặp kiểm tra từng ký tự một cách độc lập. Nếu như`isdigit()`trả lại`False`, ký tự được thêm vào câu trả lời. Các chữ số bị bỏ qua hoàn toàn. 

Cuối cùng,`"".join(result)`chuyển đổi các ký tự được thu thập thành chuỗi cuối cùng. Nếu mọi ký tự đều là một chữ số thì danh sách vẫn trống và`join`tạo ra một chuỗi trống một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
VK-Cup-2012!
```| Nhân vật hiện tại | Chữ số? | Kết quả cho đến nay | 
| --- | --- | --- | 
| V | Không | V | 
| K | Không | VK | 
| - | Không | VK- | 
| C | Không | VK-C | 
| bạn | Không | VK-Cu | 
| p | Không | Cúp VK | 
| - | Không | VK-Cúp- | 
| 2 | Có | VK-Cup- | 
| 0 | Có | VK-Cup- | 
| 1 | Có | VK-Cup- | 
| 2 | Có | VK-Cúp- | 
| ! | Không | VK-Cúp-! | 

Đầu ra cuối cùng:```
VK-Cup-!
```Dấu vết này cho thấy các biểu tượng được bảo tồn chính xác như khi chúng xuất hiện. Chỉ có ký tự số được bỏ qua. 

### Ví dụ 2 

đầu vào:```
a1b2c3
```| Nhân vật hiện tại | Chữ số? | Kết quả cho đến nay | 
| --- | --- | --- | 
| một | Không | một | 
| 1 | Có | một | 
| b | Không | ab | 
| 2 | Có | ab | 
| c | Không | abc | 
| 3 | Có | abc | 

Đầu ra cuối cùng:```
abc
```Ví dụ này chứng tỏ rằng các chữ số có thể xuất hiện giữa các ký tự bình thường và được loại bỏ một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(n) | Kết quả có thể lưu trữ tất cả các ký tự | 

Với`n ≤ 100`, thuật toán dễ dàng phù hợp trong giới hạn. Ngay cả những cách tiếp cận chậm hơn nhiều cũng sẽ vượt qua, nhưng quét tuyến tính vừa đơn giản vừa hiệu quả hơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    s = input().strip()

    result = []

    for ch in s:
        if not ch.isdigit():
            result.append(ch)

    print("".join(result))

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
assert run("VK-Cup-2012!\n") == "VK-Cup-!\n", "sample 1"

# custom cases
assert run("12345\n") == "\n", "all digits"
assert run("abc\n") == "abc\n", "no digits"
assert run("a1b2c3\n") == "abc\n", "digits between letters"
assert run("9\n") == "\n", "minimum size, single digit"
assert run("!@#456$%^789&*\n") == "!@#$%^&*\n", "symbols preserved"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`12345`| chuỗi trống | Xử lý đầu vào toàn chữ số | 
|`abc`|`abc`| Giữ nguyên các chuỗi không có chữ số | 
|`a1b2c3`|`abc`| Xóa các chữ số ở giữa | 
|`9`| chuỗi trống | Vỏ cạnh kích thước tối thiểu | 
|`!@#456⟦PROTECT_15⟧%^&*`| Giữ nguyên dấu câu và ký hiệu | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
12345
```Thuật toán quét từng ký tự. Vì tất cả các ký tự đều là chữ số nên không có ký tự nào được thêm vào`result`. 

| Nhân vật | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Bỏ qua | "" | 
| 2 | Bỏ qua | "" | 
| 3 | Bỏ qua | "" | 
| 4 | Bỏ qua | "" | 
| 5 | Bỏ qua | "" | 

Cuối cùng,`join(result)`tạo ra một chuỗi trống, chính xác là đầu ra được yêu cầu. 

Bây giờ hãy xem xét:```
a0!
```Quá trình quét diễn ra như sau: 

| Nhân vật | Hành động | Kết quả | 
| --- | --- | --- | 
| một | Nối thêm | một | 
| 0 | Bỏ qua | một | 
| ! | Nối thêm | Một! | 

Chữ số bị loại bỏ, trong khi dấu chấm câu vẫn được giữ nguyên. 

Cuối cùng, hãy xem xét:```
007bond
```| Nhân vật | Hành động | Kết quả | 
| --- | --- | --- | 
| 0 | Bỏ qua | "" | 
| 0 | Bỏ qua | "" | 
| 7 | Bỏ qua | "" | 
| b | Nối thêm | b | 
| o | Nối thêm | bo | 
| n | Nối thêm | bon | 
| d | Nối thêm | trái phiếu | 

Các chữ số hàng đầu được xử lý một cách tự nhiên vì thuật toán không dựa vào vị trí hoặc chỉ số. Nó chỉ đơn giản lọc các ký tự dựa trên việc chúng có phải là chữ số hay không.
