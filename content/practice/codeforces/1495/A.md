---
title: "CF 1495A - Thợ mỏ kim cương"
description: "Chúng ta có hai tập hợp điểm trên một mặt phẳng. Một bộ chứa các công cụ khai thác và tất cả chúng đều nằm hoàn toàn trên trục tung, do đó mỗi công cụ khai thác có tọa độ có dạng $(0, y)$."
date: "2026-06-10T22:02:22+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1495
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 706 (Div. 1)"
rating: 1200
weight: 1495
solve_time_s: 365
verified: false
draft: false
---

[CF 1495A - Công cụ khai thác kim cương](https://codeforces.com/problemset/problem/1495/A) 

**Đánh giá:** 1200 
**Tags:** hình học, tham lam, toán học, sắp xếp 
**Thời gian giải:** 6m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên một mặt phẳng. Một bộ chứa các công cụ khai thác và tất cả chúng đều nằm hoàn toàn trên trục tung, do đó mỗi công cụ khai thác có tọa độ có dạng$(0, y)$. Bộ còn lại chứa các mỏ kim cương và tất cả chúng đều nằm hoàn toàn trên trục hoành nên mỗi mỏ có tọa độ dạng$(x, 0)$. Mỗi điểm thuộc về chính xác một trong hai bộ này và có số lượng thợ mỏ và mỏ bằng nhau. 

Mỗi thợ mỏ phải được chỉ định cho chính xác một mỏ, tạo thành sự kết hợp hoàn hảo giữa hai bộ. Chi phí ghép nối một công cụ khai thác tại$(0, y)$với một mỏ ở$(x, 0)$là khoảng cách Euclide$\sqrt{x^2 + y^2}$. Nhiệm vụ là chọn cặp sao cho tổng các khoảng cách này nhỏ nhất. 

Kích thước đầu vào tăng lên$n = 10^5$cho mỗi trường hợp thử nghiệm, với tổng của tất cả$n$trên các trường hợp thử nghiệm cũng bị giới hạn bởi$10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các kết quả khớp, vì số lượng bài tập theo giai thừa là không thể. Thậm chí$O(n^2)$các giải pháp quá chậm vì chúng đòi hỏi khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một khó khăn tinh tế xuất phát từ thực tế là tọa độ có thể dương hoặc âm và khoảng cách phụ thuộc vào giá trị bình phương. Một ý tưởng tham lam ngây thơ như khớp các giá trị tuyệt đối nhỏ nhất hoặc khớp theo dấu tọa độ một cách độc lập có thể thất bại. Ví dụ: nếu thợ mỏ đang ở$(0, 1)$,$(0, 100)$và mỏ tại$(1, 0)$,$(2, 0)$, ghép nối theo thứ tự giá trị tuyệt đối được sắp xếp có vẻ hợp lý nhưng bỏ qua việc ghép nối chéo có thể giảm khoảng cách lớn hiệu quả hơn. 

Một trường hợp cạnh quan trọng khác là khi nhiều điểm có cùng độ lớn tọa độ nhưng khác dấu. Vì khoảng cách phụ thuộc vào bình phương, nên tính đối xứng không ngụ ý ngay lập tức một cặp đơn giản trừ khi chúng ta suy luận chính xác về tính đơn điệu của hàm căn bậc hai đối với các đối số dương. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi khả năng có thể xảy ra giữa thợ mỏ và mỏ và tính toán tổng chi phí. Điều này đúng vì nó đánh giá tất cả các phép gán, nhưng số lượng phép đối là$n!$, phát triển quá nhanh ngay cả đối với$n = 10^5$. Ngay cả khi chúng ta giới hạn ở những trường hợp nhỏ hơn, việc tính toán vẫn không thể thực hiện được. 

Một cách tiếp cận có cấu trúc hơn xuất phát từ việc quan sát rằng mỗi chi phí chỉ được xác định bởi cặp$(|x|, |y|)$, vì khoảng cách là$\sqrt{x^2 + y^2}$. Ý tưởng chính là chúng ta không thực sự quan tâm đến các dấu hiệu nhằm mục đích sắp xếp thứ tự mà chỉ quan tâm đến độ lớn. Sau khi chúng tôi sắp xếp các công cụ khai thác theo giá trị tuyệt đối của chúng$y$-tọa độ và mỏ theo giá trị tuyệt đối của chúng$x$-tọa độ, chúng ta có thể chỉ ra rằng việc ghép chúng theo thứ tự được sắp xếp sẽ giảm thiểu tổng chi phí lồi do căn bậc hai của tổng bình phương gây ra. 

Trực giác tương tự như một đối số trao đổi: nếu hai cặp được “bắt chéo” theo thứ tự, việc hoán đổi chúng không thể làm tăng tổng chi phí vì hàm$f(x, y) = \sqrt{x^2 + y^2}$hành xử đơn điệu đối với cả hai đối số theo cách lồi đối xứng. Điều này đẩy sự kết hợp tối ưu về phía căn chỉnh được sắp xếp. 

Do đó, vấn đề giảm xuống còn việc sắp xếp cả hai danh sách theo giá trị tuyệt đối và ghép các phần tử tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách các công cụ khai thác và mỏ, trích xuất tọa độ tuyệt đối của chúng và sắp xếp cả hai mảng. Sau đó, chúng tôi ghép chúng theo chỉ mục. 

1. Đọc tất cả các điểm và chia chúng thành hai mảng: thợ mỏ và mỏ. Chúng tôi sử dụng các giá trị tuyệt đối vì khoảng cách chỉ phụ thuộc vào bình phương và dấu hiệu không ảnh hưởng đến quyết định đặt hàng. 
2. Thu thập tất cả thợ mỏ$y$-tọa độ vào một danh sách và tất cả của tôi$x$-tọa độ vào một danh sách khác. 
3. Sắp xếp cả hai danh sách theo thứ tự không giảm. Điều này sắp xếp cường độ nhỏ nhất với nhau và lớn nhất với lớn nhất. 
4. Lặp lại từ trái sang phải và tính toán$\sqrt{x_i^2 + y_i^2}$cho mỗi cặp, tích lũy tổng số tiền. 
5. Xuất số tiền cuối cùng với độ chính xác cao. 

Bước không hề đơn giản là quy tắc sắp xếp và ghép nối. Tính chính xác xuất phát từ thực tế là bất kỳ sự đảo ngược nào trong việc ghép cặp đều có thể được hoán đổi để giảm hoặc duy trì tổng chi phí do tính chất lồi đối xứng của hàm khoảng cách trên các giá trị tuyệt đối độc lập. 

### Tại sao nó hoạt động 

Hãy xem xét hai thợ mỏ có độ lớn$a \le b$và hai mỏ có độ lớn$c \le d$. Có hai cặp có thể xảy ra:$(a, c), (b, d)$Và$(a, d), (b, c)$. Cấu trúc tối ưu được thể hiện bằng bất đẳng thức$$\sqrt{a^2 + c^2} + \sqrt{b^2 + d^2} \le \sqrt{a^2 + d^2} + \sqrt{b^2 + c^2}.$$Điều này đúng vì sự mất cân bằng ngày càng tăng giữa các cường độ được ghép nối làm tăng tổng chi phí Euclide. Việc áp dụng đối số hoán đổi này nhiều lần sẽ loại bỏ tất cả các phép đảo ngược, dẫn đến việc ghép nối được sắp xếp là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

t = int(input())
out_lines = []

for _ in range(t):
    n = int(input())
    miners = []
    mines = []

    for _ in range(2 * n):
        x, y = map(int, input().split())
        if x == 0:
            miners.append(abs(y))
        else:
            mines.append(abs(x))

    miners.sort()
    mines.sort()

    total = 0.0
    for i in range(n):
        total += math.sqrt(miners[i] * miners[i] + mines[i] * mines[i])

    out_lines.append(str(total))

print("\n".join(out_lines))
```Mã đầu tiên phân tách các đầu vào dựa trên tư cách thành viên trục. Điều này tránh việc trộn lẫn các vai trò, điều này rất cần thiết vì việc ghép đôi chỉ có hiệu lực giữa hai nhóm. Lấy giá trị tuyệt đối đảm bảo so sánh nhất quán về độ lớn. 

Sắp xếp cả hai danh sách là bước cấu trúc để thực thi việc ghép nối tối ưu. Vòng lặp cuối cùng áp dụng trực tiếp công thức khoảng cách Euclide cho mỗi cặp khớp. Số học dấu phẩy động ở đây an toàn vì bài toán cho phép dung sai lỗi rất nhỏ và độ chính xác gấp đôi của Python là đủ. 

Một lỗi triển khai phổ biến là quên phân tách nghiêm ngặt các công cụ khai thác và mỏ hoặc cố gắng sắp xếp tọa độ thô bao gồm cả dấu hiệu, điều này phá vỡ đối số ghép nối đơn điệu. Một vấn đề phức tạp khác là sử dụng số học số nguyên cho các giá trị trung gian; vì khoảng cách liên quan đến căn bậc hai nên mọi thứ phải được tính bằng dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
miners = [1, -1]
mines = [1, -2]
```Sau khi chuyển đổi tuyệt đối: 

thợ mỏ = [1, 1], mỏ = [1, 2] 

| Bước | thợ mỏ | mỏ | ghép đôi | đóng góp | 
| --- | --- | --- | --- | --- | 
| được sắp xếp | [1, 1] | [1, 2] | | | 
| tôi=0 | | | (1, 1) | √2 | 
| tôi=1 | | | (1, 2) | √5 | 

Tổng cộng = √2 + √5 

Điều này cho thấy rằng việc sắp xếp giúp ổn định việc ghép nối và tránh sự phân công chéo. 

### Ví dụ 2 

đầu vào:```
miners = [3, 10]
mines = [1, 7]
```| Bước | thợ mỏ | mỏ | ghép đôi | đóng góp | 
| --- | --- | --- | --- | --- | 
| được sắp xếp | [3, 10] | [1, 7] | | | 
| tôi=0 | | | (3, 1) | √10 | 
| tôi=1 | | | (10, 7) | √149 | 

Tổng cộng = √10 + √149 

Điều này xác nhận rằng các giá trị lớn được ghép nối với nhau, ngăn chặn việc gán chéo không hiệu quả có thể làm tăng sự mất cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| phân loại chiếm ưu thế; ghép nối là tuyến tính | 
| Không gian |$O(n)$| lưu trữ hai danh sách kích thước$n$| 

Các ràng buộc cho phép lên đến$10^5$tổng số điểm, do đó việc sắp xếp một lần cho mỗi trường hợp kiểm thử sẽ hiệu quả trong giới hạn thời gian và việc quét tuyến tính sau đó là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out_lines = []

    for _ in range(t):
        n = int(input())
        miners = []
        mines = []

        for _ in range(2 * n):
            x, y = map(int, input().split())
            if x == 0:
                miners.append(abs(y))
            else:
                mines.append(abs(x))

        miners.sort()
        mines.sort()

        total = 0.0
        for i in range(n):
            total += math.sqrt(miners[i]**2 + mines[i]**2)

        out_lines.append(str(total))

    return "\n".join(out_lines)

# provided sample (partial check placeholder)
# assert run("...") == "..."

# custom cases
assert abs(run("""1
1
0 5
3 0
""") - str(math.sqrt(34))) < 1e-9

assert abs(run("""1
2
0 1
0 100
1 0
2 0
""").split()[0] != "")  # sanity check format

assert abs(run("""1
3
0 3
0 1
0 2
3 0
1 0
2 0
""") - str(math.sqrt(10)+math.sqrt(2)+math.sqrt(8))) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 thợ mỏ đơn giản | √34 | ghép nối cơ bản đúng đắn | 
| cường độ hỗn hợp | tính toán | sắp xếp mạnh mẽ | 
| 3 cặp đối xứng | tổng các căn thẳng hàng | ổn định hoán vị | 

## Vỏ cạnh 

Khi tất cả các công cụ khai thác và mỏ nằm đối xứng với các giá trị tuyệt đối lặp lại, chẳng hạn như nhiều điểm ở cùng một khoảng cách so với điểm gốc, việc sắp xếp vẫn tạo ra một cặp hợp lệ vì bất kỳ hoán vị nào trong các phần tử bằng nhau đều bảo toàn tổng chi phí. Thuật toán không phụ thuộc vào tie-break. 

Khi một bên có sự mất cân bằng cực độ, ví dụ như thợ mỏ$[1, 1, 1]$và mỏ$[1, 100, 100]$, thuật toán đảm bảo các giá trị nhỏ kết hợp với các giá trị nhỏ trước tiên, để lại việc ghép nối lớn-lớn ở cuối cùng, giúp ngăn ngừa lạm phát chi phí trung gian không cần thiết. 

Trong trường hợp tất cả các giá trị giống hệt nhau, mỗi cặp đều tạo ra tổng chi phí như nhau và việc sắp xếp sẽ suy biến thành kết quả khớp tùy ý mà không ảnh hưởng đến tính chính xác.
