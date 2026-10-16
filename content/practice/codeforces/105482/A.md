---
title: "CF 105482A - Dây đàn acoustic"
description: "Chúng ta được cung cấp một chuỗi nhị phân và một phép biến đổi xác định liên tục thu nhỏ chuỗi đó. Mỗi bước chuyển đổi sẽ thay thế chuỗi bằng một chuỗi mới được hình thành bằng cách lấy XOR các ký tự liền kề."
date: "2026-06-23T23:23:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105482
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105482
solve_time_s: 75
verified: true
draft: false
---

[CF 105482A - Dây đàn acoustic](https://codeforces.com/problemset/problem/105482/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân và một phép biến đổi xác định liên tục thu nhỏ chuỗi đó. Mỗi bước chuyển đổi sẽ thay thế chuỗi bằng một chuỗi mới được hình thành bằng cách lấy XOR các ký tự liền kề. Vì vậy, nếu chuỗi hiện tại là$s$, chuỗi tiếp theo có độ dài ít hơn một đơn vị và mỗi vị trí sẽ trở thành giá trị chẵn lẻ của cửa sổ có độ dài-2 trong chuỗi trước đó. 

Quá trình này được lặp lại cho đến khi chỉ còn lại một bit duy nhất. Bit cuối cùng đó được xác định đầy đủ bởi chuỗi ban đầu. Nhiệm vụ là quyết định xem bit cuối cùng này có bằng 1 hay không. 

Giới hạn về độ dài lên tới$10^6$, điều này ngay lập tức loại trừ việc mô phỏng từng bước chuyển đổi. Mỗi bước giảm độ dài đi một, vì vậy một mô phỏng đơn giản sẽ tốn kém$n + (n-1) + \dots + 1$, đó là$O(n^2)$. Với$n = 10^6$, điều này sẽ vượt xa các hoạt động khả thi trong hai giây. 

Một vấn đề nhỏ xuất hiện khi xem xét chi tiết triển khai: ngay cả khi mỗi bước được triển khai hiệu quả, việc xây dựng lại chuỗi nhiều lần sẽ dẫn đến việc phân bổ và sao chép bộ nhớ lặp đi lặp lại, điều này vẫn giữ cho giải pháp bậc hai. Ví dụ: một chuỗi như`1010...`chiều dài$10^6$sẽ yêu cầu khoảng$5 \times 10^{11}$Hoạt động XOR trên tất cả các cấp độ, điều này không thực tế. 

Thuộc tính ẩn chính là giá trị cuối cùng là hàm tuyến tính trên GF(2), nghĩa là nó có thể được biểu thị dưới dạng kết hợp XOR cố định của các bit gốc với hệ số nhị thức modulo 2. Điều này tránh hoàn toàn mọi mô phỏng. 

Các trường hợp đặc biệt chủ yếu liên quan đến việc hiểu rằng quy trình này mang tính quyết định bất kể cấu trúc trung gian. Ví dụ, đầu vào`"0"`ngay lập tức trở lại`0`và đầu vào`"1"`trả lại`1`. Việc triển khai ngây thơ có thể giả định không chính xác các chuỗi trống hoặc xử lý sai các trường hợp có độ dài-1 bằng cách vẫn cố gắng chuyển đổi. 

## Phương pháp tiếp cận 

Phương pháp brute-force mô phỏng trực tiếp quá trình cộng hưởng. Bắt đầu từ chuỗi ban đầu, chúng tôi liên tục xây dựng chuỗi tiếp theo bằng cách XOR các ký tự liền kề. Điều này đơn giản và chính xác vì nó tuân theo chính xác định nghĩa vấn đề. Tuy nhiên, chi phí tăng nhanh: bước đầu tiên thực hiện$n-1$Hoạt động XOR, tiếp theo$n-2$, v.v. cho đến 1. Điều này mang lại khoảng$n(n-1)/2$các hoạt động trở nên không khả thi khi$n$đạt tới$10^6$. 

Quan sát quan trọng là mỗi phép biến đổi đều tuyến tính trên XOR. Điều đó có nghĩa là kết quả cuối cùng có thể được biểu thị dưới dạng tổng (XOR) của các bit gốc, mỗi bit nhân với một hệ số chỉ phụ thuộc vào vị trí của nó. Nếu mở rộng quy trình, chúng ta thu được tam giác Pascal modulo 2. Bit cuối cùng là XOR của tất cả các vị trí$i$sao cho hệ số nhị thức$\binom{n-1}{i-1}$thật kỳ quặc. 

Điều này biến vấn đề thành tính toán chẵn lẻ của các bit được chọn mà không cần mô phỏng quá trình. Thay vì xây dựng các lớp, chúng ta chỉ cần xác định hệ số nhị thức nào là số lẻ. Theo định lý Lucas cơ số 2,$\binom{n}{k}$là số lẻ khi và chỉ khi mọi bit được đặt vào$k$cũng được thiết lập trong$n$. Điều này đưa ra một điều kiện bitwise trực tiếp. 

Sau đó chúng tôi XOR tất cả các ký tự$s[i]$trong đó hệ số tương ứng là 1. Điều đó tạo ra bit đơn cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu (chẵn lẻ nhị thức / quy tắc XOR) |$O(n \log n)$hoặc$O(n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi đầu vào thành một mảng các bit nguyên. Điều này cho phép tích lũy XOR theo thời gian không đổi mà không cần cắt chuỗi lặp lại. 
2. Khởi tạo bộ tích lũy`ans = 0`, sẽ lưu trữ kết quả XOR cuối cùng. Bất biến là sau khi xử lý chỉ số`i`,`ans`bằng với sự đóng góp của tất cả các vị trí được xử lý tồn tại qua các lớp cộng hưởng. 
3. Đối với mỗi chỉ số`i`từ`0`ĐẾN`n-1`, xác định xem bit ở vị trí`i`góp phần vào kết quả cuối cùng. Điều này được thực hiện bằng cách kiểm tra xem hệ số nhị thức có$\binom{n-1}{i}$là số lẻ, sử dụng điều kiện bitwise$(i \& (n-1 - i)) == 0$được cải tiến thông qua lý luận theo phong cách Lucas. 
4. Nếu điều kiện hệ số được thỏa mãn, XOR`ans`với`s[i]`. Điều này tích lũy chính xác những đóng góp tồn tại thông qua tất cả việc giảm XOR. 
5. Đầu ra`ans`như là kết quả cuối cùng. 

### Tại sao nó hoạt động 

Mỗi bước cộng hưởng áp dụng một phép biến đổi tuyến tính trên GF(2), do đó toàn bộ quá trình có thể được biểu diễn dưới dạng phép nhân lặp lại với một ma trận cố định. Ứng dụng lặp đi lặp lại tương ứng với việc lấy lũy thừa của phép biến đổi đó, tạo ra cấu trúc tam giác của Pascal. Trạng thái cuối cùng là tích chập của chuỗi ban đầu với hàng cuối cùng của tam giác modulo 2 này. Vì các hệ số nhị thức modulo 2 hoạt động giống như điều kiện tập hợp bit-bit, nên chúng ta có thể kiểm tra sự đóng góp của từng vị trí một cách độc lập và chỉ XOR những vị trí tồn tại. Điều này đảm bảo sự tương đương giữa quá trình mô phỏng và tính toán tổ hợp trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def binom_odd(n, k):
    return (k & (n - k)) == 0

s = input().strip()
n = len(s)

ans = 0
N = n - 1

for i, ch in enumerate(s):
    if binom_odd(N, i):
        ans ^= (ord(ch) - 48)

print(ans)
```Mã này tránh hoàn toàn việc xây dựng các chuỗi trung gian. Điều kiện người trợ giúp`binom_odd`thực hiện mô tả đặc tính của định lý Lucas trong cơ sở 2, trong đó hệ số nhị thức là số lẻ chính xác khi không có phần nhớ nào trong phép cộng`k`Và`n-k`. Điều đó chuyển thành một bài kiểm tra bitwise đơn giản. 

Chúng tôi lặp lại chuỗi một lần, do đó không xảy ra hiện tượng chia tỷ lệ bộ nhớ ngoài bộ nhớ đầu vào. Mỗi vị trí được kiểm tra trong thời gian không đổi. 

Một lỗi phổ biến là tính toán điều kiện nhị thức không chính xác bằng cách sử dụng giai thừa hoặc tính toán trước. Điều đó sẽ thất bại do lo ngại cả về hiệu suất và tràn. Điều kiện theo bit là con đường thực tế duy nhất ở quy mô này. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`1010`Chúng tôi tính toán đóng góp cho$N = 3$. 
| tôi | s[i] | binom_odd(3, i) | và | 
| --- | --- | --- | --- | 
| 0 | 1 | đúng | 1 | 
| 1 | 0 | đúng | 1 | 
| 2 | 1 | đúng | 0 | 
| 3 | 0 | đúng | 0 | 
Kết quả cuối cùng là`0`. 

Điều này cho thấy tất cả các vị trí đóng góp như thế nào vì 3 có tất cả các bit được đặt trong phạm vi của nó, nhưng việc hủy XOR sẽ xác định kết quả. 

### Ví dụ 2:`1101`Hiện nay$N = 3$lại. 
| tôi | s[i] | binom_odd(3, i) | và | 
| --- | --- | --- | --- | 
| 0 | 1 | đúng | 1 | 
| 1 | 1 | đúng | 0 | 
| 2 | 0 | đúng | 0 | 
| 3 | 1 | đúng | 1 | 
Kết quả cuối cùng là`1`. 

Điều này xác nhận rằng cùng một mẫu hệ số vẫn có thể mang lại những kết quả khác nhau tùy thuộc vào sự phân bổ đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Truyền một lần qua chuỗi với kiểm tra bitwise theo thời gian không đổi | 
| Không gian |$O(1)$thêm | Ngoài bộ nhớ đầu vào, chỉ có bộ tích lũy được sử dụng | 

Giải pháp này phù hợp thoải mái trong giới hạn vì nó tránh được sự bùng nổ bậc hai của các lớp XOR lặp đi lặp lại. Thậm chí tại$n = 10^6$, một lần quét tuyến tính duy nhất nằm trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    n = len(s)

    def binom_odd(n, k):
        return (k & (n - k)) == 0

    ans = 0
    for i, ch in enumerate(s):
        if binom_odd(n - 1, i):
            ans ^= (ord(ch) - 48)
    return str(ans)

# provided samples
assert run("1010\n") == "0"
assert run("1101\n") == "1"

# custom cases
assert run("0\n") == "0"
assert run("1\n") == "1"
assert run("00\n") == "0"
assert run("1111\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| Xử lý độ dài tối thiểu | 
|`1`|`1`| Độ chính xác từng bit | 
|`00`|`0`| Tuyên truyền hoàn toàn bằng không | 
|`1111`|`0`| Hủy XOR trong phạm vi hoạt động đầy đủ | 

## Vỏ cạnh 

Đối với đầu vào một ký tự như`1`, vòng lặp chạy một lần với$N = 0$. điều kiện`binom_odd(0, 0)`đánh giá là đúng, vì vậy`ans`trở thành 1, phù hợp với định nghĩa rằng không cần chuyển đổi. 

Đối với đầu vào`0`, logic tương tự cũng được áp dụng nhưng XOR với 0 giữ bộ tích lũy ở mức 0, do đó kết quả vẫn đúng mà không có bất kỳ sự phân nhánh đặc biệt nào. 

Đối với các chuỗi có tính lặp lại cao như`1111...`, mọi hệ số hợp lệ đều đóng góp, do đó kết quả trở thành XOR của tất cả các bit. Thuật toán xử lý điều này một cách tự nhiên vì nó không giả định độ thưa thớt hoặc cấu trúc trong đầu vào mà chỉ đóng góp dựa trên vị trí.
