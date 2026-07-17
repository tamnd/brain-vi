---
title: "CF 103448B - bb \u53bb\u98df\u5802"
description: "Chúng ta có một tòa nhà hình chữ nhật trên mặt phẳng 2D, thẳng hàng với các trục tọa độ. Mỗi căng tin được biểu diễn dưới dạng một điểm duy nhất. Đối với mỗi căng tin, chúng tôi muốn tính khoảng cách Euclide ngắn nhất của nó tới bất kỳ điểm nào trên hình chữ nhật, bao gồm cả ranh giới và các góc bên trong của nó."
date: "2026-07-03T07:26:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103448
codeforces_index: "B"
codeforces_contest_name: "The 16-th Beihang University Collegiate Programming Contest (BCPC 2021) - Preliminary"
rating: 0
weight: 103448
solve_time_s: 40
verified: true
draft: false
---

[CF 103448B - bb \u53bb\u98df\u5802](https://codeforces.com/problemset/problem/103448/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tòa nhà hình chữ nhật trên mặt phẳng 2D, thẳng hàng với các trục tọa độ. Mỗi căng tin được biểu diễn dưới dạng một điểm duy nhất. Đối với mỗi căng tin, chúng tôi muốn tính khoảng cách Euclide ngắn nhất của nó tới bất kỳ điểm nào trên hình chữ nhật, bao gồm cả ranh giới và các góc bên trong của nó. Sau khi tính toán các khoảng cách này, chúng ta chọn căng tin có khoảng cách nhỏ nhất và nếu nhiều căng tin có cùng giá trị nhỏ nhất thì chúng ta chọn căng tin có chỉ số nhỏ nhất. 

Về mặt hình học, đây là yêu cầu khoảng cách từ một điểm đến một hình chữ nhật thẳng hàng với trục, hoạt động khác nhau tùy thuộc vào vị trí của điểm so với hình chữ nhật. Nếu điểm được căn chỉnh theo chiều ngang và chiều dọc với hình chiếu của hình chữ nhật thì điểm gần nhất nằm trên một cạnh. Nếu nó nằm chéo bên ngoài thì điểm gần nhất là góc. Nếu nó ở bên trong thì khoảng cách sẽ bằng 0, nhưng bài toán đảm bảo rõ ràng rằng không có căng tin nào nằm bên trong hoặc trên ranh giới, vì vậy mọi điểm đều hoàn toàn nằm bên ngoài. 

Ràng buộc n lên tới 100000 có nghĩa là chúng ta phải xử lý từng căng tin trong thời gian không đổi. Bất kỳ giải pháp nào liên quan đến hình học trên mỗi căng tin ngoài phép tính O(1) đều có thể chấp nhận được, nhưng bất kỳ giải pháp nào liên quan đến việc sắp xếp hoặc so sánh theo cặp đều không cần thiết và sẽ lãng phí. Cách tiếp cận hình học đơn giản chỉ phù hợp nếu nó giảm thời gian không đổi trên mỗi điểm. 

Trường hợp cạnh tinh tế xuất phát từ việc xử lý chính xác xem hình chiếu gần nhất nằm bên trong khoảng hình chữ nhật hay bên ngoài nó. Ví dụ: nếu một điểm nằm ngay phía trên hình chữ nhật thì khoảng cách gần nhất hoàn toàn là theo chiều dọc; nếu nó nằm theo đường chéo phía trên bên trái thì điểm gần nhất là góc trên cùng bên trái và khoảng cách là đường chéo. Phân loại sai những trường hợp này dẫn đến tính toán khoảng cách không chính xác. 

Một trường hợp khác là xử lý bình đẳng. Hai căng tin có thể có khoảng cách giống hệt nhau do tính đối xứng và trong trường hợp đó chúng ta phải bảo toàn cẩn thận chỉ số nhỏ nhất, nghĩa là chúng ta không thể cập nhật câu trả lời trừ khi khoảng cách mới nhỏ hơn hoàn toàn. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực rất đơn giản: đối với mỗi căng tin, chúng tôi xem xét khoảng cách của nó đến mọi điểm trên ranh giới hình chữ nhật. Vì hình chữ nhật là liên tục nên đây trở thành vấn đề tối thiểu hóa hình học hơn là phép liệt kê rời rạc. Người ta có thể thử lấy mẫu các cạnh hoặc chia hình chữ nhật thành các đoạn và tính toán khoảng cách từ điểm đến đoạn, nhưng đó là sự phức tạp không cần thiết nếu chúng ta nhận ra giải pháp dạng đóng tiêu chuẩn cho khoảng cách từ một điểm đến một hình chữ nhật thẳng hàng với trục. 

Quan sát quan trọng là hình chữ nhật tạo ra hành vi độc lập dọc theo trục x và y. Đối với bất kỳ điểm (x, y) nào, điểm gần nhất trên hình chữ nhật có được bằng cách kẹp x vào khoảng [X1, X2] và y vào [Y1, Y2]. Điều này tạo ra điểm gần nhất trên hoặc bên trong hình chữ nhật theo nghĩa khoảng cách Euclide. Khoảng cách sau đó được tính từ điểm ban đầu đến điểm được chiếu này. 

Điều này làm giảm vấn đề thành một phép tính đơn giản cho mỗi điểm: xây dựng điểm gần nhất (px, py) trong đó px là x được cắt vào phạm vi x của hình chữ nhật và py là y được cắt vào phạm vi y của nó, sau đó tính khoảng cách Euclide bình phương. Khoảng cách bình phương là đủ vì căn bậc hai là đơn điệu và không cần thiết để so sánh. 

Ý tưởng vũ lực vẫn sẽ là O(n) sau khi đơn giản hóa, nhưng cải tiến quan trọng là nhận ra rằng phép chiếu độc lập trên mỗi trục và thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm ranh giới hình học ngây thơ | O(n) với hằng số nặng hoặc tệ hơn | O(1) | Thực hành quá chậm | 
| Phương pháp kẹp tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng căng tin một cách độc lập và duy trì ứng viên tốt nhất cho đến nay.

1. Khởi tạo chỉ số câu trả lời là 1 và tính khoảng cách bình phương của nó với hình chữ nhật. Điều này thiết lập đường cơ sở để đảm bảo chúng tôi luôn có mục tiêu so sánh hợp lệ. Khoảng cách được tính bằng quy tắc chiếu được mô tả bên dưới. 
2. Với mỗi căng tin i từ 2 đến n, hãy tính điểm gần nhất của nó trên hình chữ nhật. Chúng tôi thực hiện điều này bằng cách kẹp tọa độ x của nó vào [X1, X2]. Nếu x nhỏ hơn X1 thì ta dùng X1; nếu lớn hơn X2 thì dùng X2; nếu không thì chúng ta giữ x không thay đổi. Logic tương tự áp dụng cho y với [Y1, Y2]. Bước này xây dựng điểm khả thi về mặt hình học gần nhất trên hình chữ nhật với căng tin đã cho. 
3. Tính bình phương khoảng cách giữa căng tin và điểm chiếu của nó. Khoảng cách bình phương là đủ vì thứ tự được giữ nguyên trong phép biến đổi đơn điệu và tránh các vấn đề về độ chính xác của dấu phẩy động. 
4. So sánh khoảng cách này với khoảng cách tốt nhất hiện tại. Nếu nó nhỏ hơn hoàn toàn, hãy cập nhật cả khoảng cách tốt nhất và chỉ số tốt nhất. Nếu nó bằng thì không làm gì cả vì chỉ mục trước đó phải được giữ nguyên. 
5. Sau khi xử lý tất cả các căng tin, xuất ra chỉ mục tốt nhất được lưu trữ. 

Tại sao nó hoạt động 

Điểm gần nhất trên hình chữ nhật thẳng hàng với trục lồi với điểm bên ngoài luôn thu được bằng cách giảm thiểu chuyển vị ngang và dọc một cách độc lập. Bất kỳ sai lệch nào so với tọa độ được kẹp đều làm tăng ít nhất một thành phần bình phương của khoảng cách. Điều này đảm bảo phép chiếu được xây dựng là tối ưu toàn cục cho khoảng cách Euclide. Vì mọi căng tin đều được đánh giá bằng phép chiếu tối ưu chính xác này nên mức tối thiểu đối với tất cả các ứng viên là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def clamp(v, lo, hi):
    if v < lo:
        return lo
    if v > hi:
        return hi
    return v

def dist2(x1, y1, x2, y2):
    dx = x1 - x2
    dy = y1 - y2
    return dx * dx + dy * dy

def solve():
    n = int(input())
    X1, Y1, X2, Y2 = map(int, input().split())

    best_idx = 1
    x0, y0 = map(int, input().split())

    px = clamp(x0, X1, X2)
    py = clamp(y0, Y1, Y2)
    best_dist = dist2(x0, y0, px, py)

    for i in range(2, n + 1):
        x, y = map(int, input().split())
        px = clamp(x, X1, X2)
        py = clamp(y, Y1, Y2)
        d = dist2(x, y, px, py)

        if d < best_dist:
            best_dist = d
            best_idx = i

    print(best_idx)

if __name__ == "__main__":
    solve()
```Đầu tiên, lời giải sẽ đọc hình chữ nhật và khởi tạo câu trả lời bằng căng tin đầu tiên. Điều này tránh cách viết hoa đặc biệt bên trong vòng lặp và đảm bảo các phép so sánh luôn có đường cơ sở hợp lệ. 

Hàm kẹp thực thi phép chiếu hình học lên khoảng hình chữ nhật trên mỗi trục. Hàm dist2 tránh hoàn toàn số học dấu phẩy động, điều quan trọng là các ràng buộc tọa độ tuy nhỏ nhưng các phép so sánh phải chính xác. 

Việc phá vỡ ràng buộc được xử lý hoàn toàn bằng cách chỉ cập nhật khi tìm thấy khoảng cách nhỏ hơn, bảo toàn chỉ số sớm nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

| tôi | (x, y) | chiếu (px, py) | phân chia | idx tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | (2, 9) | (2, 4) | 25 | 1 | 
| 2 | (-4, -4) | (0, 0) | 32 | 1 | 
| 3 | (1, -6) | (1, 0) | 36 | 1 | 
| 4 | (8, 7) | (4, 4) | 25 | 1 | 

Ở đây hình chữ nhật là từ (0,0) đến (4,4). Căng tin thứ nhất và thứ tư hòa nhau với khoảng cách bình phương là 25 nhưng chỉ số 1 được giữ nguyên do quy tắc hòa nhau. 

Một ví dụ thứ hai: 

đầu vào:```
1
0 0 10 10
12 5
```| tôi | (x, y) | chiếu (px, py) | phân chia | idx tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | (12, 5) | (10, 5) | 4 | 1 | 

Điều này xác nhận hành vi khi điểm nằm thẳng hàng theo chiều ngang với hình chữ nhật, trong đó chỉ có một tọa độ đóng góp vào khoảng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi căng tin yêu cầu kẹp thời gian không đổi và tính toán khoảng cách | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ | 

Quét tuyến tính lên tới 100000 điểm dễ dàng phù hợp với giới hạn thời gian, vì mỗi lần lặp chỉ sử dụng một vài phép tính số học và so sánh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-style test
assert run("""4
0 0 4 4
2 9
-4 -4
1 -6
8 7
""") == "1"

# minimum case
assert run("""1
0 0 1 1
2 2
""") == "1"

# horizontal alignment dominance
assert run("""3
0 0 10 10
5 20
-5 5
15 5
""") == "2"

# diagonal competition
assert run("""2
0 0 10 10
-1 -1
11 11
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất bên ngoài | 1 | trường hợp cơ sở đúng đắn | 
| sự thống trị ngang/dọc | 2 | hành vi kẹp đúng | 
| điểm chéo đối xứng | 1 | hòa theo chỉ số | 

## Vỏ cạnh 

Một lỗi phổ biến xảy ra khi logic chiếu được thay thế bằng khoảng cách tuyệt đối ngây thơ đến giới hạn hình chữ nhật. Ví dụ: coi khoảng cách là min(|x-X1|, |x-X2|) + min(|y-Y1|, |y-Y2|) là không chính xác vì hình học Euclide không phân tách cộng gộp. 

Lấy hình chữ nhật [0,0] đến [4,4] và điểm (-3, -3). Phép chiếu đúng là (0,0) và bình phương khoảng cách là 18. Một công thức sai có thể kết hợp sai khoảng cách trục theo cách đánh giá thấp chi phí đường chéo. 

Thuật toán xử lý việc này một cách chính xác vì cả hai tọa độ đều được kẹp độc lập, tạo ra điểm gần nhất thực sự (0,0). Khoảng cách bình phương được tính toán sẽ trở thành (−3)^2 + (−3)^2 = 18, khớp với mức tối thiểu Euclide thực trên hình chữ nhật.
