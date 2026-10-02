---
title: "CF 104874L - Độ dài và chu kỳ"
description: "Chúng tôi được cung cấp một chuỗi dài duy nhất trên các chữ cái tiếng Anh viết thường và chúng tôi muốn đo mức độ “lặp lại” của nó ở dạng bản địa hóa khắc nghiệt nhất."
date: "2026-06-28T10:09:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104874
codeforces_index: "L"
codeforces_contest_name: "2019-2020 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104874
solve_time_s: 24
verified: false
draft: false
---

[CF 104874L - Độ dài và Khoảng thời gian](https://codeforces.com/problemset/problem/104874/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi dài duy nhất trên các chữ cái tiếng Anh viết thường và chúng tôi muốn đo mức độ “lặp lại” của nó ở dạng bản địa hóa khắc nghiệt nhất. Đối tượng chúng ta đang tìm kiếm không phải là một mẫu lặp lại toàn cục mà là một đoạn liền kề bên trong chuỗi hoạt động giống như một khối lặp lại, có thể có bản sao một phần cuối cùng. 

Về mặt hình thức, chúng ta tưởng tượng việc chọn một chuỗi mẫu$x$. Bên trong chuỗi đầu vào$w$, chúng ta tìm kiếm một chuỗi con bao gồm nhiều bản sao đầy đủ của$x$, theo sau là tiền tố của$x$. Nếu chuỗi con này có tổng chiều dài$L$, thì nó đại diện cho một số phần lặp lại của$x$, cụ thể là$L / |x|$. Nhiệm vụ là tối đa hóa tỷ lệ này trên tất cả các lựa chọn có thể có của$x$và tất cả các chuỗi con phù hợp với cấu trúc này. 

Vì vậy, bài toán quy về việc tìm chuỗi con “phân số” dài nhất trong tất cả các chu kỳ có thể. 

Kích thước đầu vào đạt$2 \cdot 10^5$. Bất kỳ giải pháp nào kiểm tra tất cả các chuỗi con và tất cả các giai đoạn ứng viên đều trực tiếp dẫn đến hành vi bậc hai hoặc tệ hơn, vì có$O(n^2)$chuỗi con và mỗi lần kiểm tra định kỳ ít nhất là tuyến tính. Con số này vượt xa mức 2 giây cho phép trong Python. 

Do đó, một giải pháp đúng phải tránh lặp lại các chuỗi con một cách rõ ràng và thay vào đó sử dụng lại cấu trúc bên trong chuỗi, đặc biệt là cấu trúc tiền tố và thông tin về tính tuần hoàn. 

Có hai trường hợp phức tạp phá vỡ lý luận ngây thơ. 

Người ta đang nghĩ rằng chỉ có sự lặp lại đầy đủ mới quan trọng. Ví dụ: trong “abab”, mẫu tốt nhất là “ab” được lặp lại hai lần, cho số mũ 2. Nhưng trong các chuỗi như “mississippi”, cấu trúc tốt nhất bao gồm sự lặp lại tiền tố theo sau là một khối một phần, do đó việc bỏ qua sự trùng lặp một phần sẽ làm mất câu trả lời. 

Một giả định khác cho rằng mẫu tốt nhất phải là tiền tố của chuỗi. Điều đó là sai: các mẫu tối ưu có thể bắt đầu ở bất cứ đâu, như đã thấy trong “ississi” bên trong “mississippi”, trong đó đơn vị lặp lại không bị ràng buộc với tiền tố chung. 

Hai sự thật này buộc chúng ta phải xem xét các chuỗi con định kỳ một cách ngầm định, không chỉ các tiền tố toàn cục hoặc các chuỗi lặp chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi con$y = w[l:r]$, sau đó với mọi độ dài khoảng thời gian có thể$p$, xác minh có bao nhiêu lần lặp lại đầy đủ của mẫu độ dài ứng cử viên$p$xảy ra bên trong$y$, cộng với một hậu tố một phần có thể có. Mỗi chi phí xác minh$O(|y|)$, và có$O(n^2)$chuỗi con, đưa ra$O(n^3)$trong trường hợp xấu nhất. Ngay cả với sự tối ưu hóa, điều này vẫn sụp đổ dưới$n = 200000$. 

Quan sát quan trọng là một chuỗi con có số mũ lớn chính xác khi nó có khoảng thời gian ngắn. Nếu một chuỗi con có độ dài$L$có thời gian tối thiểu$p$, thì số mũ của nó là$L/p$. Vấn đề trở thành: tìm
