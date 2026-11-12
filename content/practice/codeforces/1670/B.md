---
title: "CF 1670B - Cuộc chiến ký túc xá"
description: "Chúng ta được cung cấp một chuỗi đại diện cho mật khẩu và một tập hợp các ký tự đặc biệt. Một chương trình chuyển đổi có thể được áp dụng nhiều lần cho chuỗi."
date: "2026-06-10T01:41:38+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1670
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 788 (Div. 2)"
rating: 1100
weight: 1670
solve_time_s: 45
verified: false
draft: false
---

[CF 1670B - Cuộc chiến ký túc xá](https://codeforces.com/problemset/problem/1670/B) 

**Đánh giá:** 1100 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho mật khẩu và một tập hợp các ký tự đặc biệt. Một chương trình chuyển đổi có thể được áp dụng nhiều lần cho chuỗi. Mỗi lần chạy sẽ xem xét mọi vị trí và đánh dấu những vị trí có hàng xóm bên phải ngay lập tức là một ký tự đặc biệt, sau đó xóa tất cả các vị trí được đánh dấu cùng một lúc. 

Quy tắc xóa hoàn toàn mang tính cục bộ: một ký tự sẽ biến mất nếu ký tự ngay sau nó thuộc tập đặc biệt. Sau khi xóa, chuỗi co lại và quy tắc tương tự có thể được áp dụng lại cho chuỗi mới. Quá trình dừng lại khi việc áp dụng quy tắc không xóa gì vì điều đó gây ra lỗi. 

Nhiệm vụ là tính toán số lần xóa thành công có thể được thực hiện trước khi đạt đến trạng thái không thể xóa được. 

Các ràng buộc cho phép tối đa 100.000 ký tự cho mỗi trường hợp thử nghiệm và tổng cộng lên tới 200.000 ký tự. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào quét và xây dựng lại chuỗi trong mỗi vòng, vì trong trường hợp xấu nhất, một trường hợp thử nghiệm có thể chuyển thành hành vi bậc hai bằng cách liên tục thu nhỏ từng ký tự một. 

Một điểm tinh tế quan trọng là việc xóa diễn ra đồng thời. Một cách tiếp cận đơn giản là loại bỏ từng ký tự một hoặc cố gắng thay đổi chuỗi trong khi lặp lại có thể dễ dàng xử lý sai các chỉ số dịch chuyển. Ví dụ: trong một chuỗi như`"ababa"`với`'b'`đặc biệt, vòng đầu tiên loại bỏ cả hai`'a'`vị trí trước`'b'`và sau khi tính toán lại, cấu trúc sẽ thay đổi theo cách phụ thuộc vào tất cả các thao tác xóa xảy ra cùng một lúc. 

Một cạm bẫy khác là cho rằng chỉ cần vượt qua hoặc loại bỏ tham lam là đủ. Mỗi vòng phụ thuộc vào cấu trúc kề được cập nhật, do đó hiệu ứng lặp lại, nhưng số lần lặp là số lượng chúng ta phải tính toán một cách hiệu quả. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ quét chuỗi liên tục, đánh dấu các vị trí có thể xóa, tạo chuỗi mới và lặp lại. Mỗi lần quét là O(n) và trong trường hợp xấu nhất, chúng tôi chỉ xóa một ký tự cho mỗi lần lặp, dẫn đến thời gian là O(n2). Với tổng số ký tự là 2×10⁵, tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta không thực sự cần mô phỏng mọi chuỗi trung gian. Mỗi ký tự “chờ” một cách hiệu quả cho đến khi một ký tự đặc biệt xuất hiện ở bên phải của nó trong cấu trúc còn sót lại hiện tại. Khi một ký tự bị loại bỏ, nó có thể làm lộ ra các mối quan hệ lân cận mới, nhưng chúng ta có thể nghĩ ngược lại: mọi ký tự đều tồn tại cho đến khi bị chặn bởi một ký tự đặc biệt vẫn ở bên phải của nó ở giai đoạn đó. 

Một góc nhìn hữu ích hơn là xử lý từ phải sang trái trong khi theo dõi mức độ lan truyền của một làn sóng xóa. Quá trình này hoạt động giống như việc loại bỏ lặp đi lặp lại các ký tự nằm ngay trước bất kỳ ký tự đặc biệt nào trong hậu tố hoạt động hiện tại. Mỗi vòng sẽ đẩy ranh giới ảnh hưởng sang trái bằng cách bỏ qua các khối cho đến khi đạt đến vị trí không còn ký tự đặc biệt nào ở bên phải. 

Điều này làm giảm vấn đề liên tục đếm số lượng ký tự có thể bị xóa trước khi đạt đến tiền tố ổn định trong đó không có ký tự đặc biệt nào tồn tại ở bên phải của bất kỳ vị trí có thể xóa còn lại nào. Số lần lặp chính xác là bao nhiêu ti
