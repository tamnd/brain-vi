---
title: "CF 1292C - Cuộc tấn công băng đảng của Xenon"
description: "Chúng ta có một cây có các nút $n$ và các cạnh $n-1$. Mỗi cạnh được gán một nhãn riêng biệt từ $0$ đến $n-2$, do đó mỗi nhãn xuất hiện đúng một lần. Đối với bất kỳ cặp nút $u, v$ nào, chúng ta xem xét đường dẫn duy nhất giữa chúng và thu thập tất cả các nhãn cạnh trên đường dẫn đó."
date: "2026-06-16T04:19:27+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1292
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 614 (Div. 1)"
rating: 2300
weight: 1292
solve_time_s: 70
verified: false
draft: false
---

[CF 1292C - Cuộc tấn công băng đảng của Xenon](https://codeforces.com/problemset/problem/1292/C) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, dfs và tương tự, dp, tham lam, cây 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$nút và$n-1$các cạnh. Mỗi cạnh được gán một nhãn riêng biệt từ$0$ĐẾN$n-2$, do đó mỗi nhãn xuất hiện đúng một lần. 

Đối với bất kỳ cặp nút nào$u, v$, chúng ta nhìn vào đường dẫn duy nhất giữa chúng và thu thập tất cả các nhãn cạnh trên đường dẫn đó. giá trị$\text{mex}(u, v)$là số nguyên không âm nhỏ nhất không xuất hiện trong số các nhãn đó. Chúng tôi tính tổng giá trị này trên tất cả các cặp nút không có thứ tự và muốn tối đa hóa tổng giá trị bằng cách chọn nhãn cạnh một cách tối ưu. 

Cấu trúc là cố định, chỉ có sự hoán vị của nhãn trên các cạnh là do chúng tôi kiểm soát. Đầu ra phụ thuộc hoàn toàn vào cách chúng ta sắp xếp các nhãn nhỏ trên các cạnh sao cho nhiều đường dẫn tránh được chúng. 

Ràng buộc$n \le 3000$ngụ ý một$O(n^2)$hoặc$O(n^2 \log n)$giải pháp có thể chấp nhận được. Bất cứ điều gì cố gắng mô phỏng rõ ràng tất cả các phép gán nhãn đều không thể thực hiện được vì có$(n-1)!$hoán vị. 

Một sai lầm ngây thơ là cho rằng vị trí chính xác của các nhãn không quan trọng lắm và thử suy luận cục bộ tham lam trên mỗi cạnh. Điều đó không thành công vì mỗi cạnh tham gia vào nhiều đường đi và việc gán một nhãn nhỏ cho cạnh “không quan trọng cục bộ” vẫn có thể ảnh hưởng đến số cặp bậc hai. 

Trường hợp thất bại thứ hai là chỉ nghĩ đến đường đi ngắn nhất hoặc cây con cục bộ. Ví dụ, trong một cây hình ngôi sao, việc đặt các nhãn nhỏ ở các cạnh trung tâm có tác động tổng thể rất khác so với việc đặt chúng trên các lá, mặc dù cấu trúc cục bộ trông có vẻ đối xứng. 

Khó khăn cốt lõi là mex phụ thuộc vào _sự vắng mặt của tiền tố nhãn dọc theo đường dẫn_, điều này kết hợp tất cả các cạnh trên toàn cầu. 

## Phương pháp tiếp cận 

Phối cảnh vũ phu sẽ thử tất cả các hoán vị của nhãn cạnh và tính tổng kết quả. Đối với mỗi nhãn, chúng tôi sẽ tính toán$\text{mex}(u,v)$cho tất cả các cặp sử dụng truy vấn đường dẫn, ít nhất là$O(n)$mỗi cặp với tiền xử lý hoặc$O(n^3)$tổng số cho mỗi hoán vị. Vì có$(n-1)!$hoán vị, điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự quan tâm trực tiếp đến giá trị mex. Thay vào đó, chúng tôi diễn giải lại mex theo phạm vi bao phủ của tiền tố: với một ngưỡng cố định$k$, giá trị$\text{mex}(u,v) \ge k$khi và chỉ nếu tất cả các nhãn$0,1,\dots,k-1$xuất hiện ở đâu đó trên con đường giữa$u$Và$v$. 

Điều này chuyển đổi vấn đề từ việc tính toán các giá trị mex sang việc đếm, cho mỗi$k$, có bao nhiêu cặp được kết nối chỉ bằng các cạnh trong số các cặp đầu tiên$k$nhãn. 

Bây giờ cấu trúc trở nên rõ ràng hơn. Nếu chúng ta sửa tập hợp các cạnh mang nhãn$0$ĐẾN$k-1$, chúng tạo thành một khu rừng. Mỗi thành phần được kết nối đóng góp các cặp có đường dẫn không yêu cầu bất kỳ nhãn bị thiếu nào bên dưới$k$. Tổng trên tất cả$k$trở thành một hàm của kích thước thành phần trên các khu rừng này. 

Câu hỏi còn lại là làm thế nào để gán nhãn để tối đa hóa hiệu ứng kết nối tích lũy này. Chiến lược tối ưu hóa ra là tham lam ngược lại: chúng ta liên tục chọn một cạnh mà việc loại bỏ sẽ chia cây thành hai thành phần và gán cho nó nhãn lớn nhất còn lại theo cách tối đa hóa những đóng góp trong tương lai. Điều này làm giảm việc chọn liên tục một cạnh giống trọng tâm để tối đa hóa tích của các kích thước thành phần được hình thành khi cắt cạnh đó. 

Tương tự, mỗi cạnh đóng góp dựa trên số lượng đường dẫn của các cặp sử dụng nó làm “trình chặn ngưỡng bị thiếu đầu tiên” theo thứ tự ghi nhãn. Điều này dẫn đến việc tính toán kích thước cây con dựa trên DFS và lựa chọn tham lam các cạnh theo trọng số bằng$sz \cdot (n - sz)$, liên tục tích lũy những đóng góp tốt nhất. 

Chúng tôi tính toán tất cả các đóng góp của cạnh trong một DFS và sắp xếp chúng, sau đó gán nhãn lớn hơn cho các cạnh có trọng số đóng góp lớn hơn
