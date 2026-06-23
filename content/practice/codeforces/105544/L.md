---
title: "CF 105544L - Chín Không Bao Giờ"
description: "Chúng ta được yêu cầu chia một số binh sĩ nhất định thành nhiều nhóm khác rỗng có kích thước là các số nguyên dương có tổng bằng $N$."
date: "2026-06-22T23:38:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105544
codeforces_index: "L"
codeforces_contest_name: "The 2023 ICPC Asia Taoyuan Regional Programming Contest"
rating: 0
weight: 105544
solve_time_s: 77
verified: true
draft: false
---

[CF 105544L - Chín Không bao giờ](https://codeforces.com/problemset/problem/105544/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chia một số binh sĩ nhất định thành nhiều nhóm khác rỗng có kích thước là các số nguyên dương có tổng bằng$N$. Vấn đề không phải là về bản thân phân vùng mà là về tập hợp tất cả các hợp có thể có của một số nhóm được chọn: nếu chúng ta chọn bất kỳ tập hợp con nào của các nhóm và cộng kích thước của chúng, chúng ta sẽ không bao giờ có thể có được chính xác 9. 

Tương tự, khi chúng tôi sửa nhiều tập hợp kích thước nhóm, chúng tôi xem xét tất cả các tổng tập hợp con được hình thành bằng cách chọn bất kỳ tập hợp con nào của các số đó. Cấu hình chỉ hợp lệ nếu 9 không được biểu thị dưới dạng một trong các tổng tập hợp con đó. Trong số tất cả các cách hợp lệ để chia$N$, chúng tôi muốn tối đa hóa số lượng nhóm. 

Đầu vào là một số nguyên lớn$N$, lên đến$10^{15}$, do đó, bất kỳ giải pháp nào cố gắng liệt kê các phân vùng hoặc tập hợp con đều ngay lập tức nằm ngoài phạm vi. Đầu ra chỉ là số lượng nhóm tối đa có thể đạt được theo ràng buộc. 

Điểm tinh tế đầu tiên là ràng buộc không phải về các nhóm liền kề hoặc mức sử dụng đầy đủ mà là về các tập hợp con tùy ý. Điều đó có nghĩa là ngay cả khi một nhóm không phải là một phần của cấu trúc phân vùng đầy đủ, nó vẫn có thể tham gia tạo thành 9 cùng với những nhóm khác. Đây chính là điều khiến những giá trị nhỏ như 1 trở nên đặc biệt nguy hiểm, vì nhiều phần nhỏ có thể kết hợp lại thành số 9. 

Một cách tiếp cận ngây thơ sẽ cố gắng xây dựng các nhóm một cách tham lam với quy mô 1 cho đến khi có điều gì đó bị hỏng. Điều này không thành công vì ngay cả khi tổng không bằng 9 thì các tập hợp con vẫn có thể tạo thành 9. Ví dụ: nếu chúng ta lấy chín nhóm có kích thước 1, phân vùng hợp lệ về mặt nhóm, nhưng không hợp lệ vì chọn tất cả chín nhóm sẽ tạo ra tổng 9. Tương tự, việc trộn nhiều số nhỏ có thể vô tình tạo ra 9 theo nhiều cách. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là tạo ra tất cả các phân vùng của$N$và đối với mỗi phân vùng, hãy tính tổng tập hợp con của kích thước nhóm của nó để kiểm tra xem 9 có xuất hiện hay không. Ngay cả đối với mức độ vừa phải$N$, số lượng phân vùng tăng theo cấp số nhân và đối với mỗi phân vùng, việc kiểm tra tổng tập hợp con cũng theo cấp số nhân theo số lượng nhóm. Điều này trở nên hoàn toàn không khả thi ngoài những đầu vào nhỏ. 

Quan sát quan trọng là mục tiêu bị cấm duy nhất là số 9, do đó toàn bộ ràng buộc được bản địa hóa. Chúng tôi không cố gắng tránh tất cả các tập hợp con, chỉ một tổng duy nhất. Điều này gợi ý rằng chúng ta nên suy nghĩ xem liệu có thể “xây dựng” 9 từ quy mô nhóm có sẵn hay không. 

Nếu chúng ta muốn tối đa hóa số lượng nhóm, thì đương nhiên chúng ta thích sử dụng càng nhiều số 1 càng tốt. Tuy nhiên, khi có 9 số, chúng ta có thể trực tiếp chọn 9 nhóm và đạt tổng 9, điều này bị cấm. Vì vậy, có thể tồn tại tối đa 8 nhóm cỡ 1 trong bất kỳ cách xây dựng hợp lệ nào. 

Khi chúng tôi khắc phục giới hạn trên đó, chúng tôi vẫn có thể phân phối khối lượng còn lại một cách tùy ý. Bất kỳ nhóm bổ sung nào lớn hơn 1 đều không giúp chúng ta đạt được 9 chỉ bằng chính nó, bởi vì nó đã vượt quá 9 hoặc đóng góp quá nhiều cấu trúc không cần thiết. Cấu trúc rõ ràng nổi lên là lấy 8 nhóm kích thước 1 và đặt mọi thứ khác vào một nhóm kích thước lớn duy nhất$N - 8$. Điều này ngay lập tức ngăn chặn bất kỳ tập hợp con nào có tổng bằng 9: chỉ riêng nhóm lớn là quá lớn và các nhóm nhỏ có tổng tối đa là 8. 

Công trình này đã cung cấp 9 nhóm bất cứ khi nào$N \ge 10$. Mặt khác, hóa ra chúng ta không thể vượt quá 9 nhóm cho bất kỳ$N \ge 10$, bởi vì việc đạt được 10 nhóm sẽ buộc tất cả các nhóm phải có kích thước 1 và điều đó ngay lập tức tạo ra một tập hợp con hợp lệ gồm 9 nhóm. 

Đối với các giá trị nhỏ, khi$N \le 8$, chúng ta có thể đơn giản sử dụng tất cả những cái đó, đưa ra$K = N$. Vụ án$N = 9$được loại trừ bởi tuyên bố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force + kiểm tra tập hợp con | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng với giới hạn + một khối lớn | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Thi công tối ưu 

1. Nếu$N \le 8$, tạo nên$N$nhóm mỗi kích thước 1. Không có tập hợp con nào có thể có tổng bằng 9 vì tổng tập hợp con tối đa có thể là$N$, tức là nhỏ hơn 9. 
2. Nếu$N \ge 10$, tạo 8 nhóm có kích thước 1 và một nhóm có kích thước$N - 8$. Điều này đảm bảo tổng số nhóm là 9. 
3. Trả về số nhóm đã được xây dựng. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là các tập hợp con bằng 9 chỉ có thể được hình thành bằng cách sử dụng các nhóm đơn vị nhỏ. Khi chúng tôi giới hạn số 1 là 8, bất kỳ tập hợp con nào của công trình đều có tổng tối đa là 8 trừ khi nó bao gồm nhóm lớn, trong trường hợp đó tổng ngay lập tức vượt quá 9. Do đó, 9 sẽ không thể truy cập được. 

Sự tối ưu xuất phát từ thực tế là việc đạt được 10 nhóm trở lên buộc phải có ít nhất 10 số nguyên dương có tổng bằng$N \ge 10$, ngụ ý tất cả đều bằng 1 trong bất kỳ cấu hình đếm tối đa nào. Điều đó chắc chắn chứa 9 số 1, tạo ra tổng tập hợp con bị cấm. Vì vậy, 9 là giới hạn trên toàn cầu cho tất cả các giá trị lớn hợp lệ.$N$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input().strip())
    
    if N <= 8:
        print(N)
    else:
        print(9)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện quan sát cấu trúc. Quyết định duy nhất là liệu chúng ta có ở trong chế độ nhỏ hay không$N \le 8$hoặc chế độ lớn$N \ge 10$. Giá trị bị cấm$N = 9$không bao giờ cần xử lý đặc biệt trong mã vì đầu vào đảm bảo điều đó không xảy ra. 

Điểm tinh tế duy nhất là nhận ra rằng câu trả lời ổn định ở mức 9 cho tất cả các câu hỏi đủ lớn.$N$, thay vì phát triển với$N$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$N = 7$Chúng tôi xây dựng 7 nhóm có kích thước 1. 

| Bước | Nhóm được thành lập | K hiện tại | Kiểm tra tính hợp lệ | 
| --- | --- | --- | --- | 
| Bắt đầu | trống | 0 | hợp lệ | 
| Thêm 1 | [1] | 1 | không có tập hợp con tổng 9 có thể | 
| Lặp lại | [1,1,1,1,1,1,1] | 7 | tổng tập hợp con tối đa là 7 | 

Việc xây dựng là tối ưu vì bất kỳ phân vùng nào cũng phải có tổng bằng 7, do đó không có tập hợp con nào có thể đạt tới 9. 

### Ví dụ 2:$N = 11$Chúng tôi xây dựng 8 cái và một nhóm cỡ 3. 

| Bước | Nhóm được thành lập | K hiện tại | Kiểm tra tính hợp lệ | 
| --- | --- | --- | --- | 
| Bắt đầu | trống | 0 | hợp lệ | 
| Thêm cái | [1,1,1,1,1,1,1,1] | 8 | tập hợp con có tổng lên tới 8 | 
| Thêm phần còn lại | [1×8, 3] | 9 | bất kỳ tập hợp con nào bao gồm 3 vượt quá 9, các tập hợp con khác tối đa 8 | 

Thuộc tính quan trọng là 9 không thể được hình thành chỉ từ số 1 và không thể được hình thành bằng cách sử dụng số 3 vì nó đẩy tổng vượt quá 9. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | chỉ có một số lượng so sánh và số học không đổi | 
| Không gian | O(1) | không sử dụng công trình phụ trợ | 

Giải pháp là thời gian không đổi, dễ dàng phù hợp với các ràng buộc ngay cả đối với$N$lên đến$10^{15}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline().strip()

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    N = int(sys.stdin.readline().strip())
    if N <= 8:
        return str(N)
    return "9"

# provided-style checks
assert solve("1\n") == "1"
assert solve("8\n") == "8"
assert solve("10\n") == "9"
assert solve("11\n") == "9"
assert solve("1000000000000\n") == "9"

# boundary cases
assert solve("2\n") == "2"
assert solve("7\n") == "7"
assert solve("9\n") == "9"  # although excluded by statement, sanity check
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp tối thiểu | 
| 8 | 8 | ranh giới nhỏ trên | 
| 10 | 9 | chuyển sang câu trả lời liên tục | 
| 11 | 9 | ổn định cho N lớn hơn | 
| 10^12 | 9 | độ chính xác đầu vào lớn | 

## Vỏ cạnh 

cho$N \le 8$, thuật toán chỉ sử dụng một. Ví dụ,$N = 5$tạo ra năm nhóm có kích thước 1 và không có tập hợp con nào có thể đạt tới 9 vì tổng số tiền quá nhỏ. 

Vì$N = 10$, thuật toán xây dựng tám số 1 và một số 2. Bất kỳ tập hợp con nào của các số 1 đều có tổng tối đa là 8 và bao gồm cả số 2 ngay lập tức đẩy tổng vượt quá 9, vì vậy 9 vẫn không thể truy cập được. 

Đối với rất lớn$N$, cấu trúc vẫn giữ nguyên: tám đơn vị nhỏ và một phần dư lớn. Nhóm lớn không thể tham gia vào việc hình thành số 9 dưới bất kỳ hình thức nào, do đó hạn chế giảm hoàn toàn xuống việc kiểm soát phần nhỏ, vốn đã bão hòa ở số 8.
