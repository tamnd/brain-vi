---
title: "CF 1051G - Phân biệt"
description: "Chúng ta được cho một chuỗi các điểm tăng dần trên dòng số nguyên. Mỗi điểm có một giá trị vị trí và trọng số. Khi tiết lộ nhiều điểm hơn từng điểm một, chúng tôi muốn duy trì cấu hình trong đó tất cả các vị trí đều khác biệt nhưng chúng tôi được phép di chuyển các điểm sang trái hoặc sang phải bằng cách sử dụng rất…"
date: "2026-06-15T10:59:54+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1051
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 51 (Rated for Div. 2)"
rating: 2900
weight: 1051
solve_time_s: 219
verified: false
draft: false
---

[CF 1051G - Phân biệt](https://codeforces.com/problemset/problem/1051/G) 

**Xếp hạng:** 2900 
**Tags:** cấu trúc dữ liệu, dsu, tham lam 
**Thời gian giải:** 3 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các điểm tăng dần trên dòng số nguyên. Mỗi điểm có một giá trị vị trí và trọng số. Khi tiết lộ nhiều điểm hơn từng điểm một, chúng tôi muốn duy trì một cấu hình trong đó tất cả các vị trí đều khác biệt, nhưng chúng tôi được phép di chuyển các điểm sang trái hoặc sang phải bằng cách sử dụng các quy tắc rất cụ thể tùy thuộc vào việc có tồn tại va chạm hay không. 

Việc di chuyển dịch chuyển một điểm sang phải chỉ được phép nếu điểm đó hiện chia sẻ vị trí của nó với ít nhất một điểm khác. Động thái này làm tăng vị thế và tiêu tốn sức nặng của nó. Việc di chuyển dịch chuyển một điểm sang trái chỉ được phép nếu điểm đó nằm ngay bên phải của một số điểm khác và động thái này làm giảm vị trí với chi phí âm, mang lại phần thưởng hiệu quả tỷ lệ thuận với trọng lượng của nó. 

Mục tiêu của mỗi tiền tố là đạt đến trạng thái không có hai điểm nào có cùng vị trí, đồng thời giảm thiểu tổng chi phí tích lũy của tất cả các bước di chuyển. 

Giải thích cấu trúc quan trọng là các va chạm tại tọa độ hoạt động giống như một "nhóm tài nguyên". Nếu nhiều điểm có cùng giá trị, chúng ta có thể phân phối lại chúng sang trái hoặc phải và chi phí cho việc này chỉ phụ thuộc vào điểm nào được di chuyển chứ không phụ thuộc vào vị trí cuối cùng của nó. Bởi vì các bước di chuyển bên trái yêu cầu hàng xóm hỗ trợ một đơn vị bên dưới và các bước di chuyển bên phải yêu cầu các bản sao, hệ thống hoạt động giống như một luồng trên một dòng số nguyên trong đó các trọng số kiểm soát các khuyến khích chuyển giao. 

Kích thước đầu vào đạt tới hai trăm nghìn phần tử, loại trừ bất kỳ giải pháp nào liên tục mô phỏng chuyển động hoặc duy trì các tập hợp va chạm rõ ràng trên mỗi tiền tố với sửa chữa bậc hai. Bất kỳ cách tiếp cận nào cố gắng giải quyết xung đột một cách tham lam trên mỗi tiền tố trong quá trình quét tuyến tính trên mỗi phần tử sẽ chuyển thành hành vi bậc hai và thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi tồn tại nhiều vị trí giống hệt nhau và trọng số khác nhau đáng kể. Ví dụ: nếu một số điểm chia sẻ giá trị 5 nhưng một điểm có trọng số rất lớn và các điểm khác có trọng số nhỏ, thì việc di chuyển sai phần tử trước có thể thay đổi đáng kể tổng chi phí vì chi phí tăng dương và giảm đi sẽ tạo ra đóng góp âm. Một chiến lược tham lam ngây thơ giải quyết các bản sao một cách tùy ý sẽ thất bại ở đây, vì hành vi tối ưu phụ thuộc vào thứ tự toàn cầu theo trọng số chứ không phải theo thứ tự đến. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là các sự trùng lặp ở cùng một vị trí phải được giải quyết, nhưng mọi động thái giải quyết đều mang lại chi phí không đối xứng gắn liền với
