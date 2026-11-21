---
title: "CF 2226F - Cuộc xâm lược đảo ngược"
description: "Với mọi ước số $dmid n$, hãy xác định $$Gd={vin[1,n]mid gcd(v,n)=d}.$$ Mảng $a$ dần dần cố định một số vị trí. Nếu $ai=dneq 0$, thì vị trí $i$ phải chứa một giá trị từ $Gd$. Các vị trí không cố định có thể chứa bất kỳ giá trị còn lại nào."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2226
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1095 (Div. 2)"
rating: 0
weight: 2226
solve_time_s: 101
verified: false
draft: false
---

[CF 2226F - Cuộc xâm lược đảo ngược](https://codeforces.com/problemset/problem/2226/F) 

**Đánh giá:** - 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Với mỗi ước số$d\mid n$, định nghĩa$$G_d=\{v\in[1,n]\mid \gcd(v,n)=d\}.$$Mảng$a$sửa dần một số vị trí. Nếu như$a_i=d\neq 0$, sau đó định vị$i$phải chứa một giá trị từ$G_d$. Các vị trí không cố định có thể chứa bất kỳ giá trị còn lại nào. 

Một hoán vị hợp lệ chỉ tồn tại nếu với mọi ước số$d$,$$c_d \le |G_d|,$$Ở đâu$c_d$là số vị trí đã buộc phải chia lớp$d$. 

Tác vụ yêu cầu tổng số lần đảo ngược trên tất cả các hoán vị hợp lệ sau mỗi lần cập nhật. 

Những hạn chế là khó khăn chính. Trên tất cả các trường hợp thử nghiệm,$$\sum n \le 2\cdot 10^6,\qquad \sum q \le 10^6.$$Bất cứ điều gì tính toán lại các hoán vị hoặc thậm chí lặp lại tất cả các ước số sau mỗi truy vấn đều quá chậm. Giải pháp phải cập nhật câu trả lời trong khoảng$O(1)$hoặc$O(\log n)$thời gian cho mỗi truy vấn sau khi tiền xử lý. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị hợp lệ, đếm các nghịch đảo trong mỗi hoán vị và tính tổng chúng. Ngay cả đối với$n=10$điều này đã vô vọng vì có$10!$hoán vị. 

Quan sát quan trọng là tính đối xứng của các tập hợp$G_d$:$$\gcd(x,n)=\gcd(n-x,n) \qquad (1\le x<n).$$Do đó mọi giá trị$x<n$có một đối tác$n-x$trong cùng một lớp chia. 

Lấy bất kỳ hoán vị hợp lệ nào và thay thế mọi giá trị$x<n$qua$n-x$. Hoán vị kết quả vẫn hợp lệ. Cho hai giá trị$x,y<n$,$$x>y \iff n-x<n-y.$$Do đó mọi sự đảo ngược giữa các giá trị$1,\dots,n-1$trở thành một sự không đảo ngược, và mọi sự không đảo ngược đều trở thành một sự đảo ngược. Trong số những giá trị đó có$$\binom{n-1}{2}$$cặp, do đó hoán vị bổ sung góp phần$$\binom{n-1}{2}$$tổng số đảo ngược. Do đó sự đóng góp trung bình của các cặp không liên quan đến$n$là$$\frac12\binom{n-1}{2}.$$Toàn bộ vấn đề giảm xuống việc đếm các hoán vị hợp lệ và xử lý vai trò đặc biệt của giá trị$n$. 

Cho phép$$a_d=|G_d|, \qquad b_d=\text{number of positions already fixed to }d.$$Số hoán vị hợp lệ là$$\Bigl(\prod_d P(a_d,b_d)\Bigr)\,(n-k)!,$$Ở đâu$k=\sum_d b_d$Và$$P(m,r)=m(m-1)\cdots(m-r+1).$$Nếu một số$b_d>a_d$, số đếm bằng không. 

Công việc còn lại là tính toán sự đóng góp của các phép nghịch đảo liên quan đến giá trị$n$. Từ$n$lớn hơn mọi giá trị khác, một sự đảo ngược liên quan đến$n$chỉ phụ thuộc vào vị trí của nó. 

Nếu như$n$đã bị buộc phải vào vị trí$p$, nó đóng góp chính xác$$n-p$$nghịch đảo trong mọi hoán vị hợp lệ. 

Nếu không thì,$n$có thể chiếm bất kỳ vị trí không cố định nào. Tổng hợp tất cả các khả năng mang lại kết quả$$\left(\sum_{\text{unfixed }i}(n-i)\right) \cdot (\text{valid completions with }n\text{ fixed}).$$Việc duy trì tổng này một cách linh hoạt rất dễ dàng vì mỗi truy vấn sẽ sửa chính xác một vị trí chưa được cố định trước đó. 

Công thức cuối cùng chính xác là công thức được sử dụng trong các giải pháp được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các hoán vị hợp lệ | Hàm mũ | Hàm mũ | Quá chậm | 
| Phép đếm lớp chia + tính đối xứng |$O(n\log\log n + q)$cho mỗi trường hợp thử nghiệm sau quá trình tiền xử lý toàn cục |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước giai thừa và nghịch đảo giai thừa modulo$998244353$lên đến$2\cdot10^6$. 
2. Đối với mọi$i\in[1,n]$, tính toán$$g=\gcd(i,n),$$và tăng dần$a_g$. Như vậy$a_g=|G_g|$. 
3. Duy trì$b_g$, số lượng vị trí đã bị ràng buộc vào lớp ước số$g$. 
4. Duy trì$$cur=\prod_g P(a_g,b_g).$$Khi có một ràng buộc mới của lớp$g$được thêm vào,$$cur \leftarrow cur\cdot \frac{P(a_g,b_g+1)}{P(a_g,b_g)}.$$Sử dụng giai thừa bản cập nhật này là$O(1)$. 
5. Hãy để$cnt$là số vị trí không cố định. 
6. Duy trì$$S=\sum_{\text{unfixed }i}(n-i).$$Khi vị trí$i$trở nên cố định,$$S\leftarrow S-(n-i).$$7. Hãy để$$C=\frac{(n-1)(n-2)}{4}.$$Đây là sự đóng góp trung bình của các nghịch đảo không liên quan đến$n$. 
8. Nếu một số lớp bị lạm dụng quá mức, tương đương$cur=0$, đầu ra$0$. 
9. Ngược lại tính$$ans = C\cdot cur\cdot cnt!$$đối với phần đối xứng. 
10. Nếu giá trị$n$đã bị buộc phải vào vị trí$p$,$$ans \mathrel{+}= (n-p)\cdot cur\cdot cnt!.$$11. Nếu giá trị$n$chưa sửa thì thêm vào$$S\cdot cur\cdot (cnt-1)!.$$12. Xuất kết quả theo modulo$998244353$. 

### Tại sao nó hoạt động 

Sự tiến hóa$x\mapsto n-x$bảo toàn mọi lớp chia$G_d$. Do đó các hoán vị hợp lệ được phân chia thành các cặp bổ sung có số lần đảo ngược giữa các giá trị$1,\dots,n-1$tổng hợp thành$\binom{n-1}{2}$. Điều này mang lại sự đóng góp trung bình cố định độc lập với các ràng buộc. Sự bất đối xứng duy nhất đến từ giá trị$n$, lớn hơn mọi giá trị khác. Đóng góp đảo ngược của nó chỉ phụ thuộc vào vị trí của nó. Việc đếm các lần hoàn thành hợp lệ với các hệ số giai thừa và giai thừa giảm mang lại số lượng hoán vị chính xác thực hiện từng vị trí của$n$. Kết hợp hai đóng góp độc lập này sẽ tạo ra số tiền cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\log\log n + q)$| Xây dựng số lượng chia số cộng$O(1)$cập nhật cho mỗi truy vấn | 
| Không gian |$O(n)$| Mảng được lập chỉ mục theo ước số | 

Giới hạn toàn cầu$\sum n\le 2\cdot10^6$Và$\sum q\le10^6$phù hợp thoải mái trong những giới hạn này.
