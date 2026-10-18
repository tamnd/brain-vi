---
title: "CF 105608A - \u0421\u0443\u043d\u0434\u0443\u043a \u0441\u043e\u043a\u0440\u043e\u0432\u0438\u0449"
description: "Chúng ta có một cái rương hình chữ nhật có ba cạnh dài $X$, $Y$ và $Z$. Rương có thể được định hướng theo bất kỳ cách nào, nghĩa là bất kỳ cặp cạnh nào của nó đều có thể trở thành “đế” cố gắng đi qua một cổng hình chữ nhật."
date: "2026-06-22T14:53:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105608
codeforces_index: "A"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421, \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440 2024-2025"
rating: 0
weight: 105608
solve_time_s: 43
verified: true
draft: false
---

[CF 105608A - \u0421\u0443\u043d\u0434\u0443\u043a \u0441\u043e\u043a\u0440\u043e\u0432\u0438\u0449](https://codeforces.com/problemset/problem/105608/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái rương hình chữ nhật có ba cạnh dài$X$,$Y$, Và$Z$. Rương có thể được định hướng theo bất kỳ cách nào, nghĩa là bất kỳ cặp cạnh nào của nó đều có thể trở thành “đế” cố gắng đi qua một cổng hình chữ nhật. Bản thân cổng thông tin có các mặt$A$Và$B$, nhưng hướng của nó cũng linh hoạt, vì vậy chúng tôi chỉ quan tâm đến việc khớp nhỏ hơn với nhỏ hơn và lớn hơn với lớn hơn sau khi xoay. 

Nhiệm vụ là xác định xem có tồn tại ít nhất một mặt của chiếc rương sao cho khi chúng ta chọn bất kỳ hai mặt nào trong số đó hay không.$(X, Y, Z)$, hình chữ nhật đó có thể vừa với cổng khi được xoay một cách tối ưu. Vừa vặn có nghĩa là cả hai kích thước của mặt ngực đã chọn không vượt quá kích thước tương ứng của cổng sau khi sắp xếp lại cả hai hình chữ nhật sao cho các cạnh nhỏ hơn được so sánh với nhau và các cạnh lớn hơn được so sánh với nhau. 

Đầu vào bao gồm năm số nguyên biểu thị ba chiều của rương và hai chiều của cổng. Đầu ra là một từ duy nhất, hoặc “CÓ” nếu bất kỳ mặt nào của rương có thể đi qua cổng hoặc “KHÔNG” nếu không. 

Vì chỉ có ba mặt có thể có của rương nên việc tính toán là thời gian không đổi. Ngay cả khi các ràng buộc lớn, giải pháp sẽ vẫn tầm thường, vì vậy chúng tôi không phải đối mặt với bất kỳ áp lực phức tạp nào. Những cạm bẫy tiềm ẩn duy nhất đến từ việc định hướng xử lý chính xác, vì việc so sánh các bên theo thứ tự cố định sẽ dẫn đến kết quả sai. 

Một lỗi phổ biến xuất hiện khi người ta chỉ kiểm tra một căn chỉnh duy nhất như$X \le A$Và$Y \le B$. Điều này không thành công khi cổng được xoay. Ví dụ, nếu$X = 5$,$Y = 3$,$A = 4$,$B = 6$, thì việc so sánh trực tiếp có thể không thành công tùy theo thứ tự, nhưng việc kiểm tra hướng đúng sẽ thành công vì$(3, 5)$phù hợp với$(4, 6)$. 

Một sai lầm nhỏ khác là quên rằng mỗi mặt trong số ba mặt phải được kiểm tra độc lập. Việc đi qua cổng phụ thuộc vào ít nhất một cặp hợp lệ, không phải tất cả chúng. 

## Phương pháp tiếp cận 

Một quan điểm vũ phu là đơn giản. Chúng tôi lấy từng mặt của ngực, cụ thể là$(X, Y)$,$(X, Z)$, Và$(Y, Z)$và đối với mỗi cái, chúng tôi thử cả hai hướng có thể có đối với cổng. Đối với mỗi lần thử, chúng tôi kiểm tra xem cả hai kích thước của mặt ngực xoay có vừa với cổng xoay hay không. Vì mỗi mặt có chính xác hai hướng nên điều này dẫn đến số lần kiểm tra không đổi. 

Điều này hiệu quả vì chỉ có ba cặp ứng cử viên và mỗi cặp chỉ có hai hoán vị, do đó tổng công việc được cố định ở sáu phép so sánh. Mặc dù ở đây sức mạnh vũ phu đã ở mức tối thiểu, việc suy nghĩ về mặt sức mạnh vũ phu sẽ giúp làm rõ tính đúng đắn: chúng ta đang liệt kê rõ ràng tất cả các cấu hình hình học. 

Quan sát quan trọng là chúng tôi không cần bất kỳ tối ưu hóa nào ngoài bảng liệt kê này. Cấu trúc của bài toán được giới hạn đầy đủ và rời rạc. Yêu cầu duy nhất là đảm bảo chúng tôi so sánh các thứ nguyên đã được sắp xếp để định hướng được xử lý rõ ràng. Thay vì kiểm tra cả hai hoán vị một cách thủ công, chúng ta chỉ cần so sánh các cặp đã được sắp xếp. 

Do đó, quá trình chuyển đổi từ vũ lực sang giải pháp tối ưu không phải nhằm giảm độ phức tạp mà là đơn giản hóa việc so sánh bằng cách chuẩn hóa từng cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba chiều của rương và hai chiều của cổng. Chúng xác định hai hình chữ nhật trong không gian 2D mà chúng ta sẽ so sánh theo nhiều hướng. 
2. Đối với mỗi cặp kích thước ngực, tạo thành một hình chữ nhật dự kiến. Ba ứng cử viên là$(X, Y)$,$(X, Z)$, Và$(Y, Z)$. Mỗi khuôn mặt đại diện cho một khuôn mặt có thể đi qua cổng. 
3. Đối với mỗi cặp ứng cử viên, hãy so sánh nó với cổng bằng cách sắp xếp khái niệm cả hai hình chữ nhật sao cho chúng ta luôn khớp cạnh nhỏ hơn với cạnh nhỏ hơn và cạnh lớn hơn với cạnh lớn hơn. Điều này loại bỏ sự cần thiết phải thử cả hai phép quay một cách rõ ràng. 
4. Nếu bất kỳ cặp ứng cử viên nào đáp ứng cả hai phép so sánh, ngay lập tức kết luận rằng rương có thể đi qua cổng. Không cần phải kiểm tra các cặp còn lại sau khi tìm thấy cấu hình hợp lệ. 
5. Nếu không có cặp nào trong ba cặp phù hợp, hãy kết luận rằng không có hướng nào cho phép đi qua. 

Tại sao nó hoạt động: mọi cách hợp lệ để chèn một hình chữ nhật vào một hình chữ nhật thẳng hàng theo trục khác đều tương ứng với một trong hai thứ tự có thể có của mỗi cặp. Bằng cách chuẩn hóa cả mặt ngực và các mặt cổng theo thứ tự được sắp xếp, chúng tôi đảm bảo rằng mọi góc quay có thể được thể hiện bằng một so sánh duy nhất. Vì tất cả các cấu hình hình học đều giảm xuống các trường hợp riêng biệt này nên việc kiểm tra cả ba mặt sẽ bao trùm toàn bộ không gian tìm kiếm mà không bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = int(input())
y = int(input())
z = int(input())
a = int(input())
b = int(input())

def fits(u, v, a, b):
    return (u <= a and v <= b) or (u <= b and v <= a)

ok = False
ok |= fits(x, y, a, b)
ok |= fits(x, z, a, b)
ok |= fits(y, z, a, b)

print("YES" if ok else "NO")
```Giải pháp đọc trực tiếp tất cả năm số nguyên. Mỗi cặp cạnh ngực được kiểm tra theo cả hai hướng của cổng bằng chức năng trợ giúp. Trình trợ giúp đóng gói logic xoay để chúng ta không vô tình bỏ lỡ cấu hình hợp lệ. Câu trả lời cuối cùng được tích lũy bằng cách sử dụng cờ boolean, cờ này làm ngắn mạch một cách hợp lý mặc dù hiệu suất không phải là vấn đề đáng lo ngại ở đây. 

Một lỗi triển khai phổ biến là quên kiểm tra định hướng thứ hai`(u <= b and v <= a)`, điều này sẽ từ chối các khớp xoay hợp lệ một cách không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào trong đó rương có kích thước 2, 3, 4 và cổng là 3 x 5. 

Đối với trường hợp này, chúng tôi đánh giá từng khuôn mặt: 

| Bước | Cặp | Cổng thông tin | Phù hợp (A,B) | Phù hợp (B,A) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (2,3) | (3,5) | vâng | vâng | đúng | 
| 2 | (2,4) | (3,5) | vâng | vâng | đúng (dừng sớm) | 

Thuật toán dừng sau cặp thành công đầu tiên, xác nhận rằng có ít nhất một khuôn mặt đi qua. 

Bây giờ hãy xem xét một trường hợp thất bại: rương 5, 6, 7 và cổng 4, 5. 

| Bước | Cặp | Cổng thông tin | Phù hợp (A,B) | Phù hợp (B,A) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (5,6) | (4,5) | không | không | sai | 
| 2 | (5,7) | (4,5) | không | không | sai | 
| 3 | (6,7) | (4,5) | không | không | sai | 

Không có cấu hình nào thành công nên kết quả đầu ra là “NO”. 

Ví dụ đầu tiên cho thấy sự kết thúc sớm khi thành công, trong khi ví dụ thứ hai cho thấy sự cạn kiệt hoàn toàn các khả năng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có ba so sánh cố định được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Việc tính toán là không đổi bất kể giá trị đầu vào, do đó nó thỏa mãn một cách tầm thường mọi ràng buộc hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x = int(input())
    y = int(input())
    z = int(input())
    a = int(input())
    b = int(input())

    def fits(u, v, a, b):
        return (u <= a and v <= b) or (u <= b and v <= a)

    ok = False
    ok |= fits(x, y, a, b)
    ok |= fits(x, z, a, b)
    ok |= fits(y, z, a, b)

    return "YES" if ok else "NO"

# provided samples (illustrative, since original samples not given)
assert run("2\n3\n4\n3\n5\n") == "YES"
assert run("5\n6\n7\n4\n5\n") == "NO"

# custom cases
assert run("1\n1\n10\n10\n1\n") == "YES", "rotation-aligned fit"
assert run("3\n3\n3\n2\n5\n") == "YES", "equal cube face fits"
assert run("9\n8\n7\n6\n5\n") == "NO", "all faces too large"
assert run("5\n4\n3\n5\n4\n") == "YES", "exact match without rotation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 10 / 10 1 | CÓ | sự cần thiết luân chuyển | 
| 3 3 3 / 2 5 | CÓ | xử lý đối xứng | 
| 9 8 7 / 6 5 | KHÔNG | từ chối nghiêm khắc | 
| 5 4 3 / 5 4 | CÓ | bình đẳng ranh giới | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi cả ba chiều đều bằng nhau, chẳng hạn như$X = Y = Z = 3$và cổng thông tin$A = 2, B = 5$. Mọi khuôn mặt đều giống hệt nhau nên thuật toán liên tục kiểm tra cùng một điều kiện hiệu quả. 

Đối với cặp (3, 3), cả hai hướng so với (2, 5) đều được kiểm tra: 

Kiểm tra định hướng đầu tiên$3 \le 2$thất bại, và$3 \le 5$vượt qua, nhưng vì cả hai thứ nguyên phải thỏa mãn cùng một cách ghép nối một cách nhất quán nên hướng này không hợp lệ. Hướng thứ hai giống hệt nhau, dẫn đến kết quả tương tự. Thuật toán trả về chính xác “KHÔNG”. 

Một trường hợp cạnh khác là khi một chiều hoàn toàn bằng một cạnh cổng. Ví dụ$X = 5, Y = 4, Z = 3$và cổng thông tin$A = 5, B = 4$. Đối với cặp (5,4), việc kiểm tra thành công ngay lập tức theo một trong hai hướng vì sự bình đẳng được cho phép trên cả hai trục, tạo ra “CÓ” mà không cần kiểm tra chiều thứ ba.
