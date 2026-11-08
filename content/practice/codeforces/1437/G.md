---
title: "CF 1437G - DBMS tử thần"
description: "Chúng tôi duy trì một tập hợp các chuỗi cố định, mỗi chuỗi đại diện cho một “tên” và mỗi tên có một giá trị liên quan thay đổi theo thời gian."
date: "2026-06-14T17:32:15+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "string-suffix-structures", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1437
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 97 (Rated for Div. 2)"
rating: 2600
weight: 1437
solve_time_s: 220
verified: false
draft: false
---

[CF 1437G - Death DBMS](https://codeforces.com/problemset/problem/1437/G) 

**Đánh giá:** 2600 
**Tags:** cấu trúc dữ liệu, cấu trúc hậu tố chuỗi, chuỗi, cây 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp các chuỗi cố định, mỗi chuỗi đại diện cho một “tên” và mỗi tên có một giá trị liên quan thay đổi theo thời gian. Hệ thống hỗ trợ hai thao tác: cập nhật giá trị của một tên và truy vấn chuỗi văn bản để tìm tên được lưu trữ nào xuất hiện dưới dạng chuỗi con liền kề, báo cáo giá trị tối đa trong số các kết quả trùng khớp đó. 

Việc đọc truy vấn trực tiếp là: đưa ra một văn bản, quét từng tên được lưu trữ và kiểm tra xem nó có xuất hiện bên trong văn bản hay không, sau đó lấy điểm tối đa trong số tất cả các kết quả trùng khớp. Vì tên không thay đổi, chỉ có điểm số của chúng thay đổi, thách thức về cấu trúc hoàn toàn là về việc phát hiện chuỗi con nhanh trên nhiều mẫu có trọng số động. 

Các ràng buộc đẩy chúng ta vào một chế độ mà việc kiểm tra chuỗi con đơn giản là không thể. Có thể có tổng cộng tối đa 300.000 ký tự trên tất cả các tên được lưu trữ và chuỗi truy vấn được kết hợp. Nếu chúng tôi thử kiểm tra mọi tên đối với mọi truy vấn bằng cách sử dụng tìm kiếm chuỗi con, thì mỗi lần kiểm tra sẽ tuyến tính theo độ dài truy vấn, tạo ra trường hợp xấu nhất khoảng 10^10 thao tác, vượt xa giới hạn. 

Ý tưởng ngây thơ thứ hai là tính toán trước tất cả các chuỗi con của mỗi truy vấn và so sánh chúng với tập hợp các tên băm. Điều này không thành công vì một chuỗi truy vấn có độ dài L có chuỗi con O(L^2) và L có thể đủ lớn đến mức điều này trở nên không khả thi dù chỉ một lần. 

Một vấn đề phức tạp hơn xuất hiện khi các bản cập nhật diễn ra thường xuyên. Bất kỳ giải pháp nào tính toán trước các câu trả lời cho mỗi truy vấn một cách độc lập đều không thể sử dụng lại cấu trúc trên các truy vấn mà không hỗ trợ cập nhật giá trị động một cách hiệu quả. 

Khó khăn chính là chúng ta liên tục hỏi: “trong một từ điển cố định gồm các mẫu có trọng số thay đổi, mẫu nào xuất hiện trong văn bản này dưới dạng chuỗi con?” 

Các trường hợp Edge phá vỡ các cách tiếp cận ngây thơ bao gồm các tên lặp lại với các chỉ mục khác nhau nhưng các chuỗi được chia sẻ, các truy vấn chứa các lần xuất hiện chồng chéo của nhiều tên và các cập nhật làm giảm giá trị về 0 trong đó câu trả lời đúng vẫn phải coi nó là hợp lệ nếu không tồn tại bộ lọc phủ định. Một trường hợp đặc biệt khó phát hiện là khi không có tên nào xuất hiện trong một truy vấn, yêu cầu đầu ra −1 thay vì 0, do đó việc khởi tạo phải phân biệt cẩn thận “không khớp” với “khớp giá trị tối thiểu”. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Đối với chuỗi truy vấn q, chúng tôi lặp lại tất cả các tên si và kiểm tra xem si có phải là chuỗi con của q hay không bằng cách sử dụng tìm kiếm chuỗi con tiêu chuẩn như Knuth-Morris-Pratt hoặc so khớp tích hợp. Nếu tìm thấy kết quả trùng khớp, chúng tôi sẽ lấy giá trị hiện tại tối đa trong số tất cả các tên trùng khớp. 

Cách tiếp cận này đúng vì nó trực tiếp thực hiện định nghĩa của truy vấn. Tuy nhiên, mỗi truy vấn có giá O (tổng chiều dài q × số mẫu) trong trường hợp xấu nhất. Với cả hai tổng đạt tới 3 × 10^5 và truy vấn lên tới 3 × 10^5, điều này trở nên không thể. 

Quan sát làm thay đổi mọi thứ là chúng ta đang tìm kiếm nhiều mẫu cùng lúc trong một văn bản. Đây chính xác là cài đặt cho máy tự động nhiều mẫu, cụ thể là máy tự động Aho-Corasick. Nó xây dựng một bộ ba tất cả các tên và tăng thêm các liên kết lỗi sao cho một lần chuyển qua chuỗi truy vấn sẽ xác định tất cả các mẫu xuất hiện dưới dạng chuỗi con trong thời gian tuyến tính. 

Sự phức tạp còn lại là các giá trị mẫu là động. Chúng tôi cần hỗ trợ cập nhật điểm mẫu và truy vấn nhanh để có điểm tối đa trong số tất cả các mẫu khớp trong quá trình truyền tải. Điều này gợi ý việc duy trì, đối với mỗi nút trie tương ứng với một đầu mẫu, một giá trị có thể thay đổi và tổng hợp các kết quả thông qua cấu trúc hỗ trợ cập nhật điểm và phạm vi truy vấn tối đa dọc theo chuỗi liên kết lỗi.

Thủ thuật tiêu chuẩn là truyền từng mẫu đến nút cuối của nó trong máy tự động và duy trì cây phân đoạn hoặc cấu trúc giống Fenwick theo thứ tự Euler của cây liên kết lỗi. Mỗi mẫu đóng góp vào chính xác một nút và khi một chuỗi truy vấn được xử lý, mọi trạng thái máy tự động được truy cập sẽ tương ứng với một chuỗi liên kết hậu tố có các nút đại diện cho tất cả các mẫu phù hợp. Chúng tôi có thể tính toán trước cho mỗi nút danh sách ID mẫu kết thúc ở đó và duy trì cấu trúc toàn cầu được khóa theo thời gian nhập nút để truy xuất các giá trị hoạt động tối đa một cách hiệu quả. 

Điều này làm giảm mỗi truy vấn xuống O(|q| log n), thế là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra chuỗi con Brute Force | O(m · tổng chiều dài mẫu cho mỗi truy vấn) | O(tổng số mẫu) | Quá chậm | 
| Aho-Corasick + cây phân đoạn trên các nút mẫu | O((Σ | q | + cập nhật) log n) | 

## Hướng dẫn thuật toán 

Đầu tiên chúng tôi xây dựng một bản thử từ tất cả các tên nạn nhân. Mỗi tên được chèn theo từng ký tự và chúng tôi lưu trữ chỉ mục của tên tại nút cuối của nó. Điều này mang lại
