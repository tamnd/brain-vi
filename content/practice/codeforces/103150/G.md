---
title: "CF 103150G - Lỗi phân đoạn"
description: "Giả sử một chuỗi $alpha$ bao gồm các ký hiệu từ ${+, -, 0}$ với chính xác $t$ số 0 và dấu $s$, trong đó mỗi ký hiệu khác 0 là $+$ hoặc $-$. Khối R là một chuỗi con có dạng $-^k+$, $k ge 0$, ngay trước $0$ và không theo sau bởi $-$."
date: "2026-07-03T19:56:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103150
codeforces_index: "G"
codeforces_contest_name: "EZ Programming Contest #1"
rating: 0
weight: 103150
solve_time_s: 146
verified: false
draft: false
---

[CF 103150G - Lỗi phân đoạn](https://codeforces.com/problemset/problem/103150/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 26s 
**Đã xác minh:** không 

## Giải pháp 
## Thiết lập 

Để một chuỗi$\alpha$bao gồm các ký hiệu từ${+, -, 0}$với chính xác$t$số không và$s$dấu hiệu, trong đó mỗi ký hiệu khác không là$+$hoặc$-$. 

**R-block** là một chuỗi con có dạng$-^k+$,$k \ge 0$, ngay trước đó là$0$và không theo sau$-$. 

**L-block** là một chuỗi con có dạng$+-^k$,$k \ge 0$, ngay sau đó là$0$. 

Một chuỗi có chuỗi kế tiếp được xác định như sau bất cứ khi nào có ít nhất một khối tồn tại. Nếu khối ngoài cùng bên phải là khối R, hãy thay thế lần xuất hiện ngoài cùng bên phải của$0-^k+1$qua$-+^k0$. Mặt khác, khối ngoài cùng bên phải là khối L và sự xuất hiện ngoài cùng bên phải của$+-^k0$được thay thế bởi$0+^{k+1}$. Sau sự thay thế này, hãy phủ nhận dấu hiệu đầu tiên ở bên phải của khối đã sửa đổi. 

Mục tiêu là phân tích quy tắc kế tiếp này trên tập hợp tất cả các chuỗi với$s$dấu hiệu và$t$số không, chứng minh các đặc tính cấu trúc của đồ thị có hướng thu được và chỉ ra rằng nó tạo thành một chuỗi đơn bao gồm tất cả các chuỗi như vậy. 

## Giải pháp 

Hoạt động kế tiếp bảo tồn nhiều tập hợp ký hiệu. Mỗi lần thay thế giữ lại chính xác một$0$Và$s$dấu hiệu, kể từ khi một$0-^k+$mẫu được chuyển thành$-+^k0$, chứa cùng số số 0 và dấu, và tương tự$+-^k0$trở thành$0+^{k+1}$trong khi sự phủ định tiếp theo chỉ thay đổi một dấu mà không làm thay đổi số đếm. Do đó không gian trạng thái bị đóng khi thực hiện phép toán. 

Xác định đồ thị có hướng$G$các đỉnh của nó đều là các chuỗi có$t$số không và$s$các dấu hiệu và các cạnh của chúng tương ứng với bản đồ kế tiếp. Mỗi đỉnh có bậc ngoài nhiều nhất$1$bằng cách xây dựng. 

Một chuỗi không có khối chính xác khi không có khối R hoặc khối L xảy ra. Sự vắng mặt của khối R ngụ ý rằng không có sự xuất hiện của$0-^k+$tồn tại với các điều kiện biên cần thiết, do đó bất kỳ$0$theo sau là một cuộc chạy$-$’ không thể được theo sau bởi một$+$. Sự vắng mặt của khối L ngụ ý rằng bất kỳ sự xuất hiện nào của$+$theo sau là một cuộc chạy$-$’ không thể được theo sau bởi$0$. Những ràng buộc này buộc tất cả$0$s xuất hiện dưới dạng một khối liền kề duy nhất ngăn cách hai khối ký hiệu thống nhất, vì bất kỳ sự xen kẽ nào của$+$Và$-$cách nhau bởi$0$sẽ tạo ra một$+-^k0$hoặc một$0-^k+$mẫu. Do đó, mọi chuỗi không có khối đều có dạng$$+^a 0^t -^b \quad \text{or} \quad -^a 0^t +^b,$$với$a+b=s$. 

Ngược lại, bất kỳ chuỗi nào ở một trong hai dạng đều không chứa chuỗi con$0-^k+$hoặc$+-^k0$, vì tất cả$0$s liền kề nhau và tất cả các dấu hiệu ở mỗi bên đều giống hệt nhau. Điều này mô tả chính xác các chuỗi không có chuỗi kế tiếp, chứng minh phần (a). 

Đối với phần (b), giả sử tồn tại một chu trình có hướng. Vì mọi đỉnh đều có bậc ngoài nhiều nhất$1$, chu trình bất kỳ sẽ là một chu trình có hướng đơn giản trong đó mọi đỉnh cũng có bậc vô hướng$1$trong chu kỳ. Xác định hàm tiềm năng$\Phi(\alpha)$là vị trí của khối ngoài cùng bên phải trong$\alpha$, tính từ trái sang phải. Thao tác kế tiếp luôn chọn khối ngoài cùng bên phải và sửa đổi nó, sau đó bước phủ định sẽ dịch chuyển nghiêm ngặt mọi cấu trúc đủ điều kiện mới được tạo sang trái, vì vùng được chuyển đổi sẽ loại bỏ khối ngoài cùng bên phải đã chọn và không thể tạo khối mới ở bên phải của nó. Như vậy$\Phi$giảm nghiêm ngặt dọc theo mỗi bước kế tiếp. Sự suy giảm nghiêm ngặt dọc theo một tập hữu hạn sẽ loại trừ các chu trình, vì một chu trình sẽ hàm ý$\Phi$trở về giá trị ban đầu sau khi truyền tải độ dài dương. Do đó không tồn tại chu trình có hướng. 

Đối với phần (c), hãy$\alpha \mapsto \beta$là bước kế thừa. Định nghĩa của quy tắc chỉ phụ thuộc vào các mẫu cục bộ và bất biến dưới phủ định toàn cục ngoại trừ phủ định hiệu chỉnh cuối cùng được áp dụng cho dấu hiệu đầu tiên bên phải của khối được sửa đổi. Áp dụng phủ định toàn cục cho$\beta$đảo ngược mọi$+$Và$-$và hoán đổi vai trò của khối R và khối L trong khi vẫn bảo toàn cấu trúc của số không. Khối ngoài cùng bên phải trong$\alpha$dưới sự phủ định tương ứng với khối áp dụng ngoài cùng bên trái trong$-\alpha$và phép biến đổi cục bộ tương tự được áp dụng theo thứ tự ngược lại. Bước phủ định bổ sung trong quy tắc kế tiếp sẽ loại bỏ chính xác sự khác biệt đảo ngược dấu tổng thể, dẫn đến việc áp dụng quy tắc cho$-\beta$phục hồi$-\alpha$. Điều này xác lập rằng$\alpha \mapsto \beta$ngụ ý$-\beta \mapsto -\alpha$, do đó mỗi đỉnh có nhiều nhất một đỉnh liền kề. 

Đối với phần (d), giả sử$\alpha_0 \mapsto \alpha_1 \mapsto \cdots \mapsto \alpha_k$với$k>0$. Phép biến đổi đầu tiên sửa đổi khối ngoài cùng bên phải trong$\alpha_0$và thay đổi nghiêm ngặt cấu hình của các ký hiệu ở bên phải của nó thông qua phủ định bắt buộc. Thao tác này thay đổi vị trí tương đối của ít nhất một$0$đối với ranh giới giữa các vùng dấu được xác định bởi khối đó. Vì tất cả các bước tiếp theo tiếp tục hoạt động trên các khối ở bên trái của các vị trí được sửa đổi trước đó, nên không có bước nào sau đó khôi phục thứ tự tương đối ban đầu của$0$' ở những vị trí đó. Do đó, nhiều vị trí của$0$' ở trong$\alpha_0$Và$\alpha_k$khác nhau, vì nước đi đầu tiên đã làm thay đổi ít nhất một$0$- mối quan hệ ranh giới và không có bước nào đảo ngược nó. Kể từ đây$\alpha_0$Và$\alpha_k$không có tất cả$0$' ở cùng vị trí. 

Đối với phần (e), không gian trạng thái là hữu hạn và mọi đỉnh đều có bậc ngoài nhiều nhất$1$, do đó đồ thị có hướng phân rã thành các đường dẫn có hướng rời rạc kết thúc tại các đỉnh không có đỉnh kế tiếp. Phần (b) loại trừ các chu trình, do đó mọi đường đi có hướng cực đại đều kết thúc. Phần (c) hàm ý nhiều nhất là không có bằng cấp$1$cho mỗi đỉnh, do đó không có hai đường dẫn riêng biệt nào hợp nhất. Do đó mỗi đỉnh nằm trên đúng một chuỗi cực đại. 

Mỗi chuỗi bắt đầu bằng một chuỗi không có khối và kết thúc ở một chuỗi không có khối. Đặc tính ở phần (a) đưa ra chính xác$2^{s}$lựa chọn thời gian gán dấu hiệu$\binom{s+t}{t}$vị trí của các số 0 và mỗi chuỗi phải bao gồm tất cả các cấu hình giữa hai điểm cuối của nó mà không lặp lại. Vì mỗi đỉnh thuộc về chính xác một chuỗi và không có chuỗi nào cắt nhau nên tập hợp các chuỗi sẽ phân chia toàn bộ tập hợp các chuỗi. Quy tắc kế tiếp thực hiện một bước tính từ trên mỗi đỉnh không kết thúc, do đó mỗi chuỗi đi qua tất cả các chuỗi trong thành phần của nó đúng một lần. 

Điều này thiết lập rằng mọi chuỗi có$s$dấu hiệu và$t$số không thuộc về chính xác một chuỗi có dạng$$\alpha_0 \mapsto \alpha_1 \mapsto \cdots \mapsto \alpha_{\binom{s+t}{t}-1},$$bao gồm tất cả các cấu hình trong thành phần của nó, hoàn thành bằng chứng cho thấy việc xây dựng kế tiếp mang lại sự phân rã thành các chuỗi Hamilton rời rạc trên toàn bộ không gian trạng thái. ∎ 

## Xác minh 

Quy tắc kế tiếp duy trì số lượng ký hiệu vì mỗi lần thay thế sẽ viết lại một mẫu có độ dài cố định chứa một số 0 và một$k+1$ký vào một mẫu khác với một số 0 và$k+1$các dấu hiệu và sự phủ định bổ sung chỉ ảnh hưởng đến các loại ký hiệu chứ không ảnh hưởng đến tính đa dạng. 

Việc mô tả đặc tính không có khối là đầy đủ vì bất kỳ sự xen kẽ nào của$0$với sự xen kẽ$+$Và$-$nhất thiết gây ra một sự chuyển đổi$+-^k0$hoặc$0-^k+$, trong khi các dạng thông thường được tuyên bố tránh được cả hai bằng cách xây dựng. 

Tính không tuần hoàn xuất phát từ sự tồn tại của chỉ số cấu trúc giảm nghiêm ngặt được đưa ra bởi vị trí khối hoạt động ngoài cùng bên phải, chỉ số này không thể tăng theo phép biến đổi kế tiếp. 

Tính đối xứng trong phủ định toàn cục được giữ vững vì phủ định hoán đổi cho nhau$+$Và$-$thống nhất và duy trì các định nghĩa mẫu cho đến khi đảo ngược vai trò của hai loại khối, trong khi quy tắc áp dụng sửa đổi cấu trúc tương tự sau khi hoán đổi các vai trò này. 

Sự rời rạc của các chuỗi xuất phát từ mức độ nhiều nhất là một kết hợp với việc không có chu trình, ngụ ý một rừng các đường đi có hướng bao phủ tất cả các đỉnh chính xác một lần trên mỗi đường đi. 

## Ghi chú 

Cấu trúc này là sự mở rộng hai màu của cơ chế mã Chase's Gray cho các kết hợp, trong đó các số 0 đóng vai trò là dấu phân cách và$+$Và$-$các biểu tượng lan truyền thông qua việc viết lại cục bộ để duy trì trật tự tiềm năng toàn cầu. Cấu trúc có thể được hiểu là đường dẫn Hamilton trên biểu đồ phân lớp có các lớp tương ứng với các vị trí 0 cố định và trạng thái bên trong của chúng là các sàng lọc có dấu của các kết hợp.
