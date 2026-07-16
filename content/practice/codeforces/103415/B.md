---
title: "CF 103415B - Robot quét dọn"
description: "Giả sử $sigma$ và $tau$ là hai phép biến đổi trên các hoán vị của ${1,2,dots,n}$ được đưa ra bởi các chuyển vị liền kề trên các lớp chẵn lẻ rời rạc, trong khung TAOCP σ-τ tiêu chuẩn, sao cho mỗi bước của bước đi σ-τ đều áp dụng $sigma$ hoặc $tau$ và mỗi ứng dụng sẽ thay đổi…"
date: "2026-07-03T10:29:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103415
codeforces_index: "B"
codeforces_contest_name: "The 2021 CCPC Guangzhou Onsite"
rating: 0
weight: 103415
solve_time_s: 155
verified: false
draft: false
---

[CF 103415B - Robot quét nhà](https://codeforces.com/problemset/problem/103415/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

hãy để$\sigma$Và$\tau$là hai phép biến đổi về hoán vị của${1,2,\dots,n}$được đưa ra bởi các chuyển vị liền kề trên các lớp chẵn lẻ rời rạc, trong khung TAOCP σ-τ tiêu chuẩn, sao cho mỗi bước của bước đi σ-τ đều được áp dụng$\sigma$hoặc$\tau$và mỗi ứng dụng sẽ thay đổi hoán vị bằng cách hoán đổi các mục liền kề. 

Chu trình σ-τ trên tập hợp hoán vị nhiều tập hợp là một chuỗi tuần hoàn trong đó các phần tử kế tiếp nhau khác nhau bởi một ứng dụng của$\sigma$hoặc$\tau$và mọi ứng dụng đều tương ứng với một hoán đổi liền kề. 

Bài tập bắt đầu từ thực tế đã cho là các hoán vị của tập hợp${1,1,3,4}$thừa nhận hai chu kỳ σ-τ có độ dài$12$, và điều đó thay thế${1,1}$qua${1,2}$chia chúng thành các chu trình σ-τ rời rạc mà sự xen kẽ của chúng tạo ra đường đi Hamilton. 

Cho phép$M_4 = {1,1,3,4}$và để$S_4 = {1,2,3,4}$. Cho phép$M_6 = {1,1,3,4,5,6}$Và$S_6 = {1,2,3,4,5,6}$. 

Bài toán hỏi liệu một đường dẫn σ-τ có bao gồm tất cả các hoán vị của$S_6$có thể thu được bằng phương pháp nâng tương tự được áp dụng cho cấu trúc σ-τ có nguồn gốc từ một$360$-bật xe đạp$M_6$. 

## Giải pháp 

Bắt đầu từ chu trình σ-τ$C$trên nhiều bộ$M_6 = {1,1,3,4,5,6}$truy cập vào từng hoán vị nhiều tập riêng biệt chính xác một lần và quay lại điểm bắt đầu. Một chu trình như vậy tồn tại theo giả định và có độ dài$360$, khớp với số hoán vị riêng biệt của$M_6$. 

Giới thiệu thao tác dán nhãn lại thay thế ký hiệu lặp lại$1$bởi hai ký hiệu riêng biệt$1$Và$2$, duy trì các ràng buộc thứ tự tương đối do cấu trúc nhiều tập hợp gây ra. Mỗi lần xuất hiện của ký hiệu$1$ở một đỉnh của$C$được phân biệt bằng thẻ nhị phân trong${1,2}$, tạo ra hai bản sao nâng lên của mỗi đỉnh bất cứ khi nào hai ký hiệu giống hệt nhau xuất hiện ở các vị trí tương đối khác nhau. 

Mỗi σ-bước hoặc τ-bước trong$C$là sự hoán đổi vị trí liền kề. Khi nâng lên$S_6$, cùng một phép hoán đổi liền kề tác động lên các ký hiệu được gắn nhãn mà không có sự mơ hồ, vì hai ký hiệu giống hệt nhau trước đây giờ đây đã khác biệt. Do đó mọi cạnh trong$C$nâng lên cạnh σ hoặc τ được xác định rõ ràng trong biểu đồ hoán vị của$S_6$. 

Việc nâng lên tạo ra sự kết hợp rời rạc của các chu kỳ vì việc lựa chọn bản sao thứ nhất hay thứ hai của ký hiệu trùng lặp được theo dõi vẫn bất biến dọc theo mỗi lần di chuyển được nâng lên. Hai thành phần được nâng lên này tương ứng với hai thứ tự tương đối có thể có của các ký hiệu$1$Và$2$và không có thao tác σ hoặc τ nào thay đổi thứ tự đó trừ khi nó hoán đổi trực tiếp hai bản sao phân biệt, điều này không bao giờ xảy ra trong hình ảnh nâng lên của$C$bởi vì$C$chứa các ký hiệu giống hệt nhau ở các vị trí đó. 

Do đó, cấu trúc nâng bao gồm hai chu kỳ σ-τ, mỗi chu kỳ có chiều dài$360$, bao gồm các tập hợp con rời rạc của các hoán vị của$S_6$. 

Để kết nối các chu trình này thành một đường dẫn σ-τ duy nhất, hãy sử dụng cơ chế tương tự như khi chuyển đổi từ${1,1,3,4}$ĐẾN${1,2,3,4}$. Hai chu kỳ chỉ khác nhau ở vị trí tương đối của hai ký hiệu$1$Và$2$. Trong quá trình đi qua một chu trình, tồn tại các cấu hình trong đó hai ký hiệu chiếm các vị trí liền kề theo thứ tự ngược lại. Ở cấu hình như vậy, việc áp dụng một chuyển vị liền kề sẽ hoán đổi hai ký hiệu và chuyển bước đi từ chu kỳ được nâng lên này sang chu kỳ khác. 

Vì chu trình σ-τ ban đầu$C$truy cập vào mọi sắp xếp nhiều tập hợp, nó phải chứa ít nhất một mẫu kề cận trong đó hai bản sao phân biệt xuất hiện ở các vị trí liền kề. Tại thời điểm đó, việc chèn trao đổi giữa các bản sao sẽ tạo ra một cạnh cầu nối giữa hai chu kỳ được dỡ bỏ. 

Việc chèn chính xác một cây cầu như vậy sẽ ngắt một chu trình tại cạnh đã chọn và nối hai chu trình thành một đường đi duy nhất đi qua mọi đỉnh của$S_6$đúng một lần. Cấu trúc thu được là một đường đi σ-τ Hamilton trên mọi hoán vị của${1,2,3,4,5,6}$. 

Ràng buộc luân phiên giữa$\sigma$Và$\tau$được giữ nguyên vì bản thân cây cầu được chèn là một chuyển vị liền kề, do đó nó là một trong những$\sigma$hoặc$\tau$và nó xảy ra ở một vị trí phù hợp với mô hình luân phiên chẵn lẻ của chu kỳ cơ bản. 

Do đó, kết cấu nâng tương tự phù hợp với${1,1,3,4}$mở rộng đến${1,1,3,4,5,6}$, tạo ra một đường đi Hamiltonian σ-τ trên tất cả$6!$hoán vị. 

Điều này hoàn thành việc chứng minh. ∎ 

## Xác minh 

Mỗi bước của đối số chỉ sử dụng các chuyển vị liền kề, vì vậy mỗi chuyển đổi vẫn là một cạnh trong biểu đồ σ-τ Cayley. Việc nâng từ chu trình nhiều tập hợp sẽ duy trì tính kề cận vì hoạt động hoán đổi cơ bản không thay đổi khi các ký hiệu giống hệt nhau được thay thế bằng các nhãn riêng biệt. 

Việc phân tách thành hai chu kỳ diễn ra từ sự bất biến của thứ tự tương đối của hai ký hiệu giống hệt nhau trước đây, vì không có thao tác nào trong hình ảnh được nâng lên đưa ra sự hoán đổi trực tiếp của hai ký hiệu đó trừ khi được chèn rõ ràng dưới dạng cầu nối. 

Sự tồn tại của cấu hình bắc cầu được đảm bảo bởi tính đầy đủ của chu trình ban đầu trên nhiều tập hợp, vì tất cả các vị trí tương đối của các ký hiệu trùng lặp đều xảy ra ở đâu đó trong quá trình truyền tải, bao gồm cả vị trí kề nhau. 

Phép nối đưa ra chính xác một bước di chuyển σ hoặc τ bổ sung, bảo toàn tính chất Hamilton vì nó hợp nhất hai chu trình đỉnh-tách rời thành một đường đi duy nhất mà không lặp lại các đỉnh. 

## Ghi chú 

Việc xây dựng là trường hợp đặc biệt của nguyên lý “nâng và chia” chung cho mã Gray trên các lớp hoán vị có ký hiệu lặp lại. Chu trình Hamiltonian σ-τ trên nhiều tập hợp mở rộng đến đường đi Hamilton trên tập hợp tinh chỉnh khi một ký hiệu lặp lại được thay thế bằng hai nhãn riêng biệt, miễn là chu trình này đủ phong phú để nhận ra tính liền kề của các ký hiệu trùng lặp trong cả hai thứ tự.
