---
title: "CF 104518I - Tên người dùng"
description: "Chúng ta được cung cấp một chuỗi các giá trị doanh thu hàng tháng và kích thước cửa sổ cố định $K$. Đối với mỗi đoạn liền kề có độ dài $K$, chúng tôi tính doanh thu trung bình của đoạn đó. Vì tất cả các cửa sổ đều có cùng độ dài nên việc so sánh số trung bình cũng tương đương với việc so sánh tổng."
date: "2026-06-30T10:40:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104518
codeforces_index: "I"
codeforces_contest_name: "UNICAMP Selection Contest 2023"
rating: 0
weight: 104518
solve_time_s: 128
verified: true
draft: false
---

[CF 104518I - Tên người dùng](https://codeforces.com/problemset/problem/104518/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi giá trị doanh thu hàng tháng và kích thước cửa sổ cố định$K$. Đối với mỗi đoạn dài liền kề$K$, chúng tôi tính toán doanh thu trung bình của nó. Vì tất cả các cửa sổ đều có cùng độ dài nên việc so sánh số trung bình cũng tương đương với việc so sánh tổng. 

Nhiệm vụ là xác định hai vị trí: chỉ mục bắt đầu sớm nhất của cửa sổ có tổng tối thiểu và chỉ mục bắt đầu sớm nhất của cửa sổ có tổng tối đa. 

Cấu trúc chính ở đây là chúng tôi không chỉ chọn một cửa sổ tốt nhất theo giá trị mà còn thực thi quy tắc ràng buộc dựa trên lần xuất hiện sớm nhất. Điều đó thay đổi cách chúng ta xử lý các tổng bằng nhau: chúng ta không được ghi đè chỉ mục trước đó khi chúng ta thấy lại cùng một giá trị. 

Các ràng buộc đạt tới$10^5$các phần tử loại trừ việc tính toán lại tổng cho mọi cửa sổ từ đầu. Một cách tiếp cận đơn giản tính tổng mỗi cửa sổ một cách độc lập sẽ mất$O(NK)$, trở nên quá lớn khi cả hai$N$Và$K$lớn. 

Các trường hợp cạnh phát sinh khi nhiều cửa sổ có cùng số tiền tối thiểu hoặc tối đa. Việc thực hiện bất cẩn cập nhật các chỉ số trên`<=`hoặc`>=`thay vì nghiêm ngặt`<`hoặc`>`sẽ chọn sai vị trí sau thay vì lần xuất hiện đầu tiên. 

Một trường hợp tế nhị khác là khi$K = 1$. Sau đó, mỗi phần tử là cửa sổ riêng của nó và câu trả lời giảm xuống lần xuất hiện đầu tiên của các giá trị mảng tối thiểu và tối đa. 

## Phương pháp tiếp cận 

Phương pháp brute-force tính tổng của mỗi cửa sổ bằng cách lặp qua nó$K$các phần tử. Điều này đúng nhưng việc lặp đi lặp lại rất nặng nề: mỗi$N-K+1$chi phí cửa sổ$O(K)$, dẫn đến$O(NK)$. Tại$10^5$, tốc độ này quá chậm. 

Sự cải thiện đến từ việc quan sát sự chồng chéo. Chia sẻ cửa sổ liền kề$K-1$các phần tử, vì vậy chúng ta có thể cập nhật tổng theo thời gian không đổi bằng cách trừ đi phần tử đi và cộng phần tử vào. Kỹ thuật cửa sổ trượt này làm giảm độ phức tạp về thời gian tuyến tính. 

Khi chúng ta có tổng mỗi cửa sổ$O(1)$, việc theo dõi mức tối thiểu và tối đa sẽ trở thành một lượt duy nhất với sự ràng buộc cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NK)$|$O(1)$| Quá chậm | 
| Cửa Sổ Trượt |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì tổng của cửa sổ đầu tiên, sau đó trượt nó qua mảng trong khi cập nhật mức tối thiểu và tối đa. 

1. Tính tổng số đầu tiên$K$các phần tử. Thao tác này sẽ khởi tạo cả cửa sổ hiện tại và đường cơ sở để so sánh. 
2. Đặt cả mức tối thiểu tốt nhất và mức tối đa tốt nhất cho tổng đầu tiên này và ghi lại chỉ số 1 cho cả hai. 
3. Di chuyển cửa sổ từng bước một từ trái sang phải. 
4. Đối với mỗi bước, hãy cập nhật tổng hiện tại bằng cách trừ phần tử rời khỏi cửa sổ và thêm phần tử mới vào. 
5. Nếu tổng mới hoàn toàn nhỏ hơn mức tối thiểu tốt nhất, hãy cập nhật giá trị tối thiểu và lưu trữ chỉ mục hiện tại. 
6. Nếu tổng mới lớn hơn mức tối đa tốt nhất, hãy cập nhật giá trị tối đa và lưu trữ chỉ mục hiện tại. 

Sự so sánh chặt chẽ là cần thiết. Nếu chúng tôi sử dụng so sánh không nghiêm ngặt, chúng tôi sẽ ghi đè các câu trả lời hợp lệ trước đó và vi phạm yêu cầu trả về lần xuất hiện đầu tiên. 

### Tại sao nó hoạt động 

Mỗi cửa sổ có thể được kiểm tra chính xác một lần theo thứ tự. Tổng trượt đảm bảo tính chính xác của từng giá trị cửa sổ và các cập nhật đơn điệu đảm bảo rằng chỉ những ứng cử viên tốt hơn mới thay thế giá trị tốt nhất hiện tại. Vì chúng tôi không bao giờ ghi đè lên các mối quan hệ nên lần xuất hiện đầu tiên được giữ nguyên bằng cách xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    v = list(map(int, input().split()))

    window_sum = sum(v[:k])

    min_sum = window_sum
    max_sum = window_sum
    min_idx = 1
    max_idx = 1

    for i in range(k, n):
        window_sum += v[i] - v[i - k]
        start_idx = i - k + 2

        if window_sum < min_sum:
            min_sum = window_sum
            min_idx = start_idx

        if window_sum > max_sum:
            max_sum = window_sum
            max_idx = start_idx

    print(min_idx, max_idx)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách đọc đầu vào và tính tổng cửa sổ đầu tiên một cách rõ ràng. Vòng lặp sau đó duy trì tính bất biến của cửa sổ trượt. Tính toán chỉ số`i - k + 2`chuyển đổi điểm cuối bên phải dựa trên số 0 thành điểm bắt đầu cửa sổ dựa trên một. 

Việc so sánh sử dụng bất đẳng thức nghiêm ngặt để bảo toàn yêu cầu xuất hiện sớm nhất. Đây là nguồn lỗi phổ biến nhất trong vấn đề này: sử dụng`<=`hoặc`>=`âm thầm phá vỡ tính đúng đắn của mối quan hệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 1 1 2 3
```Chúng tôi theo dõi tổng số tiền của cửa sổ trượt. 

| Bước | Cửa sổ | Tổng hợp | Tối thiểu | Tối đa | 
| --- | --- | --- | --- | --- | 
| ban đầu | (1,1) | 2 | 1 | 1 | 
| tôi=2 | (1,1) | 2 | 1 | 1 | 
| tôi=3 | (1,2) | 3 | 1 | 2 | 
| tôi=4 | (2,3) | 5 | 1 | 4 | 

Mức tối thiểu xảy ra ở chỉ số 1, tối đa ở chỉ số 4. 

### Ví dụ 2 

đầu vào:```
6 4
1000 1 2 3 4 100
```| Bước | Cửa sổ | Tổng hợp | Tối thiểu | Tối đa | 
| --- | --- | --- | --- | --- | 
| ban đầu | (1000,1,2,3) | 1006 | 2 | 2 | 
| tôi=5 | (1,2,3,4) | 10 | 2 | 5 | 
| tôi=6 | (2,3,4,100) | 109 | 2 | 6 | 

Bắt đầu tối thiểu ở chỉ số 2, tối đa ở chỉ số 1. 

Những dấu vết này cho thấy rằng một khi tổng cửa sổ được tính toán, nó sẽ không bao giờ được tính toán lại hoặc xem xét lại không theo thứ tự, duy trì sự ràng buộc chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Một tổng ban đầu và một lần chuyển qua mảng | 
| Không gian |$O(1)$| Chỉ các tổng và chỉ số đang chạy được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí$2 \cdot 10^5$hoạt động là tầm thường trong thời gian tuyến tính. 

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

# provided sample
assert run("5 2\n1 1 1 2 3\n") == "1 4"

# minimum window size
assert run("5 1\n5 4 3 2 1\n") == "5 1"

# all equal
assert run("4 2\n7 7 7 7\n") == "1 1"

# increasing sequence
assert run("5 3\n1 2 3 4 5\n") == "1 3"

# decreasing sequence
assert run("5 2\n5 4 3 2 1\n") == "4 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 1 | xử lý cà vạt đúng cách | 
| K=1 | chỉ số cuối cùng và đầu tiên | hành vi ranh giới | 
| tăng đơn điệu | đúng tối đa ở cuối | trượt đúng đắn | 
| giảm đơn điệu | đúng phút ở cuối | cập nhật chỉ mục | 

## Vỏ cạnh 

Khi tất cả các cửa sổ có cùng tổng, thuật toán phải bảo toàn chỉ số đầu tiên cho cả giá trị tối thiểu và tối đa. Điều này được đảm bảo vì các cập nhật chỉ diễn ra khi cải tiến nghiêm ngặt, do đó chỉ mục ban đầu vẫn không bị ảnh hưởng. 

Khi$K = 1$, mỗi phần tử là cửa sổ riêng của nó. Cơ chế trượt vẫn hoạt động: mỗi bản cập nhật thay thế tổng trước đó bằng phần tử mới và so sánh xác định chính xác lần xuất hiện đầu tiên của giá trị tối thiểu và tối đa. 

Khi tất cả các giá trị đều bằng nhau, tổng của mọi cửa sổ đều giống hệt nhau, do đó không có bản cập nhật nào được kích hoạt. Đầu ra vẫn còn`(1, 1)`, phù hợp với yêu cầu lựa chọn lần xuất hiện đầu tiên.
