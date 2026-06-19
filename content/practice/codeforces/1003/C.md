---
title: "CF 1003C - Nhiệt Độ Cao"
description: "Chúng ta được cung cấp một chuỗi nhiệt độ hàng ngày và được yêu cầu đánh giá tất cả các khoảng thời gian liền kề có độ dài ít nhất là một ngưỡng nhất định. Đối với mỗi khoảng thời gian như vậy, chúng tôi tính toán nhiệt độ trung bình của nó, nghĩa là tổng các giá trị của nó chia cho chiều dài của nó."
date: "2026-06-16T23:30:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1003
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 494 (Div. 3)"
rating: 1300
weight: 1003
solve_time_s: 90
verified: false
draft: false
---

[CF 1003C - Nhiệt độ cao](https://codeforces.com/problemset/problem/1003/C) 

**Đánh giá:** 1300 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhiệt độ hàng ngày và được yêu cầu đánh giá tất cả các khoảng thời gian liền kề có độ dài ít nhất là một ngưỡng nhất định. Đối với mỗi khoảng thời gian như vậy, chúng tôi tính toán nhiệt độ trung bình của nó, nghĩa là tổng các giá trị của nó chia cho chiều dài của nó. Nhiệm vụ là tìm giá trị trung bình tối đa có thể có trong tất cả các khoảng hợp lệ. 

Vì vậy, đối tượng chúng tôi đang tối ưu hóa không phải là kích thước cửa sổ cố định mà là mọi phân đoạn có độ dài bắt đầu từ giá trị tối thiểu và có thể mở rộng đến toàn bộ mảng. Điều này ngay lập tức làm cho vấn đề trở nên phức tạp hơn so với mức trung bình tối đa của cửa sổ cố định, bởi vì các phân đoạn dài hơn có thể đánh đổi mật độ tổng thấp hơn trong một khoảng thời gian dài hơn. 

Ràng buộc kích thước đầu vào cho phép lên tới 5000 ngày. Một cách tiếp cận hình khối ngây thơ đối với tất cả các mảng con và tính toán lại tổng sẽ là đường biên. Ngay cả một vòng lặp kép có tổng được tính toán lại cũng sẽ quá chậm trừ khi được tối ưu hóa bằng tổng tiền tố. Tuy nhiên, ngay cả với tổng tiền tố, việc liệt kê tất cả các phân đoạn O(n²) vẫn khả thi ở mức n = 5000, vì Python có thể chấp nhận được khoảng 25 triệu thao tác. 

Khó khăn tiềm ẩn là chúng ta đang tối ưu hóa một tỷ lệ chứ không phải một hàm tuyến tính. Điều này có nghĩa là đoạn tốt nhất không nhất thiết phải là một trong những đoạn ngắn nhất hoặc dài nhất được phép và không có cấu trúc đơn điệu về độ dài đoạn. 

Một số trường hợp đặc biệt bộc lộ những ý tưởng tham lam không chính xác. 

Nếu tất cả nhiệt độ đều bằng nhau thì mọi phân đoạn đều có mức trung bình như nhau, do đó bất kỳ phân đoạn hợp lệ nào đều là tối ưu. Một giải pháp đúng không được phụ thuộc vào việc chọn một độ dài cụ thể. 

Nếu mảng tăng nghiêm ngặt thì giá trị trung bình tốt nhất có thể đến từ một phân đoạn hậu tố ngắn thay vì tiền tố dài, vì việc bao gồm các giá trị thấp trước đó sẽ kéo giá trị trung bình xuống. 

Nếu k bằng n, câu trả lời đơn giản là giá trị trung bình của toàn bộ mảng. Bất kỳ giải pháp nào đòi hỏi sự linh hoạt trong việc lựa chọn độ dài đoạn vẫn phải xử lý ranh giới này một cách rõ ràng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi xem xét mọi chỉ mục bắt đầu, sau đó mở rộng chỉ mục kết thúc và chỉ đánh giá các đoạn có độ dài ít nhất là k. Đối với mỗi phân đoạn ứng viên, chúng tôi tính tổng và chia cho độ dài của nó, theo dõi giá trị tối đa. 

Với tổng tiền tố, chúng ta có thể tính bất kỳ tổng phân đoạn nào trong O(1). Điều này làm giảm tổng độ phức tạp xuống O(n2), vì có khoảng n2/2 phân đoạn. Với n = 5000, đây là khoảng 12,5 triệu đánh giá, mỗi đánh giá bao gồm một vài phép tính số học, có thể chấp nhận được. 

Quan sát quan trọng là mặc dù chúng ta không thể tránh việc kiểm tra nhiều phân đoạn nhưng chúng ta có thể tránh việc tính toán lại các tổng nhiều lần. Cấu trúc tỷ lệ không cho phép thủ thuật tìm kiếm nhị phân hoặc cắt tỉa nhanh hơn vì mục tiêu không đơn điệu về độ dài đoạn hoặc điểm cuối. 

Do đó, giải pháp tối ưu về cơ bản là ý tưởng mạnh mẽ được thực hiện hiệu quả với các tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tổng tiền tố trên tất cả các phân đoạn | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng tổng tiền tố trong đó prefix[i] lưu trữ tổng của i phần tử đầu tiên. Điều này cho phép bất kỳ tổng phân đoạn nào được tính toán trong thời gian không đổi. 
2. Lặp lại tất cả các điểm cuối bên trái có thể có l từ 1 đến n. Mỗi lựa chọn sẽ ấn định điểm bắt đầu của phân khúc ứng cử viên. 
3. Với mỗi l, lặp qua tất cả các điểm cuối bên phải r từ l + k - 1 đến n. Điều này đảm bảo mọi đoạn đều có độ dài ít nhất là k. 
4. Với mỗi cặp (l, r), hãy tính tổng phân đoạn bằng cách sử dụng tiền tố[r] − tiền tố[l − 1]. 
5. Tính giá trị trung bình bằng cách chia tổng cho (r − l + 1). 
6. Theo dõi mức trung bình tối đa nhìn thấy trên tất cả các phân đoạn hợp lệ. 

Lý do chúng ta bắt đầu r từ l + k − 1 là vì các phân đoạn ngắn hơn không được phép và sẽ chỉ giới thiệu các ứng cử viên không hợp lệ mà không đóng góp vào câu trả lời. 

### Tại sao nó hoạt động

Mỗi phân đoạn hợp lệ được biểu diễn duy nhất bằng một cặp điểm cuối (l, r) với r − l + 1 ≥ k. Thuật toán liệt kê tất cả các cặp như vậy chính xác một lần. Vì mức trung bình được tính toán chính xác cho từng phân đoạn và mức tối đa được lấy trên một tập hợp đầy đủ các ứng cử viên nên kết quả phải khớp với mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    best = 0.0

    for l in range(n):
        for r in range(l + k - 1, n):
            total = pref[r + 1] - pref[l]
            length = r - l + 1
            avg = total / length
            if avg > best:
                best = avg

    print(best)

if __name__ == "__main__":
    solve()
```Mảng tổng tiền tố được xây dựng sao cho mỗi truy vấn tổng phạm vi trở thành một phép trừ duy nhất. Điều này rất quan trọng vì nếu không có nó thì vòng lặp bên trong sẽ trở thành O(n³). Sau đó, các vòng lặp lồng nhau sẽ liệt kê tất cả các phân đoạn hợp lệ đồng thời tôn trọng giới hạn độ dài tối thiểu trực tiếp trong phạm vi chỉ mục. 

Phép chia dấu phẩy động là an toàn vì độ chính xác cần thiết là 1e-6 và độ chính xác kép của Python đủ để tính tổng lên tới khoảng 5000 × 5000. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
3 4 1 2
```Chúng tôi tính toán tổng tiền tố: 

| tôi | trước[i] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 3 | 
| 2 | 7 | 
| 3 | 8 | 
| 4 | 10 | 

Bây giờ chúng tôi liệt kê các đoạn có độ dài ít nhất là 3. 

| tôi | r | tổng hợp | chiều dài | trung bình | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 8 | 3 | 2.6667 | 
| 0 | 3 | 10 | 4 | 2,5 | 
| 1 | 3 | 7 | 3 | 2.3333 | 

Tối đa là 8/3 = 2,6667, đạt được nhờ ba yếu tố đầu tiên. 

Điều này xác nhận rằng đoạn tối ưu không nhất thiết phải là đoạn dài nhất được phép. 

### Ví dụ 2 

đầu vào:```
5 2
1 100 1 1 100
```Tổng tiền tố: 

| tôi | trước[i] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 101 | 
| 3 | 102 | 
| 4 | 103 | 
| 5 | 203 | 

Xem xét các phân khúc chính: 

| tôi | r | tổng hợp | chiều dài | trung bình | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 101 | 2 | 50,5 | 
| 3 | 4 | 101 | 2 | 50,5 | 
| 1 | 4 | 202 | 4 | 50,5 | 

Tất cả các ứng cử viên tối ưu hòa nhau ở mức 50,5. 

Điều này cho thấy rằng nhiều độ dài phân đoạn có thể tạo ra mức trung bình tối ưu giống hệt nhau, do đó thuật toán không được coi là duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Hai vòng lặp lồng nhau trên tất cả các điểm cuối phân đoạn hợp lệ | 
| Không gian | O(n) | Mảng tổng tiền tố | 

Với n lên tới 5000, thuật toán thực hiện khoảng 12,5 triệu lần lặp, phù hợp thoải mái trong giới hạn thời gian trong Python khi các phép toán là số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    best = 0.0
    for l in range(n):
        for r in range(l + k - 1, n):
            total = pref[r + 1] - pref[l]
            best = max(best, total / (r - l + 1))

    return str(best)

assert run("4 3\n3 4 1 2\n")[:5] == "2.666", "sample 1"
assert run("3 3\n1 2 3\n") == "2.0", "whole array only"

assert run("5 1\n5 5 5 5 5\n") == "5.0", "all equal"

assert run("5 2\n1 100 1 1 100\n")[:4] == "50.5", "multiple peaks"

assert run("1 1\n7\n") == "7.0", "single element"

assert run("6 3\n1 2 3 4 5 6\n")[:4] == "4.5", "increasing array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 7 | ranh giới tối thiểu | 
| tất cả đều bình đẳng | 5 | ổn định đồng đều | 
| nhiều đỉnh | 50,5 | tối ưu không duy nhất | 
| mảng tăng dần | 4,5 | sự cân bằng giữa đoạn dài và đoạn ngắn | 

## Vỏ cạnh 

Khi n bằng k, cấu trúc vòng lặp vẫn hoạt động vì với mỗi l có đúng một r hợp lệ. Thuật toán tính toán trung bình của toàn bộ mảng một cách hiệu quả trong trường hợp đó. 

Đối với chuỗi tăng nghiêm ngặt như [1, 2, 3, 4, 5], đoạn có độ dài tốt nhất ít nhất k = 2 không nhất thiết phải là mảng đầy đủ. Việc liệt kê kiểm tra các phân đoạn như [4, 5] có mức trung bình là 4,5, đánh bại các phân đoạn dài hơn như [1, 2, 3, 4, 5] với mức trung bình là 3. Điều này xác nhận rằng việc hạn chế chú ý đến các phân đoạn đầy đủ sẽ không chính xác và việc liệt kê đầy đủ là cần thiết.
