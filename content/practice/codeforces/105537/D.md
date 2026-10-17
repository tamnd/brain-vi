---
title: "CF 105537D - Tập lệnh bị lỗi"
description: "Chúng ta được cung cấp một mạng lưới các máy chủ hình tròn, mỗi máy chủ có tải không âm. Hệ thống cung cấp một hoạt động được cho là để giảm tải cục bộ, nhưng nó hơi bị hỏng: khi áp dụng cho một máy chủ, nó sẽ giảm máy chủ đó đi hai đơn vị và cũng giảm hàng xóm trước đó của nó…"
date: "2026-06-27T00:58:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105537
codeforces_index: "D"
codeforces_contest_name: "2024-2025 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 105537
solve_time_s: 25
verified: false
draft: false
---

[CF 105537D - Tập lệnh bị lỗi](https://codeforces.com/problemset/problem/105537/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng lưới các máy chủ hình tròn, mỗi máy chủ có tải không âm. Hệ thống này cung cấp một thao tác được cho là để giảm tải cục bộ, nhưng nó hơi bị hỏng: khi áp dụng cho một máy chủ, nó sẽ giảm máy chủ đó xuống hai đơn vị và cũng giảm hàng xóm trước đó trong vòng một đơn vị. Chúng tôi có thể áp dụng thao tác này nhiều lần trên bất kỳ máy chủ nào và chúng tôi được phép sử dụng quá mức ngay cả khi tải trở nên nhỏ, cho phép các giá trị giảm xuống 0 một cách hiệu quả nhưng không thấp hơn. 

Mục tiêu không phải là đạt được cấu hình tùy ý mà là làm cho tất cả các máy chủ kết thúc với cùng một mức tải, đồng thời tối đa hóa giá trị cuối cùng chung đó. Vì vậy, chúng tôi không giảm thiểu các hoạt động mà thay vào đó yêu cầu mức thống nhất có thể đạt được lớn nhất có thể được thực thi thông qua các mức giảm đi kèm này. 

Các ràng buộc cho phép tổng số máy chủ lên tới 2×10^5 trong các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai trên n đều là không thể ngay lập tức, và ngay cả n log n cũng cần được thiết kế cẩn thận vì tổng kích thước đầu vào lớn qua các thử nghiệm. 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động có thể có, theo dõi cách mỗi hoạt động truyền tải mức giảm trong chu kỳ. Điều đó không thành công vì mỗi thao tác ảnh hưởng đến hai vị trí theo cách có cấu trúc, do đó số lượng trạng thái tăng theo cấp số nhân. Ngay cả việc cố gắng giảm tham lam đến giá trị mục tiêu cũng sẽ yêu cầu kiểm tra tính khả thi trên toàn cầu nhiều lần, điều này sẽ đẩy độ phức tạp lên tới O(n^2) hoặc tệ hơn. 

Một vấn đề tế nhị hơn xuất hiện khi lý luận cục bộ. Ví dụ: giả sử chúng ta cố gắng quyết định tính khả thi của giá trị đích x bằng cách kiểm tra độc lập xem mỗi máy chủ có thể giảm xuống x bằng các hoạt động có sẵn hay không. Điều này bị phá vỡ vì các phép toán không mang tính cục bộ: áp dụng tập lệnh tại i đồng thời thay đổi i và i−1, do đó các quyết định ở các vị trí liền kề được kết hợp. Một ví dụ phản biện đơn giản là một vòng nhỏ giống như 3 máy chủ có tải [3, 0, 0], trong đó mức giảm cục bộ tham lam trên mỗi nút gợi ý tính khả thi của x = 1 một cách độc lập, nhưng bất kỳ chuỗi hoạt động thực tế nào cũng truyền tải mức giảm theo cách buộc một nút giảm quá xa. 

## Phương pháp tiếp cận 

Mấu chốt của vấn đề này là ngừng suy nghĩ về các hoạt động riêng lẻ mà thay vào đó hãy mô hình hóa tác động tích lũy của chúng. 

Mỗi lần chúng tôi áp dụng tập lệnh ở vị trí i, chúng tôi giảm một cặp tọa độ: a[i] giảm 2 và a[i−1] giảm 1. Nếu chúng tôi đặt x_i biểu thị số lần chúng tôi áp dụng tập lệnh tại i, thì tổng mức giảm ở vị trí i được xác định bởi x_i và x_{i+1} vì i bị ảnh hưởng bởi các hoạt động của chính nó và các hoạt động của nút tiếp theo. Cụ thể, máy chủ i mất 2x_i từ các hoạt động của chính nó và 1x_{i+1} từ máy chủ tiếp theo trong vòng. 

Điều này biến bài toán thành việc tìm các số nguyên không âm x_i sao cho sau khi áp dụng các ràng buộc tuyến tính này, tất cả các giá trị cuối cùng đều trở thành bằng nhau. Thay vì mô phỏng các phép toán, chúng tôi đang giải một hệ phương trình tuyến tính với các ràng buộc bất đẳng thức. 

Quan sát quan trọng là khi chúng ta cố định giá trị cuối cùng ứng cử viên k thì các giá trị x_i không phải là tùy ý. Nếu chúng ta lại
