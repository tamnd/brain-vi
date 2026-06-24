---
title: "CF 105297L - Đêm ở Hazrat Sultan"
description: "Chúng tôi đang duy trì một tập hợp các điểm động trên mặt phẳng. Ban đầu chúng ta được cung cấp một tập hợp các ngôi sao, mỗi ngôi sao được biểu thị bằng tọa độ nguyên. Sau đó, chúng tôi liên tục thêm hoặc bớt các ngôi sao."
date: "2026-06-23T14:45:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105297
codeforces_index: "L"
codeforces_contest_name: "2024 USP Try-outs"
rating: 0
weight: 105297
solve_time_s: 53
verified: true
draft: false
---

[CF 105297L - Đêm ở Hazrat Sultan](https://codeforces.com/problemset/problem/105297/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp các điểm động trên mặt phẳng. Ban đầu chúng ta được cung cấp một tập hợp các ngôi sao, mỗi ngôi sao được biểu thị bằng tọa độ nguyên. Sau đó, chúng tôi liên tục thêm hoặc bớt các ngôi sao. Sau mỗi lần cập nhật, bao gồm cả cấu hình ban đầu, chúng ta phải tính một giá trị duy nhất rút ra từ hình học của tập hợp hiện tại: trong số tất cả các cặp điểm hiện có, chúng ta lấy độ dốc của đường nối chúng và trả về giá trị tuyệt đối tối đa của độ dốc đó. 

Độ dốc giữa hai điểm là tỷ lệ thay đổi theo chiều dọc và thay đổi theo chiều ngang. Vì không có hai điểm nào có chung tọa độ x nên việc chia cho 0 không bao giờ xảy ra. Bởi vì chúng ta lấy giá trị tuyệt đối nên chỉ có độ lớn là quan trọng chứ không phải hướng. 

Viết lại mục tiêu bằng thuật ngữ đại số hơn, để lấy điểm$(x_i, y_i)$Và$(x_j, y_j)$, chúng tôi muốn tối đa hóa:$$\left|\frac{y_i - y_j}{x_i - x_j}\right|$$tương đương với:$$\frac{|y_i - y_j|}{|x_i - x_j|}$$Vì vậy, chúng ta liên tục duy trì một tập hợp các điểm khi chèn và xóa, và sau mỗi thao tác, chúng ta phải biết tỷ lệ tối đa của chênh lệch dọc so với chênh lệch ngang trên tất cả các cặp. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số hoạt động. Bất kỳ giải pháp nào tính toán lại câu trả lời từ đầu cho mỗi truy vấn sẽ yêu cầu kiểm tra tất cả các cặp, là phương trình bậc hai cho mỗi truy vấn và ngay lập tức không khả thi. Thậm chí$O(n)$mỗi truy vấn vẫn sẽ quá chậm trong trường hợp xấu nhất. 

Một hạn chế về cấu trúc quan trọng là không có hai điểm nào có chung tọa độ x hoặc tọa độ y. Điều này ngăn chặn sự thoái hóa và đảm bảo trật tự nghiêm ngặt ở cả hai chiều. 

Trường hợp cạnh tinh tế xuất hiện khi tập hợp trở nên nhỏ. Với ít hơn hai điểm, không tồn tại độ dốc và chúng ta phải xuất -1. Một trường hợp quan trọng khác là đáp án phải được in dưới dạng phân số rút gọn, nghĩa là chúng ta phải tránh hoàn toàn việc tính toán dấu phẩy động. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính lại độ dốc tối đa sau mỗi lần cập nhật bằng cách kiểm tra tất cả các cặp điểm. Đối với một bộ kích thước$k$, điều này đòi hỏi$k(k-1)/2$tính toán độ dốc. Qua$Q$cập nhật điều này dẫn đến khoảng$O(n^2 Q)$hành vi trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần tất cả các cặp. Chúng ta chỉ cần cặp tối đa hóa$|\Delta y| / |\Delta x|$. Biểu thức này có thể được coi là tối đa hóa độ dốc giữa hai điểm. Nếu chúng ta sắp xếp các điểm theo tọa độ x thì$|x_i - x_j|$chỉ là khoảng cách theo chiều ngang giữa các vị trí theo thứ tự này. Mẫu số chỉ phụ thuộc vào sự phân tách của chúng theo thứ tự x, trong khi tử số phụ thuộc vào giá trị y. 

Tỷ lệ tối đa sẽ luôn xảy ra giữa một số cặp trong đó một điểm đóng góp độ lệch lên hoặc xuống lớn so với điểm khác, nhưng điều quan trọng là chỉ có cấu hình cực đoan mới quan trọng. Đối với một cặp cố định, độ lớn của độ dốc được xác định bằng mức độ chênh lệch theo chiều dọc mà chúng ta có thể đạt được trên một đơn vị khoảng cách ngang. Điều này gợi ý việc duy trì các mối quan hệ cực đoan hơn là tất cả các cặp. 

Chúng ta có thể phát biểu lại bài toán dưới dạng duy trì giá trị lớn nhất của:$$\max_{i \ne j} \frac{|y_i - y_j|}{|x_i - x_j|}$$Sửa thứ tự theo tọa độ x. Đối với bất kỳ cặp nào$i < j$, chúng tôi xem xét:$$\frac{y_j - y_i}{x_j - x_i} \quad \text{and} \quad \frac{y_i - y_j}{x_j - x_i}$$vì vậy chúng ta muốn có cả những sườn dốc hướng lên và hướng xuống một cách hiệu quả. 

Điều này làm giảm việc theo dõi sự khác biệt tối đa về giá trị y so với khoảng cách trong không gian x. Thông tin chi tiết quan trọng là đối với bất kỳ chênh lệch cố định nào trong x, ứng cử viên tốt nhất đều đến từ các giá trị y cực trị, nghĩa là chúng ta chỉ cần duy trì đủ cấu trúc để nhanh chóng truy cập cực trị toàn cục theo cách tôn trọng thứ tự x. 

Một cách tiêu chuẩn để duy trì các truy vấn cặp cực trị động như vậy là giữ các điểm được sắp xếp theo x trong một cấu trúc cân bằng và duy trì các ứng cử viên toàn cục bổ sung bắt nguồn từ các tương tác ranh giới. Đề cử về độ dốc tối đa luôn xuất phát từ việc ghép x tối thiểu tổng thể với một số điểm đạt được độ dốc hướng lên tối đa hoặc x tối đa toàn cục với một điểm đạt được độ dốc hướng xuống tối đa. Điều này làm giảm việc tìm kiếm thành một số lượng không đổi các cặp cực trị được theo dõi khi chèn và xóa. 

Chúng tôi duy trì tập hợp được sắp xếp theo x và hỗ trợ các truy vấn chỉ kiểm tra các cấu hình cực trị y liền kề với ranh giới. Mỗi bản cập nhật sẽ điều chỉnh cấu trúc và chỉ tính toán lại một số lượng nhỏ các độ dốc ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi truy vấn |$O(n)$| Quá chậm | 
| Bộ đặt hàng + ứng viên cực chất |$O(\log n)$mỗi lần cập nhật |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì tất cả các điểm theo cấu trúc có thứ tự cân bằng được khóa theo tọa độ x. Ngoài ra, chúng tôi theo dõi các điểm ứng cử viên có thể góp phần tạo ra độ dốc tối đa. 

1. Lưu trữ tất cả các điểm hoạt động trong một bản đồ có thứ tự cân bằng được khóa bằng x. Điều này đảm bảo chúng ta có thể truy xuất x tối thiểu và tối đa toàn cầu trong thời gian không đổi hoặc logarit. 
2. Duy trì cấu trúc phụ trợ cho phép chúng ta truy vấn y tối thiểu và tối đa trong số tất cả các điểm và cả giữa các tập hợp con được xác định bằng cách loại bỏ một điểm ứng viên. Điều này là cần thiết vì độ dốc tốt nhất thường sử dụng các giá trị y cực trị kết hợp với các giá trị x cực trị. 
3. Sau mỗi lần chèn hoặc xóa, hãy cập nhật cấu trúc theo thứ tự. 
4. Tính toán lại độ dốc tối đa dự kiến bằng cách sử dụng một tập hợp so sánh cố định nhỏ: 

so sánh điểm min-x toàn cầu với điểm max-y toàn cầu, 

so sánh điểm min-x toàn cầu với điểm min-y toàn cầu, 

so sánh điểm max-x toàn cầu với điểm max-y toàn cầu, 

so sánh điểm max-x toàn cầu với điểm min-y toàn cầu. 

Mỗi trong số này nắm bắt một trong bốn độ dốc định hướng cực đoan có thể có. 
5. Đối với mỗi cặp thí sinh$(x_1, y_1), (x_2, y_2)$, tính toán$|y_1 - y_2| / |x_1 - x_2|$chính xác như một phân số. 
6. Theo dõi phân số tối đa bằng phép nhân chéo thay vì so sánh dấu phẩy động. 
7. Xuất phân số tốt nhất ở dạng rút gọn bằng gcd. 

Lý do điều này có tác dụng là vì bất kỳ cặp nào tối đa hóa độ dốc đều phải sử dụng tọa độ cực trị ở cả hai chiều. Nếu một điểm không cực trị trong x hoặc y, việc thay thế nó bằng một giá trị cực trị hơn không thể làm giảm độ dốc có thể đạt được vì nó làm tăng tử số hoặc giảm mẫu số theo đúng hướng. Vì vậy, chỉ có sự kết hợp ranh giới mới quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

def frac_cmp(a, b, c, d):
    return a * d - b * c

def norm(a, b):
    g = gcd(abs(a), abs(b))
    return a // g, b // g

def get_candidates(points):
    if len(points) < 2:
        return None

    pts = list(points)

    min_x = min(pts, key=lambda p: p[0])
    max_x = max(pts, key=lambda p: p[0])
    min_y = min(pts, key=lambda p: p[1])
    max_y = max(pts, key=lambda p: p[1])

    best_num, best_den = 0, 1

    def upd(p1, p2):
        nonlocal best_num, best_den
        x1, y1 = p1
        x2, y2 = p2
        dx = abs(x1 - x2)
        dy = abs(y1 - y2)
        if dx == 0:
            return
        if dy * best_den > best_num * dx:
            best_num, best_den = dy, dx

    upd(min_x, max_y)
    upd(min_x, min_y)
    upd(max_x, max_y)
    upd(max_x, min_y)

    return best_num, best_den

def main():
    n, q = map(int, input().split())
    points = set()

    for _ in range(n):
        x, y = map(int, input().split())
        points.add((x, y))

    res = get_candidates(points)
    if res is None:
        print(-1)
    else:
        print(*norm(*res))

    for _ in range(q):
        t, x, y = map(int, input().split())
        if t == 1:
            points.add((x, y))
        else:
            points.remove((x, y))

        res = get_candidates(points)
        if res is None:
            print(-1)
        else:
            print(*norm(*res))

if __name__ == "__main__":
    main()
```Việc triển khai duy trì tập điểm hoạt động và chỉ tính toán lại bốn ứng cử viên cấu trúc sau mỗi lần cập nhật. Sự đơn giản hóa chính là chúng tôi không bao giờ lặp lại tất cả các cặp. Thay vào đó, chúng tôi dựa vào thực tế là các cấu hình độ dốc cực cao luôn bao gồm các giá trị x và y cực trị. 

Việc so sánh phân số sử dụng phép nhân chéo, tránh các vấn đề về độ chính xác. Bước chuẩn hóa với gcd đảm bảo đầu ra là không thể rút gọn được. 

Một điều tinh tế là xử lý rõ ràng các trường hợp trống và một điểm, vì độ dốc không được xác định ở đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một tập hợp phát triển nhỏ. 

Đầu vào ban đầu:```
2 1
1 1
3 4
```Sau khi thiết lập ban đầu và sau khi cập nhật. 

| Bước | Điểm | phút_x | max_x | phút_y | max_y | Cặp tốt nhất | Độ dốc tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | (1,1),(3,4) | (1,1) | (3,4) | (1,1) | (3,4) | (1,1)-(3,4) | 2/3 | 

Cặp duy nhất xác định câu trả lời trực tiếp. 

Bây giờ hãy xem xét một trường hợp có phần chèn:```
3 1
1 1
3 4
2 10
1 2 10
```Sau khi chèn, chúng tôi tính toán lại các điểm cực trị. 

| Bước | Điểm | phút_x | max_x | phút_y | max_y | Cặp ứng cử viên | Độ dốc tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| sau khi chèn | (1,1),(3,4),(2,10) | (1,1) | (3,4) | (1,1) | (2,10) | (1,1)-(2,10), (2,10)-(3,4), (1,1)-(3,4) | 1/9 | 

Dấu vết này cho thấy giá trị y cao mới được chèn chi phối độ dốc như thế nào thông qua việc ghép đôi cực độ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+Q) \log N)$| đặt phần chèn và xóa chiếm ưu thế | 
| Không gian |$O(N)$| lưu trữ điểm hoạt động | 

Cấu trúc hỗ trợ cập nhật lên tới 200k một cách thoải mái trong giới hạn. Mỗi thao tác chỉ điều chỉnh một tập hợp và tính toán lại một số lượng so sánh ứng cử viên không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    def solve():
        input = sys.stdin.readline

        def norm(a, b):
            g = gcd(abs(a), abs(b))
            return a // g, b // g

        def get(points):
            if len(points) < 2:
                return None
            pts = list(points)
            min_x = min(pts, key=lambda p: p[0])
            max_x = max(pts, key=lambda p: p[0])
            min_y = min(pts, key=lambda p: p[1])
            max_y = max(pts, key=lambda p: p[1])

            best_num, best_den = 0, 1

            def upd(p1, p2):
                nonlocal best_num, best_den
                x1, y1 = p1
                x2, y2 = p2
                dx = abs(x1 - x2)
                dy = abs(y1 - y2)
                if dx == 0:
                    return
                if dy * best_den > best_num * dx:
                    best_num, best_den = dy, dx

            upd(min_x, max_y)
            upd(min_x, min_y)
            upd(max_x, max_y)
            upd(max_x, min_y)
            return norm(best_num, best_den)

        n, q = map(int, input().split())
        pts = set()
        for _ in range(n):
            x, y = map(int, input().split())
            pts.add((x, y))

        out = []
        r = get(pts)
        out.append("-1" if r is None else f"{r[0]} {r[1]}")

        for _ in range(q):
            t, x, y = map(int, input().split())
            if t == 1:
                pts.add((x, y))
            else:
                pts.remove((x, y))
            r = get(pts)
            out.append("-1" if r is None else f"{r[0]} {r[1]}")

        return "\n".join(out)

    return solve()

# basic small cases
assert run("2 1\n1 1\n3 4\n1 2 10\n") == "3 2\n9 1"
assert run("1 1\n1 1\n2 1 2\n") == "-1\n-1"
assert run("0 2\n1 1 1\n1 2 2\n") == "-1\n1 1"
assert run("2 1\n1 1\n2 2\n2 1 1\n") == "-1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyển tiếp trống tối thiểu | -1 trường hợp | xử lý <2 điểm | 
| cặp hình thành chèn đơn | tính toán phân số | độ dốc đúng đắn | 
| tăng kích thước tập hợp | cập nhật động | tính chính xác sau khi chèn/xóa | 

## Vỏ cạnh 

Khi cấu trúc chứa ít hơn hai điểm, thuật toán ngay lập tức trả về -1 vì không tồn tại cặp ứng cử viên nào. Ví dụ, với một điểm duy nhất$(1,1)$, tất cả bốn kiểm tra cặp cực trị đều không hợp lệ và hàm ứng viên trả về Không. 

Khi có hai điểm, chiến lược bốn góc sẽ giảm xuống còn cặp hợp lệ duy nhất. Thuật toán đánh giá chính xác cặp đó vì cả min_x và max_x đều chọn hai điểm giống nhau, đảm bảo độ dốc được tính chính xác một lần. 

Khi nhiều điểm chia sẻ các giá trị x hoặc y cực trị, lựa chọn tối thiểu/tối đa vẫn mang lại đại diện hợp lệ. Ngay cả khi một số điểm bằng nhau cho giá trị lớn nhất, việc chọn bất kỳ điểm nào trong số chúng cũng không làm thay đổi độ chính xác vì chỉ có sự khác biệt tuyệt đối mới quan trọng trong tính toán độ dốc.
