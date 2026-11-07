---
title: "CF 1381B - Hủy hợp nhất"
description: "Chúng ta được cho một hoán vị có kích thước $2n$. Nhiệm vụ là quyết định xem liệu chúng ta có thể chia các số thành hai chuỗi rời nhau $a$ và $b$, mỗi chuỗi có độ dài $n$, sao cho nếu chúng ta liên tục mô phỏng một quá trình “hợp nhất hai con trỏ” cụ thể bắt đầu từ phần đầu của $a$ và $b$, chúng ta…"
date: "2026-06-16T13:47:59+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1381
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 658 (Div. 1)"
rating: 1800
weight: 1381
solve_time_s: 88
verified: false
draft: false
---

[CF 1381B - Hủy hợp nhất](https://codeforces.com/problemset/problem/1381/B) 

**Đánh giá:** 1800 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$2n$. Nhiệm vụ là quyết định xem chúng ta có thể chia các số thành hai chuỗi rời nhau hay không$a$Và$b$, mỗi chiều dài$n$, sao cho nếu chúng ta mô phỏng lặp đi lặp lại một quá trình "hợp nhất hai con trỏ" cụ thể bắt đầu từ phần đầu của$a$Và$b$, ta thu được chính xác hoán vị đã cho. 

Quá trình hợp nhất được xác định một lần$a$Và$b$được cố định: ở mỗi bước chúng ta so sánh các phần tử phía trước hiện tại và luôn lấy phần tử nhỏ hơn. Điều này có nghĩa là trình tự cuối cùng được xác định hoàn toàn bởi thứ tự tương đối của các phần tử khi chúng xuất hiện trong$a$Và$b$, không phải bằng bất kỳ sự tự do nào trong việc lựa chọn. 

Vì vậy, câu hỏi thực sự không phải là về việc mô phỏng sự hợp nhất về phía trước, mà là về việc liệu chúng ta có thể thiết kế ngược hai chuỗi hợp lệ mà việc xen kẽ theo phép so sánh tham lam này tạo ra hoán vị mục tiêu hay không. 

Các ràng buộc là nhỏ theo một cách rất quan trọng. Mỗi bài kiểm tra có$n \le 2000$và tổng số tiền$n$nhiều nhất cũng là$2000$. Điều này ngay lập tức loại trừ bất cứ điều gì tồi tệ hơn đại khái$O(n^2)$cho mỗi thử nghiệm và gợi ý rõ ràng rằng chúng ta nên suy nghĩ theo khía cạnh lập trình động theo các vị trí hoặc theo các phần tách, thay vì các cấu trúc giai thừa hoặc hàm mũ. 

Một cách tiếp cận đơn giản là thử mọi cách để gán từng phần tử cho một trong hai$a$hoặc$b$, nhưng đó là$2^{2n}$, điều đó hoàn toàn không thể thực hiện được. Ngay cả việc cố gắng mô phỏng tính hợp lệ sau khi chuyển nhượng vẫn mang tính cấp số nhân. 

Một trường hợp thất bại tinh vi hơn xuất hiện khi chúng ta thử phép gán tham lam dựa trên so sánh cục bộ. Vì quy tắc hợp nhất phụ thuộc vào _front_ của mỗi mảng, vốn phát triển linh hoạt, nên bất kỳ phép gán tham lam nào bỏ qua các ràng buộc trong tương lai sẽ phá vỡ các hoán vị trong đó các phần tử nhỏ ban đầu “ép buộc” cấu trúc tương lai không chính xác. Ví dụ: một số nhỏ được đặt quá sớm trong mảng sai có thể chặn các so sánh cần thiết sau này, khiến cho việc phân chia hợp lệ là không thể ngay cả khi tiền tố trông nhất quán. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là gán từng phần tử của hoán vị cho một trong hai$a$hoặc$b$, đảm bảo cả hai nhận được chính xác$n$các phần tử. Đối với mỗi nhiệm vụ, chúng tôi mô phỏng xem quá trình hợp nhất có tái tạo hoán vị hay không. Điều này đòi hỏi phải duy trì hai hàng đợi và liên tục so sánh mặt trước của chúng, chi phí$O(n)$mỗi mô phỏng. Vì có$\binom{2n}{n}$các bài tập, tổng độ phức tạp là theo cấp số nhân và tăng vượt xa mọi giới hạn. 

Quan sát quan trọng là quá trình hợp nhất không phụ thuộc vào nhãn tuyệt đối mà phụ thuộc vào các ràng buộc thứ tự tương đối do cấu trúc tiền tố gây ra. Tại bất kỳ thời điểm nào trong quá trình tái thiết, điều quan trọng là phần tử nào vẫn là “mặt trận hoạt động” của hai mảng. Điều này cho thấy rằng thay vì xây dựng$a$Và$b$trực tiếp, chúng ta có thể xử lý hoán vị và quyết định cách gán từng phần tử tiền tố trong khi vẫn duy trì khả năng
