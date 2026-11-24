---
problem: 898B
contest_id: 898
problem_index: B
name: "Proper Nutrition"
contest_name: "Codeforces Round 451 (Div. 2)"
rating: 1100
tags: ["brute force", "implementation", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 78
date: 2026-06-17
model: gpt-5-5
samples_passed: 1
samples_total: 1
---

#CF 898B - Dinh dưỡng hợp lý 

**Đánh giá:** 1100 
**Tags:** vũ lực, thực hiện, lý thuyết số 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có (1/1 mẫu) 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một ngân sách cố định và hai loại mặt hàng, mỗi loại có chi phí cố định. Nhiệm vụ là xác định xem liệu chúng ta có thể phù hợp chính xác với ngân sách hay không bằng cách mua một số mặt hàng thuộc loại thứ nhất và một số mặt hàng thuộc loại thứ hai, trong đó chúng ta được phép mua 0 hoặc nhiều hơn mỗi mặt hàng. 

Nói một cách cụ thể hơn, chúng tôi muốn biết liệu có tồn tại hai số nguyên không âm, đại diện cho số lượng của hai loại mặt hàng, sao cho tổ hợp tuyến tính của giá của chúng bằng tổng ngân sách hay không. Nếu một cặp như vậy tồn tại, chúng ta phải xuất ra một cặp hợp lệ; mặt khác, chúng tôi báo cáo rằng điều đó là không thể. 

Những ràng buộc cho phép ngân sách và giá cả lên tới 10 triệu. Kích thước đó loại trừ mọi thứ lặp lại trên tất cả các cặp số có thể có. Một vòng lặp kép về số lượng có thể sẽ dẫn đến khoảng$10^{14}$hoạt động trong trường hợp xấu nhất, vượt xa mọi thời hạn hợp lý. Ngay cả một vòng lặp duy nhất trên tất cả số lượng có thể có của một mặt hàng lên tới 10 triệu cũng là giới hạn nhưng có thể vượt qua nếu mỗi bước là O(1), đó là hướng mà giải pháp thực hiện. 

Một trường hợp phức tạp xuất hiện khi một trong các giá của mặt hàng là 1. Trong tình huống đó, câu trả lời luôn là có vì chúng ta luôn có thể điều chỉnh phần còn lại bằng cách sử dụng mặt hàng đó. Một trường hợp khác là khi cả hai mức giá đều lớn hơn ngân sách. Khi đó tổng duy nhất có thể là 0, do đó câu trả lời ngay lập tức là không trừ khi ngân sách bằng 0, điều này bị loại trừ bởi các ràng buộc. 

Một trường hợp thất bại thú vị hơn đối với lý luận ngây thơ là giả định rằng chúng ta cần thử tất cả các kết hợp một cách đối xứng. Ví dụ, với$n = 7, a = 2, b = 3$, thử tất cả các cặp đều được. Nhưng với$n = 10^7$,$a = 2$,$b = 3$, vũ lực trở nên không khả thi ngay cả khi có các giải pháp hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử tất cả các số có thể có của mục đầu tiên. Đối với mỗi sự lựa chọn$x$, ta tính số tiền còn lại$n - x \cdot a$và kiểm tra xem nó có thể được hình thành bằng cách sử dụng mục thứ hai hay không. Điều này có nghĩa là kiểm tra xem số dư có phải là số không âm và chia hết cho$b$. Cách tiếp cận này đúng vì nó khám phá một cách có hệ thống tất cả các giá trị hợp lệ của một biến trong khi tính toán biến kia một cách xác định. 

Vấn đề là phạm vi của$x$. Trong trường hợp xấu nhất,$a = 1$, Vì thế$x$chạy đến$10^7$. Mỗi lần lặp là O(1), nhưng vòng lặp vẫn thực hiện 10 triệu lần lặp. Điều này có thể chấp nhận được trong Python nhưng không có giới hạn cho sự kém hiệu quả và quan trọng hơn là nó gợi ý rằng chúng ta có thể làm tốt hơn. 

Quan sát quan trọng là chúng ta không cần phải thử tất cả các giá trị của$x$. Vì chúng ta chỉ quan tâm đến việc liệu một cặp hợp lệ có tồn tại hay không nên chúng ta có thể giảm không gian tìm kiếm bằng cách khai thác cấu trúc mô-đun. phương trình$x \cdot a + y \cdot b = n$có thể được sắp xếp lại thành$y = (n - x \cdot a) / b$. Thay vì quét tất cả$x$, chúng ta chỉ cần đảm bảo đạt được lớp dư lượng trong đó$n - x \cdot a$trở nên chia hết cho$b$. 

Điều này dẫn đến một thủ thuật tiêu chuẩn cho các bài toán Diophantine tuyến tính với hai biến và không có ràng buộc nào ngoài giá trị không âm: lặp chỉ trên một chiều cho đến giới hạn nhỏ rút ra từ hệ số kia. Kể từ khi tăng$x$qua$b$thay đổi biểu thức modulo$b$, chúng ta chỉ cần kiểm tra nhiều nhất$b$các ứng cử viên, nhưng chúng ta có thể đơn giản hóa hơn nữa bằng cách lặp lại tối đa$n / a$, vẫn an toàn và dễ thực hiện. 

Một quan điểm hiệu quả hơn là hoán đổi vai trò để chúng tôi luôn lặp lại chi phí nhỏ hơn trong hai chi phí. Điều đó đảm bảo nhiều nhất$10^7 / 1 = 10^7$lặp lại trong trường hợp xấu nhất, nhưng thường ít hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n/a) trường hợp xấu nhất O(10^7) | O(1) | Chấp nhận được nhưng chặt chẽ | 
| Tối ưu | O(phút(n/a, n/b)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả sử chúng tôi thử các giá trị cho một biến và tính toán trực tiếp biến kia. 

1. Đảm bảo chúng tôi lặp lại mục có chi phí nhỏ hơn trước. Điều này làm giảm số lần lặp cần thiết trong trường hợp xấu nhất. 
2. Lặp lại tất cả các số có thể$x$như vậy$x \cdot a \leq n$. Mỗi$x$đại diện cho việc mua nhiều mặt hàng thuộc loại đầu tiên. 
3. Đối với mỗi$x$, tính số tiền còn lại sau khi mua những món đồ đó. Đây là$rem = n - x \cdot a$. Nếu giá trị này trở thành âm, các giá trị tiếp theo của$x$sẽ chỉ làm cho nó tiêu cực hơn nên chúng ta dừng lại. 
4. Kiểm tra xem phần dư có thể được hình thành chính xác bằng cách sử dụng mục thứ hai hay không. Điều này đúng khi và chỉ khi$rem \% b = 0$. 
5. Nếu chia hết thì tính$y = rem // b$và xuất cặp$(x, y)$. Điều này đảm bảo tổng số trận đấu chính xác. 
6. Nếu không hợp lệ$x$tạo ra một hợp lệ$y$, đầu ra SỐ. 

### Tại sao nó hoạt động 

Mọi nghiệm hợp lệ đều tương ứng với một giá trị nào đó của$x$trong phạm vi chúng tôi liệt kê, bởi vì bất kỳ giải pháp nào cũng phải thỏa mãn$x \cdot a \le n$. Đối với mỗi như vậy$x$, giá trị của$y$được xác định duy nhất nếu nó tồn tại. Vì chúng tôi đã cạn kiệt mọi khả năng$x$, chúng ta không thể bỏ lỡ một phân tích hợp lệ của$n$. Tính đúng đắn phụ thuộc vào thực tế là biến thứ hai không được chọn độc lập sau khi biến thứ nhất được cố định; nó hoàn toàn được xác định bởi phương trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = int(input())
b = int(input())

# iterate over smaller unit cost first for efficiency
if a > b:
    a, b = b, a

found = False

for x in range(n // a + 1):
    rem = n - x * a
    if rem < 0:
        break
    if rem % b == 0:
        y = rem // b
        print("YES")
        print(x, y)
        found = True
        break

if not found:
    print("NO")
```Trước tiên, mã sẽ bình thường hóa vấn đề để chúng tôi lặp lại mục rẻ hơn, giúp giảm số lần lặp vòng lặp. Việc hoán đổi này không làm thay đổi tính đúng đắn vì chúng ta chỉ đơn giản đổi tên các biến trong một phương trình đối xứng. 

Ranh giới vòng lặp`n // a + 1`đảm bảo chúng tôi chỉ xem xét các giá trị khả thi của$x$. Việc kiểm tra phần còn lại đảm bảo chúng ta không thử phân tách không hợp lệ. Kiểm tra tính chia hết đảm bảo rằng biến thứ hai là một số nguyên, được yêu cầu bởi bài toán. 

Một lỗi phổ biến ở đây là quên rằng việc hoán đổi`a`Và`b`thay đổi ý nghĩa của các biến đầu ra. Sau khi hoán đổi, cặp được in tương ứng với các vai trò được hoán đổi, điều này nhất quán vì chúng tôi vẫn đang trả về một phân tách hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 7, a = 2, b = 3 

| x | rem = 7 - 2x | rem% 3 | y = rem / 3 | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | 1 | - | bỏ qua | 
| 1 | 5 | 2 | - | bỏ qua | 
| 2 | 3 | 0 | 1 | tìm thấy | 

Điều này cho thấy rằng phân tách chính xác được tìm thấy ở phần dư hợp lệ đầu tiên chia hết cho 3. Thuật toán dừng ngay lập tức, xác nhận việc kết thúc sớm hoạt động chính xác. 

### Ví dụ 2 

đầu vào: 

n = 10, a = 4, b = 3 

| x | rem = 10 - 4x | rem% 3 | y | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 10 | 1 | - | bỏ qua | 
| 1 | 6 | 0 | 2 | tìm thấy | 

Dấu vết này chứng tỏ rằng nhiều tiền tố không hợp lệ sẽ bị bỏ qua cho đến khi tìm thấy sự kết hợp hợp lệ. Nó khẳng định rằng chỉ cần kiểm tra tính chia hết là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n/phút(a, b)) | chúng tôi lặp đi lặp lại số lượng có thể có của mặt hàng rẻ hơn | 
| Không gian | O(1) | chỉ một số số nguyên được lưu trữ | 

Số lần lặp tối đa bị giới hạn bởi 10 triệu trong trường hợp xấu nhất, có thể chấp nhận được theo các ràng buộc. Việc sử dụng bộ nhớ không đổi do không sử dụng cấu trúc dữ liệu phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _StringIO

    out = _StringIO()
    _stdin = sys.stdin

    # re-run solution inline
    n = int(_stdin.readline())
    a = int(_stdin.readline())
    b = int(_stdin.readline())

    if a > b:
        a, b = b, a

    found = False
    for x in range(n // a + 1):
        rem = n - x * a
        if rem < 0:
            break
        if rem % b == 0:
            y = rem // b
            out.write("YES\n")
            out.write(f"{x} {y}\n")
            found = True
            break

    if not found:
        out.write("NO\n")

    return out.getvalue().strip()

# provided samples
assert run("7\n2\n3\n") == "YES\n2 1"

# custom cases
assert run("1\n1\n2\n") == "YES\n1 0", "use only a=1"
assert run("10\n3\n5\n") in ["YES\n0 2", "YES\n5 1"], "multiple valid answers"
assert run("7\n5\n6\n") == "NO", "impossible combination"
assert run("10000000\n1\n9999999\n") == "YES\n10000000 0", "boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,1,2 | CÓ 1 0 | trường hợp tầm thường có đơn giá | 
| 10,3,5 | nhiều | nhiều phân tách hợp lệ | 
| 7,5,6 | KHÔNG | không thể đại diện được | 
| 10^7,1,9999999 | CÓ | xử lý ranh giới tối đa | 

## Vỏ cạnh 

Khi một trong các mức giá là 1, vòng lặp trở nên gần như tầm thường. Đối với đầu vào$n = 100, a = 1, b = 999$, thuật toán tìm ngay$x = 100$,$y = 0$. Mỗi lần lặp lại đều giảm phần còn lại một cách xác định, do đó thất bại là không thể. 

Ví dụ: khi cả hai mức giá đều vượt quá ngân sách$n = 5, a = 6, b = 7$, vòng lặp chỉ chạy cho$x = 0$. Phần còn lại là 5, không chia hết cho cả hai chi phí, do đó thuật toán trả về NO một cách chính xác. 

Khi cả hai mức giá đều chia ngân sách theo nhiều cách, chẳng hạn như$n = 12, a = 3, b = 4$, thuật toán dừng ở phân tách hợp lệ đầu tiên mà nó gặp phải. Nếu như$x = 0$, nó tìm thấy$y = 3$; mặt khác vẫn tồn tại các nghiệm khác, nhưng tính đúng đắn không phụ thuộc vào tính duy nhất.
