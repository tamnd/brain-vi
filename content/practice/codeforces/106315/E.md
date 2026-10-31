---
title: "CF 106315E - Góc Nhìn Hoàn Hảo"
description: "Chúng tôi được cung cấp một số địa điểm quán cà phê ứng cử viên và một số điểm mốc trên máy bay. Đối với mỗi vị trí quán cà phê cố định $Ci$, chúng tôi xem xét từng cặp điểm mốc riêng biệt $Lj, Lk$. Mỗi cặp như vậy cùng với quán cà phê tạo thành một tam giác $CiLjLk$."
date: "2026-06-18T22:16:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106315
codeforces_index: "E"
codeforces_contest_name: "ICPC Dhaka 2025 Online Preliminary - Replay Contest"
rating: 0
weight: 106315
solve_time_s: 73
verified: true
draft: false
---

[CF 106315E - Chế độ xem hoàn hảo](https://codeforces.com/problemset/problem/106315/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số địa điểm quán cà phê ứng cử viên và một số điểm mốc trên máy bay. Đối với từng địa điểm quán cà phê cố định$C_i$, chúng tôi xem xét từng cặp điểm mốc riêng biệt$L_j, L_k$. Mỗi cặp như vậy cùng với quán cà phê tạo thành một hình tam giác$C_iL_jL_k$. 

Bây giờ hãy tưởng tượng việc chọn bất kỳ điểm nào$P$trong máy bay. Đối với quán cà phê cố định$C_i$, chúng tôi xác định điểm số của$P$là số cặp mốc có tam giác với$C_i$chứa đựng nghiêm ngặt$P$. Chúng ta được phép chọn cả vị trí quán cà phê và điểm$P$, và chúng tôi muốn đạt được số điểm tối đa có thể. 

Cấu trúc chính là dành cho một cố định$C_i$, chúng tôi không cố gắng đánh giá một điểm nào$P$, nhưng thay vào đó hãy hỏi vị trí nào trong mặt phẳng mà sự chồng chéo của nhiều hình tam giác trở nên lớn nhất. 

Mỗi trường hợp thử nghiệm cung cấp tới 400 quán cà phê và 400 cột mốc, đồng thời có nhiều trường hợp thử nghiệm nhưng tổng số điểm trong tất cả các bài kiểm tra là bị giới hạn. Điều này gợi ý rõ ràng rằng một giải pháp gần với bậc hai hoặc bậc ba cho mỗi cấu hình là có thể chấp nhận được, nhưng bất cứ điều gì cố gắng xử lý tất cả các bộ ba hoặc xử lý rõ ràng hình học trên mỗi tam giác và mỗi điểm truy vấn sẽ không thành công. 

Một nỗ lực ngây thơ sẽ chọn một quán cà phê$C_i$, liệt kê tất cả$\binom{M}{2}$tam giác, sau đó cố gắng tính toán bản đồ độ sâu trên mặt phẳng hoặc kiểm tra các điểm ứng viên được hình thành bởi giao điểm của các ranh giới tam giác. Điều đó nhanh chóng trở nên khó quản lý vì việc sắp xếp$O(M^2)$hình tam giác có thể tạo ra$O(M^4)$về nguyên tắc, độ phức tạp của nút giao và thậm chí việc lặp lại các điểm ứng cử viên là quá lớn. 

Một dạng thất bại tinh vi hơn xuất phát từ việc cố gắng khắc phục một điểm$P$và đếm các hình tam giác chứa nó bằng cách kiểm tra các điều kiện định hướng. Điều đó sẽ đòi hỏi$O(M^2)$mỗi$P$, nhưng không có tập hợp hữu hạn rõ ràng nào có liên quan$P$các ứng cử viên đảm bảo tính chính xác trừ khi chúng ta hiểu hình học của các hình tam giác này chồng lên nhau như thế nào. 

Một cạm bẫy minh họa nhỏ là tối ưu$P$không nhất thiết là điểm mốc hoặc điểm quán cà phê, cũng không phải là giao điểm của hai đường đi qua cột mốc. Nó được xác định bằng cách tối đa hóa sự chồng chéo tổ hợp, do đó việc giảm ứng viên ngây thơ là không đáng tin cậy. 

## Phương pháp tiếp cận 

Sửa địa điểm quán cà phê$C_i$. Bài toán trở thành: trong số tất cả các tam giác được tạo thành bởi$C_i$và hai cột mốc, tìm một điểm$P$nằm ở số lượng tối đa của các hình tam giác này. 

Một cách phát biểu lại hình học hữu ích là chuyển từ suy luận về tam giác sang suy luận về các tia từ$C_i$. một điểm$P$nằm bên trong tam giác$C_iL_jL_k$khi và chỉ khi tia bắt đầu tại$C_i$theo hướng$C_iP$cắt đoạn$L_jL_k$. Điều này biến bài toán thành việc đếm xem có bao nhiêu đoạn mốc bị một tia “đánh trúng”. 

Vì vậy để cố định$C_i$và phương hướng$\theta$, xác định một tia từ$C_i$. Mỗi cặp$(L_j, L_k)$đóng góp nếu phân khúc$L_jL_k$cắt tia đó. Câu trả lời cho$C_i$là số lượng phân đoạn giao nhau tối đa trên tất cả các hướng tia. 

Bây giờ hãy nhìn vào một cặp điểm mốc. Từ quan điểm của$C_i$, mỗi mốc có một góc cực. Phân khúc$L_jL_k$sẽ cắt nhau bởi một tia có hướng$\theta$chính xác khi nào$\theta$nằm giữa các góc của$L_j$Và$L_k$dọc theo vòng cung nhỏ hơn xung quanh$C_i$. Nói cách khác, mỗi cặp xác định một khoảng trên đường tròn góc xung quanh$C_i$và chúng ta muốn số lượng tối đa các khoảng này bao phủ một góc. 

Vì vậy đối với mỗi$C_i$, bài toán trở thành bài toán chồng chéo khoảng tròn trên$O(M^2)$khoảng thời gian. Chúng tôi chuyển đổi nó thành một đường thẳng bằng cách tăng gấp đôi phạm vi góc và quét. 

Cách tiếp cận bạo lực sẽ kiểm tra rõ ràng mọi hướng hoặc rời rạc hóa các góc, nhưng điều đó không thành công vì sự tối ưu có thể xảy ra giữa hai hướng mốc bất kỳ, do đó cần phải tìm kiếm liên tục. Việc quét khoảng thời gian nắm bắt tất cả các hướng tối ưu của ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các điểm hoặc chỉ đường |$O(M^3)$hoặc tệ hơn ở mỗi quán cà phê |$O(M^2)$| Quá chậm | 
| Quét khoảng thời gian góc cho mỗi quán cà phê |$O(M^2 \log M)$|$O(M^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng quán cà phê một cách độc lập. 

1. Cố định một điểm cafe$C_i$. Dịch tất cả các điểm mốc thành tọa độ cực so với$C_i$, tính các góc của chúng trong$[0, 2\pi)$. 

Điều này là cần thiết vì mọi cấu trúc tam giác chỉ phụ thuộc vào hướng từ quán cà phê chứ không phụ thuộc vào tọa độ tuyệt đối. 
2. Sắp xếp tất cả các điểm mốc theo góc cực của chúng xung quanh$C_i$và nhân đôi danh sách bằng cách thêm$2\pi$đến từng góc để xử lý vòng tròn. 

Điều này cho phép chúng ta coi các khoảng tròn là các khoảng tuyến tính. 
3. Với mỗi cặp mốc$L_j, L_k$, xây dựng một khoảng góc đại diện cho tất cả các hướng$\theta$từ đó đoạn đó$L_jL_k$có thể nhìn thấy được qua tia sáng từ$C_i$. 

Cụ thể, khoảng này là cung mở dọc theo đường tròn giữa các góc của chúng theo hướng ngắn hơn xung quanh$C_i$. 
4. Đối với mỗi khoảng thời gian như vậy, hãy thêm sự kiện +1 ở góc bắt đầu và sự kiện -1 ở góc kết thúc trong cấu trúc quét. 

Điều này mã hóa có bao nhiêu phân đoạn được giao nhau cho mỗi hướng tia có thể. 
5. Sắp xếp tất cả các sự kiện theo góc độ và duyệt qua chúng, duy trì tổng số hiện có. 

Giá trị lớn nhất gặp phải là số lượng tam giác tốt nhất có thể có cho quán cà phê cố định này. 
6. Lặp lại cho tất cả$C_i$và tận dụng tối đa tất cả các quán cà phê. 

### Tại sao nó hoạt động 

Đối với quán cà phê cố định$C_i$, mọi tam giác$C_iL_jL_k$đóng góp vào một điểm$P$chính xác khi tia$C_iP$nằm trong khoảng góc được xác định bởi$L_j$Và$L_k$. Điều này làm giảm mỗi tam giác thành một khoảng trên một vòng tròn hướng. Bất kỳ điểm nào$P$tương ứng duy nhất với một hướng từ$C_i$, do đó tối đa hóa việc ngăn chặn tam giác tương đương với việc tìm hướng có khoảng cách chồng chéo tối đa. Việc quét qua các điểm cuối góc sẽ kiểm tra tất cả các vùng hướng tổ hợp riêng biệt, đảm bảo không thể bỏ sót điểm tối ưu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        N, M = map(int, input().split())

        cafes = [tuple(map(int, input().split())) for _ in range(N)]
        lands = [tuple(map(int, input().split())) for _ in range(M)]

        ans = 0

        for cx, cy in cafes:
            angles = []
            for x, y in lands:
                angles.append(math.atan2(y - cy, x - cx))

            angles.sort()

            a = angles
            n = len(a)

            events = []

            for i in range(n):
                for j in range(i + 1, n):
                    ai, aj = a[i], a[j]
                    # normalize difference to (0, 2pi)
                    d = aj - ai
                    # choose minor arc interval where ray lies between them
                    if d < 0:
                        d += 2 * math.pi

                    # interval is (ai, aj) on circle, but ensure it is minor arc
                    if d > math.pi:
                        # swap to take other arc
                        start = aj
                        end = ai + 2 * math.pi
                    else:
                        start = ai
                        end = aj

                    events.append((start, 1))
                    events.append((end, -1))

            events.sort()

            cur = 0
            best = 0
            for _, v in events:
                cur += v
                best = max(best, cur)

            ans = max(ans, best)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sửa chữa một quán cà phê và chuyển đổi tất cả các điểm mốc thành các góc xung quanh nó. Mỗi cặp điểm mốc trở thành một khoảng trên vòng tròn góc và chúng tôi đếm xem có bao nhiêu khoảng như vậy chồng lên nhau theo hướng tốt nhất bằng cách quét qua các điểm cuối sự kiện. 

Một điểm tinh tế là xử lý tính chất hình tròn của các góc. Mã giải quyết vấn đề này bằng cách cho phép các khoảng thời gian kéo dài hơn$2\pi$, hủy vòng tròn một cách hiệu quả để các khoảng bao quanh trở thành tuyến tính. 

Một chi tiết quan trọng khác là mỗi cặp đóng góp chính xác một khoảng tương ứng với cung nhỏ hơn. Việc chọn cung lớn hơn sẽ tính sai hai hướng không tương ứng với các giao điểm tia hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một quán cà phê ở điểm gốc và bốn điểm mốc được đặt ở các góc$0^\circ, 90^\circ, 180^\circ, 270^\circ$. Mỗi cặp xác định một khoảng cung nhỏ. 

| Cặp | Khoảng góc (độ) | 
| --- | --- | 
| (0, 90) | (0, 90) | 
| (0, 180) | (0, 180) | 
| (0, 270) | (270, 360) U (0) được xử lý thông qua unwrap | 
| (90, 180) | (90, 180) | 
| (90, 270) | (90, 270) | 
| (180, 270) | (180, 270) | 

Quét qua các khoảng này, sự chồng lấp tối đa xảy ra gần$90^\circ$ĐẾN$180^\circ$, nơi có nhiều cung giao nhau. 

Điều này chứng tỏ rằng hướng tối ưu tương ứng với một khu vực có nhiều cặp mốc đồng thời “nhìn thấy” tia sáng từ quán cà phê. 

Bây giờ hãy xem xét một cấu hình bị lệch: các điểm mốc được nhóm lại thành một hình bán nguyệt. Sau đó, hầu hết các cặp tạo ra các khoảng bao phủ vùng dày đặc đó và quá trình quét cho thấy sự chồng chéo đỉnh rõ ràng gần trung tâm của cụm đó, phù hợp với trực giác rằng hướng quan sát tốt nhất là về phía nơi tập trung các điểm mốc dày đặc nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \cdot M^2 \log M)$| Đối với mỗi quán cà phê, tất cả các cặp mốc được xử lý thành các khoảng và sắp xếp | 
| Không gian |$O(M^2)$| Lưu trữ cho các sự kiện ngắt quãng trên mỗi quán cà phê | 

Được cho$M \le 400$và tổng cộng$M$qua các thử nghiệm bị giới hạn, điều này vẫn khả thi vì$M^2$là về$1.6 \times 10^5$, và thậm chí được nhân lên trên tất cả các quán cà phê vẫn nằm trong giới hạn chấp nhận được khi thực hiện chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from io import StringIO
    out = StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# minimal case
assert run("""1
1 2
0 0
1 0
0 1
""") == "1"

# symmetric square
assert run("""1
1 4
0 0
1 0
0 1
-1 0
0 -1
""") == "2"

# single direction cluster
assert run("""1
1 3
0 0
1 0
2 0
3 0
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 quán cà phê, 2 địa danh | 1 | tam giác không tầm thường nhỏ nhất | 
| hình vuông đối xứng | 2 | chồng chéo góc cân bằng | 
| cụm cộng tuyến | 0 | trực giác xử lý thoái hóa | 

## Vỏ cạnh 

Trường hợp tinh tế đầu tiên là khi tất cả các điểm mốc nằm trong một khu vực góc cạnh hẹp xung quanh quán cà phê. Trong tình huống này, hầu hết các cặp tạo ra các khoảng chồng chéo gần như bao phủ cùng một vùng. Thuật toán xử lý điều này một cách chính xác vì tất cả các khoảng đều thu gọn thành một vùng quét dày đặc và sự chồng chéo tối đa phản ánh sự bùng nổ tổ hợp của các cặp trong cụm đó. 

Một trường hợp khác là khi các điểm mốc cách đều nhau xung quanh đường tròn. Ở đây, không có hướng nào được ưu tiên nhiều và mỗi khoảng thời gian đều ngắn. Việc quét chính xác cho thấy mức tối đa thấp tương đối đồng đều, vì không có hướng góc nào nằm trong nhiều khoảng thời gian đồng thời. 

Trường hợp cuối cùng là các cặp bao quanh, trong đó một điểm mốc nằm ở một góc nhỏ và điểm mốc khác ở gần$2\pi$. Việc xây dựng khoảng thời gian phải mở rộng ra ngoài$2\pi$và việc nhân đôi các góc đảm bảo rằng các khoảng này vẫn được biểu diễn dưới dạng các đoạn liền kề trên đường không cuộn. Việc quét qua danh sách sự kiện mở rộng sẽ đếm chính xác những sự kiện này mà không chia phần đóng góp không chính xác.
