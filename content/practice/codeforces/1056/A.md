---
title: "CF 1056A - Xác định dòng"
description: "Chúng ta được cung cấp một chuỗi các khoảnh khắc trong khi đi xe điện, trong đó mỗi khoảnh khắc tương ứng với một điểm dừng mà Arkady quan sát ngắn gọn những tuyến xe điện nào phục vụ điểm dừng đó. Mỗi điểm dừng liệt kê một tập hợp các mã định danh dòng và Arkady ghi nhớ toàn bộ tại mỗi điểm dừng được quan sát."
date: "2026-06-15T09:54:52+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1056
codeforces_index: "A"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 3"
rating: 800
weight: 1056
solve_time_s: 205
verified: true
draft: false
---

[CF 1056A - Xác định đường](https://codeforces.com/problemset/problem/1056/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các khoảnh khắc trong khi đi xe điện, trong đó mỗi khoảnh khắc tương ứng với một điểm dừng mà Arkady quan sát ngắn gọn những tuyến xe điện nào phục vụ điểm dừng đó. Mỗi điểm dừng liệt kê một tập hợp các mã định danh dòng và Arkady ghi nhớ toàn bộ tại mỗi điểm dừng được quan sát. 

Nhiệm vụ là xác định xem Arkady có thể đã đi tuyến xe điện nào. Một đường có giá trị nếu nó xuất hiện ở mọi điểm dừng được quan sát, bởi vì xe điện phải đi qua mọi điểm dừng mà Arkady đã nhìn thấy, và do đó đường của nó phải có mặt ở giao điểm của tất cả các tập hợp được quan sát. 

Đầu vào cung cấp nhiều bộ số nguyên, mỗi bộ đại diện cho các dòng có sẵn tại một điểm dừng cụ thể. Đầu ra là thứ tự bất kỳ của tất cả các số nguyên chung cho mọi tập hợp. 

Các ràng buộc rất nhỏ: tối đa 100 điểm dừng, mỗi điểm có tối đa 100 mã định danh dòng từ 1 đến 100. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài giao lộ tập hợp đơn giản. Ngay cả giải pháp O(n * r) hoặc O(100 * 100) cũng không đáng kể trong giới hạn 1 giây. 

Một vấn đề tế nhị có thể cản trở việc triển khai là giả sử hợp thay vì giao. Ví dụ: nếu một người thu thập không chính xác tất cả các dòng nhìn thấy qua các điểm dừng, họ có thể xuất ra các dòng chỉ xuất hiện một lần. Một cạm bẫy khác là không khởi tạo giao lộ đúng cách. Bắt đầu từ một tập hợp trống và giao nhau sẽ luôn mang lại kết quả trống, trong khi bắt đầu từ điểm dừng đầu tiên là chính xác. 

Một trường hợp thất bại cụ thể cho một ý tưởng ngây thơ dựa trên công đoàn: 

đầu vào:```
2
2 1 2
2 2 3
```Cách tiếp cận của liên minh sẽ tạo ra`{1,2,3}`, nhưng câu trả lời đúng là`{2}`vì chỉ có dòng 2 xuất hiện ở cả hai điểm dừng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xem xét mọi tuyến xe điện có thể có từ 1 đến 100 và kiểm tra xem nó có xuất hiện trong danh sách mọi điểm dừng hay không. Đối với mỗi dòng ứng cử viên, chúng tôi quét tất cả các điểm dừng và xác minh tư cách thành viên. Nếu có n điểm dừng và mỗi lần kiểm tra quét tới 100 giá trị, thì độ phức tạp sẽ trở thành O(100 * n * 100), ở đây vẫn còn nhỏ nhưng dư thừa về mặt khái niệm. 

Cấu trúc của bài toán gợi ý một cách tiếp cận đơn giản hơn: thay vì kiểm tra từng dòng ứng viên, chúng ta tính trực tiếp giao của tất cả các tập hợp. Điểm dừng đầu tiên cung cấp một tập hợp đường cơ sở có thể. Mỗi điểm dừng tiếp theo sẽ lọc tập hợp này bằng cách xóa bất kỳ dòng nào không có trong điểm dừng đó. Sau khi xử lý tất cả các điểm dừng, tập hợp còn lại chính xác là tập hợp các tuyến xe điện hợp lệ. 

Điều này hoạt động vì một dòng hợp lệ phải tồn tại sau mỗi bước lọc. Mỗi điểm dừng hoạt động như một ràng buộc và giao lộ sẽ mã hóa một cách tự nhiên “phải đáp ứng đồng thời tất cả các ràng buộc”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tất cả các dòng) | O(100 * n * 100) | O(1) thêm | Đã chấp nhận | 
| Tối ưu (đặt giao lộ) | O(n * 100) | O(100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm dừng, mỗi điểm là một tập hợp các số nguyên biểu thị các tuyến xe điện. 
2. Khởi tạo tập ứng viên sử dụng điểm dừng đầu tiên. Đây là điểm khởi đầu đúng duy nhất vì nó đã thỏa mãn ràng buộc được quan sát đầu tiên. 
3. Đối với mỗi điểm dừng tiếp theo, hãy thay thế tập hợp ứng viên bằng giao điểm của nó bằng tập hợp điểm dừng hiện tại. Bước này sẽ loại bỏ bất kỳ dòng nào không có tại điểm dừng đó. 
4. Sau khi xử lý tất cả các điểm dừng, xuất các phần tử còn lại của tập ứng viên theo thứ tự bất kỳ. 

### Tại sao nó hoạt động 

Ở mỗi bước, tập ứng cử viên thể hiện chính xác các dòng xuất hiện trong tất cả các điểm dừng được xử lý cho đến nay. Bất biến này đúng vì phép giao chỉ bảo toàn tư cách thành viên cho các phần tử có trong cả hai tập hợp. Vì chúng ta bắt đầu với điểm dừng đầu tiên nên ban đầu bất biến là đúng. Mỗi điểm dừng mới duy trì tính chính xác bằng cách lọc ra các dòng không hợp lệ mà không đưa ra các dòng mới. Sau khi xử lý tất cả các điểm dừng, bất biến sẽ mở rộng đến toàn bộ đầu vào, nghĩa là tập cuối cùng chính xác là tập hợp các dòng chung cho mọi điểm dừng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

first = set(map(int, input().split()[1:]))

for _ in range(n - 1):
    parts = list(map(int, input().split()))
    s = set(parts[1:])
    first &= s

print(*first)
```Giải pháp bắt đầu bằng cách đọc số điểm dừng. Điểm dừng đầu tiên khởi tạo tập làm việc vì nó xác định các ứng cử viên hợp lệ ban đầu. Mỗi dòng tiếp theo được phân tích cú pháp bằng cách bỏ qua số đầu tiên (chỉ là số phần tử trong điểm dừng đó) và chuyển đổi các giá trị còn lại thành một tập hợp. 

Thao tác chính là`first &= s`, thực hiện giao lộ tại chỗ. Điều này đảm bảo rằng chỉ những dòng có trong cả hai bộ mới tồn tại. Việc sử dụng giao lộ tại chỗ sẽ tránh được việc phân bổ bộ nhớ không cần thiết và giữ cho quá trình triển khai được gọn nhẹ. 

Một lỗi phổ biến là quên bỏ qua số nguyên đầu tiên trong mỗi dòng, đại diện cho kích thước của tập hợp chứ không phải là mã định danh dòng. Một sai lầm khác là khởi tạo lại tập ứng viên bên trong vòng lặp, điều này sẽ loại bỏ các ràng buộc trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 1 4 6
2 1 4
5 10 5 6 4 1
```Chúng tôi theo dõi từng ứng cử viên. 

| Bước | Dừng dòng | Bộ ứng cử viên | 
| --- | --- | --- | 
| 1 | {1, 4, 6} | {1, 4, 6} | 
| 2 | {1, 4} | {1, 4} | 
| 3 | {10, 5, 6, 4, 1} | {1, 4} | 

Sau khi xử lý tất cả các điểm dừng, các ứng viên còn lại được`{1, 4}`. 

Điều này xác nhận rằng chỉ có các dòng hiện diện nhất quán trên tất cả các quan sát mới tồn tại sau quá trình lọc lặp đi lặp lại. 

### Ví dụ 2 

đầu vào:```
2
2 7 8
3 5 7 9
```| Bước | Dừng dòng | Bộ ứng cử viên | 
| --- | --- | --- | 
| 1 | {7, 8} | {7, 8} | 
| 2 | {5, 7, 9} | {7} | 

Câu trả lời cuối cùng là`{7}`bởi vì đây là đường duy nhất chung cho cả hai điểm dừng. 

Điều này cho thấy cách loại bỏ một đường xuất hiện ở hầu hết nhưng không phải ở tất cả các điểm dừng một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 100) | Mỗi giao lộ bao gồm tối đa 100 phần tử và được lặp lại tối đa 100 điểm dừng | 
| Không gian | O(100) | Chúng tôi chỉ lưu trữ tập hợp các tuyến xe điện có thể áp dụng hiện tại | 

Các ràng buộc chặt chẽ nhưng đủ nhỏ đến mức ngay cả các thao tác tập hợp lặp đi lặp lại cũng không đáng kể. Giải pháp chạy thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys as _sys

    input = _sys.stdin.readline

    n = int(input().strip())
    first = set(map(int, input().split()[1:]))

    for _ in range(n - 1):
        parts = list(map(int, input().split()))
        s = set(parts[1:])
        first &= s

    return " ".join(map(str, first)).strip()

# provided sample
assert run("""3
3 1 4 6
2 1 4
5 10 5 6 4 1
""") == "1 4", "sample 1"

# all identical
assert run("""2
3 1 2 3
3 1 2 3
""") in ["1 2 3", "3 2 1"], "all equal"

# disjoint except one
assert run("""3
2 1 2
2 2 3
2 2 4
""") == "2", "single common element"

# single possible line repeated
assert run("""2
1 42
3 42 1 2
""") == "42", "forced line"

# minimal overlap
assert run("""2
2 1 100
2 50 1
""") == "1", "boundary overlap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ giống hệt nhau | tất cả các yếu tố | ổn định khi giao lộ không làm gì | 
| rời rạc trừ một | giá trị đơn | loại bỏ đúng | 
| dòng buộc | 42 | xử lý nút giao đơn lẻ | 
| chồng chéo ranh giới | 1 | chính xác dưới sự chồng chéo thưa thớt | 

## Vỏ cạnh 

Trường hợp một cạnh là khi điểm dừng đầu tiên chứa nhiều dòng nhưng các điểm dừng sau đó giảm dần tập hợp thành một phần tử duy nhất. Thuật toán xử lý việc này một cách tự nhiên vì giao điểm không tăng một cách đơn điệu. 

Ví dụ:```
3
5 1 2 3 4 5
3 2 4 6
2 2 7
```Từng bước một: 

Tập đầu tiên là`{1,2,3,4,5}`. 

Sau điểm dừng thứ hai`{2,4}`vẫn còn. 

Sau điểm dừng thứ ba`{2}`vẫn còn. 

Điều bất biến là tập ứng cử viên luôn chứa chính xác những dòng hợp lệ cho tất cả các điểm dừng được xử lý, do đó việc thu gọn là an toàn và không thể đảo ngược. 

Một trường hợp khác là khi tất cả các điểm dừng chia sẻ nhiều dòng. Giao lộ sẽ bảo toàn tất cả chúng mà không cần bất kỳ xử lý đặc biệt nào, vì không có bước nào loại bỏ phần tử chung hợp lệ.
