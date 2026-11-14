---
title: "CF 1778F - Tối đa hóa quyền root"
description: "Chúng ta được yêu cầu tối đa hóa giá trị của gốc cây sau khi thực hiện tối đa $k$ thao tác. Mỗi thao tác cho phép chúng ta chọn một đỉnh $v$ chưa được chọn trước đó, chọn một số $x$ chia tất cả các giá trị trong cây con của $v$ và nhân mọi giá trị trong đó…"
date: "2026-06-09T11:36:25+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "math", "number-theory", "trees"]
categories: ["algorithms"]
codeforces_contest: 1778
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 848 (Div. 2)"
rating: 2600
weight: 1778
solve_time_s: 34
verified: false
draft: false
---

[CF 1778F - Tối đa hóa Root](https://codeforces.com/problemset/problem/1778/F) 

**Đánh giá:** 2600 
**Tags:** dfs và tương tự, dp, đồ thị, toán, lý thuyết số, cây 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa giá trị của gốc cây sau khi thực hiện tối đa$k$hoạt động. Mỗi thao tác cho phép chúng ta chọn một đỉnh$v$chưa được chọn trước đó, hãy chọn một số$x$chia tất cả các giá trị trong$v$cây con của và nhân mọi giá trị trong cây con đó với$x$. Đầu vào cung cấp cấu trúc cây, giá trị đỉnh ban đầu và giới hạn$k$. Đầu ra là giá trị lớn nhất có thể có của nút gốc sau khi áp dụng các phép toán một cách tối ưu. 

Vì cây có thể có tới$10^5$các nút cho mỗi trường hợp thử nghiệm và có thể có tới$50{,}000$trường hợp thử nghiệm, bất kỳ thuật toán nào có$O(n^2)$độ phức tạp hoặc việc thử tất cả các tập hợp con của nút quá chậm. Chúng ta cần một cái gì đó tuyến tính trong$n$hoặc tệ nhất$n \log n$. Các giá trị tại các nút được giới hạn bởi 1000, điều này gợi ý rằng các phép toán dựa trên hệ số nguyên tố hoặc ước số có thể khả thi. 

Một trường hợp khó phát hiện khi$k=0$, nơi không được phép thực hiện bất kỳ thao tác nào. Một thuật toán bất cẩn luôn cố gắng nhân sẽ sửa đổi giá trị gốc không chính xác. Một trường hợp khác là khi cây có một chuỗi các nút có cùng giá trị, bởi vì các lựa chọn nhân cục bộ tham lam có thể cản trở lợi ích lớn hơn từ việc chọn cây con sâu hơn trước. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê tất cả các tập hợp con của nhiều nhất$k$các nút và tất cả các ước của mỗi cây con. Đối với mỗi lựa chọn, chúng tôi sẽ mô phỏng nhân cây con, sau đó tiếp tục đệ quy. Về nguyên tắc, điều này hoạt động, nhưng số lượng tập hợp nút có thể có là$O(\binom{n}{k})$, và cho$k$thậm chí khoảng 10 trên một cây có 100.000 nút, đây là con số lớn về mặt thiên văn. Việc tính toán ước số của cây con cũng không hề đơn giản nếu thực hiện nhiều lần. 

Điểm mấu chốt là hệ số nhân của một cây con được xác định bởi ước số chung lớn nhất (GCD) của cây con và các phép nhân sẽ truyền lên cây. Điều này có nghĩa là chúng ta có thể suy luận theo cách đệ quy: đối với một nút$v$, chúng ta có thể tính toán GCD của tất cả các phần tử con của nó, kết hợp chúng với giá trị riêng của nó để xác định các số nhân có thể có, sau đó chọn số nhân lớn nhất trước tiên để cực đại hóa nghiệm. Chúng ta có thể chính thức hóa điều này bằng cách sử dụng tìm kiếm chuyên sâu và lập trình động: mỗi nút duy trì một danh sách "các sản phẩm tốt nhất có thể đạt được bằng$j$
