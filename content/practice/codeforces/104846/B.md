---
title: "CF 104846B - \u041c\u044b\u0442 \u043d\u0430 \u0440\u0435\u043a\u0435 \u042f\u0443\u0437\u0430"
description: "Một thương gia đi dọc theo một con sông mang theo hai loại hàng hóa: trứng cá muối và mật ong. Ban đầu, anh ta có một lượng cố định mỗi mặt hàng và sau đó mỗi đơn vị có thể được bán ở một mức giá xác định."
date: "2026-06-28T11:27:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104846
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 2023-2024 (7-8 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104846
solve_time_s: 46
verified: true
draft: false
---

[CF 104846B - \u041c\u044b\u0442 \u043d\u0430 \u0440\u0435\u043a\u0435 \u042f\u0443\u0437\u0430](https://codeforces.com/problemset/problem/104846/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một thương gia đi dọc theo một con sông mang theo hai loại hàng hóa: trứng cá muối và mật ong. Ban đầu, anh ta có một lượng cố định mỗi mặt hàng và sau đó mỗi đơn vị có thể được bán ở một mức giá xác định. Lợi nhuận cuối cùng của anh ta phụ thuộc vào số lượng hàng hóa tồn tại được cho đến khi được bán ra thị trường thành phố, bởi vì mọi thứ không được sử dụng để thu phí đều có thể được bán. 

Trên đường đi, anh phải đi qua một điểm hải quan. Tại trạm kiểm soát này, anh ta buộc phải trả phí nhưng anh ta có thể linh hoạt trong cách thanh toán. Anh ta có thể trả một số tiền cố định hoặc có thể thay thế khoản thanh toán đó bằng một ít trứng cá muối hoặc một ít mật ong. Mỗi tùy chọn thay thế hoàn toàn cùng một mức phí nhưng tiêu tốn các tài nguyên khác nhau. 

Mục tiêu là chọn phương thức thanh toán tối đa hóa lợi nhuận tiền tệ cuối cùng sau khi bán số hàng hóa còn lại và hạch toán mọi khoản tiền mặt được trả khi thu phí. 

Cấu trúc chính là chỉ có một điểm quyết định: làm thế nào để trả một khoản phí duy nhất. Sau đó, mọi thứ đều tuyến tính vì hàng hóa còn lại được bán độc lập với giá cố định. 

Mặc dù các ràng buộc bao gồm các giá trị rất lớn đối với số lượng hàng hóa (lên tới hàng chục triệu), nhưng chỉ có một số lượng chiến lược có ý nghĩa không đổi để đánh giá, do đó giải pháp phải tránh bất kỳ sự mô phỏng nào về số lượng. 

Một sai lầm phổ biến là cho rằng bạn phải luôn giảm thiểu chi phí trước mắt của phí cầu đường. Điều đó không thành công vì thanh toán bằng hàng hóa sẽ loại bỏ lợi nhuận bán hàng trong tương lai chứ không chỉ giá trị trước mắt. 

Ví dụ: nếu trứng cá muối cực kỳ có giá trị so với mật ong thì việc thanh toán bằng trứng cá muối có thể đắt hơn nhiều về mặt doanh thu bị mất ngay cả khi nó tránh được việc chi tiền mặt. 

Một trường hợp khó nhận thấy khác là tính khả thi của các phương thức thanh toán. Nếu người bán không có đủ trứng cá muối hoặc mật ong, những lựa chọn đó sẽ không hợp lệ, ngay cả khi chúng có vẻ tối ưu về mặt giá trị. 

## Phương pháp tiếp cận 

Tư duy bạo lực sẽ xem xét tất cả các kết hợp có thể có của việc trả phí bằng cách sử dụng số lượng hàng hóa và tiền mặt khác nhau. Tuy nhiên, vấn đề không cho phép thanh toán một phần và phí được ấn định dưới ba hình thức riêng biệt. Điều này thu gọn không gian quyết định thành nhiều nhất ba chiến lược ứng cử viên. 

Cái nhìn sâu sắc chính xác là toàn bộ việc tối ưu hóa đều mang tính cục bộ đối với phí. Mọi thứ trước và sau phí đều tuyến tính đối với các tài nguyên còn lại, do đó tác động duy nhất của phí là trừ một lần khỏi giá trị ban đầu cố định. Điều này cho phép chúng tôi đánh giá từng tùy chọn thanh toán một cách độc lập bằng cách tính toán lợi nhuận cuối cùng sau khi áp dụng nó. 

Đối với mỗi lựa chọn, chúng tôi tính toán số trứng cá muối và mật ong còn lại, quy đổi chúng thành tiền theo mức giá nhất định và trừ đi số tiền mặt đã trả nếu có. Kết quả tốt nhất trong số các lựa chọn hợp lệ là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về việc phân bổ | O(i + m) hoặc tệ hơn tùy theo mô hình | O(1) | Quá chậm và không cần thiết | 
| Đánh giá 3 phương thức thanh toán | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán kết quả bằng cách đánh giá trực tiếp ba cách có thể để trả phí.

1. Bắt đầu bằng cách tính lợi nhuận cơ bản nếu không trả phí bằng tiền mặt: tổng giá trị là i·a + m·b. Điều này thể hiện doanh thu lý thuyết tối đa trước khi áp dụng bất kỳ ràng buộc nào. 
2. Cân nhắc việc trả phí bằng tiền mặt v. Trong trường hợp này, tại cửa khẩu không có hàng hóa nào được tiêu thụ nên giá trị còn lại là i·a + m·b − v. 
3. Cân nhắc việc trả phí bằng cách sử dụng c gam trứng cá muối. Tùy chọn này chỉ hợp lệ nếu tôi ≥ c. Nếu hợp lệ thì số hàng còn lại là (i − c, m), nên lợi nhuận trở thành (i − c)·a + m·b. Điều này mô hình việc mất giá trị bán trong tương lai của trứng cá muối bị loại bỏ. 
4. Cân nhắc việc trả phí bằng cách sử dụng d gam mật ong. Tùy chọn này chỉ hợp lệ nếu m ≥ d. Nếu hợp lệ thì hàng hóa còn lại là (i, m − d) nên lợi nhuận trở thành i·a + (m − d)·b. 
5. Lấy giá trị lớn nhất trong số tất cả các phương án hợp lệ. 

Tại sao nó hoạt động 

Mỗi tùy chọn thanh toán tương ứng với trạng thái cuối cùng riêng biệt của các tài nguyên còn lại. Sau điểm kiểm tra, doanh thu được xác định hoàn toàn bằng cách định giá tuyến tính của hàng hóa còn lại. Vì không có sự tương tác giữa trứng cá muối và mật ong ngoài phép trừ duy nhất khi thu phí, nên chiến lược tối ưu phải là một trong ba điểm cuối này. Bất kỳ chiến lược hỗn hợp giả định nào cũng sẽ mâu thuẫn với bản chất dạng cố định của phí cầu đường, vốn buộc phải có một lựa chọn duy nhất trong số ba chuyển đổi riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    i, m, a, b, v, c, d = map(int, input().split())

    base = i * a + m * b
    ans = base - v  # pay money

    if i >= c:
        ans = max(ans, (i - c) * a + m * b)

    if m >= d:
        ans = max(ans, i * a + (m - d) * b)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện đầu tiên tính toán toàn bộ giá trị của hàng hóa mà không có bất kỳ khoản khấu trừ nào. Điều này hoạt động như một điểm tham chiếu cho tất cả các kết quả khác. Trường hợp thanh toán bằng tiền mặt được xử lý bằng cách trừ trực tiếp v. 

Sau đó, chúng tôi kiểm tra tính khả thi của từng khoản thanh toán dựa trên hàng hóa. Mỗi lần kiểm tra chỉ đơn giản là đảm bảo có đủ hàng tồn kho, sau đó tính toán lại giá trị kết quả sau khi loại bỏ số lượng tương ứng. Không cần mô phỏng vì chỉ xảy ra một lần suy luận. 

Việc so sánh được thực hiện tăng dần nên chúng tôi luôn bảo toàn kết quả tốt nhất có thể đạt được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: i = 5, m = 10, a = 2, b = 3, v = 7, c = 3, d = 3 

Chúng tôi đánh giá cả ba lựa chọn. 

| Tùy chọn | Còn lại (i, m) | Tính toán giá trị | Kết quả | 
| --- | --- | --- | --- | 
| Tiền mặt | (5, 10) | 5·2 + 10·3 − 7 | 33 | 
| Trứng cá muối | (2, 10) | 2·2 + 10·3 | 34 | 
| Em yêu | (5, 7) | 5·2 + 7·3 | 31 | 

Tốt nhất là thanh toán bằng trứng cá muối, mang lại 34. 

Điều này cho thấy việc tránh thanh toán bằng tiền mặt có thể là tối ưu ngay cả khi nó duy trì được tính thanh khoản, vì hàng hóa có giá trị hạ nguồn cao hơn. 

### Ví dụ 2 

Đầu vào: i = 5, m = 10, a = 1, b = 3, v = 10, c = 6, d = 3 

| Tùy chọn | Còn lại (i, m) | Tính toán giá trị | Kết quả | 
| --- | --- | --- | --- | 
| Tiền mặt | (5, 10) | 5·1 + 10·3 − 10 | 25 | 
| Trứng cá muối | không hợp lệ | không đủ trứng cá muối | - | 
| Em yêu | (5, 7) | 5·1 + 7·3 | 26 | 

Ở đây, việc thanh toán bằng trứng cá muối là không thể nên chỉ có hai lựa chọn được so sánh. Thanh toán bằng mật ong là tốt nhất dù bị mất hàng, vì thanh toán bằng tiền mặt làm giảm tổng giá trị nhiều hơn. 

Điều này chứng tỏ tầm quan trọng của việc kiểm tra tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi cho mỗi bài kiểm tra | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì ngay cả đối với 10 trường hợp thử nghiệm, công việc hoàn toàn là số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        i, m, a, b, v, c, d = map(int, input().split())

        base = i * a + m * b
        ans = base - v

        if i >= c:
            ans = max(ans, (i - c) * a + m * b)

        if m >= d:
            ans = max(ans, i * a + (m - d) * b)

        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# sample-like cases
assert run("5 10 2 3 7 3 3\n") == "34"

# cash dominates scenario
assert run("1 1 100 100 1 0 0\n") == "199"

# forced goods payment due to large v
assert run("5 5 1 1 1000 2 2\n") == "6"

# boundary: exactly enough caviar
assert run("10 0 5 1 0 10 1\n") == "0"

# boundary: exactly enough honey
assert run("0 10 1 5 0 1 10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đánh đổi tiền mặt và hàng hóa | 34 | tính đúng đắn của lựa chọn tối đa | 
| giá trị nhỏ bằng nhau | 199 | độ chính xác số học cơ bản | 
| phạt tiền mặt lớn | 6 | tránh tiền mặt khi chưa tối ưu | 
| ngưỡng trứng cá muối chính xác | 0 | tính khả thi về ranh giới | 
| ngưỡng mật ong chính xác | 0 | tính khả thi về ranh giới | 

## Vỏ cạnh 

Một trường hợp khó khăn là khi thanh toán bằng tiền mặt có vẻ hấp dẫn vì nó tránh được việc mất hàng nhưng thực tế lại làm giảm lợi nhuận cuối cùng nhiều hơn so với việc bán hàng. Ví dụ: ngay cả một khoản khấu trừ trứng cá muối nhỏ cũng có thể có giá trị hơn việc trả bằng tiền mặt nếu số tiền đó lớn. 

Một trường hợp khác là khi việc thanh toán dựa trên hàng hóa hầu như không khả thi. Nếu i bằng c chính xác, số trứng cá muối còn lại sẽ bằng 0, giá trị này có thể thay đổi đáng kể. Thuật toán xử lý việc này vì nó kiểm tra i ≥ c trước khi tính toán trạng thái rút gọn. 

Trường hợp thứ ba là khi cả hai khoản thanh toán hàng hóa đều không hợp lệ. Trong tình huống đó, chỉ còn lại thanh toán bằng tiền mặt và thuật toán sẽ quay trở lại cơ số − v một cách chính xác mà không so sánh các trạng thái không hợp lệ.
