---
title: "CF 1165B - Đào tạo Polycarp"
description: "Chúng ta được đưa cho một danh sách các cuộc thi, mỗi cuộc thi có một số vấn đề nhất định. Polycarp luyện tập hàng ngày và vào ngày $k$, anh ta phải chọn chính xác một cuộc thi chưa sử dụng và giải chính xác $k$ các bài toán từ đó."
date: "2026-06-13T08:42:45+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1165
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 560 (Div. 3)"
rating: 1000
weight: 1165
solve_time_s: 85
verified: false
draft: false
---

[CF 1165B - Đào tạo Polycarp](https://codeforces.com/problemset/problem/1165/B) 

**Đánh giá:** 1000 
**Tags:** cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một danh sách các cuộc thi, mỗi cuộc thi có một số vấn đề nhất định. Polycarp tập luyện hàng ngày và hàng ngày$k$, anh ta phải chọn chính xác một cuộc thi chưa sử dụng và giải chính xác$k$vấn đề từ đó. Một cuộc thi có thể được sử dụng một phần: nếu nó có nhiều vấn đề hơn mức cần thiết thì phần còn lại sẽ bị loại bỏ. Nếu nó có ít hơn$k$vấn đề, nó không thể được sử dụng cho ngày hôm đó. 

Quá trình này diễn ra tuần tự theo ngày. Vào mỗi ngày, chúng tôi chỉ định một cuộc thi đủ lớn chưa được sử dụng trước đó hoặc chúng tôi dừng lại nếu không có cuộc thi nào như vậy tồn tại. Mỗi cuộc thi chỉ có thể được sử dụng tối đa một lần, vì vậy thứ tự của bài tập rất quan trọng. 

Mục tiêu là tối đa hóa số ngày liên tục anh ta có thể hoàn thành quá trình này. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các cuộc thi. Một giải pháp bậc hai về số lượng cuộc thi sẽ quá chậm vì nó có thể yêu cầu theo thứ tự$10^{10}$hoạt động. Điều này ngay lập tức gợi ý rằng chúng ta cần nhiều nhất$O(n \log n)$hoặc$O(n)$hành vi, có thể liên quan đến việc phân loại và chiến lược lựa chọn tham lam. 

Một trường hợp thất bại tinh tế xuất hiện khi các cuộc thi lớn được sử dụng quá sớm. Ví dụ: nếu chúng tôi tham gia một cuộc thi lớn vào một ngày nhỏ, chúng tôi có thể chặn một ngày trong tương lai yêu cầu ngưỡng thậm chí còn lớn hơn, trong khi một cuộc thi nhỏ hơn sẽ là đủ sớm hơn. Ngược lại, bỏ qua các cuộc thi nhỏ sớm cũng có thể làm giảm tính linh hoạt sau này. Điều này cho thấy chúng ta phải kết hợp cẩn thận các yêu cầu trong ngày nhỏ với các cuộc thi lớn vừa đủ. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp cố gắng chỉ định một cuộc thi mỗi ngày$k = 1, 2, 3, \dots$. Mỗi ngày, chúng tôi quét tất cả các cuộc thi còn lại và chọn bất kỳ cuộc thi nào có kích thước tối thiểu$k$. Điều này đúng vì nó trực tiếp tuân theo các quy tắc. Tuy nhiên, mỗi ngày yêu cầu quét tới$O(n)$các cuộc thi, và chúng tôi có thể có$O(n)$ngày, dẫn đến$O(n^2)$hoạt động quá chậm đối với$2 \cdot 10^5$. 

Nhận xét quan trọng là chúng ta không bao giờ cần xem xét các cuộc thi theo thứ tự ban đầu. Chỉ có kích thước của chúng là quan trọng. Sau khi được sắp xếp, chúng ta có thể suy luận về tính khả thi một cách tham lam: nếu chúng ta đang ở$k$, chúng tôi chỉ quan tâm liệu có tồn tại bất kỳ cuộc thi nào chưa được sử dụng với quy mô ít nhất$k$. Vì muốn tối đa hóa số ngày nên trước tiên chúng tôi cố gắng đáp ứng các yêu cầu nhỏ bằng cách sử dụng các cuộc thi nhỏ, giữ nguyên những yêu cầu lớn hơn cho những ngày sau. 

Điều này dẫn đến một chiến lược kết hợp tham lam. Chúng tôi sắp xếp quy mô cuộc thi theo thứ tự tăng dần và duy trì con trỏ tới cuộc thi nhỏ nhất chưa được sử dụng có thể đáp ứng ngày hiện tại. Chúng tôi tiến qua mảng đã sắp xếp và bất cứ khi nào quy mô cuộc thi ít nhất đáp ứng yêu cầu của ngày hiện tại, chúng tôi sẽ sử dụng nó và chuyển sang ngày tiếp theo. Nếu không, chúng tôi bỏ qua nó vì nó quá nhỏ và cũng không bao giờ có thể hữu ích cho những ngày lớn hơn trong tương lai. 

Điều này hiệu quả vì yêu cầu mỗi ngày ngày càng tăng lên, do đó, một cuộc thi quá nhỏ bây giờ sẽ không bao giờ trở nên hữu ích sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(1)$hoặc$O(n)$| Quá chậm | 
| Tham lam + Sắp xếp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả quy mô cuộc thi theo thứ tự không giảm. Việc sắp xếp là cần thiết vì chúng tôi muốn xử lý các cuộc thi nhỏ trước và tránh lãng phí sớm những cuộc thi lớn. 
2. Khởi tạo con trỏ`i = 0`theo dõi cuộc thi chưa được sử dụng nhỏ nhất hiện tại và một biến`day = 1`đại diện cho yêu cầu ngày đào tạo hiện tại. 
3. Trong khi`i < n`, kiểm tra xem cuộc thi hiện tại`a[i]`có thể hỗ trợ ngày hiện tại, nghĩa là`a[i] >= day`. 
4. Nếu`a[i] < day`, cuộc thi này quá nhỏ ngay cả đối với yêu cầu hiện tại, vì vậy chúng tôi loại bỏ nó và tăng thêm`i`. Điều này là an toàn vì ngày mai còn đòi hỏi nhiều vấn đề hơn nữa nên cuộc thi này sẽ không bao giờ trở nên hữu ích. 
5. Nếu`a[i] >= day`, chúng tôi chỉ định cuộc thi này vào ngày`day`, tăng cả hai`day`Và`i`, từ
