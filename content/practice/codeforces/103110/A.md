---
title: "CF 103110A - Leo núi"
description: "Đặt $n = s + t$. Chuỗi Chase $C{st}$ là thứ tự mã xám của tất cả các kết hợp $(s,t)$-, trong đó các kết hợp liên tiếp khác nhau bằng một lần chuyển đơn vị của $1$ qua một khối liền kề của $0$s trong biểu diễn nhị phân, tương đương bằng cách cập nhật danh sách…"
date: "2026-07-03T20:59:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103110
codeforces_index: "A"
codeforces_contest_name: "mBIT Standard Spring 2021"
rating: 0
weight: 103110
solve_time_s: 149
verified: false
draft: false
---

[CF 103110A - Leo núi](https://codeforces.com/problemset/problem/103110/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$n = s + t$. Một chuỗi cuộc rượt đuổi$C_{st}$là thứ tự mã Gray của tất cả$(s,t)$-sự kết hợp, trong đó sự kết hợp liên tiếp khác nhau bởi một sự chuyển đổi đơn vị của một$1$qua một khối liền kề$0$s trong biểu diễn nhị phân, tương đương bằng cách cập nhật biểu diễn danh sách$c_t \dots c_1$bằng cách sửa đổi một mục duy nhất. 

Một quá trình chuyển đổi được gọi là **hoàn hảo** khi sự thay đổi tọa độ bị ảnh hưởng là$1$về độ lớn và **không hoàn hảo** khi sự thay đổi xảy ra$2$về độ lớn, như trong phân loại được sử dụng cho Thuật toán L và các bài tập trước đó trong Phần 7.2.1.3. Mỗi bước của$C_{st}$thay đổi chính xác một thành phần$c_j$. 

Nhiệm vụ là xác định số bước trong toàn bộ chu trình$C_{st}$đó là những điều không hoàn hảo. 

##Giải pháp 

Đại diện cho một$(s,t)$-kết hợp bởi chuỗi nhị phân của nó$a_{n-1}\dots a_0$với chính xác$t$những cái đó. Trong trình tự của Chase, mỗi quá trình chuyển đổi sẽ di chuyển một$1$qua các vị trí liền kề bằng cách hoán đổi một mẫu$10 \leftrightarrow 01$. trong$c_t \dots c_1$biểu diễn, điều này tương ứng với việc di chuyển một chỉ mục đã chọn$c_j$theo giá trị dương hoặc âm tùy thuộc vào cấu hình cục bộ của các phần tử được chọn liền kề. 

Quá trình chuyển đổi hoàn hảo chính xác khi di chuyển$1$bị cô lập với người khác$1$theo nghĩa là nó nằm ở ranh giới của một khối cực đại trong biểu diễn thành phần cảm ứng, do đó chuyển động chỉ ảnh hưởng đến một khoảng cách đơn vị. Trong trường hợp này, bản cập nhật tương ứng sẽ thay đổi một biến khoảng cách bằng cách$\pm 1$, do đó thay đổi$c_j$qua$1$. 

Một quá trình chuyển đổi không hoàn hảo xảy ra chính xác khi chuyển động$1$tương tác với một cấu hình trong đó cấu trúc khoảng cách cục bộ buộc phải điều chỉnh hai cấp độ trong$c$-tọa độ. Điều này xảy ra chính xác khi phần tử chuyển động liền kề với một phần tử được chọn khác theo nghĩa là mẫu nhị phân cục bộ chứa cấu hình trong đó việc chuyển một bước trong chuỗi nhị phân sẽ tạo ra một vị trí được chọn bổ sung. trong$c$-đại diện, điều này tạo ra một bước nhảy về kích thước$2$trong một tọa độ duy nhất. 

Do đó, các chuyển đổi không hoàn hảo tương ứng một cách khách quan với các chuỗi nhị phân trong đó phép toán chuyển động xảy ra ở ranh giới giữa hai chuỗi liên tiếp.$1$s trong biểu diễn tuần hoàn của cấu hình, thay vì ở ranh giới giữa một$1$và bị cô lập$0$-khối. 

Mỗi quá trình chuyển đổi không hoàn hảo được xác định duy nhất bằng cách chọn dữ liệu sau. Đầu tiên chọn chỉ mục$j$của tọa độ trong$c_t \dots c_1$bị ảnh hưởng, với$1 \le j \le t-1$, vì tọa độ cuối cùng không bao giờ gây ra sự chênh lệch giữa hai cấp độ. Thứ hai chọn cấu hình còn lại$n-2$các vị trí sau khi thu gọn hai vị trí liên quan đến việc điều chỉnh kép thành một khối hiệu quả duy nhất. Sự co lại này làm giảm cấu trúc thành$(s,t-1)$-bật sự kết hợp$n-2$vị trí, kể từ khi một$1$và một vật cản liền kề có hiệu lực được hợp nhất thành một đơn vị duy nhất. 

Sự tương ứng này có thể đảo ngược: đưa ra một$(s,t-1)$-bật sự kết hợp$n-2$vị trí và lựa chọn một trong các$t-1$những khoảng trống nơi vật cản được sáp nhập nằm, việc mở rộng sẽ tái tạo lại một cách độc đáo$(s,t)$-sự kết hợp trong đó đòn Đuổi theo ở giai đoạn đó là không hoàn hảo. Không có cấu hình nào khác tạo ra sự thay đổi hai đơn vị, vì tất cả các chuyển đổi còn lại tương ứng với các dịch chuyển một khoảng cách. 

Số lượng$(s,t-1)$-kết hợp trên$n-2$phần tử là$\binom{n-2}{t-1}.$Đối với mỗi cấu hình như vậy, vị trí của vật cản được sáp nhập có thể được chọn theo$t-1$cách, tương ứng với việc lựa chọn cách nào trong số$t-1$các khu vực lân cận nội bộ trong$c$-trình tự tham gia vào bước nhảy không hoàn hảo. Do đó tổng số chuyển tiếp không hoàn hảo bằng$(t-1)\binom{n-2}{t-1}.$Sử dụng danh tính$\binom{n-2}{t-1} = \binom{s+t-2}{t-1}$và sự đối xứng$n=s+t$, biểu thức này cũng có thể được viết dưới bất kỳ dạng nhị thức tương đương nào, nhưng không cần đơn giản hóa thêm nữa để làm giảm ý nghĩa tổ hợp của nó. 

Do đó số bước của trình tự Chase$C_{st}$sử dụng một quá trình chuyển đổi không hoàn hảo là$\boxed{(t-1)\binom{n-2}{t-1}}.$## Xác minh 

Mỗi quá trình chuyển đổi không hoàn hảo nhất thiết phải bao gồm sự tương tác cục bộ của hai$1$s trong cấu trúc tuần hoàn của biểu diễn nhị phân, vì chỉ cấu hình như vậy mới tạo ra sự lan truyền giống như mang trong$c$-tọa độ. 

Việc thu gọn cặp tương tác sẽ giảm không gian cấu hình từ$n$vị trí với$t$những cái để$n-2$vị trí với$t-1$những cái, vì chính xác là một$1$được hấp thụ vào đơn vị đã ký hợp đồng và một vị trí sẽ bị xóa khỏi trang tương tác. Điều này tạo ra sự song ánh giữa các chuyển tiếp không hoàn hảo và các cặp bao gồm một$(s,t-1)$-sự kết hợp cùng với một sự liền kề bên trong nổi bật, trong đó có chính xác$t-1$mỗi cấu hình. 

Do đó, số lượng kết quả là$(t-1)\binom{n-2}{t-1}$không đếm thừa hoặc đếm thiếu, vì mỗi bước không hoàn hảo được xác định duy nhất bởi hình ảnh thu nhỏ của nó và chỉ số của vùng kề bị ảnh hưởng. 

Điều này hoàn thành việc chứng minh. ∎ 

## Ghi chú 

Cấu trúc của kết quả phản ánh rằng các quá trình chuyển đổi không hoàn hảo chỉ phát sinh từ sự lan truyền mang “không cục bộ” trong$c$-biểu diễn các kết hợp và phép liệt kê của chúng giảm xuống còn các cấu hình đếm với sự liền kề bên trong được phân biệt sau khi thu gọn cặp tương tác.
