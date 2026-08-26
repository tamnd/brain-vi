---
title: "CF 104349B - Ít SigDig nhất"
description: "Chúng tôi được cung cấp một chuỗi các trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp hai số nguyên, $n$ và $m$, và về mặt khái niệm, chúng tôi tạo thành số $n cdot 245^m$. Nhiệm vụ không phải là tính giá trị đầy đủ này mà chỉ xác định chữ số cuối cùng của nó trong cơ số 10."
date: "2026-07-01T18:14:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104349
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #13 (Boombastic-Forces)"
rating: 0
weight: 104349
solve_time_s: 89
verified: false
draft: false
---

[CF 104349B - SigDig ít nhất](https://codeforces.com/problemset/problem/104349/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp hai số nguyên,$n$Và$m$, và chúng ta hình thành một cách khái niệm số$n \cdot 245^m$. Nhiệm vụ không phải là tính giá trị đầy đủ này mà chỉ xác định chữ số cuối cùng của nó trong cơ số 10. 

Vì vậy, câu hỏi đầu ra thực sự là về chữ số hàng đơn vị của sản phẩm trong đó một thừa số là cơ số cố định$n$và thừa số còn lại là lũy thừa lớn của 245. Vì chỉ yêu cầu chữ số cuối cùng nên tất cả các giá trị vị trí cao hơn đều không liên quan và có thể bị bỏ qua hoàn toàn. 

Các ràng buộc rất lớn: cả hai$n$Và$m$có thể đi lên$10^9$, và có tới$10^3$trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào cố gắng tính toán$245^m$trực tiếp, ngay cả khi tính lũy thừa nhanh, là không cần thiết vì bản thân số mũ đã lớn nhưng đại lượng mà chúng ta quan tâm lại có cấu trúc cực kỳ nhỏ, chỉ có một chữ số. 

Ý nghĩa chính của các ràng buộc là giải pháp phải giảm mọi thứ về số học mô-đun chỉ ở chữ số cuối cùng. Mọi nỗ lực xử lý số nguyên đầy đủ hoặc thậm chí toàn bộ lũy thừa sẽ lãng phí và không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 0$. Trong trường hợp này tích luôn bằng 0 bất kể$m$, vì vậy câu trả lời luôn là 0. Một trường hợp đặc biệt quan trọng khác là khi$m = 0$. Sau đó$245^0 = 1$, vì vậy câu trả lời đơn giản là chữ số cuối cùng của$n$. Những trường hợp này rất dễ bị bỏ qua nếu người ta chỉ tập trung vào số mũ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tính toán$245^m$một cách rõ ràng và nhân với$n$, sau đó trích xuất chữ số cuối cùng. Ngay cả khi chúng ta chuyển sang lũy ​​thừa bằng bình phương, chúng ta vẫn đang làm việc với các số nguyên lớn một cách không cần thiết. Trong Python điều này có thể tồn tại ở những đầu vào nhỏ, nhưng đối với$m$lên tới$10^9$, các giá trị trung gian tăng nhanh trừ khi giảm mạnh và bước nhân trở nên vô nghĩa vì chỉ cần chữ số cuối cùng. 

Quan sát quan trọng là các chữ số cuối cùng hoạt động theo chu kỳ khi nhân. Vì chúng ta chỉ quan tâm đến chữ số cuối cùng của sản phẩm cuối cùng nên chúng ta có thể giảm cả hai$n$Và$245$modulo 10 ngay lập tức. Điều này biến vấn đề thành tính toán:$$(n \bmod 10) \cdot (5^m \bmod 10)$$vì 245 có tận cùng là 5. 

Bây giờ cấu trúc trở nên rất đơn giản. Bất kỳ lũy thừa nào của 5 đều có chữ số cuối cùng ổn định: khi chúng ta nhân 5 với chính nó dù chỉ một lần, kết quả luôn có kết quả là 5. Vậy với mọi$m \ge 1$,$5^m$kết thúc sau 5, trong khi$5^0 = 1$. 

Điều này làm giảm toàn bộ vấn đề về thời gian không đổi cho mỗi quyết định trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m)$hoặc tệ hơn trong mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quy vấn đề về số học có chữ số cuối cùng và xử lý hành vi số mũ của 5 một cách rõ ràng. 

1. Đọc$n$Và$m$cho từng trường hợp thử nghiệm. Chỉ có chữ số cuối cùng của chúng là quan trọng, nên chúng ta tính ngay$n \% 10$. 
2. Xử lý trường hợp đặc biệt$n = 0$. Nếu thừa số đầu tiên bằng 0 thì tích luôn bằng 0, vì vậy chúng ta có thể xuất ra 0 mà không cần tính toán thêm. 
3. Kiểm tra xem$m = 0$. Nếu vậy, biểu thức trở thành$n \cdot 1$, vì vậy câu trả lời chỉ đơn giản là$n \% 10$. 
4. Nếu$m \ge 1$, thay thế$245^m$với hành vi chữ số cuối cùng của nó. Vì 245 có tận cùng là 5 và$5^m$kết thúc bằng 5 cho tất cả tích cực$m$, yếu tố thứ hai đóng góp một chữ số cuối cùng cố định là 5. 
5. Nhân chữ số cuối cùng của$n$bằng 5 và xuất ra chữ số cuối cùng của tích đó. 

Ý tưởng chính là phép lũy thừa sẽ chuyển thành một mẫu không đổi khi chúng ta giảm xuống các chữ số cuối cùng. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ việc chữ số cuối cùng của tích chỉ phụ thuộc vào chữ số cuối cùng của các thừa số của nó. Về mặt hình thức, nếu$a \equiv a' \pmod{10}$Và$b \equiv b' \pmod{10}$, sau đó$ab \equiv a'b' \pmod{10}$. Điều này cho phép chúng ta thay thế$n$với$n \% 10$Và$245^m$với$(5^m \% 10)$mà không ảnh hưởng đến câu trả lời cuối cùng. 

Thuộc tính thứ hai là lũy thừa của 5 ổn định modulo 10: với mọi$m \ge 1$,$5^m \equiv 5 \pmod{10}$. Điều này loại bỏ sự cần thiết của bất kỳ logic lũy thừa nào. Do đó, thuật toán được đảm bảo tạo ra chữ số cuối cùng giống như biểu thức ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())

        if n == 0:
            print(0)
            continue

        last_n = n % 10

        if m == 0:
            print(last_n)
        else:
            print((last_n * 5) % 10)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc giảm số học về chữ số cuối cùng. Logic phân nhánh duy nhất là tách$m = 0$trường hợp từ$m \ge 1$, bởi vì nhận dạng lũy ​​thừa khác nhau ở đó. Phép nhân với 5 có thể thực hiện trực tiếp một cách an toàn vì chúng ta chỉ quan tâm đến chữ số cuối cùng. 

Sai lầm phổ biến nhất là cố gắng tính toán$245^m$sử dụng lũy ​​thừa mà không giảm modulo 10 sớm. Đó là công việc không cần thiết và che khuất hành vi tuần hoàn đơn giản của chữ số cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét hai trường hợp đại diện. 

Đầu vào đầu tiên:$n = 31, m = 1$Chúng tôi chỉ theo dõi các chữ số cuối cùng. 

| Bước | n % 10 | m | 5^m mod 10 | kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | - | - | 
| tính toán | 1 | 1 | 5 | 1 × 5 = 5 | 

Đầu ra là 5, phù hợp với thực tế là$31 \cdot 245$kết thúc ở 5. 

Đầu vào thứ hai:$n = 12, m = 2$| Bước | n % 10 | m | 5^m mod 10 | kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 2 | 2 | - | - | 
| tính toán | 2 | 2 | 5 | 2 × 5 = 10 → 0 | 

Đầu ra là 0, cho thấy phép nhân với 5 tạo ra chữ số cuối bằng 0 khi hệ số còn lại là số chẵn. 

Những ví dụ này xác nhận rằng số mũ không có tác dụng gì ngoài việc xác định liệu chúng ta có sử dụng 1 hay không (khi$m=0$) hoặc 5 (khi$m>0$). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm chỉ yêu cầu số học và phân nhánh theo thời gian không đổi | 
| Không gian |$O(1)$| Không có bộ nhớ bổ sung ngoài các biến | 

Giải pháp này nằm trong giới hạn vì ngay cả đối với$10^3$trường hợp thử nghiệm, chúng tôi chỉ thực hiện một vài phép tính số nguyên cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose  # harmless import
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())

            if n == 0:
                print(0)
                continue

            last_n = n % 10

            if m == 0:
                print(last_n)
            else:
                print((last_n * 5) % 10)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("3\n1 1\n2 2\n12 2\n") == "5\n0\n0"

# minimum input
assert run("1\n0 0\n") == "0"

# m = 0 case
assert run("1\n987 0\n") == "7"

# large n, m > 0
assert run("1\n999999999 1000000000\n") == "5"

# even n with m > 0 leading to zero
assert run("1\n20 5\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 0 | trường hợp không cạnh | 
| 987 0 | 7 | trường hợp quyền lực nhận dạng | 
| 999999999 1000000000 | 5 | độ ổn định số mũ lớn | 
| 20 5 | 0 | hành vi chữ số chẵn × 5 | 

## Vỏ cạnh 

cho$n = 0, m = 0$, thuật toán ngay lập tức trả về 0 do kiểm tra bằng 0, khớp$0 \cdot 1 = 0$. 

Vì$n = 987, m = 0$, chúng ta bỏ qua logic nguồn và trả về$987 \% 10 = 7$, xử lý chính xác số mũ nhận dạng. 

Vì$n = 20, m = 5$, chúng ta lấy chữ số cuối cùng là 0 và nhân với 5, ra 0. Mặc dù số mũ lớn nhưng việc rút gọn giúp kết quả ngay lập tức và ổn định. 

Đối với rất lớn$m$, chẳng hạn như$10^9$, thuật toán không thử lũy thừa. Nó dựa trên thực tế là bất kỳ lũy thừa dương nào của 5 đều có chữ số cuối cùng là 5, do đó phép tính vẫn giữ nguyên thời gian không đổi bất kể độ lớn.
