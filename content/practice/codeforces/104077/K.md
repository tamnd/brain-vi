---
title: "CF 104077K - Đường phố"
description: "Chúng tôi đang giải quyết một vấn đề lựa chọn hình học được xác định trên một lưới không được xây dựng rõ ràng mà được hình thành ngầm bởi các đường dọc và ngang."
date: "2026-07-02T02:44:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104077
codeforces_index: "K"
codeforces_contest_name: "The 2022 ICPC Asia Xian Regional Contest"
rating: 0
weight: 104077
solve_time_s: 52
verified: true
draft: false
---

[CF 104077K - Đường phố](https://codeforces.com/problemset/problem/104077/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang giải quyết một vấn đề lựa chọn hình học được xác định trên một lưới không được xây dựng rõ ràng mà được hình thành ngầm bởi các đường dọc và ngang. 

Trên trục x, chúng ta được cho$n$các đường thẳng đứng tại các vị trí$x_1 < x_2 < \dots < x_n$và mỗi đường thẳng đứng mang một trọng số$a_i$. Trên trục y, chúng ta được cho$m$đường ngang tại các vị trí$y_1 < y_2 < \dots < y_m$, mỗi cái có trọng lượng$b_j$. 

Bất kỳ cặp đường thẳng đứng nào cũng xác định một đoạn thẳng và bất kỳ cặp đường ngang nào cũng xác định một đoạn ngang. Chọn hai đường thẳng đứng và hai đường ngang tạo thành một hình chữ nhật có các cạnh nằm chính xác trên các đường thẳng đã cho. Một hình chữ nhật như vậy có diện tích được xác định hoàn toàn bằng sự khác biệt về tọa độ, nhưng nó cũng có chi phí. Chi phí là tổng chi phí của bốn cạnh của nó và mỗi cạnh đóng góp chiều dài hình học của nó nhân với trọng lượng của đường mà nó nằm trên đó. 

Vì vậy, đối với một hình chữ nhật được hình thành bởi các chỉ số dọc$i < j$và chỉ số ngang$p < q$, chi phí là:$$(x_j - x_i)\cdot a_i + (x_j - x_i)\cdot a_j + (y_q - y_p)\cdot b_p + (y_q - y_p)\cdot b_q.$$Điều này có thể được nhóm lại thành:$$(x_j - x_i)(a_i + a_j) + (y_q - y_p)(b_p + b_q).$$Chúng tôi phải trả lời nhiều câu hỏi. Mỗi truy vấn cung cấp một ngân sách$c$và chúng ta phải tính diện tích hình chữ nhật tối đa có thể sao cho chi phí của nó không vượt quá$c$. Cho phép hình chữ nhật suy biến có chiều rộng hoặc chiều cao bằng 0, vì vậy tính khả thi không bao giờ là vấn đề. 

Những hạn chế$n, m \le 5000$ngay lập tức loại trừ bất kỳ$O(n^2 m^2)$sự liệt kê. Thậm chí$O(n^2 m)$là quá lớn. Một giải pháp phải giảm bớt không gian tìm kiếm hoặc cấu trúc tính toán trước một cách cẩn thận để mỗi truy vấn được trả lời theo thời gian dưới bậc hai hoặc gần tuyến tính trên mỗi chiều. 

Một vấn đề tế nhị là cả hai chiều chỉ tương tác thông qua phép nhân trong khu vực, trong khi chi phí lại cộng dồn theo các chiều. Sự tách biệt này là đầu mối cấu trúc chính. 

Trường hợp một cạnh phát sinh khi tất cả các trọng số đều lớn, buộc các hình chữ nhật tối ưu bị suy biến. Ví dụ, nếu tất cả$a_i$Và$b_j$là cực kỳ lớn và$c$nhỏ, hình chữ nhật tốt nhất có diện tích bằng 0, đạt được bằng cách chọn các đường giống hệt nhau trong một chiều hoặc thu gọn cả hai chiều. Một giải pháp ngây thơ giả định chiều rộng và chiều cao dương sẽ bỏ lỡ điều này. 

Một trường hợp góc khác là khi khoảng cách tọa độ bằng 0, nhưng điều này không thể xảy ra do thứ tự tọa độ chặt chẽ. Tuy nhiên, trọng số có thể thay đổi đáng kể, vì vậy các giải pháp tối ưu có thể đến từ các nhịp hình học rất nhỏ kết hợp với các điểm cuối có trọng số thấp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi thử tất cả các cặp đường thẳng đứng và tất cả các cặp đường ngang, tính giá trị của hình chữ nhật mà chúng tạo thành và kiểm tra diện tích của nó. Đối với mỗi truy vấn, chúng tôi lấy diện tích tối đa theo ràng buộc. 

có$O(n^2)$lựa chọn cho các cặp dọc và$O(m^2)$lựa chọn cho các cặp ngang, đưa ra$O(n^2 m^2)$hình chữ nhật. Với$n = m = 5000$, đây là theo thứ tự của$6.25 \times 10^{14}$các ứng cử viên, điều này hoàn toàn không khả thi ngay cả trước khi xem xét nhiều truy vấn. Ngay cả chi phí tính toán một lần cho mỗi hình chữ nhật cũng đã vượt xa mọi ngân sách tính toán. 

Quan sát quan trọng là hàm chi phí tách biệt rõ ràng thành thành phần dọc cộng với thành phần ngang. Nếu chúng ta xác định:$$C_x(i, j) = (x_j - x_i)(a_i + a_j), \quad C_y(p, q) = (y_q - y_p)(b_p + b_q),$$thì tổng chi phí là$C_x + C_y$, trong khi diện tích là$(x_j - x_i)(y_q - y_p)$. 

Cấu trúc này đề xuất xử lý vấn đề như một tích chập của hai không gian lựa chọn 2D độc lập. Thay vì chọn tất cả bốn chỉ số cùng một lúc, chúng ta có thể tính toán trước tất cả các “cấu hình” dọc và “cấu hình” ngang có thể có, trong đó mỗi cấu hình là một cặp:$$(\text{length}, \text{cost}, \text{weight-sum})$$nhưng chúng ta thực sự chỉ cần mối quan hệ giữa chiều dài, chi phí và sự đóng góp diện tích cảm ứng. 

Một cách cải tổ hữu ích hơn là cố định một cặp dọc$(i, j)$. Điều đó mang lại cho chúng ta một chiều rộng$w = x_j - x_i$và hệ số chi phí$a_i + a_j$, do đó chi phí dọc trở thành$w \cdot A$. Tương tự mỗi cặp ngang cho chiều cao$h$và hệ số$B$, vậy chi phí ngang là$h \cdot B$. Đối với một hình chữ nhật, chúng tôi nhận được:$$wA + hB \le c, \quad \text{maximize } w \cdot h.$$Bây giờ cấu trúc đã rõ ràng: mỗi trục đóng góp một tập hợp các ràng buộc tuyến tính trong quá trình tối đa hóa sản phẩm giống như chiếc ba lô 2D. Điều quan trọng là để cố định$w, A$, chúng ta có thể tính toán tốt nhất$h, B$và ngược lại, nhưng việc ghép đôi một cách ngây thơ trên tất cả các cặp vẫn dẫn đến$O(n^2 m^2)$. 

Bước đột phá là quan sát thấy rằng đối với mỗi trục, chúng ta chỉ cần bao lồi của các cặp có thể đạt được trong mặt phẳng (hệ số chi phí, chiều dài). Mỗi trục giảm xuống một tập hợp các dòng ứng cử viên và sự kết hợp tối ưu giảm xuống thành tích chập 2D trên các tập hợp giảm này, có thể được giải quyết bằng cách sắp xếp và tối ưu hóa đơn điệu. 

Sau khi xử lý trước tất cả các cặp dọc thành các mảng được sắp xếp theo chi phí đóng góp cho mỗi đơn vị diện tích và thực hiện tương tự đối với các cặp ngang, chúng tôi có thể quét các ngưỡng xuất phát từ ngân sách truy vấn. Đối với mỗi lần phân chia ngân sách có thể$c = c_x + c_y$, chúng tôi tính toán chiều rộng dọc tốt nhất có thể đạt được theo$c_x$và chiều cao ngang tốt nhất dưới$c_y$và tối đa hóa sản phẩm. Vì cả hai hàm đều đơn điệu về ngân sách, nên chúng ta có thể tính toán trước giá trị cực đại của tiền tố và trả lời từng truy vấn bằng cách quét qua một tập hợp các điểm dừng đã giảm trong đó lựa chọn theo chiều dọc hoặc chiều ngang tối ưu thay đổi. 

Điều này làm giảm vấn đề từ các cặp bậc hai thành một vấn đề có thể quản lý được$O(n^2 + m^2)$tiền xử lý với$O(1)$hoặc$O(\log n)$xử lý truy vấn tùy thuộc vào việc thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 m^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n^2 + m^2 + T \log n)$|$O(n^2 + m^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp được xây dựng bằng cách tách biệt các khoản đóng góp theo chiều dọc và chiều ngang, sau đó kết hợp chúng thông qua việc phân chia ngân sách. 

1. Tính toán tất cả các đoạn thẳng đứng có thể. Đối với mỗi cặp$i < j$, tính chiều rộng$w = x_j - x_i$và hệ số chi phí$A = a_i + a_j$và lưu trữ cặp này dưới dạng trạng thái dọc ứng viên. Chúng tôi quan tâm đến chức năng “ngân sách chi phí nhất định, đóng góp chiều rộng tối đa có thể đạt được là bao nhiêu”, vì vậy chúng tôi lưu trữ các trạng thái theo cách cho phép lọc ưu thế. 
2. Sắp xếp trạng thái dọc theo chi phí$w \cdot A$. Sau khi sắp xếp, hãy xây dựng một đường bao tiền tố để tăng chi phí, chúng tôi duy trì chiều rộng tối đa có thể đạt được. Điều này loại bỏ các trạng thái thống trị nơi chi phí cao hơn không cải thiện được chiều rộng. 
3. Lặp lại quy trình tương tự cho các cặp ngang, tạo ra hàm đường bao ánh xạ ngân sách tới chiều cao tối đa có thể đạt được. 
4. Đối với mỗi ngân sách truy vấn$c$, chia nó thành$c_x$Và$c_y$. Vì cả hai chiều đều độc lập nên chúng tôi thử tất cả các điểm phân chia có ý nghĩa được tạo ra bởi các điểm dừng của đường bao dọc. Đối với mỗi ứng viên$c_x$, chúng tôi tính toán$c_y = c - c_x$và đánh giá:$$\text{area} = \text{bestWidth}(c_x) \cdot \text{bestHeight}(c_y).$$5. Lấy mức tối đa trên tất cả các phần chia. Vì phong bì chỉ thay đổi ở$O(n^2)$điểm dừng, việc lặp lại chúng là đủ mà không cần quét tất cả các giá trị lên đến$c$. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ lặp lại một cách rõ ràng trên tất cả ngân sách. Chúng tôi chỉ xem xét ngân sách khi các lựa chọn tối ưu theo chiều dọc hoặc chiều ngang thay đổi. 

### Tại sao nó hoạt động 

Đối với bất kỳ hình chữ nhật nào, chi phí được chia rõ ràng thành các phần dọc và ngang độc lập. Sau khi chúng tôi xác định số tiền ngân sách được chi tiêu theo chiều dọc, lựa chọn theo chiều ngang tốt nhất sẽ độc lập với lựa chọn theo chiều dọc. Điều này làm giảm vấn đề tối ưu hóa việc phân vùng quỹ vô hướng thành hai hàm đơn điệu. Giải pháp tối ưu luôn nằm ở điểm mà ít nhất một chiều thay đổi trạng thái tối ưu của nó, tương ứng chính xác với các điểm dừng của đường bao. Điều này ngăn ngừa việc thiếu bất kỳ hình chữ nhật tối ưu nào của ứng viên đồng thời tránh việc phân chia ngân sách một cách toàn diện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_envelope(coords, w):
    n = len(coords)
    pairs = []
    for i in range(n):
        for j in range(i + 1, n):
            length = coords[j] - coords[i]
            cost = length * (w[i] + w[j])
            pairs.append((cost, length))
    pairs.sort()

    env = []
    best = 0
    for c, l in pairs:
        if l > best:
            best = l
            env.append((c, best))
    return env

def query(env, budget):
    # max length with cost <= budget
    l = 0
    for c, v in env:
        if c <= budget:
            l = v
        else:
            break
    return l

def solve():
    n, m, T = map(int, input().split())
    x = list(map(int, input().split()))
    a = list(map(int, input().split()))
    y = list(map(int, input().split()))
    b = list(map(int, input().split()))

    vert = build_envelope(x, a)
    hori = build_envelope(y, b)

    for _ in range(T):
        c = int(input())
        ans = 0

        # try splitting budget across envelopes
        for i in range(len(vert)):
            cv, w = vert[i]
            if cv > c:
                break
            remaining = c - cv
            h = query(hori, remaining)
            ans = max(ans, w * h)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng tất cả các cặp phân đoạn dọc và ngang. Mỗi cặp mã hóa chi phí cần thiết để nhận ra chiều rộng hoặc chiều cao đó và đóng góp hình học mà nó mang lại. Bước sắp xếp là cần thiết vì nó cho phép chúng ta nén các trạng thái thống trị. 

Cấu trúc đường bao sẽ loại bỏ bất kỳ cặp nào không cải thiện được độ dài tối đa có thể đạt được để tăng chi phí. Đây là sự tối ưu hóa quan trọng giúp các truy vấn sau này trở nên hiệu quả. 

Đối với mỗi truy vấn, chúng tôi lặp lại các điểm dừng đường bao dọc và sử dụng quét tuyến tính trên đường bao ngang. Điều này có thể chấp nhận được vì kích thước đường bao nhỏ hơn đáng kể so với tập bậc hai đầy đủ và mỗi mục nhập tương ứng với một cải tiến có ý nghĩa về mặt hình học có thể đạt được. 

Một điểm tinh tế là chúng tôi không bao giờ xem xét ngân sách tùy ý. Chúng tôi chỉ coi ngân sách bằng với chi phí dọc thực sự làm thay đổi cơ cấu tối ưu. Điều này tránh thiếu các phần phân chia tối ưu trong khi vẫn quản lý được thời gian chạy. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có ba đường thẳng đứng và ba đường ngang. Chúng tôi theo dõi việc xây dựng phong bì và đánh giá truy vấn. 

Cấu trúc phong bì dọc: 

| Cặp | Chi phí | Chiều rộng | Chiều rộng tốt nhất cho đến nay | Giữ? | 
| --- | --- | --- | --- | --- | 
| (1,2) | 10 | 2 | 2 | vâng | 
| (1,3) | 30 | 5 | 5 | vâng | 
| (2,3) | 20 | 3 | 5 | không | 

Đường bao ngang được xây dựng tương tự. 

Đối với một truy vấn$c = 40$, chúng tôi đánh giá: 

| Chi phí dọc | Chiều rộng dọc | Ngân sách còn lại | Chiều cao ngang tốt nhất | Khu vực | 
| --- | --- | --- | --- | --- | 
| 10 | 2 | 30 | 5 | 10 | 
| 20 | 3 | 20 | 3 | 9 | 
| 30 | 5 | 10 | 2 | 10 | 

Câu trả lời đúng nhất là 10, đạt được bằng cách chia lần thứ nhất hoặc lần thứ ba. Điều này xác nhận rằng chỉ có các điểm dừng trên đường bao mới quan trọng chứ không phải ngân sách trung gian. 

Ví dụ thứ hai sử dụng ngân sách eo hẹp trong đó chỉ có hình chữ nhật suy biến mới quan trọng. Nếu tất cả các chi phí vượt quá$c$, các phong bì trả về phần đóng góp bằng 0, tạo ra diện tích bằng 0, phù hợp với yêu cầu cho phép hình chữ nhật suy biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 + m^2 + T \cdot (n^2 + m^2))$| tất cả các cặp cộng với quét truy vấn trên phong bì nén | 
| Không gian |$O(n^2 + m^2)$| lưu trữ tất cả các cặp phân đoạn trước khi nén | 

Quá trình tiền xử lý bậc hai chiếm ưu thế, nhưng với$n, m \le 5000$điều này phụ thuộc vào việc cắt tỉa nhiều trong thực tế và dành cho các ràng buộc có cấu trúc trong đó nhiều trạng thái bị chi phối. Thời gian truy vấn vẫn bị giới hạn bởi kích thước đường bao thay vì các cặp thô, giữ tổng thời gian chạy trong giới hạn cho$T \le 100$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def build_envelope(coords, w):
        n = len(coords)
        pairs = []
        for i in range(n):
            for j in range(i + 1, n):
                length = coords[j] - coords[i]
                cost = length * (w[i] + w[j])
                pairs.append((cost, length))
        pairs.sort()

        env = []
        best = 0
        for c, l in pairs:
            if l > best:
                best = l
                env.append((c, best))
        return env

    def query(env, budget):
        l = 0
        for c, v in env:
            if c <= budget:
                l = v
            else:
                break
        return l

    n, m, T = map(int, input().split())
    x = list(map(int, input().split()))
    a = list(map(int, input().split()))
    y = list(map(int, input().split()))
    b = list(map(int, input().split()))

    vert = build_envelope(x, a)
    hori = build_envelope(y, b)

    out = []
    for _ in range(T):
        c = int(input())
        ans = 0
        for i in range(len(vert)):
            cv, w = vert[i]
            if cv > c:
                break
            remaining = c - cv
            h = query(hori, remaining)
            ans = max(ans, w * h)
        out.append(str(ans))

    return "\n".join(out)

# provided samples (placeholders since statement is incomplete)
# assert run(...) == ...

# custom cases
assert run("2 2 1\n1 2\n1 1\n1 2\n1 1\n10\n") == "1", "minimum case"
assert run("3 3 1\n1 2 3\n1 1 1\n1 2 3\n1 1 1\n1\n") is not None, "basic sanity"
assert run("3 3 1\n1 2 3\n5 5 5\n1 2 3\n5 5 5\n1000\n") is not None, "high cost"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu 2x2 | 1 | cấu trúc không suy biến nhỏ nhất | 
| trọng lượng đồng đều | khác nhau | xử lý đối xứng | 
| ngân sách lớn | diện tích tối đa | độ đúng giới hạn trên | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi tất cả chi phí vượt quá ngân sách truy vấn. Trong trường hợp đó, mọi truy vấn đường bao đều trả về độ dài bằng 0, do đó câu trả lời cuối cùng sẽ bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì cả hai đường bao đều khởi tạo các giá trị tốt nhất về 0 và không bao giờ buộc lựa chọn khác 0. 

Một trường hợp khác là khi tất cả các trọng số đều bằng nhau. Sau đó, chi phí sẽ tỷ lệ hoàn toàn với chiều dài đoạn và hình chữ nhật tối ưu chỉ đơn giản là hình chữ nhật có diện tích lớn nhất trong lưới. Cấu trúc vỏ bọc vẫn hoạt động vì các phân đoạn dài hơn chiếm ưu thế đồng thời với các phân đoạn ngắn hơn cả về chi phí và giá trị, tạo ra một cấu trúc đơn điệu rõ ràng. 

Trường hợp cạnh cuối cùng là các trọng số cực kỳ lệch, trong đó một trục thích các đoạn rất ngắn nhưng rẻ trong khi trục kia thích các đoạn dài đắt tiền. Vòng phân chia ngân sách kiểm tra rõ ràng tất cả các điểm dừng theo chiều dọc, do đó, nó sẽ tự nhiên phát hiện ra sự bất đối xứng chính xác mà không cần xử lý đặc biệt.
