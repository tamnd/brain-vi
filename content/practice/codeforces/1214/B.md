---
title: "CF 1214B - Huy hiệu"
description: "Chúng ta được cho ba số nguyên mô tả một thiết lập tổ hợp nhỏ. Tổng cộng có một số lượng cố định nam và nữ, và có chính xác $n$ người tham gia sẽ tham dự một sự kiện, nhưng chúng tôi không biết trong số họ có bao nhiêu nam hay nữ."
date: "2026-06-13T17:26:19+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1214
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 583 (Div. 1 + Div. 2, based on Olympiad of Metropolises)"
rating: 1100
weight: 1214
solve_time_s: 304
verified: true
draft: false
---

[CF 1214B - Huy hiệu](https://codeforces.com/problemset/problem/1214/B) 

**Đánh giá:** 1100 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 5 phút 4 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên mô tả một thiết lập tổ hợp nhỏ. Tổng cộng có một số lượng cố định nam và nữ, và chính xác$n$những người tham gia sẽ tham dự một sự kiện, nhưng chúng tôi không biết bao nhiêu người trong số họ là nam hay nữ. Số lượng nam sinh thực tế trong sự kiện có thể dao động từ 0 đến$n$, miễn là nó không vượt quá số lượng bé trai sẵn có, và tương tự số lượng bé gái được xác định bởi phần còn lại. 

Đối với mọi sự phân chia có thể có của$n$người tham gia thành nam và nữ, chúng ta cần có khả năng cấp huy hiệu chính xác. Mỗi sự phân chia có thể yêu cầu một loại bộ huy hiệu cụ thể: nếu có$i$các bạn, chúng ta cần một bộ bài chứa chính xác$i$phù hiệu màu xanh và$n-i$phù hiệu màu đỏ. 

Chúng tôi được trao tất cả$n+1$những bộ bài có thể, nhưng chúng ta không bắt buộc phải lấy tất cả chúng. Nhiệm vụ là chọn tập con nhỏ nhất của các bộ bài này sao cho với mọi giá trị khả thi của$i$điều đó thực sự có thể xảy ra do những hạn chế về tổng số nam và nữ, ít nhất một bộ bài được chọn phù hợp với nó. 

Kích thước đầu vào nhỏ, giới hạn bởi 300. Điều này loại bỏ mọi áp lực đối với cấu trúc hoặc tối ưu hóa dữ liệu nâng cao; một số học trực tiếp hoặc lý luận tham lam là đủ. Quét bậc hai hoặc thậm chí tuyến tính là hoàn toàn có thể chấp nhận được. 

Trường hợp cạnh tinh tế xuất hiện khi một số giá trị của$i$là không thể vì thiếu con trai hay con gái. Ví dụ: nếu chỉ có 2 chàng trai, thì bất kỳ bộ bài nào yêu cầu 3 huy hiệu màu xanh trở lên đều không liên quan ngay cả khi nó tồn tại. Một cách tiếp cận ngây thơ luôn giả định tất cả$0 \ldots n$các giá trị có thể xảy ra sẽ vượt quá số bộ bài được yêu cầu. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là xem xét mọi số lượng con trai có thể$i$trong giải đấu, từ 0 đến$n$và kiểm tra xem cấu hình đó có khả thi hay không. Một cấu hình chỉ khả thi nếu$i \le b$Và$n-i \le g$. Đối với mỗi khả thi$i$, chúng ta phải bao gồm bộ bài tương ứng. 

Điều này ngay lập tức gợi ý một thao tác quét đơn giản: đánh dấu tất cả hợp lệ$i$, sau đó đếm chúng. Điều này đã giải quyết được vấn đề một cách chính xác vì mỗi giá trị hợp lệ$i$tương ứng với một bộ bài được yêu cầu riêng biệt và không bộ bài nào có thể phục vụ hai giá trị khác nhau của$i$vì mỗi bộ bài mã hóa một số lượng huy hiệu màu xanh lam cố định. 

Việc “tối ưu hóa” đối với vũ lực chủ yếu chỉ mang tính khái niệm. Chúng ta không cần phải suy luận về các tập hợp con hoặc các phần chồng chéo vì cấu trúc này là tuyến tính: mỗi phần$i$bản đồ tới chính xác một bộ bài. Nhiệm vụ thực sự duy nhất là xác định phạm vi hợp lệ$i$. 

Từ bất đẳng thức:$i \le b$Và$n-i \le g \Rightarrow i \ge n-g$. 

Rất hợp lệ$i$nằm ở ngã tư:$$\max(0, n-g) \le i \le \min(n, b)$$Câu trả lời đơn giản là kích thước của khoảng này, nếu nó không trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tính toán khoảng thời gian | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số nam nhỏ nhất có thể có trong giải đấu là$L = \max(0, n - g)$. Điều này đảm bảo chúng tôi không vượt quá số lượng nữ sinh sẵn có khi xếp thành những người tham gia còn lại. 
2. Tính số học sinh nam lớn nhất có thể là$R = \min(n, b)$, vì chúng tôi không thể vượt quá tổng số người tham gia hoặc số nam sinh có sẵn. 
3. Nếu$L > R$, không có cấu hình hợp lệ nào tồn tại, nghĩa là không có bộ bài nào thực sự cần thiết để đáp ứng một kịch bản khả thi, vì vậy câu trả lời là 0. 
4. Ngược lại, tất cả các giá trị nguyên$i$trong phạm vi$[L, R]$tương ứng với các kịch bản khả thi và mỗi kịch bản yêu cầu một bộ bài riêng biệt. 
5. Do đó, số lượng bộ bài cần thiết là$R - L + 1$. 

### Tại sao nó hoạt động 

Mỗi cấu hình giải đấu khả thi được xác định duy nhất bởi số lượng nam sinh$i$. Những ràng buộc hạn chế$i$đến một khoảng liền kề. Mỗi số nguyên trong khoảng này tương ứng với một thành phần huy hiệu bắt buộc riêng biệt và mỗi thành phần được thể hiện bằng chính xác một bộ bài. Bởi vì không có sự trùng lặp giữa các định nghĩa bộ bài, việc bao phủ tất cả các trường hợp khả thi sẽ giảm chính xác xuống việc bao phủ mọi số nguyên trong khoảng này. Không có chiến lược lựa chọn bổ sung nào có thể giảm số lượng mà không bỏ lỡ một giá trị hợp lệ$i$. 

## Giải pháp Python```
PythonRun
```Giải pháp tính toán trực tiếp các ranh giới khoảng khả thi. Ranh giới đầu tiên đảm bảo chúng ta có đủ nữ để lấp đầy phần còn lại$n - i$khe cắm. Ranh giới thứ hai đảm bảo chúng tôi không vượt quá số lượng nam sinh hoặc tổng số người tham gia. 

Chi tiết triển khai chính là xử lý cẩn thận các giới hạn giao lộ. các`max(0, n - g)`ngăn ngừa giới hạn dưới âm khi có nhiều bé gái, và`min(n, b)`giới hạn giới hạn trên một cách chính xác ngay cả khi số lượng nam sinh vượt quá$n$. Kiểm tra cuối cùng`L > R`là cần thiết để tránh số âm khi khoảng khả thi trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Tính giới hạn: 

| Bước | L = max(0, n-g) | R = phút(n, b) | Khoảng thời gian hợp lệ | 
| --- | --- | --- | --- | 
| Ban đầu | tối đa(0, 3-6)=0 | phút(3,5)=3 | [0, 3] | 

Đáp án = 4 

Điều này chứng tỏ một trường hợp có thể thực hiện được tất cả các phân phối từ 0 đến 3 bé trai vì cả hai nguồn lực đều đủ. Mỗi bộ bài đều cần thiết. 

### Ví dụ 2 

đầu vào:```

```| Bước | L | R | Khoảng thời gian hợp lệ | 
| --- | --- | --- | --- | 
| Ban đầu | tối đa(0,3-6)=0 | phút(3,3)=3 | [0,3] | 

Đáp án = 4 

Bây giờ hãy xem xét một trường hợp chặt chẽ hơn: 

đầu vào:```

```| Bước | L | R | Khoảng thời gian hợp lệ | 
| --- | --- | --- | --- | 
| Ban đầu | tối đa(0,3-2)=1 | phút(3,5)=3 | [1,3] | 

Đáp án = 3 

Điều này cho thấy rằng khi con gái bị hạn chế, giá trị thấp của$i$trở nên bất khả thi vì họ sẽ cần quá nhiều cô gái. 

Dấu vết xác nhận tính bất biến chính: chỉ những phân chia khả thi của những người tham gia mới đóng góp vào số lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài một vài số nguyên | 

Các ràng buộc cho phép lặp lại thậm chí đơn giản, nhưng việc xây dựng khoảng thời gian làm giảm giải pháp về thời gian không đổi, là tối ưu. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 2 | khoảng không tầm thường nhỏ nhất | 
| 0 5 3 | 0 | cấu hình không thể | 
| 5 2 3 | 3 | chặt chẽ cô gái ràng buộc | 

## Vỏ cạnh 

Khi con gái dồi dào và con trai là yếu tố hạn chế, giới hạn dưới$L = \max(0, n-g)$trở thành 0 và khoảng mở rộng lên trên cho đến khi$b$. Thuật toán chính xác bao gồm tất cả các khả năng$i$từ 0 đến$b$. 

Khi con trai thì nhiều nhưng con gái thì khan hiếm, giới hạn trên là$R = n$nhưng giới hạn dưới dịch chuyển lên trên$n-g$, loại bỏ các giá trị nhỏ không thể có của$i$. Khoảng thời gian được tính toán vẫn tiếp giáp và có kích thước chính xác. 

Khi cả hai$b$Và$g$quá nhỏ để tạo thành bất kỳ phép chia hợp lệ nào, điều kiện$L > R$kích hoạt, tạo ra 0, phù hợp với thực tế là không có phân phối người tham gia nào khả thi.
