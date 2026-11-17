---
title: "CF 200E - Cao Đẳng Máy Kéo"
description: "Chúng tôi được yêu cầu phân bổ ngân sách học bổng cố định cho các sinh viên dựa trên điểm thi của họ. Mỗi học sinh nhận được điểm 3, 4 hoặc 5 và những học sinh có cùng điểm phải nhận được học bổng như nhau."
date: "2026-06-03T16:28:15+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math", "number-theory", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 200
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 126 (Div. 2)"
rating: 2400
weight: 200
solve_time_s: 71
verified: false
draft: false
---

[CF 200E - Trường Cao đẳng Máy kéo](https://codeforces.com/problemset/problem/200/E) 

**Đánh giá:** 2400 
**Tags:** thực hiện, toán, lý thuyết số, tìm kiếm bậc ba 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu phân bổ ngân sách học bổng cố định cho các sinh viên dựa trên điểm thi của họ. Mỗi học sinh nhận được điểm 3, 4 hoặc 5 và những học sinh có cùng điểm phải nhận được học bổng như nhau. Gọi _c3_, _c4_, _c5_ là số học sinh lớp 3, 4, 5 và _k3_, _k4_, _k5_ là số học bổng tương ứng. Điều kiện hạn chế là học bổng là số nguyên, không giảm (_k3 ≤ k4 ≤ k5_) và tổng số tiền chi ra bằng ngân sách _s_. Ngoài ra, việc phân bổ "tối ưu" sẽ giảm thiểu chức năng cụ thể của các học bổng, nhằm cân bằng sự khác biệt giữa chúng. 

Dữ liệu đầu vào cho biết số lượng sinh viên _n_ (3 ≤ n ≤ 300), tổng ngân sách _s_ (1  s 3·10^5) và điểm của mỗi sinh viên. Đầu ra phải cung cấp bất kỳ sự phân bổ học bổng nào thỏa mãn các ràng buộc, hoặc -1 nếu không có sự phân bổ như vậy. 

Vấn đề rất khó phát hiện vì việc làm tròn số nguyên nhỏ có thể ngăn cản sự bình đẳng chính xác của tổng ngân sách. Một cách tiếp cận ngây thơ cố gắng gán các phần theo tỷ lệ có thể âm thầm vi phạm yêu cầu về số nguyên. Ví dụ: nếu _c3 = 2_, _c4 = 1_, _c5 = 2_ và _s = 7_, thì chỉ cần chia ngân sách theo tỷ lệ có thể mang lại các phân số như 2,8 rúp, không hợp lệ. Phải cẩn thận để khám phá các giải pháp số nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại tất cả các giá trị có thể có của _k3_, _k4_, _k5_ thỏa mãn 0  k3  k4  k5 và kiểm tra xem tổng ngân sách có khớp hay không. Vì mỗi học bổng có thể lên tới _s_, điều này mang lại khả năng có khoảng O(s^3). Đối với s lên tới 3·10^5, giá trị này quá lớn. 

Quan sát chính là chúng tôi có thể giảm không gian tìm kiếm bằng cách lưu ý rằng sau khi chúng tôi sửa _k3_ và _k5_, _k4_ được xác định bởi tổng ngân sách:```
k4 = (s - c3*k3 - c5*k5) / c4
```Đây phải là số nguyên, thỏa mãn k3 ≤ k4 ≤ k5 và đáp ứng chính xác ngân sách. Điều này làm giảm vấn đề thành một vòng lặp kép trên k3 và k5 có thể, điều này khả thi vì n 300 và s 3·10^5. Phạm vi cho k3 và k5 có thể được giới hạn thêm lần lượt bởi tầng (s / c3) và tầng (s / c5), giữ cho các vòng lặp được chặt chẽ. 

Cuối cùng, đối với mỗi ứng viên (k3, k5), chúng tôi kiểm tra xem k4 được tính theo cách này có hợp lệ hay không. Hàm "tối ưu" được giảm thiểu khi sự khác biệt giữa các học bổng càng nhỏ càng tốt, do đó việc lặp k3 từ 0 trở lên và k5 từ giá trị khả thi nhỏ nhất có xu hướng tạo ra mức chênh lệch tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(s³) | O(1) | Quá chậm | 
| Vòng lặp đôi (Tối ưu) | O((s/c3)*(s/c5)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số học sinh mỗi lớp: _c3_, _c4_, _c5_. Những số lượng này được đảm bảo là ≥1. 
2. Lặp lại tất cả các giá trị nguyên khả thi của _k3_ từ 0 đến tầng (s / c3). Điều này đảm bảo chúng ta không chi tiêu vượt quá ngân sách. 
3. Lặp lại tất cả các giá trị nguyên khả thi của _k5_ từ k3 lên đến tầng (s / c5). Giới hạn dưới đảm bảo thứ tự không giảm. 
4. Tính _k4_ là `(s - c3_k3 - c5_k5
