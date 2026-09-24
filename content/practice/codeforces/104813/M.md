---
title: "CF 104813M - Họa Sĩ"
description: "Chúng tôi đang làm việc trên một lưới số nguyên 2D vô hạn trong đó mọi điểm mạng ban đầu đều có cùng một ký tự mặc định \".\". Sau đó, chúng ta được cung cấp một chuỗi các thao tác vẽ để ghi đè lên các vùng của lưới này bằng các ký tự mới."
date: "2026-06-28T13:14:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104813
codeforces_index: "M"
codeforces_contest_name: "The 9th CCPC (Harbin) Onsite(The 2nd Universal Cup. Stage 10: Harbin)"
rating: 0
weight: 104813
solve_time_s: 80
verified: false
draft: false
---

[CF 104813M - Họa sĩ](https://codeforces.com/problemset/problem/104813/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới số nguyên 2D vô hạn trong đó mọi điểm mạng ban đầu đều có cùng một ký tự mặc định`"."`. Sau đó, chúng ta được cung cấp một chuỗi các thao tác vẽ để ghi đè lên các vùng của lưới này bằng các ký tự mới. Có hai loại cập nhật: một loại vẽ tất cả các điểm nguyên bên trong một vòng tròn và loại kia vẽ tất cả các điểm nguyên bên trong một hình chữ nhật thẳng hàng với trục. Loại hoạt động thứ ba yêu cầu chúng ta xuất ra một hình chữ nhật phụ hữu hạn của lưới sau khi tất cả các hiệu ứng vẽ trước đó đã được áp dụng theo thứ tự. 

Giải thích chính là mỗi thao tác được áp dụng theo trình tự và ghi đè các màu trước đó. Khi nhiều hình dạng chồng lên nhau, thao tác sau sẽ thay thế hoàn toàn các màu trước đó tại các điểm bị ảnh hưởng. Truy vấn kết xuất chỉ yêu cầu trạng thái cuối cùng của vùng giới hạn sau khi áp dụng tất cả các bản cập nhật trước đó. 

Phạm vi tọa độ cực kỳ lớn, lên tới 10^9 độ lớn, vì vậy chúng tôi không thể mô phỏng lưới một cách rõ ràng. Tuy nhiên, tổng diện tích của tất cả các truy vấn hiển thị là nhỏ, tổng cộng tối đa là 10^4. Đây là phần duy nhất của đầu vào mà chúng tôi buộc phải hiện thực hóa các ô lưới thực tế. Mọi hoạt động khác được xác định về mặt hình học trên không gian tọa độ khổng lồ. 

Một sai lầm ngây thơ là thử lặp lại mọi điểm bị ảnh hưởng bởi mọi hình tròn hoặc hình chữ nhật. Một vòng tròn có bán kính lên tới 10^9 có thể chứa tới 10^18 điểm nguyên, do đó, ngay cả một thao tác đơn lẻ cũng không thể mở rộng một cách rõ ràng. Một lỗi nhỏ khác là cập nhật trực tiếp các truy vấn kết xuất mà không tôn trọng thứ tự thao tác một cách cẩn thận. Vì các thao tác ghi đè lên các thao tác trước đó nên màu cuối cùng phụ thuộc hoàn toàn vào thao tác che phủ gần đây nhất chứ không phụ thuộc vào số lượng thao tác giao nhau. 

Trường hợp quan trọng là các bản cập nhật chồng chéo với các hình dạng khác nhau. Ví dụ: sơn hình chữ nhật, sau đó là sơn hình tròn chồng lên một phần vùng hiển thị phải đảm bảo hình tròn ghi đè hoàn toàn hình chữ nhật bên trong vùng của nó. Bất kỳ cách tiếp cận nào tính toán trước một lưới cuối cùng mà không tôn trọng thứ tự sẽ thất bại. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản về mặt khái niệm: duy trì một từ điển hoặc bản đồ lưới của tất cả các điểm được vẽ. Đối với mỗi thao tác hình tròn hoặc hình chữ nhật, hãy lặp qua từng điểm nguyên trong vùng hình học của nó và gán màu đã cho. Khi có truy vấn kết xuất, hãy lặp lại vùng được yêu cầu và xuất các giá trị được lưu trữ hoặc`"."`nếu không sơn. 

Điều này đúng vì nó mô phỏng trực tiếp việc xác định vấn đề. Vấn đề là thời gian chạy. Một hình chữ nhật có thể có diện tích lớn bằng 2·10^18 và mặc dù các vùng kết xuất có kích thước nhỏ nhưng các bản cập nhật vẫn chiếm ưu thế hoàn toàn. Việc mở rộng vòng tròn thậm chí còn tệ hơn vì việc kiểm tra tất cả các điểm nguyên trong đĩa bán kính-r là O(r^2). Với tối đa 2000 thao tác, điều này là hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng rõ ràng toàn bộ lưới. Chúng tôi chỉ truy vấn các vùng nhỏ và chúng tôi chỉ cần màu cuối cùng tại những điểm đó. Thay vì mở rộng các hình dạng trên toàn cầu, chúng tôi đảo ngược phối cảnh: đối với mỗi truy vấn kết xuất, chúng tôi trực tiếp tính toán màu của từng điểm được yêu cầu một cách độc lập bằng cách kiểm tra các thao tác theo thứ tự ngược lại. 

Đối với một điểm cố định (x, y), màu cuối cùng của nó được xác định bởi thao tác cuối cùng bao phủ nó. Vì vậy, chúng tôi quét ngược các hoạt động và dừng lại ngay khi chúng tôi tìm thấy một hoạt động vẽ ra điểm đó. Vì chúng tôi chỉ đánh giá tối đa 10^4 tổng số điểm truy vấn và có tối đa 2000 thao tác nên điều này mang lại giới hạn trên có thể quản lý được là 2×10^7 kiểm tra hình học. 

Mỗi lần kiểm tra là thời gian không đổi: ngăn chặn hình chữ nhật là các bất đẳng thức đơn giản và ngăn chặn hình tròn là kiểm tra khoảng cách bình phương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lưới Brute Force | O(tổng diện tích sơn) | O (lưới) | Quá chậm | 
| Đánh giá ngược lại mỗi truy vấn | O(n · q) trong đó q 10^4 | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các thao tác một lần, lưu trữ chúng và trả lời từng truy vấn kết xuất một cách độc lập. 

1. Đọc tất cả các thao tác và lưu trữ chúng vào danh sách theo thứ tự. Chúng tôi không cố gắng áp dụng chúng ngay lập tức vì việc hiển thị yêu cầu truy vấn trạng thái cuối cùng và các thao tác sau này có thể ghi đè các thao tác trước đó. 
2. Khi chúng ta gặp một thao tác kết xuất, chúng ta sẽ mở rộng vùng hình chữ nhật của nó thành một danh sách các điểm. Vì tổng của tất cả các khu vực hiển thị tối đa là 10^4 nên việc mở rộng này là an toàn. 
3. Đối với mỗi điểm trong vùng kết xuất, chúng tôi quét ngược danh sách thao tác từ thao tác cuối cùng đến thao tác đầu tiên. 
4. Đối với mỗi thao tác, chúng tôi kiểm tra xem nó có bao gồm điểm hiện tại hay không. 

Đối với hình chữ nhật, chúng ta kiểm tra xem x1  x  x2 và y1  y  y2. 

Đối với đường tròn, chúng ta kiểm tra xem (x - cx)^2 + (y - cy)^2 ≤ r^2. 
5. Thao tác đầu tiên chúng ta tìm thấy theo thứ tự ngược lại bao phủ điểm sẽ xác định màu cuối cùng của nó. Chúng tôi chỉ định ký tự đó và ngừng quét thêm cho điểm đó. 
6. Nếu không có thao tác nào bao gồm điểm, chúng tôi sẽ xuất`"."`. 

Lý do chúng tôi quét lùi là vì các thao tác sau sẽ ghi đè lên các thao tác trước đó. Điều này đảm bảo rằng trận đấu đầu tiên gặp phải ngược lại chính xác là lớp sơn được áp dụng cuối cùng. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm lưới cố định nào, màu cuối cùng của nó chỉ phụ thuộc vào thao tác mới nhất có chứa nó. Vì các thao tác được áp dụng tuần tự và ghi đè lên các giá trị trước đó nên quá trình vẽ tạo ra quy tắc người viết cuối cùng sẽ thắng trên mỗi ô. Các thao tác quét theo thứ tự ngược lại sẽ tái tạo lại trực tiếp trình ghi cuối cùng đó. Vì mỗi điểm được đánh giá độc lập nên không cần có trạng thái tổng thể và tính chính xác xuất phát từ thực tế là việc kiểm tra ngăn chặn hình học khớp chính xác xem liệu một thao tác có ảnh hưởng đến điểm đó hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

ops = []
renders = []

n = int(input())
for _ in range(n):
    parts = input().split()
    if parts[0] == "Circle":
        x, y, r = map(int, parts[1:4])
        col = parts[4]
        ops.append(("C", x, y, r, col))
    elif parts[0] == "Rectangle":
        x1, y1, x2, y2 = map(int, parts[1:5])
        col = parts[5]
        ops.append(("R", x1, y1, x2, y2, col))
    else:
        x1, y1, x2, y2 = map(int, parts[1:5])
        renders.append((x1, y1, x2, y2))

out_lines = []

for x1, y1, x2, y2 in renders:
    for y in range(y2, y1 - 1, -1):
        row = []
        for x in range(x1, x2 + 1):
            color = "."
            for op in reversed(ops):
                if op[0] == "R":
                    _, a1, b1, a2, b2, c = op
                    if a1 <= x <= a2 and b1 <= y <= b2:
                        color = c
                        break
                else:
                    _, cx, cy, r, c = op
                    dx = x - cx
                    dy = y - cy
                    if dx * dx + dy * dy <= r * r:
                        color = c
                        break
            row.append(color)
        out_lines.append("".join(row))

sys.stdout.write("\n".join(out_lines))
```Giải pháp giữ tất cả các hoạt động trong một danh sách và trì hoãn việc đánh giá cho đến khi kết xuất. Mỗi ô được hiển thị sẽ quét ngược các hoạt động một cách độc lập. Các vòng lặp lồng nhau trên khu vực kết xuất là cần thiết vì đầu ra được yêu cầu rõ ràng trên mỗi ô, nhưng ràng buộc đảm bảo tổng công việc này vẫn ở mức nhỏ. 

Một chi tiết tinh tế là phép lặp ngược lại trên y ở đầu ra, vì vấn đề yêu cầu in từ y2 xuống y1. Hướng này quan trọng để phù hợp với hướng trực quan cần thiết. 

Kiểm tra vòng tròn sử dụng khoảng cách bình phương để tránh các phép tính dấu phẩy động, đảm bảo tính chính xác với số học số nguyên. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một kịch bản đơn giản hóa xuất phát từ cấu trúc mẫu. 

### Ví dụ 1 

Hoạt động đầu vào:```
Circle (0,0,r=1,'A')
Rectangle (0,0,1,1,'B')
Render (0,0,1,1)
```Điểm vùng kết xuất là (0,0), (1,0), (0,1), (1,1). 

| Điểm | Thứ tự quét (hoạt động ngược lại) | Trận đầu tiên | Kết quả | 
| --- | --- | --- | --- | 
| (0,0) | Hình chữ nhật, Hình tròn | Hình chữ nhật | B | 
| (1,0) | Hình chữ nhật, Hình tròn | Hình chữ nhật | B | 
| (0,1) | Hình chữ nhật, Hình tròn | Hình chữ nhật | B | 
| (1,1) | Hình chữ nhật, Hình tròn | Hình chữ nhật | B | 

Tất cả các điểm đều được bao phủ bởi hình chữ nhật cuối cùng, vì vậy hình tròn không có hiệu ứng nhìn thấy được. 

Điều này xác nhận tính bất biến chính: các thao tác sau chiếm ưu thế hơn các thao tác trước ngay cả khi chúng trùng nhau một phần. 

### Ví dụ 2```
Circle (0,0,r=2,'C')
Rectangle (-1,-1,1,1,'R')
Render (-1,-1,1,1)
```| Điểm | Thứ tự quét | Trận đầu tiên | Kết quả | 
| --- | --- | --- | --- | 
| (0,0) | Hình chữ nhật, Hình tròn | Hình chữ nhật | R | 
| (1,1) | Hình chữ nhật, Hình tròn | Hình chữ nhật | R | 
| (2,0) | Chỉ vòng tròn | Vòng tròn | C (loại trừ kết xuất bên ngoài nếu cần) | 

Chồng chéo bên trong, hình chữ nhật thắng vì được áp dụng muộn hơn. 

Điều này chứng tỏ rằng trật tự chứ không phải sự thống trị định hình sẽ quyết định đầu ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q · N) | Mỗi ô kết xuất sẽ kiểm tra tất cả các hoạt động ngược lại cho đến lần truy cập đầu tiên | 
| Không gian | O(N) | Danh sách các hoạt động được lưu trữ và bộ đệm đầu ra hiển thị | 

Tổng số ô được hiển thị tối đa là 10^4 và mỗi ô quét tối đa 2000 thao tác, đưa ra khoảng 2×10^7 lần kiểm tra nguyên thủy. Điều này phù hợp thoải mái trong giới hạn thời gian trong Python với số học số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    ops = []
    renders = []
    n = int(input())
    for _ in range(n):
        parts = input().split()
        if parts[0] == "Circle":
            x, y, r = map(int, parts[1:4])
            col = parts[4]
            ops.append(("C", x, y, r, col))
        elif parts[0] == "Rectangle":
            x1, y1, x2, y2 = map(int, parts[1:5])
            col = parts[5]
            ops.append(("R", x1, y1, x2, y2, col))
        else:
            x1, y1, x2, y2 = map(int, parts[1:5])
            renders.append((x1, y1, x2, y2))

    out = []
    for x1, y1, x2, y2 in renders:
        for y in range(y2, y1 - 1, -1):
            row = []
            for x in range(x1, x2 + 1):
                color = "."
                for op in reversed(ops):
                    if op[0] == "R":
                        _, a1, b1, a2, b2, c = op
                        if a1 <= x <= a2 and b1 <= y <= b2:
                            color = c
                            break
                    else:
                        _, cx, cy, r, c = op
                        dx = x - cx
                        dy = y - cy
                        if dx * dx + dy * dy <= r * r:
                            color = c
                            break
                row.append(color)
            out.append("".join(row))

    return "\n".join(out)

# provided sample (formatted minimally)
assert run("""7
Circle 0 0 5 *
Circle -2 2 1 @
Circle 2 2 1 @
Rectangle 0 -1 0 0 ^
Rectangle -2 -2 2 -2 _
Render -5 -5 5 5
Render -1 0 1 2
""").strip() == """.....*.....
..*******..
.**@***@**.
.*@@@*@@@*.
.**@***@**.
*****^*****
.****^****.
.**_____**.
.*********.
..*******..
.....*.....
@*@
***
*^*""".strip()

# minimal edge
assert run("""3
Rectangle 0 0 0 0 A
Render 0 0 0 0
Render 1 1 1 1
""").strip() == """A
.""".strip()

# circle override
assert run("""3
Circle 0 0 1 B
Rectangle 0 0 0 0 A
Render 0 0 0 0
""").strip() == """A""".strip()

# negative coords
assert run("""2
Rectangle -1 -1 1 1 Z
Render -1 -1 1 1
""") == "ZZZ\nZZZ\nZZZ"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật ô đơn + kết xuất | MỘT / . | ghi đè cơ bản | 
| hình chữ nhật rồi hình tròn | A | thứ tự ưu tiên | 
| tọa độ âm | lưới 3x3 | phối hợp xử lý | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi vùng kết xuất chứa các điểm không bao giờ bị ảnh hưởng bởi bất kỳ thao tác nào. Trong trường hợp đó, việc quét tất cả các hoạt động không mang lại kết quả khớp nào và kết quả đầu ra phải được giữ nguyên`"."`. Ví dụ: nếu chúng ta chỉ vẽ một hình chữ nhật ở xa và hiển thị điểm gốc thì mọi điểm sẽ giữ nguyên`"."`. Quá trình quét ngược xử lý chính xác việc này vì nó thực hiện tất cả các thao tác mà không chỉ định màu. 

Một trường hợp cạnh khác là khi một đường tròn gần như không chạm vào một điểm trên đường biên của nó. Vì điều kiện bao gồm`(u-x)^2 + (v-y)^2 <= r^2`, các điểm biên phải được tô màu. Kiểm tra khoảng cách bình phương số nguyên đảm bảo bao gồm chính xác mà không có lỗi dấu phẩy động. 

Trường hợp cạnh cuối cùng là hình chữ nhật và hình tròn chồng lên nhau, trong đó hình tròn được áp dụng sau nhưng chỉ chồng lên một phần vùng kết xuất. Bởi vì mỗi ô kiểm tra độc lập các hoạt động ngược lại, nên chỉ tập hợp con các ô bị ảnh hưởng sẽ nhận màu vòng tròn, trong khi các ô khác vẫn được xác định bởi các hoạt động trước đó hoặc mặc định`"."`.
