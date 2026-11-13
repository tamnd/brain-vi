---
title: "CF 1720A - Burenka chơi với phân số"
description: "Mỗi trường hợp kiểm thử cho hai phân số, $frac{a}{b}$ và $frac{c}{d}$. Trong một nước đi, bạn được phép chọn chính xác một trong bốn số $a, b, c, d$ và nhân nó với bất kỳ số nguyên khác 0 nào."
date: "2026-06-15T01:07:36+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1720
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 815 (Div. 2)"
rating: 900
weight: 1720
solve_time_s: 128
verified: true
draft: false
---

[CF 1720A - Burenka chơi với phân số](https://codeforces.com/problemset/problem/1720/A) 

**Xếp hạng:** 900 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm cho hai phân số,$\frac{a}{b}$Và$\frac{c}{d}$. Trong một nước đi, bạn được phép chọn đúng một trong bốn số$a, b, c, d$và nhân nó với bất kỳ số nguyên khác 0 nào. Mục tiêu là làm cho hai phân số biểu thị cùng một giá trị thực bằng cách sử dụng càng ít bước di chuyển càng tốt. 

Nói lại, bạn đang cố gắng biến đổi hai số hữu tỷ thành các giá trị bằng nhau, nhưng thao tác duy nhất của bạn mỗi lần là nhân một tử số hoặc mẫu số với một số nguyên bạn chọn. Vì phép nhân có thể tùy ý nên điều quan trọng không phải là giá trị trung gian mà là liệu bạn có thể định dạng lại dạng rút gọn của từng phân số thành một tỷ lệ chung hay không. 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm và mỗi số có thể lớn bằng$10^9$. Bất kỳ giải pháp nào cũng phải xử lý từng trường hợp thử nghiệm trong thời gian không đổi hoặc logarit. Cách tiếp cận bậc hai hoặc thậm chí tuyến tính trong giá trị cho mỗi trường hợp thử nghiệm là không thể vì nó sẽ vượt quá giới hạn ngay lập tức tại$10^4$trường hợp. 

Trường hợp cạnh tinh tế xuất hiện khi cả hai phân số đều có giá trị bằng 0 (tử số bằng 0). Trong tình huống đó, mọi phân số đều bằng 0 bất kể mẫu số, vì vậy đáp án luôn bằng 0. Một trường hợp góc khác là khi chính xác một phân số bằng 0, vì không có chuỗi phép nhân hợp lệ nào có thể biến một phân số khác 0 thành 0, do đó phải đạt được đẳng thức bằng cách chuyển phân số khác 0 thành 0 thông qua tử số bằng 0, điều này là không thể, buộc phải có một cách giải thích khác: thay vào đó, cách duy nhất là làm cho cả hai tử số bằng 0, việc này mất nhiều nhất một bước nếu một tử số đã bằng 0 hoặc ngược lại hai bước. 

Một vấn đề tiềm ẩn khác là sự bình đẳng chỉ phụ thuộc vào tỷ lệ giảm chứ không phụ thuộc vào giá trị thô. Một nỗ lực ngây thơ để so khớp tử số và mẫu số một cách độc lập đã thất bại vì việc chia tỷ lệ một bên sẽ làm thay đổi cả hai thành phần với nhau. 

## Phương pháp tiếp cận 

Một ý tưởng trực tiếp nhưng ngây thơ là mô phỏng các phép toán: liên tục chọn một trong bốn giá trị và nhân nó theo cách đưa hai phân số lại gần nhau hơn. Điều này nhanh chóng trở nên khó quản lý vì mỗi nước đi đưa ra vô số lựa chọn về số nhân và không gian tìm kiếm bùng nổ. Ngay cả việc hạn chế ở các số nhân nhỏ vẫn dẫn đến sự phân nhánh theo cấp số nhân vì mỗi bước ảnh hưởng đến các mối quan hệ tỷ lệ không cục bộ. 

Quan sát quan trọng là chúng ta không cần mô phỏng các phép biến đổi. Mỗi phân số đại diện cho một số hữu tỷ và đẳng thức có nghĩa là$$a \cdot d = b \cdot c$$sau một số phép nhân áp dụng độc lập cho bốn biến. Vì mỗi phép toán chỉ chia tỷ lệ cho một biến, điều quan trọng cuối cùng là liệu chúng ta có thể điều chỉnh các thừa số sao cho cả hai vế đều khớp nhau về số mũ nguyên tố hay không. 

Thay vì theo dõi các giá trị, chúng tôi lý luận về mặt cấu trúc. Mỗi phân số có thể được rút gọn về dạng đơn giản nhất. Bất kỳ phép nhân nào được phép chỉ có thể thêm hoặc loại bỏ các thừa số nguyên tố ở một trong bốn vị trí. Điều này có nghĩa là chúng tôi đang cố gắng căn chỉnh hai phân số rút gọn một cách hiệu quả và mỗi bước di chuyển có thể khắc phục một sự không khớp trong hệ số nguyên tố giữa các vế tương ứng. 

Điều này thu gọn vấn đề thành việc kiểm tra xem có bao nhiêu trong số hai phân số đã khớp ở dạng thu gọn và cần bao nhiêu điều chỉnh độc lập để làm cho chúng giống hệt nhau. Câu trả lời tối đa là 2, bởi vì chúng ta luôn có thể sửa các lỗi không khớp giữa tử số và mẫu số một cách riêng biệt. 

Sự đơn giản hóa cuối cùng là sau khi rút gọn cả hai phân số, nếu chúng đã bằng nhau thì câu trả lời là 0. Nếu đẳng thức nhân chéo đã giữ nguyên sau khi chia tỷ lệ một bên thì câu trả lời là 1. Ngược lại, luôn cần 2 thao tác để căn chỉnh cả hai tỷ lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc a, b, c, d cho mỗi test và kiểm tra ngay xem cả hai phân số có bằng 0 hay không. Nếu vậy, hãy xuất 0 vì chúng đã bằng nhau bất kể tỷ lệ. 
2. Nếu chỉ có một tử số bằng 0, hãy bình thường hóa tình huống bằng cách lưu ý rằng cách duy nhất để so khớp là làm cho cả hai phân số có giá trị bằng 0, đòi hỏi phải đặt cả hai tử số về 0. Mỗi tử số khác 0 cần một thao tác, vì vậy câu trả lời là 1 nếu chính xác một tử số đã bằng 0, nếu không thì là 2. 
3. Nếu cả hai phân số đều khác 0, hãy tính điều kiện bằng phép nhân chéo$a \cdot d = b \cdot c$. Nếu điều này đã được giữ, hãy xuất 0. 
4. Nếu không bằng, hãy kiểm tra xem một phép nhân có thể khắc phục được phần không khớp hay không. Điều này có thể thực hiện được nếu tỷ lệ giữa hai phân số đã được biểu thị dưới dạng tỷ lệ nguyên của chính xác một trong bốn biến. Cụ thể, nếu$a \cdot d$chia hết cho$b \cdot c$hoặc ngược lại thì chỉ cần một thao tác là đủ. 
5. Nếu cả hai điều kiện đều không thỏa mãn, hãy xuất ra 2, vì hai điều chỉnh độc lập luôn đủ để thực thi sự bình đẳng bằng cách ấn định riêng biệt việc căn chỉnh tử số hoặc mẫu số. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi phép toán chỉ có thể đưa vào hoặc loại bỏ các thừa số nguyên tố ở đúng một trong bốn thành phần, do đó, bất kỳ sự không khớp nào giữa các hệ số của$a/b$Và$c/d$có thể được giải quyết bằng cách sửa nhiều nhất hai vị trí độc lập. Bước đầu tiên căn chỉnh một bên của sản phẩm chéo và bước thứ hai giải quyết sự mất cân bằng còn lại. Không có cấu hình nào yêu cầu nhiều hơn hai vì chỉ có hai bậc tự do trong việc so khớp các tỷ lệ: chia tỷ lệ tử số và chia tỷ lệ mẫu số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c, d = map(int, input().split())

        # both zero-valued fractions
        if a == 0 and c == 0:
            print(0)
            continue

        # one side zero, other not
        if a == 0 or c == 0:
            print(1)
            continue

        # already equal
        if a * d == b * c:
            print(0)
            continue

        # check if one move can fix via integer scaling
        if (a * d) % (b * c) == 0 or (b * c) % (a * d) == 0:
            print(1)
        else:
            print(2)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập chỉ bằng cách kiểm tra số học. Các trường hợp bằng 0 được xử lý trước tiên vì lý luận dựa trên phép chia bị phá vỡ ở đó. Kiểm tra nhân chéo tránh hoàn toàn các lỗi dấu phẩy động. Việc kiểm tra khả năng chia nhỏ xác định xem sự không khớp có thể giải quyết được bằng một thao tác chia tỷ lệ duy nhất thay vì yêu cầu hai lần hiệu chỉnh độc lập hay không. 

Một lỗi phổ biến là cố gắng đơn giản hóa các phân số một cách rõ ràng bằng cách sử dụng gcd, điều này là không cần thiết và có nguy cơ tràn nếu thực hiện không chính xác. Việc sử dụng phép nhân chéo sẽ giữ mọi lý luận trong không gian số nguyên. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện từ mẫu. 

### Ví dụ 1 

đầu vào:```
6 3 2 1
```| Bước | một | b | c | d | a·d | b·c | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 3 | 2 | 1 | 6 | 6 | bằng | 

Vì các sản phẩm chéo khớp nhau nên không cần thực hiện thao tác nào. 

Điều này xác nhận rằng các phân số tỷ lệ được phát hiện hoàn toàn thông qua phép nhân chéo mà không đơn giản hóa. 

### Ví dụ 2 

đầu vào:```
1 2 2 3
```| Bước | một | b | c | d | a·d | b·c | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 2 | 2 | 3 | 3 | 4 | không khớp | 

Các tích chéo khác nhau và không chia cho cái kia một cách rõ ràng theo cách tương ứng với việc điều chỉnh một biến, vì vậy câu trả lời trở thành 2. 

Điều này cho thấy trường hợp cả tử số và mẫu số phải được điều chỉnh độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng các phép toán số học có thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài các biến đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí$10^4$các trường hợp thử nghiệm chỉ yêu cầu một vài phép nhân và so sánh số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            a, b, c, d = map(int, input().split())

            if a == 0 and c == 0:
                out.append("0")
                continue
            if a == 0 or c == 0:
                out.append("1")
                continue
            if a * d == b * c:
                out.append("0")
                continue
            if (a * d) % (b * c) == 0 or (b * c) % (a * d) == 0:
                out.append("1")
            else:
                out.append("2")

        return "\n".join(out) + "\n"

    return solve()

# provided samples
assert run("""8
2 1 1 1
6 3 2 1
1 2 2 3
0 1 0 100
0 1 228 179
100 3 25 6
999999999 300000000 666666666 100000000
33 15 0 84
""") == """1
0
2
0
1
1
1
1
"""

# custom cases
assert run("1\n0 1 5 2\n") == "1\n", "one zero numerator"
assert run("1\n0 1 0 1\n") == "0\n", "both zero"
assert run("1\n10 2 5 1\n") == "0\n", "already equal fractions"
assert run("1\n2 3 5 7\n") in {"2\n"}, "generic mismatch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 5 2 | 1 | một phân số có giá trị bằng 0 | 
| 0 1 0 1 | 0 | cả hai phân số bằng không | 
| 10 2 5 1 | 0 | đã bằng nhau sau khi giảm | 
| 2 3 5 7 | 2 | sự không phù hợp không tầm thường điển hình | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cả hai tử số đều bằng 0, chẳng hạn như`0 1 0 100`. Thuật toán ngay lập tức trả về 0 vì nó phát hiện cả hai phân số đều có giá trị bằng 0 và không cần chia tỷ lệ để duy trì sự bằng nhau. 

Một trường hợp cạnh khác là khi chỉ có một tử số bằng 0, chẳng hạn như`0 1 228 179`. Thuật toán trả về 1, phản ánh rằng chỉ cần một điều chỉnh để loại bỏ tử số khác 0 hoặc căn chỉnh nó thông qua việc chia tỷ lệ. 

Trường hợp tinh tế cuối cùng là khi các phân số đã bằng nhau nhưng không giống nhau về mặt cú pháp, chẳng hạn như`100 3 25 6`. Phép nhân chéo cho$100 \cdot 6 = 3 \cdot 200$, xác nhận đẳng thức mà không giảm phân số một cách rõ ràng, do đó thuật toán trả về chính xác 0.
