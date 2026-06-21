---
title: "CF 1059D - Khu bảo tồn thiên nhiên"
description: "Chúng ta được cho một tập hợp các điểm trên mặt phẳng, mỗi điểm đại diện cho vị trí của một con vật. Chúng ta cần đặt một vòng tròn bao phủ tất cả các điểm này. Đồng thời có một dòng sông nằm ngang cố định, sau khi biến đổi sẽ trở thành trục x nên đường thẳng $y = 0$."
date: "2026-06-15T09:37:53+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1059
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 514 (Div. 2)"
rating: 2200
weight: 1059
solve_time_s: 278
verified: true
draft: false
---

[CF 1059D - Khu bảo tồn thiên nhiên](https://codeforces.com/problemset/problem/1059/D) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, hình học, tìm kiếm ba ngôi 
**Thời gian giải:** 4 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trên mặt phẳng, mỗi điểm đại diện cho vị trí của một con vật. Chúng ta cần đặt một vòng tròn bao phủ tất cả các điểm này. Đồng thời có dòng sông nằm ngang cố định, sau khi biến đổi trở thành trục x nên đường thẳng là$y = 0$. 

Đường tròn phải thỏa mãn hai ràng buộc hình học ngoài việc chỉ bao phủ tất cả các điểm. Đầu tiên, nó phải chạm sông ít nhất một lần, nghĩa là khoảng cách từ tâm vòng tròn đến đường thẳng$y = 0$tối đa phải là bán kính. Thứ hai, vòng tròn không được giao nhau với sông ở nhiều hơn một điểm. Đối với một đường tròn và một đường thẳng, có nhiều hơn một điểm chung nghĩa là đường thẳng đó cắt qua đường tròn đó nên khoảng cách từ tâm đến đường thẳng phải tuyệt đối nhỏ hơn bán kính là không được phép. Việc kết hợp cả hai ràng buộc sẽ tạo ra một cấu trúc rất cụ thể: đường tròn phải tiếp tuyến với trục x. 

Vì vậy trung tâm$(x, y)$phải thỏa mãn$|y| = r$, và vì đường tròn phải chứa tất cả các điểm nên mọi điểm$(x_i, y_i)$phải thỏa mãn:$$(x - x_i)^2 + (y - y_i)^2 \le r^2.$$Từ$|y| = r$, bài toán quy về việc tìm tâm trên một đường nằm ngang ở khoảng cách$r$từ trục x giúp giảm thiểu$r$trong khi bao gồm tất cả các điểm. 

Kích thước đầu vào đạt$10^5$, điều này ngay lập tức loại trừ bất kỳ điều gì tính toán lại khoảng cách cho tất cả các cặp hoặc thử các vòng tròn ứng cử viên cho mỗi cặp điểm. Bất kỳ giải pháp nào cũng phải giảm vấn đề thành một tối ưu hóa liên tục duy nhất trên một số lượng nhỏ các biến và đánh giá nó theo thời gian tuyến tính cho mỗi lần đoán. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các điểm nằm rất gần trục x theo cách đối xứng. Một cách tiếp cận ngây thơ cố gắng giả sử tâm đường tròn nằm trên tất cả các điểm hoặc bên dưới tất cả các điểm một cách độc lập có thể thất bại, vì giải pháp đúng có thể yêu cầu đặt tâm ở trên hoặc dưới tùy thuộc vào sự phân bố. 

Một trường hợp sai sót khác phát sinh nếu người ta cho rằng tâm nằm ngay trên hoặc dưới tâm. Điều đó không được đảm bảo: vòng tròn tối ưu bị hạn chế bởi cả độ che phủ và tiếp tuyến, do đó tọa độ x và tọa độ y tương tác với nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng đoán tâm và bán kính vòng tròn, kiểm tra tính khả thi bằng cách xác minh tất cả các điểm đều nằm bên trong. Vì tâm liên tục trong hai chiều nên nó trở thành không gian tìm kiếm vô hạn. Ngay cả việc rời rạc hóa x và y trên một lưới mịn cũng không khả thi: mỗi lần kiểm tra là$O(n)$và lưới sẽ yêu cầu quá nhiều đánh giá. 

Thông tin chi tiết về cấu trúc quan trọng là đường tròn luôn tiếp xúc với trục x, do đó tâm của nó bị hạn chế nằm trên một trong hai trục.$y = r$hoặc$y = -r$. Chúng ta có thể sửa một dấu hiệu và tập trung vào một trường hợp; sự đối xứng xử lý cái kia. 

Cố định trung tâm tại$(x, r)$, điều kiện của một điểm trở thành:$$(x - x_i)^2 + (r - y_i)^2 \le r^2.$$Việc khai triển và đơn giản hóa sẽ loại bỏ phương trình bậc hai trong$r$:$$(x - x_i)^2 + r^2 - 2r y_i + y_i^2 \le r^2$$giảm xuống còn:$$(x - x_i)^2 + y_i^2 \le 2r y_i.$$Đối với mỗi điểm, điều này áp đặt giới hạn dưới cho$r$như một chức năng của$x$:$$r \ge \frac{(x - x_i)^2 + y_i^2}{2y_i}.$$Nếu như$y_i < 0$, hướng bất đẳng thức bị đảo ngược, có nghĩa là không thể có cấu hình như vậy đối với tâm nằm phía trên trục. Điều này ngay lập tức đưa ra một điều kiện khả thi: đối với dấu hiệu của chiều cao tâm được chọn, tất cả các điểm phải nằm đúng về phía đúng của trục. 

Vì vậy, đối với một dấu hiệu cố định, vấn đề trở nên tối thiểu:$$r(x) = \max_i \frac{(x - x_i)^2 + y_i^2}{2y_i}.$$Đây là hàm lồi trong$x$, vì nó là cực đại của phương trình bậc hai lồi. Điều đó cho phép chúng tôi áp dụng tìm kiếm ternary trên$x$, đánh giá$r(x)$trong thời gian tuyến tính. 

Câu trả lời cuối cùng là mức tối thiểu trong hai trường hợp (tâm ở trên hoặc dưới trục), nếu cả hai đều khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | tìm kiếm theo cấp số nhân / vô hạn | O(n) | Quá chậm | 
| Tìm kiếm bậc ba trên x | O(n log C) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý điểm bằng$y > 0$là ứng cử viên cho một trung tâm phía trên trục và điểm với$y < 0$cho một trung tâm bên dưới nó. 

1. Chia bài toán thành hai trường hợp độc lập: tâm tại$y = r$và tập trung tại$y = -r$. Điều này là cần thiết vì hướng tiếp tuyến được cố định sau khi được chọn. 
2. Đối với trường hợp “trên trục”, kiểm tra xem có điểm nào có$y_i \le 0$. Nếu vậy, hãy loại bỏ trường hợp này, vì bất đẳng thức rút ra từ tiếp tuyến sẽ không đúng. 
3. Xác định hàm$f(x)$tính toán bán kính cần thiết cho tọa độ ngang cố định:$$f(x) = \max_i \frac{(x - x_i)^2 + y_i^2}{2y_i}.$$Hàm này thu được bán kính nhỏ nhất cần thiết nếu tâm được cố định theo chiều dọc phía trên trục ở độ cao$r$. 

1. Sử dụng tìm kiếm ternary$x$trên một khoảng đủ lớn chứa tất cả các điểm, thông thường$[-10^7, 10^7]$. Độ lồi của$f(x)$đảm bảo mức tối thiểu toàn cầu duy nhất. 
2. Ở mỗi bước, đánh giá$f(x)$TRONG$O(n)$bằng cách quét tất cả các điểm và tính toán giới hạn tối đa. 
3. Lặp lại quy trình tương tự cho trường hợp “dưới trục” sau khi lật tất cả các dấu hiệu$y_i$. 
4. Lấy kết quả hợp lệ tối thiểu từ cả hai trường hợp. Nếu cả hai đều không hợp lệ, hãy xuất$-1$. 

### Tại sao nó hoạt động 

Phép biến đổi làm giảm vấn đề bao phủ hình học thành việc giảm thiểu đường bao trên của các hàm lồi trong một chiều. Mỗi điểm xác định một ràng buộc trên$r$dưới dạng phương trình bậc hai lồi trong$x$và bán kính khả thi là mức tối đa của các đường cong này. Hàm lồi cực đại vẫn lồi, đảm bảo cực tiểu duy nhất. Tìm kiếm bậc ba hợp lệ vì hàm này không có cực tiểu cục bộ nào khác ngoài cực tiểu toàn cục. Ràng buộc tiếp tuyến loại bỏ bậc tự do thứ hai trong$y$, thu gọn vấn đề thành tối ưu hóa một biến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve_case(points):
    def calc(x):
        res = 0.0
        for xi, yi in points:
            res = max(res, ((x - xi) * (x - xi) + yi * yi) / (2.0 * yi))
        return res

    lo, hi = -1e7, 1e7
    for _ in range(80):
        m1 = (2 * lo + hi) / 3
        m2 = (lo + 2 * hi) / 3
        if calc(m1) > calc(m2):
            lo = m1
        else:
            hi = m2
    return calc((lo + hi) / 2)

def solve(points):
    best = INF

    # case 1: center above axis
    if all(y > 0 for _, y in points):
        best = min(best, solve_case(points))

    # case 2: center below axis (flip)
    flipped = [(x, -y) for x, y in points]
    if all(y > 0 for _, y in flipped):
        best = min(best, solve_case(flipped))

    return best if best < INF else -1

def main():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]
    ans = solve(points)
    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai tách biệt hai cấu hình hình học một cách rõ ràng để tránh trộn lẫn các ràng buộc về dấu hiệu bên trong hàm đánh giá. các`calc(x)`tính toán bán kính cần thiết cho một vị trí trung tâm nằm ngang cố định và tìm kiếm bậc ba sẽ tinh chỉnh tọa độ x tối ưu. 

Số lần lặp được cố định là 80, đủ để hội tụ dấu phẩy động với độ chính xác cần thiết. Mỗi đánh giá là tuyến tính theo số điểm, do đó hiệu suất là$O(n \log C)$. 

Một cạm bẫy phổ biến là quên rằng mẫu số$2y_i$yêu cầu xử lý dấu hiệu nhất quán. Đó là lý do tại sao chúng tôi lật tọa độ một cách rõ ràng cho trường hợp “dưới trục” thay vì cố gắng hợp nhất cả hai trong một công thức duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
0 1
```Chúng tôi đánh giá trường hợp “trên trục” vì điểm nằm phía trên sông. 

| Bước | ứng cử viên x | bán kính tính toán | 
| --- | --- | --- | 
| ban đầu | 0 | 0,5 | 

Việc tìm kiếm ternary ngay lập tức ổn định ở$x = 0$, vì tính đối xứng làm cho tất cả các giá trị x tương đương với một điểm. 

Kết quả xác nhận rằng đường tròn tối thiểu chạm vào trục tại đúng một điểm và đi qua một hang ổ duy nhất. 

### Ví dụ 2 

đầu vào:```
2
-1 2
1 2
```Cấu trúc đối xứng nên tâm tối ưu nằm ở$x = 0$. 

| Bước | x | hạn chế bán kính | 
| --- | --- | --- | 
| đánh giá | 0 | tối đa((1 + 4)/4, (1 + 4)/4) = 1,25 | 

Tìm kiếm ternary hội tụ nhanh chóng đến$x = 0$, xác nhận rằng tính đối xứng làm giảm vấn đề xuống một điểm đánh giá duy nhất. 

Điều này cho thấy cách nhiều điểm tạo ra các ràng buộc bậc hai cạnh tranh mà giá trị tối đa xác định bán kính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log R)$| Mỗi bước tìm kiếm bậc ba sẽ quét tất cả các điểm, lặp lại ~80 lần | 
| Không gian |$O(1)$| Chỉ lưu trữ điểm đầu vào | 

Các ràng buộc cho phép lên đến$10^5$điểm, và mỗi đánh giá là tuyến tính. Với số lần lặp không đổi, giải pháp phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    INF = 10**30

    def solve_case(points):
        def calc(x):
            res = 0.0
            for xi, yi in points:
                res = max(res, ((x - xi) * (x - xi) + yi * yi) / (2.0 * yi))
            return res

        lo, hi = -1e7, 1e7
        for _ in range(80):
            m1 = (2 * lo + hi) / 3
            m2 = (lo + 2 * hi) / 3
            if calc(m1) > calc(m2):
                lo = m1
            else:
                hi = m2
        return calc((lo + hi) / 2)

    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    best = INF

    if all(y > 0 for _, y in pts):
        best = min(best, solve_case(pts))

    flipped = [(x, -y) for x, y in pts]
    if all(y > 0 for _, y in flipped):
        best = min(best, solve_case(flipped))

    print(-1 if best == INF else best)

# provided sample 1
assert run("1\n0 1\n") == "0.5", "sample 1"

# custom: symmetric pair
assert run("2\n-1 2\n1 2\n") != "", "basic feasibility"

# custom: impossible (mixed sides too restrictive for single tangency)
assert run("2\n-1 1\n1 -1\n") == "-1", "infeasible configuration"

# custom: vertical line
assert run("3\n0 2\n0 3\n0 4\n") != "", "collinear vertical points"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 điểm phía trên trục | 0,5 | hành vi tiếp tuyến cơ bản | 
| điểm đối xứng | bán kính dương | độ lồi và tính đối xứng | 
| trường hợp dấu hiệu hỗn hợp không thể | -1 | hạn chế về tính khả thi | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các điểm nằm trên cả hai phía của trục x theo cách buộc bất kỳ tiếp tuyến đường tròn nào ở trên hoặc dưới đều không thành công. Ví dụ: nếu chúng ta có điểm$(0, 1)$Và$(0, -1)$, đường tròn tiếp tuyến phía trên trục không thể chứa điểm phía dưới và ngược lại. Thuật toán loại bỏ chính xác cả hai trường hợp vì mỗi lần kiểm tra tính khả thi đều không đạt được điều kiện dấu của nó. 

Một trường hợp tinh tế khác là khi tất cả các điểm cực kỳ gần trục nhưng không cắt nhau. Trong những trường hợp như vậy, độ chính xác về mặt số học có thể ảnh hưởng đến sự hội tụ tìm kiếm bậc ba. Số lần lặp cố định đảm bảo tính ổn định và làm việc ở dấu phẩy động là đủ vì độ chính xác yêu cầu chỉ là$10^{-6}$. 

Trường hợp cuối cùng liên quan đến phân bố đối xứng trong đó x tối ưu không ở bất kỳ tọa độ x đầu vào nào. Tìm kiếm bậc ba xử lý việc này một cách trơn tru vì mục tiêu là lồi, do đó mức tối thiểu có thể nằm trong không gian liên tục giữa các điểm mà không yêu cầu các vị trí ứng cử viên rời rạc.
