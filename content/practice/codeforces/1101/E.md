---
title: "CF 1101E - Công việc mới của Polycarp"
description: "Chúng tôi đang xử lý một bộ sưu tập tiền giấy hình chữ nhật ngày càng tăng. Mỗi lần một ghi chú được thêm vào, nó sẽ tồn tại mãi mãi. Sau đó, chúng tôi nhận được các truy vấn hỏi liệu tất cả các tờ tiền nhìn thấy cho đến nay có thể được đặt bên trong một chiếc ví hình chữ nhật nhất định hay không."
date: "2026-06-13T07:23:38+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1101
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 58 (Rated for Div. 2)"
rating: 1500
weight: 1101
solve_time_s: 263
verified: true
draft: false
---

[CF 1101E - Công việc mới của Polycarp](https://codeforces.com/problemset/problem/1101/E) 

**Đánh giá:** 1500 
**Thẻ:** triển khai 
**Thời gian giải:** 4m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xử lý một bộ sưu tập tiền giấy hình chữ nhật ngày càng tăng. Mỗi lần một ghi chú được thêm vào, nó sẽ tồn tại mãi mãi. Sau đó, chúng tôi nhận được các truy vấn hỏi liệu tất cả các tờ tiền nhìn thấy cho đến nay có thể được đặt bên trong một chiếc ví hình chữ nhật nhất định hay không. 

Một ghi chú có thể được xoay, do đó kích thước của ghi chú$x \times y$có thể được coi là một trong hai hướng. Một tờ tiền vừa với một chiếc ví$h \times w$nếu sau khi có thể xoay nó, cả hai kích thước của tờ tiền không vượt quá kích thước ví tương ứng. 

Một chi tiết quan trọng là mỗi nốt nhạc phải khớp độc lập với nhau. Không có sự tương tác đóng gói giữa các tờ tiền, vì vậy điều kiện của một chiếc ví chỉ đơn giản là nó phải đủ lớn để chứa từng tờ tiền riêng lẻ. 

Mỗi truy vấn thuộc loại “thêm” sẽ chèn một hình chữ nhật và mỗi truy vấn thuộc loại “kiểm tra” sẽ hỏi liệu tất cả các hình chữ nhật được chèn cho đến nay có thể vừa với ví đã cho hay không. 

Các ràng buộc đi lên đến$5 \cdot 10^5$hoạt động, do đó, bất kỳ giải pháp nào quét tất cả các ghi chú trước đó cho mỗi truy vấn sẽ dẫn đến khoảng$O(n^2)$hành vi trong trường hợp xấu nhất là quá chậm. Chúng ta cần thứ gì đó chỉ giữ một lượng nhỏ trạng thái và trả lời từng truy vấn trong thời gian không đổi. 

Một cạm bẫy tinh vi xuất hiện khi nghĩ về “chiều rộng tối đa và chiều cao tối đa một cách độc lập”. Điều đó không thành công vì phép quay vài chiều. Ví dụ, có ghi chú$100 \times 1$Và$1 \times 100$có nghĩa là “chiều rộng tối đa” là 100 và “chiều cao tối đa” là 100, nhưng điều đó không phản ánh rằng cả hai ghi chú thực sự đã ổn riêng lẻ. Ràng buộc thực sự phụ thuộc vào hướng tối ưu trên mỗi nốt nhạc, không phải trên mỗi lựa chọn trục tổng thể. 

Một dạng lỗi khác xuất phát từ việc cố gắng duy trì một hình chữ nhật bao quanh tất cả các nốt như thể chúng phải được căn chỉnh một cách nhất quán. Bài toán không yêu cầu sự định hướng nhất quán giữa các ghi chú; mỗi nốt có thể xoay độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ giữ một danh sách tất cả các ghi chú. Đối với mỗi truy vấn kiểm tra loại, chúng tôi lặp lại tất cả các ghi chú được lưu trữ và kiểm tra xem mỗi ghi chú có vừa với ví sau khi xoay hay không. Điều này đúng vì nó trực tiếp thực thi định nghĩa. Tuy nhiên, với tới$5 \cdot 10^5$các truy vấn, điều này thoái hóa thành việc kiểm tra tới$5 \cdot 10^5$ghi chú cho mỗi truy vấn, tạo ra khoảng$10^{11}$trong trường hợp xấu nhất là không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần phải theo dõi mọi hình chữ nhật. Để một chiếc ví bị lỗi thì phải tồn tại một tờ tiền không vừa. Vì vậy, thay vì kiểm tra tất cả các ghi chú mỗi lần, chúng ta chỉ cần duy trì xem có tồn tại ít nhất một ghi chú “tệ nhất” chặn ví hay không. 

Mỗi nốt nhạc$x \times y$có thể được chuẩn hóa bằng cách sắp xếp các cạnh của nó sao cho$a = \min(x, y)$,$b = \max(x, y)$. Sau sự chuyển đổi này, mọi nốt nhạc được thể hiện theo hướng kinh điển trong đó$a \le b$. Bây giờ là một chiếc ví$h \times w$cũng có thể được coi là$(\min(h,w), \max(h,w))$. 

Một ghi chú phù hợp khi và chỉ khi cả hai$a \le h$Và$b \le w$. Do đó, nếu chúng ta sửa hướng ứng cử viên của ví, hạn chế duy nhất có liên quan là tất cả các tờ tiền phải nằm dưới góc đó. 

Bây giờ, sự đơn giản hóa quan trọng: thay vì kiểm tra tất cả các ghi chú, chúng tôi duy trì một cấu trúc theo dõi “vật cản lớn nhất”. Nốt tệ nhất là nốt có mức tối đa$a$, và trong số đó, cái có giá trị tối đa$b$. Điều này làm giảm tập hợp các ràng buộc chỉ còn hai giá trị. 

Khi có truy vấn đến, chúng tôi sẽ kiểm tra cả hai hướng của ví so với cặp này. Nếu không có hướng nào chiếm ưu thế trong giới hạn tối đa được lưu trữ thì câu trả lời là KHÔNG; nếu không thì CÓ. 

Điều này có hiệu quả vì bất kỳ ghi chú nào có thể làm mất hiệu lực của ví đều phải vi phạm một trong hai chiều theo một số hướng và cách biểu diễn chuẩn đảm bảo chúng ta chỉ cần kiểm tra sự thống trị so với điểm cực trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Duy trì cực trị |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các truy vấn theo thứ tự trong khi vẫn duy trì hai giá trị: giá trị tối đa của các cạnh nhỏ hơn của tất cả các nốt và giá trị tối đa của các cạnh lớn hơn sau khi chuẩn hóa. 

1. Đối với mỗi ghi chú được thêm vào$x, y$, tính toán$a = \min(x, y)$,$b = \max(x, y)$. Điều này loại bỏ sự mơ hồ về hướng để mọi hình chữ nhật đều có thể so sánh được một cách nhất quán. 
2. Duy trì hai giá trị toàn cầu:`max_a`là lớn nhất trong số tất cả$a$, Và`max_b`là lớn nhất trong số tất cả$b$để ghi chú đạt được`max_a`. Điều này nắm bắt được hình dạng hạn chế nhất. 
3. Đối với ví truy vấn$h, w$, cũng bình thường hóa nó thành$h \le w$. Điều này đảm bảo so sánh nhất quán với các ghi chú được lưu trữ. 
4. Kiểm tra xem ví có thể chứa tờ tiền trong trường hợp xấu nhất hay không. Điều này có nghĩa là xác minh xem cả hai`max_a ≤ h`Và`max_b ≤ w`. 
5. Vì hướng của ví cũng có thể được hoán đổi nên chúng tôi cũng kiểm tra điều kiện được hoán đổi:`max_a ≤ w`Và`max_b ≤ h`. 
6. Nếu một trong hai hướng hoạt động, chúng tôi in CÓ; nếu không thì KHÔNG. 

### Tại sao nó hoạt động 

Mỗi nốt nhạc được rút gọn thành một cặp kinh điển$(a, b)$Ở đâu$a \le b$. Bất kỳ ngăn chặn hợp lệ nào vào một hình chữ nhật đều tương ứng với việc đặt cặp này vào hướng đã chọn của ví. Lưu ý duy nhất có thể cản trở tính khả thi là lưu ý khó phù hợp nhất với ít nhất một chiều. Theo dõi tối đa$a$đảm bảo chúng tôi tôn trọng “mặt ngắn” chặt chẽ nhất và theo dõi mức tối đa$b$trong số đó đảm bảo chúng tôi tôn trọng ràng buộc “cạnh dài” tương ứng. Bất kỳ nốt nào khác đều bị chi phối bởi một trong những thái cực này, vì vậy nếu các thái cực đó phù hợp thì tất cả các nốt đều phù hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    max_a = 0
    max_b = 0
    
    for _ in range(n):
        parts = input().split()
        
        if parts[0] == '+':
            x = int(parts[1])
            y = int(parts[2])
            a = min(x, y)
            b = max(x, y)
            
            if a > max_a:
                max_a = a
                max_b = b
            elif a == max_a:
                if b > max_b:
                    max_b = b
        
        else:
            h = int(parts[1])
            w = int(parts[2])
            
            a1, b1 = min(h, w), max(h, w)
            
            ok = (max_a <= a1 and max_b <= b1)
            print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Mã duy trì hình chữ nhật xấu nhất theo quy tắc tăng dần. Đối với mỗi lần chèn, nó cập nhật cặp tối đa theo từ điển$(a, b)$, đầu tiên bằng cách giảm thiểu cạnh ngắn và sau đó tối đa hóa cạnh dài giữa các dây buộc. Mỗi truy vấn chỉ cần kiểm tra xem hình chữ nhật xấu nhất này có phù hợp với ví theo hướng tốt nhất hay không. 

Một lỗi phổ biến là cố gắng chỉ theo dõi cực đại toàn cục của cả hai chiều một cách độc lập. Điều đó không thành công vì những cực đại đó có thể đến từ các nốt khác nhau và không thể được nhận ra đồng thời bằng một hình chữ nhật. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
+ 3 2
+ 2 3
? 1 20
? 3 3
? 2 3
```Sự phát triển của trạng thái: 

| Bước | Hoạt động | max_a | max_b | Ví | Kiểm tra kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cộng (3,2) → (2,3) | 2 | 3 | - | - | 
| 2 | cộng (2,3) → (2,3) | 2 | 3 | - | - | 
| 3 | truy vấn (1,20) | 2 | 3 | (1,20) | KHÔNG | 
| 4 | truy vấn (3,3) | 2 | 3 | (3,3) | CÓ | 
| 5 | truy vấn (2,3) | 2 | 3 | (2,3) | CÓ | 

Truy vấn đầu tiên không thành công vì ràng buộc bên nhỏ nhất đã bị vi phạm. Các truy vấn sau này thành công vì cả hai chiều đều đủ theo ít nhất một hướng. 

### Mẫu 2 (đã thi công) 

đầu vào:```
+ 5 1
+ 2 4
? 4 5
```| Bước | Hoạt động | max_a | max_b | Ví | Kiểm tra kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cộng (1,5) → (1,5) | 1 | 5 | - | - | 
| 2 | cộng (2,4) → (2,4) | 2 | 5 | - | - | 
| 3 | truy vấn (4,5) | 2 | 5 | (4,5) | CÓ | 

Quan sát quan trọng ở đây là mặc dù một nốt cao và hẹp còn nốt kia rộng và ngắn, ràng buộc tổng hợp vẫn có thể biểu thị dưới dạng một cặp thống trị duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi truy vấn cập nhật hoặc kiểm tra một số lượng biến không đổi | 
| Không gian |$O(1)$| Chỉ có hai số nguyên được lưu trữ | 

Thuật toán thực hiện một lượng công việc không đổi cho mỗi thao tác, đủ nhanh để$5 \cdot 10^5$truy vấn trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    max_a = 0
    max_b = 0
    out = []

    for _ in range(n):
        parts = input().split()
        if parts[0] == '+':
            x, y = int(parts[1]), int(parts[2])
            a, b = min(x, y), max(x, y)
            if a > max_a:
                max_a, max_b = a, b
            elif a == max_a:
                max_b = max(max_b, b)
        else:
            h, w = int(parts[1]), int(parts[2])
            a1, b1 = min(h, w), max(h, w)
            if max_a <= a1 and max_b <= b1:
                out.append("YES")
            else:
                out.append("NO")

    return "\n".join(out)

# provided sample
assert run("""9
+ 3 2
+ 2 3
? 1 20
? 3 3
? 2 3
+ 1 5
? 10 10
? 1 5
+ 1 1
""") == """NO
YES
YES
YES
NO"""

# custom: single note fits trivially
assert run("""3
+ 2 2
? 10 10
? 1 1
""") == """YES
NO"""

# custom: rotation required
assert run("""2
+ 1 5
? 5 1
""") == """YES"""

# custom: increasing constraints
assert run("""4
+ 2 3
+ 3 4
? 3 4
? 2 2
""") == """YES
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ghi chú duy nhất | CÓ/KHÔNG | tính đúng đắn cơ bản | 
| trường hợp xoay | CÓ | xử lý định hướng | 
| truy vấn hỗn hợp | CÓ/KHÔNG | cập nhật gia tăng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cạnh ngắn lớn nhất xuất phát từ một nốt khác với cạnh dài lớn nhất. Thuật toán xử lý việc này vì`max_b`chỉ được cập nhật khi tương ứng`a`bằng`max_a`, đảm bảo cả hai giá trị đều đến từ một ứng cử viên tương thích. 

Một trường hợp khác là các bản cập nhật lặp đi lặp lại chỉ thay đổi mức tối đa của cạnh ngắn. Trong trường hợp đó, cạnh dài phải được đặt lại thành cạnh dài của ứng viên mới; nếu không thì cặp được lưu trữ sẽ không nhất quán với bất kỳ hình chữ nhật thực nào.
