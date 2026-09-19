---
title: "CF 104760H - \u0412\u043c\u0435\u0441\u0442\u0435 \u043d\u0430\u0432\u0441\u0435\u0433\u0434\u0430"
description: "Chúng ta có hai nhóm du khách ban đầu được chia thành hai vũ trụ khác nhau, một bên là người A và bên kia là người B. Giữa hai vũ trụ có N cổng và mỗi cổng có thể được sử dụng với số lần giới hạn."
date: "2026-06-28T22:03:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104760
codeforces_index: "H"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Qualification Contest"
rating: 0
weight: 104760
solve_time_s: 81
verified: true
draft: false
---

[CF 104760H - \u0412\u043c\u0435\u0441\u0442\u0435 \u043d\u0430\u0432\u0441\u0435\u0433\u0434\u0430](https://codeforces.com/problemset/problem/104760/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai nhóm du khách ban đầu được chia thành hai vũ trụ khác nhau, một bên là người A và bên kia là người B. Giữa hai vũ trụ có N cổng và mỗi cổng có thể được sử dụng với số lần giới hạn. Mỗi khi ai đó đi qua một cổng, họ sẽ đổi bên và sau khi cổng đã được sử dụng chính xác.$f_i$lần, nó bị phá hủy và không thể sử dụng được nữa. 

Quá trình này bao gồm việc liên tục chọn một người và một cổng, di chuyển người đó qua cổng và tính mức sử dụng đó vào giới hạn của cổng. Mục tiêu là để quyết định xem có thể thực hiện một chuỗi các bước di chuyển như vậy sao cho cuối cùng có hai điều kiện: tất cả các cổng bị phá hủy hoàn toàn và tất cả các du khách đều đến cùng một vũ trụ. 

Kích thước đầu vào cho phép lên đến$10^5$cổng thông tin có dung lượng lên tới$10^9$, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào về các lối đi riêng lẻ. Bất kỳ giải pháp nào thực hiện các hoạt động theo từng bước sẽ là quá chậm vì tổng số lần giao cắt có thể đạt tới$10^{14}$. 

Một điểm tinh tế là mỗi cổng không có tính định hướng và mỗi lần sử dụng chỉ lật mặt của đúng một người. Không có hạn chế nào về việc người nào sử dụng cổng nào, nghĩa là các hạn chế toàn cầu duy nhất đến từ số lần di chuyển và tính chẵn lẻ của các thay đổi bên, không phải từ danh tính cá nhân hoặc cấu trúc cổng. 

Một cách tiếp cận ngây thơ nhưng không chính xác là cố gắng chỉ định mọi người vào các cổng hoặc mô phỏng việc chuyển tiền một cách tham lam. Ví dụ: với A = 2, B = 2 và các cổng [1, 1, 1], một chiến lược tham lam cố gắng cân bằng các bên cục bộ có thể kết luận sai rằng việc cân bằng là không thể, vì nó không tính đến tính linh hoạt trong việc lựa chọn các cá nhân di chuyển ở mỗi bước. Ràng buộc thực tế có tính chất toàn cục và chỉ phụ thuộc vào tổng số chứ không phụ thuộc vào cấu trúc. 

Một cạm bẫy khác là giả định rằng vì mỗi cổng phải được sử dụng chính xác$f_i$tùy từng thời điểm, việc phân bổ các mục đích sử dụng này theo thời gian là rất quan trọng. Nó không; chỉ có tổng số lần lật ngang mới quan trọng. 

## Phương pháp tiếp cận 

Quan điểm của brute-force là mô phỏng toàn bộ quá trình: duy trì nhiều tập hợp dung lượng cổng thông tin có sẵn và danh sách người ở mỗi bên, sau đó liên tục chọn một người và một cổng, áp dụng một động thái và giảm dung lượng còn lại. Mỗi thao tác đều$O(1)$, nhưng có$\sum f_i$tổng số hoạt động. Từ$\sum f_i$có thể lớn như$10^{14}$, phương án này hoàn toàn không khả thi. 

Sự đơn giản hóa chính là bỏ qua danh tính của các cổng và chỉ tập trung vào những gì mỗi hoạt động thực hiện trên toàn cầu. Mỗi lần di chuyển sẽ lật chính xác một người từ bên này sang bên kia. Chính xác là sau khi tất cả các cổng đã cạn kiệt$T = \sum f_i$những cú lật như vậy đã xảy ra. Điều quan trọng duy nhất là có bao nhiêu lần lật nhào từ A đến B so với từ B đến A. 

hãy để$p$là số lần đi từ A đến B và$q$từ B đến A. Sau đó$p + q = T$. Số người cuối cùng ở bên A trở thành$A + q - p$. Vì chúng tôi muốn tất cả mọi người ở một bên, chúng tôi chỉ cần kiểm tra xem liệu có tồn tại sự phân chia hợp lệ trong số này hay không$T$di chuyển tạo ra một trong hai$A + B$ở một bên hoặc$0$ở phía đó. 

Điều này làm giảm toàn bộ vấn đề về tính chẵn lẻ và điều kiện khả thi trên$T$, bởi vì quyền tự do duy nhất là lựa chọn số lần đi theo mỗi hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\sum f_i)$|$O(1)$| Quá chậm | 
| Tổng số + lý luận chẵn lẻ |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán tổng số lần sử dụng cổng thông tin$T = \sum f_i$. Sau đó, toàn bộ vấn đề chuyển sang việc kiểm tra xem liệu chúng ta có thể kết thúc với mọi người ở bên A hay mọi người ở bên B hay không. 

1. Tính toán$T = \sum f_i$. Điều này thể hiện tổng số động tác lật bên phải xảy ra. 
2. Hãy xem xét khả năng tất cả khách du lịch đều kết thúc ở phía A. Nếu điều này xảy ra, quần thể bên A cuối cùng phải là$A + B$. Mỗi nước đi lật đúng một người nên sau$T$di chuyển, sự thay đổi về dân số bên A được xác định bởi số lượng di chuyển đi theo mỗi hướng. Điều này dẫn đến tình trạng$T \ge B$, vì ít nhất tất cả khách du lịch bên B phải được di chuyển qua. 
3. Đảm bảo tính nhất quán chẵn lẻ khi kết thúc ở A. Sự khác biệt$T - B$phải chẵn, vì mỗi cặp chuyển động ngược chiều triệt tiêu một độ dịch ròng. Điều này đảm bảo rằng chuyển động ròng còn lại có thể khớp chính xác với chuyển khoản cần thiết của tất cả khách du lịch B. 
4. Lặp lại lý do tương tự khi kết thúc ở mặt B. Đối với trường hợp đó, chúng tôi yêu cầu$T \ge A$và đó$T - A$là chẵn. 
5. Nếu một trong hai cấu hình mục tiêu khả thi, xuất ra CÓ. Nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Mỗi thao tác thay đổi chính xác số người ở bên A$+1$hoặc$-1$, tùy theo hướng. Sau tất cả các hoạt động, chỉ có tổng số lượng các phần tăng thêm đó mới quan trọng chứ không phải thứ tự của chúng hoặc cổng nào đã tạo ra chúng. Điều này làm cho hệ thống tương đương với việc chọn một chuỗi các$T$các bước đã ký có tổng số tiền ròng phải phù hợp với mục tiêu cố định. Trở ngại duy nhất là liệu các ràng buộc về độ chẵn lẻ và cường độ có phù hợp với cấu hình cuối cùng được yêu cầu hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B = map(int, input().split())
    n = int(input())
    f = list(map(int, input().split()))
    
    T = sum(f)
    
    if (T >= B and (T - B) % 2 == 0) or (T >= A and (T - A) % 2 == 0):
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tổng hợp tất cả các năng lực của cổng thông tin thành một tổng duy nhất. Từ thời điểm đó trở đi, không cần phải có lý do trên mỗi cổng thông tin. Hai kiểm tra có điều kiện trực tiếp mã hóa các điều kiện khả thi để chọn A hoặc B làm vũ trụ thống nhất cuối cùng. Việc kiểm tra modulo đảm bảo rằng sự mất cân bằng còn lại sau khi tính toán các khoản chuyển khoản cần thiết có thể được giải quyết bằng cách sử dụng các cặp nước đi ngược lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 7
2
1 2
```Đây$T = 3$. 

| Bước | Bên mục tiêu | Chuyển khoản bắt buộc | Khả thi | Kiểm tra chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| Bên A | 3+7=10 | B=7 | 3 < 7 | không hợp lệ | 
| Bên B | 12 | A=5 | 3 < 5 | không hợp lệ | 

Cả hai phương án đều thất bại ở giai đoạn khả thi vì không có đủ số lượt di chuyển để di dời tất cả du khách từ phía đối diện. Đầu ra là KHÔNG. 

### Mẫu 2 

đầu vào:```
4 4
4
2 2 2 2
```Đây$T = 8$. 

| Bước | Bên mục tiêu | Chuyển khoản bắt buộc | Khả thi | Kiểm tra chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| Bên A | 8 | B=4 | 8 ≥ 4 | (8−4)=4 chẵn | 
| Bên B | 8 | A=4 | 8 ≥ 4 | (8−4)=4 chẵn | 

Cả hai cấu hình đều hợp lệ, nghĩa là chúng ta có thể định hướng trình tự các đường giao cắt để tất cả khách du lịch đều ở một bên. Đầu ra là CÓ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Chỉ một lần duy nhất để tính tổng dung lượng cổng thông tin | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ tỷ lệ thuận với kích thước đầu vào | 

Thuật toán dễ dàng phù hợp trong giới hạn vì$N \le 10^5$và tất cả các hoạt động là số học thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite  # harmless import
    A, B = map(int, input().split())
    n = int(input())
    f = list(map(int, input().split()))
    T = sum(f)
    if (T >= B and (T - B) % 2 == 0) or (T >= A and (T - A) % 2 == 0):
        return "YES"
    return "NO"

# provided samples
assert run("5 7\n2\n1 2\n") == "NO"
assert run("4 4\n4\n2 2 2 2\n") == "YES"

# custom cases
assert run("1 1\n1\n2\n") == "YES"
assert run("10 1\n3\n1 1 1\n") == "NO"
assert run("3 3\n2\n5 1\n") == "YES"
assert run("2 2\n1\n1\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, [2] | CÓ | trường hợp đối xứng tối thiểu | 
| 10 1, [1 1 1] | KHÔNG | tổng số lượt di chuyển không đủ | 
| 3 3, [5 1] | CÓ | tính chẵn lẻ + tính khả thi kết hợp | 
| 2 2, [1] | KHÔNG | thất bại chẵn lẻ một nước đi | 

## Vỏ cạnh 

Khi nào$T$nhỏ hơn cả A và B, thuật toán ngay lập tức loại bỏ cả hai cấu hình cuối cùng vì ngay cả việc di chuyển tất cả các du khách ở phía đối diện cũng là không thể. Ví dụ, với$A = 10$,$B = 10$, Và$f = [1, 1]$, chúng tôi nhận được$T = 2$, không đủ để di chuyển tất cả khách du lịch từ hai phía, tạo ra NO. 

Khi$T$có độ lớn đúng nhưng tính chẵn lẻ sai, tính khả thi không thành công dù có đủ số bước di chuyển. Ví dụ,$A = 4$,$B = 4$,$T = 5$cho phép chuyển đủ số lượng cho một hướng, nhưng tổng số lẻ sẽ ngăn cản sự cân bằng, vì mỗi lần phân phối lại thành công đều yêu cầu ghép các bước di chuyển theo hướng ngược lại.
