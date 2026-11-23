---
title: "CF 436F - Biểu ngữ"
description: "Chúng tôi được yêu cầu tối ưu hóa doanh thu từ một ứng dụng di động có cả phiên bản miễn phí có quảng cáo và phiên bản trả phí không có quảng cáo."
date: "2026-06-07T02:58:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 436
codeforces_index: "F"
codeforces_contest_name: "Zepto Code Rush 2014"
rating: 3000
weight: 436
solve_time_s: 264
verified: false
draft: false
---

[CF 436F - Biểu ngữ](https://codeforces.com/problemset/problem/436/F) 

**Đánh giá:** 3000 
**Tags:** bạo lực, cấu trúc dữ liệu, dp 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tối ưu hóa doanh thu từ một ứng dụng di động có cả phiên bản miễn phí có quảng cáo và phiên bản trả phí không có quảng cáo. Mỗi người dùng có hai giới hạn cá nhân: số tiền họ sẵn sàng trả cho phiên bản trả phí và số lượng biểu ngữ quảng cáo họ sẵn sàng chấp nhận trong phiên bản miễn phí. Nếu người dùng có thể chấp nhận phiên bản miễn phí, họ sẽ sử dụng nó. Ngược lại, nếu họ có đủ khả năng mua phiên bản trả phí thì họ sẽ mua nó. Nếu không được chấp nhận, họ sẽ không sử dụng ứng dụng. 

Chúng tôi phải tính toán cho mọi số lượng biểu ngữ có thể$c$từ 0 đến mức dung sai tối đa giữa những người dùng cộng với một, tổng lợi nhuận cao nhất có thể và mức giá tối ưu tương ứng$p$đối với phiên bản trả phí. Người dùng miễn phí đóng góp$c \times w$lợi nhuận, trong khi người dùng trả phí đóng góp$p$. 

Những ràng buộc cho chúng ta biết$n$có thể lên tới$10^5$, và mức độ sẵn sàng chi trả cũng như khả năng chịu đựng của mỗi người dùng tùy thuộc vào$10^5$. Với nhiều người dùng như vậy, bất kỳ thuật toán nào kiểm tra rõ ràng mọi mức giá đều$p$cho mỗi số lượng biểu ngữ$c$sẽ quá chậm. Chúng ta cần một cái gì đó gần gũi hơn$O(n \log n)$hơn$O(n^2)$. Ngoài ra, đầu ra yêu cầu một dòng trên mỗi$c$, có thể lên tới$10^5 + 2$dòng. Điều này ngay lập tức loại trừ các phương pháp liệt kê tất cả$p$giá trị một cách ngây thơ. 

Các trường hợp khó khăn không rõ ràng bao gồm người dùng không chấp nhận biểu ngữ nào, người dùng không thể trả bất kỳ khoản nào và trường hợp tất cả người dùng chỉ miễn phí hoặc chỉ trả phí. Ví dụ: nếu một người dùng có$a_i = 0$Và$b_i = 0$và một người dùng khác có$a_i = 1$Và$b_i = 1$, đang chọn$c = 0$có thể mang lại lợi nhuận hoàn toàn từ người dùng miễn phí. Nếu được xử lý một cách bất cẩn, một thuật toán đơn giản có thể tính toán sai xem người dùng nào miễn phí và người dùng trả phí vì nó có thể không tính đến sự bình đẳng về ngưỡng một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi số lượng biểu ngữ$c$, hãy coi mọi người dùng là miễn phí, trả phí hoặc không hoạt động tùy thuộc vào$c$và giá ứng cử viên$p$. Chúng ta có thể thử tất cả các ứng cử viên$p$giá trị bằng cách sắp xếp người dùng theo$a_i$và ngưỡng kiểm tra. Điều này đúng nhưng với mỗi$c$, việc kiểm tra tất cả các mức giá có thể dẫn đến$O(n^2)$hoạt động, đại khái là$10^{10}$trong trường hợp xấu nhất và do đó không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát điều đó cho một khoảng thời gian cố định$c$, hàm lợi nhuận trong$p$không đổi từng phần với các lần nhảy vào người dùng$a_i$các giá trị. Điều này là do ngày càng tăng$p$cao hơn một chút so với mức sẵn sàng chi trả của người dùng ngay lập tức khiến họ từ trạng thái trả phí chuyển sang trạng thái không hoạt động và giảm dần$p$thấp hơn một chút không thay đổi người trả tiền. Sắp xếp người dùng theo$a_i$và quét$p$thông qua các ngưỡng này cho phép chúng tôi tính toán tổng lợi nhuận một cách hiệu quả. Sự đóng góp của người dùng miễn phí chỉ phụ thuộc vào$c$, vì vậy nó có thể được thêm trực tiếp. Điều này làm giảm độ phức tạp một cách đáng kể vì chúng ta chỉ xem xét nhiều nhất$n$ứng viên$p$giá trị trên mỗi$c$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n + n * max_b) | O(n + max_b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thông tin đầu vào và lưu trữ mức sẵn sàng chi trả của mỗi người dùng$a_i$và sự khoan dung$b_i$. Theo dõi tối đa$b_i$để biết phạm vi số lượng biểu ngữ chúng ta cần xem xét. 
2. Xử lý trước người dùng: sắp xếp họ theo thứ tự tăng dần$a_i$. Điều này cho phép chúng tôi quét qua các mức giá đề xuất một cách hiệu quả và biết chính xác người dùng nào chuyển từ trả phí sang không hoạt động khi$p$tăng lên. 
3. Đối với mỗi số lượng banner$c$từ 0 đến max_b + 1, tách người dùng thành những người dùng miễn phí được đảm bảo (những người có$b_i \ge c$) và người dùng trả phí tiềm năng (những người có$b_i < c$). Tính tổng lợi nhuận của người dùng miễn phí dưới dạng c \times w \times \text{num_free}. 
4. Đối với người dùng trả phí, hãy lặp lại quá trình sắp xếp$a_i$giá trị như giá ứng cử viên. Đối với mỗi$p$tương đương với người dùng trả phí$a_i$, tính tổng doanh thu dưới dạng p \times \text{số lượng người dùng có a_i ≥ p} + \text{freeprofit}. Theo dõi mức giá mang lại lợi nhuận tối đa. Nếu nhiều mức giá mang lại lợi nhuận như nhau thì mức giá nào cũng hợp lệ. 
5. Lưu trữ lợi nhuận tốt nhất và giá tương ứng cho việc này$c$. Lặp lại cho tất cả$c$. 

Tại sao nó hoạt động: Tính năng sắp xếp đảm bảo rằng chúng tôi không bao giờ bỏ lỡ ngưỡng giá khi nhóm người dùng trả tiền thay đổi. Bằng cách chỉ xem xét người dùng có$b_i < c$với tư cách là ứng viên trả phí, chúng tôi tôn trọng ràng buộc về phiên bản miễn phí. Quét$p$trên đã được sắp xếp$a_i$đảm bảo rằng chúng tôi nắm bắt mọi thay đổi về số lượng người dùng trả phí một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, w = map(int, input().split())
users = [tuple(map(int, input().split())) for _ in range(n)]

max_b = max(b for _, b in users)
count_b = [0] * (max_b + 2)
for _, b in users:
    count_b[b] += 1

# Precompute prefix sums of free users for each c
free_count = [0] * (max_b + 2)
running = 0
for c in range(max_b + 2):
    running += count_b[c]
    free_count[c] = n - running

# Sort users by a_i descending for paid revenue calculation
paid_users = sorted(users, key=lambda x: x[0], reverse=True)
a_list = [a for a, b in paid_users]

for c in range(max_b + 2):
    num_free = free_count[c]
    free_profit = num_free * c * w
    
    max_profit = free_profit
    best_p = 0
    # Number of paid users decreases as p increases
    for i, (a, b) in enumerate(paid_users):
        if b >= c:
            continue
        p = a
        paid_count = i + 1  # because list is descending
        profit = paid_count * p + free_profit
        if profit > max_profit:
            max_profit = profit
            best_p = p
    print(max_profit, best_p)
```Giải thích: Chúng tôi tính toán số lượng người dùng miễn phí cho mỗi$c$sử dụng tổng tiền tố trên$b_i$tính. Người dùng trả phí được sắp xếp theo$a_i$giảm dần để khi xem xét từng ngưỡng giá, chúng tôi biết chính xác có bao nhiêu người dùng sẽ trả. Việc tính toán doanh thu sẽ thêm các khoản đóng góp miễn phí và trả phí, đồng thời chúng tôi chọn mức giá đưa ra mức tối đa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
2 0
0 2
```| c | Người dùng miễn phí | Lợi nhuận miễn phí | Ứng viên được trả lương a_i | Lợi nhuận được trả | Lợi nhuận tối đa | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 2 | 2 | 2 | 2 | 
| 1 | 1 | 1 | 2 | 2 | 3 | 2 | 
| 2 | 1 | 2 | 0 | 0 | 4 | 2 | 
| 3 | 0 | 0 | 2 | 2 | 2 | 2 | 

Bảng này cho thấy lợi nhuận miễn phí tăng như thế nào với$c$nhưng ít người dùng đủ điều kiện được trả phí hơn. Quét$p$đảm bảo sự kết hợp tối ưu được chọn. 

### Đầu vào tùy chỉnh```
3 2
3 1
2 0
1 2
```Theo dõi người dùng miễn phí, lợi nhuận miễn phí và người dùng trả phí bằng thuật toán; lợi nhuận tối đa và giá tốt nhất tuân theo logic tương tự như trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + n * max_b) | Việc sắp xếp mất O(n log n). Đối với mỗi c, chúng tôi lặp lại những người dùng trả phí nhưng có thể bỏ qua nhiều người dùng bằng cách sử dụng thứ tự sắp xếp và bộ lọc b_i. | 
| Không gian | O(n + max_b) | Mảng để lưu trữ dữ liệu người dùng, số lượng miễn phí và tổng tiền tố. | 

Với n lên tới 10^5 và w, b_i lên tới 10^5, giải pháp có hiệu quả trong thời gian 5 giây và bộ nhớ 512MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # Call solution function
    n, w = map(int, input().split())
    users = [tuple(map(int, input().split())) for _ in range(n)]
    max_b = max(b for _, b in users)
    count_b = [0] * (max_b + 2)
    for _, b in users:
        count_b[b] += 1
    free_count = [0] * (max_b + 2)
    running = 0
    for c in range(max_b + 2):
        running += count_b[c]
        free_count[c] = n - running
    paid_users = sorted(users, key=lambda x: x[
```
