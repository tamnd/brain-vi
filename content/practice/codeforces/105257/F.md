---
title: "CF 105257F - Hãy thử, AC ổn"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi cái có một danh sách các số nguyên biểu thị điểm số của các lần gửi mã khác nhau. Người chơi phải gửi mã hai lần và điểm cuối cùng không phải là tổng hoặc tối đa mà là AND theo từng bit của điểm của hai lần gửi đã chọn."
date: "2026-06-24T04:27:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105257
codeforces_index: "F"
codeforces_contest_name: "2024 ICPC ShaanXi Provincial Contest"
rating: 0
weight: 105257
solve_time_s: 43
verified: true
draft: false
---

[CF 105257F - Hãy thử, AC vẫn ổn](https://codeforces.com/problemset/problem/105257/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi cái có một danh sách các số nguyên biểu thị điểm số của các lần gửi mã khác nhau. Người chơi phải gửi mã hai lần và điểm cuối cùng không phải là tổng hoặc tối đa mà là AND theo từng bit của điểm của hai lần gửi đã chọn. Cùng một lần gửi được phép chọn hai lần, nghĩa là chúng ta có thể ghép một chỉ mục với chính nó. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là chọn hai chỉ số i và j (có thể bằng nhau) sao cho giá trị của gi & gj càng lớn càng tốt. 

Kích thước đầu vào lớn: tổng số lên tới 2×10^5 trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ chiến lược ghép đôi bậc hai nào kiểm tra tất cả các cặp một cách rõ ràng, vì điều đó sẽ dẫn đến các phép toán khoảng 4×10^10 trong trường hợp xấu nhất. Chúng tôi cần một giải pháp hoạt động theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm hoặc tốt hơn. 

Một trường hợp phức tạp phát sinh từ quy tắc “có thể gửi cùng một mã hai lần”. Nếu chi tiết này bị bỏ qua, người ta có thể cho rằng i ≠ j là bắt buộc và cố gắng tìm hai phần tử riêng biệt. Ví dụ: nếu mảng là [7, 1], cách giải thích sai có thể gợi ý câu trả lời là 7 & 1 = 1, trong khi cách giải thích đúng cho phép chọn 7 hai lần, cho ra 7 & 7 = 7. 

Một trường hợp cạnh khác là khi tất cả các số đều bằng 0 ngoại trừ một giá trị lớn. Bất kỳ việc ghép nối nào có số 0 sẽ phá hủy các bit, nhưng việc tự ghép nối sẽ giữ nguyên giá trị ban đầu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử tất cả các cặp (i, j), tính gi & gj và theo dõi mức tối đa. Điều này đúng vì nó trực tiếp đánh giá định nghĩa của vấn đề. Tuy nhiên, nó yêu cầu kiểm tra n^2 cặp cho mỗi trường hợp thử nghiệm, điều này trở nên quá chậm khi tổng n đạt 2×10^5. 

Quan sát quan trọng là bitwise AND hoạt động đơn điệu đối với các phần tử giống hệt nhau: đối với bất kỳ số x nào, việc ghép nó với chính nó sẽ mang lại x và số này ít nhất luôn lớn bằng việc ghép nó với bất kỳ số nào khác, bởi vì AND chỉ có thể tắt các bit. 

Điều này ngay lập tức thay đổi cấu trúc của vấn đề. Thay vì tìm kiếm một cặp tối ưu, chúng ta chỉ cần xem xét liệu kết quả tốt nhất có đến từ việc tự ghép hay không. Vì mọi phần tử đều là ứng cử viên hợp lệ thông qua (i, i), nên câu trả lời không thể nhỏ hơn phần tử lớn nhất trong mảng. Mặt khác, bất kỳ cặp (i, j) nào cũng tạo ra một giá trị được chứa theo từng bit trong cả gi và gj, vì vậy nó không bao giờ có thể vượt quá một trong hai giá trị riêng lẻ. Điều này có nghĩa là không có cặp nào có thể đánh bại chính phần tử tối đa. 

Do đó, chiến lược tối ưu giảm xuống việc tìm giá trị tối đa trong mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu (quét tối đa) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xử lý từng test case một cách độc lập 

Mỗi trường hợp thử nghiệm được tách biệt, vì vậy chúng tôi tính toán câu trả lời riêng cho từng danh sách số. 

### 2. Quét qua tất cả các giá trị trong khi vẫn duy trì mức tối đa đang chạy 

Chúng tôi giữ một biến`best`được khởi tạo thành 0. Với mỗi số x trong mảng, chúng tôi cập nhật`best = max(best, x)`. 

Điều này hiệu quả vì mọi phần tử đều có thể đạt được dưới dạng kết quả hợp lệ bằng cách chọn nó hai lần, vì vậy câu trả lời ít nhất phải là phần tử tối đa. 

### 3. Xuất ra giá trị lớn nhất tìm được 

Sau khi quét tất cả các phần tử, chúng tôi in`best`như câu trả lời cho trường hợp thử nghiệm đó. 

### Tại sao nó hoạt động 

Đối với hai chỉ số i và j bất kỳ, giá trị gi & gj không thể vượt quá gi và không thể vượt quá gj, bởi vì theo bit AND chỉ xóa các bit. Do đó, mọi kết quả cặp có thể có đều bị giới hạn trên bởi phần tử tối đa trong mảng. Vì chọn i = j đạt được gi nên phần tử cực đại luôn đạt được. Điều này ghim câu trả lời chính xác vào giá trị tối đa trong danh sách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(max(arr))
```Giải pháp chỉ cần đọc từng trường hợp thử nghiệm, tính toán mức tối đa và in nó. Chi tiết triển khai chính là sử dụng công cụ tích hợp sẵn của Python`max`, chạy theo thời gian tuyến tính và là tối ưu cho kích thước bài toán này. Không cần tiền xử lý bổ sung và không cần xem xét các cặp một cách rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
2
3
0 1 2
4
10 10 5 4
```### Trường hợp thử nghiệm đầu tiên 

| Bước | Giá trị hiện tại | Tốt nhất cho đến nay | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 0 | 0 | 0 | 
| 1 | 1 | 1 | 
| 2 | 2 | 2 | 

Câu trả lời cuối cùng là 2, vì chọn 2 hai lần sẽ được 2. 

Điều này xác nhận rằng thuật toán theo dõi chính xác kết quả tự ghép đôi tốt nhất có thể đạt được. 

### Trường hợp thử nghiệm thứ hai 

| Bước | Giá trị hiện tại | Tốt nhất cho đến nay | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 10 | 10 | 10 | 
| 10 | 10 | 10 | 
| 5 | 5 | 10 | 
| 4 | 4 | 10 | 

Câu trả lời là 10, đạt được bằng cách chọn một trong số 10 hai lần. Điều này cho thấy rằng mặc dù có nhiều phần tử tồn tại nhưng chỉ có phần tử tối đa mới là quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để tính tối đa | 
| Không gian | O(1) thêm | Chỉ lưu trữ mức tối đa đang chạy | 

Cho rằng tổng số phần tử trong tất cả các trường hợp thử nghiệm tối đa là 2×10^5, quá trình quét tuyến tính này phù hợp một cách thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        out.append(str(max(arr)))
    return "\n".join(out)

# provided sample-like cases
assert solve("2\n3\n0 1 2\n4\n10 10 5 4\n") == "2\n10"

# minimum size, single element
assert solve("1\n1\n7\n") == "7"

# all equal values
assert solve("1\n5\n3 3 3 3 3\n") == "3"

# mixed values
assert solve("1\n4\n1 8 3 6\n") == "8"

# includes zeros
assert solve("1\n3\n0 0 0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chính nó | hành vi tự ghép đôi | 
| tất cả đều bình đẳng | cùng giá trị | ổn định dưới sự trùng lặp | 
| giá trị hỗn hợp | lựa chọn tối đa | độ chính xác của quá trình quét | 
| tất cả số không | 0 | trường hợp cạnh không có bit dương | 

## Vỏ cạnh 

### Mảng phần tử đơn 

đầu vào:```
1
1
42
```Thuật toán khởi tạo`best = 0`và cập nhật nó thành 42. Đầu ra là 42, phù hợp với thực tế là thao tác hợp lệ duy nhất là ghép nối phần tử với chính nó. 

### Tất cả số không 

đầu vào:```
1
4
0 0 0 0
```Mỗi bản cập nhật đều giữ`best = 0`. Bất kỳ cặp nào cũng cho kết quả 0, vì vậy kết quả đầu ra là chính xác. 

### Giá trị lan tỏa rộng rãi 

đầu vào:```
1
5
1 2 4 8 16
```Quá trình quét kết thúc ở mức 16. Mặc dù việc ghép các số khác nhau làm giảm giá trị, việc tự ghép nối vẫn bảo toàn từng ứng cử viên, đảm bảo số lượng tối đa vẫn không thay đổi.
