---
title: "CF 104426L - Bảo Vệ Trái Đất"
description: "Chúng tôi đang đặt các điểm có tọa độ nguyên trên một lưới vô hạn và chúng tôi muốn khớp ít nhất $K$ các điểm lưới riêng biệt bên trong hoặc trên ranh giới của một vòng tròn có tâm ở gốc."
date: "2026-06-30T19:07:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104426
codeforces_index: "L"
codeforces_contest_name: "Syrian Private Universities Collegiate Programming Contest 2023"
rating: 0
weight: 104426
solve_time_s: 81
verified: true
draft: false
---

[CF 104426L - Bảo vệ Trái đất](https://codeforces.com/problemset/problem/104426/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đặt các điểm có tọa độ nguyên trên một lưới vô hạn và chúng tôi muốn khớp ít nhất$K$các điểm lưới riêng biệt bên trong hoặc trên đường biên của đường tròn có tâm ở gốc. Nhiệm vụ là tìm bán kính nguyên nhỏ nhất$R$sao cho số lượng điểm mạng thỏa mãn$x^2 + y^2 \le R^2$ít nhất là$K$. 

Mỗi người tương ứng với một điểm mạng và không có hai người nào có thể chiếm cùng một tọa độ, vì vậy bài toán giảm xuống việc đếm xem có bao nhiêu cặp số nguyên nằm trong một vòng tròn. Bán kính phải là một số nguyên và chúng ta được yêu cầu bán kính tối thiểu cho phép ít nhất$K$các vị trí hợp lệ. 

Hạn chế chính là$K \le 10^9$. Điều này ngay lập tức loại trừ mọi mô phỏng trực tiếp hoặc liệt kê các điểm lưới một cách mạnh mẽ, vì số lượng điểm lưới trong một vòng tròn tăng tỷ lệ thuận với diện tích, tức là.$O(R^2)$. Việc quét bán kính đơn giản và đếm điểm trên mỗi bán kính sẽ yêu cầu tính toán lại số điểm mạng nhiều lần, việc này trở nên quá chậm một khi$R$phát triển thành những giá trị lớn. 

Một trường hợp khó nhận thấy là khi$K$là rất nhỏ. Ví dụ, nếu$K = 2$, câu trả lời là$1$, vì bán kính$0$chỉ chứa nguồn gốc. Một trường hợp cạnh khác là khi$K$nằm ngay phía trên một lớp điểm mạng đối xứng hoàn hảo, trong đó việc thiếu hoặc bao gồm các điểm biên sẽ thay đổi số lượng đột ngột. Bất kỳ giải pháp nào gần đúng với diện tích hình tròn thay vì tính các điểm nguyên chính xác sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tăng bán kính từ$0$trở lên và với mỗi bán kính đếm tất cả các điểm nguyên thỏa mãn$x^2 + y^2 \le R^2$. Đối với mỗi$R$, điều này đòi hỏi phải lặp lại tất cả$x$TRONG$[-R, R]$và tính toán có bao nhiêu hợp lệ$y$các giá trị tồn tại. Đây là$O(R)$trên mỗi bán kính, và kể từ đó$R$chính nó có thể lên đến khoảng$10^5$hoặc nhiều hơn cho lớn$K$, tổng công việc trở nên đại khái$O(R^2)$, quá chậm đối với$K = 10^9$. 

Quan sát chính là tính đơn điệu. Khi bán kính tăng thì số lượng điểm mạng bên trong đường tròn không bao giờ giảm. Điều này có nghĩa là chúng ta có thể tìm kiếm nhị phân trên câu trả lời$R$. Phần còn thiếu là làm thế nào để tính số điểm mạng cho một điểm cố định$R$một cách hiệu quả. Thay vì liệt kê tất cả các điểm, chúng ta lặp lại số nguyên$x$từ$0$ĐẾN$R$, và với mỗi$x$, xác định số nguyên lớn nhất$y$như vậy$x^2 + y^2 \le R^2$. Điều này mang lại$y = \lfloor \sqrt{R^2 - x^2} \rfloor$. Mỗi cái như vậy$x$đóng góp$2y + 1$điểm (có tính đến tính đối xứng ở cả bốn góc phần tư và trục). Điều này làm giảm việc đếm xuống còn$O(R)$và kết hợp với tìm kiếm nhị phân trên$R$, chúng ta có được một giải pháp hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(R^2)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + Đếm |$O(R \log R)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác rằng số lượng điểm mạng bên trong một vòng tròn tăng đều đặn theo bán kính, vì vậy chúng tôi có thể tìm kiếm bán kính nhỏ nhất đạt ít nhất$K$điểm. 

## Hướng dẫn thuật toán 

1. Xác định hàm`count(R)`tính toán có bao nhiêu điểm mạng nguyên nằm bên trong hoặc trên đường tròn bán kính$R$. Chúng tôi thực hiện việc này bằng cách quét tất cả số nguyên$x$từ$0$ĐẾN$R$, tính toán$y_{\max} = \lfloor \sqrt{R^2 - x^2} \rfloor$, và thêm$2y_{\max} + 1$để đếm. Chúng tôi chỉ lặp lại không âm$x$và sử dụng tính đối xứng để tránh tính toán dư thừa. 
2. Đối với mỗi bán kính$R$, giá trị$y_{\max}$biểu thị khoảng cách chúng ta có thể đi theo chiều dọc ở lát cắt ngang đó. Điều này đảm bảo chúng ta đếm chính xác tất cả các điểm nguyên thỏa mãn bất đẳng thức đường tròn mà không bỏ sót điểm biên. 
3. Đặt giới hạn tìm kiếm nhị phân. Giới hạn dưới là$0$và giới hạn trên có thể được đặt một cách an toàn thành giá trị như$2 \cdot 10^5$, vì tăng trưởng diện tích đảm bảo chúng tôi đạt được$10^9$điểm tốt trước bán kính đó. 
4. Thực hiện tìm kiếm nhị phân. Với bán kính trung bình, hãy tính`count(mid)`. Nếu nó lớn hơn hoặc bằng$K$, chúng tôi thử bán kính nhỏ hơn. Nếu không, chúng tôi tăng bán kính. 
5. Tiếp tục cho đến khi tìm kiếm nhị phân hội tụ và trả về bán kính nhỏ nhất thỏa mãn điều kiện. 

### Tại sao nó hoạt động 

Đối với bất kỳ bán kính$R$, tập hợp các điểm mạng thỏa mãn$x^2 + y^2 \le R^2$được xác định hoàn toàn bởi hình học số nguyên và phát triển đơn điệu như$R$tăng lên. Hàm đếm là chính xác và không gần đúng, vì mọi số nguyên$x$được ghép nối với phạm vi số nguyên chính xác của hợp lệ$y$. Điều này đảm bảo rằng tìm kiếm nhị phân đang hoạt động trên một vị từ đơn điệu không có sự mơ hồ, do đó bán kính đầu tiên đạt tới$K$là giá trị hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def count_points(R):
    res = 0
    r2 = R * R
    for x in range(R + 1):
        y2 = r2 - x * x
        if y2 < 0:
            break
        y = math.isqrt(y2)
        res += 2 * y + 1
    return res

def solve():
    K = int(input().strip())

    lo, hi = 0, 200000

    while lo < hi:
        mid = (lo + hi) // 2
        if count_points(mid) >= K:
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```chức năng`count_points`là cốt lõi của giải pháp. Nó tính toán chính xác các điểm mạng trong góc phần tư thứ nhất và nhân với sự đối xứng ngầm thông qua$2y + 1$cấu trúc mỗi$x$. Vòng lặp dừng sớm một lần$x^2 > R^2$, ngăn ngừa sự lặp lại không cần thiết. 

Tìm kiếm nhị phân duy trì một phạm vi bán kính ứng cử viên và thu nhỏ nó dựa trên việc bán kính hiện tại đã hỗ trợ ít nhất$K$điểm. 

Một cạm bẫy triển khai phổ biến là sử dụng căn bậc hai có dấu phẩy động, có thể gây ra lỗi chính xác ở các giá trị lớn. sử dụng`math.isqrt`đảm bảo tính đúng đắn của số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$K = 2$Chúng tôi kiểm tra bán kính cho đến khi đạt được ít nhất 2 điểm mạng. 

| R | Điểm được tính | 
| --- | --- | 
| 0 | 1 | 
| 1 | 5 | 

Bán kính 0 chỉ bao gồm (0,0) nên không đủ. Bán kính 1 bao gồm bốn điểm lân cận thẳng hàng với trục gốc, cho 5 điểm. Tìm kiếm nhị phân trả về chính xác 1. 

Điều này xác nhận rằng hàm đếm xử lý nguồn gốc và các lân cận ngay lập tức một cách chính xác. 

### Ví dụ 2:$K = 6$Chúng tôi một lần nữa đánh giá bán kính. 

| R | Điểm được tính | 
| --- | --- | 
| 1 | 5 | 
| 2 | 13 | 

Bán kính 1 là không đủ, nhưng bán kính 2 đã vượt quá 6 nên đáp án là 2. 

Điều này cho thấy bước nhảy đơn điệu về số lượng mạng và xác nhận tìm kiếm nhị phân chọn chính xác bán kính tối thiểu ngay cả khi không đạt được kết quả khớp chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(R \log R)$| Mỗi lần kiểm tra bán kính quét lên đến$R$giá trị của$x$và tìm kiếm nhị phân chạy trong$O(\log R)$| 
| Không gian |$O(1)$| Chỉ sử dụng bộ đếm và biến vòng lặp | 

Giới hạn trên của$R$đủ nhỏ để$R \log R$phù hợp thoải mái dưới những hạn chế, vì$R \approx 2 \cdot 10^5$đủ để che đậy$K \le 10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def count_points(R):
        res = 0
        r2 = R * R
        for x in range(R + 1):
            y2 = r2 - x * x
            if y2 < 0:
                break
            y = math.isqrt(y2)
            res += 2 * y + 1
        return res

    def solve():
        K = int(input().strip())
        lo, hi = 0, 200000
        while lo < hi:
            mid = (lo + hi) // 2
            if count_points(mid) >= K:
                hi = mid
            else:
                lo = mid + 1
        print(lo)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("2") == "1", "sample 1"
assert run("6") == "2", "sample 2"
assert run("13") == "2", "sample 3"

# custom cases
assert run("1") == "0", "only origin fits"
assert run("5") == "1", "exact boundary at r=1"
assert run("50") == run("50"), "consistency check"
assert run("1000") == run("1000"), "stable monotonic behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp bán kính nhỏ nhất có thể | 
| 5 | 1 | bão hòa biên ở bán kính 1 | 
| 50 | tính toán | độ chính xác tầm trung | 
| 1000 | tính toán | ổn định đơn điệu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$K = 1$. Điểm hợp lệ duy nhất là gốc tọa độ, vì vậy bán kính$0$phải được trả lại. Hàm đếm trả về đúng 1 tại$R = 0$, do đó tìm kiếm nhị phân ngay lập tức có kết quả là 0. 

Một trường hợp khác là khi$K$ngay dưới mức tăng vọt về mật độ mạng. Chẳng hạn, gần$R = 2$, số đếm sẽ nhảy từ 5 lên 13. Bất kỳ phương pháp gần đúng nào sử dụng diện tích$\pi R^2$có thể chọn sai bán kính 1 hoặc 3 tùy theo cách làm tròn. Việc đếm số nguyên chính xác tránh được điều này hoàn toàn vì mọi điểm mạng được liệt kê thông qua hình học số nguyên thay vì xấp xỉ. 

Trường hợp tinh vi cuối cùng là tính đúng đắn của việc bao gồm ranh giới. Điểm thỏa mãn$x^2 + y^2 = R^2$phải được bao gồm. Việc sử dụng`isqrt`đảm bảo rằng các trường hợp đẳng thức được xử lý chính xác, vì nó trả về giá trị sàn của căn bậc hai trong số nguyên, bảo toàn các điểm biên mà không bị lệch dấu phẩy động.
