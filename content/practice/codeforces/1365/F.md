---
title: "CF 1365F - Hoán đổi lần nữa"
description: "Chúng ta được cung cấp hai mảng có độ dài bằng nhau và một quy tắc chuyển đổi rất cụ thể. Bắt đầu từ mảng đầu tiên, chúng ta được phép chọn liên tục một kích thước phân chia và hoán đổi hai khối có độ dài bằng nhau: tiền tố của kích thước đó và hậu tố có cùng kích thước."
date: "2026-06-16T11:50:44+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1365
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 648 (Div. 2)"
rating: 2100
weight: 1365
solve_time_s: 148
verified: false
draft: false
---

[CF 1365F - Hoán đổi lần nữa](https://codeforces.com/problemset/problem/1365/F) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, triển khai, sắp xếp 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai mảng có độ dài bằng nhau và một quy tắc chuyển đổi rất cụ thể. Bắt đầu từ mảng đầu tiên, chúng ta được phép chọn liên tục một kích thước phân chia và hoán đổi hai khối có độ dài bằng nhau: tiền tố của kích thước đó và hậu tố có cùng kích thước. Mọi thứ ở giữa, nếu nó tồn tại, vẫn cố định. Việc lặp lại thao tác này nhiều lần sẽ tạo ra một tập hợp các hoán vị có thể truy cập được của mảng ban đầu. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là quyết định xem có thể lấy được mảng thứ hai từ mảng đầu tiên bằng cách sử dụng các thao tác này hay không. 

Hạn chế chính là kích thước hoạt động tối đa bằng một nửa chiều dài mảng. Hạn chế đó ngăn chặn việc xoay tùy ý hoặc hoán đổi các phần tử tùy ý; chúng ta không ở trong thế giới của sự tạo ra hoán vị đầy đủ. Thay vào đó, cấu trúc của các trạng thái có thể tiếp cận bị hạn chế rất nhiều bởi cách tương tác giữa các hoán đổi khối đối xứng này. 

Kích thước đầu vào nhỏ, lên tới 500 cho mỗi lần kiểm tra và tổng cộng 500 lần kiểm tra. Điều này cho phép mọi giải pháp lên tới khoảng O(n²) hoặc O(n² log n) cho mỗi trường hợp thử nghiệm, nhưng loại trừ mọi hoạt động khám phá theo cấp số nhân về các trạng thái có thể tiếp cận. 

Một cách tiếp cận ngây thơ có thể cố gắng mô phỏng tất cả các giao dịch hoán đổi có thể có hoặc coi đây là tìm kiếm biểu đồ hoán vị. Điều đó không thành công vì số lượng cấu hình có thể truy cập tăng cực kỳ nhanh chóng ngay cả đối với n vừa phải. 

Một chế độ lỗi tinh vi hơn sẽ xuất hiện nếu chúng ta chỉ giả định rằng các mảng phải có cùng nhiều giá trị. Điều kiện đó là cần nhưng chưa đủ. Ví dụ: các mảng như`[1, 2, 3, 4]`Và`[1, 3, 2, 4]`chia sẻ các phần tử giống nhau nhưng không phải lúc nào cũng có thể được chuyển đổi tùy thuộc vào các ràng buộc về tính chẵn lẻ về cấu trúc do hoạt động gây ra. 

Khó khăn chính là phép toán bảo tồn một cấu trúc bất biến ẩn không rõ ràng từ lý luận cục bộ. 

## Phương pháp tiếp cận 

Quan điểm bạo lực sẽ coi mỗi thao tác hợp lệ là một cạnh trong biểu đồ có các nút là tất cả các hoán vị của mảng. Mỗi thao tác chọn một giá trị k và tạo ra một hoán vị mới. Từ bất kỳ nút nào, có tối đa n/2 cạnh đi ra và đồ thị có n! nút. Ngay cả khi cắt tỉa, điều này hoàn toàn không thể thực hiện được; việc khám phá ngay cả một phần rất nhỏ của không gian này cũng bùng nổ theo cách kết hợp. 

Thông tin chi tiết quan trọng là mặc dù thao tác có vẻ như có thể sắp xếp lại các phần tử nhưng nó vẫn giữ được tính bất biến về cấu trúc rất mạnh: mảng hoạt động giống như một cấu trúc hình tròn trong đó chỉ cho phép một số “đối xứng gấp” nhất định. Đặc biệt, điều quan trọng là cách các phần tử được nhóm theo vị trí tương đương và cách các nhóm này có thể được xen kẽ thông qua việc hoán đổi tiền tố-hậu tố đối xứng lặp đi lặp lại. 

Nếu chúng tôi theo dõi quá trình này cẩn thận hơn, chúng tôi có thể chỉ ra rằng các cấu hình duy nhất có thể tiếp cận được là những cấu hình có thể được tạo bằng cách liên tục thực hiện phân chia phân đoạn và đảo ngược thứ tự của hai nửa bằng nhau. Cái này
