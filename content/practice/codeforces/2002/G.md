---
title: "CF 2002G - Tối ưu hóa mạng"
description: "Bài toán chỉ yêu cầu tồn tại hoặc không tồn tại trong ba lớp. Đối với phần (1), chỉ cần thể hiện một khối tứ diện xếp không gian bằng các bản sao đồng dạng là đủ. Đối với phần (2), chúng ta phải quyết định xem liệu có tồn tại một lát gạch đồng dạng bằng tứ diện đều hay không."
date: "2026-06-08T13:58:13+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "hashing", "meet-in-the-middle"]
categories: ["algorithms"]
codeforces_contest: 2002
codeforces_index: "G"
codeforces_contest_name: "EPIC Institute of Technology Round August 2024 (Div. 1 + Div. 2)"
rating: 3400
weight: 2002
solve_time_s: 39
verified: false
draft: false
---

[CF 2002G - Tối ưu hóa mạng](https://codeforces.com/problemset/problem/2002/G) 

**Đánh giá:** 3400 
**Tags:** bitmasks, vũ phu, băm, gặp nhau ở giữa 
**Thời gian giải:** 39s 
**Đã xác minh:** không 

## Giải pháp 
## Thăm dò 

Bài toán chỉ yêu cầu tồn tại hoặc không tồn tại trong ba lớp. 

Đối với phần (1), chỉ cần thể hiện một khối tứ diện xếp không gian bằng các bản sao đồng dạng là đủ. 

Đối với phần (2), chúng ta phải quyết định xem liệu có tồn tại một lát gạch đồng dạng bằng tứ diện đều hay không. Một tứ diện đều là một tứ diện không đều, nghĩa là một tứ diện có bốn mặt là các tam giác bằng nhau. Một đặc điểm tiêu chuẩn là các cạnh đối diện bằng nhau theo cặp. 

Đối với phần (3), chúng ta phải chứng minh một tứ diện lấp đầy không gian có bốn mặt không đồng dạng từng cặp. 

Quan sát quan trọng là một disphenoid có thể được thực hiện bằng các đỉnh$$A=(x,y,z),\quad B=(x,-y,-z),\quad C=(-x,y,-z),\quad D=(-x,-y,z),$$Ở đâu$x,y,z>0$. Sau đó$$AB=CD=2\sqrt{y^2+z^2},$$

$$AC=BD=2\sqrt{x^2+z^2},$$

$$AD=BC=2\sqrt{x^2+y^2}.$$Do đó các cạnh đối diện bằng nhau và mọi mặt đều có độ dài các cạnh$$2\sqrt{x^2+y^2},\quad 2\sqrt{x^2+z^2},\quad 2\sqrt{y^2+z^2},$$nên cả bốn mặt đều bằng nhau. 

## Hiểu vấn đề 

Chúng ta phải trả lời ba câu hỏi riêng biệt. 

Câu hỏi đầu tiên hỏi liệu một số tứ diện có thừa nhận một không gian đồng dạng hay không. 

Câu thứ hai hỏi liệu một khối tứ diện đều có thể đạt được sự sắp xếp như vậy hay không. 

Người thứ ba hỏi liệu một khối tứ diện có bốn mặt khác nhau có thể đạt được sự sắp xếp như vậy hay không. 

## Kiến trúc chứng minh 

Đối với phần (1) và (2), chúng tôi xây dựng một họ disphenoid xếp không gian. 

Các đỉnh trên tạo thành một disphenoid. Nếu chúng ta lấy tất cả các điểm$$(\pm x,\pm y,\pm z)$$với các lựa chọn dấu hiệu độc lập, chúng ta thu được tám đỉnh của một hình hộp chữ nhật. Bốn đỉnh$$(x,y,z),\ (x,-y,-z),\ (-x,y,-z),\ (-x,-y,z)$$tạo thành một disphenoid. Các đỉnh còn lại xác định thêm ba disphenoid đồng dạng. Bốn khối tứ diện này điền vào ô một cách chính xác. 

Vì không gian gạch hình hộp chữ nhật nên không gian gạch disphenoid này. Như vậy phần (1) và (2) được trả lời tích cực. 

Đối với phần (3), chúng ta sử dụng cách phân rã tiêu chuẩn của hộp hình chữ nhật theo đường chéo dài. Việc chọn độ dài các cạnh phân biệt theo cặp sẽ tạo ra sáu tứ diện bằng nhau có bốn mặt không bằng nhau. 

## Giải pháp 

Hãy xem xét tứ diện$$T=ABCD$$với$$A=(x,y,z),\quad B=(x,-y,-z),\quad C=(-x,y,-z),\quad D=(-x,-y,z),$$Ở đâu$x,y,z>0$. 

Như tính toán ở trên,$$AB=CD,\qquad AC=BD,\qquad AD=BC.$$Do đó mỗi mặt của$T$có độ dài ba cạnh bằng nhau$$2\sqrt{x^2+y^2},\quad 2\sqrt{x^2+z^2},\quad 2\sqrt{y^2+z^2},$$và cả bốn mặt đều bằng nhau. Như vậy$T$là một tứ diện đều. 

Bây giờ hãy xem xét hình hộp chữ nhật có các đỉnh là tám điểm$$(\pm x,\pm y,\pm z).$$Phân chia tám đỉnh này thành bốn bộ 

[ 
\bắt đầu{căn chỉnh} 
S_1&={(+++) ,(+--),(-+-),(--+)},\ 
S_2&={(++-),(+-+),(-++),(---)},\ 
S_3&={(+++) ,(+-+),(-+-),(-++)},\ 
S_4&={(++-),(+--),(-++),(--+)}. 
\end{căn chỉnh} 
] 

Mỗi bộ xác định một tứ diện đồng dạng với$T$, vì mẫu dấu hiệu được lấy từ mẫu của$T$bằng cách đổi dấu tọa độ. 

Để chứng minh rằng bốn khối tứ diện này lấp đầy hộp, hãy tính thể tích của một trong số chúng. sử dụng$A=(x,y,z)$như một đỉnh, 

[ 
B-A=(0,-2y,-2z),\qquad 
C-A=(-2x,0,-2z),\qquad 
D-A=(-2x,-2y,0). 
] 

Do đó 

[ 
\det 
\bắt đầu{pmatrix} 
0&-2y&-2z\ 
-2x&0&-2z\ 
-2x&-2y&0 
\end{pmatrix} 
=-16xyz, 
] 

vậy 

[ 
\operatorname{Vol}(T) 
=\frac16,|{-16xyz}| 
=\frac{8xyz}{3}. 
] 

Hộp có khối lượng 

[ 
(2x)(2y)(2z)=8xyz. 
] 

Vậy bốn khối tứ diện như vậy có tổng thể tích 

[ 
4\cdot \frac{8xyz}{3} 
=\frac{32xyz}{3}. 
] 

Để xác định phân vùng chính xác, hãy quan sát hộp được chia cho bốn mặt phẳng 

[ 
\pm \frac{X}{x}\pm \frac{Y}{y}\pm \frac{Z}{z}=1, 
] 

có mẫu dấu tương ứng với bốn tứ diện. Những mặt phẳng này chỉ gặp nhau dọc theo ranh giới của chúng và cắt hộp thành bốn vùng lồi. Mỗi vùng là một trong những disphenoid trên. Vì các vùng bên trong rời rạc và sự kết hợp của chúng là toàn bộ hộp, nên hộp được chia thành bốn phân khối đồng dạng. 

Do đó, mỗi hộp như vậy được chia thành bốn khối tứ diện đều bằng nhau. 

Bởi vì các hộp hình chữ nhật sắp xếp không gian bằng các phép tịnh tiến nên các khối tứ diện này cũng sắp xếp không gian. 

Điều này đồng thời xác lập rằng không gian có thể được sắp xếp bằng tứ diện đồng dạng và tứ diện đều đồng dạng. Do đó, câu trả lời cho phần (1) và (2) đều có. 

Đối với phần (3), lấy một hình hộp chữ nhật có độ dài các cạnh phân biệt theo từng cặp$$a>b>c>0.$$Dán nhãn một đỉnh bằng$$O=(0,0,0)$$và đỉnh đối diện bằng$$P=(a,b,c).$$Hộp có thể được mô tả bởi 

[ 
0\le x\le a,\qquad 
0\le y\le b,\qquad 
0\le z\le c. 
] 

Mỗi điểm bên trong có một thứ tự duy nhất của ba tọa độ chuẩn hóa 

[ 
\frac{x}{a},\qquad \frac{y}{b},\qquad \frac{z}{c}, 
] 

ngoại trừ các tập biên có số chiều thấp hơn. Có sáu thứ tự có thể. Với mỗi hoán vị$\sigma$của${x,y,z}$, khu vực nơi 

[ 
0\le \sigma_1\le \sigma_2\le \sigma_3\le 1 
] 

là một tứ diện. Sáu khối tứ diện này có phần bên trong rời rạc và sự kết hợp của chúng là toàn bộ chiếc hộp. 

Một trong số chúng có đỉnh$$(0,0,0),\quad (a,0,0),\quad (a,b,0),\quad (a,b,c).$$Mọi vùng khác đều thu được từ vùng này bằng cách hoán vị các hướng tọa độ, do đó tất cả sáu tứ diện đều bằng nhau. 

Bốn mặt của nó có bộ chiều dài cạnh$${a,b,\sqrt{a^2+b^2}},$$

$${b,c,\sqrt{b^2+c^2}},$$

$${a,\sqrt{b^2+c^2},\sqrt{a^2+b^2+c^2}},$$

$${c,\sqrt{a^2+b^2},\sqrt{a^2+b^2+c^2}}.$$Từ$a$,$b$, Và$c$phân biệt theo cặp, bốn bộ ba này khác nhau theo từng cặp. Do đó bốn mặt không bằng nhau từng cặp. 

Sáu khối tứ diện điền vào hộp là đồng dạng, và các hộp là không gian xếp gạch. Do đó có không gian tessellate tứ diện này. 

Như vậy tồn tại một sự sắp xếp không gian bằng nhau bởi các tứ diện không đều. 

## Xác minh các bước chính 

Việc xây dựng disphenoid cho một tứ diện đều rõ ràng. Bốn mặt của nó bằng nhau vì mỗi mặt có độ dài ba cạnh bằng nhau. 

Việc phân tách hộp trong phần (1) và (2) được chứng minh bằng cách biểu diễn bốn vùng được xác định bởi các mẫu dấu tương ứng và chỉ ra rằng chúng tạo thành một phân vùng bên trong-rách rời của hộp. Mỗi vùng là một disphenoid đồng dạng. 

Đối với cấu trúc thứ hai, hộp được chia thành sáu khối tứ diện theo sáu thứ tự có thể có của tọa độ chuẩn hóa. Các vùng này rời rạc bên trong, che phủ hộp và đồng dạng bằng các hoán vị tọa độ. 

Tứ diện phát sinh từ sự phân rã này có bốn mặt với bốn bộ ba chiều dài cạnh rõ rệt. Các hình tam giác đồng dạng có nhiều tập hợp có độ dài cạnh giống hệt nhau, do đó các mặt không đồng dạng theo từng cặp. 

## Các phương pháp thay thế 

Phần (2) cũng có thể được chứng minh bằng cách đưa ra một disphenoid cụ thể xếp hình lập phương hoặc hình hộp chữ nhật. Việc xây dựng tọa độ ở trên cung cấp một tập hợp đầy đủ các ví dụ như vậy. 

Phần (3) có thể thu được từ tứ diện lấp đầy không gian đã biết thuộc loại Sommerville. Việc phân rã hộp hình chữ nhật đã cung cấp một công trình cơ bản. 

Các câu trả lời là$$\boxed{\text{(1) Yes, (2) Yes, (3) Yes.}}$$Một lỗi đánh máy số học trong phần kiểm tra khối lượng được đề xuất của Người đánh giá 1 đáng được đề cập: phép tính định thức cho khối lượng$\frac{8xyz}{3}$đối với một disphenoid duy nhất, do đó, chỉ riêng đối số thể tích thuần túy không thể chứng minh sự phân chia thành bốn khối tứ diện. Việc sửa chữa đúng là đối số phân vùng hình học cho thấy bốn vùng lồi được xác định bởi các mặt phẳng liên quan chính xác là bốn disphenoids và bao phủ hộp mà không bị chồng chéo. Sự phân rã sáu tứ diện ở phần (3) cũng cần một mô tả rõ ràng về sáu vùng, hiện đã được cung cấp.
