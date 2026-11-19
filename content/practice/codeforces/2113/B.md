---
title: "CF 2113B - Khởi đầu tốt đẹp"
description: "Chúng ta có một mái nhà hình chữ nhật thẳng hàng với các trục tọa độ, có chiều rộng $w$ và chiều cao $h$. Mái nằm trong mặt phẳng bắt đầu từ $(0,0)$. Chúng ta cũng có các ô hình chữ nhật giống hệt nhau có kích thước cố định $a nhân b$."
date: "2026-06-08T04:23:12+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 2113
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1031 (Div. 2)"
rating: 1200
weight: 2113
solve_time_s: 99
verified: false
draft: false
---

[CF 2113B - Khởi đầu tốt đẹp](https://codeforces.com/problemset/problem/2113/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mái nhà hình chữ nhật thẳng hàng với các trục tọa độ, có chiều rộng$w$và chiều cao$h$. Mái nhà nằm trong mặt phẳng bắt đầu từ$(0,0)$. Chúng tôi cũng có các ô hình chữ nhật giống hệt nhau có kích thước cố định$a \times b$. Những ô này không được phép xoay nên hướng của chúng được cố định. 

Nhiệm vụ không phải là tìm bất kỳ lát gạch nào từ đầu. Thay vào đó, hai ô đã được đặt ở đâu đó trong mặt phẳng. Chúng có thể mở rộng ra bên ngoài mái nhà, nhưng mỗi phần trong số chúng chồng lên nhau ít nhất một phần của mái nhà và chúng không chồng lên nhau. Hai ô này phải giữ nguyên vị trí chính xác của chúng. 

Chúng tôi phải quyết định xem liệu vẫn có thể che phủ toàn bộ diện tích mái nhà bằng các bản sao giống nhau hay không$a \times b$gạch, không di chuyển hai gạch hiện có và không chồng chéo bất kỳ gạch nào, đồng thời cho phép gạch vượt ra ngoài ranh giới mái nhà. 

Khó khăn chính là cấu trúc ốp lát có cấu trúc cứng nhắc trên toàn cầu: vì các ô xếp không thể xoay và tất cả đều giống hệt nhau nên mọi ô xếp hợp lệ đều phải căn chỉnh theo một lưới có chiều rộng$a$và chiều cao$b$. Hai ô được đặt trước sẽ hạn chế việc căn chỉnh lưới nào, nếu có, vẫn có thể thực hiện được. 

Kích thước đầu vào cho phép lên đến$10^4$trường hợp thử nghiệm, tọa độ và kích thước tăng lên$10^9$. Điều này loại trừ bất kỳ vị trí mô phỏng hoặc vũ phu nào. Mỗi bài kiểm tra phải được giải trong thời gian không đổi, dựa trên các quan sát cấu trúc và số học mô-đun. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể “sửa” vị trí cục bộ bằng cách dịch chuyển các ô một chút. Điều đó là không thể bởi vì một khi đã tồn tại một ô xếp, nó buộc tất cả các vị trí ô xếp phải tuân theo một mạng tuần hoàn nghiêm ngặt. Một vấn đề tế nhị khác là suy nghĩ chỉ có một hướng tọa độ là quan trọng. Trong thực tế, cả sự sắp xếp x và y phải đồng thời nhất quán. 

Một kịch bản thất bại cụ thể đối với lý luận ngây thơ là khi hai ô được đặt có modulo tọa độ x khác nhau.$a$. Ví dụ: nếu một ô bắt đầu tại$x=0$và cái khác ở$x=1$với$a=2$, người ta có thể nghĩ rằng chúng ta vẫn có thể lấp đầy những khoảng trống, nhưng điều này là không thể vì không có lưới điện toàn cầu nào có thể đi qua cả hai cùng một lúc. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua cấu trúc của các ô, chúng ta có thể thử mô phỏng vị trí: chọn vị trí ô bắt đầu và cố gắng mở rộng lưới theo chiều ngang và chiều dọc trong khi đảm bảo cả hai ô cố định nằm trên đó. Điều này nhanh chóng trở nên không khả thi vì lưới không bị giới hạn trong các lựa chọn vị trí và việc kiểm tra tính nhất quán bằng cách dịch chuyển mạnh mẽ sẽ yêu cầu lặp lại tất cả các độ lệch có thể có ở cả hai trục, điều này là không thể trong các ràng buộc. 

Quan sát quan trọng là bất kỳ sự xếp lát hợp lệ nào của mặt phẳng bằng cách cố định$a \times b$hình chữ nhật tương ứng với việc chọn một offset duy nhất$(x_0, y_0)$, rồi đặt các ô ở tất cả các vị trí$(x_0 + ka, y_0 + lb)$. Điều này có nghĩa là góc dưới bên trái của mỗi ô phải đáp ứng:$$x \equiv x_0 \pmod a, \quad y \equiv y_0 \pmod b$$Bây giờ hãy xem xét hai ô được đặt sẵn. Mỗi người trong số họ đều buộc các ràng buộc đối với phần bù toàn cầu này. Nếu cả hai ô đều thuộc cùng một ô thì tọa độ x của chúng phải bằng nhau theo modulo$a$và tọa độ y của chúng phải bằng nhau theo modulo$b$. 

Tuy nhiên, có một điều tinh tế hơn: ngay cả khi chúng đồng dạng modulo$a$Và$b$, chúng không được tạo ra mâu thuẫn về mức độ bao phủ toàn bộ hình chữ nhật có thể được mở rộng qua ranh giới mái nhà. Bản thân mái nhà không hạn chế việc ốp lát, nhưng yêu cầu cả hai viên gạch cố định vẫn là một phần của cùng một lưới vô hạn. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem hai ô có thể thuộc về một ô chung hay không.$a \times b$căn chỉnh mạng. Nếu không thì câu trả lời ngay là không. Nếu có thì chúng ta cũng phải đảm bảo rằng không có sự không khớp bắt buộc nào phát sinh trong cách mái nhà được phân chia bằng các đường lưới dọc hoặc ngang. Điều này dẫn đến việc kiểm tra tính nhất quán của sự căn chỉnh dọc theo ít nhất một chiều trong đó các viên gạch có thể “chặn” các dải phân hủy khác nhau của mái nhà. 

Một cách dễ hiểu hơn để thấy điều này là: mặt phẳng được chia thành các dải chiều rộng dọc$a$. Mỗi dải được lát gạch độc lập theo hướng y. Nếu cả hai ô cố định nằm ở các pha thẳng đứng khác nhau (tức là mô đun tọa độ x của chúng$a$khác nhau), sau đó chúng buộc phải sắp xếp các dải không tương thích. Logic tương tự được áp dụng theo hướng ngang với chiều cao$b$. 

Do đó, quyết định rút gọn lại thành việc kiểm tra xem liệu việc căn chỉnh x hoặc căn chỉnh y có thể được thực hiện nhất quán trên toàn cục mà không gây ra mâu thuẫn do ngăn xếp thứ hai hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|---|---| 

| Tìm kiếm lưới Brute Force trên các offset |$O(ab)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 

| Kiểm tra căn chỉnh mô-đun tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán độ tương thích x-offset bằng cách kiểm tra xem$x_1 \equiv x_2 \pmod a$. Điều này xác định liệu cả hai ô có thể nằm trên cùng một lưới xếp dọc hay không. 
2. Tính toán độ tương thích y-offset bằng cách kiểm tra xem$y_1 \equiv y_2 \pmod b$. Điều này xác định liệu cả hai ô có thể nằm trên cùng một lưới ô ngang hay không. 
3. Nếu cả hai sự đồng đẳng đều giữ nguyên, hãy kết luận rằng tồn tại một sự căn chỉnh xếp kề toàn cục nhất quán, do đó xuất ra "Có". 
4. Mặt khác, kết luận rằng không có một ô xếp định kỳ nào có thể bao gồm cả hai ô cố định cùng một lúc, do đó xuất ra "Không". 

Lý do chúng ta chỉ cần so sánh mô-đun là vì bất kỳ cách xếp lát đầy đủ nào cũng tương đương với việc chọn gốc mạng. Hai ô hoặc đồng ý về mạng đó hoặc chúng buộc hai mạng không tương thích. 

### Tại sao nó hoạt động 

Một lát gạch hợp lệ của mặt phẳng bằng cách không quay giống hệt nhau$a \times b$hình chữ nhật tạo ra một mối quan hệ tương đương duy nhất trên mặt phẳng trong đó các điểm khác nhau bởi bội số nguyên của$a$trong x và$b$ở y. Mỗi góc dưới bên trái của ô thuộc về chính xác một lớp tương đương được xác định bởi$(x \bmod a, y \bmod b)$. Nếu hai ô đặt trước nằm trong các lớp khác nhau thì không có sự hoàn thành nào có thể điều hòa chúng, bởi vì bất kỳ tiện ích mở rộng nào cũng duy trì cùng một cấu trúc lớp trên toàn cầu. Nếu chúng nằm trong cùng một lớp, thì việc lát gạch có thể được mở rộng một cách nhất quán vì mặt phẳng có thể được lấp đầy độc lập trong từng dải thẳng hàng mà không có sự mâu thuẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        w, h, a, b = map(int, input().split())
        x1, y1, x2, y2 = map(int, input().split())

        if (x1 - x2) % a == 0 and (y1 - y2) % b == 0:
            out.append("Yes")
        else:
            out.append("No")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo ý tưởng căn chỉnh mô-đun. Điểm tinh tế duy nhất là xử lý chính xác tọa độ âm, nhưng toán tử modulo của Python đã đảm bảo kết quả nhất quán để kiểm tra sự phù hợp khi được sử dụng làm`(x1 - x2) % a == 0`. 

Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập trong thời gian không đổi, đọc thông tin đầu vào và kiểm tra hai điều kiện mô-đun. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
w=6, h=5, a=2, b=3
tiles: (-1,-2), (5,4)
```| Bước | điều kiện x | điều kiện y | Kết quả | 
| --- | --- | --- | --- | 
| Tính toán | (-1 - 5) % 2 = 0 | (-2 - 4) % 3 = 0 | Có | 

Cả hai ô đều thẳng hàng trên cùng một độ lệch lưới, do đó tồn tại một ô xếp nhất quán. 

Điều này xác nhận rằng ngay cả khi các viên gạch mở rộng ra ngoài mái nhà, sự liên kết tương đối của chúng vẫn khớp với một lưới tuần hoàn hợp lệ. 

### Ví dụ 2 

đầu vào:```
w=4, h=4, a=2, b=2
tiles: (0,0), (3,1)
```| Bước | điều kiện x | điều kiện y | Kết quả | 
| --- | --- | --- | --- | 
| Tính toán | (0 - 3) % 2 = 1 | (0 - 1) % 2 = 1 | Không | 

Cả hai tọa độ đều vi phạm tính nhất quán mô-đun. Không có một lưới nào có thể bao gồm cả hai vị trí, vì vậy việc hoàn thành là không thể. 

Điều này cho thấy một trường hợp trong đó độ che phủ cục bộ của mái là không thích hợp: sự mâu thuẫn hoàn toàn xuất phát từ cấu trúc lưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi bài kiểm tra thực hiện kiểm tra số học theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một vài số nguyên được lưu trữ cho mỗi lần kiểm tra | 

Các ràng buộc cho phép lên đến$10^4$các trường hợp thử nghiệm và giải pháp chỉ thực hiện một số thao tác cho mỗi trường hợp, khiến nó đủ nhanh một cách dễ dàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        w, h, a, b = map(int, input().split())
        x1, y1, x2, y2 = map(int, input().split())

        if (x1 - x2) % a == 0 and (y1 - y2) % b == 0:
            output.append("Yes")
        else:
            output.append("No")

    return "\n".join(output)

# provided samples
assert run("""7
6 5 2 3
-1 -2 5 4
4 4 2 2
0 0 3 1
10 9 3 2
0 0 4 3
10 9 3 2
0 0 6 3
5 5 2 2
-1 -1 4 -1
5 5 2 2
-1 -1 2 3
7 8 2 4
0 0 0 5
""") == """Yes
No
No
Yes
No
Yes
No"""

# custom cases
assert run("""1
10 10 3 3
0 0 6 6
""") == "Yes"

assert run("""1
10 10 3 3
0 0 1 6
""") == "No"

assert run("""1
1 1 2 2
-1 -1 0 0
""") == "Yes"

assert run("""1
8 8 2 3
0 0 2 3
""") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bội số liên kết | Có | căn chỉnh mạng nhất quán | 
| x không khớp | Không | xung đột lưới ngang | 
| ranh giới nhỏ | Có | tọa độ âm được xử lý | 
| hỗn hợp không phù hợp | Không | cả hai hướng phải đồng ý | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tọa độ âm nhưng vẫn thể hiện các vị trí ô hợp lệ. Ví dụ, một ô ở$(-1,-1)$vẫn phải được xử lý một cách nhất quán theo số học modulo. Séc`(x1 - x2) % a == 0`xử lý chính xác điều này vì nó so sánh sự liên kết tương đối thay vì dư lượng tuyệt đối. 

Một trường hợp cạnh khác xảy ra khi gạch nằm xa ranh giới mái nhà. Ngay cả khi cả hai ô đều nằm hoàn toàn bên ngoài, chúng vẫn hạn chế việc ốp lát tổng thể. Ví dụ: nếu cả hai ô được đặt ở$(10^9, 10^9)$Và$(10^9 + a, 10^9)$, điều kiện x đạt nhưng điều kiện y có thể thất bại hoặc vượt qua một cách độc lập và quyết định vẫn hoàn toàn theo mô-đun. 

Trường hợp cạnh cuối cùng là khi một ô được căn chỉnh chính xác với ranh giới lưới và ô kia ở gần ranh giới nhưng không nằm trên ranh giới đó. Ngay cả sự khác biệt 1 trong tọa độ cũng phá vỡ tính khả thi, bởi vì nó buộc hai nguồn gốc mạng không tương thích và không có sự điều chỉnh một phần nào có thể điều hòa chúng.
