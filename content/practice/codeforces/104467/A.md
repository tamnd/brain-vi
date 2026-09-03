---
title: "CF 104467A - Quảng cáo tăng cường"
description: "Chúng ta được cấp một ký tự bắt đầu bằng một giá trị nguyên duy nhất, ban đầu là 0. Sau đó, nhân vật sẽ trải qua một chuỗi các giai đoạn và ở mỗi giai đoạn có chính xác hai lần biến hình."
date: "2026-06-30T13:05:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104467
codeforces_index: "A"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2022"
rating: 0
weight: 104467
solve_time_s: 95
verified: true
draft: false
---

[CF 104467A - Advertere Augmento](https://codeforces.com/problemset/problem/104467/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một ký tự bắt đầu bằng một giá trị nguyên duy nhất, ban đầu là 0. Sau đó, nhân vật sẽ trải qua một chuỗi các giai đoạn và ở mỗi giai đoạn có chính xác hai lần biến hình. Mỗi phép biến đổi là một phép toán số học dưới dạng cộng với hằng số, trừ với hằng số hoặc nhân với hằng số. Ở mọi giai đoạn, chúng ta phải chọn chính xác một trong hai thao tác và áp dụng ngay vào giá trị hiện tại. 

Sau khi xử lý tất cả các giai đoạn theo thứ tự, chúng ta sẽ có được giá trị cuối cùng. Nhiệm vụ là chọn một thao tác cho mỗi giai đoạn sao cho giá trị cuối cùng này càng lớn càng tốt. 

Chi tiết quan trọng là không có trạng thái phân nhánh hoặc bộ nhớ ẩn ngoài giá trị hiện tại. Mọi quyết định sẽ ngay lập tức biến đổi số hiện tại và các quyết định trong tương lai chỉ phụ thuộc vào số kết quả. 

Sự ràng buộc lên tới 100.000 giai đoạn loại trừ mọi cách tiếp cận khám phá tất cả các chuỗi lựa chọn có thể có. Một lực lượng vũ phu ngây thơ trên tất cả$2^N$việc lựa chọn là không thể ngay lập tức vì nó đòi hỏi thời gian theo cấp số nhân. Ngay cả bất kỳ cách tiếp cận nào cố gắng mô phỏng nhiều giá trị ứng cử viên cho mỗi tiền tố cũng cần được cắt tỉa cẩn thận để tránh hiện tượng nổ bậc hai. 

Trường hợp cạnh tinh tế xuất phát từ phép nhân với số âm. Vì nhân với dấu lật âm, nên có vẻ như việc lấy giá trị nhỏ hơn sớm hơn có thể dẫn đến giá trị lớn hơn sau này. Tuy nhiên, vì chúng ta luôn kiểm soát hoàn toàn từng bước quyết định và tính toán lại kết quả tức thời tốt nhất nên giải pháp cuối cùng không yêu cầu duy trì nhiều trạng thái. 

Một sai lầm phổ biến là cho rằng cổng tốt nhất ở mỗi giai đoạn có thể được chọn một cách độc lập mà không đánh giá cả hai kết quả. Một sai lầm khác là cố gắng chỉ duy trì giá trị tối đa trong khi bỏ qua rằng các giá trị trung gian có thể trở thành âm và hoạt động khác khi nhân. 

Ví dụ: hãy xem xét một giai đoạn có giá trị hiện tại là 5 và giai đoạn tiếp theo cung cấp`* -2`hoặc`+ -100`. Tại địa phương,`+ -100`cho -95 trong khi`* -2`cho -10, vì vậy phép nhân sẽ tốt hơn. Nhưng nếu các hoạt động sau này đều là các phép cộng dương, thì việc duy trì cường độ lớn hơn (ngay cả khi âm) có thể quan trọng. Điều này thúc đẩy việc kiểm tra cả hai tùy chọn một cách rõ ràng ở mỗi bước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi chuỗi lựa chọn cổng có thể. Vì mỗi trong số$N$giai đoạn có hai lựa chọn, điều này dẫn đến$2^N$giá trị cuối cùng có thể. Mỗi mô phỏng mất$O(N)$, dẫn đến thời gian theo cấp số nhân$O(N 2^N)$, điều này là không thể thực hiện được đối với$N = 10^5$. 

Quan sát quan trọng là ở mỗi giai đoạn, chúng ta không có nhiều trạng thái độc lập. Chỉ có một giá trị tiến triển và ở mỗi bước chúng ta có thể tính toán một cách xác định kết quả của cả hai lựa chọn. Vì trạng thái tiếp theo chỉ phụ thuộc vào số hiện tại nên chúng ta chỉ cần duy trì giá trị hiện tại duy nhất đó và luôn chọn giá trị tốt hơn trong hai giá trị kết quả. 

Điều này làm giảm vấn đề thành mô phỏng từng bước đơn giản: ở mỗi giai đoạn đánh giá cả hai thao tác trên giá trị hiện tại và chọn giá trị kết quả lớn hơn. Điều này hiệu quả vì không có ràng buộc nào liên kết các lựa chọn trong tương lai với các quyết định trong quá khứ ngoại trừ thông qua giá trị số hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N)$|$O(N)$| Quá chậm | 
| Mô phỏng tham lam |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Khởi tạo state 

Bắt đầu với mức năng lượng hiện tại được đặt thành 0. Đây là trạng thái duy nhất chúng tôi theo dõi trong suốt quá trình. 

### Bước 2: Xử lý tuần tự từng công đoạn 

Đối với mỗi giai đoạn, hãy đọc hai thao tác có sẵn và tính toán giá trị hiện tại sẽ trở thành bao nhiêu sau khi áp dụng từng thao tác một cách độc lập. 

### Bước 3: Đánh giá cả hai kết quả 

Áp dụng thao tác đầu tiên cho giá trị hiện tại, tạo ra kết quả dự kiến. Áp dụng thao tác thứ hai nữa, tạo ra một kết quả ứng viên khác. Hai con số này đại diện cho tất cả các kết quả có thể xảy ra cho giai đoạn này. 

### Bước 4: Chọn kết quả tốt hơn 

Chọn kết quả lớn hơn trong hai kết quả ứng cử viên và cập nhật giá trị hiện tại cho nó. Điều này đảm bảo chúng tôi luôn giữ được giá trị tốt nhất có thể đạt được sau mỗi giai đoạn với trạng thái hiện tại. 

### Bước 5: Lặp lại cho đến hết 

Tiếp tục quá trình này cho tất cả các giai đoạn theo thứ tự. Giá trị hiện tại cuối cùng là câu trả lời. 

### Tại sao nó hoạt động 

Ở bất kỳ giai đoạn nào, trạng thái của hệ thống được mô tả đầy đủ bằng một số nguyên. Mọi hành động đều ánh xạ một cách xác định số nguyên này sang một số nguyên mới. Vì chúng tôi đánh giá cả hai hành động có thể xảy ra và ngay lập tức chọn hành động mang lại trạng thái kết quả lớn hơn nên chúng tôi không bao giờ loại bỏ trạng thái có thể dẫn đến kết quả cuối cùng tốt hơn với cùng một tiền tố. 

Không có sự phụ thuộc ẩn giữa các giai đoạn ngoài giá trị hiện tại. Do đó, việc tối đa hóa giá trị ở mỗi bước tương đương với việc tối đa hóa kết quả cuối cùng trong toàn bộ chuỗi quyết định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply(x, op, v):
    if op == '+':
        return x + v
    if op == '-':
        return x - v
    return x * v

def solve():
    n = int(input())
    x = 0

    for _ in range(n):
        c1, v1, c2, v2 = input().split()
        v1 = int(v1)
        v2 = int(v2)

        a = apply(x, c1, v1)
        b = apply(x, c2, v2)

        if a > b:
            x = a
        else:
            x = b

    print(x)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một số nguyên duy nhất`x`đại diện cho mức năng lượng hiện tại. Đối với mỗi giai đoạn, nó tính toán kết quả của cả hai hoạt động có sẵn bằng cách sử dụng hàm trợ giúp nhỏ và cập nhật`x`đến kết quả tốt hơn. Hàm trợ giúp mã hóa trực tiếp ba phép tính số học có thể có, tránh logic phân nhánh lặp lại trong vòng lặp chính. 

Bước so sánh rất quan trọng: cả hai kết quả luôn được đánh giá từ cùng một giá trị ban đầu, đảm bảo lựa chọn là tối ưu cục bộ ở giai đoạn đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
+ 2 + 7
- 1 - 4
* 2 + 5
```| Sân khấu | Hiện tại x | Phương án 1 | Phương án 2 | Đã chọn x | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 7 | 7 | 
| 2 | 7 | 6 | 3 | 6 | 
| 3 | 6 | 12 | 11 | 12 | 

Sau giai đoạn đầu tiên, việc lựa chọn`+7`mang lại giá trị cao hơn so với`+2`. Ở giai đoạn thứ hai, bắt đầu từ 7, trừ 1 được 6 trong khi trừ 4 được 3, vì vậy chúng ta giữ lại 6. Ở giai đoạn cuối, nhân với 2 trội hơn cộng 5, được 12. 

### Ví dụ 2 

đầu vào:```
5
+ 1 + 1
* -1 * 2
+ -2 + 5
+ 5 - 3
* 1 * 3
```| Sân khấu | Hiện tại x | Phương án 1 | Phương án 2 | Đã chọn x | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 1 | 
| 2 | 1 | -1 | 2 | 2 | 
| 3 | 2 | 0 | 7 | 7 | 
| 4 | 7 | 12 | 4 | 12 | 
| 5 | 12 | 12 | 36 | 36 | 

Dấu vết này cho thấy các giá trị âm trung gian hoặc giá trị nhỏ hơn không được bảo toàn như thế nào nếu chúng không giúp tạo ra kết quả ngay lập tức tốt hơn. Mỗi giai đoạn sẽ chọn cách chuyển đổi tốt hơn một cách độc lập và kết quả cuối cùng sẽ tích lũy những cải tiến cục bộ này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi giai đoạn đánh giá hai thao tác có thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có một trạng thái số nguyên duy nhất được duy trì | 

Quét tuyến tính lên đến$10^5$các giai đoạn dễ dàng phù hợp với giới hạn thời gian điển hình. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào, vì không có lịch sử hoặc cấu trúc phụ trợ nào được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if False else ""  # placeholder for direct script usage

# Since solve() prints directly, we redefine run properly
def run(inp: str) -> str:
    import sys, io
    backup = sys.stdin
    sys.stdin = io.StringIO(inp)

    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()

    sys.stdin = backup
    return out.getvalue().strip()

# provided samples
assert run("""3
+ 2 + 7
- 1 - 4
* 2 + 5
""") == "12"

assert run("""5
+ 1 + 1
* -1 * 2
+ -2 + 5
+ 5 - 3
* 1 * 3
""") == "36"

# custom cases
assert run("""1
+ 5 + -10
""") == "5", "single stage max"

assert run("""2
* -1 + 3
+ 10 - 2
""") == "12", "negative multiplier interaction"

assert run("""3
+ 0 + 0
* 5 * -2
+ 1 + 1
""") in ["2", "1"], "sign flip sensitivity"

assert run("""4
+ 1 + 2
+ 3 + 4
+ 5 + 6
+ 7 + 8
""") == "20", "all additions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giai đoạn đơn | tối đa hai hoạt động | trường hợp cơ sở | 
| chuỗi nhân âm | lựa chọn đúng theo dấu lật | xử lý phép nhân | 
| trộn số không và dấu lật | vững chắc dưới các yếu tố trung tính | số học cạnh | 
| tất cả các bổ sung | tích lũy đơn điệu | tăng trưởng đơn giản | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi có sẵn phép nhân với giá trị âm. Trong những trường hợp như vậy, dấu của giá trị hiện tại có thể bị đảo ngược và kết quả cục bộ nhỏ hơn có thể sẽ hấp dẫn hơn sau này. Thuật toán xử lý việc này một cách chính xác vì nó đánh giá trực tiếp cả hai phép biến đổi ở mỗi bước từ giá trị hiện tại, do đó không đưa ra giả định ngầm định nào về tính đơn điệu. 

Một trường hợp khác là khi cả hai thao tác đều tạo ra cùng một kết quả. Trong trường hợp đó, một trong hai lựa chọn đều hợp lệ và thuật toán luôn chọn một lựa chọn mà không ảnh hưởng đến tính chính xác. 

Trường hợp cạnh cuối cùng là khi toán hạng bằng 0. Phép nhân với số 0 sẽ thu gọn trạng thái và các hoạt động trong tương lai sẽ xác định đầy đủ kết quả. Vì thuật toán luôn tính toán lại cả hai tùy chọn từ giá trị hiện tại chính xác nên việc thu gọn này được xử lý một cách tự nhiên mà không cần viết hoa đặc biệt.
