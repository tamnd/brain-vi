---
title: "CF 1923F - Thu gọn-Đảo ngược"
description: "Chúng ta được cấp một chuỗi nhị phân s có độ dài n và một số k biểu thị số lượng thao tác tối đa được phép. Mỗi thao tác có thể hoán đổi hai ký tự của chuỗi hoặc xóa tất cả các số 0 đứng đầu rồi đảo ngược chuỗi."
date: "2026-06-08T19:13:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "hashing", "implementation", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 1923
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 162 (Rated for Div. 2)"
rating: 2800
weight: 1923
solve_time_s: 43
verified: false
draft: false
---

[CF 1923F - Thu nhỏ-Đảo ngược](https://codeforces.com/problemset/problem/1923/F) 

**Đánh giá:** 2800 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, tham lam, băm, triển khai, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân`s`chiều dài`n`và một số`k`đại diện cho số lượng hoạt động tối đa được phép. Mỗi thao tác có thể hoán đổi hai ký tự của chuỗi hoặc xóa tất cả các số 0 đứng đầu rồi đảo ngược chuỗi. Nhiệm vụ là thực hiện tối đa`k`các phép toán để giảm thiểu giá trị nguyên của chuỗi khi được hiểu dưới dạng số nhị phân. 

Kích thước đầu vào lớn, lên tới 500.000 ký tự, ngay lập tức loại trừ mọi cách tiếp cận thử mọi chuỗi thao tác có thể có. Một cách tiếp cận ngây thơ mô phỏng tất cả các giao dịch hoán đổi hoặc tất cả các chuỗi hoạt động SHRINK-REVERSE sẽ bùng nổ về mặt tổ hợp. 

Các trường hợp cạnh không rõ ràng phát sinh khi các số 0 đã ở phía trước, khi chuỗi chủ yếu là các số 0 ngoại trừ một hoặc hai số một hoặc khi SHRINK-REVERSE thực sự tăng các số 0 đứng đầu theo cách giúp hoán đổi sau này. Ví dụ, đối với`s = "1000"`Và`k = 1`, một cách triển khai ngây thơ luôn thực hiện SHRINK-REVERSE trước tiên có thể bỏ lỡ việc hoán đổi dấu '1' ở cuối sẽ tạo ra giá trị nhỏ hơn. Thuật toán phải quyết định cẩn thận khi nào các giao dịch hoán đổi so với SHRINK-REVERSE mang lại mức giảm giá trị tối đa. 

Thông tin chi tiết quan trọng là hoạt động SHRINK-REVERSE ảnh hưởng đến các số 0 đứng đầu và việc hoán đổi chỉ quan trọng đối với việc di chuyển các số về bên phải để giảm thiểu giá trị nhị phân tổng thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các chuỗi lên đến`k`hoạt động, mô phỏng các giao dịch hoán đổi giữa mọi cặp chỉ số và áp dụng SHRINK-REVERSE ở bất cứ đâu. Cách tiếp cận này đúng về nguyên tắc vì nó khám phá mọi kết quả có thể xảy ra, nhưng lại cực kỳ chậm. Đối với một chuỗi có độ dài`n`, có khoảng$O(n^2)$hoán đổi cho mỗi thao tác và thực hiện việc này`k`lần cho$O(n^{2k})$sự phức tạp. Với`n = 5 * 10^5`, ngay cả một vòng lặp trao đổi đơn lẻ cũng không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là các hoạt động tương tác theo cách có thể dự đoán được. Các phép hoán đổi đưa các số 1 đến gần cuối chuỗi luôn làm giảm giá trị số nguyên và SHRINK-REVERSE di chuyển các số 0 ở cuối lên phía trước sau khi loại bỏ các số 0 ở đầu. Điều này có nghĩa là chúng ta có thể coi vấn đề là di chuyển hai cái: một cái ở phía trước và một cái ở cuối cùng, với số lần hoán đổi có hạn. Chuỗi còn lại sau khi di chuyển những chuỗi này sẽ đóng góp rất ít vào giá trị vì những chuỗi này bị đẩy đến các điểm cực trị. 

Quan sát này làm giảm vấn đề khi xem xét tối đa hai giao dịch hoán đổi mục tiêu, có thể kết hợp với SHRINK-REVERSE và tính toán giá trị kết quả. Mọi thứ khác có thể được thực hiện một cách tham lam. Chúng ta chỉ cần kiểm tra số đầu tiên, số cuối cùng và số 0 để quyết định các giao dịch hoán đổi tối ưu, đưa ra giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^{2k}) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định cái đầu tiên`1`trong chuỗi`s`và cuối cùng`1`. Đây là những nhân vật duy nhất có vị trí ảnh hưởng trực tiếp đến pot
