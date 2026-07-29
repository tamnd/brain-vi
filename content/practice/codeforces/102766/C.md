---
title: "CF 102766C - Chuỗi khung thông thường"
description: "Chúng ta được cung cấp một chuỗi chỉ gồm các dấu ngoặc mở (và dấu ngoặc đóng). Chúng tôi muốn chuyển đổi nó thành một chuỗi dấu ngoặc thông thường."
date: "2026-07-28T23:37:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102766
codeforces_index: "C"
codeforces_contest_name: "Codedigger Training Contest -String"
rating: 0
weight: 102766
solve_time_s: 84
verified: true
draft: false
---

[CF 102766C - Trình tự khung thông thường](https://codeforces.com/problemset/problem/102766/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ gồm các dấu ngoặc mở`(`và dấu ngoặc đóng`)`. Chúng tôi muốn chuyển đổi nó thành một chuỗi dấu ngoặc thông thường. Hai thao tác được phép có tác dụng khác nhau: xóa một ký tự sẽ loại bỏ hoàn toàn một dấu ngoặc, trong khi di chuyển một ký tự đến cuối vẫn giữ nguyên dấu ngoặc nhưng thay đổi vị trí của nó. 

Một chuỗi dấu ngoặc thông thường có hai yêu cầu. Tổng số dấu ngoặc mở và đóng phải bằng nhau và khi đọc chuỗi từ trái sang phải, số dấu ngoặc mở nhìn thấy cho đến nay không bao giờ được nhỏ hơn số dấu ngoặc đóng. Đầu vào chứa một số trường hợp độc lập, mỗi trường hợp có độ dài chuỗi và chi phí của hai thao tác, theo sau là chuỗi ngoặc. Đầu ra là chi phí tối thiểu cần thiết để làm cho mọi trường hợp đều hợp lệ. 

Tổng chiều dài của tất cả các chuỗi tối đa là$10^5$, do đó nghiệm phải gần tuyến tính. Một nghiệm bậc hai sẽ quá chậm vì một chuỗi có độ dài$10^5$sẽ tạo ra xung quanh$10^{10}$cặp vị trí để kiểm tra. Giá trị chi phí lớn cũng có nghĩa là việc triển khai phải sử dụng số nguyên 64 bit vì câu trả lời có thể vượt quá phạm vi số nguyên 32 bit. 

Một số trường hợp rất dễ xử lý sai. Nếu chuỗi đã hợp lệ thì câu trả lời là 0. 

Ví dụ:```
Input:
4 5 10
()()
```Đầu ra đúng là:```
0
```Giải pháp luôn thực hiện một số thao tác để "sửa" chuỗi sẽ không thành công ở đây. 

Một trường hợp quan trọng khác là khi chuỗi có số lượng loại dấu ngoặc không đúng.```
Input:
5 100 1
)))))
```Đầu ra đúng là:```
5000000000
```Có thêm năm dấu ngoặc đóng. Việc di chuyển chúng không làm thay đổi số lượng, vì vậy giải pháp khả thi duy nhất là xóa cả năm. Một giải pháp bất cẩn chỉ đếm các vấn đề về thứ tự có thể trả về một giá trị nhỏ hơn nhiều một cách không chính xác. 

Trường hợp phức tạp thứ ba xuất hiện khi việc di chuyển rẻ hơn việc xóa.```
Input:
2 100 1
)(
```Đầu ra đúng là:```
1
```Trình tự có số đúng trong mỗi dấu ngoặc, nhưng thứ tự không hợp lệ. Việc xóa sẽ tốn nhiều chi phí hơn so với việc xóa lần đầu tiên`)`đến cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử di chuyển mọi bộ ký tự có thể và xóa mọi bộ ký tự có thể có, sau đó kiểm tra xem chuỗi còn lại có đều đặn hay không. Điều này đúng vì mọi chuỗi thao tác có thể đều được biểu diễn nhưng nó hoàn toàn không thực tế. Ngay cả việc quyết định nhân vật nào sẽ di chuyển cũng đã mang lại kết quả$2^n$khả năng, và việc kiểm tra mọi khả năng là không thể đối với$n=10^5$. 

Quan sát chính xuất phát từ việc so sánh hai chi phí hoạt động. Nếu việc xóa không đắt hơn việc di chuyển thì không bao giờ có lý do để di chuyển dấu ngoặc. Một nước đi chỉ thay đổi vị trí của một khung và chi phí ít nhất bằng việc loại bỏ nó. Chiến lược rẻ nhất chỉ đơn giản là xóa các dấu ngoặc ngăn không cho chuỗi trở nên đều đặn. 

Khi di chuyển rẻ hơn, chúng ta nên sử dụng di chuyển bất cứ khi nào vấn đề chỉ là đặt hàng. Tuy nhiên, việc di chuyển không thể thay đổi số lượng dấu ngoặc mở và đóng, vì vậy mọi khác biệt giữa hai số đếm vẫn phải được loại bỏ bằng cách xóa. 

Sau những lần xóa cần thiết, chuỗi còn lại có số lượng bằng nhau ở cả hai dấu ngoặc. Đối với một chuỗi như vậy, số bước di chuyển tối thiểu cần thiết chính xác là số dấu ngoặc đóng chưa khớp gặp phải khi quét từ trái sang phải. Mỗi dấu ngoặc đóng như vậy sẽ tạo ra một tiền tố trong đó số dư trở thành số âm và việc di chuyển các dấu ngoặc đóng đó về cuối sẽ cố định thứ tự tiền tố. Không ít bước di chuyển nào có thể hoạt động vì mọi điểm phủ định đều cần loại bỏ một dấu ngoặc đóng có vấn đề khỏi tiền tố. 

Câu hỏi còn lại là nên bỏ dấu ngoặc nào khi di chuyển rẻ hơn. Nếu có quá nhiều dấu ngoặc mở, việc loại bỏ dấu ngoặc mở gần cuối là tốt nhất vì nó ảnh hưởng đến ít tiền tố nhất. Nếu có quá nhiều dấu ngoặc đóng, việc loại bỏ dấu ngoặc đóng gần đầu là tốt nhất vì nó làm tăng số dư sau này càng nhiều càng tốt. Những lựa chọn này mang lại sự cân bằng tối đa có thể và giảm thiểu số lần di chuyển cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số dấu ngoặc mở và đóng. Nếu việc xóa rẻ hơn hoặc bằng việc di chuyển, hãy đếm trực tiếp dấu ngoặc mở và đóng chưa khớp. Câu trả lời là số lần xóa nhân với chi phí xóa. 
2. Nếu việc di chuyển rẻ hơn, trước tiên hãy loại bỏ các dấu ngoặc thừa không thể tránh khỏi. Nếu có nhiều dấu ngoặc mở hơn, hãy loại bỏ các dấu ngoặc mở thừa ở cuối. Nếu có nhiều dấu ngoặc đóng hơn, hãy loại bỏ các dấu ngoặc đóng thừa ngay từ đầu. Điều này để lại số dư tiền tố lớn nhất có thể. 
3. Quét trình tự còn lại và giữ số dư đang hoạt động trong đó`(`thêm một và`)`trừ một. Bất cứ khi nào số dư trở nên âm, một dấu ngoặc đóng phải được chuyển xuống cuối. Đếm xem điều này xảy ra bao nhiêu lần. 
4. Nhân số lần di chuyển với chi phí di chuyển và cộng chi phí xóa từ bước đầu tiên. 

Tại sao nó hoạt động: sau khi xóa yêu cầu, chuỗi còn lại có số dấu ngoặc mở và đóng bằng nhau. Chuỗi dấu ngoặc thông thường chỉ thất bại khi một số tiền tố chứa quá nhiều dấu ngoặc đóng. Việc di chuyển chính xác các dấu ngoặc đóng bổ sung đó đến cuối sẽ loại bỏ mọi vi phạm như vậy và mỗi vi phạm tương ứng với một dấu ngoặc đóng riêng biệt phải để lại tiền tố. Các lựa chọn xóa sẽ duy trì số dư tiền tố lớn nhất có thể, do đó chúng giảm thiểu số lần di chuyển cần thiết sau đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, a, b, s):
    opens = s.count('(')
    closes = n - opens

    if a <= b:
        balance = 0
        bad_close = 0
        for c in s:
            if c == '(':
                balance += 1
            else:
                balance -= 1
                if balance < 0:
                    bad_close += 1
                    balance = 0
        bad_open = balance
        return (bad_close + bad_open) * a

    if opens > closes:
        extra = opens - closes
        removed = 0
        chars = []
        for c in reversed(s):
            if c == '(' and removed < extra:
                removed += 1
            else:
                chars.append(c)
        chars.reverse()
        s = ''.join(chars)

        balance = 0
        moves = 0
        for c in s:
            if c == '(':
                balance += 1
            else:
                balance -= 1
                if balance < 0:
                    moves += 1
                    balance = 0

        return extra * a + moves * b

    if closes > opens:
        extra = closes - opens
        removed = 0
        chars = []
        for c in s:
            if c == ')' and removed < extra:
                removed += 1
            else:
                chars.append(c)
        s = ''.join(chars)

        balance = 0
        moves = 0
        for c in s:
            if c == '(':
                balance += 1
            else:
                balance -= 1
                if balance < 0:
                    moves += 1
                    balance = 0

        return extra * a + moves * b

    balance = 0
    moves = 0
    for c in s:
        if c == '(':
            balance += 1
        else:
            balance -= 1
            if balance < 0:
                moves += 1
                balance = 0

    return moves * b

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        n, a, b = map(int, input().split())
        s = input().strip()
        ans.append(str(solve_case(n, a, b, s)))
    print('\n'.join(ans))

if __name__ == "__main__":
    main()
```Nhánh đầu tiên xử lý trường hợp việc xóa chiếm ưu thế. Quá trình quét sẽ đếm các dấu ngoặc đóng xuất hiện trước khi tồn tại đủ dấu ngoặc mở và số dư còn lại sau khi quét chính xác là số dấu ngoặc mở chưa khớp. 

Khi di chuyển rẻ hơn, mã trước tiên chỉ loại bỏ các dấu ngoặc thừa không thể tránh khỏi. Hướng loại bỏ quan trọng. Dấu ngoặc mở bổ sung sẽ bị xóa khỏi phía bên phải vì việc xóa chúng sớm hơn sẽ làm giảm nhiều số dư tiền tố. Các dấu ngoặc đóng bổ sung được loại bỏ ở phía bên trái vì những dấu ngoặc đó là những dấu ngoặc gây tổn hại đến tiền tố nhiều nhất. 

Lần quét cuối cùng đếm các bước di chuyển cần thiết. Số dư được đặt lại về 0 sau khi tìm thấy giá trị âm vì dấu ngoặc đóng không khớp đó sẽ bị chuyển đi. Chi phí có thể đạt$5 \times 10^{13}$, do đó số nguyên Python xử lý phạm vi được yêu cầu một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 100 1
)(
```Hoạt động di chuyển rẻ hơn. 

| Nhân vật | Số dư | Di chuyển | 
| --- | --- | --- | 
|`)`| -1 trở thành 0 | 1 | 
|`(`| 1 | 1 | 

Chuỗi có số lượng bằng nhau nên không cần xóa. Một dấu ngoặc đóng được chuyển xuống cuối, tạo ra`()`. 

Kết quả là:```
1
```Điều này chứng tỏ rằng các vấn đề về thứ tự nên được giải quyết bằng các bước di chuyển khi chúng rẻ hơn so với việc xóa. 

### Ví dụ 2 

đầu vào:```
3 1000 1
()(
```Có thêm một dấu ngoặc mở. 

| Nhân vật | Hành động | Số dư còn lại | 
| --- | --- | --- | 
|`(`| giữ | 1 | 
|`)`| giữ | 0 | 
|`(`| xóa như dư thừa | 0 | 

Dấu ngoặc mở thêm được loại bỏ ở cuối. Không cần di chuyển. 

Kết quả là:```
1000
```Điều này cho thấy tại sao nên xóa dấu ngoặc mở thừa ở phía bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khung chỉ được quét một số lần không đổi. | 
| Không gian | O(n) | Việc triển khai lưu trữ chuỗi đã lọc sau khi xóa. | 

Tổng của tất cả độ dài chuỗi nhiều nhất là$10^5$, do đó, một giải pháp tuyến tính dễ dàng nằm trong giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n, a, b = map(int, input().split())
        s = input().strip()
        res.append(str(solve_case(n, a, b, s)))
    sys.stdin = old_stdin
    return "\n".join(res)

def solve_case(n, a, b, s):
    opens = s.count('(')
    closes = n - opens

    if a <= b:
        bal = bad = 0
        for c in s:
            if c == '(':
                bal += 1
            else:
                bal -= 1
                if bal < 0:
                    bad += 1
                    bal = 0
        return (bad + bal) * a

    if opens > closes:
        extra = opens - closes
        rem = 0
        arr = []
        for c in reversed(s):
            if c == '(' and rem < extra:
                rem += 1
            else:
                arr.append(c)
        s = ''.join(reversed(arr))
        ans = extra * a
    elif closes > opens:
        extra = closes - opens
        rem = 0
        arr = []
        for c in s:
            if c == ')' and rem < extra:
                rem += 1
            else:
                arr.append(c)
        s = ''.join(arr)
        ans = extra * a
    else:
        ans = 0

    bal = moves = 0
    for c in s:
        if c == '(':
            bal += 1
        else:
            bal -= 1
            if bal < 0:
                moves += 1
                bal = 0

    return ans + moves * b

assert run("""5
2 100 1
)(
2 1 100
)(
3 1 1000
)()
3 1000 1
()(
5 1000000000 1
)))))
""") == """1
2
1
1000
5000000000"""

assert run("""1
1 5 2
(
""") == "5"

assert run("""1
6 10 1
))((()
""") == "2"

assert run("""1
4 7 3
()()
""") == "0"

assert run("""1
5 1 2
)))))
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 5 2 / (`|`5`| Kích thước tối thiểu và khả năng xử lý khung mở chưa từng có | 
|`1 / 6 10 1 / ))((()`|`2`| Di chuyển dấu ngoặc đóng sai vị trí | 
|`1 / 4 7 3 / ()()`|`0`| Trình tự đã hợp lệ | 
|`1 / 5 1 2 / )))))`|`5`| Yêu cầu xóa lớn | 

## Vỏ cạnh 

cho`)(`với các nước đi rẻ, thuật toán giữ cả hai dấu ngoặc vì số lượng đã bằng nhau. Quá trình quét tìm thấy đầu tiên`)`tạo ra số dư âm nên nó tính một nước đi. Sau khi di chuyển dấu ngoặc đó về cuối, kết quả là`()`, đưa ra chi phí tối thiểu. 

Vì`()(`với những nước đi rẻ tiền, có nhiều dấu ngoặc mở hơn dấu ngoặc đóng. Thuật toán loại bỏ kết quả cuối cùng`(`bởi vì đó là cách xóa ít gây hại nhất. Chuỗi còn lại đã là chuỗi thông thường nên chi phí duy nhất là xóa. 

Vì`)))))`, không có chuyển động nào có thể giúp được vì chuyển động không bao giờ làm thay đổi số lượng dấu ngoặc đóng. Thuật toán đi vào đường dẫn xóa và loại bỏ tất cả năm ký tự, tạo ra chuỗi dấu ngoặc thông thường trống. Chi phí trả lại là$5 \times 10^9$, phù hợp với việc xử lý phạm vi 64 bit được yêu cầu.
