---
title: "CF 103914K - Tính đối xứng: Lồi"
description: "Chúng ta có một đa giác lồi hoàn toàn với các đỉnh được liệt kê theo thứ tự ngược chiều kim đồng hồ. Từ đa giác này, chúng ta xem xét một chuỗi tiền tố ngày càng tăng: đa giác được hình thành bởi 3 đỉnh đầu tiên, sau đó là 4 đỉnh đầu tiên, v.v. cho đến tất cả n đỉnh."
date: "2026-07-02T07:28:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103914
codeforces_index: "K"
codeforces_contest_name: "Heltion Contest 1"
rating: 0
weight: 103914
solve_time_s: 23
verified: false
draft: false
---

[CF 103914K - Tính đối xứng: Lồi](https://codeforces.com/problemset/problem/103914/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác lồi hoàn toàn với các đỉnh được liệt kê theo thứ tự ngược chiều kim đồng hồ. Từ đa giác này, chúng ta xem xét một chuỗi tiền tố ngày càng tăng: đa giác được hình thành bởi 3 đỉnh đầu tiên, sau đó là 4 đỉnh đầu tiên, v.v. cho đến tất cả n đỉnh. Đối với mỗi đa giác tiền tố, chúng ta phải xác định tất cả các đường thẳng đóng vai trò là trục đối xứng của đa giác đó. 

Một đường được coi là một câu trả lời hợp lệ nếu việc phản ánh đa giác đối với đường đó sẽ ánh xạ đa giác lên chính nó. Bởi vì đa giác là lồi và các đỉnh được sắp xếp theo thứ tự, tính đối xứng hoàn toàn được xác định bằng cách ghép các chỉ số đỉnh dưới sự phản chiếu. 

Đầu ra không phải là một giá trị cho mỗi tiền tố mà có thể là nhiều dòng. Với mỗi i, chúng ta phải xuất ra mọi trục đối xứng của đa giác được hình thành bởi các đỉnh p1 đến pi. 

Các ràng buộc rất lớn: tổng số đỉnh trong tất cả các trường hợp thử nghiệm lên tới 3 · 10^5 và T có thể lớn tới 10^5. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào tính toán lại sự đối xứng từ đầu cho từng tiền tố theo thời gian bậc hai hoặc thậm chí tuyến tính cho mỗi tiền tố. Ngay cả tổng số O(n^2) trên tất cả các trường hợp thử nghiệm cũng sẽ quá chậm. 

Khó khăn chính về cấu trúc là mỗi đa giác tiền tố không độc lập. Khi một đỉnh mới được thêm vào, tính đối xứng chỉ có thể được bảo toàn hoặc bị phá hủy và mọi đối xứng hợp lệ phải thẳng hàng với cấu trúc hình tròn của chuỗi đỉnh. 

Một sai lầm ngây thơ là cho rằng mọi tiền tố có thể có nhiều đối xứng và cố gắng kiểm tra tất cả các trục phản xạ có thể có bằng cách ghép các điểm. Ví dụ: tiền tố đa giác thông thường có thể có nhiều đối xứng, nhưng tiền tố lồi tùy ý thường không có tiền tố nào ngoài các đối xứng ngẫu nhiên. 

Một trường hợp cạnh tinh vi khác phát sinh khi các tiền tố đầu bị suy biến về tính đối xứng trong khi các tiền tố sau mất toàn bộ tính đối xứng. Ví dụ: một hình tam giác (i = 3) luôn có tối đa 3 trục phản xạ tùy theo hình học, nhưng việc thêm điểm lồi thứ tư tùy ý thường làm giảm tính đối xứng xuống nhiều nhất là 1 hoặc 0 trục. Bất kỳ giải pháp nào tính toán lại độc lập cho mỗi tiền tố sẽ hết thời gian chờ ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ xử lý từng tiền tố Ci một cách độc lập. Đối với i cố định, chúng ta có thể thử từng cặp đỉnh và cố gắng xác định trục phản xạ ứng viên là đường phân giác vuông góc của cặp đó, sau đó xác minh xem liệu phản xạ tất cả các đỉnh có bảo toàn tập hợp hay không. Đây đã là O(i^3) trong trường hợp xấu nhất nếu được thực hiện cẩn thận, vì đối với mỗi trục ứng viên, chúng ta phải xác thực tất cả các điểm. 

Ngay cả khi được tối ưu hóa, việc kiểm tra tính đối xứng của đa giác lồi cho một trục là O(i) và có các trục ứng cử viên O(i) xuất phát từ các cặp điểm, dẫn đến O(i^2) trên mỗi tiền tố và tổng O(n^3) trên tất cả các tiền tố. Điều này vượt xa giới hạn. 

Quan sát cấu trúc quan trọng là tính đối xứng của đa giác lồi cực kỳ cứng nhắc. Bất kỳ sự đối xứng phản xạ nào cũng phải ánh xạ chu trình có thứ tự của các đỉnh lên chính nó. Điều này ngụ ý rằng nếu một sự đối xứng tồn tại, nó hoạt động như một sự đảo ngược trên các chỉ số, ghép các đỉnh đối xứng quanh một số trục. Trong một đa giác lồi, sự đảo chiều như vậy được xác định hoàn toàn bởi vị trí “trung tâm” theo thứ tự tuần hoàn: hoặc một đỉnh ánh xạ tới một đỉnh (trục xuyên qua các đỉnh đối diện) hoặc một cạnh ánh xạ tới một cạnh (trục xuyên qua điểm giữa). 

Bây giờ hãy xem xét việc thêm các đỉnh trên
