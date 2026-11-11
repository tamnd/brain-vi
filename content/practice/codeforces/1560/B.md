---
title: "CF 1560B - Ai đối diện?"
description: "Chúng ta được cấp ba nhãn riêng biệt để chỉ những người được đặt trên một vòng tròn hoàn toàn bằng nhau. Kích thước vòng tròn chẵn nhưng không xác định và các nhãn không đảm bảo liên tiếp hoặc nhỏ."
date: "2026-06-16T16:37:51+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1560
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 739 (Div. 3)"
rating: 800
weight: 1560
solve_time_s: 123
verified: false
draft: false
---

[CF 1560B - Ai đối lập?](https://codeforces.com/problemset/problem/1560/B) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp ba nhãn riêng biệt để chỉ những người được đặt trên một vòng tròn hoàn toàn bằng nhau. Kích thước vòng tròn chẵn nhưng không xác định và các nhãn không đảm bảo liên tiếp hoặc nhỏ. Quy tắc cấu trúc duy nhất là các vị trí đối diện trong vòng tròn là cố định: nếu bạn đứng ở một vị trí nào đó, người trực tiếp đối diện với tâm sẽ cách đó đúng nửa vòng tròn. 

Một mối quan hệ đã được cố định: người`a`khuôn mặt người`b`. Từ ràng buộc duy nhất này, toàn bộ kích thước vòng tròn trở nên bị hạn chế, bởi vì “đối diện” chỉ phụ thuộc vào tổng số người. Sau khi kích thước vòng kết nối phù hợp với cặp đôi này, chúng ta được yêu cầu xác định xem người đó là ai`c`đang đối mặt. Nếu không có kích thước vòng tròn chẵn nào có thể thỏa mãn ràng buộc đầu tiên, chúng ta phải báo cáo là không thể. 

Khó khăn chính là các nhãn là các số nguyên tùy ý cho đến$10^8$, vì vậy chúng tôi không trực tiếp xây dựng một hoán vị nhỏ hoặc mảng. Thay vào đó, chúng tôi đang xây dựng lại một cấu trúc mô-đun nhất quán trong đó điều ngược lại có nghĩa là thêm một nửa mô-đun kích thước vòng tròn vào tổng số. 

Thời hạn có thể lên tới$10^4$trường hợp thử nghiệm ngụ ý rằng chúng ta cần một$O(1)$mỗi giải pháp thử nghiệm. Mọi nỗ lực lặp lại các kích thước vòng tròn có thể có sẽ quá chậm vì kích thước vòng tròn không bị giới hạn ngoại trừ các ràng buộc về tính nhất quán. 

Một trường hợp cạnh tinh vi phát sinh khi kích thước vòng tròn được suy ra cần bằng 0 hoặc khi không có kích thước chẵn nào có thể đáp ứng mối quan hệ “ngược lại” cho cả hai điểm cuối. Một vấn đề khác là nhiều kích thước vòng tròn có thể thỏa mãn điều kiện, nhưng chỉ một số kích thước là hợp lệ dưới các ràng buộc về độ đồng đều. 

Ví dụ: nếu chúng ta giả định sai kích thước vòng tròn chỉ đơn giản là$2 \cdot |a - b|$, chúng ta có thể thất bại khi các nhãn quấn quanh nhau một cách khác nhau hoặc khi sự đối xứng tạo ra những mâu thuẫn. Lời giải đúng phải suy luận trực tiếp từ số học môđun hơn là trực giác hình học. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử các kích thước vòng tròn có thể$n$, bắt đầu từ 2 và tăng dần theo bước 2. Đối với mỗi ứng viên$n$, chúng tôi sẽ cố gắng chỉ định các vị trí sao cho$a$Và$b$chính xác là$n/2$tách rời modulo$n$. Một khi hợp lệ$n$được tìm thấy, chúng ta có thể tính toán điều ngược lại của$c$. 

Cách tiếp cận này đơn giản về mặt khái niệm: trong một vòng tròn có kích thước$n$, điều kiện ngược lại có nghĩa là$$(a - b) \equiv n/2 \pmod n.$$Chúng ta có thể kiểm tra giá trị của$n$cho đến khi điều này được giữ vững. Tuy nhiên, nhãn có thể lớn bằng$10^8$, vậy hợp lệ$n$cũng có thể lớn. Thậm chí cố gắng hết sức$n$lên tới$O(10^8)$mỗi trường hợp thử nghiệm là hoàn toàn không khả thi. 

Quan sát quan trọng là cấu trúc vòng tròn áp đặt một ràng buộc rất cứng nhắc: khoảng cách giữa các điểm đối diện chính xác bằng một nửa chu vi, do đó sự khác biệt tuyệt đối giữa các nhãn phải bằng nhau.$n/2$trong một số sơ đồ đánh số nhất quán. Vì các nhãn là tùy ý nhưng vòng tròn là đồng nhất, nên cách tái cấu trúc nhất quán duy nhất là kích thước vòng tròn phải là$2 \cdot |a - b|$. Nếu giá trị này không hợp lệ (cụ thể là nó phải chẵn và dương theo cách diễn giải nhất quán), thì không có giải pháp nào tồn tại. 

Khi kích thước vòng tròn được xác định, tính toán ngược lại$c$trở thành một sự phản xạ đối xứng trực tiếp: dịch chuyển một nửa kích thước vòng tròn. 

Do đó vấn đề giảm bớt từ việc tìm kiếm trên$n$để tính toán nó trực tiếp từ ràng buộc duy nhất mà chúng ta được đưa ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách giữa cặp đối diện đã cho là$d = |a - b|$. Điều này thể hiện một nửa kích thước vòng tròn vì các điểm đối xứng đối xứng quanh tâm. 
2. Nếu$d = 0$, ngay lập tức kết luận là không thể. Một vòng tròn có kích thước chẵn không thể đặt hai nhãn riêng biệt ở cùng một vị trí hoặc đối diện nhau. 
3. Đặt kích thước vòng kết nối ứng viên thành$n = 2d$. Đây là chu vi duy nhất có thể phù hợp với khoảng cách đối diện cần thiết. 
4. Kiểm tra xem có bố cục đối xứng nhất quán hay không. Vì các nhãn là trừu tượng nhưng phải bảo toàn tính đối xứng nên ràng buộc có ý nghĩa duy nhất là cấu trúc đó hợp lệ, do đó$n$phải dương và đều, điều này được đảm bảo bởi việc xây dựng. 
5. Tính ngược lại$c$bởi shifti
