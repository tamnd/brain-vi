---
title: "CF 106084A - Lấy nó hoặc gấp đôi"
description: "Chúng ta được đưa cho một câu chuyện tương tác đơn giản dẫn đến quyết định về việc liệu một giá trị có nên được chấp nhận ngay lập tức hay được phép “tăng trưởng” một lần nữa. Số tiền ban đầu $x$ được trao cho người đầu tiên."
date: "2026-06-21T16:03:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106084
codeforces_index: "A"
codeforces_contest_name: "2025 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 106084
solve_time_s: 44
verified: true
draft: false
---

[CF 106084A - Nhận hoặc nhân đôi](https://codeforces.com/problemset/problem/106084/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một câu chuyện tương tác đơn giản dẫn đến quyết định về việc liệu một giá trị có nên được chấp nhận ngay lập tức hay được phép “tăng trưởng” một lần nữa. 

Một số tiền ban đầu$x$được cung cấp cho người đầu tiên. Mỗi người trong chuỗi có thể chấp nhận số tiền hiện tại, kết thúc quá trình hoặc từ chối số tiền đó và buộc số tiền đó tăng gấp đôi cho người tiếp theo. Tuy nhiên, có một hạn chế chung: máy chủ chỉ có$d$đô la có sẵn. Nếu tăng gấp đôi số tiền hiện tại sẽ vượt quá$d$, chủ nhà không được để chuyện đó xảy ra mà phải ép người hiện tại nhận tiền ngay. 

Nhiệm vụ là xác định ngay lần đầu tiên gặp số lượng$x$, cho dù người đó sẽ nói “lấy nó” hay “nhân đôi nó”, giả định rằng tất cả mọi người đều thích nhân đôi bất cứ khi nào họ được phép làm như vậy. 

Quan sát quan trọng từ các ràng buộc là cả hai$x$Và$d$đang lên đến$10^9$. Giá trị này đủ nhỏ để việc nhân đôi lặp đi lặp lại không thể tiếp tục trong nhiều bước vì mỗi bước sẽ tăng giá trị theo cấp số nhân. Trên thực tế, sau tối đa 30 lần nhân đôi, các giá trị đã vượt quá giới hạn của số nguyên 32 bit và sau khoảng 30 đến 31 bước, chúng sẽ vượt qua$10^9$. 

Một mô phỏng ngây thơ liên tục nhân đôi cho đến khi điều kiện dừng vẫn an toàn ở đây, vì số bước là logarit trong$d$. Tuy nhiên, vấn đề thực sự thậm chí còn đơn giản hơn: chúng ta chỉ cần quyết định xem có được phép nhân đôi thêm một lần nữa ở trạng thái hiện tại hay không. 

Trường hợp nghiêm trọng nhất là hiểu nhầm liệu quyết định phụ thuộc vào các bước trong tương lai hay chỉ là hành động tiếp theo ngay lập tức. Một sai lầm tiềm ẩn khác là mô phỏng nhiều người một cách không cần thiết, trong khi vấn đề chỉ yêu cầu quyết định đầu tiên trong chuỗi. 

Ví dụ, nếu$x = 123$Và$d = 246$, nhân đôi sẽ cho chính xác 246, giá trị này hợp lệ, vì vậy câu trả lời là “nhân đôi”. Nếu như$x = 345$Và$d = 678$, nhân đôi là 690, vượt quá giới hạn nên người đó buộc phải lấy tiền ngay lập tức. 

Toàn bộ quá trình sụp đổ vào việc kiểm tra một bất đẳng thức duy nhất. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực bám sát câu chuyện theo đúng nghĩa đen. Chúng tôi bắt đầu ở giá trị$x$và liên tục mô phỏng mọi người theo trình tự. Ở mỗi bước, chúng tôi kiểm tra xem việc nhân đôi giá trị hiện tại có vượt quá$d$. Nếu có, chúng tôi dừng lại và buộc phải chấp nhận. Nếu không, chúng tôi tăng gấp đôi và tiếp tục. Chúng tôi lặp lại cho đến khi có người lấy tiền. 

Điều này hiệu quả vì quá trình này mang tính quyết định: mọi người luôn chọn tăng gấp đôi trừ khi bị buộc phải làm khác. Tuy nhiên, số lần lặp tỷ lệ thuận với số lần chúng ta có thể nhân đôi trước khi vượt quá$d$, nhiều nhất là$O(\log d)$. Mặc dù việc này tuy nhỏ nhưng vẫn là công việc không cần thiết vì bài toán chỉ yêu cầu quyết định đầu tiên. 

Cái nhìn sâu sắc quan trọng là hành vi của tất cả những người tham gia trong tương lai không liên quan đến quyết định hiện tại. Câu hỏi duy nhất là liệu số tiền hiện tại có thể được nhân đôi một cách hợp pháp hay không. Nếu như$2x > d$, thì việc nhân đôi là không thể ngay lập tức nên câu trả lời là buộc phải chấp nhận. Nếu như$2x \le d$, thì được phép nhân đôi và người đó sẽ luôn chọn nó. 

Điều này làm giảm toàn bộ quá trình xuống mức so sánh theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng toàn bộ quá trình |$O(\log d)$|$O(1)$| Đã chấp nhận | 
| Kiểm tra trực tiếp$2x \le d$|$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chỉ cần xác định xem người đầu tiên có được phép chuyển tiền hay không. 

1. Đọc các giá trị$x$Và$d$. Chúng đại diện cho ưu đãi hiện tại và ngân sách tối đa được phép. 
2. Tính toán$2x$. Điều này thể hiện lời đề nghị tiếp theo sẽ như thế nào nếu người đó chọn tăng gấp đôi. 
3. So sánh$2x$với$d$. Nếu như$2x$thực sự lớn hơn$d$, thì không được phép nhân đôi nên người đó phải chấp nhận ngay. 
4. Ngược lại, nếu$2x \le d$, người đó làm theo sở thích của họ và chọn tăng gấp đôi. 

Quyết định hoàn toàn được xác định ở bước duy nhất này, do đó không cần mô phỏng thêm. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quy trình, hạn chế duy nhất hạn chế hành vi là liệu việc tăng gấp đôi có vi phạm ngân sách hay không. Những người tham gia không có lý do gì để về sớm trừ khi bị ép buộc, vì vậy điều kiện chặn duy nhất là$2 \cdot \text{current} > d$. Vì chúng ta chỉ được hỏi về trạng thái ban đầu nên toàn bộ hệ thống chỉ còn việc kiểm tra xem lần nhân đôi đầu tiên có hợp lệ hay không. Bất kỳ chuỗi tương lai nào cũng không ảnh hưởng đến điều kiện khả thi cục bộ này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, d = map(int, input().split())

if 2 * x > d:
    print("take it")
else:
    print("double it")
```Giải pháp đọc hai số nguyên và thực hiện một so sánh số học duy nhất. Điều tinh tế duy nhất là đảm bảo phép nhân xảy ra trước khi so sánh, điều này an toàn trong Python do các số nguyên có độ chính xác tùy ý. 

Logic quyết định phản ánh trực tiếp thuật toán: chúng tôi kiểm tra xem trạng thái tiếp theo sau khi nhân đôi có hợp lệ theo ràng buộc hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`123 246`Chúng tôi tính toán xem có được phép nhân đôi hay không. 

| Bước | Hiện tại x | 2x | d | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 123 | 246 | 246 | nhân đôi nó | 

Từ$2x = d$, nhân đôi là hợp lệ, vì vậy kết quả đầu ra chính xác là “gấp đôi”. 

Điều này xác nhận rằng sự bình đẳng được cho phép và chỉ nhân đôi các khối tràn nghiêm ngặt. 

### Ví dụ 2 

đầu vào:`345 678`| Bước | Hiện tại x | 2x | d | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 345 | 690 | 678 | lấy nó | 

Đây$2x > d$, nên việc nhân đôi là không thể. Người đó phải chấp nhận ngay. 

Điều này xác nhận rằng việc so sánh là nghiêm ngặt và không bao gồm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ thực hiện một phép nhân và một phép so sánh | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép lên đến$10^9$, nhưng thuật toán thực hiện số học theo thời gian không đổi, nằm trong giới hạn tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, d = map(int, sys.stdin.readline().split())
    return "double it" if 2 * x <= d else "take it"

# provided samples
assert run("123 246\n") == "double it"
assert run("345 678\n") == "take it"

# custom cases
assert run("1 1\n") == "take it", "doubling exceeds even smallest bound"
assert run("1 2\n") == "double it", "exact boundary case"
assert run("500000000 1000000000\n") == "double it", "max boundary valid"
assert run("500000001 1000000000\n") == "take it", "just over half boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | lấy nó | cạnh tối thiểu nơi nhân đôi không thành công | 
| 1 2 | nhân đôi nó | trường hợp nhân đôi hợp lệ nhỏ nhất | 
| 500000000 1000000000 | nhân đôi nó | ranh giới rộng lớn nơi sự bình đẳng được giữ vững | 
| 500000001 1000000000 | lấy nó | lệch nhau khoảng một nửa d | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$2x = d$. Đối với đầu vào`123 246`, thuật toán tính toán$2x = 246$, không lớn hơn$d$, do đó nó sẽ xuất ra chính xác “nhân đôi nó”. Điều này khẳng định rằng sự bình đẳng không gây ra sự chấp nhận bắt buộc. 

Một trường hợp cạnh khác là khi$x = d$. Đối với đầu vào`100 100`, chúng tôi tính toán$2x = 200$, vượt quá$d$, vì vậy đầu ra là "lấy nó". Điều này cho thấy rằng ngay cả khi ưu đãi ban đầu hoàn toàn có giá cả phải chăng, việc tăng gấp đôi có thể đã không hợp lệ. 

Trường hợp thứ ba là khi$x$chỉ dưới một nửa$d$, chẳng hạn như`499999999 1000000000`. Nhân đôi vẫn còn hiệu lực, vì vậy quá trình vẫn tiếp tục. Nhưng nếu chúng ta tăng$x$bởi một, quyết định lật ngay lập tức. Điều này nhấn mạnh rằng giải pháp hoàn toàn dựa trên ranh giới và chỉ nhạy cảm với bất đẳng thức duy nhất$2x \le d$.
