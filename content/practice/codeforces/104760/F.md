---
title: "CF 104760F - \u0427\u0435\u0433\u043e-\u0442\u043e \u043d\u0435 \u0445\u0432\u0430\u0442\u0430\u0435\u0442..."
description: "Chúng ta được cấp một tập hợp các “loại” số nguyên đại diện cho các mặt hàng trong kho. Hầu hết các loại đều được cân bằng hoàn hảo: mỗi loại như vậy xuất hiện với số lần như nhau, ví dụ $S$. Chính xác có một loại ngoại lệ phá vỡ khuôn mẫu này và xuất hiện ít lần hơn, ví dụ $P$, trong đó $P < S$."
date: "2026-06-28T22:36:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104760
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Qualification Contest"
rating: 0
weight: 104760
solve_time_s: 55
verified: true
draft: false
---

[CF 104760F - \u0427\u0435\u0433\u043e-\u0442\u043e \u043d\u0435 \u0445\u0432\u0430\u0442\u0430\u0435\u0442...](https://codeforces.com/problemset/problem/104760/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các “loại” số nguyên đại diện cho các mặt hàng trong kho. Hầu hết các loại đều được cân bằng hoàn hảo: mọi loại như vậy đều xuất hiện với số lần như nhau, chẳng hạn$S$. Chính xác có một loại ngoại lệ phá vỡ mô hình này và xuất hiện ít lần hơn, chẳng hạn$P$, Ở đâu$P < S$. 

Nhiệm vụ là xác định loại ít được trình bày này. Chúng tôi không được bảo$P$hoặc$S$và không có thứ tự hoặc cấu trúc nào ngoài số liệu thô. Đầu vào chỉ đơn giản là một danh sách các số nguyên và chính xác một số nguyên riêng biệt có tần số nhỏ hơn tất cả các số nguyên khác, bằng nhau. 

Các ràng buộc cho phép lên đến$10^5$các phần tử. Điều này ngay lập tức loại trừ mọi thứ bậc hai như so sánh từng cặp tần số hoặc quét liên tục mảng để tìm từng giá trị riêng biệt. MỘT$O(N \log N)$Cách tiếp cận này có thể chấp nhận được, nhưng có đủ độ trễ cho giải pháp tuyến tính nếu chúng ta sử dụng hàm băm. 

Trường hợp cạnh tinh tế nhất là khi chỉ có hai giá trị riêng biệt và số lượng của chúng khác nhau 1. Ví dụ: nếu một giá trị xuất hiện 2 lần và giá trị khác xuất hiện 3 lần thì câu trả lời vẫn là giá trị nhỏ hơn mặc dù sự khác biệt là rất nhỏ. Một trường hợp cạnh khác là khi có số âm hoặc số nguyên có cường độ lớn; giải pháp chỉ phải dựa vào sự so sánh bình đẳng, không đặt ra các giả định. 

Một sai lầm ngây thơ sẽ là giả sử giá trị tối thiểu hoặc tối đa tương ứng với loại bị thiếu. Ví dụ, trong`[5, -3, 5, 5, -3]`, câu trả lời là`-3`, mặc dù nó nhỏ hơn 5; lý do đúng đến từ tần số, không phải giá trị. 

## Phương pháp tiếp cận 

Chiến lược brute-force là tính toán tần số của mọi giá trị riêng biệt bằng cách quét toàn bộ danh sách để tìm từng phần tử. Đối với mỗi vị trí$i$, chúng tôi đếm bao nhiêu lần$t_i$xuất hiện bằng cách lặp qua toàn bộ mảng. Điều này cung cấp cho chúng tôi tần số của mọi ứng cử viên và sau đó chúng tôi chọn giá trị có tần số nhỏ nhất. 

Cách tiếp cận này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề: chúng tôi đo lường các lần xuất hiện một cách rõ ràng. Tuy nhiên, chi phí của nó là rất cao. Đối với mỗi$N$các phần tử, chúng tôi thực hiện một phần tử khác$O(N)$quét, dẫn đến$O(N^2)$hoạt động. Với$N = 10^5$, điều này trở thành$10^{10}$những so sánh vượt xa những giới hạn thông thường. 

Sự cải tiến đến từ việc nhận ra rằng việc đếm tần số không yêu cầu quét nhiều lần. Nếu chúng ta tổng hợp số lượng trong một lần sử dụng bản đồ băm, chúng ta có thể tính toán tất cả tần số trong$O(N)$. Sau khi đếm xong, chúng ta chỉ cần lặp lại chúng một lần để tìm giá trị tần số tối thiểu. Điều này biến công việc lặp đi lặp lại thành công việc được chia sẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Đếm bản đồ băm | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị và xây dựng bảng tần số ánh xạ từng loại số nguyên với số lần xuất hiện của nó. Điều này đảm bảo chúng tôi nén tất cả các lần quét lặp lại thành một lần truyền dữ liệu. 
2. Lặp lại bảng tần số và theo dõi giá trị có tần số nhỏ nhất. Vì chính xác một giá trị được thể hiện dưới mức nên giá trị tối thiểu này được xác định rõ ràng và duy nhất. 
3. Xuất ra giá trị tương ứng với tần số tối thiểu đó. 

### Tại sao nó hoạt động 

Sự đảm bảo là tất cả các giá trị ngoại trừ một giá trị đều có cùng tần số$S$, trong khi chính xác một giá trị có tần số$P < S$. Điều này tạo ra sự phân tách chặt chẽ trong không gian tần số: một giá trị tối thiểu duy nhất và một số giá trị lớn hơn giống hệt nhau. Bản đồ băm duy trì số lượng chính xác mà không có sự mơ hồ xung đột về sự bằng nhau, do đó bảng tần số là chính xác. Việc chọn tần số tối thiểu từ bảng này phải trả về loại duy nhất được trình bày dưới mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    freq = {}
    for x in arr:
        freq[x] = freq.get(x, 0) + 1
    
    # find the element with smallest frequency
    best_val = None
    best_freq = float('inf')
    
    for val, cnt in freq.items():
        if cnt < best_freq:
            best_freq = cnt
            best_val = val
    
    print(best_val)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên xây dựng từ điển tần số trong một lần quét tuyến tính. Mỗi lần chèn hoặc cập nhật được mong đợi$O(1)$, do đó tổng số vẫn tuyến tính. 

Giai đoạn thứ hai tìm kiếm tần số nhỏ nhất trong số các khóa riêng biệt. Điều này an toàn vì số lượng giá trị riêng biệt nhiều nhất là$N$và chúng tôi chỉ quét tập hợp rút gọn này một lần. 

Một chi tiết triển khai tinh tế là việc khởi tạo`best_freq`đến vô cùng. Thay vào đó, sử dụng số nguyên lớn cũng có tác dụng, nhưng số vô cực sẽ tránh được tình trạng tràn ngẫu nhiên hoặc các lựa chọn trọng điểm không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 -3 5 5 -3
```Xây dựng tần số: 

| Giá trị | Đếm | 
| --- | --- | 
| 5 | 3 | 
| -3 | 2 | 

| Bước | Tốt nhất hiện nay | Tần số tốt nhất | 
| --- | --- | --- | 
| Bắt đầu | Không có | thông tin | 
| Kiểm tra 5 | 5 | 3 | 
| Kiểm tra -3 | -3 | 2 | 

Thuật toán đầu tiên nhìn thấy 5 với số đếm 3, sau đó cập nhật khi gặp -3 với số đếm 2. Đáp án cuối cùng là -3 vì nó có tần số nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
6
10 10 10 7 7 7
```Xây dựng tần số: 

| Giá trị | Đếm | 
| --- | --- | 
| 10 | 3 | 
| 7 | 3 | 

| Bước | Tốt nhất hiện nay | Tần số tốt nhất | 
| --- | --- | --- | 
| Bắt đầu | Không có | thông tin | 
| Kiểm tra 10 | 10 | 3 | 
| Kiểm tra 7 | 10 hoặc 7 (hòa) | 3 | 

Trong trường hợp này, cả hai tần số đều bằng nhau. Tuy nhiên, bảo đảm vấn đề cấm sự bình đẳng hoàn toàn giữa tất cả các giá trị, không có ngoại lệ; phải có chính xác một loại được trình bày dưới mức. Ví dụ này cho thấy rằng trong một đầu vào hợp lệ, các ràng buộc ở mức tối thiểu không thể xảy ra, do đó mức tối thiểu đầu tiên gặp phải là an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lần để xây dựng tần số và một lần vượt qua các khóa riêng biệt | 
| Không gian | O(N) | Bản đồ băm lưu trữ tối đa N giá trị riêng biệt | 

Với$N \le 10^5$, cả thời gian và trí nhớ đều nằm trong giới hạn thoải mái. Quét tuyến tính trên 100k phần tử là chuyện nhỏ đối với Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5\n5 -3 5 5 -3\n") == "-3"

# minimum size case
assert run("3\n1 1 2\n") == "2"

# negative values
assert run("5\n-1 -1 -1 2 2\n") == "2"

# larger skew
assert run("7\n4 4 4 4 9 9 9\n") == "9"

# already balanced except one
assert run("4\n8 8 8 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 -3 5 5 -3 | -3 | tách tần số cơ bản | 
| 3 1 1 2 | 2 | trường hợp kích thước tối thiểu | 
| -1 -1 -1 2 2 | 2 | xử lý giá trị âm | 
| 7 4 4 4 4 9 9 9 | 9 | khoảng cách tần số không đồng đều | 
| 4 8 8 8 1 | 1 | thành phần thiểu số duy nhất | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi tần số thiểu số khác với tần số đa số đúng một. Xem xét đầu vào:```
4
7 7 7 8
```Bảng tần số là`{7: 3, 8: 1}`. Thuật toán quét bản đồ và đặt`best_val = 8`vì 1 nhỏ hơn 3. Không phụ thuộc vào thứ tự của khóa hoặc giá trị, chỉ phụ thuộc vào số lượng. 

Một trường hợp cạnh khác là giá trị âm:```
5
-10 -10 -10 -3 -3
```Tần số là`{ -10: 3, -3: 2 }`. Thuật toán chọn đúng`-3`vì tần số của nó nhỏ hơn mặc dù giá trị số của nó lớn hơn. Điều này xác nhận tính đúng đắn trong phạm vi số nguyên tùy ý. 

Trường hợp cuối cùng là đầu vào lớn có nhiều bản sao. Do quá trình băm xử lý việc tổng hợp trong thời gian không đổi dự kiến ​​cho mỗi lần chèn, nên thuật toán vẫn tuyến tính và không suy giảm ngay cả khi tất cả các giá trị giống hệt nhau ngoại trừ một giá trị.
