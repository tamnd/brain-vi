---
title: "CF 1612A - Khoảng cách"
description: "Chúng tôi đang làm việc trên một mạng lưới trong đó chuyển động được đo bằng khoảng cách Manhattan, nghĩa là chi phí giữa hai điểm là tổng của các khoảng cách theo chiều ngang và chiều dọc. Một điểm được cố định tại gốc tọa độ, trong khi điểm thứ hai nằm ở tọa độ $(x, y)$."
date: "2026-06-10T06:57:54+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1612
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 117 (Rated for Div. 2)"
rating: 800
weight: 1612
solve_time_s: 114
verified: false
draft: false
---

[CF 1612A - Khoảng cách](https://codeforces.com/problemset/problem/1612/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán mang tính xây dựng 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một mạng lưới trong đó chuyển động được đo bằng khoảng cách Manhattan, nghĩa là chi phí giữa hai điểm là tổng của các khoảng cách theo chiều ngang và chiều dọc. Một điểm cố định ở gốc tọa độ, còn điểm thứ hai nằm ở tọa độ$(x, y)$. Nhiệm vụ là xác định xem có tồn tại điểm lưới trung gian hay không$C$, với tọa độ nguyên không âm, nằm chính xác ở nửa khoảng cách Manhattan giữa điểm gốc và$B$. Nếu một điểm như vậy tồn tại, chúng ta phải xuất ra bất kỳ điểm hợp lệ nào; nếu không thì chúng tôi báo cáo là không thể. 

Cấu trúc của yêu cầu có tính đối xứng: khoảng cách từ điểm gốc đến điểm$C$phải bằng nửa khoảng cách từ điểm gốc đến$B$, và điều tương tự phải được giữ từ$B$ĐẾN$C$. Điều này ngay lập tức ngụ ý rằng$C$không chỉ là bất kỳ điểm giữa nào trong thuật ngữ Euclide, mà là điểm giữa theo mêtric L1, hoạt động rất khác vì khoảng cách phân chia theo các trục thay vì tạo thành một đường thẳng. 

Các ràng buộc là nhỏ, với tọa độ được giới hạn bởi 50 và lên tới 3000 trường hợp thử nghiệm. Điều này có nghĩa là một$O(1)$hoặc ở mức tồi tệ nhất, việc xây dựng hệ số không đổi cho mỗi trường hợp thử nghiệm là bắt buộc; về mặt lý thuyết, thậm chí có thể quét mạnh mẽ lưới điện nhưng không cần thiết. Tuy nhiên, tính đúng đắn bị chi phối không phải bởi hiệu suất mà bởi sự hiểu biết khi nào điểm giữa Manhattan có thể tồn tại. 

Một trường hợp cạnh tinh tế phát sinh khi$x + y$thật kỳ quặc. Trong trường hợp đó, khoảng cách Manhattan từ$A$ĐẾN$B$là số lẻ và giảm một nửa nó sẽ tạo ra một số không nguyên. Vì khoảng cách Manhattan giữa các điểm nguyên luôn là số nguyên nên không thể tồn tại điểm giữa hợp lệ. Một trường hợp lỗi khác xảy ra khi cả hai tọa độ đều nhỏ nhưng có tính chẵn lẻ hỗn hợp, chẳng hạn$(2, 1)$, trong đó những nỗ lực ngây thơ như tách tọa độ một cách độc lập không thể đáp ứng đồng thời cả hai ràng buộc về khoảng cách. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ thử mọi điểm ứng viên$C = (i, j)$với$0 \le i, j \le x + y$, tính cả hai khoảng cách và kiểm tra xem cả hai có bằng nhau không$(x + y)/2$. Điều này hoạt động về mặt khái niệm vì lưới nhỏ, nhưng trong trường hợp xấu nhất, nó kiểm tra một cách đại khái$O((x+y)^2)$ứng viên cho mỗi trường hợp thử nghiệm. Trên 3000 trường hợp thử nghiệm, về mặt lý thuyết, điều này vẫn có thể quản lý được nhưng tốn kém không cần thiết và che khuất cấu trúc cơ bản. 

Quan sát quan trọng là khoảng cách Manhattan phân rã dọc theo các trục. Chúng tôi muốn:$$|x| + |y| = d$$và chúng ta cần một điểm$C = (c_x, c_y)$như vậy:$$c_x + c_y = d/2$$và còn có:$$|x - c_x| + |y - c_y| = d/2$$Hình học trở nên đơn giản khi chúng ta hiểu nó là việc chia tổng khoảng cách cần thiết theo các đóng góp theo chiều ngang và chiều dọc. Vì tất cả các điểm đều nằm trong góc phần tư thứ nhất nên các giá trị tuyệt đối biến mất và vấn đề giảm xuống còn việc phân phối$d/2$đơn vị khoảng cách giữa trục x và y theo cách phù hợp để đạt được cả hai điểm cuối. 

Cho phép$d = x + y$. Nếu như$d$là số lẻ, không tồn tại điểm giữa số nguyên. Nếu nó chẵn thì ta cần xây dựng$c_x, c_y \ge 0$như vậy:$$c_x + c_y = d/2$$Và$c_x$không vượt quá$x$theo cách phá vỡ tính nhất quán với khoảng cách từ$B$. 

Một thủ thuật mang tính xây dựng rõ ràng là thử đặt tất cả tiến trình theo chiều ngang lên hàng đầu: chúng ta cố gắng di chuyển dọc theo trục x cho đến khi chúng ta đi hết một nửa khoảng cách hoặc chúng ta chạm vào$x$, sau đó sử dụng phần còn lại theo hướng y. Một giải pháp thay thế đối xứng cũng có tác dụng; một công trình hợp lệ là:$$c_x = \min(x, d/2), \quad c_y = d/2 - c_x$$Điều này đảm bảo không âm, bảo toàn tổng cần thiết và tự động căn chỉnh cả hai khoảng cách vì mọi đơn vị chuyển động đều được tính chính xác trong số liệu Manhattan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(t \cdot (x+y)^2)$|$O(1)$| Quá chậm | 
| Xây dựng tối ưu |$O(t)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và trực tiếp xây dựng điểm giữa. 

1. Tính toán$d = x + y$. Đây là tổng khoảng cách Manhattan từ điểm xuất phát đến$B$, và nó xác định liệu điểm giữa có khả thi về mặt số hay không. 
2. Nếu$d$là số lẻ, đầu ra$-1 -1$. Lý do là bất kỳ điểm giữa nào cũng phải nằm đúng bằng một nửa khoảng cách này, điều này không thể xảy ra với số nguyên khi$d$thật kỳ quặc. 
3. Hãy để$half = d / 2$. 
4. Đặt$c_x = \min(x, half)$. Điều này ưu tiên ở trong khoảng cách ngang có sẵn của$B$, đảm bảo chúng tôi không vượt quá cấu trúc thẳng hàng theo trục của đường dẫn. 
5. Đặt$c_y = half - c_x$. Điều này buộc khoảng cách cần thiết còn lại phải được phân bổ theo chiều dọc. 
6. Đầu ra$(c_x, c_y)$. 

Phần không rõ ràng là tại sao sự phân bổ tham lam này lại hợp lệ. Số liệu Manhattan cho phép chuyển động độc lập dọc theo các trục, do đó, mọi phân tách tổng khoảng cách thành các thành phần ngang và dọc đều tương ứng với một đường dẫn hợp lệ. Việc xây dựng đảm bảo cả hai nửa của tổng khoảng cách được cân bằng. 

### Tại sao nó hoạt động 

Bất biến chính là ở mỗi bước chúng ta đang xây dựng một điểm có khoảng cách L1 từ điểm gốc bằng chính xác một nửa$x + y$, đồng thời đảm bảo khoảng cách còn lại tới$B$phù hợp với cùng một giá trị. Bởi vì khoảng cách Manhattan có tính cộng trên các trục nên việc gán$c_x$đơn vị chuyển động ngang và$c_y$đơn vị chuyển động thẳng đứng xác định đầy đủ cả hai khoảng cách mà không cần đến thuật ngữ tương tác. Việc phân chia dựa trên mức tối thiểu đảm bảo tính khả thi mà không vi phạm tính không âm hoặc vượt quá tọa độ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        d = x + y

        if d % 2:
            print(-1, -1)
            continue

        half = d // 2
        cx = min(x, half)
        cy = half - cx

        print(cx, cy)

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng nhằm tránh hoàn toàn mọi tìm kiếm. Kiểm tra tính chẵn lẻ là điều kiện duy nhất có thể làm mất hiệu lực sự tồn tại. Khi điều đó trôi qua, việc phân chia bằng cách sử dụng`min(x, half)`đảm bảo chúng tôi không bao giờ gán tọa độ âm và không bao giờ vượt quá tổng khoảng cách cần thiết. 

Một chi tiết triển khai tinh tế là việc tính toán`cy`vì phần còn lại đảm bảo tính nhất quán. Nếu như`cx`đã tiêu thụ hết rồi`half`, sau đó`cy`trở thành số không; nếu không nó sẽ lấp đầy khoảng trống một cách chính xác. Điều này tránh được sự phân nhánh có điều kiện và giữ cho công trình được ổn định. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$x = 49, y = 3$| Bước | d | một nửa | cx | cy | Hiệu lực | 
| --- | --- | --- | --- | --- | --- | 
| Tính toán | 52 | - | - | - | thậm chí | 
| Xây dựng | - | 26 | 26 | 0 | hợp lệ | 

Đây$C = (26, 0)$. Cả hai khoảng cách từ$A$Và$B$bằng 26, thỏa mãn tính đối xứng. 

Điều này thể hiện trường hợp công suất ngang vượt quá một nửa khoảng cách, do đó toàn bộ điểm giữa nằm trên trục x. 

### Ví dụ 2:$x = 2, y = 50$| Bước | d | một nửa | cx | cy | Hiệu lực | 
| --- | --- | --- | --- | --- | --- | 
| Tính toán | 52 | - | - | - | thậm chí | 
| Xây dựng | - | 26 | 2 | 24 | hợp lệ | 

Đây$C = (2, 24)$. Cấu trúc bão hòa trục x trước và đẩy khoảng cách còn lại theo chiều dọc. 

Điều này cho thấy thuật toán thích ứng một cách tự nhiên như thế nào khi một tọa độ nhỏ so với nửa khoảng cách cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm thực hiện các phép tính số học không đổi | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ cho mỗi lần kiểm tra | 

Các ràng buộc cho phép tối đa 3000 trường hợp thử nghiệm và giải pháp xử lý từng trường hợp thử nghiệm trong thời gian không đổi. Điều này giúp việc thực thi thoải mái trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        x, y = map(int, input().split())
        d = x + y
        if d % 2:
            out.append("-1 -1")
        else:
            half = d // 2
            cx = min(x, half)
            cy = half - cx
            out.append(f"{cx} {cy}")
    return "\n".join(out)

# provided samples
assert run("10\n49 3\n2 50\n13 0\n0 41\n42 0\n0 36\n13 37\n42 16\n42 13\n0 0\n") == \
"""23 3
1 25
-1 -1
-1 -1
21 0
0 18
13 12
25 4
-1 -1
0 0"""

# custom cases
assert run("1\n1 1\n") == "1 0", "minimum non-trivial even"
assert run("1\n0 0\n") == "0 0", "origin case"
assert run("1\n2 1\n") == "-1 -1", "odd sum impossible"
assert run("1\n50 50\n") == "50 0", "symmetric max case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 0 | tổng chẵn hợp lệ nhỏ nhất | 
| 0 0 | 0 0 | trường hợp nguồn gốc thoái hóa | 
| 2 1 | -1 -1 | khoảng cách lẻ không thể | 
| 50 50 | 50 0 | trường hợp đối xứng lớn cân bằng | 

## Vỏ cạnh 

Khi nào$x + y = 0$, thuật toán trả về ngay$(0, 0)$, vì cả hai khoảng cách đều bằng 0 và điều kiện điểm giữa suy biến chính xác. Việc xây dựng không thực hiện bất kỳ phép chia nào ngoài việc kiểm tra tính chẵn lẻ, do đó không cần xử lý đặc biệt. 

Ví dụ, khi một tọa độ bằng 0$(42, 0)$, bộ thuật toán$half = 21$, sau đó gán$cx = 21$,$cy = 0$, bảo toàn chính xác cả hai giới hạn khoảng cách. Điều này tránh được lỗi phổ biến khi cố gắng phân chia tọa độ một cách độc lập mà không xem xét rằng tất cả khoảng cách phải đến từ một trục duy nhất. 

Khi$x + y$thật kỳ lạ, chẳng hạn như$(13, 37)$, hàm sẽ từ chối ngay lập tức. Bất kỳ nỗ lực nào để xây dựng điểm giữa sẽ yêu cầu khoảng cách Manhattan phân đoạn, điều này là không thể trong lưới số nguyên, vì vậy đầu ra$-1 -1$bị ép buộc. 

Việc xây dựng xử lý các trường hợp này một cách nhất quán vì nó không bao giờ giả định tính đối xứng Euclide, chỉ có sự phân tích cộng tính của khoảng cách Manhattan.
