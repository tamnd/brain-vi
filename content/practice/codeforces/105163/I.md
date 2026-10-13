---
title: "CF 105163I - Đây là một bài toán dễ"
description: "Nhiệm vụ tập trung vào việc kiểm tra biểu diễn nhị phân của một số nguyên và suy luận về các bit riêng lẻ."
date: "2026-06-27T10:54:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105163
codeforces_index: "I"
codeforces_contest_name: "The 19th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 105163
solve_time_s: 32
verified: false
draft: false
---

[CF 105163I - Đây là một vấn đề dễ](https://codeforces.com/problemset/problem/105163/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ tập trung vào việc kiểm tra biểu diễn nhị phân của một số nguyên và suy luận về các bit riêng lẻ. Thay vì coi số đó là một giá trị duy nhất, chúng tôi diễn giải nó như một chuỗi các vị trí nhị phân độc lập, trong đó mỗi vị trí đóng góp một số 0 hoặc một số tùy thuộc vào việc bit đó có được đặt hay không. 

Mục tiêu có thể được hiểu là quét qua tất cả các vị trí bit có liên quan của số nguyên nhất định và kiểm tra từng vị trí xem bit đó có hoạt động hay không. Việc kiểm tra được thực hiện bằng cách sử dụng bitmask có dạng`(1 << i)`, cách ly bit thứ i và bit AND với số. Nếu kết quả bằng 0 thì vị trí đó chứa bit 0. 

Từ góc độ ràng buộc, cách tiếp cận dự định gợi ý rõ ràng rằng kích thước đầu vào trên mỗi thử nghiệm đủ nhỏ để quét giới hạn trên cố định trên các bit, thường là 30 hoặc 32 lần lặp cho mỗi số. Điều này ngay lập tức loại trừ bất kỳ điều gì phụ thuộc vào đệ quy trên mỗi bit hoặc khám phá tổ hợp trên nhiều số, vì những số đó sẽ là chi phí không cần thiết. Vấn đề được thiết kế sao cho chỉ cần quét O(số bit) là đủ. 

Trường hợp đặc biệt phổ biến trong các vấn đề như thế này xuất phát từ số âm nếu có liên quan đến số nguyên có dấu. Ví dụ, nếu`x = -1`trong biểu diễn có chiều rộng cố định, tất cả các bit được đặt, do đó không tồn tại bit 0. Việc triển khai bất cẩn chỉ giả định đầu vào không âm hoặc bỏ qua phần mở rộng dấu có thể tạo ra kết quả không nhất quán. Một trường hợp cạnh khác xuất hiện khi`x = 0`, trong đó mọi bit đều bằng 0 và câu trả lời phụ thuộc vào việc việc triển khai xem xét độ rộng bit cố định hay chỉ các bit quan trọng. Nếu chúng tôi chỉ quét cho đến bit được đặt cao nhất, chúng tôi sẽ trả về không chính xác các bit 0, trong khi cách diễn giải có chiều rộng cố định sẽ trả về tất cả các vị trí là 0. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là kiểm tra rõ ràng từng vị trí bit một cách độc lập. Đối với mỗi chỉ số`i`, chúng tôi xây dựng một mặt nạ`1 << i`và kiểm tra xem`(x & (1 << i))`bằng không. Điều này trực tiếp tuân theo định nghĩa về biểu diễn nhị phân và đúng vì bit AND tách biệt sự đóng góp của vị trí đơn lẻ đó. 

Cách tiếp cận này đã tối ưu về cấu trúc vì số lượng vị trí bit không đổi đối với giá trị đầu vào. Ngay cả đối với số nguyên 32 bit hoặc 64 bit, tổng công việc bị giới hạn bởi một hằng số nhỏ, do đó không thể cải thiện tiệm cận có ý nghĩa. Sự kém hiệu quả duy nhất đến từ công việc không cần thiết ngoài quá trình quét bit cố định, điều này không có ở đây. 

Quan sát quan trọng là mỗi
