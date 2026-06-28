---
title: "CF 105109E - Có phải là Vinyl không?"
description: "Chúng ta có mười điểm trên mặt phẳng, mỗi điểm phải nằm trên ranh giới của một vật thể ẩn giấu. Đối tượng được đảm bảo thuộc chính xác một trong hai loại: bản ghi vinyl có ranh giới là hình tròn hoặc băng cassette có ranh giới là hình chữ nhật thẳng hàng với trục."
date: "2026-06-27T20:03:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105109
codeforces_index: "E"
codeforces_contest_name: "UTPC Spring 2024 Open Contest"
rating: 0
weight: 105109
solve_time_s: 79
verified: false
draft: false
---

[CF 105109E - Có phải là Vinyl không?](https://codeforces.com/problemset/problem/105109/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có mười điểm trên mặt phẳng, mỗi điểm phải nằm trên ranh giới của một vật thể ẩn giấu. Đối tượng được đảm bảo thuộc chính xác một trong hai loại: bản ghi vinyl có ranh giới là hình tròn hoặc băng cassette có ranh giới là hình chữ nhật thẳng hàng với trục. 

Nhiệm vụ là quyết định hình dạng nào phù hợp với các điểm quan sát được. Đối với một đường tròn, tất cả các điểm đều nằm cách một tâm một khoảng như nhau. Đối với hình chữ nhật thẳng hàng với trục, mọi điểm ranh giới nằm trên một trong bốn cạnh thẳng thẳng hàng với trục tọa độ, nghĩa là tọa độ x của nó là giá trị x tối thiểu hoặc tối đa của hình hoặc tọa độ y của nó là giá trị y tối thiểu hoặc tối đa. 

Kích thước đầu vào là cố định và nhỏ, chỉ có mười điểm. Điều này ngay lập tức loại trừ mọi lo ngại về độ phức tạp tiệm cận. Bất kỳ giải pháp nào cho đến tính toán hình học theo thời gian không đổi đều có thể chấp nhận được. 

Khó khăn chính không phải là hiệu suất mà là độ bền về số lượng. Tọa độ là các giá trị dấu phẩy động có tối đa 10 chữ số thập phân và có thể sai lệch so với hình dạng thực lên tới 1e-6. Điều này có nghĩa là bất kỳ bài kiểm tra hình học nào cũng phải chấp nhận các lỗi epsilon nhỏ. 

Một cách tiếp cận ngây thơ để kiểm tra sự bằng nhau chính xác của tọa độ hoặc sự bằng nhau chính xác của khoảng cách sẽ thất bại vì nhiễu dấu phẩy động có thể phá vỡ sự bằng nhau ngay cả khi cấu trúc đúng. 

Trường hợp cạnh tinh tế phát sinh khi các điểm gần như được căn chỉnh nhưng không chính xác với các cạnh hình chữ nhật. Ví dụ: điểm ranh giới hình chữ nhật có thể có x = 1.9999999 thay vì 2.0. Bất kỳ sự kiểm tra bình đẳng nghiêm ngặt nào cũng sẽ từ chối một băng cassette hợp lệ một cách không chính xác. 

Tương tự, đối với các đường tròn, việc tính toán tâm từ ba điểm bằng các công thức không ổn định có thể khuếch đại sai số nổi, gây ra bán kính không nhất quán trừ khi dung sai được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng tái tạo lại hình dạng một cách rõ ràng theo cả hai giả thuyết. 

Theo giả thuyết đường tròn, chúng ta có thể chọn bất kỳ ba điểm không thẳng hàng nào, tính đường tròn ngoại tiếp và sau đó kiểm tra xem có phải tất cả mười điểm đều nằm trên đó hay không. Điều này liên quan đến việc giải một hệ phương trình về tâm và bán kính đường tròn, sau đó kiểm tra tính nhất quán. Vì chỉ có mười điểm nên việc thử tất cả các bộ ba vẫn giữ nguyên thời gian không đổi. 

Theo giả thuyết hình chữ nhật, chúng ta có thể cố gắng suy ra các cạnh của hình chữ nhật bằng cách phân cụm các điểm thành bốn nhóm tương ứng với các cạnh hoặc thử thực hiện tất cả các phép gán điểm cho các cạnh. Điều này nhanh chóng trở nên không cần thiết vì hình chữ nhật thẳng hàng với trục có cấu trúc rất cứng nhắc: tất cả các điểm phải nằm trên các đường x = minX, x = maxX, y = minY hoặc y = maxY. 

Quan sát quan trọng là chúng ta không cần phải tái tạo lại cả hai hình dạng một cách đầy đủ. Chúng ta chỉ cần kiểm tra xem các điểm có thỏa mãn các ràng buộc về cấu trúc của hình chữ nhật hay không. Nếu có, chúng tôi xuất CASSETTE. Ngược lại, do vấn đề đảm bảo, hình dạng phải là hình tròn, vì vậy chúng tôi xuất ra VINYL. 

Điều này làm giảm vấn đề xuống còn việc xác thực hình học đơn giản của thành viên hộp giới hạn được căn chỉnh theo trục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái tạo hình tròn + tái tạo hình chữ nhật | O(1) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Chỉ kiểm tra tính hợp lệ của hình chữ nhật | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả mười điểm và lưu trữ tọa độ của chúng. Chúng tôi giữ chúng trong mảng dấu phẩy động vì cần có dung sai chính xác. 
2. Tính xmin, xmax, ymin, ymax trên tất cả các điểm. Chúng xác định hình chữ nhật giới hạn được căn chỉnh theo trục nhỏ nhất chứa tất cả các điểm. 
3. Đối với mỗi điểm, hãy kiểm tra xem nó có nằm trên ranh giới của hình chữ nhật này hay không. Một điểm hợp lệ nếu tọa độ x của nó xấp xỉ bằng xmin hoặc xmax, hoặc tọa độ y của nó xấp xỉ bằng ymin hoặc ymax. Chúng tôi sử dụng so sánh epsilon thay vì đẳng thức chính xác để xử lý nhiễu nổi. 
4. Nếu mọi điểm đều thỏa mãn điều kiện biên, hãy phân loại hình dạng đó thành hình chữ nhật và xuất CASSETTE. 
5. Ngược lại, xuất VINYL. 

Lý do điều này có tác dụng là vì bất kỳ điểm ranh giới hình chữ nhật thẳng hàng theo trục nào cũng phải thỏa mãn ít nhất một trong bốn đẳng thức đó. Các điểm bên trong là không thể theo định nghĩa của “điểm biên”, vì vậy mọi điểm đầu vào hợp lệ đều phải nằm trên một cạnh. 

### Tại sao nó hoạt động 

Đối với một băng cassette chính xác, tất cả các điểm quan sát đều nằm trên giao điểm của bốn đường xác định các cạnh hình chữ nhật. Hộp giới hạn được tính toán từ các điểm khớp với hình chữ nhật thực vì ít nhất một điểm đạt được từng tọa độ cực trị trong phạm vi dung sai. Do đó, mọi điểm phải phù hợp với một trong bốn phương trình biên. 

Đối với vinyl, các điểm nằm trên một vòng tròn và thường không thỏa mãn bất kỳ ràng buộc cực đoan nào về trục thẳng hàng một cách nhất quán. Mặc dù một hình tròn có thể được bao bọc trong một hình chữ nhật, nhưng các điểm biên của nó không bị giới hạn ở bốn đường thẳng đó, do đó, ít nhất một điểm sẽ vi phạm điều kiện hình chữ nhật, gây ra sự bác bỏ giả thuyết hình chữ nhật. 

Vì bài toán đảm bảo chính xác một hình hợp lệ nên việc từ chối hình chữ nhật sẽ hàm ý hình tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

EPS = 1e-6

def close(a, b):
    return abs(a - b) <= EPS

points = []
for _ in range(10):
    x, y = map(float, input().split())
    points.append((x, y))

xs = [p[0] for p in points]
ys = [p[1] for p in points]

xmin, xmax = min(xs), max(xs)
ymin, ymax = min(ys), max(ys)

def on_rect_boundary(x, y):
    return close(x, xmin) or close(x, xmax) or close(y, ymin) or close(y, ymax)

is_rect = True
for x, y in points:
    if not on_rect_boundary(x, y):
        is_rect = False
        break

print("CASSETTE" if is_rect else "VINYL")
```Đầu tiên, mã sẽ tính toán hộp giới hạn của tất cả các điểm, sau đó xác minh điều kiện cấu trúc xác định ranh giới hình chữ nhật được căn chỉnh theo trục. Chức năng trợ giúp`close`hấp thụ lỗi dấu phẩy động lên tới 1e-6 theo yêu cầu của câu lệnh. 

Bước quyết định có chủ ý không đối xứng: chúng tôi chỉ xác nhận rõ ràng trường hợp hình chữ nhật. Điều này tránh việc lắp vòng tròn không ổn định và đảm bảo rằng chính xác một trong hai hình là hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Điểm đầu vào tương ứng với một vòng tròn. 

| Bước | kết quả kiểm tra xmin/xmax/ymin/ymax | 
| --- | --- | 
| Tính giới hạn | hình chữ nhật bao quanh các điểm tròn | 
| Xác thực điểm | ít nhất một điểm vi phạm điều kiện biên | 
| Quyết định cuối cùng | không phải hình chữ nhật | 

Các điểm vòng tròn trải dài liên tục dọc theo một đường cong, do đó một số điểm nằm hoàn toàn bên trong các cạnh của khung giới hạn. Những điểm đó không đạt trong bài kiểm tra hình chữ nhật, buộc phải phân loại là VINYL. 

### Mẫu 2 

Điểm đầu vào nằm trên một ranh giới hình chữ nhật thẳng hàng với trục. 

| Bước | kết quả kiểm tra xmin/xmax/ymin/ymax | 
| --- | --- | 
| Tính giới hạn | hình chữ nhật chính xác được phục hồi | 
| Xác thực điểm | mọi điểm khớp với một cạnh | 
| Quyết định cuối cùng | hình chữ nhật được xác nhận | 

Mọi điểm đều thẳng hàng với cạnh dọc hoặc cạnh ngang của hộp giới hạn được suy ra, vì vậy tất cả các kiểm tra đều đạt và kết quả là CASSETTE. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10) | Chúng tôi quét một số điểm cố định với số lần không đổi | 
| Không gian | O(10) | Chúng tôi chỉ lưu trữ các điểm đầu vào | 

Các ràng buộc làm cho thời gian này không đổi một cách hiệu quả. Ngay cả một cách tiếp cận tái thiết hình học hơn cũng sẽ đủ nhanh, nhưng thử nghiệm hộp giới hạn đơn giản hơn và ổn định về mặt số trong dung sai cho trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    EPS = 1e-6

    def close(a, b):
        return abs(a - b) <= EPS

    points = []
    for _ in range(10):
        x, y = map(float, input().split())
        points.append((x, y))

    xs = [p[0] for p in points]
    ys = [p[1] for p in points]

    xmin, xmax = min(xs), max(xs)
    ymin, ymax = min(ys), max(ys)

    def on_rect_boundary(x, y):
        return close(x, xmin) or close(x, xmax) or close(y, ymin) or close(y, ymax)

    for x, y in points:
        if not on_rect_boundary(x, y):
            return "VINYL"
    return "CASSETTE"

# sample-like circle (rough)
assert run("0 1\n1 0\n0 -1\n-1 0\n0.7 0.7\n-0.7 0.7\n-0.7 -0.7\n0.7 -0.7\n0.5 0.86\n-0.5 -0.86\n") == "VINYL"

# perfect rectangle
assert run("0 0\n0 1\n0 2\n0 3\n1 0\n1 1\n1 2\n1 3\n0.5 0\n0.5 3\n") == "CASSETTE"

# near-rectangle with noise
assert run("0 0\n0 1\n0 2\n0 3\n1 0\n1 1\n1 2\n1 3\n0.5000005 0\n0.4999995 3\n") == "CASSETTE"

# diagonal-ish circle-ish points
assert run("0 0\n1 1\n2 0\n1 -1\n0.7 0.7\n1.3 0.7\n1.3 -0.7\n0.7 -0.7\n1 0.2\n1 -0.2\n") == "VINYL"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòng tròn ồn ào | VINYL | điều kiện hình chữ nhật không thành công trong cấu trúc không trục | 
| hình chữ nhật sạch | CASSETTE | thỏa mãn mọi ràng buộc biên | 
| hình chữ nhật ồn ào | CASSETTE | epsilon xử lý đúng đắn | 
| hình dạng tùy ý | VINYL | từ chối các bộ không phải hình chữ nhật | 

## Vỏ cạnh 

Một kiểu thất bại điển hình là xử lý đẳng thức thả nổi quá nghiêm ngặt. Trong đầu vào gần hình chữ nhật trong đó x phải chính xác bằng 0 nhưng xuất hiện dưới dạng 1e-7, việc kiểm tra đẳng thức nghiêm ngặt sẽ từ chối một băng cassette hợp lệ một cách không chính xác. Sự so sánh dựa trên epsilon trong`close`đảm bảo các điểm đó vẫn khớp với xmin hoặc xmax. 

Một trường hợp cạnh khác là giả sử các điểm phải chính xác là các góc. Vấn đề cho phép các điểm ở bất cứ đâu dọc theo các cạnh. Thuật toán chấp nhận chính xác các điểm như (xmin, 0,3) hoặc (0,7, ymax) vì nó chỉ yêu cầu thành viên trên bất kỳ đường biên nào chứ không phải trùng khớp góc. 

Trường hợp cạnh cuối cùng là các hộp giới hạn suy biến trong đó xmin bằng xmax hoặc ymin bằng ymax do sự sụp đổ về số. Việc đảm bảo đầu vào rằng các điểm khác biệt với khoảng cách ít nhất 0,05 sẽ ngăn chặn tình trạng này, do đó hình chữ nhật vẫn được xác định rõ.
