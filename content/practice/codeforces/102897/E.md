---
title: "CF 102897E - BM \u5145\u9965"
description: "Một phân vùng của $n$ ở dạng đếm phần là một vectơ $c1,dots,cn$ thỏa mãn $c1+2c2+cdots+n cn=n,$ trong đó $ck$ là số phần bằng $k$."
date: "2026-07-04T09:22:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102897
codeforces_index: "E"
codeforces_contest_name: "The 3rd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102897
solve_time_s: 162
verified: false
draft: false
---

[CF 102897E - BM \u5145\u9965](https://codeforces.com/problemset/problem/102897/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Một phân vùng của$n$ở dạng đếm phần là một vectơ$c_1,\dots,c_n$thỏa mãn$c_1+2c_2+\cdots+n c_n=n,$Ở đâu$c_k$là số phần bằng$k$. Thứ tự colex của các phân vùng là thứ tự từ điển của trình tự đảo ngược$c_n,\dots,c_1$, vậy tọa độ quan trọng nhất là$c_n$và ít quan trọng nhất là$c_1$. 

Sự biểu diễn mà bài tập yêu cầu sẽ thêm một danh sách liên kết của các chỉ số$k$vì cái gì$c_k>0$. Nếu các chỉ số này$k_1<\cdots<k_t$, sau đó$l_0=k_1,\quad l_{k_1}=k_2,\ \dots,\ l_{k_{t-1}}=k_t,\quad l_{k_t}=0.$Cấu trúc này hỗ trợ truy cập liên tục theo thời gian tới các kích thước bộ phận chiếm dụng nhỏ nhất và lớn hơn tiếp theo. 

Thuật toán tuân theo ý tưởng cấu trúc tương tự như Thuật toán P: duy trì phân vùng hiện tại, truy cập phân vùng đó, sau đó xác định vị trí ngoài cùng bên phải theo thứ tự colex có thể tăng lên, thực hiện mức tăng tối thiểu ở đó và sửa chữa tính khả thi bằng một phép biến đổi cục bộ xác định nhằm duy trì các ràng buộc phân vùng. 

Quan sát quan trọng là trong biểu diễn trình tự, sửa đổi quan trọng là tách một phần$x+1$vào trong$x$theo sau là$1$hoặc giảm một phần và phân phối lại khối lượng đơn vị còn lại thành các phần nhỏ hơn. Trong biểu diễn số phần, cả hai thao tác đều tương ứng với việc di chuyển một đơn vị khối lượng giữa các tọa độ trong khi vẫn bảo toàn$\sum k c_k$. 

Chúng tôi duy trì bất biến rằng danh sách liên kết liệt kê chính xác tất cả các chỉ số$k$với$c_k>0$theo thứ tự tăng dần. Chúng tôi cũng duy trì quy ước ngầm rằng phân vùng hiện tại luôn hợp lệ. 

Cho phép$q$biểu thị chỉ số lớn nhất sao cho$c_q>0$. Điều này có được bằng cách theo chuỗi từ$l_0$cho đến khi$0$. 

Quá trình chuyển đổi theo thứ tự colex sẽ sửa đổi phân vùng bằng cách cố gắng tăng tọa độ lớn nhất có thể có trong vectơ$(c_n,\dots,c_1)$, tương ứng với việc cố gắng tăng$c_q$khả thi lớn nhất$q$và nếu điều này là không thể, hãy truyền hiệu ứng đến các chỉ số nhỏ hơn. Trong cấu trúc phân vùng, điều này được thực hiện bằng cách chuyển đổi liên tục một phần kích thước$x$thành các phần nhỏ hơn cho đến khi có thể tăng hợp pháp ở vị trí ứng cử viên tiếp theo. 

Do đó, thuật toán được điều khiển bằng cách quét giảm dần trên các kích thước bộ phận được sử dụng, được triển khai thông qua mảng liên kết, kết hợp với phép biến đổi cục bộ thay thế một lần xuất hiện của kích thước bộ phận bằng cấu hình “tách” chuẩn. 

Bây giờ chúng tôi trình bày thuật toán. 

Khởi tạo phân vùng cho$n$: bộ$c_1=n$Và$c_k=0$vì$k>1$. Cấu trúc liên kết là$l_0=1$Và$l_1=0$. 

Ở mỗi bước, phân vùng hiện tại được truy cập. 

Cho phép$q$là chỉ số lớn nhất với$c_q>0$, thu được bằng cách duyệt qua các liên kết từ$0$. 

Nếu như$q=1$, phân vùng là$1+1+\cdots+1$và không thể sửa đổi thêm nữa nên thuật toán kết thúc. 

Nếu không thì$q>1$và một lần xuất hiện kích thước phần$q$được chọn để sửa đổi, vì vậy$c_q\leftarrow c_q-1$. Nếu điều này làm$c_q=0$, liên kết tiền nhiệm được cập nhật để phần tử trước đó trong chuỗi bây giờ trỏ đến$l_q$. 

Phần kích thước bị loại bỏ$q$đóng góp tổng trọng lượng$q$. Phân vùng tiếp theo theo thứ tự colex có được bằng cách chuyển đổi khối lượng này thành mức tăng tối đa cho phép trong vectơ$(c_n,\dots,c_1)$, đạt được bằng cách tăng tọa độ lớn nhất có thể trong khi vẫn duy trì tính khả thi của trọng lượng còn lại. Tọa độ tối đa có thể tăng lên là$c_{q-1}$, bởi vì tăng bất kỳ$c_k$với$k\ge q$sẽ vượt quá trọng lượng có sẵn khi các ràng buộc colex được thực thi, trong khi tất cả các tọa độ lớn hơn đều bằng 0 hoặc được cố định bởi mức tối đa của$q$. 

Do đó chúng tôi cố gắng đặt khối lượng đơn vị bị loại bỏ vào$c_{q-1}$, cài đặt$c_{q-1}\leftarrow c_{q-1}+1.$Điều này chiếm một đơn vị trọng lượng bị loại bỏ$q$, để lại trọng lượng dư$q-1$phải được phân chia thành các phần không vượt quá$q-1$. 

Trọng lượng còn lại$q-1$được lấp đầy một cách tham lam bằng cách chuyển đổi nó thành những số một, đây là sự hoàn thành khả thi nhỏ nhất về mặt từ điển theo thứ tự đảo ngược. Điều này đạt được bằng cách thiết lập$c_1\leftarrow c_1+(q-1).$Việc sửa đổi kết quả sẽ bảo toàn tổng trọng lượng vì sự thay đổi về đóng góp là$-(q) + (q-1)\cdot 1 + 1\cdot (q-1)=0,$trong đó số hạng thứ hai chiếm phần kích thước mới$q-1$và số hạng thứ ba chiếm những số hạng được thêm vào. 

Cuối cùng, cấu trúc liên kết được cập nhật: nếu$q-1>1$và trước đó đã vắng mặt, nó sẽ được chèn ngay sau$l_0$hoặc sau người tiền nhiệm của$q$tùy thuộc vào việc nó đã tồn tại hay chưa; nếu như$c_1$trở nên dương và trước đó bằng 0, nó được thêm vào cuối chuỗi. 

Sau lần sửa chữa này, vectơ kết quả lại thỏa mãn điều kiện phân vùng và tương ứng với phần tử tiếp theo theo thứ tự colex vì sửa đổi làm tăng tọa độ sớm nhất có thể có trong chuỗi đảo ngược trong khi buộc tất cả các tọa độ quan trọng hơn vẫn ở mức tối thiểu. 

Tính chính xác xuất phát từ việc mô tả đặc điểm của thứ tự colex là thứ tự từ điển của$c_n,\dots,c_1$. Ở mỗi bước, thuật toán sửa đổi tọa độ khả thi ngoài cùng bên phải theo thứ tự này và việc phân phối lại trọng số đã loại bỏ thành phần hoàn thành nhỏ nhất về mặt từ điển đảm bảo rằng không có phân vùng hợp lệ trung gian nào có thể nằm giữa phân vùng hiện tại và phân vùng được tạo theo thứ tự colex. Cấu trúc liên kết đảm bảo rằng việc xác định$q$và việc duy trì tính liền kề của tọa độ khác 0 đòi hỏi thời gian tỷ lệ thuận với số lượng chỉ số bị ảnh hưởng. 

Điều này hoàn thành việc xây dựng thuật toán cần thiết. ∎
