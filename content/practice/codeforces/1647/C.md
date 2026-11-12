---
title: "CF 1647C - Madoka và những trò đùa trẻ con"
description: "Chúng tôi được cung cấp một lưới nhị phân trống ban đầu chứa đầy các số 0 và chúng tôi muốn chuyển đổi nó thành mẫu 0-1 mục tiêu."
date: "2026-06-10T04:06:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1647
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 777 (Div. 2)"
rating: 1300
weight: 1647
solve_time_s: 141
verified: false
draft: false
---

[CF 1647C - Madoka và những trò đùa trẻ con](https://codeforces.com/problemset/problem/1647/C) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới nhị phân trống ban đầu chứa đầy các số 0 và chúng tôi muốn chuyển đổi nó thành mẫu 0-1 mục tiêu. Thao tác duy nhất được phép là chọn một lưới con hình chữ nhật và sơn lại nó theo mẫu bàn cờ, trong đó ô trên cùng bên trái của hình chữ nhật đó luôn được đặt thành 0 và các màu thay thế theo khoảng cách Manhattan trong hình chữ nhật. Nếu một ô được sơn nhiều lần thì chỉ có lần sơn cuối cùng mới quan trọng. 

Vì vậy, nhiệm vụ không phải là mô phỏng việc vẽ mà là quyết định xem liệu chúng ta có thể xây dựng lưới mục tiêu bằng cách sử dụng các dấu mẫu cờ vua này hay không, và nếu có, xuất ra bất kỳ chuỗi hình chữ nhật hợp lệ nào, với nhiều nhất là$n \cdot m$hoạt động. 

Các ràng buộc là nhỏ, với$n, m \le 100$, do đó, việc xây dựng là bậc hai cho mỗi trường hợp thử nghiệm có thể được chấp nhận. Số lượng trường hợp thử nghiệm cũng rất ít nên chúng tôi có thể đủ khả năng để kiểm tra lưới nhiều lần. Điều này gợi ý rõ ràng về một cách xây dựng tham lam hơn là bất kỳ tìm kiếm toàn cầu hoặc DP nào trên các tập hợp hoạt động con. 

Một điểm tinh tế là các thao tác sẽ ghi đè hoàn toàn các thao tác trước đó bên trong hình chữ nhật đã chọn. Điều này có nghĩa là chúng tôi không cần duy trì tính chính xác trước đó mà chỉ đảm bảo rằng các lần ghi đè cuối cùng phù hợp với mục tiêu. 

Trường hợp thất bại không rõ ràng đối với lý luận ngây thơ là giả sử mọi lưới đều có thể xây dựng được vì các mẫu cờ có vẻ linh hoạt. Ví dụ, không phải lúc nào cũng có thể tạo ra một số 1 đơn lẻ trong một biển số 0 mà không ảnh hưởng đến các ràng buộc chẵn lẻ xung quanh. Một trường hợp phức tạp khác là khi lưới buộc các yêu cầu chẵn lẻ trái ngược nhau trên các hình chữ nhật chồng chéo, khiến việc tái thiết không thể thực hiện được ngay cả khi các mẫu cục bộ có vẻ nhất quán. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc xây dựng lưới từ ma trận 0 ban đầu bằng cách chọn hình chữ nhật và kiểm tra xem việc áp dụng mẫu cờ có làm giảm sự khác biệt giữa trạng thái hiện tại và trạng thái mục tiêu hay không. Điều này nhanh chóng trở nên khó giải quyết vì mỗi bước có$O(n^2 m^2)$lựa chọn hình chữ nhật và chuỗi có thể dài. Ngay cả một cuộc tìm kiếm nông cạn cũng bùng nổ theo kiểu tổ hợp. 

Quan sát quan trọng là hoạt động này có cấu trúc cực kỳ chặt chẽ: mỗi màu sơn xác định một bàn cờ được xác định đầy đủ có bậc tự do duy nhất là góc trên cùng bên trái của hình chữ nhật. Điều này có nghĩa là mỗi thao tác đang đặt một trường chẵn lẻ trên một ma trận con một cách hiệu quả. Thay vì nghĩ về cách xây dựng toàn bộ lưới, chúng ta có thể nghĩ ngược lại: bất cứ khi nào chúng ta thấy sự không khớp, chúng ta có thể cố gắng “sửa” nó bằng cách đặt một hình chữ nhật có màu cờ khớp chính xác với mẫu cục bộ được yêu cầu. 

Điều này dẫn đến chiến lược loại bỏ tham lam: xử lý các ô theo một thứ tự cố định và bất cứ khi nào một ô vẫn không chính xác, hãy buộc một hình chữ nhật được neo vào ô đó để giải quyết nó. Vì góc trên bên trái của mỗi thao tác xác định tính chẵn lẻ, nên việc neo đảm bảo thao tác sẽ sửa ô đó vĩnh viễn. 

Việc xây dựng hoạt động hiệu quả vì mọi thao tác có thể được chọn để nhắm mục tiêu đến vị trí chưa được giải quyết cuối cùng theo cách không phá vỡ các ô đã cố định trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | O(nm) | Quá chậm | 
| Xây dựng tham lam | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng câu trả lời một cách tham lam bằng cách quét qua lưới. 

1. Chúng tôi lặp lại tất cả các ô theo thứ tự hàng lớn. Lý do cho thứ tự này là khi một ô đã được sửa, các thao tác sau này sẽ không bao giờ cần sửa đổi ô đó nữa nếu chúng ta chỉ bắt đầu hình chữ nhật từ các vị trí sau. 
2. Đối với mỗi ô$(i, j)$, chúng tôi kiểm tra xem giá trị hiện tại đã khớp với mục tiêu chưa. 
3. Nếu trùng khớp, chúng ta không làm gì cả và tiếp tục. 
4. Nếu nó không khớp, chúng ta bắt đầu một hình chữ nhật có góc trên cùng bên trái tại$(i, j)$. Chúng tôi mở rộng hình chữ nhật này càng xa càng tốt đến phía dưới bên phải của lưới. 
5. Chúng ta áp dụng màu cờ vua cho hình chữ nhật này, nghĩa là ô$(x, y)$trong hình chữ nhật nhận giá trị$(x + y) \bmod 2$, với quy ước rằng$(i, j)$là số không. 
6. Chúng tôi ghi lại thao tác này và áp dụng nó vào lưới về mặt khái niệm. 

Điểm mấu chốt là bằng cách neo hình chữ nhật vào ô không chính xác đầu tiên, chúng tôi sẽ sửa nó ngay lập tức và vì các thao tác trong tương lai chỉ bắt đầu ở các vị trí sau đó nên việc chỉnh sửa này sẽ không được hoàn tác theo cách xung đột. 

Tại sao nó hoạt động: mỗi lần chúng tôi sửa một ô$(i, j)$, chúng tôi
