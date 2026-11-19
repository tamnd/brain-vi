---
title: "CF 2109B - Cắt lát để sống sót"
description: "Chúng ta có một lưới hình chữ nhật có kích thước $n nhân m$ và một mã thông báo (một con quái vật) ban đầu được đặt tại ô $(a, b)$."
date: "2026-06-08T04:38:30+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2109
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1025 (Div. 2)"
rating: 1200
weight: 2109
solve_time_s: 97
verified: true
draft: false
---

[CF 2109B - Cắt lát để sống sót](https://codeforces.com/problemset/problem/2109/B) 

**Đánh giá:** 1200 
**Tags:** bitmasks, tham lam, toán học 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$n \times m$và một mã thông báo (một con quái vật) ban đầu được đặt tại ô$(a, b)$. Hai người chơi thay nhau đưa ra quyết định, nhưng thay vì luân phiên lượt theo nghĩa thông thường, mỗi lượt bao gồm hai giai đoạn: đầu tiên là cắt hình chữ nhật hiện tại dọc theo một hàng hoặc cột đầy đủ, sau đó di chuyển quái vật vào bất kỳ vị trí nào bên trong hình chữ nhật còn lại. 

Việc cắt là thao tác duy nhất làm giảm kích thước bảng. Sau mỗi lần chém, đối thủ có thể tự do định vị lại quái vật trong hình chữ nhật còn lại, điều này có nghĩa là từ lượt tiếp theo trở đi, vị trí của nó không còn bị hạn chế bởi các bước di chuyển trước đó. Trò chơi tiếp tục cho đến khi hình chữ nhật trở thành một ô duy nhất và mục tiêu là xác định số lượt mà quá trình này diễn ra trong cách chơi tối ưu, trong đó một người chơi cố gắng giảm thiểu số lần cắt và người kia cố gắng tối đa hóa chúng. 

Một quan sát quan trọng là sau lần cắt đầu tiên, vị trí ban đầu chính xác sẽ mất đi ý nghĩa lâu dài vì quái vật có thể được di chuyển tùy ý. Điều này về cơ bản tạo ra vấn đề về việc có thể thu nhỏ một hình chữ nhật nhanh như thế nào khi mỗi lần cắt chỉ có thể đảm bảo giữ một phần chứa điểm đã chọn, nhưng đối thủ sẽ luôn định vị lại để khiến các lần cắt trong tương lai trở nên kém hiệu quả nhất có thể. 

Những hạn chế$n, m \le 10^9$ngay lập tức loại trừ bất kỳ mô phỏng nào của lưới hoặc giảm dần từng bước. Bất kỳ giải pháp khả thi nào cũng phải tính toán câu trả lời trong thời gian không đổi cho mỗi trường hợp thử nghiệm, có thể sử dụng lý luận logarit hoặc biểu thức dạng đóng. 

Ví dụ: trường hợp cạnh tinh tế xuất hiện khi một chiều đã nhỏ$1 \times m$hoặc$n \times 1$. Trong những trường hợp như vậy, chỉ có thể thực hiện một hướng cắt và các đối số đối xứng tham lam giả sử cả hai chiều co lại đồng thời có thể dẫn đến việc đếm quá mức. 

Một tình huống khó khăn khác là khi vị trí bắt đầu ở gần mép. Ví dụ, trong một$2 \times 7$lưới với quái vật ở cột 2, việc cắt tối ưu sớm có thể cô lập một lưới con nhỏ hơn nhiều so với khi quái vật bắt đầu ở trung tâm. Một cách tiếp cận ngây thơ mà bỏ qua$(a, b)$hoàn toàn sẽ thất bại trong những trường hợp như vậy vì nước đi đầu tiên rất đặc biệt và phụ thuộc vào tính bất đối xứng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực mô phỏng trạng thái trò chơi dưới dạng một hình chữ nhật và liên tục thử mọi đường cắt ngang hoặc dọc có thể, luôn cho rằng đối thủ sẽ đặt lại vị trí của quái vật theo hướng đối nghịch. Mỗi trạng thái được xác định bởi$(n, m)$và các chuyển tiếp sẽ thử mọi cách có thể để chia hình chữ nhật trong khi vẫn giữ quái vật ở trong một bên. Tuy nhiên, ngay cả việc đại diện cho tất cả các bang cũng đã mang lại$O(nm)$khả năng và mỗi quá trình chuyển đổi phụ thuộc vào việc quét các vị trí cắt có thể xảy ra, khiến nó hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là sau lần di chuyển đầu tiên, quyền tự do của con quái vật sẽ loại bỏ các ràng buộc về vị trí, do đó, vấn đề sẽ liên tục giảm một nửa các chiều theo cách tồi tệ nhất có thể. Chiến lược tối ưu trở thành: mỗi lần cắt làm giảm số hàng hoặc số cột và đối thủ đảm bảo mảnh còn lại luôn là đoạn lớn hơn có thể có sau khi cắt. 

Từ lượt thứ hai trở đi, quá trình này hoạt động giống như cắt liên tục một hình chữ nhật trong đó mỗi lần di chuyển sẽ giảm bớt một trong hai$n$hoặc$m$bằng cách chọn cách phân chia có thể giảm thiểu tiến độ. Điều này biến vấn đề thành một quá trình rút gọn logarit trên mỗi chiều. Sự phức tạp duy nhất là nước đi đầu tiên, trong đó vị trí ban đầu$(a, b)$cho phép cắt lần đầu có khả năng không đối xứng và có thể giảm ngay một chiều thành một trong hai$a$hoặc$n-a+1$và tương tự cho cột. 

Do đó, giải pháp giảm xuống còn việc đánh giá bốn hình chữ nhật bắt đầu ứng cử viên thu được từ lần cắt đầu tiên và sau đó tính toán số lần cắt cần thiết để thu nhỏ từng hình chữ nhật thu được thành$1 \times 1$, luôn giả định độ co rút xen kẽ tối ưu của các kích thước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nm) | O(nm) | Quá chậm | 
| Giảm thiểu dựa trên nhật ký tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý nước đi đầu tiên một cách riêng biệt vì đây là thời điểm duy nhất mà vị trí ban đầu của quái vật hạn chế vết cắt. 

1. Hãy xem xét một vết cắt ngang đầu tiên. Chúng ta có thể cắt phía trên hoặc phía dưới con quái vật, để lại một lưới chiều cao$a$hoặc$n-a+1$, trong khi chiều rộng vẫn giữ nguyên$m$. Chúng tôi lấy chiều cao thu được nhỏ hơn vì người chơi thu nhỏ chọn đường cắt tốt hơn. 
2. Tương tự, hãy xem xét vết cắt đầu tiên theo chiều dọc. Điều này mang lại một lưới có kích thước$n \times b$hoặc$n \times (m-b+1)$, và một lần nữa chúng tôi giữ chiều rộng nhỏ hơn. 
3. Bây giờ chúng ta định nghĩa một hàm$f(x, y)$đại diện cho số lượt cần thiết để giảm một$x \times y$hình chữ nhật để$1 \times 1$khi quái vật luôn có thể đặt lại vị trí một cách tối ưu. 
4. Trong một lượt, người chơi có thể giảm một trong hai chiều bằng cách chọn một vết cắt, nhưng đối thủ đảm bảo vị trí hiệu quả đạt được luôn buộc phải giảm bớt trong trường hợp xấu nhất trong tương lai. Điều này có nghĩa là mỗi chiều hoạt động độc lập về số lượng bước giảm một nửa được yêu cầu. 
5. Vì vậy,$f(x, y)$bằng số lần chúng ta cần giảm cả hai chiều xuống 1, trong đó mỗi lần di chuyển sẽ giảm một chiều nhiều nhất có thể nhưng chỉ bằng cách chia nó thành hai phần và giữ phần lớn hơn. 

Trên thực tế, mỗi chiều hoạt động giống như liên tục lấy nửa lớn hơn sau khi cắt, tương ứng với việc chia liên tục cho 2 trong trường hợp xấu nhất. 

Chúng tôi tính toán: 

-$f(x, y) = \lceil \log_2(x) \rceil + \lceil \log_2(y) \rceil$Cuối cùng, chúng tôi đánh giá tất cả bốn kết quả của nước đi đầu tiên và tính tổng số lượt tối thiểu bao gồm cả lần cắt ban đầu. 

### Tại sao nó hoạt động 

Sau lần cắt đầu tiên, con quái vật luôn có thể đặt lại vị trí tồi tệ nhất có thể cho những lần cắt tiếp theo. Điều này loại bỏ bất kỳ bộ nhớ vị trí nào và biến mỗi chiều thành một quy trình thu nhỏ độc lập, trong đó mỗi lần cắt giảm tối đa một mức tỷ lệ nhị phân của chiều đó. Vì một lần cắt chỉ có thể chia một trục trên mỗi lượt nên tổng số lượt sẽ tích lũy số bước giảm một nửa cần thiết dọc theo cả hai trục. Vị trí ban đầu chỉ ảnh hưởng đến lần giảm đầu tiên, sau đó quá trình trở nên hoàn toàn đối xứng và không phụ thuộc vào trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ceil_log2(x):
    # smallest k such that 2^k >= x
    k = 0
    v = 1
    while v < x:
        v <<= 1
        k += 1
    return k

def solve_case(n, m, a, b):
    # first cut horizontally
    h1 = min(a, n - a + 1)
    v1 = m

    # first cut vertically
    h2 = n
    v2 = min(b, m - b + 1)

    def f(x, y):
        return ceil_log2(x) + ceil_log2(y)

    # we already spend 1 turn on the first cut
    ans = 10**18

    ans = min(ans, 1 + f(h1, v1))
    ans = min(ans, 1 + f(h2, v2))

    return ans

t = int(input())
out = []
for _ in range(t):
    n, m, a, b = map(int, input().split())
    out.append(str(solve_case(n, m, a, b)))

print("\n".join(out))
```Việc triển khai tách biệt bước đi đầu tiên một cách rõ ràng vì đó là điểm duy nhất mà vị trí của quái vật quan trọng. Người trợ giúp`ceil_log2`tính toán số lần giảm một nửa cần thiết để một thứ nguyên đạt tới 1, mô hình này trực tiếp mô hình hóa sự phân chia tham lam trong trường hợp xấu nhất theo tái định vị đối nghịch. 

chức năng`solve_case`đánh giá cả hai hướng có thể có của lần cắt đầu tiên. Đối với mỗi hướng, nó tính toán hình chữ nhật thu được và sau đó cộng thêm chi phí để thu nhỏ nó hoàn toàn. Câu trả lời cuối cùng là mức tối thiểu trong hai khả năng này. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ mô phỏng các lựa chọn xen kẽ một cách rõ ràng. Hành vi tối ưu của đối thủ đã được mã hóa bằng cách luôn lấy phân đoạn kết quả lớn hơn trong mô hình logarit, vì vậy chúng tôi tránh bất kỳ cây trò chơi phân nhánh nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n=2, m=7, a=1, b=4$Chúng tôi đánh giá cả hai động thái đầu tiên. 

| Lần cắt đầu tiên | Lưới kết quả | f(x,y) | Tổng cộng | 
| --- | --- | --- | --- | 
| ngang | 1 × 7 | 0 + 3 = 3 | 4 | 
| dọc | 2 × 4 | 1 + 2 = 3 | 4 | 

Câu trả lời là 4, nghĩa là bất kể sự bất đối xứng ban đầu như thế nào, cả hai chiến lược đều có số lượt như nhau. 

Điều này cho thấy rằng khi một chiều đã ở mức tối thiểu, trò chơi sẽ giảm xuống mức thu nhỏ logarit thuần túy của trục còn lại. 

### Ví dụ 2 

đầu vào:$n=8, m=9, a=4, b=6$| Lần cắt đầu tiên | Lưới kết quả | f(x,y) | Tổng cộng | 
| --- | --- | --- | --- | 
| ngang | 4 × 9 | 2 + 4 = 6 | 7 | 
| dọc | 8 × 4 | 3 + 2 = 5 | 6 | 

Câu trả lời tối ưu là 6. 

Ví dụ này nêu bật việc lựa chọn vết cắt đầu tiên tốt hơn như thế nào phụ thuộc vào việc việc giảm chiều cao hay chiều rộng mang lại cấu hình logarit cân bằng hơn hay không. Việc cắt theo chiều dọc là tốt hơn vì nó làm giảm chi phí logarit lớn hơn trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi bài kiểm tra | Chỉ có một số lượng tính toán logarit không đổi được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Giải pháp dễ dàng nằm trong giới hạn vì thậm chí$10^4$các trường hợp thử nghiệm chỉ yêu cầu số học đơn giản và các vòng lặp nhỏ để tính toán logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def ceil_log2(x):
        k = 0
        v = 1
        while v < x:
            v <<= 1
            k += 1
        return k

    def solve(n, m, a, b):
        h1, v1 = min(a, n-a+1), m
        h2, v2 = n, min(b, m-b+1)

        def f(x, y):
            return ceil_log2(x) + ceil_log2(y)

        return str(min(1 + f(h1, v1), 1 + f(h2, v2)))

    t = int(input())
    out = []
    for _ in range(t):
        n, m, a, b = map(int, input().split())
        out.append(solve(n, m, a, b))
    return "\n".join(out)

# provided samples
assert run("""8
2 2 1 1
3 3 2 2
2 7 1 4
2 7 2 2
8 9 4 6
9 9 5 5
2 20 2 11
22 99 20 70
""") == """2
4
4
3
6
8
6
10"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 1 1 | 2 | lưới nhỏ nhất | 
| 2 7 1 4 | 4 | cạnh bắt đầu không đối xứng | 
| 9 9 5 5 | 8 | đối xứng bắt đầu ở giữa | 

## Vỏ cạnh 

A$1 \times m$hoặc$n \times 1$lưới loại bỏ một bậc tự do ngay lập tức. Thuật toán xử lý việc này một cách chính xác vì một trong các số hạng logarit trở thành 0 và lần cắt đầu tiên giảm trực tiếp xuống bài toán một chiều. 

Ví dụ,$1 \times 8$với bất kỳ$b$sản lượng: 

không thể cắt theo chiều ngang nên chỉ sử dụng theo chiều dọc, tạo ra$1 \times 4$, sau đó$1 \times 2$, sau đó$1 \times 1$, thực hiện 3 lượt. Công thức tạo ra$\lceil \log_2 8 \rceil = 3$, khớp chính xác. 

Khi quái vật bắt đầu ở gần một ranh giới như$(1, m)$, vết cắt ngang đầu tiên có tính không đối xứng cao và ngay lập tức giảm chiều cao xuống 1, điều này có thể giảm đáng kể chi phí trong tương lai. Thuật toán nắm bắt được điều này bằng cách lấy$\min(a, n-a+1)$, đảm bảo rằng mặt cắt tối ưu luôn được chọn thay vì giả định tính đối xứng. 

Khởi động trung tâm hoàn toàn đối xứng như$(\frac{n}{2}, \frac{m}{2})$làm cho cả hai lựa chọn đầu tiên có thể so sánh được. Thuật toán sẽ đánh giá cả hai một cách tự nhiên và chọn ra cái tốt hơn, đảm bảo không phụ thuộc vào việc phá vỡ ràng buộc tùy tiện.
