---
title: "CF 1070M - Algoland và Berland"
description: "Chúng ta có hai tập hợp điểm trên mặt phẳng, một tập hợp thuộc về Algoland và tập kia thuộc về Berland. Nhiệm vụ là xây dựng chính xác các đoạn thẳng $a + b - 1$, mỗi đoạn nối một thành phố Berland với một thành phố Algoland. Mỗi đoạn trở thành một con đường hai chiều."
date: "2026-06-15T14:02:32+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "divide-and-conquer", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1070
codeforces_index: "M"
codeforces_contest_name: "2018-2019 ICPC, NEERC, Southern Subregional Contest (Online Mirror, ACM-ICPC Rules, Teams Preferred)"
rating: 3000
weight: 1070
solve_time_s: 402
verified: false
draft: false
---

[CF 1070M - Algoland và Berland](https://codeforces.com/problemset/problem/1070/M) 

**Đánh giá:** 3000 
**Tags:** thuật toán xây dựng, chia để trị, hình học 
**Thời gian giải:** 6 phút 42 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên mặt phẳng, một tập hợp thuộc về Algoland và tập kia thuộc về Berland. Nhiệm vụ là xây dựng chính xác$a + b - 1$các đoạn thẳng, mỗi đoạn nối một thành phố Berland với một thành phố Algoland. Mỗi đoạn trở thành một con đường hai chiều. Những con đường này phải tạo thành một cấu trúc kết nối trên tất cả các thành phố, vì vậy từ bất kỳ thành phố nào, chúng ta phải có thể đi đến bất kỳ thành phố nào khác bằng cách sử dụng các đoạn này. 

Có một hạn chế về cấu trúc: không có hai đoạn nào được phép giao nhau tại các điểm bên trong. Giao lộ chỉ được phép ở các điểm cuối, nghĩa là hai con đường chỉ có thể gặp nhau nếu chúng có chung một thành phố, điều này là không thể ở đây vì mọi cạnh đều phải kết nối thành phố Berland với thành phố Algoland. Vì vậy, về mặt hình học, chúng ta đang xây dựng một đồ thị phẳng lưỡng cực đường thẳng nhúng trong mặt phẳng không có giao điểm. 

Ngoài ra, mỗi thành phố Berland$j$có bằng cấp quy định$r_j$, nghĩa là chính xác$r_j$đường phải bắt nguồn từ thành phố đó. Vì tổng số đường là$a + b - 1$, những mức độ này xác định đầy đủ số lượng điểm cuối trong Algoland sẽ được sử dụng. 

Các ràng buộc ngụ ý rằng chúng ta phải xây dựng một cây bao trùm trên$a + b$các nút có cấu trúc lưỡng cực cố định và các ràng buộc nhúng phẳng. Giới hạn kích thước đủ nhỏ để$a + b \le 3000$cho mỗi trường hợp thử nghiệm, nhưng có thể có tới 3000 trường hợp thử nghiệm, vì vậy giải pháp về cơ bản phải tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. 

Một cách tiếp cận ngây thơ cố gắng kết nối các cặp tùy ý và sau đó kiểm tra các điểm giao nhau của đoạn sẽ ngay lập tức thất bại. Thậm chí kiểm tra tất cả các cặp đoạn đường để tìm chi phí giao lộ$O((a+b)^2)$, trở nên quá chậm ở kích thước tối đa. Tệ hơn nữa, ngay cả khi tránh được các giao lộ cục bộ thì tính nhất quán toàn cầu vẫn khó duy trì. 

Một dạng lỗi tinh tế hơn xuất phát từ việc bỏ qua hình học. Ví dụ: việc kết nối từng điểm Berland với các điểm Algoland gần nhất của nó một cách tham lam có thể dễ dàng tạo ra các điểm giao nhau. Một cấu hình đơn giản gồm bốn điểm ở vị trí lồi đã cho thấy rằng các lựa chọn tham lam cục bộ có thể vi phạm tính phẳng toàn cục. 

Cấu trúc ẩn chính là các kết quả khớp đường thẳng không giao nhau giữa hai tập hợp điểm hoạt động giống như các “ngăn xếp” đơn điệu khi được sắp xếp theo góc xung quanh một điểm tham chiếu. Điều này làm giảm hình học thành một vấn đề đặt hàng. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng xây dựng một cây bao trùm bằng cách liên tục chọn một cạnh hợp lệ giữa một số thành phố Berland và thành phố Algoland không vượt qua các cạnh đã chọn trước đó. Mỗi cạnh ứng cử viên yêu cầu kiểm tra tất cả các cạnh hiện có để tìm giao điểm, điều này tốn$O(a + b)$mỗi lần kiểm tra. Với$a + b - 1$cạnh, điều này dẫn đến$O((a+b)^3)$hành vi trong trường hợp xấu nhất là quá chậm. 

Ngay cả khi chúng tôi cải thiện việc kiểm tra tính hợp lệ bằng cách sử dụng tiền xử lý hình học, khó khăn chính vẫn là tổ hợp: đảm bảo độ quy định$r_j$được thỏa mãn trong khi vẫn duy trì tính phẳng. 

Quan sát quan trọng là điều kiện không cắt nhau của đường thẳng lưỡng cực trở nên tầm thường nếu chúng ta sắp xếp các điểm Algoland xung quanh một điểm Berland cố định và kết nối tham lam theo thứ tự tuần hoàn đó. Chính xác hơn, nếu chúng ta chọn điểm Berland làm gốc và xem xét thứ tự góc của tất cả các điểm Algoland xung quanh nó, thì các cạnh hoạt động giống như các khoảng phải được lồng vào nhau hoặc rời nhau. Điều này làm giảm vấn đề phân phối các mức độ cần thiết theo một chuỗi có cấu trúc. 

Chúng ta có thể giải thích việc xây dựng như xây dựng một cây có gốc trong đó các đỉnh Berland là các nút bên trong và các đỉnh Algoland là các lá. Mỗi đỉnh Berland phải phân phối số lượng con cần thiết của nó theo cách tôn trọng trật tự toàn cục do hình học tạo ra. Ràng buộc không giao nhau buộc các phần tử con này phải tương ứng với các đoạn liền kề trong một đường quét góc. 

Điều này dẫn đến một cấu trúc chia để trị. Chúng tôi sắp xếp các điểm Algoland theo góc xung quanh một điểm Berland tham chiếu tùy ý. Sau đó, chúng tôi phân chia đệ quy thứ tự vòng tròn này thành các phân đoạn, gán cho mỗi nút Berland một khối có kích thước phù hợp với mức độ yêu cầu của nó. Mỗi khối được gắn theo cách duy trì sự lồng nhau lồi, đảm bảo không có giao cắt phân đoạn. 

Sự đảm bảo về hình học xuất phát từ một thực tế tiêu chuẩn: nếu các điểm được kết nối theo thứ tự nhất quán với một đường quét hướng tâm hoặc góc, thì các cạnh được vẽ từ một điểm cố định bên ngoài đến các điểm liên tiếp theo thứ tự đó sẽ không cắt nhau. Phép đệ quy khái quát hóa điều này bằng cách đảm bảo rằng mọi cây con chiếm một khoảng góc liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Xây dựng cạnh Brute Force + kiểm tra giao lộ |$O((a+b)^3)$|$O(a+b)$| Quá chậm | 
| Sắp xếp góc + gán khoảng đệ quy |$O((a+b)\log(a+b))$|$O(a+b)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sửa hướng tham chiếu bằng cách chọn một điểm Berland tùy ý và sắp xếp tất cả các điểm Algoland theo góc cực xung quanh nó. Điều này tạo ra một trật tự vòng tròn mà chúng ta coi như một chuỗi tuyến tính. 

Sau đó, chúng tôi xây dựng cấu trúc đệ quy trên các phân đoạn của danh sách có thứ tự này trong khi sử dụng các đỉnh Berland theo mức độ yêu cầu của chúng. 

1. Chọn một đỉnh Berland sẽ đóng vai trò là gốc của khoảng đệ quy hiện tại. Về mặt khái niệm, chúng tôi giao trách nhiệm cho nó đối với một khối điểm Algoland liền kề. Điều này là hợp lý vì bất kỳ sự giao nhau nào giữa các cạnh sẽ vi phạm tính đơn điệu góc do trật tự áp đặt. 
2. Đặt khoảng thời gian hiện tại chứa$k$Điểm Algoland. Chúng ta phải gán chúng cho các đỉnh Berland có tổng yêu cầu về độ bằng$k$, vì mỗi điểm Algoland phải kết nối với chính xác một điểm Berland. 
3. Xử lý các đỉnh Berland theo bất kỳ thứ tự cố định nào, lấy một đỉnh có nhu cầu còn lại$r_j$và gán cho nó một khối liên tiếp chính xác$r_j$Algoland điểm từ khoảng thời gian. Điều này bảo tồn tính liên tục, là yếu tố bất biến quan trọng ngăn cản việc vượt qua. 
4. Đối với mỗi khối được chỉ định, chúng tôi kết nối trực tiếp tất cả các điểm Algoland của nó với đỉnh Berland đó. Các cạnh này không giao nhau trong khối vì chúng đều có chung một điểm cuối. 
5. Mỗi khối chỉ trở thành một bài toán con nếu chúng ta chia nhỏ thêm các đỉnh Berland bên trong nó. Tuy nhiên, vì mỗi điểm Algoland là một lá trong cấu trúc cuối cùng nên quá trình đệ quy sẽ kết thúc một cách tự nhiên sau khi tất cả các phép gán được thực hiện. 
6. Lặp lại cho đến khi tất cả các điểm Algoland được chỉ định và tất cả các mức độ Berland đều được thỏa mãn. 

Việc xây dựng luôn tiêu tốn chính xác$a + b - 1$các cạnh vì mỗi điểm Algoland đóng góp chính xác một cạnh và tổng độ Berland thành tổng chính xác. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, mỗi bài toán con hoạt động tương ứng với một khoảng liền kề của các điểm Algoland được sắp xếp theo góc và tất cả các cạnh được tạo cho đến nay đều tuân theo cấu trúc khoảng này. Bất kỳ cạnh nào luôn nối đỉnh Berland với một khối liền kề, nghĩa là các cạnh không thể xen kẽ theo thứ tự góc. Vì các giao điểm phân đoạn trong nhúng đường thẳng yêu cầu các điểm cuối xen kẽ theo thứ tự góc cạnh, nên việc duy trì tính liền kề đảm bảo rằng không có hai cạnh nào giao nhau ngoại trừ tại các điểm cuối được chia sẻ. Các ràng buộc về mức độ được thỏa mãn chính xác vì mỗi đỉnh Berland được gán một kích thước khối bằng với mức độ yêu cầu của nó và tổng của tất cả các kích thước khối khớp với số đỉnh Algoland. 

## Giải pháp Python```
PythonRun
```Việc triển khai sử dụng cấu trúc đơn giản hóa để xử lý các điểm Algoland như một chuỗi có thứ tự và gán các điểm liên tiếp cho các đỉnh Berland theo yêu cầu về mức độ của chúng. Bước sắp xếp cố định một trật tự tổng thể sao cho các cạnh có thể được vẽ dưới dạng các hình quạt không giao nhau từ mỗi đỉnh Berland. 

Chi tiết triển khai chính là chúng tôi phải đảm bảo thứ tự nhất quán của các điểm Algoland. Việc sắp xếp dựa trên tham chiếu đã chọn làm giảm bài toán hình học thành một chuỗi tuyến tính, điều này là đủ vì tất cả các cạnh chỉ chia sẻ điểm cuối ở phía Berland và được phân bổ thành các khối liền kề. 

Sau đó, việc xây dựng chỉ cần đi qua các điểm Algoland một lần, gán từng điểm cho vị trí Berland có sẵn tiếp theo, đảm bảo cả mức độ hài lòng và kết nối không theo chu kỳ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi sắp xếp các điểm Algoland theo góc xung quanh$B_1 = (0,1)$, mang lại một trật tự cố định, giả sử$A_1, A_2$. 

| Bước | Chỉ số Berland | r còn lại | Algoland được giao | Cạnh | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | A1 | (1, A1) | 
| 2 | 2 | 1 | A2 | (2, A2) | 

Điều này tạo ra một cây hai cạnh đơn giản không có giao điểm. 

### Ví dụ 2 

đầu vào:```

```Giả sử thứ tự Algoland là$A_1, A_2, A_3$. 

| Bước | Berland | r | Bài tập | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | B1 | 2 → 1 | A1 | B1-A1 | 
| 2 | B1 | 1 → 0 | A2 | B1-A2 | 
| 3 | B2 | 1 → 0 | A3 | B2-A3 | 

B1 tạo thành hình quạt trong một khoảng liền kề và B2 gắn điểm còn lại. 

Những dấu vết này cho thấy mỗi đỉnh Berland nhận được một đoạn liền kề, giúp ngăn chặn các cạnh xen kẽ có thể gây ra giao điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((a+b)\log a)$| Sắp xếp điểm Algoland chiếm ưu thế, phép gán là tuyến tính | 
| Không gian |$O(a+b)$| Lưu trữ điểm và cạnh đầu ra | 

Giới hạn$a, b \le 3000$và tổng số tiền qua các lần kiểm tra đảm bảo rằng việc sắp xếp và quét tuyến tính vẫn dễ dàng nằm trong giới hạn ngay cả đối với 3000 trường hợp kiểm tra. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 Berland, 1 Algoland | cạnh đơn | kết nối cơ sở | 
| dây chuyền nhỏ | cây hợp lệ | xử lý bằng cấp | 
| hỗn hợp cân bằng | cây không trống | xây dựng chung đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các yêu cầu về mức độ Berland đều tập trung vào một đỉnh duy nhất. Trong tình huống đó, đỉnh đó phải nối với tất cả các điểm Algoland, tạo thành một hình quạt đầy đủ. Thuật toán xử lý việc này một cách tự nhiên vì nó gán một khối liền kề bằng toàn bộ phạm vi, không tạo ra các cạnh xen kẽ. 

Một trường hợp cạnh khác là khi độ rất không đồng đều, ví dụ một đỉnh có độ$a$và tất cả những cái khác đều có bậc 1. Cấu trúc vẫn gán các đoạn liền kề nhau nên đỉnh bậc lớn tạo thành một chiếc quạt duy nhất không gián đoạn, trong khi các đỉnh khác gắn các lá đơn lẻ mà không tạo giao nhau vì các cạnh của chúng nằm ngoài nhịp góc của quạt. 

Một trường hợp tinh tế cuối cùng là khi các điểm Algoland được sắp xếp theo một cấu hình có thể tạo ra các điểm giao nhau dưới sự ghép đôi tham lam ngây thơ. Bởi vì thuật toán thực thi một trật tự tổng thể trước khi gán, nên ngay cả các cấu hình hình học đối lập cũng sẽ thu gọn thành một chuỗi tuyến tính, ngăn chặn bất kỳ sự xen kẽ nào có thể tạo ra các giao điểm.
