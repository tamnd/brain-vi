---
title: "CF 105017G - Trò chơi bài"
description: "Hai người chơi bắt đầu với hai bộ số có kích thước bằng nhau. Những con số này không được so sánh trực tiếp; thay vào đó, chúng dần dần bị “phân hủy” trong quá trình chơi game."
date: "2026-06-28T02:09:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105017
codeforces_index: "G"
codeforces_contest_name: "Winter Cup 4.0 Online Mirror Contest"
rating: 0
weight: 105017
solve_time_s: 52
verified: false
draft: false
---

[CF 105017G - Trò chơi bài](https://codeforces.com/problemset/problem/105017/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi bắt đầu với hai bộ số có kích thước bằng nhau. Những con số này không được so sánh trực tiếp; thay vào đó, chúng dần dần bị “phân hủy” trong quá trình chơi game. Trong mỗi lượt, người chơi hiện tại chọn một số nguyên tố không vượt quá giới hạn L cố định, sau đó buộc đối thủ lấy một trong các số chia hết cho số nguyên tố đó và chia cho số nguyên tố đó. Nếu đối thủ không có số chia hết cho số nguyên tố đã chọn thì người chơi hiện tại sẽ thắng ngay lập tức. 

Chi tiết quan trọng là mỗi nước đi sẽ loại bỏ chính xác một lần xuất hiện của thừa số nguyên tố (không nhất thiết phải là toàn bộ hệ số nguyên tố, chỉ một bản sao của số nguyên tố đã chọn từ một số nào đó ở phía đối thủ). Người chơi thay phiên nhau bắt đầu với Rami và cả hai đều chơi hoàn hảo. 

Đầu vào mô tả hai tập hợp số nguyên, một tập hợp cho mỗi người chơi. Nhiệm vụ là xác định người chơi nào cuối cùng sẽ thắng trong lối chơi tối ưu. 

Các ràng buộc cho phép tối đa 100000 số, mỗi số tối đa 1e6. Điều này loại trừ bất kỳ cách tiếp cận nào liên tục phân tích các số một cách ngây thơ theo O(sqrt(x)) cho mỗi giá trị mà không cần xử lý trước, vì điều đó sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất. Thay vào đó, mỗi số phải được phân tích thành thừa số một cách hiệu quả, lý tưởng nhất là theo thời gian khấu hao logarit hoặc gần như không đổi bằng phương pháp dựa trên sàng. 

Một điểm tinh tế là trò chơi không quan trọng việc chọn số nguyên tố nào, bởi vì một khi số nguyên tố được chọn, yêu cầu duy nhất là đối thủ phải có ít nhất một lần xuất hiện của số nguyên tố đó. Danh tính của số nguyên tố được chọn chỉ xác định mã thông báo nào được sử dụng chứ không phải cấu trúc của cây trò chơi. 

Một trường hợp khác phát sinh khi cả hai người chơi đều có tổng “nội dung chính có thể tháo rời” giống hệt nhau. Trong trường hợp đó, việc luân phiên các lượt có nghĩa là người chơi thứ hai, Yessine, thực hiện nước đi cuối cùng và buộc phải thắng. 

## Phương pháp tiếp cận 

Thoạt nhìn, người ta có thể thử mô phỏng trò chơi. Mỗi lượt sẽ yêu cầu kiểm tra tất cả các số nguyên tố đến L và tìm một thẻ chia hết, sau đó cập nhật nó. Ngay cả khi chúng tôi duy trì số lượng hệ số trên mỗi thẻ, hệ số phân nhánh của việc chọn số nguyên tố khiến cho việc mô phỏng trở nên không cần thiết, bởi vì lựa chọn không ảnh hưởng đến cấu trúc tương lai ngoài việc tiêu thụ một đơn vị tiến bộ từ đối thủ. 

Quan sát quan trọng là mỗi số đóng góp độc lập vào tổng số “nước đi có sẵn” cho đối thủ của chủ nhân nó. Mỗi khi một số chứa thừa số nguyên tố p ≤ L, hệ số đó đại diện cho một đơn vị có thể được tiêu thụ trong trò chơi. Cho dù số nguyên tố được chọn như thế nào, việc tiêu thụ p chỉ đơn giản là làm giảm số mũ của p ở một số nào đó đi một. 

Điều này làm giảm toàn bộ trò chơi thành một quá trình đếm đơn giản. Mỗi người chơi có tổng số thừa số nguyên tố có thể tháo rời trong bộ đa của họ. Gọi R là tổng số các thừa số như vậy trong các số của Rami và Y là tổng trong các số của Yessine. Mỗi nước đi sẽ loại bỏ chính xác một đơn vị trong tổng số của đối thủ. Rami xóa khỏi Y, Yessine xóa khỏi R. 

Trò chơi luân phiên cho đến khi một bên không thể di chuyển. Điều này trở thành một quá trình cạn kiệt xen kẽ đơn giản và người chiến thắng chỉ phụ thuộc vào việc bên nào hết thiết bị có thể tháo rời trước. Kể từ khi Rami bắt đầu, anh ấy thắng chính xác khi Y lớn hơn R; nếu không thì Yessine ít nhất sẽ sống sót được lâu và giành chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng các bước đi có cập nhật hệ số | O(N √A) hoặc tệ hơn | O(N) | Quá chậm | 
| Sàng hệ số đếm + mô hình giảm tham lam | O(N log A) | O (Tối đa) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng số thành hệ số nguyên tố của nó và chỉ giữ các số nguyên tố đến L.

1. Xây dựng sàng có hệ số nguyên tố nhỏ nhất lên tới 1e6. Điều này cho phép chúng ta phân tích bất kỳ số nào theo thời gian logarit bằng cách chia liên tục cho thừa số nguyên tố nhỏ nhất của nó. 
2. Khởi tạo hai bộ đếm, R và Y, biểu thị số lần xuất hiện số nguyên tố có thể di chuyển được trong mảng của Rami và Yessine. 
3. Với mỗi số trong danh sách Rami, hãy liên tục trích xuất các thừa số nguyên tố của nó bằng cách sử dụng sàng. Với mỗi thừa số nguyên tố p, nếu p ≤ L thì tăng R thêm 1. 
4. Lặp lại quy trình tương tự cho danh sách của Yessine, tích lũy
