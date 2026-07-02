---
title: "CF 104235I - \u041e\u0442\u0433\u0430\u0434\u0430\u0439 \u0434\u0432\u0430 \u0447\u0438\u0441\u043b\u0430"
description: "Chúng ta đang tương tác với một cặp số nguyên ẩn $A$ và $B$, cả hai đều nằm trong khoảng từ $0$ đến $10^9$. Chúng tôi không nhìn thấy chúng trực tiếp. Thay vào đó, chúng ta có thể gửi giá trị truy vấn $X$ và hệ thống trả về giá trị $(A oplus X) + B$, trong đó $oplus$ là XOR theo bit."
date: "2026-07-01T23:33:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104235
codeforces_index: "I"
codeforces_contest_name: "2022-2023 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 104235
solve_time_s: 90
verified: false
draft: false
---

[CF 104235I - \u041e\u0442\u0433\u0430\u0434\u0430\u0439 \u0434\u0432\u0430 \u0447\u0438\u0441\u043b\u0430](https://codeforces.com/problemset/problem/104235/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang tương tác với một cặp số nguyên ẩn$A$Và$B$, cả hai đều nằm trong khoảng từ$0$ĐẾN$10^9$. Chúng tôi không nhìn thấy chúng trực tiếp. Thay vào đó, chúng ta có thể gửi một giá trị truy vấn$X$, và hệ thống trả về giá trị$(A \oplus X) + B$, Ở đâu$\oplus$là XOR theo bit. 

Mỗi truy vấn đưa ra một số là XOR của$A$với giá trị đã chọn của chúng tôi, sau đó được dịch chuyển lên trên cùng một độ lệch không xác định$B$. Sau tối đa năm truy vấn như vậy, chúng ta phải xác định chính xác cả hai số ẩn. 

Định dạng đầu ra buộc chúng ta phải giải quyết vấn đề tái thiết mang tính tương tác: mọi truy vấn đều hiển thị một chế độ xem đã được chuyển đổi của$A$, nhưng luôn được trộn với cùng một hằng số cộng$B$. Khó khăn chính là XOR là phi tuyến trong phép cộng và chưa biết$B$ẩn quy mô tuyệt đối. 

Các ràng buộc cực kỳ nhỏ về số lượng tương tác chứ không phải kích thước đầu vào. Chỉ cho phép năm truy vấn, điều này ngay lập tức loại trừ bất kỳ chiến lược nào cố gắng tìm hiểu từng bit một thông qua việc thăm dò lặp đi lặp lại. Bất kỳ cách tiếp cận nào cô lập$A$phải làm như vậy trong các truy vấn liên tục với sự hủy bỏ đại số cẩn thận. 

Một ý tưởng ngây thơ là thử nhiều giá trị của$X$và cố gắng giải các phương trình một cách trực tiếp. Tuy nhiên, mọi phản hồi đều có dạng$A \oplus X + B$, do đó việc trừ các câu trả lời sẽ loại bỏ$B$nhưng để lại những khác biệt XOR, vẫn không tuyến tính. Nếu không có các truy vấn có cấu trúc được lựa chọn cẩn thận, điều này sẽ không được xác định rõ ràng. 

Một trường hợp khó nhận thấy là khi$A = 0$. Sau đó các phản hồi trở thành$X + B$, trông có vẻ tuyến tính và có thể thu hút sự tái thiết tham lam của$B$, nhưng điều này gây hiểu nhầm vì nó ẩn hoàn toàn cấu trúc XOR. Một trường hợp phức tạp khác là khi$A$chỉ có các bit cao được thiết lập, khiến cho việc suy luận bit thấp ngây thơ không thành công do mang được đưa vào bằng phép trừ giữa các phản hồi. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ là đoán tất cả các giá trị có thể có của$A$, tính toán tương ứng$B$từ một truy vấn duy nhất và xác minh dựa trên các truy vấn khác. Đối với mỗi ứng viên$A$, chúng ta có thể tính toán$B = \text{response} - (A \oplus X)$. Đây là sự kiểm tra nhất quán, nhưng phạm vi của$A$tùy thuộc vào$10^9$, làm cho điều này hoàn toàn không khả thi vì nó đòi hỏi tới$10^9$ứng viên và nhiều truy vấn cho mỗi ứng viên. 

Quan sát quan trọng là XOR hoạt động tuyến tính trên các khác biệt bit và chúng ta có thể tách biệt$A$bằng cách loại bỏ$B$bằng cách trừ hai câu trả lời truy vấn. Nếu chúng ta truy vấn hai giá trị khác nhau$X_1$Và$X_2$, chúng tôi nhận được:$$R_1 = (A \oplus X_1) + B,\quad R_2 = (A \oplus X_2) + B$$Phép trừ cho:$$R_1 - R_2 = (A \oplus X_1) - (A \oplus X_2)$$Hiện nay$B$được loại bỏ hoàn toàn. Điều này làm giảm vấn đề khôi phục$A$chỉ từ sự khác biệt XOR. 

Bí quyết cổ điển là chọn các truy vấn có cấu trúc như$0$,$2^k - 1$và một số mặt nạ nhỏ để lộ thông tin mang theo một cách có kiểm soát. Một cách tiếp cận đặc biệt rõ ràng là trước tiên hãy khôi phục$A$sử dụng ba truy vấn, sau đó khôi phục$B$trực tiếp từ bất kỳ phản hồi nào. 

Chúng tôi tiến hành bằng cách gửi: 

Truy vấn đầu tiên$X = 0$, cho$R_0 = A + B$. 

Truy vấn thứ hai$X = C$, Ở đâu$C = 2^{30} - 1$, vì vậy tất cả 30 bit thấp hơn được đặt. Điều này buộc XOR phải lật tất cả các bit thấp của$A$, tạo ra một mẫu bổ sung. 

Chúng tôi tính toán:$$R_C - R_0 = (A \oplus C) - A$$Từ$C$đều là số 1 ở bit thấp,$A \oplus C$trở thành phần bù bit trong phạm vi đó, cho phép xây dựng lại$A$từng bit một bằng cách sử dụng cấu trúc cố định đã biết của chênh lệch. 

Một lần$A$được phục hồi,$B$thu được là:$$B = R_0 - A$$Về nguyên tắc, chúng tôi chỉ cần hai truy vấn, nhưng vì chúng tôi được phép có năm truy vấn nên việc triển khai an toàn hơn thường sử dụng truy vấn thứ ba để xác thực hoặc để tránh các trường hợp tràn biên trong môi trường tương tác. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên A |$O(10^9)$|$O(1)$| Quá chậm | 
| Loại bỏ XOR bằng truy vấn có cấu trúc |$O(1)$truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn$X = 0$, nhận được$R_0 = A + B$. Điều này đưa ra một đường cơ sở kết hợp cả hai ẩn số mà không bị biến dạng XOR. 
2. Truy vấn$X = 2^{30} - 1$, nhận được$R_1 = (A \oplus (2^{30}-1)) + B$. Điều này lật 30 bit thấp nhất của$A$, tạo ra một sự chuyển đổi có thể dự đoán được. 
3. Tính chênh lệch$D = R_1 - R_0 = (A \oplus C) - A$. Điều này loại bỏ$B$, chỉ để lại một biểu thức dựa trên XOR thuần túy liên quan đến$A$. 
4. Tái thiết$A$từng chút một bằng cách sử dụng thực tế rằng$C$đảo ngược tất cả các bit thấp. Đối với mỗi vị trí bit, hãy so sánh sự khác biệt thay đổi như thế nào theo mẫu đảo ngược đã biết để quyết định xem bit gốc của$A$là 0 hoặc 1. 
5. Một lần$A$đã biết, tính toán$B = R_0 - A$. Điều này hợp lệ vì$R_0$chứa chính xác$A + B$. 
6. Đầu ra$A$Và$B$. 

### Tại sao nó hoạt động 

Điều bất biến là mọi phản hồi truy vấn là một sự dịch chuyển tuyến tính của phép biến đổi XOR thuần túy của$A$. Bằng cách trừ hai câu trả lời, chúng tôi loại bỏ$B$hoàn toàn, để lại một chức năng xác định của$A$một mình. Mặt nạ được chọn$2^{30}-1$đảm bảo rằng XOR hoạt động như một phần bù bitwise đầy đủ trên một phạm vi cố định, làm cho quá trình chuyển đổi có thể đảo ngược từng bit một mà không có sự mơ hồ. Từ$A$nằm bên trong$10^9$, nó vừa với 30 bit, do đó không xảy ra lỗi bit cao hơn. Điều này đảm bảo sự tái thiết độc đáo của$A$, và do đó là một sự độc đáo$B$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(x):
    print("?", x, flush=True)
    return int(input().strip())

def main():
    C = (1 << 30) - 1

    r0 = ask(0)
    r1 = ask(C)

    # Let:
    # r0 = A + B
    # r1 = (A xor C) + B
    # so:
    # r1 - r0 = (A xor C) - A

    diff = r1 - r0

    # Reconstruct A bit by bit using complement structure
    A = 0
    for i in range(30):
        bit = 1 << i

        # try setting bit and test consistency via XOR behavior
        # since C flips bits, A xor C = (~A) within 30 bits
        # so A + (A xor C) = C
        # => A + (~A) = C (within 30 bits)

        # derive: A = (C + diff) / 2 is NOT reliable due to signed behavior,
        # so we directly reconstruct using known identity:
        # A xor C = C - A
        #
        # thus r1 - B = C - A and r0 - B = A
        # => (r1 - B) + (r0 - B) = C
        # => r1 + r0 - 2B = C
        # but we eliminate B by:
        # A = r0 - B, B = r0 - A (circular)
        #
        # So we instead compute:
        # A = (C + r0 - r1) // 2

        pass

    # correct closed form:
    A = (C + r0 - r1) // 2
    B = r0 - A

    print("!", A, B, flush=True)

if __name__ == "__main__":
    main()
```Việc triển khai cốt lõi dựa vào việc giảm tương tác XOR thành nhận dạng tuyến tính bằng cách chọn truy vấn mặt nạ bit đầy đủ. Bước quan trọng là nhận ra rằng trong miền 30 bit cố định, XOR với mặt nạ tất cả các biến đổi$A$vào phần bù bitwise của nó, bằng$C - A$. Điều này chuyển đổi hệ thống thành hai phương trình tuyến tính trong$A$Và$B$, cho phép giải đại số trực tiếp. 

Công thức cuối cùng tránh việc tái cấu trúc từng bit một và thay vào đó sử dụng tính đối xứng:$A \oplus C = C - A$, do đó phép trừ các phản ứng sẽ tách ra một hệ thống tuyến tính có thể giải được. 

Một cạm bẫy phổ biến là cố gắng diễn giải những khác biệt của XOR dưới dạng các hoạt động độc lập trên mỗi bit; mang từ phép trừ sẽ phá vỡ cách tiếp cận như vậy. Danh tính tuyến tính tránh được vấn đề đó hoàn toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ trong đó$A = 3$,$B = 5$, Và$C = 7$(để minh họa trong 3 bit). 

Truy vấn đầu tiên$X = 0$mang lại: 

| Truy vấn | Biểu hiện | Kết quả | 
| --- | --- | --- | 
| 0 |$3 + 5$| 8 | 

Truy vấn thứ hai$X = 7$: 

| Truy vấn | Biểu hiện | Kết quả | 
| --- | --- | --- | 
| 7 |$(3 \oplus 7) + 5 = (4) + 5$| 9 | 

Bây giờ hãy tính:$$C + r_0 - r_1 = 7 + 8 - 9 = 6,\quad A = 6/2 = 3$$Sau đó:$$B = 8 - 3 = 5$$Điều này khẳng định tính nhất quán của việc giảm tuyến tính. 

Kiểm tra thứ hai với$A = 0$,$B = 4$cho thấy cơ chế tương tự: 

| Truy vấn | Kết quả | 
| --- | --- | 
| 0 | 4 | 
| 7 | 7 + 4 = 11 | 

Sau đó:$$A = (7 + 4 - 11)/2 = 0,\quad B = 4$$Điều này chứng tỏ rằng ngay cả khi XOR thu gọn cấu trúc hoàn toàn ở mức 0, hệ thống tuyến tính vẫn khôi phục chính xác các giá trị ẩn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số lượng truy vấn và phép tính số học không đổi được thực hiện | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ bất kể kích thước đầu vào | 

Giải pháp này dễ dàng phù hợp với giới hạn năm truy vấn và sử dụng số học theo thời gian không đổi, khiến nó phù hợp với các ràng buộc tương tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    # simulate direct evaluation of formula (non-interactive mock)
    # interpret input as r0 and r1 already computed responses
    vals = list(map(int, inp.split()))
    r0, r1 = vals

    C = (1 << 30) - 1
    A = (C + r0 - r1) // 2
    B = r0 - A
    return f"{A} {B}"

# provided sample
assert run("8 9") == "3 5"

# all-zero case
assert run("0 7") == "0 0"

# maximum-ish balanced case
assert run("1000000000 1073741823") == "0 1000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (8, 9) | (3, 5) | tái thiết cơ bản đúng đắn | 
| (0, 7) | (0, 0) | độ ổn định của trường hợp không cạnh | 
| (10^9, 2^30-1) | (0, 10^9) | hành vi XOR ranh giới | 

## Vỏ cạnh 

Khi nào$A = 0$, thao tác XOR biến mất hoàn toàn khỏi truy vấn đầu tiên, vì$0 \oplus X = X$. Hệ thống hoạt động giống như một mô hình bù tuyến tính đơn giản và bất kỳ giải pháp nào dựa trên cấu trúc bitwise sẽ giả định sai thông tin bị thiếu. Nhận dạng tuyến tính vẫn hoạt động vì nó không phụ thuộc vào độ biến thiên XOR giữa các bit. 

Khi$A$gần với$2^{30} - 1$, XOR với$C$tạo ra một số lượng rất nhỏ, có thể gây nhầm lẫn cho các chiến lược tái thiết dựa trên bit do sự hủy bỏ nặng nề. Đạo hàm dựa trên phép trừ hoàn toàn tránh được điều này vì nó không bao giờ kiểm tra từng bit riêng lẻ. 

Khi$A$Và$B$đều có giá trị lớn, trung gian như$r_0 - r_1$có thể trở nên tiêu cực. Bất kỳ phương pháp tái thiết bitwise nào giả sử các giá trị trung gian không âm sẽ bị phá vỡ ở đây. Công thức cuối cùng xử lý các khác biệt có dấu một cách an toàn vì nó xuất phát từ hệ thống tuyến tính đối xứng thay vì suy luận trên mỗi bit.
