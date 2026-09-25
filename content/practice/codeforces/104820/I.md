---
title: "CF 104820I - \u0421\u0435\u043a\u0446\u0438\u044f \u043f\u043e \u0432\u043e\u043b\u044c\u043d\u043e\u0439 \u0431\u043e\u0440\u044c\u0431\u0435"
description: "Chúng ta có các số hạng $N$ đầu tiên của dãy $an = frac{1}{n}$, tạo ra các giá trị $1, frac{1}{2}, frac{1}{3}, dots, frac{1}{N}$."
date: "2026-06-28T12:56:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104820
codeforces_index: "I"
codeforces_contest_name: "\u0420\u0421\u041e-\u0410\u043b\u0430\u043d\u0438\u044f 2018-2023. \u0418\u0437\u0431\u0440\u0430\u043d\u043d\u043e\u0435"
rating: 0
weight: 104820
solve_time_s: 80
verified: true
draft: false
---

[CF 104820I - \u0421\u0435\u043a\u0446\u0438\u044f \u043f\u043e \u0432\u043e\u043b\u044c\u043d\u043e\u0439 \u0431\u043e\u0440\u044c\u0431\u0435](https://codeforces.com/problemset/problem/104820/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao cái đầu tiên$N$điều khoản của trình tự$a_n = \frac{1}{n}$, tạo ra các giá trị$1, \frac{1}{2}, \frac{1}{3}, \dots, \frac{1}{N}$. Cùng với chuỗi này, chúng ta cũng có một khoảng$[A, B]$, và chúng ta được yêu cầu đếm xem có bao nhiêu trong số này$N$các giá trị nằm trong khoảng đó, bao gồm cả hai điểm cuối. 

Vì vậy, nhiệm vụ hoàn toàn là lọc một tập hợp số rất cụ thể, nhưng sự tinh tế đến từ thực tế là chuỗi này không tùy ý, nó hoàn toàn dương và giảm đơn điệu. Cấu trúc đó làm cho phần lớn khoảng không liên quan đến phần lớn trục số. 

Những ràng buộc cho phép$A$Và$B$lên đến$10^9$về độ lớn và$N$lên đến$10^9$. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp đi lặp lại trên tất cả$n \le N$, vì ngay cả quét tuyến tính cũng cần tới một tỷ bước cho mỗi lần kiểm tra, vượt xa các giới hạn thông thường. Bất kỳ giải pháp hợp lệ nào cũng phải nén điều kiện thành lý luận theo thời gian không đổi dựa trên các bất đẳng thức. 

Một số tình huống khó khăn có xu hướng phá vỡ lối suy luận ngây thơ. 

Một trường hợp có vấn đề là khi khoảng nằm hoàn toàn ở phía âm, chẳng hạn$A = -5, B = -1$. Vì mỗi học kỳ$1/n$là hoàn toàn dương, không có phần tử dãy nào có thể nằm trong khoảng như vậy và câu trả lời đúng là$0$. Một cách triển khai ngây thơ quên mất dấu và cố gắng đảo ngược sự bất bình đẳng có thể bao gồm các chỉ số không chính xác. 

Một trường hợp khác là khi khoảng vượt qua 0, chẳng hạn như$A = -1, B = 2$. Ở đây mọi số hạng dương của dãy ít nhất là$0$, và tất cả các giá trị$1/n$nằm ở$(0, 1]$, do đó toàn bộ chuỗi lên đến$N$được bao gồm. Điều này có thể bị bỏ qua nếu người ta cố gắng giải không chính xác cả hai giới hạn một cách đối xứng mà không sử dụng thực tế là dãy không bao giờ trở thành số âm. 

Trường hợp tế nhị thứ ba là khi$A \ge 1$. Từ$1/n \le 1$cho tất cả$n$, chỉ có số hạng đầu tiên mới có thể thỏa mãn giới hạn dưới của ít nhất$1$, điều này buộc câu trả lời phải thu gọn lại tối đa một phần tử. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại trên tất cả$n$từ$1$ĐẾN$N$, tính toán$1/n$, và kiểm tra xem nó có nằm trong$[A, B]$. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa của dãy. Tuy nhiên, nó thực hiện$N$sự phân chia và so sánh, điều này trở nên không thể thực hiện được khi$N$đạt tới$10^9$. Thậm chí tại$10^7$, điều này đã quá chậm trong Python. 

Quan sát quan trọng là trình tự$1/n$giảm nghiêm ngặt và luôn dương. Thay vì kiểm tra từng số hạng, chúng ta có thể dịch điều kiện$$A \le \frac{1}{n} \le B$$vào những hạn chế về$n$. Bởi vì chuỗi hoạt động đơn điệu, nên các chỉ số hợp lệ tạo thành một phạm vi liền kề duy nhất và trên thực tế, cấu trúc của các số nguyên còn đơn giản hóa điều này hơn nữa. 

Ở phía trên,$\frac{1}{n} \le B$chỉ quan trọng khi$B > 0$. Từ$B$là một số nguyên, mọi số dương$B$ít nhất là$1$, và do đó mỗi số hạng$1/n \le 1 \le B$. Vì vậy giới hạn trên không bao giờ hạn chế bất cứ điều gì miễn là$B \ge 1$. 

Ở phía dưới,$\frac{1}{n} \ge A$phụ thuộc rất nhiều vào việc liệu$A$là tích cực. Nếu như$A \le 0$, mọi số hạng trong dãy đều tự động thỏa mãn nó vì mọi số hạng đều dương. Nếu như$A \ge 1$, thì chỉ có giá trị$1$(khi$n = 1$) có thể thỏa mãn bất đẳng thức. 

Điều này thu gọn toàn bộ vấn đề thành một số trường hợp có thời gian không đổi thay vì tìm kiếm bằng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lý luận trực tiếp về nơi$1/n$có thể nằm tương đối với khoảng. 

1. Kiểm tra xem$B \le 0$. Nếu vậy thì không có giá trị$1/n$có thể phù hợp vì tất cả các phần tử của dãy đều dương. Câu trả lời ngay lập tức là số không. 
2. Nếu$A \le 0$Và$B \ge 1$, mọi số hạng của dãy nằm trong khoảng từ 0 đến 1 và khoảng chứa đầy đủ vùng này. Mỗi một trong những điều đầu tiên$N$điều khoản đủ điều kiện. 
3. Nếu$A \ge 1$, thì điều kiện$1/n \ge 1$lực lượng$n = 1$. Sau đó, chúng tôi ngầm kiểm tra xem ứng cử viên duy nhất này có thỏa mãn giới hạn trên hay không, điều này sẽ thực hiện bất cứ khi nào khoảng hợp lệ và được sắp xếp. 
4. Trả về số lượng thu được từ logic trên. 

### Tại sao nó hoạt động 

Trình tự$1/n$là hoàn toàn tích cực và giảm nghiêm ngặt. Điều này có nghĩa là bất kỳ truy vấn khoảng thời gian nào cũng giảm xuống mức hiểu có bao nhiêu chỉ mục ánh xạ thành một phép biến đổi đơn điệu. Bởi vì hàm không bao giờ vượt qua 0 và không bao giờ vượt quá 1 sau số hạng đầu tiên, nên tất cả các ràng buộc có ý nghĩa sẽ chuyển thành kiểm tra ranh giới tại$n = 1$. Không có khả năng có nhiều phân đoạn hợp lệ rời rạc, do đó, việc phân tích trường hợp về việc liệu khoảng đó có nằm dưới 0, kéo dài bằng 0 hay bắt đầu trên một phân đoạn sẽ xác định đầy đủ câu trả lời hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B, N = map(int, input().split())
    
    if B <= 0:
        print(0)
        return
    
    if A <= 0:
        print(N)
        return
    
    # now A >= 1 and B >= 1 (since A <= B)
    print(1 if N >= 1 else 0)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo sự phân chia trường hợp bắt nguồn từ cấu trúc của$1/n$. Nhánh đầu tiên loại bỏ hoàn toàn tất cả các khoảng ở phía không dương, nơi mà không thuật ngữ nào có thể khớp được. Nhánh thứ hai nắm bắt bất kỳ khoảng nào bao gồm các giá trị 0 hoặc âm trên ranh giới bên trái của nó, khoảng này tự động bao gồm tất cả các giá trị chuỗi dương lên đến$N$. Trường hợp cuối cùng xử lý các khoảng hoàn toàn trong vùng dương bắt đầu từ ít nhất một, trong đó chỉ có số hạng đầu tiên$1$có thể đủ điều kiện. 

Một cạm bẫy phổ biến là cố gắng tính toán các ngưỡng như$1/A$hoặc$1/B$sử dụng phép tính dấu phẩy động. Điều đó là không cần thiết ở đây và sẽ chỉ gây ra các vấn đề về độ chính xác. Cấu trúc của các ràng buộc số nguyên cho phép giải pháp tránh được sự phân chia hoàn toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:$A = 1, B = 2, N = 3$Chúng tôi đánh giá các điều kiện trực tiếp. 

| Bước | A | B | N | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | B > 0 | 
| 2 | 1 | 2 | 3 | A > 0 nên chỉ có thể có số hạng đầu tiên | 
| 3 | 1 | 2 | 3 | 1/1 = 1 nằm trong khoảng | 

Câu trả lời là 1. Điều này xác nhận rằng chỉ phần tử đầu tiên của chuỗi mới có thể đạt giá trị 1 và tất cả các phần tử sau đó đều quá nhỏ. 

### Mẫu 2 

đầu vào:$A = -1, B = 2, N = 5$| Bước | A | B | N | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | 2 | 5 | B > 0 | 
| 2 | -1 | 2 | 5 | A <= 0, cho phép chuỗi đầy đủ | 
| 3 | -1 | 2 | 5 | Bao gồm tất cả các điều khoản | 

Câu trả lời là 5, vì toàn bộ tiền tố của các giá trị giảm dương nằm trong một khoảng bắt đầu dưới 0 và vượt quá một. 

Hai dấu vết này cho thấy hai chế độ cơ bản khác nhau: một chế độ chỉ tồn tại số hạng đầu tiên và một chế độ trong đó khoảng đủ rộng để bao gồm toàn bộ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ thực hiện một số lượng so sánh và kiểm tra trường hợp không đổi | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên đến$10^9$, do đó mọi sự phụ thuộc vào$N$sẽ là quá chậm. Một giải pháp theo thời gian không đổi là cần thiết và đủ, và việc phân tích trường hợp dẫn xuất sẽ đạt được điều này một cách trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    A, B, N = map(int, input().split())
    
    if B <= 0:
        print(0)
        return
    
    if A <= 0:
        print(N)
        return
    
    print(1 if N >= 1 else 0)

# provided samples
assert run("1 2 3") == "1"
assert run("-1 2 5") == "5"

# custom cases
assert run("-5 -1 100") == "0", "entirely negative interval"
assert run("0 10 7") == "7", "interval includes zero"
assert run("1 1 10") == "1", "only first term fits"
assert run("2 100 10") == "1", "high lower bound still only first term"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| -5 -1 100 | 0 | khoảng âm hoàn toàn | 
| 0 10 7 | 7 | khoảng bao gồm 0 | 
| 1 1 10 | 1 | khoảng đơn vị chặt chẽ | 
| 2 100 10 | 1 | phần tử đơn lực giới hạn dưới | 

## Vỏ cạnh 

Khi nào$B \le 0$, mọi thuật ngữ$1/n$là hoàn toàn dương nên không có giá trị nào có thể rơi vào trong khoảng. Đối với một đầu vào như$A = -10, B = 0, N = 5$, thuật toán ngay lập tức trả về 0 mà không cần kiểm tra thêm, phù hợp với thực tế là ngay cả giá trị chuỗi lớn nhất$1$không thỏa mãn$1 \le 0$. 

Khi$A \le 0 < B$, chẳng hạn như$A = -3, B = 2, N = 4$, mọi giá trị dãy đều nằm trong$(0,1]$, nằm hoàn toàn trong khoảng. Thuật toán chọn nhánh$A \le 0$và trả về$N = 4$, phù hợp với cả bốn điều khoản đều có giá trị. 

Khi$A \ge 1$, Ví dụ$A = 3, B = 10, N = 6$, chỉ một$1/1 = 1$có cơ hội thỏa mãn giới hạn dưới. Từ$1$cũng nằm trong khoảng, câu trả lời là 1 bất kể lớn đến mức nào$N$là.
