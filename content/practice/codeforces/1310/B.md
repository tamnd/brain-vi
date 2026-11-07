---
title: "CF 1310B - Loại bỏ kép"
description: "Chúng tôi được tổ chức một giải đấu loại trực tiếp kép hoàn toàn với các đội $2^n$, trong đó cấu trúc bảng đấu hoàn toàn cố định."
date: "2026-06-16T06:17:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1310
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2019-2020 - Elimination Round (Engine)"
rating: 2500
weight: 1310
solve_time_s: 59
verified: false
draft: false
---

[CF 1310B - Loại bỏ kép](https://codeforces.com/problemset/problem/1310/B) 

**Đánh giá:** 2500 
**Tags:** dp, triển khai 
**Thời gian giải:** 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tổ chức một giải đấu loại trực tiếp kép đầy đủ với$2^n$các đội, trong đó cấu trúc khung hoàn toàn cố định. Mọi trận đấu đều được xác định trước bằng quy tắc xếp hạt giống: các đội có chỉ số sát nhau gặp nhau trước, sau đó đội thắng sẽ tiến lên ở khung trên, trong khi đội thua sẽ rơi vào khung dưới tương ứng để cuối cùng tạo ra người thách đấu cho trận chung kết. 

Chúng tôi cũng được tặng một bộ$k$đội bóng yêu thích. Chúng ta được phép quyết định kết quả của mỗi trận đấu một cách tùy ý. Mục tiêu là tối đa hóa số trận đấu trong toàn bộ giải đấu có sự tham gia của ít nhất một đội yêu thích. 

Quan điểm quan trọng là đây không phải là vấn đề ai thắng, mà là về tần suất các đội yêu thích có thể được giữ sống sót và đấu với những đội khác, ở dấu ngoặc trên hoặc dưới. Mỗi trận đấu đóng góp 1 nếu có ít nhất một người tham gia được yêu thích. 

Cấu trúc giải đấu là một cây nhị phân đầy đủ ở khung trên kết hợp với cấu trúc phản chiếu nhiều lớp ở khung dưới. Mỗi trận đấu được cố định bằng cách phân chia các chỉ số đội, vì vậy toàn bộ giải đấu có thể được coi là sự phân chia đệ quy các khoảng thời gian của các đội. 

Những hạn chế$n \le 17$ngụ ý lên đến$2^{17} = 131072$các đội. Tổng số trận đấu cũng là$O(2^n)$, do đó, bất kỳ giải pháp nào cũng phải gần như tuyến tính hoặc gần tuyến tính về số lượng nút trong cây giải đấu ẩn này. 

Việc mô phỏng một cách ngây thơ tất cả các kết quả có thể xảy ra là không thể. Ngay cả việc ấn định kết quả một cách tham lam trong mỗi trận đấu cũng thất bại vì các quyết định ảnh hưởng đến cấu trúc trận đấu trong tương lai ở cả hai bảng đấu. 

Một trường hợp thất bại tinh vi của lý luận tham lam xảy ra khi hai đội ưa thích gặp nhau sớm. Ví dụ: nếu hai đội yêu thích trong cùng một trận đấu ở vòng đầu tiên thì chỉ một trận đấu được tính, nhưng việc sắp xếp lại kết quả một cách cẩn thận có thể làm tăng tổng số người tham gia vào các vòng sau bằng cách kiểm soát vị trí của những người thua cuộc. Những lựa chọn tham lam của địa phương không nắm bắt được sự đánh đổi toàn cầu này. 

Một trường hợp khó khăn khác là khi có chính xác một đội yêu thích. Chiến lược tối ưu không phải lúc nào cũng khiến nó thắng mọi thứ, bởi vì việc đưa nó xuống nhánh thua có thể làm tăng số trận mà nó tham gia do cấu trúc của nhánh thua, như
