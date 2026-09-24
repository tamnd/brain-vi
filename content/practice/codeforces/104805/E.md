---
title: "CF 104805E - Ngõ"
description: "Chúng ta có một số bóng tròn, mỗi bóng được gắn vào một cái cây có tâm nằm trên một đường ngang. Mỗi cây tạo ra một vùng bóng tròn trong mặt phẳng và nhiệm vụ là tính tổng diện tích được bao phủ bởi sự kết hợp của tất cả các đĩa này."
date: "2026-06-28T17:11:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104805
codeforces_index: "E"
codeforces_contest_name: "Central Russia Regional Contest, 2022"
rating: 0
weight: 104805
solve_time_s: 37
verified: false
draft: false
---

[CF 104805E - Ngõ](https://codeforces.com/problemset/problem/104805/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số bóng tròn, mỗi bóng được gắn vào một cái cây có tâm nằm trên một đường ngang. Mỗi cây tạo ra một vùng bóng tròn trong mặt phẳng và nhiệm vụ là tính tổng diện tích được bao phủ bởi sự kết hợp của tất cả các đĩa này. 

Về mặt hình học, mỗi vật là một đường tròn có tâm$(x_i, 0)$và bán kính$r_i$. Bởi vì tất cả các trung tâm đều nằm trên cùng một đường thẳng nên cấu hình là một chiều về vị trí nhưng vẫn hoàn toàn là hai chiều về tương tác. Mục đích không phải là đếm các vòng tròn hoặc sự chồng chéo mà là tính toán diện tích chính xác của khu vực được bao phủ ít nhất một lần bởi bất kỳ vòng tròn nào. 

Những hạn chế$n \le 1000$Và$|x_i|, r_i \le 1000$ngụ ý rằng một cách tiếp cận bậc hai trong$n$là khả thi, nhưng bất cứ thứ gì hình khối hoặc tệ hơn sẽ quá chậm. Một giải pháp kiểm tra tất cả các cặp đường tròn đều có thể chấp nhận được, nhưng bất kỳ giải pháp nào cố gắng rời rạc hóa mặt phẳng hoặc sử dụng các lưới mịn sẽ không thành công do độ chính xác và hiệu suất. 

Một vấn đề tế nhị là cấu trúc chồng chéo. Mặc dù tâm bị giới hạn thành một đường thẳng, các vòng tròn vẫn có thể chồng lên nhau một phần theo hai chiều. Một tổng hợp ngây thơ của các khu vực$\pi r_i^2$rõ ràng là vượt quá. Một ý tưởng ngây thơ khác, trừ trực tiếp các giao điểm theo cặp, cũng thất bại vì các phần trùng lặp ba lần sẽ bị trừ đi nhiều lần một cách không kiểm soát được. 

Một trường hợp thất bại cụ thể của phép trừ ngây thơ xuất hiện khi ba vòng tròn chồng lên nhau trong một chuỗi: 

đầu vào:```
3
0 3
4 3
8 3
```Vòng tròn ở giữa chồng lên cả hai vòng tròn khác, nhưng hai vòng tròn bên ngoài không chồng lên nhau trực tiếp. Phép trừ theo cặp đếm đôi phần trùng lặp với vòng tròn ở giữa. Câu trả lời đúng đòi hỏi phải có sự tính toán toàn cầu nhất quán về các đóng góp của ranh giới, chứ không phải sự điều chỉnh theo cặp. 

Cấu trúc của bài toán gợi ý một quan điểm dựa trên ranh giới: thay vì suy luận về phần bên trong, chúng ta theo dõi phần nào của ranh giới của mỗi vòng tròn thực sự đóng góp vào bề mặt bên ngoài của liên kết. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là ước chừng mặt phẳng bằng một lưới mịn và đánh dấu mọi ô được bao phủ bởi ít nhất một vòng tròn. Câu trả lời sẽ là số ô được đánh dấu nhân với diện tích ô. Điều này về mặt khái niệm đơn giản nhưng hoàn toàn không khả thi, vì đạt được$10^{-4}$độ chính xác sẽ đòi hỏi một lưới quá mịn, dẫn đến hàng tỷ ô. 

Một cách mạnh mẽ hơn có tính nguyên tắc hơn là tính toán hợp bằng cách sử dụng phép bao gồm-loại trừ. Đối với mỗi tập hợp con của vòng tròn, hãy tính diện tích giao điểm và các biển báo thay thế. Điều này đúng về mặt toán học nhưng theo cấp số nhân trong$n$, làm cho nó không thể sử dụng được ngoài những trường hợp rất nhỏ. 

Quan sát quan trọng là diện tích hợp có thể được tính từ ranh giới của hợp. Ranh giới chỉ bao gồm các cung tròn. Nếu chúng ta có thể xác định chính xác cung nào thuộc ranh giới bên ngoài và tính diện tích
