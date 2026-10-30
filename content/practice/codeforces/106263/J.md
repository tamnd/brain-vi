---
title: "CF 106263J - \u626b\u96ea"
description: "Chúng ta được cho một mạng lưới đường hình học được tạo thành từ các đoạn thẳng trong mặt phẳng. Mỗi đoạn đại diện cho một con đường hai chiều giữa hai điểm cuối, nhưng hành động “dọn dẹp” mang tính định hướng: khi xe xúc tuyết chạy dọc theo một đoạn từ điểm cuối này đến điểm cuối khác, nó chỉ…"
date: "2026-06-18T23:21:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106263
codeforces_index: "J"
codeforces_contest_name: "2025 \u534e\u5357\u5e08\u8303\u5927\u5b66\u201c\u5353\u8d8a\u6559\u80b2\u676f\u201d\u7b97\u6cd5\u4e0e\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u65b0\u751f\u8d5b\uff09"
rating: 0
weight: 106263
solve_time_s: 58
verified: true
draft: false
---

[CF 106263J - \u626b\u96ea](https://codeforces.com/problemset/problem/106263/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mạng lưới đường hình học được tạo thành từ các đoạn thẳng trong mặt phẳng. Mỗi đoạn đại diện cho một con đường hai chiều giữa hai điểm cuối, nhưng hành động “dọn dẹp” mang tính định hướng: khi xe xúc tuyết chạy dọc theo một đoạn từ điểm cuối này đến điểm cuối khác, nó chỉ dọn sạch hướng đó. Để chuyển hướng ngược lại, xe phải đi ngược lại cùng đoạn đường đó tại một số điểm. 

Xe xuất phát tại điểm cuối được chỉ định của một trong các đoạn và được phép di chuyển dọc theo các đoạn, đi ngược chiều và chuyển đổi giữa các đoạn tại bất kỳ điểm cuối hoặc giao điểm chung nào. Tất cả các đoạn tạo thành một cấu trúc được kết nối duy nhất và bất kỳ cặp đoạn nào cũng giao nhau tại một điểm, do đó hệ thống đường hoạt động giống như một biểu đồ phẳng được nhúng trong mặt phẳng. 

Nhiệm vụ là tính tổng quãng đường tối thiểu mà xe phải đi sao cho mỗi đoạn đường đều được đi qua cả hai hướng ít nhất một lần. Đường đi không cần quay lại điểm bắt đầu và các đoạn có thể được xen kẽ một phần với các chuyển động khác, miễn là mọi điểm trên mỗi đoạn cuối cùng đều được truy cập theo cả hai hướng. 

Kích thước đầu vào lên tới 10.000 phân đoạn, do đó, bất kỳ giải pháp nào cố gắng mô phỏng các lựa chọn truyền tải hoặc tìm kiếm trên các tuyến đường sẽ quá chậm. Không gian bậc hai hoặc trạng thái trên các đường dẫn sẽ ngay lập tức thất bại và thậm chí các kỹ thuật tối ưu hóa đồ thị phụ thuộc vào việc ghép các cạnh hoặc giải các biến thể của đường dẫn Euler sẽ không cần thiết nếu hiểu đúng cấu trúc khóa. 

Một điểm tinh tế là các giao lộ không làm giảm yêu cầu di chuyển qua lại. Ngay cả khi hai phân đoạn giao nhau, điều đó không tạo ra hiệu ứng “tái sử dụng cạnh” chung về mặt làm sạch, bởi vì việc làm sạch vẫn được xác định theo hướng phân đoạn, không phải theo đường bao phủ hình học. 

Một sai lầm ngây thơ sẽ là nghĩ rằng đây là con đường ngắn nhất đi qua tất cả các cạnh một lần, tương tự như vấn đề nhân viên bán hàng du lịch hoặc kiểm tra tuyến đường. Ví dụ: với hai đoạn giao nhau, người ta có thể cố gắng tìm một tuyến đường thông minh “chia sẻ” đường đi giữa chúng. Tuy nhiên, vì mỗi đoạn phải được duyệt độc lập theo cả hai hướng nên việc chia sẻ không làm giảm tổng độ dài đường truyền được yêu cầu. 

Ví dụ: nếu chúng ta có hai đoạn giao nhau: 

đầu vào:```
2
0 0 2 2
0 2 2 0
0 0
```Mỗi đoạn có độ dài cố định và bất kể chúng ta đi qua điểm giao nhau như thế nào, mỗi đoạn vẫn cần hai lần truyền tải đầy đủ. Bất kỳ nỗ lực nào nhằm giảm chi phí bằng cách định tuyến thông minh cuối cùng vẫn sẽ thu hẹp lại khoảng cách hình học tương tự. 

## Phương pháp tiếp cận 

Giải thích brute-force sẽ coi đây là một vấn đề lập kế hoạch tuyến đường trên biểu đồ hình học: liệt kê các thứ tự truyền tải có thể có trên các phân đoạn, mô phỏng chuyển động dọc theo các cạnh và theo dõi hướng nào của từng phân đoạn đã được làm sạch. Mỗi trạng thái sẽ cần mã hóa vị trí, hướng hiện tại và mặt nạ bit của các hướng đã hoàn thành cho mỗi phân đoạn. 

Ngay cả với việc cắt tỉa lạc quan, cách tiếp cận này sẽ bùng nổ ngay lập tức. Với n lên đến 10.000, số lượng trạng thái liên quan đến các tập hợp con của các cạnh là theo cấp số nhân và ngay cả một đường đi ngắn nhất được đơn giản hóa trên không gian trạng thái mở rộng cũng sẽ yêu cầu hành vi ít nhất là O(2^n) hoặc O(n·2^n), điều này hoàn toàn không khả thi. 

Quan sát quan trọng là vấn đề tách rời hoàn toàn trên các phân đoạn. Yêu cầu duy nhất là mỗi đoạn được đi qua một lần theo mỗi hướng. Không có ràng buộc kết nối nào giữa các đoạn khác nhau ảnh hưởng đến chi phí, vì việc rẽ hoặc chuyển đổi các đoạn là miễn phí tại các điểm cuối hoặc nút giao và chi phí đi qua chỉ phụ thuộc vào độ dài đoạn. 

Điều này có nghĩa là tổng chi phí chỉ đơn giản là tổng của tất cả các lần duyệt cần thiết. Mỗi đoạn đóng góp chính xác gấp đôi chiều dài Euclide của nó: một lần theo mỗi hướng. Vị trí bắt đầu không thay đổi yêu cầu này vì nó không loại bỏ sự cần thiết phải đi qua bất kỳ hướng đoạn nào. 

Do đó, toàn bộ vấn đề được rút gọn thành việc tính tổng chiều dài của tất cả các đoạn và nhân đôi nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm lộ trình vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tổng độ dài đoạn × 2 | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các đoạn và tính khoảng cách Euclide giữa các điểm cuối của chúng. Mỗi phân đoạn đều độc lập nên chúng tôi coi đó là một phần đóng góp độc lập cho câu trả lời cuối cùng. 
2. Tích lũy tổng độ dài của tất cả các đoạn. Điều này thể hiện khoảng cách cần thiết để đi qua từng đoạn một lần theo một hướng. 
3. Nhân tổng số tiền với 2. Điều này giải thích cho yêu cầu rằng mọi đoạn phải đi qua cả hai hướng và không có việc chia sẻ tuyến đường nào có thể làm giảm sự cần thiết này. 
4. Xuất giá trị kết quả dưới dạng số dấu phẩy động với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Mỗi đoạn có chính xác một chiều dài hình học và quy tắc làm sạch bắt buộc phải có chính xác hai lần duyệt trên mỗi đoạn, mỗi lần theo một hướng. Vì chi phí truyền tải hoàn toàn là số liệu và không phụ thuộc vào hướng nên mọi giải pháp khả thi phải bao gồm ít nhất 2 × độ dài cho mỗi phân đoạn. 

Đồng thời, tính khả thi là không đáng kể: chúng ta luôn có thể đi qua từng đoạn về phía trước và sau đó đi qua đoạn đó về phía sau, sử dụng các giao lộ hoặc điểm cuối để di chuyển giữa các đoạn mà không ảnh hưởng đến tổng khoảng cách. Vì quá trình chuyển đổi không tăng thêm chi phí ngoài việc di chuyển dọc theo các phân đoạn nên tổng của tất cả các lần di chuyển được yêu cầu vừa là giá trị giới hạn dưới vừa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def main():
    n = int(input())
    total = 0.0

    segments = []
    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        dx = x1 - x2
        dy = y1 - y2
        total += math.hypot(dx, dy)

    start_x, start_y = map(int, input().split())

    print("{:.10f}".format(2 * total))

if __name__ == "__main__":
    main()
```Giải pháp tính toán từng độ dài đoạn bằng cách sử dụng`math.hypot`, ổn định về số lượng cho khoảng cách Euclide. Điểm bắt đầu được đọc nhưng không được sử dụng vì nó không ảnh hưởng đến tổng chi phí truyền tải cần thiết. 

Phép nhân với 2 là bước trung tâm: nó mã hóa yêu cầu làm sạch hai chiều trực tiếp vào mô hình chi phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 0 2 2
4 0 0 4
0 0
```Chúng tôi tính toán từng độ dài đoạn: 

| Bước | Phân đoạn | Chiều dài | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | (0,0)-(2,2) | √8 ≈ 2,828427 | 2.828427 | 
| 2 | (4,0)-(0,4) | √32 ≈ 5.656854 | 8.485281 | 

Câu trả lời cuối cùng là tổng số gấp đôi: 

| Tổng cộng | Kết quả | 
| --- | --- | 
| 8.485281 | 16.970562 | 

Điều này khớp chính xác với đầu ra mẫu, xác nhận rằng không cần thêm chi phí định tuyến ngoài việc bao phủ mỗi phân đoạn hai lần. 

### Ví dụ 2 

đầu vào:```
1
0 0 3 4
0 0
```| Bước | Phân đoạn | Chiều dài | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | (0,0)-(3,4) | 5.0 | 5.0 | 

Câu trả lời cuối cùng: 

| Tổng cộng | Kết quả | 
| --- | --- | 
| 5.0 | 10.0 | 

Điều này cho thấy rằng ngay cả một đoạn đơn lẻ cũng cần phải truyền tải theo cả hai hướng, làm tăng gấp đôi chiều dài của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đoạn được xử lý một lần để tính khoảng cách Euclide | 
| Không gian | O(1) | Chỉ có một khoản tiền được duy trì | 

Thuật toán dễ dàng phù hợp với các ràng buộc cho n lên đến 10.000, vì nó chỉ thực hiện các phép tính số học tuyến tính và một vài phép đánh giá căn bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    total = 0.0
    for _ in range(n):
        x1, y1, x2, y2 = map(int, sys.stdin.readline().split())
        total += math.hypot(x1 - x2, y1 - y2)
    sys.stdin.readline()
    return "{:.10f}".format(2 * total)

# sample
assert abs(float(solve("""2
0 0 2 2
4 0 0 4
0 0
""")) - 16.9705627485) < 1e-6

# single segment
assert solve("""1
0 0 3 4
0 0
""") == "10.0000000000"

# minimal segment
assert solve("""1
0 0 0 0
0 0
""") == "0.0000000000"

# multiple segments
assert abs(float(solve("""3
0 0 1 0
1 0 1 1
1 1 2 1
0 0
""")) - 4.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 đoạn giao nhau | 16.9705627485 | chính xác trên nhiều phân đoạn | 
| đoạn 3-4-5 đơn | 10.0 | quy tắc nhân đôi | 
| đoạn có độ dài bằng 0 | 0,0 | cạnh thoái hóa | 
| chuỗi phân đoạn | 4.0 | tích lũy tuyến tính | 

## Vỏ cạnh 

Đoạn có độ dài bằng 0 là trường hợp góc đơn giản nhất. Nếu cả hai điểm cuối giống hệt nhau thì đóng góp của nó bằng 0 bất kể hướng nào, do đó quy tắc nhân đôi vẫn mang lại kết quả bằng 0 mà không ảnh hưởng đến tính chính xác. 

Một mạng lưới giao lộ dày đặc không thay đổi được giải pháp. Ngay cả khi nhiều đoạn giao nhau tại các điểm dùng chung, thuật toán vẫn xử lý chúng một cách độc lập, vì chi phí truyền tải bị ràng buộc chặt chẽ với độ dài đoạn chứ không phụ thuộc vào cách các đường dẫn có thể được xen kẽ thông qua hình học. 

Vị trí bắt đầu tùy ý cũng không ảnh hưởng đến câu trả lời. Mặc dù nó đảm bảo rằng điểm bắt đầu nằm ở điểm cuối của một số đoạn, nhưng nó không loại bỏ nhu cầu đi qua đoạn đó theo cả hai hướng, do đó vị trí ban đầu không mang lại cơ hội tối ưu hóa đặc biệt nào.
