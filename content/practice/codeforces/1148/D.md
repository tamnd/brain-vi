---
title: "CF 1148D - Việc làm bẩn thỉu Làm bẩn giá rẻ"
description: "Chúng ta có một tập hợp $n$ các cặp số nguyên rời nhau. Mỗi số nguyên từ $1$ đến $2n$ xuất hiện chính xác một lần trên tất cả các cặp, vì vậy mỗi số thuộc về chính xác một cặp và không có sự trùng lặp."
date: "2026-06-12T03:09:35+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1148
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 3"
rating: 1800
weight: 1148
solve_time_s: 36
verified: false
draft: false
---

[CF 1148D - Hành động bẩn thỉu được thực hiện với giá rẻ mạt](https://codeforces.com/problemset/problem/1148/D) 

**Đánh giá:** 1800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 36s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập$n$các cặp số nguyên rời nhau. Mọi số nguyên từ$1$ĐẾN$2n$xuất hiện chính xác một lần trên tất cả các cặp, vì vậy mỗi số thuộc về đúng một cặp và không có sự trùng lặp. 

Chúng ta được phép chọn một số cặp này và sắp xếp các cặp đã chọn theo thứ tự. Sau khi sắp xếp thứ tự, chúng ta nối chúng bằng cách viết cả hai phần tử của mỗi cặp theo thứ tự đó, tạo ra một chuỗi có độ dài$2t$nếu chúng ta chọn$t$cặp. Hạn chế là chuỗi kết quả này phải xen kẽ chặt chẽ giữa các giá trị tăng và giảm, bắt đầu bằng mức tăng hoặc bắt đầu bằng mức giảm. 

Vì vậy, đầu ra không chỉ là một tập hợp con của các cặp mà còn là thứ tự của các cặp đó sao cho khi mở rộng, chuỗi đầy đủ sẽ xen kẽ lên hoặc xuống một cách nghiêm ngặt. 

Ràng buộc chính là tất cả các giá trị đều khác biệt trên toàn cầu và bao trùm một phạm vi cố định. Điều này cho thấy rõ ràng rằng các quyết định đặt hàng được thúc đẩy bởi sự so sánh giữa các giá trị chứ không phải bất kỳ cấu trúc bên trong nào của các phần tử lặp lại. 

giới hạn$n \le 3 \cdot 10^5$ngụ ý rằng bất kỳ$O(n^2)$hoặc thậm chí$O(n \log n)$Phương pháp mô phỏng nặng theo từng cặp sẽ gặp rủi ro nếu nó liên quan đến việc kiểm tra tính khả thi lặp đi lặp lại hoặc DP theo cặp. Một chiến lược tham lam tuyến tính hoặc gần tuyến tính được mong đợi. 

Một nỗ lực ngây thơ sẽ thử tất cả các tập hợp con hoặc cố gắng xây dựng một chuỗi xen kẽ hợp lệ dài nhất bằng DP trên các tập hợp con của các cặp. Điều đó không thành công vì ngay cả việc kiểm tra tính khả thi của việc đặt hàng$k$các cặp yêu cầu xác minh các ràng buộc xen kẽ trong quá trình ghép nối và có nhiều tập hợp con và thứ tự theo cấp số nhân. 

Một kẻ tham lam ngây thơ tinh tế hơn có thể sắp xếp các cặp theo phần tử thứ nhất hoặc phần tử thứ hai và cố gắng xây dựng sự xen kẽ một cách tham lam. Điều này phá vỡ trong trường hợp các quyết định đặt hàng địa phương chặn việc mở rộng toàn cầu. Ví dụ: việc chọn sớm một cặp có phần tử đầu tiên rất lớn có thể buộc bước tiếp theo không hợp lệ vì phần tử thứ hai của nó có thể chiếm ưu thế trong tất cả các tùy chọn còn lại. 

Một kịch bản thất bại cụ thể đối với kẻ tham lam ngây thơ là khi có nhiều cặp xen kẽ trong không gian giá trị: 

Ý tưởng đầu vào:$$(1, 100), (2, 99), (3, 98), \dots$$Nếu chúng ta tham lam chọn dựa trên phần tử thứ nhất nhỏ hoặc phần tử thứ hai lớn mà không xem xét cấu trúc xen kẽ, chúng ta sẽ nhanh chóng gặp khó khăn mặc dù tồn tại một chuỗi xen kẽ dài. 

Giải pháp đúng phải khai thác thuộc tính cấu trúc của các chuỗi xen kẽ và thứ tự tổng thể của các số. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng xây dựng tất cả các tập hợp con của các cặp và tất cả các hoán vị trong mỗi tập hợp con, kiểm tra xem chuỗi được nối có thay thế hay không. Thậm chí hạn chế lựa chọn tập hợp con, có$2^n$các lựa chọn và đối với mỗi tập hợp con, việc sắp xếp các cặp tốn ít nhất$O(t \log t)$hoặc$O(t)$kiểm tra, dẫn đến độ phức tạp theo cấp số nhân. 

Quan sát quan trọng là ràng buộc trình tự hoàn toàn mang tính cục bộ: mỗi cặp được nối thêm mới chỉ tương tác với số cuối cùng của chuỗi hiện tại. Điều này cho thấy chúng ta nên duy trì một “giá trị cuối hiện tại” duy nhất và thực thi rằng mỗi cặp tiếp theo phải mở rộng luân phiên một cách chính xác. 

Cái nhìn sâu sắc hơn về cấu trúc là mỗi cặp đóng góp hai số và chúng ta có thể chọn hướng ngầm thông qua việc sắp xếp các cặp trong chuỗi. Thay vì quyết định hướng một cách độc lập, chúng tôi coi mỗi cặp là một khối linh hoạt phải phù hợp với một chuỗi xen kẽ. 

Chiến lược tối ưu trở thành một công trình tham lam: chúng tôi sắp xếp các cặp theo một tọa độ và cố gắng xây dựng một chuỗi xen kẽ dài nhất bằng cách luôn mở rộng khi có thể. Bí quyết là khi các cặp được xử lý theo thứ tự được sắp xếp, chúng ta có thể duy trì giá trị được sử dụng cuối cùng và quyết định xem liệu chúng ta có thể nối thêm một cặp mới trong khi vẫn duy trì xen kẽ hay không, giảm vấn đề một cách hiệu quả thành lựa chọn chuỗi con tham lam trên các cặp có thứ tự. 

Lý do sắp xếp hữu ích là thứ tự toàn cục của các số từ$1$ĐẾN$2n$thực thi cấu trúc đơn điệu: khi chúng tôi xử lý các cặp theo thứ tự tăng dần của một điểm cuối, chúng tôi đảm bảo rằng các lựa chọn trong tương lai không làm mất hiệu lực các quyết định thay thế trước đó vì tất cả xung đột đều được giải quyết bằng cách so sánh với một giá trị biên duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam tối ưu |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống việc xây dựng chuỗi các cặp xen kẽ hợp lệ dài nhất theo thứ tự một lượt. 

### Các bước 

1. Sắp xếp tất cả các cặp theo phần tử đầu tiên của chúng. 

Điều này đưa ra một thứ tự cố định trong đó chúng tôi xử lý các ứng viên từ giá trị đầu tiên nhỏ hơn đến lớn hơn, giảm xung đột trong tương lai. 
2. Duy trì hai trạng thái xen kẽ có thể có: một trạng thái trong đó so sánh cuối cùng là “lớn hơn” và một trạng thái là “nhỏ hơn”. 

Những điều này tương ứng với việc so sánh được yêu cầu tiếp theo sẽ tăng hay giảm. 
3. Bắt đầu từ một chuỗi trống và cố gắng mở rộng một cách tham lam. Đối với mỗi cặp theo thứ tự được sắp xếp, hãy cố gắng đặt nó ở cuối chuỗi hiện tại nếu nó thỏa mãn điều kiện luân phiên bắt buộc đối với số cuối cùng được sử dụng trong chuỗi mở rộng. 
4. Đối với mỗi cặp, hãy mô phỏng cả hai hướng có thể đặt hai giá trị của nó. Một hướng có thể thỏa mãn điều kiện luân phiên trong khi hướng kia có thể không, vì vậy chúng tôi luôn chọn hướng duy trì tính khả thi. 
5. Nối cặp nếu ít nhất một hướng hợp lệ theo yêu cầu luân phiên hiện tại, đồng thời cập nhật giá trị cuối cùng và lật hướng so sánh dự kiến. 
6. Tiếp tục cho đến khi tất cả các cặp được xử lý. 

Việc xây dựng đảm bảo rằng chúng tôi luôn mở rộng chuỗi xen kẽ hợp lệ dài nhất có thể nhất quán
