---
title: "CF 102803E - Mọi người đều mất ai đó"
description: "Chúng ta được cấp một hoán vị mô tả thứ tự từ điển của tất cả các hậu tố của một chuỗi chưa biết. Chúng tôi cũng nhận được một số giá trị của mảng LCP giữa các hậu tố lân cận theo thứ tự đó. Một số giá trị LCP đã bị xóa và thay thế bằng -1."
date: "2026-07-26T16:22:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "E"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 48
verified: false
draft: false
---

[CF 102803E - Mọi người đều lạc mất ai đó](https://codeforces.com/problemset/problem/102803/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị mô tả thứ tự từ điển của tất cả các hậu tố của một chuỗi chưa biết. Chúng tôi cũng nhận được một số giá trị của mảng LCP giữa các hậu tố lân cận theo thứ tự đó. Một số giá trị LCP đã bị xóa và thay thế bằng`-1`. Nhiệm vụ là xây dựng lại chuỗi nhỏ nhất về mặt từ điển có thể tạo ra mảng hậu tố này và tất cả các giá trị LCP đã biết. 

Mảng hậu tố cho chúng ta biết nhiều điều hơn là chỉ sắp xếp thứ tự. Nếu hai hậu tố lân cận trong mảng hậu tố có giá trị LCP đã biết`h`, đầu tiên của họ`h`các ký tự phải giống hệt nhau. Nếu cả hai hậu tố tiếp tục sau những hậu tố đó`h`thì ký tự tiếp theo của hậu tố nhỏ hơn phải nhỏ hơn ký tự tiếp theo của hậu tố lớn hơn. Đây là những hạn chế trực tiếp duy nhất mà chúng ta cần phải đáp ứng, bởi vì việc làm cho mọi cặp lân cận xuất hiện theo thứ tự bắt buộc sẽ tự động sửa toàn bộ mảng hậu tố. 

Độ dài tối đa là 5000 cho một trường hợp thử nghiệm và tổng của tất cả các độ dài tối đa là`4.1 * 10^4`. Điều này loại trừ việc thử mọi chuỗi có thể hoặc so sánh nhiều lần nhiều chuỗi ứng cử viên. Một giải pháp xoay quanh thời gian bậc hai cho mỗi trường hợp thử nghiệm có thể được chấp nhận, nhưng cách tiếp cận bậc ba thì không. Bảng chữ cái được cố định thành 26 chữ cái viết thường, cho phép chúng ta sử dụng phép gán ký tự tham lam thay vì tìm kiếm tốn kém. 

Có một số bẫy mà việc triển khai trực tiếp có thể bỏ qua. Nếu giá trị độ cao bằng 0 thì hai hậu tố phải bắt đầu bằng các ký tự khác nhau chứ không phải không thể hợp nhất được. Ví dụ: nếu thứ tự hậu tố là`[1,2]`cho chuỗi`"ab"`, chiều cao đã biết là`0`và câu trả lời phải là`"ab"`, không`"aa"`. 

Nếu một hậu tố được chứa hoàn toàn dưới dạng tiền tố của một hậu tố khác thì giá trị LCP không tạo ra sự bất bình đẳng về ký tự sau khi nó kết thúc. Ví dụ, chuỗi`"aa"`có hậu tố`"a"`Và`"aa"`. LCP của họ là`1`, nhưng không có so sánh ký tự thứ hai để thực thi. 

Một lỗi phổ biến khác là gán các chữ cái theo thứ tự mảng hậu tố. Chuỗi đầu ra được sắp xếp theo vị trí ban đầu, do đó, sự xuất hiện sớm nhất của lớp tương đương trong chuỗi gốc sẽ xác định lựa chọn tham lam. Mảng hậu tố chỉ được sử dụng để tạo các ràng buộc. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực có thể cố gắng xây dựng các chuỗi theo thứ tự từ điển và tính toán các mảng hậu tố của chúng cho đến khi một chuỗi khớp với đầu vào. Điều này đúng vì chuỗi khớp đầu tiên sẽ là câu trả lời nhỏ nhất, nhưng không gian tìm kiếm thì`26^n`, điều này là không thể ngay cả đối với các chuỗi rất nhỏ. 

Một nỗ lực hợp lý hơn là khôi phục lại mọi quan hệ bình đẳng trước tiên. Mọi giá trị LCP đã biết đều nói rằng hai phạm vi của chuỗi gốc bằng nhau. Sau khi áp dụng các đẳng thức đó, mọi vị trí đều thuộc về một lớp tương đương nào đó. Nhiệm vụ còn lại là gán một ký tự cho mỗi lớp. 

Điều quan trọng cần lưu ý là những hạn chế còn lại chỉ là sự so sánh giữa các lớp. Nếu hai lớp có mối quan hệ`A < B`, ký tự được gán của`A`phải nhỏ hơn ký tự được chỉ định của`B`. 

Bây giờ xử lý chuỗi gốc từ trái sang phải. Khi một lớp xuất hiện lần đầu tiên, đặc tính của nó phải được chọn. Các lớp xuất hiện trước đó đã được ấn định sẵn, vì vậy chúng ta chỉ cần tôn trọng giới hạn dưới và giới hạn trên được tạo bởi các lớp lân cận đã được chỉ định. Chọn ký tự nhỏ nhất có thể luôn là tối ưu. Bất kỳ lớp nào trong tương lai tùy thuộc vào lớp này đều có thể thích ứng bằng cách chọn một ký tự lớn hơn sau này. 

Các ràng buộc đẳng thức được xử lý bằng cấu trúc tập hợp rời rạc. Các ràng buộc so sánh tạo thành một biểu đồ tuần hoàn có hướng vì chúng đến từ một mảng hậu tố hợp lệ, do đó việc gán ký tự tham lam không thể bị kẹt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(26^n)`|`O(n)`| Quá chậm | 
| DSU bình đẳng + sự phân công tham lam |`O(n^2)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một cấu trúc kết hợp tập hợp rời rạc trên`n`các vị trí. Đối với mọi giá trị chiều cao đã biết giữa hai hậu tố lân cận, hãy hợp nhất các ký tự tương ứng bên trong tiền tố chung. Nếu chiều cao là`h`, vị trí`sa[i-1] + k`Và`sa[i] + k`đều bình đẳng với mọi`0 <= k < h`. 

DSU nhóm chính xác những vị trí phải chứa cùng một ký tự. 

1. Nén mọi vị trí vào đại diện DSU của nó. Các đại diện trở thành các biến cần ký tự được gán. 
2. Đối với mọi giá trị độ cao đã biết, hãy kiểm tra các ký tự đầu tiên sau tiền tố chung. Nếu cả hai hậu tố vẫn còn ký tự, hãy thêm một cạnh có hướng từ lớp ký tự hậu tố nhỏ hơn vào lớp ký tự hậu tố lớn hơn. 

Điều này tạo ra các ràng buộc của hình thức`classA < classB`. 

1. Quét vị trí từ trái sang phải. Khi một lớp được nhìn thấy lần đầu tiên, hãy tính chữ cái nhỏ nhất lớn hơn mọi lớp trước đã được gán và nhỏ hơn mọi lớp kế tiếp đã được gán. 

Lần xuất hiện đầu tiên của một lớp rất quan trọng vì đó là nơi sớm nhất mà việc thay đổi chữ cái của nó sẽ ảnh hưởng đến chuỗi cuối cùng. 

1. Thay thế mọi vị trí bằng ký tự của lớp của nó và xuất ra chuỗi kết quả. 

Tại sao nó hoạt động: 

Bước DSU không bao giờ hợp nhất các vị trí được phép khác nhau. Mọi sự hợp nhất đều xuất phát từ một tiền tố chung bắt buộc. Bước biểu đồ ghi lại mọi so sánh ký tự bắt buộc giữa các hậu tố lân cận. Vì các hậu tố lân cận xác định thứ tự mảng hậu tố hoàn chỉnh nên việc đáp ứng tất cả các so sánh này sẽ mang lại chính xác thứ tự được yêu cầu. 

Trong quá trình gán tham lam, khi một lớp nhận được một lá thư, tất cả các ràng buộc liên quan đến các lớp đã được gán đều sẽ được kiểm tra. Các ràng buộc liên quan đến các lớp trong tương lai luôn có thể được thỏa mãn bằng cách gán chúng sau vì thuật toán chọn chữ cái nhỏ nhất có sẵn, chừa càng nhiều khoảng trống càng tốt phía trên nó. Do đó chuỗi được tạo ra là chuỗi nhỏ nhất có thể. 

## Giải pháp Python 

Việc triển khai đầy đủ và các phần còn lại sẽ tiếp tục trong thông báo tiếp theo.
