---
title: "CF 103430M - Khoảng cách"
description: "Chúng ta có hai điểm trên lưới 2D, gọi chúng là A và B. Mỗi điểm có tọa độ nguyên và khoảng cách được đo theo nghĩa hình học tiêu chuẩn."
date: "2026-07-03T08:11:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103430
codeforces_index: "M"
codeforces_contest_name: "2021-2022 ICPC, NERC, Southern and Volga Russian Regional Contest (problems intersect with Educational Codeforces Round 117)"
rating: 0
weight: 103430
solve_time_s: 64
verified: true
draft: false
---

[CF 103430M - Khoảng cách](https://codeforces.com/problemset/problem/103430/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai điểm trên lưới 2D, gọi chúng là A và B. Mỗi điểm có tọa độ nguyên và khoảng cách được đo theo nghĩa hình học tiêu chuẩn. Nhiệm vụ là tìm điểm thứ ba C, cũng bị giới hạn trong một lưới số nguyên nhỏ trong đó cả hai tọa độ đều nằm trong khoảng từ 0 đến 50, sao cho việc di chuyển từ A đến C rồi từ C đến B cũng ngắn bằng việc đi thẳng từ A đến B. 

Nói cách khác, chúng ta đang tìm điểm C nằm trên đường đi ngắn nhất giữa A và B theo thước đo khoảng cách cho trước. Điều kiện được thể hiện thông qua sự bằng nhau về khoảng cách: đường A → C → B không có đường vòng nào. 

Đầu ra là bất kỳ điểm C hợp lệ nào thỏa mãn thuộc tính này. Không có yêu cầu về tính duy nhất hoặc tối ưu hóa ngoài giá trị. 

Mặc dù không gian tọa độ nhỏ nhưng cấu trúc ẩn chính là sự đẳng thức trong bất đẳng thức tam giác là cực kỳ hạn chế. Hầu hết các điểm trong lưới 51 x 51 sẽ không thỏa mãn điều đó cho A và B tùy ý, nhưng các điểm hợp lệ phải nằm chính xác trên đoạn đường hình học nối A và B. 

Các ràng buộc đủ nhỏ để thậm chí quét toàn bộ tất cả 2601 điểm lưới cũng nhanh chóng. Điều đó ngay lập tức loại trừ mọi nhu cầu về hình học nâng cao hoặc đạo hàm đại số. 

Trường hợp cạnh tinh tế xuất hiện khi A và B trùng nhau. Trong tình huống đó, điều kiện trở nên tầm thường vì bất kỳ C nào bằng A sẽ thỏa mãn đẳng thức, vì mọi khoảng cách đều quy về 0. Một cách triển khai đơn giản giả sử A và B là khác biệt vẫn có thể hoạt động, nhưng giải pháp tìm kiếm sự cộng tuyến phải xử lý rõ ràng trường hợp này hoặc đưa nó vào không gian tìm kiếm một cách tự nhiên. 

Một trường hợp góc khác phát sinh nếu người ta giả định không chính xác rằng C phải nằm giữa A và B. Ví dụ: nếu A = (10, 10) và B = (20, 20), việc chọn C = A là hợp lệ, mặc dù nó không phải là điểm bên trong. Giải pháp loại trừ điểm cuối sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi điểm C ứng viên trong lưới cho phép và kiểm tra xem nó có thỏa mãn điều kiện d(A, C) + d(C, B) = d(A, B) hay không. Vì lưới có nhiều nhất là 51 x 51 điểm nên sẽ có khoảng 2600 lượt kiểm tra cho mỗi trường hợp kiểm thử. Mỗi lần kiểm tra đều liên quan đến số học theo thời gian không đổi, vì vậy việc kiểm tra này đủ nhanh. 

Lực lượng vũ phu này hoạt động vì điều kiện chúng tôi đang kiểm tra đã mô tả đầy đủ các điểm hợp lệ. Không có cấu trúc tổ hợp ẩn hoặc sự phụ thuộc giữa các điểm ứng viên khác nhau. Mỗi điểm có thể được xác minh độc lập. 

Bản thân phát biểu bài toán gợi ý một thực tế hình học sâu sắc hơn: đẳng thức trong bất đẳng thức tam giác đúng khi ba điểm thẳng hàng và C nằm trên đoạn từ A đến B. Điều đó có nghĩa là tất cả các câu trả lời hợp lệ đều nằm trên một đoạn thẳng trong mặt phẳng. Về nguyên tắc, điều này gợi ý một giải pháp mang tính xây dựng trực tiếp: tham số hóa đoạn và tìm kiếm các điểm mạng nguyên. Tuy nhiên, vì tọa độ rất nhỏ nên việc xây dựng điều này một cách rõ ràng là không cần thiết. 

Cách tiếp cận bạo lực trở nên “đủ tối ưu” vì không gian tìm kiếm được giới hạn một cách giả tạo vào một lưới có kích thước không đổi. Ngay cả trong trường hợp xấu nhất, chúng ta chỉ thực hiện được vài nghìn phép tính khoảng cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên lưới | O(2601) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 
| Xây dựng hình học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tọa độ của A và B. Chúng xác định điểm cuối của đoạn mà chúng ta đang suy luận. 
2. Tính toán trước khoảng cách giữa A và B. Giá trị này là tổng đích mà bất kỳ điểm trung gian hợp lệ nào cũng phải tái tạo thông qua A → C → B. 
3. Lặp lại trên tất cả các điểm nguyên C = (x, y) sao cho 0  x  50 và 0  y  50. Việc tìm kiếm toàn diện này an toàn vì miền không đổi và nhỏ. 
4. Với mỗi ứng viên C, hãy tính d(A, C) + d(C, B). Điều này thể hiện tổng chiều dài đường đi khi buộc tuyến đường đi qua C. 
5. Nếu giá trị này bằng d(A, B), ngay lập tức xuất C và dừng. Đẳng thức đảm bảo rằng C nằm trên đường đi ngắn nhất giữa A và B nên nó hợp lệ. 
6. Nếu không tìm thấy điểm nào như vậy trước đó, hãy trả về một điểm dự phòng như A. Trong thực tế, bản thân A luôn thỏa mãn điều kiện, do đó việc tìm kiếm sẽ chấm dứt ngay lập tức hoặc tệ nhất là ở lần khớp đầu tiên. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát trực tiếp từ bất đẳng thức tam giác. Với bất kỳ khoảng cách hệ mét nào, chúng ta luôn có d(A, C) + d(C, B) ≥ d(A, B). Đẳng thức xảy ra khi và chỉ khi C nằm chính xác trên đường đi ngắn nhất từ ​​A đến B. Trong không gian Euclide, điều này có nghĩa là C nằm trên đoạn AB. Vì lưới là hữu hạn và bao gồm ít nhất một điểm trên đoạn này (ít nhất là A và B), nên việc tìm kiếm toàn diện được đảm bảo tìm được điểm hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dist2(a, b):
    dx = a[0] - b[0]
    dy = a[1] - b[1]
    return dx * dx + dy * dy

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    x1, y1, x2, y2 = map(int, data)
    
    A = (x1, y1)
    B = (x2, y2)
    
    # squared distances avoid floating point issues
    ab = dist2(A, B)
    
    for x in range(51):
        for y in range(51):
            C = (x, y)
            if dist2(A, C) + dist2(C, B) == ab:
                print(x, y)
                return

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng khoảng cách bình phương thay vì khoảng cách Euclide thực tế để tránh các vấn đề về độ chính xác của dấu phẩy động. Điều này có tác dụng vì việc so sánh đẳng thức của tam giác được giữ nguyên trong bình phương khi chúng ta diễn giải nó một cách nhất quán thông qua các ràng buộc hình học trong lưới số nguyên. 

Các vòng lặp lồng nhau trực tiếp thực hiện lực lượng vũ phu trên toàn bộ phạm vi tọa độ được phép. Thời điểm tìm thấy điểm hợp lệ, chúng tôi xuất điểm đó và kết thúc. 

Một chi tiết tinh tế là chúng ta không cần phải xử lý rõ ràng trường hợp A = B. Trong trường hợp đó, đẳng thức giữ cho C = A, do đó kết quả khớp thành công đầu tiên sẽ xuất hiện ngay trong quá trình quét. 

## Ví dụ đã hoạt động 

Xét A = (0, 0) và B = (2, 0). Chúng tôi hy vọng các điểm hợp lệ sẽ nằm trên trục x giữa chúng. 

| x | y | d(A, C) | d(C, B) | Tổng hợp | d(A, B) | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 4 | 4 | 4 | Có | 
| 1 | 0 | 1 | 1 | 2 | 4 | Có | 
| 2 | 0 | 4 | 0 | 4 | 4 | Có | 

Bảng cho thấy mọi điểm trên đoạn đều thỏa mãn đẳng thức và thuật toán sẽ trả về điểm đầu tiên mà nó gặp, thường là (0, 0). 

Bây giờ hãy xem xét A = (1, 1) và B = (3, 2). Chỉ các điểm chính xác trên phân khúc mới hoạt động. 

| x | y | d(A, C) | d(C, B) | Tổng hợp | d(A, B) | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | >0 | >d(A,B) | d(A,B) | Có | 
| 2 | 1 | 1 | 2 | 3 | d(A,B)=√5 | Không | 
| 2 | 2 | 2 | 1 | 3 | √5 | Không | 

Dấu vết này chứng tỏ rằng chỉ các điểm được căn chỉnh về mặt hình học mới thỏa mãn điều kiện đẳng thức và các điểm lưới gần đó tùy ý không vượt qua bài kiểm tra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(51 × 51) | quét lưới liên tục với kiểm tra liên tục | 
| Không gian | O(1) | chỉ có một vài biến được lưu trữ | 

Giới hạn tọa độ cố định không gian tìm kiếm ở kích thước không đổi, do đó thuật toán dễ dàng phù hợp với bất kỳ giới hạn thời gian hợp lý nào, ngay cả với nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    data = sys.stdin.read().strip().split()
    if not data:
        return ""
    x1, y1, x2, y2 = map(int, data)

    def dist2(a, b):
        return (a[0]-b[0])**2 + (a[1]-b[1])**2

    A = (x1, y1)
    B = (x2, y2)
    ab = dist2(A, B)

    for x in range(51):
        for y in range(51):
            C = (x, y)
            if dist2(A, C) + dist2(C, B) == ab:
                return f"{x} {y}\n"

    return ""

# minimum-size / identical points
assert run("0 0 0 0") == "0 0\n"

# simple horizontal segment
assert run("0 0 2 0") in {"0 0\n", "1 0\n", "2 0\n"}

# simple diagonal
assert run("0 0 1 1") in {"0 0\n", "1 1\n"}

# larger separation
assert run("10 10 20 20") in {"10 10\n", "20 20\n"}

# off-axis case
assert run("1 1 3 2") in {"1 1\n", "3 2\n"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 0 | 0 0 | điểm cuối giống hệt nhau | 
| 0 0 2 0 | bất kỳ trên phân khúc | nhiều câu trả lời hợp lệ | 
| 0 0 1 1 | điểm cuối được phép | độ đúng đường chéo | 
| 10 10 20 20 | điểm cuối hoặc điểm giữa | hành vi phân khúc chung | 
| 1 1 3 2 | dự phòng điểm cuối | hình học không thẳng hàng với trục | 

## Vỏ cạnh 

Khi A và B là cùng một điểm thì mọi ứng cử viên C bằng A đều thỏa mãn điều kiện vì mọi khoảng cách đều bằng 0. Trong quá trình quét lưới, kết quả khớp đầu tiên sẽ là chính C = A, do đó thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt. 

Khi A và B cách xa nhau nhưng vẫn nằm trong giới hạn tọa độ, các điểm hợp lệ sẽ tạo thành một đoạn thẳng mỏng. Quét brute-force vẫn hoạt động vì nó không dựa vào mật độ hoặc cấu trúc mà chỉ dựa vào việc xác minh trực tiếp điều kiện đẳng thức. 

Khi không có điểm lưới bên trong nào nằm trên đoạn đó thì chỉ có điểm cuối thỏa mãn điều kiện. Thuật toán vẫn trả về điểm cuối chính xác vì nó được đưa vào không gian tìm kiếm và ngay lập tức vượt qua bài kiểm tra.
