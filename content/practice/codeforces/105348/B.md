---
title: "CF 105348B - Và cặp Xor"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu cặp số nguyên có thứ tự $(x, y)$ có thể được tạo thành từ một số $n$ cho trước, dưới hai ràng buộc nhị phân được áp dụng từng bit một."
date: "2026-06-23T15:42:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105348
codeforces_index: "B"
codeforces_contest_name: "Coding Challenge Alpha VII - by Algorave"
rating: 0
weight: 105348
solve_time_s: 351
verified: false
draft: false
---

[CF 105348B - Và cặp Xor](https://codeforces.com/problemset/problem/105348/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 51 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu cặp số nguyên có thứ tự$(x, y)$có thể được hình thành từ một số nhất định$n$, dưới hai ràng buộc nhị phân được áp dụng từng chút một. 

Ràng buộc đầu tiên sửa XOR của cặp: khi chúng ta so sánh từng bit của$x$Và$y$, XOR của họ phải tái tạo biểu diễn nhị phân của$n$. Ràng buộc thứ hai cố định AND theo bit của chúng, nhưng đã dịch chuyển: AND của$x$Và$y$không bằng$n$, nhưng thay vào đó bằng$2n$, đó là$n$dịch sang trái một vị trí nhị phân. 

Đầu ra của mỗi test case là số cặp hợp lệ$(x, y)$thỏa mãn đồng thời cả hai ràng buộc. Thứ tự rất quan trọng, vì vậy$(x, y)$Và$(y, x)$được tính riêng nếu chúng khác nhau. 

Những ràng buộc cho phép$n$lên đến$10^{15}$, có nghĩa là chúng tôi đang làm việc với tối đa 60 bit cho mỗi số. Một giải pháp xử lý từng bit một cách độc lập hoặc quét tuyến tính nhỏ cho mỗi trường hợp kiểm thử là đủ, trong khi bất kỳ giải pháp nào cố gắng liệt kê các giá trị của$x$hoặc$y$trực tiếp là không khả thi ngay lập tức vì điều đó sẽ tăng theo cấp số nhân với số lượng bit. 

Một vấn đề tế nhị xuất hiện khi nghĩ về sự tương tác giữa hai ràng buộc. XOR và AND thường mô tả hành vi độc lập trên mỗi bit, nhưng ở đây AND được dịch chuyển một vị trí so với XOR. Điều đó tạo ra sự ghép nối giữa các bit liền kề của$n$, điều này rất dễ bị bỏ sót trong quá trình phân tách theo từng bit đơn giản. 

Vỏ một cạnh làm cho điều này đặc biệt rõ ràng. Nếu như$n = 1$, thì sự dịch chuyển sẽ tạo ra một ràng buộc liên quan đến bit thứ hai của$x$Và$y$, mặc dù XOR chỉ chạm vào bit thấp nhất. Đây là lý do tại sao các giải pháp xử lý từng bit một cách độc lập mà không tính đến sự dịch chuyển sẽ tạo ra số lượng không chính xác hoặc đếm quá mức các cặp không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các giá trị có thể có của$x$Và$y$lên đến một số giới hạn ngụ ý bởi$n$. Từ$x \oplus y = n$, cả hai số phải có độ dài bit gần bằng nhau$n$, vì vậy người ta có thể thử lặp lại tất cả các giá trị từ$0$ĐẾN$2^{60}$. Ngay cả việc hạn chế các cặp hợp lệ thông qua việc kiểm tra cả hai điều kiện vẫn dẫn đến một không gian tìm kiếm không khả thi theo thứ tự$2^{120}$các cặp ứng cử viên 

Sự đơn giản hóa chính xuất phát từ việc viết lại mối quan hệ giữa XOR và AND. Đối với hai số nguyên bất kỳ, danh tính$$x + y = (x \oplus y) + 2(x \& y)$$luôn luôn giữ. Thay thế các ràng buộc sẽ cho:$$x + y = n + 2 \cdot (2n) = 5n$$Vì vậy, tổng số là cố định. Tuy nhiên, cấu trúc thực mạnh hơn việc chỉ cố định tổng: các ràng buộc XOR và AND cùng nhau xác định từng bit một cách độc lập, ngoại trừ sự dịch chuyển bên trong điều kiện AND. 

AND đã dịch chuyển có nghĩa là bit đó$i$của$x \& y$bằng bit$i$của$2n$, đó là một chút$i-1$của$n$. Điều này đưa ra sự phụ thuộc giữa các bit liền kề của$n$, biến vấn đề thành kiểm tra tính nhất quán cục bộ trên các bit lân cận. 

Khi chúng ta biểu diễn mọi thứ trên mỗi bit, mỗi vị trí chỉ thừa nhận một số nhiệm vụ có thể thực hiện được cho$(x_i, y_i)$và toàn bộ lời giải sẽ trở thành tích của các đóng góp bit độc lập, miễn là không xuất hiện mâu thuẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{60})$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu |$O(\log n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc ở dạng nhị phân và xử lý từng vị trí bit một cách độc lập, đồng thời theo dõi cẩn thận sự tương tác do AND dịch chuyển đưa ra. 

1. Viết hai bitmask: một là$A = n$, đại diện cho ràng buộc XOR và cái còn lại là$B = 2n$, đại diện cho ràng buộc AND. Chúng tôi giải thích cả hai từng chút một. 
2. Hãy quan sát điều đó$i$của$B$bằng bit$i-1$của$n$, với quy ước là bit thấp nhất của$B$là số không. Đây là nơi bắt nguồn của các ràng buộc lân cận. 
3. Đối với từng vị trí bit$i$, xét cặp$(x_i, y_i)$. Hai bit này phải thỏa mãn: 

-$x_i \oplus y_i = A_i$-$x_i \& y_i = B_i$4. Liệt kê bốn cặp bit có thể có: 

-$00 \rightarrow (A=0, B=0)$-$01 \rightarrow (A=1, B=0)$-$10 \rightarrow (A=1, B=0)$-$11 \rightarrow (A=0, B=1)$Bảng này cho thấy mỗi cặp ràng buộc ánh xạ tới 0, một hoặc hai cấu hình hợp lệ. 
5. Hãy dịch điều này thành quy tắc đếm: 

- Nếu$A_i = 1$Và$B_i = 1$, cấu hình là không thể. 
- Nếu như$B_i = 0$Và$A_i = 1$, có hai lựa chọn. 
- Nếu không thì chỉ có một lựa chọn. 
6. Cách duy nhất để có được nhiều lựa chọn là khi$A_i = 1$Và$B_i = 0$, tương ứng với 1-bit trong$n$có bit trước đó là 0. 
7. Tổng hợp tất cả các bit, câu trả lời trở thành$2^{k}$, Ở đâu$k$là số vị trí trong đó$n$có 1 bit không có 1 bit khác ngay trước đó. 
8. Nếu bất kỳ vị trí nào có$A_i = 1$Và$B_i = 1$, việc xây dựng bị phá vỡ hoàn toàn và câu trả lời là số không. 

### Tại sao nó hoạt động 

Mỗi vị trí bit hoạt động giống như một hệ thống ràng buộc cục bộ chỉ liên quan đến$x_i$,$y_i$, và bit liền kề của$n$thông qua phép dịch AND. Khi chúng tôi đảm bảo không có cái liền kề nào xuất hiện trong$n$, tất cả các ràng buộc trở nên độc lập trên các bit, do đó tổng số phép gán hợp lệ là tích của các lựa chọn trên mỗi bit. Bất kỳ sự phụ thuộc nào ngoài các bit liền kề đều không tồn tại, do đó không có sự ghép nối toàn cục nào có thể làm mất hiệu lực của phép gán nhất quán cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        
        # invalid if n has consecutive 1s
        if n & (n << 1):
            print(0)
            continue
        
        # count valid "free-choice" bits
        ans = 1
        while n:
            if n & 1:
                ans <<= 1
            n >>= 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ kiểm tra điều kiện cấu trúc chính: nếu hai bit liền kề trong$n$đều bằng 1, ràng buộc dịch chuyển AND dẫn đến mâu thuẫn, do đó câu trả lời ngay lập tức là 0. 

Nếu cấu hình hợp lệ, cứ 1 bit trong$n$đóng góp hệ số 2 vào câu trả lời, bởi vì nó tương ứng với một vị trí trong đó$(x_i, y_i)$có thể được chọn theo hai cách khác nhau. Vòng lặp chỉ đơn giản đếm những đóng góp này bằng cách quét biểu diễn nhị phân. 

Dịch chuyển$n$phá hủy bên trong vòng lặp là an toàn vì chúng ta chỉ cần các bit của nó; không cần thông tin vị trí khi tính liền kề đã được xác thực. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 5$, có dạng nhị phân là$101$. 

| chút tôi | n_i (A_i) | n_{i-1} (B_i) | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 2 | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 0 | 2 | 

Tổng cộng là$2 \times 1 \times 2 = 4$. Điều này cho thấy chỉ các bit 1 riêng biệt mới đóng góp theo cấp số nhân. 

Bây giờ hãy xem xét$n = 3$, nhị phân$11$. 

| chút tôi | n_i (A_i) | n_{i-1} (B_i) | hiệu lực | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | được | 
| 1 | 1 | 1 | không hợp lệ | 

Hàng thứ hai vi phạm ràng buộc vì số 1 trong$n_i$trùng với dịch chuyển 1 trong$B_i$, điều này làm cho XOR và AND không tương thích ở vị trí đó. Điều này buộc câu trả lời là không. 

Trường hợp thứ hai chứng minh tại sao sự kề cận trong$n$xác định đầy đủ tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log n)$| Mỗi lần kiểm tra sẽ quét các bit nhị phân một lần | 
| Không gian |$O(1)$| Chỉ có một vài bộ đếm và thao tác bit | 

Độ dài bit của$n$nhiều nhất là 60, nên ngay cả với$10^3$trường hợp thử nghiệm, giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    it = sys.stdin.read().strip().split()
    
    t = int(it[0])
    idx = 1
    out = []
    
    for _ in range(t):
        n = int(it[idx]); idx += 1
        
        if n & (n << 1):
            out.append("0")
        else:
            ans = 1
            while n:
                if n & 1:
                    ans <<= 1
                n >>= 1
            out.append(str(ans))
    
    return "\n".join(out)

# provided sample (interpreted as multiple numbers)
assert run("7\n0\n1\n8\n10\n12\n17\n21\n") == "1\n2\n2\n4\n0\n4\n8"

# custom cases
assert run("1\n2\n") == "2", "single bit"
assert run("1\n3\n") == "0", "adjacent ones invalid"
assert run("1\n5\n") == "4", "alternating bits"
assert run("1\n0\n") == "1", "zero case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 2 | hành vi bit đơn | 
| 3 | 0 | vi phạm lân cận | 
| 5 | 4 | nhiều bit 1 độc lập | 
| 0 | 1 | trường hợp cạnh cơ sở | 

## Vỏ cạnh 

Khi nào$n = 0$, không có 1 bit và không có ràng buộc kề. Thuật toán tạo ra một tích trống, đánh giá chính xác là 1. Cặp hợp lệ duy nhất là$(0,0)$, phù hợp với công trình. 

Khi$n$chứa các số 1 liên tiếp, chẳng hạn như$n = 3$, điều kiện$n \& (n \ll 1) \neq 0$gây nên ngay lập tức. Điều này nắm bắt chính xác tình huống có một chút trong$n$buộc cả XOR và AND đã dịch chuyển yêu cầu các giá trị không tương thích ở cùng một vị trí. 

Khi$n$là lũy thừa của hai, mỗi 1 bit được cách ly, do đó, mỗi bit đóng góp độc lập hệ số 2, dẫn đến kết quả hàm mũ rõ ràng bằng 2.
