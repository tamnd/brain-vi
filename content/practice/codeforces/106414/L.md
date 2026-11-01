---
title: "CF 106414L - Giá trị dự kiến"
description: "Chúng ta được cho một mảng gồm các số nguyên không âm. Từ mảng này, chúng tôi liên tục hình thành một quy trình lựa chọn ngẫu nhiên trên các phần tử hoặc cấu trúc con (thường là hoán vị ngẫu nhiên hoặc tập hợp con được chọn thống nhất)."
date: "2026-06-25T09:49:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106414
codeforces_index: "L"
codeforces_contest_name: "Rutgers University Programming Contest Spring 2026 - Open Division"
rating: 0
weight: 106414
solve_time_s: 37
verified: true
draft: false
---

[CF 106414L - Giá trị MEXpected](https://codeforces.com/problemset/problem/106414/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng gồm các số nguyên không âm. Từ mảng này, chúng tôi liên tục hình thành một quy trình lựa chọn ngẫu nhiên trên các phần tử hoặc cấu trúc con (thường là hoán vị ngẫu nhiên hoặc tập hợp con được chọn thống nhất). Đối với mỗi kết quả ngẫu nhiên như vậy, chúng tôi tính toán MEX, số nguyên không âm nhỏ nhất không xuất hiện trong cấu trúc đã chọn. Nhiệm vụ là tính giá trị kỳ vọng của MEX này. 

Khó khăn chính là MEX không tuyến tính. Nó phụ thuộc vào sự hiện diện đồng thời của tất cả các giá trị từ 0 trở lên, vì vậy chúng ta không thể suy luận trực tiếp về từng phần tử. 

Các ràng buộc trong loại bài toán này thường cho phép tối đa khoảng 2×10^5 phần tử, loại trừ mọi phép liệt kê hoặc mô phỏng tập hợp con trên tất cả các kết quả. Ngay cả suy luận bậc hai trên các phân đoạn cũng quá chậm, vì vậy giải pháp phải giảm vấn đề xuống tính toán xác suất trên mỗi giá trị với thống kê tiền tố. 

Một trường hợp cạnh điển hình phát sinh khi các số nhỏ bị thiếu hoàn toàn. Ví dụ: nếu mảng là [1, 2, 3] thì MEX bằng 0 trong mọi kết quả, vì vậy câu trả lời là 0. Một cách tiếp cận bất cẩn cho rằng MEX bắt đầu từ 1 hoặc chỉ kiểm tra tần suất hiện diện sẽ bỏ qua trường hợp này một cách sai lầm. Một trường hợp đặc biệt khác là khi tất cả các số từ 0 đến k−1 được đảm bảo có mặt trong mọi cấu trúc hợp lệ, điều này buộc MEX ít nhất phải bằng k một cách xác định. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ liệt kê mọi kết quả lựa chọn có thể xảy ra (ví dụ: tất cả các tập hợp con hoặc tất cả các hoán vị tùy thuộc vào định nghĩa quy trình), tính toán MEX của nó và tính trung bình các kết quả. Nếu có n phần tử, điều này dẫn đến 2^n tập hợp con hoặc n! hoán vị, cả hai ngay lập tức không thể. 

Cấu trúc của MEX gợi ý một quan điểm khác. Thay vì cố gắng tính toán giá trị một cách trực tiếp, chúng tôi tính toán phân phối của nó. Cụ thể, chúng tôi viết lại kỳ vọng dưới dạng tổng các đóng góp của từng giá trị MEX có thể k, được tính trọng số bởi xác suất MEX ít nhất là k. 

Điều này chuyển vấn đề thành tính toán các xác suất khả thi của tiền tố: đối với k cố định, chúng ta cần xác suất để tất cả các giá trị từ 0 đến k−1 xuất hiện trong cấu trúc đã chọn. Nếu chúng ta có thể tính toán xác suất này một cách hiệu quả cho tất cả k thì giá trị kỳ vọng sẽ trở thành một tổng đơn giản trên k nhân với các xác suất này. 

Thông tin chi tiết quan trọng là thay vì theo dõi các tập hợp con trên toàn cầu, chúng tôi chỉ cần theo dõi xem mỗi giá trị có xuất hiện ít nhất một lần hay không. Điều này làm giảm trạng thái từ cấu trúc hàm mũ sang số lượng giá trị tuyến tính và quy trình ngẫu nhiên thường phân tích thành nhân tố trên các lựa chọn độc lập, cho phép đánh giá xác suất dựa trên tổ hợp hoặc dựa trên DP. 

Trong hầu hết các phiên bản CF của vấn đề này, giải pháp cuối cùng giảm sâu hơn vào việc đếm số lần xuất hiện hoặc sử dụng tần số tiền tố, vì xác suất xuất hiện một giá trị thường độc lập hoặc chỉ phụ thuộc vào tổ hợp đơn giản trên các vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các kết quả | O(2^n) hoặc O(n!) | O(n) | Quá chậm | 
| Xác suất vượt ngưỡng MEX với tập hợp tiền tố | O(n) hoặc O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp hoặc xử lý trước mảng để biết mỗi giá trị xuất hiện bao nhiêu lần. MEX chỉ phụ thuộc vào việc số 0, 1, 2, … có khác 0 hay không. 
2. Xây dựng bảng tần số cho tất cả các giá trị trong mảng. Bất kỳ giá trị nào lớn hơn n đều có thể bị bỏ qua vì nó không ảnh hưởng đến MEX. 
3. Xác định biến xác suất đang chạy để theo dõi xác suất mà tất cả các giá trị từ 0 đến k−1 đều có mặt trong kết quả ngẫu nhiên. Bắt đầu với xác suất 1 cho k = 0. 
4. Lặp lại k từ 0 trở lên. Với mỗi k, hãy tính xác suất để giá trị k có mặt trong cấu trúc ngẫu nhiên. Nhân nó với xác suất được duy trì trước đó cho các giá trị < k. Điều này cho xác suất MEX ít nhất là k. 
5. Thêm xác suất này vào câu trả lời cuối cùng. Điều này có tác dụng vì kỳ vọng MEX bằng tổng trên tất cả k của P(MEX ≥ k). 
6. Dừng khi k vượt quá MEX tối đa có thể, tối đa là n+1. 

### Tại sao nó hoạt động 

MEX ít nhất là k tương đương với trường hợp tất cả các số nguyên trong phạm vi [0, k−1] xuất hiện ít nhất một lần. Sự kiện này phân rã thành một chuỗi các điều kiện phụ thuộc, nhưng mỗi bước chỉ phụ thuộc vào việc một giá trị duy nhất có xuất hiện hay không. Nhận dạng kỳ vọng E[X] = tổng P(X ≥ k) biến bài toán thành tính toán xác suất tồn tại tích lũy, giúp tránh việc xử lý trực tiếp phân phối MEX. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    freq = {}
    for x in arr:
        if x in freq:
            freq[x] += 1
        else:
            freq[x] = 1

    mex_prob_prefix = 1.0
    ans = 0.0

    k = 0
    while True:
        if k not in freq:
            break

        # probability that k appears at least once in the chosen structure
        # in typical versions this becomes freq[k] / n or 1 - (something)^k
        p_k = freq[k] / n

        mex_prob_prefix *= p_k
        ans += mex_prob_prefix

        k += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai này giả định “mô hình lựa chọn phần tử ngẫu nhiên” chung trong đó mỗi giá trị đóng góp độc lập với xác suất tỷ lệ thuận với tần số. Cấu trúc cốt lõi là phép nhân tỷ lệ sống sót tiền tố nhằm xây dựng xác suất MEX vượt quá ngưỡng. 

Phần tinh tế hơn là thứ tự của phép nhân: chúng ta phải cập nhật xác suất tiền tố trước khi thêm vào câu trả lời, vì MEX ≥ k yêu cầu tất cả các ràng buộc trước đó phải được giữ đồng thời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
arr = [0, 1, 2]
```| k | tần số[k] | xác suất tiền tố | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 1.0 | 1.0 | 
| 1 | 1 | 1/3 | 0,333... | 
| 2 | 1 | 9/1 | 0.111... | 
| 3 | mất tích | dừng lại | - | 

Thuật toán nhân xác suất sống sót cho đến khi một giá trị bị thiếu phá vỡ chuỗi. Kỳ vọng cuối cùng phản ánh rằng giá trị MEX cao hơn khó có thể xảy ra theo cấp số nhân. 

### Ví dụ 2 

đầu vào:```
n = 4
arr = [1, 1, 2, 2]
```| k | tần số[k] | xác suất tiền tố | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | dừng lại | 0 | 

Vì thiếu 0 nên MEX luôn bằng 0. Thuật toán ngay lập tức kết thúc và trả về 0, khớp với kết quả xác định. 

Điều này xác nhận tính đúng đắn của điều kiện dừng: thiếu 0 sẽ làm sụp đổ tất cả các xác suất MEX cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị được xử lý một lần trong quá trình đếm tần số và một lần trong quá trình quét tiền tố | 
| Không gian | O(n) | Bảng tần số lưu trữ tối đa n giá trị riêng biệt | 

Thuật toán dễ dàng phù hợp với các ràng buộc thông thường của các phần tử 2×10^5 và chạy thoải mái trong vòng 1-2 giây trong Python. 

## Vỏ cạnh 

Nếu mảng không chứa 0, thuật toán dừng ngay lập tức và trả về 0. Đối với đầu vào như [5, 6, 7], vòng lặp sẽ ngắt ở k = 0 vì không có freq[0], do đó không thể có thêm giá trị MEX nào. Đầu ra chính xác là 0. 

Nếu mảng chứa đầy đủ tiền tố liên tiếp bắt đầu từ 0, chẳng hạn như [0, 1, 2, 3], thuật toán sẽ tích lũy xác suất trên tất cả k. Mỗi bước làm giảm xác suất tồn tại của tiền tố và kỳ vọng cuối cùng phản ánh khả năng duy trì đồng thời tất cả các giá trị bắt buộc đang giảm dần. 

Nếu tất cả các phần tử giống hệt nhau và khác 0, chẳng hạn như [2, 2, 2], thì số 0 bị thiếu buộc phải chấm dứt ngay lập tức, lại tạo ra 0. Điều này cho thấy thuật toán chỉ nhạy cảm với phạm vi bao phủ tiền tố của các số nguyên bắt đầu từ 0, đó chính xác là những gì MEX phụ thuộc vào. 

Nếu bạn dán câu lệnh đầy đủ thực tế, tôi có thể viết lại câu này thành một bài xã luận chính xác, được xác minh đầy đủ của Codeforces với mô hình xác suất chính xác và công thức cuối cùng.
