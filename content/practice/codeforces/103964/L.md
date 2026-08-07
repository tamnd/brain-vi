---
title: "CF 103964L - Thuốc của Huatuo"
description: "Giả sử $c1c2cdots cn$ là biểu diễn số phần của một phân vùng của $n$, sao cho $sum{j=1}^n j cj = n.$ Thứ tự colex trên các phân vùng tương ứng với thứ tự từ điển trên vectơ đảo ngược $cn c{n-1}cdots c1$, do đó, các phân vùng liên tiếp có được bằng cách tạo ra sớm nhất…"
date: "2026-07-04T11:25:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103964
codeforces_index: "L"
codeforces_contest_name: "The 2015 China Collegiate Programming Contest (CCPC 2015)"
rating: 0
weight: 103964
solve_time_s: 107
verified: false
draft: false
---

[CF 103964L - Thuốc của Huatuo](https://codeforces.com/problemset/problem/103964/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

hãy để$c_1c_2\cdots c_n$là đại diện đếm phần của một phân vùng của$n$, để có thể$\sum_{j=1}^n j c_j = n.$Thứ tự colex trên các phân vùng tương ứng với thứ tự từ điển trên vectơ đảo ngược$c_n c_{n-1}\cdots c_1$, do đó, có được các phân vùng liên tiếp bằng cách thực hiện thay đổi sớm nhất có thể khi quét các chỉ số từ dưới lên$n$. 

Một phân vùng đạt cực đại theo thứ tự này một cách chính xác khi nó ít nhất không có phần nào về kích thước.$2$, đó là khi$c_2=\cdots=c_n=0$, kể từ đây$c_1=n$. Đây là yếu tố cuối cùng của thế hệ. 

Bất kỳ phân vùng nonterminal nào cũng chứa một số chỉ mục lớn nhất$k\ge 2$với$c_k>0$. Theo cách giải thích của Ferrers, điều này tương ứng với phần ngoài cùng bên phải vượt quá$1$. Phép toán kế tiếp trong Thuật toán P sẽ thay thế một phần như vậy$k$qua$k-1$cùng với một bổ sung$1$, bảo toàn tổng số tiền kể từ$k = (k-1)+1.$Ở dạng đếm phần, phép biến đổi này thay đổi chính xác ba thành phần:$c_k \leftarrow c_k - 1,\quad c_{k-1} \leftarrow c_{k-1} + 1,\quad c_1 \leftarrow c_1 + 1.$Tất cả khác$c_j$vẫn không thay đổi. 

Hoạt động này bảo toàn danh tính phân vùng vì tổng đóng góp của các chỉ số được sửa đổi thay đổi theo$-k + (k-1) + 1 = 0.$Cấu trúc liên kết$l_0,l_1,\dots,l_n$duy trì trình tự tăng dần của các chỉ số$k$với$c_k>0$. Nếu các chỉ số hiện tại khác 0 là$k_1<\cdots<k_t$, sau đó$l_0=k_1$,$l_{k_i}=k_{i+1}$, Và$l_{k_t}=0$. Điều này cho phép định vị lớn nhất$k$với$c_k>0$bằng cách đi qua từ$l_0$tiến về phía trước cho đến khi đạt được$0$và sau đó quay trở lại các phiên bản trước được lưu trữ nếu được triển khai hoặc tương đương duy trì một con trỏ đuôi tới$k_t$. 

Thuật toán tạo được định nghĩa như sau. 

P1. [Khởi tạo.] Đặt$c_n\leftarrow 1$Và$c_j\leftarrow 0$vì$1\le j<n$. Bộ$l_0\leftarrow n$,$l_n\leftarrow 0$và tất cả các liên kết khác không được xác định. Bộ$k\leftarrow n$. 

P2. [Truy cập.] Truy cập vectơ hiện tại$c_1c_2\cdots c_n$. Sau đó đặt$k$đến chỉ số lớn nhất với$c_k>0$Và$k\ge 2$bằng cách đi theo chuỗi liên kết từ$l_0$cho đến khi tới nút cuối cùng. Nếu không như vậy$k$tồn tại, chấm dứt. 

P3. [Tách bước.] Đặt$c_k\leftarrow c_k-1$,$c_{k-1}\leftarrow c_{k-1}+1$, Và$c_1\leftarrow c_1+1$. 

P4. [Cập nhật liên kết cho$k$Và$k-1$.] Nếu như$c_k=0$, di dời$k$khỏi danh sách liên kết bằng cách chuyển hướng liên kết trước đó của nó: if$p=l_0,\dots$là tiền thân của$k$, sau đó đặt$l_p\leftarrow l_k$. Nếu như$c_{k-1}$đã từng là$0$trước khi tăng, chèn$k-1$vào danh sách bằng cách thiết lập$l_{k-1}\leftarrow l_k$và sau đó thiết lập$l_k\leftarrow k-1$; mặt khác không cần thay đổi cấu trúc ngoài việc điều chỉnh số lượng. 

P5. [Cập nhật$k$.] Trở lại P2. 

Mỗi lần lặp thay thế chính xác một phần$k\ge 2$qua$k-1$Và$1$, điều này làm giảm nghiêm ngặt cách biểu diễn từ điển đảo ngược do tọa độ được sửa đổi đầu tiên từ bên phải bị giảm từ$1$(trong phần mở rộng nhị phân tiềm ẩn của phần đó) thành cấu hình bắt đầu bằng mục nhập khác 0 nhỏ hơn. Điều này phù hợp với cấu trúc kế thừa của Thuật toán P, trong đó phần không phải ngoài cùng bên phải$1$một phần bị giảm đi và phần đuôi của$1$s được điều chỉnh. 

Mỗi phân vùng khác biệt với$1^n$chứa ít nhất một phần$\ge 2$, do đó một số$k\ge 2$tồn tại bất cứ khi nào thuật toán chưa kết thúc. Việc chuyển đổi làm tăng nghiêm ngặt số lượng các phần và chuyển trọng số sang các chỉ số nhỏ hơn, đảm bảo không có phân vùng nào bị lặp lại, vì việc đảo ngược hoạt động sẽ yêu cầu hợp nhất một phần$1$với một$k-1$khôi phục lại trạng thái trước đó một cách duy nhất. 

Vì mỗi bước tương ứng với một ứng dụng duy nhất của phép biến đổi trên số lớn nhất được xác định duy nhất$k\ge 2$, mỗi phân vùng có chính xác một phân vùng trước và một phân vùng kế tiếp trong quá trình này, khớp với cấu trúc truyền tải tuyến tính của phân vùng poset theo thứ tự colex. 

Điều này hoàn thành việc xây dựng và chứng minh tính đúng đắn. ∎
