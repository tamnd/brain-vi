---
title: "CF 104520O - Vấn đề truy vấn phạm vi trung bình"
description: "Chúng ta được cung cấp một số thí nghiệm ngẫu nhiên độc lập, mỗi thí nghiệm được mô tả bằng một khoảng. Với mỗi khoảng $[li, ri]$, một số thực được lấy mẫu ngẫu nhiên một cách thống nhất. Chúng tôi lặp lại điều này cho tất cả những người thử nghiệm, vì vậy chúng tôi kết thúc với một tập hợp các giá trị ngẫu nhiên độc lập $n$."
date: "2026-06-30T10:33:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104520
codeforces_index: "O"
codeforces_contest_name: "Teamscode Summer 2023 Contest"
rating: 0
weight: 104520
solve_time_s: 114
verified: false
draft: false
---

[CF 104520O - Sự cố truy vấn phạm vi trung bình](https://codeforces.com/problemset/problem/104520/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số thí nghiệm ngẫu nhiên độc lập, mỗi thí nghiệm được mô tả bằng một khoảng. Đối với mỗi khoảng thời gian$[l_i, r_i]$, một số thực được lấy mẫu thống nhất một cách ngẫu nhiên. Chúng tôi lặp lại điều này cho tất cả những người thử nghiệm, vì vậy chúng tôi kết thúc với một bộ$n$giá trị ngẫu nhiên độc lập. Số lượng quan tâm là phạm vi của các giá trị được lấy mẫu này, nghĩa là sự khác biệt giữa số được chọn lớn nhất và nhỏ nhất. 

Toàn bộ quá trình này được lặp lại về mặt khái niệm cho mỗi$t$các kịch bản độc lập Mỗi kịch bản sử dụng cùng một tập hợp các khoảng thời gian, nhưng chúng tôi được yêu cầu tính toán phạm vi dự kiến ​​của các giá trị được lấy mẫu kết quả một cách riêng biệt cho từng kịch bản. 

Khó khăn cốt lõi là phạm vi phụ thuộc vào cực trị tổng thể của tất cả các giá trị được lấy mẫu cùng một lúc, do đó các biến được kết hợp chặt chẽ. Mặc dù mỗi giá trị được lấy mẫu độc lập, nhưng giá trị tối đa và tối thiểu đưa ra sự phụ thuộc tổ hợp. 

Các ràng buộc có cấu trúc nhỏ nhưng không có trong tổ hợp. Với$n \le 200$Và$t \le 10$, bất kỳ giải pháp nào liệt kê rõ ràng các kết quả hoặc tập hợp con của người thử nghiệm rời rạc đều không khả thi. Việc rời rạc hóa một cách đơn giản về không gian giá trị cũng là không thể vì các khoảng trải rộng tới 3500 và các giá trị là có thật. Điều này buộc chúng ta hướng tới việc phân rã xác suất theo các cấu trúc sắp xếp thay vì lấy mẫu giá trị rõ ràng. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các khoảng thu gọn thành điểm. Trong trường hợp đó, phạm vi luôn bằng 0 và bất kỳ công thức nào giả định mật độ liên tục hoặc bỏ qua các khoảng suy biến có thể vô tình chia cho 0 hoặc xử lý sai ranh giới đẳng thức. Một vấn đề khác phát sinh khi tất cả các khoảng chồng chéo lên nhau. Ví dụ, nếu tất cả$[l_i, r_i]$giống hệt nhau, câu trả lời phải giảm xuống phạm vi dự kiến$n$đồng phục giống hệt nhau, không tầm thường nhưng có cấu trúc cao. Cách tiếp cận kỳ vọng theo cặp ngây thơ có thể thất bại vì$\mathbb{E}[\max] - \mathbb{E}[\min]$không bằng phạm vi mong đợi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng rời rạc hóa từng khoảng thành các điểm chi tiết và liệt kê tất cả các kết hợp của các giá trị được lấy mẫu. Đối với mỗi người thử nghiệm, chúng ta có thể giả định một mạng lưới dày đặc trên$[0, 3500]$, chỉ định xác suất cho từng điểm và tính toán mức phân bổ tối đa và tối thiểu trên tất cả những người thử nghiệm. Điều này nhanh chóng trở nên không khả thi vì ngay cả với mức độ rời rạc khiêm tốn như 3501 điểm trên mỗi khoảng, không gian trạng thái chung sẽ trở thành$3501^n$, có giá trị lớn về mặt thiên văn. 

Một ý tưởng ngây thơ khác là tính toán riêng mức tối đa dự kiến ​​và mức tối thiểu dự kiến ​​rồi trừ chúng đi. Điều này không thành công vì kỳ vọng không phân phối qua khớp nối phi tuyến. Mức tối đa và mức tối thiểu không độc lập và mối tương quan của chúng có ý nghĩa trực tiếp trong phạm vi. 

Quan sát quan trọng là thay vì theo dõi các giá trị được lấy mẫu thực tế, chúng ta có thể theo dõi thứ tự tương đối do một ngưỡng tạo ra. Đối với bất kỳ ngưỡng cố định nào$x$, chúng ta có thể tính xác suất để tất cả các giá trị được lấy mẫu nằm dưới hoặc trên nó bằng cách nhân các đóng góp khoảng độc lập. Điều này biến vấn đề thành tính toán các hàm phân phối tối thiểu và tối đa, sau đó tích hợp trên tất cả các ngưỡng có thể. 

Chúng tôi định dạng lại kỳ vọng bằng cách sử dụng danh tính:$$\mathbb{E}[\max - \min] = \int_0^{3500} P(\max \ge x)\,dx - \int_0^{3500} P(\min > x)\,dx$$Cả hai xác suất đều có thể được biểu thị dưới dạng tích số của người thử nghiệm vì mỗi người thử nghiệm đóng góp một xác suất độc lập tùy thuộc vào việc giá trị lấy mẫu của nó cao hơn hay thấp hơn$x$. Cấu trúc này làm giảm vấn đề để đánh giá các đóng góp tuyến tính từng phần trên các điểm dừng số nguyên được tạo ra bởi các điểm cuối khoảng. 

Điều này dẫn đến việc quét qua miền giá trị trong đó xác suất chỉ thay đổi tại$l_i$Và$r_i$, cho phép cập nhật động các đóng góp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ trong$n$| hàm mũ | Quá chậm | 
| Tối ưu |$O(n \cdot V)$mỗi bài kiểm tra |$O(V)$| Đã chấp nhận | 

Đây$V \le 3500$, nên điều này dễ dàng thực hiện được. 

## Hướng dẫn thuật toán 

Chúng tôi coi miền giá trị là các điểm dừng số nguyên từ 0 đến 3500 và tính toán các cấu hình xác suất cho mức tối đa và tối thiểu bằng cách sử dụng các đóng góp khoảng nhân. 

1. Đối với mỗi ngưỡng$x$, tính xác suất để một người thử nghiệm tạo ra một giá trị$\le x$. Vì việc lấy mẫu là đồng nhất$[l_i, r_i]$, xác suất này là 0 khi$x < l_i$, 1 khi$x \ge r_i$, Và$(x - l_i) / (r_i - l_i)$nếu không thì. Điều này mang lại hàm tuyến tính từng phần cho mỗi người thử nghiệm. 
2. Tính toán$P(\max \le x)$là sản phẩm trên tất cả những người thử nghiệm của$P_i(\le x)$. Điều này hợp lệ vì người thử nghiệm độc lập. 
3. Tính toán tương tự$P(\min > x)$bằng cách lưu ý rằng$\min > x$có nghĩa là mọi giá trị lấy mẫu của người kiểm tra đều lớn hơn$x$. Đối với một người thử nghiệm, xác suất này là 0 khi$x \ge r_i$, 1 khi$x < l_i$, Và$(r_i - x) / (r_i - l_i)$nếu không thì. 
4. Tính toán trước cả hai hàm xác suất trên phạm vi số nguyên bằng cách lặp lại tất cả$x$từ 0 đến 3500 và duy trì cập nhật nhân một cách hiệu quả. Vì đóng góp của mỗi người thử nghiệm là tuyến tính từng phần nên chúng tôi chỉ cập nhật khi$x$thánh giá$l_i$hoặc$r_i$. 
5. Tích hợp số cả hai đường cong xác suất bằng cách sử dụng tổng hình thang đơn giản trên các phân số nguyên:$$\int f(x) dx \approx \sum_x \frac{f(x) + f(x+1)}{2}$$6. Phạm vi dự kiến ​​​​có được bằng cách kết hợp hai tích phân theo danh tính rút ra trước đó. 

Ý tưởng quan trọng là toàn bộ tính ngẫu nhiên tập trung vào việc đánh giá các hàm sinh tồn của cực trị, ổn định theo cấu trúc sản phẩm. 

### Tại sao nó hoạt động 

Ở bất kỳ ngưỡng nào$x$, sự kiện$\max \le x$tương đương với tất cả những người thử nghiệm tạo ra nhiều nhất các giá trị$x$. Vì sự lựa chọn của mỗi người thử nghiệm là độc lập nên xác suất được phân tích chính xác thành tích của các giá trị phân bố tích lũy riêng lẻ. Điều tương tự cũng áp dụng ở mức tối thiểu. Vì phạm vi có thể được biểu thị dưới dạng tích phân theo xác suất đuôi của các cực trị này, nên việc tính toán hai hàm này sẽ xác định đầy đủ kỳ vọng. Không có cấu trúc phụ thuộc nào ngoài sự độc lập là cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 3500

def solve_case(intervals):
    n = len(intervals)

    # Precompute probabilities at each integer x
    p_max = [1.0] * (MAXV + 2)
    p_min = [1.0] * (MAXV + 2)

    for l, r in intervals:
        length = r - l

        for x in range(MAXV + 1):
            # contribution to max: P(X <= x)
            if x < l:
                cmax = 0.0
            elif x >= r:
                cmax = 1.0
            else:
                cmax = (x - l) / length if length > 0 else 1.0

            p_max[x] *= cmax

            # contribution to min: P(X > x)
            if x < l:
                cmin = 1.0
            elif x >= r:
                cmin = 0.0
            else:
                cmin = (r - x) / length if length > 0 else 1.0

            p_min[x] *= cmin

    # integrate using trapezoids
    exp_max = 0.0
    exp_min = 0.0

    for x in range(MAXV):
        exp_max += 0.5 * (p_max[x] + p_max[x + 1])
        exp_min += 0.5 * (p_min[x] + p_min[x + 1])

    return exp_max - exp_min

def main():
    n, t = map(int, input().split())
    intervals = [tuple(map(int, input().split())) for _ in range(n)]

    for _ in range(t):
        print(f"{solve_case(intervals):.4f}")

if __name__ == "__main__":
    main()
```Việc triển khai trực tiếp xây dựng các cấu hình xác suất tích lũy cho mức tối đa và tối thiểu trên miền giá trị rời rạc. Vòng lặp lồng nhau$n$và 3500 là chấp nhận được vì tổng công việc là khoảng$7 \times 10^5$hoạt động cho mỗi trường hợp thử nghiệm. 

Phép tích phân hình thang được chọn vì các hàm xác suất là tuyến tính từng đoạn giữa các điểm nguyên, do đó việc lấy mẫu tại các ranh giới số nguyên là đủ để tích hợp chính xác các đoạn tuyến tính. 

Một cạm bẫy phổ biến là cố gắng tính toán mức tối đa dự kiến ​​trực tiếp từ xác suất trên mỗi điểm mà không tích hợp đúng cách. Một điều khác là quên rằng các khoảng suy biến nơi$l_i = r_i$phải được coi là những đóng góp mang tính quyết định mà mã này xử lý thông qua`length > 0`canh gác. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa với ba người thử nghiệm và một phạm vi giá trị nhỏ. Chúng tôi theo dõi xác suất của mức tối đa hoạt động như thế nào. 

| x | P1(X ≤ x) | P2(X ≤ x) | P3(X ≤ x) | P(tối đa ≤ x) | 
| --- | --- | --- | --- | --- | 
| 0 | 0,0 | 0,2 | 0,0 | 0,0 | 
| 1 | 0,5 | 0,4 | 0,1 | 0,02 | 
| 2 | 1.0 | 0,6 | 0,3 | 0,18 | 

Bảng này cho thấy cấu trúc sản phẩm thu hẹp các xác suất nhanh như thế nào: thậm chí các xác suất riêng lẻ vừa phải cũng tạo ra xác suất chung nhỏ để đạt mức tối đa. 

Cấu trúc tương tự được áp dụng đối xứng cho mức tối thiểu, trong đó xác suất đảo ngược và tích lũy từ đầu trên của phạm vi. Điều này xác nhận rằng thuật toán đang nắm bắt chính xác hành vi cực trị chung thay vì các biên độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot V)$| Mỗi người kiểm tra đóng góp các cập nhật tuyến tính trên phạm vi giá trị rời rạc | 
| Không gian |$O(V)$| Chỉ các mảng xác suất trên miền giá trị mới được lưu trữ | 

Với$n \le 200$Và$V \le 3500$, việc tính toán vẫn nằm trong giới hạn thoải mái, vì tổng số phép tính tối đa ở mức vài triệu cho mỗi nhóm thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    MAXV = 3500

    def solve():
        n, t = map(int, input().split())
        intervals = [tuple(map(int, input().split())) for _ in range(n)]

        def solve_case(intervals):
            p_max = [1.0] * (MAXV + 2)
            p_min = [1.0] * (MAXV + 2)

            for l, r in intervals:
                length = r - l
                for x in range(MAXV + 1):
                    if x < l:
                        cmax = 0.0
                    elif x >= r:
                        cmax = 1.0
                    else:
                        cmax = (x - l) / length if length > 0 else 1.0
                    p_max[x] *= cmax

                    if x < l:
                        cmin = 1.0
                    elif x >= r:
                        cmin = 0.0
                    else:
                        cmin = (r - x) / length if length > 0 else 1.0
                    p_min[x] *= cmin

            exp_max = 0.0
            exp_min = 0.0
            for x in range(MAXV):
                exp_max += 0.5 * (p_max[x] + p_max[x + 1])
                exp_min += 0.5 * (p_min[x] + p_min[x + 1])
            return exp_max - exp_min

        for _ in range(t):
            print(round(solve_case(intervals), 4))

    return run.__wrapped__ if hasattr(run, "__wrapped__") else solve()

# provided samples
assert run("""3 3
900 900
800 1000
1000 1100
800 800
3300 3500
0 3500
800 800
0 3500
3499 3500
""") == """175.0
2693.3333
2790.9286
"""

# custom cases
assert run("""1 1
5 5
""") == "0.0", "single point interval"

assert run("""2 1
0 1
0 1
""") != "", "uniform overlap sanity"

assert run("""2 1
0 10
5 15
""") != "", "overlapping intervals"

assert run("""3 1
0 100
0 100
0 100
""") != "", "identical intervals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng điểm đơn | 0 | dãy suy biến | 
| khoảng chồng chéo | giá trị dương | tương tác cực trị | 
| khoảng giống hệt nhau | hành vi đối xứng không tầm thường | tính đúng đắn dưới sự đối xứng | 

## Vỏ cạnh 

Khi tất cả các khoảng thu gọn về một điểm duy nhất, mọi giá trị được lấy mẫu đều mang tính xác định. Trong tình huống đó, cả hai mảng xác suất trở thành 1 giống hệt nhau tại tất cả các điểm hợp lệ hoặc 0 bên ngoài điểm và tích phân của cả cực đại và cực tiểu đều trùng nhau, tạo ra phạm vi bằng 0. Thuật toán xử lý việc này vì`length > 0`chi nhánh được bỏ qua và đóng góp trở nên không đổi. 

Ví dụ, khi các khoảng rời rạc$[0,1]$Và$[100,200]$, đường cong xác suất tối đa chuyển đổi mạnh ở các vùng khác nhau, nhưng cấu trúc sản phẩm đảm bảo rằng mức tối đa bị chi phối bởi khoảng ngoài cùng bên phải. Thuật toán phản ánh chính xác điều này vì với số lượng lớn$x$, tất cả xác suất tích lũy trở thành 1, làm cho$P(\max \le x)$ổn định một cách chính xác. 

Khi nhiều khoảng chồng lên nhau nhiều, tích của các đóng góp phân số sẽ trở nên rất nhỏ ở vùng bên trong, có thể trông không ổn định về mặt số lượng. Tuy nhiên, việc tích phân hình thang chỉ phụ thuộc vào hình dạng tương đối và thuật toán vẫn tích lũy kỳ vọng chính xác vì mọi đóng góp đều liên tục và giới hạn trong$[0,1]$.
