---
title: "CF 105608D - \u0414\u0432\u0430 \u0448\u0438\u0444\u0440\u0430"
description: "Chúng ta được cho hai số nguyên biểu thị kết quả mong muốn của một số quy trình mã hóa ẩn được áp dụng cho một chuỗi các chữ cái Latinh viết thường."
date: "2026-06-22T18:00:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105608
codeforces_index: "D"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421, \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440 2024-2025"
rating: 0
weight: 105608
solve_time_s: 56
verified: true
draft: false
---

[CF 105608D - \u0414\u0432\u0430 \u0448\u0438\u0444\u0440\u0430](https://codeforces.com/problemset/problem/105608/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên biểu thị kết quả mong muốn của một số quy trình mã hóa ẩn được áp dụng cho một chuỗi các chữ cái Latinh viết thường. Mỗi chữ cái đóng góp vào hai giá trị tích lũy là A và B, nhưng theo một cách hơi bất đối xứng: mỗi ký tự tăng B thêm đúng một đơn vị so với giá trị nó tăng A. Nói cách khác, so với A, B nhận được thêm một đơn vị đóng góp từ mỗi chữ cái. 

Từ mối quan hệ này, sự đơn giản hóa then chốt xuất hiện. Nếu chuỗi có độ dài L thì B luôn chính xác là A + L. Điều này loại bỏ ngay một bậc tự do và chuyển nhiệm vụ sang kiểm tra xem một chuỗi nhất quán có độ dài nào đó có thể tồn tại hay không, sau đó xây dựng một chuỗi nếu có thể. 

Bản thân chuỗi đó đóng góp vào A thông qua việc lập chỉ mục bảng chữ cái thông thường, trong đó các chữ cái hoạt động giống như trọng số trong phạm vi từ 0 đến 25. Một chữ cái như`a`đóng góp 0,`b`đóng góp 1, và`z`đóng góp 25. B không phụ thuộc trực tiếp vào các trọng số này, chỉ phụ thuộc vào thực tế là mỗi ký tự tồn tại. 

Vì vậy, nhiệm vụ trở thành: chọn độ dài L và các chữ cái L sao cho tổng trọng số của chúng bằng A, đồng thời đảm bảo rằng B phải bằng A + L. Vì B đã cho nên L buộc phải là B − A. Điều này loại bỏ mọi quyền tự do trong việc chọn độ dài. 

Sau đó, vấn đề giảm xuống còn việc quyết định liệu chúng ta có thể biểu thị A dưới dạng tổng của các giá trị L, mỗi giá trị nằm trong khoảng từ 0 đến 25 hay không, và nếu có thì xây dựng một chuỗi như vậy. 

Các ràng buộc đầu vào ngụ ý rằng chúng ta đang làm việc trong một chế độ trong đó L có thể đủ lớn để việc ép buộc tất cả các chuỗi là không thể. Ngay cả đối với L vừa phải, việc liệt kê tất cả các kết hợp sẽ bùng nổ theo cấp số nhân. Do đó, mọi nghiệm hợp lệ đều phải tuyến tính trong L. 

Các trường hợp khó khăn xuất hiện khi tính khả thi không thành công. Nếu L nhỏ hơn 1 thì không có chuỗi hợp lệ vì chúng ta không thể xây dựng chuỗi có độ dài không dương. Một trường hợp lỗi khác phát sinh khi A quá lớn để có thể phân phối trên L ký tự, nghĩa là ngay cả khi mọi ký tự đều được`z`, chúng ta không thể đạt tới A. Giới hạn trên đó là 25 × L. Tương tự, A không thể âm hoặc vượt quá mức có thể đạt được trong giới hạn mỗi ký tự. 

Một ví dụ đơn giản minh họa tình trạng lỗi. Nếu A là 10 và L là 1 thì chúng ta sẽ cần một ký tự duy nhất đóng góp 10, điều này là không thể vì tối đa là 25 nên ký tự này thực sự ổn; ví dụ tốt hơn: nếu A là 30 và L là 1 thì điều đó là không thể. Ngược lại, nếu A là 0 và L là 1, chúng ta có thể sử dụng`a`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng một chuỗi có độ dài L và thử mọi cách gán các chữ cái có thể, kiểm tra xem tổng trọng số của chúng có bằng A hay không. Điều này đơn giản về mặt khái niệm vì nó phản ánh trực tiếp định nghĩa ràng buộc. Tuy nhiên, mỗi vị trí có 26 lựa chọn, do đó không gian tìm kiếm là 26^L, điều này trở nên không khả thi ngay cả đối với L khoảng 20. 

Cấu trúc của vấn đề cho phép quan sát mạnh mẽ hơn nhiều. Chúng ta không quan tâm đến hoán vị của các chữ cái, chỉ quan tâm đến việc liệu chúng ta có thể phân tách A thành L phần giới hạn hay không. Đây là một bài toán thành phần giới hạn cổ điển, nhưng có cấu trúc tham lam rất cụ thể: lấy sớm các giá trị lớn hơn không hạn chế tính khả thi trong tương lai miễn là chúng ta vẫn ở trong khả năng còn lại. 

Điều này cho phép xây dựng tham lam. Vì mỗi nhân vật có thể đóng góp nhiều nhất là 25 nên chúng tôi cố gắng chỉ định càng nhiều càng tốt ở mỗi bước, ưu tiên`z`bất cứ khi nào chúng ta vẫn cần các giá trị lớn của A. Nếu A còn lại nhỏ hơn 25, chúng ta đặt chính xác những gì cần thiết và điền vào phần còn lại bằng các số 0 ngầm thông qua`a`. 

Tính đúng đắn xuất phát từ thực tế là việc giảm A đi giá trị tối đa có thể có ở mỗi bước sẽ giữ chúng ta trong vùng khả thi miễn là tính khả thi ban đầu vẫn giữ nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^L) | O(L) | Quá chậm | 
| Xây dựng tham lam tối ưu | O(L) | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách chuyển đổi vấn đề thành kiểm tra độ dài và tính khả thi, sau đó xây dựng chuỗi một cách tham lam. 

1. Tính độ dài L cần thiết là B − A. Điều này rút ra trực tiếp từ quan sát rằng mỗi ký tự đóng góp thêm chính xác một đơn vị cho B so với A, do đó B luôn là A cộng với số ký tự. 
2. Kiểm tra xem L có ít nhất là 1 hay không. Nếu không, không có chuỗi nào tồn tại vì chúng ta không thể tạo thành một chuỗi khác trống có độ dài âm hoặc bằng 0. 
3. Kiểm tra xem A có nằm trong phạm vi khả thi so với L hay không, cụ thể là từ 0 đến 25 × L. Giới hạn dưới đảm bảo chúng ta không cần đóng góp âm và giới hạn trên đảm bảo thậm chí tối đa hóa tất cả các ký tự như`z`không vượt quá A 
4. Nếu điều kiện khả thi không thành công, hãy xuất -1 ngay lập tức. 
5. Khởi tạo một chuỗi kết quả trống và lặp lại chính xác L lần, tạo chuỗi ký tự theo ký tự từ trái sang phải. 
6. Ở mỗi bước, chọn phần đóng góp cho ký tự hiện tại là min(25, A còn lại). Đây là giá trị lớn nhất có thể mà chúng ta có thể chỉ định một cách an toàn mà không vượt quá yêu cầu còn lại. 
7. Chuyển phần đóng góp bằng số này thành một ký tự bằng cách bù trừ từ`'a'`, thêm nó vào kết quả và trừ nó khỏi A. 
8. Tiếp tục cho đến khi tất cả các vị trí được lấp đầy. Cuối cùng, A trở thành 0, nghĩa là tổng đóng góp khớp chính xác với giá trị yêu cầu. 

Tại sao nó hoạt động dựa trên việc duy trì một bất biến đơn giản: sau mỗi bước, A còn lại luôn có thể đạt được với số ký tự còn lại vì chúng tôi không bao giờ giảm A xuống dưới 0 và không bao giờ tiêu thụ nhiều hơn 25 cho mỗi ký tự. Vì tính khả thi ban đầu đảm bảo A ≤ 25 × L nên mức tiêu thụ tối đa tham lam không thể cản trở các bước trong tương lai. Mọi giá trị còn lại luôn có thể phân phối được vì mỗi bước đều bảo toàn điều kiện A 25 × còn lại_length. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())
b = int(input())

length = b - a

if length < 1 or a < 0 or a > 25 * length:
    print(-1)
    sys.exit()

ans = []

for _ in range(length):
    take = min(25, a)
    ans.append(chr(ord('a') + take))
    a -= take

print(''.join(ans))
```Việc triển khai bắt đầu bằng cách lấy độ dài bắt buộc từ mối quan hệ giữa A và B. Kiểm tra tính khả thi đảm bảo rằng chúng tôi không cố gắng xây dựng khi tổng mục tiêu A không thể được phân phối trên số lượng ký tự có sẵn. 

Vòng lặp tham lam là cốt lõi của giải pháp. Mỗi lần lặp lại tiêu tốn mức đóng góp tối đa có thể trong khi vẫn tôn trọng giới hạn trên của 25. Việc chuyển đổi`chr(ord('a') + take)`ánh xạ trọng số số trở lại thành chữ cái. 

Một điều tinh tế là đảm bảo rằng A không bao giờ được phép trở thành âm, điều này bị ngăn chặn bởi phép toán min. Một điều nữa là chúng ta không bao giờ cần quay lại vì tính khả thi đảm bảo những lựa chọn tham lam luôn để lại phần dư hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

A = 5, B = 8 

Ở đây L = 3. 

Chúng tôi xây dựng chuỗi từng bước. 

| Bước | Còn lại A | Đi | Nhân vật | Mới Còn Lại A | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | f | 0 | 
| 2 | 0 | 0 | một | 0 | 
| 3 | 0 | 0 | một | 0 | 

Kết quả là`faa`. Điều này có hiệu quả vì tổng trọng số là 5 và độ dài là 3, do đó B tự động trở thành 8. 

### Ví dụ 2 

đầu vào: 

A = 30, B = 35 

Ở đây L = 5. 

| Bước | Còn lại A | Đi | Nhân vật | Mới Còn Lại A | 
| --- | --- | --- | --- | --- | 
| 1 | 30 | 25 | z | 5 | 
| 2 | 5 | 5 | f | 0 | 
| 3 | 0 | 0 | một | 0 | 
| 4 | 0 | 0 | một | 0 | 
| 5 | 0 | 0 | một | 0 | 

Đầu ra trở thành`zfa aa`không có khoảng trống,`zfa aa`→`zfaaa`. 

Điều này chứng tỏ những khoản đóng góp lớn được dồn vào các vị trí ban đầu một cách tham lam như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) | Mỗi ký tự được tính chính xác một lần với công việc liên tục | 
| Không gian | O(L) | Chúng tôi lưu trữ chuỗi kết quả | 

Độ dài L bằng B − A, do đó thuật toán tuyến tính theo kích thước của đầu ra. Điều này là tối ưu vì mọi cấu trúc hợp lệ ít nhất phải ghi toàn bộ chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a = int(input())
    b = int(input())

    length = b - a

    if length < 1 or a < 0 or a > 25 * length:
        return "-1"

    ans = []
    for _ in range(length):
        take = min(25, a)
        ans.append(chr(ord('a') + take))
        a -= take

    return ''.join(ans)

# sample-like checks
assert run("0\n3\n") == "aaa"
assert run("5\n8\n") == "faa"

# minimum length invalid
assert run("5\n5\n") == "-1"

# single character max
assert run("25\n26\n") == "z"

# large greedy split
assert run("30\n35\n") == "zfaaa"

# all zeros case
assert run("0\n1\n") == "a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0, 3 | aaa | tất cả những đóng góp tối thiểu | 
| 5, 5 | -1 | độ dài không hợp lệ | 
| 25, 26 | z | ký tự tối đa đơn | 
| 30, 35 | zfaaa | tham lam chia tách A | 
| 0, 1 | một | trường hợp hợp lệ nhỏ nhất biên | 

## Vỏ cạnh 

Khi B bằng A, độ dài tính toán sẽ bằng 0. Thuật toán ngay lập tức bác bỏ trường hợp này vì một chuỗi hợp lệ phải chứa ít nhất một ký tự. Đối với đầu vào A = 10, B = 10, chúng ta nhận được L = 0 và đầu ra là -1 do không tồn tại cấu trúc nào. 

Khi A cực kỳ lớn so với L, ví dụ A = 100 và L = 3, việc kiểm tra tính khả thi không thành công vì thậm chí ba`z`ký tự chỉ đóng góp 75. Thuật toán dừng chính xác trước khi thử xây dựng. 

Khi A bằng 0, cấu trúc suy biến thành tất cả`a`nhân vật. Với A = 0 và bất kỳ L hợp lệ nào, mỗi lần lặp đều nhận 0, tạo ra một chuỗi các số 0 có trọng số, tương ứng với các lần lặp lại.`a`.
