---
title: "CF 105838H - Triển khai phòng thủ"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả một tập hợp các điểm trên lưới 2D vô hạn. Mỗi điểm đại diện cho một tháp được đặt ở tọa độ nguyên."
date: "2026-06-21T22:40:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105838
codeforces_index: "H"
codeforces_contest_name: "The 14th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 105838
solve_time_s: 46
verified: true
draft: false
---

[CF 105838H - Triển khai phòng thủ](https://codeforces.com/problemset/problem/105838/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả một tập hợp các điểm trên lưới 2D vô hạn. Mỗi điểm đại diện cho một tháp được đặt ở tọa độ nguyên. Từ mỗi tháp, bốn tia liên tục được phát ra theo hướng dương và âm của trục x và trục y, kéo dài vô tận và bao phủ mọi điểm chúng đi qua. 

Hai tòa tháp được coi là “tấn công lẫn nhau” nếu mỗi tòa tháp nằm trên ít nhất một tia của đối phương. Vì các tia chỉ đi dọc theo các đường trục song song nên điều kiện này chuyển thành một cấu trúc hình học rất cứng nhắc: một tòa tháp ở$(a_i, b_i)$tấn công một tòa tháp khác tại$(a_j, b_j)$khi và chỉ khi chúng có cùng tọa độ x hoặc cùng tọa độ y. 

Nhiệm vụ là đếm số cặp không có thứ tự$(i, j)$,$i < j$, sao cho$a_i = a_j$hoặc$b_i = b_j$. 

Những hạn chế là lớn. Tổng số điểm trên tất cả các trường hợp thử nghiệm lên tới$10^5$, do đó, bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các cặp, sẽ là$O(n^2)$, ngay lập tức là quá chậm. Chúng ta cần một cái gì đó gần với thời gian tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp phức tạp nhưng quan trọng là xử lý các bản sao một cách cẩn thận. Nếu nhiều tòa tháp có chung cả tọa độ x và y thì chúng sẽ là những điểm giống hệt nhau, nhưng bài toán đảm bảo tọa độ là khác nhau, vì vậy chúng ta tránh được sự mơ hồ đó. Tuy nhiên, chúng ta vẫn phải tránh các cặp đếm kép có chung cả hai tọa độ trong suy luận trung gian, mặc dù điều đó không thể xảy ra ở đây do tính duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản: kiểm tra từng cặp tháp và xác minh xem tọa độ x hay tọa độ y của chúng khớp nhau. Điều này đúng vì nó trực tiếp thực hiện định nghĩa tấn công lẫn nhau. Tuy nhiên, với tới$10^5$điểm, điều này đòi hỏi khoảng$5 \times 10^9$so sánh trong trường hợp xấu nhất là không khả thi. 

Quan sát quan trọng là điều kiện phân tách rõ ràng thành hai quan hệ tương đương độc lập. Một cặp hợp lệ nếu nó thuộc cùng một nhóm x hoặc cùng một nhóm y. Điều này gợi ý tính số lượng đóng góp theo nhóm thay vì theo cặp. 

Nếu chúng ta sửa tọa độ x$a$, tất cả các tòa tháp có tọa độ x đó tạo thành một nhóm. Bất kỳ cặp nào trong nhóm này đều hợp lệ, đóng góp$\binom{k}{2}$nếu có$k$tháp có giá trị x đó. Tương tự, với mỗi tọa độ y$b$, tất cả các tòa tháp có tọa độ y đó cũng đóng góp$\binom{k}{2}$. 

Vấn đề tế nhị là cách tiếp cận kết hợp các điều kiện này có thể nhân đôi số lượng các cặp có chung cả x và y. Tuy nhiên, vì tất cả các điểm đều khác nhau nên không có hai tòa tháp nào có chung cả hai tọa độ nên không có giao điểm nào để trừ. 

Do đó, bài toán giảm xuống việc đếm tần số của các giá trị x và giá trị y một cách riêng biệt và tính tổng các đóng góp tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đếm tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc tất cả tọa độ x và tọa độ y, đồng thời lưu trữ số đếm tần số cho từng giá trị riêng biệt. Chúng tôi duy trì hai bản đồ hoặc mảng băm: một cho giá trị x và một cho giá trị y. Điều này là cần thiết vì câu trả lời chỉ phụ thuộc vào số điểm chia sẻ mỗi tọa độ chứ không phụ thuộc vào danh tính của chúng. 
2. Với mọi giá trị tọa độ x phân biệt, gọi tần số của nó là$k$. Chúng tôi thêm$k \cdot (k - 1) / 2$để trả lời. Điều này đếm tất cả các cặp tháp không có thứ tự được căn chỉnh theo chiều dọc trên đường x đó. 
3. Với mọi giá trị tọa độ y phân biệt, gọi tần số của nó là$k$. Chúng tôi lại thêm$k \cdot (k - 1) / 2$để trả lời. Điều này đếm tất cả các cặp tháp không có thứ tự được sắp xếp theo chiều ngang trên đường y đó. 
4. Xuất tổng tích lũy cho test case. 

Bước lý luận chính là mỗi cặp hợp lệ được xác định duy nhất bằng cách căn chỉnh theo chiều dọc hoặc chiều ngang. Vì chúng tôi đang đếm tất cả các cặp bên trong mỗi lớp tọa độ một cách độc lập nên mọi tương tác hợp lệ sẽ được ghi lại chính xác một lần. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ cặp tháp hợp lệ nào. Nếu tấn công nhau thì phải chia sẻ x hoặc chia sẻ y. Nếu họ chia sẻ x, họ sẽ được bao gồm trong chính xác một đóng góp của nhóm x thông qua$\binom{k}{2}$. Nếu họ chia sẻ y, họ sẽ được bao gồm trong đúng một đóng góp của nhóm y. Vì tọa độ là khác nhau nên một cặp không thể chia sẻ đồng thời cả x và y, do đó không có sự chồng chéo giữa hai quá trình đếm. Điều này thiết lập sự tương ứng một-một giữa các cặp hợp lệ và các đóng góp được tổng hợp bằng thuật toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        xs = list(map(int, input().split()))
        ys = list(map(int, input().split()))

        cx = {}
        cy = {}

        for x in xs:
            cx[x] = cx.get(x, 0) + 1
        for y in ys:
            cy[y] = cy.get(y, 0) + 1

        ans = 0

        for v in cx.values():
            ans += v * (v - 1) // 2
        for v in cy.values():
            ans += v * (v - 1) // 2

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách việc đếm thành hai tích lũy tần số độc lập. Các bản dựng vòng lặp đầu tiên sẽ đếm số lượng tháp nằm trên mỗi đường thẳng đứng và các bản dựng thứ hai sẽ tính số lượng các đường ngang. Vòng lặp cuối cùng chuyển đổi các tần số này thành số lượng cặp bằng cách sử dụng công thức kết hợp tiêu chuẩn. 

Một lỗi triển khai phổ biến là cố gắng tăng câu trả lời trong quá trình phân tích cú pháp đầu vào mà không tách biệt hoàn toàn số x và y, điều này có nguy cơ trộn lẫn logic và khiến việc gỡ lỗi trở nên khó khăn hơn. Một cạm bẫy khác là quên phép chia số nguyên trong công thức tổ hợp, nhưng Python`//`đảm bảo tính đúng đắn. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ: 

đầu vào:```
n = 4
x = [1, 1, 2, 3]
y = [1, 2, 2, 2]
```Chúng tôi tính toán tần số. 

Đối với giá trị x: 

| x | đếm | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 1 | 

Đối với giá trị y: 

| y | đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 3 | 

Bây giờ chúng tôi tính toán đóng góp. 

| Nguồn | Giá trị | Đóng góp | 
| --- | --- | --- | 
| x=1 | 2 | 1 | 
| x=2 | 1 | 0 | 
| x=3 | 1 | 0 | 
| y=1 | 1 | 0 | 
| y=2 | 3 | 3 | 

Câu trả lời cuối cùng là 4. 

Dấu vết này cho thấy mỗi nhóm được căn chỉnh theo trục đóng góp độc lập và các cặp được phân chia tự nhiên theo tọa độ bằng nhau. 

Bây giờ hãy xem xét trường hợp tất cả các điểm nằm trên một đường ngang: 

đầu vào:```
n = 5
x = [1, 2, 3, 4, 5]
y = [7, 7, 7, 7, 7]
```| y | đếm | đóng góp | 
| --- | --- | --- | 
| 7 | 5 | 10 | 

Tất cả số x là 1 nên chúng không đóng góp gì. Kết quả là 10, khớp tất cả các cặp trong số năm điểm. 

Điều này chứng tỏ rằng chỉ riêng nhóm y đã nắm bắt được đầy đủ tất cả các tương tác khi căn chỉnh hoàn toàn theo chiều ngang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi điểm được xử lý một lần cho x và một lần cho y và tập hợp tần số là tuyến tính | 
| Không gian | O(n) | Bản đồ băm lưu trữ tối đa n giá trị tọa độ riêng biệt | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm vẫn tuyến tính trong tổng số điểm, phù hợp thoải mái trong giới hạn$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        xs = list(map(int, input().split()))
        ys = list(map(int, input().split()))
        cx = Counter(xs)
        cy = Counter(ys)

        ans = 0
        for v in cx.values():
            ans += v * (v - 1) // 2
        for v in cy.values():
            ans += v * (v - 1) // 2
        out.append(str(ans))
    return "\n".join(out)

# provided sample (as interpreted from statement)
assert run("""1
4
1 2 3 1
2 3 1 3
""") == "2"

# all on same x
assert run("""1
3
5 5 5
1 2 3
""") == "3"

# all on same y
assert run("""1
4
1 2 3 4
7 7 7 7
""") == "6"

# no shared coordinates
assert run("""1
3
1 2 3
4 5 6
""") == "0"

# mixed case
assert run("""1
5
1 1 2 2 3
1 2 2 3 3
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều giống nhau x | 3 | độ chính xác của nhóm dọc | 
| tất cả đều giống nhau | 6 | độ chính xác của nhóm ngang | 
| không chồng chéo | 0 | không có kết quả dương tính giả | 
| hỗn hợp | 4 | tính đúng đắn của việc đếm kết hợp | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi tất cả các điểm có cùng tọa độ x. Thuật toán xử lý việc này bằng cách đặt tất cả các điểm vào một nhóm tần số x duy nhất, tạo ra$\binom{n}{2}$, trong khi tất cả các tần số y bị cô lập và đóng góp bằng không. Dấu vết trong các ví dụ đã thực hiện đã xác nhận rằng chỉ riêng nhóm x đã tạo ra câu trả lời đầy đủ và chính xác. 

Một trường hợp khác là khi tất cả các điểm có cùng tọa độ y. Về mặt đối xứng, tần số y chiếm ưu thế và phía x không đóng góp gì. Vì thuật toán xử lý x và y độc lập nên không có số hạng tương tác nào có thể làm sai lệch kết quả. 

Trường hợp cạnh cuối cùng là khi tất cả các tọa độ đều khác biệt ở cả hai chiều. Cả hai bản đồ tần số đều bao gồm toàn bộ một bản đồ, vì vậy mọi$\binom{1}{2}$hạn biến mất. Thuật toán đưa ra kết quả bằng 0 một cách chính xác, phù hợp với thực tế là không có hai tòa tháp nào có chung đường tấn công.
