---
title: "CF 105537G - Trò chơi hủy diệt"
description: "Chúng ta được cấp một bảng một chiều rất lớn kéo dài vô tận về bên phải. Trên một số tế bào của nó có nhiều chip màu đỏ và xanh. Chip đỏ thuộc về người chơi đầu tiên, chip xanh thuộc về người chơi thứ hai."
date: "2026-06-27T00:59:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105537
codeforces_index: "G"
codeforces_contest_name: "2024-2025 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 105537
solve_time_s: 26
verified: false
draft: false
---

[CF 105537G - Trò chơi hủy diệt](https://codeforces.com/problemset/problem/105537/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng một chiều rất lớn kéo dài vô tận về bên phải. Trên một số tế bào của nó có nhiều chip màu đỏ và xanh. Chip đỏ thuộc về người chơi đầu tiên, chip xanh thuộc về người chơi thứ hai. Nhiều chip cùng màu có thể nằm trong cùng một ô, nhưng ban đầu mỗi ô bị chiếm giữ chỉ chứa các chip có một màu. 

Các cầu thủ luân phiên di chuyển. Trong một lượt, người chơi không làm gì hoặc chọn một trong các con chip của họ và di chuyển nó sang trái hoặc phải một bước. Nếu ô đích đã chứa ít nhất một chip đối thủ, cả hai bên ngay lập tức mất chính xác một chip ở đó, hủy bỏ một cặp một cách hiệu quả. Nếu nó chỉ chứa chip của riêng họ hoặc trống, chip chỉ cần tham gia vào ô đó. 

Người chơi sẽ thua ngay lập tức khi hết chip. Nếu cuối cùng cả hai đều hết, trò chơi sẽ hòa. Nếu trận đấu tiếp tục trong một thời gian rất dài mà không kết thúc thì cũng được coi là hòa. 

Nhiệm vụ là xác định kết quả trong lối chơi hoàn hảo và nếu người chơi đầu tiên có thể buộc một kịch bản thắng hoặc hòa/thắng, thì cũng đưa ra một nước đi đầu tiên hợp lệ để bắt đầu một chiến lược tối ưu. 

Đầu vào chỉ mô tả các ô không trống dưới dạng cặp vị trí, số lượng chip và màu sắc. Các vị trí lên tới 10^6, trong khi tổng số chip trên tất cả các ô có thể lên tới 10^6 cho mỗi thang trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng phải tuyến tính nhất về số lượng ô bị chiếm dụng, bởi vì mọi nỗ lực mô phỏng chuyển động của chip từng bước sẽ yêu cầu thời gian tỷ lệ thuận với số lần di chuyển, có thể là bậc hai hoặc tệ hơn. 

Một mô phỏng ngây thơ của cách chơi tối ưu xen kẽ là không thể. Ngay cả việc theo dõi từng chip riêng lẻ cũng dẫn đến số lượng chuyển động trong trường hợp xấu nhất tỷ lệ thuận với khoảng cách giữa các ô cách xa nhau nhân với số lượng chip, dễ dàng vượt quá 10^12 thao tác. 

Một trường hợp phức tạp phát sinh khi các chip bị mất cân bằng nghiêm trọng ở một vùng. Ví dụ: một chip màu đỏ ở xa bên phải của nhiều chip xanh sẽ tạo ra chuỗi tương tác dài. Cách tiếp cận tham lam ngây thơ “luôn di chuyển về phía kẻ thù gần nhất” có thể thất bại vì nó bỏ qua động lực hủy bỏ trong đó nhiều chip tiêu diệt hàng loạt một cách hiệu quả thay vì từng con một. 

Một trường hợp cạnh quan trọng khác là khi tất cả các chip của một màu được phân cụm cách xa màu kia. Mặc dù dường như không thể tương tác cục bộ, nhưng cách chơi tối ưu vẫn cho phép chờ đợi, bỏ lượt và thay đổi vị trí, vì vậy lý luận “chẵn lẻ khoảng cách” đơn giản là không đủ. 

## Phương pháp tiếp cận 

Một mô hình vũ lực trực tiếp
