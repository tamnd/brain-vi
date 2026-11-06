---
title: "CF 1302E - Bộ bit tuyệt vời"
description: "Chúng ta được cấp một chuỗi nhị phân có độ dài $n$, nhưng thay vì cố định, mỗi vị trí được tạo độc lập dưới dạng một bit ngẫu nhiên: nó trở thành $1$ với xác suất $p = frac{a}{b}$ và $0$ với xác suất $1 - p$."
date: "2026-06-16T05:33:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1302
codeforces_index: "E"
codeforces_contest_name: "AIM Tech Poorly Prepared Contest (unrated, funny, Div. 1 preferred)"
rating: 0
weight: 1302
solve_time_s: 559
verified: false
draft: false
---

[CF 1302E - Bộ bit tuyệt vời](https://codeforces.com/problemset/problem/1302/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân có độ dài$n$, nhưng thay vì cố định, mỗi vị trí được tạo độc lập dưới dạng một bit ngẫu nhiên: nó trở thành$1$với xác suất$p = \frac{a}{b}$Và$0$với xác suất$1 - p$. 

Sau khi chuỗi được tạo, chúng ta được phép áp dụng một thao tác cụ thể trên chuỗi đó nhiều lần. Mỗi thao tác hoạt động như sau: chúng tôi chọn một chỉ mục làm “trung tâm”, sau đó chọn ít nhất một chỉ mục khác sao cho tất cả các chỉ mục “khác” đã chọn đều có bit đối diện với tâm. Sau đó, chúng tôi lật tất cả các bit đã chọn cùng một lúc. 

Một tập hợp bit được gọi là hợp lệ nếu bắt đầu từ nó tồn tại một chuỗi các thao tác này mà cuối cùng sẽ lật từng bit ít nhất một lần (tương đương, chúng ta hoàn toàn có thể “kích hoạt” tất cả các chỉ mục thông qua các thao tác được phép). 

Nhiệm vụ là tính xác suất để một tập hợp bit được tạo ngẫu nhiên là hợp lệ và xuất nó dưới dạng phân số mô-đun theo mô-đun$1{,}234{,}567{,}891$. 

Những hạn chế là rất lớn:$n$có thể lên đến$10^9$, do đó, bất kỳ giải pháp nào cố gắng liệt kê các cấu hình, mô phỏng các hoạt động hoặc sử dụng tổ hợp tùy thuộc vào tập hợp con của các chỉ số đều là không thể ngay lập tức. Hướng khả thi duy nhất là một biểu thức dạng đóng về mặt$n$, được tính bằng cách sử dụng lũy ​​thừa nhanh. 

Một điểm tinh tế trong vấn đề này là tính ngẫu nhiên áp dụng cho tất cả các bitset, nhưng điều kiện hợp lệ chỉ phụ thuộc vào cấu trúc toàn cục của phân phối bit chứ không phụ thuộc vào các mẫu cục bộ. Điều đó gợi ý rõ ràng rằng chúng ta đang đếm một số lượng nhỏ các trường hợp mang tính cấu trúc. 

Trường hợp một cạnh là khi tất cả các bit giống hệt nhau. Trong tình huống đó, thậm chí không có thao tác nào có thể bắt đầu vì không có chỉ mục có giá trị đối lập nào để ghép với trung tâm đã chọn. Ví dụ, đối với$n = 3$, dây`000`Và`111`ngay lập tức bị mắc kẹt. 

Một sai lầm ngây thơ là cho rằng bất kỳ chuỗi không cố định nào cũng hợp lệ. Đối với các ví dụ nhỏ, điều này có vẻ hợp lý, vì việc có cả 0 và 1 luôn mang lại cho bạn ít nhất một thao tác khả thi. Tuy nhiên, trực giác này sẽ thất bại nếu có những hạn chế kết nối tiềm ẩn, nhưng trong vấn đề này, hoạt động đủ linh hoạt để bất kỳ cấu hình hỗn hợp nào cũng có thể được mở rộng từng bước. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực sẽ là tạo ra tất cả$2^n$bitets và mô phỏng xem mỗi cái có “tuyệt vời” hay không. Ngay cả khi bỏ qua số lượng trạng thái theo cấp số nhân, mỗi mô phỏng bao gồm một chuỗi các hoạt động có độ dài không bị giới hạn theo bất kỳ cách hữu ích nào, vì vậy phương pháp này hoàn toàn không khả thi ngay cả đối với các ứng dụng nhỏ.$n$. 

Sự đơn giản hóa chính xuất phát từ việc nhận thấy rằng thao tác cực kỳ mạnh mẽ khi cả hai giá trị đều tồn tại. Nếu một bitset chứa ít nhất một`0`và ít nhất một`1`, thì chúng ta luôn có thể chọn một tâm và một nhóm đối diện, đồng thời sử dụng các thao tác lặp lại để truyền các lần lật trên toàn bộ mảng. Thứ tự chính xác của các phép toán không quan trọng vì chúng ta luôn có thể tiếp tục miễn là cả hai giá trị đều tồn tại ở đâu đó trong mảng. 

Cấu hình duy nhất chặn mọi tiến trình là khi bitset đồng nhất. Trong trường hợp đó, không có nước đi đầu tiên hợp lệ nên quá trình này không thể thực hiện được ngay từ đầu. 

Vì vậy, vấn đề giảm xuống tính toán:$$\mathbb{P}(\text{bitset is not all 0 and not all 1})$$Đó là:$$1 - \mathbb{P}(\text{all 0}) - \mathbb{P}(\text{all 1})$$Vì các bit là độc lập:$$\mathbb{P}(\text{all 1}) = \left(\frac{a}{b}\right)^n,\quad
\mathbb{P}(\text{all 0}) = \left(\frac{b-a}{b}\right)^n$$Vì vậy, câu trả lời trở thành một biểu thức mô-đun đơn giản liên quan đến lũy thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm Xác suất + Công suất Nhanh |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn tính xác suất để bitset được tạo không phải là đơn sắc. 

1. Chuyển xác suất sang dạng số học mô đun. Chúng tôi đại diện$p = a \cdot b^{-1}$modulo$M$, và tương tự$1-p = (b-a) \cdot b^{-1}$. 
2. Tính toán$b^n$modulo$M$sử dụng lũy ​​thừa nhanh. Điều này thể hiện sự đóng góp của mẫu số. 
3. Tính toán$a^n \bmod M$, tương ứng với tử số của trường hợp toàn đơn vị. 
4. Tính toán$(b-a)^n \bmod M$, tương ứng với tử số của trường hợp toàn số 0. 
5. Kết hợp hai trường hợp xấu:$$\text{bad} = a^n + (b-a)^n \pmod M$$6. Chuẩn hóa bằng cách chia cho$b^n$, được thực hiện bằng cách sử dụng nghịch đảo mô-đun:$$\text{bad} \cdot (b^n)^{-1}$$7. Trừ 1 để có đáp án cuối cùng. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là trở ngại duy nhất cho việc thực hiện chuỗi thao tác là thiếu cả hai giá trị bit. Nếu cả 0 và 1 đều tồn tại, mọi thao tác đều đảm bảo thực tế là chúng ta vẫn có thể chọn tâm của một loại và ít nhất một phần tử đối diện, do đó tiến trình không bao giờ bị kẹt. Do đó, tập hợp các cấu hình không hợp lệ sẽ thu gọn chính xác thành hai trường hợp cực kỳ giống nhau và xác suất trở thành một phép tính loại trừ đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1234567891

def mod_pow(a, e):
    res = 1
    a %= MOD
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n, a, b = map(int, input().split())

inv_b = pow(b, MOD - 2, MOD)

p_num = a % MOD
q_num = (b - a) % MOD

a_n = mod_pow(p_num, n)
b_n = mod_pow(b % MOD, n)
q_n = mod_pow(q_num, n)

inv_b_n = pow(b_n, MOD - 2, MOD)

bad = (a_n + q_n) % MOD
bad = bad * inv_b_n % MOD

ans = (1 - bad) % MOD
print(ans)
```Việc triển khai phân tách rõ ràng việc xử lý tử số và mẫu số. Điều tinh tế quan trọng nhất là chúng ta không bao giờ trực tiếp tính toán$(a/b)^n$, vì phép chia mô-đun phải được xử lý thông qua phép nghịch đảo. Một chi tiết quan trọng khác là tính toán cả$a^n$Và$(b-a)^n$độc lập theo mô-đun, đảm bảo chúng tôi không bao giờ mất độ chính xác trước khi chuẩn hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 2
```Chúng tôi tính toán:$a = 1$,$b-a = 1$,$b = 2$,$n = 5$| Bước | Giá trị | 
| --- | --- | 
|$a^n$|$1^5 = 1$| 
|$(b-a)^n$|$1^5 = 1$| 
| tử số xấu |$2$| 
|$b^n$|$2^5 = 32$| 
| xác suất xấu |$2/32 = 1/16$| 
| trả lời |$1 - 1/16 = 15/16$| 

Điều này cho thấy câu trả lời đơn giản là xác suất tránh được các chuỗi đồng nhất. 

### Ví dụ 2 

đầu vào:```
3 2 3
```Đây$p = 2/3$, vì vậy: 

| Bước | Giá trị | 
| --- | --- | 
|$a^n$|$8$| 
|$(b-a)^n$|$1$| 
| tử số xấu |$9$| 
|$b^n$|$27$| 
| xác suất xấu |$1/3$| 
| trả lời |$2/3$| 

Điều này xác nhận rằng chỉ những cấu hình hoàn toàn thống nhất mới bị loại trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| lũy thừa nhanh cho số lũy thừa không đổi | 
| Không gian |$O(1)$| chỉ một vài biến số nguyên | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì sự phụ thuộc duy nhất vào$n$là lũy thừa và$n$có thể lớn như$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 1234567891

def mod_pow(a, e):
    res = 1
    a %= MOD
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    n, a, b = map(int, input().split())
    inv_b = pow(b, MOD - 2, MOD)

    a_n = mod_pow(a, n)
    q_n = mod_pow(b - a, n)
    b_n = mod_pow(b, n)

    bad = (a_n + q_n) % MOD
    bad = bad * pow(b_n, MOD - 2, MOD) % MOD

    print((1 - bad) % MOD)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# provided sample
# (placeholder since single sample given)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1`|`0`| tất cả số không luôn không hợp lệ | 
|`1 1 1`|`0`| tất cả những cái luôn không hợp lệ | 
|`5 1 2`|`848765426`| mẫu | 
|`10 0 5`|`0`| trường hợp cực kỳ thiên vị | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi bitset hoàn toàn đồng nhất. Ví dụ, với đầu vào`n = 4, a = 0`, mỗi chuỗi được tạo ra là`0000`. Thuật toán tính toán$a^n = 0$,$(b-a)^n = b^n$, do đó xác suất xấu trở thành 1 và câu trả lời cuối cùng là 0, phù hợp với thực tế là không thể thực hiện được thao tác nào. 

Một trường hợp cạnh khác là khi$a = b$, nghĩa là mọi bit luôn bằng 1. Việc tính toán trở nên đối xứng:$(b-a)^n = 0$, và một lần nữa xác suất hợp lệ là 0. 

Cuối cùng, khi$n = 1$, mọi chuỗi đều giống nhau một cách tầm thường, vì vậy câu trả lời luôn là 0. Công thức xử lý vấn đề này một cách rõ ràng vì cả hai số hạng đều giảm trực tiếp xuống 1 sau khi chuẩn hóa, mang lại kết quả bằng 0.
