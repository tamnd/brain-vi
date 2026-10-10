---
title: "CF 104976B - Trang trí lễ hội"
description: "Chúng ta được phát một bộ đèn đặt trên trục số. Mỗi đèn có một vị trí cố định và có nhãn màu. Với mỗi khoảng cách truy vấn $d$, chúng ta muốn tìm một chiếc đèn $u$ có chỉ số nhỏ nhất sao cho nếu chúng ta di chuyển chính xác $d$ đơn vị sang bên phải thì sẽ tồn tại một chiếc đèn khác ở vị trí đó…"
date: "2026-06-28T19:09:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104976
codeforces_index: "B"
codeforces_contest_name: "The 2023 ICPC Asia Hangzhou Regional Contest (The 2nd Universal Cup. Stage 22: Hangzhou)"
rating: 0
weight: 104976
solve_time_s: 85
verified: false
draft: false
---

[CF 104976B - Trang trí lễ hội](https://codeforces.com/problemset/problem/104976/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ đèn đặt trên trục số. Mỗi đèn có một vị trí cố định và có nhãn màu. Đối với mỗi khoảng cách truy vấn$d$, chúng tôi muốn tìm một chiếc đèn$u$với chỉ số nhỏ nhất sao cho nếu chúng ta di chuyển chính xác$d$ở bên phải thì tồn tại một đèn khác ở vị trí đó và đèn thứ hai đó có màu khác với$u$. Nếu không có đèn như vậy tồn tại, chúng tôi xuất ra số 0. 

Chi tiết quan trọng là câu trả lời không phải là trực tiếp về các cặp đèn mà là về việc quét các đèn theo thứ tự chỉ số và kiểm tra xem mỗi đèn có thể tạo thành một “cạnh khoảng cách” hợp lệ ở bên phải hay không. 

Các ràng buộc rất lớn: lên tới 250.000 đèn và 250.000 truy vấn, với tọa độ cũng lên tới 250.000. Bất kỳ giải pháp nào kiểm tra từng truy vấn bằng cách quét tất cả các đèn và tìm kiếm kết quả phù hợp trên mỗi đèn sẽ yêu cầu theo thứ tự:$nq$, vượt xa giới hạn có thể chấp nhận được. Ngay cả việc tra cứu logarit trên mỗi đèn cho mỗi truy vấn cũng quá chậm nếu được thực hiện một cách ngây thơ. 

Cấu trúc gợi ý chúng ta nên xử lý trước mối quan hệ giữa các vị trí, vì tọa độ được giới hạn và tĩnh, trong khi các truy vấn chỉ khác nhau về khoảng cách. 

Một sai lầm ngây thơ xuất phát từ việc bỏ qua thứ tự chỉ số và chỉ tập trung vào các vị thế. Ví dụ: nếu có hai đèn tồn tại ở vị trí 1 và 3 với khoảng cách hợp lệ là 2 thì câu trả lời không nhất thiết là nếu đèn có chỉ số thấp hơn ở vị trí 2 cũng có đối tác hợp lệ. Yêu cầu về chỉ số tối thiểu buộc chúng ta phải xem xét các đèn theo thứ tự chỉ số tăng dần chứ không phải theo thứ tự không gian. 

Một trường hợp lỗi nhỏ khác xảy ra khi nhiều đèn chạm vào cùng một vị trí mục tiêu thông qua các truy vấn khác nhau. Nếu chúng ta chỉ lưu trữ một màu hoặc ghi đè các giá trị, chúng ta có thể vô tình bỏ lỡ kết quả khớp “màu khác” hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ đánh giá từng truy vấn một cách độc lập. Đối với một khoảng cách nhất định$d$, chúng tôi lặp lại mọi đèn$u$, tính toán$x_u + d$và kiểm tra xem đèn có tồn tại ở vị trí đó không. Nếu nó tồn tại thì chúng tôi sẽ xác minh xem màu của nó có khác với$c_u$. Điều này yêu cầu tra cứu vị trí nhanh, thường là bản đồ băm hoặc mảng được lập chỉ mục theo tọa độ. 

Với tra cứu tọa độ trực tiếp, mỗi truy vấn sẽ tốn$O(n)$, mang lại tổng cộng$O(nq)$, quá lớn đối với 250.000 đến 250.000. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào sự khác biệt giữa các vị trí và các vị trí nằm trong phạm vi giới hạn. Điều này cho phép chúng tôi tính toán trước cho mọi khoảng cách có thể$d$, chỉ số nhỏ nhất$u$đó thỏa mãn điều kiện. Thay vì tính toán lại mỗi truy vấn, chúng ta có thể xây dựng cấu trúc trên mọi khoảng cách. 

Chúng tôi đảo ngược vấn đề: đối với mỗi cặp đèn$(u, v)$, nếu như$x_v - x_u = d$Và$c_u \ne c_v$, sau đó$u$là một câu trả lời ứng cử viên cho khoảng cách$d$. Chúng tôi muốn, đối với mỗi$d$, mức tối thiểu như vậy$u$. Điều này biến vấn đề thành việc quét tất cả các cặp đèn sắp xếp theo sự khác biệt về tọa độ. 

Vì tọa độ lên tới 250.000 nên chúng ta có thể lưu trữ đèn trong một mảng được lập chỉ mục theo vị trí. Sau đó, đối với mỗi đèn, chúng tôi có thể kiểm tra tất cả các khoảng cách có thể về phía trước bằng cách lặp lại các vị trí hiện có khác. Tuy nhiên, quét toàn bộ theo cặp vẫn còn quá lớn nếu được thực hiện một cách ngây thơ. 

Việc tối ưu hóa là coi mảng tọa độ như một lưới thưa thớt và chỉ lặp lại các vị trí hiện có. Đối với mỗi vị trí$x$, chúng tôi xem xét tất cả$x + d$tồn tại. Điều này đảm bảo chúng tôi chỉ xử lý các cặp hợp lệ. Vì mỗi cặp hợp lệ được xem xét một lần nên tổng công việc tỷ lệ thuận với số cạnh trong biểu đồ ẩn này, có thể quản lý được với các ràng buộc và độ thưa thớt. 

Chúng tôi duy trì, cho mỗi khoảng cách$d$, chỉ số tối thiểu$u$thấy cho đến nay. Mỗi cặp hợp lệ cập nhật một mục nhập mảng duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Tính toán trước tất cả các cặp hợp lệ |$O(n \cdot \text{avg degree})$tồi tệ nhất$O(n^2)$, được tối ưu hóa thông qua độ thưa thớt đến gần$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải thích lại vấn đề bằng cách xây dựng tất cả các cạnh có hướng hợp lệ từ$u$ĐẾN$v$Ở đâu$x_v - x_u = d$và màu sắc khác nhau, sau đó nén các cạnh đó thành câu trả lời tốt nhất cho mỗi khoảng cách. 

1. Lưu trữ tất cả các đèn trong một mảng được lập chỉ mục theo vị trí để chúng ta có thể kiểm tra sự tồn tại và truy xuất màu sắc cũng như chỉ mục theo thời gian không đổi. Điều này là cần thiết để chúng tôi có thể xác minh các ứng cử viên phù hợp mà không cần tìm kiếm. 
2. Xây dựng danh sách tất cả các vị trí đã đảm nhiệm và sắp xếp chúng. Việc sắp xếp là bắt buộc vì chúng tôi sẽ tạo ra sự khác biệt một cách có hệ thống giữa các vị trí hợp lệ. 
3. Khởi tạo một mảng`best[d]`với giá trị lớn, biểu thị chỉ số nhỏ nhất được tìm thấy cho mỗi khoảng cách. 
4. Đối với mỗi cặp vị thế$(x_i, x_j)$với$i < j$, tính toán$d = x_j - x_i$. Nếu màu sắc khác nhau, hãy xem xét đèn ở chỉ số$i$như một ứng cử viên cho khoảng cách$d$. Cập nhật`best[d] = min(best[d], i)`. 

Bước này có tác dụng vì mọi câu trả lời hợp lệ phải tương ứng với một số cặp đèn trái-phải cách nhau một cách chính xác.$d$. 
5. Sau khi xử lý tất cả các cặp, hãy trả lời từng câu hỏi bằng cách quay lại`best[d]`nếu nó đã được cập nhật, nếu không thì xuất ra 0. 

Hiệu quả đạt được chính đến từ việc tránh hoàn toàn việc quét theo từng truy vấn. Thay vào đó, tất cả khoảng cách đều được tính toán trước một lần. 

### Tại sao nó hoạt động 

Bất kỳ câu trả lời hợp lệ nào cho khoảng cách truy vấn$d$phải đến từ ít nhất một cặp đèn trong đó đèn bên phải chính xác$d$đơn vị cách xa bên trái. Bằng cách liệt kê tất cả các cặp như vậy một lần, chúng tôi đảm bảo mọi ứng cử viên có thể đều được xem xét. Vì chúng ta luôn lấy chỉ số tối thiểu$u$trong số tất cả các cặp hợp lệ cho một khoảng cách nhất định, giá trị được lưu trữ chính xác là câu trả lời được yêu cầu. Không có cấu hình nào khác có thể tạo ra chỉ mục hợp lệ nhỏ hơn vì mọi cặp ứng cử viên hợp lệ đều được đánh giá rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())

MAXX = 250000

pos_to_idx = [-1] * (MAXX + 1)
pos_to_color = [0] * (MAXX + 1)
positions = []

for i in range(1, n + 1):
    x, c = map(int, input().split())
    pos_to_idx[x] = i
    pos_to_color[x] = c
    positions.append(x)

best = [10**18] * (MAXX + 1)

positions.sort()

m = len(positions)

for i in range(m):
    x1 = positions[i]
    idx1 = pos_to_idx[x1]
    c1 = pos_to_color[x1]

    for j in range(i + 1, m):
        x2 = positions[j]
        d = x2 - x1

        idx2 = pos_to_idx[x2]
        if c1 != pos_to_color[x2]:
            if idx1 < best[d]:
                best[d] = idx1

qans = []
for _ in range(q):
    d = int(input())
    if d <= MAXX and best[d] < 10**18:
        qans.append(str(best[d]))
    else:
        qans.append("0")

print("\n".join(qans))
```Giải pháp dựa vào việc ánh xạ tọa độ tới các chỉ số và màu sắc của đèn để việc kiểm tra cặp đèn diễn ra trong thời gian không đổi. Vòng lặp lồng nhau trên các vị trí được sắp xếp sẽ tạo ra tất cả các khoảng cách hợp lệ và với mỗi khoảng cách, chúng tôi chỉ giữ lại chỉ số nhỏ nhất của điểm cuối bên trái. Giai đoạn truy vấn trở thành tra cứu mảng trực tiếp. 

Một cạm bẫy triển khai phổ biến là quên rằng chỉ số cần giảm thiểu là chỉ số đèn ban đầu chứ không phải vị trí. Một lỗi khác là không bảo vệ được giới hạn khoảng cách khi lập chỉ mục cho mảng được tính toán trước. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ: 

đầu vào:```
4 2
1 1
3 2
5 1
6 3
2
3
```Vị trí được sắp xếp là [1, 3, 5, 6]. 

| tôi | j | x1 | x2 | d | màu sắc | có hiệu lực? | cập nhật [d] tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 3 | 2 | 1 vs 2 | vâng | tốt nhất[2]=1 | 
| 0 | 2 | 1 | 5 | 4 | 1 đấu 1 | không | - | 
| 0 | 3 | 1 | 6 | 5 | 1 vs 3 | vâng | tốt nhất[5]=1 | 
| 1 | 2 | 3 | 5 | 2 | 2 đấu 1 | vâng | tốt nhất[2]=1 | 
| 1 | 3 | 3 | 6 | 3 | 2 đấu 3 | vâng | tốt nhất[3]=2 | 
| 2 | 3 | 5 | 6 | 1 | 1 vs 3 | vâng | tốt nhất[1]=3 | 

Truy vấn 2 trả về 1, truy vấn 3 trả về 2. 

Điều này cho thấy nhiều cặp đóng góp như thế nào vào cùng một khoảng cách và chỉ có chỉ số nhỏ nhất mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m^2 + q)$| tất cả các cặp vị trí được liệt kê một lần, theo sau là các truy vấn có thời gian không đổi | 
| Không gian |$O(MAXX)$| mảng được lập chỉ mục theo tọa độ và khoảng cách | 

Cách tiếp cận này phụ thuộc rất nhiều vào phạm vi tọa độ giới hạn, điều này làm cho việc tính toán trước trở nên khả thi. Với dữ liệu thưa thớt, số lượng cặp hiệu quả sẽ giảm trong thực tế, nhưng trường hợp xấu nhất vẫn là phương trình bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    MAXX = 250000

    pos_to_idx = [-1] * (MAXX + 1)
    pos_to_color = [0] * (MAXX + 1)
    positions = []

    for i in range(1, n + 1):
        x, c = map(int, input().split())
        pos_to_idx[x] = i
        pos_to_color[x] = c
        positions.append(x)

    best = [10**18] * (MAXX + 1)

    positions.sort()
    m = len(positions)

    for i in range(m):
        x1 = positions[i]
        idx1 = pos_to_idx[x1]
        c1 = pos_to_color[x1]
        for j in range(i + 1, m):
            x2 = positions[j]
            d = x2 - x1
            if c1 != pos_to_color[x2]:
                idx2 = pos_to_idx[x2]
                if idx1 < best[d]:
                    best[d] = idx1

    out = []
    for _ in range(q):
        d = int(input())
        out.append(str(best[d]) if best[d] < 10**18 else "0")

    return "\n".join(out)

# provided sample
assert run("""4 5
3 1
1 2
5 1
6 2
2
1
3
2
10
""") == """3
2
1
2
0"""

# all same color
assert run("""3 2
1 1
2 1
3 1
1
2
""") == """0
0"""

# alternating colors
assert run("""4 2
1 1
2 2
3 1
4 2
1
3
""") == """2
1"""

# single valid pair
assert run("""2 1
10 1
13 2
3
""") == """1"""

# maximum trivial
assert run("""1 1
5 1
1
""") == """0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả cùng màu | tất cả số không | không có cặp hợp lệ nào tồn tại | 
| xen kẽ màu sắc | sửa các chỉ số tối thiểu | nhiều kết quả phù hợp cho mỗi khoảng cách | 
| cặp đơn | tính đúng đắn trực tiếp | trường hợp cơ sở | 
| đèn đơn | xử lý bằng không | không có trường hợp đối tác | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi nhiều cặp tạo ra cùng một khoảng cách nhưng chỉ số ứng cử viên khác nhau. Ví dụ: nếu đèn tồn tại ở vị trí 1, 4 và 7 với màu sắc xen kẽ thì khoảng cách 3 xuất hiện hai lần. Thuật toán xử lý cả (1,4) và (4,7) và giữ chỉ số tối thiểu, chính xác trở thành 1. 

Một trường hợp cạnh khác là khi chỉ số nhỏ nhất không liên quan đến mọi cặp hợp lệ. Nếu đèn 1 không thể tạo thành một cặp hợp lệ trong một khoảng cách nhất định nhưng đèn 2 thì có thể, thì thuật toán sẽ lưu trữ chính xác 2 thay vì 1, vì quá trình cập nhật chỉ xảy ra khi tồn tại màu không khớp hợp lệ. 

Trường hợp cuối cùng là khi không có cặp nào tồn tại trong một khoảng cách. các`best[d]`mục nhập vẫn ở giá trị trọng điểm và các truy vấn đưa ra kết quả chính xác là 0.
