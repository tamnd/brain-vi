---
title: "CF 103536B - Rắc rối"
description: "Đặt $(a{n-1},dots,a1,a0)$ là các phần tử của mã Gray phản ánh bậc ba, do đó các bộ dữ liệu liên tiếp khác nhau ở đúng một tọa độ $+1$ hoặc $-1$, với tất cả các mục nhập bằng ${0,1,2}$. Chuỗi đầy đủ chạy qua tất cả các bộ $n$-bộ ba $3^n$."
date: "2026-07-03T05:50:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103536
codeforces_index: "B"
codeforces_contest_name: "classic problems (for e-maxx)"
rating: 0
weight: 103536
solve_time_s: 125
verified: false
draft: false
---

[CF 103536B - Rắc rối](https://codeforces.com/problemset/problem/103536/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$(a_{n-1},\dots,a_1,a_0)$là các phần tử của mã Gray phản ánh bậc ba, do đó các bộ liên tiếp khác nhau ở đúng một tọa độ$+1$hoặc$-1$, với tất cả các mục trong${0,1,2}$. Trình tự đầy đủ chạy qua tất cả$3^n$ba loài$n$-bộ dữ liệu. 

Đặt trạng thái hợp lệ cho phần (a) là một bộ thỏa mãn$a_{n-1}+\cdots+a_1+a_0=t.$Đặt trạng thái hợp lệ cho phần (b) là cấu hình nhiều bộ ba thỏa mãn$\{a_{n-1},\dots,a_0\}=\{r\cdot 0,\; s\cdot 1,\; t\cdot 2\}, \quad r+s+t=n.$Mã màu xám cửa xoay$\Gamma_{st}$trên các kết hợp nhị phân có đặc tính xác định là các trạng thái hợp lệ kế tiếp nhau khác nhau bởi một trao đổi liền kề của$0$Và$1$trong biểu diễn chuỗi bit, do đó bằng một bước di chuyển cục bộ trong cấu trúc tổ hợp tương ứng. 

Câu hỏi đặt ra là liệu các thuộc tính “cửa quay” tương tự có đúng hay không sau khi hạn chế mã Gray phản ánh bậc ba đối với các tập con được mô tả trong (a) và (b). 

##Giải pháp 

Mã Gray phản xạ ba ngôi được xây dựng sao cho các trạng thái đầy đủ liên tiếp khác nhau đúng một tọa độ bởi$\pm 1$. Nếu như$x=(x_{n-1},\dots,x_0)$Và$y=(y_{n-1},\dots,y_0)$liên tiếp trong mã đầy đủ thì tồn tại một chỉ mục duy nhất$j$như vậy$y_j=x_j\pm 1$Và$y_i=x_i$cho tất cả$i\neq j$. 

### Phần (a) 

Hạn chế ở các trạng thái có tổng cố định$t$. Cho phép$x$Và$y$là các phần tử liên tiếp trong mã Gray phản ánh bậc ba đầy đủ mà cả hai đều thỏa mãn$\sum_i x_i=t$Và$\sum_i y_i=t$. 

Từ$y$khác với$x$trong đúng một tọa độ$j$, sự bằng nhau của các tổng dẫn đến mâu thuẫn:$\sum_i y_i-\sum_i x_i = (x_j\pm 1 - x_j)=\pm 1 \neq 0.$Do đó, không có hai phần tử liên tiếp nào của mã Gray phản ánh ba ngôi đầy đủ đều nằm trong tập được xác định bởi$\sum a_i=t$. Do đó, bất kỳ sự chuyển đổi nào giữa các trạng thái hợp lệ đều phải bỏ qua ít nhất một trạng thái không hợp lệ trung gian. 

Cho phép$x$Và$x'$là các trạng thái hợp lệ liên tiếp trong dãy con cảm ứng. Con đường từ$x$ĐẾN$x'$trong mã Gray đầy đủ thay đổi một tọa độ bằng$+1$hoặc$-1$ở mỗi bước và tổng thay đổi ròng sẽ bảo toàn giới hạn tổng. Do đó, hiệu ứng tổng thể là một chuỗi hữu hạn chuyển đơn vị giữa các tọa độ: một số tọa độ tăng theo$1$và những thứ khác giảm đi$1$, với tổng số tăng và số giảm bằng nhau. 

Số tọa độ được thay đổi có thể vượt quá hai, vì mã Gray có thể thực hiện một chuỗi tăng giảm xen kẽ trước khi quay trở lại siêu phẳng$\sum a_i=t$. Không có ràng buộc nào buộc các tọa độ trung gian của lần quay lại đầu tiên chỉ liên quan đến một mức tăng và một mức giảm duy nhất. 

Do đó, sự kề cận cảm ứng trên tập hạn chế không phải là một phép toán cục bộ đơn lẻ tương tự như nút giao cửa quay. Cấu trúc không phải là mã Gray trên biểu đồ kiểu Johnson làm cơ sở cho các kết hợp nhị phân có trọng số cố định. 

### Phần (b) 

Bây giờ hạn chế các bộ dữ liệu có loại nhiều bộ cố định${r\cdot 0,s\cdot 1,t\cdot 2}$. Mỗi bước mã Gray đầy đủ thay đổi chính xác một tọa độ bằng$\pm 1$, do đó, loại nhiều tập hợp chỉ được giữ nguyên nếu thay đổi được bù bằng thay đổi ngược lại sau đó trong cùng tọa độ hoặc bằng thay đổi cân bằng ở nơi khác. 

Giữa hai trạng thái hợp lệ liên tiếp trong dãy con cảm ứng, sự tiến hóa trong mã Gray bậc ba đầy đủ tạo ra một đường dẫn trong đó các ký hiệu được tăng và giảm từng ký hiệu một. Hiệu ứng thực giữa các điểm cuối hợp lệ là một số tọa độ trải qua quá trình chuyển khối lượng đơn vị theo chu kỳ giữa các cấp$0,1,2$và điều này có thể liên quan đến nhiều tọa độ. 

Không giống như trường hợp nhị phân cơ bản của mã Gray cửa quay$\Gamma_{st}$, không có biểu diễn nào trong đó mọi trạng thái hợp lệ đều tương ứng với một tập hợp con có các chuyển đổi được điều chỉnh bởi một hoạt động trao đổi duy nhất bảo toàn cấu trúc kề cận cục bộ. Động lực trung gian liên quan đến việc mang theo cả hai hướng của quá trình chữ số bậc ba và những lần mang này lan truyền qua nhiều vị trí trước khi thành phần hợp lệ của số lượng ký hiệu được khôi phục. 

Do đó, chuỗi cảm ứng trên các hoán vị nhiều tập hợp không tương ứng với đường dẫn Hamilton trong biểu đồ Cayley đơn giản được tạo ra bởi các chuyển vị cục bộ hoặc trao đổi ký hiệu liền kề. Các quá trình chuyển đổi không bị giới hạn ở một hoán đổi cục bộ có giới hạn tương tự như hoạt động cửa quay nhị phân. 

## Xác minh 

Trong phần (a), hạn chế chính là một bước mã Gray sẽ thay đổi tổng bằng$\pm 1$, điều này mâu thuẫn với tính bất biến của ràng buộc tổng. Do đó, không có lân cận một bước nào tồn tại được hạn chế, buộc phải chuyển đổi nhiều bước giữa các trạng thái hợp lệ. 

Trong phần (b), một bước mã Gray duy nhất giữ nguyên tổng số ký hiệu nhưng thay đổi một giá trị ký hiệu, điều này làm thay đổi loại nhiều bộ trừ khi được bù bằng những thay đổi nghịch đảo sau đó. Do đó, hạn chế tạo ra các chuyển đổi bao gồm nhiều cập nhật tọa độ đơn, không thể rút gọn thành một trao đổi tổ hợp duy nhất. 

Do đó, cả hai cấu trúc cảm ứng đều không bảo toàn được đặc tính thuộc tính kề cận một bước của mã Gray cửa quay. 

## Kết luận 

Mã Gray phản ánh bậc ba không giữ lại các thuộc tính liền kề kiểu cửa quay theo một trong hai hạn chế. Trong cả hai trường hợp, chuỗi cảm ứng giữa các trạng thái hợp lệ yêu cầu điều chỉnh tổng hợp nhiều bước thay vì một thao tác trao đổi cục bộ duy nhất, do đó cấu trúc kề cận mã Gray mạnh bị mất. ∎
