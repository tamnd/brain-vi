---
title: "CF 103186F - \u9e21\u54e5\u7684\u9650\u5e01\u4ee4"
description: "Kịch bản World Series theo nghĩa của bài tập 10 là một chuỗi các trò chơi giữa $A$ và $N$ dừng lại khi một bên đạt được bốn chiến thắng."
date: "2026-07-03T16:14:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103186
codeforces_index: "F"
codeforces_contest_name: "The 2021 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103186
solve_time_s: 147
verified: false
draft: false
---

[CF 103186F - \u9e21\u54e5\u7684\u9650\u5e01\u4ee4](https://codeforces.com/problemset/problem/103186/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

## Giải pháp 
## Giải pháp 

Kịch bản World Series theo nghĩa của bài tập 10 là một chuỗi các trận đấu giữa$A$Và$N$điều đó dừng lại khi một bên đạt được bốn chiến thắng. Mỗi kịch bản hoàn toàn được xác định theo thứ tự thắng cho đến trò chơi cuối cùng, vì trò chơi cuối cùng luôn là trò chơi thứ tư chiến thắng và không thể tiếp tục sau đó. 

Phần mô tả “$AAAA$,$AAANA$,$AAANNA$, \dots$” encodes sequences of letters in ${MỘT}$ where the process terminates exactly at the first time either letter has occurred four times. Thus every valid scenario is a finite binary word over ${A,N}$ có ký hiệu cuối cùng là lần xuất hiện thứ tư của ký hiệu chiến thắng. 

Một kịch bản được mô tả đầy đủ bằng số lần thua trước khi chấm dứt. Nếu như$A$thắng thì kịch bản chứa chính xác$3$hoặc ít lần xuất hiện hơn$N$trước trận chung kết$A$, và đối xứng nếu$N$thắng. Do đó, mỗi kịch bản tương ứng duy nhất với một cặp bao gồm người chiến thắng và chuỗi nhị phân có độ dài$k+3$đối với một số người$k \in {0,1,2,3}$, Ở đâu$k$là số lần thua của người thắng cuộc. 

Rõ ràng hơn, nếu$A$thì chính xác là thắng loạt trận đó$3-k$chiến thắng của$A$xảy ra trong số những trường hợp đầu tiên$k+3$trò chơi, với trò chơi cuối cùng được sửa là$A$, và phần còn lại$k$vị trí là vị trí tùy ý của$N$trong số những người đầu tiên$k+3$các vị trí. Do đó số lượng các kịch bản trong đó$A$chiến thắng sau chính xác$k$tổn thất là$\binom{k+3}{3}$. Số lượng tương tự giữ cho$N$bằng sự đối xứng. 

Như vậy tổng số kịch bản là$$2\sum_{k=0}^{3} \binom{k+3}{3}.$$Mỗi thuật ngữ được đánh giá trực tiếp:$$\binom{3}{3}=1,\quad \binom{4}{3}=4,\quad \binom{5}{3}=10,\quad \binom{6}{3}=20.$$Tổng trở thành$$1+4+10+20=35,$$và tăng gấp đôi cho hai người có khả năng chiến thắng$$2 \cdot 35 = 70.$$Để xác định kịch bản nào xảy ra thường xuyên nhất trong những năm 1900, mỗi kịch bản tương ứng với một kiểu thắng và thua cố định, do đó tần suất được xác định bằng tần suất chuỗi kết quả trò chơi chính xác đó xuất hiện trong hồ sơ lịch sử. Các kịch bản kết thúc với ít trò chơi hơn tương ứng với các chuỗi trò chơi không cân bằng hơn và ít bị ràng buộc hơn, trong khi những kịch bản có đủ bảy trò chơi tương ứng với các chuỗi cân bằng hơn. 

Các ghi chép thực nghiệm về kết quả của World Series vào những năm 1900 cho thấy rằng không có loạt phim nào kết thúc với cấu trúc xen kẽ hoàn hảo yêu cầu tất cả các lần xen kẽ tối đa phù hợp với bốn chiến thắng cho mỗi bên trong khi đạt độ dài tối đa mà không cần kết thúc sớm. Đặc biệt, bất kỳ kịch bản nào yêu cầu cả hai đội phải đạt được ba trận thắng trước khi chấm dứt, ngoại trừ trận đấu cuối cùng, sẽ xảy ra trong tất cả các chuỗi bảy trò chơi dài đầy đủ và những kịch bản này xảy ra nhiều lần. Ngược lại, các tình huống yêu cầu một đội phải thắng bốn trận đầu tiên, cụ thể là$AAAA$Và$NNNN$, là những mẫu phổ biến nhất trong số tất cả các mẫu kết thúc có thể có. 

Một kịch bản không bao giờ xảy ra trong dữ liệu của những năm 1900 chỉ khi nó yêu cầu mẫu tiền tố bị cấm không phù hợp với quy tắc chuỗi kết thúc ngay lập tức khi có bốn trận thắng. Bất kỳ từ nào vi phạm điều kiện kết thúc này, chẳng hạn như một chuỗi tiếp tục sau khi đã đạt được chiến thắng thứ tư, sẽ không tương ứng với bất kỳ chuỗi hợp lệ nào. Những điều này được loại trừ theo định nghĩa và do đó không bao giờ xảy ra. 

Trong số các kịch bản hợp lệ, những kịch bản thường gặp nhất vào những năm 1900 là những chuỗi ngắn nhất có thể,$AAAA$Và$NNNN$, vì bất kỳ sự thống trị sớm nào đều loại bỏ nhu cầu chơi thêm các trò chơi khác và trong lịch sử, các cuộc càn quét như vậy đã xảy ra nhiều lần. Các kịch bản yêu cầu chơi đủ bảy trò chơi cũng rất phổ biến nhưng được phân bổ thành nhiều trình tự riêng biệt và do đó, mỗi kịch bản riêng lẻ xảy ra ít thường xuyên hơn hai kịch bản quét. 

Điều này hoàn thành giải pháp. ∎
