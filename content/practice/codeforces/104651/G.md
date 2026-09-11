---
title: "CF 104651G - GCD của So khớp mẫu"
description: "Chúng ta có một cơ sở $m$ và một chuỗi mẫu $P$ trên các chữ cái viết thường. Chúng ta diễn giải bất kỳ số nguyên dương nào dưới dạng số $m$-ary, được viết dưới dạng một chuỗi các chữ số."
date: "2026-06-29T16:28:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104651
codeforces_index: "G"
codeforces_contest_name: "The 2023 CCPC Online Contest"
rating: 0
weight: 104651
solve_time_s: 34
verified: false
draft: false
---

[CF 104651G - GCD của So khớp mẫu](https://codeforces.com/problemset/problem/104651/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một cơ sở$m$và một chuỗi mẫu$P$trên các chữ cái viết thường. Chúng ta diễn giải bất kỳ số nguyên dương nào là một$m$- số thứ tự, được viết dưới dạng một chuỗi các chữ số. Ràng buộc là các chữ số của số này phải khớp với mẫu thông qua việc gắn nhãn lại tính từ giữa các giá trị chữ số và chữ cái. 

Cụ thể hơn, mỗi vị trí trong căn cứ-$m$đại diện của một số được dán nhãn bằng một chữ cái từ$P$. Chúng ta được phép gán các chữ số riêng biệt$0 \ldots m-1$thành các chữ cái, nhưng phép gán phải mang tính nội từ: các chữ cái khác nhau phải ánh xạ tới các chữ số khác nhau. Khi một chữ cái được gán một chữ số, mọi lần xuất hiện của chữ cái đó đều phải sử dụng cùng một chữ số. Chữ số đứng đầu cũng là một chữ số, nhưng phải khác 0 vì chúng ta xem xét các số nguyên dương có biểu diễn vị trí tiêu chuẩn. 

Chúng ta xét tất cả các số nguyên dương có cơ số$m$chuỗi chữ số có thể thu được bằng cách chọn ánh xạ nội xạ hợp lệ từ các chữ cái sang chữ số. Trong số tất cả các số nguyên như vậy, chúng ta được yêu cầu tính ước số chung lớn nhất của chúng ở dạng thập phân. 

Các ràng buộc cực kỳ lớn về số lượng trường hợp thử nghiệm, lên tới 500.000, nhưng mỗi mẫu rất ngắn, độ dài tối đa là 16 và cơ sở nhiều nhất là 16. Điều này gợi ý rõ ràng rằng mỗi trường hợp thử nghiệm phải được xử lý trong thời gian không đổi hoặc gần như không đổi sau một số lần tính toán trước cố định trên các mẫu nhỏ. 

Một cách giải thích ngây thơ sẽ cố gắng liệt kê tất cả các ánh xạ nội xạ từ các chữ cái đến các chữ số và sau đó tạo ra tất cả các số nguyên, nhưng điều này ngay lập tức bùng nổ. Vì$k$có những chữ cái riêng biệt$P(m,k)$ánh xạ và mỗi ánh xạ cho chính xác một số nguyên. Ngay cả với$m \le 16$, con số này vốn đã lớn và việc nhân lên tới nửa triệu trường hợp thử nghiệm khiến điều đó là không thể. 

Một vấn đề tinh vi hơn là ngay cả khi chúng ta tính toán các giá trị, độ lớn của chúng vẫn tăng như$m^{|P|}$, có thể vẫn lớn, nhưng trở ngại thực sự là sự bùng nổ tổ hợp của các phép gán hợp lệ. 

Một vấn đề cấu trúc quan trọng là các hoán vị khác nhau của phép gán chữ số có thể tạo ra các số khác nhau, nhưng gcd của chúng có thể thu gọn về giá trị nhỏ hơn nhiều. Vì vậy, chúng ta không được yêu cầu liệt kê các giá trị mà phải trích xuất một bất biến số học chung. 

Các trường hợp cạnh đáng chú ý bao gồm các mẫu có các chữ cái lặp lại, các mẫu có tất cả các chữ cái riêng biệt và các mẫu trong đó ký tự đầu xuất hiện nhiều lần. Ví dụ, nếu$P = "aaaa"$, khi đó chỉ có một chữ số được sử dụng và mỗi số hợp lệ là một chữ số đại diện trong cơ số$m$, tạo ra một cấu trúc chuỗi hình học đơn giản. Nếu tất cả các ký tự đều khác biệt thì chúng ta đang hoán vị các chữ số một cách tự do và gcd sẽ bị chi phối bởi tính đối xứng trên tất cả các hoán vị. 

## Phương pháp tiếp cận 

Một ý tưởng vũ phu rất đơn giản. Đối với mỗi trường hợp thử nghiệm, liệt kê tất cả các ánh xạ nội xạ từ các chữ cái riêng biệt trong$P$đến chữ số$0 \ldots m-1$, thực thi rằng vị trí đầu tiên không được ánh xạ tới 0, xây dựng giá trị số nguyên tương ứng trong cơ sở$m$và tính gcd trên tất cả các giá trị được xây dựng. 

Điều này đúng nhưng đắt tiền. Nếu có$k$các chữ cái khác nhau thì số lần tiêm là$m \cdot (m-1) \cdots (m-k+1)$, trong trường hợp xấu nhất là$16!$- quy mô, khoảng$2 \times 10^{13}$khả năng cho mỗi trường hợp thử nghiệm ở mức cực độ. Ngay cả khi áp dụng biện pháp cắt tỉa, lặp đi lặp lại hơn 500.000 lần thử nghiệm cũng không khả thi. 

Quan sát quan trọng là chúng ta không bao giờ cần những con số thực tế, chỉ cần ước số chung lớn nhất của chúng. Điều này có nghĩa là chúng tôi muốn số nguyên lớn nhất chia tất cả các giá trị được xây dựng. Điều đó gợi ý nên tập trung vào những thuộc tính nào là bất biến trong tất cả các phép gán nội xạ hợp lệ. 

Mỗi số hợp lệ có thể được viết dưới dạng đa thức trong$m$, trong đó các hệ số là các chữ số được gán cho các chữ cái. Nếu chúng ta thay thế các chữ cái bằng các biến, mỗi phép gán sẽ đánh giá cùng một tổ hợp chữ số tuyến tính. Gcd trên tất cả các phép gán nội xạ sẽ trở thành gcd trên tất cả các đánh giá ở dạng tuyến tính này dưới tất cả các hoán vị của các chữ số. 

Điều này trở thành một vấn đề đối xứng cổ điển: thay vì liệt kê các bài tập, chúng tôi nghiên cứu cấu trúc gây ra bởi các hoán vị của các chữ số trên các vị trí được nhóm theo danh tính chữ cái. 

Sự giảm thiểu quan trọng là điều quan trọng là mỗi chữ số xuất hiện có trọng số theo lũy thừa vị trí bao nhiêu lần. Gcd thu gọn thành một hàm gồm hai đại lượng: tổng trọng số vị trí trên mỗi chữ cái và cấu trúc mô-đun của hoán vị các chữ số. Trong thực tế, gcd cuối cùng chỉ phụ thuộc vào tổng các vị trí chữ số và liệu mẫu có thực thi các ràng buộc duy nhất hay lặp lại hay không, làm giảm vấn đề tính toán một số duy nhất xuất phát từ trọng số vị trí và tính đối xứng tổ hợp của các phép gán. 

Điều này cho phép giảm từng trường hợp thử nghiệm để đếm các đóng góp vị trí cho mỗi chữ cái và kết hợp chúng bằng cách sử dụng quy tắc số học cố định bắt nguồn từ bất biến hoán vị, tránh bất kỳ phép liệt kê nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ về số lượng các chữ cái riêng biệt | O(1) | Quá chậm | 
| Tối ưu | O( | P | ) cho mỗi trường hợp thử nghiệm | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích mẫu này như việc gán các chữ số cho các chữ cái, sau đó tổng hợp các đóng góp của từng chữ cái được tính theo lũy thừa vị trí của$m$. 

1. Đối với mỗi trường hợp kiểm thử, hãy tính trọng số vị trí$w_i = m^i$cho từng vị trí trong mẫu. Điều này mã hóa sự đóng góp của một chữ số được đặt ở vị trí$i$. 
2. Với mỗi chữ cái riêng biệt, hãy tính tổng trọng số của nó$W(c)$, đó là tổng của$m^i$trên tất cả các vị trí mà chữ cái đó xuất hiện. Điều này làm giảm mẫu thành một tổ hợp tuyến tính của các biến chữ cái. 
3. Xác định xem ký tự đầu có bị buộc phải khác 0 hay không. Hạn chế này ảnh hưởng đến những hoán vị chữ số nào là hợp lệ và xác định liệu có áp dụng tính đối xứng hoàn toàn giữa các chữ số hay không hay liệu chúng ta có mất một bậc tự do trong phép gán hay không. 
4. Quan sát rằng tất cả các số nguyên hợp lệ đều thu được bằng cách gán các chữ số riêng biệt cho các chữ cái, vì vậy mỗi số nguyên là tổng của
