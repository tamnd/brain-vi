---
title: "CF 103664B - \u0411\u043e\u043b\u044c\u0448\u0438\u0435 \u044d\u043a\u0440\u0430\u043d\u044b"
description: "Giả sử $G$ là đồ thị Cayley của $Sn$ với tập sinh $${sigma,tau}, qquad sigma = (1,2,dots,n), quad tau = (1,2),$$ trong đó $n ge 3$ là số lẻ."
date: "2026-07-02T21:49:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103664
codeforces_index: "B"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2019"
rating: 0
weight: 103664
solve_time_s: 129
verified: false
draft: false
---

[CF 103664B - \u0411\u043e\u043b\u044c\u0448\u0438\u0435 \u044d\u043a\u0440\u0430\u043d\u044b](https://codeforces.com/problemset/problem/103664/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

hãy để$G$là đồ thị Cayley của$S_n$với tổ máy phát điện$$\{\sigma,\tau\}, \qquad \sigma = (1\,2\,\dots\,n), \quad \tau = (1\,2),$$Ở đâu$n \ge 3$thật kỳ quặc. Tập đỉnh là$S_n$, và với mỗi$g \in S_n$có các cạnh định hướng$g \to g\sigma$Và$g \to g\tau$(được hiểu là các cạnh vô hướng theo nghĩa đồ thị Cayley thông thường). 

Vấn đề đặt ra là liệu$G$có chu trình Hamilton với mọi số lẻ$n \ge 3$. 

## Kết quả đã biết 

các yếu tố$\sigma$Và$\tau$phát ra$S_n$. Thật vậy, sự liên hợp$\tau$bằng quyền hạn của$\sigma$sản lượng$$\sigma^k \tau \sigma^{-k} = (k+1 \; k+2),$$với các chỉ số được giải thích theo modulo$n$, do đó tất cả các chuyển vị liền kề đều được tạo ra, do đó tất cả$S_n$. 

Hoán vị$\sigma$có dấu hiệu$(-1)^{n-1}$, vậy khi nào$n$thật kỳ lạ,$\sigma$là chẵn. Chuyển vị$\tau$thật kỳ quặc. Do đó các cạnh được dán nhãn bởi$\sigma$duy trì sự bình đẳng trong$S_n$, trong khi các cạnh được dán nhãn bởi$\tau$chuyển đổi tính chẵn lẻ. Do đó, biểu đồ Cayley chứa cả cạnh bảo toàn chẵn lẻ và cạnh đảo ngược chẵn lẻ, do đó, nó không phải là lưỡng cực đối với riêng tập hợp tạo; tính chẵn lẻ không áp đặt sự cản trở toàn cầu đối với Hamiltonicity. 

Nhóm con$\langle \sigma \rangle$là trật tự tuần hoàn$n$. Phân vùng bên phải của nó$S_n$vào trong$(n-1)!$lớp, mỗi lớp có kích thước$n$. Trong mỗi coset, nhân với$\sigma$gây ra một$n$-xe đạp. 

Đồ thị Cayley của các nhóm đối xứng được tạo ra bởi các tập hợp chuyển vị và chu trình tùy ý được biết đến là Hamiltonian trong nhiều trường hợp có cấu trúc, đặc biệt khi các bộ tạo tương ứng với các chuyển vị liền kề tiêu chuẩn hoặc khi đồ thị Cayley là tích Descartes hoặc vòng hoa của đồ thị Hamilton đơn giản hơn. Tuy nhiên, không có định lý tổng quát nào áp dụng thống nhất cho cặp${\sigma,\tau}$cho tất cả$n$. 

Đối với trường hợp nhỏ: 

-$n=3$:$\sigma=(1,2,3)$,$\tau=(1,2)$tạo ra$S_3$. Đồ thị Cayley trên$6$các đỉnh có thể dễ dàng kiểm tra xem có chứa chu trình Hamilton bằng cách liệt kê rõ ràng hay không. 
-$n=5$và các giá trị cao hơn chấp nhận các chu trình Hamilton được xây dựng bằng tính toán cho các trường hợp cụ thể, nhưng không có cấu trúc cấu trúc thống nhất nào được thiết lập trong tài liệu ở dạng được yêu cầu ở đây. 

Vấn đề chung nằm trong bối cảnh rộng hơn của đồ thị Hamiltonicity of Cayley của$S_n$, điều này không được giải quyết cho các tập sinh tùy ý, mặc dù nhiều họ đặc biệt được biết đến là Hamiltonian. 

## Đối số một phần 

Viết$H = \langle \sigma \rangle$. Mỗi chiếc áo lót bên phải$gH$có hình thức$$g, \; g\sigma, \; g\sigma^2, \; \dots, \; g\sigma^{n-1},$$hình thành một$n$-Chu kỳ dưới phép nhân với$\sigma$. Do đó các cạnh được dán nhãn bởi$\sigma$phân tích đồ thị Cayley thành$(n-1)!$chu kỳ rời rạc. 

Hành động của$\tau$bản đồ một chiếc áo khoác$gH$đến chỗ ngủ$g\tau H$. Từ$$\tau \sigma^k \tau = (1\,2)\,(1\,2\,\dots\,n)^k\,(1\,2),$$máy phát điện$\tau$liên hợp$\sigma$với một phép quay tuần hoàn của các ký hiệu liền kề, do đó nó hoán vị các lớp ghép theo cách được xác định bởi tác động cảm ứng lên$H \backslash S_n$. 

Điều này tạo ra một cấu trúc thương số trên các lớp lân cận giống như một đồ thị rất đều đặn trên$(n-1)!$đỉnh, trong đó mỗi đỉnh có một “$\sigma$-cycle” cấu trúc bên trong và một “$\tau$-khớp" với một chu trình khác. Một chu trình Hamilton trong$G$sẽ tương ứng với một phép duyệt: 

đầu tiên đi dọc theo một$\sigma$đoạn -cycle, sau đó sử dụng một$\tau$-edge để nhảy sang một coset khác và tiếp tục sao cho mỗi chu kỳ coset được ghép chính xác một lần vào một chu kỳ chung. 

Trở ngại cho việc xây dựng quy nạp trực tiếp là đồ thị lớp cảm ứng không phải là một chu trình hoặc cây đơn giản; nó có sự chồng chéo không cần thiết đến từ sự tương tác giữa phép nhân phải với$\sigma$và liên hợp bởi$\tau$. Điều này ngăn chặn việc giảm đơn giản thành đệ quy kiểu P của Thuật toán hoặc thành mã Gray của sản phẩm vòng hoa mà không có cấu trúc bổ sung. 

Không có bất biến nào được biết đến bắt nguồn từ thống kê chẵn lẻ, cấu trúc coset hoặc thống kê đảo ngược buộc Hamiltonicity phải thất bại, nhưng cũng không có bất kỳ biến nào mang lại một sơ đồ truyền tải hoàn chỉnh. 

## Trạng thái 

Sự tồn tại chu trình Hamilton trong đồ thị Cayley của$S_n$được tạo bởi$\sigma = (1,2,\dots,n)$Và$\tau = (1,2)$vì tất cả những điều kỳ lạ$n \ge 3$không được giải quyết bằng một định lý tổng quát trong tài liệu tiêu chuẩn về đồ thị Hamiltonicity của Cayley của các nhóm đối xứng. Bài toán vẫn mở ở dạng tổng quát này, chỉ có một phần cách xây dựng và xác nhận tính toán cho các giá trị cụ thể của$n$. 

Điều này hoàn thành việc phân tích giải pháp. ∎
