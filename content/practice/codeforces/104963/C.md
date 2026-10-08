---
title: "CF 104963C - \u041d\u0430\u043b\u043e\u0433"
description: "Chúng tôi được cấp một tập hợp các căn hộ, mỗi căn có diện tích ban đầu. Daniil có thể giảm diện tích của bất kỳ căn hộ nào bằng cách áp dụng lặp đi lặp lại một phép toán: chọn hệ số $y$, trả $y$ xu và chia diện tích hiện tại cho $y$, nhưng chỉ khi kết quả vẫn là số nguyên."
date: "2026-06-28T18:20:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104963
codeforces_index: "C"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2022. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 104963
solve_time_s: 29
verified: false
draft: false
---

[CF 104963C - \u041d\u0430\u043b\u043e\u0433](https://codeforces.com/problemset/problem/104963/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một tập hợp các căn hộ, mỗi căn có diện tích ban đầu. Daniil có thể giảm diện tích của bất kỳ căn hộ nào bằng cách áp dụng nhiều lần một thao tác: chọn một hệ số$y$, chi trả$y$tiền xu và chia diện tích hiện tại cho$y$, nhưng chỉ khi kết quả vẫn là số nguyên. Thao tác này có thể được áp dụng nhiều lần cho cùng một căn hộ, nghĩa là diện tích cuối cùng của mỗi căn hộ phải là một ước số nào đó thu được thông qua chuỗi phân tích nhân tử của giá trị ban đầu. 

Chi phí không gắn liền với số bước mà gắn với tích của các hệ số phân chia được chọn qua các bước. Nếu một căn hộ có diện tích$a$bị giảm đi thông qua các yếu tố$y_1, y_2, \dots, y_t$, tổng chi phí là$y_1 + y_2 + \dots + y_t$, và diện tích cuối cùng trở thành$a / (y_1 y_2 \cdots y_t)$. Mục tiêu là phân phối tối đa$k$tiền xu trên các căn hộ sao cho diện tích căn hộ cuối cùng tối đa càng nhỏ càng tốt. 

Đầu ra là một số duy nhất: giá trị nhỏ nhất có thể của diện tích căn hộ tối đa sau khi giảm tối ưu. 

Những hạn chế$n \le 10^5$Và$a_i \le 10^6$ngay lập tức loại trừ bất kỳ hệ số động cho mỗi truy vấn hoặc lập trình động cho mỗi giá trị trên phạm vi lớn. Một giải pháp phải tính toán trước thông tin lên đến$10^6$và sau đó xử lý tất cả các căn hộ ở dạng gần tuyến tính hoặc$O(n \log A)$thời gian. Từ$k$có thể lớn như$10^9$, bất kỳ cách tiếp cận nào mô phỏng việc chi tiêu tiền xu từng bước đều không thể thực hiện được. 

Một cách giải thích ngây thơ có thể cố gắng mô phỏng tất cả các chuỗi phân chia cho mỗi căn hộ hoặc giảm đi căn hộ lớn nhất một cách tham lam. Cả hai đều thất bại vì chi phí giảm như nhau phụ thuộc vào sự lựa chọn yếu tố chứ không chỉ phụ thuộc vào giá trị cuối cùng. 

Một trường hợp khó phát hiện khi một căn hộ có diện tích đắc địa. Ví dụ,$a_i = 997$. Cách duy nhất để giảm nó là chia cho 997 với giá 997, cực kỳ đắt. Bất kỳ chiến lược tham lam nào liên tục giảm mức tối đa hiện tại mà không xem xét đến hiệu quả chi phí sẽ lãng phí ngân sách cho các số tổng hợp trước tiên và sau đó mắc kẹt với các số nguyên tố thống trị mức tối đa. 

Một trường hợp khác là khi tất cả các căn hộ đều bằng nhau và tổng hợp, chẳng hạn$a = [12, 12, 12]$. Chiến lược tối ưu có thể giảm mạnh một số căn hộ trong khi vẫn giữ nguyên những căn hộ khác, do đó, việc xử lý thống nhất tất cả các căn hộ sẽ dẫn đến việc sử dụng ngân sách dưới mức tối ưu. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng gán cho mỗi căn hộ một giá trị cuối cùng$b_i$như vậy$b_i$chia rẽ$a_i$, và chi phí để giảm$a_i$ĐẾN$b_i$được tính toán bằng cách tìm kiếm tất cả các chuỗi các phép chia hợp lệ. Đối với mỗi căn hộ, chúng tôi sẽ liệt kê tất cả các ước số có thể tiếp cận và chi phí tối thiểu của chúng, sau đó thử kết hợp giữa các căn hộ để đảm bảo mức tối đa$b_i$được giảm thiểu dưới tổng chi phí$k$. Điều này nhanh chóng bùng nổ vì ngay cả một con số lên tới$10^6$có thể có hàng nghìn chuỗi ước số và kết hợp các lựa chọn trên$10^5$căn hộ làm cho nó theo cấp số nhân. 

Quan sát quan trọng là chúng ta không bao giờ cần trình tự hoạt động chính xác. Chúng tôi chỉ quan tâm đến cách tốt nhất để giảm mỗi số xuống một giá trị ngưỡng$x$, và liệu có thể xây dựng tối đa tất cả các căn hộ$x$với tổng chi phí$\le k$. Điều này chuyển vấn đề thành một vấn đề quyết định: đối với một$x$, tính chi phí tối thiểu cần thiết để giảm mỗi$a_i$nhiều nhất là$x$, sau đó tính tổng tất cả các căn hộ. Nếu chúng ta có thể trả lời câu hỏi này một cách nhanh chóng, chúng ta có thể tìm kiếm nhị phân khả thi nhỏ nhất$x$. 

Cái nhìn sâu sắc quan trọng thứ hai là làm thế nào để tính toán chi phí tối thiểu để giảm một số lượng$a$xuống tới
