---
title: "CF 2034H - Rayan đấu với Rayaneh"
description: "Chúng ta được cho một tập hợp các số nguyên dương phân biệt. Chúng tôi muốn tập hợp con lớn nhất có thuộc tính là không có số được chọn nào có thể được biểu thị dưới dạng tổ hợp tuyến tính nguyên của các số được chọn còn lại. Đối với số nguyên, nhóm con được tạo bởi tập hợp số rất đơn giản."
date: "2026-06-08T11:39:57+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2034
codeforces_index: "H"
codeforces_contest_name: "Rayan Programming Contest 2024 - Selection (Codeforces Round 989, Div. 1 + Div. 2)"
rating: 3300
weight: 2034
solve_time_s: 151
verified: false
draft: false
---

[CF 2034H - Rayan vs. Rayaneh](https://codeforces.com/problemset/problem/2034/H) 

**Đánh giá:** 3300 
**Tags:** vũ lực, dfs và tương tự, dp, lý thuyết số 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên dương phân biệt. Chúng tôi muốn tập hợp con lớn nhất có thuộc tính là không có số được chọn nào có thể được biểu thị dưới dạng tổ hợp tuyến tính nguyên của các số được chọn còn lại. 

Đối với số nguyên, nhóm con được tạo bởi tập hợp số rất đơn giản. Nếu một tập hợp các số nguyên có gcd bằng$g$, thì mọi tổ hợp tuyến tính số nguyên chính xác là bội số của$g$. Điều này ngay lập tức biến bài toán thành bài toán gcd thay vì bài toán đại số tuyến tính. 

Giả sử chúng ta chọn một tập hợp con$S$, và để$$g = \gcd(S).$$Đối với một phần tử$x \in S$, cho phép$$d_x = \gcd(S \setminus \{x\}).$$phần tử$x$thuộc về khoảng nguyên của các phần tử khác khi và chỉ khi$d_x \mid x$. Từ$$\gcd(x,d_x)=g,$$điều kiện trở thành$$d_x > g.$$Do đó, một tập hợp con là hợp lệ khi và chỉ nếu mọi phần tử đều thỏa mãn$$\gcd(S\setminus\{x\}) > \gcd(S).$$Đó là quan sát quan trọng đầu tiên. 

Các ràng buộc lớn về số lượng phần tử đầu vào. Một bài kiểm tra có thể chứa$10^5$giá trị và tổng kích thước trên tất cả các trường hợp thử nghiệm là$3\cdot10^6$. Bất kỳ thuật toán nào cố gắng kiểm tra trực tiếp các tập hợp con lớn đều vô vọng. Chúng ta cần khai thác thực tế là mọi giá trị nhiều nhất là$10^5$. 

Trường hợp cạnh nguy hiểm nhất là giả định tính độc lập tuyến tính thông thường đối với số thực. Ví dụ:```
2 3 6
```Các số độc lập tuyến tính với$\mathbb R$, nhưng không dưới sự kết hợp tuyến tính số nguyên vì$$6 = 6\cdot2 - 2\cdot3.$$Câu trả lời đúng có kích thước$2$. 

Một lỗi dễ mắc phải khác là quên so sánh với gcd của toàn bộ tập hợp con. Coi như```
4 6
```Gcd của tập hợp con là$2$. Loại bỏ một trong hai phần tử sẽ để lại gcd$4$hoặc$6$, cả hai đều lớn hơn$2$, vậy cặp này hợp lệ. 

Cạm bẫy thứ ba là xử lý số$1$. Nếu một phần tử chuẩn hóa bằng$1$, nó không chứa thừa số nguyên tố nào cả. Những yếu tố như vậy hoạt động rất khác nhau trong cấu trúc nhân chứng được mô tả dưới đây và phải được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê tất cả các tập hợp con và kiểm tra điều kiện$$\gcd(S\setminus\{x\}) > \gcd(S)$$cho mọi phần tử được chọn. 

Điều này đúng vì nó suy ra trực tiếp từ đặc tính trên. Thật không may, ngay cả đối với$n=40$, số lượng tập hợp con đã là khoảng$10^{12}$. Với$n$đạt tới$10^5$, vũ lực là hoàn toàn không thể. 

Quan sát quan trọng đến từ việc chia mọi số đã chọn cho gcd của toàn bộ tập hợp con. 

Cho phép$$b_i=\frac{a_i}{g}, \qquad \gcd(b_1,\dots,b_k)=1.$$Điều kiện hợp lệ trở thành$$\gcd(b_1,\dots,b_{i-1},b_{i+1},\dots,b_k)>1$$cho mọi$i$. 

Điều này có nghĩa là sau khi loại bỏ$b_i$, tồn tại một số nguyên tố$p$chia hết các phần tử còn lại. Vì gcd của tất cả các số được chuẩn hóa là$1$, cùng số nguyên tố đó không thể chia được$b_i$. 

Vì vậy, mọi phần tử được chọn đều phải có **số nguyên tố chứng kiến**:```
the witness prime divides every other selected element
and divides none of this element.
```Các số nguyên tố chứng kiến ​​sẽ tự động phân biệt. Nếu cùng một số nguyên tố chứng kiến ​​hai phần tử khác nhau thì số nguyên tố đó sẽ phải vắng mặt ở cả hai phần tử đó, điều này là không thể. 

Bây giờ hãy xem một tập hợp con hợp lệ với các số nguyên tố chứng kiến$$p_1,p_2,\dots,p_k.$$Yếu tố được chứng kiến ​​bởi$p_i$phải chia hết cho mọi số nguyên tố chứng kiến ​​ngoại trừ$p_i$. Do đó mỗi phần tử chuẩn hóa có thể chia hết cho ít nhất$$\prod_{j\ne i} p_j.$$Vì mọi giá trị lớn nhất là$10^5$,$$2\cdot3\cdot5\cdot7\cdot11\cdot13=30030,$$trong khi$$2\cdot3\cdot5\cdot7\cdot11\cdot13\cdot17=510510>10^5.$$Vì vậy, một tập hợp con hợp lệ có thể chứa tối đa bảy phần tử. 

Ràng buộc đó thay đổi toàn bộ vấn đề. Câu trả lời không bao giờ lớn hơn bảy. Một khi chúng ta biết điều đó, một DP cấp nhỏ phụ trách các nhiệm vụ nhân chứng chính sẽ trở nên khả thi. 

Giải pháp được chấp nhận xây dựng cấu trúc nhân chứng ứng cử viên bằng cách sử dụng hệ số của các số lên đến$10^5$, thực hiện DP trên mặt nạ của các số nguyên tố chứng kiến ​​và xây dựng lại một tập hợp con hợp lệ tối đa. Giới hạn câu trả lời nhỏ là điều làm cho việc tìm kiếm có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(V \log V + \text{DP})$Ở đâu$V=10^5$|$O(V)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Viết lại điều kiện độc lập 

Đối với một tập hợp con được chọn$S$, cho phép$$g=\gcd(S).$$Đối với mỗi phần tử$x$,$$x \text{ is generated by the others} \iff \gcd(S\setminus\{x\}) \mid x.$$sử dụng$$\gcd(x,\gcd(S\setminus\{x\}))=g,$$điều này trở thành$$\gcd(S\setminus\{x\})>g.$$### 2. Chuẩn hóa theo gcd toàn cục 

Chia mọi số đã chọn cho$g$. 

Các số chuẩn hóa có gcd bằng$1$. 

Bây giờ điều kiện là$$\gcd(\text{all normalized numbers except }i)>1.$$### 3. Giới thiệu số nguyên tố chứng kiến 

Đối với mọi phần tử được chọn$b_i$, chọn số nguyên tố$p_i$chia tất cả các số chuẩn hóa khác. 

Vì gcd của tất cả các số được chuẩn hóa là$1$, số nguyên tố đó không chia hết$b_i$. 

Do đó, mỗi phần tử có một số nguyên tố chứng kiến ​​chỉ vắng mặt ở phần tử đó. 

### 4. Chứng minh đáp án có nhiều nhất là bảy 

Số nguyên tố chứng kiến là khác biệt. 

Yếu tố được chứng kiến ​​bởi$p_i$phải chứa tất cả các số nguyên tố chứng kiến ​​ngoại trừ$p_i$. 

Do đó mọi phần tử chuẩn hóa đều chia hết cho ít nhất là tích của$k-1$số nguyên tố riêng biệt. 

Tích của sáu số nguyên tố nhỏ nhất là$30030$, trong khi tích của bảy số nguyên tố nhỏ nhất đã vượt quá$10^5$. 

Vì thế$$k \le 7.$$### 5. Bù
