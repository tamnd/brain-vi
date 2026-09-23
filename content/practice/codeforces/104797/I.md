---
title: "CF 104797I - Phát triển khu vực"
description: "Chúng ta được cung cấp một biểu đồ có hướng của các ngôi làng được kết nối bằng đường bộ. Mỗi con đường kết nối hai ngôi làng và có một hướng quy định trong đầu vào, nhưng chúng tôi có thể tự do chỉ định luồng thương nhân cuối cùng trên mỗi con đường theo hướng nhất định hoặc theo hướng ngược lại."
date: "2026-06-28T13:45:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104797
codeforces_index: "I"
codeforces_contest_name: "2021-2022 ICPC Central Europe Regional Contest (CERC 21)"
rating: 0
weight: 104797
solve_time_s: 29
verified: false
draft: false
---

[CF 104797I - Phát triển khu vực](https://codeforces.com/problemset/problem/104797/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng của các ngôi làng được kết nối bằng đường bộ. Mỗi con đường kết nối hai ngôi làng và có một hướng quy định trong đầu vào, nhưng chúng tôi có thể tự do chỉ định luồng thương nhân cuối cùng trên mỗi con đường theo hướng nhất định hoặc theo hướng ngược lại. Điều chúng ta phải quyết định là lượng nguyên chính xác của luồng xe trên mỗi con đường, trong đó việc đảo ngược một con đường được biểu thị bằng giá trị âm. 

Mỗi con đường phải chở một số lượng dương người buôn bán và con số đó phải nhỏ hơn một mô đun nhất định$M$. Yêu cầu quan trọng là một định luật bảo toàn: tại mỗi làng, tổng dòng chảy vào phải bằng tổng dòng chảy đi một cách chính xác chứ không chỉ xấp xỉ. 

Khó khăn đến từ việc chúng ta không bắt đầu từ một biểu đồ trống. Chúng ta được giao một phép gán sơ bộ về các luồng trên các cạnh, và phép gán này đã thỏa mãn chỉ bảo toàn modulo$M$. Vì vậy, tại mỗi nút, sự mất cân bằng giữa luồng vào và luồng ra là bội số của$M$, nhưng không nhất thiết phải bằng không. 

Chúng ta phải điều chỉnh các luồng trên các cạnh để việc bảo toàn trở nên chính xác, đồng thời giữ mọi trọng số của cạnh trong phạm vi$1$ĐẾN$M-1$hoặc báo cáo rằng không có sự điều chỉnh nào như vậy. 

Biểu đồ đủ lớn để bất kỳ cách tiếp cận nào phụ thuộc vào việc liệt kê các bài tập trên mỗi cạnh đều không thể thực hiện được. Với tối đa$10^4$các cạnh và$10^3$đỉnh, chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính về số cạnh. 

Một điểm tinh tế quan trọng là điều kiện ban đầu không phải là tùy ý: luồng đã cho đã thỏa mãn modulo cân bằng nút$M$. Điều này có nghĩa là mọi đỉnh đều có thâm hụt được xác định rõ ràng là bội số của$M$, điều này gợi ý rõ ràng về vấn đề nâng mô-đun hơn là tối ưu hóa luồng chung. 

Một trường hợp cạnh đơn giản để lộ cấu trúc. Giả sử hai nút được kết nối bằng một cạnh duy nhất. Nếu sự mất cân bằng modulo buộc một luồng ròng khác 0 thì không có cách nào khắc phục được vì mọi phép gán trên cạnh đó phải nằm giữa$1$Và$M-1$, do đó lực bảo toàn tạo ra sự bình đẳng, điều này có thể là không thể. Một trường hợp cạnh khác là biểu đồ bị ngắt kết nối trong đó mỗi thành phần phải thỏa mãn sự bảo toàn chính xác một cách độc lập, nếu không thì không có sự điều chỉnh toàn cục nào có thể khắc phục được. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là coi mỗi cạnh là một biến và cố gắng gán cho nó một giá trị giữa$1$Và$M-1$, và sau đó thực thi điều đó đối với mỗi đỉnh, tổng của các điểm đến bằng với các điểm đi ra một cách chính xác. Điều này trở thành một hệ phương trình tuyến tính có bất đẳng thức. Một cách tiếp cận bạo lực sẽ thử tất cả các phép gán, theo cấp số nhân về số cạnh và ngay lập tức không khả thi ngay cả đối với các trường hợp nhỏ, vì mỗi cạnh có$M-1$sự lựa chọn và$R$có thể$10^4$. 

Một nỗ lực có cấu trúc hơn là coi đây là một bài toán bảo toàn dòng chảy và cố gắng giải nó bằng cách sử dụng đại số tuyến tính trên các số nguyên. Điều kiện modulo trước hết gợi ý giải hệ modulo$M$, sau đó nâng nó lên số nguyên trong phạm vi giới hạn. Tuy nhiên, việc nâng trực tiếp không thành công vì các giá trị cạnh bị giới hạn và hoàn toàn dương, do đó các giải pháp mô-đun đơn giản có thể sử dụng các giá trị$0$những điều bị cấm. 
Quan sát quan trọng là
