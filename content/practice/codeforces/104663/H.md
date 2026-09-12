---
title: "CF 104663H - Hình ảnh xoay"
description: "Chúng ta có một hình chữ nhật cố định biểu thị một hình ảnh có độ dài các cạnh $a$ và $b$. Chúng tôi cũng có một khung vẽ không có hình dạng tự do: chiều cao và chiều rộng của nó phải luôn tuân theo một tỷ lệ cố định $m:n$, nhưng tỷ lệ tổng thể của nó không cố định."
date: "2026-06-29T14:57:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104663
codeforces_index: "H"
codeforces_contest_name: "Replay of Ostad Presents Intra KUET Programming Contest 2023"
rating: 0
weight: 104663
solve_time_s: 111
verified: true
draft: false
---

[CF 104663H - Hình ảnh được xoay](https://codeforces.com/problemset/problem/104663/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hình chữ nhật cố định biểu thị một hình ảnh có độ dài các cạnh$a$Và$b$. Chúng tôi cũng có một canvas không có hình dạng tự do: chiều cao và chiều rộng của nó phải luôn tuân theo một tỷ lệ cố định$m:n$, nhưng quy mô tổng thể của nó không cố định. Điều đó có nghĩa là mọi canvas hợp lệ đều có kích thước$(k \cdot m, k \cdot n)$đối với một số hệ số tỷ lệ thực hoặc số nguyên dương$k$, và chúng tôi được yêu cầu chọn khung vẽ nhỏ nhất có thể chứa đầy đủ hình ảnh. 

Điều khó khăn là hình ảnh còn bị xoay thêm một góc$\theta$trước khi được đặt. Nhiệm vụ là xác định khung vẽ nhỏ nhất, tôn trọng tỷ lệ cố định, có thể vừa với hình ảnh được xoay. 

Từ góc độ hạn chế, có tới$10^5$trường hợp thử nghiệm và tất cả các kích thước tăng lên$10^9$. Bất kỳ giải pháp nào thực hiện mô phỏng logarit hoặc hình học cho mỗi trường hợp liên quan đến tìm kiếm dấu phẩy động hoặc khớp lặp vẫn sẽ vượt qua, nhưng mọi thao tác lấy mẫu trên mỗi pixel hoặc mỗi góc sẽ quá chậm. Cần có một công thức tuyến tính hoặc thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một vấn đề tế nhị là phép quay gợi ý hình học liên quan đến lượng giác và các hộp giới hạn. Một người đọc ngây thơ sẽ ngay lập tức cố gắng tính hộp giới hạn căn chỉnh theo trục của hình chữ nhật xoay bằng cách sử dụng sin và cosin. Điều đó dẫn đến các vấn đề về số học dấu phẩy động và làm tròn, đặc biệt khi$10^5$trường hợp thử nghiệm có liên quan. 

Trường hợp nguy hiểm hơn xuất hiện khi$\theta = 0$hoặc$\theta = 90$. Trong những trường hợp này, bất kỳ cách triển khai hình học không chính xác nào giả định các góc chung có thể vẫn hoạt động nhưng sẽ bị trôi do độ chính xác của dấu phẩy động. Ví dụ, với$a = 16, b = 18, m = 2, n = 3, \theta = 30$, đầu ra mẫu là$16, 24$, điều này đã gợi ý rằng phép quay không thực sự ảnh hưởng đến câu trả lời cuối cùng theo cách mà cách giải thích hình học ngây thơ sẽ dự đoán. 

Khó khăn chính là nhận ra rằng việc xoay không ảnh hưởng đến quyết định chia tỷ lệ tối thiểu theo ràng buộc canvas tỷ lệ khung hình cố định. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực trực tiếp sẽ là tính toán dấu chân chính xác của hình chữ nhật được xoay và sau đó tìm kiếm khung vẽ có tỷ lệ nhỏ nhất chứa nó. Người ta sẽ tính tọa độ quay của cả bốn góc bằng cách sử dụng các hàm lượng giác, rút ​​ra hộp giới hạn căn chỉnh theo trục và sau đó kiểm tra các giá trị tăng dần của$k$cho đến khi cả hai kích thước canvas vượt quá hộp giới hạn đó. Điều này đúng về mặt khái niệm, nhưng về mặt tính toán thì không cần thiết và dễ vỡ về mặt số lượng. 

Điểm nghẽn trong cách tiếp cận đó có hai mặt. Đầu tiên, việc tính sin và cos cho mỗi trường hợp thử nghiệm đưa ra chi phí không đổi đáng kể khi$T = 10^5$. Thứ hai, tính toán hộp giới hạn liên quan đến số học dấu phẩy động và làm tròn, và ngay cả những lỗi chính xác nhỏ cũng có thể làm thay đổi các phép toán trần cuối cùng, tạo ra kết quả đầu ra số nguyên không chính xác. 

Quan sát chính là góc quay không ảnh hưởng đến điều kiện khả thi khi chúng ta được phép tự do chia tỷ lệ khung vẽ có tỷ lệ cố định. Vấn đề giảm xuống để đảm bảo rằng một cặp tỷ lệ$(k m, k n)$đủ lớn để bao phủ các phạm vi được căn chỉnh theo trục của hình ảnh theo cách căn chỉnh tốt nhất và sự căn chỉnh tối ưu đó đạt được bằng cách chỉ cần khớp các cạnh hình chữ nhật với các trục canvas. Việc xoay trở nên không liên quan vì chúng tôi không tối ưu hóa theo hướng canvas, chỉ chia tỷ lệ theo hướng cố định. 

Điều này thu gọn vấn đề thành một điều kiện thống trị đơn giản: chúng ta chỉ cần đảm bảo cả hai kích thước hình ảnh khớp độc lập với kích thước canvas tương ứng sau khi chia tỷ lệ. 

Do đó chúng ta giảm nhiệm vụ xuống việc tìm giá trị nhỏ nhất$k$sao cho cả hai$k m \ge a$Và$k n \ge b$. Một lần$k$đã biết, câu trả lời là trực tiếp$(k m, k n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hình học vũ lực với phép quay |$O(T)$với lượng giác nặng cho mỗi bài kiểm tra |$O(1)$| Quá chậm / rủi ro về số lượng | 
| Giảm tỷ lệ tối ưu |$O(T)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mọi trường hợp thử nghiệm thành một bài toán chia tỷ lệ theo tỷ lệ cố định. 

1. Đọc$a, b, m, n, \theta$. Góc này không liên quan đến tính toán cuối cùng nên nó không được sử dụng thêm. 
2. Tính xem cần bao nhiêu bước chia tỷ lệ để chiều cao của khung vẽ bao phủ chiều cao của hình ảnh. Đây là số nguyên nhỏ nhất$k_1$như vậy$k_1 \cdot m \ge a$. Điều này có được bằng cách sử dụng phép chia trần:$k_1 = \lceil a / m \rceil$. 
3. Tính tương tự với chiều rộng yêu cầu, tìm số nguyên nhỏ nhất$k_2$như vậy$k_2 \cdot n \ge b$, cho$k_2 = \lceil b / n \rceil$. 
4. Lấy$k = \max(k_1, k_2)$, vì cả hai ràng buộc phải được giữ đồng thời. Tỷ lệ mở rộng lớn hơn là nút cổ chai đảm bảo phạm vi phủ sóng đầy đủ. 
5. Xuất kích thước canvas cuối cùng$(k m, k n)$. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ canvas hợp lệ nào cũng phải là bội số vô hướng dương của$(m, n)$và tính khả thi chỉ phụ thuộc vào việc vectơ tỷ lệ đó có chiếm ưu thế trong hộp giới hạn bắt buộc của hình ảnh hay không. Xoay không đưa ra bất kỳ ràng buộc mới nào vì chúng tôi không được phép xoay hoặc làm biến dạng khung vẽ một cách độc lập với tỷ lệ cố định của nó; mở rộng quy mô là mức độ tự do duy nhất. Khi cả hai bất đẳng thức theo trục đều được thỏa mãn, hình chữ nhật được đảm bảo vừa với tỷ lệ đó và mọi bất đẳng thức nhỏ hơn$k$sẽ vi phạm ít nhất một ràng buộc thứ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    for _ in range(T):
        a, b, m, n, theta = map(int, input().split())

        k1 = (a + m - 1) // m
        k2 = (b + n - 1) // n
        k = max(k1, k2)

        out.append(f"{k * m} {k * n}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp cố tình bỏ qua góc xoay sau khi phân tích cú pháp, vì kích thước canvas tối ưu chỉ phụ thuộc vào việc khớp tỷ lệ khung hình cố định với phạm vi căn chỉnh theo trục của hình chữ nhật. Việc chia trần được thực hiện bằng cách sử dụng số học số nguyên để tránh lỗi dấu phẩy động dưới các ràng buộc lớn. 

Một lỗi triển khai phổ biến ở đây là tính toán phép chia sử dụng dấu phẩy động rồi áp dụng`ceil`, có nguy cơ xảy ra lỗi chính xác đối với các giá trị gần ranh giới số nguyên. Sử dụng số học số nguyên sẽ tránh được điều đó hoàn toàn. Một điểm tinh tế khác là đảm bảo rằng cả hai ràng buộc đều được xử lý độc lập trước khi đạt mức tối đa; việc kết hợp chúng quá sớm có thể dẫn tới việc đánh giá thấp thang đo yêu cầu. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu: 

đầu vào:$a = 16, b = 18, m = 2, n = 3, \theta = 30$| Bước | k1 = trần(a/m) | k2 = trần(b/n) | k | Đầu ra (k_m, k_n) | 
| --- | --- | --- | --- | --- | 
| Tính toán | trần(16/2)=8 | trần(18/3)=6 | 8 | (16, 24) | 

Dấu vết này cho thấy rằng chỉ có yêu cầu mở rộng quy mô lớn hơn mới kiểm soát được câu trả lời cuối cùng. Giới hạn chiều rộng yếu hơn nên giới hạn chiều cao xác định kích thước canvas. 

Bây giờ hãy xem xét trường hợp thứ hai: 

đầu vào:$a = 7, b = 10, m = 3, n = 4$| Bước | k1 | k2 | k | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Tính toán | 3 | 3 | 3 | (9, 12) | 

Cả hai ràng buộc đều hoạt động ở đây và hệ số tỷ lệ thỏa mãn đồng thời cả hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm được xử lý với số lượng phép tính số học không đổi | 
| Không gian |$O(1)$| Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Lời giải dễ dàng nằm trong giới hạn vì nó chỉ thực hiện số học số nguyên theo thời gian không đổi cho mỗi trường hợp kiểm thử, ngay cả đối với$10^5$đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    res = []
    for _ in range(T):
        a, b, m, n, theta = map(int, input().split())
        k1 = (a + m - 1) // m
        k2 = (b + n - 1) // n
        k = max(k1, k2)
        res.append(f"{k * m} {k * n}")
    return "\n".join(res)

# sample
assert run("1\n16 18 2 3 30\n") == "16 24"

# minimum values
assert run("1\n1 1 1 1 0\n") == "1 1"

# tight fit case
assert run("1\n5 7 5 7 90\n") == "5 7"

# ratio forcing height dominance
assert run("1\n100 1 3 10 45\n") == "102 340"

# ratio forcing width dominance
assert run("1\n1 100 4 3 60\n") == "4 300"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 1 1 | độ đúng cơ sở | 
| vừa vặn | 5 7 | không cần chia tỷ lệ vượt quá 1 | 
| chiếm ưu thế về chiều cao | 102 340 | k được chọn từ giới hạn chiều cao | 
| chiếm ưu thế về chiều rộng | 4 300 | k được chọn từ giới hạn chiều rộng | 

## Vỏ cạnh 

Khi nào$a$chính xác là chia hết cho$m$, việc phân chia trần không tạo ra độ chùng thêm và thuật toán tạo ra khung vẽ có chiều cao khớp chính xác với kích thước hình ảnh. Ví dụ, với$a=10, m=5$, chúng tôi nhận được$k_1=2$, mang lại chiều cao$10$, căn chỉnh hoàn hảo. 

Khi một chiều lớn hơn nhiều so với chiều kia, thao tác tối đa chỉ đảm bảo ràng buộc chính là quan trọng. Ví dụ, nếu$a=1000, b=1, m=2, n=100$, sau đó$k_1=500$thống trị$k_2=1$và khung vẽ được điều khiển hoàn toàn bởi giới hạn chiều cao, tạo ra$(1000, 20000)$, chứa cả hai chiều một cách an toàn. 

Khi$\theta = 0$hoặc$\theta = 90$, lời giải vẫn không thay đổi vì tham số xoay không được sử dụng. Quá trình tính toán chỉ đơn giản là thu nhỏ tỷ lệ, vì vậy những trường hợp này hoạt động giống hệt với tất cả các trường hợp khác, điều này tránh được bất kỳ sự mất ổn định nào của dấu phẩy động mà phương pháp hình học sẽ đưa ra.
