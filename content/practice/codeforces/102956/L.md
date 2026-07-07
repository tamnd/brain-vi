---
title: "CF 102956L - Đơn vị bán dẫn dành cho doanh nghiệp"
description: "Cách Mười hai lần phân loại vị trí của các quả bóng $n$ vào các bình $m$ tùy theo các quả bóng và bình được dán nhãn hay không và liệu mỗi bình có bị hạn chế hay không, bắt buộc phải chứa nhiều nhất một quả bóng hay được yêu cầu chứa ít nhất một quả bóng."
date: "2026-07-04T07:12:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102956
codeforces_index: "L"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, Belarusian SU Contest (XXI Open Cup, Grand Prix of Belarus)"
rating: 0
weight: 102956
solve_time_s: 156
verified: false
draft: false
---

[CF 102956L - Đơn vị bán dẫn dành cho doanh nghiệp](https://codeforces.com/problemset/problem/102956/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Cách thứ mười hai phân loại các vị trí của$n$bóng vào$m$bình tùy theo quả bóng và bình được dán nhãn hay không và liệu mỗi bình có không bị hạn chế, bắt buộc phải chứa nhiều nhất một quả bóng hay bắt buộc phải chứa ít nhất một quả bóng. 

Mỗi mục là một bài toán đếm tiêu chuẩn mà câu trả lời của nó phải đúng khi$m=0$hoặc$n=0$. Trong những trường hợp ranh giới đó, các ràng buộc xác định sẽ xác định xem số đếm có$0$hoặc$1$. 

###1.$n$quả bóng được dán nhãn,$m$chiếc bình được dán nhãn 

Mỗi quả bóng độc lập chọn một trong các$m$bình đựng nước, đưa$m$lựa chọn cho mỗi quả bóng. Tổng số chức năng từ một$n$-phần tử được đặt thành một$m$- tập hợp phần tử là do đó$$m^n.$$Khi$m=0$, không có hàm nào tồn tại trừ khi$n=0$, trong trường hợp đó hàm trống đóng góp$1$. Như vậy công thức$0^0=1$là nhất quán trong bối cảnh này. 

###2.$n$quả bóng được dán nhãn,$m$bình được dán nhãn, tối đa một quả bóng cho mỗi bình 

Điều này tính số lần tiêm từ một$n$-phần tử được đặt thành một$m$-bộ phần tử. Nếu như$n>m$, không có mũi tiêm nào tồn tại. Vì$n\le m$, số đó là$$m(m-1)\cdots(m-n+1)=\frac{m!}{(m-n)!}.$$Nếu như$n=0$, có chính xác một lần tiêm trống, vì vậy giá trị là$1$cho tất cả$m$. Nếu như$m=0$, giá trị là$1$khi$n=0$Và$0$nếu không thì. 

###3.$n$quả bóng được dán nhãn,$m$bình được dán nhãn, ít nhất một quả bóng cho mỗi bình 

Điều này tính các dự đoán từ một$n$-đặt lên một$m$-bộ. Số được đưa ra bằng cách bao gồm-loại trừ:$$\sum_{k=0}^m (-1)^k \binom{m}{k}(m-k)^n.$$Khi$m=0$, điều này bằng$1$nếu như$n=0$Và$0$ngược lại, vì hàm rỗng là phép chiếu duy nhất lên tập rỗng. 

###4.$n$quả bóng không nhãn,$m$chiếc bình được dán nhãn 

Đây là số lượng nhiều bộ kích thước$n$rút ra từ một$m$- tập hợp phần tử, thành phần yếu tương đương của$n$vào trong$m$các bộ phận. Công thức là$$\binom{n+m-1}{m-1}=\binom{n+m-1}{n}.$$Khi$m=0$, trường hợp hợp lệ duy nhất là$n=0$, cho$1$. Khi$n=0$, tất cả các bình đều trống rỗng, cho$1$cho mọi$m$. 

###5.$n$quả bóng không nhãn,$m$bình được dán nhãn, tối đa một quả bóng cho mỗi bình 

Mỗi chiếc bình chứa một trong hai$0$hoặc$1$bóng, vì vậy chúng tôi chọn$n$bình từ$m$. Số lượng là$$\binom{m}{n}.$$Đây là$0$khi$n>m$, và bằng$1$khi$n=0$hoặc$m=0$với$n=0$. 

###6.$n$quả bóng không nhãn,$m$bình được dán nhãn, ít nhất một quả bóng cho mỗi bình 

Điều này tính các thành phần của$n$vào trong$m$những phần tích cực. Số lượng là$$\binom{n-1}{m-1}$$vì$n\ge m\ge 1$, Và$0$nếu không thì. 

Khi$m=0$, giá trị là$1$nếu như$n=0$Và$0$nếu không thì. Khi$n=0$, không có phần dương nào tồn tại trừ khi$m=0$. 

###7.$n$quả bóng được dán nhãn,$m$chiếc bình không nhãn mác 

Điều này đếm các phân vùng của một$n$-phần tử được đặt nhiều nhất$m$khối. Số là số Bell bị hạn chế$$\sum_{k=0}^m S(n,k),$$Ở đâu$S(n,k)$là số Stirling loại hai. 

Khi$m\ge n$, số này trở thành số Chuông$B_n$. Khi$n=0$, giá trị là$1$cho tất cả$m$. Khi$m=0$, đó là$1$chỉ nếu$n=0$. 

###8.$n$quả bóng được dán nhãn,$m$bình không có nhãn, tối đa một quả bóng cho mỗi bình 

Mỗi cấu hình tương ứng với việc chọn một tập hợp con các quả bóng có kích thước tối đa$m$, vì mỗi chiếc bình được sử dụng chứa chính xác một quả bóng và các chiếc bình không thể phân biệt được. Như vậy số đó là$$\sum_{k=0}^{\min(n,m)} \binom{n}{k}.$$Khi$m\ge n$, điều này bằng$2^n$. Khi$n=0$, đó là$1$. 

###9.$n$quả bóng được dán nhãn,$m$bình không có nhãn, ít nhất một quả bóng mỗi bình 

Điều này đếm các phân vùng của một$n$-đặt vào chính xác$m$khối:$$S(n,m).$$Khi$m>n$, giá trị là$0$. Khi$n=0$, đó là$1$nếu như$m=0$Và$0$nếu không thì. 

###10.$n$quả bóng không nhãn,$m$chiếc bình không nhãn mác 

Điều này đếm các phân vùng số nguyên của$n$vào nhiều nhất$m$bộ phận:$$p(n,\le m),$$số lượng phân vùng của$n$với số phần lớn nhất nhiều nhất$m$. 

Khi$m\ge n$, số này bằng số phân vùng thông thường$p(n)$. Khi$n=0$, giá trị là$1$cho tất cả$m$. 

###11.$n$quả bóng không nhãn,$m$bình không có nhãn, tối đa một quả bóng cho mỗi bình 

Vì các quả bóng giống hệt nhau và các bình giống hệt nhau nên chỉ có số lượng bình chứa là quan trọng, nhưng sức chứa bị giới hạn ở$0$hoặc$1$. Một cấu hình chỉ tồn tại nếu$n\le m$và trong trường hợp đó có chính xác một cấu hình. Do đó giá trị là$$[n\le m],$$Ở đâu$[P]$là$1$nếu như$P$là đúng và$0$nếu không thì. 

###12.$n$quả bóng không nhãn,$m$bình không có nhãn, ít nhất một quả bóng mỗi bình 

Điều này đếm các phân vùng của$n$vào chính xác$m$bộ phận:$$p(n,m),$$số lượng phân vùng số nguyên của$n$với chính xác$m$những lời triệu tập tích cực Đó là$0$Trừ khi$n\ge m$. Khi$n=0$, nó bằng$1$nếu như$m=0$Và$0$nếu không thì. 

### Tính nhất quán cuối cùng 

Mỗi công thức tôn trọng các điều kiện biên$n=0$Và$m=0$bằng cách tạo ra một cấu hình trống duy nhất hoặc cấm bất kỳ cấu hình nào, phù hợp với các diễn giải tổ hợp trong tất cả mười hai trường hợp. 

Điều này hoàn thành giải pháp. ∎
