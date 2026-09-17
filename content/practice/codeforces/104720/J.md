---
title: "CF 104720J - Cá Hồi Khói"
description: "Chúng ta được cung cấp một lưới biểu thị sàn nhà bếp. Một số ô bị chặn, một số ô mở và một ô chứa vị trí bắt đầu của đầu bếp trong khi ô khác chứa tủ lạnh."
date: "2026-06-29T07:12:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104720
codeforces_index: "J"
codeforces_contest_name: "UTPC x WiCS Contest 10-06-23"
rating: 0
weight: 104720
solve_time_s: 27
verified: false
draft: false
---

[CF 104720J - Cá hồi khói](https://codeforces.com/problemset/problem/104720/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới biểu thị sàn nhà bếp. Một số ô bị chặn, một số ô mở và một ô chứa vị trí bắt đầu của đầu bếp trong khi ô khác chứa tủ lạnh. Người đầu bếp di chuyển từng bước một theo bốn hướng chính và phải luôn di chuyển, không bao giờ đứng yên tại chỗ. 

Mỗi lần di chuyển đều có chi phí cạn kiệt cơ bản là 1. Tuy nhiên, lưới điện bị ảnh hưởng bởi khói không tĩnh. Thay vào đó, có một lưới thứ hai lặp lại theo chiều ngang và dịch chuyển sang trái một cột mỗi bước. Ở mỗi bước thời gian, mỗi ô của căn bếp đều có khói hoặc không bị khói bao phủ, tùy thuộc vào sự dịch chuyển này. Nếu đầu bếp rời khỏi một ô vào thời điểm ô đó bị khói bao phủ, chi phí cho việc di chuyển đó sẽ trở thành 3 thay vì 1. 

Nhiệm vụ là tính toán tổng lượng khí thải tối thiểu có thể cần thiết để di chuyển từ ô ban đầu đến tủ lạnh, có tính đến kiểu khói phát triển theo thời gian và ảnh hưởng đến chi phí để rời khỏi mỗi ô. 

Kích thước lưới tối đa là 100 x 100 và khoảng thời gian khói tối đa là 100 cột. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào chỉ phụ thuộc vào các ô lưới là không đủ vì chi phí cũng phụ thuộc vào thời gian. Một trạng thái hợp lệ phải mã hóa không chỉ vị trí mà còn cả thời gian điều chỉnh chu kỳ khói. Điều này đẩy chúng ta tới bài toán đường đi ngắn nhất trong không gian trạng thái với tối đa khoảng 100 × 100 × 100 trạng thái, tức là khoảng một triệu trạng thái, nằm trong giới hạn đối với giải pháp dựa trên Dijkstra. 

Một nỗ lực ngây thơ bỏ qua thời gian hoặc giả định chi phí cố định cho mỗi ô sẽ thất bại vì cùng một ô có thể có các chi phí khác nhau tùy thuộc vào thời điểm nó còn lại. 

Một trường hợp thất bại khó phát hiện khi tuyến đường tối ưu phụ thuộc vào việc chờ đợi gián tiếp bằng cách đi đường vòng. Ví dụ: hãy xem xét một con đường trong đó việc đi qua một tuyến đường ngắn hơn buộc bạn phải rời khỏi một ô trong thời điểm có nhiều khói, trong khi đường vòng dài hơn một chút sẽ điều chỉnh cùng một ô với thời điểm không khói, giúp giảm chi phí. Bất kỳ giải pháp nào giảm điều này thành lưới có trọng số tĩnh đều không thể nắm bắt được sự tương tác này. 

Một trường hợp lỗi khác phát sinh nếu người ta giả định rằng kiểu khói được cố định trên mỗi ô. Vì mô hình dịch chuyển từng bước nên một giải pháp đúng phải coi thời gian là một phần của trạng thái, nếu không, các đường dẫn không gian giống hệt nhau được thực hiện tại các thời điểm khác nhau sẽ bị hợp nhất không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ đơn giản là mô phỏng tất cả các đường đi có thể có qua lưới trong khi theo dõi thời gian một cách rõ ràng. Ở mỗi bước, chúng tôi cố gắng di chuyển theo bốn hướng và giữ nguyên bước thời gian hiện tại. Chi phí của mỗi lần di chuyển phụ thuộc vào việc ô hiện tại có bị khói vào thời điểm đó hay không. Điều này có thể được thực hiện bằng cách tìm kiếm đầy đủ trên tất cả các đường dẫn có thể. 

Tuy nhiên, số lượng đường đi có thể tăng theo cấp số nhân với độ dài đường đi. Ngay cả trong lưới 100 x 100, có thể có một số lượng lớn các đường dẫn đơn giản và chiều thời gian nhân con số này lên tới 100 pha. Cách tiếp cận này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là mặc dù thời gian trôi qua nhưng điều đó chỉ quan trọng ở modulo K, vì dạng khói lặp lại sau mỗi K bước. Điều này biến bài toán thành bài toán đường đi ngắn nhất trên biểu đồ mở rộng trong đó mỗi trạng thái được xác định bởi một bộ dữ liệu (hàng, cột, mod thời gian K). Từ mỗi trạng thái, có tối đa bốn lần chuyển đổi sang các ô liền kề và mỗi lần chuyển đổi sẽ tăng thời gian theo modulo K. 

Cấu trúc này cho phép chúng ta áp dụng thuật toán Dijkstra vì tất cả các trọng số của cạnh đều không âm (1 hoặc 3). Không gian trạng thái tối đa là 10^6 nút và các cạnh gấp khoảng bốn lần, đủ hiệu quả với hàng đợi ưu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo chiều dài đường dẫn | O(1) thêm | Quá chậm | 
| Dijkstra mở rộng theo thời gian | O(N M K log(N M K)) | O(N M K) | Đã chấp nhận | 

## Hướng dẫn thuật toán

### 1. Tính toán trước smo
