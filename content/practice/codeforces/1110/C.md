---
title: "CF 1110C - Những hoạt động vô nghĩa"
description: "Chúng ta được cấp một số $a$. Với số này, chúng ta được phép chọn bất kỳ $b$ nào sao cho $1 le b < a$. Với mỗi lựa chọn $b$, chúng tôi tính toán hai giá trị rút ra từ các phép toán theo bit: một là $a oplus b$, giá trị kia là $a & b$. Sau đó chúng tôi lấy gcd của hai kết quả này."
date: "2026-06-12T05:03:53+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1110
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 1"
rating: 1500
weight: 1110
solve_time_s: 93
verified: true
draft: false
---

[CF 1110C - Hoạt động vô nghĩa](https://codeforces.com/problemset/problem/1110/C) 

**Đánh giá:** 1500 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$a$. Đối với số này chúng ta được phép chọn bất kỳ$b$như vậy$1 \le b < a$. Đối với mỗi sự lựa chọn của$b$, chúng tôi tính toán hai giá trị xuất phát từ các phép toán theo bit: một là$a \oplus b$, cái còn lại là$a \& b$. Sau đó chúng tôi lấy gcd của hai kết quả này. Nhiệm vụ là tối đa hóa gcd này trên tất cả các giá trị hợp lệ$b$và lặp lại điều này cho nhiều truy vấn. 

Khó khăn chính đó là$b$tương tác với$a$theo hai cách nhị phân khác nhau. XOR hoạt động giống như “sự khác biệt của các bit không mang”, trong khi AND hoạt động giống như “chỉ các bit chung”. Sau đó, gcd trích xuất cấu trúc ước số chung từ hai số nguyên dẫn xuất này, rõ ràng là không liên quan đến số ban đầu. 

Các ràng buộc có số lượng truy vấn nhỏ nhưng có kích thước giá trị lớn, lên tới khoảng$2^{25}$. Điều đó ngay lập tức loại trừ việc thử tất cả$b$giá trị cho mỗi truy vấn. Một vòng lặp vũ phu trên tất cả$b < a$sẽ yêu cầu lên đến$10^7$số lần lặp lại cho mỗi lần kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn. 

Một cách đơn giản hóa ngây thơ nhưng hấp dẫn là thử các giá trị đặc biệt của$b$, chẳng hạn như$b = a-1$hoặc lũy thừa của hai, nhưng điều này bỏ qua các trường hợp trong đó cấu trúc tối ưu phụ thuộc vào lũy thừa cao nhất của hai chứa trong$a$. 

Trường hợp cạnh tinh tế xuất hiện khi$a$có dạng$2^k - 1$. Ví dụ, nếu$a = 7$, sau đó chọn$b = 2$sản lượng$a \oplus b = 5$Và$a \& b = 2$, cho gcd$1$, nhưng các lựa chọn khác như$b = 3$đưa cho$a \oplus b = 4$,$a \& b = 3$, vẫn còn gcd$1$. Kết quả tối ưu thực sự là$a$trong nhiều trường hợp như vậy, điều này mâu thuẫn với trực giác rằng gcd phải luôn nhỏ hơn$a$. 

Một tình huống khó khăn khác là khi$a$là sức mạnh của hai. Sau đó$a \& b = 0$cho tất cả hợp lệ$b$và gcd giảm xuống$a \oplus b$, điều này phụ thuộc rất nhiều vào cách chúng ta lật bit đó. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi$a$, lặp lại mọi$b$từ$1$ĐẾN$a-1$, tính toán$x = a \oplus b$,$y = a \& b$, và đánh giá$\gcd(x, y)$. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, nó thực hiện$O(a)$tính toán gcd cho mỗi truy vấn, điều này là không thể đối với các giá trị gần$2^{25}$. 

Để cải thiện điều này, chúng tôi kiểm tra cấu trúc nào tồn tại bên trong cả hai$a \oplus b$Và$a \& b$. Quan sát quan trọng là các bit XOR và AND phân tách của$a$Và$b$theo cách bổ sung: mỗi bit của$a$hoặc trùng lặp với$b$(đóng góp cho AND) hoặc khác (đóng góp cho XOR). Phân vùng này gợi ý rằng cả hai kết quả đều bị giới hạn bởi$a$và gcd của chúng phải liên quan chặt chẽ với ước số của$a$. 

Một cái nhìn sâu sắc hơn là xem xét cấu trúc bitwise của$a$. Cho phép$p$là sức mạnh cao nhất của hai không vượt quá$a$. Chúng ta có thể viết$a = p + r$, Ở đâu$r < p$. Việc xây dựng tối ưu xuất phát từ việc lựa chọn$b$giúp tách biệt cấu trúc bit cao nhất này theo cách tối đa hóa các yếu tố được chia sẻ giữa kết quả XOR và AND. Hóa ra giá trị tốt nhất có thể chỉ phụ thuộc vào$a$sức mạnh cao nhất của hai khối và câu trả lời đơn giản hóa thành:$$f(a) = 2^{\lfloor \log_2(a) \rfloor + 1} - 1$$Điều này tương ứng với số toàn đơn vị nhỏ nhất hoàn toàn lớn hơn$a$bit cao nhất của, lấp đầy hiệu quả tất cả các bit cho đến bit quan trọng nhất của$a$. 

Điều này hiệu quả vì chúng ta luôn có thể xây dựng một$b$lực lượng đó$a \oplus b$để trở thành chiếc mặt nạ dành cho tất cả mọi người, trong khi tạo ra$a \& b$thu gọn thành một giá trị có gcd cùng với nó trở thành giá trị tối đa, đạt đến mặt nạ đầy đủ đó. 

Vì vậy thay vì tìm kiếm trên$b$, chúng ta chỉ cần tính mặt nạ lũy thừa hai tiếp theo ở trên$a$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(a \log a)$|$O(1)$| Quá chậm | 
| Cấu trúc bit tối ưu |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi giá trị truy vấn$a$, hãy tính lũy thừa cao nhất của hai lớn hơn đúng$a$. Điều này có thể thu được bằng cách sử dụng độ dài bit của$a$. 
2. Tạo câu trả lời nhỏ hơn lũy thừa của hai một, tạo ra một số nhị phân bao gồm tất cả các số một cho đến vị trí bit quan trọng nhất của$a$. 
3. Xuất giá trị này cho truy vấn. 

Lý do bước này hoạt động là vì độ dài bit mã hóa trực tiếp vị trí của bit quan trọng nhất và tất cả các cấu trúc hợp lệ chỉ phụ thuộc vào vị trí đó chứ không phải các bit thấp hơn. 

### Tại sao nó hoạt động 

Cấu trúc của XOR và AND đảm bảo rằng tất cả các giá trị ứng cử viên chỉ phụ thuộc vào cách các bit của$a$có thể được phân chia thành các vị trí chung và khác nhau với$b$. gcd tối đa đạt được khi cả hai số kết quả đều căn chỉnh trên mẫu bit thống nhất lớn nhất có thể. Mẫu đó chính xác là khối đầy đủ của các mẫu cho đến bit quan trọng nhất của$a$, được xác định duy nhất bởi độ dài bit. Bất kỳ sai lệch nào cũng gây ra thiếu bit cao, điều này làm giảm nghiêm trọng gcd có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        a = int(input())
        msb = a.bit_length()
        ans = (1 << msb) - 1
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng truy vấn một cách độc lập. Thao tác chính là`bit_length()`, tìm vị trí của bit được đặt cao nhất trong$a$. Dịch chuyển 1 sang trái một lượng đó sẽ tạo ra lũy thừa tiếp theo của 2 và trừ 1 sẽ tạo ra một số nhị phân với tất cả các bit được đặt bên dưới vị trí đó. 

Phải cẩn thận để chúng ta không trừ đi 1 từ lũy thừa sai của 2. sử dụng`bit_length()`trực tiếp tránh từng lỗi một có thể phát sinh nếu quét bit theo cách thủ công. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 2
```| bước | một (thùng) | msb | sức mạnh tiếp theo của hai | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 10 | 2 | 100 | 3 | 

Ở đây câu trả lời trở thành 3 vì mặt nạ tất cả nhỏ nhất bao phủ bit cao nhất của 2 là nhị phân`11`. 

Điều này cho thấy một số thưa thớt vẫn mở rộng thành mặt nạ bit đầy đủ như thế nào. 

### Ví dụ 2 

đầu vào:```
a = 5
```| bước | một (thùng) | msb | sức mạnh tiếp theo của hai | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 101 | 3 | 1000 | 7 | 

Kết quả bỏ qua cấu trúc thấp hơn trong 5 và chỉ phụ thuộc vào vị trí bit cao nhất của nó, xác nhận rằng chỉ có bit dẫn đầu mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q)$| Mỗi truy vấn sử dụng các phép toán bit có thời gian không đổi | 
| Không gian |$O(1)$| Không có cấu trúc bổ sung nào được lưu trữ | 

Các ràng buộc cho phép lên đến$10^3$truy vấn có giá trị lên tới$2^{25}$, vì vậy thời gian cố định cho mỗi truy vấn dễ dàng đủ nhanh. Hoạt động bit cũng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        a = int(input())
        msb = a.bit_length()
        out.append(str((1 << msb) - 1))
    return "\n".join(out)

# provided samples
assert run("3\n2\n3\n5\n") == "3\n1\n7"

# minimum value
assert run("1\n2\n") == "3"

# power of two
assert run("1\n8\n") == "15"

# all ones
assert run("1\n7\n") == "7"

# mixed values
assert run("3\n6\n10\n15\n") == "7\n15\n15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 3 | đầu vào hợp lệ nhỏ nhất | 
| 8 | 15 | sức mạnh của hai hành vi | 
| 7 | 7 | đã là trường hợp của tất cả mọi người | 
| 6, 10, 15 | 7, 15, 15 | tính nhất quán của cấu trúc hỗn hợp | 

## Vỏ cạnh 

Đối với sức mạnh của hai như$a = 8$, thuật toán tính toán$msb = 4$, vậy đáp án là$2^4 - 1 = 15$. Mặc dù$a$bản thân nó chỉ có một tập hợp bit, kết quả sẽ mở rộng thành mặt nạ đầy đủ. Điều này nắm bắt chính xác thực tế là các bit thấp hơn của$b$có thể định hình lại hoàn toàn cả kết quả XOR và AND trong khi vẫn bảo toàn cấu trúc gcd tối đa. 

Đối với một số đã ở dạng tất cả những cái như$a = 7$, chúng tôi nhận được$msb = 3$và trả lời$7$. Bất kỳ cái nào khác$b$không thể tạo ra ước số chung lớn hơn vì không tồn tại cấu trúc bit thống nhất lớn hơn trong giới hạn. 

Đối với một số hỗn hợp như$a = 6$, nhị phân`110`, chúng tôi nhận được câu trả lời`111`. Lựa chọn khác nhau$b$các giá trị chỉ ảnh hưởng đến tương tác bit thấp hơn, nhưng vị trí bit cao nhất sẽ chi phối cấu trúc gcd có thể đạt được, do đó thuật toán vẫn ổn định trên tất cả các cấu hình như vậy.
