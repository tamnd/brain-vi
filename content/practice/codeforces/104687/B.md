---
title: "CF 104687B - \u041e\u0442\u0441\u043e\u0440\u0442\u0438\u0440\u043e\u0432\u0430\u0442\u044c \u043c\u0430\u0441\u0441\u0438\u0432"
description: "Chúng ta được cấp một dãy số nguyên và được phép sắp xếp lại nó bằng cách sắp xếp. Sau khi sắp xếp, chúng tôi tính tổng “chi phí chênh lệch liền kề”, là tổng của chênh lệch tuyệt đối giữa mỗi cặp phần tử liên tiếp trong chuỗi được sắp xếp."
date: "2026-06-29T14:43:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104687
codeforces_index: "B"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u0432 \u0426\u0420\u041e\u0414 2022"
rating: 0
weight: 104687
solve_time_s: 69
verified: false
draft: false
---

[CF 104687B - \u041e\u0442\u0441\u043e\u0440\u0442\u0438\u0440\u043e\u0432\u0430\u0442\u044c \u043c\u0430\u0441\u0441\u0438\u0432](https://codeforces.com/problemset/problem/104687/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và được phép sắp xếp lại nó bằng cách sắp xếp. Sau khi sắp xếp, chúng tôi tính tổng “chi phí chênh lệch liền kề”, là tổng của chênh lệch tuyệt đối giữa mỗi cặp phần tử liên tiếp trong chuỗi được sắp xếp. Nhiệm vụ là xuất ra giá trị cuối cùng này. 

Nói cách khác, chúng ta lấy mảng, sắp xếp lại nó theo thứ tự không giảm, sau đó đo xem nó “kéo dài” bao nhiêu từ phần tử này sang phần tử tiếp theo khi đi từ trái sang phải. Đây là phép tính một lượt sau khi sắp xếp. 

Sự ràng buộc về$n$nhỏ, nhiều nhất là 100 và giá trị có thể lên tới$10^5$. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ$O(n^2)$hoặc$O(n \log n)$giải pháp dễ dàng đủ nhanh. Ngay cả một loại đơn giản cũng chiếm ưu thế trong thời gian chạy và có thể chấp nhận được. 

Một điểm tinh tế là tổng giá trị tuyệt đối phụ thuộc rất nhiều vào thứ tự. Nếu không sắp xếp, các sai phân liền kề có thể lớn hoặc nhỏ tùy ý tùy theo sự sắp xếp. Một cách tiếp cận ngây thơ mà bỏ qua việc sắp xếp rõ ràng sẽ tạo ra kết quả không chính xác. 

Một lỗi điển hình là tính toán sự khác biệt trên mảng ban đầu thay vì mảng đã được sắp xếp. Ví dụ, với đầu vào đã cho`[3, 1, 2]`, số tiền ban đầu là`|3-1| + |1-2| = 2 + 1 = 3`, trong khi tính toán được sắp xếp chính xác sẽ cho`|1-2| + |2-3| = 1 + 1 = 2`. Sự không khớp này cho thấy việc sắp xếp không phải là tùy chọn. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng nhau. Mảng được sắp xếp không đổi và kết quả phải bằng 0. Bất kỳ quá trình triển khai nào vô tình sử dụng các khác biệt có dấu thay vì giá trị tuyệt đối vẫn có thể vượt qua một số thử nghiệm nhưng không thành công trong các trường hợp có dấu hỗn hợp. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là thử tất cả các hoán vị của mảng, tính tổng chênh lệch liền kề cho mỗi hoán vị và có thể lấy giá trị tối thiểu hoặc một số giá trị bắt buộc tùy theo cách giải thích. Trong vấn đề này, vì việc sắp xếp là bắt buộc một cách rõ ràng nên chế độ xem bạo lực trở nên không cần thiết, nhưng nó vẫn hữu ích như một mô hình tinh thần: chúng ta đang tìm kiếm một thứ tự làm cho chuỗi có cấu trúc đủ để đánh giá dễ dàng. 

Liệt kê tất cả các chi phí hoán vị$n!$cấu hình và mỗi chi phí đánh giá$O(n)$, điều này trở nên hoàn toàn không khả thi ngay cả đối với$n = 10$. Điều này dẫn đến vụ nổ nhà máy. 

Quan sát quan trọng là bài toán không yêu cầu chúng ta tối ưu hóa các hoán vị. Nó yêu cầu chúng ta sắp xếp trước một cách rõ ràng. Khi mảng được sắp xếp, cấu trúc trở nên đơn điệu. Trong một mảng được sắp xếp, cách tốt nhất để tính tổng các khác biệt liền kề chỉ đơn giản là xem qua một lần. Không có thứ tự thay thế nào cần xem xét nên vụ nổ tổ hợp biến mất. 

Do đó, giải pháp rút gọn thành hai thao tác: sắp xếp và tích lũy tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Sắp xếp + quét |$O(n \log n)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Các bước 

1. Đọc số nguyên$n$và mảng$a$. Đây là một trường hợp thử nghiệm duy nhất nên chúng tôi xử lý nó một lần. 
2. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo các phần tử liền kề có giá trị gần nhất có thể theo nghĩa toàn cục, loại bỏ các hiệu ứng sắp xếp tùy ý. 
3. Khởi tạo biến tích lũy`ans = 0`. Điều này sẽ lưu trữ tổng của sự khác biệt tuyệt đối liền kề. 
4. Lặp lại từ chỉ mục`1`ĐẾN`n - 1`. Với mỗi vị trí hãy tính`a[i] - a[i-1]`. Vì mảng đã được sắp xếp nên chênh lệch này luôn không âm nên giá trị tuyệt đối là không cần thiết. 
5. Thêm từng điểm khác biệt vào`ans`. 
6. Đầu ra`ans`. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, chuỗi là đơn điệu không giảm, do đó mọi sai phân liền kề đều bằng khoảng cách thực giữa các thống kê thứ tự liên tiếp. Bất kỳ thứ tự nào khác sẽ đưa ra những bước nhảy lớn hơn hoặc nhân đôi những khác biệt nhỏ cục bộ nhiều lần. Thứ tự được sắp xếp làm cho mọi phần tử tham gia chính xác vào các chuyển đổi cần thiết tối thiểu giữa các giá trị liên tiếp, do đó tổng các sai phân liền kề được xác định duy nhất và có thể tính toán trực tiếp từ mảng đã sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    a.sort()
    
    ans = 0
    for i in range(1, n):
        ans += a[i] - a[i - 1]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện rất đơn giản. Việc sắp xếp được thực hiện tại chỗ, giúp giảm thiểu mức sử dụng bộ nhớ. Vòng lặp bắt đầu từ chỉ mục 1 để tránh kiểm tra ranh giới cho chỉ mục 0. Vì mảng được sắp xếp, chúng tôi thay thế chênh lệch tuyệt đối bằng phép trừ trực tiếp, giúp tránh các lệnh gọi hàm không cần thiết và đơn giản hóa logic. 

Một lỗi phổ biến là quên sắp xếp hoặc tính toán`abs(a[i] - a[i-1])`trên mảng ban đầu. Cả hai đều vi phạm sự chuyển đổi dự kiến ​​được mô tả trong bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 1 2
```Mảng được sắp xếp là`[1, 2, 3]`. 

| tôi | một[i-1] | một [tôi] | khác biệt | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 1 | 
| 2 | 2 | 3 | 1 | 2 | 

Đầu ra là`2`. 

Điều này xác nhận rằng việc sắp xếp sẽ thay đổi các mối quan hệ kề cận và làm giảm các bước nhảy lớn tùy ý. 

### Ví dụ 2 

đầu vào:```
5
4 4 4 4 4
```Mảng được sắp xếp là`[4, 4, 4, 4, 4]`. 

| tôi | một[i-1] | một [tôi] | khác biệt | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 0 | 0 | 
| 2 | 4 | 4 | 0 | 0 | 
| 3 | 4 | 4 | 0 | 0 | 
| 4 | 4 | 4 | 0 | 0 | 

Đầu ra là`0`. 

Điều này xác nhận thuật toán xử lý các mảng thống nhất một cách chính xác và không tạo ra sự đóng góp nhân tạo nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian |$O(1)$thêm | sắp xếp tại chỗ, chỉ sử dụng ắc quy | 

Với$n \le 100$, giải pháp sẽ chạy dưới mức giới hạn thời gian hợp lý. Ngay cả khi nhiều trường hợp thử nghiệm được thêm vào, độ phức tạp tương tự sẽ vẫn đủ dễ dàng. 

Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào, không đáng kể đối với phạm vi ràng buộc này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    a.sort()
    ans = 0
    for i in range(1, n):
        ans += a[i] - a[i - 1]
    print(ans)

# provided sample
assert run("3\n3 1 2\n") == "2"

# minimum size
assert run("2\n5 1\n") == "4"

# all equal
assert run("4\n7 7 7 7\n") == "0"

# already sorted
assert run("5\n1 2 3 4 5\n") == "4"

# reverse order
assert run("5\n5 4 3 2 1\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 5 1 | 4 | độ chính xác kích thước tối thiểu | 
| 4 7 7 7 7 | 0 | giá trị thống nhất | 
| 5 1 2 3 4 5 | 4 | trường hợp đã được sắp xếp | 
| 5 5 4 3 2 1 | 4 | ổn định trật tự ngược | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các phần tử đều giống hệt nhau. Đối với đầu vào`[7, 7, 7, 7]`, việc sắp xếp không làm gì cả và mọi sự khác biệt liền kề đều bằng không. Thuật toán tạo ra`0`bởi vì mỗi phép trừ mang lại kết quả bằng 0, xác nhận không có sự tích lũy ngẫu nhiên. 

Một trường hợp khác là khi đầu vào đã được sắp xếp. Vì`[1, 2, 3, 4, 5]`, thuật toán không thực hiện thay đổi cấu trúc và quá trình quét trực tiếp tính toán tổng các khoảng trống liên tiếp, điều này đúng theo cách xây dựng. 

Trường hợp thứ ba là sắp xếp ngược lại. Vì`[5, 4, 3, 2, 1]`, sắp xếp biến nó thành`[1, 2, 3, 4, 5]`, sau đó việc tính toán tiến hành giống hệt với trường hợp đã được sắp xếp. Điều này cho thấy thuật toán bất biến theo thứ tự ban đầu và chỉ phụ thuộc vào cấu trúc nhiều tập hợp.
