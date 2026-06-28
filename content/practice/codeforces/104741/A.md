---
title: "CF 104741A - A+B\u95ee\u9898"
description: "Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn chứa ba số được viết trong cùng một hệ thống số vị trí không xác định với cơ số $X$, trong đó $X$ là một số nguyên nào đó nằm trong khoảng từ 2 đến 16."
date: "2026-06-28T23:18:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104741
codeforces_index: "A"
codeforces_contest_name: "The 10th Jimei University Programming Contest"
rating: 0
weight: 104741
solve_time_s: 51
verified: true
draft: false
---

[CF 104741A - A+B\u95ee\u9898](https://codeforces.com/problemset/problem/104741/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn chứa ba số được viết trong cùng một hệ thống số vị trí không xác định với cơ số$X$, Ở đâu$X$là một số nguyên nằm trong khoảng từ 2 đến 16. Các chữ số được viết bằng ký tự`0-9`Và`A-F`, vì vậy chúng có thể biểu thị các giá trị lên tới 15. 

Đối với mỗi truy vấn, chúng ta được biết rằng số thứ nhất cộng với số thứ hai bằng số thứ ba khi diễn giải theo cơ số.$X$. Bản thân cơ sở không xác định và các truy vấn khác nhau có thể tương ứng với các cơ sở hợp lệ khác nhau. Nhiệm vụ của chúng tôi là phục hồi bất kỳ căn cứ nào$X$điều đó làm cho đẳng thức đúng cho mỗi truy vấn. 

Các ràng buộc ngụ ý đến$10^5$truy vấn, vì vậy mỗi truy vấn phải được kiểm tra trong thời gian cố định hoặc gần như không đổi. Một giải pháp cố gắng chuyển đổi hoàn toàn các chuỗi thành số nguyên lớn bằng cách sử dụng số học lặp lại cho từng cơ sở một cách độc lập vẫn sẽ hoạt động nếu được giới hạn cẩn thận, nhưng mọi thứ bậc hai về độ dài chữ số cho mỗi truy vấn sẽ gặp rủi ro trong trường hợp lặp lại trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất phát từ các chữ số không hợp lệ. Nếu một số có chứa một chữ số như`F`, thì cơ sở bất kỳ$X \le 15$ngay lập tức không hợp lệ. Một chế độ lỗi khác là bỏ qua việc truyền mang: ngay cả khi tổng các chữ số khớp cục bộ, việc kiểm tra mang bị thiếu có thể chấp nhận các cơ sở không hợp lệ một cách không chính xác. 

Ví dụ, hãy xem xét một trường hợp trong đó$A = 1F$,$B = 1$, Và$S = 20$. Trong cơ sở 16 điều này có tác dụng, nhưng ở cơ sở 15 nó không hợp lệ vì chữ số`F`không được phép. Một cách tiếp cận đơn giản chỉ kiểm tra số học sau khi chuyển đổi từng ký tự mà không xác thực phạm vi chữ số sẽ chấp nhận những trường hợp như vậy một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi truy vấn, hãy thử mọi cơ sở$X$từ 2 đến 16, chuyển đổi$A$,$B$, Và$S$thành các số nguyên trong cơ sở đó và kiểm tra xem đẳng thức có đúng không. Vì phạm vi cơ sở nhỏ và cố định nên điều này cho thấy tính khả thi. 

Tuy nhiên, chi phí chính là chuyển đổi. Nếu chúng ta chuyển đổi một số bằng cách nhân và cộng lặp lại cho mỗi chữ số thì mỗi chuyển đổi là$O(L)$, Ở đâu$L$là số chữ số. Làm điều này ba lần cho mỗi cơ sở sẽ mang lại$O(3L)$, và trên 15 căn cứ chúng tôi nhận được$O(45L)$mỗi truy vấn. Với$10^5$truy vấn, điều này vẫn ổn trong Python nếu được triển khai rõ ràng, nhưng chúng ta có thể làm tốt hơn bằng cách tránh chuyển đổi hoàn toàn lặp lại. 

Quan sát quan trọng là chúng ta không cần các giá trị nguyên đầy đủ. Chúng ta chỉ cần kiểm tra xem phép cộng có đúng với một cơ số nhất định hay không. Điều đó có thể được xác minh từng chữ số bằng mô phỏng nhớ, giống hệt như phép cộng thủ công. Điều này tránh việc xây dựng các số nguyên lớn và giảm mỗi lần kiểm tra thành quét tuyến tính trên các chữ số một lần. 

Vì vậy, chiến lược tối ưu là thử từng cơ sở và xác nhận phương trình bằng cách sử dụng phép cộng chữ số với phép lan truyền mang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chuyển đổi đầy đủ trên mỗi cơ sở) |$O(T \cdot 16 \cdot L)$|$O(1)$thêm | Đã chấp nhận | 
| Tối ưu (kiểm tra mang theo chữ số) |$O(T \cdot 16 \cdot L)$|$O(1)$thêm | Đã chấp nhận | 

Sự cải thiện chủ yếu nằm ở các hệ số không đổi và hành vi bộ nhớ hơn là độ phức tạp tiệm cận, nhưng nó đơn giản hóa tính chính xác và tránh việc xây dựng số nguyên không cần thiết. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. Đối với một truy vấn cố định, chúng tôi xác định cơ số nhỏ nhất có thể từ các chữ số của nó rồi kiểm tra tất cả các cơ số hợp lệ lên đến 16. 

1. Trích xuất tất cả các giá trị chữ số xuất hiện trong$A$,$B$, Và$S$, và tính giá trị chữ số lớn nhất. Cơ sở phải lớn hơn giá trị này, nếu không biểu diễn sẽ không hợp lệ. 
2. Đối với mỗi cơ sở ứng cử viên$X$từ$\max\_digit + 1$đến 16, cố gắng xác minh xem$A + B = S$giữ trong căn cứ$X$. 
3. Để xác minh cơ số, hãy mô phỏng phép cộng từ chữ số có nghĩa nhỏ nhất đến chữ số có nghĩa nhất. Chúng ta đảo ngược cả ba chuỗi để chỉ số 0 tương ứng với vị trí đơn vị. 
4. Duy trì giá trị khởi đầu là 0. Tại mỗi vị trí chữ số, hãy tính tổng các chữ số tương ứng từ$A$Và$B$, thêm số mang và so sánh với chữ số của$S$modulo$X$. Nếu xảy ra sự không phù hợp, hãy loại bỏ đế này ngay lập tức. 
5. Sau khi xử lý tất cả các chữ số, đảm bảo rằng mọi chữ số còn lại khớp với các chữ số còn lại của$S$. Nếu mọi thứ đều nhất quán, hãy chấp nhận cơ sở này và xuất nó. 

Chúng tôi dừng lại ở cơ sở hợp lệ đầu tiên vì mọi câu trả lời hợp lệ đều được chấp nhận. 

### Tại sao nó hoạt động 

Thuật toán thực thi tính nhất quán cấp chữ số chính xác của phép cộng theo cơ sở$X$. Biến mang nắm bắt tất cả các phụ thuộc chữ số chéo, do đó mọi vị trí đều được xác thực theo cùng các quy tắc số học xác định hệ thống chữ số vị trí. Bởi vì chúng tôi thử tất cả các cơ sở có thể có trong phạm vi khả thi duy nhất nên tính đầy đủ được đảm bảo và vì mỗi ứng cử viên được kiểm tra bằng các quy tắc số học chính xác nên tính đúng đắn được đảm bảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def val(c):
    if '0' <= c <= '9':
        return ord(c) - ord('0')
    return ord(c) - ord('A') + 10

def ok(a, b, s, base):
    carry = 0
    i = len(a) - 1
    j = len(b) - 1
    k = len(s) - 1

    while k >= 0:
        av = val(a[i]) if i >= 0 else 0
        bv = val(b[j]) if j >= 0 else 0
        sv = val(s[k])

        total = av + bv + carry
        if total % base != sv:
            return False
        carry = total // base

        i -= 1
        j -= 1
        k -= 1

    while i >= 0 or j >= 0:
        av = val(a[i]) if i >= 0 else 0
        bv = val(b[j]) if j >= 0 else 0
        total = av + bv + carry
        carry = total // base
        if total % base != 0:
            return False
        i -= 1
        j -= 1

    return carry == 0

def solve():
    T = int(input())
    for _ in range(T):
        a, b, s = input().split()

        max_digit = 0
        for ch in a + b + s:
            max_digit = max(max_digit, val(ch))

        for base in range(max_digit + 1, 17):
            if ok(a, b, s, base):
                print(base)
                break

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chuyển đổi các ký tự thành các giá trị chữ số một cách nhất quán trên toàn bộ dữ liệu đầu vào. các`ok`Hàm này là trình xác minh cốt lõi: nó mô phỏng phép cộng từ phải sang trái trong khi thực thi các ràng buộc cơ sở thông qua số học mô-đun và truyền bá mang. Vòng lặp thứ hai xử lý các chữ số còn sót lại khi một số dài hơn số kia, đảm bảo không còn phần đóng góp ẩn nào. 

Một sai lầm phổ biến là quên rằng cơ số phải lớn hơn mọi chữ số có mặt. Việc kiểm tra đó tránh được sự mô phỏng không cần thiết trong các cơ sở không hợp lệ và ngăn chặn việc chấp nhận sai. 

## Ví dụ đã hoạt động 

Xem xét truy vấn`1 1 10`. 

Chúng ta kiểm tra các cơ số có thể bắt đầu từ 2. Trong cơ số 2, chữ số`1 + 1`sản xuất`10`, khớp với số thứ ba. 

| Vị trí | Một chữ số | Chữ số B | Chữ số S | mang vào | tổng hợp | cơ sở mod | thực hiện | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| đơn vị | 1 | 1 | 0 | 0 | 2 | 0 | 1 | 
| tiếp theo | 0 | 0 | 1 | 1 | 1 | 1 | 0 | 

Điều này xác nhận cơ sở 2 là hợp lệ. 

Bây giờ hãy xem xét`F F 1E`. 

Chúng tôi kiểm tra cơ số 16. Trong hệ thập lục phân,`F + F = 1E`. 

| Vị trí | Một chữ số | Chữ số B | Chữ số S | mang vào | tổng hợp | mod 16 | thực hiện | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| đơn vị | 15 | 15 | 14 | 0 | 30 | 14 | 1 | 
| tiếp theo | 0 | 0 | 1 | 1 | 1 | 1 | 0 | 

Điều này xác nhận cơ sở 16 là hợp lệ. 

Các dấu vết cho thấy tính chính xác phụ thuộc hoàn toàn vào sự lan truyền mang nhất quán hơn là sự bình đẳng của chuỗi trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot 16 \cdot L)$| Mỗi truy vấn thử tối đa 15 cơ sở, mỗi truy vấn được xác thực bằng cách quét tuyến tính trên các chữ số | 
| Không gian |$O(1)$| Chỉ các biến bổ sung không đổi cho mô phỏng mang | 

Các ràng buộc cho phép lên đến$10^5$các truy vấn, nhưng mỗi truy vấn chỉ liên quan đến công việc có hệ số không đổi nhỏ trên các chuỗi ngắn, do đó giải pháp phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# basic samples
assert run("2\n1 1 10\nF F 1E\n") == "2\n16"

# minimal digits
assert run("1\n0 0 0\n") == "2"

# base boundary digit 15 forces base 16
assert run("1\nF 0 F\n") == "16"

# carry-heavy case
assert run("1\n1F 1 20\n") == "16"

# multiple queries
assert run("3\n1 1 10\nF F 1E\n1 0 1\n") == "2\n16\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 10`|`2`| đế nhỏ nhất có mang theo | 
|`F F 1E`|`16`| xử lý chữ số cơ sở tối đa | 
|`0 0 0`|`2`| trường hợp không tầm thường | 
|`1F 1 20`|`16`| mang qua các độ dài khác nhau | 
| nhiều truy vấn | hỗn hợp | độ chính xác hàng loạt | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi tất cả các chữ số chỉ hợp lệ ở cơ số 16, chẳng hạn như liên quan đến`F`. Thuật toán đặt chính xác cơ số tối thiểu thành 16 và tránh việc kiểm tra các cơ số nhỏ hơn không cần thiết. 

Đối với đầu vào`F 0 F`, chữ số lớn nhất là 15 nên chỉ kiểm tra cơ số 16. Mô phỏng mang theo ngay lập tức xác nhận`F + 0 = F`, tạo ra số 0 và khớp chữ số chính xác, do đó đầu ra là 16. 

Một trường hợp cạnh khác là khi các số có độ dài khác nhau. Vì`1 0 1`, số thứ hai không đóng góp gì ngoài một chữ số của nó và logic mang xử lý chính xác các vị trí bị thiếu bằng 0. Thuật toán vẫn tạo ra cơ số 2 mà không có cách viết hoa đặc biệt vì các chữ số vắng mặt được mô hình hóa một cách tự nhiên dưới dạng số 0 trong số học vị trí.
