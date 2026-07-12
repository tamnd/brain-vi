---
title: "CF 103192A - \u51fa\u9898\u4eba\u7684\u788e\u788e\u5ff5"
description: "Giả sử các cơ sở kinh điển được biểu diễn dưới dạng $(alpha1,dots,alphat)$ như trong bài tập 12, trong đó mỗi $alphai$ là một chuỗi nhị phân có độ dài $n$ với chính xác một vị trí phân biệt bằng $1$."
date: "2026-07-03T16:10:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103192
codeforces_index: "A"
codeforces_contest_name: "The 9-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 103192
solve_time_s: 151
verified: false
draft: false
---

[CF 103192A - \u51fa\u9898\u4eba\u7684\u788e\u788e\u5ff5](https://codeforces.com/problemset/problem/103192/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Hãy để các cơ sở kinh điển được biểu diễn dưới dạng$(\alpha_1,\dots,\alpha_t)$như trong bài tập 12, trong đó mỗi$\alpha_i$là một chuỗi nhị phân có độ dài$n$với đúng một vị trí phân biệt bằng$1$. Tương đương, mỗi$\alpha_i$mã hóa sự lựa chọn của một phần tử của${0,1,\dots,n-1}$, vì vậy cơ sở kinh điển là một thứ tự$t$-tuple của các chỉ số riêng biệt. Viết các chỉ số đã chọn theo thứ tự tăng dần sẽ tạo ra một biểu diễn tiêu chuẩn của cùng một đối tượng như một$t$-tập hợp con, nhưng biểu diễn bộ dữ liệu theo thứ tự là biểu diễn tương thích với các bước di chuyển một bit trong mã hóa tọa độ của từng tập hợp con$\alpha_i$. 

Hai cơ sở chính tắc khác nhau bằng cách chỉ thay đổi một bit khi và chỉ khi chính xác một trong các$\alpha_i$thay đổi vị trí phân biệt của nó bằng cách$+1$hoặc$-1$trong mã hóa nhị phân của nó, vì tất cả các tọa độ khác vẫn cố định. Cách giải thích này phù hợp với sự kề cận được hiển thị trong ví dụ cho$n=3$,$t=2$, trong đó mỗi bước thay đổi chính xác một chuỗi tọa độ giữa hai hàng. 

Việc xây dựng tiến hành bằng quy nạp trên$n$. Để cố định$n$Và$t$, cho phép$G(n,t)$biểu thị đồ thị có các đỉnh là cơ sở chính tắc và các cạnh của nó nối các cặp khác nhau đúng một bit. Mục tiêu là xây dựng đường đi Hamilton trong$G(n,t)$. 

Vì$t=0$hoặc$t=n$, đồ thị có một đỉnh duy nhất, do đó khẳng định này là không đáng kể. Cho rằng$0<t<n$và giả sử đường đi Hamilton tồn tại với mọi giá trị nhỏ hơn của$n$. 

Phân chia các cơ sở kinh điển theo vị trí thay đổi đầu tiên giữa các cơ sở kế tiếp nhau$\alpha_i$khi quét tuple từ trái sang phải. Cụ thể hơn, chia tập đỉnh thành hai lớp tùy theo tọa độ đầu tiên$\alpha_1$sử dụng ký hiệu$0$trong bit có ý nghĩa ít nhất của nó hoặc$1$trong bit ít quan trọng nhất của nó. Hai đồ thị con cảm ứng này đẳng cấu với$G(n-1,t)$Và$G(n-1,t-1)$sau khi xóa hoặc thu gọn tọa độ tương ứng với vị trí bit đó, vì việc sửa một bit sẽ làm giảm các vị trí có sẵn hoặc số lượng tọa độ hoạt động theo cách giống hệt như cách phân rã tiêu chuẩn của các kết hợp trong Phần 7.2.1.3. 

Đường đi Hamilton trong$G(n-1,t)$liệt kê tất cả các cơ sở kinh điển có tọa độ đầu tiên tránh được bit phân biệt, trong khi đường đi Hamilton trong$G(n-1,t-1)$liệt kê những nơi mà bit đó được sử dụng trong chính xác một trong các$\alpha_i$. Hai đường dẫn này có thể được nối với nhau bằng một thao tác lật cạnh duy nhất để chuyển bit đó trong một tọa độ của bộ dữ liệu, vì chính xác một tọa độ thay đổi từ một$0$-vị trí của một$1$-vị trí hoặc ngược lại, tạo ra sự liền kề hợp lệ trong$G(n,t)$. 

Thứ tự nối được chọn sao cho điểm cuối của đường dẫn thứ nhất khác chính xác một bit so với điểm bắt đầu của đường dẫn thứ hai, điều này được đảm bảo bằng cách lấy các cấu trúc bổ sung trên các lệnh gọi đệ quy giống như mã Gray nhị phân được phản ánh cho siêu khối. Sự phản ánh đảm bảo rằng cấu trúc điểm cuối của$G(n-1,t)$phù hợp với cấu trúc bắt đầu của$G(n-1,t-1)$với sự khác biệt chính xác một bit trong một tọa độ duy nhất, do đó cạnh kết nối tồn tại trong$G(n,t)$. 

Điều này tạo ra một đường dẫn truy cập mọi cơ sở chính tắc chính xác một lần, vì mỗi khối đệ quy sẽ sử dụng hết một tập hợp con các đỉnh rời rạc và quá trình phân tách bao gồm tất cả các khả năng phân bổ bit phân biệt trên toàn bộ bộ dữ liệu.$(\alpha_1,\dots,\alpha_t)$. Mỗi chuyển đổi thay đổi chính xác một bit vì tất cả các chuyển đổi đệ quy đều bảo toàn tính kề cận trong các sơ đồ con tương ứng của chúng và quá trình chuyển đổi giữa các khối sẽ lật chính xác một bit tọa độ. 

Như vậy tồn tại một đường màu xám đi qua tất cả các cơ sở kinh điển$(\alpha_1,\dots,\alpha_t)$trong đó các đỉnh liên tiếp cách nhau đúng một bit. 

Điều này hoàn thành việc chứng minh. ∎
