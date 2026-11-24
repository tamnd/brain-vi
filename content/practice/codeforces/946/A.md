---
title: "CF 946A - Phân vùng"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép chia danh sách này thành hai nhóm theo bất kỳ cách nào chúng ta muốn, với quy tắc duy nhất là mọi phần tử phải thuộc chính xác một trong hai nhóm."
date: "2026-06-17T02:29:17+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 946
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 39 (Rated for Div. 2)"
rating: 800
weight: 946
solve_time_s: 82
verified: true
draft: false
---

[CF 946A - Phân vùng](https://codeforces.com/problemset/problem/946/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép chia danh sách này thành hai nhóm theo bất kỳ cách nào chúng ta muốn, với quy tắc duy nhất là mọi phần tử phải thuộc chính xác một trong hai nhóm. Một nhóm đóng góp tích cực vào điểm số, trong khi nhóm còn lại đóng góp tiêu cực. Chính xác hơn, nếu chúng ta gọi hai nhóm$b$Và$c$, giá trị chúng ta quan tâm là tổng các phần tử trong$b$trừ tổng các phần tử trong$c$và chúng tôi muốn làm cho giá trị này càng lớn càng tốt. 

Thiết lập này ngay lập tức gợi ý rằng mỗi phần tử ảnh hưởng độc lập đến biểu thức cuối cùng tùy thuộc vào nhóm mà nó được đặt. Vì mỗi phần tử được thêm một lần (nếu được đặt trong$b$) hoặc bị trừ một lần (nếu đặt vào$c$), quyết định cho từng phần tử là cục bộ, nhưng mục tiêu kết hợp chúng thông qua một tổng toàn cục duy nhất. 

Các ràng buộc rất nhỏ, tối đa là 100 số, mỗi số nằm trong khoảng từ -100 đến 100. Điều này loại trừ mọi nhu cầu về chiến lược tối ưu hóa hoặc cắt tỉa nâng cao. Ngay cả việc liệt kê theo cấp số nhân các phép gán trên tất cả các phần tử vẫn có giá trị về mặt khái niệm ở quy mô này, mặc dù không cần thiết. Một lý luận tham lam tuyến tính hoặc gần tuyến tính là đủ. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều âm. Một trực giác ngây thơ có thể gợi ý rằng việc trộn chúng giữa các nhóm có thể giúp cân bằng các giá trị, nhưng vì mọi phần tử bị buộc vào chính xác một nhóm nên việc đặt số âm vào nhóm dương thực sự sẽ làm giảm điểm, trong khi đặt số đó vào nhóm âm sẽ biến nó thành một đóng góp tích cực. Ví dụ, với đầu vào$[-5]$, đưa nó vào$b$sản lượng$-5$, nhưng đặt nó vào$c$sản lượng$5$, vậy đáp án tối ưu là$5$. Bất kỳ chiến lược nào giả định “nhóm dương phải chứa số lớn hơn” mà không xem xét việc đảo dấu sẽ thất bại ở đây. 

Một trường hợp cạnh khác là giá trị bằng 0. Các số 0 không ảnh hưởng đến tổng số bất kể sự phân công, nhưng việc triển khai bất cẩn cố gắng “cân bằng” quy mô nhóm hoặc dựa vào việc sắp xếp có thể xử lý sai bằng cách vô tình thay đổi cấu trúc mà không cải thiện mục tiêu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force xem xét mọi khả năng gán của từng phần tử vào một trong hai nhóm$b$hoặc nhóm$c$. Đối với mỗi$n$các phần tử, có hai sự lựa chọn, dẫn đến$2^n$các phân vùng có thể. Đối với mỗi phân vùng, việc tính tổng của cả hai nhóm sẽ mất$O(n)$, đưa ra độ phức tạp tổng thể của$O(n \cdot 2^n)$. Với$n = 100$, điều này trở nên lớn về mặt thiên văn và hoàn toàn không khả thi. 

Quan sát quan trọng là biểu thức$B - C$có thể được viết lại theo cách loại bỏ mọi sự phụ thuộc giữa các phần tử. Nếu một phần tử$a_i$được đặt trong$b$, nó góp phần$+a_i$. Nếu nó được đặt ở$c$, nó góp phần$-a_i$. Vì vậy, mỗi phần tử đóng góp độc lập$+a_i$hoặc$-a_i$và chúng tôi muốn tối đa hóa tổng số tiền đóng góp đã ký này. 

Điều này ngay lập tức làm giảm vấn đề thành việc quyết định, đối với mỗi phần tử, liệu$a_i$hoặc$-a_i$lớn hơn. Để tối đa hóa sự đóng góp, chúng tôi luôn chọn giá trị dương của$|a_i|$, vì giữa$a_i$Và$-a_i$, luôn lớn hơn$|a_i|$nếu chúng ta gán nó chính xác: số dương sẽ chuyển đến$b$, số âm đi đến$c$. 

Vì vậy, chiến lược tối ưu chỉ đơn giản là tính tổng các giá trị tuyệt đối của tất cả các phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Tối ưu (chọn dấu) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách các số nguyên. 

Mỗi phần tử được xử lý độc lập vì sự đóng góp của nó chỉ phụ thuộc vào nhóm được gán cho nó. 
2. Khởi tạo một biến tích lũy về 0. 

Điều này sẽ lưu trữ giá trị tối đa có thể đạt được của$B - C$. 
3. Đối với mỗi phần tử$a_i$, tính giá trị tuyệt đối của nó và cộng vào bộ tích lũy. 

Điều này tương ứng với việc chọn cách tốt hơn để đặt$a_i$TRONG$b$hoặc đặt nó vào$c$, vì những lựa chọn đó mang lại$a_i$Và$-a_i$tương ứng. 
4. Sau khi xử lý tất cả các phần tử, xuất bộ tích lũy. 

Lý do đằng sau bước 3 chính là cốt lõi của giải pháp: mọi yếu tố độc lập đều đóng góp tích cực hoặc tiêu cực và chúng tôi luôn chọn hướng phát huy tối đa sự đóng góp của nó. 

### Tại sao nó hoạt động 

Hàm mục tiêu phân hủy thành tổng trên các lựa chọn độc lập:$$B - C = \sum_{i=1}^{n} x_i$$mỗi nơi$x_i$là một trong hai$a_i$hoặc$-a_i$. Vì mỗi số hạng độc lập với các số hạng khác nên việc tối đa hóa tổng số sẽ giảm xuống mức tối đa hóa từng số hạng riêng lẻ. Lựa chọn$\max(a_i, -a_i)$cho mỗi chỉ số mang lại lợi nhuận$|a_i|$, đảm bảo không thể thực hiện được cải tiến cục bộ và do đó không thể thực hiện được cải tiến toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

ans = 0
for x in a:
    ans += abs(x)

print(ans)
```Giải pháp đọc đầu vào theo thời gian tuyến tính và lặp lại một lần qua mảng. The accumulator`ans`phản ánh trực tiếp sự phân rã của mục tiêu thành các đóng góp độc lập cho từng phần tử. Chi tiết triển khai chính đang sử dụng`abs(x)`thay vì phân nhánh có điều kiện; cả hai đều tương đương nhau, nhưng dạng giá trị tuyệt đối làm giảm nguy cơ sai dấu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 -2 0
```| Yếu tố | Giá trị | Lựa chọn | Đóng góp | Tổng Chạy | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | b | +1 | 1 | 
| 2 | -2 | c | +2 | 3 | 
| 3 | 0 | hoặc | 0 | 3 | 

Dấu vết này cho thấy mỗi phần tử được gán độc lập để tối đa hóa sự đóng góp của nó. Số 0 không đóng góp gì bất kể vị trí đặt, xác nhận rằng nó không ảnh hưởng đến quá trình ra quyết định. 

### Ví dụ 2 

đầu vào:```
4
-5 -1 3 2
```| Yếu tố | Giá trị | Lựa chọn | Đóng góp | Tổng Chạy | 
| --- | --- | --- | --- | --- | 
| 1 | -5 | c | +5 | 5 | 
| 2 | -1 | c | +1 | 6 | 
| 3 | 3 | b | +3 | 9 | 
| 4 | 2 | b | +2 | 11 | 

Ví dụ này nhấn mạnh rằng các giá trị âm luôn được chuyển sang nhóm dương một cách hiệu quả và các giá trị dương vẫn nằm trong nhóm dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một lần để tính giá trị tuyệt đối của nó | 
| Không gian |$O(1)$| Chỉ có một khoản tiền được duy trì | 

Quét tuyến tính dễ dàng phù hợp với các ràng buộc vì$n \le 100$và thậm chí đầu vào lớn hơn nhiều cũng không gây ra vấn đề gì. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    return str(sum(abs(x) for x in a))

# provided samples
assert run("3\n1 -2 0\n") == "3"

# all positive
assert run("4\n1 2 3 4\n") == "10"

# all negative
assert run("4\n-1 -2 -3 -4\n") == "10"

# single element
assert run("1\n-100\n") == "100"

# includes zeros
assert run("5\n0 0 0 0 0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tích cực | 10 | không cần lật biển báo | 
| tất cả đều tiêu cực | 10 | chuyển tối ưu vào nhóm c | 
| phần tử đơn | 100 | độ đúng ranh giới | 
| tất cả số không | 0 | các yếu tố trung lập được xử lý chính xác | 

## Vỏ cạnh 

Đối với một yếu tố tiêu cực duy nhất như`[-7]`, thuật toán tính toán`abs(-7) = 7`, tương ứng với việc đặt nó vào nhóm$c$. Nếu thay vào đó nó được đặt trong nhóm$b$, phần đóng góp sẽ là$-7$, điều này còn tệ hơn nữa, việc xác nhận sự lựa chọn tham lam là chính xác. 

Đối với một mảng số không như`[0, 0, 0]`, mỗi phần tử đóng góp bằng 0 bất kể vị trí. Thuật toán tích lũy số 0 một cách chính xác mà không đưa ra bất kỳ quyết định trong trường hợp đặc biệt nào và không có sự sắp xếp lại nào có thể cải thiện kết quả vì tất cả các phép gán đều tương đương.
