---
title: "CF 61A - Nhà toán học siêu nhanh"
description: "Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau. Mỗi ký tự là '0' hoặc '1'. Đối với mọi vị trí, chúng tôi so sánh các ký tự từ hai chuỗi. Nếu hai ký tự khác nhau thì chuỗi kết quả sẽ là '1' ở vị trí đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 61
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 57 (Div. 2)"
rating: 800
weight: 61
solve_time_s: 124
verified: true
draft: false
---

[CF 61A - Nhà toán học siêu nhanh](https://codeforces.com/problemset/problem/61/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau. Mỗi nhân vật đều là`'0'`hoặc`'1'`. Đối với mọi vị trí, chúng tôi so sánh các ký tự từ hai chuỗi. 

Nếu hai ký tự khác nhau, chuỗi kết quả sẽ`'1'`ở vị trí đó. Nếu chúng giống nhau, chuỗi kết quả sẽ`'0'`. 

Hoạt động này chính xác là hoạt động XOR nhị phân được áp dụng từng chữ số. 

Ví dụ: nếu một chuỗi có`'1'`và cái kia có`'0'`, đầu ra trở thành`'1'`. Nếu cả hai đều`'1'`hoặc cả hai đều là`'0'`, đầu ra trở thành`'0'`. 

Các dây có độ dài tối đa là 100, cực kỳ nhỏ. Ngay cả việc quét tuyến tính đơn giản trên các chuỗi cũng đủ nhanh. Bất kỳ thuật toán nào có độ phức tạp về thời gian tỷ lệ thuận với độ dài chuỗi sẽ kết thúc ngay lập tức trong giới hạn. 

Mối nguy hiểm chính trong vấn đề này không phải là hiệu suất mà là tính chính xác. Những lỗi triển khai nhỏ có thể âm thầm tạo ra chuỗi nhị phân sai. 

Một sai lầm dễ mắc phải là quên giữ nguyên các số 0 đứng đầu. Coi như:```
Input:
001
001
```Đầu ra đúng là:```
000
```Nếu ai đó chuyển đổi chuỗi thành số nguyên và ngược lại, họ có thể vô tình chỉ in`0`, điều này sai vì vấn đề yêu cầu phải giữ tất cả các vị trí. 

Một lỗi phổ biến khác là hiểu nhầm phép toán là phép cộng nhị phân thay vì XOR. Ví dụ:```
Input:
1
1
```Đầu ra đúng:```
0
```Việc triển khai bất cẩn bằng cách sử dụng phép cộng modulo 2 không chính xác hoặc xử lý các bit mang có thể tạo ra kết quả sai. 

Trường hợp tinh tế thứ ba là các mẫu xen kẽ trong đó mọi so sánh đều thay đổi:```
Input:
1010
0101
```Đầu ra đúng:```
1111
```Điều này kiểm tra xem việc triển khai có so sánh từng vị trí một cách độc lập hay không thay vì cố gắng diễn giải các chuỗi dưới dạng số nhị phân đầy đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng chuỗi ký tự. Đối với mọi chỉ mục, hãy so sánh hai ký tự. Nếu chúng khác nhau, hãy nối thêm`'1'`để trả lời. Nếu không thì nối thêm`'0'`. 

Điều này đã giải quyết vấn đề một cách hiệu quả vì độ dài tối đa chỉ là 100. Ngay cả khi chúng ta thực hiện nhiều lần chuyển qua chuỗi, thời gian chạy vẫn sẽ rất nhỏ. 

Một phiên bản ngây thơ hơn sẽ liên tục xây dựng câu trả lời bằng cách nối chuỗi:```
ans += '1'
```bên trong vòng lặp. Trong Python, việc nối chuỗi lặp đi lặp lại có thể trở nên không hiệu quả đối với các đầu vào rất lớn vì các chuỗi là bất biến. Mỗi phép nối sẽ tạo ra một chuỗi mới. Đối với độ dài 100, mô hình này vẫn hoạt động hoàn toàn tốt, nhưng trong các bài toán lớn hơn, mô hình này có thể trở thành bậc hai. 

Cách tiếp cận rõ ràng hơn là thu thập các ký tự trong danh sách và nối chúng một lần vào cuối. Quan sát quan trọng là mọi chữ số đầu ra chỉ phụ thuộc vào cặp chữ số đầu vào tương ứng. Không có vị trí nào ảnh hưởng đến bất kỳ vị trí nào khác. Điều đó có nghĩa là chúng ta không bao giờ cần mang, tiền xử lý hoặc cấu trúc dữ liệu nâng cao. 

Toàn bộ nhiệm vụ giảm xuống thành một so sánh tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nối chuỗi Brute Force | O(n²) về mặt lý thuyết | O(n) | Đã chấp nhận | 
| Xây dựng từng ký tự tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi nhị phân. 

Cả hai chuỗi đều được đảm bảo có cùng độ dài, vì vậy chúng ta có thể xử lý chúng theo chỉ mục một cách an toàn. 
2. Tạo một danh sách trống để lưu trữ các ký tự trả lời. 

Việc sử dụng danh sách sẽ tránh được việc tái phân bổ chuỗi lặp lại trong quá trình xây dựng. 
3. Lặp lại qua mọi vị trí từ`0`ĐẾN`n - 1`. 

Mỗi vị trí đều độc lập với những vị trí khác. 
4. So sánh các ký tự ở chỉ mục hiện tại. 

Nếu các ký tự khác nhau, hãy nối thêm`'1'`vào danh sách câu trả lời. Nếu không thì nối thêm`'0'`. 

Điều này khớp trực tiếp với định nghĩa XOR từ vấn đề. 
5. Nối tất cả các ký tự đã thu thập thành một chuỗi. 

Chuỗi cuối cùng giữ nguyên mọi vị trí, kể cả số 0 đứng đầu. 
6. In kết quả. 

### Tại sao nó hoạt động 

Đối với mỗi chỉ số`i`, bài toán xác định chữ số trả lời hoàn toàn từ cặp`(a[i], b[i])`. 

Thuật toán áp dụng chính xác quy tắc này một cách độc lập ở mọi vị trí: 

- tạo ra các chữ số bằng nhau`'0'`- tạo ra các chữ số khác nhau`'1'`Vì mọi vị trí đều được xử lý chính xác và độc lập nên chuỗi kết quả hoàn chỉnh là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = input().strip()
b = input().strip()

ans = []

for x, y in zip(a, b):
    if x != y:
        ans.append('1')
    else:
        ans.append('0')

print(''.join(ans))
```Hai dòng đầu tiên đọc chuỗi nhị phân và xóa các ký tự dòng mới ở cuối bằng`strip()`. 

Câu trả lời được lưu trữ trong danh sách vì việc thêm vào danh sách sẽ hiệu quả. Sau khi xử lý tất cả các vị trí,`''.join(ans)`tạo chuỗi cuối cùng trong một thao tác. 

Vòng lặp sử dụng`zip(a, b)`để lặp qua các vị trí khớp từ cả hai chuỗi cùng một lúc. Vì vấn đề đảm bảo độ dài bằng nhau nên mọi cặp ký tự đều được căn chỉnh chính xác. 

Sự so sánh`x != y`khớp chính xác với điều kiện XOR từ câu lệnh. Không cần chuyển đổi số nguyên, điều này tránh được các lỗi liên quan đến số 0 đứng đầu hoặc số học nhị phân. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1010100
0100101
```| Vị trí | Chuỗi đầu tiên | Chuỗi thứ hai | Khác biệt? | Chữ số đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | Có | 1 | 
| 1 | 0 | 1 | Có | 1 | 
| 2 | 1 | 0 | Có | 1 | 
| 3 | 0 | 0 | Không | 0 | 
| 4 | 1 | 1 | Không | 0 | 
| 5 | 0 | 0 | Không | 0 | 
| 6 | 0 | 1 | Có | 1 | 

Đầu ra cuối cùng:```
1110001
```Dấu vết này cho thấy mỗi vị trí được xử lý độc lập. Các chữ số phù hợp sẽ trở thành`0`, các chữ số khác nhau trở thành`1`. 

### Ví dụ 2 

đầu vào:```
1111
1111
```| Vị trí | Chuỗi đầu tiên | Chuỗi thứ hai | Khác biệt? | Chữ số đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | Không | 0 | 
| 1 | 1 | 1 | Không | 0 | 
| 2 | 1 | 1 | Không | 0 | 
| 3 | 1 | 1 | Không | 0 | 

Đầu ra cuối cùng:```
0000
```Ví dụ này xác nhận rằng các số 0 đứng đầu trong kết quả phải được giữ nguyên. Câu trả lời không phải là duy nhất`0`, đó là một chuỗi bốn ký tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(n) | Chuỗi câu trả lời lưu trữ`n`nhân vật | 

Độ dài tối đa chỉ là 100 nên thuật toán chạy thoải mái trong giới hạn. Cả thời gian chạy và mức sử dụng bộ nhớ đều rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    a = input().strip()
    b = input().strip()

    ans = []

    for x, y in zip(a, b):
        if x != y:
            ans.append('1')
        else:
            ans.append('0')

    print(''.join(ans))

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
assert run("1010100\n0100101\n") == "1110001\n", "sample 1"

# minimum size
assert run("0\n0\n") == "0\n", "single equal digits"

# minimum size different
assert run("1\n0\n") == "1\n", "single different digits"

# all equal values
assert run("11111\n11111\n") == "00000\n", "all positions equal"

# alternating pattern
assert run("1010\n0101\n") == "1111\n", "every position differs"

# leading zeros preserved
assert run("0011\n0011\n") == "0000\n", "leading zeros must remain"

# maximum-style repeated pattern
a = "01" * 50
b = "10" * 50
expected = "11" * 50 + "\n"
assert run(f"{a}\n{b}\n") == expected, "length 100 case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 / 0`|`0`| Đầu vào nhỏ nhất có thể | 
|`1 / 0`|`1`| Các chữ số đơn khác nhau | 
|`11111 / 11111`|`00000`| Chuỗi bằng nhau | 
|`1010 / 0101`|`1111`| Mọi vị trí đều khác nhau | 
|`0011 / 0011`|`0000`| Số 0 đứng đầu được bảo tồn | 
| Chiều dài 100 dây xen kẽ | Tất cả`1`s | Hành vi hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét trường hợp cả hai chuỗi giống hệt nhau và bắt đầu bằng số 0:```
Input:
001
001
```Thuật toán so sánh từng cặp chữ số: 

-`0`vs`0`→`0`-`0`vs`0`→`0`-`1`vs`1`→`0`Câu trả lời được tạo ra là:```
000
```Vì giải pháp này hoạt động trực tiếp với chuỗi thay vì số nguyên nên các số 0 đứng đầu vẫn được giữ nguyên. 

Bây giờ hãy xem xét đầu vào khác nhau nhỏ nhất có thể:```
Input:
1
0
```Thuật toán xử lý vị trí duy nhất: 

-`1`vs`0`→`1`Đầu ra:```
1
```Điều này xác nhận quy tắc XOR được triển khai chính xác. 

Cuối cùng, hãy xem xét trường hợp mỗi vị trí đều khác nhau:```
Input:
1010
0101
```So sánh từng bước: 

-`1`vs`0`→`1`-`0`vs`1`→`1`-`1`vs`0`→`1`-`0`vs`1`→`1`Đầu ra:```
1111
```Điều này xác minh rằng thuật toán xử lý các vị trí một cách độc lập thay vì thực hiện số học nhị phân bằng mang.
