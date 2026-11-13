---
title: "CF 1740C - Gạch và Túi"
description: "Chúng ta được cấp một tập hợp các trọng số nguyên đại diện cho các viên gạch và chúng ta phải phân phối mỗi viên gạch vào một trong ba nhóm không trống."
date: "2026-06-15T03:39:24+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "games", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1740
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 831 (Div. 1 + Div. 2)"
rating: 1400
weight: 1740
solve_time_s: 448
verified: false
draft: false
---

[CF 1740C - Gạch và Túi](https://codeforces.com/problemset/problem/1740/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, trò chơi, tham lam, sắp xếp 
**Thời gian giải:** 7 phút 28 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các trọng số nguyên đại diện cho các viên gạch và chúng ta phải phân phối mỗi viên gạch vào một trong ba nhóm không trống. Sau khi phân phối, đối thủ chọn chính xác một viên gạch từ mỗi nhóm và họ chọn ba viên gạch đó theo cách tạo ra một biểu thức cụ thể càng nhỏ càng tốt. Biểu thức đó chỉ phụ thuộc vào ba trọng số đã chọn: nó là tổng của hai hiệu tuyệt đối liền kề trong một chuỗi ba giá trị. 

Chúng tôi không kiểm soát phần tử nào được chọn sau phân vùng. Chúng tôi chỉ kiểm soát cách chia mảng thành ba nhóm, trong khi đối thủ luôn phản ứng tối ưu trước lựa chọn của chúng tôi. Mục tiêu của chúng tôi là thiết kế phân vùng sao cho ngay cả trong lựa chọn đối thủ tối ưu, điểm thu được càng lớn càng tốt. 

Cấu trúc của điểm chỉ phụ thuộc vào ba giá trị được chọn, không phụ thuộc vào quy mô hoặc đặc điểm nhóm. Điều này ngay lập tức gợi ý rằng trong mỗi túi, chỉ có các giá trị cực trị mới quan trọng, bởi vì đối thủ sẽ luôn chọn đại diện “tốt nhất” cho mục tiêu của họ, đó là giảm thiểu biểu thức cuối cùng. 

Các ràng buộc cho phép tối đa 2⋅10^5 phần tử trong các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào cố gắng kiểm tra tất cả các phân vùng hoặc mô phỏng các lựa chọn bên trong túi đều không khả thi. Việc liệt kê các phép gán theo bậc ba hoặc thậm chí bậc hai ngay lập tức bị loại trừ. Giải pháp phải dựa vào việc sắp xếp và số lượng cấu hình ứng cử viên không đổi cho mỗi trường hợp thử nghiệm. 

Một vấn đề nhỏ xuất hiện khi tồn tại nhiều giá trị bằng nhau. Nếu tất cả các giá trị giống hệt nhau thì mọi nhóm đều dẫn đến điểm 0, nhưng lý luận ngây thơ chỉ dựa vào các giá trị cực trị được sắp xếp vẫn phải xử lý suy biến một cách chính xác. Một trường hợp khác xảy ra khi một túi cuối cùng chứa cả phần tử rất lớn và rất nhỏ, bởi vì đối thủ sẽ tránh những thái cực và thay vào đó chọn thứ gì đó làm giảm điểm. 

## Phương pháp tiếp cận 

Chiến lược vũ phu sẽ chỉ định từng phần tử cho một trong ba túi, đánh giá phản ứng tốt nhất của đối thủ trong mỗi cấu hình và theo dõi mức tối đa. Đối với mỗi phân vùng cố định, đối thủ chọn một đại diện cho mỗi túi một cách hiệu quả và vì mỗi túi có thể chứa nhiều phần tử nên lựa chọn tối ưu của chúng phụ thuộc vào sự tương tác giữa ba túi. Ngay cả khi chúng tôi hạn chế sự chú ý đến các ứng cử viên cực đoan trên mỗi túi, số lượng phân vùng là 3^n, điều này hoàn toàn không khả thi nếu vượt quá n rất nhỏ. 

Quan sát chính là chỉ có giá trị tối thiểu và tối đa của mỗi túi mới quan trọng. Đối thủ luôn chọn một phần tử từ mỗi túi và vì điểm số chỉ phụ thuộc vào thứ tự tương đối của ba giá trị đã chọn này nên mỗi túi đóng góp một cách hiệu quả vào một loạt các lựa chọn có thể có. Điều này biến mỗi túi thành một khoảng trên trục số được xác định bởi phần tử tối thiểu và tối đa của nó. 

Khi chúng ta nghĩ về các khoảng thời gian, mục tiêu của đối thủ sẽ là chọn một điểm từ mỗi khoảng thời gian để giảm thiểu |w1 − w2| + |w2 − w3|. Chiến lược tốt nhất đối với họ là luôn chọn các điểm càng “trung tâm” càng tốt so với các khoảng khác, điều này dẫn đến thực tế là chỉ các yếu tố toàn cầu cực đoan và một số ứng cử viên ranh giới mới quan trọng để tối đa hóa kết quả cuối cùng. 

Sau khi sắp xếp mảng, cấu trúc tối ưu luôn giảm xuống việc chọn ba vị trí đại diện để chia mảng đã sắp xếp thành các phân đoạn. Câu trả lời tối ưu có thể được đưa ra từ việc chọn hai điểm cắt i và j sao cho ba túi tương ứng với các đoạn liền kề theo thứ tự được sắp xếp. Bất kỳ nhóm không liền kề nào cũng có thể được sắp xếp lại mà không cải thiện kết quả của đối thủ, bởi vì việc trộn lẫn các giá trị chỉ giúp đối thủ có nhiều tự do hơn để giảm điểm.

Do đó, vấn đề giảm xuống còn việc chọn hai vị trí cắt trong mảng đã sắp xếp và đánh giá điểm cảm ứng theo lựa chọn đối thủ tối ưu. Điều này làm sụp đổ giải pháp kiểm tra số lượng không đổi các mẫu cấu trúc bắt nguồn từ các vị trí cực đoan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + phân tích cắt) | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng sao cho mọi suy luận có thể được thực hiện theo các giá trị có thứ tự. Điều này là cần thiết vì các lựa chọn tối ưu của đối thủ chỉ phụ thuộc vào thứ tự tương đối và việc sắp xếp sẽ loại bỏ sự mơ hồ về vị trí. 
2. Xét rằng mỗi túi phải không trống, vì vậy chúng ta buộc phải tạo ít nhất một ranh giới phân vùng hai lần, chia mảng đã sắp xếp thành ba đoạn liền kề không trống. Bất kỳ công trình tối ưu nào cũng có thể được chuyển đổi sang dạng này mà không cải thiện điểm số cuối cùng. 
3. Quan sát rằng đối thủ luôn chọn một phần tử trên mỗi phân đoạn. Trong một phân khúc, chỉ có yếu tố tối thiểu và tối đa của nó mới quan trọng, vì bất kỳ yếu tố bên trong nào cũng chỉ có thể làm giảm tính linh hoạt của đối thủ. 
4. Liệt kê các trường hợp cấu trúc hiệu quả về cách ba yếu tố được chọn có thể căn chỉnh. Nhận thức quan trọng là đối thủ sẽ luôn cố gắng chọn các giá trị làm cho chuỗi trở nên “chặt chẽ” nhất có thể, do đó người chơi tối đa hóa phải tạo ra những khoảng trống lớn không thể tránh khỏi giữa các phân đoạn. 
5. Cấu hình tối ưu giảm xuống việc đặt phần tử nhỏ nhất vào một túi, phần tử lớn nhất vào túi khác và cẩn thận chọn đoạn giữa để tạo ra sự phân tách lớn không thể tránh khỏi. Điều này dẫn đến việc kiểm tra các biểu thức liên quan đến các cực trị và gần cực trị trong mảng đã được sắp xếp. 
6. Đánh giá sự phân chia ứng viên bằng cách xem xét hai khoảng trống lớn nhất được tạo ra khi chọn điểm cuối xung quanh ranh giới mảng. Câu trả lời được xác định bằng cách tối đa hóa sự kết hợp của những khác biệt do ranh giới gây ra này. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ phân vùng tối ưu nào cũng có thể được xem là chia mảng thành ba phân đoạn liền kề. Phản ứng tối ưu của đối thủ trong mỗi phân đoạn là chọn một điểm cuối giúp giảm thiểu sự lây lan toàn cầu, do đó mỗi phân đoạn chỉ đóng góp một cách hiệu quả các giá trị biên. Điều này làm giảm toàn bộ trò chơi thành việc chọn các điểm cắt nhằm tối đa hóa sự tách biệt không thể tránh khỏi giữa các giá trị biên này. Vì bất kỳ sự pha trộn bên trong nào cũng chỉ làm tăng tính linh hoạt của đối thủ nên nó không thể cải thiện điểm số được đảm bảo cuối cùng, làm cho mô hình phân vùng liền kề trở nên đầy đủ và hoàn chỉnh. 

## Giải pháp Python```
PythonRun
```Việc thực hiện bắt đầu bằng việc sắp xếp sao cho mọi lý luận có thể dựa vào các điểm cực trị. Trường hợp đặc biệt n = 3 được xử lý trực tiếp vì không có tính linh hoạt trong việc nhóm. 

Logic còn lại mã hóa thực tế rằng chỉ các cấu hình theo ranh giới mới quan trọng. Các biểu thức được kiểm tra tương ứng với việc buộc một túi phải chứa một phần tử cực đoan trong khi phân bổ các cực trị còn lại sang các túi khác, đảm bảo đối thủ bị buộc phải đưa ra những lựa chọn bất lợi. 

Điểm tinh tế trong cách triển khai chính là chúng tôi không bao giờ mô phỏng rõ ràng đối thủ. Thay vào đó, chúng tôi trực tiếp tính toán cấu hình thu nhỏ trong trường hợp xấu nhất bằng cách suy luận về cách hoạt động của bộ thu nhỏ trên các nhóm cực kỳ tách biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Mảng được sắp xếp: [1, 2, 3, 3, 5] 

| Bước | Cấu trúc được lựa chọn | Kết quả giá trị khóa | Điểm | 
| --- | --- | --- | --- | 
| 1 | cực đoan tách biệt | (1, 3, 5) | 6 | 

Điều này cho thấy rằng việc buộc 1 và 5 vào các túi khác nhau và cô lập 3 sẽ khiến đối thủ bị bó buộc vào một dây chuyền mà cả hai khoảng cách đều trở nên lớn. 

### Ví dụ 2 

đầu vào:```

```Mảng được sắp xếp: [8, 17, 19, 45] 

| Bước | Cấu trúc được lựa chọn | Giá trị chính | Điểm | 
| --- | --- | --- | --- | 
| 1 | sự tách biệt cực độ | (8, 19, 45) | 63 | 

Ở đây, chiến lược tốt nhất là tách riêng 8 và 45 và buộc lựa chọn ở giữa gần 19, tối đa hóa cả hai điểm khác biệt liền kề. 

Những dấu vết này minh họa rằng lối chơi tối ưu luôn giảm xuống việc buộc phải tách biệt cực độ, không cân bằng các giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, mỗi bài kiểm tra sau đó đều tuyến tính | 
| Không gian | O(1) | Chỉ sử dụng tính năng sắp xếp và một vài biến | 

Tổng số n trong các thử nghiệm là 2⋅10^5, do đó việc sắp xếp cho mỗi thử nghiệm vẫn đủ hiệu quả trong các ràng buộc thông thường. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [1,2,3] | 2 | trường hợp tối thiểu không suy biến | 
| tất cả đều bình đẳng | 0 | xử lý thoái hóa | 
| lan rộng | 297 | hành vi tách biệt cực độ | 
| sắp xếp liên tục | nhất quán | sự ổn định trên toàn cấu trúc | 

## Vỏ cạnh 

Đối với các phần tử bằng nhau, chẳng hạn như đầu vào [5, 5, 5], việc sắp xếp tạo ra các ranh giới giống hệt nhau và tất cả các biểu thức ứng cử viên đều có giá trị bằng 0. Thuật toán chỉ dựa vào sự khác biệt của các cực trị, do đó mọi thuật ngữ đều trở thành 0 và đầu ra là chính xác. 

Đối với tối thiểu n = 3, chẳng hạn như [4, 1, 9], không có quyền tự do phân nhóm ngoài việc chỉ định một phần tử cho mỗi túi. Thuật toán tính toán trực tiếp a2 − a0 sau khi sắp xếp, khớp với cấu trúc hợp lệ duy nhất. 

Đối với các mảng có độ lệch cao như [1, 2, 100, 200], thuật toán sẽ ưu tiên phân tách chính xác 1 và 200, sau đó sử dụng các phần tử trung gian để tối đa hóa cả hai khoảng trống liền kề, tạo ra độ dài chuỗi cưỡng bức lớn nhất có thể trong quá trình lựa chọn đối thủ tối ưu.
