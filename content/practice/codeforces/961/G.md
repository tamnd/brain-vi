---
title: "CF 961G - Phân vùng"
description: "Chúng ta được cấp các phần tử có trọng số $n$. Mỗi phần tử $i$ có trọng số $wi$. Chúng tôi xem xét tất cả các cách để chia các phần tử $n$ này thành các nhóm chính xác $k$ không trống. Mỗi nhóm như vậy là một phân vùng không có thứ tự, do đó chỉ có cấu trúc thành viên là quan trọng chứ không phải bất kỳ nhãn nào của nhóm."
date: "2026-06-17T01:47:40+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 961
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 41 (Rated for Div. 2)"
rating: 2700
weight: 961
solve_time_s: 25
verified: false
draft: false
---

[CF 961G - Phân vùng](https://codeforces.com/problemset/problem/961/G) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$n$các phần tử có trọng số. Mỗi phần tử$i$có trọng lượng$w_i$. Chúng tôi xem xét mọi cách để phân chia chúng$n$thành phần chính xác$k$các nhóm không trống. Mỗi nhóm như vậy là một phân vùng không có thứ tự, do đó chỉ có cấu trúc thành viên là quan trọng chứ không phải bất kỳ nhãn nào của nhóm. 

Đối với bất kỳ phân vùng cố định nào, giá trị của nó được tính như sau. Mỗi tập hợp con đóng góp một điểm bằng kích thước của nó nhân với tổng trọng số bên trong nó. Tổng giá trị của phân vùng là tổng của các đóng góp tập hợp con này. Nhiệm vụ là tính tổng giá trị này trên tất cả các phân vùng hợp lệ thành$k$tập hợp con. 

Khó khăn không phải ở việc đánh giá một phân vùng mà ở việc tổng hợp các đóng góp trên một họ phân vùng khổng lồ, có số lượng tăng gần giống như số Stirling loại thứ hai, theo cấp số nhân trong$n$. Với$n$lên đến$2 \cdot 10^5$, việc liệt kê các phân vùng là không thể, và thậm chí$O(n^2)$quá chậm. 

Một vấn đề nhỏ sẽ xuất hiện nếu người ta cố gắng xử lý các phân vùng một cách độc lập cho mỗi phần tử hoặc theo kích thước tập hợp con. Ví dụ: một ý tưởng ngây thơ có thể thử đếm số lần trọng số xuất hiện trong một tập hợp con có kích thước$s$, nhưng điều này bỏ qua rằng phân bố kích thước tập hợp con được kết hợp giữa tất cả các phần tử thông qua ràng buộc toàn cục về việc có chính xác$k$tập hợp con. 

Một lỗi minh họa nhỏ: nếu tất cả các trọng số là 1 và$n=3, k=2$, một nỗ lực ngây thơ xử lý các phần tử một cách độc lập có thể vượt quá cấu hình trong đó các phần tử khác nhau nằm trong các tập hợp con có kích thước khác nhau mà không tôn trọng rằng các phân vùng là đối tượng chung. 

Giải pháp đúng phải tổng hợp các đóng góp bằng cách đối xứng trên các phần tử, giảm vấn đề về việc đếm tần suất một phần tử cố định đóng góp trên tất cả các phân vùng. 

## Phương pháp tiếp cận 

Quan sát chính là tính tuyến tính trên các phần tử. Trọng số phân vùng là tổng của các tập hợp con và trọng số của mỗi tập hợp con chính là tổng của các phần tử, nhân với kích thước tập hợp con. Mở rộng mọi thứ, chúng tôi viết lại sự đóng góp của một phân vùng$R$:$$W(R) = \sum_{S \in R} |S| \sum_{i \in S} w_i$$Hoán đổi tổng:$$W(R) = \sum_{i=1}^n w_i \cdot |S(i)|,$$Ở đâu$S(i)$là tập con chứa phần tử$i$, Và$|S(i)|$là kích thước của nó. Vì vậy, mỗi phần tử đóng góp trọng lượng của nó nhân với kích thước khối của chính nó. 

Bây giờ toàn bộ vấn đề được quy về tính toán, cho mỗi phần tử$i$, tổng của tất cả các phân vùng có kích thước của khối chứa$i$. Theo tính đối xứng, giá trị này giống hệt nhau đối với tất cả các phần tử, do đó câu trả lời trở thành:$$\left(\sum_{i=1}^n w_i\right) \cdot F(n,k),$$Ở đâu$F(n,k)$là tổng kích thước của khối chứa phần tử cố định trên tất cả các phân vùng thành$k$khối. 

Bây giờ chúng ta sửa phần tử 1. Giả sử khối của nó có kích thước$s$. Sau đó ta chọn phần còn lại$s-1$các phần tử của khối của nó trong$\binom{n-1}{s-1}$cách. Sau khi sửa xong khối này, phần còn lại$n-s$các phần tử phải được phân chia thành$k-1$khối, đóng góp$S(n-s,k-1)$, số Stirling loại hai. 

Như vậy:$$F(n,$$
