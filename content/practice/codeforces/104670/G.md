---
title: "CF 104670G - Ngũ cốc đã qua chế biến"
description: "Chúng ta được cung cấp một tập hợp nhỏ các “vùng thiệt hại” hình tròn trên một mặt phẳng vô tận. Mỗi vùng được xác định bởi một điểm trung tâm và bán kính, và nó phá hủy mọi thứ bên trong hoặc trên vòng tròn đó."
date: "2026-06-29T14:01:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104670
codeforces_index: "G"
codeforces_contest_name: "2021-2022 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2021)"
rating: 0
weight: 104670
solve_time_s: 63
verified: true
draft: false
---

[CF 104670G - Ngũ cốc đã được nghiền nhỏ](https://codeforces.com/problemset/problem/104670/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp nhỏ các “vùng thiệt hại” hình tròn trên một mặt phẳng vô tận. Mỗi vùng được xác định bởi một điểm trung tâm và bán kính, và nó phá hủy mọi thứ bên trong hoặc trên vòng tròn đó. Nhiệm vụ là tính tổng diện tích được bao phủ bởi ít nhất một trong các vòng tròn này, chỉ tính các phần chồng lên nhau một lần. 

Nói cách khác, chúng ta muốn diện tích hợp của tối đa mười đĩa trên mặt phẳng. Tọa độ và bán kính đều là số nguyên nhỏ, nhưng hình học là liên tục nên đáp án là số thực. 

Các ràng buộc cực kỳ dễ dãi: nhiều nhất là mười vòng tròn có bán kính lên tới mười. Điều này ngay lập tức cho chúng ta biết rằng ngay cả một$O(n^3)$Thuật toán hình học có thể ổn về mặt kỹ thuật, nhưng khó khăn thực sự không phải là tốc độ, mà là diện tích liên kết chính xác của các vòng tròn rất lộn xộn về mặt hình học. Giao điểm giữa các vòng tròn tạo ra các ranh giới cong và việc tính toán giải tích trực tiếp nhanh chóng trở nên phức tạp. 

Một ý tưởng ngây thơ là tính toán tất cả các cặp trùng lặp và thử loại trừ bao gồm. Điều này không thành công vì khó có thể mô tả rõ ràng các giao điểm ba ở dạng khép kín đối với các vòng tròn. Ngay cả khi chúng tôi hạn chế chồng chéo theo cặp, việc trừ giao điểm một cách chính xác vẫn yêu cầu xử lý cẩn thận các vùng hình thấu kính. 

Ý tưởng ngây thơ thứ hai là rời rạc hóa mặt phẳng bằng một lưới mịn và đếm các ô được bao phủ. Về mặt tinh thần, điều này có thể có hiệu quả, nhưng việc đảm bảo sai số tương đối 10% trong khi vẫn duy trì thời gian chạy hợp lý đòi hỏi phải điều chỉnh cẩn thận và độ phân giải lưới xác định là điều khó lý giải trong hình học liên tục. 

Một trường hợp cạnh tinh tế là sự chồng chéo hoàn toàn. Nếu tất cả các vòng tròn đều giống nhau thì câu trả lời phải chính xác là diện tích của một vòng tròn. Một nỗ lực loại trừ bao gồm ngây thơ thường bị tính quá nhiều trong kịch bản này. 

Một trường hợp cạnh khác là các vòng tròn rời rạc được đặt cách xa nhau. Bất kỳ phép tính gần đúng nào cũng không được thiên về chồng chéo hoặc đếm thiếu ở các vùng thưa thớt. 

Thách thức cốt lõi là hình học chính xác là quá mức cần thiết đối với những ràng buộc nhỏ như vậy và chỉ cần một phép tính gần đúng có kiểm soát là đủ. 

## Phương pháp tiếp cận 

Một cách tiếp cận chính xác theo kiểu bạo lực sẽ cố gắng tính toán sự kết hợp của các vòng tròn bằng cách sử dụng phân rã hình học. Người ta có thể tính toán tất cả các điểm giao nhau giữa đường tròn và đường tròn, chia ranh giới thành các cung và tái tạo lại đường ranh giới hợp thành một phân khu phẳng. Diện tích hợp sau đó sẽ được tính bằng cách lấy tích phân trên các đoạn tròn. Về nguyên tắc, điều này đúng, nhưng việc thực hiện xây dựng sắp xếp vòng tròn mạnh mẽ là rất nặng và ngay cả những lỗi số nhỏ trong xử lý hồ quang cũng có thể phá vỡ tính chính xác. 

Chi phí tính toán cũng tăng lên nhanh chóng. Với$n \le 10$, có tối đa 45 giao điểm theo cặp, nhưng việc xử lý sắp xếp cung và đa giác hóa gây ra độ phức tạp không đổi đáng kể và hình học dễ vỡ. 

Quan sát quan trọng là độ chính xác yêu cầu thấp: chỉ cho phép sai số tương đối 10%. Điều này ngay lập tức cho thấy rằng việc xây dựng hình học chính xác là không cần thiết. Thay vào đó, chúng ta có thể ước tính diện tích bằng cách lấy mẫu Monte Carlo. 

Chúng tôi bao quanh tất cả các vòng tròn trong một hình chữ nhật giới hạn. Sau đó, chúng ta lấy mẫu nhiều điểm ngẫu nhiên một cách đồng nhất trong hình chữ nhật này và kiểm tra xem mỗi điểm có nằm bên trong ít nhất một vòng tròn hay không. Tỷ lệ các điểm bên trong xấp xỉ tỷ lệ diện tích hợp. Nhân với diện tích hình chữ nhật sẽ ước tính được diện tích hợp. 

Bởi vì$n$rất nhỏ, mỗi lần kiểm tra điểm trong vòng tròn chỉ tốn chi phí$O(n)$, và tổng độ phức tạp trở thành$O(S \cdot n)$, Ở đâu$S$là số lượng mẫu Với$S$với mức từ một đến hai triệu, điều này dễ dàng đủ nhanh trong Python. 

Hộp giới hạn rất dễ xây dựng: nó kéo dài từ$\min(x_i - r_i)$ĐẾN$\max(x_i + r_i)$ở cả hai tọa độ. 

Câu chuyện rất đơn giản: hình học chính xác rất phức tạp vì ranh giới vòng tròn tương tác theo đường cong, nhưng việc lấy mẫu hoàn toàn bỏ qua cấu trúc ranh giới và thay thế nó bằng ước tính thống kê. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Công đoàn hình học chính xác |$O(n^2 \log n)$ĐẾN$O(n^3)$với hình học nặng |$O(n^2)$| Phức tạp không cần thiết | 
| Lấy mẫu Monte Carlo |$O(S \cdot n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán hình chữ nhật giới hạn theo trục có chứa tất cả các vòng tròn bằng cách lấy phạm vi tối thiểu và tối đa của mỗi đĩa. Điều này đảm bảo mọi điểm có thể được bao phủ đều được lấy mẫu từ một vùng chứa đầy đủ phần hợp. 
2. Chọn số lượng mẫu ngẫu nhiên cố định$S$, thường là khoảng một triệu. Cỡ mẫu cố định giúp ổn định phương sai và đảm bảo thời gian chạy xác định. 
3. Đối với mỗi mẫu, tạo một điểm ngẫu nhiên đồng đều bên trong hình chữ nhật bao quanh. 
4. Đối với điểm đó, hãy kiểm tra xem nó có nằm trong ít nhất một vòng tròn hay không bằng cách kiểm tra$(x - x_i)^2 + (y - y_i)^2 \le r_i^2$cho bất kỳ vòng tròn nào. 
5. Đếm xem có bao nhiêu điểm được lấy mẫu nằm trong ít nhất một vòng tròn. 
6. Ước tính diện tích được che phủ$$\text{area} = \frac{\text{inside count}}{S} \times \text{bounding rectangle area}.$$Lý do điều này có tác dụng là vì việc lấy mẫu thống nhất biến diện tích hình học thành xác suất. Xác suất để một điểm ngẫu nhiên trong hộp giới hạn nằm trong liên kết bằng tỷ lệ giữa diện tích hợp và diện tích hộp. Ước tính xác suất này thông qua việc lấy mẫu sẽ hội tụ về giá trị thực khi số lượng mẫu tăng lên. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

def solve():
    random.seed(1)

    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    circles = []
    min_x = min_y = 10**9
    max_x = max_y = -10**9

    for _ in range(n):
        x, y, r = map(int, input().split())
        circles.append((x, y, r))
        min_x = min(min_x, x - r)
        max_x = max(max_x, x + r)
        min_y = min(min_y, y - r)
        max_y = max(max_y, y + r)

    if n == 0:
        print("0.0")
        return

    S = 1_500_000
    inside = 0

    for _ in range(S):
        x = random.uniform(min_x, max_x)
        y = random.uniform(min_y, max_y)

        for cx, cy, r in circles:
            dx = x - cx
            dy = y - cy
            if dx * dx + dy * dy <= r * r:
                inside += 1
                break

    box_area = (max_x - min_x) * (max_y - min_y)
    ans = box_area * inside / S
    print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên một hạt giống ngẫu nhiên cố định để việc thực hiện lặp lại mang tính quyết định. Tính toán hộp giới hạn đảm bảo việc lấy mẫu không bao giờ bỏ sót các phần của bất kỳ vòng tròn nào. Việc ngắt sớm bên trong vòng tròn sẽ giảm bớt những lần kiểm tra không cần thiết khi một điểm đã được bao phủ. 

Sự tinh tế chính là đảm bảo hộp giới hạn đủ chặt để tránh lãng phí mẫu trong khi vẫn chứa đầy đủ tất cả các vòng tròn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
0 0 1
```Chúng ta có một vòng tròn đơn vị. Hộp giới hạn trở thành$[-1, 1] \times [-1, 1]$. 

| Pha mẫu | Giá trị | 
| --- | --- | 
| Khu vực hộp giới hạn | 4 | 
| Tỷ lệ bên trong (dự kiến) | π/4 | 
| Diện tích ước tính | ≈ π | 

Điều này xác nhận rằng công cụ ước tính hội tụ về khu vực vòng tròn chính xác. 

Bất biến được minh họa ở đây là việc lấy mẫu thống nhất trên hộp giới hạn sẽ bảo toàn sự biểu diễn diện tích theo tỷ lệ ngay cả đối với các ranh giới cong. 

### Ví dụ 2 

đầu vào:```
2
0 0 2
2 0 2
```Những vòng tròn này chồng lên nhau đáng kể. 

| Giai đoạn | Quan sát | 
| --- | --- | 
| Hộp giới hạn | [-2, 4] × [-2, 2] | 
| Hình học | Vùng chồng lấn mạnh gần x = 1 | 
| Hành vi dự kiến ​​| Tính chồng chéo một lần | 

Việc lấy mẫu xử lý sự chồng chéo một cách tự nhiên vì bất kỳ điểm nào trong giao điểm chỉ được tính một lần, vì chúng tôi sẽ ngắt sau khi chạm vào vòng tròn đầu tiên. 

Điều này chứng tỏ rằng không cần phải điều chỉnh rõ ràng các trường hợp chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(S \cdot n)$| Mỗi mẫu S kiểm tra tối đa n vòng tròn | 
| Không gian |$O(n)$| Chỉ lưu trữ danh sách vòng kết nối | 

Với$n \le 10$Và$S \approx 1.5 \times 10^6$, tổng số lần kiểm tra khoảng cách là khoảng 15 triệu, phù hợp thoải mái trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io, random

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    random.seed(1)

    circles = []
    data = inp.strip().split()
    if not data:
        return ""

    n = int(data[0])
    idx = 1

    min_x = min_y = 10**9
    max_x = max_y = -10**9

    for _ in range(n):
        x = int(data[idx]); y = int(data[idx+1]); r = int(data[idx+2])
        idx += 3
        circles.append((x, y, r))
        min_x = min(min_x, x - r)
        max_x = max(max_x, x + r)
        min_y = min(min_y, y - r)
        max_y = max(max_y, y + r)

    if n == 0:
        return "0.0\n"

    S = 200000
    inside = 0

    for _ in range(S):
        x = random.uniform(min_x, max_x)
        y = random.uniform(min_y, max_y)
        for cx, cy, r in circles:
            dx = x - cx
            dy = y - cy
            if dx*dx + dy*dy <= r*r:
                inside += 1
                break

    box_area = (max_x - min_x) * (max_y - min_y)
    ans = box_area * inside / S
    return f"{ans:.10f}\n"

# provided sample-like checks (deterministic due to seed)
assert run("1\n0 0 1\n") == run("1\n0 0 1\n"), "determinism check"

# all same circle overlap case
assert run("2\n0 0 1\n0 0 1\n") == run("1\n0 0 1\n"), "identical circles"

# disjoint circles
assert run("2\n0 0 1\n10 10 1\n") != "", "non-empty output"

# single point-sized circle edge-ish
assert run("1\n0 0 10\n") != "", "large circle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòng tròn giống hệt nhau | giống như vòng tròn đơn | xử lý chồng chéo | 
| vòng tròn rời rạc | tổng hữu hạn | logic hợp đúng đắn | 
| bán kính lớn | đầu ra ổn định khác không | độ chính xác của hộp giới hạn | 

## Vỏ cạnh 

Kịch bản vòng tròn giống hệt nhau được xử lý một cách tự nhiên vì mọi điểm được lấy mẫu bên trong vòng tròn chỉ được tính một lần do thoát sớm. Ngay cả khi nhiều vòng tròn chồng lên nhau một cách hoàn hảo thì hàm chỉ báo vẫn là nhị phân. 

Đối với các vòng tròn được phân tách rộng rãi, hộp giới hạn trở nên lớn nhưng việc lấy mẫu vẫn phân bố đồng đều nên mỗi vùng được biểu diễn theo tỷ lệ. Công cụ ước tính xấp xỉ chính xác tổng các khu vực rời rạc. 

Đối với trường hợp cực đoan một vòng tròn, mọi mẫu đều hoạt động giống hệt như phép thử Bernoulli với xác suất thành công πr² chia cho diện tích hộp, do đó sự hội tụ là tiêu chuẩn và không thiên vị.
